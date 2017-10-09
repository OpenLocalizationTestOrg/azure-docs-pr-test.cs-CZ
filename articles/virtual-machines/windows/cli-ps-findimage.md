---
title: "bitové kopie aaaSelect virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí Azure PowerSHell toodetermine hello vydavatele, nabídky, SKU a verze pro Image Marketplace virtuálních počítačů."
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
ms.openlocfilehash: 752edcd0935f5141832e49503ae800ea0145e219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-windows-vm-images-in-hello-azure-marketplace-with-azure-powershell"></a><span data-ttu-id="94c9f-103">Jak Image toofind virtuální počítač s Windows v nástroji hello Azure Marketplace s prostředím Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="94c9f-103">How toofind Windows VM images in hello Azure Marketplace with Azure PowerShell</span></span>

<span data-ttu-id="94c9f-104">Toto téma popisuje, jak Image toouse prostředí Azure PowerShell toofind virtuálních počítačů v nástroji hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="94c9f-104">This topic describes how toouse Azure PowerShell toofind VM images in hello Azure Marketplace.</span></span> <span data-ttu-id="94c9f-105">Při vytvoření virtuálního počítače s Windows, použijte tento informace toospecify image pořízenou prostřednictvím Marketplace.</span><span class="sxs-lookup"><span data-stu-id="94c9f-105">Use this information toospecify a Marketplace image when you create a Windows VM.</span></span>

<span data-ttu-id="94c9f-106">Ujistěte se, zda je nainstalován a nakonfigurován hello nejnovější [modul Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="94c9f-106">Make sure that you installed and configured hello latest [Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>



## <a name="table-of-commonly-used-windows-images"></a><span data-ttu-id="94c9f-107">Tabulka běžně používané bitových kopií systému Windows</span><span class="sxs-lookup"><span data-stu-id="94c9f-107">Table of commonly used Windows images</span></span>
| <span data-ttu-id="94c9f-108">Název vydavatele</span><span class="sxs-lookup"><span data-stu-id="94c9f-108">PublisherName</span></span> | <span data-ttu-id="94c9f-109">Nabídka</span><span class="sxs-lookup"><span data-stu-id="94c9f-109">Offer</span></span> | <span data-ttu-id="94c9f-110">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="94c9f-110">Sku</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="94c9f-111">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-111">MicrosoftWindowsServer</span></span> |<span data-ttu-id="94c9f-112">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-112">WindowsServer</span></span> |<span data-ttu-id="94c9f-113">2016 Datacenter</span><span class="sxs-lookup"><span data-stu-id="94c9f-113">2016-Datacenter</span></span> |
| <span data-ttu-id="94c9f-114">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-114">MicrosoftWindowsServer</span></span> |<span data-ttu-id="94c9f-115">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-115">WindowsServer</span></span> |<span data-ttu-id="94c9f-116">2016 Datacenter Server Core</span><span class="sxs-lookup"><span data-stu-id="94c9f-116">2016-Datacenter-Server-Core</span></span> |
| <span data-ttu-id="94c9f-117">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-117">MicrosoftWindowsServer</span></span> |<span data-ttu-id="94c9f-118">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-118">WindowsServer</span></span> |<span data-ttu-id="94c9f-119">2016 datového centra s kontejnery</span><span class="sxs-lookup"><span data-stu-id="94c9f-119">2016-Datacenter-with-Containers</span></span> |
| <span data-ttu-id="94c9f-120">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-120">MicrosoftWindowsServer</span></span> |<span data-ttu-id="94c9f-121">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-121">WindowsServer</span></span> |<span data-ttu-id="94c9f-122">2016. Nano Server</span><span class="sxs-lookup"><span data-stu-id="94c9f-122">2016-Nano-Server</span></span> |
| <span data-ttu-id="94c9f-123">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-123">MicrosoftWindowsServer</span></span> |<span data-ttu-id="94c9f-124">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-124">WindowsServer</span></span> |<span data-ttu-id="94c9f-125">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="94c9f-125">2012-R2-Datacenter</span></span> |
| <span data-ttu-id="94c9f-126">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-126">MicrosoftWindowsServer</span></span> |<span data-ttu-id="94c9f-127">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-127">WindowsServer</span></span> |<span data-ttu-id="94c9f-128">AKTUALIZACE SP1 2008 R2</span><span class="sxs-lookup"><span data-stu-id="94c9f-128">2008-R2-SP1</span></span> |
| <span data-ttu-id="94c9f-129">MicrosoftDynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="94c9f-129">MicrosoftDynamicsNAV</span></span> |<span data-ttu-id="94c9f-130">DynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="94c9f-130">DynamicsNAV</span></span> |<span data-ttu-id="94c9f-131">2017</span><span class="sxs-lookup"><span data-stu-id="94c9f-131">2017</span></span> |
| <span data-ttu-id="94c9f-132">MicrosoftSharePoint</span><span class="sxs-lookup"><span data-stu-id="94c9f-132">MicrosoftSharePoint</span></span> |<span data-ttu-id="94c9f-133">MicrosoftSharePointServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-133">MicrosoftSharePointServer</span></span> |<span data-ttu-id="94c9f-134">2016</span><span class="sxs-lookup"><span data-stu-id="94c9f-134">2016</span></span> |
| <span data-ttu-id="94c9f-135">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-135">MicrosoftSQLServer</span></span> |<span data-ttu-id="94c9f-136">SQL2016 WS2016</span><span class="sxs-lookup"><span data-stu-id="94c9f-136">SQL2016-WS2016</span></span> |<span data-ttu-id="94c9f-137">Enterprise</span><span class="sxs-lookup"><span data-stu-id="94c9f-137">Enterprise</span></span> |
| <span data-ttu-id="94c9f-138">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="94c9f-138">MicrosoftSQLServer</span></span> |<span data-ttu-id="94c9f-139">SQL2014SP2 WS2012R2</span><span class="sxs-lookup"><span data-stu-id="94c9f-139">SQL2014SP2-WS2012R2</span></span> |<span data-ttu-id="94c9f-140">Enterprise</span><span class="sxs-lookup"><span data-stu-id="94c9f-140">Enterprise</span></span> |
| <span data-ttu-id="94c9f-141">MicrosoftWindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="94c9f-141">MicrosoftWindowsServerHPCPack</span></span> |<span data-ttu-id="94c9f-142">WindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="94c9f-142">WindowsServerHPCPack</span></span> |<span data-ttu-id="94c9f-143">2012R2</span><span class="sxs-lookup"><span data-stu-id="94c9f-143">2012R2</span></span> |
| <span data-ttu-id="94c9f-144">MicrosoftWindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="94c9f-144">MicrosoftWindowsServerEssentials</span></span> |<span data-ttu-id="94c9f-145">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="94c9f-145">WindowsServerEssentials</span></span> |<span data-ttu-id="94c9f-146">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="94c9f-146">WindowsServerEssentials</span></span> |

## <a name="find-specific-images"></a><span data-ttu-id="94c9f-147">Vyhledání konkrétních imagí</span><span class="sxs-lookup"><span data-stu-id="94c9f-147">Find specific images</span></span>


<span data-ttu-id="94c9f-148">Při vytváření nového virtuálního počítače pomocí Azure Resource Manageru, v některých případech je třeba toospecify bitovou kopii s kombinací hello hello následující vlastnosti bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="94c9f-148">When creating a new virtual machine with Azure Resource Manager, in some cases you need toospecify an image with hello combination of hello following image properties:</span></span>

* <span data-ttu-id="94c9f-149">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="94c9f-149">Publisher</span></span>
* <span data-ttu-id="94c9f-150">Nabídka</span><span class="sxs-lookup"><span data-stu-id="94c9f-150">Offer</span></span>
* <span data-ttu-id="94c9f-151">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="94c9f-151">SKU</span></span>

<span data-ttu-id="94c9f-152">Například použít tyto hodnoty s hello [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) rutiny prostředí PowerShell nebo pomocí šablony skupiny prostředků v které je nutné zadat typ hello toobe virtuální počítač vytvořit.</span><span class="sxs-lookup"><span data-stu-id="94c9f-152">For example, use these values with hello [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell cmdlet, or with a resource group template in which you must specify hello type of VM toobe created.</span></span>

<span data-ttu-id="94c9f-153">Pokud potřebujete toodetermine tyto hodnoty, můžete spustit hello [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), a [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) rutiny toonavigate hello Image.</span><span class="sxs-lookup"><span data-stu-id="94c9f-153">If you need toodetermine these values, you can run hello [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), and [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets toonavigate hello images.</span></span> <span data-ttu-id="94c9f-154">Můžete určit tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="94c9f-154">You determine these values:</span></span>

1. <span data-ttu-id="94c9f-155">Seznam vydavatelů image hello.</span><span class="sxs-lookup"><span data-stu-id="94c9f-155">List hello image publishers.</span></span>
2. <span data-ttu-id="94c9f-156">Pro daného vydavatele vypsat jeho nabídky.</span><span class="sxs-lookup"><span data-stu-id="94c9f-156">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="94c9f-157">Pro danou nabídku vypsat její skladovou jednotku (SKU).</span><span class="sxs-lookup"><span data-stu-id="94c9f-157">For a given offer, list their SKUs.</span></span>

<span data-ttu-id="94c9f-158">Nejprve seznam vydavatelů hello s hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="94c9f-158">First, list hello publishers with hello following commands:</span></span>

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

<span data-ttu-id="94c9f-159">Zadejte název vydavatele zvolené a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="94c9f-159">Fill in your chosen publisher name and run hello following commands:</span></span>

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="94c9f-160">Zadejte název vaší zvolené nabídka a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="94c9f-160">Fill in your chosen offer name and run hello following commands:</span></span>

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="94c9f-161">Z hello výstup hello `Get-AzureRMVMImageSku` příkaz, budete mít všechny informace o hello potřebujete toospecify hello image pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="94c9f-161">From hello output of hello `Get-AzureRMVMImageSku` command, you have all hello information you need toospecify hello image for a new virtual machine.</span></span>

<span data-ttu-id="94c9f-162">Úplný příklad ukazuje, Hello následující:</span><span class="sxs-lookup"><span data-stu-id="94c9f-162">hello following shows a full example:</span></span>

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

<span data-ttu-id="94c9f-163">Výstup:</span><span class="sxs-lookup"><span data-stu-id="94c9f-163">Output:</span></span>

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

<span data-ttu-id="94c9f-164">Pro vydavatele "MicrosoftWindowsServer" hello:</span><span class="sxs-lookup"><span data-stu-id="94c9f-164">For hello "MicrosoftWindowsServer" publisher:</span></span>

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="94c9f-165">Výstup:</span><span class="sxs-lookup"><span data-stu-id="94c9f-165">Output:</span></span>

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

<span data-ttu-id="94c9f-166">Pro nabídka "Windows Server" hello:</span><span class="sxs-lookup"><span data-stu-id="94c9f-166">For hello "WindowsServer" offer:</span></span>

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="94c9f-167">Výstup:</span><span class="sxs-lookup"><span data-stu-id="94c9f-167">Output:</span></span>

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

<span data-ttu-id="94c9f-168">Z tohoto seznamu, zkopírujte hello zvolený název SKU, a budete mít všechny informace hello hello `Set-AzureRMVMSourceImage` rutiny prostředí PowerShell nebo pro šablonu skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="94c9f-168">From this list, copy hello chosen SKU name, and you have all hello information for hello `Set-AzureRMVMSourceImage` PowerShell cmdlet or for a resource group template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94c9f-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94c9f-169">Next steps</span></span>
<span data-ttu-id="94c9f-170">Teď můžete zvolit přesněji hello obraz chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="94c9f-170">Now you can choose precisely hello image you want toouse.</span></span> <span data-ttu-id="94c9f-171">najdete v části virtuální počítač rychle pomocí informace o obrázku hello, který jste právě najít, toocreate [vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="94c9f-171">toocreate a virtual machine quickly by using hello image information, which you just found, see [Create a Windows virtual machine with PowerShell](quick-create-powershell.md).</span></span>
