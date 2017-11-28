---
title: "aaaHow toouse úložiště objektů Blob (úložiště objektů) z Ruby | Microsoft Docs"
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
services: storage
documentationcenter: ruby
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: e2fe4c45-27b0-4d15-b3fb-e7eb574db717
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 638826777f5a7ae8330fd67cdbb51d5eee1736a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-ruby"></a><span data-ttu-id="3de60-103">Jak toouse úložiště objektů Blob z Ruby</span><span class="sxs-lookup"><span data-stu-id="3de60-103">How toouse Blob storage from Ruby</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="3de60-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3de60-104">Overview</span></span>
<span data-ttu-id="3de60-105">Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="3de60-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="3de60-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="3de60-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="3de60-107">Úložiště objektů blob je také odkazované tooas objektu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3de60-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="3de60-108">Tento průvodce se dozvíte, jak tooperform běžné scénáře s využitím úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="3de60-108">This guide will show you how tooperform common scenarios using Blob storage.</span></span> <span data-ttu-id="3de60-109">Ukázky Hello jsou zapsány pomocí hello Ruby rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3de60-109">hello samples are written using hello Ruby API.</span></span> <span data-ttu-id="3de60-110">Hello pokryté scénáře zahrnují **odesílání, výpis, stahování,** a **odstraňování** objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="3de60-110">hello scenarios covered include **uploading, listing, downloading,** and **deleting** blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="3de60-111">Vytvoření Ruby aplikace</span><span class="sxs-lookup"><span data-stu-id="3de60-111">Create a Ruby application</span></span>
<span data-ttu-id="3de60-112">Vytvořte aplikaci pro poznámky Ruby.</span><span class="sxs-lookup"><span data-stu-id="3de60-112">Create a Ruby application.</span></span> <span data-ttu-id="3de60-113">Pokyny najdete v tématu [Ruby, na které webové aplikace ve virtuálním počítači Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span><span class="sxs-lookup"><span data-stu-id="3de60-113">For instructions, see [Ruby on Rails Web application on an Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="3de60-114">Konfigurace vaší aplikace tooaccess úložiště</span><span class="sxs-lookup"><span data-stu-id="3de60-114">Configure your application tooaccess Storage</span></span>
<span data-ttu-id="3de60-115">toouse Azure Storage, potřebujete toodownload a použití hello Ruby balíček azure, který obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="3de60-115">toouse Azure Storage, you need toodownload and use hello Ruby azure package, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="3de60-116">Použijte RubyGems tooobtain hello balíček</span><span class="sxs-lookup"><span data-stu-id="3de60-116">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="3de60-117">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="3de60-117">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="3de60-118">Zadejte "gem instalace azure" hello příkazového okna tooinstall hello gem a závislosti.</span><span class="sxs-lookup"><span data-stu-id="3de60-118">Type "gem install azure" in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="3de60-119">Importovat balíček hello</span><span class="sxs-lookup"><span data-stu-id="3de60-119">Import hello package</span></span>
<span data-ttu-id="3de60-120">Pomocí svém oblíbeném textovém editoru, přidejte následující toohello horní části hello Ruby souboru, kde chcete úložiště toouse hello:</span><span class="sxs-lookup"><span data-stu-id="3de60-120">Using your favorite text editor, add hello following toohello top of hello Ruby file where you intend toouse storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="3de60-121">Nastavit připojení k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3de60-121">Set up an Azure Storage Connection</span></span>
<span data-ttu-id="3de60-122">modul Hello azure, bude číst proměnné prostředí hello **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_ACCESS_KEY** pro účet úložiště Azure tooyour tooconnect jsou požadovány informace.</span><span class="sxs-lookup"><span data-stu-id="3de60-122">hello azure module will read hello environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS_KEY** for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="3de60-123">Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu hello před použitím **Azure::Blob::BlobService** s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="3de60-123">If these environment variables are not set, you must specify hello account information before using **Azure::Blob::BlobService** with hello following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="3de60-124">tooobtain tyto hodnoty ze Správce prostředků úložiště nebo klasický účet v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="3de60-124">tooobtain these values from a classic or Resource Manager storage account in hello Azure portal:</span></span>

1. <span data-ttu-id="3de60-125">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3de60-125">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3de60-126">Přejděte toohello úložiště účet, že který má toouse.</span><span class="sxs-lookup"><span data-stu-id="3de60-126">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="3de60-127">V okně Nastavení hello na hello správné, klikněte na tlačítko **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="3de60-127">In hello Settings blade on hello right, click **Access Keys**.</span></span>
4. <span data-ttu-id="3de60-128">V okně klíče přístup hello který se zobrazí uvidíte hello přístupový klíč 1 a 2 přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="3de60-128">In hello Access keys blade that appears, you'll see hello access key 1 and access key 2.</span></span> <span data-ttu-id="3de60-129">Můžete použít kteroukoli z nich.</span><span class="sxs-lookup"><span data-stu-id="3de60-129">You can use either of these.</span></span>
5. <span data-ttu-id="3de60-130">Klikněte na tlačítko hello kopírování ikonu toocopy hello klíče toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="3de60-130">Click hello copy icon toocopy hello key toohello clipboard.</span></span>

<span data-ttu-id="3de60-131">tooobtain tyto hodnoty z úložiště classic účet na portálu Azure classic hello:</span><span class="sxs-lookup"><span data-stu-id="3de60-131">tooobtain these values from a classic storage account in hello classic Azure portal:</span></span>

1. <span data-ttu-id="3de60-132">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="3de60-132">Log in toohello [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="3de60-133">Přejděte toohello úložiště účet, že který má toouse.</span><span class="sxs-lookup"><span data-stu-id="3de60-133">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="3de60-134">Klikněte na tlačítko **SPRAVOVAT přístupové klíče** na hello dolní části navigačního podokna hello.</span><span class="sxs-lookup"><span data-stu-id="3de60-134">Click **MANAGE ACCESS KEYS** at hello bottom of hello navigation pane.</span></span>
4. <span data-ttu-id="3de60-135">V místním dialogovém okně hello zobrazí se název účtu úložiště hello, primární přístupový klíč a sekundární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="3de60-135">In hello pop-up dialog, you'll see hello storage account name, primary access key and secondary access key.</span></span> <span data-ttu-id="3de60-136">Pro přístupový klíč můžete použít buď hello primární nebo sekundární jeden hello.</span><span class="sxs-lookup"><span data-stu-id="3de60-136">For access key, you can use either hello primary one or hello secondary one.</span></span>
5. <span data-ttu-id="3de60-137">Klikněte na tlačítko hello kopírování ikonu toocopy hello klíče toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="3de60-137">Click hello copy icon toocopy hello key toohello clipboard.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="3de60-138">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="3de60-138">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="3de60-139">Hello **Azure::Blob::BlobService** objekt vám umožňuje spolupracovat s kontejnery a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="3de60-139">hello **Azure::Blob::BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="3de60-140">toocreate kontejner, použijte hello **vytvořit\_container()** metoda.</span><span class="sxs-lookup"><span data-stu-id="3de60-140">toocreate a container, use hello **create\_container()** method.</span></span>

<span data-ttu-id="3de60-141">Hello následující příklad kódu vytvoří kontejner nebo vytiskne hello chyby, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="3de60-141">hello following code example creates a container or prints hello error if there is any.</span></span>

```ruby
azure_blob_service = Azure::Blob::BlobService.new
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

<span data-ttu-id="3de60-142">Pokud chcete soubory hello toomake v kontejneru hello veřejné, můžete nastavit oprávnění hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3de60-142">If you want toomake hello files in hello container public, you can set hello container's permissions.</span></span>

<span data-ttu-id="3de60-143">Upravíte na hello <strong>vytvořit\_container()</strong> volání toopass hello **: veřejné\_přístup\_úroveň** možnost:</span><span class="sxs-lookup"><span data-stu-id="3de60-143">You can just modify hello <strong>create\_container()</strong> call toopass hello **:public\_access\_level** option:</span></span>

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

<span data-ttu-id="3de60-144">Platné hodnoty pro hello **: veřejné\_přístup\_úroveň** možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="3de60-144">Valid values for hello **:public\_access\_level** option are:</span></span>

* <span data-ttu-id="3de60-145">**objekt BLOB:** Určuje veřejný přístup pro čtení pro objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="3de60-145">**blob:** Specifies public read access for blobs.</span></span> <span data-ttu-id="3de60-146">Data objektů BLOB v tomto kontejneru může číst přes anonymní žádost, ale kontejneru data nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3de60-146">Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="3de60-147">Klienty nelze vytvořit výčet objektů BLOB v kontejneru hello přes anonymní žádost.</span><span class="sxs-lookup"><span data-stu-id="3de60-147">Clients cannot enumerate blobs within hello container via anonymous request.</span></span>
* <span data-ttu-id="3de60-148">**kontejner:** Určuje úplné veřejný přístup pro čtení pro data kontejnerů a objektů blob.</span><span class="sxs-lookup"><span data-stu-id="3de60-148">**container:** Specifies full public read access for container and blob data.</span></span> <span data-ttu-id="3de60-149">Klienti, můžete vytvořit výčet objektů BLOB v kontejneru hello přes anonymní žádost, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="3de60-149">Clients can enumerate blobs within hello container via anonymous request, but cannot enumerate containers within hello storage account.</span></span>

<span data-ttu-id="3de60-150">Alternativně můžete upravit úroveň veřejný přístup hello kontejner pomocí **nastavit\_kontejneru\_acl()** metoda toospecify hello veřejný přístup úroveň.</span><span class="sxs-lookup"><span data-stu-id="3de60-150">Alternatively, you can modify hello public access level of a container by using **set\_container\_acl()** method toospecify hello public access level.</span></span>

<span data-ttu-id="3de60-151">Následující ukázka kódu změny hello veřejný přístup úroveň příliš Hello**kontejneru**:</span><span class="sxs-lookup"><span data-stu-id="3de60-151">hello following code example changes hello public access level too**container**:</span></span>

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="3de60-152">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="3de60-152">Upload a blob into a container</span></span>
<span data-ttu-id="3de60-153">Objekt blob obsahu tooa tooupload, použijte hello **vytvořit\_bloku\_blob()** metoda toocreate hello objektů blob, použijte soubor nebo řetězec jako hello obsahu objektu hello blob.</span><span class="sxs-lookup"><span data-stu-id="3de60-153">tooupload content tooa blob, use hello **create\_block\_blob()** method toocreate hello blob, use a file or string as hello content of hello blob.</span></span>

<span data-ttu-id="3de60-154">Hello následující kód odešle hello soubor **test.png** jako nový blob s názvem "– Objekt blob image" v kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="3de60-154">hello following code uploads hello file **test.png** as a new blob named "image-blob" in hello container.</span></span>

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="3de60-155">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="3de60-155">List hello blobs in a container</span></span>
<span data-ttu-id="3de60-156">kontejnery hello toolist, používají **list_containers()** metoda.</span><span class="sxs-lookup"><span data-stu-id="3de60-156">toolist hello containers, use **list_containers()** method.</span></span>
<span data-ttu-id="3de60-157">použít toolist hello objektů BLOB v kontejneru, **seznamu\_blobs()** metoda.</span><span class="sxs-lookup"><span data-stu-id="3de60-157">toolist hello blobs within a container, use **list\_blobs()** method.</span></span>

<span data-ttu-id="3de60-158">To výstupy hello adresy URL všech objektů BLOB hello v všechny kontejnery hello hello účtu.</span><span class="sxs-lookup"><span data-stu-id="3de60-158">This outputs hello urls of all hello blobs in all hello containers for hello account.</span></span>

```ruby
containers = azure_blob_service.list_containers()
containers.each do |container|
    blobs = azure_blob_service.list_blobs(container.name)
    blobs.each do |blob|
    puts blob.name
    end
end
```

## <a name="download-blobs"></a><span data-ttu-id="3de60-159">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="3de60-159">Download blobs</span></span>
<span data-ttu-id="3de60-160">objekty BLOB toodownload, použijte hello **získat\_blob()** metoda tooretrieve hello obsah.</span><span class="sxs-lookup"><span data-stu-id="3de60-160">toodownload blobs, use hello **get\_blob()** method tooretrieve hello contents.</span></span>

<span data-ttu-id="3de60-161">Hello následující příklad kódu ukazuje, jak pomocí **získat\_blob()** toodownload hello obsah "Objekt blob image" a zapisují tooa místního souboru.</span><span class="sxs-lookup"><span data-stu-id="3de60-161">hello following code example demonstrates using **get\_blob()** toodownload hello contents of "image-blob" and write it tooa local file.</span></span>

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a><span data-ttu-id="3de60-162">Odstranit objekt Blob</span><span class="sxs-lookup"><span data-stu-id="3de60-162">Delete a Blob</span></span>
<span data-ttu-id="3de60-163">Nakonec toodelete objekt blob, použijte hello **odstranit\_blob()** metoda.</span><span class="sxs-lookup"><span data-stu-id="3de60-163">Finally, toodelete a blob, use hello **delete\_blob()** method.</span></span> <span data-ttu-id="3de60-164">Hello následující příklad kódu ukazuje, jak toodelete objekt blob.</span><span class="sxs-lookup"><span data-stu-id="3de60-164">hello following code example demonstrates how toodelete a blob.</span></span>

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a><span data-ttu-id="3de60-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3de60-165">Next steps</span></span>
<span data-ttu-id="3de60-166">toolearn o složitějších úlohách úložiště, použijte tyto odkazy:</span><span class="sxs-lookup"><span data-stu-id="3de60-166">toolearn about more complex storage tasks, follow these links:</span></span>

* [<span data-ttu-id="3de60-167">Blog týmu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3de60-167">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* <span data-ttu-id="3de60-168">[Azure SDK pro Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="3de60-168">[Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>
* [<span data-ttu-id="3de60-169">Přenos dat pomocí příkazového řádku Azcopy hello</span><span class="sxs-lookup"><span data-stu-id="3de60-169">Transfer data with hello AzCopy Command-Line Utility</span></span>](storage-use-azcopy.md)

