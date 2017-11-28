---
title: "aaaEnable skupin zabezpečení sítě v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** povolit sítě zabezpečení skupiny **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 2f70fe432aa452f833a5c322d13102ebbd6dbb69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a><span data-ttu-id="e7011-103">Povolit skupin zabezpečení sítě v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="e7011-103">Enable Network Security Groups in Azure Security Center</span></span>
<span data-ttu-id="e7011-104">Azure Security Center doporučuje, abyste povolili skupinu zabezpečení sítě (NSG), pokud ještě není povolené.</span><span class="sxs-lookup"><span data-stu-id="e7011-104">Azure Security Center recommends that you enable a network security group (NSG) if one is not already enabled.</span></span> <span data-ttu-id="e7011-105">Skupiny Nsg obsahují seznam pravidel seznamu řízení přístupu (ACL), která povolují nebo odpírají síťový provoz tooyour instance virtuálních počítačů ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="e7011-105">NSGs contain a list of Access Control List (ACL) rules that allow or deny network traffic tooyour VM instances in a Virtual Network.</span></span> <span data-ttu-id="e7011-106">Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="e7011-106">NSGs can be associated with either subnets or individual VM instances within that subnet.</span></span> <span data-ttu-id="e7011-107">Pokud je skupina NSG přidružená k podsíti, pravidla seznamu ACL hello platí tooall hello instance virtuálních počítačů v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="e7011-107">When an NSG is associated with a subnet, hello ACL rules apply tooall hello VM instances in that subnet.</span></span> <span data-ttu-id="e7011-108">Kromě toho tooan provoz jednotlivých virtuálních počítačů, je možné omezit tím, že přidružíte skupinu NSG další přímo toothat virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e7011-108">In addition, traffic tooan individual VM can be restricted further by associating an NSG directly toothat VM.</span></span> <span data-ttu-id="e7011-109">víc najdete v části toolearn [co je skupina zabezpečení sítě (NSG)?](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="e7011-109">toolearn more see [What is a Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)</span></span>

<span data-ttu-id="e7011-110">Pokud nemáte skupiny Nsg povoleno, Security Center uvede dvě tooyou doporučení: Povolit skupin zabezpečení sítě na podsítě a povolit skupin zabezpečení sítě na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="e7011-110">If you do not have NSGs enabled, Security Center presents two recommendations tooyou: Enable Network Security Groups on subnets and Enable Network Security Groups on virtual machines.</span></span> <span data-ttu-id="e7011-111">Můžete vybrat úroveň, podsíť nebo virtuální počítač, tooapply skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="e7011-111">You choose which level, subnet or VM, tooapply NSGs.</span></span>

> [!NOTE]
> <span data-ttu-id="e7011-112">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="e7011-112">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="e7011-113">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="e7011-113">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="e7011-114">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="e7011-114">Implement hello recommendation</span></span>
1. <span data-ttu-id="e7011-115">V hello **doporučení** vyberte **povolit skupin zabezpečení sítě** v podsítích, nebo na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="e7011-115">In hello **Recommendations** blade, select **Enable Network Security Groups** on subnets or on virtual machines.</span></span>
   <span data-ttu-id="e7011-116">![Povolení skupin zabezpečení sítě][1]</span><span class="sxs-lookup"><span data-stu-id="e7011-116">![Enable Network Security Groups][1]</span></span>
2. <span data-ttu-id="e7011-117">Otevře se okno hello **nakonfigurovat chybějící skupiny zabezpečení sítě** pro podsítě nebo pro virtuální počítače, v závislosti na hello doporučení, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="e7011-117">This opens hello blade **Configure Missing Network Security Groups** for subnets or for virtual machines, depending on hello recommendation that you selected.</span></span> <span data-ttu-id="e7011-118">Vyberte podsíť nebo virtuální počítač tooconfigure skupinu NSG na.</span><span class="sxs-lookup"><span data-stu-id="e7011-118">Select a subnet or a virtual machine tooconfigure an NSG on.</span></span>

   ![Konfigurace NSG pro podsíť][2]

   ![Konfigurace NSG pro virtuální počítač][3]
3. <span data-ttu-id="e7011-121">Na hello **zvolit skupinu zabezpečení sítě** okno, nebo vyberte existující skupinu NSG **vytvořit nový** toocreate skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="e7011-121">On hello **Choose network security group** blade, select an existing NSG or select **Create new** toocreate an NSG.</span></span>

   ![Vyberte skupinu zabezpečení sítě][4]

<span data-ttu-id="e7011-123">Pokud vytvoříte skupinu NSG, postupujte podle kroků hello v [jak hello skupiny Nsg toomanage pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) toocreate skupinu NSG a nastavte pravidla zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e7011-123">If you create an NSG, follow hello steps in [How toomanage NSGs using hello Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) toocreate an NSG and set security rules.</span></span>

## <a name="see-also"></a><span data-ttu-id="e7011-124">Viz také</span><span class="sxs-lookup"><span data-stu-id="e7011-124">See also</span></span>
<span data-ttu-id="e7011-125">Tento článek vám ukázal, jak tooimplement hello Security Center doporučení "Povolit skupin zabezpečení sítě" pro podsítě nebo virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e7011-125">This article showed you how tooimplement hello Security Center recommendation "Enable Network Security Groups" for subnets or virtual machines.</span></span> <span data-ttu-id="e7011-126">Další informace o povolení skupin Nsg, toolearn najdete hello následující:</span><span class="sxs-lookup"><span data-stu-id="e7011-126">toolearn more about enabling NSGs, see hello following:</span></span>

* [<span data-ttu-id="e7011-127">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="e7011-127">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="e7011-128">Jak hello skupiny Nsg toomanage pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e7011-128">How toomanage NSGs using hello Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="e7011-129">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="e7011-129">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="e7011-130">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="e7011-130">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="e7011-131">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="e7011-131">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="e7011-132">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e7011-132">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="e7011-133">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="e7011-133">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="e7011-134">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="e7011-134">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="e7011-135">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="e7011-135">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="e7011-136">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.</span><span class="sxs-lookup"><span data-stu-id="e7011-136">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
