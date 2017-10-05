---
title: "Povolit skupin zabezpečení sítě v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** povolit sítě zabezpečení skupiny **."
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
ms.openlocfilehash: 1e034d59d8847f237fa0d4c772344d45cd618576
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a><span data-ttu-id="5dcec-103">Povolit skupin zabezpečení sítě v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="5dcec-103">Enable Network Security Groups in Azure Security Center</span></span>
<span data-ttu-id="5dcec-104">Azure Security Center doporučuje, abyste povolili skupinu zabezpečení sítě (NSG), pokud ještě není povolené.</span><span class="sxs-lookup"><span data-stu-id="5dcec-104">Azure Security Center recommends that you enable a network security group (NSG) if one is not already enabled.</span></span> <span data-ttu-id="5dcec-105">Skupiny Nsg obsahují seznam pravidel seznamu řízení přístupu (ACL), která povolují nebo odpírají síťový provoz instancím virtuálních počítačů ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="5dcec-105">NSGs contain a list of Access Control List (ACL) rules that allow or deny network traffic to your VM instances in a Virtual Network.</span></span> <span data-ttu-id="5dcec-106">Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="5dcec-106">NSGs can be associated with either subnets or individual VM instances within that subnet.</span></span> <span data-ttu-id="5dcec-107">Pokud je skupina zabezpečení sítě přidružená k podsíti, pravidla seznamu ACL platí pro všechny instance virtuálních počítačů v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="5dcec-107">When an NSG is associated with a subnet, the ACL rules apply to all the VM instances in that subnet.</span></span> <span data-ttu-id="5dcec-108">Kromě toho je možné omezit provoz do konkrétního virtuálního počítače další tím, že přidružíte skupinu NSG přímo do tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5dcec-108">In addition, traffic to an individual VM can be restricted further by associating an NSG directly to that VM.</span></span> <span data-ttu-id="5dcec-109">Další informace najdete v další [co je skupina zabezpečení sítě (NSG)?](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="5dcec-109">To learn more see [What is a Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)</span></span>

<span data-ttu-id="5dcec-110">Pokud nemáte skupiny Nsg povoleno, Security Center nabízí dva doporučení vám: Povolit skupin zabezpečení sítě na podsítě a povolit skupin zabezpečení sítě na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="5dcec-110">If you do not have NSGs enabled, Security Center presents two recommendations to you: Enable Network Security Groups on subnets and Enable Network Security Groups on virtual machines.</span></span> <span data-ttu-id="5dcec-111">Můžete vybrat úroveň, podsíť nebo virtuální počítač, chcete-li použít skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="5dcec-111">You choose which level, subnet or VM, to apply NSGs.</span></span>

> [!NOTE]
> <span data-ttu-id="5dcec-112">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="5dcec-112">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="5dcec-113">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="5dcec-113">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="5dcec-114">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="5dcec-114">Implement the recommendation</span></span>
1. <span data-ttu-id="5dcec-115">V **doporučení** vyberte **povolit skupin zabezpečení sítě** v podsítích, nebo na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="5dcec-115">In the **Recommendations** blade, select **Enable Network Security Groups** on subnets or on virtual machines.</span></span>
   <span data-ttu-id="5dcec-116">![Povolení skupin zabezpečení sítě][1]</span><span class="sxs-lookup"><span data-stu-id="5dcec-116">![Enable Network Security Groups][1]</span></span>
2. <span data-ttu-id="5dcec-117">Otevře se okno pro **nakonfigurovat chybějící skupiny zabezpečení sítě** pro podsítě nebo pro virtuální počítače, v závislosti na doporučení, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="5dcec-117">This opens the blade **Configure Missing Network Security Groups** for subnets or for virtual machines, depending on the recommendation that you selected.</span></span> <span data-ttu-id="5dcec-118">Vyberte virtuální počítač nakonfigurovat skupinu NSG na nebo podsíť.</span><span class="sxs-lookup"><span data-stu-id="5dcec-118">Select a subnet or a virtual machine to configure an NSG on.</span></span>

   ![Konfigurace NSG pro podsíť][2]

   ![Konfigurace NSG pro virtuální počítač][3]
3. <span data-ttu-id="5dcec-121">Na **zvolit skupinu zabezpečení sítě** okno, nebo vyberte existující skupinu NSG **vytvořit nový** vytvořit skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="5dcec-121">On the **Choose network security group** blade, select an existing NSG or select **Create new** to create an NSG.</span></span>

   ![Vyberte skupinu zabezpečení sítě][4]

<span data-ttu-id="5dcec-123">Pokud vytvoříte skupinu NSG, postupujte podle kroků v [Správa skupin Nsg pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) vytvořit skupinu NSG a nastavit pravidla zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5dcec-123">If you create an NSG, follow the steps in [How to manage NSGs using the Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) to create an NSG and set security rules.</span></span>

## <a name="see-also"></a><span data-ttu-id="5dcec-124">Viz také</span><span class="sxs-lookup"><span data-stu-id="5dcec-124">See also</span></span>
<span data-ttu-id="5dcec-125">Tento článek ukázal, jak implementovat Security Center doporučení "Povolit skupin zabezpečení sítě" pro podsítě nebo virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5dcec-125">This article showed you how to implement the Security Center recommendation "Enable Network Security Groups" for subnets or virtual machines.</span></span> <span data-ttu-id="5dcec-126">Další informace o povolení skupin Nsg, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="5dcec-126">To learn more about enabling NSGs, see the following:</span></span>

* [<span data-ttu-id="5dcec-127">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="5dcec-127">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="5dcec-128">Správa skupin Nsg pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5dcec-128">How to manage NSGs using the Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="5dcec-129">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="5dcec-129">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="5dcec-130">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5dcec-130">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="5dcec-131">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="5dcec-131">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="5dcec-132">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="5dcec-132">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="5dcec-133">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="5dcec-133">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="5dcec-134">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="5dcec-134">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="5dcec-135">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="5dcec-135">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="5dcec-136">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.</span><span class="sxs-lookup"><span data-stu-id="5dcec-136">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
