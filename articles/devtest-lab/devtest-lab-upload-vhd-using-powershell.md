---
title: "Nahrání souboru virtuálního pevného disku do Azure DevTest Labs pomocí prostředí PowerShell | Microsoft Docs"
description: "Nahrání souboru virtuálního pevného disku do testovacího prostředí na účet úložiště pomocí prostředí PowerShell"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 3c43ef77b8fa10cd6dbd726968264f32f7a3dd0f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a><span data-ttu-id="ac44f-103">Nahrání souboru virtuálního pevného disku do testovacího prostředí na účet úložiště pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac44f-103">Upload VHD file to lab's storage account using PowerShell</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="ac44f-104">V Azure DevTest Labs soubory virtuálního pevného disku lze vytvořit vlastní Image, které se používají ke zřízení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ac44f-104">In Azure DevTest Labs, VHD files can be used to create custom images, which are used to provision virtual machines.</span></span> <span data-ttu-id="ac44f-105">Následující kroky vás provedou pomocí prostředí PowerShell pro nahrání souboru virtuálního pevného disku k účtu úložiště testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ac44f-105">The following steps walk you through using PowerShell to upload a VHD file to a lab's storage account.</span></span> <span data-ttu-id="ac44f-106">Jakmile jste odeslali souboru virtuálního pevného disku [další kroky části](#next-steps) jsou uvedeny některé články, které ukazují, jak vytvořit vlastní image z nahrávaný soubor virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="ac44f-106">Once you've uploaded your VHD file, the [Next steps section](#next-steps) lists some articles that illustrate how to create a custom image from the uploaded VHD file.</span></span> <span data-ttu-id="ac44f-107">Další informace o disky a virtuální pevné disky v Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="ac44f-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="ac44f-108">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="ac44f-108">Step-by-step instructions</span></span>

<span data-ttu-id="ac44f-109">Následující kroky vás provedou nahrání souboru virtuálního pevného disku do Azure DevTest Labs pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ac44f-109">The following steps walk you through uploading a VHD file to Azure DevTest Labs using PowerShell.</span></span> 

1. <span data-ttu-id="ac44f-110">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="ac44f-110">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="ac44f-111">Vyberte **Další služby** a poté ze seznamu vyberte **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="ac44f-111">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="ac44f-112">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="ac44f-112">From the list of labs, select the desired lab.</span></span>  

1. <span data-ttu-id="ac44f-113">V okně v prostředí, vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="ac44f-113">On the lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="ac44f-114">V testovacím prostředí **konfigurace** vyberte **vlastní Image (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="ac44f-114">On the lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="ac44f-115">Na **vlastní image** okně vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="ac44f-115">On the **Custom images** blade, Select **+Add**.</span></span> 

1. <span data-ttu-id="ac44f-116">Na **vlastní image** vyberte **virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="ac44f-116">On the **Custom image** blade, select **VHD**.</span></span>

1. <span data-ttu-id="ac44f-117">Na **virtuálního pevného disku** vyberte **nahrát virtuální pevný disk pomocí prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="ac44f-117">On the **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>

    ![Nahrání virtuálního pevného disku pomocí prostředí PowerShell](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. <span data-ttu-id="ac44f-119">Na **Odeslat bitovou kopii pomocí prostředí PowerShell** okně zkopírovat do textového editoru generovaného skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ac44f-119">On the **Upload an image using PowerShell** blade, copy the generated PowerShell script to a text editor.</span></span>

1. <span data-ttu-id="ac44f-120">Změnit **LocalFilePath** parametr **přidat AzureVhd** rutiny tak, aby odkazovalo na umístění souboru virtuálního pevného disku, který chcete odeslat.</span><span class="sxs-lookup"><span data-stu-id="ac44f-120">Modify the **LocalFilePath** parameter of the **Add-AzureVhd** cmdlet to point to the location of the VHD file you want to upload.</span></span>

1. <span data-ttu-id="ac44f-121">V řádku prostředí PowerShell, spusťte **přidat AzureVhd** rutiny (s upravenou **LocalFilePath** parametr).</span><span class="sxs-lookup"><span data-stu-id="ac44f-121">At a PowerShell prompt, run the **Add-AzureVhd** cmdlet (with the modified **LocalFilePath** parameter).</span></span>

> [!WARNING] 
> 
> <span data-ttu-id="ac44f-122">Proces odesílání soubor virtuálního pevného disku může být náročná v závislosti na velikosti souboru virtuálního pevného disku a rychlost připojení.</span><span class="sxs-lookup"><span data-stu-id="ac44f-122">The process of uploading a VHD file can be lengthy depending on the size of the VHD file and your connection speed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac44f-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac44f-123">Next steps</span></span>

- [<span data-ttu-id="ac44f-124">Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ac44f-124">Create a custom image in Azure DevTest Labs from a VHD file using the Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="ac44f-125">Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac44f-125">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
