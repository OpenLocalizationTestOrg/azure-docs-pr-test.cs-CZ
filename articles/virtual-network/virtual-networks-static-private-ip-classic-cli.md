---
title: "Nakonfigurovat privátní IP adresy pro virtuální počítače (klasické) - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat privátní IP adresy pro virtuální počítače (klasické) pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17386acf-c708-4103-9b22-ff9bf04b778d
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ed0fe2fea20671063395b9ff089599853278989d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-cli-10"></a><span data-ttu-id="c63a7-103">Nakonfigurovat privátní IP adresy pro virtuální počítač (klasický) pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c63a7-103">Configure private IP addresses for a virtual machine (Classic) using the Azure CLI 1.0</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="c63a7-104">Tento článek se týká modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="c63a7-104">This article covers the classic deployment model.</span></span> <span data-ttu-id="c63a7-105">Můžete také [spravovat statickou privátní IP adresou v modelu nasazení Resource Manager](virtual-networks-static-private-ip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c63a7-105">You can also [manage a static private IP address in the Resource Manager deployment model](virtual-networks-static-private-ip-arm-cli.md).</span></span>

<span data-ttu-id="c63a7-106">Níže uvedené příkazy rozhraní příkazového řádku Azure ukázka očekávat jednoduché prostředí již vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="c63a7-106">The sample Azure CLI commands below expect a simple environment already created.</span></span> <span data-ttu-id="c63a7-107">Pokud chcete ke spuštění příkazů, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c63a7-107">If you want to run the commands as they are displayed in this document, first build the test environment described in [create a vnet](virtual-networks-create-vnet-classic-cli.md).</span></span>

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="c63a7-108">Postup při vytváření virtuálního počítače zadat statickou privátní IP adresu</span><span class="sxs-lookup"><span data-stu-id="c63a7-108">How to specify a static private IP address when creating a VM</span></span>
<span data-ttu-id="c63a7-109">Chcete-li vytvořit nový virtuální počítač s názvem *DNS01* v novou cloudovou službu s názvem *TestService* závislosti na scénáři výše, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="c63a7-109">To create a new VM named *DNS01* in a new cloud service named *TestService* based on the scenario above, follow these steps:</span></span>

1. <span data-ttu-id="c63a7-110">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="c63a7-110">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="c63a7-111">Spustit **vytvoření azure služba** příkaz pro vytvoření cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c63a7-111">Run the **azure service create** command to create the cloud service.</span></span>
   
        azure service create TestService --location uscentral
   
    <span data-ttu-id="c63a7-112">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="c63a7-112">Expected output:</span></span>
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. <span data-ttu-id="c63a7-113">Spustit **azure vytvořit virtuální počítač** příkaz pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c63a7-113">Run the **azure create vm** command to create the VM.</span></span> <span data-ttu-id="c63a7-114">Všimněte si, hodnota privátní statické IP adresy.</span><span class="sxs-lookup"><span data-stu-id="c63a7-114">Notice the value for a static private IP address.</span></span> <span data-ttu-id="c63a7-115">Seznam uvedený za výstupem vysvětluje použité parametry.</span><span class="sxs-lookup"><span data-stu-id="c63a7-115">The list shown after the output explains the parameters used.</span></span>
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    <span data-ttu-id="c63a7-116">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="c63a7-116">Expected output:</span></span>
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * <span data-ttu-id="c63a7-117">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="c63a7-117">**-l (or --location)**.</span></span> <span data-ttu-id="c63a7-118">Oblast Azure, kde bude vytvořen virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c63a7-118">Azure region where the VM will be created.</span></span> <span data-ttu-id="c63a7-119">V našem scénáři je to *centralus*.</span><span class="sxs-lookup"><span data-stu-id="c63a7-119">For our scenario, *centralus*.</span></span>
   * <span data-ttu-id="c63a7-120">**-n (nebo název virtuálního počítače –)**.</span><span class="sxs-lookup"><span data-stu-id="c63a7-120">**-n (or --vm-name)**.</span></span> <span data-ttu-id="c63a7-121">Název virtuálního počítače, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="c63a7-121">Name of the VM to be created.</span></span>
   * <span data-ttu-id="c63a7-122">**-w (nebo--virtuální síťový název)**.</span><span class="sxs-lookup"><span data-stu-id="c63a7-122">**-w (or --virtual-network-name)**.</span></span> <span data-ttu-id="c63a7-123">Název sítě VNet, kde bude vytvořen virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c63a7-123">Name of the VNet where the VM will be created.</span></span> 
   * <span data-ttu-id="c63a7-124">**-S (nebo--statické ip)**.</span><span class="sxs-lookup"><span data-stu-id="c63a7-124">**-S (or --static-ip)**.</span></span> <span data-ttu-id="c63a7-125">Statickou privátní IP adresu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c63a7-125">Static private IP address for the VM.</span></span>
   * <span data-ttu-id="c63a7-126">**TestService**.</span><span class="sxs-lookup"><span data-stu-id="c63a7-126">**TestService**.</span></span> <span data-ttu-id="c63a7-127">Název cloudové služby, kde bude vytvořen virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c63a7-127">Name of the cloud service where the VM will be created.</span></span>
   * <span data-ttu-id="c63a7-128">**bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**.</span><span class="sxs-lookup"><span data-stu-id="c63a7-128">**bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**.</span></span> <span data-ttu-id="c63a7-129">Bitová kopie používaná k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c63a7-129">Image used to create the VM.</span></span>
   * <span data-ttu-id="c63a7-130">**AdminUser**.</span><span class="sxs-lookup"><span data-stu-id="c63a7-130">**adminuser**.</span></span> <span data-ttu-id="c63a7-131">Místní správce pro virtuální počítač s Windows.</span><span class="sxs-lookup"><span data-stu-id="c63a7-131">Local administrator for the Windows VM.</span></span>
   * <span data-ttu-id="c63a7-132">**AdminP@ssw0rd**.</span><span class="sxs-lookup"><span data-stu-id="c63a7-132">**AdminP@ssw0rd**.</span></span> <span data-ttu-id="c63a7-133">Heslo místního správce pro virtuální počítač Windows.</span><span class="sxs-lookup"><span data-stu-id="c63a7-133">Local administrator password for the Windows VM.</span></span>

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="c63a7-134">Jak načíst statickou privátní IP adresu informace pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="c63a7-134">How to retrieve static private IP address information for a VM</span></span>
<span data-ttu-id="c63a7-135">Chcete-li zobrazit statické soukromé informace IP adresu pro virtuální počítač vytvořený pomocí skriptu pro výše uvedené, spuštěním následujícího příkazu příkazového řádku Azure CLI a zkontrolujte, jestli hodnota pro *sítě StaticIP*:</span><span class="sxs-lookup"><span data-stu-id="c63a7-135">To view the static private IP address information for the VM created with the script above, run the following Azure CLI command and observe the value for *Network StaticIP*:</span></span>

    azure vm static-ip show DNS01

<span data-ttu-id="c63a7-136">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="c63a7-136">Expected output:</span></span>

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="c63a7-137">Postup odebrání statickou privátní IP adresu z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c63a7-137">How to remove a static private IP address from a VM</span></span>
<span data-ttu-id="c63a7-138">Odeberte statickou privátní IP adresu přidat do virtuálního počítače ve skriptu výše, spusťte následující příkaz rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="c63a7-138">To remove the static private IP address added to the VM in the script above, run the following Azure CLI command:</span></span>

    azure vm static-ip remove DNS01

<span data-ttu-id="c63a7-139">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="c63a7-139">Expected output:</span></span>

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a><span data-ttu-id="c63a7-140">Postup přidání statickou privátní IP adresu do stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c63a7-140">How to add a static private IP to an existing VM</span></span>
<span data-ttu-id="c63a7-141">Chcete-li přidat statického privátní IP adresu, které se virtuální počítač vytvořený skript výše, o následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c63a7-141">To add a static private IP address to the VM created using the script above, runt he following command:</span></span>

    azure vm static-ip set DNS01 192.168.1.101

<span data-ttu-id="c63a7-142">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="c63a7-142">Expected output:</span></span>

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a><span data-ttu-id="c63a7-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c63a7-143">Next steps</span></span>
* <span data-ttu-id="c63a7-144">Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="c63a7-144">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="c63a7-145">Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="c63a7-145">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="c63a7-146">Obrátit [vyhrazené IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="c63a7-146">Consult the [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

