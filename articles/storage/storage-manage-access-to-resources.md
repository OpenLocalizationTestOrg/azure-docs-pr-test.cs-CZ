---
title: "aaaEnable veřejný přístup čtení pro kontejnery a objekty BLOB v Azure Blob storage | Microsoft Docs"
description: "Zjistěte, jak toomake kontejnery a objekty BLOB, které jsou dostupné pro anonymní přístup a jak tooaccess je prostřednictvím kódu programu."
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
ms.openlocfilehash: 0675b5dc4d32a3a0a34376ae4c049542b07ba03a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-anonymous-read-access-toocontainers-and-blobs"></a><span data-ttu-id="faf87-103">Správa toocontainers anonymní přístup pro čtení a objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="faf87-103">Manage anonymous read access toocontainers and blobs</span></span>
<span data-ttu-id="faf87-104">Můžete povolit přístup pro čtení anonymní, veřejné tooa kontejner a jeho objekty BLOB v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="faf87-104">You can enable anonymous, public read access tooa container and its blobs in Azure Blob storage.</span></span> <span data-ttu-id="faf87-105">Díky tomu můžete udělit oprávnění jen pro čtení toothese prostředky bez sdílení klíč účtu a bez nutnosti sdílený přístupový podpis (SAS).</span><span class="sxs-lookup"><span data-stu-id="faf87-105">By doing so, you can grant read-only access toothese resources without sharing your account key, and without requiring a shared access signature (SAS).</span></span>

<span data-ttu-id="faf87-106">Veřejný přístup pro čtení je nejvhodnější pro scénáře, kde chcete určité tooalways objekty BLOB nebudou dostupné pro anonymní přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="faf87-106">Public read access is best for scenarios where you want certain blobs tooalways be available for anonymous read access.</span></span> <span data-ttu-id="faf87-107">Pro větší kontrolu můžete vytvořit sdílený přístupový podpis.</span><span class="sxs-lookup"><span data-stu-id="faf87-107">For more fine-grained control, you can create a shared access signature.</span></span> <span data-ttu-id="faf87-108">Sdílené přístupové podpisy povolit tooprovide omezený přístup pomocí různých oprávnění pro konkrétní časové období.</span><span class="sxs-lookup"><span data-stu-id="faf87-108">Shared access signatures enable you tooprovide restricted access using different permissions, for a specific time period.</span></span> <span data-ttu-id="faf87-109">Další informace o vytváření sdílený přístup podpisy, najdete v části [pomocí sdílené přístupové podpisy (SAS) ve službě Azure Storage](storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="faf87-109">For more information about creating shared access signatures, see [Using shared access signatures (SAS) in Azure Storage](storage-dotnet-shared-access-signature-part-1.md).</span></span>

## <a name="grant-anonymous-users-permissions-toocontainers-and-blobs"></a><span data-ttu-id="faf87-110">Udělení oprávnění toocontainers anonymním uživatelům a objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="faf87-110">Grant anonymous users permissions toocontainers and blobs</span></span>
<span data-ttu-id="faf87-111">Ve výchozím nastavení kontejner a všechny objekty BLOB v něm může být přístupné pouze vlastník hello hello účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="faf87-111">By default, a container and any blobs within it may be accessed only by hello owner of hello storage account.</span></span> <span data-ttu-id="faf87-112">toogive anonymní uživatelé oprávnění ke čtení tooa kontejner a jeho objekty BLOB, můžete nastavit hello kontejneru oprávnění tooallow veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="faf87-112">toogive anonymous users read permissions tooa container and its blobs, you can set hello container permissions tooallow public access.</span></span> <span data-ttu-id="faf87-113">Anonymní uživatelé mohou číst objekty BLOB do kontejneru, veřejně přístupný bez ověřování hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="faf87-113">Anonymous users can read blobs within a publicly accessible container without authenticating hello request.</span></span>

<span data-ttu-id="faf87-114">Kontejner můžete nakonfigurovat s hello následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="faf87-114">You can configure a container with hello following permissions:</span></span>

* <span data-ttu-id="faf87-115">**Žádný veřejný přístup pro čtení:** hello kontejner a jeho objekty BLOB je přístupný jenom vlastník účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="faf87-115">**No public read access:** hello container and its blobs can be accessed only by hello storage account owner.</span></span> <span data-ttu-id="faf87-116">Toto je výchozí hello pro všechny nové kontejnery.</span><span class="sxs-lookup"><span data-stu-id="faf87-116">This is hello default for all new containers.</span></span>
* <span data-ttu-id="faf87-117">**Veřejný přístup pro objekty BLOB pouze pro čtení:** objektů BLOB v kontejneru hello mohou být přečteny anonymní žádost, ale kontejneru data nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="faf87-117">**Public read access for blobs only:** Blobs within hello container can be read by anonymous request, but container data is not available.</span></span> <span data-ttu-id="faf87-118">Anonymní klienti nemůže vytvořit výčet hello objektů BLOB v kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="faf87-118">Anonymous clients cannot enumerate hello blobs within hello container.</span></span>
* <span data-ttu-id="faf87-119">**Úplný veřejný přístup pro čtení:** všechna data kontejnerů a objektů blob mohou být přečteny anonymní žádost.</span><span class="sxs-lookup"><span data-stu-id="faf87-119">**Full public read access:** All container and blob data can be read by anonymous request.</span></span> <span data-ttu-id="faf87-120">Klienti mohou anonymní žádosti o výčet objektů BLOB v kontejneru hello, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="faf87-120">Clients can enumerate blobs within hello container by anonymous request, but cannot enumerate containers within hello storage account.</span></span>

<span data-ttu-id="faf87-121">Můžete použít následující oprávnění kontejneru tooset hello:</span><span class="sxs-lookup"><span data-stu-id="faf87-121">You can use hello following tooset container permissions:</span></span>

* [<span data-ttu-id="faf87-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="faf87-122">Azure portal</span></span>](https://portal.azure.com)
* [<span data-ttu-id="faf87-123">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="faf87-123">Azure PowerShell</span></span>](storage-powershell-guide-full.md#how-to-manage-azure-blobs)
* [<span data-ttu-id="faf87-124">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="faf87-124">Azure CLI 2.0</span></span>](storage-azure-cli.md#create-and-manage-blobs)
* <span data-ttu-id="faf87-125">Programově pomocí jedné z knihovny klienta úložiště hello nebo hello REST API</span><span class="sxs-lookup"><span data-stu-id="faf87-125">Programmatically, by using one of hello storage client libraries or hello REST API</span></span>

### <a name="set-container-permissions-in-hello-azure-portal"></a><span data-ttu-id="faf87-126">Nastavte oprávnění kontejneru v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="faf87-126">Set container permissions in hello Azure portal</span></span>
<span data-ttu-id="faf87-127">oprávnění ke kontejneru tooset hello [portál Azure](https://portal.azure.com), postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="faf87-127">tooset container permissions in hello [Azure portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="faf87-128">Otevřete váš **účet úložiště** okna portálu hello.</span><span class="sxs-lookup"><span data-stu-id="faf87-128">Open your **Storage account** blade in hello portal.</span></span> <span data-ttu-id="faf87-129">Váš účet úložiště můžete najít tak, že vyberete **účty úložiště** v hlavní nabídce portálu okně hello.</span><span class="sxs-lookup"><span data-stu-id="faf87-129">You can find your storage account by selecting **Storage accounts** in hello main portal menu blade.</span></span>
1. <span data-ttu-id="faf87-130">V části **služby objektů BLOB** v okně hello nabídky, vyberte **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="faf87-130">Under **BLOB SERVICE** on hello menu blade, select **Containers**.</span></span>
1. <span data-ttu-id="faf87-131">Klikněte pravým tlačítkem na kontejner řádek hello nebo vyberte hello třemi tečkami tooopen hello kontejneru **kontextovou nabídku**.</span><span class="sxs-lookup"><span data-stu-id="faf87-131">Right-click on hello container row or select hello ellipsis tooopen hello container's **Context menu**.</span></span>
1. <span data-ttu-id="faf87-132">Vyberte **zásada přístupu** v kontextové nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="faf87-132">Select **Access policy** in hello context menu.</span></span>
1. <span data-ttu-id="faf87-133">Vyberte **přistupovat typu** z hello rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="faf87-133">Select an **Access type** from hello drop down menu.</span></span>

    ![Metadata kontejneru dialogové okno Upravit](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a><span data-ttu-id="faf87-135">Nastavte oprávnění kontejner s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="faf87-135">Set container permissions with .NET</span></span>
<span data-ttu-id="faf87-136">tooset oprávnění pro kontejner pomocí C# a hello Klientská knihovna pro úložiště pro .NET, nejdřív načíst hello kontejneru existující oprávnění tak, že volání hello **GetPermissions** metoda.</span><span class="sxs-lookup"><span data-stu-id="faf87-136">tooset permissions for a container using C# and hello Storage Client Library for .NET, first retrieve hello container's existing permissions by calling hello **GetPermissions** method.</span></span> <span data-ttu-id="faf87-137">Potom sadu hello **PublicAccess** vlastnost hello **BlobContainerPermissions** objekt, který je vrácen hello **GetPermissions** metoda.</span><span class="sxs-lookup"><span data-stu-id="faf87-137">Then set hello **PublicAccess** property for hello **BlobContainerPermissions** object that is returned by hello **GetPermissions** method.</span></span> <span data-ttu-id="faf87-138">Nakonec zavolejte hello **měli** metoda s hello aktualizovat oprávnění.</span><span class="sxs-lookup"><span data-stu-id="faf87-138">Finally, call hello **SetPermissions** method with hello updated permissions.</span></span>

<span data-ttu-id="faf87-139">Následující ukázka Hello nastaví hello kontejneru oprávnění toofull veřejný přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="faf87-139">hello following example sets hello container's permissions toofull public read access.</span></span> <span data-ttu-id="faf87-140">tooset oprávnění toopublic přístup pro čtení pro objekty BLOB, nastavte hello **PublicAccess** vlastnost příliš**BlobContainerPublicAccessType.Blob**.</span><span class="sxs-lookup"><span data-stu-id="faf87-140">tooset permissions toopublic read access for blobs only, set hello **PublicAccess** property too**BlobContainerPublicAccessType.Blob**.</span></span> <span data-ttu-id="faf87-141">tooremove všechna oprávnění pro anonymní uživatele nastavit vlastnost hello příliš**BlobContainerPublicAccessType.Off**.</span><span class="sxs-lookup"><span data-stu-id="faf87-141">tooremove all permissions for anonymous users, set hello property too**BlobContainerPublicAccessType.Off**.</span></span>

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a><span data-ttu-id="faf87-142">Přístup k kontejnery a objekty BLOB anonymně</span><span class="sxs-lookup"><span data-stu-id="faf87-142">Access containers and blobs anonymously</span></span>
<span data-ttu-id="faf87-143">Konstruktory, které nevyžadují přihlašovací údaje můžete použít klienta, který přistupuje ke kontejnerům a objektům BLOB anonymně.</span><span class="sxs-lookup"><span data-stu-id="faf87-143">A client that accesses containers and blobs anonymously can use constructors that do not require credentials.</span></span> <span data-ttu-id="faf87-144">Hello následující příklady ukazují několik různých způsobů anonymně tooreference prostředky služby objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="faf87-144">hello following examples show a few different ways tooreference Blob service resources anonymously.</span></span>

### <a name="create-an-anonymous-client-object"></a><span data-ttu-id="faf87-145">Vytvoření objektu anonymním klientem</span><span class="sxs-lookup"><span data-stu-id="faf87-145">Create an anonymous client object</span></span>
<span data-ttu-id="faf87-146">Můžete vytvořit nový objekt klienta služby pro anonymní přístup tím, že koncový bod služby objektů Blob hello hello účtu.</span><span class="sxs-lookup"><span data-stu-id="faf87-146">You can create a new service client object for anonymous access by providing hello Blob service endpoint for hello account.</span></span> <span data-ttu-id="faf87-147">Nicméně je nutné znát hello název kontejneru v daném účtu, který je k dispozici pro anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="faf87-147">However, you must also know hello name of a container in that account that's available for anonymous access.</span></span>

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create hello client object using hello Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference tooa container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read hello container's properties. Note this is only possible when hello container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a><span data-ttu-id="faf87-148">Odkaz kontejner anonymně</span><span class="sxs-lookup"><span data-stu-id="faf87-148">Reference a container anonymously</span></span>
<span data-ttu-id="faf87-149">Pokud máte hello URL tooa kontejner, který je anonymně k dispozici, můžete ho tooreference hello kontejneru přímo.</span><span class="sxs-lookup"><span data-stu-id="faf87-149">If you have hello URL tooa container that is anonymously available, you can use it tooreference hello container directly.</span></span>

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference tooa container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in hello container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a><span data-ttu-id="faf87-150">Referenční objekt blob anonymně</span><span class="sxs-lookup"><span data-stu-id="faf87-150">Reference a blob anonymously</span></span>
<span data-ttu-id="faf87-151">Pokud máte hello URL tooa blob, které jsou dostupné pro anonymní přístup, můžete odkazovat hello blob přímo pomocí této adresy URL:</span><span class="sxs-lookup"><span data-stu-id="faf87-151">If you have hello URL tooa blob that is available for anonymous access, you can reference hello blob directly using that URL:</span></span>

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-tooanonymous-users"></a><span data-ttu-id="faf87-152">Uživatelé k dispozici tooanonymous funkce</span><span class="sxs-lookup"><span data-stu-id="faf87-152">Features available tooanonymous users</span></span>
<span data-ttu-id="faf87-153">Hello následující tabulka ukazuje operací, které může volat anonymní uživatelé, když kontejneru seznamu ACL tooallow veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="faf87-153">hello following table shows which operations may be called by anonymous users when a container's ACL is set tooallow public access.</span></span>

| <span data-ttu-id="faf87-154">Operace REST</span><span class="sxs-lookup"><span data-stu-id="faf87-154">REST Operation</span></span> | <span data-ttu-id="faf87-155">Oprávnění Úplné veřejný přístup pro čtení</span><span class="sxs-lookup"><span data-stu-id="faf87-155">Permission with full public read access</span></span> | <span data-ttu-id="faf87-156">Oprávnění s veřejný přístup pro čtení pro pouze objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="faf87-156">Permission with public read access for blobs only</span></span> |
| --- | --- | --- |
| <span data-ttu-id="faf87-157">Seznam kontejnery</span><span class="sxs-lookup"><span data-stu-id="faf87-157">List Containers</span></span> |<span data-ttu-id="faf87-158">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-158">Owner only</span></span> |<span data-ttu-id="faf87-159">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-159">Owner only</span></span> |
| <span data-ttu-id="faf87-160">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="faf87-160">Create Container</span></span> |<span data-ttu-id="faf87-161">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-161">Owner only</span></span> |<span data-ttu-id="faf87-162">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-162">Owner only</span></span> |
| <span data-ttu-id="faf87-163">Získat vlastnosti kontejneru</span><span class="sxs-lookup"><span data-stu-id="faf87-163">Get Container Properties</span></span> |<span data-ttu-id="faf87-164">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-164">All</span></span> |<span data-ttu-id="faf87-165">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-165">Owner only</span></span> |
| <span data-ttu-id="faf87-166">Získat Metadata kontejneru</span><span class="sxs-lookup"><span data-stu-id="faf87-166">Get Container Metadata</span></span> |<span data-ttu-id="faf87-167">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-167">All</span></span> |<span data-ttu-id="faf87-168">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-168">Owner only</span></span> |
| <span data-ttu-id="faf87-169">Nastavit Metadata kontejneru</span><span class="sxs-lookup"><span data-stu-id="faf87-169">Set Container Metadata</span></span> |<span data-ttu-id="faf87-170">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-170">Owner only</span></span> |<span data-ttu-id="faf87-171">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-171">Owner only</span></span> |
| <span data-ttu-id="faf87-172">Získání kontejneru seznamu ACL</span><span class="sxs-lookup"><span data-stu-id="faf87-172">Get Container ACL</span></span> |<span data-ttu-id="faf87-173">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-173">Owner only</span></span> |<span data-ttu-id="faf87-174">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-174">Owner only</span></span> |
| <span data-ttu-id="faf87-175">Nastavit kontejneru seznamu ACL</span><span class="sxs-lookup"><span data-stu-id="faf87-175">Set Container ACL</span></span> |<span data-ttu-id="faf87-176">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-176">Owner only</span></span> |<span data-ttu-id="faf87-177">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-177">Owner only</span></span> |
| <span data-ttu-id="faf87-178">Odstranit kontejner</span><span class="sxs-lookup"><span data-stu-id="faf87-178">Delete Container</span></span> |<span data-ttu-id="faf87-179">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-179">Owner only</span></span> |<span data-ttu-id="faf87-180">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-180">Owner only</span></span> |
| <span data-ttu-id="faf87-181">Seznam objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="faf87-181">List Blobs</span></span> |<span data-ttu-id="faf87-182">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-182">All</span></span> |<span data-ttu-id="faf87-183">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-183">Owner only</span></span> |
| <span data-ttu-id="faf87-184">Uveďte objektů Blob</span><span class="sxs-lookup"><span data-stu-id="faf87-184">Put Blob</span></span> |<span data-ttu-id="faf87-185">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-185">Owner only</span></span> |<span data-ttu-id="faf87-186">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-186">Owner only</span></span> |
| <span data-ttu-id="faf87-187">Získání objektu Blob</span><span class="sxs-lookup"><span data-stu-id="faf87-187">Get Blob</span></span> |<span data-ttu-id="faf87-188">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-188">All</span></span> |<span data-ttu-id="faf87-189">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-189">All</span></span> |
| <span data-ttu-id="faf87-190">Získat vlastnosti objektu Blob</span><span class="sxs-lookup"><span data-stu-id="faf87-190">Get Blob Properties</span></span> |<span data-ttu-id="faf87-191">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-191">All</span></span> |<span data-ttu-id="faf87-192">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-192">All</span></span> |
| <span data-ttu-id="faf87-193">Nastavit vlastnosti objektů Blob</span><span class="sxs-lookup"><span data-stu-id="faf87-193">Set Blob Properties</span></span> |<span data-ttu-id="faf87-194">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-194">Owner only</span></span> |<span data-ttu-id="faf87-195">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-195">Owner only</span></span> |
| <span data-ttu-id="faf87-196">Získat Metadata objektu Blob</span><span class="sxs-lookup"><span data-stu-id="faf87-196">Get Blob Metadata</span></span> |<span data-ttu-id="faf87-197">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-197">All</span></span> |<span data-ttu-id="faf87-198">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-198">All</span></span> |
| <span data-ttu-id="faf87-199">Nastavit Metadata objektu Blob</span><span class="sxs-lookup"><span data-stu-id="faf87-199">Set Blob Metadata</span></span> |<span data-ttu-id="faf87-200">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-200">Owner only</span></span> |<span data-ttu-id="faf87-201">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-201">Owner only</span></span> |
| <span data-ttu-id="faf87-202">Uveďte bloku</span><span class="sxs-lookup"><span data-stu-id="faf87-202">Put Block</span></span> |<span data-ttu-id="faf87-203">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-203">Owner only</span></span> |<span data-ttu-id="faf87-204">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-204">Owner only</span></span> |
| <span data-ttu-id="faf87-205">Získat seznam blokovaných položek (pouze potvrdit bloky)</span><span class="sxs-lookup"><span data-stu-id="faf87-205">Get Block List (committed blocks only)</span></span> |<span data-ttu-id="faf87-206">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-206">All</span></span> |<span data-ttu-id="faf87-207">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-207">All</span></span> |
| <span data-ttu-id="faf87-208">Získat seznam blokovaných (pouze nepotvrzené bloky nebo všechny bloky)</span><span class="sxs-lookup"><span data-stu-id="faf87-208">Get Block List (uncommitted blocks only or all blocks)</span></span> |<span data-ttu-id="faf87-209">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-209">Owner only</span></span> |<span data-ttu-id="faf87-210">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-210">Owner only</span></span> |
| <span data-ttu-id="faf87-211">Uveďte seznam blokovaných položek</span><span class="sxs-lookup"><span data-stu-id="faf87-211">Put Block List</span></span> |<span data-ttu-id="faf87-212">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-212">Owner only</span></span> |<span data-ttu-id="faf87-213">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-213">Owner only</span></span> |
| <span data-ttu-id="faf87-214">Odstranit objekt Blob</span><span class="sxs-lookup"><span data-stu-id="faf87-214">Delete Blob</span></span> |<span data-ttu-id="faf87-215">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-215">Owner only</span></span> |<span data-ttu-id="faf87-216">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-216">Owner only</span></span> |
| <span data-ttu-id="faf87-217">Zkopírování objektu Blob</span><span class="sxs-lookup"><span data-stu-id="faf87-217">Copy Blob</span></span> |<span data-ttu-id="faf87-218">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-218">Owner only</span></span> |<span data-ttu-id="faf87-219">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-219">Owner only</span></span> |
| <span data-ttu-id="faf87-220">Objekt Blob snímku</span><span class="sxs-lookup"><span data-stu-id="faf87-220">Snapshot Blob</span></span> |<span data-ttu-id="faf87-221">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-221">Owner only</span></span> |<span data-ttu-id="faf87-222">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-222">Owner only</span></span> |
| <span data-ttu-id="faf87-223">Objekt Blob zapůjčení</span><span class="sxs-lookup"><span data-stu-id="faf87-223">Lease Blob</span></span> |<span data-ttu-id="faf87-224">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-224">Owner only</span></span> |<span data-ttu-id="faf87-225">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-225">Owner only</span></span> |
| <span data-ttu-id="faf87-226">Uveďte stránky</span><span class="sxs-lookup"><span data-stu-id="faf87-226">Put Page</span></span> |<span data-ttu-id="faf87-227">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-227">Owner only</span></span> |<span data-ttu-id="faf87-228">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-228">Owner only</span></span> |
| <span data-ttu-id="faf87-229">Get rozsahů stránek</span><span class="sxs-lookup"><span data-stu-id="faf87-229">Get Page Ranges</span></span> |<span data-ttu-id="faf87-230">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-230">All</span></span> |<span data-ttu-id="faf87-231">Všechny</span><span class="sxs-lookup"><span data-stu-id="faf87-231">All</span></span> |
| <span data-ttu-id="faf87-232">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="faf87-232">Append Blob</span></span> |<span data-ttu-id="faf87-233">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-233">Owner only</span></span> |<span data-ttu-id="faf87-234">Pouze vlastník</span><span class="sxs-lookup"><span data-stu-id="faf87-234">Owner only</span></span> |

## <a name="next-steps"></a><span data-ttu-id="faf87-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="faf87-235">Next steps</span></span>

* [<span data-ttu-id="faf87-236">Ověřování pro hello služby úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="faf87-236">Authentication for hello Azure Storage Services</span></span>](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [<span data-ttu-id="faf87-237">Použití sdílených přístupových podpisů (SAS)</span><span class="sxs-lookup"><span data-stu-id="faf87-237">Using Shared Access Signatures (SAS)</span></span>](storage-dotnet-shared-access-signature-part-1.md)
* [<span data-ttu-id="faf87-238">Delegování přístupu se sdíleným přístupovým podpisem</span><span class="sxs-lookup"><span data-stu-id="faf87-238">Delegating Access with a Shared Access Signature</span></span>](https://msdn.microsoft.com/library/azure/ee395415.aspx)