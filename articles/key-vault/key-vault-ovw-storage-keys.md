---
ms.assetid: 
title: "Klíče účtu úložiště Azure Key Vault"
description: "Klíče účtu úložiště poskytují seemless integrace mezi Azure Key Vault a klíče založené na přístup k účtu úložiště Azure."
ms.topic: article
services: key-vault
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/25/2017
ms.openlocfilehash: 3148088c88236c64e089fd25c98eb8ac7cdcbfea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-key-vault-storage-account-keys"></a><span data-ttu-id="d1828-103">Klíče účtu úložiště Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d1828-103">Azure Key Vault Storage Account Keys</span></span>

<span data-ttu-id="d1828-104">Před klíčů k účtu Azure Key Vault úložiště vývojáři museli spravovat vlastní klíče účtu úložiště Azure (ASA) a jejich otočení ručně nebo pomocí externího automatizace.</span><span class="sxs-lookup"><span data-stu-id="d1828-104">Before Azure Key Vault Storage Account Keys, developers had to manage their own Azure Storage Account (ASA) keys and rotate them manually or through an external automation.</span></span> <span data-ttu-id="d1828-105">Nyní, klíče účtu úložiště klíč trezoru jsou implementované jako [tajné klíče Key Vault](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) pro ověřování pomocí účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d1828-105">Now, Key Vault Storage Account Keys are implemented as [Key Vault secrets](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) for authenticating with an Azure Storage Account.</span></span> 

<span data-ttu-id="d1828-106">Klíčové funkce ASA spravuje tajný otočení pro vás a odebírá potřebu, přímém kontaktu s klíčem ASA prostřednictvím nabídky sdíleného přístupu podpisy (SAS) jako metodu.</span><span class="sxs-lookup"><span data-stu-id="d1828-106">The ASA key feature manages secret rotation for you and removes the need for your direct contact with an ASA key by offering Shared Access Signatures (SAS) as a method.</span></span> 

<span data-ttu-id="d1828-107">Další obecné informace o účtech úložiště Azure najdete v tématu [účty Azure storage](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span><span class="sxs-lookup"><span data-stu-id="d1828-107">For more general information on Azure Storage Accounts, see [About Azure storage accounts](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span></span>

## <a name="supporting-interfaces"></a><span data-ttu-id="d1828-108">Podpora rozhraní</span><span class="sxs-lookup"><span data-stu-id="d1828-108">Supporting interfaces</span></span>

<span data-ttu-id="d1828-109">Funkce klíče účtu úložiště Azure je začátku k dispozici prostřednictvím REST, .NET / C# a prostředí PowerShell rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d1828-109">The Azure Storage Account keys feature is initially available through the REST, .NET/C# and PowerShell interfaces.</span></span> <span data-ttu-id="d1828-110">Další informace najdete v tématu [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="d1828-110">For more information, see [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).</span></span>


## <a name="storage-account-keys-behavior"></a><span data-ttu-id="d1828-111">Chování klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="d1828-111">Storage account keys behavior</span></span>

### <a name="what-key-vault-manages"></a><span data-ttu-id="d1828-112">Co spravuje Key Vault</span><span class="sxs-lookup"><span data-stu-id="d1828-112">What Key Vault manages</span></span>

<span data-ttu-id="d1828-113">Při použití klíče účtu úložiště Key Vault provede několik funkcí správy interní vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="d1828-113">Key Vault performs several internal management functions on your behalf when you use Storage Account Keys.</span></span>

1. <span data-ttu-id="d1828-114">Azure Key Vault spravuje klíče z Azure Storage účtu (ASA).</span><span class="sxs-lookup"><span data-stu-id="d1828-114">Azure Key Vault manages keys of an Azure Storage Account (ASA).</span></span> 
    - <span data-ttu-id="d1828-115">Azure Key Vault interně, můžete seznam klíčů (sync) s účtem úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d1828-115">Internally, Azure Key Vault can list (sync) keys with an Azure Storage Account.</span></span>  
    - <span data-ttu-id="d1828-116">Azure Key Vault regeneruje (otočí) klíče pravidelně.</span><span class="sxs-lookup"><span data-stu-id="d1828-116">Azure Key Vault regenerates (rotates) the keys periodically.</span></span> 
    - <span data-ttu-id="d1828-117">Hodnoty klíče se nikdy vrátí jako odpověď na volajícího.</span><span class="sxs-lookup"><span data-stu-id="d1828-117">Key values are never returned in response to caller.</span></span> 
    - <span data-ttu-id="d1828-118">Azure Key Vault spravuje klíče účtů úložiště a klasické účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1828-118">Azure Key Vault manages keys of both Storage Accounts and Classic Storage Accounts.</span></span> 
2. <span data-ttu-id="d1828-119">Azure Key Vault umožňuje vás, vlastníka trezoru nebo objekt, k vytvoření definice SAS (účet nebo SAS služby).</span><span class="sxs-lookup"><span data-stu-id="d1828-119">Azure Key Vault allows you, the vault/object owner, to create SAS (account or service SAS) definitions.</span></span> 
    - <span data-ttu-id="d1828-120">SAS, vytvořené pomocí definice SAS, je vrácena jako tajný klíč prostřednictvím cesta k identifikátoru URI REST.</span><span class="sxs-lookup"><span data-stu-id="d1828-120">The SAS value, created using SAS definition, is returned as a secret via the REST URI path.</span></span> <span data-ttu-id="d1828-121">Další informace najdete v tématu [operace účet úložiště Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span><span class="sxs-lookup"><span data-stu-id="d1828-121">For more information, see [Azure Key Vault storage account operations](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span></span>

### <a name="naming-guidance"></a><span data-ttu-id="d1828-122">Pokyny pro pojmenování</span><span class="sxs-lookup"><span data-stu-id="d1828-122">Naming guidance</span></span>

<span data-ttu-id="d1828-123">Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="d1828-123">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>  
 
<span data-ttu-id="d1828-124">Název definice SAS musí být 1-102 znaků obsahující pouze 0 – 9,-z, A-Z.</span><span class="sxs-lookup"><span data-stu-id="d1828-124">A SAS definition name must be 1-102 characters in length containing only 0-9, a-z, A-Z.</span></span>

## <a name="developer-experience"></a><span data-ttu-id="d1828-125">Možnosti pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="d1828-125">Developer experience</span></span> 

### <a name="before-azure-key-vault-storage-keys"></a><span data-ttu-id="d1828-126">Před klíče úložiště Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d1828-126">Before Azure Key Vault Storage Keys</span></span> 

<span data-ttu-id="d1828-127">Vývojáři použít k proveďte následující postupy se klíč účtu úložiště a získat tak přístup k úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="d1828-127">Developers used to need to do the following practices with a storage account key to get access to Azure storage.</span></span> 
 
 ```
//create storage account using connection string containing account name 
// and the storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a><span data-ttu-id="d1828-128">Po klíče úložiště Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d1828-128">After Azure Key Vault Storage Keys</span></span> 

```
//Please make sure to set storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Use PowerShell command to get Secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  
-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use credentials and the Blob storage endpoint to create a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If SAS token is about to expire then Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);

  ```
 
 ### <a name="developer-best-practices"></a><span data-ttu-id="d1828-129">Doporučené postupy</span><span class="sxs-lookup"><span data-stu-id="d1828-129">Developer best practices</span></span> 

- <span data-ttu-id="d1828-130">Povolte jenom spravovat klíče ASA Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d1828-130">Only allow Key Vault to manage your ASA keys.</span></span> <span data-ttu-id="d1828-131">Nepokoušejte se spravovat sami, bude narušovat procesy Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d1828-131">Do not attempt to manage them yourself, you will interfere with Key Vault's processes.</span></span> 
- <span data-ttu-id="d1828-132">Nepovolit ASA klíče, který bude spravován více než jeden objekt Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d1828-132">Do not allow ASA keys to be managed by more than one Key Vault object.</span></span> 
- <span data-ttu-id="d1828-133">Pokud budete muset ručně znovu vygenerovat klíče ASA, doporučujeme vám obnovit prostřednictvím Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d1828-133">If you need to manually regenerate your ASA keys, we recommend that you regenerate them via Key Vault.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="d1828-134">Začínáme</span><span class="sxs-lookup"><span data-stu-id="d1828-134">Getting started</span></span>

### <a name="setup-for-role-based-access-control-rbac-permissions"></a><span data-ttu-id="d1828-135">Instalační program pro oprávnění k přístupu na základě rolí k řízení (RBAC)</span><span class="sxs-lookup"><span data-stu-id="d1828-135">Setup for role-based access control (RBAC) permissions</span></span>

<span data-ttu-id="d1828-136">Key Vault potřebuje oprávnění k *seznamu* a *znovu vygenerovat* klíče pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1828-136">Key Vault needs permissions to *list* and *regenerate* keys for a storage account.</span></span> <span data-ttu-id="d1828-137">Nastavte tato oprávnění pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d1828-137">Set up these permissions using the following steps:</span></span>

- <span data-ttu-id="d1828-138">Získejte ObjectId trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="d1828-138">Get ObjectId of Key Vault:</span></span> 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- <span data-ttu-id="d1828-139">Přiřazení role operátora klíče úložiště Azure Key Vault Identity:</span><span class="sxs-lookup"><span data-stu-id="d1828-139">Assign Storage Key Operator role to Azure Key Vault Identity:</span></span> 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > <span data-ttu-id="d1828-140">Pro typ účtu classic, nastavte parametr role na *"Classic úložiště účet klíč služby Role operátora"*.</span><span class="sxs-lookup"><span data-stu-id="d1828-140">For a classic account type, set the role parameter to *"Classic Storage Account Key Operator Service Role"*.</span></span>

### <a name="storage-account-onboarding"></a><span data-ttu-id="d1828-141">Registrace účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="d1828-141">Storage account onboarding</span></span> 

<span data-ttu-id="d1828-142">Příklad: jako vlastník objektu Key Vault, které přidáte objekt účtu úložiště Azure Key Vault pro zaváděním účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1828-142">Example: As a Key Vault object owner you add a storage account object to your Azure Key Vault to onboard a storage account.</span></span>

<span data-ttu-id="d1828-143">Během registrace, Key Vault musí ověřit, že identitu účtu registrace má oprávnění k *seznamu* a *znovu vygenerovat* klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1828-143">During onboarding, Key Vault needs to verify that the identity of the onboarding account has permissions to *list* and to *regenerate* storage keys.</span></span> <span data-ttu-id="d1828-144">Chcete-li ověřit, tato oprávnění, Key Vault získá OBO (na Behalf Of) token pomocí ověřovací služby, cílové skupiny nastavená na Azure Resource Manager a díky *seznamu* klíče volání služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d1828-144">In order to verify these permissions, Key Vault gets an OBO (On Behalf Of) token from the authentication service, audience set to Azure Resource Manager, and makes a *list* key call to the Azure Storage service.</span></span> <span data-ttu-id="d1828-145">Pokud *seznamu* volání selže, Key Vault vytvoření objektu selže s kódem stavu HTTP z *zakázáno*.</span><span class="sxs-lookup"><span data-stu-id="d1828-145">If the *list* call fails, the Key Vault object creation fails with a HTTP status code of *Forbidden*.</span></span> <span data-ttu-id="d1828-146">S úložištěm entity váš trezor klíčů v mezipaměti klíče uvedené tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="d1828-146">The keys listed in this fashion are cached with your key vault entity storage.</span></span> 

<span data-ttu-id="d1828-147">Key Vault musí ověřte, zda má identita *znovu vygenerovat* oprávnění předtím, než ji může převzít vlastnictví opětovné generování klíče.</span><span class="sxs-lookup"><span data-stu-id="d1828-147">Key Vault must verify that the identity has *regenerate* permissions before it can take ownership of regenerating your keys.</span></span> <span data-ttu-id="d1828-148">Chcete-li ověřit, že identita, prostřednictvím OBO token, stejně jako první strany identita Key Vault má tato oprávnění:</span><span class="sxs-lookup"><span data-stu-id="d1828-148">To verify that the identity, via OBO token, as well as the Key Vault first party identity has these permissions:</span></span>

- <span data-ttu-id="d1828-149">Key Vault seznam oprávnění RBAC na prostředků účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1828-149">Key Vault lists RBAC permissions on the storage account resource.</span></span>
- <span data-ttu-id="d1828-150">Key Vault ověří odpovědi pomocí regulárního výrazu shody akce a jiné akce.</span><span class="sxs-lookup"><span data-stu-id="d1828-150">Key Vault validates the response via regular expression matching of actions and non-actions.</span></span> 

<span data-ttu-id="d1828-151">Najít příklady podpůrné v [Key Vault - spravované ukázky klíče účtu úložiště](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span><span class="sxs-lookup"><span data-stu-id="d1828-151">Find some supporting examples at [Key Vault - Managed Storage Account Keys Samples](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span></span>

<span data-ttu-id="d1828-152">Není-li identitu *znovu vygenerovat* oprávnění nebo pokud Key Vault první strany identity nemá *seznamu* nebo *znovu vygenerovat* oprávnění a potom registrace požadavek selže, vrátí kód odpovídající chyby a zprávy.</span><span class="sxs-lookup"><span data-stu-id="d1828-152">If the identity does not have *regenerate* permissions or if Key Vault's first party identity doesn’t have *list* or *regenerate* permission, then the onboarding request fails returning an appropriate error code and message.</span></span> 

<span data-ttu-id="d1828-153">OBO token bude fungovat pouze při použití vlastních, aplikace nativního klienta PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d1828-153">The OBO token will only work when you use first-party, native client applications of either PowerShell or CLI.</span></span>

## <a name="other-applications"></a><span data-ttu-id="d1828-154">Ostatní aplikace</span><span class="sxs-lookup"><span data-stu-id="d1828-154">Other applications</span></span>

- <span data-ttu-id="d1828-155">Tokeny SAS, vytvářeny pomocí klíčů k účtu úložiště Key Vault, zadejte i více řízený přístup k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d1828-155">SAS tokens, constructed using Key Vault storage account keys, provide even more controlled access to an Azure storage account.</span></span> <span data-ttu-id="d1828-156">Další informace najdete v tématu [pomocí sdílené přístupové podpisy](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="d1828-156">For more information, see [Using shared access signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

## <a name="see-also"></a><span data-ttu-id="d1828-157">Viz také</span><span class="sxs-lookup"><span data-stu-id="d1828-157">See also</span></span>

- [<span data-ttu-id="d1828-158">Informace o klíčích, tajných kódech a certifikátech</span><span class="sxs-lookup"><span data-stu-id="d1828-158">About keys, secrets, and certificates</span></span>](https://docs.microsoft.com/rest/api/keyvault/)
- [<span data-ttu-id="d1828-159">Blog týmu trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="d1828-159">Key Vault Team Blog</span></span>](https://blogs.technet.microsoft.com/kv/)
