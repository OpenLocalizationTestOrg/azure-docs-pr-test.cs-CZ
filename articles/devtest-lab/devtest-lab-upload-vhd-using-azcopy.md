---
title: "aaaUpload virtuálního pevného disku soubor pomocí nástroje AzCopy tooAzure DevTest Labs | Microsoft Docs"
description: "Nahrát účtu úložiště toolab souboru virtuálního pevného disku pomocí nástroje AzCopy"
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
ms.openlocfilehash: 14f9e933b0bd27451f6bcb94841ecc381213e578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-azcopy"></a><span data-ttu-id="75172-103">Nahrát účtu úložiště toolab souboru virtuálního pevného disku pomocí nástroje AzCopy</span><span class="sxs-lookup"><span data-stu-id="75172-103">Upload VHD file toolab's storage account using AzCopy</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="75172-104">V Azure DevTest Labs může být soubory VHD použité toocreate vlastní Image, které jsou používané tooprovision virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="75172-104">In Azure DevTest Labs, VHD files can be used toocreate custom images, which are used tooprovision virtual machines.</span></span> <span data-ttu-id="75172-105">Hello následující kroky vás provedou pomocí tooupload nástroj příkazového řádku AzCopy hello virtuálního pevného disku soubor tooa testovacího prostředí na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="75172-105">hello following steps walk you through using hello AzCopy command-line utility tooupload a VHD file tooa lab's storage account.</span></span> <span data-ttu-id="75172-106">Jakmile jste odeslali souboru virtuálního pevného disku, hello [další kroky části](#next-steps) jsou uvedeny některé články, které ilustrují, jak toocreate vlastní image z hello nahrát soubor virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="75172-106">Once you've uploaded your VHD file, hello [Next steps section](#next-steps) lists some articles that illustrate how toocreate a custom image from hello uploaded VHD file.</span></span> <span data-ttu-id="75172-107">Další informace o disky a virtuální pevné disky v Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="75172-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

> [!NOTE] 
>  
> <span data-ttu-id="75172-108">AzCopy je nástroj příkazového řádku pouze pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="75172-108">AzCopy is a Windows-only command-line utility.</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="75172-109">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="75172-109">Step-by-step instructions</span></span>

<span data-ttu-id="75172-110">Hello následující kroky procházení vás odesílání virtuálního pevného disku soubor tooAzure DevTest Labs pomocí [AzCopy](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="75172-110">hello following steps walk you through uploading a VHD file tooAzure DevTest Labs using [AzCopy](http://aka.ms/downloadazcopy).</span></span> 

1. <span data-ttu-id="75172-111">Získáte hello název účtu úložiště hello prostředí pomocí hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="75172-111">Get hello name of hello lab's storage account using hello Azure portal:</span></span>

1. <span data-ttu-id="75172-112">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="75172-112">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="75172-113">Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="75172-113">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="75172-114">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="75172-114">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="75172-115">V okně prostředí hello vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="75172-115">On hello lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="75172-116">V testovacím hello **konfigurace** vyberte **vlastní Image (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="75172-116">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="75172-117">Na hello **vlastní image** okně vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="75172-117">On hello **Custom images** blade, Select **+Add**.</span></span> 

1. <span data-ttu-id="75172-118">Na hello **vlastní image** vyberte **virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="75172-118">On hello **Custom image** blade, select **VHD**.</span></span>

1. <span data-ttu-id="75172-119">Na hello **virtuálního pevného disku** vyberte **nahrát virtuální pevný disk pomocí prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="75172-119">On hello **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>

    ![Nahrání virtuálního pevného disku pomocí prostředí PowerShell](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. <span data-ttu-id="75172-121">Hello **Odeslat bitovou kopii pomocí prostředí PowerShell** zobrazuje volání toohello **přidat AzureVhd** rutiny.</span><span class="sxs-lookup"><span data-stu-id="75172-121">hello **Upload an image using PowerShell** blade displays a call toohello **Add-AzureVhd** cmdlet.</span></span> <span data-ttu-id="75172-122">první parametr Hello (*cílové*) obsahuje hello identifikátor URI pro kontejner objektů blob (*odešle*) ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="75172-122">hello first parameter (*Destination*) contains hello URI for a blob container (*uploads*) in hello following format:</span></span>

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. <span data-ttu-id="75172-123">Poznamenejte si hello úplný identifikátor URI, jako je tomu v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="75172-123">Make note of hello full URI as it is used in later steps.</span></span>

1. <span data-ttu-id="75172-124">Nahrajte soubor virtuálního pevného disku hello pomocí AzCopy:</span><span class="sxs-lookup"><span data-stu-id="75172-124">Upload hello VHD file using AzCopy:</span></span>
 
1. <span data-ttu-id="75172-125">[Stáhněte a nainstalujte nejnovější verzi AzCopy hello](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="75172-125">[Download and install hello latest version of AzCopy](http://aka.ms/downloadazcopy).</span></span>

1. <span data-ttu-id="75172-126">Otevřete okno příkazového řádku a přejděte instalační adresář AzCopy toohello.</span><span class="sxs-lookup"><span data-stu-id="75172-126">Open a command window and navigate toohello AzCopy installation directory.</span></span> <span data-ttu-id="75172-127">Volitelně můžete přidat hello AzCopy tooyour systému cesta k umístění instalace.</span><span class="sxs-lookup"><span data-stu-id="75172-127">Optionally, you can add hello AzCopy installation location tooyour system path.</span></span> <span data-ttu-id="75172-128">AzCopy je ve výchozím nastavení nainstalované toohello následující adresář:</span><span class="sxs-lookup"><span data-stu-id="75172-128">By default, AzCopy is installed toohello following directory:</span></span>

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. <span data-ttu-id="75172-129">Pomocí hello účet klíč a objektů blob kontejneru úložiště identifikátor URI, spusťte následující příkaz na příkazovém řádku hello hello.</span><span class="sxs-lookup"><span data-stu-id="75172-129">Using hello storage account key and blob container URI, run hello following command at hello command prompt.</span></span> <span data-ttu-id="75172-130">Hello *vhdFileName* hodnota musí toobe v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="75172-130">hello *vhdFileName* value needs toobe in quotes.</span></span> <span data-ttu-id="75172-131">Hello proces odesílání soubor virtuálního pevného disku může být náročná v závislosti na velikosti hello hello souboru virtuálního pevného disku a rychlost připojení.</span><span class="sxs-lookup"><span data-stu-id="75172-131">hello process of uploading a VHD file can be lengthy depending on hello size of hello VHD file and your connection speed.</span></span>   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a><span data-ttu-id="75172-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75172-132">Next steps</span></span>

- [<span data-ttu-id="75172-133">Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="75172-133">Create a custom image in Azure DevTest Labs from a VHD file using hello Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="75172-134">Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="75172-134">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)