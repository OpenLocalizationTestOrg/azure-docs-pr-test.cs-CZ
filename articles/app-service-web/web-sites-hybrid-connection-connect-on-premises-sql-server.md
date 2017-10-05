---
title: "Připojení k místnímu SQL serveru z webové aplikace v Azure App Service pomocí hybridních připojení"
description: "Vytvoření webové aplikace v Microsoft Azure a připojte ho k databázi systému SQL Server na místě"
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
ms.openlocfilehash: 12456ef3e2aecfa7a03cca97de2ff6ffd9602357
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Připojení k místnímu SQL serveru z webové aplikace v Azure App Service pomocí hybridních připojení
Hybridní připojení se můžete připojit [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webových aplikací pro místní prostředky, které používají statický port TCP. Podporované prostředky zahrnují Microsoft SQL Server, MySQL, webové rozhraní API HTTP, služby App Service a většina vlastních webových služeb.

V tomto kurzu se dozvíte, jak vytvořit webové aplikace služby App Service v [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715), webovou aplikaci připojit k místní databázi SQL serveru pomocí nové funkce hybridní připojení, vytvořit jednoduchou aplikaci ASP.NET který používat hybridní připojení a nasadit aplikace do webové aplikace služby App Service. Hotové webové aplikace v Azure uloží v databázi členství, která je místní přihlašovací údaje uživatele. Kurz předpokládá žádné předchozí zkušenosti s používáním Azure nebo v technologii ASP.NET.

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> Je k dispozici pouze v části webových aplikací funkci hybridní připojení [portálu Azure](https://portal.azure.com). Vytvoření připojení ve službě BizTalk Services naleznete v tématu [hybridní připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274).  
> 
> 

## <a name="prerequisites"></a>Požadavky
K dokončení tohoto kurzu, budete potřebovat následující produkty. Všechny jsou k dispozici v bezplatné verze, abyste mohli začít, vývoj pro Azure zcela zdarma.

* **Předplatné Azure** – bezplatné předplatné, najdete v části [bezplatná zkušební verze Azure](/pricing/free-trial/).
* **Visual Studio 2013** – Pokud chcete stáhnout bezplatnou zkušební verzi Visual Studio 2013, najdete v části [Visual Studio stáhne](http://www.visualstudio.com/downloads/download-visual-studio-vs). Než budete pokračovat, nainstalujte tento.
* **Rozhraní Microsoft .NET Framework 3.5 Service Pack 1** – Pokud je váš operační systém Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 nebo Windows Server 2008 R2, můžete ho povolit v Ovládacích panelech > programy a funkce > zapnout Funkce systému Windows nebo vypnout. Jinak, můžete ji stáhnout z [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).
* **SQL Server 2014 Express with Tools** -Microsoft SQL Server Express si zdarma stáhněte na [databáze platforma Microsoft webové stránky](http://www.microsoft.com/web/platform/database.aspx). Vyberte **Express** (ne LocalDB) verze. **Express with Tools** verze zahrnuje SQL Server Management Studio, které budete používat v tomto kurzu.
* **SQL Server Management Studio Express** – to je součástí systému SQL Server 2014 Express pomocí nástroje pro stažení výše uvedených, ale pokud potřebujete nainstalovat samostatně, můžete stáhnout a nainstalujte ji z [SQL Server Express stahování stránka](http://www.microsoft.com/web/platform/database.aspx).

Kurz předpokládá, že máte předplatné Azure, jestli máte nainstalovanou sadu Visual Studio 2013, a že jste nainstalovali nebo povoleno rozhraní .NET Framework 3.5. Tento kurz ukazuje, jak nainstalovat systém SQL Server 2014 Express v konfiguraci, který pracuje s funkci Azure hybridní připojení (výchozí instanci s statický port TCP). Před zahájením tohoto kurzu, stáhněte z umístění uvedených výše, pokud nemáte nainstalovaný server SQL Server SQL Server 2014 Express with Tools.

### <a name="notes"></a>Poznámky
Používat hybridní připojení místní databázi systému SQL Server nebo SQL Server Express, musí být povolená na statický port TCP/IP. Výchozí instance na serveru SQL Server používají statický port 1433, zatímco pojmenovaných instancí nepodporují.

Počítač, na kterém jste nainstalovali agenta na místní správce hybridního připojení:

* Musí mít odchozí připojení k Azure přes:

| Port | Proč |
| --- | --- |
| 80 |**Požadované** pro port HTTP pro ověření certifikátu a volitelně pro datové připojení. |
| 443 |**Volitelné** pro datové připojení. Pokud na hodnotu 443 odchozí připojení není k dispozici, použije se TCP port 80. |
| 5671 a 9352 |**Doporučená** ale volitelné pro datové připojení. Všimněte si, že tento režim obvykle dává vyšší propustnost. Pokud k těmto portům odchozí připojení není k dispozici, použije se port 443 protokolu TCP. |

* Musí být schopen kontaktovat *hostname*:*ČísloPortu* vaše místní prostředku.

Postup v tomto článku předpokládá, že používáte prohlížeč z počítače, který bude hostitelem agenta místní hybridní připojení.

Pokud už máte nainstalovaný v konfiguraci a v prostředí, které splňuje podmínky popsané výše server SQL Server, můžete přeskočit a začínat [vytvořit databázi systému SQL Server na místě](#CreateSQLDB).

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>A. Nainstalujte systém SQL Server Express, povolte protokol TCP/IP a vytvořit databázi systému SQL Server na místě
V této části se dozvíte, jak nainstalovat systém SQL Server Express, povolte protokol TCP/IP a vytvořit databázi tak, aby webová aplikace bude fungovat s portálu Azure.

### <a name="install-sql-server-express"></a>Nainstalujte SQL Server Express.
1. Chcete-li nainstalovat systém SQL Server Express, spusťte **SQLEXPRWT_x64_ENU.exe** nebo **SQLEXPR_x86_ENU.exe** soubor, který jste stáhli. Zobrazí se Průvodce Centrum instalace SQL serveru.
   
    ![Instalace serveru SQL][SQLServerInstall]
2. Zvolte **samostatná instalace nový Server SQL nebo přidání funkcí do existující instalace**. Postupujte podle pokynů, až na přijímání výchozí možnosti a nastavení, **konfigurace Instance** stránky.
3. Na **konfigurace Instance** vyberte **výchozí instance**.
   
    ![Vyberte výchozí instanci][ChooseDefaultInstance]
   
    Ve výchozím nastavení výchozí instance systému SQL Server čeká na požadavky od klientů systému SQL Server na statický port 1433, což je co vyžaduje funkci hybridní připojení. Pojmenované instance používají dynamické porty a UDP, které nejsou podporovány hybridní připojení.
4. Přijměte výchozí hodnoty na **konfigurace serveru** stránky.
5. Na **konfigurace databázového stroje** v části **režim ověřování**, zvolte **smíšený režim (ověřování systému SQL Server a ověřování systému Windows)**a zadejte heslo.
   
    ![Zvolte ve smíšeném režimu][ChooseMixedMode]
   
    V tomto kurzu budete používat ověřování serveru SQL Server. Ujistěte se, že jste si pamatujete heslo, které poskytujete, protože jej budete potřebovat později.
6. Krok procházení zbývající části v průvodci pro dokončení instalace.

### <a name="enable-tcpip"></a>Povolte protokol TCP/IP
Pokud chcete povolit protokol TCP/IP, budete používat SQL Server Configuration Manager, který byl nainstalován při instalaci systému SQL Server Express. Postupujte podle kroků v [povolit síťový protokol TCP/IP pro SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) než budete pokračovat.

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a>Vytvořit databázi systému SQL Server na místě
Webové aplikace Visual Studio vyžaduje členství v databázi, která je přístupná v Azure. To vyžaduje databázi systému SQL Server nebo SQL Server Express (ne LocalDB databázi, šablona MVC používá ve výchozím nastavení), tak další vytvoříte databázi členství.

1. V systému SQL Server Management Studio připojte k systému SQL Server, který jste právě nainstalovali. (Pokud **připojit k serveru** dialogové okno nezobrazí automaticky, přejděte na **Průzkumník objektů** v levém podokně klikněte na **připojit**a pak klikněte na tlačítko  **Databáze modul**.) ![Připojení k serveru][SSMSConnectToServer]
   
    Pro **typ serveru**, zvolte **databázový stroj**. Pro **název serveru**, můžete použít **localhost** nebo název počítače, který používáte. Zvolte **ověřování serveru SQL Server**a potom se přihlaste pomocí uživatelského jména správce systému a heslo, které jste vytvořili dříve.
2. Chcete-li vytvořit novou databázi pomocí SQL Server Management Studio, klikněte pravým tlačítkem **databáze** v Průzkumníku objektů a potom klikněte na **novou databázi**.
   
    ![Vytvoření nové databáze][SSMScreateNewDB]
3. V **novou databázi** dialogové okno, zadejte MembershipDB pro název databáze a pak klikněte na tlačítko **OK**.
   
    ![Zadejte název databáze][SSMSprovideDBname]
   
    Všimněte si, že jste nebyly provedeny žádné změny do databáze v tomto okamžiku. Informace o členství v budou automaticky později přidány webovou aplikací, když jej spustíte.
4. V Průzkumníku objektů, pokud rozbalíte **databáze**, uvidíte, že byla vytvořena databáze členství.
   
    ![MembershipDB vytvořen][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-the-azure-portal"></a>B. Vytvořit webovou aplikaci na portálu Azure
> [!NOTE]
> Pokud jste již vytvořili webovou aplikaci na portálu Azure, který chcete použít pro tento kurz, můžete přeskočit na [vytvořit hybridní připojení a služby BizTalk](#CreateHC) a pokračovat dál.
> 
> 

1. V [portálu Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.
   
    ![tlačítko Nový][New]
2. Konfigurace webové aplikace a pak klikněte na **vytvořit**.
   
    ![Název webu][WebsiteCreationBlade]
3. Po chvíli se webové aplikace se vytvoří a otevře se okno jeho webové aplikace. V okně se svisle posouvatelným řídicí panel, který vám umožní spravovat webové aplikace.
   
    ![Spuštění webu][WebSiteRunningBlade]
   
    Pokud chcete ověřit, webová aplikace je za provozu, můžete kliknout na **Procházet** ikonu zobrazíte výchozí stránky.

V dalším kroku vytvoříte hybridní připojení a služba BizTalk webové aplikace.

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. Vytvoření hybridní připojení a služby BizTalk
1. Zpět na portálu, přejděte do nastavení a klikněte na tlačítko **sítě** > **nakonfigurovat koncové body hybridního připojení**.
   
    ![Hybridní připojení][CreateHCHCIcon]
2. V okně hybridní připojení, klikněte na **přidat** > **nové hybridní připojení**.
3. Na **vytvořit hybridní připojení** okno:
   
   * Pro **název**, zadejte název pro připojení.
   * Pro **Hostname**, zadejte název počítače hostitelského počítače systému SQL Server.
   * Pro **Port**, zadejte 1433 (výchozí port pro SQL Server).
   * Klikněte na tlačítko **služby BizTalk** > **novou službu BizTalk** a zadejte název pro službu BizTalk.
     
     ![Vytvořit hybridní připojení.][TwinCreateHCBlades]
4. Klikněte na tlačítko **OK** dvakrát.
   
    Po dokončení procesu **oznámení** oblasti blikat zelená **úspěch** a **hybridní připojení** okno se zobrazí nový hybridní připojení se stavem jako **Nepřipojeno**.
   
    ![Vytvoření jednoho hybridní připojení][CreateHCOneConnectionCreated]

V tomto okamžiku jste dokončili důležitou součástí infrastruktury cloudu hybridní připojení. V dalším kroku vytvoříte odpovídající část místně.

<a name="InstallHCM"></a>

## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>D. Instalovat místní správce hybridního připojení k připojení
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Teď, když je kompletní infrastrukturu hybridní připojení, vytvoříte webovou aplikaci, která jej používá.

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a>E. Vytvořit základní projekt ASP.NET, upravit připojovací řetězec databáze a spusťte projekt lokálně.
### <a name="create-a-basic-aspnet-project"></a>Vytvoření základního projektu ASP.NET
1. V sadě Visual Studio na **souboru** nabídky, vytvořte nový projekt:
   
    ![Nový projekt Visual Studio][HCVSNewProject]
2. V **šablony** části **nový projekt** dialogovém okně, vyberte **webové** a zvolte **webové aplikace ASP.NET**a pak klikněte na tlačítko  **OK**.
   
    ![Vyberte webové aplikace ASP.NET][HCVSChooseASPNET]
3. V **nový projekt ASP.NET** dialogovém okně, vyberte **MVC**a potom klikněte na **OK**.
   
    ![Zvolte MVC][HCVSChooseMVC]
4. Po vytvoření projektu, zobrazí se stránka readme aplikace. Ještě nespouštějte webového projektu.
   
    ![Stránce Readme][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a>Upravit připojovací řetězec databáze pro aplikaci
V tomto kroku upravíte připojovací řetězec, který určuje aplikace, kde najít místní databáze SQL Server Express. Připojovací řetězec je v souboru Web.config aplikace, který obsahuje informace o konfiguraci pro aplikaci.

> [!NOTE]
> Aby se zajistilo, že vaše aplikace používá databázi, kterou jste vytvořili v systému SQL Server Express a nikoli k tomu v sadě Visual Studio výchozí LocalDB, je důležité, provedení tohoto kroku před spuštěním projektu.
> 
> 

1. V Průzkumníku řešení poklikejte na soubor Web.config.
   
    ![Soubor web.config][HCVSChooseWebConfig]
2. Upravit **connectionStrings** oddílu tak, aby odkazoval na databázi systému SQL Server na místním počítači, řiďte se syntaxí v následujícím příkladu:
   
    ![Připojovací řetězec][HCVSConnectionString]
   
    Při sestavování připojovací řetězec, mějte na paměti následující:
   
   * Pokud se připojujete k pojmenované instanci namísto výchozí instance (například YourServer\SQLEXPRESS), musíte nakonfigurovat SQL Server na použití statických portů. Informace o konfiguraci statických portů najdete v tématu [postup konfigurace systému SQL Server tak, aby naslouchala na určitém portu](http://support.microsoft.com/kb/823938). Ve výchozím nastavení používají pojmenované instance UDP a dynamické porty, které nejsou podporovány hybridní připojení.
   * Je doporučeno, můžete určit port (standardně, jak je znázorněno v příkladu 1433) na připojovací řetězec tak, aby vám může být opravdu, místní systém SQL Server má TCP povoleno a používá správný port.
   * Nezapomeňte použít ověřování systému SQL Server se pokud chcete připojit, zadání ID uživatele a heslo v připojovacím řetězci.
3. Klikněte na tlačítko **Uložit** v sadě Visual Studio k uložení souboru Web.config.

### <a name="run-the-project-locally-and-register-a-new-user"></a>Spusťte projekt lokálně a registraci nového uživatele
1. Teď spusťte místně nový webový projekt kliknutím na tlačítko Procházet pod ladění. Tento příklad používá aplikace Internet Explorer.
   
    ![Spusťte projekt][HCVSRunProject]
2. V pravém horním rohu stránky výchozí webové stránky, zvolte **zaregistrovat** zaregistrovat nový účet:
   
    ![Zaregistrovat nový účet][HCVSRegisterLocally]
3. Zadejte uživatelské jméno a heslo:
   
    ![Zadejte uživatelské jméno a heslo][HCVSCreateNewAccount]
   
    Toto automaticky vytvoří databázi na vaše místní systém SQL Server, který obsahuje informace o členství pro vaši aplikaci. Jeden z tabulky (**dbo. AspNetUsers**) blokování webové aplikace přihlašovací údaje uživatele jako ty, které jste zadali. Zobrazí se tato tabulka později v tomto kurzu.
4. Zavřete okno prohlížeče výchozí webové stránky. To ukončí aplikaci v sadě Visual Studio.

Nyní jste připraveni na další krok, který je k publikování aplikace do Azure a otestovat ji.

<a name="PubNTest"></a>

## <a name="f-publish-the-web-application-to-azure-and-test-it"></a>F. Publikování webové aplikace do Azure a otestovat ji
Nyní budete publikování aplikace do webové aplikace služby App Service a otestujte ji chcete zobrazit, jak hybridní připojení, které jste dříve nakonfigurovali je používána pro webovou aplikaci připojit k databázi na místním počítači.

### <a name="publish-the-web-application"></a>Publikování webové aplikace
1. Můžete si stáhnout váš profil publikování pro webové aplikace App Service na portálu Azure. V okně vaší webové aplikace, klikněte na tlačítko **profilu publikování Get**a pak soubor uložte do počítače.
   
    ![Stažení profilu publikování][PortalDownloadPublishProfile]
   
    Dále budete importovat tento soubor do webové aplikace Visual Studio.
2. V sadě Visual Studio, klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a vyberte **publikovat**.
   
    ![Vyberte publikování][HCVSRightClickProjectSelectPublish]
3. V **Publikovat Web** dialogové okno na **profil** , zvolte **Import**.
   
    ![Import][HCVSPublishWebDialogImport]
4. Přejděte do vaší stažený profil publikování, vyberte ho a pak klikněte na tlačítko **OK**.
   
    ![Vyhledejte profil][HCVSBrowseToImportPubProfile]
5. Informace o publikování je naimportována a zobrazí v **připojení** kartě dialogového okna.
   
    ![Klikněte na tlačítko Publikovat][HCVSClickPublish]
   
    Klikněte na **Publikovat**.
   
    Po dokončení publikování, bude váš prohlížeč spustit a zobrazit aplikace teď známé ASP.NET – s tím rozdílem, že nyní je za provozu v cloudu Azure!

V dalším kroku použijete za provozu webové aplikace zobrazíte jeho hybridní připojení v akci.

### <a name="test-the-completed-web-application-on-azure"></a>Testování hotové webové aplikace v Azure
1. V horní části napravo od webové stránky v Azure, zvolte **přihlásit**.
   
    ![Test přihlášení][HCTestLogIn]
2. Webové aplikace služby App Service je teď připojený k databázi členství webové aplikace na místním počítači. Chcete-li to ověřit, přihlaste se pomocí stejných přihlašovacích údajů, které jste zadali dříve v místní databázi.
   
    ![Hello pozdravu][HCTestHelloContoso]
3. Chcete-li otestovat další nové hybridní připojení, odhlaste se z Azure webové aplikace a zaregistrujte se jako jiný uživatel. Zadejte nové uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.
   
    ![Test zaregistrovat jiný uživatel][HCTestRegisterRelecloud]
4. Pokud chcete ověřit, že byly uloženy přihlašovací údaje nového uživatele v místní databázi prostřednictvím hybridní připojení, otevřete SQL Management Studio v místním počítači. V Průzkumníku objektů rozbalte **MembershipDB** databáze a potom rozbalte **tabulky**. Klikněte pravým tlačítkem myši **dbo. AspNetUsers** členství tabulky a zvolte **řádky vyberte Top 1000** zobrazíte výsledky.
   
    ![Zobrazení výsledků][HCTestSSMSTree]
5. Vaše tabulka členství v místní nyní zobrazuje oba účty – ten, který jste vytvořili místně a ten, který jste vytvořili v cloudu Azure. Ten, který jste vytvořili v cloudu byly uloženy do místní databáze pomocí funkce Azure hybridní připojení.
   
    ![Registrovaní uživatelé v místní databázi][HCTestShowMemberDb]

Nyní jste vytvořili a nasazené webové aplikace ASP.NET, která používá hybridní připojení mezi webovou aplikaci v cloudu Azure a místní databázi systému SQL Server. Blahopřejeme!

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
