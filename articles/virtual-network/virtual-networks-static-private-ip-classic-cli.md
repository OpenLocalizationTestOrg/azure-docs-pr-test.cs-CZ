---
title: "aaaConfigure privátních IP adres pro virtuální počítače (klasické) - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooconfigure privátních IP adres pro virtuální počítače (klasické) pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
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
ms.openlocfilehash: 417a57181bcf5c2e6101bf3bdf63fc94ebc99df5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-cli-10"></a><span data-ttu-id="7b8ae-103">Nakonfigurovat privátní IP adresy pro virtuální počítač (klasický) pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="7b8ae-103">Configure private IP addresses for a virtual machine (Classic) using hello Azure CLI 1.0</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="7b8ae-104">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="7b8ae-105">Můžete také [spravovat statickou privátní IP adresou v modelu nasazení Resource Manager hello](virtual-networks-static-private-ip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7b8ae-105">You can also [manage a static private IP address in hello Resource Manager deployment model](virtual-networks-static-private-ip-arm-cli.md).</span></span>

<span data-ttu-id="7b8ae-106">níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-106">hello sample Azure CLI commands below expect a simple environment already created.</span></span> <span data-ttu-id="7b8ae-107">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7b8ae-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-classic-cli.md).</span></span>

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="7b8ae-108">Jak toospecify statickou privátní IP adresy při vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7b8ae-108">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="7b8ae-109">toocreate nový virtuální počítač s názvem *DNS01* v novou cloudovou službu s názvem *TestService* podle hello scénář výše, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="7b8ae-109">toocreate a new VM named *DNS01* in a new cloud service named *TestService* based on hello scenario above, follow these steps:</span></span>

1. <span data-ttu-id="7b8ae-110">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-110">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="7b8ae-111">Spustit hello **vytvoření azure služba** příkaz toocreate hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-111">Run hello **azure service create** command toocreate hello cloud service.</span></span>
   
        azure service create TestService --location uscentral
   
    <span data-ttu-id="7b8ae-112">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="7b8ae-112">Expected output:</span></span>
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. <span data-ttu-id="7b8ae-113">Spustit hello **azure vytvořit virtuální počítač** příkaz toocreate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-113">Run hello **azure create vm** command toocreate hello VM.</span></span> <span data-ttu-id="7b8ae-114">Všimněte si, hodnota hello se statickou privátní IP adresou.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-114">Notice hello value for a static private IP address.</span></span> <span data-ttu-id="7b8ae-115">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-115">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    <span data-ttu-id="7b8ae-116">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="7b8ae-116">Expected output:</span></span>
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting too"Small".
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
   
   * <span data-ttu-id="7b8ae-117">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-117">**-l (or --location)**.</span></span> <span data-ttu-id="7b8ae-118">Oblast Azure, kde bude vytvořen hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-118">Azure region where hello VM will be created.</span></span> <span data-ttu-id="7b8ae-119">V našem scénáři je to *centralus*.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-119">For our scenario, *centralus*.</span></span>
   * <span data-ttu-id="7b8ae-120">**-n (nebo název virtuálního počítače –)**.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-120">**-n (or --vm-name)**.</span></span> <span data-ttu-id="7b8ae-121">Název toobe hello virtuálních počítačů vytvořena.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-121">Name of hello VM toobe created.</span></span>
   * <span data-ttu-id="7b8ae-122">**-w (nebo--virtuální síťový název)**.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-122">**-w (or --virtual-network-name)**.</span></span> <span data-ttu-id="7b8ae-123">Název hello virtuální síť, kde bude vytvořen hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-123">Name of hello VNet where hello VM will be created.</span></span> 
   * <span data-ttu-id="7b8ae-124">**-S (nebo--statické ip)**.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-124">**-S (or --static-ip)**.</span></span> <span data-ttu-id="7b8ae-125">Statickou privátní IP adresou pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-125">Static private IP address for hello VM.</span></span>
   * <span data-ttu-id="7b8ae-126">**TestService**.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-126">**TestService**.</span></span> <span data-ttu-id="7b8ae-127">Název hello cloudové služby, kde bude vytvořen hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-127">Name of hello cloud service where hello VM will be created.</span></span>
   * <span data-ttu-id="7b8ae-128">**bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-128">**bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**.</span></span> <span data-ttu-id="7b8ae-129">Bitová kopie používaná toocreate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-129">Image used toocreate hello VM.</span></span>
   * <span data-ttu-id="7b8ae-130">**AdminUser**.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-130">**adminuser**.</span></span> <span data-ttu-id="7b8ae-131">Místní správce pro virtuální počítač s Windows hello.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-131">Local administrator for hello Windows VM.</span></span>
   * <span data-ttu-id="7b8ae-132">**AdminP@ssw0rd**.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-132">**AdminP@ssw0rd**.</span></span> <span data-ttu-id="7b8ae-133">Heslo místního správce pro virtuální počítač s Windows hello.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-133">Local administrator password for hello Windows VM.</span></span>

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="7b8ae-134">Jak tooretrieve statickou privátní IP adresu pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="7b8ae-134">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="7b8ae-135">tooview hello statickou privátní IP adresu pro hello virtuální počítač vytvořen pomocí skriptu hello výše, spusťte následující příkaz rozhraní příkazového řádku Azure hello a sledovat hello hodnotu *sítě StaticIP*:</span><span class="sxs-lookup"><span data-stu-id="7b8ae-135">tooview hello static private IP address information for hello VM created with hello script above, run hello following Azure CLI command and observe hello value for *Network StaticIP*:</span></span>

    azure vm static-ip show DNS01

<span data-ttu-id="7b8ae-136">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="7b8ae-136">Expected output:</span></span>

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="7b8ae-137">Jak tooremove statickou privátní IP adresu z virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7b8ae-137">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="7b8ae-138">tooremove hello statickou privátní IP adresou přidána toohello virtuálních počítačů ve skriptu hello nad spuštění hello následující příkaz rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="7b8ae-138">tooremove hello static private IP address added toohello VM in hello script above, run hello following Azure CLI command:</span></span>

    azure vm static-ip remove DNS01

<span data-ttu-id="7b8ae-139">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="7b8ae-139">Expected output:</span></span>

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-tooadd-a-static-private-ip-tooan-existing-vm"></a><span data-ttu-id="7b8ae-140">Jak tooadd statickou privátní tooan IP existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="7b8ae-140">How tooadd a static private IP tooan existing VM</span></span>
<span data-ttu-id="7b8ae-141">tooadd statickou privátní IP adresu toohello virtuálních počítačů, které jsou vytvořené pomocí skriptu hello výše, o následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7b8ae-141">tooadd a static private IP address toohello VM created using hello script above, runt he following command:</span></span>

    azure vm static-ip set DNS01 192.168.1.101

<span data-ttu-id="7b8ae-142">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="7b8ae-142">Expected output:</span></span>

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a><span data-ttu-id="7b8ae-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7b8ae-143">Next steps</span></span>
* <span data-ttu-id="7b8ae-144">Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-144">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="7b8ae-145">Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="7b8ae-145">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="7b8ae-146">Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="7b8ae-146">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

