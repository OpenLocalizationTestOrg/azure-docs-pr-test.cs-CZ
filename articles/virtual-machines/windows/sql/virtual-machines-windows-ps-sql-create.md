---
title: "Vytvoření virtuálního počítače s SQL serverem v prostředí Azure PowerShell (Resource Manager) | Microsoft Docs"
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
ms.openlocfilehash: aa90a1d017af5f477407ab33f0580904472f412b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a><span data-ttu-id="346bf-103">Zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="346bf-103">Provision a SQL Server virtual machine using Azure PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="346bf-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="346bf-104">Portal</span></span>](virtual-machines-windows-portal-sql-server-provision.md)
> * [<span data-ttu-id="346bf-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="346bf-105">PowerShell</span></span>](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a><span data-ttu-id="346bf-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="346bf-106">Overview</span></span>
<span data-ttu-id="346bf-107">V tomto kurzu se dozvíte, jak vytvořit jeden virtuální počítač Azure pomocí **Azure Resource Manager** modelu nasazení pomocí rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="346bf-107">This tutorial shows you how to create a single Azure virtual machine using the **Azure Resource Manager** deployment model using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="346bf-108">V tomto kurzu vytvoříme jednoho virtuálního počítače pomocí jednoho disku z bitové kopie v galerii SQL.</span><span class="sxs-lookup"><span data-stu-id="346bf-108">In this tutorial, we will create a single virtual machine using a single disk drive from an image in the SQL Gallery.</span></span> <span data-ttu-id="346bf-109">Vytvoříme nové zprostředkovatele pro úložiště, síť a výpočetní prostředky, které se použijí pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="346bf-109">We will create new providers for the storage, network, and compute resources that will be used by the virtual machine.</span></span> <span data-ttu-id="346bf-110">Pokud máte existující zprostředkovatele pro kterýkoli z těchto prostředků, můžete místo toho použít těchto zprostředkovatelů.</span><span class="sxs-lookup"><span data-stu-id="346bf-110">If you have existing providers for any of these resources, you can use those providers instead.</span></span>

<span data-ttu-id="346bf-111">Pokud potřebujete classic verzi tohoto tématu, přečtěte si [zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell Classic](../classic/ps-sql-create.md).</span><span class="sxs-lookup"><span data-stu-id="346bf-111">If you need the classic version of this topic, see [Provision a SQL Server virtual machine using Azure PowerShell Classic](../classic/ps-sql-create.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="346bf-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="346bf-112">Prerequisites</span></span>
<span data-ttu-id="346bf-113">Pro účely tohoto kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="346bf-113">For this tutorial you'll need:</span></span>

* <span data-ttu-id="346bf-114">Účet Azure a předplatné před zahájením.</span><span class="sxs-lookup"><span data-stu-id="346bf-114">An Azure account and subscription before you start.</span></span> <span data-ttu-id="346bf-115">Pokud nemáte, si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="346bf-115">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="346bf-116">[Azure PowerShell)](/powershell/azure/overview), minimální verze 1.4.0 nebo novější (Tento kurz vytvořené pomocí verze 1.5.0).</span><span class="sxs-lookup"><span data-stu-id="346bf-116">[Azure PowerShell)](/powershell/azure/overview), minimum version of 1.4.0 or later (this tutorial written using version 1.5.0).</span></span>
  * <span data-ttu-id="346bf-117">Pokud chcete načíst vaše verze, zadejte **Azure Get-Module - ListAvailable**.</span><span class="sxs-lookup"><span data-stu-id="346bf-117">To retrieve your version, type **Get-Module Azure -ListAvailable**.</span></span>

## <a name="configure-your-subscription"></a><span data-ttu-id="346bf-118">Konfigurovat předplatné</span><span class="sxs-lookup"><span data-stu-id="346bf-118">Configure your subscription</span></span>
<span data-ttu-id="346bf-119">Otevřete prostředí Windows PowerShell a spuštěním následující rutiny vytvořit přístup k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="346bf-119">Open Windows PowerShell and establish access to your Azure account by running the following cmdlet.</span></span> <span data-ttu-id="346bf-120">Zobrazí se přihlašovací obrazovka k zadání pověření.</span><span class="sxs-lookup"><span data-stu-id="346bf-120">You will be presented with a sign in screen to enter your credentials.</span></span> <span data-ttu-id="346bf-121">Použijte stejnou e-mailu a heslo, které používáte pro přihlášení k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="346bf-121">Use the same email and password that you use to sign in to the Azure portal.</span></span>

    Add-AzureRmAccount

<span data-ttu-id="346bf-122">Po úspěšném přihlášení se zobrazí některé informace na obrazovce, která zahrnuje ID předplatného, se kterým jste přihlášení.</span><span class="sxs-lookup"><span data-stu-id="346bf-122">After successfully signing in you will see some information on screen that includes the SubscriptionId with which you signed in.</span></span> <span data-ttu-id="346bf-123">Toto je předplatné, ve kterém se vytvoří prostředky pro tento kurz, pokud nezměníte do jiného předplatného.</span><span class="sxs-lookup"><span data-stu-id="346bf-123">This is the subscription in which the resources for this tutorial will be created unless you change to a different subscription.</span></span> <span data-ttu-id="346bf-124">Pokud máte více SubscriptionIds, spusťte následující rutinu k zobrazení seznamu všech vaší SubscriptionIds:</span><span class="sxs-lookup"><span data-stu-id="346bf-124">If you have multiple SubscriptionIds, run the following cmdlet to return a list of all of your SubscriptionIds:</span></span>

    Get-AzureRmSubscription

<span data-ttu-id="346bf-125">Chcete-li změnit na jiné ID předplatného, spusťte následující rutinu s vaší požadované ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="346bf-125">To change to another SubscriptionID, run the following cmdlet with your desired SubscriptionId.</span></span>

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a><span data-ttu-id="346bf-126">Definování proměnné bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="346bf-126">Define image variables</span></span>
<span data-ttu-id="346bf-127">Pro zjednodušení použitelnost a pochopení dokončené skriptu z tohoto kurzu, spustíme definováním počet proměnných.</span><span class="sxs-lookup"><span data-stu-id="346bf-127">To simplify usability and understanding of the completed script from this tutorial, we will start by defining a number of variables.</span></span> <span data-ttu-id="346bf-128">Podle svých potřeb, ale pozor omezení týkající se názvu délky a speciální znaky, při úpravě hodnoty poskytnuté pojmenování změně hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="346bf-128">Change the parameter values as you see fit, but beware of naming restrictions related to name lengths and special characters when modifying the values provided.</span></span>

### <a name="location-and-resource-group"></a><span data-ttu-id="346bf-129">Umístění a skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="346bf-129">Location and Resource Group</span></span>
<span data-ttu-id="346bf-130">Dvě proměnné, které slouží k určení oblasti dat a skupině prostředků, do kterého chcete vytvořit další prostředky pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="346bf-130">Use two variables to define the data region and the resource group into which you will create the other resources for the virtual machine.</span></span>

<span data-ttu-id="346bf-131">Upravte požadovaným způsobem a potom spusťte následující rutiny k chybě při inicializaci tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="346bf-131">Modify as desired and then execute the following cmdlets to initialize these variables.</span></span>

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a><span data-ttu-id="346bf-132">Úložiště vlastností</span><span class="sxs-lookup"><span data-stu-id="346bf-132">Storage properties</span></span>
<span data-ttu-id="346bf-133">Použijte následující proměnné zadat účet úložiště a typ úložiště, který se má použít pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="346bf-133">Use the following variables to define the storage account and the type of storage to be used by the virtual machine.</span></span>

<span data-ttu-id="346bf-134">Upravte požadovaným způsobem a potom spusťte následující rutinu k chybě při inicializaci tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="346bf-134">Modify as desired and then execute the following cmdlet to initialize these variables.</span></span> <span data-ttu-id="346bf-135">Všimněte si, že v tomto příkladu používáme [Storage úrovně Premium](../../../storage/common/storage-premium-storage.md), který se doporučuje pro úlohy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="346bf-135">Note that in this example, we are using [Premium Storage](../../../storage/common/storage-premium-storage.md), which is recommended for production workloads.</span></span> <span data-ttu-id="346bf-136">Podrobnosti na tyto pokyny a dalších doporučeních najdete v tématu [osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-performance.md).</span><span class="sxs-lookup"><span data-stu-id="346bf-136">For details on this guidance and other recommendations, see [Performance best practices for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).</span></span>

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a><span data-ttu-id="346bf-137">Vlastnosti sítě</span><span class="sxs-lookup"><span data-stu-id="346bf-137">Network properties</span></span>
<span data-ttu-id="346bf-138">Použijte následující proměnné k definování síťového rozhraní, metoda přidělení TCP/IP, název virtuální sítě, název virtuální podsítě, rozsah IP adres pro virtuální síť, rozsah IP adres pro podsíť a popisek názvu veřejné domény pro použití sítě ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="346bf-138">Use the following variables to define the network interface, the TCP/IP allocation method, the virtual network name, the virtual subnet name, the range of IP addresses for the virtual network, the range of IP addresses for the subnet, and the public domain name label to be used by the network in the virtual machine.</span></span>

<span data-ttu-id="346bf-139">Upravte požadovaným způsobem a potom spusťte následující rutinu k chybě při inicializaci tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="346bf-139">Modify as desired and then execute the following cmdlet to initialize these variables.</span></span>

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"

### <a name="virtual-machine-properties"></a><span data-ttu-id="346bf-140">Vlastnosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="346bf-140">Virtual machine properties</span></span>
<span data-ttu-id="346bf-141">Zadat název virtuálního počítače, název počítače, velikost virtuálního počítače a název disku operačního systému pro virtuální počítač, použijte následující proměnné.</span><span class="sxs-lookup"><span data-stu-id="346bf-141">Use the following variables to define the virtual machine name, the computer name, the virtual machine size, and the operating system disk name for the virtual machine.</span></span>

<span data-ttu-id="346bf-142">Upravte požadovaným způsobem a potom spusťte následující rutinu k chybě při inicializaci tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="346bf-142">Modify as desired and then execute the following cmdlet to initialize these variables.</span></span>

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a><span data-ttu-id="346bf-143">Vlastnosti bitové kopie</span><span class="sxs-lookup"><span data-stu-id="346bf-143">Image properties</span></span>
<span data-ttu-id="346bf-144">Chcete-li definovat obrázek, který má používat pro virtuální počítač, použijte následující proměnné.</span><span class="sxs-lookup"><span data-stu-id="346bf-144">Use the following variables to define the image to use for the virtual machine.</span></span> <span data-ttu-id="346bf-145">V tomto příkladu se používá bitovou kopii systému SQL Server 2016 Enterprise.</span><span class="sxs-lookup"><span data-stu-id="346bf-145">In this example, the SQL Server 2016 Enterprise image is used.</span></span>

<span data-ttu-id="346bf-146">Upravte požadovaným způsobem a potom spusťte následující rutinu k chybě při inicializaci tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="346bf-146">Modify as desired and then execute the following cmdlet to initialize these variables.</span></span>

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

<span data-ttu-id="346bf-147">Všimněte si, že můžete získat úplný seznam nabídky bitovou kopii systému SQL Server pomocí příkazu Get-AzureRmVMImageOffer:</span><span class="sxs-lookup"><span data-stu-id="346bf-147">Note that you can get a full list of SQL Server image offerings with the Get-AzureRmVMImageOffer command:</span></span>

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

<span data-ttu-id="346bf-148">A zobrazí se dostupné pro nabídky pomocí příkazu Get-AzureRmVMImageSku SKU.</span><span class="sxs-lookup"><span data-stu-id="346bf-148">And you can see the Skus available for an offering with the Get-AzureRmVMImageSku command.</span></span> <span data-ttu-id="346bf-149">Následující příkaz ukazuje všech skladových položek k dispozici pro **SQL2014SP1 WS2012R2** nabízejí.</span><span class="sxs-lookup"><span data-stu-id="346bf-149">The following command shows all Skus available for the **SQL2014SP1-WS2012R2** offer.</span></span>

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a><span data-ttu-id="346bf-150">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="346bf-150">Create a resource group</span></span>
<span data-ttu-id="346bf-151">Pomocí modelu nasazení Resource Manager je první objekt, který vytvoříte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="346bf-151">With the Resource Manager deployment model, the first object that you create is the resource group.</span></span> <span data-ttu-id="346bf-152">Budeme používat [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) vytvořte skupinu prostředků Azure a jeho prostředků se název skupiny prostředků a umístění definované proměnné, které jste dříve inicializován.</span><span class="sxs-lookup"><span data-stu-id="346bf-152">We will use the [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet to create an Azure resource group and its resources with the resource group name and location defined by the variables that you previously initialized.</span></span>

<span data-ttu-id="346bf-153">Spusťte následující rutinu k vytvoření nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="346bf-153">Execute the following cmdlet to create your new resource group.</span></span>

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a><span data-ttu-id="346bf-154">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="346bf-154">Create a storage account</span></span>
<span data-ttu-id="346bf-155">Virtuální počítač vyžaduje prostředků úložiště pro disk operačního systému a souborů protokolu a data systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="346bf-155">The virtual machine requires storage resources for the operating system disk and for the SQL Server data and log files.</span></span> <span data-ttu-id="346bf-156">Pro jednoduchost vytvoříme jediný disk pro obojí.</span><span class="sxs-lookup"><span data-stu-id="346bf-156">For simplicity, we will create a single disk for both.</span></span> <span data-ttu-id="346bf-157">Můžete připojit další disky později pomocí [disku Azure přidat](/powershell/module/azure/add-azuredisk) rutiny, aby bylo možné umístit data systému SQL Server a protokolu se soubory ve vyhrazených disků.</span><span class="sxs-lookup"><span data-stu-id="346bf-157">You can attach additional disks later using the [Add-Azure Disk](/powershell/module/azure/add-azuredisk) cmdlet in order to place your SQL Server data and log files on dedicated disks.</span></span> <span data-ttu-id="346bf-158">Budeme používat [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) vytvořte standardní účet úložiště v nové skupiny prostředků a název účtu úložiště, název Sku úložiště a umístění definovat pomocí proměnné, které jste dříve inicializován.</span><span class="sxs-lookup"><span data-stu-id="346bf-158">We will use the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet to create a standard storage account in your new resource group and with the storage account name, storage Sku name, and location defined using the variables that you previously initialized.</span></span>

<span data-ttu-id="346bf-159">Spusťte následující rutinu k vytvoření nového účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="346bf-159">Execute the following cmdlet to create your new storage account.</span></span>

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a><span data-ttu-id="346bf-160">Vytvoření síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="346bf-160">Create network resources</span></span>
<span data-ttu-id="346bf-161">Virtuální počítač vyžaduje počet síťovým prostředkům pro připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="346bf-161">The virtual machine requires a number of network resources for network connectivity.</span></span>

* <span data-ttu-id="346bf-162">Každý virtuální počítač vyžaduje virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="346bf-162">Each virtual machine requires a virtual network.</span></span>
* <span data-ttu-id="346bf-163">Virtuální síť musí mít alespoň jednu podsíť definované.</span><span class="sxs-lookup"><span data-stu-id="346bf-163">A virtual network must have at least one subnet defined.</span></span>
* <span data-ttu-id="346bf-164">Síťové rozhraní musí být definovány se veřejné nebo privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="346bf-164">A network interface must be defined with either a public or a private IP address.</span></span>

### <a name="create-a-virtual-network-subnet-configuration"></a><span data-ttu-id="346bf-165">Vytvoření konfigurace podsítě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="346bf-165">Create a virtual network subnet configuration</span></span>
<span data-ttu-id="346bf-166">Spustíme vytvořením konfigurace pro naše virtuální síť s podsítí.</span><span class="sxs-lookup"><span data-stu-id="346bf-166">We will start by creating a subnet configuration for our virtual network.</span></span> <span data-ttu-id="346bf-167">V našem kurzu vytvoříme výchozí podsíť pomocí [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) rutiny.</span><span class="sxs-lookup"><span data-stu-id="346bf-167">For our tutorial, we will create a default subnet using the [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet.</span></span> <span data-ttu-id="346bf-168">Pomocí názvu a adresy předponu podsítě definovat pomocí proměnné, které jste dříve inicializovat vytvoříme naše konfigurace podsítě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="346bf-168">We will create our virtual network subnet configuration with the subnet name and address prefix defined using the variables that you previously initialized.</span></span>

> [!NOTE]
> <span data-ttu-id="346bf-169">Můžete definovat další vlastnosti konfigurace podsítě virtuální sítě pomocí této rutiny, ale který je nad rámec tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="346bf-169">You can define additional properties of the virtual network subnet configuration using this cmdlet, but that is beyond the scope of this tutorial.</span></span>
>
>

<span data-ttu-id="346bf-170">Spusťte následující rutiny můžete vytvořit konfiguraci virtuální podsítě.</span><span class="sxs-lookup"><span data-stu-id="346bf-170">Execute the following cmdlet to create your virtual subnet configuration.</span></span>

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a><span data-ttu-id="346bf-171">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="346bf-171">Create a virtual network</span></span>
<span data-ttu-id="346bf-172">V dalším kroku vytvoříme naše pomocí virtuální sítě [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) rutiny.</span><span class="sxs-lookup"><span data-stu-id="346bf-172">Next, we will create our virtual network using the [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet.</span></span> <span data-ttu-id="346bf-173">Vytvoříme naše virtuální sítě v nové skupiny prostředků, název, umístění a adresu předpona definovaná pomocí proměnné, které jste dříve inicializovat a pomocí konfigurace podsítě, který jste zadali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="346bf-173">We will create our virtual network in your new resource group, with the name, location, and address prefix defined using the variables that you previously initialized, and using the subnet configuration that you defined in the previous step.</span></span>

<span data-ttu-id="346bf-174">Spusťte následující rutinu pro vytvoření svojí virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="346bf-174">Execute the following cmdlet to create your virtual network.</span></span>

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a><span data-ttu-id="346bf-175">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="346bf-175">Create the public IP address</span></span>
<span data-ttu-id="346bf-176">Teď, když máme naše virtuální síť definován, je potřeba nakonfigurovat IP adresu pro připojení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="346bf-176">Now that we have our virtual network defined, we need to configure an IP address for connectivity to the virtual machine.</span></span> <span data-ttu-id="346bf-177">V tomto kurzu vytvoříme veřejnou IP adresu pomocí dynamických IP adres pro podporu připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="346bf-177">For this tutorial, we will create a public IP address using dynamic IP addressing to support Internet connectivity.</span></span> <span data-ttu-id="346bf-178">Budeme používat [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) vytvořte veřejnou IP adresu v prevously vytvořenou skupinu prostředků a název, umístění, metoda přidělení a popisek názvu domény DNS definovat pomocí proměnné, které jste dříve inicializován.</span><span class="sxs-lookup"><span data-stu-id="346bf-178">We will use the [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet to create the public IP address in the resource group created prevously and with the name, location, allocation method, and DNS domain name label defined using the variables that you previously initialized.</span></span>

> [!NOTE]
> <span data-ttu-id="346bf-179">Můžete definovat další vlastnosti veřejné IP adresy pomocí této rutiny, ale který je nad rámec tohoto počáteční kurzu.</span><span class="sxs-lookup"><span data-stu-id="346bf-179">You can define additional properties of the public IP address using this cmdlet, but that is beyond the scope of this initial tutorial.</span></span> <span data-ttu-id="346bf-180">Můžete také vytvořit privátní adresu nebo adresu se statickou adresou, ale který je také nad rámec tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="346bf-180">You could also create a private address or an address with a static address, but that is also beyond the scope of this tutorial.</span></span>
>
>

<span data-ttu-id="346bf-181">Spusťte následující rutiny můžete vytvořit veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="346bf-181">Execute the following cmdlet to create your public IP address.</span></span>

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a><span data-ttu-id="346bf-182">Vytvořit rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="346bf-182">Create the network interface</span></span>
<span data-ttu-id="346bf-183">Nyní jsme připraveni k vytvoření síťového rozhraní, které budou používat naše virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="346bf-183">We are now ready to create the network interface that our virtual machine will use.</span></span> <span data-ttu-id="346bf-184">Budeme používat [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) předtím definovaný vytvořte naše síťové rozhraní, ve skupině prostředků vytvořili dříve a název, umístění, podsíť a veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="346bf-184">We will use the [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet to create our network interface in the resource group created earlier and with the name, location, subnet and public IP address previously defined.</span></span>

<span data-ttu-id="346bf-185">Spusťte následující rutinu k vytvoření síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="346bf-185">Execute the following cmdlet to create your network interface.</span></span>

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a><span data-ttu-id="346bf-186">Konfigurace objektu virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="346bf-186">Configure a VM object</span></span>
<span data-ttu-id="346bf-187">Teď, když máme úložiště a síťové prostředky, které jsou definované, jsme připraveni k definování výpočetní prostředky pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="346bf-187">Now that we have storage and network resources defined, we are ready to define compute resources for the virtual machine.</span></span> <span data-ttu-id="346bf-188">V našem kurzu jsme zadejte velikost virtuálního počítače a různé vlastnosti operačního systému, zadejte definovat úložiště objektů blob síťového rozhraní, které jsme dříve vytvořili, a pak zadejte disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="346bf-188">For our tutorial, we will specify the virtual machine size and various operating system properties, specify the network interface that we previously created, define blob storage, and then specify the operating system disk.</span></span>

### <a name="create-the-vm-object"></a><span data-ttu-id="346bf-189">Vytvořit objekt virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="346bf-189">Create the VM object</span></span>
<span data-ttu-id="346bf-190">Spustíme zadáním velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="346bf-190">We will start by specifying the virtual machine size.</span></span> <span data-ttu-id="346bf-191">V tomto kurzu zadáváme DS13.</span><span class="sxs-lookup"><span data-stu-id="346bf-191">For this tutorial, we are specifying a DS13.</span></span> <span data-ttu-id="346bf-192">Budeme používat [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) vytvořte objekt konfigurovat virtuální počítač s názvem a velikostí, které jsou definované pomocí proměnné, které jste dříve inicializován.</span><span class="sxs-lookup"><span data-stu-id="346bf-192">We will use the [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet to create a configurable virtual machine object with the name and size defined using the variables that you previously initialized.</span></span>

<span data-ttu-id="346bf-193">Spusťte následující rutinu k vytvoření objektu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="346bf-193">Execute the following cmdlet to create the virtual machine object.</span></span>

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a><span data-ttu-id="346bf-194">Vytvořit objekt přihlašovacích údajů k uchování jméno a heslo pro přihlašovací údaje místního správce</span><span class="sxs-lookup"><span data-stu-id="346bf-194">Create a credential object to hold the name and password for the local administrator credentials</span></span>
<span data-ttu-id="346bf-195">Než budeme moct nastavit vlastnosti operačního systému pro virtuální počítač, je potřeba zadat přihlašovací údaje pro účet místního správce jako zabezpečený řetězec.</span><span class="sxs-lookup"><span data-stu-id="346bf-195">Before we can set the operating system properties for the virtual machine, we need to supply the credentials for the local administrator account as a secure string.</span></span> <span data-ttu-id="346bf-196">K tomu, budeme používat [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="346bf-196">To accomplish this, we will use the [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

<span data-ttu-id="346bf-197">Spusťte následující rutinu a v okně požadavku pověření prostředí Windows PowerShell, zadejte jméno a heslo pro účet místního správce ve virtuálním počítači Windows.</span><span class="sxs-lookup"><span data-stu-id="346bf-197">Execute the following cmdlet and, in the Windows PowerShell credential request window, type the name and password to use for the local administrator account in the Windows virtual machine.</span></span>

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a><span data-ttu-id="346bf-198">Nastavit vlastnosti operačního systému pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="346bf-198">Set the operating system properties for the virtual machine</span></span>
<span data-ttu-id="346bf-199">Nyní jsme připravení nastavit vlastnosti operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="346bf-199">Now we are ready to set the virtual machine's operating system properties.</span></span> <span data-ttu-id="346bf-200">Budeme používat [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) vyžadují rutiny nastavte typ operačního systému jako Windows, [agenta virtuálního počítače](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) k instalaci, zadejte, že rutina umožňuje automatickou aktualizaci a nastavte název virtuálního počítače, název počítače a přihlašovacích údajů pomocí proměnné, které jste dříve inicializován.</span><span class="sxs-lookup"><span data-stu-id="346bf-200">We will use the [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet to set the type of operating system as Windows, require the [virtual machine agent](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) to be installed, specify that the cmdlet enables auto update and set the virtual machine name, the computer name, and the credential using the variables that you previously initialized.</span></span>

<span data-ttu-id="346bf-201">Spusťte následující rutinu a nastavte vlastnosti operačního systému pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="346bf-201">Execute the following cmdlet to set the operating system properties for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a><span data-ttu-id="346bf-202">Přidání rozhraní sítě k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="346bf-202">Add the network interface to the virtual machine</span></span>
<span data-ttu-id="346bf-203">Potom přidáme síťového rozhraní, které jsme vytvořili dříve k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="346bf-203">Next, we will add the network interface that we created previously to the virtual machine.</span></span> <span data-ttu-id="346bf-204">Budeme používat [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) přidejte síťové rozhraní, pomocí proměnné rozhraní sítě, která jste definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="346bf-204">We will use the [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet to add the network interface using the network interface variable that you defined earlier.</span></span>

<span data-ttu-id="346bf-205">Spusťte následující rutiny můžete nastavit rozhraní sítě pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="346bf-205">Execute the following cmdlet to set the network interface for your virtual machine.</span></span>

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a><span data-ttu-id="346bf-206">Nastavení umístění úložiště objektů blob pro disk, který se má použít pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="346bf-206">Set the blob storage location for the disk to be used by the virtual machine</span></span>
<span data-ttu-id="346bf-207">V dalším kroku se nastaví umístění úložiště objektů blob pro disk, který se má použít pro virtuální počítač pomocí proměnné, které jste definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="346bf-207">Next, we will set the blob storage location for the disk to be used by the virtual machine using the variables that you defined earlier.</span></span>

<span data-ttu-id="346bf-208">Spusťte následující rutiny můžete nastavit umístění úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="346bf-208">Execute the following cmdlet to set the blob storage location.</span></span>

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a><span data-ttu-id="346bf-209">Nastavit vlastnosti disku pro virtuální počítač operační systém</span><span class="sxs-lookup"><span data-stu-id="346bf-209">Set the operating system disk properties for the virtual machine</span></span>
<span data-ttu-id="346bf-210">Operační systém v dalším kroku bude nastaví vlastnosti disku pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="346bf-210">Next, we will set the operating system disk properties for the virtual machine.</span></span> <span data-ttu-id="346bf-211">Budeme používat [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) rutiny k určení, že operační systém pro virtuální počítač bude pocházet z bitové kopie, nastavení ukládání do mezipaměti pro čtení, (protože SQL Server je instalován na stejném disku) a zadejte název virtuálního počítače a disku operačního systému definovaná pomocí proměnné, které definovaného dříve.</span><span class="sxs-lookup"><span data-stu-id="346bf-211">We will use the [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet to specify that the operating system for the virtual machine will come from an image, to set caching to read only (because SQL Server is being installed on the same disk) and define the virtual machine name and the operating system disk defined using the variables that we defined earlier.</span></span>

<span data-ttu-id="346bf-212">Spusťte následující rutiny můžete nastavit vlastnosti disku pro virtuální počítač operační systém.</span><span class="sxs-lookup"><span data-stu-id="346bf-212">Execute the following cmdlet to set the operating system disk properties for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a><span data-ttu-id="346bf-213">Zadat image platformy pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="346bf-213">Specify the platform image for the virtual machine</span></span>
<span data-ttu-id="346bf-214">Naše poslední krok konfigurace slouží k zadání image platformy pro naše virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="346bf-214">Our last configuration step is to specify the platform image for our virtual machine.</span></span> <span data-ttu-id="346bf-215">V našem kurzu se používá nejnovější bitovou kopii systému SQL Server 2016 CTP.</span><span class="sxs-lookup"><span data-stu-id="346bf-215">For our tutorial, we are using the latest SQL Server 2016 CTP image.</span></span> <span data-ttu-id="346bf-216">Budeme používat [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) rutiny k použití této image podle definice proměnné, které jste definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="346bf-216">We will use the [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet to use this image as defined by the variables that you defined earlier.</span></span>

<span data-ttu-id="346bf-217">Spusťte následující rutiny můžete zadat image platformy pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="346bf-217">Execute the following cmdlet to specify the platform image for your virtual machine.</span></span>

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a><span data-ttu-id="346bf-218">Vytvoření virtuálního počítače SQL</span><span class="sxs-lookup"><span data-stu-id="346bf-218">Create the SQL VM</span></span>
<span data-ttu-id="346bf-219">Teď, když dokončíte kroky konfigurace, jste připraveni k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="346bf-219">Now that you have finished the configuration steps, you are ready to create the virtual machine.</span></span> <span data-ttu-id="346bf-220">Budeme používat [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) rutiny k vytvoření virtuálního počítače pomocí proměnné, které jsme definovali.</span><span class="sxs-lookup"><span data-stu-id="346bf-220">We will use the [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet to create the virtual machine using the variables that we have defined.</span></span>

<span data-ttu-id="346bf-221">Spusťte následující rutinu k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="346bf-221">Execute the following cmdlet to create your virtual machine.</span></span>

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

<span data-ttu-id="346bf-222">Při vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="346bf-222">The virtual machine is created.</span></span> <span data-ttu-id="346bf-223">Všimněte si, že standardní účet úložiště je pro Diagnostika spouštění vytvořit, protože zadaný účet úložiště pro disk virtuálního počítače je prémiový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="346bf-223">Notice that a standard storage account is created for boot diagnostics because the specified storage account for the virtual machine's disk is a premium storage account.</span></span>

<span data-ttu-id="346bf-224">Tento počítač nyní můžete zobrazit na portálu Azure najdete v části [jeho veřejnou IP adresu a jeho plně kvalifikovaný název domény](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="346bf-224">You can now view this machine in the Azure Portal to see [its public IP address and its fully qualified domain name](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

## <a name="example-script"></a><span data-ttu-id="346bf-225">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="346bf-225">Example script</span></span>
<span data-ttu-id="346bf-226">Následující skript obsahuje dokončení skriptu prostředí PowerShell pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="346bf-226">The following script contains the complete PowerShell script for this tutorial.</span></span> <span data-ttu-id="346bf-227">Se předpokládá, že máte již instalace předplatné Azure pro použití s **Add-AzureRmAccount** a **Select-AzureRmSubscription** příkazy.</span><span class="sxs-lookup"><span data-stu-id="346bf-227">It assumes that you have already setup the Azure subscription to use with the **Add-AzureRmAccount** and **Select-AzureRmSubscription** commands.</span></span>

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
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a><span data-ttu-id="346bf-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="346bf-228">Next steps</span></span>
<span data-ttu-id="346bf-229">Po vytvoření virtuálního počítače, jste připraveni k připojení k virtuálnímu počítači pomocí RDP a nastavení připojení.</span><span class="sxs-lookup"><span data-stu-id="346bf-229">After the virtual machine is created, you are ready to connect to the virtual machine using RDP and setup connectivity.</span></span> <span data-ttu-id="346bf-230">Další informace najdete v tématu [připojení SQL serveru virtuálnímu počítači na platformě Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).</span><span class="sxs-lookup"><span data-stu-id="346bf-230">For more information, see [Connect to a SQL Server Virtual Machine on Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).</span></span>

