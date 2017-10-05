---
title: "Úvod do služby Azure Data Catalog | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled služby Microsoft Azure Data Catalog, a to včetně jejích funkcí a potíží, na které se zaměřuje. Data Catalog umožňuje všem uživatelům registrovat, objevovat, pochopit a využívat zdroje dat."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: cc733907-17ec-4153-9f0c-5b3754b2db19
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: a28a7679831201fcf3a9d1c15497ff706c2752a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="what-is-azure-data-catalog"></a><span data-ttu-id="00b90-104">Co je Azure Data Catalog?</span><span class="sxs-lookup"><span data-stu-id="00b90-104">What is Azure Data Catalog?</span></span>
<span data-ttu-id="00b90-105">Azure Data Catalog je plně spravovaná cloudová služba, jejíž uživatelé mohou objevovat zdroje dat, které potřebují, a nalezené zdroje dat pochopit.</span><span class="sxs-lookup"><span data-stu-id="00b90-105">Azure Data Catalog is a fully managed cloud service whose users can discover the data sources they need and understand the data sources they find.</span></span> <span data-ttu-id="00b90-106">Zároveň Data Catalog pomáhá organizacím vytěžit více z jejich stávajících investic.</span><span class="sxs-lookup"><span data-stu-id="00b90-106">At the same time, Data Catalog helps organizations get more value from their existing investments.</span></span> 

<span data-ttu-id="00b90-107">Pomocí katalogu Data Catalog může každý uživatel (analytik, odborník přes data nebo vývojář) zdroje dat objevovat, pochopit a využívat.</span><span class="sxs-lookup"><span data-stu-id="00b90-107">With Data Catalog, any user (analyst, data scientist, or developer) can discover, understand, and consume data sources.</span></span> <span data-ttu-id="00b90-108">Data Catalog obsahuje crowdsourcingový model metadat a poznámek.</span><span class="sxs-lookup"><span data-stu-id="00b90-108">Data Catalog includes a crowdsourcing model of metadata and annotations.</span></span> <span data-ttu-id="00b90-109">Je jediným ústředním místem, kam všichni uživatelé v rámci organizace mohou přispět svými znalostmi a vybudovat datovou komunitu a kulturu.</span><span class="sxs-lookup"><span data-stu-id="00b90-109">It is a single, central place for all of an organization's users to contribute their knowledge and build a community and culture of data.</span></span>

## <a name="discovery-challenges-for-data-consumers"></a><span data-ttu-id="00b90-110">Problémy zjišťování pro spotřebitele dat</span><span class="sxs-lookup"><span data-stu-id="00b90-110">Discovery challenges for data consumers</span></span>
<span data-ttu-id="00b90-111">Tradičně je zjišťování zdrojů podnikových dat organický proces založený na kmenových znalostech.</span><span class="sxs-lookup"><span data-stu-id="00b90-111">Traditionally, discovering enterprise data sources has been an organic process based on tribal knowledge.</span></span> <span data-ttu-id="00b90-112">Pro společnosti, které chtějí získat maximální hodnotu ze svých informačních prostředků, to představuje mnoho výzev:</span><span class="sxs-lookup"><span data-stu-id="00b90-112">For companies that want to get the most value from their information assets, this approach presents numerous challenges:</span></span>

* <span data-ttu-id="00b90-113">Uživatelé si nemusí uvědomovat, že zdroje dat existují, pokud s nimi nepřicházejí do styku při jiném procesu.</span><span class="sxs-lookup"><span data-stu-id="00b90-113">Users might not be aware that a data source exists unless they come into contact with it as part of another process.</span></span> <span data-ttu-id="00b90-114">Žádné centrální umístění, kde jsou zdroje dat registrovány, neexistuje.</span><span class="sxs-lookup"><span data-stu-id="00b90-114">There is no central location where data sources are registered.</span></span>
* <span data-ttu-id="00b90-115">Pokud uživatel nezná umístění zdroje dat, nemůže se k těmto datům pomocí klientské aplikace připojit.</span><span class="sxs-lookup"><span data-stu-id="00b90-115">Unless users know the location of a data source, they cannot connect to the data by using a client application.</span></span> <span data-ttu-id="00b90-116">Možnosti využití dat vyžadují, aby uživatelé znali připojovací řetězec nebo cestu.</span><span class="sxs-lookup"><span data-stu-id="00b90-116">Data-consumption experiences require users to know the connection string or path.</span></span>
* <span data-ttu-id="00b90-117">Pokud uživatel nezná umístění dokumentace zdroje dat, nemůže zamýšlenému použití dat rozumět.</span><span class="sxs-lookup"><span data-stu-id="00b90-117">Unless users know the location of a data source's documentation, they cannot understand the intended uses of the data.</span></span> <span data-ttu-id="00b90-118">Zdroje dat a dokumentace se mohou nacházet na různých místech a používat se v různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="00b90-118">Data sources and documentation might live in a variety of places and be consumed through a variety of experiences.</span></span>
* <span data-ttu-id="00b90-119">Pokud mají uživatelé dotazy týkající se informačního prostředku, musí vyhledat odborníka nebo tým odpovědný za příslušná data a zapojit tyto odborníky offline.</span><span class="sxs-lookup"><span data-stu-id="00b90-119">If users have questions about an information asset, they must locate the expert or team that's responsible for the data and engage them offline.</span></span> <span data-ttu-id="00b90-120">Žádné explicitní spojení mezi daty a zaměstnanci s odborným pohledem na jejich použití neexistuje.</span><span class="sxs-lookup"><span data-stu-id="00b90-120">There is no explicit connection between data and those with expert perspectives on its use.</span></span>
* <span data-ttu-id="00b90-121">Pokud uživatelé nerozumí procesu pro vyžádání přístupu ke zdroji dat, zjištění zdroje dat a jeho dokumentace jim stejně nepomůže k těmto datům získat přístup.</span><span class="sxs-lookup"><span data-stu-id="00b90-121">Unless users understand the process for requesting access to the data source, discovering the data source and its documentation still does not help them access the data.</span></span>

## <a name="discovery-challenges-for-data-producers"></a><span data-ttu-id="00b90-122">Problémy zjišťování pro producenty dat</span><span class="sxs-lookup"><span data-stu-id="00b90-122">Discovery challenges for data producers</span></span>
<span data-ttu-id="00b90-123">Přestože se spotřebitelé dat potýkají s těmito dříve popsanými výzvami, uživatelé odpovědní za vytváření a správu informačních prostředků se potýkají s vlastními výzvami:</span><span class="sxs-lookup"><span data-stu-id="00b90-123">Although data consumers face the previously mentioned challenges, users who are responsible for producing and maintaining information assets face challenges of their own:</span></span>

* <span data-ttu-id="00b90-124">Zadávání poznámek ke zdrojům dat s popisnými metadaty je často ztráta času.</span><span class="sxs-lookup"><span data-stu-id="00b90-124">Annotating data sources with descriptive metadata is often a lost effort.</span></span> <span data-ttu-id="00b90-125">Klientské aplikace obvykle popisy uložené ve zdroji dat ignorují.</span><span class="sxs-lookup"><span data-stu-id="00b90-125">Client applications typically ignore descriptions that are stored in the data source.</span></span>
* <span data-ttu-id="00b90-126">Vytváření dokumentace pro zdroje dat je také často ztráta času.</span><span class="sxs-lookup"><span data-stu-id="00b90-126">Creating documentation for data sources is often a lost effort.</span></span> <span data-ttu-id="00b90-127">Synchronizování dokumentace se zdrojem dat je neustávající odpovědnost a uživatelé nemusí dokumentaci důvěřovat, protože je často považována za zastaralou.</span><span class="sxs-lookup"><span data-stu-id="00b90-127">Keeping documentation in sync with data sources is an ongoing responsibility, and users might lack trust in documentation that's perceived as being out of date.</span></span>
* <span data-ttu-id="00b90-128">Vytváření a správa dokumentace pro zdroje dat je složitá a časově náročná.</span><span class="sxs-lookup"><span data-stu-id="00b90-128">Creating and maintaining documentation for data sources is complex and time-consuming.</span></span> <span data-ttu-id="00b90-129">O to větší je výzva tuto dokumentaci učinit snadno dostupnou pro každého uživatele, který příslušný zdroj dat používá.</span><span class="sxs-lookup"><span data-stu-id="00b90-129">Making that documentation readily available to everyone who uses the data source can be even more so.</span></span>
* <span data-ttu-id="00b90-130">Omezení přístupu ke zdrojům dat a zajištění, aby spotřebitelé dat věděli, jak požádat o přístup, je neustávající výzva.</span><span class="sxs-lookup"><span data-stu-id="00b90-130">Restricting access to data sources and ensuring that data consumers know how to request access is an ongoing challenge.</span></span>

<span data-ttu-id="00b90-131">Když se tyto výzvy zkombinují, představují významnou překážkou pro společnosti, které chtějí podněcovat a podporovat používání a pochopení podnikových dat.</span><span class="sxs-lookup"><span data-stu-id="00b90-131">When such challenges are combined, they present a significant barrier for companies who want to encourage and promote the use and understanding of enterprise data.</span></span>

## <a name="azure-data-catalog-can-help"></a><span data-ttu-id="00b90-132">Azure Data Catalog může pomoci</span><span class="sxs-lookup"><span data-stu-id="00b90-132">Azure Data Catalog can help</span></span>
<span data-ttu-id="00b90-133">Data Catalog je určen k řešení těchto problémů a pomáhá podnikům získat větší hodnotu ze stávajících prostředků.</span><span class="sxs-lookup"><span data-stu-id="00b90-133">Data Catalog is designed to address these problems and to help enterprises get the most value from their existing information assets.</span></span> <span data-ttu-id="00b90-134">Data Catalog činí zdroje dat snadno objevitelné a srozumitelné pro uživatele, kteří tato data spravují.</span><span class="sxs-lookup"><span data-stu-id="00b90-134">Data Catalog makes data sources easily discoverable and understandable by the users who manage the data.</span></span>

<span data-ttu-id="00b90-135">Data Catalog poskytuje službu na principu cloudu, do níž lze zaregistrovat zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="00b90-135">Data Catalog provides a cloud-based service into which a data source can be registered.</span></span> <span data-ttu-id="00b90-136">Data zůstávají uložena ve stávajícím umístění, ale do katalogu Data Catalog se přidá kopie metadat spolu s odkazem na umístění zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="00b90-136">The data remains in its existing location, but a copy of its metadata is added to Data Catalog, along with a reference to the data-source location.</span></span> <span data-ttu-id="00b90-137">Tato metadata jsou také indexována, aby byl každý zdroj dat snadno objevitelný prostřednictvím vyhledávání a aby byl srozumitelný uživatelům, kteří ho objevili.</span><span class="sxs-lookup"><span data-stu-id="00b90-137">The metadata is also indexed to make each data source easily discoverable via search and understandable to the users who discover it.</span></span>

<span data-ttu-id="00b90-138">Po zaregistrování zdroje dat mohou být jeho metadata rozšířena uživatelem, který ho zaregistroval, nebo jinými uživateli v podniku.</span><span class="sxs-lookup"><span data-stu-id="00b90-138">After a data source has been registered, its metadata can then be enriched, either by the user who registered it or by other users in the enterprise.</span></span> <span data-ttu-id="00b90-139">Každý uživatel může opatřit poznámkami zdroj dat tím, že přidá popisy, značky nebo další metadata, například dokumentaci a procesy pro žádosti o přístup ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="00b90-139">Any user can annotate a data source by providing descriptions, tags, or other metadata, such as documentation and processes for requesting data source access.</span></span> <span data-ttu-id="00b90-140">Tato popisná metadata doplňují strukturální metadata (například názvy sloupců a typy dat) zaregistrovaná ze zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="00b90-140">This descriptive metadata supplements the structural metadata (such as column names and data types) that's registered from the data source.</span></span>

<span data-ttu-id="00b90-141">Primárním účelem registrace zdrojů dat je zjišťování a porozumění zdrojům, a jejich používání.</span><span class="sxs-lookup"><span data-stu-id="00b90-141">Discovering and understanding data sources and their use is the primary purpose of registering the sources.</span></span> <span data-ttu-id="00b90-142">Podnikoví uživatelé mohou potřebovat data pro business intelligence, vývoj aplikací, datové vědy nebo jiný úkol, ve kterém jsou vyžadována správná data.</span><span class="sxs-lookup"><span data-stu-id="00b90-142">Enterprise users might need data for business intelligence, application development, data science, or any other task where the right data is required.</span></span> <span data-ttu-id="00b90-143">Mohou využít zkušenosti s objevováním v katalogu Data Catalog, aby rychle našli data, která odpovídají jejich potřebám, pochopili je, vyhodnotili jejich vhodnost pro daný účel a využili je otevřením zdroje dat v upřednostňovaném nástroji.</span><span class="sxs-lookup"><span data-stu-id="00b90-143">They can use the Data Catalog discovery experience to quickly find data that matches their needs, understand the data to evaluate its fitness for the purpose, and consume the data by opening the data source in their tool of choice.</span></span> 

<span data-ttu-id="00b90-144">Současně mohou uživatelé přispívat do katalogu označováním, dokumentováním a zadáváním poznámek ke zdrojům dat, které jsou již zaregistrovány.</span><span class="sxs-lookup"><span data-stu-id="00b90-144">At the same time, users can contribute to the catalog by tagging, documenting, and annotating data sources that have already been registered.</span></span> <span data-ttu-id="00b90-145">Mohou také registrovat nové zdroje dat, které lze poté objevit, pochopit a využít komunitou uživatelů katalogu.</span><span class="sxs-lookup"><span data-stu-id="00b90-145">They can also register new data sources, which can then be discovered, understood, and consumed by the community of catalog users.</span></span>

![Možnosti katalogu Data Catalog](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## <a name="learn-more-about-data-catalog"></a><span data-ttu-id="00b90-147">Další informace o katalogu Data Catalog</span><span class="sxs-lookup"><span data-stu-id="00b90-147">Learn more about Data Catalog</span></span>
<span data-ttu-id="00b90-148">Další informace o možnostech katalogu Data Catalog naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="00b90-148">To learn more about the capabilities of Data Catalog, see:</span></span>

* [<span data-ttu-id="00b90-149">Postup registrace zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="00b90-149">How to register data sources</span></span>](data-catalog-how-to-register.md)
* [<span data-ttu-id="00b90-150">Zjišťování zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="00b90-150">How to discover data sources</span></span>](data-catalog-how-to-discover.md)
* [<span data-ttu-id="00b90-151">Postup přidání poznámek ke zdrojům dat</span><span class="sxs-lookup"><span data-stu-id="00b90-151">How to annotate data sources</span></span>](data-catalog-how-to-annotate.md)
* [<span data-ttu-id="00b90-152">Postup dokumentování zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="00b90-152">How to document data sources</span></span>](data-catalog-how-to-documentation.md)
* [<span data-ttu-id="00b90-153">Jak se připojit ke zdrojům dat</span><span class="sxs-lookup"><span data-stu-id="00b90-153">How to connect to data sources</span></span>](data-catalog-how-to-connect.md)
* [<span data-ttu-id="00b90-154">Jak pracovat s velkým objemem dat</span><span class="sxs-lookup"><span data-stu-id="00b90-154">How to work with big data</span></span>](data-catalog-how-to-big-data.md)
* [<span data-ttu-id="00b90-155">Jak spravovat datové prostředky</span><span class="sxs-lookup"><span data-stu-id="00b90-155">How to manage data assets</span></span>](data-catalog-how-to-manage.md)
* [<span data-ttu-id="00b90-156">Jak nastavit obchodní glosář</span><span class="sxs-lookup"><span data-stu-id="00b90-156">How to set up the Business Glossary</span></span>](data-catalog-how-to-business-glossary.md)
* [<span data-ttu-id="00b90-157">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="00b90-157">Frequently asked questions</span></span>](data-catalog-frequently-asked-questions.md)

## <a name="next-steps"></a><span data-ttu-id="00b90-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="00b90-158">Next steps</span></span>
<span data-ttu-id="00b90-159">Pokud chcete začít s katalogem Data Catalog, přejděte na:</span><span class="sxs-lookup"><span data-stu-id="00b90-159">To get started with Data Catalog, go to:</span></span>
* [<span data-ttu-id="00b90-160">Microsoft Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="00b90-160">Microsoft Azure Data Catalog</span></span>](https://www.azuredatacatalog.com)
* [<span data-ttu-id="00b90-161">Začínáme s Azure Data Catalogem</span><span class="sxs-lookup"><span data-stu-id="00b90-161">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)