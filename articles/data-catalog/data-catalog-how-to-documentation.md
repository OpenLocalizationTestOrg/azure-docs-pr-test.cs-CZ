---
title: zdroje dat toodocument aaaHow | Microsoft Docs
description: "Jak tooarticle zvýraznění jak toodocument datových prostředcích v Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 053b1701-b848-4ada-b726-6f485caa9961
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 4e46838d91ab4d0c0bc569ac526a0c729134bb10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="document-data-sources"></a><span data-ttu-id="7f3a5-103">Dokumentování zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="7f3a5-103">Document data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="7f3a5-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="7f3a5-104">Introduction</span></span>
<span data-ttu-id="7f3a5-105">**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="7f3a5-106">Jinými slovy **Azure Data Catalog** se točí kolem pomoci zjistit, osoby *pochopit*a použít zdroje dat a pomáhá organizacím tooget informace z jejich stávajících dat hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-106">In other words, **Azure Data Catalog** is all about helping people discover, *understand*, and use data sources, and helping organizations tooget more value from their existing data.</span></span>

<span data-ttu-id="7f3a5-107">Při registraci zdroje dat s **Azure Data Catalog**, jeho metadata se zkopíruje a indexované službou hello, ale není hello scénáře ukončení existuje.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by hello service, but hello story doesn’t end there.</span></span> <span data-ttu-id="7f3a5-108">**Azure Data Catalog** také umožňuje uživatelům tooprovide vlastní kompletní dokumentaci, která můžete popsat hello využití a běžné scénáře pro zdroj dat hello.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-108">**Azure Data Catalog** also allows users tooprovide their own complete documentation that can describe hello usage and common scenarios for hello data source.</span></span>

<span data-ttu-id="7f3a5-109">V [jak zdroje dat tooannotate](data-catalog-how-to-annotate.md), zjistíte, že odborníky, kteří znají hello zdroj dat může opatřit poznámkami ho pomocí značek a popis.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-109">In [How tooannotate data sources](data-catalog-how-to-annotate.md), you learn that experts who know hello data source can annotate it with tags and a description.</span></span> <span data-ttu-id="7f3a5-110">Hello **Azure Data Catalog** portál obsahuje bohatou textového editoru, aby uživatelé můžete plně dokumentů datových prostředků a kontejnery.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-110">hello **Azure Data Catalog** portal includes a rich text editor so that users can fully document data assets and containers.</span></span> <span data-ttu-id="7f3a5-111">Hello editor zahrnuje formátování, jako je například záhlaví, formátování tabulek, číslované seznamy a seznamy s odrážkami textu odstavce.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-111">hello editor includes paragraph formatting, such as headings, text formatting, bulleted lists, numbered lists, and tables.</span></span>

<span data-ttu-id="7f3a5-112">Značky a popisy jsou ideální pro jednoduché poznámky.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-112">Tags and descriptions are great for simple annotations.</span></span> <span data-ttu-id="7f3a5-113">Ale spotřebitelé dat toohelp lépe pochopit hello použít zdroj dat a obchodní scénáře pro zdroj dat, odborník může poskytnout úplný, podrobnou dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-113">However, toohelp data consumers better understand hello use of a data source, and business scenarios for a data source, an expert can provide complete, detailed documentation.</span></span> <span data-ttu-id="7f3a5-114">Je snadno toodocument zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-114">It's easy toodocument a data source.</span></span> <span data-ttu-id="7f3a5-115">Vyberte datový prostředek nebo kontejner a vyberte **dokumentaci**.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-115">Select a data asset or container, and choose **Documentation**.</span></span>

![](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a><span data-ttu-id="7f3a5-116">Dokumentace datových prostředků</span><span class="sxs-lookup"><span data-stu-id="7f3a5-116">Documenting data assets</span></span>
<span data-ttu-id="7f3a5-117">Hello výhodou **Azure Data Catalog** dokumentaci vám umožní toouse katalogu dat jako obsahu úložiště toocreate dokončení popisů datových prostředků.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-117">hello benefit of **Azure Data Catalog** documentation allows you toouse your Data Catalog as a content repository toocreate a complete narrative of your data assets.</span></span> <span data-ttu-id="7f3a5-118">Můžete si prostudovat podrobné obsah, který popisuje kontejnery a tabulek.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-118">You can explore detailed content that describes containers and tables.</span></span> <span data-ttu-id="7f3a5-119">Pokud již máte obsah v úložišti jiného obsahu, například SharePoint nebo sdílenou složku, můžete přidat toohello asset dokumentace odkazy tooreference tento existujícího obsahu.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-119">If you already have content in another content repository, such as SharePoint or a file share, you can add toohello asset documentation links tooreference this existing content.</span></span> <span data-ttu-id="7f3a5-120">Díky této funkci existující dokumenty zjistitelná.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-120">This feature makes your existing documents more discoverable.</span></span>

> [!NOTE]
> <span data-ttu-id="7f3a5-121">Dokumentace není součástí indexu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-121">Documentation is not included in search index.</span></span>
>
>

![](media/data-catalog-documentation/data-catalog-documentation2.png)

<span data-ttu-id="7f3a5-122">Hello úroveň dokumentace může být v rozsahu od popisující hello vlastnosti a hodnoty dat asset kontejneru tooa podrobný popis schématu tabulky v rámci kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-122">hello level of documentation can range from describing hello characteristics and value of a data asset container tooa detailed description of table schema within a container.</span></span> <span data-ttu-id="7f3a5-123">úroveň Hello dokumenty poskytnuté by měl doprovází vašim obchodním potřebám.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-123">hello level of documentation provided should be driven by your business needs.</span></span> <span data-ttu-id="7f3a5-124">Ale obecně uvádíme několik výhody a nevýhody dokumentace datových prostředků:</span><span class="sxs-lookup"><span data-stu-id="7f3a5-124">But in general, here are a few pros and cons of documenting data assets:</span></span>

* <span data-ttu-id="7f3a5-125">Zdokumentujte právě kontejner: veškerý obsah hello je na jednom místě, ale může být nedostatek potřeby podrobnosti o uživatelé toomake informované rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-125">Document just a container: All hello content is in one place, but might lack necessary details for users toomake an informed decision.</span></span>
* <span data-ttu-id="7f3a5-126">Zdokumentujte právě hello tabulky: obsahu je konkrétní toothat objektu, ale mají vaši uživatelé více míst pro dokumenty.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-126">Document just hello tables: Content is specific toothat object, but your users have multiple places for documents.</span></span>
* <span data-ttu-id="7f3a5-127">Zdokumentujte kontejnery a tabulky: nejvíce komplexní přístup, ale může způsobit další údržby hello dokumentů.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-127">Document containers and tables: Most comprehensive approach, but might introduce more maintenance of hello documents.</span></span>

## <a name="summary"></a><span data-ttu-id="7f3a5-128">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7f3a5-128">Summary</span></span>
<span data-ttu-id="7f3a5-129">Dokumentace zdroje dat s **Azure Data Catalog** můžete vytvořit komentáře o datových prostředcích v množství podrobností, kolik potřebujete.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-129">Documenting data sources with **Azure Data Catalog** can create a narrative about your data assets in as much detail as you need.</span></span>  <span data-ttu-id="7f3a5-130">Pomocí odkazů můžete propojit toocontent uložené v existující úložiště obsahu, který spojuje vaše stávající dokumenty a data prostředků.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-130">By using links, you can link toocontent stored in an existing content repository, which brings your existing docs and data assets together.</span></span> <span data-ttu-id="7f3a5-131">Jakmile vaši uživatelé zjistit prostředky příslušná data, mohou mít úplnou sadu dokumentace.</span><span class="sxs-lookup"><span data-stu-id="7f3a5-131">Once your users discover appropriate data assets, they can have a complete set of documentation.</span></span>
