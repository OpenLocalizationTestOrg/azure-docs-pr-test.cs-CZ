---
title: "Použít aktualizace systému v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak implementovat Azure Security Center doporučení ** použít aktualizace systému ** a ** po systému aktualizace ** restartuje."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: 50cdea437db5387813c6a3905d14b6904d2aba34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="apply-system-updates-in-azure-security-center"></a><span data-ttu-id="f8000-103">Použít aktualizace systému v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="f8000-103">Apply system updates in Azure Security Center</span></span>
<span data-ttu-id="f8000-104">Azure Security Center monitoruje denně Windows a Linux virtuálních počítačů (VM) pro chybějící aktualizace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="f8000-104">Azure Security Center monitors daily Windows and  Linux virtual machines (VMs) for missing operating system updates.</span></span> <span data-ttu-id="f8000-105">Security Center načte seznam dostupných zabezpečení a důležité aktualizace ze služby Windows Update nebo Windows Server Update Services (WSUS), podle toho, která je nakonfigurovaná služba virtuální počítač s Windows.</span><span class="sxs-lookup"><span data-stu-id="f8000-105">Security Center retrieves a list of available security and critical updates from Windows Update or Windows Server Update Services (WSUS), depending on which service is configured on a Windows VM.</span></span>  <span data-ttu-id="f8000-106">Security Center také zkontroluje nejnovější aktualizace v systémech Linux.</span><span class="sxs-lookup"><span data-stu-id="f8000-106">Security Center also checks for the latest updates in Linux systems.</span></span> <span data-ttu-id="f8000-107">Pokud virtuální počítač je chybějící aktualizace systému, Security Center bude doporučujeme použít aktualizace systému</span><span class="sxs-lookup"><span data-stu-id="f8000-107">If your VM is missing a system update, Security Center will recommend that you apply system updates</span></span>

> [!NOTE]
> <span data-ttu-id="f8000-108">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="f8000-108">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="f8000-109">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="f8000-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="f8000-110">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="f8000-110">Implement the recommendation</span></span>
1. <span data-ttu-id="f8000-111">V **doporučení** vyberte **aktualizace systému**.</span><span class="sxs-lookup"><span data-stu-id="f8000-111">In the **Recommendations** blade, select **Apply system updates**.</span></span>

   ![Nainstalovat aktualizace systému][1]
2. <span data-ttu-id="f8000-113">**Aktualizace systému** otevře se okno se seznamem chybějící aktualizace systému virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f8000-113">The **Apply system updates** blade opens displaying a list of VMs missing system updates.</span></span> <span data-ttu-id="f8000-114">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f8000-114">Select a VM.</span></span>

   ![Vyberte virtuální počítač][2]
3. <span data-ttu-id="f8000-116">Otevře se okno se seznamem chybějící aktualizace pro tento virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f8000-116">A blade opens displaying a list of missing updates for that VM.</span></span> <span data-ttu-id="f8000-117">Vyberte aktualizace systému.</span><span class="sxs-lookup"><span data-stu-id="f8000-117">Select a system update.</span></span> <span data-ttu-id="f8000-118">V tomto příkladu budeme vyberte KB3156016.</span><span class="sxs-lookup"><span data-stu-id="f8000-118">In this example, let’s select KB3156016.</span></span>

   ![Chybějící aktualizace zabezpečení][3]

4. <span data-ttu-id="f8000-120">Postupujte podle kroků v **aktualizací zabezpečení** okno použít chybějící aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f8000-120">Follow the steps in the **Security Update** blade to apply the missing update.</span></span>

   ![Aktualizace zabezpečení][4]

## <a name="reboot-after-system-updates"></a><span data-ttu-id="f8000-122">Restartovat po aktualizacích systému</span><span class="sxs-lookup"><span data-stu-id="f8000-122">Reboot after system updates</span></span>
1. <span data-ttu-id="f8000-123">Vraťte se na **doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="f8000-123">Return to the **Recommendations** blade.</span></span> <span data-ttu-id="f8000-124">Nový záznam vygenerovalo po použití aktualizací systému, nazývá **restartovat po aktualizacích systému**.</span><span class="sxs-lookup"><span data-stu-id="f8000-124">A new entry was generated after you applied system updates, called **Reboot after system updates**.</span></span> <span data-ttu-id="f8000-125">Tato položka umožňuje vědět, že je potřeba restartovat virtuální počítač pro dokončení procesu použití aktualizace systému.</span><span class="sxs-lookup"><span data-stu-id="f8000-125">This entry lets you know that you need to reboot the VM to complete the process of applying system updates.</span></span>

   ![Restartovat po aktualizacích systému][5]
2. <span data-ttu-id="f8000-127">Vyberte **restartovat po aktualizacích systému**.</span><span class="sxs-lookup"><span data-stu-id="f8000-127">Select **Reboot after system updates**.</span></span> <span data-ttu-id="f8000-128">Tím se otevře **restart čeká na dokončení aktualizací systému** procesu aktualizací okno se seznamem virtuálních počítačů, které potřebujete k dokončení použít systém restartovat.</span><span class="sxs-lookup"><span data-stu-id="f8000-128">This opens **A restart is pending to complete system updates** blade displaying a list of VMs that you need to restart to complete the apply system updates process.</span></span>

   ![Čeká se na restartování][6]

<span data-ttu-id="f8000-130">Restartujte virtuální počítač z Azure do dokončení procesu.</span><span class="sxs-lookup"><span data-stu-id="f8000-130">Restart the VM from Azure to complete the process.</span></span>

## <a name="see-also"></a><span data-ttu-id="f8000-131">Viz také</span><span class="sxs-lookup"><span data-stu-id="f8000-131">See also</span></span>
<span data-ttu-id="f8000-132">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="f8000-132">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="f8000-133">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="f8000-133">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="f8000-134">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="f8000-134">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="f8000-135">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="f8000-135">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="f8000-136">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="f8000-136">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="f8000-137">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="f8000-137">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="f8000-138">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="f8000-138">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="f8000-139">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="f8000-139">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
