---
title: aaaHow tooMigrate a publikovat webovou aplikaci tooan Azure Cloud Service ze sady Visual Studio | Microsoft Docs
description: "Zjistěte, jak toomigrate a publikování vaší webové aplikace tooan cloudové služby Azure pomocí sady Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9394adfd-a645-4664-9354-dd5df08e8c91
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: a2832c37d2ebdbc1e69a307d16b65b1c87e9070c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-migrate-and-publish-a-web-application-tooan-azure-cloud-service-from-visual-studio"></a>Postupy: migrace a publikovat webovou aplikaci tooan Cloudová služba Azure ze sady Visual Studio
tootake výhod hello hostitelských služeb a škálovatelnost Azure, může být vhodné toomigrate a publikování webové aplikace tooan cloudové služby Azure. Webovou aplikaci můžete spustit v Azure s minimálními změnami tooyour stávající aplikaci.

> [!NOTE]
> Toto téma se věnuje nasazení toocloud služeb, není tooweb lokalit. Informace o nasazení tooweb lokality najdete v tématu [nasazení webové aplikace v Azure App Service](app-service-web/web-sites-deploy.md).
>
>

Seznam konkrétních šablon, které jsou podporovány pro obě Visual C# a Visual Basic, najdete v tématu hello **podporované šablony projektů** dál v tomto tématu.

Nejprve je nutné povolit webové aplikace pro Azure ze sady Visual Studio. Hello následující obrázek znázorňuje hello klíčové kroky toopublish stávající webovou aplikaci můžete přidat projektu Azure toouse pro nasazení. Tento proces se přidá projektu Azure s hello požadované webové role tooyour řešení. Na základě hello typu webový projekt, který máte, hello vlastnosti projektu pro sestavení jsou také aktualizovat Pokud balíček služby hello vyžaduje další sestavení pro nasazení.

![Publikování tooMicrosoft webové aplikace Azure](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

> [!NOTE]
> Hello **převést**, **převést tooAzure projekt cloudové služby** příkazu se zobrazí pouze pro webový projekt hello ve vašem řešení. Například příkaz hello není k dispozici pro projekt Silverlight ve vašem řešení.
> Když vytvoříte balíček služby nebo publikování tooAzure vaší aplikace, může dojít k upozornění nebo chyby. Tyto chyby a varování můžete před nasazením tooAzure opravit problémy. Například může zobrazit upozornění na chybějící sestavení. Další informace o tom, jak tootreat žádné upozornění jako chyby, najdete v části [konfigurace projektu Azure Cloud Service pomocí sady Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Je-li sestavit aplikaci, spusťte ho místně pomocí emulátoru služby výpočty hello nebo ji publikovat tooAzure, může se zobrazit následující došlo k chybě v hello hello **seznam chyb** okno: **hello zadána cesta, název souboru, nebo obě jsou příliš dlouhé** . K této chybě dojde, protože délka hello hello Azure projektu plně kvalifikovaný název je příliš dlouhý. Délka Hello hello název projektu, včetně hello úplná cesta, nemůže být více než 146 znaků. To je třeba název úplné projektu hello včetně cesta k souboru pro Azure projekt, který je vytvořen pro aplikaci Silverlight: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Toomove může mít vaše řešení tooa jiný adresář, který má cesta tooreduce hello kratší hello projektu plně kvalifikovaný název.
>
>

toomigrate a publikovat webové aplikace tooAzure ze sady Visual Studio, postupujte podle těchto kroků.

## <a name="enable-a-web-application-for-deployment-tooazure"></a>Povolit webové aplikace pro nasazení tooAzure
### <a name="tooenable-a-web-application-for-deployment-tooazure"></a>tooenable webové aplikace pro nasazení tooAzure
1. tooenable webové aplikace pro nasazení tooAzure, otevřete hello místní nabídku pro webového projektu v řešení a zvolte Přidat projekt nasazení Azure.

    dojde k Hello následující akce:

   * Volá se projektu Azure `<name of hello web project>.Azure` je přidána toohello řešení pro vaši aplikaci.
   * Webové role pro webový projekt hello se přidá toothis projektu Azure.
   * Hello **místní kopie** vlastnost tootrue nastavena pro všechny sestavení, které jsou požadovány pro MVC 2, MVC 3, MVC 4 a Silverlight obchodních aplikací. Tento postup přidá tyto balíček služby toohello sestavení, který se používá pro nasazení.

   > [!IMPORTANT]
   > Pokud máte další sestavení nebo soubory, které jsou požadovány pro tuto webovou aplikaci, musíte ručně nastavit hello vlastnosti pro tyto soubory. Informace o tom, jak tooset těchto vlastností najdete v části hello **souborů v balíčku služby hello** dále v tomto článku.
   >
   > [!NOTE]
   > Pokud pro konkrétní webový projekt webové role již existuje v projektu Azure v řešení hello **převést**, **převést tooAzure projekt cloudové služby** se nezobrazí v místní nabídce hello pro tímto webovým projektem .
   >
   >

   Pokud máte více webových projektů ve webové aplikaci a chcete toocreate webové role pro každý webový projekt, musíte provést kroky hello v tomto postupu pro každý webový projekt. Tím se vytvoří samostatný Azure projekty pro každou webovou roli. Každý webový projekt můžete publikovat samostatně. Alternativně můžete ručně přidat jiný projekt webové role tooan existující Azure ve webové aplikaci. toodo této, otevřete hello místní nabídky pro hello **role** v projektu Azure vyberte **přidat**, pak **projekt webové Role v řešení**, zvolte hello tooadd projektu jako webové role a potom zvolte hello **OK** tlačítko.

## <a name="use-an-azure-sql-database-for-your-application"></a>Použít databázi Azure SQL pro vaši aplikaci
Pokud máte připojovací řetězec pro svoji webovou aplikaci, která používá databázi systému SQL Server, který je místně hello, musíte změnit tento připojovací řetězec toouse instanci databáze SQL Azure který je hostitelem místo.

> [!IMPORTANT]
> Vaše předplatné musíte povolit toouse databáze SQL. Pokud vaše předplatné z hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), můžete určit, jaké služby poskytuje vašeho předplatného. Hello tyto pokyny platí toohello vydané [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885). Pokud používáte hello [portál Azure](http://portal.microsoft.com), toohello další postup vynechat.
>
>

### <a name="toouse-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>toouse instanci SQL databáze vaší webové role pro připojovací řetězec
1. toocreate instanci databáze SQL v hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), postupujte podle kroků hello v hello následujícího článku: [vytvořit databázi serveru SQL](http://go.microsoft.com/fwlink/?LinkId=225109).

   > [!NOTE]
   > Při nastavování hello pravidla brány firewall pro instanci SQL databáze, je nutné vybrat hello **povolit tomuto serveru jiné služby Azure tooaccess** zaškrtávací políčko.
   >
   >
2. toocreate na instanci databáze SQL toouse pro připojovací řetězec, postupujte podle kroků hello v další části hello v hello následujícího článku: [vytvoření databáze SQL](http://go.microsoft.com/fwlink/?LinkId=225110).
3. toocopy hello ADO.NET připojovací řetězec toouse pro připojovací řetězec, proveďte následující kroky v hello hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).  

   1. Zvolte hello **databáze** tlačítko a pak otevřete hello uzel pro předplatné hello používá toocreate instanci databáze SQL.
   2. toodisplay hello k dispozici instance databáze SQL, zvolte hello **databází SQL** uzlu.
   3. Vlastnosti hello toodisplay pro hello databázi, zvolte hello databáze. Hello **vlastnosti** zobrazení se zobrazí.

      > [!NOTE]
      > Pokud hello **vlastnosti** zobrazení se nezobrazí, bude pravděpodobně nutné tooopen ho pomocí hello oddělovač.
      >
      >
   4. toodisplay hello připojovací řetězce, vyberte další tooView tlačítko hello třemi tečkami (...).

      Hello **připojovací řetězce** zobrazí se dialogové okno.
   5. toocopy hello připojovací řetězec ADO.NET, zvýrazňování textu hello a zvolte hello klávesy Ctrl + C.
   6. Dialogové okno hello tooclose vyberte hello **Zavřít** tlačítko.
4. tooreplace hello připojení řetězce v toouse souboru web.config hello tuto instanci SQL databáze, otevřete soubor web.config hello, zvýrazněte hello existující připojovací řetězec a zvolte klávesy Ctrl + V hello. Hello připojovací řetězec ADO.NET pro hello instanci SQL databáze nahradí hello existující připojovací řetězec.
5. Musíte taky přidat parametr hello `MultipleActiveResultSets=True` toohello připojovací řetězec. Hello připojovací řetězec musí mít hello následující formát:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```
6. (Volitelné) Připojovací řetězec alternativní metoda toochanging hello přímo v souboru web.config hello je tooadd části do jednoho hello souborů transformace web.config, v závislosti na konfiguraci sestavení hello použít toocreate vašeho balíčku služby. Otevřete soubor Web.Debug.Config hello nebo soubor Web.Release.Config hello. Přidejte hello následující části do tohoto souboru:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```
7. Uložte soubor hello, kterou můžete upravit a znovu publikovat aplikace.

### <a name="toouse-an-instance-of-sql-database-by-using-hello-azure-classic-portal"></a>hello toouse instanci databáze SQL pomocí portálu Azure classic
1. V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), vyberte uzel hello databáze SQL.

   * Pokud se zobrazí hello instanci SQL databáze, které chcete toouse, zvolte tooopen ho.
   * Pokud jste nevytvořili žádné instance, vyberte příslušný odkaz hello a pak vytvořit instanci.
2. Po otevření nebo vytvořit instanci databáze, zvolte hello **připojovací řetězce** odkaz.
3. Na hello dolní části stránky hello vyberte nastavení brány firewall tooconfigure hello odkaz a přijměte výchozí hodnoty hello nebo nakonfigurujte hello hodnoty, které potřebujete.
4. Zkopírujte připojovací řetězec ADO.NET hello, vložte jej do souboru web.config přes hello staré připojovací řetězec pro hello místní databázi a mít jistotu tooadd `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-tooazure"></a>Publikování tooAzure webové aplikace
### <a name="toopublish-a-web-application-tooazure"></a>toopublish tooAzure webové aplikace
1. aplikace hello tootest v hello místní vývojové prostředí pomocí emulátoru služby výpočty Azure hello, otevřete hello místní nabídku pro hello Azure projektu pro hello webovou roli a vyberte **nastavit jako spouštěný projekt**. Zvolte **ladění**, **spustit ladění** (klávesové: **F5**).

    Hello **hello spuštění prostředí Azure ladění** otevře se dialogové okno a spuštění aplikace hello v prohlížeči hello. Pro konkrétní podrobnosti o tom, jak toostart každý typ webové aplikace v hello emulátor služby výpočty, najdete v části hello tabulky v této části.
2. tooset hello služeb pro vaše aplikace toopublish tooAzure, musí mít účet Microsoft a předplatné Azure. Hello použijte kroky v následující tématu tooset až vašim službám hello: [připravit toopublish nebo nasadit aplikaci Azure ze sady Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
3. tooAzure toopublish hello webové aplikace, otevřete hello místní nabídku pro hello webový projekt a zvolte **publikování tooAzure**.

    Hello **publikování aplikaci Azure** otevře se dialogové okno a Visual Studio spustí proces nasazení hello. Další informace o tom, jak toopublish hello aplikace, najdete v tématu hello **publikovat aplikaci Azure ze sady Visual Studio** v [publikování cloudové služby pomocí nástroje Azure hello](vs-azure-tools-publishing-a-cloud-service.md).

   > [!NOTE]
   > Můžete také publikovat hello webové aplikace z hello projektu Azure. toodo, otevřete hello místní nabídku pro hello projektu Azure a zvolte **publikovat**.
   >
   >
4. průběh hello toosee hello nasazení, můžete zobrazit hello **protokol činnosti Azure** okno. Tento protokol se automaticky zobrazí při spuštění procesu nasazení hello. Můžete rozbalit hello řádku položky v protokolu aktivit hello tooshow podrobné informace, jak ukazuje následující obrázek hello:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)
5. Proces nasazení hello (volitelné) toocancel, otevřete hello místní nabídky hello řádku položky v protokolu aktivit hello a zvolte **zrušit a odeberte**. To zastaví proces nasazení hello a odstraní prostředí nasazení hello z Azure.

   > [!NOTE]
   > tooremove bylo toto nasazení prostředí po jeho nasazení, je nutné použít hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
   >
   >
6. (Volitelné) Po spuštění instance role mít Visual Studio automaticky zobrazí hello nasazení prostředí v hello **Azure Compute** uzlu v **Průzkumník cloudu** nebo **Průzkumníka serveru**. Odtud můžete zobrazit stav hello instancí hello jednotlivé role.

    Hello následující obrázek znázorňuje hello instancí rolí ve **Průzkumníka serveru** době, kdy jsou stále v hello Probíhá inicializace stavu:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)
7. tooaccess aplikace po nasazení, vyberte hello šipku další tooyour nasazení při stav **dokončeno** se zobrazí v hello **protokol činnosti Azure**. Zobrazí se hello adresu URL pro webovou aplikaci v Azure. Viz následující tabulka hello podrobnosti o tom, jak toostart konkrétní typ webové aplikace z Azure hello.

    Hello následující tabulka uvádí hello podrobnosti o tom, jak toostart konkrétní webové aplikace z Azure nebo toorun nebo ladění webové aplikace místně pomocí hello výpočetní emulátor Azure:

   | Typ webové aplikace | Spustit/Debug místně pomocí hello výpočetní emulátor | spuštění v Azure |
   | --- | --- | --- |
   | Webová aplikace ASP.NET |V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Zvolte hello adresa URL hypertextový odkaz zobrazí v hello **nasazení** karta hello **protokol činnosti Azure** tooload hello úvodní stránku v prohlížeči hello. |
   | Webová aplikace ASP.NET MVC 2 |V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Zvolte hello adresa URL hypertextový odkaz zobrazí v hello **nasazení** karta hello **protokol činnosti Azure** tooload hello úvodní stránku v prohlížeči hello. |
   | Webové aplikace ASP.NET MVC 3 |V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Zvolte hello adresa URL hypertextový odkaz zobrazí v hello **nasazení** karta hello **protokol činnosti Azure** tooload hello úvodní stránku v prohlížeči hello. |
   | Webové aplikace ASP.NET MVC 4 |V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Zvolte hello adresa URL hypertextový odkaz zobrazí v hello **nasazení** karta hello **protokol činnosti Azure** tooload hello úvodní stránku v prohlížeči hello. |
   | Prázdná webová aplikace ASP.NET |Je nutné přidat stránku .aspx ve vaší aplikaci, která nastavíte jako hello úvodní stránky pro webový projekt. V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Pokud máte výchozí stránku .aspx ve vaší aplikaci, vyberte hello adresa URL hypertextový odkaz zobrazí v hello **nasazení** karta hello **protokol činnosti Azure** a načte tuto stránku v prohlížeči hello. Pokud máte jiný .aspx stránky, je třeba toonavigate toothis konkrétní stránky pomocí hello následující formát pro adresu url:`<url for deployment>/<name of page>.aspx` |
   | Aplikace Silverlight |V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Je třeba toonavigate toohello konkrétní stránky pro vaši aplikaci pomocí hello následující formát pro adresu url:`<url for deployment>/<name of page>.aspx` |
   | Obchodní aplikace Silverlight |V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Je třeba toonavigate toohello konkrétní stránky pro vaši aplikaci pomocí hello následující formát pro adresu url:`<url for deployment>/<name of page>.aspx` |
   | Navigace aplikace Silverlight |V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Je třeba toonavigate toohello konkrétní stránky pro vaši aplikaci pomocí hello následující formát pro adresu url:`<url for deployment>/<name of page>.aspx` |
   | Aplikace služby WCF |Je třeba nastavit soubor .svc hello jako hello úvodní stránka pro svůj projekt služby WCF. V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Budete potřebovat soubor svc toohello toonavigate pro vaši aplikaci pomocí hello následující formát pro adresu url:`<url for deployment>/<name of service file>.svc` |
   | Aplikace služby pracovního postupu WCF |Je třeba nastavit soubor .svc hello jako hello úvodní stránka pro svůj projekt služby WCF. V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Budete potřebovat soubor svc toohello toonavigate pro vaši aplikaci pomocí hello následující formát pro adresu url:`<url for deployment>/<name of service file>.svc` |
   | Dynamické ASP.NET Entity |V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Je nutné aktualizovat připojovací řetězec hello (viz další část). Musíte taky toonavigate toohello konkrétní stránky pro vaši aplikaci pomocí hello následující formát pro adresu url:`<url for deployment>/<name of page>.aspx` |
   | TooSQL Linq Dynamická Data technologie ASP.NET |V řádku nabídek hello zvolte **ladění**, **spustit ladění** (klávesové: Zvolte hello **F5** klíč.). |Hello kroky v tomto postupu je třeba provést: použít databázi SQL Azure pro aplikaci (viz výše uvedené části v tomto tématu). Musíte taky toonavigate toohello konkrétní stránky pro vaši aplikaci pomocí hello následující formát pro adresu url:`<url for deployment>/<name of page>.aspx` |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Aktualizovat připojovací řetězec pro ASP.NET dynamické entity
### <a name="tooupdate-a-connection-string-for-aspnet-dynamic-entities"></a>tooUpdate připojovací řetězec pro dynamické entity ASP.NET
1. toocreate databázi SQL Azure, která lze použít pro webovou aplikaci ASP.NET dynamické entity, postupujte podle kroků hello v postupu hello **použít databázi SQL Azure pro vaši aplikaci** výše v tomto tématu.
2. Přidání hello tabulek a polí, které je nutné pro tuto databázi z hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
3. Hello připojovací řetězec pro tento typ aplikace má hello formátu v souboru web.config hello:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Aktualizace hello *connectionString* hodnotu s hello připojovací řetězec ADO.NET pro vaši databázi SQL Azure následujícím způsobem:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```
4. soubor web.config hello toosave s hello změny, které jste provedli toohello připojovacího řetězce v řádku nabídek hello zvolte **soubor**, **uložit soubor web.config**.

## <a name="supported-project-templates"></a>Šablony projektů podporované
toopublish tooAzure aplikace webové aplikace hello musí používat jednu z hello šablony projektů pro C# nebo Visual Basic, který je uvedený v tabulce hello.

| Skupina šablon projektu | Šablona projektu |
| --- | --- |
| Web |Webová aplikace ASP.NET |
| Web |Webová aplikace ASP.NET MVC 2 |
| Web |Webové aplikace ASP.NET MVC 3 |
| Web |Webové aplikace ASP.NET MVC4 |
| Web |Prázdná webová aplikace ASP.NET |
| Web |Prázdná webová aplikace ASP.NET MVC 2 |
| Web |Webovou aplikaci ASP.NET dynamické dat entity |
| Web |TooSQL Linq Dynamická Data technologie ASP.NET webové aplikace |
| Silverlight |Aplikace Silverlight |
| Silverlight |Obchodní aplikace Silverlight |
| Silverlight |Navigace aplikace Silverlight |
| WCF |Aplikace služby WCF |
| WCF |Aplikace služby pracovního postupu WCF |
| Pracovní postup |Aplikace služby pracovního postupu WCF |

## <a name="next-steps"></a>Další kroky
Další informace o publikování najdete v tématu [připravit tooPublish nebo nasazení aplikace Azure ze sady Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Také si přečtěte [nastavení až s názvem ověřování pověření](vs-azure-tools-setting-up-named-authentication-credentials.md).
