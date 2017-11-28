---
title: "Vyberte bitové kopie virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Další informace o použití prostředí Azure PowerSHell k určení vydavatele, nabídky, SKU a verze pro Image Marketplace virtuálních počítačů."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 814ae260123c045d4b6766bf4b312f874cd77068
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a><span data-ttu-id="31d9e-103">Postup nalezení bitové kopie virtuálního počítače s Windows v Azure Marketplace s prostředím Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="31d9e-103">How to find Windows VM images in the Azure Marketplace with Azure PowerShell</span></span>

<span data-ttu-id="31d9e-104">Toto téma popisuje, jak pomocí prostředí Azure PowerShell k vyhledání Image virtuálních počítačů v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="31d9e-104">This topic describes how to use Azure PowerShell to find VM images in the Azure Marketplace.</span></span> <span data-ttu-id="31d9e-105">Tyto informace slouží k určení image pořízenou prostřednictvím Marketplace při vytvoření virtuálního počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="31d9e-105">Use this information to specify a Marketplace image when you create a Windows VM.</span></span>

<span data-ttu-id="31d9e-106">Ujistěte se, zda je nainstalován a nakonfigurován nejnovější [modul Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="31d9e-106">Make sure that you installed and configured the latest [Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>



## <a name="table-of-commonly-used-windows-images"></a><span data-ttu-id="31d9e-107">Tabulka běžně používané bitových kopií systému Windows</span><span class="sxs-lookup"><span data-stu-id="31d9e-107">Table of commonly used Windows images</span></span>
| <span data-ttu-id="31d9e-108">Název vydavatele</span><span class="sxs-lookup"><span data-stu-id="31d9e-108">PublisherName</span></span> | <span data-ttu-id="31d9e-109">Nabídka</span><span class="sxs-lookup"><span data-stu-id="31d9e-109">Offer</span></span> | <span data-ttu-id="31d9e-110">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="31d9e-110">Sku</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="31d9e-111">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-111">MicrosoftWindowsServer</span></span> |<span data-ttu-id="31d9e-112">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-112">WindowsServer</span></span> |<span data-ttu-id="31d9e-113">2016 Datacenter</span><span class="sxs-lookup"><span data-stu-id="31d9e-113">2016-Datacenter</span></span> |
| <span data-ttu-id="31d9e-114">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-114">MicrosoftWindowsServer</span></span> |<span data-ttu-id="31d9e-115">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-115">WindowsServer</span></span> |<span data-ttu-id="31d9e-116">2016 Datacenter Server Core</span><span class="sxs-lookup"><span data-stu-id="31d9e-116">2016-Datacenter-Server-Core</span></span> |
| <span data-ttu-id="31d9e-117">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-117">MicrosoftWindowsServer</span></span> |<span data-ttu-id="31d9e-118">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-118">WindowsServer</span></span> |<span data-ttu-id="31d9e-119">2016 datového centra s kontejnery</span><span class="sxs-lookup"><span data-stu-id="31d9e-119">2016-Datacenter-with-Containers</span></span> |
| <span data-ttu-id="31d9e-120">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-120">MicrosoftWindowsServer</span></span> |<span data-ttu-id="31d9e-121">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-121">WindowsServer</span></span> |<span data-ttu-id="31d9e-122">2016. Nano Server</span><span class="sxs-lookup"><span data-stu-id="31d9e-122">2016-Nano-Server</span></span> |
| <span data-ttu-id="31d9e-123">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-123">MicrosoftWindowsServer</span></span> |<span data-ttu-id="31d9e-124">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-124">WindowsServer</span></span> |<span data-ttu-id="31d9e-125">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="31d9e-125">2012-R2-Datacenter</span></span> |
| <span data-ttu-id="31d9e-126">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-126">MicrosoftWindowsServer</span></span> |<span data-ttu-id="31d9e-127">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-127">WindowsServer</span></span> |<span data-ttu-id="31d9e-128">AKTUALIZACE SP1 2008 R2</span><span class="sxs-lookup"><span data-stu-id="31d9e-128">2008-R2-SP1</span></span> |
| <span data-ttu-id="31d9e-129">MicrosoftDynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="31d9e-129">MicrosoftDynamicsNAV</span></span> |<span data-ttu-id="31d9e-130">DynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="31d9e-130">DynamicsNAV</span></span> |<span data-ttu-id="31d9e-131">2017</span><span class="sxs-lookup"><span data-stu-id="31d9e-131">2017</span></span> |
| <span data-ttu-id="31d9e-132">MicrosoftSharePoint</span><span class="sxs-lookup"><span data-stu-id="31d9e-132">MicrosoftSharePoint</span></span> |<span data-ttu-id="31d9e-133">MicrosoftSharePointServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-133">MicrosoftSharePointServer</span></span> |<span data-ttu-id="31d9e-134">2016</span><span class="sxs-lookup"><span data-stu-id="31d9e-134">2016</span></span> |
| <span data-ttu-id="31d9e-135">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-135">MicrosoftSQLServer</span></span> |<span data-ttu-id="31d9e-136">SQL2016 WS2016</span><span class="sxs-lookup"><span data-stu-id="31d9e-136">SQL2016-WS2016</span></span> |<span data-ttu-id="31d9e-137">Enterprise</span><span class="sxs-lookup"><span data-stu-id="31d9e-137">Enterprise</span></span> |
| <span data-ttu-id="31d9e-138">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="31d9e-138">MicrosoftSQLServer</span></span> |<span data-ttu-id="31d9e-139">SQL2014SP2 WS2012R2</span><span class="sxs-lookup"><span data-stu-id="31d9e-139">SQL2014SP2-WS2012R2</span></span> |<span data-ttu-id="31d9e-140">Enterprise</span><span class="sxs-lookup"><span data-stu-id="31d9e-140">Enterprise</span></span> |
| <span data-ttu-id="31d9e-141">MicrosoftWindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="31d9e-141">MicrosoftWindowsServerHPCPack</span></span> |<span data-ttu-id="31d9e-142">WindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="31d9e-142">WindowsServerHPCPack</span></span> |<span data-ttu-id="31d9e-143">2012R2</span><span class="sxs-lookup"><span data-stu-id="31d9e-143">2012R2</span></span> |
| <span data-ttu-id="31d9e-144">MicrosoftWindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="31d9e-144">MicrosoftWindowsServerEssentials</span></span> |<span data-ttu-id="31d9e-145">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="31d9e-145">WindowsServerEssentials</span></span> |<span data-ttu-id="31d9e-146">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="31d9e-146">WindowsServerEssentials</span></span> |

## <a name="find-specific-images"></a><span data-ttu-id="31d9e-147">Vyhledání konkrétních imagí</span><span class="sxs-lookup"><span data-stu-id="31d9e-147">Find specific images</span></span>


<span data-ttu-id="31d9e-148">Při vytváření nového virtuálního počítače pomocí Azure Resource Manageru je v některých případech nutné zadat image kombinací následujících vlastností image:</span><span class="sxs-lookup"><span data-stu-id="31d9e-148">When creating a new virtual machine with Azure Resource Manager, in some cases you need to specify an image with the combination of the following image properties:</span></span>

* <span data-ttu-id="31d9e-149">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="31d9e-149">Publisher</span></span>
* <span data-ttu-id="31d9e-150">Nabídka</span><span class="sxs-lookup"><span data-stu-id="31d9e-150">Offer</span></span>
* <span data-ttu-id="31d9e-151">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="31d9e-151">SKU</span></span>

<span data-ttu-id="31d9e-152">Například použít tyto hodnoty [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) rutiny prostředí PowerShell nebo pomocí šablony skupiny prostředků, ve kterém musíte zadat typ virtuálního počítače, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="31d9e-152">For example, use these values with the [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell cmdlet, or with a resource group template in which you must specify the type of VM to be created.</span></span>

<span data-ttu-id="31d9e-153">Pokud potřebujete zjistit tyto hodnoty, můžete spustit [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), a [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) rutiny přejděte bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="31d9e-153">If you need to determine these values, you can run the [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), and [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets to navigate the images.</span></span> <span data-ttu-id="31d9e-154">Můžete určit tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="31d9e-154">You determine these values:</span></span>

1. <span data-ttu-id="31d9e-155">Vypsat vydavatele imagí.</span><span class="sxs-lookup"><span data-stu-id="31d9e-155">List the image publishers.</span></span>
2. <span data-ttu-id="31d9e-156">Pro daného vydavatele vypsat jeho nabídky.</span><span class="sxs-lookup"><span data-stu-id="31d9e-156">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="31d9e-157">Pro danou nabídku vypsat její skladovou jednotku (SKU).</span><span class="sxs-lookup"><span data-stu-id="31d9e-157">For a given offer, list their SKUs.</span></span>

<span data-ttu-id="31d9e-158">Nejprve vypište vydavatele pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="31d9e-158">First, list the publishers with the following commands:</span></span>

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

<span data-ttu-id="31d9e-159">Vyplňte název zvoleného vydavatele a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="31d9e-159">Fill in your chosen publisher name and run the following commands:</span></span>

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="31d9e-160">Vyplňte název zvolené nabídky a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="31d9e-160">Fill in your chosen offer name and run the following commands:</span></span>

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="31d9e-161">Z výstupu `Get-AzureRMVMImageSku` příkaz, abyste měli všechny informace, musíte zadat bitovou kopii pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="31d9e-161">From the output of the `Get-AzureRMVMImageSku` command, you have all the information you need to specify the image for a new virtual machine.</span></span>

<span data-ttu-id="31d9e-162">Následuje úplný příklad:</span><span class="sxs-lookup"><span data-stu-id="31d9e-162">The following shows a full example:</span></span>

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

<span data-ttu-id="31d9e-163">Výstup:</span><span class="sxs-lookup"><span data-stu-id="31d9e-163">Output:</span></span>

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

<span data-ttu-id="31d9e-164">Pro vydavatele „MicrosoftWindowsServer“:</span><span class="sxs-lookup"><span data-stu-id="31d9e-164">For the "MicrosoftWindowsServer" publisher:</span></span>

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="31d9e-165">Výstup:</span><span class="sxs-lookup"><span data-stu-id="31d9e-165">Output:</span></span>

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

<span data-ttu-id="31d9e-166">Pro nabídku „WindowsServer“:</span><span class="sxs-lookup"><span data-stu-id="31d9e-166">For the "WindowsServer" offer:</span></span>

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="31d9e-167">Výstup:</span><span class="sxs-lookup"><span data-stu-id="31d9e-167">Output:</span></span>

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

<span data-ttu-id="31d9e-168">Z tohoto seznamu si zkopírujte název zvolené skladové jednotky (SKU) a máte veškeré informace pro rutinu PowerShellu `Set-AzureRMVMSourceImage` nebo pro šablonu skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="31d9e-168">From this list, copy the chosen SKU name, and you have all the information for the `Set-AzureRMVMSourceImage` PowerShell cmdlet or for a resource group template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="31d9e-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31d9e-169">Next steps</span></span>
<span data-ttu-id="31d9e-170">Nyní si můžete vybrat přesně tu image, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="31d9e-170">Now you can choose precisely the image you want to use.</span></span> <span data-ttu-id="31d9e-171">Pokud chcete rychle vytvořit virtuální počítač pomocí informace o obrázku, který jste právě najít, přečtěte si téma [vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="31d9e-171">To create a virtual machine quickly by using the image information, which you just found, see [Create a Windows virtual machine with PowerShell](quick-create-powershell.md).</span></span>
