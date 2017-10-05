---
title: "Předplatné a účet pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro předplatných a účtů v Azure."
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
ms.openlocfilehash: 19695a9960d8e8f0dfca4bf0ca10761fe6ae7ff0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-linux-vms"></a><span data-ttu-id="f4ca1-103">Azure předplatného a účty pokyny pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="f4ca1-103">Azure subscription and accounts guidelines for Linux VMs</span></span>

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="f4ca1-104">Tento článek se týká vědět, jak se přístup ke správě předplatné a účet jako prostředí a zvětšování uživatelské základny.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-104">This article focuses on understanding how to approach subscription and account management as your environment and user base grows.</span></span>

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a><span data-ttu-id="f4ca1-105">Postup implementace pro předplatných a účtů</span><span class="sxs-lookup"><span data-stu-id="f4ca1-105">Implementation guidelines for subscriptions and accounts</span></span>
<span data-ttu-id="f4ca1-106">Rozhodnutí:</span><span class="sxs-lookup"><span data-stu-id="f4ca1-106">Decisions:</span></span>

* <span data-ttu-id="f4ca1-107">Jaká sada předplatných a účtů proveďte potřebujete k hostování zatížení IT nebo infrastrukturu?</span><span class="sxs-lookup"><span data-stu-id="f4ca1-107">What set of subscriptions and accounts do you need to host your IT workload or infrastructure?</span></span>
* <span data-ttu-id="f4ca1-108">Jak rozdělení hierarchii podle vaší organizace?</span><span class="sxs-lookup"><span data-stu-id="f4ca1-108">How to break down the hierarchy to fit your organization?</span></span>

<span data-ttu-id="f4ca1-109">Úlohy:</span><span class="sxs-lookup"><span data-stu-id="f4ca1-109">Tasks:</span></span>

* <span data-ttu-id="f4ca1-110">Definujte hierarchii logické organizace, jako byste chtěli spravovat z úrovně předplatného.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-110">Define your logical organization hierarchy as you would like to manage it from a subscription level.</span></span>
* <span data-ttu-id="f4ca1-111">Tak, aby odpovídala této logické hierarchii, zadejte požadované účty a předplatná každý účet.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-111">To match this logical hierarchy, define the accounts required and subscriptions under each account.</span></span>
* <span data-ttu-id="f4ca1-112">Vytvořte sadu předplatných a účtů pomocí zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-112">Create the set of subscriptions and accounts using your naming convention.</span></span>

## <a name="subscriptions-and-accounts"></a><span data-ttu-id="f4ca1-113">Předplatná a účty</span><span class="sxs-lookup"><span data-stu-id="f4ca1-113">Subscriptions and accounts</span></span>
<span data-ttu-id="f4ca1-114">Chcete-li pracovat s Azure, je třeba jeden nebo víc předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-114">To work with Azure, you need one or more Azure subscriptions.</span></span> <span data-ttu-id="f4ca1-115">Prostředky, jako virtuální počítače (VM) nebo virtuální sítě existuje v těchto předplatných.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-115">Resources like virtual machines (VMs) or virtual networks exist in of those subscriptions.</span></span>

* <span data-ttu-id="f4ca1-116">Podnikoví zákazníci obvykle mají podnikového zápisu, který je nejvyšší prostředků v hierarchii a je přidružen k jedné nebo více účtů.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-116">Enterprise customers typically have an Enterprise Enrollment, which is the top-most resource in the hierarchy, and is associated to one or more accounts.</span></span>
* <span data-ttu-id="f4ca1-117">Příjemci a zákazníky bez podnikového zápisu je prostředek nejvyšší účet.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-117">For consumers and customers without an Enterprise Enrollment, the top-most resource is the account.</span></span>
* <span data-ttu-id="f4ca1-118">Odběry jsou přidružené k účtům a může být jeden nebo více odběrů na účtu.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-118">Subscriptions are associated to accounts, and there can be one or more subscriptions per account.</span></span> <span data-ttu-id="f4ca1-119">Fakturační informace na úrovni předplatného Azure záznamy.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-119">Azure records billing information at the subscription level.</span></span>

<span data-ttu-id="f4ca1-120">Z důvodu limit dvě úrovně hierarchie v relaci účet nebo předplatné je důležité sladit zásady vytváření názvů účtů a odběry fakturace potřebám.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-120">Due to the limit of two hierarchy levels on the Account/Subscription relationship, it is important to align the naming convention of accounts and subscriptions to the billing needs.</span></span> <span data-ttu-id="f4ca1-121">Například pokud globální společnost používá Azure, může zvolit tak, aby měl jeden účet na oblast a odběry na spravované úrovně oblast:</span><span class="sxs-lookup"><span data-stu-id="f4ca1-121">For instance, if a global company uses Azure, they might choose to have one account per region, and have subscriptions managed at the region level:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

<span data-ttu-id="f4ca1-122">Můžete například použít následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="f4ca1-122">For instance, you might use the following structure:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

<span data-ttu-id="f4ca1-123">Pokud oblast rozhodne mít více než jedno předplatné, které jsou přidružené k určité skupiny, by měly zásady vytváření názvů začlenit způsob, jak kódování doplňující data na účtu nebo název odběru.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-123">If a region decides to have more than one subscription associated to a particular group, the naming convention should incorporate a way to encode the extra data on either the account or the subscription name.</span></span> <span data-ttu-id="f4ca1-124">Tato organizace umožňuje massaging fakturační údaje, které během fakturace sestavy generovat nové úrovně hierarchie:</span><span class="sxs-lookup"><span data-stu-id="f4ca1-124">This organization allows massaging billing data to generate the new levels of hierarchy during billing reports:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

<span data-ttu-id="f4ca1-125">Organizace může vypadat podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f4ca1-125">The organization could look like the following example:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

<span data-ttu-id="f4ca1-126">Poskytujeme podrobné fakturace prostřednictvím soubor ke stažení pro jeden účet, nebo pro všechny účty v smlouvu enterprise agreement.</span><span class="sxs-lookup"><span data-stu-id="f4ca1-126">We provide detailed billing via a downloadable file for a single account, or for all accounts in an enterprise agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4ca1-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4ca1-127">Next steps</span></span>
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

