---
title: "Vytvoření sdílené složky Azure | Dokumentace Microsoftu"
description: "Postup vytvoření sdílené složky Azure ve službě Azure File Storage pomocí webu Azure Portal, PowerShellu a Azure CLI."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: b81701e2544ace092f007e5d98b3141e1f7da724
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-file-share-in-azure-file-storage"></a><span data-ttu-id="41119-103">Vytvoření sdílené složky ve službě Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="41119-103">Create a File Share in Azure File storage</span></span>
<span data-ttu-id="41119-104">Sdílené složky Azure můžete vytvářet pomocí webu [Azure Portal](https://portal.azure.com/), rutin PowerShellu pro službu Azure Storage, klientských knihoven pro službu Azure Storage nebo rozhraní REST API pro službu Azure Storage. V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="41119-104">You can create Azure File shares using [Azure portal](https://portal.azure.com/), the Azure Storage PowerShell cmdlets, the Azure Storage client libraries, or the Azure Storage REST API.In this tutorial, you will learn:</span></span>
* [<span data-ttu-id="41119-105">Jak vytvořit sdílenou složku Azure pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="41119-105">How to create an Azure File share using the Azure portal</span></span>](#Create file share through the Portal)
* [<span data-ttu-id="41119-106">Jak vytvořit sdílenou složku Azure pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="41119-106">How to create an Azure File share using Powershell</span></span>](#Create file share using PowerShell)
* [<span data-ttu-id="41119-107">Jak vytvořit sdílenou složku Azure pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="41119-107">How to create an Azure File share using CLI</span></span>](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a><span data-ttu-id="41119-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="41119-108">Prerequisites</span></span>
<span data-ttu-id="41119-109">Pokud chcete vytvořit sdílenou složku Azure, můžete použít už existující účet úložiště nebo [vytvořit nový účet úložiště Azure](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="41119-109">To create an Azure File share, you can use a storage account that already exists, or [create a new Azure storage account](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span> <span data-ttu-id="41119-110">Pokud chcete vytvořit sdílenou složku Azure pomocí PowerShellu, budete potřebovat klíč účtu a název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="41119-110">To create an Azure File share with PowerShell, you will need the account key and name of your storage account..</span></span> <span data-ttu-id="41119-111">Pokud se chystáte použít PowerShell nebo rozhraní příkazového řádku, budete potřebovat klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="41119-111">You will need Storage account key if you plan to use Powershell or CLI.</span></span>

## <a name="create-file-share-through-the-portal"></a><span data-ttu-id="41119-112">Vytvoření sdílené složky prostřednictvím portálu</span><span class="sxs-lookup"><span data-stu-id="41119-112">Create file share through the Portal</span></span>
1. <span data-ttu-id="41119-113">**Přejděte do okna Účet úložiště na webu Azure Portal:**</span><span class="sxs-lookup"><span data-stu-id="41119-113">**Go to Storage Account blade on Azure Portal**:</span></span>    
    <span data-ttu-id="41119-114">![Okno Účet úložiště](./media/storage-how-to-create-file-share/create-file-share-portal1.png)</span><span class="sxs-lookup"><span data-stu-id="41119-114">![Storage Account Blade](./media/storage-how-to-create-file-share/create-file-share-portal1.png)</span></span>

2. <span data-ttu-id="41119-115">**Klikněte na tlačítko Přidat sdílenou složku:**</span><span class="sxs-lookup"><span data-stu-id="41119-115">**Click on add File Share button**:</span></span>    
    <span data-ttu-id="41119-116">![Kliknutí na tlačítko Přidat sdílenou složku](./media/storage-how-to-create-file-share/create-file-share-portal2.png)</span><span class="sxs-lookup"><span data-stu-id="41119-116">![Click the add file share button](./media/storage-how-to-create-file-share/create-file-share-portal2.png)</span></span>

3. <span data-ttu-id="41119-117">**Zadejte název a kvótu. Aktuálně může být kvóta maximálně 5 TB:**</span><span class="sxs-lookup"><span data-stu-id="41119-117">**Provide Name and Quota. Quota currently can be maximum 5TB**:</span></span>    
    <span data-ttu-id="41119-118">![Zadání názvu a požadované kvóty pro novou sdílenou složku](./media/storage-how-to-create-file-share/create-file-share-portal3.png)</span><span class="sxs-lookup"><span data-stu-id="41119-118">![Provide a name and a desired quota for the new file share](./media/storage-how-to-create-file-share/create-file-share-portal3.png)</span></span>

4. <span data-ttu-id="41119-119">**Zobrazte novou sdílenou složku:** ![Zobrazení nové sdílené složky](./media/storage-how-to-create-file-share/create-file-share-portal4.png)</span><span class="sxs-lookup"><span data-stu-id="41119-119">**View your new file share**:  ![View your new file share](./media/storage-how-to-create-file-share/create-file-share-portal4.png)</span></span>

5. <span data-ttu-id="41119-120">**Nahrajte soubor:** ![Nahrání souboru](./media/storage-how-to-create-file-share/create-file-share-portal5.png)</span><span class="sxs-lookup"><span data-stu-id="41119-120">**Upload a file**:  ![Upload a file](./media/storage-how-to-create-file-share/create-file-share-portal5.png)</span></span>

6. <span data-ttu-id="41119-121">**Přejděte do sdílené složky a spravujte adresáře a soubory:** ![Procházení sdílené složky](./media/storage-how-to-create-file-share/create-file-share-portal6.png)</span><span class="sxs-lookup"><span data-stu-id="41119-121">**Browse into your file share and manage your directories and files**:  ![Browse file share](./media/storage-how-to-create-file-share/create-file-share-portal6.png)</span></span>


## <a name="create-file-share-through-powershell"></a><span data-ttu-id="41119-122">Vytvoření sdílené složky prostřednictvím PowerShellu</span><span class="sxs-lookup"><span data-stu-id="41119-122">Create file share through PowerShell</span></span>
<span data-ttu-id="41119-123">K používání PowerShellu budete potřebovat stáhnout a nainstalovat rutiny modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="41119-123">To prepare to use PowerShell, download and install the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="41119-124">Bod instalace a pokyny pro instalaci jsou popsané v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="41119-124">See [How to install and configure Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) for the install point and installation instructions.</span></span>

> [!Note]  
> <span data-ttu-id="41119-125">Doporučuje se stáhnout a nainstalovat nebo upgradovat na nejnovější modul Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="41119-125">It's recommended that you download and install or upgrade to the latest Azure PowerShell module.</span></span>

1. <span data-ttu-id="41119-126">**Vytvořte kontext pro účet úložiště a klíč.** Kontext obsahuje název účtu úložiště a klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="41119-126">**Create a context for your storage account and key** The context encapsulates the storage account name and account key.</span></span> <span data-ttu-id="41119-127">Pokyny pro zkopírování klíče účtu z [webu Azure Portal](https://portal.azure.com/) najdete v tématu [Zobrazení a zkopírování přístupových klíčů k úložišti](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="41119-127">For instructions on copying your account key from the [Azure portal](https://portal.azure.com/), see [View and copy storage access keys](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).</span></span>

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. <span data-ttu-id="41119-128">**Vytvořte novou sdílenou složku:**</span><span class="sxs-lookup"><span data-stu-id="41119-128">**Create a new file share**:</span></span>    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> <span data-ttu-id="41119-129">Název vaší sdílené složky musí obsahovat jen malá písmena.</span><span class="sxs-lookup"><span data-stu-id="41119-129">The name of your file share must be all lowercase.</span></span> <span data-ttu-id="41119-130">Kompletní informace o zadávání názvů sdílených složek a souborů najdete v tématu [Pojmenování a odkazování na sdílené složky, soubory a metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span><span class="sxs-lookup"><span data-stu-id="41119-130">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>

## <a name="create-file-share-through-command-line-interface-cli"></a><span data-ttu-id="41119-131">Vytvoření sdílené složky prostřednictvím rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="41119-131">Create file share through Command Line Interface (CLI)</span></span>
1. <span data-ttu-id="41119-132">**V rámci přípravy na použití rozhraní příkazového řádku (CLI) stáhněte a nainstalujte Azure CLI.**</span><span class="sxs-lookup"><span data-stu-id="41119-132">**To prepare to use a Command Line Interface (CLI), download and install the Azure CLI.**</span></span>  
    <span data-ttu-id="41119-133">Viz témata [Instalace Azure CLI 2.0](/cli/azure/install-az-cli2.md) a [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="41119-133">See [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) and [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span>

2. <span data-ttu-id="41119-134">**Vytvořte připojovací řetězec k účtu úložiště, kde chcete sdílenou složku vytvořit.**</span><span class="sxs-lookup"><span data-stu-id="41119-134">**Create a connection string to the storage account where you want to create the share.**</span></span>  
    <span data-ttu-id="41119-135">V následujícím příkladu nahraďte ```<storage-account>``` a ```<resource_group>``` názvem svého účtu úložiště a skupinou prostředků.</span><span class="sxs-lookup"><span data-stu-id="41119-135">Replace ```<storage-account>``` and ```<resource_group>``` with your storage account name and resource group in the following example.</span></span>

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
    fi
    ```

3. <span data-ttu-id="41119-136">**Vytvoření sdílené složky**</span><span class="sxs-lookup"><span data-stu-id="41119-136">**Create file share**</span></span>
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a><span data-ttu-id="41119-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="41119-137">Next steps</span></span>
* [<span data-ttu-id="41119-138">Připojení ke sdílené složce a její připojení – Windows</span><span class="sxs-lookup"><span data-stu-id="41119-138">Connect and Mount File Share - Windows</span></span>](storage-how-to-use-files-windows.md)
* [<span data-ttu-id="41119-139">Připojení ke sdílené složce a její připojení – Linux</span><span class="sxs-lookup"><span data-stu-id="41119-139">Connect and Mount File Share - Linux</span></span>](../storage-how-to-use-files-linux.md)
* [<span data-ttu-id="41119-140">Připojení ke sdílené složce a její připojení – macOS</span><span class="sxs-lookup"><span data-stu-id="41119-140">Connect and Mount File Share - macOS</span></span>](storage-how-to-use-files-mac.md)

<span data-ttu-id="41119-141">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="41119-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="41119-142">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="41119-142">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="41119-143">Řešení potíží ve Windows</span><span class="sxs-lookup"><span data-stu-id="41119-143">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="41119-144">Řešení potíží v Linuxu</span><span class="sxs-lookup"><span data-stu-id="41119-144">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)   