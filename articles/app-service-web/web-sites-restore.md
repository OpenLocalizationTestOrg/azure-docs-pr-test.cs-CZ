---
title: "Obnovení aplikace v Azure"
description: "Zjistěte, jak vaše aplikace obnovit ze zálohy."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 5fe74d992edb7028fa4a2500e427013d98ebc250
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="restore-an-app-in-azure"></a><span data-ttu-id="4dc08-103">Obnovení aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="4dc08-103">Restore an app in Azure</span></span>
<span data-ttu-id="4dc08-104">Tento článek ukazuje, jak obnovit aplikace v [Azure App Service](../app-service/app-service-value-prop-what-is.md) který jste dříve zálohovali (viz [zálohování aplikace v Azure](web-sites-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="4dc08-104">This article shows you how to restore an app in [Azure App Service](../app-service/app-service-value-prop-what-is.md) that you have previously backed up (see [Back up your app in Azure](web-sites-backup.md)).</span></span> <span data-ttu-id="4dc08-105">Můžete obnovit do předchozího stavu vaší aplikace pomocí jeho propojené databází na vyžádání nebo vytvořit novou aplikaci na základě jedné z původní aplikaci zálohování.</span><span class="sxs-lookup"><span data-stu-id="4dc08-105">You can restore your app with its linked databases on-demand to a previous state, or create a new app based on one of your original app's backup.</span></span> <span data-ttu-id="4dc08-106">Aplikační služba Azure podporuje následující databáze pro zálohování a obnovení:</span><span class="sxs-lookup"><span data-stu-id="4dc08-106">Azure App Service supports the following databases for backup and restore:</span></span>
- [<span data-ttu-id="4dc08-107">SQL Database</span><span class="sxs-lookup"><span data-stu-id="4dc08-107">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
- [<span data-ttu-id="4dc08-108">Azure databáze pro databázi MySQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="4dc08-108">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
- [<span data-ttu-id="4dc08-109">Azure databázi PostgreSQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="4dc08-109">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
- [<span data-ttu-id="4dc08-110">Databáze MySQL cleardb –</span><span class="sxs-lookup"><span data-stu-id="4dc08-110">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [<span data-ttu-id="4dc08-111">MySQL v aplikaci</span><span class="sxs-lookup"><span data-stu-id="4dc08-111">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

<span data-ttu-id="4dc08-112">Obnovení ze zálohy je k dispozici pro aplikace běžící **standardní** a **Premium** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="4dc08-112">Restoring from backups is available to apps running in **Standard** and **Premium** tier.</span></span> <span data-ttu-id="4dc08-113">Informace o vertikálním navýšení kapacity aplikace naleznete v tématu [škálování aplikace v Azure](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="4dc08-113">For information about scaling up your app, see [Scale up an app in Azure](web-sites-scale.md).</span></span> <span data-ttu-id="4dc08-114">**Premium** úroveň umožňuje větší počet denní zálohování než **standardní** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="4dc08-114">**Premium** tier allows a greater number of daily backups to be performed than **Standard** tier.</span></span>

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a><span data-ttu-id="4dc08-115">Obnovení aplikace z existující zálohy</span><span class="sxs-lookup"><span data-stu-id="4dc08-115">Restore an app from an existing backup</span></span>
1. <span data-ttu-id="4dc08-116">Na **nastavení** vaší aplikace na portálu Azure klikněte na **zálohování** zobrazíte **zálohování** okno.</span><span class="sxs-lookup"><span data-stu-id="4dc08-116">On the **Settings** blade of your app in the Azure Portal, click **Backups** to display the **Backups** blade.</span></span> <span data-ttu-id="4dc08-117">Pak klikněte na tlačítko **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="4dc08-117">Then click **Restore**.</span></span>
   
    ![Zvolte teď obnovení][ChooseRestoreNow]
2. <span data-ttu-id="4dc08-119">V **obnovení** okno, nejprve vyberte zálohování zdroj.</span><span class="sxs-lookup"><span data-stu-id="4dc08-119">In the **Restore** blade, first select the backup source.</span></span>
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    <span data-ttu-id="4dc08-120">**Zálohování aplikace** možnost ukazuje všechny stávající zálohy aktuální aplikaci, a můžete snadno vybrat jednu.</span><span class="sxs-lookup"><span data-stu-id="4dc08-120">The **App backup** option shows you all the existing backups of the current app, and you can easily select one.</span></span>
    <span data-ttu-id="4dc08-121">**Úložiště** možnost umožňuje vybrat všechny záložní soubor ZIP z jakékoli existující účet služby Azure Storage a kontejner v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="4dc08-121">The **Storage** option lets you select any backup ZIP file from any existing Azure Storage account and container in your subscription.</span></span>
    <span data-ttu-id="4dc08-122">Pokud se pokoušíte obnovit zálohu jiné aplikaci, použijte **úložiště** možnost.</span><span class="sxs-lookup"><span data-stu-id="4dc08-122">If you're trying to restore a backup of another app, use the **Storage** option.</span></span>
3. <span data-ttu-id="4dc08-123">Zadejte cíl pro obnovení aplikace v **cíl obnovení**.</span><span class="sxs-lookup"><span data-stu-id="4dc08-123">Then, specify the destination for the app restore in **Restore destination**.</span></span>
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > <span data-ttu-id="4dc08-124">Pokud se rozhodnete **přepsat**, všechny je vymazat a přepsat existující data v aktuální aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4dc08-124">If you choose **Overwrite**, all existing data in your current app is erased and overwritten.</span></span> <span data-ttu-id="4dc08-125">Před kliknutím na **OK**, ujistěte se, že je přesně co chcete udělat.</span><span class="sxs-lookup"><span data-stu-id="4dc08-125">Before you click **OK**, make sure that it is exactly what you want to do.</span></span>
   > 
   > 
   
    <span data-ttu-id="4dc08-126">Můžete vybrat **stávající aplikace** obnovení zálohy aplikace do jiné aplikace ve stejné skupině provedena.</span><span class="sxs-lookup"><span data-stu-id="4dc08-126">You can select **Existing App** to restore the app backup to another app in the same resoure group.</span></span> <span data-ttu-id="4dc08-127">Než použijete tuto možnost, měli jste již vytvořili jiné aplikace ve vaší skupině prostředků s zrcadlení konfigurace databáze na jednu definované v aplikaci zálohování.</span><span class="sxs-lookup"><span data-stu-id="4dc08-127">Before you use this option, you should have already created another app in your resource group with mirroring database configuration to the one defined in the app backup.</span></span> <span data-ttu-id="4dc08-128">Můžete také vytvořit **nový** obnovit svůj obsah do aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dc08-128">You can also Create a **New** app to restore your content to.</span></span>

4. <span data-ttu-id="4dc08-129">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4dc08-129">Click **OK**.</span></span>

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a><span data-ttu-id="4dc08-130">Stáhnout nebo odstranit zálohy z účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="4dc08-130">Download or delete a backup from a storage account</span></span>
1. <span data-ttu-id="4dc08-131">Z hlavní **Procházet** okno portálu Azure, vyberte **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4dc08-131">From the main **Browse** blade of the Azure portal, select **Storage accounts**.</span></span> <span data-ttu-id="4dc08-132">Zobrazí se seznam existující účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="4dc08-132">A list of your existing storage accounts is displayed.</span></span>
2. <span data-ttu-id="4dc08-133">Vyberte účet úložiště, který obsahuje zálohu, kterou chcete stáhnout nebo odstranit. Zobrazí se okno pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4dc08-133">Select the storage account that contains the backup that you want to download or delete.The blade for the storage account is displayed.</span></span>
3. <span data-ttu-id="4dc08-134">V okně účtu úložiště vyberte kontejner, který chcete</span><span class="sxs-lookup"><span data-stu-id="4dc08-134">In the storage account blade, select the container you want</span></span>
   
    ![Zobrazení kontejnery][ViewContainers]
4. <span data-ttu-id="4dc08-136">Vyberte záložní soubor, který chcete stáhnout nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="4dc08-136">Select backup file you want to download or delete.</span></span>
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. <span data-ttu-id="4dc08-138">Klikněte na tlačítko **Stáhnout** nebo **odstranit** v závislosti na tom, co chcete udělat.</span><span class="sxs-lookup"><span data-stu-id="4dc08-138">Click **Download** or **Delete** depending on what you want to do.</span></span>  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a><span data-ttu-id="4dc08-139">Monitorování operací obnovení</span><span class="sxs-lookup"><span data-stu-id="4dc08-139">Monitor a restore operation</span></span>
<span data-ttu-id="4dc08-140">Chcete-li zobrazit podrobnosti o úspěchu nebo neúspěchu operace obnovení aplikace, přejděte na **protokol aktivit** okno na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4dc08-140">To see details about the success or failure of the app restore operation, navigate to the **Activity Log** blade in the Azure portal.</span></span>  
 

<span data-ttu-id="4dc08-141">Přejděte dolů a najděte požadovanou operaci obnovení a klikněte na tlačítko ji vyberte.</span><span class="sxs-lookup"><span data-stu-id="4dc08-141">Scroll down to find the desired restore operation and click to select it.</span></span>

<span data-ttu-id="4dc08-142">V okně podrobností zobrazuje dostupné informace související s operací obnovení.</span><span class="sxs-lookup"><span data-stu-id="4dc08-142">The details blade displays the available information related to the restore operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4dc08-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4dc08-143">Next Steps</span></span>
<span data-ttu-id="4dc08-144">Lze zálohovat a obnovovat aplikace služby App Service pomocí rozhraní REST API (viz [REST použít k zálohování a obnovení služby App Service aplikace](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="4dc08-144">You can backup and restore App Service apps using REST API (see [Use REST to back up and restore App Service apps](websites-csm-backup.md)).</span></span>


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
