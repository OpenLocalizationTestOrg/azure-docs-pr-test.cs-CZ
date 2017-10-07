---
title: "základní Azure webovou aplikaci pomocí aaaCreate Eclipse | Microsoft Docs"
description: Tento kurz ukazuje, jak toouse hello Azure Toolkit pro Eclipse toocreate Hello World webovou aplikaci pro Azure.
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
ms.openlocfilehash: b2f42e0e7a5b98760ec02fab2fc38f9f07b1156b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a><span data-ttu-id="e9bbf-103">Vytvořte základní Azure webovou aplikaci pomocí Eclipse</span><span class="sxs-lookup"><span data-stu-id="e9bbf-103">Create a basic Azure web app using Eclipse</span></span>
<span data-ttu-id="e9bbf-104">Tento kurz ukazuje, jak toocreate a nasazení základní tooAzure aplikace Hello, World jako webovou aplikaci pomocí hello [nástrojů Azure pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="e9bbf-104">This tutorial shows how toocreate and deploy a basic Hello World application tooAzure as a Web App by using hello [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="e9bbf-105">Základní příklad JSP je uvedený na jednoduchost, ale podobným způsobem může být vhodné pro Java servlet, co se týče nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="e9bbf-106">Po dokončení tohoto kurzu, vaše aplikace bude vypadat podobně jako toohello následující ilustrace, při zobrazení ve webovém prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-106">When you have completed this tutorial, your application will look similar toohello following illustration when you view it in a web browser:</span></span>

![Verze Preview Hello World aplikace][01]

## <a name="prerequisites"></a><span data-ttu-id="e9bbf-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e9bbf-108">Prerequisites</span></span>
* <span data-ttu-id="e9bbf-109">Java Developer Kit (JDK), v 1.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-109">A Java Developer Kit (JDK), v 1.8 or later.</span></span>
* <span data-ttu-id="e9bbf-110">Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE, vzhled Luna nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-110">Eclipse IDE for Java EE Developers, Luna or later.</span></span> <span data-ttu-id="e9bbf-111">To si můžete stáhnout z <http://www.eclipse.org/downloads/>.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-111">This can be downloaded from <http://www.eclipse.org/downloads/>.</span></span>
* <span data-ttu-id="e9bbf-112">Distribuční založené na jazyce Java webový server nebo aplikačního serveru jako například [Apache Tomcat] nebo [Jetty].</span><span class="sxs-lookup"><span data-stu-id="e9bbf-112">A distribution of a Java-based web server or application server, such as [Apache Tomcat] or [Jetty].</span></span>
* <span data-ttu-id="e9bbf-113">Předplatné Azure, který můžete získat z <https://azure.microsoft.com/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-113">An Azure subscription, which can be acquired from <https://azure.microsoft.com/free/> or <http://azure.microsoft.com/pricing/purchase-options/>.</span></span>
* <span data-ttu-id="e9bbf-114">Hello [nástrojů Azure pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="e9bbf-114">hello [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="e9bbf-115">Informace o instalaci hello nástrojů Azure najdete v tématu [hello instalace nástrojů Azure pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="e9bbf-115">For information about installing hello Azure Toolkit, see [Installing hello Azure Toolkit for Eclipse].</span></span>

## <a name="toocreate-a-hello-world-application"></a><span data-ttu-id="e9bbf-116">toocreate aplikace Hello World</span><span class="sxs-lookup"><span data-stu-id="e9bbf-116">toocreate a Hello World application</span></span>
<span data-ttu-id="e9bbf-117">Nejdříve začneme s vytvořením projektu Java.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-117">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="e9bbf-118">Spusťte Eclipse a v nabídce hello na **soubor**, klikněte na tlačítko **nový**a potom klikněte na **Dynamic Web Project**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-118">Start Eclipse, and at hello menu click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="e9bbf-119">(Pokud nevidíte **Dynamic Web Project** uvedené jako dostupné projekt po kliknutí na **soubor** a **nový**, pak hello následující: klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**, klikněte na tlačítko **projektu...** , rozbalte položku **webové**, klikněte na tlačítko **Dynamic Web Project**a klikněte na tlačítko **Další**.)</span><span class="sxs-lookup"><span data-stu-id="e9bbf-119">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do hello following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>
2. <span data-ttu-id="e9bbf-120">Pro účely tohoto kurzu, pojmenujte projekt hello **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-120">For purposes of this tutorial, name hello project **MyWebApp**.</span></span> <span data-ttu-id="e9bbf-121">Na obrazovce zobrazí podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-121">Your screen will appear similar toohello following:</span></span>
   
    ![Vytvoření nového dynamického webového projektu][02]
3. <span data-ttu-id="e9bbf-123">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-123">Click **Finish**.</span></span>
4. <span data-ttu-id="e9bbf-124">V rámci zobrazení Eclipse na prohlížeči projektu rozbalte **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-124">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="e9bbf-125">Klikněte pravým tlačítkem na **WebContent**, pak na **New** (Nový) a nakonec na **JSP File** (Soubor JSP).</span><span class="sxs-lookup"><span data-stu-id="e9bbf-125">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>
5. <span data-ttu-id="e9bbf-126">V hello **nový soubor JSP** dialogové okno, název souboru hello **index.jsp**, zachovat hello nadřazené složky jako **MyWebApp/WebContent**a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-126">In hello **New JSP File** dialog box, name hello file **index.jsp**, keep hello parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>
6. <span data-ttu-id="e9bbf-127">V hello **vybrat šablonu JSP** dialogové okno, pro účely tohoto kurzu možnost **nový soubor JSP (html)**a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-127">In hello **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>
7. <span data-ttu-id="e9bbf-128">Když se soubor index.jsp otevře v prostředí Eclipse, přidejte v zobrazení textu toodynamically **Hello, World!**</span><span class="sxs-lookup"><span data-stu-id="e9bbf-128">When your index.jsp file opens in Eclipse, add in text toodynamically display **Hello World!**</span></span> <span data-ttu-id="e9bbf-129">v rámci existující hello `<body>` elementu.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-129">within hello existing `<body>` element.</span></span> <span data-ttu-id="e9bbf-130">Aktualizovaný `<body>` obsah by měla vypadat přibližně hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-130">Your updated `<body>` content should resemble hello following example:</span></span>
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
8. <span data-ttu-id="e9bbf-131">Uložte index.jsp.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-131">Save index.jsp.</span></span>

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a><span data-ttu-id="e9bbf-132">toodeploy tooan vaše aplikace Azure Web App kontejneru</span><span class="sxs-lookup"><span data-stu-id="e9bbf-132">toodeploy your application tooan Azure Web App Container</span></span>
<span data-ttu-id="e9bbf-133">Existuje několik způsobů, pomocí kterých můžete nasadit tooAzure webové aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-133">There are several ways by which you can deploy a Java web application tooAzure.</span></span> <span data-ttu-id="e9bbf-134">Tento kurz popisuje jedno z nejjednodušší hello: bude vaše aplikace nasazené tooan kontejneru Azure Web App – žádné speciální projektu typu ani další nástroje jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-134">This tutorial describes one of hello simplest: your application will be deployed tooan Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="e9bbf-135">Hello JDK a hello webový kontejner software budou poskytovat vám Azure, takže není bez nutnosti tooupload vlastní; je třeba je webové aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-135">hello JDK and hello web container software will be provided for you by Azure, so there is no need tooupload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="e9bbf-136">V důsledku toho hello publikování pro vaše aplikace bude trvat sekund, ne minuty.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-136">As a result, hello publishing process for your application will take seconds, not minutes.</span></span>

1. <span data-ttu-id="e9bbf-137">V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-137">In Eclipse's Project Explorer, right-click **MyWebApp**.</span></span>
2. <span data-ttu-id="e9bbf-138">V místní nabídce hello vyberte **Azure**, pak klikněte na tlačítko **publikovat jako webové aplikace Azure...**</span><span class="sxs-lookup"><span data-stu-id="e9bbf-138">In hello context menu, select **Azure**, then click **Publish as Azure Web App...**</span></span>
   
    ![Publikování v Azure Web App][03]
   
    <span data-ttu-id="e9bbf-140">Můžete taky v hello prohlížeči projektu vybráno projektu webové aplikace, můžete kliknout na hello **publikovat** rozevírací tlačítka na panelu nástrojů hello a vyberte **publikovat jako webové aplikace Azure** zde:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-140">Alternatively, while your web application project is selected in hello Project Explorer, you can click hello **Publish** dropdown button on hello toolbar and select **Publish as Azure Web App** from there:</span></span>
   
    ![Publikování v Azure Web App][14]
3. <span data-ttu-id="e9bbf-142">Pokud nebyly již přihlášení k Azure z prostředí Eclipse, bude výzvami toosign ke svému účtu Azure:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-142">If you have not already signed into Azure from Eclipse, you will be prompted toosign into your Azure account:</span></span>
   
    ![Azure přihlašovací dialogové okno][04]
   
    <span data-ttu-id="e9bbf-144">Pokud máte několik účtů Azure, přihlaste se některé výzvy hello během hello proces se může zobrazit více než jednou, i když se objeví toobe hello stejné.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-144">If you have multiple Azure accounts, some of hello prompts during hello sign in process may be shown more than once, even if they appear toobe hello same.</span></span> <span data-ttu-id="e9bbf-145">V takovém případě pokračujte, následující hello přihlášení pokyny.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-145">When this happens, continue following hello sign in instructions.</span></span>
4. <span data-ttu-id="e9bbf-146">Po úspěšné registraci ke svému účtu Azure, hello **Spravovat odběry** dialogové okno se zobrazí seznam odběry, které jsou spojeny pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-146">After you have successfully signed into your Azure account, hello **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="e9bbf-147">Pokud existuje více odběry v seznamu a chcete toowork se pouze konkrétní podmnožinu je, případně může zrušit zaškrtnutí hello ty, které potřebujete toouse.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-147">If there are multiple subscriptions listed and you want toowork with only a specific subset of them, you may optionally uncheck hello ones you do want toouse.</span></span> <span data-ttu-id="e9bbf-148">Pokud jste vybrali vašich předplatných, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-148">When you have selected your subscriptions, click **Close**.</span></span>
   
    ![Spravovat předplatná, dialogové okno][05]
5. <span data-ttu-id="e9bbf-150">Když hello **nasazení tooAzure webové aplikace kontejneru** se zobrazí dialogové okno, zobrazí všechny kontejnery webové aplikace, které jste předtím vytvořili; Pokud jste nevytvořili žádné kontejnery, bude hello seznam prázdný.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-150">When hello **Deploy tooAzure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, hello list will be empty.</span></span>
   
    ![Nasazení webové aplikace kontejneru tooAzure – dialogové okno][06]
6. <span data-ttu-id="e9bbf-152">Pokud jste dosud nevytvořili kontejner Azure webové aplikace před, nebo pokud chcete toopublish vaší aplikace tooa nový kontejner, použijte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-152">If you have not created an Azure Web App Container before, or if you would like toopublish your application tooa new container, use hello following steps.</span></span> <span data-ttu-id="e9bbf-153">Jinak vyberte existujícího webového kontejneru aplikace a přeskočit toostep 7 níže.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-153">Otherwise, select an existing Web App Container and skip toostep 7 below.</span></span>
   
   1. <span data-ttu-id="e9bbf-154">Klikněte na tlačítko **nové...**</span><span class="sxs-lookup"><span data-stu-id="e9bbf-154">Click **New...**</span></span>
      
       ![Nasazení webové aplikace kontejneru tooAzure – dialogové okno][15]
   2. <span data-ttu-id="e9bbf-156">Hello **nový kontejner webové aplikace** zobrazí se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-156">hello **New Web App Container** dialog box will be displayed:</span></span>
      
       ![Dialogové okno Nový kontejner webové aplikace][07a]
   3. <span data-ttu-id="e9bbf-158">Zadejte **popisek DNS** pro kontejner vaší webové aplikace; to budou formovat hello listu DNS popisek hello hostitelské adresy URL pro webovou aplikaci v Azure.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-158">Enter a **DNS Label** for your Web App Container; this will form hello leaf DNS label of hello host URL for your web application in Azure.</span></span> <span data-ttu-id="e9bbf-159">(Všimněte si, že hello název musí být k dispozici a v souladu s požadavky pojmenování webové aplikace tooAzure.)</span><span class="sxs-lookup"><span data-stu-id="e9bbf-159">(Note that hello name must be available and conform tooAzure Web App naming requirements.)</span></span>
   4. <span data-ttu-id="e9bbf-160">V hello **webový kontejner** rozevírací nabídky, vyberte hello příslušný software pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-160">In hello **Web Container** drop-down menu, select hello appropriate software for your application.</span></span>
      
       <span data-ttu-id="e9bbf-161">V současné době můžete z Tomcat 8, Tomcat 7 nebo Jetty 9.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-161">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="e9bbf-162">Poslední distribuce softwaru hello vybrané budou poskytovat Azure a bude spuštěna v posledních distribučním JDK 8 vytvořené Oracle a poskytovaný platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-162">A recent distribution of hello selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>
   5. <span data-ttu-id="e9bbf-163">V hello **předplatné** rozevírací nabídky, vyberte hello předplatné chcete toouse pro toto nasazení.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-163">In hello **Subscription** drop-down menu, select hello subscription you want toouse for this deployment.</span></span>
   6. <span data-ttu-id="e9bbf-164">V hello **skupiny prostředků** rozevírací nabídky vyberte hello skupinu prostředků pro který chcete tooassociate vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-164">In hello **Resource Group** drop-down menu, select hello Resource Group with which you want tooassociate your Web App.</span></span> <span data-ttu-id="e9bbf-165">(Skupiny prostředků azure povolit, že jste toogroup související prostředky společně, aby, například, můžete je odstranit společně.)</span><span class="sxs-lookup"><span data-stu-id="e9bbf-165">(Azure Resource Groups allow you toogroup related resources together so that, for example, they can be deleted together.)</span></span>
      
       <span data-ttu-id="e9bbf-166">Můžete vybrat existující skupinu prostředků (pokud nějaké máte) a přeskočit toostep g níže nebo hello použijte následující tyto kroky toocreate novou skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-166">You can select an existing Resource Group (if you have any) and skip toostep g below, or use hello following these steps toocreate a new Resource Group:</span></span>
      
      * <span data-ttu-id="e9bbf-167">Klikněte na tlačítko **nové...**</span><span class="sxs-lookup"><span data-stu-id="e9bbf-167">Click **New...**</span></span>
      * <span data-ttu-id="e9bbf-168">Hello **novou skupinu prostředků** zobrazí se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-168">hello **New Resource Group** dialog box will be displayed:</span></span>
        
          ![Dialogové okno nové skupiny prostředků][08]
      * <span data-ttu-id="e9bbf-170">V hello hello **název** textovému poli, zadejte název nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-170">In hello hello **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="e9bbf-171">V hello hello **oblast** rozevírací nabídky vyberte hello odpovídající Azure datového centra umístění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-171">In hello hello **Region** drop-down menu, select hello appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="e9bbf-172">Volitelné: Ve výchozím nastavení, poslední distribuce Java 8 bude nasazeno Azure automaticky tooyour webové aplikace kontejneru jako vaše prostředí Java Virtual Machine.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-172">OPTIONAL: By default, a recent distribution of Java 8 will be deployed by Azure automatically tooyour web app container as your JVM.</span></span> <span data-ttu-id="e9bbf-173">Můžete však zadejte jinou verzi a distribuci hello prostředí Java Virtual Machine, pokud to vyžaduje vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-173">However, you can specify a different version and distribution of hello JVM if your Web App requires it.</span></span> <span data-ttu-id="e9bbf-174">toospecify hello JDK pro webovou aplikaci, klikněte na tlačítko hello **JDK** a vyberte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-174">toospecify hello JDK for your Web App, click hello **JDK** tab, and select one of hello following options:</span></span>
        
        * <span data-ttu-id="e9bbf-175">**Nasadit výchozí hello JDK nabízené službou Azure Web Apps**: Tato možnost nasadí poslední distribuce Java 8.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-175">**Deploy hello default JDK offered by Azure Web Apps service**: This option will deploy a recent distribution of Java 8.</span></span>
        * <span data-ttu-id="e9bbf-176">**Nasazení 3. stran JDK dostupné v Azure**: Tato možnost vám umožňuje toochoose ze seznamu hello JDKs, které jsou poskytovány Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-176">**Deploy a 3rd party JDK available on Azure**: This option allows you toochoose from hello list of JDKs which are provided by Microsoft Azure.</span></span>
        * <span data-ttu-id="e9bbf-177">**Nasazení vlastní JDK z tohoto umístění stahování**: Tato možnost vám umožňuje toospecify vlastní JDK distribuci, která musí být zabalené jako soubor ZIP a nahrát tooeither umístění veřejně dostupné stahování nebo úložiště Azure účet, pro kterou je máte přístup.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-177">**Deploy my own JDK from this download location**: This option allows you toospecify your own JDK distribution, which must be packaged as a ZIP file and uploaded tooeither a publicly available download location or an Azure storage account for which you have access.</span></span>
          
          ![Dialogové okno Nový kontejner webové aplikace][07b]
   7. <span data-ttu-id="e9bbf-179">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-179">Click **OK**.</span></span>
   8. <span data-ttu-id="e9bbf-180">Hello **plán služby App Service** rozevírací nabídce uvádí plány hello aplikace služby, které jsou přidruženy hello skupinu prostředků, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-180">hello **App Service Plan** drop-down menu lists hello app service plans that are associated with hello Resource Group that you selected.</span></span> <span data-ttu-id="e9bbf-181">(Plán služby app Service zadejte informace, jako je hello umístění webové aplikace hello cenová úroveň a velikost instance výpočetní hello.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-181">(App Service Plans specify information such as hello location of your Web App, hello pricing tier and hello compute instance size.</span></span> <span data-ttu-id="e9bbf-182">Jednoho plánu služby App Service můžete použít pro více webových aplikací, které je důvod, proč je zachován odděleně od konkrétní nasazení webové aplikace.)</span><span class="sxs-lookup"><span data-stu-id="e9bbf-182">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       <span data-ttu-id="e9bbf-183">Můžete vybrat existující plán služby App Service (pokud nějaké máte) a přeskočit toostep h níže, nebo použijte hello následující toocreate tyto kroky nový plán služby App Service:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-183">You can select an existing App Service Plan (if you have any) and skip toostep h below, or use hello following these steps toocreate a new App Service Plan:</span></span>
      
      * <span data-ttu-id="e9bbf-184">Klikněte na tlačítko **nové...**</span><span class="sxs-lookup"><span data-stu-id="e9bbf-184">Click **New...**</span></span>
      * <span data-ttu-id="e9bbf-185">Hello **nový plán služby App Service** zobrazí se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-185">hello **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Nový plán služby App Service dialogové okno][09]
      * <span data-ttu-id="e9bbf-187">V hello hello **název** textovému poli, zadejte název pro váš nový plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-187">In hello hello **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="e9bbf-188">V hello hello **umístění** rozevírací nabídky vyberte hello odpovídající Azure datového centra umístění pro plán hello.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-188">In hello hello **Location** drop-down menu, select hello appropriate Azure data center location for hello plan.</span></span>
      * <span data-ttu-id="e9bbf-189">V hello hello **cenová úroveň** rozevírací nabídky vyberte hello příslušné ceny pro plán hello.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-189">In hello hello **Pricing Tier** drop-down menu, select hello appropriate pricing for hello plan.</span></span> <span data-ttu-id="e9bbf-190">Při testování můžete **volné**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-190">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="e9bbf-191">V hello hello **velikost Instance** rozevírací nabídky, vyberte hello velikost odpovídající instance hello plánu.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-191">In hello hello **Instance Size** drop-down menu, select hello appropriate instance size for hello plan.</span></span> <span data-ttu-id="e9bbf-192">Při testování můžete **malé**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-192">For testing purposes you can choose **Small**.</span></span>
   9. <span data-ttu-id="e9bbf-193">Po dokončení všech hello výše uvedených kroků, by měl vypadat dialogové okno Nový kontejner webové aplikace hello hello následující obrázek:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-193">Once you have completed all of hello above steps, hello New Web App Container dialog box should resemble hello following illustration:</span></span>
      
       ![Dialogové okno Nový kontejner webové aplikace][10]
   10. <span data-ttu-id="e9bbf-195">Klikněte na tlačítko **OK** toocomplete hello vytvoření kontejneru vaší nové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-195">Click **OK** toocomplete hello creation of your new Web App container.</span></span>
       
        <span data-ttu-id="e9bbf-196">Počkejte několik sekund hello seznam toobe kontejnery webové aplikace hello aktualizovat a nově vytvořený webový kontejner aplikace měla by být vybrána nyní v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-196">Wait a few seconds for hello list of hello Web App containers toobe refreshed, and your newly-created web app container should now be selected in hello list.</span></span>
7. <span data-ttu-id="e9bbf-197">Jste nyní připraven toocomplete hello počátečního nasazení tooAzure vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-197">You are now ready toocomplete hello initial deployment of your Web App tooAzure:</span></span>
   
    ![Nasazení webové aplikace kontejneru tooAzure – dialogové okno][11]
   
    <span data-ttu-id="e9bbf-199">Klikněte na tlačítko **OK** toodeploy vaše toohello aplikace Java vybraný kontejner webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-199">Click **OK** toodeploy your Java application toohello selected Web App container.</span></span>
   
    <span data-ttu-id="e9bbf-200">Ve výchozím nastavení budou aplikace nasazeny jako podadresáři hello aplikačního serveru.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-200">By default, your application will be deployed as a subdirectory of hello application server.</span></span> <span data-ttu-id="e9bbf-201">Pokud chcete ho nasadit jako kořenová aplikace hello toobe, zkontrolujte hello **nasazení tooroot** políčko před kliknutím na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-201">If you want it toobe deployed as hello root application, check hello **Deploy tooroot** checkbox before clicking **OK**.</span></span>
8. <span data-ttu-id="e9bbf-202">V dalším kroku byste měli vidět hello **protokol činnosti Azure** zobrazení, která bude znamenat hello stav nasazení vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-202">Next, you should see hello **Azure Activity Log** view, which will indicate hello deployment status of your Web App.</span></span>
   
    ![Protokol činnosti Azure][12]
   
    <span data-ttu-id="e9bbf-204">Hello proces nasazení vaší webové aplikace tooAzure měli jenom pár sekund trvat toocomplete.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-204">hello process of deploying your Web App tooAzure should take only a few seconds toocomplete.</span></span> <span data-ttu-id="e9bbf-205">Pokud vaše aplikace připravené, zobrazí se odkaz s názvem **publikováno** v hello **stav** sloupce.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-205">When your application ready, you will see a link named **Published** in hello **Status** column.</span></span> <span data-ttu-id="e9bbf-206">Po kliknutí na odkaz hello, bude vám trvat domovskou stránku tooyour nasazené webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-206">When you click hello link, it will take you tooyour deployed Web App's home page.</span></span>

## <a name="updating-your-web-app"></a><span data-ttu-id="e9bbf-207">Aktualizace webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e9bbf-207">Updating your web app</span></span>
<span data-ttu-id="e9bbf-208">Aktualizace stávající spuštění webové aplikace Azure je rychlý a snadný proces a máte dvě možnosti pro aktualizaci:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-208">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="e9bbf-209">Můžete aktualizovat hello nasazení stávající webovou aplikaci Java.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-209">You can update hello deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="e9bbf-210">Můžete publikovat další aplikaci toohello Java stejnému kontejneru webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-210">You can publish an additional Java application toohello same Web App Container.</span></span>

<span data-ttu-id="e9bbf-211">V obou případech hello proces je stejný jako a trvá jenom pár sekund:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-211">In either case, hello process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="e9bbf-212">V prohlížeči projektu Eclipse hello klikněte pravým tlačítkem na aplikaci Java hello mají tooupdate nebo přidat tooan existující webové aplikace kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-212">In hello Eclipse project explorer, right-click hello Java application you want tooupdate or add tooan existing Web App Container.</span></span>
2. <span data-ttu-id="e9bbf-213">Jakmile se zobrazí hello kontextové nabídky, vyberte **Azure** a potom **publikovat jako webové aplikace Azure...**</span><span class="sxs-lookup"><span data-stu-id="e9bbf-213">When hello context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="e9bbf-214">Vzhledem k tomu, že jste již přihlášení dříve, zobrazí se seznam kontejnerů existující webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-214">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="e9bbf-215">Vyberte jeden má toopublish nebo znovu publikovat vaše Java aplikace tooand klikněte na tlačítko hello **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-215">Select hello one you want toopublish or re-publish your Java application tooand click **OK**.</span></span>

<span data-ttu-id="e9bbf-216">Hello později, několik sekund **protokol činnosti Azure** zobrazení se zobrazí aktualizovaná nasazení jako **publikováno** a bude moct tooverify aktualizované aplikace ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-216">A few seconds later, hello **Azure Activity Log** view will show your updated deployment as **Published** and you will be able tooverify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="e9bbf-217">Spuštění, zastavení nebo restartování existující webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="e9bbf-217">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="e9bbf-218">toostart nebo zastavit existující webové aplikace Azure kontejneru (včetně všech aplikací v jazyce Java hello nasazené v ní), můžete použít hello **Azure Explorer** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-218">toostart or stop an existing Azure Web App container, (including all hello deployed Java applications in it), you can use hello **Azure Explorer** view.</span></span>

<span data-ttu-id="e9bbf-219">Pokud hello **Azure Explorer** zobrazení ještě není otevřené, který můžete otevřít kliknutím na tlačítko pak **okno** nabídky v prostředí Eclipse klikněte **zobrazit zobrazení**, pak **jiné...** , pak **Azure**a potom klikněte na **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-219">If hello **Azure Explorer** view is not already open, you can open it by clicking then **Window** menu in Eclipse, then click **Show View**, then **Other...**, then **Azure**, and then click **Azure Explorer**.</span></span> <span data-ttu-id="e9bbf-220">Pokud je uživatel není přihlášený dříve, vás vyzve toodo tak.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-220">If you have not previously logged in, it will prompt you toodo so.</span></span>

<span data-ttu-id="e9bbf-221">Když hello **Azure Explorer** se zobrazí, postupujte podle těchto kroků toostart použít nebo zastavit vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-221">When hello **Azure Explorer** view is displayed, use follow these steps toostart or stop your Web App:</span></span> 

1. <span data-ttu-id="e9bbf-222">Rozbalte hello **Azure** uzlu.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-222">Expand hello **Azure** node.</span></span>
2. <span data-ttu-id="e9bbf-223">Rozbalte hello **webové aplikace** uzlu.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-223">Expand hello **Web Apps** node.</span></span> 
3. <span data-ttu-id="e9bbf-224">Klikněte pravým tlačítkem na hello požadované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-224">Right-click hello desired Web App.</span></span>
4. <span data-ttu-id="e9bbf-225">Jakmile se zobrazí hello kontextové nabídky, klikněte na tlačítko **spustit**, **Zastavit**, nebo **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-225">When hello context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="e9bbf-226">Upozorňujeme, že jsou možnosti nabídky hello kontextově, tak můžete zastavit funkční webovou aplikaci nebo pouze spustí webovou aplikaci, která není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="e9bbf-226">Note that hello menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
    ![Zastavení stávající webovou aplikaci][13]

## <a name="next-steps"></a><span data-ttu-id="e9bbf-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9bbf-228">Next Steps</span></span>
<span data-ttu-id="e9bbf-229">Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="e9bbf-229">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="e9bbf-230">[nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e9bbf-230">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="e9bbf-231">[hello instalace nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e9bbf-231">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="e9bbf-232">*Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse (v tomto článku)*</span><span class="sxs-lookup"><span data-stu-id="e9bbf-232">*Create a Hello World Web App for Azure in Eclipse (This Article)*</span></span>
  * <span data-ttu-id="e9bbf-233">[Co je nového v hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e9bbf-233">[What's New in hello Azure Toolkit for Eclipse]</span></span>
* <span data-ttu-id="e9bbf-234">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="e9bbf-234">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="e9bbf-235">[Instalace hello Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="e9bbf-235">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="e9bbf-236">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="e9bbf-236">[Create a Hello World Web App for Azure in IntelliJ]</span></span>
  * <span data-ttu-id="e9bbf-237">[Co je nového v hello nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="e9bbf-237">[What's New in hello Azure Toolkit for IntelliJ]</span></span>

<span data-ttu-id="e9bbf-238">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java].</span><span class="sxs-lookup"><span data-stu-id="e9bbf-238">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

<span data-ttu-id="e9bbf-239">Další informace o vytváření webové aplikace Azure najdete v tématu hello [webových aplikací – přehled].</span><span class="sxs-lookup"><span data-stu-id="e9bbf-239">For additional information about creating Azure Web Apps, see hello [Web Apps Overview].</span></span>

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[hello instalace nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[webových aplikací – přehled]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

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
