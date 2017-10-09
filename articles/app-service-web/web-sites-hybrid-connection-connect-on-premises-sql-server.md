---
title: "aaaConnect tooon místnímu SQL serveru z webové aplikace v Azure App Service pomocí hybridních připojení"
description: "Vytvoření webové aplikace v Microsoft Azure a připojte ho tooan, místní databázi systému SQL Server"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 2b4e0539-1a0b-4aa1-8a69-b4b053c3b2e5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: cephalin
ms.openlocfilehash: 2e8f8f7e0b9733cfb0433697615faba4358c6023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Připojit tooon místnímu SQL serveru z webové aplikace v Azure App Service pomocí hybridních připojení
Hybridní připojení se můžete připojit [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) prostředky tooon místní webové aplikace, které používají statický port TCP. Podporované prostředky zahrnují Microsoft SQL Server, MySQL, webové rozhraní API HTTP, služby App Service a většina vlastních webových služeb.

V tomto kurzu se dozvíte, jak toocreate aplikační službu webové aplikace v hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715)se připojte hello webové aplikace tooyour místní databázi systému SQL Server pomocí nové funkce hybridní připojení hello, vytvořit jednoduché ASP.NET aplikace, který bude používat hello hybridní připojení a nasadit aplikace toohello hello webové aplikace App Service. Hello dokončit webové aplikace v Azure uloží v databázi členství, která je místní přihlašovací údaje uživatele. Hello kurz předpokládá žádné předchozí zkušenosti s používáním Azure nebo v technologii ASP.NET.

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> část webové aplikace Hello hello hybridní připojení funkce je k dispozici pouze v hello [portálu Azure](https://portal.azure.com). toocreate připojení ve službě BizTalk Services najdete v části [hybridní připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274).  
> 
> 

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat hello následující produkty. Všechny jsou k dispozici v bezplatné verze, abyste mohli začít, vývoj pro Azure zcela zdarma.

* **Předplatné Azure** – bezplatné předplatné, najdete v části [bezplatná zkušební verze Azure](/pricing/free-trial/).
* **Visual Studio 2013** -toodownload bezplatná zkušební verze produktu Visual Studio 2013, najdete v části [Visual Studio stáhne](http://www.visualstudio.com/downloads/download-visual-studio-vs). Než budete pokračovat, nainstalujte tento.
* **Rozhraní Microsoft .NET Framework 3.5 Service Pack 1** – Pokud je váš operační systém Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 nebo Windows Server 2008 R2, můžete ho povolit v Ovládacích panelech > programy a funkce > zapnout Funkce systému Windows nebo vypnout. Jinak, můžete ji stáhnout z hello [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).
* **SQL Server 2014 Express with Tools** -Microsoft SQL Server Express si zdarma stáhněte v hello [databáze platforma Microsoft webové stránky](http://www.microsoft.com/web/platform/database.aspx). Zvolte hello **Express** (ne LocalDB) verze. Hello **Express with Tools** verze zahrnuje SQL Server Management Studio, které budete používat v tomto kurzu.
* **SQL Server Management Studio Express** – je součástí systému SQL Server 2014 Express hello nástroje pro stažení výše uvedených, ale pokud budete potřebovat tooinstall ho samostatně, můžete stáhnout a nainstalovat z hello [SQL Server Express stránce pro stažení](http://www.microsoft.com/web/platform/database.aspx).

Hello kurz předpokládá, že máte předplatné Azure, jestli máte nainstalovanou sadu Visual Studio 2013, a že jste nainstalovali nebo povoleno rozhraní .NET Framework 3.5. Hello kurzu se dozvíte, jak funkce tooinstall SQL Server 2014 Express v konfiguraci, který pracuje s hello Azure hybridní připojení (výchozí instanci s statický port TCP). Před zahájením hello kurzu, stáhněte z umístění hello uvedených výše, pokud nemáte nainstalovaný server SQL Server SQL Server 2014 Express with Tools.

### <a name="notes"></a>Poznámky
toouse místní SQL Server nebo SQL Server Express databáze s hybridní připojení TCP/IP musí toobe povoleno na statický port. Výchozí instance na serveru SQL Server používají statický port 1433, zatímco pojmenovaných instancí nepodporují.

Hello počítač, na který instalujete hello místní správce hybridního připojení agenta:

* Musí mít tooAzure odchozí připojení přes:

| Port | Proč |
| --- | --- |
| 80 |**Požadované** pro port HTTP pro ověření certifikátu a volitelně pro datové připojení. |
| 443 |**Volitelné** pro datové připojení. Pokud too443 odchozí připojení není k dispozici, použije se TCP port 80. |
| 5671 a 9352 |**Doporučená** ale volitelné pro datové připojení. Všimněte si, že tento režim obvykle dává vyšší propustnost. Pokud porty toothese odchozí připojení není k dispozici, použije se port 443 protokolu TCP. |

* Musí být schopný tooreach hello *hostname*:*ČísloPortu* vaše místní prostředku.

Hello postup v tomto článku předpokládá, že používáte prohlížeč hello z hello počítače, který bude hostitelem hello místní hybridní připojení agenta.

Pokud už máte nainstalovaný v konfiguraci a v prostředí, které splňuje podmínky hello popsané výše server SQL Server, můžete přeskočit a začínat [vytvořit databázi systému SQL Server na místě](#CreateSQLDB).

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>A. Nainstalujte systém SQL Server Express, povolte protokol TCP/IP a vytvořit databázi systému SQL Server na místě
V této části se dozvíte, jak tooinstall SQL Server Express, povolte protokol TCP/IP a vytvořit databázi tak, aby webová aplikace bude fungovat s hello portálu Azure.

### <a name="install-sql-server-express"></a>Nainstalujte SQL Server Express.
1. SQL Server Express, tooinstall spustit hello **SQLEXPRWT_x64_ENU.exe** nebo **SQLEXPR_x86_ENU.exe** soubor, který jste stáhli. Zobrazí se Průvodce Hello Centrum instalace SQL serveru.
   
    ![Instalace serveru SQL][SQLServerInstall]
2. Zvolte **nová instalace SQL serveru samostatné, nebo přidejte existující instalace funkce tooan**. Postupujte podle pokynů hello, přijetí hello výchozí možnosti a nastavení, dokud nezískáte toohello **konfigurace Instance** stránky.
3. Na hello **konfigurace Instance** vyberte **výchozí instance**.
   
    ![Vyberte výchozí instanci][ChooseDefaultInstance]
   
    Ve výchozím nastavení hello výchozí instance systému SQL Server čeká na požadavky od klientů systému SQL Server na statický port 1433, což je co hello hybridní připojení funkce vyžaduje. Pojmenované instance používají dynamické porty a UDP, které nejsou podporovány hybridní připojení.
4. Přijměte výchozí hodnoty hello na hello **konfigurace serveru** stránky.
5. Na hello **konfigurace databázového stroje** v části **režim ověřování**, zvolte **smíšený režim (ověřování systému SQL Server a ověřování systému Windows)**a zadejte heslo.
   
    ![Zvolte ve smíšeném režimu][ChooseMixedMode]
   
    V tomto kurzu budete používat ověřování serveru SQL Server. Být jisti tooremember hello heslo, které zadáte, protože jej budete potřebovat později.
6. Projděte hello zbytek hello Průvodce toocomplete hello instalace.

### <a name="enable-tcpip"></a>Povolte protokol TCP/IP
tooenable TCP/IP, budete používat SQL Server Configuration Manager, který byl nainstalován při instalaci systému SQL Server Express. Postupujte podle kroků hello v [povolit síťový protokol TCP/IP pro SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) než budete pokračovat.

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a>Vytvořit databázi systému SQL Server na místě
Webové aplikace Visual Studio vyžaduje členství v databázi, která je přístupná v Azure. To vyžaduje databázi systému SQL Server nebo SQL Server Express (ne hello LocalDB databáze hello používá šablony MVC ve výchozím nastavení), takže vytvoříte databázi členství hello Další.

1. V systému SQL Server Management Studio připojte toohello systému SQL Server, kterou jste právě nainstalovali. (Pokud hello **připojit tooServer** dialogové okno nezobrazí automaticky, přejděte příliš**Průzkumník objektů** v levém podokně hello, klikněte na **Connect**a potom klikněte na **Databáze modul**.) ![Připojit tooServer][SSMSConnectToServer]
   
    Pro **typ serveru**, zvolte **databázový stroj**. Pro **název serveru**, můžete použít **localhost** nebo hello název hello počítače, který používáte. Zvolte **ověřování serveru SQL Server**a potom se přihlaste se pomocí hello sa uživatelské jméno a heslo hello, kterou jste vytvořili dříve.
2. toocreate novou databázi pomocí SQL Server Management Studio, klikněte pravým tlačítkem na **databáze** v Průzkumníku objektů a potom klikněte na **novou databázi**.
   
    ![Vytvoření nové databáze][SSMScreateNewDB]
3. V hello **novou databázi** dialogové okno, zadejte MembershipDB pro hello název databáze a pak klikněte na tlačítko **OK**.
   
    ![Zadejte název databáze][SSMSprovideDBname]
   
    Poznámka: všechny změny toohello databáze v tuto chvíli Nedělejte. informace o členství Hello budou automaticky později přidány webovou aplikací, když jej spustíte.
4. V Průzkumníku objektů, pokud rozbalíte **databáze**, zobrazí se, že hello členství databáze byla vytvořena.
   
    ![MembershipDB vytvořen][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-hello-azure-portal"></a>B. Vytvoření webové aplikace v hello portálu Azure
> [!NOTE]
> Pokud jste již vytvořili webovou aplikaci v hello portálu Azure, že chcete toouse pro účely tohoto kurzu, můžete přeskočit příliš[vytvořit hybridní připojení a služby BizTalk](#CreateHC) a pokračovat dál.
> 
> 

1. V hello [portálu Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.
   
    ![tlačítko Nový][New]
2. Konfigurace webové aplikace a pak klikněte na **vytvořit**.
   
    ![Název webu][WebsiteCreationBlade]
3. Po chvíli se hello webové aplikace se vytvoří a otevře se okno jeho webové aplikace. okno Hello se svisle posouvatelným řídicí panel, který vám umožní spravovat webové aplikace.
   
    ![Spuštění webu][WebSiteRunningBlade]
   
    tooverify hello webová aplikace je za provozu, můžete kliknout na hello **Procházet** ikonu toodisplay hello výchozí stránky.

V dalším kroku vytvoříte hybridní připojení a služby BizTalk pro hello webovou aplikaci.

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. Vytvoření hybridní připojení a služby BizTalk
1. Zpět v hello portálu, přejděte toosettings a klikněte na tlačítko **sítě** > **nakonfigurovat koncové body hybridního připojení**.
   
    ![Hybridní připojení][CreateHCHCIcon]
2. V okně připojení hybridní hello, klikněte na tlačítko **přidat** > **nové hybridní připojení**.
3. Na hello **vytvořit hybridní připojení** okno:
   
   * Pro **název**, zadejte název připojení hello.
   * Pro **Hostname**, zadejte název počítače hello hostitelského počítače systému SQL Server.
   * Pro **Port**, zadejte 1433 (hello výchozí port pro SQL Server).
   * Klikněte na tlačítko **služby BizTalk** > **novou službu BizTalk** a zadejte název pro hello služby BizTalk.
     
     ![Vytvořit hybridní připojení.][TwinCreateHCBlades]
4. Klikněte na tlačítko **OK** dvakrát.
   
    Když hello dokončení procesu hello **oznámení** oblasti blikat zelená **úspěch** a hello **hybridní připojení** okno se zobrazí hello nové hybridní připojení s Hello stav jako **Nepřipojeno**.
   
    ![Vytvoření jednoho hybridní připojení][CreateHCOneConnectionCreated]

V tomto okamžiku jste dokončili důležitou součástí infrastruktury hello cloudu hybridní připojení. V dalším kroku vytvoříte odpovídající část místně.

<a name="InstallHCM"></a>

## <a name="d-install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a>D. Instalace hello místní správce hybridního připojení toocomplete hello připojení
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Teď dokončení tuto infrastrukturu hello hybridní připojení se vytvoří webovou aplikaci, která jej používá.

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-hello-database-connection-string-and-run-hello-project-locally"></a>E. Vytvořit základní projekt ASP.NET, upravit připojovací řetězec databáze hello a spustit projekt hello místně
### <a name="create-a-basic-aspnet-project"></a>Vytvoření základního projektu ASP.NET
1. V sadě Visual Studio na hello **souboru** nabídky, vytvořte nový projekt:
   
    ![Nový projekt Visual Studio][HCVSNewProject]
2. V hello **šablony** části hello **nový projekt** dialogovém okně, vyberte **webové** a zvolte **webové aplikace ASP.NET**a pak klikněte na tlačítko  **OK**.
   
    ![Vyberte webové aplikace ASP.NET][HCVSChooseASPNET]
3. V hello **nový projekt ASP.NET** dialogovém okně, vyberte **MVC**a potom klikněte na **OK**.
   
    ![Zvolte MVC][HCVSChooseMVC]
4. Po vytvoření projektu hello, zobrazí se stránka readme aplikace hello. Ještě nespouštějte hello webového projektu.
   
    ![Stránce Readme][HCVSReadmePage]

### <a name="edit-hello-database-connection-string-for-hello-application"></a>Upravit připojovací řetězec databáze hello aplikace hello
V tomto kroku upravíte hello připojovací řetězec, který sděluje aplikace kde toofind místní systém SQL Server Express databáze. Hello připojovací řetězec je v souboru Web.config aplikace hello, který obsahuje informace o konfiguraci pro aplikaci hello.

> [!NOTE]
> tooensure, které vaše aplikace používá hello databáze, kterou jste vytvořili v systému SQL Server Express a ne hello, jeden v sadě Visual Studio výchozí LocalDB, je důležité, provedení tohoto kroku před spuštěním projektu.
> 
> 

1. V Průzkumníku řešení poklikejte na soubor Web.config hello.
   
    ![Soubor web.config][HCVSChooseWebConfig]
2. Upravit hello **connectionStrings** části toopoint toohello databáze systému SQL Server na místním počítači, řiďte se syntaxí hello v hello následující ukázka:
   
    ![Připojovací řetězec][HCVSConnectionString]
   
    Při sestavování hello připojovací řetězec, mějte na paměti hello následující:
   
   * Pokud se připojujete tooa pojmenovanou instanci namísto výchozí instance (například YourServer\SQLEXPRESS), je nutné nakonfigurovat porty statické toouse vašeho systému SQL Server. Informace o konfiguraci statických portů najdete v tématu [jak tooconfigure toolisten systému SQL Server na určitém portu](http://support.microsoft.com/kb/823938). Ve výchozím nastavení používají pojmenované instance UDP a dynamické porty, které nejsou podporovány hybridní připojení.
   * Doporučuje se, že zadáváte hello portu (ve výchozím nastavení, jak je znázorněno v příkladu hello 1433) na hello připojovací řetězec, abyste se ujistili, že místní systém SQL Server má TCP povoleno a používá správný port hello.
   * Mějte na paměti, tooconnect toouse ověřování systému SQL Server, zadání hello ID uživatele a heslo v připojovacím řetězci.
3. Klikněte na tlačítko **Uložit** v souboru Web.config hello toosave Visual Studio.

### <a name="run-hello-project-locally-and-register-a-new-user"></a>Spusťte projekt hello místně a registraci nového uživatele
1. Teď spusťte místně nový webový projekt kliknutím na tlačítko Procházet hello pod ladění. Tento příklad používá aplikace Internet Explorer.
   
    ![Spusťte projekt][HCVSRunProject]
2. V horní pravé hello hello výchozí webové stránky, zvolte **zaregistrovat** tooregister nový účet:
   
    ![Zaregistrovat nový účet][HCVSRegisterLocally]
3. Zadejte uživatelské jméno a heslo:
   
    ![Zadejte uživatelské jméno a heslo][HCVSCreateNewAccount]
   
    Toto automaticky vytvoří databázi na vaše místní systém SQL Server, který obsahuje informace o členství hello pro vaši aplikaci. Jedna z tabulek hello (**dbo. AspNetUsers**) blokování webové aplikace jako hello ta, která jste zadali přihlašovací údaje uživatele. Zobrazí se tato tabulka později v kurzu hello.
4. Zavřete okno prohlížeče hello hello výchozí webové stránky. To zastaví hello aplikace v sadě Visual Studio.

Jsou nyní připraveny pro další krok text hello, což je tooAzure aplikace hello toopublish a otestovat.

<a name="PubNTest"></a>

## <a name="f-publish-hello-web-application-tooazure-and-test-it"></a>F. Publikování hello webové aplikace tooAzure a otestovat ji
Nyní, budete publikovat vaše aplikace tooyour webové aplikace App Service a otestovat ji toosee, jak jste nakonfigurovali dříve hello hybridní připojení je právě používané tooconnect databáze toohello webové aplikace na místním počítači.

### <a name="publish-hello-web-application"></a>Publikování webové aplikace hello
1. Můžete si stáhnout váš profil publikování pro hello webové aplikace App Service v hello portálu Azure. V okně hello pro vaši webovou aplikaci, klikněte na tlačítko **profilu publikování Get**a potom uložte hello souboru tooyour počítače.
   
    ![Stažení profilu publikování][PortalDownloadPublishProfile]
   
    Dále budete importovat tento soubor do webové aplikace Visual Studio.
2. V sadě Visual Studio, klikněte pravým tlačítkem na název projektu hello v Průzkumníku řešení a vyberte **publikovat**.
   
    ![Vyberte publikování][HCVSRightClickProjectSelectPublish]
3. V hello **Publikovat Web** dialog v hello **profil** , zvolte **Import**.
   
    ![Import][HCVSPublishWebDialogImport]
4. Procházet tooyour stáhnout profil publikování, vyberte ho a pak klikněte na tlačítko **OK**.
   
    ![Procházet tooprofile][HCVSBrowseToImportPubProfile]
5. Informace o publikování je naimportována a zobrazí v hello **připojení** kartě hello dialogového okna.
   
    ![Klikněte na tlačítko Publikovat][HCVSClickPublish]
   
    Klikněte na **Publikovat**.
   
    Po dokončení publikování bude váš prohlížeč spustit a zobrazit aplikace teď známé ASP.NET – s tím rozdílem, že je nyní za provozu v hello cloudu Azure!

V dalším kroku použijete vaše živou webovou aplikaci toosee jeho hybridní připojení v akci.

### <a name="test-hello-completed-web-application-on-azure"></a>Test hello dokončit webové aplikace v Azure
1. V horní části hello napravo od webové stránky v Azure, zvolte **přihlásit**.
   
    ![Test přihlášení][HCTestLogIn]
2. App Service, které se webová aplikace je teď připojené databáze členství tooyour webové aplikace na místním počítači. tooverify se přihlaste se pomocí stejných přihlašovacích údajů, které jste zadali v hello místní databáze dříve hello.
   
    ![Hello pozdravu][HCTestHelloContoso]
3. toofurther vyzkoušejte nové hybridní připojení, odhlaste se z Azure webové aplikace a zaregistrujte se jako jiný uživatel. Zadejte nové uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.
   
    ![Test zaregistrovat jiný uživatel][HCTestRegisterRelecloud]
4. tooverify že hello přihlašovací údaje nového uživatele byly uloženy v místní databázi prostřednictvím hybridní připojení, otevřete SQL Management Studio v místním počítači. V Průzkumníku objektů rozbalte hello **MembershipDB** databáze a potom rozbalte **tabulky**. Klikněte pravým tlačítkem na hello **dbo. AspNetUsers** členství tabulky a zvolte **řádky vyberte Top 1000** tooview hello výsledky.
   
    ![Zobrazení výsledků hello][HCTestSSMSTree]
5. Vaše tabulka členství v místní nyní zobrazuje oba účty - hello ten, který jste vytvořili místně a hello ten, který jste vytvořili v hello cloudu Azure. Hello ten, který jste vytvořili v cloudu hello se uložila tooyour místní databázi pomocí funkce Azure hybridní připojení.
   
    ![Registrovaní uživatelé v místní databázi][HCTestShowMemberDb]

Nyní jste vytvořili a nasazené webové aplikace ASP.NET, která používá hybridní připojení mezi webovou aplikaci v hello cloudu Azure a místní databázi systému SQL Server. Blahopřejeme!

## <a name="see-also"></a>Viz také
[Přehled hybridních připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Točitost zavádí hybridní připojení (video na kanálu 9)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Přehled hybridních připojení](/services/biztalk-services/)

[BizTalk Services: Karty řídicí panel, sledování, škálování, konfigurovat a hybridní připojení.](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Vytváření reálného hybridního cloudu s bezproblémové přenositelnost aplikace (Channel 9 video)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Přístup k místním prostředkům v Azure App Service pomocí hybridních připojení](web-sites-hybrid-connection-get-started.md)

[Přehled technologie ASP.NET Identity](http://www.asp.net/identity)

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
