---
title: aaaInstall Endpoint Protection v Azure Security Center | Microsoft Docs
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** nainstalovat Endpoint Protection **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: fea891810e042e4d4f6e55094c0cd4de013ba68a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-endpoint-protection-in-azure-security-center"></a><span data-ttu-id="6759a-103">Nainstalovat službu Endpoint Protection v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="6759a-103">Install Endpoint Protection in Azure Security Center</span></span>
<span data-ttu-id="6759a-104">Azure Security Center doporučuje nainstalovat službu endpoint protection v Azure virtuální počítače (VM), pokud ještě není povolené aplikace endpoint protection.</span><span class="sxs-lookup"><span data-stu-id="6759a-104">Azure Security Center recommends that you install endpoint protection on your Azure virtual machines (VMs) if endpoint protection is not already enabled.</span></span> <span data-ttu-id="6759a-105">Toto doporučení se vztahují pouze tooWindows virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6759a-105">This recommendation applies tooWindows VMs only.</span></span>

> [!NOTE]
> <span data-ttu-id="6759a-106">Tento příklad nasazení používá Antimalware od Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="6759a-106">This example deployment uses Microsoft Antimalware.</span></span> <span data-ttu-id="6759a-107">V tématu [integrace partnera v Azure Security Center](security-center-partner-integration.md#partners-that-integrate-with-security-center) seznam partnerů integrovat Security Center.</span><span class="sxs-lookup"><span data-stu-id="6759a-107">See [Partner Integration in Azure Security Center](security-center-partner-integration.md#partners-that-integrate-with-security-center) for a list of partners integrated with Security Center.</span></span>  
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="6759a-108">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="6759a-108">Implement hello recommendation</span></span>

> [!NOTE]
> <span data-ttu-id="6759a-109">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="6759a-109">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="6759a-110">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="6759a-110">This document is not a step-by-step guide.</span></span>
>
>

1. <span data-ttu-id="6759a-111">V hello **doporučení** vyberte **nainstalovat službu Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="6759a-111">In hello **Recommendations** blade, select **Install Endpoint Protection**.</span></span>
   <span data-ttu-id="6759a-112">![Vyberte možnost nainstalovat službu Endpoint Protection][1]</span><span class="sxs-lookup"><span data-stu-id="6759a-112">![Select Install Endpoint Protection][1]</span></span>
2. <span data-ttu-id="6759a-113">Hello **nainstalovat službu Endpoint Protection** otevře se okno se seznamem virtuálních počítačů bez ochrany koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="6759a-113">hello **Install Endpoint Protection** blade opens displaying a list of VMs without endpoint protection.</span></span> <span data-ttu-id="6759a-114">Vyberte ze seznamu hello hello virtuálních počítačů, které chcete tooinstall aplikace endpoint protection na a klikněte na tlačítko **nainstalovat na virtuálních počítačích**.</span><span class="sxs-lookup"><span data-stu-id="6759a-114">Select from hello list hello VMs that you want tooinstall endpoint protection on and click **Install on VMs**.</span></span>
   <span data-ttu-id="6759a-115">![Vyberte virtuální počítače tooinstall Endpoint Protection na][2]</span><span class="sxs-lookup"><span data-stu-id="6759a-115">![Select VMs tooinstall Endpoint Protection on][2]</span></span>
3. <span data-ttu-id="6759a-116">Hello **vyberte Endpoint Protection** otevře se okno tooallow jste tooselect hello řešení ochrany koncových bodů chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="6759a-116">hello **Select Endpoint Protection** blade opens tooallow you tooselect hello endpoint protection solution you want toouse.</span></span> <span data-ttu-id="6759a-117">V tomto příkladu budeme vyberte **Antimalware od Microsoftu**.</span><span class="sxs-lookup"><span data-stu-id="6759a-117">In this example, let's select **Microsoft Antimalware**.</span></span>
   <span data-ttu-id="6759a-118">![Vyberte aplikace Endpoint Protection][3]</span><span class="sxs-lookup"><span data-stu-id="6759a-118">![Select Endpoint Protection][3]</span></span>
4. <span data-ttu-id="6759a-119">Další informace o řešení ochrany koncových bodů hello se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="6759a-119">Additional information about hello endpoint protection solution is displayed.</span></span> <span data-ttu-id="6759a-120">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6759a-120">Select **Create**.</span></span>
   <span data-ttu-id="6759a-121">![Vytvořte antimalwarové řešení][4]</span><span class="sxs-lookup"><span data-stu-id="6759a-121">![Create antimalware solution][4]</span></span>
5. <span data-ttu-id="6759a-122">Zadejte nastavení konfigurace hello vyžaduje na hello **přidat rozšíření** a pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="6759a-122">Enter hello required configuration settings on hello **Add Extension** blade, and then select **OK**.</span></span> <span data-ttu-id="6759a-123">toolearn Další informace o nastavení konfigurace hello, najdete v části [výchozí a vlastní konfigurace antimalwarových](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).</span><span class="sxs-lookup"><span data-stu-id="6759a-123">toolearn more about hello configuration settings, see [Default and Custom Antimalware Configuration](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).</span></span>

<span data-ttu-id="6759a-124">[Microsoft Antimalware](../security/azure-security-antimalware.md) je teď aktivní na hello vybrané virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6759a-124">[Microsoft Antimalware](../security/azure-security-antimalware.md) is now active on hello selected VMs.</span></span>

## <a name="see-also"></a><span data-ttu-id="6759a-125">Viz také</span><span class="sxs-lookup"><span data-stu-id="6759a-125">See also</span></span>
<span data-ttu-id="6759a-126">Tento článek ukázal, jak tooimplement hello Security Center doporučení "Nainstalovat Endpoint Protection."</span><span class="sxs-lookup"><span data-stu-id="6759a-126">This article showed you how tooimplement hello Security Center recommendation "Install Endpoint Protection."</span></span> <span data-ttu-id="6759a-127">toolearn Další informace o povolení Antimalware od Microsoftu v Azure, najdete v části hello následujícím dokumentu:</span><span class="sxs-lookup"><span data-stu-id="6759a-127">toolearn more about enabling Microsoft Antimalware in Azure, see hello following document:</span></span>

* <span data-ttu-id="6759a-128">[Antimalware od Microsoftu pro cloudové služby a virtuální počítače](../security/azure-security-antimalware.md) – zjistěte, jak toodeploy Antimalware od Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="6759a-128">[Microsoft Antimalware for Cloud Services and Virtual Machines](../security/azure-security-antimalware.md) -- Learn how toodeploy Microsoft Antimalware.</span></span>

<span data-ttu-id="6759a-129">toolearn Další informace o Security Center, najdete v části hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="6759a-129">toolearn more about Security Center, see hello following documents:</span></span>

* <span data-ttu-id="6759a-130">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6759a-130">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies.</span></span>
* <span data-ttu-id="6759a-131">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="6759a-131">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="6759a-132">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6759a-132">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="6759a-133">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6759a-133">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="6759a-134">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="6759a-134">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="6759a-135">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="6759a-135">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="6759a-136">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="6759a-136">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
