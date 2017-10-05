---
title: "Jak načíst vyvážit virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o použití nástroje pro vyrovnávání zatížení Azure k vytvoření vysoce dostupné a zabezpečené aplikace napříč tři virtuální počítače s Linuxem"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 7b3a089d2f6386afcc46cbc4377594be0d758fc6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-load-balance-linux-virtual-machines-in-azure-to-create-a-highly-available-application"></a><span data-ttu-id="42e71-103">Jak načíst vyvážit virtuální počítače s Linuxem v Azure k vytvoření vysoce dostupné aplikace</span><span class="sxs-lookup"><span data-stu-id="42e71-103">How to load balance Linux virtual machines in Azure to create a highly available application</span></span>
<span data-ttu-id="42e71-104">Vyrovnávání zatížení poskytuje vyšší úroveň dostupnosti rozloží příchozí žádosti napříč více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="42e71-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="42e71-105">V tomto kurzu informace o různé součásti nástroje pro vyrovnávání zatížení Azure, které distribuci přenosů a zajištění vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="42e71-105">In this tutorial, you learn about the different components of the Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="42e71-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="42e71-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="42e71-107">Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="42e71-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="42e71-108">Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="42e71-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="42e71-109">Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42e71-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="42e71-110">Pomocí cloud init můžete vytvořit základní aplikaci Node.js</span><span class="sxs-lookup"><span data-stu-id="42e71-110">Use cloud-init to create a basic Node.js app</span></span>
> * <span data-ttu-id="42e71-111">Vytváření virtuálních počítačů a připojit ke službě Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42e71-111">Create virtual machines and attach to a load balancer</span></span>
> * <span data-ttu-id="42e71-112">Zobrazit nástroj pro vyrovnávání zatížení v akci</span><span class="sxs-lookup"><span data-stu-id="42e71-112">View a load balancer in action</span></span>
> * <span data-ttu-id="42e71-113">Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42e71-113">Add and remove VMs from a load balancer</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="42e71-114">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="42e71-114">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="42e71-115">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="42e71-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="42e71-116">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="42e71-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="azure-load-balancer-overview"></a><span data-ttu-id="42e71-117">Přehled nástroje pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="42e71-117">Azure load balancer overview</span></span>
<span data-ttu-id="42e71-118">K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku.</span><span class="sxs-lookup"><span data-stu-id="42e71-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="42e71-119">Sondu stavu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a distribuuje jenom přenosy na provozní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="42e71-119">A load balancer health probe monitors a given port on each VM and only distributes traffic to an operational VM.</span></span>

<span data-ttu-id="42e71-120">Můžete definovat na front-endové konfiguraci protokolu IP, která obsahuje jeden nebo více veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="42e71-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="42e71-121">Tuto konfiguraci front-end IP adresy umožňuje Vyrovnávání zatížení a aplikace přístupné přes Internet.</span><span class="sxs-lookup"><span data-stu-id="42e71-121">This front-end IP configuration allows your load balancer and applications to be accessible over the Internet.</span></span> 

<span data-ttu-id="42e71-122">Virtuální počítače připojit k nástroji pro vyrovnávání zatížení pomocí jejich virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="42e71-122">Virtual machines connect to a load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="42e71-123">K distribuci provoz na virtuální počítače, fond back-end adres obsahuje IP adresy virtuální (NIC) připojené ke službě Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="42e71-123">To distribute traffic to the VMs, a back-end address pool contains the IP addresses of the virtual (NICs) connected to the load balancer.</span></span>

<span data-ttu-id="42e71-124">Pokud chcete řídit tok přenosů dat, definujete pravidla nástroje pro vyrovnávání zatížení pro určité porty a protokoly, které jsou mapovány na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="42e71-124">To control the flow of traffic, you define load balancer rules for specific ports and protocols that map to your VMs.</span></span>

<span data-ttu-id="42e71-125">Pokud jste postupovali podle předchozích kurzu [vytvořit škálovací sadu virtuálních počítačů](tutorial-create-vmss.md), nástroj pro vyrovnávání zatížení pro vás vytvořil.</span><span class="sxs-lookup"><span data-stu-id="42e71-125">If you followed the previous tutorial to [create a virtual machine scale set](tutorial-create-vmss.md), a load balancer was created for you.</span></span> <span data-ttu-id="42e71-126">Všechny tyto součásti byly nakonfigurovány pro vás v rámci sady škálování.</span><span class="sxs-lookup"><span data-stu-id="42e71-126">All these components were configured for you as part of the scale set.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="42e71-127">Vytvořit nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="42e71-127">Create Azure load balancer</span></span>
<span data-ttu-id="42e71-128">Tato část podrobně popisuje, jak můžete vytvořit a nakonfigurovat jednotlivé komponenty služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="42e71-128">This section details how you can create and configure each component of the load balancer.</span></span> <span data-ttu-id="42e71-129">Než bude možné vytvořit nástroj pro vyrovnávání zatížení, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-129">Before you can create your load balancer, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="42e71-130">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupLoadBalancer* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="42e71-130">The following example creates a resource group named *myResourceGroupLoadBalancer* in the *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="42e71-131">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="42e71-131">Create a public IP address</span></span>
<span data-ttu-id="42e71-132">Pro přístup k vaší aplikace v síti Internet, musíte nástroj pro vyrovnávání zatížení veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="42e71-132">To access your app on the Internet, you need a public IP address for the load balancer.</span></span> <span data-ttu-id="42e71-133">Vytvoření veřejné IP adresy s [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-133">Create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="42e71-134">Následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* v *myResourceGroupLoadBalancer* skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="42e71-134">The following example creates a public IP address named *myPublicIP* in the *myResourceGroupLoadBalancer* resource group:</span></span>

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="42e71-135">Vytvoření nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42e71-135">Create a load balancer</span></span>
<span data-ttu-id="42e71-136">Vytvořit nástroj pro vyrovnávání zatížení s [az sítě lb vytvořit](/cli/azure/network/lb#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-136">Create a load balancer with [az network lb create](/cli/azure/network/lb#create).</span></span> <span data-ttu-id="42e71-137">Následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem *myLoadBalancer* a přiřadí *myPublicIP* ke konfiguraci front-end IP adresy:</span><span class="sxs-lookup"><span data-stu-id="42e71-137">The following example creates a load balancer named *myLoadBalancer* and assigns the *myPublicIP* address to the front-end IP configuration:</span></span>

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a><span data-ttu-id="42e71-138">Vytvoření test stavu</span><span class="sxs-lookup"><span data-stu-id="42e71-138">Create a health probe</span></span>
<span data-ttu-id="42e71-139">Povolit službu Vyrovnávání zatížení k monitorování stavu aplikace, použijte Test stavu.</span><span class="sxs-lookup"><span data-stu-id="42e71-139">To allow the load balancer to monitor the status of your app, you use a health probe.</span></span> <span data-ttu-id="42e71-140">Test stavu dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení, podle jejich reakce na kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="42e71-140">The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks.</span></span> <span data-ttu-id="42e71-141">Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení po dvě po sobě jdoucích selhání v intervalech 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="42e71-141">By default, a VM is removed from the load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="42e71-142">Můžete vytvořit test stavu na základě protokolu nebo na stránce Kontrola specifickém stavu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="42e71-142">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="42e71-143">Následující příklad vytvoří sondou TCP.</span><span class="sxs-lookup"><span data-stu-id="42e71-143">The following example creates a TCP probe.</span></span> <span data-ttu-id="42e71-144">Můžete také vytvořit vlastní sondy HTTP pro další kontroly podrobné stavu.</span><span class="sxs-lookup"><span data-stu-id="42e71-144">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="42e71-145">Pokud používáte vlastní sondu HTTP, musíte vytvořit stránka pro kontrolu stavu, jako například *healthcheck.js*.</span><span class="sxs-lookup"><span data-stu-id="42e71-145">When using a custom HTTP probe, you must create the health check page, such as *healthcheck.js*.</span></span> <span data-ttu-id="42e71-146">Sondy musí vrátit **HTTP 200 OK** odpovědi pro vyrovnávání zatížení na hostiteli mějte otočení.</span><span class="sxs-lookup"><span data-stu-id="42e71-146">The probe must return an **HTTP 200 OK** response for the load balancer to keep the host in rotation.</span></span>

<span data-ttu-id="42e71-147">K vytvoření stavu sondou TCP, použijete [vytvoření testu vyrovnáváním zatížení sítě az](/cli/azure/network/lb/probe#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-147">To create a TCP health probe, you use [az network lb probe create](/cli/azure/network/lb/probe#create).</span></span> <span data-ttu-id="42e71-148">Následující příklad vytvoří kontrolu stavu s názvem *myHealthProbe*:</span><span class="sxs-lookup"><span data-stu-id="42e71-148">The following example creates a health probe named *myHealthProbe*:</span></span>

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="42e71-149">Vytvořit pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="42e71-149">Create a load balancer rule</span></span>
<span data-ttu-id="42e71-150">Pravidlo Vyrovnávání zatížení se používá k definování, jak se provoz rozděluje k virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="42e71-150">A load balancer rule is used to define how traffic is distributed to the VMs.</span></span> <span data-ttu-id="42e71-151">Můžete definovat front-endové konfiguraci protokolu IP pro příchozí provoz a fond back-end IP příjem provozu, společně s požadovaný zdrojový a cílový port.</span><span class="sxs-lookup"><span data-stu-id="42e71-151">You define the front-end IP configuration for the incoming traffic and the back-end IP pool to receive the traffic, along with the required source and destination port.</span></span> <span data-ttu-id="42e71-152">Pokud chcete mít jistotu, že virtuální počítače pouze v pořádku přijímat přenosy, také definovat test stavu použít.</span><span class="sxs-lookup"><span data-stu-id="42e71-152">To make sure only healthy VMs receive traffic, you also define the health probe to use.</span></span>

<span data-ttu-id="42e71-153">Vytvořit pravidlo Vyrovnávání zatížení s [vytvořit pravidlo vyrovnáváním zatížení sítě az](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-153">Create a load balancer rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="42e71-154">Následující příklad vytvoří pravidlo s názvem *myLoadBalancerRule*, používá *myHealthProbe* test stavu a zůstatky přenosy na portu *80*:</span><span class="sxs-lookup"><span data-stu-id="42e71-154">The following example creates a rule named *myLoadBalancerRule*, uses the *myHealthProbe* health probe, and balances traffic on port *80*:</span></span>

```azurecli-interactive 
az network lb rule create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a><span data-ttu-id="42e71-155">Konfigurace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="42e71-155">Configure virtual network</span></span>
<span data-ttu-id="42e71-156">Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte doprovodné materiály virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="42e71-156">Before you deploy some VMs and can test your balancer, create the supporting virtual network resources.</span></span> <span data-ttu-id="42e71-157">Další informace o virtuálních sítích najdete v tématu [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="42e71-157">For more information about virtual networks, see the [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="42e71-158">Vytvoření síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="42e71-158">Create network resources</span></span>
<span data-ttu-id="42e71-159">Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-159">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="42e71-160">Následující příklad vytvoří virtuální síť s názvem *myVnet* s podsítí s názvem *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="42e71-160">The following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

<span data-ttu-id="42e71-161">Chcete-li přidat skupinu zabezpečení sítě, je použít [vytvořit az sítě nsg](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-161">To add a network security group, you use [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="42e71-162">Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="42e71-162">The following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="42e71-163">Vytvoření pravidla skupiny zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-163">Create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="42e71-164">Následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem *myNetworkSecurityGroupRule*:</span><span class="sxs-lookup"><span data-stu-id="42e71-164">The following example creates a network security group rule named *myNetworkSecurityGroupRule*:</span></span>

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

<span data-ttu-id="42e71-165">Virtuální síťové adaptéry jsou vytvořeny pomocí [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-165">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="42e71-166">Následující příklad vytvoří tři virtuálních síťových karet.</span><span class="sxs-lookup"><span data-stu-id="42e71-166">The following example creates three virtual NICs.</span></span> <span data-ttu-id="42e71-167">(Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro vaši aplikaci v následujících krocích).</span><span class="sxs-lookup"><span data-stu-id="42e71-167">(One virtual NIC for each VM you create for your app in the following steps).</span></span> <span data-ttu-id="42e71-168">Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a jejich přidání do Vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="42e71-168">You can create additional virtual NICs and VMs at any time and add them to the load balancer:</span></span>

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-virtual-machines"></a><span data-ttu-id="42e71-169">Vytváření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="42e71-169">Create virtual machines</span></span>

### <a name="create-cloud-init-config"></a><span data-ttu-id="42e71-170">Vytvoření konfigurace cloudu init</span><span class="sxs-lookup"><span data-stu-id="42e71-170">Create cloud-init config</span></span>
<span data-ttu-id="42e71-171">V předchozích kurz [postup přizpůsobení virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md), jste se dozvěděli, jak automatizovat přizpůsobení virtuálního počítače s inicializací cloudu.</span><span class="sxs-lookup"><span data-stu-id="42e71-171">In a previous tutorial on [How to customize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how to automate VM customization with cloud-init.</span></span> <span data-ttu-id="42e71-172">Konfiguračního souboru stejné cloudové init můžete použít k instalaci NGINX a spuštění jednoduchou aplikaci Node.js "Zdravím svět".</span><span class="sxs-lookup"><span data-stu-id="42e71-172">You can use the same cloud-init configuration file to install NGINX and run a simple 'Hello World' Node.js app.</span></span>

<span data-ttu-id="42e71-173">V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a vložte následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="42e71-173">In your current shell, create a file named *cloud-init.txt* and paste the following configuration.</span></span> <span data-ttu-id="42e71-174">Například vytvoření souboru v prostředí cloudu není na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="42e71-174">For example, create the file in the Cloud Shell not on your local machine.</span></span> <span data-ttu-id="42e71-175">Zadejte `sensible-editor cloud-init.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory.</span><span class="sxs-lookup"><span data-stu-id="42e71-175">Enter `sensible-editor cloud-init.txt` to create the file and see a list of available editors.</span></span> <span data-ttu-id="42e71-176">Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:</span><span class="sxs-lookup"><span data-stu-id="42e71-176">Make sure that the whole cloud-init file is copied correctly, especially the first line:</span></span>

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-virtual-machines"></a><span data-ttu-id="42e71-177">Vytváření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="42e71-177">Create virtual machines</span></span>
<span data-ttu-id="42e71-178">Pokud chcete zvýšit vysokou dostupnost vaší aplikace, umístíte virtuální počítače v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="42e71-178">To improve the high availability of your app, place your VMs in an availability set.</span></span> <span data-ttu-id="42e71-179">Další informace o nastavení dostupnosti, najdete v předchozí [vytváření vysoce dostupných virtuálních počítačů](tutorial-availability-sets.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="42e71-179">For more information about availability sets, see the previous [How to create highly available virtual machines](tutorial-availability-sets.md) tutorial.</span></span>

<span data-ttu-id="42e71-180">Vytvořit sadu s dostupnosti [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-180">Create an availability set with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="42e71-181">Následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="42e71-181">The following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

<span data-ttu-id="42e71-182">Nyní můžete vytvořit virtuálních počítačů s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="42e71-182">Now you can create the VMs with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="42e71-183">Následující příklad vytvoří tři virtuální počítače a generuje klíče SSH, pokud už neexistují:</span><span class="sxs-lookup"><span data-stu-id="42e71-183">The following example creates three VMs and generates SSH keys if they do not already exist:</span></span>

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroupLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

<span data-ttu-id="42e71-184">Existují úlohy na pozadí, které dál běžet po rozhraní příkazového řádku Azure se vrátíte do řádku.</span><span class="sxs-lookup"><span data-stu-id="42e71-184">There are background tasks that continue to run after the Azure CLI returns you to the prompt.</span></span> <span data-ttu-id="42e71-185">`--no-wait` Parametr nečeká všechny na dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="42e71-185">The `--no-wait` parameter does not wait for all the tasks to complete.</span></span> <span data-ttu-id="42e71-186">To může být jiná několik minut, než může aplikaci používat.</span><span class="sxs-lookup"><span data-stu-id="42e71-186">It may be another couple of minutes before you can access the app.</span></span> <span data-ttu-id="42e71-187">Test stavu nástroje pro vyrovnávání zatížení automaticky rozpozná, když aplikace běží na každém virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="42e71-187">The load balancer health probe automatically detects when the app is running on each VM.</span></span> <span data-ttu-id="42e71-188">Jakmile aplikace běží, spustí se pravidlo Vyrovnávání zatížení k distribuci přenosů.</span><span class="sxs-lookup"><span data-stu-id="42e71-188">Once the app is running, the load balancer rule starts to distribute traffic.</span></span>


## <a name="test-load-balancer"></a><span data-ttu-id="42e71-189">Nástroj pro vyrovnávání zatížení testu</span><span class="sxs-lookup"><span data-stu-id="42e71-189">Test load balancer</span></span>
<span data-ttu-id="42e71-190">Získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="42e71-190">Obtain the public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="42e71-191">Následující příklad, získá IP adresu pro *myPublicIP* vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="42e71-191">The following example obtains the IP address for *myPublicIP* created earlier:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="42e71-192">Potom můžete zadat veřejnou IP adresu v do webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="42e71-192">You can then enter the public IP address in to a web browser.</span></span> <span data-ttu-id="42e71-193">Mějte na paměti,-trvá několik minut virtuálních počítačů bude připravená, před spuštěním nástroje pro vyrovnávání zatížení softwaru k distribuci přenosů na ně.</span><span class="sxs-lookup"><span data-stu-id="42e71-193">Remember - it takes a few minutes the the VMs to be ready before the load balancer starts to distribute traffic to them.</span></span> <span data-ttu-id="42e71-194">Aplikace se zobrazí, včetně názvu hostitele virtuálního počítače, který nástroje pro vyrovnávání zatížení distribuován provoz jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="42e71-194">The app is displayed, including the hostname of the VM that the load balancer distributed traffic to as in the following example:</span></span>

![Spuštěné aplikace Node.js](./media/tutorial-load-balancer/running-nodejs-app.png)

<span data-ttu-id="42e71-196">Nástroje pro vyrovnávání zatížení provoz distribuovat mezi všechny tři virtuální počítače spuštěné aplikace najdete můžete můžete vynutit obnovení webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="42e71-196">To see the load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="42e71-197">Přidání a odebrání virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="42e71-197">Add and remove VMs</span></span>
<span data-ttu-id="42e71-198">Potřebujete provést údržbu na virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="42e71-198">You may need to perform maintenance on the VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="42e71-199">Jak nakládat s zvýšení provozu do vaší aplikace, musíte pro přidání dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="42e71-199">To deal with increased traffic to your app, you may need to add additional VMs.</span></span> <span data-ttu-id="42e71-200">V této části se dozvíte, jak odebrat nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="42e71-200">This section shows you how to remove or add a VM from the load balancer.</span></span>

### <a name="remove-a-vm-from-the-load-balancer"></a><span data-ttu-id="42e71-201">Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42e71-201">Remove a VM from the load balancer</span></span>
<span data-ttu-id="42e71-202">Virtuální počítač můžete odebrat z fondu adres back-end s [odebrat az sítě síťový adaptér ip-config fond adres](/cli/azure/network/nic/ip-config/address-pool#remove).</span><span class="sxs-lookup"><span data-stu-id="42e71-202">You can remove a VM from the backend address pool with [az network nic ip-config address-pool remove](/cli/azure/network/nic/ip-config/address-pool#remove).</span></span> <span data-ttu-id="42e71-203">Následující příklad odebere virtuální síťový adaptér pro **Můjvp2** z *myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="42e71-203">The following example removes the virtual NIC for **myVM2** from *myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

<span data-ttu-id="42e71-204">Zobrazit nástroje pro vyrovnávání zatížení provoz distribuovat mezi zbývající dva virtuální počítače používající vaši aplikaci je můžete vynutit obnovení webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="42e71-204">To see the load balancer distribute traffic across the remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="42e71-205">Nyní můžete provést údržbu na virtuální počítač, jako je instalace aktualizací operačního systému nebo provádění restartování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="42e71-205">You can now perform maintenance on the VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-to-the-load-balancer"></a><span data-ttu-id="42e71-206">Přidat virtuální počítač ke službě Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42e71-206">Add a VM to the load balancer</span></span>
<span data-ttu-id="42e71-207">Po provedení údržby virtuálních počítačů, nebo pokud potřebujete rozšířit kapacitu, můžete přidat virtuální počítač do fondu adres back-end s [az sítě síťový adaptér ip-config fond adres přidat](/cli/azure/network/nic/ip-config/address-pool#add).</span><span class="sxs-lookup"><span data-stu-id="42e71-207">After performing VM maintenance, or if you need to expand capacity, you can add a VM to the backend address pool with [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#add).</span></span> <span data-ttu-id="42e71-208">Následující příklad přidá virtuální síťovou kartu pro **Můjvp2** k *myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="42e71-208">The following example adds the virtual NIC for **myVM2** to *myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a><span data-ttu-id="42e71-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42e71-209">Next steps</span></span>
<span data-ttu-id="42e71-210">V tomto kurzu jste vytvořili pro vyrovnávání zatížení a je připojený virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="42e71-210">In this tutorial, you created a load balancer and attached VMs to it.</span></span> <span data-ttu-id="42e71-211">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="42e71-211">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="42e71-212">Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="42e71-212">Create an Azure load balancer</span></span>
> * <span data-ttu-id="42e71-213">Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="42e71-213">Create a load balancer health probe</span></span>
> * <span data-ttu-id="42e71-214">Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42e71-214">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="42e71-215">Pomocí cloud init můžete vytvořit základní aplikaci Node.js</span><span class="sxs-lookup"><span data-stu-id="42e71-215">Use cloud-init to create a basic Node.js app</span></span>
> * <span data-ttu-id="42e71-216">Vytváření virtuálních počítačů a připojit ke službě Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42e71-216">Create virtual machines and attach to a load balancer</span></span>
> * <span data-ttu-id="42e71-217">Zobrazit nástroj pro vyrovnávání zatížení v akci</span><span class="sxs-lookup"><span data-stu-id="42e71-217">View a load balancer in action</span></span>
> * <span data-ttu-id="42e71-218">Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42e71-218">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="42e71-219">Přechodu na v dalším kurzu se dozvíte informace o komponentách virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="42e71-219">Advance to the next tutorial to learn more about Azure virtual network components.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="42e71-220">Správa virtuálních počítačů a virtuálních sítí</span><span class="sxs-lookup"><span data-stu-id="42e71-220">Manage VMs and virtual networks</span></span>](tutorial-virtual-network.md)
