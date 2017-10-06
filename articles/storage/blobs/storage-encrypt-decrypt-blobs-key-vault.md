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
ms.openlocfilehash: e387dd419a51b5b1df62d10ead97268e8295ff56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a><span data-ttu-id="d067c-103">Kurz: Šifrování a dešifrování objektů BLOB v úložišti Microsoft Azure pomocí Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d067c-103">Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault</span></span>
## <a name="introduction"></a><span data-ttu-id="d067c-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="d067c-104">Introduction</span></span>
<span data-ttu-id="d067c-105">Tento kurz popisuje, jak používat toomake šifrování na straně klienta úložiště s Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d067c-105">This tutorial covers how toomake use of client-side storage encryption with Azure Key Vault.</span></span> <span data-ttu-id="d067c-106">Provede vás provedou postupem tooencrypt a dešifrování objektů blob v konzolové aplikaci použití těchto technologií.</span><span class="sxs-lookup"><span data-stu-id="d067c-106">It walks you through how tooencrypt and decrypt a blob in a console application using these technologies.</span></span>

<span data-ttu-id="d067c-107">**Odhadovaný čas toocomplete:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="d067c-107">**Estimated time toocomplete:** 20 minutes</span></span>

<span data-ttu-id="d067c-108">Souhrnné informace o Azure Key Vault naleznete v tématu [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d067c-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md).</span></span>

<span data-ttu-id="d067c-109">Další informace o šifrování na straně klienta pro Azure Storage najdete v části [šifrování na straně klienta a Azure Key Vault pro Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d067c-109">For overview information about client-side encryption for Azure Storage, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d067c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d067c-110">Prerequisites</span></span>
<span data-ttu-id="d067c-111">toocomplete tento kurz, musíte mít hello následující:</span><span class="sxs-lookup"><span data-stu-id="d067c-111">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="d067c-112">Účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="d067c-112">An Azure Storage account</span></span>
* <span data-ttu-id="d067c-113">Visual Studio 2013 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d067c-113">Visual Studio 2013 or later</span></span>
* <span data-ttu-id="d067c-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d067c-114">Azure PowerShell</span></span>

## <a name="overview-of-client-side-encryption"></a><span data-ttu-id="d067c-115">Přehled šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="d067c-115">Overview of client-side encryption</span></span>
<span data-ttu-id="d067c-116">Přehled šifrování na straně klienta pro Azure Storage najdete v tématu [šifrování na straně klienta a Azure Key Vault pro úložiště Microsoft Azure](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="d067c-116">For an overview of client-side encryption for Azure Storage, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)</span></span>

<span data-ttu-id="d067c-117">Zde je stručný popis toho, jak funguje šifrování na straně klienta:</span><span class="sxs-lookup"><span data-stu-id="d067c-117">Here is a brief description of how client side encryption works:</span></span>

1. <span data-ttu-id="d067c-118">Klient Azure Storage Hello SDK vytvoří obsahu šifrovací klíč (CEK), což je použití jednoho bránu symetrického klíče.</span><span class="sxs-lookup"><span data-stu-id="d067c-118">hello Azure Storage client SDK generates a content encryption key (CEK), which is a one-time-use symmetric key.</span></span>
2. <span data-ttu-id="d067c-119">Zákaznická data zašifrována pomocí této CEK.</span><span class="sxs-lookup"><span data-stu-id="d067c-119">Customer data is encrypted using this CEK.</span></span>
3. <span data-ttu-id="d067c-120">Hello CEK je vnořena (šifrované) pomocí hello klíče šifrovacího klíče (KEK).</span><span class="sxs-lookup"><span data-stu-id="d067c-120">hello CEK is then wrapped (encrypted) using hello key encryption key (KEK).</span></span> <span data-ttu-id="d067c-121">Hello KEK je identifikovaná identifikátorem klíče a může být pár asymetrických klíčů nebo symetrického klíče a může být spravovaný místně nebo uložené v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d067c-121">hello KEK is identified by a key identifier and can be an asymmetric key pair or a symmetric key and can be managed locally or stored in Azure Key Vault.</span></span> <span data-ttu-id="d067c-122">Hello klienta úložiště, samotné nikdy přístup toohello KEK.</span><span class="sxs-lookup"><span data-stu-id="d067c-122">hello Storage client itself never has access toohello KEK.</span></span> <span data-ttu-id="d067c-123">Vyvolá právě hello klíče zabalení algoritmus, který zajišťuje Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d067c-123">It just invokes hello key wrapping algorithm that is provided by Key Vault.</span></span> <span data-ttu-id="d067c-124">Zákazníci můžete zvolit toouse vlastního zprostředkovatele pro klíč zabalení/rozbalování Pokud chtějí.</span><span class="sxs-lookup"><span data-stu-id="d067c-124">Customers can choose toouse custom providers for key wrapping/unwrapping if they want.</span></span>
4. <span data-ttu-id="d067c-125">Hello šifrovaná data se pak nahrán toohello služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d067c-125">hello encrypted data is then uploaded toohello Azure Storage service.</span></span>

## <a name="set-up-your-azure-key-vault"></a><span data-ttu-id="d067c-126">Nastavení Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d067c-126">Set up your Azure Key Vault</span></span>
<span data-ttu-id="d067c-127">V pořadí tooproceed v tomto kurzu, budete potřebovat následující kroky, které jsou uvedeny v kurzu hello hello toodo [Začínáme s Azure Key Vault](../../key-vault/key-vault-get-started.md):</span><span class="sxs-lookup"><span data-stu-id="d067c-127">In order tooproceed with this tutorial, you need toodo hello following steps, which are outlined in hello tutorial  [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md):</span></span>

* <span data-ttu-id="d067c-128">Vytvoření trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="d067c-128">Create a key vault.</span></span>
* <span data-ttu-id="d067c-129">Přidáte klíč nebo tajný toohello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="d067c-129">Add a key or secret toohello key vault.</span></span>
* <span data-ttu-id="d067c-130">Zaregistrujte aplikaci s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d067c-130">Register an application with Azure Active Directory.</span></span>
* <span data-ttu-id="d067c-131">Autorizovat hello aplikace toouse hello klíče nebo tajného klíče.</span><span class="sxs-lookup"><span data-stu-id="d067c-131">Authorize hello application toouse hello key or secret.</span></span>

<span data-ttu-id="d067c-132">Ujistěte se, poznamenejte si hello ClientID a ClientSecret, které byly generovány při registraci aplikace na Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d067c-132">Make note of hello ClientID and ClientSecret that were generated when registering an application with Azure Active Directory.</span></span>

<span data-ttu-id="d067c-133">Vytvořte oba klíče v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="d067c-133">Create both keys in hello key vault.</span></span> <span data-ttu-id="d067c-134">Pro hello zbytek hello kurzu předpokládáme, že jste použili následující názvy hello: ContosoKeyVault a TestRSAKey1.</span><span class="sxs-lookup"><span data-stu-id="d067c-134">We assume for hello rest of hello tutorial that you have used hello following names: ContosoKeyVault and TestRSAKey1.</span></span>

## <a name="create-a-console-application-with-packages-and-appsettings"></a><span data-ttu-id="d067c-135">Vytvořte konzolovou aplikaci s balíčky a AppSettings</span><span class="sxs-lookup"><span data-stu-id="d067c-135">Create a console application with packages and AppSettings</span></span>
<span data-ttu-id="d067c-136">V sadě Visual Studio vytvořte novou konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d067c-136">In Visual Studio, create a new console application.</span></span>

<span data-ttu-id="d067c-137">Přidání balíčků nuget potřebné v hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="d067c-137">Add necessary nuget packages in hello Package Manager Console.</span></span>

```
Install-Package WindowsAzure.Storage

// This is hello latest stable release for ADAL.
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

<span data-ttu-id="d067c-138">Přidejte AppSettings toohello App.Config.</span><span class="sxs-lookup"><span data-stu-id="d067c-138">Add AppSettings toohello App.Config.</span></span>

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

<span data-ttu-id="d067c-139">Přidejte následující hello `using` příkazů a ujistěte se, tooadd projektu toohello tooSystem.Configuration odkaz.</span><span class="sxs-lookup"><span data-stu-id="d067c-139">Add hello following `using` statements and make sure tooadd a reference tooSystem.Configuration toohello project.</span></span>

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

## <a name="add-a-method-tooget-a-token-tooyour-console-application"></a><span data-ttu-id="d067c-140">Přidat metoda tooget tokenu tooyour konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="d067c-140">Add a method tooget a token tooyour console application</span></span>
<span data-ttu-id="d067c-141">Hello následující metoda se používá Key Vault třídy, které je třeba tooauthenticate pro trezor klíčů tooyour přístup.</span><span class="sxs-lookup"><span data-stu-id="d067c-141">hello following method is used by Key Vault classes that need tooauthenticate for access tooyour key vault.</span></span>

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

## <a name="access-storage-and-key-vault-in-your-program"></a><span data-ttu-id="d067c-142">Přístup k úložišti a Key Vault v programu</span><span class="sxs-lookup"><span data-stu-id="d067c-142">Access Storage and Key Vault in your program</span></span>
<span data-ttu-id="d067c-143">Hello hlavní funkce přidejte následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="d067c-143">In hello Main function, add hello following code.</span></span>

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
> <span data-ttu-id="d067c-144">Trezor klíčů objektové modely</span><span class="sxs-lookup"><span data-stu-id="d067c-144">Key Vault Object Models</span></span>
> 
> <span data-ttu-id="d067c-145">Je důležité toounderstand, že jsou ve skutečnosti dvě Key Vault objekt modelů toobe vědět: jeden je založena na hello REST API (KeyVault obor názvů) a hello jiných je rozšířením pro šifrování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="d067c-145">It is important toounderstand that there are actually two Key Vault object models toobe aware of: one is based on hello REST API (KeyVault namespace) and hello other is an extension for client-side encryption.</span></span>
> 
> <span data-ttu-id="d067c-146">Hello klíč trezoru klient komunikuje s hello REST API a porozuměl jim JSON webové klíče a tajné klíče pro hello dva druhy věcí, které jsou obsaženy v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d067c-146">hello Key Vault Client interacts with hello REST API and understands JSON Web Keys and secrets for hello two kinds of things that are contained in Key Vault.</span></span>
> 
> <span data-ttu-id="d067c-147">Hello klíč trezoru rozšíření jsou třídy, které zdá se, že vytvořený specificky pro šifrování na straně klienta ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d067c-147">hello Key Vault Extensions are classes that seem specifically created for client-side encryption in Azure Storage.</span></span> <span data-ttu-id="d067c-148">Obsahují rozhraní pro třídy podle hello konceptu překladač klíč a klíče (IKey).</span><span class="sxs-lookup"><span data-stu-id="d067c-148">They contain an interface for keys (IKey) and classes based on hello concept of a Key Resolver.</span></span> <span data-ttu-id="d067c-149">Existují dvě implementace IKey, je nutné, aby tooknow: RSAKey a SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="d067c-149">There are two implementations of IKey that you need tooknow: RSAKey and SymmetricKey.</span></span> <span data-ttu-id="d067c-150">Nyní dějí toocoincide s hello věcí, které jsou obsaženy v Key Vault, ale v tomto okamžiku jsou nezávislé třídy (takže hello klíč a tajný klíč načíst hello klíč trezoru klient neimplementuje IKey).</span><span class="sxs-lookup"><span data-stu-id="d067c-150">Now they happen toocoincide with hello things that are contained in a Key Vault, but at this point they are independent classes (so hello Key and Secret retrieved by hello Key Vault Client do not implement IKey).</span></span>
> 
> 

## <a name="encrypt-blob-and-upload"></a><span data-ttu-id="d067c-151">Šifrování objektů blob a nahrajte</span><span class="sxs-lookup"><span data-stu-id="d067c-151">Encrypt blob and upload</span></span>
<span data-ttu-id="d067c-152">Přidejte následující hello kód tooencrypt objekt blob a nahrajte ho tooyour účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d067c-152">Add hello following code tooencrypt a blob and upload it tooyour Azure storage account.</span></span> <span data-ttu-id="d067c-153">Hello **ResolveKeyAsync** vrátí metoda, která se používá IKey.</span><span class="sxs-lookup"><span data-stu-id="d067c-153">hello **ResolveKeyAsync** method that is used returns an IKey.</span></span>

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

<span data-ttu-id="d067c-154">Toto je snímek obrazovky hello [portálu Azure Classic](https://manage.windowsazure.com) pro objekt blob, která byla dříve zašifrována pomocí šifrování na straně klienta pomocí klíče uloženého v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d067c-154">Following is a screenshot from hello [Azure Classic Portal](https://manage.windowsazure.com) for a blob that has been encrypted by using client-side encryption with a key stored in Key Vault.</span></span> <span data-ttu-id="d067c-155">Hello **KeyId** vlastnost je hello identifikátor URI pro klíč hello v Key Vault, který slouží jako hello KEK.</span><span class="sxs-lookup"><span data-stu-id="d067c-155">hello **KeyId** property is hello URI for hello key in Key Vault that acts as hello KEK.</span></span> <span data-ttu-id="d067c-156">Hello **EncryptedKey** vlastnost obsahuje zašifrovaná verze hello CEK hello.</span><span class="sxs-lookup"><span data-stu-id="d067c-156">hello **EncryptedKey** property contains hello encrypted version of hello CEK.</span></span>

![Snímek obrazovky zobrazující metadata objektu Blob, který obsahuje metadata šifrování](./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png)

> [!NOTE]
> <span data-ttu-id="d067c-158">Pokud se podíváte na hello BlobEncryptionPolicy konstruktor, uvidíte, že může přijmout klíč nebo překladač.</span><span class="sxs-lookup"><span data-stu-id="d067c-158">If you look at hello BlobEncryptionPolicy constructor, you will see that it can accept a key and/or a resolver.</span></span> <span data-ttu-id="d067c-159">Mějte na paměti, které tuto chvíli nemůžete použít překladač pro šifrování, protože není aktuálně dělá podporují výchozí klíč.</span><span class="sxs-lookup"><span data-stu-id="d067c-159">Be aware that right now you cannot use a resolver for encryption because it does not currently support a default key.</span></span>
> 
> 

## <a name="decrypt-blob-and-download"></a><span data-ttu-id="d067c-160">Dešifrování objektů blob a stáhnout</span><span class="sxs-lookup"><span data-stu-id="d067c-160">Decrypt blob and download</span></span>
<span data-ttu-id="d067c-161">Dešifrování je ve skutečnosti při použití hello překladač třídy smysl.</span><span class="sxs-lookup"><span data-stu-id="d067c-161">Decryption is really when using hello Resolver classes make sense.</span></span> <span data-ttu-id="d067c-162">ID Hello hello klíče pro šifrování je přidružený objekt blob hello ve svých metadatech, takže neexistuje žádný důvod pro jste tooretrieve hello klíč a zapamatovat si hello přidružení mezi klíč a objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d067c-162">hello ID of hello key used for encryption is associated with hello blob in its metadata, so there is no reason for you tooretrieve hello key and remember hello association between key and blob.</span></span> <span data-ttu-id="d067c-163">Máte právě toomake se, že klíči hello zůstane v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d067c-163">You just have toomake sure that hello key remains in Key Vault.</span></span>   

<span data-ttu-id="d067c-164">Hello privátní klíč klíč RSA zůstane v Key Vault, takže pro dešifrování toooccur hello šifrované klíče z hello metadata objektu blob, který obsahuje hello CEK je odeslána tooKey trezoru pro dešifrování.</span><span class="sxs-lookup"><span data-stu-id="d067c-164">hello private key of an RSA Key remains in Key Vault, so for decryption toooccur, hello Encrypted Key from hello blob metadata that contains hello CEK is sent tooKey Vault for decryption.</span></span>

<span data-ttu-id="d067c-165">Přidejte následující toodecrypt hello blob, který jste právě nahráli hello.</span><span class="sxs-lookup"><span data-stu-id="d067c-165">Add hello following toodecrypt hello blob that you just uploaded.</span></span>

```csharp
// In this case, we will not pass a key and only pass hello resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> <span data-ttu-id="d067c-166">Je jednodušší, včetně několik jinými druhy překladače služby správy klíčů toomake: AggregateKeyResolver a CachingKeyResolver.</span><span class="sxs-lookup"><span data-stu-id="d067c-166">There are a couple of other kinds of resolvers toomake key management easier, including: AggregateKeyResolver and CachingKeyResolver.</span></span>
> 
> 

## <a name="use-key-vault-secrets"></a><span data-ttu-id="d067c-167">Pomocí Key Vault tajné klíče</span><span class="sxs-lookup"><span data-stu-id="d067c-167">Use Key Vault secrets</span></span>
<span data-ttu-id="d067c-168">Hello způsob toouse tajný klíč pomocí šifrování na straně klienta je prostřednictvím hello SymmetricKey třídy, protože v podstatě symetrický klíč je tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="d067c-168">hello way toouse a secret with client-side encryption is via hello SymmetricKey class because a secret is essentially a symmetric key.</span></span> <span data-ttu-id="d067c-169">Ale, jak je uvedeno výše, tajný klíč v Key Vault nemapují přesně tooa SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="d067c-169">But, as noted above, a secret in Key Vault does not map exactly tooa SymmetricKey.</span></span> <span data-ttu-id="d067c-170">Existují toounderstand několik věcí:</span><span class="sxs-lookup"><span data-stu-id="d067c-170">There are a few things toounderstand:</span></span>

* <span data-ttu-id="d067c-171">klíč Hello v SymmetricKey má toobe pevnou délkou: 128, 192, 256, 384 nebo 512 bitů.</span><span class="sxs-lookup"><span data-stu-id="d067c-171">hello key in a SymmetricKey has toobe a fixed length: 128, 192, 256, 384, or 512 bits.</span></span>
* <span data-ttu-id="d067c-172">Hello klíč v SymmetricKey by měla mít kódování Base64.</span><span class="sxs-lookup"><span data-stu-id="d067c-172">hello key in a SymmetricKey should be Base64 encoded.</span></span>
* <span data-ttu-id="d067c-173">Key Vault tajný klíč, který se použije jako SymmetricKey musí toohave obsahu typ "application/octet-stream" v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d067c-173">A Key Vault secret that will be used as a SymmetricKey needs toohave a Content Type of "application/octet-stream" in Key Vault.</span></span>

<span data-ttu-id="d067c-174">Tady je příklad v prostředí PowerShell v Key Vault, který lze použít jako SymmetricKey vytváření tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="d067c-174">Here is an example in PowerShell of creating a secret in Key Vault that can be used as a SymmetricKey.</span></span>
<span data-ttu-id="d067c-175">Upozorňujeme, že hodnota hello pevný programového, $key, je pro demonstrační účely jenom.</span><span class="sxs-lookup"><span data-stu-id="d067c-175">Please note that hello hard coded value, $key, is for demonstration purpose only.</span></span> <span data-ttu-id="d067c-176">V kódu můžete toogenerate tento klíč.</span><span class="sxs-lookup"><span data-stu-id="d067c-176">In your own code you'll want toogenerate this key.</span></span>

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

<span data-ttu-id="d067c-177">Ve vaší aplikaci konzoly můžete použít stejné volání jako před tooretrieve tento tajný jako SymmetricKey hello.</span><span class="sxs-lookup"><span data-stu-id="d067c-177">In your console application, you can use hello same call as before tooretrieve this secret as a SymmetricKey.</span></span>

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
<span data-ttu-id="d067c-178">A je to.</span><span class="sxs-lookup"><span data-stu-id="d067c-178">That's it.</span></span> <span data-ttu-id="d067c-179">Užijte si ji!</span><span class="sxs-lookup"><span data-stu-id="d067c-179">Enjoy!</span></span>

## <a name="next-steps"></a><span data-ttu-id="d067c-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d067c-180">Next steps</span></span>
<span data-ttu-id="d067c-181">Další informace o používání služby Microsoft Azure Storage pomocí C#, najdete v části [Microsoft Azure Storage Client Library pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="d067c-181">For more information about using Microsoft Azure Storage with C#, see [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="d067c-182">Další informace o hello Blob REST API najdete v tématu [rozhraní API REST služby objektů Blob](https://msdn.microsoft.com/library/azure/dd135733.aspx).</span><span class="sxs-lookup"><span data-stu-id="d067c-182">For more information about hello Blob REST API, see [Blob Service REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).</span></span>

<span data-ttu-id="d067c-183">Hello nejnovější informace o úložišti Microsoft Azure, přejděte toohello [Blog týmu Azure Storage Microsoft](http://blogs.msdn.com/b/windowsazurestorage/).</span><span class="sxs-lookup"><span data-stu-id="d067c-183">For hello latest information on Microsoft Azure Storage, go toohello [Microsoft Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/).</span></span>
