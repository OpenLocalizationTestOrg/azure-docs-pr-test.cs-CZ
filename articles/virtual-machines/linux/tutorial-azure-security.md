---
title: "Virtuální počítače Azure Security Center a Linux v Azure | Microsoft Docs"
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
ms.openlocfilehash: 7f76da8cd5f4299c64c6e99d0521829c454d8c6c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a><span data-ttu-id="197fe-103">Sledování zabezpečení virtuálního počítače pomocí Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="197fe-103">Monitor virtual machine security by using Azure Security Center</span></span>

<span data-ttu-id="197fe-104">Azure Security Center můžete získat přehled o Azure prostředku postupy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="197fe-104">Azure Security Center can help you gain visibility into your Azure resource security practices.</span></span> <span data-ttu-id="197fe-105">Security Center nabízí integrované zabezpečení monitorování.</span><span class="sxs-lookup"><span data-stu-id="197fe-105">Security Center offers integrated security monitoring.</span></span> <span data-ttu-id="197fe-106">Může zjistit hrozeb, které jinak může nevšimli.</span><span class="sxs-lookup"><span data-stu-id="197fe-106">It can detect threats that otherwise might go unnoticed.</span></span> <span data-ttu-id="197fe-107">V tomto kurzu se dozvíte o Azure Security Center a postup:</span><span class="sxs-lookup"><span data-stu-id="197fe-107">In this tutorial, you learn about Azure Security Center, and how to:</span></span>
 
> [!div class="checklist"]
> * <span data-ttu-id="197fe-108">Nastavení shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="197fe-108">Set up data collection</span></span>
> * <span data-ttu-id="197fe-109">Nastavte zásady zabezpečení</span><span class="sxs-lookup"><span data-stu-id="197fe-109">Set up security policies</span></span>
> * <span data-ttu-id="197fe-110">Zobrazení a opravte problémy s konfigurací stavu</span><span class="sxs-lookup"><span data-stu-id="197fe-110">View and fix configuration health issues</span></span>
> * <span data-ttu-id="197fe-111">Zkontrolujte zjištěnými hrozbami</span><span class="sxs-lookup"><span data-stu-id="197fe-111">Review detected threats</span></span>  

## <a name="security-center-overview"></a><span data-ttu-id="197fe-112">Security Center – přehled</span><span class="sxs-lookup"><span data-stu-id="197fe-112">Security Center overview</span></span>

<span data-ttu-id="197fe-113">Security Center identifikuje potenciální potíže s konfigurací virtuálního počítače (VM) a cílem bezpečnostní hrozby.</span><span class="sxs-lookup"><span data-stu-id="197fe-113">Security Center identifies potential virtual machine (VM) configuration issues and targeted security threats.</span></span> <span data-ttu-id="197fe-114">To může zahrnovat virtuální počítače, které chybí skupin zabezpečení sítě, nezašifrované disků a útoku hrubou silou protokol RDP (Remote Desktop).</span><span class="sxs-lookup"><span data-stu-id="197fe-114">These might include VMs that are missing network security groups, unencrypted disks, and brute-force Remote Desktop Protocol (RDP) attacks.</span></span> <span data-ttu-id="197fe-115">Informace se zobrazí na řídicím panelu Security Center v grafy snadno čitelné.</span><span class="sxs-lookup"><span data-stu-id="197fe-115">The information is shown on the Security Center dashboard in easy-to-read graphs.</span></span>

<span data-ttu-id="197fe-116">Chcete-li přístup k řídicímu panelu Security Center na portálu Azure, v nabídce vyberte **Security Center**.</span><span class="sxs-lookup"><span data-stu-id="197fe-116">To access the Security Center dashboard, in the Azure portal, on the menu, select  **Security Center**.</span></span> <span data-ttu-id="197fe-117">Na řídicím panelu můžete zobrazit stav zabezpečení prostředí Azure, najít a počet aktuální doporučení a zobrazit aktuální stav výstrahy hrozeb.</span><span class="sxs-lookup"><span data-stu-id="197fe-117">On the dashboard, you can see the security health of your Azure environment, find a count of current recommendations, and view the current state of threat alerts.</span></span> <span data-ttu-id="197fe-118">Můžete rozbalit každý vysoké úrovně grafu pro zobrazení dalších podrobností.</span><span class="sxs-lookup"><span data-stu-id="197fe-118">You can expand each high-level chart to see more detail.</span></span>

![Řídicí panel Security Center](./media/tutorial-azure-security/asc-dash.png)

<span data-ttu-id="197fe-120">Security Center překročí data zjišťování poskytnout doporučení pro problémy, které zjistí.</span><span class="sxs-lookup"><span data-stu-id="197fe-120">Security Center goes beyond data discovery to provide recommendations for issues that it detects.</span></span> <span data-ttu-id="197fe-121">Například pokud virtuální počítač byl nasazen bez skupiny zabezpečení služby připojené síti, Security Center zobrazuje doporučení, s nápravy kroky, které můžete provést.</span><span class="sxs-lookup"><span data-stu-id="197fe-121">For example, if a VM was deployed without an attached network security group, Security Center displays a recommendation, with remediation steps you can take.</span></span> <span data-ttu-id="197fe-122">Získáte automatizovanou nápravu, aniž byste museli opustit kontext služby Security Center.</span><span class="sxs-lookup"><span data-stu-id="197fe-122">You get automated remediation without leaving the context of Security Center.</span></span>  

![Doporučení](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a><span data-ttu-id="197fe-124">Nastavení shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="197fe-124">Set up data collection</span></span>

<span data-ttu-id="197fe-125">Než do zabezpečení konfigurací virtuálních počítačů můžete získat viditelnost, budete muset nastavit shromažďování dat Security Center.</span><span class="sxs-lookup"><span data-stu-id="197fe-125">Before you can get visibility into VM security configurations, you need to set up Security Center data collection.</span></span> <span data-ttu-id="197fe-126">To zahrnuje zapnutí shromažďování dat a vytvoření účtu úložiště Azure pro uložení shromážděná data.</span><span class="sxs-lookup"><span data-stu-id="197fe-126">This involves turning on data collection and creating an Azure storage account to hold collected data.</span></span> 

1. <span data-ttu-id="197fe-127">Na řídicím panelu Security Center klikněte na tlačítko **zásady zabezpečení**a potom vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="197fe-127">On the Security Center dashboard, click **Security policy**, and then select your subscription.</span></span> 
2. <span data-ttu-id="197fe-128">Pro **shromažďování dat**, vyberte **na**.</span><span class="sxs-lookup"><span data-stu-id="197fe-128">For **Data collection**, select **On**.</span></span>
3. <span data-ttu-id="197fe-129">Chcete-li vytvořit účet úložiště, vyberte **zvolte účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="197fe-129">To create a storage account, select **Choose a storage account**.</span></span> <span data-ttu-id="197fe-130">Pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="197fe-130">Then, select **OK**.</span></span>
4. <span data-ttu-id="197fe-131">Na **zásady zabezpečení** vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="197fe-131">On the **Security Policy** blade, select **Save**.</span></span> 

<span data-ttu-id="197fe-132">Agenta pro sběr dat Security Center je nainstalován na všech virtuálních počítačů a shromažďování dat začíná.</span><span class="sxs-lookup"><span data-stu-id="197fe-132">The Security Center data collection agent is then installed on all VMs, and data collection begins.</span></span> 

## <a name="set-up-a-security-policy"></a><span data-ttu-id="197fe-133">Nastavte zásady zabezpečení</span><span class="sxs-lookup"><span data-stu-id="197fe-133">Set up a security policy</span></span>

<span data-ttu-id="197fe-134">Zásady zabezpečení se používá k definování položky, pro které Security Center shromažďuje data a díky doporučení.</span><span class="sxs-lookup"><span data-stu-id="197fe-134">Security policies are used to define the items for which Security Center collects data and makes recommendations.</span></span> <span data-ttu-id="197fe-135">Můžete použít jiné bezpečnostní zásady pro různé skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="197fe-135">You can apply different security policies to different sets of Azure resources.</span></span> <span data-ttu-id="197fe-136">I když ve výchozím nastavení jsou prostředky Azure porovnán s všechny položky zásad, můžete vypnout jednotlivé zásady položky pro všechny prostředky Azure nebo pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="197fe-136">Although by default Azure resources are evaluated against all policy items, you can turn off individual policy items for all Azure resources or for a resource group.</span></span> <span data-ttu-id="197fe-137">Podrobné informace o zásadách zabezpečení Security Center najdete v tématu [nastavení zásad zabezpečení v Azure Security Center](../../security-center/security-center-policies.md).</span><span class="sxs-lookup"><span data-stu-id="197fe-137">For in-depth information about Security Center security policies, see [Set security policies in Azure Security Center](../../security-center/security-center-policies.md).</span></span> 

<span data-ttu-id="197fe-138">Nastavení zásad zabezpečení pro všechny prostředky Azure:</span><span class="sxs-lookup"><span data-stu-id="197fe-138">To set up a security policy for all Azure resources:</span></span>

1. <span data-ttu-id="197fe-139">Na řídicím panelu Security Center, vyberte **zásady zabezpečení**a potom vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="197fe-139">On the Security Center dashboard, select **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="197fe-140">Vyberte **zásada Zabránění**.</span><span class="sxs-lookup"><span data-stu-id="197fe-140">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="197fe-141">Zapněte nebo vypněte zásady položky, které chcete použít pro všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="197fe-141">Turn on or turn off policy items that you want to apply to all Azure resources.</span></span>
4. <span data-ttu-id="197fe-142">Jakmile budete hotovi, vyberte nastavení, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="197fe-142">When you're finished selecting your settings, select **OK**.</span></span>
5. <span data-ttu-id="197fe-143">Na **zásady zabezpečení** vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="197fe-143">On the **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="197fe-144">Nastavení zásad pro určité skupiny zdrojů:</span><span class="sxs-lookup"><span data-stu-id="197fe-144">To set up a policy for a specific resource group:</span></span>

1. <span data-ttu-id="197fe-145">Na řídicím panelu Security Center, vyberte **zásady zabezpečení**a pak vyberte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="197fe-145">On the Security Center dashboard, select **Security policy**, and then select a resource group.</span></span>
2. <span data-ttu-id="197fe-146">Vyberte **zásada Zabránění**.</span><span class="sxs-lookup"><span data-stu-id="197fe-146">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="197fe-147">Zapněte nebo vypněte zásady položky, které chcete použít ke skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="197fe-147">Turn on or turn off policy items that you want to apply to the resource group.</span></span>
4. <span data-ttu-id="197fe-148">V části **DĚDIČNOSTI**, vyberte **jedinečný**.</span><span class="sxs-lookup"><span data-stu-id="197fe-148">Under **INHERITANCE**, select **Unique**.</span></span>
5. <span data-ttu-id="197fe-149">Jakmile budete hotovi, vyberte nastavení, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="197fe-149">When you're finished selecting your settings, select **OK**.</span></span>
6. <span data-ttu-id="197fe-150">Na **zásady zabezpečení** vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="197fe-150">On the **Security policy** blade, select **Save**.</span></span>  

<span data-ttu-id="197fe-151">Také můžete vypnout shromažďování dat pro určité skupiny zdrojů na této stránce.</span><span class="sxs-lookup"><span data-stu-id="197fe-151">You also can turn off data collection for a specific resource group on this page.</span></span>

<span data-ttu-id="197fe-152">V následujícím příkladu, byl vytvořen jedinečné zásady pro skupinu prostředků s názvem *myResoureGroup*.</span><span class="sxs-lookup"><span data-stu-id="197fe-152">In the following example, a unique policy has been created for a resource group named *myResoureGroup*.</span></span> <span data-ttu-id="197fe-153">V této zásadě jsou vypnuté disku šifrování a webové aplikace brány firewall doporučení.</span><span class="sxs-lookup"><span data-stu-id="197fe-153">In this policy, disk encryption and web application firewall recommendations are turned off.</span></span>

![Jedinečné zásady](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a><span data-ttu-id="197fe-155">Zobrazit stav konfigurace virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="197fe-155">View VM configuration health</span></span>

<span data-ttu-id="197fe-156">Po zapnutá shromažďování dat a nastavit zásadu zabezpečení, Security Center začne poskytovat výstrahy a doporučení.</span><span class="sxs-lookup"><span data-stu-id="197fe-156">After you've turned on data collection and set a security policy, Security Center begins to provide alerts and recommendations.</span></span> <span data-ttu-id="197fe-157">Při nasazování virtuálních počítačů, agenta pro sběr dat je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="197fe-157">As VMs are deployed, the data collection agent is installed.</span></span> <span data-ttu-id="197fe-158">Security Center je pak naplněný daty pro nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="197fe-158">Security Center is then populated with data for the new VMs.</span></span> <span data-ttu-id="197fe-159">Podrobné informace o stavu konfigurace virtuálních počítačů najdete v tématu [chránit virtuální počítače ve službě Security Center](../../security-center/security-center-virtual-machine-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="197fe-159">For in-depth information about VM configuration health, see [Protect your VMs in Security Center](../../security-center/security-center-virtual-machine-recommendations.md).</span></span> 

<span data-ttu-id="197fe-160">Jak se data shromažďují, je agregován stav prostředku pro každý virtuální počítač a souvisejících prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="197fe-160">As data is collected, the resource health for each VM and related Azure resource is aggregated.</span></span> <span data-ttu-id="197fe-161">Informace jsou zobrazeny v diagramu snadno čitelné.</span><span class="sxs-lookup"><span data-stu-id="197fe-161">The information is shown in an easy-to-read chart.</span></span> 

<span data-ttu-id="197fe-162">Chcete-li zobrazit stav prostředku:</span><span class="sxs-lookup"><span data-stu-id="197fe-162">To view resource health:</span></span>

1.  <span data-ttu-id="197fe-163">Na zabezpečení Center řídicího panelu, v části **stav zabezpečení prostředků**, vyberte **výpočetní**.</span><span class="sxs-lookup"><span data-stu-id="197fe-163">On the Security Center dashboard, under **Resource security health**, select **Compute**.</span></span> 
2.  <span data-ttu-id="197fe-164">Na **výpočetní** vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="197fe-164">On the **Compute** blade, select **Virtual machines**.</span></span> <span data-ttu-id="197fe-165">Toto zobrazení obsahuje souhrn stavu konfigurace pro všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="197fe-165">This view provides a summary of the configuration status for all your VMs.</span></span>

![Výpočetní stavu](./media/tutorial-azure-security/compute-health.png)

<span data-ttu-id="197fe-167">Pokud chcete zobrazit všechna doporučení pro virtuální počítač, vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="197fe-167">To see all recommendations for a VM, select the VM.</span></span> <span data-ttu-id="197fe-168">Doporučení a náprava jsou podrobněji v další části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="197fe-168">Recommendations and remediation are covered in more detail in the next section of this tutorial.</span></span>

## <a name="remediate-configuration-issues"></a><span data-ttu-id="197fe-169">Opravit problémy s konfigurací</span><span class="sxs-lookup"><span data-stu-id="197fe-169">Remediate configuration issues</span></span>

<span data-ttu-id="197fe-170">Po zahájení Security Center k naplnění dat konfigurace doporučení jsou provedená na základě zásad zabezpečení, kterou vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="197fe-170">After Security Center begins to populate with configuration data, recommendations are made based on the security policy you set up.</span></span> <span data-ttu-id="197fe-171">Například pokud virtuální počítač vytvořený bez skupinu zabezpečení sítě spojenou doporučení přišla k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="197fe-171">For instance, if a VM was set up without an associated network security group, a recommendation is made to create one.</span></span> 

<span data-ttu-id="197fe-172">Chcete-li zobrazit seznam všech doporučení:</span><span class="sxs-lookup"><span data-stu-id="197fe-172">To see a list of all recommendations:</span></span> 

1. <span data-ttu-id="197fe-173">Na řídicím panelu Security Center, vyberte **doporučení**.</span><span class="sxs-lookup"><span data-stu-id="197fe-173">On the Security Center dashboard, select **Recommendations**.</span></span>
2. <span data-ttu-id="197fe-174">Vyberte konkrétní doporučení.</span><span class="sxs-lookup"><span data-stu-id="197fe-174">Select a specific recommendation.</span></span> <span data-ttu-id="197fe-175">Zobrazí se seznam všech prostředků, pro kterou platí doporučení.</span><span class="sxs-lookup"><span data-stu-id="197fe-175">A list of all resources for which the recommendation applies appears.</span></span>
3. <span data-ttu-id="197fe-176">Chcete-li použít doporučení, vyberte konkrétní prostředku.</span><span class="sxs-lookup"><span data-stu-id="197fe-176">To apply a recommendation, select a specific resource.</span></span> 
4. <span data-ttu-id="197fe-177">Postupujte podle pokynů pro nápravu kroky.</span><span class="sxs-lookup"><span data-stu-id="197fe-177">Follow the instructions for remediation steps.</span></span> 

<span data-ttu-id="197fe-178">V mnoha případech Security Center nabízí řešitelné kroky, které můžete provést k vyřešení doporučení, aniž byste museli opustit Security Center.</span><span class="sxs-lookup"><span data-stu-id="197fe-178">In many cases, Security Center provides actionable steps you can take to address a recommendation without leaving Security Center.</span></span> <span data-ttu-id="197fe-179">V následujícím příkladu Security Center zjišťuje skupinu zabezpečení sítě, který má neomezený příchozího pravidla.</span><span class="sxs-lookup"><span data-stu-id="197fe-179">In the following example, Security Center detects a network security group that has an unrestricted inbound rule.</span></span> <span data-ttu-id="197fe-180">Na stránce doporučení můžete vybrat **upravit příchozí pravidla** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="197fe-180">On the recommendation page, you can select the **Edit inbound rules** button.</span></span> <span data-ttu-id="197fe-181">Zobrazí se uživatelské rozhraní, které je potřeba upravit pravidlo.</span><span class="sxs-lookup"><span data-stu-id="197fe-181">The UI that is needed to modify the rule appears.</span></span> 

![Doporučení](./media/tutorial-azure-security/remediation.png)

<span data-ttu-id="197fe-183">Podle doporučení opraví, jsou označeny jako vyřešené.</span><span class="sxs-lookup"><span data-stu-id="197fe-183">As recommendations are remediated, they are marked as resolved.</span></span> 

## <a name="view-detected-threats"></a><span data-ttu-id="197fe-184">Zobrazit zjištěnými hrozbami</span><span class="sxs-lookup"><span data-stu-id="197fe-184">View detected threats</span></span>

<span data-ttu-id="197fe-185">Kromě doporučené konfigurace prostředků Security Center zobrazuje výstrahy detekce hrozeb.</span><span class="sxs-lookup"><span data-stu-id="197fe-185">In addition to resource configuration recommendations, Security Center displays threat detection alerts.</span></span> <span data-ttu-id="197fe-186">Funkce výstrahy zabezpečení agreguje data shromážděná z jednotlivých virtuálních počítačů Azure síťové protokoly a připojených partnerských řešení ke zjištění ohrožení zabezpečení proti prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="197fe-186">The security alerts feature aggregates data collected from each VM, Azure networking logs, and connected partner solutions to detect security threats against Azure resources.</span></span> <span data-ttu-id="197fe-187">Podrobné informace o možnostech detekce hrozeb Security Center najdete v tématu [funkce zjišťování služby Azure Security Center](../../security-center/security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="197fe-187">For in-depth information about Security Center threat detection capabilities, see [Azure Security Center detection capabilities](../../security-center/security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="197fe-188">Funkce výstrahy zabezpečení vyžaduje Security Center cenová úroveň zvýšit z *volné* k *standardní*.</span><span class="sxs-lookup"><span data-stu-id="197fe-188">The security alerts feature requires the Security Center pricing tier to be increased from *Free* to *Standard*.</span></span> <span data-ttu-id="197fe-189">30denní **bezplatnou zkušební verzi** je k dispozici, když přesouváte vyšší cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="197fe-189">A 30-day **free trial** is available when you move to this higher pricing tier.</span></span> 

<span data-ttu-id="197fe-190">Chcete-li změnit cenovou úroveň:</span><span class="sxs-lookup"><span data-stu-id="197fe-190">To change the pricing tier:</span></span>  

1. <span data-ttu-id="197fe-191">Na řídicím panelu Security Center klikněte na tlačítko **zásady zabezpečení**a potom vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="197fe-191">On the Security Center dashboard, click **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="197fe-192">Vyberte **cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="197fe-192">Select **Pricing tier**.</span></span>
3. <span data-ttu-id="197fe-193">Vyberte novou vrstvu a pak vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="197fe-193">Select the new tier, and then select **Select**.</span></span>
4. <span data-ttu-id="197fe-194">Na **zásady zabezpečení** vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="197fe-194">On the **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="197fe-195">Po změně cenové úrovně, začne grafu výstrahy zabezpečení k naplnění jako zjištění ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="197fe-195">After you've changed the pricing tier, the security alerts graph begins to populate as security threats are detected.</span></span>

![Výstrahy zabezpečení](./media/tutorial-azure-security/security-alerts.png)

<span data-ttu-id="197fe-197">Vyberte příslušnou výstrahu a zobrazit informace.</span><span class="sxs-lookup"><span data-stu-id="197fe-197">Select an alert to view information.</span></span> <span data-ttu-id="197fe-198">Například se zobrazí popis ohrožení, čas detekce, všechny pokusy hrozeb a doporučené nápravy.</span><span class="sxs-lookup"><span data-stu-id="197fe-198">For example, you can see a description of the threat, the detection time, all threat attempts, and the recommended remediation.</span></span> <span data-ttu-id="197fe-199">V následujícím příkladu byla zjištěna útoku hrubou silou RDP s 294 neúspěšných pokusů protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="197fe-199">In the following example, an RDP brute-force attack was detected, with 294 failed RDP attempts.</span></span> <span data-ttu-id="197fe-200">Doporučené řešení je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="197fe-200">A recommended resolution is provided.</span></span>

![Útok protokolu RDP](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a><span data-ttu-id="197fe-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="197fe-202">Next steps</span></span>
<span data-ttu-id="197fe-203">V tomto kurzu nastavení Azure Security Center a poté zkontrolovat virtuální počítače ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="197fe-203">In this tutorial, you set up Azure Security Center, and then reviewed VMs in Security Center.</span></span> <span data-ttu-id="197fe-204">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="197fe-204">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="197fe-205">Nastavení shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="197fe-205">Set up data collection</span></span>
> * <span data-ttu-id="197fe-206">Nastavte zásady zabezpečení</span><span class="sxs-lookup"><span data-stu-id="197fe-206">Set up security policies</span></span>
> * <span data-ttu-id="197fe-207">Zobrazení a opravte problémy s konfigurací stavu</span><span class="sxs-lookup"><span data-stu-id="197fe-207">View and fix configuration health issues</span></span>
> * <span data-ttu-id="197fe-208">Zkontrolujte zjištěnými hrozbami</span><span class="sxs-lookup"><span data-stu-id="197fe-208">Review detected threats</span></span>

<span data-ttu-id="197fe-209">Přejít na další informace o vytváření položek konfigurace nebo CD kanál s volaných, Githubu a Docker v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="197fe-209">Advance to the next tutorial to learn more about creating a CI/CD pipeline with Jenkins, GitHub, and Docker.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="197fe-210">Vytvoření položek konfigurace nebo CD infrastruktury s volaných Githubu a Docker</span><span class="sxs-lookup"><span data-stu-id="197fe-210">Create CI/CD infrastructure with Jenkins, GitHub, and Docker</span></span>](tutorial-jenkins-github-docker-cicd.md)

