---
title: "aaaHow toomanage Azure File storage z hello portálu Azure | Microsoft Docs"
description: "Přečtěte si toouse hello Azure toomanage portálu Azure File storage."
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
ms.openlocfilehash: 6ad2fbbf9ee2f86748b1b175d0f63274144dc5eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-file-storage-from-hello-azure-portal"></a><span data-ttu-id="e9595-103">Jak hello toouse Azure File storage z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e9595-103">How toouse Azure File storage from hello Azure Portal</span></span>
<span data-ttu-id="e9595-104">Hello [portál Azure](https://portal.azure.com) poskytuje uživatelské rozhraní pro správu Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="e9595-104">hello [Azure portal](https://portal.azure.com) provides a user interface for managing Azure File storage.</span></span> <span data-ttu-id="e9595-105">Můžete provádět následující akce z webového prohlížeče hello:</span><span class="sxs-lookup"><span data-stu-id="e9595-105">You can perform hello following actions from your web browser:</span></span>

* <span data-ttu-id="e9595-106">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="e9595-106">Create a File Share</span></span>
* <span data-ttu-id="e9595-107">Nahrávání a stahování souborů tooand ze sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e9595-107">Upload and download files tooand from your file share.</span></span>
* <span data-ttu-id="e9595-108">Sledování hello skutečné využití každé sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e9595-108">Monitor hello actual usage of each file share.</span></span>
* <span data-ttu-id="e9595-109">Upravte kvótu velikosti sdílené složky souboru hello.</span><span class="sxs-lookup"><span data-stu-id="e9595-109">Adjust hello file share size quota.</span></span>
* <span data-ttu-id="e9595-110">Zkopírujte hello připojení příkazy toouse toomount sdílené složky souboru z klienta Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="e9595-110">Copy hello mount commands toouse toomount your file share from a Windows or Linux client.</span></span>

## <a name="create-file-share"></a><span data-ttu-id="e9595-111">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="e9595-111">Create file share</span></span>
1. <span data-ttu-id="e9595-112">Přihlaste se toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e9595-112">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="e9595-113">V navigační nabídce hello, klikněte na tlačítko **účty úložiště** nebo **účty úložiště (klasické)**.</span><span class="sxs-lookup"><span data-stu-id="e9595-113">On hello navigation menu, click **Storage accounts** or **Storage accounts (classic)**.</span></span>
    
    ![Snímek obrazovky, který ukazuje, jak toocreate sdílená hello portálu](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. <span data-ttu-id="e9595-115">Vyberte svůj účet úložiště</span><span class="sxs-lookup"><span data-stu-id="e9595-115">Choose your storage account.</span></span>

    ![Snímek obrazovky, který ukazuje, jak toocreate sdílená hello portálu](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. <span data-ttu-id="e9595-117">Vyberte službu „Files“.</span><span class="sxs-lookup"><span data-stu-id="e9595-117">Choose "Files" service.</span></span>

    ![Snímek obrazovky, který ukazuje, jak toocreate sdílená hello portálu](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. <span data-ttu-id="e9595-119">Klikněte na tlačítko "Sdílené složky" a postupujte podle toocreate odkaz hello první soubor sdílet.</span><span class="sxs-lookup"><span data-stu-id="e9595-119">Click "File shares" and follow hello link toocreate your first file share.</span></span>

    ![Snímek obrazovky, který ukazuje, jak toocreate sdílená hello portálu](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. <span data-ttu-id="e9595-121">Zadejte název sdílené složky souborů hello a hello velikost hello soubor sdílené složky (až too5120 GB) toocreate svou první sdílenou.</span><span class="sxs-lookup"><span data-stu-id="e9595-121">Fill in hello file share name and hello size of hello file share (up too5120 GB) toocreate your first file share.</span></span> <span data-ttu-id="e9595-122">Po vytvoření hello sdílené složky ji můžete připojit z jakéhokoli souborové systému, který podporuje SMB 2.1 nebo SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="e9595-122">Once hello file share has been created, you can mount it from any file system that supports SMB 2.1 or SMB 3.0.</span></span> <span data-ttu-id="e9595-123">Uživatel může klepnout na **kvóty** na hello velikost sdílené složky toochange hello hello souboru až too5120 GB.</span><span class="sxs-lookup"><span data-stu-id="e9595-123">You could click on **Quota** on hello file share toochange hello size of hello file up too5120 GB.</span></span> <span data-ttu-id="e9595-124">Naleznete příliš[cenové kalkulačce pro Azure](https://azure.microsoft.com/pricing/calculator/) tooestimate hello náklady na úložiště používání Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="e9595-124">Please refer too[Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) tooestimate hello storage cost of using Azure File storage.</span></span>

    ![Snímek obrazovky, který ukazuje, jak toocreate sdílená hello portálu](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a><span data-ttu-id="e9595-126">Ukládání a stahování souborů</span><span class="sxs-lookup"><span data-stu-id="e9595-126">Upload and download files</span></span>
1. <span data-ttu-id="e9595-127">Vyberte jednu sdílenou složku, kterou jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e9595-127">Choose one file share your have already created.</span></span>

    ![Snímek obrazovky, který ukazuje, jak hello tooupload a stahování souborů z portálu](./media/storage-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. <span data-ttu-id="e9595-129">Klikněte na tlačítko **nahrát** otevřete hello uživatelské rozhraní pro ukládání souborů.</span><span class="sxs-lookup"><span data-stu-id="e9595-129">Click **Upload** to open hello user interface for files uploading.</span></span>

    ![Snímek obrazovky, který ukazuje, jak tooupload souborů z portálu hello](./media/storage-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-toofile-share"></a><span data-ttu-id="e9595-131">Připojení sdílené složky toofile</span><span class="sxs-lookup"><span data-stu-id="e9595-131">Connect toofile share</span></span>
-  <span data-ttu-id="e9595-132">Klikněte na tlačítko **Connect** získat hello příkazového řádku pro připojování hello sdílené složky z Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="e9595-132">Click **Connect** to get hello command line for mounting hello file share from Windows and Linux.</span></span> <span data-ttu-id="e9595-133">Pro uživatele, Linux, můžete se také podívat příliš[jak toouse Azure File storage s Linuxem](../storage-how-to-use-files-linux.md) další připojení pokyny pro ostatní distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="e9595-133">For Linux users, you can also refer too[How toouse Azure File storage with Linux](../storage-how-to-use-files-linux.md) for more mounting instructions for other Linux distributions.</span></span>

    ![Snímek obrazovky, který ukazuje, jak toomount hello sdílené složky](./media/storage-how-to-use-files-portal/use-files-portal-connect.png)
-  <span data-ttu-id="e9595-135">Můžete zkopírovat hello příkazů pro připojení souboru sdílenou složku v systému Windows nebo Linux a spusťte jej z jste virtuální počítač Azure nebo na místní počítač.</span><span class="sxs-lookup"><span data-stu-id="e9595-135">You can copy hello commands for mounting file share on Windows or Linux and run it from you Azure VM or on-premises machine.</span></span>

    ![Snímek obrazovky, který zobrazuje hello připojení příkazy pro systém Windows a Linux](./media/storage-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

<span data-ttu-id="e9595-137">**Tip:**</span><span class="sxs-lookup"><span data-stu-id="e9595-137">**Tip:**</span></span>  
<span data-ttu-id="e9595-138">toofind přístupový klíč účtu úložiště hello pro připojení, klikněte na **zobrazení přístupových klíčů pro tento účet úložiště** dole hello hello připojit stránky.</span><span class="sxs-lookup"><span data-stu-id="e9595-138">toofind hello storage account access key for mounting, click on **View access keys for this storage account** at hello bottom of hello connect page.</span></span>

## <a name="see-also"></a><span data-ttu-id="e9595-139">Viz také</span><span class="sxs-lookup"><span data-stu-id="e9595-139">See also</span></span>
<span data-ttu-id="e9595-140">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="e9595-140">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="e9595-141">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="e9595-141">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="e9595-142">Řešení potíží ve Windows</span><span class="sxs-lookup"><span data-stu-id="e9595-142">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="e9595-143">Řešení potíží v Linuxu</span><span class="sxs-lookup"><span data-stu-id="e9595-143">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)    