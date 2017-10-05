---
title: "Povolit agenta virtuálního počítače v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** povolit virtuálního počítače agenta **."
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
ms.openlocfilehash: 337a7adfd93c76882a749685702bea6d1524c96a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-vm-agent-in-azure-security-center"></a><span data-ttu-id="a09c7-103">Povolit agenta virtuálního počítače v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="a09c7-103">Enable VM Agent in Azure Security Center</span></span>
<span data-ttu-id="a09c7-104">Musí být nainstalovaný Agent virtuálního počítače na virtuální počítače (VM) za účelem [povolení shromažďování dat](security-center-enable-data-collection.md).</span><span class="sxs-lookup"><span data-stu-id="a09c7-104">The VM Agent must be installed on virtual machines (VMs) in order to [enable data collection](security-center-enable-data-collection.md).</span></span>  <span data-ttu-id="a09c7-105">Azure Security Center vám umožní zobrazit které virtuální počítače vyžadují agenta virtuálního počítače a doporučí, abyste povolili agenta virtuálního počítače na těchto virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="a09c7-105">Azure Security Center enables you to see which VMs require the VM Agent and will recommend that you enable the VM Agent on those VMs.</span></span>

<span data-ttu-id="a09c7-106">Agent virtuálního počítače je ve výchozím nastavení nainstalován na virtuálních počítačích nasazených z Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a09c7-106">The VM Agent is installed by default for VMs that are deployed from the Azure Marketplace.</span></span> <span data-ttu-id="a09c7-107">V článku [Agenti a rozšíření virtuálních počítačů – Část 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) najdete informace o tom, jak agenta virtuálního počítače nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="a09c7-107">The article [VM Agent and Extensions – Part 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) provides information on how to install the VM Agent.</span></span>

> [!NOTE]
> <span data-ttu-id="a09c7-108">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="a09c7-108">This document introduces the service by using an example deployment.</span></span> <span data-ttu-id="a09c7-109">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="a09c7-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="a09c7-110">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="a09c7-110">Implement the recommendation</span></span>
1. <span data-ttu-id="a09c7-111">V **doporučení okno**, vyberte **povolit agenta virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="a09c7-111">In the **Recommendations blade**, select **Enable VM Agent**.</span></span>
   <span data-ttu-id="a09c7-112">![Povolení agenta virtuálního počítače][1]</span><span class="sxs-lookup"><span data-stu-id="a09c7-112">![Enable VM Agent][1]</span></span>
2. <span data-ttu-id="a09c7-113">Otevře se okno pro **virtuálního počítače agenta je chybí nebo neodpovídá**.</span><span class="sxs-lookup"><span data-stu-id="a09c7-113">This opens the blade **VM Agent Is Missing Or Not Responding**.</span></span> <span data-ttu-id="a09c7-114">Toto okno obsahuje seznam virtuálních počítačů, které vyžadují agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a09c7-114">This blade lists the VMs that require the VM Agent.</span></span> <span data-ttu-id="a09c7-115">Postupujte podle pokynů v okně instalace agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a09c7-115">Follow the instructions on the blade to install the VM agent.</span></span>
   <span data-ttu-id="a09c7-116">![Chybí Agent virtuálního počítače][2]</span><span class="sxs-lookup"><span data-stu-id="a09c7-116">![VM Agent is missing][2]</span></span>

## <a name="see-also"></a><span data-ttu-id="a09c7-117">Viz také</span><span class="sxs-lookup"><span data-stu-id="a09c7-117">See also</span></span>
<span data-ttu-id="a09c7-118">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="a09c7-118">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="a09c7-119">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak se konfigurují zásady zabezpečení pro vaše předplatné Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="a09c7-119">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="a09c7-120">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="a09c7-120">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="a09c7-121">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="a09c7-121">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="a09c7-122">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="a09c7-122">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="a09c7-123">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="a09c7-123">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="a09c7-124">[Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.</span><span class="sxs-lookup"><span data-stu-id="a09c7-124">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="a09c7-125">[Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) – Získejte nejnovější informace o zabezpečení Azure.</span><span class="sxs-lookup"><span data-stu-id="a09c7-125">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
