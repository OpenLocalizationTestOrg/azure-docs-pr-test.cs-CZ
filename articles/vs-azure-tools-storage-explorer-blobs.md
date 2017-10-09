---
title: "aaaManage prostředků Azure Blob Storage pomocí Storage Exploreru (Preview) | Microsoft Docs"
description: "Správa objektů Blob v Azure kontejnerům a objektům BLOB pomocí Storage Exploreru (Preview)"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 503dd061b205875da127378ab48e8d465800fc09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a><span data-ttu-id="beb08-103">Správa prostředků Azure Blob Storage pomocí Storage Exploreru (Preview)</span><span class="sxs-lookup"><span data-stu-id="beb08-103">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="beb08-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="beb08-104">Overview</span></span>
<span data-ttu-id="beb08-105">[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která je přístupná z kdekoli v hello, world pomocí protokolů HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="beb08-105">[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span>
<span data-ttu-id="beb08-106">Data tooexpose úložiště objektů Blob můžete použít veřejně toohello svět nebo data aplikací toostore soukromě.</span><span class="sxs-lookup"><span data-stu-id="beb08-106">You can use Blob storage tooexpose data publicly toohello world, or toostore application data privately.</span></span> <span data-ttu-id="beb08-107">V tomto článku se dozvíte jak toouse toowork Storage Explorer (Preview) s kontejnery objektů blob a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="beb08-107">In this article, you'll learn how toouse Storage Explorer (Preview) toowork with blob containers and blobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="beb08-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="beb08-108">Prerequisites</span></span>
<span data-ttu-id="beb08-109">toocomplete hello kroky v tomto článku, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="beb08-109">toocomplete hello steps in this article, you'll need hello following:</span></span>

* [<span data-ttu-id="beb08-110">Stažení a instalace Storage Exploreru (Preview)</span><span class="sxs-lookup"><span data-stu-id="beb08-110">Download and install Storage Explorer (preview)</span></span>](http://www.storageexplorer.com)
* [<span data-ttu-id="beb08-111">Připojení účtu úložiště Azure tooa nebo služby</span><span class="sxs-lookup"><span data-stu-id="beb08-111">Connect tooa Azure storage account or service</span></span>](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a><span data-ttu-id="beb08-112">Vytvořte kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="beb08-112">Create a blob container</span></span>
<span data-ttu-id="beb08-113">Všechny objekty BLOB se musí nacházet v kontejneru objektů blob, který je jednoduše logické seskupení objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="beb08-113">All blobs must reside in a blob container, which is simply a logical grouping of blobs.</span></span> <span data-ttu-id="beb08-114">Účet může obsahovat neomezený počet kontejnerů a každý kontejner můžete pojmout neomezený počet objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="beb08-114">An account can contain an unlimited number of containers, and each container can store an unlimited number of blobs.</span></span>

<span data-ttu-id="beb08-115">Hello následující kroky popisují jak toocreate kontejner objektů blob v rámci Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="beb08-115">hello following steps illustrate how toocreate a blob container within Storage Explorer (Preview).</span></span>

1. <span data-ttu-id="beb08-116">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="beb08-116">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="beb08-117">V levém podokně hello rozbalte účet hello úložiště, ve kterém chcete kontejner objektů blob toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-117">In hello left pane, expand hello storage account within which you wish toocreate hello blob container.</span></span>
3. <span data-ttu-id="beb08-118">Klikněte pravým tlačítkem na **kontejnery objektů Blob**a - hello kontextové nabídce vyberte **vytvořit kontejner objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="beb08-118">Right-click **Blob Containers**, and - from hello context menu - select **Create Blob Container**.</span></span>

   ![Vytvoření objektu blob kontejnery kontextové nabídky][0]
4. <span data-ttu-id="beb08-120">V textovém poli se zobrazí pod hello **kontejnery objektů Blob** složky.</span><span class="sxs-lookup"><span data-stu-id="beb08-120">A text box will appear below hello **Blob Containers** folder.</span></span> <span data-ttu-id="beb08-121">Zadejte název hello kontejnerech objektů blob.</span><span class="sxs-lookup"><span data-stu-id="beb08-121">Enter hello name for your blob container.</span></span> <span data-ttu-id="beb08-122">V tématu hello [pravidla pojmenování kontejneru](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) části Seznam pravidel a omezení pro pojmenovávání kontejnerů objektů blob.</span><span class="sxs-lookup"><span data-stu-id="beb08-122">See hello [Container naming rules](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) section for a list of rules and restrictions on naming blob containers.</span></span>

   ![Vytvoření textového pole kontejnery objektů Blob][1]
5. <span data-ttu-id="beb08-124">Stiskněte klávesu **Enter** po dokončení kontejneru objektu blob hello toocreate nebo **Esc** toocancel.</span><span class="sxs-lookup"><span data-stu-id="beb08-124">Press **Enter** when done toocreate hello blob container, or **Esc** toocancel.</span></span> <span data-ttu-id="beb08-125">Po úspěšném vytvoření kontejneru objektů blob hello se zobrazí v části hello **kontejnery objektů Blob** složku pro hello vybraný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="beb08-125">Once hello blob container has been successfully created, it will be displayed under hello **Blob Containers** folder for hello selected storage account.</span></span>

   ![Vytvořit kontejner objektů BLOB][2]

## <a name="view-a-blob-containers-contents"></a><span data-ttu-id="beb08-127">Zobrazit obsah kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="beb08-127">View a blob container's contents</span></span>
<span data-ttu-id="beb08-128">Kontejnery objektů BLOB obsahovat objekty BLOB a složky (které mohou obsahovat také objekty BLOB).</span><span class="sxs-lookup"><span data-stu-id="beb08-128">Blob containers contain blobs and folders (that can also contain blobs).</span></span>

<span data-ttu-id="beb08-129">Hello následující kroky popisují jak tooview hello obsah kontejneru objektů blob v rámci Storage Explorer (Preview):</span><span class="sxs-lookup"><span data-stu-id="beb08-129">hello following steps illustrate how tooview hello contents of a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="beb08-130">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="beb08-130">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="beb08-131">V levém podokně hello rozbalte účet úložiště hello obsahující chcete tooview kontejner objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-131">In hello left pane, expand hello storage account containing hello blob container you wish tooview.</span></span>
3. <span data-ttu-id="beb08-132">Rozbalte účet úložiště hello **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="beb08-132">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="beb08-133">Klikněte pravým tlačítkem na kontejner objektů blob hello je chcete tooview a - hello kontextové nabídce vyberte **otevřete Editor kontejner objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="beb08-133">Right-click hello blob container you wish tooview, and - from hello context menu - select **Open Blob Container Editor**.</span></span>
   <span data-ttu-id="beb08-134">Dvakrát klikněte na kontejner objektů blob hello chcete tooview.</span><span class="sxs-lookup"><span data-stu-id="beb08-134">You can also double-click hello blob container you wish tooview.</span></span>

   ![Otevřete blob kontejneru editor kontextové nabídky][19]
5. <span data-ttu-id="beb08-136">Hello hlavním podokně se zobrazí obsah kontejneru objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-136">hello main pane will display hello blob container's contents.</span></span>

   ![Editor kontejner objektů BLOB][3]

## <a name="delete-a-blob-container"></a><span data-ttu-id="beb08-138">Odstranit kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="beb08-138">Delete a blob container</span></span>
<span data-ttu-id="beb08-139">Kontejnery objektů BLOB můžete snadno vytvořit a odstraněn podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="beb08-139">Blob containers can be easily created and deleted as needed.</span></span> <span data-ttu-id="beb08-140">(toosee jak toodelete jednotlivé objekty BLOB, najdete v části toohello [Správa objektů BLOB v kontejneru objektů blob](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="beb08-140">(toosee how toodelete individual blobs, refer toohello section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="beb08-141">Hello následující kroky popisují jak toodelete kontejner objektů blob v rámci Storage Explorer (Preview):</span><span class="sxs-lookup"><span data-stu-id="beb08-141">hello following steps illustrate how toodelete a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="beb08-142">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="beb08-142">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="beb08-143">V levém podokně hello rozbalte účet úložiště hello obsahující chcete tooview kontejner objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-143">In hello left pane, expand hello storage account containing hello blob container you wish tooview.</span></span>
3. <span data-ttu-id="beb08-144">Rozbalte účet úložiště hello **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="beb08-144">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="beb08-145">Klikněte pravým tlačítkem na kontejner objektů blob hello je chcete toodelete a - hello kontextové nabídce vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="beb08-145">Right-click hello blob container you wish toodelete, and - from hello context menu - select **Delete**.</span></span>
   <span data-ttu-id="beb08-146">Můžete také stisknout **odstranit** toodelete hello aktuálně vybraného objektu blob kontejneru.</span><span class="sxs-lookup"><span data-stu-id="beb08-146">You can also press **Delete** toodelete hello currently selected blob container.</span></span>

   ![Odstranit objekt blob kontejneru kontextové nabídky][4]
5. <span data-ttu-id="beb08-148">Vyberte **Ano** toohello dialogové okno potvrzení.</span><span class="sxs-lookup"><span data-stu-id="beb08-148">Select **Yes** toohello confirmation dialog.</span></span>

   ![Odstranit objekt blob kontejneru potvrzení][5]

## <a name="copy-a-blob-container"></a><span data-ttu-id="beb08-150">Zkopírujte kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="beb08-150">Copy a blob container</span></span>
<span data-ttu-id="beb08-151">Storage Explorer (Preview) vám umožní toocopy schránky toohello kontejner objektů blob a potom vložení, který kontejner objektů blob do jiný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="beb08-151">Storage Explorer (Preview) enables you toocopy a blob container toohello clipboard, and then paste that blob container into another storage account.</span></span> <span data-ttu-id="beb08-152">(toosee jak toocopy jednotlivé objekty BLOB, najdete v části toohello [Správa objektů BLOB v kontejneru objektů blob](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="beb08-152">(toosee how toocopy individual blobs, refer toohello section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="beb08-153">Hello následující kroky ukazují, jak toocopy kontejner objektů blob z jednoho úložiště účet tooanother.</span><span class="sxs-lookup"><span data-stu-id="beb08-153">hello following steps illustrate how toocopy a blob container from one storage account tooanother.</span></span>

1. <span data-ttu-id="beb08-154">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="beb08-154">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="beb08-155">V levém podokně hello rozbalte účet úložiště hello obsahující chcete toocopy kontejner objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-155">In hello left pane, expand hello storage account containing hello blob container you wish toocopy.</span></span>
3. <span data-ttu-id="beb08-156">Rozbalte účet úložiště hello **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="beb08-156">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="beb08-157">Klikněte pravým tlačítkem na kontejner objektů blob hello je chcete toocopy a - hello kontextové nabídce vyberte **kontejner objektů Blob kopie**.</span><span class="sxs-lookup"><span data-stu-id="beb08-157">Right-click hello blob container you wish toocopy, and - from hello context menu - select **Copy Blob Container**.</span></span>

   ![Kopírovat objekt blob kontejneru kontextové nabídky][6]
5. <span data-ttu-id="beb08-159">Klikněte pravým tlačítkem na požadované hello "target" účet úložiště, do kterého chcete kontejner objektů blob hello toopaste a - hello kontextové nabídce vyberte **kontejner objektů Blob vložení**.</span><span class="sxs-lookup"><span data-stu-id="beb08-159">Right-click hello desired "target" storage account into which you want toopaste hello blob container, and - from hello context menu - select **Paste Blob Container**.</span></span>

   ![Vložení objektu blob kontejneru kontextové nabídky][7]

## <a name="get-hello-sas-for-a-blob-container"></a><span data-ttu-id="beb08-161">Získat hello SAS pro kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="beb08-161">Get hello SAS for a blob container</span></span>
<span data-ttu-id="beb08-162">A [sdílený přístupový podpis (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) poskytuje Delegovaný přístup tooresources ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="beb08-162">A [shared access signature (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) provides delegated access tooresources in your storage account.</span></span>
<span data-ttu-id="beb08-163">To znamená, že můžete udělit, že klient omezené oprávnění tooobjects ve vašem účtu úložiště v zadaném časovém intervalu a zadanou sadu oprávnění, aniž by museli sdílet klíče pro přístup k účtu.</span><span class="sxs-lookup"><span data-stu-id="beb08-163">This means that you can grant a client limited permissions tooobjects in your storage account for a specified period of time and with a specified set of permissions, without having to share your account access keys.</span></span>

<span data-ttu-id="beb08-164">Hello následující kroky popisují jak toocreate SAS pro kontejner objektů blob:</span><span class="sxs-lookup"><span data-stu-id="beb08-164">hello following steps illustrate how toocreate a SAS for a blob container:</span></span>

1. <span data-ttu-id="beb08-165">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="beb08-165">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="beb08-166">V levém podokně hello rozbalte účet úložiště hello obsahující kontejner objektů blob hello, pro kterou chcete tooget SAS.</span><span class="sxs-lookup"><span data-stu-id="beb08-166">In hello left pane, expand hello storage account containing hello blob container for which you wish tooget a SAS.</span></span>
3. <span data-ttu-id="beb08-167">Rozbalte účet úložiště hello **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="beb08-167">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="beb08-168">Klikněte pravým tlačítkem na kontejner objektů blob požadované hello a - hello kontextové nabídce vyberte **sdíleného přístupového podpisu**.</span><span class="sxs-lookup"><span data-stu-id="beb08-168">Right-click hello desired blob container, and - from hello context menu - select **Get Shared Access Signature**.</span></span>

   ![Získat SAS kontextové nabídky][8]
5. <span data-ttu-id="beb08-170">V hello **sdíleného přístupového podpisu** dialogové okno, zadejte hello zásad, spuštění a datum vypršení platnosti, časové pásmo a chcete pro prostředek hello úrovně přístupu.</span><span class="sxs-lookup"><span data-stu-id="beb08-170">In hello **Shared Access Signature** dialog, specify hello policy, start and expiration dates, time zone, and access levels you want for hello resource.</span></span>

   ![Získat možnosti SAS][9]
6. <span data-ttu-id="beb08-172">Jakmile budete hotovi, zadání možností hello SAS, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="beb08-172">When you're finished specifying hello SAS options, select **Create**.</span></span>
7. <span data-ttu-id="beb08-173">Druhý **sdíleného přístupového podpisu** poté zobrazí dialogové okno, seznamy hello kontejner objektů blob společně s hello adresy URL a QueryStrings můžete použít tooaccess hello prostředků úložiště.</span><span class="sxs-lookup"><span data-stu-id="beb08-173">A second **Shared Access Signature** dialog will then display that lists hello blob container along with hello URL and QueryStrings you can use tooaccess hello storage resource.</span></span>
   <span data-ttu-id="beb08-174">Vyberte **kopie** další URL toohello chcete toocopy toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="beb08-174">Select **Copy** next toohello URL you wish toocopy toohello clipboard.</span></span>

   ![Zkopírování adresy URL SAS][10]
8. <span data-ttu-id="beb08-176">Až budete hotovi, vyberte **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="beb08-176">When done, select **Close**.</span></span>

## <a name="manage-access-policies-for-a-blob-container"></a><span data-ttu-id="beb08-177">Správa zásad přístupu pro kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="beb08-177">Manage Access Policies for a blob container</span></span>
<span data-ttu-id="beb08-178">Hello následující kroky popisují jak toomanage (přidání a odebrání) získat přístup k zásadám pro kontejner objektů blob:</span><span class="sxs-lookup"><span data-stu-id="beb08-178">hello following steps illustrate how toomanage (add and remove) access policies for a blob container:</span></span>

1. <span data-ttu-id="beb08-179">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="beb08-179">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="beb08-180">V levém podokně hello rozbalte účet úložiště hello obsahující kontejner objektů blob hello jejichž zásady přístupu, které chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="beb08-180">In hello left pane, expand hello storage account containing hello blob container whose access policies you wish toomanage.</span></span>
3. <span data-ttu-id="beb08-181">Rozbalte účet úložiště hello **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="beb08-181">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="beb08-182">Vyberte kontejner objektů blob požadované hello a - z kontextové nabídky hello - vyberte **spravovat zásady přístupu**.</span><span class="sxs-lookup"><span data-stu-id="beb08-182">Select hello desired blob container, and - from hello context menu - select **Manage Access Policies**.</span></span>

   ![Místní nabídka Spravovat zásady přístupu][11]
5. <span data-ttu-id="beb08-184">Hello **zásady přístupu** dialogové okno se zobrazí seznam všechny zásady přístupu, které jsou vytvořeny již pro kontejner objektů blob vybrané hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-184">hello **Access Policies** dialog will list any access policies already created for hello selected blob container.</span></span>

   ![Možnosti zásad přístupu][12]        
6. <span data-ttu-id="beb08-186">Pomocí těchto kroků v závislosti na úlohu správy zásad přístupu hello:</span><span class="sxs-lookup"><span data-stu-id="beb08-186">Follow these steps depending on hello access policy management task:</span></span>

   * <span data-ttu-id="beb08-187">**Přidání nové zásady přístupu:** Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="beb08-187">**Add a new access policy** - Select **Add**.</span></span> <span data-ttu-id="beb08-188">Po vygenerování hello **zásady přístupu** dialogové okno se zobrazí hello nově přidaná přístup zásad (s výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="beb08-188">Once generated, hello **Access Policies** dialog will display hello newly added access policy (with default settings).</span></span>
   * <span data-ttu-id="beb08-189">**Upravit zásady přístupu** – proveďte veškeré požadované úpravy a vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="beb08-189">**Edit an access policy** -  Make any desired edits, and select **Save**.</span></span>
   * <span data-ttu-id="beb08-190">**Odebrat zásady přístupu** – vyberte **odebrat** další zásady přístupu toohello chcete tooremove.</span><span class="sxs-lookup"><span data-stu-id="beb08-190">**Remove an access policy** - Select **Remove** next toohello access policy you wish tooremove.</span></span>

## <a name="set-hello-public-access-level-for-a-blob-container"></a><span data-ttu-id="beb08-191">Nastavit hello úroveň veřejný přístup pro kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="beb08-191">Set hello Public Access Level for a blob container</span></span>
<span data-ttu-id="beb08-192">Ve výchozím nastavení, každý kontejner objektů blob je nastaven příliš "Žádné veřejný přístup".</span><span class="sxs-lookup"><span data-stu-id="beb08-192">By default, every blob container is set too"No public access".</span></span>

<span data-ttu-id="beb08-193">Hello následující kroky popisují toospecify veřejné přístupu úrovně pro kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="beb08-193">hello following steps illustrate how toospecify a public access level for a blob container.</span></span>

1. <span data-ttu-id="beb08-194">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="beb08-194">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="beb08-195">V levém podokně hello rozbalte účet úložiště hello obsahující kontejner objektů blob hello jejichž zásady přístupu, které chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="beb08-195">In hello left pane, expand hello storage account containing hello blob container whose access policies you wish toomanage.</span></span>
3. <span data-ttu-id="beb08-196">Rozbalte účet úložiště hello **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="beb08-196">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="beb08-197">Vyberte kontejner objektů blob požadované hello a - z kontextové nabídky hello - vyberte **nastavit úroveň veřejný přístup**.</span><span class="sxs-lookup"><span data-stu-id="beb08-197">Select hello desired blob container, and - from hello context menu - select **Set Public Access Level**.</span></span>

   ![Nastavení úrovně kontextovou nabídku veřejného přístupu][13]
5. <span data-ttu-id="beb08-199">V hello **nastavit úroveň veřejný přístup kontejneru** dialogové okno, zadejte úroveň přístupu hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="beb08-199">In hello **Set Container Public Access Level** dialog, specify hello desired access level.</span></span>

   ![Nastavení možností úrovně veřejného přístupu][14]
6. <span data-ttu-id="beb08-201">Vyberte **Použít**.</span><span class="sxs-lookup"><span data-stu-id="beb08-201">Select **Apply**.</span></span>

## <a name="managing-blobs-in-a-blob-container"></a><span data-ttu-id="beb08-202">Správa objektů BLOB v kontejneru objektů blob</span><span class="sxs-lookup"><span data-stu-id="beb08-202">Managing blobs in a blob container</span></span>
<span data-ttu-id="beb08-203">Po vytvoření kontejneru objektů blob, můžete nahrát objekt blob kontejneru objektů blob toothat, stažení objektů blob tooyour místní počítač, otevřete objekt blob na místním počítači a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="beb08-203">Once you've created a blob container, you can upload a blob toothat blob container, download a blob tooyour local computer, open a blob on your local computer, and much more.</span></span>

<span data-ttu-id="beb08-204">Hello následující kroky ukazují, jak toomanage hello objekty BLOB (a složky) v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="beb08-204">hello following steps illustrate how toomanage hello blobs (and folders) within a blob container.</span></span>

1. <span data-ttu-id="beb08-205">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="beb08-205">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="beb08-206">V levém podokně hello rozbalte účet úložiště hello obsahující chcete toomanage kontejner objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-206">In hello left pane, expand hello storage account containing hello blob container you wish toomanage.</span></span>
3. <span data-ttu-id="beb08-207">Rozbalte účet úložiště hello **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="beb08-207">Expand hello storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="beb08-208">Dvakrát klikněte na kontejner objektů blob hello chcete tooview.</span><span class="sxs-lookup"><span data-stu-id="beb08-208">Double-click hello blob container you wish tooview.</span></span>
5. <span data-ttu-id="beb08-209">Hello hlavním podokně se zobrazí obsah kontejneru objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-209">hello main pane will display hello blob container's contents.</span></span>

   ![Kontejner objektů blob zobrazení][3]
6. <span data-ttu-id="beb08-211">Hello hlavním podokně se zobrazí obsah kontejneru objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-211">hello main pane will display hello blob container's contents.</span></span>
7. <span data-ttu-id="beb08-212">Postupujte podle těchto kroků v závislosti na úkolu hello chcete tooperform:</span><span class="sxs-lookup"><span data-stu-id="beb08-212">Follow these steps depending on hello task you wish tooperform:</span></span>

   * <span data-ttu-id="beb08-213">**Nahrát kontejner objektů blob tooa soubory**</span><span class="sxs-lookup"><span data-stu-id="beb08-213">**Upload files tooa blob container**</span></span>

     1. <span data-ttu-id="beb08-214">Na panelu nástrojů hello hlavním podokně vyberte **nahrát**a potom **nahrání souborů** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-214">On hello main pane's toolbar, select **Upload**, and then **Upload Files** from hello drop-down menu.</span></span>

        ![Nahrát soubory nabídky][15]
     2. <span data-ttu-id="beb08-216">V hello **nahrání souborů** dialogové okno, vyberte hello třemi tečkami (**...** ) na pravé straně hello hello tlačítko **soubory** textového pole tooselect hello soubory chcete tooupload.</span><span class="sxs-lookup"><span data-stu-id="beb08-216">In hello **Upload files** dialog, select hello ellipsis (**…**) button on hello right side of hello **Files** text box tooselect hello file(s) you wish tooupload.</span></span>

        ![Nahrávání souborů možnosti][16]
     3. <span data-ttu-id="beb08-218">Zadejte typ hello **Blob typu**.</span><span class="sxs-lookup"><span data-stu-id="beb08-218">Specify hello type of **Blob type**.</span></span> <span data-ttu-id="beb08-219">článek Hello [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) hello rozdíly mezi hello vysvětluje různé typy objektů blob.</span><span class="sxs-lookup"><span data-stu-id="beb08-219">hello article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains hello differences between hello various blob types.</span></span>
     4. <span data-ttu-id="beb08-220">Volitelně zadejte cílovou složku, do kterého se nahraje hello vybrané soubory.</span><span class="sxs-lookup"><span data-stu-id="beb08-220">Optionally, specify a target folder into which hello selected file(s) will be uploaded.</span></span> <span data-ttu-id="beb08-221">Pokud hello cílové složce neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="beb08-221">If hello target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="beb08-222">Vyberte **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="beb08-222">Select **Upload**.</span></span>
   * <span data-ttu-id="beb08-223">**Nahrát kontejneru objektů blob tooa složky**</span><span class="sxs-lookup"><span data-stu-id="beb08-223">**Upload a folder tooa blob container**</span></span>

     1. <span data-ttu-id="beb08-224">Na panelu nástrojů hello hlavním podokně vyberte **nahrát**a potom **nahrát složky** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-224">On hello main pane's toolbar, select **Upload**, and then **Upload Folder** from hello drop-down menu.</span></span>

        ![Nabídka Nahrát složku][17]
     2. <span data-ttu-id="beb08-226">V hello **nahrávání složky** dialogové okno, vyberte hello třemi tečkami (**...** ) na pravé straně hello hello tlačítko **složky** textové pole tooselect hello složku, jejíž obsah, chcete tooupload.</span><span class="sxs-lookup"><span data-stu-id="beb08-226">In hello **Upload folder** dialog, select hello ellipsis (**…**) button on hello right side of hello **Folder** text box tooselect hello folder whose contents you wish tooupload.</span></span>

        ![Nahrát Možnosti složky][18]
     3. <span data-ttu-id="beb08-228">Zadejte typ hello **Blob typu**.</span><span class="sxs-lookup"><span data-stu-id="beb08-228">Specify hello type of **Blob type**.</span></span> <span data-ttu-id="beb08-229">článek Hello [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) hello rozdíly mezi hello vysvětluje různé typy objektů blob.</span><span class="sxs-lookup"><span data-stu-id="beb08-229">hello article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains hello differences between hello various blob types.</span></span>
     4. <span data-ttu-id="beb08-230">Volitelně zadejte cílovou složku, do které hello se nahraje obsah vybrané složky.</span><span class="sxs-lookup"><span data-stu-id="beb08-230">Optionally, specify a target folder into which hello selected folder's contents will be uploaded.</span></span> <span data-ttu-id="beb08-231">Pokud hello cílové složce neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="beb08-231">If hello target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="beb08-232">Vyberte **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="beb08-232">Select **Upload**.</span></span>
   * <span data-ttu-id="beb08-233">**Stáhněte si místní počítač tooyour objektů blob**</span><span class="sxs-lookup"><span data-stu-id="beb08-233">**Download a blob tooyour local computer**</span></span>

     1. <span data-ttu-id="beb08-234">Vyberte objekt blob hello chcete toodownload.</span><span class="sxs-lookup"><span data-stu-id="beb08-234">Select hello blob you wish toodownload.</span></span>
     2. <span data-ttu-id="beb08-235">Na panelu nástrojů hello hlavním podokně vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="beb08-235">On hello main pane's toolbar, select **Download**.</span></span>
     3. <span data-ttu-id="beb08-236">V hello **zadejte, kam toosave hello stáhli blob** dialogové okno, zadejte hello umístění, kam má hello blob stáhli, a hello název chcete toogive ho.</span><span class="sxs-lookup"><span data-stu-id="beb08-236">In hello **Specify where toosave hello downloaded blob** dialog, specify hello location where you want hello blob downloaded, and hello name you wish toogive it.</span></span>  
     4. <span data-ttu-id="beb08-237">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="beb08-237">Select **Save**.</span></span>
   * <span data-ttu-id="beb08-238">**Otevřete objekt blob do místního počítače**</span><span class="sxs-lookup"><span data-stu-id="beb08-238">**Open a blob on your local computer**</span></span>

     1. <span data-ttu-id="beb08-239">Vyberte objekt blob hello chcete tooopen.</span><span class="sxs-lookup"><span data-stu-id="beb08-239">Select hello blob you wish tooopen.</span></span>
     2. <span data-ttu-id="beb08-240">Na panelu nástrojů hello hlavním podokně vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="beb08-240">On hello main pane's toolbar, select **Open**.</span></span>
     3. <span data-ttu-id="beb08-241">Objekt blob Hello se stáhne a otevřít pomocí aplikace hello spojené s hello blob základní typ souboru.</span><span class="sxs-lookup"><span data-stu-id="beb08-241">hello blob will be downloaded and opened using hello application associated with hello blob's underlying file type.</span></span>
   * <span data-ttu-id="beb08-242">**Kopírování schránky toohello objektů blob**</span><span class="sxs-lookup"><span data-stu-id="beb08-242">**Copy a blob toohello clipboard**</span></span>

     1. <span data-ttu-id="beb08-243">Vyberte objekt blob hello chcete toocopy.</span><span class="sxs-lookup"><span data-stu-id="beb08-243">Select hello blob you wish toocopy.</span></span>
     2. <span data-ttu-id="beb08-244">Na panelu nástrojů hello hlavním podokně vyberte **kopie**.</span><span class="sxs-lookup"><span data-stu-id="beb08-244">On hello main pane's toolbar, select **Copy**.</span></span>
     3. <span data-ttu-id="beb08-245">V levém podokně hello přejděte tooanother kontejner objektů blob a dvojím kliknutím tooview v hlavním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="beb08-245">In hello left pane, navigate tooanother blob container, and double-click it tooview it in hello main pane.</span></span>
     4. <span data-ttu-id="beb08-246">Na panelu nástrojů hello hlavním podokně vyberte **vložení** toocreate kopii hello objektů blob.</span><span class="sxs-lookup"><span data-stu-id="beb08-246">On hello main pane's toolbar, select **Paste** toocreate a copy of hello blob.</span></span>
   * <span data-ttu-id="beb08-247">**Odstranit objekt blob**</span><span class="sxs-lookup"><span data-stu-id="beb08-247">**Delete a blob**</span></span>

     1. <span data-ttu-id="beb08-248">Vyberte objekt blob hello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="beb08-248">Select hello blob you wish toodelete.</span></span>
     2. <span data-ttu-id="beb08-249">Na panelu nástrojů hello hlavním podokně vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="beb08-249">On hello main pane's toolbar, select **Delete**.</span></span>
     3. <span data-ttu-id="beb08-250">Vyberte **Ano** toohello dialogové okno potvrzení.</span><span class="sxs-lookup"><span data-stu-id="beb08-250">Select **Yes** toohello confirmation dialog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="beb08-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="beb08-251">Next steps</span></span>
* <span data-ttu-id="beb08-252">Zobrazení hello [nejnovější poznámky k verzi Storage Explorer (Preview) a videa](http://www.storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="beb08-252">View hello [latest Storage Explorer (Preview) release notes and videos](http://www.storageexplorer.com).</span></span>
* <span data-ttu-id="beb08-253">Zjistěte, jak příliš[vytvářet aplikace, které používají Azure BLOB, tabulek, front a soubory](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="beb08-253">Learn how too[create applications using Azure blobs, tables, queues, and files](https://azure.microsoft.com/documentation/services/storage/).</span></span>

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
