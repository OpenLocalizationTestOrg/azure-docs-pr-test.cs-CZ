---
title: "aktualizace systému aaaApply v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello doporučení služby Azure Security Center ** použít aktualizace systému ** a ** po systému aktualizace ** restartuje."
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
ms.openlocfilehash: 02024f1558b4758c09141fe1934c2e1a9845cc96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apply-system-updates-in-azure-security-center"></a><span data-ttu-id="3710b-103">Použít aktualizace systému v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="3710b-103">Apply system updates in Azure Security Center</span></span>
<span data-ttu-id="3710b-104">Azure Security Center monitoruje denně Windows a Linux virtuálních počítačů (VM) pro chybějící aktualizace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3710b-104">Azure Security Center monitors daily Windows and  Linux virtual machines (VMs) for missing operating system updates.</span></span> <span data-ttu-id="3710b-105">Security Center načte seznam dostupných zabezpečení a důležité aktualizace ze služby Windows Update nebo Windows Server Update Services (WSUS), podle toho, která je nakonfigurovaná služba virtuální počítač s Windows.</span><span class="sxs-lookup"><span data-stu-id="3710b-105">Security Center retrieves a list of available security and critical updates from Windows Update or Windows Server Update Services (WSUS), depending on which service is configured on a Windows VM.</span></span>  <span data-ttu-id="3710b-106">Security Center také zkontroluje nejnovější aktualizace hello v systémech Linux.</span><span class="sxs-lookup"><span data-stu-id="3710b-106">Security Center also checks for hello latest updates in Linux systems.</span></span> <span data-ttu-id="3710b-107">Pokud virtuální počítač je chybějící aktualizace systému, Security Center bude doporučujeme použít aktualizace systému</span><span class="sxs-lookup"><span data-stu-id="3710b-107">If your VM is missing a system update, Security Center will recommend that you apply system updates</span></span>

> [!NOTE]
> <span data-ttu-id="3710b-108">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="3710b-108">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="3710b-109">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="3710b-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="3710b-110">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="3710b-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="3710b-111">V hello **doporučení** vyberte **aktualizace systému**.</span><span class="sxs-lookup"><span data-stu-id="3710b-111">In hello **Recommendations** blade, select **Apply system updates**.</span></span>

   ![Nainstalovat aktualizace systému][1]
2. <span data-ttu-id="3710b-113">Hello **aktualizace systému** otevře se okno se seznamem chybějící aktualizace systému virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3710b-113">hello **Apply system updates** blade opens displaying a list of VMs missing system updates.</span></span> <span data-ttu-id="3710b-114">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3710b-114">Select a VM.</span></span>

   ![Vyberte virtuální počítač][2]
3. <span data-ttu-id="3710b-116">Otevře se okno se seznamem chybějící aktualizace pro tento virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3710b-116">A blade opens displaying a list of missing updates for that VM.</span></span> <span data-ttu-id="3710b-117">Vyberte aktualizace systému.</span><span class="sxs-lookup"><span data-stu-id="3710b-117">Select a system update.</span></span> <span data-ttu-id="3710b-118">V tomto příkladu budeme vyberte KB3156016.</span><span class="sxs-lookup"><span data-stu-id="3710b-118">In this example, let’s select KB3156016.</span></span>

   ![Chybějící aktualizace zabezpečení][3]

4. <span data-ttu-id="3710b-120">Postupujte podle kroků hello v hello **aktualizací zabezpečení** okno tooapply hello chybějící aktualizace.</span><span class="sxs-lookup"><span data-stu-id="3710b-120">Follow hello steps in hello **Security Update** blade tooapply hello missing update.</span></span>

   ![Aktualizace zabezpečení][4]

## <a name="reboot-after-system-updates"></a><span data-ttu-id="3710b-122">Restartovat po aktualizacích systému</span><span class="sxs-lookup"><span data-stu-id="3710b-122">Reboot after system updates</span></span>
1. <span data-ttu-id="3710b-123">Vrátí toohello **doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="3710b-123">Return toohello **Recommendations** blade.</span></span> <span data-ttu-id="3710b-124">Nový záznam vygenerovalo po použití aktualizací systému, nazývá **restartovat po aktualizacích systému**.</span><span class="sxs-lookup"><span data-stu-id="3710b-124">A new entry was generated after you applied system updates, called **Reboot after system updates**.</span></span> <span data-ttu-id="3710b-125">Tato položka způsobem zjistíte, je nutné, aby tooreboot hello virtuálních počítačů toocomplete hello proces použití aktualizací systému.</span><span class="sxs-lookup"><span data-stu-id="3710b-125">This entry lets you know that you need tooreboot hello VM toocomplete hello process of applying system updates.</span></span>

   ![Restartovat po aktualizacích systému][5]
2. <span data-ttu-id="3710b-127">Vyberte **restartovat po aktualizacích systému**.</span><span class="sxs-lookup"><span data-stu-id="3710b-127">Select **Reboot after system updates**.</span></span> <span data-ttu-id="3710b-128">Tím se otevře **restartování je aktualizace systému čekající toocomplete** okno se seznamem virtuálních počítačů, je nutné, aby toorestart toocomplete hello použít proces aktualizace systému.</span><span class="sxs-lookup"><span data-stu-id="3710b-128">This opens **A restart is pending toocomplete system updates** blade displaying a list of VMs that you need toorestart toocomplete hello apply system updates process.</span></span>

   ![Čeká se na restartování][6]

<span data-ttu-id="3710b-130">Restartujte hello virtuálních počítačů z Azure toocomplete hello procesu.</span><span class="sxs-lookup"><span data-stu-id="3710b-130">Restart hello VM from Azure toocomplete hello process.</span></span>

## <a name="see-also"></a><span data-ttu-id="3710b-131">Viz také</span><span class="sxs-lookup"><span data-stu-id="3710b-131">See also</span></span>
<span data-ttu-id="3710b-132">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="3710b-132">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="3710b-133">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="3710b-133">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="3710b-134">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="3710b-134">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="3710b-135">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="3710b-135">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="3710b-136">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="3710b-136">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="3710b-137">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="3710b-137">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="3710b-138">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="3710b-138">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="3710b-139">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="3710b-139">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
