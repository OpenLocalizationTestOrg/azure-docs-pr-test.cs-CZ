---
title: "Integrované zabezpečení do Azure architektury návrhů | Microsoft Docs"
description: " Tento článek vám pomůže pochopit architektuře aplikace a služby v Azure, aby bylo snazší integrovat do návrh a implementaci zabezpečení. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 4f2d9386-bda3-4ec8-8b1a-cd0c11242ffc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: 91e46d690d3e7c298bc3b4020cc383ca99c43c4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="application-architecture-on-azure"></a><span data-ttu-id="7cbdb-103">Aplikační architektura v Azure</span><span class="sxs-lookup"><span data-stu-id="7cbdb-103">Application architecture on Azure</span></span>
<span data-ttu-id="7cbdb-104">K zabezpečení vašich cloudových řešení v Microsoft Azure, silné architektury foundation je velmi důležité.</span><span class="sxs-lookup"><span data-stu-id="7cbdb-104">To help secure your cloud-based solutions on Microsoft Azure, a strong architectural foundation is critical.</span></span> <span data-ttu-id="7cbdb-105">Architekti, Designer a implementátory informačních technologií využívat silné znalostní báze architektury aplikací a služeb.</span><span class="sxs-lookup"><span data-stu-id="7cbdb-105">Architects, designers, and implementers all benefit from a strong knowledge of application and services architecture.</span></span> <span data-ttu-id="7cbdb-106">Tyto základní znalosti pomáhá seznámení s komponentami cloudového řešení a usnadňují integraci zabezpečení do všech aspektů návrhu a implementace.</span><span class="sxs-lookup"><span data-stu-id="7cbdb-106">This foundational knowledge helps you understand all the components of your cloud-based solutions and make it easier to integrate security into all aspects of your design and implementation.</span></span>

<span data-ttu-id="7cbdb-107">Máme následující vám pomůžou s architektury vyšetřování a návrhy:</span><span class="sxs-lookup"><span data-stu-id="7cbdb-107">We have the following to help you with your architectural investigations and designs:</span></span>

* <span data-ttu-id="7cbdb-108">Architektury infographics</span><span class="sxs-lookup"><span data-stu-id="7cbdb-108">Architectural infographics</span></span>
* <span data-ttu-id="7cbdb-109">Architektury plány</span><span class="sxs-lookup"><span data-stu-id="7cbdb-109">Architectural blueprints</span></span>
* <span data-ttu-id="7cbdb-110">Cloudové a podnikové sadu symbolů</span><span class="sxs-lookup"><span data-stu-id="7cbdb-110">Cloud and enterprise symbol set</span></span>
* <span data-ttu-id="7cbdb-111">Šablona 3D plán, podle kterého aplikace Visio</span><span class="sxs-lookup"><span data-stu-id="7cbdb-111">3D blueprint Visio template</span></span>

## <a name="architectural-infographics"></a><span data-ttu-id="7cbdb-112">Architektury infographics</span><span class="sxs-lookup"><span data-stu-id="7cbdb-112">Architectural infographics</span></span>
<span data-ttu-id="7cbdb-113">Společnost Microsoft publikuje několik architektury související plakáty/infographics.</span><span class="sxs-lookup"><span data-stu-id="7cbdb-113">Microsoft publishes several architectural related posters/infographics.</span></span> <span data-ttu-id="7cbdb-114">Patří mezi ně:</span><span class="sxs-lookup"><span data-stu-id="7cbdb-114">They include:</span></span>

* [<span data-ttu-id="7cbdb-115">Vytváření reálných cloudových aplikací</span><span class="sxs-lookup"><span data-stu-id="7cbdb-115">Building Real-World Cloud Applications</span></span>](https://azure.microsoft.com/documentation/infographics/building-real-world-cloud-apps/)
* [<span data-ttu-id="7cbdb-116">Škálování s cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7cbdb-116">Scaling with Cloud Services</span></span>](https://azure.microsoft.com/documentation/infographics/cloud-services/)

## <a name="architectural-blueprints"></a><span data-ttu-id="7cbdb-117">Architektury plány</span><span class="sxs-lookup"><span data-stu-id="7cbdb-117">Architectural blueprints</span></span>
<span data-ttu-id="7cbdb-118">Společnost Microsoft publikuje sadu vysoké úrovně [architektury plány vyfotografovat](http://aka.ms/azblueprints) znázorňující způsob sestavení konkrétní typy systémů používajících produkty společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7cbdb-118">Microsoft publishes a set of high-level [architectural blueprints](http://aka.ms/azblueprints) showing how to build specific types of systems using Microsoft products.</span></span>
<span data-ttu-id="7cbdb-119">Každý plán, podle kterého zahrnuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="7cbdb-119">Each blueprint includes a:</span></span>

* <span data-ttu-id="7cbdb-120">Ploché 2D na základě Visio 2003 soubor, který si můžete stáhnout a upravit</span><span class="sxs-lookup"><span data-stu-id="7cbdb-120">Flat 2D Visio 2003-based file that you can download and modify</span></span>
* <span data-ttu-id="7cbdb-121">Barevné 3D perspektivy soubor PDF plán, podle kterého představit méně technické cílové skupiny</span><span class="sxs-lookup"><span data-stu-id="7cbdb-121">Colorful 3D perspective PDF file to introduce the blueprint to less technical audiences</span></span>
* <span data-ttu-id="7cbdb-122">Video, které provede 3D verze</span><span class="sxs-lookup"><span data-stu-id="7cbdb-122">Video that walks through the 3D version</span></span>

## <a name="cloud-and-enterprise-symbol-set"></a><span data-ttu-id="7cbdb-123">Cloudové a podnikové sadu symbolů</span><span class="sxs-lookup"><span data-stu-id="7cbdb-123">Cloud and enterprise symbol set</span></span>
<span data-ttu-id="7cbdb-124">[Zobrazit aplikace Visio a symboly cvičení video](http://aka.ms/CnESymbolsVideo) a potom [stáhnout cloudu a nastavte Enterprise Symbol](http://aka.ms/CnESymbols) k usnadnění vytváření technické materiály, které popisují Azure, Windows Server, SQL Server a další.</span><span class="sxs-lookup"><span data-stu-id="7cbdb-124">[View the Visio and symbols training video](http://aka.ms/CnESymbolsVideo) and then [download the Cloud and Enterprise Symbol set](http://aka.ms/CnESymbols) to help create technical materials that describe Azure, Windows Server, SQL Server and more.</span></span> <span data-ttu-id="7cbdb-125">Symboly v architektuře diagramy, školicí materiály, prezentace, datové listy, infographics, dokumenty White Paper a i třetích stran knih můžete použít, pokud knihy soupravy osoby používat produkty společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7cbdb-125">You can use the symbols in architecture diagrams, training materials, presentations, datasheets, infographics, whitepapers, and even third party books if the book trains people to use Microsoft products.</span></span> <span data-ttu-id="7cbdb-126">Však nejsou určeny pro použití v uživatelská rozhraní.</span><span class="sxs-lookup"><span data-stu-id="7cbdb-126">However, they are not meant for use in user interfaces.</span></span>

## <a name="3d-blueprint-visio-template"></a><span data-ttu-id="7cbdb-127">Šablona 3D plán, podle kterého aplikace Visio</span><span class="sxs-lookup"><span data-stu-id="7cbdb-127">3D blueprint Visio template</span></span>
<span data-ttu-id="7cbdb-128">3D verze [plány vyfotografovat Architektura Microsoft](http://aka.ms/azblueprints) byla původně vytvořena v nástroji jiných společností než Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7cbdb-128">The 3D versions of the [Microsoft Architecture Blueprints](http://aka.ms/azblueprints) were initially created in a non-Microsoft tool.</span></span> <span data-ttu-id="7cbdb-129">Novou šablonu aplikace Visio 2013 (nebo novější) dodaný 5 srpna 2015 jako součást [Microsoft Architecture certifikační průběhu distribuován na EDX.ORG](https://docs.microsoft.com/azure/architecture/#microsoft-architecture-certification-course).</span><span class="sxs-lookup"><span data-stu-id="7cbdb-129">A new Visio 2013 (and later) template shipped on August 5, 2015 as part of a [Microsoft Architecture certification course distributed on EDX.ORG](https://docs.microsoft.com/azure/architecture/#microsoft-architecture-certification-course).</span></span>

<span data-ttu-id="7cbdb-130">Dále je dostupný mimo během šablony.</span><span class="sxs-lookup"><span data-stu-id="7cbdb-130">The template is also available outside the course.</span></span>

* <span data-ttu-id="7cbdb-131">[Zobrazit video školení](http://aka.ms/3dBlueprintTemplateVideo) první, abyste věděli, co můžete dělat</span><span class="sxs-lookup"><span data-stu-id="7cbdb-131">[View the training video](http://aka.ms/3dBlueprintTemplateVideo) first so you know what it can do</span></span>
* <span data-ttu-id="7cbdb-132">Stažení [Microsoft 3d šablony plán, podle kterého aplikace Visio](http://aka.ms/3DBlueprintTemplate)</span><span class="sxs-lookup"><span data-stu-id="7cbdb-132">Download the [Microsoft 3d Blueprint Visio Template](http://aka.ms/3DBlueprintTemplate)</span></span>
* <span data-ttu-id="7cbdb-133">Stažení [cloudové a podnikové symboly](https://docs.microsoft.com/azure/architecture/#drawing-symbol-and-icon-sets) pro použití s 3D šablony</span><span class="sxs-lookup"><span data-stu-id="7cbdb-133">Download the [Cloud and Enterprise Symbols](https://docs.microsoft.com/azure/architecture/#drawing-symbol-and-icon-sets) to use with the 3D template</span></span>
