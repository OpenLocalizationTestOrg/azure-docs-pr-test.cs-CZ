---
title: "Omezení přístupu prostřednictvím internetových koncových bodů v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** omezit přístup prostřednictvím internetové koncový bod **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 727d88c9-163b-4ea0-a4ce-3be43686599f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: f7309c617f1705205e2c9f1b1b48d141391d45da
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a><span data-ttu-id="25086-103">Omezení přístupu prostřednictvím internetových koncových bodů v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="25086-103">Restrict access through Internet-facing endpoints in Azure Security Center</span></span>
<span data-ttu-id="25086-104">Azure Security Center doporučí, omezení přístupu prostřednictvím internetových koncových bodů, pokud žádné skupiny zabezpečení sítě (Nsg) má jednu nebo více příchozí pravidla, která umožňují přístup z "žádné" zdrojové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="25086-104">Azure Security Center will recommend that you restrict access through Internet-facing endpoints if any of your Network Security Groups (NSGs) has one or more inbound rules that allow access from “any” source IP address.</span></span> <span data-ttu-id="25086-105">Otevírání přístup k "žádné" může povolit útočníci k přístupu k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="25086-105">Opening access to “any” may enable attackers to access your resources.</span></span> <span data-ttu-id="25086-106">Security Center doporučí, že upravíte tyto příchozích pravidel pro omezení přístupu ke zdrojové IP adresy, které skutečně potřebují přístup.</span><span class="sxs-lookup"><span data-stu-id="25086-106">Security Center will recommend that you edit these inbound rules to restrict access to source IP addresses that actually need access.</span></span>

<span data-ttu-id="25086-107">Toto doporučení se generuje pro všechny port jiný web, který má "žádný" jako zdroj.</span><span class="sxs-lookup"><span data-stu-id="25086-107">This recommendation is generated for any non-web port that has "any" as source.</span></span>

> [!NOTE]
> <span data-ttu-id="25086-108">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="25086-108">This document introduces the service by using an example deployment.</span></span> <span data-ttu-id="25086-109">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="25086-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="25086-110">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="25086-110">Implement the recommendation</span></span>
1. <span data-ttu-id="25086-111">V **doporučení okno**, vyberte **omezit přístup prostřednictvím internetové koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="25086-111">In the **Recommendations blade**, select **Restrict access through Internet facing endpoint**.</span></span>

   ![Omezit přístup přes internetový koncový bod][1]
2. <span data-ttu-id="25086-113">Otevře se okno pro **omezit přístup prostřednictvím internetové koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="25086-113">This opens the blade **Restrict access through Internet facing endpoint**.</span></span> <span data-ttu-id="25086-114">Toto okno Seznam virtuálních počítačů (VM) s příchozích pravidel, které vytvářejí potenciálním potížím se zabezpečením.</span><span class="sxs-lookup"><span data-stu-id="25086-114">This blade lists the virtual machines (VMs) with inbound rules that create a potential security issue.</span></span> <span data-ttu-id="25086-115">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="25086-115">Select a VM.</span></span>

   ![Vyberte virtuální počítač][2]
3. <span data-ttu-id="25086-117">**NSG** zobrazuje informace skupinu zabezpečení sítě, související příchozích pravidel a přidružené virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="25086-117">The **NSG** blade displays Network Security Group information, related inbound rules, and the associated VM.</span></span> <span data-ttu-id="25086-118">Vyberte **upravit příchozí pravidla** pokračujte úpravy příchozího pravidla.</span><span class="sxs-lookup"><span data-stu-id="25086-118">Select **Edit inbound rules** to proceed with editing an inbound rule.</span></span>

   ![Okno skupina zabezpečení sítě][3]
4. <span data-ttu-id="25086-120">Na **příchozí pravidla zabezpečení** vyberte příchozí pravidlo, které chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="25086-120">On the **Inbound security rules** blade select the inbound rule to edit.</span></span> <span data-ttu-id="25086-121">V tomto příkladu budeme vyberte **AllowWeb**.</span><span class="sxs-lookup"><span data-stu-id="25086-121">In this example, let’s select **AllowWeb**.</span></span>

   ![Příchozí pravidla zabezpečení][4]

   <span data-ttu-id="25086-123">Všimněte si, můžete také vybrat **výchozí pravidla** zobrazíte sadu výchozích pravidel obsažené ve všech skupin Nsg.</span><span class="sxs-lookup"><span data-stu-id="25086-123">Note, you can also select **Default rules** to see the set of default rules contained by all NSGs.</span></span> <span data-ttu-id="25086-124">Výchozí pravidla nelze odstranit, ale protože je jim přiřazená s nižší prioritou, dají se přepsat pravidly, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="25086-124">The default rules cannot be deleted but, because they are assigned a lower priority, they can be overridden by the rules that you create.</span></span> <span data-ttu-id="25086-125">Další informace o [výchozí pravidla](../virtual-network/virtual-networks-nsg.md#default-rules).</span><span class="sxs-lookup"><span data-stu-id="25086-125">Learn more about [default rules](../virtual-network/virtual-networks-nsg.md#default-rules).</span></span>

   ![Výchozí pravidla][5]
5. <span data-ttu-id="25086-127">Na **AllowWeb** okně Upravit vlastnosti příchozí pravidlo tak, aby **zdroj** je IP adresa nebo blok IP adres.</span><span class="sxs-lookup"><span data-stu-id="25086-127">On the **AllowWeb** blade, edit the properties of the inbound rule so that the **Source** is an IP address or block of IP addresses.</span></span> <span data-ttu-id="25086-128">Další informace o vlastnostech příchozí pravidlo, najdete v části [pravidla NSG](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span><span class="sxs-lookup"><span data-stu-id="25086-128">To learn more about the properties of the inbound rule, see [NSG rules](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span></span>

   ![Upravit pravidlo pro příchozí][6]

## <a name="see-also"></a><span data-ttu-id="25086-130">Viz také</span><span class="sxs-lookup"><span data-stu-id="25086-130">See also</span></span>
<span data-ttu-id="25086-131">Tento článek vám ukázal, jak provést doporučení Security Center "Omezit přístup prostřednictvím internetový koncový bod."</span><span class="sxs-lookup"><span data-stu-id="25086-131">This article showed you how to implement the Security Center recommendation "Restrict access through Internet facing endpoint.”</span></span> <span data-ttu-id="25086-132">Další informace o povolení skupiny Nsg a pravidla, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="25086-132">To learn more about enabling NSGs and rules, see the following:</span></span>

* [<span data-ttu-id="25086-133">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="25086-133">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="25086-134">Správa skupin Nsg pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="25086-134">How to manage NSGs using the Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="25086-135">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="25086-135">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="25086-136">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak se konfigurují zásady zabezpečení pro vaše předplatné Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="25086-136">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="25086-137">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="25086-137">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="25086-138">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="25086-138">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="25086-139">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="25086-139">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="25086-140">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="25086-140">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="25086-141">[Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.</span><span class="sxs-lookup"><span data-stu-id="25086-141">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="25086-142">[Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) – Získejte nejnovější informace o zabezpečení Azure.</span><span class="sxs-lookup"><span data-stu-id="25086-142">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
