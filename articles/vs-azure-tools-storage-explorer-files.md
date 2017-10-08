---
title: aaaUsing Storage Explorer (Preview) s Azure File storage | Microsoft Docs
description: "Zjistěte, jak zjistěte, jak toouse toowork Storage Explorer (Preview) se souborem sdílené složky a soubory."
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/09/2017
ms.author: cawa
ms.openlocfilehash: 98eb3cde711ae3dbfdb6ffaec23ae24f822370e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-storage-explorer-preview-with-azure-file-storage"></a><span data-ttu-id="3cad2-103">Použití Storage Exploreru (Preview) se službou Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="3cad2-103">Using Storage Explorer (Preview) with Azure File storage</span></span>

<span data-ttu-id="3cad2-104">Azure File storage je služba, která nabízí sdílené složky v cloudu pomocí hello hello standardní protokol Server Message Block (SMB).</span><span class="sxs-lookup"><span data-stu-id="3cad2-104">Azure File storage is a service that offers file shares in hello cloud using hello standard Server Message Block (SMB) Protocol.</span></span> <span data-ttu-id="3cad2-105">Podporují se SMB 2.1 i SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="3cad2-105">Both SMB 2.1 and SMB 3.0 are supported.</span></span> <span data-ttu-id="3cad2-106">S Azure File storage můžete migrovat starší aplikace, které spoléhají na sdílené složky tooAzure souboru rychle a bez nákladných přepisů.</span><span class="sxs-lookup"><span data-stu-id="3cad2-106">With Azure File storage, you can migrate legacy applications that rely on file shares tooAzure quickly and without costly rewrites.</span></span> <span data-ttu-id="3cad2-107">Můžete použít soubor úložiště tooexpose data veřejně toohello svět nebo data aplikací toostore soukromě.</span><span class="sxs-lookup"><span data-stu-id="3cad2-107">You can use File storage tooexpose data publicly toohello world, or toostore application data privately.</span></span> <span data-ttu-id="3cad2-108">V tomto článku se dozvíte, jak toouse toowork Storage Explorer (Preview) se souborem sdílené složky a soubory.</span><span class="sxs-lookup"><span data-stu-id="3cad2-108">In this article, you'll learn how toouse Storage Explorer (Preview) toowork with file shares and files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3cad2-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3cad2-109">Prerequisites</span></span>

<span data-ttu-id="3cad2-110">toocomplete hello kroky v tomto článku, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="3cad2-110">toocomplete hello steps in this article, you'll need hello following:</span></span>

- [<span data-ttu-id="3cad2-111">Stažení a instalace Storage Exploreru (Preview)</span><span class="sxs-lookup"><span data-stu-id="3cad2-111">Download and install Storage Explorer (preview)</span></span>](http://www.storageexplorer.com/)

- [<span data-ttu-id="3cad2-112">Připojení účtu úložiště Azure tooa nebo služby</span><span class="sxs-lookup"><span data-stu-id="3cad2-112">Connect tooa Azure storage account or service</span></span>](https://docs.microsoft.com//azure/vs-azure-tools-storage-manage-with-storage-explorer#connect-to-a-storage-account-or-service)

## <a name="create-a-file-share"></a><span data-ttu-id="3cad2-113">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="3cad2-113">Create a File Share</span></span>

<span data-ttu-id="3cad2-114">Všechny soubory se musí nacházet ve sdílené složce, což je jednoduše logické seskupení souborů.</span><span class="sxs-lookup"><span data-stu-id="3cad2-114">All files must reside in a file share, which is simply a logical grouping of files.</span></span> <span data-ttu-id="3cad2-115">Účet může obsahovat neomezený počet sdílených složek a v každé sdílené složce může být uložen neomezený počet souborů.</span><span class="sxs-lookup"><span data-stu-id="3cad2-115">An account can contain an unlimited number of file shares, and each share can store an unlimited number of files.</span></span>

<span data-ttu-id="3cad2-116">Hello následující kroky ukazují, jak toocreate soubor sdílet v rámci Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="3cad2-116">hello following steps illustrate how toocreate a file share within Storage Explorer (Preview).</span></span>

1. <span data-ttu-id="3cad2-117">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="3cad2-117">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="3cad2-118">V levém podokně hello rozbalte účet úložiště hello v rámci kterého chcete toocreate hello sdílené složky</span><span class="sxs-lookup"><span data-stu-id="3cad2-118">In hello left pane, expand hello storage account within which you wish toocreate hello File Share</span></span>

3. <span data-ttu-id="3cad2-119">Klikněte pravým tlačítkem na **sdílené složky**a - hello kontextové nabídce vyberte **vytvoření sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-119">Right-click **File Shares**, and - from hello context menu - select **Create File Share**.</span></span>

    ![Vytvoření sdílené složky](media/vs-azure-tools-storage-explorer-files/image1.png)

4. <span data-ttu-id="3cad2-121">V textovém poli se zobrazí pod hello **sdílené složky** složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-121">A text box will appear below hello **File Shares** folder.</span></span> <span data-ttu-id="3cad2-122">Zadejte název sdílené složky hello.</span><span class="sxs-lookup"><span data-stu-id="3cad2-122">Enter hello name for your file share.</span></span> <span data-ttu-id="3cad2-123">V tématu hello [sdílet pravidla pojmenování](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) části Seznam pravidel a omezení pro pojmenovávání sdílených složek.</span><span class="sxs-lookup"><span data-stu-id="3cad2-123">See hello [Share naming rules](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) section for a list of rules and restrictions on naming file shares.</span></span>

    ![Pojmenování hello sdílené složky](media/vs-azure-tools-storage-explorer-files/image2.png)

5. <span data-ttu-id="3cad2-125">Stiskněte klávesu **Enter** při done toocreate hello sdílené složky, nebo **Esc** toocancel.</span><span class="sxs-lookup"><span data-stu-id="3cad2-125">Press **Enter** when done toocreate hello file share, or **Esc** toocancel.</span></span> <span data-ttu-id="3cad2-126">Po úspěšném vytvoření hello sdílené složky se zobrazí v části hello **sdílené složky** složku pro hello vybraný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3cad2-126">Once hello file share has been successfully created, it will be displayed under hello **File Shares** folder for hello selected storage account.</span></span>

    ![Nová sdílená složka Hello](media/vs-azure-tools-storage-explorer-files/image3.png)

## <a name="view-a-file-shares-contents"></a><span data-ttu-id="3cad2-128">Zobrazení obsahu sdílené složky</span><span class="sxs-lookup"><span data-stu-id="3cad2-128">View a file share's contents</span></span>

<span data-ttu-id="3cad2-129">Sdílené složky obsahují soubory a složky (ty také můžou obsahovat soubory).</span><span class="sxs-lookup"><span data-stu-id="3cad2-129">File shares contain files and folders (that can also contain files).</span></span>

<span data-ttu-id="3cad2-130">Hello následující kroky popisují jak sdílet obsah hello tooview souboru v rámci Storage Explorer (Preview): +</span><span class="sxs-lookup"><span data-stu-id="3cad2-130">hello following steps illustrate how tooview hello contents of a file share within Storage Explorer (Preview):+</span></span>

1. <span data-ttu-id="3cad2-131">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="3cad2-131">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="3cad2-132">V levém podokně hello rozbalte účet úložiště hello obsahující chcete tooview hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-132">In hello left pane, expand hello storage account containing hello file share you wish tooview.</span></span>

3. <span data-ttu-id="3cad2-133">Rozbalte účet úložiště hello **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-133">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="3cad2-134">Klikněte pravým tlačítkem na hello sdílené složky můžete chcete tooview a - hello kontextové nabídce vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-134">Right-click hello file share you wish tooview, and - from hello context menu - select **Open**.</span></span> <span data-ttu-id="3cad2-135">Dvakrát klikněte na sdílenou složku hello chcete tooview.</span><span class="sxs-lookup"><span data-stu-id="3cad2-135">You can also double-click hello file share you wish tooview.</span></span>

    ![Otevření sdílené složky](media/vs-azure-tools-storage-explorer-files/image4.png)

5. <span data-ttu-id="3cad2-137">Hello hlavním podokně se zobrazí obsah hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-137">hello main pane will display hello file share's contents.</span></span>
    
    ![Hello obsah sdílené složky](media/vs-azure-tools-storage-explorer-files/image5.png)

## <a name="delete-a-file-share"></a><span data-ttu-id="3cad2-139">Odstranění sdílené složky</span><span class="sxs-lookup"><span data-stu-id="3cad2-139">Delete a file share</span></span>

<span data-ttu-id="3cad2-140">Sdílené složky můžete podle potřeby snadno vytvářet a odstraňovat.</span><span class="sxs-lookup"><span data-stu-id="3cad2-140">File shares can be easily created and deleted as needed.</span></span> <span data-ttu-id="3cad2-141">(toosee jak toodelete jednotlivé soubory, naleznete v části toohello [správu souborů ve sdílené složce](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="3cad2-141">(toosee how toodelete individual files, refer toohello section, [Managing files in a file share](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="3cad2-142">Hello následující kroky popisují, jak toodelete soubor sdílet v rámci Storage Explorer (Preview):</span><span class="sxs-lookup"><span data-stu-id="3cad2-142">hello following steps illustrate how toodelete a file share within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="3cad2-143">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="3cad2-143">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="3cad2-144">V levém podokně hello rozbalte účet úložiště hello obsahující chcete tooview hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-144">In hello left pane, expand hello storage account containing hello file share you wish tooview.</span></span>

3. <span data-ttu-id="3cad2-145">Rozbalte účet úložiště hello **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-145">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="3cad2-146">Klikněte pravým tlačítkem na hello sdílené složky můžete chcete toodelete a - hello kontextové nabídce vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-146">Right-click hello file share you wish toodelete, and - from hello context menu - select **Delete**.</span></span> <span data-ttu-id="3cad2-147">Můžete také stisknout **odstranit** toodelete hello aktuálně vybrané sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-147">You can also press **Delete** toodelete hello currently selected file share.</span></span>

    ![Odstranění](media/vs-azure-tools-storage-explorer-files/image6.png)

5. <span data-ttu-id="3cad2-149">Vyberte **Ano** toohello dialogové okno potvrzení.</span><span class="sxs-lookup"><span data-stu-id="3cad2-149">Select **Yes** toohello confirmation dialog.</span></span>
    
    ![Potvrzovací dialogové okno](media/vs-azure-tools-storage-explorer-files/image7.png)

## <a name="copy-a-file-share"></a><span data-ttu-id="3cad2-151">Kopírování sdílené složky</span><span class="sxs-lookup"><span data-stu-id="3cad2-151">Copy a file share</span></span>

<span data-ttu-id="3cad2-152">Storage Explorer (Preview) vám umožní toocopy schránky toohello sdílenou složku soubor a vložte tuto sdílenou složku souborů do jiný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3cad2-152">Storage Explorer (Preview) enables you toocopy a file share toohello clipboard, and then paste that file share into another storage account.</span></span> <span data-ttu-id="3cad2-153">(toosee jak toocopy jednotlivé soubory, naleznete v části toohello [správu souborů ve sdílené složce](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="3cad2-153">(toosee how toocopy individual files, refer toohello section, [Managing files in a file share](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="3cad2-154">Hello následující kroky ukazují, jak sdílet toocopy soubor z jednoho tooanother účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3cad2-154">hello following steps illustrate how toocopy a file share from one storage account tooanother.</span></span>

1. <span data-ttu-id="3cad2-155">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="3cad2-155">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="3cad2-156">V levém podokně hello rozbalte účet úložiště hello obsahující chcete toocopy hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-156">In hello left pane, expand hello storage account containing hello file share you wish toocopy.</span></span>

3. <span data-ttu-id="3cad2-157">Rozbalte účet úložiště hello **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-157">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="3cad2-158">Klikněte pravým tlačítkem na hello sdílené složky můžete chcete toocopy a - hello kontextové nabídce vyberte **sdílená složka kopie**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-158">Right-click hello file share you wish toocopy, and - from hello context menu - select **Copy File Share**.</span></span>

    ![Kopírování sdílené složky](media/vs-azure-tools-storage-explorer-files/image8.png)

5. <span data-ttu-id="3cad2-160">Klikněte pravým tlačítkem na požadovaný hello "target" účet úložiště, do kterého chcete toopaste hello sdílené složky a - hello kontextové nabídce vyberte **sdílená složka vložení**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-160">Right-click hello desired "target" storage account into which you want toopaste hello file share, and - from hello context menu - select **Paste File Share**.</span></span>

    ![Vložení sdílené složky](media/vs-azure-tools-storage-explorer-files/image9.png)

## <a name="get-hello-sas-for-a-file-share"></a><span data-ttu-id="3cad2-162">Získat hello SAS pro sdílené složky</span><span class="sxs-lookup"><span data-stu-id="3cad2-162">Get hello SAS for a file share</span></span>

<span data-ttu-id="3cad2-163">A [sdílený přístupový podpis (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) poskytuje Delegovaný přístup tooresources ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3cad2-163">A [shared access signature (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) provides delegated access tooresources in your storage account.</span></span> <span data-ttu-id="3cad2-164">To znamená, že můžete udělit, že klient omezené oprávnění tooobjects ve vašem účtu úložiště v zadaném časovém intervalu a zadanou sadu oprávnění, bez nutnosti tooshare klíče pro přístup k účtu.</span><span class="sxs-lookup"><span data-stu-id="3cad2-164">This means that you can grant a client limited permissions tooobjects in your storage account for a specified period of time and with a specified set of permissions, without having tooshare your account access keys.</span></span>

<span data-ttu-id="3cad2-165">Hello následující kroky popisují jak toocreate SAS pro soubor sdílet: +</span><span class="sxs-lookup"><span data-stu-id="3cad2-165">hello following steps illustrate how toocreate a SAS for a file share:+</span></span>

1. <span data-ttu-id="3cad2-166">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="3cad2-166">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="3cad2-167">V levém podokně hello rozbalte účet úložiště hello obsahující hello sdílené složky, pro kterou chcete tooget SAS.</span><span class="sxs-lookup"><span data-stu-id="3cad2-167">In hello left pane, expand hello storage account containing hello file share for which you wish tooget a SAS.</span></span>

3. <span data-ttu-id="3cad2-168">Rozbalte účet úložiště hello **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-168">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="3cad2-169">Klikněte pravým tlačítkem na hello požadované sdílené složky a z kontextové nabídky hello - vyberte **sdíleného přístupového podpisu**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-169">Right-click hello desired file share, and - from hello context menu - select **Get Shared Access Signature**.</span></span>

    ![Získání sdíleného přístupového podpisu](media/vs-azure-tools-storage-explorer-files/image10.png)

5. <span data-ttu-id="3cad2-171">V hello **sdíleného přístupového podpisu** dialogové okno, zadejte hello zásad, spuštění a datum vypršení platnosti, časové pásmo a chcete pro prostředek hello úrovně přístupu.</span><span class="sxs-lookup"><span data-stu-id="3cad2-171">In hello **Shared Access Signature** dialog, specify hello policy, start and expiration dates, time zone, and access levels you want for hello resource.</span></span>

    ![Dialogové okno Sdílený přístupový podpis](media/vs-azure-tools-storage-explorer-files/image11.png)

6. <span data-ttu-id="3cad2-173">Jakmile budete hotovi, zadání možností hello SAS, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-173">When you're finished specifying hello SAS options, select **Create**.</span></span>

7. <span data-ttu-id="3cad2-174">Druhý **sdíleného přístupového podpisu** poté zobrazí dialogové okno, seznamy hello sdílené složky společně s hello adresy URL a QueryStrings můžete použít tooaccess hello prostředků úložiště.</span><span class="sxs-lookup"><span data-stu-id="3cad2-174">A second **Shared Access Signature** dialog will then display that lists hello file share along with hello URL and QueryStrings you can use tooaccess hello storage resource.</span></span> <span data-ttu-id="3cad2-175">Vyberte **kopie** další URL toohello chcete toocopy toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-175">Select **Copy** next toohello URL you wish toocopy toohello clipboard.</span></span>
    
    ![Druhé dialogové okno Sdílený přístupový podpis](media/vs-azure-tools-storage-explorer-files/image12.png)

8. <span data-ttu-id="3cad2-177">Až budete hotovi, vyberte **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-177">When done, select **Close**.</span></span>

## <a name="manage-access-policies-for-a-file-share"></a><span data-ttu-id="3cad2-178">Správa zásad přístupu pro sdílenou složku</span><span class="sxs-lookup"><span data-stu-id="3cad2-178">Manage Access Policies for a file share</span></span>

<span data-ttu-id="3cad2-179">Hello následující kroky popisují jak toomanage (přidání a odebrání) získat přístup k zásadám pro sdílené složky: +.</span><span class="sxs-lookup"><span data-stu-id="3cad2-179">hello following steps illustrate how toomanage (add and remove) access policies for a file share:+ .</span></span> <span data-ttu-id="3cad2-180">Zásady přístupu Hello se používá pro vytvoření adresy URL SAS, pomocí kterého se uživatelé mohou používat tooaccess hello soubor úložiště prostředků v definované období.</span><span class="sxs-lookup"><span data-stu-id="3cad2-180">hello Access Policies is used for creating SAS URLs through which people can use tooaccess hello Storage File resource during a defined period of time.</span></span>

1. <span data-ttu-id="3cad2-181">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="3cad2-181">Open Storage Explorer (Preview).</span></span>

2. <span data-ttu-id="3cad2-182">V levém podokně hello rozbalte účet úložiště hello obsahující hello sdílené složky jejichž zásady přístupu, které chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="3cad2-182">In hello left pane, expand hello storage account containing hello file share whose access policies you wish toomanage.</span></span>

3. <span data-ttu-id="3cad2-183">Rozbalte účet úložiště hello **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-183">Expand hello storage account's **File Shares**.</span></span>

4. <span data-ttu-id="3cad2-184">Vyberte hello požadované sdílené složky a - z kontextové nabídky hello - vyberte **spravovat zásady přístupu**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-184">Select hello desired file share, and - from hello context menu - select **Manage Access Policies**.</span></span>

    ![Místní nabídka Spravovat zásady přístupu](media/vs-azure-tools-storage-explorer-files/image13.png)

5. <span data-ttu-id="3cad2-186">Hello **zásady přístupu** dialogové okno se zobrazí seznam všechny zásady přístupu, které jsou vytvořeny již pro hello vybrané sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-186">hello **Access Policies** dialog will list any access policies already created for hello selected file share.</span></span>
    
    ![Zásady přístupu](media/vs-azure-tools-storage-explorer-files/image14.png)

6. <span data-ttu-id="3cad2-188">Pomocí těchto kroků v závislosti na úlohu správy zásad přístupu hello:</span><span class="sxs-lookup"><span data-stu-id="3cad2-188">Follow these steps depending on hello access policy management task:</span></span>
    
    - <span data-ttu-id="3cad2-189">**Přidání nové zásady přístupu:** Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-189">**Add a new access policy** - Select **Add**.</span></span> <span data-ttu-id="3cad2-190">Po vygenerování hello **zásady přístupu** dialogové okno se zobrazí hello nově přidaná přístup zásad (s výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="3cad2-190">Once generated, hello **Access Policies** dialog will display hello newly added access policy (with default settings).</span></span>

    - <span data-ttu-id="3cad2-191">**Úprava zásady přístupu:** Proveďte požadované úpravy a vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-191">**Edit an access policy** - Make any desired edits, and select **Save**.</span></span>

    - <span data-ttu-id="3cad2-192">**Odebrat zásady přístupu** – vyberte **odebrat** další zásady přístupu toohello chcete tooremove.</span><span class="sxs-lookup"><span data-stu-id="3cad2-192">**Remove an access policy** - Select **Remove** next toohello access policy you wish tooremove.</span></span>

7. <span data-ttu-id="3cad2-193">Vytvořte novou adresu SAS URL pomocí hello zásady přístupu, které jste vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="3cad2-193">Create a new SAS URL using hello Access Policy you created earlier:</span></span>
    
    ![Získání sdíleného přístupového podpisu](media/vs-azure-tools-storage-explorer-files/image15.png)
    
    ![Název a vlastnosti sdíleného přístupového podpisu](media/vs-azure-tools-storage-explorer-files/image16.png)

## <a name="managing-files-in-a-file-share"></a><span data-ttu-id="3cad2-196">Správa souborů ve sdílené složce</span><span class="sxs-lookup"><span data-stu-id="3cad2-196">Managing files in a file share</span></span>

<span data-ttu-id="3cad2-197">Po vytvoření sdílené složky, můžete nahrát soubor toothat sdílené složky, stáhněte soubor tooyour místní počítač, otevřete soubor na místním počítači a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="3cad2-197">Once you've created a file share, you can upload a file toothat file share, download a file tooyour local computer, open a file on your local computer, and much more.</span></span>

<span data-ttu-id="3cad2-198">Hello následující kroky znázorňují, jak sdílet toomanage hello soubory (a složky) v souboru.</span><span class="sxs-lookup"><span data-stu-id="3cad2-198">hello following steps illustrate how toomanage hello files (and folders) within a file share.</span></span>

1.  <span data-ttu-id="3cad2-199">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="3cad2-199">Open Storage Explorer (Preview).</span></span>

2.  <span data-ttu-id="3cad2-200">V levém podokně hello rozbalte účet úložiště hello obsahující chcete toomanage hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-200">In hello left pane, expand hello storage account containing hello file share you wish toomanage.</span></span>

3.  <span data-ttu-id="3cad2-201">Rozbalte účet úložiště hello **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-201">Expand hello storage account's **File Shares**.</span></span>

4.  <span data-ttu-id="3cad2-202">Dvakrát klikněte na sdílenou složku hello chcete tooview.</span><span class="sxs-lookup"><span data-stu-id="3cad2-202">Double-click hello file share you wish tooview.</span></span>

5.  <span data-ttu-id="3cad2-203">Hello hlavním podokně se zobrazí obsah hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-203">hello main pane will display hello file share's contents.</span></span>

    ![Hello obsah sdílené složky](media/vs-azure-tools-storage-explorer-files/image17.png)

6.  <span data-ttu-id="3cad2-205">Hello hlavním podokně se zobrazí obsah hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-205">hello main pane will display hello file share's contents.</span></span>

7.  <span data-ttu-id="3cad2-206">Postupujte podle těchto kroků v závislosti na úkolu hello chcete tooperform:</span><span class="sxs-lookup"><span data-stu-id="3cad2-206">Follow these steps depending on hello task you wish tooperform:</span></span>

    - <span data-ttu-id="3cad2-207">**Nahrání souborů tooa sdílené složky**</span><span class="sxs-lookup"><span data-stu-id="3cad2-207">**Upload files tooa file share**</span></span>

        <span data-ttu-id="3cad2-208">a.</span><span class="sxs-lookup"><span data-stu-id="3cad2-208">a.</span></span>  <span data-ttu-id="3cad2-209">Na panelu nástrojů hello hlavním podokně vyberte **nahrát**a potom **nahrání souborů** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="3cad2-209">On hello main pane's toolbar, select **Upload**, and then **Upload Files** from hello drop-down menu.</span></span>

        ![Nahrání souborů](media/vs-azure-tools-storage-explorer-files/image18.png)
        
        <span data-ttu-id="3cad2-211">b.</span><span class="sxs-lookup"><span data-stu-id="3cad2-211">b.</span></span> <span data-ttu-id="3cad2-212">V hello **nahrání souborů** dialogové okno, vyberte hello třemi tečkami (**...** ) na pravé straně hello hello tlačítko **soubory** textového pole tooselect hello soubory chcete tooupload.</span><span class="sxs-lookup"><span data-stu-id="3cad2-212">In hello **Upload files** dialog, select hello ellipsis (**…**) button on hello right side of hello **Files** text box tooselect hello file(s) you wish tooupload.</span></span>

        ![Přidání souborů](media/vs-azure-tools-storage-explorer-files/image19.png)

        <span data-ttu-id="3cad2-214">c.</span><span class="sxs-lookup"><span data-stu-id="3cad2-214">c.</span></span> <span data-ttu-id="3cad2-215">Vyberte **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-215">Select **Upload**.</span></span>

    - <span data-ttu-id="3cad2-216">**Nahrát tooa sdílené složky**</span><span class="sxs-lookup"><span data-stu-id="3cad2-216">**Upload a folder tooa file share**</span></span>
        
        <span data-ttu-id="3cad2-217">a.</span><span class="sxs-lookup"><span data-stu-id="3cad2-217">a.</span></span> <span data-ttu-id="3cad2-218">Na panelu nástrojů hello hlavním podokně vyberte **nahrát**a potom **nahrát složky** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="3cad2-218">On hello main pane's toolbar, select **Upload**, and then **Upload Folder** from hello drop-down menu.</span></span>

        ![Nabídka Nahrát složku](media/vs-azure-tools-storage-explorer-files/image20.png)

        <span data-ttu-id="3cad2-220">b.</span><span class="sxs-lookup"><span data-stu-id="3cad2-220">b.</span></span> <span data-ttu-id="3cad2-221">V hello **nahrávání složky** dialogové okno, vyberte hello třemi tečkami (**...** ) na pravé straně hello hello tlačítko **složky** textové pole tooselect hello složku, jejíž obsah, chcete tooupload.</span><span class="sxs-lookup"><span data-stu-id="3cad2-221">In hello **Upload folder** dialog, select hello ellipsis (**…**) button on hello right side of hello **Folder** text box tooselect hello folder whose contents you wish tooupload.</span></span>

        <span data-ttu-id="3cad2-222">c.</span><span class="sxs-lookup"><span data-stu-id="3cad2-222">c.</span></span> <span data-ttu-id="3cad2-223">Volitelně zadejte cílovou složku, do které hello se nahraje obsah vybrané složky.</span><span class="sxs-lookup"><span data-stu-id="3cad2-223">Optionally, specify a target folder into which hello selected folder's contents will be uploaded.</span></span> <span data-ttu-id="3cad2-224">Pokud hello cílové složce neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="3cad2-224">If hello target folder doesn’t exist, it will be created.</span></span>

        <span data-ttu-id="3cad2-225">d.</span><span class="sxs-lookup"><span data-stu-id="3cad2-225">d.</span></span> <span data-ttu-id="3cad2-226">Vyberte **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-226">Select **Upload**.</span></span>

    - <span data-ttu-id="3cad2-227">**Stáhněte si soubor tooyour místní počítač**</span><span class="sxs-lookup"><span data-stu-id="3cad2-227">**Download a file tooyour local computer**</span></span>
        
        <span data-ttu-id="3cad2-228">a.</span><span class="sxs-lookup"><span data-stu-id="3cad2-228">a.</span></span> <span data-ttu-id="3cad2-229">Vyberte soubor hello chcete toodownload.</span><span class="sxs-lookup"><span data-stu-id="3cad2-229">Select hello file you wish toodownload.</span></span>
        
        <span data-ttu-id="3cad2-230">b.</span><span class="sxs-lookup"><span data-stu-id="3cad2-230">b.</span></span> <span data-ttu-id="3cad2-231">Na panelu nástrojů hello hlavním podokně vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-231">On hello main pane's toolbar, select **Download**.</span></span>
        
        <span data-ttu-id="3cad2-232">c.</span><span class="sxs-lookup"><span data-stu-id="3cad2-232">c.</span></span> <span data-ttu-id="3cad2-233">V hello **zadejte, kde toosave hello stáhli soubor** dialogové okno, zadejte hello umístění, kam chcete soubor hello stažený a hello název chcete toogive ho.</span><span class="sxs-lookup"><span data-stu-id="3cad2-233">In hello **Specify where toosave hello downloaded file** dialog, specify hello location where you want hello file downloaded, and hello name you wish toogive it.</span></span>

        <span data-ttu-id="3cad2-234">d.</span><span class="sxs-lookup"><span data-stu-id="3cad2-234">d.</span></span> <span data-ttu-id="3cad2-235">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-235">Select **Save**.</span></span>

    - <span data-ttu-id="3cad2-236">**Otevření souboru na místním počítači**</span><span class="sxs-lookup"><span data-stu-id="3cad2-236">**Open a file on your local computer**</span></span>
        
        <span data-ttu-id="3cad2-237">a.</span><span class="sxs-lookup"><span data-stu-id="3cad2-237">a.</span></span>  <span data-ttu-id="3cad2-238">Vyberte soubor hello chcete tooopen.</span><span class="sxs-lookup"><span data-stu-id="3cad2-238">Select hello file you wish tooopen.</span></span>
        
        <span data-ttu-id="3cad2-239">b.</span><span class="sxs-lookup"><span data-stu-id="3cad2-239">b.</span></span>  <span data-ttu-id="3cad2-240">Na panelu nástrojů hello hlavním podokně vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-240">On hello main pane's toolbar, select **Open**.</span></span>
        
        <span data-ttu-id="3cad2-241">c.</span><span class="sxs-lookup"><span data-stu-id="3cad2-241">c.</span></span>  <span data-ttu-id="3cad2-242">Hello soubor se stáhne a otevřít pomocí aplikace hello spojené s hello základní typ souboru.</span><span class="sxs-lookup"><span data-stu-id="3cad2-242">hello file will be downloaded and opened using hello application associated with hello file's underlying file type.</span></span>

    - <span data-ttu-id="3cad2-243">**Zkopírujte soubor toohello schránky**</span><span class="sxs-lookup"><span data-stu-id="3cad2-243">**Copy a file toohello clipboard**</span></span>

        <span data-ttu-id="3cad2-244">a.</span><span class="sxs-lookup"><span data-stu-id="3cad2-244">a.</span></span> <span data-ttu-id="3cad2-245">Vyberte soubor hello chcete toocopy.</span><span class="sxs-lookup"><span data-stu-id="3cad2-245">Select hello file you wish toocopy.</span></span>

        <span data-ttu-id="3cad2-246">b.</span><span class="sxs-lookup"><span data-stu-id="3cad2-246">b.</span></span> <span data-ttu-id="3cad2-247">Na panelu nástrojů hello hlavním podokně vyberte **kopie**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-247">On hello main pane's toolbar, select **Copy**.</span></span>

        <span data-ttu-id="3cad2-248">c.</span><span class="sxs-lookup"><span data-stu-id="3cad2-248">c.</span></span> <span data-ttu-id="3cad2-249">V levém podokně hello přejděte tooanother sdílené složky a klikněte dvakrát na jeho tooview v hlavním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="3cad2-249">In hello left pane, navigate tooanother file share, and double-click it tooview it in hello main pane.</span></span>

        <span data-ttu-id="3cad2-250">d.</span><span class="sxs-lookup"><span data-stu-id="3cad2-250">d.</span></span> <span data-ttu-id="3cad2-251">Na panelu nástrojů hello hlavním podokně vyberte **vložení** toocreate kopii souboru hello.</span><span class="sxs-lookup"><span data-stu-id="3cad2-251">On hello main pane's toolbar, select **Paste** toocreate a copy of hello file.</span></span>

    - <span data-ttu-id="3cad2-252">**Odstranění souboru**</span><span class="sxs-lookup"><span data-stu-id="3cad2-252">**Delete a file**</span></span>

        <span data-ttu-id="3cad2-253">a.</span><span class="sxs-lookup"><span data-stu-id="3cad2-253">a.</span></span> <span data-ttu-id="3cad2-254">Vyberte soubor hello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="3cad2-254">Select hello file you wish toodelete.</span></span>

        <span data-ttu-id="3cad2-255">b.</span><span class="sxs-lookup"><span data-stu-id="3cad2-255">b.</span></span> <span data-ttu-id="3cad2-256">Na panelu nástrojů hello hlavním podokně vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3cad2-256">On hello main pane's toolbar, select **Delete**.</span></span>

        <span data-ttu-id="3cad2-257">c.</span><span class="sxs-lookup"><span data-stu-id="3cad2-257">c.</span></span> <span data-ttu-id="3cad2-258">Vyberte **Ano** toohello dialogové okno potvrzení.</span><span class="sxs-lookup"><span data-stu-id="3cad2-258">Select **Yes** toohello confirmation dialog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3cad2-259">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3cad2-259">Next steps</span></span>

- <span data-ttu-id="3cad2-260">Zobrazení hello [nejnovější poznámky k verzi Storage Explorer (Preview) a videa](http://www.storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="3cad2-260">View hello [latest Storage Explorer (Preview) release notes and videos](http://www.storageexplorer.com/).</span></span>

- <span data-ttu-id="3cad2-261">Zjistěte, jak příliš[vytvářet aplikace, které používají Azure BLOB, tabulek, front a soubory](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="3cad2-261">Learn how too[create applications using Azure blobs, tables, queues, and files](https://azure.microsoft.com/documentation/services/storage/).</span></span>
