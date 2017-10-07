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
# <a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a><span data-ttu-id="582d5-103">Přístup k místním prostředkům pomocí hybridních připojení v prostředí Azure App Service</span><span class="sxs-lookup"><span data-stu-id="582d5-103">Access on-premises resources using hybrid connections in Azure App Service</span></span>
<span data-ttu-id="582d5-104">Můžete se připojit službě Azure App Service aplikace tooany místní prostředek, který používá statický port TCP, jako je například SQL Server, MySQL, webové rozhraní API HTTP a většina vlastních webových služeb.</span><span class="sxs-lookup"><span data-stu-id="582d5-104">You can connect an Azure App Service app tooany on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="582d5-105">Tento článek ukazuje, jak toocreate hybridní připojení mezi služby App Service a místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="582d5-105">This article shows you how toocreate a hybrid connection between App Service and an on-premises SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="582d5-106">část webové aplikace Hello hello hybridní připojení funkce je k dispozici pouze v hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="582d5-106">hello Web Apps portion of hello Hybrid Connections feature is available only in hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="582d5-107">toocreate připojení ve službě BizTalk Services najdete v části [hybridní připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="582d5-107">toocreate a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span> 
> 
> <span data-ttu-id="582d5-108">Tento obsah platí také tooMobile aplikace v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="582d5-108">This content also applies tooMobile Apps in Azure App Service.</span></span> 
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="582d5-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="582d5-109">Prerequisites</span></span>
* <span data-ttu-id="582d5-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="582d5-110">An Azure subscription.</span></span> <span data-ttu-id="582d5-111">Bezplatné předplatné, najdete v části [bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="582d5-111">For a free subscription, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
  
    <span data-ttu-id="582d5-112">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="582d5-112">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="582d5-113">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="582d5-113">No credit cards required; no commitments.</span></span>
* <span data-ttu-id="582d5-114">toouse místní SQL Server nebo SQL Server Express databáze s hybridní připojení TCP/IP musí toobe povoleno na statický port.</span><span class="sxs-lookup"><span data-stu-id="582d5-114">toouse an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs toobe enabled on a static port.</span></span> <span data-ttu-id="582d5-115">Pomocí výchozí instance na serveru SQL Server je doporučeno, protože používá statický port 1433.</span><span class="sxs-lookup"><span data-stu-id="582d5-115">Using a default instance on SQL Server is recommended because it uses static port 1433.</span></span> <span data-ttu-id="582d5-116">Informace o instalaci a konfiguraci systému SQL Server Express pro použití s hybridní připojení najdete v tématu [tooan připojit místní systém SQL Server z Azure webu pomocí hybridních připojení](http://go.microsoft.com/fwlink/?LinkID=397979).</span><span class="sxs-lookup"><span data-stu-id="582d5-116">For information on installing and configuring SQL Server Express for use with hybrid connections, see [Connect tooan on-premises SQL Server from an Azure web site using Hybrid Connections](http://go.microsoft.com/fwlink/?LinkID=397979).</span></span>
* <span data-ttu-id="582d5-117">Hello počítač, na který instalujete hello místní správce hybridního připojení agenta popsané dále v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="582d5-117">hello computer on which you install hello on-premises Hybrid Connection Manager agent described later in this article:</span></span>
  
  * <span data-ttu-id="582d5-118">Musí být schopný tooconnect tooAzure přes port 5671</span><span class="sxs-lookup"><span data-stu-id="582d5-118">Must be able tooconnect tooAzure over port 5671</span></span>
  * <span data-ttu-id="582d5-119">Musí být schopný tooreach hello *hostname*:*ČísloPortu* vaše místní prostředku.</span><span class="sxs-lookup"><span data-stu-id="582d5-119">Must be able tooreach hello *hostname*:*portnumber* of your on-premises resource.</span></span> 

> [!NOTE]
> <span data-ttu-id="582d5-120">Hello postup v tomto článku předpokládá, že používáte prohlížeč hello z hello počítače, který bude hostitelem hello místní hybridní připojení agenta.</span><span class="sxs-lookup"><span data-stu-id="582d5-120">hello steps in this article assume that you are using hello browser from hello computer that will host hello on-premises hybrid connection agent.</span></span>
> 
> 

## <a name="create-a-web-app-in-hello-azure-portal"></a><span data-ttu-id="582d5-121">Vytvoření webové aplikace v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="582d5-121">Create a web app in hello Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="582d5-122">Pokud jste již vytvořili webovou aplikaci nebo back-end mobilní aplikace v portálu Azure, že chcete pro tento kurz toouse hello, můžete přeskočit příliš[vytvořit hybridní připojení a služby BizTalk](#CreateHC) a spustit z ní.</span><span class="sxs-lookup"><span data-stu-id="582d5-122">If you have already created a web app or Mobile App backend in hello Azure Portal that you want toouse for this tutorial, you can skip ahead too[Create a Hybrid Connection and a BizTalk Service](#CreateHC) and start from there.</span></span>
> 
> 

1. <span data-ttu-id="582d5-123">V hello levého horního rohu hello [portálu Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="582d5-123">In hello upper left corner of hello [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web App**.</span></span>
   
    ![Nové webové aplikace][NewWebsite]
2. <span data-ttu-id="582d5-125">Na hello **webové aplikace** okno, zadejte adresu URL a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="582d5-125">On hello **Web app** blade, provide a URL and click **Create**.</span></span> 
   
    ![Název webu][WebsiteCreationBlade]
3. <span data-ttu-id="582d5-127">Po chvíli se hello webové aplikace se vytvoří a otevře se okno jeho webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="582d5-127">After a few moments, hello web app is created and its web app blade appears.</span></span> <span data-ttu-id="582d5-128">okno Hello se svisle posouvatelným řídicí panel, který vám umožní spravovat váš web.</span><span class="sxs-lookup"><span data-stu-id="582d5-128">hello blade is a vertically scrollable dashboard that lets you manage your site.</span></span>
   
    ![Spuštění webu][WebSiteRunningBlade]
4. <span data-ttu-id="582d5-130">Web tooverify hello je za provozu, můžete kliknout na hello **Procházet** ikonu toodisplay hello výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="582d5-130">tooverify hello site is live, you can click hello **Browse** icon toodisplay hello default page.</span></span>
   
    ![Klikněte na tlačítko Procházet toosee vaší webové aplikace][Browse]
   
    ![Výchozí webové aplikace stránky][DefaultWebSitePage]

<span data-ttu-id="582d5-133">V dalším kroku vytvoříte hybridní připojení a služby BizTalk pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="582d5-133">Next, you will create a hybrid connection and a BizTalk service for hello web app.</span></span>

<a name="CreateHC"></a>

## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="582d5-134">Vytvoření hybridní připojení a služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="582d5-134">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="582d5-135">V okně vaší webové aplikace klikněte na **všechna nastavení** > **sítě** > **nakonfigurovat koncové body hybridního připojení**.</span><span class="sxs-lookup"><span data-stu-id="582d5-135">In your web app blade click on **All settings** > **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hybridní připojení][CreateHCHCIcon]
2. <span data-ttu-id="582d5-137">V okně připojení hybridní hello, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="582d5-137">On hello Hybrid connections blade, click **Add**.</span></span>
   
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
   -->
3. <span data-ttu-id="582d5-138">Hello **přidat hybridní připojení** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="582d5-138">hello **Add a hybrid connection** blade opens.</span></span>  <span data-ttu-id="582d5-139">Vzhledem k tomu, že je to první hybridní připojení, hello **nové hybridní připojení** je předem vybrali možnost a hello **vytvořit hybridní připojení** otevře se okno pro vás.</span><span class="sxs-lookup"><span data-stu-id="582d5-139">Since this is your first hybrid connection, hello **New hybrid connection** option is preselected, and hello **Create hybrid connection** blade opens for you.</span></span>
   
    ![Vytvořit hybridní připojení.][TwinCreateHCBlades]
   
    <span data-ttu-id="582d5-141">Na hello **vytvořit hybridní připojení okno**:</span><span class="sxs-lookup"><span data-stu-id="582d5-141">On hello **Create hybrid connection blade**:</span></span>
   
   * <span data-ttu-id="582d5-142">Pro **název**, zadejte název připojení hello.</span><span class="sxs-lookup"><span data-stu-id="582d5-142">For **Name**, provide a name for hello connection.</span></span>
   * <span data-ttu-id="582d5-143">Pro **Hostname**, zadejte název hello hello na místní počítač, který je hostitelem prostředku.</span><span class="sxs-lookup"><span data-stu-id="582d5-143">For **Hostname**, enter hello name of hello on-premises computer that hosts your resource.</span></span>
   * <span data-ttu-id="582d5-144">Pro **Port**, zadejte číslo portu hello že místnímu prostředku používá (1433 pro výchozí instanci systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="582d5-144">For **Port**, enter hello port number that your on-premises resource uses (1433 for a SQL Server default instance).</span></span>
   * <span data-ttu-id="582d5-145">Klikněte na tlačítko **Biz Talk služby**</span><span class="sxs-lookup"><span data-stu-id="582d5-145">Click **Biz Talk Service**</span></span>
4. <span data-ttu-id="582d5-146">Hello **vytvořit službu BizTalk** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="582d5-146">hello **Create BizTalk Service** blade opens.</span></span> <span data-ttu-id="582d5-147">Zadejte název pro hello služby BizTalk a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="582d5-147">Enter a name for hello BizTalk service, and then click **OK**.</span></span>
   
    ![Vytvoření služby BizTalk][CreateHCCreateBTS]
   
    <span data-ttu-id="582d5-149">Hello **vytvořit službu BizTalk** okno se zavře a vrátíte se toohello **vytvořit hybridní připojení** okno.</span><span class="sxs-lookup"><span data-stu-id="582d5-149">hello **Create BizTalk Service** blade closes and you are returned toohello **Create hybrid connection** blade.</span></span>
5. <span data-ttu-id="582d5-150">V okně hello vytvořit hybridní připojení, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="582d5-150">On hello Create hybrid connection blade, click **OK**.</span></span> 
   
    ![Klikněte na tlačítko OK][CreateBTScomplete]
6. <span data-ttu-id="582d5-152">Po dokončení procesu hello hello oznamovací oblast v hello portál vás upozorní, že hello připojení byl úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="582d5-152">When hello process completes, hello notifications area in hello Portal informs you that hello connection has been successfully created.</span></span>
   
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
7. <span data-ttu-id="582d5-153">V okně webové aplikace hello hello **hybridní připojení** nyní zobrazuje ikona vytvořený 1 hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="582d5-153">On hello web app's blade, hello **Hybrid connections** icon now shows that 1 hybrid connection has been created.</span></span>
   
    ![Vytvoření jednoho hybridní připojení][CreateHCOneConnectionCreated]

<span data-ttu-id="582d5-155">V tomto okamžiku jste dokončili důležitou součástí infrastruktury hello cloudu hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="582d5-155">At this point, you have completed an important part of hello cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="582d5-156">V dalším kroku vytvoříte odpovídající část místně.</span><span class="sxs-lookup"><span data-stu-id="582d5-156">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a><span data-ttu-id="582d5-157">Instalace hello místní správce hybridního připojení toocomplete hello připojení</span><span class="sxs-lookup"><span data-stu-id="582d5-157">Install hello on-premises Hybrid Connection Manager toocomplete hello connection</span></span>
1. <span data-ttu-id="582d5-158">V okně hello webové aplikace, klikněte na tlačítko **všechna nastavení** > **sítě** > **nakonfigurovat koncové body hybridního připojení**.</span><span class="sxs-lookup"><span data-stu-id="582d5-158">On hello web app's blade, click **All settings** > **Networking** > **Configure your hybrid connection endpoints**.</span></span> 
   
    ![Ikona hybridní připojení][HCIcon]
2. <span data-ttu-id="582d5-160">Na hello **hybridní připojení** okno, hello **stav** nedávno přidat koncový bod zobrazuje sloupec pro hello **Nepřipojeno**.</span><span class="sxs-lookup"><span data-stu-id="582d5-160">On hello **Hybrid connections** blade, hello **Status** column for hello recently added endpoint shows **Not connected**.</span></span> <span data-ttu-id="582d5-161">Klikněte na tlačítko hello připojení tooconfigure ho.</span><span class="sxs-lookup"><span data-stu-id="582d5-161">Click hello connection tooconfigure it.</span></span>
   
    ![Není připojeno][NotConnected]
   
    <span data-ttu-id="582d5-163">Otevře se okno připojení hybridní Hello.</span><span class="sxs-lookup"><span data-stu-id="582d5-163">hello Hybrid connection blade opens.</span></span>
   
    ![NotConnectedBlade][NotConnectedBlade]
3. <span data-ttu-id="582d5-165">V okně hello, klikněte na tlačítko **naslouchací proces instalace**.</span><span class="sxs-lookup"><span data-stu-id="582d5-165">On hello blade, click **Listener Setup**.</span></span>
   
    ![Klikněte na tlačítko naslouchací proces instalace][ClickListenerSetup]
4. <span data-ttu-id="582d5-167">Hello **vlastnosti hybridní připojení** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="582d5-167">hello **Hybrid connection properties** blade opens.</span></span> <span data-ttu-id="582d5-168">V části **místní správce hybridního připojení**, zvolte **kliknutím sem tooinstall**.</span><span class="sxs-lookup"><span data-stu-id="582d5-168">Under **On-premises Hybrid Connection Manager**, choose **Click here tooinstall**.</span></span>
   
    ![Kliknutím sem tooinstall][ClickToInstallHCM]
5. <span data-ttu-id="582d5-170">V hello spustit aplikaci zabezpečení dialogové okno s upozorněním, zvolte **spustit** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="582d5-170">In hello Application Run security warning dialog, choose **Run** toocontinue.</span></span>
   
    ![Zvolte toocontinue spustit][ApplicationRunWarning]
6. <span data-ttu-id="582d5-172">V hello **řízení uživatelských účtů** dialogovém okně, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="582d5-172">In hello **User Account Control** dialog, choose **Yes**.</span></span>
   
   ![Klepněte na tlačítko Ano][UAC]
7. <span data-ttu-id="582d5-174">Hello správce hybridního připojení je stažen a nainstalován za vás.</span><span class="sxs-lookup"><span data-stu-id="582d5-174">hello Hybrid Connection Manager is downloaded and installed for you.</span></span> 
   
    ![Instalace][HCMInstalling]
8. <span data-ttu-id="582d5-176">Po dokončení instalace hello, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="582d5-176">When hello install completes, click **Close**.</span></span>
   
    ![Klikněte na tlačítko Zavřít][HCMInstallComplete]
   
    <span data-ttu-id="582d5-178">Na hello **hybridní připojení** okno, hello **stav** teď zobrazuje sloupec **připojeno**.</span><span class="sxs-lookup"><span data-stu-id="582d5-178">On hello **Hybrid connections** blade, hello **Status** column now shows **Connected**.</span></span> 
   
    ![Připojené stav][HCStatusConnected]

<span data-ttu-id="582d5-180">Nyní je kompletní tuto infrastrukturu hello hybridní připojení, můžete vytvořit hybridní aplikace, která jej používá.</span><span class="sxs-lookup"><span data-stu-id="582d5-180">Now that hello hybrid connection infrastructure is complete, you can create a hybrid application that uses it.</span></span> 

> [!NOTE]
> <span data-ttu-id="582d5-181">Hello následující části ukazují, jak toouse hybridní připojení s projektem back-end mobilní aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="582d5-181">hello following sections show you how toouse a hybrid connection with a Mobile Apps .NET backend project.</span></span>
> 
> 

## <a name="configure-hello-mobile-app-net-backend-project-tooconnect-toohello-sql-server-database"></a><span data-ttu-id="582d5-182">Konfigurace hello mobilní aplikace .NET back-end projektu tooconnect toohello databázi systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="582d5-182">Configure hello Mobile App .NET backend project tooconnect toohello SQL Server database</span></span>
<span data-ttu-id="582d5-183">Ve službě App Service projektu back-end mobilní aplikace .NET je právě webové aplikace ASP.NET s další Mobile Apps SDK nainstalován a inicializován.</span><span class="sxs-lookup"><span data-stu-id="582d5-183">In App Service, a Mobile Apps .NET backend project is just an ASP.NET web app with an additional Mobile Apps SDK installed and initialized.</span></span> <span data-ttu-id="582d5-184">toouse vaší webové aplikace jako back-end mobilní aplikace, musíte [stáhnout a inicializace hello mobilní aplikace .NET back-end SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).</span><span class="sxs-lookup"><span data-stu-id="582d5-184">toouse your web app as a Mobile Apps backend, you must [download and initialize hello Mobile Apps .NET backend SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).</span></span>  

<span data-ttu-id="582d5-185">Pro Mobile Apps také potřebovat toodefine připojovací řetězec pro hello místní databázi a upravte toouse hello back-end toto připojení.</span><span class="sxs-lookup"><span data-stu-id="582d5-185">For Mobile Apps, you also need toodefine a connection string for hello on-premises database and modify hello backend toouse this connection.</span></span> 

1. <span data-ttu-id="582d5-186">V Průzkumníku řešení v sadě Visual Studio, otevřete hello souboru Web.config back-endu mobilní aplikace .NET, vyhledejte hello **connectionStrings** přidejte nový záznam SqlClient jako hello následující, které body toohello místní SQL Databáze serveru:</span><span class="sxs-lookup"><span data-stu-id="582d5-186">In Solution Explorer in Visual Studio, open hello Web.config file for your Mobile App .NET backend, locate hello **connectionStrings** section, add a new SqlClient entry like hello following, which points toohello on-premises SQL Server database:</span></span>
   
        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />
   
    <span data-ttu-id="582d5-187">Mějte na paměti, tooreplace `<**secure_password**>` do tohoto řetězce s heslem hello jste vytvořili pro *HybridConnectionLogin*.</span><span class="sxs-lookup"><span data-stu-id="582d5-187">Remember tooreplace `<**secure_password**>` in this string with hello password you created for  *HybridConnectionLogin*.</span></span>
2. <span data-ttu-id="582d5-188">Klikněte na tlačítko **Uložit** v souboru Web.config hello toosave Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="582d5-188">Click **Save** in Visual Studio toosave hello Web.config file.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="582d5-189">Toto nastavení připojení se používá při spuštění v místním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="582d5-189">This connection setting is used when running on hello local computer.</span></span> <span data-ttu-id="582d5-190">Při spuštění v Azure, je toto nastavení přepsat nastavení připojení hello definované hello portálu.</span><span class="sxs-lookup"><span data-stu-id="582d5-190">When running in Azure, this setting is overriden by hello connection setting defined in hello portal.</span></span>
   > 
   > 
3. <span data-ttu-id="582d5-191">Rozbalte hello **modely** složku a otevřete hello datového modelu souboru, který končí v *Context.cs*.</span><span class="sxs-lookup"><span data-stu-id="582d5-191">Expand hello **Models** folder and open hello data model file, which ends in *Context.cs*.</span></span>
4. <span data-ttu-id="582d5-192">Upravit hello **DbContext** toopass konstruktor instance hello hodnotu `OnPremisesDBConnection` toohello základní **DbContext** konstruktor, podobně jako toohello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="582d5-192">Modify hello **DbContext** instance constructor toopass hello value `OnPremisesDBConnection` toohello base **DbContext** constructor, similar toohello following snippet:</span></span>
   
        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }
   
    <span data-ttu-id="582d5-193">Služba Hello nyní použije hello nové připojení toohello databáze SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="582d5-193">hello service will now use hello new connection toohello SQL Server database.</span></span>

## <a name="update-hello-mobile-app-backend-toouse-hello-on-premises-connection-string"></a><span data-ttu-id="582d5-194">Aktualizovat připojovací řetězec místní toouse hello hello mobilní aplikace back-end</span><span class="sxs-lookup"><span data-stu-id="582d5-194">Update hello Mobile App backend toouse hello on-premises connection string</span></span>
<span data-ttu-id="582d5-195">Dále musíte tooadd nastavení aplikace pro tento nový připojovací řetězec, aby ho bylo možné použít z Azure.</span><span class="sxs-lookup"><span data-stu-id="582d5-195">Next, you need tooadd an app setting for this new connection string so that it can be used from Azure.</span></span>  

1. <span data-ttu-id="582d5-196">Zpět v hello [portál Azure](https://portal.azure.com) v hello kódu webové aplikace back-end mobilní aplikace, klikněte na **všechna nastavení**, pak **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="582d5-196">Back in hello [Azure portal](https://portal.azure.com) in hello web app backend code for your Mobile App, click **All settings**, then **Application settings**.</span></span>
2. <span data-ttu-id="582d5-197">V hello **webové nastavení aplikace** okno, posuňte se dolů příliš**připojovací řetězce** a přidejte nové **systému SQL Server** připojovací řetězec s názvem `OnPremisesDBConnection` s hodnotou jako `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.</span><span class="sxs-lookup"><span data-stu-id="582d5-197">In hello **Web app settings** blade, scroll down too**Connection strings** and add an new **SQL Server** connection string named `OnPremisesDBConnection` with a value like `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.</span></span>
   
    <span data-ttu-id="582d5-198">Nahraďte `<**secure_password**>` s hello zabezpečeného hesla pro místní databázi.</span><span class="sxs-lookup"><span data-stu-id="582d5-198">Replace `<**secure_password**>` with hello secure password for your on-premises database.</span></span>
   
    ![Připojovací řetězec pro místní databázi](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)
3. <span data-ttu-id="582d5-200">Stiskněte klávesu **Uložit** toosave hello hybridní připojení a připojovací řetězec jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="582d5-200">Press **Save** toosave hello hybrid connection and connection string you just created.</span></span>

<span data-ttu-id="582d5-201">V tomto okamžiku můžete znovu publikovat hello serverový projekt a otestovat hello nové připojení s vaší stávající klienty mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="582d5-201">At this point you can republish hello server project and test hello new connection with your existing Mobile Apps clients.</span></span> <span data-ttu-id="582d5-202">Data budou číst a zapisovat toohello místní databázi pomocí hello hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="582d5-202">Data will be read from and written toohello on-premises database using hello hybrid connection.</span></span>

<a name="NextSteps"></a>

## <a name="next-steps"></a><span data-ttu-id="582d5-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="582d5-203">Next Steps</span></span>
* <span data-ttu-id="582d5-204">Informace o vytváření webové aplikace ASP.NET, který používá hybridní připojení najdete v tématu [tooan připojit místní systém SQL Server z Azure webu pomocí hybridních připojení](http://go.microsoft.com/fwlink/?LinkID=397979).</span><span class="sxs-lookup"><span data-stu-id="582d5-204">For information on creating an ASP.NET web application that uses a hybrid connection, see [Connect tooan on-premises SQL Server from an Azure web site using Hybrid Connections](http://go.microsoft.com/fwlink/?LinkID=397979).</span></span> 

### <a name="additional-resources"></a><span data-ttu-id="582d5-205">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="582d5-205">Additional Resources</span></span>
[<span data-ttu-id="582d5-206">Přehled hybridních připojení</span><span class="sxs-lookup"><span data-stu-id="582d5-206">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="582d5-207">Josh Točitost zavádí hybridní připojení (video na kanálu 9)</span><span class="sxs-lookup"><span data-stu-id="582d5-207">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="582d5-208">Hybridní připojení webu</span><span class="sxs-lookup"><span data-stu-id="582d5-208">Hybrid Connections web site</span></span>](https://azure.microsoft.com/services/biztalk-services/)

[<span data-ttu-id="582d5-209">BizTalk Services: Karty řídicí panel, sledování, škálování, konfigurovat a hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="582d5-209">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="582d5-210">Vytváření reálného hybridního cloudu s bezproblémové přenositelnost aplikace (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="582d5-210">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="582d5-211">Připojit tooan místní SQL Server z Azure Mobile Services používat hybridní připojení (video na kanálu 9)</span><span class="sxs-lookup"><span data-stu-id="582d5-211">Connect tooan on-premises SQL Server from Azure Mobile Services using Hybrid Connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a><span data-ttu-id="582d5-212">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="582d5-212">What's changed</span></span>
* <span data-ttu-id="582d5-213">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="582d5-213">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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
