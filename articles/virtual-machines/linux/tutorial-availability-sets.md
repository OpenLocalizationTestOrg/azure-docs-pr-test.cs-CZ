---
title: "Kurz sady dostupnosti pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o skupiny dostupnosti pro virtuální počítače s Linuxem v Azure."
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 63fe3f165864f06228604cac56d06cc061ab25f5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-availability-sets"></a><span data-ttu-id="eac06-103">Jak používat skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="eac06-103">How to use availability sets</span></span>


<span data-ttu-id="eac06-104">V tomto kurzu se dozvíte, jak zvýšit dostupnost a spolehlivost řešení virtuálního počítače v Azure pomocí funkce názvem skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eac06-104">In this tutorial, you will learn how to increase the availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="eac06-105">Skupiny dostupnosti Ujistěte se, že virtuálních počítačů nasadit v Azure jsou distribuovány na více clusterů izolované hardwaru.</span><span class="sxs-lookup"><span data-stu-id="eac06-105">Availability sets ensure that the VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="eac06-106">To zajišťuje, že pokud dojde selhání hardwaru nebo softwaru v rámci Azure, bude mít vliv pouze dílčí sadu virtuálních počítačů a že celkového řešení zůstanou dostupné a funkční z perspektivy zákazníky ho pomocí.</span><span class="sxs-lookup"><span data-stu-id="eac06-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from the perspective of your customers using it.</span></span>

<span data-ttu-id="eac06-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="eac06-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eac06-108">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="eac06-108">Create an availability set</span></span>
> * <span data-ttu-id="eac06-109">Vytvoření virtuálního počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="eac06-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="eac06-110">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="eac06-110">Check available VM sizes</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="eac06-111">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="eac06-111">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="eac06-112">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="eac06-112">Run `az --version` to find the version.</span></span> <span data-ttu-id="eac06-113">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="eac06-113">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="availability-set-overview"></a><span data-ttu-id="eac06-114">Přehled sady dostupnosti</span><span class="sxs-lookup"><span data-stu-id="eac06-114">Availability set overview</span></span>

<span data-ttu-id="eac06-115">Skupiny dostupnosti je logické seskupení funkci, která můžete použít v Azure k zajištění, že prostředky virtuálních počítačů, které umístíte v něm jsou od sebe navzájem oddělené, když jsou nasazeny v rámci datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="eac06-115">An Availability Set is a logical grouping capability that you can use in Azure to ensure that the VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="eac06-116">Azure zajistí, že virtuální počítače umístíte do skupiny dostupnosti spustit více fyzických serverů, výpočetní stojany, jednotky úložiště a síťové přepínače.</span><span class="sxs-lookup"><span data-stu-id="eac06-116">Azure ensures that the VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="eac06-117">To zajistí, že v případě hardwaru nebo softwaru Azure selhání, bude mít vliv jenom podmnožinu virtuální počítače a aplikace celkové zůstane nahoru a nadále k dispozici pro vaše zákazníky.</span><span class="sxs-lookup"><span data-stu-id="eac06-117">This ensures that in the event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue to be available to your customers.</span></span> <span data-ttu-id="eac06-118">Pomocí nastavení dostupnosti je nezbytné schopnost využít, pokud chcete vytvářet spolehlivé cloudové řešení.</span><span class="sxs-lookup"><span data-stu-id="eac06-118">Using Availability Sets is an essential capability to leverage when you want to build reliable cloud solutions.</span></span>

<span data-ttu-id="eac06-119">Pojďme typické řešení virtuálních počítačů na základě kterých může mít 4 front-end webové servery a používat 2 back-end virtuální počítače, které jsou hostiteli databáze.</span><span class="sxs-lookup"><span data-stu-id="eac06-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="eac06-120">S Azure, by chcete definovat dvě sady dostupnosti před nasazením virtuálních počítačů: jednu sadu dostupnosti pro vrstvu "web" a jednu sadu dostupnosti pro vrstvu "databáze".</span><span class="sxs-lookup"><span data-stu-id="eac06-120">With Azure, you’d want to define two availability sets before you deploy your VMs: one availability set for the “web” tier and one availability set for the “database” tier.</span></span> <span data-ttu-id="eac06-121">Při vytváření nového virtuálního počítače můžete určit skupiny dostupnosti jako parametr pro virtuální počítač az vytvoření příkazu a Azure automaticky zajistí, že virtuální počítače, vytvořte v rámci sady k dispozici jsou izolované napříč několika prostředcích fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="eac06-121">When you create a new VM you can then specify the availability set as a parameter to the az vm create command, and Azure will automatically ensure that the VMs you create within the available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="eac06-122">To znamená, že pokud fyzický hardware, který webový Server nebo virtuální počítače databáze serveru běží na problém, víte, že ostatní instance webového serveru a databáze virtuální počítače zůstanou spuštěné bez problémů vzhledem k tomu, že jsou na jiný hardware.</span><span class="sxs-lookup"><span data-stu-id="eac06-122">This means that if the physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that the other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="eac06-123">Pokud chcete nasadit spolehlivé řešení založená na virtuálních počítačů v rámci Azure byste měli vždycky používat skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eac06-123">You should always use Availability Sets when you want to deploy reliable VM-based solutions within Azure.</span></span>


## <a name="create-an-availability-set"></a><span data-ttu-id="eac06-124">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="eac06-124">Create an availability set</span></span>

<span data-ttu-id="eac06-125">Můžete vytvořit pomocí sadu dostupnosti [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="eac06-125">You can create an availability set using [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="eac06-126">V tomto příkladu jsme nastavit počet aktualizací a chyby domén v *2* pro skupinu dostupnosti s názvem *myAvailabilitySet* v *myResourceGroupAvailability* Skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="eac06-126">In this example, we set both the number of update and fault domains at *2* for the availability set named *myAvailabilitySet* in the *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="eac06-127">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="eac06-127">Create a resource group.</span></span>

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

<span data-ttu-id="eac06-128">Skupiny dostupnosti umožňují izolovat prostředky napříč "domén selhání" a "update domény".</span><span class="sxs-lookup"><span data-stu-id="eac06-128">Availability Sets allow you to isolate resources across "fault domains" and "update domains".</span></span> <span data-ttu-id="eac06-129">A **doména selhání** představuje kolekci izolované serveru + síť + úložiště prostředků.</span><span class="sxs-lookup"><span data-stu-id="eac06-129">A **fault domain** represents an isolated collection of server + network + storage resources.</span></span> <span data-ttu-id="eac06-130">V předchozím příkladu jsme znamenat, že chceme, aby naše dostupnosti být distribuován do alespoň dva domén selhání při nasazení naše virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="eac06-130">In the preceding example, we indicate that we want our availability set to be distributed across at least two fault domains when our VMs are deployed.</span></span> <span data-ttu-id="eac06-131">Také znamenat, že chceme, aby naše dostupnosti distribuované napříč dvěma **aktualizovat domény**.</span><span class="sxs-lookup"><span data-stu-id="eac06-131">We also indicate that we want our availability set distributed across two **update domains**.</span></span>  <span data-ttu-id="eac06-132">Dvě domény aktualizace zajistěte, aby při Azure provádí aktualizace softwaru naše prostředky virtuálních počítačů izolované, brání veškerý software, které jsou spuštěné pod naše virtuálních počítačů ze aktualizaci ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="eac06-132">Two update domains ensure that when Azure performs software updates our VM resources are isolated, preventing all the software running underneath our VM from being updated at the same time.</span></span>

## <a name="configure-virtual-network"></a><span data-ttu-id="eac06-133">Konfigurace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="eac06-133">Configure virtual network</span></span>
<span data-ttu-id="eac06-134">Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte doprovodné materiály virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="eac06-134">Before you deploy some VMs and can test your balancer, create the supporting virtual network resources.</span></span> <span data-ttu-id="eac06-135">Další informace o virtuálních sítích najdete v tématu [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="eac06-135">For more information about virtual networks, see the [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="eac06-136">Vytvoření síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="eac06-136">Create network resources</span></span>
<span data-ttu-id="eac06-137">Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="eac06-137">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="eac06-138">Následující příklad vytvoří virtuální síť s názvem *myVnet* s podsítí s názvem *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="eac06-138">The following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupAvailability \
    --name myVnet \
    --subnet-name mySubnet
```
<span data-ttu-id="eac06-139">Virtuální síťové adaptéry jsou vytvořeny pomocí [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="eac06-139">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="eac06-140">Následující příklad vytvoří tři virtuálních síťových karet.</span><span class="sxs-lookup"><span data-stu-id="eac06-140">The following example creates three virtual NICs.</span></span> <span data-ttu-id="eac06-141">(Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro vaši aplikaci v následujících krocích).</span><span class="sxs-lookup"><span data-stu-id="eac06-141">(One virtual NIC for each VM you create for your app in the following steps).</span></span> <span data-ttu-id="eac06-142">Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a jejich přidání do Vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="eac06-142">You can create additional virtual NICs and VMs at any time and add them to the load balancer:</span></span>

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupAvailability \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="eac06-143">Vytvořit virtuální počítače uvnitř skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="eac06-143">Create VMs inside an availability set</span></span>

<span data-ttu-id="eac06-144">Virtuální počítače musí být vytvořen v rámci skupinu dostupnosti a ujistěte se, že jsou správně rozmístěny v hardwaru.</span><span class="sxs-lookup"><span data-stu-id="eac06-144">VMs must be created within the availability set to make sure they are correctly distributed across the hardware.</span></span> <span data-ttu-id="eac06-145">Nelze přidat existující virtuální počítač po vytvoření sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eac06-145">You can't add an existing VM to an availability set after it is created.</span></span> 

<span data-ttu-id="eac06-146">Při vytváření virtuálního počítače pomocí [az virtuální počítač vytvořit](/cli/azure/vm#create) zadáte skupinu dostupnosti pomocí `--availability-set` parametr zadat název skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eac06-146">When you create a VM using [az vm create](/cli/azure/vm#create) you specify the availability set using the `--availability-set` parameter to specify the name of the availability set.</span></span>

```azurecli-interactive 
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --nics myNic$i \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

<span data-ttu-id="eac06-147">Nyní je k dispozici dva virtuální počítače v rámci naší sady nově vytvořený dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eac06-147">We now have two virtual machines within our newly created availability set.</span></span> <span data-ttu-id="eac06-148">Protože jsou ve stejné skupině dostupnosti, Azure zajistí, že virtuální počítače a jejich prostředky (včetně datových disků) jsou rozmístěny v izolované fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="eac06-148">Because they are in the same availability set, Azure will ensure that the VMs and all their resources (including data disks) are distributed across isolated physical hardware.</span></span> <span data-ttu-id="eac06-149">Toto rozdělení pomáhá zajistit mnohem vyšší dostupnosti naše celkového řešení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="eac06-149">This distribution helps ensure much higher availability of our overall VM solution.</span></span>

<span data-ttu-id="eac06-150">Jednou z věcí, které se můžete setkat při přidávání virtuálních počítačů je, že konkrétní velikost virtuálního počítače již není k dispozici pro použití v rámci vaší sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eac06-150">One thing you may encounter as you add VMs is that a particular VM size is no longer available to use within your availability set.</span></span> <span data-ttu-id="eac06-151">Tento problém může dojít, když už není dostatečnou kapacitu pro přidání při zachování pravidla izolace vynucuje skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eac06-151">This issue can happen if there is no longer enough capacity to add it while preserving the isolation rules the availability set enforces.</span></span> <span data-ttu-id="eac06-152">Můžete zkontrolovat, uvidíte, jaké velikosti virtuálních počítačů jsou k dispozici pro použití v rámci existující dostupnosti nastavit pomocí `--availability-set list-sizes` parametr.</span><span class="sxs-lookup"><span data-stu-id="eac06-152">You can check to see what VM sizes are available to use within an existing availability set using the `--availability-set list-sizes` parameter.</span></span>

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="eac06-153">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="eac06-153">Check for available VM sizes</span></span> 

<span data-ttu-id="eac06-154">Můžete přidat další virtuální počítače pro skupinu dostupnosti později, ale musíte vědět, jaké velikosti virtuálních počítačů jsou k dispozici na hardware.</span><span class="sxs-lookup"><span data-stu-id="eac06-154">You can add more VMs to the availability set later, but you need to know what VM sizes are available on the hardware.</span></span> <span data-ttu-id="eac06-155">Použití [seznam velikosti az virtuálních počítačů sady dostupnosti](/cli/azure/availability-set#list-sizes) seznam všech dostupných velikostí na hardwaru clusteru pro skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="eac06-155">Use [az vm availability-set list-sizes](/cli/azure/availability-set#list-sizes) to list all the available sizes on the hardware cluster for the availability set.</span></span>

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a><span data-ttu-id="eac06-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eac06-156">Next steps</span></span>

<span data-ttu-id="eac06-157">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="eac06-157">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eac06-158">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="eac06-158">Create an availability set</span></span>
> * <span data-ttu-id="eac06-159">Vytvoření virtuálního počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="eac06-159">Create a VM in an availability set</span></span>
> * <span data-ttu-id="eac06-160">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="eac06-160">Check available VM sizes</span></span>

<span data-ttu-id="eac06-161">Přechodu na v dalším kurzu se dozvíte o sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="eac06-161">Advance to the next tutorial to learn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="eac06-162">Vytvoření škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="eac06-162">Create a VM scale set</span></span>](tutorial-create-vmss.md)

