---
title: "aaaAzure Security Center a Linux virtuálních počítačů v Azure | Microsoft Docs"
description: "Další informace o zabezpečení pro virtuální počítač Azure Linux s Azure Security Center."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/07/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7aa0de35fb311457e769f152c8575ec43e41c39c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a><span data-ttu-id="e99c2-103">Sledování zabezpečení virtuálního počítače pomocí Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="e99c2-103">Monitor virtual machine security by using Azure Security Center</span></span>

<span data-ttu-id="e99c2-104">Azure Security Center můžete získat přehled o Azure prostředku postupy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e99c2-104">Azure Security Center can help you gain visibility into your Azure resource security practices.</span></span> <span data-ttu-id="e99c2-105">Security Center nabízí integrované zabezpečení monitorování.</span><span class="sxs-lookup"><span data-stu-id="e99c2-105">Security Center offers integrated security monitoring.</span></span> <span data-ttu-id="e99c2-106">Může zjistit hrozeb, které jinak může nevšimli.</span><span class="sxs-lookup"><span data-stu-id="e99c2-106">It can detect threats that otherwise might go unnoticed.</span></span> <span data-ttu-id="e99c2-107">V tomto kurzu se dozvíte o Azure Security Center a postup:</span><span class="sxs-lookup"><span data-stu-id="e99c2-107">In this tutorial, you learn about Azure Security Center, and how to:</span></span>
 
> [!div class="checklist"]
> * <span data-ttu-id="e99c2-108">Nastavení shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="e99c2-108">Set up data collection</span></span>
> * <span data-ttu-id="e99c2-109">Nastavte zásady zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e99c2-109">Set up security policies</span></span>
> * <span data-ttu-id="e99c2-110">Zobrazení a opravte problémy s konfigurací stavu</span><span class="sxs-lookup"><span data-stu-id="e99c2-110">View and fix configuration health issues</span></span>
> * <span data-ttu-id="e99c2-111">Zkontrolujte zjištěnými hrozbami</span><span class="sxs-lookup"><span data-stu-id="e99c2-111">Review detected threats</span></span>  

## <a name="security-center-overview"></a><span data-ttu-id="e99c2-112">Security Center – přehled</span><span class="sxs-lookup"><span data-stu-id="e99c2-112">Security Center overview</span></span>

<span data-ttu-id="e99c2-113">Security Center identifikuje potenciální potíže s konfigurací virtuálního počítače (VM) a cílem bezpečnostní hrozby.</span><span class="sxs-lookup"><span data-stu-id="e99c2-113">Security Center identifies potential virtual machine (VM) configuration issues and targeted security threats.</span></span> <span data-ttu-id="e99c2-114">To může zahrnovat virtuální počítače, které chybí skupin zabezpečení sítě, nezašifrované disků a útoku hrubou silou protokol RDP (Remote Desktop).</span><span class="sxs-lookup"><span data-stu-id="e99c2-114">These might include VMs that are missing network security groups, unencrypted disks, and brute-force Remote Desktop Protocol (RDP) attacks.</span></span> <span data-ttu-id="e99c2-115">informace Hello je zobrazena na řídicím panelu Security Center hello ve snadno čitelném grafy.</span><span class="sxs-lookup"><span data-stu-id="e99c2-115">hello information is shown on hello Security Center dashboard in easy-to-read graphs.</span></span>

<span data-ttu-id="e99c2-116">tooaccess hello řídicího panelu Security Center, v hello portál Azure, v nabídce hello, vyberte **Security Center**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-116">tooaccess hello Security Center dashboard, in hello Azure portal, on hello menu, select  **Security Center**.</span></span> <span data-ttu-id="e99c2-117">Na řídicím panelu hello můžete zobrazit stav zabezpečení hello prostředí Azure, najít počet aktuální doporučení a zobrazit aktuální stav výstrahy threat hello.</span><span class="sxs-lookup"><span data-stu-id="e99c2-117">On hello dashboard, you can see hello security health of your Azure environment, find a count of current recommendations, and view hello current state of threat alerts.</span></span> <span data-ttu-id="e99c2-118">Každý toosee vysoké úrovně graf můžete rozbalit podrobněji.</span><span class="sxs-lookup"><span data-stu-id="e99c2-118">You can expand each high-level chart toosee more detail.</span></span>

![Řídicí panel Security Center](./media/tutorial-azure-security/asc-dash.png)

<span data-ttu-id="e99c2-120">Security Center překročí data zjišťování tooprovide doporučení pro problémy, které zjistí.</span><span class="sxs-lookup"><span data-stu-id="e99c2-120">Security Center goes beyond data discovery tooprovide recommendations for issues that it detects.</span></span> <span data-ttu-id="e99c2-121">Například pokud virtuální počítač byl nasazen bez skupiny zabezpečení služby připojené síti, Security Center zobrazuje doporučení, s nápravy kroky, které můžete provést.</span><span class="sxs-lookup"><span data-stu-id="e99c2-121">For example, if a VM was deployed without an attached network security group, Security Center displays a recommendation, with remediation steps you can take.</span></span> <span data-ttu-id="e99c2-122">Získáte automatizovanou nápravu, aniž byste museli opustit hello kontextu služby Security Center.</span><span class="sxs-lookup"><span data-stu-id="e99c2-122">You get automated remediation without leaving hello context of Security Center.</span></span>  

![Doporučení](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a><span data-ttu-id="e99c2-124">Nastavení shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="e99c2-124">Set up data collection</span></span>

<span data-ttu-id="e99c2-125">Než můžete získat viditelnost do zabezpečení konfigurací virtuálních počítačů, je třeba tooset procesu shromažďování dat Security Center.</span><span class="sxs-lookup"><span data-stu-id="e99c2-125">Before you can get visibility into VM security configurations, you need tooset up Security Center data collection.</span></span> <span data-ttu-id="e99c2-126">To zahrnuje zapnutí shromažďování dat a vytvoření data toohold shromážděných účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e99c2-126">This involves turning on data collection and creating an Azure storage account toohold collected data.</span></span> 

1. <span data-ttu-id="e99c2-127">Na řídicím panelu Security Center hello, klikněte na tlačítko **zásady zabezpečení**a potom vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="e99c2-127">On hello Security Center dashboard, click **Security policy**, and then select your subscription.</span></span> 
2. <span data-ttu-id="e99c2-128">Pro **shromažďování dat**, vyberte **na**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-128">For **Data collection**, select **On**.</span></span>
3. <span data-ttu-id="e99c2-129">Vyberte účet úložiště toocreate **zvolte účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-129">toocreate a storage account, select **Choose a storage account**.</span></span> <span data-ttu-id="e99c2-130">Pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-130">Then, select **OK**.</span></span>
4. <span data-ttu-id="e99c2-131">Na hello **zásady zabezpečení** vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-131">On hello **Security Policy** blade, select **Save**.</span></span> 

<span data-ttu-id="e99c2-132">agenta pro sběr dat Hello Security Center je nainstalován na všech virtuálních počítačů a shromažďování dat začíná.</span><span class="sxs-lookup"><span data-stu-id="e99c2-132">hello Security Center data collection agent is then installed on all VMs, and data collection begins.</span></span> 

## <a name="set-up-a-security-policy"></a><span data-ttu-id="e99c2-133">Nastavte zásady zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e99c2-133">Set up a security policy</span></span>

<span data-ttu-id="e99c2-134">Zásady zabezpečení jsou použité toodefine hello položky, pro které Security Center shromažďuje data a díky doporučení.</span><span class="sxs-lookup"><span data-stu-id="e99c2-134">Security policies are used toodefine hello items for which Security Center collects data and makes recommendations.</span></span> <span data-ttu-id="e99c2-135">Můžete použít jiné bezpečnostní zásady toodifferent sady prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e99c2-135">You can apply different security policies toodifferent sets of Azure resources.</span></span> <span data-ttu-id="e99c2-136">I když ve výchozím nastavení jsou prostředky Azure porovnán s všechny položky zásad, můžete vypnout jednotlivé zásady položky pro všechny prostředky Azure nebo pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e99c2-136">Although by default Azure resources are evaluated against all policy items, you can turn off individual policy items for all Azure resources or for a resource group.</span></span> <span data-ttu-id="e99c2-137">Podrobné informace o zásadách zabezpečení Security Center najdete v tématu [nastavení zásad zabezpečení v Azure Security Center](../../security-center/security-center-policies.md).</span><span class="sxs-lookup"><span data-stu-id="e99c2-137">For in-depth information about Security Center security policies, see [Set security policies in Azure Security Center](../../security-center/security-center-policies.md).</span></span> 

<span data-ttu-id="e99c2-138">tooset si zásady zabezpečení pro všechny prostředky Azure:</span><span class="sxs-lookup"><span data-stu-id="e99c2-138">tooset up a security policy for all Azure resources:</span></span>

1. <span data-ttu-id="e99c2-139">Na řídicím panelu Security Center hello, vyberte **zásady zabezpečení**a potom vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="e99c2-139">On hello Security Center dashboard, select **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="e99c2-140">Vyberte **zásada Zabránění**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-140">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="e99c2-141">Zapněte nebo vypněte zásady položky, které chcete tooapply tooall prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e99c2-141">Turn on or turn off policy items that you want tooapply tooall Azure resources.</span></span>
4. <span data-ttu-id="e99c2-142">Jakmile budete hotovi, vyberte nastavení, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-142">When you're finished selecting your settings, select **OK**.</span></span>
5. <span data-ttu-id="e99c2-143">Na hello **zásady zabezpečení** vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-143">On hello **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="e99c2-144">tooset si zásadu pro určité skupiny zdrojů:</span><span class="sxs-lookup"><span data-stu-id="e99c2-144">tooset up a policy for a specific resource group:</span></span>

1. <span data-ttu-id="e99c2-145">Na řídicím panelu Security Center hello, vyberte **zásady zabezpečení**a pak vyberte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e99c2-145">On hello Security Center dashboard, select **Security policy**, and then select a resource group.</span></span>
2. <span data-ttu-id="e99c2-146">Vyberte **zásada Zabránění**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-146">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="e99c2-147">Zapnout nebo vypnout zásady položky, že chcete skupinu prostředků toohello tooapply.</span><span class="sxs-lookup"><span data-stu-id="e99c2-147">Turn on or turn off policy items that you want tooapply toohello resource group.</span></span>
4. <span data-ttu-id="e99c2-148">V části **DĚDIČNOSTI**, vyberte **jedinečný**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-148">Under **INHERITANCE**, select **Unique**.</span></span>
5. <span data-ttu-id="e99c2-149">Jakmile budete hotovi, vyberte nastavení, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-149">When you're finished selecting your settings, select **OK**.</span></span>
6. <span data-ttu-id="e99c2-150">Na hello **zásady zabezpečení** vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-150">On hello **Security policy** blade, select **Save**.</span></span>  

<span data-ttu-id="e99c2-151">Také můžete vypnout shromažďování dat pro určité skupiny zdrojů na této stránce.</span><span class="sxs-lookup"><span data-stu-id="e99c2-151">You also can turn off data collection for a specific resource group on this page.</span></span>

<span data-ttu-id="e99c2-152">V následujícím příkladu hello, byl vytvořen jedinečné zásady pro skupinu prostředků s názvem *myResoureGroup*.</span><span class="sxs-lookup"><span data-stu-id="e99c2-152">In hello following example, a unique policy has been created for a resource group named *myResoureGroup*.</span></span> <span data-ttu-id="e99c2-153">V této zásadě jsou vypnuté disku šifrování a webové aplikace brány firewall doporučení.</span><span class="sxs-lookup"><span data-stu-id="e99c2-153">In this policy, disk encryption and web application firewall recommendations are turned off.</span></span>

![Jedinečné zásady](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a><span data-ttu-id="e99c2-155">Zobrazit stav konfigurace virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e99c2-155">View VM configuration health</span></span>

<span data-ttu-id="e99c2-156">Po zapnutá shromažďování dat a nastavit zásadu zabezpečení, Security Center začne tooprovide výstrahy a doporučení.</span><span class="sxs-lookup"><span data-stu-id="e99c2-156">After you've turned on data collection and set a security policy, Security Center begins tooprovide alerts and recommendations.</span></span> <span data-ttu-id="e99c2-157">Při nasazování virtuálních počítačů, je nainstalována agenta pro sběr dat hello.</span><span class="sxs-lookup"><span data-stu-id="e99c2-157">As VMs are deployed, hello data collection agent is installed.</span></span> <span data-ttu-id="e99c2-158">Security Center je pak naplněný daty pro hello nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e99c2-158">Security Center is then populated with data for hello new VMs.</span></span> <span data-ttu-id="e99c2-159">Podrobné informace o stavu konfigurace virtuálních počítačů najdete v tématu [chránit virtuální počítače ve službě Security Center](../../security-center/security-center-virtual-machine-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="e99c2-159">For in-depth information about VM configuration health, see [Protect your VMs in Security Center](../../security-center/security-center-virtual-machine-recommendations.md).</span></span> 

<span data-ttu-id="e99c2-160">Jak se data shromažďují, je agregován hello stav prostředku pro každý virtuální počítač a souvisejících prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e99c2-160">As data is collected, hello resource health for each VM and related Azure resource is aggregated.</span></span> <span data-ttu-id="e99c2-161">Hello informace se zobrazí v grafu aplikace snadno čitelné.</span><span class="sxs-lookup"><span data-stu-id="e99c2-161">hello information is shown in an easy-to-read chart.</span></span> 

<span data-ttu-id="e99c2-162">stav prostředku tooview:</span><span class="sxs-lookup"><span data-stu-id="e99c2-162">tooview resource health:</span></span>

1.  <span data-ttu-id="e99c2-163">Na řídicím panelu Security Center hello v části **stav zabezpečení prostředků**, vyberte **výpočetní**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-163">On hello Security Center dashboard, under **Resource security health**, select **Compute**.</span></span> 
2.  <span data-ttu-id="e99c2-164">Na hello **výpočetní** vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-164">On hello **Compute** blade, select **Virtual machines**.</span></span> <span data-ttu-id="e99c2-165">Toto zobrazení obsahuje souhrn stavu hello konfigurace pro všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e99c2-165">This view provides a summary of hello configuration status for all your VMs.</span></span>

![Výpočetní stavu](./media/tutorial-azure-security/compute-health.png)

<span data-ttu-id="e99c2-167">toosee všechna doporučení pro virtuální počítač, vyberte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e99c2-167">toosee all recommendations for a VM, select hello VM.</span></span> <span data-ttu-id="e99c2-168">Doporučení a nápravu, jsou popsané v podrobněji hello další části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e99c2-168">Recommendations and remediation are covered in more detail in hello next section of this tutorial.</span></span>

## <a name="remediate-configuration-issues"></a><span data-ttu-id="e99c2-169">Opravit problémy s konfigurací</span><span class="sxs-lookup"><span data-stu-id="e99c2-169">Remediate configuration issues</span></span>

<span data-ttu-id="e99c2-170">Po zahájení toopopulate s konfigurační data Security Center doporučení jsou provedená na základě hello zásady zabezpečení, kterou vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="e99c2-170">After Security Center begins toopopulate with configuration data, recommendations are made based on hello security policy you set up.</span></span> <span data-ttu-id="e99c2-171">Pokud virtuální počítač vytvořený bez skupinu zabezpečení sítě spojenou se provádí doporučení pro instanci toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="e99c2-171">For instance, if a VM was set up without an associated network security group, a recommendation is made toocreate one.</span></span> 

<span data-ttu-id="e99c2-172">toosee seznam všech doporučení:</span><span class="sxs-lookup"><span data-stu-id="e99c2-172">toosee a list of all recommendations:</span></span> 

1. <span data-ttu-id="e99c2-173">Na řídicím panelu Security Center hello, vyberte **doporučení**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-173">On hello Security Center dashboard, select **Recommendations**.</span></span>
2. <span data-ttu-id="e99c2-174">Vyberte konkrétní doporučení.</span><span class="sxs-lookup"><span data-stu-id="e99c2-174">Select a specific recommendation.</span></span> <span data-ttu-id="e99c2-175">Zobrazí se seznam všech prostředků, pro kterou platí doporučení hello.</span><span class="sxs-lookup"><span data-stu-id="e99c2-175">A list of all resources for which hello recommendation applies appears.</span></span>
3. <span data-ttu-id="e99c2-176">tooapply doporučení, vyberte konkrétní prostředku.</span><span class="sxs-lookup"><span data-stu-id="e99c2-176">tooapply a recommendation, select a specific resource.</span></span> 
4. <span data-ttu-id="e99c2-177">Postupujte podle pokynů hello nápravy kroky.</span><span class="sxs-lookup"><span data-stu-id="e99c2-177">Follow hello instructions for remediation steps.</span></span> 

<span data-ttu-id="e99c2-178">V mnoha případech Security Center nabízí řešitelné postup můžete provést tooaddress doporučení aniž byste museli opustit Security Center.</span><span class="sxs-lookup"><span data-stu-id="e99c2-178">In many cases, Security Center provides actionable steps you can take tooaddress a recommendation without leaving Security Center.</span></span> <span data-ttu-id="e99c2-179">V následujícím příkladu hello Security Center zjišťuje skupinu zabezpečení sítě, který má neomezený příchozího pravidla.</span><span class="sxs-lookup"><span data-stu-id="e99c2-179">In hello following example, Security Center detects a network security group that has an unrestricted inbound rule.</span></span> <span data-ttu-id="e99c2-180">Na stránce hello doporučení, můžete vybrat hello **upravit příchozí pravidla** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e99c2-180">On hello recommendation page, you can select hello **Edit inbound rules** button.</span></span> <span data-ttu-id="e99c2-181">Zobrazí se uživatelské rozhraní, které je potřebné toomodify hello pravidlo Hello.</span><span class="sxs-lookup"><span data-stu-id="e99c2-181">hello UI that is needed toomodify hello rule appears.</span></span> 

![Doporučení](./media/tutorial-azure-security/remediation.png)

<span data-ttu-id="e99c2-183">Podle doporučení opraví, jsou označeny jako vyřešené.</span><span class="sxs-lookup"><span data-stu-id="e99c2-183">As recommendations are remediated, they are marked as resolved.</span></span> 

## <a name="view-detected-threats"></a><span data-ttu-id="e99c2-184">Zobrazit zjištěnými hrozbami</span><span class="sxs-lookup"><span data-stu-id="e99c2-184">View detected threats</span></span>

<span data-ttu-id="e99c2-185">Kromě toho doporučené konfigurace tooresource, Security Center zobrazuje výstrah o zjištěných hrozbách.</span><span class="sxs-lookup"><span data-stu-id="e99c2-185">In addition tooresource configuration recommendations, Security Center displays threat detection alerts.</span></span> <span data-ttu-id="e99c2-186">Funkce výstrahy zabezpečení Hello agreguje data shromážděná z jednotlivých virtuálních počítačů, Azure síťové protokoly a připojených partnerských řešení toodetect bezpečnostní hrozby proti prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e99c2-186">hello security alerts feature aggregates data collected from each VM, Azure networking logs, and connected partner solutions toodetect security threats against Azure resources.</span></span> <span data-ttu-id="e99c2-187">Podrobné informace o možnostech detekce hrozeb Security Center najdete v tématu [funkce zjišťování služby Azure Security Center](../../security-center/security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="e99c2-187">For in-depth information about Security Center threat detection capabilities, see [Azure Security Center detection capabilities](../../security-center/security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="e99c2-188">Funkce výstrahy zabezpečení Hello vyžaduje hello Security Center cenová úroveň toobe vyšší z *volné* příliš*standardní*.</span><span class="sxs-lookup"><span data-stu-id="e99c2-188">hello security alerts feature requires hello Security Center pricing tier toobe increased from *Free* too*Standard*.</span></span> <span data-ttu-id="e99c2-189">30denní **bezplatnou zkušební verzi** je k dispozici, když přesouváte toothis vyšší cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="e99c2-189">A 30-day **free trial** is available when you move toothis higher pricing tier.</span></span> 

<span data-ttu-id="e99c2-190">cenová úroveň toochange hello:</span><span class="sxs-lookup"><span data-stu-id="e99c2-190">toochange hello pricing tier:</span></span>  

1. <span data-ttu-id="e99c2-191">Na řídicím panelu Security Center hello, klikněte na tlačítko **zásady zabezpečení**a potom vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="e99c2-191">On hello Security Center dashboard, click **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="e99c2-192">Vyberte **cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-192">Select **Pricing tier**.</span></span>
3. <span data-ttu-id="e99c2-193">Vyberte hello novou vrstvu a pak vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-193">Select hello new tier, and then select **Select**.</span></span>
4. <span data-ttu-id="e99c2-194">Na hello **zásady zabezpečení** vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e99c2-194">On hello **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="e99c2-195">Po změně hello cenovou úroveň, grafu výstrahy zabezpečení hello začíná toopopulate podle zjištění ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e99c2-195">After you've changed hello pricing tier, hello security alerts graph begins toopopulate as security threats are detected.</span></span>

![Výstrahy zabezpečení](./media/tutorial-azure-security/security-alerts.png)

<span data-ttu-id="e99c2-197">Vyberte výstrahy tooview informace.</span><span class="sxs-lookup"><span data-stu-id="e99c2-197">Select an alert tooview information.</span></span> <span data-ttu-id="e99c2-198">Můžete například zobrazíte popis hello hrozby, čas detekce hello, všechny pokusy hrozeb a hello doporučené nápravy.</span><span class="sxs-lookup"><span data-stu-id="e99c2-198">For example, you can see a description of hello threat, hello detection time, all threat attempts, and hello recommended remediation.</span></span> <span data-ttu-id="e99c2-199">V následující ukázka hello bylo zjištěno útoku hrubou silou RDP s 294 neúspěšných pokusů protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="e99c2-199">In hello following example, an RDP brute-force attack was detected, with 294 failed RDP attempts.</span></span> <span data-ttu-id="e99c2-200">Doporučené řešení je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e99c2-200">A recommended resolution is provided.</span></span>

![Útok protokolu RDP](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a><span data-ttu-id="e99c2-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e99c2-202">Next steps</span></span>
<span data-ttu-id="e99c2-203">V tomto kurzu nastavení Azure Security Center a poté zkontrolovat virtuální počítače ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="e99c2-203">In this tutorial, you set up Azure Security Center, and then reviewed VMs in Security Center.</span></span> <span data-ttu-id="e99c2-204">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="e99c2-204">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e99c2-205">Nastavení shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="e99c2-205">Set up data collection</span></span>
> * <span data-ttu-id="e99c2-206">Nastavte zásady zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e99c2-206">Set up security policies</span></span>
> * <span data-ttu-id="e99c2-207">Zobrazení a opravte problémy s konfigurací stavu</span><span class="sxs-lookup"><span data-stu-id="e99c2-207">View and fix configuration health issues</span></span>
> * <span data-ttu-id="e99c2-208">Zkontrolujte zjištěnými hrozbami</span><span class="sxs-lookup"><span data-stu-id="e99c2-208">Review detected threats</span></span>

<span data-ttu-id="e99c2-209">Posunutí toohello další kurz toolearn informace o vytváření položek konfigurace nebo CD kanál s volaných, Githubu a Docker.</span><span class="sxs-lookup"><span data-stu-id="e99c2-209">Advance toohello next tutorial toolearn more about creating a CI/CD pipeline with Jenkins, GitHub, and Docker.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e99c2-210">Vytvoření položek konfigurace nebo CD infrastruktury s volaných Githubu a Docker</span><span class="sxs-lookup"><span data-stu-id="e99c2-210">Create CI/CD infrastructure with Jenkins, GitHub, and Docker</span></span>](tutorial-jenkins-github-docker-cicd.md)

