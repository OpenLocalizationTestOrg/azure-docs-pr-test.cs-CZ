---
title: "Nahrání souboru virtuálního pevného disku do Azure DevTest Labs pomocí AzCopy | Microsoft Docs"
description: "Nahrání souboru virtuálního pevného disku do testovacího prostředí na účtu úložiště pomocí nástroje AzCopy"
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
ms.openlocfilehash: a4f43354740d9f17570932b0b9c753f46d67dc33
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a><span data-ttu-id="ac617-103">Nahrání souboru virtuálního pevného disku do testovacího prostředí na účtu úložiště pomocí nástroje AzCopy</span><span class="sxs-lookup"><span data-stu-id="ac617-103">Upload VHD file to lab's storage account using AzCopy</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="ac617-104">V Azure DevTest Labs soubory virtuálního pevného disku lze vytvořit vlastní Image, které se používají ke zřízení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ac617-104">In Azure DevTest Labs, VHD files can be used to create custom images, which are used to provision virtual machines.</span></span> <span data-ttu-id="ac617-105">Následující kroky vás provedou pomocí příkazového řádku azcopy Pokud chcete nahrát soubor virtuálního pevného disku do účtu úložiště testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ac617-105">The following steps walk you through using the AzCopy command-line utility to upload a VHD file to a lab's storage account.</span></span> <span data-ttu-id="ac617-106">Jakmile jste odeslali souboru virtuálního pevného disku [další kroky části](#next-steps) jsou uvedeny některé články, které ukazují, jak vytvořit vlastní image z nahrávaný soubor virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="ac617-106">Once you've uploaded your VHD file, the [Next steps section](#next-steps) lists some articles that illustrate how to create a custom image from the uploaded VHD file.</span></span> <span data-ttu-id="ac617-107">Další informace o disky a virtuální pevné disky v Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="ac617-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

> [!NOTE] 
>  
> <span data-ttu-id="ac617-108">AzCopy je nástroj příkazového řádku pouze pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="ac617-108">AzCopy is a Windows-only command-line utility.</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="ac617-109">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="ac617-109">Step-by-step instructions</span></span>

<span data-ttu-id="ac617-110">Následující kroky vás provedou nahrání souboru virtuálního pevného disku do Azure DevTest Labs pomocí [AzCopy](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="ac617-110">The following steps walk you through uploading a VHD file to Azure DevTest Labs using [AzCopy](http://aka.ms/downloadazcopy).</span></span> 

1. <span data-ttu-id="ac617-111">Získání názvu účtu úložiště v prostředí pomocí portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="ac617-111">Get the name of the lab's storage account using the Azure portal:</span></span>

1. <span data-ttu-id="ac617-112">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="ac617-112">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="ac617-113">Vyberte **Další služby** a poté ze seznamu vyberte **DevTest Labs**.</span><span class="sxs-lookup"><span data-stu-id="ac617-113">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="ac617-114">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="ac617-114">From the list of labs, select the desired lab.</span></span>  

1. <span data-ttu-id="ac617-115">V okně v prostředí, vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="ac617-115">On the lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="ac617-116">V testovacím prostředí **konfigurace** vyberte **vlastní Image (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="ac617-116">On the lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="ac617-117">Na **vlastní image** okně vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="ac617-117">On the **Custom images** blade, Select **+Add**.</span></span> 

1. <span data-ttu-id="ac617-118">Na **vlastní image** vyberte **virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="ac617-118">On the **Custom image** blade, select **VHD**.</span></span>

1. <span data-ttu-id="ac617-119">Na **virtuálního pevného disku** vyberte **nahrát virtuální pevný disk pomocí prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="ac617-119">On the **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>

    ![Nahrání virtuálního pevného disku pomocí prostředí PowerShell](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. <span data-ttu-id="ac617-121">**Odeslat bitovou kopii pomocí prostředí PowerShell** zobrazuje volání **přidat AzureVhd** rutiny.</span><span class="sxs-lookup"><span data-stu-id="ac617-121">The **Upload an image using PowerShell** blade displays a call to the **Add-AzureVhd** cmdlet.</span></span> <span data-ttu-id="ac617-122">První parametr (*cílové*) obsahuje identifikátor URI pro kontejner objektů blob (*odešle*) v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="ac617-122">The first parameter (*Destination*) contains the URI for a blob container (*uploads*) in the following format:</span></span>

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. <span data-ttu-id="ac617-123">Poznamenejte si úplný identifikátor URI jako se používá v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="ac617-123">Make note of the full URI as it is used in later steps.</span></span>

1. <span data-ttu-id="ac617-124">Nahrajte soubor virtuálního pevného disku pomocí nástroje AzCopy:</span><span class="sxs-lookup"><span data-stu-id="ac617-124">Upload the VHD file using AzCopy:</span></span>
 
1. <span data-ttu-id="ac617-125">[Stáhněte a nainstalujte nejnovější verzi AzCopy](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="ac617-125">[Download and install the latest version of AzCopy](http://aka.ms/downloadazcopy).</span></span>

1. <span data-ttu-id="ac617-126">Otevřete okno příkazového řádku a přejděte do instalačního adresáře nástroje AzCopy.</span><span class="sxs-lookup"><span data-stu-id="ac617-126">Open a command window and navigate to the AzCopy installation directory.</span></span> <span data-ttu-id="ac617-127">Umístění instalace AzCopy můžete volitelně přidat cestu v systému.</span><span class="sxs-lookup"><span data-stu-id="ac617-127">Optionally, you can add the AzCopy installation location to your system path.</span></span> <span data-ttu-id="ac617-128">Ve výchozím nastavení je AzCopy nainstalovat na následující adresář:</span><span class="sxs-lookup"><span data-stu-id="ac617-128">By default, AzCopy is installed to the following directory:</span></span>

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. <span data-ttu-id="ac617-129">Pomocí účtu klíč a objektů blob kontejneru úložiště identifikátor URI, spusťte následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="ac617-129">Using the storage account key and blob container URI, run the following command at the command prompt.</span></span> <span data-ttu-id="ac617-130">*VhdFileName* hodnota musí být v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="ac617-130">The *vhdFileName* value needs to be in quotes.</span></span> <span data-ttu-id="ac617-131">Proces odesílání soubor virtuálního pevného disku může být náročná v závislosti na velikosti souboru virtuálního pevného disku a rychlost připojení.</span><span class="sxs-lookup"><span data-stu-id="ac617-131">The process of uploading a VHD file can be lengthy depending on the size of the VHD file and your connection speed.</span></span>   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a><span data-ttu-id="ac617-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac617-132">Next steps</span></span>

- [<span data-ttu-id="ac617-133">Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ac617-133">Create a custom image in Azure DevTest Labs from a VHD file using the Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="ac617-134">Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac617-134">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)