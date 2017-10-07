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
# <a name="use-powershell-tooback-up-and-restore-app-service-apps"></a><span data-ttu-id="6be3c-103">Pomocí prostředí PowerShell tooback a obnovení aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="6be3c-103">Use PowerShell tooback up and restore App Service apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6be3c-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6be3c-104">PowerShell</span></span>](app-service-powershell-backup.md)
> * [<span data-ttu-id="6be3c-105">REST API</span><span class="sxs-lookup"><span data-stu-id="6be3c-105">REST API</span></span>](../app-service-web/websites-csm-backup.md)
> 
> 

<span data-ttu-id="6be3c-106">Zjistěte, jak tooback toouse prostředí Azure PowerShell a obnovení [aplikací App Service](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="6be3c-106">Learn how toouse Azure PowerShell tooback up and restore [App Service apps](https://azure.microsoft.com/services/app-service/web/).</span></span> <span data-ttu-id="6be3c-107">Další informace o zálohování webové aplikace, včetně požadavků a omezení, najdete v části [zálohování webové aplikace ve službě Azure App Service](../app-service-web/web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="6be3c-107">For more information about web app backups, including requirements and restrictions, see [Back up a web app in Azure App Service](../app-service-web/web-sites-backup.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6be3c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6be3c-108">Prerequisites</span></span>
<span data-ttu-id="6be3c-109">prostředí PowerShell toomanage toouse aplikaci Zálohování, musíte hello následující:</span><span class="sxs-lookup"><span data-stu-id="6be3c-109">toouse PowerShell toomanage your app backups, you need hello following:</span></span>

* <span data-ttu-id="6be3c-110">**Adresa URL typu SAS** umožňuje čtení a zápis tooan Azure Storage kontejneru.</span><span class="sxs-lookup"><span data-stu-id="6be3c-110">**A SAS URL** that allows read and write access tooan Azure Storage container.</span></span> <span data-ttu-id="6be3c-111">V tématu [vysvětlení modelu SAS hello](../storage/common/storage-dotnet-shared-access-signature-part-1.md) vysvětlení SAS adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6be3c-111">See [Understanding hello SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for an explanation of SAS URLs.</span></span> <span data-ttu-id="6be3c-112">V tématu [použití Azure Powershellu s Azure Storage](../storage/common/storage-powershell-guide-full.md) příklady Správa úložiště Azure pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6be3c-112">See [Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) for examples of managing Azure Storage using PowerShell.</span></span>
* <span data-ttu-id="6be3c-113">**Připojovací řetězec databáze** Pokud chcete tooback zálohu databáze spolu s vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6be3c-113">**A database connection string** if you want tooback up a database along with your web app.</span></span>

### <a name="how-toogenerate-a-sas-url-toouse-with-hello-web-app-backup-cmdlets"></a><span data-ttu-id="6be3c-114">Jak toogenerate toouse SAS adresa URL webové aplikace hello zálohování rutiny</span><span class="sxs-lookup"><span data-stu-id="6be3c-114">How toogenerate a SAS URL toouse with hello web app backup cmdlets</span></span>
<span data-ttu-id="6be3c-115">Adresa URL typu SAS můžete vygenerovat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6be3c-115">A SAS URL can be generated with PowerShell.</span></span> <span data-ttu-id="6be3c-116">Tady je příklad, jak toogenerate ten, který lze použít s hello rutiny popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="6be3c-116">Here is an example of how toogenerate one that can be used with hello cmdlets discussed in this article.</span></span>

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure tooselect hello appropriate key. Here we select hello first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a><span data-ttu-id="6be3c-117">Nainstalovat Azure PowerShell 1.3.2 nebo novější</span><span class="sxs-lookup"><span data-stu-id="6be3c-117">Install Azure PowerShell 1.3.2 or greater</span></span>
<span data-ttu-id="6be3c-118">V tématu [použití Azure Powershellu s Azure Resource Manager](/powershell/azure/overview) pokyny k instalaci a použití prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6be3c-118">See [Using Azure PowerShell with Azure Resource Manager](/powershell/azure/overview) for instructions on installing and using Azure PowerShell.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="6be3c-119">Vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="6be3c-119">Create a backup</span></span>
<span data-ttu-id="6be3c-120">Pomocí rutiny toocreate hello New-AzureRmWebAppBackup zálohování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6be3c-120">Use hello New-AzureRmWebAppBackup cmdlet toocreate a backup of a web app.</span></span>

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

<span data-ttu-id="6be3c-121">Vytvoří zálohy s automaticky vygenerovaným názvem.</span><span class="sxs-lookup"><span data-stu-id="6be3c-121">This creates a backup with an automatically generated name.</span></span> <span data-ttu-id="6be3c-122">Pokud chcete tooprovide název vašeho zálohování, použijte hello název_zálohy volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="6be3c-122">If you would like tooprovide a name for your backup, use hello BackupName optional parameter.</span></span>

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

<span data-ttu-id="6be3c-123">tooinclude databáze v hello zálohování, nejprve vytvořte pomocí rutiny New-AzureRmWebAppDatabaseBackupSetting hello nastavení zálohování databáze a potom zadejte nastavení v hello databáze parametru rutiny New-AzureRmWebAppBackup hello.</span><span class="sxs-lookup"><span data-stu-id="6be3c-123">tooinclude a database in hello backup, first create a database backup setting using hello New-AzureRmWebAppDatabaseBackupSetting cmdlet, then supply that setting in hello Databases parameter of hello New-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="6be3c-124">parametr databáze Hello přijímá pole nastavení databáze, umožní vám tooback až více než jednu databázi.</span><span class="sxs-lookup"><span data-stu-id="6be3c-124">hello Databases parameter accepts an array of database settings, allowing you tooback up more than one database.</span></span>

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a><span data-ttu-id="6be3c-125">Získat zálohy</span><span class="sxs-lookup"><span data-stu-id="6be3c-125">Get backups</span></span>
<span data-ttu-id="6be3c-126">Rutina Get-AzureRmWebAppBackupList Hello vrátí pole všechny zálohy pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6be3c-126">hello Get-AzureRmWebAppBackupList cmdlet returns an array of all backups for a web app.</span></span> <span data-ttu-id="6be3c-127">Je třeba zadat název hello hello webové aplikace a jeho skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="6be3c-127">You must supply hello name of hello web app and its resource group.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

<span data-ttu-id="6be3c-128">tooget konkrétní zálohu, použijte rutinu Get-AzureRmWebAppBackup hello.</span><span class="sxs-lookup"><span data-stu-id="6be3c-128">tooget a specific backup, use hello Get-AzureRmWebAppBackup cmdlet.</span></span>

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

<span data-ttu-id="6be3c-129">Také můžete předat objekt webové aplikace do rutiny správu záloh hello ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="6be3c-129">You can also pipe a web app object into any of hello backup management cmdlets for convenience.</span></span>

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a><span data-ttu-id="6be3c-130">Naplánovat automatické zálohování</span><span class="sxs-lookup"><span data-stu-id="6be3c-130">Schedule automatic backups</span></span>
<span data-ttu-id="6be3c-131">Můžete naplánovat zálohování toohappen automaticky v zadaných intervalech.</span><span class="sxs-lookup"><span data-stu-id="6be3c-131">You can schedule backups toohappen automatically at a specified interval.</span></span> <span data-ttu-id="6be3c-132">tooconfigure plán zálohování pomocí rutiny hello AzureRmWebAppBackupConfiguration upravit.</span><span class="sxs-lookup"><span data-stu-id="6be3c-132">tooconfigure a backup schedule, use hello Edit-AzureRmWebAppBackupConfiguration cmdlet.</span></span> <span data-ttu-id="6be3c-133">Tato rutina přijímá několik parametrů:</span><span class="sxs-lookup"><span data-stu-id="6be3c-133">This cmdlet takes several parameters:</span></span>

* <span data-ttu-id="6be3c-134">**Název** – hello název hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6be3c-134">**Name** - hello name of hello web app.</span></span>
* <span data-ttu-id="6be3c-135">**Název skupiny prostředků** – hello název hello prostředků skupiny obsahující hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6be3c-135">**ResourceGroupName** - hello name of hello resource group containing hello web app.</span></span>
* <span data-ttu-id="6be3c-136">**Slot** – volitelné.</span><span class="sxs-lookup"><span data-stu-id="6be3c-136">**Slot** - Optional.</span></span> <span data-ttu-id="6be3c-137">Název Hello hello slot webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6be3c-137">hello name of hello web app slot.</span></span>
* <span data-ttu-id="6be3c-138">**StorageAccountUrl** -hello SAS URL pro kontejner úložiště Azure hello použít toostore hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="6be3c-138">**StorageAccountUrl** - hello SAS URL for hello Azure Storage container used toostore hello backups.</span></span>
* <span data-ttu-id="6be3c-139">**FrequencyInterval** – jak často hello zálohování by měl být provedeny číselné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6be3c-139">**FrequencyInterval** - Numeric value for how often hello backups should be made.</span></span> <span data-ttu-id="6be3c-140">Musí být kladné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="6be3c-140">Must be a positive integer.</span></span>
* <span data-ttu-id="6be3c-141">**FrequencyUnit** -jednotka času pro jak často hello zálohování by měl být provedeny.</span><span class="sxs-lookup"><span data-stu-id="6be3c-141">**FrequencyUnit** - Unit of time for how often hello backups should be made.</span></span> <span data-ttu-id="6be3c-142">Možnosti jsou hodin a dnů.</span><span class="sxs-lookup"><span data-stu-id="6be3c-142">Options are Hour and Day.</span></span>
* <span data-ttu-id="6be3c-143">**RetentionPeriodInDays** – jak dlouho má být uložen hello automatické zálohování, než bude automaticky odstraněna.</span><span class="sxs-lookup"><span data-stu-id="6be3c-143">**RetentionPeriodInDays** - How many days hello automatic backups should be saved before being automatically deleted.</span></span>
* <span data-ttu-id="6be3c-144">**StartTime** – volitelné.</span><span class="sxs-lookup"><span data-stu-id="6be3c-144">**StartTime** - Optional.</span></span> <span data-ttu-id="6be3c-145">Hello čas spuštění hello automatické zálohování.</span><span class="sxs-lookup"><span data-stu-id="6be3c-145">hello time when hello automatic backups should begin.</span></span> <span data-ttu-id="6be3c-146">Zálohování začne okamžitě, pokud je null.</span><span class="sxs-lookup"><span data-stu-id="6be3c-146">Backups begin immediately if this is null.</span></span> <span data-ttu-id="6be3c-147">Musí být hodnota DateTime.</span><span class="sxs-lookup"><span data-stu-id="6be3c-147">Must be a DateTime.</span></span>
* <span data-ttu-id="6be3c-148">**Databáze** – volitelné.</span><span class="sxs-lookup"><span data-stu-id="6be3c-148">**Databases** - Optional.</span></span> <span data-ttu-id="6be3c-149">Pole DatabaseBackupSettings pro toobackup hello databáze.</span><span class="sxs-lookup"><span data-stu-id="6be3c-149">An array of DatabaseBackupSettings for hello databases toobackup.</span></span>
* <span data-ttu-id="6be3c-150">**KeepAtLeastOneBackup** – volitelné přepnout parametr.</span><span class="sxs-lookup"><span data-stu-id="6be3c-150">**KeepAtLeastOneBackup** - Optional switched parameter.</span></span> <span data-ttu-id="6be3c-151">Zadejte toto Pokud jednu zálohu by měly být udržovány vždy hello účet úložiště, bez ohledu na to stáří je.</span><span class="sxs-lookup"><span data-stu-id="6be3c-151">Supply this if one backup should always be kept in hello storage account, regardless of how old it is.</span></span>

<span data-ttu-id="6be3c-152">Tady je příklad toho, jak toouse tuto rutinu.</span><span class="sxs-lookup"><span data-stu-id="6be3c-152">Following is an example of how toouse this cmdlet.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

<span data-ttu-id="6be3c-153">tooget hello aktuální plán zálohování, použijte rutinu Get-AzureRmWebAppBackupConfiguration hello.</span><span class="sxs-lookup"><span data-stu-id="6be3c-153">tooget hello current backup schedule, use hello Get-AzureRmWebAppBackupConfiguration cmdlet.</span></span> <span data-ttu-id="6be3c-154">To může být užitečné pro úpravy plánu, který je už nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="6be3c-154">This can be useful for modifying a schedule that has already been configured.</span></span>

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify hello configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply hello new configuration by piping it into hello Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a><span data-ttu-id="6be3c-155">Obnovení ze zálohy, webové aplikace</span><span class="sxs-lookup"><span data-stu-id="6be3c-155">Restore a web app from a backup</span></span>
<span data-ttu-id="6be3c-156">toorestore webové aplikace ze zálohy, použijte rutinu hello AzureRmWebAppBackup obnovení.</span><span class="sxs-lookup"><span data-stu-id="6be3c-156">toorestore a web app from a backup, use hello Restore-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="6be3c-157">Nejjednodušší způsob, jak toouse Hello Tato rutina je toopipe v zálohování objektu načíst z rutiny Get-AzureRmWebAppBackup hello nebo rutiny Get-AzureRmWebAppBackupList.</span><span class="sxs-lookup"><span data-stu-id="6be3c-157">hello easiest way toouse this cmdlet is toopipe in a backup object retrieved from hello Get-AzureRmWebAppBackup cmdlet or Get-AzureRmWebAppBackupList cmdlet.</span></span>

<span data-ttu-id="6be3c-158">Jakmile máte objekt zálohování, zřetězit ho do rutiny obnovení AzureRmWebAppBackup hello.</span><span class="sxs-lookup"><span data-stu-id="6be3c-158">Once you have a backup object, you can pipe it into hello Restore-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="6be3c-159">Zadejte hello přepsat přepínač parametr tooindicate, že máte v úmyslu toooverwrite hello obsah vaší webové aplikace s hello obsah hello zálohování.</span><span class="sxs-lookup"><span data-stu-id="6be3c-159">Specify hello Overwrite switch parameter tooindicate that you intend toooverwrite hello contents of your web app with hello contents of hello backup.</span></span> <span data-ttu-id="6be3c-160">Obsahuje-li hello zálohování databáze, jsou tyto databáze také obnovit.</span><span class="sxs-lookup"><span data-stu-id="6be3c-160">If hello backup contains databases, those databases are restored as well.</span></span>

        $backup | Restore-AzureRmWebAppBackup -Overwrite

<span data-ttu-id="6be3c-161">Následuje příklad jak toouse hello AzureRmWebAppBackup obnovení tak, že zadáte všechny parametry hello.</span><span class="sxs-lookup"><span data-stu-id="6be3c-161">Following is an example of how toouse hello Restore-AzureRmWebAppBackup by specifying all hello parameters.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a><span data-ttu-id="6be3c-162">Odstranit zálohy</span><span class="sxs-lookup"><span data-stu-id="6be3c-162">Delete a backup</span></span>
<span data-ttu-id="6be3c-163">toodelete zálohu, použijte rutinu Remove-AzureRmWebAppBackup hello.</span><span class="sxs-lookup"><span data-stu-id="6be3c-163">toodelete a backup, use hello Remove-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="6be3c-164">Tím se odeberou hello zálohování z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6be3c-164">This removes hello backup from your storage account.</span></span> <span data-ttu-id="6be3c-165">Zadejte název aplikace, jeho skupin prostředků a hello ID hello zálohování chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="6be3c-165">Specify your app name, its resource group, and hello ID of hello backup you want toodelete.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

<span data-ttu-id="6be3c-166">Také můžete předat objekt zálohování do rutiny toodelete hello AzureRmWebAppBackup odebrat ji.</span><span class="sxs-lookup"><span data-stu-id="6be3c-166">You can also pipe a backup object into hello Remove-AzureRmWebAppBackup cmdlet toodelete it.</span></span>

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
