---
title: "Běžné scénáře služby Azure Data Catalog | Microsoft Docs"
description: "Přehled běžné scénáře pro Azure Data Catalog, včetně registrace a zjišťování zdrojů dat vysoké hodnoty, povolení samoobslužné služby business intelligence a zaznamenávání stávajících znalostí o zdrojích dat a procesy."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 60930d78-d2d4-4d5d-9651-bdda50b0da0e
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: e01e6fa3b9871541bf9573963ced05848a3726e3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-common-scenarios"></a><span data-ttu-id="7ab25-103">Běžné scénáře služby Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="7ab25-103">Azure Data Catalog common scenarios</span></span>
<span data-ttu-id="7ab25-104">Tento článek představuje běžné scénáře, kde Azure Data Catalog může pomoci organizaci získat větší hodnotu z její existující zdroje dat..</span><span class="sxs-lookup"><span data-stu-id="7ab25-104">This article presents common scenarios where Azure Data Catalog can help your organization get more value from its existing data sources.</span></span>

## <a name="scenario-1-registration-of-central-data-sources"></a><span data-ttu-id="7ab25-105">Scénář 1: Registrace centrálních zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="7ab25-105">Scenario 1: Registration of central data sources</span></span>
<span data-ttu-id="7ab25-106">Víc zdrojů dat vysoké hodnoty mají často organizace.</span><span class="sxs-lookup"><span data-stu-id="7ab25-106">Organizations often have many high-value data sources.</span></span> <span data-ttu-id="7ab25-107">Tyto zdroje dat zahrnují-obchodní, online zpracování systémy (OLTP), datových skladů a databází nástroje business intelligence nebo analytics transakcí.</span><span class="sxs-lookup"><span data-stu-id="7ab25-107">These data sources include line-of-business, online transaction processing (OLTP) systems, data warehouses, and business intelligence/analytics databases.</span></span> <span data-ttu-id="7ab25-108">Počet systémů a překryv mezi nimi, obvykle v čase podnikání momentální a zvětšuje samotný podnik zpracovaní prostřednictvím, například fúze a akvizice.</span><span class="sxs-lookup"><span data-stu-id="7ab25-108">The number of systems, and the overlap between them, typically grows over time as business needs evolve and the business itself evolves through, for example, mergers and acquisitions.</span></span>

<span data-ttu-id="7ab25-109">Může být složité pro členy organizace vědět, kde vyhledejte data v rámci těchto zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="7ab25-109">It can be difficult for organization members to know where to locate the data within these data sources.</span></span> <span data-ttu-id="7ab25-110">Jsou všechny příliš běžné otázky takto:</span><span class="sxs-lookup"><span data-stu-id="7ab25-110">Questions like the following are all too common:</span></span>

* <span data-ttu-id="7ab25-111">Tři HR systémů používaných v rámci společnosti, který použít k vytvoření tohoto typu sestavy?</span><span class="sxs-lookup"><span data-stu-id="7ab25-111">Of the three HR systems used within the company, which should I use to create this type of report?</span></span>
* <span data-ttu-id="7ab25-112">Kde najdu získat certifikovaných prodejní čísla pro fiskálního roku, který právě ukončen?</span><span class="sxs-lookup"><span data-stu-id="7ab25-112">Where should I go to get the certified sales numbers for the fiscal year that just ended?</span></span>
* <span data-ttu-id="7ab25-113">Kdo by měl požádejte nebo co je proces, který by měl pomůže získat přístup k datovému skladu?</span><span class="sxs-lookup"><span data-stu-id="7ab25-113">Who should I ask, or what is the process I should use to get access to the data warehouse?</span></span>
* <span data-ttu-id="7ab25-114">Neznámého Pokud tato čísla jsou správná.</span><span class="sxs-lookup"><span data-stu-id="7ab25-114">I don’t know if these numbers are right.</span></span> <span data-ttu-id="7ab25-115">I kdo pro náhled na tom, jak by měla tato data využít, než je možné sdílet tento řídicí panel se členy týmu?</span><span class="sxs-lookup"><span data-stu-id="7ab25-115">Who can I ask for insight on how this data is supposed to be used before I share this dashboard with my team?</span></span>

<span data-ttu-id="7ab25-116">Na tyto a další otázky můžete zadat Azure Data Catalog odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7ab25-116">To these and other questions, Azure Data Catalog can provide answers.</span></span> <span data-ttu-id="7ab25-117">Zdroje dat centrální vysoké hodnoty, spravované IT, které se používají v organizacích jsou často logické výchozím bodem pro sestavování katalogu.</span><span class="sxs-lookup"><span data-stu-id="7ab25-117">The central, high-value, IT-managed data sources that are used across organizations are often the logical starting point for populating the catalog.</span></span> <span data-ttu-id="7ab25-118">I když každý uživatel může zaregistrovat zdroj dat, s katalogu kick-started se zdroji dat, které budou pravděpodobně definovat hodnotu pro maximální počet uživatelů, pomáhá jednotky přijetí a použití systému.</span><span class="sxs-lookup"><span data-stu-id="7ab25-118">Although any user can register a data source, having the catalog kick-started with the data sources that are most likely to provide value to the largest number of users helps drive adoption and use of the system.</span></span> 

<span data-ttu-id="7ab25-119">Pokud jsou Začínáme s Azure Data Catalog, identifikujete a zaregistrujete klíčových zdrojích dat používané mnoha různými týmy spotřebitelé dat může být vaším prvním krokem na úspěšné provedení.</span><span class="sxs-lookup"><span data-stu-id="7ab25-119">If you are getting started with Azure Data Catalog, identifying and registering key data sources that are used by many different teams of data consumers can be your first step to success.</span></span>

<span data-ttu-id="7ab25-120">Tento scénář také představuje příležitost k přidání poznámek ke zdrojům dat vysoké hodnoty, aby byly snadněji pochopit a přístup.</span><span class="sxs-lookup"><span data-stu-id="7ab25-120">This scenario also presents an opportunity to annotate the high-value data sources to make them easier to understand and access.</span></span> <span data-ttu-id="7ab25-121">Je také informace o tom, jak mohou uživatelé žádat o přístup ke zdroji dat jeden klíčovým prvkem toto úsilí.</span><span class="sxs-lookup"><span data-stu-id="7ab25-121">One key aspect of this effort is to include information on how users can request access to the data source.</span></span> <span data-ttu-id="7ab25-122">S Azure Data Catalog můžete zadat e-mailovou adresu uživatele nebo týmu, který je zodpovědný za řízení přístupu zdroj dat, odkazů na existující nástroje nebo dokumentaci nebo libovolný text, který popisuje proces žádosti o přístup.</span><span class="sxs-lookup"><span data-stu-id="7ab25-122">With Azure Data Catalog, you can provide the email address of the user or team that's responsible for controlling data-source access, links to existing tools or documentation, or free text that describes the access-request process.</span></span> <span data-ttu-id="7ab25-123">Tyto informace pomáhají členy, kteří zjišťovat registrované datové zdroje, ale kteří ještě nemají oprávnění k přístupu k datům pomocí procesů, které jsou definovány a řídí vlastníky zdroje dat snadno požádat o přístup.</span><span class="sxs-lookup"><span data-stu-id="7ab25-123">This information helps members who discover registered data sources but who do not yet have permissions to access the data to easily request access by using the processes that are defined and controlled by the data-source owners.</span></span>

## <a name="scenario-2-self-service-business-intelligence"></a><span data-ttu-id="7ab25-124">Scénář 2: Samoobslužné služby business intelligence</span><span class="sxs-lookup"><span data-stu-id="7ab25-124">Scenario 2: Self-service business intelligence</span></span>
<span data-ttu-id="7ab25-125">Přestože tradiční podnikové řešení business intelligence nadále být neocenitelnou pomocí součástí krajiny data řada organizací, změna se stimulací podle obchodních udělal samoobslužné služby BI více důležité.</span><span class="sxs-lookup"><span data-stu-id="7ab25-125">Although traditional corporate business-intelligence solutions continue to be an invaluable part of many organizations’ data landscapes, the changing pace of business has made self-service BI more and more important.</span></span> <span data-ttu-id="7ab25-126">Pomocí samoobslužné služby BI pracovníků s informacemi a analytici můžete vytvořit vlastní sestavy, sešity a řídicí panely bez spoléhání na centrální IT tým nebo je omezené na základě plánu a dostupnost IT tým.</span><span class="sxs-lookup"><span data-stu-id="7ab25-126">By using self-service BI, information workers and analysts can create their own reports, workbooks, and dashboards without relying on a central IT team or being restricted by that IT team’s schedule and availability.</span></span>

<span data-ttu-id="7ab25-127">V samoobslužné scénáře BI uživatelé běžně kombinovat data z více zdrojů, řada z nich nemusí dříve byl použit pro BI a analýzy.</span><span class="sxs-lookup"><span data-stu-id="7ab25-127">In self-service BI scenarios, users commonly combine data from multiple sources, many of which might not have previously been used for BI and analysis.</span></span> <span data-ttu-id="7ab25-128">I když některé z těchto zdrojů dat už může být známé, může být náročné Pokud chcete zjistit, co dělat, vyhledejte a vyhodnotit potenciální zdroje dat pro danou úlohu.</span><span class="sxs-lookup"><span data-stu-id="7ab25-128">Although some of these data sources might already be known, it can be challenging to discover what to do to locate and evaluate potential data sources for a given task.</span></span>

<span data-ttu-id="7ab25-129">Tradičně, tento proces zjišťování je ruční: analytici používat jejich připojení k síti peer k ostatním, kteří pracují s daty se žádá o identifikaci.</span><span class="sxs-lookup"><span data-stu-id="7ab25-129">Traditionally, this discovery process is a manual one: analysts use their peer network connections to identify others who work with the data being sought.</span></span> <span data-ttu-id="7ab25-130">Po zdroje dat nalezen a použít, proces se opakuje opakujte pro každý další samoobslužné služby BI úsilí, s více uživateli provádění redundantní ruční proces zjišťování.</span><span class="sxs-lookup"><span data-stu-id="7ab25-130">After a data source is found and used, the process repeats itself again for each subsequent self-service BI effort, with multiple users performing a redundant manual process of discovery.</span></span>

<span data-ttu-id="7ab25-131">S Azure Data Catalog může vaše organizace rozdělit tento cyklus úsilí.</span><span class="sxs-lookup"><span data-stu-id="7ab25-131">With Azure Data Catalog, your organization can break this cycle of effort.</span></span> <span data-ttu-id="7ab25-132">Po zjištění zdroji dat prostřednictvím tradiční znamená, můžete zaregistrovat analytik, aby jeho snadněji zjistitelné jinými uživateli v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="7ab25-132">After discovering a data source through traditional means, an analyst can register it to make it more easily discoverable by other users in the future.</span></span> <span data-ttu-id="7ab25-133">I když analytika můžete přidat další hodnoty zadávání poznámek k registrované datové prostředky, není nutné provést ve stejnou dobu jako registrace tato anotace.</span><span class="sxs-lookup"><span data-stu-id="7ab25-133">Although the analyst can add more value by annotating the registered data assets, this annotation does not need to take place at the same time as registration.</span></span> <span data-ttu-id="7ab25-134">Uživatelé mohou přispívat v čase, jako jejich povolení plány postupně přidáním hodnoty do zdroje dat, které jsou zaregistrovány v katalogu.</span><span class="sxs-lookup"><span data-stu-id="7ab25-134">Users can contribute over time, as their schedules permit, gradually adding value to the data sources registered in the catalog.</span></span>

<span data-ttu-id="7ab25-135">Tato organickým růstem katalogu obsahu je přirozené doplněk k počáteční registrace centrálních zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="7ab25-135">This organic growth of the catalog content is a natural complement to the up-front registration of central data sources.</span></span> <span data-ttu-id="7ab25-136">Předem vyplní katalogu s daty, která bude nutné mnoho uživatelů může být motivator pro počáteční použití a zjišťování.</span><span class="sxs-lookup"><span data-stu-id="7ab25-136">Pre-populating the catalog with data that many users will need can be a motivator for initial use and discovery.</span></span> <span data-ttu-id="7ab25-137">Povolení uživatelům zaregistrujte a opatřete poznámkami další zdroje může být způsob, jak udržovat je i ostatním členům organizace pověření.</span><span class="sxs-lookup"><span data-stu-id="7ab25-137">Enabling users to register and annotate additional sources can be a way to keep them and other organization members engaged.</span></span>

<span data-ttu-id="7ab25-138">Je vhodné poznamenat, že i když tento scénář se zaměřuje konkrétně na BI samoobslužné služby, stejné vzory a problémy platí pro rozsáhlé podnikové BI projekty také.</span><span class="sxs-lookup"><span data-stu-id="7ab25-138">It’s worth noting that although this scenario focuses specifically on self-service BI, the same patterns and challenges apply to large-scale corporate BI projects as well.</span></span> <span data-ttu-id="7ab25-139">Pomocí katalogu Data Catalog vaší organizace může zvýšit žádné úsilí, který zahrnuje ruční proces zdroj dat zjišťování.</span><span class="sxs-lookup"><span data-stu-id="7ab25-139">By using Data Catalog, your organization can improve any effort that involves a manual process of data-source discovery.</span></span>

## <a name="scenario-3-capturing-tribal-knowledge"></a><span data-ttu-id="7ab25-140">Scénář 3: Zaznamenání kmenové znalosti</span><span class="sxs-lookup"><span data-stu-id="7ab25-140">Scenario 3: Capturing tribal knowledge</span></span>
<span data-ttu-id="7ab25-141">Jak poznáte, jaká data je nutné provést úlohu a kde najít data?</span><span class="sxs-lookup"><span data-stu-id="7ab25-141">How do you know what data you need to do your job, and where to find that data?</span></span>

<span data-ttu-id="7ab25-142">Pokud jste si v úloze nějakou dobu, je pravděpodobně právě znát.</span><span class="sxs-lookup"><span data-stu-id="7ab25-142">If you’ve been in your job for a while, you probably just know.</span></span> <span data-ttu-id="7ab25-143">Prošli postupné učení procesu a v čase se naučili o zdrojích dat, které jsou klíčem k každodenní práce.</span><span class="sxs-lookup"><span data-stu-id="7ab25-143">You’ve gone through a gradual learning process, and over time have learned about the data sources that are key to your day-to-day work.</span></span>

<span data-ttu-id="7ab25-144">V případě nového zaměstnance připojen váš tým, jak tato osoba vědět, jaká data je vyžadován pro úlohy a kde je najít?</span><span class="sxs-lookup"><span data-stu-id="7ab25-144">When a new employee joins your team, how does that person know what data is required for the job, and where to find it?</span></span>

<span data-ttu-id="7ab25-145">Pravděpodobné se nové osobě obsahuje vám tyto otázky.</span><span class="sxs-lookup"><span data-stu-id="7ab25-145">Odds are, the new person comes to you with these questions.</span></span>

<span data-ttu-id="7ab25-146">Tento probíhající přenos kmenové znalosti je součástí procesu zjišťování zdrojů dat v organizacích malé i velké.</span><span class="sxs-lookup"><span data-stu-id="7ab25-146">This ongoing transfer of tribal knowledge is part of the data-source discovery process in organizations large and small.</span></span> <span data-ttu-id="7ab25-147">Mít senior a zkušenosti členové týmu průběhu let vytvořená znalostní báze a novější členové týmu se naučili a požádat ho, když mají dotazy.</span><span class="sxs-lookup"><span data-stu-id="7ab25-147">More senior and experienced team members have built up knowledge over the years, and newer team members have learned to ask them when they have questions.</span></span> <span data-ttu-id="7ab25-148">Nejvíce podstatné informace často existuje pouze v vedoucích několik klíčových osob a když tyto osoby na dovolenou nebo nechte týmem, vyskytne organizace.</span><span class="sxs-lookup"><span data-stu-id="7ab25-148">The most vital information often exists only in the heads of a few key people, and when those people are on vacation or leave the team, the organization suffers.</span></span>

<span data-ttu-id="7ab25-149">Data odborníky normálně se snažit dokumentu své znalosti její sdílení e-mailem nebo do dokumentů aplikace Word na týmovém webu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="7ab25-149">Data experts ordinarily make an effort to document their knowledge, sharing it via email or in Word documents on a team SharePoint site.</span></span> <span data-ttu-id="7ab25-150">I když může být tento přístup, zavádí nové zjišťování problému: jak uživatelé zjistit, co dokumentace existuje a kde je najít?</span><span class="sxs-lookup"><span data-stu-id="7ab25-150">Although this approach can be valuable, it introduces a new discovery problem: how do people know what documentation exists, and where to find it?</span></span>

<span data-ttu-id="7ab25-151">S Azure Data Catalog má vaše organizace jednu centrální umístění pro ukládání a sdílení této kmenové znalosti a pro provedení snadno zjistitelné.</span><span class="sxs-lookup"><span data-stu-id="7ab25-151">With Azure Data Catalog, your organization has a single, central location for storing and sharing this tribal knowledge, and for making it easily discoverable.</span></span> <span data-ttu-id="7ab25-152">V katalogu Data Catalog může vaše data odborníky opatřit poznámkami datové prostředky a zadejte odkazy na stávající dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="7ab25-152">In Data Catalog, your data experts can annotate data assets directly and provide links to existing documentation.</span></span> <span data-ttu-id="7ab25-153">Pokud organizace členy pomocí katalogu můžete zjistit zdroj dat, budete najdou nejen zdroji sám sebe, ale také znalostní báze, který dříve existoval pouze s danou odborníky vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="7ab25-153">When organization members use the catalog to discover a data source, they'll find not only the source itself, but also the knowledge that previously existed only in the minds of your organization's experts.</span></span>