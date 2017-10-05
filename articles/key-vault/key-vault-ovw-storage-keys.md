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
# <a name="azure-key-vault-storage-account-keys"></a>Klíče účtu úložiště Azure Key Vault

Před klíčů k účtu Azure Key Vault úložiště vývojáři museli spravovat vlastní klíče účtu úložiště Azure (ASA) a jejich otočení ručně nebo pomocí externího automatizace. Nyní, klíče účtu úložiště klíč trezoru jsou implementované jako [tajné klíče Key Vault](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) pro ověřování pomocí účtu úložiště Azure. 

Klíčové funkce ASA spravuje tajný otočení pro vás a odebírá potřebu, přímém kontaktu s klíčem ASA prostřednictvím nabídky sdíleného přístupu podpisy (SAS) jako metodu. 

Další obecné informace o účtech úložiště Azure najdete v tématu [účty Azure storage](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Podpora rozhraní

Funkce klíče účtu úložiště Azure je začátku k dispozici prostřednictvím REST, .NET / C# a prostředí PowerShell rozhraní. Další informace najdete v tématu [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).


## <a name="storage-account-keys-behavior"></a>Chování klíče účtu úložiště

### <a name="what-key-vault-manages"></a>Co spravuje Key Vault

Při použití klíče účtu úložiště Key Vault provede několik funkcí správy interní vaším jménem.

1. Azure Key Vault spravuje klíče z Azure Storage účtu (ASA). 
    - Azure Key Vault interně, můžete seznam klíčů (sync) s účtem úložiště Azure.  
    - Azure Key Vault regeneruje (otočí) klíče pravidelně. 
    - Hodnoty klíče se nikdy vrátí jako odpověď na volajícího. 
    - Azure Key Vault spravuje klíče účtů úložiště a klasické účty úložiště. 
2. Azure Key Vault umožňuje vás, vlastníka trezoru nebo objekt, k vytvoření definice SAS (účet nebo SAS služby). 
    - SAS, vytvořené pomocí definice SAS, je vrácena jako tajný klíč prostřednictvím cesta k identifikátoru URI REST. Další informace najdete v tématu [operace účet úložiště Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

### <a name="naming-guidance"></a>Pokyny pro pojmenování

Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.  
 
Název definice SAS musí být 1-102 znaků obsahující pouze 0 – 9,-z, A-Z.

## <a name="developer-experience"></a>Možnosti pro vývojáře 

### <a name="before-azure-key-vault-storage-keys"></a>Před klíče úložiště Azure Key Vault 

Vývojáři použít k proveďte následující postupy se klíč účtu úložiště a získat tak přístup k úložišti Azure. 
 
 ```
//create storage account using connection string containing account name 
// and the storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>Po klíče úložiště Azure Key Vault 

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
 
 ### <a name="developer-best-practices"></a>Doporučené postupy 

- Povolte jenom spravovat klíče ASA Key Vault. Nepokoušejte se spravovat sami, bude narušovat procesy Key Vault. 
- Nepovolit ASA klíče, který bude spravován více než jeden objekt Key Vault. 
- Pokud budete muset ručně znovu vygenerovat klíče ASA, doporučujeme vám obnovit prostřednictvím Key Vault. 

## <a name="getting-started"></a>Začínáme

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Instalační program pro oprávnění k přístupu na základě rolí k řízení (RBAC)

Key Vault potřebuje oprávnění k *seznamu* a *znovu vygenerovat* klíče pro účet úložiště. Nastavte tato oprávnění pomocí následujících kroků:

- Získejte ObjectId trezoru klíčů: 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- Přiřazení role operátora klíče úložiště Azure Key Vault Identity: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > Pro typ účtu classic, nastavte parametr role na *"Classic úložiště účet klíč služby Role operátora"*.

### <a name="storage-account-onboarding"></a>Registrace účtu úložiště 

Příklad: jako vlastník objektu Key Vault, které přidáte objekt účtu úložiště Azure Key Vault pro zaváděním účet úložiště.

Během registrace, Key Vault musí ověřit, že identitu účtu registrace má oprávnění k *seznamu* a *znovu vygenerovat* klíče úložiště. Chcete-li ověřit, tato oprávnění, Key Vault získá OBO (na Behalf Of) token pomocí ověřovací služby, cílové skupiny nastavená na Azure Resource Manager a díky *seznamu* klíče volání služby Azure Storage. Pokud *seznamu* volání selže, Key Vault vytvoření objektu selže s kódem stavu HTTP z *zakázáno*. S úložištěm entity váš trezor klíčů v mezipaměti klíče uvedené tímto způsobem. 

Key Vault musí ověřte, zda má identita *znovu vygenerovat* oprávnění předtím, než ji může převzít vlastnictví opětovné generování klíče. Chcete-li ověřit, že identita, prostřednictvím OBO token, stejně jako první strany identita Key Vault má tato oprávnění:

- Key Vault seznam oprávnění RBAC na prostředků účtu úložiště.
- Key Vault ověří odpovědi pomocí regulárního výrazu shody akce a jiné akce. 

Najít příklady podpůrné v [Key Vault - spravované ukázky klíče účtu úložiště](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Není-li identitu *znovu vygenerovat* oprávnění nebo pokud Key Vault první strany identity nemá *seznamu* nebo *znovu vygenerovat* oprávnění a potom registrace požadavek selže, vrátí kód odpovídající chyby a zprávy. 

OBO token bude fungovat pouze při použití vlastních, aplikace nativního klienta PowerShell nebo rozhraní příkazového řádku.

## <a name="other-applications"></a>Ostatní aplikace

- Tokeny SAS, vytvářeny pomocí klíčů k účtu úložiště Key Vault, zadejte i více řízený přístup k účtu úložiště Azure. Další informace najdete v tématu [pomocí sdílené přístupové podpisy](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Viz také

- [Informace o klíčích, tajných kódech a certifikátech](https://docs.microsoft.com/rest/api/keyvault/)
- [Blog týmu trezoru klíčů](https://blogs.technet.microsoft.com/kv/)
