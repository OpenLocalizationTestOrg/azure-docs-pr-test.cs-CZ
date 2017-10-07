---
title: "aaaGet začít s Azure Search v Node.js | Microsoft Docs"
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
ms.openlocfilehash: e9c7d756c2ea191ee2a285485c90439b96aa73b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-nodejs"></a><span data-ttu-id="19773-103">Začínáme se službou Azure Search v Node.js</span><span class="sxs-lookup"><span data-stu-id="19773-103">Get started with Azure Search in Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="19773-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="19773-104">Portal</span></span>](search-get-started-portal.md)
> * [<span data-ttu-id="19773-105">.NET</span><span class="sxs-lookup"><span data-stu-id="19773-105">.NET</span></span>](search-howto-dotnet-sdk.md)
> 
> 

<span data-ttu-id="19773-106">Zjistěte, jak toobuild vlastní Node.js Hledat aplikace, která používá službu Azure Search své zkušenosti vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="19773-106">Learn how toobuild a custom Node.js search application that uses Azure Search for its search experience.</span></span> <span data-ttu-id="19773-107">Tento kurz používá hello [rozhraní REST API služby Azure Search](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objekty a operace, na které se použijí v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="19773-107">This tutorial uses hello [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objects and operations used in this exercise.</span></span>

<span data-ttu-id="19773-108">Použili jsme [Node.js](https://Nodejs.org) a NPM, [Sublime Text 3](http://www.sublimetext.com/3)a prostředí Windows PowerShell na Windows 8.1 toodevelop a testování tento kód.</span><span class="sxs-lookup"><span data-stu-id="19773-108">We used [Node.js](https://Nodejs.org) and NPM, [Sublime Text 3](http://www.sublimetext.com/3), and Windows PowerShell on Windows 8.1 toodevelop and test this code.</span></span>

<span data-ttu-id="19773-109">toorun této ukázky musíte mít službu Azure Search, které se můžete zaregistrovat si v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="19773-109">toorun this sample, you must have an Azure Search service, which you can sign up for in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="19773-110">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="19773-110">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for step-by-step instructions.</span></span>

## <a name="about-hello-data"></a><span data-ttu-id="19773-111">O datech hello</span><span class="sxs-lookup"><span data-stu-id="19773-111">About hello data</span></span>
<span data-ttu-id="19773-112">Tato ukázková aplikace používá data z hello [Spojených států Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), která jsou filtrovaná na velikost datové sady hello státu Rhode Island tooreduce hello.</span><span class="sxs-lookup"><span data-stu-id="19773-112">This sample application uses data from hello [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtered on hello state of Rhode Island tooreduce hello dataset size.</span></span> <span data-ttu-id="19773-113">Použijeme tato data toobuild vyhledávací aplikaci, která najde významné budovy, například nemocnice a školy, a geologické prvky, jako jsou datové proudy, jezera a vrcholy.</span><span class="sxs-lookup"><span data-stu-id="19773-113">We'll use this data toobuild a search application that returns landmark buildings such as hospitals and schools, as well as geological features like streams, lakes, and summits.</span></span>

<span data-ttu-id="19773-114">V této aplikaci hello **DataIndexer** program sestaví a zatížením hello index pomocí [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) konstrukce, načítání hello filtrovat sadu dat USGS z veřejné databáze Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="19773-114">In this application, hello **DataIndexer** program builds and loads hello index using an [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) construct, retrieving hello filtered USGS dataset from a public Azure SQL Database.</span></span> <span data-ttu-id="19773-115">Přihlašovací údaje a připojení zdroje dat online toohello informace je uvedené v kódu programu hello.</span><span class="sxs-lookup"><span data-stu-id="19773-115">Credentials and connection information toohello online data source is provided in hello program code.</span></span> <span data-ttu-id="19773-116">Není potřeba žádná další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="19773-116">No further configuration is necessary.</span></span>

> [!NOTE]
> <span data-ttu-id="19773-117">Jsme použili filtr na tuto datovou sadu toostay pod hello 10 000 dokumentů limit hello volné cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="19773-117">We applied a filter on this dataset toostay under hello 10,000 document limit of hello free pricing tier.</span></span> <span data-ttu-id="19773-118">Pokud používáte úroveň standard hello, toto omezení neplatí.</span><span class="sxs-lookup"><span data-stu-id="19773-118">If you use hello standard tier, this limit does not apply.</span></span> <span data-ttu-id="19773-119">Podrobnosti týkající se kapacity u jednotlivých cenových úrovní najdete v tématu [Omezení služby Search](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="19773-119">For details about capacity for each pricing tier, see [Search service limits](search-limits-quotas-capacity.md).</span></span>
> 
> 

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a><span data-ttu-id="19773-120">Nalezení názvu služby hello a klíč api-key služby Azure Search</span><span class="sxs-lookup"><span data-stu-id="19773-120">Find hello service name and api-key of your Azure Search service</span></span>
<span data-ttu-id="19773-121">Po vytvoření služby hello návratovou adresu URL hello portálu tooget toohello nebo `api-key`.</span><span class="sxs-lookup"><span data-stu-id="19773-121">After you create hello service, return toohello portal tooget hello URL or `api-key`.</span></span> <span data-ttu-id="19773-122">Připojení tooyour službu vyhledávání vyžadují, abyste měli obě adresy URL hello a `api-key` tooauthenticate hello volání.</span><span class="sxs-lookup"><span data-stu-id="19773-122">Connections tooyour Search service require that you have both hello URL and an `api-key` tooauthenticate hello call.</span></span>

1. <span data-ttu-id="19773-123">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="19773-123">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="19773-124">Hello přechod panelu klikněte na **služby vyhledávání** toolist všechny služby Azure Search zřízených pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="19773-124">In hello jump bar, click **Search service** toolist all Azure Search services provisioned for your subscription.</span></span>
3. <span data-ttu-id="19773-125">Vyberte službu hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="19773-125">Select hello service you want toouse.</span></span>
4. <span data-ttu-id="19773-126">Na řídicím panelu služby hello měli byste vidět dlaždice se základními informacemi, jako je například hello ikonu klíče pro přístup ke klíčům správce hello.</span><span class="sxs-lookup"><span data-stu-id="19773-126">On hello service dashboard, you should see tiles for essential information, such as hello key icon for accessing hello admin keys.</span></span>
5. <span data-ttu-id="19773-127">Zkopírujte adresu URL služby hello, klíč správce a klíč dotazu.</span><span class="sxs-lookup"><span data-stu-id="19773-127">Copy hello service URL, an admin key, and a query key.</span></span> <span data-ttu-id="19773-128">Je třeba všechny tři později při jejich přidání souboru config.js toohello.</span><span class="sxs-lookup"><span data-stu-id="19773-128">You need all three later when you add them toohello config.js file.</span></span>

## <a name="download-hello-sample-files"></a><span data-ttu-id="19773-129">Stažení ukázkových souborů hello</span><span class="sxs-lookup"><span data-stu-id="19773-129">Download hello sample files</span></span>
<span data-ttu-id="19773-130">Použijte buď jeden z hello následující ukázka hello toodownload přístupy.</span><span class="sxs-lookup"><span data-stu-id="19773-130">Use either one of hello following approaches toodownload hello sample.</span></span>

1. <span data-ttu-id="19773-131">Přejděte příliš[AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).</span><span class="sxs-lookup"><span data-stu-id="19773-131">Go too[AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).</span></span>
2. <span data-ttu-id="19773-132">Klikněte na tlačítko **stáhnout ZIP**, uložte soubor .zip hello a extrahujte všechny soubory hello obsahuje.</span><span class="sxs-lookup"><span data-stu-id="19773-132">Click **Download ZIP**, save hello .zip file, and then extract all hello files it contains.</span></span>

<span data-ttu-id="19773-133">Všechny následné úpravy souborů a spouštěné příkazy se provádí na souborech v této složce.</span><span class="sxs-lookup"><span data-stu-id="19773-133">All subsequent file modifications and run statements are made against files in this folder.</span></span>

## <a name="update-hello-configjs-with-your-search-service-url-and-api-key"></a><span data-ttu-id="19773-134">Aktualizace souboru config.js hello.</span><span class="sxs-lookup"><span data-stu-id="19773-134">Update hello config.js.</span></span> <span data-ttu-id="19773-135">pomocí adresy URL služby Search a klíče rozhraní API</span><span class="sxs-lookup"><span data-stu-id="19773-135">with your Search service URL and api-key</span></span>
<span data-ttu-id="19773-136">Pomocí hello adresy URL a rozhraní api-key, který jste zkopírovali dříve, zadejte adresu URL hello, klíč správce a klíč dotazu v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="19773-136">Using hello URL and api-key that you copied earlier, specify hello URL, admin-key, and query-key in configuration file.</span></span>

<span data-ttu-id="19773-137">Klíče správce poskytují plnou kontrolu nad operacemi služby, včetně vytvoření nebo odstranění indexu a nahrávání dokumentů.</span><span class="sxs-lookup"><span data-stu-id="19773-137">Admin keys grant full control over service operations, including creating or deleting an index and loading documents.</span></span> <span data-ttu-id="19773-138">Naproti tomu klíče dotazu jsou operacím jen pro čtení, obvykle používají v klientských aplikací, které se připojují tooAzure vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="19773-138">In contrast, query keys are for read-only operations, typically used by client applications that connect tooAzure Search.</span></span>

<span data-ttu-id="19773-139">V této ukázce jsme zahrnují hello dotazu klíče toohelp posílit osvědčený postup hello používá klíč dotazu hello v klientských aplikacích.</span><span class="sxs-lookup"><span data-stu-id="19773-139">In this sample, we include hello query key toohelp reinforce hello best practice of using hello query key in client applications.</span></span>

<span data-ttu-id="19773-140">Následující snímek obrazovky ukazuje Hello **config.js** otevřete v textovém editoru, s hello příslušné položky jsou označené, aby mohli zobrazit, kde tooupdate hello soubor s hello hodnoty, které jsou platné pro vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="19773-140">hello following screenshot shows **config.js** open in a text editor, with hello relevant entries demarcated so that you can see where tooupdate hello file with hello values that are valid for your search service.</span></span>

![][5]

## <a name="host-a-runtime-environment-for-hello-sample"></a><span data-ttu-id="19773-141">Hostování běhového prostředí pro ukázku hello</span><span class="sxs-lookup"><span data-stu-id="19773-141">Host a runtime environment for hello sample</span></span>
<span data-ttu-id="19773-142">Hello ukázka vyžaduje server HTTP, které můžete nainstalovat globálně pomocí npm.</span><span class="sxs-lookup"><span data-stu-id="19773-142">hello sample requires an HTTP server, which you can install globally using npm.</span></span>

<span data-ttu-id="19773-143">Pro hello následující příkazy použijte okno prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="19773-143">Use a PowerShell window for hello following commands.</span></span>

1. <span data-ttu-id="19773-144">Přejděte toohello složku, která obsahuje hello **package.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="19773-144">Navigate toohello folder that contains hello **package.json** file.</span></span>
2. <span data-ttu-id="19773-145">Zadejte `npm install`.</span><span class="sxs-lookup"><span data-stu-id="19773-145">Type `npm install`.</span></span>
3. <span data-ttu-id="19773-146">Zadejte `npm install -g http-server`.</span><span class="sxs-lookup"><span data-stu-id="19773-146">Type `npm install -g http-server`.</span></span>

## <a name="build-hello-index-and-run-hello-application"></a><span data-ttu-id="19773-147">Sestavení indexu hello a spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="19773-147">Build hello index and run hello application</span></span>
1. <span data-ttu-id="19773-148">Zadejte `npm run indexDocuments`.</span><span class="sxs-lookup"><span data-stu-id="19773-148">Type `npm run indexDocuments`.</span></span>
2. <span data-ttu-id="19773-149">Zadejte `npm run build`.</span><span class="sxs-lookup"><span data-stu-id="19773-149">Type `npm run build`.</span></span>
3. <span data-ttu-id="19773-150">Zadejte `npm run start_server`.</span><span class="sxs-lookup"><span data-stu-id="19773-150">Type `npm run start_server`.</span></span>
4. <span data-ttu-id="19773-151">Nasměrování prohlížeče na adresu `http://localhost:8080/index.html`</span><span class="sxs-lookup"><span data-stu-id="19773-151">Direct your browser at `http://localhost:8080/index.html`</span></span>

## <a name="search-on-usgs-data"></a><span data-ttu-id="19773-152">Hledání v datech USGS</span><span class="sxs-lookup"><span data-stu-id="19773-152">Search on USGS data</span></span>
<span data-ttu-id="19773-153">Hello sada dat USGS obsahuje záznamy, které jsou relevantní toohello státu Rhode Island.</span><span class="sxs-lookup"><span data-stu-id="19773-153">hello USGS data set includes records that are relevant toohello state of Rhode Island.</span></span> <span data-ttu-id="19773-154">Pokud kliknete na tlačítko **vyhledávání** na prázdného vyhledávacího pole, získáte hello prvních 50 položek, což je výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="19773-154">If you click **Search** on an empty search box, you get hello top 50 entries, which is hello default.</span></span>

<span data-ttu-id="19773-155">Zadat hledaný termín poskytuje vyhledávací hello něco toogo na.</span><span class="sxs-lookup"><span data-stu-id="19773-155">Entering a search term gives hello search engine something toogo on.</span></span> <span data-ttu-id="19773-156">Zkuste zadat místní název.</span><span class="sxs-lookup"><span data-stu-id="19773-156">Try entering a regional name.</span></span> <span data-ttu-id="19773-157">"Roger Williams" byla hello prvním guvernérem státu Rhode Island.</span><span class="sxs-lookup"><span data-stu-id="19773-157">"Roger Williams" was hello first governor of Rhode Island.</span></span> <span data-ttu-id="19773-158">Je po něm pojmenovaná celá řada parků, budov a škol.</span><span class="sxs-lookup"><span data-stu-id="19773-158">Numerous parks, buildings, and schools are named after him.</span></span>

![][9]

<span data-ttu-id="19773-159">Může taky zkusit kterýkoli z těchto výrazů:</span><span class="sxs-lookup"><span data-stu-id="19773-159">You could also try any of these terms:</span></span>

* <span data-ttu-id="19773-160">Pawtucket</span><span class="sxs-lookup"><span data-stu-id="19773-160">Pawtucket</span></span>
* <span data-ttu-id="19773-161">Pembroke</span><span class="sxs-lookup"><span data-stu-id="19773-161">Pembroke</span></span>
* <span data-ttu-id="19773-162">goose +cape</span><span class="sxs-lookup"><span data-stu-id="19773-162">goose +cape</span></span>

## <a name="next-steps"></a><span data-ttu-id="19773-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19773-163">Next steps</span></span>
<span data-ttu-id="19773-164">Toto je hello první kurz služby Azure Search na základě Node.js a hello sadu dat USGS.</span><span class="sxs-lookup"><span data-stu-id="19773-164">This is hello first Azure Search tutorial based on Node.js and hello USGS dataset.</span></span> <span data-ttu-id="19773-165">V čase budete rozšiřujeme tento kurz toodemonstrate dalších vyhledávacích funkcí, které můžete chtít toouse ve vlastních řešeních.</span><span class="sxs-lookup"><span data-stu-id="19773-165">Over time, we'll extend this tutorial toodemonstrate additional search features you might want toouse in your custom solutions.</span></span>

<span data-ttu-id="19773-166">Pokud už službu Azure Search trochu znáte, tato ukázka vám může posloužit jako odrazový můstek k vyzkoušení modulů pro automatické návrhy (našeptávání nebo automatické dokončování dotazů), filtrů a fasetové navigace.</span><span class="sxs-lookup"><span data-stu-id="19773-166">If you already have some background in Azure Search, you can use this sample as a springboard for trying suggesters (type-ahead or autocomplete queries), filters, and faceted navigation.</span></span> <span data-ttu-id="19773-167">Můžete taky zdokonalit stránku výsledků hledání hello přidáním počtů a dávkování dokumenty tak, aby hello výsledky procházet po stránkách.</span><span class="sxs-lookup"><span data-stu-id="19773-167">You can also improve upon hello search results page by adding counts and batching documents so that users can page through hello results.</span></span>

<span data-ttu-id="19773-168">Nové tooAzure hledání?</span><span class="sxs-lookup"><span data-stu-id="19773-168">New tooAzure Search?</span></span> <span data-ttu-id="19773-169">Doporučujeme vyzkoušet ostatní kurzy toodevelop představu o co můžete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="19773-169">We recommend trying other tutorials toodevelop an understanding of what you can create.</span></span> <span data-ttu-id="19773-170">Navštivte naše [stránky dokumentace, která](https://azure.microsoft.com/documentation/services/search/) toofind více prostředků.</span><span class="sxs-lookup"><span data-stu-id="19773-170">Visit our [documentation page](https://azure.microsoft.com/documentation/services/search/) toofind more resources.</span></span> <span data-ttu-id="19773-171">Můžete také zobrazit hello odkazy v našem [seznamu videí a kurzů](search-video-demo-tutorial-list.md) tooaccess Další informace.</span><span class="sxs-lookup"><span data-stu-id="19773-171">You can also view hello links in our [Video and Tutorial list](search-video-demo-tutorial-list.md) tooaccess more information.</span></span>

<!--Image references-->
[1]: ./media/search-get-started-Nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-Nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-Nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-Nodejs/AzSearch-Nodejs-configjs.png
[9]: ./media/search-get-started-Nodejs/rogerwilliamsschool.png
