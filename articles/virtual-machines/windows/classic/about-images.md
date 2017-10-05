---
title: "O bitových kopií pro virtuální počítače s Windows | Microsoft Docs"
description: "Další informace o použití bitové kopie s virtuální počítače s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cynthn
ms.openlocfilehash: d421cee0becabdf81d865036d0c98b12b077152b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="about-images-for-windows-virtual-machines"></a><span data-ttu-id="f9107-103">O bitových kopií pro virtuální počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="f9107-103">About images for Windows virtual machines</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f9107-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f9107-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f9107-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="f9107-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="f9107-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f9107-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="f9107-107">Informace o hledání a používání imagí v modelu Resource Manager najdete v tématu [zde](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f9107-107">For information about finding and using images in the Resource Manager model, see [here](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a><span data-ttu-id="f9107-108">Práce s obrázky</span><span class="sxs-lookup"><span data-stu-id="f9107-108">Working with images</span></span>

<span data-ttu-id="f9107-109">Modul Azure PowerShell a portálu Azure můžete použít ke správě bitových kopií, která je k dispozici k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="f9107-109">You can use the Azure PowerShell module and the Azure portal to manage the images available to your Azure subscription.</span></span> <span data-ttu-id="f9107-110">Modul Azure PowerShell poskytuje další parametry příkazu, takže přesně určit pouze požadované anebo najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="f9107-110">The Azure PowerShell module provides more command options, so you can pinpoint exactly what you want to see or do.</span></span> <span data-ttu-id="f9107-111">Portál Azure poskytuje grafickým uživatelským rozhraním pro řadu každodenních úloh pro správu.</span><span class="sxs-lookup"><span data-stu-id="f9107-111">The Azure portal provides a GUI for many of the everyday administrative tasks.</span></span>

<span data-ttu-id="f9107-112">Zde jsou některé příklady, které používají modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f9107-112">Here are some examples that use the Azure PowerShell module.</span></span>

* <span data-ttu-id="f9107-113">**Získat všechny image**:`Get-AzureVMImage`vrátí seznam hodnot všechny bitové kopie k dispozici v aktuálním předplatném: vaše Image a ty poskytovaný Azure nebo partnerů.</span><span class="sxs-lookup"><span data-stu-id="f9107-113">**Get all images**:`Get-AzureVMImage`returns a list of all the images available in your current subscription: your images and those provided by Azure or partners.</span></span> <span data-ttu-id="f9107-114">V rozevíracím seznamu může být velký.</span><span class="sxs-lookup"><span data-stu-id="f9107-114">The resulting list could be large.</span></span> <span data-ttu-id="f9107-115">Další příklady ukazují, jak získat seznam kratší.</span><span class="sxs-lookup"><span data-stu-id="f9107-115">The next examples show how to get a shorter list.</span></span>
* <span data-ttu-id="f9107-116">**Získat image rodiny**:`Get-AzureVMImage | select ImageFamily` získá seznam image rodiny zobrazením řetězce **ImageFamily** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f9107-116">**Get image families**:`Get-AzureVMImage | select ImageFamily` gets a list of image families by showing strings **ImageFamily** property.</span></span>
* <span data-ttu-id="f9107-117">**Získat všechny bitové kopie v konkrétní rodině**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`</span><span class="sxs-lookup"><span data-stu-id="f9107-117">**Get all images in a specific family**: `Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`</span></span>
* <span data-ttu-id="f9107-118">**Najít Image virtuálních počítačů**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` Tato rutina funguje tak, že filtrování DataDiskConfiguration vlastnost, která se vztahuje pouze na Image virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f9107-118">**Find VM Images**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` This cmdlet works by filtering the DataDiskConfiguration property, which only applies to VM Images.</span></span> <span data-ttu-id="f9107-119">Tento příklad také vyfiltruje výstup jenom na název popisku a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="f9107-119">This example also filters the output to only the label and image name.</span></span>
* <span data-ttu-id="f9107-120">**Uložte bitovou kopii zobecněný**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`</span><span class="sxs-lookup"><span data-stu-id="f9107-120">**Save a generalized image**: `Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`</span></span>
* <span data-ttu-id="f9107-121">**Uložte bitovou kopii specializované**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`</span><span class="sxs-lookup"><span data-stu-id="f9107-121">**Save a specialized image**: `Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`</span></span>

  > [!TIP]
  > <span data-ttu-id="f9107-122">Parametr OSState je potřeba k vytvoření image virtuálního počítače, která obsahuje disk operačního systému a datové disky připojené.</span><span class="sxs-lookup"><span data-stu-id="f9107-122">The OSState parameter is required to create a VM image, which includes the operating system disk and attached data disks.</span></span> <span data-ttu-id="f9107-123">Pokud nepoužijete parametr, rutina vytvoří bitovou kopii operačního systému.</span><span class="sxs-lookup"><span data-stu-id="f9107-123">If you don’t use the parameter, the cmdlet creates an OS image.</span></span> <span data-ttu-id="f9107-124">Hodnota parametru udává, zda obrázek zobecněn nebo specializuje, na základě na tom, jestli disk operačního systému je připravena pro opakované použití.</span><span class="sxs-lookup"><span data-stu-id="f9107-124">The value of the parameter indicates whether the image is generalized or specialized, based on whether the operating system disk has been prepared for reuse.</span></span>

* <span data-ttu-id="f9107-125">**Odstranit bitovou kopii**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`</span><span class="sxs-lookup"><span data-stu-id="f9107-125">**Delete an image**: `Remove-AzureVMImage –ImageName "MyOldVmImage"`</span></span>

## <a name="next-steps"></a><span data-ttu-id="f9107-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f9107-126">Next Steps</span></span>
<span data-ttu-id="f9107-127">Můžete také [vytvoření počítače s Windows pomocí portálu Azure](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="f9107-127">You can also [create a Windows machine using the Azure portal](tutorial.md).</span></span>
