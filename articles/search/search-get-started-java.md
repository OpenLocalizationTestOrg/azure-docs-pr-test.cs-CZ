---
title: "Začínáme s Azure Search v Javě | Dokumentace Microsoftu"
description: "Jak vytvořit hostovanou cloudovou vyhledávací aplikaci v Azure pomocí programovacího jazyka Java."
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
ms.openlocfilehash: f6ca06a0349def97b38a1bf6d0d8f36236077e92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-search-in-java"></a><span data-ttu-id="14ff4-103">Začínáme s Azure Search v Javě</span><span class="sxs-lookup"><span data-stu-id="14ff4-103">Get started with Azure Search in Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="14ff4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="14ff4-104">Portal</span></span>](search-get-started-portal.md)
> * [<span data-ttu-id="14ff4-105">.NET</span><span class="sxs-lookup"><span data-stu-id="14ff4-105">.NET</span></span>](search-howto-dotnet-sdk.md)
> 
> 

<span data-ttu-id="14ff4-106">Naučte se sestavit vlastní vyhledávací aplikaci Java, která k hledání používá službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="14ff4-106">Learn how to build a custom Java search application that uses Azure Search for its search experience.</span></span> <span data-ttu-id="14ff4-107">V tomto kurzu se pomocí [rozhraní REST API služby Azure Search](https://msdn.microsoft.com/library/dn798935.aspx) vytvoří objekty a operace, které se použijí v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="14ff4-107">This tutorial uses the [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) to construct the objects and operations used in this exercise.</span></span>

<span data-ttu-id="14ff4-108">Pokud chcete tuto ukázku spustit, musíte mít službu Azure Search, ke které se můžete zaregistrovat na webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="14ff4-108">To run this sample, you must have an Azure Search service, which you can sign up for in the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="14ff4-109">Podrobné pokyny najdete v tématu [Vytvoření služby Azure Search na portálu](search-create-service-portal.md).</span><span class="sxs-lookup"><span data-stu-id="14ff4-109">See [Create an Azure Search service in the portal](search-create-service-portal.md) for step-by-step instructions.</span></span>

<span data-ttu-id="14ff4-110">Pro vytvoření a testování tohoto příkladu jsme použili následující software:</span><span class="sxs-lookup"><span data-stu-id="14ff4-110">We used the following software to build and test this sample:</span></span>

* <span data-ttu-id="14ff4-111">[Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar)</span><span class="sxs-lookup"><span data-stu-id="14ff4-111">[Eclipse IDE for Java EE Developers](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar).</span></span> <span data-ttu-id="14ff4-112">Ujistěte se, že jste stáhli verzi EE.</span><span class="sxs-lookup"><span data-stu-id="14ff4-112">Be sure to download the EE version.</span></span> <span data-ttu-id="14ff4-113">Jeden z kroků ověření vyžaduje funkci, která se nachází pouze v této edici.</span><span class="sxs-lookup"><span data-stu-id="14ff4-113">One of the verification steps requires a feature that is found only in this edition.</span></span>
* [<span data-ttu-id="14ff4-114">JDK 8u40</span><span class="sxs-lookup"><span data-stu-id="14ff4-114">JDK 8u40</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [<span data-ttu-id="14ff4-115">Apache Tomcat 8.0</span><span class="sxs-lookup"><span data-stu-id="14ff4-115">Apache Tomcat 8.0</span></span>](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a><span data-ttu-id="14ff4-116">Informace o datech</span><span class="sxs-lookup"><span data-stu-id="14ff4-116">About the data</span></span>
<span data-ttu-id="14ff4-117">Tato ukázková aplikace používá data agentury [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), která jsou filtrovaná pro stát Rhode Island, aby se zmenšila velikost datové sady.</span><span class="sxs-lookup"><span data-stu-id="14ff4-117">This sample application uses data from the [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtered on the state of Rhode Island to reduce the dataset size.</span></span> <span data-ttu-id="14ff4-118">Pomocí těchto dat sestavíme vyhledávací aplikaci, která najde významné budovy, například nemocnice a školy, a geologické prvky, jako jsou vodní toky, jezera a vrcholy.</span><span class="sxs-lookup"><span data-stu-id="14ff4-118">We'll use this data to build a search application that returns landmark buildings such as hospitals and schools, as well as geological features like streams, lakes, and summits.</span></span>

<span data-ttu-id="14ff4-119">Program **SearchServlet.java** v této aplikaci sestaví a načte index pomocí konstruktoru [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx), přičemž načte filtrovanou sadu dat USGS z veřejné databáze Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="14ff4-119">In this application, the **SearchServlet.java** program builds and loads the index using an [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) construct, retrieving the filtered USGS dataset from a public Azure SQL Database.</span></span> <span data-ttu-id="14ff4-120">Předdefinované přihlašovací údaje a informace o připojení k online zdroji dat jsou uvedené v kódu programu.</span><span class="sxs-lookup"><span data-stu-id="14ff4-120">Predefined credentials and connection  information to the online data source are provided in the program code.</span></span> <span data-ttu-id="14ff4-121">Z hlediska přístupu k datům není potřeba žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="14ff4-121">In terms of data access, no further configuration is necessary.</span></span>

> [!NOTE]
> <span data-ttu-id="14ff4-122">U této sady dat jsme použili filtr, abychom dodrželi omezení 10 000 dokumentů pro cenovou úroveň Free.</span><span class="sxs-lookup"><span data-stu-id="14ff4-122">We applied a filter on this dataset to stay under the 10,000 document limit of the free pricing tier.</span></span> <span data-ttu-id="14ff4-123">Pokud používáte úroveň Standard, toto omezení se na vás nevztahuje a můžete upravit tento kód, aby používal větší datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="14ff4-123">If you use the standard tier, this limit does not apply, and you can modify this code to use a bigger dataset.</span></span> <span data-ttu-id="14ff4-124">Podrobnosti týkající se kapacity u jednotlivých cenových úrovní najdete v tématu [Omezení](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="14ff4-124">For details about capacity for each pricing tier, see [Limits and constraints](search-limits-quotas-capacity.md).</span></span>
> 
> 

## <a name="about-the-program-files"></a><span data-ttu-id="14ff4-125">O souborech programu</span><span class="sxs-lookup"><span data-stu-id="14ff4-125">About the program files</span></span>
<span data-ttu-id="14ff4-126">Následující seznam popisuje soubory, které se vztahují k tomuto příkladu.</span><span class="sxs-lookup"><span data-stu-id="14ff4-126">The following list describes the files that are relevant to this sample.</span></span>

* <span data-ttu-id="14ff4-127">Search.jsp: Poskytuje uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="14ff4-127">Search.jsp: Provides the user interface</span></span>
* <span data-ttu-id="14ff4-128">SearchServlet.java: Poskytuje metody (podobně jako řadič v MVC).</span><span class="sxs-lookup"><span data-stu-id="14ff4-128">SearchServlet.java: Provides methods (similar to a controller in MVC)</span></span>
* <span data-ttu-id="14ff4-129">SearchServiceClient.java: Zpracovává požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="14ff4-129">SearchServiceClient.java: Handles HTTP requests</span></span>
* <span data-ttu-id="14ff4-130">SearchServiceHelper.java: Pomocná třída, která poskytuje statické metody.</span><span class="sxs-lookup"><span data-stu-id="14ff4-130">SearchServiceHelper.java: A helper class that provides static methods</span></span>
* <span data-ttu-id="14ff4-131">Document.java: Poskytuje datový model.</span><span class="sxs-lookup"><span data-stu-id="14ff4-131">Document.java: Provides the data model</span></span>
* <span data-ttu-id="14ff4-132">config.properties: Nastavuje adresu URL služby Search a klíč api-key.</span><span class="sxs-lookup"><span data-stu-id="14ff4-132">config.properties: Sets the Search service URL and api-key</span></span>
* <span data-ttu-id="14ff4-133">Pom.xml: Závislost Maven</span><span class="sxs-lookup"><span data-stu-id="14ff4-133">Pom.xml: A Maven dependency</span></span>

<a id="sub-2"></a>

## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a><span data-ttu-id="14ff4-134">Nalezení názvu služby a klíče api-key služby Azure Search</span><span class="sxs-lookup"><span data-stu-id="14ff4-134">Find the service name and api-key of your Azure Search service</span></span>
<span data-ttu-id="14ff4-135">Všechna volání rozhraní API REST služby Azure Search vyžadují, abyste zadali adresu URL služby a klíč api-key.</span><span class="sxs-lookup"><span data-stu-id="14ff4-135">All REST API calls into Azure Search require that you provide the service URL and an api-key.</span></span> 

1. <span data-ttu-id="14ff4-136">Přihlaste se k [Portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="14ff4-136">Sign in to the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="14ff4-137">Na panelu odkazů klikněte na **Služba Search** a zobrazte výpis všech služeb Azure Search zřízených pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="14ff4-137">In the jump bar, click **Search service** to list all of the Azure Search services provisioned for your subscription.</span></span>
3. <span data-ttu-id="14ff4-138">Vyberte službu, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="14ff4-138">Select the service you want to use.</span></span>
4. <span data-ttu-id="14ff4-139">Na řídicím panelu služby uvidíte dlaždice se základními informacemi a ikonu klíče pro přístup ke klíčům správce.</span><span class="sxs-lookup"><span data-stu-id="14ff4-139">On the service dashboard, you'll see tiles for essential information as well as the key icon for accessing the admin keys.</span></span>
   
      ![][3]
5. <span data-ttu-id="14ff4-140">Zkopírujte adresu URL služby a klíč správce.</span><span class="sxs-lookup"><span data-stu-id="14ff4-140">Copy the service URL and an admin key.</span></span> <span data-ttu-id="14ff4-141">Budete je potřebovat později, až je budete přidávat do souboru **config.properties**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-141">You will need them later, when you add them to the **config.properties** file.</span></span>

## <a name="download-the-sample-files"></a><span data-ttu-id="14ff4-142">Stažení ukázkových souborů</span><span class="sxs-lookup"><span data-stu-id="14ff4-142">Download the sample files</span></span>
1. <span data-ttu-id="14ff4-143">Přejděte na [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="14ff4-143">Go to [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) on GitHub.</span></span>
2. <span data-ttu-id="14ff4-144">Klikněte na **Stáhnout ZIP**, uložte soubor .zip na disk a potom z něj extrahujte všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="14ff4-144">Click **Download ZIP**, save the .zip file to disk, and then extract all the files it contains.</span></span> <span data-ttu-id="14ff4-145">Zvažte extrahování souborů do pracovního prostoru Java, aby bylo později snazší projekt najít.</span><span class="sxs-lookup"><span data-stu-id="14ff4-145">Consider extracting the files into your Java workspace to make it easier to find the project later.</span></span>
3. <span data-ttu-id="14ff4-146">Ukázkové soubory jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="14ff4-146">The sample files are read-only.</span></span> <span data-ttu-id="14ff4-147">Klikněte pravým tlačítkem na vlastnosti složky a vymažte atribut jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="14ff4-147">Right-click folder properties and clear the read-only attribute.</span></span>

<span data-ttu-id="14ff4-148">Všechny následné úpravy souborů a spouštěné příkazy se budou provádět na souborech v této složce.</span><span class="sxs-lookup"><span data-stu-id="14ff4-148">All subsequent file modifications and run statements will be made against files in this folder.</span></span>  

## <a name="import-project"></a><span data-ttu-id="14ff4-149">Import projektu</span><span class="sxs-lookup"><span data-stu-id="14ff4-149">Import project</span></span>
1. <span data-ttu-id="14ff4-150">V prostředí Eclipse zvolte **Soubor** > **Import** > **Obecné** > **Existující projekty do pracovního prostoru**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-150">In Eclipse, choose **File** > **Import** > **General** > **Existing Projects into Workspace**.</span></span>
   
    ![][4]
2. <span data-ttu-id="14ff4-151">V okně **Vybrat kořenový adresář** přejděte do složky obsahující ukázkové soubory.</span><span class="sxs-lookup"><span data-stu-id="14ff4-151">In **Select root directory**, browse to the folder containing sample files.</span></span> <span data-ttu-id="14ff4-152">Vyberte složku, která obsahuje složku .project.</span><span class="sxs-lookup"><span data-stu-id="14ff4-152">Select the folder that contains the .project folder.</span></span> <span data-ttu-id="14ff4-153">Projekt by se měl zobrazit v seznamu **Projekty** jako vybraná položka.</span><span class="sxs-lookup"><span data-stu-id="14ff4-153">The project should appear in the **Projects** list as a selected item.</span></span>
   
    ![][12]
3. <span data-ttu-id="14ff4-154">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-154">Click **Finish**.</span></span>
4. <span data-ttu-id="14ff4-155">Pomocí **Prohlížeče projektu** můžete zobrazit a upravit soubory.</span><span class="sxs-lookup"><span data-stu-id="14ff4-155">Use **Project Explorer** to view and edit the files.</span></span> <span data-ttu-id="14ff4-156">Pokud ještě není otevřený, klikněte na **Okno** > **Zobrazit zobrazení** > **Prohlížeč projektu** nebo ho otevřete pomocí klávesové zkratky.</span><span class="sxs-lookup"><span data-stu-id="14ff4-156">If it's not already open, click **Window** > **Show View** > **Project Explorer** or use the shortcut to open it.</span></span>

## <a name="configure-the-service-url-and-api-key"></a><span data-ttu-id="14ff4-157">Konfigurace adresy URL služby a klíče api-key</span><span class="sxs-lookup"><span data-stu-id="14ff4-157">Configure the service URL and api-key</span></span>
1. <span data-ttu-id="14ff4-158">V **Prohlížeči projektu**, dvakrát klikněte na soubor **config.properties**, abyste upravili nastavení konfigurace obsahující název serveru a klíč api-key.</span><span class="sxs-lookup"><span data-stu-id="14ff4-158">In **Project Explorer**, double-click **config.properties** to edit the configuration settings containing the server name and api-key.</span></span>
2. <span data-ttu-id="14ff4-159">Podívejte se na postup uvedený dříve v tomto článku, kde jste našli adresu URL služby a klíč api-key v [webu Azure Portal](https://portal.azure.com), abyste získali hodnoty, které nyní zadáte do souboru **config.properties**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-159">Refer to the steps earlier in this article, where you found the service URL and api-key in the [Azure Portal](https://portal.azure.com), to get the values you will now enter into **config.properties**.</span></span>
3. <span data-ttu-id="14ff4-160">V souboru **config.properties** nahraďte položku „Api Key“ klíčem api-key pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="14ff4-160">In **config.properties**, replace "Api Key" with the api-key for your service.</span></span> <span data-ttu-id="14ff4-161">Název služby (první komponenta adresy URL http://název_služby.search.windows.net) dále nahrazuje položku „service name“ ve stejném souboru.</span><span class="sxs-lookup"><span data-stu-id="14ff4-161">Next, service name (the first component of the URL http://servicename.search.windows.net) replaces "service name" in the same file.</span></span>
   
    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a><span data-ttu-id="14ff4-162">Konfigurace prostředí projektu, buildu a běhového prostředí</span><span class="sxs-lookup"><span data-stu-id="14ff4-162">Configure the project, build and runtime environments</span></span>
1. <span data-ttu-id="14ff4-163">V prostředí Eclipse v Prohlížeči projektu klikněte pravým tlačítkem myši na projekt > **Vlastnosti** > **Omezující vlastnosti projektu**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-163">In Eclipse, in Project Explorer, right-click the project > **Properties** > **Project Facets**.</span></span>
2. <span data-ttu-id="14ff4-164">Vyberte **Dynamic Web Module**, **Java** a **JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-164">Select **Dynamic Web Module**, **Java**, and **JavaScript**.</span></span>
   
    ![][6]
3. <span data-ttu-id="14ff4-165">Klikněte na **Použít**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-165">Click **Apply**.</span></span>
4. <span data-ttu-id="14ff4-166">Vyberte **Okno** > **Předvolby** > **Server** > **Běhová prostředí** > **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-166">Select **Window** > **Preferences** > **Server** > **Runtime Environments** > **Add..**.</span></span>
5. <span data-ttu-id="14ff4-167">Rozbalte položku Apache a vyberte verzi serveru Apache Tomcat, kterou jste dříve nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="14ff4-167">Expand Apache and select the version of the Apache Tomcat server you previously installed.</span></span> <span data-ttu-id="14ff4-168">V našem systému jsme nainstalovali verzi 8.</span><span class="sxs-lookup"><span data-stu-id="14ff4-168">On our system, we installed version 8.</span></span>
   
    ![][7]
6. <span data-ttu-id="14ff4-169">Na další stránce zadejte instalační adresář Tomcat.</span><span class="sxs-lookup"><span data-stu-id="14ff4-169">On the next page, specify the Tomcat installation directory.</span></span> <span data-ttu-id="14ff4-170">V počítači s Windows to bude pravděpodobně C:\Program Files\Apache Software Foundation\Tomcat *verze*.</span><span class="sxs-lookup"><span data-stu-id="14ff4-170">On a Windows computer, this will most likely be C:\Program Files\Apache Software Foundation\Tomcat *version*.</span></span>
7. <span data-ttu-id="14ff4-171">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-171">Click **Finish**.</span></span>
8. <span data-ttu-id="14ff4-172">Vyberte **Okno** > **Předvolby** > **Java** > **Nainstalovaná prostředí JRE** > **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-172">Select **Window** > **Preferences** > **Java** > **Installed JREs** > **Add**.</span></span>
9. <span data-ttu-id="14ff4-173">V okně **Přidat prostředí JRE**, vyberte **Standardní virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-173">In **Add JRE**, select **Standard VM**.</span></span>
10. <span data-ttu-id="14ff4-174">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-174">Click **Next**.</span></span>
11. <span data-ttu-id="14ff4-175">V definici prostředí JRE v kořenovém adresáři JRE klikněte na **Adresář**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-175">In JRE Definition, in JRE home, click **Directory**.</span></span>
12. <span data-ttu-id="14ff4-176">Přejděte do adresáře **Program Files** > **Java** a vyberte sadu JDK, kterou jste dříve nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="14ff4-176">Navigate to **Program Files** > **Java** and select the JDK you previously installed.</span></span> <span data-ttu-id="14ff4-177">Je důležité vybrat jako prostředí JRE sadu JDK.</span><span class="sxs-lookup"><span data-stu-id="14ff4-177">It's important to select the JDK as the JRE.</span></span>
13. <span data-ttu-id="14ff4-178">V okně Nainstalovaná prostředí JRE zvolte **JDK**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-178">In Installed JREs, choose the **JDK**.</span></span> <span data-ttu-id="14ff4-179">Vaše nastavení by mělo vypadat jako na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="14ff4-179">Your settings should look similar to the following screenshot.</span></span>
    
    ![][9]
14. <span data-ttu-id="14ff4-180">Volitelně vyberte **Okno** > **Webový prohlížeč** > **Internet Explorer**, aby se aplikaci spustila v okně externího prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="14ff4-180">Optionally, select **Window** > **Web Browser** > **Internet Explorer** to open the application in an external browser window.</span></span> <span data-ttu-id="14ff4-181">Použití externího prohlížeče poskytuje lepší uživatelské prostředí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="14ff4-181">Using an external browser gives you a better Web application experience.</span></span>
    
    ![][8]

<span data-ttu-id="14ff4-182">Nyní jste dokončili úlohy konfigurace.</span><span class="sxs-lookup"><span data-stu-id="14ff4-182">You have now completed the configuration tasks.</span></span> <span data-ttu-id="14ff4-183">V dalším kroku sestavíte a spustíte projekt.</span><span class="sxs-lookup"><span data-stu-id="14ff4-183">Next, you'll build and run the project.</span></span>

## <a name="build-the-project"></a><span data-ttu-id="14ff4-184">Sestavení projektu</span><span class="sxs-lookup"><span data-stu-id="14ff4-184">Build the project</span></span>
1. <span data-ttu-id="14ff4-185">V Prohlížeči projektu klikněte pravým tlačítkem na název projektu a zvolte **Spustit jako** > **Build Maven...**, abyste nakonfigurovali projekt.</span><span class="sxs-lookup"><span data-stu-id="14ff4-185">In Project Explorer, right-click the project name and choose **Run As** > **Maven build...** to configure the project.</span></span>
   
    ![][10]
2. <span data-ttu-id="14ff4-186">V okně Upravit konfiguraci v části Cíle zadejte „clean install“ a pak klikněte na **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-186">In Edit Configuration, in Goals, type "clean install", and then click **Run**.</span></span>

<span data-ttu-id="14ff4-187">Stavové zprávy se zobrazují v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="14ff4-187">Status messages are output to the console window.</span></span> <span data-ttu-id="14ff4-188">Měli byste vidět zprávu o úspěšném sestavení, která oznamuje sestavení projektu bez chyb.</span><span class="sxs-lookup"><span data-stu-id="14ff4-188">You should see BUILD SUCCESS indicating the project built without errors.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="14ff4-189">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="14ff4-189">Run the app</span></span>
<span data-ttu-id="14ff4-190">V tomto posledním kroku spustíte aplikaci v běhovém prostředí místního serveru.</span><span class="sxs-lookup"><span data-stu-id="14ff4-190">In this last step, you will run the application in a local server runtime environment.</span></span>

<span data-ttu-id="14ff4-191">Pokud jste v prostředí Eclipse ještě neurčili běhové prostředí serveru, budete to muset učinit nyní.</span><span class="sxs-lookup"><span data-stu-id="14ff4-191">If you haven't yet specified a server runtime environment in Eclipse, you'll need to do that first.</span></span>

1. <span data-ttu-id="14ff4-192">V Prohlížeči projektu rozbalte položku **WebContent**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-192">In Project Explorer, expand **WebContent**.</span></span>
2. <span data-ttu-id="14ff4-193">Klikněte pravým tlačítkem na **Search.jsp** > **Spustit jako** > **Spustit na serveru**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-193">Right-click **Search.jsp** > **Run As** > **Run on Server**.</span></span> <span data-ttu-id="14ff4-194">Vyberte server Apache Tomcat a potom klikněte na **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-194">Select the Apache Tomcat server, and then click **Run**.</span></span>

> [!TIP]
> <span data-ttu-id="14ff4-195">Pokud jste k uložení projektu použili jiný pracovní prostor než výchozí, budete muset upravit **Konfiguraci spuštění**, aby odkazovala na umístění projektu a nedošlo k chybě při spuštění serveru.</span><span class="sxs-lookup"><span data-stu-id="14ff4-195">If you used a non-default workspace to store your project, you'll need to modify **Run Configuration** to point to the project location to avoid a server start-up error.</span></span> <span data-ttu-id="14ff4-196">V Prohlížeči projektu klikněte pravým tlačítkem na **Search.jsp** > **Spustit jako** > **Konfigurace spuštění**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-196">In Project Explorer, right-click **Search.jsp** > **Run As** > **Run Configurations**.</span></span> <span data-ttu-id="14ff4-197">Vyberte server Apache Tomcat.</span><span class="sxs-lookup"><span data-stu-id="14ff4-197">Select the Apache Tomcat server.</span></span> <span data-ttu-id="14ff4-198">Klikněte na **Argumenty**.</span><span class="sxs-lookup"><span data-stu-id="14ff4-198">Click **Arguments**.</span></span> <span data-ttu-id="14ff4-199">Klikněte na **Pracovní prostor** nebo **Systém souborů**, abyste nastavili složku obsahující projekt.</span><span class="sxs-lookup"><span data-stu-id="14ff4-199">Click **Workspace** or **File System** to set the folder containing the project.</span></span>
> 
> 

<span data-ttu-id="14ff4-200">Při spuštění aplikace by se mělo zobrazit okno prohlížeče, které poskytuje vyhledávací pole pro zadání termínů.</span><span class="sxs-lookup"><span data-stu-id="14ff4-200">When you run the application, you should see a browser window, providing a search box for entering terms.</span></span>

<span data-ttu-id="14ff4-201">Před kliknutím na tlačítko **Hledat** počkejte přibližně jednu minutu, aby měla služba čas na vytvoření a načtení indexu.</span><span class="sxs-lookup"><span data-stu-id="14ff4-201">Wait about one minute before clicking **Search** to give the service time to create and load the index.</span></span> <span data-ttu-id="14ff4-202">Pokud se zobrazí chyba HTTP 404, bude třeba před dalším pokusem ještě chvíli počkat.</span><span class="sxs-lookup"><span data-stu-id="14ff4-202">If you get an HTTP 404 error, you just need to wait a little bit longer before trying again.</span></span>

## <a name="search-on-usgs-data"></a><span data-ttu-id="14ff4-203">Hledání v datech USGS</span><span class="sxs-lookup"><span data-stu-id="14ff4-203">Search on USGS data</span></span>
<span data-ttu-id="14ff4-204">Sada dat USGS obsahuje záznamy, které se vztahují ke státu Rhode Island.</span><span class="sxs-lookup"><span data-stu-id="14ff4-204">The USGS data set includes records that are relevant to the state of Rhode Island.</span></span> <span data-ttu-id="14ff4-205">Pokud u prázdného vyhledávacího pole kliknete na tlačítko **Hledat**, obdržíte prvních 50 položek, což je výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="14ff4-205">If you click **Search** on an empty search box, you will get the top 50 entries, which is the default.</span></span>

<span data-ttu-id="14ff4-206">Když zadáte hledaný výraz, vyhledávací web bude mít s čím pracovat.</span><span class="sxs-lookup"><span data-stu-id="14ff4-206">Entering a search term will give the search engine something to go on.</span></span> <span data-ttu-id="14ff4-207">Zkuste zadat místní název.</span><span class="sxs-lookup"><span data-stu-id="14ff4-207">Try entering a regional name.</span></span> <span data-ttu-id="14ff4-208">Roger Williams byl prvním guvernérem státu Rhode Island.</span><span class="sxs-lookup"><span data-stu-id="14ff4-208">"Roger Williams" was the first governor of Rhode Island.</span></span> <span data-ttu-id="14ff4-209">Je po něm pojmenovaná celá řada parků, budov a škol.</span><span class="sxs-lookup"><span data-stu-id="14ff4-209">Numerous parks, buildings, and schools are named after him.</span></span>

![][11]

<span data-ttu-id="14ff4-210">Může taky zkusit kterýkoli z těchto výrazů:</span><span class="sxs-lookup"><span data-stu-id="14ff4-210">You could also try any of these terms:</span></span>

* <span data-ttu-id="14ff4-211">Pawtucket</span><span class="sxs-lookup"><span data-stu-id="14ff4-211">Pawtucket</span></span>
* <span data-ttu-id="14ff4-212">Pembroke</span><span class="sxs-lookup"><span data-stu-id="14ff4-212">Pembroke</span></span>
* <span data-ttu-id="14ff4-213">goose +cape</span><span class="sxs-lookup"><span data-stu-id="14ff4-213">goose +cape</span></span>

## <a name="next-steps"></a><span data-ttu-id="14ff4-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14ff4-214">Next steps</span></span>
<span data-ttu-id="14ff4-215">Toto je první kurz služby Azure Search založený na Javě a sadě dat USGS.</span><span class="sxs-lookup"><span data-stu-id="14ff4-215">This is the first Azure Search tutorial based on Java and the USGS dataset.</span></span> <span data-ttu-id="14ff4-216">Postupně ho budeme rozšiřovat o ukázky dalších vyhledávacích funkcí, které by se vám ve vlastních řešeních mohly hodit.</span><span class="sxs-lookup"><span data-stu-id="14ff4-216">Over time, we'll extend this tutorial to demonstrate additional search features you might want to use in your custom solutions.</span></span>

<span data-ttu-id="14ff4-217">Pokud již máte základní vědomosti o službě Azure Search, můžete tuto ukázku použít jako odrazový můstek pro další experimentování, případně rozšíření [stránky vyhledávání](search-pagination-page-layout.md) nebo implementaci [fasetové navigace](search-faceted-navigation.md).</span><span class="sxs-lookup"><span data-stu-id="14ff4-217">If you already have some background in Azure Search, you can use this sample as a springboard for further experimentation, perhaps augmenting the [search page](search-pagination-page-layout.md), or implementing [faceted navigation](search-faceted-navigation.md).</span></span> <span data-ttu-id="14ff4-218">Můžete taky zdokonalit stránku výsledků hledání přidáním počtů a dávkováním dokumentů, aby se výsledky daly procházet po stránkách.</span><span class="sxs-lookup"><span data-stu-id="14ff4-218">You can also improve upon the search results page by adding counts and batching documents so that users can page through the results.</span></span>

<span data-ttu-id="14ff4-219">Jste nováčky ve službě Azure Search?</span><span class="sxs-lookup"><span data-stu-id="14ff4-219">New to Azure Search?</span></span> <span data-ttu-id="14ff4-220">Doporučujeme vyzkoušet ostatní kurzy a vytvořit si představu o tom, co se dá vytvořit.</span><span class="sxs-lookup"><span data-stu-id="14ff4-220">We recommend trying other tutorials to develop an understanding of what you can create.</span></span> <span data-ttu-id="14ff4-221">Pokud hledáte další zdroje, přejděte na [stránku dokumentace](https://azure.microsoft.com/documentation/services/search/).</span><span class="sxs-lookup"><span data-stu-id="14ff4-221">Visit our [documentation page](https://azure.microsoft.com/documentation/services/search/) to find more resources.</span></span> <span data-ttu-id="14ff4-222">Přístup k dalším informacím vám poskytnou taky odkazy v [Seznamu videí a kurzů](search-video-demo-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="14ff4-222">You can also view the links in our [Video and Tutorial list](search-video-demo-tutorial-list.md) to access more information.</span></span>

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
