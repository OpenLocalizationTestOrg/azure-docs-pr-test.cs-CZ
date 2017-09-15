<!--author=SharS last changed: 9/17/15-->

<span data-ttu-id="9e48a-101">V tomto postupu budete:</span><span class="sxs-lookup"><span data-stu-id="9e48a-101">In this procedure, you will:</span></span>

1. <span data-ttu-id="9e48a-102">[Příprava ke spuštění spustitelného souboru Funkce Maintainer](#to-prepare-to-run-the-maintainer) .</span><span class="sxs-lookup"><span data-stu-id="9e48a-102">[Prepare to run the Maintainer executable](#to-prepare-to-run-the-maintainer) .</span></span>
2. <span data-ttu-id="9e48a-103">[Příprava databáze obsahu a Koš pro okamžité odstranění objektů BLOB osamocené](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).</span><span class="sxs-lookup"><span data-stu-id="9e48a-103">[Prepare the content database and Recycle Bin for immediate deletion of orphaned BLOBs](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).</span></span>
3. <span data-ttu-id="9e48a-104">[Spustit Maintainer.exe](#to-run-the-maintainer).</span><span class="sxs-lookup"><span data-stu-id="9e48a-104">[Run Maintainer.exe](#to-run-the-maintainer).</span></span>
4. <span data-ttu-id="9e48a-105">[Obnovit databázi obsahu a nastavení koše](#to-revert-the-content-database-and-recycle-bin-settings).</span><span class="sxs-lookup"><span data-stu-id="9e48a-105">[Revert the content database and Recycle Bin settings](#to-revert-the-content-database-and-recycle-bin-settings).</span></span>

#### <a name="to-prepare-to-run-the-maintainer"></a><span data-ttu-id="9e48a-106">Příprava ke spuštění funkce Maintainer</span><span class="sxs-lookup"><span data-stu-id="9e48a-106">To prepare to run the Maintainer</span></span>
1. <span data-ttu-id="9e48a-107">Na webovém serveru front-end otevřete prostředí SharePoint 2013 Management Shell jako správce.</span><span class="sxs-lookup"><span data-stu-id="9e48a-107">On the Web front-end server, open the SharePoint 2013 Management Shell as an administrator.</span></span>
2. <span data-ttu-id="9e48a-108">Přejděte do složky *spouštěcí jednotka*: \Program Files\Microsoft 10.50\Maintainer SQL vzdáleného úložiště objektů Blob\.</span><span class="sxs-lookup"><span data-stu-id="9e48a-108">Navigate to the folder *boot drive*:\Program Files\Microsoft SQL Remote Blob Storage 10.50\Maintainer\.</span></span>
3. <span data-ttu-id="9e48a-109">Přejmenování **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** k **web.config**.</span><span class="sxs-lookup"><span data-stu-id="9e48a-109">Rename **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** to **web.config**.</span></span>
4. <span data-ttu-id="9e48a-110">Použití `aspnet_regiis -pdf connectionStrings` pro dešifrování souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="9e48a-110">Use `aspnet_regiis -pdf connectionStrings` to decrypt the web.config file.</span></span>
5. <span data-ttu-id="9e48a-111">V souboru web.config dešifrovaný v části `connectionStrings` uzlu přidat připojovací řetězec pro vaše instance SQL serveru a název databáze obsahu.</span><span class="sxs-lookup"><span data-stu-id="9e48a-111">In the decrypted web.config file, under the `connectionStrings` node, add the connection string for your SQL server instance and the content database name.</span></span> <span data-ttu-id="9e48a-112">Prohlédněte si následující příklad.</span><span class="sxs-lookup"><span data-stu-id="9e48a-112">See the following example.</span></span>
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. <span data-ttu-id="9e48a-113">Použití `aspnet_regiis –pef connectionStrings` k opětovnému zašifrování souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="9e48a-113">Use `aspnet_regiis –pef connectionStrings` to re-encrypt the web.config file.</span></span> 
7. <span data-ttu-id="9e48a-114">Přejmenujte soubor web.config Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config.</span><span class="sxs-lookup"><span data-stu-id="9e48a-114">Rename web.config to Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config.</span></span> 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a><span data-ttu-id="9e48a-115">Příprava obsahu databáze a Koš okamžitě Odstranit osamocené objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="9e48a-115">To prepare the content database and Recycle Bin to immediately delete orphaned BLOBs</span></span>
1. <span data-ttu-id="9e48a-116">Na serveru SQL v SQL Management Studio, spusťte následující dotazy aktualizace pro cílovou databázi obsahu:</span><span class="sxs-lookup"><span data-stu-id="9e48a-116">On the SQL Server, in SQL Management Studio, run the following update queries for the target content database:</span></span> 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. <span data-ttu-id="9e48a-117">Na webovém serveru front-end v části **centrální správy**, upravit **obecné nastavení webové aplikace** pro požadovanou databázi obsahu k dočasnému zakázání funkce Koš.</span><span class="sxs-lookup"><span data-stu-id="9e48a-117">On the web front-end server, under **Central Administration**, edit the **Web Application General Settings** for the desired content database to temporarily disable the Recycle Bin.</span></span> <span data-ttu-id="9e48a-118">Tato akce také vyprázdníte koš pro všechny související kolekce webů.</span><span class="sxs-lookup"><span data-stu-id="9e48a-118">This action will also empty the Recycle Bin for any related site collections.</span></span> <span data-ttu-id="9e48a-119">Chcete-li to provést, klikněte na tlačítko **centrální správy** -> **Správa aplikací** -> **webových aplikací (Správa webové aplikace)**  ->  **SharePoint - 80** -> **obecné nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9e48a-119">To do this, click **Central Administration** -> **Application Management** -> **Web Applications (Manage web applications)** -> **SharePoint - 80** -> **General Application Settings**.</span></span> <span data-ttu-id="9e48a-120">Nastavte **Stav koše** k **OFF**.</span><span class="sxs-lookup"><span data-stu-id="9e48a-120">Set the **Recycle Bin Status** to **OFF**.</span></span>
   
    ![Obecné nastavení webové aplikace](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a><span data-ttu-id="9e48a-122">Spuštění funkce Maintainer</span><span class="sxs-lookup"><span data-stu-id="9e48a-122">To run the Maintainer</span></span>
* <span data-ttu-id="9e48a-123">Na webovém serveru front-end v prostředí SharePoint 2013 Management Shell, spusťte Funkce Maintainer následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9e48a-123">On the web front-end server, in the SharePoint 2013 Management Shell, run the Maintainer as follows:</span></span>
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > <span data-ttu-id="9e48a-124">Pouze `GarbageCollection` operace je podporována pro zařízení StorSimple v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="9e48a-124">Only the `GarbageCollection` operation is supported for StorSimple at this time.</span></span> <span data-ttu-id="9e48a-125">Všimněte si také, zda jsou parametry vydán pro Microsoft.Data.SqlRemoteBlobs.Maintainer.exe velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="9e48a-125">Also note that the parameters issued for Microsoft.Data.SqlRemoteBlobs.Maintainer.exe are case sensitive.</span></span> 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a><span data-ttu-id="9e48a-126">Můžete obnovit databázi obsahu a nastavení koše</span><span class="sxs-lookup"><span data-stu-id="9e48a-126">To revert the content database and Recycle Bin settings</span></span>
1. <span data-ttu-id="9e48a-127">Na serveru SQL v SQL Management Studio, spusťte následující dotazy aktualizace pro cílovou databázi obsahu:</span><span class="sxs-lookup"><span data-stu-id="9e48a-127">On the SQL Server, in SQL Management Studio, run the following update queries for the target content database:</span></span>
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. <span data-ttu-id="9e48a-128">Na webovém serveru front-end v **centrální správy**, upravit **obecné nastavení webové aplikace** pro databázi požadovaného obsahu znovu zapnout Koš.</span><span class="sxs-lookup"><span data-stu-id="9e48a-128">On the web front-end server, in **Central Administration**, edit the **Web Application General Settings** for the desired content database to re-enable the Recycle Bin.</span></span> <span data-ttu-id="9e48a-129">Chcete-li to provést, klikněte na tlačítko **centrální správy** -> **Správa aplikací** -> **webových aplikací (Správa webové aplikace)**  ->  **SharePoint - 80** -> **obecné nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9e48a-129">To do this, click **Central Administration** -> **Application Management** -> **Web Applications (Manage web applications)** -> **SharePoint - 80** -> **General Application Settings**.</span></span> <span data-ttu-id="9e48a-130">Nastavit stav Bin recyklaci na **ON**.</span><span class="sxs-lookup"><span data-stu-id="9e48a-130">Set the Recycle Bin Status to **ON**.</span></span>

