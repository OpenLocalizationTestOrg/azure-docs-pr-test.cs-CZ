---
title: "Strojového učení dokumentace doporučení rozhraní API | Microsoft Docs"
description: "Dokumentace Azure Machine Learning doporučení API pro modul doporučení k dispozici na webu Microsoft Azure Marketplace."
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 32c3ab2f-fdd7-48cc-b501-ad55c79b87dc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: LuisCa
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 1fba64d78d779344e2895b0d54419186b7584865
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-recommendations-api-documentation"></a><span data-ttu-id="a4578-103">Dokumentace k rozhraní Azure Machine Learning Recommendations API</span><span class="sxs-lookup"><span data-stu-id="a4578-103">Azure Machine Learning Recommendations API Documentation</span></span>
> [!NOTE]
> <span data-ttu-id="a4578-104">Měli byste začít používat službu doporučení rozhraní API kognitivní místo tuto verzi.</span><span class="sxs-lookup"><span data-stu-id="a4578-104">You should start using the Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="a4578-105">Službu kognitivní doporučení budou nahrazení této služby, a všechny nové funkce bude vyvinutý existuje.</span><span class="sxs-lookup"><span data-stu-id="a4578-105">The Recommendations Cognitive Service will be replacing this service, and all the new features will be developed there.</span></span> <span data-ttu-id="a4578-106">Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.</span><span class="sxs-lookup"><span data-stu-id="a4578-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="a4578-107">Další informace o [migraci na novou službu kognitivní](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="a4578-107">Learn more about [Migrating to the new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="a4578-108">Tento dokument znázorňuje rozhraní API Microsoft Azure Machine Learning doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-108">This document depicts Microsoft Azure Machine Learning Recommendations APIs.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="a4578-109">1. Obecné – přehled</span><span class="sxs-lookup"><span data-stu-id="a4578-109">1. General overview</span></span>
<span data-ttu-id="a4578-110">Tento dokument je referenční dokumentace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a4578-110">This document is an API reference.</span></span> <span data-ttu-id="a4578-111">Měli byste začít s dokumentu "Azure Machine Learning doporučení – rychlý Start".</span><span class="sxs-lookup"><span data-stu-id="a4578-111">You should start with the “Azure Machine Learning Recommendation - Quick Start” document.</span></span>

<span data-ttu-id="a4578-112">Rozhraní API služby Azure Machine Learning doporučení je možné rozdělit do následujících logických skupin:</span><span class="sxs-lookup"><span data-stu-id="a4578-112">The Azure Machine Learning Recommendations API can be divided into the following logical groups:</span></span>

* <span data-ttu-id="a4578-113"><ins>Omezení</ins> -omezení rozhraní API doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-113"><ins>Limitations</ins> - Recommendations API limitations.</span></span>
* <span data-ttu-id="a4578-114"><ins>Obecné informace</ins> -informace o ověřování, služba URI a správu verzí.</span><span class="sxs-lookup"><span data-stu-id="a4578-114"><ins>General Information</ins> - Information on authentication, service URI and versioning.</span></span>
* <span data-ttu-id="a4578-115"><ins>Model Basic</ins> – rozhraní API, které vám umožňují provádět základní operace v modelu (například vytvářet, aktualizovat a odstraňovat model).</span><span class="sxs-lookup"><span data-stu-id="a4578-115"><ins>Model Basic</ins> - APIs that enable you to do the basic operations on model (e.g. create, update and delete a model).</span></span>
* <span data-ttu-id="a4578-116"><ins>Model Upřesnit</ins> – rozhraní API, která vám umožní získat pokročilé statistiky dat na modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-116"><ins>Model Advanced</ins> - APIs that enable you to get advanced data insights on the model.</span></span>
* <span data-ttu-id="a4578-117"><ins>Model obchodní pravidla</ins> – rozhraní API, která vám umožní spravovat obchodní pravidla o výsledcích doporučení modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-117"><ins>Model Business Rules</ins> - APIs that enable you to manage business rules on the model recommendation results.</span></span>
* <span data-ttu-id="a4578-118"><ins>Katalog</ins> – rozhraní API, které vám umožňují provádět základní operace ve model katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-118"><ins>Catalog</ins> - APIs that enable you to do basic operations on a model catalog.</span></span> <span data-ttu-id="a4578-119">Katalog obsahuje informace o metadatech na položky dat o využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-119">A catalog contains metadata information on the items of the usage data.</span></span>
* <span data-ttu-id="a4578-120"><ins>Funkce</ins> – rozhraní API umožňujících získáte přehledy na položku do katalogu a jak tyto informace slouží k vytvoření lepší doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-120"><ins>Feature</ins> - APIs that enable to get insights on item into the catalog and how to use this information to build better recommendations.</span></span>
* <span data-ttu-id="a4578-121"><ins>Data o využití</ins> – rozhraní API, které vám umožňují provádět základní operace s daty využití modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-121"><ins>Usage Data</ins> - APIs that enable you to do basic operations on the model usage data.</span></span> <span data-ttu-id="a4578-122">Ve formuláři Základní data o využití se skládá z řádků, které zahrnují dvojici & č. 60; userId & č. 62; & č. 60; itemId & č. 62;.</span><span class="sxs-lookup"><span data-stu-id="a4578-122">Usage data in the basic form consists of rows that include pairs of &#60;userId&#62;,&#60;itemId&#62;.</span></span>
* <span data-ttu-id="a4578-123"><ins>Sestavení</ins> – rozhraní API, která vám umožní aktivovat build modelu a provádět základní operace, které se vztahují na tento build.</span><span class="sxs-lookup"><span data-stu-id="a4578-123"><ins>Build</ins> - APIs that enable you to trigger a model build and do basic operations that are related to this build.</span></span> <span data-ttu-id="a4578-124">Až budete mít data o využití hodí v situaci, můžete aktivovat sestavení modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-124">You can trigger a model build once you have valuable usage data.</span></span>
* <span data-ttu-id="a4578-125"><ins>Doporučení</ins> – rozhraní API, která vám umožní využívat doporučení až po skončení sestavení modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-125"><ins>Recommendation</ins> - APIs that enable you to consume recommendations once the build of a model ends.</span></span>
* <span data-ttu-id="a4578-126"><ins>Uživatelská Data</ins> – rozhraní API, která vám umožní načíst informace o uživatelských dat využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-126"><ins>User Data</ins> - APIs that enable you to fetch information on the user usage data.</span></span>
* <span data-ttu-id="a4578-127"><ins>Oznámení</ins> – rozhraní API, která vám umožní přijímat oznámení o problémech souvisejících s vaše operace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a4578-127"><ins>Notifications</ins> - APIs that enable you to receive notifications on problems related to your API operations.</span></span> <span data-ttu-id="a4578-128">(Například zasíláte data o využití přes získávání dat a většiny událostí dochází k selhání zpracování.</span><span class="sxs-lookup"><span data-stu-id="a4578-128">(For example, you are reporting usage data via Data Acquisition and most of the events processing are failing.</span></span> <span data-ttu-id="a4578-129">Oznámení o chybě bude vyvolána.)</span><span class="sxs-lookup"><span data-stu-id="a4578-129">An error notification will be raised.)</span></span>

## <a name="2-limitations"></a><span data-ttu-id="a4578-130">2. Omezení</span><span class="sxs-lookup"><span data-stu-id="a4578-130">2. Limitations</span></span>
* <span data-ttu-id="a4578-131">Maximální počet modelů podle předplatného je 10.</span><span class="sxs-lookup"><span data-stu-id="a4578-131">The maximum number of models per subscription is 10.</span></span>
* <span data-ttu-id="a4578-132">Maximální počet sestavení za modelu je 20.</span><span class="sxs-lookup"><span data-stu-id="a4578-132">The maximum number of builds per model is 20.</span></span>
* <span data-ttu-id="a4578-133">Maximální počet položek, které mohou být uloženy katalog je 100 000.</span><span class="sxs-lookup"><span data-stu-id="a4578-133">The maximum number of items that a catalog can hold is 100,000.</span></span>
* <span data-ttu-id="a4578-134">Maximální počet bodů využití, které jsou zachovány je ~ 5 000 000.</span><span class="sxs-lookup"><span data-stu-id="a4578-134">The maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="a4578-135">Nejstarší budou odstraněna, pokud nové se nahrál nebo nahlásí.</span><span class="sxs-lookup"><span data-stu-id="a4578-135">The oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="a4578-136">Maximální velikost dat, který může odeslat v BLOGU (například import data katalogu, data o využití import) je 200MB.</span><span class="sxs-lookup"><span data-stu-id="a4578-136">The maximum size of data that can be sent in POST (e.g. import catalog data, import usage data) is 200MB.</span></span>
* <span data-ttu-id="a4578-137">Maximální počet položek, které můžete být požádáni o při získávání doporučení je 150.</span><span class="sxs-lookup"><span data-stu-id="a4578-137">The maximum number of items that can be asked for when getting recommendations is 150.</span></span>

## <a name="3-apis---general-information"></a><span data-ttu-id="a4578-138">3. Rozhraní API – obecné informace</span><span class="sxs-lookup"><span data-stu-id="a4578-138">3. APIs - general information</span></span>
### <a name="31-authentication"></a><span data-ttu-id="a4578-139">3.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-139">3.1.</span></span> <span data-ttu-id="a4578-140">Authentication</span><span class="sxs-lookup"><span data-stu-id="a4578-140">Authentication</span></span>
<span data-ttu-id="a4578-141">Postupujte podle pokynů Microsoft Azure Marketplace týkající se ověřování.</span><span class="sxs-lookup"><span data-stu-id="a4578-141">Please follow the Microsoft Azure Marketplace guidelines regarding authentication.</span></span> <span data-ttu-id="a4578-142">Na webu marketplace podporuje základní nebo OAuth metodu ověřování.</span><span class="sxs-lookup"><span data-stu-id="a4578-142">The marketplace supports either the Basic or OAuth authentication method.</span></span>

### <a name="32-service-uri"></a><span data-ttu-id="a4578-143">3.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-143">3.2.</span></span> <span data-ttu-id="a4578-144">URI služby</span><span class="sxs-lookup"><span data-stu-id="a4578-144">Service URI</span></span>
<span data-ttu-id="a4578-145">Kořenový adresář identifikátor URI pro rozhraní API služby Azure Machine Learning doporučení je [sem.](https://api.datamarket.azure.com/amla/recommendations/v3/)</span><span class="sxs-lookup"><span data-stu-id="a4578-145">The service root URI for the Azure Machine Learning Recommendations APIs is [here.](https://api.datamarket.azure.com/amla/recommendations/v3/)</span></span>

<span data-ttu-id="a4578-146">Službu úplný identifikátor URI je vyjádřit pomocí elementů specifikace prostředí OData.</span><span class="sxs-lookup"><span data-stu-id="a4578-146">The full service URI is expressed using elements of the OData specification.</span></span>  

### <a name="33-api-version"></a><span data-ttu-id="a4578-147">3.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-147">3.3.</span></span> <span data-ttu-id="a4578-148">Verze rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a4578-148">API version</span></span>
<span data-ttu-id="a4578-149">Každé volání rozhraní API bude mít na konci, parametr dotazu s názvem apiVersion, který musí být nastavena na 1.0.</span><span class="sxs-lookup"><span data-stu-id="a4578-149">Each API call will have, at the end, a query parameter called apiVersion that should be set to 1.0.</span></span>

### <a name="34-ids-are-case-sensitive"></a><span data-ttu-id="a4578-150">3.4.</span><span class="sxs-lookup"><span data-stu-id="a4578-150">3.4.</span></span> <span data-ttu-id="a4578-151">ID jsou malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="a4578-151">IDs are case sensitive</span></span>
<span data-ttu-id="a4578-152">ID, vrácený některé z rozhraní API se velká a malá písmena a by měl být použit jako takový, při předány jako parametry při následných voláních rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a4578-152">IDs, returned by any of the APIs, are case sensitive and should be used as such when passed as parameters in subsequent API calls.</span></span> <span data-ttu-id="a4578-153">ID modelu a ID katalogu pro instanci, jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="a4578-153">For instance, model IDs and catalog IDs are case sensitive.</span></span>

## <a name="4-recommendations-quality-and-cold-items"></a><span data-ttu-id="a4578-154">4. Doporučení kvality a Cold položky</span><span class="sxs-lookup"><span data-stu-id="a4578-154">4. Recommendations Quality and Cold Items</span></span>
### <a name="41-recommendation-quality"></a><span data-ttu-id="a4578-155">4.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-155">4.1.</span></span> <span data-ttu-id="a4578-156">Doporučení kvality</span><span class="sxs-lookup"><span data-stu-id="a4578-156">Recommendation quality</span></span>
<span data-ttu-id="a4578-157">Vytvoření modelu doporučení je obvykle dostatek, aby umožňovalo systému poskytnout doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-157">Creating a recommendation model is usually enough to allow the system to provide recommendations.</span></span> <span data-ttu-id="a4578-158">Nicméně kvality doporučení se může lišit podle využití zpracovat a pokrytí katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-158">Nevertheless, recommendation quality varies based on the usage processed and the coverage of the catalog.</span></span> <span data-ttu-id="a4578-159">Například pokud máte spoustu cold položky (položek bez významné využití), systém bude mít problémy poskytuje doporučení pro tyto položky nebo pomocí takové položky jako doporučenou jeden.</span><span class="sxs-lookup"><span data-stu-id="a4578-159">For example if you have a lot of cold items (items without significant usage), the system will have difficulties providing a recommendation for such an item or using such an item as a recommended one.</span></span> <span data-ttu-id="a4578-160">Překonat problém cold položky systému dovoluje metadata položky, které chcete vylepšit doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-160">In order to overcome the cold item problem, the system allows the use of metadata of the items to enhance the recommendations.</span></span> <span data-ttu-id="a4578-161">Tato metadata se označuje jako funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-161">This metadata is referred to as features.</span></span> <span data-ttu-id="a4578-162">Typické funkce jsou autor knihy nebo video z objektu actor.</span><span class="sxs-lookup"><span data-stu-id="a4578-162">Typical features are a book's author or a movie's actor.</span></span> <span data-ttu-id="a4578-163">Funkce poskytované prostřednictvím katalogu ve formě řetězce klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="a4578-163">Features are provided via the catalog in the form of key/value strings.</span></span> <span data-ttu-id="a4578-164">Úplný formát souboru katalogu, naleznete [importovat část katalogu](#81-import-catalog-data).</span><span class="sxs-lookup"><span data-stu-id="a4578-164">For the full format of the catalog file, please refer to the [import catalog section](#81-import-catalog-data).</span></span> 

### <a name="42-rank-build"></a><span data-ttu-id="a4578-165">4.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-165">4.2.</span></span> <span data-ttu-id="a4578-166">RANK sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-166">Rank build</span></span>
<span data-ttu-id="a4578-167">Funkce můžete vylepšit modelu doporučení, ale k tomu vyžaduje použití smysluplný funkcí.</span><span class="sxs-lookup"><span data-stu-id="a4578-167">Features can enhance the recommendation model, but to do so requires the use of meaningful features.</span></span> <span data-ttu-id="a4578-168">Pro tento účel, který byl zaveden nového sestavení - rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-168">For this purpose a new build was introduced - a rank build.</span></span> <span data-ttu-id="a4578-169">Toto sestavení se zařadit užitečnost funkcí.</span><span class="sxs-lookup"><span data-stu-id="a4578-169">This build will rank the usefulness of features.</span></span> <span data-ttu-id="a4578-170">Důležité funkce je funkce s skóre pořadí 2 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="a4578-170">A meaningful feature is a feature with a rank score of 2 and up.</span></span>
<span data-ttu-id="a4578-171">Po porozumět tomu, které funkce mají význam, aktivovat build doporučení s seznamu (nebo dílčí seznam) smysluplný funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-171">After understanding which of the features are meaningful, trigger a recommendation build with the list (or sublist) of meaningful features.</span></span> <span data-ttu-id="a4578-172">Je možné použít tyto funkce pro zvýšení záložním položky a cold položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-172">It is possible to use these feature for the enhancement of both warm items and cold items.</span></span> <span data-ttu-id="a4578-173">Chcete-li je používat pro záložním položky `UseFeatureInModel` parametr sestavení by měly být nastavené.</span><span class="sxs-lookup"><span data-stu-id="a4578-173">In order to use them for warm items, the `UseFeatureInModel` build parameter should be set up.</span></span> <span data-ttu-id="a4578-174">Aby bylo možné používat funkce pro studenou položky `AllowColdItemPlacement` by měl být povolen parametr sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-174">In order to use features for cold items, the `AllowColdItemPlacement` build parameter should be enabled.</span></span>
<span data-ttu-id="a4578-175">Poznámka: Není možné povolit `AllowColdItemPlacement` bez povolení `UseFeatureInModel`.</span><span class="sxs-lookup"><span data-stu-id="a4578-175">Note: It is not possible to enable `AllowColdItemPlacement` without enabling `UseFeatureInModel`.</span></span>

### <a name="43-recommendation-reasoning"></a><span data-ttu-id="a4578-176">4.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-176">4.3.</span></span> <span data-ttu-id="a4578-177">Doporučení reasoning</span><span class="sxs-lookup"><span data-stu-id="a4578-177">Recommendation reasoning</span></span>
<span data-ttu-id="a4578-178">Doporučení reasoning je další aspekt používání funkcí.</span><span class="sxs-lookup"><span data-stu-id="a4578-178">Recommendation reasoning is another aspect of feature usage.</span></span> <span data-ttu-id="a4578-179">Modul Azure Machine Learning doporučení skutečně, můžete použít funkce k vysvětlení doporučení (také známa jako</span><span class="sxs-lookup"><span data-stu-id="a4578-179">Indeed, the Azure Machine Learning Recommendations engine can use features to provide recommendation explanations (a.k.a.</span></span> <span data-ttu-id="a4578-180">důvody), což větší jistotou v položce doporučené od doporučení příjemce.</span><span class="sxs-lookup"><span data-stu-id="a4578-180">reasoning), leading to more confidence in the recommended item from the recommendation consumer.</span></span>
<span data-ttu-id="a4578-181">Chcete-li povolit reasoning, `AllowFeatureCorrelation` a `ReasoningFeatureList` instalační program před požaduje doporučení sestavení musí být parametry.</span><span class="sxs-lookup"><span data-stu-id="a4578-181">To enable reasoning, the `AllowFeatureCorrelation` and `ReasoningFeatureList` parameters should be setup prior to requesting a recommendation build.</span></span>

## <a name="5-model-basic"></a><span data-ttu-id="a4578-182">5. Model Basic</span><span class="sxs-lookup"><span data-stu-id="a4578-182">5. Model Basic</span></span>
### <a name="51-create-model"></a><span data-ttu-id="a4578-183">5.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-183">5.1.</span></span> <span data-ttu-id="a4578-184">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-184">Create Model</span></span>
<span data-ttu-id="a4578-185">Vytvoří žádost o "Vytvoření modelu".</span><span class="sxs-lookup"><span data-stu-id="a4578-185">Creates a “create model” request.</span></span>

| <span data-ttu-id="a4578-186">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-186">HTTP Method</span></span> | <span data-ttu-id="a4578-187">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-187">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-188">POST</span><span class="sxs-lookup"><span data-stu-id="a4578-188">POST</span></span> |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-189">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-189">Example:</span></span><br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-190">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-190">Parameter Name</span></span> | <span data-ttu-id="a4578-191">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-191">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-192">%{ModelName/</span><span class="sxs-lookup"><span data-stu-id="a4578-192">modelName</span></span> |<span data-ttu-id="a4578-193">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="a4578-193">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="a4578-194">Maximální délka: 20</span><span class="sxs-lookup"><span data-stu-id="a4578-194">Max length: 20</span></span> |
| <span data-ttu-id="a4578-195">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-195">apiVersion</span></span> |<span data-ttu-id="a4578-196">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-196">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-197">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-197">Request Body</span></span> |<span data-ttu-id="a4578-198">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-198">NONE</span></span> |

<span data-ttu-id="a4578-199">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-199">**Response**:</span></span>

<span data-ttu-id="a4578-200">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-200">HTTP Status code: 200</span></span>

* <span data-ttu-id="a4578-201">`feed/entry/content/properties/id`-Obsahuje ID modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-201">`feed/entry/content/properties/id` - Contains the model ID.</span></span>
  <span data-ttu-id="a4578-202">**Poznámka:**: ID modelu je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="a4578-202">**Note**: model ID is case sensitive.</span></span>

<span data-ttu-id="a4578-203">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-203">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="52-get-model"></a><span data-ttu-id="a4578-204">5.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-204">5.2.</span></span> <span data-ttu-id="a4578-205">Získat modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-205">Get Model</span></span>
<span data-ttu-id="a4578-206">Vytvoří žádost o "get model".</span><span class="sxs-lookup"><span data-stu-id="a4578-206">Creates a “get model” request.</span></span>

| <span data-ttu-id="a4578-207">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-207">HTTP Method</span></span> | <span data-ttu-id="a4578-208">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-208">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-209">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-209">GET</span></span> |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="a4578-210">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-210">Example:</span></span><br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-211">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-211">Parameter Name</span></span> | <span data-ttu-id="a4578-212">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-212">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-213">id</span><span class="sxs-lookup"><span data-stu-id="a4578-213">id</span></span> |<span data-ttu-id="a4578-214">Jedinečný identifikátor modelu (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="a4578-214">Unique identifier of the model (case sensitive)</span></span> |
| <span data-ttu-id="a4578-215">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-215">apiVersion</span></span> |<span data-ttu-id="a4578-216">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-216">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-217">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-217">Request Body</span></span> |<span data-ttu-id="a4578-218">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-218">NONE</span></span> |

<span data-ttu-id="a4578-219">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-219">**Response**:</span></span>

<span data-ttu-id="a4578-220">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-220">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-221">Data modelu naleznete v části následující prvky:</span><span class="sxs-lookup"><span data-stu-id="a4578-221">The model data can be found under the following elements:</span></span>

* <span data-ttu-id="a4578-222">`feed/entry/content/properties/Id`-Jedinečné ID modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-222">`feed/entry/content/properties/Id` - Model unique ID.</span></span>
* <span data-ttu-id="a4578-223">`feed/entry/content/properties/Name`-Název modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-223">`feed/entry/content/properties/Name` - Model name.</span></span>
* <span data-ttu-id="a4578-224">`feed/entry/content/properties/Date`-Datum vytvoření model.</span><span class="sxs-lookup"><span data-stu-id="a4578-224">`feed/entry/content/properties/Date` - Model creation date.</span></span>
* <span data-ttu-id="a4578-225">`feed/entry/content/properties/Status`-Modelu stav.</span><span class="sxs-lookup"><span data-stu-id="a4578-225">`feed/entry/content/properties/Status` - Model status.</span></span> <span data-ttu-id="a4578-226">Jeden z následujících:</span><span class="sxs-lookup"><span data-stu-id="a4578-226">One of the following:</span></span>
  * <span data-ttu-id="a4578-227">Vytvořit - Model je vytvořený a neobsahuje katalogu a využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-227">Created - Model is created and does not contain Catalog and Usage.</span></span>
  * <span data-ttu-id="a4578-228">ReadyForBuild - Model se vytvoří a obsahuje katalogu a využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-228">ReadyForBuild - Model is created and contains Catalog and Usage.</span></span>
* <span data-ttu-id="a4578-229">`feed/entry/content/properties/HasActiveBuild`-Určuje, pokud byl úspěšně sestaven modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-229">`feed/entry/content/properties/HasActiveBuild` - Indicates if the model was built successfully.</span></span>
* <span data-ttu-id="a4578-230">`feed/entry/content/properties/BuildId`-ID modelu active sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-230">`feed/entry/content/properties/BuildId` - Model active build ID.</span></span>
* <span data-ttu-id="a4578-231">`feed/entry/content/properties/Mpr`-Model střední percentilu hodnocení (MPR - Další informace naleznete v tématu ModelInsight).</span><span class="sxs-lookup"><span data-stu-id="a4578-231">`feed/entry/content/properties/Mpr` - Model mean percentile ranking (MPR - see ModelInsight for more information).</span></span>
* <span data-ttu-id="a4578-232">`feed/entry/content/properties/UserName`-Model interní uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="a4578-232">`feed/entry/content/properties/UserName` - Model internal user name.</span></span>

<span data-ttu-id="a4578-233">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-233">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get A List Of All Models</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T14:35:51Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
        <d:Name m:type="Edm.String">vah-11111</d:Name>
        <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
        <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
        <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
        <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
        <d:Description m:type="Edm.String">short description</d:Description>
        <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="53----get-all-models"></a><span data-ttu-id="a4578-234">5.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-234">5.3.</span></span>    <span data-ttu-id="a4578-235">Získat všechny modely</span><span class="sxs-lookup"><span data-stu-id="a4578-235">Get All Models</span></span>
<span data-ttu-id="a4578-236">Načte všechny modely aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-236">Retrieves all models of the current user.</span></span>

| <span data-ttu-id="a4578-237">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-237">HTTP Method</span></span> | <span data-ttu-id="a4578-238">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-238">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-239">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-239">GET</span></span> |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br><span data-ttu-id="a4578-240">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-240">Example:</span></span><br>`<rootURI>/GetAllModels?apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-241">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-241">Parameter Name</span></span> | <span data-ttu-id="a4578-242">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-242">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-243">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-243">apiVersion</span></span> |<span data-ttu-id="a4578-244">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-244">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-245">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-245">Request Body</span></span> |<span data-ttu-id="a4578-246">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-246">NONE</span></span> |

<span data-ttu-id="a4578-247">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-247">**Response**:</span></span>

<span data-ttu-id="a4578-248">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-248">HTTP Status code: 200</span></span>

* <span data-ttu-id="a4578-249">`feed/entry/content/properties/Id`-Jedinečné ID modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-249">`feed/entry/content/properties/Id` - Model unique ID.</span></span>
* <span data-ttu-id="a4578-250">`feed/entry/content/properties/Name`-Název modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-250">`feed/entry/content/properties/Name` - Model name.</span></span>
* <span data-ttu-id="a4578-251">`feed/entry/content/properties/Date`-Datum vytvoření model.</span><span class="sxs-lookup"><span data-stu-id="a4578-251">`feed/entry/content/properties/Date` - Model creation date.</span></span>
* <span data-ttu-id="a4578-252">`feed/entry/content/properties/Status`-Modelu stav.</span><span class="sxs-lookup"><span data-stu-id="a4578-252">`feed/entry/content/properties/Status` - Model status.</span></span> <span data-ttu-id="a4578-253">Jeden z následujících:</span><span class="sxs-lookup"><span data-stu-id="a4578-253">One of the following:</span></span>
  * <span data-ttu-id="a4578-254">Vytvořit - Model je vytvořený a neobsahuje katalogu a využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-254">Created - Model is created and does not contain Catalog and Usage.</span></span>
  * <span data-ttu-id="a4578-255">ReadyForBuild - Model se vytvoří a obsahuje katalogu a využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-255">ReadyForBuild - Model is created and contains Catalog and Usage.</span></span>
* <span data-ttu-id="a4578-256">`feed/entry/content/properties/HasActiveBuild`-Určuje, pokud byl úspěšně sestaven modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-256">`feed/entry/content/properties/HasActiveBuild` - Indicates if the model was built successfully.</span></span>
* <span data-ttu-id="a4578-257">`feed/entry/content/properties/BuildId`-ID modelu active sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-257">`feed/entry/content/properties/BuildId` - Model active build ID.</span></span>
* <span data-ttu-id="a4578-258">`feed/entry/content/properties/Mpr`-Model MPR (Další informace naleznete v tématu ModelInsight).</span><span class="sxs-lookup"><span data-stu-id="a4578-258">`feed/entry/content/properties/Mpr` - Model MPR (see ModelInsight for more information).</span></span>
* <span data-ttu-id="a4578-259">`feed/entry/content/properties/UserName`-Model interní uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="a4578-259">`feed/entry/content/properties/UserName` - Model internal user name.</span></span>
* <span data-ttu-id="a4578-260">`feed/entry/content/properties/UsageFileNames`-Seznam souborů využívání modelu oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="a4578-260">`feed/entry/content/properties/UsageFileNames` - List of model usage files separated by comma.</span></span>
* <span data-ttu-id="a4578-261">`feed/entry/content/properties/CatalogId`-ID modelu katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-261">`feed/entry/content/properties/CatalogId` - Model catalog ID.</span></span>
* <span data-ttu-id="a4578-262">`feed/entry/content/properties/Description`-Modelu popis.</span><span class="sxs-lookup"><span data-stu-id="a4578-262">`feed/entry/content/properties/Description` - Model description.</span></span>
* <span data-ttu-id="a4578-263">`feed/entry/content/properties/CatalogFileName`-Název souboru katalogu model.</span><span class="sxs-lookup"><span data-stu-id="a4578-263">`feed/entry/content/properties/CatalogFileName` - Model catalog file name.</span></span>

<span data-ttu-id="a4578-264">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-264">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get A List Of All Models</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-28T14:35:51Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
                    <d:Name m:type="Edm.String">vah-11111</d:Name>
                    <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
                    <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
                    <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
                    <d:BuildId m:type="Edm.String">-1</d:BuildId>
                    <d:Mpr m:type="Edm.String">0</d:Mpr>
                    <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
                    <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
                    <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
                    <d:Description m:type="Edm.String">short description</d:Description>
                    <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="54----update-model"></a><span data-ttu-id="a4578-265">5.4.</span><span class="sxs-lookup"><span data-stu-id="a4578-265">5.4.</span></span>    <span data-ttu-id="a4578-266">Aktualizace modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-266">Update Model</span></span>
<span data-ttu-id="a4578-267">Můžete aktualizovat popis modelu nebo ID aktivního sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-267">You can update the model description or the active build ID.</span></span><br><span data-ttu-id="a4578-268">
<ins>ID aktivního sestavení</ins> -každé sestavení pro každý model má sestavení ID.</span><span class="sxs-lookup"><span data-stu-id="a4578-268">
<ins>Active build ID</ins> - Every build for every model has a build ID.</span></span> <span data-ttu-id="a4578-269">ID aktivního sestavení je prvním úspěšném sestavení každý nový model.</span><span class="sxs-lookup"><span data-stu-id="a4578-269">The active build ID is the first successful build of every new model.</span></span> <span data-ttu-id="a4578-270">Jakmile máte ID aktivního sestavení a proveďte další sestavení pro stejný model, je nutné explicitně nastavit jako výchozí ID sestavení Pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="a4578-270">Once you have an active build ID and you do additional builds for the same model, you need to explicitly set it as the default build ID if you want to.</span></span> <span data-ttu-id="a4578-271">Když budete používat doporučení, pokud nezadáte ID sestavení, které chcete použít, výchozí nastavení se použije automaticky.</span><span class="sxs-lookup"><span data-stu-id="a4578-271">When you consume recommendations, if you do not specify the build ID that you want to use, the default one will be used automatically.</span></span><br>
<span data-ttu-id="a4578-272">Tento mechanismus umožňuje – až budete mít model doporučení v produkčním prostředí - pro vytvoření nových modelů a testování je před zvýšením úrovně je do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4578-272">This mechanism enables you - once you have a recommendation model in production - to build new models and test them before you promote them to production.</span></span>

| <span data-ttu-id="a4578-273">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-273">HTTP Method</span></span> | <span data-ttu-id="a4578-274">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-274">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-275">PUT</span><span class="sxs-lookup"><span data-stu-id="a4578-275">PUT</span></span> |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><span data-ttu-id="a4578-276">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-276">Example:</span></span><br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-277">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-277">Parameter Name</span></span> | <span data-ttu-id="a4578-278">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-278">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-279">id</span><span class="sxs-lookup"><span data-stu-id="a4578-279">id</span></span> |<span data-ttu-id="a4578-280">Jedinečný identifikátor modelu (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="a4578-280">Unique identifier of the model (case sensitive)</span></span> |
| <span data-ttu-id="a4578-281">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-281">apiVersion</span></span> |<span data-ttu-id="a4578-282">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-282">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-283">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-283">Request Body</span></span> |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br><span data-ttu-id="a4578-284">Všimněte si, že značky XML, popis a ActiveBuildId jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="a4578-284">Note that the XML tags Description and ActiveBuildId are optional.</span></span> <span data-ttu-id="a4578-285">Pokud nechcete nastavit popis nebo ActiveBuildId, odeberte celý značky.</span><span class="sxs-lookup"><span data-stu-id="a4578-285">If you do not want to set Description or ActiveBuildId, remove the entire tag.</span></span> |

<span data-ttu-id="a4578-286">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-286">**Response**:</span></span>

<span data-ttu-id="a4578-287">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-287">HTTP Status code: 200</span></span>

### <a name="55----delete-model"></a><span data-ttu-id="a4578-288">5.5.</span><span class="sxs-lookup"><span data-stu-id="a4578-288">5.5.</span></span>    <span data-ttu-id="a4578-289">Odstranit Model</span><span class="sxs-lookup"><span data-stu-id="a4578-289">Delete Model</span></span>
<span data-ttu-id="a4578-290">Odstraní existující model podle ID.</span><span class="sxs-lookup"><span data-stu-id="a4578-290">Deletes an existing model by ID.</span></span>

| <span data-ttu-id="a4578-291">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-291">HTTP Method</span></span> | <span data-ttu-id="a4578-292">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-292">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-293">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="a4578-293">DELETE</span></span> |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="a4578-294">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-294">Example:</span></span><br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-295">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-295">Parameter Name</span></span> | <span data-ttu-id="a4578-296">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-296">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-297">id</span><span class="sxs-lookup"><span data-stu-id="a4578-297">id</span></span> |<span data-ttu-id="a4578-298">Jedinečný identifikátor modelu (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="a4578-298">Unique identifier of the model (case sensitive)</span></span> |
| <span data-ttu-id="a4578-299">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-299">apiVersion</span></span> |<span data-ttu-id="a4578-300">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-300">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-301">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-301">Request Body</span></span> |<span data-ttu-id="a4578-302">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-302">NONE</span></span> |

<span data-ttu-id="a4578-303">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-303">**Response**:</span></span>

<span data-ttu-id="a4578-304">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-304">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-305">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-305">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Delete Model by Id</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T10:39:33Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">DeleteModelByIdEntity</title>
    <updated>2014-10-28T10:39:33Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:string m:type="Edm.String"></d:string>
      </m:properties>
    </content>
      </entry>
    </feed>

## <a name="6-model-advanced"></a><span data-ttu-id="a4578-306">6. Rozšířené modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-306">6. Model Advanced</span></span>
### <a name="61----model-data-insight"></a><span data-ttu-id="a4578-307">6.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-307">6.1.</span></span>    <span data-ttu-id="a4578-308">Přehled modelu dat</span><span class="sxs-lookup"><span data-stu-id="a4578-308">Model Data Insight</span></span>
<span data-ttu-id="a4578-309">Vrátí data o využití, která tento model byl sestaven s statistické údaje.</span><span class="sxs-lookup"><span data-stu-id="a4578-309">Returns statistical data on the usage data that this model was built with.</span></span>

<span data-ttu-id="a4578-310">K dispozici pouze pro sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-310">Available only for Recommendation build.</span></span>

| <span data-ttu-id="a4578-311">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-311">HTTP Method</span></span> | <span data-ttu-id="a4578-312">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-312">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-313">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-313">GET</span></span> |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="a4578-314">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-314">Example:</span></span><br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-315">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-315">Parameter Name</span></span> | <span data-ttu-id="a4578-316">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-316">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-317">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-317">modelId</span></span> |<span data-ttu-id="a4578-318">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-318">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-319">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-319">apiVersion</span></span> |<span data-ttu-id="a4578-320">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-320">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-321">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-321">Request Body</span></span> |<span data-ttu-id="a4578-322">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-322">NONE</span></span> |

<span data-ttu-id="a4578-323">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-323">**Response**:</span></span>

<span data-ttu-id="a4578-324">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-324">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-325">Data se vrátí jako kolekci vlastností.</span><span class="sxs-lookup"><span data-stu-id="a4578-325">The data is returned as a collection of properties.</span></span>

* <span data-ttu-id="a4578-326">`feed/entry/id/content/properties/key`-Obsahuje název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-326">`feed/entry/id/content/properties/key` - Holds the property name.</span></span>
* <span data-ttu-id="a4578-327">`feed/entry/id/content/properties/value`-Obsahuje hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-327">`feed/entry/id/content/properties/value` - Holds the property value.</span></span>

<span data-ttu-id="a4578-328">Následující tabulka znázorňuje hodnotu, která představuje každý klíč.</span><span class="sxs-lookup"><span data-stu-id="a4578-328">The table below depicts the value that each key represents.</span></span>

| <span data-ttu-id="a4578-329">Klíč</span><span class="sxs-lookup"><span data-stu-id="a4578-329">Key</span></span> | <span data-ttu-id="a4578-330">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-330">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-331">AvgItemLength</span><span class="sxs-lookup"><span data-stu-id="a4578-331">AvgItemLength</span></span> |<span data-ttu-id="a4578-332">Průměrný počet jedinečných uživatelů na každou položku.</span><span class="sxs-lookup"><span data-stu-id="a4578-332">Average number of distinct users per item.</span></span> |
| <span data-ttu-id="a4578-333">AvgUserLength</span><span class="sxs-lookup"><span data-stu-id="a4578-333">AvgUserLength</span></span> |<span data-ttu-id="a4578-334">Průměrný počet jedinečných položek na uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-334">Average number of distinct items per user.</span></span> |
| <span data-ttu-id="a4578-335">DensificationNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="a4578-335">DensificationNumberOfItems</span></span> |<span data-ttu-id="a4578-336">Počet položek po vyřazení položky, které nelze modelována.</span><span class="sxs-lookup"><span data-stu-id="a4578-336">Number of items after pruning items that cannot be modelled.</span></span> |
| <span data-ttu-id="a4578-337">DensificationNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="a4578-337">DensificationNumberOfUsers</span></span> |<span data-ttu-id="a4578-338">Počet bodů využití po vyřazení uživatelů a položky, které nelze modelována.</span><span class="sxs-lookup"><span data-stu-id="a4578-338">Number of usage points after pruning users and items that can't be modelled.</span></span> |
| <span data-ttu-id="a4578-339">DensificationNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="a4578-339">DensificationNumberOfRecords</span></span> |<span data-ttu-id="a4578-340">Počet bodů využití po vyřazení uživatelů a položky, které nelze modelována.</span><span class="sxs-lookup"><span data-stu-id="a4578-340">Number of usage points after pruning users and items that can't be modelled.</span></span> |
| <span data-ttu-id="a4578-341">MaxItemLength</span><span class="sxs-lookup"><span data-stu-id="a4578-341">MaxItemLength</span></span> |<span data-ttu-id="a4578-342">Počet jedinečných uživatelů pro nejoblíbenější položku.</span><span class="sxs-lookup"><span data-stu-id="a4578-342">Number of distinct users for the most popular item.</span></span> |
| <span data-ttu-id="a4578-343">MaxUserLength</span><span class="sxs-lookup"><span data-stu-id="a4578-343">MaxUserLength</span></span> |<span data-ttu-id="a4578-344">Maximální počet jedinečných položek pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-344">Maximal number of distinct items for a user.</span></span> |
| <span data-ttu-id="a4578-345">MinItemLength</span><span class="sxs-lookup"><span data-stu-id="a4578-345">MinItemLength</span></span> |<span data-ttu-id="a4578-346">Maximální počet jedinečných uživatelů pro položku.</span><span class="sxs-lookup"><span data-stu-id="a4578-346">Maximal number of distinct users for an item.</span></span> |
| <span data-ttu-id="a4578-347">MinUserLength</span><span class="sxs-lookup"><span data-stu-id="a4578-347">MinUserLength</span></span> |<span data-ttu-id="a4578-348">Minimální počet jedinečných položek pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-348">Minimal number of distinct items for a user.</span></span> |
| <span data-ttu-id="a4578-349">RawNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="a4578-349">RawNumberOfItems</span></span> |<span data-ttu-id="a4578-350">Počet položek v souborech použití.</span><span class="sxs-lookup"><span data-stu-id="a4578-350">Number of items in the usage files.</span></span> |
| <span data-ttu-id="a4578-351">RawNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="a4578-351">RawNumberOfUsers</span></span> |<span data-ttu-id="a4578-352">Počet bodů využití před všechny vyřazení.</span><span class="sxs-lookup"><span data-stu-id="a4578-352">Number of usage points before any pruning.</span></span> |
| <span data-ttu-id="a4578-353">RawNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="a4578-353">RawNumberOfRecords</span></span> |<span data-ttu-id="a4578-354">Počet bodů využití před všechny vyřazení.</span><span class="sxs-lookup"><span data-stu-id="a4578-354">Number of usage points before any pruning.</span></span> |
| <span data-ttu-id="a4578-355">SamplingNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="a4578-355">SamplingNumberOfItems</span></span> |<span data-ttu-id="a4578-356">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="a4578-356">N/A</span></span> |
| <span data-ttu-id="a4578-357">SamplingNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="a4578-357">SamplingNumberOfRecords</span></span> |<span data-ttu-id="a4578-358">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="a4578-358">N/A</span></span> |
| <span data-ttu-id="a4578-359">SamplingNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="a4578-359">SamplingNumberOfUsers</span></span> |<span data-ttu-id="a4578-360">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="a4578-360">N/A</span></span> |

<span data-ttu-id="a4578-361">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-361">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get data insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgItemLength</d:Key>
        <d:Value m:type="Edm.String">18.936</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgUserLength</d:Key>
        <d:Value m:type="Edm.String">51.879</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfItems</d:Key>
        <d:Value m:type="Edm.String">2,000</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfRecords</d:Key>
        <d:Value m:type="Edm.String">37,872</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
        <m:properties>
            <d:Key m:type="Edm.String">DensificationNumberOfUsers</d:Key>
            <d:Value m:type="Edm.String">730</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxItemLength</d:Key>
                <d:Value m:type="Edm.String">4,704</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxUserLength</d:Key>
                <d:Value m:type="Edm.String">190</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinItemLength</d:Key>
                <d:Value m:type="Edm.String">8</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinUserLength</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="62----model-insight"></a><span data-ttu-id="a4578-362">6.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-362">6.2.</span></span>    <span data-ttu-id="a4578-363">Přehled modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-363">Model Insight</span></span>
<span data-ttu-id="a4578-364">Vrátí model náhled na aktivní sestavení nebo (je-li zadána) na konkrétní sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-364">Returns model insight on the active build or (if given) on a specific build.</span></span>

<span data-ttu-id="a4578-365">K dispozici pouze pro sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-365">Available only for Recommendation build.</span></span>

| <span data-ttu-id="a4578-366">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-366">HTTP Method</span></span> | <span data-ttu-id="a4578-367">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-367">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-368">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-368">GET</span></span> |<span data-ttu-id="a4578-369">S ID aktivního sestavení:</span><span class="sxs-lookup"><span data-stu-id="a4578-369">With active build ID:</span></span><br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-370">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-370">Example:</span></span><br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-371">S konkrétní sestavovací ID:</span><span class="sxs-lookup"><span data-stu-id="a4578-371">With specific build ID:</span></span><br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-372">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-372">Parameter Name</span></span> | <span data-ttu-id="a4578-373">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-373">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-374">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-374">modelId</span></span> |<span data-ttu-id="a4578-375">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-375">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-376">buildId</span><span class="sxs-lookup"><span data-stu-id="a4578-376">buildId</span></span> |<span data-ttu-id="a4578-377">Volitelné – číslo, které identifikuje úspěšném sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-377">Optional - number that identifies a successful build.</span></span> |
| <span data-ttu-id="a4578-378">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-378">apiVersion</span></span> |<span data-ttu-id="a4578-379">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-379">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-380">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-380">Request Body</span></span> |<span data-ttu-id="a4578-381">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-381">NONE</span></span> |

<span data-ttu-id="a4578-382">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-382">**Response**:</span></span>

<span data-ttu-id="a4578-383">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-383">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-384">Data se vrátí jako kolekci vlastností.</span><span class="sxs-lookup"><span data-stu-id="a4578-384">The data is returned as a collection of properties.</span></span>

* `feed/entry/id/content/properties/key`
* `feed/entry/id/content/properties/value`

<span data-ttu-id="a4578-385">Následující tabulka znázorňuje hodnotu, která představuje každý klíč.</span><span class="sxs-lookup"><span data-stu-id="a4578-385">The table below depicts the value that each key represents.</span></span>

| <span data-ttu-id="a4578-386">Klíč</span><span class="sxs-lookup"><span data-stu-id="a4578-386">Key</span></span> | <span data-ttu-id="a4578-387">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-387">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-388">CatalogCoverage</span><span class="sxs-lookup"><span data-stu-id="a4578-388">CatalogCoverage</span></span> |<span data-ttu-id="a4578-389">Jaké součástí katalogu můžete modelována s vzorce používání.</span><span class="sxs-lookup"><span data-stu-id="a4578-389">What part of the catalog can be modelled with usage patterns.</span></span> <span data-ttu-id="a4578-390">Zbývající položky potřebovat funkce založené na obsah.</span><span class="sxs-lookup"><span data-stu-id="a4578-390">The rest of the items will need content-based features.</span></span> |
| <span data-ttu-id="a4578-391">Pravidlem zásad správy</span><span class="sxs-lookup"><span data-stu-id="a4578-391">Mpr</span></span> |<span data-ttu-id="a4578-392">Střední percentilu hodnocení modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-392">Mean percentile ranking of the model.</span></span> <span data-ttu-id="a4578-393">Nižší je lepší.</span><span class="sxs-lookup"><span data-stu-id="a4578-393">Lower is better.</span></span> |
| <span data-ttu-id="a4578-394">NumberOfDimensions</span><span class="sxs-lookup"><span data-stu-id="a4578-394">NumberOfDimensions</span></span> |<span data-ttu-id="a4578-395">Počet dimenzí používá algoritmus factorization matice.</span><span class="sxs-lookup"><span data-stu-id="a4578-395">Number of dimensions used by the matrix factorization algorithm.</span></span> |

<span data-ttu-id="a4578-396">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-396">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get model insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">CatalogCoverage</d:Key>
                <d:Value m:type="Edm.String">1.000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">Mpr</d:Key>
                <d:Value m:type="Edm.String">0.367</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">NumberOfDimensions</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="63----get-model-sample"></a><span data-ttu-id="a4578-397">6.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-397">6.3.</span></span>    <span data-ttu-id="a4578-398">Získat ukázky modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-398">Get Model Sample</span></span>
<span data-ttu-id="a4578-399">Získá ukázku modelu doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-399">Gets a sample of the recommendation model.</span></span>

| <span data-ttu-id="a4578-400">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-400">HTTP Method</span></span> | <span data-ttu-id="a4578-401">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-401">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-402">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-402">GET</span></span> |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="a4578-403">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-403">Example:</span></span><br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-404">S konkrétní sestavovací ID:</span><span class="sxs-lookup"><span data-stu-id="a4578-404">With specific build ID:</span></span><br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="a4578-405">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-405">Example:</span></span><br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-406">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-406">Parameter Name</span></span> | <span data-ttu-id="a4578-407">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-407">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-408">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-408">modelId</span></span> |<span data-ttu-id="a4578-409">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-409">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-410">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-410">apiVersion</span></span> |<span data-ttu-id="a4578-411">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-411">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-412">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-412">Request Body</span></span> |<span data-ttu-id="a4578-413">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-413">NONE</span></span> |

<span data-ttu-id="a4578-414">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-414">**Response**:</span></span>

<span data-ttu-id="a4578-415">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-415">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-416">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-416">OData XML</span></span>

<span data-ttu-id="a4578-417">Odpověď se vrátí ve formátu raw textu:</span><span class="sxs-lookup"><span data-stu-id="a4578-417">Response is returned in raw text format:</span></span>

<pre>
Level 1
---------------
655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel
    655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel Rating: 0.5215
    3f471802-f84f-44a0-99c8-6d2e7418eec1, Black Hawk Down: A Story of Modern War Rating: 0.5151
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5148
    6afc18e4-8c2a-43d1-9021-57543d6b11d8, Imajica Rating: 0.5146
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.514
56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai
    56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai Rating: 0.5218
    53156702-cc0c-443d-b718-6fb74b2491d3, Son of \ Rating: 0.5212
    fb8cf7a6-8719-46ee-97d4-92f931d77a3a, Smoke and Mirrors: Short Fictions and Illusions Rating: 0.5188
    8f5fe006-79e4-4679-816b-950989d1db4b, A Place I've Never Been (Contemporary American Fiction) Rating: 0.5156
    d8db4583-cc0f-49ce-bc95-b7fa3491623f, Happiness: A Novel Rating: 0.5156
50471eec-9aeb-4900-84d7-21567ab18546, If the Buddha Dated: A Handbook for Finding Love on a Spiritual Path
    cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, Divine Secrets of the Ya-Ya Sisterhood: A Novel Rating: 0.5266
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5252
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5244
    e2cbf7ad-0636-4117-8b30-298da6df7077, Animal Dreams Rating: 0.5227
    6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Confessions of an Ugly Stepsister: A Novel Rating: 0.5222
5e97148f-defb-4d74-af2d-80f4763bf531, The Deep End of the Ocean (Oprah's Book Club)
    5e97148f-defb-4d74-af2d-80f4763bf531, The Deep End of the Ocean (Oprah's Book Club) Rating: 0.537
    5dcbac37-2946-4f2a-a0b3-bbe710f9409a, Up Island: A Novel Rating: 0.5277
    bc5b69db-733b-4346-adde-3927544258f7, Downtown Rating: 0.5275
    31fe5c63-3e5a-48d0-802b-d3b0f989a634, Have a Nice Day: A Tale of Blood and Sweatsocks Rating: 0.5252
    0adf981a-b65b-4c11-b36b-78aca2f948a2, The Perfect Storm: A True Story of Men Against the Sea Rating: 0.5238
68f97068-ae1a-4163-9e94-396b800b743d, Modoc: The True Story of the Greatest Elephant That Ever Lived
    68f97068-ae1a-4163-9e94-396b800b743d, Modoc: The True Story of the Greatest Elephant That Ever Lived Rating: 0.5379
    6724862e-e4e7-4022-9614-1468d8b902ff, Little House on the Prairie Rating: 0.5345
    cdedb837-1620-496d-94c4-6ccfed888320, Little House in the Big Woods Rating: 0.5325
    382164ba-406b-4187-b726-d7a54b9d790d, The Tao of Pooh Rating: 0.5309
    6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5, On the Banks of Plum Creek Rating: 0.5285
37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships
    37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars, Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships Rating: 0.5397
    f2be16d4-5faf-4d32-ab83-7ba74d29261e, Politically Correct Bedtime Stories: Modern Tales for Our Life and Times Rating: 0.5207
    ef732c5c-334b-4d6b-ab82-7255eb7286d0, Honor Among Thieves Rating: 0.5195
    0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, The Giving Tree Rating: 0.5194
    883b360f-8b42-407f-b977-2f44ad840877, Scary Stories to Tell in the Dark: Collected from American Folklore (Scary Stories) Rating: 0.5184
ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: The Craft of Baseball
    d008dae9-c73a-40a1-9a9b-96d5cf546f36, The Gulag Archipelago 1918-1956: An Experiment in Literary Investigation I-II Rating: 0.5416
    ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: The Craft of Baseball Rating: 0.5403
    49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland Rating: 0.5394
    cc7964fd-d30f-478e-a425-93ddbdf094ed, Magic the Gathering: Arena Vol. 1 Rating: 0.5379
    8a1e9f36-97af-4614-bed9-24e3940a05f3, More Sniglets: Any Word That Doesn't Appear in the Dictionary but Should Rating: 0.5377
12a6d988-be21-4a09-8143-9d5f4261ba16, A Dream of Eagles
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5417
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.5416
    1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, The Family Rating: 0.5371
    56daeffe-7d48-43cd-8ef8-7dffd0c103d3, Kilo Class Rating: 0.5366
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5366
df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish
    56d33036-dfda-46b9-8e2a-76cb03921bb0, The X-Files: Ground Zero Rating: 0.5417
    0780cde8-6529-4e1d-b6c6-082c1b80e596, Twelve Red Herrings Rating: 0.5416
    df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish Rating: 0.5408
    400fe331-2c35-490c-adbc-b28b4b73d56c, Shall We Tell the President? Rating: 0.5383
    f86ad7d0-5c03-42b3-aebf-13d44aec8b30, Shades of Grace Rating: 0.5358
de1f62a4-89e6-44d2-aaee-992a4bf093f1, The Map That Changed the World: William Smith and the Birth of Modern Geology
    de1f62a4-89e6-44d2-aaee-992a4bf093f1, The Map That Changed the World: William Smith and the Birth of Modern Geology Rating: 0.5422
    b303538f-e2c6-4a2c-b425-8d21e684fc3e, My Uncle Oswald Rating: 0.5385
    34b84627-48af-4a4c-96c4-b26fb3863f56, Midnight In the Garden of Good and Evil Rating: 0.5379
    306cbaa7-b1a8-4142-9d55-e11b5018a7a8, The Street Lawyer Rating: 0.5376
    e53b4baa-8c09-45c4-95c0-b6a26b98770b, Miss Smillas Feeling for Snow Rating: 0.5367

Level 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, The Sinister Pig (Hillerman Tony)
    352aaea1-6b12-454d-a3d5-46379d9e4eb2, The Sinister Pig (Hillerman Tony) Rating: 0.5425
    74c49398-bc10-4af5-a658-a996a1201254, Children of the Storm (Peters Elizabeth) Rating: 0.5387
    9ba80080-196e-43fd-8025-391d963f77e7, The Floating Girl Rating: 0.5372
    e68f81d5-7745-4cc7-b943-fedb8fcc2ced, Killer Smile (Scottoline Lisa) Rating: 0.5353
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5332
c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days
    0adf981a-b65b-4c11-b36b-78aca2f948a2, The Perfect Storm: A True Story of Men Against the Sea Rating: 0.5433
    c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days Rating: 0.543
    a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: Any Word That Doesn't Appear in the Dictionary But Should) Rating: 0.5327
    6f6e192e-0d64-49ca-9b63-f09413ea1ee6, Politically Correct Holiday Stories: For an Enlightened Yuletide Season Rating: 0.5307
    798051a8-147d-4d46-b0dc-e836325029e6, AGE OF INNOCENCE (MOVIE TIE-IN) Rating: 0.5301
73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics)
    cba8163f-6536-436b-8130-47b4a43c827f, Trust No One (The Official Guide to the X-Files Vol. 2) Rating: 0.5434
    5708e4cb-2492-49c0-94a8-cc413eec5d89, Small Gods (Discworld Novels (Paperback)) Rating: 0.5406
    73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics) Rating: 0.5403
    d885b0bd-ae4b-452d-bdf2-faa90197dbc9, The Color of Magic Rating: 0.539
    b133a9c4-4784-4db3-b100-d0d6dffb94d2, The Truth Is Out There (The Official Guide to the X-Files Vol. 1) Rating: 0.5367
271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why the Winged Whale Sings
    271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why the Winged Whale Sings Rating: 0.5445
    2de1c354-90ff-47c5-a0db-1bad7d88ef94, The Salaryman's Wife (Children of Violence Series) Rating: 0.5329
    d279416e-19c0-43f8-9ec9-a585947879ca, Zen Attitude Rating: 0.5316
    c8f854d7-3de3-4b23-8217-f4f851670fd4, Revenge of the Cootie Girls: A Robin Hudson Mystery (Robin Hudson Mysteries (Paperback)) Rating: 0.5305
    8ef4751c-7074-409e-a3ac-d49b222fc864, Where the Wild Things Are Rating: 0.5289
9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God
    9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God Rating: 0.5446
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, The Bean Trees Rating: 0.5389
    65ecbdd1-131c-40c3-a3d6-d86ca281377a, The God of Small Things Rating: 0.5387
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, The Stone Diaries Rating: 0.5355
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5344
5f17d90a-2604-4fe8-8977-1a280b9098b1, One for the Money (Stephanie Plum Novels (Paperback))
    5f17d90a-2604-4fe8-8977-1a280b9098b1, One for the Money (Stephanie Plum Novels (Paperback)) Rating: 0.5446
    57169b2b-9a8a-486b-9aac-1ed98ce57168, Final Appeal Rating: 0.5332
    efcb1bc4-7278-4a8f-b491-befde02070d6, Moment of Truth Rating: 0.5329
    1efa91a2-993b-4c43-9f5c-3454fc12612d, Burn Factor Rating: 0.5309
    24c59962-458a-4ec8-b95d-d694e861919c, At Home in Mitford (The Mitford Years) Rating: 0.5303
4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: The Boy Who Was Raised As a Girl
    4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: The Boy Who Was Raised As a Girl Rating: 0.5449
    cd5f2c03-20cb-43be-a1fb-3b4233e63222, Pigs in Heaven Rating: 0.5329
    19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, Love in the Time of Cholera (Penguin Great Books of the 20th Century) Rating: 0.5267
    15689d09-c711-4844-84d8-130a90237b26, Bel Canto Rating: 0.5245
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5235
98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories
    f874b5a3-5d40-4436-94ff-0fa1c090ddf5, The Sun Also Rises (A Scribner classic) Rating: 0.5451
    98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories Rating: 0.5442
    0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Unseen Rating: 0.5421
    15316ca6-1e38-425f-893d-691944a47000, More Scary Stories To Tell In The Dark Rating: 0.5409
    329d5682-3dc3-4206-8aa2-eef4b1032258, Letters from the Earth Rating: 0.54
5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover))
    5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover)) Rating: 0.5462
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5372
    604eb3bd-6026-4f51-bffd-9fb54f180400, Family Pictures: A Novel Rating: 0.5341
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5334
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, The Bean Trees Rating: 0.5319
d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven
    d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven Rating: 0.5491
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5401
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, The Stone Diaries Rating: 0.5393
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5382
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5367

</pre>


## <a name="7-model-business-rules"></a><span data-ttu-id="a4578-418">7. Model obchodní pravidla</span><span class="sxs-lookup"><span data-stu-id="a4578-418">7. Model Business Rules</span></span>
<span data-ttu-id="a4578-419">Toto jsou typy pravidel, které jsou podporovány:</span><span class="sxs-lookup"><span data-stu-id="a4578-419">These are the types of rules supported:</span></span>

* <span data-ttu-id="a4578-420"><strong>BlockList</strong> -BlockList umožňuje zadat seznam položek, které nechcete vracet ve výsledcích doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-420"><strong>BlockList</strong> - BlockList enables you to provide a list of items that you do not want to return in the recommendation results.</span></span> 
* <span data-ttu-id="a4578-421"><strong>FeatureBlockList</strong> -BlockList funkce umožňuje blokovat položek na základě hodnot její funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-421"><strong>FeatureBlockList</strong> - Feature BlockList enables you to block items based on the values of its features.</span></span>

<span data-ttu-id="a4578-422">*Neodesílat více než 1 000 položek v jednom blocklist pravidlo nebo volání může časový limit. Pokud potřebujete blokovat více než 1 000 položek, můžete provést několik blocklist volání.*</span><span class="sxs-lookup"><span data-stu-id="a4578-422">*Do not send more than 1000 items in a single blocklist rule or your call may timeout. If you need to block more than 1000 items, you can make several blocklist calls.*</span></span>

* <span data-ttu-id="a4578-423"><strong>Upsale</strong> -Upsale umožňuje vynucovat položek k vrácení ve výsledcích doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-423"><strong>Upsale</strong> - Upsale enables you to enforce items to return in the recommendation results.</span></span>
* <span data-ttu-id="a4578-424"><strong>Seznam povolených adres</strong> -seznamu povolených vám umožní pouze navrhovat doporučení ze seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="a4578-424"><strong>WhiteList</strong> - White List enables you to only suggest recommendations from a list of items.</span></span>
* <span data-ttu-id="a4578-425"><strong>FeatureWhiteList</strong> – seznam povolených funkcí umožňuje pouze doporučujeme položky, které mají hodnoty příslušné funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-425"><strong>FeatureWhiteList</strong> - Feature White List enables you to only recommend items that have specific feature values.</span></span>
* <span data-ttu-id="a4578-426"><strong>PerSeedBlockList</strong> -na seznam blokovaných počáteční hodnoty můžete zadat na položku seznam položek, které nemůže být vrácen jako doporučení výsledky.</span><span class="sxs-lookup"><span data-stu-id="a4578-426"><strong>PerSeedBlockList</strong> - Per Seed Block List enables you to provide per item a list of items that cannot be returned as recommendation results.</span></span>

### <a name="71----get-model-rules"></a><span data-ttu-id="a4578-427">7.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-427">7.1.</span></span>    <span data-ttu-id="a4578-428">Získat pravidla modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-428">Get Model Rules</span></span>
| <span data-ttu-id="a4578-429">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-429">HTTP Method</span></span> | <span data-ttu-id="a4578-430">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-430">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-431">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-431">GET</span></span> |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="a4578-432">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-432">Example:</span></span><br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-433">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-433">Parameter Name</span></span> | <span data-ttu-id="a4578-434">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-434">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-435">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-435">modelId</span></span> |<span data-ttu-id="a4578-436">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-436">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-437">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-437">apiVersion</span></span> |<span data-ttu-id="a4578-438">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-438">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-439">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-439">Request Body</span></span> |<span data-ttu-id="a4578-440">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-440">NONE</span></span> |

<span data-ttu-id="a4578-441">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-441">**Response**:</span></span>

<span data-ttu-id="a4578-442">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-442">HTTP Status code: 200</span></span>

* <span data-ttu-id="a4578-443">`feed/entry/content/properties/Id`-Jedinečný identifikátor tohoto pravidla.</span><span class="sxs-lookup"><span data-stu-id="a4578-443">`feed/entry/content/properties/Id` - Unique identifier of this rule.</span></span>
* <span data-ttu-id="a4578-444">`feed/entry/content/properties/Type`-Typ pravidla.</span><span class="sxs-lookup"><span data-stu-id="a4578-444">`feed/entry/content/properties/Type` - Type of the rule.</span></span>
* <span data-ttu-id="a4578-445">`feed/entry/content/properties/Parameter`-Parametr pravidla.</span><span class="sxs-lookup"><span data-stu-id="a4578-445">`feed/entry/content/properties/Parameter` - Rule parameter.</span></span>

<span data-ttu-id="a4578-446">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-446">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get a list of rules for a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T12:58:57Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000043</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1000"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000044</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1001"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="72----add-rule"></a><span data-ttu-id="a4578-447">7.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-447">7.2.</span></span>    <span data-ttu-id="a4578-448">Přidat pravidlo</span><span class="sxs-lookup"><span data-stu-id="a4578-448">Add Rule</span></span>
| <span data-ttu-id="a4578-449">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-449">HTTP Method</span></span> | <span data-ttu-id="a4578-450">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-450">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-451">POST</span><span class="sxs-lookup"><span data-stu-id="a4578-451">POST</span></span> |`<rootURI>/AddRule?apiVersion=%271.0%27` |
| <span data-ttu-id="a4578-452">ZÁHLAVÍ</span><span class="sxs-lookup"><span data-stu-id="a4578-452">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="a4578-453">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-453">Parameter Name</span></span> | <span data-ttu-id="a4578-454">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-454">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-455">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-455">apiVersion</span></span> |<span data-ttu-id="a4578-456">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-456">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-457">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-457">Request Body</span></span> | |

<span data-ttu-id="a4578-458"><ins>Vždy, když poskytnete ID položek obchodní pravidla, nezapomeňte použít externí Id položky (stejným Id, který jste použili v katalogu souboru)</ins></span><span class="sxs-lookup"><span data-stu-id="a4578-458"><ins>Whenever providing Item Ids for business rules, make sure to use the External Id of the item (the same Id that you used in the catalog file)</ins></span></span><br><span data-ttu-id="a4578-459">
<ins>Chcete-li přidat pravidlo BlockList:</ins></span><span class="sxs-lookup"><span data-stu-id="a4578-459">
<ins>To add a BlockList rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><span data-ttu-id="a4578-460"><ins>
<ins>Chcete-li přidat pravidlo FeatureBlockList:</ins></span><span class="sxs-lookup"><span data-stu-id="a4578-460"><ins>
<ins>To add a FeatureBlockList rule:</ins></span></span><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><span data-ttu-id="a4578-461"><ins>Chcete-li přidat pravidlo Upsale:</ins></span><span class="sxs-lookup"><span data-stu-id="a4578-461"><ins> To add an Upsale rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br><span data-ttu-id="a4578-462">
<ins>Chcete-li přidat pravidlo seznamu povolených IP adres:</ins></span><span class="sxs-lookup"><span data-stu-id="a4578-462">
<ins>To add a WhiteList rule:</ins></span></span><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><span data-ttu-id="a4578-463"><ins>
<ins>Chcete-li přidat pravidlo FeatureWhiteList:</ins></span><span class="sxs-lookup"><span data-stu-id="a4578-463"><ins>
<ins>To add a FeatureWhiteList rule:</ins></span></span><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><span data-ttu-id="a4578-464"><ins>Chcete-li přidat pravidlo PerSeedBlockList:</ins></span><span class="sxs-lookup"><span data-stu-id="a4578-464"><ins> To add a PerSeedBlockList rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|

<span data-ttu-id="a4578-465">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-465">**Response**:</span></span>

<span data-ttu-id="a4578-466">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-466">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-467">Rozhraní API vrátí nově vytvořeného pravidla s jeho podrobnostmi.</span><span class="sxs-lookup"><span data-stu-id="a4578-467">The API returns the newly created rule with its details.</span></span> <span data-ttu-id="a4578-468">Vlastnost pravidla lze načíst z následující cesty:</span><span class="sxs-lookup"><span data-stu-id="a4578-468">The rules property can be retrieved from the following paths:</span></span>

* <span data-ttu-id="a4578-469">`feed/entry/content/properties/Id`-Jedinečný identifikátor tohoto pravidla.</span><span class="sxs-lookup"><span data-stu-id="a4578-469">`feed/entry/content/properties/Id` - Unique identifier of this rule.</span></span>
* <span data-ttu-id="a4578-470">`feed/entry/content/properties/Type`-Typ pravidla: BlockList nebo Upsale.</span><span class="sxs-lookup"><span data-stu-id="a4578-470">`feed/entry/content/properties/Type` - Type of the rule: BlockList or Upsale.</span></span>
* <span data-ttu-id="a4578-471">`feed/entry/content/properties/Parameter`-Parametr pravidla.</span><span class="sxs-lookup"><span data-stu-id="a4578-471">`feed/entry/content/properties/Parameter` - Rule parameter.</span></span>

<span data-ttu-id="a4578-472">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-472">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add A Rule</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T11:13:28Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AddARuleEntity</title>
        <updated>2014-11-05T11:13:28Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000041</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1002"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="73----delete-rule"></a><span data-ttu-id="a4578-473">7.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-473">7.3.</span></span>    <span data-ttu-id="a4578-474">Odstranit pravidlo</span><span class="sxs-lookup"><span data-stu-id="a4578-474">Delete Rule</span></span>
| <span data-ttu-id="a4578-475">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-475">HTTP Method</span></span> | <span data-ttu-id="a4578-476">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-476">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-477">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="a4578-477">DELETE</span></span> |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-478">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-478">Example:</span></span><br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-479">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-479">Parameter Name</span></span> | <span data-ttu-id="a4578-480">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-480">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-481">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-481">modelId</span></span> |<span data-ttu-id="a4578-482">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-482">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-483">filterId</span><span class="sxs-lookup"><span data-stu-id="a4578-483">filterId</span></span> |<span data-ttu-id="a4578-484">Jedinečný identifikátor filtru</span><span class="sxs-lookup"><span data-stu-id="a4578-484">Unique identifier of the filter</span></span> |
| <span data-ttu-id="a4578-485">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-485">apiVersion</span></span> |<span data-ttu-id="a4578-486">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-486">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-487">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-487">Request Body</span></span> |<span data-ttu-id="a4578-488">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-488">NONE</span></span> |

<span data-ttu-id="a4578-489">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-489">**Response**:</span></span>

<span data-ttu-id="a4578-490">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-490">HTTP Status code: 200</span></span>

### <a name="74----delete-all-rules"></a><span data-ttu-id="a4578-491">7.4.</span><span class="sxs-lookup"><span data-stu-id="a4578-491">7.4.</span></span>    <span data-ttu-id="a4578-492">Odstranit všechna pravidla</span><span class="sxs-lookup"><span data-stu-id="a4578-492">Delete All Rules</span></span>
| <span data-ttu-id="a4578-493">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-493">HTTP Method</span></span> | <span data-ttu-id="a4578-494">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-494">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-495">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="a4578-495">DELETE</span></span> |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-496">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-496">Example:</span></span><br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-497">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-497">Parameter Name</span></span> | <span data-ttu-id="a4578-498">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-498">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-499">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-499">modelId</span></span> |<span data-ttu-id="a4578-500">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-500">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-501">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-501">apiVersion</span></span> |<span data-ttu-id="a4578-502">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-502">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-503">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-503">Request Body</span></span> |<span data-ttu-id="a4578-504">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-504">NONE</span></span> |

<span data-ttu-id="a4578-505">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-505">**Response**:</span></span>

<span data-ttu-id="a4578-506">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-506">HTTP Status code: 200</span></span>

## <a name="8-catalog"></a><span data-ttu-id="a4578-507">8. Katalogu</span><span class="sxs-lookup"><span data-stu-id="a4578-507">8. Catalog</span></span>
### <a name="81----import-catalog-data"></a><span data-ttu-id="a4578-508">8.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-508">8.1.</span></span>    <span data-ttu-id="a4578-509">Umožňuje importovat Data Catalog</span><span class="sxs-lookup"><span data-stu-id="a4578-509">Import Catalog Data</span></span>
<span data-ttu-id="a4578-510">Pokud nahrajete několik katalog souborů do stejného modelu s několika volání, jsme vloží nové položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-510">If you upload several catalog files to the same model with several calls, we will insert only the new catalog items.</span></span> <span data-ttu-id="a4578-511">Existující položky zůstane s původními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="a4578-511">Existing items will remain with the original values.</span></span> <span data-ttu-id="a4578-512">Data katalogu nelze aktualizovat pomocí této metody.</span><span class="sxs-lookup"><span data-stu-id="a4578-512">You cannot update catalog data by using this method.</span></span>

<span data-ttu-id="a4578-513">Data katalogu postupujte podle následujícího formátu:</span><span class="sxs-lookup"><span data-stu-id="a4578-513">The catalog data should follow the following format:</span></span>

* <span data-ttu-id="a4578-514">Bez funkce –`<Item Id>,<Item Name>,<Item Category>[,<Description>]`</span><span class="sxs-lookup"><span data-stu-id="a4578-514">Without features - `<Item Id>,<Item Name>,<Item Category>[,<Description>]`</span></span>
* <span data-ttu-id="a4578-515">S funkcemi-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`</span><span class="sxs-lookup"><span data-stu-id="a4578-515">With features - `<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`</span></span>

<span data-ttu-id="a4578-516">Poznámka: Maximální velikost souboru je 200MB.</span><span class="sxs-lookup"><span data-stu-id="a4578-516">Note: The maximum file size is 200MB.</span></span>

<span data-ttu-id="a4578-517">** Formát podrobností **</span><span class="sxs-lookup"><span data-stu-id="a4578-517">** Format details **</span></span>

| <span data-ttu-id="a4578-518">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a4578-518">Name</span></span> | <span data-ttu-id="a4578-519">Povinné</span><span class="sxs-lookup"><span data-stu-id="a4578-519">Mandatory</span></span> | <span data-ttu-id="a4578-520">Typ</span><span class="sxs-lookup"><span data-stu-id="a4578-520">Type</span></span> | <span data-ttu-id="a4578-521">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-521">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a4578-522">Id položky</span><span class="sxs-lookup"><span data-stu-id="a4578-522">Item Id</span></span> |<span data-ttu-id="a4578-523">Ano</span><span class="sxs-lookup"><span data-stu-id="a4578-523">Yes</span></span> |<span data-ttu-id="a4578-524">[A-z], [-z], [0-9] [_] &#40; Podtržítko &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="a4578-524">[A-z], [a-z], [0-9], [_] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="a4578-525">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="a4578-525">Max length: 50</span></span> |<span data-ttu-id="a4578-526">Jedinečný identifikátor položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-526">Unique identifier of an item.</span></span> |
| <span data-ttu-id="a4578-527">Název položky</span><span class="sxs-lookup"><span data-stu-id="a4578-527">Item Name</span></span> |<span data-ttu-id="a4578-528">Ano</span><span class="sxs-lookup"><span data-stu-id="a4578-528">Yes</span></span> |<span data-ttu-id="a4578-529">Žádné alfanumerické znaky</span><span class="sxs-lookup"><span data-stu-id="a4578-529">Any alphanumeric characters</span></span><br> <span data-ttu-id="a4578-530">Maximální délka: 255</span><span class="sxs-lookup"><span data-stu-id="a4578-530">Max length: 255</span></span> |<span data-ttu-id="a4578-531">Název položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-531">Item name.</span></span> |
| <span data-ttu-id="a4578-532">Kategorie položky</span><span class="sxs-lookup"><span data-stu-id="a4578-532">Item Category</span></span> |<span data-ttu-id="a4578-533">Ano</span><span class="sxs-lookup"><span data-stu-id="a4578-533">Yes</span></span> |<span data-ttu-id="a4578-534">Žádné alfanumerické znaky</span><span class="sxs-lookup"><span data-stu-id="a4578-534">Any alphanumeric characters</span></span> <br> <span data-ttu-id="a4578-535">Maximální délka: 255</span><span class="sxs-lookup"><span data-stu-id="a4578-535">Max length: 255</span></span> |<span data-ttu-id="a4578-536">Kategorie, do které patří tato položka (například vaření knihy, obrázkům...); nesmí být prázdné.</span><span class="sxs-lookup"><span data-stu-id="a4578-536">Category to which this item belongs (e.g. Cooking Books, Drama…); can be empty.</span></span> |
| <span data-ttu-id="a4578-537">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-537">Description</span></span> |<span data-ttu-id="a4578-538">Ne, pokud funkce jsou k dispozici (ale nesmí být prázdné)</span><span class="sxs-lookup"><span data-stu-id="a4578-538">No, unless features are present (but can be empty)</span></span> |<span data-ttu-id="a4578-539">Žádné alfanumerické znaky</span><span class="sxs-lookup"><span data-stu-id="a4578-539">Any alphanumeric characters</span></span> <br> <span data-ttu-id="a4578-540">Maximální délka: 4000</span><span class="sxs-lookup"><span data-stu-id="a4578-540">Max length: 4000</span></span> |<span data-ttu-id="a4578-541">Popis této položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-541">Description of this item.</span></span> |
| <span data-ttu-id="a4578-542">Seznam funkcí</span><span class="sxs-lookup"><span data-stu-id="a4578-542">Features list</span></span> |<span data-ttu-id="a4578-543">Ne</span><span class="sxs-lookup"><span data-stu-id="a4578-543">No</span></span> |<span data-ttu-id="a4578-544">Žádné alfanumerické znaky</span><span class="sxs-lookup"><span data-stu-id="a4578-544">Any alphanumeric characters</span></span> <br> <span data-ttu-id="a4578-545">Maximální délka: 4000; Maximální počet funkcí: 20</span><span class="sxs-lookup"><span data-stu-id="a4578-545">Max length: 4000; Max number of features:20</span></span> |<span data-ttu-id="a4578-546">Čárkami oddělený seznam název funkce = hodnota funkce, který slouží k vylepšení modelu doporučení; v tématu [Advanced témata](#2-advanced-topics) části.</span><span class="sxs-lookup"><span data-stu-id="a4578-546">Comma-separated list of feature name=feature value that can be used to enhance model recommendation; see [Advanced topics](#2-advanced-topics) section.</span></span> |

| <span data-ttu-id="a4578-547">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-547">HTTP Method</span></span> | <span data-ttu-id="a4578-548">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-548">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-549">POST</span><span class="sxs-lookup"><span data-stu-id="a4578-549">POST</span></span> |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-550">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-550">Example:</span></span><br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |
| <span data-ttu-id="a4578-551">ZÁHLAVÍ</span><span class="sxs-lookup"><span data-stu-id="a4578-551">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="a4578-552">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-552">Parameter Name</span></span> | <span data-ttu-id="a4578-553">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-553">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-554">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-554">modelId</span></span> |<span data-ttu-id="a4578-555">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-555">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-556">Název souboru</span><span class="sxs-lookup"><span data-stu-id="a4578-556">filename</span></span> |<span data-ttu-id="a4578-557">Textové identifikátor katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-557">Textual identifier of the catalog.</span></span><br><span data-ttu-id="a4578-558">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="a4578-558">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="a4578-559">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="a4578-559">Max length: 50</span></span> |
| <span data-ttu-id="a4578-560">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-560">apiVersion</span></span> |<span data-ttu-id="a4578-561">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-561">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-562">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-562">Request Body</span></span> |<span data-ttu-id="a4578-563">Příklad (s funkcí):</span><span class="sxs-lookup"><span data-stu-id="a4578-563">Example (with features):</span></span><br/><span data-ttu-id="a4578-564">vytváření 2406e770-769c-4189-89de-1c9283f93a96, Clara Callan adresáře, popis knihy = Richard Wright vydavatele = Harper Flamingo Kanada, rok = 2001</span><span class="sxs-lookup"><span data-stu-id="a4578-564">2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book,the book  description,author=Richard Wright,publisher=Harper Flamingo Canada,year=2001</span></span><br><span data-ttu-id="a4578-565">21bf8088-b6c0-4509-870c-e1c7ac78304a, místnosti zapomenutí: vytváření A Fiction (Byzantium kniha), kniha,, = Nick Bantock vydavatele = Harpercollins, rok = 1997</span><span class="sxs-lookup"><span data-stu-id="a4578-565">21bf8088-b6c0-4509-870c-e1c7ac78304a,The Forgetting Room: A Fiction (Byzantium Book),Book,,author=Nick Bantock,publisher=Harpercollins,year=1997</span></span><br><span data-ttu-id="a4578-566">3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, kniha,, vytvářet = Alexander Findley vydavatele = HarperFlamingo Kanada rok = 2001</span><span class="sxs-lookup"><span data-stu-id="a4578-566">3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book,,author=Timothy Findley, publisher=HarperFlamingo Canada, year=2001</span></span><br><span data-ttu-id="a4578-567">552a1940-21e4-4399-82bb-594b46d7ed54, omezení zvířata, kniha, popis knihy vytvářet = Magnus lisoven vydavatele = plošinová publikování rok = 1998</span><span class="sxs-lookup"><span data-stu-id="a4578-567">552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book,the book description,author=Magnus Mills, publisher=Arcade Publishing, year=1998</span></span></pre> |

<span data-ttu-id="a4578-568">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-568">**Response**:</span></span>

<span data-ttu-id="a4578-569">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-569">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-570">Rozhraní API vrátí sestavy importu.</span><span class="sxs-lookup"><span data-stu-id="a4578-570">The API returns a report of the import.</span></span>

* <span data-ttu-id="a4578-571">`feed\entry\content\properties\LineCount`-Přijaté počet řádků.</span><span class="sxs-lookup"><span data-stu-id="a4578-571">`feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="a4578-572">`feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby.</span><span class="sxs-lookup"><span data-stu-id="a4578-572">`feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due to an error.</span></span>

<span data-ttu-id="a4578-573">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-573">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Import catalog file</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
       <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ImportCatalogFileEntity2</title>
        <updated>2014-10-05T06:58:04Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:LineCount m:type="Edm.String">4</d:LineCount>
                <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="82----get-catalog"></a><span data-ttu-id="a4578-574">8.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-574">8.2.</span></span>    <span data-ttu-id="a4578-575">Získat katalogu</span><span class="sxs-lookup"><span data-stu-id="a4578-575">Get Catalog</span></span>
<span data-ttu-id="a4578-576">Načte všechny položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-576">Retrieves all catalog items.</span></span>
<span data-ttu-id="a4578-577">Katalog bude načtených v daný okamžik jednu stránku.</span><span class="sxs-lookup"><span data-stu-id="a4578-577">The catalog will be retrieved one page at a time.</span></span> <span data-ttu-id="a4578-578">Pokud chcete získat položky v konkrétním indexem, můžete použít parametr $skip odata.</span><span class="sxs-lookup"><span data-stu-id="a4578-578">If you want to get items at a particular index, you can use the $skip odata parameter.</span></span> <span data-ttu-id="a4578-579">Například pokud chcete získat položky začínající na pozici 100, přidejte parametr $skip = 100 na požadavek.</span><span class="sxs-lookup"><span data-stu-id="a4578-579">For instance if you want to get items starting at position 100, add the parameter $skip=100 to the request.</span></span>

| <span data-ttu-id="a4578-580">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-580">HTTP Method</span></span> | <span data-ttu-id="a4578-581">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-581">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-582">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-582">GET</span></span> |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-583">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-583">Example:</span></span><br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-584">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-584">Parameter Name</span></span> | <span data-ttu-id="a4578-585">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-585">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-586">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-586">modelId</span></span> |<span data-ttu-id="a4578-587">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-587">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-588">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-588">apiVersion</span></span> |<span data-ttu-id="a4578-589">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-589">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-590">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-590">Request Body</span></span> |<span data-ttu-id="a4578-591">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-591">NONE</span></span> |

<span data-ttu-id="a4578-592">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-592">**Response**:</span></span>

<span data-ttu-id="a4578-593">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-593">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-594">Odpověď obsahuje jeden záznam za položka katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-594">The response includes one entry per catalog item.</span></span> <span data-ttu-id="a4578-595">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-595">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-596">`feed/entry/content/properties/ExternalId`-ID položky katalogu externí, zadanému zákazník.</span><span class="sxs-lookup"><span data-stu-id="a4578-596">`feed/entry/content/properties/ExternalId` - Catalog item external ID, the one provided by the customer.</span></span>
* <span data-ttu-id="a4578-597">`feed/entry/content/properties/InternalId`-Katalogu interní ID položky, ten, který vygeneruje Azure Machine Learning doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-597">`feed/entry/content/properties/InternalId` - Catalog item internal ID, the one that Azure Machine Learning Recommendations has generated.</span></span>
* <span data-ttu-id="a4578-598">`feed/entry/content/properties/Name`-Název položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-598">`feed/entry/content/properties/Name` - Catalog item name.</span></span>
* <span data-ttu-id="a4578-599">`feed/entry/content/properties/Category`-Kategorie položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-599">`feed/entry/content/properties/Category` - Catalog item category.</span></span>
* <span data-ttu-id="a4578-600">`feed/entry/content/properties/Description`-Katalogu popis položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-600">`feed/entry/content/properties/Description` - Catalog item description.</span></span>
* <span data-ttu-id="a4578-601">`feed/entry/content/properties/Metadata`-Metadata položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-601">`feed/entry/content/properties/Metadata` - Catalog item metadata.</span></span>

<span data-ttu-id="a4578-602">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-602">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get All Catalog Items</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">552A1940-21E4-4399-82BB-594B46D7ED54</d:ExternalId>
                <d:InternalId m:type="Edm.String">060db26a-e6a6-464e-bb52-436d2da82a50</d:InternalId>
                <d:Name m:type="Edm.String">Restraint of Beasts</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:ExternalId>
                <d:InternalId m:type="Edm.String">209d7bfe-2eb9-4455-92a3-7c867a41a74a</d:InternalId>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">3BB5CB44-D143-4BDD-A55C-443964BF4B23</d:ExternalId>
                <d:InternalId m:type="Edm.String">913ff79b-059b-4d4d-8042-6fa4cc1391dd</d:InternalId>
                <d:Name m:type="Edm.String">Spadework</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">21BF8088-B6C0-4509-870C-E1C7AC78304A</d:ExternalId>
                <d:InternalId m:type="Edm.String">ea65e4fa-768c-40b4-92c3-69d3e8178691</d:InternalId>
                <d:Name m:type="Edm.String">The Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="83----get-catalog-items-by-token"></a><span data-ttu-id="a4578-603">8.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-603">8.3.</span></span>    <span data-ttu-id="a4578-604">Získat položky katalogu pomocí tokenu</span><span class="sxs-lookup"><span data-stu-id="a4578-604">Get Catalog Items by Token</span></span>
| <span data-ttu-id="a4578-605">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-605">HTTP Method</span></span> | <span data-ttu-id="a4578-606">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-606">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-607">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-607">GET</span></span> |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-608">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-608">Example:</span></span><br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-609">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-609">Parameter Name</span></span> | <span data-ttu-id="a4578-610">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-610">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-611">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-611">modelId</span></span> |<span data-ttu-id="a4578-612">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-612">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-613">Token</span><span class="sxs-lookup"><span data-stu-id="a4578-613">token</span></span> |<span data-ttu-id="a4578-614">Token názvu položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-614">Token of the catalog item’s name.</span></span> <span data-ttu-id="a4578-615">By mělo obsahovat aspoň 3 znaky.</span><span class="sxs-lookup"><span data-stu-id="a4578-615">Should contain at least 3 characters.</span></span> |
| <span data-ttu-id="a4578-616">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-616">apiVersion</span></span> |<span data-ttu-id="a4578-617">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-617">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-618">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-618">Request Body</span></span> |<span data-ttu-id="a4578-619">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-619">NONE</span></span> |

<span data-ttu-id="a4578-620">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-620">**Response**:</span></span>

<span data-ttu-id="a4578-621">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-621">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-622">Odpověď obsahuje jeden záznam za položka katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-622">The response includes one entry per catalog item.</span></span> <span data-ttu-id="a4578-623">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-623">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-624">`feed/entry/content/properties/InternalId`-Katalogu interní ID položky, ten, který vygeneruje Azure Machine Learning doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-624">`feed/entry/content/properties/InternalId` - Catalog item internal ID, the one that Azure Machine Learning Recommendations has generated.</span></span>
* <span data-ttu-id="a4578-625">`feed/entry/content/properties/Name`-Název položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-625">`feed/entry/content/properties/Name` - Catalog item name.</span></span>
* <span data-ttu-id="a4578-626">`feed/entry/content/properties/Rating`-(pro budoucí použití)</span><span class="sxs-lookup"><span data-stu-id="a4578-626">`feed/entry/content/properties/Rating` -  (for future use)</span></span>
* <span data-ttu-id="a4578-627">`feed/entry/content/properties/Reasoning`-(pro budoucí použití)</span><span class="sxs-lookup"><span data-stu-id="a4578-627">`feed/entry/content/properties/Reasoning` -  (for future use)</span></span>
* <span data-ttu-id="a4578-628">`feed/entry/content/properties/Metadata`-(pro budoucí použití)</span><span class="sxs-lookup"><span data-stu-id="a4578-628">`feed/entry/content/properties/Metadata` -  (for future use)</span></span>
* <span data-ttu-id="a4578-629">`feed/entry/content/properties/FormattedRating`-(pro budoucí použití)</span><span class="sxs-lookup"><span data-stu-id="a4578-629">`feed/entry/content/properties/FormattedRating` - (for future use)</span></span>

<span data-ttu-id="a4578-630">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-630">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get Catalog items that contain a token</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:48:19Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">CatalogItemsThatContainATokenEntity</title>
            <updated>2014-10-29T11:48:19Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                  <m:properties>
                    <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                    <d:Name m:type="Edm.String">Clara Callan</d:Name>
                    <d:Rating m:type="Edm.Double">0</d:Rating>
                    <d:Reasoning m:type="Edm.String"></d:Reasoning>
                    <d:Metadata m:type="Edm.String"></d:Metadata>
                    <d:FormattedRating m:type="Edm.Double" m:null="true"></d:FormattedRating>
                  </m:properties>
            </content>
        </entry>
    </feed>

## <a name="9-usage-data"></a><span data-ttu-id="a4578-631">9. Údaje o využití</span><span class="sxs-lookup"><span data-stu-id="a4578-631">9. Usage data</span></span>
### <a name="91----import-usage-data"></a><span data-ttu-id="a4578-632">9.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-632">9.1.</span></span>    <span data-ttu-id="a4578-633">Importovat Data o využití</span><span class="sxs-lookup"><span data-stu-id="a4578-633">Import Usage Data</span></span>
#### <a name="911-uploading-file"></a><span data-ttu-id="a4578-634">9.1.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-634">9.1.1.</span></span> <span data-ttu-id="a4578-635">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="a4578-635">Uploading File</span></span>
<span data-ttu-id="a4578-636">V této části ukazuje, jak odeslat data o využití pomocí souboru.</span><span class="sxs-lookup"><span data-stu-id="a4578-636">This section shows how to upload usage data by using a file.</span></span> <span data-ttu-id="a4578-637">Můžete volat toto rozhraní API se data o využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-637">You can call this API several times with usage data.</span></span> <span data-ttu-id="a4578-638">Všechna data o využití se uloží pro všechna volání.</span><span class="sxs-lookup"><span data-stu-id="a4578-638">All usage data will be saved for all calls.</span></span>

| <span data-ttu-id="a4578-639">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-639">HTTP Method</span></span> | <span data-ttu-id="a4578-640">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-640">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-641">POST</span><span class="sxs-lookup"><span data-stu-id="a4578-641">POST</span></span> |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-642">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-642">Example:</span></span><br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-643">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-643">Parameter Name</span></span> | <span data-ttu-id="a4578-644">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-644">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-645">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-645">modelId</span></span> |<span data-ttu-id="a4578-646">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-646">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-647">Název souboru</span><span class="sxs-lookup"><span data-stu-id="a4578-647">filename</span></span> |<span data-ttu-id="a4578-648">Textové identifikátor katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-648">Textual identifier of the catalog.</span></span><br><span data-ttu-id="a4578-649">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="a4578-649">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="a4578-650">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="a4578-650">Max length: 50</span></span> |
| <span data-ttu-id="a4578-651">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-651">apiVersion</span></span> |<span data-ttu-id="a4578-652">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-652">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-653">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-653">Request Body</span></span> |<span data-ttu-id="a4578-654">Data o využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-654">Usage data.</span></span> <span data-ttu-id="a4578-655">Formát:</span><span class="sxs-lookup"><span data-stu-id="a4578-655">Format:</span></span><br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th><span data-ttu-id="a4578-656">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a4578-656">Name</span></span></th><th><span data-ttu-id="a4578-657">Povinné</span><span class="sxs-lookup"><span data-stu-id="a4578-657">Mandatory</span></span></th><th><span data-ttu-id="a4578-658">Typ</span><span class="sxs-lookup"><span data-stu-id="a4578-658">Type</span></span></th><th><span data-ttu-id="a4578-659">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-659">Description</span></span></th></tr><tr><td><span data-ttu-id="a4578-660">Id uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-660">User Id</span></span></td><td><span data-ttu-id="a4578-661">Ano</span><span class="sxs-lookup"><span data-stu-id="a4578-661">Yes</span></span></td><td><span data-ttu-id="a4578-662">[A-z], [-z], [0-9] [_] &#40; Podtržítko &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="a4578-662">[A-z], [a-z], [0-9], [_] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="a4578-663">Maximální délka: 255</span><span class="sxs-lookup"><span data-stu-id="a4578-663">Max length: 255</span></span> </td><td><span data-ttu-id="a4578-664">Jedinečný identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-664">Unique identifier of a user.</span></span></td></tr><tr><td><span data-ttu-id="a4578-665">Id položky</span><span class="sxs-lookup"><span data-stu-id="a4578-665">Item Id</span></span></td><td><span data-ttu-id="a4578-666">Ano</span><span class="sxs-lookup"><span data-stu-id="a4578-666">Yes</span></span></td><td><span data-ttu-id="a4578-667">[A-z], [-z], [0-9] [&#95;] &#40; Podtržítko &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="a4578-667">[A-z], [a-z], [0-9], [&#95;] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="a4578-668">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="a4578-668">Max length: 50</span></span></td><td><span data-ttu-id="a4578-669">Jedinečný identifikátor položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-669">Unique identifier of an item.</span></span></td></tr><tr><td><span data-ttu-id="a4578-670">Čas</span><span class="sxs-lookup"><span data-stu-id="a4578-670">Time</span></span></td><td><span data-ttu-id="a4578-671">Ne</span><span class="sxs-lookup"><span data-stu-id="a4578-671">No</span></span></td><td><span data-ttu-id="a4578-672">Datum ve formátu: rrrr/MM/ddTHH (například 2013/06/20T10:00:00)</span><span class="sxs-lookup"><span data-stu-id="a4578-672">Date in format: YYYY/MM/DDTHH:MM:SS (e.g. 2013/06/20T10:00:00)</span></span></td><td><span data-ttu-id="a4578-673">Čas data.</span><span class="sxs-lookup"><span data-stu-id="a4578-673">Time of data.</span></span></td></tr><tr><td><span data-ttu-id="a4578-674">Událost</span><span class="sxs-lookup"><span data-stu-id="a4578-674">Event</span></span></td><td><span data-ttu-id="a4578-675">Ne; Pokud zadaný musí taky datum</span><span class="sxs-lookup"><span data-stu-id="a4578-675">No; if supplied then must also put date</span></span></td><td><span data-ttu-id="a4578-676">Jeden z následujících:</span><span class="sxs-lookup"><span data-stu-id="a4578-676">One of the following:</span></span><br><span data-ttu-id="a4578-677">• Klikněte na tlačítko</span><span class="sxs-lookup"><span data-stu-id="a4578-677">• Click</span></span><br><span data-ttu-id="a4578-678">• RecommendationClick</span><span class="sxs-lookup"><span data-stu-id="a4578-678">• RecommendationClick</span></span><br><span data-ttu-id="a4578-679">• AddShopCart</span><span class="sxs-lookup"><span data-stu-id="a4578-679">•    AddShopCart</span></span><br><span data-ttu-id="a4578-680">• RemoveShopCart</span><span class="sxs-lookup"><span data-stu-id="a4578-680">• RemoveShopCart</span></span><br><span data-ttu-id="a4578-681">• Nákupu</span><span class="sxs-lookup"><span data-stu-id="a4578-681">• Purchase</span></span></td><td></td></tr></table><br><span data-ttu-id="a4578-682">Maximální velikost souboru: 200MB</span><span class="sxs-lookup"><span data-stu-id="a4578-682">Maximum file size: 200MB</span></span><br><br><span data-ttu-id="a4578-683">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-683">Example:</span></span><br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

<span data-ttu-id="a4578-684">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-684">**Response**:</span></span>

<span data-ttu-id="a4578-685">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-685">HTTP Status code: 200</span></span>

* <span data-ttu-id="a4578-686">`Feed\entry\content\properties\LineCount`-Přijaté počet řádků.</span><span class="sxs-lookup"><span data-stu-id="a4578-686">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="a4578-687">`Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby.</span><span class="sxs-lookup"><span data-stu-id="a4578-687">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due to an error.</span></span>
* <span data-ttu-id="a4578-688">`Feed\entry\content\properties\FileId`-Souboru identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a4578-688">`Feed\entry\content\properties\FileId` - File identifier.</span></span>

<span data-ttu-id="a4578-689">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-689">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="912-using-data-acquisition"></a><span data-ttu-id="a4578-690">9.1.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-690">9.1.2.</span></span> <span data-ttu-id="a4578-691">Pomocí získávání dat</span><span class="sxs-lookup"><span data-stu-id="a4578-691">Using Data Acquisition</span></span>
<span data-ttu-id="a4578-692">V této části ukazuje, jak k odesílání událostí v reálném čase na Azure Machine Learning doporučení, obvykle z vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="a4578-692">This section shows how to send events in real time to Azure Machine Learning Recommendations, usually from your website.</span></span>

| <span data-ttu-id="a4578-693">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-693">HTTP Method</span></span> | <span data-ttu-id="a4578-694">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-694">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-695">POST</span><span class="sxs-lookup"><span data-stu-id="a4578-695">POST</span></span> |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27` |
| <span data-ttu-id="a4578-696">ZÁHLAVÍ</span><span class="sxs-lookup"><span data-stu-id="a4578-696">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="a4578-697">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-697">Parameter Name</span></span> | <span data-ttu-id="a4578-698">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-698">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-699">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-699">apiVersion</span></span> |<span data-ttu-id="a4578-700">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-700">1.0</span></span> |
| <span data-ttu-id="a4578-701">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-701">Request body</span></span> |<span data-ttu-id="a4578-702">Vkládání dat události pro všechny události, který chcete poslat.</span><span class="sxs-lookup"><span data-stu-id="a4578-702">Event data entry for each event you want to send.</span></span> <span data-ttu-id="a4578-703">Byste měli odeslat pro stejné relace uživatele nebo prohlížeče stejné ID v poli ID relace.</span><span class="sxs-lookup"><span data-stu-id="a4578-703">You should send for the same user or browser session the same ID in the SessionId field.</span></span> <span data-ttu-id="a4578-704">(Viz ukázka textu události, které jsou níže).</span><span class="sxs-lookup"><span data-stu-id="a4578-704">(See sample of event body below.)</span></span> |

* <span data-ttu-id="a4578-705">Příklad pro události, klikněte na tlačítko'.</span><span class="sxs-lookup"><span data-stu-id="a4578-705">Example for event 'Click':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>
* <span data-ttu-id="a4578-706">Příklad pro událost 'RecommendationClick':</span><span class="sxs-lookup"><span data-stu-id="a4578-706">Example for event 'RecommendationClick':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RecommendationClick</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="a4578-707">Příklad pro událost 'AddShopCart':</span><span class="sxs-lookup"><span data-stu-id="a4578-707">Example for event 'AddShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="a4578-708">Příklad pro událost 'RemoveShopCart':</span><span class="sxs-lookup"><span data-stu-id="a4578-708">Example for event 'RemoveShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
              <EventData>
                      <Name>RemoveShopCart</Name>
                      <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="a4578-709">Příklad pro události, nákupu'.</span><span class="sxs-lookup"><span data-stu-id="a4578-709">Example for event 'Purchase':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
            <EventData>
                <Name>Purchase</Name>
                <PurchaseItems>
                    <PurchaseItem>
                        <ItemId>ABBF8081-C5C0-4F09-9701-E1C7AC78304A</ItemId>
                        <Count>1</Count>
                    </PurchaseItem>
                    <PurchaseItem>
                        <ItemId>21BF8088-B6C0-4509-870C-11C0AC7F304B</ItemId>
                        <Count>3</Count>
                    </PurchaseItem>
                </PurchaseItems>
            </EventData>
        </EventData>
        </Event>
* <span data-ttu-id="a4578-710">Příklad odesílání 2 události, klikněte na' a 'AddShopCart':</span><span class="sxs-lookup"><span data-stu-id="a4578-710">Example sending 2 events, 'Click' and 'AddShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>Click</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
          <ItemName>itemName</ItemName>
          <ItemDescription>item description</ItemDescription>
          <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
          </EventData>
        </Event>

<span data-ttu-id="a4578-711">**Odpověď**: kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-711">**Response**: HTTP Status code: 200</span></span>

### <a name="92----list-model-usage-files"></a><span data-ttu-id="a4578-712">9.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-712">9.2.</span></span>    <span data-ttu-id="a4578-713">Seznam modelu využití souborů</span><span class="sxs-lookup"><span data-stu-id="a4578-713">List Model Usage Files</span></span>
<span data-ttu-id="a4578-714">Načte metadata všechny soubory modelu využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-714">Retrieves metadata of all model usage files.</span></span>
<span data-ttu-id="a4578-715">Využití, které budou soubory načtených v daný okamžik jednu stránku.</span><span class="sxs-lookup"><span data-stu-id="a4578-715">The usage files will be retrieved one page at a time.</span></span> <span data-ttu-id="a4578-716">Položky neobsahuje 100 každé stránky.</span><span class="sxs-lookup"><span data-stu-id="a4578-716">Each page containes 100 items.</span></span> <span data-ttu-id="a4578-717">Pokud chcete získat položky v konkrétním indexem, můžete použít parametr $skip odata.</span><span class="sxs-lookup"><span data-stu-id="a4578-717">If you want to get items at a particular index, you can use the $skip odata parameter.</span></span> <span data-ttu-id="a4578-718">Například pokud chcete získat položky začínající na pozici 100, přidejte parametr $skip = 100 na požadavek.</span><span class="sxs-lookup"><span data-stu-id="a4578-718">For instance if you want to get items starting at position 100, add the parameter $skip=100 to the request.</span></span>

| <span data-ttu-id="a4578-719">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-719">HTTP Method</span></span> | <span data-ttu-id="a4578-720">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-720">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-721">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-721">GET</span></span> |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-722">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-722">Example:</span></span><br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-723">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-723">Parameter Name</span></span> | <span data-ttu-id="a4578-724">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-724">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-725">forModelId</span><span class="sxs-lookup"><span data-stu-id="a4578-725">forModelId</span></span> |<span data-ttu-id="a4578-726">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-726">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-727">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-727">apiVersion</span></span> |<span data-ttu-id="a4578-728">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-728">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-729">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-729">Request Body</span></span> |<span data-ttu-id="a4578-730">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-730">NONE</span></span> |

<span data-ttu-id="a4578-731">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-731">**Response**:</span></span>

<span data-ttu-id="a4578-732">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-732">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-733">Odpověď obsahuje jeden záznam za použití souboru.</span><span class="sxs-lookup"><span data-stu-id="a4578-733">The response includes one entry per usage file.</span></span> <span data-ttu-id="a4578-734">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-734">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-735">`feed\entry\content\properties\Id`– ID využití souboru.</span><span class="sxs-lookup"><span data-stu-id="a4578-735">`feed\entry\content\properties\Id` - Usage file ID.</span></span>
* <span data-ttu-id="a4578-736">`feed\entry\content\properties\Length`-Využití délka souboru v MB.</span><span class="sxs-lookup"><span data-stu-id="a4578-736">`feed\entry\content\properties\Length` - Usage file length in MB.</span></span>
* <span data-ttu-id="a4578-737">`feed\entry\content\properties\DateModified`-Datum vytvoření souboru využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-737">`feed\entry\content\properties\DateModified` - Date when the usage file was created.</span></span>
* <span data-ttu-id="a4578-738">`feed\entry\content\properties\UseInModel`– Jestli využití souboru se používá v modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-738">`feed\entry\content\properties\UseInModel` - Whether the usage file is used in the model.</span></span>

<span data-ttu-id="a4578-739">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-739">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of model's usage files info</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
            <updated>2014-10-30T09:40:25Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">4c067b42-e975-4cb2-8c98-a6ab80ed6d63</d:Id>
                    <d:Length m:type="Edm.Double">0</d:Length>
                    <d:DateModified m:type="Edm.String">10/30/2014 9:19:57 AM</d:DateModified>
                    <d:UseInModel m:type="Edm.String">true</d:UseInModel>
                </m:properties>
            </content>
        </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">3126d816-4e80-4248-8339-1ebbdb9d544d</d:Id>
                <d:Length m:type="Edm.Double">0.001</d:Length>
                <d:DateModified m:type="Edm.String">10/30/2014 9:21:44 AM</d:DateModified>
                <d:UseInModel m:type="Edm.String">true</d:UseInModel>
              </m:properties>
        </content>
    </entry>
</feed>

### <a name="93----get-usage-statistics"></a><span data-ttu-id="a4578-740">9.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-740">9.3.</span></span>    <span data-ttu-id="a4578-741">Získat statistiku využití</span><span class="sxs-lookup"><span data-stu-id="a4578-741">Get Usage Statistics</span></span>
<span data-ttu-id="a4578-742">Získá Statistika využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-742">Gets usage statistics.</span></span>

| <span data-ttu-id="a4578-743">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-743">HTTP Method</span></span> | <span data-ttu-id="a4578-744">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-744">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-745">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-745">GET</span></span> |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-746">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-746">Example:</span></span><br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-747">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-747">Parameter Name</span></span> | <span data-ttu-id="a4578-748">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-748">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-749">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-749">modelId</span></span> |<span data-ttu-id="a4578-750">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-750">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-751">Počátečním</span><span class="sxs-lookup"><span data-stu-id="a4578-751">startDate</span></span> |<span data-ttu-id="a4578-752">Počáteční datum.</span><span class="sxs-lookup"><span data-stu-id="a4578-752">Start date.</span></span> <span data-ttu-id="a4578-753">Formát: rrrr/MM/ddTHH</span><span class="sxs-lookup"><span data-stu-id="a4578-753">Format: yyyy/MM/ddTHH:mm:ss</span></span> |
| <span data-ttu-id="a4578-754">Koncové datum</span><span class="sxs-lookup"><span data-stu-id="a4578-754">endDate</span></span> |<span data-ttu-id="a4578-755">Koncové datum.</span><span class="sxs-lookup"><span data-stu-id="a4578-755">End date.</span></span> <span data-ttu-id="a4578-756">Formát: rrrr/MM/ddTHH</span><span class="sxs-lookup"><span data-stu-id="a4578-756">Format: yyyy/MM/ddTHH:mm:ss</span></span> |
| <span data-ttu-id="a4578-757">eventTypes</span><span class="sxs-lookup"><span data-stu-id="a4578-757">eventTypes</span></span> |<span data-ttu-id="a4578-758">Textový soubor s oddělovači řetězec typů událostí nebo hodnota null, zobrazíte všechny události</span><span class="sxs-lookup"><span data-stu-id="a4578-758">Comma-separated string of event types or null to get all events</span></span> |
| <span data-ttu-id="a4578-759">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-759">apiVersion</span></span> |<span data-ttu-id="a4578-760">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-760">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-761">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-761">Request Body</span></span> |<span data-ttu-id="a4578-762">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-762">NONE</span></span> |

<span data-ttu-id="a4578-763">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-763">**Response**:</span></span>

<span data-ttu-id="a4578-764">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-764">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-765">Kolekci elementů klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="a4578-765">A collection of key/value elements.</span></span> <span data-ttu-id="a4578-766">Každé z nich obsahuje součet události pro určitý typ události seskupené podle hodinu.</span><span class="sxs-lookup"><span data-stu-id="a4578-766">Each one contains the sum of events for a specific event type grouped by hour.</span></span>

* <span data-ttu-id="a4578-767">`feed\entry[i]\content\properties\Key`-Obsahuje čas (seskupené podle hodinu) a typ události.</span><span class="sxs-lookup"><span data-stu-id="a4578-767">`feed\entry[i]\content\properties\Key` - Contains the time (grouped by hour) and the event type.</span></span>
* <span data-ttu-id="a4578-768">`feed\entry[i]\content\properties\Value`-Počet celkový počet událostí.</span><span class="sxs-lookup"><span data-stu-id="a4578-768">`feed\entry[i]\content\properties\Value` - Total event count.</span></span>

<span data-ttu-id="a4578-769">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-769">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get usage statistics</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-18T11:39:16Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;Click</d:Event>
                <d:Value m:type="Edm.String">5</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;RecommendationClick</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
              </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/1/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/5/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="94----get-usage-file-sample"></a><span data-ttu-id="a4578-770">9.4.</span><span class="sxs-lookup"><span data-stu-id="a4578-770">9.4.</span></span>    <span data-ttu-id="a4578-771">Získat ukázkový soubor využití</span><span class="sxs-lookup"><span data-stu-id="a4578-771">Get Usage File Sample</span></span>
<span data-ttu-id="a4578-772">Načte první 2KB využití obsahu souboru.</span><span class="sxs-lookup"><span data-stu-id="a4578-772">Retrieves the first 2KB of usage file content.</span></span>

| <span data-ttu-id="a4578-773">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-773">HTTP Method</span></span> | <span data-ttu-id="a4578-774">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-774">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-775">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-775">GET</span></span> |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-776">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-776">Example:</span></span><br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-777">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-777">Parameter Name</span></span> | <span data-ttu-id="a4578-778">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-778">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-779">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-779">modelId</span></span> |<span data-ttu-id="a4578-780">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-780">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-781">fileId</span><span class="sxs-lookup"><span data-stu-id="a4578-781">fileId</span></span> |<span data-ttu-id="a4578-782">Jedinečný identifikátor využití souboru modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-782">Unique identifier of the model usage file</span></span> |
| <span data-ttu-id="a4578-783">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-783">apiVersion</span></span> |<span data-ttu-id="a4578-784">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-784">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-785">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-785">Request Body</span></span> |<span data-ttu-id="a4578-786">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-786">NONE</span></span> |

<span data-ttu-id="a4578-787">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-787">**Response**:</span></span>

<span data-ttu-id="a4578-788">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-788">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-789">Odpověď se vrátí ve formátu raw textu:</span><span class="sxs-lookup"><span data-stu-id="a4578-789">Response is returned in raw text format:</span></span>

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
</pre>


### <a name="95----get-model-usage-file"></a><span data-ttu-id="a4578-790">9.5.</span><span class="sxs-lookup"><span data-stu-id="a4578-790">9.5.</span></span>    <span data-ttu-id="a4578-791">Získat soubor modelu využití</span><span class="sxs-lookup"><span data-stu-id="a4578-791">Get Model Usage File</span></span>
<span data-ttu-id="a4578-792">Načte celý obsah souboru využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-792">Retrieves the full content of the usage file.</span></span>

| <span data-ttu-id="a4578-793">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-793">HTTP Method</span></span> | <span data-ttu-id="a4578-794">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-794">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-795">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-795">GET</span></span> |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-796">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-796">Example:</span></span><br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-797">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-797">Parameter Name</span></span> | <span data-ttu-id="a4578-798">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-798">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-799">Mid –</span><span class="sxs-lookup"><span data-stu-id="a4578-799">mid</span></span> |<span data-ttu-id="a4578-800">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-800">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-801">FID</span><span class="sxs-lookup"><span data-stu-id="a4578-801">fid</span></span> |<span data-ttu-id="a4578-802">Jedinečný identifikátor využití souboru modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-802">Unique identifier of the model usage file</span></span> |
| <span data-ttu-id="a4578-803">Stahování</span><span class="sxs-lookup"><span data-stu-id="a4578-803">download</span></span> |<span data-ttu-id="a4578-804">1</span><span class="sxs-lookup"><span data-stu-id="a4578-804">1</span></span> |
| <span data-ttu-id="a4578-805">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-805">apiVersion</span></span> |<span data-ttu-id="a4578-806">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-806">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-807">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-807">Request Body</span></span> |<span data-ttu-id="a4578-808">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-808">NONE</span></span> |

<span data-ttu-id="a4578-809">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-809">**Response**:</span></span>

<span data-ttu-id="a4578-810">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-810">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-811">Odpověď se vrátí ve formátu raw textu:</span><span class="sxs-lookup"><span data-stu-id="a4578-811">Response is returned in raw text format:</span></span>

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
244881,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
50547,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
213090,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
260655,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
72214,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
36326,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189336,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260655,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
162100,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
54946,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260965,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
102758,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
112602,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
163925,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
262998,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
144717,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
</pre>

### <a name="96----delete-usage-file"></a><span data-ttu-id="a4578-812">9.6.</span><span class="sxs-lookup"><span data-stu-id="a4578-812">9.6.</span></span>    <span data-ttu-id="a4578-813">Odstranit soubor využití</span><span class="sxs-lookup"><span data-stu-id="a4578-813">Delete Usage File</span></span>
<span data-ttu-id="a4578-814">Odstraní soubor využití zadaného modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-814">Deletes the specified model usage file.</span></span>

| <span data-ttu-id="a4578-815">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-815">HTTP Method</span></span> | <span data-ttu-id="a4578-816">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-816">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-817">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="a4578-817">DELETE</span></span> |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-818">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-818">Example:</span></span><br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-819">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-819">Parameter Name</span></span> | <span data-ttu-id="a4578-820">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-820">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-821">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-821">modelId</span></span> |<span data-ttu-id="a4578-822">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-822">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-823">fileId</span><span class="sxs-lookup"><span data-stu-id="a4578-823">fileId</span></span> |<span data-ttu-id="a4578-824">Jedinečný identifikátor souboru k odstranění</span><span class="sxs-lookup"><span data-stu-id="a4578-824">Unique identifier of the file to be deleted</span></span> |
| <span data-ttu-id="a4578-825">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-825">apiVersion</span></span> |<span data-ttu-id="a4578-826">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-826">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-827">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-827">Request Body</span></span> |<span data-ttu-id="a4578-828">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-828">NONE</span></span> |

<span data-ttu-id="a4578-829">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-829">**Response**:</span></span>

<span data-ttu-id="a4578-830">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-830">HTTP Status code: 200</span></span>

### <a name="97----delete-all-usage-files"></a><span data-ttu-id="a4578-831">9.7.</span><span class="sxs-lookup"><span data-stu-id="a4578-831">9.7.</span></span>    <span data-ttu-id="a4578-832">Odstraní všechny soubory využití</span><span class="sxs-lookup"><span data-stu-id="a4578-832">Delete All Usage Files</span></span>
<span data-ttu-id="a4578-833">Odstraní všechny soubory modelu využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-833">Deletes all model usage files.</span></span>

| <span data-ttu-id="a4578-834">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-834">HTTP Method</span></span> | <span data-ttu-id="a4578-835">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-835">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-836">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="a4578-836">DELETE</span></span> |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-837">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-837">Example:</span></span><br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-838">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-838">Parameter Name</span></span> | <span data-ttu-id="a4578-839">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-839">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-840">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-840">modelId</span></span> |<span data-ttu-id="a4578-841">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-841">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-842">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-842">apiVersion</span></span> |<span data-ttu-id="a4578-843">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-843">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-844">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-844">Request Body</span></span> |<span data-ttu-id="a4578-845">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-845">NONE</span></span> |

<span data-ttu-id="a4578-846">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-846">**Response**:</span></span>

<span data-ttu-id="a4578-847">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-847">HTTP Status code: 200</span></span>

## <a name="10-features"></a><span data-ttu-id="a4578-848">10. Funkce</span><span class="sxs-lookup"><span data-stu-id="a4578-848">10. Features</span></span>
<span data-ttu-id="a4578-849">V této části ukazuje, jak načíst informace o funkci, jako je například importované funkce a jejich hodnoty, jejich pořadí a toto pořadí byl přidělen.</span><span class="sxs-lookup"><span data-stu-id="a4578-849">This section shows how to retrieve feature information, such as the imported features and their values, their rank, and when this rank was allocated.</span></span> <span data-ttu-id="a4578-850">Funkce importují v rámci katalogu dat a jejich pořadí je pak přidružené, pokud se provádí rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-850">Features are imported as part of the catalog data, and then their rank is associated when a rank build is done.</span></span>
<span data-ttu-id="a4578-851">Funkce pořadí můžete změnit podle vzoru data o využití a typu položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-851">Feature rank can change according to the pattern of usage data and type of items.</span></span> <span data-ttu-id="a4578-852">Ale pro konzistentní využití nebo položky, pořadí by měl mít jenom malé kolísání.</span><span class="sxs-lookup"><span data-stu-id="a4578-852">But for consistent usage/items, the rank should have only small fluctuations.</span></span>
<span data-ttu-id="a4578-853">Pořadí funkcí je nezáporné číslo.</span><span class="sxs-lookup"><span data-stu-id="a4578-853">The rank of features is a non-negative number.</span></span> <span data-ttu-id="a4578-854">Číslo 0 znamená, že tato funkce nebyla seřazeny (se stane, když vyvolání toto rozhraní API před dokončením první rank sestavení).</span><span class="sxs-lookup"><span data-stu-id="a4578-854">The  number 0 means that the feature was not ranked (happens if you invoke this API prior to the completion of the first rank build).</span></span> <span data-ttu-id="a4578-855">Datum, kdy byl pořadí s atributy se nazývá skóre podle aktuálnosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-855">The date at which the rank was attributed is called the score freshness.</span></span>

### <a name="101-get-features-info-for-last-rank-build"></a><span data-ttu-id="a4578-856">10.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-856">10.1.</span></span> <span data-ttu-id="a4578-857">Získání informací o funkcích (pro poslední Rank sestavení)</span><span class="sxs-lookup"><span data-stu-id="a4578-857">Get Features Info (For Last Rank Build)</span></span>
<span data-ttu-id="a4578-858">Načte informace o funkci, včetně hodnocení pro poslední úspěšné rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-858">Retrieves the feature information, including ranking, for the last successful rank build.</span></span>

| <span data-ttu-id="a4578-859">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-859">HTTP Method</span></span> | <span data-ttu-id="a4578-860">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-860">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-861">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-861">GET</span></span> |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-862">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-862">Example:</span></span><br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-863">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-863">Parameter Name</span></span> | <span data-ttu-id="a4578-864">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-864">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-865">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-865">modelId</span></span> |<span data-ttu-id="a4578-866">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-866">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-867">samplingSize</span><span class="sxs-lookup"><span data-stu-id="a4578-867">samplingSize</span></span> |<span data-ttu-id="a4578-868">Počet hodnot, které chcete pro každou součást podle data, která je obsažená v katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-868">Number of values to include for each feature according to the data present in the catalog.</span></span> <br/><span data-ttu-id="a4578-869">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a4578-869">Possible values are:</span></span><br> <span data-ttu-id="a4578-870">-1 - všechny vzorky.</span><span class="sxs-lookup"><span data-stu-id="a4578-870">-1 - All samples.</span></span> <br><span data-ttu-id="a4578-871">0 – žádný vzorkování.</span><span class="sxs-lookup"><span data-stu-id="a4578-871">0 - No sampling.</span></span> <br><span data-ttu-id="a4578-872">N - vrátí N ukázky pro každý název funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-872">N - Return N samples for each feature name.</span></span> |
| <span data-ttu-id="a4578-873">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-873">apiVersion</span></span> |<span data-ttu-id="a4578-874">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-874">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-875">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-875">Request Body</span></span> |<span data-ttu-id="a4578-876">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-876">NONE</span></span> |

<span data-ttu-id="a4578-877">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-877">**Response**:</span></span>

<span data-ttu-id="a4578-878">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-878">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-879">Odpověď obsahuje seznam položek informace o funkci.</span><span class="sxs-lookup"><span data-stu-id="a4578-879">The response contains a list of feature info entries.</span></span> <span data-ttu-id="a4578-880">Každý záznam obsahuje:</span><span class="sxs-lookup"><span data-stu-id="a4578-880">Each entry contains:</span></span>

* <span data-ttu-id="a4578-881">`feed/entry/content/m:properties/d:Name`– Název funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-881">`feed/entry/content/m:properties/d:Name` - Feature name.</span></span>
* <span data-ttu-id="a4578-882">`feed/entry/content/m:properties/d:RankUpdateDate`-Datum, kdy pořadí byl přidělen k této funkci také znám jako</span><span class="sxs-lookup"><span data-stu-id="a4578-882">`feed/entry/content/m:properties/d:RankUpdateDate` - Date at which the rank was allocated to this feature, a.k.a.</span></span> <span data-ttu-id="a4578-883">skóre podle aktuálnosti funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-883">score freshness feature.</span></span> <span data-ttu-id="a4578-884">Historická data (0001-01-01T00:00:00 ") znamená, že byla provedena žádná rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-884">A historical date ('0001-01-01T00:00:00') means that no rank build was performed.</span></span>
* <span data-ttu-id="a4578-885">`feed/entry/content/m:properties/d:Rank`– Pořadí funkce (float).</span><span class="sxs-lookup"><span data-stu-id="a4578-885">`feed/entry/content/m:properties/d:Rank` - Feature rank (float).</span></span> <span data-ttu-id="a4578-886">Pořadí 2.0 nebo vyšší je považován za dobré funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-886">A rank of 2.0 and up is considered a good feature.</span></span>
* <span data-ttu-id="a4578-887">`feed/entry/content/m:properties/d:SampleValues`-Textový soubor s oddělovači seznamu hodnot až je požadovaná velikost vzorkování.</span><span class="sxs-lookup"><span data-stu-id="a4578-887">`feed/entry/content/m:properties/d:SampleValues` - Comma-separated list of values up to the sampling size requested.</span></span>

<span data-ttu-id="a4578-888">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-888">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get the features of a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-01-08T13:15:02Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Author</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Publisher</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1"/>
        <contenttype="application/xml">
        <m:properties>
            <d:Name m:type="Edm.String">Year</d:Name>
            <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
            <d:Rank m:type="Edm.String">0</d:Rank>
            <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
        </m:properties>
        </content>
    </entry>
</feed>

### <a name="102-get-features-info-for-specific-rank-build"></a><span data-ttu-id="a4578-889">10.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-889">10.2.</span></span> <span data-ttu-id="a4578-890">Získání informací o funkcích (pro konkrétní Rank sestavení)</span><span class="sxs-lookup"><span data-stu-id="a4578-890">Get Features Info (For Specific Rank Build)</span></span>
<span data-ttu-id="a4578-891">Načte informace o funkci, včetně hodnocení pro konkrétní rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-891">Retrieves the feature information, including the ranking for a specific rank build.</span></span>

| <span data-ttu-id="a4578-892">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-892">HTTP Method</span></span> | <span data-ttu-id="a4578-893">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-893">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-894">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-894">GET</span></span> |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-895">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-895">Example:</span></span><br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-896">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-896">Parameter Name</span></span> | <span data-ttu-id="a4578-897">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-897">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-898">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-898">modelId</span></span> |<span data-ttu-id="a4578-899">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-899">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-900">samplingSize</span><span class="sxs-lookup"><span data-stu-id="a4578-900">samplingSize</span></span> |<span data-ttu-id="a4578-901">Počet hodnot, které chcete pro každou součást podle data, která je obsažená v katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-901">Number of values to include for each feature according to the data present in the catalog.</span></span><br/> <span data-ttu-id="a4578-902">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a4578-902">Possible values are:</span></span><br> <span data-ttu-id="a4578-903">-1 - všechny vzorky.</span><span class="sxs-lookup"><span data-stu-id="a4578-903">-1 - All samples.</span></span> <br><span data-ttu-id="a4578-904">0 – žádný vzorkování.</span><span class="sxs-lookup"><span data-stu-id="a4578-904">0 - No sampling.</span></span> <br><span data-ttu-id="a4578-905">N - vrátí N ukázky pro každý název funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-905">N - Return N samples for each feature name.</span></span> |
| <span data-ttu-id="a4578-906">rankBuildId</span><span class="sxs-lookup"><span data-stu-id="a4578-906">rankBuildId</span></span> |<span data-ttu-id="a4578-907">Jedinečný identifikátor rank sestavení nebo -1 pro poslední rank sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-907">Unique identifier of the rank build or -1 for the last rank build</span></span> |
| <span data-ttu-id="a4578-908">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-908">apiVersion</span></span> |<span data-ttu-id="a4578-909">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-909">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-910">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-910">Request Body</span></span> |<span data-ttu-id="a4578-911">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-911">NONE</span></span> |

<span data-ttu-id="a4578-912">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-912">**Response**:</span></span>

<span data-ttu-id="a4578-913">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-913">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-914">Odpověď obsahuje seznam položek informace o funkci.</span><span class="sxs-lookup"><span data-stu-id="a4578-914">The response contains a list of feature info entries.</span></span> <span data-ttu-id="a4578-915">Každý záznam obsahuje:</span><span class="sxs-lookup"><span data-stu-id="a4578-915">Each entry contains:</span></span>

* <span data-ttu-id="a4578-916">`feed/entry/content/m:properties/d:Name`– Název funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-916">`feed/entry/content/m:properties/d:Name` - Feature name.</span></span>
* <span data-ttu-id="a4578-917">`feed/entry/content/m:properties/d:RankUpdateDate`-Datum, kdy pořadí byl přidělen k této funkci také znám jako</span><span class="sxs-lookup"><span data-stu-id="a4578-917">`feed/entry/content/m:properties/d:RankUpdateDate` - Date at which the rank was allocated to this feature, a.k.a.</span></span> <span data-ttu-id="a4578-918">skóre podle aktuálnosti funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-918">score freshness feature.</span></span> <span data-ttu-id="a4578-919">Historická data (0001-01-01T00:00:00 ") znamená, že byla provedena žádná rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-919">A historical date ('0001-01-01T00:00:00') means that no rank build was performed.</span></span>
* <span data-ttu-id="a4578-920">`feed/entry/content/m:properties/d:Rank`– Pořadí funkce (float).</span><span class="sxs-lookup"><span data-stu-id="a4578-920">`feed/entry/content/m:properties/d:Rank` - Feature rank (float).</span></span> <span data-ttu-id="a4578-921">Pořadí 2.0 nebo vyšší je považován za dobré funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-921">A rank of 2.0 and up is considered a good feature.</span></span>
* <span data-ttu-id="a4578-922">`feed/entry/content/m:properties/d:SampleValues`-Textový soubor s oddělovači seznamu hodnot až je požadovaná velikost vzorkování.</span><span class="sxs-lookup"><span data-stu-id="a4578-922">`feed/entry/content/m:properties/d:SampleValues` - Comma-separated list of values up to the sampling size requested.</span></span>

<span data-ttu-id="a4578-923">OData</span><span class="sxs-lookup"><span data-stu-id="a4578-923">OData</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get the features of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:54:22Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Author</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">3.38867426</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Publisher</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.67839336</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Year</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.12352145</d:Rank>
                    <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
                </m:properties>
            </content>
        </entry>
    </feed>


## <a name="11-build"></a><span data-ttu-id="a4578-924">11. Sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-924">11. Build</span></span>
  <span data-ttu-id="a4578-925">Tato část vysvětluje různé rozhraní API související s sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-925">This section explains the different APIs related to builds.</span></span> <span data-ttu-id="a4578-926">Existují 3 typy sestavení: sestavení doporučení, rank sestavení a sestavení FBT (často koupili společně).</span><span class="sxs-lookup"><span data-stu-id="a4578-926">There are 3 types of builds: a recommendation build, a rank build and an FBT (frequently bought together) build.</span></span>

<span data-ttu-id="a4578-927">Účelem sestavení doporučení je chcete generovat model doporučení použít pro předpovědi.</span><span class="sxs-lookup"><span data-stu-id="a4578-927">The recommendation build's purpose is to generate a recommendation model used for predictions.</span></span> <span data-ttu-id="a4578-928">Predikce (pro tento typ sestavení) má ve dvou typů:</span><span class="sxs-lookup"><span data-stu-id="a4578-928">Predictions (for this type of build) come in two flavors:</span></span>

* <span data-ttu-id="a4578-929">I2I - také znám jako</span><span class="sxs-lookup"><span data-stu-id="a4578-929">I2I - a.k.a.</span></span> <span data-ttu-id="a4578-930">Doporučení položkami - danou položku nebo seznam položek, tato možnost bude předpovídat seznam položek, které by mohly zajímat vysoké.</span><span class="sxs-lookup"><span data-stu-id="a4578-930">Item to Item recommendations - given an item or a list of items this option will predict a list of items that are likely to be of high interest.</span></span>
* <span data-ttu-id="a4578-931">U2I - také znám jako</span><span class="sxs-lookup"><span data-stu-id="a4578-931">U2I - a.k.a.</span></span> <span data-ttu-id="a4578-932">Uživatele pro položky doporučení – dané id uživatele (a volitelně seznam položek) Tato možnost bude předvídání seznam položek, které by mohly být vysoké týkající se daného uživatele (a další volbu položek).</span><span class="sxs-lookup"><span data-stu-id="a4578-932">User to Item recommendations - given a user id (and optionally a list of items) this option will predict a list of items that are likely to be of high interest for the given user (and its additional choice of items).</span></span> <span data-ttu-id="a4578-933">U2I doporučení jsou založená na historii položky, které byly týkající se uživatelů až do okamžiku, kdy byl vytvořený modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-933">The U2I recommendations are based on the history of items that were of interest for the user up to the time the model was built.</span></span>

<span data-ttu-id="a4578-934">Rank sestavení je technické sestavení, který umožňuje získat informace o použitelnosti vaší funkcí.</span><span class="sxs-lookup"><span data-stu-id="a4578-934">A rank build is a technical build that allows you to learn about the usefulness of your features.</span></span> <span data-ttu-id="a4578-935">Obvykle aby bylo možné získat nejlepších výsledků pro model doporučení zahrnující funkce, byste měli vzít následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4578-935">Usually, in order to get the best result for a recommendation model involving features, you should take the following steps:</span></span>

* <span data-ttu-id="a4578-936">Aktivovat rank build (Pokud skóre vaší funkcí není stabilní) a počkejte, dokud získat skóre funkce.</span><span class="sxs-lookup"><span data-stu-id="a4578-936">Trigger a rank build (unless the score of your features is stable) and wait till you get the feature score.</span></span>
* <span data-ttu-id="a4578-937">Načtení pořadí vaší funkce voláním [získat informace o funkcích](#101-get-features-info-for-last-rank-build) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a4578-937">Retrieve the rank of your features by calling the [Get Features Info](#101-get-features-info-for-last-rank-build) API.</span></span>
* <span data-ttu-id="a4578-938">Konfigurace sestavení doporučení s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="a4578-938">Configure a recommendation build with the following parameters:</span></span>
  * <span data-ttu-id="a4578-939">`useFeatureInModel`– Nastavte na hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="a4578-939">`useFeatureInModel` - Set to True.</span></span>
  * <span data-ttu-id="a4578-940">`ModelingFeatureList`– Nastavte na textový soubor s oddělovači seznam funkcí se skóre 2.0 nebo více (podle pořadí, které jste získali v předchozím kroku).</span><span class="sxs-lookup"><span data-stu-id="a4578-940">`ModelingFeatureList` - Set to a comma-separated list of features with a score of 2.0 or more (according to the ranks you retrieved in the previous step).</span></span>
  * <span data-ttu-id="a4578-941">`AllowColdItemPlacement`– Nastavte na hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="a4578-941">`AllowColdItemPlacement` - Set to True.</span></span>
  * <span data-ttu-id="a4578-942">Volitelně můžete nastavit `EnableFeatureCorrelation` na hodnotu True a `ReasoningFeatureList` do seznamu funkcí, kterou chcete použít pro vysvětlení (obvykle seznam stejné funkce použité v modelování nebo dílčí seznam).</span><span class="sxs-lookup"><span data-stu-id="a4578-942">Optionally you can set `EnableFeatureCorrelation` to True and `ReasoningFeatureList` to the list of features you want to use for explanations (usually the same list of features used in modelling or a sublist).</span></span>
* <span data-ttu-id="a4578-943">Aktivovat sestavení doporučení s konfigurovanými parametry.</span><span class="sxs-lookup"><span data-stu-id="a4578-943">Trigger the recommendation build with the configured parameters.</span></span>

<span data-ttu-id="a4578-944">Poznámka: Pokud nenakonfigurujete žádné parametry (například vyvolání doporučení sestavení bez parametrů) nebo explicitně nezakážete využití funkcí (například `UseFeatureInModel` nastavena na hodnotu False), nastaví parametry související s funkcí, které vysvětleno v systému výše uvedené hodnoty v případě, že pořadí sestavení existuje.</span><span class="sxs-lookup"><span data-stu-id="a4578-944">Note: If you do not configure any parameters (e.g. invoke the recommendation build without parameters) or you do not explicitly disable the usage of features (e.g. `UseFeatureInModel` set to False), the system will set up the feature-related parameters to the explained values above in case a rank build exists.</span></span>

<span data-ttu-id="a4578-945">Neexistuje žádné omezení na spuštění rank sestavení a doporučení sestavení současně pro stejný model.</span><span class="sxs-lookup"><span data-stu-id="a4578-945">There is no restriction on running a rank build and a recommendation build concurrently for the same model.</span></span> <span data-ttu-id="a4578-946">Nicméně na stejný model paralelně nelze spustit dvě sestavení stejného typu.</span><span class="sxs-lookup"><span data-stu-id="a4578-946">Nevertheless, you cannot run two builds of the same type on the same model in parallel.</span></span>

<span data-ttu-id="a4578-947">Sestavení FBT (často koupili společně) je ještě jiný algoritmus doporučení názvem někdy "konzervativní" doporučené, který je vhodný pro katalogů, které nejsou ve své podstatě homogenní (homogenní: knihy, filmy, některé jídlo dotazování; bez homogenní: počítače a zařízení, mezi doménami, velmi různorodému).</span><span class="sxs-lookup"><span data-stu-id="a4578-947">An FBT (Frequently bought together) build is yet another recommendations algorithm called sometimes "conservative" recommender, which is good for catalogs that are not homogeneous in nature (homogeneous: books, movies, some food, fashion; non-homogeneous: computer and devices, cross-domain, highly diverse).</span></span>

<span data-ttu-id="a4578-948">Poznámka: Pokud využití soubory, které jste odeslali obsahovat volitelné pole "typ události" pak pro FBT modelování pouze "Nákupu" události se použije.</span><span class="sxs-lookup"><span data-stu-id="a4578-948">Note: if the usage files that you uploaded contain the optional field "event type" then for FBT modelling only "Purchase" events will be used.</span></span> <span data-ttu-id="a4578-949">Pokud je k dispozici žádný typ události, že všechny události bude považovat za nákupu.</span><span class="sxs-lookup"><span data-stu-id="a4578-949">If no event type is provided all events will be considered as purchase.</span></span>

#### <a name="111-build-parameters"></a><span data-ttu-id="a4578-950">11.1 sestavení parametry</span><span class="sxs-lookup"><span data-stu-id="a4578-950">11.1 Build parameters</span></span>
<span data-ttu-id="a4578-951">Každý typ sestavení je možné nakonfigurovat přes sadu parametrů (které popsané níže).</span><span class="sxs-lookup"><span data-stu-id="a4578-951">Each build type can be configured via a set of parameters (depicted below).</span></span> <span data-ttu-id="a4578-952">Pokud nenakonfigurujete parametry, systém automaticky atribut hodnoty na parametry podle informace obsažené v době, že aktivovat build.</span><span class="sxs-lookup"><span data-stu-id="a4578-952">If you don't configure the parameters, the system will automatically attribute values to the parameters according to the information present at the time you trigger a build.</span></span>

##### <a name="1111-usage-condenser"></a><span data-ttu-id="a4578-953">11.1.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-953">11.1.1.</span></span> <span data-ttu-id="a4578-954">Chladič využití</span><span class="sxs-lookup"><span data-stu-id="a4578-954">Usage condenser</span></span>
<span data-ttu-id="a4578-955">Další šumu než informace může obsahovat uživatele nebo položky s několika bodů využití.</span><span class="sxs-lookup"><span data-stu-id="a4578-955">Users or items with few usage points might contain more noise than information.</span></span> <span data-ttu-id="a4578-956">Systém se pokusí minimální počet bodů využití na uživatele nebo položku, který se má použít v modelu předpovědi.</span><span class="sxs-lookup"><span data-stu-id="a4578-956">The system attempts to predict the minimal number of usage points per user/item to be used in a model.</span></span> <span data-ttu-id="a4578-957">Toto číslo se bude nacházet v rozsahu definovaného ItemCutoffLowerBound a ItemCutoffUpperBound parametry pro položky a rozsah definovaný hodnotami parametrů UserCutOffLowerBound a UserCutoffUpperBound pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-957">This number will be within the range defined by the ItemCutoffLowerBound and ItemCutoffUpperBound parameters for items, and the range defined by the UserCutOffLowerBound and UserCutoffUpperBound parameters for users.</span></span> <span data-ttu-id="a4578-958">Chladič vliv na položky nebo uživatelé se dá minimalizovat nastavení alespoň jeden odpovídající mezí nule.</span><span class="sxs-lookup"><span data-stu-id="a4578-958">The condenser effect on items or users can be minimized by setting at least one of the corresponding bounds to zero.</span></span>

##### <a name="1112-rank-build-parameters"></a><span data-ttu-id="a4578-959">11.1.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-959">11.1.2.</span></span> <span data-ttu-id="a4578-960">Pořadí sestavení parametry</span><span class="sxs-lookup"><span data-stu-id="a4578-960">Rank build parameters</span></span>
<span data-ttu-id="a4578-961">Následující tabulka znázorňuje sestavení parametry rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-961">The table below depicts the build parameters for a rank build.</span></span>

| <span data-ttu-id="a4578-962">Klíč</span><span class="sxs-lookup"><span data-stu-id="a4578-962">Key</span></span> | <span data-ttu-id="a4578-963">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-963">Description</span></span> | <span data-ttu-id="a4578-964">Typ</span><span class="sxs-lookup"><span data-stu-id="a4578-964">Type</span></span> | <span data-ttu-id="a4578-965">Platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a4578-965">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a4578-966">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="a4578-966">NumberOfModelIterations</span></span> |<span data-ttu-id="a4578-967">Počet iterací, které provádí modelu se odráží celkovou dobu výpočtů a přesnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-967">The number of iterations the model performs is reflected by the overall compute time and the model accuracy.</span></span> <span data-ttu-id="a4578-968">Tím vyšší je číslo, lepší přesnost budete mít, ale bude trvat delší dobu výpočtů.</span><span class="sxs-lookup"><span data-stu-id="a4578-968">The higher the number, the better accuracy you will get, but the compute time will take longer.</span></span> |<span data-ttu-id="a4578-969">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-969">Integer</span></span> |<span data-ttu-id="a4578-970">10-50</span><span class="sxs-lookup"><span data-stu-id="a4578-970">10-50</span></span> |
| <span data-ttu-id="a4578-971">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="a4578-971">NumberOfModelDimensions</span></span> |<span data-ttu-id="a4578-972">Počet dimenzí má vztah k číslo modelu se pokusí najít v rámci vašich dat funkcí.</span><span class="sxs-lookup"><span data-stu-id="a4578-972">The number of dimensions relates to the number of 'features' the model will try to find within your data.</span></span> <span data-ttu-id="a4578-973">Zvýšením počtu dimenzí vám umožní lépe vyladit výsledky do menší clusterů.</span><span class="sxs-lookup"><span data-stu-id="a4578-973">Increasing the number of dimensions will allow better fine-tuning of the results into smaller clusters.</span></span> <span data-ttu-id="a4578-974">Však příliš mnoho dimenze zabrání modelu nalézt korelací mezi položkami.</span><span class="sxs-lookup"><span data-stu-id="a4578-974">However, too many dimensions will prevent the model from finding correlations between items.</span></span> |<span data-ttu-id="a4578-975">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-975">Integer</span></span> |<span data-ttu-id="a4578-976">10-40</span><span class="sxs-lookup"><span data-stu-id="a4578-976">10-40</span></span> |
| <span data-ttu-id="a4578-977">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="a4578-977">ItemCutOffLowerBound</span></span> |<span data-ttu-id="a4578-978">Definuje položku dolní mez pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-978">Defines the item lower bound for the condenser.</span></span> <span data-ttu-id="a4578-979">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-979">See usage condenser above.</span></span> |<span data-ttu-id="a4578-980">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-980">Integer</span></span> |<span data-ttu-id="a4578-981">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-981">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-982">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="a4578-982">ItemCutOffUpperBound</span></span> |<span data-ttu-id="a4578-983">Definuje položku horní mez pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-983">Defines the item upper bound for the condenser.</span></span> <span data-ttu-id="a4578-984">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-984">See usage condenser above.</span></span> |<span data-ttu-id="a4578-985">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-985">Integer</span></span> |<span data-ttu-id="a4578-986">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-986">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-987">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="a4578-987">UserCutOffLowerBound</span></span> |<span data-ttu-id="a4578-988">Definuje dolní mez uživatele pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-988">Defines the user lower bound for the condenser.</span></span> <span data-ttu-id="a4578-989">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-989">See usage condenser above.</span></span> |<span data-ttu-id="a4578-990">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-990">Integer</span></span> |<span data-ttu-id="a4578-991">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-991">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-992">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="a4578-992">UserCutOffUpperBound</span></span> |<span data-ttu-id="a4578-993">Definuje uživatele horní mez pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-993">Defines the user upper bound for the condenser.</span></span> <span data-ttu-id="a4578-994">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-994">See usage condenser above.</span></span> |<span data-ttu-id="a4578-995">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-995">Integer</span></span> |<span data-ttu-id="a4578-996">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-996">2 or more (0 disable condenser)</span></span> |

##### <a name="1113-recommendation-build-parameters"></a><span data-ttu-id="a4578-997">11.1.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-997">11.1.3.</span></span> <span data-ttu-id="a4578-998">Parametry doporučení sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-998">Recommendation build parameters</span></span>
<span data-ttu-id="a4578-999">Následující tabulka znázorňuje parametry sestavení pro sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-999">The table below depicts the build parameters for recommendation build.</span></span>

| <span data-ttu-id="a4578-1000">Klíč</span><span class="sxs-lookup"><span data-stu-id="a4578-1000">Key</span></span> | <span data-ttu-id="a4578-1001">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-1001">Description</span></span> | <span data-ttu-id="a4578-1002">Typ</span><span class="sxs-lookup"><span data-stu-id="a4578-1002">Type</span></span> | <span data-ttu-id="a4578-1003">Platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1003">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a4578-1004">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="a4578-1004">NumberOfModelIterations</span></span> |<span data-ttu-id="a4578-1005">Počet iterací, které provádí modelu se odráží celkovou dobu výpočtů a přesnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1005">The number of iterations the model performs is reflected by the overall compute time and the model accuracy.</span></span> <span data-ttu-id="a4578-1006">Tím vyšší je číslo, lepší přesnost budete mít, ale bude trvat delší dobu výpočtů.</span><span class="sxs-lookup"><span data-stu-id="a4578-1006">The higher the number, the better accuracy you will get, but the compute time will take longer.</span></span> |<span data-ttu-id="a4578-1007">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1007">Integer</span></span> |<span data-ttu-id="a4578-1008">10-50</span><span class="sxs-lookup"><span data-stu-id="a4578-1008">10-50</span></span> |
| <span data-ttu-id="a4578-1009">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="a4578-1009">NumberOfModelDimensions</span></span> |<span data-ttu-id="a4578-1010">Počet dimenzí má vztah k číslo modelu se pokusí najít v rámci vašich dat funkcí.</span><span class="sxs-lookup"><span data-stu-id="a4578-1010">The number of dimensions relates to the number of 'features' the model will try to find within your data.</span></span> <span data-ttu-id="a4578-1011">Zvýšením počtu dimenzí vám umožní lépe vyladit výsledky do menší clusterů.</span><span class="sxs-lookup"><span data-stu-id="a4578-1011">Increasing the number of dimensions will allow better fine-tuning of the results into smaller clusters.</span></span> <span data-ttu-id="a4578-1012">Však příliš mnoho dimenze zabrání modelu nalézt korelací mezi položkami.</span><span class="sxs-lookup"><span data-stu-id="a4578-1012">However, too many dimensions will prevent the model from finding correlations between items.</span></span> |<span data-ttu-id="a4578-1013">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1013">Integer</span></span> |<span data-ttu-id="a4578-1014">10-40</span><span class="sxs-lookup"><span data-stu-id="a4578-1014">10-40</span></span> |
| <span data-ttu-id="a4578-1015">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="a4578-1015">ItemCutOffLowerBound</span></span> |<span data-ttu-id="a4578-1016">Definuje položku dolní mez pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-1016">Defines the item lower bound for the condenser.</span></span> <span data-ttu-id="a4578-1017">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-1017">See usage condenser above.</span></span> |<span data-ttu-id="a4578-1018">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1018">Integer</span></span> |<span data-ttu-id="a4578-1019">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-1019">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-1020">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="a4578-1020">ItemCutOffUpperBound</span></span> |<span data-ttu-id="a4578-1021">Definuje položku horní mez pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-1021">Defines the item upper bound for the condenser.</span></span> <span data-ttu-id="a4578-1022">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-1022">See usage condenser above.</span></span> |<span data-ttu-id="a4578-1023">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1023">Integer</span></span> |<span data-ttu-id="a4578-1024">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-1024">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-1025">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="a4578-1025">UserCutOffLowerBound</span></span> |<span data-ttu-id="a4578-1026">Definuje dolní mez uživatele pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-1026">Defines the user lower bound for the condenser.</span></span> <span data-ttu-id="a4578-1027">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-1027">See usage condenser above.</span></span> |<span data-ttu-id="a4578-1028">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1028">Integer</span></span> |<span data-ttu-id="a4578-1029">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-1029">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-1030">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="a4578-1030">UserCutOffUpperBound</span></span> |<span data-ttu-id="a4578-1031">Definuje uživatele horní mez pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-1031">Defines the user upper bound for the condenser.</span></span> <span data-ttu-id="a4578-1032">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-1032">See usage condenser above.</span></span> |<span data-ttu-id="a4578-1033">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1033">Integer</span></span> |<span data-ttu-id="a4578-1034">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-1034">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-1035">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-1035">Description</span></span> |<span data-ttu-id="a4578-1036">Vytvořte popis.</span><span class="sxs-lookup"><span data-stu-id="a4578-1036">Build description.</span></span> |<span data-ttu-id="a4578-1037">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a4578-1037">String</span></span> |<span data-ttu-id="a4578-1038">Jakýkoli text, maximální 512 znaků</span><span class="sxs-lookup"><span data-stu-id="a4578-1038">Any text, maximum 512 chars</span></span> |
| <span data-ttu-id="a4578-1039">EnableModelingInsights</span><span class="sxs-lookup"><span data-stu-id="a4578-1039">EnableModelingInsights</span></span> |<span data-ttu-id="a4578-1040">Umožňuje výpočetní metriky pro model doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1040">Allows you to compute metrics on the recommendation model.</span></span> |<span data-ttu-id="a4578-1041">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a4578-1041">Boolean</span></span> |<span data-ttu-id="a4578-1042">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="a4578-1042">True/False</span></span> |
| <span data-ttu-id="a4578-1043">UseFeaturesInModel</span><span class="sxs-lookup"><span data-stu-id="a4578-1043">UseFeaturesInModel</span></span> |<span data-ttu-id="a4578-1044">Určuje funkce lze nastavit v zájmu modelu doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1044">Indicates if features can be used in order to enhance the recommendation model.</span></span> |<span data-ttu-id="a4578-1045">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a4578-1045">Boolean</span></span> |<span data-ttu-id="a4578-1046">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="a4578-1046">True/False</span></span> |
| <span data-ttu-id="a4578-1047">ModelingFeatureList</span><span class="sxs-lookup"><span data-stu-id="a4578-1047">ModelingFeatureList</span></span> |<span data-ttu-id="a4578-1048">Čárkami oddělený seznam názvů funkcí pro použití v sestavení doporučení, aby se zvýšila doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1048">Comma-separated list of feature names to be used in the recommendation build, in order to enhance the recommendation.</span></span> |<span data-ttu-id="a4578-1049">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a4578-1049">String</span></span> |<span data-ttu-id="a4578-1050">Funkce názvy až 512 znaků</span><span class="sxs-lookup"><span data-stu-id="a4578-1050">Feature names, up to 512 chars</span></span> |
| <span data-ttu-id="a4578-1051">AllowColdItemPlacement</span><span class="sxs-lookup"><span data-stu-id="a4578-1051">AllowColdItemPlacement</span></span> |<span data-ttu-id="a4578-1052">Označuje, pokud doporučení by také push cold položky prostřednictvím podobností funkci.</span><span class="sxs-lookup"><span data-stu-id="a4578-1052">Indicates if the recommendation should also push cold items via feature similarity.</span></span> |<span data-ttu-id="a4578-1053">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a4578-1053">Boolean</span></span> |<span data-ttu-id="a4578-1054">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="a4578-1054">True/False</span></span> |
| <span data-ttu-id="a4578-1055">EnableFeatureCorrelation</span><span class="sxs-lookup"><span data-stu-id="a4578-1055">EnableFeatureCorrelation</span></span> |<span data-ttu-id="a4578-1056">Určuje funkce lze nastavit v reasoning.</span><span class="sxs-lookup"><span data-stu-id="a4578-1056">Indicates if features can be used in reasoning.</span></span> |<span data-ttu-id="a4578-1057">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a4578-1057">Boolean</span></span> |<span data-ttu-id="a4578-1058">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="a4578-1058">True/False</span></span> |
| <span data-ttu-id="a4578-1059">ReasoningFeatureList</span><span class="sxs-lookup"><span data-stu-id="a4578-1059">ReasoningFeatureList</span></span> |<span data-ttu-id="a4578-1060">Čárkami oddělený seznam názvů funkcí má být použit pro odůvodnění věty (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1060">Comma-separated list of feature names to be used for reasoning sentences (e.g. recommendation explanations).</span></span> |<span data-ttu-id="a4578-1061">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a4578-1061">String</span></span> |<span data-ttu-id="a4578-1062">Funkce názvy až 512 znaků</span><span class="sxs-lookup"><span data-stu-id="a4578-1062">Feature names, up to 512 chars</span></span> |
| <span data-ttu-id="a4578-1063">EnableU2I</span><span class="sxs-lookup"><span data-stu-id="a4578-1063">EnableU2I</span></span> |<span data-ttu-id="a4578-1064">Povolit přizpůsobené doporučení také znám jako</span><span class="sxs-lookup"><span data-stu-id="a4578-1064">Allow the personalized recommendation a.k.a.</span></span> <span data-ttu-id="a4578-1065">U2I (uživatelům položky doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1065">U2I (user to item recommendations).</span></span> |<span data-ttu-id="a4578-1066">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a4578-1066">Boolean</span></span> |<span data-ttu-id="a4578-1067">Logická hodnota (true výchozí)</span><span class="sxs-lookup"><span data-stu-id="a4578-1067">True/False (default true)</span></span> |

##### <a name="1114-fbt-build-parameters"></a><span data-ttu-id="a4578-1068">11.1.4.</span><span class="sxs-lookup"><span data-stu-id="a4578-1068">11.1.4.</span></span> <span data-ttu-id="a4578-1069">Parametry FBT sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-1069">FBT build parameters</span></span>
<span data-ttu-id="a4578-1070">Následující tabulka znázorňuje parametry sestavení pro sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1070">The table below depicts the build parameters for recommendation build.</span></span>

| <span data-ttu-id="a4578-1071">Klíč</span><span class="sxs-lookup"><span data-stu-id="a4578-1071">Key</span></span> | <span data-ttu-id="a4578-1072">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-1072">Description</span></span> | <span data-ttu-id="a4578-1073">Typ</span><span class="sxs-lookup"><span data-stu-id="a4578-1073">Type</span></span> | <span data-ttu-id="a4578-1074">Platnou hodnotu (výchozí)</span><span class="sxs-lookup"><span data-stu-id="a4578-1074">Valid Value (Default)</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a4578-1075">FbtSupportThreshold</span><span class="sxs-lookup"><span data-stu-id="a4578-1075">FbtSupportThreshold</span></span> |<span data-ttu-id="a4578-1076">Jak konzervativní je modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1076">How conservative the model is.</span></span> <span data-ttu-id="a4578-1077">Počet položek, které mají být považovány za pro modelování společné opakování.</span><span class="sxs-lookup"><span data-stu-id="a4578-1077">Number of co-occurrences of items to be considered for modeling.</span></span> |<span data-ttu-id="a4578-1078">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1078">Integer</span></span> |<span data-ttu-id="a4578-1079">3-50 (6)</span><span class="sxs-lookup"><span data-stu-id="a4578-1079">3-50 (6)</span></span> |
| <span data-ttu-id="a4578-1080">FbtMaxItemSetSize</span><span class="sxs-lookup"><span data-stu-id="a4578-1080">FbtMaxItemSetSize</span></span> |<span data-ttu-id="a4578-1081">Počet položek v sadě časté Bounds.</span><span class="sxs-lookup"><span data-stu-id="a4578-1081">Bounds the number of items in a frequent set.</span></span> |<span data-ttu-id="a4578-1082">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1082">Integer</span></span> |<span data-ttu-id="a4578-1083">2-3 (2)</span><span class="sxs-lookup"><span data-stu-id="a4578-1083">2-3 (2)</span></span> |
| <span data-ttu-id="a4578-1084">FbtMinimalScore</span><span class="sxs-lookup"><span data-stu-id="a4578-1084">FbtMinimalScore</span></span> |<span data-ttu-id="a4578-1085">Minimální skóre, který by měl být sadu často chcete-li být do vrácených výsledků zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="a4578-1085">Minimal score that a frequent set should have in order to be included in the returned results.</span></span> <span data-ttu-id="a4578-1086">Čím lépe.</span><span class="sxs-lookup"><span data-stu-id="a4578-1086">The higher the better.</span></span> |<span data-ttu-id="a4578-1087">Double</span><span class="sxs-lookup"><span data-stu-id="a4578-1087">Double</span></span> |<span data-ttu-id="a4578-1088">0 a vyšší (0)</span><span class="sxs-lookup"><span data-stu-id="a4578-1088">0 and above (0)</span></span> |
| <span data-ttu-id="a4578-1089">FbtSimilarityFunction</span><span class="sxs-lookup"><span data-stu-id="a4578-1089">FbtSimilarityFunction</span></span> |<span data-ttu-id="a4578-1090">Definuje podobností funkci má být používána sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1090">Defines the similarity function to be used by the build.</span></span> <span data-ttu-id="a4578-1091">Navýšení upřednostňuje serendipity, společné výskyt upřednostňuje předvídatelnost a Jaccard je to dobrý kompromis mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="a4578-1091">Lift favors serendipity, Co-occurrence favors predictability, and Jaccard is a nice compromise between the two.</span></span> |<span data-ttu-id="a4578-1092">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a4578-1092">String</span></span> |<span data-ttu-id="a4578-1093">cooccurrence, navýšení, jaccard (navýšení)</span><span class="sxs-lookup"><span data-stu-id="a4578-1093">cooccurrence, lift, jaccard (lift)</span></span> |

### <a name="112-trigger-a-recommendation-build"></a><span data-ttu-id="a4578-1094">11.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-1094">11.2.</span></span> <span data-ttu-id="a4578-1095">Aktivovat Build doporučení</span><span class="sxs-lookup"><span data-stu-id="a4578-1095">Trigger a Recommendation Build</span></span>
  <span data-ttu-id="a4578-1096">Ve výchozím nastavení toto rozhraní API aktivují sestavení modelu doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1096">By default this API will trigger a recommendation model build.</span></span> <span data-ttu-id="a4578-1097">K aktivaci rank sestavení (Chcete-li stanovení skóre funkce), slouží variant rozhraní API sestavení s parametrem typu sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1097">To trigger a rank build (in order to score  features), the build API variant with build type parameter should be used.</span></span>

| <span data-ttu-id="a4578-1098">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1098">HTTP Method</span></span> | <span data-ttu-id="a4578-1099">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1099">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1100">POST</span><span class="sxs-lookup"><span data-stu-id="a4578-1100">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1101">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1101">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |
| <span data-ttu-id="a4578-1102">ZÁHLAVÍ</span><span class="sxs-lookup"><span data-stu-id="a4578-1102">HEADER</span></span> |<span data-ttu-id="a4578-1103">`"Content-Type", "text/xml"`(Pokud odesílá text žádosti)</span><span class="sxs-lookup"><span data-stu-id="a4578-1103">`"Content-Type", "text/xml"` (If sending Request Body)</span></span> |

| <span data-ttu-id="a4578-1104">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1104">Parameter Name</span></span> | <span data-ttu-id="a4578-1105">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1105">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1106">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1106">modelId</span></span> |<span data-ttu-id="a4578-1107">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1107">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1108">userDescription</span><span class="sxs-lookup"><span data-stu-id="a4578-1108">userDescription</span></span> |<span data-ttu-id="a4578-1109">Textové identifikátor katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1109">Textual identifier of the catalog.</span></span> <span data-ttu-id="a4578-1110">Poznámka: Pokud použijete prostory musí zakódovat ho s % 20 místo.</span><span class="sxs-lookup"><span data-stu-id="a4578-1110">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="a4578-1111">Najdete v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1111">See example above.</span></span><br><span data-ttu-id="a4578-1112">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="a4578-1112">Max length: 50</span></span> |
| <span data-ttu-id="a4578-1113">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1113">apiVersion</span></span> |<span data-ttu-id="a4578-1114">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1114">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-1115">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-1115">Request Body</span></span> |<span data-ttu-id="a4578-1116">Pokud je ponecháno prázdné pak sestavení bude proveden výchozí parametry sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1116">If left empty then the build will be executed with the default build parameters.</span></span><br><br><span data-ttu-id="a4578-1117">Pokud chcete nastavit parametry sestavení, odešlete parametry ve formátu XML do datové části jako následující příklad.</span><span class="sxs-lookup"><span data-stu-id="a4578-1117">If you want to set the build parameters, send the parameters as XML into the body as in the following sample.</span></span> <span data-ttu-id="a4578-1118">(Viz část "Sestavení parametry" Další informace o parametrech.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>`</span><span class="sxs-lookup"><span data-stu-id="a4578-1118">(See the "Build parameters" section for an explanation of the parameters.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>`</span></span> |

<span data-ttu-id="a4578-1119">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-1119">**Response**:</span></span>

<span data-ttu-id="a4578-1120">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1120">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1121">Toto je asynchronní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a4578-1121">This is an asynchronous API.</span></span> <span data-ttu-id="a4578-1122">Zobrazí se ID sestavení jako odpověď.</span><span class="sxs-lookup"><span data-stu-id="a4578-1122">You will get a build ID as a response.</span></span> <span data-ttu-id="a4578-1123">Vědět, pokud sestavení skončila, by měly volat rozhraní API "Získat sestavení stavu systému Model" a vyhledejte toto ID sestavení v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a4578-1123">To know when the build has ended, you should call the “Get Builds Status of a Model” API and locate this build ID in the response.</span></span> <span data-ttu-id="a4578-1124">Všimněte si, že sestavení může trvat minut až hodin v závislosti na velikosti dat.</span><span class="sxs-lookup"><span data-stu-id="a4578-1124">Note that a build can take from minutes to hours depending on the size of the data.</span></span>

<span data-ttu-id="a4578-1125">Nemůžete spotřebovat doporučení do sestavení ukončí.</span><span class="sxs-lookup"><span data-stu-id="a4578-1125">You cannot consume recommendations till the build ends.</span></span>

<span data-ttu-id="a4578-1126">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="a4578-1126">Valid build status:</span></span>

* <span data-ttu-id="a4578-1127">Vytvořit - byla vytvořena žádost sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1127">Create - Build request was created.</span></span>
* <span data-ttu-id="a4578-1128">Ve frontě - sestavení žádost byla odeslána a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="a4578-1128">Queued - Build request was sent and it is queued.</span></span>
* <span data-ttu-id="a4578-1129">Probíhá vytváření - sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1129">Building - Build is in progress.</span></span>
* <span data-ttu-id="a4578-1130">Úspěch - sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="a4578-1130">Success - Build ended successfully.</span></span>
* <span data-ttu-id="a4578-1131">Chyba: sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="a4578-1131">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="a4578-1132">Zrušena - sestavení byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="a4578-1132">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="a4578-1133">Zrušení - byla odeslána žádost o zrušení pro sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1133">Cancelling - A cancel request for the build was sent.</span></span>

<span data-ttu-id="a4578-1134">Všimněte si, že ID sestavení naleznete v následující cestě:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="a4578-1134">Note that the build ID can be found under the following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="a4578-1135">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-1135">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="113-trigger-build-recommendation-rank-or-fbt"></a><span data-ttu-id="a4578-1136">11.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-1136">11.3.</span></span> <span data-ttu-id="a4578-1137">Aktivační události sestavení (doporučení, pořadí nebo FBT)</span><span class="sxs-lookup"><span data-stu-id="a4578-1137">Trigger Build (Recommendation, Rank or FBT)</span></span>
| <span data-ttu-id="a4578-1138">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1138">HTTP Method</span></span> | <span data-ttu-id="a4578-1139">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1139">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1140">POST</span><span class="sxs-lookup"><span data-stu-id="a4578-1140">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1141">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1141">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27` |
| <span data-ttu-id="a4578-1142">ZÁHLAVÍ</span><span class="sxs-lookup"><span data-stu-id="a4578-1142">HEADER</span></span> |<span data-ttu-id="a4578-1143">`"Content-Type", "text/xml"`(Pokud odesílá text žádosti)</span><span class="sxs-lookup"><span data-stu-id="a4578-1143">`"Content-Type", "text/xml"` (If sending Request Body)</span></span> |

| <span data-ttu-id="a4578-1144">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1144">Parameter Name</span></span> | <span data-ttu-id="a4578-1145">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1145">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1146">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1146">modelId</span></span> |<span data-ttu-id="a4578-1147">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1147">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1148">userDescription</span><span class="sxs-lookup"><span data-stu-id="a4578-1148">userDescription</span></span> |<span data-ttu-id="a4578-1149">Textové identifikátor katalogu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1149">Textual identifier of the catalog.</span></span> <span data-ttu-id="a4578-1150">Poznámka: Pokud použijete prostory musí zakódovat ho s % 20 místo.</span><span class="sxs-lookup"><span data-stu-id="a4578-1150">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="a4578-1151">Najdete v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1151">See example above.</span></span><br><span data-ttu-id="a4578-1152">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="a4578-1152">Max length: 50</span></span> |
| <span data-ttu-id="a4578-1153">buildType</span><span class="sxs-lookup"><span data-stu-id="a4578-1153">buildType</span></span> |<span data-ttu-id="a4578-1154">Typ sestavení, která má být vyvolán:</span><span class="sxs-lookup"><span data-stu-id="a4578-1154">Type of the build to invoke:</span></span> <br/> <span data-ttu-id="a4578-1155">-"Doporučení" doporučení sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-1155">- 'Recommendation' for recommendation build</span></span> <br> <span data-ttu-id="a4578-1156">-Řazení rank sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-1156">- 'Ranking' for rank build</span></span> <br/> <span data-ttu-id="a4578-1157">-'Fbt' FBT sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-1157">- 'Fbt' for FBT build</span></span> |
| <span data-ttu-id="a4578-1158">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1158">apiVersion</span></span> |<span data-ttu-id="a4578-1159">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1159">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-1160">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-1160">Request Body</span></span> |<span data-ttu-id="a4578-1161">Pokud je ponecháno prázdné pak sestavení bude proveden výchozí parametry sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1161">If left empty then the build will be executed with the default build parameters.</span></span><br><br><span data-ttu-id="a4578-1162">Pokud chcete nastavit parametry sestavení, odešlete je ve formátu XML do datové části jako v následující ukázce.</span><span class="sxs-lookup"><span data-stu-id="a4578-1162">If you want to set build parameters, send them as XML into the body like in the following sample.</span></span> <span data-ttu-id="a4578-1163">(Viz část "Sestavení parametry" vysvětlení a úplný seznam parametrů.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>`</span><span class="sxs-lookup"><span data-stu-id="a4578-1163">(See the "Build parameters" section for an explanation and full list of the parameters.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>`</span></span> |

<span data-ttu-id="a4578-1164">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-1164">**Response**:</span></span>

<span data-ttu-id="a4578-1165">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1165">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1166">Toto je asynchronní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a4578-1166">This is an asynchronous API.</span></span> <span data-ttu-id="a4578-1167">Zobrazí se ID sestavení jako odpověď.</span><span class="sxs-lookup"><span data-stu-id="a4578-1167">You will get a build ID as a response.</span></span> <span data-ttu-id="a4578-1168">Vědět, pokud sestavení skončila, by měly volat rozhraní API "Získat sestavení stavu systému Model" a vyhledejte toto ID sestavení v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a4578-1168">To know when the build has ended, you should call the “Get Builds Status of a Model” API and locate this build ID in the response.</span></span> <span data-ttu-id="a4578-1169">Všimněte si, že sestavení může trvat minut až hodin v závislosti na velikosti dat.</span><span class="sxs-lookup"><span data-stu-id="a4578-1169">Note that a build can take from minutes to hours depending on the size of the data.</span></span>

<span data-ttu-id="a4578-1170">Nemůžete spotřebovat doporučení do sestavení ukončí.</span><span class="sxs-lookup"><span data-stu-id="a4578-1170">You cannot consume recommendations till the build ends.</span></span>

<span data-ttu-id="a4578-1171">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="a4578-1171">Valid build status:</span></span>

* <span data-ttu-id="a4578-1172">Vytvoření – Model byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="a4578-1172">Create - Model was created.</span></span>
* <span data-ttu-id="a4578-1173">Ve frontě - sestavení modelu byla spuštěna a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="a4578-1173">Queued - Model build was triggered and it is queued.</span></span>
* <span data-ttu-id="a4578-1174">Sestavení – Model sestavuje.</span><span class="sxs-lookup"><span data-stu-id="a4578-1174">Building - Model is being built.</span></span>
* <span data-ttu-id="a4578-1175">Úspěch - sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="a4578-1175">Success - Build ended successfully.</span></span>
* <span data-ttu-id="a4578-1176">Chyba: sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="a4578-1176">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="a4578-1177">Zrušena - sestavení byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="a4578-1177">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="a4578-1178">Zrušení - sestavení se ruší.</span><span class="sxs-lookup"><span data-stu-id="a4578-1178">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="a4578-1179">Všimněte si, že ID sestavení naleznete v následující cestě:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="a4578-1179">Note that the build ID can be found under the following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="a4578-1180">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-1180">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
      </entry>
    </feed>




### <a name="114-get-builds-status-of-a-model"></a><span data-ttu-id="a4578-1181">11.4.</span><span class="sxs-lookup"><span data-stu-id="a4578-1181">11.4.</span></span> <span data-ttu-id="a4578-1182">Získat stav sestavení modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1182">Get Builds Status of a Model</span></span>
<span data-ttu-id="a4578-1183">Načte sestavení a jejich stavu pro zadaný model.</span><span class="sxs-lookup"><span data-stu-id="a4578-1183">Retrieves builds and their status for a specified model.</span></span>

| <span data-ttu-id="a4578-1184">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1184">HTTP Method</span></span> | <span data-ttu-id="a4578-1185">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1185">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1186">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1186">GET</span></span> |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1187">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1187">Example:</span></span><br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1188">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1188">Parameter Name</span></span> | <span data-ttu-id="a4578-1189">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1189">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1190">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1190">modelId</span></span> |<span data-ttu-id="a4578-1191">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1191">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1192">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="a4578-1192">onlyLastBuild</span></span> |<span data-ttu-id="a4578-1193">Označuje, zda vrátit veškerá historie sestavení modelu nebo pouze stav poslední sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-1193">Indicates whether to return all the build history of the model or only the status of the most recent build</span></span> |
| <span data-ttu-id="a4578-1194">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1194">apiVersion</span></span> |<span data-ttu-id="a4578-1195">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1195">1.0</span></span> |

<span data-ttu-id="a4578-1196">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-1196">**Response**:</span></span>

<span data-ttu-id="a4578-1197">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1197">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1198">Odpověď obsahuje jeden záznam za sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1198">The response includes one entry per build.</span></span> <span data-ttu-id="a4578-1199">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1199">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1200">`feed/entry/content/properties/UserName`-Název uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-1200">`feed/entry/content/properties/UserName` - Name of the user.</span></span>
* <span data-ttu-id="a4578-1201">`feed/entry/content/properties/ModelName`-Název modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1201">`feed/entry/content/properties/ModelName` - Name of the model.</span></span>
* <span data-ttu-id="a4578-1202">`feed/entry/content/properties/ModelId`-Model jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a4578-1202">`feed/entry/content/properties/ModelId` - Model unique identifier.</span></span>
* <span data-ttu-id="a4578-1203">`feed/entry/content/properties/IsDeployed`– Jestli (také známa jako nasazení sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-1203">`feed/entry/content/properties/IsDeployed` - Whether the build is deployed (a.k.a.</span></span> <span data-ttu-id="a4578-1204">aktivní sestavení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1204">active build).</span></span>
* <span data-ttu-id="a4578-1205">`feed/entry/content/properties/BuildId`-Vytvořte jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a4578-1205">`feed/entry/content/properties/BuildId` - Build unique identifier.</span></span>
* <span data-ttu-id="a4578-1206">`feed/entry/content/properties/BuildType`-Typ sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1206">`feed/entry/content/properties/BuildType` - Type of the build.</span></span>
* <span data-ttu-id="a4578-1207">`feed/entry/content/properties/Status`-Stav sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1207">`feed/entry/content/properties/Status` - Build status.</span></span> <span data-ttu-id="a4578-1208">Může být jedna z následujících akcí: Chyba, sestavování, zařazeno do fronty, Cancelling, zrušeno, úspěch.</span><span class="sxs-lookup"><span data-stu-id="a4578-1208">Can be one of the following: Error, Building, Queued, Cancelling, Cancelled, Success.</span></span>
* <span data-ttu-id="a4578-1209">`feed/entry/content/properties/StatusMessage`-Zpráva podrobné informace o stavu (platí pouze pro konkrétní stavy).</span><span class="sxs-lookup"><span data-stu-id="a4578-1209">`feed/entry/content/properties/StatusMessage` - Detailed status message (applies only to specific states).</span></span>
* <span data-ttu-id="a4578-1210">`feed/entry/content/properties/Progress`-Vytvořte průběh (%).</span><span class="sxs-lookup"><span data-stu-id="a4578-1210">`feed/entry/content/properties/Progress` - Build progress (%).</span></span>
* <span data-ttu-id="a4578-1211">`feed/entry/content/properties/StartTime`-Čas spuštění sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1211">`feed/entry/content/properties/StartTime` - Build start time.</span></span>
* <span data-ttu-id="a4578-1212">`feed/entry/content/properties/EndTime`-Vytvořte koncový čas.</span><span class="sxs-lookup"><span data-stu-id="a4578-1212">`feed/entry/content/properties/EndTime` - Build end time.</span></span>
* <span data-ttu-id="a4578-1213">`feed/entry/content/properties/ExecutionTime`-Vytvořte doba trvání.</span><span class="sxs-lookup"><span data-stu-id="a4578-1213">`feed/entry/content/properties/ExecutionTime` - Build duration.</span></span>
* <span data-ttu-id="a4578-1214">`feed/entry/content/properties/ProgressStep`-Podrobnosti o aktuální fáze sestavení v průběhu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1214">`feed/entry/content/properties/ProgressStep` - Details about the current stage of a build in progress.</span></span>

<span data-ttu-id="a4578-1215">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="a4578-1215">Valid build status:</span></span>

* <span data-ttu-id="a4578-1216">Vytvořit - sestavení požadavek položka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="a4578-1216">Created - Build request entry was created.</span></span>
* <span data-ttu-id="a4578-1217">Ve frontě - sestavení požadavku byla spuštěna a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="a4578-1217">Queued - Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="a4578-1218">Probíhá vytváření - sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1218">Building - Build is in process.</span></span>
* <span data-ttu-id="a4578-1219">Úspěch - sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="a4578-1219">Success - Build ended successfully.</span></span>
* <span data-ttu-id="a4578-1220">Chyba: sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="a4578-1220">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="a4578-1221">Zrušena - sestavení byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="a4578-1221">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="a4578-1222">Zrušení - sestavení se ruší.</span><span class="sxs-lookup"><span data-stu-id="a4578-1222">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="a4578-1223">Platné hodnoty pro typ sestavení:</span><span class="sxs-lookup"><span data-stu-id="a4578-1223">Valid values for build type:</span></span>

* <span data-ttu-id="a4578-1224">Pořadí - pořadí sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1224">Rank - Rank build.</span></span>
* <span data-ttu-id="a4578-1225">Doporučení - doporučení sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1225">Recommendation - Recommendation build.</span></span>

<span data-ttu-id="a4578-1226">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-1226">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T17:51:10Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T17:51:10Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
                    <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
                    <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
                    <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
                    <d:BuildId m:type="Edm.String">1000272</d:BuildId>
                    <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
                    <d:Status m:type="Edm.String">Success</d:Status>
                    <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
                    <d:Progress m:type="Edm.String">0</d:Progress>
                    <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
                    <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
                    <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
                    <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
                    <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
                </m:properties>
            </content>
        </entry>
    </feed>


### <a name="115-get-builds-status"></a><span data-ttu-id="a4578-1227">11.5.</span><span class="sxs-lookup"><span data-stu-id="a4578-1227">11.5.</span></span> <span data-ttu-id="a4578-1228">Získat stav sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-1228">Get Builds Status</span></span>
<span data-ttu-id="a4578-1229">Načte sestavení stavy všech modelů uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-1229">Retrieves build statuses of all models of a user.</span></span>

| <span data-ttu-id="a4578-1230">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1230">HTTP Method</span></span> | <span data-ttu-id="a4578-1231">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1231">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1232">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1232">GET</span></span> |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1233">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1233">Example:</span></span><br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1234">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1234">Parameter Name</span></span> | <span data-ttu-id="a4578-1235">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1235">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1236">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="a4578-1236">onlyLastBuild</span></span> |<span data-ttu-id="a4578-1237">Označuje, zda vrátit veškerá historie sestavení modelu nebo pouze stav poslední sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1237">Indicates whether to return all the build history of the model or only the status of the most recent build.</span></span> |
| <span data-ttu-id="a4578-1238">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1238">apiVersion</span></span> |<span data-ttu-id="a4578-1239">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1239">1.0</span></span> |

<span data-ttu-id="a4578-1240">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-1240">**Response**:</span></span>

<span data-ttu-id="a4578-1241">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1241">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1242">Odpověď obsahuje jeden záznam za sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1242">The response includes one entry per build.</span></span> <span data-ttu-id="a4578-1243">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1243">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1244">`feed/entry/content/properties/UserName`-Název uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-1244">`feed/entry/content/properties/UserName` - Name of the user.</span></span>
* <span data-ttu-id="a4578-1245">`feed/entry/content/properties/ModelName`-Název modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1245">`feed/entry/content/properties/ModelName` - Name of the model.</span></span>
* <span data-ttu-id="a4578-1246">`feed/entry/content/properties/ModelId`-Model jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a4578-1246">`feed/entry/content/properties/ModelId` - Model unique identifier.</span></span>
* <span data-ttu-id="a4578-1247">`feed/entry/content/properties/IsDeployed`– Jestli sestavení nasazeno.</span><span class="sxs-lookup"><span data-stu-id="a4578-1247">`feed/entry/content/properties/IsDeployed` - Whether the build is deployed.</span></span>
* <span data-ttu-id="a4578-1248">`feed/entry/content/properties/BuildId`-Vytvořte jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a4578-1248">`feed/entry/content/properties/BuildId` - Build unique identifier.</span></span>
* <span data-ttu-id="a4578-1249">`feed/entry/content/properties/BuildType`-Typ sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1249">`feed/entry/content/properties/BuildType` - Type of the build.</span></span>
* <span data-ttu-id="a4578-1250">`feed/entry/content/properties/Status`-Stav sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1250">`feed/entry/content/properties/Status` - Build status.</span></span> <span data-ttu-id="a4578-1251">Může být jedna z následujících akcí: Chyba, sestavování, zařazeno do fronty, zrušeno, Cancelling, úspěch.</span><span class="sxs-lookup"><span data-stu-id="a4578-1251">Can be one of the following: Error, Building, Queued, Cancelled, Cancelling, Success.</span></span>
* <span data-ttu-id="a4578-1252">`feed/entry/content/properties/StatusMessage`-Zpráva podrobné informace o stavu (platí pouze pro konkrétní stavy).</span><span class="sxs-lookup"><span data-stu-id="a4578-1252">`feed/entry/content/properties/StatusMessage` - Detailed status message (applies only to specific states).</span></span>
* <span data-ttu-id="a4578-1253">`feed/entry/content/properties/Progress`-Vytvořte průběh (%).</span><span class="sxs-lookup"><span data-stu-id="a4578-1253">`feed/entry/content/properties/Progress` - Build progress (%).</span></span>
* <span data-ttu-id="a4578-1254">`feed/entry/content/properties/StartTime`-Čas spuštění sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1254">`feed/entry/content/properties/StartTime` - Build start time.</span></span>
* <span data-ttu-id="a4578-1255">`feed/entry/content/properties/EndTime`-Vytvořte koncový čas.</span><span class="sxs-lookup"><span data-stu-id="a4578-1255">`feed/entry/content/properties/EndTime` - Build end time.</span></span>
* <span data-ttu-id="a4578-1256">`feed/entry/content/properties/ExecutionTime`-Vytvořte doba trvání.</span><span class="sxs-lookup"><span data-stu-id="a4578-1256">`feed/entry/content/properties/ExecutionTime` - Build duration.</span></span>
* <span data-ttu-id="a4578-1257">`feed/entry/content/properties/ProgressStep`-Podrobnosti o aktuální fáze sestavení v průběhu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1257">`feed/entry/content/properties/ProgressStep` - Details about the current stage of a build in progress.</span></span>

<span data-ttu-id="a4578-1258">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="a4578-1258">Valid build status:</span></span>

* <span data-ttu-id="a4578-1259">Vytvořit - sestavení požadavek položka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="a4578-1259">Created - Build request entry was created.</span></span>
* <span data-ttu-id="a4578-1260">Ve frontě - sestavení požadavku byla spuštěna a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="a4578-1260">Queued - Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="a4578-1261">Probíhá vytváření - sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1261">Building - Build is in process.</span></span>
* <span data-ttu-id="a4578-1262">Úspěch - sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="a4578-1262">Success - Build ended successfully.</span></span>
* <span data-ttu-id="a4578-1263">Chyba: sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="a4578-1263">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="a4578-1264">Zrušena - sestavení byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="a4578-1264">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="a4578-1265">Zrušení - sestavení se ruší.</span><span class="sxs-lookup"><span data-stu-id="a4578-1265">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="a4578-1266">Platné hodnoty pro typ sestavení:</span><span class="sxs-lookup"><span data-stu-id="a4578-1266">Valid values for build type:</span></span>

* <span data-ttu-id="a4578-1267">Pořadí - pořadí sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1267">Rank - Rank build.</span></span>
* <span data-ttu-id="a4578-1268">Doporučení - doporučení sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1268">Recommendation - Recommendation build.</span></span>

<span data-ttu-id="a4578-1269">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-1269">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T18:41:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T18:41:21Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
                    <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
                    <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
                    <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
                    <d:BuildId m:type="Edm.String">1000272</d:BuildId>
                    <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
                    <d:Status m:type="Edm.String">Success</d:Status>
                    <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
                    <d:Progress m:type="Edm.String">0</d:Progress>
                    <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
                    <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
                    <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
                    <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
                    <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
                </m:properties>
            </content>
        </entry>
    </feed>


### <a name="116-delete-build"></a><span data-ttu-id="a4578-1270">11.6.</span><span class="sxs-lookup"><span data-stu-id="a4578-1270">11.6.</span></span> <span data-ttu-id="a4578-1271">Odstranění sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-1271">Delete Build</span></span>
<span data-ttu-id="a4578-1272">Odstraní sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1272">Deletes a build.</span></span>

<span data-ttu-id="a4578-1273">POZNÁMKA:</span><span class="sxs-lookup"><span data-stu-id="a4578-1273">NOTE:</span></span> <br><span data-ttu-id="a4578-1274">Nelze odstranit aktivní sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1274">You cannot delete an active build.</span></span> <span data-ttu-id="a4578-1275">Model je třeba aktualizovat na různých active Build před odstraněním.</span><span class="sxs-lookup"><span data-stu-id="a4578-1275">The model should be updated to a different active build before you delete it.</span></span><br><span data-ttu-id="a4578-1276">Nelze odstranit v průběhu sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1276">You cannot delete an in-progress build.</span></span> <span data-ttu-id="a4578-1277">Sestavení by měl zrušit nejdřív voláním <strong>zrušit sestavení</strong>.</span><span class="sxs-lookup"><span data-stu-id="a4578-1277">You should cancel the build first by calling <strong>Cancel Build</strong>.</span></span>

| <span data-ttu-id="a4578-1278">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1278">HTTP Method</span></span> | <span data-ttu-id="a4578-1279">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1279">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1280">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="a4578-1280">DELETE</span></span> |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1281">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1281">Example:</span></span><br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1282">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1282">Parameter Name</span></span> | <span data-ttu-id="a4578-1283">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1283">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1284">buildId</span><span class="sxs-lookup"><span data-stu-id="a4578-1284">buildId</span></span> |<span data-ttu-id="a4578-1285">Jedinečný identifikátor sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1285">Unique identifier of the build.</span></span> |
| <span data-ttu-id="a4578-1286">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1286">apiVersion</span></span> |<span data-ttu-id="a4578-1287">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1287">1.0</span></span> |

<span data-ttu-id="a4578-1288">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1288">**Response:**</span></span>

<span data-ttu-id="a4578-1289">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1289">HTTP Status code: 200</span></span>

### <a name="117-cancel-build"></a><span data-ttu-id="a4578-1290">11.7.</span><span class="sxs-lookup"><span data-stu-id="a4578-1290">11.7.</span></span> <span data-ttu-id="a4578-1291">Zrušit sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-1291">Cancel Build</span></span>
<span data-ttu-id="a4578-1292">Zruší sestavení se při vytváření stavu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1292">Cancels a build that is in building status.</span></span>

| <span data-ttu-id="a4578-1293">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1293">HTTP Method</span></span> | <span data-ttu-id="a4578-1294">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1294">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1295">PUT</span><span class="sxs-lookup"><span data-stu-id="a4578-1295">PUT</span></span> |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1296">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1296">Example:</span></span><br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1297">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1297">Parameter Name</span></span> | <span data-ttu-id="a4578-1298">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1298">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1299">buildId</span><span class="sxs-lookup"><span data-stu-id="a4578-1299">buildId</span></span> |<span data-ttu-id="a4578-1300">Jedinečný identifikátor sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1300">Unique identifier of the build.</span></span> |
| <span data-ttu-id="a4578-1301">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1301">apiVersion</span></span> |<span data-ttu-id="a4578-1302">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1302">1.0</span></span> |

<span data-ttu-id="a4578-1303">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1303">**Response:**</span></span>

<span data-ttu-id="a4578-1304">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1304">HTTP Status code: 200</span></span>

### <a name="118-get-build-parameters"></a><span data-ttu-id="a4578-1305">11.8.</span><span class="sxs-lookup"><span data-stu-id="a4578-1305">11.8.</span></span> <span data-ttu-id="a4578-1306">Získávání parametrů sestavení</span><span class="sxs-lookup"><span data-stu-id="a4578-1306">Get Build Parameters</span></span>
<span data-ttu-id="a4578-1307">Načte sestavení parametry.</span><span class="sxs-lookup"><span data-stu-id="a4578-1307">Retrieves build parameters.</span></span>

| <span data-ttu-id="a4578-1308">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1308">HTTP Method</span></span> | <span data-ttu-id="a4578-1309">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1309">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1310">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1310">GET</span></span> |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1311">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1311">Example:</span></span><br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1312">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1312">Parameter Name</span></span> | <span data-ttu-id="a4578-1313">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1313">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1314">buildId</span><span class="sxs-lookup"><span data-stu-id="a4578-1314">buildId</span></span> |<span data-ttu-id="a4578-1315">Jedinečný identifikátor sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1315">Unique identifier of the build.</span></span> |
| <span data-ttu-id="a4578-1316">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1316">apiVersion</span></span> |<span data-ttu-id="a4578-1317">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1317">1.0</span></span> |

<span data-ttu-id="a4578-1318">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1318">**Response:**</span></span>

<span data-ttu-id="a4578-1319">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1319">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1320">Toto rozhraní API vrátí kolekci elementů klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="a4578-1320">This API returns a collection of key/value elements.</span></span> <span data-ttu-id="a4578-1321">Každý prvek představuje parametr a její hodnotu:</span><span class="sxs-lookup"><span data-stu-id="a4578-1321">Each element represents a parameter and its value:</span></span>

* <span data-ttu-id="a4578-1322">`feed/entry/content/properties/Key`-Vytvořte název parametru.</span><span class="sxs-lookup"><span data-stu-id="a4578-1322">`feed/entry/content/properties/Key` - Build parameter name.</span></span>
* <span data-ttu-id="a4578-1323">`feed/entry/content/properties/Value`-Vytvořte hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="a4578-1323">`feed/entry/content/properties/Value` - Build parameter value.</span></span>

<span data-ttu-id="a4578-1324">Následující tabulka znázorňuje hodnotu, která představuje každý klíč.</span><span class="sxs-lookup"><span data-stu-id="a4578-1324">The table below depicts the value that each key represents.</span></span>

| <span data-ttu-id="a4578-1325">Klíč</span><span class="sxs-lookup"><span data-stu-id="a4578-1325">Key</span></span> | <span data-ttu-id="a4578-1326">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-1326">Description</span></span> | <span data-ttu-id="a4578-1327">Typ</span><span class="sxs-lookup"><span data-stu-id="a4578-1327">Type</span></span> | <span data-ttu-id="a4578-1328">Platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1328">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a4578-1329">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="a4578-1329">NumberOfModelIterations</span></span> |<span data-ttu-id="a4578-1330">Počet iterací, které provádí modelu se odráží celkovou dobu výpočtů a přesnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1330">The number of iterations the model performs is reflected by the overall compute time and the model accuracy.</span></span> <span data-ttu-id="a4578-1331">Tím vyšší je číslo, lepší přesnost budete mít, ale bude trvat delší dobu výpočtů.</span><span class="sxs-lookup"><span data-stu-id="a4578-1331">The higher the number, the better accuracy you will get, but the compute time will take longer.</span></span> |<span data-ttu-id="a4578-1332">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1332">Integer</span></span> |<span data-ttu-id="a4578-1333">10-50</span><span class="sxs-lookup"><span data-stu-id="a4578-1333">10-50</span></span> |
| <span data-ttu-id="a4578-1334">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="a4578-1334">NumberOfModelDimensions</span></span> |<span data-ttu-id="a4578-1335">Počet dimenzí má vztah k číslo modelu se pokusí najít v rámci vašich dat funkcí.</span><span class="sxs-lookup"><span data-stu-id="a4578-1335">The number of dimensions relates to the number of 'features' the model will try to find within your data.</span></span> <span data-ttu-id="a4578-1336">Zvýšením počtu dimenzí vám umožní lépe vyladit výsledky do menší clusterů.</span><span class="sxs-lookup"><span data-stu-id="a4578-1336">Increasing the number of dimensions will allow better fine-tuning of the results into smaller clusters.</span></span> <span data-ttu-id="a4578-1337">Však příliš mnoho dimenze zabrání modelu nalézt korelací mezi položkami.</span><span class="sxs-lookup"><span data-stu-id="a4578-1337">However, too many dimensions will prevent the model from finding correlations between items.</span></span> |<span data-ttu-id="a4578-1338">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1338">Integer</span></span> |<span data-ttu-id="a4578-1339">10-40</span><span class="sxs-lookup"><span data-stu-id="a4578-1339">10-40</span></span> |
| <span data-ttu-id="a4578-1340">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="a4578-1340">ItemCutOffLowerBound</span></span> |<span data-ttu-id="a4578-1341">Definuje položku dolní mez pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-1341">Defines the item lower bound for the condenser.</span></span> <span data-ttu-id="a4578-1342">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-1342">See usage condenser above.</span></span> |<span data-ttu-id="a4578-1343">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1343">Integer</span></span> |<span data-ttu-id="a4578-1344">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-1344">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-1345">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="a4578-1345">ItemCutOffUpperBound</span></span> |<span data-ttu-id="a4578-1346">Definuje položku horní mez pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-1346">Defines the item upper bound for the condenser.</span></span> <span data-ttu-id="a4578-1347">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-1347">See usage condenser above.</span></span> |<span data-ttu-id="a4578-1348">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1348">Integer</span></span> |<span data-ttu-id="a4578-1349">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-1349">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-1350">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="a4578-1350">UserCutOffLowerBound</span></span> |<span data-ttu-id="a4578-1351">Definuje dolní mez uživatele pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-1351">Defines the user lower bound for the condenser.</span></span> <span data-ttu-id="a4578-1352">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-1352">See usage condenser above.</span></span> |<span data-ttu-id="a4578-1353">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1353">Integer</span></span> |<span data-ttu-id="a4578-1354">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-1354">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-1355">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="a4578-1355">UserCutOffUpperBound</span></span> |<span data-ttu-id="a4578-1356">Definuje uživatele horní mez pro chladič.</span><span class="sxs-lookup"><span data-stu-id="a4578-1356">Defines the user upper bound for the condenser.</span></span> <span data-ttu-id="a4578-1357">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="a4578-1357">See usage condenser above.</span></span> |<span data-ttu-id="a4578-1358">Integer</span><span class="sxs-lookup"><span data-stu-id="a4578-1358">Integer</span></span> |<span data-ttu-id="a4578-1359">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="a4578-1359">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="a4578-1360">Popis</span><span class="sxs-lookup"><span data-stu-id="a4578-1360">Description</span></span> |<span data-ttu-id="a4578-1361">Vytvořte popis.</span><span class="sxs-lookup"><span data-stu-id="a4578-1361">Build description.</span></span> |<span data-ttu-id="a4578-1362">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a4578-1362">String</span></span> |<span data-ttu-id="a4578-1363">Jakýkoli text, maximální 512 znaků</span><span class="sxs-lookup"><span data-stu-id="a4578-1363">Any text, maximum 512 chars</span></span> |
| <span data-ttu-id="a4578-1364">EnableModelingInsights</span><span class="sxs-lookup"><span data-stu-id="a4578-1364">EnableModelingInsights</span></span> |<span data-ttu-id="a4578-1365">Umožňuje výpočetní metriky pro model doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1365">Allows you to compute metrics on the recommendation model.</span></span> |<span data-ttu-id="a4578-1366">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a4578-1366">Boolean</span></span> |<span data-ttu-id="a4578-1367">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="a4578-1367">True/False</span></span> |
| <span data-ttu-id="a4578-1368">UseFeaturesInModel</span><span class="sxs-lookup"><span data-stu-id="a4578-1368">UseFeaturesInModel</span></span> |<span data-ttu-id="a4578-1369">Určuje funkce lze nastavit v zájmu modelu doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1369">Indicates if features can be used in order to enhance the recommendation model.</span></span> |<span data-ttu-id="a4578-1370">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a4578-1370">Boolean</span></span> |<span data-ttu-id="a4578-1371">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="a4578-1371">True/False</span></span> |
| <span data-ttu-id="a4578-1372">ModelingFeatureList</span><span class="sxs-lookup"><span data-stu-id="a4578-1372">ModelingFeatureList</span></span> |<span data-ttu-id="a4578-1373">Čárkami oddělený seznam názvů funkcí pro použití v sestavení doporučení, aby se zvýšila doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1373">Comma-separated list of feature names to be used in the recommendation build, in order to enhance the recommendation.</span></span> |<span data-ttu-id="a4578-1374">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a4578-1374">String</span></span> |<span data-ttu-id="a4578-1375">Funkce názvy až 512 znaků</span><span class="sxs-lookup"><span data-stu-id="a4578-1375">Feature names, up to 512 chars</span></span> |
| <span data-ttu-id="a4578-1376">AllowColdItemPlacement</span><span class="sxs-lookup"><span data-stu-id="a4578-1376">AllowColdItemPlacement</span></span> |<span data-ttu-id="a4578-1377">Označuje, pokud doporučení by také push cold položky prostřednictvím podobností funkci.</span><span class="sxs-lookup"><span data-stu-id="a4578-1377">Indicates if the recommendation should also push cold items via feature similarity.</span></span> |<span data-ttu-id="a4578-1378">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a4578-1378">Boolean</span></span> |<span data-ttu-id="a4578-1379">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="a4578-1379">True/False</span></span> |
| <span data-ttu-id="a4578-1380">EnableFeatureCorrelation</span><span class="sxs-lookup"><span data-stu-id="a4578-1380">EnableFeatureCorrelation</span></span> |<span data-ttu-id="a4578-1381">Určuje funkce lze nastavit v reasoning.</span><span class="sxs-lookup"><span data-stu-id="a4578-1381">Indicates if features can be used in reasoning.</span></span> |<span data-ttu-id="a4578-1382">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a4578-1382">Boolean</span></span> |<span data-ttu-id="a4578-1383">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="a4578-1383">True/False</span></span> |
| <span data-ttu-id="a4578-1384">ReasoningFeatureList</span><span class="sxs-lookup"><span data-stu-id="a4578-1384">ReasoningFeatureList</span></span> |<span data-ttu-id="a4578-1385">Čárkami oddělený seznam názvů funkcí má být použit pro odůvodnění věty (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1385">Comma-separated list of feature names to be used for reasoning sentences (e.g. recommendation explanations).</span></span> |<span data-ttu-id="a4578-1386">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a4578-1386">String</span></span> |<span data-ttu-id="a4578-1387">Funkce názvy až 512 znaků</span><span class="sxs-lookup"><span data-stu-id="a4578-1387">Feature names, up to 512 chars</span></span> |

<span data-ttu-id="a4578-1388">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-1388">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get build parameters</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:50:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UseFeaturesInModel</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">AllowColdItemPlacement</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableFeatureCorrelation</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableModelingInsights</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelIterations</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
            <titletype="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelDimensions</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <linkrel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ModelingFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ReasoningFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">Description</d:Key>
                    <d:Value m:type="Edm.String">rankBuild</d:Value>
                </m:properties>
            </content>
        </entry>
    </feed>

## <a name="12-recommendation"></a><span data-ttu-id="a4578-1389">12. Doporučení</span><span class="sxs-lookup"><span data-stu-id="a4578-1389">12. Recommendation</span></span>
### <a name="121-get-item-recommendations-for-active-build"></a><span data-ttu-id="a4578-1390">12.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-1390">12.1.</span></span> <span data-ttu-id="a4578-1391">Získání položky doporučení (pro aktivní sestavení)</span><span class="sxs-lookup"><span data-stu-id="a4578-1391">Get Item Recommendations (for active build)</span></span>
<span data-ttu-id="a4578-1392">Získejte doporučení active sestavení typu "Doporučení" nebo "Fbt" založené na seznam položek semen (vstupu).</span><span class="sxs-lookup"><span data-stu-id="a4578-1392">Get recommendations of the active build of type "Recommendation" or "Fbt" based on a list of seeds (input) items.</span></span>

| <span data-ttu-id="a4578-1393">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1393">HTTP Method</span></span> | <span data-ttu-id="a4578-1394">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1394">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1395">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1395">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1396">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1396">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1397">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1397">Parameter Name</span></span> | <span data-ttu-id="a4578-1398">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1398">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1399">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1399">modelId</span></span> |<span data-ttu-id="a4578-1400">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1400">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1401">položky ItemID</span><span class="sxs-lookup"><span data-stu-id="a4578-1401">itemIds</span></span> |<span data-ttu-id="a4578-1402">Textový soubor s oddělovači seznam položek, doporučujeme pro.</span><span class="sxs-lookup"><span data-stu-id="a4578-1402">Comma-separated list of the items to recommend for.</span></span> <br><span data-ttu-id="a4578-1403">Pokud je aktivní sestavení typu FBT můžete odeslat pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="a4578-1403">If the active build is of type FBT then you can send only one item.</span></span> <br><span data-ttu-id="a4578-1404">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="a4578-1404">Max length: 1024</span></span> |
| <span data-ttu-id="a4578-1405">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="a4578-1405">numberOfResults</span></span> |<span data-ttu-id="a4578-1406">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="a4578-1406">Number of required results</span></span> <br> <span data-ttu-id="a4578-1407">Maximální počet: 150</span><span class="sxs-lookup"><span data-stu-id="a4578-1407">Max: 150</span></span> |
| <span data-ttu-id="a4578-1408">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="a4578-1408">includeMetatadata</span></span> |<span data-ttu-id="a4578-1409">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="a4578-1409">Future use, always false</span></span> |
| <span data-ttu-id="a4578-1410">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1410">apiVersion</span></span> |<span data-ttu-id="a4578-1411">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1411">1.0</span></span> |

<span data-ttu-id="a4578-1412">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1412">**Response:**</span></span>

<span data-ttu-id="a4578-1413">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1413">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1414">Odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1414">The response includes one entry per recommended item.</span></span> <span data-ttu-id="a4578-1415">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1415">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1416">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1416">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="a4578-1417">`Feed\entry\content\properties\Name`-Název položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1417">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="a4578-1418">`Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-1418">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="a4578-1419">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1419">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="a4578-1420">Následující příklad odpověď obsahuje 10 doporučené položek.</span><span class="sxs-lookup"><span data-stu-id="a4578-1420">The example response below includes 10 recommended items.</span></span>

<span data-ttu-id="a4578-1421">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-1421">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="122-get-item-recommendations-of-a-specific-build"></a><span data-ttu-id="a4578-1422">12.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-1422">12.2.</span></span> <span data-ttu-id="a4578-1423">Získání položky doporučení (konkrétní sestavení)</span><span class="sxs-lookup"><span data-stu-id="a4578-1423">Get Item Recommendations (of a specific build)</span></span>
<span data-ttu-id="a4578-1424">Získejte doporučení konkrétní sestavení typu "Doporučení" nebo "Fbt".</span><span class="sxs-lookup"><span data-stu-id="a4578-1424">Get recommendations of a specific build of type "Recommendation" or "Fbt".</span></span>

| <span data-ttu-id="a4578-1425">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1425">HTTP Method</span></span> | <span data-ttu-id="a4578-1426">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1426">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1427">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1427">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1428">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1428">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1429">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1429">Parameter Name</span></span> | <span data-ttu-id="a4578-1430">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1430">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1431">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1431">modelId</span></span> |<span data-ttu-id="a4578-1432">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1432">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1433">položky ItemID</span><span class="sxs-lookup"><span data-stu-id="a4578-1433">itemIds</span></span> |<span data-ttu-id="a4578-1434">Textový soubor s oddělovači seznam položek, doporučujeme pro.</span><span class="sxs-lookup"><span data-stu-id="a4578-1434">Comma-separated list of the items to recommend for.</span></span> <br><span data-ttu-id="a4578-1435">Pokud je aktivní sestavení typu FBT můžete odeslat pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="a4578-1435">If the active build is of type FBT then you can send only one item.</span></span> <br><span data-ttu-id="a4578-1436">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="a4578-1436">Max length: 1024</span></span> |
| <span data-ttu-id="a4578-1437">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="a4578-1437">numberOfResults</span></span> |<span data-ttu-id="a4578-1438">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="a4578-1438">Number of required results</span></span> <br> <span data-ttu-id="a4578-1439">Maximální počet: 150</span><span class="sxs-lookup"><span data-stu-id="a4578-1439">Max: 150</span></span> |
| <span data-ttu-id="a4578-1440">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="a4578-1440">includeMetatadata</span></span> |<span data-ttu-id="a4578-1441">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="a4578-1441">Future use, always false</span></span> |
| <span data-ttu-id="a4578-1442">buildId</span><span class="sxs-lookup"><span data-stu-id="a4578-1442">buildId</span></span> |<span data-ttu-id="a4578-1443">id sestavení, který má používat pro tento požadavek doporučení</span><span class="sxs-lookup"><span data-stu-id="a4578-1443">the build id to use for this recommendation request</span></span> |
| <span data-ttu-id="a4578-1444">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1444">apiVersion</span></span> |<span data-ttu-id="a4578-1445">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1445">1.0</span></span> |

<span data-ttu-id="a4578-1446">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1446">**Response:**</span></span>

<span data-ttu-id="a4578-1447">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1447">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1448">Odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1448">The response includes one entry per recommended item.</span></span> <span data-ttu-id="a4578-1449">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1449">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1450">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1450">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="a4578-1451">`Feed\entry\content\properties\Name`-Název položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1451">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="a4578-1452">`Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-1452">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="a4578-1453">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1453">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="a4578-1454">Podívejte se na příklad odpovědi v 12.1</span><span class="sxs-lookup"><span data-stu-id="a4578-1454">See a response example in 12.1</span></span>

### <a name="123-get-fbt-recommendations-for-active-build"></a><span data-ttu-id="a4578-1455">12.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-1455">12.3.</span></span> <span data-ttu-id="a4578-1456">Získejte doporučení FBT (pro aktivní sestavení)</span><span class="sxs-lookup"><span data-stu-id="a4578-1456">Get FBT Recommendations (for active build)</span></span>
<span data-ttu-id="a4578-1457">Získejte doporučení active sestavení typu "Fbt" v závislosti na položce počáteční hodnotu (vstup).</span><span class="sxs-lookup"><span data-stu-id="a4578-1457">Get recommendations of the active build of type "Fbt" based on a seed (input) item.</span></span>

| <span data-ttu-id="a4578-1458">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1458">HTTP Method</span></span> | <span data-ttu-id="a4578-1459">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1459">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1460">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1460">GET</span></span> |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1461">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1461">Example:</span></span><br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1462">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1462">Parameter Name</span></span> | <span data-ttu-id="a4578-1463">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1463">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1464">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1464">modelId</span></span> |<span data-ttu-id="a4578-1465">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1465">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1466">itemId</span><span class="sxs-lookup"><span data-stu-id="a4578-1466">itemId</span></span> |<span data-ttu-id="a4578-1467">Doporučujeme pro položku.</span><span class="sxs-lookup"><span data-stu-id="a4578-1467">Item to recommend for.</span></span> <br><span data-ttu-id="a4578-1468">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="a4578-1468">Max length: 1024</span></span> |
| <span data-ttu-id="a4578-1469">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="a4578-1469">numberOfResults</span></span> |<span data-ttu-id="a4578-1470">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="a4578-1470">Number of required results</span></span> <br><span data-ttu-id="a4578-1471">Maximální počet: 150</span><span class="sxs-lookup"><span data-stu-id="a4578-1471">Max: 150</span></span> |
| <span data-ttu-id="a4578-1472">minimalScore</span><span class="sxs-lookup"><span data-stu-id="a4578-1472">minimalScore</span></span> |<span data-ttu-id="a4578-1473">Minimální skóre, který chcete-li být do vrácených výsledků zahrnuty by měl být časté sady</span><span class="sxs-lookup"><span data-stu-id="a4578-1473">Minimal score that a frequent set should have in order to be included in the returned results</span></span> |
| <span data-ttu-id="a4578-1474">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="a4578-1474">includeMetatadata</span></span> |<span data-ttu-id="a4578-1475">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="a4578-1475">Future use, always false</span></span> |
| <span data-ttu-id="a4578-1476">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1476">apiVersion</span></span> |<span data-ttu-id="a4578-1477">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1477">1.0</span></span> |

<span data-ttu-id="a4578-1478">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1478">**Response:**</span></span>

<span data-ttu-id="a4578-1479">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1479">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1480">Odpověď obsahuje jeden záznam za sadu Doporučené položky (sady položek, které jsou obvykle koupili společně s položka počáteční hodnoty nebo vstupních).</span><span class="sxs-lookup"><span data-stu-id="a4578-1480">The response includes one entry per recommended item set (a set of items which are usually bought together with the seed/input item).</span></span> <span data-ttu-id="a4578-1481">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1481">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1482">`Feed\entry\content\properties\Id1`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1482">`Feed\entry\content\properties\Id1` - Recommended item ID.</span></span>
* <span data-ttu-id="a4578-1483">`Feed\entry\content\properties\Name1`-Název položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1483">`Feed\entry\content\properties\Name1` - Name of the item.</span></span>
* <span data-ttu-id="a4578-1484">`Feed\entry\content\properties\Id2`ID položky doporučené 2. (volitelné).</span><span class="sxs-lookup"><span data-stu-id="a4578-1484">`Feed\entry\content\properties\Id2` - 2nd recommended item ID (optional).</span></span>
* <span data-ttu-id="a4578-1485">`Feed\entry\content\properties\Name2`-Název 2. položka (volitelné).</span><span class="sxs-lookup"><span data-stu-id="a4578-1485">`Feed\entry\content\properties\Name2` - Name of the 2nd item (optional).</span></span>
* <span data-ttu-id="a4578-1486">`Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-1486">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="a4578-1487">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1487">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="a4578-1488">Následující příklad odpověď obsahuje 3 sad Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1488">The example response below includes 3 recommended item sets.</span></span>

<span data-ttu-id="a4578-1489">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-1489">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemId='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">159</d:Id1>
        <d:Name1 m:type="Edm.String">159</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">52</d:Id1>
        <d:Name1 m:type="Edm.String">52</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.533343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">35</d:Id1>
        <d:Name1 m:type="Edm.String">35</d:Name1>
        <d:Id2 m:type="Edm.String">102</d:Id2>
        <d:Name2 m:type="Edm.String">102</d:Name2>
        <d:Rating m:type="Edm.Double">0.523343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '35' and '102'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="124-get-fbt-recommendations-of-a-specific-build"></a><span data-ttu-id="a4578-1490">12.4.</span><span class="sxs-lookup"><span data-stu-id="a4578-1490">12.4.</span></span> <span data-ttu-id="a4578-1491">Získejte doporučení FBT (z konkrétní sestavení)</span><span class="sxs-lookup"><span data-stu-id="a4578-1491">Get FBT Recommendations (of a specific build)</span></span>
<span data-ttu-id="a4578-1492">Získejte doporučení konkrétní sestavení typu "Fbt".</span><span class="sxs-lookup"><span data-stu-id="a4578-1492">Get recommendations of a specific build of type "Fbt".</span></span>

| <span data-ttu-id="a4578-1493">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1493">HTTP Method</span></span> | <span data-ttu-id="a4578-1494">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1494">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1495">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1495">GET</span></span> |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1496">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1496">Example:</span></span><br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1497">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1497">Parameter Name</span></span> | <span data-ttu-id="a4578-1498">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1498">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1499">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1499">modelId</span></span> |<span data-ttu-id="a4578-1500">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1500">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1501">itemId</span><span class="sxs-lookup"><span data-stu-id="a4578-1501">itemId</span></span> |<span data-ttu-id="a4578-1502">Doporučujeme pro položku.</span><span class="sxs-lookup"><span data-stu-id="a4578-1502">Item to recommend for.</span></span> <br><span data-ttu-id="a4578-1503">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="a4578-1503">Max length: 1024</span></span> |
| <span data-ttu-id="a4578-1504">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="a4578-1504">numberOfResults</span></span> |<span data-ttu-id="a4578-1505">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="a4578-1505">Number of required results</span></span> <br><span data-ttu-id="a4578-1506">Maximální počet: 150</span><span class="sxs-lookup"><span data-stu-id="a4578-1506">Max: 150</span></span> |
| <span data-ttu-id="a4578-1507">minimalScore</span><span class="sxs-lookup"><span data-stu-id="a4578-1507">minimalScore</span></span> |<span data-ttu-id="a4578-1508">Minimální skóre, který chcete-li být do vrácených výsledků zahrnuty by měl být časté sady</span><span class="sxs-lookup"><span data-stu-id="a4578-1508">Minimal score that a frequent set should have in order to be included in the returned results</span></span> |
| <span data-ttu-id="a4578-1509">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="a4578-1509">includeMetatadata</span></span> |<span data-ttu-id="a4578-1510">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="a4578-1510">Future use, always false</span></span> |
| <span data-ttu-id="a4578-1511">buildId</span><span class="sxs-lookup"><span data-stu-id="a4578-1511">buildId</span></span> |<span data-ttu-id="a4578-1512">id sestavení, který má používat pro tento požadavek doporučení</span><span class="sxs-lookup"><span data-stu-id="a4578-1512">the build id to use for this recommendation request</span></span> |
| <span data-ttu-id="a4578-1513">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1513">apiVersion</span></span> |<span data-ttu-id="a4578-1514">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1514">1.0</span></span> |

<span data-ttu-id="a4578-1515">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1515">**Response:**</span></span>

<span data-ttu-id="a4578-1516">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1516">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1517">Odpověď obsahuje jeden záznam za sadu Doporučené položky (sady položek, které jsou obvykle koupili společně s položka počáteční hodnoty nebo vstupních).</span><span class="sxs-lookup"><span data-stu-id="a4578-1517">The response includes one entry per recommended item set (a set of items which are usually bought together with the seed/input item).</span></span> <span data-ttu-id="a4578-1518">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1518">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1519">`Feed\entry\content\properties\Id1`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1519">`Feed\entry\content\properties\Id1` - Recommended item ID.</span></span>
* <span data-ttu-id="a4578-1520">`Feed\entry\content\properties\Name1`-Název položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1520">`Feed\entry\content\properties\Name1` - Name of the item.</span></span>
* <span data-ttu-id="a4578-1521">`Feed\entry\content\properties\Id2`ID položky doporučené 2. (volitelné).</span><span class="sxs-lookup"><span data-stu-id="a4578-1521">`Feed\entry\content\properties\Id2` - 2nd recommended item ID (optional).</span></span>
* <span data-ttu-id="a4578-1522">`Feed\entry\content\properties\Name2`-Název 2. položka (volitelné).</span><span class="sxs-lookup"><span data-stu-id="a4578-1522">`Feed\entry\content\properties\Name2` - Name of the 2nd item (optional).</span></span>
* <span data-ttu-id="a4578-1523">`Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-1523">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="a4578-1524">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1524">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="a4578-1525">Podívejte se na příklad odpovědi v 12.3</span><span class="sxs-lookup"><span data-stu-id="a4578-1525">See a response example in 12.3</span></span>

### <a name="125-get-user-recommendations-for-active-build"></a><span data-ttu-id="a4578-1526">12.5.</span><span class="sxs-lookup"><span data-stu-id="a4578-1526">12.5.</span></span> <span data-ttu-id="a4578-1527">Získejte doporučení uživatele (pro aktivní sestavení)</span><span class="sxs-lookup"><span data-stu-id="a4578-1527">Get User Recommendations (for active build)</span></span>
<span data-ttu-id="a4578-1528">Získejte doporučení uživatele sestavení typu "Doporučení" označená jako aktivní sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1528">Get user recommendations of a build of type "Recommendation" marked as active build.</span></span>

<span data-ttu-id="a4578-1529">Rozhraní API, vrátí se seznam předpokládaných položky podle historie využití uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-1529">The API will return a list of predicted item according to the usage history of the user.</span></span>

<span data-ttu-id="a4578-1530">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="a4578-1530">Notes:</span></span> 

1. <span data-ttu-id="a4578-1531">Neexistuje žádný uživatel doporučení pro FBT sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1531">There is no user recommendation for FBT build.</span></span>
2. <span data-ttu-id="a4578-1532">Pokud je aktivní sestavení FBT bude tato metoda vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1532">If the active build is FBT this method will returns an error.</span></span>

| <span data-ttu-id="a4578-1533">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1533">HTTP Method</span></span> | <span data-ttu-id="a4578-1534">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1534">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1535">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1535">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1536">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1536">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1537">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1537">Parameter Name</span></span> | <span data-ttu-id="a4578-1538">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1538">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1539">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1539">modelId</span></span> |<span data-ttu-id="a4578-1540">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1540">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1541">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1541">userId</span></span> |<span data-ttu-id="a4578-1542">Jedinečný identifikátor uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1542">Unique identifier of the user</span></span> |
| <span data-ttu-id="a4578-1543">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="a4578-1543">numberOfResults</span></span> |<span data-ttu-id="a4578-1544">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="a4578-1544">Number of required results</span></span> |
| <span data-ttu-id="a4578-1545">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="a4578-1545">includeMetatadata</span></span> |<span data-ttu-id="a4578-1546">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="a4578-1546">Future use, always false</span></span> |
| <span data-ttu-id="a4578-1547">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1547">apiVersion</span></span> |<span data-ttu-id="a4578-1548">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1548">1.0</span></span> |

<span data-ttu-id="a4578-1549">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1549">**Response:**</span></span>

<span data-ttu-id="a4578-1550">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1550">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1551">Odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1551">The response includes one entry per recommended item.</span></span> <span data-ttu-id="a4578-1552">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1552">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1553">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1553">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="a4578-1554">`Feed\entry\content\properties\Name`-Název položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1554">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="a4578-1555">`Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-1555">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="a4578-1556">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1556">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="a4578-1557">Podívejte se na příklad odpovědi v 12.1</span><span class="sxs-lookup"><span data-stu-id="a4578-1557">See a response example in 12.1</span></span>

### <a name="126-get-user-recommendations-with-item-list-for-active-build"></a><span data-ttu-id="a4578-1558">12.6.</span><span class="sxs-lookup"><span data-stu-id="a4578-1558">12.6.</span></span> <span data-ttu-id="a4578-1559">Získat doporučení uživatele s položky seznamu (pro aktivní sestavení)</span><span class="sxs-lookup"><span data-stu-id="a4578-1559">Get User Recommendations with item list (for active build)</span></span>
<span data-ttu-id="a4578-1560">Získat uživatele doporučení sestavení typu "Doporučení" označená jako aktivní sestavení s další seznam položek</span><span class="sxs-lookup"><span data-stu-id="a4578-1560">Get user recommendations of a build of type "Recommendation" marked as active build with an additional list of items</span></span>

<span data-ttu-id="a4578-1561">Rozhraní API vrátí seznam předpokládaných položky podle historie využití uživatele a další zadané položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1561">The API will return a list of predicted item according to the usage history of the user and the additional provided items.</span></span>

<span data-ttu-id="a4578-1562">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="a4578-1562">Notes:</span></span> 

1. <span data-ttu-id="a4578-1563">Neexistuje žádný uživatel doporučení pro FBT sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1563">There is no user recommendation for FBT build.</span></span>
2. <span data-ttu-id="a4578-1564">Pokud je aktivní sestavení FBT bude tato metoda vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1564">If the active build is FBT this method will returns an error.</span></span>

| <span data-ttu-id="a4578-1565">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1565">HTTP Method</span></span> | <span data-ttu-id="a4578-1566">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1566">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1567">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1567">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1568">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1568">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1569">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1569">Parameter Name</span></span> | <span data-ttu-id="a4578-1570">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1570">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1571">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1571">modelId</span></span> |<span data-ttu-id="a4578-1572">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1572">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1573">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1573">userId</span></span> |<span data-ttu-id="a4578-1574">Jedinečný identifikátor uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1574">Unique identifier of the user</span></span> |
| <span data-ttu-id="a4578-1575">itemsIds</span><span class="sxs-lookup"><span data-stu-id="a4578-1575">itemsIds</span></span> |<span data-ttu-id="a4578-1576">Textový soubor s oddělovači seznam položek, doporučujeme pro.</span><span class="sxs-lookup"><span data-stu-id="a4578-1576">Comma-separated list of the items to recommend for.</span></span> <span data-ttu-id="a4578-1577">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="a4578-1577">Max length: 1024</span></span> |
| <span data-ttu-id="a4578-1578">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="a4578-1578">numberOfResults</span></span> |<span data-ttu-id="a4578-1579">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="a4578-1579">Number of required results</span></span> |
| <span data-ttu-id="a4578-1580">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="a4578-1580">includeMetatadata</span></span> |<span data-ttu-id="a4578-1581">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="a4578-1581">Future use, always false</span></span> |
| <span data-ttu-id="a4578-1582">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1582">apiVersion</span></span> |<span data-ttu-id="a4578-1583">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1583">1.0</span></span> |

<span data-ttu-id="a4578-1584">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1584">**Response:**</span></span>

<span data-ttu-id="a4578-1585">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1585">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1586">Odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1586">The response includes one entry per recommended item.</span></span> <span data-ttu-id="a4578-1587">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1587">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1588">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1588">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="a4578-1589">`Feed\entry\content\properties\Name`-Název položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1589">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="a4578-1590">`Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-1590">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="a4578-1591">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1591">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="a4578-1592">Podívejte se na příklad odpovědi v 12.1</span><span class="sxs-lookup"><span data-stu-id="a4578-1592">See a response example in 12.1</span></span>

### <a name="127-get-user-recommendations--of-a-specific-build"></a><span data-ttu-id="a4578-1593">12.7.</span><span class="sxs-lookup"><span data-stu-id="a4578-1593">12.7.</span></span> <span data-ttu-id="a4578-1594">Získejte doporučení uživatele (z konkrétní sestavení)</span><span class="sxs-lookup"><span data-stu-id="a4578-1594">Get User Recommendations  (of a specific build)</span></span>
<span data-ttu-id="a4578-1595">Získejte doporučení uživatele konkrétní sestavení typu "Doporučení".</span><span class="sxs-lookup"><span data-stu-id="a4578-1595">Get user recommendations of a specific build of type "Recommendation".</span></span>

<span data-ttu-id="a4578-1596">Rozhraní API, vrátí se seznam předpokládaných položky podle historie využití uživatele (používá se v určitém sestavení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1596">The API will return a list of predicted item according to the usage history of the user (used in the specific build).</span></span>

<span data-ttu-id="a4578-1597">Poznámka: Neexistuje žádný uživatel doporučení pro FBT sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1597">Note: There is no user recommendation for FBT build.</span></span>

| <span data-ttu-id="a4578-1598">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1598">HTTP Method</span></span> | <span data-ttu-id="a4578-1599">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1599">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1600">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1600">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1601">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1601">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1602">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1602">Parameter Name</span></span> | <span data-ttu-id="a4578-1603">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1603">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1604">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1604">modelId</span></span> |<span data-ttu-id="a4578-1605">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1605">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1606">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1606">userId</span></span> |<span data-ttu-id="a4578-1607">Jedinečný identifikátor uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1607">Unique identifier of the user</span></span> |
| <span data-ttu-id="a4578-1608">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="a4578-1608">numberOfResults</span></span> |<span data-ttu-id="a4578-1609">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="a4578-1609">Number of required results</span></span> |
| <span data-ttu-id="a4578-1610">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="a4578-1610">includeMetatadata</span></span> |<span data-ttu-id="a4578-1611">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="a4578-1611">Future use, always false</span></span> |
| <span data-ttu-id="a4578-1612">buildId</span><span class="sxs-lookup"><span data-stu-id="a4578-1612">buildId</span></span> |<span data-ttu-id="a4578-1613">id sestavení, který má používat pro tento požadavek doporučení</span><span class="sxs-lookup"><span data-stu-id="a4578-1613">the build id to use for this recommendation request</span></span> |
| <span data-ttu-id="a4578-1614">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1614">apiVersion</span></span> |<span data-ttu-id="a4578-1615">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1615">1.0</span></span> |

<span data-ttu-id="a4578-1616">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1616">**Response:**</span></span>

<span data-ttu-id="a4578-1617">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1617">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1618">Odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1618">The response includes one entry per recommended item.</span></span> <span data-ttu-id="a4578-1619">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1619">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1620">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1620">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="a4578-1621">`Feed\entry\content\properties\Name`-Název položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1621">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="a4578-1622">`Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-1622">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="a4578-1623">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1623">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="a4578-1624">Podívejte se na příklad odpovědi v 12.1</span><span class="sxs-lookup"><span data-stu-id="a4578-1624">See a response example in 12.1</span></span>

### <a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a><span data-ttu-id="a4578-1625">12.8.</span><span class="sxs-lookup"><span data-stu-id="a4578-1625">12.8.</span></span> <span data-ttu-id="a4578-1626">Získat doporučení uživatele s položky seznamu (konkrétní sestavení)</span><span class="sxs-lookup"><span data-stu-id="a4578-1626">Get User Recommendations with item list (of a specific build)</span></span>
<span data-ttu-id="a4578-1627">Získejte doporučení uživatele konkrétní sestavení typu "Doporučení" a seznam dalších položek.</span><span class="sxs-lookup"><span data-stu-id="a4578-1627">Get user recommendations of a specific build of type "Recommendation" and the list of additional items.</span></span>

<span data-ttu-id="a4578-1628">Rozhraní API vrátí seznam předpokládaných položky podle historie využití uživatele a další seznam položek.</span><span class="sxs-lookup"><span data-stu-id="a4578-1628">The API will return a list of predicted item according to the usage history of the user and the additional list of items.</span></span>

<span data-ttu-id="a4578-1629">Poznámka: Tthere je žádné uživatele doporučení pro FBT sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1629">Note: Tthere is no user recommendation for FBT build.</span></span>

| <span data-ttu-id="a4578-1630">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1630">HTTP Method</span></span> | <span data-ttu-id="a4578-1631">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1631">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1632">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1632">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1633">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1633">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1634">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1634">Parameter Name</span></span> | <span data-ttu-id="a4578-1635">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1635">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1636">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1636">modelId</span></span> |<span data-ttu-id="a4578-1637">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1637">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1638">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1638">userId</span></span> |<span data-ttu-id="a4578-1639">Jedinečný identifikátor uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1639">Unique identifier of the user</span></span> |
| <span data-ttu-id="a4578-1640">položky ItemID</span><span class="sxs-lookup"><span data-stu-id="a4578-1640">itemIds</span></span> |<span data-ttu-id="a4578-1641">Textový soubor s oddělovači seznam položek, doporučujeme pro.</span><span class="sxs-lookup"><span data-stu-id="a4578-1641">Comma-separated list of the items to recommend for.</span></span> <span data-ttu-id="a4578-1642">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="a4578-1642">Max length: 1024</span></span> |
| <span data-ttu-id="a4578-1643">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="a4578-1643">numberOfResults</span></span> |<span data-ttu-id="a4578-1644">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="a4578-1644">Number of required results</span></span> |
| <span data-ttu-id="a4578-1645">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="a4578-1645">includeMetatadata</span></span> |<span data-ttu-id="a4578-1646">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="a4578-1646">Future use, always false</span></span> |
| <span data-ttu-id="a4578-1647">buildId</span><span class="sxs-lookup"><span data-stu-id="a4578-1647">buildId</span></span> |<span data-ttu-id="a4578-1648">id sestavení, který má používat pro tento požadavek doporučení</span><span class="sxs-lookup"><span data-stu-id="a4578-1648">the build id to use for this recommendation request</span></span> |
| <span data-ttu-id="a4578-1649">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1649">apiVersion</span></span> |<span data-ttu-id="a4578-1650">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1650">1.0</span></span> |

<span data-ttu-id="a4578-1651">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1651">**Response:**</span></span>

<span data-ttu-id="a4578-1652">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1652">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1653">Odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1653">The response includes one entry per recommended item.</span></span> <span data-ttu-id="a4578-1654">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1654">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1655">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1655">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="a4578-1656">`Feed\entry\content\properties\Name`-Název položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1656">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="a4578-1657">`Feed\entry\content\properties\Rating`-Hodnocení doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="a4578-1657">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="a4578-1658">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="a4578-1658">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="a4578-1659">Podívejte se na příklad odpovědi v 12.1</span><span class="sxs-lookup"><span data-stu-id="a4578-1659">See a response example in 12.1</span></span>

## <a name="13-user-usage-history"></a><span data-ttu-id="a4578-1660">13. Historie využití uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1660">13. User Usage History</span></span>
<span data-ttu-id="a4578-1661">Jakmile doporučení model byl vytvořený systému umožní načíst historii uživatele (položky přidružené k určitým uživatelem) používá pro sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1661">Once a recommendation model was built the system will allow to retrieve the user history (items associated to a specific user) used for the build.</span></span>
<span data-ttu-id="a4578-1662">Toto rozhraní API umožňují načíst historii uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1662">This API allow to retrieve the user history</span></span>

<span data-ttu-id="a4578-1663">Poznámka: historie uživatelů je aktuálně k dispozici pouze pro sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1663">Note: the user history is currently available only for recommendation builds.</span></span>

### <a name="131-retrieve-user-history"></a><span data-ttu-id="a4578-1664">13.1 načíst historii uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1664">13.1 Retrieve user history</span></span>
<span data-ttu-id="a4578-1665">Načtení seznamu položky, které jsou používány v active sestavení nebo v zadané sestavení pro dané id daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-1665">Retrieve the list of item used in the active build or in the specified build for the given user id.</span></span>

| <span data-ttu-id="a4578-1666">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1666">HTTP Method</span></span> | <span data-ttu-id="a4578-1667">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1667">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1668">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1668">GET</span></span> |<span data-ttu-id="a4578-1669">Načtení historie uživatele active sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1669">Get the user history for the active build.</span></span><br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/><span data-ttu-id="a4578-1670">Načtení historie uživatele pro danou sestavení`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`</span><span class="sxs-lookup"><span data-stu-id="a4578-1670">Get the user history for the given build `<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`</span></span><br/><br/><span data-ttu-id="a4578-1671">Příklad:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`</span><span class="sxs-lookup"><span data-stu-id="a4578-1671">Example:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`</span></span> |

| <span data-ttu-id="a4578-1672">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1672">Parameter Name</span></span> | <span data-ttu-id="a4578-1673">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1673">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1674">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1674">modelId</span></span> |<span data-ttu-id="a4578-1675">Jedinečný identifikátor modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1675">the unique identifier of the model.</span></span> |
| <span data-ttu-id="a4578-1676">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="a4578-1676">userId</span></span> |<span data-ttu-id="a4578-1677">Jedinečný identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4578-1677">the unique identifier of the user.</span></span> |
| <span data-ttu-id="a4578-1678">buildId</span><span class="sxs-lookup"><span data-stu-id="a4578-1678">buildId</span></span> |<span data-ttu-id="a4578-1679">Volitelný parametr, povolit k označení, ze které sestavení historii uživatele by měla být načítání</span><span class="sxs-lookup"><span data-stu-id="a4578-1679">optional parameter, allow to indicate from which build the user history should be fetch</span></span> |
| <span data-ttu-id="a4578-1680">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1680">apiVersion</span></span> |<span data-ttu-id="a4578-1681">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1681">1.0</span></span> |

<span data-ttu-id="a4578-1682">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1682">**Response:**</span></span>

<span data-ttu-id="a4578-1683">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1683">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1684">Odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1684">The response includes one entry per recommended item.</span></span> <span data-ttu-id="a4578-1685">Každá položka má následující data:</span><span class="sxs-lookup"><span data-stu-id="a4578-1685">Each entry has the following data:</span></span>

* <span data-ttu-id="a4578-1686">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1686">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="a4578-1687">`Feed\entry\content\properties\Name`-Název položky.</span><span class="sxs-lookup"><span data-stu-id="a4578-1687">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="a4578-1688">`Feed\entry\content\properties\Rating`-NENÍ K DISPOZICI.</span><span class="sxs-lookup"><span data-stu-id="a4578-1688">`Feed\entry\content\properties\Rating` - N/A.</span></span>
* <span data-ttu-id="a4578-1689">`Feed\entry\content\properties\Reasoning`-NENÍ K DISPOZICI.</span><span class="sxs-lookup"><span data-stu-id="a4578-1689">`Feed\entry\content\properties\Reasoning` - N/A.</span></span>

<span data-ttu-id="a4578-1690">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-1690">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get User History</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-05-26T15:32:47Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">CatalogItemsThatContainATokenEntity</title>
        <updated>2015-05-26T15:32:47Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Rating m:type="Edm.Double">0</d:Rating>
                <d:Reasoning m:type="Edm.String"/>
                <d:Metadata m:type="Edm.String"/>
                <d:FormattedRating m:type="Edm.Double" m:null="true"/>
            </m:properties>
        </content>
    </entry>
</feed>

## <a name="14-notifications"></a><span data-ttu-id="a4578-1691">14. Oznámení</span><span class="sxs-lookup"><span data-stu-id="a4578-1691">14. Notifications</span></span>
<span data-ttu-id="a4578-1692">Azure Machine Learning doporučení vytvoří oznámení, když dojde k trvalé chyby v systému.</span><span class="sxs-lookup"><span data-stu-id="a4578-1692">Azure Machine Learning Recommendations creates notifications when persistent errors happen in the system.</span></span> <span data-ttu-id="a4578-1693">Existují 3 typy oznámení:</span><span class="sxs-lookup"><span data-stu-id="a4578-1693">There are 3 types of notifications:</span></span>

1. <span data-ttu-id="a4578-1694">Sestavení selhání – toto oznámení se aktivuje pro každou chybu sestavení.</span><span class="sxs-lookup"><span data-stu-id="a4578-1694">Build failure - This notification is triggered for every build failure.</span></span>
2. <span data-ttu-id="a4578-1695">Získávání dat zpracování selhání – toto oznámení se aktivuje, když máme více než 100 chyby za posledních 5 minut při zpracování událostí využití za modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1695">Data acquisition processing failure - This notification is triggered when we have more than 100 errors in the last 5 minutes in the processing of usage events per model.</span></span>
3. <span data-ttu-id="a4578-1696">Doporučení spotřeba selhání – toto oznámení se aktivuje, když máme více než 100 chyby za posledních 5 minut při zpracování požadavků doporučení za modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1696">Recommendation consumption failure - This notification is triggered when we have more than 100 errors in the last 5 minutes in the processing of recommendation requests per model.</span></span>

### <a name="141-get-notifications"></a><span data-ttu-id="a4578-1697">14.1.</span><span class="sxs-lookup"><span data-stu-id="a4578-1697">14.1.</span></span> <span data-ttu-id="a4578-1698">Dostávat oznámení</span><span class="sxs-lookup"><span data-stu-id="a4578-1698">Get Notifications</span></span>
<span data-ttu-id="a4578-1699">Načte všechna oznámení pro všechny modely nebo pro jeden model.</span><span class="sxs-lookup"><span data-stu-id="a4578-1699">Retrieves all the notifications for all models or for a single model.</span></span>

| <span data-ttu-id="a4578-1700">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1700">HTTP Method</span></span> | <span data-ttu-id="a4578-1701">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1701">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1702">GET</span><span class="sxs-lookup"><span data-stu-id="a4578-1702">GET</span></span> |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1703">Získávání všechna oznámení pro všechny modely:</span><span class="sxs-lookup"><span data-stu-id="a4578-1703">Getting all notifications for all models:</span></span><br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1704">Příklad pro získávání oznámení pro konkrétní model.</span><span class="sxs-lookup"><span data-stu-id="a4578-1704">Example for getting notifications for a specific model:</span></span><br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277` |

| <span data-ttu-id="a4578-1705">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1705">Parameter Name</span></span> | <span data-ttu-id="a4578-1706">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1706">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1707">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1707">modelId</span></span> |<span data-ttu-id="a4578-1708">Volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="a4578-1708">Optional parameter.</span></span> <span data-ttu-id="a4578-1709">Když tento parametr vynechán, zobrazí se všechna oznámení pro všechny modely.</span><span class="sxs-lookup"><span data-stu-id="a4578-1709">When omitted, you will get all notifications for all models.</span></span> <br><span data-ttu-id="a4578-1710">Platná hodnota: Jedinečný identifikátor modelu.</span><span class="sxs-lookup"><span data-stu-id="a4578-1710">Valid value: unique identifier of the model.</span></span> |
| <span data-ttu-id="a4578-1711">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1711">apiVersion</span></span> |<span data-ttu-id="a4578-1712">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1712">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-1713">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-1713">Request Body</span></span> |<span data-ttu-id="a4578-1714">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-1714">NONE</span></span> |

<span data-ttu-id="a4578-1715">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="a4578-1715">**Response:**</span></span>

<span data-ttu-id="a4578-1716">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1716">HTTP Status code: 200</span></span>

<span data-ttu-id="a4578-1717">OData XML</span><span class="sxs-lookup"><span data-stu-id="a4578-1717">OData XML</span></span>

    The response includes one entry per notification. Each entry has the following data:
        * feed\entry\content\properties\UserName - Internal user name identification.
        * feed\entry\content\properties\ModelId - Model ID.
        * feed\entry\content\properties\Message - Notification message.
        * feed\entry\content\properties\DateCreated - Date that this notification was created in UTC format.
        * feed\entry\content\properties\NotificationType - Notification types. Values are BuildFailure, RecommendationFailure, and DataAquisitionFailure.

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of Notifications for a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-04T13:24:23Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'" />
        <entry>
                <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfNotificationsForAUserEntity</title>
            <updated>2014-11-04T13:24:23Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">515506bc-3693-4dce-a5e2-81cb3e8efb56@dm.com</d:UserName>
                    <d:ModelId m:type="Edm.String">967136e8-f868-4258-9331-10d567f87fae</d:ModelId>
                    <d:Message m:type="Edm.String">Build failed for user</d:Message>
                    <d:DateCreated m:type="Edm.String">2014-11-04T13:23:14.383</d:DateCreated>
                    <d:NotificationType m:type="Edm.String">BuildFailure</d:NotificationType>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="142-delete-model-notifications"></a><span data-ttu-id="a4578-1718">14.2.</span><span class="sxs-lookup"><span data-stu-id="a4578-1718">14.2.</span></span> <span data-ttu-id="a4578-1719">Oznámení o Model odstraňovat</span><span class="sxs-lookup"><span data-stu-id="a4578-1719">Delete Model Notifications</span></span>
<span data-ttu-id="a4578-1720">Odstraní všechny čtení oznámení pro model.</span><span class="sxs-lookup"><span data-stu-id="a4578-1720">Deletes all read notifications for a model.</span></span>

| <span data-ttu-id="a4578-1721">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1721">HTTP Method</span></span> | <span data-ttu-id="a4578-1722">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1722">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1723">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="a4578-1723">DELETE</span></span> |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="a4578-1724">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a4578-1724">Example:</span></span><br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1725">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1725">Parameter Name</span></span> | <span data-ttu-id="a4578-1726">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1726">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1727">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="a4578-1727">modelId</span></span> |<span data-ttu-id="a4578-1728">Jedinečný identifikátor modelu</span><span class="sxs-lookup"><span data-stu-id="a4578-1728">Unique identifier of the model</span></span> |
| <span data-ttu-id="a4578-1729">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1729">apiVersion</span></span> |<span data-ttu-id="a4578-1730">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1730">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-1731">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-1731">Request Body</span></span> |<span data-ttu-id="a4578-1732">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-1732">NONE</span></span> |

<span data-ttu-id="a4578-1733">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-1733">**Response**:</span></span>

<span data-ttu-id="a4578-1734">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1734">HTTP Status code: 200</span></span>

### <a name="143-delete-user-notifications"></a><span data-ttu-id="a4578-1735">14.3.</span><span class="sxs-lookup"><span data-stu-id="a4578-1735">14.3.</span></span> <span data-ttu-id="a4578-1736">Odstranit oznámení uživateli</span><span class="sxs-lookup"><span data-stu-id="a4578-1736">Delete User Notifications</span></span>
<span data-ttu-id="a4578-1737">Odstraní všechna oznámení pro všechny modely.</span><span class="sxs-lookup"><span data-stu-id="a4578-1737">Deletes all notifications for all models.</span></span>

| <span data-ttu-id="a4578-1738">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a4578-1738">HTTP Method</span></span> | <span data-ttu-id="a4578-1739">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="a4578-1739">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1740">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="a4578-1740">DELETE</span></span> |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27` |

| <span data-ttu-id="a4578-1741">Název parametru</span><span class="sxs-lookup"><span data-stu-id="a4578-1741">Parameter Name</span></span> | <span data-ttu-id="a4578-1742">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a4578-1742">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="a4578-1743">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a4578-1743">apiVersion</span></span> |<span data-ttu-id="a4578-1744">1.0</span><span class="sxs-lookup"><span data-stu-id="a4578-1744">1.0</span></span> |
|  | |
| <span data-ttu-id="a4578-1745">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="a4578-1745">Request Body</span></span> |<span data-ttu-id="a4578-1746">NONE</span><span class="sxs-lookup"><span data-stu-id="a4578-1746">NONE</span></span> |

<span data-ttu-id="a4578-1747">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="a4578-1747">**Response**:</span></span>

<span data-ttu-id="a4578-1748">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="a4578-1748">HTTP Status code: 200</span></span>

## <a name="15-legal"></a><span data-ttu-id="a4578-1749">15. Právní informace</span><span class="sxs-lookup"><span data-stu-id="a4578-1749">15. Legal</span></span>
<span data-ttu-id="a4578-1750">Tento dokument je poskytován "jako-je".</span><span class="sxs-lookup"><span data-stu-id="a4578-1750">This document is provided “as-is”.</span></span> <span data-ttu-id="a4578-1751">Informace a názory vyjádřené v tomto dokumentu včetně adres URL a dalších odkazů na internetové weby, mohou změnit bez předchozího upozornění.</span><span class="sxs-lookup"><span data-stu-id="a4578-1751">Information and views expressed in this document, including URL and other Internet Web site references, may change without notice.</span></span><br><br>
<span data-ttu-id="a4578-1752">Některé příklady použité v ukázkách jsou jenom ilustrativní a smyšlené.</span><span class="sxs-lookup"><span data-stu-id="a4578-1752">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="a4578-1753">Žádný skutečný vztah nebo připojení je určený nebo událostmi.</span><span class="sxs-lookup"><span data-stu-id="a4578-1753">No real association or connection is intended or should be inferred.</span></span><br><br>
<span data-ttu-id="a4578-1754">Tento dokument neposkytuje jste žádná zákonná práva duševního vlastnictví produktů společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a4578-1754">This document does not provide you with any legal rights to any intellectual property in any Microsoft product.</span></span> <span data-ttu-id="a4578-1755">Můžete kopírovat a tento dokument použít pro interní referenční účely.</span><span class="sxs-lookup"><span data-stu-id="a4578-1755">You may copy and use this document for your internal, reference purposes.</span></span><br><br>
<span data-ttu-id="a4578-1756">© 2015 Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a4578-1756">© 2015 Microsoft.</span></span> <span data-ttu-id="a4578-1757">Všechna práva vyhrazena.</span><span class="sxs-lookup"><span data-stu-id="a4578-1757">All rights reserved.</span></span>

