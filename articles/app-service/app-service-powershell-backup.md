---
title: "Pomocí prostředí PowerShell pro zálohování a obnovení aplikace služby App Service"
description: "Další informace o použití prostředí PowerShell pro zálohování a obnovení aplikace v Azure App Service"
services: app-service
documentationcenter: 
author: NKing92
manager: erikre
editor: 
ms.assetid: 7ea8661e-aefb-4823-9626-6bff980cdebf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2016
ms.author: nicking
ms.openlocfilehash: 34a7e1d025c301ca056753d964bb3c5f4f1a62d8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>Pomocí prostředí PowerShell pro zálohování a obnovení aplikace služby App Service
> [!div class="op_single_selector"]
> * [PowerShell](app-service-powershell-backup.md)
> * [REST API](../app-service-web/websites-csm-backup.md)
> 
> 

Další informace o použití prostředí Azure PowerShell k zálohování a obnovení [aplikací App Service](https://azure.microsoft.com/services/app-service/web/). Další informace o zálohování webové aplikace, včetně požadavků a omezení, najdete v části [zálohování webové aplikace ve službě Azure App Service](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Požadavky
Pomocí prostředí PowerShell pro správu zálohování vaší aplikace, budete potřebovat následující:

* **Adresa URL typu SAS** oprávnění ke čtení a zápisu do kontejneru Azure Storage, který umožňuje. V tématu [vysvětlení modelu SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md) vysvětlení SAS adresy URL. V tématu [použití Azure Powershellu s Azure Storage](../storage/common/storage-powershell-guide-full.md) příklady Správa úložiště Azure pomocí prostředí PowerShell.
* **Připojovací řetězec databáze** Pokud chcete zálohovat databázi společně s vaší webové aplikace.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Jak vygenerovat SAS adresa URL pro použití s rutinami zálohování webové aplikace
Adresa URL typu SAS můžete vygenerovat pomocí prostředí PowerShell. Tady je příklad toho, jak vygenerovat ten, který lze použít s rutiny popsané v tomto článku.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Nainstalovat Azure PowerShell 1.3.2 nebo novější
V tématu [použití Azure Powershellu s Azure Resource Manager](/powershell/azure/overview) pokyny k instalaci a použití prostředí Azure PowerShell.

## <a name="create-a-backup"></a>Vytvoření zálohy
Pomocí rutiny New-AzureRmWebAppBackup k vytvoření zálohy, webové aplikace.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Vytvoří zálohy s automaticky vygenerovaným názvem. Pokud chcete k zadání názvu vašeho zálohování, použijte název_zálohy volitelný parametr.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Databáze zahrnout do zálohování, nejprve vytvořte pomocí rutiny New-AzureRmWebAppDatabaseBackupSetting nastavení zálohování databáze a potom zadejte toto nastavení v parametru databáze rutiny New-AzureRmWebAppBackup. Parametr databáze přijímá pole nastavení databáze, díky tomu můžete zálohovat více než jednu databázi.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Získat zálohy
Rutinu Get-AzureRmWebAppBackupList vrátí pole všech záloh pro webovou aplikaci. Je třeba zadat název webové aplikace a jeho skupin prostředků.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Chcete-li získat konkrétní zálohu, použijte rutinu Get-AzureRmWebAppBackup.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Také můžete předat objekt webové aplikace do rutiny zálohování správy ke zvýšení pohodlí.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Naplánovat automatické zálohování
Můžete naplánovat zálohování provést automaticky v zadaných intervalech. Pokud chcete nakonfigurovat plán zálohování, použijte rutinu AzureRmWebAppBackupConfiguration upravit. Tato rutina přijímá několik parametrů:

* **Název** -název webové aplikace.
* **Název skupiny prostředků** -název skupiny prostředků obsahující webovou aplikaci.
* **Slot** – volitelné. Název slot webové aplikace.
* **StorageAccountUrl** – adresa URL SAS pro kontejner Azure Storage používá k ukládání záloh.
* **FrequencyInterval** – jak často má být k zálohování číselné hodnoty. Musí být kladné celé číslo.
* **FrequencyUnit** -jednotka času pro jak často má být k zálohování. Možnosti jsou hodin a dnů.
* **RetentionPeriodInDays** – jak dlouho má být uložen automatické zálohování, než bude automaticky odstraněna.
* **StartTime** – volitelné. Čas, kdy by měl začínat automatické zálohování. Zálohování začne okamžitě, pokud je null. Musí být hodnota DateTime.
* **Databáze** – volitelné. Pole DatabaseBackupSettings pro databáze pro zálohování.
* **KeepAtLeastOneBackup** – volitelné přepnout parametr. Zadejte tento Pokud jednu zálohu by měly být udržovány vždy v účtu úložiště, bez ohledu na to, kolik je.

Tady je příklad toho, jak použít tuto rutinu.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Chcete-li získat aktuální plán zálohování, použijte rutinu Get-AzureRmWebAppBackupConfiguration. To může být užitečné pro úpravy plánu, který je už nakonfigurovaný.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Obnovení ze zálohy, webové aplikace
Webovou aplikaci obnovit ze zálohy, použijte rutinu AzureRmWebAppBackup obnovení. Nejjednodušší způsob, jak použít tuto rutinu je kanál v objektu zálohy načíst z rutiny Get-AzureRmWebAppBackup nebo rutiny Get-AzureRmWebAppBackupList.

Jakmile máte objekt zálohování, zřetězit ho do rutiny AzureRmWebAppBackup obnovení. Zadejte parametr přepsat přepínače k označení, kterou chcete přepsat obsah vaší webové aplikace se obsah zálohy. Pokud záloha obsahuje databáze, jsou tyto databáze také obnovit.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Následuje příklad použití Restore-AzureRmWebAppBackup zadáním všechny parametry.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Odstranit zálohy
Pokud chcete odstranit zálohy, použijte rutinu Remove-AzureRmWebAppBackup. Tím se odeberou zálohování z vašeho účtu úložiště. Zadejte název vaší aplikace, jeho skupin prostředků a ID zálohu, kterou chcete odstranit.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Také můžete předat objekt zálohování do rutiny Remove-AzureRmWebAppBackup ho odstranit.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
