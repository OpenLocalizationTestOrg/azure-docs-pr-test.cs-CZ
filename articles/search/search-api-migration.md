---
title: "aaaUpgrading toohello rozhraní API služby REST Azure vyhledávání verze 2016-09-01 | Microsoft Docs"
description: "Upgrade toohello rozhraní API služby REST Azure vyhledávání verze 2016-09-01"
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
ms.openlocfilehash: d0276b9cc52996a59f9aa726c27e62c6082eb908
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrading-toohello-azure-search-service-rest-api-version-2016-09-01"></a><span data-ttu-id="c8b6f-103">Upgrade toohello rozhraní API služby REST Azure vyhledávání verze 2016-09-01</span><span class="sxs-lookup"><span data-stu-id="c8b6f-103">Upgrading toohello Azure Search Service REST API version 2016-09-01</span></span>
<span data-ttu-id="c8b6f-104">Pokud používáte verze 2015-02-28 nebo 2015-02-28-Preview Dobrý den [rozhraní REST API služby Azure Search](https://msdn.microsoft.com/library/azure/dn798935.aspx), tento článek vám pomůže při upgradu vaší aplikace toouse hello další všeobecně dostupná verze rozhraní API, 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-104">If you're using version 2015-02-28 or 2015-02-28-Preview of hello [Azure Search Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), this article will help you upgrade your application toouse hello next generally available API version, 2016-09-01.</span></span>

<span data-ttu-id="c8b6f-105">Verze 2016-09-01 Dobrý den REST API obsahuje některé změny z předchozích verzí.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-105">Version 2016-09-01 of hello REST API contains some changes from earlier versions.</span></span> <span data-ttu-id="c8b6f-106">Toto jsou většinou zpětně kompatibilní, takže měnili kód měli vyžadovat jen minimální úsilí, v závislosti na verzi, které jste používali před.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-106">These are mostly backward compatible, so changing your code should require only minimal effort, depending on which version you were using before.</span></span> <span data-ttu-id="c8b6f-107">V tématu [tooupgrade kroky](#UpgradeSteps) pokyny toochange váš kód toouse hello nové verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-107">See [Steps tooupgrade](#UpgradeSteps) for instructions on how toochange your code toouse hello new API version.</span></span>

> [!NOTE]
> <span data-ttu-id="c8b6f-108">Instanci služby Azure Search podporuje několik verzí rozhraní REST API, včetně hello nejnovějšího.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-108">Your Azure Search service instance supports several REST API versions, including hello latest one.</span></span> <span data-ttu-id="c8b6f-109">Toouse verze můžete pokračovat v případě, že je už hello nejnovějšího, ale doporučujeme migraci nejnovější verze vašeho kódu toouse hello.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-109">You can continue toouse a version when it is no longer hello latest one, but we recommend that you migrate your code toouse hello newest version.</span></span>

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a><span data-ttu-id="c8b6f-110">Co je nového ve verzi 2016-09-01</span><span class="sxs-lookup"><span data-stu-id="c8b6f-110">What's new in version 2016-09-01</span></span>
<span data-ttu-id="c8b6f-111">Verze 2016-09-01 je hello druhý všeobecně dostupná verze hello rozhraní REST API služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-111">Version 2016-09-01 is hello second generally available release of hello Azure Search Service REST API.</span></span> <span data-ttu-id="c8b6f-112">Nové funkce v této verzi rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="c8b6f-112">New features in this API version include:</span></span>

* <span data-ttu-id="c8b6f-113">[Vlastní analyzátorů](https://aka.ms/customanalyzers), které umožňují tootake kontrolu nad hello proces převodu textu indexovanou a vyhledávat tokenů.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-113">[Custom analyzers](https://aka.ms/customanalyzers), which allow you tootake control over hello process of converting text into indexable and searchable tokens.</span></span>
* <span data-ttu-id="c8b6f-114">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) a [Azure Table Storage](search-howto-indexing-azure-tables.md) indexery, které umožňují tooeasily importovat data ze služby Azure storage do Azure Search v plánu nebo na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-114">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) and [Azure Table Storage](search-howto-indexing-azure-tables.md) indexers, which allow you tooeasily import data from Azure storage into Azure Search on a schedule or on-demand.</span></span>
* <span data-ttu-id="c8b6f-115">[Pole mapování](search-indexer-field-mappings.md), které umožňují toocustomize indexery jak importovat data do Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-115">[Field mappings](search-indexer-field-mappings.md), which allow you toocustomize how indexers import data into Azure Search.</span></span>
* <span data-ttu-id="c8b6f-116">Značky etag binárním rozsáhlým, díky tooupdate hello Definice indexy, indexery a zdroje dat způsobem bezpečných souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-116">ETags, which allow you tooupdate hello definitions of indexes, indexers, and data sources in a concurrency-safe manner.</span></span> 

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a><span data-ttu-id="c8b6f-117">Kroky tooupgrade</span><span class="sxs-lookup"><span data-stu-id="c8b6f-117">Steps tooupgrade</span></span>
<span data-ttu-id="c8b6f-118">Pokud provádíte upgrade z verze 2015-02-28, pravděpodobně nebudete mít toomake tooyour žádné změny kódu než číslo verze toochange hello.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-118">If you are upgrading from version 2015-02-28, you probably won't have toomake any changes tooyour code, other than toochange hello version number.</span></span> <span data-ttu-id="c8b6f-119">Hello pouze situace, ve kterých budete pravděpodobně toochange kódu jsou při:</span><span class="sxs-lookup"><span data-stu-id="c8b6f-119">hello only situations in which you may need toochange code are when:</span></span>

* <span data-ttu-id="c8b6f-120">Váš kód selže, když nerozpoznané vlastnosti jsou vráceny v odpovědi rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-120">Your code fails when unrecognized properties are returned in an API response.</span></span> <span data-ttu-id="c8b6f-121">Ve výchozím nastavení je třeba vlastnosti, které nerozumí ignorovat vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-121">By default your application should ignore properties that it does not understand.</span></span>
* <span data-ttu-id="c8b6f-122">Váš kód potrvají žádostí o rozhraní API a pokusí tooresend je nová verze rozhraní API toohello.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-122">Your code persists API requests and tries tooresend them toohello new API version.</span></span> <span data-ttu-id="c8b6f-123">Například tomu může dojít, pokud vaše aplikace bude pokračování tokeny vrácená z hello rozhraní API služby Search (Další informace vyhledejte `@search.nextPageParameters` v hello [referenční dokumentace rozhraní API pro vyhledávání](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).</span><span class="sxs-lookup"><span data-stu-id="c8b6f-123">For example, this might happen if your application persists continuation tokens returned from hello Search API (for more information, look for `@search.nextPageParameters` in hello [Search API Reference](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).</span></span>

<span data-ttu-id="c8b6f-124">Pokud některý z těchto situacích použít tooyou, pak musíte toochange kódu odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-124">If either of these situations apply tooyou, then you may need toochange your code accordingly.</span></span> <span data-ttu-id="c8b6f-125">Jinak, žádné změny by měl být nutné, pokud chcete, aby toostart pomocí hello [nové funkce](#WhatsNew) verze 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-125">Otherwise, no changes should be necessary unless you want toostart using hello [new features](#WhatsNew) of version 2016-09-01.</span></span>

<span data-ttu-id="c8b6f-126">Pokud provádíte upgrade z verze 2015-02-28-Preview, hello výše uvedené platí také, ale také musí být vědět, že některé funkce preview nejsou dostupné ve verzi 2016-09-01:</span><span class="sxs-lookup"><span data-stu-id="c8b6f-126">If you are upgrading from version 2015-02-28-Preview, hello above also applies, but you must also be aware that some preview features are not available in version 2016-09-01:</span></span>

* <span data-ttu-id="c8b6f-127">Podpora indexeru Azure Blob Storage pro soubory CSV a objekty BLOB obsahující pole JSON.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-127">Azure Blob Storage indexer support for CSV files and blobs containing JSON arrays.</span></span>
* <span data-ttu-id="c8b6f-128">Synonyma.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-128">Synonyms</span></span>
* <span data-ttu-id="c8b6f-129">"Informace jako to" dotazy</span><span class="sxs-lookup"><span data-stu-id="c8b6f-129">"More like this" queries</span></span>

<span data-ttu-id="c8b6f-130">Pokud váš kód používá tyto funkce, nebudete moct tooupgrade too2016-09-01 bez odebrání vašeho využití z nich.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-130">If your code uses these features, you will not be able tooupgrade too2016-09-01 without removing your usage of them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c8b6f-131">Prosím mějte na paměti, verzi preview rozhraní API jsou určené pro testování a vyhodnocení a neměl by se používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-131">Please remember, preview APIs are intended for testing and evaluation, and should not be used in production environments.</span></span>
> 
> 

## <a name="conclusion"></a><span data-ttu-id="c8b6f-132">Závěr</span><span class="sxs-lookup"><span data-stu-id="c8b6f-132">Conclusion</span></span>
<span data-ttu-id="c8b6f-133">Pokud potřebujete další podrobnosti o použití hello rozhraní REST API služby Azure Search, přečtěte si hello nedávno aktualizován [referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/azure/dn798935.aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-133">If you need more details on using hello Azure Search Service REST API, see hello recently updated [API Reference](https://msdn.microsoft.com/library/azure/dn798935.aspx) on MSDN.</span></span>

<span data-ttu-id="c8b6f-134">Uvítáme vaše názory na Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-134">We welcome your feedback on Azure Search.</span></span> <span data-ttu-id="c8b6f-135">Pokud narazíte na potíže, myslíte, že volné tooask nám nápovědu k hello [fórum Azure Search MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) nebo [StackOverflow](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c8b6f-135">If you encounter problems, feel free tooask us for help on hello [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) or [StackOverflow](http://stackoverflow.com/).</span></span> <span data-ttu-id="c8b6f-136">Pokud dotaz se žádostí o Azure hledání StackOverflow, ujistěte se, že tootag její `azure-search`.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-136">If you're asking a question about Azure Search on StackOverflow, make sure tootag it with `azure-search`.</span></span>

<span data-ttu-id="c8b6f-137">Děkujeme za používání služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c8b6f-137">Thank you for using Azure Search!</span></span>

