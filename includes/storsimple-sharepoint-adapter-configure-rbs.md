<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> Při provádění změn toohello StorSimple adaptéru pro konfiguraci RBS služby SharePoint, musíte být přihlášeni pomocí uživatelského účtu, který je součástí skupiny Domain Admins toohello. Konfigurační stránku hello musí navíc přístup z prohlížeče systémem hello stejné hostitele jako centrální správy.
> 
> 

#### <a name="tooconfigure-rbs"></a>tooconfigure RBS
1. Otevřete stránku hello Centrální správa SharePoint a procházet příliš**nastavení systému**. 
2. V hello **Azure StorSimple** klikněte na tlačítko **nakonfigurovat adaptér StorSimple**.
   
    ![Konfigurace hello adaptér StorSimple](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. Na hello **nakonfigurovat adaptér StorSimple** stránky:
   
   1. Ujistěte se, že hello **povolení úprav cesta** je zaškrtnuté políčko.
   2. Hello textového pole zadejte cestu Universal Naming Convention (UNC) hello hello úložišti objektů BLOB.
      
      > [!NOTE]
      > svazek úložiště objektů BLOB Hello musí být hostované na jednotce iSCSI nakonfigurovat v zařízení StorSimple hello.

   3. Klikněte na tlačítko hello **povolit** tlačítko pod jednotlivých databází obsahu hello chcete tooconfigure pro vzdálené úložiště.
      
      > [!NOTE]
      > úložiště objektů BLOB Hello musí být sdílené a dostupné všechny (WFE) servery front-end webové a hello uživatelský účet, který je nakonfigurován pro serverové farmy služby SharePoint hello musí mít přístup toohello sdílené složky.
      
      ![Povolit zprostředkovatele RBS hello](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      Když povolíte nebo zakážete RBS, zobrazí se také hello následující zprávou.
      
      ![Konfigurace zařízení StorSimple adaptéru povolit zakázat](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. Klikněte na tlačítko hello **aktualizace** tlačítko tooapply hello konfigurace. Když kliknete na tlačítko hello **aktualizace** tlačítko hello stav konfigurace RBS bude aktualizována na všech serverech WFE a celou farmu hello budou RBS povolena. Zobrazí se následující zpráva Hello.
      
      ![Zpráva Konfigurace adaptéru](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > Pokud konfigurujete RBS pro farmu služby SharePoint s velmi velký počet databází (větší než 200), webovou stránku hello Centrální správa SharePoint může vypršení časového limitu. Pokud k tomu dojde, aktualizujte stránku hello. Proces konfigurace hello to nemá vliv.

4. Ověření konfigurace hello:
   
   1. Přihlaste se na webu Centrální správa SharePoint toohello a procházet toohello **nakonfigurovat adaptér StorSimple** stránky.
   2. Zkontrolujte toomake podrobnosti konfigurace hello se, že budou odpovídat hello nastavení, které jste zadali. 
5. Ověřte, že RBS funguje správně:
   
   1. Nahrajte tooSharePoint dokumentu. 
   2. Procházejte cestu UNC toohello, který jste nakonfigurovali. Ujistěte se, zda byl vytvořen hello RBS adresářovou strukturu a obsahuje hello nahrán objekt.
6. (Volitelné) Můžete použít hello Microsoft RBS `Migrate()` rutiny prostředí PowerShell, které jsou součástí služby SharePoint toomigrate existující objekt BLOB obsahu toohello zařízení StorSimple. Další informace najdete v tématu [migraci obsahu do nebo z RBS v SharePoint 2013] [ 6] nebo [migraci obsahu do nebo z RBS (SharePoint Foundation 2010)] [7].
7. (Volitelné) Na zkušební instalace můžete ověřit, že byly objekty BLOB hello přesunuty z databáze obsahu hello: 
   
   1. Spusťte SQL Management Studio.
   2. Spusťte dotaz hello ListBlobsInDB_2010.sql nebo ListBlobsInDB_2013.sql, následujícím způsobem.
      
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
      
      Pokud RBS bylo správně nakonfigurované, by se zobrazit hodnotu NULL ve sloupci hello SizeOfContentInDB pro libovolný objekt, který byl odeslán a úspěšně externalized s RBS.
8. (Volitelné) Po konfiguraci RBS a přesunout všech objektů BLOB obsahu toohello zařízení StorSimple, můžete přesunout hello databázi obsahu toohello zařízení. Pokud si zvolíte toomove hello obsahu databáze, doporučujeme nakonfigurovat hello databázi obsahu úložiště na zařízení hello jako primární svazek. Pak použijte navázat, že SQL Server osvědčených postupů zařízení StorSimple pro toohello toomigrate hello databázi obsahu. 
   
   > [!NOTE]
   > Přesunutí hello databázi obsahu toohello zařízení je podporována pouze pro řadu hello StorSimple 8000 (není podporována pro řadu hello 5000 a 7000).
   
   Pokud ukládáte objekty BLOB a hello databáze obsahu v samostatných svazcích na zařízení StorSimple hello, doporučujeme je nakonfigurovat v hello stejné kontejneru svazků. Tím se zajistí, že se budou zálohovány společně.
   
   > [!WARNING]
   > Pokud jste nepovolili RBS, nedoporučujeme přesunutí zařízení StorSimple toohello hello databázi obsahu. Toto je netestované konfigurace.
   
9. Dalším krokem přejděte toohello: [konfigurace uvolňování paměti](#configure-garbage-collection).

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
