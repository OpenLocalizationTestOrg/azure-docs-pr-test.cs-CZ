---
title: "Použití PowerShellu ke správě služby Azure File Storage | Dokumentace Microsoftu"
description: "Zjistěte, jak pomocí PowerShellu spravovat službu Azure File Storage."
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
ms.openlocfilehash: 148375b156c4ae1aa4bf203d215f7ed607a71b89
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="how-to-use-powershell-to-manage-azure-file-storage"></a><span data-ttu-id="16b11-103">Použití PowerShellu ke správě služby Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="16b11-103">How to use PowerShell to manage Azure File storage</span></span>
<span data-ttu-id="16b11-104">Azure PowerShell můžete použít k vytváření a správě sdílených složek.</span><span class="sxs-lookup"><span data-stu-id="16b11-104">You can use Azure PowerShell to create and manage file shares.</span></span>

## <a name="install-the-powershell-cmdlets-for-azure-storage"></a><span data-ttu-id="16b11-105">Instalace rutin PowerShellu pro Azure Storage</span><span class="sxs-lookup"><span data-stu-id="16b11-105">Install the PowerShell cmdlets for Azure Storage</span></span>
<span data-ttu-id="16b11-106">K používání PowerShellu budete potřebovat stáhnout a nainstalovat rutiny modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="16b11-106">To prepare to use PowerShell, download and install the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="16b11-107">Bod instalace a pokyny pro instalaci jsou popsané v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="16b11-107">See [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for the install point and installation instructions.</span></span>

> [!NOTE]
> <span data-ttu-id="16b11-108">Doporučuje se stáhnout a nainstalovat nebo upgradovat na nejnovější modul Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="16b11-108">It's recommended that you download and install or upgrade to the latest Azure PowerShell module.</span></span>
> 
> 

<span data-ttu-id="16b11-109">Klikněte na **Start** a zadejte **Windows PowerShell**, tím otevřete okno Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="16b11-109">Open an Azure PowerShell window by clicking **Start** and typing **Windows PowerShell**.</span></span> <span data-ttu-id="16b11-110">Okno PowerShell pro vás nahraje modul Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="16b11-110">The PowerShell window loads the Azure Powershell module for you.</span></span>

## <a name="create-a-context-for-your-storage-account-and-key"></a><span data-ttu-id="16b11-111">Vytvoření kontextu pro účet úložiště a klíč</span><span class="sxs-lookup"><span data-stu-id="16b11-111">Create a context for your storage account and key</span></span>
<span data-ttu-id="16b11-112">Vytvořte kontext účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="16b11-112">Create the storage account context.</span></span> <span data-ttu-id="16b11-113">Kontext obsahuje název účtu úložiště a klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="16b11-113">The context encapsulates the storage account name and account key.</span></span> <span data-ttu-id="16b11-114">Pokyny pro zkopírování klíče účtu z [webu Azure Portal](https://portal.azure.com) najdete v tématu [Zobrazení a zkopírování přístupových klíčů k úložišti](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="16b11-114">For instructions on copying your account key from the [Azure portal](https://portal.azure.com), see [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span></span>

<span data-ttu-id="16b11-115">Místo `storage-account-name` a `storage-account-key` zadejte název svého účtu úložiště a klíč v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="16b11-115">Replace `storage-account-name` and `storage-account-key` with your storage account name and key in the following example.</span></span>

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a><span data-ttu-id="16b11-116">Vytvoření nové sdílené složky</span><span class="sxs-lookup"><span data-stu-id="16b11-116">Create a new file share</span></span>
<span data-ttu-id="16b11-117">Vytvořte novou sdílenou složku s názvem `logs`.</span><span class="sxs-lookup"><span data-stu-id="16b11-117">Create the new share, named `logs`.</span></span>

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

<span data-ttu-id="16b11-118">Teď máte sdílenou složku v úložišti File.</span><span class="sxs-lookup"><span data-stu-id="16b11-118">You now have a file share in File storage.</span></span> <span data-ttu-id="16b11-119">Dál přidáme nový adresář a soubor.</span><span class="sxs-lookup"><span data-stu-id="16b11-119">Next we'll add a directory and a file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="16b11-120">Název vaší sdílené složky musí obsahovat jen malá písmena.</span><span class="sxs-lookup"><span data-stu-id="16b11-120">The name of your file share must be all lowercase.</span></span> <span data-ttu-id="16b11-121">Kompletní informace o zadávání názvů sdílených složek a souborů najdete v tématu [Pojmenování a odkazování na sdílené složky, soubory a metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span><span class="sxs-lookup"><span data-stu-id="16b11-121">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>
> 
> 

## <a name="create-a-directory-in-the-file-share"></a><span data-ttu-id="16b11-122">Vytvoření adresáře ve sdílené složce</span><span class="sxs-lookup"><span data-stu-id="16b11-122">Create a directory in the file share</span></span>
<span data-ttu-id="16b11-123">Ve sdílené složce vytvořte adresář.</span><span class="sxs-lookup"><span data-stu-id="16b11-123">Create a directory in the share.</span></span> <span data-ttu-id="16b11-124">V následujícím příkladu má adresář název `CustomLogs`.</span><span class="sxs-lookup"><span data-stu-id="16b11-124">In the following example, the directory is named `CustomLogs`.</span></span>

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-to-the-directory"></a><span data-ttu-id="16b11-125">Uložení místního souboru do adresáře</span><span class="sxs-lookup"><span data-stu-id="16b11-125">Upload a local file to the directory</span></span>
<span data-ttu-id="16b11-126">Teď do adresáře uložte místní soubor.</span><span class="sxs-lookup"><span data-stu-id="16b11-126">Now upload a local file to the directory.</span></span> <span data-ttu-id="16b11-127">V následujícím příkladu se uloží soubor z `C:\temp\Log1.txt`.</span><span class="sxs-lookup"><span data-stu-id="16b11-127">The following example uploads a file from `C:\temp\Log1.txt`.</span></span> <span data-ttu-id="16b11-128">Upravte cestu k souboru tak, aby odkazovala na platný soubor na vašem místním počítači.</span><span class="sxs-lookup"><span data-stu-id="16b11-128">Edit the file path so that it points to a valid file on your local machine.</span></span>

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-the-files-in-the-directory"></a><span data-ttu-id="16b11-129">Zobrazení seznamu souborů v adresáři</span><span class="sxs-lookup"><span data-stu-id="16b11-129">List the files in the directory</span></span>
<span data-ttu-id="16b11-130">Pokud chcete soubor zobrazit v adresáři, můžete vyvolat seznam všech souborů v adresáři.</span><span class="sxs-lookup"><span data-stu-id="16b11-130">To see the file in the directory, you can list all of the directory's files.</span></span> <span data-ttu-id="16b11-131">Tento příkaz vrátí soubory a podadresáře v adresáři CustomLogs (pokud tam nějaké jsou).</span><span class="sxs-lookup"><span data-stu-id="16b11-131">This command returns the files and subdirectories (if there are any) in the CustomLogs directory.</span></span>

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

<span data-ttu-id="16b11-132">Get-AzureStorageFile vrátí seznam souborů a adresářů pro jakýkoli adresář, ve kterém se objekt předá.</span><span class="sxs-lookup"><span data-stu-id="16b11-132">Get-AzureStorageFile returns a list of files and directories for whatever directory object is passed in.</span></span> <span data-ttu-id="16b11-133">„Get-AzureStorageFile -Share $s“ vrátí seznam souborů a adresářů v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="16b11-133">"Get-AzureStorageFile -Share $s" returns a list of files and directories in the root directory.</span></span> <span data-ttu-id="16b11-134">Pokud chcete získat seznam souborů v podadresáři, musíte předat podadresář pro  Get-AzureStorageFile.</span><span class="sxs-lookup"><span data-stu-id="16b11-134">To get a list of files in a subdirectory, you have to pass the subdirectory to Get-AzureStorageFile.</span></span> <span data-ttu-id="16b11-135">Tohle to udělá – první část příkazu vrátí instanci adresáře podadresáře CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="16b11-135">That's what this does -- the first part of the command up to the pipe returns a directory instance of the subdirectory CustomLogs.</span></span> <span data-ttu-id="16b11-136">To se potom předá do Get-AzureStorageFile, a to vrátí soubory a adresáře v CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="16b11-136">Then that is passed into Get-AzureStorageFile, which returns the files and directories in CustomLogs.</span></span>

## <a name="copy-files"></a><span data-ttu-id="16b11-137">Kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="16b11-137">Copy files</span></span>
<span data-ttu-id="16b11-138">Od Azure PowerShell verze 0.9.7 můžete kopírovat soubor do jiného souboru, soubor do objektu nebo objekt blob do souboru.</span><span class="sxs-lookup"><span data-stu-id="16b11-138">Beginning with version 0.9.7 of Azure PowerShell, you can copy a file to another file, a file to a blob, or a blob to a file.</span></span> <span data-ttu-id="16b11-139">Dole ukážeme, jak se operace kopírovaní dají provést pomocí rutin PowerShell.</span><span class="sxs-lookup"><span data-stu-id="16b11-139">Below we demonstrate how to perform these copy operations using PowerShell cmdlets.</span></span>

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a><span data-ttu-id="16b11-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16b11-140">Next steps</span></span>
<span data-ttu-id="16b11-141">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="16b11-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="16b11-142">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="16b11-142">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="16b11-143">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="16b11-143">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)