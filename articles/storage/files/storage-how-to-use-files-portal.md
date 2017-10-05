---
title: "Správa služby Azure File Storage z webu Azure Portal | Dokumentace Microsoftu"
description: "Zjistěte, jak pomocí webu Azure Portal spravovat službu Azure File Storage."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: d5ffa7cff0a31e36f5a96aaa4b2d477fa39333fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-file-storage-from-the-azure-portal"></a><span data-ttu-id="2fc6f-103">Správa služby Azure File Storage z webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2fc6f-103">How to use Azure File storage from the Azure Portal</span></span>
<span data-ttu-id="2fc6f-104">[Azure Portal](https://portal.azure.com) nabízí uživatelské rozhraní pro správu služby Azure File Storage.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-104">The [Azure portal](https://portal.azure.com) provides a user interface for managing Azure File storage.</span></span> <span data-ttu-id="2fc6f-105">Z webového prohlížeče můžete provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="2fc6f-105">You can perform the following actions from your web browser:</span></span>

* <span data-ttu-id="2fc6f-106">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="2fc6f-106">Create a File Share</span></span>
* <span data-ttu-id="2fc6f-107">Ukládat soubory do sdílené složky a stahovat soubory ze sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-107">Upload and download files to and from your file share.</span></span>
* <span data-ttu-id="2fc6f-108">Monitorovat skutečné využití každé sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-108">Monitor the actual usage of each file share.</span></span>
* <span data-ttu-id="2fc6f-109">Upravit kvótu velikosti sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-109">Adjust the file share size quota.</span></span>
* <span data-ttu-id="2fc6f-110">Kopírovat příkazy pro připojení a použít je k připojení sdílené složky z klienta pro Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-110">Copy the mount commands to use to mount your file share from a Windows or Linux client.</span></span>

## <a name="create-file-share"></a><span data-ttu-id="2fc6f-111">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="2fc6f-111">Create file share</span></span>
1. <span data-ttu-id="2fc6f-112">Přihlaste se k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-112">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="2fc6f-113">V navigační nabídce klikněte na **Účty úložiště** nebo **Účty úložiště (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-113">On the navigation menu, click **Storage accounts** or **Storage accounts (classic)**.</span></span>
    
    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. <span data-ttu-id="2fc6f-115">Vyberte svůj účet úložiště</span><span class="sxs-lookup"><span data-stu-id="2fc6f-115">Choose your storage account.</span></span>

    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. <span data-ttu-id="2fc6f-117">Vyberte službu „Files“.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-117">Choose "Files" service.</span></span>

    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. <span data-ttu-id="2fc6f-119">Klikněte na „Sdílené složky“ a pomocí odkazu vytvořte svou první sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-119">Click "File shares" and follow the link to create your first file share.</span></span>

    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. <span data-ttu-id="2fc6f-121">Zadejte název sdílené složky a její velikost (max. 5120 GB) a vaše první složka se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-121">Fill in the file share name and the size of the file share (up to 5120 GB) to create your first file share.</span></span> <span data-ttu-id="2fc6f-122">Po vytvoření sdílené složky ji můžete připojit z jakéhokoli souborové systému, který podporuje SMB 2.1 nebo SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-122">Once the file share has been created, you can mount it from any file system that supports SMB 2.1 or SMB 3.0.</span></span> <span data-ttu-id="2fc6f-123">Po kliknutí na **Kvóta** sdílené složky můžete změnit velikost souboru až do velikosti 5 120 GB.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-123">You could click on **Quota** on the file share to change the size of the file up to 5120 GB.</span></span> <span data-ttu-id="2fc6f-124">Pro odhad nákladů na úložiště při použití služby Azure File Storage použijte [cenovou kalkulačku funkcí Azure](https://azure.microsoft.com/pricing/calculator/).</span><span class="sxs-lookup"><span data-stu-id="2fc6f-124">Please refer to [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) to estimate the storage cost of using Azure File storage.</span></span>

    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a><span data-ttu-id="2fc6f-126">Ukládání a stahování souborů</span><span class="sxs-lookup"><span data-stu-id="2fc6f-126">Upload and download files</span></span>
1. <span data-ttu-id="2fc6f-127">Vyberte jednu sdílenou složku, kterou jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-127">Choose one file share your have already created.</span></span>

    ![Snímek obrazovky, který ukazuje ukládání a stahování souborů z portálu.](./media/storage-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. <span data-ttu-id="2fc6f-129">Klikněte na **Uložit** a otevře se uživatelské prostředí pro ukládání souborů.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-129">Click **Upload** to open the user interface for files uploading.</span></span>

    ![Snímek obrazovky, který ukazuje ukládání souborů z portálu.](./media/storage-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-to-file-share"></a><span data-ttu-id="2fc6f-131">Připojení ke sdílené složce</span><span class="sxs-lookup"><span data-stu-id="2fc6f-131">Connect to file share</span></span>
-  <span data-ttu-id="2fc6f-132">Klikněte na **Připojit** a zobrazí se příkazový řádek pro připojení sdílené složky z Windows a Linuxu.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-132">Click **Connect** to get the command line for mounting the file share from Windows and Linux.</span></span> <span data-ttu-id="2fc6f-133">Pokud jste uživatelem Linuxu, další informace o připojení pro jiné distribuce Linuxu najdete v tématu [Používání služby Azure File Storage s Linuxem](../storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2fc6f-133">For Linux users, you can also refer to [How to use Azure File storage with Linux](../storage-how-to-use-files-linux.md) for more mounting instructions for other Linux distributions.</span></span>

    ![Snímek obrazovky, který ukazuje připojení sdílené složky.](./media/storage-how-to-use-files-portal/use-files-portal-connect.png)
-  <span data-ttu-id="2fc6f-135">Můžete zkopírovat příkazy pro připojení sdílené složky v systému Windows nebo Linux a spustit je z virtuálního počítače Azure nebo místního počítače.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-135">You can copy the commands for mounting file share on Windows or Linux and run it from you Azure VM or on-premises machine.</span></span>

    ![Snímek obrazovky, který ukazuje příkazy pro připojení pro Windows a Linux](./media/storage-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

<span data-ttu-id="2fc6f-137">**Tip:**</span><span class="sxs-lookup"><span data-stu-id="2fc6f-137">**Tip:**</span></span>  
<span data-ttu-id="2fc6f-138">Přístupový klíč účtu úložiště pro připojení najdete tak, že kliknete na **Zobrazit přístupové klíče pro tento účet úložiště** ve spodní části stránky připojení.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-138">To find the storage account access key for mounting, click on **View access keys for this storage account** at the bottom of the connect page.</span></span>

## <a name="see-also"></a><span data-ttu-id="2fc6f-139">Viz také</span><span class="sxs-lookup"><span data-stu-id="2fc6f-139">See also</span></span>
<span data-ttu-id="2fc6f-140">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="2fc6f-140">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="2fc6f-141">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="2fc6f-141">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="2fc6f-142">Řešení potíží ve Windows</span><span class="sxs-lookup"><span data-stu-id="2fc6f-142">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="2fc6f-143">Řešení potíží v Linuxu</span><span class="sxs-lookup"><span data-stu-id="2fc6f-143">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)    