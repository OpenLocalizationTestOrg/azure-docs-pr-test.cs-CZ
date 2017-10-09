<!--author=SharS last changed: 9/17/15-->

<span data-ttu-id="d8f03-101">V tomto postupu budete:</span><span class="sxs-lookup"><span data-stu-id="d8f03-101">In this procedure, you will:</span></span>

1. <span data-ttu-id="d8f03-102">[Příprava toorun hello Funkce Maintainer spustitelný soubor](#to-prepare-to-run-the-maintainer) .</span><span class="sxs-lookup"><span data-stu-id="d8f03-102">[Prepare toorun hello Maintainer executable](#to-prepare-to-run-the-maintainer) .</span></span>
2. <span data-ttu-id="d8f03-103">[Příprava databáze obsahu hello a Koš pro okamžité odstranění objektů BLOB osamocené](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).</span><span class="sxs-lookup"><span data-stu-id="d8f03-103">[Prepare hello content database and Recycle Bin for immediate deletion of orphaned BLOBs](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).</span></span>
3. <span data-ttu-id="d8f03-104">[Spustit Maintainer.exe](#to-run-the-maintainer).</span><span class="sxs-lookup"><span data-stu-id="d8f03-104">[Run Maintainer.exe](#to-run-the-maintainer).</span></span>
4. <span data-ttu-id="d8f03-105">[Obnovit databázi obsahu hello a nastavení koše](#to-revert-the-content-database-and-recycle-bin-settings).</span><span class="sxs-lookup"><span data-stu-id="d8f03-105">[Revert hello content database and Recycle Bin settings](#to-revert-the-content-database-and-recycle-bin-settings).</span></span>

#### <a name="tooprepare-toorun-hello-maintainer"></a><span data-ttu-id="d8f03-106">tooprepare toorun hello Funkce Maintainer</span><span class="sxs-lookup"><span data-stu-id="d8f03-106">tooprepare toorun hello Maintainer</span></span>
1. <span data-ttu-id="d8f03-107">Na hello webovém serveru front-end otevřete hello SharePoint 2013 Management Shell jako správce.</span><span class="sxs-lookup"><span data-stu-id="d8f03-107">On hello Web front-end server, open hello SharePoint 2013 Management Shell as an administrator.</span></span>
2. <span data-ttu-id="d8f03-108">Přejděte na složku toohello *spouštěcí jednotka*: \Program Files\Microsoft 10.50\Maintainer SQL vzdáleného úložiště objektů Blob\.</span><span class="sxs-lookup"><span data-stu-id="d8f03-108">Navigate toohello folder *boot drive*:\Program Files\Microsoft SQL Remote Blob Storage 10.50\Maintainer\.</span></span>
3. <span data-ttu-id="d8f03-109">Přejmenování **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** příliš**web.config**.</span><span class="sxs-lookup"><span data-stu-id="d8f03-109">Rename **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** too**web.config**.</span></span>
4. <span data-ttu-id="d8f03-110">Použití `aspnet_regiis -pdf connectionStrings` souboru web.config toodecrypt hello.</span><span class="sxs-lookup"><span data-stu-id="d8f03-110">Use `aspnet_regiis -pdf connectionStrings` toodecrypt hello web.config file.</span></span>
5. <span data-ttu-id="d8f03-111">V souboru web.config dešifrovaný hello, v části hello `connectionStrings` uzlu, přidejte hello připojovací řetězec pro vaše instance systému SQL server a název databáze obsahu hello.</span><span class="sxs-lookup"><span data-stu-id="d8f03-111">In hello decrypted web.config file, under hello `connectionStrings` node, add hello connection string for your SQL server instance and hello content database name.</span></span> <span data-ttu-id="d8f03-112">Viz následující ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="d8f03-112">See hello following example.</span></span>
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. <span data-ttu-id="d8f03-113">Použití `aspnet_regiis –pef connectionStrings` toore-zašifrování souboru web.config hello.</span><span class="sxs-lookup"><span data-stu-id="d8f03-113">Use `aspnet_regiis –pef connectionStrings` toore-encrypt hello web.config file.</span></span> 
7. <span data-ttu-id="d8f03-114">Přejmenujte soubor web.config tooMicrosoft.Data.SqlRemoteBlobs.Maintainer.exe.config.</span><span class="sxs-lookup"><span data-stu-id="d8f03-114">Rename web.config tooMicrosoft.Data.SqlRemoteBlobs.Maintainer.exe.config.</span></span> 

#### <a name="tooprepare-hello-content-database-and-recycle-bin-tooimmediately-delete-orphaned-blobs"></a><span data-ttu-id="d8f03-115">databázi obsahu hello tooprepare a Koš tooimmediately odstranění osamocených objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="d8f03-115">tooprepare hello content database and Recycle Bin tooimmediately delete orphaned BLOBs</span></span>
1. <span data-ttu-id="d8f03-116">Na hello systému SQL Server v SQL Management Studio spusťte následující aktualizace dotazů pro databázi obsahu cíl hello hello:</span><span class="sxs-lookup"><span data-stu-id="d8f03-116">On hello SQL Server, in SQL Management Studio, run hello following update queries for hello target content database:</span></span> 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. <span data-ttu-id="d8f03-117">Na hello webový front-end server, v části **centrální správy**, upravit hello **obecné nastavení webové aplikace** pro hello požadovaného obsahu databáze tootemporarily zakázat hello Koš.</span><span class="sxs-lookup"><span data-stu-id="d8f03-117">On hello web front-end server, under **Central Administration**, edit hello **Web Application General Settings** for hello desired content database tootemporarily disable hello Recycle Bin.</span></span> <span data-ttu-id="d8f03-118">Tato akce odstraní také prázdný hello Koš pro všechny kolekce příslušné lokalitě.</span><span class="sxs-lookup"><span data-stu-id="d8f03-118">This action will also empty hello Recycle Bin for any related site collections.</span></span> <span data-ttu-id="d8f03-119">toodo tento, klikněte na tlačítko **centrální správy** -> **Správa aplikací** -> **webových aplikací (Správa webové aplikace)**  ->  **SharePoint - 80** -> **obecné nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d8f03-119">toodo this, click **Central Administration** -> **Application Management** -> **Web Applications (Manage web applications)** -> **SharePoint - 80** -> **General Application Settings**.</span></span> <span data-ttu-id="d8f03-120">Sada hello **Stav koše** příliš**OFF**.</span><span class="sxs-lookup"><span data-stu-id="d8f03-120">Set hello **Recycle Bin Status** too**OFF**.</span></span>
   
    ![Obecné nastavení webové aplikace](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="toorun-hello-maintainer"></a><span data-ttu-id="d8f03-122">toorun hello Funkce Maintainer</span><span class="sxs-lookup"><span data-stu-id="d8f03-122">toorun hello Maintainer</span></span>
* <span data-ttu-id="d8f03-123">Na hello webovém serveru front-end v hello SharePoint 2013 Management Shell, spusťte hello Funkce Maintainer následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d8f03-123">On hello web front-end server, in hello SharePoint 2013 Management Shell, run hello Maintainer as follows:</span></span>
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > <span data-ttu-id="d8f03-124">Pouze hello `GarbageCollection` operace je podporována pro zařízení StorSimple v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="d8f03-124">Only hello `GarbageCollection` operation is supported for StorSimple at this time.</span></span> <span data-ttu-id="d8f03-125">Všimněte si také, zda jsou parametry hello vydán pro Microsoft.Data.SqlRemoteBlobs.Maintainer.exe velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="d8f03-125">Also note that hello parameters issued for Microsoft.Data.SqlRemoteBlobs.Maintainer.exe are case sensitive.</span></span> 
  > 
  > 

#### <a name="toorevert-hello-content-database-and-recycle-bin-settings"></a><span data-ttu-id="d8f03-126">databázi obsahu hello toorevert a nastavení koše</span><span class="sxs-lookup"><span data-stu-id="d8f03-126">toorevert hello content database and Recycle Bin settings</span></span>
1. <span data-ttu-id="d8f03-127">Na hello systému SQL Server v SQL Management Studio spusťte následující aktualizace dotazů pro databázi obsahu cíl hello hello:</span><span class="sxs-lookup"><span data-stu-id="d8f03-127">On hello SQL Server, in SQL Management Studio, run hello following update queries for hello target content database:</span></span>
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. <span data-ttu-id="d8f03-128">Na hello webovém serveru front-end v **centrální správy**, upravit hello **obecné nastavení webové aplikace** pro hello potřeby databáze obsahu toore povolit hello Koš.</span><span class="sxs-lookup"><span data-stu-id="d8f03-128">On hello web front-end server, in **Central Administration**, edit hello **Web Application General Settings** for hello desired content database toore-enable hello Recycle Bin.</span></span> <span data-ttu-id="d8f03-129">toodo tento, klikněte na tlačítko **centrální správy** -> **Správa aplikací** -> **webových aplikací (Správa webové aplikace)**  ->  **SharePoint - 80** -> **obecné nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d8f03-129">toodo this, click **Central Administration** -> **Application Management** -> **Web Applications (Manage web applications)** -> **SharePoint - 80** -> **General Application Settings**.</span></span> <span data-ttu-id="d8f03-130">Nastavit stav koše hello příliš**ON**.</span><span class="sxs-lookup"><span data-stu-id="d8f03-130">Set hello Recycle Bin Status too**ON**.</span></span>

