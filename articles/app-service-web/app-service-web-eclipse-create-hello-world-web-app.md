---
title: "Vytvořte základní Azure webovou aplikaci pomocí Eclipse | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak používat Azure nástrojů pro Eclipse k vytvoření Hello World webové aplikace pro Azure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: 683d6828546995a4dc60d8cac0669f356501c723
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a><span data-ttu-id="66236-103">Vytvořte základní Azure webovou aplikaci pomocí Eclipse</span><span class="sxs-lookup"><span data-stu-id="66236-103">Create a basic Azure web app using Eclipse</span></span>
<span data-ttu-id="66236-104">Tento kurz ukazuje, jak vytvořit a nasadit základní aplikace Hello World do Azure jako webovou aplikaci pomocí [nástrojů Azure pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="66236-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a Web App by using the [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="66236-105">Základní příklad JSP je uvedený na jednoduchost, ale podobným způsobem může být vhodné pro Java servlet, co se týče nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="66236-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="66236-106">Po dokončení tohoto kurzu, vaše aplikace bude vypadat podobně jako na následujícím obrázku, při zobrazení ve webovém prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="66236-106">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Verze Preview Hello World aplikace][01]

## <a name="prerequisites"></a><span data-ttu-id="66236-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="66236-108">Prerequisites</span></span>
* <span data-ttu-id="66236-109">Java Developer Kit (JDK), v 1.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="66236-109">A Java Developer Kit (JDK), v 1.8 or later.</span></span>
* <span data-ttu-id="66236-110">Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE, vzhled Luna nebo novější.</span><span class="sxs-lookup"><span data-stu-id="66236-110">Eclipse IDE for Java EE Developers, Luna or later.</span></span> <span data-ttu-id="66236-111">To si můžete stáhnout z <http://www.eclipse.org/downloads/>.</span><span class="sxs-lookup"><span data-stu-id="66236-111">This can be downloaded from <http://www.eclipse.org/downloads/>.</span></span>
* <span data-ttu-id="66236-112">Distribuční založené na jazyce Java webový server nebo aplikačního serveru jako například [Apache Tomcat] nebo [Jetty].</span><span class="sxs-lookup"><span data-stu-id="66236-112">A distribution of a Java-based web server or application server, such as [Apache Tomcat] or [Jetty].</span></span>
* <span data-ttu-id="66236-113">Předplatné Azure, který můžete získat z <https://azure.microsoft.com/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.</span><span class="sxs-lookup"><span data-stu-id="66236-113">An Azure subscription, which can be acquired from <https://azure.microsoft.com/free/> or <http://azure.microsoft.com/pricing/purchase-options/>.</span></span>
* <span data-ttu-id="66236-114">[nástrojů Azure pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="66236-114">The [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="66236-115">Informace o instalaci sady Azure najdete v tématu [instalaci sady nástrojů Azure pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="66236-115">For information about installing the Azure Toolkit, see [Installing the Azure Toolkit for Eclipse].</span></span>

## <a name="to-create-a-hello-world-application"></a><span data-ttu-id="66236-116">Vytvoření aplikace Hello World</span><span class="sxs-lookup"><span data-stu-id="66236-116">To create a Hello World application</span></span>
<span data-ttu-id="66236-117">Nejdříve začneme s vytvořením projektu Java.</span><span class="sxs-lookup"><span data-stu-id="66236-117">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="66236-118">Spusťte Eclipse a v nabídce klikněte na položku **soubor**, klikněte na tlačítko **nový**a potom klikněte na **Dynamic Web Project**.</span><span class="sxs-lookup"><span data-stu-id="66236-118">Start Eclipse, and at the menu click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="66236-119">(Pokud nevidíte **Dynamic Web Project** uvedené jako dostupné projekt po kliknutí na **soubor** a **nový**, postupujte takto: klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**, klikněte na tlačítko **projektu...** , rozbalte položku **webové**, klikněte na tlačítko **Dynamic Web Project**a klikněte na tlačítko **Další**.)</span><span class="sxs-lookup"><span data-stu-id="66236-119">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>
2. <span data-ttu-id="66236-120">Pro účely tohoto kurzu, název projektu **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="66236-120">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="66236-121">Na obrazovce se zobrazí podobná této:</span><span class="sxs-lookup"><span data-stu-id="66236-121">Your screen will appear similar to the following:</span></span>
   
    ![Vytvoření nového dynamického webového projektu][02]
3. <span data-ttu-id="66236-123">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="66236-123">Click **Finish**.</span></span>
4. <span data-ttu-id="66236-124">V rámci zobrazení Eclipse na prohlížeči projektu rozbalte **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="66236-124">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="66236-125">Klikněte pravým tlačítkem na **WebContent**, pak na **New** (Nový) a nakonec na **JSP File** (Soubor JSP).</span><span class="sxs-lookup"><span data-stu-id="66236-125">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>
5. <span data-ttu-id="66236-126">V **nový soubor JSP** dialogové okno, název souboru **index.jsp**, nadřazený adresář ponechte na jako **MyWebApp/WebContent**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="66236-126">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>
6. <span data-ttu-id="66236-127">V **vybrat šablonu JSP** dialogové okno, pro účely tohoto kurzu možnost **nový soubor JSP (html)**a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="66236-127">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>
7. <span data-ttu-id="66236-128">Když se soubor index.jsp otevře v prostředí Eclipse, přidejte text dynamicky zobrazíte **Hello, World!**</span><span class="sxs-lookup"><span data-stu-id="66236-128">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="66236-129">do existujícího elementu `<body>`.</span><span class="sxs-lookup"><span data-stu-id="66236-129">within the existing `<body>` element.</span></span> <span data-ttu-id="66236-130">Aktualizovaný `<body>` obsah by měl podobat následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="66236-130">Your updated `<body>` content should resemble the following example:</span></span>
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
8. <span data-ttu-id="66236-131">Uložte index.jsp.</span><span class="sxs-lookup"><span data-stu-id="66236-131">Save index.jsp.</span></span>

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a><span data-ttu-id="66236-132">Chcete-li nasadit aplikaci s kontejnerem Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="66236-132">To deploy your application to an Azure Web App Container</span></span>
<span data-ttu-id="66236-133">Existuje několik způsobů, pomocí kterých můžete nasadit webovou aplikaci Java do Azure.</span><span class="sxs-lookup"><span data-stu-id="66236-133">There are several ways by which you can deploy a Java web application to Azure.</span></span> <span data-ttu-id="66236-134">Tento kurz popisuje jedno z nejjednodušší: vaše aplikace bude nasazena na kontejner Azure Web App – žádné speciální projektu typu ani další nástroje jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="66236-134">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="66236-135">Sadu JDK a webový kontejner software budou poskytovat vám Azure, takže není nutné nahrát vlastní; je třeba je webové aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="66236-135">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="66236-136">Proces publikování pro aplikaci v důsledku toho bude trvat sekund, ne minuty.</span><span class="sxs-lookup"><span data-stu-id="66236-136">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

1. <span data-ttu-id="66236-137">V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="66236-137">In Eclipse's Project Explorer, right-click **MyWebApp**.</span></span>
2. <span data-ttu-id="66236-138">V místní nabídce vyberte **Azure**, pak klikněte na tlačítko **publikovat jako webové aplikace Azure...**</span><span class="sxs-lookup"><span data-stu-id="66236-138">In the context menu, select **Azure**, then click **Publish as Azure Web App...**</span></span>
   
    ![Publikování v Azure Web App][03]
   
    <span data-ttu-id="66236-140">Alternativně projektu webové aplikace v prohlížeči projektu vybráno, kliknutím **publikovat** rozevírací tlačítka na panelu nástrojů a vyberte **publikovat jako webové aplikace Azure** zde:</span><span class="sxs-lookup"><span data-stu-id="66236-140">Alternatively, while your web application project is selected in the Project Explorer, you can click the **Publish** dropdown button on the toolbar and select **Publish as Azure Web App** from there:</span></span>
   
    ![Publikování v Azure Web App][14]
3. <span data-ttu-id="66236-142">Pokud nebyly již přihlášení k Azure z prostředí Eclipse, zobrazí se výzva k přihlásit k účtu Azure:</span><span class="sxs-lookup"><span data-stu-id="66236-142">If you have not already signed into Azure from Eclipse, you will be prompted to sign into your Azure account:</span></span>
   
    ![Azure přihlašovací dialogové okno][04]
   
    <span data-ttu-id="66236-144">Pokud máte více účtů Azure, některé výzvy při přihlašování v procesu se může zobrazit více než jednou, i když se objeví být stejné.</span><span class="sxs-lookup"><span data-stu-id="66236-144">If you have multiple Azure accounts, some of the prompts during the sign in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="66236-145">V takovém případě pokračujte po přihlášení pokyny.</span><span class="sxs-lookup"><span data-stu-id="66236-145">When this happens, continue following the sign in instructions.</span></span>
4. <span data-ttu-id="66236-146">Po úspěšné registraci ke svému účtu Azure **Spravovat odběry** dialogové okno se zobrazí seznam odběry, které jsou spojeny pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="66236-146">After you have successfully signed into your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="66236-147">Pokud jsou uvedena v seznamu více předplatných a chcete pracovat pouze konkrétní podmnožinu je, případně může zrušit zaškrtnutí ty, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="66236-147">If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the ones you do want to use.</span></span> <span data-ttu-id="66236-148">Pokud jste vybrali vašich předplatných, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="66236-148">When you have selected your subscriptions, click **Close**.</span></span>
   
    ![Spravovat předplatná, dialogové okno][05]
5. <span data-ttu-id="66236-150">Když **nasadit do Azure Web App kontejneru** se zobrazí dialogové okno, zobrazí všechny kontejnery webové aplikace, které jste předtím vytvořili; Pokud jste nevytvořili žádné kontejnery, bude seznam prázdný.</span><span class="sxs-lookup"><span data-stu-id="66236-150">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
    ![Nasazení do dialogového okna kontejneru Azure webové aplikace][06]
6. <span data-ttu-id="66236-152">Pokud jste dosud nevytvořili kontejner Azure webové aplikace před, nebo pokud chcete publikovat aplikaci pro nový kontejner, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="66236-152">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="66236-153">Jinak vyberte existujícího webového kontejneru aplikace a přejděte ke kroku 7 níže.</span><span class="sxs-lookup"><span data-stu-id="66236-153">Otherwise, select an existing Web App Container and skip to step 7 below.</span></span>
   
   1. <span data-ttu-id="66236-154">Klikněte na tlačítko **nové...**</span><span class="sxs-lookup"><span data-stu-id="66236-154">Click **New...**</span></span>
      
       ![Nasazení do dialogového okna kontejneru Azure webové aplikace][15]
   2. <span data-ttu-id="66236-156">**Nový kontejner webové aplikace** zobrazí se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="66236-156">The **New Web App Container** dialog box will be displayed:</span></span>
      
       ![Dialogové okno Nový kontejner webové aplikace][07a]
   3. <span data-ttu-id="66236-158">Zadejte **popisek DNS** pro kontejner vaší webové aplikace; to budou formovat popisek DNS listu hostitelské adresy URL pro webovou aplikaci v Azure.</span><span class="sxs-lookup"><span data-stu-id="66236-158">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="66236-159">(Všimněte si, že název musí být k dispozici a v souladu s požadavky na pojmenování webové aplikace Azure.)</span><span class="sxs-lookup"><span data-stu-id="66236-159">(Note that the name must be available and conform to Azure Web App naming requirements.)</span></span>
   4. <span data-ttu-id="66236-160">V **webový kontejner** rozevírací nabídky vyberte příslušný software pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="66236-160">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
       <span data-ttu-id="66236-161">V současné době můžete z Tomcat 8, Tomcat 7 nebo Jetty 9.</span><span class="sxs-lookup"><span data-stu-id="66236-161">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="66236-162">Poslední distribuční vybrané softwaru budou poskytovat Azure a bude spuštěna v posledních distribučním JDK 8 vytvořené Oracle a poskytovaný platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="66236-162">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>
   5. <span data-ttu-id="66236-163">V **předplatné** rozevírací nabídky vyberte předplatné, kterou chcete použít pro toto nasazení.</span><span class="sxs-lookup"><span data-stu-id="66236-163">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>
   6. <span data-ttu-id="66236-164">V **skupiny prostředků** rozevírací nabídky vyberte skupinu prostředků, pro který chcete přidružit webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="66236-164">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="66236-165">(Skupiny prostředků azure umožňují seskupit související prostředky tak, aby například, můžete je odstranit společně.)</span><span class="sxs-lookup"><span data-stu-id="66236-165">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
       <span data-ttu-id="66236-166">Můžete vyberte existující skupinu prostředků (pokud nějaké máte) a přeskočit na krok g níže nebo použít tyto kroky k vytvoření nové skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="66236-166">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following these steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="66236-167">Klikněte na tlačítko **nové...**</span><span class="sxs-lookup"><span data-stu-id="66236-167">Click **New...**</span></span>
      * <span data-ttu-id="66236-168">**Novou skupinu prostředků** zobrazí se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="66236-168">The **New Resource Group** dialog box will be displayed:</span></span>
        
          ![Dialogové okno nové skupiny prostředků][08]
      * <span data-ttu-id="66236-170">V **název** textovému poli, zadejte název nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="66236-170">In the the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="66236-171">V **oblast** rozevírací nabídky vyberte odpovídající Azure datového centra umístění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="66236-171">In the the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="66236-172">Volitelné: Ve výchozím nastavení, poslední distribuce Java 8 bude nasazeno Azure automaticky do vaší webové aplikace kontejneru jako vaše prostředí Java Virtual Machine.</span><span class="sxs-lookup"><span data-stu-id="66236-172">OPTIONAL: By default, a recent distribution of Java 8 will be deployed by Azure automatically to your web app container as your JVM.</span></span> <span data-ttu-id="66236-173">Můžete však zadejte jinou verzi a distribuci systém JVM, pokud to vyžaduje vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="66236-173">However, you can specify a different version and distribution of the JVM if your Web App requires it.</span></span> <span data-ttu-id="66236-174">Chcete-li zadat sadu JDK pro vaši webovou aplikaci, klikněte na tlačítko **JDK** a vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="66236-174">To specify the JDK for your Web App, click the **JDK** tab, and select one of the following options:</span></span>
        
        * <span data-ttu-id="66236-175">**Nasadit výchozí JDK nabízené službou Azure Web Apps**: Tato možnost nasadí poslední distribuce Java 8.</span><span class="sxs-lookup"><span data-stu-id="66236-175">**Deploy the default JDK offered by Azure Web Apps service**: This option will deploy a recent distribution of Java 8.</span></span>
        * <span data-ttu-id="66236-176">**Nasazení 3. stran JDK dostupné v Azure**: Tato možnost vám umožňuje vybrat ze seznamu JDKs, které jsou poskytovány Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="66236-176">**Deploy a 3rd party JDK available on Azure**: This option allows you to choose from the list of JDKs which are provided by Microsoft Azure.</span></span>
        * <span data-ttu-id="66236-177">**Nasazení vlastní JDK z tohoto umístění stahování**: Tato možnost umožňuje zadat vlastní JDK distribuce, které musí být jako soubor ZIP zabalené a nahrané do umístění veřejně dostupné stahování nebo účet úložiště Azure, pro které máte přístup.</span><span class="sxs-lookup"><span data-stu-id="66236-177">**Deploy my own JDK from this download location**: This option allows you to specify your own JDK distribution, which must be packaged as a ZIP file and uploaded to either a publicly available download location or an Azure storage account for which you have access.</span></span>
          
          ![Dialogové okno Nový kontejner webové aplikace][07b]
   7. <span data-ttu-id="66236-179">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="66236-179">Click **OK**.</span></span>
   8. <span data-ttu-id="66236-180">**Plán služby App Service** rozevírací nabídce uvádí plány služby aplikace, které jsou spojeny se skupinami prostředků, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="66236-180">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="66236-181">(Informace o umístění vaší webové aplikace, cenové úrovně a velikost instance výpočetní zadejte plány služby app Service.</span><span class="sxs-lookup"><span data-stu-id="66236-181">(App Service Plans specify information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="66236-182">Jednoho plánu služby App Service můžete použít pro více webových aplikací, které je důvod, proč je zachován odděleně od konkrétní nasazení webové aplikace.)</span><span class="sxs-lookup"><span data-stu-id="66236-182">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       <span data-ttu-id="66236-183">Můžete vyberte existující plán služby App Service (pokud nějaké máte) a přeskočit na krok h níže nebo použít tyto kroky k vytvoření nový plán služby App Service:</span><span class="sxs-lookup"><span data-stu-id="66236-183">You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following these steps to create a new App Service Plan:</span></span>
      
      * <span data-ttu-id="66236-184">Klikněte na tlačítko **nové...**</span><span class="sxs-lookup"><span data-stu-id="66236-184">Click **New...**</span></span>
      * <span data-ttu-id="66236-185">**Nový plán služby App Service** zobrazí se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="66236-185">The **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Nový plán služby App Service dialogové okno][09]
      * <span data-ttu-id="66236-187">V **název** textovému poli, zadejte název pro váš nový plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="66236-187">In the the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="66236-188">V **umístění** rozevírací nabídky vyberte odpovídající Azure datového centra umístění pro plán.</span><span class="sxs-lookup"><span data-stu-id="66236-188">In the the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="66236-189">V **cenová úroveň** rozevírací nabídky vyberte příslušné ceny pro plán.</span><span class="sxs-lookup"><span data-stu-id="66236-189">In the the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="66236-190">Při testování můžete **volné**.</span><span class="sxs-lookup"><span data-stu-id="66236-190">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="66236-191">V **velikost Instance** rozevírací nabídky, vyberte příslušnou instanci velikost plánu.</span><span class="sxs-lookup"><span data-stu-id="66236-191">In the the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="66236-192">Při testování můžete **malé**.</span><span class="sxs-lookup"><span data-stu-id="66236-192">For testing purposes you can choose **Small**.</span></span>
   9. <span data-ttu-id="66236-193">Po dokončení všech výše uvedených kroků, dialogové okno Nový kontejner webové aplikace by měl vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="66236-193">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
       ![Dialogové okno Nový kontejner webové aplikace][10]
   10. <span data-ttu-id="66236-195">Klikněte na tlačítko **OK** vytvoření kontejneru vaší nové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="66236-195">Click **OK** to complete the creation of your new Web App container.</span></span>
       
        <span data-ttu-id="66236-196">Počkejte několik sekund seznam kontejnery webové aplikace nutné je aktualizovat a nově vytvořený webový kontejner aplikace měla by být vybrána nyní v seznamu.</span><span class="sxs-lookup"><span data-stu-id="66236-196">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>
7. <span data-ttu-id="66236-197">Nyní jste připraveni k dokončení počáteční nasazení vaší webové aplikace do Azure:</span><span class="sxs-lookup"><span data-stu-id="66236-197">You are now ready to complete the initial deployment of your Web App to Azure:</span></span>
   
    ![Nasazení do dialogového okna kontejneru Azure webové aplikace][11]
   
    <span data-ttu-id="66236-199">Klikněte na tlačítko **OK** k nasazení aplikace v jazyce Java do vybraného kontejneru webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="66236-199">Click **OK** to deploy your Java application to the selected Web App container.</span></span>
   
    <span data-ttu-id="66236-200">Ve výchozím nastavení budou aplikace nasazeny jako podadresáři aplikačního serveru.</span><span class="sxs-lookup"><span data-stu-id="66236-200">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="66236-201">Pokud chcete, aby ho nasadit jako kořenová aplikace, podívejte se **nasadit do kořenové** políčko před kliknutím na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="66236-201">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>
8. <span data-ttu-id="66236-202">V dalším kroku byste měli vidět **protokol činnosti Azure** zobrazení, které se označují stav nasazení vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="66236-202">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
    ![Protokol činnosti Azure][12]
   
    <span data-ttu-id="66236-204">Proces nasazení webové aplikace do Azure by měl trvat jenom pár sekund.</span><span class="sxs-lookup"><span data-stu-id="66236-204">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="66236-205">Pokud vaše aplikace připravené, zobrazí se odkaz s názvem **publikováno** v **stav** sloupce.</span><span class="sxs-lookup"><span data-stu-id="66236-205">When your application ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="66236-206">Po kliknutí na odkaz, jeho přejdete na domovskou stránku vaší nasazené webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="66236-206">When you click the link, it will take you to your deployed Web App's home page.</span></span>

## <a name="updating-your-web-app"></a><span data-ttu-id="66236-207">Aktualizace webové aplikace</span><span class="sxs-lookup"><span data-stu-id="66236-207">Updating your web app</span></span>
<span data-ttu-id="66236-208">Aktualizace stávající spuštění webové aplikace Azure je rychlý a snadný proces a máte dvě možnosti pro aktualizaci:</span><span class="sxs-lookup"><span data-stu-id="66236-208">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="66236-209">Můžete aktualizovat nasazení stávající webovou aplikaci Java.</span><span class="sxs-lookup"><span data-stu-id="66236-209">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="66236-210">Můžete publikovat aplikaci Java další ke stejnému kontejneru webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="66236-210">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="66236-211">V obou případech proces je stejný jako a trvá jenom pár sekund:</span><span class="sxs-lookup"><span data-stu-id="66236-211">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="66236-212">V prohlížeči projektu Eclipse klikněte pravým tlačítkem na aplikaci Java, které chcete aktualizovat nebo přidat do existujícího webového kontejneru aplikace.</span><span class="sxs-lookup"><span data-stu-id="66236-212">In the Eclipse project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>
2. <span data-ttu-id="66236-213">Po zobrazení v místní nabídce vyberte **Azure** a potom **publikovat jako webové aplikace Azure...**</span><span class="sxs-lookup"><span data-stu-id="66236-213">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="66236-214">Vzhledem k tomu, že jste již přihlášení dříve, zobrazí se seznam kontejnerů existující webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="66236-214">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="66236-215">Vyberte ten, který chcete publikovat nebo znovu publikovat aplikace Java a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="66236-215">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="66236-216">Později, několik sekund **protokol činnosti Azure** zobrazení se zobrazí aktualizovaná nasazení jako **publikováno** a vy nebudete moci ověřit aktualizované aplikace ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="66236-216">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="66236-217">Spuštění, zastavení nebo restartování existující webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="66236-217">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="66236-218">Spuštění nebo zastavení existující webové aplikace Azure kontejneru (včetně všech nasazených aplikací Java v ní), můžete použít **Azure Explorer** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="66236-218">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="66236-219">Pokud **Průzkumník Azure** zobrazení ještě není otevřené, který můžete otevřít kliknutím na tlačítko pak **okno** nabídky v prostředí Eclipse klikněte **zobrazit zobrazení**, pak **jiné...** , pak **Azure**a potom klikněte na **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="66236-219">If the **Azure Explorer** view is not already open, you can open it by clicking then **Window** menu in Eclipse, then click **Show View**, then **Other...**, then **Azure**, and then click **Azure Explorer**.</span></span> <span data-ttu-id="66236-220">Pokud jste nepřihlásili dříve, vyzve vás k tomu.</span><span class="sxs-lookup"><span data-stu-id="66236-220">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="66236-221">Když **Azure Explorer** zobrazení, pomocí následujícího postupu spuštění nebo zastavení webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="66236-221">When the **Azure Explorer** view is displayed, use follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="66236-222">Rozbalte **Azure** uzlu.</span><span class="sxs-lookup"><span data-stu-id="66236-222">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="66236-223">Rozbalte **webové aplikace** uzlu.</span><span class="sxs-lookup"><span data-stu-id="66236-223">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="66236-224">Klikněte pravým tlačítkem na požadovanou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="66236-224">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="66236-225">Jakmile se zobrazí místní nabídky, klikněte na tlačítko **spustit**, **Zastavit**, nebo **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="66236-225">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="66236-226">Všimněte si, že možnosti nabídky kontextově, tak můžete zastavit funkční webovou aplikaci nebo pouze spustí webovou aplikaci, která není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="66236-226">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
    ![Zastavení stávající webovou aplikaci][13]

## <a name="next-steps"></a><span data-ttu-id="66236-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66236-228">Next Steps</span></span>
<span data-ttu-id="66236-229">Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="66236-229">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="66236-230">[nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="66236-230">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="66236-231">[instalaci sady nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="66236-231">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="66236-232">*Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse (v tomto článku)*</span><span class="sxs-lookup"><span data-stu-id="66236-232">*Create a Hello World Web App for Azure in Eclipse (This Article)*</span></span>
  * <span data-ttu-id="66236-233">[Novinky v sadě Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="66236-233">[What's New in the Azure Toolkit for Eclipse]</span></span>
* <span data-ttu-id="66236-234">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="66236-234">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="66236-235">[Instalace sady Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="66236-235">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="66236-236">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="66236-236">[Create a Hello World Web App for Azure in IntelliJ]</span></span>
  * <span data-ttu-id="66236-237">[Novinky v sadě Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="66236-237">[What's New in the Azure Toolkit for IntelliJ]</span></span>

<span data-ttu-id="66236-238">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].</span><span class="sxs-lookup"><span data-stu-id="66236-238">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<span data-ttu-id="66236-239">Další informace o vytváření webové aplikace Azure najdete v tématu [webových aplikací – přehled].</span><span class="sxs-lookup"><span data-stu-id="66236-239">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

<span data-ttu-id="66236-240">[nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="66236-240">[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="66236-241">[Sada Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="66236-241">[Azure Toolkit for IntelliJ]: ../azure-toolkit-for-intellij.md</span></span>
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
<span data-ttu-id="66236-242">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="66236-242">[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="66236-243">[instalaci sady nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="66236-243">[Installing the Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="66236-244">[Instalace sady Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="66236-244">[Installing the Azure Toolkit for IntelliJ]: ../azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="66236-245">[Novinky v sadě Azure Toolkit pro Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="66236-245">[What's New in the Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="66236-246">[Novinky v sadě Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="66236-246">[What's New in the Azure Toolkit for IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="66236-247">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="66236-247">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="66236-248">[webových aplikací – přehled]: ./app-service-web-overview.md</span><span class="sxs-lookup"><span data-stu-id="66236-248">[Web Apps Overview]: ./app-service-web-overview.md</span></span>
<span data-ttu-id="66236-249">[Apache Tomcat]: http://tomcat.apache.org/</span><span class="sxs-lookup"><span data-stu-id="66236-249">[Apache Tomcat]: http://tomcat.apache.org/</span></span>
<span data-ttu-id="66236-250">[Jetty]: http://www.eclipse.org/jetty/</span><span class="sxs-lookup"><span data-stu-id="66236-250">[Jetty]: http://www.eclipse.org/jetty/</span></span>

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
