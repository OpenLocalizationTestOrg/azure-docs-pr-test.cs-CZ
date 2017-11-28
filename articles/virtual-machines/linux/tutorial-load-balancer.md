---
title: "aaaHow tooload vyvážit virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure zatížení vyrovnávání toocreate vysoká dostupnost a zabezpečení aplikací v rámci tři virtuální počítače s Linuxem"
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
ms.openlocfilehash: f01752c3caec3489ee13e63000775769f3236e11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-linux-virtual-machines-in-azure-toocreate-a-highly-available-application"></a><span data-ttu-id="fc438-103">Jak tooload vyvážit virtuální počítače s Linuxem v Azure toocreate vysoce dostupné aplikace</span><span class="sxs-lookup"><span data-stu-id="fc438-103">How tooload balance Linux virtual machines in Azure toocreate a highly available application</span></span>
<span data-ttu-id="fc438-104">Vyrovnávání zatížení poskytuje vyšší úroveň dostupnosti rozloží příchozí žádosti napříč více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fc438-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="fc438-105">V tomto kurzu informace o hello různé součásti nástroje pro vyrovnávání zatížení Azure hello které distribuci přenosů a zajištění vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="fc438-105">In this tutorial, you learn about hello different components of hello Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="fc438-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="fc438-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fc438-107">Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="fc438-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="fc438-108">Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="fc438-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="fc438-109">Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="fc438-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="fc438-110">Použít cloudové init toocreate základní aplikaci Node.js</span><span class="sxs-lookup"><span data-stu-id="fc438-110">Use cloud-init toocreate a basic Node.js app</span></span>
> * <span data-ttu-id="fc438-111">Vytváření virtuálních počítačů a připojte tooa nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="fc438-111">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="fc438-112">Zobrazit nástroj pro vyrovnávání zatížení v akci</span><span class="sxs-lookup"><span data-stu-id="fc438-112">View a load balancer in action</span></span>
> * <span data-ttu-id="fc438-113">Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="fc438-113">Add and remove VMs from a load balancer</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fc438-114">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fc438-114">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="fc438-115">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="fc438-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="fc438-116">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fc438-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="azure-load-balancer-overview"></a><span data-ttu-id="fc438-117">Přehled nástroje pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="fc438-117">Azure load balancer overview</span></span>
<span data-ttu-id="fc438-118">K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku.</span><span class="sxs-lookup"><span data-stu-id="fc438-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="fc438-119">Stav sondu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a pouze distribuuje provoz tooan provozní virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fc438-119">A load balancer health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span>

<span data-ttu-id="fc438-120">Můžete definovat na front-endové konfiguraci protokolu IP, která obsahuje jeden nebo více veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="fc438-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="fc438-121">Tuto konfiguraci front-end IP adresy umožňuje vaší zatížení vyrovnávání a aplikace toobe přístupné přes hello Internet.</span><span class="sxs-lookup"><span data-stu-id="fc438-121">This front-end IP configuration allows your load balancer and applications toobe accessible over hello Internet.</span></span> 

<span data-ttu-id="fc438-122">Virtuální počítače připojit nástroj pro vyrovnávání zatížení tooa pomocí jejich virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="fc438-122">Virtual machines connect tooa load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="fc438-123">toodistribute toohello přenosy virtuálních počítačů, fond back-end adres obsahuje hello IP adresy služby Vyrovnávání zatížení připojená toohello virtuální (NIC) hello.</span><span class="sxs-lookup"><span data-stu-id="fc438-123">toodistribute traffic toohello VMs, a back-end address pool contains hello IP addresses of hello virtual (NICs) connected toohello load balancer.</span></span>

<span data-ttu-id="fc438-124">toocontrol hello tok přenosů, můžete definovat pravidla nástroje pro vyrovnávání zatížení pro určité porty a protokoly, které mapují tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fc438-124">toocontrol hello flow of traffic, you define load balancer rules for specific ports and protocols that map tooyour VMs.</span></span>

<span data-ttu-id="fc438-125">Pokud jste postupovali podle předchozích kurzu hello příliš[vytvořit škálovací sadu virtuálních počítačů](tutorial-create-vmss.md), nástroj pro vyrovnávání zatížení pro vás vytvořil.</span><span class="sxs-lookup"><span data-stu-id="fc438-125">If you followed hello previous tutorial too[create a virtual machine scale set](tutorial-create-vmss.md), a load balancer was created for you.</span></span> <span data-ttu-id="fc438-126">Všechny tyto součásti byly nakonfigurovány pro vás v rámci sady škálování hello.</span><span class="sxs-lookup"><span data-stu-id="fc438-126">All these components were configured for you as part of hello scale set.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="fc438-127">Vytvořit nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="fc438-127">Create Azure load balancer</span></span>
<span data-ttu-id="fc438-128">V této části podrobně popisuje, jak můžete vytvořit a nakonfigurovat jednotlivé komponenty služby Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="fc438-128">This section details how you can create and configure each component of hello load balancer.</span></span> <span data-ttu-id="fc438-129">Než bude možné vytvořit nástroj pro vyrovnávání zatížení, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-129">Before you can create your load balancer, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="fc438-130">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupLoadBalancer* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="fc438-130">hello following example creates a resource group named *myResourceGroupLoadBalancer* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="fc438-131">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="fc438-131">Create a public IP address</span></span>
<span data-ttu-id="fc438-132">tooaccess hello vaší aplikace na Internetu, musíte na veřejnou IP adresu pro nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="fc438-132">tooaccess your app on hello Internet, you need a public IP address for hello load balancer.</span></span> <span data-ttu-id="fc438-133">Vytvoření veřejné IP adresy s [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-133">Create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="fc438-134">Hello následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* v hello *myResourceGroupLoadBalancer* skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="fc438-134">hello following example creates a public IP address named *myPublicIP* in hello *myResourceGroupLoadBalancer* resource group:</span></span>

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="fc438-135">Vytvoření nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="fc438-135">Create a load balancer</span></span>
<span data-ttu-id="fc438-136">Vytvořit nástroj pro vyrovnávání zatížení s [az sítě lb vytvořit](/cli/azure/network/lb#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-136">Create a load balancer with [az network lb create](/cli/azure/network/lb#create).</span></span> <span data-ttu-id="fc438-137">Hello následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem *myLoadBalancer* a přiřadí hello *myPublicIP* konfiguraci front-end IP adresy toohello:</span><span class="sxs-lookup"><span data-stu-id="fc438-137">hello following example creates a load balancer named *myLoadBalancer* and assigns hello *myPublicIP* address toohello front-end IP configuration:</span></span>

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a><span data-ttu-id="fc438-138">Vytvoření test stavu</span><span class="sxs-lookup"><span data-stu-id="fc438-138">Create a health probe</span></span>
<span data-ttu-id="fc438-139">tooallow hello stavu služby Vyrovnávání zatížení toomonitor hello vaší aplikace, použijte Test stavu.</span><span class="sxs-lookup"><span data-stu-id="fc438-139">tooallow hello load balancer toomonitor hello status of your app, you use a health probe.</span></span> <span data-ttu-id="fc438-140">Test stavu Hello dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení hello podle jejich kontroly toohealth odpovědi.</span><span class="sxs-lookup"><span data-stu-id="fc438-140">hello health probe dynamically adds or removes VMs from hello load balancer rotation based on their response toohealth checks.</span></span> <span data-ttu-id="fc438-141">Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení hello po dvě po sobě jdoucích selhání v intervalech 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="fc438-141">By default, a VM is removed from hello load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="fc438-142">Můžete vytvořit test stavu na základě protokolu nebo na stránce Kontrola specifickém stavu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fc438-142">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="fc438-143">Hello následující ukázka vytvoří sondou TCP.</span><span class="sxs-lookup"><span data-stu-id="fc438-143">hello following example creates a TCP probe.</span></span> <span data-ttu-id="fc438-144">Můžete také vytvořit vlastní sondy HTTP pro další kontroly podrobné stavu.</span><span class="sxs-lookup"><span data-stu-id="fc438-144">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="fc438-145">Pokud používáte vlastní sondu HTTP, musíte vytvořit hello stránka pro kontrolu stavu, jako například *healthcheck.js*.</span><span class="sxs-lookup"><span data-stu-id="fc438-145">When using a custom HTTP probe, you must create hello health check page, such as *healthcheck.js*.</span></span> <span data-ttu-id="fc438-146">Test Hello musí vrátit **HTTP 200 OK** odpovědi pro hello zatížení vyrovnávání tookeep hello hostitele v otočení.</span><span class="sxs-lookup"><span data-stu-id="fc438-146">hello probe must return an **HTTP 200 OK** response for hello load balancer tookeep hello host in rotation.</span></span>

<span data-ttu-id="fc438-147">toocreate sondou TCP stavu, můžete použít [vytvoření testu vyrovnáváním zatížení sítě az](/cli/azure/network/lb/probe#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-147">toocreate a TCP health probe, you use [az network lb probe create](/cli/azure/network/lb/probe#create).</span></span> <span data-ttu-id="fc438-148">Hello následující příklad vytvoří sondu stavu s názvem *myHealthProbe*:</span><span class="sxs-lookup"><span data-stu-id="fc438-148">hello following example creates a health probe named *myHealthProbe*:</span></span>

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="fc438-149">Vytvořit pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="fc438-149">Create a load balancer rule</span></span>
<span data-ttu-id="fc438-150">Pravidlo Vyrovnávání zatížení je použité toodefine jak přenosy jsou distribuované toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fc438-150">A load balancer rule is used toodefine how traffic is distributed toohello VMs.</span></span> <span data-ttu-id="fc438-151">Můžete definovat hello front-endové konfiguraci protokolu IP pro příchozí provoz hello a hello back-end fondu tooreceive hello provozu IP, spolu s hello požadované zdrojový a cílový port.</span><span class="sxs-lookup"><span data-stu-id="fc438-151">You define hello front-end IP configuration for hello incoming traffic and hello back-end IP pool tooreceive hello traffic, along with hello required source and destination port.</span></span> <span data-ttu-id="fc438-152">toomake se, že virtuální počítače pouze v pořádku přijímat přenosy, také definovat toouse test stavu hello.</span><span class="sxs-lookup"><span data-stu-id="fc438-152">toomake sure only healthy VMs receive traffic, you also define hello health probe toouse.</span></span>

<span data-ttu-id="fc438-153">Vytvořit pravidlo Vyrovnávání zatížení s [vytvořit pravidlo vyrovnáváním zatížení sítě az](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-153">Create a load balancer rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="fc438-154">Hello následující příklad vytvoří pravidlo s názvem *myLoadBalancerRule*, používá hello *myHealthProbe* test stavu a zůstatky přenosy na portu *80*:</span><span class="sxs-lookup"><span data-stu-id="fc438-154">hello following example creates a rule named *myLoadBalancerRule*, uses hello *myHealthProbe* health probe, and balances traffic on port *80*:</span></span>

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


## <a name="configure-virtual-network"></a><span data-ttu-id="fc438-155">Konfigurace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="fc438-155">Configure virtual network</span></span>
<span data-ttu-id="fc438-156">Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte hello podpora prostředky virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fc438-156">Before you deploy some VMs and can test your balancer, create hello supporting virtual network resources.</span></span> <span data-ttu-id="fc438-157">Další informace o virtuálních sítích najdete v tématu hello [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="fc438-157">For more information about virtual networks, see hello [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="fc438-158">Vytvoření síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="fc438-158">Create network resources</span></span>
<span data-ttu-id="fc438-159">Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-159">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="fc438-160">Hello následující příklad vytvoří virtuální síť s názvem *myVnet* s podsítí s názvem *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="fc438-160">hello following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

<span data-ttu-id="fc438-161">tooadd skupinu zabezpečení sítě, můžete použít [vytvořit az sítě nsg](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-161">tooadd a network security group, you use [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="fc438-162">Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="fc438-162">hello following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="fc438-163">Vytvoření pravidla skupiny zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-163">Create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="fc438-164">Hello následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem *myNetworkSecurityGroupRule*:</span><span class="sxs-lookup"><span data-stu-id="fc438-164">hello following example creates a network security group rule named *myNetworkSecurityGroupRule*:</span></span>

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

<span data-ttu-id="fc438-165">Virtuální síťové adaptéry jsou vytvořeny pomocí [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-165">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="fc438-166">Hello následující příklad vytvoří tři virtuálních síťových karet.</span><span class="sxs-lookup"><span data-stu-id="fc438-166">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="fc438-167">(Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro aplikace v rámci hello následující kroky).</span><span class="sxs-lookup"><span data-stu-id="fc438-167">(One virtual NIC for each VM you create for your app in hello following steps).</span></span> <span data-ttu-id="fc438-168">Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a přidat je nástroj pro vyrovnávání zatížení toohello:</span><span class="sxs-lookup"><span data-stu-id="fc438-168">You can create additional virtual NICs and VMs at any time and add them toohello load balancer:</span></span>

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

## <a name="create-virtual-machines"></a><span data-ttu-id="fc438-169">Vytváření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fc438-169">Create virtual machines</span></span>

### <a name="create-cloud-init-config"></a><span data-ttu-id="fc438-170">Vytvoření konfigurace cloudu init</span><span class="sxs-lookup"><span data-stu-id="fc438-170">Create cloud-init config</span></span>
<span data-ttu-id="fc438-171">V předchozích kurzu na [jak toocustomize virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md), jste se naučili jak tooautomate přizpůsobení virtuálního počítače s inicializací cloudu.</span><span class="sxs-lookup"><span data-stu-id="fc438-171">In a previous tutorial on [How toocustomize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how tooautomate VM customization with cloud-init.</span></span> <span data-ttu-id="fc438-172">Můžete použít hello stejné cloudové init konfigurační soubor tooinstall NGINX a spusťte jednoduchou aplikaci Node.js "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="fc438-172">You can use hello same cloud-init configuration file tooinstall NGINX and run a simple 'Hello World' Node.js app.</span></span>

<span data-ttu-id="fc438-173">V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a hello vložte následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fc438-173">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="fc438-174">Můžete například vytvořte soubor hello v hello cloudové prostředí není na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="fc438-174">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="fc438-175">Zadejte `sensible-editor cloud-init.txt` toocreate hello souboru a zobrazit seznam dostupných editory.</span><span class="sxs-lookup"><span data-stu-id="fc438-175">Enter `sensible-editor cloud-init.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="fc438-176">Ujistěte se, že tento soubor celou cloudu init hello správně zkopírován, zejména hello první řádek:</span><span class="sxs-lookup"><span data-stu-id="fc438-176">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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

### <a name="create-virtual-machines"></a><span data-ttu-id="fc438-177">Vytváření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fc438-177">Create virtual machines</span></span>
<span data-ttu-id="fc438-178">tooimprove hello vysokou dostupnost vaší aplikace, umístit virtuální počítače v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="fc438-178">tooimprove hello high availability of your app, place your VMs in an availability set.</span></span> <span data-ttu-id="fc438-179">Další informace o nastavení dostupnosti, najdete v části hello předchozí [jak toocreate vysoce dostupných virtuálních počítačů](tutorial-availability-sets.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="fc438-179">For more information about availability sets, see hello previous [How toocreate highly available virtual machines](tutorial-availability-sets.md) tutorial.</span></span>

<span data-ttu-id="fc438-180">Vytvořit sadu s dostupnosti [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-180">Create an availability set with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="fc438-181">Hello následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="fc438-181">hello following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

<span data-ttu-id="fc438-182">Nyní můžete vytvořit hello virtuálních počítačů s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="fc438-182">Now you can create hello VMs with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="fc438-183">Hello následující příklad vytvoří tři virtuální počítače a generuje klíče SSH, pokud už neexistují:</span><span class="sxs-lookup"><span data-stu-id="fc438-183">hello following example creates three VMs and generates SSH keys if they do not already exist:</span></span>

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

<span data-ttu-id="fc438-184">Existují úlohy na pozadí, které pokračovat toorun po hello příkazového řádku Azure CLI vrátí, toohello řádku.</span><span class="sxs-lookup"><span data-stu-id="fc438-184">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="fc438-185">Hello `--no-wait` parametr nečeká pro všechny úlohy toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="fc438-185">hello `--no-wait` parameter does not wait for all hello tasks toocomplete.</span></span> <span data-ttu-id="fc438-186">To může být jiná několik minut před přístupem k aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="fc438-186">It may be another couple of minutes before you can access hello app.</span></span> <span data-ttu-id="fc438-187">Hello test stavu nástroje pro vyrovnávání zatížení automaticky zjišťuje, když na každém virtuálním počítači běží aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fc438-187">hello load balancer health probe automatically detects when hello app is running on each VM.</span></span> <span data-ttu-id="fc438-188">Jakmile hello aplikace běží, se spustí pravidlo Vyrovnávání zatížení hello toodistribute provoz.</span><span class="sxs-lookup"><span data-stu-id="fc438-188">Once hello app is running, hello load balancer rule starts toodistribute traffic.</span></span>


## <a name="test-load-balancer"></a><span data-ttu-id="fc438-189">Nástroj pro vyrovnávání zatížení testu</span><span class="sxs-lookup"><span data-stu-id="fc438-189">Test load balancer</span></span>
<span data-ttu-id="fc438-190">Získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="fc438-190">Obtain hello public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="fc438-191">Hello následující příklad získá hello IP adresu pro *myPublicIP* vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="fc438-191">hello following example obtains hello IP address for *myPublicIP* created earlier:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="fc438-192">Potom můžete zadat hello veřejnou IP adresu ve webovém prohlížeči tooa.</span><span class="sxs-lookup"><span data-stu-id="fc438-192">You can then enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="fc438-193">Mějte na paměti, – trvá několik minut hello hello virtuální počítače toobe připravené před spuštěním nástroje pro vyrovnávání zatížení hello toodistribute provoz toothem.</span><span class="sxs-lookup"><span data-stu-id="fc438-193">Remember - it takes a few minutes hello hello VMs toobe ready before hello load balancer starts toodistribute traffic toothem.</span></span> <span data-ttu-id="fc438-194">Hello aplikace se zobrazí, včetně hello název hostitele virtuálního počítače hello tento nástroj pro vyrovnávání zatížení hello distribuované tooas provoz v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="fc438-194">hello app is displayed, including hello hostname of hello VM that hello load balancer distributed traffic tooas in hello following example:</span></span>

![Spuštěné aplikace Node.js](./media/tutorial-load-balancer/running-nodejs-app.png)

<span data-ttu-id="fc438-196">Nástroj pro vyrovnávání zatížení toosee hello distribuuje provoz přes všechny tři virtuální počítače spuštěné aplikace, můžete můžete vynutit obnovení webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="fc438-196">toosee hello load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="fc438-197">Přidání a odebrání virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fc438-197">Add and remove VMs</span></span>
<span data-ttu-id="fc438-198">Může být nutné tooperform údržby na hello virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="fc438-198">You may need tooperform maintenance on hello VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="fc438-199">toodeal zvýšení provozu tooyour aplikace, může být nutné tooadd dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fc438-199">toodeal with increased traffic tooyour app, you may need tooadd additional VMs.</span></span> <span data-ttu-id="fc438-200">V této části se dozvíte, jak tooremove nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="fc438-200">This section shows you how tooremove or add a VM from hello load balancer.</span></span>

### <a name="remove-a-vm-from-hello-load-balancer"></a><span data-ttu-id="fc438-201">Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení hello</span><span class="sxs-lookup"><span data-stu-id="fc438-201">Remove a VM from hello load balancer</span></span>
<span data-ttu-id="fc438-202">Virtuální počítač můžete odebrat z fondu adres back-end hello s [odebrat az sítě síťový adaptér ip-config fond adres](/cli/azure/network/nic/ip-config/address-pool#remove).</span><span class="sxs-lookup"><span data-stu-id="fc438-202">You can remove a VM from hello backend address pool with [az network nic ip-config address-pool remove](/cli/azure/network/nic/ip-config/address-pool#remove).</span></span> <span data-ttu-id="fc438-203">Následující příklad odebere Hello hello virtuální síťovou kartu pro **Můjvp2** z *myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="fc438-203">hello following example removes hello virtual NIC for **myVM2** from *myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

<span data-ttu-id="fc438-204">Nástroj pro vyrovnávání zatížení toosee hello distribuuje provoz přes hello zbývající dva virtuální počítače používající vaši aplikaci je můžete vynutit obnovení webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="fc438-204">toosee hello load balancer distribute traffic across hello remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="fc438-205">Nyní můžete provést údržbu na hello virtuálních počítačů, jako je instalace aktualizací operačního systému nebo provádění restartování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fc438-205">You can now perform maintenance on hello VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-toohello-load-balancer"></a><span data-ttu-id="fc438-206">Přidání toohello virtuálního počítače, služby pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="fc438-206">Add a VM toohello load balancer</span></span>
<span data-ttu-id="fc438-207">Po provedení údržby virtuálních počítačů, nebo pokud potřebujete tooexpand kapacitu, můžete přidat fond adres back-end toohello virtuálních počítačů s [az sítě síťový adaptér ip-config fond adres přidat](/cli/azure/network/nic/ip-config/address-pool#add).</span><span class="sxs-lookup"><span data-stu-id="fc438-207">After performing VM maintenance, or if you need tooexpand capacity, you can add a VM toohello backend address pool with [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#add).</span></span> <span data-ttu-id="fc438-208">Hello následující příklad přidá hello virtuální síťovou kartu pro **Můjvp2** příliš*myLoadBalancer*:</span><span class="sxs-lookup"><span data-stu-id="fc438-208">hello following example adds hello virtual NIC for **myVM2** too*myLoadBalancer*:</span></span>

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a><span data-ttu-id="fc438-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc438-209">Next steps</span></span>
<span data-ttu-id="fc438-210">V tomto kurzu jste vytvořili pro vyrovnávání zatížení a připojené tooit virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fc438-210">In this tutorial, you created a load balancer and attached VMs tooit.</span></span> <span data-ttu-id="fc438-211">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="fc438-211">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fc438-212">Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="fc438-212">Create an Azure load balancer</span></span>
> * <span data-ttu-id="fc438-213">Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="fc438-213">Create a load balancer health probe</span></span>
> * <span data-ttu-id="fc438-214">Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="fc438-214">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="fc438-215">Použít cloudové init toocreate základní aplikaci Node.js</span><span class="sxs-lookup"><span data-stu-id="fc438-215">Use cloud-init toocreate a basic Node.js app</span></span>
> * <span data-ttu-id="fc438-216">Vytváření virtuálních počítačů a připojte tooa nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="fc438-216">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="fc438-217">Zobrazit nástroj pro vyrovnávání zatížení v akci</span><span class="sxs-lookup"><span data-stu-id="fc438-217">View a load balancer in action</span></span>
> * <span data-ttu-id="fc438-218">Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="fc438-218">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="fc438-219">Posunutí toohello další kurz toolearn informace o komponentách virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="fc438-219">Advance toohello next tutorial toolearn more about Azure virtual network components.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fc438-220">Správa virtuálních počítačů a virtuálních sítí</span><span class="sxs-lookup"><span data-stu-id="fc438-220">Manage VMs and virtual networks</span></span>](tutorial-virtual-network.md)
