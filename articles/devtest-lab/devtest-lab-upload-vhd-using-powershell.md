---
title: "aaaUpload virtuálního pevného disku soubor tooAzure DevTest Labs pomocí prostředí PowerShell | Microsoft Docs"
description: "Nahrání virtuálního pevného disku soubor toolab účet úložiště pomocí prostředí PowerShell"
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
ms.openlocfilehash: 9c3ee96e212457b0ef8203714b419350cb97f895
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-powershell"></a><span data-ttu-id="2418f-103">Nahrání virtuálního pevného disku soubor toolab účet úložiště pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2418f-103">Upload VHD file toolab's storage account using PowerShell</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="2418f-104">V Azure DevTest Labs může být soubory VHD použité toocreate vlastní Image, které jsou používané tooprovision virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2418f-104">In Azure DevTest Labs, VHD files can be used toocreate custom images, which are used tooprovision virtual machines.</span></span> <span data-ttu-id="2418f-105">Hello následující kroky vás provedou pomocí prostředí PowerShell tooupload virtuálního pevného disku soubor tooa testovacího prostředí na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="2418f-105">hello following steps walk you through using PowerShell tooupload a VHD file tooa lab's storage account.</span></span> <span data-ttu-id="2418f-106">Jakmile jste odeslali souboru virtuálního pevného disku, hello [další kroky části](#next-steps) jsou uvedeny některé články, které ilustrují, jak toocreate vlastní image z hello nahrát soubor virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="2418f-106">Once you've uploaded your VHD file, hello [Next steps section](#next-steps) lists some articles that illustrate how toocreate a custom image from hello uploaded VHD file.</span></span> <span data-ttu-id="2418f-107">Další informace o disky a virtuální pevné disky v Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="2418f-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="2418f-108">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="2418f-108">Step-by-step instructions</span></span>

<span data-ttu-id="2418f-109">Hello následující kroky vás odesílání virtuálního pevného disku soubor tooAzure DevTest Labs pomocí prostředí PowerShell procházení.</span><span class="sxs-lookup"><span data-stu-id="2418f-109">hello following steps walk you through uploading a VHD file tooAzure DevTest Labs using PowerShell.</span></span> 

1. <span data-ttu-id="2418f-110">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="2418f-110">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="2418f-111">Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="2418f-111">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="2418f-112">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="2418f-112">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="2418f-113">V okně prostředí hello vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="2418f-113">On hello lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="2418f-114">V testovacím hello **konfigurace** vyberte **vlastní Image (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="2418f-114">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="2418f-115">Na hello **vlastní image** okně vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="2418f-115">On hello **Custom images** blade, Select **+Add**.</span></span> 

1. <span data-ttu-id="2418f-116">Na hello **vlastní image** vyberte **virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="2418f-116">On hello **Custom image** blade, select **VHD**.</span></span>

1. <span data-ttu-id="2418f-117">Na hello **virtuálního pevného disku** vyberte **nahrát virtuální pevný disk pomocí prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="2418f-117">On hello **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>

    ![Nahrání virtuálního pevného disku pomocí prostředí PowerShell](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. <span data-ttu-id="2418f-119">Na hello **Odeslat bitovou kopii pomocí prostředí PowerShell** okno, kopie hello generované prostředí PowerShell skriptu tooa textový editor.</span><span class="sxs-lookup"><span data-stu-id="2418f-119">On hello **Upload an image using PowerShell** blade, copy hello generated PowerShell script tooa text editor.</span></span>

1. <span data-ttu-id="2418f-120">Upravit hello **LocalFilePath** parametr hello **přidat AzureVhd** rutiny toopoint toohello umístění hello chcete tooupload souboru virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="2418f-120">Modify hello **LocalFilePath** parameter of hello **Add-AzureVhd** cmdlet toopoint toohello location of hello VHD file you want tooupload.</span></span>

1. <span data-ttu-id="2418f-121">V řádku prostředí PowerShell, spusťte hello **přidat AzureVhd** rutiny (s hello upravit **LocalFilePath** parametr).</span><span class="sxs-lookup"><span data-stu-id="2418f-121">At a PowerShell prompt, run hello **Add-AzureVhd** cmdlet (with hello modified **LocalFilePath** parameter).</span></span>

> [!WARNING] 
> 
> <span data-ttu-id="2418f-122">Hello proces odesílání soubor virtuálního pevného disku může být náročná v závislosti na velikosti hello hello souboru virtuálního pevného disku a rychlost připojení.</span><span class="sxs-lookup"><span data-stu-id="2418f-122">hello process of uploading a VHD file can be lengthy depending on hello size of hello VHD file and your connection speed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2418f-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2418f-123">Next steps</span></span>

- [<span data-ttu-id="2418f-124">Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2418f-124">Create a custom image in Azure DevTest Labs from a VHD file using hello Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="2418f-125">Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2418f-125">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
