---
title: "Povolit veřejný přístup pro čtení pro kontejnery a objekty BLOB v Azure Blob storage | Microsoft Docs"
description: "Zjistěte, jak zpřístupnit kontejnery a objekty BLOB pro anonymní přístup a jak přistupovat k nim prostřednictvím kódu programu."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a2cffee6-3224-4f2a-8183-66ca23b2d2d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: marsma
ms.openlocfilehash: 8d4f4c7c208baf0db6155eb78a53e37c4ec1e023
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a><span data-ttu-id="b1ea0-103">Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob</span><span class="sxs-lookup"><span data-stu-id="b1ea0-103">Manage anonymous read access to containers and blobs</span></span>
<span data-ttu-id="b1ea0-104">Můžete povolit anonymní, veřejný přístup pro čtení kontejner a jeho objekty BLOB v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-104">You can enable anonymous, public read access to a container and its blobs in Azure Blob storage.</span></span> <span data-ttu-id="b1ea0-105">Díky tomu můžete udělit přístup jen pro čtení k těmto prostředkům bez sdílení klíč účtu a bez nutnosti sdílený přístupový podpis (SAS).</span><span class="sxs-lookup"><span data-stu-id="b1ea0-105">By doing so, you can grant read-only access to these resources without sharing your account key, and without requiring a shared access signature (SAS).</span></span>

<span data-ttu-id="b1ea0-106">Veřejný přístup pro čtení je nejvhodnější pro scénáře, kde chcete určité objekty BLOB vždy k dispozici pro anonymní přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-106">Public read access is best for scenarios where you want certain blobs to always be available for anonymous read access.</span></span> <span data-ttu-id="b1ea0-107">Pro větší kontrolu můžete vytvořit sdílený přístupový podpis.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-107">For more fine-grained control, you can create a shared access signature.</span></span> <span data-ttu-id="b1ea0-108">Sdílené přístupové podpisy umožňují zadat omezený přístup pomocí různých oprávnění pro konkrétní časové období.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-108">Shared access signatures enable you to provide restricted access using different permissions, for a specific time period.</span></span> <span data-ttu-id="b1ea0-109">Další informace o vytváření sdílený přístup podpisy, najdete v části [pomocí sdílené přístupové podpisy (SAS) ve službě Azure Storage](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b1ea0-109">For more information about creating shared access signatures, see [Using shared access signatures (SAS) in Azure Storage](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span></span>

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a><span data-ttu-id="b1ea0-110">Udělte anonymním uživatelům oprávnění ke kontejnerům a objektům BLOB</span><span class="sxs-lookup"><span data-stu-id="b1ea0-110">Grant anonymous users permissions to containers and blobs</span></span>
<span data-ttu-id="b1ea0-111">Ve výchozím kontejneru a všechny objekty BLOB v něm může být přístupné jenom vlastník účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-111">By default, a container and any blobs within it may be accessed only by the owner of the storage account.</span></span> <span data-ttu-id="b1ea0-112">Umožnit anonymní uživatelé oprávnění ke čtení pro kontejner a jeho objekty BLOB, můžete nastavit kontejner oprávnění, která umožní veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-112">To give anonymous users read permissions to a container and its blobs, you can set the container permissions to allow public access.</span></span> <span data-ttu-id="b1ea0-113">Anonymní uživatelé mohou číst objekty BLOB do kontejneru, veřejně přístupný bez ověřování žádosti.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-113">Anonymous users can read blobs within a publicly accessible container without authenticating the request.</span></span>

<span data-ttu-id="b1ea0-114">Kontejner můžete nakonfigurovat s následujícími oprávněními:</span><span class="sxs-lookup"><span data-stu-id="b1ea0-114">You can configure a container with the following permissions:</span></span>

* <span data-ttu-id="b1ea0-115">**Žádný veřejný přístup pro čtení:** kontejner a jeho objekty BLOB je přístupný jenom vlastník účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-115">**No public read access:** The container and its blobs can be accessed only by the storage account owner.</span></span> <span data-ttu-id="b1ea0-116">Toto je výchozí nastavení pro všechny nové kontejnery.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-116">This is the default for all new containers.</span></span>
* <span data-ttu-id="b1ea0-117">**Veřejný přístup pro objekty BLOB pouze pro čtení:** anonymní žádosti může číst objekty BLOB v kontejneru, ale kontejneru data nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-117">**Public read access for blobs only:** Blobs within the container can be read by anonymous request, but container data is not available.</span></span> <span data-ttu-id="b1ea0-118">Anonymní klienti nemůže vytvořit výčet objektů BLOB v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-118">Anonymous clients cannot enumerate the blobs within the container.</span></span>
* <span data-ttu-id="b1ea0-119">**Úplný veřejný přístup pro čtení:** všechna data kontejnerů a objektů blob mohou být přečteny anonymní žádost.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-119">**Full public read access:** All container and blob data can be read by anonymous request.</span></span> <span data-ttu-id="b1ea0-120">Klienti mohou anonymní žádosti o výčet objektů BLOB v kontejneru, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-120">Clients can enumerate blobs within the container by anonymous request, but cannot enumerate containers within the storage account.</span></span>

<span data-ttu-id="b1ea0-121">Nastavit oprávnění kontejneru, můžete použít následující:</span><span class="sxs-lookup"><span data-stu-id="b1ea0-121">You can use the following to set container permissions:</span></span>

* [<span data-ttu-id="b1ea0-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b1ea0-122">Azure portal</span></span>](https://portal.azure.com)
* [<span data-ttu-id="b1ea0-123">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b1ea0-123">Azure PowerShell</span></span>](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#how-to-manage-azure-blobs)
* [<span data-ttu-id="b1ea0-124">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b1ea0-124">Azure CLI 2.0</span></span>](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* <span data-ttu-id="b1ea0-125">Programově pomocí jedné z knihovny klienta úložiště nebo REST API</span><span class="sxs-lookup"><span data-stu-id="b1ea0-125">Programmatically, by using one of the storage client libraries or the REST API</span></span>

### <a name="set-container-permissions-in-the-azure-portal"></a><span data-ttu-id="b1ea0-126">Nastavte oprávnění kontejneru na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b1ea0-126">Set container permissions in the Azure portal</span></span>
<span data-ttu-id="b1ea0-127">Nastavit oprávnění kontejneru [portál Azure](https://portal.azure.com), postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b1ea0-127">To set container permissions in the [Azure portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="b1ea0-128">Otevřete váš **účet úložiště** okno na portálu.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-128">Open your **Storage account** blade in the portal.</span></span> <span data-ttu-id="b1ea0-129">Váš účet úložiště můžete najít tak, že vyberete **účty úložiště** v okně portálu přejděte z hlavní nabídky.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-129">You can find your storage account by selecting **Storage accounts** in the main portal menu blade.</span></span>
1. <span data-ttu-id="b1ea0-130">V části **služby objektů BLOB** v okně nabídky vyberte **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-130">Under **BLOB SERVICE** on the menu blade, select **Containers**.</span></span>
1. <span data-ttu-id="b1ea0-131">Klikněte pravým tlačítkem na kontejner řádek nebo vyberte se třemi tečkami otevřete kontejneru **kontextovou nabídku**.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-131">Right-click on the container row or select the ellipsis to open the container's **Context menu**.</span></span>
1. <span data-ttu-id="b1ea0-132">Vyberte **zásada přístupu** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-132">Select **Access policy** in the context menu.</span></span>
1. <span data-ttu-id="b1ea0-133">Vyberte **přistupovat typu** z rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-133">Select an **Access type** from the drop down menu.</span></span>

    ![Metadata kontejneru dialogové okno Upravit](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a><span data-ttu-id="b1ea0-135">Nastavte oprávnění kontejner s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="b1ea0-135">Set container permissions with .NET</span></span>
<span data-ttu-id="b1ea0-136">Nastavení oprávnění pro kontejner pomocí C# a knihovnu klienta služby Storage pro .NET, nejdřív načtěte stávající oprávnění kontejneru voláním **GetPermissions** metoda.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-136">To set permissions for a container using C# and the Storage Client Library for .NET, first retrieve the container's existing permissions by calling the **GetPermissions** method.</span></span> <span data-ttu-id="b1ea0-137">Nastavte **PublicAccess** vlastnost **BlobContainerPermissions** objekt, který je vrácen **GetPermissions** metoda.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-137">Then set the **PublicAccess** property for the **BlobContainerPermissions** object that is returned by the **GetPermissions** method.</span></span> <span data-ttu-id="b1ea0-138">Nakonec zavolejte **měli** metoda aktualizované oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-138">Finally, call the **SetPermissions** method with the updated permissions.</span></span>

<span data-ttu-id="b1ea0-139">Následující příklad nastaví kontejneru oprávnění pro úplné veřejný přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-139">The following example sets the container's permissions to full public read access.</span></span> <span data-ttu-id="b1ea0-140">Chcete-li nastavit oprávnění pro veřejný přístup pro čtení pro pouze objekty BLOB, nastavte **PublicAccess** vlastnost **BlobContainerPublicAccessType.Blob**.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-140">To set permissions to public read access for blobs only, set the **PublicAccess** property to **BlobContainerPublicAccessType.Blob**.</span></span> <span data-ttu-id="b1ea0-141">Chcete-li odebrat všechna oprávnění pro anonymní uživatele, nastavte vlastnost na **BlobContainerPublicAccessType.Off**.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-141">To remove all permissions for anonymous users, set the property to **BlobContainerPublicAccessType.Off**.</span></span>

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a><span data-ttu-id="b1ea0-142">Přístup k kontejnery a objekty BLOB anonymně</span><span class="sxs-lookup"><span data-stu-id="b1ea0-142">Access containers and blobs anonymously</span></span>
<span data-ttu-id="b1ea0-143">Konstruktory, které nevyžadují přihlašovací údaje můžete použít klienta, který přistupuje ke kontejnerům a objektům BLOB anonymně.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-143">A client that accesses containers and blobs anonymously can use constructors that do not require credentials.</span></span> <span data-ttu-id="b1ea0-144">Následující příklady ukazují několik různých způsobů anonymně odkazovat na prostředky služby objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-144">The following examples show a few different ways to reference Blob service resources anonymously.</span></span>

### <a name="create-an-anonymous-client-object"></a><span data-ttu-id="b1ea0-145">Vytvoření objektu anonymním klientem</span><span class="sxs-lookup"><span data-stu-id="b1ea0-145">Create an anonymous client object</span></span>
<span data-ttu-id="b1ea0-146">Můžete vytvořit nový objekt klienta služby pro anonymní přístup tím, že poskytuje koncový bod služby objektů Blob pro účet.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-146">You can create a new service client object for anonymous access by providing the Blob service endpoint for the account.</span></span> <span data-ttu-id="b1ea0-147">Můžete však také musíte znát název kontejneru v daném účtu, který je k dispozici pro anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-147">However, you must also know the name of a container in that account that's available for anonymous access.</span></span>

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a><span data-ttu-id="b1ea0-148">Odkaz kontejner anonymně</span><span class="sxs-lookup"><span data-stu-id="b1ea0-148">Reference a container anonymously</span></span>
<span data-ttu-id="b1ea0-149">Pokud máte adresu URL do kontejneru, který je anonymně k dispozici, můžete tak, aby odkazovaly kontejneru přímo.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-149">If you have the URL to a container that is anonymously available, you can use it to reference the container directly.</span></span>

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a><span data-ttu-id="b1ea0-150">Referenční objekt blob anonymně</span><span class="sxs-lookup"><span data-stu-id="b1ea0-150">Reference a blob anonymously</span></span>
<span data-ttu-id="b1ea0-151">Pokud máte adresu URL do objektu blob, které jsou dostupné pro anonymní přístup, můžete odkazovat přímo pomocí této adresy URL objektu blob:</span><span class="sxs-lookup"><span data-stu-id="b1ea0-151">If you have the URL to a blob that is available for anonymous access, you can reference the blob directly using that URL:</span></span>

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a><span data-ttu-id="b1ea0-152">Funkce, které jsou dostupné pro anonymní uživatele</span><span class="sxs-lookup"><span data-stu-id="b1ea0-152">Features available to anonymous users</span></span>
<span data-ttu-id="b1ea0-153">Následující tabulka uvádí operací, které může být volána anonymní uživatelé při ACL kontejneru je nastavená na veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="b1ea0-153">The following table shows which operations may be called by anonymous users when a container's ACL is set to allow public access.</span></span>

| <span data-ttu-id="b1ea0-154">Operace REST</span><span class="sxs-lookup"><span data-stu-id="b1ea0-154">REST Operation</span></span> | <span data-ttu-id="b1ea0-155">Oprávnění Úplné veřejný přístup pro čtení</span><span class="sxs-lookup"><span data-stu-id="b1ea0-155">Permission with full public read access</span></span> | <span data-ttu-id="b1ea0-156">Oprávnění s veřejný přístup pro čtení pro pouze objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="b1ea0-156">Permission with public read access for blobs only</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b1ea0-157">Seznam kontejnery</span><span class="sxs-lookup"><span data-stu-id="b1ea0-157">List Containers</span></span> |<span data-ttu-id="b1ea0-158">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-158">Owner only</span></span> |<span data-ttu-id="b1ea0-159">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-159">Owner only</span></span> |
| <span data-ttu-id="b1ea0-160">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="b1ea0-160">Create Container</span></span> |<span data-ttu-id="b1ea0-161">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-161">Owner only</span></span> |<span data-ttu-id="b1ea0-162">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-162">Owner only</span></span> |
| <span data-ttu-id="b1ea0-163">Získat vlastnosti kontejneru</span><span class="sxs-lookup"><span data-stu-id="b1ea0-163">Get Container Properties</span></span> |<span data-ttu-id="b1ea0-164">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-164">All</span></span> |<span data-ttu-id="b1ea0-165">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-165">Owner only</span></span> |
| <span data-ttu-id="b1ea0-166">Získat Metadata kontejneru</span><span class="sxs-lookup"><span data-stu-id="b1ea0-166">Get Container Metadata</span></span> |<span data-ttu-id="b1ea0-167">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-167">All</span></span> |<span data-ttu-id="b1ea0-168">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-168">Owner only</span></span> |
| <span data-ttu-id="b1ea0-169">Nastavit Metadata kontejneru</span><span class="sxs-lookup"><span data-stu-id="b1ea0-169">Set Container Metadata</span></span> |<span data-ttu-id="b1ea0-170">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-170">Owner only</span></span> |<span data-ttu-id="b1ea0-171">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-171">Owner only</span></span> |
| <span data-ttu-id="b1ea0-172">Získání kontejneru seznamu ACL</span><span class="sxs-lookup"><span data-stu-id="b1ea0-172">Get Container ACL</span></span> |<span data-ttu-id="b1ea0-173">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-173">Owner only</span></span> |<span data-ttu-id="b1ea0-174">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-174">Owner only</span></span> |
| <span data-ttu-id="b1ea0-175">Nastavit kontejneru seznamu ACL</span><span class="sxs-lookup"><span data-stu-id="b1ea0-175">Set Container ACL</span></span> |<span data-ttu-id="b1ea0-176">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-176">Owner only</span></span> |<span data-ttu-id="b1ea0-177">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-177">Owner only</span></span> |
| <span data-ttu-id="b1ea0-178">Odstranit kontejner</span><span class="sxs-lookup"><span data-stu-id="b1ea0-178">Delete Container</span></span> |<span data-ttu-id="b1ea0-179">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-179">Owner only</span></span> |<span data-ttu-id="b1ea0-180">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-180">Owner only</span></span> |
| <span data-ttu-id="b1ea0-181">Seznam objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="b1ea0-181">List Blobs</span></span> |<span data-ttu-id="b1ea0-182">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-182">All</span></span> |<span data-ttu-id="b1ea0-183">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-183">Owner only</span></span> |
| <span data-ttu-id="b1ea0-184">Uveďte objektů Blob</span><span class="sxs-lookup"><span data-stu-id="b1ea0-184">Put Blob</span></span> |<span data-ttu-id="b1ea0-185">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-185">Owner only</span></span> |<span data-ttu-id="b1ea0-186">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-186">Owner only</span></span> |
| <span data-ttu-id="b1ea0-187">Získání objektu Blob</span><span class="sxs-lookup"><span data-stu-id="b1ea0-187">Get Blob</span></span> |<span data-ttu-id="b1ea0-188">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-188">All</span></span> |<span data-ttu-id="b1ea0-189">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-189">All</span></span> |
| <span data-ttu-id="b1ea0-190">Získat vlastnosti objektu Blob</span><span class="sxs-lookup"><span data-stu-id="b1ea0-190">Get Blob Properties</span></span> |<span data-ttu-id="b1ea0-191">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-191">All</span></span> |<span data-ttu-id="b1ea0-192">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-192">All</span></span> |
| <span data-ttu-id="b1ea0-193">Nastavit vlastnosti objektů Blob</span><span class="sxs-lookup"><span data-stu-id="b1ea0-193">Set Blob Properties</span></span> |<span data-ttu-id="b1ea0-194">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-194">Owner only</span></span> |<span data-ttu-id="b1ea0-195">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-195">Owner only</span></span> |
| <span data-ttu-id="b1ea0-196">Získat Metadata objektu Blob</span><span class="sxs-lookup"><span data-stu-id="b1ea0-196">Get Blob Metadata</span></span> |<span data-ttu-id="b1ea0-197">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-197">All</span></span> |<span data-ttu-id="b1ea0-198">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-198">All</span></span> |
| <span data-ttu-id="b1ea0-199">Nastavit Metadata objektu Blob</span><span class="sxs-lookup"><span data-stu-id="b1ea0-199">Set Blob Metadata</span></span> |<span data-ttu-id="b1ea0-200">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-200">Owner only</span></span> |<span data-ttu-id="b1ea0-201">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-201">Owner only</span></span> |
| <span data-ttu-id="b1ea0-202">Uveďte bloku</span><span class="sxs-lookup"><span data-stu-id="b1ea0-202">Put Block</span></span> |<span data-ttu-id="b1ea0-203">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-203">Owner only</span></span> |<span data-ttu-id="b1ea0-204">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-204">Owner only</span></span> |
| <span data-ttu-id="b1ea0-205">Získat seznam blokovaných položek (pouze potvrdit bloky)</span><span class="sxs-lookup"><span data-stu-id="b1ea0-205">Get Block List (committed blocks only)</span></span> |<span data-ttu-id="b1ea0-206">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-206">All</span></span> |<span data-ttu-id="b1ea0-207">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-207">All</span></span> |
| <span data-ttu-id="b1ea0-208">Získat seznam blokovaných (pouze nepotvrzené bloky nebo všechny bloky)</span><span class="sxs-lookup"><span data-stu-id="b1ea0-208">Get Block List (uncommitted blocks only or all blocks)</span></span> |<span data-ttu-id="b1ea0-209">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-209">Owner only</span></span> |<span data-ttu-id="b1ea0-210">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-210">Owner only</span></span> |
| <span data-ttu-id="b1ea0-211">Uveďte seznam blokovaných položek</span><span class="sxs-lookup"><span data-stu-id="b1ea0-211">Put Block List</span></span> |<span data-ttu-id="b1ea0-212">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-212">Owner only</span></span> |<span data-ttu-id="b1ea0-213">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-213">Owner only</span></span> |
| <span data-ttu-id="b1ea0-214">Odstranit objekt Blob</span><span class="sxs-lookup"><span data-stu-id="b1ea0-214">Delete Blob</span></span> |<span data-ttu-id="b1ea0-215">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-215">Owner only</span></span> |<span data-ttu-id="b1ea0-216">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-216">Owner only</span></span> |
| <span data-ttu-id="b1ea0-217">Zkopírování objektu Blob</span><span class="sxs-lookup"><span data-stu-id="b1ea0-217">Copy Blob</span></span> |<span data-ttu-id="b1ea0-218">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-218">Owner only</span></span> |<span data-ttu-id="b1ea0-219">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-219">Owner only</span></span> |
| <span data-ttu-id="b1ea0-220">Objekt Blob snímku</span><span class="sxs-lookup"><span data-stu-id="b1ea0-220">Snapshot Blob</span></span> |<span data-ttu-id="b1ea0-221">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-221">Owner only</span></span> |<span data-ttu-id="b1ea0-222">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-222">Owner only</span></span> |
| <span data-ttu-id="b1ea0-223">Objekt Blob zapůjčení</span><span class="sxs-lookup"><span data-stu-id="b1ea0-223">Lease Blob</span></span> |<span data-ttu-id="b1ea0-224">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-224">Owner only</span></span> |<span data-ttu-id="b1ea0-225">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-225">Owner only</span></span> |
| <span data-ttu-id="b1ea0-226">Uveďte stránky</span><span class="sxs-lookup"><span data-stu-id="b1ea0-226">Put Page</span></span> |<span data-ttu-id="b1ea0-227">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-227">Owner only</span></span> |<span data-ttu-id="b1ea0-228">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-228">Owner only</span></span> |
| <span data-ttu-id="b1ea0-229">Get rozsahů stránek</span><span class="sxs-lookup"><span data-stu-id="b1ea0-229">Get Page Ranges</span></span> |<span data-ttu-id="b1ea0-230">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-230">All</span></span> |<span data-ttu-id="b1ea0-231">Všechny</span><span class="sxs-lookup"><span data-stu-id="b1ea0-231">All</span></span> |
| <span data-ttu-id="b1ea0-232">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="b1ea0-232">Append Blob</span></span> |<span data-ttu-id="b1ea0-233">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-233">Owner only</span></span> |<span data-ttu-id="b1ea0-234">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="b1ea0-234">Owner only</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b1ea0-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b1ea0-235">Next steps</span></span>

* [<span data-ttu-id="b1ea0-236">Ověřování služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b1ea0-236">Authentication for the Azure Storage Services</span></span>](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [<span data-ttu-id="b1ea0-237">Použití sdílených přístupových podpisů (SAS)</span><span class="sxs-lookup"><span data-stu-id="b1ea0-237">Using Shared Access Signatures (SAS)</span></span>](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [<span data-ttu-id="b1ea0-238">Delegování přístupu se sdíleným přístupovým podpisem</span><span class="sxs-lookup"><span data-stu-id="b1ea0-238">Delegating Access with a Shared Access Signature</span></span>](https://msdn.microsoft.com/library/azure/ee395415.aspx)
