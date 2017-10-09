---
title: "aaaUsing hello 2.0 rozhraní příkazového řádku Azure s Azure Storage | Microsoft Docs"
description: "Zjistěte, jak toouse hello rozhraní příkazového řádku Azure (Azure CLI) 2.0, s toocreate Azure Storage a spravovat účty úložiště a pracovat s Azure BLOB a soubory. Hello Azure CLI 2.0 je nástroj napříč platformami napsané v Pythonu."
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
ms.openlocfilehash: 14e6eb0c913676380c90a72563276245e7f08aa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-20-with-azure-storage"></a><span data-ttu-id="529d6-104">Použití hello 2.0 rozhraní příkazového řádku Azure s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="529d6-104">Using hello Azure CLI 2.0 with Azure Storage</span></span>

<span data-ttu-id="529d6-105">Hello open source, a platformy Azure CLI 2.0 obsahuje sadu příkazů pro práci s hello platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="529d6-105">hello open-source, cross-platform Azure CLI 2.0 provides a set of commands for working with hello Azure platform.</span></span> <span data-ttu-id="529d6-106">Poskytuje mnohem hello stejné funkce najít v hello [portál Azure](https://portal.azure.com), včetně přístupu bohaté data.</span><span class="sxs-lookup"><span data-stu-id="529d6-106">It provides much of hello same functionality found in hello [Azure portal](https://portal.azure.com), including rich data access.</span></span>

<span data-ttu-id="529d6-107">V této příručce, jsme ukazují, jak toouse hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooperform několik úloh práci s prostředky v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="529d6-107">In this guide, we show you how toouse hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooperform several tasks working with resources in your Azure Storage account.</span></span> <span data-ttu-id="529d6-108">Doporučujeme stáhnout a nainstalovat nebo upgradovat toohello nejnovější verzi 2.0 rozhraní příkazového řádku hello před použitím tohoto průvodce.</span><span class="sxs-lookup"><span data-stu-id="529d6-108">We recommend that you download and install or upgrade toohello latest version of hello CLI 2.0 before using this guide.</span></span>

<span data-ttu-id="529d6-109">Příklady Hello v příručce hello předpokládají použití hello hello prostředí Bash na Ubuntu, ale jiné platformy proveďte podobně.</span><span class="sxs-lookup"><span data-stu-id="529d6-109">hello examples in hello guide assume hello use of hello Bash shell on Ubuntu, but other platforms should perform similarly.</span></span> 

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a><span data-ttu-id="529d6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="529d6-110">Prerequisites</span></span>
<span data-ttu-id="529d6-111">Tato příručka předpokládá, že chápete základní koncepty hello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="529d6-111">This guide assumes that you understand hello basic concepts of Azure Storage.</span></span> <span data-ttu-id="529d6-112">Předpokládá také, že jste možnost toosatisfy hello účet vytvoření požadavky, které jsou uvedeny níže Azure a hello služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="529d6-112">It also assumes that you're able toosatisfy hello account creation requirements that are specified below for Azure and hello Storage service.</span></span>

### <a name="accounts"></a><span data-ttu-id="529d6-113">Účty</span><span class="sxs-lookup"><span data-stu-id="529d6-113">Accounts</span></span>
* <span data-ttu-id="529d6-114">**Účet Azure**: Pokud ještě nemáte předplatné Azure, [vytvořit bezplatný účet Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="529d6-114">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="529d6-115">**Účet Storage**: Viz část [Vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech Azure Storage](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="529d6-115">**Storage account**: See [Create a storage account](storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](storage-create-storage-account.md).</span></span>

### <a name="install-hello-azure-cli-20"></a><span data-ttu-id="529d6-116">Nainstalujte hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="529d6-116">Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="529d6-117">Stáhněte a nainstalujte hello 2.0 rozhraní příkazového řádku Azure podle pokynů hello uvedených v [nainstalovat Azure CLI 2.0](/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="529d6-117">Download and install hello Azure CLI 2.0 by following hello instructions outlined in [Install Azure CLI 2.0](/cli/azure/install-az-cli2).</span></span>

> [!TIP]
> <span data-ttu-id="529d6-118">Pokud máte potíže s instalací hello, podívejte se na hello [řešení potíží s instalací](/cli/azure/install-az-cli2#installation-troubleshooting) části článku hello a hello [nainstalovat řešení potíží s](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) Průvodce na Githubu.</span><span class="sxs-lookup"><span data-stu-id="529d6-118">If you have trouble with hello installation, check out hello [Installation Troubleshooting](/cli/azure/install-az-cli2#installation-troubleshooting) section of hello article, and hello [Install Troubleshooting](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) guide on GitHub.</span></span>
>

## <a name="working-with-hello-cli"></a><span data-ttu-id="529d6-119">Práce s hello rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="529d6-119">Working with hello CLI</span></span>

<span data-ttu-id="529d6-120">Po instalaci hello rozhraní příkazového řádku, můžete hello `az` příkazu v příkazech rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) hello tooaccess rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="529d6-120">Once you've installed hello CLI, you can use hello `az` command in your command-line interface (Bash, Terminal, Command Prompt) tooaccess hello Azure CLI commands.</span></span> <span data-ttu-id="529d6-121">Typ hello `az` příkaz toosee úplný seznam hello základní příkazy (hello následující příklad výstupu byla zkrácena):</span><span class="sxs-lookup"><span data-stu-id="529d6-121">Type hello `az` command toosee a full list of hello base commands (hello following example output has been truncated):</span></span>

```
     /\
    /  \    _____   _ _ __ ___
   / /\ \  |_  / | | | \'__/ _ \
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome toohello cool new Azure CLI!

Here are hello base commands:

    account          : Manage subscriptions.
    acr              : Manage Azure container registries.
    acs              : Manage Azure Container Services.
    ad               : Synchronize on-premises directories and manage Azure Active Directory
                       resources.
    ...
```

<span data-ttu-id="529d6-122">Ve svém rozhraní příkazového řádku spusťte příkaz hello `az storage --help` toolist hello `storage` příkaz podskupiny.</span><span class="sxs-lookup"><span data-stu-id="529d6-122">In your command-line interface, execute hello command `az storage --help` toolist hello `storage` command subgroups.</span></span> <span data-ttu-id="529d6-123">popisy Hello hello podskupiny poskytovat přehled o hello hello funkce, rozhraní příkazového řádku Azure poskytuje pro práci s vaše prostředky úložiště.</span><span class="sxs-lookup"><span data-stu-id="529d6-123">hello descriptions of hello subgroups provide an overview of hello functionality hello Azure CLI provides for working with your storage resources.</span></span>

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
    file     : File shares that use hello standard SMB 3.0 protocol.
    logging  : Manage Storage service logging information.
    message  : Manage queue storage messages.
    metrics  : Manage Storage service metrics.
    queue    : Use queues tooeffectively scale applications according tootraffic.
    share    : Manage file shares.
    table    : NoSQL key-value storage using semi-structured datasets.
```

## <a name="connect-hello-cli-tooyour-azure-subscription"></a><span data-ttu-id="529d6-124">Připojení rozhraní příkazového řádku tooyour hello předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="529d6-124">Connect hello CLI tooyour Azure subscription</span></span>

<span data-ttu-id="529d6-125">toowork s hello prostředky ve vašem předplatném Azure, musíte nejprve se přihlásit tooyour účet Azure s `az login`.</span><span class="sxs-lookup"><span data-stu-id="529d6-125">toowork with hello resources in your Azure subscription, you must first log in tooyour Azure account with `az login`.</span></span> <span data-ttu-id="529d6-126">Existuje několik způsobů, které se můžete přihlásit:</span><span class="sxs-lookup"><span data-stu-id="529d6-126">There are several ways you can log in:</span></span>

* <span data-ttu-id="529d6-127">**Interaktivní přihlášení**:`az login`</span><span class="sxs-lookup"><span data-stu-id="529d6-127">**Interactive login**: `az login`</span></span>
* <span data-ttu-id="529d6-128">**Přihlaste se pomocí uživatelského jména a hesla**:`az login -u johndoe@contoso.com -p VerySecret`</span><span class="sxs-lookup"><span data-stu-id="529d6-128">**Log in with user name and password**: `az login -u johndoe@contoso.com -p VerySecret`</span></span>
  * <span data-ttu-id="529d6-129">To nebude fungovat s účty Microsoft nebo účty, které používají službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="529d6-129">This doesn't work with Microsoft accounts or accounts that use multi-factor authentication.</span></span>
* <span data-ttu-id="529d6-130">**Přihlaste se pomocí objektu služby**:`az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="529d6-130">**Log in with a service principal**: `az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`</span></span>

## <a name="azure-cli-20-sample-script"></a><span data-ttu-id="529d6-131">Azure CLI 2.0 ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="529d6-131">Azure CLI 2.0 sample script</span></span>

<span data-ttu-id="529d6-132">V dalším kroku jsme budete pracovat se skript malé prostředí, který vystavuje pár základních toointeract příkazy Azure CLI 2.0 s prostředky Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="529d6-132">Next, we'll work with a small shell script that issues a few basic Azure CLI 2.0 commands toointeract with Azure Storage resources.</span></span> <span data-ttu-id="529d6-133">skript Hello nejprve vytvoří nový kontejner ve vašem účtu úložiště, a poté odešle existující kontejner toothat souboru (jako objekt blob).</span><span class="sxs-lookup"><span data-stu-id="529d6-133">hello script first creates a new container in your storage account, then uploads an existing file (as a blob) toothat container.</span></span> <span data-ttu-id="529d6-134">Potom zobrazí seznam všech objektů BLOB v kontejneru hello a nakonec stáhne cíl tooa hello souboru v místním počítači, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="529d6-134">It then lists all blobs in hello container, and finally, downloads hello file tooa destination on your local computer that you specify.</span></span>

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

export container_name=<container_name>
export blob_name=<blob_name>
export file_to_upload=<file_to_upload>
export destination_file=<destination_file>

echo "Creating hello container..."
az storage container create --name $container_name

echo "Uploading hello file..."
az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

echo "Listing hello blobs..."
az storage blob list --container-name $container_name --output table

echo "Downloading hello file..."
az storage blob download --container-name $container_name --name $blob_name --file $destination_file --output table

echo "Done"
```

<span data-ttu-id="529d6-135">**Nakonfigurujte a spusťte skript hello**</span><span class="sxs-lookup"><span data-stu-id="529d6-135">**Configure and run hello script**</span></span>

1. <span data-ttu-id="529d6-136">Otevřete svém oblíbeném textovém editoru, pak zkopírujte a vložte hello předcházející skriptu do editoru hello.</span><span class="sxs-lookup"><span data-stu-id="529d6-136">Open your favorite text editor, then copy and paste hello preceding script into hello editor.</span></span>

2. <span data-ttu-id="529d6-137">Potom aktualizujte hello skriptu proměnné tooreflect nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="529d6-137">Next, update hello script's variables tooreflect your configuration settings.</span></span> <span data-ttu-id="529d6-138">Nahraďte následující hodnoty jako zadaný hello:</span><span class="sxs-lookup"><span data-stu-id="529d6-138">Replace hello following values as specified:</span></span>

   * <span data-ttu-id="529d6-139">**\<storage_account_name\>**  hello název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="529d6-139">**\<storage_account_name\>** hello name of your storage account.</span></span>
   * <span data-ttu-id="529d6-140">**\<storage_account_key\>**  hello primární nebo sekundární přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="529d6-140">**\<storage_account_key\>** hello primary or secondary access key for your storage account.</span></span>
   * <span data-ttu-id="529d6-141">**\<container_name\>**  název hello nový kontejner toocreate, jako je například "azure-rozhraní příkazového řádku – ukázka container".</span><span class="sxs-lookup"><span data-stu-id="529d6-141">**\<container_name\>** A name hello new container toocreate, such as "azure-cli-sample-container".</span></span>
   * <span data-ttu-id="529d6-142">**\<blob_name\>**  název hello cílový objekt blob v kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="529d6-142">**\<blob_name\>** A name for hello destination blob in hello container.</span></span>
   * <span data-ttu-id="529d6-143">**\<file_to_upload\>**  hello toosmall cestě k souboru v místním počítači, jako například "~ / images/HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="529d6-143">**\<file_to_upload\>** hello path toosmall file on your local computer, such as "~/images/HelloWorld.png".</span></span>
   * <span data-ttu-id="529d6-144">**\<destination_file\>**  hello cílová cesta souboru, jako například "~ / downloadedImage.png".</span><span class="sxs-lookup"><span data-stu-id="529d6-144">**\<destination_file\>** hello destination file path, such as "~/downloadedImage.png".</span></span>

3. <span data-ttu-id="529d6-145">Po aktualizaci hello nezbytné proměnné, uložte skript hello a ukončete editor.</span><span class="sxs-lookup"><span data-stu-id="529d6-145">After you've updated hello necessary variables, save hello script and exit your editor.</span></span> <span data-ttu-id="529d6-146">Další kroky Hello předpokládá, že jste s názvem vašeho skriptu **my_storage_sample.sh**.</span><span class="sxs-lookup"><span data-stu-id="529d6-146">hello next steps assume you've named your script **my_storage_sample.sh**.</span></span>

4. <span data-ttu-id="529d6-147">Označte hello skriptu jako spustitelný soubor, v případě potřeby:`chmod +x my_storage_sample.sh`</span><span class="sxs-lookup"><span data-stu-id="529d6-147">Mark hello script as executable, if necessary: `chmod +x my_storage_sample.sh`</span></span>

5. <span data-ttu-id="529d6-148">Spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="529d6-148">Execute hello script.</span></span> <span data-ttu-id="529d6-149">Například v Bash:`./my_storage_sample.sh`</span><span class="sxs-lookup"><span data-stu-id="529d6-149">For example, in Bash: `./my_storage_sample.sh`</span></span>

<span data-ttu-id="529d6-150">Měli vidět výstup podobný toohello následující a hello  **\<destination_file\>**  jste zadali v hello skriptu by se měla objevit v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="529d6-150">You should see output similar toohello following, and hello **\<destination_file\>** you specified in hello script should appear on your local computer.</span></span>

```
Creating hello container...
{
  "created": true
}
Uploading hello file...
Percent complete: %100.0
Listing hello blobs...
Name       Blob Type      Length  Content Type              Last Modified
---------  -----------  --------  ------------------------  -------------------------
README.md  BlockBlob        6700  application/octet-stream  2017-05-12T20:54:59+00:00
Downloading hello file...
Name
---------
README.md
Done
```

> [!TIP]
> <span data-ttu-id="529d6-151">Hello předchozí výstup je v **tabulky** formátu.</span><span class="sxs-lookup"><span data-stu-id="529d6-151">hello preceding output is in **table** format.</span></span> <span data-ttu-id="529d6-152">Můžete určit, který výstupní formát toouse zadáním hello `--output` argument v příkazech rozhraní příkazového řádku, nebo nastavit globálně pomocí `az configure`.</span><span class="sxs-lookup"><span data-stu-id="529d6-152">You can specify which output format toouse by specifying hello `--output` argument in your CLI commands, or set it globally using `az configure`.</span></span>
>

## <a name="manage-storage-accounts"></a><span data-ttu-id="529d6-153">Správa účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="529d6-153">Manage storage accounts</span></span>

### <a name="create-a-new-storage-account"></a><span data-ttu-id="529d6-154">Vytvořit nový účet úložiště</span><span class="sxs-lookup"><span data-stu-id="529d6-154">Create a new storage account</span></span>
<span data-ttu-id="529d6-155">toouse Azure Storage, potřebujete účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="529d6-155">toouse Azure Storage, you need a storage account.</span></span> <span data-ttu-id="529d6-156">Můžete vytvořit nový účet úložiště Azure, po dokončení konfigurace počítače příliš[připojení odběru tooyour](#connect-to-your-azure-subscription).</span><span class="sxs-lookup"><span data-stu-id="529d6-156">You can create a new Azure Storage account after you've configured your computer too[connect tooyour subscription](#connect-to-your-azure-subscription).</span></span>

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* <span data-ttu-id="529d6-157">`--location`[Vyžaduje]: umístění.</span><span class="sxs-lookup"><span data-stu-id="529d6-157">`--location` [Required]: Location.</span></span> <span data-ttu-id="529d6-158">Například "západní USA".</span><span class="sxs-lookup"><span data-stu-id="529d6-158">For example, "West US".</span></span>
* <span data-ttu-id="529d6-159">`--name`[Vyžaduje]: název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="529d6-159">`--name` [Required]: hello storage account name.</span></span> <span data-ttu-id="529d6-160">Hello název musí mít délku 3 too24 znaků a používat jenom malé alfanumerické znaky.</span><span class="sxs-lookup"><span data-stu-id="529d6-160">hello name must be 3 too24 characters in length, and use only lowercase alphanumeric characters.</span></span>
* <span data-ttu-id="529d6-161">`--resource-group`[Vyžaduje]: název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="529d6-161">`--resource-group` [Required]: Name of resource group.</span></span>
* <span data-ttu-id="529d6-162">`--sku`[Vyžaduje]: hello účet úložiště SKU.</span><span class="sxs-lookup"><span data-stu-id="529d6-162">`--sku` [Required]: hello storage account SKU.</span></span> <span data-ttu-id="529d6-163">Povolené hodnoty:</span><span class="sxs-lookup"><span data-stu-id="529d6-163">Allowed values:</span></span>
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a><span data-ttu-id="529d6-164">Nastavení proměnných prostředí výchozí účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="529d6-164">Set default Azure storage account environment variables</span></span>
<span data-ttu-id="529d6-165">Můžete mít více účtů úložiště ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="529d6-165">You can have multiple storage accounts in your Azure subscription.</span></span> <span data-ttu-id="529d6-166">tooselect jeden z nich příkazy toouse pro všechny následné úložiště, můžete nastavit tyto proměnné prostředí:</span><span class="sxs-lookup"><span data-stu-id="529d6-166">tooselect one of them toouse for all subsequent storage commands, you can set these environment variables:</span></span>

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

<span data-ttu-id="529d6-167">Jiný způsob tooset výchozí účet úložiště je pomocí připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="529d6-167">Another way tooset a default storage account is by using a connection string.</span></span> <span data-ttu-id="529d6-168">Nejdřív získat hello připojovací řetězec s hello `show-connection-string` příkaz:</span><span class="sxs-lookup"><span data-stu-id="529d6-168">First, get hello connection string with hello `show-connection-string` command:</span></span>

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

<span data-ttu-id="529d6-169">Potom kopie hello výstup připojovací řetězec a nastavte hello `AZURE_STORAGE_CONNECTION_STRING` proměnnou prostředí (může být nutné tooenclose hello připojovací řetězec v uvozovkách):</span><span class="sxs-lookup"><span data-stu-id="529d6-169">Then copy hello output connection string and set hello `AZURE_STORAGE_CONNECTION_STRING` environment variable (you might need tooenclose hello connection string in quotes):</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> <span data-ttu-id="529d6-170">Všechny příklady v následující části tohoto článku hello předpokládají, že jste nastavili hello `AZURE_STORAGE_ACCOUNT` a `AZURE_STORAGE_ACCESS_KEY` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="529d6-170">All examples in hello following sections of this article assume that you've set hello `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_ACCESS_KEY` environment variables.</span></span>
>

## <a name="create-and-manage-blobs"></a><span data-ttu-id="529d6-171">Vytvářet a spravovat objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="529d6-171">Create and manage blobs</span></span>
<span data-ttu-id="529d6-172">Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která je přístupná z kdekoli v hello, world pomocí protokolů HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="529d6-172">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="529d6-173">V této části se předpokládá, že jste již obeznámeni s koncepty úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="529d6-173">This section assumes that you are already familiar with Azure Blob storage concepts.</span></span> <span data-ttu-id="529d6-174">Podrobné informace najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../blobs/storage-dotnet-how-to-use-blobs.md) a [koncepty služby objektů Blob](/rest/api/storageservices/blob-service-concepts).</span><span class="sxs-lookup"><span data-stu-id="529d6-174">For detailed information, see [Get started with Azure Blob storage using .NET](../blobs/storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](/rest/api/storageservices/blob-service-concepts).</span></span>

### <a name="create-a-container"></a><span data-ttu-id="529d6-175">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="529d6-175">Create a container</span></span>
<span data-ttu-id="529d6-176">Každý objekt blob v úložišti Azure musí být v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="529d6-176">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="529d6-177">Kontejner můžete vytvořit pomocí hello `az storage container create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="529d6-177">You can create a container by using hello `az storage container create` command:</span></span>

```azurecli
az storage container create --name <container_name>
```

<span data-ttu-id="529d6-178">Můžete nastavit jednu ze tří úrovní přístupu pro čtení pro nový kontejner zadáním hello volitelné `--public-access` argument:</span><span class="sxs-lookup"><span data-stu-id="529d6-178">You can set one of three levels of read access for a new container by specifying hello optional  `--public-access` argument:</span></span>

* <span data-ttu-id="529d6-179">`off`(výchozí): datový kontejner je privátní toohello vlastníka účtu.</span><span class="sxs-lookup"><span data-stu-id="529d6-179">`off` (default): Container data is private toohello account owner.</span></span>
* <span data-ttu-id="529d6-180">`blob`: Pro objekty BLOB veřejný přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="529d6-180">`blob`: Public read access for blobs.</span></span>
* <span data-ttu-id="529d6-181">`container`: Veřejné pro čtení a seznamu přístup toohello celého kontejneru.</span><span class="sxs-lookup"><span data-stu-id="529d6-181">`container`: Public read and list access toohello entire container.</span></span>

<span data-ttu-id="529d6-182">Další informace najdete v tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](../blobs/storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="529d6-182">For more information, see [Manage anonymous read access toocontainers and blobs](../blobs/storage-manage-access-to-resources.md).</span></span>

### <a name="upload-a-blob-tooa-container"></a><span data-ttu-id="529d6-183">Nahrát tooa kontejneru objektů blob</span><span class="sxs-lookup"><span data-stu-id="529d6-183">Upload a blob tooa container</span></span>
<span data-ttu-id="529d6-184">Podporuje Azure úložiště objektů Blob bloku, připojit a objekty BLOB stránky.</span><span class="sxs-lookup"><span data-stu-id="529d6-184">Azure Blob storage supports block, append, and page blobs.</span></span> <span data-ttu-id="529d6-185">Nahrávání objekty BLOB kontejneru tooa pomocí hello `blob upload` příkaz:</span><span class="sxs-lookup"><span data-stu-id="529d6-185">Upload blobs tooa container by using hello `blob upload` command:</span></span>

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

 <span data-ttu-id="529d6-186">Ve výchozím nastavení, hello `blob upload` příkaz odešle objekty BLOB toopage *.vhd soubory nebo objekty BLOB bloku, jinak hodnota.</span><span class="sxs-lookup"><span data-stu-id="529d6-186">By default, hello `blob upload` command uploads *.vhd files toopage blobs, or block blobs otherwise.</span></span> <span data-ttu-id="529d6-187">toospecify, jiné při zadejte nahrát objekt blob, můžete použít hello `--type` argument – povolené hodnoty jsou `append`, `block`, a `page`.</span><span class="sxs-lookup"><span data-stu-id="529d6-187">toospecify another type when you upload a blob, you can use hello `--type` argument--allowed values are `append`, `block`, and `page`.</span></span>

 <span data-ttu-id="529d6-188">Další informace o typech hello jiný objektu blob, najdete v části [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span><span class="sxs-lookup"><span data-stu-id="529d6-188">For more information on hello different blob types, see [Understanding Block Blobs, Append Blobs, and Page Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span></span>


### <a name="download-a-blob-from-a-container"></a><span data-ttu-id="529d6-189">Stažení objektu blob z kontejneru</span><span class="sxs-lookup"><span data-stu-id="529d6-189">Download a blob from a container</span></span>
<span data-ttu-id="529d6-190">Tento příklad ukazuje, jak toodownload objekt blob z kontejneru:</span><span class="sxs-lookup"><span data-stu-id="529d6-190">This example demonstrates how toodownload a blob from a container:</span></span>

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="529d6-191">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="529d6-191">List hello blobs in a container</span></span>

<span data-ttu-id="529d6-192">Seznam hello objektů BLOB v kontejneru s hello [az úložiště objektů blob seznamu](/cli/azure/storage/blob#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="529d6-192">List hello blobs in a container with hello [az storage blob list](/cli/azure/storage/blob#list) command.</span></span>

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a><span data-ttu-id="529d6-193">Kopírování objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="529d6-193">Copy blobs</span></span>
<span data-ttu-id="529d6-194">Můžete asynchronně kopírovat objekty blob v rámci účtů a oblastí nebo napříč nimi.</span><span class="sxs-lookup"><span data-stu-id="529d6-194">You can copy blobs within or across storage accounts and regions asynchronously.</span></span>

<span data-ttu-id="529d6-195">Hello následující příklad ukazuje, jak toocopy objekty BLOB z jednoho úložiště účet tooanother.</span><span class="sxs-lookup"><span data-stu-id="529d6-195">hello following example demonstrates how toocopy blobs from one storage account tooanother.</span></span> <span data-ttu-id="529d6-196">Nejdříve vytvoříme kontejner v účtu úložiště zdroj hello, zadání veřejný přístup pro čtení pro jeho objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="529d6-196">We first create a container in hello source storage account, specifying public read-access for its blobs.</span></span> <span data-ttu-id="529d6-197">V dalším kroku jsme nahrát kontejner toohello souborů a nakonec kopie hello objektů blob z tohoto kontejneru do kontejneru v hello cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="529d6-197">Next, we upload a file toohello container, and finally, copy hello blob from that container into a container in hello destination storage account.</span></span>

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob toocontainer in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/Pictures/sourcefile.png \
    --name sourcefile.png

# Copy blob from source account toodestination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

<span data-ttu-id="529d6-198">V hello výše příklad hello cílový kontejner již musí existovat v hello cílový účet úložiště pro toosucceed operace kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="529d6-198">In hello above example, hello destination container must already exist in hello destination storage account for hello copy operation toosucceed.</span></span> <span data-ttu-id="529d6-199">Kromě toho hello zdrojový objekt blob zadaný v hello `--source-uri` argument musí buď zahrnují token sdílený přístupový podpis (SAS), nebo veřejně přístupný, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="529d6-199">Additionally, hello source blob specified in hello `--source-uri` argument must either include a shared access signature (SAS) token, or be publicly accessible, as in this example.</span></span>

### <a name="delete-a-blob"></a><span data-ttu-id="529d6-200">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="529d6-200">Delete a blob</span></span>
<span data-ttu-id="529d6-201">toodelete objekt blob, použijte hello `blob delete` příkaz:</span><span class="sxs-lookup"><span data-stu-id="529d6-201">toodelete a blob, use hello `blob delete` command:</span></span>

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a><span data-ttu-id="529d6-202">Vytvořit a spravovat sdílené složky</span><span class="sxs-lookup"><span data-stu-id="529d6-202">Create and manage file shares</span></span>
<span data-ttu-id="529d6-203">Azure File storage nabízí sdílené úložiště pro aplikace, které používají hello zpráva bloku protokol Server (SMB).</span><span class="sxs-lookup"><span data-stu-id="529d6-203">Azure File storage offers shared storage for applications using hello Server Message Block (SMB) protocol.</span></span> <span data-ttu-id="529d6-204">Virtuální počítače Microsoft Azure a cloudové služby i místní aplikace můžou sdílet souborová data přes sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="529d6-204">Microsoft Azure virtual machines and cloud services, as well as on-premises applications, can share file data via mounted shares.</span></span> <span data-ttu-id="529d6-205">Můžete spravovat sdílené složky a data souborů prostřednictvím hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="529d6-205">You can manage file shares and file data via hello Azure CLI.</span></span> <span data-ttu-id="529d6-206">Další informace o Azure File storage najdete v tématu [Začínáme s Azure File storage ve Windows](../storage-dotnet-how-to-use-files.md) nebo [jak toouse Azure File storage s Linuxem](../storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="529d6-206">For more information on Azure File storage, see [Get started with Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) or [How toouse Azure File storage with Linux](../storage-how-to-use-files-linux.md).</span></span>

### <a name="create-a-file-share"></a><span data-ttu-id="529d6-207">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="529d6-207">Create a file share</span></span>
<span data-ttu-id="529d6-208">Azure File sdílená složka je sdílená složka SMB v Azure.</span><span class="sxs-lookup"><span data-stu-id="529d6-208">An Azure File share is an SMB file share in Azure.</span></span> <span data-ttu-id="529d6-209">Všechny adresáře a soubory musí být vytvořeny ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="529d6-209">All directories and files must be created in a file share.</span></span> <span data-ttu-id="529d6-210">Účet může obsahovat neomezený počet sdílených složek a sdílená složka můžete obsahovat neomezený počet souborů, až toohello limity kapacity účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="529d6-210">An account can contain an unlimited number of shares, and a share can store an unlimited number of files, up toohello capacity limits of hello storage account.</span></span> <span data-ttu-id="529d6-211">Hello následující příklad vytvoří sdílenou složku s názvem **název_sdílené_položky**.</span><span class="sxs-lookup"><span data-stu-id="529d6-211">hello following example creates a file share named **myshare**.</span></span>

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a><span data-ttu-id="529d6-212">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="529d6-212">Create a directory</span></span>
<span data-ttu-id="529d6-213">Adresář poskytuje hierarchické uspořádání sdílenou složku Azure.</span><span class="sxs-lookup"><span data-stu-id="529d6-213">A directory provides a hierarchical structure in an Azure file share.</span></span> <span data-ttu-id="529d6-214">Hello následující příklad vytvoří adresář s názvem **adresář** v hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="529d6-214">hello following example creates a directory named **myDir** in hello file share.</span></span>

```azurecli
az storage directory create --name myDir --share-name myshare
```

<span data-ttu-id="529d6-215">Cestu k adresáři může obsahovat více úrovních, například **dir1/dir2**.</span><span class="sxs-lookup"><span data-stu-id="529d6-215">A directory path can include multiple levels, for example **dir1/dir2**.</span></span> <span data-ttu-id="529d6-216">Nicméně je nutné zajistit, že před vytvořením podadresáři existují všechny nadřazeného adresáře.</span><span class="sxs-lookup"><span data-stu-id="529d6-216">However, you must ensure that all parent directories exist before creating a subdirectory.</span></span> <span data-ttu-id="529d6-217">Například pro cestu **dir1/dir2**, je nutné nejprve vytvořit adresář **dir1**, pak vytvořit adresář **dir2**.</span><span class="sxs-lookup"><span data-stu-id="529d6-217">For example, for path **dir1/dir2**, you must first create directory **dir1**, then create directory **dir2**.</span></span>

### <a name="upload-a-local-file-tooa-share"></a><span data-ttu-id="529d6-218">Nahrát tooa místní sdílené složky</span><span class="sxs-lookup"><span data-stu-id="529d6-218">Upload a local file tooa share</span></span>
<span data-ttu-id="529d6-219">Hello následujícím příkladu se uloží soubor z **~/temp/samplefile.txt** tooroot hello **název_sdílené_položky** sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="529d6-219">hello following example uploads a file from **~/temp/samplefile.txt** tooroot of hello **myshare** file share.</span></span> <span data-ttu-id="529d6-220">Hello `--source` argument určuje hello existující tooupload místního souboru.</span><span class="sxs-lookup"><span data-stu-id="529d6-220">hello `--source` argument specifies hello existing local file tooupload.</span></span>

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

<span data-ttu-id="529d6-221">Stejně jako u vytváření adresáře, můžete určit cestu k adresáři v rámci hello sdílenou složku tooupload hello tooan existující adresář souboru ve složce hello:</span><span class="sxs-lookup"><span data-stu-id="529d6-221">As with directory creation, you can specify a directory path within hello share tooupload hello file tooan existing directory within hello share:</span></span>

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

<span data-ttu-id="529d6-222">Soubor ve sdílené složce hello může být až velikost too1 TB.</span><span class="sxs-lookup"><span data-stu-id="529d6-222">A file in hello share can be up too1 TB in size.</span></span>

### <a name="list-hello-files-in-a-share"></a><span data-ttu-id="529d6-223">Seznam hello soubory ve sdílené složce</span><span class="sxs-lookup"><span data-stu-id="529d6-223">List hello files in a share</span></span>
<span data-ttu-id="529d6-224">Můžete vytvořit seznam souborů a adresářů ve sdílené složce pomocí hello `az storage file list` příkaz:</span><span class="sxs-lookup"><span data-stu-id="529d6-224">You can list files and directories in a share by using hello `az storage file list` command:</span></span>

```azurecli
# List hello files in hello root of a share
az storage file list --share-name myshare --output table

# List hello files in a directory within a share
az storage file list --share-name myshare/myDir --output table

# List hello files in a path within a share
az storage file list --share-name myshare --path myDir/mySubDir/MySubDir2 --output table
```

### <a name="copy-files"></a><span data-ttu-id="529d6-225">Kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="529d6-225">Copy files</span></span>      
<span data-ttu-id="529d6-226">Můžete zkopírovat soubor tooanother souboru, soubor tooa objekt blob nebo soubor tooa objektů blob.</span><span class="sxs-lookup"><span data-stu-id="529d6-226">You can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="529d6-227">Například toocopy tooa adresář souboru v jiné sdílené složce:</span><span class="sxs-lookup"><span data-stu-id="529d6-227">For example, toocopy a file tooa directory in a different share:</span></span>        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="next-steps"></a><span data-ttu-id="529d6-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="529d6-228">Next steps</span></span>
<span data-ttu-id="529d6-229">Tady jsou některé další zdroje informací o další informace o práci s hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="529d6-229">Here are some additional resources for learning more about working with hello Azure CLI 2.0.</span></span>

* [<span data-ttu-id="529d6-230">Začínáme s Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="529d6-230">Get started with Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [<span data-ttu-id="529d6-231">Přehled příkazů Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="529d6-231">Azure CLI 2.0 command reference</span></span>](/cli/azure)
* [<span data-ttu-id="529d6-232">Azure CLI 2.0 na Githubu</span><span class="sxs-lookup"><span data-stu-id="529d6-232">Azure CLI 2.0 on GitHub</span></span>](https://github.com/Azure/azure-cli)
