<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> <span data-ttu-id="f0389-101">Při provádění změn toohello StorSimple adaptéru pro konfiguraci RBS služby SharePoint, musíte být přihlášeni pomocí uživatelského účtu, který je součástí skupiny Domain Admins toohello.</span><span class="sxs-lookup"><span data-stu-id="f0389-101">When making changes toohello StorSimple Adapter for SharePoint RBS configuration, you must be logged on with a user account that belongs toohello Domain Admins group.</span></span> <span data-ttu-id="f0389-102">Konfigurační stránku hello musí navíc přístup z prohlížeče systémem hello stejné hostitele jako centrální správy.</span><span class="sxs-lookup"><span data-stu-id="f0389-102">Additionally, you must access hello configuration page from a browser running on hello same host as Central Administration.</span></span>
> 
> 

#### <a name="tooconfigure-rbs"></a><span data-ttu-id="f0389-103">tooconfigure RBS</span><span class="sxs-lookup"><span data-stu-id="f0389-103">tooconfigure RBS</span></span>
1. <span data-ttu-id="f0389-104">Otevřete stránku hello Centrální správa SharePoint a procházet příliš**nastavení systému**.</span><span class="sxs-lookup"><span data-stu-id="f0389-104">Open hello SharePoint Central Administration page, and browse too**System Settings**.</span></span> 
2. <span data-ttu-id="f0389-105">V hello **Azure StorSimple** klikněte na tlačítko **nakonfigurovat adaptér StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="f0389-105">In hello **Azure StorSimple** section, click **Configure StorSimple Adapter**.</span></span>
   
    ![Konfigurace hello adaptér StorSimple](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. <span data-ttu-id="f0389-107">Na hello **nakonfigurovat adaptér StorSimple** stránky:</span><span class="sxs-lookup"><span data-stu-id="f0389-107">On hello **Configure StorSimple Adapter** page:</span></span>
   
   1. <span data-ttu-id="f0389-108">Ujistěte se, že hello **povolení úprav cesta** je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="f0389-108">Make sure that hello **Enable editing path** check box is selected.</span></span>
   2. <span data-ttu-id="f0389-109">Hello textového pole zadejte cestu Universal Naming Convention (UNC) hello hello úložišti objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="f0389-109">In hello text box, type hello Universal Naming Convention (UNC) path of hello BLOB store.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="f0389-110">svazek úložiště objektů BLOB Hello musí být hostované na jednotce iSCSI nakonfigurovat v zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="f0389-110">hello BLOB store volume must be hosted on an iSCSI volume configured on hello StorSimple device.</span></span>

   3. <span data-ttu-id="f0389-111">Klikněte na tlačítko hello **povolit** tlačítko pod jednotlivých databází obsahu hello chcete tooconfigure pro vzdálené úložiště.</span><span class="sxs-lookup"><span data-stu-id="f0389-111">Click hello **Enable** button below each of hello content databases that you want tooconfigure for remote storage.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="f0389-112">úložiště objektů BLOB Hello musí být sdílené a dostupné všechny (WFE) servery front-end webové a hello uživatelský účet, který je nakonfigurován pro serverové farmy služby SharePoint hello musí mít přístup toohello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="f0389-112">hello BLOB store must be shared and reachable by all web front-end (WFE) servers, and hello user account that is configured for hello SharePoint server farm must have access toohello share.</span></span>
      
      ![Povolit zprostředkovatele RBS hello](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      <span data-ttu-id="f0389-114">Když povolíte nebo zakážete RBS, zobrazí se také hello následující zprávou.</span><span class="sxs-lookup"><span data-stu-id="f0389-114">When you enable or disable RBS, you will also see hello following message.</span></span>
      
      ![Konfigurace zařízení StorSimple adaptéru povolit zakázat](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. <span data-ttu-id="f0389-116">Klikněte na tlačítko hello **aktualizace** tlačítko tooapply hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f0389-116">Click hello **Update** button tooapply hello configuration.</span></span> <span data-ttu-id="f0389-117">Když kliknete na tlačítko hello **aktualizace** tlačítko hello stav konfigurace RBS bude aktualizována na všech serverech WFE a celou farmu hello budou RBS povolena.</span><span class="sxs-lookup"><span data-stu-id="f0389-117">When you click hello **Update** button, hello RBS configuration status will be updated on all WFE servers, and hello entire farm will be RBS-enabled.</span></span> <span data-ttu-id="f0389-118">Zobrazí se následující zpráva Hello.</span><span class="sxs-lookup"><span data-stu-id="f0389-118">hello following message appears.</span></span>
      
      ![Zpráva Konfigurace adaptéru](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > <span data-ttu-id="f0389-120">Pokud konfigurujete RBS pro farmu služby SharePoint s velmi velký počet databází (větší než 200), webovou stránku hello Centrální správa SharePoint může vypršení časového limitu. Pokud k tomu dojde, aktualizujte stránku hello.</span><span class="sxs-lookup"><span data-stu-id="f0389-120">If you are configuring RBS for a SharePoint farm with a very large number of databases (greater than 200), hello SharePoint Central Administration web page might time out. If that occurs, refresh hello page.</span></span> <span data-ttu-id="f0389-121">Proces konfigurace hello to nemá vliv.</span><span class="sxs-lookup"><span data-stu-id="f0389-121">This does not affect hello configuration process.</span></span>

4. <span data-ttu-id="f0389-122">Ověření konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="f0389-122">Verify hello configuration:</span></span>
   
   1. <span data-ttu-id="f0389-123">Přihlaste se na webu Centrální správa SharePoint toohello a procházet toohello **nakonfigurovat adaptér StorSimple** stránky.</span><span class="sxs-lookup"><span data-stu-id="f0389-123">Log on toohello SharePoint Central Administration website, and browse toohello **Configure StorSimple Adapter** page.</span></span>
   2. <span data-ttu-id="f0389-124">Zkontrolujte toomake podrobnosti konfigurace hello se, že budou odpovídat hello nastavení, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="f0389-124">Check hello configuration details toomake sure that they match hello settings that you entered.</span></span> 
5. <span data-ttu-id="f0389-125">Ověřte, že RBS funguje správně:</span><span class="sxs-lookup"><span data-stu-id="f0389-125">Verify that RBS works correctly:</span></span>
   
   1. <span data-ttu-id="f0389-126">Nahrajte tooSharePoint dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f0389-126">Upload a document tooSharePoint.</span></span> 
   2. <span data-ttu-id="f0389-127">Procházejte cestu UNC toohello, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="f0389-127">Browse toohello UNC path that you configured.</span></span> <span data-ttu-id="f0389-128">Ujistěte se, zda byl vytvořen hello RBS adresářovou strukturu a obsahuje hello nahrán objekt.</span><span class="sxs-lookup"><span data-stu-id="f0389-128">Make sure that hello RBS directory structure was created and that it contains hello uploaded object.</span></span>
6. <span data-ttu-id="f0389-129">(Volitelné) Můžete použít hello Microsoft RBS `Migrate()` rutiny prostředí PowerShell, které jsou součástí služby SharePoint toomigrate existující objekt BLOB obsahu toohello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="f0389-129">(Optional) You can use hello Microsoft RBS `Migrate()` PowerShell cmdlet included with SharePoint toomigrate existing BLOB content toohello StorSimple device.</span></span> <span data-ttu-id="f0389-130">Další informace najdete v tématu [migraci obsahu do nebo z RBS v SharePoint 2013] [ 6] nebo [migraci obsahu do nebo z RBS (SharePoint Foundation 2010)] [7].</span><span class="sxs-lookup"><span data-stu-id="f0389-130">For more information, see [Migrate content into or out of RBS in SharePoint 2013][6] or [Migrate content into or out of RBS (SharePoint Foundation 2010)][7].</span></span>
7. <span data-ttu-id="f0389-131">(Volitelné) Na zkušební instalace můžete ověřit, že byly objekty BLOB hello přesunuty z databáze obsahu hello:</span><span class="sxs-lookup"><span data-stu-id="f0389-131">(Optional) On test installations, you can verify that hello BLOBs were moved out of hello content database as follows:</span></span> 
   
   1. <span data-ttu-id="f0389-132">Spusťte SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="f0389-132">Start SQL Management Studio.</span></span>
   2. <span data-ttu-id="f0389-133">Spusťte dotaz hello ListBlobsInDB_2010.sql nebo ListBlobsInDB_2013.sql, následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="f0389-133">Run hello ListBlobsInDB_2010.sql or ListBlobsInDB_2013.sql query, as follows.</span></span>
      
      ```
      **ListBlobsInDB_2013.sql**
      
        USE WSS_Content
        GO
      
        SELECT DocStreams.DocId,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               DocStreams.RbsId,
               TimeLastModified
      
        FROM DocStreams
      
             INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      
      **ListBlobsInDB_2010.sql**
      
        USE WSS_Content
        GO
      
        SELECT AllDocStreams.Id,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               RbsId,
               TimeLastModified
        FROM AllDocStreams
      
             INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      ```
      
      <span data-ttu-id="f0389-134">Pokud RBS bylo správně nakonfigurované, by se zobrazit hodnotu NULL ve sloupci hello SizeOfContentInDB pro libovolný objekt, který byl odeslán a úspěšně externalized s RBS.</span><span class="sxs-lookup"><span data-stu-id="f0389-134">If RBS was configured correctly, a NULL value should appear in hello SizeOfContentInDB column for any object that was uploaded and successfully externalized with RBS.</span></span>
8. <span data-ttu-id="f0389-135">(Volitelné) Po konfiguraci RBS a přesunout všech objektů BLOB obsahu toohello zařízení StorSimple, můžete přesunout hello databázi obsahu toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="f0389-135">(Optional) After you configure RBS and move all BLOB content toohello StorSimple device, you can move hello content database toohello device.</span></span> <span data-ttu-id="f0389-136">Pokud si zvolíte toomove hello obsahu databáze, doporučujeme nakonfigurovat hello databázi obsahu úložiště na zařízení hello jako primární svazek.</span><span class="sxs-lookup"><span data-stu-id="f0389-136">If you choose toomove hello content database, we recommend that you configure hello content database storage on hello device as a primary volume.</span></span> <span data-ttu-id="f0389-137">Pak použijte navázat, že SQL Server osvědčených postupů zařízení StorSimple pro toohello toomigrate hello databázi obsahu.</span><span class="sxs-lookup"><span data-stu-id="f0389-137">Then, use established SQL Server best practices toomigrate hello content database toohello StorSimple device.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="f0389-138">Přesunutí hello databázi obsahu toohello zařízení je podporována pouze pro řadu hello StorSimple 8000 (není podporována pro řadu hello 5000 a 7000).</span><span class="sxs-lookup"><span data-stu-id="f0389-138">Moving hello content database toohello device is only supported for hello StorSimple 8000 series (it is not supported for hello 5000 or 7000 series).</span></span>
   
   <span data-ttu-id="f0389-139">Pokud ukládáte objekty BLOB a hello databáze obsahu v samostatných svazcích na zařízení StorSimple hello, doporučujeme je nakonfigurovat v hello stejné kontejneru svazků.</span><span class="sxs-lookup"><span data-stu-id="f0389-139">If you store BLOBs and hello content database in separate volumes on hello StorSimple device, we recommend that you configure them in hello same volume container.</span></span> <span data-ttu-id="f0389-140">Tím se zajistí, že se budou zálohovány společně.</span><span class="sxs-lookup"><span data-stu-id="f0389-140">This ensures that they will be backed up together.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="f0389-141">Pokud jste nepovolili RBS, nedoporučujeme přesunutí zařízení StorSimple toohello hello databázi obsahu.</span><span class="sxs-lookup"><span data-stu-id="f0389-141">If you have not enabled RBS, we do not recommend moving hello content database toohello StorSimple device.</span></span> <span data-ttu-id="f0389-142">Toto je netestované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f0389-142">This is an untested configuration.</span></span>
   
9. <span data-ttu-id="f0389-143">Dalším krokem přejděte toohello: [konfigurace uvolňování paměti](#configure-garbage-collection).</span><span class="sxs-lookup"><span data-stu-id="f0389-143">Go toohello next step: [Configure garbage collection](#configure-garbage-collection).</span></span>

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
