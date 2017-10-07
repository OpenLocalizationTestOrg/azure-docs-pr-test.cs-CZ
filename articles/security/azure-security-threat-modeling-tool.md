---
title: "aaaMicrosoft nástroj pro modelování hrozeb – Azure | Microsoft Docs"
description: "Hlavní stránka pro hello Microsoft Threat modelování nástroj, obsahující informace o zahájení práce s nástrojem hello včetně procesu modelování hrozeb hello"
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
ms.openlocfilehash: 923225a30c592ffdb1d254000451cfaac54a0e68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-threat-modeling-tool"></a><span data-ttu-id="6b1e1-103">Nástroj Microsoft Threat modelování</span><span class="sxs-lookup"><span data-stu-id="6b1e1-103">Microsoft Threat Modeling Tool</span></span>

<span data-ttu-id="6b1e1-104">Hello nástroj modelování hrozeb je element jádra systému hello Microsoft životního cyklu SDL (Security Development).</span><span class="sxs-lookup"><span data-stu-id="6b1e1-104">hello Threat Modeling Tool is a core element of hello Microsoft Security Development Lifecycle (SDL).</span></span> <span data-ttu-id="6b1e1-105">Umožňuje softwaru architects tooidentify a zmírnit potenciální potíže se zabezpečením již v rané fázi, když jsou poměrně jednoduché a nákladově efektivní tooresolve.</span><span class="sxs-lookup"><span data-stu-id="6b1e1-105">It allows software architects tooidentify and mitigate potential security issues early, when they are relatively easy and cost-effective tooresolve.</span></span> <span data-ttu-id="6b1e1-106">V důsledku toho výrazně snižuje hello celkové náklady na vývoj.</span><span class="sxs-lookup"><span data-stu-id="6b1e1-106">As a result, it greatly reduces hello total cost of development.</span></span> <span data-ttu-id="6b1e1-107">Také jsme chtěli hello nástroj s odborníky nesouvisející se zabezpečením na paměti, snadněji modelování hrozeb pro všechny vývojáře tím, že poskytuje jasné pokyny o vytvoření a analýza hrozeb modelů.</span><span class="sxs-lookup"><span data-stu-id="6b1e1-107">Also, we designed hello tool with non-security experts in mind, making threat modeling easier for all developers by providing clear guidance on creating and analyzing threat models.</span></span> 

<span data-ttu-id="6b1e1-108">Hello nástroj umožňuje všem uživatelům:</span><span class="sxs-lookup"><span data-stu-id="6b1e1-108">hello tool enables anyone to:</span></span>

* <span data-ttu-id="6b1e1-109">Komunikaci o návrhu hello zabezpečení svých počítačů</span><span class="sxs-lookup"><span data-stu-id="6b1e1-109">Communicate about hello security design of their systems</span></span>
* <span data-ttu-id="6b1e1-110">Analýza těchto návrhů pro použití Principy metodika potenciální potíže se zabezpečením</span><span class="sxs-lookup"><span data-stu-id="6b1e1-110">Analyze those designs for potential security issues using a proven methodology</span></span>
* <span data-ttu-id="6b1e1-111">Navrhněte a spravovat jejich zmírnění pro problémy se zabezpečením</span><span class="sxs-lookup"><span data-stu-id="6b1e1-111">Suggest and manage mitigations for security issues</span></span>

<span data-ttu-id="6b1e1-112">Tady jsou některé možnosti nástrojů a inovací, jenom tooname několik:</span><span class="sxs-lookup"><span data-stu-id="6b1e1-112">Here are some tooling capabilities and innovations, just tooname a few:</span></span>

* <span data-ttu-id="6b1e1-113">**Automatizace:** pokyny a zpětné vazby v kreslení modelu</span><span class="sxs-lookup"><span data-stu-id="6b1e1-113">**Automation:** Guidance and feedback in drawing a model</span></span>
* <span data-ttu-id="6b1e1-114">**STRIDE za Element:** na základě analýzy hrozeb a jejich zmírnění</span><span class="sxs-lookup"><span data-stu-id="6b1e1-114">**STRIDE per Element:** Guided analysis of threats and mitigations</span></span>
* <span data-ttu-id="6b1e1-115">**Vytváření sestav:** aktivity související se zabezpečením a testování ve fázi ověření hello</span><span class="sxs-lookup"><span data-stu-id="6b1e1-115">**Reporting:** Security activities and testing in hello verification phase</span></span>
* <span data-ttu-id="6b1e1-116">**Jedinečný metodika:** toobetter uživatele povoluje vizualizaci a porozumění hrozeb</span><span class="sxs-lookup"><span data-stu-id="6b1e1-116">**Unique Methodology:** Enables users toobetter visualize and understand threats</span></span>
* <span data-ttu-id="6b1e1-117">**Určený pro vývojáře a na střed na Software:** na prostředky nebo útočníci střed mnoho přístupů.</span><span class="sxs-lookup"><span data-stu-id="6b1e1-117">**Designed for Developers and Centered on Software:** many approaches are centered on assets or attackers.</span></span> <span data-ttu-id="6b1e1-118">Jsme se jedná o software.</span><span class="sxs-lookup"><span data-stu-id="6b1e1-118">We are centered on software.</span></span> <span data-ttu-id="6b1e1-119">Máme zaměřit na aktivity, které všechny vývojáři softwaru a architektům jsou obeznámeni s – například vykreslování obrázků pro jejich architektura softwaru</span><span class="sxs-lookup"><span data-stu-id="6b1e1-119">We build on activities that all software developers and architects are familiar with -- such as drawing pictures for their software architecture</span></span>
* <span data-ttu-id="6b1e1-120">**Zaměřuje na návrh analýzy:** hello termín "modelování hrozeb" může označovat tooeither požadavky nebo techniku analysis návrhu.</span><span class="sxs-lookup"><span data-stu-id="6b1e1-120">**Focused on Design Analysis:** hello term "threat modeling" can refer tooeither a requirements or a design analysis technique.</span></span> <span data-ttu-id="6b1e1-121">V některých případech odkazuje tooa komplexní kombinací hello dva.</span><span class="sxs-lookup"><span data-stu-id="6b1e1-121">Sometimes, it refers tooa complex blend of hello two.</span></span> <span data-ttu-id="6b1e1-122">Hello Microsoft SDL přístup toothreat modelování je technika analysis cílených návrhu</span><span class="sxs-lookup"><span data-stu-id="6b1e1-122">hello Microsoft SDL approach toothreat modeling is a focused design analysis technique</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b1e1-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b1e1-123">Next steps</span></span>

<span data-ttu-id="6b1e1-124">Hello následující tabulka obsahuje odkazy na důležité tooget, které jste začali s hello nástroj modelování hrozeb:</span><span class="sxs-lookup"><span data-stu-id="6b1e1-124">hello table below contains important links tooget you started with hello Threat Modeling Tool:</span></span>

| <span data-ttu-id="6b1e1-125">Krok</span><span class="sxs-lookup"><span data-stu-id="6b1e1-125">Step</span></span>  | <span data-ttu-id="6b1e1-126">Popis</span><span class="sxs-lookup"><span data-stu-id="6b1e1-126">Description</span></span>                                                                                   |
| ----- | --------------------------------------------------------------------------------------------- |
| <span data-ttu-id="6b1e1-127">**1**</span><span class="sxs-lookup"><span data-stu-id="6b1e1-127">**1**</span></span> | [<span data-ttu-id="6b1e1-128">Stáhněte si nástroj modelování hrozeb hello</span><span class="sxs-lookup"><span data-stu-id="6b1e1-128">Download hello Threat Modeling Tool</span></span>](https://aka.ms/tmtpreview)                                |
| <span data-ttu-id="6b1e1-129">**2**</span><span class="sxs-lookup"><span data-stu-id="6b1e1-129">**2**</span></span> | [<span data-ttu-id="6b1e1-130">Přečtěte si, že naše Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="6b1e1-130">Read Our getting started guide</span></span>](./azure-security-threat-modeling-tool-getting-started.md)    |
| <span data-ttu-id="6b1e1-131">**3**</span><span class="sxs-lookup"><span data-stu-id="6b1e1-131">**3**</span></span> | [<span data-ttu-id="6b1e1-132">Seznamte se s funkcí hello</span><span class="sxs-lookup"><span data-stu-id="6b1e1-132">Get familiar with hello features</span></span>](./azure-security-threat-modeling-tool-feature-overview.md)   |
| <span data-ttu-id="6b1e1-133">**4**</span><span class="sxs-lookup"><span data-stu-id="6b1e1-133">**4**</span></span> | [<span data-ttu-id="6b1e1-134">Další informace o generovaného threat kategorií</span><span class="sxs-lookup"><span data-stu-id="6b1e1-134">Learn about generated threat categories</span></span>](./azure-security-threat-modeling-tool-threats.md)   |
| <span data-ttu-id="6b1e1-135">**5**</span><span class="sxs-lookup"><span data-stu-id="6b1e1-135">**5**</span></span> | [<span data-ttu-id="6b1e1-136">Najít jejich zmírnění hrozeb toogenerated</span><span class="sxs-lookup"><span data-stu-id="6b1e1-136">Find mitigations toogenerated threats</span></span>](./azure-security-threat-modeling-tool-mitigations.md) |

## <a name="resources"></a><span data-ttu-id="6b1e1-137">Zdroje</span><span class="sxs-lookup"><span data-stu-id="6b1e1-137">Resources</span></span>

<span data-ttu-id="6b1e1-138">Tady je několik starší články stále relevantní toothreat modelování dnes:</span><span class="sxs-lookup"><span data-stu-id="6b1e1-138">Here are a few older articles still relevant toothreat modeling today:</span></span>

* [<span data-ttu-id="6b1e1-139">Článek na hello důležitosti modelování hrozeb</span><span class="sxs-lookup"><span data-stu-id="6b1e1-139">Article on hello Importance of Threat Modeling</span></span>](https://msdn.microsoft.com/magazine/dd347831.aspx)
* [<span data-ttu-id="6b1e1-140">Školení publikovaná Trustworthy Computing připravila</span><span class="sxs-lookup"><span data-stu-id="6b1e1-140">Training Published by Trustworthy Computing</span></span>](https://www.microsoft.com/download/details.aspx?id=16420)

<span data-ttu-id="6b1e1-141">Podívejte se na několik odborníky nástroj modelování hrozeb, které je dokončili:</span><span class="sxs-lookup"><span data-stu-id="6b1e1-141">Check out what a few Threat Modeling Tool experts have done:</span></span>

* [<span data-ttu-id="6b1e1-142">Správce hrozeb</span><span class="sxs-lookup"><span data-stu-id="6b1e1-142">Threats Manager</span></span>](https://simoneonsecurity.com/threatsmanagersetup-v1-5-10/)
* [<span data-ttu-id="6b1e1-143">Blog Simone Curzi zabezpečení</span><span class="sxs-lookup"><span data-stu-id="6b1e1-143">Simone Curzi Security Blog</span></span>](https://simoneonsecurity.com/)