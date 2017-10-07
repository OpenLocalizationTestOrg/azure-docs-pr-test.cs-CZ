---
title: "aaaEnable agenta virtuálního počítače v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** povolit virtuálního počítače agenta **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 9bd71e638b020780537da25fd4cf7baf34d3e11a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-vm-agent-in-azure-security-center"></a><span data-ttu-id="daf7b-103">Povolit agenta virtuálního počítače v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="daf7b-103">Enable VM Agent in Azure Security Center</span></span>
<span data-ttu-id="daf7b-104">Hello agenta virtuálního počítače musí být nainstalován na virtuálních počítačích (VM) v pořadí příliš[povolení shromažďování dat](security-center-enable-data-collection.md).</span><span class="sxs-lookup"><span data-stu-id="daf7b-104">hello VM Agent must be installed on virtual machines (VMs) in order too[enable data collection](security-center-enable-data-collection.md).</span></span>  <span data-ttu-id="daf7b-105">Azure Security Center umožňuje vám toosee které virtuální počítače vyžadují hello agenta virtuálního počítače a doporučí, abyste povolili hello agenta virtuálního počítače na těchto virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="daf7b-105">Azure Security Center enables you toosee which VMs require hello VM Agent and will recommend that you enable hello VM Agent on those VMs.</span></span>

<span data-ttu-id="daf7b-106">Hello agenta virtuálního počítače je nainstalována ve výchozím nastavení pro virtuální počítače, které byly nasazeny pomocí hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="daf7b-106">hello VM Agent is installed by default for VMs that are deployed from hello Azure Marketplace.</span></span> <span data-ttu-id="daf7b-107">článek Hello [agenta virtuálního počítače a rozšíření – část 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) poskytuje informace o tom, jak tooinstall hello agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="daf7b-107">hello article [VM Agent and Extensions – Part 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) provides information on how tooinstall hello VM Agent.</span></span>

> [!NOTE]
> <span data-ttu-id="daf7b-108">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="daf7b-108">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="daf7b-109">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="daf7b-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="daf7b-110">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="daf7b-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="daf7b-111">V hello **doporučení okno**, vyberte **povolit agenta virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="daf7b-111">In hello **Recommendations blade**, select **Enable VM Agent**.</span></span>
   <span data-ttu-id="daf7b-112">![Povolení agenta virtuálního počítače][1]</span><span class="sxs-lookup"><span data-stu-id="daf7b-112">![Enable VM Agent][1]</span></span>
2. <span data-ttu-id="daf7b-113">Otevře se okno hello **virtuálního počítače agenta je chybí nebo neodpovídá**.</span><span class="sxs-lookup"><span data-stu-id="daf7b-113">This opens hello blade **VM Agent Is Missing Or Not Responding**.</span></span> <span data-ttu-id="daf7b-114">Toto okno obsahuje seznam hello virtuálních počítačů, které vyžadují hello agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="daf7b-114">This blade lists hello VMs that require hello VM Agent.</span></span> <span data-ttu-id="daf7b-115">Postupujte podle pokynů hello na agenta virtuálního počítače hello okno tooinstall hello.</span><span class="sxs-lookup"><span data-stu-id="daf7b-115">Follow hello instructions on hello blade tooinstall hello VM agent.</span></span>
   <span data-ttu-id="daf7b-116">![Chybí Agent virtuálního počítače][2]</span><span class="sxs-lookup"><span data-stu-id="daf7b-116">![VM Agent is missing][2]</span></span>

## <a name="see-also"></a><span data-ttu-id="daf7b-117">Viz také</span><span class="sxs-lookup"><span data-stu-id="daf7b-117">See also</span></span>
<span data-ttu-id="daf7b-118">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="daf7b-118">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="daf7b-119">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md)– zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="daf7b-119">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="daf7b-120">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="daf7b-120">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="daf7b-121">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md)– zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="daf7b-121">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="daf7b-122">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md)– zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="daf7b-122">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="daf7b-123">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="daf7b-123">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="daf7b-124">[Nejčastější dotazy k Azure Security Center](security-center-faq.md)– přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="daf7b-124">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="daf7b-125">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/)– získejte nejnovější informace zabezpečení Azure hello a informace.</span><span class="sxs-lookup"><span data-stu-id="daf7b-125">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
