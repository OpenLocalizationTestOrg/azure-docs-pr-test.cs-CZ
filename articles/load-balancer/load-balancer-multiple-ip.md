---
title: "Víc konfigurací IP adres v Azure pro vyrovnávání zatížení | Microsoft Docs"
description: "Vyrovnávání zatížení napříč primární a sekundární konfigurace protokolu IP."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: cf1e68c7b37b2506de007bdf24eea63a27187a33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-the-azure-portal"></a><span data-ttu-id="c90c2-103">Víc konfigurací IP adres pomocí portálu Azure pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="c90c2-103">Load balancing on multiple IP configurations using the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c90c2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c90c2-104">Portal</span></span>](load-balancer-multiple-ip.md)
> * [<span data-ttu-id="c90c2-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c90c2-105">PowerShell</span></span>](load-balancer-multiple-ip-powershell.md)
> * [<span data-ttu-id="c90c2-106">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="c90c2-106">CLI</span></span>](load-balancer-multiple-ip-cli.md)

<span data-ttu-id="c90c2-107">Tento článek popisuje, jak používat nástroj pro vyrovnávání zatížení Azure s více IP adres v sekundárním síťovém rozhraní (NIC). V tomto scénáři máme dva virtuální počítače se systémem Windows, každý s primární a sekundární síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="c90c2-107">This article describes how to use Azure Load Balancer with multiple IP addresses on a secondary network interface (NIC).For this scenario, we have two VMs running Windows, each with a primary and a secondary NIC.</span></span> <span data-ttu-id="c90c2-108">Každé sekundární síťové karty má dvě konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="c90c2-108">Each of the secondary NICs have two IP configurations.</span></span> <span data-ttu-id="c90c2-109">Každý virtuální počítač hostuje weby contoso.com a fabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="c90c2-109">Each VM hosts both websites contoso.com and fabrikam.com.</span></span> <span data-ttu-id="c90c2-110">Každý web je vázána na jednu z konfigurace protokolu IP v sekundárním síťovém adaptéru.</span><span class="sxs-lookup"><span data-stu-id="c90c2-110">Each website is bound to one of the IP configurations on the secondary NIC.</span></span> <span data-ttu-id="c90c2-111">Nástroj pro vyrovnávání zatížení Azure používáme ke zveřejnění dvě front-end IP adresy, jednu pro každý web, distribuovat provoz do příslušných konfiguraci protokolu IP pro web.</span><span class="sxs-lookup"><span data-stu-id="c90c2-111">We use Azure Load Balancer to expose two frontend IP addresses, one for each website, to distribute traffic to the respective IP configuration for the website.</span></span> <span data-ttu-id="c90c2-112">Tento scénář používá stejné číslo portu mezi frontends, jak oba back-end fondu IP adres.</span><span class="sxs-lookup"><span data-stu-id="c90c2-112">This scenario uses the same port number across both frontends, as well as both backend pool IP addresses.</span></span>

![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

##<a name="prerequisites"></a><span data-ttu-id="c90c2-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c90c2-114">Prerequisites</span></span>
<span data-ttu-id="c90c2-115">Tento příklad předpokládá, že máte skupinu prostředků s názvem *contosofabrikam* s následující konfigurací:</span><span class="sxs-lookup"><span data-stu-id="c90c2-115">This example assumes that you have a Resource Group named *contosofabrikam* with the following configuration:</span></span>
 -  <span data-ttu-id="c90c2-116">obsahuje virtuální síť s názvem *myVNet*, dva virtuální počítače, které volá *VM1* a *virtuálního počítače 2* v uvedeném pořadí v rámci stejné skupině s názvem dostupnosti *myAvailset*.</span><span class="sxs-lookup"><span data-stu-id="c90c2-116">includes a virtual network named *myVNet*, two VMs called *VM1* and *VM2* respectively within the same availability set named *myAvailset*.</span></span> 
 - <span data-ttu-id="c90c2-117">Každý virtuální počítač má síťový adaptér primární a sekundární síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="c90c2-117">each VM has a primary NIC and a secondary NIC.</span></span> <span data-ttu-id="c90c2-118">Primární síťové adaptéry jsou pojmenované *VM1NIC1* a *VM2NIC1* a sekundární síťové adaptéry jsou pojmenované *VM1NIC2* a *VM2NIC2*.</span><span class="sxs-lookup"><span data-stu-id="c90c2-118">The primary NICs are named *VM1NIC1* and *VM2NIC1* and the secondary NICs are named *VM1NIC2* and *VM2NIC2*.</span></span> <span data-ttu-id="c90c2-119">Další informace o vytváření virtuálních počítačů s více síťovými kartami najdete v tématu [vytvoření virtuálního počítače s více síťovými kartami pomocí prostředí PowerShell](../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="c90c2-119">For more information about creating VMs with multiple NICs, see [Create a VM with multiple NICs using PowerShell](../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span></span>

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a><span data-ttu-id="c90c2-120">Postup na víc konfigurací IP adres Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="c90c2-120">Steps to load balance on multiple IP configurations</span></span>

<span data-ttu-id="c90c2-121">Postupujte podle těchto pokynů dosáhnout scénáři uvedeném v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="c90c2-121">Follow these steps below to achieve the scenario outlined in this article:</span></span>

### <a name="step-1-configure-the-secondary-nics-for-each-vm"></a><span data-ttu-id="c90c2-122">Krok 1: Konfigurace sekundární síťové adaptéry pro každý virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="c90c2-122">STEP 1: Configure the secondary NICs for each VM</span></span>

<span data-ttu-id="c90c2-123">Pro každý virtuální počítač ve virtuální síti přidejte sadu konfiguraci protokolu IP pro sekundární síťový adaptér následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c90c2-123">For each VM in your virtual network, add set IP configuration for the secondary NIC as follows:</span></span>  

1. <span data-ttu-id="c90c2-124">V prohlížeči přejděte na portálu Azure: http://portal.azure.com a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="c90c2-124">From a browser navigate to the Azure portal: http://portal.azure.com and login with your Azure account.</span></span>
2. <span data-ttu-id="c90c2-125">Na levé straně horní části obrazovky, klikněte na ikonu skupiny prostředků a potom klikněte na skupinu prostředků virtuálních počítačů, které se nacházejí v (například *contosofabrikam*).</span><span class="sxs-lookup"><span data-stu-id="c90c2-125">On the top left-hand side of the screen, click the Resource Group icon, and then click the resource group the VMs are located in (for example, *contosofabrikam*).</span></span> <span data-ttu-id="c90c2-126">**Skupiny prostředků** se nyní zobrazí okno, které jsou uvedeny všechny prostředky, které společně s rozhraní sítě pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="c90c2-126">The **Resource groups** blade that lists all the resources along with the network interfaces for the VMs is now displayed.</span></span>
3. <span data-ttu-id="c90c2-127">Na sekundární síťový adaptér každého virtuálního počítače přidejte konfiguraci IP adres následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c90c2-127">To the secondary NIC of each VM, add an IP configuration as follows:</span></span>
    1. <span data-ttu-id="c90c2-128">Vyberte síťové rozhraní, které chcete přidat konfigurace IP adresy na.</span><span class="sxs-lookup"><span data-stu-id="c90c2-128">Select the network interface you want to add the IP configuration to.</span></span>
    2. <span data-ttu-id="c90c2-129">V okně, který se zobrazí pro síťový adaptér, který jste vybrali, klikněte na tlačítko **konfigurace protokolu IP**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-129">In the blade that appears for the NIC that you selected, click **IP configurations**.</span></span> <span data-ttu-id="c90c2-130">Pak klikněte na tlačítko **přidat** směrem nahoru v okně, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c90c2-130">Then click **Add** towards the top of the blade that shows up.</span></span>
    3. <span data-ttu-id="c90c2-131">V **přidat IP konfigurace** okně přidejte druhý konfigurace protokolu IP na síťový adaptér následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c90c2-131">In the **Add IP configurations** blade, add a second IP configuration to the NIC as follows:</span></span> 
        1. <span data-ttu-id="c90c2-132">Zadejte název vaší sekundární konfiguraci IP adresy (například pro VM1 a virtuálního počítače 2 název konfigurace protokolu IP jako *VM1NIC2 ipconfig2* a *VM2NIC2 ipconfig2* v uvedeném pořadí).</span><span class="sxs-lookup"><span data-stu-id="c90c2-132">Type a name for your secondary IP configuration (for example, for VM1 and VM2 name the IP configurations as *VM1NIC2-ipconfig2* and *VM2NIC2-ipconfig2* respectively).</span></span>
        2. <span data-ttu-id="c90c2-133">Pro **privátní IP adresa**, pro **přidělení**, vyberte **statické**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-133">For **Private IP address**, for **Allocation**, select **Static**.</span></span>
        3. <span data-ttu-id="c90c2-134">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-134">Click **OK**.</span></span>
        4. <span data-ttu-id="c90c2-135">Po dokončení druhé konfiguraci protokolu IP pro sekundární síťový adaptér se zobrazí v **konfigurace protokolu IP** okno nastavení pro daný síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="c90c2-135">When the second IP configuration for the secondary NIC is complete, it is displayed in the **IP configurations** settings blade for the given NIC.</span></span>

### <a name="step-2-create-a-load-balancer"></a><span data-ttu-id="c90c2-136">Krok 2: Vytvoření služby Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="c90c2-136">STEP 2: Create a load balancer</span></span>

<span data-ttu-id="c90c2-137">Nástroj pro vyrovnávání zatížení vytvořte takto:</span><span class="sxs-lookup"><span data-stu-id="c90c2-137">Create a load balancer as follows:</span></span>

1. <span data-ttu-id="c90c2-138">V prohlížeči přejděte na portálu Azure: http://portal.azure.com a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="c90c2-138">From a browser navigate to the Azure portal: http://portal.azure.com and login with your Azure account.</span></span>
2. <span data-ttu-id="c90c2-139">Na levé straně horní části obrazovky, klikněte na tlačítko **nový** > **sítě** > **nástroj pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-139">On the top left-hand side of the screen, click **New** > **Networking** > **Load Balancer**.</span></span> <span data-ttu-id="c90c2-140">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-140">Next, click **Create**.</span></span>
3. <span data-ttu-id="c90c2-141">V okně **Vytvořit nástroj pro vyrovnávání zatížení** zadejte název svého nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c90c2-141">In the **Create load balancer** blade, type a name for your load balancer.</span></span> <span data-ttu-id="c90c2-142">Zde je volána *mylb*.</span><span class="sxs-lookup"><span data-stu-id="c90c2-142">Here it is called *mylb*.</span></span>
4. <span data-ttu-id="c90c2-143">V části veřejnou IP adresu vytvořit novou veřejnou IP adresu volat **PublicIP1**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-143">Under Public IP address, create a new public IP called **PublicIP1**.</span></span>
5. <span data-ttu-id="c90c2-144">Ve skupině prostředků, vyberte existující skupinu prostředků virtuálních počítačů (například *contosofabrikam*).</span><span class="sxs-lookup"><span data-stu-id="c90c2-144">Under Resource Group, select the existing Resource group of your VMs (for example, *contosofabrikam*).</span></span> <span data-ttu-id="c90c2-145">Pak vyberte odpovídající umístění a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-145">Then, select an appropriate location, and then click **OK**.</span></span> <span data-ttu-id="c90c2-146">Nástroj pro vyrovnávání zatížení se začne nasazovat. Úspěšné dokončení nasazení potrvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="c90c2-146">The load balancer will then start to deploy and will take a few minutes to successfully complete deployment.</span></span>
6. <span data-ttu-id="c90c2-147">Po nasazení nástroje pro vyrovnávání zatížení se zobrazí jako prostředek ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="c90c2-147">Once deployed, the load balancer is displayed as a resource in your Resource Group.</span></span>

### <a name="step-3-configure-the-frontend-ip-pool"></a><span data-ttu-id="c90c2-148">Krok 3: Konfigurace fondu IP front-endu</span><span class="sxs-lookup"><span data-stu-id="c90c2-148">STEP 3: Configure the frontend IP pool</span></span>

<span data-ttu-id="c90c2-149">Konfigurujte fond IP front-endu pro každý web (Contoso a Fabrikam) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c90c2-149">Configure your frontend IP pool for each website (Contoso and Fabrikam) as follows:</span></span>

1. <span data-ttu-id="c90c2-150">Na portálu, klikněte na tlačítko **další služby** > typ **veřejnou IP adresu** pole filtru, a pak klikněte na **veřejné IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-150">In the portal, click **More services** > type **Public IP address** in the filter box, and then click **Public IP addresses**.</span></span> <span data-ttu-id="c90c2-151">Klikněte na tlačítko **přidat** směrem nahoru v okně, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c90c2-151">Click **Add** towards the top of the blade that shows up.</span></span>
2. <span data-ttu-id="c90c2-152">Nakonfigurovat dvě veřejné IP adresy (*PublicIP1* a *PublicIP2*) pro oba weby (contoso a fabrikam) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c90c2-152">Configure two Public IP addresses (*PublicIP1* and *PublicIP2*) for both websites (contoso and fabrikam) as follows:</span></span>
    1. <span data-ttu-id="c90c2-153">Zadejte název pro vaše IP adresa front-endu.</span><span class="sxs-lookup"><span data-stu-id="c90c2-153">Type a name for your frontend IP address.</span></span>
    2. <span data-ttu-id="c90c2-154">Pro **skupiny prostředků**, vyberte existující skupinu prostředků virtuálních počítačů (například *contosofabrikam*).</span><span class="sxs-lookup"><span data-stu-id="c90c2-154">For **Resource Group**, select the existing Resource Group of the VMs (for example, *contosofabrikam*).</span></span>
    3. <span data-ttu-id="c90c2-155">Pro **umístění**, vyberte stejné umístění jako virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="c90c2-155">For **Location**, select the same location as the VMs.</span></span>
    4. <span data-ttu-id="c90c2-156">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-156">Click **OK**.</span></span>
    5. <span data-ttu-id="c90c2-157">Po vytvoření jsou dvě veřejné IP adresy, jsou obě zobrazená v **veřejnou IP adresu** okno adresy.</span><span class="sxs-lookup"><span data-stu-id="c90c2-157">Once the two Public IP addresses are created, they are both displayed in the **Public IP** addresses blade.</span></span>
3. <span data-ttu-id="c90c2-158">Na portálu, klikněte na tlačítko **další služby** > typ **nástroj pro vyrovnávání zatížení** pole filtru, a pak klikněte na **Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-158">In the portal, click **More services** > type **load balancer** in the filter box, and then click **Load Balancer**.</span></span>  
4. <span data-ttu-id="c90c2-159">Vyberte pro vyrovnávání zatížení (*mylb*), které chcete přidat fondu IP front-endu.</span><span class="sxs-lookup"><span data-stu-id="c90c2-159">Select the load balancer (*mylb*) that you want to add the frontend IP pool to.</span></span>
5. <span data-ttu-id="c90c2-160">V části **nastavení**, vyberte **front-endové fondy**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-160">Under **Settings**, select **Frontend Pools**.</span></span> <span data-ttu-id="c90c2-161">Pak klikněte na tlačítko **přidat** směrem nahoru v okně, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c90c2-161">Then click **Add** towards the top of the blade that shows up.</span></span>
6. <span data-ttu-id="c90c2-162">Zadejte název pro vaše IP adresa front-endu (*farbikamfe* nebo **contosofe*).</span><span class="sxs-lookup"><span data-stu-id="c90c2-162">Type a name for your frontend IP address (*farbikamfe* or **contosofe*).</span></span>
7. <span data-ttu-id="c90c2-163">Klikněte na tlačítko **IP adresu** a na **zvolte veřejnou IP adresu** okně vyberte IP adresy pro vaší front-endu (*PublicIP1* nebo *PublicIP2*).</span><span class="sxs-lookup"><span data-stu-id="c90c2-163">Click **IP address** and on the **Choose Public IP address** blade, select the IP addresses for your frontend (*PublicIP1* or *PublicIP2*).</span></span>
8. <span data-ttu-id="c90c2-164">Zopakujte kroky 3 až 7 v této části, chcete-li vytvořit druhou IP adresu front-endu.</span><span class="sxs-lookup"><span data-stu-id="c90c2-164">Repeat steps 3 to 7 within this section to create the second frontend IP address.</span></span>
9. <span data-ttu-id="c90c2-165">Po dokončení konfigurace fondu IP front-endu obě IP adresy front-endu jsou zobrazeny v **fond IP front-endu** okno Nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c90c2-165">When the frontend IP pool configuration is complete, both frontend IP addresses are displayed in the **Frontend IP Pool** blade of your load balancer.</span></span> 
    
### <a name="step-4-configure-the-backend-pool"></a><span data-ttu-id="c90c2-166">Krok 4: Konfigurace fondu back-end</span><span class="sxs-lookup"><span data-stu-id="c90c2-166">STEP 4: Configure the backend pool</span></span>   
<span data-ttu-id="c90c2-167">Konfigurace back-endové fondy adres na nástroj pro vyrovnávání zatížení pro každý web (Contoso a Fabrikam) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c90c2-167">Configure the backend address pools on your load balancer for each website (Contoso and Fabrikam) as follows:</span></span>
        
1. <span data-ttu-id="c90c2-168">Na portálu, klikněte na tlačítko **další služby** > zadejte nástroj pro vyrovnávání zatížení do pole Filtr a potom klikněte na **Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-168">In the portal, click **More services** > type load balancer in the filter box, and then click **Load Balancer**.</span></span>  
2. <span data-ttu-id="c90c2-169">Vyberte pro vyrovnávání zatížení (*mylb*), které chcete přidat back-endové fondy k.</span><span class="sxs-lookup"><span data-stu-id="c90c2-169">Select the load balancer (*mylb*) that you want to add the backend pools to.</span></span>
3. <span data-ttu-id="c90c2-170">V části **nastavení**, vyberte **back-endové fondy**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-170">Under **Settings**, select **Backend Pools**.</span></span> <span data-ttu-id="c90c2-171">Zadejte název pro fond back-end (například *contosopool* nebo *fabrikampool*).</span><span class="sxs-lookup"><span data-stu-id="c90c2-171">Type a name for your backend pool (for example, *contosopool* or *fabrikampool*).</span></span> <span data-ttu-id="c90c2-172">Klikněte **přidat** tlačítko k horní části okna, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c90c2-172">Then click the **Add** button toward the top of the blade that shows up.</span></span> 
4. <span data-ttu-id="c90c2-173">Pro **přidružené k**, vyberte **sadu dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-173">For **Associated to**, select **Availability set**.</span></span>
5. <span data-ttu-id="c90c2-174">Pro **sadu dostupnosti**, vyberte **myAvailset**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-174">For **Availability set**, select **myAvailset**.</span></span>
6. <span data-ttu-id="c90c2-175">Přidejte cílový konfigurace IP sítě, pro oba virtuální počítače (viz obrázek 2):</span><span class="sxs-lookup"><span data-stu-id="c90c2-175">Add Target network IP configurations, for both VMs as follows (see Figure 2):</span></span>  
    1. <span data-ttu-id="c90c2-176">Pro **cílový virtuální počítač**, vyberte virtuální počítač, který chcete přidat do fondu back-end (například VM1 nebo virtuálního počítače 2).</span><span class="sxs-lookup"><span data-stu-id="c90c2-176">For **Target Virtual machine**, select the VM that you want to add to the backend pool (for example, VM1 or VM2).</span></span>
    2. <span data-ttu-id="c90c2-177">Pro **konfiguraci IP adresy sítě**, vyberte sekundární konfiguraci IP síťových adaptérů pro tento virtuální počítač (například VM1NIC2 ipconfig2 nebo VM2NIC2 ipconfig2).</span><span class="sxs-lookup"><span data-stu-id="c90c2-177">For **Network IP configuration**, select the secondary NICs IP configuration for that VM (for example, VM1NIC2-ipconfig2 or VM2NIC2-ipconfig2).</span></span>
    <span data-ttu-id="c90c2-178">![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-backendpool.PNG)</span><span class="sxs-lookup"><span data-stu-id="c90c2-178">![LB scenario image](./media/load-balancer-multiple-ip/lb-backendpool.PNG)</span></span>
            
        <span data-ttu-id="c90c2-179">**Obrázek 2**: konfigurace pro vyrovnávání zatížení s back-endové fondy</span><span class="sxs-lookup"><span data-stu-id="c90c2-179">**Figure 2**: Configuring the load balancer with backend pools</span></span>  
7. <span data-ttu-id="c90c2-180">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-180">Click **OK**.</span></span>
8. <span data-ttu-id="c90c2-181">Po dokončení konfigurace fondu back-end i back-endové fondy adres se zobrazují v **okno fond back-end** z nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c90c2-181">When the backend pool configuration is complete, both backend address pools are displayed in the **Backend pool blade** of your load balancer.</span></span>

### <a name="step-5-configure-a-health-probe-for-your-load-balancer"></a><span data-ttu-id="c90c2-182">Krok 5: Konfigurace test stavu pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="c90c2-182">STEP 5: Configure a health probe for your load balancer</span></span>
<span data-ttu-id="c90c2-183">Test stavu pro nástroj pro vyrovnávání zatížení nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c90c2-183">Configure a health probe for your load balancer as follows:</span></span>
    1. <span data-ttu-id="c90c2-184">Na portálu, klikněte na tlačítko Další služby > zadejte nástroj pro vyrovnávání zatížení do pole Filtr a potom klikněte na **Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-184">In the portal, click More services > type load balancer in the filter box, and then click **Load Balancer**.</span></span>  
    2. <span data-ttu-id="c90c2-185">Vyberte, kterou chcete přidat fondy back-end pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c90c2-185">Select the load balancer that you want to add the backend pools to.</span></span>
    3. <span data-ttu-id="c90c2-186">V části **nastavení**, vyberte **test stavu**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-186">Under **Settings**, select **Health probe**.</span></span> <span data-ttu-id="c90c2-187">Pak klikněte na tlačítko **přidat** směrem nahoru v okně, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c90c2-187">Then click **Add** towards the top of the blade that shows up.</span></span>
    4. <span data-ttu-id="c90c2-188">Zadejte název pro test stavu (například protokol HTTP) a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-188">Type a name for the health probe (for example, HTTP), and click **OK**.</span></span>

### <a name="step-6-configure-load-balancing-rules"></a><span data-ttu-id="c90c2-189">Krok 6: Konfigurace pravidel Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="c90c2-189">STEP 6: Configure load balancing rules</span></span>
<span data-ttu-id="c90c2-190">Konfigurace pravidel Vyrovnávání zatížení (*HTTPc* a *HTTPf*) pro každý web následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c90c2-190">Configure load balancing rules (*HTTPc* and *HTTPf*) for each website as follows:</span></span>
    
1. <span data-ttu-id="c90c2-191">V části **nastavení**, vyberte **test stavu**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-191">Under **Settings**, select **Health probe**.</span></span> <span data-ttu-id="c90c2-192">Pak klikněte na tlačítko **přidat** směrem nahoru v okně, který se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c90c2-192">Then click **Add** towards the top of the blade that shows up.</span></span>
2. <span data-ttu-id="c90c2-193">Pro **název**, zadejte název pro pravidlo Vyrovnávání zatížení (například *HTTPc* pro společnost Contoso, nebo *HTTPf* pro společnost Fabrikam)</span><span class="sxs-lookup"><span data-stu-id="c90c2-193">For **Name**, type a name for the load balancing rule (for example, *HTTPc* for Contoso, or *HTTPf* for Fabrikam)</span></span>
3. <span data-ttu-id="c90c2-194">Pro adresu IP front-endu, vyberte front-endovou IP adresu (například *Contosofe* nebo *Fabrikamfe*)</span><span class="sxs-lookup"><span data-stu-id="c90c2-194">For Frontend IP address, select the the frontend IP address (for example *Contosofe* or *Fabrikamfe*)</span></span>
4. <span data-ttu-id="c90c2-195">Pro **Port** a **back-endový port**, ponechte výchozí hodnotu **80**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-195">For **Port** and **Backend port**, keep the default value **80**.</span></span>
5. <span data-ttu-id="c90c2-196">Pro **plovoucí IP adresa (přímá odpověď ze serveru)**, klikněte na tlačítko **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-196">For **Floating IP (direct server return)**, click **Enabled**.</span></span>
6. <span data-ttu-id="c90c2-197">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c90c2-197">Click **OK**.</span></span>
7. <span data-ttu-id="c90c2-198">Opakujte kroky 1 až 6 v rámci této části vytvoříte druhé pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c90c2-198">Repeat steps 1 to 6 within this section to create the second Load Balancer rule.</span></span>
8. <span data-ttu-id="c90c2-199">Pokud Vyrovnávání zatížení pravidla konfigurace je hotová, jak pravidla ((*HTTPc* a *HTTPf*) se zobrazí v **pravidla Vyrovnávání zatížení** okno Nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c90c2-199">When the load balancing rules configuration is complete, both rules ((*HTTPc* and *HTTPf*) are displayed in the **Load balancing rules** blade of your load balancer.</span></span>

### <a name="step-7-configure-dns-records"></a><span data-ttu-id="c90c2-200">Krok 7: Nakonfigurovat záznamy DNS</span><span class="sxs-lookup"><span data-stu-id="c90c2-200">STEP 7: Configure DNS records</span></span>
<span data-ttu-id="c90c2-201">Nakonec je nutné nakonfigurovat záznamy prostředků DNS tak, aby odkazoval na příslušné front-endovou IP adresu služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c90c2-201">Finally, you must configure DNS resource records to point to the respective frontend IP address of the load balancer.</span></span> <span data-ttu-id="c90c2-202">Může hostovat vaše domény v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="c90c2-202">You may host your domains in Azure DNS.</span></span> <span data-ttu-id="c90c2-203">Další informace o používání s nástrojem pro vyrovnávání zatížení Azure DNS najdete v tématu [pomocí Azure DNS s jinými službami Azure](../dns/dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="c90c2-203">For more information about using Azure DNS with Load Balancer, see [Using Azure DNS with other Azure services](../dns/dns-for-azure-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c90c2-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c90c2-204">Next steps</span></span>
- <span data-ttu-id="c90c2-205">Další informace o tom, jak kombinací v Azure v rámci služby Vyrovnávání zatížení [pomocí služby Vyrovnávání zatížení v Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span><span class="sxs-lookup"><span data-stu-id="c90c2-205">Learn more about how to combine load balancing services in Azure in [Using load-balancing services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span></span>
- <span data-ttu-id="c90c2-206">Zjistěte, jak můžete použít různé typy protokolů v Azure ke správě a odstraňování potíží pro vyrovnávání zatížení v [protokolu pro vyrovnávání zatížení Azure analytics](../load-balancer/load-balancer-monitor-log.md).</span><span class="sxs-lookup"><span data-stu-id="c90c2-206">Learn how you can use different types of logs in Azure to manage and troubleshoot load balancer in [Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).</span></span>