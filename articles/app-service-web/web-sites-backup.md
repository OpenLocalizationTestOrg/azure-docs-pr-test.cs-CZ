---
title: "Zálohování aplikace v Azure"
description: "Naučte se vytvářet zálohy aplikací ve službě Azure App Service."
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
ms.openlocfilehash: 77e983afaaba8e944ab1f337e1c28ced83b63205
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-your-app-in-azure"></a><span data-ttu-id="a06d6-103">Zálohování aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="a06d6-103">Back up your app in Azure</span></span>
<span data-ttu-id="a06d6-104">Back up a obnovení funkce v [Azure App Service](../app-service/app-service-value-prop-what-is.md) umožňuje snadno vytvářet zálohy aplikaci ručně nebo podle plánu.</span><span class="sxs-lookup"><span data-stu-id="a06d6-104">The Back up and Restore feature in [Azure App Service](../app-service/app-service-value-prop-what-is.md) lets you easily create app backups manually or on a schedule.</span></span> <span data-ttu-id="a06d6-105">Aplikace můžete obnovit do snímku do předchozího stavu pomocí přepsal stávající aplikace nebo při obnovování jiné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a06d6-105">You can restore the app to a snapshot of a previous state by overwriting the existing app or restoring to another app.</span></span> 

<span data-ttu-id="a06d6-106">Informace o obnovení ze zálohy aplikace najdete v tématu [obnovení aplikace v Azure](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="a06d6-106">For information on restoring an app from backup, see [Restore an app in Azure](web-sites-restore.md).</span></span>

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a><span data-ttu-id="a06d6-107">Co se zálohuje</span><span class="sxs-lookup"><span data-stu-id="a06d6-107">What gets backed up</span></span>
<span data-ttu-id="a06d6-108">Služby App Service můžete zálohovat na účtu úložiště Azure a kontejner, který jste nakonfigurovali v aplikaci použijte následující informace.</span><span class="sxs-lookup"><span data-stu-id="a06d6-108">App Service can backup the following information to an Azure storage account and container that you have configured your app to use.</span></span> 

* <span data-ttu-id="a06d6-109">Konfigurace aplikací</span><span class="sxs-lookup"><span data-stu-id="a06d6-109">App configuration</span></span>
* <span data-ttu-id="a06d6-110">Obsah souboru</span><span class="sxs-lookup"><span data-stu-id="a06d6-110">File content</span></span>
* <span data-ttu-id="a06d6-111">Databáze připojenou k aplikaci</span><span class="sxs-lookup"><span data-stu-id="a06d6-111">Database connected to your app</span></span>

<span data-ttu-id="a06d6-112">Funkce zálohování podporuje následující databáze řešení:</span><span class="sxs-lookup"><span data-stu-id="a06d6-112">The following database solutions are supported with backup feature:</span></span> 
   - [<span data-ttu-id="a06d6-113">SQL Database</span><span class="sxs-lookup"><span data-stu-id="a06d6-113">SQL Database</span></span>](https://azure.microsoft.com/en-us/services/sql-database/)
   - [<span data-ttu-id="a06d6-114">Azure databáze pro databázi MySQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="a06d6-114">Azure Database for MySQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/mysql)
   - [<span data-ttu-id="a06d6-115">Azure databázi PostgreSQL (Preview)</span><span class="sxs-lookup"><span data-stu-id="a06d6-115">Azure Database for PostgreSQL (Preview)</span></span>](https://azure.microsoft.com/en-us/services/postgres)
   - [<span data-ttu-id="a06d6-116">Databáze MySQL cleardb –</span><span class="sxs-lookup"><span data-stu-id="a06d6-116">ClearDB MySQL</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
   - [<span data-ttu-id="a06d6-117">MySQL v aplikaci</span><span class="sxs-lookup"><span data-stu-id="a06d6-117">MySQL in-app</span></span>](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  <span data-ttu-id="a06d6-118">Každá záloha je kompletní kopie offline vaší aplikace, ne přírůstkové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a06d6-118">Each backup is a complete offline copy of your app, not an incremental update.</span></span>
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a><span data-ttu-id="a06d6-119">Požadavky a omezení</span><span class="sxs-lookup"><span data-stu-id="a06d6-119">Requirements and restrictions</span></span>
* <span data-ttu-id="a06d6-120">Zálohování a obnovení funkce vyžaduje plán služby App Service ve **standardní** vrstvy nebo **Premium** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="a06d6-120">The Back up and Restore feature requires the App Service plan to be in the **Standard** tier or **Premium** tier.</span></span> <span data-ttu-id="a06d6-121">Další informace o škálování používat vyšší úroveň plánu služby App Service najdete v tématu [škálování aplikace v Azure](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="a06d6-121">For more information about scaling your App Service plan to use a higher tier, see [Scale up an app in Azure](web-sites-scale.md).</span></span>  
  <span data-ttu-id="a06d6-122">**Premium** úroveň umožňuje větší počet denní zálohování ups než **standardní** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="a06d6-122">**Premium** tier allows a greater number of daily back ups than **Standard** tier.</span></span>
* <span data-ttu-id="a06d6-123">Budete potřebovat účet úložiště Azure a kontejner ve stejném předplatném jako aplikace, které chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="a06d6-123">You need an Azure storage account and container in the same subscription as the app that you want to backup.</span></span> <span data-ttu-id="a06d6-124">Další informace o účtech Azure storage, najdete v článku [odkazy](#moreaboutstorage) na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="a06d6-124">For more information on Azure storage accounts, see the [links](#moreaboutstorage) at the end of this article.</span></span>
* <span data-ttu-id="a06d6-125">Zálohování může být až 10 GB aplikaci a databázi obsahu.</span><span class="sxs-lookup"><span data-stu-id="a06d6-125">Backups can be up to 10 GB of app and database content.</span></span> <span data-ttu-id="a06d6-126">Pokud velikost zálohování překračuje tento limit, dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="a06d6-126">If the backup size exceeds this limit, you get an error .</span></span>

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a><span data-ttu-id="a06d6-127">Vytvoření ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="a06d6-127">Create a manual backup</span></span>
1. <span data-ttu-id="a06d6-128">V [portálu Azure](https://portal.azure.com), přejděte do okna vaší aplikace, vyberte **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="a06d6-128">In the [Azure Portal](https://portal.azure.com), navigate to your app's blade, select **Backups**.</span></span> <span data-ttu-id="a06d6-129">**Zálohování** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="a06d6-129">The **Backups** blade will be displayed.</span></span>
   
    ![Stránka zálohy][ChooseBackupsPage]
   
   > [!NOTE]
   > <span data-ttu-id="a06d6-131">Pokud se zobrazí následující zpráva, klikněte na něj upgradovat plán služby App Service, abyste mohli pokračovat v zálohování.</span><span class="sxs-lookup"><span data-stu-id="a06d6-131">If you see the message below, click it to upgrade your App Service plan before you can proceed with backups.</span></span>
   > <span data-ttu-id="a06d6-132">V tématu [škálování aplikace v Azure](web-sites-scale.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a06d6-132">See [Scale up an app in Azure](web-sites-scale.md) for more information.</span></span>  
   > <span data-ttu-id="a06d6-133">![Zvolte účet úložiště](./media/web-sites-backup/01UpgradePlan1.png)</span><span class="sxs-lookup"><span data-stu-id="a06d6-133">![Choose storage account](./media/web-sites-backup/01UpgradePlan1.png)</span></span>
   > 
   > 

2. <span data-ttu-id="a06d6-134">V **zálohování** okně klikněte na tlačítko **konfigurace**
![klikněte na tlačítko Konfigurovat.](./media/web-sites-backup/ClickConfigure1.png)</span><span class="sxs-lookup"><span data-stu-id="a06d6-134">In the **Backup** blade, Click **Configure**
![Click Configure](./media/web-sites-backup/ClickConfigure1.png)</span></span>
3. <span data-ttu-id="a06d6-135">V **konfigurace zálohování** okně klikněte na tlačítko **úložiště: není nakonfigurováno** ke konfiguraci účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a06d6-135">In the **Backup Configuration** blade, click **Storage: Not configured** to configure a storage account.</span></span>
   
    ![Zvolte účet úložiště][ChooseStorageAccount]
4. <span data-ttu-id="a06d6-137">Vyberte cíl zálohování tak, že vyberete **účet úložiště** a **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="a06d6-137">Choose your backup destination by selecting a **Storage Account** and **Container**.</span></span> <span data-ttu-id="a06d6-138">Účet úložiště musí patřit do stejného předplatného jako aplikace, které chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="a06d6-138">The storage account must belong to the same subscription as the app you want to back up.</span></span> <span data-ttu-id="a06d6-139">Pokud chcete, můžete vytvořit nový účet úložiště nebo nový kontejner v odpovídajících podoken.</span><span class="sxs-lookup"><span data-stu-id="a06d6-139">If you wish, you can create a new storage account or a new container in the respective blades.</span></span> <span data-ttu-id="a06d6-140">Když jste hotovi, klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="a06d6-140">When you're done, click **Select**.</span></span>
   
    ![Zvolte účet úložiště](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. <span data-ttu-id="a06d6-142">V **konfigurace zálohování** okno, které je stále ponechány otevřené, můžete nakonfigurovat **příkaz Backup Database**, vyberte databáze, které chcete zahrnout do zálohy (databáze SQL nebo MySQL) a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a06d6-142">In the **Backup Configuration** blade that is still left open, you can configure **Backup Database**, then select the databases you want to include in the backups (SQL database or MySQL), then click **OK**.</span></span>  
   
    ![Zvolte účet úložiště](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > <span data-ttu-id="a06d6-144">Pro databázi se objeví v tomto seznamu, musí existovat jeho připojovací řetězec v **připojovací řetězce** části **nastavení aplikace** okno pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a06d6-144">For a database to appear in this list, its connection string must exist in the **Connection strings** section of the **Application settings** blade for your app.</span></span>
   > 
   > 
6. <span data-ttu-id="a06d6-145">V **konfigurace zálohování** okně klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a06d6-145">In the **Backup Configuration** blade, click **Save**.</span></span>    
7. <span data-ttu-id="a06d6-146">V **zálohování** okně klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="a06d6-146">In the  **Backups** blade, click **Backup**.</span></span>
   
    ![Tlačítko BackUpNow][BackUpNow]
   
    <span data-ttu-id="a06d6-148">Zobrazí zprávu o průběhu během procesu zálohování.</span><span class="sxs-lookup"><span data-stu-id="a06d6-148">You see a progress message during the backup process.</span></span>

<span data-ttu-id="a06d6-149">Jakmile nakonfigurovaný účet úložiště a kontejneru můžete kdykoli spustit ruční zálohy.</span><span class="sxs-lookup"><span data-stu-id="a06d6-149">Once the storage account and container is configured you can initiate a manual backup at any time.</span></span>  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a><span data-ttu-id="a06d6-150">Konfigurace automatického zálohování</span><span class="sxs-lookup"><span data-stu-id="a06d6-150">Configure automated backups</span></span>
1. <span data-ttu-id="a06d6-151">V **konfigurace zálohy** okně nastavit **naplánovaná zálohování** k **na**.</span><span class="sxs-lookup"><span data-stu-id="a06d6-151">In the **Backup Configuration** blade, set **Scheduled backup** to **On**.</span></span> 
   
    ![Zvolte účet úložiště](./media/web-sites-backup/05ScheduleBackup1.png)
2. <span data-ttu-id="a06d6-153">Nastavte plán zálohování, které se zobrazí možnosti, **naplánované zálohování** k **na**, podle potřeby nakonfigurujte plán zálohování a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a06d6-153">Backup schedule options will show up, set **Scheduled Backup** to **On**, then configure the backup schedule as desired and click **OK**.</span></span>
   
    ![Povolit automatické zálohování][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a><span data-ttu-id="a06d6-155">Nakonfigurujte částečné zálohy</span><span class="sxs-lookup"><span data-stu-id="a06d6-155">Configure Partial Backups</span></span>
<span data-ttu-id="a06d6-156">Někdy nechcete zálohování všechno ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a06d6-156">Sometimes you don't want to backup everything on your app.</span></span> <span data-ttu-id="a06d6-157">Tady je pár příkladů:</span><span class="sxs-lookup"><span data-stu-id="a06d6-157">Here are a few examples:</span></span>

* <span data-ttu-id="a06d6-158">Můžete [nastavení týdenní zálohování](web-sites-backup.md#configure-automated-backups) vaší aplikace, který obsahuje statický obsah, který nikdy změny, jako starý příspěvcích na blogu nebo bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a06d6-158">You [set up weekly backups](web-sites-backup.md#configure-automated-backups) of your app that contains static content that never changes, such as old blog posts or images.</span></span>
* <span data-ttu-id="a06d6-159">Vaše aplikace obsahuje více než 10 GB obsahu (který je maximální velikost, můžete zálohovat v čase).</span><span class="sxs-lookup"><span data-stu-id="a06d6-159">Your app has over 10 GB of content (that's the max amount you can backup at a time).</span></span>
* <span data-ttu-id="a06d6-160">Nechcete zálohování souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="a06d6-160">You don't want to backup the log files.</span></span>

<span data-ttu-id="a06d6-161">Částečné zálohy umožňuje zvolit přesně který souborů, které jste chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="a06d6-161">Partial backups allows you choose exactly which files you want to backup.</span></span>

### <a name="exclude-files-from-your-backup"></a><span data-ttu-id="a06d6-162">Vyloučit soubory ze zálohy</span><span class="sxs-lookup"><span data-stu-id="a06d6-162">Exclude files from your backup</span></span>
<span data-ttu-id="a06d6-163">Předpokládejme, že máte aplikaci, která obsahuje soubory protokolu a statické bitové kopie, které byly zálohování jednou a nebudete změnit.</span><span class="sxs-lookup"><span data-stu-id="a06d6-163">Suppose you have an app that contains log files and static images that have been backup once and are not going to change.</span></span> <span data-ttu-id="a06d6-164">V takových případech můžete vyloučit tyto soubory a složky z ukládají v budoucí zálohy.</span><span class="sxs-lookup"><span data-stu-id="a06d6-164">In such cases you can exclude those folders and files from being stored in your future backups.</span></span> <span data-ttu-id="a06d6-165">Vyloučit soubory a složky ze záloh, vytváření `_backup.filter` v soubor `D:\home\site\wwwroot` složky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a06d6-165">To exclude files and folders from your backups, create a `_backup.filter` file in the `D:\home\site\wwwroot` folder of your app.</span></span> <span data-ttu-id="a06d6-166">Zadejte seznam souborů a složek, které chcete vyloučit v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="a06d6-166">Specify the list of files and folders you want to exclude in this file.</span></span> 

<span data-ttu-id="a06d6-167">Snadný způsob, jak přístup k souborům je použití Kudu.</span><span class="sxs-lookup"><span data-stu-id="a06d6-167">An easy way to access your files is to use Kudu .</span></span> <span data-ttu-id="a06d6-168">Klikněte na tlačítko **Rozšířené nástroje -> – přejděte** nastavení pro vaši webovou aplikaci pro přístup k modulu Kudu.</span><span class="sxs-lookup"><span data-stu-id="a06d6-168">Click **Advanced Tools -> Go** setting for your web app to access Kudu.</span></span>

![Kudu pomocí portálu][kudu-portal]

<span data-ttu-id="a06d6-170">Identifikujte složky, které chcete vyloučit ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="a06d6-170">Identify the folders that you want to exclude from your backups.</span></span>  <span data-ttu-id="a06d6-171">Například chcete filtrovat zvýrazněné složky a soubory.</span><span class="sxs-lookup"><span data-stu-id="a06d6-171">For example , you want to filter out the highlighted folder and files.</span></span>

![Složky bitových kopií][ImagesFolder]

<span data-ttu-id="a06d6-173">Vytvořte soubor s názvem `_backup.filter` a put v seznamu nahoře v souboru, ale odebrat `D:\home`.</span><span class="sxs-lookup"><span data-stu-id="a06d6-173">Create a file called `_backup.filter` and put the list above in the file, but remove `D:\home`.</span></span> <span data-ttu-id="a06d6-174">Zobrazí seznam jeden adresář nebo soubor na každém řádku.</span><span class="sxs-lookup"><span data-stu-id="a06d6-174">List one directory or file per line.</span></span> <span data-ttu-id="a06d6-175">Takže obsah souboru by mělo být:</span><span class="sxs-lookup"><span data-stu-id="a06d6-175">So the content of the file should be:</span></span>
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

<span data-ttu-id="a06d6-176">Nahrát `_backup.filter` do souboru `D:\home\site\wwwroot\` adresáři vašeho webu pomocí [ftp](web-sites-deploy.md#ftp) nebo jiné metody.</span><span class="sxs-lookup"><span data-stu-id="a06d6-176">Upload `_backup.filter` file to the `D:\home\site\wwwroot\` directory of your site using [ftp](web-sites-deploy.md#ftp) or any other method.</span></span> <span data-ttu-id="a06d6-177">Pokud chcete, můžete vytvořit soubor přímo pomocí modulu Kudu `DebugConsole` a vložit obsah existuje.</span><span class="sxs-lookup"><span data-stu-id="a06d6-177">If you wish, you can create the file directly using Kudu  `DebugConsole` and insert the content there.</span></span>

<span data-ttu-id="a06d6-178">Spuštění zálohování stejným způsobem, obvyklým způsobem [ručně](#create-a-manual-backup) nebo [automaticky](#configure-automated-backups).</span><span class="sxs-lookup"><span data-stu-id="a06d6-178">Run backups the same way you would normally do it, [manually](#create-a-manual-backup) or [automatically](#configure-automated-backups).</span></span> <span data-ttu-id="a06d6-179">Nyní, všechny soubory a složky, které jsou určené v `_backup.filter` je vyloučen z budoucí zálohy plánované nebo ručně spustit.</span><span class="sxs-lookup"><span data-stu-id="a06d6-179">Now, any files and folders that are specified in `_backup.filter` is excluded from the future backups scheduled or manually initiated.</span></span> 

> [!NOTE]
> <span data-ttu-id="a06d6-180">Obnovit částečné zálohy lokality stejně jako kdybyste [obnovit zálohu regulární](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="a06d6-180">You restore partial backups of your site the same way you would [restore a regular backup](web-sites-restore.md).</span></span> <span data-ttu-id="a06d6-181">Proces obnovení nemá správné věci.</span><span class="sxs-lookup"><span data-stu-id="a06d6-181">The restore process does the right thing.</span></span>
> 
> <span data-ttu-id="a06d6-182">Po obnovení úplné zálohy, veškerý obsah na webu se nahradí ať je v záloze.</span><span class="sxs-lookup"><span data-stu-id="a06d6-182">When a full backup is restored, all content on the site is replaced with whatever is in the backup.</span></span> <span data-ttu-id="a06d6-183">Pokud je soubor v lokalitě, ale není v zálohování získá odstranit.</span><span class="sxs-lookup"><span data-stu-id="a06d6-183">If a file is on the site, but not in the backup it gets deleted.</span></span> <span data-ttu-id="a06d6-184">Ale když se obnoví částečné zálohy veškerý obsah, který se nachází v jedné z zakázané adresářů nebo všechny zakázané souboru, je ponechán beze.</span><span class="sxs-lookup"><span data-stu-id="a06d6-184">But when a partial backup is restored, any content that is located in one of the blacklisted directories, or any blacklisted file, is left as is.</span></span>
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a><span data-ttu-id="a06d6-185">Ukládání záloh</span><span class="sxs-lookup"><span data-stu-id="a06d6-185">How backups are stored</span></span>
<span data-ttu-id="a06d6-186">Po provedení jednoho nebo více zálohování pro aplikaci Zálohování, se zobrazují **kontejnery** okno účtu úložiště a vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="a06d6-186">After you have made one or more backups for your app, the backups are visible on the **Containers** blade of your storage account, and your app.</span></span> <span data-ttu-id="a06d6-187">V účtu úložiště, každá záloha se skládá z`.zip` soubor, který obsahuje data záloh a `.xml` soubor, který obsahuje manifest z `.zip` souboru obsahu.</span><span class="sxs-lookup"><span data-stu-id="a06d6-187">In the storage account, each backup consists of a`.zip` file that contains the backup data and an `.xml` file that contains a manifest of the `.zip` file contents.</span></span> <span data-ttu-id="a06d6-188">Můžete rozbalte a procházet tyto soubory, pokud chcete přístup k zálohování bez ve skutečnosti provádí obnovení aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a06d6-188">You can unzip and browse these files if you want to access your backups without actually performing an app restore.</span></span>

<span data-ttu-id="a06d6-189">V kořenovém souboru the.zip je uložena záloha databáze pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a06d6-189">The database backup for the app is stored in the root of the.zip file.</span></span> <span data-ttu-id="a06d6-190">Pro databázi SQL je soubor souboru BACPAC (bez přípony souboru) a mohou být naimportovány.</span><span class="sxs-lookup"><span data-stu-id="a06d6-190">For a SQL database, this is a BACPAC file (no file extension) and can be imported.</span></span> <span data-ttu-id="a06d6-191">Vytvoření databáze SQL podle export souboru BACPAC naleznete v tématu [Import souboru BACPAC soubor, který chcete vytvořit novou databázi uživatele](http://technet.microsoft.com/library/hh710052.aspx).</span><span class="sxs-lookup"><span data-stu-id="a06d6-191">To create a SQL database based on the BACPAC export, see [Import a BACPAC File to Create a New User Database](http://technet.microsoft.com/library/hh710052.aspx).</span></span>

> [!WARNING]
> <span data-ttu-id="a06d6-192">Změna některý ze souborů ve vaší **websitebackups** kontejner může způsobit zálohování stane neplatnou a proto není – obnovitelné.</span><span class="sxs-lookup"><span data-stu-id="a06d6-192">Altering any of the files in your **websitebackups** container can cause the backup to become invalid and therefore non-restorable.</span></span>
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="a06d6-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a06d6-193">Next Steps</span></span>
<span data-ttu-id="a06d6-194">Informace o obnovení ze zálohy aplikace najdete v tématu [obnovení aplikace v Azure](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="a06d6-194">For information on restoring an app from a backup, see [Restore an app in Azure](web-sites-restore.md).</span></span> <span data-ttu-id="a06d6-195">Také lze zálohovat a obnovovat aplikace služby App Service pomocí rozhraní REST API (v tématu [použití REST pro zálohování a obnovení služby App Service aplikace](websites-csm-backup.md)).</span><span class="sxs-lookup"><span data-stu-id="a06d6-195">You can also backup and restore App Service apps using REST API (see [Use REST to backup and restore App Service apps](websites-csm-backup.md)).</span></span>


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

