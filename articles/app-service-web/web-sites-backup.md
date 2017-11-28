---
title: "aaaBack vaší aplikaci v Azure"
description: "Zjistěte, jak toocreate zálohování aplikací ve službě Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 6223b6bd-84ec-48df-943f-461d84605694
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: e41d93d322bbc48b45b28eeaa817928d83c2b9d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-your-app-in-azure"></a><span data-ttu-id="72e25-103">Zálohování aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="72e25-103">Back up your app in Azure</span></span>
<span data-ttu-id="72e25-104">Hello zálohování a obnovení funkce v [Azure App Service](../app-service/app-service-value-prop-what-is.md) umožňuje snadno vytvářet zálohy aplikaci ručně nebo podle plánu.</span><span class="sxs-lookup"><span data-stu-id="72e25-104">hello Back up and Restore feature in [Azure App Service](../app-service/app-service-value-prop-what-is.md) lets you easily create app backups manually or on a schedule.</span></span> <span data-ttu-id="72e25-105">Přepisování stávající aplikace hello nebo obnovení tooanother aplikace můžete obnovit hello aplikace tooa snímku do předchozího stavu.</span><span class="sxs-lookup"><span data-stu-id="72e25-105">You can restore hello app tooa snapshot of a previous state by overwriting hello existing app or restoring tooanother app.</span></span> 

<span data-ttu-id="72e25-106">Informace o obnovení ze zálohy aplikace najdete v tématu [obnovení aplikace v Azure](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="72e25-106">For information on restoring an app from backup, see [Restore an app in Azure](web-sites-restore.md).</span></span>

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a><span data-ttu-id="72e25-107">Co se zálohuje</span><span class="sxs-lookup"><span data-stu-id="72e25-107">What gets backed up</span></span>
<span data-ttu-id="72e25-108">Služby App Service můžete zálohovat hello následující informace o účtu úložiště Azure tooan a kontejnerů, které jste nakonfigurovali toouse vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="72e25-108">App Service can backup hello following information tooan Azure storage account and container that you have configured your app toouse.</span></span> 

* <span data-ttu-id="72e25-109">Konfigurace aplikací</span><span class="sxs-lookup"><span data-stu-id="72e25-109">App configuration</span></span>
* <span data-ttu-id="72e25-110">Obsah souboru</span><span class="sxs-lookup"><span data-stu-id="72e25-110">File content</span></span>
* <span data-ttu-id="72e25-111">Databáze připojené tooyour aplikace</span><span class="sxs-lookup"><span data-stu-id="72e25-111">Database connected tooyour app</span></span>

<span data-ttu-id="72e25-112">Funkce zálohování podporuje Hello následující databáze řešení:</span><span class="sxs-lookup"><span data-stu-id="72e25-112">hello following database solutions are supported with backup feature:</span></span> 
   - [<span data-ttu-id="72e25-113">SQL Database</span><span class="sxs-lookup"><span data-stu-id="72e25-113">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
   - [<span data-ttu-id="72e25-114">Azure databáze pro databázi MySQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="72e25-114">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
   - [<span data-ttu-id="72e25-115">Azure databázi PostgreSQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="72e25-115">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
   - [<span data-ttu-id="72e25-116">Databáze MySQL cleardb –</span><span class="sxs-lookup"><span data-stu-id="72e25-116">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
   - [<span data-ttu-id="72e25-117">MySQL v aplikaci</span><span class="sxs-lookup"><span data-stu-id="72e25-117">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  <span data-ttu-id="72e25-118">Každá záloha je kompletní kopie offline vaší aplikace, ne přírůstkové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="72e25-118">Each backup is a complete offline copy of your app, not an incremental update.</span></span>
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a><span data-ttu-id="72e25-119">Požadavky a omezení</span><span class="sxs-lookup"><span data-stu-id="72e25-119">Requirements and restrictions</span></span>
* <span data-ttu-id="72e25-120">Hello zálohování a obnovení funkce vyžaduje hello toobe plán služby App Service v hello **standardní** vrstvy nebo **Premium** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="72e25-120">hello Back up and Restore feature requires hello App Service plan toobe in hello **Standard** tier or **Premium** tier.</span></span> <span data-ttu-id="72e25-121">Další informace o škálování vašeho toouse plán služby App Service vyšší úroveň najdete v tématu [škálování aplikace v Azure](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="72e25-121">For more information about scaling your App Service plan toouse a higher tier, see [Scale up an app in Azure](web-sites-scale.md).</span></span>  
  <span data-ttu-id="72e25-122">**Premium** úroveň umožňuje větší počet denní zálohování ups než **standardní** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="72e25-122">**Premium** tier allows a greater number of daily back ups than **Standard** tier.</span></span>
* <span data-ttu-id="72e25-123">Budete potřebovat účet úložiště Azure a kontejneru v hello stejnému předplatnému jako hello aplikace, které chcete toobackup.</span><span class="sxs-lookup"><span data-stu-id="72e25-123">You need an Azure storage account and container in hello same subscription as hello app that you want toobackup.</span></span> <span data-ttu-id="72e25-124">Další informace o účtech Azure storage najdete v tématu hello [odkazy](#moreaboutstorage) na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="72e25-124">For more information on Azure storage accounts, see hello [links](#moreaboutstorage) at hello end of this article.</span></span>
* <span data-ttu-id="72e25-125">Zálohování může být až too10 GB aplikaci a databázi obsahu.</span><span class="sxs-lookup"><span data-stu-id="72e25-125">Backups can be up too10 GB of app and database content.</span></span> <span data-ttu-id="72e25-126">Pokud velikost hello zálohování překračuje tento limit, dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="72e25-126">If hello backup size exceeds this limit, you get an error .</span></span>

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a><span data-ttu-id="72e25-127">Vytvoření ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="72e25-127">Create a manual backup</span></span>
1. <span data-ttu-id="72e25-128">V hello [portálu Azure](https://portal.azure.com)přejděte okně tooyour aplikace, vyberte **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="72e25-128">In hello [Azure Portal](https://portal.azure.com), navigate tooyour app's blade, select **Backups**.</span></span> <span data-ttu-id="72e25-129">Hello **zálohování** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="72e25-129">hello **Backups** blade will be displayed.</span></span>
   
    ![Stránka zálohy][ChooseBackupsPage]
   
   > [!NOTE]
   > <span data-ttu-id="72e25-131">Pokud se zobrazí zpráva hello níže, klikněte na něj tooupgrade plán služby App Service předtím, než můžete pokračovat v zálohování.</span><span class="sxs-lookup"><span data-stu-id="72e25-131">If you see hello message below, click it tooupgrade your App Service plan before you can proceed with backups.</span></span>
   > <span data-ttu-id="72e25-132">V tématu [škálování aplikace v Azure](web-sites-scale.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="72e25-132">See [Scale up an app in Azure](web-sites-scale.md) for more information.</span></span>  
   > <span data-ttu-id="72e25-133">![Zvolte účet úložiště](./media/web-sites-backup/01UpgradePlan1.png)</span><span class="sxs-lookup"><span data-stu-id="72e25-133">![Choose storage account](./media/web-sites-backup/01UpgradePlan1.png)</span></span>
   > 
   > 

2. <span data-ttu-id="72e25-134">V hello **zálohování** okně klikněte na tlačítko **konfigurace**
![klikněte na tlačítko Konfigurovat.](./media/web-sites-backup/ClickConfigure1.png)</span><span class="sxs-lookup"><span data-stu-id="72e25-134">In hello **Backup** blade, Click **Configure**
![Click Configure](./media/web-sites-backup/ClickConfigure1.png)</span></span>
3. <span data-ttu-id="72e25-135">V hello **konfigurace zálohování** okně klikněte na tlačítko **úložiště: není nakonfigurováno** tooconfigure účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="72e25-135">In hello **Backup Configuration** blade, click **Storage: Not configured** tooconfigure a storage account.</span></span>
   
    ![Zvolte účet úložiště][ChooseStorageAccount]
4. <span data-ttu-id="72e25-137">Vyberte cíl zálohování tak, že vyberete **účet úložiště** a **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="72e25-137">Choose your backup destination by selecting a **Storage Account** and **Container**.</span></span> <span data-ttu-id="72e25-138">účet úložiště Hello musí patřit toohello stejnému předplatnému jako aplikace hello chcete tooback nahoru.</span><span class="sxs-lookup"><span data-stu-id="72e25-138">hello storage account must belong toohello same subscription as hello app you want tooback up.</span></span> <span data-ttu-id="72e25-139">Pokud chcete, můžete vytvořit nový účet úložiště nebo nový kontejner v odpovídajících podoken hello.</span><span class="sxs-lookup"><span data-stu-id="72e25-139">If you wish, you can create a new storage account or a new container in hello respective blades.</span></span> <span data-ttu-id="72e25-140">Když jste hotovi, klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="72e25-140">When you're done, click **Select**.</span></span>
   
    ![Zvolte účet úložiště](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. <span data-ttu-id="72e25-142">V hello **konfigurace zálohování** okno, které je stále ponechány otevřené, můžete nakonfigurovat **příkaz Backup Database**, pak vyberte hello databází, které tooinclude v hello zálohování (databáze SQL nebo MySQL), a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="72e25-142">In hello **Backup Configuration** blade that is still left open, you can configure **Backup Database**, then select hello databases you want tooinclude in hello backups (SQL database or MySQL), then click **OK**.</span></span>  
   
    ![Zvolte účet úložiště](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > <span data-ttu-id="72e25-144">Pro databáze tooappear v tomto seznamu, musí existovat jeho připojovací řetězec v hello **připojovací řetězce** části hello **nastavení aplikace** okno pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="72e25-144">For a database tooappear in this list, its connection string must exist in hello **Connection strings** section of hello **Application settings** blade for your app.</span></span>
   > 
   > 
6. <span data-ttu-id="72e25-145">V hello **konfigurace zálohování** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="72e25-145">In hello **Backup Configuration** blade, click **Save**.</span></span>    
7. <span data-ttu-id="72e25-146">V hello **zálohování** okně klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="72e25-146">In hello  **Backups** blade, click **Backup**.</span></span>
   
    ![Tlačítko BackUpNow][BackUpNow]
   
    <span data-ttu-id="72e25-148">Zobrazí zprávu o průběhu během procesu zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="72e25-148">You see a progress message during hello backup process.</span></span>

<span data-ttu-id="72e25-149">Po hello účtu úložiště a kontejneru konfigurace můžete kdykoli spustit ruční zálohy.</span><span class="sxs-lookup"><span data-stu-id="72e25-149">Once hello storage account and container is configured you can initiate a manual backup at any time.</span></span>  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a><span data-ttu-id="72e25-150">Konfigurace automatického zálohování</span><span class="sxs-lookup"><span data-stu-id="72e25-150">Configure automated backups</span></span>
1. <span data-ttu-id="72e25-151">V hello **konfigurace zálohy** okně nastavit **naplánovaná zálohování** příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="72e25-151">In hello **Backup Configuration** blade, set **Scheduled backup** too**On**.</span></span> 
   
    ![Zvolte účet úložiště](./media/web-sites-backup/05ScheduleBackup1.png)
2. <span data-ttu-id="72e25-153">Nastavte plán zálohování, které se zobrazí možnosti, **naplánované zálohování** příliš**na**, podle potřeby nakonfigurujte plán zálohování hello a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="72e25-153">Backup schedule options will show up, set **Scheduled Backup** too**On**, then configure hello backup schedule as desired and click **OK**.</span></span>
   
    ![Povolit automatické zálohování][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a><span data-ttu-id="72e25-155">Nakonfigurujte částečné zálohy</span><span class="sxs-lookup"><span data-stu-id="72e25-155">Configure Partial Backups</span></span>
<span data-ttu-id="72e25-156">V některých případech nechcete, aby toobackup všechno, co ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="72e25-156">Sometimes you don't want toobackup everything on your app.</span></span> <span data-ttu-id="72e25-157">Tady je pár příkladů:</span><span class="sxs-lookup"><span data-stu-id="72e25-157">Here are a few examples:</span></span>

* <span data-ttu-id="72e25-158">Můžete [nastavení týdenní zálohování](web-sites-backup.md#configure-automated-backups) vaší aplikace, který obsahuje statický obsah, který nikdy změny, jako starý příspěvcích na blogu nebo bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="72e25-158">You [set up weekly backups](web-sites-backup.md#configure-automated-backups) of your app that contains static content that never changes, such as old blog posts or images.</span></span>
* <span data-ttu-id="72e25-159">Vaše aplikace obsahuje více než 10 GB obsahu (který je hello maximální velikost, můžete zálohovat v čase).</span><span class="sxs-lookup"><span data-stu-id="72e25-159">Your app has over 10 GB of content (that's hello max amount you can backup at a time).</span></span>
* <span data-ttu-id="72e25-160">Nechcete, aby soubory protokolu toobackup hello.</span><span class="sxs-lookup"><span data-stu-id="72e25-160">You don't want toobackup hello log files.</span></span>

<span data-ttu-id="72e25-161">Částečné zálohy umožňuje zvolit přesně který souborů, které jste má toobackup.</span><span class="sxs-lookup"><span data-stu-id="72e25-161">Partial backups allows you choose exactly which files you want toobackup.</span></span>

### <a name="exclude-files-from-your-backup"></a><span data-ttu-id="72e25-162">Vyloučit soubory ze zálohy</span><span class="sxs-lookup"><span data-stu-id="72e25-162">Exclude files from your backup</span></span>
<span data-ttu-id="72e25-163">Předpokládejme, že máte aplikaci, která obsahuje soubory protokolu a statické bitové kopie, které byly zálohování jednou a nebudete toochange.</span><span class="sxs-lookup"><span data-stu-id="72e25-163">Suppose you have an app that contains log files and static images that have been backup once and are not going toochange.</span></span> <span data-ttu-id="72e25-164">V takových případech můžete vyloučit tyto soubory a složky z ukládají v budoucí zálohy.</span><span class="sxs-lookup"><span data-stu-id="72e25-164">In such cases you can exclude those folders and files from being stored in your future backups.</span></span> <span data-ttu-id="72e25-165">tooexclude soubory a složky ze záloh, vytváření `_backup.filter` souboru v hello `D:\home\site\wwwroot` složky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="72e25-165">tooexclude files and folders from your backups, create a `_backup.filter` file in hello `D:\home\site\wwwroot` folder of your app.</span></span> <span data-ttu-id="72e25-166">Zadejte hello seznam souborů a složek, které chcete tooexclude v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="72e25-166">Specify hello list of files and folders you want tooexclude in this file.</span></span> 

<span data-ttu-id="72e25-167">Snadný způsob tooaccess toouse Kudu je vaše soubory.</span><span class="sxs-lookup"><span data-stu-id="72e25-167">An easy way tooaccess your files is toouse Kudu .</span></span> <span data-ttu-id="72e25-168">Klikněte na tlačítko **Rozšířené nástroje -> – přejděte** nastavení pro vaše webové aplikace tooaccess Kudu.</span><span class="sxs-lookup"><span data-stu-id="72e25-168">Click **Advanced Tools -> Go** setting for your web app tooaccess Kudu.</span></span>

![Kudu pomocí portálu][kudu-portal]

<span data-ttu-id="72e25-170">Identifikujte složky hello chcete tooexclude ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="72e25-170">Identify hello folders that you want tooexclude from your backups.</span></span>  <span data-ttu-id="72e25-171">Například chcete toofilter hello zvýrazněné složky a soubory.</span><span class="sxs-lookup"><span data-stu-id="72e25-171">For example , you want toofilter out hello highlighted folder and files.</span></span>

![Složky bitových kopií][ImagesFolder]

<span data-ttu-id="72e25-173">Vytvořte soubor s názvem `_backup.filter` a do souboru hello umístit hello výše uvedeného seznamu, ale odebrat `D:\home`.</span><span class="sxs-lookup"><span data-stu-id="72e25-173">Create a file called `_backup.filter` and put hello list above in hello file, but remove `D:\home`.</span></span> <span data-ttu-id="72e25-174">Zobrazí seznam jeden adresář nebo soubor na každém řádku.</span><span class="sxs-lookup"><span data-stu-id="72e25-174">List one directory or file per line.</span></span> <span data-ttu-id="72e25-175">Proto musí být hello obsah souboru hello:</span><span class="sxs-lookup"><span data-stu-id="72e25-175">So hello content of hello file should be:</span></span>
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

<span data-ttu-id="72e25-176">Nahrát `_backup.filter` souboru toohello `D:\home\site\wwwroot\` adresáři vašeho webu pomocí [ftp](web-sites-deploy.md#ftp) nebo jiné metody.</span><span class="sxs-lookup"><span data-stu-id="72e25-176">Upload `_backup.filter` file toohello `D:\home\site\wwwroot\` directory of your site using [ftp](web-sites-deploy.md#ftp) or any other method.</span></span> <span data-ttu-id="72e25-177">Pokud chcete, můžete vytvořit soubor hello přímo pomocí modulu Kudu `DebugConsole` a vložit obsah hello existuje.</span><span class="sxs-lookup"><span data-stu-id="72e25-177">If you wish, you can create hello file directly using Kudu  `DebugConsole` and insert hello content there.</span></span>

<span data-ttu-id="72e25-178">Spuštění zálohování hello stejný způsob, obvyklým způsobem [ručně](#create-a-manual-backup) nebo [automaticky](#configure-automated-backups).</span><span class="sxs-lookup"><span data-stu-id="72e25-178">Run backups hello same way you would normally do it, [manually](#create-a-manual-backup) or [automatically](#configure-automated-backups).</span></span> <span data-ttu-id="72e25-179">Nyní, všechny soubory a složky, které jsou určené v `_backup.filter` je vyloučen z hello budoucí zálohy plánované nebo ručně spustit.</span><span class="sxs-lookup"><span data-stu-id="72e25-179">Now, any files and folders that are specified in `_backup.filter` is excluded from hello future backups scheduled or manually initiated.</span></span> 

> [!NOTE]
> <span data-ttu-id="72e25-180">Obnovení částečné zálohy vaší lokality hello stejným způsobem, jakým byste [obnovit zálohu regulární](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="72e25-180">You restore partial backups of your site hello same way you would [restore a regular backup](web-sites-restore.md).</span></span> <span data-ttu-id="72e25-181">proces obnovení Hello hello správné věci.</span><span class="sxs-lookup"><span data-stu-id="72e25-181">hello restore process does hello right thing.</span></span>
> 
> <span data-ttu-id="72e25-182">Po obnovení úplné zálohy, veškerý obsah na webu hello se nahradí ať je hello zálohování.</span><span class="sxs-lookup"><span data-stu-id="72e25-182">When a full backup is restored, all content on hello site is replaced with whatever is in hello backup.</span></span> <span data-ttu-id="72e25-183">Pokud je soubor na hello lokality, ale není v zálohování hello získá odstranit.</span><span class="sxs-lookup"><span data-stu-id="72e25-183">If a file is on hello site, but not in hello backup it gets deleted.</span></span> <span data-ttu-id="72e25-184">Ale když se obnoví částečné zálohy veškerý obsah, který se nachází v jedné adresářů hello zakázáno, nebo jakékoli zakázané souboru, je ponechán beze.</span><span class="sxs-lookup"><span data-stu-id="72e25-184">But when a partial backup is restored, any content that is located in one of hello blacklisted directories, or any blacklisted file, is left as is.</span></span>
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a><span data-ttu-id="72e25-185">Ukládání záloh</span><span class="sxs-lookup"><span data-stu-id="72e25-185">How backups are stored</span></span>
<span data-ttu-id="72e25-186">Po provedení jednoho nebo více zálohování pro aplikaci Zálohování hello jsou viditelné na hello **kontejnery** okno účtu úložiště a vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="72e25-186">After you have made one or more backups for your app, hello backups are visible on hello **Containers** blade of your storage account, and your app.</span></span> <span data-ttu-id="72e25-187">V účtu úložiště hello, každá záloha se skládá z`.zip` soubor, který obsahuje hello zálohovaná data a `.xml` soubor, který obsahuje manifest hello `.zip` souboru obsahu.</span><span class="sxs-lookup"><span data-stu-id="72e25-187">In hello storage account, each backup consists of a`.zip` file that contains hello backup data and an `.xml` file that contains a manifest of hello `.zip` file contents.</span></span> <span data-ttu-id="72e25-188">Můžete rozbalte a procházet tyto soubory, pokud chcete, aby tooaccess zálohování bez ve skutečnosti obnovení aplikace.</span><span class="sxs-lookup"><span data-stu-id="72e25-188">You can unzip and browse these files if you want tooaccess your backups without actually performing an app restore.</span></span>

<span data-ttu-id="72e25-189">Hello zálohy databáze aplikace hello je uložená v kořenovém hello the.zip souboru.</span><span class="sxs-lookup"><span data-stu-id="72e25-189">hello database backup for hello app is stored in hello root of the.zip file.</span></span> <span data-ttu-id="72e25-190">Pro databázi SQL je soubor souboru BACPAC (bez přípony souboru) a mohou být naimportovány.</span><span class="sxs-lookup"><span data-stu-id="72e25-190">For a SQL database, this is a BACPAC file (no file extension) and can be imported.</span></span> <span data-ttu-id="72e25-191">toocreate databázi SQL podle hello export souboru BACPAC najdete v tématu [importovat soubor souboru BACPAC tooCreate novou databázi uživatele](http://technet.microsoft.com/library/hh710052.aspx).</span><span class="sxs-lookup"><span data-stu-id="72e25-191">toocreate a SQL database based on hello BACPAC export, see [Import a BACPAC File tooCreate a New User Database](http://technet.microsoft.com/library/hh710052.aspx).</span></span>

> [!WARNING]
> <span data-ttu-id="72e25-192">Změna žádné soubory hello ve vaší **websitebackups** kontejner může způsobit hello zálohování toobecome neplatný a proto není – obnovitelné.</span><span class="sxs-lookup"><span data-stu-id="72e25-192">Altering any of hello files in your **websitebackups** container can cause hello backup toobecome invalid and therefore non-restorable.</span></span>
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="72e25-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72e25-193">Next Steps</span></span>
<span data-ttu-id="72e25-194">Informace o obnovení ze zálohy aplikace najdete v tématu [obnovení aplikace v Azure](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="72e25-194">For information on restoring an app from a backup, see [Restore an app in Azure](web-sites-restore.md).</span></span> <span data-ttu-id="72e25-195">Také lze zálohovat a obnovovat aplikace služby App Service pomocí rozhraní REST API (v tématu [aplikace pomocí REST toobackup a obnovení služby App Service](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="72e25-195">You can also backup and restore App Service apps using REST API (see [Use REST toobackup and restore App Service apps](websites-csm-backup.md)).</span></span>


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage1.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount-1.png
[IncludedDatabases]: ./media/web-sites-backup/03IncludedDatabases.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow1.png
[BackupProgress]: ./media/web-sites-backup/05BackupProgress.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn1.png
[Frequency]: ./media/web-sites-backup/07Frequency.png
[StartDate]: ./media/web-sites-backup/08StartDate.png
[StartTime]: ./media/web-sites-backup/09StartTime.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
[kudu-portal]:./media/web-sites-backup/kudu-portal.PNG

