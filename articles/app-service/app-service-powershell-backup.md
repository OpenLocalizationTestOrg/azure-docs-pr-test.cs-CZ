---
title: "tooback aaaUse prostředí PowerShell a obnovení aplikace služby App Service"
description: "Zjistěte, jak tooback toouse prostředí PowerShell a obnovení aplikace v Azure App Service"
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
ms.openlocfilehash: 4042166f6c650841926f010056d6c80ab2de57e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooback-up-and-restore-app-service-apps"></a>Pomocí prostředí PowerShell tooback a obnovení aplikace služby App Service
> [!div class="op_single_selector"]
> * [PowerShell](app-service-powershell-backup.md)
> * [REST API](../app-service-web/websites-csm-backup.md)
> 
> 

Zjistěte, jak tooback toouse prostředí Azure PowerShell a obnovení [aplikací App Service](https://azure.microsoft.com/services/app-service/web/). Další informace o zálohování webové aplikace, včetně požadavků a omezení, najdete v části [zálohování webové aplikace ve službě Azure App Service](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Požadavky
prostředí PowerShell toomanage toouse aplikaci Zálohování, musíte hello následující:

* **Adresa URL typu SAS** umožňuje čtení a zápis tooan Azure Storage kontejneru. V tématu [vysvětlení modelu SAS hello](../storage/common/storage-dotnet-shared-access-signature-part-1.md) vysvětlení SAS adresy URL. V tématu [použití Azure Powershellu s Azure Storage](../storage/common/storage-powershell-guide-full.md) příklady Správa úložiště Azure pomocí prostředí PowerShell.
* **Připojovací řetězec databáze** Pokud chcete tooback zálohu databáze spolu s vaší webové aplikace.

### <a name="how-toogenerate-a-sas-url-toouse-with-hello-web-app-backup-cmdlets"></a>Jak toogenerate toouse SAS adresa URL webové aplikace hello zálohování rutiny
Adresa URL typu SAS můžete vygenerovat pomocí prostředí PowerShell. Tady je příklad, jak toogenerate ten, který lze použít s hello rutiny popsané v tomto článku.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure tooselect hello appropriate key. Here we select hello first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Nainstalovat Azure PowerShell 1.3.2 nebo novější
V tématu [použití Azure Powershellu s Azure Resource Manager](/powershell/azure/overview) pokyny k instalaci a použití prostředí Azure PowerShell.

## <a name="create-a-backup"></a>Vytvoření zálohy
Pomocí rutiny toocreate hello New-AzureRmWebAppBackup zálohování webové aplikace.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Vytvoří zálohy s automaticky vygenerovaným názvem. Pokud chcete tooprovide název vašeho zálohování, použijte hello název_zálohy volitelný parametr.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

tooinclude databáze v hello zálohování, nejprve vytvořte pomocí rutiny New-AzureRmWebAppDatabaseBackupSetting hello nastavení zálohování databáze a potom zadejte nastavení v hello databáze parametru rutiny New-AzureRmWebAppBackup hello. parametr databáze Hello přijímá pole nastavení databáze, umožní vám tooback až více než jednu databázi.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Získat zálohy
Rutina Get-AzureRmWebAppBackupList Hello vrátí pole všechny zálohy pro webovou aplikaci. Je třeba zadat název hello hello webové aplikace a jeho skupin prostředků.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

tooget konkrétní zálohu, použijte rutinu Get-AzureRmWebAppBackup hello.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Také můžete předat objekt webové aplikace do rutiny správu záloh hello ke zvýšení pohodlí.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Naplánovat automatické zálohování
Můžete naplánovat zálohování toohappen automaticky v zadaných intervalech. tooconfigure plán zálohování pomocí rutiny hello AzureRmWebAppBackupConfiguration upravit. Tato rutina přijímá několik parametrů:

* **Název** – hello název hello webové aplikace.
* **Název skupiny prostředků** – hello název hello prostředků skupiny obsahující hello webové aplikace.
* **Slot** – volitelné. Název Hello hello slot webové aplikace.
* **StorageAccountUrl** -hello SAS URL pro kontejner úložiště Azure hello použít toostore hello zálohy.
* **FrequencyInterval** – jak často hello zálohování by měl být provedeny číselné hodnoty. Musí být kladné celé číslo.
* **FrequencyUnit** -jednotka času pro jak často hello zálohování by měl být provedeny. Možnosti jsou hodin a dnů.
* **RetentionPeriodInDays** – jak dlouho má být uložen hello automatické zálohování, než bude automaticky odstraněna.
* **StartTime** – volitelné. Hello čas spuštění hello automatické zálohování. Zálohování začne okamžitě, pokud je null. Musí být hodnota DateTime.
* **Databáze** – volitelné. Pole DatabaseBackupSettings pro toobackup hello databáze.
* **KeepAtLeastOneBackup** – volitelné přepnout parametr. Zadejte toto Pokud jednu zálohu by měly být udržovány vždy hello účet úložiště, bez ohledu na to stáří je.

Tady je příklad toho, jak toouse tuto rutinu.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

tooget hello aktuální plán zálohování, použijte rutinu Get-AzureRmWebAppBackupConfiguration hello. To může být užitečné pro úpravy plánu, který je už nakonfigurovaný.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify hello configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply hello new configuration by piping it into hello Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Obnovení ze zálohy, webové aplikace
toorestore webové aplikace ze zálohy, použijte rutinu hello AzureRmWebAppBackup obnovení. Nejjednodušší způsob, jak toouse Hello Tato rutina je toopipe v zálohování objektu načíst z rutiny Get-AzureRmWebAppBackup hello nebo rutiny Get-AzureRmWebAppBackupList.

Jakmile máte objekt zálohování, zřetězit ho do rutiny obnovení AzureRmWebAppBackup hello. Zadejte hello přepsat přepínač parametr tooindicate, že máte v úmyslu toooverwrite hello obsah vaší webové aplikace s hello obsah hello zálohování. Obsahuje-li hello zálohování databáze, jsou tyto databáze také obnovit.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Následuje příklad jak toouse hello AzureRmWebAppBackup obnovení tak, že zadáte všechny parametry hello.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Odstranit zálohy
toodelete zálohu, použijte rutinu Remove-AzureRmWebAppBackup hello. Tím se odeberou hello zálohování z vašeho účtu úložiště. Zadejte název aplikace, jeho skupin prostředků a hello ID hello zálohování chcete toodelete.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Také můžete předat objekt zálohování do rutiny toodelete hello AzureRmWebAppBackup odebrat ji.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
