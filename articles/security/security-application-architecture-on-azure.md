---
title: "zabezpečení aaaIntegrate do Azure architektury návrhů | Microsoft Docs"
description: " Tento článek vám pomůže porozumět hello architektura aplikací a služeb na Azure toomake je snazší toointegrate zabezpečení do návrhu a implementace. "
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
ms.openlocfilehash: cfca8a1a2766f72bc3340c4e3df0019eb8b5a1e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-architecture-on-azure"></a><span data-ttu-id="1f55a-103">Aplikační architektura v Azure</span><span class="sxs-lookup"><span data-stu-id="1f55a-103">Application architecture on Azure</span></span>
<span data-ttu-id="1f55a-104">toohelp zabezpečit vaše cloudové řešení v Microsoft Azure, silné architektury foundation je velmi důležité.</span><span class="sxs-lookup"><span data-stu-id="1f55a-104">toohelp secure your cloud-based solutions on Microsoft Azure, a strong architectural foundation is critical.</span></span> <span data-ttu-id="1f55a-105">Architekti, Designer a implementátory informačních technologií využívat silné znalostní báze architektury aplikací a služeb.</span><span class="sxs-lookup"><span data-stu-id="1f55a-105">Architects, designers, and implementers all benefit from a strong knowledge of application and services architecture.</span></span> <span data-ttu-id="1f55a-106">Tyto základní znalosti vám pomůže pochopit všechny součásti hello cloudového řešení a bylo snazší toointegrate zabezpečení do všech aspektů návrhu a implementace.</span><span class="sxs-lookup"><span data-stu-id="1f55a-106">This foundational knowledge helps you understand all hello components of your cloud-based solutions and make it easier toointegrate security into all aspects of your design and implementation.</span></span>

<span data-ttu-id="1f55a-107">Budeme mít hello následující toohelp můžete s architektury vyšetřování a návrhy:</span><span class="sxs-lookup"><span data-stu-id="1f55a-107">We have hello following toohelp you with your architectural investigations and designs:</span></span>

* <span data-ttu-id="1f55a-108">Architektury infographics</span><span class="sxs-lookup"><span data-stu-id="1f55a-108">Architectural infographics</span></span>
* <span data-ttu-id="1f55a-109">Architektury plány</span><span class="sxs-lookup"><span data-stu-id="1f55a-109">Architectural blueprints</span></span>
* <span data-ttu-id="1f55a-110">Cloudové a podnikové sadu symbolů</span><span class="sxs-lookup"><span data-stu-id="1f55a-110">Cloud and enterprise symbol set</span></span>
* <span data-ttu-id="1f55a-111">Šablona 3D plán, podle kterého aplikace Visio</span><span class="sxs-lookup"><span data-stu-id="1f55a-111">3D blueprint Visio template</span></span>

## <a name="architectural-infographics"></a><span data-ttu-id="1f55a-112">Architektury infographics</span><span class="sxs-lookup"><span data-stu-id="1f55a-112">Architectural infographics</span></span>
<span data-ttu-id="1f55a-113">Společnost Microsoft publikuje několik architektury související plakáty/infographics.</span><span class="sxs-lookup"><span data-stu-id="1f55a-113">Microsoft publishes several architectural related posters/infographics.</span></span> <span data-ttu-id="1f55a-114">Patří mezi ně:</span><span class="sxs-lookup"><span data-stu-id="1f55a-114">They include:</span></span>

* [<span data-ttu-id="1f55a-115">Vytváření reálných cloudových aplikací</span><span class="sxs-lookup"><span data-stu-id="1f55a-115">Building Real-World Cloud Applications</span></span>](https://azure.microsoft.com/documentation/infographics/building-real-world-cloud-apps/)
* [<span data-ttu-id="1f55a-116">Škálování s cloudové služby</span><span class="sxs-lookup"><span data-stu-id="1f55a-116">Scaling with Cloud Services</span></span>](https://azure.microsoft.com/documentation/infographics/cloud-services/)

## <a name="architectural-blueprints"></a><span data-ttu-id="1f55a-117">Architektury plány</span><span class="sxs-lookup"><span data-stu-id="1f55a-117">Architectural blueprints</span></span>
<span data-ttu-id="1f55a-118">Společnost Microsoft publikuje sadu vysoké úrovně [architektury plány vyfotografovat](http://aka.ms/azblueprints) zobrazující jak toobuild konkrétní typy systémů používajících produkty společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1f55a-118">Microsoft publishes a set of high-level [architectural blueprints](http://aka.ms/azblueprints) showing how toobuild specific types of systems using Microsoft products.</span></span>
<span data-ttu-id="1f55a-119">Každý plán, podle kterého zahrnuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="1f55a-119">Each blueprint includes a:</span></span>

* <span data-ttu-id="1f55a-120">Ploché 2D na základě Visio 2003 soubor, který si můžete stáhnout a upravit</span><span class="sxs-lookup"><span data-stu-id="1f55a-120">Flat 2D Visio 2003-based file that you can download and modify</span></span>
* <span data-ttu-id="1f55a-121">Barevné 3D perspektivy PDF soubor toointroduce hello plán, podle kterého tooless technické cílové skupiny</span><span class="sxs-lookup"><span data-stu-id="1f55a-121">Colorful 3D perspective PDF file toointroduce hello blueprint tooless technical audiences</span></span>
* <span data-ttu-id="1f55a-122">Video, které provede hello 3D verze</span><span class="sxs-lookup"><span data-stu-id="1f55a-122">Video that walks through hello 3D version</span></span>

## <a name="cloud-and-enterprise-symbol-set"></a><span data-ttu-id="1f55a-123">Cloudové a podnikové sadu symbolů</span><span class="sxs-lookup"><span data-stu-id="1f55a-123">Cloud and enterprise symbol set</span></span>
<span data-ttu-id="1f55a-124">[Zobrazit hello Visio a symboly cvičení video](http://aka.ms/CnESymbolsVideo) a potom [stáhnout sadu cloudové a podnikové Symbol hello](http://aka.ms/CnESymbols) toohelp vytvořit technické materiály, které popisují Azure, Windows Server, SQL Server a další.</span><span class="sxs-lookup"><span data-stu-id="1f55a-124">[View hello Visio and symbols training video](http://aka.ms/CnESymbolsVideo) and then [download hello Cloud and Enterprise Symbol set](http://aka.ms/CnESymbols) toohelp create technical materials that describe Azure, Windows Server, SQL Server and more.</span></span> <span data-ttu-id="1f55a-125">Pokud hello kniha vlaky uživatelé toouse produkty společnosti Microsoft můžete použít hello symboly v architektuře diagramy, školicí materiály, prezentace, datové listy, infographics, dokumenty White Paper a i knih třetích stran.</span><span class="sxs-lookup"><span data-stu-id="1f55a-125">You can use hello symbols in architecture diagrams, training materials, presentations, datasheets, infographics, whitepapers, and even third party books if hello book trains people toouse Microsoft products.</span></span> <span data-ttu-id="1f55a-126">Však nejsou určeny pro použití v uživatelská rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1f55a-126">However, they are not meant for use in user interfaces.</span></span>

## <a name="3d-blueprint-visio-template"></a><span data-ttu-id="1f55a-127">Šablona 3D plán, podle kterého aplikace Visio</span><span class="sxs-lookup"><span data-stu-id="1f55a-127">3D blueprint Visio template</span></span>
<span data-ttu-id="1f55a-128">Hello 3D verzích hello [plány vyfotografovat Architektura Microsoft](http://aka.ms/azblueprints) byla původně vytvořena v nástroji jiných společností než Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1f55a-128">hello 3D versions of hello [Microsoft Architecture Blueprints](http://aka.ms/azblueprints) were initially created in a non-Microsoft tool.</span></span> <span data-ttu-id="1f55a-129">Novou šablonu aplikace Visio 2013 (nebo novější) dodaný 5 srpna 2015 jako součást [Microsoft Architecture certifikační průběhu distribuován na EDX.ORG](https://docs.microsoft.com/azure/architecture/#microsoft-architecture-certification-course).</span><span class="sxs-lookup"><span data-stu-id="1f55a-129">A new Visio 2013 (and later) template shipped on August 5, 2015 as part of a [Microsoft Architecture certification course distributed on EDX.ORG](https://docs.microsoft.com/azure/architecture/#microsoft-architecture-certification-course).</span></span>

<span data-ttu-id="1f55a-130">Šablona Hello je také k dispozici mimo hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="1f55a-130">hello template is also available outside hello course.</span></span>

* <span data-ttu-id="1f55a-131">[Zobrazit hello školení video](http://aka.ms/3dBlueprintTemplateVideo) první, abyste věděli, co můžete dělat</span><span class="sxs-lookup"><span data-stu-id="1f55a-131">[View hello training video](http://aka.ms/3dBlueprintTemplateVideo) first so you know what it can do</span></span>
* <span data-ttu-id="1f55a-132">Stáhnout hello [Microsoft 3d šablony plán, podle kterého aplikace Visio](http://aka.ms/3DBlueprintTemplate)</span><span class="sxs-lookup"><span data-stu-id="1f55a-132">Download hello [Microsoft 3d Blueprint Visio Template](http://aka.ms/3DBlueprintTemplate)</span></span>
* <span data-ttu-id="1f55a-133">Stáhnout hello [cloudové a podnikové symboly](https://docs.microsoft.com/azure/architecture/#drawing-symbol-and-icon-sets) toouse pomocí šablony 3D hello</span><span class="sxs-lookup"><span data-stu-id="1f55a-133">Download hello [Cloud and Enterprise Symbols](https://docs.microsoft.com/azure/architecture/#drawing-symbol-and-icon-sets) toouse with hello 3D template</span></span>
