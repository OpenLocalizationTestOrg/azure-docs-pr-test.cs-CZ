---
title: "aaaUsing hello 1.0 rozhraní příkazového řádku Azure s Azure Storage | Microsoft Docs"
description: "Zjistěte, jak toouse hello rozhraní příkazového řádku Azure (Azure CLI) 1.0, s toocreate Azure Storage a spravovat účty úložiště a pracovat s Azure BLOB a soubory. Hello rozhraní příkazového řádku Azure je nástroj pro různé platformy"
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: b502232a-e8f6-4d6c-befd-3476592e0e35
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
ms.openlocfilehash: 25e459403dde631741403c8722ed07beafac35c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-10-with-azure-storage"></a><span data-ttu-id="491b5-104">Použití hello 1.0 rozhraní příkazového řádku Azure s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="491b5-104">Using hello Azure CLI 1.0 with Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="491b5-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="491b5-105">Overview</span></span>

<span data-ttu-id="491b5-106">Hello rozhraní příkazového řádku Azure poskytuje sadu softwaru open source, příkazy a platformy pro práci s hello platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="491b5-106">hello Azure CLI provides a set of open source, cross-platform commands for working with hello Azure Platform.</span></span> <span data-ttu-id="491b5-107">Poskytuje mnohem hello stejné funkce najít v hello [portál Azure](https://portal.azure.com) také bohaté data přístup k funkcím.</span><span class="sxs-lookup"><span data-stu-id="491b5-107">It provides much of hello same functionality found in hello [Azure portal](https://portal.azure.com) as well as rich data access functionality.</span></span>

<span data-ttu-id="491b5-108">V této příručce, jsme budete prozkoumat jak toouse [rozhraní příkazového řádku Azure (Azure CLI)](../../cli-install-nodejs.md) tooperform řadu vývoj a správu úloh s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="491b5-108">In this guide, we'll explore how toouse [Azure Command-Line Interface (Azure CLI)](../../cli-install-nodejs.md) tooperform a variety of development and administration tasks with Azure Storage.</span></span> <span data-ttu-id="491b5-109">Doporučujeme stáhnout a nainstalovat nebo upgradovat toohello nejnovější Azure CLI před použitím tohoto průvodce.</span><span class="sxs-lookup"><span data-stu-id="491b5-109">We recommend that you download and install or upgrade toohello latest Azure CLI before using this guide.</span></span>

<span data-ttu-id="491b5-110">Tato příručka předpokládá, že chápete základní koncepty hello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="491b5-110">This guide assumes that you understand hello basic concepts of Azure Storage.</span></span> <span data-ttu-id="491b5-111">Příručka Hello obsahuje řadu skriptů toodemonstrate hello využití hello rozhraní příkazového řádku Azure s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="491b5-111">hello guide provides a number of scripts toodemonstrate hello usage of hello Azure CLI with Azure Storage.</span></span> <span data-ttu-id="491b5-112">Být jisti tooupdate hello skriptu proměnné na základě vaší konfigurace před spuštěním každý skript.</span><span class="sxs-lookup"><span data-stu-id="491b5-112">Be sure tooupdate hello script variables based on your configuration before running each script.</span></span>

> [!NOTE]
> <span data-ttu-id="491b5-113">Hello příručka obsahuje příklady příkazu a skript příkazového řádku Azure CLI hello klasické účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="491b5-113">hello guide provides hello Azure CLI command and script examples for classic storage accounts.</span></span> <span data-ttu-id="491b5-114">V tématu [hello pomocí Azure CLI pro Mac, Linux a Windows se správou prostředků Azure](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) pro příkazy příkazového řádku Azure CLI pro účty úložiště Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="491b5-114">See [Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) for Azure CLI commands for Resource Manager storage accounts.</span></span>
>
>

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-hello-azure-cli-in-5-minutes"></a><span data-ttu-id="491b5-115">Začínáme s Azure Storage a hello rozhraní příkazového řádku Azure během 5 minut</span><span class="sxs-lookup"><span data-stu-id="491b5-115">Get started with Azure Storage and hello Azure CLI in 5 minutes</span></span>
<span data-ttu-id="491b5-116">Tato příručka používá Ubuntu příklady, ale jiné platformy operačního systému proveďte podobně.</span><span class="sxs-lookup"><span data-stu-id="491b5-116">This guide uses Ubuntu for examples, but other OS platforms should perform similarly.</span></span>

<span data-ttu-id="491b5-117">**Nové tooAzure:** získat předplatné Microsoft Azure a účet Microsoft přidružené k tomuto předplatnému.</span><span class="sxs-lookup"><span data-stu-id="491b5-117">**New tooAzure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="491b5-118">Informace o možnostech nákupu Azure najdete v tématu [bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/), [zakoupit možnosti](https://azure.microsoft.com/pricing/purchase-options/), a [člen nabízí](https://azure.microsoft.com/pricing/member-offers/) (pro členy MSDN, BizSpark, Microsoft Partner Network, a další programy společnosti Microsoft).</span><span class="sxs-lookup"><span data-stu-id="491b5-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="491b5-119">V tématu [přiřazení rolí správce v Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Další informace o předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="491b5-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="491b5-120">**Po vytvoření odběru služby Microsoft Azure a účet:**</span><span class="sxs-lookup"><span data-stu-id="491b5-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="491b5-121">Stáhněte a nainstalujte hello příkazového řádku Azure CLI hello pokynů uvedených v [hello instalace rozhraní příkazového řádku Azure](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="491b5-121">Download and install hello Azure CLI following hello instructions outlined in [Install hello Azure CLI](../../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="491b5-122">Hello rozhraní příkazového řádku Azure je po instalace, bude možné toouse hello azure příkaz z vaší rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) tooaccess hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="491b5-122">Once hello Azure CLI has been installed, you will be able toouse hello azure command from your command-line interface (Bash, Terminal, Command prompt) tooaccess hello Azure CLI commands.</span></span> <span data-ttu-id="491b5-123">Typ hello _azure_ příkaz a jste měli vidět následující výstup hello.</span><span class="sxs-lookup"><span data-stu-id="491b5-123">Type hello _azure_ command and you should see hello following output.</span></span>

    ![Výstup příkazu Azure](./media/storage-azure-cli/azure_command.png)   
3. <span data-ttu-id="491b5-125">Hello rozhraní příkazového řádku, zadejte `azure storage` toolist se všechny hello úložiště azure příkazy a získat první dojem o hello funkce hello příkazového řádku Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="491b5-125">In hello command-line interface, type `azure storage` toolist out all hello azure storage commands and get a first impression of hello functionalities hello Azure CLI provides.</span></span> <span data-ttu-id="491b5-126">Můžete zadat název příkazu s **-h** parametr (například `azure storage share create -h`) podrobností toosee syntaxe příkazu.</span><span class="sxs-lookup"><span data-stu-id="491b5-126">You can type command name with **-h** parameter (for example, `azure storage share create -h`) toosee details of command syntax.</span></span>
4. <span data-ttu-id="491b5-127">Nyní sdělíme vám jednoduchý skript, který ukazuje základní rozhraní příkazového řádku Azure tooaccess příkazy Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="491b5-127">Now, we'll give you a simple script that shows basic Azure CLI commands tooaccess Azure Storage.</span></span> <span data-ttu-id="491b5-128">skript Hello nejprve vás vyzve tooset dvě proměnné pro váš účet úložiště a klíč.</span><span class="sxs-lookup"><span data-stu-id="491b5-128">hello script will first ask you tooset two variables for your storage account and key.</span></span> <span data-ttu-id="491b5-129">Skript hello pak vytvořit nový kontejner v rámci tohoto nového účtu úložiště a nahrajte existující kontejner toothat bitové kopie souboru (binární rozsáhlý objekt).</span><span class="sxs-lookup"><span data-stu-id="491b5-129">Then, hello script will create a new container in this new storage account and upload an existing image file (blob) toothat container.</span></span> <span data-ttu-id="491b5-130">Po hello skript obsahuje seznam všech objektů BLOB v kontejneru, stáhne hello bitové kopie souboru toohello cílový adresář, který existuje v místním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="491b5-130">After hello script lists all blobs in that container, it will download hello image file toohello destination directory which exists on hello local computer.</span></span>

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating hello container..."
    azure storage container create $container_name

    echo "Uploading hello image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing hello blobs..."
    azure storage blob list $container_name

    echo "Downloading hello image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. <span data-ttu-id="491b5-131">V místním počítači otevřete upřednostňované textový editor (systémem vim např.).</span><span class="sxs-lookup"><span data-stu-id="491b5-131">In your local computer, open your preferred text editor (vim for example).</span></span> <span data-ttu-id="491b5-132">Zadejte hello výše skript do textového editoru.</span><span class="sxs-lookup"><span data-stu-id="491b5-132">Type hello above script into your text editor.</span></span>
6. <span data-ttu-id="491b5-133">Nyní musíte tooupdate hello skriptu proměnné na základě svého nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="491b5-133">Now, you need tooupdate hello script variables based on your configuration settings.</span></span>

   * <span data-ttu-id="491b5-134">**< Storage_account_name >** použít hello zadané ve skriptu hello název nebo zadejte nový název pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="491b5-134">**<storage_account_name>** Use hello given name in hello script or enter a new name for your storage account.</span></span> <span data-ttu-id="491b5-135">**Důležité:** hello název účtu úložiště hello musí být jedinečný v Azure.</span><span class="sxs-lookup"><span data-stu-id="491b5-135">**Important:** hello name of hello storage account must be unique in Azure.</span></span> <span data-ttu-id="491b5-136">Musí být malými písmeny, příliš!</span><span class="sxs-lookup"><span data-stu-id="491b5-136">It must be lowercase, too!</span></span>
   * <span data-ttu-id="491b5-137">**< storage_account_key >** hello přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="491b5-137">**<storage_account_key>** hello access key of your storage account.</span></span>
   * <span data-ttu-id="491b5-138">**< Container_name >** použít hello zadané ve skriptu hello název nebo zadejte nový název pro váš kontejner.</span><span class="sxs-lookup"><span data-stu-id="491b5-138">**<container_name>** Use hello given name in hello script or enter a new name for your container.</span></span>
   * <span data-ttu-id="491b5-139">**< Image_to_upload >** zadejte obrázku tooa cestu v místním počítači, jako například: "~ / images/HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="491b5-139">**<image_to_upload>** Enter a path tooa picture on your local computer, such as: "~/images/HelloWorld.png".</span></span>
   * <span data-ttu-id="491b5-140">**< Destination_folder >** Enter toostore souborů cesta tooa místního adresáře stáhli ze služby Azure Storage, například: "~/downloadImages".</span><span class="sxs-lookup"><span data-stu-id="491b5-140">**<destination_folder>** Enter a path tooa local directory toostore files downloaded from Azure Storage, such as: "~/downloadImages".</span></span>
7. <span data-ttu-id="491b5-141">Po aktualizaci hello nezbytné proměnné v vim, stisknutím kombinace kláves `ESC`, `:`, `wq!` toosave hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="491b5-141">After you've updated hello necessary variables in vim, press key combinations `ESC`, `:`, `wq!` toosave hello script.</span></span>
8. <span data-ttu-id="491b5-142">toorun tento skript, jednoduše zadejte hello název souboru skriptu v konzole hello bash.</span><span class="sxs-lookup"><span data-stu-id="491b5-142">toorun this script, simply type hello script file name in hello bash console.</span></span> <span data-ttu-id="491b5-143">Po spuštění tohoto skriptu, měli byste mít místní cílovou složku, která zahrnuje hello stáhnout soubor bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="491b5-143">After this script runs, you should have a local destination folder that includes hello downloaded image file.</span></span> <span data-ttu-id="491b5-144">Hello následující snímek obrazovky ukazuje příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="491b5-144">hello following screenshot shows an example output:</span></span>

<span data-ttu-id="491b5-145">Po spuštění skriptu hello byste měli mít místní cílovou složku, která zahrnuje hello stáhnout soubor bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="491b5-145">After hello script runs, you should have a local destination folder that includes hello downloaded image file.</span></span>

## <a name="manage-storage-accounts-with-hello-azure-cli"></a><span data-ttu-id="491b5-146">Správa účtů úložiště s hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="491b5-146">Manage storage accounts with hello Azure CLI</span></span>
### <a name="connect-tooyour-azure-subscription"></a><span data-ttu-id="491b5-147">Připojit tooyour předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="491b5-147">Connect tooyour Azure subscription</span></span>
<span data-ttu-id="491b5-148">Zatímco většina příkazů hello úložiště bude fungovat bez předplatného Azure, doporučujeme předplatné tooyour tooconnect z hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="491b5-148">While most of hello storage commands will work without an Azure subscription, we recommend you tooconnect tooyour subscription from hello Azure CLI.</span></span> <span data-ttu-id="491b5-149">tooconfigure hello rozhraní příkazového řádku Azure toowork s předplatným, postupujte podle kroků hello v [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure hello](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="491b5-149">tooconfigure hello Azure CLI toowork with your subscription, follow hello steps in [Connect tooan Azure subscription from hello Azure CLI](../../xplat-cli-connect.md).</span></span>

### <a name="create-a-new-storage-account"></a><span data-ttu-id="491b5-150">Vytvořit nový účet úložiště</span><span class="sxs-lookup"><span data-stu-id="491b5-150">Create a new storage account</span></span>
<span data-ttu-id="491b5-151">toouse úložiště Azure, budete potřebovat účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="491b5-151">toouse Azure storage, you will need a storage account.</span></span> <span data-ttu-id="491b5-152">Po nakonfigurování vaše počítače tooconnect tooyour předplatné, můžete vytvořit nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="491b5-152">You can create a new Azure storage account after you have configured your computer tooconnect tooyour subscription.</span></span>

```azurecli
azure storage account create <account_name>
```

<span data-ttu-id="491b5-153">Hello název svého účtu úložiště musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="491b5-153">hello name of your storage account must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a><span data-ttu-id="491b5-154">Nastavit výchozí účet úložiště Azure v proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="491b5-154">Set a default Azure storage account in environment variables</span></span>
<span data-ttu-id="491b5-155">Můžete mít více účtů úložiště v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="491b5-155">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="491b5-156">Můžete zvolit jeden z nich a nastavit ho v hello proměnné prostředí pro všechny úložiště hello příkazů v hello stejné relace.</span><span class="sxs-lookup"><span data-stu-id="491b5-156">You can choose one of them and set it in hello environment variables for all hello storage commands in hello same session.</span></span> <span data-ttu-id="491b5-157">To vám umožní toorun hello rozhraní příkazového řádku Azure úložiště příkazy bez zadání hello úložiště účtu a klíč explicitně.</span><span class="sxs-lookup"><span data-stu-id="491b5-157">This enables you toorun hello Azure CLI storage commands without specifying hello storage account and key explicitly.</span></span>

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

<span data-ttu-id="491b5-158">Jiný způsob tooset výchozí účet úložiště používá připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="491b5-158">Another way tooset a default storage account is using connection string.</span></span> <span data-ttu-id="491b5-159">Za prvé získáte hello připojovací řetězec pomocí příkazu:</span><span class="sxs-lookup"><span data-stu-id="491b5-159">Firstly get hello connection string by command:</span></span>

```azurecli
azure storage account connectionstring show <account_name>
```

<span data-ttu-id="491b5-160">Zkopírujte připojovací řetězec výstup hello a nastavte ji tooenvironment proměnné:</span><span class="sxs-lookup"><span data-stu-id="491b5-160">Then copy hello output connection string and set it tooenvironment variable:</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a><span data-ttu-id="491b5-161">Vytvářet a spravovat objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="491b5-161">Create and manage blobs</span></span>
<span data-ttu-id="491b5-162">Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která je přístupná z kdekoli v hello, world pomocí protokolů HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="491b5-162">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="491b5-163">V této části se předpokládá, že jste již obeznámeni s koncepty úložiště objektů Blob v Azure hello.</span><span class="sxs-lookup"><span data-stu-id="491b5-163">This section assumes that you are already familiar with hello Azure Blob storage concepts.</span></span> <span data-ttu-id="491b5-164">Podrobné informace najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../blobs/storage-dotnet-how-to-use-blobs.md) a [koncepty služby objektů Blob](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="491b5-164">For detailed information, see [Get started with Azure Blob storage using .NET](../blobs/storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="create-a-container"></a><span data-ttu-id="491b5-165">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="491b5-165">Create a container</span></span>
<span data-ttu-id="491b5-166">Každý objekt blob v úložišti Azure musí být v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="491b5-166">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="491b5-167">Můžete vytvořit kontejner privátní pomocí hello `azure storage container create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="491b5-167">You can create a private container using hello `azure storage container create` command:</span></span>

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> <span data-ttu-id="491b5-168">Existují tři úrovně anonymní přístup pro čtení: **vypnout**, **Blob**, a **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="491b5-168">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="491b5-169">tooprevent anonymní přístup k tooblobs parametru oprávnění sady hello příliš**vypnout**.</span><span class="sxs-lookup"><span data-stu-id="491b5-169">tooprevent anonymous access tooblobs, set hello Permission parameter too**Off**.</span></span> <span data-ttu-id="491b5-170">Ve výchozím nastavení nový kontejner hello je privátní a je přístupný jenom vlastník účtu hello.</span><span class="sxs-lookup"><span data-stu-id="491b5-170">By default, hello new container is private and can be accessed only by hello account owner.</span></span> <span data-ttu-id="491b5-171">čtení tooallow anonymní veřejný přístup k prostředkům tooblob, ale není toocontainer metadata nebo toohello seznam objektů BLOB v kontejneru hello, nastavte parametr oprávnění hello příliš**Blob**.</span><span class="sxs-lookup"><span data-stu-id="491b5-171">tooallow anonymous public read access tooblob resources, but not toocontainer metadata or toohello list of blobs in hello container, set hello Permission parameter too**Blob**.</span></span> <span data-ttu-id="491b5-172">čtení tooallow úplné veřejný přístup k prostředkům tooblob, metadata kontejneru a hello seznam objektů BLOB v kontejneru hello, nastavte parametr oprávnění hello příliš**kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="491b5-172">tooallow full public read access tooblob resources, container metadata, and hello list of blobs in hello container, set hello Permission parameter too**Container**.</span></span> <span data-ttu-id="491b5-173">Další informace najdete v tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](../blobs/storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="491b5-173">For more information, see [Manage anonymous read access toocontainers and blobs](../blobs/storage-manage-access-to-resources.md).</span></span>
>
>

### <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="491b5-174">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="491b5-174">Upload a blob into a container</span></span>
<span data-ttu-id="491b5-175">Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.</span><span class="sxs-lookup"><span data-stu-id="491b5-175">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="491b5-176">Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="491b5-176">For more information, see [Understanding Block Blobs, Append Blobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="491b5-177">tooupload objekty BLOB v kontejneru tooa, můžete použít hello `azure storage blob upload`.</span><span class="sxs-lookup"><span data-stu-id="491b5-177">tooupload blobs in tooa container, you can use hello `azure storage blob upload`.</span></span> <span data-ttu-id="491b5-178">Ve výchozím nastavení odešle tento příkaz hello objekt blob bloku tooa místní soubory.</span><span class="sxs-lookup"><span data-stu-id="491b5-178">By default, this command uploads hello local files tooa block blob.</span></span> <span data-ttu-id="491b5-179">Typ hello toospecify pro hello blob, můžete použít hello `--blobtype` parametr.</span><span class="sxs-lookup"><span data-stu-id="491b5-179">toospecify hello type for hello blob, you can use hello `--blobtype` parameter.</span></span>

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a><span data-ttu-id="491b5-180">Stáhnout objekty BLOB z kontejneru</span><span class="sxs-lookup"><span data-stu-id="491b5-180">Download blobs from a container</span></span>
<span data-ttu-id="491b5-181">Hello následující příklad ukazuje, jak z kontejneru objektů BLOB toodownload.</span><span class="sxs-lookup"><span data-stu-id="491b5-181">hello following example demonstrates how toodownload blobs from a container.</span></span>

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a><span data-ttu-id="491b5-182">Kopírování objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="491b5-182">Copy blobs</span></span>
<span data-ttu-id="491b5-183">Můžete asynchronně kopírovat objekty blob v rámci účtů a oblastí nebo napříč nimi.</span><span class="sxs-lookup"><span data-stu-id="491b5-183">You can copy blobs within or across storage accounts and regions asynchronously.</span></span>

<span data-ttu-id="491b5-184">Hello následující příklad ukazuje, jak toocopy objekty BLOB z jednoho úložiště účet tooanother.</span><span class="sxs-lookup"><span data-stu-id="491b5-184">hello following example demonstrates how toocopy blobs from one storage account tooanother.</span></span> <span data-ttu-id="491b5-185">V této ukázce jsme vytvořit kontejner, kde jsou veřejně, objekty BLOB anonymně přístupné.</span><span class="sxs-lookup"><span data-stu-id="491b5-185">In this sample we create a container where blobs are publicly, anonymously accessible.</span></span>

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

<span data-ttu-id="491b5-186">Tato ukázka provede asynchronní kopírování.</span><span class="sxs-lookup"><span data-stu-id="491b5-186">This sample performs an asynchronous copy.</span></span> <span data-ttu-id="491b5-187">Můžete sledovat stav hello každé operace kopírování spuštěním hello `azure storage blob copy show` operaci.</span><span class="sxs-lookup"><span data-stu-id="491b5-187">You can monitor hello status of each copy operation by running hello `azure storage blob copy show` operation.</span></span>

<span data-ttu-id="491b5-188">Všimněte si, že hello zdrojová adresa URL zadaná pro operace kopírování hello musí být veřejně přístupná, nebo zahrnout token SAS (sdílený přístupový podpis).</span><span class="sxs-lookup"><span data-stu-id="491b5-188">Note that hello source URL provided for hello copy operation must either be publicly accessible, or include a SAS (shared access signature) token.</span></span>

### <a name="delete-a-blob"></a><span data-ttu-id="491b5-189">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="491b5-189">Delete a blob</span></span>
<span data-ttu-id="491b5-190">toodelete objekt blob, použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="491b5-190">toodelete a blob, use hello below command:</span></span>

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a><span data-ttu-id="491b5-191">Vytvořit a spravovat sdílené složky</span><span class="sxs-lookup"><span data-stu-id="491b5-191">Create and manage file shares</span></span>
<span data-ttu-id="491b5-192">Azure File storage nabízí sdílené úložiště pro aplikace, které používají standardní protokol SMB hello.</span><span class="sxs-lookup"><span data-stu-id="491b5-192">Azure File storage offers shared storage for applications using hello standard SMB protocol.</span></span> <span data-ttu-id="491b5-193">Virtuální počítače Microsoft Azure a cloudové služby i místní aplikace můžou sdílet souborová data přes sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="491b5-193">Microsoft Azure virtual machines and cloud services, as well as on-premises applications, can share file data via mounted shares.</span></span> <span data-ttu-id="491b5-194">Můžete spravovat sdílené složky a data souborů prostřednictvím hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="491b5-194">You can manage file shares and file data via hello Azure CLI.</span></span> <span data-ttu-id="491b5-195">Další informace o Azure File storage najdete v tématu [Začínáme s Azure File storage ve Windows](../storage-dotnet-how-to-use-files.md) nebo [jak toouse Azure File storage s Linuxem](../storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="491b5-195">For more information on Azure File storage, see [Get started with Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) or [How toouse Azure File storage with Linux](../storage-how-to-use-files-linux.md).</span></span>

### <a name="create-a-file-share"></a><span data-ttu-id="491b5-196">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="491b5-196">Create a file share</span></span>
<span data-ttu-id="491b5-197">Azure File sdílená složka je sdílená složka SMB v Azure.</span><span class="sxs-lookup"><span data-stu-id="491b5-197">An Azure File share is an SMB file share in Azure.</span></span> <span data-ttu-id="491b5-198">Všechny adresáře a soubory musí být vytvořeny ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="491b5-198">All directories and files must be created in a file share.</span></span> <span data-ttu-id="491b5-199">Účet může obsahovat neomezený počet sdílených složek a sdílená složka můžete obsahovat neomezený počet souborů, až toohello limity kapacity účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="491b5-199">An account can contain an unlimited number of shares, and a share can store an unlimited number of files, up toohello capacity limits of hello storage account.</span></span> <span data-ttu-id="491b5-200">Hello následující příklad vytvoří sdílenou složku s názvem **název_sdílené_položky**.</span><span class="sxs-lookup"><span data-stu-id="491b5-200">hello following example creates a file share named **myshare**.</span></span>

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a><span data-ttu-id="491b5-201">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="491b5-201">Create a directory</span></span>
<span data-ttu-id="491b5-202">Adresář poskytuje volitelné hierarchická struktura pro sdílenou složku Azure.</span><span class="sxs-lookup"><span data-stu-id="491b5-202">A directory provides an optional hierarchical structure for an Azure file share.</span></span> <span data-ttu-id="491b5-203">Hello následující příklad vytvoří adresář s názvem **adresář** v hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="491b5-203">hello following example creates a directory named **myDir** in hello file share.</span></span>

```azurecli
azure storage directory create myshare myDir
```

<span data-ttu-id="491b5-204">Všimněte si, že cesta k adresáři může obsahovat několik úrovní *například*, **/ b**.</span><span class="sxs-lookup"><span data-stu-id="491b5-204">Note that directory path can include multiple levels, *e.g.*, **a/b**.</span></span> <span data-ttu-id="491b5-205">Nicméně je nutné zajistit, že existují všechny nadřazeného adresáře.</span><span class="sxs-lookup"><span data-stu-id="491b5-205">However, you must ensure that all parent directories exist.</span></span> <span data-ttu-id="491b5-206">Například pro cestu **/ b**, je nutné vytvořit adresář **** nejdřív poté vytvořte adresář **b**.</span><span class="sxs-lookup"><span data-stu-id="491b5-206">For example, for path **a/b**, you must create directory **a** first, then create directory **b**.</span></span>

### <a name="upload-a-local-file-toodirectory"></a><span data-ttu-id="491b5-207">Nahrát toodirectory místního souboru</span><span class="sxs-lookup"><span data-stu-id="491b5-207">Upload a local file toodirectory</span></span>
<span data-ttu-id="491b5-208">Hello následujícím příkladu se uloží soubor z **~/temp/samplefile.txt** toohello **adresář** adresáře.</span><span class="sxs-lookup"><span data-stu-id="491b5-208">hello following example uploads a file from **~/temp/samplefile.txt** toohello **myDir** directory.</span></span> <span data-ttu-id="491b5-209">Cesta k souboru hello upravte tak, aby ukazoval tooa platný soubor na místním počítači:</span><span class="sxs-lookup"><span data-stu-id="491b5-209">Edit hello file path so that it points tooa valid file on your local machine:</span></span>

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

<span data-ttu-id="491b5-210">Všimněte si, že soubor ve sdílené složce hello může být až velikost too1 TB.</span><span class="sxs-lookup"><span data-stu-id="491b5-210">Note that a file in hello share can be up too1 TB in size.</span></span>

### <a name="list-hello-files-in-hello-share-root-or-directory"></a><span data-ttu-id="491b5-211">Zobrazení seznamu souborů hello v kořenové hello sdílenou složku nebo adresář</span><span class="sxs-lookup"><span data-stu-id="491b5-211">List hello files in hello share root or directory</span></span>
<span data-ttu-id="491b5-212">Můžete vytvořit seznam hello soubory a podadresáře v kořenové sdílenou složku nebo adresář pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="491b5-212">You can list hello files and subdirectories in a share root or a directory using hello following command:</span></span>

```azurecli
azure storage file list myshare myDir
```

<span data-ttu-id="491b5-213">Všimněte si, že tento název adresáře hello je volitelné pro hello výpis operaci.</span><span class="sxs-lookup"><span data-stu-id="491b5-213">Note that hello directory name is optional for hello listing operation.</span></span> <span data-ttu-id="491b5-214">Pokud tento parametr vynechán, hello příkaz vypíše obsah hello hello kořenový adresář sdílené složky hello.</span><span class="sxs-lookup"><span data-stu-id="491b5-214">If omitted, hello command lists hello contents of hello root directory of hello share.</span></span>

### <a name="copy-files"></a><span data-ttu-id="491b5-215">Kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="491b5-215">Copy files</span></span>
<span data-ttu-id="491b5-216">Od verze 0.9.8 rozhraní příkazového řádku Azure, můžete zkopírovat soubor tooanother souboru, soubor tooa objekt blob nebo soubor tooa objektů blob.</span><span class="sxs-lookup"><span data-stu-id="491b5-216">Beginning with version 0.9.8 of Azure CLI, you can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="491b5-217">Dole ukážeme, jak se tooperform tyto zkopírovat operace pomocí rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="491b5-217">Below we demonstrate how tooperform these copy operations using CLI commands.</span></span> <span data-ttu-id="491b5-218">toocopy nový adresář toohello souboru:</span><span class="sxs-lookup"><span data-stu-id="491b5-218">toocopy a file toohello new directory:</span></span>

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

<span data-ttu-id="491b5-219">toocopy adresář souboru tooa objektů blob:</span><span class="sxs-lookup"><span data-stu-id="491b5-219">toocopy a blob tooa file directory:</span></span>

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a><span data-ttu-id="491b5-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="491b5-220">Next Steps</span></span>

<span data-ttu-id="491b5-221">Reference k příkazům 1.0 rozhraní příkazového řádku Azure můžete najít pro práci s prostředky úložiště tady:</span><span class="sxs-lookup"><span data-stu-id="491b5-221">You can find Azure CLI 1.0 command reference for working with Storage resources here:</span></span>

* [<span data-ttu-id="491b5-222">Azure CLI příkazy v režimu Resource Manager</span><span class="sxs-lookup"><span data-stu-id="491b5-222">Azure CLI commands in Resource Manager mode</span></span>](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [<span data-ttu-id="491b5-223">Azure CLI příkazy v režimu Azure Service Management</span><span class="sxs-lookup"><span data-stu-id="491b5-223">Azure CLI commands in Azure Service Management mode</span></span>](../../cli-install-nodejs.md)

<span data-ttu-id="491b5-224">Může také jako tootry hello [Azure CLI 2.0](../storage-azure-cli.md), naše rozhraní příkazového řádku generace napsané v Pythonu, pro použití s modelem nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="491b5-224">You may also like tootry hello [Azure CLI 2.0](../storage-azure-cli.md), our next-generation CLI written in Python, for use with hello Resource Manager deployment model.</span></span>
