---
title: "Virtuální sítě Azure a virtuální počítače s Linuxem | Microsoft Docs"
description: "Kurz – Správa virtuálních sítí Azure a virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI"
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
ms.openlocfilehash: 2366905b8160675f77cbc41ba97540af70be8c01
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a><span data-ttu-id="a89f0-103">Správa virtuálních sítí Azure a virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a89f0-103">Manage Azure Virtual Networks and Linux Virtual Machines with the Azure CLI</span></span>

<span data-ttu-id="a89f0-104">Virtuální počítače Azure pomocí Azure sítě pro vnitřní a vnější síťovou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="a89f0-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="a89f0-105">Tento kurz vás provede nasazením dva virtuální počítače a konfigurace Azure sítě pro tyto virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="a89f0-105">This tutorial walks through deploying two virtual machines and configuring Azure networking for these VMs.</span></span> <span data-ttu-id="a89f0-106">V příkladech v tomto kurzu předpokládá, že virtuální počítače hostují webové aplikace s databáze back-end, ale není v tomto kurzu nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="a89f0-106">The examples in this tutorial assume that the VMs are hosting a web application with a database back-end, however an application is not deployed in the tutorial.</span></span> <span data-ttu-id="a89f0-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="a89f0-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a89f0-108">Nasazení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="a89f0-108">Deploy a virtual network</span></span>
> * <span data-ttu-id="a89f0-109">Vytvořte podsíť virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a89f0-109">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="a89f0-110">Připojení k podsíti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a89f0-110">Attach virtual machines to a subnet</span></span>
> * <span data-ttu-id="a89f0-111">Spravovat veřejné IP adresy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a89f0-111">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="a89f0-112">Zabezpečené příchozí přenosy z Internetu</span><span class="sxs-lookup"><span data-stu-id="a89f0-112">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="a89f0-113">Zabezpečený virtuální počítač k přenosy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a89f0-113">Secure VM to VM traffic</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a89f0-114">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a89f0-114">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="a89f0-115">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="a89f0-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="a89f0-116">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a89f0-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="vm-networking-overview"></a><span data-ttu-id="a89f0-117">Přehled sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a89f0-117">VM networking overview</span></span>

<span data-ttu-id="a89f0-118">Virtuální sítě Azure povolit zabezpečené sítě připojení mezi virtuálními počítači, internet a jinými službami Azure, jako je například Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="a89f0-118">Azure virtual networks enable secure network connections between virtual machines, the internet, and other Azure services such as Azure SQL database.</span></span> <span data-ttu-id="a89f0-119">Virtuální sítě jsou rozdělit do logických segmenty označují jako podsítě.</span><span class="sxs-lookup"><span data-stu-id="a89f0-119">Virtual networks are broken down into logical segments called subnets.</span></span> <span data-ttu-id="a89f0-120">Podsítě se používají k řízení toku sítě a jako hranice zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="a89f0-120">Subnets are used to control network flow, and as a security boundary.</span></span> <span data-ttu-id="a89f0-121">Při nasazování virtuálního počítače, obvykle zahrnuje rozhraní virtuální sítě, který je připojen k podsíti.</span><span class="sxs-lookup"><span data-stu-id="a89f0-121">When deploying a VM, it generally includes a virtual network interface, which is attached to a subnet.</span></span>

## <a name="deploy-virtual-network"></a><span data-ttu-id="a89f0-122">Nasazení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="a89f0-122">Deploy virtual network</span></span>

<span data-ttu-id="a89f0-123">V tomto kurzu se vytvoří jedné virtuální sítě se dvěma podsítěmi.</span><span class="sxs-lookup"><span data-stu-id="a89f0-123">For this tutorial, a single virtual network is created with two subnets.</span></span> <span data-ttu-id="a89f0-124">Podsíť front-end pro hostování webové aplikace a podsíť back-end pro hostování databázový server.</span><span class="sxs-lookup"><span data-stu-id="a89f0-124">A front-end subnet for hosting a web application, and a back-end subnet for hosting a database server.</span></span>

<span data-ttu-id="a89f0-125">Než bude možné vytvořit virtuální síť, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="a89f0-125">Before you can create a virtual network, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="a89f0-126">Následující příklad vytvoří skupinu prostředků s názvem *myRGNetwork* v eastus umístění.</span><span class="sxs-lookup"><span data-stu-id="a89f0-126">The following example creates a resource group named *myRGNetwork* in the eastus location.</span></span>

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a><span data-ttu-id="a89f0-127">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="a89f0-127">Create virtual network</span></span>

<span data-ttu-id="a89f0-128">Nám [vytvoření sítě vnet az](/cli/azure/network/vnet#create) příkaz pro vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a89f0-128">Us the [az network vnet create](/cli/azure/network/vnet#create) command to create a virtual network.</span></span> <span data-ttu-id="a89f0-129">V tomto příkladu je název sítě *mvVnet* a předponu adresy z *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="a89f0-129">In this example, the network is named *mvVnet* and is given an address prefix of *10.0.0.0/16*.</span></span> <span data-ttu-id="a89f0-130">Podsíť je vytvořen také s názvem *mySubnetFrontEnd* a předponu *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="a89f0-130">A subnet is also created with a name of *mySubnetFrontEnd* and a prefix of *10.0.1.0/24*.</span></span> <span data-ttu-id="a89f0-131">Později v tomto kurzu je front-end virtuálního počítače připojené k této podsíti.</span><span class="sxs-lookup"><span data-stu-id="a89f0-131">Later in this tutorial a front-end VM is connected to this subnet.</span></span> 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a><span data-ttu-id="a89f0-132">Vytvoření podsítě</span><span class="sxs-lookup"><span data-stu-id="a89f0-132">Create subnet</span></span>

<span data-ttu-id="a89f0-133">Novou podsíť je přidaný do virtuální sítě pomocí [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a89f0-133">A new subnet is added to the virtual network using the [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) command.</span></span> <span data-ttu-id="a89f0-134">V tomto příkladu je název podsítě *mySubnetBackEnd* a předponu adresy z *10.0.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="a89f0-134">In this example, the subnet is named *mySubnetBackEnd* and is given an address prefix of *10.0.2.0/24*.</span></span> <span data-ttu-id="a89f0-135">Tato podsíť se používá u všech služeb back-end.</span><span class="sxs-lookup"><span data-stu-id="a89f0-135">This subnet is used with all back-end services.</span></span>

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

<span data-ttu-id="a89f0-136">V tomto okamžiku síť byla vytvořil a rozdělena na dvě podsítě, jeden pro front-endové služby a jinou pro back endové služby.</span><span class="sxs-lookup"><span data-stu-id="a89f0-136">At this point, a network has been created and segmented into two subnets, one for front-end services, and another for back-end services.</span></span> <span data-ttu-id="a89f0-137">V další části jsou virtuální počítače vytvořené a připojené k tyto podsítě.</span><span class="sxs-lookup"><span data-stu-id="a89f0-137">In the next section, virtual machines are created and connected to these subnets.</span></span>

## <a name="understand-public-ip-address"></a><span data-ttu-id="a89f0-138">Pochopení veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="a89f0-138">Understand public IP address</span></span>

<span data-ttu-id="a89f0-139">Veřejná IP adresa umožňuje prostředků Azure bude přístupný na Internetu.</span><span class="sxs-lookup"><span data-stu-id="a89f0-139">A public IP address allows Azure resources to be accessible on the internet.</span></span> <span data-ttu-id="a89f0-140">V této části kurzu je virtuální počítač vytvořený na ukazují, jak pracovat s veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a89f0-140">In this section of the tutorial, a VM is created to demonstrate how to work with public IP addresses.</span></span>

### <a name="allocation-method"></a><span data-ttu-id="a89f0-141">Metoda přidělování</span><span class="sxs-lookup"><span data-stu-id="a89f0-141">Allocation method</span></span>

<span data-ttu-id="a89f0-142">Veřejné IP adresy mohou být přiděleny jako dynamická nebo statická.</span><span class="sxs-lookup"><span data-stu-id="a89f0-142">A public IP address can be allocated as either dynamic or static.</span></span> <span data-ttu-id="a89f0-143">Ve výchozím nastavení veřejnou IP adresu přidělí dynamicky.</span><span class="sxs-lookup"><span data-stu-id="a89f0-143">By default, public IP address dynamically allocated.</span></span> <span data-ttu-id="a89f0-144">Dynamické IP adresy vydávají, když virtuální počítač je navrácena.</span><span class="sxs-lookup"><span data-stu-id="a89f0-144">Dynamic IP addresses are released when a VM is deallocated.</span></span> <span data-ttu-id="a89f0-145">To způsobí, že IP adresa, chcete-li změnit během všechny operace, která zahrnuje navrácení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a89f0-145">This behavior causes the IP address to change during any operation that includes a VM deallocation.</span></span>

<span data-ttu-id="a89f0-146">Metoda přidělení lze nastavit na static, který zajišťuje, že IP adresa zůstanou zařazené do virtuálního počítače, i během deallocated stavu.</span><span class="sxs-lookup"><span data-stu-id="a89f0-146">The allocation method can be set to static, which ensures that the IP address remain assigned to a VM, even during a deallocated state.</span></span> <span data-ttu-id="a89f0-147">Pokud používáte staticky přidělená adresa IP, nelze zadat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a89f0-147">When using a statically allocated IP address, the IP address itself cannot be specified.</span></span> <span data-ttu-id="a89f0-148">Místo toho je přidělen z fondu adres k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a89f0-148">Instead, it is allocated from a pool of available addresses.</span></span>

### <a name="dynamic-allocation"></a><span data-ttu-id="a89f0-149">Dynamické přidělení</span><span class="sxs-lookup"><span data-stu-id="a89f0-149">Dynamic allocation</span></span>

<span data-ttu-id="a89f0-150">Při vytváření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz, je dynamický metodu přidělení výchozí veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a89f0-150">When creating a VM with the [az vm create](/cli/azure/vm#create) command, the default public IP address allocation method is dynamic.</span></span> <span data-ttu-id="a89f0-151">V následujícím příkladu se vytvoří virtuální počítač s dynamickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a89f0-151">In the following example, a VM is created with a dynamic IP address.</span></span> 

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

### <a name="static-allocation"></a><span data-ttu-id="a89f0-152">Statického přidělování</span><span class="sxs-lookup"><span data-stu-id="a89f0-152">Static allocation</span></span>

<span data-ttu-id="a89f0-153">Při vytváření virtuálního počítače pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz, zahrnout `--public-ip-address-allocation static` argument přiřadit statickou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a89f0-153">When creating a virtual machine using the [az vm create](/cli/azure/vm#create) command, include the `--public-ip-address-allocation static` argument to assign a static public IP address.</span></span> <span data-ttu-id="a89f0-154">Tato operace není ukázáno v tomto kurzu, ale v další části se změní dynamicky přidělené IP adresy na staticky přidělené adresy.</span><span class="sxs-lookup"><span data-stu-id="a89f0-154">This operation is not demonstrated in this tutorial, however in the next section a dynamically allocated IP address is changed to a statically allocated address.</span></span> 

### <a name="change-allocation-method"></a><span data-ttu-id="a89f0-155">Změnit přidělení – metoda</span><span class="sxs-lookup"><span data-stu-id="a89f0-155">Change allocation method</span></span>

<span data-ttu-id="a89f0-156">Metoda přidělení IP adres se dá změnit pomocí [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a89f0-156">The IP address allocation method can be changed using the [az network public-ip update](/cli/azure/network/public-ip#update) command.</span></span> <span data-ttu-id="a89f0-157">V tomto příkladu metoda přidělení IP adresy front-endu virtuálního počítače se změní na statické.</span><span class="sxs-lookup"><span data-stu-id="a89f0-157">In this example, the IP address allocation method of the front-end VM is changed to static.</span></span>

<span data-ttu-id="a89f0-158">Nejprve zrušit přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a89f0-158">First, deallocate the VM.</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

<span data-ttu-id="a89f0-159">Použití [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip#update) příkaz k aktualizaci metoda přidělení.</span><span class="sxs-lookup"><span data-stu-id="a89f0-159">Use the [az network public-ip update](/cli/azure/network/public-ip#update) command to update the allocation method.</span></span> <span data-ttu-id="a89f0-160">V takovém případě `--allocation-method` je nastavena na *statické*.</span><span class="sxs-lookup"><span data-stu-id="a89f0-160">In this case, the `--allocation-method` is being set to *static*.</span></span>

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

<span data-ttu-id="a89f0-161">Spusťte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a89f0-161">Start the VM.</span></span>

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a><span data-ttu-id="a89f0-162">Žádné veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="a89f0-162">No public IP address</span></span>

<span data-ttu-id="a89f0-163">Virtuální počítač se často, nemusí být přístupné přes internet.</span><span class="sxs-lookup"><span data-stu-id="a89f0-163">Often, a VM does not need to be accessible over the internet.</span></span> <span data-ttu-id="a89f0-164">Chcete-li vytvořit virtuální počítač bez veřejnou IP adresu, použijte `--public-ip-address ""` argument s prázdnou sadu dvojité uvozovky.</span><span class="sxs-lookup"><span data-stu-id="a89f0-164">To create a VM without a public IP address, use the `--public-ip-address ""` argument with an empty set of double quotes.</span></span> <span data-ttu-id="a89f0-165">Tato konfigurace je ukázán později v tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="a89f0-165">This configuration is demonstrated later in this tutorial</span></span>

## <a name="secure-network-traffic"></a><span data-ttu-id="a89f0-166">Zabezpečení provozu sítě</span><span class="sxs-lookup"><span data-stu-id="a89f0-166">Secure network traffic</span></span>

<span data-ttu-id="a89f0-167">Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, která prostředkům připojeným k virtuálním sítím Azure povolují nebo odpírají síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="a89f0-167">A network security group (NSG) contains a list of security rules that allow or deny network traffic to resources connected to Azure Virtual Networks (VNet).</span></span> <span data-ttu-id="a89f0-168">Skupiny Nsg můžou být přidružena k podsítě nebo jednotlivých síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a89f0-168">NSGs can be associated to subnets or individual network interfaces.</span></span> <span data-ttu-id="a89f0-169">Když skupinu NSG je spojen s síťovým rozhraním, bude se vztahovat jenom přidružený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a89f0-169">When an NSG is associated with a network interface, it applies only the associated VM.</span></span> <span data-ttu-id="a89f0-170">Pokud je skupina zabezpečení sítě přidružená k podsíti, pravidla se vztahují na všechny prostředky, které jsou připojené k příslušné podsíti.</span><span class="sxs-lookup"><span data-stu-id="a89f0-170">When an NSG is associated to a subnet, the rules apply to all resources connected to the subnet.</span></span> 

### <a name="network-security-group-rules"></a><span data-ttu-id="a89f0-171">Pravidla skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="a89f0-171">Network security group rules</span></span>

<span data-ttu-id="a89f0-172">Pravidla NSG definovat síťové porty, přes které provoz povolený nebo zakázaný.</span><span class="sxs-lookup"><span data-stu-id="a89f0-172">NSG rules define networking ports over which traffic is allowed or denied.</span></span> <span data-ttu-id="a89f0-173">Pravidla mohou obsahovat zdrojové a cílové rozsahy IP adres, tak, aby je řízen provoz mezi konkrétní systémy nebo podsítě.</span><span class="sxs-lookup"><span data-stu-id="a89f0-173">The rules can include source and destination IP address ranges so that traffic is controlled between specific systems or subnets.</span></span> <span data-ttu-id="a89f0-174">Pravidla NSG také zahrnovat prioritu (mezi 1 – a 4096).</span><span class="sxs-lookup"><span data-stu-id="a89f0-174">NSG rules also include a priority (between 1—and 4096).</span></span> <span data-ttu-id="a89f0-175">Pravidla jsou vyhodnocovány v pořadí podle priority.</span><span class="sxs-lookup"><span data-stu-id="a89f0-175">Rules are evaluated in the order of priority.</span></span> <span data-ttu-id="a89f0-176">Pravidlo s prioritou 100 vyhodnotí před pravidlo s prioritou 200.</span><span class="sxs-lookup"><span data-stu-id="a89f0-176">A rule with a priority of 100 is evaluated before a rule with priority 200.</span></span>

<span data-ttu-id="a89f0-177">Všechny skupiny NSG obsahují sadu výchozích pravidel.</span><span class="sxs-lookup"><span data-stu-id="a89f0-177">All NSGs contain a set of default rules.</span></span> <span data-ttu-id="a89f0-178">Výchozí pravidla se nedají odstranit, ale protože je jim přiřazená nejnižší priorita, dají se přepsat pravidly, která vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="a89f0-178">The default rules cannot be deleted, but because they are assigned the lowest priority, they can be overridden by the rules that you create.</span></span>

- <span data-ttu-id="a89f0-179">**Virtuální síť** – provoz pocházející a ukončování ve virtuální síti je povolena v příchozí a odchozí.</span><span class="sxs-lookup"><span data-stu-id="a89f0-179">**Virtual network** - Traffic originating and ending in a virtual network is allowed both in inbound and outbound directions.</span></span>
- <span data-ttu-id="a89f0-180">**Internet** – odchozí provoz je povolený, ale jsou blokovány příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="a89f0-180">**Internet** - Outbound traffic is allowed, but inbound traffic is blocked.</span></span>
- <span data-ttu-id="a89f0-181">**Nástroj pro vyrovnávání zatížení** – nástroj pro vyrovnávání zatížení povolit Azure testovat stav virtuálních počítačů a instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="a89f0-181">**Load balancer** - Allow Azure’s load balancer to probe the health of your VMs and role instances.</span></span> <span data-ttu-id="a89f0-182">Pokud nepoužíváte skupinu s vyrovnáváním zatížení, můžete přepsat toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="a89f0-182">If you are not using a load balanced set, you can override this rule.</span></span>

### <a name="create-network-security-groups"></a><span data-ttu-id="a89f0-183">Vytvoření skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="a89f0-183">Create network security groups</span></span>

<span data-ttu-id="a89f0-184">Lze vytvořit skupinu zabezpečení sítě ve stejnou dobu jako virtuální počítač pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a89f0-184">A network security group can be created at the same time as a VM using the [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="a89f0-185">Když to uděláte, se NSG k rozhraní sítě virtuálních počítačů a automaticky vytvořit, aby povolovala přenosy na portu je pravidlo NSG *22* z jakéhokoli zdroje.</span><span class="sxs-lookup"><span data-stu-id="a89f0-185">When doing so, the NSG is associated with the VMs network interface and an NSG rule is auto created to allow traffic on port *22* from any source.</span></span> <span data-ttu-id="a89f0-186">V tomto kurzu front-end NSG se automaticky vytvořené s front-endu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a89f0-186">Earlier in this tutorial, the front-end NSG was auto-created with the front-end VM.</span></span> <span data-ttu-id="a89f0-187">Pravidlo NSG se také automaticky vytvořenou pro port 22.</span><span class="sxs-lookup"><span data-stu-id="a89f0-187">An NSG rule was also auto created for port 22.</span></span> 

<span data-ttu-id="a89f0-188">V některých případech může být užitečné předem vytvořit skupinu NSG, například když by neměly být vytvářeny výchozí pravidla pro SSH, nebo když by mělo být připojeno NSG k podsíti.</span><span class="sxs-lookup"><span data-stu-id="a89f0-188">In some cases, it may be helpful to pre-create an NSG, such as when default SSH rules should not be created, or when the NSG should be attached to a subnet.</span></span> 

<span data-ttu-id="a89f0-189">Použití [vytvořit az sítě nsg](/cli/azure/network/nsg#create) příkazu vytvořte skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a89f0-189">Use the [az network nsg create](/cli/azure/network/nsg#create) command to create a network security group.</span></span>

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

<span data-ttu-id="a89f0-190">Místo přidružení skupiny NSG k síťové rozhraní, je přidružená k podsíti.</span><span class="sxs-lookup"><span data-stu-id="a89f0-190">Instead of associating the NSG to a network interface, it is associated with a subnet.</span></span> <span data-ttu-id="a89f0-191">V této konfiguraci dědí žádné virtuální počítače, který je připojen k podsíti pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="a89f0-191">In this configuration, any VM that is attached to the subnet inherits the NSG rules.</span></span>

<span data-ttu-id="a89f0-192">Aktualizovat existující podsíť s názvem *mySubnetBackEnd* se nová skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="a89f0-192">Update the existing subnet named *mySubnetBackEnd* with the new NSG.</span></span>

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

<span data-ttu-id="a89f0-193">Teď vytvořte virtuální počítač, který je připojen k *mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="a89f0-193">Now create a virtual machine, which is attached to the *mySubnetBackEnd*.</span></span> <span data-ttu-id="a89f0-194">Všimněte si, že `--nsg` argument má hodnotu prázdný dvojité uvozovky.</span><span class="sxs-lookup"><span data-stu-id="a89f0-194">Notice that the `--nsg` argument has a value of empty double quotes.</span></span> <span data-ttu-id="a89f0-195">Skupina NSG nemusí být vytvořen s virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="a89f0-195">An NSG does not need to be created with the VM.</span></span> <span data-ttu-id="a89f0-196">Virtuální počítač je připojený k podsíti back-end, který je chráněn s předem vytvořené NSG back-end.</span><span class="sxs-lookup"><span data-stu-id="a89f0-196">The VM is attached to the back-end subnet, which is protected with the pre-created back-end NSG.</span></span> <span data-ttu-id="a89f0-197">Tato skupina NSG se vztahuje na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a89f0-197">This NSG applies to the VM.</span></span> <span data-ttu-id="a89f0-198">Také si zde všimnout, který `--public-ip-address` argument má hodnotu prázdný dvojité uvozovky.</span><span class="sxs-lookup"><span data-stu-id="a89f0-198">Also, notice here that the `--public-ip-address` argument has a value of empty double quotes.</span></span> <span data-ttu-id="a89f0-199">Tato konfigurace vytvoří virtuální počítač bez veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a89f0-199">This configuration creates a VM without a public IP address.</span></span> 

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

### <a name="secure-incoming-traffic"></a><span data-ttu-id="a89f0-200">Zabezpečené příchozí provoz</span><span class="sxs-lookup"><span data-stu-id="a89f0-200">Secure incoming traffic</span></span>

<span data-ttu-id="a89f0-201">V okamžiku vytvoření front-endu virtuálního počítače bylo pravidlo NSG vytvořeno povolit příchozí přenosy na portu 22.</span><span class="sxs-lookup"><span data-stu-id="a89f0-201">When the front-end VM was created, an NSG rule was created to allow incoming traffic on port 22.</span></span> <span data-ttu-id="a89f0-202">Toto pravidlo umožňuje připojení SSH pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a89f0-202">This rule allows SSH connections to the VM.</span></span> <span data-ttu-id="a89f0-203">V tomto příkladu má být povoleno přenosy na portu *80*.</span><span class="sxs-lookup"><span data-stu-id="a89f0-203">For this example, traffic should also be allowed on port *80*.</span></span> <span data-ttu-id="a89f0-204">Tato konfigurace umožňuje webovou aplikaci nelze přistupovat ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="a89f0-204">This configuration allows a web application to be accessed on the VM.</span></span>

<span data-ttu-id="a89f0-205">Použití [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz k vytvoření pravidla pro port *80*.</span><span class="sxs-lookup"><span data-stu-id="a89f0-205">Use the [az network nsg rule create](/cli/azure/network/nsg/rule#create) command to create a rule for port *80*.</span></span>

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

<span data-ttu-id="a89f0-206">Front-endu virtuální počítač je nyní pouze přístupné na portu *22* a port *80*.</span><span class="sxs-lookup"><span data-stu-id="a89f0-206">The front-end VM is now only accessible on port *22* and port *80*.</span></span> <span data-ttu-id="a89f0-207">Všechny ostatní příchozí přenosy je blokováno v skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a89f0-207">All other incoming traffic is blocked at the network security group.</span></span> <span data-ttu-id="a89f0-208">Může být užitečné k vizualizaci konfigurace pravidel NSG.</span><span class="sxs-lookup"><span data-stu-id="a89f0-208">It may be helpful to visualize the NSG rule configurations.</span></span> <span data-ttu-id="a89f0-209">Konfigurace pravidla NSG se vraťte [az sítě pravidlo seznamu](/cli/azure/network/nsg/rule#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a89f0-209">Return the NSG rule configuration with the [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGFrontEnd --output table
```

<span data-ttu-id="a89f0-210">Výstup:</span><span class="sxs-lookup"><span data-stu-id="a89f0-210">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix      DestinationPortRange  Direction    Name                 Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -----------------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                                               22  Inbound      default-allow-ssh        1000  Tcp         Succeeded            myRGNetwork      *                      *
Allow     *                                               80  Inbound      http                      200  Tcp         Succeeded            myRGNetwork      *                      *
```

### <a name="secure-vm-to-vm-traffic"></a><span data-ttu-id="a89f0-211">Zabezpečený virtuální počítač k přenosy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a89f0-211">Secure VM to VM traffic</span></span>

<span data-ttu-id="a89f0-212">Pravidla skupiny zabezpečení sítě můžete také použít mezi virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="a89f0-212">Network security group rules can also apply between VMs.</span></span> <span data-ttu-id="a89f0-213">V tomto příkladu musí front-end virtuálních počítačů komunikovat s back-end virtuálního počítače na portu *22* a *3306*.</span><span class="sxs-lookup"><span data-stu-id="a89f0-213">For this example, the front-end VM needs to communicate with the back-end VM on port *22* and *3306*.</span></span> <span data-ttu-id="a89f0-214">Tato konfigurace umožňuje připojení SSH z front-endu virtuálního počítače a také povolit aplikace front-endu virtuálního počítače ke komunikaci s databází MySQL back-end.</span><span class="sxs-lookup"><span data-stu-id="a89f0-214">This configuration allows SSH connections from the front-end VM, and also allow an application on the front-end VM to communicate with a back-end MySQL database.</span></span> <span data-ttu-id="a89f0-215">Všechny ostatní přenosy by se zablokovat mezi virtuálními počítači front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="a89f0-215">All other traffic should be blocked between the front-end and back-end virtual machines.</span></span>

<span data-ttu-id="a89f0-216">Použití [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz k vytvoření pravidla pro port 22.</span><span class="sxs-lookup"><span data-stu-id="a89f0-216">Use the [az network nsg rule create](/cli/azure/network/nsg/rule#create) command to create a rule for port 22.</span></span> <span data-ttu-id="a89f0-217">Všimněte si, že `--source-address-prefix` argument určuje hodnotu *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="a89f0-217">Notice that the `--source-address-prefix` argument specifies a value of *10.0.1.0/24*.</span></span> <span data-ttu-id="a89f0-218">Tato konfigurace zajistí, že jsou povoleny pouze přenosy z podsítě front-endu prostřednictvím NSG.</span><span class="sxs-lookup"><span data-stu-id="a89f0-218">This configuration ensures that only traffic from the front-end subnet is allowed through the NSG.</span></span>

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

<span data-ttu-id="a89f0-219">Teď můžete přidáte pravidlo pro provoz MySQL na portu 3306.</span><span class="sxs-lookup"><span data-stu-id="a89f0-219">Now add a rule for MySQL traffic on port 3306.</span></span>

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

<span data-ttu-id="a89f0-220">Navíc vzhledem k tomu, že skupiny Nsg výchozí pravidlo povolující veškerý provoz mezi virtuálními počítači ve stejné virtuální síti, lze vytvořit pravidlo pro skupiny Nsg back-end zablokovat veškerý provoz.</span><span class="sxs-lookup"><span data-stu-id="a89f0-220">Finally, because NSGs have a default rule allowing all traffic between VMs in the same VNet, a rule can be created for the back-end NSGs to block all traffic.</span></span> <span data-ttu-id="a89f0-221">Všimněte si zde, že `--priority` je zadána hodnota *300*, která je nižší tohoto pravidla NSG i MySQL.</span><span class="sxs-lookup"><span data-stu-id="a89f0-221">Notice here that the `--priority` is given a value of *300*, which is lower that both the NSG and MySQL rules.</span></span> <span data-ttu-id="a89f0-222">Tato konfigurace zajistí, že provoz SSH a MySQL stále může prostřednictvím NSG.</span><span class="sxs-lookup"><span data-stu-id="a89f0-222">This configuration ensures that SSH and MySQL traffic is still allowed through the NSG.</span></span>

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

<span data-ttu-id="a89f0-223">Je back-end virtuální počítač nyní pouze dostupný na portu *22* a port *3306* z podsítě front-endu.</span><span class="sxs-lookup"><span data-stu-id="a89f0-223">The back-end VM is now only accessible on port *22* and port *3306* from the front-end subnet.</span></span> <span data-ttu-id="a89f0-224">Všechny ostatní příchozí přenosy je blokováno v skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a89f0-224">All other incoming traffic is blocked at the network security group.</span></span> <span data-ttu-id="a89f0-225">Může být užitečné k vizualizaci konfigurace pravidel NSG.</span><span class="sxs-lookup"><span data-stu-id="a89f0-225">It may be helpful to visualize the NSG rule configurations.</span></span> <span data-ttu-id="a89f0-226">Konfigurace pravidla NSG se vraťte [az sítě pravidlo seznamu](/cli/azure/network/nsg/rule#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a89f0-226">Return the NSG rule configuration with the [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGBackEnd --output table
```

<span data-ttu-id="a89f0-227">Výstup:</span><span class="sxs-lookup"><span data-stu-id="a89f0-227">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix    DestinationPortRange    Direction    Name       Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                           22                      Inbound      SSH             100  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Allow     *                           3306                    Inbound      MySQL           200  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Deny      *                           *                       Inbound      denyAll         300  Tcp         Succeeded            myRGNetwork      *                      *
```

## <a name="next-steps"></a><span data-ttu-id="a89f0-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a89f0-228">Next steps</span></span>

<span data-ttu-id="a89f0-229">V tomto kurzu jste vytvořili a zabezpečené sítě v souvislosti s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="a89f0-229">In this tutorial, you created and secured Azure networks as related to virtual machines.</span></span> <span data-ttu-id="a89f0-230">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="a89f0-230">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a89f0-231">Nasazení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="a89f0-231">Deploy a virtual network</span></span>
> * <span data-ttu-id="a89f0-232">Vytvořte podsíť virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a89f0-232">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="a89f0-233">Připojení k podsíti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a89f0-233">Attach virtual machines to a subnet</span></span>
> * <span data-ttu-id="a89f0-234">Spravovat veřejné IP adresy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a89f0-234">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="a89f0-235">Zabezpečené příchozí přenosy z Internetu</span><span class="sxs-lookup"><span data-stu-id="a89f0-235">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="a89f0-236">Zabezpečený virtuální počítač k přenosy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a89f0-236">Secure VM to VM traffic</span></span>

<span data-ttu-id="a89f0-237">Přechodu na v dalším kurzu se dozvíte o zabezpečení dat na virtuální počítače pomocí zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="a89f0-237">Advance to the next tutorial to learn about securing data on virtual machines using Azure backup.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="a89f0-238">Zálohovat virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="a89f0-238">Back up Linux virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
