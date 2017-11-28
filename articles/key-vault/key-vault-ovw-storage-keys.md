---
ms.assetid: 
title: "aaaAzure klíče účtu úložiště klíč trezoru"
description: "Klíče účtu úložiště poskytují seemless integrace mezi Azure Key Vault a tooAzure přístup ke klíčům podle účtu úložiště."
ms.topic: article
services: key-vault
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/25/2017
ms.openlocfilehash: becdf97798a08164c48d3a7a14aea6ca54085c9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-storage-account-keys"></a><span data-ttu-id="82f86-103">Klíče účtu úložiště Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="82f86-103">Azure Key Vault Storage Account Keys</span></span>

<span data-ttu-id="82f86-104">Před klíčů k účtu Azure Key Vault úložiště vývojáři měla toomanage vlastní klíče účtu úložiště Azure (ASA) a jejich otočení ručně nebo pomocí externího automatizace.</span><span class="sxs-lookup"><span data-stu-id="82f86-104">Before Azure Key Vault Storage Account Keys, developers had toomanage their own Azure Storage Account (ASA) keys and rotate them manually or through an external automation.</span></span> <span data-ttu-id="82f86-105">Nyní, klíče účtu úložiště klíč trezoru jsou implementované jako [tajné klíče Key Vault](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) pro ověřování pomocí účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="82f86-105">Now, Key Vault Storage Account Keys are implemented as [Key Vault secrets](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) for authenticating with an Azure Storage Account.</span></span> 

<span data-ttu-id="82f86-106">klíčové funkce Hello ASA spravuje tajný otočení pro vás a odebere hello potřebu přímém kontaktu s klíčem ASA prostřednictvím nabídky sdíleného přístupu podpisy (SAS) jako metodu.</span><span class="sxs-lookup"><span data-stu-id="82f86-106">hello ASA key feature manages secret rotation for you and removes hello need for your direct contact with an ASA key by offering Shared Access Signatures (SAS) as a method.</span></span> 

<span data-ttu-id="82f86-107">Další obecné informace o účtech úložiště Azure najdete v tématu [účty Azure storage](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span><span class="sxs-lookup"><span data-stu-id="82f86-107">For more general information on Azure Storage Accounts, see [About Azure storage accounts](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span></span>

## <a name="supporting-interfaces"></a><span data-ttu-id="82f86-108">Podpora rozhraní</span><span class="sxs-lookup"><span data-stu-id="82f86-108">Supporting interfaces</span></span>

<span data-ttu-id="82f86-109">Hello funkce klíče účtu úložiště Azure je původně k dispozici prostřednictvím hello REST, .NET / C# a prostředí PowerShell rozhraní.</span><span class="sxs-lookup"><span data-stu-id="82f86-109">hello Azure Storage Account keys feature is initially available through hello REST, .NET/C# and PowerShell interfaces.</span></span> <span data-ttu-id="82f86-110">Další informace najdete v tématu [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="82f86-110">For more information, see [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).</span></span>


## <a name="storage-account-keys-behavior"></a><span data-ttu-id="82f86-111">Chování klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="82f86-111">Storage account keys behavior</span></span>

### <a name="what-key-vault-manages"></a><span data-ttu-id="82f86-112">Co spravuje Key Vault</span><span class="sxs-lookup"><span data-stu-id="82f86-112">What Key Vault manages</span></span>

<span data-ttu-id="82f86-113">Při použití klíče účtu úložiště Key Vault provede několik funkcí správy interní vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="82f86-113">Key Vault performs several internal management functions on your behalf when you use Storage Account Keys.</span></span>

1. <span data-ttu-id="82f86-114">Azure Key Vault spravuje klíče z Azure Storage účtu (ASA).</span><span class="sxs-lookup"><span data-stu-id="82f86-114">Azure Key Vault manages keys of an Azure Storage Account (ASA).</span></span> 
    - <span data-ttu-id="82f86-115">Azure Key Vault interně, můžete seznam klíčů (sync) s účtem úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="82f86-115">Internally, Azure Key Vault can list (sync) keys with an Azure Storage Account.</span></span>  
    - <span data-ttu-id="82f86-116">Azure Key Vault regeneruje (otočí) hello klíče pravidelně.</span><span class="sxs-lookup"><span data-stu-id="82f86-116">Azure Key Vault regenerates (rotates) hello keys periodically.</span></span> 
    - <span data-ttu-id="82f86-117">V odpovědi toocaller jsou nikdy vrátil hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="82f86-117">Key values are never returned in response toocaller.</span></span> 
    - <span data-ttu-id="82f86-118">Azure Key Vault spravuje klíče účtů úložiště a klasické účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="82f86-118">Azure Key Vault manages keys of both Storage Accounts and Classic Storage Accounts.</span></span> 
2. <span data-ttu-id="82f86-119">Azure Key Vault umožňuje můžete hello trezoru nebo objektu vlastníka, definice toocreate SAS (účet nebo SAS služby).</span><span class="sxs-lookup"><span data-stu-id="82f86-119">Azure Key Vault allows you, hello vault/object owner, toocreate SAS (account or service SAS) definitions.</span></span> 
    - <span data-ttu-id="82f86-120">Hello hodnotu SAS, vytvořené pomocí definice SAS, se vrátí jako tajný klíč prostřednictvím cesta REST URI hello.</span><span class="sxs-lookup"><span data-stu-id="82f86-120">hello SAS value, created using SAS definition, is returned as a secret via hello REST URI path.</span></span> <span data-ttu-id="82f86-121">Další informace najdete v tématu [operace účet úložiště Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span><span class="sxs-lookup"><span data-stu-id="82f86-121">For more information, see [Azure Key Vault storage account operations](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span></span>

### <a name="naming-guidance"></a><span data-ttu-id="82f86-122">Pokyny pro pojmenování</span><span class="sxs-lookup"><span data-stu-id="82f86-122">Naming guidance</span></span>

<span data-ttu-id="82f86-123">Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="82f86-123">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>  
 
<span data-ttu-id="82f86-124">Název definice SAS musí být 1-102 znaků obsahující pouze 0 – 9,-z, A-Z.</span><span class="sxs-lookup"><span data-stu-id="82f86-124">A SAS definition name must be 1-102 characters in length containing only 0-9, a-z, A-Z.</span></span>

## <a name="developer-experience"></a><span data-ttu-id="82f86-125">Možnosti pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="82f86-125">Developer experience</span></span> 

### <a name="before-azure-key-vault-storage-keys"></a><span data-ttu-id="82f86-126">Před klíče úložiště Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="82f86-126">Before Azure Key Vault Storage Keys</span></span> 

<span data-ttu-id="82f86-127">Toodo hello tooneed vývojáři použít následující postupy s úložiště tooAzure přístup tooget klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="82f86-127">Developers used tooneed toodo hello following practices with a storage account key tooget access tooAzure storage.</span></span> 
 
 ```
//create storage account using connection string containing account name 
// and hello storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a><span data-ttu-id="82f86-128">Po klíče úložiště Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="82f86-128">After Azure Key Vault Storage Keys</span></span> 

```
//Please make sure tooset storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Use PowerShell command tooget Secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  
-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using hello SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use credentials and hello Blob storage endpoint toocreate a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If SAS token is about tooexpire then Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);

  ```
 
 ### <a name="developer-best-practices"></a><span data-ttu-id="82f86-129">Doporučené postupy</span><span class="sxs-lookup"><span data-stu-id="82f86-129">Developer best practices</span></span> 

- <span data-ttu-id="82f86-130">Key Vault toomanage povolte jenom vaše ASA klíče.</span><span class="sxs-lookup"><span data-stu-id="82f86-130">Only allow Key Vault toomanage your ASA keys.</span></span> <span data-ttu-id="82f86-131">Nepokoušejte toomanage je sami, koliduje s procesy Key Vault.</span><span class="sxs-lookup"><span data-stu-id="82f86-131">Do not attempt toomanage them yourself, you will interfere with Key Vault's processes.</span></span> 
- <span data-ttu-id="82f86-132">Nepovolit toobe klíče ASA spravuje více než jeden objekt Key Vault.</span><span class="sxs-lookup"><span data-stu-id="82f86-132">Do not allow ASA keys toobe managed by more than one Key Vault object.</span></span> 
- <span data-ttu-id="82f86-133">Pokud potřebujete toomanually znovu vygenerovat klíče ASA, doporučujeme vám obnovit prostřednictvím Key Vault.</span><span class="sxs-lookup"><span data-stu-id="82f86-133">If you need toomanually regenerate your ASA keys, we recommend that you regenerate them via Key Vault.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="82f86-134">Začínáme</span><span class="sxs-lookup"><span data-stu-id="82f86-134">Getting started</span></span>

### <a name="setup-for-role-based-access-control-rbac-permissions"></a><span data-ttu-id="82f86-135">Instalační program pro oprávnění k přístupu na základě rolí k řízení (RBAC)</span><span class="sxs-lookup"><span data-stu-id="82f86-135">Setup for role-based access control (RBAC) permissions</span></span>

<span data-ttu-id="82f86-136">Key Vault potřebuje oprávnění příliš*seznamu* a *znovu vygenerovat* klíče pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="82f86-136">Key Vault needs permissions too*list* and *regenerate* keys for a storage account.</span></span> <span data-ttu-id="82f86-137">Nastavení těchto oprávnění pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="82f86-137">Set up these permissions using hello following steps:</span></span>

- <span data-ttu-id="82f86-138">Získejte ObjectId trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="82f86-138">Get ObjectId of Key Vault:</span></span> 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- <span data-ttu-id="82f86-139">Přiřaďte role tooAzure operátor klíč úložiště klíč trezoru Identity:</span><span class="sxs-lookup"><span data-stu-id="82f86-139">Assign Storage Key Operator role tooAzure Key Vault Identity:</span></span> 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > <span data-ttu-id="82f86-140">Pro typ účtu classic, nastavte parametr role hello příliš*"Classic úložiště účet klíč služby Role operátora"*.</span><span class="sxs-lookup"><span data-stu-id="82f86-140">For a classic account type, set hello role parameter too*"Classic Storage Account Key Operator Service Role"*.</span></span>

### <a name="storage-account-onboarding"></a><span data-ttu-id="82f86-141">Registrace účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="82f86-141">Storage account onboarding</span></span> 

<span data-ttu-id="82f86-142">Příklad: Jako vlastník objektu Key Vault přidat úložiště účet objekt tooyour Azure Key Vault tooonboard účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="82f86-142">Example: As a Key Vault object owner you add a storage account object tooyour Azure Key Vault tooonboard a storage account.</span></span>

<span data-ttu-id="82f86-143">Během registrace, Key Vault musí tooverify hello identitu účtu registrace hello příliš má oprávnění*seznamu* a příliš*znovu vygenerovat* klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="82f86-143">During onboarding, Key Vault needs tooverify that hello identity of hello onboarding account has permissions too*list* and too*regenerate* storage keys.</span></span> <span data-ttu-id="82f86-144">V pořadí tooverify tato oprávnění, Key Vault získá OBO (na Behalf Of) token ze služby ověřování hello, cílová skupina nastavit tooAzure Resource Manager a umožňuje *seznamu* klíče služby Azure Storage toohello volání.</span><span class="sxs-lookup"><span data-stu-id="82f86-144">In order tooverify these permissions, Key Vault gets an OBO (On Behalf Of) token from hello authentication service, audience set tooAzure Resource Manager, and makes a *list* key call toohello Azure Storage service.</span></span> <span data-ttu-id="82f86-145">Pokud hello *seznamu* volání selže, hello Key Vault vytvoření objektu selže s kódem stavu HTTP z *zakázáno*.</span><span class="sxs-lookup"><span data-stu-id="82f86-145">If hello *list* call fails, hello Key Vault object creation fails with a HTTP status code of *Forbidden*.</span></span> <span data-ttu-id="82f86-146">s úložištěm entity váš trezor klíčů v mezipaměti klíče Hello uvedené tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="82f86-146">hello keys listed in this fashion are cached with your key vault entity storage.</span></span> 

<span data-ttu-id="82f86-147">Key Vault musí ověřte, zda text hello identity *znovu vygenerovat* oprávnění předtím, než ji může převzít vlastnictví opětovné generování klíče.</span><span class="sxs-lookup"><span data-stu-id="82f86-147">Key Vault must verify that hello identity has *regenerate* permissions before it can take ownership of regenerating your keys.</span></span> <span data-ttu-id="82f86-148">tooverify, který hello identitu prostřednictvím tokenu OBO, jakož i hello Key Vault první strany identita má tato oprávnění:</span><span class="sxs-lookup"><span data-stu-id="82f86-148">tooverify that hello identity, via OBO token, as well as hello Key Vault first party identity has these permissions:</span></span>

- <span data-ttu-id="82f86-149">Key Vault seznam oprávnění RBAC na prostředku účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="82f86-149">Key Vault lists RBAC permissions on hello storage account resource.</span></span>
- <span data-ttu-id="82f86-150">Key Vault ověří hello odpovědi odpovídající regulárnímu výrazu akcí a jiné akce.</span><span class="sxs-lookup"><span data-stu-id="82f86-150">Key Vault validates hello response via regular expression matching of actions and non-actions.</span></span> 

<span data-ttu-id="82f86-151">Najít příklady podpůrné v [Key Vault - spravované ukázky klíče účtu úložiště](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span><span class="sxs-lookup"><span data-stu-id="82f86-151">Find some supporting examples at [Key Vault - Managed Storage Account Keys Samples](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span></span>

<span data-ttu-id="82f86-152">Pokud nemá hello identity *znovu vygenerovat* oprávnění nebo pokud Key Vault první strany identity nemá *seznamu* nebo *znovu vygenerovat* oprávnění a potom hello registrace požadavek selže, vrátí kód odpovídající chyby a zprávy.</span><span class="sxs-lookup"><span data-stu-id="82f86-152">If hello identity does not have *regenerate* permissions or if Key Vault's first party identity doesn’t have *list* or *regenerate* permission, then hello onboarding request fails returning an appropriate error code and message.</span></span> 

<span data-ttu-id="82f86-153">Hello OBO token bude fungovat pouze při použití vlastních, aplikace nativního klienta PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="82f86-153">hello OBO token will only work when you use first-party, native client applications of either PowerShell or CLI.</span></span>

## <a name="other-applications"></a><span data-ttu-id="82f86-154">Ostatní aplikace</span><span class="sxs-lookup"><span data-stu-id="82f86-154">Other applications</span></span>

- <span data-ttu-id="82f86-155">Tokeny SAS, vytvářeny pomocí klíčů k účtu úložiště Key Vault, zadejte i další tooan řízený přístup účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="82f86-155">SAS tokens, constructed using Key Vault storage account keys, provide even more controlled access tooan Azure storage account.</span></span> <span data-ttu-id="82f86-156">Další informace najdete v tématu [pomocí sdílené přístupové podpisy](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="82f86-156">For more information, see [Using shared access signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

## <a name="see-also"></a><span data-ttu-id="82f86-157">Viz také</span><span class="sxs-lookup"><span data-stu-id="82f86-157">See also</span></span>

- [<span data-ttu-id="82f86-158">Informace o klíčích, tajných kódech a certifikátech</span><span class="sxs-lookup"><span data-stu-id="82f86-158">About keys, secrets, and certificates</span></span>](https://docs.microsoft.com/rest/api/keyvault/)
- [<span data-ttu-id="82f86-159">Blog týmu trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="82f86-159">Key Vault Team Blog</span></span>](https://blogs.technet.microsoft.com/kv/)
