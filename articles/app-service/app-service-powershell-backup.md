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
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a><span data-ttu-id="090a0-103">Pomocí prostředí PowerShell pro zálohování a obnovení aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="090a0-103">Use PowerShell to back up and restore App Service apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="090a0-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="090a0-104">PowerShell</span></span>](app-service-powershell-backup.md)
> * [<span data-ttu-id="090a0-105">REST API</span><span class="sxs-lookup"><span data-stu-id="090a0-105">REST API</span></span>](../app-service-web/websites-csm-backup.md)
> 
> 

<span data-ttu-id="090a0-106">Další informace o použití prostředí Azure PowerShell k zálohování a obnovení [aplikací App Service](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="090a0-106">Learn how to use Azure PowerShell to back up and restore [App Service apps](https://azure.microsoft.com/services/app-service/web/).</span></span> <span data-ttu-id="090a0-107">Další informace o zálohování webové aplikace, včetně požadavků a omezení, najdete v části [zálohování webové aplikace ve službě Azure App Service](../app-service-web/web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="090a0-107">For more information about web app backups, including requirements and restrictions, see [Back up a web app in Azure App Service](../app-service-web/web-sites-backup.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="090a0-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="090a0-108">Prerequisites</span></span>
<span data-ttu-id="090a0-109">Pomocí prostředí PowerShell pro správu zálohování vaší aplikace, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="090a0-109">To use PowerShell to manage your app backups, you need the following:</span></span>

* <span data-ttu-id="090a0-110">**Adresa URL typu SAS** oprávnění ke čtení a zápisu do kontejneru Azure Storage, který umožňuje.</span><span class="sxs-lookup"><span data-stu-id="090a0-110">**A SAS URL** that allows read and write access to an Azure Storage container.</span></span> <span data-ttu-id="090a0-111">V tématu [vysvětlení modelu SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md) vysvětlení SAS adresy URL.</span><span class="sxs-lookup"><span data-stu-id="090a0-111">See [Understanding the SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for an explanation of SAS URLs.</span></span> <span data-ttu-id="090a0-112">V tématu [použití Azure Powershellu s Azure Storage](../storage/common/storage-powershell-guide-full.md) příklady Správa úložiště Azure pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="090a0-112">See [Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) for examples of managing Azure Storage using PowerShell.</span></span>
* <span data-ttu-id="090a0-113">**Připojovací řetězec databáze** Pokud chcete zálohovat databázi společně s vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="090a0-113">**A database connection string** if you want to back up a database along with your web app.</span></span>

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a><span data-ttu-id="090a0-114">Jak vygenerovat SAS adresa URL pro použití s rutinami zálohování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="090a0-114">How to generate a SAS URL to use with the web app backup cmdlets</span></span>
<span data-ttu-id="090a0-115">Adresa URL typu SAS můžete vygenerovat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="090a0-115">A SAS URL can be generated with PowerShell.</span></span> <span data-ttu-id="090a0-116">Tady je příklad toho, jak vygenerovat ten, který lze použít s rutiny popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="090a0-116">Here is an example of how to generate one that can be used with the cmdlets discussed in this article.</span></span>

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a><span data-ttu-id="090a0-117">Nainstalovat Azure PowerShell 1.3.2 nebo novější</span><span class="sxs-lookup"><span data-stu-id="090a0-117">Install Azure PowerShell 1.3.2 or greater</span></span>
<span data-ttu-id="090a0-118">V tématu [použití Azure Powershellu s Azure Resource Manager](/powershell/azure/overview) pokyny k instalaci a použití prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="090a0-118">See [Using Azure PowerShell with Azure Resource Manager](/powershell/azure/overview) for instructions on installing and using Azure PowerShell.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="090a0-119">Vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="090a0-119">Create a backup</span></span>
<span data-ttu-id="090a0-120">Pomocí rutiny New-AzureRmWebAppBackup k vytvoření zálohy, webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="090a0-120">Use the New-AzureRmWebAppBackup cmdlet to create a backup of a web app.</span></span>

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

<span data-ttu-id="090a0-121">Vytvoří zálohy s automaticky vygenerovaným názvem.</span><span class="sxs-lookup"><span data-stu-id="090a0-121">This creates a backup with an automatically generated name.</span></span> <span data-ttu-id="090a0-122">Pokud chcete k zadání názvu vašeho zálohování, použijte název_zálohy volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="090a0-122">If you would like to provide a name for your backup, use the BackupName optional parameter.</span></span>

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

<span data-ttu-id="090a0-123">Databáze zahrnout do zálohování, nejprve vytvořte pomocí rutiny New-AzureRmWebAppDatabaseBackupSetting nastavení zálohování databáze a potom zadejte toto nastavení v parametru databáze rutiny New-AzureRmWebAppBackup.</span><span class="sxs-lookup"><span data-stu-id="090a0-123">To include a database in the backup, first create a database backup setting using the New-AzureRmWebAppDatabaseBackupSetting cmdlet, then supply that setting in the Databases parameter of the New-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="090a0-124">Parametr databáze přijímá pole nastavení databáze, díky tomu můžete zálohovat více než jednu databázi.</span><span class="sxs-lookup"><span data-stu-id="090a0-124">The Databases parameter accepts an array of database settings, allowing you to back up more than one database.</span></span>

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a><span data-ttu-id="090a0-125">Získat zálohy</span><span class="sxs-lookup"><span data-stu-id="090a0-125">Get backups</span></span>
<span data-ttu-id="090a0-126">Rutinu Get-AzureRmWebAppBackupList vrátí pole všech záloh pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="090a0-126">The Get-AzureRmWebAppBackupList cmdlet returns an array of all backups for a web app.</span></span> <span data-ttu-id="090a0-127">Je třeba zadat název webové aplikace a jeho skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="090a0-127">You must supply the name of the web app and its resource group.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

<span data-ttu-id="090a0-128">Chcete-li získat konkrétní zálohu, použijte rutinu Get-AzureRmWebAppBackup.</span><span class="sxs-lookup"><span data-stu-id="090a0-128">To get a specific backup, use the Get-AzureRmWebAppBackup cmdlet.</span></span>

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

<span data-ttu-id="090a0-129">Také můžete předat objekt webové aplikace do rutiny zálohování správy ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="090a0-129">You can also pipe a web app object into any of the backup management cmdlets for convenience.</span></span>

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a><span data-ttu-id="090a0-130">Naplánovat automatické zálohování</span><span class="sxs-lookup"><span data-stu-id="090a0-130">Schedule automatic backups</span></span>
<span data-ttu-id="090a0-131">Můžete naplánovat zálohování provést automaticky v zadaných intervalech.</span><span class="sxs-lookup"><span data-stu-id="090a0-131">You can schedule backups to happen automatically at a specified interval.</span></span> <span data-ttu-id="090a0-132">Pokud chcete nakonfigurovat plán zálohování, použijte rutinu AzureRmWebAppBackupConfiguration upravit.</span><span class="sxs-lookup"><span data-stu-id="090a0-132">To configure a backup schedule, use the Edit-AzureRmWebAppBackupConfiguration cmdlet.</span></span> <span data-ttu-id="090a0-133">Tato rutina přijímá několik parametrů:</span><span class="sxs-lookup"><span data-stu-id="090a0-133">This cmdlet takes several parameters:</span></span>

* <span data-ttu-id="090a0-134">**Název** -název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="090a0-134">**Name** - The name of the web app.</span></span>
* <span data-ttu-id="090a0-135">**Název skupiny prostředků** -název skupiny prostředků obsahující webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="090a0-135">**ResourceGroupName** - The name of the resource group containing the web app.</span></span>
* <span data-ttu-id="090a0-136">**Slot** – volitelné.</span><span class="sxs-lookup"><span data-stu-id="090a0-136">**Slot** - Optional.</span></span> <span data-ttu-id="090a0-137">Název slot webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="090a0-137">The name of the web app slot.</span></span>
* <span data-ttu-id="090a0-138">**StorageAccountUrl** – adresa URL SAS pro kontejner Azure Storage používá k ukládání záloh.</span><span class="sxs-lookup"><span data-stu-id="090a0-138">**StorageAccountUrl** - The SAS URL for the Azure Storage container used to store the backups.</span></span>
* <span data-ttu-id="090a0-139">**FrequencyInterval** – jak často má být k zálohování číselné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="090a0-139">**FrequencyInterval** - Numeric value for how often the backups should be made.</span></span> <span data-ttu-id="090a0-140">Musí být kladné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="090a0-140">Must be a positive integer.</span></span>
* <span data-ttu-id="090a0-141">**FrequencyUnit** -jednotka času pro jak často má být k zálohování.</span><span class="sxs-lookup"><span data-stu-id="090a0-141">**FrequencyUnit** - Unit of time for how often the backups should be made.</span></span> <span data-ttu-id="090a0-142">Možnosti jsou hodin a dnů.</span><span class="sxs-lookup"><span data-stu-id="090a0-142">Options are Hour and Day.</span></span>
* <span data-ttu-id="090a0-143">**RetentionPeriodInDays** – jak dlouho má být uložen automatické zálohování, než bude automaticky odstraněna.</span><span class="sxs-lookup"><span data-stu-id="090a0-143">**RetentionPeriodInDays** - How many days the automatic backups should be saved before being automatically deleted.</span></span>
* <span data-ttu-id="090a0-144">**StartTime** – volitelné.</span><span class="sxs-lookup"><span data-stu-id="090a0-144">**StartTime** - Optional.</span></span> <span data-ttu-id="090a0-145">Čas, kdy by měl začínat automatické zálohování.</span><span class="sxs-lookup"><span data-stu-id="090a0-145">The time when the automatic backups should begin.</span></span> <span data-ttu-id="090a0-146">Zálohování začne okamžitě, pokud je null.</span><span class="sxs-lookup"><span data-stu-id="090a0-146">Backups begin immediately if this is null.</span></span> <span data-ttu-id="090a0-147">Musí být hodnota DateTime.</span><span class="sxs-lookup"><span data-stu-id="090a0-147">Must be a DateTime.</span></span>
* <span data-ttu-id="090a0-148">**Databáze** – volitelné.</span><span class="sxs-lookup"><span data-stu-id="090a0-148">**Databases** - Optional.</span></span> <span data-ttu-id="090a0-149">Pole DatabaseBackupSettings pro databáze pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="090a0-149">An array of DatabaseBackupSettings for the databases to backup.</span></span>
* <span data-ttu-id="090a0-150">**KeepAtLeastOneBackup** – volitelné přepnout parametr.</span><span class="sxs-lookup"><span data-stu-id="090a0-150">**KeepAtLeastOneBackup** - Optional switched parameter.</span></span> <span data-ttu-id="090a0-151">Zadejte tento Pokud jednu zálohu by měly být udržovány vždy v účtu úložiště, bez ohledu na to, kolik je.</span><span class="sxs-lookup"><span data-stu-id="090a0-151">Supply this if one backup should always be kept in the storage account, regardless of how old it is.</span></span>

<span data-ttu-id="090a0-152">Tady je příklad toho, jak použít tuto rutinu.</span><span class="sxs-lookup"><span data-stu-id="090a0-152">Following is an example of how to use this cmdlet.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

<span data-ttu-id="090a0-153">Chcete-li získat aktuální plán zálohování, použijte rutinu Get-AzureRmWebAppBackupConfiguration.</span><span class="sxs-lookup"><span data-stu-id="090a0-153">To get the current backup schedule, use the Get-AzureRmWebAppBackupConfiguration cmdlet.</span></span> <span data-ttu-id="090a0-154">To může být užitečné pro úpravy plánu, který je už nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="090a0-154">This can be useful for modifying a schedule that has already been configured.</span></span>

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a><span data-ttu-id="090a0-155">Obnovení ze zálohy, webové aplikace</span><span class="sxs-lookup"><span data-stu-id="090a0-155">Restore a web app from a backup</span></span>
<span data-ttu-id="090a0-156">Webovou aplikaci obnovit ze zálohy, použijte rutinu AzureRmWebAppBackup obnovení.</span><span class="sxs-lookup"><span data-stu-id="090a0-156">To restore a web app from a backup, use the Restore-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="090a0-157">Nejjednodušší způsob, jak použít tuto rutinu je kanál v objektu zálohy načíst z rutiny Get-AzureRmWebAppBackup nebo rutiny Get-AzureRmWebAppBackupList.</span><span class="sxs-lookup"><span data-stu-id="090a0-157">The easiest way to use this cmdlet is to pipe in a backup object retrieved from the Get-AzureRmWebAppBackup cmdlet or Get-AzureRmWebAppBackupList cmdlet.</span></span>

<span data-ttu-id="090a0-158">Jakmile máte objekt zálohování, zřetězit ho do rutiny AzureRmWebAppBackup obnovení.</span><span class="sxs-lookup"><span data-stu-id="090a0-158">Once you have a backup object, you can pipe it into the Restore-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="090a0-159">Zadejte parametr přepsat přepínače k označení, kterou chcete přepsat obsah vaší webové aplikace se obsah zálohy.</span><span class="sxs-lookup"><span data-stu-id="090a0-159">Specify the Overwrite switch parameter to indicate that you intend to overwrite the contents of your web app with the contents of the backup.</span></span> <span data-ttu-id="090a0-160">Pokud záloha obsahuje databáze, jsou tyto databáze také obnovit.</span><span class="sxs-lookup"><span data-stu-id="090a0-160">If the backup contains databases, those databases are restored as well.</span></span>

        $backup | Restore-AzureRmWebAppBackup -Overwrite

<span data-ttu-id="090a0-161">Následuje příklad použití Restore-AzureRmWebAppBackup zadáním všechny parametry.</span><span class="sxs-lookup"><span data-stu-id="090a0-161">Following is an example of how to use the Restore-AzureRmWebAppBackup by specifying all the parameters.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a><span data-ttu-id="090a0-162">Odstranit zálohy</span><span class="sxs-lookup"><span data-stu-id="090a0-162">Delete a backup</span></span>
<span data-ttu-id="090a0-163">Pokud chcete odstranit zálohy, použijte rutinu Remove-AzureRmWebAppBackup.</span><span class="sxs-lookup"><span data-stu-id="090a0-163">To delete a backup, use the Remove-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="090a0-164">Tím se odeberou zálohování z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="090a0-164">This removes the backup from your storage account.</span></span> <span data-ttu-id="090a0-165">Zadejte název vaší aplikace, jeho skupin prostředků a ID zálohu, kterou chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="090a0-165">Specify your app name, its resource group, and the ID of the backup you want to delete.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

<span data-ttu-id="090a0-166">Také můžete předat objekt zálohování do rutiny Remove-AzureRmWebAppBackup ho odstranit.</span><span class="sxs-lookup"><span data-stu-id="090a0-166">You can also pipe a backup object into the Remove-AzureRmWebAppBackup cmdlet to delete it.</span></span>

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
