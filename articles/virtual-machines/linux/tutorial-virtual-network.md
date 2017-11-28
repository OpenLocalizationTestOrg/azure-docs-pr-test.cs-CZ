---
title: "aaaAzure virtuálních sítí a virtuální počítače s Linuxem | Microsoft Docs"
description: "Kurz – Správa virtuálních sítí Azure a virtuální počítače Linux s hello rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 57e6bd4de16f0e31d53dc67bf50dc5730d43712b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-hello-azure-cli"></a><span data-ttu-id="fbb1c-103">Správa virtuálních sítí Azure a virtuální počítače Linux s hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="fbb1c-103">Manage Azure Virtual Networks and Linux Virtual Machines with hello Azure CLI</span></span>

<span data-ttu-id="fbb1c-104">Virtuální počítače Azure pomocí Azure sítě pro vnitřní a vnější síťovou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="fbb1c-105">Tento kurz vás provede nasazením dva virtuální počítače a konfigurace Azure sítě pro tyto virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-105">This tutorial walks through deploying two virtual machines and configuring Azure networking for these VMs.</span></span> <span data-ttu-id="fbb1c-106">Hello příklady v tomto kurzu předpokládají, že virtuální počítače hello hostují webové aplikace s databáze back-end, ale aplikace není nasazena v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-106">hello examples in this tutorial assume that hello VMs are hosting a web application with a database back-end, however an application is not deployed in hello tutorial.</span></span> <span data-ttu-id="fbb1c-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="fbb1c-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fbb1c-108">Nasazení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="fbb1c-108">Deploy a virtual network</span></span>
> * <span data-ttu-id="fbb1c-109">Vytvořte podsíť virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-109">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="fbb1c-110">Připojte tooa podsíť virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fbb1c-110">Attach virtual machines tooa subnet</span></span>
> * <span data-ttu-id="fbb1c-111">Spravovat veřejné IP adresy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fbb1c-111">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="fbb1c-112">Zabezpečené příchozí přenosy z Internetu</span><span class="sxs-lookup"><span data-stu-id="fbb1c-112">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="fbb1c-113">Zabezpečení přenosu tooVM virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fbb1c-113">Secure VM tooVM traffic</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fbb1c-114">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-114">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="fbb1c-115">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="fbb1c-116">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fbb1c-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="vm-networking-overview"></a><span data-ttu-id="fbb1c-117">Přehled sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fbb1c-117">VM networking overview</span></span>

<span data-ttu-id="fbb1c-118">Virtuální sítě Azure povolit zabezpečené sítě připojení mezi virtuálními počítači, hello Internetu a jinými službami Azure, jako je například Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-118">Azure virtual networks enable secure network connections between virtual machines, hello internet, and other Azure services such as Azure SQL database.</span></span> <span data-ttu-id="fbb1c-119">Virtuální sítě jsou rozdělit do logických segmenty označují jako podsítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-119">Virtual networks are broken down into logical segments called subnets.</span></span> <span data-ttu-id="fbb1c-120">Podsítě se používají toocontrol tok sítě a jako hranice zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-120">Subnets are used toocontrol network flow, and as a security boundary.</span></span> <span data-ttu-id="fbb1c-121">Při nasazování virtuálního počítače, obvykle zahrnuje rozhraní virtuální sítě, které je připojené tooa podsítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-121">When deploying a VM, it generally includes a virtual network interface, which is attached tooa subnet.</span></span>

## <a name="deploy-virtual-network"></a><span data-ttu-id="fbb1c-122">Nasazení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="fbb1c-122">Deploy virtual network</span></span>

<span data-ttu-id="fbb1c-123">V tomto kurzu se vytvoří jedné virtuální sítě se dvěma podsítěmi.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-123">For this tutorial, a single virtual network is created with two subnets.</span></span> <span data-ttu-id="fbb1c-124">Podsíť front-end pro hostování webové aplikace a podsíť back-end pro hostování databázový server.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-124">A front-end subnet for hosting a web application, and a back-end subnet for hosting a database server.</span></span>

<span data-ttu-id="fbb1c-125">Než bude možné vytvořit virtuální síť, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="fbb1c-125">Before you can create a virtual network, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="fbb1c-126">Hello následující příklad vytvoří skupinu prostředků s názvem *myRGNetwork* v umístění eastus hello.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-126">hello following example creates a resource group named *myRGNetwork* in hello eastus location.</span></span>

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a><span data-ttu-id="fbb1c-127">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="fbb1c-127">Create virtual network</span></span>

<span data-ttu-id="fbb1c-128">Nám hello [vytvoření sítě vnet az](/cli/azure/network/vnet#create) příkaz toocreate virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-128">Us hello [az network vnet create](/cli/azure/network/vnet#create) command toocreate a virtual network.</span></span> <span data-ttu-id="fbb1c-129">V tomto příkladu je název sítě hello *mvVnet* a předponu adresy z *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-129">In this example, hello network is named *mvVnet* and is given an address prefix of *10.0.0.0/16*.</span></span> <span data-ttu-id="fbb1c-130">Podsíť je vytvořen také s názvem *mySubnetFrontEnd* a předponu *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-130">A subnet is also created with a name of *mySubnetFrontEnd* and a prefix of *10.0.1.0/24*.</span></span> <span data-ttu-id="fbb1c-131">Později v tomto kurzu front-end virtuální počítač je připojený toothis podsíť.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-131">Later in this tutorial a front-end VM is connected toothis subnet.</span></span> 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a><span data-ttu-id="fbb1c-132">Vytvoření podsítě</span><span class="sxs-lookup"><span data-stu-id="fbb1c-132">Create subnet</span></span>

<span data-ttu-id="fbb1c-133">Přidá novou podsíť toohello virtuální sítě pomocí hello [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-133">A new subnet is added toohello virtual network using hello [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) command.</span></span> <span data-ttu-id="fbb1c-134">V tomto příkladu je hello podsíť s názvem *mySubnetBackEnd* a předponu adresy z *10.0.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-134">In this example, hello subnet is named *mySubnetBackEnd* and is given an address prefix of *10.0.2.0/24*.</span></span> <span data-ttu-id="fbb1c-135">Tato podsíť se používá u všech služeb back-end.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-135">This subnet is used with all back-end services.</span></span>

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

<span data-ttu-id="fbb1c-136">V tomto okamžiku síť byla vytvořil a rozdělena na dvě podsítě, jeden pro front-endové služby a jinou pro back endové služby.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-136">At this point, a network has been created and segmented into two subnets, one for front-end services, and another for back-end services.</span></span> <span data-ttu-id="fbb1c-137">V další části hello virtuální počítače jsou vytvořené a připojené toothese podsítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-137">In hello next section, virtual machines are created and connected toothese subnets.</span></span>

## <a name="understand-public-ip-address"></a><span data-ttu-id="fbb1c-138">Pochopení veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="fbb1c-138">Understand public IP address</span></span>

<span data-ttu-id="fbb1c-139">Veřejná IP adresa umožňuje toobe prostředky Azure dostupné na hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-139">A public IP address allows Azure resources toobe accessible on hello internet.</span></span> <span data-ttu-id="fbb1c-140">V této části kurzu hello je virtuální počítač vytvořený toodemonstrate jak toowork s veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-140">In this section of hello tutorial, a VM is created toodemonstrate how toowork with public IP addresses.</span></span>

### <a name="allocation-method"></a><span data-ttu-id="fbb1c-141">Metoda přidělování</span><span class="sxs-lookup"><span data-stu-id="fbb1c-141">Allocation method</span></span>

<span data-ttu-id="fbb1c-142">Veřejné IP adresy mohou být přiděleny jako dynamická nebo statická.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-142">A public IP address can be allocated as either dynamic or static.</span></span> <span data-ttu-id="fbb1c-143">Ve výchozím nastavení veřejnou IP adresu přidělí dynamicky.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-143">By default, public IP address dynamically allocated.</span></span> <span data-ttu-id="fbb1c-144">Dynamické IP adresy vydávají, když virtuální počítač je navrácena.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-144">Dynamic IP addresses are released when a VM is deallocated.</span></span> <span data-ttu-id="fbb1c-145">To způsobí, že hello IP adresu toochange během všechny operace, která zahrnuje navrácení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-145">This behavior causes hello IP address toochange during any operation that includes a VM deallocation.</span></span>

<span data-ttu-id="fbb1c-146">může být nastavena metoda přidělení Hello toostatic, což zajistí, že zůstanou hello IP adresy přiřazené tooa virtuální počítač, i během deallocated stavu.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-146">hello allocation method can be set toostatic, which ensures that hello IP address remain assigned tooa VM, even during a deallocated state.</span></span> <span data-ttu-id="fbb1c-147">Při použití staticky přidělená adresa IP, nelze zadat IP adresu hello sám sebe.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-147">When using a statically allocated IP address, hello IP address itself cannot be specified.</span></span> <span data-ttu-id="fbb1c-148">Místo toho je přidělen z fondu adres k dispozici.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-148">Instead, it is allocated from a pool of available addresses.</span></span>

### <a name="dynamic-allocation"></a><span data-ttu-id="fbb1c-149">Dynamické přidělení</span><span class="sxs-lookup"><span data-stu-id="fbb1c-149">Dynamic allocation</span></span>

<span data-ttu-id="fbb1c-150">Při vytváření virtuálního počítače pomocí hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz, je hello výchozí veřejnou IP adresu metoda přidělení dynamické.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-150">When creating a VM with hello [az vm create](/cli/azure/vm#create) command, hello default public IP address allocation method is dynamic.</span></span> <span data-ttu-id="fbb1c-151">V následujícím příkladu hello se vytvoří virtuální počítač s dynamickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-151">In hello following example, a VM is created with a dynamic IP address.</span></span> 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontEndVM \
  --vnet-name myVnet \
  --subnet mySubnetFrontEnd \
  --nsg myNSGFrontEnd \
  --public-ip-address myFrontEndIP \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="static-allocation"></a><span data-ttu-id="fbb1c-152">Statického přidělování</span><span class="sxs-lookup"><span data-stu-id="fbb1c-152">Static allocation</span></span>

<span data-ttu-id="fbb1c-153">Při vytváření virtuálního počítače pomocí hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz, zahrnují hello `--public-ip-address-allocation static` argument tooassign statickou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-153">When creating a virtual machine using hello [az vm create](/cli/azure/vm#create) command, include hello `--public-ip-address-allocation static` argument tooassign a static public IP address.</span></span> <span data-ttu-id="fbb1c-154">Tato operace není ukázáno v tomto kurzu, ale v další části hello dynamicky přidělené IP adresy je změněné tooa staticky přidělenou adresu.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-154">This operation is not demonstrated in this tutorial, however in hello next section a dynamically allocated IP address is changed tooa statically allocated address.</span></span> 

### <a name="change-allocation-method"></a><span data-ttu-id="fbb1c-155">Změnit přidělení – metoda</span><span class="sxs-lookup"><span data-stu-id="fbb1c-155">Change allocation method</span></span>

<span data-ttu-id="fbb1c-156">Metoda přidělování adres IP Hello se dá změnit pomocí hello [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-156">hello IP address allocation method can be changed using hello [az network public-ip update](/cli/azure/network/public-ip#update) command.</span></span> <span data-ttu-id="fbb1c-157">V tomto příkladu hello metoda přidělení IP adres hello front-end virtuálního počítače se změní toostatic.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-157">In this example, hello IP address allocation method of hello front-end VM is changed toostatic.</span></span>

<span data-ttu-id="fbb1c-158">Nejprve zrušit přidělení hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-158">First, deallocate hello VM.</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

<span data-ttu-id="fbb1c-159">Použití hello [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip#update) příkaz metoda přidělení tooupdate hello.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-159">Use hello [az network public-ip update](/cli/azure/network/public-ip#update) command tooupdate hello allocation method.</span></span> <span data-ttu-id="fbb1c-160">V takovém případě hello `--allocation-method` byl nastaven příliš*statické*.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-160">In this case, hello `--allocation-method` is being set too*static*.</span></span>

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

<span data-ttu-id="fbb1c-161">Spusťte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-161">Start hello VM.</span></span>

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a><span data-ttu-id="fbb1c-162">Žádné veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="fbb1c-162">No public IP address</span></span>

<span data-ttu-id="fbb1c-163">Často, virtuální počítač nemusí toobe přístupné prostřednictvím Internetu hello.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-163">Often, a VM does not need toobe accessible over hello internet.</span></span> <span data-ttu-id="fbb1c-164">toocreate virtuální počítač bez veřejnou IP adresu, použijte hello `--public-ip-address ""` argument s prázdnou sadu dvojité uvozovky.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-164">toocreate a VM without a public IP address, use hello `--public-ip-address ""` argument with an empty set of double quotes.</span></span> <span data-ttu-id="fbb1c-165">Tato konfigurace je ukázán později v tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="fbb1c-165">This configuration is demonstrated later in this tutorial</span></span>

## <a name="secure-network-traffic"></a><span data-ttu-id="fbb1c-166">Zabezpečení provozu sítě</span><span class="sxs-lookup"><span data-stu-id="fbb1c-166">Secure network traffic</span></span>

<span data-ttu-id="fbb1c-167">Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, které povolí nebo zakáže přenášení tooresources provoz sítě připojené tooAzure virtuálních sítí (VNet).</span><span class="sxs-lookup"><span data-stu-id="fbb1c-167">A network security group (NSG) contains a list of security rules that allow or deny network traffic tooresources connected tooAzure Virtual Networks (VNet).</span></span> <span data-ttu-id="fbb1c-168">Skupiny Nsg můžou být přidružené toosubnets nebo jednotlivých síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-168">NSGs can be associated toosubnets or individual network interfaces.</span></span> <span data-ttu-id="fbb1c-169">Je-li skupinu NSG je spojen s síťovým rozhraním, platí pouze hello přidružené virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-169">When an NSG is associated with a network interface, it applies only hello associated VM.</span></span> <span data-ttu-id="fbb1c-170">Pokud skupinu NSG přidružená tooa podsíť, hello pravidla použít tooall prostředky připojené toohello podsítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-170">When an NSG is associated tooa subnet, hello rules apply tooall resources connected toohello subnet.</span></span> 

### <a name="network-security-group-rules"></a><span data-ttu-id="fbb1c-171">Pravidla skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="fbb1c-171">Network security group rules</span></span>

<span data-ttu-id="fbb1c-172">Pravidla NSG definovat síťové porty, přes které provoz povolený nebo zakázaný.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-172">NSG rules define networking ports over which traffic is allowed or denied.</span></span> <span data-ttu-id="fbb1c-173">Hello pravidla může obsahovat zdrojové a cílové rozsahy IP adres, tak, aby je řízen provoz mezi konkrétní systémy nebo podsítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-173">hello rules can include source and destination IP address ranges so that traffic is controlled between specific systems or subnets.</span></span> <span data-ttu-id="fbb1c-174">Pravidla NSG také zahrnovat prioritu (mezi 1 – a 4096).</span><span class="sxs-lookup"><span data-stu-id="fbb1c-174">NSG rules also include a priority (between 1—and 4096).</span></span> <span data-ttu-id="fbb1c-175">Pravidla se vyhodnocují v hello pořadí podle priority.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-175">Rules are evaluated in hello order of priority.</span></span> <span data-ttu-id="fbb1c-176">Pravidlo s prioritou 100 vyhodnotí před pravidlo s prioritou 200.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-176">A rule with a priority of 100 is evaluated before a rule with priority 200.</span></span>

<span data-ttu-id="fbb1c-177">Všechny skupiny NSG obsahují sadu výchozích pravidel.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-177">All NSGs contain a set of default rules.</span></span> <span data-ttu-id="fbb1c-178">nelze odstranit Hello výchozí pravidla, ale protože je jim přiřazená nejnižší priorita hello, dají se přepsat pravidly hello, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-178">hello default rules cannot be deleted, but because they are assigned hello lowest priority, they can be overridden by hello rules that you create.</span></span>

- <span data-ttu-id="fbb1c-179">**Virtuální síť** – provoz pocházející a ukončování ve virtuální síti je povolena v příchozí a odchozí.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-179">**Virtual network** - Traffic originating and ending in a virtual network is allowed both in inbound and outbound directions.</span></span>
- <span data-ttu-id="fbb1c-180">**Internet** – odchozí provoz je povolený, ale jsou blokovány příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-180">**Internet** - Outbound traffic is allowed, but inbound traffic is blocked.</span></span>
- <span data-ttu-id="fbb1c-181">**Nástroj pro vyrovnávání zatížení** -povolit Azure zatížení vyrovnávání tooprobe hello stavu virtuálních počítačů a instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-181">**Load balancer** - Allow Azure’s load balancer tooprobe hello health of your VMs and role instances.</span></span> <span data-ttu-id="fbb1c-182">Pokud nepoužíváte skupinu s vyrovnáváním zatížení, můžete přepsat toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-182">If you are not using a load balanced set, you can override this rule.</span></span>

### <a name="create-network-security-groups"></a><span data-ttu-id="fbb1c-183">Vytvoření skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="fbb1c-183">Create network security groups</span></span>

<span data-ttu-id="fbb1c-184">Skupina zabezpečení sítě lze vytvořit v hello stejný čas jako virtuálního počítače pomocí hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-184">A network security group can be created at hello same time as a VM using hello [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="fbb1c-185">Když to uděláte, je spojen s rozhraní sítě virtuálních počítačů hello hello NSG a automaticky vytvořená tooallow přenosy na portu je pravidlo NSG *22* z jakéhokoli zdroje.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-185">When doing so, hello NSG is associated with hello VMs network interface and an NSG rule is auto created tooallow traffic on port *22* from any source.</span></span> <span data-ttu-id="fbb1c-186">V tomto kurzu hello hello front-end NSG se automaticky vytvořené pomocí virtuálních počítačů front-end.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-186">Earlier in this tutorial, hello front-end NSG was auto-created with hello front-end VM.</span></span> <span data-ttu-id="fbb1c-187">Pravidlo NSG se také automaticky vytvořenou pro port 22.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-187">An NSG rule was also auto created for port 22.</span></span> 

<span data-ttu-id="fbb1c-188">V některých případech může být užitečné toopre-vytvořit skupinu NSG, například když by neměly být vytvářeny výchozí pravidla pro SSH, nebo když hello NSG by měly být připojené tooa podsítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-188">In some cases, it may be helpful toopre-create an NSG, such as when default SSH rules should not be created, or when hello NSG should be attached tooa subnet.</span></span> 

<span data-ttu-id="fbb1c-189">Použití hello [vytvořit az sítě nsg](/cli/azure/network/nsg#create) příkaz toocreate skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-189">Use hello [az network nsg create](/cli/azure/network/nsg#create) command toocreate a network security group.</span></span>

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

<span data-ttu-id="fbb1c-190">Místo přidružení hello NSG tooa síťové rozhraní, je přidružená k podsíti.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-190">Instead of associating hello NSG tooa network interface, it is associated with a subnet.</span></span> <span data-ttu-id="fbb1c-191">V této konfiguraci dědí všechny virtuální počítač, který je připojený toohello podsíť hello pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-191">In this configuration, any VM that is attached toohello subnet inherits hello NSG rules.</span></span>

<span data-ttu-id="fbb1c-192">Aktualizovat hello existující podsíť s názvem *mySubnetBackEnd* s hello nová skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-192">Update hello existing subnet named *mySubnetBackEnd* with hello new NSG.</span></span>

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

<span data-ttu-id="fbb1c-193">Nyní vytvořit virtuální počítač, který je připojený toohello *mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-193">Now create a virtual machine, which is attached toohello *mySubnetBackEnd*.</span></span> <span data-ttu-id="fbb1c-194">Všimněte si, že hello `--nsg` argument má hodnotu prázdný dvojité uvozovky.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-194">Notice that hello `--nsg` argument has a value of empty double quotes.</span></span> <span data-ttu-id="fbb1c-195">Skupina NSG nemusí toobe vytvořené pomocí hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-195">An NSG does not need toobe created with hello VM.</span></span> <span data-ttu-id="fbb1c-196">Hello virtuální počítač je připojený toohello back-end podsítě, který je chráněn s hello předem vytvořené NSG back-end.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-196">hello VM is attached toohello back-end subnet, which is protected with hello pre-created back-end NSG.</span></span> <span data-ttu-id="fbb1c-197">Tato skupina NSG použije toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-197">This NSG applies toohello VM.</span></span> <span data-ttu-id="fbb1c-198">Také si zde všimnout, že hello `--public-ip-address` argument má hodnotu prázdný dvojité uvozovky.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-198">Also, notice here that hello `--public-ip-address` argument has a value of empty double quotes.</span></span> <span data-ttu-id="fbb1c-199">Tato konfigurace vytvoří virtuální počítač bez veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-199">This configuration creates a VM without a public IP address.</span></span> 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackEndVM \
  --vnet-name myVnet \
  --subnet mySubnetBackEnd \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="secure-incoming-traffic"></a><span data-ttu-id="fbb1c-200">Zabezpečené příchozí provoz</span><span class="sxs-lookup"><span data-stu-id="fbb1c-200">Secure incoming traffic</span></span>

<span data-ttu-id="fbb1c-201">Hello front-end virtuálních počítačů v okamžiku vytvoření pravidlo NSG byl vytvořen tooallow příchozí přenosy na portu 22.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-201">When hello front-end VM was created, an NSG rule was created tooallow incoming traffic on port 22.</span></span> <span data-ttu-id="fbb1c-202">Toto pravidlo umožňuje toohello připojení SSH virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-202">This rule allows SSH connections toohello VM.</span></span> <span data-ttu-id="fbb1c-203">V tomto příkladu má být povoleno přenosy na portu *80*.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-203">For this example, traffic should also be allowed on port *80*.</span></span> <span data-ttu-id="fbb1c-204">Tato konfigurace umožňuje toobe webové aplikace na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-204">This configuration allows a web application toobe accessed on hello VM.</span></span>

<span data-ttu-id="fbb1c-205">Použití hello [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz toocreate pravidlo pro port *80*.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-205">Use hello [az network nsg rule create](/cli/azure/network/nsg/rule#create) command toocreate a rule for port *80*.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGFrontEnd \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

<span data-ttu-id="fbb1c-206">Hello front-end virtuálního počítače je teď pouze přístupné na portu *22* a port *80*.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-206">hello front-end VM is now only accessible on port *22* and port *80*.</span></span> <span data-ttu-id="fbb1c-207">Všechny ostatní příchozí přenosy je blokováno v hello skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-207">All other incoming traffic is blocked at hello network security group.</span></span> <span data-ttu-id="fbb1c-208">To může být užitečné toovisualize hello NSG pravidlo konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-208">It may be helpful toovisualize hello NSG rule configurations.</span></span> <span data-ttu-id="fbb1c-209">Konfigurace pravidla NSG návratový hello s hello [az sítě pravidlo seznamu](/cli/azure/network/nsg/rule#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-209">Return hello NSG rule configuration with hello [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGFrontEnd --output table
```

<span data-ttu-id="fbb1c-210">Výstup:</span><span class="sxs-lookup"><span data-stu-id="fbb1c-210">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix      DestinationPortRange  Direction    Name                 Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -----------------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                                               22  Inbound      default-allow-ssh        1000  Tcp         Succeeded            myRGNetwork      *                      *
Allow     *                                               80  Inbound      http                      200  Tcp         Succeeded            myRGNetwork      *                      *
```

### <a name="secure-vm-toovm-traffic"></a><span data-ttu-id="fbb1c-211">Zabezpečení přenosu tooVM virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fbb1c-211">Secure VM tooVM traffic</span></span>

<span data-ttu-id="fbb1c-212">Pravidla skupiny zabezpečení sítě můžete také použít mezi virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-212">Network security group rules can also apply between VMs.</span></span> <span data-ttu-id="fbb1c-213">V tomto příkladu hello hello front-end virtuálních počítačů musí toocommunicate s back-end virtuálních počítačů na portu *22* a *3306*.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-213">For this example, hello front-end VM needs toocommunicate with hello back-end VM on port *22* and *3306*.</span></span> <span data-ttu-id="fbb1c-214">Tato konfigurace umožňuje připojení SSH z hello front-end virtuálních počítačů a taky umožnit aplikaci na hello front-end toocommunicate virtuálních počítačů s databází MySQL back-end.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-214">This configuration allows SSH connections from hello front-end VM, and also allow an application on hello front-end VM toocommunicate with a back-end MySQL database.</span></span> <span data-ttu-id="fbb1c-215">Všechny ostatní přenosy by se zablokovat mezi virtuálními počítači hello front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-215">All other traffic should be blocked between hello front-end and back-end virtual machines.</span></span>

<span data-ttu-id="fbb1c-216">Použití hello [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz toocreate pravidlo pro port 22.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-216">Use hello [az network nsg rule create](/cli/azure/network/nsg/rule#create) command toocreate a rule for port 22.</span></span> <span data-ttu-id="fbb1c-217">Všimněte si, že hello `--source-address-prefix` argument určuje hodnotu *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-217">Notice that hello `--source-address-prefix` argument specifies a value of *10.0.1.0/24*.</span></span> <span data-ttu-id="fbb1c-218">Tato konfigurace zajistí, že jsou povoleny pouze přenosy z podsítě front-endu hello prostřednictvím hello NSG.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-218">This configuration ensures that only traffic from hello front-end subnet is allowed through hello NSG.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

<span data-ttu-id="fbb1c-219">Teď můžete přidáte pravidlo pro provoz MySQL na portu 3306.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-219">Now add a rule for MySQL traffic on port 3306.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

<span data-ttu-id="fbb1c-220">Nakonec, protože skupiny Nsg výchozí pravidlo, které povoluje všechny přenosy mezi virtuálními počítači v hello stejné virtuální síti, pravidlo lze vytvořit pro hello back-end skupiny Nsg tooblock veškerý provoz.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-220">Finally, because NSGs have a default rule allowing all traffic between VMs in hello same VNet, a rule can be created for hello back-end NSGs tooblock all traffic.</span></span> <span data-ttu-id="fbb1c-221">Si zde všimnout, že hello `--priority` je zadána hodnota *300*, což je nižší, že oba hello pravidla NSG a MySQL.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-221">Notice here that hello `--priority` is given a value of *300*, which is lower that both hello NSG and MySQL rules.</span></span> <span data-ttu-id="fbb1c-222">Tato konfigurace zajistí, že provoz SSH a MySQL stále může prostřednictvím hello NSG.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-222">This configuration ensures that SSH and MySQL traffic is still allowed through hello NSG.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

<span data-ttu-id="fbb1c-223">Hello back-end virtuální počítač nyní pouze dostupný na portu *22* a port *3306* z podsítě front-endu hello.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-223">hello back-end VM is now only accessible on port *22* and port *3306* from hello front-end subnet.</span></span> <span data-ttu-id="fbb1c-224">Všechny ostatní příchozí přenosy je blokováno v hello skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-224">All other incoming traffic is blocked at hello network security group.</span></span> <span data-ttu-id="fbb1c-225">To může být užitečné toovisualize hello NSG pravidlo konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-225">It may be helpful toovisualize hello NSG rule configurations.</span></span> <span data-ttu-id="fbb1c-226">Konfigurace pravidla NSG návratový hello s hello [az sítě pravidlo seznamu](/cli/azure/network/nsg/rule#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-226">Return hello NSG rule configuration with hello [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGBackEnd --output table
```

<span data-ttu-id="fbb1c-227">Výstup:</span><span class="sxs-lookup"><span data-stu-id="fbb1c-227">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix    DestinationPortRange    Direction    Name       Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                           22                      Inbound      SSH             100  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Allow     *                           3306                    Inbound      MySQL           200  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Deny      *                           *                       Inbound      denyAll         300  Tcp         Succeeded            myRGNetwork      *                      *
```

## <a name="next-steps"></a><span data-ttu-id="fbb1c-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fbb1c-228">Next steps</span></span>

<span data-ttu-id="fbb1c-229">V tomto kurzu jste vytvořili a zabezpečené sítě Azure jako související toovirtual počítače.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-229">In this tutorial, you created and secured Azure networks as related toovirtual machines.</span></span> <span data-ttu-id="fbb1c-230">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="fbb1c-230">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fbb1c-231">Nasazení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="fbb1c-231">Deploy a virtual network</span></span>
> * <span data-ttu-id="fbb1c-232">Vytvořte podsíť virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-232">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="fbb1c-233">Připojte tooa podsíť virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fbb1c-233">Attach virtual machines tooa subnet</span></span>
> * <span data-ttu-id="fbb1c-234">Spravovat veřejné IP adresy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fbb1c-234">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="fbb1c-235">Zabezpečené příchozí přenosy z Internetu</span><span class="sxs-lookup"><span data-stu-id="fbb1c-235">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="fbb1c-236">Zabezpečení přenosu tooVM virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fbb1c-236">Secure VM tooVM traffic</span></span>

<span data-ttu-id="fbb1c-237">Posunutí další kurz toolearn toohello o zabezpečení dat na virtuální počítače pomocí zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="fbb1c-237">Advance toohello next tutorial toolearn about securing data on virtual machines using Azure backup.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="fbb1c-238">Zálohovat virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="fbb1c-238">Back up Linux virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
