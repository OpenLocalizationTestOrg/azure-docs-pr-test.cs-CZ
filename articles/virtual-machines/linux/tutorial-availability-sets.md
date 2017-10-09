---
title: "aaaAvailability nastaví kurz pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o hello skupiny dostupnosti pro virtuální počítače s Linuxem v Azure."
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
ms.openlocfilehash: 2a91e4a6057180035ec51410d9fffccaca343758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a><span data-ttu-id="c860f-103">Jak nastaví toouse dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c860f-103">How toouse availability sets</span></span>


<span data-ttu-id="c860f-104">V tomto kurzu se dozvíte, jak tooincrease hello dostupnost a spolehlivost řešení virtuálního počítače v Azure pomocí funkce názvem skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c860f-104">In this tutorial, you will learn how tooincrease hello availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="c860f-105">Skupiny dostupnosti Ujistěte se, že hello virtuálních počítačů nasadit v Azure jsou distribuovány na více clusterů izolované hardwaru.</span><span class="sxs-lookup"><span data-stu-id="c860f-105">Availability sets ensure that hello VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="c860f-106">To zajišťuje, že pokud dojde selhání hardwaru nebo softwaru v rámci Azure, bude mít vliv pouze dílčí sadu virtuálních počítačů a že celkového řešení zůstanou dostupné a funkční z hlediska hello zákazníků použití.</span><span class="sxs-lookup"><span data-stu-id="c860f-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from hello perspective of your customers using it.</span></span>

<span data-ttu-id="c860f-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="c860f-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c860f-108">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c860f-108">Create an availability set</span></span>
> * <span data-ttu-id="c860f-109">Vytvoření virtuálního počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c860f-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="c860f-110">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c860f-110">Check available VM sizes</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c860f-111">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c860f-111">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="c860f-112">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="c860f-112">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="c860f-113">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c860f-113">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="availability-set-overview"></a><span data-ttu-id="c860f-114">Přehled sady dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c860f-114">Availability set overview</span></span>

<span data-ttu-id="c860f-115">Skupiny dostupnosti je logické seskupení schopností, který můžete použít v Azure tooensure že hello prostředky virtuálních počítačů, které umístíte v něm jsou od sebe navzájem oddělené, když jsou nasazeny v rámci datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="c860f-115">An Availability Set is a logical grouping capability that you can use in Azure tooensure that hello VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="c860f-116">Azure zajistí, že hello virtuální počítače umístíte do skupiny dostupnosti spustit více fyzických serverů, výpočetní stojany, jednotky úložiště a síťové přepínače.</span><span class="sxs-lookup"><span data-stu-id="c860f-116">Azure ensures that hello VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="c860f-117">To zajistí, že hello události hardwaru nebo softwaru Azure chyby, bude mít vliv jenom podmnožinu virtuální počítače a aplikace celkový zůstane nahoru a bude pokračovat zákazníky k dispozici tooyour toobe.</span><span class="sxs-lookup"><span data-stu-id="c860f-117">This ensures that in hello event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue toobe available tooyour customers.</span></span> <span data-ttu-id="c860f-118">Pomocí nastavení dostupnosti je tooleverage nezbytné funkce, pokud chcete toobuild spolehlivé cloudové řešení.</span><span class="sxs-lookup"><span data-stu-id="c860f-118">Using Availability Sets is an essential capability tooleverage when you want toobuild reliable cloud solutions.</span></span>

<span data-ttu-id="c860f-119">Pojďme typické řešení virtuálních počítačů na základě kterých může mít 4 front-end webové servery a používat 2 back-end virtuální počítače, které jsou hostiteli databáze.</span><span class="sxs-lookup"><span data-stu-id="c860f-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="c860f-120">S Azure, je vhodnější toodefine dvě sady dostupnosti před nasazením virtuálních počítačů: sadu jeden dostupnosti pro vrstvu "web" hello a jednu sadu dostupnosti pro vrstvu "databáze" hello.</span><span class="sxs-lookup"><span data-stu-id="c860f-120">With Azure, you’d want toodefine two availability sets before you deploy your VMs: one availability set for hello “web” tier and one availability set for hello “database” tier.</span></span> <span data-ttu-id="c860f-121">Při vytváření nového virtuálního počítače můžete určit hello dostupnosti jako virtuální počítač parametr toohello az vytvoření příkazu a Azure automaticky zajistí, že hello virtuálních počítačů, které vytvoříte v rámci hello k dispozici sady jsou izolovány napříč několika prostředcích fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="c860f-121">When you create a new VM you can then specify hello availability set as a parameter toohello az vm create command, and Azure will automatically ensure that hello VMs you create within hello available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="c860f-122">To znamená, že pokud hello fyzický hardware, který webový Server nebo virtuální počítače databáze serveru běží na problém, víte, že hello ostatní instance webového serveru a databáze virtuální počítače zůstanou spuštěné bez problémů vzhledem k tomu, že jsou na jiný hardware.</span><span class="sxs-lookup"><span data-stu-id="c860f-122">This means that if hello physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that hello other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="c860f-123">Pokud chcete toodeploy spolehlivé virtuálních počítačů na základě řešení v rámci Azure byste měli vždycky používat skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c860f-123">You should always use Availability Sets when you want toodeploy reliable VM-based solutions within Azure.</span></span>


## <a name="create-an-availability-set"></a><span data-ttu-id="c860f-124">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c860f-124">Create an availability set</span></span>

<span data-ttu-id="c860f-125">Můžete vytvořit pomocí sadu dostupnosti [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="c860f-125">You can create an availability set using [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="c860f-126">V tomto příkladu jsme nastavit počet aktualizací a chyby domén v obou hello *2* pro sadu s názvem dostupnosti hello *myAvailabilitySet* v hello *myResourceGroupAvailability*skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c860f-126">In this example, we set both hello number of update and fault domains at *2* for hello availability set named *myAvailabilitySet* in hello *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="c860f-127">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c860f-127">Create a resource group.</span></span>

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

<span data-ttu-id="c860f-128">Skupiny dostupnosti povolit tooisolate prostředky napříč "domén selhání" a "update domény".</span><span class="sxs-lookup"><span data-stu-id="c860f-128">Availability Sets allow you tooisolate resources across "fault domains" and "update domains".</span></span> <span data-ttu-id="c860f-129">A **doména selhání** představuje kolekci izolované serveru + síť + úložiště prostředků.</span><span class="sxs-lookup"><span data-stu-id="c860f-129">A **fault domain** represents an isolated collection of server + network + storage resources.</span></span> <span data-ttu-id="c860f-130">V předchozím příkladu hello jsme znamenat, že má být, že naše dostupnosti nastaven toobe rozložené mezi aspoň dva domén selhání při nasazení naše virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c860f-130">In hello preceding example, we indicate that we want our availability set toobe distributed across at least two fault domains when our VMs are deployed.</span></span> <span data-ttu-id="c860f-131">Také znamenat, že chceme, aby naše dostupnosti distribuované napříč dvěma **aktualizovat domény**.</span><span class="sxs-lookup"><span data-stu-id="c860f-131">We also indicate that we want our availability set distributed across two **update domains**.</span></span>  <span data-ttu-id="c860f-132">Dvě domény aktualizace zajistěte, aby byly při Azure provádí aktualizace softwaru naše prostředky virtuálních počítačů izolované, brání veškerý software hello spuštění pod naše virtuálních počítačů z aktualizovaných v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="c860f-132">Two update domains ensure that when Azure performs software updates our VM resources are isolated, preventing all hello software running underneath our VM from being updated at hello same time.</span></span>

## <a name="configure-virtual-network"></a><span data-ttu-id="c860f-133">Konfigurace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="c860f-133">Configure virtual network</span></span>
<span data-ttu-id="c860f-134">Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte hello podpora prostředky virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c860f-134">Before you deploy some VMs and can test your balancer, create hello supporting virtual network resources.</span></span> <span data-ttu-id="c860f-135">Další informace o virtuálních sítích najdete v tématu hello [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="c860f-135">For more information about virtual networks, see hello [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="c860f-136">Vytvoření síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="c860f-136">Create network resources</span></span>
<span data-ttu-id="c860f-137">Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="c860f-137">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="c860f-138">Hello následující příklad vytvoří virtuální síť s názvem *myVnet* s podsítí s názvem *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="c860f-138">hello following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupAvailability \
    --name myVnet \
    --subnet-name mySubnet
```
<span data-ttu-id="c860f-139">Virtuální síťové adaptéry jsou vytvořeny pomocí [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="c860f-139">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="c860f-140">Hello následující příklad vytvoří tři virtuálních síťových karet.</span><span class="sxs-lookup"><span data-stu-id="c860f-140">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="c860f-141">(Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro aplikace v rámci hello následující kroky).</span><span class="sxs-lookup"><span data-stu-id="c860f-141">(One virtual NIC for each VM you create for your app in hello following steps).</span></span> <span data-ttu-id="c860f-142">Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a přidat je nástroj pro vyrovnávání zatížení toohello:</span><span class="sxs-lookup"><span data-stu-id="c860f-142">You can create additional virtual NICs and VMs at any time and add them toohello load balancer:</span></span>

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

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="c860f-143">Vytvořit virtuální počítače uvnitř skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c860f-143">Create VMs inside an availability set</span></span>

<span data-ttu-id="c860f-144">Virtuální počítače musí být vytvořen v rámci toomake sadu dostupnosti hello se, že jsou správně distribuovány mezi hello hardwaru.</span><span class="sxs-lookup"><span data-stu-id="c860f-144">VMs must be created within hello availability set toomake sure they are correctly distributed across hello hardware.</span></span> <span data-ttu-id="c860f-145">Nelze přidat sadu po vytvoření existující tooan dostupnosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c860f-145">You can't add an existing VM tooan availability set after it is created.</span></span> 

<span data-ttu-id="c860f-146">Při vytváření virtuálního počítače pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) zadejte hello dostupnosti pomocí hello `--availability-set` parametr toospecify hello název sady dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="c860f-146">When you create a VM using [az vm create](/cli/azure/vm#create) you specify hello availability set using hello `--availability-set` parameter toospecify hello name of hello availability set.</span></span>

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

<span data-ttu-id="c860f-147">Nyní je k dispozici dva virtuální počítače v rámci naší sady nově vytvořený dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c860f-147">We now have two virtual machines within our newly created availability set.</span></span> <span data-ttu-id="c860f-148">Protože jsou v hello stejné nastavení dostupnosti, Azure bude zajištěno, že hello virtuální počítače a všechny jejich prostředky (včetně datových disků) jsou rozmístěny v izolované fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="c860f-148">Because they are in hello same availability set, Azure will ensure that hello VMs and all their resources (including data disks) are distributed across isolated physical hardware.</span></span> <span data-ttu-id="c860f-149">Toto rozdělení pomáhá zajistit mnohem vyšší dostupnosti naše celkového řešení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c860f-149">This distribution helps ensure much higher availability of our overall VM solution.</span></span>

<span data-ttu-id="c860f-150">Jednou z věcí, které se můžete setkat při přidávání virtuálních počítačů je, že konkrétní velikost virtuálního počítače již není k dispozici toouse v rámci vaší sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c860f-150">One thing you may encounter as you add VMs is that a particular VM size is no longer available toouse within your availability set.</span></span> <span data-ttu-id="c860f-151">Tento problém může dojít, když už není dostatek tooadd kapacity, které při zachování sada dostupnosti hello izolace pravidla hello vynucuje.</span><span class="sxs-lookup"><span data-stu-id="c860f-151">This issue can happen if there is no longer enough capacity tooadd it while preserving hello isolation rules hello availability set enforces.</span></span> <span data-ttu-id="c860f-152">Můžete zkontrolovat toosee jaké velikosti virtuálních počítačů jsou k dispozici toouse v rámci existující dostupnosti nastavit pomocí hello `--availability-set list-sizes` parametr.</span><span class="sxs-lookup"><span data-stu-id="c860f-152">You can check toosee what VM sizes are available toouse within an existing availability set using hello `--availability-set list-sizes` parameter.</span></span>

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="c860f-153">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c860f-153">Check for available VM sizes</span></span> 

<span data-ttu-id="c860f-154">Můžete přidat další virtuální počítače toohello dostupnosti později, ale potřebujete tooknow jaké velikosti virtuálních počítačů jsou k dispozici na hello hardwaru.</span><span class="sxs-lookup"><span data-stu-id="c860f-154">You can add more VMs toohello availability set later, but you need tooknow what VM sizes are available on hello hardware.</span></span> <span data-ttu-id="c860f-155">Použití [seznam velikosti az virtuálních počítačů sady dostupnosti](/cli/azure/availability-set#list-sizes) toolist všech dostupných velikostí hello na hello hardwaru clusteru pro sadu dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="c860f-155">Use [az vm availability-set list-sizes](/cli/azure/availability-set#list-sizes) toolist all hello available sizes on hello hardware cluster for hello availability set.</span></span>

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a><span data-ttu-id="c860f-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c860f-156">Next steps</span></span>

<span data-ttu-id="c860f-157">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="c860f-157">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c860f-158">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c860f-158">Create an availability set</span></span>
> * <span data-ttu-id="c860f-159">Vytvoření virtuálního počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c860f-159">Create a VM in an availability set</span></span>
> * <span data-ttu-id="c860f-160">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c860f-160">Check available VM sizes</span></span>

<span data-ttu-id="c860f-161">Posunutí další kurz toolearn toohello o sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c860f-161">Advance toohello next tutorial toolearn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c860f-162">Vytvoření škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c860f-162">Create a VM scale set</span></span>](tutorial-create-vmss.md)

