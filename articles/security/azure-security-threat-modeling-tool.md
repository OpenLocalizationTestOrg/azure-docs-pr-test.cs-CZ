---
title: "Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "Hlavní stránka pro Microsoft Threat modelování nástroj, který obsahuje informace o Začínáme s nástroji včetně procesu modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 6e26b0af2a16a872c8e02b736e24019b47ed5780
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="microsoft-threat-modeling-tool"></a><span data-ttu-id="f4712-103">Nástroj Microsoft Threat modelování</span><span class="sxs-lookup"><span data-stu-id="f4712-103">Microsoft Threat Modeling Tool</span></span>

<span data-ttu-id="f4712-104">Nástroj modelování hrozeb je element základní životního cyklu pro vývoj zabezpečení Microsoft (SDL).</span><span class="sxs-lookup"><span data-stu-id="f4712-104">The Threat Modeling Tool is a core element of the Microsoft Security Development Lifecycle (SDL).</span></span> <span data-ttu-id="f4712-105">To umožňuje architekty softwaru k identifikaci a zmírnit potenciální potíže se zabezpečením již v rané fázi, když jsou poměrně jednoduché a nákladově efektivní řešení.</span><span class="sxs-lookup"><span data-stu-id="f4712-105">It allows software architects to identify and mitigate potential security issues early, when they are relatively easy and cost-effective to resolve.</span></span> <span data-ttu-id="f4712-106">V důsledku toho výrazně snižuje celkové náklady na vývoj.</span><span class="sxs-lookup"><span data-stu-id="f4712-106">As a result, it greatly reduces the total cost of development.</span></span> <span data-ttu-id="f4712-107">Také jsme chtěli nástroj s odborníky nesouvisející se zabezpečením na paměti, snadněji modelování hrozeb pro všechny vývojáře tím, že poskytuje jasné pokyny o vytvoření a analýza hrozeb modelů.</span><span class="sxs-lookup"><span data-stu-id="f4712-107">Also, we designed the tool with non-security experts in mind, making threat modeling easier for all developers by providing clear guidance on creating and analyzing threat models.</span></span> 

<span data-ttu-id="f4712-108">Tento nástroj umožňuje všem uživatelům:</span><span class="sxs-lookup"><span data-stu-id="f4712-108">The tool enables anyone to:</span></span>

* <span data-ttu-id="f4712-109">Komunikaci o návrhu zabezpečení svých počítačů</span><span class="sxs-lookup"><span data-stu-id="f4712-109">Communicate about the security design of their systems</span></span>
* <span data-ttu-id="f4712-110">Analýza těchto návrhů pro použití Principy metodika potenciální potíže se zabezpečením</span><span class="sxs-lookup"><span data-stu-id="f4712-110">Analyze those designs for potential security issues using a proven methodology</span></span>
* <span data-ttu-id="f4712-111">Navrhněte a spravovat jejich zmírnění pro problémy se zabezpečením</span><span class="sxs-lookup"><span data-stu-id="f4712-111">Suggest and manage mitigations for security issues</span></span>

<span data-ttu-id="f4712-112">Tady jsou některé možnosti nástrojů a inovací, jenom na název několik:</span><span class="sxs-lookup"><span data-stu-id="f4712-112">Here are some tooling capabilities and innovations, just to name a few:</span></span>

* <span data-ttu-id="f4712-113">**Automatizace:** pokyny a zpětné vazby v kreslení modelu</span><span class="sxs-lookup"><span data-stu-id="f4712-113">**Automation:** Guidance and feedback in drawing a model</span></span>
* <span data-ttu-id="f4712-114">**STRIDE za Element:** na základě analýzy hrozeb a jejich zmírnění</span><span class="sxs-lookup"><span data-stu-id="f4712-114">**STRIDE per Element:** Guided analysis of threats and mitigations</span></span>
* <span data-ttu-id="f4712-115">**Vytváření sestav:** aktivity související se zabezpečením a testování ve fázi ověření</span><span class="sxs-lookup"><span data-stu-id="f4712-115">**Reporting:** Security activities and testing in the verification phase</span></span>
* <span data-ttu-id="f4712-116">**Jedinečný metodika:** umožňuje uživatelům lepší vizualizaci a porozumění hrozeb</span><span class="sxs-lookup"><span data-stu-id="f4712-116">**Unique Methodology:** Enables users to better visualize and understand threats</span></span>
* <span data-ttu-id="f4712-117">**Určený pro vývojáře a na střed na Software:** na prostředky nebo útočníci střed mnoho přístupů.</span><span class="sxs-lookup"><span data-stu-id="f4712-117">**Designed for Developers and Centered on Software:** many approaches are centered on assets or attackers.</span></span> <span data-ttu-id="f4712-118">Jsme se jedná o software.</span><span class="sxs-lookup"><span data-stu-id="f4712-118">We are centered on software.</span></span> <span data-ttu-id="f4712-119">Máme zaměřit na aktivity, které všechny vývojáři softwaru a architektům jsou obeznámeni s – například vykreslování obrázků pro jejich architektura softwaru</span><span class="sxs-lookup"><span data-stu-id="f4712-119">We build on activities that all software developers and architects are familiar with -- such as drawing pictures for their software architecture</span></span>
* <span data-ttu-id="f4712-120">**Zaměřuje na návrh analýzy:** termín "modelování hrozeb" mohou odkazovat na požadavky nebo techniku analysis návrhu.</span><span class="sxs-lookup"><span data-stu-id="f4712-120">**Focused on Design Analysis:** The term "threat modeling" can refer to either a requirements or a design analysis technique.</span></span> <span data-ttu-id="f4712-121">V některých případech odkazuje na komplexní blend dvou.</span><span class="sxs-lookup"><span data-stu-id="f4712-121">Sometimes, it refers to a complex blend of the two.</span></span> <span data-ttu-id="f4712-122">Microsoft SDL přístup k modelování hrozeb je technika analysis cílených návrhu</span><span class="sxs-lookup"><span data-stu-id="f4712-122">The Microsoft SDL approach to threat modeling is a focused design analysis technique</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4712-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4712-123">Next steps</span></span>

<span data-ttu-id="f4712-124">Následující tabulka obsahuje důležité odkazy, které vám pomůžou začít s nástrojem modelování hrozeb:</span><span class="sxs-lookup"><span data-stu-id="f4712-124">The table below contains important links to get you started with the Threat Modeling Tool:</span></span>

| <span data-ttu-id="f4712-125">Krok</span><span class="sxs-lookup"><span data-stu-id="f4712-125">Step</span></span>  | <span data-ttu-id="f4712-126">Popis</span><span class="sxs-lookup"><span data-stu-id="f4712-126">Description</span></span>                                                                                   |
| ----- | --------------------------------------------------------------------------------------------- |
| <span data-ttu-id="f4712-127">**1**</span><span class="sxs-lookup"><span data-stu-id="f4712-127">**1**</span></span> | [<span data-ttu-id="f4712-128">Stáhnout riziko, že nástroj modelování</span><span class="sxs-lookup"><span data-stu-id="f4712-128">Download the Threat Modeling Tool</span></span>](https://aka.ms/tmtpreview)                                |
| <span data-ttu-id="f4712-129">**2**</span><span class="sxs-lookup"><span data-stu-id="f4712-129">**2**</span></span> | [<span data-ttu-id="f4712-130">Přečtěte si, že naše Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="f4712-130">Read Our getting started guide</span></span>](./azure-security-threat-modeling-tool-getting-started.md)    |
| <span data-ttu-id="f4712-131">**3**</span><span class="sxs-lookup"><span data-stu-id="f4712-131">**3**</span></span> | [<span data-ttu-id="f4712-132">Seznamte se s funkcí</span><span class="sxs-lookup"><span data-stu-id="f4712-132">Get familiar with the features</span></span>](./azure-security-threat-modeling-tool-feature-overview.md)   |
| <span data-ttu-id="f4712-133">**4**</span><span class="sxs-lookup"><span data-stu-id="f4712-133">**4**</span></span> | [<span data-ttu-id="f4712-134">Další informace o generovaného threat kategorií</span><span class="sxs-lookup"><span data-stu-id="f4712-134">Learn about generated threat categories</span></span>](./azure-security-threat-modeling-tool-threats.md)   |
| <span data-ttu-id="f4712-135">**5**</span><span class="sxs-lookup"><span data-stu-id="f4712-135">**5**</span></span> | [<span data-ttu-id="f4712-136">Najít jejich zmírnění generovaného hrozby</span><span class="sxs-lookup"><span data-stu-id="f4712-136">Find mitigations to generated threats</span></span>](./azure-security-threat-modeling-tool-mitigations.md) |

## <a name="resources"></a><span data-ttu-id="f4712-137">Zdroje</span><span class="sxs-lookup"><span data-stu-id="f4712-137">Resources</span></span>

<span data-ttu-id="f4712-138">Zde jsou stále relevantní pro modelování hrozeb ještě dnes několik starší články:</span><span class="sxs-lookup"><span data-stu-id="f4712-138">Here are a few older articles still relevant to threat modeling today:</span></span>

* [<span data-ttu-id="f4712-139">V článku na významu modelování hrozeb</span><span class="sxs-lookup"><span data-stu-id="f4712-139">Article on the Importance of Threat Modeling</span></span>](https://msdn.microsoft.com/magazine/dd347831.aspx)
* [<span data-ttu-id="f4712-140">Školení publikovaná Trustworthy Computing připravila</span><span class="sxs-lookup"><span data-stu-id="f4712-140">Training Published by Trustworthy Computing</span></span>](https://www.microsoft.com/download/details.aspx?id=16420)

<span data-ttu-id="f4712-141">Podívejte se na několik odborníky nástroj modelování hrozeb, které je dokončili:</span><span class="sxs-lookup"><span data-stu-id="f4712-141">Check out what a few Threat Modeling Tool experts have done:</span></span>

* [<span data-ttu-id="f4712-142">Správce hrozeb</span><span class="sxs-lookup"><span data-stu-id="f4712-142">Threats Manager</span></span>](https://simoneonsecurity.com/threatsmanagersetup-v1-5-10/)
* [<span data-ttu-id="f4712-143">Blog Simone Curzi zabezpečení</span><span class="sxs-lookup"><span data-stu-id="f4712-143">Simone Curzi Security Blog</span></span>](https://simoneonsecurity.com/)