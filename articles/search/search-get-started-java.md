---
title: "aaaGet začít s Azure Search v Javě | Microsoft Docs"
description: "Jak toobuild hostovaného cloudu vyhledat aplikaci v Azure pomocí programovacího jazyka Java."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 8b4df3c9-3ae5-4e3a-b4bb-74b516a91c8e
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 07/14/2016
ms.author: evboyle
ms.openlocfilehash: 5476a2103f3b60fe6ec78ff3d3fdba9fcff55c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-java"></a><span data-ttu-id="1a012-103">Začínáme s Azure Search v Javě</span><span class="sxs-lookup"><span data-stu-id="1a012-103">Get started with Azure Search in Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1a012-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1a012-104">Portal</span></span>](search-get-started-portal.md)
> * [<span data-ttu-id="1a012-105">.NET</span><span class="sxs-lookup"><span data-stu-id="1a012-105">.NET</span></span>](search-howto-dotnet-sdk.md)
> 
> 

<span data-ttu-id="1a012-106">Zjistěte, jak toobuild vlastní Java Hledat aplikace, která používá službu Azure Search své zkušenosti vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="1a012-106">Learn how toobuild a custom Java search application that uses Azure Search for its search experience.</span></span> <span data-ttu-id="1a012-107">Tento kurz používá hello [rozhraní REST API služby Azure Search](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objekty a operace, na které se použijí v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="1a012-107">This tutorial uses hello [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objects and operations used in this exercise.</span></span>

<span data-ttu-id="1a012-108">toorun této ukázky musíte mít službu Azure Search, které se můžete zaregistrovat si v hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1a012-108">toorun this sample, you must have an Azure Search service, which you can sign up for in hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="1a012-109">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="1a012-109">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for step-by-step instructions.</span></span>

<span data-ttu-id="1a012-110">Můžeme použít hello následující software toobuild a testování tohoto příkladu:</span><span class="sxs-lookup"><span data-stu-id="1a012-110">We used hello following software toobuild and test this sample:</span></span>

* <span data-ttu-id="1a012-111">[Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar)</span><span class="sxs-lookup"><span data-stu-id="1a012-111">[Eclipse IDE for Java EE Developers](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar).</span></span> <span data-ttu-id="1a012-112">Být jisti verzi EE toodownload hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-112">Be sure toodownload hello EE version.</span></span> <span data-ttu-id="1a012-113">Jeden z kroků ověření hello vyžaduje funkci, která se nachází pouze v této edici.</span><span class="sxs-lookup"><span data-stu-id="1a012-113">One of hello verification steps requires a feature that is found only in this edition.</span></span>
* [<span data-ttu-id="1a012-114">JDK 8u40</span><span class="sxs-lookup"><span data-stu-id="1a012-114">JDK 8u40</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [<span data-ttu-id="1a012-115">Apache Tomcat 8.0</span><span class="sxs-lookup"><span data-stu-id="1a012-115">Apache Tomcat 8.0</span></span>](http://tomcat.apache.org/download-80.cgi)

## <a name="about-hello-data"></a><span data-ttu-id="1a012-116">O datech hello</span><span class="sxs-lookup"><span data-stu-id="1a012-116">About hello data</span></span>
<span data-ttu-id="1a012-117">Tato ukázková aplikace používá data z hello [Spojených států Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), která jsou filtrovaná na velikost datové sady hello státu Rhode Island tooreduce hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-117">This sample application uses data from hello [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtered on hello state of Rhode Island tooreduce hello dataset size.</span></span> <span data-ttu-id="1a012-118">Použijeme tato data toobuild vyhledávací aplikaci, která najde významné budovy, například nemocnice a školy, a geologické prvky, jako jsou datové proudy, jezera a vrcholy.</span><span class="sxs-lookup"><span data-stu-id="1a012-118">We'll use this data toobuild a search application that returns landmark buildings such as hospitals and schools, as well as geological features like streams, lakes, and summits.</span></span>

<span data-ttu-id="1a012-119">V této aplikaci hello **SearchServlet.java** program sestaví a zatížením hello index pomocí [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) konstrukce, načítání hello filtrovat sadu dat USGS z veřejné databáze Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1a012-119">In this application, hello **SearchServlet.java** program builds and loads hello index using an [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) construct, retrieving hello filtered USGS dataset from a public Azure SQL Database.</span></span> <span data-ttu-id="1a012-120">Předdefinované přihlašovací údaje a připojení informace toohello online zdroji dat jsou uvedené v kódu programu hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-120">Predefined credentials and connection  information toohello online data source are provided in hello program code.</span></span> <span data-ttu-id="1a012-121">Z hlediska přístupu k datům není potřeba žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1a012-121">In terms of data access, no further configuration is necessary.</span></span>

> [!NOTE]
> <span data-ttu-id="1a012-122">Jsme použili filtr na tuto datovou sadu toostay pod hello 10 000 dokumentů limit hello volné cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="1a012-122">We applied a filter on this dataset toostay under hello 10,000 document limit of hello free pricing tier.</span></span> <span data-ttu-id="1a012-123">Pokud používáte úroveň standard hello, toto omezení se nevztahuje a můžete upravit tento kód toouse větší datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="1a012-123">If you use hello standard tier, this limit does not apply, and you can modify this code toouse a bigger dataset.</span></span> <span data-ttu-id="1a012-124">Podrobnosti týkající se kapacity u jednotlivých cenových úrovní najdete v tématu [Omezení](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="1a012-124">For details about capacity for each pricing tier, see [Limits and constraints](search-limits-quotas-capacity.md).</span></span>
> 
> 

## <a name="about-hello-program-files"></a><span data-ttu-id="1a012-125">O souborech programu hello</span><span class="sxs-lookup"><span data-stu-id="1a012-125">About hello program files</span></span>
<span data-ttu-id="1a012-126">Hello následující seznam popisuje hello soubory, které jsou relevantní toothis ukázka.</span><span class="sxs-lookup"><span data-stu-id="1a012-126">hello following list describes hello files that are relevant toothis sample.</span></span>

* <span data-ttu-id="1a012-127">Search.jsp: Poskytuje uživatelské rozhraní hello</span><span class="sxs-lookup"><span data-stu-id="1a012-127">Search.jsp: Provides hello user interface</span></span>
* <span data-ttu-id="1a012-128">SearchServlet.java: Poskytuje metody (podobně jako tooa řadič v MVC)</span><span class="sxs-lookup"><span data-stu-id="1a012-128">SearchServlet.java: Provides methods (similar tooa controller in MVC)</span></span>
* <span data-ttu-id="1a012-129">SearchServiceClient.java: Zpracovává požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="1a012-129">SearchServiceClient.java: Handles HTTP requests</span></span>
* <span data-ttu-id="1a012-130">SearchServiceHelper.java: Pomocná třída, která poskytuje statické metody.</span><span class="sxs-lookup"><span data-stu-id="1a012-130">SearchServiceHelper.java: A helper class that provides static methods</span></span>
* <span data-ttu-id="1a012-131">Document.Java: Poskytuje datový model hello</span><span class="sxs-lookup"><span data-stu-id="1a012-131">Document.java: Provides hello data model</span></span>
* <span data-ttu-id="1a012-132">config.Properties: Nastavuje adresu URL služby Search hello a klíč api-key</span><span class="sxs-lookup"><span data-stu-id="1a012-132">config.properties: Sets hello Search service URL and api-key</span></span>
* <span data-ttu-id="1a012-133">Pom.xml: Závislost Maven</span><span class="sxs-lookup"><span data-stu-id="1a012-133">Pom.xml: A Maven dependency</span></span>

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a><span data-ttu-id="1a012-134">Nalezení názvu služby hello a klíč api-key služby Azure Search</span><span class="sxs-lookup"><span data-stu-id="1a012-134">Find hello service name and api-key of your Azure Search service</span></span>
<span data-ttu-id="1a012-135">Všechna volání rozhraní REST API Azure Search vyžaduje, aby vám poskytla hello adresa URL služby a klíč rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="1a012-135">All REST API calls into Azure Search require that you provide hello service URL and an api-key.</span></span> 

1. <span data-ttu-id="1a012-136">Přihlaste se toohello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1a012-136">Sign in toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="1a012-137">Hello přechod panelu klikněte na **služby vyhledávání** toolist všechny hello služby Azure Search zřízených pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="1a012-137">In hello jump bar, click **Search service** toolist all of hello Azure Search services provisioned for your subscription.</span></span>
3. <span data-ttu-id="1a012-138">Vyberte službu hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="1a012-138">Select hello service you want toouse.</span></span>
4. <span data-ttu-id="1a012-139">Na hello panelu služby uvidíte dlaždice se základními informacemi a ikonu hello klíče pro přístup ke klíčům správce hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-139">On hello service dashboard, you'll see tiles for essential information as well as hello key icon for accessing hello admin keys.</span></span>
   
      ![][3]
5. <span data-ttu-id="1a012-140">Zkopírujte adresu URL hello služby a klíč správce.</span><span class="sxs-lookup"><span data-stu-id="1a012-140">Copy hello service URL and an admin key.</span></span> <span data-ttu-id="1a012-141">Budete je potřebovat později, až je budete přidávat toohello **config.properties** souboru.</span><span class="sxs-lookup"><span data-stu-id="1a012-141">You will need them later, when you add them toohello **config.properties** file.</span></span>

## <a name="download-hello-sample-files"></a><span data-ttu-id="1a012-142">Stažení ukázkových souborů hello</span><span class="sxs-lookup"><span data-stu-id="1a012-142">Download hello sample files</span></span>
1. <span data-ttu-id="1a012-143">Přejděte příliš[AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="1a012-143">Go too[AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) on GitHub.</span></span>
2. <span data-ttu-id="1a012-144">Klikněte na tlačítko **stáhnout ZIP**, uložte toodisk soubor .zip hello a pak extrahujte všechny soubory hello obsahuje.</span><span class="sxs-lookup"><span data-stu-id="1a012-144">Click **Download ZIP**, save hello .zip file toodisk, and then extract all hello files it contains.</span></span> <span data-ttu-id="1a012-145">Zvažte extrahování hello souborů do vašeho pracovního prostoru toomake Java snazší projekt toofind hello později.</span><span class="sxs-lookup"><span data-stu-id="1a012-145">Consider extracting hello files into your Java workspace toomake it easier toofind hello project later.</span></span>
3. <span data-ttu-id="1a012-146">Hello ukázkové soubory jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="1a012-146">hello sample files are read-only.</span></span> <span data-ttu-id="1a012-147">Klikněte pravým tlačítkem na složku vlastnosti a zrušte hello atribut jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="1a012-147">Right-click folder properties and clear hello read-only attribute.</span></span>

<span data-ttu-id="1a012-148">Všechny následné úpravy souborů a spouštěné příkazy se budou provádět na souborech v této složce.</span><span class="sxs-lookup"><span data-stu-id="1a012-148">All subsequent file modifications and run statements will be made against files in this folder.</span></span>  

## <a name="import-project"></a><span data-ttu-id="1a012-149">Import projektu</span><span class="sxs-lookup"><span data-stu-id="1a012-149">Import project</span></span>
1. <span data-ttu-id="1a012-150">V prostředí Eclipse zvolte **Soubor** > **Import** > **Obecné** > **Existující projekty do pracovního prostoru**.</span><span class="sxs-lookup"><span data-stu-id="1a012-150">In Eclipse, choose **File** > **Import** > **General** > **Existing Projects into Workspace**.</span></span>
   
    ![][4]
2. <span data-ttu-id="1a012-151">V **vybrat kořenový adresář**, procházet toohello složky obsahující ukázkové soubory.</span><span class="sxs-lookup"><span data-stu-id="1a012-151">In **Select root directory**, browse toohello folder containing sample files.</span></span> <span data-ttu-id="1a012-152">Vyberte složku hello, která obsahuje složku .project hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-152">Select hello folder that contains hello .project folder.</span></span> <span data-ttu-id="1a012-153">Hello projekt by se měla objevit v hello **projekty** seznamu vybranou položku.</span><span class="sxs-lookup"><span data-stu-id="1a012-153">hello project should appear in hello **Projects** list as a selected item.</span></span>
   
    ![][12]
3. <span data-ttu-id="1a012-154">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="1a012-154">Click **Finish**.</span></span>
4. <span data-ttu-id="1a012-155">Použití **Project Exploreru** tooview a úpravy souborů hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-155">Use **Project Explorer** tooview and edit hello files.</span></span> <span data-ttu-id="1a012-156">Pokud již není otevřený, klikněte na tlačítko **okno** > **zobrazit zobrazení** > **Project Exploreru** nebo pomocí zástupce tooopen hello ho.</span><span class="sxs-lookup"><span data-stu-id="1a012-156">If it's not already open, click **Window** > **Show View** > **Project Explorer** or use hello shortcut tooopen it.</span></span>

## <a name="configure-hello-service-url-and-api-key"></a><span data-ttu-id="1a012-157">Konfigurace hello adresa URL služby a klíč api-key</span><span class="sxs-lookup"><span data-stu-id="1a012-157">Configure hello service URL and api-key</span></span>
1. <span data-ttu-id="1a012-158">V **Project Exploreru**, dvakrát klikněte na **config.properties** nastavení konfigurace hello tooedit obsahující hello název serveru a klíč api-key.</span><span class="sxs-lookup"><span data-stu-id="1a012-158">In **Project Explorer**, double-click **config.properties** tooedit hello configuration settings containing hello server name and api-key.</span></span>
2. <span data-ttu-id="1a012-159">Odkazovat toohello postup uvedený dříve v tomto článku, kde jste našli hello adresa URL služby a klíč api-key v hello [portálu Azure](https://portal.azure.com), tooget hello hodnoty, které nyní zadáte do **config.properties**.</span><span class="sxs-lookup"><span data-stu-id="1a012-159">Refer toohello steps earlier in this article, where you found hello service URL and api-key in hello [Azure Portal](https://portal.azure.com), tooget hello values you will now enter into **config.properties**.</span></span>
3. <span data-ttu-id="1a012-160">V **config.properties**, nahraďte "Api Key" hello api-key pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="1a012-160">In **config.properties**, replace "Api Key" with hello api-key for your service.</span></span> <span data-ttu-id="1a012-161">V dalším kroku název služby (hello první komponenta adresy URL http://servicename.search.windows.net hello) nahradí "název služby" v hello stejného souboru.</span><span class="sxs-lookup"><span data-stu-id="1a012-161">Next, service name (hello first component of hello URL http://servicename.search.windows.net) replaces "service name" in hello same file.</span></span>
   
    ![][5]

## <a name="configure-hello-project-build-and-runtime-environments"></a><span data-ttu-id="1a012-162">Konfigurovat hello projektu, buildu a běhového prostředí</span><span class="sxs-lookup"><span data-stu-id="1a012-162">Configure hello project, build and runtime environments</span></span>
1. <span data-ttu-id="1a012-163">V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na projekt hello > **vlastnosti** > **omezující vlastnosti projektu**.</span><span class="sxs-lookup"><span data-stu-id="1a012-163">In Eclipse, in Project Explorer, right-click hello project > **Properties** > **Project Facets**.</span></span>
2. <span data-ttu-id="1a012-164">Vyberte **Dynamic Web Module**, **Java** a **JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="1a012-164">Select **Dynamic Web Module**, **Java**, and **JavaScript**.</span></span>
   
    ![][6]
3. <span data-ttu-id="1a012-165">Klikněte na **Použít**.</span><span class="sxs-lookup"><span data-stu-id="1a012-165">Click **Apply**.</span></span>
4. <span data-ttu-id="1a012-166">Vyberte **Okno** > **Předvolby** > **Server** > **Běhová prostředí** > **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="1a012-166">Select **Window** > **Preferences** > **Server** > **Runtime Environments** > **Add..**.</span></span>
5. <span data-ttu-id="1a012-167">Apache rozbalte a vyberte server Apache Tomcat hello, které jste dříve nainstalovali verzi hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-167">Expand Apache and select hello version of hello Apache Tomcat server you previously installed.</span></span> <span data-ttu-id="1a012-168">V našem systému jsme nainstalovali verzi 8.</span><span class="sxs-lookup"><span data-stu-id="1a012-168">On our system, we installed version 8.</span></span>
   
    ![][7]
6. <span data-ttu-id="1a012-169">Na další stránku hello zadejte instalační adresář Tomcat hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-169">On hello next page, specify hello Tomcat installation directory.</span></span> <span data-ttu-id="1a012-170">V počítači s Windows to bude pravděpodobně C:\Program Files\Apache Software Foundation\Tomcat *verze*.</span><span class="sxs-lookup"><span data-stu-id="1a012-170">On a Windows computer, this will most likely be C:\Program Files\Apache Software Foundation\Tomcat *version*.</span></span>
7. <span data-ttu-id="1a012-171">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="1a012-171">Click **Finish**.</span></span>
8. <span data-ttu-id="1a012-172">Vyberte **Okno** > **Předvolby** > **Java** > **Nainstalovaná prostředí JRE** > **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="1a012-172">Select **Window** > **Preferences** > **Java** > **Installed JREs** > **Add**.</span></span>
9. <span data-ttu-id="1a012-173">V okně **Přidat prostředí JRE**, vyberte **Standardní virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="1a012-173">In **Add JRE**, select **Standard VM**.</span></span>
10. <span data-ttu-id="1a012-174">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="1a012-174">Click **Next**.</span></span>
11. <span data-ttu-id="1a012-175">V definici prostředí JRE v kořenovém adresáři JRE klikněte na **Adresář**.</span><span class="sxs-lookup"><span data-stu-id="1a012-175">In JRE Definition, in JRE home, click **Directory**.</span></span>
12. <span data-ttu-id="1a012-176">Přejděte příliš**Program Files** > **Java** a vyberte hello JDK jste dříve nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="1a012-176">Navigate too**Program Files** > **Java** and select hello JDK you previously installed.</span></span> <span data-ttu-id="1a012-177">Je důležité tooselect hello JDK jako prostředí JRE hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-177">It's important tooselect hello JDK as hello JRE.</span></span>
13. <span data-ttu-id="1a012-178">V okně nainstalovaná prostředí JRE, zvolte hello **JDK**.</span><span class="sxs-lookup"><span data-stu-id="1a012-178">In Installed JREs, choose hello **JDK**.</span></span> <span data-ttu-id="1a012-179">Vaše nastavení by měla vypadat podobně jako toohello následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="1a012-179">Your settings should look similar toohello following screenshot.</span></span>
    
    ![][9]
14. <span data-ttu-id="1a012-180">Volitelně vyberte **okno** > **webový prohlížeč** > **Internet Explorer** aplikace hello tooopen v okně externího prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1a012-180">Optionally, select **Window** > **Web Browser** > **Internet Explorer** tooopen hello application in an external browser window.</span></span> <span data-ttu-id="1a012-181">Použití externího prohlížeče poskytuje lepší uživatelské prostředí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a012-181">Using an external browser gives you a better Web application experience.</span></span>
    
    ![][8]

<span data-ttu-id="1a012-182">Teď jste dokončili úlohy konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-182">You have now completed hello configuration tasks.</span></span> <span data-ttu-id="1a012-183">V dalším kroku sestavíte a spusťte projekt hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-183">Next, you'll build and run hello project.</span></span>

## <a name="build-hello-project"></a><span data-ttu-id="1a012-184">Sestavení projektu hello</span><span class="sxs-lookup"><span data-stu-id="1a012-184">Build hello project</span></span>
1. <span data-ttu-id="1a012-185">V prohlížeči projektu klikněte pravým tlačítkem na název projektu hello a zvolte **spustit jako** > **sestavení Maven...**  tooconfigure hello projektu.</span><span class="sxs-lookup"><span data-stu-id="1a012-185">In Project Explorer, right-click hello project name and choose **Run As** > **Maven build...** tooconfigure hello project.</span></span>
   
    ![][10]
2. <span data-ttu-id="1a012-186">V okně Upravit konfiguraci v části Cíle zadejte „clean install“ a pak klikněte na **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="1a012-186">In Edit Configuration, in Goals, type "clean install", and then click **Run**.</span></span>

<span data-ttu-id="1a012-187">Stavové zprávy se výstup okna konzoly toohello.</span><span class="sxs-lookup"><span data-stu-id="1a012-187">Status messages are output toohello console window.</span></span> <span data-ttu-id="1a012-188">Měli byste vidět naznačuje projektu hello ÚSPĚŠNOSTI sestavení vytvořené bez chyb.</span><span class="sxs-lookup"><span data-stu-id="1a012-188">You should see BUILD SUCCESS indicating hello project built without errors.</span></span>

## <a name="run-hello-app"></a><span data-ttu-id="1a012-189">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="1a012-189">Run hello app</span></span>
<span data-ttu-id="1a012-190">V tomto posledním kroku spustíte hello aplikace v prostředí runtime místní server.</span><span class="sxs-lookup"><span data-stu-id="1a012-190">In this last step, you will run hello application in a local server runtime environment.</span></span>

<span data-ttu-id="1a012-191">Pokud jste v prostředí Eclipse ještě neurčili běhové prostředí serveru, budete potřebovat toodo nyní.</span><span class="sxs-lookup"><span data-stu-id="1a012-191">If you haven't yet specified a server runtime environment in Eclipse, you'll need toodo that first.</span></span>

1. <span data-ttu-id="1a012-192">V Prohlížeči projektu rozbalte položku **WebContent**.</span><span class="sxs-lookup"><span data-stu-id="1a012-192">In Project Explorer, expand **WebContent**.</span></span>
2. <span data-ttu-id="1a012-193">Klikněte pravým tlačítkem na **Search.jsp** > **Spustit jako** > **Spustit na serveru**.</span><span class="sxs-lookup"><span data-stu-id="1a012-193">Right-click **Search.jsp** > **Run As** > **Run on Server**.</span></span> <span data-ttu-id="1a012-194">Vyberte server Apache Tomcat hello a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="1a012-194">Select hello Apache Tomcat server, and then click **Run**.</span></span>

> [!TIP]
> <span data-ttu-id="1a012-195">Pokud jste použili toostore prostoru jiné než výchozí projekt, budete potřebovat toomodify **konfigurace spustit** toopoint toohello projektu umístění tooavoid chybě při spuštění serveru.</span><span class="sxs-lookup"><span data-stu-id="1a012-195">If you used a non-default workspace toostore your project, you'll need toomodify **Run Configuration** toopoint toohello project location tooavoid a server start-up error.</span></span> <span data-ttu-id="1a012-196">V Prohlížeči projektu klikněte pravým tlačítkem na **Search.jsp** > **Spustit jako** > **Konfigurace spuštění**.</span><span class="sxs-lookup"><span data-stu-id="1a012-196">In Project Explorer, right-click **Search.jsp** > **Run As** > **Run Configurations**.</span></span> <span data-ttu-id="1a012-197">Vyberte server Apache Tomcat hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-197">Select hello Apache Tomcat server.</span></span> <span data-ttu-id="1a012-198">Klikněte na **Argumenty**.</span><span class="sxs-lookup"><span data-stu-id="1a012-198">Click **Arguments**.</span></span> <span data-ttu-id="1a012-199">Klikněte na tlačítko **prostoru** nebo **systém souborů** tooset hello složku obsahující projekt hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-199">Click **Workspace** or **File System** tooset hello folder containing hello project.</span></span>
> 
> 

<span data-ttu-id="1a012-200">Při spuštění aplikace hello, měli byste vidět okno prohlížeče, poskytuje vyhledávací pole pro zadání termínů.</span><span class="sxs-lookup"><span data-stu-id="1a012-200">When you run hello application, you should see a browser window, providing a search box for entering terms.</span></span>

<span data-ttu-id="1a012-201">Počkejte přibližně jednu minutu před kliknutím na tlačítko **vyhledávání** toogive hello služby čas toocreate a zatížení hello index.</span><span class="sxs-lookup"><span data-stu-id="1a012-201">Wait about one minute before clicking **Search** toogive hello service time toocreate and load hello index.</span></span> <span data-ttu-id="1a012-202">Pokud se zobrazí chyba HTTP 404, bude třeba jen toowait trochu déle, než to zkusíte znovu.</span><span class="sxs-lookup"><span data-stu-id="1a012-202">If you get an HTTP 404 error, you just need toowait a little bit longer before trying again.</span></span>

## <a name="search-on-usgs-data"></a><span data-ttu-id="1a012-203">Hledání v datech USGS</span><span class="sxs-lookup"><span data-stu-id="1a012-203">Search on USGS data</span></span>
<span data-ttu-id="1a012-204">Hello sada dat USGS obsahuje záznamy, které jsou relevantní toohello státu Rhode Island.</span><span class="sxs-lookup"><span data-stu-id="1a012-204">hello USGS data set includes records that are relevant toohello state of Rhode Island.</span></span> <span data-ttu-id="1a012-205">Pokud kliknete na tlačítko **vyhledávání** na prázdného vyhledávacího pole, obdržíte prvních 50 položek hello, což je výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="1a012-205">If you click **Search** on an empty search box, you will get hello top 50 entries, which is hello default.</span></span>

<span data-ttu-id="1a012-206">Zadat hledaný termín získáte hello vyhledávacího webu něco toogo na.</span><span class="sxs-lookup"><span data-stu-id="1a012-206">Entering a search term will give hello search engine something toogo on.</span></span> <span data-ttu-id="1a012-207">Zkuste zadat místní název.</span><span class="sxs-lookup"><span data-stu-id="1a012-207">Try entering a regional name.</span></span> <span data-ttu-id="1a012-208">"Roger Williams" byla hello prvním guvernérem státu Rhode Island.</span><span class="sxs-lookup"><span data-stu-id="1a012-208">"Roger Williams" was hello first governor of Rhode Island.</span></span> <span data-ttu-id="1a012-209">Je po něm pojmenovaná celá řada parků, budov a škol.</span><span class="sxs-lookup"><span data-stu-id="1a012-209">Numerous parks, buildings, and schools are named after him.</span></span>

![][11]

<span data-ttu-id="1a012-210">Může taky zkusit kterýkoli z těchto výrazů:</span><span class="sxs-lookup"><span data-stu-id="1a012-210">You could also try any of these terms:</span></span>

* <span data-ttu-id="1a012-211">Pawtucket</span><span class="sxs-lookup"><span data-stu-id="1a012-211">Pawtucket</span></span>
* <span data-ttu-id="1a012-212">Pembroke</span><span class="sxs-lookup"><span data-stu-id="1a012-212">Pembroke</span></span>
* <span data-ttu-id="1a012-213">goose +cape</span><span class="sxs-lookup"><span data-stu-id="1a012-213">goose +cape</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a012-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a012-214">Next steps</span></span>
<span data-ttu-id="1a012-215">Toto je hello první kurz služby Azure Search založený na Javě a hello sadu dat USGS.</span><span class="sxs-lookup"><span data-stu-id="1a012-215">This is hello first Azure Search tutorial based on Java and hello USGS dataset.</span></span> <span data-ttu-id="1a012-216">V čase budete rozšiřujeme tento kurz toodemonstrate dalších vyhledávacích funkcí, které můžete chtít toouse ve vlastních řešeních.</span><span class="sxs-lookup"><span data-stu-id="1a012-216">Over time, we'll extend this tutorial toodemonstrate additional search features you might want toouse in your custom solutions.</span></span>

<span data-ttu-id="1a012-217">Pokud již máte základní ve službě Azure Search, můžete tuto ukázku jako odrazový můstek pro další experimentování, případně rozšířit hello [stránky hledání](search-pagination-page-layout.md), nebo implementaci [Fasetové navigace](search-faceted-navigation.md).</span><span class="sxs-lookup"><span data-stu-id="1a012-217">If you already have some background in Azure Search, you can use this sample as a springboard for further experimentation, perhaps augmenting hello [search page](search-pagination-page-layout.md), or implementing [faceted navigation](search-faceted-navigation.md).</span></span> <span data-ttu-id="1a012-218">Můžete taky zdokonalit stránku výsledků hledání hello přidáním počtů a dávkování dokumenty tak, aby hello výsledky procházet po stránkách.</span><span class="sxs-lookup"><span data-stu-id="1a012-218">You can also improve upon hello search results page by adding counts and batching documents so that users can page through hello results.</span></span>

<span data-ttu-id="1a012-219">Nové tooAzure hledání?</span><span class="sxs-lookup"><span data-stu-id="1a012-219">New tooAzure Search?</span></span> <span data-ttu-id="1a012-220">Doporučujeme vyzkoušet ostatní kurzy toodevelop představu o co můžete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="1a012-220">We recommend trying other tutorials toodevelop an understanding of what you can create.</span></span> <span data-ttu-id="1a012-221">Navštivte naše [stránky dokumentace, která](https://azure.microsoft.com/documentation/services/search/) toofind více prostředků.</span><span class="sxs-lookup"><span data-stu-id="1a012-221">Visit our [documentation page](https://azure.microsoft.com/documentation/services/search/) toofind more resources.</span></span> <span data-ttu-id="1a012-222">Můžete také zobrazit hello odkazy v našem [seznamu videí a kurzů](search-video-demo-tutorial-list.md) tooaccess Další informace.</span><span class="sxs-lookup"><span data-stu-id="1a012-222">You can also view hello links in our [Video and Tutorial list](search-video-demo-tutorial-list.md) tooaccess more information.</span></span>

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
