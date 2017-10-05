---
title: "Kurz: Šifrování a dešifrování objektů BLOB v úložišti Azure pomocí Azure Key Vault | Microsoft Docs"
description: "Jak šifrování a dešifrování objektu blob pomocí šifrování na straně klienta pro úložiště Microsoft Azure s Azure Key Vault."
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
ms.openlocfilehash: 0c33742a0212e670072a947a2d2ab8304c77b973
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a><span data-ttu-id="5379c-103">Kurz: Šifrování a dešifrování objektů BLOB v úložišti Microsoft Azure pomocí Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5379c-103">Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault</span></span>
## <a name="introduction"></a><span data-ttu-id="5379c-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="5379c-104">Introduction</span></span>
<span data-ttu-id="5379c-105">Tento kurz se zaměřuje na postup ověření pomocí šifrování na straně klienta úložiště s Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5379c-105">This tutorial covers how to make use of client-side storage encryption with Azure Key Vault.</span></span> <span data-ttu-id="5379c-106">Provede vás provedou postupem pro šifrování a dešifrování objektů blob v konzolové aplikaci použití těchto technologií.</span><span class="sxs-lookup"><span data-stu-id="5379c-106">It walks you through how to encrypt and decrypt a blob in a console application using these technologies.</span></span>

<span data-ttu-id="5379c-107">**Odhadovaný čas dokončení:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="5379c-107">**Estimated time to complete:** 20 minutes</span></span>

<span data-ttu-id="5379c-108">Souhrnné informace o Azure Key Vault naleznete v tématu [co je Azure Key Vault?](../key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5379c-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](../key-vault/key-vault-whatis.md).</span></span>

<span data-ttu-id="5379c-109">Další informace o šifrování na straně klienta pro Azure Storage najdete v části [šifrování na straně klienta a Azure Key Vault pro Microsoft Azure Storage](storage-client-side-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="5379c-109">For overview information about client-side encryption for Azure Storage, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](storage-client-side-encryption.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5379c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5379c-110">Prerequisites</span></span>
<span data-ttu-id="5379c-111">K dokončení tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="5379c-111">To complete this tutorial, you must have the following:</span></span>

* <span data-ttu-id="5379c-112">Účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="5379c-112">An Azure Storage account</span></span>
* <span data-ttu-id="5379c-113">Visual Studio 2013 nebo novější</span><span class="sxs-lookup"><span data-stu-id="5379c-113">Visual Studio 2013 or later</span></span>
* <span data-ttu-id="5379c-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5379c-114">Azure PowerShell</span></span>

## <a name="overview-of-client-side-encryption"></a><span data-ttu-id="5379c-115">Přehled šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="5379c-115">Overview of client-side encryption</span></span>
<span data-ttu-id="5379c-116">Přehled šifrování na straně klienta pro Azure Storage najdete v tématu [šifrování na straně klienta a Azure Key Vault pro úložiště Microsoft Azure](storage-client-side-encryption.md)</span><span class="sxs-lookup"><span data-stu-id="5379c-116">For an overview of client-side encryption for Azure Storage, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](storage-client-side-encryption.md)</span></span>

<span data-ttu-id="5379c-117">Zde je stručný popis toho, jak funguje šifrování na straně klienta:</span><span class="sxs-lookup"><span data-stu-id="5379c-117">Here is a brief description of how client side encryption works:</span></span>

1. <span data-ttu-id="5379c-118">Klient Azure Storage SDK vytvoří obsahu šifrovací klíč (CEK), což je použití jednoho bránu symetrického klíče.</span><span class="sxs-lookup"><span data-stu-id="5379c-118">The Azure Storage client SDK generates a content encryption key (CEK), which is a one-time-use symmetric key.</span></span>
2. <span data-ttu-id="5379c-119">Zákaznická data zašifrována pomocí této CEK.</span><span class="sxs-lookup"><span data-stu-id="5379c-119">Customer data is encrypted using this CEK.</span></span>
3. <span data-ttu-id="5379c-120">CEK je vnořena (šifrované) pomocí klíčů šifrovacího klíče (KEK).</span><span class="sxs-lookup"><span data-stu-id="5379c-120">The CEK is then wrapped (encrypted) using the key encryption key (KEK).</span></span> <span data-ttu-id="5379c-121">Klíče KEK je identifikovaná identifikátorem klíče a může být pár asymetrických klíčů nebo symetrického klíče a mohou být spravovaný místně nebo uloženy v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5379c-121">The KEK is identified by a key identifier and can be an asymmetric key pair or a symmetric key and can be managed locally or stored in Azure Key Vault.</span></span> <span data-ttu-id="5379c-122">Klient úložiště pro samotné nikdy má přístup k klíče KEK.</span><span class="sxs-lookup"><span data-stu-id="5379c-122">The Storage client itself never has access to the KEK.</span></span> <span data-ttu-id="5379c-123">Vyvolá právě algoritmus zabalení klíče, který zajišťuje Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5379c-123">It just invokes the key wrapping algorithm that is provided by Key Vault.</span></span> <span data-ttu-id="5379c-124">Tato volba se pro klíč zabalení/rozbalování Pokud chtějí použít vlastního zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="5379c-124">Customers can choose to use custom providers for key wrapping/unwrapping if they want.</span></span>
4. <span data-ttu-id="5379c-125">Šifrovaná data se pak odešlou do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5379c-125">The encrypted data is then uploaded to the Azure Storage service.</span></span>

## <a name="set-up-your-azure-key-vault"></a><span data-ttu-id="5379c-126">Nastavení Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5379c-126">Set up your Azure Key Vault</span></span>
<span data-ttu-id="5379c-127">Chcete-li pokračovat v tomto kurzu, je třeba provést následující kroky, které jsou uvedeny v tomto kurzu [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md):</span><span class="sxs-lookup"><span data-stu-id="5379c-127">In order to proceed with this tutorial, you need to do the following steps, which are outlined in the tutorial  [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md):</span></span>

* <span data-ttu-id="5379c-128">Vytvoření trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="5379c-128">Create a key vault.</span></span>
* <span data-ttu-id="5379c-129">Přidejte klíče nebo tajného klíče do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="5379c-129">Add a key or secret to the key vault.</span></span>
* <span data-ttu-id="5379c-130">Zaregistrujte aplikaci s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5379c-130">Register an application with Azure Active Directory.</span></span>
* <span data-ttu-id="5379c-131">Autorizovat aplikaci pro použití klíče nebo tajného klíče.</span><span class="sxs-lookup"><span data-stu-id="5379c-131">Authorize the application to use the key or secret.</span></span>

<span data-ttu-id="5379c-132">Poznamenejte si ClientID a ClientSecret, které byly generovány při registraci aplikace na Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5379c-132">Make note of the ClientID and ClientSecret that were generated when registering an application with Azure Active Directory.</span></span>

<span data-ttu-id="5379c-133">Vytvořte oba klíče v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="5379c-133">Create both keys in the key vault.</span></span> <span data-ttu-id="5379c-134">Pro zbývající části tohoto kurzu předpokládáme, že jste použili následující názvy: ContosoKeyVault a TestRSAKey1.</span><span class="sxs-lookup"><span data-stu-id="5379c-134">We assume for the rest of the tutorial that you have used the following names: ContosoKeyVault and TestRSAKey1.</span></span>

## <a name="create-a-console-application-with-packages-and-appsettings"></a><span data-ttu-id="5379c-135">Vytvořte konzolovou aplikaci s balíčky a AppSettings</span><span class="sxs-lookup"><span data-stu-id="5379c-135">Create a console application with packages and AppSettings</span></span>
<span data-ttu-id="5379c-136">V sadě Visual Studio vytvořte novou konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5379c-136">In Visual Studio, create a new console application.</span></span>

<span data-ttu-id="5379c-137">Přidání balíčků nuget potřebné v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="5379c-137">Add necessary nuget packages in the Package Manager Console.</span></span>

```
Install-Package WindowsAzure.Storage

// This is the latest stable release for ADAL.
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

<span data-ttu-id="5379c-138">Přidejte AppSettings souboru App.Config.</span><span class="sxs-lookup"><span data-stu-id="5379c-138">Add AppSettings to the App.Config.</span></span>

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

<span data-ttu-id="5379c-139">Přidejte následující `using` příkazů a ujistěte se, že do projektu přidejte odkaz na System.Configuration.</span><span class="sxs-lookup"><span data-stu-id="5379c-139">Add the following `using` statements and make sure to add a reference to System.Configuration to the project.</span></span>

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

## <a name="add-a-method-to-get-a-token-to-your-console-application"></a><span data-ttu-id="5379c-140">Přidejte metodu k získání tokenu pro konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="5379c-140">Add a method to get a token to your console application</span></span>
<span data-ttu-id="5379c-141">Následující metoda se používá Key Vault třídy, které potřebují k ověřování pro přístup k trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="5379c-141">The following method is used by Key Vault classes that need to authenticate for access to your key vault.</span></span>

```csharp
private async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(
        ConfigurationManager.AppSettings["clientId"],
        ConfigurationManager.AppSettings["clientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

## <a name="access-storage-and-key-vault-in-your-program"></a><span data-ttu-id="5379c-142">Přístup k úložišti a Key Vault v programu</span><span class="sxs-lookup"><span data-stu-id="5379c-142">Access Storage and Key Vault in your program</span></span>
<span data-ttu-id="5379c-143">Hlavní funkce přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="5379c-143">In the Main function, add the following code.</span></span>

```csharp
// This is standard code to interact with Blob storage.
StorageCredentials creds = new StorageCredentials(
    ConfigurationManager.AppSettings["accountName"],
       ConfigurationManager.AppSettings["accountKey"]);
CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
CloudBlobClient client = account.CreateCloudBlobClient();
CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
contain.CreateIfNotExists();

// The Resolver object is used to interact with Key Vault for Azure Storage.
// This is where the GetToken method from above is used.
KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);
```

> [!NOTE]
> <span data-ttu-id="5379c-144">Trezor klíčů objektové modely</span><span class="sxs-lookup"><span data-stu-id="5379c-144">Key Vault Object Models</span></span>
> 
> <span data-ttu-id="5379c-145">Je důležité si uvědomit, že jsou ve skutečnosti dvě Key Vault objektové modely zajímat: jeden je založená na volání rozhraní API REST (KeyVault obor názvů) a druhá je rozšířením pro šifrování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="5379c-145">It is important to understand that there are actually two Key Vault object models to be aware of: one is based on the REST API (KeyVault namespace) and the other is an extension for client-side encryption.</span></span>
> 
> <span data-ttu-id="5379c-146">Klíč trezoru klient komunikuje se službou REST API a porozuměl jim JSON webové klíče a tajné klíče pro dva druhy věcí, které jsou obsaženy v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5379c-146">The Key Vault Client interacts with the REST API and understands JSON Web Keys and secrets for the two kinds of things that are contained in Key Vault.</span></span>
> 
> <span data-ttu-id="5379c-147">Klíč trezoru rozšíření jsou třídy, které zdá se, že vytvořený specificky pro šifrování na straně klienta ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5379c-147">The Key Vault Extensions are classes that seem specifically created for client-side encryption in Azure Storage.</span></span> <span data-ttu-id="5379c-148">Obsahují rozhraní pro třídy podle konceptu překladač klíč a klíče (IKey).</span><span class="sxs-lookup"><span data-stu-id="5379c-148">They contain an interface for keys (IKey) and classes based on the concept of a Key Resolver.</span></span> <span data-ttu-id="5379c-149">Existují dvě implementace IKey, které je nutné znát: RSAKey a SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="5379c-149">There are two implementations of IKey that you need to know: RSAKey and SymmetricKey.</span></span> <span data-ttu-id="5379c-150">Nyní dějí se shoduje s věcí, které jsou obsaženy v Key Vault, ale v tomto okamžiku jsou nezávislé třídy (tak klíč a tajný klíč načíst klíč trezoru klient neimplementuje IKey).</span><span class="sxs-lookup"><span data-stu-id="5379c-150">Now they happen to coincide with the things that are contained in a Key Vault, but at this point they are independent classes (so the Key and Secret retrieved by the Key Vault Client do not implement IKey).</span></span>
> 
> 

## <a name="encrypt-blob-and-upload"></a><span data-ttu-id="5379c-151">Šifrování objektů blob a nahrajte</span><span class="sxs-lookup"><span data-stu-id="5379c-151">Encrypt blob and upload</span></span>
<span data-ttu-id="5379c-152">Přidejte následující kód k šifrování objekt blob a nahrajte ho do účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5379c-152">Add the following code to encrypt a blob and upload it to your Azure storage account.</span></span> <span data-ttu-id="5379c-153">**ResolveKeyAsync** vrátí metoda, která se používá IKey.</span><span class="sxs-lookup"><span data-stu-id="5379c-153">The **ResolveKeyAsync** method that is used returns an IKey.</span></span>

```csharp
// Retrieve the key that you created previously.
// The IKey that is returned here is an RsaKey.
// Remember that we used the names contosokeyvault and testrsakey1.
var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();

// Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Reference a block blob.
CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

// Upload using the UploadFromStream method.
using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
    blob.UploadFromStream(stream, stream.Length, null, options, null);
```

<span data-ttu-id="5379c-154">Toto je snímek obrazovky [portálu Azure Classic](https://manage.windowsazure.com) pro objekt blob, která byla dříve zašifrována pomocí šifrování na straně klienta pomocí klíče uloženého v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5379c-154">Following is a screenshot from the [Azure Classic Portal](https://manage.windowsazure.com) for a blob that has been encrypted by using client-side encryption with a key stored in Key Vault.</span></span> <span data-ttu-id="5379c-155">**KeyId** vlastnost je identifikátor URI pro klíč v trezoru klíč, který funguje jako klíče KEK.</span><span class="sxs-lookup"><span data-stu-id="5379c-155">The **KeyId** property is the URI for the key in Key Vault that acts as the KEK.</span></span> <span data-ttu-id="5379c-156">**EncryptedKey** vlastnost obsahuje zašifrovaná verze CEK.</span><span class="sxs-lookup"><span data-stu-id="5379c-156">The **EncryptedKey** property contains the encrypted version of the CEK.</span></span>

![Snímek obrazovky zobrazující metadata objektu Blob, který obsahuje metadata šifrování](./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png)

> [!NOTE]
> <span data-ttu-id="5379c-158">Pokud se podíváte na BlobEncryptionPolicy konstruktoru, uvidíte, že může přijmout klíč nebo překladač.</span><span class="sxs-lookup"><span data-stu-id="5379c-158">If you look at the BlobEncryptionPolicy constructor, you will see that it can accept a key and/or a resolver.</span></span> <span data-ttu-id="5379c-159">Mějte na paměti, které tuto chvíli nemůžete použít překladač pro šifrování, protože není aktuálně dělá podporují výchozí klíč.</span><span class="sxs-lookup"><span data-stu-id="5379c-159">Be aware that right now you cannot use a resolver for encryption because it does not currently support a default key.</span></span>
> 
> 

## <a name="decrypt-blob-and-download"></a><span data-ttu-id="5379c-160">Dešifrování objektů blob a stáhnout</span><span class="sxs-lookup"><span data-stu-id="5379c-160">Decrypt blob and download</span></span>
<span data-ttu-id="5379c-161">Dešifrování je ve skutečnosti při používání tříd překladač smysl.</span><span class="sxs-lookup"><span data-stu-id="5379c-161">Decryption is really when using the Resolver classes make sense.</span></span> <span data-ttu-id="5379c-162">ID klíče pro šifrování je přidružený objekt blob ve svých metadatech, takže neexistuje žádný důvod pro načíst klíč a nezapomeňte přidružení mezi klíč a objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5379c-162">The ID of the key used for encryption is associated with the blob in its metadata, so there is no reason for you to retrieve the key and remember the association between key and blob.</span></span> <span data-ttu-id="5379c-163">Musíte se ujistěte, že klíč zůstane v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5379c-163">You just have to make sure that the key remains in Key Vault.</span></span>   

<span data-ttu-id="5379c-164">Privátní klíč klíč RSA zůstane v Key Vault, takže k dešifrování proběhnout, je zašifrovaná klíč z metadata objektu blob, který obsahuje CEK odeslán Key Vault pro dešifrování.</span><span class="sxs-lookup"><span data-stu-id="5379c-164">The private key of an RSA Key remains in Key Vault, so for decryption to occur, the Encrypted Key from the blob metadata that contains the CEK is sent to Key Vault for decryption.</span></span>

<span data-ttu-id="5379c-165">Přidejte následující příkaz pro dešifrování objektů blob, který jste právě nahráli.</span><span class="sxs-lookup"><span data-stu-id="5379c-165">Add the following to decrypt the blob that you just uploaded.</span></span>

```csharp
// In this case, we will not pass a key and only pass the resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> <span data-ttu-id="5379c-166">Existuje několik dalších druhy překladače v zájmu snazší správy klíčů, včetně: AggregateKeyResolver a CachingKeyResolver.</span><span class="sxs-lookup"><span data-stu-id="5379c-166">There are a couple of other kinds of resolvers to make key management easier, including: AggregateKeyResolver and CachingKeyResolver.</span></span>
> 
> 

## <a name="use-key-vault-secrets"></a><span data-ttu-id="5379c-167">Pomocí Key Vault tajné klíče</span><span class="sxs-lookup"><span data-stu-id="5379c-167">Use Key Vault secrets</span></span>
<span data-ttu-id="5379c-168">Za způsob, jak pomocí šifrování na straně klienta tajný klíč je prostřednictvím třídy SymmetricKey, protože v podstatě symetrický klíč je tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="5379c-168">The way to use a secret with client-side encryption is via the SymmetricKey class because a secret is essentially a symmetric key.</span></span> <span data-ttu-id="5379c-169">Ale, jak je uvedeno výše, tajný klíč v Key Vault nejsou namapované přesně SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="5379c-169">But, as noted above, a secret in Key Vault does not map exactly to a SymmetricKey.</span></span> <span data-ttu-id="5379c-170">Existuje několik postupů k pochopení:</span><span class="sxs-lookup"><span data-stu-id="5379c-170">There are a few things to understand:</span></span>

* <span data-ttu-id="5379c-171">Musí být pevnou délkou klíče v SymmetricKey: 128, 192, 256, 384 nebo 512 bitů.</span><span class="sxs-lookup"><span data-stu-id="5379c-171">The key in a SymmetricKey has to be a fixed length: 128, 192, 256, 384, or 512 bits.</span></span>
* <span data-ttu-id="5379c-172">Klíče v SymmetricKey by měla mít kódování Base64.</span><span class="sxs-lookup"><span data-stu-id="5379c-172">The key in a SymmetricKey should be Base64 encoded.</span></span>
* <span data-ttu-id="5379c-173">Key Vault tajný klíč, který se použije jako SymmetricKey musí mít obsahu typ "application/octet-stream" v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5379c-173">A Key Vault secret that will be used as a SymmetricKey needs to have a Content Type of "application/octet-stream" in Key Vault.</span></span>

<span data-ttu-id="5379c-174">Tady je příklad v prostředí PowerShell v Key Vault, který lze použít jako SymmetricKey vytváření tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="5379c-174">Here is an example in PowerShell of creating a secret in Key Vault that can be used as a SymmetricKey.</span></span>
<span data-ttu-id="5379c-175">Upozorňujeme, že pevně zakódovaný hodnota, $key, je pro demonstrační účely jenom.</span><span class="sxs-lookup"><span data-stu-id="5379c-175">Please note that the hard coded value, $key, is for demonstration purpose only.</span></span> <span data-ttu-id="5379c-176">V kódu budete chtít vygenerovat tento klíč.</span><span class="sxs-lookup"><span data-stu-id="5379c-176">In your own code you'll want to generate this key.</span></span>

```csharp
// Here we are making a 128-bit key so we have 16 characters.
//     The characters are in the ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute the VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

<span data-ttu-id="5379c-177">Ve vaší aplikaci konzoly můžete použít stejné volání jako před načíst tento tajný klíč jako SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="5379c-177">In your console application, you can use the same call as before to retrieve this secret as a SymmetricKey.</span></span>

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
<span data-ttu-id="5379c-178">A je to.</span><span class="sxs-lookup"><span data-stu-id="5379c-178">That's it.</span></span> <span data-ttu-id="5379c-179">Užijte si ji!</span><span class="sxs-lookup"><span data-stu-id="5379c-179">Enjoy!</span></span>

## <a name="next-steps"></a><span data-ttu-id="5379c-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5379c-180">Next steps</span></span>
<span data-ttu-id="5379c-181">Další informace o používání služby Microsoft Azure Storage pomocí C#, najdete v části [Microsoft Azure Storage Client Library pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="5379c-181">For more information about using Microsoft Azure Storage with C#, see [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="5379c-182">Další informace o rozhraní REST API objektů Blob najdete v tématu [rozhraní API REST služby objektů Blob](https://msdn.microsoft.com/library/azure/dd135733.aspx).</span><span class="sxs-lookup"><span data-stu-id="5379c-182">For more information about the Blob REST API, see [Blob Service REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).</span></span>

<span data-ttu-id="5379c-183">Nejnovější informace o úložišti Microsoft Azure, přejděte na [Blog týmu Azure Storage Microsoft](http://blogs.msdn.com/b/windowsazurestorage/).</span><span class="sxs-lookup"><span data-stu-id="5379c-183">For the latest information on Microsoft Azure Storage, go to the [Microsoft Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/).</span></span>
