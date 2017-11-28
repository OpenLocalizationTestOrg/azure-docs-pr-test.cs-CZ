---
title: "aaaSubscription a účet pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro předplatných a účtů v Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19343826-7eef-42a1-98be-4ec65b0f377a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9025a40783c008310ebd0f674deb4a9001ae974a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-linux-vms"></a><span data-ttu-id="95ed7-103">Azure předplatného a účty pokyny pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="95ed7-103">Azure subscription and accounts guidelines for Linux VMs</span></span>

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="95ed7-104">Tento článek se zaměřuje na pochopení, jak se zvětšuje tooapproach předplatné a účet správy jako vaše prostředí a uživatelské základny.</span><span class="sxs-lookup"><span data-stu-id="95ed7-104">This article focuses on understanding how tooapproach subscription and account management as your environment and user base grows.</span></span>

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a><span data-ttu-id="95ed7-105">Postup implementace pro předplatných a účtů</span><span class="sxs-lookup"><span data-stu-id="95ed7-105">Implementation guidelines for subscriptions and accounts</span></span>
<span data-ttu-id="95ed7-106">Rozhodnutí:</span><span class="sxs-lookup"><span data-stu-id="95ed7-106">Decisions:</span></span>

* <span data-ttu-id="95ed7-107">Jaká sada z předplatných a účtů potřebujete toohost IT zatížení nebo infrastrukturu?</span><span class="sxs-lookup"><span data-stu-id="95ed7-107">What set of subscriptions and accounts do you need toohost your IT workload or infrastructure?</span></span>
* <span data-ttu-id="95ed7-108">Jak toobreak dolů hello hierarchie toofit organizaci?</span><span class="sxs-lookup"><span data-stu-id="95ed7-108">How toobreak down hello hierarchy toofit your organization?</span></span>

<span data-ttu-id="95ed7-109">Úlohy:</span><span class="sxs-lookup"><span data-stu-id="95ed7-109">Tasks:</span></span>

* <span data-ttu-id="95ed7-110">Definujte hierarchii logické organizace, jako byste chtěli toomanage z úrovně předplatného.</span><span class="sxs-lookup"><span data-stu-id="95ed7-110">Define your logical organization hierarchy as you would like toomanage it from a subscription level.</span></span>
* <span data-ttu-id="95ed7-111">toomatch tuto logické hierarchii, zadejte hello účty požadované a předplatná každý účet.</span><span class="sxs-lookup"><span data-stu-id="95ed7-111">toomatch this logical hierarchy, define hello accounts required and subscriptions under each account.</span></span>
* <span data-ttu-id="95ed7-112">Vytvořte sadu hello předplatných a účtů pomocí zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="95ed7-112">Create hello set of subscriptions and accounts using your naming convention.</span></span>

## <a name="subscriptions-and-accounts"></a><span data-ttu-id="95ed7-113">Předplatná a účty</span><span class="sxs-lookup"><span data-stu-id="95ed7-113">Subscriptions and accounts</span></span>
<span data-ttu-id="95ed7-114">toowork s Azure, budete potřebovat jeden nebo víc předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="95ed7-114">toowork with Azure, you need one or more Azure subscriptions.</span></span> <span data-ttu-id="95ed7-115">Prostředky, jako virtuální počítače (VM) nebo virtuální sítě existuje v těchto předplatných.</span><span class="sxs-lookup"><span data-stu-id="95ed7-115">Resources like virtual machines (VMs) or virtual networks exist in of those subscriptions.</span></span>

* <span data-ttu-id="95ed7-116">Podnikoví zákazníci obvykle mají podnikového zápisu, který je hello nejvyšší prostředků v hierarchii hello a přidružené tooone nebo další účty.</span><span class="sxs-lookup"><span data-stu-id="95ed7-116">Enterprise customers typically have an Enterprise Enrollment, which is hello top-most resource in hello hierarchy, and is associated tooone or more accounts.</span></span>
* <span data-ttu-id="95ed7-117">Příjemci a zákazníky bez podnikového zápisu hello nejvyšší prostředek je účet hello.</span><span class="sxs-lookup"><span data-stu-id="95ed7-117">For consumers and customers without an Enterprise Enrollment, hello top-most resource is hello account.</span></span>
* <span data-ttu-id="95ed7-118">Odběry jsou přidružené tooaccounts a může být jeden nebo více odběrů na účtu.</span><span class="sxs-lookup"><span data-stu-id="95ed7-118">Subscriptions are associated tooaccounts, and there can be one or more subscriptions per account.</span></span> <span data-ttu-id="95ed7-119">Fakturační informace na úrovni předplatného hello Azure záznamy.</span><span class="sxs-lookup"><span data-stu-id="95ed7-119">Azure records billing information at hello subscription level.</span></span>

<span data-ttu-id="95ed7-120">Z důvodu omezení toohello úrovní dvě hierarchie v relaci hello účet nebo předplatné je důležité tooalign hello zásady vytváření názvů účtům a předplatným toohello fakturace potřebám.</span><span class="sxs-lookup"><span data-stu-id="95ed7-120">Due toohello limit of two hierarchy levels on hello Account/Subscription relationship, it is important tooalign hello naming convention of accounts and subscriptions toohello billing needs.</span></span> <span data-ttu-id="95ed7-121">Například pokud globální společnost používá Azure, mohou si vyberou toohave jeden účet na oblast a odběry na spravované hello úrovně oblast:</span><span class="sxs-lookup"><span data-stu-id="95ed7-121">For instance, if a global company uses Azure, they might choose toohave one account per region, and have subscriptions managed at hello region level:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

<span data-ttu-id="95ed7-122">Můžete například použít hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="95ed7-122">For instance, you might use hello following structure:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

<span data-ttu-id="95ed7-123">Pokud se v oblasti rozhodne toohave více než jedno předplatné přidružené tooa konkrétní skupiny, zásady vytváření názvů hello by měla zahrnovat tooencode způsob hello doplňující data na účtu hello nebo název odběru hello.</span><span class="sxs-lookup"><span data-stu-id="95ed7-123">If a region decides toohave more than one subscription associated tooa particular group, hello naming convention should incorporate a way tooencode hello extra data on either hello account or hello subscription name.</span></span> <span data-ttu-id="95ed7-124">Tato organizace umožňuje massaging fakturační data toogenerate hello nové úrovně hierarchie během fakturace sestavy:</span><span class="sxs-lookup"><span data-stu-id="95ed7-124">This organization allows massaging billing data toogenerate hello new levels of hierarchy during billing reports:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

<span data-ttu-id="95ed7-125">Hello organizace může vypadat hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="95ed7-125">hello organization could look like hello following example:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

<span data-ttu-id="95ed7-126">Poskytujeme podrobné fakturace prostřednictvím soubor ke stažení pro jeden účet, nebo pro všechny účty v smlouvu enterprise agreement.</span><span class="sxs-lookup"><span data-stu-id="95ed7-126">We provide detailed billing via a downloadable file for a single account, or for all accounts in an enterprise agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="95ed7-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95ed7-127">Next steps</span></span>
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

