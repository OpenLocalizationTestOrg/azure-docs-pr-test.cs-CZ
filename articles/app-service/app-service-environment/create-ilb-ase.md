---
title: "Vytváření a používání vyrovnávání zatížení interní pomocí služby Azure App Service environment"
description: "Podrobnosti o tom, jak vytvořit a používat Azure App Service environment izolované Internetu"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: e7f85aaf2d940f114248d5925a1e97fe0f6bda6c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a><span data-ttu-id="6e27e-103">Vytvořit a použít vyrovnávání zatížení interní s služby App Service environment</span><span class="sxs-lookup"><span data-stu-id="6e27e-103">Create and use an internal load balancer with an App Service environment</span></span> #

 <span data-ttu-id="6e27e-104">Azure App Service Environment je nasazení služby Azure App Service do používanou podsíť virtuální sítě Azure (VNet).</span><span class="sxs-lookup"><span data-stu-id="6e27e-104">Azure App Service Environment is a deployment of Azure App Service into a subnet in an Azure virtual network (VNet).</span></span> <span data-ttu-id="6e27e-105">Existují dva způsoby, jak nasadit služby App Service environment (App Service Environment):</span><span class="sxs-lookup"><span data-stu-id="6e27e-105">There are two ways to deploy an App Service environment (ASE):</span></span> 

- <span data-ttu-id="6e27e-106">V VIP na externí IP adresu často označuje jako externí App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-106">With a VIP on an external IP address, often called an External ASE.</span></span>
- <span data-ttu-id="6e27e-107">S virtuální IP adresu na interní IP adresu často říká App Service Environment ILB protože vnitřní koncový bod je vyrovnávání interní zatížení (ILB).</span><span class="sxs-lookup"><span data-stu-id="6e27e-107">With a VIP on an internal IP address, often called an ILB ASE because the internal endpoint is an internal load balancer (ILB).</span></span> 

<span data-ttu-id="6e27e-108">Tento článek ukazuje, jak vytvořit App Service Environment ILB.</span><span class="sxs-lookup"><span data-stu-id="6e27e-108">This article shows you how to create an ILB ASE.</span></span> <span data-ttu-id="6e27e-109">Přehled App Service Environment, najdete v části [Úvod do služby App Service Environment][Intro].</span><span class="sxs-lookup"><span data-stu-id="6e27e-109">For an overview on the ASE, see [Introduction to App Service environments][Intro].</span></span> <span data-ttu-id="6e27e-110">Naučte se vytvářet externí App Service Environment, najdete v tématu [vytvořit externí App Service Environment][MakeExternalASE].</span><span class="sxs-lookup"><span data-stu-id="6e27e-110">To learn how to create an External ASE, see [Create an External ASE][MakeExternalASE].</span></span>

## <a name="overview"></a><span data-ttu-id="6e27e-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="6e27e-111">Overview</span></span> ##

<span data-ttu-id="6e27e-112">App Service Environment se koncový bod přístupné z Internetu nebo IP adresu můžete nasadit ve vaší virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="6e27e-112">You can deploy an ASE with an internet-accessible endpoint or with an IP address in your VNet.</span></span> <span data-ttu-id="6e27e-113">Chcete-li nastavit adresu IP adresu virtuální sítě, musí být nasazený App Service Environment s ILB.</span><span class="sxs-lookup"><span data-stu-id="6e27e-113">To set the IP address to a VNet address, the ASE must be deployed with an ILB.</span></span> <span data-ttu-id="6e27e-114">Při nasazení vaší App Service Environment s ILB, je nutné zadat:</span><span class="sxs-lookup"><span data-stu-id="6e27e-114">When you deploy your ASE with an ILB, you must provide:</span></span>

-   <span data-ttu-id="6e27e-115">Vlastní domény, který použijete při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e27e-115">Your own domain that you use when you create your apps.</span></span>
-   <span data-ttu-id="6e27e-116">Certifikát použitý pro protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6e27e-116">The certificate used for HTTPS.</span></span>
-   <span data-ttu-id="6e27e-117">Správa služby DNS pro vaši doménu.</span><span class="sxs-lookup"><span data-stu-id="6e27e-117">DNS management for your domain.</span></span>

<span data-ttu-id="6e27e-118">Naopak může provádět akce, jako:</span><span class="sxs-lookup"><span data-stu-id="6e27e-118">In return, you can do things such as:</span></span>

-   <span data-ttu-id="6e27e-119">Hostování aplikací intranetu bezpečně v cloudu, což přistupujete přes síť site-to-site nebo Azure ExpressRoute VPN.</span><span class="sxs-lookup"><span data-stu-id="6e27e-119">Host intranet applications securely in the cloud, which you access through a site-to-site or Azure ExpressRoute VPN.</span></span>
-   <span data-ttu-id="6e27e-120">Aplikace hostitele, v cloudu, které nejsou uvedené v veřejné servery DNS.</span><span class="sxs-lookup"><span data-stu-id="6e27e-120">Host apps in the cloud that aren't listed in public DNS servers.</span></span>
-   <span data-ttu-id="6e27e-121">Vytvořte samostatný internet back-end aplikace, které aplikace front-endu může bezpečně integrovat.</span><span class="sxs-lookup"><span data-stu-id="6e27e-121">Create internet-isolated back-end apps, which your front-end apps can securely integrate with.</span></span>

### <a name="disabled-functionality"></a><span data-ttu-id="6e27e-122">Zakázané funkce</span><span class="sxs-lookup"><span data-stu-id="6e27e-122">Disabled functionality</span></span> ###

<span data-ttu-id="6e27e-123">Existují některé věci, které nejde dělat, když používáte ILB App Service Environment:</span><span class="sxs-lookup"><span data-stu-id="6e27e-123">There are some things that you can't do when you use an ILB ASE:</span></span>

-   <span data-ttu-id="6e27e-124">Použijte SSL založenou na protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="6e27e-124">Use IP-based SSL.</span></span>
-   <span data-ttu-id="6e27e-125">Přiřaďte IP adresy na konkrétní aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e27e-125">Assign IP addresses to specific apps.</span></span>
-   <span data-ttu-id="6e27e-126">Zakoupení a použití certifikátu s aplikací prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6e27e-126">Buy and use a certificate with an app through the Azure portal.</span></span> <span data-ttu-id="6e27e-127">Můžete získat certifikáty přímo od certifikační autority a jejich používání s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="6e27e-127">You can obtain certificates directly from a certificate authority and use them with your apps.</span></span> <span data-ttu-id="6e27e-128">Nelze získat je prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6e27e-128">You can't obtain them through the Azure portal.</span></span>

## <a name="create-an-ilb-ase"></a><span data-ttu-id="6e27e-129">Vytvořit App Service Environment ILB</span><span class="sxs-lookup"><span data-stu-id="6e27e-129">Create an ILB ASE</span></span> ##

<span data-ttu-id="6e27e-130">Chcete-li vytvořit ILB App Service Environment:</span><span class="sxs-lookup"><span data-stu-id="6e27e-130">To create an ILB ASE:</span></span>

1. <span data-ttu-id="6e27e-131">Na portálu Azure vyberte **nový** > **Web + mobilní** > **App Service Environment**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-131">In the Azure portal, select **New** > **Web + Mobile** > **App Service Environment**.</span></span>

2. <span data-ttu-id="6e27e-132">Vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="6e27e-132">Select your subscription.</span></span>

3. <span data-ttu-id="6e27e-133">Vyberte nebo vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e27e-133">Select or create a resource group.</span></span>

4. <span data-ttu-id="6e27e-134">Vyberte nebo vytvořte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="6e27e-134">Select or create a VNet.</span></span>

5. <span data-ttu-id="6e27e-135">Pokud vyberete stávající virtuální síť, budete muset vytvořit podsíť pro uložení App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-135">If you select an existing VNet, you need to create a subnet to hold the ASE.</span></span> <span data-ttu-id="6e27e-136">Nezapomeňte nastavit velikost podsítě dostatečně velký pro přizpůsobení případnému budoucímu růstu vašeho App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-136">Make sure to set a subnet size large enough to accommodate any future growth of your ASE.</span></span> <span data-ttu-id="6e27e-137">Doporučujeme velikost `/25`, která je 128 adresy a dokáže zpracovat App Service Environment maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="6e27e-137">We recommend a size of `/25`, which has 128 addresses and can handle a maximum-sized ASE.</span></span> <span data-ttu-id="6e27e-138">Je minimální velikost, můžete vybrat `/28`.</span><span class="sxs-lookup"><span data-stu-id="6e27e-138">The minimum size you can select is a `/28`.</span></span> <span data-ttu-id="6e27e-139">Po infrastruktury potřebuje, tato velikost je možné rozšířit, aby nesmí být delší než 11 instancí.</span><span class="sxs-lookup"><span data-stu-id="6e27e-139">After infrastructure needs, this size can be scaled to a maximum of 11 instances.</span></span>

    * <span data-ttu-id="6e27e-140">Překročila maximální výchozí počet instancí 100 v plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="6e27e-140">Go beyond the default maximum of 100 instances in your App Service plans.</span></span>

    * <span data-ttu-id="6e27e-141">Škálování téměř 100, ale s více Rychlé škálování front-endu.</span><span class="sxs-lookup"><span data-stu-id="6e27e-141">Scale near 100 but with more rapid front-end scaling.</span></span>

6. <span data-ttu-id="6e27e-142">Vyberte **virtuální sítě nebo umístění** > **konfigurace virtuální sítě**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-142">Select **Virtual Network/Location** > **Virtual Network Configuration**.</span></span> <span data-ttu-id="6e27e-143">Nastavte **VIP typ** k **interní**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-143">Set the **VIP Type** to **Internal**.</span></span>

7. <span data-ttu-id="6e27e-144">Zadejte název domény.</span><span class="sxs-lookup"><span data-stu-id="6e27e-144">Enter a domain name.</span></span> <span data-ttu-id="6e27e-145">Tato doména je používaný pro aplikace vytvořené v této App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-145">This domain is the one used for apps created in this ASE.</span></span> <span data-ttu-id="6e27e-146">Platí určitá omezení.</span><span class="sxs-lookup"><span data-stu-id="6e27e-146">There are some restrictions.</span></span> <span data-ttu-id="6e27e-147">Nemůže být:</span><span class="sxs-lookup"><span data-stu-id="6e27e-147">It can't be:</span></span>

    * <span data-ttu-id="6e27e-148">NET</span><span class="sxs-lookup"><span data-stu-id="6e27e-148">net</span></span>   

    * <span data-ttu-id="6e27e-149">azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="6e27e-149">azurewebsites.net</span></span>

    * <span data-ttu-id="6e27e-150">p.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="6e27e-150">p.azurewebsites.net</span></span>

    * <span data-ttu-id="6e27e-151">&lt;asename&gt;. p.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="6e27e-151">&lt;asename&gt;.p.azurewebsites.net</span></span>

   <span data-ttu-id="6e27e-152">Použít vlastní název domény pro aplikace a název domény používaný vaší App Service Environment se nesmí překrývat.</span><span class="sxs-lookup"><span data-stu-id="6e27e-152">The custom domain name used for apps and the domain name used by your ASE can't overlap.</span></span> <span data-ttu-id="6e27e-153">Pro App Service Environment ILB s názvem domény _contoso.com_, nemůžete použít vlastní názvy domén pro vaše aplikace, jako je:</span><span class="sxs-lookup"><span data-stu-id="6e27e-153">For an ILB ASE with the domain name _contoso.com_, you can't use custom domain names for your apps like:</span></span>

    * <span data-ttu-id="6e27e-154">www.contoso.com</span><span class="sxs-lookup"><span data-stu-id="6e27e-154">www.contoso.com</span></span>

    * <span data-ttu-id="6e27e-155">Abcd.def.contoso.com</span><span class="sxs-lookup"><span data-stu-id="6e27e-155">abcd.def.contoso.com</span></span>

    * <span data-ttu-id="6e27e-156">Abcd.contoso.com</span><span class="sxs-lookup"><span data-stu-id="6e27e-156">abcd.contoso.com</span></span>

   <span data-ttu-id="6e27e-157">Pokud znáte názvy vlastních domén pro vaše aplikace, zvolte doménu pro App Service Environment ILB, který nebude došlo ke konfliktu s těmito názvy vlastních domén.</span><span class="sxs-lookup"><span data-stu-id="6e27e-157">If you know the custom domain names for your apps, choose a domain for the ILB ASE that won’t have a conflict with those custom domain names.</span></span> <span data-ttu-id="6e27e-158">V tomto příkladu můžete použít něco jako *contoso-internal.com* pro doménu vaší App Service Environment vzhledem k tomu, který nebude v konfliktu s názvy vlastních domén, které končí *. contoso.com*.</span><span class="sxs-lookup"><span data-stu-id="6e27e-158">In this example, you can use something like *contoso-internal.com* for the domain of your ASE because that won't conflict with custom domain names that end in *.contoso.com*.</span></span>

8. <span data-ttu-id="6e27e-159">Vyberte **OK**a potom vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-159">Select **OK**, and then select **Create**.</span></span>

    ![Vytvoření služby ASE][1]

<span data-ttu-id="6e27e-161">Na **virtuální sítě** okna, je **konfigurace virtuální sítě** možnost.</span><span class="sxs-lookup"><span data-stu-id="6e27e-161">On the **Virtual Network** blade, there is a **Virtual Network Configuration** option.</span></span> <span data-ttu-id="6e27e-162">Můžete ho vybrat externí VIP nebo interní VIP.</span><span class="sxs-lookup"><span data-stu-id="6e27e-162">You can use it to select an External VIP or an Internal VIP.</span></span> <span data-ttu-id="6e27e-163">Výchozí hodnota je **externí**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-163">The default is **External**.</span></span> <span data-ttu-id="6e27e-164">Pokud vyberete **externí**, vaše App Service Environment používá přístupné z Internetu VIP.</span><span class="sxs-lookup"><span data-stu-id="6e27e-164">If you select **External**, your ASE uses an internet-accessible VIP.</span></span> <span data-ttu-id="6e27e-165">Pokud vyberete **interní**, vaše App Service Environment je nakonfigurován s ILB na IP adresu ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="6e27e-165">If you select **Internal**, your ASE is configured with an ILB on an IP address within your VNet.</span></span>

<span data-ttu-id="6e27e-166">Po výběru **interní**, možnost přidávat další IP adresy k vaší App Service Environment se odebere.</span><span class="sxs-lookup"><span data-stu-id="6e27e-166">After you select **Internal**, the ability to add more IP addresses to your ASE is removed.</span></span> <span data-ttu-id="6e27e-167">Místo toho je třeba zadat doménu App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-167">Instead, you need to provide the domain of the ASE.</span></span> <span data-ttu-id="6e27e-168">V App Service Environment externí VIP název App Service Environment se používá v doméně pro aplikace vytvořené v této App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-168">In an ASE with an External VIP, the name of the ASE is used in the domain for apps created in that ASE.</span></span>

<span data-ttu-id="6e27e-169">Pokud nastavíte **VIP typ** k **interní**, vaše jméno App Service Environment není v doméně používat pro App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-169">If you set **VIP Type** to **Internal**, your ASE name is not used in the domain for the ASE.</span></span> <span data-ttu-id="6e27e-170">Zadejte doménu explicitně.</span><span class="sxs-lookup"><span data-stu-id="6e27e-170">You specify the domain explicitly.</span></span> <span data-ttu-id="6e27e-171">Pokud vaše doména je *contoso.corp.net* a vytvoříte aplikaci v tom, že s názvem App Service Environment *timereporting*, adresa URL pro tuto aplikaci je timereporting.contoso.corp.net.</span><span class="sxs-lookup"><span data-stu-id="6e27e-171">If your domain is *contoso.corp.net* and you create an app in that ASE named *timereporting*, the URL for that app is timereporting.contoso.corp.net.</span></span>


## <a name="create-an-app-in-an-ilb-ase"></a><span data-ttu-id="6e27e-172">Vytvoření aplikace v App Service Environment ILB</span><span class="sxs-lookup"><span data-stu-id="6e27e-172">Create an app in an ILB ASE</span></span> ##

<span data-ttu-id="6e27e-173">Vytvoření aplikace v App Service Environment ILB stejným způsobem, obvykle vytvoříte aplikaci v App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-173">You create an app in an ILB ASE in the same way that you create an app in an ASE normally.</span></span>

1. <span data-ttu-id="6e27e-174">Na portálu Azure vyberte **nový** > **Web + mobilní** > **webové** nebo **Mobile** nebo **rozhraní API Aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-174">In the Azure portal, select **New** > **Web + Mobile** > **Web** or **Mobile** or **API App**.</span></span>

2. <span data-ttu-id="6e27e-175">Zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e27e-175">Enter the name of the app.</span></span>

3. <span data-ttu-id="6e27e-176">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="6e27e-176">Select the subscription.</span></span>

4. <span data-ttu-id="6e27e-177">Vyberte nebo vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e27e-177">Select or create a resource group.</span></span>

5. <span data-ttu-id="6e27e-178">Vyberte nebo vytvořte plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="6e27e-178">Select or create an App Service plan.</span></span> <span data-ttu-id="6e27e-179">Pokud chcete vytvořit nový plán aplikační služby, vyberte jako umístění vaší App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-179">If you want to create a new App Service plan, select your ASE as the location.</span></span> <span data-ttu-id="6e27e-180">Vyberte fond pracovních procesů, kam chcete plán služby App Service, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="6e27e-180">Select the worker pool where you want your App Service plan to be created.</span></span> <span data-ttu-id="6e27e-181">Když vytvoříte plán služby App Service, vyberte vaše App Service Environment jako umístění a fondu pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="6e27e-181">When you create the App Service plan, select your ASE as the location and the worker pool.</span></span> <span data-ttu-id="6e27e-182">Když zadáte název aplikace, domény v části název vaší aplikace je nahrazena domény pro váš App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-182">When you specify the name of the app, the domain under your app name is replaced by the domain for your ASE.</span></span>

6. <span data-ttu-id="6e27e-183">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-183">Select **Create**.</span></span> <span data-ttu-id="6e27e-184">Pokud chcete aplikaci se zobrazují na řídicím panelu, vyberte **připnout na řídicí panel** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="6e27e-184">If you want the app to appear on your dashboard, select the **Pin to dashboard** check box.</span></span>

    ![Vytvoření plánu služby App Service][2]

    <span data-ttu-id="6e27e-186">V části **název aplikace**, název domény se aktualizuje tak, aby odrážela doménu vaší App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-186">Under **App name**, the domain name is updated to reflect the domain of your ASE.</span></span>

## <a name="post-ilb-ase-creation-validation"></a><span data-ttu-id="6e27e-187">Ověření vytvoření POST-ILB App Service Environment</span><span class="sxs-lookup"><span data-stu-id="6e27e-187">Post-ILB ASE creation validation</span></span> ##

<span data-ttu-id="6e27e-188">App Service Environment ILB je trochu jiná než jiných - ILB App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-188">An ILB ASE is slightly different than the non-ILB ASE.</span></span> <span data-ttu-id="6e27e-189">Jak je již uvedeno potřebujete spravovat vlastní DNS.</span><span class="sxs-lookup"><span data-stu-id="6e27e-189">As already noted, you need to manage your own DNS.</span></span> <span data-ttu-id="6e27e-190">Je nutné také zadat svůj vlastní certifikát pro připojení prostřednictvím protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6e27e-190">You also have to provide your own certificate for HTTPS connections.</span></span>

<span data-ttu-id="6e27e-191">Po vytvoření vaší App Service Environment, název domény zobrazuje doménu, kterou jste zadali.</span><span class="sxs-lookup"><span data-stu-id="6e27e-191">After you create your ASE, the domain name shows the domain you specified.</span></span> <span data-ttu-id="6e27e-192">Nová položka se zobrazí v **nastavení** nabídky s názvem **ILB certifikát**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-192">A new item appears in the **Setting** menu called **ILB Certificate**.</span></span> <span data-ttu-id="6e27e-193">App Service Environment se vytvoří certifikát, který není zadejte doménu ILB App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-193">The ASE is created with a certificate that doesn't specify the ILB ASE domain.</span></span> <span data-ttu-id="6e27e-194">Pokud používáte App Service Environment pomocí certifikátu, prohlížeč informuje, že je neplatný.</span><span class="sxs-lookup"><span data-stu-id="6e27e-194">If you use the ASE with that certificate, your browser tells you that it's invalid.</span></span> <span data-ttu-id="6e27e-195">Tento certifikát usnadňuje testování HTTPS, ale budete muset nahrát vlastní certifikát, který je vázaný na doménu ILB App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-195">This certificate makes it easier to test HTTPS, but you need to upload your own certificate that's tied to your ILB ASE domain.</span></span> <span data-ttu-id="6e27e-196">Tento krok je nutný bez ohledu na to, jestli je vašeho certifikátu podepsaného svým držitelem nebo získali od certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="6e27e-196">This step is necessary regardless of whether your certificate is self-signed or acquired from a certificate authority.</span></span>

![Název domény ILB App Service Environment][3]

<span data-ttu-id="6e27e-198">Vaše ILB App Service Environment musí platný certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="6e27e-198">Your ILB ASE needs a valid SSL certificate.</span></span> <span data-ttu-id="6e27e-199">Použít interní certifikační úřady, koupit certifikát od externí vystavitele nebo použít certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="6e27e-199">Use internal certificate authorities, purchase a certificate from an external issuer, or use a self-signed certificate.</span></span> <span data-ttu-id="6e27e-200">Bez ohledu na zdroji certifikátu protokolu SSL musí být správně nakonfigurované následující atributy certifikátu:</span><span class="sxs-lookup"><span data-stu-id="6e27e-200">Regardless of the source of the SSL certificate, the following certificate attributes need to be configured properly:</span></span>

* <span data-ttu-id="6e27e-201">**Předmět**: Tento atribut musí být nastaven na *.your kořenové domény sem.</span><span class="sxs-lookup"><span data-stu-id="6e27e-201">**Subject**: This attribute must be set to *.your-root-domain-here.</span></span>
* <span data-ttu-id="6e27e-202">**Alternativní název subjektu**: Tento atribut musí obsahovat i **.your kořenové domény sem* a **.scm.your-kořenové-domény-zde*.</span><span class="sxs-lookup"><span data-stu-id="6e27e-202">**Subject Alternative Name**: This attribute must include both **.your-root-domain-here* and **.scm.your-root-domain-here*.</span></span> <span data-ttu-id="6e27e-203">Připojení SSL k webu SCM/Kudu spojené s každou aplikaci použít adresu ve tvaru *your-app-name.scm.your-root-domain-here*.</span><span class="sxs-lookup"><span data-stu-id="6e27e-203">SSL connections to the SCM/Kudu site associated with each app use an address of the form *your-app-name.scm.your-root-domain-here*.</span></span>

<span data-ttu-id="6e27e-204">Převést nebo uložení certifikátu SSL jako soubor .pfx.</span><span class="sxs-lookup"><span data-stu-id="6e27e-204">Convert/save the SSL certificate as a .pfx file.</span></span> <span data-ttu-id="6e27e-205">Soubor .pfx, musíte zahrnout všechny zprostředkující a kořenové certifikáty.</span><span class="sxs-lookup"><span data-stu-id="6e27e-205">The .pfx file must include all intermediate and root certificates.</span></span> <span data-ttu-id="6e27e-206">Zabezpečte ji pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="6e27e-206">Secure it with a password.</span></span>

<span data-ttu-id="6e27e-207">Pokud chcete vytvořit certifikát podepsaný svým držitelem, můžete tady použít příkazy prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e27e-207">If you want to create a self-signed certificate, you can use the PowerShell commands here.</span></span> <span data-ttu-id="6e27e-208">Nezapomeňte použít název domény App Service Environment ILB místo *internal.contoso.com*:</span><span class="sxs-lookup"><span data-stu-id="6e27e-208">Be sure to use your ILB ASE domain name instead of *internal.contoso.com*:</span></span> 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

<span data-ttu-id="6e27e-209">Certifikát, který tyto příkazy prostředí PowerShell generovat označen příznakem pomocí prohlížeče, protože certifikát nebyl vytvořen z certifikační autority, který je v prohlížeči řetěz certifikátů.</span><span class="sxs-lookup"><span data-stu-id="6e27e-209">The certificate that these PowerShell commands generate is flagged by browsers because the certificate wasn't created by a certificate authority that's in your browser's chain of trust.</span></span> <span data-ttu-id="6e27e-210">Chcete-li získat certifikát, který váš prohlížeč vztahy důvěryhodnosti, zakoupit od komerční certifikační autority v prohlížeči řetěz certifikátů.</span><span class="sxs-lookup"><span data-stu-id="6e27e-210">To get a certificate that your browser trusts, procure it from a commercial certificate authority in your browser's chain of trust.</span></span> 

![Nastavte ILB certifikát][4]

<span data-ttu-id="6e27e-212">Nahrát vlastní certifikáty a otestovat přístup:</span><span class="sxs-lookup"><span data-stu-id="6e27e-212">To upload your own certificates and test access:</span></span>

1. <span data-ttu-id="6e27e-213">Po vytvoření App Service Environment, přejděte na rozhraní App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-213">After the ASE is created, go to the ASE UI.</span></span> <span data-ttu-id="6e27e-214">Vyberte **App Service Environment** > **nastavení** > **ILB certifikát**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-214">Select **ASE** > **Settings** > **ILB Certificate**.</span></span>

2. <span data-ttu-id="6e27e-215">Pokud chcete nastavit certifikát ILB, vyberte soubor .pfx certifikátu a zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="6e27e-215">To set the ILB certificate, select the certificate .pfx file and enter the password.</span></span> <span data-ttu-id="6e27e-216">Tento krok trvá určitou dobu ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="6e27e-216">This step takes some time to process.</span></span> <span data-ttu-id="6e27e-217">Zobrazí se zpráva, že operace nahrávání probíhá.</span><span class="sxs-lookup"><span data-stu-id="6e27e-217">A message appears stating that an upload operation is in progress.</span></span>

3. <span data-ttu-id="6e27e-218">Získáte adresu ILB pro vaše App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-218">Get the ILB address for your ASE.</span></span> <span data-ttu-id="6e27e-219">Vyberte **App Service Environment** > **vlastnosti** > **virtuální IP adresa**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-219">Select **ASE** > **Properties** > **Virtual IP Address**.</span></span>

4. <span data-ttu-id="6e27e-220">Vytvoření webové aplikace ve vaší App Service Environment, po vytvoření App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-220">Create a web app in your ASE after the ASE is created.</span></span>

5. <span data-ttu-id="6e27e-221">Vytvoření virtuálního počítače, pokud nemáte v této virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6e27e-221">Create a VM if you don't have one in that VNet.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6e27e-222">Nemáte pokuste se vytvořit tento virtuální počítač ve stejné podsíti jako App Service Environment, protože se nezdaří nebo způsobit problémy.</span><span class="sxs-lookup"><span data-stu-id="6e27e-222">Don't try to create this VM in the same subnet as the ASE because it will fail or cause problems.</span></span>
    >
    >

6. <span data-ttu-id="6e27e-223">Nastavení DNS pro doménu služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-223">Set the DNS for your ASE domain.</span></span> <span data-ttu-id="6e27e-224">Můžete použít zástupný znak s doménou v DNS.</span><span class="sxs-lookup"><span data-stu-id="6e27e-224">You can use a wildcard with your domain in your DNS.</span></span> <span data-ttu-id="6e27e-225">Chcete-li provést několik jednoduchých testů, upravte souboru hostitelů na vašem virtuálním počítači nastavit název webové aplikace na virtuální IP adresy IP adresu:</span><span class="sxs-lookup"><span data-stu-id="6e27e-225">To do some simple tests, edit the hosts file on your VM to set the web app name to the VIP IP address:</span></span>

    <span data-ttu-id="6e27e-226">a.</span><span class="sxs-lookup"><span data-stu-id="6e27e-226">a.</span></span> <span data-ttu-id="6e27e-227">Pokud název domény vaší App Service Environment _. ilbase.com_ a vytvoříte webovou aplikaci s názvem _mytestapp_, je určena v _mytestapp.ilbase.com_. Pak můžete nastavit _mytestapp.ilbase.com_ přeložit na adresu ILB.</span><span class="sxs-lookup"><span data-stu-id="6e27e-227">If your ASE has the domain name _.ilbase.com_ and you create the web app named _mytestapp_, it's addressed at _mytestapp.ilbase.com_. You then set _mytestapp.ilbase.com_ to resolve to the ILB address.</span></span> <span data-ttu-id="6e27e-228">(V systému Windows, soubor hostitele je v _C:\Windows\System32\drivers\etc\_.)</span><span class="sxs-lookup"><span data-stu-id="6e27e-228">(On Windows, the hosts file is at _C:\Windows\System32\drivers\etc\_.)</span></span>

    <span data-ttu-id="6e27e-229">b.</span><span class="sxs-lookup"><span data-stu-id="6e27e-229">b.</span></span> <span data-ttu-id="6e27e-230">Testování nasazení publikování na webu nebo přístup ke konzole rozšířené, vytvořte záznam pro _mytestapp.scm.ilbase.com_.</span><span class="sxs-lookup"><span data-stu-id="6e27e-230">To test web deployment publishing or access to the advanced console, create a record for _mytestapp.scm.ilbase.com_.</span></span>

7. <span data-ttu-id="6e27e-231">Použít prohlížeč na tento virtuální počítač a přejděte k http://mytestapp.ilbase.com. (Nebo přejděte na ať je název vaší webové aplikace s vaší domény.)</span><span class="sxs-lookup"><span data-stu-id="6e27e-231">Use a browser on that VM and go to http://mytestapp.ilbase.com. (Or go to whatever your web app name is with your domain.)</span></span>

8. <span data-ttu-id="6e27e-232">Použít prohlížeč na tento virtuální počítač a přejděte k https://mytestapp.ilbase.com. Pokud používáte certifikát podepsaný svým držitelem, přijměte nedostatek zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6e27e-232">Use a browser on that VM and go to https://mytestapp.ilbase.com. If you use a self-signed certificate, accept the lack of security.</span></span>

    <span data-ttu-id="6e27e-233">IP adresu pro vaše ILB je uveden v části **IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="6e27e-233">The IP address for your ILB is listed under **IP addresses**.</span></span> <span data-ttu-id="6e27e-234">Tento seznam má také IP adres používaných externí virtuální IP adresy a pro správu příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="6e27e-234">This list also has the IP addresses used by the external VIP and for inbound management traffic.</span></span>

    ![ILB IP adresa][5]

### <a name="web-jobs-functions-and-the-ilb-ase"></a><span data-ttu-id="6e27e-236">Webové úlohy, funkce a App Service Environment ILB</span><span class="sxs-lookup"><span data-stu-id="6e27e-236">Web jobs, Functions and the ILB ASE</span></span>

<span data-ttu-id="6e27e-237">Funkce a webové úlohy jsou podporovány v App Service Environment ILB, ale pro tento portál pro práci s nimi musí mít přístup k síti na web SCM.</span><span class="sxs-lookup"><span data-stu-id="6e27e-237">Both Functions and web jobs are supported on an ILB ASE but for the portal to work with them, you must have network access to the SCM site.</span></span>  <span data-ttu-id="6e27e-238">To znamená, že váš prohlížeč musí být buď na hostitele, který je buď v nebo připojení k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="6e27e-238">This means your browser must either be on a host that is either in or connected to the virtual network.</span></span>  

<span data-ttu-id="6e27e-239">Pokud používáte Azure Functions v App Service Environment ILB, může získat chybovou zprávu, která říká "Nedokážeme teď načíst vaše funkce.</span><span class="sxs-lookup"><span data-stu-id="6e27e-239">When you use Azure Functions on an ILB ASE, you might get an error message that says "We are not able to retrieve your functions right now.</span></span> <span data-ttu-id="6e27e-240">Zkuste to prosím znovu později."</span><span class="sxs-lookup"><span data-stu-id="6e27e-240">Please try again later."</span></span> <span data-ttu-id="6e27e-241">K této chybě dojde, protože uživatelské rozhraní funkce využívá webu SCM přes protokol HTTPS a kořenový certifikát není v prohlížeči řetěz certifikátů.</span><span class="sxs-lookup"><span data-stu-id="6e27e-241">This error occurs because the Functions UI leverages the SCM site over HTTPS and the root certificate is not in the browser chain of trust.</span></span> <span data-ttu-id="6e27e-242">Webové úlohy je podobný problém.</span><span class="sxs-lookup"><span data-stu-id="6e27e-242">Web jobs has a similar problem.</span></span> <span data-ttu-id="6e27e-243">Tento problém můžete provést jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="6e27e-243">To avoid this problem you can do one of the following:</span></span>

- <span data-ttu-id="6e27e-244">Přidání certifikátu do úložiště důvěryhodných certifikátů.</span><span class="sxs-lookup"><span data-stu-id="6e27e-244">Add the certificate to your trusted certificate store.</span></span> <span data-ttu-id="6e27e-245">To odblokuje okraj a prohlížeče Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="6e27e-245">This unblocks Edge and Internet Explorer.</span></span>
- <span data-ttu-id="6e27e-246">Použijte Chrome a přejděte na web SCM nejprve, přijměte nedůvěryhodný certifikát a potom přejděte na portál.</span><span class="sxs-lookup"><span data-stu-id="6e27e-246">Use Chrome and go to the SCM site first, accept the untrusted certificate and then go to the portal.</span></span>
- <span data-ttu-id="6e27e-247">Použijte komerční certifikát, který je v řetězu certifikátů váš prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="6e27e-247">Use a commercial certificate that is in your browser chain of trust.</span></span>  <span data-ttu-id="6e27e-248">Toto je nejlepší možnost.</span><span class="sxs-lookup"><span data-stu-id="6e27e-248">This is the best option.</span></span>  

## <a name="dns-configuration"></a><span data-ttu-id="6e27e-249">Konfigurace služby DNS</span><span class="sxs-lookup"><span data-stu-id="6e27e-249">DNS configuration</span></span> ##

<span data-ttu-id="6e27e-250">Pokud používáte externí VIP, DNS spravuje Azure.</span><span class="sxs-lookup"><span data-stu-id="6e27e-250">When you use an External VIP, the DNS is managed by Azure.</span></span> <span data-ttu-id="6e27e-251">Všechny aplikace vytvořená v vaší App Service Environment se automaticky přidá do Azure DNS, který je veřejném DNS.</span><span class="sxs-lookup"><span data-stu-id="6e27e-251">Any app created in your ASE is automatically added to Azure DNS, which is a public DNS.</span></span> <span data-ttu-id="6e27e-252">ILB App Service Environment musí spravovat vlastní DNS.</span><span class="sxs-lookup"><span data-stu-id="6e27e-252">In an ILB ASE, you must manage your own DNS.</span></span> <span data-ttu-id="6e27e-253">Pro danou doménu, jako _contoso.net_, budete muset vytvořit záznamy DNS A v serveru DNS, které odkazují na vaši adresu ILB pro:</span><span class="sxs-lookup"><span data-stu-id="6e27e-253">For a given domain such as _contoso.net_, you need to create DNS A records in your DNS that point to your ILB address for:</span></span>

- <span data-ttu-id="6e27e-254">*. contoso.net</span><span class="sxs-lookup"><span data-stu-id="6e27e-254">*.contoso.net</span></span>
- <span data-ttu-id="6e27e-255">*. scm.contoso.net</span><span class="sxs-lookup"><span data-stu-id="6e27e-255">*.scm.contoso.net</span></span>

<span data-ttu-id="6e27e-256">Pokud doménu ILB App Service Environment se používá pro několik věcí mimo tento App Service Environment, může být třeba spravovat DNS na základě názvu na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6e27e-256">If your ILB ASE domain is used for multiple things outside this ASE, you might need to manage DNS on a per-app-name basis.</span></span> <span data-ttu-id="6e27e-257">Tato metoda je náročné, protože je nutné přidat každý nový název aplikace do DNS při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="6e27e-257">This method is challenging because you need to add each new app name into your DNS when you create it.</span></span> <span data-ttu-id="6e27e-258">Z tohoto důvodu doporučujeme používat vyhrazené domény.</span><span class="sxs-lookup"><span data-stu-id="6e27e-258">For this reason, we recommend that you use a dedicated domain.</span></span>

## <a name="publish-with-an-ilb-ase"></a><span data-ttu-id="6e27e-259">Publikování s ILB App Service Environment</span><span class="sxs-lookup"><span data-stu-id="6e27e-259">Publish with an ILB ASE</span></span> ##

<span data-ttu-id="6e27e-260">Pro každou aplikaci, která je vytvořena jsou dva koncové body.</span><span class="sxs-lookup"><span data-stu-id="6e27e-260">For every app that's created, there are two endpoints.</span></span> <span data-ttu-id="6e27e-261">V App Service Environment ILB, máte  *&lt;název aplikace >.&lt; App Service Environment domény ILB >* a  *&lt;název aplikace > .scm.&lt; App Service Environment domény ILB >*.</span><span class="sxs-lookup"><span data-stu-id="6e27e-261">In an ILB ASE, you have *&lt;app name>.&lt;ILB ASE Domain>* and *&lt;app name>.scm.&lt;ILB ASE Domain>*.</span></span> 

<span data-ttu-id="6e27e-262">Název lokality SCM přejdete ke konzole Kudu názvem **pokročilé portál**, v rámci portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6e27e-262">The SCM site name takes you to the Kudu console, called the **Advanced portal**, within the Azure portal.</span></span> <span data-ttu-id="6e27e-263">Konzola Kudu umožňuje zobrazení proměnné prostředí, prozkoumejte disku, použijte konzolu a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="6e27e-263">The Kudu console lets you view environment variables, explore the disk, use a console, and much more.</span></span> <span data-ttu-id="6e27e-264">Další informace najdete v tématu [konzoly Kudu pro službu Azure App Service][Kudu].</span><span class="sxs-lookup"><span data-stu-id="6e27e-264">For more information, see [Kudu console for Azure App Service][Kudu].</span></span> 

<span data-ttu-id="6e27e-265">V víceklientské služby App Service a externí App Service Environment není jednotné přihlašování mezi portálu Azure a Kudu konzoly.</span><span class="sxs-lookup"><span data-stu-id="6e27e-265">In the multitenant App Service and in an External ASE, there's single sign-on between the Azure portal and the Kudu console.</span></span> <span data-ttu-id="6e27e-266">Pro ILB App Service Environment budete muset však použít pro přihlášení ke konzole Kudu vaše přihlašovací údaje pro publikování.</span><span class="sxs-lookup"><span data-stu-id="6e27e-266">For the ILB ASE, however, you need to use your publishing credentials to sign into the Kudu console.</span></span>

<span data-ttu-id="6e27e-267">Internetové systémy položek konfigurace, například Githubu a Visual Studio Team Services, nefungují s ILB App Service Environment, protože publikování koncový bod není dostupný na Internetu.</span><span class="sxs-lookup"><span data-stu-id="6e27e-267">Internet-based CI systems, such as GitHub and Visual Studio Team Services, don't work with an ILB ASE because the publishing endpoint isn't internet accessible.</span></span> <span data-ttu-id="6e27e-268">Místo toho musíte použít CI systému, který používá model pull, jako je Dropbox.</span><span class="sxs-lookup"><span data-stu-id="6e27e-268">Instead, you need to use a CI system that uses a pull model, such as Dropbox.</span></span>

<span data-ttu-id="6e27e-269">Publikování koncové body pro aplikace v App Service Environment ILB použijte domény, který byl vytvořený App Service Environment ILB.</span><span class="sxs-lookup"><span data-stu-id="6e27e-269">The publishing endpoints for apps in an ILB ASE use the domain that the ILB ASE was created with.</span></span> <span data-ttu-id="6e27e-270">Tuto doménu se zobrazí v profilu publikování aplikace a v okně portálu aplikace (**přehled** > **Essentials** a také **vlastnosti**).</span><span class="sxs-lookup"><span data-stu-id="6e27e-270">This domain appears in the app's publishing profile and in the app's portal blade (**Overview** > **Essentials** and also **Properties**).</span></span> <span data-ttu-id="6e27e-271">Pokud máte App Service Environment ILB s subdoméně *contoso.net* a aplikaci s názvem *test*, použijte *mytest.contoso.net* pro FTP a *mytest.scm.contoso.net* pro nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="6e27e-271">If you have an ILB ASE with the subdomain *contoso.net* and an app named *mytest*, use *mytest.contoso.net* for FTP and *mytest.scm.contoso.net* for web deployment.</span></span>

## <a name="couple-an-ilb-ase-with-a-waf-device"></a><span data-ttu-id="6e27e-272">Spojte ILB App Service Environment se zařízením firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="6e27e-272">Couple an ILB ASE with a WAF device</span></span> ##

<span data-ttu-id="6e27e-273">Aplikační služba Azure poskytuje mnoho bezpečnostní opatření, které chrání systém.</span><span class="sxs-lookup"><span data-stu-id="6e27e-273">Azure App Service provides many security measures that protect the system.</span></span> <span data-ttu-id="6e27e-274">Mohou také pomoci určit, zda se hacker aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e27e-274">They also help to determine whether an app was hacked.</span></span> <span data-ttu-id="6e27e-275">Nejlepší ochrany pro webovou aplikaci je spojte hostující platforma, jako je například Azure App Service pomocí brány firewall webových aplikací (firewall webových aplikací).</span><span class="sxs-lookup"><span data-stu-id="6e27e-275">The best protection for a web application is to couple a hosting platform, such as Azure App Service, with a web application firewall (WAF).</span></span> <span data-ttu-id="6e27e-276">Protože App Service Environment ILB má koncový bod sítě izolované aplikace, je vhodné pro toto použití.</span><span class="sxs-lookup"><span data-stu-id="6e27e-276">Because the ILB ASE has a network-isolated application endpoint, it's appropriate for such a use.</span></span>

<span data-ttu-id="6e27e-277">Další informace o tom, jak nakonfigurovat App Service Environment vaší ILB zařízení firewall webových aplikací najdete v tématu [konfigurace brány firewall webových aplikací pomocí služby App Service environment][ASEWAF].</span><span class="sxs-lookup"><span data-stu-id="6e27e-277">To learn more about how to configure your ILB ASE with a WAF device, see [Configure a web application firewall with your App Service environment][ASEWAF].</span></span> <span data-ttu-id="6e27e-278">Tento článek ukazuje, jak používat virtuální zařízení Barracuda s vaší App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="6e27e-278">This article shows how to use a Barracuda virtual appliance with your ASE.</span></span> <span data-ttu-id="6e27e-279">Další možností je použití Azure Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="6e27e-279">Another option is to use Azure Application Gateway.</span></span> <span data-ttu-id="6e27e-280">Aplikační brána používá pravidla základní OWASP zabezpečit všechny aplikace se umístí za ho.</span><span class="sxs-lookup"><span data-stu-id="6e27e-280">Application Gateway uses the OWASP core rules to secure any applications placed behind it.</span></span> <span data-ttu-id="6e27e-281">Další informace o Application Gateway najdete v tématu [Úvod do brány firewall Azure webových aplikací][AppGW].</span><span class="sxs-lookup"><span data-stu-id="6e27e-281">For more information about Application Gateway, see [Introduction to the Azure web application firewall][AppGW].</span></span>

## <a name="get-started"></a><span data-ttu-id="6e27e-282">Začínáme</span><span class="sxs-lookup"><span data-stu-id="6e27e-282">Get started</span></span> ##

<span data-ttu-id="6e27e-283">Všechny články a postupy pro ASEs jsou k dispozici v [soubor README pro služby App Service Environment][ASEReadme].</span><span class="sxs-lookup"><span data-stu-id="6e27e-283">All articles and how-to instructions for ASEs are available in the [README for App Service environments][ASEReadme].</span></span>

* <span data-ttu-id="6e27e-284">Chcete-li začít pracovat s ASEs, přečtěte si téma [Úvod do služby App Service Environment][Intro].</span><span class="sxs-lookup"><span data-stu-id="6e27e-284">To get started with ASEs, see [Introduction to App Service environments][Intro].</span></span>
* <span data-ttu-id="6e27e-285">Další informace o platformě Azure App Service najdete v článku [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).</span><span class="sxs-lookup"><span data-stu-id="6e27e-285">For more information about the Azure App Service platform, see [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).</span></span>
 
<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
