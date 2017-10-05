---
title: "Začínáme se službou Azure Search v Node.js | Dokumentace Microsoftu"
description: "Projděte si sestavení vyhledávací aplikace v hostované cloudové vyhledávací službě v Azure pomocí programovacího jazyka Node.js."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 0625dc1b-9db6-40d5-ba9a-4738b75cbe19
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: evboyle
ms.openlocfilehash: 32865ed986f5eea961ef2c3813dcc6531498c90a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-search-in-nodejs"></a><span data-ttu-id="7bebc-103">Začínáme se službou Azure Search v Node.js</span><span class="sxs-lookup"><span data-stu-id="7bebc-103">Get started with Azure Search in Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7bebc-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7bebc-104">Portal</span></span>](search-get-started-portal.md)
> * [<span data-ttu-id="7bebc-105">.NET</span><span class="sxs-lookup"><span data-stu-id="7bebc-105">.NET</span></span>](search-howto-dotnet-sdk.md)
> 
> 

<span data-ttu-id="7bebc-106">Naučte se sestavit vlastní vyhledávací aplikaci v Node.js, která k hledání používá službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="7bebc-106">Learn how to build a custom Node.js search application that uses Azure Search for its search experience.</span></span> <span data-ttu-id="7bebc-107">V tomto kurzu se pomocí [rozhraní REST API služby Azure Search](https://msdn.microsoft.com/library/dn798935.aspx) vytvoří objekty a operace, které se použijí v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="7bebc-107">This tutorial uses the [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) to construct the objects and operations used in this exercise.</span></span>

<span data-ttu-id="7bebc-108">Tento kód jsme vyvinuli a testovali pomocí [Node.js](https://Nodejs.org) a NPM, [Sublime Text 3](http://www.sublimetext.com/3) a Windows PowerShellu v systému Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="7bebc-108">We used [Node.js](https://Nodejs.org) and NPM, [Sublime Text 3](http://www.sublimetext.com/3), and Windows PowerShell on Windows 8.1 to develop and test this code.</span></span>

<span data-ttu-id="7bebc-109">Pokud chcete tuto ukázku spustit, musíte mít službu Azure Search, ke které se můžete zaregistrovat na webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7bebc-109">To run this sample, you must have an Azure Search service, which you can sign up for in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="7bebc-110">Podrobné pokyny najdete v tématu [Vytvoření služby Azure Search na portálu](search-create-service-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7bebc-110">See [Create an Azure Search service in the portal](search-create-service-portal.md) for step-by-step instructions.</span></span>

## <a name="about-the-data"></a><span data-ttu-id="7bebc-111">Informace o datech</span><span class="sxs-lookup"><span data-stu-id="7bebc-111">About the data</span></span>
<span data-ttu-id="7bebc-112">Tato ukázková aplikace používá data agentury [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), která jsou filtrovaná pro stát Rhode Island, aby se zmenšila velikost datové sady.</span><span class="sxs-lookup"><span data-stu-id="7bebc-112">This sample application uses data from the [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtered on the state of Rhode Island to reduce the dataset size.</span></span> <span data-ttu-id="7bebc-113">Pomocí těchto dat sestavíme vyhledávací aplikaci, která najde významné budovy, například nemocnice a školy, a geologické prvky, jako jsou vodní toky, jezera a vrcholy.</span><span class="sxs-lookup"><span data-stu-id="7bebc-113">We'll use this data to build a search application that returns landmark buildings such as hospitals and schools, as well as geological features like streams, lakes, and summits.</span></span>

<span data-ttu-id="7bebc-114">Program **DataIndexer** v této aplikaci sestaví a načte index pomocí konstruktoru [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx), přičemž načte filtrovanou sadu dat USGS z veřejné databáze Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="7bebc-114">In this application, the **DataIndexer** program builds and loads the index using an [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) construct, retrieving the filtered USGS dataset from a public Azure SQL Database.</span></span> <span data-ttu-id="7bebc-115">Přihlašovací údaje a informace o připojení k online zdroji dat jsou uvedené v kódu programu.</span><span class="sxs-lookup"><span data-stu-id="7bebc-115">Credentials and connection information to the online data source is provided in the program code.</span></span> <span data-ttu-id="7bebc-116">Není potřeba žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7bebc-116">No further configuration is necessary.</span></span>

> [!NOTE]
> <span data-ttu-id="7bebc-117">U této sady dat jsme použili filtr, abychom dodrželi omezení 10 000 dokumentů pro cenovou úroveň Free.</span><span class="sxs-lookup"><span data-stu-id="7bebc-117">We applied a filter on this dataset to stay under the 10,000 document limit of the free pricing tier.</span></span> <span data-ttu-id="7bebc-118">Pokud používáte úroveň Standard, toto omezení neplatí.</span><span class="sxs-lookup"><span data-stu-id="7bebc-118">If you use the standard tier, this limit does not apply.</span></span> <span data-ttu-id="7bebc-119">Podrobnosti týkající se kapacity u jednotlivých cenových úrovní najdete v tématu [Omezení služby Search](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="7bebc-119">For details about capacity for each pricing tier, see [Search service limits](search-limits-quotas-capacity.md).</span></span>
> 
> 

<a id="sub-2"></a>

## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a><span data-ttu-id="7bebc-120">Nalezení názvu služby a klíče API služby Azure Search</span><span class="sxs-lookup"><span data-stu-id="7bebc-120">Find the service name and api-key of your Azure Search service</span></span>
<span data-ttu-id="7bebc-121">Po vytvoření služby se vraťte na portál a získejte adresu URL nebo `api-key`.</span><span class="sxs-lookup"><span data-stu-id="7bebc-121">After you create the service, return to the portal to get the URL or `api-key`.</span></span> <span data-ttu-id="7bebc-122">Připojení k službě Search vyžadují, abyste k ověření volání měli adresu URL i `api-key`.</span><span class="sxs-lookup"><span data-stu-id="7bebc-122">Connections to your Search service require that you have both the URL and an `api-key` to authenticate the call.</span></span>

1. <span data-ttu-id="7bebc-123">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7bebc-123">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7bebc-124">Na panelu odkazů klikněte na **Služba Search** a zobrazte výpis všech služeb Azure Search zřízených pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="7bebc-124">In the jump bar, click **Search service** to list all Azure Search services provisioned for your subscription.</span></span>
3. <span data-ttu-id="7bebc-125">Vyberte službu, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="7bebc-125">Select the service you want to use.</span></span>
4. <span data-ttu-id="7bebc-126">Na řídicím panelu služby byste měli vidět dlaždice se základními informacemi, jako například ikonu klíče pro přístup ke klíčům správce.</span><span class="sxs-lookup"><span data-stu-id="7bebc-126">On the service dashboard, you should see tiles for essential information, such as the key icon for accessing the admin keys.</span></span>
5. <span data-ttu-id="7bebc-127">Zkopírujte adresu URL služby, klíč správce a klíč dotazu.</span><span class="sxs-lookup"><span data-stu-id="7bebc-127">Copy the service URL, an admin key, and a query key.</span></span> <span data-ttu-id="7bebc-128">Všechny tři položky budete potřebovat později, kdy je přidáte do souboru config.js.</span><span class="sxs-lookup"><span data-stu-id="7bebc-128">You need all three later when you add them to the config.js file.</span></span>

## <a name="download-the-sample-files"></a><span data-ttu-id="7bebc-129">Stažení ukázkových souborů</span><span class="sxs-lookup"><span data-stu-id="7bebc-129">Download the sample files</span></span>
<span data-ttu-id="7bebc-130">Stáhněte ukázku pomocí libovolného z následujících dvou přístupů.</span><span class="sxs-lookup"><span data-stu-id="7bebc-130">Use either one of the following approaches to download the sample.</span></span>

1. <span data-ttu-id="7bebc-131">Přejděte na [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).</span><span class="sxs-lookup"><span data-stu-id="7bebc-131">Go to [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).</span></span>
2. <span data-ttu-id="7bebc-132">Klikněte na **Stáhnout ZIP**, uložte soubor .zip a potom z něj extrahujte všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="7bebc-132">Click **Download ZIP**, save the .zip file, and then extract all the files it contains.</span></span>

<span data-ttu-id="7bebc-133">Všechny následné úpravy souborů a spouštěné příkazy se provádí na souborech v této složce.</span><span class="sxs-lookup"><span data-stu-id="7bebc-133">All subsequent file modifications and run statements are made against files in this folder.</span></span>

## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a><span data-ttu-id="7bebc-134">Aktualizace souboru config.js.</span><span class="sxs-lookup"><span data-stu-id="7bebc-134">Update the config.js.</span></span> <span data-ttu-id="7bebc-135">pomocí adresy URL služby Search a klíče rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7bebc-135">with your Search service URL and api-key</span></span>
<span data-ttu-id="7bebc-136">Pomocí adresy URL a klíče rozhraní API, které jste zkopírovali dříve, zadejte do konfiguračního souboru adresu URL, klíč správce a klíč dotazu.</span><span class="sxs-lookup"><span data-stu-id="7bebc-136">Using the URL and api-key that you copied earlier, specify the URL, admin-key, and query-key in configuration file.</span></span>

<span data-ttu-id="7bebc-137">Klíče správce poskytují plnou kontrolu nad operacemi služby, včetně vytvoření nebo odstranění indexu a nahrávání dokumentů.</span><span class="sxs-lookup"><span data-stu-id="7bebc-137">Admin keys grant full control over service operations, including creating or deleting an index and loading documents.</span></span> <span data-ttu-id="7bebc-138">Klíče dotazu oproti tomu slouží k operacím jen pro čtení, které se obvykle používají v klientských aplikacích, které se připojují k službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="7bebc-138">In contrast, query keys are for read-only operations, typically used by client applications that connect to Azure Search.</span></span>

<span data-ttu-id="7bebc-139">Do této ukázky jsme zařadili klíč dotazu, abychom pomohli posílit osvědčený postup, kdy se v klientských aplikacích používá klíč dotazu.</span><span class="sxs-lookup"><span data-stu-id="7bebc-139">In this sample, we include the query key to help reinforce the best practice of using the query key in client applications.</span></span>

<span data-ttu-id="7bebc-140">Následující snímek obrazovky ukazuje soubor **config.js** otevřený v textovém editoru – příslušné položky jsou označené, abyste viděli, kde je potřeba soubor aktualizovat pomocí hodnot platných pro vaši vyhledávací službu.</span><span class="sxs-lookup"><span data-stu-id="7bebc-140">The following screenshot shows **config.js** open in a text editor, with the relevant entries demarcated so that you can see where to update the file with the values that are valid for your search service.</span></span>

![][5]

## <a name="host-a-runtime-environment-for-the-sample"></a><span data-ttu-id="7bebc-141">Hostování běhového prostředí pro ukázku</span><span class="sxs-lookup"><span data-stu-id="7bebc-141">Host a runtime environment for the sample</span></span>
<span data-ttu-id="7bebc-142">Ukázka vyžaduje server HTTP, který můžete nainstalovat globálně pomocí npm.</span><span class="sxs-lookup"><span data-stu-id="7bebc-142">The sample requires an HTTP server, which you can install globally using npm.</span></span>

<span data-ttu-id="7bebc-143">Pro následující příkazy použijte okno prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7bebc-143">Use a PowerShell window for the following commands.</span></span>

1. <span data-ttu-id="7bebc-144">Přejděte do složky, která obsahuje soubor **package.json**.</span><span class="sxs-lookup"><span data-stu-id="7bebc-144">Navigate to the folder that contains the **package.json** file.</span></span>
2. <span data-ttu-id="7bebc-145">Zadejte `npm install`.</span><span class="sxs-lookup"><span data-stu-id="7bebc-145">Type `npm install`.</span></span>
3. <span data-ttu-id="7bebc-146">Zadejte `npm install -g http-server`.</span><span class="sxs-lookup"><span data-stu-id="7bebc-146">Type `npm install -g http-server`.</span></span>

## <a name="build-the-index-and-run-the-application"></a><span data-ttu-id="7bebc-147">Sestavení indexu a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="7bebc-147">Build the index and run the application</span></span>
1. <span data-ttu-id="7bebc-148">Zadejte `npm run indexDocuments`.</span><span class="sxs-lookup"><span data-stu-id="7bebc-148">Type `npm run indexDocuments`.</span></span>
2. <span data-ttu-id="7bebc-149">Zadejte `npm run build`.</span><span class="sxs-lookup"><span data-stu-id="7bebc-149">Type `npm run build`.</span></span>
3. <span data-ttu-id="7bebc-150">Zadejte `npm run start_server`.</span><span class="sxs-lookup"><span data-stu-id="7bebc-150">Type `npm run start_server`.</span></span>
4. <span data-ttu-id="7bebc-151">Nasměrování prohlížeče na adresu `http://localhost:8080/index.html`</span><span class="sxs-lookup"><span data-stu-id="7bebc-151">Direct your browser at `http://localhost:8080/index.html`</span></span>

## <a name="search-on-usgs-data"></a><span data-ttu-id="7bebc-152">Hledání v datech USGS</span><span class="sxs-lookup"><span data-stu-id="7bebc-152">Search on USGS data</span></span>
<span data-ttu-id="7bebc-153">Sada dat USGS obsahuje záznamy, které se vztahují ke státu Rhode Island.</span><span class="sxs-lookup"><span data-stu-id="7bebc-153">The USGS data set includes records that are relevant to the state of Rhode Island.</span></span> <span data-ttu-id="7bebc-154">Pokud u prázdného vyhledávacího pole kliknete na tlačítko **Hledat**, obdržíte prvních 50 položek, což je výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="7bebc-154">If you click **Search** on an empty search box, you get the top 50 entries, which is the default.</span></span>

<span data-ttu-id="7bebc-155">Když zadáte hledaný výraz, vyhledávací web bude mít s čím pracovat.</span><span class="sxs-lookup"><span data-stu-id="7bebc-155">Entering a search term gives the search engine something to go on.</span></span> <span data-ttu-id="7bebc-156">Zkuste zadat místní název.</span><span class="sxs-lookup"><span data-stu-id="7bebc-156">Try entering a regional name.</span></span> <span data-ttu-id="7bebc-157">Roger Williams byl prvním guvernérem státu Rhode Island.</span><span class="sxs-lookup"><span data-stu-id="7bebc-157">"Roger Williams" was the first governor of Rhode Island.</span></span> <span data-ttu-id="7bebc-158">Je po něm pojmenovaná celá řada parků, budov a škol.</span><span class="sxs-lookup"><span data-stu-id="7bebc-158">Numerous parks, buildings, and schools are named after him.</span></span>

![][9]

<span data-ttu-id="7bebc-159">Může taky zkusit kterýkoli z těchto výrazů:</span><span class="sxs-lookup"><span data-stu-id="7bebc-159">You could also try any of these terms:</span></span>

* <span data-ttu-id="7bebc-160">Pawtucket</span><span class="sxs-lookup"><span data-stu-id="7bebc-160">Pawtucket</span></span>
* <span data-ttu-id="7bebc-161">Pembroke</span><span class="sxs-lookup"><span data-stu-id="7bebc-161">Pembroke</span></span>
* <span data-ttu-id="7bebc-162">goose +cape</span><span class="sxs-lookup"><span data-stu-id="7bebc-162">goose +cape</span></span>

## <a name="next-steps"></a><span data-ttu-id="7bebc-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7bebc-163">Next steps</span></span>
<span data-ttu-id="7bebc-164">Toto je první kurz služby Azure Search založený na Node.js a sadě dat USGS.</span><span class="sxs-lookup"><span data-stu-id="7bebc-164">This is the first Azure Search tutorial based on Node.js and the USGS dataset.</span></span> <span data-ttu-id="7bebc-165">Postupně ho budeme rozšiřovat o ukázky dalších vyhledávacích funkcí, které by se vám ve vlastních řešeních mohly hodit.</span><span class="sxs-lookup"><span data-stu-id="7bebc-165">Over time, we'll extend this tutorial to demonstrate additional search features you might want to use in your custom solutions.</span></span>

<span data-ttu-id="7bebc-166">Pokud už službu Azure Search trochu znáte, tato ukázka vám může posloužit jako odrazový můstek k vyzkoušení modulů pro automatické návrhy (našeptávání nebo automatické dokončování dotazů), filtrů a fasetové navigace.</span><span class="sxs-lookup"><span data-stu-id="7bebc-166">If you already have some background in Azure Search, you can use this sample as a springboard for trying suggesters (type-ahead or autocomplete queries), filters, and faceted navigation.</span></span> <span data-ttu-id="7bebc-167">Můžete taky zdokonalit stránku výsledků hledání přidáním počtů a dávkováním dokumentů, aby se výsledky daly procházet po stránkách.</span><span class="sxs-lookup"><span data-stu-id="7bebc-167">You can also improve upon the search results page by adding counts and batching documents so that users can page through the results.</span></span>

<span data-ttu-id="7bebc-168">Jste nováčky ve službě Azure Search?</span><span class="sxs-lookup"><span data-stu-id="7bebc-168">New to Azure Search?</span></span> <span data-ttu-id="7bebc-169">Doporučujeme vyzkoušet ostatní kurzy a vytvořit si představu o tom, co se dá vytvořit.</span><span class="sxs-lookup"><span data-stu-id="7bebc-169">We recommend trying other tutorials to develop an understanding of what you can create.</span></span> <span data-ttu-id="7bebc-170">Pokud hledáte další zdroje, přejděte na [stránku dokumentace](https://azure.microsoft.com/documentation/services/search/).</span><span class="sxs-lookup"><span data-stu-id="7bebc-170">Visit our [documentation page](https://azure.microsoft.com/documentation/services/search/) to find more resources.</span></span> <span data-ttu-id="7bebc-171">Přístup k dalším informacím vám poskytnou taky odkazy v [Seznamu videí a kurzů](search-video-demo-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="7bebc-171">You can also view the links in our [Video and Tutorial list](search-video-demo-tutorial-list.md) to access more information.</span></span>

<!--Image references-->
[1]: ./media/search-get-started-Nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-Nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-Nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-Nodejs/AzSearch-Nodejs-configjs.png
[9]: ./media/search-get-started-Nodejs/rogerwilliamsschool.png
