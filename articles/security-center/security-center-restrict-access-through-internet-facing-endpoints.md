---
title: "aaaRestrict přístupu prostřednictvím internetových koncových bodů v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** omezit přístup prostřednictvím internetové koncový bod **."
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
ms.openlocfilehash: ee72497088618d4db29b5ae4183f4fe77b498423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a><span data-ttu-id="91969-103">Omezení přístupu prostřednictvím internetových koncových bodů v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="91969-103">Restrict access through Internet-facing endpoints in Azure Security Center</span></span>
<span data-ttu-id="91969-104">Azure Security Center doporučí, omezení přístupu prostřednictvím internetových koncových bodů, pokud žádné skupiny zabezpečení sítě (Nsg) má jednu nebo více příchozí pravidla, která umožňují přístup z "žádné" zdrojové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="91969-104">Azure Security Center will recommend that you restrict access through Internet-facing endpoints if any of your Network Security Groups (NSGs) has one or more inbound rules that allow access from “any” source IP address.</span></span> <span data-ttu-id="91969-105">Přístup k otevírání příliš "žádné" může povolit útočníci tooaccess vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="91969-105">Opening access too“any” may enable attackers tooaccess your resources.</span></span> <span data-ttu-id="91969-106">Security Center doporučí, že upravíte tyto příchozích pravidel toorestrict přístup toosource IP adresy, které skutečně potřebují přístup.</span><span class="sxs-lookup"><span data-stu-id="91969-106">Security Center will recommend that you edit these inbound rules toorestrict access toosource IP addresses that actually need access.</span></span>

<span data-ttu-id="91969-107">Toto doporučení se generuje pro všechny port jiný web, který má "žádný" jako zdroj.</span><span class="sxs-lookup"><span data-stu-id="91969-107">This recommendation is generated for any non-web port that has "any" as source.</span></span>

> [!NOTE]
> <span data-ttu-id="91969-108">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="91969-108">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="91969-109">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="91969-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="91969-110">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="91969-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="91969-111">V hello **doporučení okno**, vyberte **omezit přístup prostřednictvím internetové koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="91969-111">In hello **Recommendations blade**, select **Restrict access through Internet facing endpoint**.</span></span>

   ![Omezit přístup přes internetový koncový bod][1]
2. <span data-ttu-id="91969-113">Otevře se okno hello **omezit přístup prostřednictvím internetové koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="91969-113">This opens hello blade **Restrict access through Internet facing endpoint**.</span></span> <span data-ttu-id="91969-114">Toto okno obsahuje seznam hello virtuální počítače (VM) s příchozích pravidel, které vytvářejí potenciálním potížím se zabezpečením.</span><span class="sxs-lookup"><span data-stu-id="91969-114">This blade lists hello virtual machines (VMs) with inbound rules that create a potential security issue.</span></span> <span data-ttu-id="91969-115">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="91969-115">Select a VM.</span></span>

   ![Vyberte virtuální počítač][2]
3. <span data-ttu-id="91969-117">Hello **NSG** okno zobrazuje informace o skupinu zabezpečení sítě, související příchozích pravidel a hello přidružené virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="91969-117">hello **NSG** blade displays Network Security Group information, related inbound rules, and hello associated VM.</span></span> <span data-ttu-id="91969-118">Vyberte **upravit příchozí pravidla** tooproceed s úpravy příchozího pravidla.</span><span class="sxs-lookup"><span data-stu-id="91969-118">Select **Edit inbound rules** tooproceed with editing an inbound rule.</span></span>

   ![Okno skupina zabezpečení sítě][3]
4. <span data-ttu-id="91969-120">Na hello **příchozí pravidla zabezpečení** okně vyberte hello tooedit příchozí pravidlo.</span><span class="sxs-lookup"><span data-stu-id="91969-120">On hello **Inbound security rules** blade select hello inbound rule tooedit.</span></span> <span data-ttu-id="91969-121">V tomto příkladu budeme vyberte **AllowWeb**.</span><span class="sxs-lookup"><span data-stu-id="91969-121">In this example, let’s select **AllowWeb**.</span></span>

   ![Příchozí pravidla zabezpečení][4]

   <span data-ttu-id="91969-123">Všimněte si, můžete také vybrat **výchozí pravidla** toosee hello sadu výchozích pravidel obsažené ve všech skupin Nsg.</span><span class="sxs-lookup"><span data-stu-id="91969-123">Note, you can also select **Default rules** toosee hello set of default rules contained by all NSGs.</span></span> <span data-ttu-id="91969-124">výchozí pravidla Hello nelze odstranit, ale protože je jim přiřazená s nižší prioritou, dají se přepsat pravidly hello, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="91969-124">hello default rules cannot be deleted but, because they are assigned a lower priority, they can be overridden by hello rules that you create.</span></span> <span data-ttu-id="91969-125">Další informace o [výchozí pravidla](../virtual-network/virtual-networks-nsg.md#default-rules).</span><span class="sxs-lookup"><span data-stu-id="91969-125">Learn more about [default rules](../virtual-network/virtual-networks-nsg.md#default-rules).</span></span>

   ![Výchozí pravidla][5]
5. <span data-ttu-id="91969-127">Na hello **AllowWeb** okně Upravit vlastnosti hello hello příchozí pravidlo, které hello **zdroj** je IP adresa nebo blok IP adres.</span><span class="sxs-lookup"><span data-stu-id="91969-127">On hello **AllowWeb** blade, edit hello properties of hello inbound rule so that hello **Source** is an IP address or block of IP addresses.</span></span> <span data-ttu-id="91969-128">toolearn Další informace o vlastnosti hello hello příchozí pravidlo, najdete v části [pravidla NSG](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span><span class="sxs-lookup"><span data-stu-id="91969-128">toolearn more about hello properties of hello inbound rule, see [NSG rules](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span></span>

   ![Upravit pravidlo pro příchozí][6]

## <a name="see-also"></a><span data-ttu-id="91969-130">Viz také</span><span class="sxs-lookup"><span data-stu-id="91969-130">See also</span></span>
<span data-ttu-id="91969-131">Tento článek ukázal, jak tooimplement hello Security Center doporučení "omezit přístup prostřednictvím internetové koncový bod."</span><span class="sxs-lookup"><span data-stu-id="91969-131">This article showed you how tooimplement hello Security Center recommendation "Restrict access through Internet facing endpoint.”</span></span> <span data-ttu-id="91969-132">toolearn Další informace o povolení skupiny Nsg a pravidla, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="91969-132">toolearn more about enabling NSGs and rules, see hello following:</span></span>

* [<span data-ttu-id="91969-133">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="91969-133">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="91969-134">Jak hello skupiny Nsg toomanage pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="91969-134">How toomanage NSGs using hello Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="91969-135">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="91969-135">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="91969-136">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md)– zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="91969-136">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="91969-137">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="91969-137">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="91969-138">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md)– zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="91969-138">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="91969-139">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md)– zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="91969-139">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="91969-140">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="91969-140">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="91969-141">[Nejčastější dotazy k Azure Security Center](security-center-faq.md)– přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="91969-141">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="91969-142">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/)– získejte nejnovější informace zabezpečení Azure hello a informace.</span><span class="sxs-lookup"><span data-stu-id="91969-142">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
