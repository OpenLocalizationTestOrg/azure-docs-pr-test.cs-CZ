---
title: "Přístup k místním prostředkům pomocí hybridních připojení v prostředí Azure App Service"
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
ms.openlocfilehash: fbd22e6e285c5ddaef2a473671d4a06a97384b4a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a>Přístup k místním prostředkům pomocí hybridních připojení v prostředí Azure App Service
Aplikace služby Azure App Service může připojit k místnímu prostředku, který používá statický port TCP, jako je například SQL Server, MySQL, webové rozhraní API HTTP a většina vlastních webových služeb. Tento článek ukazuje, jak vytvořit hybridní připojení mezi služby App Service a místní databázi systému SQL Server.

> [!NOTE]
> Je k dispozici pouze v části webových aplikací funkci hybridní připojení [portálu Azure](https://portal.azure.com). Vytvoření připojení ve službě BizTalk Services naleznete v tématu [hybridní připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274). 
> 
> Tento obsah platí také pro Mobile Apps v Azure App Service. 
> 
> 

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure. Bezplatné předplatné, najdete v části [bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
  
    Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
* Používat hybridní připojení místní databázi systému SQL Server nebo SQL Server Express, musí být povolená na statický port TCP/IP. Pomocí výchozí instance na serveru SQL Server je doporučeno, protože používá statický port 1433. Informace o instalaci a konfiguraci systému SQL Server Express pro použití s hybridní připojení najdete v tématu [připojení k serveru SQL místní z Azure webu pomocí hybridních připojení](http://go.microsoft.com/fwlink/?LinkID=397979).
* Počítač, na který instalujete místní správce hybridního připojení agenta popsané dále v tomto článku:
  
  * Musí být schopný se připojit k Azure přes port 5671
  * Musí být schopen kontaktovat *hostname*:*ČísloPortu* vaše místní prostředku. 

> [!NOTE]
> Postup v tomto článku předpokládá, že používáte prohlížeč z počítače, který bude hostitelem agenta místní hybridní připojení.
> 
> 

## <a name="create-a-web-app-in-the-azure-portal"></a>Vytvořit webovou aplikaci na portálu Azure
> [!NOTE]
> Pokud jste již vytvořili webovou aplikaci nebo back-end mobilní aplikace na portálu Azure, který chcete použít pro tento kurz, můžete přeskočit na [vytvořit hybridní připojení a služby BizTalk](#CreateHC) a spustit z ní.
> 
> 

1. V levém horním rohu [portálu Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.
   
    ![Nové webové aplikace][NewWebsite]
2. Na **webové aplikace** okno, zadejte adresu URL a klikněte na **vytvořit**. 
   
    ![Název webu][WebsiteCreationBlade]
3. Po chvíli se webové aplikace se vytvoří a otevře se okno jeho webové aplikace. V okně se svisle posouvatelným řídicí panel, který vám umožní spravovat váš web.
   
    ![Spuštění webu][WebSiteRunningBlade]
4. Pokud chcete ověřit, web je za provozu, můžete kliknout na **Procházet** ikonu zobrazíte výchozí stránky.
   
    ![Klikněte na tlačítko Procházet zobrazíte vaší webové aplikace][Browse]
   
    ![Výchozí webové aplikace stránky][DefaultWebSitePage]

V dalším kroku vytvoříte hybridní připojení a služba BizTalk webové aplikace.

<a name="CreateHC"></a>

## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a>Vytvoření hybridní připojení a služby BizTalk
1. V okně vaší webové aplikace klikněte na **všechna nastavení** > **sítě** > **nakonfigurovat koncové body hybridního připojení**.
   
    ![Hybridní připojení][CreateHCHCIcon]
2. V okně hybridní připojení, klikněte na **přidat**.
   
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
   -->
3. **Přidat hybridní připojení** otevře se okno.  Vzhledem k tomu, že toto je první hybridní připojení **nové hybridní připojení** je předem vybrali možnost a **vytvořit hybridní připojení** otevře se okno pro vás.
   
    ![Vytvořit hybridní připojení.][TwinCreateHCBlades]
   
    Na **vytvořit hybridní připojení okno**:
   
   * Pro **název**, zadejte název pro připojení.
   * Pro **Hostname**, zadejte název místního počítače, který hostuje prostředku.
   * Pro **Port**, zadejte číslo portu, aby používal místnímu prostředku (1433 pro výchozí instanci systému SQL Server).
   * Klikněte na tlačítko **Biz Talk služby**
4. **Vytvořit službu BizTalk** otevře se okno. Zadejte název pro službu BizTalk a pak klikněte na tlačítko **OK**.
   
    ![Vytvoření služby BizTalk][CreateHCCreateBTS]
   
    **Vytvořit službu BizTalk** okno se zavře a vrátíte se **vytvořit hybridní připojení** okno.
5. V okně Vytvořit hybridní připojení, klikněte na **OK**. 
   
    ![Klikněte na tlačítko OK][CreateBTScomplete]
6. Po dokončení procesu oblast oznámení na portálu vás upozorní, že připojení byl úspěšně vytvořen.
   
    <!--- TODO
   
    Everything fails at this step. I can't create a BizTalk service in the dogfood portal. I switch to the classic portal
    (full portal) and created the BizTalk service but it doesn't seem to let you connnect them - When you finish the
    Create hybrid conn step, you get the following error
    Failed to create hybrid connection RelecIoudHC. The 
    resource type could not be found in the namespace 
    'Microsoft.BizTaIkServices for api version 2014-06-01'.
   
    The error indicates it couldn't find the type, not the instance.
    ![Success notification][CreateHCSuccessNotification]
    -->
7. V okně webové aplikace **hybridní připojení** nyní zobrazuje ikona vytvořený 1 hybridní připojení.
   
    ![Vytvoření jednoho hybridní připojení][CreateHCOneConnectionCreated]

V tomto okamžiku jste dokončili důležitou součástí infrastruktury cloudu hybridní připojení. V dalším kroku vytvoříte odpovídající část místně.

<a name="InstallHCM"></a>

## <a name="install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>Instalovat místní správce hybridního připojení k připojení
1. V okně webové aplikace, klikněte na tlačítko **všechna nastavení** > **sítě** > **nakonfigurovat koncové body hybridního připojení**. 
   
    ![Ikona hybridní připojení][HCIcon]
2. Na **hybridní připojení** okně **stav** sloupec pro ukazuje nedávno přidané koncový bod **Nepřipojeno**. Klikněte na připojení k jeho konfiguraci.
   
    ![Není připojeno][NotConnected]
   
    Otevře se okno hybridní připojení.
   
    ![NotConnectedBlade][NotConnectedBlade]
3. V okně klikněte na tlačítko **naslouchací proces instalace**.
   
    ![Klikněte na tlačítko naslouchací proces instalace][ClickListenerSetup]
4. **Vlastnosti hybridní připojení** otevře se okno. V části **místní správce hybridního připojení**, zvolte **nainstalujete kliknutím sem.**.
   
    ![Chcete-li nainstalovat, klikněte sem][ClickToInstallHCM]
5. V aplikaci spustit zabezpečení dialogové okno s upozorněním, zvolte **spustit** pokračujte.
   
    ![Zvolte spustit a pokračovat][ApplicationRunWarning]
6. V **řízení uživatelských účtů** dialogovém okně, vyberte **Ano**.
   
   ![Klepněte na tlačítko Ano][UAC]
7. Správce hybridního připojení je stažen a nainstalován za vás. 
   
    ![Instalace][HCMInstalling]
8. Po dokončení instalace klikněte na tlačítko **Zavřít**.
   
    ![Klikněte na tlačítko Zavřít][HCMInstallComplete]
   
    Na **hybridní připojení** okně **stav** teď zobrazuje sloupec **připojeno**. 
   
    ![Připojené stav][HCStatusConnected]

Teď, když je kompletní infrastrukturu hybridní připojení, můžete vytvořit hybridní aplikace, která jej používá. 

> [!NOTE]
> Následující části ukazují, jak používat hybridní připojení s projektem back-end mobilní aplikace .NET.
> 
> 

## <a name="configure-the-mobile-app-net-backend-project-to-connect-to-the-sql-server-database"></a>Konfigurace projektu back-end mobilní aplikace .NET pro připojení k databázi systému SQL Server
Ve službě App Service projektu back-end mobilní aplikace .NET je právě webové aplikace ASP.NET s další Mobile Apps SDK nainstalován a inicializován. Používat webové aplikace jako back-end mobilní aplikace, musíte [stáhnout a inicializace back-end mobilní aplikace .NET SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).  

Pro mobilní aplikace musíte také připojovací řetězec pro místní databázi definování a úprava back-end využívání tohoto připojení. 

1. V Průzkumníku řešení v sadě Visual Studio otevřete soubor Web.config pro váš back-end mobilní aplikace .NET, vyhledejte **connectionStrings** přidejte nový záznam SqlClient jako následující text, který odkazuje na místní databázi systému SQL Server:
   
        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />
   
    Nezapomeňte nahradit `<**secure_password**>` do tohoto řetězce s heslem, které jste vytvořili pro *HybridConnectionLogin*.
2. Klikněte na tlačítko **Uložit** v sadě Visual Studio k uložení souboru Web.config.
   
   > [!NOTE]
   > Toto nastavení připojení se používá při spuštění v místním počítači. Při spuštění v Azure, je toto nastavení přepsat nastavení připojení definované v portálu.
   > 
   > 
3. Rozbalte **modely** složky a otevřete soubor modelu dat, které končí na *Context.cs*.
4. Změnit **DbContext** konstruktoru instance předat hodnotu `OnPremisesDBConnection` s jejím základem **DbContext** konstruktoru, podobně jako následující fragment kódu:
   
        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }
   
    Služba bude nyní používat nové připojení k databázi systému SQL Server.

## <a name="update-the-mobile-app-backend-to-use-the-on-premises-connection-string"></a>Aktualizovat back-end mobilní aplikace použít místní připojovací řetězec
Dále musíte přidat nastavení aplikace pro tento nový připojovací řetězec, aby ho bylo možné použít z Azure.  

1. Zpět v [portál Azure](https://portal.azure.com) ve webové aplikaci back-end kód mobilní aplikace, klikněte na tlačítko **všechna nastavení**, pak **nastavení aplikace**.
2. V **webové nastavení aplikace** okno, přejděte dolů k **připojovací řetězce** a přidejte nové **systému SQL Server** připojovací řetězec s názvem `OnPremisesDBConnection` s hodnotou jako `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.
   
    Nahraďte `<**secure_password**>` zabezpečeného heslem pro váš místní databázi.
   
    ![Připojovací řetězec pro místní databázi](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)
3. Stiskněte klávesu **Uložit** uložit hybridní připojení a připojení řetězec jste právě vytvořili.

V tomto okamžiku můžete znovu publikovat serverový projekt a otestujte nové připojení s vaší stávající klienty Mobile Apps. Data budou číst z a zapisovat do místní databáze pomocí hybridní připojení.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Další kroky
* Informace o vytváření webové aplikace ASP.NET, který používá hybridní připojení najdete v tématu [připojení k serveru SQL místní z Azure webu pomocí hybridních připojení](http://go.microsoft.com/fwlink/?LinkID=397979). 

### <a name="additional-resources"></a>Další zdroje
[Přehled hybridních připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Točitost zavádí hybridní připojení (video na kanálu 9)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Hybridní připojení webu](https://azure.microsoft.com/services/biztalk-services/)

[BizTalk Services: Karty řídicí panel, sledování, škálování, konfigurovat a hybridní připojení.](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Vytváření reálného hybridního cloudu s bezproblémové přenositelnost aplikace (Channel 9 video)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Připojit k serveru SQL místní z Azure Mobile Services používat hybridní připojení (video na kanálu 9)](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

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
