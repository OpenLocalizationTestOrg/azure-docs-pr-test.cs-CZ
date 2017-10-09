<!--author=SharS last changed: 9/17/15-->

V tomto postupu budete:

1. [Příprava toorun hello Funkce Maintainer spustitelný soubor](#to-prepare-to-run-the-maintainer) .
2. [Příprava databáze obsahu hello a Koš pro okamžité odstranění objektů BLOB osamocené](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Spustit Maintainer.exe](#to-run-the-maintainer).
4. [Obnovit databázi obsahu hello a nastavení koše](#to-revert-the-content-database-and-recycle-bin-settings).

#### <a name="tooprepare-toorun-hello-maintainer"></a>tooprepare toorun hello Funkce Maintainer
1. Na hello webovém serveru front-end otevřete hello SharePoint 2013 Management Shell jako správce.
2. Přejděte na složku toohello *spouštěcí jednotka*: \Program Files\Microsoft 10.50\Maintainer SQL vzdáleného úložiště objektů Blob\.
3. Přejmenování **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** příliš**web.config**.
4. Použití `aspnet_regiis -pdf connectionStrings` souboru web.config toodecrypt hello.
5. V souboru web.config dešifrovaný hello, v části hello `connectionStrings` uzlu, přidejte hello připojovací řetězec pro vaše instance systému SQL server a název databáze obsahu hello. Viz následující ukázka hello.
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Použití `aspnet_regiis –pef connectionStrings` toore-zašifrování souboru web.config hello. 
7. Přejmenujte soubor web.config tooMicrosoft.Data.SqlRemoteBlobs.Maintainer.exe.config. 

#### <a name="tooprepare-hello-content-database-and-recycle-bin-tooimmediately-delete-orphaned-blobs"></a>databázi obsahu hello tooprepare a Koš tooimmediately odstranění osamocených objektů BLOB
1. Na hello systému SQL Server v SQL Management Studio spusťte následující aktualizace dotazů pro databázi obsahu cíl hello hello: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. Na hello webový front-end server, v části **centrální správy**, upravit hello **obecné nastavení webové aplikace** pro hello požadovaného obsahu databáze tootemporarily zakázat hello Koš. Tato akce odstraní také prázdný hello Koš pro všechny kolekce příslušné lokalitě. toodo tento, klikněte na tlačítko **centrální správy** -> **Správa aplikací** -> **webových aplikací (Správa webové aplikace)**  ->  **SharePoint - 80** -> **obecné nastavení aplikace**. Sada hello **Stav koše** příliš**OFF**.
   
    ![Obecné nastavení webové aplikace](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="toorun-hello-maintainer"></a>toorun hello Funkce Maintainer
* Na hello webovém serveru front-end v hello SharePoint 2013 Management Shell, spusťte hello Funkce Maintainer následujícím způsobem:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Pouze hello `GarbageCollection` operace je podporována pro zařízení StorSimple v tuto chvíli. Všimněte si také, zda jsou parametry hello vydán pro Microsoft.Data.SqlRemoteBlobs.Maintainer.exe velká a malá písmena. 
  > 
  > 

#### <a name="toorevert-hello-content-database-and-recycle-bin-settings"></a>databázi obsahu hello toorevert a nastavení koše
1. Na hello systému SQL Server v SQL Management Studio spusťte následující aktualizace dotazů pro databázi obsahu cíl hello hello:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Na hello webovém serveru front-end v **centrální správy**, upravit hello **obecné nastavení webové aplikace** pro hello potřeby databáze obsahu toore povolit hello Koš. toodo tento, klikněte na tlačítko **centrální správy** -> **Správa aplikací** -> **webových aplikací (Správa webové aplikace)**  ->  **SharePoint - 80** -> **obecné nastavení aplikace**. Nastavit stav koše hello příliš**ON**.

