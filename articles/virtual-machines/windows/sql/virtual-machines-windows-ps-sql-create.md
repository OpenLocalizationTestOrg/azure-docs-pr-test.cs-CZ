---
title: "aaaCreate virtuálního počítače systému SQL Server v prostředí Azure PowerShell (Resource Manager) | Microsoft Docs"
description: "Obsahuje kroky a skriptů prostředí PowerShell pro vytvoření virtuálního počítače Azure s obrázky Galerie virtuálního počítače systému SQL Server."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/17/2017
ms.author: jroth
ms.openlocfilehash: 2b8cb8f69ff9894a95eab617816a60c8674eeefa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a><span data-ttu-id="3318c-103">Zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="3318c-103">Provision a SQL Server virtual machine using Azure PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3318c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3318c-104">Portal</span></span>](virtual-machines-windows-portal-sql-server-provision.md)
> * [<span data-ttu-id="3318c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3318c-105">PowerShell</span></span>](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a><span data-ttu-id="3318c-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="3318c-106">Overview</span></span>
<span data-ttu-id="3318c-107">Tento kurz ukazuje, jak toocreate jednoho virtuálního počítače Azure pomocí hello **Azure Resource Manager** modelu nasazení pomocí rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3318c-107">This tutorial shows you how toocreate a single Azure virtual machine using hello **Azure Resource Manager** deployment model using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="3318c-108">V tomto kurzu vytvoříme jednoho virtuálního počítače pomocí jednoho disku z bitové kopie v hello SQL galerie.</span><span class="sxs-lookup"><span data-stu-id="3318c-108">In this tutorial, we will create a single virtual machine using a single disk drive from an image in hello SQL Gallery.</span></span> <span data-ttu-id="3318c-109">Vytvoříme nové zprostředkovatele pro hello úložiště, síť a výpočetní prostředky, které se použijí hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3318c-109">We will create new providers for hello storage, network, and compute resources that will be used by hello virtual machine.</span></span> <span data-ttu-id="3318c-110">Pokud máte existující zprostředkovatele pro kterýkoli z těchto prostředků, můžete místo toho použít těchto zprostředkovatelů.</span><span class="sxs-lookup"><span data-stu-id="3318c-110">If you have existing providers for any of these resources, you can use those providers instead.</span></span>

<span data-ttu-id="3318c-111">Pokud třeba hello classic verzi tohoto tématu, přečtěte si téma [zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell Classic](../classic/ps-sql-create.md).</span><span class="sxs-lookup"><span data-stu-id="3318c-111">If you need hello classic version of this topic, see [Provision a SQL Server virtual machine using Azure PowerShell Classic](../classic/ps-sql-create.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3318c-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3318c-112">Prerequisites</span></span>
<span data-ttu-id="3318c-113">Pro účely tohoto kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="3318c-113">For this tutorial you'll need:</span></span>

* <span data-ttu-id="3318c-114">Účet Azure a předplatné před zahájením.</span><span class="sxs-lookup"><span data-stu-id="3318c-114">An Azure account and subscription before you start.</span></span> <span data-ttu-id="3318c-115">Pokud nemáte, si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3318c-115">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="3318c-116">[Azure PowerShell)](/powershell/azure/overview), minimální verze 1.4.0 nebo novější (Tento kurz vytvořené pomocí verze 1.5.0).</span><span class="sxs-lookup"><span data-stu-id="3318c-116">[Azure PowerShell)](/powershell/azure/overview), minimum version of 1.4.0 or later (this tutorial written using version 1.5.0).</span></span>
  * <span data-ttu-id="3318c-117">tooretrieve verze, typ **Azure Get-Module - ListAvailable**.</span><span class="sxs-lookup"><span data-stu-id="3318c-117">tooretrieve your version, type **Get-Module Azure -ListAvailable**.</span></span>

## <a name="configure-your-subscription"></a><span data-ttu-id="3318c-118">Konfigurovat předplatné</span><span class="sxs-lookup"><span data-stu-id="3318c-118">Configure your subscription</span></span>
<span data-ttu-id="3318c-119">Otevřete prostředí Windows PowerShell a spuštěním následující rutiny hello vytvořit tooyour přístup k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="3318c-119">Open Windows PowerShell and establish access tooyour Azure account by running hello following cmdlet.</span></span> <span data-ttu-id="3318c-120">Zobrazí se přihlašovací obrazovka tooenter přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="3318c-120">You will be presented with a sign in screen tooenter your credentials.</span></span> <span data-ttu-id="3318c-121">Použití hello stejné e-mailu a heslo použít toosign v toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3318c-121">Use hello same email and password that you use toosign in toohello Azure portal.</span></span>

    Add-AzureRmAccount

<span data-ttu-id="3318c-122">Po úspěšném přihlášení se zobrazí některé informace na obrazovce, která zahrnuje hello ID předplatného, se kterým jste přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3318c-122">After successfully signing in you will see some information on screen that includes hello SubscriptionId with which you signed in.</span></span> <span data-ttu-id="3318c-123">Toto je hello předplatné, ve kterém se vytvoří hello prostředky pro tento kurz nezměníte tooa jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="3318c-123">This is hello subscription in which hello resources for this tutorial will be created unless you change tooa different subscription.</span></span> <span data-ttu-id="3318c-124">Pokud máte více SubscriptionIds, spusťte následující rutinu tooreturn hello seznam všech vaší SubscriptionIds:</span><span class="sxs-lookup"><span data-stu-id="3318c-124">If you have multiple SubscriptionIds, run hello following cmdlet tooreturn a list of all of your SubscriptionIds:</span></span>

    Get-AzureRmSubscription

<span data-ttu-id="3318c-125">tooanother toochange SubscriptionID, spusťte následující rutinu s vaší požadované ID předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-125">toochange tooanother SubscriptionID, run hello following cmdlet with your desired SubscriptionId.</span></span>

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a><span data-ttu-id="3318c-126">Definování proměnné bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="3318c-126">Define image variables</span></span>
<span data-ttu-id="3318c-127">toosimplify použitelnost a vysvětlení skriptu hello dokončení tohoto kurzu, spustíme definováním počet proměnných.</span><span class="sxs-lookup"><span data-stu-id="3318c-127">toosimplify usability and understanding of hello completed script from this tutorial, we will start by defining a number of variables.</span></span> <span data-ttu-id="3318c-128">Hodnoty parametru hello změnit podle svých potřeb, ale pozor pojmenování délky související tooname omezení a speciální znaky, při úpravě hello hodnoty poskytnuté.</span><span class="sxs-lookup"><span data-stu-id="3318c-128">Change hello parameter values as you see fit, but beware of naming restrictions related tooname lengths and special characters when modifying hello values provided.</span></span>

### <a name="location-and-resource-group"></a><span data-ttu-id="3318c-129">Umístění a skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="3318c-129">Location and Resource Group</span></span>
<span data-ttu-id="3318c-130">Použití dvou proměnných toodefine hello data oblast a hello skupiny prostředků do které chcete vytvořit hello další prostředky pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-130">Use two variables toodefine hello data region and hello resource group into which you will create hello other resources for hello virtual machine.</span></span>

<span data-ttu-id="3318c-131">Podle potřeby a potom spusťte následující rutiny tooinitialize hello tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="3318c-131">Modify as desired and then execute hello following cmdlets tooinitialize these variables.</span></span>

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a><span data-ttu-id="3318c-132">Úložiště vlastností</span><span class="sxs-lookup"><span data-stu-id="3318c-132">Storage properties</span></span>
<span data-ttu-id="3318c-133">Použijte následující proměnné toodefine hello úložiště účet a hello typ úložiště toobe používané virtuálním počítačem hello hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-133">Use hello following variables toodefine hello storage account and hello type of storage toobe used by hello virtual machine.</span></span>

<span data-ttu-id="3318c-134">Podle potřeby a potom spusťte následující rutinu tooinitialize hello tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="3318c-134">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span> <span data-ttu-id="3318c-135">Všimněte si, že v tomto příkladu používáme [Storage úrovně Premium](../../../storage/common/storage-premium-storage.md), který se doporučuje pro úlohy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3318c-135">Note that in this example, we are using [Premium Storage](../../../storage/common/storage-premium-storage.md), which is recommended for production workloads.</span></span> <span data-ttu-id="3318c-136">Podrobnosti na tyto pokyny a dalších doporučeních najdete v tématu [osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-performance.md).</span><span class="sxs-lookup"><span data-stu-id="3318c-136">For details on this guidance and other recommendations, see [Performance best practices for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).</span></span>

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a><span data-ttu-id="3318c-137">Vlastnosti sítě</span><span class="sxs-lookup"><span data-stu-id="3318c-137">Network properties</span></span>
<span data-ttu-id="3318c-138">Použijte následující proměnné toodefine hello síťové rozhraní, metoda přidělení hello TCP/IP, hello název virtuální sítě, název hello virtuální podsítě, hello rozsah IP adres pro virtuální síť hello, hello rozsah IP adres pro podsíť hello a hello hello Popisek toobe název veřejné domény používané hello sítí v hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3318c-138">Use hello following variables toodefine hello network interface, hello TCP/IP allocation method, hello virtual network name, hello virtual subnet name, hello range of IP addresses for hello virtual network, hello range of IP addresses for hello subnet, and hello public domain name label toobe used by hello network in hello virtual machine.</span></span>

<span data-ttu-id="3318c-139">Podle potřeby a potom spusťte následující rutinu tooinitialize hello tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="3318c-139">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span>

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"

### <a name="virtual-machine-properties"></a><span data-ttu-id="3318c-140">Vlastnosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3318c-140">Virtual machine properties</span></span>
<span data-ttu-id="3318c-141">Použijte název proměnné toodefine hello virtuálního počítače, název počítače hello, hello velikost virtuálního počítače a hello název disku operačního systému pro virtuální počítač hello hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-141">Use hello following variables toodefine hello virtual machine name, hello computer name, hello virtual machine size, and hello operating system disk name for hello virtual machine.</span></span>

<span data-ttu-id="3318c-142">Podle potřeby a potom spusťte následující rutinu tooinitialize hello tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="3318c-142">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span>

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a><span data-ttu-id="3318c-143">Vlastnosti bitové kopie</span><span class="sxs-lookup"><span data-stu-id="3318c-143">Image properties</span></span>
<span data-ttu-id="3318c-144">Použijte následující proměnné toodefine hello image toouse pro virtuální počítač hello hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-144">Use hello following variables toodefine hello image toouse for hello virtual machine.</span></span> <span data-ttu-id="3318c-145">V tomto příkladu se používá SQL Server 2016 Enterprise hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="3318c-145">In this example, hello SQL Server 2016 Enterprise image is used.</span></span>

<span data-ttu-id="3318c-146">Podle potřeby a potom spusťte následující rutinu tooinitialize hello tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="3318c-146">Modify as desired and then execute hello following cmdlet tooinitialize these variables.</span></span>

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

<span data-ttu-id="3318c-147">Všimněte si, že můžete získat úplný seznam nabídky bitovou kopii systému SQL Server pomocí příkazu Get-AzureRmVMImageOffer hello:</span><span class="sxs-lookup"><span data-stu-id="3318c-147">Note that you can get a full list of SQL Server image offerings with hello Get-AzureRmVMImageOffer command:</span></span>

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

<span data-ttu-id="3318c-148">A uvidíte hello SKU, které jsou k dispozici pro nabídky pomocí příkazu Get-AzureRmVMImageSku hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-148">And you can see hello Skus available for an offering with hello Get-AzureRmVMImageSku command.</span></span> <span data-ttu-id="3318c-149">Hello následující příkaz ukazuje všech skladových položek k dispozici pro hello **SQL2014SP1 WS2012R2** nabízejí.</span><span class="sxs-lookup"><span data-stu-id="3318c-149">hello following command shows all Skus available for hello **SQL2014SP1-WS2012R2** offer.</span></span>

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a><span data-ttu-id="3318c-150">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="3318c-150">Create a resource group</span></span>
<span data-ttu-id="3318c-151">Pomocí modelu nasazení Resource Manager hello je hello první objekt, který vytvoříte skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-151">With hello Resource Manager deployment model, hello first object that you create is hello resource group.</span></span> <span data-ttu-id="3318c-152">Budeme používat hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) toocreate rutiny skupinu prostředků Azure a jeho prostředků s prostředky hello skupina název a umístění definované hello proměnné, které jste dříve inicializován.</span><span class="sxs-lookup"><span data-stu-id="3318c-152">We will use hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet toocreate an Azure resource group and its resources with hello resource group name and location defined by hello variables that you previously initialized.</span></span>

<span data-ttu-id="3318c-153">Spusťte následující rutinu toocreate hello nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="3318c-153">Execute hello following cmdlet toocreate your new resource group.</span></span>

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a><span data-ttu-id="3318c-154">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="3318c-154">Create a storage account</span></span>
<span data-ttu-id="3318c-155">Hello virtuální počítač vyžaduje prostředků úložiště pro disk operačního systému hello a hello dat systému SQL Server a soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="3318c-155">hello virtual machine requires storage resources for hello operating system disk and for hello SQL Server data and log files.</span></span> <span data-ttu-id="3318c-156">Pro jednoduchost vytvoříme jediný disk pro obojí.</span><span class="sxs-lookup"><span data-stu-id="3318c-156">For simplicity, we will create a single disk for both.</span></span> <span data-ttu-id="3318c-157">Můžete připojit další disky později pomocí hello [disku Azure přidat](/powershell/module/azure/add-azuredisk) rutinu v pořadí tooplace data systému SQL Server a soubory protokolu na vyhrazených disků.</span><span class="sxs-lookup"><span data-stu-id="3318c-157">You can attach additional disks later using hello [Add-Azure Disk](/powershell/module/azure/add-azuredisk) cmdlet in order tooplace your SQL Server data and log files on dedicated disks.</span></span> <span data-ttu-id="3318c-158">Budeme používat hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny toocreate standardní úložiště účtů v nové skupiny prostředků a s názvem účtu úložiště hello, název Sku úložiště a umístění definovat pomocí hello proměnné, které dříve inicializován.</span><span class="sxs-lookup"><span data-stu-id="3318c-158">We will use hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet toocreate a standard storage account in your new resource group and with hello storage account name, storage Sku name, and location defined using hello variables that you previously initialized.</span></span>

<span data-ttu-id="3318c-159">Spusťte následující rutinu toocreate hello váš nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3318c-159">Execute hello following cmdlet toocreate your new storage account.</span></span>

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a><span data-ttu-id="3318c-160">Vytvoření síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="3318c-160">Create network resources</span></span>
<span data-ttu-id="3318c-161">Hello virtuální počítač vyžaduje pro připojení k síti číslo síťových prostředků.</span><span class="sxs-lookup"><span data-stu-id="3318c-161">hello virtual machine requires a number of network resources for network connectivity.</span></span>

* <span data-ttu-id="3318c-162">Každý virtuální počítač vyžaduje virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="3318c-162">Each virtual machine requires a virtual network.</span></span>
* <span data-ttu-id="3318c-163">Virtuální síť musí mít alespoň jednu podsíť definované.</span><span class="sxs-lookup"><span data-stu-id="3318c-163">A virtual network must have at least one subnet defined.</span></span>
* <span data-ttu-id="3318c-164">Síťové rozhraní musí být definovány se veřejné nebo privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="3318c-164">A network interface must be defined with either a public or a private IP address.</span></span>

### <a name="create-a-virtual-network-subnet-configuration"></a><span data-ttu-id="3318c-165">Vytvoření konfigurace podsítě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="3318c-165">Create a virtual network subnet configuration</span></span>
<span data-ttu-id="3318c-166">Spustíme vytvořením konfigurace pro naše virtuální síť s podsítí.</span><span class="sxs-lookup"><span data-stu-id="3318c-166">We will start by creating a subnet configuration for our virtual network.</span></span> <span data-ttu-id="3318c-167">V našem kurzu vytvoříme výchozí podsíť pomocí hello [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3318c-167">For our tutorial, we will create a default subnet using hello [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet.</span></span> <span data-ttu-id="3318c-168">Pomocí názvu a adresy předpona serveru hello podsítě definovat pomocí hello proměnné, které jste dříve inicializovat vytvoříme naše konfigurace podsítě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3318c-168">We will create our virtual network subnet configuration with hello subnet name and address prefix defined using hello variables that you previously initialized.</span></span>

> [!NOTE]
> <span data-ttu-id="3318c-169">Můžete definovat další vlastnosti konfigurace podsítě virtuální sítě hello použití této rutiny, ale který je nad rámec tohoto návodu hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-169">You can define additional properties of hello virtual network subnet configuration using this cmdlet, but that is beyond hello scope of this tutorial.</span></span>
>
>

<span data-ttu-id="3318c-170">Spusťte následující rutinu toocreate hello konfiguraci virtuální podsítě.</span><span class="sxs-lookup"><span data-stu-id="3318c-170">Execute hello following cmdlet toocreate your virtual subnet configuration.</span></span>

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a><span data-ttu-id="3318c-171">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="3318c-171">Create a virtual network</span></span>
<span data-ttu-id="3318c-172">V dalším kroku vytvoříme naše virtuální sítě pomocí hello [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3318c-172">Next, we will create our virtual network using hello [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet.</span></span> <span data-ttu-id="3318c-173">V nové skupiny prostředků, hello název, umístění a předpona adresy definované pomocí hello proměnné, které jste dříve inicializovat a použití konfigurace hello podsítě, který jste zadali v předchozím kroku hello vytvoříme naše virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3318c-173">We will create our virtual network in your new resource group, with hello name, location, and address prefix defined using hello variables that you previously initialized, and using hello subnet configuration that you defined in hello previous step.</span></span>

<span data-ttu-id="3318c-174">Spusťte následující rutiny toocreate hello vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3318c-174">Execute hello following cmdlet toocreate your virtual network.</span></span>

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-hello-public-ip-address"></a><span data-ttu-id="3318c-175">Vytvoření veřejné IP adresy hello</span><span class="sxs-lookup"><span data-stu-id="3318c-175">Create hello public IP address</span></span>
<span data-ttu-id="3318c-176">Teď, když máme naše virtuální sítě definované, potřebujeme tooconfigure IP adresu pro připojení k toohello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3318c-176">Now that we have our virtual network defined, we need tooconfigure an IP address for connectivity toohello virtual machine.</span></span> <span data-ttu-id="3318c-177">V tomto kurzu vytvoříme veřejnou IP adresu pomocí dynamických IP adresování toosupport připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="3318c-177">For this tutorial, we will create a public IP address using dynamic IP addressing toosupport Internet connectivity.</span></span> <span data-ttu-id="3318c-178">Budeme používat hello [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) rutiny toocreate hello veřejnou IP adresu v hello prostředků skupina vytvořená prevously a hello název, umístění, metoda přidělení a popisek názvu domény DNS je definovat pomocí hello proměnné, které jste dříve inicializován.</span><span class="sxs-lookup"><span data-stu-id="3318c-178">We will use hello [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet toocreate hello public IP address in hello resource group created prevously and with hello name, location, allocation method, and DNS domain name label defined using hello variables that you previously initialized.</span></span>

> [!NOTE]
> <span data-ttu-id="3318c-179">Můžete definovat další vlastnosti hello veřejné IP adresy pomocí této rutiny, ale který je mimo obor hello počáteční kurzu.</span><span class="sxs-lookup"><span data-stu-id="3318c-179">You can define additional properties of hello public IP address using this cmdlet, but that is beyond hello scope of this initial tutorial.</span></span> <span data-ttu-id="3318c-180">Můžete také vytvořit privátní adresu nebo adresu se statickou adresou, ale který je také nad rámec tohoto návodu hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-180">You could also create a private address or an address with a static address, but that is also beyond hello scope of this tutorial.</span></span>
>
>

<span data-ttu-id="3318c-181">Spusťte následující rutinu toocreate hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="3318c-181">Execute hello following cmdlet toocreate your public IP address.</span></span>

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-hello-network-interface"></a><span data-ttu-id="3318c-182">Vytvoření hello síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="3318c-182">Create hello network interface</span></span>
<span data-ttu-id="3318c-183">Snažíme se teď připravena toocreate hello síťové rozhraní, které budou používat naše virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3318c-183">We are now ready toocreate hello network interface that our virtual machine will use.</span></span> <span data-ttu-id="3318c-184">Budeme používat hello [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) rutiny toocreate naše rozhraní sítě ve skupině prostředků hello dříve vytvořili a hello název, umístění, podsíť a veřejnou IP adresu předtím definovaný.</span><span class="sxs-lookup"><span data-stu-id="3318c-184">We will use hello [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet toocreate our network interface in hello resource group created earlier and with hello name, location, subnet and public IP address previously defined.</span></span>

<span data-ttu-id="3318c-185">Spusťte následující rutinu toocreate hello síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3318c-185">Execute hello following cmdlet toocreate your network interface.</span></span>

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a><span data-ttu-id="3318c-186">Konfigurace objektu virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3318c-186">Configure a VM object</span></span>
<span data-ttu-id="3318c-187">Teď, když máme úložiště a síťové prostředky, které jsou definované, jsme připravené toodefine výpočetní prostředky pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-187">Now that we have storage and network resources defined, we are ready toodefine compute resources for hello virtual machine.</span></span> <span data-ttu-id="3318c-188">V našem kurzu jsme se zadejte hello velikost virtuálního počítače a různé vlastnosti operačního systému, zadejte hello síťové rozhraní, které jsme dříve vytvořili, definovat úložiště objektů blob a pak zadejte hello disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3318c-188">For our tutorial, we will specify hello virtual machine size and various operating system properties, specify hello network interface that we previously created, define blob storage, and then specify hello operating system disk.</span></span>

### <a name="create-hello-vm-object"></a><span data-ttu-id="3318c-189">Vytvořit objekt virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="3318c-189">Create hello VM object</span></span>
<span data-ttu-id="3318c-190">Spustíme zadáním hello velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3318c-190">We will start by specifying hello virtual machine size.</span></span> <span data-ttu-id="3318c-191">V tomto kurzu zadáváme DS13.</span><span class="sxs-lookup"><span data-stu-id="3318c-191">For this tutorial, we are specifying a DS13.</span></span> <span data-ttu-id="3318c-192">Budeme používat hello [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) rutiny toocreate objekt konfigurovat virtuální počítač s hello názvem a velikostí definovat pomocí hello proměnné, které jste dříve inicializován.</span><span class="sxs-lookup"><span data-stu-id="3318c-192">We will use hello [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet toocreate a configurable virtual machine object with hello name and size defined using hello variables that you previously initialized.</span></span>

<span data-ttu-id="3318c-193">Spusťte následující rutiny toocreate hello virtuálního počítače objekt hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-193">Execute hello following cmdlet toocreate hello virtual machine object.</span></span>

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-toohold-hello-name-and-password-for-hello-local-administrator-credentials"></a><span data-ttu-id="3318c-194">Vytvořte přihlašovací údaje objektu toohold hello jméno a heslo pro přihlašovací údaje místního správce hello</span><span class="sxs-lookup"><span data-stu-id="3318c-194">Create a credential object toohold hello name and password for hello local administrator credentials</span></span>
<span data-ttu-id="3318c-195">Než budeme moct nastavit hello vlastnosti operačního systému pro virtuální počítač hello, potřebujeme toosupply hello pověření pro účet místního správce hello jako zabezpečený řetězec.</span><span class="sxs-lookup"><span data-stu-id="3318c-195">Before we can set hello operating system properties for hello virtual machine, we need toosupply hello credentials for hello local administrator account as a secure string.</span></span> <span data-ttu-id="3318c-196">tooaccomplish, budeme používat hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3318c-196">tooaccomplish this, we will use hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

<span data-ttu-id="3318c-197">Spuštění hello následující rutiny a v okně požadavku pověření hello prostředí Windows PowerShell, zadejte hello toouse jméno a heslo pro účet místního správce hello ve virtuálním počítači Windows hello.</span><span class="sxs-lookup"><span data-stu-id="3318c-197">Execute hello following cmdlet and, in hello Windows PowerShell credential request window, type hello name and password toouse for hello local administrator account in hello Windows virtual machine.</span></span>

    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."

### <a name="set-hello-operating-system-properties-for-hello-virtual-machine"></a><span data-ttu-id="3318c-198">Nastavit vlastnosti hello operačního systému pro virtuální počítač hello</span><span class="sxs-lookup"><span data-stu-id="3318c-198">Set hello operating system properties for hello virtual machine</span></span>
<span data-ttu-id="3318c-199">Nyní jsme vlastnosti operačního systému připravené tooset hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3318c-199">Now we are ready tooset hello virtual machine's operating system properties.</span></span> <span data-ttu-id="3318c-200">Budeme používat hello [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) vyžadují rutiny tooset hello typ operačního systému jako Windows hello [agenta virtuálního počítače](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toobe nainstalovaná, zadejte tuto rutinu hello umožňuje automaticky Aktualizovat a nastavit hello název virtuálního počítače, název počítače hello a hello pověření pomocí hello proměnné, které jste dříve inicializován.</span><span class="sxs-lookup"><span data-stu-id="3318c-200">We will use hello [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet tooset hello type of operating system as Windows, require hello [virtual machine agent](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toobe installed, specify that hello cmdlet enables auto update and set hello virtual machine name, hello computer name, and hello credential using hello variables that you previously initialized.</span></span>

<span data-ttu-id="3318c-201">Spusťte hello následující rutiny tooset hello vlastnosti operačního systému pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3318c-201">Execute hello following cmdlet tooset hello operating system properties for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-hello-network-interface-toohello-virtual-machine"></a><span data-ttu-id="3318c-202">Přidat hello síťové rozhraní toohello virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="3318c-202">Add hello network interface toohello virtual machine</span></span>
<span data-ttu-id="3318c-203">Potom přidáme hello síťové rozhraní, že jsme vytvořili dříve toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3318c-203">Next, we will add hello network interface that we created previously toohello virtual machine.</span></span> <span data-ttu-id="3318c-204">Budeme používat hello [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) rutiny tooadd hello síťových rozhraní pomocí hello síťové rozhraní proměnnou, kterou jste definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="3318c-204">We will use hello [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet tooadd hello network interface using hello network interface variable that you defined earlier.</span></span>

<span data-ttu-id="3318c-205">Spusťte hello následující rutiny tooset hello síťové rozhraní pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3318c-205">Execute hello following cmdlet tooset hello network interface for your virtual machine.</span></span>

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-hello-blob-storage-location-for-hello-disk-toobe-used-by-hello-virtual-machine"></a><span data-ttu-id="3318c-206">Nastavte umístění úložiště objektů blob hello toobe disku hello používá hello virtuálním počítačem</span><span class="sxs-lookup"><span data-stu-id="3318c-206">Set hello blob storage location for hello disk toobe used by hello virtual machine</span></span>
<span data-ttu-id="3318c-207">V dalším kroku se nastaví hello umístění úložiště objektů blob pro toobe disku hello používá hello virtuálního počítače pomocí hello proměnné, které jste definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="3318c-207">Next, we will set hello blob storage location for hello disk toobe used by hello virtual machine using hello variables that you defined earlier.</span></span>

<span data-ttu-id="3318c-208">Spusťte hello následující umístění úložiště objektů blob hello tooset rutiny.</span><span class="sxs-lookup"><span data-stu-id="3318c-208">Execute hello following cmdlet tooset hello blob storage location.</span></span>

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-hello-operating-system-disk-properties-for-hello-virtual-machine"></a><span data-ttu-id="3318c-209">Nastavit vlastnosti disku pro virtuální počítač hello hello operačního systému</span><span class="sxs-lookup"><span data-stu-id="3318c-209">Set hello operating system disk properties for hello virtual machine</span></span>
<span data-ttu-id="3318c-210">V dalším kroku jsme nastaví hello operačního systému disku vlastnosti hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3318c-210">Next, we will set hello operating system disk properties for hello virtual machine.</span></span> <span data-ttu-id="3318c-211">Použijeme hello [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) toospecify rutiny, která hello operačního systému pro virtuální počítač hello bude pocházet z bitové kopie, tooset ukládání do mezipaměti pouze tooread (protože je během instalace systému SQL Server na hello stejný disk) a definovat název virtuálního počítače Hello a disk operačního systému hello definovat pomocí hello proměnné, které jsme definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="3318c-211">We will use hello [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet toospecify that hello operating system for hello virtual machine will come from an image, tooset caching tooread only (because SQL Server is being installed on hello same disk) and define hello virtual machine name and hello operating system disk defined using hello variables that we defined earlier.</span></span>

<span data-ttu-id="3318c-212">Spusťte hello následující vlastnosti disku rutiny tooset hello operačního systému pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3318c-212">Execute hello following cmdlet tooset hello operating system disk properties for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-hello-platform-image-for-hello-virtual-machine"></a><span data-ttu-id="3318c-213">Zadejte hello image platformy pro virtuální počítač hello</span><span class="sxs-lookup"><span data-stu-id="3318c-213">Specify hello platform image for hello virtual machine</span></span>
<span data-ttu-id="3318c-214">Naše poslední krok konfigurace je image platformy hello toospecify pro naše virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3318c-214">Our last configuration step is toospecify hello platform image for our virtual machine.</span></span> <span data-ttu-id="3318c-215">V našem kurzu používáme hello nejnovější bitovou kopii systému SQL Server 2016 CTP.</span><span class="sxs-lookup"><span data-stu-id="3318c-215">For our tutorial, we are using hello latest SQL Server 2016 CTP image.</span></span> <span data-ttu-id="3318c-216">Budeme používat hello [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) rutiny toouse tuto bitovou kopii podle definice hello proměnné, které jste definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="3318c-216">We will use hello [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet toouse this image as defined by hello variables that you defined earlier.</span></span>

<span data-ttu-id="3318c-217">Spusťte hello následující rutiny toospecify hello image platformy pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3318c-217">Execute hello following cmdlet toospecify hello platform image for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-hello-sql-vm"></a><span data-ttu-id="3318c-218">Vytvořit virtuální počítač SQL hello</span><span class="sxs-lookup"><span data-stu-id="3318c-218">Create hello SQL VM</span></span>
<span data-ttu-id="3318c-219">Teď, když dokončíte kroky konfigurace hello, jste připravené toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3318c-219">Now that you have finished hello configuration steps, you are ready toocreate hello virtual machine.</span></span> <span data-ttu-id="3318c-220">Budeme používat hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) rutiny toocreate hello virtuálního počítače pomocí hello proměnné, které jsme definovali.</span><span class="sxs-lookup"><span data-stu-id="3318c-220">We will use hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet toocreate hello virtual machine using hello variables that we have defined.</span></span>

<span data-ttu-id="3318c-221">Spusťte následující rutiny toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3318c-221">Execute hello following cmdlet toocreate your virtual machine.</span></span>

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

<span data-ttu-id="3318c-222">Při vytvoření Hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3318c-222">hello virtual machine is created.</span></span> <span data-ttu-id="3318c-223">Všimněte si, že standardní účet úložiště je pro Diagnostika spouštění vytvořit, protože hello zadaný účet úložiště pro disk hello virtuálního počítače je prémiový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3318c-223">Notice that a standard storage account is created for boot diagnostics because hello specified storage account for hello virtual machine's disk is a premium storage account.</span></span>

<span data-ttu-id="3318c-224">Tento počítač nyní můžete zobrazit v portálu Azure toosee hello [jeho veřejnou IP adresu a jeho plně kvalifikovaný název domény](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="3318c-224">You can now view this machine in hello Azure Portal toosee [its public IP address and its fully qualified domain name](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

## <a name="example-script"></a><span data-ttu-id="3318c-225">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="3318c-225">Example script</span></span>
<span data-ttu-id="3318c-226">Hello následující skript obsahuje hello dokončení skriptu prostředí PowerShell pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3318c-226">hello following script contains hello complete PowerShell script for this tutorial.</span></span> <span data-ttu-id="3318c-227">Se předpokládá, že máte již instalace hello předplatného Azure toouse s hello **Add-AzureRmAccount** a **Select-AzureRmSubscription** příkazy.</span><span class="sxs-lookup"><span data-stu-id="3318c-227">It assumes that you have already setup hello Azure subscription toouse with hello **Add-AzureRmAccount** and **Select-AzureRmSubscription** commands.</span></span>

    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create hello VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a><span data-ttu-id="3318c-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3318c-228">Next steps</span></span>
<span data-ttu-id="3318c-229">Po vytvoření virtuálního počítače hello jste připravené tooconnect toohello virtuálního počítače pomocí připojení RDP a instalační program.</span><span class="sxs-lookup"><span data-stu-id="3318c-229">After hello virtual machine is created, you are ready tooconnect toohello virtual machine using RDP and setup connectivity.</span></span> <span data-ttu-id="3318c-230">Další informace najdete v tématu [připojit tooa virtuálního počítače systému SQL Server na platformě Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).</span><span class="sxs-lookup"><span data-stu-id="3318c-230">For more information, see [Connect tooa SQL Server Virtual Machine on Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).</span></span>

