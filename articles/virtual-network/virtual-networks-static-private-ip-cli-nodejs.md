---
title: "aaaConfigure privátních IP adres pro virtuální počítače – 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooconfigure privátních IP adres pro virtuální počítače pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6caae79c98c7bc5f246b7bb3bb8bd8f032eb2d8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-cli-10"></a><span data-ttu-id="746c9-103">Nakonfigurovat privátní IP adresy pro virtuální počítač pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="746c9-103">Configure private IP addresses for a virtual machine using hello Azure CLI 1.0</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="746c9-104">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="746c9-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="746c9-105">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="746c9-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="746c9-106">[Azure CLI 1.0](#how-to-specify-a-static-private-ip-address-when-creating-a-vm) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="746c9-106">[Azure CLI 1.0](#how-to-specify-a-static-private-ip-address-when-creating-a-vm) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="746c9-107">[Azure CLI 2.0](virtual-networks-static-private-ip-arm-cli.md) -naše generace CLI pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="746c9-107">[Azure CLI 2.0](virtual-networks-static-private-ip-arm-cli.md) - our next-generation CLI for hello resource management deployment model</span></span> 

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="746c9-108">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="746c9-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="746c9-109">Můžete také [spravovat statickou privátní IP adresou v modelu nasazení classic hello](virtual-networks-static-private-ip-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="746c9-109">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="746c9-110">níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="746c9-110">hello sample Azure CLI commands below expect a simple environment already created.</span></span> <span data-ttu-id="746c9-111">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="746c9-111">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-cli.md).</span></span>

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="746c9-112">Jak toospecify statickou privátní IP adresy při vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="746c9-112">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="746c9-113">virtuální počítač s názvem toocreate *DNS01* v hello *front-endu* podsíť virtuální sítě s názvem *TestVNet* se statickou privátní IP z *192.168.1.101*, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="746c9-113">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="746c9-114">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="746c9-114">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="746c9-115">Spustit hello **azure konfigurace režim** příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="746c9-115">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>
   
        azure config mode arm
   
    <span data-ttu-id="746c9-116">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="746c9-116">Expected output:</span></span>
   
        info:    New mode is arm
3. <span data-ttu-id="746c9-117">Spustit hello **vytvoření veřejné ip síť azure** toocreate veřejnou IP adresu pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="746c9-117">Run hello **azure network public-ip create** toocreate a public IP for hello VM.</span></span> <span data-ttu-id="746c9-118">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="746c9-118">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure network public-ip create -g TestRG -n TestPIP -l centralus
   
    <span data-ttu-id="746c9-119">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="746c9-119">Expected output:</span></span>
   
        info:    Executing command network public-ip create
        + Looking up hello public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up hello public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK
   
   * <span data-ttu-id="746c9-120">**-g (nebo --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-120">**-g (or --resource-group)**.</span></span> <span data-ttu-id="746c9-121">Název hello prostředků skupiny hello veřejnou IP adresu budou vytvořeny v.</span><span class="sxs-lookup"><span data-stu-id="746c9-121">Name of hello resource group hello public IP will be created in.</span></span>
   * <span data-ttu-id="746c9-122">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-122">**-n (or --name)**.</span></span> <span data-ttu-id="746c9-123">Název veřejné IP adresy hello.</span><span class="sxs-lookup"><span data-stu-id="746c9-123">Name of hello public IP.</span></span>
   * <span data-ttu-id="746c9-124">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-124">**-l (or --location)**.</span></span> <span data-ttu-id="746c9-125">Oblast Azure, kde bude vytvořen hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="746c9-125">Azure region where hello public IP will be created.</span></span> <span data-ttu-id="746c9-126">V našem scénáři je to *centralus*.</span><span class="sxs-lookup"><span data-stu-id="746c9-126">For our scenario, *centralus*.</span></span>
4. <span data-ttu-id="746c9-127">Spustit hello **síťových adaptérů sítě azure vytvořit** příkaz toocreate síťový adaptér se statickou privátní IP.</span><span class="sxs-lookup"><span data-stu-id="746c9-127">Run hello **azure network nic create** command toocreate a NIC with a static private IP.</span></span> <span data-ttu-id="746c9-128">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="746c9-128">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd
   
    <span data-ttu-id="746c9-129">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="746c9-129">Expected output:</span></span>
   
        + Looking up hello network interface "TestNIC"
        + Looking up hello subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up hello network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
   
   * <span data-ttu-id="746c9-130">**-a (nebo--privátní. ip adresa)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-130">**-a (or --private-ip-address)**.</span></span> <span data-ttu-id="746c9-131">Statickou privátní IP adresou pro hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="746c9-131">Static private IP address for hello NIC.</span></span>
   * <span data-ttu-id="746c9-132">**-m (nebo--název podsítě virtuální sítě)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-132">**-m (or --subnet-vnet-name)**.</span></span> <span data-ttu-id="746c9-133">Název hello virtuální síť, kde bude vytvořen hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="746c9-133">Name of hello VNet where hello NIC will be created.</span></span>
   * <span data-ttu-id="746c9-134">**-k (nebo--název podsítě)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-134">**-k (or --subnet-name)**.</span></span> <span data-ttu-id="746c9-135">Název podsítě hello, kde bude vytvořen hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="746c9-135">Name of hello subnet where hello NIC will be created.</span></span>
5. <span data-ttu-id="746c9-136">Spustit hello **vytvoření virtuálního počítače azure** hello toocreate příkaz virtuálního počítače pomocí hello veřejná IP adresa a síťovou kartu vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="746c9-136">Run hello **azure vm create** command toocreate hello VM using hello public IP and NIC created above.</span></span> <span data-ttu-id="746c9-137">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="746c9-137">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd
   
    <span data-ttu-id="746c9-138">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="746c9-138">Expected output:</span></span>
   
        info:    Executing command vm create
        + Looking up hello VM "DNS01"
        info:    Using hello VM Size "Standard_A1"
        warn:    hello image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    hello [OS, Data] Disk or image configuration requires storage account
        + Looking up hello storage account vnetstorage
        + Looking up hello NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in hello NIC "TestNIC"
        info:    Found public ip parameters, trying toosetup PublicIP profile
        + Looking up hello public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up hello NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK
   
   * <span data-ttu-id="746c9-139">**-y (nebo typ operačního systému –)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-139">**-y (or --os-type)**.</span></span> <span data-ttu-id="746c9-140">Typ operačního systému pro virtuální počítač, hello buď *Windows* nebo *Linux*.</span><span class="sxs-lookup"><span data-stu-id="746c9-140">Type of operating system for hello VM, either *Windows* or *Linux*.</span></span>
   * <span data-ttu-id="746c9-141">**-f (nebo--seskupování-name)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-141">**-f (or --nic-name)**.</span></span> <span data-ttu-id="746c9-142">Použije se název hello hello síťový adaptér virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="746c9-142">Name of hello NIC hello VM will use.</span></span>
   * <span data-ttu-id="746c9-143">**-i (nebo--název veřejné IP adresy)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-143">**-i (or --public-ip-name)**.</span></span> <span data-ttu-id="746c9-144">Název veřejné IP hello hello virtuální počítač bude používat.</span><span class="sxs-lookup"><span data-stu-id="746c9-144">Name of hello public IP hello VM will use.</span></span>
   * <span data-ttu-id="746c9-145">**-F (nebo--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-145">**-F (or --vnet-name)**.</span></span> <span data-ttu-id="746c9-146">Název hello virtuální síť, kde bude vytvořen hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="746c9-146">Name of hello VNet where hello VM will be created.</span></span>
   * <span data-ttu-id="746c9-147">**-j (nebo--vnet název podsítě)**.</span><span class="sxs-lookup"><span data-stu-id="746c9-147">**-j (or --vnet-subnet-name)**.</span></span> <span data-ttu-id="746c9-148">Název podsítě hello, kde bude vytvořen hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="746c9-148">Name of hello subnet where hello VM will be created.</span></span>

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="746c9-149">Jak tooretrieve statickou privátní IP adresu pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="746c9-149">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="746c9-150">tooview hello statickou privátní IP adresu pro hello virtuální počítač vytvořen pomocí skriptu hello výše, spusťte následující příkaz rozhraní příkazového řádku Azure hello a sledovat hello hodnoty pro *alokační metody privátní IP* a *privátní IP adresa*:</span><span class="sxs-lookup"><span data-stu-id="746c9-150">tooview hello static private IP address information for hello VM created with hello script above, run hello following Azure CLI command and observe hello values for *Private IP alloc-method* and *Private IP address*:</span></span>

    azure vm show -g TestRG -n DNS01

<span data-ttu-id="746c9-151">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="746c9-151">Expected output:</span></span>

    info:    Executing command vm show
    + Looking up hello VM "DNS01"
    + Looking up hello NIC "TestNIC"
    + Looking up hello public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="746c9-152">Jak tooremove statickou privátní IP adresu z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="746c9-152">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="746c9-153">Statickou privátní IP adresu nelze odebrat z síťový adaptér v Azure CLI pro Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="746c9-153">You cannot remove a static private IP address from a NIC in Azure CLI for Resource Manager.</span></span> <span data-ttu-id="746c9-154">Musíte vytvořit novou síťovou kartu, která používá dynamický IP, odeberte hello předchozí síťový adaptér z hello virtuálních počítačů, a poté přidejte hello nové toohello síťový adaptér virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="746c9-154">You must create a new NIC that uses a dynamic IP, remove hello previous NIC from hello VM, and then add hello new NIC toohello VM.</span></span> <span data-ttu-id="746c9-155">toochange hello síťovou kartu pro hello virtuálních počítačů použít int eh příkazů výše, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="746c9-155">toochange hello NIC for hello VM used int eh commands above, follow hello steps below.</span></span>

1. <span data-ttu-id="746c9-156">Spustit hello **síťových adaptérů sítě azure vytvořit** příkaz toocreate nový síťový adaptér pomocí dynamické přidělování IP adres.</span><span class="sxs-lookup"><span data-stu-id="746c9-156">Run hello **azure network nic create** command toocreate a new NIC using dynamic IP allocation.</span></span> <span data-ttu-id="746c9-157">Všimněte si, jak není nutné toospecify hello IP adresu této doby.</span><span class="sxs-lookup"><span data-stu-id="746c9-157">Notice how you do not need toospecify hello IP address this time.</span></span>
   
        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd
   
    <span data-ttu-id="746c9-158">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="746c9-158">Expected output:</span></span>
   
        info:    Executing command network nic create
        + Looking up hello network interface "TestNIC2"
        + Looking up hello subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up hello network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
2. <span data-ttu-id="746c9-159">Spustit hello **sada virtuálních počítačů azure** příkaz toochange hello síťový adaptér používá hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="746c9-159">Run hello **azure vm set** command toochange hello NIC used by hello VM.</span></span>
   
        azure vm set -g TestRG -n DNS01 -N TestNIC2
   
    <span data-ttu-id="746c9-160">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="746c9-160">Expected output:</span></span>
   
        info:    Executing command vm set
        + Looking up hello VM "DNS01"
        + Looking up hello NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK
3. <span data-ttu-id="746c9-161">Pokud je potřeba, spusťte hello **odstranit síťových adaptérů sítě azure** příkaz toodelete hello staré síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="746c9-161">If wanted, run hello **azure network nic delete** command toodelete hello old NIC.</span></span>
   
        azure network nic delete -g TestRG -n TestNIC --quiet
   
    <span data-ttu-id="746c9-162">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="746c9-162">Expected output:</span></span>
   
        info:    Executing command network nic delete
        + Looking up hello network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="746c9-163">Jak tooadd statickou privátní IP adresa tooan existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="746c9-163">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="746c9-164">tooadd statickou privátní IP adresu toohello síťový adaptér používá virtuální počítač vytvořený skript hello výše, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="746c9-164">tooadd a static private IP address toohello NIC used by teh VM created using hello script above, run hello following command:</span></span>

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

<span data-ttu-id="746c9-165">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="746c9-165">Expected output:</span></span>

    info:    Executing command network nic set
    + Looking up hello network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up hello network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a><span data-ttu-id="746c9-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="746c9-166">Next steps</span></span>
* <span data-ttu-id="746c9-167">Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="746c9-167">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="746c9-168">Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="746c9-168">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="746c9-169">Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="746c9-169">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

