---
title: "Správa prostředků Azure Blob Storage pomocí Storage Exploreru (Preview) | Microsoft Docs"
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
ms.openlocfilehash: f833027203441e12340bd93f3570de073d297223
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a><span data-ttu-id="c4578-103">Správa prostředků Azure Blob Storage pomocí Storage Exploreru (Preview)</span><span class="sxs-lookup"><span data-stu-id="c4578-103">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="c4578-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c4578-104">Overview</span></span>
<span data-ttu-id="c4578-105">[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která jsou přístupná odkudkoli na světě prostřednictvím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c4578-105">[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span>
<span data-ttu-id="c4578-106">Službu Blob Storage můžete používat ke zveřejňování dat pro celý svět, nebo k soukromému ukládání dat aplikací.</span><span class="sxs-lookup"><span data-stu-id="c4578-106">You can use Blob storage to expose data publicly to the world, or to store application data privately.</span></span> <span data-ttu-id="c4578-107">V tomto článku se naučíte používat Storage Explorer (Preview) pro práci s kontejnery objektů blob a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="c4578-107">In this article, you'll learn how to use Storage Explorer (Preview) to work with blob containers and blobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4578-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c4578-108">Prerequisites</span></span>
<span data-ttu-id="c4578-109">K dokončení kroků v tomto článku budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="c4578-109">To complete the steps in this article, you'll need the following:</span></span>

* [<span data-ttu-id="c4578-110">Stažení a instalace Storage Exploreru (Preview)</span><span class="sxs-lookup"><span data-stu-id="c4578-110">Download and install Storage Explorer (preview)</span></span>](http://www.storageexplorer.com)
* [<span data-ttu-id="c4578-111">Připojení k účtu úložiště nebo službě Azure</span><span class="sxs-lookup"><span data-stu-id="c4578-111">Connect to a Azure storage account or service</span></span>](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a><span data-ttu-id="c4578-112">Vytvořte kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="c4578-112">Create a blob container</span></span>
<span data-ttu-id="c4578-113">Všechny objekty BLOB se musí nacházet v kontejneru objektů blob, který je jednoduše logické seskupení objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="c4578-113">All blobs must reside in a blob container, which is simply a logical grouping of blobs.</span></span> <span data-ttu-id="c4578-114">Účet může obsahovat neomezený počet kontejnerů a každý kontejner můžete pojmout neomezený počet objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="c4578-114">An account can contain an unlimited number of containers, and each container can store an unlimited number of blobs.</span></span>

<span data-ttu-id="c4578-115">Následující kroky ukazují, jak vytvořit kontejner objektů blob v rámci Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="c4578-115">The following steps illustrate how to create a blob container within Storage Explorer (Preview).</span></span>

1. <span data-ttu-id="c4578-116">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="c4578-116">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="c4578-117">V levém podokně rozbalte účet úložiště, ve kterém chcete vytvořit kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-117">In the left pane, expand the storage account within which you wish to create the blob container.</span></span>
3. <span data-ttu-id="c4578-118">Klikněte pravým tlačítkem na **kontejnery objektů Blob**a v místní nabídce - vyberte **vytvořit kontejner objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c4578-118">Right-click **Blob Containers**, and - from the context menu - select **Create Blob Container**.</span></span>

   ![Vytvoření objektu blob kontejnery kontextové nabídky][0]
4. <span data-ttu-id="c4578-120">V textovém poli se zobrazí níže **kontejnery objektů Blob** složky.</span><span class="sxs-lookup"><span data-stu-id="c4578-120">A text box will appear below the **Blob Containers** folder.</span></span> <span data-ttu-id="c4578-121">Zadejte název pro váš kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-121">Enter the name for your blob container.</span></span> <span data-ttu-id="c4578-122">Najdete v článku [pravidla pojmenování kontejneru](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) části Seznam pravidel a omezení pro pojmenovávání kontejnerů objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-122">See the [Container naming rules](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) section for a list of rules and restrictions on naming blob containers.</span></span>

   ![Vytvoření textového pole kontejnery objektů Blob][1]
5. <span data-ttu-id="c4578-124">Stiskněte klávesu **Enter** po dokončení vytvoření kontejneru objektů blob nebo **Esc** zrušit.</span><span class="sxs-lookup"><span data-stu-id="c4578-124">Press **Enter** when done to create the blob container, or **Esc** to cancel.</span></span> <span data-ttu-id="c4578-125">Po úspěšném vytvoření kontejneru objektů blob se zobrazí v části **kontejnery objektů Blob** složku pro vybraný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4578-125">Once the blob container has been successfully created, it will be displayed under the **Blob Containers** folder for the selected storage account.</span></span>

   ![Vytvořit kontejner objektů BLOB][2]

## <a name="view-a-blob-containers-contents"></a><span data-ttu-id="c4578-127">Zobrazit obsah kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="c4578-127">View a blob container's contents</span></span>
<span data-ttu-id="c4578-128">Kontejnery objektů BLOB obsahovat objekty BLOB a složky (které mohou obsahovat také objekty BLOB).</span><span class="sxs-lookup"><span data-stu-id="c4578-128">Blob containers contain blobs and folders (that can also contain blobs).</span></span>

<span data-ttu-id="c4578-129">Následující kroky ukazují, jak zobrazit obsah kontejneru objektů blob v rámci Storage Explorer (Preview):</span><span class="sxs-lookup"><span data-stu-id="c4578-129">The following steps illustrate how to view the contents of a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="c4578-130">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="c4578-130">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="c4578-131">V levém podokně rozbalte kontejner objektů blob obsahující účet úložiště, že které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="c4578-131">In the left pane, expand the storage account containing the blob container you wish to view.</span></span>
3. <span data-ttu-id="c4578-132">Rozbalte účet úložiště **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c4578-132">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="c4578-133">Klikněte pravým tlačítkem na kontejner objektů blob, které chcete zobrazit a v místní nabídce - vyberte **otevřete Editor kontejner objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c4578-133">Right-click the blob container you wish to view, and - from the context menu - select **Open Blob Container Editor**.</span></span>
   <span data-ttu-id="c4578-134">Dvakrát klikněte na kontejner objektů blob, který si přejete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="c4578-134">You can also double-click the blob container you wish to view.</span></span>

   ![Otevřete blob kontejneru editor kontextové nabídky][19]
5. <span data-ttu-id="c4578-136">V hlavním podokně se zobrazí obsah kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-136">The main pane will display the blob container's contents.</span></span>

   ![Editor kontejner objektů BLOB][3]

## <a name="delete-a-blob-container"></a><span data-ttu-id="c4578-138">Odstranit kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="c4578-138">Delete a blob container</span></span>
<span data-ttu-id="c4578-139">Kontejnery objektů BLOB můžete snadno vytvořit a odstraněn podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="c4578-139">Blob containers can be easily created and deleted as needed.</span></span> <span data-ttu-id="c4578-140">(Chcete-li zjistit, jak odstranit jednotlivé objekty BLOB, informace naleznete v sekci [Správa objektů BLOB v kontejneru objektů blob](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="c4578-140">(To see how to delete individual blobs, refer to the section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="c4578-141">Následující kroky ukazují, jak odstranit kontejner objektů blob v rámci Storage Explorer (Preview):</span><span class="sxs-lookup"><span data-stu-id="c4578-141">The following steps illustrate how to delete a blob container within Storage Explorer (Preview):</span></span>

1. <span data-ttu-id="c4578-142">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="c4578-142">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="c4578-143">V levém podokně rozbalte kontejner objektů blob obsahující účet úložiště, že které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="c4578-143">In the left pane, expand the storage account containing the blob container you wish to view.</span></span>
3. <span data-ttu-id="c4578-144">Rozbalte účet úložiště **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c4578-144">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="c4578-145">Klikněte pravým tlačítkem na kontejner objektů blob, které chcete odstranit a v místní nabídce - vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c4578-145">Right-click the blob container you wish to delete, and - from the context menu - select **Delete**.</span></span>
   <span data-ttu-id="c4578-146">Můžete také stisknout **odstranit** odstranit kontejner aktuálně vybraných objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-146">You can also press **Delete** to delete the currently selected blob container.</span></span>

   ![Odstranit objekt blob kontejneru kontextové nabídky][4]
5. <span data-ttu-id="c4578-148">V potvrzovacím dialogovém okně klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="c4578-148">Select **Yes** to the confirmation dialog.</span></span>

   ![Odstranit objekt blob kontejneru potvrzení][5]

## <a name="copy-a-blob-container"></a><span data-ttu-id="c4578-150">Zkopírujte kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="c4578-150">Copy a blob container</span></span>
<span data-ttu-id="c4578-151">Storage Explorer (Preview) umožňuje kopírovat kontejner objektů blob do schránky a vložte tento kontejner objektů blob do jiný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4578-151">Storage Explorer (Preview) enables you to copy a blob container to the clipboard, and then paste that blob container into another storage account.</span></span> <span data-ttu-id="c4578-152">(Chcete-li zjistit, jak zkopírovat jednotlivé objekty BLOB, informace naleznete v sekci [Správa objektů BLOB v kontejneru objektů blob](#managing-blobs-in-a-blob-container).)</span><span class="sxs-lookup"><span data-stu-id="c4578-152">(To see how to copy individual blobs, refer to the section, [Managing blobs in a blob container](#managing-blobs-in-a-blob-container).)</span></span>

<span data-ttu-id="c4578-153">Následující kroky ukazují, jak zkopírovat kontejner objektů blob z jeden účet úložiště do druhého.</span><span class="sxs-lookup"><span data-stu-id="c4578-153">The following steps illustrate how to copy a blob container from one storage account to another.</span></span>

1. <span data-ttu-id="c4578-154">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="c4578-154">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="c4578-155">V levém podokně rozbalte kontejner objektů blob obsahující účet úložiště, že které chcete kopírovat.</span><span class="sxs-lookup"><span data-stu-id="c4578-155">In the left pane, expand the storage account containing the blob container you wish to copy.</span></span>
3. <span data-ttu-id="c4578-156">Rozbalte účet úložiště **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c4578-156">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="c4578-157">Klikněte pravým tlačítkem na kontejner objektů blob, které chcete kopírovat a v místní nabídce - vyberte **kontejner objektů Blob kopie**.</span><span class="sxs-lookup"><span data-stu-id="c4578-157">Right-click the blob container you wish to copy, and - from the context menu - select **Copy Blob Container**.</span></span>

   ![Kopírovat objekt blob kontejneru kontextové nabídky][6]
5. <span data-ttu-id="c4578-159">Klikněte pravým tlačítkem na účet úložiště požadované "target", do kterého chcete vložit kontejneru objektů blob a v místní nabídce - vyberte **vložit kontejner objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c4578-159">Right-click the desired "target" storage account into which you want to paste the blob container, and - from the context menu - select **Paste Blob Container**.</span></span>

   ![Vložení objektu blob kontejneru kontextové nabídky][7]

## <a name="get-the-sas-for-a-blob-container"></a><span data-ttu-id="c4578-161">Získání sdíleného přístupového podpisu (SAS) pro kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="c4578-161">Get the SAS for a blob container</span></span>
<span data-ttu-id="c4578-162">[Sdílený přístupový podpis (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) poskytuje delegovaný přístup k prostředkům ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4578-162">A [shared access signature (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) provides delegated access to resources in your storage account.</span></span>
<span data-ttu-id="c4578-163">To znamená, že můžete klientovi udělit omezená oprávnění k objektům ve vašem účtu úložiště po stanovené časové období a s konkrétní sadou oprávnění, aniž byste museli sdílet přístupové klíče vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="c4578-163">This means that you can grant a client limited permissions to objects in your storage account for a specified period of time and with a specified set of permissions, without having to share your account access keys.</span></span>

<span data-ttu-id="c4578-164">Následující kroky ukazují, jak vytvořit SAS pro kontejner objektů blob:</span><span class="sxs-lookup"><span data-stu-id="c4578-164">The following steps illustrate how to create a SAS for a blob container:</span></span>

1. <span data-ttu-id="c4578-165">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="c4578-165">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="c4578-166">V levém podokně rozbalte obsahující kontejner objektů blob, pro kterou chcete získat SAS účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4578-166">In the left pane, expand the storage account containing the blob container for which you wish to get a SAS.</span></span>
3. <span data-ttu-id="c4578-167">Rozbalte účet úložiště **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c4578-167">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="c4578-168">Klikněte pravým tlačítkem na kontejner objektů blob požadované a v místní nabídce - vyberte **sdíleného přístupového podpisu**.</span><span class="sxs-lookup"><span data-stu-id="c4578-168">Right-click the desired blob container, and - from the context menu - select **Get Shared Access Signature**.</span></span>

   ![Získat SAS kontextové nabídky][8]
5. <span data-ttu-id="c4578-170">V dialogovém okně **Sdílený přístupový podpis** zadejte zásadu, počáteční datum a datum vypršení platnosti, časové pásmo a požadované úrovně přístupu k prostředku.</span><span class="sxs-lookup"><span data-stu-id="c4578-170">In the **Shared Access Signature** dialog, specify the policy, start and expiration dates, time zone, and access levels you want for the resource.</span></span>

   ![Získat možnosti SAS][9]
6. <span data-ttu-id="c4578-172">Jakmile budete hotovi se zadáváním možností sdíleného přístupového podpisu, vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c4578-172">When you're finished specifying the SAS options, select **Create**.</span></span>
7. <span data-ttu-id="c4578-173">Druhý **sdíleného přístupového podpisu** dialogové okno se zobrazí, pak se seznamem kontejneru objektů blob společně s adresu URL a QueryStrings můžete použít pro přístup k prostředku úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4578-173">A second **Shared Access Signature** dialog will then display that lists the blob container along with the URL and QueryStrings you can use to access the storage resource.</span></span>
   <span data-ttu-id="c4578-174">Vyberte **Kopírovat** vedle adresy URL, kterou chcete zkopírovat do schránky.</span><span class="sxs-lookup"><span data-stu-id="c4578-174">Select **Copy** next to the URL you wish to copy to the clipboard.</span></span>

   ![Zkopírování adresy URL SAS][10]
8. <span data-ttu-id="c4578-176">Až budete hotovi, vyberte **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="c4578-176">When done, select **Close**.</span></span>

## <a name="manage-access-policies-for-a-blob-container"></a><span data-ttu-id="c4578-177">Správa zásad přístupu pro kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="c4578-177">Manage Access Policies for a blob container</span></span>
<span data-ttu-id="c4578-178">Následující kroky ukazují, jak spravovat (přidání a odebrání) získat přístup k zásadám pro kontejner objektů blob:</span><span class="sxs-lookup"><span data-stu-id="c4578-178">The following steps illustrate how to manage (add and remove) access policies for a blob container:</span></span>

1. <span data-ttu-id="c4578-179">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="c4578-179">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="c4578-180">V levém podokně rozbalte účet úložiště obsahující kontejner objektů blob, jejichž zásady přístupu, které chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="c4578-180">In the left pane, expand the storage account containing the blob container whose access policies you wish to manage.</span></span>
3. <span data-ttu-id="c4578-181">Rozbalte účet úložiště **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c4578-181">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="c4578-182">Vyberte kontejner objektů blob požadované a v místní nabídce - vyberte **spravovat zásady přístupu**.</span><span class="sxs-lookup"><span data-stu-id="c4578-182">Select the desired blob container, and - from the context menu - select **Manage Access Policies**.</span></span>

   ![Místní nabídka Spravovat zásady přístupu][11]
5. <span data-ttu-id="c4578-184">**Zásady přístupu** dialogové okno se zobrazí seznam všechny zásady přístupu, které jsou vytvořeny již pro vybraný objekt blob kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c4578-184">The **Access Policies** dialog will list any access policies already created for the selected blob container.</span></span>

   ![Možnosti zásad přístupu][12]        
6. <span data-ttu-id="c4578-186">V závislosti na úloze správy zásad přístupu postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="c4578-186">Follow these steps depending on the access policy management task:</span></span>

   * <span data-ttu-id="c4578-187">**Přidání nové zásady přístupu:** Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="c4578-187">**Add a new access policy** - Select **Add**.</span></span> <span data-ttu-id="c4578-188">Po vygenerování se nová zásada přístupu (s výchozím nastavením) zobrazí v dialogovém okně **Zásady přístupu**.</span><span class="sxs-lookup"><span data-stu-id="c4578-188">Once generated, the **Access Policies** dialog will display the newly added access policy (with default settings).</span></span>
   * <span data-ttu-id="c4578-189">**Upravit zásady přístupu** – proveďte veškeré požadované úpravy a vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c4578-189">**Edit an access policy** -  Make any desired edits, and select **Save**.</span></span>
   * <span data-ttu-id="c4578-190">**Odebrání zásady přístupu:** Vyberte **Odebrat** vedle zásady přístupu, kterou chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="c4578-190">**Remove an access policy** - Select **Remove** next to the access policy you wish to remove.</span></span>

## <a name="set-the-public-access-level-for-a-blob-container"></a><span data-ttu-id="c4578-191">Nastavení úrovně veřejný přístup pro kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="c4578-191">Set the Public Access Level for a blob container</span></span>
<span data-ttu-id="c4578-192">Ve výchozím nastavení každý kontejner objektů blob nastavena na "Žádný veřejný přístup".</span><span class="sxs-lookup"><span data-stu-id="c4578-192">By default, every blob container is set to "No public access".</span></span>

<span data-ttu-id="c4578-193">Následující kroky ukazují, jak k určení úrovně veřejný přístup pro kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-193">The following steps illustrate how to specify a public access level for a blob container.</span></span>

1. <span data-ttu-id="c4578-194">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="c4578-194">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="c4578-195">V levém podokně rozbalte účet úložiště obsahující kontejner objektů blob, jejichž zásady přístupu, které chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="c4578-195">In the left pane, expand the storage account containing the blob container whose access policies you wish to manage.</span></span>
3. <span data-ttu-id="c4578-196">Rozbalte účet úložiště **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c4578-196">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="c4578-197">Vyberte kontejner objektů blob požadované a v místní nabídce - vyberte **nastavit úroveň veřejný přístup**.</span><span class="sxs-lookup"><span data-stu-id="c4578-197">Select the desired blob container, and - from the context menu - select **Set Public Access Level**.</span></span>

   ![Nastavení úrovně kontextovou nabídku veřejného přístupu][13]
5. <span data-ttu-id="c4578-199">V **nastavit úroveň veřejný přístup kontejneru** dialogové okno, zadejte úroveň požadované přístupu.</span><span class="sxs-lookup"><span data-stu-id="c4578-199">In the **Set Container Public Access Level** dialog, specify the desired access level.</span></span>

   ![Nastavení možností úrovně veřejného přístupu][14]
6. <span data-ttu-id="c4578-201">Vyberte **Použít**.</span><span class="sxs-lookup"><span data-stu-id="c4578-201">Select **Apply**.</span></span>

## <a name="managing-blobs-in-a-blob-container"></a><span data-ttu-id="c4578-202">Správa objektů BLOB v kontejneru objektů blob</span><span class="sxs-lookup"><span data-stu-id="c4578-202">Managing blobs in a blob container</span></span>
<span data-ttu-id="c4578-203">Po vytvoření kontejneru objektů blob, můžete nahrát objekt blob kontejneru objektů blob, stáhne objekt blob do místního počítače, otevřete objekt blob na místním počítači a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="c4578-203">Once you've created a blob container, you can upload a blob to that blob container, download a blob to your local computer, open a blob on your local computer, and much more.</span></span>

<span data-ttu-id="c4578-204">Následující kroky ukazují, jak spravovat objekty BLOB (a složky) v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-204">The following steps illustrate how to manage the blobs (and folders) within a blob container.</span></span>

1. <span data-ttu-id="c4578-205">Otevřete Storage Explorer (Preview).</span><span class="sxs-lookup"><span data-stu-id="c4578-205">Open Storage Explorer (Preview).</span></span>
2. <span data-ttu-id="c4578-206">V levém podokně rozbalte kontejner objektů blob obsahující účet úložiště, že které chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="c4578-206">In the left pane, expand the storage account containing the blob container you wish to manage.</span></span>
3. <span data-ttu-id="c4578-207">Rozbalte účet úložiště **kontejnery objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c4578-207">Expand the storage account's **Blob Containers**.</span></span>
4. <span data-ttu-id="c4578-208">Dvakrát klikněte na kontejner objektů blob, který si přejete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="c4578-208">Double-click the blob container you wish to view.</span></span>
5. <span data-ttu-id="c4578-209">V hlavním podokně se zobrazí obsah kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-209">The main pane will display the blob container's contents.</span></span>

   ![Kontejner objektů blob zobrazení][3]
6. <span data-ttu-id="c4578-211">V hlavním podokně se zobrazí obsah kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-211">The main pane will display the blob container's contents.</span></span>
7. <span data-ttu-id="c4578-212">V závislosti na úloze, kterou chcete provést, postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="c4578-212">Follow these steps depending on the task you wish to perform:</span></span>

   * <span data-ttu-id="c4578-213">**Nahrání souborů do kontejneru objektů blob**</span><span class="sxs-lookup"><span data-stu-id="c4578-213">**Upload files to a blob container**</span></span>

     1. <span data-ttu-id="c4578-214">Na panelu nástrojů v hlavním podokně vyberte **Nahrát** a pak v rozevírací nabídce vyberte **Nahrát soubory**.</span><span class="sxs-lookup"><span data-stu-id="c4578-214">On the main pane's toolbar, select **Upload**, and then **Upload Files** from the drop-down menu.</span></span>

        ![Nahrát soubory nabídky][15]
     2. <span data-ttu-id="c4578-216">V dialogovém okně **Nahrát soubory** vyberte tlačítko se třemi tečkami (**...**) na pravé straně textového pole **Soubory** a vyberte soubory, které chcete nahrát.</span><span class="sxs-lookup"><span data-stu-id="c4578-216">In the **Upload files** dialog, select the ellipsis (**…**) button on the right side of the **Files** text box to select the file(s) you wish to upload.</span></span>

        ![Nahrávání souborů možnosti][16]
     3. <span data-ttu-id="c4578-218">Zadejte typ **Blob typu**.</span><span class="sxs-lookup"><span data-stu-id="c4578-218">Specify the type of **Blob type**.</span></span> <span data-ttu-id="c4578-219">Článek [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) vysvětluje rozdíly mezi různé typy objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-219">The article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains the differences between the various blob types.</span></span>
     4. <span data-ttu-id="c4578-220">Volitelně zadejte cílovou složku, do kterého se nahraje vybrané soubory.</span><span class="sxs-lookup"><span data-stu-id="c4578-220">Optionally, specify a target folder into which the selected file(s) will be uploaded.</span></span> <span data-ttu-id="c4578-221">Pokud cílová složka neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="c4578-221">If the target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="c4578-222">Vyberte **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="c4578-222">Select **Upload**.</span></span>
   * <span data-ttu-id="c4578-223">**Nahrajte do složky na kontejner objektů blob**</span><span class="sxs-lookup"><span data-stu-id="c4578-223">**Upload a folder to a blob container**</span></span>

     1. <span data-ttu-id="c4578-224">Na panelu nástrojů v hlavním podokně vyberte **Nahrát** a pak v rozevírací nabídce vyberte **Nahrát složku**.</span><span class="sxs-lookup"><span data-stu-id="c4578-224">On the main pane's toolbar, select **Upload**, and then **Upload Folder** from the drop-down menu.</span></span>

        ![Nabídka Nahrát složku][17]
     2. <span data-ttu-id="c4578-226">V dialogovém okně **Nahrát složku** vyberte tlačítko se třemi tečkami (**...**) na pravé straně textového pole **Složka** a vyberte složku, jejíž obsah chcete nahrát.</span><span class="sxs-lookup"><span data-stu-id="c4578-226">In the **Upload folder** dialog, select the ellipsis (**…**) button on the right side of the **Folder** text box to select the folder whose contents you wish to upload.</span></span>

        ![Nahrát Možnosti složky][18]
     3. <span data-ttu-id="c4578-228">Zadejte typ **Blob typu**.</span><span class="sxs-lookup"><span data-stu-id="c4578-228">Specify the type of **Blob type**.</span></span> <span data-ttu-id="c4578-229">Článek [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) vysvětluje rozdíly mezi různé typy objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-229">The article [Get started with Azure Blob storage using .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) explains the differences between the various blob types.</span></span>
     4. <span data-ttu-id="c4578-230">Volitelně můžete zadat cílovou složku, do které se obsah vybrané složky nahraje.</span><span class="sxs-lookup"><span data-stu-id="c4578-230">Optionally, specify a target folder into which the selected folder's contents will be uploaded.</span></span> <span data-ttu-id="c4578-231">Pokud cílová složka neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="c4578-231">If the target folder doesn’t exist, it will be created.</span></span>
     5. <span data-ttu-id="c4578-232">Vyberte **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="c4578-232">Select **Upload**.</span></span>
   * <span data-ttu-id="c4578-233">**Stáhnout objekt blob do místního počítače**</span><span class="sxs-lookup"><span data-stu-id="c4578-233">**Download a blob to your local computer**</span></span>

     1. <span data-ttu-id="c4578-234">Vyberte objekt blob, který chcete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="c4578-234">Select the blob you wish to download.</span></span>
     2. <span data-ttu-id="c4578-235">Na panelu nástrojů v hlavním podokně vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="c4578-235">On the main pane's toolbar, select **Download**.</span></span>
     3. <span data-ttu-id="c4578-236">V **určení místa k uložení stažených blob** dialogové okno, zadejte umístění, kam má objekt blob stáhli a název chcete pojmenujte ho.</span><span class="sxs-lookup"><span data-stu-id="c4578-236">In the **Specify where to save the downloaded blob** dialog, specify the location where you want the blob downloaded, and the name you wish to give it.</span></span>  
     4. <span data-ttu-id="c4578-237">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c4578-237">Select **Save**.</span></span>
   * <span data-ttu-id="c4578-238">**Otevřete objekt blob do místního počítače**</span><span class="sxs-lookup"><span data-stu-id="c4578-238">**Open a blob on your local computer**</span></span>

     1. <span data-ttu-id="c4578-239">Vyberte objekt blob, který chcete otevřít.</span><span class="sxs-lookup"><span data-stu-id="c4578-239">Select the blob you wish to open.</span></span>
     2. <span data-ttu-id="c4578-240">Na panelu nástrojů v hlavním podokně vyberte **Otevřít**.</span><span class="sxs-lookup"><span data-stu-id="c4578-240">On the main pane's toolbar, select **Open**.</span></span>
     3. <span data-ttu-id="c4578-241">Objekt blob se stáhne a otevřít pomocí aplikace přidružené k objektu blob základní typ souboru.</span><span class="sxs-lookup"><span data-stu-id="c4578-241">The blob will be downloaded and opened using the application associated with the blob's underlying file type.</span></span>
   * <span data-ttu-id="c4578-242">**Kopírovat objekt blob do schránky.**</span><span class="sxs-lookup"><span data-stu-id="c4578-242">**Copy a blob to the clipboard**</span></span>

     1. <span data-ttu-id="c4578-243">Vyberte objekt blob, který chcete kopírovat.</span><span class="sxs-lookup"><span data-stu-id="c4578-243">Select the blob you wish to copy.</span></span>
     2. <span data-ttu-id="c4578-244">Na panelu nástrojů v hlavním podokně vyberte **Kopírovat**.</span><span class="sxs-lookup"><span data-stu-id="c4578-244">On the main pane's toolbar, select **Copy**.</span></span>
     3. <span data-ttu-id="c4578-245">V levém podokně přejděte na jiný kontejner objektů blob a dvojím kliknutím zobrazíte v hlavním podokně.</span><span class="sxs-lookup"><span data-stu-id="c4578-245">In the left pane, navigate to another blob container, and double-click it to view it in the main pane.</span></span>
     4. <span data-ttu-id="c4578-246">Na panelu nástrojů v hlavním podokně vyberte **vložení** vytvoření kopie objektu blob.</span><span class="sxs-lookup"><span data-stu-id="c4578-246">On the main pane's toolbar, select **Paste** to create a copy of the blob.</span></span>
   * <span data-ttu-id="c4578-247">**Odstranit objekt blob**</span><span class="sxs-lookup"><span data-stu-id="c4578-247">**Delete a blob**</span></span>

     1. <span data-ttu-id="c4578-248">Vyberte objekt blob, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="c4578-248">Select the blob you wish to delete.</span></span>
     2. <span data-ttu-id="c4578-249">Na panelu nástrojů v hlavním podokně vyberte **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c4578-249">On the main pane's toolbar, select **Delete**.</span></span>
     3. <span data-ttu-id="c4578-250">V potvrzovacím dialogovém okně klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="c4578-250">Select **Yes** to the confirmation dialog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4578-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4578-251">Next steps</span></span>
* <span data-ttu-id="c4578-252">Podívejte se na [nejnovější poznámky k verzi a videa pro Storage Explorer (Preview)](http://www.storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="c4578-252">View the [latest Storage Explorer (Preview) release notes and videos](http://www.storageexplorer.com).</span></span>
* <span data-ttu-id="c4578-253">Zjistěte, jak [vytvářet aplikace pomocí objektů blob, tabulek, dotazů a souborů Azure](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="c4578-253">Learn how to [create applications using Azure blobs, tables, queues, and files](https://azure.microsoft.com/documentation/services/storage/).</span></span>

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
