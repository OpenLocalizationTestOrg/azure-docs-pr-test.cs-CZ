---
title: "Použití Azure CLI 1.0 s Azure Storage | Microsoft Docs"
description: "Naučte se používat rozhraní příkazového řádku Azure (Azure CLI) 1.0, s Azure Storage pro vytváření a správu účtů úložiště a pracovat s Azure BLOB a soubory. Rozhraní příkazového řádku Azure je nástroj pro různé platformy"
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
ms.openlocfilehash: 837cf0f2b8db011b38de795339560574027030f8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-azure-cli-10-with-azure-storage"></a><span data-ttu-id="a77a6-104">Použití Azure CLI 1.0 s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="a77a6-104">Using the Azure CLI 1.0 with Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="a77a6-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="a77a6-105">Overview</span></span>

<span data-ttu-id="a77a6-106">Rozhraní příkazového řádku Azure poskytuje sadu softwaru open source, příkazy a platformy pro práci s platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-106">The Azure CLI provides a set of open source, cross-platform commands for working with the Azure Platform.</span></span> <span data-ttu-id="a77a6-107">Poskytuje hodně podobné funkce v nalezen [portál Azure](https://portal.azure.com) také bohaté data přístup k funkcím.</span><span class="sxs-lookup"><span data-stu-id="a77a6-107">It provides much of the same functionality found in the [Azure portal](https://portal.azure.com) as well as rich data access functionality.</span></span>

<span data-ttu-id="a77a6-108">V této příručce, jsme budete prozkoumat použití [rozhraní příkazového řádku Azure (Azure CLI)](../../cli-install-nodejs.md) k provádění různých úloh vývoj a správu s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a77a6-108">In this guide, we'll explore how to use [Azure Command-Line Interface (Azure CLI)](../../cli-install-nodejs.md) to perform a variety of development and administration tasks with Azure Storage.</span></span> <span data-ttu-id="a77a6-109">Doporučujeme stáhnout a nainstalovat nebo upgradovat na nejnovější rozhraní příkazového řádku Azure před použitím tohoto průvodce.</span><span class="sxs-lookup"><span data-stu-id="a77a6-109">We recommend that you download and install or upgrade to the latest Azure CLI before using this guide.</span></span>

<span data-ttu-id="a77a6-110">Tato příručka předpokládá, že chápete základní koncepty Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a77a6-110">This guide assumes that you understand the basic concepts of Azure Storage.</span></span> <span data-ttu-id="a77a6-111">Průvodce poskytuje řadu skripty, které ukazují použití Azure CLI s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a77a6-111">The guide provides a number of scripts to demonstrate the usage of the Azure CLI with Azure Storage.</span></span> <span data-ttu-id="a77a6-112">Nezapomeňte aktualizovat proměnné skriptu na základě vaší konfigurace před spuštěním každý skript.</span><span class="sxs-lookup"><span data-stu-id="a77a6-112">Be sure to update the script variables based on your configuration before running each script.</span></span>

> [!NOTE]
> <span data-ttu-id="a77a6-113">Průvodce poskytuje příklady příkazu a skript příkazového řádku Azure CLI pro klasické účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="a77a6-113">The guide provides the Azure CLI command and script examples for classic storage accounts.</span></span> <span data-ttu-id="a77a6-114">V tématu [pomocí rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows se správou prostředků Azure](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) pro příkazy příkazového řádku Azure CLI pro účty úložiště Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a77a6-114">See [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) for Azure CLI commands for Resource Manager storage accounts.</span></span>
>
>

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a><span data-ttu-id="a77a6-115">Začínáme s Azure Storage a rozhraní příkazového řádku Azure během 5 minut</span><span class="sxs-lookup"><span data-stu-id="a77a6-115">Get started with Azure Storage and the Azure CLI in 5 minutes</span></span>
<span data-ttu-id="a77a6-116">Tato příručka používá Ubuntu příklady, ale jiné platformy operačního systému proveďte podobně.</span><span class="sxs-lookup"><span data-stu-id="a77a6-116">This guide uses Ubuntu for examples, but other OS platforms should perform similarly.</span></span>

<span data-ttu-id="a77a6-117">**Nové do Azure:** získat předplatné Microsoft Azure a účet Microsoft přidružené k tomuto předplatnému.</span><span class="sxs-lookup"><span data-stu-id="a77a6-117">**New to Azure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="a77a6-118">Informace o možnostech nákupu Azure najdete v tématu [bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/), [zakoupit možnosti](https://azure.microsoft.com/pricing/purchase-options/), a [člen nabízí](https://azure.microsoft.com/pricing/member-offers/) (pro členy MSDN, BizSpark, Microsoft Partner Network, a další programy společnosti Microsoft).</span><span class="sxs-lookup"><span data-stu-id="a77a6-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="a77a6-119">V tématu [přiřazení rolí správce v Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Další informace o předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="a77a6-120">**Po vytvoření odběru služby Microsoft Azure a účet:**</span><span class="sxs-lookup"><span data-stu-id="a77a6-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="a77a6-121">Stáhnout a nainstalovat rozhraní příkazového řádku Azure, postupujte podle pokynů uvedených v [nainstalovat Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a77a6-121">Download and install the Azure CLI following the instructions outlined in [Install the Azure CLI](../../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="a77a6-122">Po instalaci rozhraní příkazového řádku Azure, bude možné používat příkaz azure z vaší rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) pro přístup k příkazy rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-122">Once the Azure CLI has been installed, you will be able to use the azure command from your command-line interface (Bash, Terminal, Command prompt) to access the Azure CLI commands.</span></span> <span data-ttu-id="a77a6-123">Typ _azure_ příkaz a jste měli vidět následující výstup.</span><span class="sxs-lookup"><span data-stu-id="a77a6-123">Type the _azure_ command and you should see the following output.</span></span>

    ![Výstup příkazu Azure](./media/storage-azure-cli/azure_command.png)   
3. <span data-ttu-id="a77a6-125">V rozhraní příkazového řádku zadejte `azure storage` seznam se všechny příkazy úložiště azure a získat první dojem funkcí rozhraní příkazového řádku Azure poskytuje.</span><span class="sxs-lookup"><span data-stu-id="a77a6-125">In the command-line interface, type `azure storage` to list out all the azure storage commands and get a first impression of the functionalities the Azure CLI provides.</span></span> <span data-ttu-id="a77a6-126">Můžete zadat název příkazu s **-h** parametr (například `azure storage share create -h`) můžete zobrazit podrobnosti o syntaxi příkazu.</span><span class="sxs-lookup"><span data-stu-id="a77a6-126">You can type command name with **-h** parameter (for example, `azure storage share create -h`) to see details of command syntax.</span></span>
4. <span data-ttu-id="a77a6-127">Nyní sdělíme vám jednoduchý skript, který ukazuje základní rozhraní příkazového řádku Azure pro přístup k úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-127">Now, we'll give you a simple script that shows basic Azure CLI commands to access Azure Storage.</span></span> <span data-ttu-id="a77a6-128">Skript nejprve vás vyzve k nastavení dvou proměnných pro váš účet úložiště a klíč.</span><span class="sxs-lookup"><span data-stu-id="a77a6-128">The script will first ask you to set two variables for your storage account and key.</span></span> <span data-ttu-id="a77a6-129">Skript se pak vytvořit nový kontejner v rámci tohoto nového účtu úložiště a nahrajte existující soubor bitové kopie (binární rozsáhlý objekt) do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a77a6-129">Then, the script will create a new container in this new storage account and upload an existing image file (blob) to that container.</span></span> <span data-ttu-id="a77a6-130">Po skript obsahuje seznam všech objektů BLOB v kontejneru, stáhne soubor bitové kopie do cílového adresáře, která již existuje v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="a77a6-130">After the script lists all blobs in that container, it will download the image file to the destination directory which exists on the local computer.</span></span>

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating the container..."
    azure storage container create $container_name

    echo "Uploading the image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing the blobs..."
    azure storage blob list $container_name

    echo "Downloading the image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. <span data-ttu-id="a77a6-131">V místním počítači otevřete upřednostňované textový editor (systémem vim např.).</span><span class="sxs-lookup"><span data-stu-id="a77a6-131">In your local computer, open your preferred text editor (vim for example).</span></span> <span data-ttu-id="a77a6-132">Skript výše zadejte do textového editoru.</span><span class="sxs-lookup"><span data-stu-id="a77a6-132">Type the above script into your text editor.</span></span>
6. <span data-ttu-id="a77a6-133">Teď je potřeba aktualizovat proměnné skriptu na základě svého nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a77a6-133">Now, you need to update the script variables based on your configuration settings.</span></span>

   * <span data-ttu-id="a77a6-134">**< Storage_account_name >** použít daným názvem ve skriptu nebo zadejte nový název pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a77a6-134">**<storage_account_name>** Use the given name in the script or enter a new name for your storage account.</span></span> <span data-ttu-id="a77a6-135">**Důležité:** název účtu úložiště musí být jedinečný v Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-135">**Important:** The name of the storage account must be unique in Azure.</span></span> <span data-ttu-id="a77a6-136">Musí být malými písmeny, příliš!</span><span class="sxs-lookup"><span data-stu-id="a77a6-136">It must be lowercase, too!</span></span>
   * <span data-ttu-id="a77a6-137">**< Storage_account_key >** přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a77a6-137">**<storage_account_key>** The access key of your storage account.</span></span>
   * <span data-ttu-id="a77a6-138">**< Container_name >** použít daným názvem ve skriptu nebo zadejte nový název pro váš kontejner.</span><span class="sxs-lookup"><span data-stu-id="a77a6-138">**<container_name>** Use the given name in the script or enter a new name for your container.</span></span>
   * <span data-ttu-id="a77a6-139">**< Image_to_upload >** zadejte cestu k obrázku v místním počítači, jako například: "~ / images/HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="a77a6-139">**<image_to_upload>** Enter a path to a picture on your local computer, such as: "~/images/HelloWorld.png".</span></span>
   * <span data-ttu-id="a77a6-140">**< Destination_folder >** zadejte cestu k místnímu adresáři ukládat soubory stahované z Azure Storage, například: "~/downloadImages".</span><span class="sxs-lookup"><span data-stu-id="a77a6-140">**<destination_folder>** Enter a path to a local directory to store files downloaded from Azure Storage, such as: "~/downloadImages".</span></span>
7. <span data-ttu-id="a77a6-141">Po aktualizaci nezbytné proměnné v vim, stisknutím kombinace kláves `ESC`, `:`, `wq!` Uložte skript.</span><span class="sxs-lookup"><span data-stu-id="a77a6-141">After you've updated the necessary variables in vim, press key combinations `ESC`, `:`, `wq!` to save the script.</span></span>
8. <span data-ttu-id="a77a6-142">Pokud chcete spustit tento skript, jednoduše zadejte název souboru skriptu v konzole bash.</span><span class="sxs-lookup"><span data-stu-id="a77a6-142">To run this script, simply type the script file name in the bash console.</span></span> <span data-ttu-id="a77a6-143">Po spuštění tohoto skriptu, měli byste mít místní cílovou složku, která obsahuje soubor stažený bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a77a6-143">After this script runs, you should have a local destination folder that includes the downloaded image file.</span></span> <span data-ttu-id="a77a6-144">Následující snímek obrazovky ukazuje příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="a77a6-144">The following screenshot shows an example output:</span></span>

<span data-ttu-id="a77a6-145">Po spuštění skriptu, měli byste mít místní cílovou složku, která obsahuje soubor stažený bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a77a6-145">After the script runs, you should have a local destination folder that includes the downloaded image file.</span></span>

## <a name="manage-storage-accounts-with-the-azure-cli"></a><span data-ttu-id="a77a6-146">Správa účtů úložiště pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a77a6-146">Manage storage accounts with the Azure CLI</span></span>
### <a name="connect-to-your-azure-subscription"></a><span data-ttu-id="a77a6-147">Připojení k předplatnému služby Azure</span><span class="sxs-lookup"><span data-stu-id="a77a6-147">Connect to your Azure subscription</span></span>
<span data-ttu-id="a77a6-148">Zatímco většina příkazů, úložiště bude fungovat bez předplatného Azure, doporučujeme vám umožní připojit se k vašemu předplatnému z příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-148">While most of the storage commands will work without an Azure subscription, we recommend you to connect to your subscription from the Azure CLI.</span></span> <span data-ttu-id="a77a6-149">Pokud chcete nakonfigurovat rozhraní příkazového řádku Azure pro práci s vaším předplatným, postupujte podle kroků v [připojení k předplatnému Azure z rozhraní příkazového řádku Azure](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="a77a6-149">To configure the Azure CLI to work with your subscription, follow the steps in [Connect to an Azure subscription from the Azure CLI](../../xplat-cli-connect.md).</span></span>

### <a name="create-a-new-storage-account"></a><span data-ttu-id="a77a6-150">Vytvořit nový účet úložiště</span><span class="sxs-lookup"><span data-stu-id="a77a6-150">Create a new storage account</span></span>
<span data-ttu-id="a77a6-151">Pokud chcete používat úložiště Azure, potřebujete účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a77a6-151">To use Azure storage, you will need a storage account.</span></span> <span data-ttu-id="a77a6-152">Po nakonfigurování počítače pro připojení k vašemu předplatnému, můžete vytvořit nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-152">You can create a new Azure storage account after you have configured your computer to connect to your subscription.</span></span>

```azurecli
azure storage account create <account_name>
```

<span data-ttu-id="a77a6-153">Název účtu úložiště musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="a77a6-153">The name of your storage account must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a><span data-ttu-id="a77a6-154">Nastavit výchozí účet úložiště Azure v proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="a77a6-154">Set a default Azure storage account in environment variables</span></span>
<span data-ttu-id="a77a6-155">Můžete mít více účtů úložiště v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="a77a6-155">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="a77a6-156">Můžete zvolit jeden z nich a nastavte ji v proměnných prostředí pro všechny příkazy úložiště ve stejné relaci.</span><span class="sxs-lookup"><span data-stu-id="a77a6-156">You can choose one of them and set it in the environment variables for all the storage commands in the same session.</span></span> <span data-ttu-id="a77a6-157">To umožňuje spouštět příkazy příkazového řádku Azure CLI úložiště bez zadání účtu úložiště a klíč explicitně.</span><span class="sxs-lookup"><span data-stu-id="a77a6-157">This enables you to run the Azure CLI storage commands without specifying the storage account and key explicitly.</span></span>

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

<span data-ttu-id="a77a6-158">Jiný způsob, jak nastavit výchozí účet úložiště používá připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="a77a6-158">Another way to set a default storage account is using connection string.</span></span> <span data-ttu-id="a77a6-159">Za prvé získáte připojovací řetězec pomocí příkazu:</span><span class="sxs-lookup"><span data-stu-id="a77a6-159">Firstly get the connection string by command:</span></span>

```azurecli
azure storage account connectionstring show <account_name>
```

<span data-ttu-id="a77a6-160">Pak zkopírujte připojovací řetězec výstup a nastavte ji do proměnné prostředí:</span><span class="sxs-lookup"><span data-stu-id="a77a6-160">Then copy the output connection string and set it to environment variable:</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a><span data-ttu-id="a77a6-161">Vytvářet a spravovat objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="a77a6-161">Create and manage blobs</span></span>
<span data-ttu-id="a77a6-162">Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která jsou přístupná odkudkoli na světě prostřednictvím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a77a6-162">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="a77a6-163">V této části se předpokládá, že jste již obeznámeni s koncepty úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-163">This section assumes that you are already familiar with the Azure Blob storage concepts.</span></span> <span data-ttu-id="a77a6-164">Podrobné informace najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../blobs/storage-dotnet-how-to-use-blobs.md) a [koncepty služby objektů Blob](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="a77a6-164">For detailed information, see [Get started with Azure Blob storage using .NET](../blobs/storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="create-a-container"></a><span data-ttu-id="a77a6-165">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="a77a6-165">Create a container</span></span>
<span data-ttu-id="a77a6-166">Každý objekt blob v úložišti Azure musí být v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a77a6-166">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="a77a6-167">Můžete vytvořit privátní kontejneru pomocí `azure storage container create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="a77a6-167">You can create a private container using the `azure storage container create` command:</span></span>

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> <span data-ttu-id="a77a6-168">Existují tři úrovně anonymní přístup pro čtení: **vypnout**, **Blob**, a **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="a77a6-168">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="a77a6-169">Chcete-li zabránit anonymní přístup k objektům BLOB, nastavte parametr oprávnění na **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="a77a6-169">To prevent anonymous access to blobs, set the Permission parameter to **Off**.</span></span> <span data-ttu-id="a77a6-170">Ve výchozím nastavení je nový kontejner je privátní a můžete přistupovat pouze pomocí vlastníka účtu.</span><span class="sxs-lookup"><span data-stu-id="a77a6-170">By default, the new container is private and can be accessed only by the account owner.</span></span> <span data-ttu-id="a77a6-171">Chcete-li povolit anonymní veřejný přístup pro čtení k prostředkům blob, ale ne pro metadata kontejneru nebo do seznamu objektů BLOB v kontejneru, nastavte parametr oprávnění na **Blob**.</span><span class="sxs-lookup"><span data-stu-id="a77a6-171">To allow anonymous public read access to blob resources, but not to container metadata or to the list of blobs in the container, set the Permission parameter to **Blob**.</span></span> <span data-ttu-id="a77a6-172">Povolit úplné veřejný přístup pro čtení do objektu blob prostředků, metadata kontejneru a seznamu objektů BLOB v kontejneru, nastavte parametr oprávnění na **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="a77a6-172">To allow full public read access to blob resources, container metadata, and the list of blobs in the container, set the Permission parameter to **Container**.</span></span> <span data-ttu-id="a77a6-173">Další informace najdete v tématu [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](../blobs/storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="a77a6-173">For more information, see [Manage anonymous read access to containers and blobs](../blobs/storage-manage-access-to-resources.md).</span></span>
>
>

### <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="a77a6-174">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="a77a6-174">Upload a blob into a container</span></span>
<span data-ttu-id="a77a6-175">Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.</span><span class="sxs-lookup"><span data-stu-id="a77a6-175">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="a77a6-176">Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="a77a6-176">For more information, see [Understanding Block Blobs, Append Blobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="a77a6-177">Pokud chcete nahrát do kontejneru objektů BLOB v, můžete použít `azure storage blob upload`.</span><span class="sxs-lookup"><span data-stu-id="a77a6-177">To upload blobs in to a container, you can use the `azure storage blob upload`.</span></span> <span data-ttu-id="a77a6-178">Ve výchozím nastavení odešle tento příkaz místních souborů do objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="a77a6-178">By default, this command uploads the local files to a block blob.</span></span> <span data-ttu-id="a77a6-179">Chcete-li zadat typ pro tento objekt blob, můžete použít `--blobtype` parametr.</span><span class="sxs-lookup"><span data-stu-id="a77a6-179">To specify the type for the blob, you can use the `--blobtype` parameter.</span></span>

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a><span data-ttu-id="a77a6-180">Stáhnout objekty BLOB z kontejneru</span><span class="sxs-lookup"><span data-stu-id="a77a6-180">Download blobs from a container</span></span>
<span data-ttu-id="a77a6-181">Následující příklad ukazuje, jak stáhnout objekty BLOB kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a77a6-181">The following example demonstrates how to download blobs from a container.</span></span>

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a><span data-ttu-id="a77a6-182">Kopírování objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="a77a6-182">Copy blobs</span></span>
<span data-ttu-id="a77a6-183">Můžete asynchronně kopírovat objekty blob v rámci účtů a oblastí nebo napříč nimi.</span><span class="sxs-lookup"><span data-stu-id="a77a6-183">You can copy blobs within or across storage accounts and regions asynchronously.</span></span>

<span data-ttu-id="a77a6-184">Následující příklad ukazuje, jak kopírovat objekt blob z jednoho účtu úložiště do druhého.</span><span class="sxs-lookup"><span data-stu-id="a77a6-184">The following example demonstrates how to copy blobs from one storage account to another.</span></span> <span data-ttu-id="a77a6-185">V této ukázce jsme vytvořit kontejner, kde jsou veřejně, objekty BLOB anonymně přístupné.</span><span class="sxs-lookup"><span data-stu-id="a77a6-185">In this sample we create a container where blobs are publicly, anonymously accessible.</span></span>

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

<span data-ttu-id="a77a6-186">Tato ukázka provede asynchronní kopírování.</span><span class="sxs-lookup"><span data-stu-id="a77a6-186">This sample performs an asynchronous copy.</span></span> <span data-ttu-id="a77a6-187">Můžete sledovat stav každé operace kopírování spuštěním `azure storage blob copy show` operaci.</span><span class="sxs-lookup"><span data-stu-id="a77a6-187">You can monitor the status of each copy operation by running the `azure storage blob copy show` operation.</span></span>

<span data-ttu-id="a77a6-188">Všimněte si, že zadaná adresa URL zdroje operace kopírování musí být veřejně přístupná nebo může zahrnovat token SAS (sdílený přístupový podpis).</span><span class="sxs-lookup"><span data-stu-id="a77a6-188">Note that the source URL provided for the copy operation must either be publicly accessible, or include a SAS (shared access signature) token.</span></span>

### <a name="delete-a-blob"></a><span data-ttu-id="a77a6-189">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="a77a6-189">Delete a blob</span></span>
<span data-ttu-id="a77a6-190">Pokud chcete odstranit objekt blob, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a77a6-190">To delete a blob, use the below command:</span></span>

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a><span data-ttu-id="a77a6-191">Vytvořit a spravovat sdílené složky</span><span class="sxs-lookup"><span data-stu-id="a77a6-191">Create and manage file shares</span></span>
<span data-ttu-id="a77a6-192">Azure File storage nabízí sdílené úložiště pro aplikace, které používají standardní protokol SMB.</span><span class="sxs-lookup"><span data-stu-id="a77a6-192">Azure File storage offers shared storage for applications using the standard SMB protocol.</span></span> <span data-ttu-id="a77a6-193">Virtuální počítače Microsoft Azure a cloudové služby i místní aplikace můžou sdílet souborová data přes sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="a77a6-193">Microsoft Azure virtual machines and cloud services, as well as on-premises applications, can share file data via mounted shares.</span></span> <span data-ttu-id="a77a6-194">Můžete spravovat sdílené složky a data souborů prostřednictvím rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-194">You can manage file shares and file data via the Azure CLI.</span></span> <span data-ttu-id="a77a6-195">Další informace o Azure File storage najdete v tématu [Začínáme s Azure File storage ve Windows](../storage-dotnet-how-to-use-files.md) nebo [postup používání Azure File storage s Linuxem](../storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="a77a6-195">For more information on Azure File storage, see [Get started with Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) or [How to use Azure File storage with Linux](../storage-how-to-use-files-linux.md).</span></span>

### <a name="create-a-file-share"></a><span data-ttu-id="a77a6-196">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="a77a6-196">Create a file share</span></span>
<span data-ttu-id="a77a6-197">Azure File sdílená složka je sdílená složka SMB v Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-197">An Azure File share is an SMB file share in Azure.</span></span> <span data-ttu-id="a77a6-198">Všechny adresáře a soubory musí být vytvořeny ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="a77a6-198">All directories and files must be created in a file share.</span></span> <span data-ttu-id="a77a6-199">Účet může obsahovat neomezený počet sdílených složek a sdílená složka můžete obsahovat neomezený počet souborů až limity kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a77a6-199">An account can contain an unlimited number of shares, and a share can store an unlimited number of files, up to the capacity limits of the storage account.</span></span> <span data-ttu-id="a77a6-200">Následující příklad vytvoří sdílenou složku s názvem **název_sdílené_položky**.</span><span class="sxs-lookup"><span data-stu-id="a77a6-200">The following example creates a file share named **myshare**.</span></span>

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a><span data-ttu-id="a77a6-201">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="a77a6-201">Create a directory</span></span>
<span data-ttu-id="a77a6-202">Adresář poskytuje volitelné hierarchická struktura pro sdílenou složku Azure.</span><span class="sxs-lookup"><span data-stu-id="a77a6-202">A directory provides an optional hierarchical structure for an Azure file share.</span></span> <span data-ttu-id="a77a6-203">Následující příklad vytvoří adresář s názvem **adresář** do sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="a77a6-203">The following example creates a directory named **myDir** in the file share.</span></span>

```azurecli
azure storage directory create myshare myDir
```

<span data-ttu-id="a77a6-204">Všimněte si, že cesta k adresáři může obsahovat několik úrovní *například*, **/ b**.</span><span class="sxs-lookup"><span data-stu-id="a77a6-204">Note that directory path can include multiple levels, *e.g.*, **a/b**.</span></span> <span data-ttu-id="a77a6-205">Nicméně je nutné zajistit, že existují všechny nadřazeného adresáře.</span><span class="sxs-lookup"><span data-stu-id="a77a6-205">However, you must ensure that all parent directories exist.</span></span> <span data-ttu-id="a77a6-206">Například pro cestu **/ b**, je nutné vytvořit adresář **** nejdřív poté vytvořte adresář **b**.</span><span class="sxs-lookup"><span data-stu-id="a77a6-206">For example, for path **a/b**, you must create directory **a** first, then create directory **b**.</span></span>

### <a name="upload-a-local-file-to-directory"></a><span data-ttu-id="a77a6-207">Uložte místní soubor do adresáře</span><span class="sxs-lookup"><span data-stu-id="a77a6-207">Upload a local file to directory</span></span>
<span data-ttu-id="a77a6-208">V následujícím příkladu se uloží soubor z **~/temp/samplefile.txt** k **adresář** adresáře.</span><span class="sxs-lookup"><span data-stu-id="a77a6-208">The following example uploads a file from **~/temp/samplefile.txt** to the **myDir** directory.</span></span> <span data-ttu-id="a77a6-209">Upravte cestu k souboru tak, aby odkazovala na platný soubor na místním počítači:</span><span class="sxs-lookup"><span data-stu-id="a77a6-209">Edit the file path so that it points to a valid file on your local machine:</span></span>

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

<span data-ttu-id="a77a6-210">Všimněte si, že soubor ve sdílené složce může být velikost až 1 TB.</span><span class="sxs-lookup"><span data-stu-id="a77a6-210">Note that a file in the share can be up to 1 TB in size.</span></span>

### <a name="list-the-files-in-the-share-root-or-directory"></a><span data-ttu-id="a77a6-211">Seznam souborů v kořenové sdílenou složku nebo adresář</span><span class="sxs-lookup"><span data-stu-id="a77a6-211">List the files in the share root or directory</span></span>
<span data-ttu-id="a77a6-212">Je možné uvést soubory a podadresáře v kořenové sdílenou složku nebo adresář pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="a77a6-212">You can list the files and subdirectories in a share root or a directory using the following command:</span></span>

```azurecli
azure storage file list myshare myDir
```

<span data-ttu-id="a77a6-213">Všimněte si, že je název adresáře pro operace výpisu volitelné.</span><span class="sxs-lookup"><span data-stu-id="a77a6-213">Note that the directory name is optional for the listing operation.</span></span> <span data-ttu-id="a77a6-214">Pokud tento parametr vynechán, příkaz vypíše obsah kořenový adresář sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="a77a6-214">If omitted, the command lists the contents of the root directory of the share.</span></span>

### <a name="copy-files"></a><span data-ttu-id="a77a6-215">Kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="a77a6-215">Copy files</span></span>
<span data-ttu-id="a77a6-216">Od verze 0.9.8 rozhraní příkazového řádku Azure, můžete zkopírovat soubor do jiného souboru, soubor do objektu blob, nebo objekt blob do souboru.</span><span class="sxs-lookup"><span data-stu-id="a77a6-216">Beginning with version 0.9.8 of Azure CLI, you can copy a file to another file, a file to a blob, or a blob to a file.</span></span> <span data-ttu-id="a77a6-217">Dole ukážeme, jak provádět operace kopírovaní pomocí rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="a77a6-217">Below we demonstrate how to perform these copy operations using CLI commands.</span></span> <span data-ttu-id="a77a6-218">Kopírování souboru do nového adresáře:</span><span class="sxs-lookup"><span data-stu-id="a77a6-218">To copy a file to the new directory:</span></span>

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

<span data-ttu-id="a77a6-219">Zkopírovat objekt blob do adresáře souboru:</span><span class="sxs-lookup"><span data-stu-id="a77a6-219">To copy a blob to a file directory:</span></span>

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a><span data-ttu-id="a77a6-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a77a6-220">Next Steps</span></span>

<span data-ttu-id="a77a6-221">Reference k příkazům 1.0 rozhraní příkazového řádku Azure můžete najít pro práci s prostředky úložiště tady:</span><span class="sxs-lookup"><span data-stu-id="a77a6-221">You can find Azure CLI 1.0 command reference for working with Storage resources here:</span></span>

* [<span data-ttu-id="a77a6-222">Azure CLI příkazy v režimu Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a77a6-222">Azure CLI commands in Resource Manager mode</span></span>](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [<span data-ttu-id="a77a6-223">Azure CLI příkazy v režimu Azure Service Management</span><span class="sxs-lookup"><span data-stu-id="a77a6-223">Azure CLI commands in Azure Service Management mode</span></span>](../../cli-install-nodejs.md)

<span data-ttu-id="a77a6-224">Může také jako pokusit [Azure CLI 2.0](../storage-azure-cli.md), naše rozhraní příkazového řádku generace napsané v Pythonu, pro použití s modelem nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a77a6-224">You may also like to try the [Azure CLI 2.0](../storage-azure-cli.md), our next-generation CLI written in Python, for use with the Resource Manager deployment model.</span></span>
