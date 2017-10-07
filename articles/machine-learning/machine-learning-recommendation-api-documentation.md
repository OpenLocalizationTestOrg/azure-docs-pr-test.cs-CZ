---
title: "aaaMachine dokumentaci k rozhraní API doporučení Learning | Microsoft Docs"
description: "Dokumentace Azure Machine Learning doporučení API pro modul doporučení k dispozici v hello Microsoft Azure Marketplace."
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
redirect_document_id: True
ms.openlocfilehash: d1cec228bf23870c05c8ab8df2779b0c3c65b06d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-recommendations-api-documentation"></a><span data-ttu-id="ca626-103">Dokumentace k rozhraní Azure Machine Learning Recommendations API</span><span class="sxs-lookup"><span data-stu-id="ca626-103">Azure Machine Learning Recommendations API Documentation</span></span>
> [!NOTE]
> <span data-ttu-id="ca626-104">Měli byste začít používat hello kognitivní služby API doporučení místo tuto verzi.</span><span class="sxs-lookup"><span data-stu-id="ca626-104">You should start using hello Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="ca626-105">Hello kognitivní službu doporučení budou nahrazení této služby, a všechny nové funkce hello bude vyvinutý existuje.</span><span class="sxs-lookup"><span data-stu-id="ca626-105">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="ca626-106">Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.</span><span class="sxs-lookup"><span data-stu-id="ca626-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="ca626-107">Další informace o [toohello migrace nové kognitivní služby](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="ca626-107">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="ca626-108">Tento dokument znázorňuje rozhraní API Microsoft Azure Machine Learning doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-108">This document depicts Microsoft Azure Machine Learning Recommendations APIs.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="ca626-109">1. Obecné – přehled</span><span class="sxs-lookup"><span data-stu-id="ca626-109">1. General overview</span></span>
<span data-ttu-id="ca626-110">Tento dokument je referenční dokumentace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca626-110">This document is an API reference.</span></span> <span data-ttu-id="ca626-111">Měli byste začít s hello "Azure Machine Learning doporučení – rychlý Start" dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ca626-111">You should start with hello “Azure Machine Learning Recommendation - Quick Start” document.</span></span>

<span data-ttu-id="ca626-112">Hello Azure Machine Learning doporučení API je možné rozdělit do hello následující logické skupiny:</span><span class="sxs-lookup"><span data-stu-id="ca626-112">hello Azure Machine Learning Recommendations API can be divided into hello following logical groups:</span></span>

* <span data-ttu-id="ca626-113"><ins>Omezení</ins> -omezení rozhraní API doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-113"><ins>Limitations</ins> - Recommendations API limitations.</span></span>
* <span data-ttu-id="ca626-114"><ins>Obecné informace</ins> -informace o ověřování, služba URI a správu verzí.</span><span class="sxs-lookup"><span data-stu-id="ca626-114"><ins>General Information</ins> - Information on authentication, service URI and versioning.</span></span>
* <span data-ttu-id="ca626-115"><ins>Model Basic</ins> – rozhraní API, která umožňují toodo hello základní operace v modelu (například vytvářet, aktualizovat a odstraňovat model).</span><span class="sxs-lookup"><span data-stu-id="ca626-115"><ins>Model Basic</ins> - APIs that enable you toodo hello basic operations on model (e.g. create, update and delete a model).</span></span>
* <span data-ttu-id="ca626-116"><ins>Model Upřesnit</ins> – rozhraní API, která umožňují tooget pokročilé statistiky dat na modelu hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-116"><ins>Model Advanced</ins> - APIs that enable you tooget advanced data insights on hello model.</span></span>
* <span data-ttu-id="ca626-117"><ins>Model obchodní pravidla</ins> – rozhraní API, která umožňují toomanage obchodní pravidla o výsledcích doporučení hello modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-117"><ins>Model Business Rules</ins> - APIs that enable you toomanage business rules on hello model recommendation results.</span></span>
* <span data-ttu-id="ca626-118"><ins>Katalog</ins> – rozhraní API, která umožňují toodo základní operace ve model katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-118"><ins>Catalog</ins> - APIs that enable you toodo basic operations on a model catalog.</span></span> <span data-ttu-id="ca626-119">Katalog obsahuje informace o metadatech na hello položky dat o využití hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-119">A catalog contains metadata information on hello items of hello usage data.</span></span>
* <span data-ttu-id="ca626-120"><ins>Funkce</ins> – rozhraní API umožňujících tooget insights na položku do katalogu hello a jak toouse tato doporučení lepší toobuild informace.</span><span class="sxs-lookup"><span data-stu-id="ca626-120"><ins>Feature</ins> - APIs that enable tooget insights on item into hello catalog and how toouse this information toobuild better recommendations.</span></span>
* <span data-ttu-id="ca626-121"><ins>Data o využití</ins> – rozhraní API, která umožňují toodo základní operace s daty využití modelu hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-121"><ins>Usage Data</ins> - APIs that enable you toodo basic operations on hello model usage data.</span></span> <span data-ttu-id="ca626-122">Data o využití v základním tvaru hello se skládá z řádků, které zahrnují dvojici & č. 60; userId & č. 62; & č. 60; itemId & č. 62;.</span><span class="sxs-lookup"><span data-stu-id="ca626-122">Usage data in hello basic form consists of rows that include pairs of &#60;userId&#62;,&#60;itemId&#62;.</span></span>
* <span data-ttu-id="ca626-123"><ins>Sestavení</ins> – rozhraní API, které umožňují tootrigger model sestavení a provést základní operace, které jsou související toothis sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-123"><ins>Build</ins> - APIs that enable you tootrigger a model build and do basic operations that are related toothis build.</span></span> <span data-ttu-id="ca626-124">Až budete mít data o využití hodí v situaci, můžete aktivovat sestavení modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-124">You can trigger a model build once you have valuable usage data.</span></span>
* <span data-ttu-id="ca626-125"><ins>Doporučení</ins> – rozhraní API, která umožňují tooconsume doporučení až po skončení hello sestavení modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-125"><ins>Recommendation</ins> - APIs that enable you tooconsume recommendations once hello build of a model ends.</span></span>
* <span data-ttu-id="ca626-126"><ins>Uživatelská Data</ins> – rozhraní API, která umožňují toofetch informace o data o využití hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-126"><ins>User Data</ins> - APIs that enable you toofetch information on hello user usage data.</span></span>
* <span data-ttu-id="ca626-127"><ins>Oznámení</ins> – rozhraní API, která umožňují tooreceive oznámení o problémech souvisejících s operací tooyour rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca626-127"><ins>Notifications</ins> - APIs that enable you tooreceive notifications on problems related tooyour API operations.</span></span> <span data-ttu-id="ca626-128">(Například zasíláte data o využití přes získávání dat a většinu hello událostí dochází k selhání zpracování.</span><span class="sxs-lookup"><span data-stu-id="ca626-128">(For example, you are reporting usage data via Data Acquisition and most of hello events processing are failing.</span></span> <span data-ttu-id="ca626-129">Oznámení o chybě bude vyvolána.)</span><span class="sxs-lookup"><span data-stu-id="ca626-129">An error notification will be raised.)</span></span>

## <a name="2-limitations"></a><span data-ttu-id="ca626-130">2. Omezení</span><span class="sxs-lookup"><span data-stu-id="ca626-130">2. Limitations</span></span>
* <span data-ttu-id="ca626-131">maximální počet modelů jedno předplatné Hello je 10.</span><span class="sxs-lookup"><span data-stu-id="ca626-131">hello maximum number of models per subscription is 10.</span></span>
* <span data-ttu-id="ca626-132">maximální počet sestavení za modelu Hello je 20.</span><span class="sxs-lookup"><span data-stu-id="ca626-132">hello maximum number of builds per model is 20.</span></span>
* <span data-ttu-id="ca626-133">maximální počet položek, které mohou být uloženy katalog Hello je 100 000.</span><span class="sxs-lookup"><span data-stu-id="ca626-133">hello maximum number of items that a catalog can hold is 100,000.</span></span>
* <span data-ttu-id="ca626-134">maximální počet bodů využití, které jsou zachovány Hello je ~ 5 000 000.</span><span class="sxs-lookup"><span data-stu-id="ca626-134">hello maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="ca626-135">Pokud nové se nahrál nebo nahlásí se odstraní nejstarší Hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-135">hello oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="ca626-136">maximální velikost dat, který může odeslat v BLOGU (například import data katalogu, data o využití import) Hello je 200MB.</span><span class="sxs-lookup"><span data-stu-id="ca626-136">hello maximum size of data that can be sent in POST (e.g. import catalog data, import usage data) is 200MB.</span></span>
* <span data-ttu-id="ca626-137">maximální počet položek, které můžete být požádáni o při získávání doporučení Hello je 150.</span><span class="sxs-lookup"><span data-stu-id="ca626-137">hello maximum number of items that can be asked for when getting recommendations is 150.</span></span>

## <a name="3-apis---general-information"></a><span data-ttu-id="ca626-138">3. Rozhraní API – obecné informace</span><span class="sxs-lookup"><span data-stu-id="ca626-138">3. APIs - general information</span></span>
### <a name="31-authentication"></a><span data-ttu-id="ca626-139">3.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-139">3.1.</span></span> <span data-ttu-id="ca626-140">Authentication</span><span class="sxs-lookup"><span data-stu-id="ca626-140">Authentication</span></span>
<span data-ttu-id="ca626-141">Postupujte podle pokynů Microsoft Azure Marketplace hello týkající se ověřování.</span><span class="sxs-lookup"><span data-stu-id="ca626-141">Please follow hello Microsoft Azure Marketplace guidelines regarding authentication.</span></span> <span data-ttu-id="ca626-142">Hello marketplace podporuje metody ověřování Basic nebo OAuth hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-142">hello marketplace supports either hello Basic or OAuth authentication method.</span></span>

### <a name="32-service-uri"></a><span data-ttu-id="ca626-143">3.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-143">3.2.</span></span> <span data-ttu-id="ca626-144">URI služby</span><span class="sxs-lookup"><span data-stu-id="ca626-144">Service URI</span></span>
<span data-ttu-id="ca626-145">Hello služby kořenová identifikátor URI pro hello rozhraní API služby Azure Machine Learning doporučení je [sem.](https://api.datamarket.azure.com/amla/recommendations/v3/)</span><span class="sxs-lookup"><span data-stu-id="ca626-145">hello service root URI for hello Azure Machine Learning Recommendations APIs is [here.](https://api.datamarket.azure.com/amla/recommendations/v3/)</span></span>

<span data-ttu-id="ca626-146">úplné URI služby Hello je vyjádřit pomocí elementů hello specifikace prostředí OData.</span><span class="sxs-lookup"><span data-stu-id="ca626-146">hello full service URI is expressed using elements of hello OData specification.</span></span>  

### <a name="33-api-version"></a><span data-ttu-id="ca626-147">3.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-147">3.3.</span></span> <span data-ttu-id="ca626-148">Verze rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ca626-148">API version</span></span>
<span data-ttu-id="ca626-149">Každé volání rozhraní API bude mít na konci hello parametr dotazu s názvem apiVersion, který by mělo být nastavené too1.0.</span><span class="sxs-lookup"><span data-stu-id="ca626-149">Each API call will have, at hello end, a query parameter called apiVersion that should be set too1.0.</span></span>

### <a name="34-ids-are-case-sensitive"></a><span data-ttu-id="ca626-150">3.4.</span><span class="sxs-lookup"><span data-stu-id="ca626-150">3.4.</span></span> <span data-ttu-id="ca626-151">ID jsou malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="ca626-151">IDs are case sensitive</span></span>
<span data-ttu-id="ca626-152">ID, vrácený žádné hello rozhraní API, jsou velká a malá písmena a by měl být použit jako takový, při předány jako parametry při následných voláních rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca626-152">IDs, returned by any of hello APIs, are case sensitive and should be used as such when passed as parameters in subsequent API calls.</span></span> <span data-ttu-id="ca626-153">ID modelu a ID katalogu pro instanci, jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="ca626-153">For instance, model IDs and catalog IDs are case sensitive.</span></span>

## <a name="4-recommendations-quality-and-cold-items"></a><span data-ttu-id="ca626-154">4. Doporučení kvality a Cold položky</span><span class="sxs-lookup"><span data-stu-id="ca626-154">4. Recommendations Quality and Cold Items</span></span>
### <a name="41-recommendation-quality"></a><span data-ttu-id="ca626-155">4.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-155">4.1.</span></span> <span data-ttu-id="ca626-156">Doporučení kvality</span><span class="sxs-lookup"><span data-stu-id="ca626-156">Recommendation quality</span></span>
<span data-ttu-id="ca626-157">Vytvoření modelu doporučení je obvykle dostatek tooallow hello systému tooprovide doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-157">Creating a recommendation model is usually enough tooallow hello system tooprovide recommendations.</span></span> <span data-ttu-id="ca626-158">Nicméně kvality doporučení se liší podle využití hello zpracovat a hello pokrytí hello katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-158">Nevertheless, recommendation quality varies based on hello usage processed and hello coverage of hello catalog.</span></span> <span data-ttu-id="ca626-159">Například pokud máte spoustu cold položky (položek bez významné využití), hello systému bude mít problémy poskytuje doporučení pro tyto položky nebo pomocí takové položky jako doporučenou jeden.</span><span class="sxs-lookup"><span data-stu-id="ca626-159">For example if you have a lot of cold items (items without significant usage), hello system will have difficulties providing a recommendation for such an item or using such an item as a recommended one.</span></span> <span data-ttu-id="ca626-160">V pořadí tooovercome hello cold položky problém umožňuje systém hello hello použití metadata hello položky tooenhance hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-160">In order tooovercome hello cold item problem, hello system allows hello use of metadata of hello items tooenhance hello recommendations.</span></span> <span data-ttu-id="ca626-161">Tato metadata jsou odkazované tooas funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-161">This metadata is referred tooas features.</span></span> <span data-ttu-id="ca626-162">Typické funkce jsou autor knihy nebo video z objektu actor.</span><span class="sxs-lookup"><span data-stu-id="ca626-162">Typical features are a book's author or a movie's actor.</span></span> <span data-ttu-id="ca626-163">Funkce poskytované prostřednictvím hello katalogu v podobě hello řetězců klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="ca626-163">Features are provided via hello catalog in hello form of key/value strings.</span></span> <span data-ttu-id="ca626-164">Hello úplný formát souboru katalogu hello, naleznete toohello [importovat část katalogu](#81-import-catalog-data).</span><span class="sxs-lookup"><span data-stu-id="ca626-164">For hello full format of hello catalog file, please refer toohello [import catalog section](#81-import-catalog-data).</span></span> 

### <a name="42-rank-build"></a><span data-ttu-id="ca626-165">4.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-165">4.2.</span></span> <span data-ttu-id="ca626-166">RANK sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-166">Rank build</span></span>
<span data-ttu-id="ca626-167">Funkce můžete vylepšit hello doporučení modelu, ale toodo proto vyžaduje použití hello smysluplný funkcí.</span><span class="sxs-lookup"><span data-stu-id="ca626-167">Features can enhance hello recommendation model, but toodo so requires hello use of meaningful features.</span></span> <span data-ttu-id="ca626-168">Pro tento účel, který byl zaveden nového sestavení - rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-168">For this purpose a new build was introduced - a rank build.</span></span> <span data-ttu-id="ca626-169">Toto sestavení se zařadit hello užitečnost funkcí.</span><span class="sxs-lookup"><span data-stu-id="ca626-169">This build will rank hello usefulness of features.</span></span> <span data-ttu-id="ca626-170">Důležité funkce je funkce s skóre pořadí 2 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="ca626-170">A meaningful feature is a feature with a rank score of 2 and up.</span></span>
<span data-ttu-id="ca626-171">Po porozumět tomu, které funkce hello mají význam, aktivovat build doporučení s seznamu hello (nebo dílčí seznam) smysluplný funkcí.</span><span class="sxs-lookup"><span data-stu-id="ca626-171">After understanding which of hello features are meaningful, trigger a recommendation build with hello list (or sublist) of meaningful features.</span></span> <span data-ttu-id="ca626-172">Je možné toouse, které tyto funkce pro vylepšení hello záložním položky a cold položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-172">It is possible toouse these feature for hello enhancement of both warm items and cold items.</span></span> <span data-ttu-id="ca626-173">V pořadí toouse je záložním položky hello `UseFeatureInModel` parametr sestavení by měly být nastavené.</span><span class="sxs-lookup"><span data-stu-id="ca626-173">In order toouse them for warm items, hello `UseFeatureInModel` build parameter should be set up.</span></span> <span data-ttu-id="ca626-174">V pořadí toouse funkce pro studenou položky, hello `AllowColdItemPlacement` by měl být povolen parametr sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-174">In order toouse features for cold items, hello `AllowColdItemPlacement` build parameter should be enabled.</span></span>
<span data-ttu-id="ca626-175">Poznámka: Není možné tooenable `AllowColdItemPlacement` bez povolení `UseFeatureInModel`.</span><span class="sxs-lookup"><span data-stu-id="ca626-175">Note: It is not possible tooenable `AllowColdItemPlacement` without enabling `UseFeatureInModel`.</span></span>

### <a name="43-recommendation-reasoning"></a><span data-ttu-id="ca626-176">4.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-176">4.3.</span></span> <span data-ttu-id="ca626-177">Doporučení reasoning</span><span class="sxs-lookup"><span data-stu-id="ca626-177">Recommendation reasoning</span></span>
<span data-ttu-id="ca626-178">Doporučení reasoning je další aspekt používání funkcí.</span><span class="sxs-lookup"><span data-stu-id="ca626-178">Recommendation reasoning is another aspect of feature usage.</span></span> <span data-ttu-id="ca626-179">Ve skutečnosti hello modul Azure Machine Learning doporučení můžete použít funkce tooprovide doporučení vysvětlení (také známa jako</span><span class="sxs-lookup"><span data-stu-id="ca626-179">Indeed, hello Azure Machine Learning Recommendations engine can use features tooprovide recommendation explanations (a.k.a.</span></span> <span data-ttu-id="ca626-180">odůvodnění), mezer na začátku toomore spolehlivosti v hello doporučuje položky z hello doporučení příjemce.</span><span class="sxs-lookup"><span data-stu-id="ca626-180">reasoning), leading toomore confidence in hello recommended item from hello recommendation consumer.</span></span>
<span data-ttu-id="ca626-181">tooenable odůvodnění, hello `AllowFeatureCorrelation` a `ReasoningFeatureList` musí být parametry instalace předchozí toorequesting sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-181">tooenable reasoning, hello `AllowFeatureCorrelation` and `ReasoningFeatureList` parameters should be setup prior toorequesting a recommendation build.</span></span>

## <a name="5-model-basic"></a><span data-ttu-id="ca626-182">5. Model Basic</span><span class="sxs-lookup"><span data-stu-id="ca626-182">5. Model Basic</span></span>
### <a name="51-create-model"></a><span data-ttu-id="ca626-183">5.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-183">5.1.</span></span> <span data-ttu-id="ca626-184">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="ca626-184">Create Model</span></span>
<span data-ttu-id="ca626-185">Vytvoří žádost o "Vytvoření modelu".</span><span class="sxs-lookup"><span data-stu-id="ca626-185">Creates a “create model” request.</span></span>

| <span data-ttu-id="ca626-186">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-186">HTTP Method</span></span> | <span data-ttu-id="ca626-187">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-187">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-188">POST</span><span class="sxs-lookup"><span data-stu-id="ca626-188">POST</span></span> |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-189">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-189">Example:</span></span><br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-190">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-190">Parameter Name</span></span> | <span data-ttu-id="ca626-191">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-191">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-192">%{ModelName/</span><span class="sxs-lookup"><span data-stu-id="ca626-192">modelName</span></span> |<span data-ttu-id="ca626-193">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="ca626-193">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="ca626-194">Maximální délka: 20</span><span class="sxs-lookup"><span data-stu-id="ca626-194">Max length: 20</span></span> |
| <span data-ttu-id="ca626-195">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-195">apiVersion</span></span> |<span data-ttu-id="ca626-196">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-196">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-197">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-197">Request Body</span></span> |<span data-ttu-id="ca626-198">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-198">NONE</span></span> |

<span data-ttu-id="ca626-199">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-199">**Response**:</span></span>

<span data-ttu-id="ca626-200">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-200">HTTP Status code: 200</span></span>

* <span data-ttu-id="ca626-201">`feed/entry/content/properties/id`-Obsahuje ID hello modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-201">`feed/entry/content/properties/id` - Contains hello model ID.</span></span>
  <span data-ttu-id="ca626-202">**Poznámka:**: ID modelu je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="ca626-202">**Note**: model ID is case sensitive.</span></span>

<span data-ttu-id="ca626-203">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-203">OData XML</span></span>

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

### <a name="52-get-model"></a><span data-ttu-id="ca626-204">5.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-204">5.2.</span></span> <span data-ttu-id="ca626-205">Získat modelu</span><span class="sxs-lookup"><span data-stu-id="ca626-205">Get Model</span></span>
<span data-ttu-id="ca626-206">Vytvoří žádost o "get model".</span><span class="sxs-lookup"><span data-stu-id="ca626-206">Creates a “get model” request.</span></span>

| <span data-ttu-id="ca626-207">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-207">HTTP Method</span></span> | <span data-ttu-id="ca626-208">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-208">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-209">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-209">GET</span></span> |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="ca626-210">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-210">Example:</span></span><br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-211">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-211">Parameter Name</span></span> | <span data-ttu-id="ca626-212">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-212">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-213">id</span><span class="sxs-lookup"><span data-stu-id="ca626-213">id</span></span> |<span data-ttu-id="ca626-214">Jedinečný identifikátor modelu hello (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="ca626-214">Unique identifier of hello model (case sensitive)</span></span> |
| <span data-ttu-id="ca626-215">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-215">apiVersion</span></span> |<span data-ttu-id="ca626-216">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-216">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-217">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-217">Request Body</span></span> |<span data-ttu-id="ca626-218">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-218">NONE</span></span> |

<span data-ttu-id="ca626-219">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-219">**Response**:</span></span>

<span data-ttu-id="ca626-220">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-220">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-221">data modelu Hello najdete v části hello následující prvky:</span><span class="sxs-lookup"><span data-stu-id="ca626-221">hello model data can be found under hello following elements:</span></span>

* <span data-ttu-id="ca626-222">`feed/entry/content/properties/Id`-Jedinečné ID modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-222">`feed/entry/content/properties/Id` - Model unique ID.</span></span>
* <span data-ttu-id="ca626-223">`feed/entry/content/properties/Name`-Název modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-223">`feed/entry/content/properties/Name` - Model name.</span></span>
* <span data-ttu-id="ca626-224">`feed/entry/content/properties/Date`-Datum vytvoření model.</span><span class="sxs-lookup"><span data-stu-id="ca626-224">`feed/entry/content/properties/Date` - Model creation date.</span></span>
* <span data-ttu-id="ca626-225">`feed/entry/content/properties/Status`-Modelu stav.</span><span class="sxs-lookup"><span data-stu-id="ca626-225">`feed/entry/content/properties/Status` - Model status.</span></span> <span data-ttu-id="ca626-226">Jedna z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="ca626-226">One of hello following:</span></span>
  * <span data-ttu-id="ca626-227">Vytvořit - Model je vytvořený a neobsahuje katalogu a využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-227">Created - Model is created and does not contain Catalog and Usage.</span></span>
  * <span data-ttu-id="ca626-228">ReadyForBuild - Model se vytvoří a obsahuje katalogu a využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-228">ReadyForBuild - Model is created and contains Catalog and Usage.</span></span>
* <span data-ttu-id="ca626-229">`feed/entry/content/properties/HasActiveBuild`-Určuje, pokud byl úspěšně sestaven hello modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-229">`feed/entry/content/properties/HasActiveBuild` - Indicates if hello model was built successfully.</span></span>
* <span data-ttu-id="ca626-230">`feed/entry/content/properties/BuildId`-ID modelu active sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-230">`feed/entry/content/properties/BuildId` - Model active build ID.</span></span>
* <span data-ttu-id="ca626-231">`feed/entry/content/properties/Mpr`-Model střední percentilu hodnocení (MPR - Další informace naleznete v tématu ModelInsight).</span><span class="sxs-lookup"><span data-stu-id="ca626-231">`feed/entry/content/properties/Mpr` - Model mean percentile ranking (MPR - see ModelInsight for more information).</span></span>
* <span data-ttu-id="ca626-232">`feed/entry/content/properties/UserName`-Model interní uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="ca626-232">`feed/entry/content/properties/UserName` - Model internal user name.</span></span>

<span data-ttu-id="ca626-233">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-233">OData XML</span></span>

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

### <a name="53----get-all-models"></a><span data-ttu-id="ca626-234">5.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-234">5.3.</span></span>    <span data-ttu-id="ca626-235">Získat všechny modely</span><span class="sxs-lookup"><span data-stu-id="ca626-235">Get All Models</span></span>
<span data-ttu-id="ca626-236">Načte všechny modely hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-236">Retrieves all models of hello current user.</span></span>

| <span data-ttu-id="ca626-237">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-237">HTTP Method</span></span> | <span data-ttu-id="ca626-238">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-238">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-239">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-239">GET</span></span> |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br><span data-ttu-id="ca626-240">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-240">Example:</span></span><br>`<rootURI>/GetAllModels?apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-241">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-241">Parameter Name</span></span> | <span data-ttu-id="ca626-242">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-242">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-243">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-243">apiVersion</span></span> |<span data-ttu-id="ca626-244">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-244">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-245">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-245">Request Body</span></span> |<span data-ttu-id="ca626-246">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-246">NONE</span></span> |

<span data-ttu-id="ca626-247">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-247">**Response**:</span></span>

<span data-ttu-id="ca626-248">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-248">HTTP Status code: 200</span></span>

* <span data-ttu-id="ca626-249">`feed/entry/content/properties/Id`-Jedinečné ID modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-249">`feed/entry/content/properties/Id` - Model unique ID.</span></span>
* <span data-ttu-id="ca626-250">`feed/entry/content/properties/Name`-Název modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-250">`feed/entry/content/properties/Name` - Model name.</span></span>
* <span data-ttu-id="ca626-251">`feed/entry/content/properties/Date`-Datum vytvoření model.</span><span class="sxs-lookup"><span data-stu-id="ca626-251">`feed/entry/content/properties/Date` - Model creation date.</span></span>
* <span data-ttu-id="ca626-252">`feed/entry/content/properties/Status`-Modelu stav.</span><span class="sxs-lookup"><span data-stu-id="ca626-252">`feed/entry/content/properties/Status` - Model status.</span></span> <span data-ttu-id="ca626-253">Jedna z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="ca626-253">One of hello following:</span></span>
  * <span data-ttu-id="ca626-254">Vytvořit - Model je vytvořený a neobsahuje katalogu a využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-254">Created - Model is created and does not contain Catalog and Usage.</span></span>
  * <span data-ttu-id="ca626-255">ReadyForBuild - Model se vytvoří a obsahuje katalogu a využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-255">ReadyForBuild - Model is created and contains Catalog and Usage.</span></span>
* <span data-ttu-id="ca626-256">`feed/entry/content/properties/HasActiveBuild`-Určuje, pokud byl úspěšně sestaven hello modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-256">`feed/entry/content/properties/HasActiveBuild` - Indicates if hello model was built successfully.</span></span>
* <span data-ttu-id="ca626-257">`feed/entry/content/properties/BuildId`-ID modelu active sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-257">`feed/entry/content/properties/BuildId` - Model active build ID.</span></span>
* <span data-ttu-id="ca626-258">`feed/entry/content/properties/Mpr`-Model MPR (Další informace naleznete v tématu ModelInsight).</span><span class="sxs-lookup"><span data-stu-id="ca626-258">`feed/entry/content/properties/Mpr` - Model MPR (see ModelInsight for more information).</span></span>
* <span data-ttu-id="ca626-259">`feed/entry/content/properties/UserName`-Model interní uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="ca626-259">`feed/entry/content/properties/UserName` - Model internal user name.</span></span>
* <span data-ttu-id="ca626-260">`feed/entry/content/properties/UsageFileNames`-Seznam souborů využívání modelu oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="ca626-260">`feed/entry/content/properties/UsageFileNames` - List of model usage files separated by comma.</span></span>
* <span data-ttu-id="ca626-261">`feed/entry/content/properties/CatalogId`-ID modelu katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-261">`feed/entry/content/properties/CatalogId` - Model catalog ID.</span></span>
* <span data-ttu-id="ca626-262">`feed/entry/content/properties/Description`-Modelu popis.</span><span class="sxs-lookup"><span data-stu-id="ca626-262">`feed/entry/content/properties/Description` - Model description.</span></span>
* <span data-ttu-id="ca626-263">`feed/entry/content/properties/CatalogFileName`-Název souboru katalogu model.</span><span class="sxs-lookup"><span data-stu-id="ca626-263">`feed/entry/content/properties/CatalogFileName` - Model catalog file name.</span></span>

<span data-ttu-id="ca626-264">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-264">OData XML</span></span>

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

### <a name="54----update-model"></a><span data-ttu-id="ca626-265">5.4.</span><span class="sxs-lookup"><span data-stu-id="ca626-265">5.4.</span></span>    <span data-ttu-id="ca626-266">Aktualizace modelu</span><span class="sxs-lookup"><span data-stu-id="ca626-266">Update Model</span></span>
<span data-ttu-id="ca626-267">Můžete aktualizovat popis modelu hello nebo hello ID aktivního sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-267">You can update hello model description or hello active build ID.</span></span><br><span data-ttu-id="ca626-268">
<ins>ID aktivního sestavení</ins> -každé sestavení pro každý model má sestavení ID.</span><span class="sxs-lookup"><span data-stu-id="ca626-268">
<ins>Active build ID</ins> - Every build for every model has a build ID.</span></span> <span data-ttu-id="ca626-269">ID aktivního sestavení Hello je hello prvním úspěšném sestavení každý nový model.</span><span class="sxs-lookup"><span data-stu-id="ca626-269">hello active build ID is hello first successful build of every new model.</span></span> <span data-ttu-id="ca626-270">Jakmile máte ID aktivního sestavení a provádět další sestavení pro hello stejného modelu, je nutné tooexplicitly nastavit jako hello výchozí sestavení ID Pokud budete chtít.</span><span class="sxs-lookup"><span data-stu-id="ca626-270">Once you have an active build ID and you do additional builds for hello same model, you need tooexplicitly set it as hello default build ID if you want to.</span></span> <span data-ttu-id="ca626-271">Když spotřebujete doporučení, pokud nezadáte ID hello sestavení, který chcete toouse, výchozí hello, jeden budou automaticky použita.</span><span class="sxs-lookup"><span data-stu-id="ca626-271">When you consume recommendations, if you do not specify hello build ID that you want toouse, hello default one will be used automatically.</span></span><br>
<span data-ttu-id="ca626-272">Tento mechanismus umožňuje - po model doporučení v produkčním prostředí - toobuild nové modely a otestovat je před zvýšením úrovně je tooproduction.</span><span class="sxs-lookup"><span data-stu-id="ca626-272">This mechanism enables you - once you have a recommendation model in production - toobuild new models and test them before you promote them tooproduction.</span></span>

| <span data-ttu-id="ca626-273">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-273">HTTP Method</span></span> | <span data-ttu-id="ca626-274">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-274">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-275">PUT</span><span class="sxs-lookup"><span data-stu-id="ca626-275">PUT</span></span> |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><span data-ttu-id="ca626-276">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-276">Example:</span></span><br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-277">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-277">Parameter Name</span></span> | <span data-ttu-id="ca626-278">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-278">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-279">id</span><span class="sxs-lookup"><span data-stu-id="ca626-279">id</span></span> |<span data-ttu-id="ca626-280">Jedinečný identifikátor modelu hello (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="ca626-280">Unique identifier of hello model (case sensitive)</span></span> |
| <span data-ttu-id="ca626-281">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-281">apiVersion</span></span> |<span data-ttu-id="ca626-282">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-282">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-283">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-283">Request Body</span></span> |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br><span data-ttu-id="ca626-284">Všimněte si, že hello XML značky popis a ActiveBuildId jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="ca626-284">Note that hello XML tags Description and ActiveBuildId are optional.</span></span> <span data-ttu-id="ca626-285">Pokud nechcete, aby tooset popis nebo ActiveBuildId, odeberte celý značky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-285">If you do not want tooset Description or ActiveBuildId, remove hello entire tag.</span></span> |

<span data-ttu-id="ca626-286">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-286">**Response**:</span></span>

<span data-ttu-id="ca626-287">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-287">HTTP Status code: 200</span></span>

### <a name="55----delete-model"></a><span data-ttu-id="ca626-288">5.5.</span><span class="sxs-lookup"><span data-stu-id="ca626-288">5.5.</span></span>    <span data-ttu-id="ca626-289">Odstranit Model</span><span class="sxs-lookup"><span data-stu-id="ca626-289">Delete Model</span></span>
<span data-ttu-id="ca626-290">Odstraní existující model podle ID.</span><span class="sxs-lookup"><span data-stu-id="ca626-290">Deletes an existing model by ID.</span></span>

| <span data-ttu-id="ca626-291">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-291">HTTP Method</span></span> | <span data-ttu-id="ca626-292">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-292">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-293">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="ca626-293">DELETE</span></span> |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="ca626-294">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-294">Example:</span></span><br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-295">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-295">Parameter Name</span></span> | <span data-ttu-id="ca626-296">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-296">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-297">id</span><span class="sxs-lookup"><span data-stu-id="ca626-297">id</span></span> |<span data-ttu-id="ca626-298">Jedinečný identifikátor modelu hello (malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="ca626-298">Unique identifier of hello model (case sensitive)</span></span> |
| <span data-ttu-id="ca626-299">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-299">apiVersion</span></span> |<span data-ttu-id="ca626-300">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-300">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-301">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-301">Request Body</span></span> |<span data-ttu-id="ca626-302">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-302">NONE</span></span> |

<span data-ttu-id="ca626-303">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-303">**Response**:</span></span>

<span data-ttu-id="ca626-304">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-304">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-305">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-305">OData XML</span></span>

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

## <a name="6-model-advanced"></a><span data-ttu-id="ca626-306">6. Rozšířené modelu</span><span class="sxs-lookup"><span data-stu-id="ca626-306">6. Model Advanced</span></span>
### <a name="61----model-data-insight"></a><span data-ttu-id="ca626-307">6.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-307">6.1.</span></span>    <span data-ttu-id="ca626-308">Přehled modelu dat</span><span class="sxs-lookup"><span data-stu-id="ca626-308">Model Data Insight</span></span>
<span data-ttu-id="ca626-309">Vrátí data o využití hello tento model byl sestaven s statistické údaje.</span><span class="sxs-lookup"><span data-stu-id="ca626-309">Returns statistical data on hello usage data that this model was built with.</span></span>

<span data-ttu-id="ca626-310">K dispozici pouze pro sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-310">Available only for Recommendation build.</span></span>

| <span data-ttu-id="ca626-311">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-311">HTTP Method</span></span> | <span data-ttu-id="ca626-312">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-312">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-313">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-313">GET</span></span> |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="ca626-314">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-314">Example:</span></span><br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-315">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-315">Parameter Name</span></span> | <span data-ttu-id="ca626-316">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-316">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-317">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-317">modelId</span></span> |<span data-ttu-id="ca626-318">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-318">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-319">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-319">apiVersion</span></span> |<span data-ttu-id="ca626-320">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-320">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-321">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-321">Request Body</span></span> |<span data-ttu-id="ca626-322">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-322">NONE</span></span> |

<span data-ttu-id="ca626-323">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-323">**Response**:</span></span>

<span data-ttu-id="ca626-324">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-324">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-325">Hello data jsou vrácena jako kolekci vlastností.</span><span class="sxs-lookup"><span data-stu-id="ca626-325">hello data is returned as a collection of properties.</span></span>

* <span data-ttu-id="ca626-326">`feed/entry/id/content/properties/key`-Obsahuje název vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-326">`feed/entry/id/content/properties/key` - Holds hello property name.</span></span>
* <span data-ttu-id="ca626-327">`feed/entry/id/content/properties/value`-Obsahuje hodnotu vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-327">`feed/entry/id/content/properties/value` - Holds hello property value.</span></span>

<span data-ttu-id="ca626-328">Následující tabulka Hello znázorňuje hello hodnotu, která představuje každý klíč.</span><span class="sxs-lookup"><span data-stu-id="ca626-328">hello table below depicts hello value that each key represents.</span></span>

| <span data-ttu-id="ca626-329">Klíč</span><span class="sxs-lookup"><span data-stu-id="ca626-329">Key</span></span> | <span data-ttu-id="ca626-330">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-330">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-331">AvgItemLength</span><span class="sxs-lookup"><span data-stu-id="ca626-331">AvgItemLength</span></span> |<span data-ttu-id="ca626-332">Průměrný počet jedinečných uživatelů na každou položku.</span><span class="sxs-lookup"><span data-stu-id="ca626-332">Average number of distinct users per item.</span></span> |
| <span data-ttu-id="ca626-333">AvgUserLength</span><span class="sxs-lookup"><span data-stu-id="ca626-333">AvgUserLength</span></span> |<span data-ttu-id="ca626-334">Průměrný počet jedinečných položek na uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-334">Average number of distinct items per user.</span></span> |
| <span data-ttu-id="ca626-335">DensificationNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="ca626-335">DensificationNumberOfItems</span></span> |<span data-ttu-id="ca626-336">Počet položek po vyřazení položky, které nelze modelována.</span><span class="sxs-lookup"><span data-stu-id="ca626-336">Number of items after pruning items that cannot be modelled.</span></span> |
| <span data-ttu-id="ca626-337">DensificationNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="ca626-337">DensificationNumberOfUsers</span></span> |<span data-ttu-id="ca626-338">Počet bodů využití po vyřazení uživatelů a položky, které nelze modelována.</span><span class="sxs-lookup"><span data-stu-id="ca626-338">Number of usage points after pruning users and items that can't be modelled.</span></span> |
| <span data-ttu-id="ca626-339">DensificationNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="ca626-339">DensificationNumberOfRecords</span></span> |<span data-ttu-id="ca626-340">Počet bodů využití po vyřazení uživatelů a položky, které nelze modelována.</span><span class="sxs-lookup"><span data-stu-id="ca626-340">Number of usage points after pruning users and items that can't be modelled.</span></span> |
| <span data-ttu-id="ca626-341">MaxItemLength</span><span class="sxs-lookup"><span data-stu-id="ca626-341">MaxItemLength</span></span> |<span data-ttu-id="ca626-342">Počet jedinečných uživatelů pro nejoblíbenější položku hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-342">Number of distinct users for hello most popular item.</span></span> |
| <span data-ttu-id="ca626-343">MaxUserLength</span><span class="sxs-lookup"><span data-stu-id="ca626-343">MaxUserLength</span></span> |<span data-ttu-id="ca626-344">Maximální počet jedinečných položek pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-344">Maximal number of distinct items for a user.</span></span> |
| <span data-ttu-id="ca626-345">MinItemLength</span><span class="sxs-lookup"><span data-stu-id="ca626-345">MinItemLength</span></span> |<span data-ttu-id="ca626-346">Maximální počet jedinečných uživatelů pro položku.</span><span class="sxs-lookup"><span data-stu-id="ca626-346">Maximal number of distinct users for an item.</span></span> |
| <span data-ttu-id="ca626-347">MinUserLength</span><span class="sxs-lookup"><span data-stu-id="ca626-347">MinUserLength</span></span> |<span data-ttu-id="ca626-348">Minimální počet jedinečných položek pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-348">Minimal number of distinct items for a user.</span></span> |
| <span data-ttu-id="ca626-349">RawNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="ca626-349">RawNumberOfItems</span></span> |<span data-ttu-id="ca626-350">Počet položek v souborech využití hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-350">Number of items in hello usage files.</span></span> |
| <span data-ttu-id="ca626-351">RawNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="ca626-351">RawNumberOfUsers</span></span> |<span data-ttu-id="ca626-352">Počet bodů využití před všechny vyřazení.</span><span class="sxs-lookup"><span data-stu-id="ca626-352">Number of usage points before any pruning.</span></span> |
| <span data-ttu-id="ca626-353">RawNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="ca626-353">RawNumberOfRecords</span></span> |<span data-ttu-id="ca626-354">Počet bodů využití před všechny vyřazení.</span><span class="sxs-lookup"><span data-stu-id="ca626-354">Number of usage points before any pruning.</span></span> |
| <span data-ttu-id="ca626-355">SamplingNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="ca626-355">SamplingNumberOfItems</span></span> |<span data-ttu-id="ca626-356">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ca626-356">N/A</span></span> |
| <span data-ttu-id="ca626-357">SamplingNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="ca626-357">SamplingNumberOfRecords</span></span> |<span data-ttu-id="ca626-358">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ca626-358">N/A</span></span> |
| <span data-ttu-id="ca626-359">SamplingNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="ca626-359">SamplingNumberOfUsers</span></span> |<span data-ttu-id="ca626-360">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ca626-360">N/A</span></span> |

<span data-ttu-id="ca626-361">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-361">OData XML</span></span>

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

### <a name="62----model-insight"></a><span data-ttu-id="ca626-362">6.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-362">6.2.</span></span>    <span data-ttu-id="ca626-363">Přehled modelu</span><span class="sxs-lookup"><span data-stu-id="ca626-363">Model Insight</span></span>
<span data-ttu-id="ca626-364">Vrátí model náhled na hello active sestavení nebo (je-li zadána) na konkrétní sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-364">Returns model insight on hello active build or (if given) on a specific build.</span></span>

<span data-ttu-id="ca626-365">K dispozici pouze pro sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-365">Available only for Recommendation build.</span></span>

| <span data-ttu-id="ca626-366">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-366">HTTP Method</span></span> | <span data-ttu-id="ca626-367">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-367">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-368">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-368">GET</span></span> |<span data-ttu-id="ca626-369">S ID aktivního sestavení:</span><span class="sxs-lookup"><span data-stu-id="ca626-369">With active build ID:</span></span><br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-370">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-370">Example:</span></span><br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-371">S konkrétní sestavovací ID:</span><span class="sxs-lookup"><span data-stu-id="ca626-371">With specific build ID:</span></span><br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-372">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-372">Parameter Name</span></span> | <span data-ttu-id="ca626-373">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-373">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-374">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-374">modelId</span></span> |<span data-ttu-id="ca626-375">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-375">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-376">buildId</span><span class="sxs-lookup"><span data-stu-id="ca626-376">buildId</span></span> |<span data-ttu-id="ca626-377">Volitelné – číslo, které identifikuje úspěšném sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-377">Optional - number that identifies a successful build.</span></span> |
| <span data-ttu-id="ca626-378">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-378">apiVersion</span></span> |<span data-ttu-id="ca626-379">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-379">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-380">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-380">Request Body</span></span> |<span data-ttu-id="ca626-381">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-381">NONE</span></span> |

<span data-ttu-id="ca626-382">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-382">**Response**:</span></span>

<span data-ttu-id="ca626-383">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-383">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-384">Hello data jsou vrácena jako kolekci vlastností.</span><span class="sxs-lookup"><span data-stu-id="ca626-384">hello data is returned as a collection of properties.</span></span>

* `feed/entry/id/content/properties/key`
* `feed/entry/id/content/properties/value`

<span data-ttu-id="ca626-385">Následující tabulka Hello znázorňuje hello hodnotu, která představuje každý klíč.</span><span class="sxs-lookup"><span data-stu-id="ca626-385">hello table below depicts hello value that each key represents.</span></span>

| <span data-ttu-id="ca626-386">Klíč</span><span class="sxs-lookup"><span data-stu-id="ca626-386">Key</span></span> | <span data-ttu-id="ca626-387">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-387">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-388">CatalogCoverage</span><span class="sxs-lookup"><span data-stu-id="ca626-388">CatalogCoverage</span></span> |<span data-ttu-id="ca626-389">Jaká část hello katalogu můžete modelována s vzorce používání.</span><span class="sxs-lookup"><span data-stu-id="ca626-389">What part of hello catalog can be modelled with usage patterns.</span></span> <span data-ttu-id="ca626-390">Hello zbytek hello položky potřebovat funkce založené na obsah.</span><span class="sxs-lookup"><span data-stu-id="ca626-390">hello rest of hello items will need content-based features.</span></span> |
| <span data-ttu-id="ca626-391">Pravidlem zásad správy</span><span class="sxs-lookup"><span data-stu-id="ca626-391">Mpr</span></span> |<span data-ttu-id="ca626-392">Střední percentilu hodnocení hello modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-392">Mean percentile ranking of hello model.</span></span> <span data-ttu-id="ca626-393">Nižší je lepší.</span><span class="sxs-lookup"><span data-stu-id="ca626-393">Lower is better.</span></span> |
| <span data-ttu-id="ca626-394">NumberOfDimensions</span><span class="sxs-lookup"><span data-stu-id="ca626-394">NumberOfDimensions</span></span> |<span data-ttu-id="ca626-395">Počet dimenzí používá algoritmus factorization matice hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-395">Number of dimensions used by hello matrix factorization algorithm.</span></span> |

<span data-ttu-id="ca626-396">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-396">OData XML</span></span>

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

### <a name="63----get-model-sample"></a><span data-ttu-id="ca626-397">6.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-397">6.3.</span></span>    <span data-ttu-id="ca626-398">Získat ukázky modelu</span><span class="sxs-lookup"><span data-stu-id="ca626-398">Get Model Sample</span></span>
<span data-ttu-id="ca626-399">Získá ukázku hello doporučení modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-399">Gets a sample of hello recommendation model.</span></span>

| <span data-ttu-id="ca626-400">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-400">HTTP Method</span></span> | <span data-ttu-id="ca626-401">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-401">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-402">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-402">GET</span></span> |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="ca626-403">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-403">Example:</span></span><br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-404">S konkrétní sestavovací ID:</span><span class="sxs-lookup"><span data-stu-id="ca626-404">With specific build ID:</span></span><br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="ca626-405">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-405">Example:</span></span><br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-406">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-406">Parameter Name</span></span> | <span data-ttu-id="ca626-407">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-407">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-408">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-408">modelId</span></span> |<span data-ttu-id="ca626-409">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-409">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-410">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-410">apiVersion</span></span> |<span data-ttu-id="ca626-411">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-411">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-412">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-412">Request Body</span></span> |<span data-ttu-id="ca626-413">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-413">NONE</span></span> |

<span data-ttu-id="ca626-414">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-414">**Response**:</span></span>

<span data-ttu-id="ca626-415">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-415">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-416">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-416">OData XML</span></span>

<span data-ttu-id="ca626-417">Odpověď se vrátí ve formátu raw textu:</span><span class="sxs-lookup"><span data-stu-id="ca626-417">Response is returned in raw text format:</span></span>

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
50471eec-9aeb-4900-84d7-21567ab18546, If hello Buddha Dated: A Handbook for Finding Love on a Spiritual Path
    cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, Divine Secrets of hello Ya-Ya Sisterhood: A Novel Rating: 0.5266
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5252
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5244
    e2cbf7ad-0636-4117-8b30-298da6df7077, Animal Dreams Rating: 0.5227
    6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Confessions of an Ugly Stepsister: A Novel Rating: 0.5222
5e97148f-defb-4d74-af2d-80f4763bf531, hello Deep End of hello Ocean (Oprah's Book Club)
    5e97148f-defb-4d74-af2d-80f4763bf531, hello Deep End of hello Ocean (Oprah's Book Club) Rating: 0.537
    5dcbac37-2946-4f2a-a0b3-bbe710f9409a, Up Island: A Novel Rating: 0.5277
    bc5b69db-733b-4346-adde-3927544258f7, Downtown Rating: 0.5275
    31fe5c63-3e5a-48d0-802b-d3b0f989a634, Have a Nice Day: A Tale of Blood and Sweatsocks Rating: 0.5252
    0adf981a-b65b-4c11-b36b-78aca2f948a2, hello Perfect Storm: A True Story of Men Against hello Sea Rating: 0.5238
68f97068-ae1a-4163-9e94-396b800b743d, Modoc: hello True Story of hello Greatest Elephant That Ever Lived
    68f97068-ae1a-4163-9e94-396b800b743d, Modoc: hello True Story of hello Greatest Elephant That Ever Lived Rating: 0.5379
    6724862e-e4e7-4022-9614-1468d8b902ff, Little House on hello Prairie Rating: 0.5345
    cdedb837-1620-496d-94c4-6ccfed888320, Little House in hello Big Woods Rating: 0.5325
    382164ba-406b-4187-b726-d7a54b9d790d, hello Tao of Pooh Rating: 0.5309
    6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5, On hello Banks of Plum Creek Rating: 0.5285
37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships
    37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars, Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships Rating: 0.5397
    f2be16d4-5faf-4d32-ab83-7ba74d29261e, Politically Correct Bedtime Stories: Modern Tales for Our Life and Times Rating: 0.5207
    ef732c5c-334b-4d6b-ab82-7255eb7286d0, Honor Among Thieves Rating: 0.5195
    0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, hello Giving Tree Rating: 0.5194
    883b360f-8b42-407f-b977-2f44ad840877, Scary Stories tooTell in hello Dark: Collected from American Folklore (Scary Stories) Rating: 0.5184
ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: hello Craft of Baseball
    d008dae9-c73a-40a1-9a9b-96d5cf546f36, hello Gulag Archipelago 1918-1956: An Experiment in Literary Investigation I-II Rating: 0.5416
    ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: hello Craft of Baseball Rating: 0.5403
    49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland Rating: 0.5394
    cc7964fd-d30f-478e-a425-93ddbdf094ed, Magic hello Gathering: Arena Vol. 1 Rating: 0.5379
    8a1e9f36-97af-4614-bed9-24e3940a05f3, More Sniglets: Any Word That Doesn't Appear in hello Dictionary but Should Rating: 0.5377
12a6d988-be21-4a09-8143-9d5f4261ba16, A Dream of Eagles
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5417
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.5416
    1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, hello Family Rating: 0.5371
    56daeffe-7d48-43cd-8ef8-7dffd0c103d3, Kilo Class Rating: 0.5366
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5366
df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish
    56d33036-dfda-46b9-8e2a-76cb03921bb0, hello X-Files: Ground Zero Rating: 0.5417
    0780cde8-6529-4e1d-b6c6-082c1b80e596, Twelve Red Herrings Rating: 0.5416
    df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish Rating: 0.5408
    400fe331-2c35-490c-adbc-b28b4b73d56c, Shall We Tell hello President? Rating: 0.5383
    f86ad7d0-5c03-42b3-aebf-13d44aec8b30, Shades of Grace Rating: 0.5358
de1f62a4-89e6-44d2-aaee-992a4bf093f1, hello Map That Changed hello World: William Smith and hello Birth of Modern Geology
    de1f62a4-89e6-44d2-aaee-992a4bf093f1, hello Map That Changed hello World: William Smith and hello Birth of Modern Geology Rating: 0.5422
    b303538f-e2c6-4a2c-b425-8d21e684fc3e, My Uncle Oswald Rating: 0.5385
    34b84627-48af-4a4c-96c4-b26fb3863f56, Midnight In hello Garden of Good and Evil Rating: 0.5379
    306cbaa7-b1a8-4142-9d55-e11b5018a7a8, hello Street Lawyer Rating: 0.5376
    e53b4baa-8c09-45c4-95c0-b6a26b98770b, Miss Smillas Feeling for Snow Rating: 0.5367

Level 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, hello Sinister Pig (Hillerman Tony)
    352aaea1-6b12-454d-a3d5-46379d9e4eb2, hello Sinister Pig (Hillerman Tony) Rating: 0.5425
    74c49398-bc10-4af5-a658-a996a1201254, Children of hello Storm (Peters Elizabeth) Rating: 0.5387
    9ba80080-196e-43fd-8025-391d963f77e7, hello Floating Girl Rating: 0.5372
    e68f81d5-7745-4cc7-b943-fedb8fcc2ced, Killer Smile (Scottoline Lisa) Rating: 0.5353
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5332
c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days
    0adf981a-b65b-4c11-b36b-78aca2f948a2, hello Perfect Storm: A True Story of Men Against hello Sea Rating: 0.5433
    c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days Rating: 0.543
    a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: Any Word That Doesn't Appear in hello Dictionary But Should) Rating: 0.5327
    6f6e192e-0d64-49ca-9b63-f09413ea1ee6, Politically Correct Holiday Stories: For an Enlightened Yuletide Season Rating: 0.5307
    798051a8-147d-4d46-b0dc-e836325029e6, AGE OF INNOCENCE (MOVIE TIE-IN) Rating: 0.5301
73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics)
    cba8163f-6536-436b-8130-47b4a43c827f, Trust No One (hello Official Guide toohello X-Files Vol. 2) Rating: 0.5434
    5708e4cb-2492-49c0-94a8-cc413eec5d89, Small Gods (Discworld Novels (Paperback)) Rating: 0.5406
    73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics) Rating: 0.5403
    d885b0bd-ae4b-452d-bdf2-faa90197dbc9, hello Color of Magic Rating: 0.539
    b133a9c4-4784-4db3-b100-d0d6dffb94d2, hello Truth Is Out There (hello Official Guide toohello X-Files Vol. 1) Rating: 0.5367
271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why hello Winged Whale Sings
    271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why hello Winged Whale Sings Rating: 0.5445
    2de1c354-90ff-47c5-a0db-1bad7d88ef94, hello Salaryman's Wife (Children of Violence Series) Rating: 0.5329
    d279416e-19c0-43f8-9ec9-a585947879ca, Zen Attitude Rating: 0.5316
    c8f854d7-3de3-4b23-8217-f4f851670fd4, Revenge of hello Cootie Girls: A Robin Hudson Mystery (Robin Hudson Mysteries (Paperback)) Rating: 0.5305
    8ef4751c-7074-409e-a3ac-d49b222fc864, Where hello Wild Things Are Rating: 0.5289
9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God
    9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God Rating: 0.5446
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, hello Bean Trees Rating: 0.5389
    65ecbdd1-131c-40c3-a3d6-d86ca281377a, hello God of Small Things Rating: 0.5387
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, hello Stone Diaries Rating: 0.5355
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5344
5f17d90a-2604-4fe8-8977-1a280b9098b1, One for hello Money (Stephanie Plum Novels (Paperback))
    5f17d90a-2604-4fe8-8977-1a280b9098b1, One for hello Money (Stephanie Plum Novels (Paperback)) Rating: 0.5446
    57169b2b-9a8a-486b-9aac-1ed98ce57168, Final Appeal Rating: 0.5332
    efcb1bc4-7278-4a8f-b491-befde02070d6, Moment of Truth Rating: 0.5329
    1efa91a2-993b-4c43-9f5c-3454fc12612d, Burn Factor Rating: 0.5309
    24c59962-458a-4ec8-b95d-d694e861919c, At Home in Mitford (hello Mitford Years) Rating: 0.5303
4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: hello Boy Who Was Raised As a Girl
    4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: hello Boy Who Was Raised As a Girl Rating: 0.5449
    cd5f2c03-20cb-43be-a1fb-3b4233e63222, Pigs in Heaven Rating: 0.5329
    19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, Love in hello Time of Cholera (Penguin Great Books of hello 20th Century) Rating: 0.5267
    15689d09-c711-4844-84d8-130a90237b26, Bel Canto Rating: 0.5245
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5235
98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories
    f874b5a3-5d40-4436-94ff-0fa1c090ddf5, hello Sun Also Rises (A Scribner classic) Rating: 0.5451
    98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories Rating: 0.5442
    0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Unseen Rating: 0.5421
    15316ca6-1e38-425f-893d-691944a47000, More Scary Stories tooTell In hello Dark Rating: 0.5409
    329d5682-3dc3-4206-8aa2-eef4b1032258, Letters from hello Earth Rating: 0.54
5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover))
    5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover)) Rating: 0.5462
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5372
    604eb3bd-6026-4f51-bffd-9fb54f180400, Family Pictures: A Novel Rating: 0.5341
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5334
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, hello Bean Trees Rating: 0.5319
d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven
    d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven Rating: 0.5491
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5401
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, hello Stone Diaries Rating: 0.5393
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5382
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5367

</pre>


## <a name="7-model-business-rules"></a><span data-ttu-id="ca626-418">7. Model obchodní pravidla</span><span class="sxs-lookup"><span data-stu-id="ca626-418">7. Model Business Rules</span></span>
<span data-ttu-id="ca626-419">Toto jsou typy hello pravidel podporované:</span><span class="sxs-lookup"><span data-stu-id="ca626-419">These are hello types of rules supported:</span></span>

* <span data-ttu-id="ca626-420"><strong>BlockList</strong> -BlockList vám umožní tooprovide seznam položek, které nechcete tooreturn ve výsledcích doporučení hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-420"><strong>BlockList</strong> - BlockList enables you tooprovide a list of items that you do not want tooreturn in hello recommendation results.</span></span> 
* <span data-ttu-id="ca626-421"><strong>FeatureBlockList</strong> -BlockList funkce umožňuje vám položek tooblock na základě hodnot hello její funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-421"><strong>FeatureBlockList</strong> - Feature BlockList enables you tooblock items based on hello values of its features.</span></span>

<span data-ttu-id="ca626-422">*Neodesílat více než 1 000 položek v jednom blocklist pravidlo nebo volání může časový limit. Pokud potřebujete tooblock více než 1 000 položek, můžete provést několik blocklist volání.*</span><span class="sxs-lookup"><span data-stu-id="ca626-422">*Do not send more than 1000 items in a single blocklist rule or your call may timeout. If you need tooblock more than 1000 items, you can make several blocklist calls.*</span></span>

* <span data-ttu-id="ca626-423"><strong>Upsale</strong> -Upsale vám umožní tooenforce tooreturn položek ve výsledcích doporučení hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-423"><strong>Upsale</strong> - Upsale enables you tooenforce items tooreturn in hello recommendation results.</span></span>
* <span data-ttu-id="ca626-424"><strong>Seznam povolených adres</strong> -umožňuje seznamu povolených tooonly můžete navrhnout doporučení ze seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="ca626-424"><strong>WhiteList</strong> - White List enables you tooonly suggest recommendations from a list of items.</span></span>
* <span data-ttu-id="ca626-425"><strong>FeatureWhiteList</strong> – seznam povolených funkcí vám umožní tooonly doporučujeme položky, které mají hodnoty příslušné funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-425"><strong>FeatureWhiteList</strong> - Feature White List enables you tooonly recommend items that have specific feature values.</span></span>
* <span data-ttu-id="ca626-426"><strong>PerSeedBlockList</strong> – na počáteční hodnoty blokovaných umožňuje tooprovide za položku seznam položek, které nemůže být vrácen jako doporučení výsledky.</span><span class="sxs-lookup"><span data-stu-id="ca626-426"><strong>PerSeedBlockList</strong> - Per Seed Block List enables you tooprovide per item a list of items that cannot be returned as recommendation results.</span></span>

### <a name="71----get-model-rules"></a><span data-ttu-id="ca626-427">7.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-427">7.1.</span></span>    <span data-ttu-id="ca626-428">Získat pravidla modelu</span><span class="sxs-lookup"><span data-stu-id="ca626-428">Get Model Rules</span></span>
| <span data-ttu-id="ca626-429">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-429">HTTP Method</span></span> | <span data-ttu-id="ca626-430">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-430">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-431">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-431">GET</span></span> |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="ca626-432">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-432">Example:</span></span><br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-433">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-433">Parameter Name</span></span> | <span data-ttu-id="ca626-434">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-434">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-435">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-435">modelId</span></span> |<span data-ttu-id="ca626-436">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-436">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-437">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-437">apiVersion</span></span> |<span data-ttu-id="ca626-438">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-438">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-439">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-439">Request Body</span></span> |<span data-ttu-id="ca626-440">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-440">NONE</span></span> |

<span data-ttu-id="ca626-441">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-441">**Response**:</span></span>

<span data-ttu-id="ca626-442">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-442">HTTP Status code: 200</span></span>

* <span data-ttu-id="ca626-443">`feed/entry/content/properties/Id`-Jedinečný identifikátor tohoto pravidla.</span><span class="sxs-lookup"><span data-stu-id="ca626-443">`feed/entry/content/properties/Id` - Unique identifier of this rule.</span></span>
* <span data-ttu-id="ca626-444">`feed/entry/content/properties/Type`-Typ pravidla hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-444">`feed/entry/content/properties/Type` - Type of hello rule.</span></span>
* <span data-ttu-id="ca626-445">`feed/entry/content/properties/Parameter`-Parametr pravidla.</span><span class="sxs-lookup"><span data-stu-id="ca626-445">`feed/entry/content/properties/Parameter` - Rule parameter.</span></span>

<span data-ttu-id="ca626-446">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-446">OData XML</span></span>

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

### <a name="72----add-rule"></a><span data-ttu-id="ca626-447">7.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-447">7.2.</span></span>    <span data-ttu-id="ca626-448">Přidat pravidlo</span><span class="sxs-lookup"><span data-stu-id="ca626-448">Add Rule</span></span>
| <span data-ttu-id="ca626-449">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-449">HTTP Method</span></span> | <span data-ttu-id="ca626-450">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-450">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-451">POST</span><span class="sxs-lookup"><span data-stu-id="ca626-451">POST</span></span> |`<rootURI>/AddRule?apiVersion=%271.0%27` |
| <span data-ttu-id="ca626-452">ZÁHLAVÍ</span><span class="sxs-lookup"><span data-stu-id="ca626-452">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="ca626-453">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-453">Parameter Name</span></span> | <span data-ttu-id="ca626-454">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-454">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-455">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-455">apiVersion</span></span> |<span data-ttu-id="ca626-456">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-456">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-457">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-457">Request Body</span></span> | |

<span data-ttu-id="ca626-458"><ins>Vždy, když poskytnete ID položek obchodní pravidla, ujistěte se, zda text hello toouse externí Id položky hello (hello stejným Id, který jste použili v soubor katalogu hello)</ins></span><span class="sxs-lookup"><span data-stu-id="ca626-458"><ins>Whenever providing Item Ids for business rules, make sure toouse hello External Id of hello item (hello same Id that you used in hello catalog file)</ins></span></span><br><span data-ttu-id="ca626-459">
<ins>tooadd BlockList pravidlo:</ins></span><span class="sxs-lookup"><span data-stu-id="ca626-459">
<ins>tooadd a BlockList rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><span data-ttu-id="ca626-460"><ins>
<ins>tooadd FeatureBlockList pravidlo:</ins></span><span class="sxs-lookup"><span data-stu-id="ca626-460"><ins>
<ins>tooadd a FeatureBlockList rule:</ins></span></span><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><span data-ttu-id="ca626-461"><ins>tooadd pravidlo Upsale:</ins></span><span class="sxs-lookup"><span data-stu-id="ca626-461"><ins> tooadd an Upsale rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br><span data-ttu-id="ca626-462">
<ins>tooadd pravidlo seznamu povolených IP adres:</ins></span><span class="sxs-lookup"><span data-stu-id="ca626-462">
<ins>tooadd a WhiteList rule:</ins></span></span><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><span data-ttu-id="ca626-463"><ins>
<ins>tooadd FeatureWhiteList pravidlo:</ins></span><span class="sxs-lookup"><span data-stu-id="ca626-463"><ins>
<ins>tooadd a FeatureWhiteList rule:</ins></span></span><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><span data-ttu-id="ca626-464"><ins>tooadd PerSeedBlockList pravidlo:</ins></span><span class="sxs-lookup"><span data-stu-id="ca626-464"><ins> tooadd a PerSeedBlockList rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|

<span data-ttu-id="ca626-465">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-465">**Response**:</span></span>

<span data-ttu-id="ca626-466">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-466">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-467">Hello rozhraní API vrátí hello nově vytvořeného pravidla s jeho podrobnostmi.</span><span class="sxs-lookup"><span data-stu-id="ca626-467">hello API returns hello newly created rule with its details.</span></span> <span data-ttu-id="ca626-468">Vlastnost pravidla Hello lze načíst z hello následující cesty:</span><span class="sxs-lookup"><span data-stu-id="ca626-468">hello rules property can be retrieved from hello following paths:</span></span>

* <span data-ttu-id="ca626-469">`feed/entry/content/properties/Id`-Jedinečný identifikátor tohoto pravidla.</span><span class="sxs-lookup"><span data-stu-id="ca626-469">`feed/entry/content/properties/Id` - Unique identifier of this rule.</span></span>
* <span data-ttu-id="ca626-470">`feed/entry/content/properties/Type`-Typ pravidla hello: BlockList nebo Upsale.</span><span class="sxs-lookup"><span data-stu-id="ca626-470">`feed/entry/content/properties/Type` - Type of hello rule: BlockList or Upsale.</span></span>
* <span data-ttu-id="ca626-471">`feed/entry/content/properties/Parameter`-Parametr pravidla.</span><span class="sxs-lookup"><span data-stu-id="ca626-471">`feed/entry/content/properties/Parameter` - Rule parameter.</span></span>

<span data-ttu-id="ca626-472">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-472">OData XML</span></span>

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

### <a name="73----delete-rule"></a><span data-ttu-id="ca626-473">7.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-473">7.3.</span></span>    <span data-ttu-id="ca626-474">Odstranit pravidlo</span><span class="sxs-lookup"><span data-stu-id="ca626-474">Delete Rule</span></span>
| <span data-ttu-id="ca626-475">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-475">HTTP Method</span></span> | <span data-ttu-id="ca626-476">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-476">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-477">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="ca626-477">DELETE</span></span> |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-478">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-478">Example:</span></span><br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-479">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-479">Parameter Name</span></span> | <span data-ttu-id="ca626-480">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-480">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-481">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-481">modelId</span></span> |<span data-ttu-id="ca626-482">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-482">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-483">filterId</span><span class="sxs-lookup"><span data-stu-id="ca626-483">filterId</span></span> |<span data-ttu-id="ca626-484">Jedinečný identifikátor hello filtru</span><span class="sxs-lookup"><span data-stu-id="ca626-484">Unique identifier of hello filter</span></span> |
| <span data-ttu-id="ca626-485">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-485">apiVersion</span></span> |<span data-ttu-id="ca626-486">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-486">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-487">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-487">Request Body</span></span> |<span data-ttu-id="ca626-488">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-488">NONE</span></span> |

<span data-ttu-id="ca626-489">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-489">**Response**:</span></span>

<span data-ttu-id="ca626-490">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-490">HTTP Status code: 200</span></span>

### <a name="74----delete-all-rules"></a><span data-ttu-id="ca626-491">7.4.</span><span class="sxs-lookup"><span data-stu-id="ca626-491">7.4.</span></span>    <span data-ttu-id="ca626-492">Odstranit všechna pravidla</span><span class="sxs-lookup"><span data-stu-id="ca626-492">Delete All Rules</span></span>
| <span data-ttu-id="ca626-493">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-493">HTTP Method</span></span> | <span data-ttu-id="ca626-494">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-494">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-495">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="ca626-495">DELETE</span></span> |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-496">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-496">Example:</span></span><br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-497">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-497">Parameter Name</span></span> | <span data-ttu-id="ca626-498">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-498">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-499">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-499">modelId</span></span> |<span data-ttu-id="ca626-500">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-500">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-501">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-501">apiVersion</span></span> |<span data-ttu-id="ca626-502">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-502">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-503">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-503">Request Body</span></span> |<span data-ttu-id="ca626-504">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-504">NONE</span></span> |

<span data-ttu-id="ca626-505">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-505">**Response**:</span></span>

<span data-ttu-id="ca626-506">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-506">HTTP Status code: 200</span></span>

## <a name="8-catalog"></a><span data-ttu-id="ca626-507">8. Katalogu</span><span class="sxs-lookup"><span data-stu-id="ca626-507">8. Catalog</span></span>
### <a name="81----import-catalog-data"></a><span data-ttu-id="ca626-508">8.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-508">8.1.</span></span>    <span data-ttu-id="ca626-509">Umožňuje importovat Data Catalog</span><span class="sxs-lookup"><span data-stu-id="ca626-509">Import Catalog Data</span></span>
<span data-ttu-id="ca626-510">Pokud nahrajete několik katalogu soubory toohello stejný model s několika volání, jsme vloží pouze hello nové položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-510">If you upload several catalog files toohello same model with several calls, we will insert only hello new catalog items.</span></span> <span data-ttu-id="ca626-511">Existující položky zůstane s hello původní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ca626-511">Existing items will remain with hello original values.</span></span> <span data-ttu-id="ca626-512">Data katalogu nelze aktualizovat pomocí této metody.</span><span class="sxs-lookup"><span data-stu-id="ca626-512">You cannot update catalog data by using this method.</span></span>

<span data-ttu-id="ca626-513">data katalogu Hello postupujte hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="ca626-513">hello catalog data should follow hello following format:</span></span>

* <span data-ttu-id="ca626-514">Bez funkce –`<Item Id>,<Item Name>,<Item Category>[,<Description>]`</span><span class="sxs-lookup"><span data-stu-id="ca626-514">Without features - `<Item Id>,<Item Name>,<Item Category>[,<Description>]`</span></span>
* <span data-ttu-id="ca626-515">S funkcemi-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`</span><span class="sxs-lookup"><span data-stu-id="ca626-515">With features - `<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`</span></span>

<span data-ttu-id="ca626-516">Poznámka: hello maximální velikost souboru je 200MB.</span><span class="sxs-lookup"><span data-stu-id="ca626-516">Note: hello maximum file size is 200MB.</span></span>

<span data-ttu-id="ca626-517">** Formát podrobností **</span><span class="sxs-lookup"><span data-stu-id="ca626-517">** Format details **</span></span>

| <span data-ttu-id="ca626-518">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ca626-518">Name</span></span> | <span data-ttu-id="ca626-519">Povinné</span><span class="sxs-lookup"><span data-stu-id="ca626-519">Mandatory</span></span> | <span data-ttu-id="ca626-520">Typ</span><span class="sxs-lookup"><span data-stu-id="ca626-520">Type</span></span> | <span data-ttu-id="ca626-521">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-521">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ca626-522">Id položky</span><span class="sxs-lookup"><span data-stu-id="ca626-522">Item Id</span></span> |<span data-ttu-id="ca626-523">Ano</span><span class="sxs-lookup"><span data-stu-id="ca626-523">Yes</span></span> |<span data-ttu-id="ca626-524">[A-z], [-z], [0-9] [_] &#40; Podtržítko &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="ca626-524">[A-z], [a-z], [0-9], [_] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="ca626-525">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="ca626-525">Max length: 50</span></span> |<span data-ttu-id="ca626-526">Jedinečný identifikátor položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-526">Unique identifier of an item.</span></span> |
| <span data-ttu-id="ca626-527">Název položky</span><span class="sxs-lookup"><span data-stu-id="ca626-527">Item Name</span></span> |<span data-ttu-id="ca626-528">Ano</span><span class="sxs-lookup"><span data-stu-id="ca626-528">Yes</span></span> |<span data-ttu-id="ca626-529">Žádné alfanumerické znaky</span><span class="sxs-lookup"><span data-stu-id="ca626-529">Any alphanumeric characters</span></span><br> <span data-ttu-id="ca626-530">Maximální délka: 255</span><span class="sxs-lookup"><span data-stu-id="ca626-530">Max length: 255</span></span> |<span data-ttu-id="ca626-531">Název položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-531">Item name.</span></span> |
| <span data-ttu-id="ca626-532">Kategorie položky</span><span class="sxs-lookup"><span data-stu-id="ca626-532">Item Category</span></span> |<span data-ttu-id="ca626-533">Ano</span><span class="sxs-lookup"><span data-stu-id="ca626-533">Yes</span></span> |<span data-ttu-id="ca626-534">Žádné alfanumerické znaky</span><span class="sxs-lookup"><span data-stu-id="ca626-534">Any alphanumeric characters</span></span> <br> <span data-ttu-id="ca626-535">Maximální délka: 255</span><span class="sxs-lookup"><span data-stu-id="ca626-535">Max length: 255</span></span> |<span data-ttu-id="ca626-536">Kategorie toowhich tuto položku patří (například vaření knihy, obrázkům...); nesmí být prázdné.</span><span class="sxs-lookup"><span data-stu-id="ca626-536">Category toowhich this item belongs (e.g. Cooking Books, Drama…); can be empty.</span></span> |
| <span data-ttu-id="ca626-537">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-537">Description</span></span> |<span data-ttu-id="ca626-538">Ne, pokud funkce jsou k dispozici (ale nesmí být prázdné)</span><span class="sxs-lookup"><span data-stu-id="ca626-538">No, unless features are present (but can be empty)</span></span> |<span data-ttu-id="ca626-539">Žádné alfanumerické znaky</span><span class="sxs-lookup"><span data-stu-id="ca626-539">Any alphanumeric characters</span></span> <br> <span data-ttu-id="ca626-540">Maximální délka: 4000</span><span class="sxs-lookup"><span data-stu-id="ca626-540">Max length: 4000</span></span> |<span data-ttu-id="ca626-541">Popis této položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-541">Description of this item.</span></span> |
| <span data-ttu-id="ca626-542">Seznam funkcí</span><span class="sxs-lookup"><span data-stu-id="ca626-542">Features list</span></span> |<span data-ttu-id="ca626-543">Ne</span><span class="sxs-lookup"><span data-stu-id="ca626-543">No</span></span> |<span data-ttu-id="ca626-544">Žádné alfanumerické znaky</span><span class="sxs-lookup"><span data-stu-id="ca626-544">Any alphanumeric characters</span></span> <br> <span data-ttu-id="ca626-545">Maximální délka: 4000; Maximální počet funkcí: 20</span><span class="sxs-lookup"><span data-stu-id="ca626-545">Max length: 4000; Max number of features:20</span></span> |<span data-ttu-id="ca626-546">Čárkami oddělený seznam název funkce = hodnota funkce, které můžou být použité tooenhance modelu doporučení; v tématu [Advanced témata](#2-advanced-topics) části.</span><span class="sxs-lookup"><span data-stu-id="ca626-546">Comma-separated list of feature name=feature value that can be used tooenhance model recommendation; see [Advanced topics](#2-advanced-topics) section.</span></span> |

| <span data-ttu-id="ca626-547">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-547">HTTP Method</span></span> | <span data-ttu-id="ca626-548">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-548">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-549">POST</span><span class="sxs-lookup"><span data-stu-id="ca626-549">POST</span></span> |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-550">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-550">Example:</span></span><br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |
| <span data-ttu-id="ca626-551">ZÁHLAVÍ</span><span class="sxs-lookup"><span data-stu-id="ca626-551">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="ca626-552">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-552">Parameter Name</span></span> | <span data-ttu-id="ca626-553">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-553">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-554">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-554">modelId</span></span> |<span data-ttu-id="ca626-555">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-555">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-556">Název souboru</span><span class="sxs-lookup"><span data-stu-id="ca626-556">filename</span></span> |<span data-ttu-id="ca626-557">Textové identifikátor hello katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-557">Textual identifier of hello catalog.</span></span><br><span data-ttu-id="ca626-558">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="ca626-558">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="ca626-559">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="ca626-559">Max length: 50</span></span> |
| <span data-ttu-id="ca626-560">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-560">apiVersion</span></span> |<span data-ttu-id="ca626-561">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-561">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-562">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-562">Request Body</span></span> |<span data-ttu-id="ca626-563">Příklad (s funkcí):</span><span class="sxs-lookup"><span data-stu-id="ca626-563">Example (with features):</span></span><br/><span data-ttu-id="ca626-564">2406e770-769c-4189-89de-1c9283f93a96, Clara Callan adresáře, popis hello knihy, vytvářet = Richard Wright vydavatele = Harper Flamingo Kanadě rok = 2001</span><span class="sxs-lookup"><span data-stu-id="ca626-564">2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book,hello book  description,author=Richard Wright,publisher=Harper Flamingo Canada,year=2001</span></span><br><span data-ttu-id="ca626-565">hello 21bf8088-b6c0-4509-870c-e1c7ac78304a, Forgetting místnosti: vytváření A Fiction (Byzantium kniha), kniha,, = Nick Bantock vydavatele = Harpercollins, roku = 1997</span><span class="sxs-lookup"><span data-stu-id="ca626-565">21bf8088-b6c0-4509-870c-e1c7ac78304a,hello Forgetting Room: A Fiction (Byzantium Book),Book,,author=Nick Bantock,publisher=Harpercollins,year=1997</span></span><br><span data-ttu-id="ca626-566">3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, kniha,, vytvářet = Alexander Findley vydavatele = HarperFlamingo Kanada rok = 2001</span><span class="sxs-lookup"><span data-stu-id="ca626-566">3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book,,author=Timothy Findley, publisher=HarperFlamingo Canada, year=2001</span></span><br><span data-ttu-id="ca626-567">552a1940-21e4-4399-82bb-594b46d7ed54, omezení zvířata, kniha, popis hello knihy, vytvářet = Magnus lisoven vydavatele = plošinová publikování rok = 1998</span><span class="sxs-lookup"><span data-stu-id="ca626-567">552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book,hello book description,author=Magnus Mills, publisher=Arcade Publishing, year=1998</span></span></pre> |

<span data-ttu-id="ca626-568">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-568">**Response**:</span></span>

<span data-ttu-id="ca626-569">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-569">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-570">Hello rozhraní API vrátí sestavy hello importu.</span><span class="sxs-lookup"><span data-stu-id="ca626-570">hello API returns a report of hello import.</span></span>

* <span data-ttu-id="ca626-571">`feed\entry\content\properties\LineCount`-Přijaté počet řádků.</span><span class="sxs-lookup"><span data-stu-id="ca626-571">`feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="ca626-572">`feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby tooan.</span><span class="sxs-lookup"><span data-stu-id="ca626-572">`feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due tooan error.</span></span>

<span data-ttu-id="ca626-573">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-573">OData XML</span></span>

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

### <a name="82----get-catalog"></a><span data-ttu-id="ca626-574">8.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-574">8.2.</span></span>    <span data-ttu-id="ca626-575">Získat katalogu</span><span class="sxs-lookup"><span data-stu-id="ca626-575">Get Catalog</span></span>
<span data-ttu-id="ca626-576">Načte všechny položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-576">Retrieves all catalog items.</span></span>
<span data-ttu-id="ca626-577">Hello katalog bude načtených v daný okamžik jednu stránku.</span><span class="sxs-lookup"><span data-stu-id="ca626-577">hello catalog will be retrieved one page at a time.</span></span> <span data-ttu-id="ca626-578">Pokud chcete tooget položek v konkrétním indexem, můžete použít parametr hello $skip odata.</span><span class="sxs-lookup"><span data-stu-id="ca626-578">If you want tooget items at a particular index, you can use hello $skip odata parameter.</span></span> <span data-ttu-id="ca626-579">Například pokud chcete tooget položky začínající na pozici 100, přidejte parametr hello $skip = 100 toohello požadavku.</span><span class="sxs-lookup"><span data-stu-id="ca626-579">For instance if you want tooget items starting at position 100, add hello parameter $skip=100 toohello request.</span></span>

| <span data-ttu-id="ca626-580">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-580">HTTP Method</span></span> | <span data-ttu-id="ca626-581">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-581">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-582">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-582">GET</span></span> |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-583">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-583">Example:</span></span><br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-584">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-584">Parameter Name</span></span> | <span data-ttu-id="ca626-585">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-585">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-586">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-586">modelId</span></span> |<span data-ttu-id="ca626-587">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-587">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-588">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-588">apiVersion</span></span> |<span data-ttu-id="ca626-589">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-589">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-590">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-590">Request Body</span></span> |<span data-ttu-id="ca626-591">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-591">NONE</span></span> |

<span data-ttu-id="ca626-592">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-592">**Response**:</span></span>

<span data-ttu-id="ca626-593">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-593">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-594">Hello odpověď obsahuje jeden záznam za položka katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-594">hello response includes one entry per catalog item.</span></span> <span data-ttu-id="ca626-595">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-595">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-596">`feed/entry/content/properties/ExternalId`-Externí ID položky katalogu, hello jedno poskytované hello zákazníka.</span><span class="sxs-lookup"><span data-stu-id="ca626-596">`feed/entry/content/properties/ExternalId` - Catalog item external ID, hello one provided by hello customer.</span></span>
* <span data-ttu-id="ca626-597">`feed/entry/content/properties/InternalId`-Katalogu interní ID položky hello generovaný Azure Machine Learning doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-597">`feed/entry/content/properties/InternalId` - Catalog item internal ID, hello one that Azure Machine Learning Recommendations has generated.</span></span>
* <span data-ttu-id="ca626-598">`feed/entry/content/properties/Name`-Název položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-598">`feed/entry/content/properties/Name` - Catalog item name.</span></span>
* <span data-ttu-id="ca626-599">`feed/entry/content/properties/Category`-Kategorie položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-599">`feed/entry/content/properties/Category` - Catalog item category.</span></span>
* <span data-ttu-id="ca626-600">`feed/entry/content/properties/Description`-Katalogu popis položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-600">`feed/entry/content/properties/Description` - Catalog item description.</span></span>
* <span data-ttu-id="ca626-601">`feed/entry/content/properties/Metadata`-Metadata položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-601">`feed/entry/content/properties/Metadata` - Catalog item metadata.</span></span>

<span data-ttu-id="ca626-602">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-602">OData XML</span></span>

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
                <d:Name m:type="Edm.String">hello Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="83----get-catalog-items-by-token"></a><span data-ttu-id="ca626-603">8.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-603">8.3.</span></span>    <span data-ttu-id="ca626-604">Získat položky katalogu pomocí tokenu</span><span class="sxs-lookup"><span data-stu-id="ca626-604">Get Catalog Items by Token</span></span>
| <span data-ttu-id="ca626-605">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-605">HTTP Method</span></span> | <span data-ttu-id="ca626-606">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-606">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-607">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-607">GET</span></span> |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-608">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-608">Example:</span></span><br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-609">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-609">Parameter Name</span></span> | <span data-ttu-id="ca626-610">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-610">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-611">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-611">modelId</span></span> |<span data-ttu-id="ca626-612">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-612">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-613">Token</span><span class="sxs-lookup"><span data-stu-id="ca626-613">token</span></span> |<span data-ttu-id="ca626-614">Token položka katalogu hello názvu.</span><span class="sxs-lookup"><span data-stu-id="ca626-614">Token of hello catalog item’s name.</span></span> <span data-ttu-id="ca626-615">By mělo obsahovat aspoň 3 znaky.</span><span class="sxs-lookup"><span data-stu-id="ca626-615">Should contain at least 3 characters.</span></span> |
| <span data-ttu-id="ca626-616">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-616">apiVersion</span></span> |<span data-ttu-id="ca626-617">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-617">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-618">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-618">Request Body</span></span> |<span data-ttu-id="ca626-619">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-619">NONE</span></span> |

<span data-ttu-id="ca626-620">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-620">**Response**:</span></span>

<span data-ttu-id="ca626-621">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-621">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-622">Hello odpověď obsahuje jeden záznam za položka katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-622">hello response includes one entry per catalog item.</span></span> <span data-ttu-id="ca626-623">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-623">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-624">`feed/entry/content/properties/InternalId`-Katalogu interní ID položky hello generovaný Azure Machine Learning doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-624">`feed/entry/content/properties/InternalId` - Catalog item internal ID, hello one that Azure Machine Learning Recommendations has generated.</span></span>
* <span data-ttu-id="ca626-625">`feed/entry/content/properties/Name`-Název položky katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-625">`feed/entry/content/properties/Name` - Catalog item name.</span></span>
* <span data-ttu-id="ca626-626">`feed/entry/content/properties/Rating`-(pro budoucí použití)</span><span class="sxs-lookup"><span data-stu-id="ca626-626">`feed/entry/content/properties/Rating` -  (for future use)</span></span>
* <span data-ttu-id="ca626-627">`feed/entry/content/properties/Reasoning`-(pro budoucí použití)</span><span class="sxs-lookup"><span data-stu-id="ca626-627">`feed/entry/content/properties/Reasoning` -  (for future use)</span></span>
* <span data-ttu-id="ca626-628">`feed/entry/content/properties/Metadata`-(pro budoucí použití)</span><span class="sxs-lookup"><span data-stu-id="ca626-628">`feed/entry/content/properties/Metadata` -  (for future use)</span></span>
* <span data-ttu-id="ca626-629">`feed/entry/content/properties/FormattedRating`-(pro budoucí použití)</span><span class="sxs-lookup"><span data-stu-id="ca626-629">`feed/entry/content/properties/FormattedRating` - (for future use)</span></span>

<span data-ttu-id="ca626-630">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-630">OData XML</span></span>

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

## <a name="9-usage-data"></a><span data-ttu-id="ca626-631">9. Údaje o využití</span><span class="sxs-lookup"><span data-stu-id="ca626-631">9. Usage data</span></span>
### <a name="91----import-usage-data"></a><span data-ttu-id="ca626-632">9.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-632">9.1.</span></span>    <span data-ttu-id="ca626-633">Importovat Data o využití</span><span class="sxs-lookup"><span data-stu-id="ca626-633">Import Usage Data</span></span>
#### <a name="911-uploading-file"></a><span data-ttu-id="ca626-634">9.1.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-634">9.1.1.</span></span> <span data-ttu-id="ca626-635">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="ca626-635">Uploading File</span></span>
<span data-ttu-id="ca626-636">Tato část uvádí, jak data o využití tooupload pomocí souboru.</span><span class="sxs-lookup"><span data-stu-id="ca626-636">This section shows how tooupload usage data by using a file.</span></span> <span data-ttu-id="ca626-637">Můžete volat toto rozhraní API se data o využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-637">You can call this API several times with usage data.</span></span> <span data-ttu-id="ca626-638">Všechna data o využití se uloží pro všechna volání.</span><span class="sxs-lookup"><span data-stu-id="ca626-638">All usage data will be saved for all calls.</span></span>

| <span data-ttu-id="ca626-639">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-639">HTTP Method</span></span> | <span data-ttu-id="ca626-640">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-640">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-641">POST</span><span class="sxs-lookup"><span data-stu-id="ca626-641">POST</span></span> |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-642">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-642">Example:</span></span><br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-643">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-643">Parameter Name</span></span> | <span data-ttu-id="ca626-644">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-644">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-645">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-645">modelId</span></span> |<span data-ttu-id="ca626-646">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-646">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-647">Název souboru</span><span class="sxs-lookup"><span data-stu-id="ca626-647">filename</span></span> |<span data-ttu-id="ca626-648">Textové identifikátor hello katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-648">Textual identifier of hello catalog.</span></span><br><span data-ttu-id="ca626-649">Pouze písmena (A-Z, a – z), čísla (0-9), pomlčky (-) a podtržítka (_) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="ca626-649">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="ca626-650">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="ca626-650">Max length: 50</span></span> |
| <span data-ttu-id="ca626-651">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-651">apiVersion</span></span> |<span data-ttu-id="ca626-652">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-652">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-653">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-653">Request Body</span></span> |<span data-ttu-id="ca626-654">Data o využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-654">Usage data.</span></span> <span data-ttu-id="ca626-655">Formát:</span><span class="sxs-lookup"><span data-stu-id="ca626-655">Format:</span></span><br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th><span data-ttu-id="ca626-656">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ca626-656">Name</span></span></th><th><span data-ttu-id="ca626-657">Povinné</span><span class="sxs-lookup"><span data-stu-id="ca626-657">Mandatory</span></span></th><th><span data-ttu-id="ca626-658">Typ</span><span class="sxs-lookup"><span data-stu-id="ca626-658">Type</span></span></th><th><span data-ttu-id="ca626-659">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-659">Description</span></span></th></tr><tr><td><span data-ttu-id="ca626-660">Id uživatele</span><span class="sxs-lookup"><span data-stu-id="ca626-660">User Id</span></span></td><td><span data-ttu-id="ca626-661">Ano</span><span class="sxs-lookup"><span data-stu-id="ca626-661">Yes</span></span></td><td><span data-ttu-id="ca626-662">[A-z], [-z], [0-9] [_] &#40; Podtržítko &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="ca626-662">[A-z], [a-z], [0-9], [_] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="ca626-663">Maximální délka: 255</span><span class="sxs-lookup"><span data-stu-id="ca626-663">Max length: 255</span></span> </td><td><span data-ttu-id="ca626-664">Jedinečný identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-664">Unique identifier of a user.</span></span></td></tr><tr><td><span data-ttu-id="ca626-665">Id položky</span><span class="sxs-lookup"><span data-stu-id="ca626-665">Item Id</span></span></td><td><span data-ttu-id="ca626-666">Ano</span><span class="sxs-lookup"><span data-stu-id="ca626-666">Yes</span></span></td><td><span data-ttu-id="ca626-667">[A-z], [-z], [0-9] [&#95;] &#40; Podtržítko &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="ca626-667">[A-z], [a-z], [0-9], [&#95;] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="ca626-668">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="ca626-668">Max length: 50</span></span></td><td><span data-ttu-id="ca626-669">Jedinečný identifikátor položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-669">Unique identifier of an item.</span></span></td></tr><tr><td><span data-ttu-id="ca626-670">Čas</span><span class="sxs-lookup"><span data-stu-id="ca626-670">Time</span></span></td><td><span data-ttu-id="ca626-671">Ne</span><span class="sxs-lookup"><span data-stu-id="ca626-671">No</span></span></td><td><span data-ttu-id="ca626-672">Datum ve formátu: rrrr/MM/ddTHH (například 2013/06/20T10:00:00)</span><span class="sxs-lookup"><span data-stu-id="ca626-672">Date in format: YYYY/MM/DDTHH:MM:SS (e.g. 2013/06/20T10:00:00)</span></span></td><td><span data-ttu-id="ca626-673">Čas data.</span><span class="sxs-lookup"><span data-stu-id="ca626-673">Time of data.</span></span></td></tr><tr><td><span data-ttu-id="ca626-674">Událost</span><span class="sxs-lookup"><span data-stu-id="ca626-674">Event</span></span></td><td><span data-ttu-id="ca626-675">Ne; Pokud zadaný musí taky datum</span><span class="sxs-lookup"><span data-stu-id="ca626-675">No; if supplied then must also put date</span></span></td><td><span data-ttu-id="ca626-676">Jedna z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="ca626-676">One of hello following:</span></span><br><span data-ttu-id="ca626-677">• Klikněte na tlačítko</span><span class="sxs-lookup"><span data-stu-id="ca626-677">• Click</span></span><br><span data-ttu-id="ca626-678">• RecommendationClick</span><span class="sxs-lookup"><span data-stu-id="ca626-678">• RecommendationClick</span></span><br><span data-ttu-id="ca626-679">• AddShopCart</span><span class="sxs-lookup"><span data-stu-id="ca626-679">•    AddShopCart</span></span><br><span data-ttu-id="ca626-680">• RemoveShopCart</span><span class="sxs-lookup"><span data-stu-id="ca626-680">• RemoveShopCart</span></span><br><span data-ttu-id="ca626-681">• Nákupu</span><span class="sxs-lookup"><span data-stu-id="ca626-681">• Purchase</span></span></td><td></td></tr></table><br><span data-ttu-id="ca626-682">Maximální velikost souboru: 200MB</span><span class="sxs-lookup"><span data-stu-id="ca626-682">Maximum file size: 200MB</span></span><br><br><span data-ttu-id="ca626-683">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-683">Example:</span></span><br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

<span data-ttu-id="ca626-684">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-684">**Response**:</span></span>

<span data-ttu-id="ca626-685">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-685">HTTP Status code: 200</span></span>

* <span data-ttu-id="ca626-686">`Feed\entry\content\properties\LineCount`-Přijaté počet řádků.</span><span class="sxs-lookup"><span data-stu-id="ca626-686">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="ca626-687">`Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vložit z důvodu chyby tooan.</span><span class="sxs-lookup"><span data-stu-id="ca626-687">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due tooan error.</span></span>
* <span data-ttu-id="ca626-688">`Feed\entry\content\properties\FileId`-Souboru identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ca626-688">`Feed\entry\content\properties\FileId` - File identifier.</span></span>

<span data-ttu-id="ca626-689">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-689">OData XML</span></span>

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


#### <a name="912-using-data-acquisition"></a><span data-ttu-id="ca626-690">9.1.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-690">9.1.2.</span></span> <span data-ttu-id="ca626-691">Pomocí získávání dat</span><span class="sxs-lookup"><span data-stu-id="ca626-691">Using Data Acquisition</span></span>
<span data-ttu-id="ca626-692">Tato část uvádí, jak toosend událostí v reálném čas tooAzure Machine Learning doporučení, obvykle z vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="ca626-692">This section shows how toosend events in real time tooAzure Machine Learning Recommendations, usually from your website.</span></span>

| <span data-ttu-id="ca626-693">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-693">HTTP Method</span></span> | <span data-ttu-id="ca626-694">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-694">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-695">POST</span><span class="sxs-lookup"><span data-stu-id="ca626-695">POST</span></span> |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27` |
| <span data-ttu-id="ca626-696">ZÁHLAVÍ</span><span class="sxs-lookup"><span data-stu-id="ca626-696">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="ca626-697">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-697">Parameter Name</span></span> | <span data-ttu-id="ca626-698">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-698">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-699">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-699">apiVersion</span></span> |<span data-ttu-id="ca626-700">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-700">1.0</span></span> |
| <span data-ttu-id="ca626-701">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-701">Request body</span></span> |<span data-ttu-id="ca626-702">Vkládání dat události pro všechny události chcete toosend.</span><span class="sxs-lookup"><span data-stu-id="ca626-702">Event data entry for each event you want toosend.</span></span> <span data-ttu-id="ca626-703">By měli poslat pro hello stejné relace uživatele nebo prohlížeče hello stejným ID v poli SessionId hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-703">You should send for hello same user or browser session hello same ID in hello SessionId field.</span></span> <span data-ttu-id="ca626-704">(Viz ukázka textu události, které jsou níže).</span><span class="sxs-lookup"><span data-stu-id="ca626-704">(See sample of event body below.)</span></span> |

* <span data-ttu-id="ca626-705">Příklad pro události, klikněte na tlačítko'.</span><span class="sxs-lookup"><span data-stu-id="ca626-705">Example for event 'Click':</span></span>
  
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
* <span data-ttu-id="ca626-706">Příklad pro událost 'RecommendationClick':</span><span class="sxs-lookup"><span data-stu-id="ca626-706">Example for event 'RecommendationClick':</span></span>
  
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
* <span data-ttu-id="ca626-707">Příklad pro událost 'AddShopCart':</span><span class="sxs-lookup"><span data-stu-id="ca626-707">Example for event 'AddShopCart':</span></span>
  
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
* <span data-ttu-id="ca626-708">Příklad pro událost 'RemoveShopCart':</span><span class="sxs-lookup"><span data-stu-id="ca626-708">Example for event 'RemoveShopCart':</span></span>
  
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
* <span data-ttu-id="ca626-709">Příklad pro události, nákupu'.</span><span class="sxs-lookup"><span data-stu-id="ca626-709">Example for event 'Purchase':</span></span>
  
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
* <span data-ttu-id="ca626-710">Příklad odesílání 2 události, klikněte na' a 'AddShopCart':</span><span class="sxs-lookup"><span data-stu-id="ca626-710">Example sending 2 events, 'Click' and 'AddShopCart':</span></span>
  
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

<span data-ttu-id="ca626-711">**Odpověď**: kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-711">**Response**: HTTP Status code: 200</span></span>

### <a name="92----list-model-usage-files"></a><span data-ttu-id="ca626-712">9.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-712">9.2.</span></span>    <span data-ttu-id="ca626-713">Seznam modelu využití souborů</span><span class="sxs-lookup"><span data-stu-id="ca626-713">List Model Usage Files</span></span>
<span data-ttu-id="ca626-714">Načte metadata všechny soubory modelu využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-714">Retrieves metadata of all model usage files.</span></span>
<span data-ttu-id="ca626-715">Hello využití, které budou soubory načíst jednu stránku současně.</span><span class="sxs-lookup"><span data-stu-id="ca626-715">hello usage files will be retrieved one page at a time.</span></span> <span data-ttu-id="ca626-716">Položky neobsahuje 100 každé stránky.</span><span class="sxs-lookup"><span data-stu-id="ca626-716">Each page containes 100 items.</span></span> <span data-ttu-id="ca626-717">Pokud chcete tooget položek v konkrétním indexem, můžete použít parametr hello $skip odata.</span><span class="sxs-lookup"><span data-stu-id="ca626-717">If you want tooget items at a particular index, you can use hello $skip odata parameter.</span></span> <span data-ttu-id="ca626-718">Například pokud chcete tooget položky začínající na pozici 100, přidejte parametr hello $skip = 100 toohello požadavku.</span><span class="sxs-lookup"><span data-stu-id="ca626-718">For instance if you want tooget items starting at position 100, add hello parameter $skip=100 toohello request.</span></span>

| <span data-ttu-id="ca626-719">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-719">HTTP Method</span></span> | <span data-ttu-id="ca626-720">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-720">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-721">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-721">GET</span></span> |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-722">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-722">Example:</span></span><br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-723">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-723">Parameter Name</span></span> | <span data-ttu-id="ca626-724">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-724">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-725">forModelId</span><span class="sxs-lookup"><span data-stu-id="ca626-725">forModelId</span></span> |<span data-ttu-id="ca626-726">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-726">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-727">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-727">apiVersion</span></span> |<span data-ttu-id="ca626-728">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-728">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-729">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-729">Request Body</span></span> |<span data-ttu-id="ca626-730">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-730">NONE</span></span> |

<span data-ttu-id="ca626-731">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-731">**Response**:</span></span>

<span data-ttu-id="ca626-732">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-732">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-733">Hello odpověď obsahuje jeden záznam za použití souboru.</span><span class="sxs-lookup"><span data-stu-id="ca626-733">hello response includes one entry per usage file.</span></span> <span data-ttu-id="ca626-734">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-734">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-735">`feed\entry\content\properties\Id`– ID využití souboru.</span><span class="sxs-lookup"><span data-stu-id="ca626-735">`feed\entry\content\properties\Id` - Usage file ID.</span></span>
* <span data-ttu-id="ca626-736">`feed\entry\content\properties\Length`-Využití délka souboru v MB.</span><span class="sxs-lookup"><span data-stu-id="ca626-736">`feed\entry\content\properties\Length` - Usage file length in MB.</span></span>
* <span data-ttu-id="ca626-737">`feed\entry\content\properties\DateModified`-Datum vytvoření souboru využití hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-737">`feed\entry\content\properties\DateModified` - Date when hello usage file was created.</span></span>
* <span data-ttu-id="ca626-738">`feed\entry\content\properties\UseInModel`– Jestli hello využití souboru se používá v modelu hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-738">`feed\entry\content\properties\UseInModel` - Whether hello usage file is used in hello model.</span></span>

<span data-ttu-id="ca626-739">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-739">OData XML</span></span>

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

### <a name="93----get-usage-statistics"></a><span data-ttu-id="ca626-740">9.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-740">9.3.</span></span>    <span data-ttu-id="ca626-741">Získat statistiku využití</span><span class="sxs-lookup"><span data-stu-id="ca626-741">Get Usage Statistics</span></span>
<span data-ttu-id="ca626-742">Získá Statistika využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-742">Gets usage statistics.</span></span>

| <span data-ttu-id="ca626-743">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-743">HTTP Method</span></span> | <span data-ttu-id="ca626-744">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-744">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-745">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-745">GET</span></span> |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-746">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-746">Example:</span></span><br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-747">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-747">Parameter Name</span></span> | <span data-ttu-id="ca626-748">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-748">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-749">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-749">modelId</span></span> |<span data-ttu-id="ca626-750">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-750">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-751">Počátečním</span><span class="sxs-lookup"><span data-stu-id="ca626-751">startDate</span></span> |<span data-ttu-id="ca626-752">Počáteční datum.</span><span class="sxs-lookup"><span data-stu-id="ca626-752">Start date.</span></span> <span data-ttu-id="ca626-753">Formát: rrrr/MM/ddTHH</span><span class="sxs-lookup"><span data-stu-id="ca626-753">Format: yyyy/MM/ddTHH:mm:ss</span></span> |
| <span data-ttu-id="ca626-754">Koncové datum</span><span class="sxs-lookup"><span data-stu-id="ca626-754">endDate</span></span> |<span data-ttu-id="ca626-755">Koncové datum.</span><span class="sxs-lookup"><span data-stu-id="ca626-755">End date.</span></span> <span data-ttu-id="ca626-756">Formát: rrrr/MM/ddTHH</span><span class="sxs-lookup"><span data-stu-id="ca626-756">Format: yyyy/MM/ddTHH:mm:ss</span></span> |
| <span data-ttu-id="ca626-757">eventTypes</span><span class="sxs-lookup"><span data-stu-id="ca626-757">eventTypes</span></span> |<span data-ttu-id="ca626-758">Textový soubor s oddělovači řetězec události typy nebo hodnota null tooget všechny události</span><span class="sxs-lookup"><span data-stu-id="ca626-758">Comma-separated string of event types or null tooget all events</span></span> |
| <span data-ttu-id="ca626-759">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-759">apiVersion</span></span> |<span data-ttu-id="ca626-760">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-760">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-761">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-761">Request Body</span></span> |<span data-ttu-id="ca626-762">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-762">NONE</span></span> |

<span data-ttu-id="ca626-763">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-763">**Response**:</span></span>

<span data-ttu-id="ca626-764">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-764">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-765">Kolekci elementů klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="ca626-765">A collection of key/value elements.</span></span> <span data-ttu-id="ca626-766">Každé z nich obsahuje součet hello události pro určitý typ události seskupené podle hodinu.</span><span class="sxs-lookup"><span data-stu-id="ca626-766">Each one contains hello sum of events for a specific event type grouped by hour.</span></span>

* <span data-ttu-id="ca626-767">`feed\entry[i]\content\properties\Key`-Obsahuje dobu hello (seskupené podle hodinu) a typ události hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-767">`feed\entry[i]\content\properties\Key` - Contains hello time (grouped by hour) and hello event type.</span></span>
* <span data-ttu-id="ca626-768">`feed\entry[i]\content\properties\Value`-Počet celkový počet událostí.</span><span class="sxs-lookup"><span data-stu-id="ca626-768">`feed\entry[i]\content\properties\Value` - Total event count.</span></span>

<span data-ttu-id="ca626-769">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-769">OData XML</span></span>

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

### <a name="94----get-usage-file-sample"></a><span data-ttu-id="ca626-770">9.4.</span><span class="sxs-lookup"><span data-stu-id="ca626-770">9.4.</span></span>    <span data-ttu-id="ca626-771">Získat ukázkový soubor využití</span><span class="sxs-lookup"><span data-stu-id="ca626-771">Get Usage File Sample</span></span>
<span data-ttu-id="ca626-772">Načte hello první 2KB využití obsahu souboru.</span><span class="sxs-lookup"><span data-stu-id="ca626-772">Retrieves hello first 2KB of usage file content.</span></span>

| <span data-ttu-id="ca626-773">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-773">HTTP Method</span></span> | <span data-ttu-id="ca626-774">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-774">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-775">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-775">GET</span></span> |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-776">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-776">Example:</span></span><br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-777">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-777">Parameter Name</span></span> | <span data-ttu-id="ca626-778">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-778">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-779">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-779">modelId</span></span> |<span data-ttu-id="ca626-780">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-780">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-781">fileId</span><span class="sxs-lookup"><span data-stu-id="ca626-781">fileId</span></span> |<span data-ttu-id="ca626-782">Jedinečný identifikátor hello modelu využití souboru</span><span class="sxs-lookup"><span data-stu-id="ca626-782">Unique identifier of hello model usage file</span></span> |
| <span data-ttu-id="ca626-783">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-783">apiVersion</span></span> |<span data-ttu-id="ca626-784">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-784">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-785">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-785">Request Body</span></span> |<span data-ttu-id="ca626-786">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-786">NONE</span></span> |

<span data-ttu-id="ca626-787">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-787">**Response**:</span></span>

<span data-ttu-id="ca626-788">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-788">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-789">Odpověď se vrátí ve formátu raw textu:</span><span class="sxs-lookup"><span data-stu-id="ca626-789">Response is returned in raw text format:</span></span>

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


### <a name="95----get-model-usage-file"></a><span data-ttu-id="ca626-790">9.5.</span><span class="sxs-lookup"><span data-stu-id="ca626-790">9.5.</span></span>    <span data-ttu-id="ca626-791">Získat soubor modelu využití</span><span class="sxs-lookup"><span data-stu-id="ca626-791">Get Model Usage File</span></span>
<span data-ttu-id="ca626-792">Načte hello celý obsah souboru využití hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-792">Retrieves hello full content of hello usage file.</span></span>

| <span data-ttu-id="ca626-793">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-793">HTTP Method</span></span> | <span data-ttu-id="ca626-794">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-794">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-795">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-795">GET</span></span> |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-796">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-796">Example:</span></span><br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-797">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-797">Parameter Name</span></span> | <span data-ttu-id="ca626-798">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-798">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-799">Mid –</span><span class="sxs-lookup"><span data-stu-id="ca626-799">mid</span></span> |<span data-ttu-id="ca626-800">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-800">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-801">FID</span><span class="sxs-lookup"><span data-stu-id="ca626-801">fid</span></span> |<span data-ttu-id="ca626-802">Jedinečný identifikátor hello modelu využití souboru</span><span class="sxs-lookup"><span data-stu-id="ca626-802">Unique identifier of hello model usage file</span></span> |
| <span data-ttu-id="ca626-803">Stahování</span><span class="sxs-lookup"><span data-stu-id="ca626-803">download</span></span> |<span data-ttu-id="ca626-804">1</span><span class="sxs-lookup"><span data-stu-id="ca626-804">1</span></span> |
| <span data-ttu-id="ca626-805">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-805">apiVersion</span></span> |<span data-ttu-id="ca626-806">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-806">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-807">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-807">Request Body</span></span> |<span data-ttu-id="ca626-808">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-808">NONE</span></span> |

<span data-ttu-id="ca626-809">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-809">**Response**:</span></span>

<span data-ttu-id="ca626-810">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-810">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-811">Odpověď se vrátí ve formátu raw textu:</span><span class="sxs-lookup"><span data-stu-id="ca626-811">Response is returned in raw text format:</span></span>

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

### <a name="96----delete-usage-file"></a><span data-ttu-id="ca626-812">9.6.</span><span class="sxs-lookup"><span data-stu-id="ca626-812">9.6.</span></span>    <span data-ttu-id="ca626-813">Odstranit soubor využití</span><span class="sxs-lookup"><span data-stu-id="ca626-813">Delete Usage File</span></span>
<span data-ttu-id="ca626-814">Odstraní soubor hello zadaného modelu využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-814">Deletes hello specified model usage file.</span></span>

| <span data-ttu-id="ca626-815">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-815">HTTP Method</span></span> | <span data-ttu-id="ca626-816">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-816">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-817">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="ca626-817">DELETE</span></span> |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-818">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-818">Example:</span></span><br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-819">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-819">Parameter Name</span></span> | <span data-ttu-id="ca626-820">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-820">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-821">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-821">modelId</span></span> |<span data-ttu-id="ca626-822">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-822">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-823">fileId</span><span class="sxs-lookup"><span data-stu-id="ca626-823">fileId</span></span> |<span data-ttu-id="ca626-824">Jedinečný identifikátor toobe souboru hello odstranit</span><span class="sxs-lookup"><span data-stu-id="ca626-824">Unique identifier of hello file toobe deleted</span></span> |
| <span data-ttu-id="ca626-825">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-825">apiVersion</span></span> |<span data-ttu-id="ca626-826">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-826">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-827">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-827">Request Body</span></span> |<span data-ttu-id="ca626-828">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-828">NONE</span></span> |

<span data-ttu-id="ca626-829">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-829">**Response**:</span></span>

<span data-ttu-id="ca626-830">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-830">HTTP Status code: 200</span></span>

### <a name="97----delete-all-usage-files"></a><span data-ttu-id="ca626-831">9.7.</span><span class="sxs-lookup"><span data-stu-id="ca626-831">9.7.</span></span>    <span data-ttu-id="ca626-832">Odstraní všechny soubory využití</span><span class="sxs-lookup"><span data-stu-id="ca626-832">Delete All Usage Files</span></span>
<span data-ttu-id="ca626-833">Odstraní všechny soubory modelu využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-833">Deletes all model usage files.</span></span>

| <span data-ttu-id="ca626-834">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-834">HTTP Method</span></span> | <span data-ttu-id="ca626-835">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-835">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-836">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="ca626-836">DELETE</span></span> |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-837">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-837">Example:</span></span><br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-838">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-838">Parameter Name</span></span> | <span data-ttu-id="ca626-839">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-839">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-840">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-840">modelId</span></span> |<span data-ttu-id="ca626-841">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-841">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-842">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-842">apiVersion</span></span> |<span data-ttu-id="ca626-843">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-843">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-844">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-844">Request Body</span></span> |<span data-ttu-id="ca626-845">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-845">NONE</span></span> |

<span data-ttu-id="ca626-846">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-846">**Response**:</span></span>

<span data-ttu-id="ca626-847">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-847">HTTP Status code: 200</span></span>

## <a name="10-features"></a><span data-ttu-id="ca626-848">10. Funkce</span><span class="sxs-lookup"><span data-stu-id="ca626-848">10. Features</span></span>
<span data-ttu-id="ca626-849">Tato část uvádí, jak tooretrieve funkci informace, jako je například funkce hello importovat a jejich hodnoty, jejich pořadí, a pokud byl přidělen toto pořadí.</span><span class="sxs-lookup"><span data-stu-id="ca626-849">This section shows how tooretrieve feature information, such as hello imported features and their values, their rank, and when this rank was allocated.</span></span> <span data-ttu-id="ca626-850">Funkce importují v rámci hello katalogu dat a jejich pořadí je pak přidružené, pokud se provádí rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-850">Features are imported as part of hello catalog data, and then their rank is associated when a rank build is done.</span></span>
<span data-ttu-id="ca626-851">Funkce pořadí můžete změnit podle vzoru toohello data o využití a typu položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-851">Feature rank can change according toohello pattern of usage data and type of items.</span></span> <span data-ttu-id="ca626-852">Ale pro konzistentní využití nebo položky, hello pořadí by měl mít jenom malé kolísání.</span><span class="sxs-lookup"><span data-stu-id="ca626-852">But for consistent usage/items, hello rank should have only small fluctuations.</span></span>
<span data-ttu-id="ca626-853">pořadí Hello funkcí je nezáporné číslo.</span><span class="sxs-lookup"><span data-stu-id="ca626-853">hello rank of features is a non-negative number.</span></span> <span data-ttu-id="ca626-854">Hello číslo 0 znamená nebyl seřazeny této funkce hello (se stane, když vyvoláte dokončení předchozí toohello hello první rank sestavení toto rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="ca626-854">hello  number 0 means that hello feature was not ranked (happens if you invoke this API prior toohello completion of hello first rank build).</span></span> <span data-ttu-id="ca626-855">hello skóre podle aktuálnosti se nazývá Hello datum, kdy byl hello pořadí s atributy.</span><span class="sxs-lookup"><span data-stu-id="ca626-855">hello date at which hello rank was attributed is called hello score freshness.</span></span>

### <a name="101-get-features-info-for-last-rank-build"></a><span data-ttu-id="ca626-856">10.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-856">10.1.</span></span> <span data-ttu-id="ca626-857">Získání informací o funkcích (pro poslední Rank sestavení)</span><span class="sxs-lookup"><span data-stu-id="ca626-857">Get Features Info (For Last Rank Build)</span></span>
<span data-ttu-id="ca626-858">Načte informace o funkci hello, včetně hodnocení hello poslední úspěšné rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-858">Retrieves hello feature information, including ranking, for hello last successful rank build.</span></span>

| <span data-ttu-id="ca626-859">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-859">HTTP Method</span></span> | <span data-ttu-id="ca626-860">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-860">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-861">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-861">GET</span></span> |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-862">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-862">Example:</span></span><br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-863">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-863">Parameter Name</span></span> | <span data-ttu-id="ca626-864">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-864">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-865">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-865">modelId</span></span> |<span data-ttu-id="ca626-866">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-866">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-867">samplingSize</span><span class="sxs-lookup"><span data-stu-id="ca626-867">samplingSize</span></span> |<span data-ttu-id="ca626-868">Počet hodnot tooinclude pro každou součást podle toohello data obsažená v katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-868">Number of values tooinclude for each feature according toohello data present in hello catalog.</span></span> <br/><span data-ttu-id="ca626-869">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ca626-869">Possible values are:</span></span><br> <span data-ttu-id="ca626-870">-1 - všechny vzorky.</span><span class="sxs-lookup"><span data-stu-id="ca626-870">-1 - All samples.</span></span> <br><span data-ttu-id="ca626-871">0 – žádný vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ca626-871">0 - No sampling.</span></span> <br><span data-ttu-id="ca626-872">N - vrátí N ukázky pro každý název funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-872">N - Return N samples for each feature name.</span></span> |
| <span data-ttu-id="ca626-873">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-873">apiVersion</span></span> |<span data-ttu-id="ca626-874">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-874">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-875">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-875">Request Body</span></span> |<span data-ttu-id="ca626-876">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-876">NONE</span></span> |

<span data-ttu-id="ca626-877">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-877">**Response**:</span></span>

<span data-ttu-id="ca626-878">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-878">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-879">Hello odpovědi obsahuje seznam položek informace o funkci.</span><span class="sxs-lookup"><span data-stu-id="ca626-879">hello response contains a list of feature info entries.</span></span> <span data-ttu-id="ca626-880">Každý záznam obsahuje:</span><span class="sxs-lookup"><span data-stu-id="ca626-880">Each entry contains:</span></span>

* <span data-ttu-id="ca626-881">`feed/entry/content/m:properties/d:Name`– Název funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-881">`feed/entry/content/m:properties/d:Name` - Feature name.</span></span>
* <span data-ttu-id="ca626-882">`feed/entry/content/m:properties/d:RankUpdateDate`-Datum, na které hello pořadí byl funkce přidělené toothis také znám jako</span><span class="sxs-lookup"><span data-stu-id="ca626-882">`feed/entry/content/m:properties/d:RankUpdateDate` - Date at which hello rank was allocated toothis feature, a.k.a.</span></span> <span data-ttu-id="ca626-883">skóre podle aktuálnosti funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-883">score freshness feature.</span></span> <span data-ttu-id="ca626-884">Historická data (0001-01-01T00:00:00 ") znamená, že byla provedena žádná rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-884">A historical date ('0001-01-01T00:00:00') means that no rank build was performed.</span></span>
* <span data-ttu-id="ca626-885">`feed/entry/content/m:properties/d:Rank`– Pořadí funkce (float).</span><span class="sxs-lookup"><span data-stu-id="ca626-885">`feed/entry/content/m:properties/d:Rank` - Feature rank (float).</span></span> <span data-ttu-id="ca626-886">Pořadí 2.0 nebo vyšší je považován za dobré funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-886">A rank of 2.0 and up is considered a good feature.</span></span>
* <span data-ttu-id="ca626-887">`feed/entry/content/m:properties/d:SampleValues`-Textový soubor s oddělovači seznamu hodnot si požadovaná velikost toohello vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ca626-887">`feed/entry/content/m:properties/d:SampleValues` - Comma-separated list of values up toohello sampling size requested.</span></span>

<span data-ttu-id="ca626-888">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-888">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get hello features of a model</subtitle>
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

### <a name="102-get-features-info-for-specific-rank-build"></a><span data-ttu-id="ca626-889">10.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-889">10.2.</span></span> <span data-ttu-id="ca626-890">Získání informací o funkcích (pro konkrétní Rank sestavení)</span><span class="sxs-lookup"><span data-stu-id="ca626-890">Get Features Info (For Specific Rank Build)</span></span>
<span data-ttu-id="ca626-891">Načte informace o funkci hello, včetně hello řazení pro konkrétní rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-891">Retrieves hello feature information, including hello ranking for a specific rank build.</span></span>

| <span data-ttu-id="ca626-892">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-892">HTTP Method</span></span> | <span data-ttu-id="ca626-893">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-893">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-894">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-894">GET</span></span> |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-895">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-895">Example:</span></span><br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-896">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-896">Parameter Name</span></span> | <span data-ttu-id="ca626-897">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-897">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-898">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-898">modelId</span></span> |<span data-ttu-id="ca626-899">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-899">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-900">samplingSize</span><span class="sxs-lookup"><span data-stu-id="ca626-900">samplingSize</span></span> |<span data-ttu-id="ca626-901">Počet hodnot tooinclude pro každou součást podle toohello data obsažená v katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-901">Number of values tooinclude for each feature according toohello data present in hello catalog.</span></span><br/> <span data-ttu-id="ca626-902">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ca626-902">Possible values are:</span></span><br> <span data-ttu-id="ca626-903">-1 - všechny vzorky.</span><span class="sxs-lookup"><span data-stu-id="ca626-903">-1 - All samples.</span></span> <br><span data-ttu-id="ca626-904">0 – žádný vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ca626-904">0 - No sampling.</span></span> <br><span data-ttu-id="ca626-905">N - vrátí N ukázky pro každý název funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-905">N - Return N samples for each feature name.</span></span> |
| <span data-ttu-id="ca626-906">rankBuildId</span><span class="sxs-lookup"><span data-stu-id="ca626-906">rankBuildId</span></span> |<span data-ttu-id="ca626-907">Jedinečný identifikátor hello rank sestavení nebo -1 pro hello poslední rank sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-907">Unique identifier of hello rank build or -1 for hello last rank build</span></span> |
| <span data-ttu-id="ca626-908">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-908">apiVersion</span></span> |<span data-ttu-id="ca626-909">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-909">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-910">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-910">Request Body</span></span> |<span data-ttu-id="ca626-911">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-911">NONE</span></span> |

<span data-ttu-id="ca626-912">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-912">**Response**:</span></span>

<span data-ttu-id="ca626-913">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-913">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-914">Hello odpovědi obsahuje seznam položek informace o funkci.</span><span class="sxs-lookup"><span data-stu-id="ca626-914">hello response contains a list of feature info entries.</span></span> <span data-ttu-id="ca626-915">Každý záznam obsahuje:</span><span class="sxs-lookup"><span data-stu-id="ca626-915">Each entry contains:</span></span>

* <span data-ttu-id="ca626-916">`feed/entry/content/m:properties/d:Name`– Název funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-916">`feed/entry/content/m:properties/d:Name` - Feature name.</span></span>
* <span data-ttu-id="ca626-917">`feed/entry/content/m:properties/d:RankUpdateDate`-Datum, na které hello pořadí byl funkce přidělené toothis také znám jako</span><span class="sxs-lookup"><span data-stu-id="ca626-917">`feed/entry/content/m:properties/d:RankUpdateDate` - Date at which hello rank was allocated toothis feature, a.k.a.</span></span> <span data-ttu-id="ca626-918">skóre podle aktuálnosti funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-918">score freshness feature.</span></span> <span data-ttu-id="ca626-919">Historická data (0001-01-01T00:00:00 ") znamená, že byla provedena žádná rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-919">A historical date ('0001-01-01T00:00:00') means that no rank build was performed.</span></span>
* <span data-ttu-id="ca626-920">`feed/entry/content/m:properties/d:Rank`– Pořadí funkce (float).</span><span class="sxs-lookup"><span data-stu-id="ca626-920">`feed/entry/content/m:properties/d:Rank` - Feature rank (float).</span></span> <span data-ttu-id="ca626-921">Pořadí 2.0 nebo vyšší je považován za dobré funkce.</span><span class="sxs-lookup"><span data-stu-id="ca626-921">A rank of 2.0 and up is considered a good feature.</span></span>
* <span data-ttu-id="ca626-922">`feed/entry/content/m:properties/d:SampleValues`-Textový soubor s oddělovači seznamu hodnot si požadovaná velikost toohello vzorkování.</span><span class="sxs-lookup"><span data-stu-id="ca626-922">`feed/entry/content/m:properties/d:SampleValues` - Comma-separated list of values up toohello sampling size requested.</span></span>

<span data-ttu-id="ca626-923">OData</span><span class="sxs-lookup"><span data-stu-id="ca626-923">OData</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get hello features of a model</subtitle>
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


## <a name="11-build"></a><span data-ttu-id="ca626-924">11. Sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-924">11. Build</span></span>
  <span data-ttu-id="ca626-925">Tato část vysvětluje hello různých rozhraní API související s toobuilds.</span><span class="sxs-lookup"><span data-stu-id="ca626-925">This section explains hello different APIs related toobuilds.</span></span> <span data-ttu-id="ca626-926">Existují 3 typy sestavení: sestavení doporučení, rank sestavení a sestavení FBT (často koupili společně).</span><span class="sxs-lookup"><span data-stu-id="ca626-926">There are 3 types of builds: a recommendation build, a rank build and an FBT (frequently bought together) build.</span></span>

<span data-ttu-id="ca626-927">účelem sestavení doporučení Hello je toogenerate používá model doporučení pro předpovědi.</span><span class="sxs-lookup"><span data-stu-id="ca626-927">hello recommendation build's purpose is toogenerate a recommendation model used for predictions.</span></span> <span data-ttu-id="ca626-928">Predikce (pro tento typ sestavení) má ve dvou typů:</span><span class="sxs-lookup"><span data-stu-id="ca626-928">Predictions (for this type of build) come in two flavors:</span></span>

* <span data-ttu-id="ca626-929">I2I - také znám jako</span><span class="sxs-lookup"><span data-stu-id="ca626-929">I2I - a.k.a.</span></span> <span data-ttu-id="ca626-930">Položka tooItem doporučení - danou položku nebo seznam položek bude tato možnost předpovědi seznam položek, které jsou pravděpodobně toobe vysoké zájmu.</span><span class="sxs-lookup"><span data-stu-id="ca626-930">Item tooItem recommendations - given an item or a list of items this option will predict a list of items that are likely toobe of high interest.</span></span>
* <span data-ttu-id="ca626-931">U2I - také znám jako</span><span class="sxs-lookup"><span data-stu-id="ca626-931">U2I - a.k.a.</span></span> <span data-ttu-id="ca626-932">Doporučení tooItem uživatele – zadané id uživatele (a volitelně seznam položek) Tato možnost bude předpovídat seznam položek, které jsou pravděpodobně toobe vysoké týkající se hello daného uživatele (a další volbu položek).</span><span class="sxs-lookup"><span data-stu-id="ca626-932">User tooItem recommendations - given a user id (and optionally a list of items) this option will predict a list of items that are likely toobe of high interest for hello given user (and its additional choice of items).</span></span> <span data-ttu-id="ca626-933">Hello U2I doporučení jsou založená na hello historie položek, které byly zajímavé pro uživatele hello až toohello době hello model byl vytvořený.</span><span class="sxs-lookup"><span data-stu-id="ca626-933">hello U2I recommendations are based on hello history of items that were of interest for hello user up toohello time hello model was built.</span></span>

<span data-ttu-id="ca626-934">Rank sestavení je technické sestavení, který vám umožní toolearn o hello užitečnost vaší funkcí.</span><span class="sxs-lookup"><span data-stu-id="ca626-934">A rank build is a technical build that allows you toolearn about hello usefulness of your features.</span></span> <span data-ttu-id="ca626-935">Obvykle v pořadí tooget hello nejlepší výsledek pro model doporučení zahrnující funkce, byste měli vzít hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ca626-935">Usually, in order tooget hello best result for a recommendation model involving features, you should take hello following steps:</span></span>

* <span data-ttu-id="ca626-936">Aktivovat rank build (Pokud hello skóre vaší funkcí není stabilní) a počkejte, dokud získat hello funkce skóre.</span><span class="sxs-lookup"><span data-stu-id="ca626-936">Trigger a rank build (unless hello score of your features is stable) and wait till you get hello feature score.</span></span>
* <span data-ttu-id="ca626-937">Načíst hello pořadí vaší funkce tak, že volání hello [získat informace o funkcích](#101-get-features-info-for-last-rank-build) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca626-937">Retrieve hello rank of your features by calling hello [Get Features Info](#101-get-features-info-for-last-rank-build) API.</span></span>
* <span data-ttu-id="ca626-938">Konfigurace sestavení doporučení s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="ca626-938">Configure a recommendation build with hello following parameters:</span></span>
  * <span data-ttu-id="ca626-939">`useFeatureInModel`-Set tooTrue.</span><span class="sxs-lookup"><span data-stu-id="ca626-939">`useFeatureInModel` - Set tooTrue.</span></span>
  * <span data-ttu-id="ca626-940">`ModelingFeatureList`-Set tooa čárkami oddělený seznam funkcí se skóre 2.0 nebo více (podle pořadí toohello, který jste získali v předchozím kroku hello).</span><span class="sxs-lookup"><span data-stu-id="ca626-940">`ModelingFeatureList` - Set tooa comma-separated list of features with a score of 2.0 or more (according toohello ranks you retrieved in hello previous step).</span></span>
  * <span data-ttu-id="ca626-941">`AllowColdItemPlacement`-Set tooTrue.</span><span class="sxs-lookup"><span data-stu-id="ca626-941">`AllowColdItemPlacement` - Set tooTrue.</span></span>
  * <span data-ttu-id="ca626-942">Volitelně můžete nastavit `EnableFeatureCorrelation` tooTrue a `ReasoningFeatureList` toohello seznam funkcí, které chcete použít toouse pro vysvětlení (obvykle hello používá stejný seznam funkcí v modelování nebo dílčí seznam).</span><span class="sxs-lookup"><span data-stu-id="ca626-942">Optionally you can set `EnableFeatureCorrelation` tooTrue and `ReasoningFeatureList` toohello list of features you want toouse for explanations (usually hello same list of features used in modelling or a sublist).</span></span>
* <span data-ttu-id="ca626-943">Aktivační událost hello doporučení sestavení s hello nakonfigurovat parametry.</span><span class="sxs-lookup"><span data-stu-id="ca626-943">Trigger hello recommendation build with hello configured parameters.</span></span>

<span data-ttu-id="ca626-944">Poznámka: Pokud nenakonfigurujete žádné parametry (například vyvolání hello doporučení sestavení bez parametrů) nebo explicitně nezakážete hello využití funkcí (například `UseFeatureInModel` nastavit tooFalse), systému hello se nastavit parametry související s funkcí hello toohello popsané výše uvedené hodnoty v případě, že existuje rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-944">Note: If you do not configure any parameters (e.g. invoke hello recommendation build without parameters) or you do not explicitly disable hello usage of features (e.g. `UseFeatureInModel` set tooFalse), hello system will set up hello feature-related parameters toohello explained values above in case a rank build exists.</span></span>

<span data-ttu-id="ca626-945">Neexistuje žádné omezení na spuštění rank sestavení a doporučení sestavení současně pro hello stejný model.</span><span class="sxs-lookup"><span data-stu-id="ca626-945">There is no restriction on running a rank build and a recommendation build concurrently for hello same model.</span></span> <span data-ttu-id="ca626-946">Nicméně nelze spustit dvě sestavení hello stejný typ na hello stejný model paralelně.</span><span class="sxs-lookup"><span data-stu-id="ca626-946">Nevertheless, you cannot run two builds of hello same type on hello same model in parallel.</span></span>

<span data-ttu-id="ca626-947">Sestavení FBT (často koupili společně) je ještě jiný algoritmus doporučení názvem někdy "konzervativní" doporučené, který je vhodný pro katalogů, které nejsou ve své podstatě homogenní (homogenní: knihy, filmy, některé jídlo dotazování; bez homogenní: počítače a zařízení, mezi doménami, velmi různorodému).</span><span class="sxs-lookup"><span data-stu-id="ca626-947">An FBT (Frequently bought together) build is yet another recommendations algorithm called sometimes "conservative" recommender, which is good for catalogs that are not homogeneous in nature (homogeneous: books, movies, some food, fashion; non-homogeneous: computer and devices, cross-domain, highly diverse).</span></span>

<span data-ttu-id="ca626-948">Poznámka: Pokud hello využití soubory, které jste odeslali obsahovat hello volitelné pole "typ události" pak pro FBT modelování pouze "Nákupu" události se použije.</span><span class="sxs-lookup"><span data-stu-id="ca626-948">Note: if hello usage files that you uploaded contain hello optional field "event type" then for FBT modelling only "Purchase" events will be used.</span></span> <span data-ttu-id="ca626-949">Pokud je k dispozici žádný typ události, že všechny události bude považovat za nákupu.</span><span class="sxs-lookup"><span data-stu-id="ca626-949">If no event type is provided all events will be considered as purchase.</span></span>

#### <a name="111-build-parameters"></a><span data-ttu-id="ca626-950">11.1 sestavení parametry</span><span class="sxs-lookup"><span data-stu-id="ca626-950">11.1 Build parameters</span></span>
<span data-ttu-id="ca626-951">Každý typ sestavení je možné nakonfigurovat přes sadu parametrů (které popsané níže).</span><span class="sxs-lookup"><span data-stu-id="ca626-951">Each build type can be configured via a set of parameters (depicted below).</span></span> <span data-ttu-id="ca626-952">Pokud nenakonfigurujete hello parametry, bude systém hello automaticky atribut toohello parametrů podle toohello informace obsažené v době hello, že aktivovat build.</span><span class="sxs-lookup"><span data-stu-id="ca626-952">If you don't configure hello parameters, hello system will automatically attribute values toohello parameters according toohello information present at hello time you trigger a build.</span></span>

##### <a name="1111-usage-condenser"></a><span data-ttu-id="ca626-953">11.1.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-953">11.1.1.</span></span> <span data-ttu-id="ca626-954">Chladič využití</span><span class="sxs-lookup"><span data-stu-id="ca626-954">Usage condenser</span></span>
<span data-ttu-id="ca626-955">Další šumu než informace může obsahovat uživatele nebo položky s několika bodů využití.</span><span class="sxs-lookup"><span data-stu-id="ca626-955">Users or items with few usage points might contain more noise than information.</span></span> <span data-ttu-id="ca626-956">Hello systém pokusí toopredict hello minimální počet bodů využití podle uživatele nebo položku toobe použít v modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-956">hello system attempts toopredict hello minimal number of usage points per user/item toobe used in a model.</span></span> <span data-ttu-id="ca626-957">Toto číslo se bude v rozsahu hello definované hello ItemCutoffLowerBound a ItemCutoffUpperBound parametry položky a hello rozsah definovaný hello UserCutOffLowerBound a UserCutoffUpperBound parametry pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-957">This number will be within hello range defined by hello ItemCutoffLowerBound and ItemCutoffUpperBound parameters for items, and hello range defined by hello UserCutOffLowerBound and UserCutoffUpperBound parameters for users.</span></span> <span data-ttu-id="ca626-958">nastavení alespoň jeden z hello odpovídající rozsah toozero můžete minimalizovat Hello chladič vliv na položky nebo uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ca626-958">hello condenser effect on items or users can be minimized by setting at least one of hello corresponding bounds toozero.</span></span>

##### <a name="1112-rank-build-parameters"></a><span data-ttu-id="ca626-959">11.1.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-959">11.1.2.</span></span> <span data-ttu-id="ca626-960">Pořadí sestavení parametry</span><span class="sxs-lookup"><span data-stu-id="ca626-960">Rank build parameters</span></span>
<span data-ttu-id="ca626-961">Následující tabulka Hello znázorňuje hello sestavení parametry pro rank sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-961">hello table below depicts hello build parameters for a rank build.</span></span>

| <span data-ttu-id="ca626-962">Klíč</span><span class="sxs-lookup"><span data-stu-id="ca626-962">Key</span></span> | <span data-ttu-id="ca626-963">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-963">Description</span></span> | <span data-ttu-id="ca626-964">Typ</span><span class="sxs-lookup"><span data-stu-id="ca626-964">Type</span></span> | <span data-ttu-id="ca626-965">Platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ca626-965">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ca626-966">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="ca626-966">NumberOfModelIterations</span></span> |<span data-ttu-id="ca626-967">Hello počet iterací provede hello modelu se odráží hello celkové výpočetní čas a hello přesnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-967">hello number of iterations hello model performs is reflected by hello overall compute time and hello model accuracy.</span></span> <span data-ttu-id="ca626-968">vyšší číslo hello Hello, hello lepší přesnosti, které budete mít, ale hello výpočetní dobu bude trvat déle.</span><span class="sxs-lookup"><span data-stu-id="ca626-968">hello higher hello number, hello better accuracy you will get, but hello compute time will take longer.</span></span> |<span data-ttu-id="ca626-969">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-969">Integer</span></span> |<span data-ttu-id="ca626-970">10-50</span><span class="sxs-lookup"><span data-stu-id="ca626-970">10-50</span></span> |
| <span data-ttu-id="ca626-971">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="ca626-971">NumberOfModelDimensions</span></span> |<span data-ttu-id="ca626-972">Hello počet dimenzí platí, že toohello počet "funkce" hello modelu se pokusí toofind v rámci vaše data.</span><span class="sxs-lookup"><span data-stu-id="ca626-972">hello number of dimensions relates toohello number of 'features' hello model will try toofind within your data.</span></span> <span data-ttu-id="ca626-973">Zvýšením počtu hello dimenzí vám umožní lepší doladění hello výsledky do menší clusterů.</span><span class="sxs-lookup"><span data-stu-id="ca626-973">Increasing hello number of dimensions will allow better fine-tuning of hello results into smaller clusters.</span></span> <span data-ttu-id="ca626-974">Však příliš mnoho dimenze zabrání hello modelu nalézt korelací mezi položkami.</span><span class="sxs-lookup"><span data-stu-id="ca626-974">However, too many dimensions will prevent hello model from finding correlations between items.</span></span> |<span data-ttu-id="ca626-975">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-975">Integer</span></span> |<span data-ttu-id="ca626-976">10-40</span><span class="sxs-lookup"><span data-stu-id="ca626-976">10-40</span></span> |
| <span data-ttu-id="ca626-977">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="ca626-977">ItemCutOffLowerBound</span></span> |<span data-ttu-id="ca626-978">Definuje hello položky dolní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-978">Defines hello item lower bound for hello condenser.</span></span> <span data-ttu-id="ca626-979">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-979">See usage condenser above.</span></span> |<span data-ttu-id="ca626-980">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-980">Integer</span></span> |<span data-ttu-id="ca626-981">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-981">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-982">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="ca626-982">ItemCutOffUpperBound</span></span> |<span data-ttu-id="ca626-983">Definuje hello položky horní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-983">Defines hello item upper bound for hello condenser.</span></span> <span data-ttu-id="ca626-984">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-984">See usage condenser above.</span></span> |<span data-ttu-id="ca626-985">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-985">Integer</span></span> |<span data-ttu-id="ca626-986">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-986">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-987">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="ca626-987">UserCutOffLowerBound</span></span> |<span data-ttu-id="ca626-988">Definuje hello uživatele dolní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-988">Defines hello user lower bound for hello condenser.</span></span> <span data-ttu-id="ca626-989">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-989">See usage condenser above.</span></span> |<span data-ttu-id="ca626-990">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-990">Integer</span></span> |<span data-ttu-id="ca626-991">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-991">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-992">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="ca626-992">UserCutOffUpperBound</span></span> |<span data-ttu-id="ca626-993">Definuje hello uživatele horní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-993">Defines hello user upper bound for hello condenser.</span></span> <span data-ttu-id="ca626-994">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-994">See usage condenser above.</span></span> |<span data-ttu-id="ca626-995">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-995">Integer</span></span> |<span data-ttu-id="ca626-996">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-996">2 or more (0 disable condenser)</span></span> |

##### <a name="1113-recommendation-build-parameters"></a><span data-ttu-id="ca626-997">11.1.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-997">11.1.3.</span></span> <span data-ttu-id="ca626-998">Parametry doporučení sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-998">Recommendation build parameters</span></span>
<span data-ttu-id="ca626-999">Následující tabulka Hello znázorňuje hello parametry sestavení pro sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-999">hello table below depicts hello build parameters for recommendation build.</span></span>

| <span data-ttu-id="ca626-1000">Klíč</span><span class="sxs-lookup"><span data-stu-id="ca626-1000">Key</span></span> | <span data-ttu-id="ca626-1001">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-1001">Description</span></span> | <span data-ttu-id="ca626-1002">Typ</span><span class="sxs-lookup"><span data-stu-id="ca626-1002">Type</span></span> | <span data-ttu-id="ca626-1003">Platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1003">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ca626-1004">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="ca626-1004">NumberOfModelIterations</span></span> |<span data-ttu-id="ca626-1005">Hello počet iterací provede hello modelu se odráží hello celkové výpočetní čas a hello přesnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1005">hello number of iterations hello model performs is reflected by hello overall compute time and hello model accuracy.</span></span> <span data-ttu-id="ca626-1006">vyšší číslo hello Hello, hello lepší přesnosti, které budete mít, ale hello výpočetní dobu bude trvat déle.</span><span class="sxs-lookup"><span data-stu-id="ca626-1006">hello higher hello number, hello better accuracy you will get, but hello compute time will take longer.</span></span> |<span data-ttu-id="ca626-1007">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1007">Integer</span></span> |<span data-ttu-id="ca626-1008">10-50</span><span class="sxs-lookup"><span data-stu-id="ca626-1008">10-50</span></span> |
| <span data-ttu-id="ca626-1009">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="ca626-1009">NumberOfModelDimensions</span></span> |<span data-ttu-id="ca626-1010">Hello počet dimenzí platí, že toohello počet "funkce" hello modelu se pokusí toofind v rámci vaše data.</span><span class="sxs-lookup"><span data-stu-id="ca626-1010">hello number of dimensions relates toohello number of 'features' hello model will try toofind within your data.</span></span> <span data-ttu-id="ca626-1011">Zvýšením počtu hello dimenzí vám umožní lepší doladění hello výsledky do menší clusterů.</span><span class="sxs-lookup"><span data-stu-id="ca626-1011">Increasing hello number of dimensions will allow better fine-tuning of hello results into smaller clusters.</span></span> <span data-ttu-id="ca626-1012">Však příliš mnoho dimenze zabrání hello modelu nalézt korelací mezi položkami.</span><span class="sxs-lookup"><span data-stu-id="ca626-1012">However, too many dimensions will prevent hello model from finding correlations between items.</span></span> |<span data-ttu-id="ca626-1013">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1013">Integer</span></span> |<span data-ttu-id="ca626-1014">10-40</span><span class="sxs-lookup"><span data-stu-id="ca626-1014">10-40</span></span> |
| <span data-ttu-id="ca626-1015">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="ca626-1015">ItemCutOffLowerBound</span></span> |<span data-ttu-id="ca626-1016">Definuje hello položky dolní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1016">Defines hello item lower bound for hello condenser.</span></span> <span data-ttu-id="ca626-1017">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-1017">See usage condenser above.</span></span> |<span data-ttu-id="ca626-1018">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1018">Integer</span></span> |<span data-ttu-id="ca626-1019">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-1019">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-1020">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="ca626-1020">ItemCutOffUpperBound</span></span> |<span data-ttu-id="ca626-1021">Definuje hello položky horní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1021">Defines hello item upper bound for hello condenser.</span></span> <span data-ttu-id="ca626-1022">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-1022">See usage condenser above.</span></span> |<span data-ttu-id="ca626-1023">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1023">Integer</span></span> |<span data-ttu-id="ca626-1024">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-1024">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-1025">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="ca626-1025">UserCutOffLowerBound</span></span> |<span data-ttu-id="ca626-1026">Definuje hello uživatele dolní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1026">Defines hello user lower bound for hello condenser.</span></span> <span data-ttu-id="ca626-1027">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-1027">See usage condenser above.</span></span> |<span data-ttu-id="ca626-1028">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1028">Integer</span></span> |<span data-ttu-id="ca626-1029">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-1029">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-1030">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="ca626-1030">UserCutOffUpperBound</span></span> |<span data-ttu-id="ca626-1031">Definuje hello uživatele horní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1031">Defines hello user upper bound for hello condenser.</span></span> <span data-ttu-id="ca626-1032">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-1032">See usage condenser above.</span></span> |<span data-ttu-id="ca626-1033">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1033">Integer</span></span> |<span data-ttu-id="ca626-1034">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-1034">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-1035">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-1035">Description</span></span> |<span data-ttu-id="ca626-1036">Vytvořte popis.</span><span class="sxs-lookup"><span data-stu-id="ca626-1036">Build description.</span></span> |<span data-ttu-id="ca626-1037">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ca626-1037">String</span></span> |<span data-ttu-id="ca626-1038">Jakýkoli text, maximální 512 znaků</span><span class="sxs-lookup"><span data-stu-id="ca626-1038">Any text, maximum 512 chars</span></span> |
| <span data-ttu-id="ca626-1039">EnableModelingInsights</span><span class="sxs-lookup"><span data-stu-id="ca626-1039">EnableModelingInsights</span></span> |<span data-ttu-id="ca626-1040">Umožňuje toocompute metriky na hello doporučení modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1040">Allows you toocompute metrics on hello recommendation model.</span></span> |<span data-ttu-id="ca626-1041">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ca626-1041">Boolean</span></span> |<span data-ttu-id="ca626-1042">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="ca626-1042">True/False</span></span> |
| <span data-ttu-id="ca626-1043">UseFeaturesInModel</span><span class="sxs-lookup"><span data-stu-id="ca626-1043">UseFeaturesInModel</span></span> |<span data-ttu-id="ca626-1044">Označuje, pokud funkce lze použít v pořadí tooenhance hello doporučení modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1044">Indicates if features can be used in order tooenhance hello recommendation model.</span></span> |<span data-ttu-id="ca626-1045">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ca626-1045">Boolean</span></span> |<span data-ttu-id="ca626-1046">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="ca626-1046">True/False</span></span> |
| <span data-ttu-id="ca626-1047">ModelingFeatureList</span><span class="sxs-lookup"><span data-stu-id="ca626-1047">ModelingFeatureList</span></span> |<span data-ttu-id="ca626-1048">Čárkami oddělený seznam toobe názvy funkce použité v sestavení hello doporučení, v pořadí tooenhance hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1048">Comma-separated list of feature names toobe used in hello recommendation build, in order tooenhance hello recommendation.</span></span> |<span data-ttu-id="ca626-1049">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ca626-1049">String</span></span> |<span data-ttu-id="ca626-1050">Názvy funkcí, až too512 znaků</span><span class="sxs-lookup"><span data-stu-id="ca626-1050">Feature names, up too512 chars</span></span> |
| <span data-ttu-id="ca626-1051">AllowColdItemPlacement</span><span class="sxs-lookup"><span data-stu-id="ca626-1051">AllowColdItemPlacement</span></span> |<span data-ttu-id="ca626-1052">Označuje, pokud hello doporučení by také push cold položky prostřednictvím podobností funkci.</span><span class="sxs-lookup"><span data-stu-id="ca626-1052">Indicates if hello recommendation should also push cold items via feature similarity.</span></span> |<span data-ttu-id="ca626-1053">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ca626-1053">Boolean</span></span> |<span data-ttu-id="ca626-1054">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="ca626-1054">True/False</span></span> |
| <span data-ttu-id="ca626-1055">EnableFeatureCorrelation</span><span class="sxs-lookup"><span data-stu-id="ca626-1055">EnableFeatureCorrelation</span></span> |<span data-ttu-id="ca626-1056">Určuje funkce lze nastavit v reasoning.</span><span class="sxs-lookup"><span data-stu-id="ca626-1056">Indicates if features can be used in reasoning.</span></span> |<span data-ttu-id="ca626-1057">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ca626-1057">Boolean</span></span> |<span data-ttu-id="ca626-1058">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="ca626-1058">True/False</span></span> |
| <span data-ttu-id="ca626-1059">ReasoningFeatureList</span><span class="sxs-lookup"><span data-stu-id="ca626-1059">ReasoningFeatureList</span></span> |<span data-ttu-id="ca626-1060">Textový soubor s oddělovači seznam toobe názvy funkcí pro odůvodnění věty (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1060">Comma-separated list of feature names toobe used for reasoning sentences (e.g. recommendation explanations).</span></span> |<span data-ttu-id="ca626-1061">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ca626-1061">String</span></span> |<span data-ttu-id="ca626-1062">Názvy funkcí, až too512 znaků</span><span class="sxs-lookup"><span data-stu-id="ca626-1062">Feature names, up too512 chars</span></span> |
| <span data-ttu-id="ca626-1063">EnableU2I</span><span class="sxs-lookup"><span data-stu-id="ca626-1063">EnableU2I</span></span> |<span data-ttu-id="ca626-1064">Povolit hello přizpůsobené doporučení také znám jako</span><span class="sxs-lookup"><span data-stu-id="ca626-1064">Allow hello personalized recommendation a.k.a.</span></span> <span data-ttu-id="ca626-1065">U2I (doporučení tooitem uživatele).</span><span class="sxs-lookup"><span data-stu-id="ca626-1065">U2I (user tooitem recommendations).</span></span> |<span data-ttu-id="ca626-1066">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ca626-1066">Boolean</span></span> |<span data-ttu-id="ca626-1067">Logická hodnota (true výchozí)</span><span class="sxs-lookup"><span data-stu-id="ca626-1067">True/False (default true)</span></span> |

##### <a name="1114-fbt-build-parameters"></a><span data-ttu-id="ca626-1068">11.1.4.</span><span class="sxs-lookup"><span data-stu-id="ca626-1068">11.1.4.</span></span> <span data-ttu-id="ca626-1069">Parametry FBT sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-1069">FBT build parameters</span></span>
<span data-ttu-id="ca626-1070">Následující tabulka Hello znázorňuje hello parametry sestavení pro sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1070">hello table below depicts hello build parameters for recommendation build.</span></span>

| <span data-ttu-id="ca626-1071">Klíč</span><span class="sxs-lookup"><span data-stu-id="ca626-1071">Key</span></span> | <span data-ttu-id="ca626-1072">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-1072">Description</span></span> | <span data-ttu-id="ca626-1073">Typ</span><span class="sxs-lookup"><span data-stu-id="ca626-1073">Type</span></span> | <span data-ttu-id="ca626-1074">Platnou hodnotu (výchozí)</span><span class="sxs-lookup"><span data-stu-id="ca626-1074">Valid Value (Default)</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ca626-1075">FbtSupportThreshold</span><span class="sxs-lookup"><span data-stu-id="ca626-1075">FbtSupportThreshold</span></span> |<span data-ttu-id="ca626-1076">Jak je model konzervativní hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1076">How conservative hello model is.</span></span> <span data-ttu-id="ca626-1077">Počet společné výskyty toobe položky, které jsou považovány za pro modelování.</span><span class="sxs-lookup"><span data-stu-id="ca626-1077">Number of co-occurrences of items toobe considered for modeling.</span></span> |<span data-ttu-id="ca626-1078">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1078">Integer</span></span> |<span data-ttu-id="ca626-1079">3-50 (6)</span><span class="sxs-lookup"><span data-stu-id="ca626-1079">3-50 (6)</span></span> |
| <span data-ttu-id="ca626-1080">FbtMaxItemSetSize</span><span class="sxs-lookup"><span data-stu-id="ca626-1080">FbtMaxItemSetSize</span></span> |<span data-ttu-id="ca626-1081">Rozsah hello počet položek v sadě časté.</span><span class="sxs-lookup"><span data-stu-id="ca626-1081">Bounds hello number of items in a frequent set.</span></span> |<span data-ttu-id="ca626-1082">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1082">Integer</span></span> |<span data-ttu-id="ca626-1083">2-3 (2)</span><span class="sxs-lookup"><span data-stu-id="ca626-1083">2-3 (2)</span></span> |
| <span data-ttu-id="ca626-1084">FbtMinimalScore</span><span class="sxs-lookup"><span data-stu-id="ca626-1084">FbtMinimalScore</span></span> |<span data-ttu-id="ca626-1085">Minimální skóre, které často sadu by měla mít v pořadí toobe součástí hello vrátí výsledky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1085">Minimal score that a frequent set should have in order toobe included in hello returned results.</span></span> <span data-ttu-id="ca626-1086">Hello vyšší hello lepší.</span><span class="sxs-lookup"><span data-stu-id="ca626-1086">hello higher hello better.</span></span> |<span data-ttu-id="ca626-1087">Double</span><span class="sxs-lookup"><span data-stu-id="ca626-1087">Double</span></span> |<span data-ttu-id="ca626-1088">0 a vyšší (0)</span><span class="sxs-lookup"><span data-stu-id="ca626-1088">0 and above (0)</span></span> |
| <span data-ttu-id="ca626-1089">FbtSimilarityFunction</span><span class="sxs-lookup"><span data-stu-id="ca626-1089">FbtSimilarityFunction</span></span> |<span data-ttu-id="ca626-1090">Definuje hello podobností funkci toobe používané hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1090">Defines hello similarity function toobe used by hello build.</span></span> <span data-ttu-id="ca626-1091">Navýšení upřednostňuje serendipity, společné výskyt upřednostňuje předvídatelnost a Jaccard je to dobrý kompromis mezi dvěma hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1091">Lift favors serendipity, Co-occurrence favors predictability, and Jaccard is a nice compromise between hello two.</span></span> |<span data-ttu-id="ca626-1092">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ca626-1092">String</span></span> |<span data-ttu-id="ca626-1093">cooccurrence, navýšení, jaccard (navýšení)</span><span class="sxs-lookup"><span data-stu-id="ca626-1093">cooccurrence, lift, jaccard (lift)</span></span> |

### <a name="112-trigger-a-recommendation-build"></a><span data-ttu-id="ca626-1094">11.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-1094">11.2.</span></span> <span data-ttu-id="ca626-1095">Aktivovat Build doporučení</span><span class="sxs-lookup"><span data-stu-id="ca626-1095">Trigger a Recommendation Build</span></span>
  <span data-ttu-id="ca626-1096">Ve výchozím nastavení toto rozhraní API aktivují sestavení modelu doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1096">By default this API will trigger a recommendation model build.</span></span> <span data-ttu-id="ca626-1097">tootrigger pořadí sestavení (v pořadí tooscore funkcí) by mělo být použito hello sestavení rozhraní API variant s parametrem typu sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1097">tootrigger a rank build (in order tooscore  features), hello build API variant with build type parameter should be used.</span></span>

| <span data-ttu-id="ca626-1098">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1098">HTTP Method</span></span> | <span data-ttu-id="ca626-1099">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1099">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1100">POST</span><span class="sxs-lookup"><span data-stu-id="ca626-1100">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1101">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1101">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |
| <span data-ttu-id="ca626-1102">ZÁHLAVÍ</span><span class="sxs-lookup"><span data-stu-id="ca626-1102">HEADER</span></span> |<span data-ttu-id="ca626-1103">`"Content-Type", "text/xml"`(Pokud odesílá text žádosti)</span><span class="sxs-lookup"><span data-stu-id="ca626-1103">`"Content-Type", "text/xml"` (If sending Request Body)</span></span> |

| <span data-ttu-id="ca626-1104">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1104">Parameter Name</span></span> | <span data-ttu-id="ca626-1105">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1105">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1106">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1106">modelId</span></span> |<span data-ttu-id="ca626-1107">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1107">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1108">userDescription</span><span class="sxs-lookup"><span data-stu-id="ca626-1108">userDescription</span></span> |<span data-ttu-id="ca626-1109">Textové identifikátor hello katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1109">Textual identifier of hello catalog.</span></span> <span data-ttu-id="ca626-1110">Poznámka: Pokud použijete prostory musí zakódovat ho s % 20 místo.</span><span class="sxs-lookup"><span data-stu-id="ca626-1110">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="ca626-1111">Najdete v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1111">See example above.</span></span><br><span data-ttu-id="ca626-1112">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="ca626-1112">Max length: 50</span></span> |
| <span data-ttu-id="ca626-1113">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1113">apiVersion</span></span> |<span data-ttu-id="ca626-1114">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1114">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-1115">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-1115">Request Body</span></span> |<span data-ttu-id="ca626-1116">Pokud je ponecháno prázdné pak hello sestavení bude proveden hello výchozí sestavení parametry.</span><span class="sxs-lookup"><span data-stu-id="ca626-1116">If left empty then hello build will be executed with hello default build parameters.</span></span><br><br><span data-ttu-id="ca626-1117">Pokud chcete tooset hello sestavení parametry, odešlete do textu hello jako hello následující ukázka hello parametry ve formátu XML.</span><span class="sxs-lookup"><span data-stu-id="ca626-1117">If you want tooset hello build parameters, send hello parameters as XML into hello body as in hello following sample.</span></span> <span data-ttu-id="ca626-1118">(Viz část hello "sestavení parametry" Vysvětlení parametrů hello.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>`</span><span class="sxs-lookup"><span data-stu-id="ca626-1118">(See hello "Build parameters" section for an explanation of hello parameters.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>`</span></span> |

<span data-ttu-id="ca626-1119">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-1119">**Response**:</span></span>

<span data-ttu-id="ca626-1120">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1120">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1121">Toto je asynchronní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca626-1121">This is an asynchronous API.</span></span> <span data-ttu-id="ca626-1122">Zobrazí se ID sestavení jako odpověď.</span><span class="sxs-lookup"><span data-stu-id="ca626-1122">You will get a build ID as a response.</span></span> <span data-ttu-id="ca626-1123">tooknow při sestavení hello skončila, měli byste volání rozhraní API "Získat sestavení stavu systému Model" hello a vyhledejte ID toto sestavení v odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1123">tooknow when hello build has ended, you should call hello “Get Builds Status of a Model” API and locate this build ID in hello response.</span></span> <span data-ttu-id="ca626-1124">Všimněte si, že sestavení může trvat od toohours minut v závislosti na velikosti hello dat hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1124">Note that a build can take from minutes toohours depending on hello size of hello data.</span></span>

<span data-ttu-id="ca626-1125">Doporučení nelze používat, dokud hello sestavení elementy end.</span><span class="sxs-lookup"><span data-stu-id="ca626-1125">You cannot consume recommendations till hello build ends.</span></span>

<span data-ttu-id="ca626-1126">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="ca626-1126">Valid build status:</span></span>

* <span data-ttu-id="ca626-1127">Vytvořit - byla vytvořena žádost sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1127">Create - Build request was created.</span></span>
* <span data-ttu-id="ca626-1128">Ve frontě - sestavení žádost byla odeslána a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="ca626-1128">Queued - Build request was sent and it is queued.</span></span>
* <span data-ttu-id="ca626-1129">Probíhá vytváření - sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1129">Building - Build is in progress.</span></span>
* <span data-ttu-id="ca626-1130">Úspěch - sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="ca626-1130">Success - Build ended successfully.</span></span>
* <span data-ttu-id="ca626-1131">Chyba: sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="ca626-1131">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="ca626-1132">Zrušena - sestavení byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="ca626-1132">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="ca626-1133">Zrušení - byla odeslána žádost o zrušení hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1133">Cancelling - A cancel request for hello build was sent.</span></span>

<span data-ttu-id="ca626-1134">Všimněte si, že hello sestavení, který ID naleznete v části hello následující cesty:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="ca626-1134">Note that hello build ID can be found under hello following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="ca626-1135">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-1135">OData XML</span></span>

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

### <a name="113-trigger-build-recommendation-rank-or-fbt"></a><span data-ttu-id="ca626-1136">11.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-1136">11.3.</span></span> <span data-ttu-id="ca626-1137">Aktivační události sestavení (doporučení, pořadí nebo FBT)</span><span class="sxs-lookup"><span data-stu-id="ca626-1137">Trigger Build (Recommendation, Rank or FBT)</span></span>
| <span data-ttu-id="ca626-1138">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1138">HTTP Method</span></span> | <span data-ttu-id="ca626-1139">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1139">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1140">POST</span><span class="sxs-lookup"><span data-stu-id="ca626-1140">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1141">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1141">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27` |
| <span data-ttu-id="ca626-1142">ZÁHLAVÍ</span><span class="sxs-lookup"><span data-stu-id="ca626-1142">HEADER</span></span> |<span data-ttu-id="ca626-1143">`"Content-Type", "text/xml"`(Pokud odesílá text žádosti)</span><span class="sxs-lookup"><span data-stu-id="ca626-1143">`"Content-Type", "text/xml"` (If sending Request Body)</span></span> |

| <span data-ttu-id="ca626-1144">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1144">Parameter Name</span></span> | <span data-ttu-id="ca626-1145">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1145">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1146">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1146">modelId</span></span> |<span data-ttu-id="ca626-1147">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1147">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1148">userDescription</span><span class="sxs-lookup"><span data-stu-id="ca626-1148">userDescription</span></span> |<span data-ttu-id="ca626-1149">Textové identifikátor hello katalogu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1149">Textual identifier of hello catalog.</span></span> <span data-ttu-id="ca626-1150">Poznámka: Pokud použijete prostory musí zakódovat ho s % 20 místo.</span><span class="sxs-lookup"><span data-stu-id="ca626-1150">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="ca626-1151">Najdete v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1151">See example above.</span></span><br><span data-ttu-id="ca626-1152">Maximální délka: 50</span><span class="sxs-lookup"><span data-stu-id="ca626-1152">Max length: 50</span></span> |
| <span data-ttu-id="ca626-1153">buildType</span><span class="sxs-lookup"><span data-stu-id="ca626-1153">buildType</span></span> |<span data-ttu-id="ca626-1154">Typ tooinvoke sestavení hello:</span><span class="sxs-lookup"><span data-stu-id="ca626-1154">Type of hello build tooinvoke:</span></span> <br/> <span data-ttu-id="ca626-1155">-"Doporučení" doporučení sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-1155">- 'Recommendation' for recommendation build</span></span> <br> <span data-ttu-id="ca626-1156">-Řazení rank sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-1156">- 'Ranking' for rank build</span></span> <br/> <span data-ttu-id="ca626-1157">-'Fbt' FBT sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-1157">- 'Fbt' for FBT build</span></span> |
| <span data-ttu-id="ca626-1158">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1158">apiVersion</span></span> |<span data-ttu-id="ca626-1159">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1159">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-1160">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-1160">Request Body</span></span> |<span data-ttu-id="ca626-1161">Pokud je ponecháno prázdné pak hello sestavení bude proveden hello výchozí sestavení parametry.</span><span class="sxs-lookup"><span data-stu-id="ca626-1161">If left empty then hello build will be executed with hello default build parameters.</span></span><br><br><span data-ttu-id="ca626-1162">Pokud chcete tooset sestavení parametry, odešlete je ve formátu XML do textu hello jako v hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="ca626-1162">If you want tooset build parameters, send them as XML into hello body like in hello following sample.</span></span> <span data-ttu-id="ca626-1163">(Viz část hello "sestavení parametry" vysvětlení a úplný seznam parametrů hello.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>`</span><span class="sxs-lookup"><span data-stu-id="ca626-1163">(See hello "Build parameters" section for an explanation and full list of hello parameters.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>`</span></span> |

<span data-ttu-id="ca626-1164">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-1164">**Response**:</span></span>

<span data-ttu-id="ca626-1165">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1165">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1166">Toto je asynchronní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ca626-1166">This is an asynchronous API.</span></span> <span data-ttu-id="ca626-1167">Zobrazí se ID sestavení jako odpověď.</span><span class="sxs-lookup"><span data-stu-id="ca626-1167">You will get a build ID as a response.</span></span> <span data-ttu-id="ca626-1168">tooknow při sestavení hello skončila, měli byste volání rozhraní API "Získat sestavení stavu systému Model" hello a vyhledejte ID toto sestavení v odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1168">tooknow when hello build has ended, you should call hello “Get Builds Status of a Model” API and locate this build ID in hello response.</span></span> <span data-ttu-id="ca626-1169">Všimněte si, že sestavení může trvat od toohours minut v závislosti na velikosti hello dat hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1169">Note that a build can take from minutes toohours depending on hello size of hello data.</span></span>

<span data-ttu-id="ca626-1170">Doporučení nelze používat, dokud hello sestavení elementy end.</span><span class="sxs-lookup"><span data-stu-id="ca626-1170">You cannot consume recommendations till hello build ends.</span></span>

<span data-ttu-id="ca626-1171">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="ca626-1171">Valid build status:</span></span>

* <span data-ttu-id="ca626-1172">Vytvoření – Model byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="ca626-1172">Create - Model was created.</span></span>
* <span data-ttu-id="ca626-1173">Ve frontě - sestavení modelu byla spuštěna a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="ca626-1173">Queued - Model build was triggered and it is queued.</span></span>
* <span data-ttu-id="ca626-1174">Sestavení – Model sestavuje.</span><span class="sxs-lookup"><span data-stu-id="ca626-1174">Building - Model is being built.</span></span>
* <span data-ttu-id="ca626-1175">Úspěch - sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="ca626-1175">Success - Build ended successfully.</span></span>
* <span data-ttu-id="ca626-1176">Chyba: sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="ca626-1176">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="ca626-1177">Zrušena - sestavení byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="ca626-1177">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="ca626-1178">Zrušení - sestavení se ruší.</span><span class="sxs-lookup"><span data-stu-id="ca626-1178">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="ca626-1179">Všimněte si, že hello sestavení, který ID naleznete v části hello následující cesty:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="ca626-1179">Note that hello build ID can be found under hello following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="ca626-1180">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-1180">OData XML</span></span>

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




### <a name="114-get-builds-status-of-a-model"></a><span data-ttu-id="ca626-1181">11.4.</span><span class="sxs-lookup"><span data-stu-id="ca626-1181">11.4.</span></span> <span data-ttu-id="ca626-1182">Získat stav sestavení modelu</span><span class="sxs-lookup"><span data-stu-id="ca626-1182">Get Builds Status of a Model</span></span>
<span data-ttu-id="ca626-1183">Načte sestavení a jejich stavu pro zadaný model.</span><span class="sxs-lookup"><span data-stu-id="ca626-1183">Retrieves builds and their status for a specified model.</span></span>

| <span data-ttu-id="ca626-1184">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1184">HTTP Method</span></span> | <span data-ttu-id="ca626-1185">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1185">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1186">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1186">GET</span></span> |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1187">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1187">Example:</span></span><br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1188">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1188">Parameter Name</span></span> | <span data-ttu-id="ca626-1189">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1189">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1190">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1190">modelId</span></span> |<span data-ttu-id="ca626-1191">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1191">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1192">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="ca626-1192">onlyLastBuild</span></span> |<span data-ttu-id="ca626-1193">Určuje, zda všechny hello tooreturn sestavit historie hello modelu nebo pouze stav hello hello poslední sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-1193">Indicates whether tooreturn all hello build history of hello model or only hello status of hello most recent build</span></span> |
| <span data-ttu-id="ca626-1194">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1194">apiVersion</span></span> |<span data-ttu-id="ca626-1195">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1195">1.0</span></span> |

<span data-ttu-id="ca626-1196">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-1196">**Response**:</span></span>

<span data-ttu-id="ca626-1197">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1197">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1198">Hello odpověď obsahuje jeden záznam za sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1198">hello response includes one entry per build.</span></span> <span data-ttu-id="ca626-1199">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1199">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1200">`feed/entry/content/properties/UserName`-Název hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-1200">`feed/entry/content/properties/UserName` - Name of hello user.</span></span>
* <span data-ttu-id="ca626-1201">`feed/entry/content/properties/ModelName`-Název modelu hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1201">`feed/entry/content/properties/ModelName` - Name of hello model.</span></span>
* <span data-ttu-id="ca626-1202">`feed/entry/content/properties/ModelId`-Model jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ca626-1202">`feed/entry/content/properties/ModelId` - Model unique identifier.</span></span>
* <span data-ttu-id="ca626-1203">`feed/entry/content/properties/IsDeployed`– Jestli hello sestavení nasazeno do (také známa jako</span><span class="sxs-lookup"><span data-stu-id="ca626-1203">`feed/entry/content/properties/IsDeployed` - Whether hello build is deployed (a.k.a.</span></span> <span data-ttu-id="ca626-1204">aktivní sestavení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1204">active build).</span></span>
* <span data-ttu-id="ca626-1205">`feed/entry/content/properties/BuildId`-Vytvořte jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ca626-1205">`feed/entry/content/properties/BuildId` - Build unique identifier.</span></span>
* <span data-ttu-id="ca626-1206">`feed/entry/content/properties/BuildType`-Typ hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1206">`feed/entry/content/properties/BuildType` - Type of hello build.</span></span>
* <span data-ttu-id="ca626-1207">`feed/entry/content/properties/Status`-Stav sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1207">`feed/entry/content/properties/Status` - Build status.</span></span> <span data-ttu-id="ca626-1208">Může být jedna z následujících hello: Chyba, sestavování, zařazeno do fronty, Cancelling, zrušeno, úspěch.</span><span class="sxs-lookup"><span data-stu-id="ca626-1208">Can be one of hello following: Error, Building, Queued, Cancelling, Cancelled, Success.</span></span>
* <span data-ttu-id="ca626-1209">`feed/entry/content/properties/StatusMessage`-Podrobné stavové zprávy (platí pouze toospecific stavy).</span><span class="sxs-lookup"><span data-stu-id="ca626-1209">`feed/entry/content/properties/StatusMessage` - Detailed status message (applies only toospecific states).</span></span>
* <span data-ttu-id="ca626-1210">`feed/entry/content/properties/Progress`-Vytvořte průběh (%).</span><span class="sxs-lookup"><span data-stu-id="ca626-1210">`feed/entry/content/properties/Progress` - Build progress (%).</span></span>
* <span data-ttu-id="ca626-1211">`feed/entry/content/properties/StartTime`-Čas spuštění sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1211">`feed/entry/content/properties/StartTime` - Build start time.</span></span>
* <span data-ttu-id="ca626-1212">`feed/entry/content/properties/EndTime`-Vytvořte koncový čas.</span><span class="sxs-lookup"><span data-stu-id="ca626-1212">`feed/entry/content/properties/EndTime` - Build end time.</span></span>
* <span data-ttu-id="ca626-1213">`feed/entry/content/properties/ExecutionTime`-Vytvořte doba trvání.</span><span class="sxs-lookup"><span data-stu-id="ca626-1213">`feed/entry/content/properties/ExecutionTime` - Build duration.</span></span>
* <span data-ttu-id="ca626-1214">`feed/entry/content/properties/ProgressStep`-Podrobnosti o hello aktuální fáze sestavení v průběhu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1214">`feed/entry/content/properties/ProgressStep` - Details about hello current stage of a build in progress.</span></span>

<span data-ttu-id="ca626-1215">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="ca626-1215">Valid build status:</span></span>

* <span data-ttu-id="ca626-1216">Vytvořit - sestavení požadavek položka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="ca626-1216">Created - Build request entry was created.</span></span>
* <span data-ttu-id="ca626-1217">Ve frontě - sestavení požadavku byla spuštěna a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="ca626-1217">Queued - Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="ca626-1218">Probíhá vytváření - sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1218">Building - Build is in process.</span></span>
* <span data-ttu-id="ca626-1219">Úspěch - sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="ca626-1219">Success - Build ended successfully.</span></span>
* <span data-ttu-id="ca626-1220">Chyba: sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="ca626-1220">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="ca626-1221">Zrušena - sestavení byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="ca626-1221">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="ca626-1222">Zrušení - sestavení se ruší.</span><span class="sxs-lookup"><span data-stu-id="ca626-1222">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="ca626-1223">Platné hodnoty pro typ sestavení:</span><span class="sxs-lookup"><span data-stu-id="ca626-1223">Valid values for build type:</span></span>

* <span data-ttu-id="ca626-1224">Pořadí - pořadí sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1224">Rank - Rank build.</span></span>
* <span data-ttu-id="ca626-1225">Doporučení - doporučení sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1225">Recommendation - Recommendation build.</span></span>

<span data-ttu-id="ca626-1226">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-1226">OData XML</span></span>

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


### <a name="115-get-builds-status"></a><span data-ttu-id="ca626-1227">11.5.</span><span class="sxs-lookup"><span data-stu-id="ca626-1227">11.5.</span></span> <span data-ttu-id="ca626-1228">Získat stav sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-1228">Get Builds Status</span></span>
<span data-ttu-id="ca626-1229">Načte sestavení stavy všech modelů uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-1229">Retrieves build statuses of all models of a user.</span></span>

| <span data-ttu-id="ca626-1230">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1230">HTTP Method</span></span> | <span data-ttu-id="ca626-1231">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1231">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1232">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1232">GET</span></span> |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1233">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1233">Example:</span></span><br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1234">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1234">Parameter Name</span></span> | <span data-ttu-id="ca626-1235">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1235">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1236">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="ca626-1236">onlyLastBuild</span></span> |<span data-ttu-id="ca626-1237">Určuje, zda všechny hello tooreturn sestavit historie hello modelu nebo pouze stav hello hello poslední sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1237">Indicates whether tooreturn all hello build history of hello model or only hello status of hello most recent build.</span></span> |
| <span data-ttu-id="ca626-1238">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1238">apiVersion</span></span> |<span data-ttu-id="ca626-1239">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1239">1.0</span></span> |

<span data-ttu-id="ca626-1240">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-1240">**Response**:</span></span>

<span data-ttu-id="ca626-1241">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1241">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1242">Hello odpověď obsahuje jeden záznam za sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1242">hello response includes one entry per build.</span></span> <span data-ttu-id="ca626-1243">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1243">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1244">`feed/entry/content/properties/UserName`-Název hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-1244">`feed/entry/content/properties/UserName` - Name of hello user.</span></span>
* <span data-ttu-id="ca626-1245">`feed/entry/content/properties/ModelName`-Název modelu hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1245">`feed/entry/content/properties/ModelName` - Name of hello model.</span></span>
* <span data-ttu-id="ca626-1246">`feed/entry/content/properties/ModelId`-Model jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ca626-1246">`feed/entry/content/properties/ModelId` - Model unique identifier.</span></span>
* <span data-ttu-id="ca626-1247">`feed/entry/content/properties/IsDeployed`– Jestli hello sestavení nasazeno.</span><span class="sxs-lookup"><span data-stu-id="ca626-1247">`feed/entry/content/properties/IsDeployed` - Whether hello build is deployed.</span></span>
* <span data-ttu-id="ca626-1248">`feed/entry/content/properties/BuildId`-Vytvořte jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ca626-1248">`feed/entry/content/properties/BuildId` - Build unique identifier.</span></span>
* <span data-ttu-id="ca626-1249">`feed/entry/content/properties/BuildType`-Typ hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1249">`feed/entry/content/properties/BuildType` - Type of hello build.</span></span>
* <span data-ttu-id="ca626-1250">`feed/entry/content/properties/Status`-Stav sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1250">`feed/entry/content/properties/Status` - Build status.</span></span> <span data-ttu-id="ca626-1251">Může být jedna z následujících hello: Chyba, sestavování, zařazeno do fronty, zrušeno, Cancelling, úspěch.</span><span class="sxs-lookup"><span data-stu-id="ca626-1251">Can be one of hello following: Error, Building, Queued, Cancelled, Cancelling, Success.</span></span>
* <span data-ttu-id="ca626-1252">`feed/entry/content/properties/StatusMessage`-Podrobné stavové zprávy (platí pouze toospecific stavy).</span><span class="sxs-lookup"><span data-stu-id="ca626-1252">`feed/entry/content/properties/StatusMessage` - Detailed status message (applies only toospecific states).</span></span>
* <span data-ttu-id="ca626-1253">`feed/entry/content/properties/Progress`-Vytvořte průběh (%).</span><span class="sxs-lookup"><span data-stu-id="ca626-1253">`feed/entry/content/properties/Progress` - Build progress (%).</span></span>
* <span data-ttu-id="ca626-1254">`feed/entry/content/properties/StartTime`-Čas spuštění sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1254">`feed/entry/content/properties/StartTime` - Build start time.</span></span>
* <span data-ttu-id="ca626-1255">`feed/entry/content/properties/EndTime`-Vytvořte koncový čas.</span><span class="sxs-lookup"><span data-stu-id="ca626-1255">`feed/entry/content/properties/EndTime` - Build end time.</span></span>
* <span data-ttu-id="ca626-1256">`feed/entry/content/properties/ExecutionTime`-Vytvořte doba trvání.</span><span class="sxs-lookup"><span data-stu-id="ca626-1256">`feed/entry/content/properties/ExecutionTime` - Build duration.</span></span>
* <span data-ttu-id="ca626-1257">`feed/entry/content/properties/ProgressStep`-Podrobnosti o hello aktuální fáze sestavení v průběhu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1257">`feed/entry/content/properties/ProgressStep` - Details about hello current stage of a build in progress.</span></span>

<span data-ttu-id="ca626-1258">Stav platný sestavení:</span><span class="sxs-lookup"><span data-stu-id="ca626-1258">Valid build status:</span></span>

* <span data-ttu-id="ca626-1259">Vytvořit - sestavení požadavek položka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="ca626-1259">Created - Build request entry was created.</span></span>
* <span data-ttu-id="ca626-1260">Ve frontě - sestavení požadavku byla spuštěna a je ve frontě.</span><span class="sxs-lookup"><span data-stu-id="ca626-1260">Queued - Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="ca626-1261">Probíhá vytváření - sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1261">Building - Build is in process.</span></span>
* <span data-ttu-id="ca626-1262">Úspěch - sestavení skončila úspěšně.</span><span class="sxs-lookup"><span data-stu-id="ca626-1262">Success - Build ended successfully.</span></span>
* <span data-ttu-id="ca626-1263">Chyba: sestavení skončila s chybou.</span><span class="sxs-lookup"><span data-stu-id="ca626-1263">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="ca626-1264">Zrušena - sestavení byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="ca626-1264">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="ca626-1265">Zrušení - sestavení se ruší.</span><span class="sxs-lookup"><span data-stu-id="ca626-1265">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="ca626-1266">Platné hodnoty pro typ sestavení:</span><span class="sxs-lookup"><span data-stu-id="ca626-1266">Valid values for build type:</span></span>

* <span data-ttu-id="ca626-1267">Pořadí - pořadí sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1267">Rank - Rank build.</span></span>
* <span data-ttu-id="ca626-1268">Doporučení - doporučení sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1268">Recommendation - Recommendation build.</span></span>

<span data-ttu-id="ca626-1269">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-1269">OData XML</span></span>

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


### <a name="116-delete-build"></a><span data-ttu-id="ca626-1270">11.6.</span><span class="sxs-lookup"><span data-stu-id="ca626-1270">11.6.</span></span> <span data-ttu-id="ca626-1271">Odstranění sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-1271">Delete Build</span></span>
<span data-ttu-id="ca626-1272">Odstraní sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1272">Deletes a build.</span></span>

<span data-ttu-id="ca626-1273">POZNÁMKA:</span><span class="sxs-lookup"><span data-stu-id="ca626-1273">NOTE:</span></span> <br><span data-ttu-id="ca626-1274">Nelze odstranit aktivní sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1274">You cannot delete an active build.</span></span> <span data-ttu-id="ca626-1275">Hello model by měl být před odstraněním aktualizovat tooa různých active sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1275">hello model should be updated tooa different active build before you delete it.</span></span><br><span data-ttu-id="ca626-1276">Nelze odstranit v průběhu sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1276">You cannot delete an in-progress build.</span></span> <span data-ttu-id="ca626-1277">Sestavení hello měli nejprve zrušit voláním <strong>zrušit sestavení</strong>.</span><span class="sxs-lookup"><span data-stu-id="ca626-1277">You should cancel hello build first by calling <strong>Cancel Build</strong>.</span></span>

| <span data-ttu-id="ca626-1278">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1278">HTTP Method</span></span> | <span data-ttu-id="ca626-1279">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1279">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1280">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="ca626-1280">DELETE</span></span> |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1281">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1281">Example:</span></span><br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1282">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1282">Parameter Name</span></span> | <span data-ttu-id="ca626-1283">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1283">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1284">buildId</span><span class="sxs-lookup"><span data-stu-id="ca626-1284">buildId</span></span> |<span data-ttu-id="ca626-1285">Jedinečný identifikátor hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1285">Unique identifier of hello build.</span></span> |
| <span data-ttu-id="ca626-1286">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1286">apiVersion</span></span> |<span data-ttu-id="ca626-1287">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1287">1.0</span></span> |

<span data-ttu-id="ca626-1288">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1288">**Response:**</span></span>

<span data-ttu-id="ca626-1289">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1289">HTTP Status code: 200</span></span>

### <a name="117-cancel-build"></a><span data-ttu-id="ca626-1290">11.7.</span><span class="sxs-lookup"><span data-stu-id="ca626-1290">11.7.</span></span> <span data-ttu-id="ca626-1291">Zrušit sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-1291">Cancel Build</span></span>
<span data-ttu-id="ca626-1292">Zruší sestavení se při vytváření stavu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1292">Cancels a build that is in building status.</span></span>

| <span data-ttu-id="ca626-1293">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1293">HTTP Method</span></span> | <span data-ttu-id="ca626-1294">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1294">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1295">PUT</span><span class="sxs-lookup"><span data-stu-id="ca626-1295">PUT</span></span> |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1296">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1296">Example:</span></span><br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1297">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1297">Parameter Name</span></span> | <span data-ttu-id="ca626-1298">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1298">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1299">buildId</span><span class="sxs-lookup"><span data-stu-id="ca626-1299">buildId</span></span> |<span data-ttu-id="ca626-1300">Jedinečný identifikátor hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1300">Unique identifier of hello build.</span></span> |
| <span data-ttu-id="ca626-1301">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1301">apiVersion</span></span> |<span data-ttu-id="ca626-1302">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1302">1.0</span></span> |

<span data-ttu-id="ca626-1303">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1303">**Response:**</span></span>

<span data-ttu-id="ca626-1304">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1304">HTTP Status code: 200</span></span>

### <a name="118-get-build-parameters"></a><span data-ttu-id="ca626-1305">11.8.</span><span class="sxs-lookup"><span data-stu-id="ca626-1305">11.8.</span></span> <span data-ttu-id="ca626-1306">Získávání parametrů sestavení</span><span class="sxs-lookup"><span data-stu-id="ca626-1306">Get Build Parameters</span></span>
<span data-ttu-id="ca626-1307">Načte sestavení parametry.</span><span class="sxs-lookup"><span data-stu-id="ca626-1307">Retrieves build parameters.</span></span>

| <span data-ttu-id="ca626-1308">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1308">HTTP Method</span></span> | <span data-ttu-id="ca626-1309">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1309">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1310">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1310">GET</span></span> |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1311">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1311">Example:</span></span><br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1312">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1312">Parameter Name</span></span> | <span data-ttu-id="ca626-1313">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1313">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1314">buildId</span><span class="sxs-lookup"><span data-stu-id="ca626-1314">buildId</span></span> |<span data-ttu-id="ca626-1315">Jedinečný identifikátor hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1315">Unique identifier of hello build.</span></span> |
| <span data-ttu-id="ca626-1316">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1316">apiVersion</span></span> |<span data-ttu-id="ca626-1317">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1317">1.0</span></span> |

<span data-ttu-id="ca626-1318">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1318">**Response:**</span></span>

<span data-ttu-id="ca626-1319">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1319">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1320">Toto rozhraní API vrátí kolekci elementů klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="ca626-1320">This API returns a collection of key/value elements.</span></span> <span data-ttu-id="ca626-1321">Každý prvek představuje parametr a její hodnotu:</span><span class="sxs-lookup"><span data-stu-id="ca626-1321">Each element represents a parameter and its value:</span></span>

* <span data-ttu-id="ca626-1322">`feed/entry/content/properties/Key`-Vytvořte název parametru.</span><span class="sxs-lookup"><span data-stu-id="ca626-1322">`feed/entry/content/properties/Key` - Build parameter name.</span></span>
* <span data-ttu-id="ca626-1323">`feed/entry/content/properties/Value`-Vytvořte hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="ca626-1323">`feed/entry/content/properties/Value` - Build parameter value.</span></span>

<span data-ttu-id="ca626-1324">Následující tabulka Hello znázorňuje hello hodnotu, která představuje každý klíč.</span><span class="sxs-lookup"><span data-stu-id="ca626-1324">hello table below depicts hello value that each key represents.</span></span>

| <span data-ttu-id="ca626-1325">Klíč</span><span class="sxs-lookup"><span data-stu-id="ca626-1325">Key</span></span> | <span data-ttu-id="ca626-1326">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-1326">Description</span></span> | <span data-ttu-id="ca626-1327">Typ</span><span class="sxs-lookup"><span data-stu-id="ca626-1327">Type</span></span> | <span data-ttu-id="ca626-1328">Platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1328">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ca626-1329">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="ca626-1329">NumberOfModelIterations</span></span> |<span data-ttu-id="ca626-1330">Hello počet iterací provede hello modelu se odráží hello celkové výpočetní čas a hello přesnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1330">hello number of iterations hello model performs is reflected by hello overall compute time and hello model accuracy.</span></span> <span data-ttu-id="ca626-1331">vyšší číslo hello Hello, hello lepší přesnosti, které budete mít, ale hello výpočetní dobu bude trvat déle.</span><span class="sxs-lookup"><span data-stu-id="ca626-1331">hello higher hello number, hello better accuracy you will get, but hello compute time will take longer.</span></span> |<span data-ttu-id="ca626-1332">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1332">Integer</span></span> |<span data-ttu-id="ca626-1333">10-50</span><span class="sxs-lookup"><span data-stu-id="ca626-1333">10-50</span></span> |
| <span data-ttu-id="ca626-1334">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="ca626-1334">NumberOfModelDimensions</span></span> |<span data-ttu-id="ca626-1335">Hello počet dimenzí platí, že toohello počet "funkce" hello modelu se pokusí toofind v rámci vaše data.</span><span class="sxs-lookup"><span data-stu-id="ca626-1335">hello number of dimensions relates toohello number of 'features' hello model will try toofind within your data.</span></span> <span data-ttu-id="ca626-1336">Zvýšením počtu hello dimenzí vám umožní lepší doladění hello výsledky do menší clusterů.</span><span class="sxs-lookup"><span data-stu-id="ca626-1336">Increasing hello number of dimensions will allow better fine-tuning of hello results into smaller clusters.</span></span> <span data-ttu-id="ca626-1337">Však příliš mnoho dimenze zabrání hello modelu nalézt korelací mezi položkami.</span><span class="sxs-lookup"><span data-stu-id="ca626-1337">However, too many dimensions will prevent hello model from finding correlations between items.</span></span> |<span data-ttu-id="ca626-1338">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1338">Integer</span></span> |<span data-ttu-id="ca626-1339">10-40</span><span class="sxs-lookup"><span data-stu-id="ca626-1339">10-40</span></span> |
| <span data-ttu-id="ca626-1340">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="ca626-1340">ItemCutOffLowerBound</span></span> |<span data-ttu-id="ca626-1341">Definuje hello položky dolní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1341">Defines hello item lower bound for hello condenser.</span></span> <span data-ttu-id="ca626-1342">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-1342">See usage condenser above.</span></span> |<span data-ttu-id="ca626-1343">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1343">Integer</span></span> |<span data-ttu-id="ca626-1344">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-1344">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-1345">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="ca626-1345">ItemCutOffUpperBound</span></span> |<span data-ttu-id="ca626-1346">Definuje hello položky horní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1346">Defines hello item upper bound for hello condenser.</span></span> <span data-ttu-id="ca626-1347">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-1347">See usage condenser above.</span></span> |<span data-ttu-id="ca626-1348">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1348">Integer</span></span> |<span data-ttu-id="ca626-1349">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-1349">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-1350">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="ca626-1350">UserCutOffLowerBound</span></span> |<span data-ttu-id="ca626-1351">Definuje hello uživatele dolní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1351">Defines hello user lower bound for hello condenser.</span></span> <span data-ttu-id="ca626-1352">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-1352">See usage condenser above.</span></span> |<span data-ttu-id="ca626-1353">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1353">Integer</span></span> |<span data-ttu-id="ca626-1354">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-1354">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-1355">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="ca626-1355">UserCutOffUpperBound</span></span> |<span data-ttu-id="ca626-1356">Definuje hello uživatele horní mez pro chladič hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1356">Defines hello user upper bound for hello condenser.</span></span> <span data-ttu-id="ca626-1357">V tématu Použití chladič výše.</span><span class="sxs-lookup"><span data-stu-id="ca626-1357">See usage condenser above.</span></span> |<span data-ttu-id="ca626-1358">Integer</span><span class="sxs-lookup"><span data-stu-id="ca626-1358">Integer</span></span> |<span data-ttu-id="ca626-1359">2 nebo více (0 zakázat chladič)</span><span class="sxs-lookup"><span data-stu-id="ca626-1359">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="ca626-1360">Popis</span><span class="sxs-lookup"><span data-stu-id="ca626-1360">Description</span></span> |<span data-ttu-id="ca626-1361">Vytvořte popis.</span><span class="sxs-lookup"><span data-stu-id="ca626-1361">Build description.</span></span> |<span data-ttu-id="ca626-1362">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ca626-1362">String</span></span> |<span data-ttu-id="ca626-1363">Jakýkoli text, maximální 512 znaků</span><span class="sxs-lookup"><span data-stu-id="ca626-1363">Any text, maximum 512 chars</span></span> |
| <span data-ttu-id="ca626-1364">EnableModelingInsights</span><span class="sxs-lookup"><span data-stu-id="ca626-1364">EnableModelingInsights</span></span> |<span data-ttu-id="ca626-1365">Umožňuje toocompute metriky na hello doporučení modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1365">Allows you toocompute metrics on hello recommendation model.</span></span> |<span data-ttu-id="ca626-1366">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ca626-1366">Boolean</span></span> |<span data-ttu-id="ca626-1367">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="ca626-1367">True/False</span></span> |
| <span data-ttu-id="ca626-1368">UseFeaturesInModel</span><span class="sxs-lookup"><span data-stu-id="ca626-1368">UseFeaturesInModel</span></span> |<span data-ttu-id="ca626-1369">Označuje, pokud funkce lze použít v pořadí tooenhance hello doporučení modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1369">Indicates if features can be used in order tooenhance hello recommendation model.</span></span> |<span data-ttu-id="ca626-1370">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ca626-1370">Boolean</span></span> |<span data-ttu-id="ca626-1371">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="ca626-1371">True/False</span></span> |
| <span data-ttu-id="ca626-1372">ModelingFeatureList</span><span class="sxs-lookup"><span data-stu-id="ca626-1372">ModelingFeatureList</span></span> |<span data-ttu-id="ca626-1373">Čárkami oddělený seznam toobe názvy funkce použité v sestavení hello doporučení, v pořadí tooenhance hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1373">Comma-separated list of feature names toobe used in hello recommendation build, in order tooenhance hello recommendation.</span></span> |<span data-ttu-id="ca626-1374">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ca626-1374">String</span></span> |<span data-ttu-id="ca626-1375">Názvy funkcí, až too512 znaků</span><span class="sxs-lookup"><span data-stu-id="ca626-1375">Feature names, up too512 chars</span></span> |
| <span data-ttu-id="ca626-1376">AllowColdItemPlacement</span><span class="sxs-lookup"><span data-stu-id="ca626-1376">AllowColdItemPlacement</span></span> |<span data-ttu-id="ca626-1377">Označuje, pokud hello doporučení by také push cold položky prostřednictvím podobností funkci.</span><span class="sxs-lookup"><span data-stu-id="ca626-1377">Indicates if hello recommendation should also push cold items via feature similarity.</span></span> |<span data-ttu-id="ca626-1378">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ca626-1378">Boolean</span></span> |<span data-ttu-id="ca626-1379">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="ca626-1379">True/False</span></span> |
| <span data-ttu-id="ca626-1380">EnableFeatureCorrelation</span><span class="sxs-lookup"><span data-stu-id="ca626-1380">EnableFeatureCorrelation</span></span> |<span data-ttu-id="ca626-1381">Určuje funkce lze nastavit v reasoning.</span><span class="sxs-lookup"><span data-stu-id="ca626-1381">Indicates if features can be used in reasoning.</span></span> |<span data-ttu-id="ca626-1382">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ca626-1382">Boolean</span></span> |<span data-ttu-id="ca626-1383">Hodnotu true nebo False</span><span class="sxs-lookup"><span data-stu-id="ca626-1383">True/False</span></span> |
| <span data-ttu-id="ca626-1384">ReasoningFeatureList</span><span class="sxs-lookup"><span data-stu-id="ca626-1384">ReasoningFeatureList</span></span> |<span data-ttu-id="ca626-1385">Textový soubor s oddělovači seznam toobe názvy funkcí pro odůvodnění věty (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1385">Comma-separated list of feature names toobe used for reasoning sentences (e.g. recommendation explanations).</span></span> |<span data-ttu-id="ca626-1386">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ca626-1386">String</span></span> |<span data-ttu-id="ca626-1387">Názvy funkcí, až too512 znaků</span><span class="sxs-lookup"><span data-stu-id="ca626-1387">Feature names, up too512 chars</span></span> |

<span data-ttu-id="ca626-1388">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-1388">OData XML</span></span>

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

## <a name="12-recommendation"></a><span data-ttu-id="ca626-1389">12. Doporučení</span><span class="sxs-lookup"><span data-stu-id="ca626-1389">12. Recommendation</span></span>
### <a name="121-get-item-recommendations-for-active-build"></a><span data-ttu-id="ca626-1390">12.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-1390">12.1.</span></span> <span data-ttu-id="ca626-1391">Získání položky doporučení (pro aktivní sestavení)</span><span class="sxs-lookup"><span data-stu-id="ca626-1391">Get Item Recommendations (for active build)</span></span>
<span data-ttu-id="ca626-1392">Získejte doporučení hello active sestavení typu "Doporučení" nebo "Fbt" založené na seznam položek semen (vstup).</span><span class="sxs-lookup"><span data-stu-id="ca626-1392">Get recommendations of hello active build of type "Recommendation" or "Fbt" based on a list of seeds (input) items.</span></span>

| <span data-ttu-id="ca626-1393">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1393">HTTP Method</span></span> | <span data-ttu-id="ca626-1394">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1394">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1395">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1395">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1396">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1396">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1397">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1397">Parameter Name</span></span> | <span data-ttu-id="ca626-1398">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1398">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1399">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1399">modelId</span></span> |<span data-ttu-id="ca626-1400">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1400">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1401">položky ItemID</span><span class="sxs-lookup"><span data-stu-id="ca626-1401">itemIds</span></span> |<span data-ttu-id="ca626-1402">Textový soubor s oddělovači seznam hello položky toorecommend pro.</span><span class="sxs-lookup"><span data-stu-id="ca626-1402">Comma-separated list of hello items toorecommend for.</span></span> <br><span data-ttu-id="ca626-1403">Pokud je aktivní sestavení hello zadejte FBT pak můžete odeslat pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="ca626-1403">If hello active build is of type FBT then you can send only one item.</span></span> <br><span data-ttu-id="ca626-1404">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="ca626-1404">Max length: 1024</span></span> |
| <span data-ttu-id="ca626-1405">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="ca626-1405">numberOfResults</span></span> |<span data-ttu-id="ca626-1406">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="ca626-1406">Number of required results</span></span> <br> <span data-ttu-id="ca626-1407">Maximální počet: 150</span><span class="sxs-lookup"><span data-stu-id="ca626-1407">Max: 150</span></span> |
| <span data-ttu-id="ca626-1408">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="ca626-1408">includeMetatadata</span></span> |<span data-ttu-id="ca626-1409">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="ca626-1409">Future use, always false</span></span> |
| <span data-ttu-id="ca626-1410">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1410">apiVersion</span></span> |<span data-ttu-id="ca626-1411">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1411">1.0</span></span> |

<span data-ttu-id="ca626-1412">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1412">**Response:**</span></span>

<span data-ttu-id="ca626-1413">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1413">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1414">Hello odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1414">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="ca626-1415">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1415">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1416">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1416">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="ca626-1417">`Feed\entry\content\properties\Name`-Název položky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1417">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="ca626-1418">`Feed\entry\content\properties\Rating`-Hodnocení hello doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="ca626-1418">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="ca626-1419">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1419">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="ca626-1420">odpověď příklad Hello níže zahrnuje 10 položek doporučené.</span><span class="sxs-lookup"><span data-stu-id="ca626-1420">hello example response below includes 10 recommended items.</span></span>

<span data-ttu-id="ca626-1421">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-1421">OData XML</span></span>

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

### <a name="122-get-item-recommendations-of-a-specific-build"></a><span data-ttu-id="ca626-1422">12.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-1422">12.2.</span></span> <span data-ttu-id="ca626-1423">Získání položky doporučení (konkrétní sestavení)</span><span class="sxs-lookup"><span data-stu-id="ca626-1423">Get Item Recommendations (of a specific build)</span></span>
<span data-ttu-id="ca626-1424">Získejte doporučení konkrétní sestavení typu "Doporučení" nebo "Fbt".</span><span class="sxs-lookup"><span data-stu-id="ca626-1424">Get recommendations of a specific build of type "Recommendation" or "Fbt".</span></span>

| <span data-ttu-id="ca626-1425">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1425">HTTP Method</span></span> | <span data-ttu-id="ca626-1426">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1426">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1427">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1427">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1428">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1428">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1429">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1429">Parameter Name</span></span> | <span data-ttu-id="ca626-1430">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1430">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1431">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1431">modelId</span></span> |<span data-ttu-id="ca626-1432">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1432">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1433">položky ItemID</span><span class="sxs-lookup"><span data-stu-id="ca626-1433">itemIds</span></span> |<span data-ttu-id="ca626-1434">Textový soubor s oddělovači seznam hello položky toorecommend pro.</span><span class="sxs-lookup"><span data-stu-id="ca626-1434">Comma-separated list of hello items toorecommend for.</span></span> <br><span data-ttu-id="ca626-1435">Pokud je aktivní sestavení hello zadejte FBT pak můžete odeslat pouze jednu položku.</span><span class="sxs-lookup"><span data-stu-id="ca626-1435">If hello active build is of type FBT then you can send only one item.</span></span> <br><span data-ttu-id="ca626-1436">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="ca626-1436">Max length: 1024</span></span> |
| <span data-ttu-id="ca626-1437">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="ca626-1437">numberOfResults</span></span> |<span data-ttu-id="ca626-1438">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="ca626-1438">Number of required results</span></span> <br> <span data-ttu-id="ca626-1439">Maximální počet: 150</span><span class="sxs-lookup"><span data-stu-id="ca626-1439">Max: 150</span></span> |
| <span data-ttu-id="ca626-1440">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="ca626-1440">includeMetatadata</span></span> |<span data-ttu-id="ca626-1441">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="ca626-1441">Future use, always false</span></span> |
| <span data-ttu-id="ca626-1442">buildId</span><span class="sxs-lookup"><span data-stu-id="ca626-1442">buildId</span></span> |<span data-ttu-id="ca626-1443">Hello sestavení toouse id pro tento požadavek doporučení</span><span class="sxs-lookup"><span data-stu-id="ca626-1443">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="ca626-1444">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1444">apiVersion</span></span> |<span data-ttu-id="ca626-1445">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1445">1.0</span></span> |

<span data-ttu-id="ca626-1446">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1446">**Response:**</span></span>

<span data-ttu-id="ca626-1447">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1447">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1448">Hello odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1448">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="ca626-1449">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1449">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1450">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1450">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="ca626-1451">`Feed\entry\content\properties\Name`-Název položky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1451">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="ca626-1452">`Feed\entry\content\properties\Rating`-Hodnocení hello doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="ca626-1452">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="ca626-1453">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1453">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="ca626-1454">Podívejte se na příklad odpovědi v 12.1</span><span class="sxs-lookup"><span data-stu-id="ca626-1454">See a response example in 12.1</span></span>

### <a name="123-get-fbt-recommendations-for-active-build"></a><span data-ttu-id="ca626-1455">12.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-1455">12.3.</span></span> <span data-ttu-id="ca626-1456">Získejte doporučení FBT (pro aktivní sestavení)</span><span class="sxs-lookup"><span data-stu-id="ca626-1456">Get FBT Recommendations (for active build)</span></span>
<span data-ttu-id="ca626-1457">Získejte doporučení hello active sestavení typu "Fbt" v závislosti na položce počáteční hodnotu (vstup).</span><span class="sxs-lookup"><span data-stu-id="ca626-1457">Get recommendations of hello active build of type "Fbt" based on a seed (input) item.</span></span>

| <span data-ttu-id="ca626-1458">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1458">HTTP Method</span></span> | <span data-ttu-id="ca626-1459">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1459">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1460">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1460">GET</span></span> |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1461">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1461">Example:</span></span><br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1462">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1462">Parameter Name</span></span> | <span data-ttu-id="ca626-1463">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1463">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1464">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1464">modelId</span></span> |<span data-ttu-id="ca626-1465">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1465">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1466">itemId</span><span class="sxs-lookup"><span data-stu-id="ca626-1466">itemId</span></span> |<span data-ttu-id="ca626-1467">Položka toorecommend pro.</span><span class="sxs-lookup"><span data-stu-id="ca626-1467">Item toorecommend for.</span></span> <br><span data-ttu-id="ca626-1468">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="ca626-1468">Max length: 1024</span></span> |
| <span data-ttu-id="ca626-1469">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="ca626-1469">numberOfResults</span></span> |<span data-ttu-id="ca626-1470">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="ca626-1470">Number of required results</span></span> <br><span data-ttu-id="ca626-1471">Maximální počet: 150</span><span class="sxs-lookup"><span data-stu-id="ca626-1471">Max: 150</span></span> |
| <span data-ttu-id="ca626-1472">minimalScore</span><span class="sxs-lookup"><span data-stu-id="ca626-1472">minimalScore</span></span> |<span data-ttu-id="ca626-1473">Minimální skóre, které často sadu by měla mít v pořadí toobe součástí hello vrátí výsledky</span><span class="sxs-lookup"><span data-stu-id="ca626-1473">Minimal score that a frequent set should have in order toobe included in hello returned results</span></span> |
| <span data-ttu-id="ca626-1474">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="ca626-1474">includeMetatadata</span></span> |<span data-ttu-id="ca626-1475">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="ca626-1475">Future use, always false</span></span> |
| <span data-ttu-id="ca626-1476">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1476">apiVersion</span></span> |<span data-ttu-id="ca626-1477">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1477">1.0</span></span> |

<span data-ttu-id="ca626-1478">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1478">**Response:**</span></span>

<span data-ttu-id="ca626-1479">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1479">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1480">Hello odpověď obsahuje jeden záznam za sadu Doporučené položky (sady položek, které jsou obvykle koupili společně s položka počáteční hodnoty nebo vstupních hello).</span><span class="sxs-lookup"><span data-stu-id="ca626-1480">hello response includes one entry per recommended item set (a set of items which are usually bought together with hello seed/input item).</span></span> <span data-ttu-id="ca626-1481">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1481">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1482">`Feed\entry\content\properties\Id1`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1482">`Feed\entry\content\properties\Id1` - Recommended item ID.</span></span>
* <span data-ttu-id="ca626-1483">`Feed\entry\content\properties\Name1`-Název položky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1483">`Feed\entry\content\properties\Name1` - Name of hello item.</span></span>
* <span data-ttu-id="ca626-1484">`Feed\entry\content\properties\Id2`ID položky doporučené 2. (volitelné).</span><span class="sxs-lookup"><span data-stu-id="ca626-1484">`Feed\entry\content\properties\Id2` - 2nd recommended item ID (optional).</span></span>
* <span data-ttu-id="ca626-1485">`Feed\entry\content\properties\Name2`-Název hello 2. položka (volitelné).</span><span class="sxs-lookup"><span data-stu-id="ca626-1485">`Feed\entry\content\properties\Name2` - Name of hello 2nd item (optional).</span></span>
* <span data-ttu-id="ca626-1486">`Feed\entry\content\properties\Rating`-Hodnocení hello doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="ca626-1486">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="ca626-1487">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1487">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="ca626-1488">odpověď příklad Hello níže obsahuje 3 sad Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1488">hello example response below includes 3 recommended item sets.</span></span>

<span data-ttu-id="ca626-1489">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-1489">OData XML</span></span>

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

### <a name="124-get-fbt-recommendations-of-a-specific-build"></a><span data-ttu-id="ca626-1490">12.4.</span><span class="sxs-lookup"><span data-stu-id="ca626-1490">12.4.</span></span> <span data-ttu-id="ca626-1491">Získejte doporučení FBT (z konkrétní sestavení)</span><span class="sxs-lookup"><span data-stu-id="ca626-1491">Get FBT Recommendations (of a specific build)</span></span>
<span data-ttu-id="ca626-1492">Získejte doporučení konkrétní sestavení typu "Fbt".</span><span class="sxs-lookup"><span data-stu-id="ca626-1492">Get recommendations of a specific build of type "Fbt".</span></span>

| <span data-ttu-id="ca626-1493">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1493">HTTP Method</span></span> | <span data-ttu-id="ca626-1494">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1494">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1495">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1495">GET</span></span> |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1496">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1496">Example:</span></span><br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1497">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1497">Parameter Name</span></span> | <span data-ttu-id="ca626-1498">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1498">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1499">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1499">modelId</span></span> |<span data-ttu-id="ca626-1500">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1500">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1501">itemId</span><span class="sxs-lookup"><span data-stu-id="ca626-1501">itemId</span></span> |<span data-ttu-id="ca626-1502">Položka toorecommend pro.</span><span class="sxs-lookup"><span data-stu-id="ca626-1502">Item toorecommend for.</span></span> <br><span data-ttu-id="ca626-1503">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="ca626-1503">Max length: 1024</span></span> |
| <span data-ttu-id="ca626-1504">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="ca626-1504">numberOfResults</span></span> |<span data-ttu-id="ca626-1505">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="ca626-1505">Number of required results</span></span> <br><span data-ttu-id="ca626-1506">Maximální počet: 150</span><span class="sxs-lookup"><span data-stu-id="ca626-1506">Max: 150</span></span> |
| <span data-ttu-id="ca626-1507">minimalScore</span><span class="sxs-lookup"><span data-stu-id="ca626-1507">minimalScore</span></span> |<span data-ttu-id="ca626-1508">Minimální skóre, které často sadu by měla mít v pořadí toobe součástí hello vrátí výsledky</span><span class="sxs-lookup"><span data-stu-id="ca626-1508">Minimal score that a frequent set should have in order toobe included in hello returned results</span></span> |
| <span data-ttu-id="ca626-1509">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="ca626-1509">includeMetatadata</span></span> |<span data-ttu-id="ca626-1510">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="ca626-1510">Future use, always false</span></span> |
| <span data-ttu-id="ca626-1511">buildId</span><span class="sxs-lookup"><span data-stu-id="ca626-1511">buildId</span></span> |<span data-ttu-id="ca626-1512">Hello sestavení toouse id pro tento požadavek doporučení</span><span class="sxs-lookup"><span data-stu-id="ca626-1512">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="ca626-1513">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1513">apiVersion</span></span> |<span data-ttu-id="ca626-1514">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1514">1.0</span></span> |

<span data-ttu-id="ca626-1515">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1515">**Response:**</span></span>

<span data-ttu-id="ca626-1516">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1516">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1517">Hello odpověď obsahuje jeden záznam za sadu Doporučené položky (sady položek, které jsou obvykle koupili společně s položka počáteční hodnoty nebo vstupních hello).</span><span class="sxs-lookup"><span data-stu-id="ca626-1517">hello response includes one entry per recommended item set (a set of items which are usually bought together with hello seed/input item).</span></span> <span data-ttu-id="ca626-1518">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1518">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1519">`Feed\entry\content\properties\Id1`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1519">`Feed\entry\content\properties\Id1` - Recommended item ID.</span></span>
* <span data-ttu-id="ca626-1520">`Feed\entry\content\properties\Name1`-Název položky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1520">`Feed\entry\content\properties\Name1` - Name of hello item.</span></span>
* <span data-ttu-id="ca626-1521">`Feed\entry\content\properties\Id2`ID položky doporučené 2. (volitelné).</span><span class="sxs-lookup"><span data-stu-id="ca626-1521">`Feed\entry\content\properties\Id2` - 2nd recommended item ID (optional).</span></span>
* <span data-ttu-id="ca626-1522">`Feed\entry\content\properties\Name2`-Název hello 2. položka (volitelné).</span><span class="sxs-lookup"><span data-stu-id="ca626-1522">`Feed\entry\content\properties\Name2` - Name of hello 2nd item (optional).</span></span>
* <span data-ttu-id="ca626-1523">`Feed\entry\content\properties\Rating`-Hodnocení hello doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="ca626-1523">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="ca626-1524">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1524">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="ca626-1525">Podívejte se na příklad odpovědi v 12.3</span><span class="sxs-lookup"><span data-stu-id="ca626-1525">See a response example in 12.3</span></span>

### <a name="125-get-user-recommendations-for-active-build"></a><span data-ttu-id="ca626-1526">12.5.</span><span class="sxs-lookup"><span data-stu-id="ca626-1526">12.5.</span></span> <span data-ttu-id="ca626-1527">Získejte doporučení uživatele (pro aktivní sestavení)</span><span class="sxs-lookup"><span data-stu-id="ca626-1527">Get User Recommendations (for active build)</span></span>
<span data-ttu-id="ca626-1528">Získejte doporučení uživatele sestavení typu "Doporučení" označená jako aktivní sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1528">Get user recommendations of a build of type "Recommendation" marked as active build.</span></span>

<span data-ttu-id="ca626-1529">Hello rozhraní API, vrátí se seznam předpokládaných položky podle historie využití toohello hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-1529">hello API will return a list of predicted item according toohello usage history of hello user.</span></span>

<span data-ttu-id="ca626-1530">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="ca626-1530">Notes:</span></span> 

1. <span data-ttu-id="ca626-1531">Neexistuje žádný uživatel doporučení pro FBT sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1531">There is no user recommendation for FBT build.</span></span>
2. <span data-ttu-id="ca626-1532">Pokud hello active sestavení je FBT bude tato metoda vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1532">If hello active build is FBT this method will returns an error.</span></span>

| <span data-ttu-id="ca626-1533">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1533">HTTP Method</span></span> | <span data-ttu-id="ca626-1534">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1534">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1535">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1535">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1536">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1536">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1537">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1537">Parameter Name</span></span> | <span data-ttu-id="ca626-1538">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1538">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1539">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1539">modelId</span></span> |<span data-ttu-id="ca626-1540">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1540">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1541">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="ca626-1541">userId</span></span> |<span data-ttu-id="ca626-1542">Jedinečný identifikátor uživatele hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1542">Unique identifier of hello user</span></span> |
| <span data-ttu-id="ca626-1543">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="ca626-1543">numberOfResults</span></span> |<span data-ttu-id="ca626-1544">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="ca626-1544">Number of required results</span></span> |
| <span data-ttu-id="ca626-1545">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="ca626-1545">includeMetatadata</span></span> |<span data-ttu-id="ca626-1546">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="ca626-1546">Future use, always false</span></span> |
| <span data-ttu-id="ca626-1547">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1547">apiVersion</span></span> |<span data-ttu-id="ca626-1548">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1548">1.0</span></span> |

<span data-ttu-id="ca626-1549">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1549">**Response:**</span></span>

<span data-ttu-id="ca626-1550">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1550">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1551">Hello odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1551">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="ca626-1552">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1552">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1553">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1553">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="ca626-1554">`Feed\entry\content\properties\Name`-Název položky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1554">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="ca626-1555">`Feed\entry\content\properties\Rating`-Hodnocení hello doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="ca626-1555">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="ca626-1556">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1556">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="ca626-1557">Podívejte se na příklad odpovědi v 12.1</span><span class="sxs-lookup"><span data-stu-id="ca626-1557">See a response example in 12.1</span></span>

### <a name="126-get-user-recommendations-with-item-list-for-active-build"></a><span data-ttu-id="ca626-1558">12.6.</span><span class="sxs-lookup"><span data-stu-id="ca626-1558">12.6.</span></span> <span data-ttu-id="ca626-1559">Získat doporučení uživatele s položky seznamu (pro aktivní sestavení)</span><span class="sxs-lookup"><span data-stu-id="ca626-1559">Get User Recommendations with item list (for active build)</span></span>
<span data-ttu-id="ca626-1560">Získat uživatele doporučení sestavení typu "Doporučení" označená jako aktivní sestavení s další seznam položek</span><span class="sxs-lookup"><span data-stu-id="ca626-1560">Get user recommendations of a build of type "Recommendation" marked as active build with an additional list of items</span></span>

<span data-ttu-id="ca626-1561">Hello rozhraní API, vrátí se seznam předpokládaných položky podle historie využití toohello hello uživatele a další zadané položky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1561">hello API will return a list of predicted item according toohello usage history of hello user and hello additional provided items.</span></span>

<span data-ttu-id="ca626-1562">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="ca626-1562">Notes:</span></span> 

1. <span data-ttu-id="ca626-1563">Neexistuje žádný uživatel doporučení pro FBT sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1563">There is no user recommendation for FBT build.</span></span>
2. <span data-ttu-id="ca626-1564">Pokud hello active sestavení je FBT bude tato metoda vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1564">If hello active build is FBT this method will returns an error.</span></span>

| <span data-ttu-id="ca626-1565">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1565">HTTP Method</span></span> | <span data-ttu-id="ca626-1566">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1566">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1567">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1567">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1568">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1568">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1569">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1569">Parameter Name</span></span> | <span data-ttu-id="ca626-1570">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1570">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1571">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1571">modelId</span></span> |<span data-ttu-id="ca626-1572">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1572">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1573">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="ca626-1573">userId</span></span> |<span data-ttu-id="ca626-1574">Jedinečný identifikátor uživatele hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1574">Unique identifier of hello user</span></span> |
| <span data-ttu-id="ca626-1575">itemsIds</span><span class="sxs-lookup"><span data-stu-id="ca626-1575">itemsIds</span></span> |<span data-ttu-id="ca626-1576">Textový soubor s oddělovači seznam hello položky toorecommend pro.</span><span class="sxs-lookup"><span data-stu-id="ca626-1576">Comma-separated list of hello items toorecommend for.</span></span> <span data-ttu-id="ca626-1577">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="ca626-1577">Max length: 1024</span></span> |
| <span data-ttu-id="ca626-1578">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="ca626-1578">numberOfResults</span></span> |<span data-ttu-id="ca626-1579">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="ca626-1579">Number of required results</span></span> |
| <span data-ttu-id="ca626-1580">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="ca626-1580">includeMetatadata</span></span> |<span data-ttu-id="ca626-1581">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="ca626-1581">Future use, always false</span></span> |
| <span data-ttu-id="ca626-1582">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1582">apiVersion</span></span> |<span data-ttu-id="ca626-1583">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1583">1.0</span></span> |

<span data-ttu-id="ca626-1584">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1584">**Response:**</span></span>

<span data-ttu-id="ca626-1585">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1585">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1586">Hello odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1586">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="ca626-1587">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1587">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1588">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1588">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="ca626-1589">`Feed\entry\content\properties\Name`-Název položky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1589">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="ca626-1590">`Feed\entry\content\properties\Rating`-Hodnocení hello doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="ca626-1590">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="ca626-1591">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1591">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="ca626-1592">Podívejte se na příklad odpovědi v 12.1</span><span class="sxs-lookup"><span data-stu-id="ca626-1592">See a response example in 12.1</span></span>

### <a name="127-get-user-recommendations--of-a-specific-build"></a><span data-ttu-id="ca626-1593">12.7.</span><span class="sxs-lookup"><span data-stu-id="ca626-1593">12.7.</span></span> <span data-ttu-id="ca626-1594">Získejte doporučení uživatele (z konkrétní sestavení)</span><span class="sxs-lookup"><span data-stu-id="ca626-1594">Get User Recommendations  (of a specific build)</span></span>
<span data-ttu-id="ca626-1595">Získejte doporučení uživatele konkrétní sestavení typu "Doporučení".</span><span class="sxs-lookup"><span data-stu-id="ca626-1595">Get user recommendations of a specific build of type "Recommendation".</span></span>

<span data-ttu-id="ca626-1596">Hello rozhraní API, vrátí se seznam předpokládaných položky podle historie využití toohello hello uživatele (používá se v sestavení konkrétní hello).</span><span class="sxs-lookup"><span data-stu-id="ca626-1596">hello API will return a list of predicted item according toohello usage history of hello user (used in hello specific build).</span></span>

<span data-ttu-id="ca626-1597">Poznámka: Neexistuje žádný uživatel doporučení pro FBT sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1597">Note: There is no user recommendation for FBT build.</span></span>

| <span data-ttu-id="ca626-1598">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1598">HTTP Method</span></span> | <span data-ttu-id="ca626-1599">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1599">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1600">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1600">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1601">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1601">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1602">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1602">Parameter Name</span></span> | <span data-ttu-id="ca626-1603">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1603">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1604">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1604">modelId</span></span> |<span data-ttu-id="ca626-1605">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1605">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1606">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="ca626-1606">userId</span></span> |<span data-ttu-id="ca626-1607">Jedinečný identifikátor uživatele hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1607">Unique identifier of hello user</span></span> |
| <span data-ttu-id="ca626-1608">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="ca626-1608">numberOfResults</span></span> |<span data-ttu-id="ca626-1609">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="ca626-1609">Number of required results</span></span> |
| <span data-ttu-id="ca626-1610">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="ca626-1610">includeMetatadata</span></span> |<span data-ttu-id="ca626-1611">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="ca626-1611">Future use, always false</span></span> |
| <span data-ttu-id="ca626-1612">buildId</span><span class="sxs-lookup"><span data-stu-id="ca626-1612">buildId</span></span> |<span data-ttu-id="ca626-1613">Hello sestavení toouse id pro tento požadavek doporučení</span><span class="sxs-lookup"><span data-stu-id="ca626-1613">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="ca626-1614">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1614">apiVersion</span></span> |<span data-ttu-id="ca626-1615">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1615">1.0</span></span> |

<span data-ttu-id="ca626-1616">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1616">**Response:**</span></span>

<span data-ttu-id="ca626-1617">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1617">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1618">Hello odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1618">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="ca626-1619">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1619">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1620">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1620">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="ca626-1621">`Feed\entry\content\properties\Name`-Název položky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1621">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="ca626-1622">`Feed\entry\content\properties\Rating`-Hodnocení hello doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="ca626-1622">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="ca626-1623">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1623">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="ca626-1624">Podívejte se na příklad odpovědi v 12.1</span><span class="sxs-lookup"><span data-stu-id="ca626-1624">See a response example in 12.1</span></span>

### <a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a><span data-ttu-id="ca626-1625">12.8.</span><span class="sxs-lookup"><span data-stu-id="ca626-1625">12.8.</span></span> <span data-ttu-id="ca626-1626">Získat doporučení uživatele s položky seznamu (konkrétní sestavení)</span><span class="sxs-lookup"><span data-stu-id="ca626-1626">Get User Recommendations with item list (of a specific build)</span></span>
<span data-ttu-id="ca626-1627">Získejte doporučení uživatele konkrétní sestavení typu "Doporučení" a hello seznam dalších položek.</span><span class="sxs-lookup"><span data-stu-id="ca626-1627">Get user recommendations of a specific build of type "Recommendation" and hello list of additional items.</span></span>

<span data-ttu-id="ca626-1628">Hello rozhraní API, vrátí se seznam předpokládaných položky podle historie využití toohello hello uživatele a hello další seznam položek.</span><span class="sxs-lookup"><span data-stu-id="ca626-1628">hello API will return a list of predicted item according toohello usage history of hello user and hello additional list of items.</span></span>

<span data-ttu-id="ca626-1629">Poznámka: Tthere je žádné uživatele doporučení pro FBT sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1629">Note: Tthere is no user recommendation for FBT build.</span></span>

| <span data-ttu-id="ca626-1630">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1630">HTTP Method</span></span> | <span data-ttu-id="ca626-1631">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1631">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1632">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1632">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1633">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1633">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1634">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1634">Parameter Name</span></span> | <span data-ttu-id="ca626-1635">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1635">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1636">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1636">modelId</span></span> |<span data-ttu-id="ca626-1637">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1637">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1638">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="ca626-1638">userId</span></span> |<span data-ttu-id="ca626-1639">Jedinečný identifikátor uživatele hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1639">Unique identifier of hello user</span></span> |
| <span data-ttu-id="ca626-1640">položky ItemID</span><span class="sxs-lookup"><span data-stu-id="ca626-1640">itemIds</span></span> |<span data-ttu-id="ca626-1641">Textový soubor s oddělovači seznam hello položky toorecommend pro.</span><span class="sxs-lookup"><span data-stu-id="ca626-1641">Comma-separated list of hello items toorecommend for.</span></span> <span data-ttu-id="ca626-1642">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="ca626-1642">Max length: 1024</span></span> |
| <span data-ttu-id="ca626-1643">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="ca626-1643">numberOfResults</span></span> |<span data-ttu-id="ca626-1644">Počet požadovaných výsledků</span><span class="sxs-lookup"><span data-stu-id="ca626-1644">Number of required results</span></span> |
| <span data-ttu-id="ca626-1645">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="ca626-1645">includeMetatadata</span></span> |<span data-ttu-id="ca626-1646">Budoucí použití, vždy hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="ca626-1646">Future use, always false</span></span> |
| <span data-ttu-id="ca626-1647">buildId</span><span class="sxs-lookup"><span data-stu-id="ca626-1647">buildId</span></span> |<span data-ttu-id="ca626-1648">Hello sestavení toouse id pro tento požadavek doporučení</span><span class="sxs-lookup"><span data-stu-id="ca626-1648">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="ca626-1649">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1649">apiVersion</span></span> |<span data-ttu-id="ca626-1650">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1650">1.0</span></span> |

<span data-ttu-id="ca626-1651">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1651">**Response:**</span></span>

<span data-ttu-id="ca626-1652">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1652">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1653">Hello odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1653">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="ca626-1654">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1654">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1655">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1655">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="ca626-1656">`Feed\entry\content\properties\Name`-Název položky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1656">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="ca626-1657">`Feed\entry\content\properties\Rating`-Hodnocení hello doporučení; vyšší číslo znamená vyšší spolehlivosti.</span><span class="sxs-lookup"><span data-stu-id="ca626-1657">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="ca626-1658">`Feed\entry\content\properties\Reasoning`-Doporučení důvody (např. vysvětlení doporučení).</span><span class="sxs-lookup"><span data-stu-id="ca626-1658">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="ca626-1659">Podívejte se na příklad odpovědi v 12.1</span><span class="sxs-lookup"><span data-stu-id="ca626-1659">See a response example in 12.1</span></span>

## <a name="13-user-usage-history"></a><span data-ttu-id="ca626-1660">13. Historie využití uživatele</span><span class="sxs-lookup"><span data-stu-id="ca626-1660">13. User Usage History</span></span>
<span data-ttu-id="ca626-1661">Jakmile doporučení model byl vytvořený umožní hello systém tooretrieve historie uživatelů hello (položky přidružené tooa konkrétního uživatele) používá pro sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1661">Once a recommendation model was built hello system will allow tooretrieve hello user history (items associated tooa specific user) used for hello build.</span></span>
<span data-ttu-id="ca626-1662">Toto rozhraní API povolit historie uživatelů tooretrieve hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1662">This API allow tooretrieve hello user history</span></span>

<span data-ttu-id="ca626-1663">Poznámka: historie uživatelů hello je aktuálně k dispozici pouze pro sestavení doporučení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1663">Note: hello user history is currently available only for recommendation builds.</span></span>

### <a name="131-retrieve-user-history"></a><span data-ttu-id="ca626-1664">13.1 načíst historii uživatele</span><span class="sxs-lookup"><span data-stu-id="ca626-1664">13.1 Retrieve user history</span></span>
<span data-ttu-id="ca626-1665">Načtení hello seznam položek, které jsou používány hello active sestavení nebo v hello zadané sestavení pro hello zadané id uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-1665">Retrieve hello list of item used in hello active build or in hello specified build for hello given user id.</span></span>

| <span data-ttu-id="ca626-1666">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1666">HTTP Method</span></span> | <span data-ttu-id="ca626-1667">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1667">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1668">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1668">GET</span></span> |<span data-ttu-id="ca626-1669">Zobrazit historii uživatele hello hello active sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1669">Get hello user history for hello active build.</span></span><br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/><span data-ttu-id="ca626-1670">Získání historie hello uživatelů pro hello zadané sestavení`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`</span><span class="sxs-lookup"><span data-stu-id="ca626-1670">Get hello user history for hello given build `<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`</span></span><br/><br/><span data-ttu-id="ca626-1671">Příklad:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`</span><span class="sxs-lookup"><span data-stu-id="ca626-1671">Example:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`</span></span> |

| <span data-ttu-id="ca626-1672">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1672">Parameter Name</span></span> | <span data-ttu-id="ca626-1673">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1673">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1674">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1674">modelId</span></span> |<span data-ttu-id="ca626-1675">Jedinečný identifikátor Hello hello modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1675">hello unique identifier of hello model.</span></span> |
| <span data-ttu-id="ca626-1676">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="ca626-1676">userId</span></span> |<span data-ttu-id="ca626-1677">Jedinečný identifikátor Hello hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca626-1677">hello unique identifier of hello user.</span></span> |
| <span data-ttu-id="ca626-1678">buildId</span><span class="sxs-lookup"><span data-stu-id="ca626-1678">buildId</span></span> |<span data-ttu-id="ca626-1679">Volitelný parametr, povolit tooindicate, ze které sestavení by měla být historie uživatelů hello načtení</span><span class="sxs-lookup"><span data-stu-id="ca626-1679">optional parameter, allow tooindicate from which build hello user history should be fetch</span></span> |
| <span data-ttu-id="ca626-1680">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1680">apiVersion</span></span> |<span data-ttu-id="ca626-1681">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1681">1.0</span></span> |

<span data-ttu-id="ca626-1682">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1682">**Response:**</span></span>

<span data-ttu-id="ca626-1683">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1683">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1684">Hello odpověď obsahuje jeden záznam za doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1684">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="ca626-1685">Každá položka má hello následující data:</span><span class="sxs-lookup"><span data-stu-id="ca626-1685">Each entry has hello following data:</span></span>

* <span data-ttu-id="ca626-1686">`Feed\entry\content\properties\Id`– ID Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="ca626-1686">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="ca626-1687">`Feed\entry\content\properties\Name`-Název položky hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1687">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="ca626-1688">`Feed\entry\content\properties\Rating`-NENÍ K DISPOZICI.</span><span class="sxs-lookup"><span data-stu-id="ca626-1688">`Feed\entry\content\properties\Rating` - N/A.</span></span>
* <span data-ttu-id="ca626-1689">`Feed\entry\content\properties\Reasoning`-NENÍ K DISPOZICI.</span><span class="sxs-lookup"><span data-stu-id="ca626-1689">`Feed\entry\content\properties\Reasoning` - N/A.</span></span>

<span data-ttu-id="ca626-1690">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-1690">OData XML</span></span>

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

## <a name="14-notifications"></a><span data-ttu-id="ca626-1691">14. Oznámení</span><span class="sxs-lookup"><span data-stu-id="ca626-1691">14. Notifications</span></span>
<span data-ttu-id="ca626-1692">Azure Machine Learning doporučení vytvoří oznámení, když dojde k trvalé chyby v systému hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1692">Azure Machine Learning Recommendations creates notifications when persistent errors happen in hello system.</span></span> <span data-ttu-id="ca626-1693">Existují 3 typy oznámení:</span><span class="sxs-lookup"><span data-stu-id="ca626-1693">There are 3 types of notifications:</span></span>

1. <span data-ttu-id="ca626-1694">Sestavení selhání – toto oznámení se aktivuje pro každou chybu sestavení.</span><span class="sxs-lookup"><span data-stu-id="ca626-1694">Build failure - This notification is triggered for every build failure.</span></span>
2. <span data-ttu-id="ca626-1695">Získávání dat zpracování selhání – toto oznámení se aktivuje, když jsme má více než 100 chyby ve hello posledních 5 minut v hello zpracování událostí využití za modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1695">Data acquisition processing failure - This notification is triggered when we have more than 100 errors in hello last 5 minutes in hello processing of usage events per model.</span></span>
3. <span data-ttu-id="ca626-1696">Doporučení spotřeba selhání – toto oznámení se aktivuje, když jsme má více než 100 chyby ve hello posledních 5 minut v hello zpracování požadavků doporučení za modelu.</span><span class="sxs-lookup"><span data-stu-id="ca626-1696">Recommendation consumption failure - This notification is triggered when we have more than 100 errors in hello last 5 minutes in hello processing of recommendation requests per model.</span></span>

### <a name="141-get-notifications"></a><span data-ttu-id="ca626-1697">14.1.</span><span class="sxs-lookup"><span data-stu-id="ca626-1697">14.1.</span></span> <span data-ttu-id="ca626-1698">Dostávat oznámení</span><span class="sxs-lookup"><span data-stu-id="ca626-1698">Get Notifications</span></span>
<span data-ttu-id="ca626-1699">Načte všechny hello oznámení pro všechny modely nebo pro jeden model.</span><span class="sxs-lookup"><span data-stu-id="ca626-1699">Retrieves all hello notifications for all models or for a single model.</span></span>

| <span data-ttu-id="ca626-1700">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1700">HTTP Method</span></span> | <span data-ttu-id="ca626-1701">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1701">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1702">GET</span><span class="sxs-lookup"><span data-stu-id="ca626-1702">GET</span></span> |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1703">Získávání všechna oznámení pro všechny modely:</span><span class="sxs-lookup"><span data-stu-id="ca626-1703">Getting all notifications for all models:</span></span><br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1704">Příklad pro získávání oznámení pro konkrétní model.</span><span class="sxs-lookup"><span data-stu-id="ca626-1704">Example for getting notifications for a specific model:</span></span><br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277` |

| <span data-ttu-id="ca626-1705">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1705">Parameter Name</span></span> | <span data-ttu-id="ca626-1706">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1706">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1707">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1707">modelId</span></span> |<span data-ttu-id="ca626-1708">Volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="ca626-1708">Optional parameter.</span></span> <span data-ttu-id="ca626-1709">Když tento parametr vynechán, zobrazí se všechna oznámení pro všechny modely.</span><span class="sxs-lookup"><span data-stu-id="ca626-1709">When omitted, you will get all notifications for all models.</span></span> <br><span data-ttu-id="ca626-1710">Platná hodnota: Jedinečný identifikátor modelu hello.</span><span class="sxs-lookup"><span data-stu-id="ca626-1710">Valid value: unique identifier of hello model.</span></span> |
| <span data-ttu-id="ca626-1711">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1711">apiVersion</span></span> |<span data-ttu-id="ca626-1712">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1712">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-1713">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-1713">Request Body</span></span> |<span data-ttu-id="ca626-1714">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-1714">NONE</span></span> |

<span data-ttu-id="ca626-1715">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="ca626-1715">**Response:**</span></span>

<span data-ttu-id="ca626-1716">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1716">HTTP Status code: 200</span></span>

<span data-ttu-id="ca626-1717">OData XML</span><span class="sxs-lookup"><span data-stu-id="ca626-1717">OData XML</span></span>

    hello response includes one entry per notification. Each entry has hello following data:
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

### <a name="142-delete-model-notifications"></a><span data-ttu-id="ca626-1718">14.2.</span><span class="sxs-lookup"><span data-stu-id="ca626-1718">14.2.</span></span> <span data-ttu-id="ca626-1719">Oznámení o Model odstraňovat</span><span class="sxs-lookup"><span data-stu-id="ca626-1719">Delete Model Notifications</span></span>
<span data-ttu-id="ca626-1720">Odstraní všechny čtení oznámení pro model.</span><span class="sxs-lookup"><span data-stu-id="ca626-1720">Deletes all read notifications for a model.</span></span>

| <span data-ttu-id="ca626-1721">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1721">HTTP Method</span></span> | <span data-ttu-id="ca626-1722">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1722">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1723">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="ca626-1723">DELETE</span></span> |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="ca626-1724">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ca626-1724">Example:</span></span><br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1725">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1725">Parameter Name</span></span> | <span data-ttu-id="ca626-1726">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1726">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1727">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="ca626-1727">modelId</span></span> |<span data-ttu-id="ca626-1728">Jedinečný identifikátor modelu hello</span><span class="sxs-lookup"><span data-stu-id="ca626-1728">Unique identifier of hello model</span></span> |
| <span data-ttu-id="ca626-1729">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1729">apiVersion</span></span> |<span data-ttu-id="ca626-1730">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1730">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-1731">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-1731">Request Body</span></span> |<span data-ttu-id="ca626-1732">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-1732">NONE</span></span> |

<span data-ttu-id="ca626-1733">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-1733">**Response**:</span></span>

<span data-ttu-id="ca626-1734">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1734">HTTP Status code: 200</span></span>

### <a name="143-delete-user-notifications"></a><span data-ttu-id="ca626-1735">14.3.</span><span class="sxs-lookup"><span data-stu-id="ca626-1735">14.3.</span></span> <span data-ttu-id="ca626-1736">Odstranit oznámení uživateli</span><span class="sxs-lookup"><span data-stu-id="ca626-1736">Delete User Notifications</span></span>
<span data-ttu-id="ca626-1737">Odstraní všechna oznámení pro všechny modely.</span><span class="sxs-lookup"><span data-stu-id="ca626-1737">Deletes all notifications for all models.</span></span>

| <span data-ttu-id="ca626-1738">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="ca626-1738">HTTP Method</span></span> | <span data-ttu-id="ca626-1739">IDENTIFIKÁTOR URI</span><span class="sxs-lookup"><span data-stu-id="ca626-1739">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1740">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="ca626-1740">DELETE</span></span> |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27` |

| <span data-ttu-id="ca626-1741">Název parametru</span><span class="sxs-lookup"><span data-stu-id="ca626-1741">Parameter Name</span></span> | <span data-ttu-id="ca626-1742">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="ca626-1742">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="ca626-1743">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ca626-1743">apiVersion</span></span> |<span data-ttu-id="ca626-1744">1.0</span><span class="sxs-lookup"><span data-stu-id="ca626-1744">1.0</span></span> |
|  | |
| <span data-ttu-id="ca626-1745">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="ca626-1745">Request Body</span></span> |<span data-ttu-id="ca626-1746">NONE</span><span class="sxs-lookup"><span data-stu-id="ca626-1746">NONE</span></span> |

<span data-ttu-id="ca626-1747">**Odpověď**:</span><span class="sxs-lookup"><span data-stu-id="ca626-1747">**Response**:</span></span>

<span data-ttu-id="ca626-1748">Kód stavu HTTP: 200</span><span class="sxs-lookup"><span data-stu-id="ca626-1748">HTTP Status code: 200</span></span>

## <a name="15-legal"></a><span data-ttu-id="ca626-1749">15. Právní informace</span><span class="sxs-lookup"><span data-stu-id="ca626-1749">15. Legal</span></span>
<span data-ttu-id="ca626-1750">Tento dokument je poskytován "jako-je".</span><span class="sxs-lookup"><span data-stu-id="ca626-1750">This document is provided “as-is”.</span></span> <span data-ttu-id="ca626-1751">Informace a názory vyjádřené v tomto dokumentu včetně adres URL a dalších odkazů na internetové weby, mohou změnit bez předchozího upozornění.</span><span class="sxs-lookup"><span data-stu-id="ca626-1751">Information and views expressed in this document, including URL and other Internet Web site references, may change without notice.</span></span><br><br>
<span data-ttu-id="ca626-1752">Některé příklady použité v ukázkách jsou jenom ilustrativní a smyšlené.</span><span class="sxs-lookup"><span data-stu-id="ca626-1752">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="ca626-1753">Žádný skutečný vztah nebo připojení je určený nebo událostmi.</span><span class="sxs-lookup"><span data-stu-id="ca626-1753">No real association or connection is intended or should be inferred.</span></span><br><br>
<span data-ttu-id="ca626-1754">Tento dokument vám neposkytuje žádná zákonná práva tooany týkající se duševního vlastnictví produktů společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ca626-1754">This document does not provide you with any legal rights tooany intellectual property in any Microsoft product.</span></span> <span data-ttu-id="ca626-1755">Můžete kopírovat a tento dokument použít pro interní referenční účely.</span><span class="sxs-lookup"><span data-stu-id="ca626-1755">You may copy and use this document for your internal, reference purposes.</span></span><br><br>
<span data-ttu-id="ca626-1756">© 2015 Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ca626-1756">© 2015 Microsoft.</span></span> <span data-ttu-id="ca626-1757">Všechna práva vyhrazena.</span><span class="sxs-lookup"><span data-stu-id="ca626-1757">All rights reserved.</span></span>

