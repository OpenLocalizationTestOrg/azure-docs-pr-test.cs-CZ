---
title: "Použití Azure CLI 2.0 s Azure Storage | Microsoft Docs"
description: "Naučte se používat rozhraní příkazového řádku Azure (Azure CLI) 2.0, s Azure Storage pro vytváření a správu účtů úložiště a pracovat s Azure BLOB a soubory. Azure CLI 2.0 je nástroj napříč platformami napsané v Pythonu."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 06/02/2017
ms.author: marsma
ms.openlocfilehash: 6098216f7dd901ea48fb3ab969c7934cc288b247
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-azure-cli-20-with-azure-storage"></a><span data-ttu-id="2e224-104">Použití Azure CLI 2.0 s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2e224-104">Using the Azure CLI 2.0 with Azure Storage</span></span>

<span data-ttu-id="2e224-105">Open source, a platformy Azure CLI 2.0 obsahuje sadu příkazů pro práci s platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="2e224-105">The open-source, cross-platform Azure CLI 2.0 provides a set of commands for working with the Azure platform.</span></span> <span data-ttu-id="2e224-106">Poskytuje hodně podobné funkce v nalezen [portál Azure](https://portal.azure.com), včetně přístupu bohaté data.</span><span class="sxs-lookup"><span data-stu-id="2e224-106">It provides much of the same functionality found in the [Azure portal](https://portal.azure.com), including rich data access.</span></span>

<span data-ttu-id="2e224-107">V této příručce, jsme ukazují, jak používat [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) provést několik úkolů práci s prostředky v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="2e224-107">In this guide, we show you how to use the [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) to perform several tasks working with resources in your Azure Storage account.</span></span> <span data-ttu-id="2e224-108">Doporučujeme stáhnout a nainstalovat nebo upgradovat na nejnovější verzi 2.0 rozhraní příkazového řádku před použitím tohoto průvodce.</span><span class="sxs-lookup"><span data-stu-id="2e224-108">We recommend that you download and install or upgrade to the latest version of the CLI 2.0 before using this guide.</span></span>

<span data-ttu-id="2e224-109">V příkladech v Průvodci se předpokládá použití prostředí Bash na Ubuntu, ale jiné platformy proveďte podobně.</span><span class="sxs-lookup"><span data-stu-id="2e224-109">The examples in the guide assume the use of the Bash shell on Ubuntu, but other platforms should perform similarly.</span></span> 

[!INCLUDE [storage-cli-versions](../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a><span data-ttu-id="2e224-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2e224-110">Prerequisites</span></span>
<span data-ttu-id="2e224-111">Tato příručka předpokládá, že chápete základní koncepty Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="2e224-111">This guide assumes that you understand the basic concepts of Azure Storage.</span></span> <span data-ttu-id="2e224-112">Předpokládá také, že jste vyhovět požadavkům vytvoření účtu, které jsou uvedeny níže pro Azure a služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e224-112">It also assumes that you're able to satisfy the account creation requirements that are specified below for Azure and the Storage service.</span></span>

### <a name="accounts"></a><span data-ttu-id="2e224-113">Účty</span><span class="sxs-lookup"><span data-stu-id="2e224-113">Accounts</span></span>
* <span data-ttu-id="2e224-114">**Účet Azure**: Pokud ještě nemáte předplatné Azure, [vytvořit bezplatný účet Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="2e224-114">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="2e224-115">**Účet Storage**: Viz část [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech Azure Storage](../storage/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="2e224-115">**Storage account**: See [Create a storage account](../storage/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/storage-create-storage-account.md).</span></span>

### <a name="install-the-azure-cli-20"></a><span data-ttu-id="2e224-116">Instalace Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2e224-116">Install the Azure CLI 2.0</span></span>

<span data-ttu-id="2e224-117">Stáhněte a nainstalujte podle pokynů uvedených v Azure CLI 2.0 [nainstalovat Azure CLI 2.0](/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="2e224-117">Download and install the Azure CLI 2.0 by following the instructions outlined in [Install Azure CLI 2.0](/cli/azure/install-az-cli2).</span></span>

> [!TIP]
> <span data-ttu-id="2e224-118">Pokud máte potíže s instalací, podívejte se [řešení potíží s instalací](/cli/azure/install-az-cli2#installation-troubleshooting) v článku a [nainstalovat řešení potíží s](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) Průvodce na Githubu.</span><span class="sxs-lookup"><span data-stu-id="2e224-118">If you have trouble with the installation, check out the [Installation Troubleshooting](/cli/azure/install-az-cli2#installation-troubleshooting) section of the article, and the [Install Troubleshooting](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) guide on GitHub.</span></span>
>

## <a name="working-with-the-cli"></a><span data-ttu-id="2e224-119">Práce s rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="2e224-119">Working with the CLI</span></span>

<span data-ttu-id="2e224-120">Po instalaci rozhraní příkazového řádku, můžete `az` příkazu ve svém rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) pro přístup k příkazy rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2e224-120">Once you've installed the CLI, you can use the `az` command in your command-line interface (Bash, Terminal, Command Prompt) to access the Azure CLI commands.</span></span> <span data-ttu-id="2e224-121">Typ `az` příkaz chcete zobrazit úplný seznam základní příkazy (výstupu v následujícím příkladu byla zkrácena):</span><span class="sxs-lookup"><span data-stu-id="2e224-121">Type the `az` command to see a full list of the base commands (the following example output has been truncated):</span></span>

```
     /\
    /  \    _____   _ _ __ ___
   / /\ \  |_  / | | | \'__/ _ \
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome to the cool new Azure CLI!

Here are the base commands:

    account          : Manage subscriptions.
    acr              : Manage Azure container registries.
    acs              : Manage Azure Container Services.
    ad               : Synchronize on-premises directories and manage Azure Active Directory
                       resources.
    ...
```

<span data-ttu-id="2e224-122">Ve svém rozhraní příkazového řádku spusťte příkaz `az storage --help` do seznamu `storage` příkaz podskupiny.</span><span class="sxs-lookup"><span data-stu-id="2e224-122">In your command-line interface, execute the command `az storage --help` to list the `storage` command subgroups.</span></span> <span data-ttu-id="2e224-123">Popisy podskupiny poskytují přehled funkcí, které poskytuje Azure CLI pro práci s vaše prostředky úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e224-123">The descriptions of the subgroups provide an overview of the functionality the Azure CLI provides for working with your storage resources.</span></span>

```
Group
    az storage: Durable, highly available, and massively scalable cloud storage.

Subgroups:
    account  : Manage storage accounts.
    blob     : Object storage for unstructured data.
    container: Manage blob storage containers.
    cors     : Manage Storage service Cross-Origin Resource Sharing (CORS).
    directory: Manage file storage directories.
    entity   : Manage table storage entities.
    file     : File shares that use the standard SMB 3.0 protocol.
    logging  : Manage Storage service logging information.
    message  : Manage queue storage messages.
    metrics  : Manage Storage service metrics.
    queue    : Use queues to effectively scale applications according to traffic.
    share    : Manage file shares.
    table    : NoSQL key-value storage using semi-structured datasets.
```

## <a name="connect-the-cli-to-your-azure-subscription"></a><span data-ttu-id="2e224-124">Připojení rozhraní příkazového řádku k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="2e224-124">Connect the CLI to your Azure subscription</span></span>

<span data-ttu-id="2e224-125">Chcete-li pracovat s prostředky ve vašem předplatném Azure, musíte nejprve se přihlásit k účtu Azure s `az login`.</span><span class="sxs-lookup"><span data-stu-id="2e224-125">To work with the resources in your Azure subscription, you must first log in to your Azure account with `az login`.</span></span> <span data-ttu-id="2e224-126">Existuje několik způsobů, které se můžete přihlásit:</span><span class="sxs-lookup"><span data-stu-id="2e224-126">There are several ways you can log in:</span></span>

* <span data-ttu-id="2e224-127">**Interaktivní přihlášení**:`az login`</span><span class="sxs-lookup"><span data-stu-id="2e224-127">**Interactive login**: `az login`</span></span>
* <span data-ttu-id="2e224-128">**Přihlaste se pomocí uživatelského jména a hesla**:`az login -u johndoe@contoso.com -p VerySecret`</span><span class="sxs-lookup"><span data-stu-id="2e224-128">**Log in with user name and password**: `az login -u johndoe@contoso.com -p VerySecret`</span></span>
  * <span data-ttu-id="2e224-129">To nebude fungovat s účty Microsoft nebo účty, které používají službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="2e224-129">This doesn't work with Microsoft accounts or accounts that use multi-factor authentication.</span></span>
* <span data-ttu-id="2e224-130">**Přihlaste se pomocí objektu služby**:`az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="2e224-130">**Log in with a service principal**: `az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`</span></span>

## <a name="azure-cli-20-sample-script"></a><span data-ttu-id="2e224-131">Azure CLI 2.0 ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="2e224-131">Azure CLI 2.0 sample script</span></span>

<span data-ttu-id="2e224-132">V dalším kroku jsme budete pracovat se skript malé prostředí, který vystavuje pár základních příkazů 2.0 rozhraní příkazového řádku Azure k interakci s prostředky Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="2e224-132">Next, we'll work with a small shell script that issues a few basic Azure CLI 2.0 commands to interact with Azure Storage resources.</span></span> <span data-ttu-id="2e224-133">Skript nejprve vytvoří nový kontejner ve vašem účtu úložiště, a poté odešle existující soubor (jako objekt blob) do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2e224-133">The script first creates a new container in your storage account, then uploads an existing file (as a blob) to that container.</span></span> <span data-ttu-id="2e224-134">Potom zobrazí seznam všech objektů BLOB v kontejneru a nakonec stáhne soubor do cílového umístění v místním počítači, který určíte.</span><span class="sxs-lookup"><span data-stu-id="2e224-134">It then lists all blobs in the container, and finally, downloads the file to a destination on your local computer that you specify.</span></span>

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

export container_name=<container_name>
export blob_name=<blob_name>
export file_to_upload=<file_to_upload>
export destination_file=<destination_file>

echo "Creating the container..."
az storage container create --name $container_name

echo "Uploading the file..."
az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

echo "Listing the blobs..."
az storage blob list --container-name $container_name --output table

echo "Downloading the file..."
az storage blob download --container-name $container_name --name $blob_name --file $destination_file --output table

echo "Done"
```

<span data-ttu-id="2e224-135">**Nakonfigurujte a spusťte skript**</span><span class="sxs-lookup"><span data-stu-id="2e224-135">**Configure and run the script**</span></span>

1. <span data-ttu-id="2e224-136">Otevřete svém oblíbeném textovém editoru, pak zkopírujte a vložte uvedený skript do editoru.</span><span class="sxs-lookup"><span data-stu-id="2e224-136">Open your favorite text editor, then copy and paste the preceding script into the editor.</span></span>

2. <span data-ttu-id="2e224-137">Potom aktualizujte proměnné ke skriptu, aby odrážela nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2e224-137">Next, update the script's variables to reflect your configuration settings.</span></span> <span data-ttu-id="2e224-138">Nahraďte uvedeného následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2e224-138">Replace the following values as specified:</span></span>

   * <span data-ttu-id="2e224-139">**\<storage_account_name\>**  název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e224-139">**\<storage_account_name\>** The name of your storage account.</span></span>
   * <span data-ttu-id="2e224-140">**\<storage_account_key\>**  primární nebo sekundární přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e224-140">**\<storage_account_key\>** The primary or secondary access key for your storage account.</span></span>
   * <span data-ttu-id="2e224-141">**\<container_name\>**  A název je nový kontejner, pokud chcete vytvořit, jako je například "azure-rozhraní příkazového řádku – ukázka container".</span><span class="sxs-lookup"><span data-stu-id="2e224-141">**\<container_name\>** A name the new container to create, such as "azure-cli-sample-container".</span></span>
   * <span data-ttu-id="2e224-142">**\<blob_name\>**  název pro cílový objekt blob v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2e224-142">**\<blob_name\>** A name for the destination blob in the container.</span></span>
   * <span data-ttu-id="2e224-143">**\<file_to_upload\>**  cestu k malým souboru v místním počítači, jako například "~ / images/HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="2e224-143">**\<file_to_upload\>** The path to small file on your local computer, such as "~/images/HelloWorld.png".</span></span>
   * <span data-ttu-id="2e224-144">**\<destination_file\>**  cíl cesta k souboru, jako například "~ / downloadedImage.png".</span><span class="sxs-lookup"><span data-stu-id="2e224-144">**\<destination_file\>** The destination file path, such as "~/downloadedImage.png".</span></span>

3. <span data-ttu-id="2e224-145">Po aktualizaci nezbytné proměnné, uložte skript a ukončete editor.</span><span class="sxs-lookup"><span data-stu-id="2e224-145">After you've updated the necessary variables, save the script and exit your editor.</span></span> <span data-ttu-id="2e224-146">Další kroky předpokládají, že jste s názvem vašeho skriptu **my_storage_sample.sh**.</span><span class="sxs-lookup"><span data-stu-id="2e224-146">The next steps assume you've named your script **my_storage_sample.sh**.</span></span>

4. <span data-ttu-id="2e224-147">Označte do skriptu jako spustitelný soubor, v případě potřeby:`chmod +x my_storage_sample.sh`</span><span class="sxs-lookup"><span data-stu-id="2e224-147">Mark the script as executable, if necessary: `chmod +x my_storage_sample.sh`</span></span>

5. <span data-ttu-id="2e224-148">Spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="2e224-148">Execute the script.</span></span> <span data-ttu-id="2e224-149">Například v Bash:`./my_storage_sample.sh`</span><span class="sxs-lookup"><span data-stu-id="2e224-149">For example, in Bash: `./my_storage_sample.sh`</span></span>

<span data-ttu-id="2e224-150">Měli byste vidět výstup podobný následujícímu a  **\<destination_file\>**  kterou jste zadali v skriptu by se měla objevit v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="2e224-150">You should see output similar to the following, and the **\<destination_file\>** you specified in the script should appear on your local computer.</span></span>

```
Creating the container...
{
  "created": true
}
Uploading the file...
Percent complete: %100.0
Listing the blobs...
Name       Blob Type      Length  Content Type              Last Modified
---------  -----------  --------  ------------------------  -------------------------
README.md  BlockBlob        6700  application/octet-stream  2017-05-12T20:54:59+00:00
Downloading the file...
Name
---------
README.md
Done
```

> [!TIP]
> <span data-ttu-id="2e224-151">Tento výstup je v **tabulky** formátu.</span><span class="sxs-lookup"><span data-stu-id="2e224-151">The preceding output is in **table** format.</span></span> <span data-ttu-id="2e224-152">Můžete určit, který výstupní formát používat tak, že zadáte `--output` argument v příkazech rozhraní příkazového řádku, nebo nastavit globálně pomocí `az configure`.</span><span class="sxs-lookup"><span data-stu-id="2e224-152">You can specify which output format to use by specifying the `--output` argument in your CLI commands, or set it globally using `az configure`.</span></span>
>

## <a name="manage-storage-accounts"></a><span data-ttu-id="2e224-153">Správa účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="2e224-153">Manage storage accounts</span></span>

### <a name="create-a-new-storage-account"></a><span data-ttu-id="2e224-154">Vytvořit nový účet úložiště</span><span class="sxs-lookup"><span data-stu-id="2e224-154">Create a new storage account</span></span>
<span data-ttu-id="2e224-155">Pokud chcete vyzkoušet službu Azure Storage, potřebujete účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e224-155">To use Azure Storage, you need a storage account.</span></span> <span data-ttu-id="2e224-156">Můžete vytvořit nový účet úložiště Azure, po dokončení konfigurace počítače pro [připojení k vašemu předplatnému](#connect-to-your-azure-subscription).</span><span class="sxs-lookup"><span data-stu-id="2e224-156">You can create a new Azure Storage account after you've configured your computer to [connect to your subscription](#connect-to-your-azure-subscription).</span></span>

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* <span data-ttu-id="2e224-157">`--location`[Vyžaduje]: umístění.</span><span class="sxs-lookup"><span data-stu-id="2e224-157">`--location` [Required]: Location.</span></span> <span data-ttu-id="2e224-158">Například "západní USA".</span><span class="sxs-lookup"><span data-stu-id="2e224-158">For example, "West US".</span></span>
* <span data-ttu-id="2e224-159">`--name`[Vyžaduje]: název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e224-159">`--name` [Required]: The storage account name.</span></span> <span data-ttu-id="2e224-160">Název musí mít délku 3 až 24 znaků a používat jenom malé alfanumerické znaky.</span><span class="sxs-lookup"><span data-stu-id="2e224-160">The name must be 3 to 24 characters in length, and use only lowercase alphanumeric characters.</span></span>
* <span data-ttu-id="2e224-161">`--resource-group`[Vyžaduje]: název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2e224-161">`--resource-group` [Required]: Name of resource group.</span></span>
* <span data-ttu-id="2e224-162">`--sku`[Vyžaduje]: účet úložiště SKU.</span><span class="sxs-lookup"><span data-stu-id="2e224-162">`--sku` [Required]: The storage account SKU.</span></span> <span data-ttu-id="2e224-163">Povolené hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2e224-163">Allowed values:</span></span>
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a><span data-ttu-id="2e224-164">Nastavení proměnných prostředí výchozí účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="2e224-164">Set default Azure storage account environment variables</span></span>
<span data-ttu-id="2e224-165">Můžete mít více účtů úložiště ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="2e224-165">You can have multiple storage accounts in your Azure subscription.</span></span> <span data-ttu-id="2e224-166">Vyberte jednu z nich chcete použít pro žádné další příkazy, můžete nastavit tyto proměnné prostředí:</span><span class="sxs-lookup"><span data-stu-id="2e224-166">To select one of them to use for all subsequent storage commands, you can set these environment variables:</span></span>

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

<span data-ttu-id="2e224-167">Jiný způsob, jak nastavit výchozí účet úložiště je pomocí připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="2e224-167">Another way to set a default storage account is by using a connection string.</span></span> <span data-ttu-id="2e224-168">Nejdřív získat připojovací řetězec s `show-connection-string` příkaz:</span><span class="sxs-lookup"><span data-stu-id="2e224-168">First, get the connection string with the `show-connection-string` command:</span></span>

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

<span data-ttu-id="2e224-169">Pak zkopírujte připojovací řetězec výstup a nastavte `AZURE_STORAGE_CONNECTION_STRING` proměnnou prostředí (budete muset uzavřete připojovací řetězec v uvozovkách):</span><span class="sxs-lookup"><span data-stu-id="2e224-169">Then copy the output connection string and set the `AZURE_STORAGE_CONNECTION_STRING` environment variable (you might need to enclose the connection string in quotes):</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> <span data-ttu-id="2e224-170">Všechny příklady v následujících částech v tomto článku předpokládá, že jste nastavili `AZURE_STORAGE_ACCOUNT` a `AZURE_STORAGE_ACCESS_KEY` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="2e224-170">All examples in the following sections of this article assume that you've set the `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_ACCESS_KEY` environment variables.</span></span>
>

## <a name="create-and-manage-blobs"></a><span data-ttu-id="2e224-171">Vytvářet a spravovat objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="2e224-171">Create and manage blobs</span></span>
<span data-ttu-id="2e224-172">Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která jsou přístupná odkudkoli na světě prostřednictvím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2e224-172">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="2e224-173">V této části se předpokládá, že jste již obeznámeni s koncepty úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="2e224-173">This section assumes that you are already familiar with Azure Blob storage concepts.</span></span> <span data-ttu-id="2e224-174">Podrobné informace najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) a [koncepty služby objektů Blob](/rest/api/storageservices/blob-service-concepts).</span><span class="sxs-lookup"><span data-stu-id="2e224-174">For detailed information, see [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](/rest/api/storageservices/blob-service-concepts).</span></span>

### <a name="create-a-container"></a><span data-ttu-id="2e224-175">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="2e224-175">Create a container</span></span>
<span data-ttu-id="2e224-176">Každý objekt blob v úložišti Azure musí být v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2e224-176">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="2e224-177">Kontejner můžete vytvořit pomocí `az storage container create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="2e224-177">You can create a container by using the `az storage container create` command:</span></span>

```azurecli
az storage container create --name <container_name>
```

<span data-ttu-id="2e224-178">Můžete nastavit jednu ze tří úrovní přístupu pro čtení pro nový kontejner zadáním volitelného `--public-access` argument:</span><span class="sxs-lookup"><span data-stu-id="2e224-178">You can set one of three levels of read access for a new container by specifying the optional  `--public-access` argument:</span></span>

* <span data-ttu-id="2e224-179">`off`(výchozí): Kontejner dat je soukromé pro vlastníka účtu.</span><span class="sxs-lookup"><span data-stu-id="2e224-179">`off` (default): Container data is private to the account owner.</span></span>
* <span data-ttu-id="2e224-180">`blob`: Pro objekty BLOB veřejný přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="2e224-180">`blob`: Public read access for blobs.</span></span>
* <span data-ttu-id="2e224-181">`container`: Veřejné pro čtení a seznamu přístup k celé kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2e224-181">`container`: Public read and list access to the entire container.</span></span>

<span data-ttu-id="2e224-182">Další informace najdete v tématu [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="2e224-182">For more information, see [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md).</span></span>

### <a name="upload-a-blob-to-a-container"></a><span data-ttu-id="2e224-183">Odeslání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="2e224-183">Upload a blob to a container</span></span>
<span data-ttu-id="2e224-184">Podporuje Azure úložiště objektů Blob bloku, připojit a objekty BLOB stránky.</span><span class="sxs-lookup"><span data-stu-id="2e224-184">Azure Blob storage supports block, append, and page blobs.</span></span> <span data-ttu-id="2e224-185">Odešlete objektů BLOB do kontejneru `blob upload` příkaz:</span><span class="sxs-lookup"><span data-stu-id="2e224-185">Upload blobs to a container by using the `blob upload` command:</span></span>

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

 <span data-ttu-id="2e224-186">Ve výchozím nastavení `blob upload` příkaz nahrávání souborů *.vhd objekty BLOB stránky, nebo objekty BLOB bloku, jinak hodnota.</span><span class="sxs-lookup"><span data-stu-id="2e224-186">By default, the `blob upload` command uploads *.vhd files to page blobs, or block blobs otherwise.</span></span> <span data-ttu-id="2e224-187">Chcete-li zadat jiný typ, pokud nahrát objekt blob, můžete použít `--type` argument – povolené hodnoty jsou `append`, `block`, a `page`.</span><span class="sxs-lookup"><span data-stu-id="2e224-187">To specify another type when you upload a blob, you can use the `--type` argument--allowed values are `append`, `block`, and `page`.</span></span>

 <span data-ttu-id="2e224-188">Další informace o typech jiný objektu blob najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span><span class="sxs-lookup"><span data-stu-id="2e224-188">For more information on the different blob types, see [Understanding Block Blobs, Append Blobs, and Page Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span></span>


### <a name="download-a-blob-from-a-container"></a><span data-ttu-id="2e224-189">Stažení objektu blob z kontejneru</span><span class="sxs-lookup"><span data-stu-id="2e224-189">Download a blob from a container</span></span>
<span data-ttu-id="2e224-190">Tento příklad ukazuje, jak stáhnout z kontejneru objektu blob:</span><span class="sxs-lookup"><span data-stu-id="2e224-190">This example demonstrates how to download a blob from a container:</span></span>

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="2e224-191">Zobrazí seznam objektů blob v kontejneru</span><span class="sxs-lookup"><span data-stu-id="2e224-191">List the blobs in a container</span></span>

<span data-ttu-id="2e224-192">Seznam objektů BLOB v kontejneru s [az úložiště objektů blob seznamu](/cli/azure/storage/blob#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="2e224-192">List the blobs in a container with the [az storage blob list](/cli/azure/storage/blob#list) command.</span></span>

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a><span data-ttu-id="2e224-193">Kopírování objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="2e224-193">Copy blobs</span></span>
<span data-ttu-id="2e224-194">Můžete asynchronně kopírovat objekty blob v rámci účtů a oblastí nebo napříč nimi.</span><span class="sxs-lookup"><span data-stu-id="2e224-194">You can copy blobs within or across storage accounts and regions asynchronously.</span></span>

<span data-ttu-id="2e224-195">Následující příklad ukazuje, jak kopírovat objekt blob z jednoho účtu úložiště do druhého.</span><span class="sxs-lookup"><span data-stu-id="2e224-195">The following example demonstrates how to copy blobs from one storage account to another.</span></span> <span data-ttu-id="2e224-196">Nejdřív vytvoříme kontejner ve zdrojovém účtu úložiště a pro jeho objekty blob zadáme veřejné oprávnění ke čtení.</span><span class="sxs-lookup"><span data-stu-id="2e224-196">We first create a container in the source storage account, specifying public read-access for its blobs.</span></span> <span data-ttu-id="2e224-197">Potom do tohoto kontejneru odešleme soubor a nakonec objekt blob z tohoto kontejneru zkopírujeme do kontejneru v cílovém účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e224-197">Next, we upload a file to the container, and finally, copy the blob from that container into a container in the destination storage account.</span></span>

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob to container in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/Pictures/sourcefile.png \
    --name sourcefile.png

# Copy blob from source account to destination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

<span data-ttu-id="2e224-198">V tomto příkladu cílový kontejner již musí existovat v cílový účet úložiště pro kopírování proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="2e224-198">In the above example, the destination container must already exist in the destination storage account for the copy operation to succeed.</span></span> <span data-ttu-id="2e224-199">Zdrojový objekt blob zadaný argumentem `--source-uri` musí navíc buď zahrnovat token sdíleného přístupového podpisu (SAS), nebo musí být veřejně přístupný jako v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="2e224-199">Additionally, the source blob specified in the `--source-uri` argument must either include a shared access signature (SAS) token, or be publicly accessible, as in this example.</span></span>

### <a name="delete-a-blob"></a><span data-ttu-id="2e224-200">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="2e224-200">Delete a blob</span></span>
<span data-ttu-id="2e224-201">Pokud chcete odstranit objekt blob, použijte `blob delete` příkaz:</span><span class="sxs-lookup"><span data-stu-id="2e224-201">To delete a blob, use the `blob delete` command:</span></span>

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a><span data-ttu-id="2e224-202">Vytvořit a spravovat sdílené složky</span><span class="sxs-lookup"><span data-stu-id="2e224-202">Create and manage file shares</span></span>
<span data-ttu-id="2e224-203">Azure File storage nabízí sdílené úložiště pro aplikace, které používají protokol Server Message Block (SMB).</span><span class="sxs-lookup"><span data-stu-id="2e224-203">Azure File storage offers shared storage for applications using the Server Message Block (SMB) protocol.</span></span> <span data-ttu-id="2e224-204">Virtuální počítače Microsoft Azure a cloudové služby i místní aplikace můžou sdílet souborová data přes sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="2e224-204">Microsoft Azure virtual machines and cloud services, as well as on-premises applications, can share file data via mounted shares.</span></span> <span data-ttu-id="2e224-205">Můžete spravovat sdílené složky a data souborů prostřednictvím rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2e224-205">You can manage file shares and file data via the Azure CLI.</span></span> <span data-ttu-id="2e224-206">Další informace o Azure File storage najdete v tématu [Začínáme s Azure File storage ve Windows](storage-dotnet-how-to-use-files.md) nebo [postup používání Azure File storage s Linuxem](storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2e224-206">For more information on Azure File storage, see [Get started with Azure File storage on Windows](storage-dotnet-how-to-use-files.md) or [How to use Azure File storage with Linux](storage-how-to-use-files-linux.md).</span></span>

### <a name="create-a-file-share"></a><span data-ttu-id="2e224-207">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="2e224-207">Create a file share</span></span>
<span data-ttu-id="2e224-208">Azure File sdílená složka je sdílená složka SMB v Azure.</span><span class="sxs-lookup"><span data-stu-id="2e224-208">An Azure File share is an SMB file share in Azure.</span></span> <span data-ttu-id="2e224-209">Všechny adresáře a soubory musí být vytvořeny ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="2e224-209">All directories and files must be created in a file share.</span></span> <span data-ttu-id="2e224-210">Účet může obsahovat neomezený počet sdílených složek a sdílená složka můžete obsahovat neomezený počet souborů až limity kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e224-210">An account can contain an unlimited number of shares, and a share can store an unlimited number of files, up to the capacity limits of the storage account.</span></span> <span data-ttu-id="2e224-211">Následující příklad vytvoří sdílenou složku s názvem **název_sdílené_položky**.</span><span class="sxs-lookup"><span data-stu-id="2e224-211">The following example creates a file share named **myshare**.</span></span>

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a><span data-ttu-id="2e224-212">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="2e224-212">Create a directory</span></span>
<span data-ttu-id="2e224-213">Adresář poskytuje hierarchické uspořádání sdílenou složku Azure.</span><span class="sxs-lookup"><span data-stu-id="2e224-213">A directory provides a hierarchical structure in an Azure file share.</span></span> <span data-ttu-id="2e224-214">Následující příklad vytvoří adresář s názvem **adresář** do sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="2e224-214">The following example creates a directory named **myDir** in the file share.</span></span>

```azurecli
az storage directory create --name myDir --share-name myshare
```

<span data-ttu-id="2e224-215">Cestu k adresáři může obsahovat více úrovních, například **dir1/dir2**.</span><span class="sxs-lookup"><span data-stu-id="2e224-215">A directory path can include multiple levels, for example **dir1/dir2**.</span></span> <span data-ttu-id="2e224-216">Nicméně je nutné zajistit, že před vytvořením podadresáři existují všechny nadřazeného adresáře.</span><span class="sxs-lookup"><span data-stu-id="2e224-216">However, you must ensure that all parent directories exist before creating a subdirectory.</span></span> <span data-ttu-id="2e224-217">Například pro cestu **dir1/dir2**, je nutné nejprve vytvořit adresář **dir1**, pak vytvořit adresář **dir2**.</span><span class="sxs-lookup"><span data-stu-id="2e224-217">For example, for path **dir1/dir2**, you must first create directory **dir1**, then create directory **dir2**.</span></span>

### <a name="upload-a-local-file-to-a-share"></a><span data-ttu-id="2e224-218">Uložení místního souboru do sdílené složky</span><span class="sxs-lookup"><span data-stu-id="2e224-218">Upload a local file to a share</span></span>
<span data-ttu-id="2e224-219">V následujícím příkladu se uloží soubor z **~/temp/samplefile.txt** kořenového adresáře **název_sdílené_položky** sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="2e224-219">The following example uploads a file from **~/temp/samplefile.txt** to root of the **myshare** file share.</span></span> <span data-ttu-id="2e224-220">`--source` Argument určuje existující místní soubor k odeslání.</span><span class="sxs-lookup"><span data-stu-id="2e224-220">The `--source` argument specifies the existing local file to upload.</span></span>

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

<span data-ttu-id="2e224-221">Jako s vytváření adresáře, můžete určit cestu k adresáři ve sdílené složce nahrát soubor do existujícího adresáře ve sdílené složce:</span><span class="sxs-lookup"><span data-stu-id="2e224-221">As with directory creation, you can specify a directory path within the share to upload the file to an existing directory within the share:</span></span>

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

<span data-ttu-id="2e224-222">Soubor ve sdílené složce může být velikost až 1 TB.</span><span class="sxs-lookup"><span data-stu-id="2e224-222">A file in the share can be up to 1 TB in size.</span></span>

### <a name="list-the-files-in-a-share"></a><span data-ttu-id="2e224-223">Seznam souborů ve sdílené složce</span><span class="sxs-lookup"><span data-stu-id="2e224-223">List the files in a share</span></span>
<span data-ttu-id="2e224-224">Můžete vytvořit seznam souborů a adresářů ve sdílené složce pomocí `az storage file list` příkaz:</span><span class="sxs-lookup"><span data-stu-id="2e224-224">You can list files and directories in a share by using the `az storage file list` command:</span></span>

```azurecli
# List the files in the root of a share
az storage file list --share-name myshare --output table

# List the files in a directory within a share
az storage file list --share-name myshare/myDir --output table

# List the files in a path within a share
az storage file list --share-name myshare --path myDir/mySubDir/MySubDir2 --output table
```

### <a name="copy-files"></a><span data-ttu-id="2e224-225">Kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="2e224-225">Copy files</span></span>      
<span data-ttu-id="2e224-226">Soubor můžete zkopírovat do jiného souboru, soubor do objektu blob, nebo objekt blob do souboru.</span><span class="sxs-lookup"><span data-stu-id="2e224-226">You can copy a file to another file, a file to a blob, or a blob to a file.</span></span> <span data-ttu-id="2e224-227">Chcete-li například kopírování souboru do jiného adresáře v jinou sdílenou složku:</span><span class="sxs-lookup"><span data-stu-id="2e224-227">For example, to copy a file to a directory in a different share:</span></span>        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="next-steps"></a><span data-ttu-id="2e224-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e224-228">Next steps</span></span>
<span data-ttu-id="2e224-229">Tady jsou některé další zdroje informací o další informace o práci s Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="2e224-229">Here are some additional resources for learning more about working with the Azure CLI 2.0.</span></span>

* [<span data-ttu-id="2e224-230">Začínáme s Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2e224-230">Get started with Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [<span data-ttu-id="2e224-231">Přehled příkazů Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2e224-231">Azure CLI 2.0 command reference</span></span>](/cli/azure)
* [<span data-ttu-id="2e224-232">Azure CLI 2.0 na Githubu</span><span class="sxs-lookup"><span data-stu-id="2e224-232">Azure CLI 2.0 on GitHub</span></span>](https://github.com/Azure/azure-cli)
