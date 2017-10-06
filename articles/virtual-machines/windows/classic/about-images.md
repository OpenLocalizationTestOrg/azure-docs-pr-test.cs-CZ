---
title: "aaaAbout bitových kopií pro virtuální počítače s Windows | Microsoft Docs"
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
ms.openlocfilehash: c7cfa1d018a5e99d5b68f559ec9ae1f14e4dec8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-images-for-windows-virtual-machines"></a><span data-ttu-id="1333f-103">O bitových kopií pro virtuální počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="1333f-103">About images for Windows virtual machines</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1333f-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="1333f-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="1333f-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="1333f-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="1333f-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="1333f-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="1333f-107">Informace o hledání a používání imagí v modelu Resource Manager hello najdete v tématu [zde](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1333f-107">For information about finding and using images in hello Resource Manager model, see [here](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a><span data-ttu-id="1333f-108">Práce s obrázky</span><span class="sxs-lookup"><span data-stu-id="1333f-108">Working with images</span></span>

<span data-ttu-id="1333f-109">Můžete použít modul Azure PowerShell hello a hello Azure portálu toomanage hello bitové kopie k dispozici tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="1333f-109">You can use hello Azure PowerShell module and hello Azure portal toomanage hello images available tooyour Azure subscription.</span></span> <span data-ttu-id="1333f-110">modul Azure PowerShell Hello poskytuje další parametry příkazu, takže lze přesně určit, jak přesné toosee nebo proveďte.</span><span class="sxs-lookup"><span data-stu-id="1333f-110">hello Azure PowerShell module provides more command options, so you can pinpoint exactly what you want toosee or do.</span></span> <span data-ttu-id="1333f-111">Hello portál Azure poskytuje grafické rozhraní pro řadu úloh správy každý den hello.</span><span class="sxs-lookup"><span data-stu-id="1333f-111">hello Azure portal provides a GUI for many of hello everyday administrative tasks.</span></span>

<span data-ttu-id="1333f-112">Zde jsou některé příklady, které používají modul Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="1333f-112">Here are some examples that use hello Azure PowerShell module.</span></span>

* <span data-ttu-id="1333f-113">**Získat všechny image**:`Get-AzureVMImage`vrátí seznam všech hello obrázků dostupné v aktuálním předplatném: vaše Image a ty poskytovaný Azure nebo partnerů.</span><span class="sxs-lookup"><span data-stu-id="1333f-113">**Get all images**:`Get-AzureVMImage`returns a list of all hello images available in your current subscription: your images and those provided by Azure or partners.</span></span> <span data-ttu-id="1333f-114">Výsledný seznam Hello může být velký.</span><span class="sxs-lookup"><span data-stu-id="1333f-114">hello resulting list could be large.</span></span> <span data-ttu-id="1333f-115">Zobrazit další příklady jak Hello tooget seznam kratší.</span><span class="sxs-lookup"><span data-stu-id="1333f-115">hello next examples show how tooget a shorter list.</span></span>
* <span data-ttu-id="1333f-116">**Získat image rodiny**:`Get-AzureVMImage | select ImageFamily` získá seznam image rodiny zobrazením řetězce **ImageFamily** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1333f-116">**Get image families**:`Get-AzureVMImage | select ImageFamily` gets a list of image families by showing strings **ImageFamily** property.</span></span>
* <span data-ttu-id="1333f-117">**Získat všechny bitové kopie v konkrétní rodině**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`</span><span class="sxs-lookup"><span data-stu-id="1333f-117">**Get all images in a specific family**: `Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`</span></span>
* <span data-ttu-id="1333f-118">**Najít Image virtuálních počítačů**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` Tato rutina funguje tak, že filtrování hello DataDiskConfiguration vlastnosti, která se vztahuje pouze tooVM bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1333f-118">**Find VM Images**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` This cmdlet works by filtering hello DataDiskConfiguration property, which only applies tooVM Images.</span></span> <span data-ttu-id="1333f-119">Tento příklad také vyfiltruje hello výstup tooonly hello label a název obrázku.</span><span class="sxs-lookup"><span data-stu-id="1333f-119">This example also filters hello output tooonly hello label and image name.</span></span>
* <span data-ttu-id="1333f-120">**Uložte bitovou kopii zobecněný**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`</span><span class="sxs-lookup"><span data-stu-id="1333f-120">**Save a generalized image**: `Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`</span></span>
* <span data-ttu-id="1333f-121">**Uložte bitovou kopii specializované**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`</span><span class="sxs-lookup"><span data-stu-id="1333f-121">**Save a specialized image**: `Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`</span></span>

  > [!TIP]
  > <span data-ttu-id="1333f-122">Parametr OSState Hello je požadovaná toocreate image virtuálního počítače, který zahrnuje hello disk operačního systému a datové disky připojené.</span><span class="sxs-lookup"><span data-stu-id="1333f-122">hello OSState parameter is required toocreate a VM image, which includes hello operating system disk and attached data disks.</span></span> <span data-ttu-id="1333f-123">Pokud nepoužijete parametr hello, hello rutina vytvoří bitovou kopii operačního systému.</span><span class="sxs-lookup"><span data-stu-id="1333f-123">If you don’t use hello parameter, hello cmdlet creates an OS image.</span></span> <span data-ttu-id="1333f-124">Hello hodnota parametru hello označuje, zda obrázek hello je zobecněn nebo specializuje, na základě na tom, jestli disk operačního systému hello připravený pro opakované použití.</span><span class="sxs-lookup"><span data-stu-id="1333f-124">hello value of hello parameter indicates whether hello image is generalized or specialized, based on whether hello operating system disk has been prepared for reuse.</span></span>

* <span data-ttu-id="1333f-125">**Odstranit bitovou kopii**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`</span><span class="sxs-lookup"><span data-stu-id="1333f-125">**Delete an image**: `Remove-AzureVMImage –ImageName "MyOldVmImage"`</span></span>

## <a name="next-steps"></a><span data-ttu-id="1333f-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1333f-126">Next Steps</span></span>
<span data-ttu-id="1333f-127">Můžete také [vytvoření počítače s Windows pomocí portálu Azure hello](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="1333f-127">You can also [create a Windows machine using hello Azure portal](tutorial.md).</span></span>
