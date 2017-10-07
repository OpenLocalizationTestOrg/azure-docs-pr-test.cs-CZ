---
title: "aaaAccess místních prostředků pomocí hybridních připojení ve službě Azure App Service"
description: "Vytvořte připojení mezi webovou aplikaci v Azure App Service a místnímu prostředku, který používá statický port TCP"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: a46ed26b-df8e-4fc3-8e05-2d002a6ee508
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: cephalin
ms.openlocfilehash: de7c57b94f4bd6250a93757817178e8455daae4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a>Přístup k místním prostředkům pomocí hybridních připojení v prostředí Azure App Service
Můžete se připojit službě Azure App Service aplikace tooany místní prostředek, který používá statický port TCP, jako je například SQL Server, MySQL, webové rozhraní API HTTP a většina vlastních webových služeb. Tento článek ukazuje, jak toocreate hybridní připojení mezi služby App Service a místní databázi systému SQL Server.

> [!NOTE]
> část webové aplikace Hello hello hybridní připojení funkce je k dispozici pouze v hello [portálu Azure](https://portal.azure.com). toocreate připojení ve službě BizTalk Services najdete v části [hybridní připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274). 
> 
> Tento obsah platí také tooMobile aplikace v Azure App Service. 
> 
> 

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure. Bezplatné předplatné, najdete v části [bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
  
    Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
* toouse místní SQL Server nebo SQL Server Express databáze s hybridní připojení TCP/IP musí toobe povoleno na statický port. Pomocí výchozí instance na serveru SQL Server je doporučeno, protože používá statický port 1433. Informace o instalaci a konfiguraci systému SQL Server Express pro použití s hybridní připojení najdete v tématu [tooan připojit místní systém SQL Server z Azure webu pomocí hybridních připojení](http://go.microsoft.com/fwlink/?LinkID=397979).
* Hello počítač, na který instalujete hello místní správce hybridního připojení agenta popsané dále v tomto článku:
  
  * Musí být schopný tooconnect tooAzure přes port 5671
  * Musí být schopný tooreach hello *hostname*:*ČísloPortu* vaše místní prostředku. 

> [!NOTE]
> Hello postup v tomto článku předpokládá, že používáte prohlížeč hello z hello počítače, který bude hostitelem hello místní hybridní připojení agenta.
> 
> 

## <a name="create-a-web-app-in-hello-azure-portal"></a>Vytvoření webové aplikace v hello portálu Azure
> [!NOTE]
> Pokud jste již vytvořili webovou aplikaci nebo back-end mobilní aplikace v portálu Azure, že chcete pro tento kurz toouse hello, můžete přeskočit příliš[vytvořit hybridní připojení a služby BizTalk](#CreateHC) a spustit z ní.
> 
> 

1. V hello levého horního rohu hello [portálu Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.
   
    ![Nové webové aplikace][NewWebsite]
2. Na hello **webové aplikace** okno, zadejte adresu URL a klikněte na **vytvořit**. 
   
    ![Název webu][WebsiteCreationBlade]
3. Po chvíli se hello webové aplikace se vytvoří a otevře se okno jeho webové aplikace. okno Hello se svisle posouvatelným řídicí panel, který vám umožní spravovat váš web.
   
    ![Spuštění webu][WebSiteRunningBlade]
4. Web tooverify hello je za provozu, můžete kliknout na hello **Procházet** ikonu toodisplay hello výchozí stránky.
   
    ![Klikněte na tlačítko Procházet toosee vaší webové aplikace][Browse]
   
    ![Výchozí webové aplikace stránky][DefaultWebSitePage]

V dalším kroku vytvoříte hybridní připojení a služby BizTalk pro hello webovou aplikaci.

<a name="CreateHC"></a>

## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a>Vytvoření hybridní připojení a služby BizTalk
1. V okně vaší webové aplikace klikněte na **všechna nastavení** > **sítě** > **nakonfigurovat koncové body hybridního připojení**.
   
    ![Hybridní připojení][CreateHCHCIcon]
2. V okně připojení hybridní hello, klikněte na tlačítko **přidat**.
   
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
   -->
3. Hello **přidat hybridní připojení** otevře se okno.  Vzhledem k tomu, že je to první hybridní připojení, hello **nové hybridní připojení** je předem vybrali možnost a hello **vytvořit hybridní připojení** otevře se okno pro vás.
   
    ![Vytvořit hybridní připojení.][TwinCreateHCBlades]
   
    Na hello **vytvořit hybridní připojení okno**:
   
   * Pro **název**, zadejte název připojení hello.
   * Pro **Hostname**, zadejte název hello hello na místní počítač, který je hostitelem prostředku.
   * Pro **Port**, zadejte číslo portu hello že místnímu prostředku používá (1433 pro výchozí instanci systému SQL Server).
   * Klikněte na tlačítko **Biz Talk služby**
4. Hello **vytvořit službu BizTalk** otevře se okno. Zadejte název pro hello služby BizTalk a pak klikněte na tlačítko **OK**.
   
    ![Vytvoření služby BizTalk][CreateHCCreateBTS]
   
    Hello **vytvořit službu BizTalk** okno se zavře a vrátíte se toohello **vytvořit hybridní připojení** okno.
5. V okně hello vytvořit hybridní připojení, klikněte na tlačítko **OK**. 
   
    ![Klikněte na tlačítko OK][CreateBTScomplete]
6. Po dokončení procesu hello hello oznamovací oblast v hello portál vás upozorní, že hello připojení byl úspěšně vytvořen.
   
    <!--- TODO
   
    Everything fails at this step. I can't create a BizTalk service in hello dogfood portal. I switch toohello classic portal
    (full portal) and created hello BizTalk service but it doesn't seem toolet you connnect them - When you finish the
    Create hybrid conn step, you get hello following error
    Failed toocreate hybrid connection RelecIoudHC. hello 
    resource type could not be found in hello namespace 
    'Microsoft.BizTaIkServices for api version 2014-06-01'.
   
    hello error indicates it couldn't find hello type, not hello instance.
    ![Success notification][CreateHCSuccessNotification]
    -->
7. V okně webové aplikace hello hello **hybridní připojení** nyní zobrazuje ikona vytvořený 1 hybridní připojení.
   
    ![Vytvoření jednoho hybridní připojení][CreateHCOneConnectionCreated]

V tomto okamžiku jste dokončili důležitou součástí infrastruktury hello cloudu hybridní připojení. V dalším kroku vytvoříte odpovídající část místně.

<a name="InstallHCM"></a>

## <a name="install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a>Instalace hello místní správce hybridního připojení toocomplete hello připojení
1. V okně hello webové aplikace, klikněte na tlačítko **všechna nastavení** > **sítě** > **nakonfigurovat koncové body hybridního připojení**. 
   
    ![Ikona hybridní připojení][HCIcon]
2. Na hello **hybridní připojení** okno, hello **stav** nedávno přidat koncový bod zobrazuje sloupec pro hello **Nepřipojeno**. Klikněte na tlačítko hello připojení tooconfigure ho.
   
    ![Není připojeno][NotConnected]
   
    Otevře se okno připojení hybridní Hello.
   
    ![NotConnectedBlade][NotConnectedBlade]
3. V okně hello, klikněte na tlačítko **naslouchací proces instalace**.
   
    ![Klikněte na tlačítko naslouchací proces instalace][ClickListenerSetup]
4. Hello **vlastnosti hybridní připojení** otevře se okno. V části **místní správce hybridního připojení**, zvolte **kliknutím sem tooinstall**.
   
    ![Kliknutím sem tooinstall][ClickToInstallHCM]
5. V hello spustit aplikaci zabezpečení dialogové okno s upozorněním, zvolte **spustit** toocontinue.
   
    ![Zvolte toocontinue spustit][ApplicationRunWarning]
6. V hello **řízení uživatelských účtů** dialogovém okně, vyberte **Ano**.
   
   ![Klepněte na tlačítko Ano][UAC]
7. Hello správce hybridního připojení je stažen a nainstalován za vás. 
   
    ![Instalace][HCMInstalling]
8. Po dokončení instalace hello, klikněte na tlačítko **Zavřít**.
   
    ![Klikněte na tlačítko Zavřít][HCMInstallComplete]
   
    Na hello **hybridní připojení** okno, hello **stav** teď zobrazuje sloupec **připojeno**. 
   
    ![Připojené stav][HCStatusConnected]

Nyní je kompletní tuto infrastrukturu hello hybridní připojení, můžete vytvořit hybridní aplikace, která jej používá. 

> [!NOTE]
> Hello následující části ukazují, jak toouse hybridní připojení s projektem back-end mobilní aplikace .NET.
> 
> 

## <a name="configure-hello-mobile-app-net-backend-project-tooconnect-toohello-sql-server-database"></a>Konfigurace hello mobilní aplikace .NET back-end projektu tooconnect toohello databázi systému SQL Server
Ve službě App Service projektu back-end mobilní aplikace .NET je právě webové aplikace ASP.NET s další Mobile Apps SDK nainstalován a inicializován. toouse vaší webové aplikace jako back-end mobilní aplikace, musíte [stáhnout a inicializace hello mobilní aplikace .NET back-end SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).  

Pro Mobile Apps také potřebovat toodefine připojovací řetězec pro hello místní databázi a upravte toouse hello back-end toto připojení. 

1. V Průzkumníku řešení v sadě Visual Studio, otevřete hello souboru Web.config back-endu mobilní aplikace .NET, vyhledejte hello **connectionStrings** přidejte nový záznam SqlClient jako hello následující, které body toohello místní SQL Databáze serveru:
   
        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />
   
    Mějte na paměti, tooreplace `<**secure_password**>` do tohoto řetězce s heslem hello jste vytvořili pro *HybridConnectionLogin*.
2. Klikněte na tlačítko **Uložit** v souboru Web.config hello toosave Visual Studio.
   
   > [!NOTE]
   > Toto nastavení připojení se používá při spuštění v místním počítači hello. Při spuštění v Azure, je toto nastavení přepsat nastavení připojení hello definované hello portálu.
   > 
   > 
3. Rozbalte hello **modely** složku a otevřete hello datového modelu souboru, který končí v *Context.cs*.
4. Upravit hello **DbContext** toopass konstruktor instance hello hodnotu `OnPremisesDBConnection` toohello základní **DbContext** konstruktor, podobně jako toohello následující fragment kódu:
   
        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }
   
    Služba Hello nyní použije hello nové připojení toohello databáze SQL serveru.

## <a name="update-hello-mobile-app-backend-toouse-hello-on-premises-connection-string"></a>Aktualizovat připojovací řetězec místní toouse hello hello mobilní aplikace back-end
Dále musíte tooadd nastavení aplikace pro tento nový připojovací řetězec, aby ho bylo možné použít z Azure.  

1. Zpět v hello [portál Azure](https://portal.azure.com) v hello kódu webové aplikace back-end mobilní aplikace, klikněte na **všechna nastavení**, pak **nastavení aplikace**.
2. V hello **webové nastavení aplikace** okno, posuňte se dolů příliš**připojovací řetězce** a přidejte nové **systému SQL Server** připojovací řetězec s názvem `OnPremisesDBConnection` s hodnotou jako `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.
   
    Nahraďte `<**secure_password**>` s hello zabezpečeného hesla pro místní databázi.
   
    ![Připojovací řetězec pro místní databázi](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)
3. Stiskněte klávesu **Uložit** toosave hello hybridní připojení a připojovací řetězec jste právě vytvořili.

V tomto okamžiku můžete znovu publikovat hello serverový projekt a otestovat hello nové připojení s vaší stávající klienty mobilní aplikace. Data budou číst a zapisovat toohello místní databázi pomocí hello hybridní připojení.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Další kroky
* Informace o vytváření webové aplikace ASP.NET, který používá hybridní připojení najdete v tématu [tooan připojit místní systém SQL Server z Azure webu pomocí hybridních připojení](http://go.microsoft.com/fwlink/?LinkID=397979). 

### <a name="additional-resources"></a>Další zdroje
[Přehled hybridních připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Točitost zavádí hybridní připojení (video na kanálu 9)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Hybridní připojení webu](https://azure.microsoft.com/services/biztalk-services/)

[BizTalk Services: Karty řídicí panel, sledování, škálování, konfigurovat a hybridní připojení.](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Vytváření reálného hybridního cloudu s bezproblémové přenositelnost aplikace (Channel 9 video)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Připojit tooan místní SQL Server z Azure Mobile Services používat hybridní připojení (video na kanálu 9)](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[New]:./media/web-sites-hybrid-connection-get-started/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-get-started/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-get-started/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-get-started/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-get-started/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-get-started/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-get-started/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-get-started/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-get-started/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-get-started/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-get-started/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-get-started/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-get-started/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-get-started/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-get-started/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-get-started/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-get-started/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-get-started/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-get-started/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-get-started/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-get-started/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-get-started/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-get-started/D10HCStatusConnected.png
