---
title: aaaRestore aplikace v Azure
description: "Zjistěte, jak toorestore aplikace ze zálohy."
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
ms.openlocfilehash: 4b54029a9197064f990f29a3c4558c8322668714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-app-in-azure"></a><span data-ttu-id="1d4ca-103">Obnovení aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="1d4ca-103">Restore an app in Azure</span></span>
<span data-ttu-id="1d4ca-104">Tento článek ukazuje, jak toorestore na aplikace v [Azure App Service](../app-service/app-service-value-prop-what-is.md) který jste dříve zálohovali (najdete v části [zálohování aplikace v Azure](web-sites-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="1d4ca-104">This article shows you how toorestore an app in [Azure App Service](../app-service/app-service-value-prop-what-is.md) that you have previously backed up (see [Back up your app in Azure](web-sites-backup.md)).</span></span> <span data-ttu-id="1d4ca-105">Dokáže obnovit vaší aplikace pomocí předchozího stavu na vyžádání tooa propojené databáze nebo vytvořit novou aplikaci na základě jedné z původní aplikaci zálohování.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-105">You can restore your app with its linked databases on-demand tooa previous state, or create a new app based on one of your original app's backup.</span></span> <span data-ttu-id="1d4ca-106">Aplikační služba Azure podporuje následující databáze pro zálohování a obnovení hello:</span><span class="sxs-lookup"><span data-stu-id="1d4ca-106">Azure App Service supports hello following databases for backup and restore:</span></span>
- [<span data-ttu-id="1d4ca-107">SQL Database</span><span class="sxs-lookup"><span data-stu-id="1d4ca-107">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
- [<span data-ttu-id="1d4ca-108">Azure databáze pro databázi MySQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="1d4ca-108">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
- [<span data-ttu-id="1d4ca-109">Azure databázi PostgreSQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="1d4ca-109">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
- [<span data-ttu-id="1d4ca-110">Databáze MySQL cleardb –</span><span class="sxs-lookup"><span data-stu-id="1d4ca-110">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [<span data-ttu-id="1d4ca-111">MySQL v aplikaci</span><span class="sxs-lookup"><span data-stu-id="1d4ca-111">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

<span data-ttu-id="1d4ca-112">Obnovení ze zálohy je k dispozici tooapps spuštěné v **standardní** a **Premium** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-112">Restoring from backups is available tooapps running in **Standard** and **Premium** tier.</span></span> <span data-ttu-id="1d4ca-113">Informace o vertikálním navýšení kapacity aplikace naleznete v tématu [škálování aplikace v Azure](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="1d4ca-113">For information about scaling up your app, see [Scale up an app in Azure](web-sites-scale.md).</span></span> <span data-ttu-id="1d4ca-114">**Premium** úroveň umožňuje větší počet denní zálohy toobe provést než **standardní** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-114">**Premium** tier allows a greater number of daily backups toobe performed than **Standard** tier.</span></span>

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a><span data-ttu-id="1d4ca-115">Obnovení aplikace z existující zálohy</span><span class="sxs-lookup"><span data-stu-id="1d4ca-115">Restore an app from an existing backup</span></span>
1. <span data-ttu-id="1d4ca-116">Na hello **nastavení** okna aplikace na hello portálu Azure, klikněte na tlačítko **zálohování** toodisplay hello **zálohování** okno.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-116">On hello **Settings** blade of your app in hello Azure Portal, click **Backups** toodisplay hello **Backups** blade.</span></span> <span data-ttu-id="1d4ca-117">Pak klikněte na tlačítko **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-117">Then click **Restore**.</span></span>
   
    ![Zvolte teď obnovení][ChooseRestoreNow]
2. <span data-ttu-id="1d4ca-119">V hello **obnovení** okno, první zálohování zdroj vyberte hello.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-119">In hello **Restore** blade, first select hello backup source.</span></span>
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    <span data-ttu-id="1d4ca-120">Hello **zálohování aplikace** možnost uvádí všechny hello existující zálohy hello aktuální aplikaci, a můžete snadno vybrat jednu.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-120">hello **App backup** option shows you all hello existing backups of hello current app, and you can easily select one.</span></span>
    <span data-ttu-id="1d4ca-121">Hello **úložiště** možnost umožňuje vybrat všechny záložní soubor ZIP z jakékoli existující účet služby Azure Storage a kontejner v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-121">hello **Storage** option lets you select any backup ZIP file from any existing Azure Storage account and container in your subscription.</span></span>
    <span data-ttu-id="1d4ca-122">Pokud se snažíte toorestore zálohu jiné aplikaci, použijte hello **úložiště** možnost.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-122">If you're trying toorestore a backup of another app, use hello **Storage** option.</span></span>
3. <span data-ttu-id="1d4ca-123">Potom můžete určit hello cíl pro obnovení aplikace hello v **cíl obnovení**.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-123">Then, specify hello destination for hello app restore in **Restore destination**.</span></span>
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > <span data-ttu-id="1d4ca-124">Pokud se rozhodnete **přepsat**, všechny je vymazat a přepsat existující data v aktuální aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-124">If you choose **Overwrite**, all existing data in your current app is erased and overwritten.</span></span> <span data-ttu-id="1d4ca-125">Před kliknutím na **OK**, ujistěte se, že je přesně co chcete toodo.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-125">Before you click **OK**, make sure that it is exactly what you want toodo.</span></span>
   > 
   > 
   
    <span data-ttu-id="1d4ca-126">Můžete vybrat **stávající aplikace** toorestore hello aplikaci Zálohování tooanother aplikace v hello stejnou skupinu provedena.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-126">You can select **Existing App** toorestore hello app backup tooanother app in hello same resoure group.</span></span> <span data-ttu-id="1d4ca-127">Než použijete tuto možnost, měli jste již vytvořili jiné aplikace ve vaší skupině prostředků s zrcadlení databáze konfigurace toohello, definovaná v zálohování aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-127">Before you use this option, you should have already created another app in your resource group with mirroring database configuration toohello one defined in hello app backup.</span></span> <span data-ttu-id="1d4ca-128">Můžete také vytvořit **nový** toorestore aplikace svůj obsah do.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-128">You can also Create a **New** app toorestore your content to.</span></span>

4. <span data-ttu-id="1d4ca-129">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-129">Click **OK**.</span></span>

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a><span data-ttu-id="1d4ca-130">Stáhnout nebo odstranit zálohy z účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="1d4ca-130">Download or delete a backup from a storage account</span></span>
1. <span data-ttu-id="1d4ca-131">Z hlavní hello **Procházet** okno hello portál Azure, vyberte **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-131">From hello main **Browse** blade of hello Azure portal, select **Storage accounts**.</span></span> <span data-ttu-id="1d4ca-132">Zobrazí se seznam existující účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-132">A list of your existing storage accounts is displayed.</span></span>
2. <span data-ttu-id="1d4ca-133">Vyberte účet úložiště hello, který obsahuje hello zálohování, který chcete, zobrazí se okno toodownload nebo delete.hello pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-133">Select hello storage account that contains hello backup that you want toodownload or delete.hello blade for hello storage account is displayed.</span></span>
3. <span data-ttu-id="1d4ca-134">V okně účtu úložiště hello vyberte hello kontejner, který chcete</span><span class="sxs-lookup"><span data-stu-id="1d4ca-134">In hello storage account blade, select hello container you want</span></span>
   
    ![Zobrazení kontejnery][ViewContainers]
4. <span data-ttu-id="1d4ca-136">Vyberte soubor zálohy mají toodownload nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-136">Select backup file you want toodownload or delete.</span></span>
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. <span data-ttu-id="1d4ca-138">Klikněte na tlačítko **Stáhnout** nebo **odstranit** v závislosti na tom, co jste má toodo.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-138">Click **Download** or **Delete** depending on what you want toodo.</span></span>  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a><span data-ttu-id="1d4ca-139">Monitorování operací obnovení</span><span class="sxs-lookup"><span data-stu-id="1d4ca-139">Monitor a restore operation</span></span>
<span data-ttu-id="1d4ca-140">toosee podrobnosti o hello úspěšné nebo neúspěšné operace obnovení hello aplikace, přejděte toohello **protokol aktivit** okno v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-140">toosee details about hello success or failure of hello app restore operation, navigate toohello **Activity Log** blade in hello Azure portal.</span></span>  
 

<span data-ttu-id="1d4ca-141">Přejděte dolů tooselect toofind hello potřeby obnovení operaci a klikněte na tlačítko ji.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-141">Scroll down toofind hello desired restore operation and click tooselect it.</span></span>

<span data-ttu-id="1d4ca-142">Hello podrobnosti okno zobrazí hello k dispozici informace týkající se toohello operace obnovení.</span><span class="sxs-lookup"><span data-stu-id="1d4ca-142">hello details blade displays hello available information related toohello restore operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d4ca-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d4ca-143">Next Steps</span></span>
<span data-ttu-id="1d4ca-144">Lze zálohovat a obnovovat aplikace služby App Service pomocí rozhraní REST API (viz [tooback pomocí REST a obnovení služby App Service aplikace](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="1d4ca-144">You can backup and restore App Service apps using REST API (see [Use REST tooback up and restore App Service apps](websites-csm-backup.md)).</span></span>


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
