---
title: "Jak používat úložiště objektů Blob (úložiště objektů) z Ruby | Microsoft Docs"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
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
ms.openlocfilehash: 7f7d0c52b2b50a360711477e8e0eafc07ddcf374
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-blob-storage-from-ruby"></a><span data-ttu-id="60be4-103">Používání úložiště Blob z Ruby</span><span class="sxs-lookup"><span data-stu-id="60be4-103">How to use Blob storage from Ruby</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="60be4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="60be4-104">Overview</span></span>
<span data-ttu-id="60be4-105">Úložiště objektů blob v Azure je služba, která ukládá nestrukturovaná data v cloudu jako objekty nebo objekty blob.</span><span class="sxs-lookup"><span data-stu-id="60be4-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="60be4-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="60be4-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="60be4-107">Blob storage se také nazývá úložiště objektů.</span><span class="sxs-lookup"><span data-stu-id="60be4-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="60be4-108">Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="60be4-108">This guide will show you how to perform common scenarios using Blob storage.</span></span> <span data-ttu-id="60be4-109">Ukázky jsou zapsány pomocí rozhraní API Ruby.</span><span class="sxs-lookup"><span data-stu-id="60be4-109">The samples are written using the Ruby API.</span></span> <span data-ttu-id="60be4-110">Pokryté scénáře zahrnují **odesílání, výpis, stahování,** a **odstraňování** objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="60be4-110">The scenarios covered include **uploading, listing, downloading,** and **deleting** blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="60be4-111">Vytvoření Ruby aplikace</span><span class="sxs-lookup"><span data-stu-id="60be4-111">Create a Ruby application</span></span>
<span data-ttu-id="60be4-112">Vytvořte aplikaci pro poznámky Ruby.</span><span class="sxs-lookup"><span data-stu-id="60be4-112">Create a Ruby application.</span></span> <span data-ttu-id="60be4-113">Pokyny najdete v tématu [Ruby, na které webové aplikace ve virtuálním počítači Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span><span class="sxs-lookup"><span data-stu-id="60be4-113">For instructions, see [Ruby on Rails Web application on an Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="60be4-114">Konfigurace aplikace pro přístup k úložišti</span><span class="sxs-lookup"><span data-stu-id="60be4-114">Configure your application to access Storage</span></span>
<span data-ttu-id="60be4-115">Pokud chcete používat Azure Storage, musíte stáhnout a použít Ruby azure balíčku, který obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště.</span><span class="sxs-lookup"><span data-stu-id="60be4-115">To use Azure Storage, you need to download and use the Ruby azure package, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-rubygems-to-obtain-the-package"></a><span data-ttu-id="60be4-116">Použití RubyGems získat balíček</span><span class="sxs-lookup"><span data-stu-id="60be4-116">Use RubyGems to obtain the package</span></span>
1. <span data-ttu-id="60be4-117">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="60be4-117">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="60be4-118">"Gem instalace azure" zadejte v příkazovém okně instalace gem a závislostí.</span><span class="sxs-lookup"><span data-stu-id="60be4-118">Type "gem install azure" in the command window to install the gem and dependencies.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="60be4-119">Import balíčku</span><span class="sxs-lookup"><span data-stu-id="60be4-119">Import the package</span></span>
<span data-ttu-id="60be4-120">Na začátek souboru Ruby, kde máte v úmyslu používat úložiště pomocí svém oblíbeném textovém editoru, přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="60be4-120">Using your favorite text editor, add the following to the top of the Ruby file where you intend to use storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="60be4-121">Nastavit připojení k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="60be4-121">Set up an Azure Storage Connection</span></span>
<span data-ttu-id="60be4-122">Modul azure, bude číst proměnné prostředí **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_ACCESS_KEY** informace požadované pro připojení k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="60be4-122">The azure module will read the environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS_KEY** for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="60be4-123">Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu před použitím **Azure::Blob::BlobService** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="60be4-123">If these environment variables are not set, you must specify the account information before using **Azure::Blob::BlobService** with the following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="60be4-124">K získání těchto hodnot z klasický nebo účet správce prostředků úložiště na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="60be4-124">To obtain these values from a classic or Resource Manager storage account in the Azure portal:</span></span>

1. <span data-ttu-id="60be4-125">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="60be4-125">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="60be4-126">Přejděte na účet úložiště, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="60be4-126">Navigate to the storage account you want to use.</span></span>
3. <span data-ttu-id="60be4-127">V okně nastavení na pravé straně klikněte na tlačítko **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="60be4-127">In the Settings blade on the right, click **Access Keys**.</span></span>
4. <span data-ttu-id="60be4-128">V okně klíče přístup, který se zobrazí uvidíte přístupový klíč 1 a 2 přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="60be4-128">In the Access keys blade that appears, you'll see the access key 1 and access key 2.</span></span> <span data-ttu-id="60be4-129">Můžete použít kteroukoli z nich.</span><span class="sxs-lookup"><span data-stu-id="60be4-129">You can use either of these.</span></span>
5. <span data-ttu-id="60be4-130">Kliknutím na ikonu kopírování do schránky zkopírujte klíč.</span><span class="sxs-lookup"><span data-stu-id="60be4-130">Click the copy icon to copy the key to the clipboard.</span></span>

<span data-ttu-id="60be4-131">K získání těchto hodnot z účtu úložiště classic na portálu Azure classic:</span><span class="sxs-lookup"><span data-stu-id="60be4-131">To obtain these values from a classic storage account in the classic Azure portal:</span></span>

1. <span data-ttu-id="60be4-132">Přihlaste se k [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="60be4-132">Log in to the [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="60be4-133">Přejděte na účet úložiště, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="60be4-133">Navigate to the storage account you want to use.</span></span>
3. <span data-ttu-id="60be4-134">Klikněte na tlačítko **SPRAVOVAT přístupové klíče** v dolní části navigačního podokna.</span><span class="sxs-lookup"><span data-stu-id="60be4-134">Click **MANAGE ACCESS KEYS** at the bottom of the navigation pane.</span></span>
4. <span data-ttu-id="60be4-135">V dialogovém okně automaticky otevírané okno se zobrazí název účtu úložiště, primární přístupový klíč a sekundární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="60be4-135">In the pop-up dialog, you'll see the storage account name, primary access key and secondary access key.</span></span> <span data-ttu-id="60be4-136">Pro přístupový klíč můžete použít primární nebo sekundární jeden.</span><span class="sxs-lookup"><span data-stu-id="60be4-136">For access key, you can use either the primary one or the secondary one.</span></span>
5. <span data-ttu-id="60be4-137">Kliknutím na ikonu kopírování do schránky zkopírujte klíč.</span><span class="sxs-lookup"><span data-stu-id="60be4-137">Click the copy icon to copy the key to the clipboard.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="60be4-138">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="60be4-138">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="60be4-139">**Azure::Blob::BlobService** objekt vám umožňuje spolupracovat s kontejnery a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="60be4-139">The **Azure::Blob::BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="60be4-140">Chcete-li vytvořit kontejner, použijte **vytvořit\_container()** metoda.</span><span class="sxs-lookup"><span data-stu-id="60be4-140">To create a container, use the **create\_container()** method.</span></span>

<span data-ttu-id="60be4-141">Následující příklad kódu vytvoří kontejner nebo vytiskne chybu, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="60be4-141">The following code example creates a container or prints the error if there is any.</span></span>

```ruby
azure_blob_service = Azure::Blob::BlobService.new
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

<span data-ttu-id="60be4-142">Pokud chcete, aby soubory v kontejneru veřejného, můžete nastavit oprávnění kontejneru.</span><span class="sxs-lookup"><span data-stu-id="60be4-142">If you want to make the files in the container public, you can set the container's permissions.</span></span>

<span data-ttu-id="60be4-143">Můžete upravit pouze <strong>vytvořit\_container()</strong> volání předat **: veřejné\_přístup\_úroveň** možnost:</span><span class="sxs-lookup"><span data-stu-id="60be4-143">You can just modify the <strong>create\_container()</strong> call to pass the **:public\_access\_level** option:</span></span>

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

<span data-ttu-id="60be4-144">Platné hodnoty pro **: veřejné\_přístup\_úroveň** možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="60be4-144">Valid values for the **:public\_access\_level** option are:</span></span>

* <span data-ttu-id="60be4-145">**objekt BLOB:** Určuje veřejný přístup pro čtení pro objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="60be4-145">**blob:** Specifies public read access for blobs.</span></span> <span data-ttu-id="60be4-146">Data objektů BLOB v tomto kontejneru může číst přes anonymní žádost, ale kontejneru data nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="60be4-146">Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="60be4-147">Klienty nelze vytvořit výčet objektů BLOB v kontejneru přes anonymní žádost.</span><span class="sxs-lookup"><span data-stu-id="60be4-147">Clients cannot enumerate blobs within the container via anonymous request.</span></span>
* <span data-ttu-id="60be4-148">**kontejner:** Určuje úplné veřejný přístup pro čtení pro data kontejnerů a objektů blob.</span><span class="sxs-lookup"><span data-stu-id="60be4-148">**container:** Specifies full public read access for container and blob data.</span></span> <span data-ttu-id="60be4-149">Klienti, můžete vytvořit výčet objektů BLOB v kontejneru přes anonymní žádost, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="60be4-149">Clients can enumerate blobs within the container via anonymous request, but cannot enumerate containers within the storage account.</span></span>

<span data-ttu-id="60be4-150">Alternativně můžete upravit úroveň veřejný přístup kontejner pomocí **nastavit\_kontejneru\_acl()** metoda k určení úrovně veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="60be4-150">Alternatively, you can modify the public access level of a container by using **set\_container\_acl()** method to specify the public access level.</span></span>

<span data-ttu-id="60be4-151">Následující příklad kódu se změní na úroveň veřejný přístup **kontejneru**:</span><span class="sxs-lookup"><span data-stu-id="60be4-151">The following code example changes the public access level to **container**:</span></span>

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="60be4-152">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="60be4-152">Upload a blob into a container</span></span>
<span data-ttu-id="60be4-153">Chcete-li nahrát obsah do objektu blob, použijte **vytvořit\_bloku\_blob()** metodu pro vytvoření objektu blob, použijte jako obsah objektu blob souboru nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="60be4-153">To upload content to a blob, use the **create\_block\_blob()** method to create the blob, use a file or string as the content of the blob.</span></span>

<span data-ttu-id="60be4-154">Následující kód nahrávání souboru **test.png** jako nový blob s názvem "– Objekt blob image" v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="60be4-154">The following code uploads the file **test.png** as a new blob named "image-blob" in the container.</span></span>

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="60be4-155">Zobrazí seznam objektů blob v kontejneru</span><span class="sxs-lookup"><span data-stu-id="60be4-155">List the blobs in a container</span></span>
<span data-ttu-id="60be4-156">K zobrazení seznamu kontejnery, použijte **list_containers()** metoda.</span><span class="sxs-lookup"><span data-stu-id="60be4-156">To list the containers, use **list_containers()** method.</span></span>
<span data-ttu-id="60be4-157">K zobrazení seznamu objektů BLOB v kontejneru, použijte **seznamu\_blobs()** metoda.</span><span class="sxs-lookup"><span data-stu-id="60be4-157">To list the blobs within a container, use **list\_blobs()** method.</span></span>

<span data-ttu-id="60be4-158">To výstupy adresy URL všech objektů BLOB v všechny kontejnery pro účet.</span><span class="sxs-lookup"><span data-stu-id="60be4-158">This outputs the urls of all the blobs in all the containers for the account.</span></span>

```ruby
containers = azure_blob_service.list_containers()
containers.each do |container|
    blobs = azure_blob_service.list_blobs(container.name)
    blobs.each do |blob|
    puts blob.name
    end
end
```

## <a name="download-blobs"></a><span data-ttu-id="60be4-159">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="60be4-159">Download blobs</span></span>
<span data-ttu-id="60be4-160">Chcete-li stáhnout objekty BLOB, použijte **získat\_blob()** metoda můžete načíst obsah.</span><span class="sxs-lookup"><span data-stu-id="60be4-160">To download blobs, use the **get\_blob()** method to retrieve the contents.</span></span>

<span data-ttu-id="60be4-161">Následující příklad kódu ukazuje, jak pomocí **získat\_blob()** ke stažení obsahu "Objekt blob image" a zapisovat do místního souboru.</span><span class="sxs-lookup"><span data-stu-id="60be4-161">The following code example demonstrates using **get\_blob()** to download the contents of "image-blob" and write it to a local file.</span></span>

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a><span data-ttu-id="60be4-162">Odstranit objekt Blob</span><span class="sxs-lookup"><span data-stu-id="60be4-162">Delete a Blob</span></span>
<span data-ttu-id="60be4-163">Nakonec pokud chcete odstranit objekt blob, použijte **odstranit\_blob()** metoda.</span><span class="sxs-lookup"><span data-stu-id="60be4-163">Finally, to delete a blob, use the **delete\_blob()** method.</span></span> <span data-ttu-id="60be4-164">Následující příklad kódu ukazuje, jak odstranit objekt blob.</span><span class="sxs-lookup"><span data-stu-id="60be4-164">The following code example demonstrates how to delete a blob.</span></span>

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a><span data-ttu-id="60be4-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60be4-165">Next steps</span></span>
<span data-ttu-id="60be4-166">Další informace o složitějších úlohách úložiště najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="60be4-166">To learn about more complex storage tasks, follow these links:</span></span>

* [<span data-ttu-id="60be4-167">Blog týmu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="60be4-167">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* <span data-ttu-id="60be4-168">[Azure SDK pro Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="60be4-168">[Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>
* [<span data-ttu-id="60be4-169">Přenos dat pomocí nástroje příkazového řádku AzCopy</span><span class="sxs-lookup"><span data-stu-id="60be4-169">Transfer data with the AzCopy Command-Line Utility</span></span>](storage-use-azcopy.md)

