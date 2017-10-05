---
title: "Upgrade na rozhraní API služby Azure Search služby REST verze 2016-09-01 | Microsoft Docs"
description: "Upgrade na rozhraní API služby Azure Search služby REST verze 2016-09-01"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 6183fa6c-48bb-4af7-adae-4be3bc43c3ed
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: brjohnst
ms.openlocfilehash: f6a189c2e314b91c490583a86d8bacca8ec78a0f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="upgrading-to-the-azure-search-service-rest-api-version-2016-09-01"></a><span data-ttu-id="93cd0-103">Upgrade na rozhraní API služby Azure Search služby REST verze 2016-09-01</span><span class="sxs-lookup"><span data-stu-id="93cd0-103">Upgrading to the Azure Search Service REST API version 2016-09-01</span></span>
<span data-ttu-id="93cd0-104">Pokud používáte verze 2015-02-28 nebo 2015-02-28 – Náhled [rozhraní REST API služby Azure Search](https://msdn.microsoft.com/library/azure/dn798935.aspx), tento článek vám pomůže při upgradu aplikace k používání všeobecně dostupná verze rozhraní API 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="93cd0-104">If you're using version 2015-02-28 or 2015-02-28-Preview of the [Azure Search Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), this article will help you upgrade your application to use the next generally available API version, 2016-09-01.</span></span>

<span data-ttu-id="93cd0-105">Verze 2016-09-01 rozhraní REST API obsahuje některé změny z předchozích verzí.</span><span class="sxs-lookup"><span data-stu-id="93cd0-105">Version 2016-09-01 of the REST API contains some changes from earlier versions.</span></span> <span data-ttu-id="93cd0-106">Toto jsou většinou zpětně kompatibilní, takže měnili kód měli vyžadovat jen minimální úsilí, v závislosti na verzi, které jste používali před.</span><span class="sxs-lookup"><span data-stu-id="93cd0-106">These are mostly backward compatible, so changing your code should require only minimal effort, depending on which version you were using before.</span></span> <span data-ttu-id="93cd0-107">V tématu [kroky pro upgrade](#UpgradeSteps) pokyny o tom, jak upravit svůj kód k použití nové verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="93cd0-107">See [Steps to upgrade](#UpgradeSteps) for instructions on how to change your code to use the new API version.</span></span>

> [!NOTE]
> <span data-ttu-id="93cd0-108">Instanci služby Azure Search podporuje několik verzí rozhraní REST API, včetně nejnovější.</span><span class="sxs-lookup"><span data-stu-id="93cd0-108">Your Azure Search service instance supports several REST API versions, including the latest one.</span></span> <span data-ttu-id="93cd0-109">Můžete použít verzi, když je už nejnovějšího, ale doporučujeme migraci kód a použít nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="93cd0-109">You can continue to use a version when it is no longer the latest one, but we recommend that you migrate your code to use the newest version.</span></span>

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a><span data-ttu-id="93cd0-110">Co je nového ve verzi 2016-09-01</span><span class="sxs-lookup"><span data-stu-id="93cd0-110">What's new in version 2016-09-01</span></span>
<span data-ttu-id="93cd0-111">Verze 2016-09-01 je druhý všeobecně dostupná verze rozhraní API REST služby vyhledávání Azure.</span><span class="sxs-lookup"><span data-stu-id="93cd0-111">Version 2016-09-01 is the second generally available release of the Azure Search Service REST API.</span></span> <span data-ttu-id="93cd0-112">Nové funkce v této verzi rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="93cd0-112">New features in this API version include:</span></span>

* <span data-ttu-id="93cd0-113">[Vlastní analyzátorů](https://aka.ms/customanalyzers), které umožňují převzít kontrolu nad tímto procesem převodu textu indexovanou a vyhledávat tokenů.</span><span class="sxs-lookup"><span data-stu-id="93cd0-113">[Custom analyzers](https://aka.ms/customanalyzers), which allow you to take control over the process of converting text into indexable and searchable tokens.</span></span>
* <span data-ttu-id="93cd0-114">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) a [Azure Table Storage](search-howto-indexing-azure-tables.md) indexery, které vám umožní snadno importovat data ze služby Azure storage do Azure Search v plánu nebo na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="93cd0-114">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) and [Azure Table Storage](search-howto-indexing-azure-tables.md) indexers, which allow you to easily import data from Azure storage into Azure Search on a schedule or on-demand.</span></span>
* <span data-ttu-id="93cd0-115">[Pole mapování](search-indexer-field-mappings.md), který slouží k úpravě indexery jak importovat data do Azure Search.</span><span class="sxs-lookup"><span data-stu-id="93cd0-115">[Field mappings](search-indexer-field-mappings.md), which allow you to customize how indexers import data into Azure Search.</span></span>
* <span data-ttu-id="93cd0-116">Značky etag binárním rozsáhlým, která umožňuje aktualizovat definice indexy, indexery a zdroje dat způsobem bezpečných souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="93cd0-116">ETags, which allow you to update the definitions of indexes, indexers, and data sources in a concurrency-safe manner.</span></span> 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a><span data-ttu-id="93cd0-117">Kroky pro upgrade</span><span class="sxs-lookup"><span data-stu-id="93cd0-117">Steps to upgrade</span></span>
<span data-ttu-id="93cd0-118">Pokud provádíte upgrade z verze 2015-02-28, pravděpodobně nebudete mít proveďte změny v kódu, jiné než, chcete-li změnit číslo verze.</span><span class="sxs-lookup"><span data-stu-id="93cd0-118">If you are upgrading from version 2015-02-28, you probably won't have to make any changes to your code, other than to change the version number.</span></span> <span data-ttu-id="93cd0-119">Pouze situace, ve kterých budete muset změnit kód jsou při:</span><span class="sxs-lookup"><span data-stu-id="93cd0-119">The only situations in which you may need to change code are when:</span></span>

* <span data-ttu-id="93cd0-120">Váš kód selže, když nerozpoznané vlastnosti jsou vráceny v odpovědi rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="93cd0-120">Your code fails when unrecognized properties are returned in an API response.</span></span> <span data-ttu-id="93cd0-121">Ve výchozím nastavení je třeba vlastnosti, které nerozumí ignorovat vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="93cd0-121">By default your application should ignore properties that it does not understand.</span></span>
* <span data-ttu-id="93cd0-122">Váš kód potrvají žádostí o rozhraní API a pokusí se přeposlat na novou verzi rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="93cd0-122">Your code persists API requests and tries to resend them to the new API version.</span></span> <span data-ttu-id="93cd0-123">Například tomu může dojít, pokud vaše aplikace bude pokračování tokeny vrácená z rozhraní API služby Search (Další informace vyhledejte `@search.nextPageParameters` v [referenční dokumentace rozhraní API pro vyhledávání](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).</span><span class="sxs-lookup"><span data-stu-id="93cd0-123">For example, this might happen if your application persists continuation tokens returned from the Search API (for more information, look for `@search.nextPageParameters` in the [Search API Reference](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).</span></span>

<span data-ttu-id="93cd0-124">Pokud některý z těchto situacích se na vás vztahovat, budete muset odpovídajícím způsobem upravit svůj kód.</span><span class="sxs-lookup"><span data-stu-id="93cd0-124">If either of these situations apply to you, then you may need to change your code accordingly.</span></span> <span data-ttu-id="93cd0-125">V opačném žádné změny by měl být nutné, pokud chcete začít používat [nové funkce](#WhatsNew) verze 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="93cd0-125">Otherwise, no changes should be necessary unless you want to start using the [new features](#WhatsNew) of version 2016-09-01.</span></span>

<span data-ttu-id="93cd0-126">Pokud provádíte upgrade z verze 2015-02-28-Preview, výše platí také, ale také musí být vědět, že některé funkce preview nejsou dostupné ve verzi 2016-09-01:</span><span class="sxs-lookup"><span data-stu-id="93cd0-126">If you are upgrading from version 2015-02-28-Preview, the above also applies, but you must also be aware that some preview features are not available in version 2016-09-01:</span></span>

* <span data-ttu-id="93cd0-127">Podpora indexeru Azure Blob Storage pro soubory CSV a objekty BLOB obsahující pole JSON.</span><span class="sxs-lookup"><span data-stu-id="93cd0-127">Azure Blob Storage indexer support for CSV files and blobs containing JSON arrays.</span></span>
* <span data-ttu-id="93cd0-128">Synonyma.</span><span class="sxs-lookup"><span data-stu-id="93cd0-128">Synonyms</span></span>
* <span data-ttu-id="93cd0-129">"Informace jako to" dotazy</span><span class="sxs-lookup"><span data-stu-id="93cd0-129">"More like this" queries</span></span>

<span data-ttu-id="93cd0-130">Pokud váš kód používá tyto funkce, nebudete moci upgradovat na 2016-09-01 bez odebrání vašeho využití z nich.</span><span class="sxs-lookup"><span data-stu-id="93cd0-130">If your code uses these features, you will not be able to upgrade to 2016-09-01 without removing your usage of them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93cd0-131">Prosím mějte na paměti, verzi preview rozhraní API jsou určené pro testování a vyhodnocení a neměl by se používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="93cd0-131">Please remember, preview APIs are intended for testing and evaluation, and should not be used in production environments.</span></span>
> 
> 

## <a name="conclusion"></a><span data-ttu-id="93cd0-132">Závěr</span><span class="sxs-lookup"><span data-stu-id="93cd0-132">Conclusion</span></span>
<span data-ttu-id="93cd0-133">Pokud potřebujete další podrobnosti o použití rozhraní API REST služby vyhledávání Azure, přečtěte si nedávno aktualizovaného [referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/azure/dn798935.aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="93cd0-133">If you need more details on using the Azure Search Service REST API, see the recently updated [API Reference](https://msdn.microsoft.com/library/azure/dn798935.aspx) on MSDN.</span></span>

<span data-ttu-id="93cd0-134">Uvítáme vaše názory na Azure Search.</span><span class="sxs-lookup"><span data-stu-id="93cd0-134">We welcome your feedback on Azure Search.</span></span> <span data-ttu-id="93cd0-135">Pokud narazíte na potíže, neváhejte nám požádat o pomoc na [fórum Azure Search MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) nebo [StackOverflow](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="93cd0-135">If you encounter problems, feel free to ask us for help on the [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) or [StackOverflow](http://stackoverflow.com/).</span></span> <span data-ttu-id="93cd0-136">Pokud dotaz se žádostí o službě Azure Search na StackOverflow, je třeba označit její `azure-search`.</span><span class="sxs-lookup"><span data-stu-id="93cd0-136">If you're asking a question about Azure Search on StackOverflow, make sure to tag it with `azure-search`.</span></span>

<span data-ttu-id="93cd0-137">Děkujeme za používání služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="93cd0-137">Thank you for using Azure Search!</span></span>

