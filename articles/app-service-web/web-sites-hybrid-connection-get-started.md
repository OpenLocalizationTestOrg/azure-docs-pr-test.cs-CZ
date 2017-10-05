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
# <a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a><span data-ttu-id="4fa45-103">Přístup k místním prostředkům pomocí hybridních připojení v prostředí Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4fa45-103">Access on-premises resources using hybrid connections in Azure App Service</span></span>
<span data-ttu-id="4fa45-104">Aplikace služby Azure App Service může připojit k místnímu prostředku, který používá statický port TCP, jako je například SQL Server, MySQL, webové rozhraní API HTTP a většina vlastních webových služeb.</span><span class="sxs-lookup"><span data-stu-id="4fa45-104">You can connect an Azure App Service app to any on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="4fa45-105">Tento článek ukazuje, jak vytvořit hybridní připojení mezi služby App Service a místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4fa45-105">This article shows you how to create a hybrid connection between App Service and an on-premises SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="4fa45-106">Je k dispozici pouze v části webových aplikací funkci hybridní připojení [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4fa45-106">The Web Apps portion of the Hybrid Connections feature is available only in the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="4fa45-107">Vytvoření připojení ve službě BizTalk Services naleznete v tématu [hybridní připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="4fa45-107">To create a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span> 
> 
> <span data-ttu-id="4fa45-108">Tento obsah platí také pro Mobile Apps v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4fa45-108">This content also applies to Mobile Apps in Azure App Service.</span></span> 
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="4fa45-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4fa45-109">Prerequisites</span></span>
* <span data-ttu-id="4fa45-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4fa45-110">An Azure subscription.</span></span> <span data-ttu-id="4fa45-111">Bezplatné předplatné, najdete v části [bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4fa45-111">For a free subscription, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
  
    <span data-ttu-id="4fa45-112">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4fa45-112">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="4fa45-113">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="4fa45-113">No credit cards required; no commitments.</span></span>
* <span data-ttu-id="4fa45-114">Používat hybridní připojení místní databázi systému SQL Server nebo SQL Server Express, musí být povolená na statický port TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="4fa45-114">To use an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs to be enabled on a static port.</span></span> <span data-ttu-id="4fa45-115">Pomocí výchozí instance na serveru SQL Server je doporučeno, protože používá statický port 1433.</span><span class="sxs-lookup"><span data-stu-id="4fa45-115">Using a default instance on SQL Server is recommended because it uses static port 1433.</span></span> <span data-ttu-id="4fa45-116">Informace o instalaci a konfiguraci systému SQL Server Express pro použití s hybridní připojení najdete v tématu [připojení k serveru SQL místní z Azure webu pomocí hybridních připojení](http://go.microsoft.com/fwlink/?LinkID=397979).</span><span class="sxs-lookup"><span data-stu-id="4fa45-116">For information on installing and configuring SQL Server Express for use with hybrid connections, see [Connect to an on-premises SQL Server from an Azure web site using Hybrid Connections](http://go.microsoft.com/fwlink/?LinkID=397979).</span></span>
* <span data-ttu-id="4fa45-117">Počítač, na který instalujete místní správce hybridního připojení agenta popsané dále v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="4fa45-117">The computer on which you install the on-premises Hybrid Connection Manager agent described later in this article:</span></span>
  
  * <span data-ttu-id="4fa45-118">Musí být schopný se připojit k Azure přes port 5671</span><span class="sxs-lookup"><span data-stu-id="4fa45-118">Must be able to connect to Azure over port 5671</span></span>
  * <span data-ttu-id="4fa45-119">Musí být schopen kontaktovat *hostname*:*ČísloPortu* vaše místní prostředku.</span><span class="sxs-lookup"><span data-stu-id="4fa45-119">Must be able to reach the *hostname*:*portnumber* of your on-premises resource.</span></span> 

> [!NOTE]
> <span data-ttu-id="4fa45-120">Postup v tomto článku předpokládá, že používáte prohlížeč z počítače, který bude hostitelem agenta místní hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="4fa45-120">The steps in this article assume that you are using the browser from the computer that will host the on-premises hybrid connection agent.</span></span>
> 
> 

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="4fa45-121">Vytvořit webovou aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4fa45-121">Create a web app in the Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="4fa45-122">Pokud jste již vytvořili webovou aplikaci nebo back-end mobilní aplikace na portálu Azure, který chcete použít pro tento kurz, můžete přeskočit na [vytvořit hybridní připojení a služby BizTalk](#CreateHC) a spustit z ní.</span><span class="sxs-lookup"><span data-stu-id="4fa45-122">If you have already created a web app or Mobile App backend in the Azure Portal that you want to use for this tutorial, you can skip ahead to [Create a Hybrid Connection and a BizTalk Service](#CreateHC) and start from there.</span></span>
> 
> 

1. <span data-ttu-id="4fa45-123">V levém horním rohu [portálu Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-123">In the upper left corner of the [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web App**.</span></span>
   
    ![Nové webové aplikace][NewWebsite]
2. <span data-ttu-id="4fa45-125">Na **webové aplikace** okno, zadejte adresu URL a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-125">On the **Web app** blade, provide a URL and click **Create**.</span></span> 
   
    ![Název webu][WebsiteCreationBlade]
3. <span data-ttu-id="4fa45-127">Po chvíli se webové aplikace se vytvoří a otevře se okno jeho webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fa45-127">After a few moments, the web app is created and its web app blade appears.</span></span> <span data-ttu-id="4fa45-128">V okně se svisle posouvatelným řídicí panel, který vám umožní spravovat váš web.</span><span class="sxs-lookup"><span data-stu-id="4fa45-128">The blade is a vertically scrollable dashboard that lets you manage your site.</span></span>
   
    ![Spuštění webu][WebSiteRunningBlade]
4. <span data-ttu-id="4fa45-130">Pokud chcete ověřit, web je za provozu, můžete kliknout na **Procházet** ikonu zobrazíte výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="4fa45-130">To verify the site is live, you can click the **Browse** icon to display the default page.</span></span>
   
    ![Klikněte na tlačítko Procházet zobrazíte vaší webové aplikace][Browse]
   
    ![Výchozí webové aplikace stránky][DefaultWebSitePage]

<span data-ttu-id="4fa45-133">V dalším kroku vytvoříte hybridní připojení a služba BizTalk webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fa45-133">Next, you will create a hybrid connection and a BizTalk service for the web app.</span></span>

<a name="CreateHC"></a>

## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="4fa45-134">Vytvoření hybridní připojení a služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="4fa45-134">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="4fa45-135">V okně vaší webové aplikace klikněte na **všechna nastavení** > **sítě** > **nakonfigurovat koncové body hybridního připojení**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-135">In your web app blade click on **All settings** > **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hybridní připojení][CreateHCHCIcon]
2. <span data-ttu-id="4fa45-137">V okně hybridní připojení, klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-137">On the Hybrid connections blade, click **Add**.</span></span>
   
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
   -->
3. <span data-ttu-id="4fa45-138">**Přidat hybridní připojení** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="4fa45-138">The **Add a hybrid connection** blade opens.</span></span>  <span data-ttu-id="4fa45-139">Vzhledem k tomu, že toto je první hybridní připojení **nové hybridní připojení** je předem vybrali možnost a **vytvořit hybridní připojení** otevře se okno pro vás.</span><span class="sxs-lookup"><span data-stu-id="4fa45-139">Since this is your first hybrid connection, the **New hybrid connection** option is preselected, and the **Create hybrid connection** blade opens for you.</span></span>
   
    ![Vytvořit hybridní připojení.][TwinCreateHCBlades]
   
    <span data-ttu-id="4fa45-141">Na **vytvořit hybridní připojení okno**:</span><span class="sxs-lookup"><span data-stu-id="4fa45-141">On the **Create hybrid connection blade**:</span></span>
   
   * <span data-ttu-id="4fa45-142">Pro **název**, zadejte název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="4fa45-142">For **Name**, provide a name for the connection.</span></span>
   * <span data-ttu-id="4fa45-143">Pro **Hostname**, zadejte název místního počítače, který hostuje prostředku.</span><span class="sxs-lookup"><span data-stu-id="4fa45-143">For **Hostname**, enter the name of the on-premises computer that hosts your resource.</span></span>
   * <span data-ttu-id="4fa45-144">Pro **Port**, zadejte číslo portu, aby používal místnímu prostředku (1433 pro výchozí instanci systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="4fa45-144">For **Port**, enter the port number that your on-premises resource uses (1433 for a SQL Server default instance).</span></span>
   * <span data-ttu-id="4fa45-145">Klikněte na tlačítko **Biz Talk služby**</span><span class="sxs-lookup"><span data-stu-id="4fa45-145">Click **Biz Talk Service**</span></span>
4. <span data-ttu-id="4fa45-146">**Vytvořit službu BizTalk** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="4fa45-146">The **Create BizTalk Service** blade opens.</span></span> <span data-ttu-id="4fa45-147">Zadejte název pro službu BizTalk a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-147">Enter a name for the BizTalk service, and then click **OK**.</span></span>
   
    ![Vytvoření služby BizTalk][CreateHCCreateBTS]
   
    <span data-ttu-id="4fa45-149">**Vytvořit službu BizTalk** okno se zavře a vrátíte se **vytvořit hybridní připojení** okno.</span><span class="sxs-lookup"><span data-stu-id="4fa45-149">The **Create BizTalk Service** blade closes and you are returned to the **Create hybrid connection** blade.</span></span>
5. <span data-ttu-id="4fa45-150">V okně Vytvořit hybridní připojení, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-150">On the Create hybrid connection blade, click **OK**.</span></span> 
   
    ![Klikněte na tlačítko OK][CreateBTScomplete]
6. <span data-ttu-id="4fa45-152">Po dokončení procesu oblast oznámení na portálu vás upozorní, že připojení byl úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="4fa45-152">When the process completes, the notifications area in the Portal informs you that the connection has been successfully created.</span></span>
   
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
7. <span data-ttu-id="4fa45-153">V okně webové aplikace **hybridní připojení** nyní zobrazuje ikona vytvořený 1 hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="4fa45-153">On the web app's blade, the **Hybrid connections** icon now shows that 1 hybrid connection has been created.</span></span>
   
    ![Vytvoření jednoho hybridní připojení][CreateHCOneConnectionCreated]

<span data-ttu-id="4fa45-155">V tomto okamžiku jste dokončili důležitou součástí infrastruktury cloudu hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="4fa45-155">At this point, you have completed an important part of the cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="4fa45-156">V dalším kroku vytvoříte odpovídající část místně.</span><span class="sxs-lookup"><span data-stu-id="4fa45-156">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a><span data-ttu-id="4fa45-157">Instalovat místní správce hybridního připojení k připojení</span><span class="sxs-lookup"><span data-stu-id="4fa45-157">Install the on-premises Hybrid Connection Manager to complete the connection</span></span>
1. <span data-ttu-id="4fa45-158">V okně webové aplikace, klikněte na tlačítko **všechna nastavení** > **sítě** > **nakonfigurovat koncové body hybridního připojení**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-158">On the web app's blade, click **All settings** > **Networking** > **Configure your hybrid connection endpoints**.</span></span> 
   
    ![Ikona hybridní připojení][HCIcon]
2. <span data-ttu-id="4fa45-160">Na **hybridní připojení** okně **stav** sloupec pro ukazuje nedávno přidané koncový bod **Nepřipojeno**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-160">On the **Hybrid connections** blade, the **Status** column for the recently added endpoint shows **Not connected**.</span></span> <span data-ttu-id="4fa45-161">Klikněte na připojení k jeho konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="4fa45-161">Click the connection to configure it.</span></span>
   
    ![Není připojeno][NotConnected]
   
    <span data-ttu-id="4fa45-163">Otevře se okno hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="4fa45-163">The Hybrid connection blade opens.</span></span>
   
    ![NotConnectedBlade][NotConnectedBlade]
3. <span data-ttu-id="4fa45-165">V okně klikněte na tlačítko **naslouchací proces instalace**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-165">On the blade, click **Listener Setup**.</span></span>
   
    ![Klikněte na tlačítko naslouchací proces instalace][ClickListenerSetup]
4. <span data-ttu-id="4fa45-167">**Vlastnosti hybridní připojení** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="4fa45-167">The **Hybrid connection properties** blade opens.</span></span> <span data-ttu-id="4fa45-168">V části **místní správce hybridního připojení**, zvolte **nainstalujete kliknutím sem.**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-168">Under **On-premises Hybrid Connection Manager**, choose **Click here to install**.</span></span>
   
    ![Chcete-li nainstalovat, klikněte sem][ClickToInstallHCM]
5. <span data-ttu-id="4fa45-170">V aplikaci spustit zabezpečení dialogové okno s upozorněním, zvolte **spustit** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="4fa45-170">In the Application Run security warning dialog, choose **Run** to continue.</span></span>
   
    ![Zvolte spustit a pokračovat][ApplicationRunWarning]
6. <span data-ttu-id="4fa45-172">V **řízení uživatelských účtů** dialogovém okně, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-172">In the **User Account Control** dialog, choose **Yes**.</span></span>
   
   ![Klepněte na tlačítko Ano][UAC]
7. <span data-ttu-id="4fa45-174">Správce hybridního připojení je stažen a nainstalován za vás.</span><span class="sxs-lookup"><span data-stu-id="4fa45-174">The Hybrid Connection Manager is downloaded and installed for you.</span></span> 
   
    ![Instalace][HCMInstalling]
8. <span data-ttu-id="4fa45-176">Po dokončení instalace klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-176">When the install completes, click **Close**.</span></span>
   
    ![Klikněte na tlačítko Zavřít][HCMInstallComplete]
   
    <span data-ttu-id="4fa45-178">Na **hybridní připojení** okně **stav** teď zobrazuje sloupec **připojeno**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-178">On the **Hybrid connections** blade, the **Status** column now shows **Connected**.</span></span> 
   
    ![Připojené stav][HCStatusConnected]

<span data-ttu-id="4fa45-180">Teď, když je kompletní infrastrukturu hybridní připojení, můžete vytvořit hybridní aplikace, která jej používá.</span><span class="sxs-lookup"><span data-stu-id="4fa45-180">Now that the hybrid connection infrastructure is complete, you can create a hybrid application that uses it.</span></span> 

> [!NOTE]
> <span data-ttu-id="4fa45-181">Následující části ukazují, jak používat hybridní připojení s projektem back-end mobilní aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="4fa45-181">The following sections show you how to use a hybrid connection with a Mobile Apps .NET backend project.</span></span>
> 
> 

## <a name="configure-the-mobile-app-net-backend-project-to-connect-to-the-sql-server-database"></a><span data-ttu-id="4fa45-182">Konfigurace projektu back-end mobilní aplikace .NET pro připojení k databázi systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="4fa45-182">Configure the Mobile App .NET backend project to connect to the SQL Server database</span></span>
<span data-ttu-id="4fa45-183">Ve službě App Service projektu back-end mobilní aplikace .NET je právě webové aplikace ASP.NET s další Mobile Apps SDK nainstalován a inicializován.</span><span class="sxs-lookup"><span data-stu-id="4fa45-183">In App Service, a Mobile Apps .NET backend project is just an ASP.NET web app with an additional Mobile Apps SDK installed and initialized.</span></span> <span data-ttu-id="4fa45-184">Používat webové aplikace jako back-end mobilní aplikace, musíte [stáhnout a inicializace back-end mobilní aplikace .NET SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).</span><span class="sxs-lookup"><span data-stu-id="4fa45-184">To use your web app as a Mobile Apps backend, you must [download and initialize the Mobile Apps .NET backend SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).</span></span>  

<span data-ttu-id="4fa45-185">Pro mobilní aplikace musíte také připojovací řetězec pro místní databázi definování a úprava back-end využívání tohoto připojení.</span><span class="sxs-lookup"><span data-stu-id="4fa45-185">For Mobile Apps, you also need to define a connection string for the on-premises database and modify the backend to use this connection.</span></span> 

1. <span data-ttu-id="4fa45-186">V Průzkumníku řešení v sadě Visual Studio otevřete soubor Web.config pro váš back-end mobilní aplikace .NET, vyhledejte **connectionStrings** přidejte nový záznam SqlClient jako následující text, který odkazuje na místní databázi systému SQL Server:</span><span class="sxs-lookup"><span data-stu-id="4fa45-186">In Solution Explorer in Visual Studio, open the Web.config file for your Mobile App .NET backend, locate the **connectionStrings** section, add a new SqlClient entry like the following, which points to the on-premises SQL Server database:</span></span>
   
        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />
   
    <span data-ttu-id="4fa45-187">Nezapomeňte nahradit `<**secure_password**>` do tohoto řetězce s heslem, které jste vytvořili pro *HybridConnectionLogin*.</span><span class="sxs-lookup"><span data-stu-id="4fa45-187">Remember to replace `<**secure_password**>` in this string with the password you created for  *HybridConnectionLogin*.</span></span>
2. <span data-ttu-id="4fa45-188">Klikněte na tlačítko **Uložit** v sadě Visual Studio k uložení souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="4fa45-188">Click **Save** in Visual Studio to save the Web.config file.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4fa45-189">Toto nastavení připojení se používá při spuštění v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="4fa45-189">This connection setting is used when running on the local computer.</span></span> <span data-ttu-id="4fa45-190">Při spuštění v Azure, je toto nastavení přepsat nastavení připojení definované v portálu.</span><span class="sxs-lookup"><span data-stu-id="4fa45-190">When running in Azure, this setting is overriden by the connection setting defined in the portal.</span></span>
   > 
   > 
3. <span data-ttu-id="4fa45-191">Rozbalte **modely** složky a otevřete soubor modelu dat, které končí na *Context.cs*.</span><span class="sxs-lookup"><span data-stu-id="4fa45-191">Expand the **Models** folder and open the data model file, which ends in *Context.cs*.</span></span>
4. <span data-ttu-id="4fa45-192">Změnit **DbContext** konstruktoru instance předat hodnotu `OnPremisesDBConnection` s jejím základem **DbContext** konstruktoru, podobně jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="4fa45-192">Modify the **DbContext** instance constructor to pass the value `OnPremisesDBConnection` to the base **DbContext** constructor, similar to the following snippet:</span></span>
   
        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }
   
    <span data-ttu-id="4fa45-193">Služba bude nyní používat nové připojení k databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4fa45-193">The service will now use the new connection to the SQL Server database.</span></span>

## <a name="update-the-mobile-app-backend-to-use-the-on-premises-connection-string"></a><span data-ttu-id="4fa45-194">Aktualizovat back-end mobilní aplikace použít místní připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="4fa45-194">Update the Mobile App backend to use the on-premises connection string</span></span>
<span data-ttu-id="4fa45-195">Dále musíte přidat nastavení aplikace pro tento nový připojovací řetězec, aby ho bylo možné použít z Azure.</span><span class="sxs-lookup"><span data-stu-id="4fa45-195">Next, you need to add an app setting for this new connection string so that it can be used from Azure.</span></span>  

1. <span data-ttu-id="4fa45-196">Zpět v [portál Azure](https://portal.azure.com) ve webové aplikaci back-end kód mobilní aplikace, klikněte na tlačítko **všechna nastavení**, pak **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4fa45-196">Back in the [Azure portal](https://portal.azure.com) in the web app backend code for your Mobile App, click **All settings**, then **Application settings**.</span></span>
2. <span data-ttu-id="4fa45-197">V **webové nastavení aplikace** okno, přejděte dolů k **připojovací řetězce** a přidejte nové **systému SQL Server** připojovací řetězec s názvem `OnPremisesDBConnection` s hodnotou jako `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.</span><span class="sxs-lookup"><span data-stu-id="4fa45-197">In the **Web app settings** blade, scroll down to **Connection strings** and add an new **SQL Server** connection string named `OnPremisesDBConnection` with a value like `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.</span></span>
   
    <span data-ttu-id="4fa45-198">Nahraďte `<**secure_password**>` zabezpečeného heslem pro váš místní databázi.</span><span class="sxs-lookup"><span data-stu-id="4fa45-198">Replace `<**secure_password**>` with the secure password for your on-premises database.</span></span>
   
    ![Připojovací řetězec pro místní databázi](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)
3. <span data-ttu-id="4fa45-200">Stiskněte klávesu **Uložit** uložit hybridní připojení a připojení řetězec jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4fa45-200">Press **Save** to save the hybrid connection and connection string you just created.</span></span>

<span data-ttu-id="4fa45-201">V tomto okamžiku můžete znovu publikovat serverový projekt a otestujte nové připojení s vaší stávající klienty Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="4fa45-201">At this point you can republish the server project and test the new connection with your existing Mobile Apps clients.</span></span> <span data-ttu-id="4fa45-202">Data budou číst z a zapisovat do místní databáze pomocí hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="4fa45-202">Data will be read from and written to the on-premises database using the hybrid connection.</span></span>

<a name="NextSteps"></a>

## <a name="next-steps"></a><span data-ttu-id="4fa45-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4fa45-203">Next Steps</span></span>
* <span data-ttu-id="4fa45-204">Informace o vytváření webové aplikace ASP.NET, který používá hybridní připojení najdete v tématu [připojení k serveru SQL místní z Azure webu pomocí hybridních připojení](http://go.microsoft.com/fwlink/?LinkID=397979).</span><span class="sxs-lookup"><span data-stu-id="4fa45-204">For information on creating an ASP.NET web application that uses a hybrid connection, see [Connect to an on-premises SQL Server from an Azure web site using Hybrid Connections](http://go.microsoft.com/fwlink/?LinkID=397979).</span></span> 

### <a name="additional-resources"></a><span data-ttu-id="4fa45-205">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4fa45-205">Additional Resources</span></span>
[<span data-ttu-id="4fa45-206">Přehled hybridních připojení</span><span class="sxs-lookup"><span data-stu-id="4fa45-206">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="4fa45-207">Josh Točitost zavádí hybridní připojení (video na kanálu 9)</span><span class="sxs-lookup"><span data-stu-id="4fa45-207">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="4fa45-208">Hybridní připojení webu</span><span class="sxs-lookup"><span data-stu-id="4fa45-208">Hybrid Connections web site</span></span>](https://azure.microsoft.com/services/biztalk-services/)

[<span data-ttu-id="4fa45-209">BizTalk Services: Karty řídicí panel, sledování, škálování, konfigurovat a hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="4fa45-209">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="4fa45-210">Vytváření reálného hybridního cloudu s bezproblémové přenositelnost aplikace (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="4fa45-210">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="4fa45-211">Připojit k serveru SQL místní z Azure Mobile Services používat hybridní připojení (video na kanálu 9)</span><span class="sxs-lookup"><span data-stu-id="4fa45-211">Connect to an on-premises SQL Server from Azure Mobile Services using Hybrid Connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a><span data-ttu-id="4fa45-212">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="4fa45-212">What's changed</span></span>
* <span data-ttu-id="4fa45-213">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="4fa45-213">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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
