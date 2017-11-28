---
title: "aaaHow toouse prostředí PowerShell toomanage Azure File storage | Microsoft Docs"
description: "Přečtěte si toouse prostředí PowerShell toomanage Azure File storage."
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
ms.openlocfilehash: 0e30e8796cf8bbf5f9249b26179d5e0f9077c8fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-powershell-toomanage-azure-file-storage"></a><span data-ttu-id="f7a1b-103">Jak toouse prostředí PowerShell toomanage Azure File storage</span><span class="sxs-lookup"><span data-stu-id="f7a1b-103">How toouse PowerShell toomanage Azure File storage</span></span>
<span data-ttu-id="f7a1b-104">Můžete použít Azure PowerShell toocreate a spravovat sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-104">You can use Azure PowerShell toocreate and manage file shares.</span></span>

## <a name="install-hello-powershell-cmdlets-for-azure-storage"></a><span data-ttu-id="f7a1b-105">Instalace hello rutin prostředí PowerShell pro Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f7a1b-105">Install hello PowerShell cmdlets for Azure Storage</span></span>
<span data-ttu-id="f7a1b-106">tooprepare toouse prostředí PowerShell, stáhněte a nainstalujte hello rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-106">tooprepare toouse PowerShell, download and install hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="f7a1b-107">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) hello nainstalovat bod a pokyny k instalaci.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-107">See [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for hello install point and installation instructions.</span></span>

> [!NOTE]
> <span data-ttu-id="f7a1b-108">Doporučujeme stáhnout a nainstalovat nebo upgradovat toohello nejnovější modul Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-108">It's recommended that you download and install or upgrade toohello latest Azure PowerShell module.</span></span>
> 
> 

<span data-ttu-id="f7a1b-109">Klikněte na **Start** a zadejte **Windows PowerShell**, tím otevřete okno Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-109">Open an Azure PowerShell window by clicking **Start** and typing **Windows PowerShell**.</span></span> <span data-ttu-id="f7a1b-110">okno PowerShell Hello načte modul Azure Powershell hello za vás.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-110">hello PowerShell window loads hello Azure Powershell module for you.</span></span>

## <a name="create-a-context-for-your-storage-account-and-key"></a><span data-ttu-id="f7a1b-111">Vytvoření kontextu pro účet úložiště a klíč</span><span class="sxs-lookup"><span data-stu-id="f7a1b-111">Create a context for your storage account and key</span></span>
<span data-ttu-id="f7a1b-112">Vytvořte kontext účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-112">Create hello storage account context.</span></span> <span data-ttu-id="f7a1b-113">kontext Hello obsahuje hello klíč účtu úložiště účet a název.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-113">hello context encapsulates hello storage account name and account key.</span></span> <span data-ttu-id="f7a1b-114">Pokyny pro zkopírování klíče účtu z hello [portál Azure](https://portal.azure.com), najdete v části [zobrazení a zkopírování přístupových klíčů úložiště](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="f7a1b-114">For instructions on copying your account key from hello [Azure portal](https://portal.azure.com), see [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span></span>

<span data-ttu-id="f7a1b-115">Nahraďte `storage-account-name` a `storage-account-key` s název účtu úložiště a klíč v hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-115">Replace `storage-account-name` and `storage-account-key` with your storage account name and key in hello following example.</span></span>

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a><span data-ttu-id="f7a1b-116">Vytvoření nové sdílené složky</span><span class="sxs-lookup"><span data-stu-id="f7a1b-116">Create a new file share</span></span>
<span data-ttu-id="f7a1b-117">Vytvoření hello novou sdílenou složku s názvem `logs`.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-117">Create hello new share, named `logs`.</span></span>

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

<span data-ttu-id="f7a1b-118">Teď máte sdílenou složku v úložišti File.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-118">You now have a file share in File storage.</span></span> <span data-ttu-id="f7a1b-119">Dál přidáme nový adresář a soubor.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-119">Next we'll add a directory and a file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f7a1b-120">Hello název sdílené složky musí být psaný malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-120">hello name of your file share must be all lowercase.</span></span> <span data-ttu-id="f7a1b-121">Kompletní informace o zadávání názvů sdílených složek a souborů najdete v tématu [Pojmenování a odkazování na sdílené složky, soubory a metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7a1b-121">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>
> 
> 

## <a name="create-a-directory-in-hello-file-share"></a><span data-ttu-id="f7a1b-122">Vytvoření adresáře ve sdílené složce hello</span><span class="sxs-lookup"><span data-stu-id="f7a1b-122">Create a directory in hello file share</span></span>
<span data-ttu-id="f7a1b-123">Vytvořte adresář ve sdílené složce hello.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-123">Create a directory in hello share.</span></span> <span data-ttu-id="f7a1b-124">V následujícím příkladu hello, adresář hello název `CustomLogs`.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-124">In hello following example, hello directory is named `CustomLogs`.</span></span>

```powershell
# create a directory in hello share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-toohello-directory"></a><span data-ttu-id="f7a1b-125">Nahrát adresář toohello místního souboru</span><span class="sxs-lookup"><span data-stu-id="f7a1b-125">Upload a local file toohello directory</span></span>
<span data-ttu-id="f7a1b-126">Teď nahrajte adresář toohello místního souboru.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-126">Now upload a local file toohello directory.</span></span> <span data-ttu-id="f7a1b-127">Hello následujícím příkladu se uloží soubor z `C:\temp\Log1.txt`.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-127">hello following example uploads a file from `C:\temp\Log1.txt`.</span></span> <span data-ttu-id="f7a1b-128">Cesta k souboru hello upravte tak, aby ukazoval tooa platný soubor na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-128">Edit hello file path so that it points tooa valid file on your local machine.</span></span>

```powershell
# upload a local file toohello new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-hello-files-in-hello-directory"></a><span data-ttu-id="f7a1b-129">Seznam hello soubory v adresáři hello</span><span class="sxs-lookup"><span data-stu-id="f7a1b-129">List hello files in hello directory</span></span>
<span data-ttu-id="f7a1b-130">toosee hello soubor v adresáři hello, můžete seznam všech souborů hello adresáře.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-130">toosee hello file in hello directory, you can list all of hello directory's files.</span></span> <span data-ttu-id="f7a1b-131">Tento příkaz vrátí hello soubory a podadresáře (pokud existují) v adresáři CustomLogs hello.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-131">This command returns hello files and subdirectories (if there are any) in hello CustomLogs directory.</span></span>

```powershell
# list files in hello new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

<span data-ttu-id="f7a1b-132">Get-AzureStorageFile vrátí seznam souborů a adresářů pro jakýkoli adresář, ve kterém se objekt předá.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-132">Get-AzureStorageFile returns a list of files and directories for whatever directory object is passed in.</span></span> <span data-ttu-id="f7a1b-133">"Get-AzureStorageFile-Share $s" vrátí seznam souborů a adresářů v kořenovém adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-133">"Get-AzureStorageFile -Share $s" returns a list of files and directories in hello root directory.</span></span> <span data-ttu-id="f7a1b-134">seznam souborů v podadresáři tooget, máte toopass hello podadresáři tooGet-AzureStorageFile.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-134">tooget a list of files in a subdirectory, you have toopass hello subdirectory tooGet-AzureStorageFile.</span></span> <span data-ttu-id="f7a1b-135">Se to udělá – první část příkazu hello až toohello kanálu hello vrátí instanci adresáře podadresáře hello CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-135">That's what this does -- hello first part of hello command up toohello pipe returns a directory instance of hello subdirectory CustomLogs.</span></span> <span data-ttu-id="f7a1b-136">To se potom předá do Get-AzureStorageFile, která vrátí hodnotu hello soubory a adresáře v CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-136">Then that is passed into Get-AzureStorageFile, which returns hello files and directories in CustomLogs.</span></span>

## <a name="copy-files"></a><span data-ttu-id="f7a1b-137">Kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="f7a1b-137">Copy files</span></span>
<span data-ttu-id="f7a1b-138">Od verze 0.9.7 prostředí Azure PowerShell, můžete zkopírovat soubor tooanother souboru, soubor tooa objekt blob nebo soubor tooa objektů blob.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-138">Beginning with version 0.9.7 of Azure PowerShell, you can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="f7a1b-139">Dole ukážeme, jak se tooperform tyto zkopírovat operace pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-139">Below we demonstrate how tooperform these copy operations using PowerShell cmdlets.</span></span>

```powershell
# copy a file toohello new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob tooa file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a><span data-ttu-id="f7a1b-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7a1b-140">Next steps</span></span>
<span data-ttu-id="f7a1b-141">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="f7a1b-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="f7a1b-142">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="f7a1b-142">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="f7a1b-143">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="f7a1b-143">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)