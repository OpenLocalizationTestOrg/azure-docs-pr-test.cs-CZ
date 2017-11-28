---
title: "aaaCreate a spravovat virtuální počítače Linux s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – vytvořit a spravovat virtuální počítače Linux s hello rozhraní příkazového řádku Azure"
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
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 05f7c1cf860f809bc13f110778d3bddd619ac6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-linux-vms-with-hello-azure-cli"></a><span data-ttu-id="0e1be-103">Vytvořit a spravovat virtuální počítače Linux s hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="0e1be-103">Create and Manage Linux VMs with hello Azure CLI</span></span>

<span data-ttu-id="0e1be-104">Virtuální počítače Azure, zadejte plně konfigurovatelné a flexibilní výpočetního prostředí.</span><span class="sxs-lookup"><span data-stu-id="0e1be-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="0e1be-105">Tento kurz se zaměřuje na základní virtuální počítač Azure nasazení položky například vyberete velikost virtuálního počítače, výběr image virtuálního počítače a nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e1be-105">This tutorial covers basic Azure virtual machine deployment items such as selecting a VM size, selecting a VM image, and deploying a VM.</span></span> <span data-ttu-id="0e1be-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="0e1be-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0e1be-107">Vytvořit a připojit tooa virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-107">Create and connect tooa VM</span></span>
> * <span data-ttu-id="0e1be-108">Vyberte a použijte Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-108">Select and use VM images</span></span>
> * <span data-ttu-id="0e1be-109">Zobrazení a používání určité velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-109">View and use specific VM sizes</span></span>
> * <span data-ttu-id="0e1be-110">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0e1be-110">Resize a VM</span></span>
> * <span data-ttu-id="0e1be-111">Zobrazení a pochopit stav virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0e1be-111">View and understand VM state</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0e1be-112">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0e1be-112">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="0e1be-113">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="0e1be-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="0e1be-114">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0e1be-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-resource-group"></a><span data-ttu-id="0e1be-115">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="0e1be-115">Create resource group</span></span>

<span data-ttu-id="0e1be-116">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e1be-116">Create a resource group with hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="0e1be-117">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="0e1be-117">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="0e1be-118">Před virtuálního počítače je třeba vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="0e1be-118">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="0e1be-119">V tomto příkladu skupinu prostředků s názvem *myResourceGroupVM* je vytvořen v hello *eastus* oblast.</span><span class="sxs-lookup"><span data-stu-id="0e1be-119">In this example, a resource group named *myResourceGroupVM* is created in hello *eastus* region.</span></span> 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

<span data-ttu-id="0e1be-120">Skupina prostředků Hello je zadána při vytváření nebo úpravách virtuálního počítače, které jsou viditelné v rámci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0e1be-120">hello resource group is specified when creating or modifying a VM, which can be seen throughout this tutorial.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="0e1be-121">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0e1be-121">Create virtual machine</span></span>

<span data-ttu-id="0e1be-122">Vytvoření virtuálního počítače pomocí hello [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e1be-122">Create a virtual machine with hello [az vm create](https://docs.microsoft.com/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="0e1be-123">Při vytváření virtuálního počítače, jsou k dispozici jako jsou bitové kopie operačního systému, přihlašovací údaje velikost a správu disku několik možností.</span><span class="sxs-lookup"><span data-stu-id="0e1be-123">When creating a virtual machine, several options are available such as operating system image, disk sizing, and administrative credentials.</span></span> <span data-ttu-id="0e1be-124">V tomto příkladu se vytvoří virtuální počítač s názvem *Můjvp* systémem Ubuntu Server.</span><span class="sxs-lookup"><span data-stu-id="0e1be-124">In this example, a virtual machine is created with a name of *myVM* running Ubuntu Server.</span></span> 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

<span data-ttu-id="0e1be-125">Jednou hello, kterou virtuální počítač byl vytvořen, hello rozhraní příkazového řádku Azure výstupy informace o hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e1be-125">Once hello VM has been created, hello Azure CLI outputs information about hello VM.</span></span> <span data-ttu-id="0e1be-126">Poznamenejte si hello `publicIpAddress`, tato adresa může být použité tooaccess hello virtuální počítač...</span><span class="sxs-lookup"><span data-stu-id="0e1be-126">Take note of hello `publicIpAddress`, this address can be used tooaccess hello virtual machine..</span></span> 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-toovm"></a><span data-ttu-id="0e1be-127">Připojit tooVM</span><span class="sxs-lookup"><span data-stu-id="0e1be-127">Connect tooVM</span></span>

<span data-ttu-id="0e1be-128">Nyní můžete přihlásit toohello virtuálních počítačů pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="0e1be-128">You can now connect toohello VM using SSH.</span></span> <span data-ttu-id="0e1be-129">Nahraďte hello příklad IP adresu hello `publicIpAddress` si poznamenali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="0e1be-129">Replace hello example IP address with hello `publicIpAddress` noted in hello previous step.</span></span>

```bash
ssh 52.174.34.95
```

<span data-ttu-id="0e1be-130">Po dokončení s hello virtuálního počítače, zavřete relace SSH hello.</span><span class="sxs-lookup"><span data-stu-id="0e1be-130">Once finished with hello VM, close hello SSH session.</span></span> 

```bash
exit
```

## <a name="understand-vm-images"></a><span data-ttu-id="0e1be-131">Pochopení Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-131">Understand VM images</span></span>

<span data-ttu-id="0e1be-132">Hello Azure marketplace obsahuje celou řadu imagí, které se dají použít toocreate virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e1be-132">hello Azure marketplace includes many images that can be used toocreate VMs.</span></span> <span data-ttu-id="0e1be-133">V předchozích krocích hello byl vytvořen virtuální počítač pomocí Ubuntu obrázku.</span><span class="sxs-lookup"><span data-stu-id="0e1be-133">In hello previous steps, a virtual machine was created using an Ubuntu image.</span></span> <span data-ttu-id="0e1be-134">V tomto kroku hello rozhraní příkazového řádku Azure je použité toosearch hello marketplace CentOS obrázek, který je pak používá toodeploy druhý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="0e1be-134">In this step, hello Azure CLI is used toosearch hello marketplace for a CentOS image, which is then used toodeploy a second virtual machine.</span></span>  

<span data-ttu-id="0e1be-135">toosee seznam hello nejčastěji používaná bitové kopie, použijte hello [seznamu obrázků virtuálních počítačů az](/cli/azure/vm/image#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e1be-135">toosee a list of hello most commonly used images, use hello [az vm image list](/cli/azure/vm/image#list) command.</span></span>

```azurecli-interactive 
az vm image list --output table
```

<span data-ttu-id="0e1be-136">výstup příkazu Hello vrátí hello nejoblíbenější Image virtuálních počítačů v Azure.</span><span class="sxs-lookup"><span data-stu-id="0e1be-136">hello command output returns hello most popular VM images on Azure.</span></span>

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

<span data-ttu-id="0e1be-137">Úplný seznam můžete zobrazit tak, že přidáte hello `--all` argument.</span><span class="sxs-lookup"><span data-stu-id="0e1be-137">A full list can be seen by adding hello `--all` argument.</span></span> <span data-ttu-id="0e1be-138">seznam obrázků Hello lze také filtrovat podle `--publisher` nebo `–-offer`.</span><span class="sxs-lookup"><span data-stu-id="0e1be-138">hello image list can also be filtered by `--publisher` or `–-offer`.</span></span> <span data-ttu-id="0e1be-139">V tomto příkladu hello seznam je filtrovaný pro všechny Image s nabídku odpovídající *CentOS*.</span><span class="sxs-lookup"><span data-stu-id="0e1be-139">In this example, hello list is filtered for all images with an offer that matches *CentOS*.</span></span> 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

<span data-ttu-id="0e1be-140">Částečné výstup:</span><span class="sxs-lookup"><span data-stu-id="0e1be-140">Partial output:</span></span>

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

<span data-ttu-id="0e1be-141">toodeploy virtuálního počítače pomocí konkrétní image, poznamenejte si hodnotu hello v hello *Urn* sloupce.</span><span class="sxs-lookup"><span data-stu-id="0e1be-141">toodeploy a VM using a specific image, take note of hello value in hello *Urn* column.</span></span> <span data-ttu-id="0e1be-142">Při zadávání hello bitové kopie, číslo verze bitové kopie hello lze nahradit "nejnovější", který vybere hello nejnovější verzi distribučních hello.</span><span class="sxs-lookup"><span data-stu-id="0e1be-142">When specifying hello image, hello image version number can be replaced with “latest”, which selects hello latest version of hello distribution.</span></span> <span data-ttu-id="0e1be-143">V tomto příkladu hello `--image` argument je použité toospecify hello nejnovější verze CentOS 6.5 bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="0e1be-143">In this example, hello `--image` argument is used toospecify hello latest version of a CentOS 6.5 image.</span></span>  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a><span data-ttu-id="0e1be-144">Pochopení velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-144">Understand VM sizes</span></span>

<span data-ttu-id="0e1be-145">Velikost virtuálního počítače určuje hello množství výpočetní prostředky, například procesoru, grafického procesoru a paměti, které jsou vytvářeny k dispozici toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e1be-145">A virtual machine size determines hello amount of compute resources such as CPU, GPU, and memory that are made available toohello virtual machine.</span></span> <span data-ttu-id="0e1be-146">Třeba virtuální počítače toobe odpovídající velikost pro hello očekává pracovní zatížení.</span><span class="sxs-lookup"><span data-stu-id="0e1be-146">Virtual machines need toobe sized appropriately for hello expected work load.</span></span> <span data-ttu-id="0e1be-147">Hodnota se zvyšuje zatížení, můžete ke změně velikosti existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e1be-147">If workload increases, an existing virtual machine can be resized.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="0e1be-148">Velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-148">VM Sizes</span></span>

<span data-ttu-id="0e1be-149">Následující tabulka Hello rozděluje velikosti do případy použití.</span><span class="sxs-lookup"><span data-stu-id="0e1be-149">hello following table categorizes sizes into use cases.</span></span>  

| <span data-ttu-id="0e1be-150">Typ</span><span class="sxs-lookup"><span data-stu-id="0e1be-150">Type</span></span>                     | <span data-ttu-id="0e1be-151">Velikosti</span><span class="sxs-lookup"><span data-stu-id="0e1be-151">Sizes</span></span>           |    <span data-ttu-id="0e1be-152">Popis</span><span class="sxs-lookup"><span data-stu-id="0e1be-152">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="0e1be-153">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="0e1be-153">General purpose</span></span>](sizes-general.md)         |<span data-ttu-id="0e1be-154">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7</span><span class="sxs-lookup"><span data-stu-id="0e1be-154">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span></span>| <span data-ttu-id="0e1be-155">Vyrovnáváním procesoru do paměti.</span><span class="sxs-lookup"><span data-stu-id="0e1be-155">Balanced CPU-to-memory.</span></span> <span data-ttu-id="0e1be-156">Ideální pro vývojáře nebo test a řešení pro malé toomedium aplikacím a datům.</span><span class="sxs-lookup"><span data-stu-id="0e1be-156">Ideal for dev / test and small toomedium applications and data solutions.</span></span>  |
| [<span data-ttu-id="0e1be-157">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="0e1be-157">Compute optimized</span></span>](sizes-compute.md)   | <span data-ttu-id="0e1be-158">Služby FS, F</span><span class="sxs-lookup"><span data-stu-id="0e1be-158">Fs, F</span></span>             | <span data-ttu-id="0e1be-159">Vysoké využití procesoru do paměti.</span><span class="sxs-lookup"><span data-stu-id="0e1be-159">High CPU-to-memory.</span></span> <span data-ttu-id="0e1be-160">Je vhodný pro střední provoz aplikace, síťových zařízení a dávkových procesů.</span><span class="sxs-lookup"><span data-stu-id="0e1be-160">Good for medium traffic applications, network appliances, and batch processes.</span></span>        |
| [<span data-ttu-id="0e1be-161">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="0e1be-161">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)    | <span data-ttu-id="0e1be-162">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="0e1be-162">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="0e1be-163">Vysoká paměti na core.</span><span class="sxs-lookup"><span data-stu-id="0e1be-163">High memory-to-core.</span></span> <span data-ttu-id="0e1be-164">Výborně hodí pro relačních databází, střední toolarge mezipaměti a analýzy v paměti.</span><span class="sxs-lookup"><span data-stu-id="0e1be-164">Great for relational databases, medium toolarge caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="0e1be-165">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="0e1be-165">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)      | <span data-ttu-id="0e1be-166">Ls</span><span class="sxs-lookup"><span data-stu-id="0e1be-166">Ls</span></span>                | <span data-ttu-id="0e1be-167">Vysoká propustnost disku a V/V.</span><span class="sxs-lookup"><span data-stu-id="0e1be-167">High disk throughput and IO.</span></span> <span data-ttu-id="0e1be-168">Ideální pro databáze NoSQL, SQL a velké objemy dat.</span><span class="sxs-lookup"><span data-stu-id="0e1be-168">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="0e1be-169">GPU</span><span class="sxs-lookup"><span data-stu-id="0e1be-169">GPU</span></span>](sizes-gpu.md)          | <span data-ttu-id="0e1be-170">VS, NC</span><span class="sxs-lookup"><span data-stu-id="0e1be-170">NV, NC</span></span>            | <span data-ttu-id="0e1be-171">Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa.</span><span class="sxs-lookup"><span data-stu-id="0e1be-171">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span>       |
| [<span data-ttu-id="0e1be-172">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="0e1be-172">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="0e1be-173">H, A8 11</span><span class="sxs-lookup"><span data-stu-id="0e1be-173">H, A8-11</span></span>          | <span data-ttu-id="0e1be-174">Naše nejúčinnějších procesoru virtuálních počítačů s volitelné vysokou propustností síťová rozhraní (počítače RDMA).</span><span class="sxs-lookup"><span data-stu-id="0e1be-174">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> 


### <a name="find-available-vm-sizes"></a><span data-ttu-id="0e1be-175">Najít dostupných velikostí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-175">Find available VM sizes</span></span>

<span data-ttu-id="0e1be-176">toosee seznam virtuálních počítačů velikostí k dispozici v určité oblasti, použijte hello [seznam velikosti virtuálních počítačů az](/cli/azure/vm#list-sizes) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e1be-176">toosee a list of VM sizes available in a particular region, use hello [az vm list-sizes](/cli/azure/vm#list-sizes) command.</span></span> 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

<span data-ttu-id="0e1be-177">Částečné výstup:</span><span class="sxs-lookup"><span data-stu-id="0e1be-177">Partial output:</span></span>

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a><span data-ttu-id="0e1be-178">Vytvoření virtuálního počítače s určitou velikost</span><span class="sxs-lookup"><span data-stu-id="0e1be-178">Create VM with specific size</span></span>

<span data-ttu-id="0e1be-179">V hello předchozí příklad vytvoření virtuálního počítače velikost nebylo zadáno, výsledkem je výchozí velikost.</span><span class="sxs-lookup"><span data-stu-id="0e1be-179">In hello previous VM creation example, a size was not provided, which results in a default size.</span></span> <span data-ttu-id="0e1be-180">Velikost virtuálního počítače lze vybrat pomocí čas vytvoření [vytvořit virtuální počítač az](/cli/azure/vm#create) a hello `--size` argument.</span><span class="sxs-lookup"><span data-stu-id="0e1be-180">A VM size can be selected at creation time using [az vm create](/cli/azure/vm#create) and hello `--size` argument.</span></span> 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a><span data-ttu-id="0e1be-181">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0e1be-181">Resize a VM</span></span>

<span data-ttu-id="0e1be-182">Po nasazení virtuálního počítače, může být změněnou tooincrease nebo snížit přidělení prostředků.</span><span class="sxs-lookup"><span data-stu-id="0e1be-182">After a VM has been deployed, it can be resized tooincrease or decrease resource allocation.</span></span>

<span data-ttu-id="0e1be-183">Před změnou velikosti virtuálního počítače, zkontrolujte, zda hello požadovaná velikost je k dispozici na aktuální Azure cluster hello.</span><span class="sxs-lookup"><span data-stu-id="0e1be-183">Before resizing a VM, check if hello desired size is available on hello current Azure cluster.</span></span> <span data-ttu-id="0e1be-184">Hello [az virtuálních počítačů seznamu-virtuálních počítačů-změny velikosti options](/cli/azure/vm#list-vm-resize-options) příkaz vrátí hello seznam velikostí.</span><span class="sxs-lookup"><span data-stu-id="0e1be-184">hello [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options) command returns hello list of sizes.</span></span> 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
<span data-ttu-id="0e1be-185">V případě, že velikost je k dispozici potřeby hello hello virtuálních počítačů velikost lze změnit ze stavu na zapnuté, ale po restartu během operace hello.</span><span class="sxs-lookup"><span data-stu-id="0e1be-185">If hello desired size is available, hello VM can be resized from a powered-on state, however it is rebooted during hello operation.</span></span> <span data-ttu-id="0e1be-186">Použití hello [změnit velikost virtuálního počítače az]( /cli/azure/vm#resize) příkaz tooperform hello změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="0e1be-186">Use hello [az vm resize]( /cli/azure/vm#resize) command tooperform hello resize.</span></span>

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

<span data-ttu-id="0e1be-187">V případě potřeby hello velikost není v aktuální clusteru hello hello virtuálních počítačů potřeb, které může dojít toobe navrácena před hello změnit velikost operace.</span><span class="sxs-lookup"><span data-stu-id="0e1be-187">If hello desired size is not on hello current cluster, hello VM needs toobe deallocated before hello resize operation can occur.</span></span> <span data-ttu-id="0e1be-188">Použití hello [az OM deallocate]( /cli/azure/vm#deallocate) příkaz toostop a navrátit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e1be-188">Use hello [az vm deallocate]( /cli/azure/vm#deallocate) command toostop and deallocate hello VM.</span></span> <span data-ttu-id="0e1be-189">Všimněte si, když hello virtuální počítač je zapnutý zpět, může odeberou všechna data na dočasné disku hello.</span><span class="sxs-lookup"><span data-stu-id="0e1be-189">Note, when hello VM is powered back on, any data on hello temp disk may be removed.</span></span> <span data-ttu-id="0e1be-190">Hello veřejnou IP adresu také změní, pokud se používá statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0e1be-190">hello public IP address also changes unless a static IP address is being used.</span></span> 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

<span data-ttu-id="0e1be-191">Jakmile navrácena, může dojít, změny velikosti hello.</span><span class="sxs-lookup"><span data-stu-id="0e1be-191">Once deallocated, hello resize can occur.</span></span> 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

<span data-ttu-id="0e1be-192">Po změně velikosti hello, lze spustit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e1be-192">After hello resize, hello VM can be started.</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a><span data-ttu-id="0e1be-193">Stavy spotřeby virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-193">VM power states</span></span>

<span data-ttu-id="0e1be-194">Virtuální počítač Azure může mít jednu z mnoha snížené spotřeby energie.</span><span class="sxs-lookup"><span data-stu-id="0e1be-194">An Azure VM can have one of many power states.</span></span> <span data-ttu-id="0e1be-195">Tento stav představuje hello aktuální stav hello virtuálních počítačů z hlediska hello hello hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="0e1be-195">This state represents hello current state of hello VM from hello standpoint of hello hypervisor.</span></span> 

### <a name="power-states"></a><span data-ttu-id="0e1be-196">Stavy spotřeby.</span><span class="sxs-lookup"><span data-stu-id="0e1be-196">Power states</span></span>

| <span data-ttu-id="0e1be-197">Stav napájení</span><span class="sxs-lookup"><span data-stu-id="0e1be-197">Power State</span></span> | <span data-ttu-id="0e1be-198">Popis</span><span class="sxs-lookup"><span data-stu-id="0e1be-198">Description</span></span>
|----|----|
| <span data-ttu-id="0e1be-199">Spouštění</span><span class="sxs-lookup"><span data-stu-id="0e1be-199">Starting</span></span> | <span data-ttu-id="0e1be-200">Označuje, že se spouští hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e1be-200">Indicates hello virtual machine is being started.</span></span> |
| <span data-ttu-id="0e1be-201">Běžící (Spuštěno)</span><span class="sxs-lookup"><span data-stu-id="0e1be-201">Running</span></span> | <span data-ttu-id="0e1be-202">Označuje, že aby hello virtuální počítač běží.</span><span class="sxs-lookup"><span data-stu-id="0e1be-202">Indicates that hello virtual machine is running.</span></span> |
| <span data-ttu-id="0e1be-203">Zastavování</span><span class="sxs-lookup"><span data-stu-id="0e1be-203">Stopping</span></span> | <span data-ttu-id="0e1be-204">Označuje, že Probíhá zastavování virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="0e1be-204">Indicates that hello virtual machine is being stopped.</span></span> | 
| <span data-ttu-id="0e1be-205">Zastaveno</span><span class="sxs-lookup"><span data-stu-id="0e1be-205">Stopped</span></span> | <span data-ttu-id="0e1be-206">Označuje, že tento virtuální počítač hello je zastavena.</span><span class="sxs-lookup"><span data-stu-id="0e1be-206">Indicates that hello virtual machine is stopped.</span></span> <span data-ttu-id="0e1be-207">Virtuální počítače ve stavu Zastaveno hello stále platit poplatky výpočty.</span><span class="sxs-lookup"><span data-stu-id="0e1be-207">Virtual machines in hello stopped state still incur compute charges.</span></span>  |
| <span data-ttu-id="0e1be-208">Rušení přidělování</span><span class="sxs-lookup"><span data-stu-id="0e1be-208">Deallocating</span></span> | <span data-ttu-id="0e1be-209">Označuje, že tento virtuální počítač hello je navrácena.</span><span class="sxs-lookup"><span data-stu-id="0e1be-209">Indicates that hello virtual machine is being deallocated.</span></span> |
| <span data-ttu-id="0e1be-210">Zrušeno</span><span class="sxs-lookup"><span data-stu-id="0e1be-210">Deallocated</span></span> | <span data-ttu-id="0e1be-211">Označuje, že hello virtuálního počítače je odebrán z hello hypervisoru, ale stále k dispozici v rovině řízení hello.</span><span class="sxs-lookup"><span data-stu-id="0e1be-211">Indicates that hello virtual machine is removed from hello hypervisor but still available in hello control plane.</span></span> <span data-ttu-id="0e1be-212">Virtuální počítače v hello Deallocated stavu nevznikají poplatky za výpočty.</span><span class="sxs-lookup"><span data-stu-id="0e1be-212">Virtual machines in hello Deallocated state do not incur compute charges.</span></span> |
| - | <span data-ttu-id="0e1be-213">Označuje, že stav napájení hello hello virtuálního počítače neznámý.</span><span class="sxs-lookup"><span data-stu-id="0e1be-213">Indicates that hello power state of hello virtual machine is unknown.</span></span> |

### <a name="find-power-state"></a><span data-ttu-id="0e1be-214">Najít stav napájení</span><span class="sxs-lookup"><span data-stu-id="0e1be-214">Find power state</span></span>

<span data-ttu-id="0e1be-215">Stav hello tooretrieve konkrétní virtuální počítač, použijte hello [az virtuální počítač získat zobrazení instance](/cli/azure/vm#get-instance-view) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e1be-215">tooretrieve hello state of a particular VM, use hello [az vm get instance-view](/cli/azure/vm#get-instance-view) command.</span></span> <span data-ttu-id="0e1be-216">Zda toospecify být platný název pro virtuální počítač a skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="0e1be-216">Be sure toospecify a valid name for a virtual machine and resource group.</span></span> 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

<span data-ttu-id="0e1be-217">Výstup:</span><span class="sxs-lookup"><span data-stu-id="0e1be-217">Output:</span></span>

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a><span data-ttu-id="0e1be-218">Úlohy správy</span><span class="sxs-lookup"><span data-stu-id="0e1be-218">Management tasks</span></span>

<span data-ttu-id="0e1be-219">Během hello životního cyklu virtuálního počítače můžete úlohy správy toorun například spuštění, zastavení nebo odstranění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e1be-219">During hello life-cycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="0e1be-220">Kromě toho můžete toocreate skripty tooautomate opakovaných nebo komplexní úlohy.</span><span class="sxs-lookup"><span data-stu-id="0e1be-220">Additionally, you may want toocreate scripts tooautomate repetitive or complex tasks.</span></span> <span data-ttu-id="0e1be-221">Pomocí hello rozhraní příkazového řádku Azure, mnoho běžné úlohy správy lze spustit z příkazového řádku hello nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="0e1be-221">Using hello Azure CLI, many common management tasks can be run from hello command line or in scripts.</span></span> 

### <a name="get-ip-address"></a><span data-ttu-id="0e1be-222">Získat IP adresu</span><span class="sxs-lookup"><span data-stu-id="0e1be-222">Get IP address</span></span>

<span data-ttu-id="0e1be-223">Tento příkaz vrátí hello privátní a veřejné IP adresy virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e1be-223">This command returns hello private and public IP addresses of a virtual machine.</span></span>  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a><span data-ttu-id="0e1be-224">Zastavit virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="0e1be-224">Stop virtual machine</span></span>

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a><span data-ttu-id="0e1be-225">Spustit virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="0e1be-225">Start virtual machine</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a><span data-ttu-id="0e1be-226">Odstraňte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="0e1be-226">Delete resource group</span></span>

<span data-ttu-id="0e1be-227">Odstranění skupiny prostředků se také odstraní všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="0e1be-227">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a><span data-ttu-id="0e1be-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e1be-228">Next steps</span></span>

<span data-ttu-id="0e1be-229">V tomto kurzu jste se dozvěděli o základní vytvoření virtuálního počítače a správy, jako například:</span><span class="sxs-lookup"><span data-stu-id="0e1be-229">In this tutorial, you learned about basic VM creation and management such as how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0e1be-230">Vytvořit a připojit tooa virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-230">Create and connect tooa VM</span></span>
> * <span data-ttu-id="0e1be-231">Vyberte a použijte Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-231">Select and use VM images</span></span>
> * <span data-ttu-id="0e1be-232">Zobrazení a používání určité velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e1be-232">View and use specific VM sizes</span></span>
> * <span data-ttu-id="0e1be-233">Změna velikosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0e1be-233">Resize a VM</span></span>
> * <span data-ttu-id="0e1be-234">Zobrazení a pochopit stav virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0e1be-234">View and understand VM state</span></span>

<span data-ttu-id="0e1be-235">Posunutí další kurz toolearn toohello o disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e1be-235">Advance toohello next tutorial toolearn about VM disks.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="0e1be-236">Vytvoření a správa virtuálních počítačů disky</span><span class="sxs-lookup"><span data-stu-id="0e1be-236">Create and Manage VM disks</span></span>](./tutorial-manage-disks.md)
