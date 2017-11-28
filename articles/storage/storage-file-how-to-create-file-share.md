---
title: "aaaHow toocreate sdílenou složku Azure | Microsoft Docs"
description: "Jak toocreate Azure soubor sdílet v Azure File storage pomocí hello portálu Azure, PowerShell a hello rozhraní příkazového řádku Azure."
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
ms.openlocfilehash: c4c59966b82d743fb4ecf79f48c0c8e61295d03d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-file-share-in-azure-file-storage"></a><span data-ttu-id="1f980-103">Vytvoření sdílené složky ve službě Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="1f980-103">Create a File Share in Azure File storage</span></span>
<span data-ttu-id="1f980-104">Můžete vytvořit sdílené složky Azure File pomocí [portál Azure](https://portal.azure.com/)hello rutin Powershellu pro úložiště Azure, hello knihovny klienta Azure Storage nebo REST API pro Azure Storage hello. V tomto kurzu se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="1f980-104">You can create Azure File shares using [Azure portal](https://portal.azure.com/), hello Azure Storage PowerShell cmdlets, hello Azure Storage client libraries, or hello Azure Storage REST API.In this tutorial, you will learn:</span></span>
* [<span data-ttu-id="1f980-105">Jak toocreate Azure File sdílet pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1f980-105">How toocreate an Azure File share using hello Azure portal</span></span>](#Create file share through hello Portal)
* [<span data-ttu-id="1f980-106">Jak toocreate Azure File sdílet pomocí prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="1f980-106">How toocreate an Azure File share using Powershell</span></span>](#Create file share using PowerShell)
* [<span data-ttu-id="1f980-107">Jak toocreate Azure File sdílet pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="1f980-107">How toocreate an Azure File share using CLI</span></span>](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a><span data-ttu-id="1f980-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1f980-108">Prerequisites</span></span>
<span data-ttu-id="1f980-109">toocreate sdílenou složku Azure, můžete použít účet úložiště, který již existuje, nebo [vytvořit nový účet úložiště Azure](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="1f980-109">toocreate an Azure File share, you can use a storage account that already exists, or [create a new Azure storage account](storage-create-storage-account.md).</span></span> <span data-ttu-id="1f980-110">toocreate sdílenou Azure pomocí prostředí PowerShell, budete potřebovat název svého účtu úložiště a klíč účtu hello...</span><span class="sxs-lookup"><span data-stu-id="1f980-110">toocreate an Azure File share with PowerShell, you will need hello account key and name of your storage account..</span></span> <span data-ttu-id="1f980-111">Pokud máte v plánu toouse prostředí Powershell nebo rozhraní příkazového řádku, budete potřebovat klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1f980-111">You will need Storage account key if you plan toouse Powershell or CLI.</span></span>

## <a name="create-file-share-through-hello-portal"></a><span data-ttu-id="1f980-112">Vytvoření sdílené složky prostřednictvím hello portálu</span><span class="sxs-lookup"><span data-stu-id="1f980-112">Create file share through hello Portal</span></span>
1. <span data-ttu-id="1f980-113">**Okno účtu tooStorage přejděte na portálu Azure**:</span><span class="sxs-lookup"><span data-stu-id="1f980-113">**Go tooStorage Account blade on Azure Portal**:</span></span>    
    <span data-ttu-id="1f980-114">![Okno Účet úložiště](media/storage-file-how-to-create-file-share/create-file-share-portal1.png)</span><span class="sxs-lookup"><span data-stu-id="1f980-114">![Storage Account Blade](media/storage-file-how-to-create-file-share/create-file-share-portal1.png)</span></span>

2. <span data-ttu-id="1f980-115">**Klikněte na tlačítko Přidat sdílenou složku:**</span><span class="sxs-lookup"><span data-stu-id="1f980-115">**Click on add File Share button**:</span></span>    
    <span data-ttu-id="1f980-116">![Klikněte na tlačítko hello přidat sdílené složky souboru – tlačítko](media/storage-file-how-to-create-file-share/create-file-share-portal2.png)</span><span class="sxs-lookup"><span data-stu-id="1f980-116">![Click hello add file share button](media/storage-file-how-to-create-file-share/create-file-share-portal2.png)</span></span>

3. <span data-ttu-id="1f980-117">**Zadejte název a kvótu. Aktuálně může být kvóta maximálně 5 TB:**</span><span class="sxs-lookup"><span data-stu-id="1f980-117">**Provide Name and Quota. Quota currently can be maximum 5TB**:</span></span>    
    <span data-ttu-id="1f980-118">![Zadejte název a požadované kvótu pro hello nové sdílené složky](media/storage-file-how-to-create-file-share/create-file-share-portal3.png)</span><span class="sxs-lookup"><span data-stu-id="1f980-118">![Provide a name and a desired quota for hello new file share](media/storage-file-how-to-create-file-share/create-file-share-portal3.png)</span></span>

4. <span data-ttu-id="1f980-119">**Zobrazte novou sdílenou složku:**![Zobrazení nové sdílené složky](media/storage-file-how-to-create-file-share/create-file-share-portal4.png)</span><span class="sxs-lookup"><span data-stu-id="1f980-119">**View your new file share**:  ![View your new file share](media/storage-file-how-to-create-file-share/create-file-share-portal4.png)</span></span>

5. <span data-ttu-id="1f980-120">**Nahrajte soubor:**![Nahrání souboru](media/storage-file-how-to-create-file-share/create-file-share-portal5.png)</span><span class="sxs-lookup"><span data-stu-id="1f980-120">**Upload a file**:  ![Upload a file](media/storage-file-how-to-create-file-share/create-file-share-portal5.png)</span></span>

6. <span data-ttu-id="1f980-121">**Přejděte do sdílené složky a spravujte adresáře a soubory:**![Procházení sdílené složky](media/storage-file-how-to-create-file-share/create-file-share-portal6.png)</span><span class="sxs-lookup"><span data-stu-id="1f980-121">**Browse into your file share and manage your directories and files**:  ![Browse file share](media/storage-file-how-to-create-file-share/create-file-share-portal6.png)</span></span>


## <a name="create-file-share-through-powershell"></a><span data-ttu-id="1f980-122">Vytvoření sdílené složky prostřednictvím PowerShellu</span><span class="sxs-lookup"><span data-stu-id="1f980-122">Create file share through PowerShell</span></span>
<span data-ttu-id="1f980-123">tooprepare toouse prostředí PowerShell, stáhněte a nainstalujte hello rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1f980-123">tooprepare toouse PowerShell, download and install hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="1f980-124">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) hello nainstalovat bod a pokyny k instalaci.</span><span class="sxs-lookup"><span data-stu-id="1f980-124">See [How tooinstall and configure Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) for hello install point and installation instructions.</span></span>

> [!Note]  
> <span data-ttu-id="1f980-125">Doporučujeme stáhnout a nainstalovat nebo upgradovat toohello nejnovější modul Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1f980-125">It's recommended that you download and install or upgrade toohello latest Azure PowerShell module.</span></span>

1. <span data-ttu-id="1f980-126">**Vytvoření kontextu pro účet úložiště a klíč** hello kontext obsahuje hello klíč účtu úložiště účet a název.</span><span class="sxs-lookup"><span data-stu-id="1f980-126">**Create a context for your storage account and key** hello context encapsulates hello storage account name and account key.</span></span> <span data-ttu-id="1f980-127">Pokyny pro zkopírování klíče účtu z [webu Azure Portal](https://portal.azure.com/) najdete v tématu [Zobrazení a zkopírování přístupových klíčů k úložišti](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="1f980-127">For instructions on copying your account key from the [Azure portal](https://portal.azure.com/), see [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span></span>

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. <span data-ttu-id="1f980-128">**Vytvořte novou sdílenou složku:**</span><span class="sxs-lookup"><span data-stu-id="1f980-128">**Create a new file share**:</span></span>    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> <span data-ttu-id="1f980-129">Hello název sdílené složky musí být psaný malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="1f980-129">hello name of your file share must be all lowercase.</span></span> <span data-ttu-id="1f980-130">Kompletní informace o zadávání názvů sdílených složek a souborů najdete v tématu [Pojmenování a odkazování na sdílené složky, soubory a metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span><span class="sxs-lookup"><span data-stu-id="1f980-130">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>

## <a name="create-file-share-through-command-line-interface-cli"></a><span data-ttu-id="1f980-131">Vytvoření sdílené složky prostřednictvím rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="1f980-131">Create file share through Command Line Interface (CLI)</span></span>
1. <span data-ttu-id="1f980-132">**tooprepare toouse rozhraní příkazového řádku (CLI), stáhněte a nainstalujte hello rozhraní příkazového řádku Azure.**</span><span class="sxs-lookup"><span data-stu-id="1f980-132">**tooprepare toouse a Command Line Interface (CLI), download and install hello Azure CLI.**</span></span>  
    <span data-ttu-id="1f980-133">Viz témata [Instalace Azure CLI 2.0](/cli/azure/install-az-cli2.md) a [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1f980-133">See [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) and [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span>

2. <span data-ttu-id="1f980-134">**Vytvoření účtu úložiště toohello řetězec připojení místo toocreate hello sdílené složky.**</span><span class="sxs-lookup"><span data-stu-id="1f980-134">**Create a connection string toohello storage account where you want toocreate hello share.**</span></span>  
    <span data-ttu-id="1f980-135">Nahraďte ```<storage-account>``` a ```<resource_group>``` s váš účet název a prostředků skupiny úložišť v hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="1f980-135">Replace ```<storage-account>``` and ```<resource_group>``` with your storage account name and resource group in hello following example.</span></span>

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve hello connection string."
    fi
    ```

3. <span data-ttu-id="1f980-136">**Vytvoření sdílené složky**</span><span class="sxs-lookup"><span data-stu-id="1f980-136">**Create file share**</span></span>
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a><span data-ttu-id="1f980-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f980-137">Next steps</span></span>
* [<span data-ttu-id="1f980-138">Připojení ke sdílené složce a její připojení – Windows</span><span class="sxs-lookup"><span data-stu-id="1f980-138">Connect and Mount File Share - Windows</span></span>](storage-file-how-to-use-files-windows.md)
* [<span data-ttu-id="1f980-139">Připojení ke sdílené složce a její připojení – Linux</span><span class="sxs-lookup"><span data-stu-id="1f980-139">Connect and Mount File Share - Linux</span></span>](storage-how-to-use-files-linux.md)
* [<span data-ttu-id="1f980-140">Připojení ke sdílené složce a její připojení – macOS</span><span class="sxs-lookup"><span data-stu-id="1f980-140">Connect and Mount File Share - macOS</span></span>](storage-file-how-to-use-files-mac.md)

<span data-ttu-id="1f980-141">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="1f980-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="1f980-142">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="1f980-142">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="1f980-143">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="1f980-143">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)