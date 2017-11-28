---
title: "aaaUpload virtuálního pevného disku soubor pomocí Microsoft Azure Storage Explorer tooAzure DevTest Labs | Microsoft Docs"
description: "Nahrání virtuálního pevného disku soubor toolab účtu úložiště pomocí Microsoft Azure Storage Explorer"
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
ms.openlocfilehash: 686691e3676cea4b2d7cd8bf045bc43a792c667e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-microsoft-azure-storage-explorer"></a><span data-ttu-id="f7cd1-103">Nahrání virtuálního pevného disku soubor toolab účtu úložiště pomocí Microsoft Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="f7cd1-103">Upload VHD file toolab's storage account using Microsoft Azure Storage Explorer</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="f7cd1-104">V Azure DevTest Labs může být soubory VHD použité toocreate vlastní Image, které jsou používané tooprovision virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-104">In Azure DevTest Labs, VHD files can be used toocreate custom images, which are used tooprovision virtual machines.</span></span> <span data-ttu-id="f7cd1-105">Tento článek ukazuje, jak toouse [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) účet úložiště tooa testovacím souborů tooupload virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-105">This article illustrates how toouse [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooupload a VHD file tooa lab's storage account.</span></span> <span data-ttu-id="f7cd1-106">Jakmile jste odeslali souboru virtuálního pevného disku, hello [další kroky části](#next-steps) jsou uvedeny některé články, které ilustrují, jak toocreate vlastní image z hello nahrát soubor virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-106">Once you've uploaded your VHD file, hello [Next steps section](#next-steps) lists some articles that illustrate how toocreate a custom image from hello uploaded VHD file.</span></span> <span data-ttu-id="f7cd1-107">Další informace o disky a virtuální pevné disky v Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="f7cd1-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="f7cd1-108">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="f7cd1-108">Step-by-step instructions</span></span>

<span data-ttu-id="f7cd1-109">Hello následující kroky procházení vás odesílání virtuálního pevného disku soubor tooDevTest Labs pomocí [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="f7cd1-109">hello following steps walk you through uploading a VHD file tooDevTest Labs using [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>

1. <span data-ttu-id="f7cd1-110">[Stáhněte a nainstalujte nejnovější verzi hello Microsoft Azure Storage Explorer hello](http://www.storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="f7cd1-110">[Download and install hello latest version of hello Microsoft Azure Storage Explorer](http://www.storageexplorer.com).</span></span>

1. <span data-ttu-id="f7cd1-111">Získáte hello název účtu úložiště hello prostředí pomocí hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="f7cd1-111">Get hello name of hello lab's storage account using hello Azure portal:</span></span>

    1. <span data-ttu-id="f7cd1-112">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="f7cd1-112">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
    
    1. <span data-ttu-id="f7cd1-113">Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-113">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>
    
    1. <span data-ttu-id="f7cd1-114">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-114">From hello list of labs, select hello desired lab.</span></span>  
    
    1. <span data-ttu-id="f7cd1-115">V okně prostředí hello vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-115">On hello lab's blade, select **Configuration**.</span></span> 
    
    1. <span data-ttu-id="f7cd1-116">V testovacím hello **konfigurace** vyberte **vlastní Image (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-116">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>
    
    1. <span data-ttu-id="f7cd1-117">Na hello **vlastní image** okně vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-117">On hello **Custom images** blade, Select **+Add**.</span></span> 
    
    1. <span data-ttu-id="f7cd1-118">Na hello **vlastní image** vyberte **virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-118">On hello **Custom image** blade, select **VHD**.</span></span>
    
    1. <span data-ttu-id="f7cd1-119">Na hello **virtuálního pevného disku** vyberte **nahrát virtuální pevný disk pomocí prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-119">On hello **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>
    
        ![Nahrání virtuálního pevného disku pomocí prostředí PowerShell][0]
    
    1. <span data-ttu-id="f7cd1-121">Hello **Odeslat bitovou kopii pomocí prostředí PowerShell** zobrazuje volání toohello **přidat AzureVhd** rutiny.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-121">hello **Upload an image using PowerShell** blade displays a call toohello **Add-AzureVhd** cmdlet.</span></span> <span data-ttu-id="f7cd1-122">první parametr Hello (*cílové*) obsahuje název účtu úložiště hello pro testovací prostředí hello v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="f7cd1-122">hello first parameter (*Destination*) contains hello storage account name for hello lab in hello following format:</span></span>
    
        <span data-ttu-id="f7cd1-123">https://<STORAGE-ACCOUNT-name>.BLOB.Core.Windows.NET/uploads/...</span><span class="sxs-lookup"><span data-stu-id="f7cd1-123">https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...</span></span> 

    1. <span data-ttu-id="f7cd1-124">Poznamenejte si název účtu úložiště hello jako se používá v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-124">Make note of hello storage account name as it is used in later steps.</span></span>
    
1. <span data-ttu-id="f7cd1-125">Připojte tooan účtu předplatného Azure pomocí Průzkumníka úložiště.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-125">Connect tooan Azure subscription account using Storage Explorer.</span></span>

    > [!TIP] 
    > 
    > <span data-ttu-id="f7cd1-126">Storage Explorer podporuje několik možností připojení.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-126">Storage Explorer supports several connection options.</span></span> <span data-ttu-id="f7cd1-127">Tato část ukazuje připojující účet úložiště tooa spojené s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-127">This section illustrates connecting tooa storage account associated with your Azure subscription.</span></span> <span data-ttu-id="f7cd1-128">toosee hello další možnosti připojení nepodporuje Storage Explorer, najdete v článku toohello [Začínáme se Storage Explorerem](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="f7cd1-128">toosee hello other connection options supported by Storage Explorer, refer toohello article, [Getting started with Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>
 
    1. <span data-ttu-id="f7cd1-129">Otevřete Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-129">Open Storage Explorer.</span></span>
    
    1. <span data-ttu-id="f7cd1-130">V Průzkumníku úložiště vyberte **nastavení účtu Azure**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-130">In Storage Explorer, select **Azure Account settings**.</span></span> 
    
        ![Nastavení účtu Azure][1]
    
    1. <span data-ttu-id="f7cd1-132">Hello levý panel zobrazuje účty Microsoft hello, pod kterým jste přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-132">hello left pane displays hello Microsoft accounts you've logged in to.</span></span> <span data-ttu-id="f7cd1-133">tooconnect tooanother účtu, vyberte možnost **přidat účet**a postupujte podle pokynů hello dialogová okna toosign pomocí účtu Microsoft, který je přidružen alespoň jeden aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-133">tooconnect tooanother account, select **Add an account**, and follow hello dialogs toosign in with a Microsoft account that is associated with at least one active Azure subscription.</span></span>
    
        ![Přidání účtu][2]
    
    1. <span data-ttu-id="f7cd1-135">Jakmile se úspěšně přihlásíte účtem Microsoft, levém podokně hello naplní s hello předplatná Azure, které jsou přidružené k tomuto účtu.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-135">Once you successfully sign in with a Microsoft account, hello left pane populates with hello Azure subscriptions associated with that account.</span></span> <span data-ttu-id="f7cd1-136">Vyberte hello předplatná Azure, se kterými chcete toowork a potom vyberte **použít**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-136">Select hello Azure subscriptions with which you want toowork, and then select **Apply**.</span></span> <span data-ttu-id="f7cd1-137">(Výběr **všechny odběry** přepínačů hello výběr všech nebo žádná z hello uvedených předplatných Azure.)</span><span class="sxs-lookup"><span data-stu-id="f7cd1-137">(Selecting **All subscriptions** toggles hello selection of all or none of hello listed Azure subscriptions.)</span></span>
    
        ![Výběr předplatných Azure][3]
    
    1. <span data-ttu-id="f7cd1-139">Hello levém podokně zobrazí hello účty úložiště přidružené ke hello vybraná předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-139">hello left pane displays hello storage accounts associated with hello selected Azure subscriptions.</span></span>
    
        ![Vybraná předplatná Azure][4]

1. <span data-ttu-id="f7cd1-141">Vyhledejte účet úložiště hello laboratoře:</span><span class="sxs-lookup"><span data-stu-id="f7cd1-141">Locate hello lab's storage account:</span></span>

    1. <span data-ttu-id="f7cd1-142">V levém podokně Storage Exploreru hello vyhledejte a rozbalte uzel hello pro hello předplatného Azure, které vlastní hello testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-142">In hello Storage Explorer left pane, locate, and expand hello node for hello Azure subscription that owns hello lab.</span></span>
    
    1. <span data-ttu-id="f7cd1-143">V uzlu hello předplatné, rozbalte položku **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-143">Under hello subscription's node, expand **Storage Accounts**.</span></span>

    1. <span data-ttu-id="f7cd1-144">Rozbalte uzly protokolů tooreveal uzlu účet úložiště hello laboratoř pro **kontejnery objektů Blob**, **sdílené složky**, **fronty**, a **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-144">Expand hello lab's storage account node tooreveal nodes for **Blob Containers**, **File Shares**, **Queues**, and **Tables**.</span></span>
    
    1. <span data-ttu-id="f7cd1-145">Rozbalte hello **kontejnery objektů Blob** uzlu.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-145">Expand hello **Blob Containers** node.</span></span>
    
    1. <span data-ttu-id="f7cd1-146">Vyberte toodisplay kontejner objektů blob nahrávání hello jeho obsah v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-146">Select hello uploads blob container toodisplay its contents in hello right pane.</span></span>
        
        ![Nahrát adresáře][5]

1. <span data-ttu-id="f7cd1-148">Nahrajte soubor virtuálního pevného disku hello pomocí Průzkumníka úložiště:</span><span class="sxs-lookup"><span data-stu-id="f7cd1-148">Upload hello VHD file using Storage Explorer:</span></span>

    1. <span data-ttu-id="f7cd1-149">V pravém podokně Storage Exploreru hello byste měli vidět v seznamu objektů BLOB hello v hello **odešle** kontejneru objektů blob s testovacím hello účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-149">In hello Storage Explorer right pane, you should see a listing of hello blobs in hello **uploads** blob container of hello lab's storage account.</span></span> <span data-ttu-id="f7cd1-150">Na panelu nástrojů editoru objektů blob hello, vyberte **nahrát**</span><span class="sxs-lookup"><span data-stu-id="f7cd1-150">On hello blob editor toolbar, select **Upload**</span></span> 
        
        ![Tlačítko Odeslat][6]
    
    1. <span data-ttu-id="f7cd1-152">Z hello **nahrát** rozevírací nabídky vyberte **nahrání souborů...** .</span><span class="sxs-lookup"><span data-stu-id="f7cd1-152">From hello **Upload** drop-down menu, select **Upload files...**.</span></span>
    
    1. <span data-ttu-id="f7cd1-153">Na hello **nahrání souborů** dialogové okno, vyberte hello třemi tečkami.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-153">On hello **Upload files** dialog, select hello ellipsis.</span></span>
        
        ![Vyberte soubor][8]  

    1. <span data-ttu-id="f7cd1-155">Na hello **vyberte soubory tooupload** dialogu procházení toohello požadovaného souboru virtuálního pevného disku, vyberte ho a potom vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-155">On hello **Select files tooupload** dialog, browse toohello desired VHD file, select it, and then select **Open**.</span></span>
    
    1. <span data-ttu-id="f7cd1-156">Při vrátil toohello **nahrání souborů** dialogové okno, změna **Blob typ** příliš**objekt Blob stránky**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-156">When returned toohello **Upload files** dialog, change **Blob type** too**Page Blob**.</span></span>
    
    1. <span data-ttu-id="f7cd1-157">Vyberte **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-157">Select **Upload**.</span></span>

        ![Vyberte soubor][9]  
    
    1. <span data-ttu-id="f7cd1-159">Hello Storage Explorer **protokol aktivit** podokně se zobrazují stav stahování hello (spolu s odkazy toocancel hello nahrávání).</span><span class="sxs-lookup"><span data-stu-id="f7cd1-159">hello Storage Explorer **Activity Log** pane shows hello download status (along with links toocancel hello upload).</span></span> <span data-ttu-id="f7cd1-160">Hello proces odesílání soubor virtuálního pevného disku může být náročná v závislosti na velikosti hello hello souboru virtuálního pevného disku a rychlost připojení.</span><span class="sxs-lookup"><span data-stu-id="f7cd1-160">hello process of uploading a VHD file can be lengthy depending on hello size of hello VHD file and your connection speed.</span></span> 

        ![Stav nahrávání souboru][10]  

## <a name="next-steps"></a><span data-ttu-id="f7cd1-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7cd1-162">Next steps</span></span>

- [<span data-ttu-id="f7cd1-163">Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f7cd1-163">Create a custom image in Azure DevTest Labs from a VHD file using hello Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="f7cd1-164">Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7cd1-164">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

[0]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-image-using-psh.png
[1]: ./media/devtest-lab-upload-vhd-using-storage-explorer/settings-icon.png
[2]: ./media/devtest-lab-upload-vhd-using-storage-explorer/add-account-link.png
[3]: ./media/devtest-lab-upload-vhd-using-storage-explorer/subscriptions-list.png
[4]: ./media/devtest-lab-upload-vhd-using-storage-explorer/storage-accounts-list.png
[5]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-dir.png
[6]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-button.png
[7]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-files.png
[8]: ./media/devtest-lab-upload-vhd-using-storage-explorer/select-file.png
[9]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-file.png
[10]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-status.png
