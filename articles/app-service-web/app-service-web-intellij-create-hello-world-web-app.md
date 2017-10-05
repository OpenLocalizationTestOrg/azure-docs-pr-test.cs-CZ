---
title: "Vytvoření základní Azure webové aplikace v IntelliJ | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak vytvořit Hello World webovou aplikaci pro Azure pomocí sady nástrojů Azure pro IntelliJ."
services: app-service\web
documentationcenter: java
author: selvasingh
manager: erikre
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 20b2c3d86f5bde9302647f345aa99b030d78613a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a><span data-ttu-id="42ef6-103">Vytvoření základní Azure webové aplikace v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="42ef6-103">Create a basic Azure web app in IntelliJ</span></span>
<span data-ttu-id="42ef6-104">Tento kurz ukazuje, jak vytvořit a nasadit základní aplikace Hello World do Azure jako webovou aplikaci pomocí [nástrojů Azure pro IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="42ef6-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a Web App by using the [Azure Toolkit for IntelliJ].</span></span> <span data-ttu-id="42ef6-105">Základní příklad JSP je uvedený na jednoduchost, ale podobným způsobem může být vhodné pro Java servlet, co se týče nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="42ef6-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="42ef6-106">Po dokončení tohoto kurzu, vaše aplikace bude vypadat podobně jako na následujícím obrázku, při zobrazení ve webovém prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="42ef6-106">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Ukázkové webové stránky][01]

## <a name="prerequisites"></a><span data-ttu-id="42ef6-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="42ef6-108">Prerequisites</span></span>
* <span data-ttu-id="42ef6-109">Java Developer Kit (JDK), v 1.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="42ef6-109">A Java Developer Kit (JDK), v 1.8 or later.</span></span>
* <span data-ttu-id="42ef6-110">IntelliJ IDEA Ultimate Edition.</span><span class="sxs-lookup"><span data-stu-id="42ef6-110">IntelliJ IDEA Ultimate Edition.</span></span> <span data-ttu-id="42ef6-111">To si můžete stáhnout z <https://www.jetbrains.com/idea/download/index.html>.</span><span class="sxs-lookup"><span data-stu-id="42ef6-111">This can be downloaded from <https://www.jetbrains.com/idea/download/index.html>.</span></span>
* <span data-ttu-id="42ef6-112">Distribuční založené na jazyce Java webový server nebo aplikačního serveru jako například [Apache Tomcat] nebo [Jetty].</span><span class="sxs-lookup"><span data-stu-id="42ef6-112">A distribution of a Java-based web server or application server, such as [Apache Tomcat] or [Jetty].</span></span>
* <span data-ttu-id="42ef6-113">Předplatné Azure, který můžete získat z <https://azure.microsoft.com/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.</span><span class="sxs-lookup"><span data-stu-id="42ef6-113">An Azure subscription, which can be acquired from <https://azure.microsoft.com/free/> or <http://azure.microsoft.com/pricing/purchase-options/>.</span></span>
* <span data-ttu-id="42ef6-114">[nástrojů Azure pro IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="42ef6-114">The [Azure Toolkit for IntelliJ].</span></span> <span data-ttu-id="42ef6-115">Informace o instalaci sady Azure najdete v tématu [instalace sady Toolkit Azure pro IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="42ef6-115">For information about installing the Azure Toolkit, see [Installing the Azure Toolkit for IntelliJ].</span></span>

## <a name="to-create-a-hello-world-application"></a><span data-ttu-id="42ef6-116">Vytvoření aplikace Hello World</span><span class="sxs-lookup"><span data-stu-id="42ef6-116">To create a Hello World application</span></span>
<span data-ttu-id="42ef6-117">Nejdříve začneme s vytvořením projektu Java.</span><span class="sxs-lookup"><span data-stu-id="42ef6-117">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="42ef6-118">Spusťte IntelliJ a klikněte na **soubor** nabídky, pak klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-118">Start IntelliJ and click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
    ![Soubor nový projekt][02]
2. <span data-ttu-id="42ef6-120">V dialogovém okně Nový projekt vyberte **Java**, pak **webové aplikace**a potom klikněte na **nový** přidat projekt SDK.</span><span class="sxs-lookup"><span data-stu-id="42ef6-120">In the New Project dialog box, select **Java**, then **Web Application**, and then click **New** to add a Project SDK.</span></span>
   
    ![Dialogové okno Nový projekt][03a]
   
3. <span data-ttu-id="42ef6-122">Vyberte domovský adresář pro dialogové okno JDK, vyberte složku, kde je nainstalován vaší JDK a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-122">In the Select Home Directory for JDK dialog box, select the folder where your JDK is installed, and then click **OK**.</span></span> <span data-ttu-id="42ef6-123">Klikněte na tlačítko **Další** v dialogovém okně Nový projekt pokračovat.</span><span class="sxs-lookup"><span data-stu-id="42ef6-123">Click **Next** in the New Project dialog box to continue.</span></span>
   
    ![Zadejte JDK domovský adresář][03b]
4. <span data-ttu-id="42ef6-125">Pro účely tohoto kurzu, název projektu **Java-Web-aplikace-na-Azure**a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-125">For purposes of this tutorial, name the project **Java-Web-App-On-Azure**, and then click **Finish**.</span></span>
   
    ![Dialogové okno Nový projekt][04]
5. <span data-ttu-id="42ef6-127">V rámci zobrazení IntelliJ na prohlížeči projektu rozbalte **Java-Web-aplikace-na-Azure**, pak rozbalte **webové**a potom dvakrát klikněte na **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-127">Within IntelliJ's Project Explorer view, expand **Java-Web-App-On-Azure**, then expand **web**, and then double-click **index.jsp**.</span></span>
   
    ![Otevřete indexovou stránku][05c]
6. <span data-ttu-id="42ef6-129">Když se soubor index.jsp otevře v IntelliJ, přidejte v dynamicky zobrazený text **Hello, World!**</span><span class="sxs-lookup"><span data-stu-id="42ef6-129">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="42ef6-130">do existujícího elementu `<body>`.</span><span class="sxs-lookup"><span data-stu-id="42ef6-130">within the existing `<body>` element.</span></span> <span data-ttu-id="42ef6-131">Aktualizovaný `<body>` obsah by měl podobat následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="42ef6-131">Your updated `<body>` content should resemble the following example:</span></span>
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
7. <span data-ttu-id="42ef6-132">Uložte index.jsp.</span><span class="sxs-lookup"><span data-stu-id="42ef6-132">Save index.jsp.</span></span>

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a><span data-ttu-id="42ef6-133">Chcete-li nasadit aplikaci s kontejnerem Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="42ef6-133">To deploy your application to an Azure Web App Container</span></span>
<span data-ttu-id="42ef6-134">Existuje několik způsobů, pomocí kterých můžete nasadit webovou aplikaci Java do Azure.</span><span class="sxs-lookup"><span data-stu-id="42ef6-134">There are several ways by which you can deploy a Java web application to Azure.</span></span> <span data-ttu-id="42ef6-135">Tento kurz popisuje jedno z nejjednodušší: vaše aplikace bude nasazena na kontejner Azure Web App – žádné speciální projektu typu ani další nástroje jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="42ef6-135">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="42ef6-136">Sadu JDK a webový kontejner software budou poskytovat vám Azure, takže není nutné nahrát vlastní; je třeba je webové aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="42ef6-136">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="42ef6-137">Proces publikování pro aplikaci v důsledku toho bude trvat sekund, ne minuty.</span><span class="sxs-lookup"><span data-stu-id="42ef6-137">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

<span data-ttu-id="42ef6-138">Než můžete publikovat aplikaci, musíte nejprve nakonfigurovat nastavení modulu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-138">Before you publish your application, you first need to configure your module settings.</span></span> <span data-ttu-id="42ef6-139">Chcete-li tak učinit, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="42ef6-139">To do so, use the following steps:</span></span>

1. <span data-ttu-id="42ef6-140">V prohlížeči projektu na IntelliJ, klikněte pravým tlačítkem myši **Java-Web-aplikace-na-Azure** projektu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-140">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="42ef6-141">Jakmile se zobrazí místní nabídky, klikněte na tlačítko **otevřete nastavení modulu**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-141">When the context menu appears, click **Open Module Settings**.</span></span>

    ![Otevřete nastavení modulu][05a]
2. <span data-ttu-id="42ef6-143">Jakmile se zobrazí dialogové okno strukturu projektu:</span><span class="sxs-lookup"><span data-stu-id="42ef6-143">When the Project Structure dialog box appears:</span></span>

   <span data-ttu-id="42ef6-144">a.</span><span class="sxs-lookup"><span data-stu-id="42ef6-144">a.</span></span> <span data-ttu-id="42ef6-145">Klikněte na tlačítko **artefakty** v seznamu **nastavení projektu**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-145">Click **Artifacts** in the list of **Project Settings**.</span></span>
   <span data-ttu-id="42ef6-146">b.</span><span class="sxs-lookup"><span data-stu-id="42ef6-146">b.</span></span> <span data-ttu-id="42ef6-147">Změňte název artefaktů v **název** pole tak, aby neobsahuje prázdný znak nebo speciální znaky; to je nezbytné, protože název se použije v identifikátor URI (Uniform Resource).</span><span class="sxs-lookup"><span data-stu-id="42ef6-147">Change the artifact name in the **Name** box so that it doesn't contain whitespace or special characters; this is necessary since the name will be used in the Uniform Resource Identifier (URI).</span></span>
   <span data-ttu-id="42ef6-148">c.</span><span class="sxs-lookup"><span data-stu-id="42ef6-148">c.</span></span> <span data-ttu-id="42ef6-149">Změna **typ** k **webové aplikace: archivu**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-149">Change the **Type** to **Web Application: Archive**.</span></span>
   <span data-ttu-id="42ef6-150">d.</span><span class="sxs-lookup"><span data-stu-id="42ef6-150">d.</span></span> <span data-ttu-id="42ef6-151">Klikněte na tlačítko **OK** zavřete dialogové okno strukturu projektu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-151">Click **OK** to close the Project Structure dialog box.</span></span>

    ![Otevřete nastavení modulu][05b]

<span data-ttu-id="42ef6-153">Pokud jste nakonfigurovali nastavení modulu, můžete publikovat svoji aplikaci do Azure pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="42ef6-153">When you have configured your module settings, you can publish your application to Azure by using the following steps:</span></span>

1. <span data-ttu-id="42ef6-154">V prohlížeči projektu na IntelliJ, klikněte pravým tlačítkem myši **Java-Web-aplikace-na-Azure** projektu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-154">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="42ef6-155">Po zobrazení v místní nabídce vyberte **Azure**a pak klikněte na tlačítko **publikovat jako webové aplikace Azure...**</span><span class="sxs-lookup"><span data-stu-id="42ef6-155">When the context menu appears, select **Azure**, and then click **Publish as Azure Web App...**</span></span>
   
    ![Místní nabídka publikovat Azure][06]
2. <span data-ttu-id="42ef6-157">Pokud nebyly již přihlášení k Azure z IntelliJ, zobrazí se výzva k přihlásit k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="42ef6-157">If you have not already signed into Azure from IntelliJ, you will be prompted to sign into your Azure account.</span></span> <span data-ttu-id="42ef6-158">(Pokud máte více účtů Azure, některé výzvy při přihlašování v procesu se může zobrazit více než jednou, i když se objeví být stejné.</span><span class="sxs-lookup"><span data-stu-id="42ef6-158">(If you have multiple Azure accounts, some of the prompts during the sign in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="42ef6-159">V takovém případě postupujte dál podle pokynů přihlášení.)</span><span class="sxs-lookup"><span data-stu-id="42ef6-159">When this happens, continue to follow the sign in instructions.)</span></span>
   
    ![Protokolů Azure v dialogovém okně][07]
3. <span data-ttu-id="42ef6-161">Po úspěšné registraci ke svému účtu Azure **Spravovat odběry** dialogové okno se zobrazí seznam odběry, které jsou spojeny pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="42ef6-161">After you have successfully signed into your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="42ef6-162">(Pokud existuje jsou uvedena v seznamu více předplatných a chcete pracovat pouze konkrétní podmnožinu z nich, vám může volitelně zrušte zaškrtnutí políčka odběry, které nechcete použít.) Pokud jste vybrali vašich předplatných, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-162">(If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the subscriptions you don't want to use.) When you have selected your subscriptions, click **Close**.</span></span>
   
    ![Správa odběrů][08]
4. <span data-ttu-id="42ef6-164">Když **nasadit do Azure Web App kontejneru** se zobrazí dialogové okno, zobrazí všechny kontejnery webové aplikace, které jste předtím vytvořili; Pokud jste nevytvořili žádné kontejnery, bude seznam prázdný.</span><span class="sxs-lookup"><span data-stu-id="42ef6-164">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
    ![Kontejnery aplikací][09]
5. <span data-ttu-id="42ef6-166">Pokud jste dosud nevytvořili kontejner Azure webové aplikace před, nebo pokud chcete publikovat aplikaci pro nový kontejner, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="42ef6-166">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="42ef6-167">Jinak vyberte existujícího webového kontejneru aplikace a přejděte ke kroku 6 níže.</span><span class="sxs-lookup"><span data-stu-id="42ef6-167">Otherwise, select an existing Web App Container and skip to step 6 below.</span></span>
   
   1. <span data-ttu-id="42ef6-168">Klikněte na**+**</span><span class="sxs-lookup"><span data-stu-id="42ef6-168">Click **+**</span></span>
      
       ![Přidat kontejner aplikace][10]
   2. <span data-ttu-id="42ef6-170">**Nový kontejner webové aplikace** dialogové okno se zobrazí, který se použije pro následujících několika krocích.</span><span class="sxs-lookup"><span data-stu-id="42ef6-170">The **New Web App Container** dialog box will be displayed, which will be used for the next several steps.</span></span>
      
       ![Nový kontejner aplikace][11a]
   3. <span data-ttu-id="42ef6-172">Zadejte **popisek DNS** pro kontejner vaší webové aplikace; to budou formovat popisek DNS listu hostitelské adresy URL pro webovou aplikaci v Azure.</span><span class="sxs-lookup"><span data-stu-id="42ef6-172">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="42ef6-173">Všimněte si, že název musí být k dispozici a v souladu s požadavky na pojmenování webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="42ef6-173">Note that the name must be available and conform to Azure Web App naming requirements.</span></span>
   4. <span data-ttu-id="42ef6-174">V **webový kontejner** rozevírací nabídky vyberte příslušný software pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="42ef6-174">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
       <span data-ttu-id="42ef6-175">V současné době můžete z Tomcat 8, Tomcat 7 nebo Jetty 9.</span><span class="sxs-lookup"><span data-stu-id="42ef6-175">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="42ef6-176">Poslední distribuční vybrané softwaru budou poskytovat Azure a bude spuštěna v posledních distribučním JDK 8 vytvořené Oracle a poskytovaný platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="42ef6-176">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>
   5. <span data-ttu-id="42ef6-177">V **předplatné** rozevírací nabídky vyberte předplatné, kterou chcete použít pro toto nasazení.</span><span class="sxs-lookup"><span data-stu-id="42ef6-177">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>
   6. <span data-ttu-id="42ef6-178">V **skupiny prostředků** rozevírací nabídky vyberte skupinu prostředků, pro který chcete přidružit webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="42ef6-178">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="42ef6-179">(Skupiny prostředků azure umožňují seskupit související prostředky tak, aby například, můžete je odstranit společně.)</span><span class="sxs-lookup"><span data-stu-id="42ef6-179">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
       <span data-ttu-id="42ef6-180">Můžete vybrat existující skupinu prostředků (pokud nějaké máte) a přeskočit na krok g níže, nebo pomocí následujících kroků můžete vytvořit novou skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="42ef6-180">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="42ef6-181">Vyberte  **&lt; &lt; vytvořit novou skupinu prostředků &gt; &gt;**  v **skupiny prostředků** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="42ef6-181">Select **&lt;&lt; Create new Resource Group &gt;&gt;** in the **Resource Group** drop-down menu.</span></span>
      * <span data-ttu-id="42ef6-182">**Novou skupinu prostředků** zobrazí se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="42ef6-182">The **New Resource Group** dialog box will be displayed:</span></span>
        
          ![Novou skupinu prostředků][12]
      * <span data-ttu-id="42ef6-184">V **název** textovému poli, zadejte název nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="42ef6-184">In the the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="42ef6-185">V **oblast** rozevírací nabídky vyberte odpovídající Azure datového centra umístění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="42ef6-185">In the the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="42ef6-186">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-186">Click **OK**.</span></span>
   7. <span data-ttu-id="42ef6-187">**Plán služby App Service** rozevírací nabídce uvádí plány služby aplikace, které jsou spojeny se skupinami prostředků, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="42ef6-187">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="42ef6-188">(Plán služby App Service určuje informace o umístění vaší webové aplikace, cenové úrovně a velikost výpočetní instance.</span><span class="sxs-lookup"><span data-stu-id="42ef6-188">(An App Service Plan specifies information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="42ef6-189">Jednoho plánu služby App Service můžete použít pro více webových aplikací, které je důvod, proč je zachován odděleně od konkrétní nasazení webové aplikace.)</span><span class="sxs-lookup"><span data-stu-id="42ef6-189">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       <span data-ttu-id="42ef6-190">Můžete vyberte existující plán služby App Service (pokud nějaké máte) a přeskočit na krok h níže, nebo použijte následující postup k vytvoření nový plán služby App Service:</span><span class="sxs-lookup"><span data-stu-id="42ef6-190">You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following steps to create a new App Service Plan:</span></span>
      
      * <span data-ttu-id="42ef6-191">Vyberte  **&lt; &lt; vytvořit nový plán služby App Service &gt; &gt;**  v **plán služby App Service** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="42ef6-191">Select **&lt;&lt; Create new App Service Plan &gt;&gt;** in the **App Service Plan** drop-down menu.</span></span>
      * <span data-ttu-id="42ef6-192">**Nový plán služby App Service** zobrazí se dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="42ef6-192">The **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Nový plán aplikační služby][13]
      * <span data-ttu-id="42ef6-194">V **název** textovému poli, zadejte název pro váš nový plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="42ef6-194">In the the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="42ef6-195">V **umístění** rozevírací nabídky vyberte odpovídající Azure datového centra umístění pro plán.</span><span class="sxs-lookup"><span data-stu-id="42ef6-195">In the the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="42ef6-196">V **cenová úroveň** rozevírací nabídky vyberte příslušné ceny pro plán.</span><span class="sxs-lookup"><span data-stu-id="42ef6-196">In the the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="42ef6-197">Při testování můžete **volné**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-197">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="42ef6-198">V **velikost Instance** rozevírací nabídky, vyberte příslušnou instanci velikost plánu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-198">In the the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="42ef6-199">Při testování můžete **malé**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-199">For testing purposes you can choose **Small**.</span></span>
      * <span data-ttu-id="42ef6-200">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-200">Click **OK**.</span></span>
   8. <span data-ttu-id="42ef6-201">(Volitelné) Ve výchozím nastavení poslední distribuce Java 8 budou automaticky nasazeny jako vaše prostředí Java Virtual Machine Azure do kontejneru vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="42ef6-201">(Optional) By default, a recent distribution of Java 8 will be automatically deployed as your JVM by Azure to your web app container.</span></span> <span data-ttu-id="42ef6-202">Můžete ale vybrat jinou verzi a distribuci systém JVM.</span><span class="sxs-lookup"><span data-stu-id="42ef6-202">However, you can select a different version and distribution of the JVM.</span></span> <span data-ttu-id="42ef6-203">Chcete-li tak učinit, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="42ef6-203">To do so, use the following steps:</span></span>
      
      * <span data-ttu-id="42ef6-204">Klikněte **JDK** ve **nový kontejner webové aplikace** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="42ef6-204">Click the **JDK** tab in the **New Web App Container** dialog box.</span></span>
      * <span data-ttu-id="42ef6-205">Můžete zvolit jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="42ef6-205">You can choose from one of the following options:</span></span>
        
        * <span data-ttu-id="42ef6-206">Nasadit výchozí JDK, které nabízí Azure</span><span class="sxs-lookup"><span data-stu-id="42ef6-206">Deploy the default JDK which is offered by Azure</span></span>
        * <span data-ttu-id="42ef6-207">Nasazení 3. stran JDK z rozevíracího seznamu další JDKs, které jsou k dispozici v Azure</span><span class="sxs-lookup"><span data-stu-id="42ef6-207">Deploy a 3rd party JDK from a drop-down list of additional JDKs which are available on Azure</span></span>
        * <span data-ttu-id="42ef6-208">Nasazení vlastní JDK, které musí být zabalené jako soubor ZIP a buď veřejně dostupné nebo ve vašem účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="42ef6-208">Deploy a custom JDK, which must be packaged as a ZIP file and either publicly available or in your Azure storage account</span></span>
        
        ![Novou kartu JDK kontejneru aplikace][11b]
   9. <span data-ttu-id="42ef6-210">Po dokončení všech výše uvedených kroků, dialogové okno Nový kontejner webové aplikace by měl vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="42ef6-210">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
       ![Nový kontejner aplikace][14]
   10. <span data-ttu-id="42ef6-212">Klikněte na tlačítko **OK** vytvoření kontejneru vaší nové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="42ef6-212">Click **OK** to complete the creation of your new Web App container.</span></span>
       
        <span data-ttu-id="42ef6-213">Počkejte několik sekund seznam kontejnery webové aplikace nutné je aktualizovat a nově vytvořený webový kontejner aplikace měla by být vybrána nyní v seznamu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-213">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>
6. <span data-ttu-id="42ef6-214">Nyní jste připraveni k dokončení počáteční nasazení vaší webové aplikace do Azure; Klikněte na tlačítko **OK** k nasazení aplikace v jazyce Java do vybraného kontejneru webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="42ef6-214">You are now ready to complete the initial deployment of your Web App to Azure; click **OK** to deploy your Java application to the selected Web App container.</span></span> <span data-ttu-id="42ef6-215">Ve výchozím nastavení budou aplikace nasazeny jako podadresáři aplikačního serveru.</span><span class="sxs-lookup"><span data-stu-id="42ef6-215">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="42ef6-216">Pokud chcete, aby ho nasadit jako kořenová aplikace, podívejte se **nasadit do kořenové** políčko před kliknutím na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-216">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>
   
    ![Nasazení do Azure][15]
7. <span data-ttu-id="42ef6-218">V dalším kroku byste měli vidět **protokol činnosti Azure** zobrazení, které se označují stav nasazení vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="42ef6-218">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
    ![Ukazatel průběhu][16]
   
    <span data-ttu-id="42ef6-220">Proces nasazení webové aplikace do Azure by měl trvat jenom pár sekund.</span><span class="sxs-lookup"><span data-stu-id="42ef6-220">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="42ef6-221">Pokud vaše aplikace připravené, zobrazí se odkaz s názvem **publikováno** v **stav** sloupce.</span><span class="sxs-lookup"><span data-stu-id="42ef6-221">When your application ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="42ef6-222">Po kliknutí na odkaz, jeho přejdete na domovskou stránku vaší nasazené webové aplikace, nebo můžete použít kroky v následující části a přejděte do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="42ef6-222">When you click the link, it will take you to your deployed Web App's home page, or you can use the steps in the following section to browse to your web app.</span></span>

## <a name="browsing-to-your-web-app-on-azure"></a><span data-ttu-id="42ef6-223">Procházení do vaší webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="42ef6-223">Browsing to your Web App on Azure</span></span>
<span data-ttu-id="42ef6-224">Chcete-li procházet do vaší webové aplikace v Azure, můžete použít **Azure Explorer** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="42ef6-224">To browse to your Web App on Azure, you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="42ef6-225">Pokud **Azure Explorer** zobrazení ještě není otevřené, který můžete otevřít kliknutím na tlačítko pak **zobrazení** nabídky v IntelliJ, pak klikněte na **nástroj Windows**a potom klikněte na  **Průzkumník služby**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-225">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="42ef6-226">Pokud jste nepřihlásili dříve, vyzve vás k tomu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-226">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="42ef6-227">Když **Azure Explorer** se zobrazí, použijte proveďte následující kroky a přejděte do vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="42ef6-227">When the **Azure Explorer** view is displayed, use follow these steps to browse to your Web App:</span></span> 

1. <span data-ttu-id="42ef6-228">Rozbalte **Azure** uzlu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-228">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="42ef6-229">Rozbalte **webové aplikace** uzlu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-229">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="42ef6-230">Klikněte pravým tlačítkem na požadovanou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="42ef6-230">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="42ef6-231">Jakmile se zobrazí místní nabídky, klikněte na tlačítko **otevřít v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-231">When the context menu appears, click **Open in Browser**.</span></span>
   
    ![Procházet webové aplikace][17]

## <a name="updating-your-web-app"></a><span data-ttu-id="42ef6-233">Aktualizace webové aplikace</span><span class="sxs-lookup"><span data-stu-id="42ef6-233">Updating your web app</span></span>
<span data-ttu-id="42ef6-234">Aktualizace stávající spuštění webové aplikace Azure je rychlý a snadný proces a máte dvě možnosti pro aktualizaci:</span><span class="sxs-lookup"><span data-stu-id="42ef6-234">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="42ef6-235">Můžete aktualizovat nasazení stávající webovou aplikaci Java.</span><span class="sxs-lookup"><span data-stu-id="42ef6-235">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="42ef6-236">Můžete publikovat aplikaci Java další ke stejnému kontejneru webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="42ef6-236">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="42ef6-237">V obou případech proces je stejný jako a trvá jenom pár sekund:</span><span class="sxs-lookup"><span data-stu-id="42ef6-237">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="42ef6-238">V prohlížeči projektu IntelliJ klikněte pravým tlačítkem na aplikaci Java, které chcete aktualizovat nebo přidat do existujícího webového kontejneru aplikace.</span><span class="sxs-lookup"><span data-stu-id="42ef6-238">In the IntelliJ project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>
2. <span data-ttu-id="42ef6-239">Po zobrazení v místní nabídce vyberte **Azure** a potom **publikovat jako webové aplikace Azure...**</span><span class="sxs-lookup"><span data-stu-id="42ef6-239">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="42ef6-240">Vzhledem k tomu, že jste již přihlášení dříve, zobrazí se seznam kontejnerů existující webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="42ef6-240">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="42ef6-241">Vyberte ten, který chcete publikovat nebo znovu publikovat aplikace Java a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-241">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="42ef6-242">Později, několik sekund **protokol činnosti Azure** zobrazení se zobrazí aktualizovaná nasazení jako **publikováno** a vy nebudete moci ověřit aktualizované aplikace ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="42ef6-242">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="42ef6-243">Spuštění, zastavení nebo restartování existující webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="42ef6-243">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="42ef6-244">Spuštění nebo zastavení existující webové aplikace Azure kontejneru (včetně všech nasazených aplikací Java v ní), můžete použít **Azure Explorer** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="42ef6-244">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="42ef6-245">Pokud **Azure Explorer** zobrazení ještě není otevřené, který můžete otevřít kliknutím na tlačítko pak **zobrazení** nabídky v IntelliJ, pak klikněte na **nástroj Windows**a potom klikněte na  **Průzkumník služby**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-245">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="42ef6-246">Pokud jste nepřihlásili dříve, vyzve vás k tomu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-246">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="42ef6-247">Když **Azure Explorer** zobrazení, pomocí následujícího postupu spuštění nebo zastavení webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="42ef6-247">When the **Azure Explorer** view is displayed, use follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="42ef6-248">Rozbalte **Azure** uzlu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-248">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="42ef6-249">Rozbalte **webové aplikace** uzlu.</span><span class="sxs-lookup"><span data-stu-id="42ef6-249">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="42ef6-250">Klikněte pravým tlačítkem na požadovanou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="42ef6-250">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="42ef6-251">Jakmile se zobrazí místní nabídky, klikněte na tlačítko **spustit**, **Zastavit**, nebo **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="42ef6-251">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="42ef6-252">Všimněte si, že možnosti nabídky kontextově, tak můžete zastavit funkční webovou aplikaci nebo pouze spustí webovou aplikaci, která není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="42ef6-252">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
    ![Zastavení webové aplikace][18]

## <a name="next-steps"></a><span data-ttu-id="42ef6-254">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42ef6-254">Next Steps</span></span>
<span data-ttu-id="42ef6-255">Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="42ef6-255">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="42ef6-256">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="42ef6-256">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="42ef6-257">[Instalace sady Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="42ef6-257">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="42ef6-258">[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="42ef6-258">[Create a Hello World Web App for Azure in Eclipse]</span></span>
  * <span data-ttu-id="42ef6-259">[Novinky v sadě Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="42ef6-259">[What's New in the Azure Toolkit for Eclipse]</span></span>
* <span data-ttu-id="42ef6-260">[nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="42ef6-260">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="42ef6-261">[instalace sady Toolkit Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="42ef6-261">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="42ef6-262">*Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ (v tomto článku)*</span><span class="sxs-lookup"><span data-stu-id="42ef6-262">*Create a Hello World Web App for Azure in IntelliJ (This Article)*</span></span>
  * <span data-ttu-id="42ef6-263">[Novinky v sadě Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="42ef6-263">[What's New in the Azure Toolkit for IntelliJ]</span></span>

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="42ef6-264">Viz také</span><span class="sxs-lookup"><span data-stu-id="42ef6-264">See Also</span></span>
<span data-ttu-id="42ef6-265">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].</span><span class="sxs-lookup"><span data-stu-id="42ef6-265">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<span data-ttu-id="42ef6-266">Další informace o vytváření webové aplikace Azure najdete v tématu [webových aplikací – přehled].</span><span class="sxs-lookup"><span data-stu-id="42ef6-266">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse.md
[nástrojů Azure pro IntelliJ]: ../azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Instalace sady Azure Toolkit pro Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[instalace sady Toolkit Azure pro IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Novinky v sadě Azure Toolkit pro Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Novinky v sadě Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[webových aplikací – přehled]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-SDK-Dialog.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05a]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Module-Settings.png
[05b]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Project-Structure-Dialog.png
[05c]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11a]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[11b]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container-JDK-Tab.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
