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
# <a name="azure-key-vault-storage-account-keys"></a>Klíče účtu úložiště Azure Key Vault

Před klíčů k účtu Azure Key Vault úložiště vývojáři měla toomanage vlastní klíče účtu úložiště Azure (ASA) a jejich otočení ručně nebo pomocí externího automatizace. Nyní, klíče účtu úložiště klíč trezoru jsou implementované jako [tajné klíče Key Vault](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) pro ověřování pomocí účtu úložiště Azure. 

klíčové funkce Hello ASA spravuje tajný otočení pro vás a odebere hello potřebu přímém kontaktu s klíčem ASA prostřednictvím nabídky sdíleného přístupu podpisy (SAS) jako metodu. 

Další obecné informace o účtech úložiště Azure najdete v tématu [účty Azure storage](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Podpora rozhraní

Hello funkce klíče účtu úložiště Azure je původně k dispozici prostřednictvím hello REST, .NET / C# a prostředí PowerShell rozhraní. Další informace najdete v tématu [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).


## <a name="storage-account-keys-behavior"></a>Chování klíče účtu úložiště

### <a name="what-key-vault-manages"></a>Co spravuje Key Vault

Při použití klíče účtu úložiště Key Vault provede několik funkcí správy interní vaším jménem.

1. Azure Key Vault spravuje klíče z Azure Storage účtu (ASA). 
    - Azure Key Vault interně, můžete seznam klíčů (sync) s účtem úložiště Azure.  
    - Azure Key Vault regeneruje (otočí) hello klíče pravidelně. 
    - V odpovědi toocaller jsou nikdy vrátil hodnoty klíče. 
    - Azure Key Vault spravuje klíče účtů úložiště a klasické účty úložiště. 
2. Azure Key Vault umožňuje můžete hello trezoru nebo objektu vlastníka, definice toocreate SAS (účet nebo SAS služby). 
    - Hello hodnotu SAS, vytvořené pomocí definice SAS, se vrátí jako tajný klíč prostřednictvím cesta REST URI hello. Další informace najdete v tématu [operace účet úložiště Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

### <a name="naming-guidance"></a>Pokyny pro pojmenování

Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.  
 
Název definice SAS musí být 1-102 znaků obsahující pouze 0 – 9,-z, A-Z.

## <a name="developer-experience"></a>Možnosti pro vývojáře 

### <a name="before-azure-key-vault-storage-keys"></a>Před klíče úložiště Azure Key Vault 

Toodo hello tooneed vývojáři použít následující postupy s úložiště tooAzure přístup tooget klíče účtu úložiště. 
 
 ```
//create storage account using connection string containing account name 
// and hello storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>Po klíče úložiště Azure Key Vault 

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
 
 ### <a name="developer-best-practices"></a>Doporučené postupy 

- Key Vault toomanage povolte jenom vaše ASA klíče. Nepokoušejte toomanage je sami, koliduje s procesy Key Vault. 
- Nepovolit toobe klíče ASA spravuje více než jeden objekt Key Vault. 
- Pokud potřebujete toomanually znovu vygenerovat klíče ASA, doporučujeme vám obnovit prostřednictvím Key Vault. 

## <a name="getting-started"></a>Začínáme

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Instalační program pro oprávnění k přístupu na základě rolí k řízení (RBAC)

Key Vault potřebuje oprávnění příliš*seznamu* a *znovu vygenerovat* klíče pro účet úložiště. Nastavení těchto oprávnění pomocí hello následující kroky:

- Získejte ObjectId trezoru klíčů: 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- Přiřaďte role tooAzure operátor klíč úložiště klíč trezoru Identity: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > Pro typ účtu classic, nastavte parametr role hello příliš*"Classic úložiště účet klíč služby Role operátora"*.

### <a name="storage-account-onboarding"></a>Registrace účtu úložiště 

Příklad: Jako vlastník objektu Key Vault přidat úložiště účet objekt tooyour Azure Key Vault tooonboard účet úložiště.

Během registrace, Key Vault musí tooverify hello identitu účtu registrace hello příliš má oprávnění*seznamu* a příliš*znovu vygenerovat* klíče úložiště. V pořadí tooverify tato oprávnění, Key Vault získá OBO (na Behalf Of) token ze služby ověřování hello, cílová skupina nastavit tooAzure Resource Manager a umožňuje *seznamu* klíče služby Azure Storage toohello volání. Pokud hello *seznamu* volání selže, hello Key Vault vytvoření objektu selže s kódem stavu HTTP z *zakázáno*. s úložištěm entity váš trezor klíčů v mezipaměti klíče Hello uvedené tímto způsobem. 

Key Vault musí ověřte, zda text hello identity *znovu vygenerovat* oprávnění předtím, než ji může převzít vlastnictví opětovné generování klíče. tooverify, který hello identitu prostřednictvím tokenu OBO, jakož i hello Key Vault první strany identita má tato oprávnění:

- Key Vault seznam oprávnění RBAC na prostředku účet úložiště hello.
- Key Vault ověří hello odpovědi odpovídající regulárnímu výrazu akcí a jiné akce. 

Najít příklady podpůrné v [Key Vault - spravované ukázky klíče účtu úložiště](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Pokud nemá hello identity *znovu vygenerovat* oprávnění nebo pokud Key Vault první strany identity nemá *seznamu* nebo *znovu vygenerovat* oprávnění a potom hello registrace požadavek selže, vrátí kód odpovídající chyby a zprávy. 

Hello OBO token bude fungovat pouze při použití vlastních, aplikace nativního klienta PowerShell nebo rozhraní příkazového řádku.

## <a name="other-applications"></a>Ostatní aplikace

- Tokeny SAS, vytvářeny pomocí klíčů k účtu úložiště Key Vault, zadejte i další tooan řízený přístup účtu úložiště Azure. Další informace najdete v tématu [pomocí sdílené přístupové podpisy](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Viz také

- [Informace o klíčích, tajných kódech a certifikátech](https://docs.microsoft.com/rest/api/keyvault/)
- [Blog týmu trezoru klíčů](https://blogs.technet.microsoft.com/kv/)
