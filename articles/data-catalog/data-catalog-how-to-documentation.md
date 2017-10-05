---
title: "Postup dokumentování zdrojů dat | Microsoft Docs"
description: "Postupy: článek zvýraznění postup dokumentování datových prostředcích v Azure Data Catalog."
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
ms.openlocfilehash: ffe951f60afb57524568fe1ed3b3668d0088e124
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="document-data-sources"></a><span data-ttu-id="bc28c-103">Dokumentování zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="bc28c-103">Document data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="bc28c-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="bc28c-104">Introduction</span></span>
<span data-ttu-id="bc28c-105">**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace.</span><span class="sxs-lookup"><span data-stu-id="bc28c-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="bc28c-106">Jinými slovy **Azure Data Catalog** se točí kolem pomoci zjistit, osoby *pochopit*, použijte zdroje dat a pomáhá organizacím získat větší hodnotu z jejich stávající data.</span><span class="sxs-lookup"><span data-stu-id="bc28c-106">In other words, **Azure Data Catalog** is all about helping people discover, *understand*, and use data sources, and helping organizations to get more value from their existing data.</span></span>

<span data-ttu-id="bc28c-107">Při registraci zdroje dat s **Azure Data Catalog**, jeho metadata se zkopíruje a indexované službou, ale není článek existuje ukončení.</span><span class="sxs-lookup"><span data-stu-id="bc28c-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by the service, but the story doesn’t end there.</span></span> <span data-ttu-id="bc28c-108">**Azure Data Catalog** také umožňuje uživatelům poskytnout vlastní kompletní dokumentaci popisující běžné scénáře pro zdroj dat a využití.</span><span class="sxs-lookup"><span data-stu-id="bc28c-108">**Azure Data Catalog** also allows users to provide their own complete documentation that can describe the usage and common scenarios for the data source.</span></span>

<span data-ttu-id="bc28c-109">V [postup přidání poznámek ke zdrojům dat](data-catalog-how-to-annotate.md), zjistíte, že odborníky, kteří znají zdroj dat může opatřit poznámkami ho pomocí značek a popis.</span><span class="sxs-lookup"><span data-stu-id="bc28c-109">In [How to annotate data sources](data-catalog-how-to-annotate.md), you learn that experts who know the data source can annotate it with tags and a description.</span></span> <span data-ttu-id="bc28c-110">**Azure Data Catalog** portál obsahuje bohatou textového editoru, aby uživatelé můžete plně dokumentů datových prostředků a kontejnery.</span><span class="sxs-lookup"><span data-stu-id="bc28c-110">The **Azure Data Catalog** portal includes a rich text editor so that users can fully document data assets and containers.</span></span> <span data-ttu-id="bc28c-111">Editor zahrnuje formátování odstavce, jako je například záhlaví, formátování textu, seznamy s odrážkami, číslované seznamy a tabulky.</span><span class="sxs-lookup"><span data-stu-id="bc28c-111">The editor includes paragraph formatting, such as headings, text formatting, bulleted lists, numbered lists, and tables.</span></span>

<span data-ttu-id="bc28c-112">Značky a popisy jsou ideální pro jednoduché poznámky.</span><span class="sxs-lookup"><span data-stu-id="bc28c-112">Tags and descriptions are great for simple annotations.</span></span> <span data-ttu-id="bc28c-113">Chcete-li spotřebitelé dat se lépe pochopit zdroje dat a obchodní scénáře použití pro zdroj dat, ale odborník může poskytovat dokončení, podrobnou dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="bc28c-113">However, to help data consumers better understand the use of a data source, and business scenarios for a data source, an expert can provide complete, detailed documentation.</span></span> <span data-ttu-id="bc28c-114">Je snadné zdroj dat dokumentu.</span><span class="sxs-lookup"><span data-stu-id="bc28c-114">It's easy to document a data source.</span></span> <span data-ttu-id="bc28c-115">Vyberte datový prostředek nebo kontejner a vyberte **dokumentaci**.</span><span class="sxs-lookup"><span data-stu-id="bc28c-115">Select a data asset or container, and choose **Documentation**.</span></span>

![](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a><span data-ttu-id="bc28c-116">Dokumentace datových prostředků</span><span class="sxs-lookup"><span data-stu-id="bc28c-116">Documenting data assets</span></span>
<span data-ttu-id="bc28c-117">Výhodou **Azure Data Catalog** dokumentaci vám umožňuje používat Data Catalog jako obsahu úložiště k vytvoření komentáře dokončení datových prostředků.</span><span class="sxs-lookup"><span data-stu-id="bc28c-117">The benefit of **Azure Data Catalog** documentation allows you to use your Data Catalog as a content repository to create a complete narrative of your data assets.</span></span> <span data-ttu-id="bc28c-118">Můžete si prostudovat podrobné obsah, který popisuje kontejnery a tabulek.</span><span class="sxs-lookup"><span data-stu-id="bc28c-118">You can explore detailed content that describes containers and tables.</span></span> <span data-ttu-id="bc28c-119">Pokud již máte obsah v úložišti jiného obsahu, například SharePoint nebo sdílenou složku, můžete přidat do odkazy dokumentace asset odkazovat na tuto existující obsah.</span><span class="sxs-lookup"><span data-stu-id="bc28c-119">If you already have content in another content repository, such as SharePoint or a file share, you can add to the asset documentation links to reference this existing content.</span></span> <span data-ttu-id="bc28c-120">Díky této funkci existující dokumenty zjistitelná.</span><span class="sxs-lookup"><span data-stu-id="bc28c-120">This feature makes your existing documents more discoverable.</span></span>

> [!NOTE]
> <span data-ttu-id="bc28c-121">Dokumentace není součástí indexu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="bc28c-121">Documentation is not included in search index.</span></span>
>
>

![](media/data-catalog-documentation/data-catalog-documentation2.png)

<span data-ttu-id="bc28c-122">Úroveň dokumentaci rozsahu popisující vlastnosti a hodnotu kontejner dat asset k podrobný popis schématu tabulky v rámci kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bc28c-122">The level of documentation can range from describing the characteristics and value of a data asset container to a detailed description of table schema within a container.</span></span> <span data-ttu-id="bc28c-123">Úroveň dokumenty poskytnuté by měl být doprovází vašim obchodním potřebám.</span><span class="sxs-lookup"><span data-stu-id="bc28c-123">The level of documentation provided should be driven by your business needs.</span></span> <span data-ttu-id="bc28c-124">Ale obecně uvádíme několik výhody a nevýhody dokumentace datových prostředků:</span><span class="sxs-lookup"><span data-stu-id="bc28c-124">But in general, here are a few pros and cons of documenting data assets:</span></span>

* <span data-ttu-id="bc28c-125">Zdokumentujte právě kontejner: veškerý obsah na jednom místě, ale může nedostatku potřebné detaily uživatelům provést informované rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="bc28c-125">Document just a container: All the content is in one place, but might lack necessary details for users to make an informed decision.</span></span>
* <span data-ttu-id="bc28c-126">Zdokumentujte právě tabulky: obsah je specifické pro tento objekt, ale mají vaši uživatelé více míst pro dokumenty.</span><span class="sxs-lookup"><span data-stu-id="bc28c-126">Document just the tables: Content is specific to that object, but your users have multiple places for documents.</span></span>
* <span data-ttu-id="bc28c-127">Zdokumentujte kontejnery a tabulky: nejvíce komplexní přístup, ale může způsobit další údržby dokumentů.</span><span class="sxs-lookup"><span data-stu-id="bc28c-127">Document containers and tables: Most comprehensive approach, but might introduce more maintenance of the documents.</span></span>

## <a name="summary"></a><span data-ttu-id="bc28c-128">Souhrn</span><span class="sxs-lookup"><span data-stu-id="bc28c-128">Summary</span></span>
<span data-ttu-id="bc28c-129">Dokumentace zdroje dat s **Azure Data Catalog** můžete vytvořit komentáře o datových prostředcích v množství podrobností, kolik potřebujete.</span><span class="sxs-lookup"><span data-stu-id="bc28c-129">Documenting data sources with **Azure Data Catalog** can create a narrative about your data assets in as much detail as you need.</span></span>  <span data-ttu-id="bc28c-130">Pomocí odkazů můžete propojit na obsah uložený v existující úložišti obsahu, který spojuje vaše stávající dokumenty a data prostředků.</span><span class="sxs-lookup"><span data-stu-id="bc28c-130">By using links, you can link to content stored in an existing content repository, which brings your existing docs and data assets together.</span></span> <span data-ttu-id="bc28c-131">Jakmile vaši uživatelé zjistit prostředky příslušná data, mohou mít úplnou sadu dokumentace.</span><span class="sxs-lookup"><span data-stu-id="bc28c-131">Once your users discover appropriate data assets, they can have a complete set of documentation.</span></span>
