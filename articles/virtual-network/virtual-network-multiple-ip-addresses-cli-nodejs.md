---
title: "aaaVM s více IP adres pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak tooassign více IP adres tooa virtuálního počítače pomocí hello 1.0 rozhraní příkazového řádku Azure | Správce prostředků."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 83ad48e67309fb21d5aca967d4f2c01afdc0b5cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-azure-cli-10"></a><span data-ttu-id="c63e9-103">Přiřadit více IP adres toovirtual počítačů pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c63e9-103">Assign multiple IP addresses toovirtual machines using Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="c63e9-104">Tento článek vysvětluje, jak toocreate virtuální počítač (VM) prostřednictvím pomocí modelu nasazení Azure Resource Manager hello hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="c63e9-104">This article explains how toocreate a virtual machine (VM) through hello Azure Resource Manager deployment model using hello Azure CLI 1.0.</span></span> <span data-ttu-id="c63e9-105">Tooresources vytvořené pomocí modelu nasazení classic hello nelze přiřadit více IP adres.</span><span class="sxs-lookup"><span data-stu-id="c63e9-105">Multiple IP addresses cannot be assigned tooresources created through hello classic deployment model.</span></span> <span data-ttu-id="c63e9-106">Další informace o modelech nasazení Azure, přečtěte si hello toolearn [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c63e9-106">toolearn more about Azure deployment models, read hello [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="c63e9-107"><a name = "create"></a>Vytvoření virtuálního počítače s více IP adres</span><span class="sxs-lookup"><span data-stu-id="c63e9-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="c63e9-108">Můžete dokončit tuto úlohu pomocí hello 1.0 rozhraní příkazového řádku Azure (v tomto článku) nebo hello [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c63e9-108">You can complete this task using hello Azure CLI 1.0 (this article) or hello [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md).</span></span> <span data-ttu-id="c63e9-109">Hello kroky, které následují vysvětlují, jak toocreate příklad virtuálního počítače s více IP adres, jak je popsáno v hello scénáři.</span><span class="sxs-lookup"><span data-stu-id="c63e9-109">hello steps that follow explain how toocreate an example VM with multiple IP addresses, as described in hello scenario.</span></span> <span data-ttu-id="c63e9-110">Změnit názvy proměnných a typy IP adres podle potřeby týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="c63e9-110">Change variable names and IP address types as required for your implementation.</span></span>

1. <span data-ttu-id="c63e9-111">Instalace a konfigurace hello Azure CLI 1.0 podle následující hello kroky v hello [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článek a přihlaste se k účtu Azure s hello `azure-login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c63e9-111">Install and configure hello Azure CLI 1.0 by following hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article and log into your Azure account with hello `azure-login` command.</span></span>

2. <span data-ttu-id="c63e9-112">Vytvořte skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="c63e9-112">Create a resource group:</span></span>
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. <span data-ttu-id="c63e9-113">Vytvořte virtuální síť:</span><span class="sxs-lookup"><span data-stu-id="c63e9-113">Create a virtual network:</span></span>

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. <span data-ttu-id="c63e9-114">Vytvořte podsíť ve virtuální síti hello:</span><span class="sxs-lookup"><span data-stu-id="c63e9-114">Create a subnet in hello virtual network:</span></span>

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. <span data-ttu-id="c63e9-115">Vytvořte účet úložiště pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c63e9-115">Create  a storage account for hello VM.</span></span> <span data-ttu-id="c63e9-116">Před spuštěním hello následující příkaz, nahraďte *můj_účet_úložiště* s jedinečným názvem.</span><span class="sxs-lookup"><span data-stu-id="c63e9-116">Before running hello following command, replace *mystorageaccount* with a unique name.</span></span> <span data-ttu-id="c63e9-117">Název Hello musí být jedinečný v rámci Azure:</span><span class="sxs-lookup"><span data-stu-id="c63e9-117">hello name must be unique across Azure:</span></span>

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. <span data-ttu-id="c63e9-118">Vytvořte hello konfigurace protokolu IP, síťový adaptér a přiřaďte hello IP konfigurace toohello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="c63e9-118">Create hello IP configurations, a NIC, and assign hello IP configurations toohello NIC.</span></span> <span data-ttu-id="c63e9-119">Můžete přidat, odebrat ani změnit hello konfigurace podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="c63e9-119">You can add, remove, or change hello configurations as necessary.</span></span> <span data-ttu-id="c63e9-120">Hello následující konfigurace jsou popsané v hello scénáři:</span><span class="sxs-lookup"><span data-stu-id="c63e9-120">hello following configurations are described in hello scenario:</span></span>

    <span data-ttu-id="c63e9-121">**IPConfig-1**</span><span class="sxs-lookup"><span data-stu-id="c63e9-121">**IPConfig-1**</span></span>

    <span data-ttu-id="c63e9-122">Zadejte hello příkazy, které následují toocreate:</span><span class="sxs-lookup"><span data-stu-id="c63e9-122">Enter hello commands that follow toocreate:</span></span>

    - <span data-ttu-id="c63e9-123">Prostředek veřejné IP adresy s statickou veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="c63e9-123">A public IP address resource with a static public IP address</span></span>
    - <span data-ttu-id="c63e9-124">Síťový adaptér, přiřazování hello veřejnou IP adresu a statické tooit privátní adresy IP.</span><span class="sxs-lookup"><span data-stu-id="c63e9-124">A NIC, assigning hello public IP address and a static private IP address tooit.</span></span>
    
    <span data-ttu-id="c63e9-125">Nahraďte *mypublicdns* s názvem, který je v rámci hello umístění Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="c63e9-125">Replace *mypublicdns* with a name that is unique within hello Azure location.</span></span>

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > <span data-ttu-id="c63e9-126">Veřejné IP adresy mají nominální poplatek.</span><span class="sxs-lookup"><span data-stu-id="c63e9-126">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="c63e9-127">více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky.</span><span class="sxs-lookup"><span data-stu-id="c63e9-127">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="c63e9-128">Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném.</span><span class="sxs-lookup"><span data-stu-id="c63e9-128">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="c63e9-129">Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.</span><span class="sxs-lookup"><span data-stu-id="c63e9-129">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

    <span data-ttu-id="c63e9-130">**IPConfig-2**</span><span class="sxs-lookup"><span data-stu-id="c63e9-130">**IPConfig-2**</span></span>

     <span data-ttu-id="c63e9-131">Zadejte následující příkazy toocreate hello nový prostředek veřejné IP adresy a nové konfigurace IP adresy se statickou veřejnou IP adresu a statickou privátní IP adresou:</span><span class="sxs-lookup"><span data-stu-id="c63e9-131">Enter hello following commands toocreate a new public IP address resource and a new IP configuration with a static public IP address and a static private IP address:</span></span>
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    <span data-ttu-id="c63e9-132">**IPConfig – 3**</span><span class="sxs-lookup"><span data-stu-id="c63e9-132">**IPConfig-3**</span></span>

    <span data-ttu-id="c63e9-133">Zadejte následující příkazy toocreate konfiguraci IP adres se statickou privátní IP adresou a žádné veřejnou IP adresu hello:</span><span class="sxs-lookup"><span data-stu-id="c63e9-133">Enter hello following commands toocreate an IP configuration with a static private IP address and no public IP address:</span></span>

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    ><span data-ttu-id="c63e9-134">Když v tomto článku přiřadí všechny tooa konfigurace IP jednu síťovou kartu, je také možné přiřadit více IP konfigurace tooany síťový adaptér ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="c63e9-134">Though this article assigns all IP configurations tooa single NIC, you can also assign multiple IP configurations tooany NIC in a VM.</span></span> <span data-ttu-id="c63e9-135">toolearn jak toocreate virtuálního počítače s více síťovými kartami, přečtěte si text hello vytvořit virtuální počítač s více síťových adaptérů článku.</span><span class="sxs-lookup"><span data-stu-id="c63e9-135">toolearn how toocreate a VM with multiple NICs, read hello Create a VM with multiple NICs article.</span></span>

7. <span data-ttu-id="c63e9-136">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="c63e9-136">Create a Linux VM</span></span> 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    <span data-ttu-id="c63e9-137">například toochange hello v2 tooStandard DS2 velikost virtuálního počítače, jednoduše přidejte následující vlastnost hello `--vm-size Standard_DS3_v2` toohello `azure vm create` příkazu v kroku 6.</span><span class="sxs-lookup"><span data-stu-id="c63e9-137">toochange hello VM size tooStandard DS2 v2, for example, simply add hello following property `--vm-size Standard_DS3_v2` toohello `azure vm create` command in step 6.</span></span>

8. <span data-ttu-id="c63e9-138">Zadejte následující příkaz tooview hello seskupování hello a hello související konfigurace protokolu IP:</span><span class="sxs-lookup"><span data-stu-id="c63e9-138">Enter hello following command tooview hello NIC and hello associated IP configurations:</span></span>

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. <span data-ttu-id="c63e9-139">Přidat hello privátní IP adresy toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="c63e9-139">Add hello private IP addresses toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span>

## <span data-ttu-id="c63e9-140"><a name="add"></a>Přidat tooa IP adresy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c63e9-140"><a name="add"></a>Add IP addresses tooa VM</span></span>

<span data-ttu-id="c63e9-141">Můžete přidat další privátní a veřejné IP adresy tooan stávající síťové karty pomocí hello kroků, které následují.</span><span class="sxs-lookup"><span data-stu-id="c63e9-141">You can add additional private and public IP addresses tooan existing NIC by completing hello steps that follow.</span></span> <span data-ttu-id="c63e9-142">Příklady Hello stavějí hello [scénář](#Scenario) popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c63e9-142">hello examples build upon hello [scenario](#Scenario) described in this article.</span></span>

1. <span data-ttu-id="c63e9-143">Otevřete rozhraní příkazového řádku Azure a dokončení hello zbývající kroky v této části v rámci jedné relace rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c63e9-143">Open Azure CLI and complete hello remaining steps in this section within a single CLI session.</span></span> <span data-ttu-id="c63e9-144">Pokud ještě nemáte rozhraní příkazového řádku Azure, instalaci a konfiguraci, dokončení hello kroky v hello [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článek a přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="c63e9-144">If you don't already have Azure CLI installed and configured, complete hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article and log into your Azure account.</span></span>

2. <span data-ttu-id="c63e9-145">Proveďte kroky hello v jednom z hello následující části, podle požadavků:</span><span class="sxs-lookup"><span data-stu-id="c63e9-145">Complete hello steps in one of hello following sections, based on your requirements:</span></span>

    - <span data-ttu-id="c63e9-146">**Přidejte privátní IP adresy**</span><span class="sxs-lookup"><span data-stu-id="c63e9-146">**Add a private IP address**</span></span>
    
        <span data-ttu-id="c63e9-147">tooadd tooa privátní adresy IP síťový adaptér, je nutné vytvořit konfiguraci IP adres pomocí níže uvedeného příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="c63e9-147">tooadd a private IP address tooa NIC, you must create an IP configuration using hello command below.</span></span> <span data-ttu-id="c63e9-148">Hello statickou adresu musí obsahovat adresu nepoužívané hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="c63e9-148">hello static address must be an unused address for hello subnet.</span></span>

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        <span data-ttu-id="c63e9-149">Vytvořte tolik konfigurace, jak požadujete, pomocí konfigurace jedinečné názvy a privátní IP adresy (pro konfigurace s statické IP adresy).</span><span class="sxs-lookup"><span data-stu-id="c63e9-149">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    - <span data-ttu-id="c63e9-150">**Přidejte veřejnou IP adresu**</span><span class="sxs-lookup"><span data-stu-id="c63e9-150">**Add a public IP address**</span></span>
    
        <span data-ttu-id="c63e9-151">Veřejná IP adresa se přidá přidružením tooeither na novou konfiguraci protokolu IP nebo existující konfiguraci IP adres.</span><span class="sxs-lookup"><span data-stu-id="c63e9-151">A public IP address is added by associating it tooeither a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="c63e9-152">Kroky hello v jedné z hello částí, které následují, potřebujete.</span><span class="sxs-lookup"><span data-stu-id="c63e9-152">Complete hello steps in one of hello sections that follow, as you require.</span></span>

        > [!NOTE]
        > <span data-ttu-id="c63e9-153">Veřejné IP adresy mají nominální poplatek.</span><span class="sxs-lookup"><span data-stu-id="c63e9-153">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="c63e9-154">více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky.</span><span class="sxs-lookup"><span data-stu-id="c63e9-154">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="c63e9-155">Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném.</span><span class="sxs-lookup"><span data-stu-id="c63e9-155">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="c63e9-156">Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.</span><span class="sxs-lookup"><span data-stu-id="c63e9-156">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
        >

        <span data-ttu-id="c63e9-157">**Přidružení hello prostředků tooa nová konfigurace IP**</span><span class="sxs-lookup"><span data-stu-id="c63e9-157">**Associate hello resource tooa new IP configuration**</span></span>
    
        <span data-ttu-id="c63e9-158">Vždy, když přidáte veřejnou IP adresu v novou konfiguraci protokolu IP, musíte taky přidat privátní IP adresy, protože všechny konfigurace protokolu IP, musí mít privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="c63e9-158">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="c63e9-159">Můžete přidat existující prostředek veřejné IP adresy, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="c63e9-159">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="c63e9-160">toocreate novou, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c63e9-160">toocreate a new one, enter hello following command:</span></span>

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        <span data-ttu-id="c63e9-161">toocreate na novou konfiguraci protokolu IP se statickou privátní IP adresou a hello přidružené *myPublicIP3* veřejných IP adres prostředků, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c63e9-161">toocreate a new IP configuration with a static private IP address and hello associated *myPublicIP3* public IP address resource, enter hello following command:</span></span>

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        <span data-ttu-id="c63e9-162">**Přidružení hello prostředků tooan existující konfigurace IP**</span><span class="sxs-lookup"><span data-stu-id="c63e9-162">**Associate hello resource tooan existing IP configuration**</span></span>

        <span data-ttu-id="c63e9-163">Prostředek veřejné IP adresy lze pouze přidružené tooan konfigurace protokolu IP, který ho přidružené neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="c63e9-163">A public IP address resource can only be associated tooan IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="c63e9-164">Můžete určit, zda konfiguraci IP adres má přidružené veřejnou IP adresu zadáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c63e9-164">You can determine whether an IP configuration has an associated public IP address by entering hello following command:</span></span>

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        <span data-ttu-id="c63e9-165">Vyhledejte podobné toohello řádku, ten, který následuje pro IPConfig 3 v hello vrátil výstup:</span><span class="sxs-lookup"><span data-stu-id="c63e9-165">Look for a line similar toohello one that follows for IPConfig-3 in hello returned output:</span></span>

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        <span data-ttu-id="c63e9-166">Od hello **veřejnou IP adresu** sloupec pro *IpConfig 3* je prázdné, žádný prostředek veřejné IP adresy je aktuálně přidružené tooit.</span><span class="sxs-lookup"><span data-stu-id="c63e9-166">Since hello **Public IP** column for *IpConfig-3* is blank, no public IP address resource is currently associated tooit.</span></span> <span data-ttu-id="c63e9-167">Můžete přidat existující veřejnou IP adresu prostředku tooIpConfig-3, nebo zadejte následující příkaz toocreate jeden hello:</span><span class="sxs-lookup"><span data-stu-id="c63e9-167">You can add an existing public IP address resource tooIpConfig-3, or enter hello following command toocreate one:</span></span>

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        <span data-ttu-id="c63e9-168">Zadejte následující příkaz tooassociate hello veřejných IP adres toohello stávající IP konfigurace prostředků s názvem hello *IPConfig 3*:</span><span class="sxs-lookup"><span data-stu-id="c63e9-168">Enter hello following command tooassociate hello public IP address resource toohello existing IP configuration named *IPConfig-3*:</span></span>
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. <span data-ttu-id="c63e9-169">Zobrazení hello privátních IP adres a hello veřejnou IP adresu prostředky přiřazené toohello hello síťový adaptér tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c63e9-169">View hello private IP addresses and hello public IP address resources assigned toohello NIC by entering hello following command:</span></span>

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      <span data-ttu-id="c63e9-170">Výstup je podobné toohello následující vrácen Hello:</span><span class="sxs-lookup"><span data-stu-id="c63e9-170">hello returned output is similar toohello following:</span></span>
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. <span data-ttu-id="c63e9-171">Přidat hello privátních IP adres jste přidali toohello seskupování toohello virtuálních počítačů operačního systému podle následujících pokynů hello v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="c63e9-171">Add hello private IP addresses you added toohello NIC toohello VM operating system by following hello instructions in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="c63e9-172">Nepřidávejte hello veřejné IP adresy toohello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="c63e9-172">Do not add hello public IP addresses toohello operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
