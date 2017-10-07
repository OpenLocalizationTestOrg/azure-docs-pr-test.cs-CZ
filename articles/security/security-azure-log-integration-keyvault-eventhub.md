---
title: "protokoly aaaIntegrate z Azure Key Vault pomocí služby Event Hubs | Microsoft Docs"
description: "Kurz, který poskytuje hello potřebné kroky toomake Key Vault protokolů k dispozici tooa SIEM s využitím protokolu integrace se službou Azure"
services: security
author: barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.topic: article
ms.date: 08/07/2017
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: ada2fc846cc6bf09e12cc2c016815b27afef0d50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a>Kurz pro Azure integrace protokolu: proces Azure Key Vault událostí pomocí služby Event Hubs

Můžete použít protokol integrace se službou Azure tooretrieve protokolují události a systém je k dispozici tooyour zabezpečení informací a událostí správy (SIEM). Tento kurz ukazuje příklad jak integrace se službou Azure protokolu lze použít tooprocess protokoly, které jsou získány prostřednictvím Azure Event Hubs.
 
Použijte tento kurz tooget seznámen se jak integrace se službou protokolu Azure a Event Hubs pracovní společně podle kroků hello příklady kroků a pochopení, jak každý krok podporuje hello řešení. Potom můžete provést co když jste se naučili v tomto poli toocreate vlastní toosupport kroky jedinečným požadavkům vaší společnosti.

>[!WARNING]
Hello kroky a příkazy v tomto kurzu nejsou určený toobe zkopírovat a vložit. Jsou jenom příklady. Nepoužívejte příkazy prostředí PowerShell hello "tak, jak je ve vašem prostředí za provozu. Je nutné přizpůsobit podle vašeho prostředí jedinečné.


Tento kurz vás provede procesem hello trvá centra událostí zaznamenané tooan aktivity Azure Key Vault a zpřístupnění jako systém SIEM tooyour soubory JSON. Pak můžete nakonfigurovat souborů JSON hello tooprocess systému SIEM.

>[!NOTE]
>Většina hello kroky v tomto kurzu je konfigurace trezorů klíčů, účty úložiště a event hubs. konkrétní kroky integrace se službou Azure protokolu Hello jsou na hello konce tohoto kurzu. Neprovádějte kroky v produkčním prostředí. Ty jsou určené pro prostředí laboratoře. Je nutné přizpůsobit hello kroky před jejich používání v produkčním prostředí.

Zadané informace společně pomáhá hello způsobem, že rozumíte hello důvodech každý krok. Odkazy tooother články poskytují další podrobnosti na určité témata.

Další informace o hello služby, které uvádí v tomto kurzu najdete v části: 

- [Azure Key Vault](../key-vault/key-vault-whatis.md)
- [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Integrace protokolů Azure](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a>Počáteční nastavení

Před dokončením hello kroky v tomto článku, je třeba hello následující:

1. Předplatné Azure a účet v tomto předplatném s právy správce. Pokud nemáte předplatné, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/).
 
2. Systém s toohello přístup k Internetu, který splňuje požadavky hello instalaci protokolu integrace se službou Azure. Hello systém může být v cloudové službě nebo hostovaný místně.

3. [Integrace se službou Azure protokolu](https://www.microsoft.com/download/details.aspx?id=53324) nainstalována. tooinstall ho:

   a. Používejte systém toohello tooconnect vzdálené plochy je uveden v kroku 2.   
   b. Zkopírujte hello integrace se službou Azure protokolu instalačního programu toohello systému. Můžete [stáhnout instalační soubory hello](https://www.microsoft.com/download/details.aspx?id=53324).   
   c. Spusťte instalační program hello a přijměte licenční podmínky softwaru společnosti Microsoft hello.   
   d. Pokud bude poskytovat informace telemetrie, ponechejte zaškrtnuté zaškrtávací políčko hello. Pokud místo není odesílali tooMicrosoft informace o využití, zrušte zaškrtnutí políčka hello.
   
   Další informace o protokolu integrace se službou Azure a jak tooinstall, najdete v části [integrace protokolu Azure s Azure Diagnostics protokolování a předávání událostí systému Windows](security-azure-log-integration-get-started.md).

4. Hello nejnovější verze prostředí PowerShell.
 
   Pokud máte nainstalovaný Windows Server 2016, pak máte alespoň PowerShell 5.0. Pokud používáte jinou verzi systému Windows Server, bude pravděpodobně starší verzi prostředí PowerShell nainstalovaný. Můžete zkontrolovat hello verze zadáním ```get-host``` v okně prostředí PowerShell. Pokud nemáte nainstalovaný PowerShell 5.0, můžete [stažení](https://www.microsoft.com/download/details.aspx?id=50395).

   Až budete mít alespoň PowerShell 5.0, abyste mohli pokračovat tooinstall hello nejnovější verzi:
   
   a. V okně prostředí PowerShell, zadejte hello ```Install-Module Azure``` příkaz. Kroky instalace hello.    
   b. Zadejte hello ```Install-Module AzureRM``` příkaz. Kroky instalace hello.

   Další informace najdete v tématu [nainstalovat Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).


## <a name="create-supporting-infrastructure-elements"></a>Vytváření podpůrné prvků infrastruktury

1. Otevřete okno prostředí PowerShell se zvýšenými oprávněními a přejděte příliš**C:\Program Files\Microsoft Azure protokolu integrace**.
2. Importujte rutiny AzLog hello spuštěním skriptu hello LoadAzLogModule.ps1. Zadejte hello `.\LoadAzLogModule.ps1` příkaz. (Všimněte si hello ". \" v tomto příkazu.) Mělo by se vám zobrazit přibližně toto:</br>

   ![Seznam načtených modulů](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. Zadejte hello `Login-AzureRmAccount` příkaz. V okně přihlášení hello zadejte přihlašovací údaje hello hello odběru, který budete používat pro účely tohoto kurzu.

   >[!NOTE]
   >Pokud je to hello poprvé, který jste přihlášení tooAzure z tohoto počítače, zobrazí se zpráva o povolení data o využití Microsoft toocollect prostředí PowerShell. Doporučujeme povolit tuto kolekci dat, protože je použité tooimprove prostředí Azure PowerShell.

4. Po úspěšném ověření jste přihlášeni a zobrazí informace o hello v hello následující snímek obrazovky. Poznamenejte si název odběru ID a předplatné hello, protože je budete potřebovat později toocomplete kroky.

   ![Okno prostředí PowerShell](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. Vytvořte proměnné toostore hodnoty, které se použijí později. Zadejte všechny hello následující řádky prostředí PowerShell. Můžete potřebovat tooadjust hello hodnoty toomatch prostředí.
    - ```$subscriptionName = ‘Visual Studio Ultimate with MSDN’```(Název odběru mohou lišit. Zobrazí se jako součást hello výstup hello předchozí příkaz.)
    - ```$location = 'West US'```(Tato proměnná bude použité toopass hello umístění, kde by měl být vytvořen prostředky. Můžete změnit této proměnné toobe libovolného umístění podle vaší volby.)
    - ```$random = Get-Random```
    - ``` $name = 'azlogtest' + $random```(můžete použít jakýkoli název hello, ale měl by obsahovat pouze malá písmena a číslice.)
    - ``` $storageName = $name```(Tato proměnná se použije pro název účtu úložiště hello.)
    - ```$rgname = $name ```(Tato proměnná se použije pro název skupiny prostředků hello.)
    - ``` $eventHubNameSpaceName = $name```(To je hello název oboru názvů centra událostí hello.)
6. Zadejte hello předplatné, které budete pracovat s:
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. Vytvořte skupinu prostředků:
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   Pokud zadáte `$rg` v tomto okamžiku byste měli vidět výstup podobný toothis snímek obrazovky:

   ![Výstup po vytvoření skupiny prostředků](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. Vytvořte účet úložiště, které budou použité tookeep zaznamenávat informace o stavu:
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. Vytvoření oboru názvů centra událostí hello. Toto je požadovaná toocreate centra událostí.
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. Získání ID hello pravidlo, které budou použity s hello Statistika zprostředkovatele:
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. Získat všechny možné Azure umístění a hello názvy tooa proměnné, které je možné přidat do pozdějšího kroku:
    
    a. ```$locationObjects = Get-AzureRMLocation```    
    b. ```$locations = @('global') + $locationobjects.location```
    
    Pokud zadáte `$locations` v tomto okamžiku zobrazí hello umístění názvy bez hello vrácený Get-AzureRmLocation dalších informací.
12. Vytvoření profilu protokolu správce Azure Resource Manager: 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    Další informace o hello profil protokolů Azure najdete v tématu [přehled hello protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

> [!NOTE]
> Při pokusu o toocreate profil protokolu, může se zobrazí chybové hlášení. Potom můžete zkontrolovat hello dokumentace pro Get-AzureRmLogProfile a AzureRmLogProfile odebrat. Pokud spustíte Get-AzureRmLogProfile, zobrazí informace o profilu protokolu hello. Odstraněním hello stávající profil protokolu zadáním hello ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` příkaz.
>
>![Správce prostředků profilu došlo k chybě](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a>Vytvořte trezor klíčů

1. Vytvoření trezoru klíčů hello:

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. Konfigurace protokolování pro trezor klíčů hello:

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a>Generovat aktivitu protokolu

Požadavky nutné toobe odeslané tooKey aktivity protokolu toogenerate trezoru. Akce, jako generování klíčů, ukládání tajné klíče, nebo čtení tajných klíčů z Key Vault vytvoří položky protokolu.

1. Zobrazí aktuální klíče úložiště hello:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. Generovat nové **key2**:
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. Nezobrazovat hello klíče, zjistíte, že **key2** obsahuje jiné hodnoty:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. Nastavte a číst tajný toogenerate položky další protokolu:
    
   a. ```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b. ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Vrátí tajné](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a>Konfigurace integrace protokolů Azure

Teď, když jste nakonfigurovali všechny hello požadované prvky toohave protokolování v Key Vault tooan centra událostí, je třeba tooconfigure integrace se službou Azure protokolu:

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

Spusťte příkaz hello AzLog pro každé Centrum událostí:

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

Za minutu spuštěných hello poslední dva příkazy měli byste vidět generován soubory JSON. Je můžete potvrdit, že sledováním hello directory **C:\users\AzLog\EventHubJson**.

## <a name="next-steps"></a>Další kroky

- [Integrace Azure protokolu – nejčastější dotazy](security-azure-log-integration-faq.md)
- [Začínáme s Azure protokolu integrace](security-azure-log-integration-get-started.md)
- [Protokoly z prostředků Azure integrovat do vašeho systému SIEM systémy](security-azure-log-integration-overview.md)
