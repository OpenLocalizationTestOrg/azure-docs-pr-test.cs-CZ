---
title: "Kurz: Šifrování a dešifrování objektů BLOB v úložišti Azure pomocí Azure Key Vault | Microsoft Docs"
description: "Jak tooencrypt a dešifrování objektu blob pomocí šifrování na straně klienta pro úložiště Microsoft Azure s Azure Key Vault."
services: storage
documentationcenter: 
author: adhurwit
manager: jasonsav
editor: tysonn
ms.assetid: 027e8631-c1bf-48c1-9d9b-f6843e88b583
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 01/23/2017
ms.author: adhurwit
ms.openlocfilehash: 3eb64f104f378dd09ef295c94e03167655883391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Kurz: Šifrování a dešifrování objektů BLOB v úložišti Microsoft Azure pomocí Azure Key Vault
## <a name="introduction"></a>Úvod
Tento kurz popisuje, jak používat toomake šifrování na straně klienta úložiště s Azure Key Vault. Provede vás provedou postupem tooencrypt a dešifrování objektů blob v konzolové aplikaci použití těchto technologií.

**Odhadovaný čas toocomplete:** 20 minut

Souhrnné informace o Azure Key Vault naleznete v tématu [co je Azure Key Vault?](../key-vault/key-vault-whatis.md).

Další informace o šifrování na straně klienta pro Azure Storage najdete v části [šifrování na straně klienta a Azure Key Vault pro Microsoft Azure Storage](storage-client-side-encryption.md).

## <a name="prerequisites"></a>Požadavky
toocomplete tento kurz, musíte mít hello následující:

* Účet úložiště Azure
* Visual Studio 2013 nebo novější
* Azure PowerShell

## <a name="overview-of-client-side-encryption"></a>Přehled šifrování na straně klienta
Přehled šifrování na straně klienta pro Azure Storage najdete v tématu [šifrování na straně klienta a Azure Key Vault pro úložiště Microsoft Azure](storage-client-side-encryption.md)

Zde je stručný popis toho, jak funguje šifrování na straně klienta:

1. Klient Azure Storage Hello SDK vytvoří obsahu šifrovací klíč (CEK), což je použití jednoho bránu symetrického klíče.
2. Zákaznická data zašifrována pomocí této CEK.
3. Hello CEK je vnořena (šifrované) pomocí hello klíče šifrovacího klíče (KEK). Hello KEK je identifikovaná identifikátorem klíče a může být pár asymetrických klíčů nebo symetrického klíče a může být spravovaný místně nebo uložené v Azure Key Vault. Hello klienta úložiště, samotné nikdy přístup toohello KEK. Vyvolá právě hello klíče zabalení algoritmus, který zajišťuje Key Vault. Zákazníci můžete zvolit toouse vlastního zprostředkovatele pro klíč zabalení/rozbalování Pokud chtějí.
4. Hello šifrovaná data se pak nahrán toohello služby Azure Storage.

## <a name="set-up-your-azure-key-vault"></a>Nastavení Azure Key Vault
V pořadí tooproceed v tomto kurzu, budete potřebovat následující kroky, které jsou uvedeny v kurzu hello hello toodo [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md):

* Vytvoření trezoru klíčů.
* Přidáte klíč nebo tajný toohello trezoru klíčů.
* Zaregistrujte aplikaci s Azure Active Directory.
* Autorizovat hello aplikace toouse hello klíče nebo tajného klíče.

Ujistěte se, poznamenejte si hello ClientID a ClientSecret, které byly generovány při registraci aplikace na Azure Active Directory.

Vytvořte oba klíče v trezoru klíčů hello. Pro hello zbytek hello kurzu předpokládáme, že jste použili následující názvy hello: ContosoKeyVault a TestRSAKey1.

## <a name="create-a-console-application-with-packages-and-appsettings"></a>Vytvořte konzolovou aplikaci s balíčky a AppSettings
V sadě Visual Studio vytvořte novou konzolovou aplikaci.

Přidání balíčků nuget potřebné v hello Konzola správce balíčků.

```
Install-Package WindowsAzure.Storage

// This is hello latest stable release for ADAL.
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

Přidejte AppSettings toohello App.Config.

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

Přidejte následující hello `using` příkazů a ujistěte se, tooadd projektu toohello tooSystem.Configuration odkaz.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Configuration;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.Azure.KeyVault;
using System.Threading;        
using System.IO;
```

## <a name="add-a-method-tooget-a-token-tooyour-console-application"></a>Přidat metoda tooget tokenu tooyour konzolové aplikace
Hello následující metoda se používá Key Vault třídy, které je třeba tooauthenticate pro trezor klíčů tooyour přístup.

```csharp
private async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(
        ConfigurationManager.AppSettings["clientId"],
        ConfigurationManager.AppSettings["clientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed tooobtain hello JWT token");

    return result.AccessToken;
}
```

## <a name="access-storage-and-key-vault-in-your-program"></a>Přístup k úložišti a Key Vault v programu
Hello hlavní funkce přidejte následující kód hello.

```csharp
// This is standard code toointeract with Blob storage.
StorageCredentials creds = new StorageCredentials(
    ConfigurationManager.AppSettings["accountName"],
       ConfigurationManager.AppSettings["accountKey"]);
CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
CloudBlobClient client = account.CreateCloudBlobClient();
CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
contain.CreateIfNotExists();

// hello Resolver object is used toointeract with Key Vault for Azure Storage.
// This is where hello GetToken method from above is used.
KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);
```

> [!NOTE]
> Trezor klíčů objektové modely
> 
> Je důležité toounderstand, že jsou ve skutečnosti dvě Key Vault objekt modelů toobe vědět: jeden je založena na hello REST API (KeyVault obor názvů) a hello jiných je rozšířením pro šifrování na straně klienta.
> 
> Hello klíč trezoru klient komunikuje s hello REST API a porozuměl jim JSON webové klíče a tajné klíče pro hello dva druhy věcí, které jsou obsaženy v Key Vault.
> 
> Hello klíč trezoru rozšíření jsou třídy, které zdá se, že vytvořený specificky pro šifrování na straně klienta ve službě Azure Storage. Obsahují rozhraní pro třídy podle hello konceptu překladač klíč a klíče (IKey). Existují dvě implementace IKey, je nutné, aby tooknow: RSAKey a SymmetricKey. Nyní dějí toocoincide s hello věcí, které jsou obsaženy v Key Vault, ale v tomto okamžiku jsou nezávislé třídy (takže hello klíč a tajný klíč načíst hello klíč trezoru klient neimplementuje IKey).
> 
> 

## <a name="encrypt-blob-and-upload"></a>Šifrování objektů blob a nahrajte
Přidejte následující hello kód tooencrypt objekt blob a nahrajte ho tooyour účtu úložiště Azure. Hello **ResolveKeyAsync** vrátí metoda, která se používá IKey.

```csharp
// Retrieve hello key that you created previously.
// hello IKey that is returned here is an RsaKey.
// Remember that we used hello names contosokeyvault and testrsakey1.
var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();

// Now you simply use hello RSA key tooencrypt by setting it in hello BlobEncryptionPolicy.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Reference a block blob.
CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

// Upload using hello UploadFromStream method.
using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
    blob.UploadFromStream(stream, stream.Length, null, options, null);
```

Toto je snímek obrazovky hello [portálu Azure Classic](https://manage.windowsazure.com) pro objekt blob, která byla dříve zašifrována pomocí šifrování na straně klienta pomocí klíče uloženého v Key Vault. Hello **KeyId** vlastnost je hello identifikátor URI pro klíč hello v Key Vault, který slouží jako hello KEK. Hello **EncryptedKey** vlastnost obsahuje zašifrovaná verze hello CEK hello.

![Snímek obrazovky zobrazující metadata objektu Blob, který obsahuje metadata šifrování](./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png)

> [!NOTE]
> Pokud se podíváte na hello BlobEncryptionPolicy konstruktor, uvidíte, že může přijmout klíč nebo překladač. Mějte na paměti, které tuto chvíli nemůžete použít překladač pro šifrování, protože není aktuálně dělá podporují výchozí klíč.
> 
> 

## <a name="decrypt-blob-and-download"></a>Dešifrování objektů blob a stáhnout
Dešifrování je ve skutečnosti při použití hello překladač třídy smysl. ID Hello hello klíče pro šifrování je přidružený objekt blob hello ve svých metadatech, takže neexistuje žádný důvod pro jste tooretrieve hello klíč a zapamatovat si hello přidružení mezi klíč a objektů blob. Máte právě toomake se, že klíči hello zůstane v Key Vault.   

Hello privátní klíč klíč RSA zůstane v Key Vault, takže pro dešifrování toooccur hello šifrované klíče z hello metadata objektu blob, který obsahuje hello CEK je odeslána tooKey trezoru pro dešifrování.

Přidejte následující toodecrypt hello blob, který jste právě nahráli hello.

```csharp
// In this case, we will not pass a key and only pass hello resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> Je jednodušší, včetně několik jinými druhy překladače služby správy klíčů toomake: AggregateKeyResolver a CachingKeyResolver.
> 
> 

## <a name="use-key-vault-secrets"></a>Pomocí Key Vault tajné klíče
Hello způsob toouse tajný klíč pomocí šifrování na straně klienta je prostřednictvím hello SymmetricKey třídy, protože v podstatě symetrický klíč je tajný klíč. Ale, jak je uvedeno výše, tajný klíč v Key Vault nemapují přesně tooa SymmetricKey. Existují toounderstand několik věcí:

* klíč Hello v SymmetricKey má toobe pevnou délkou: 128, 192, 256, 384 nebo 512 bitů.
* Hello klíč v SymmetricKey by měla mít kódování Base64.
* Key Vault tajný klíč, který se použije jako SymmetricKey musí toohave obsahu typ "application/octet-stream" v Key Vault.

Tady je příklad v prostředí PowerShell v Key Vault, který lze použít jako SymmetricKey vytváření tajný klíč.
Upozorňujeme, že hodnota hello pevný programového, $key, je pro demonstrační účely jenom. V kódu můžete toogenerate tento klíč.

```csharp
// Here we are making a 128-bit key so we have 16 characters.
//     hello characters are in hello ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute hello VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

Ve vaší aplikaci konzoly můžete použít stejné volání jako před tooretrieve tento tajný jako SymmetricKey hello.

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
A je to. Užijte si ji!

## <a name="next-steps"></a>Další kroky
Další informace o používání služby Microsoft Azure Storage pomocí C#, najdete v části [Microsoft Azure Storage Client Library pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Další informace o hello Blob REST API najdete v tématu [rozhraní API REST služby objektů Blob](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Hello nejnovější informace o úložišti Microsoft Azure, přejděte toohello [Blog týmu Azure Storage Microsoft](http://blogs.msdn.com/b/windowsazurestorage/).
