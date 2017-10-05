---
title: "Zkopírovat nebo přesunout data do služby Azure Storage s AzCopy v systému Windows | Microsoft Docs"
description: "Přesunutí nebo zkopírování dat z objektu blob, table a obsah souboru nebo pomocí AzCopy na nástroj systému Windows. Kopírování dat do úložiště Azure z místních souborů, nebo zkopírujte data v rámci nebo mezi účty úložiště. Snadno migrujte data do úložiště Azure."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: aa155738-7c69-4a83-94f8-b97af4461274
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2017
ms.author: seguler
ms.openlocfilehash: 9806697747b2409d4bd0ae19dc0b9fe01f500dc0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="transfer-data-with-the-azcopy-on-windows"></a><span data-ttu-id="ba399-105">Přenos dat pomocí AzCopy v systému Windows</span><span class="sxs-lookup"><span data-stu-id="ba399-105">Transfer data with the AzCopy on Windows</span></span>
<span data-ttu-id="ba399-106">AzCopy je nástroj příkazového řádku pro kopírování dat do a z úložiště objektů Blob v Microsoft Azure, souboru a tabulky pomocí jednoduchých příkazů optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="ba399-106">AzCopy is a command-line utility designed for copying data to and from Microsoft Azure Blob, File, and Table storage using simple commands with optimal performance.</span></span> <span data-ttu-id="ba399-107">Data můžete zkopírovat z jednoho objektu do druhého v rámci účtu úložiště nebo mezi účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="ba399-107">You can copy data from one object to another within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="ba399-108">Existují dvě verze nástroje AzCopy, které si můžete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="ba399-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="ba399-109">AzCopy v systému Windows je obsažena v rozhraní .NET Framework a nabízí možnosti příkazového řádku Windows styl.</span><span class="sxs-lookup"><span data-stu-id="ba399-109">AzCopy on Windows is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="ba399-110">[AzCopy v systému Linux](storage-use-azcopy-linux.md) sestavena pomocí rozhraní .NET Framework Core, které cílí platformy Linux nabídky stylu POSIX možnosti příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ba399-110">[AzCopy on Linux](storage-use-azcopy-linux.md) is built with .NET Core Framework which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="ba399-111">Tento článek se zabývá AzCopy v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="ba399-111">This article covers AzCopy on Windows.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="ba399-112">Stáhněte a nainstalujte AzCopy</span><span class="sxs-lookup"><span data-stu-id="ba399-112">Download and install AzCopy</span></span>
### <a name="azcopy-on-windows"></a><span data-ttu-id="ba399-113">AzCopy ve Windows</span><span class="sxs-lookup"><span data-stu-id="ba399-113">AzCopy on Windows</span></span>
<span data-ttu-id="ba399-114">Stažení [nejnovější verzi AzCopy v systému Windows](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="ba399-114">Download the [latest version of AzCopy on Windows](http://aka.ms/downloadazcopy).</span></span>

#### <a name="installation-on-windows"></a><span data-ttu-id="ba399-115">Instalace v systému Windows</span><span class="sxs-lookup"><span data-stu-id="ba399-115">Installation on Windows</span></span>
<span data-ttu-id="ba399-116">Po instalaci nástroje AzCopy pomocí Instalační služby systému Windows, otevřete okno příkazového řádku a přejděte do instalačního adresáře nástroje AzCopy ve vašem počítači - kde `AzCopy.exe` spustitelný soubor se nachází.</span><span class="sxs-lookup"><span data-stu-id="ba399-116">After installing AzCopy on Windows using the installer, open a command window and navigate to the AzCopy installation directory on your computer - where the `AzCopy.exe` executable is located.</span></span> <span data-ttu-id="ba399-117">V případě potřeby můžete přidat umístění instalace AzCopy cestu v systému.</span><span class="sxs-lookup"><span data-stu-id="ba399-117">If desired, you can add the AzCopy installation location to your system path.</span></span> <span data-ttu-id="ba399-118">Ve výchozím nastavení, je nainstalován nástroj AzCopy k `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` nebo `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="ba399-118">By default, AzCopy is installed to `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` or `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span></span>

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="ba399-119">Zápis vaše první příkaz AzCopy</span><span class="sxs-lookup"><span data-stu-id="ba399-119">Writing your first AzCopy command</span></span>
<span data-ttu-id="ba399-120">Základní syntaxe pro příkazy AzCopy je:</span><span class="sxs-lookup"><span data-stu-id="ba399-120">The basic syntax for AzCopy commands is:</span></span>

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

<span data-ttu-id="ba399-121">Následující příklady ukazují celou řadu scénářů pro kopírování dat do a z tabulek, souborů a objektů BLOB služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ba399-121">The following examples demonstrate a variety of scenarios for copying data to and from Microsoft Azure Blobs, Files, and Tables.</span></span> <span data-ttu-id="ba399-122">Odkazovat [AzCopy parametry](#azcopy-parameters) části Podrobné vysvětlení parametrů použitých v každém vzorku.</span><span class="sxs-lookup"><span data-stu-id="ba399-122">Refer to the [AzCopy Parameters](#azcopy-parameters) section for a detailed explanation of the parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="ba399-123">Objekt BLOB: stažení</span><span class="sxs-lookup"><span data-stu-id="ba399-123">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="ba399-124">Stáhnout jediného objektu blob</span><span class="sxs-lookup"><span data-stu-id="ba399-124">Download single blob</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="ba399-125">Všimněte si, že pokud složce `C:\myfolder` neexistuje, vytvoří AzCopy a stažení `abc.txt ` do nové složky.</span><span class="sxs-lookup"><span data-stu-id="ba399-125">Note that if the folder `C:\myfolder` does not exist, AzCopy creates it and download `abc.txt ` into the new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="ba399-126">Stáhnout jediného objektu blob ze sekundární oblasti</span><span class="sxs-lookup"><span data-stu-id="ba399-126">Download single blob from secondary region</span></span>

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="ba399-127">Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.</span><span class="sxs-lookup"><span data-stu-id="ba399-127">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="ba399-128">Stažení všech objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="ba399-128">Download all blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="ba399-129">Předpokládejme, že následující objekty BLOB se nacházejí v zadaném kontejneru:</span><span class="sxs-lookup"><span data-stu-id="ba399-129">Assume the following blobs reside in the specified container:</span></span>  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="ba399-130">Po stažení operaci adresáři `C:\myfolder` zahrnuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="ba399-130">After the download operation, the directory `C:\myfolder` includes the following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

<span data-ttu-id="ba399-131">Pokud nezadáte možnost `/S`, se stáhnou žádné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="ba399-131">If you do not specify option `/S`, no blobs are downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="ba399-132">Stáhnout objekty BLOB se zadanou předponou.</span><span class="sxs-lookup"><span data-stu-id="ba399-132">Download blobs with specified prefix</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

<span data-ttu-id="ba399-133">Předpokládejme, že následující objekty BLOB se nacházejí v zadaném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ba399-133">Assume the following blobs reside in the specified container.</span></span> <span data-ttu-id="ba399-134">Všechny objekty BLOB začínající předponou `a` staženy:</span><span class="sxs-lookup"><span data-stu-id="ba399-134">All blobs beginning with the prefix `a` are downloaded:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="ba399-135">Po stažení operaci složce `C:\myfolder` zahrnuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="ba399-135">After the download operation, the folder `C:\myfolder` includes the following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

<span data-ttu-id="ba399-136">Předpona, která se vztahuje na virtuální adresář, který tvoří první část názvu objektu blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-136">The prefix applies to the virtual directory, which forms the first part of the blob name.</span></span> <span data-ttu-id="ba399-137">Ve výše uvedeném příkladu virtuální adresář neodpovídá zadané předpony, takže nebude stažen.</span><span class="sxs-lookup"><span data-stu-id="ba399-137">In the example shown above, the virtual directory does not match the specified prefix, so it is not downloaded.</span></span> <span data-ttu-id="ba399-138">Kromě toho pokud možnost `\S` není zadán, AzCopy nestáhne žádné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="ba399-138">In addition, if the option `\S` is not specified, AzCopy does not download any blobs.</span></span>

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a><span data-ttu-id="ba399-139">Nastavte čas poslední úpravy exportované soubory na stejnou hodnotu jako zdroj objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="ba399-139">Set the last-modified time of exported files to be same as the source blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

<span data-ttu-id="ba399-140">Můžete také vyloučit objekty BLOB z operace stažení podle jejich čas poslední změny.</span><span class="sxs-lookup"><span data-stu-id="ba399-140">You can also exclude blobs from the download operation based on their last-modified time.</span></span> <span data-ttu-id="ba399-141">Pokud chcete vyloučit objekty BLOB, jejichž poslední úpravy času je například stejnou nebo novější, než cílový soubor, přidejte `/XN` možnost:</span><span class="sxs-lookup"><span data-stu-id="ba399-141">For example, if you want to exclude blobs whose last modified time is the same or newer than the destination file, add the `/XN` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

<span data-ttu-id="ba399-142">Nebo pokud chcete vyloučit objekty BLOB, jejichž poslední úpravy času je na stejné nebo starší než cílový soubor, přidejte `/XO` možnost:</span><span class="sxs-lookup"><span data-stu-id="ba399-142">Or if you want to exclude blobs whose last modified time is the same or older than the destination file, add the `/XO` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a><span data-ttu-id="ba399-143">Objekt BLOB: nahrát</span><span class="sxs-lookup"><span data-stu-id="ba399-143">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="ba399-144">Odeslat jedním souborem</span><span class="sxs-lookup"><span data-stu-id="ba399-144">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="ba399-145">Pokud zadaný cílový kontejner neexistuje, AzCopy ji vytvoří a odešle soubor do ní.</span><span class="sxs-lookup"><span data-stu-id="ba399-145">If the specified destination container does not exist, AzCopy creates it and uploads the file into it.</span></span>

### <a name="upload-single-file-to-virtual-directory"></a><span data-ttu-id="ba399-146">Jediný soubor nahrát do virtuálního adresáře</span><span class="sxs-lookup"><span data-stu-id="ba399-146">Upload single file to virtual directory</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="ba399-147">Pokud zadaný virtuální adresář neexistuje, AzCopy nahrávání souboru ve svém názvu virtuálního adresáře (*například*, `vd/abc.txt` v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="ba399-147">If the specified virtual directory does not exist, AzCopy uploads the file to include the virtual directory in its name (*e.g.*, `vd/abc.txt` in the example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="ba399-148">Odeslat všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="ba399-148">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

<span data-ttu-id="ba399-149">Zadání `/S` odešle obsah zadaného adresáře k rekurzivnímu úložiště objektů Blob, což znamená, že jsou také odeslány všechny podsložky a jejich soubory.</span><span class="sxs-lookup"><span data-stu-id="ba399-149">Specifying option `/S` uploads the contents of the specified directory to Blob storage recursively, meaning that all subfolders and their files are uploaded as well.</span></span> <span data-ttu-id="ba399-150">Například předpokládejme následující soubory jsou umístěny ve složce `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="ba399-150">For instance, assume the following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="ba399-151">Po operaci odeslání kontejneru obsahuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="ba399-151">After the upload operation, the container includes the following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="ba399-152">Pokud nezadáte možnost `/S`, AzCopy nebyl odeslán rekurzivně.</span><span class="sxs-lookup"><span data-stu-id="ba399-152">If you do not specify option `/S`, AzCopy does not upload recursively.</span></span> <span data-ttu-id="ba399-153">Po operaci odeslání kontejneru obsahuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="ba399-153">After the upload operation, the container includes the following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="ba399-154">Nahrání souborů odpovídající zadanému vzoru</span><span class="sxs-lookup"><span data-stu-id="ba399-154">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

<span data-ttu-id="ba399-155">Předpokládejme následující soubory jsou umístěny ve složce `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="ba399-155">Assume the following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="ba399-156">Po operaci odeslání kontejneru obsahuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="ba399-156">After the upload operation, the container includes the following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="ba399-157">Pokud nezadáte možnost `/S`, AzCopy odešle pouze objekty BLOB, které nejsou jsou umístěny ve virtuálním adresáři:</span><span class="sxs-lookup"><span data-stu-id="ba399-157">If you do not specify option `/S`, AzCopy only uploads blobs that don't reside in a virtual directory:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="ba399-158">Zadejte typ MIME obsahu cílový objekt blob</span><span class="sxs-lookup"><span data-stu-id="ba399-158">Specify the MIME content type of a destination blob</span></span>
<span data-ttu-id="ba399-159">Ve výchozím nastavení, AzCopy nastaví typ obsahu cílový objekt blob do `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="ba399-159">By default, AzCopy sets the content type of a destination blob to `application/octet-stream`.</span></span> <span data-ttu-id="ba399-160">Počínaje verzí 3.1.0, lze explicitně zadat typ obsahu prostřednictvím možnosti `/SetContentType:[content-type]`.</span><span class="sxs-lookup"><span data-stu-id="ba399-160">Beginning with version 3.1.0, you can explicitly specify the content type via the option `/SetContentType:[content-type]`.</span></span> <span data-ttu-id="ba399-161">Tuto syntaxi nastaví typ obsahu pro všechny objekty BLOB v operaci odeslání.</span><span class="sxs-lookup"><span data-stu-id="ba399-161">This syntax sets the content type for all blobs in an upload operation.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

<span data-ttu-id="ba399-162">Pokud zadáte `/SetContentType` bez hodnoty AzCopy nastaví jednotlivých objektů blob nebo typ obsahu souboru podle jeho přípony.</span><span class="sxs-lookup"><span data-stu-id="ba399-162">If you specify `/SetContentType` without a value, AzCopy sets each blob or file's content type according to its file extension.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="blob-copy"></a><span data-ttu-id="ba399-163">Objekt BLOB: kopírování</span><span class="sxs-lookup"><span data-stu-id="ba399-163">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="ba399-164">Zkopírujte jediného objektu blob v rámci účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="ba399-164">Copy single blob within Storage account</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="ba399-165">Při kopírování objektu blob v účtu úložiště, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="ba399-165">When you copy a blob within a Storage account, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="ba399-166">Zkopírujte jediného objektu blob mezi různými účty úložiště</span><span class="sxs-lookup"><span data-stu-id="ba399-166">Copy single blob across Storage accounts</span></span>

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="ba399-167">Při kopírování objektu blob mezi různými účty úložiště [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="ba399-167">When you copy a blob across Storage accounts, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a><span data-ttu-id="ba399-168">Zkopírujte jediného objektu blob ze sekundární oblasti primární oblasti</span><span class="sxs-lookup"><span data-stu-id="ba399-168">Copy single blob from secondary region to primary region</span></span>

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="ba399-169">Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.</span><span class="sxs-lookup"><span data-stu-id="ba399-169">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="ba399-170">Zkopírujte jediného objektu blob a jeho snímky mezi různými účty úložiště</span><span class="sxs-lookup"><span data-stu-id="ba399-170">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

<span data-ttu-id="ba399-171">Po operaci kopírování zahrnuje cílový kontejner objektu blob a jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="ba399-171">After the copy operation, the target container includes the blob and its snapshots.</span></span> <span data-ttu-id="ba399-172">Za předpokladu, že objektu blob ve výše uvedeném příkladu má dva snímky, kontejner obsahuje následující objektů blob a snímky:</span><span class="sxs-lookup"><span data-stu-id="ba399-172">Assuming the blob in the example above has two snapshots, the container includes the following blob and snapshots:</span></span>

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="ba399-173">Synchronně kopírovat mezi různými účty úložiště objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="ba399-173">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="ba399-174">AzCopy ve výchozím nastavení zkopíruje data mezi dva koncové body úložiště asynchronně.</span><span class="sxs-lookup"><span data-stu-id="ba399-174">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="ba399-175">Proto se operace kopírování spouští pozadí využití kapacity přebytečné šířky pásma, který má žádné SLA z hlediska jak rychle se zkopíruje do objektu blob a AzCopy pravidelně kontroluje stav kopírování, dokud kopírování je dokončena nebo se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="ba399-175">Therefore, the copy operation runs in the background using spare bandwidth capacity that has no SLA in terms of how fast a blob is copied, and AzCopy periodically checks the copy status until the copying is completed or failed.</span></span>

<span data-ttu-id="ba399-176">`/SyncCopy` Možnost zajistí, že operace kopírování získá konzistentní rychlost.</span><span class="sxs-lookup"><span data-stu-id="ba399-176">The `/SyncCopy` option ensures that the copy operation gets consistent speed.</span></span> <span data-ttu-id="ba399-177">AzCopy provede synchronní kopie stažením objektů blob pro kopírování ze zadaného zdroje do místní paměti a uložte je do cílového umístění úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-177">AzCopy performs the synchronous copy by downloading the blobs to copy from the specified source to local memory, and then uploading them to the Blob storage destination.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

<span data-ttu-id="ba399-178">`/SyncCopy`může vygenerovat další odchozí nákladů ve srovnání s asynchronní kopírování, doporučuje se tuto možnost použít ve virtuálním počítači Azure, který je ve stejné oblasti jako váš účet úložiště zdroj předejdete odchozí náklady.</span><span class="sxs-lookup"><span data-stu-id="ba399-178">`/SyncCopy` might generate additional egress cost compared to asynchronous copy, the recommended approach is to use this option in an Azure VM that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="ba399-179">Soubor: stažení</span><span class="sxs-lookup"><span data-stu-id="ba399-179">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="ba399-180">Stáhnout jedním souborem</span><span class="sxs-lookup"><span data-stu-id="ba399-180">Download single file</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="ba399-181">Pokud je zadaný zdroj sdílenou složku Azure, pak buď musíte zadat přesný název souboru, (*například* `abc.txt`) ke stažení jeden soubor nebo zadejte možnost `/S` ke stažení všechny soubory ve sdílené složce rekurzivně.</span><span class="sxs-lookup"><span data-stu-id="ba399-181">If the specified source is an Azure file share, then you must either specify the exact file name, (*e.g.* `abc.txt`) to download a single file, or specify option `/S` to download all files in the share recursively.</span></span> <span data-ttu-id="ba399-182">Probíhá pokus o zadat šablonu souboru a možnost `/S` společně dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="ba399-182">Attempting to specify both a file pattern and option `/S` together results in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="ba399-183">Stáhnout všechny soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-183">Download all files</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="ba399-184">Všimněte si, že všechny prázdné složky nestahují.</span><span class="sxs-lookup"><span data-stu-id="ba399-184">Note that any empty folders are not downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="ba399-185">Soubor: nahrát</span><span class="sxs-lookup"><span data-stu-id="ba399-185">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="ba399-186">Odeslat jedním souborem</span><span class="sxs-lookup"><span data-stu-id="ba399-186">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="ba399-187">Odeslat všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="ba399-187">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

<span data-ttu-id="ba399-188">Všimněte si, že nejsou uloženy žádné prázdné složky.</span><span class="sxs-lookup"><span data-stu-id="ba399-188">Note that any empty folders are not uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="ba399-189">Nahrání souborů odpovídající zadanému vzoru</span><span class="sxs-lookup"><span data-stu-id="ba399-189">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="file-copy"></a><span data-ttu-id="ba399-190">Soubor: kopírování</span><span class="sxs-lookup"><span data-stu-id="ba399-190">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="ba399-191">Kopírování mezi sdílenými složkami</span><span class="sxs-lookup"><span data-stu-id="ba399-191">Copy across file shares</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="ba399-192">Při kopírování souboru mezi sdílenými složkami [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="ba399-192">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-to-blob"></a><span data-ttu-id="ba399-193">Kopírování ze sdílené složky do objektu blob</span><span class="sxs-lookup"><span data-stu-id="ba399-193">Copy from file share to blob</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="ba399-194">Při kopírování souboru ze sdílené složky do objektu blob, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="ba399-194">When you copy a file from file share to blob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>


### <a name="copy-from-blob-to-file-share"></a><span data-ttu-id="ba399-195">Zkopírujte z objektu blob do sdílené složky</span><span class="sxs-lookup"><span data-stu-id="ba399-195">Copy from blob to file share</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="ba399-196">Při kopírování souboru z objektu blob do sdílené složky [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="ba399-196">When you copy a file from blob to file share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="ba399-197">Synchronně kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="ba399-197">Synchronously copy files</span></span>
<span data-ttu-id="ba399-198">Můžete zadat `/SyncCopy` možnost ke zkopírování dat z úložiště souborů do úložiště File, ze souboru úložiště do úložiště objektů Blob a z úložiště objektů Blob k úložišti souborů synchronně, AzCopy k tomu stahování zdrojová data do místní paměti a nahrajte ho znovu do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="ba399-198">You can specify the `/SyncCopy` option to copy data from File Storage to File Storage, from File Storage to Blob Storage and from Blob Storage to File Storage synchronously, AzCopy does this by downloading the source data to local memory and upload it again to destination.</span></span> <span data-ttu-id="ba399-199">Standardní výstupní náklady platí.</span><span class="sxs-lookup"><span data-stu-id="ba399-199">Standard egress cost applies.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

<span data-ttu-id="ba399-200">Při kopírování ze souboru úložiště do úložiště objektů Blob, výchozí typ objektu blob je objekt blob bloku, může uživatel zadat možnost `/BlobType:page` Chcete-li změnit typ cílového objektu blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-200">When copying from File Storage to Blob Storage, the default blob type is block blob, user can specify option `/BlobType:page` to change the destination blob type.</span></span>

<span data-ttu-id="ba399-201">Všimněte si, že `/SyncCopy` může generovat další odchozí náklady srovnáním asynchronní kopírování, doporučuje se tuto možnost použít ve virtuálním počítači Azure, která je ve stejné oblasti jako váš účet úložiště zdroj předejdete odchozí náklady.</span><span class="sxs-lookup"><span data-stu-id="ba399-201">Note that `/SyncCopy` might generate additional egress cost comparing to asynchronous copy, the recommended approach is to use this option in the Azure VM which is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="table-export"></a><span data-ttu-id="ba399-202">Tabulka: Export</span><span class="sxs-lookup"><span data-stu-id="ba399-202">Table: Export</span></span>
### <a name="export-table"></a><span data-ttu-id="ba399-203">Export tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-203">Export table</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

<span data-ttu-id="ba399-204">AzCopy zapíše soubor manifestu do určené cílové složky.</span><span class="sxs-lookup"><span data-stu-id="ba399-204">AzCopy writes a manifest file to the specified destination folder.</span></span> <span data-ttu-id="ba399-205">Soubor manifestu se používá v procesu importu vyhledat potřebná data souborů a provést ověření dat.</span><span class="sxs-lookup"><span data-stu-id="ba399-205">The manifest file is used in the import process to locate the necessary data files and perform data validation.</span></span> <span data-ttu-id="ba399-206">Soubor manifestu používá ve výchozím nastavení této zásady vytváření názvů:</span><span class="sxs-lookup"><span data-stu-id="ba399-206">The manifest file uses the following naming convention by default:</span></span>

    <account name>_<table name>_<timestamp>.manifest

<span data-ttu-id="ba399-207">Uživatele můžete také zadat možnost `/Manifest:<manifest file name>` nastavit název souboru manifestu.</span><span class="sxs-lookup"><span data-stu-id="ba399-207">User can also specify the option `/Manifest:<manifest file name>` to set the manifest file name.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a><span data-ttu-id="ba399-208">Export rozdělené do několika souborů</span><span class="sxs-lookup"><span data-stu-id="ba399-208">Split export into multiple files</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

<span data-ttu-id="ba399-209">Používá AzCopy *svazku index* v názvech souborů dat rozdělení k rozlišení více souborů.</span><span class="sxs-lookup"><span data-stu-id="ba399-209">AzCopy uses a *volume index* in the split data file names to distinguish multiple files.</span></span> <span data-ttu-id="ba399-210">Index založený na svazku se skládá ze dvou částí *index klíče rozsahu oddílu* a *rozdělení souboru index*.</span><span class="sxs-lookup"><span data-stu-id="ba399-210">The volume index consists of two parts, a *partition key range index* and a *split file index*.</span></span> <span data-ttu-id="ba399-211">Obě indexy jsou od nuly.</span><span class="sxs-lookup"><span data-stu-id="ba399-211">Both indexes are zero-based.</span></span>

<span data-ttu-id="ba399-212">Index klíče rozsahu oddílu je 0, pokud uživatel není nastavena možnost `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="ba399-212">The partition key range index is 0 if the user does not specify option `/PKRS`.</span></span>

<span data-ttu-id="ba399-213">Předpokládejme například, AzCopy generuje dva soubory dat, až uživatel vybere možnost `/SplitSize`.</span><span class="sxs-lookup"><span data-stu-id="ba399-213">For instance, suppose AzCopy generates two data files after the user specifies option `/SplitSize`.</span></span> <span data-ttu-id="ba399-214">Výsledná data názvy souborů může být:</span><span class="sxs-lookup"><span data-stu-id="ba399-214">The resulting data file names might be:</span></span>

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

<span data-ttu-id="ba399-215">Všimněte si, že minimální možné hodnoty pro možnost `/SplitSize` je 32 MB.</span><span class="sxs-lookup"><span data-stu-id="ba399-215">Note that the minimum possible value for option `/SplitSize` is 32MB.</span></span> <span data-ttu-id="ba399-216">Pokud je zadaný cíl úložiště objektů Blob, AzCopy rozdělí datový soubor po jeho velikosti dosáhne omezení velikosti objektu blob (200GB), bez ohledu na tom, jestli možnost `/SplitSize` byla zadána uživatelem.</span><span class="sxs-lookup"><span data-stu-id="ba399-216">If the specified destination is Blob storage, AzCopy splits the data file once its sizes reaches the blob size limitation (200GB), regardless of whether option `/SplitSize` has been specified by the user.</span></span>

### <a name="export-table-to-json-or-csv-data-file-format"></a><span data-ttu-id="ba399-217">Export tabulky do formátu dat JSON nebo sdílený svazek clusteru</span><span class="sxs-lookup"><span data-stu-id="ba399-217">Export table to JSON or CSV data file format</span></span>
<span data-ttu-id="ba399-218">AzCopy ve výchozím nastavení exportuje tabulky soubory dat JSON.</span><span class="sxs-lookup"><span data-stu-id="ba399-218">AzCopy by default exports tables to JSON data files.</span></span> <span data-ttu-id="ba399-219">Můžete zadat možnost `/PayloadFormat:JSON|CSV` Export tabulky jako JSON nebo CSV.</span><span class="sxs-lookup"><span data-stu-id="ba399-219">You can specify the option `/PayloadFormat:JSON|CSV` to export the tables as JSON or CSV.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

<span data-ttu-id="ba399-220">Při zadávání Formát datové části sdíleného svazku clusteru, AzCopy taky generuje soubor schématu s příponou `.schema.csv` pro každý datový soubor.</span><span class="sxs-lookup"><span data-stu-id="ba399-220">When specifying the CSV payload format, AzCopy also generates a schema file with file extension `.schema.csv` for each data file.</span></span>

### <a name="export-table-entities-concurrently"></a><span data-ttu-id="ba399-221">Export tabulky souběžně entity</span><span class="sxs-lookup"><span data-stu-id="ba399-221">Export table entities concurrently</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

<span data-ttu-id="ba399-222">AzCopy spustí souběžných operací pro export entity, když uživatel vybere možnost `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="ba399-222">AzCopy starts concurrent operations to export entities when the user specifies option `/PKRS`.</span></span> <span data-ttu-id="ba399-223">Každé operace exportuje jeden rozsah klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="ba399-223">Each operation exports one partition key range.</span></span>

<span data-ttu-id="ba399-224">Všimněte si, že počet souběžných operací se řídí možnost `/NC`.</span><span class="sxs-lookup"><span data-stu-id="ba399-224">Note that the number of concurrent operations is also controlled by option `/NC`.</span></span> <span data-ttu-id="ba399-225">AzCopy používá jako výchozí hodnota počet jader procesorů `/NC` při kopírování entity tabulky, i když `/NC` nebyl zadán.</span><span class="sxs-lookup"><span data-stu-id="ba399-225">AzCopy uses the number of core processors as the default value of `/NC` when copying table entities, even if `/NC` was not specified.</span></span> <span data-ttu-id="ba399-226">Když uživatel vybere možnost `/PKRS`, AzCopy používá menší ze dvou hodnot – rozsahy klíčů oddílu versus implicitně nebo explicitně zadaná souběžných operací – k určení počtu souběžných operací spustit.</span><span class="sxs-lookup"><span data-stu-id="ba399-226">When the user specifies option `/PKRS`, AzCopy uses the smaller of the two values - partition key ranges versus implicitly or explicitly specified concurrent operations - to determine the number of concurrent operations to start.</span></span> <span data-ttu-id="ba399-227">Další podrobnosti, zadejte `AzCopy /?:NC` na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="ba399-227">For more details, type `AzCopy /?:NC` at the command line.</span></span>

### <a name="export-table-to-blob"></a><span data-ttu-id="ba399-228">Export tabulky do objektu blob</span><span class="sxs-lookup"><span data-stu-id="ba399-228">Export table to blob</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

<span data-ttu-id="ba399-229">AzCopy generuje soubor dat JSON do kontejneru objektů blob s následující zásady vytváření názvů:</span><span class="sxs-lookup"><span data-stu-id="ba399-229">AzCopy generates a JSON data file into the blob container with following naming convention:</span></span>

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

<span data-ttu-id="ba399-230">Vygenerovaný soubor dat JSON následuje Formát datové části pro – minimální metadata.</span><span class="sxs-lookup"><span data-stu-id="ba399-230">The generated JSON data file follows the payload format for minimal metadata.</span></span> <span data-ttu-id="ba399-231">Podrobnosti o této Formát datové části v tématu [Formát datové části pro operace služby s tabulkou](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba399-231">For details on this payload format, see [Payload Format for Table Service Operations](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span></span>

<span data-ttu-id="ba399-232">Upozorňujeme, že při exportu tabulky do objektů BLOB, AzCopy stahování entity tabulky do místní dočasné datové soubory a pak odešle tyto entity do objektu blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-232">Note that when exporting tables to blobs, AzCopy downloads the Table entities to local temporary data files and then uploads those entities to the blob.</span></span> <span data-ttu-id="ba399-233">Tyto soubory dočasná data budou umístěny do složky souboru deníku s výchozí cestou "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", můžete zadat možnost/Z: [deníku – soubor a složka] deník změnit umístění složky souborů a proto změňte umístění souborů dočasná data.</span><span class="sxs-lookup"><span data-stu-id="ba399-233">These temporary data files are put into the journal file folder with the default path "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", you can specify option /Z:[journal-file-folder] to change the journal file folder location and thus change the temporary data files location.</span></span> <span data-ttu-id="ba399-234">Velikost dočasné datové soubory, které je určeno podle velikosti entity tabulky a velikost, který jste zadali pomocí /SplitSize možnost, i když soubor dočasná data v místním disku je okamžitě odstraněn, až se nahrají na objekt blob, ujistěte se, že máte dostatek místa na místním disku k ukládání těchto souborů dočasná data před jejich odstraněním.</span><span class="sxs-lookup"><span data-stu-id="ba399-234">The temporary data files' size is decided by your table entities' size and the size you specified with the option /SplitSize, although the temporary data file in local disk is deleted instantly once it has been uploaded to the blob, please make sure you have enough local disk space to store these temporary data files before they are deleted.</span></span>

## <a name="table-import"></a><span data-ttu-id="ba399-235">Tabulka: Import</span><span class="sxs-lookup"><span data-stu-id="ba399-235">Table: Import</span></span>
### <a name="import-table"></a><span data-ttu-id="ba399-236">Tabulky import</span><span class="sxs-lookup"><span data-stu-id="ba399-236">Import table</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

<span data-ttu-id="ba399-237">Možnost `/EntityOperation` určuje způsob vložení entity do tabulky.</span><span class="sxs-lookup"><span data-stu-id="ba399-237">The option `/EntityOperation` indicates how to insert entities into the table.</span></span> <span data-ttu-id="ba399-238">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ba399-238">Possible values are:</span></span>

* <span data-ttu-id="ba399-239">`InsertOrSkip`: Přeskočí stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce.</span><span class="sxs-lookup"><span data-stu-id="ba399-239">`InsertOrSkip`: Skips an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="ba399-240">`InsertOrMerge`: Sloučí existující entitu nebo vloží novou entitu, pokud neexistuje v tabulce.</span><span class="sxs-lookup"><span data-stu-id="ba399-240">`InsertOrMerge`: Merges an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="ba399-241">`InsertOrReplace`: Nahrazuje stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce.</span><span class="sxs-lookup"><span data-stu-id="ba399-241">`InsertOrReplace`: Replaces an existing entity or inserts a new entity if it does not exist in the table.</span></span>

<span data-ttu-id="ba399-242">Všimněte si, že nelze zadat možnost `/PKRS` ve scénáři importu.</span><span class="sxs-lookup"><span data-stu-id="ba399-242">Note that you cannot specify option `/PKRS` in the import scenario.</span></span> <span data-ttu-id="ba399-243">Na rozdíl od export scénář, ve kterém musíte zadat možnost `/PKRS` spuštění souběžných operací, AzCopy spuštěním souběžných operací ve výchozím nastavení při importu tabulky.</span><span class="sxs-lookup"><span data-stu-id="ba399-243">Unlike the export scenario, in which you must specify option `/PKRS` to start concurrent operations, AzCopy starts concurrent operations by default when you import a table.</span></span> <span data-ttu-id="ba399-244">Výchozí počet souběžných operací spuštění se rovná počet jader procesorů; Můžete však zadat jiný počet souběžných s možností `/NC`.</span><span class="sxs-lookup"><span data-stu-id="ba399-244">The default number of concurrent operations started is equal to the number of core processors; however, you can specify a different number of concurrent with option `/NC`.</span></span> <span data-ttu-id="ba399-245">Další podrobnosti, zadejte `AzCopy /?:NC` na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="ba399-245">For more details, type `AzCopy /?:NC` at the command line.</span></span>

<span data-ttu-id="ba399-246">Všimněte si, že AzCopy podporuje pouze import pro formát JSON, není sdílený svazek clusteru.</span><span class="sxs-lookup"><span data-stu-id="ba399-246">Note that AzCopy only supports importing for JSON, not CSV.</span></span> <span data-ttu-id="ba399-247">AzCopy nepodporuje import tabulky z vytvořené uživatelem JSON a manifest soubory.</span><span class="sxs-lookup"><span data-stu-id="ba399-247">AzCopy does not support table imports from user-created JSON and manifest files.</span></span> <span data-ttu-id="ba399-248">Oba tyto soubory musí pocházet z tabulky exportního AzCopy.</span><span class="sxs-lookup"><span data-stu-id="ba399-248">Both of these files must come from an AzCopy table export.</span></span> <span data-ttu-id="ba399-249">Abyste se vyhnuli chybám, prosím neupravujte exportovaný JSON a soubor manifestu.</span><span class="sxs-lookup"><span data-stu-id="ba399-249">To avoid errors, please do not modify the exported JSON or manifest file.</span></span>

### <a name="import-entities-to-table-using-blobs"></a><span data-ttu-id="ba399-250">Importovat entity do tabulky pomocí objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="ba399-250">Import entities to table using blobs</span></span>
<span data-ttu-id="ba399-251">Předpokládejme, kontejner objektů Blob obsahuje následující: souboru A JSON představující Azure Table a jeho doprovodné soubor manifestu.</span><span class="sxs-lookup"><span data-stu-id="ba399-251">Assume a Blob container contains the following: A JSON file representing an Azure Table and its accompanying manifest file.</span></span>

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

<span data-ttu-id="ba399-252">Spuštěním následujícího příkazu pro import entity do tabulky s použitím souboru manifestu v tomto kontejneru objektů blob:</span><span class="sxs-lookup"><span data-stu-id="ba399-252">You can run the following command to import entities into a table using the manifest file in that blob container:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a><span data-ttu-id="ba399-253">Další funkce AzCopy</span><span class="sxs-lookup"><span data-stu-id="ba399-253">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a><span data-ttu-id="ba399-254">Kopírovat pouze data, která neexistuje v cílovém</span><span class="sxs-lookup"><span data-stu-id="ba399-254">Only copy data that doesn't exist in the destination</span></span>
<span data-ttu-id="ba399-255">`/XO` a `/XN` parametry umožňují vyloučit prostředky starší nebo novější zdroj z kopírování, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="ba399-255">The `/XO` and `/XN` parameters allow you to exclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="ba399-256">Pokud chcete kopírovat zdroje prostředky, které neexistují v cílovém, můžete zadat oba parametry v příkazu AzCopy:</span><span class="sxs-lookup"><span data-stu-id="ba399-256">If you only want to copy source resources that don't exist in the destination, you can specify both parameters in the AzCopy command:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

<span data-ttu-id="ba399-257">Všimněte si, že to není podporováno zdrojové nebo cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="ba399-257">Note that this is not supported when either the source or destination is a table.</span></span>

### <a name="use-a-response-file-to-specify-command-line-parameters"></a><span data-ttu-id="ba399-258">Zadejte parametry příkazového řádku pomocí souboru odpovědí</span><span class="sxs-lookup"><span data-stu-id="ba399-258">Use a response file to specify command-line parameters</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

<span data-ttu-id="ba399-259">Parametry příkazového řádku AzCopy můžete zahrnout do souboru odpovědí.</span><span class="sxs-lookup"><span data-stu-id="ba399-259">You can include any AzCopy command-line parameters in a response file.</span></span> <span data-ttu-id="ba399-260">AzCopy zpracovává parametry v souboru jako v případě, kdyby byly zadány na příkazovém řádku, provádění přímé nahrazení s obsahem souboru.</span><span class="sxs-lookup"><span data-stu-id="ba399-260">AzCopy processes the parameters in the file as if they had been specified on the command line, performing a direct substitution with the contents of the file.</span></span>

<span data-ttu-id="ba399-261">Předpokládejme, soubor odpovědí s názvem `copyoperation.txt`, který obsahuje následující řádky.</span><span class="sxs-lookup"><span data-stu-id="ba399-261">Assume a response file named `copyoperation.txt`, that contains the following lines.</span></span> <span data-ttu-id="ba399-262">Každý parametr AzCopy můžete zadat na jeden řádek</span><span class="sxs-lookup"><span data-stu-id="ba399-262">Each AzCopy parameter can be specified on a single line</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

<span data-ttu-id="ba399-263">nebo na samostatné řádky:</span><span class="sxs-lookup"><span data-stu-id="ba399-263">or on separate lines:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

<span data-ttu-id="ba399-264">AzCopy selže, pokud rozložení parametru mezi dvěma čárami, jak je vidět tady pro `/sourcekey` parametr:</span><span class="sxs-lookup"><span data-stu-id="ba399-264">AzCopy fails if you split the parameter across two lines, as shown here for the `/sourcekey` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a><span data-ttu-id="ba399-265">Zadejte parametry příkazového řádku pomocí několika soubory odezvy</span><span class="sxs-lookup"><span data-stu-id="ba399-265">Use multiple response files to specify command-line parameters</span></span>
<span data-ttu-id="ba399-266">Předpokládejme, soubor odpovědí s názvem `source.txt` kontejner zdroj, který určuje:</span><span class="sxs-lookup"><span data-stu-id="ba399-266">Assume a response file named `source.txt` that specifies a source container:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer

<span data-ttu-id="ba399-267">A soubor odpovědí s názvem `dest.txt` určující cílovou složku v systému souborů:</span><span class="sxs-lookup"><span data-stu-id="ba399-267">And a response file named `dest.txt` that specifies a destination folder in the file system:</span></span>

    /Dest:C:\myfolder

<span data-ttu-id="ba399-268">A soubor odpovědí s názvem `options.txt` určující možnosti AzCopy:</span><span class="sxs-lookup"><span data-stu-id="ba399-268">And a response file named `options.txt` that specifies options for AzCopy:</span></span>

    /S /Y

<span data-ttu-id="ba399-269">Volat AzCopy s těmito soubory odpovědí, které jsou umístěny v adresáři `C:\responsefiles`, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="ba399-269">To call AzCopy with these response files, all of which reside in a directory `C:\responsefiles`, use this command:</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

<span data-ttu-id="ba399-270">AzCopy zpracovává tento příkaz stejně, jako kdyby jste zahrnuli všechny jednotlivé parametry na příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="ba399-270">AzCopy processes this command just as it would if you included all of the individual parameters on the command line:</span></span>

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="ba399-271">Zadejte sdílený přístupový podpis (SAS)</span><span class="sxs-lookup"><span data-stu-id="ba399-271">Specify a shared access signature (SAS)</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

<span data-ttu-id="ba399-272">Můžete také určit SAS URI kontejneru:</span><span class="sxs-lookup"><span data-stu-id="ba399-272">You can also specify a SAS on the container URI:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a><span data-ttu-id="ba399-273">Složky pro soubor deníku</span><span class="sxs-lookup"><span data-stu-id="ba399-273">Journal file folder</span></span>
<span data-ttu-id="ba399-274">Pokaždé, když příkaz azcopy, zkontroluje existenci souboru deníku ve výchozí složce, nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti.</span><span class="sxs-lookup"><span data-stu-id="ba399-274">Each time you issue a command to AzCopy, it checks whether a journal file exists in the default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="ba399-275">Pokud soubor deníku neexistuje ani na jednom místě, AzCopy zpracovává operaci jako nové a generuje nový soubor deníku.</span><span class="sxs-lookup"><span data-stu-id="ba399-275">If the journal file does not exist in either place, AzCopy treats the operation as new and generates a new journal file.</span></span>

<span data-ttu-id="ba399-276">Pokud soubor deníku neexistuje, AzCopy kontroluje, zda příkazového řádku, který můžete vložit odpovídá příkazového řádku v souboru deníku.</span><span class="sxs-lookup"><span data-stu-id="ba399-276">If the journal file does exist, AzCopy checks whether the command line that you input matches the command line in the journal file.</span></span> <span data-ttu-id="ba399-277">Pokud se dva příkazové řádky shodují, obnoví AzCopy neúplné operaci.</span><span class="sxs-lookup"><span data-stu-id="ba399-277">If the two command lines match, AzCopy resumes the incomplete operation.</span></span> <span data-ttu-id="ba399-278">Pokud se neshodují, zobrazí se výzva k buď přepsat soubor deníku spustit novou operaci, nebo zrušit aktuální operace.</span><span class="sxs-lookup"><span data-stu-id="ba399-278">If they do not match, you are prompted to either overwrite the journal file to start a new operation, or to cancel the current operation.</span></span>

<span data-ttu-id="ba399-279">Pokud chcete použít výchozí umístění pro soubor deníku:</span><span class="sxs-lookup"><span data-stu-id="ba399-279">If you want to use the default location for the journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

<span data-ttu-id="ba399-280">Pokud není možnost `/Z`, nebo zadejte možnost `/Z` bez cesta ke složce, jako v příkladu nahoře, AzCopy vytvoří soubor deníku ve výchozím umístění, což je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="ba399-280">If you omit option `/Z`, or specify option `/Z` without the folder path, as shown above, AzCopy creates the journal file in the default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="ba399-281">Pokud soubor deníku již existuje, AzCopy obnoví operaci na základě souboru deníku.</span><span class="sxs-lookup"><span data-stu-id="ba399-281">If the journal file already exists, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="ba399-282">Pokud chcete určit vlastní umístění souboru deníku:</span><span class="sxs-lookup"><span data-stu-id="ba399-282">If you want to specify a custom location for the journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

<span data-ttu-id="ba399-283">Tento příklad vytvoří soubor deníku, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ba399-283">This example creates the journal file if it does not already exist.</span></span> <span data-ttu-id="ba399-284">Pokud neexistuje, AzCopy obnoví operaci na základě souboru deníku.</span><span class="sxs-lookup"><span data-stu-id="ba399-284">If it does exist, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="ba399-285">Pokud chcete pokračovat v činnosti AzCopy:</span><span class="sxs-lookup"><span data-stu-id="ba399-285">If you want to resume an AzCopy operation:</span></span>

```azcopy
AzCopy /Z:C:\journalfolder\
```

<span data-ttu-id="ba399-286">Tento příklad obnoví poslední operace, které může se nepodařilo dokončit.</span><span class="sxs-lookup"><span data-stu-id="ba399-286">This example resumes the last operation, which may have failed to complete.</span></span>

### <a name="generate-a-log-file"></a><span data-ttu-id="ba399-287">Generování souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="ba399-287">Generate a log file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

<span data-ttu-id="ba399-288">Pokud zadáte možnost `/V` bez zadání cestu k souboru na úplné protokolování, pak AzCopy vytváří protokolový soubor ve výchozím umístění, což je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="ba399-288">If you specify option `/V` without providing a file path to the verbose log, then AzCopy creates the log file in the default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span>

<span data-ttu-id="ba399-289">Jinak můžete vytvořit soubor protokolu v do vlastního umístění:</span><span class="sxs-lookup"><span data-stu-id="ba399-289">Otherwise, you can create an log file in a custom location:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

<span data-ttu-id="ba399-290">Všimněte si, že pokud zadáte relativní cestu následující možnost `/V`, jako například `/V:test/azcopy1.log`, pak podrobného protokolování se vytvoří v aktuálním pracovním adresáři v podsložce s názvem `test`.</span><span class="sxs-lookup"><span data-stu-id="ba399-290">Note that if you specify a relative path following option `/V`, such as `/V:test/azcopy1.log`, then the verbose log is created in the current working directory within a subfolder named `test`.</span></span>

### <a name="specify-the-number-of-concurrent-operations-to-start"></a><span data-ttu-id="ba399-291">Zadejte počet souběžných operací spuštění</span><span class="sxs-lookup"><span data-stu-id="ba399-291">Specify the number of concurrent operations to start</span></span>
<span data-ttu-id="ba399-292">Možnost `/NC` určuje počet souběžných kopie operací.</span><span class="sxs-lookup"><span data-stu-id="ba399-292">Option `/NC` specifies the number of concurrent copy operations.</span></span> <span data-ttu-id="ba399-293">Ve výchozím nastavení spustí AzCopy počet souběžných operací, pokud chcete zvýšit propustnost dat přenosu.</span><span class="sxs-lookup"><span data-stu-id="ba399-293">By default, AzCopy starts a certain number of concurrent operations to increase the data transfer throughput.</span></span> <span data-ttu-id="ba399-294">Pro operace s tabulkou počet souběžných operací se rovná počet procesorů, které máte.</span><span class="sxs-lookup"><span data-stu-id="ba399-294">For Table operations, the number of concurrent operations is equal to the number of processors you have.</span></span> <span data-ttu-id="ba399-295">Pro operace objektů Blob a souboru, počet souběžných operací je rovnat 8 časy počet procesorů, které máte.</span><span class="sxs-lookup"><span data-stu-id="ba399-295">For Blob and File operations, the number of concurrent operations is equal 8 times the number of processors you have.</span></span> <span data-ttu-id="ba399-296">Pokud používáte AzCopy přes síť s malou šířkou pásma, můžete zadat nižší číslo pro /NC, aby se zabránilo selhání kvůli konfliktům prostředků.</span><span class="sxs-lookup"><span data-stu-id="ba399-296">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for /NC to avoid failure caused by resource competition.</span></span>

### <a name="run-azcopy-against-azure-storage-emulator"></a><span data-ttu-id="ba399-297">Spusťte nástroj AzCopy emulátoru úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="ba399-297">Run AzCopy against Azure Storage Emulator</span></span>
<span data-ttu-id="ba399-298">Můžete spustit AzCopy proti [emulátoru úložiště Azure](storage-use-emulator.md) pro objekty BLOB:</span><span class="sxs-lookup"><span data-stu-id="ba399-298">You can run AzCopy against the [Azure Storage Emulator](storage-use-emulator.md) for Blobs:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

<span data-ttu-id="ba399-299">a tabulky:</span><span class="sxs-lookup"><span data-stu-id="ba399-299">and Tables:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a><span data-ttu-id="ba399-300">Parametry AzCopy</span><span class="sxs-lookup"><span data-stu-id="ba399-300">AzCopy Parameters</span></span>
<span data-ttu-id="ba399-301">Parametry pro AzCopy jsou popsané níže.</span><span class="sxs-lookup"><span data-stu-id="ba399-301">Parameters for AzCopy are described below.</span></span> <span data-ttu-id="ba399-302">Můžete také zadat jednu z následujících příkazů z příkazového řádku nápovědu pomocí nástroje AzCopy:</span><span class="sxs-lookup"><span data-stu-id="ba399-302">You can also type one of the following commands from the command line for help in using AzCopy:</span></span>

* <span data-ttu-id="ba399-303">Podrobnou nápovědu příkazového řádku pro AzCopy:`AzCopy /?`</span><span class="sxs-lookup"><span data-stu-id="ba399-303">For detailed command-line help for AzCopy: `AzCopy /?`</span></span>
* <span data-ttu-id="ba399-304">Další informace o všech AzCopy parametr:`AzCopy /?:SourceKey`</span><span class="sxs-lookup"><span data-stu-id="ba399-304">For detailed help with any AzCopy parameter: `AzCopy /?:SourceKey`</span></span>
* <span data-ttu-id="ba399-305">Příklady příkazového řádku:`AzCopy /?:Samples`</span><span class="sxs-lookup"><span data-stu-id="ba399-305">For command-line examples: `AzCopy /?:Samples`</span></span>

### <a name="sourcesource"></a><span data-ttu-id="ba399-306">/ Zdroj: "zdroj"</span><span class="sxs-lookup"><span data-stu-id="ba399-306">/Source:"source"</span></span>
<span data-ttu-id="ba399-307">Určuje zdroj dat ze kterého chcete zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="ba399-307">Specifies the source data from which to copy.</span></span> <span data-ttu-id="ba399-308">Zdrojem může být adresář systému souborů, kontejner objektů blob, virtuální adresář objektů blob, sdílené složky úložiště, adresáři se souborem úložiště nebo Azure table.</span><span class="sxs-lookup"><span data-stu-id="ba399-308">The source can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="ba399-309">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-309">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destdestination"></a><span data-ttu-id="ba399-310">/ Cíle: "cílové"</span><span class="sxs-lookup"><span data-stu-id="ba399-310">/Dest:"destination"</span></span>
<span data-ttu-id="ba399-311">Určuje cíl pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="ba399-311">Specifies the destination to copy to.</span></span> <span data-ttu-id="ba399-312">Cílem může být adresář systému souborů, kontejner objektů blob, virtuální adresář objektů blob, sdílené složky úložiště, adresáři se souborem úložiště nebo Azure table.</span><span class="sxs-lookup"><span data-stu-id="ba399-312">The destination can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="ba399-313">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-313">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="patternfile-pattern"></a><span data-ttu-id="ba399-314">/ Vzor: "vzor souborů"</span><span class="sxs-lookup"><span data-stu-id="ba399-314">/Pattern:"file-pattern"</span></span>
<span data-ttu-id="ba399-315">Určuje vzor souborů, který označuje soubory, které chcete kopírovat.</span><span class="sxs-lookup"><span data-stu-id="ba399-315">Specifies a file pattern that indicates which files to copy.</span></span> <span data-ttu-id="ba399-316">Chování parametr /Pattern je určen podle umístění zdroje dat a přítomnost možnost rekurzivní režim.</span><span class="sxs-lookup"><span data-stu-id="ba399-316">The behavior of the /Pattern parameter is determined by the location of the source data, and the presence of the recursive mode option.</span></span> <span data-ttu-id="ba399-317">Prostřednictvím možnosti parametrem/s. je zadán rekurzivní režim</span><span class="sxs-lookup"><span data-stu-id="ba399-317">Recursive mode is specified via option /S.</span></span>

<span data-ttu-id="ba399-318">Pokud zadaný zdrojový adresář v systému souborů, pak jsou platné standardní zástupné znaky a vzor souborů poskytuje je shoda na základě souborů v adresáři.</span><span class="sxs-lookup"><span data-stu-id="ba399-318">If the specified source is a directory in the file system, then standard wildcards are in effect, and the file pattern provided is matched against files within the directory.</span></span> <span data-ttu-id="ba399-319">Pokud možnost, který je zadán parametr, pak AzCopy taky odpovídá zadanému vzoru proti všechny soubory ve všech podsložek pod adresářem.</span><span class="sxs-lookup"><span data-stu-id="ba399-319">If option /S is specified, then AzCopy also matches the specified pattern against all files in any subfolders beneath the directory.</span></span>

<span data-ttu-id="ba399-320">Pokud zadaný zdroj je kontejner objektů blob nebo virtuální adresář, nejsou použity zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="ba399-320">If the specified source is a blob container or virtual directory, then wildcards are not applied.</span></span> <span data-ttu-id="ba399-321">Pokud je možnost, který je zadán parametr, pak AzCopy interpretuje jako předpona objektu blob vzoru zadaný soubor.</span><span class="sxs-lookup"><span data-stu-id="ba399-321">If option /S is specified, then AzCopy interprets the specified file pattern as a blob prefix.</span></span> <span data-ttu-id="ba399-322">Pokud možnosti, kterou není zadán parametr, pak AzCopy odpovídá vzoru souboru s názvy objektů blob přesný.</span><span class="sxs-lookup"><span data-stu-id="ba399-322">If option /S is not specified, then AzCopy matches the file pattern against exact blob names.</span></span>

<span data-ttu-id="ba399-323">Pokud se zadaný zdroj sdílenou složku Azure, pak musí buď zadejte přesný název souboru (například abc.txt) zkopírovat jeden soubor nebo zadejte možnost /S zkopíruje všechny soubory ve sdílené složce rekurzivně.</span><span class="sxs-lookup"><span data-stu-id="ba399-323">If the specified source is an Azure file share, then you must either specify the exact file name, (e.g. abc.txt) to copy a single file, or specify option /S to copy all files in the share recursively.</span></span> <span data-ttu-id="ba399-324">Probíhá pokus o zadejte oba souboru vzor a možnost /S společně výsledků v chybě.</span><span class="sxs-lookup"><span data-stu-id="ba399-324">Attempting to specify both a file pattern and option /S together results in an error.</span></span>

<span data-ttu-id="ba399-325">AzCopy používá, když / Source je kontejner objektů blob nebo virtuální adresář objektů blob a používá porovnávání ve všech ostatních případech odpovídající malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="ba399-325">AzCopy uses case-sensitive matching when the /Source is a blob container or blob virtual directory, and uses case-insensitive matching in all the other cases.</span></span>

<span data-ttu-id="ba399-326">Soubor výchozím způsobem používaným při žádné vzor souborů je *.*</span><span class="sxs-lookup"><span data-stu-id="ba399-326">The default file pattern used when no file pattern is specified is *.*</span></span> <span data-ttu-id="ba399-327">pro umístění systému souborů nebo prázdnou předponu pro umístění služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="ba399-327">for a file system location or an empty prefix for an Azure Storage location.</span></span> <span data-ttu-id="ba399-328">Zadání více vzorů souborů není podporováno.</span><span class="sxs-lookup"><span data-stu-id="ba399-328">Specifying multiple file patterns is not supported.</span></span>

<span data-ttu-id="ba399-329">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-329">**Applicable to:** Blobs, Files</span></span>

### <a name="destkeystorage-key"></a><span data-ttu-id="ba399-330">/ DestKey: "klíč úložiště"</span><span class="sxs-lookup"><span data-stu-id="ba399-330">/DestKey:"storage-key"</span></span>
<span data-ttu-id="ba399-331">Určuje klíč účtu úložiště pro cílový prostředek.</span><span class="sxs-lookup"><span data-stu-id="ba399-331">Specifies the storage account key for the destination resource.</span></span>

<span data-ttu-id="ba399-332">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-332">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destsassas-token"></a><span data-ttu-id="ba399-333">/ DestSAS: "tokenu sas"</span><span class="sxs-lookup"><span data-stu-id="ba399-333">/DestSAS:"sas-token"</span></span>
<span data-ttu-id="ba399-334">Určuje sdíleného přístupového podpisu (SAS) s oprávněními ke čtení a zápisu pro cíl (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="ba399-334">Specifies a Shared Access Signature (SAS) with READ and WRITE permissions for the destination (if applicable).</span></span> <span data-ttu-id="ba399-335">Obklopit SAS s dvojité uvozovky, protože pravděpodobně obsahuje speciální znaky příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ba399-335">Surround the SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="ba399-336">Pokud cílový prostředek je kontejner objektů blob, sdílené složky nebo tabulky, můžete buď zadat tuto možnost, za nímž následuje tokenu SAS nebo jako součást cílový kontejner objektů blob, sdílené složky nebo identifikátor URI tabulky bez této možnosti můžete zadat SAS.</span><span class="sxs-lookup"><span data-stu-id="ba399-336">If the destination resource is a blob container, file share or table, you can either specify this option followed by the SAS token, or you can specify the SAS as part of the destination blob container, file share or table's URI, without this option.</span></span>

<span data-ttu-id="ba399-337">Pokud zdrojové a cílové jsou oba objekty BLOB, pak cílový objekt blob se musí nacházet ve stejném účtu úložiště jako zdrojový objekt blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-337">If the source and destination are both blobs, then the destination blob must reside within the same storage account as the source blob.</span></span>

<span data-ttu-id="ba399-338">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-338">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcekeystorage-key"></a><span data-ttu-id="ba399-339">/ SourceKey: "klíč úložiště"</span><span class="sxs-lookup"><span data-stu-id="ba399-339">/SourceKey:"storage-key"</span></span>
<span data-ttu-id="ba399-340">Určuje klíč účtu úložiště pro zdrojovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="ba399-340">Specifies the storage account key for the source resource.</span></span>

<span data-ttu-id="ba399-341">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-341">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcesassas-token"></a><span data-ttu-id="ba399-342">/ SourceSAS: "tokenu sas"</span><span class="sxs-lookup"><span data-stu-id="ba399-342">/SourceSAS:"sas-token"</span></span>
<span data-ttu-id="ba399-343">Určuje sdíleného přístupového podpisu oprávnění ke čtení a seznamu pro zdroj (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="ba399-343">Specifies a Shared Access Signature with READ and LIST permissions for the source (if applicable).</span></span> <span data-ttu-id="ba399-344">Obklopit SAS s dvojité uvozovky, protože pravděpodobně obsahuje speciální znaky příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ba399-344">Surround the SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="ba399-345">Pokud zdroj prostředků je kontejner objektů blob a je k dispozici klíč ani SAS, je pro kontejner objektů blob čtení přes anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="ba399-345">If the source resource is a blob container, and neither a key nor a SAS is provided, then the blob container is read via anonymous access.</span></span>

<span data-ttu-id="ba399-346">Pokud je zdroj sdílené složky nebo tabulka, je třeba zadat klíč nebo SAS.</span><span class="sxs-lookup"><span data-stu-id="ba399-346">If the source is a file share or table, a key or a SAS must be provided.</span></span>

<span data-ttu-id="ba399-347">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-347">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="s"></a><span data-ttu-id="ba399-348">/S</span><span class="sxs-lookup"><span data-stu-id="ba399-348">/S</span></span>
<span data-ttu-id="ba399-349">Určuje režim rekurzivní operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="ba399-349">Specifies recursive mode for copy operations.</span></span> <span data-ttu-id="ba399-350">V režimu rekurzivní AzCopy zkopíruje všechny objekty BLOB nebo soubory, které se shodují se vzorem zadaný soubor, včetně programů v podsložkách.</span><span class="sxs-lookup"><span data-stu-id="ba399-350">In recursive mode, AzCopy copies all blobs or files that match the specified file pattern, including those in subfolders.</span></span>

<span data-ttu-id="ba399-351">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-351">**Applicable to:** Blobs, Files</span></span>

### <a name="blobtypeblock--page--append"></a><span data-ttu-id="ba399-352">/ BlobType: "blokem" | "stránka" | "připojit"</span><span class="sxs-lookup"><span data-stu-id="ba399-352">/BlobType:"block" | "page" | "append"</span></span>
<span data-ttu-id="ba399-353">Určuje, zda cílový objekt blob je objekt blob bloku, objektů blob stránky nebo doplňovací objekt blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-353">Specifies whether the destination blob is a block blob, a page blob, or an append blob.</span></span> <span data-ttu-id="ba399-354">Tuto možnost lze použít pouze v případě, že nahráváte do objektu blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-354">This option is applicable only when you are uploading a blob.</span></span> <span data-ttu-id="ba399-355">Jinak je generována chyba.</span><span class="sxs-lookup"><span data-stu-id="ba399-355">Otherwise, an error is generated.</span></span> <span data-ttu-id="ba399-356">Pokud cílový objekt blob a není tato možnost zadána, ve výchozím nastavení, AzCopy vytvoří objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="ba399-356">If the destination is a blob and this option is not specified, by default, AzCopy creates a block blob.</span></span>

<span data-ttu-id="ba399-357">**Platí pro:** objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="ba399-357">**Applicable to:** Blobs</span></span>

### <a name="checkmd5"></a><span data-ttu-id="ba399-358">/ CheckMD5</span><span class="sxs-lookup"><span data-stu-id="ba399-358">/CheckMD5</span></span>
<span data-ttu-id="ba399-359">Vypočítá hodnotu hash MD5 pro stažená data a ověří, zda hodnota hash MD5 uložené v objektu blob nebo vlastnost MD5 obsah souboru odpovídá vypočtený hash.</span><span class="sxs-lookup"><span data-stu-id="ba399-359">Calculates an MD5 hash for downloaded data and verifies that the MD5 hash stored in the blob or file's Content-MD5 property matches the calculated hash.</span></span> <span data-ttu-id="ba399-360">Kontrola MD5 je vypnutý ve výchozím nastavení, proto musíte určit tuto možnost, při stahování dat provést kontrolu MD5.</span><span class="sxs-lookup"><span data-stu-id="ba399-360">The MD5 check is turned off by default, so you must specify this option to perform the MD5 check when downloading data.</span></span>

<span data-ttu-id="ba399-361">Všimněte si, že Azure Storage nezaručuje, že je aktuální hodnota hash MD5 uložené pro objekt blob, nebo soubor.</span><span class="sxs-lookup"><span data-stu-id="ba399-361">Note that Azure Storage doesn't guarantee that the MD5 hash stored for the blob or file is up-to-date.</span></span> <span data-ttu-id="ba399-362">Je zodpovědností klienta aktualizovat MD5 změně objektů blob nebo souboru je.</span><span class="sxs-lookup"><span data-stu-id="ba399-362">It is client's responsibility to update the MD5 whenever the blob or file is modified.</span></span>

<span data-ttu-id="ba399-363">AzCopy vždy nastaví vlastnost obsah MD5 pro objektů blob v Azure nebo soubor nahráním do služby.</span><span class="sxs-lookup"><span data-stu-id="ba399-363">AzCopy always sets the Content-MD5 property for an Azure blob or file after uploading it to the service.</span></span>  

<span data-ttu-id="ba399-364">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-364">**Applicable to:** Blobs, Files</span></span>

### <a name="snapshot"></a><span data-ttu-id="ba399-365">/ Snímku</span><span class="sxs-lookup"><span data-stu-id="ba399-365">/Snapshot</span></span>
<span data-ttu-id="ba399-366">Označuje, zda přenos snímky.</span><span class="sxs-lookup"><span data-stu-id="ba399-366">Indicates whether to transfer snapshots.</span></span> <span data-ttu-id="ba399-367">Tato možnost je platná, pouze pokud je zdroj objektu blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-367">This option is only valid when the source is a blob.</span></span>

<span data-ttu-id="ba399-368">Přejmenování snímky přenášená objektů blob v tomto formátu: .extension název objektu blob (snímku čas)</span><span class="sxs-lookup"><span data-stu-id="ba399-368">The transferred blob snapshots are renamed in this format: blob-name (snapshot-time).extension</span></span>

<span data-ttu-id="ba399-369">Ve výchozím nastavení nebudou zkopírovány snímky.</span><span class="sxs-lookup"><span data-stu-id="ba399-369">By default, snapshots are not copied.</span></span>

<span data-ttu-id="ba399-370">**Platí pro:** objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="ba399-370">**Applicable to:** Blobs</span></span>

### <a name="vverbose-log-file"></a><span data-ttu-id="ba399-371">/ V: [podrobné souboru protokolu]</span><span class="sxs-lookup"><span data-stu-id="ba399-371">/V:[verbose-log-file]</span></span>
<span data-ttu-id="ba399-372">Výstupy podrobné stavové zprávy do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="ba399-372">Outputs verbose status messages into a log file.</span></span>

<span data-ttu-id="ba399-373">Ve výchozím nastavení, je soubor podrobného protokolování s názvem AzCopyVerbose.log v `%LocalAppData%\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="ba399-373">By default, the verbose log file is named AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="ba399-374">Pokud zadáte existující umístění souborů pro tuto možnost, podrobného protokolování se připojuje k souboru.</span><span class="sxs-lookup"><span data-stu-id="ba399-374">If you specify an existing file location for this option, the verbose log is appended to that file.</span></span>  

<span data-ttu-id="ba399-375">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-375">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="zjournal-file-folder"></a><span data-ttu-id="ba399-376">/ Z: [deníku – soubor a složka]</span><span class="sxs-lookup"><span data-stu-id="ba399-376">/Z:[journal-file-folder]</span></span>
<span data-ttu-id="ba399-377">Určuje složku souboru deníku pro operace obnovení.</span><span class="sxs-lookup"><span data-stu-id="ba399-377">Specifies a journal file folder for resuming an operation.</span></span>

<span data-ttu-id="ba399-378">AzCopy vždy podporuje obnovení, pokud operace byla přerušena.</span><span class="sxs-lookup"><span data-stu-id="ba399-378">AzCopy always supports resuming if an operation has been interrupted.</span></span>

<span data-ttu-id="ba399-379">Pokud není tato možnost zadána, nebo je zadán bez cestu ke složce, pak AzCopy vytvoří ve výchozím umístění, což je % LocalAppData%\Microsoft\Azure\AzCopy soubor deníku.</span><span class="sxs-lookup"><span data-stu-id="ba399-379">If this option is not specified, or it is specified without a folder path, then AzCopy creates the journal file in the default location, which is %LocalAppData%\Microsoft\Azure\AzCopy.</span></span>

<span data-ttu-id="ba399-380">Pokaždé, když příkaz azcopy, zkontroluje existenci souboru deníku ve výchozí složce, nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti.</span><span class="sxs-lookup"><span data-stu-id="ba399-380">Each time you issue a command to AzCopy, it checks whether a journal file exists in the default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="ba399-381">Pokud soubor deníku neexistuje ani na jednom místě, AzCopy zpracovává operaci jako nové a generuje nový soubor deníku.</span><span class="sxs-lookup"><span data-stu-id="ba399-381">If the journal file does not exist in either place, AzCopy treats the operation as new and generates a new journal file.</span></span>

<span data-ttu-id="ba399-382">Pokud soubor deníku neexistuje, AzCopy kontroluje, zda příkazového řádku, který můžete vložit odpovídá příkazového řádku v souboru deníku.</span><span class="sxs-lookup"><span data-stu-id="ba399-382">If the journal file does exist, AzCopy checks whether the command line that you input matches the command line in the journal file.</span></span> <span data-ttu-id="ba399-383">Pokud se dva příkazové řádky shodují, obnoví AzCopy neúplné operaci.</span><span class="sxs-lookup"><span data-stu-id="ba399-383">If the two command lines match, AzCopy resumes the incomplete operation.</span></span> <span data-ttu-id="ba399-384">Pokud se neshodují, zobrazí se výzva k buď přepsat soubor deníku spustit novou operaci, nebo zrušit aktuální operace.</span><span class="sxs-lookup"><span data-stu-id="ba399-384">If they do not match, you are prompted to either overwrite the journal file to start a new operation, or to cancel the current operation.</span></span>

<span data-ttu-id="ba399-385">Soubor deníku se odstraní po úspěšném dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="ba399-385">The journal file is deleted upon successful completion of the operation.</span></span>

<span data-ttu-id="ba399-386">Všimněte si, že obnovení ze souboru deníku vytvořeného v předchozí verzi AzCopy operace není podporována.</span><span class="sxs-lookup"><span data-stu-id="ba399-386">Note that resuming an operation from a journal file created by a previous version of AzCopy is not supported.</span></span>

<span data-ttu-id="ba399-387">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-387">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="parameter-file"></a><span data-ttu-id="ba399-388">/@:"Parameter-File"</span><span class="sxs-lookup"><span data-stu-id="ba399-388">/@:"parameter-file"</span></span>
<span data-ttu-id="ba399-389">Určuje soubor, který obsahuje parametry.</span><span class="sxs-lookup"><span data-stu-id="ba399-389">Specifies a file that contains parameters.</span></span> <span data-ttu-id="ba399-390">AzCopy zpracovává parametry v souboru stejně, jako kdyby kdyby byly zadány na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="ba399-390">AzCopy processes the parameters in the file just as if they had been specified on the command line.</span></span>

<span data-ttu-id="ba399-391">V souboru odpovědí můžete zadat několik parametrů na jeden řádek nebo zadejte každý parametr na samostatném řádku.</span><span class="sxs-lookup"><span data-stu-id="ba399-391">In a response file, you can either specify multiple parameters on a single line, or specify each parameter on its own line.</span></span> <span data-ttu-id="ba399-392">Všimněte si, že jednotlivé parametr nemůže zahrnovat více řádků.</span><span class="sxs-lookup"><span data-stu-id="ba399-392">Note that an individual parameter cannot span multiple lines.</span></span>

<span data-ttu-id="ba399-393">Soubory odezvy může zahrnovat komentáře řádky, které začínají symbolem #.</span><span class="sxs-lookup"><span data-stu-id="ba399-393">Response files can include comments lines that begin with the # symbol.</span></span>

<span data-ttu-id="ba399-394">Můžete zadat několik souborů odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba399-394">You can specify multiple response files.</span></span> <span data-ttu-id="ba399-395">Všimněte si však, že AzCopy nepodporuje soubory vnořené odezvy.</span><span class="sxs-lookup"><span data-stu-id="ba399-395">However, note that AzCopy does not support nested response files.</span></span>

<span data-ttu-id="ba399-396">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-396">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="y"></a><span data-ttu-id="ba399-397">/Y</span><span class="sxs-lookup"><span data-stu-id="ba399-397">/Y</span></span>
<span data-ttu-id="ba399-398">Potlačí všechny výzvy potvrzení AzCopy.</span><span class="sxs-lookup"><span data-stu-id="ba399-398">Suppresses all AzCopy confirmation prompts.</span></span>

<span data-ttu-id="ba399-399">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-399">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="l"></a><span data-ttu-id="ba399-400">/ L</span><span class="sxs-lookup"><span data-stu-id="ba399-400">/L</span></span>
<span data-ttu-id="ba399-401">Určuje operaci výpis pouze; žádná data budou zkopírována.</span><span class="sxs-lookup"><span data-stu-id="ba399-401">Specifies a listing operation only; no data is copied.</span></span>

<span data-ttu-id="ba399-402">AzCopy interpretuje použití této možnosti simulace pro spuštění příkazového řádku bez možnost /L a počty kolik objekty se zkopírují, můžete zadat možnost /V ve stejnou dobu a zkontrolujte, jaké objekty jsou kopírovány v podrobného protokolování.</span><span class="sxs-lookup"><span data-stu-id="ba399-402">AzCopy interprets the using of this option as a simulation for running the command line without this option /L and counts how many objects are copied, you can specify option /V at the same time to check which objects are copied in the verbose log.</span></span>

<span data-ttu-id="ba399-403">Chování tato možnost je určena také umístění zdroje dat a přítomnost rekurzivní možnost /S a soubor vzor možnost režim /Pattern.</span><span class="sxs-lookup"><span data-stu-id="ba399-403">The behavior of this option is also determined by the location of the source data and the presence of the recursive mode option /S and file pattern option /Pattern.</span></span>

<span data-ttu-id="ba399-404">AzCopy vyžaduje oprávnění seznamu a přečtěte si toto umístění zdroje, při použití této možnosti.</span><span class="sxs-lookup"><span data-stu-id="ba399-404">AzCopy requires LIST and READ permission of this source location when using this option.</span></span>

<span data-ttu-id="ba399-405">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-405">**Applicable to:** Blobs, Files</span></span>

### <a name="mt"></a><span data-ttu-id="ba399-406">/ MT</span><span class="sxs-lookup"><span data-stu-id="ba399-406">/MT</span></span>
<span data-ttu-id="ba399-407">Nastaví čas poslední změny stažený soubor být stejný jako zdrojový objekt blob nebo souboru.</span><span class="sxs-lookup"><span data-stu-id="ba399-407">Sets the downloaded file's last-modified time to be the same as the source blob or file's.</span></span>

<span data-ttu-id="ba399-408">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-408">**Applicable to:** Blobs, Files</span></span>

### <a name="xn"></a><span data-ttu-id="ba399-409">/XN</span><span class="sxs-lookup"><span data-stu-id="ba399-409">/XN</span></span>
<span data-ttu-id="ba399-410">Vyloučí novější zdrojovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="ba399-410">Excludes a newer source resource.</span></span> <span data-ttu-id="ba399-411">Prostředek není zkopírovat, pokud čas poslední změny zdroje je stejná nebo novější než cílový.</span><span class="sxs-lookup"><span data-stu-id="ba399-411">The resource is not copied if the last modified time of the source is the same or newer than destination.</span></span>

<span data-ttu-id="ba399-412">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-412">**Applicable to:** Blobs, Files</span></span>

### <a name="xo"></a><span data-ttu-id="ba399-413">/XO</span><span class="sxs-lookup"><span data-stu-id="ba399-413">/XO</span></span>
<span data-ttu-id="ba399-414">Vyloučí starší zdrojovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="ba399-414">Excludes an older source resource.</span></span> <span data-ttu-id="ba399-415">Prostředek není zkopírována, pokud čas poslední změny zdroje stejné nebo starší než cílový.</span><span class="sxs-lookup"><span data-stu-id="ba399-415">The resource is not copied if the last modified time of the source is the same or older than destination.</span></span>

<span data-ttu-id="ba399-416">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-416">**Applicable to:** Blobs, Files</span></span>

### <a name="a"></a><span data-ttu-id="ba399-417">/A</span><span class="sxs-lookup"><span data-stu-id="ba399-417">/A</span></span>
<span data-ttu-id="ba399-418">Ukládání pouze soubory, které mají atribut Archivovat nastaven.</span><span class="sxs-lookup"><span data-stu-id="ba399-418">Uploads only files that have the Archive attribute set.</span></span>

<span data-ttu-id="ba399-419">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-419">**Applicable to:** Blobs, Files</span></span>

### <a name="iarashcnetoi"></a><span data-ttu-id="ba399-420">/ IA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="ba399-420">/IA:[RASHCNETOI]</span></span>
<span data-ttu-id="ba399-421">Ukládání pouze soubory, které mají všechny zadané atributy sady.</span><span class="sxs-lookup"><span data-stu-id="ba399-421">Uploads only files that have any of the specified attributes set.</span></span>

<span data-ttu-id="ba399-422">Dostupné atributy patří:</span><span class="sxs-lookup"><span data-stu-id="ba399-422">Available attributes include:</span></span>

* <span data-ttu-id="ba399-423">R = soubory jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="ba399-423">R = Read-only files</span></span>
* <span data-ttu-id="ba399-424">A = soubory připravené k archivaci</span><span class="sxs-lookup"><span data-stu-id="ba399-424">A = Files ready for archiving</span></span>
* <span data-ttu-id="ba399-425">S = systémové soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-425">S = System files</span></span>
* <span data-ttu-id="ba399-426">H = skryté soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-426">H = Hidden files</span></span>
* <span data-ttu-id="ba399-427">C = komprimované soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-427">C = Compressed files</span></span>
* <span data-ttu-id="ba399-428">N = normální soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-428">N = Normal files</span></span>
* <span data-ttu-id="ba399-429">E = šifrovaných souborů</span><span class="sxs-lookup"><span data-stu-id="ba399-429">E = Encrypted files</span></span>
* <span data-ttu-id="ba399-430">T = dočasné soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-430">T = Temporary files</span></span>
* <span data-ttu-id="ba399-431">O = Offline soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-431">O = Offline files</span></span>
* <span data-ttu-id="ba399-432">I = není indexované soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-432">I = Non-indexed files</span></span>

<span data-ttu-id="ba399-433">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-433">**Applicable to:** Blobs, Files</span></span>

### <a name="xarashcnetoi"></a><span data-ttu-id="ba399-434">/ XA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="ba399-434">/XA:[RASHCNETOI]</span></span>
<span data-ttu-id="ba399-435">Vyloučí soubory, které mají všechny zadané atributy sady.</span><span class="sxs-lookup"><span data-stu-id="ba399-435">Excludes files that have any of the specified attributes set.</span></span>

<span data-ttu-id="ba399-436">Dostupné atributy patří:</span><span class="sxs-lookup"><span data-stu-id="ba399-436">Available attributes include:</span></span>

* <span data-ttu-id="ba399-437">R = soubory jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="ba399-437">R = Read-only files</span></span>
* <span data-ttu-id="ba399-438">A = soubory připravené k archivaci</span><span class="sxs-lookup"><span data-stu-id="ba399-438">A = Files ready for archiving</span></span>
* <span data-ttu-id="ba399-439">S = systémové soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-439">S = System files</span></span>
* <span data-ttu-id="ba399-440">H = skryté soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-440">H = Hidden files</span></span>
* <span data-ttu-id="ba399-441">C = komprimované soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-441">C = Compressed files</span></span>
* <span data-ttu-id="ba399-442">N = normální soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-442">N = Normal files</span></span>
* <span data-ttu-id="ba399-443">E = šifrovaných souborů</span><span class="sxs-lookup"><span data-stu-id="ba399-443">E = Encrypted files</span></span>
* <span data-ttu-id="ba399-444">T = dočasné soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-444">T = Temporary files</span></span>
* <span data-ttu-id="ba399-445">O = Offline soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-445">O = Offline files</span></span>
* <span data-ttu-id="ba399-446">I = není indexované soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-446">I = Non-indexed files</span></span>

<span data-ttu-id="ba399-447">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-447">**Applicable to:** Blobs, Files</span></span>

### <a name="delimiterdelimiter"></a><span data-ttu-id="ba399-448">/ Oddělovač: "oddělovač.</span><span class="sxs-lookup"><span data-stu-id="ba399-448">/Delimiter:"delimiter"</span></span>
<span data-ttu-id="ba399-449">Označuje znak oddělovač použitý pro vymezení virtuálních adresářů v název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-449">Indicates the delimiter character used to delimit virtual directories in a blob name.</span></span>

<span data-ttu-id="ba399-450">Ve výchozím nastavení používá AzCopy / jako oddělovací znak.</span><span class="sxs-lookup"><span data-stu-id="ba399-450">By default, AzCopy uses / as the delimiter character.</span></span> <span data-ttu-id="ba399-451">Ale AzCopy podporuje používání libovolného znaku běžné (například @, #, nebo %) jako oddělovač.</span><span class="sxs-lookup"><span data-stu-id="ba399-451">However, AzCopy supports using any common character (such as @, #, or %) as a delimiter.</span></span> <span data-ttu-id="ba399-452">Pokud potřebujete použít jeden z těchto speciálních znaků v příkazovém řádku, uzavřete název souboru s dvojité uvozovky.</span><span class="sxs-lookup"><span data-stu-id="ba399-452">If you need to include one of these special characters on the command line, enclose the file name with double quotes.</span></span>

<span data-ttu-id="ba399-453">Tato možnost platí pouze pro stahování objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="ba399-453">This option is only applicable for downloading blobs.</span></span>

<span data-ttu-id="ba399-454">**Platí pro:** objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="ba399-454">**Applicable to:** Blobs</span></span>

### <a name="ncnumber-of-concurrent-operations"></a><span data-ttu-id="ba399-455">/ NC: "číslo z souběžných operace"</span><span class="sxs-lookup"><span data-stu-id="ba399-455">/NC:"number-of-concurrent-operations"</span></span>
<span data-ttu-id="ba399-456">Určuje počet souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="ba399-456">Specifies the number of concurrent operations.</span></span>

<span data-ttu-id="ba399-457">AzCopy ve výchozím nastavení spustí počet souběžných operací, pokud chcete zvýšit propustnost dat přenosu.</span><span class="sxs-lookup"><span data-stu-id="ba399-457">AzCopy by default starts a certain number of concurrent operations to increase the data transfer throughput.</span></span> <span data-ttu-id="ba399-458">Všimněte si, že velký počet souběžných operací v prostředí s malou šířkou pásma může zahlcovat síťové připojení a zabránit v plně dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="ba399-458">Note that large number of concurrent operations in a low-bandwidth environment may overwhelm the network connection and prevent the operations from fully completing.</span></span> <span data-ttu-id="ba399-459">Omezení souběžných operací podle skutečné dostupnou šířku pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="ba399-459">Throttle concurrent operations based on actual available network bandwidth.</span></span>

<span data-ttu-id="ba399-460">Horní limit pro souběžných operací je 512.</span><span class="sxs-lookup"><span data-stu-id="ba399-460">The upper limit for concurrent operations is 512.</span></span>

<span data-ttu-id="ba399-461">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-461">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcetypeblob--table"></a><span data-ttu-id="ba399-462">/ SourceType: "Blob" | "Tabulka"</span><span class="sxs-lookup"><span data-stu-id="ba399-462">/SourceType:"Blob" | "Table"</span></span>
<span data-ttu-id="ba399-463">Určuje, že `source` prostředek je k dispozici v místním vývojovém prostředí, spouštění v emulátoru úložiště objektu blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-463">Specifies that the `source` resource is a blob available in the local development environment, running in the storage emulator.</span></span>

<span data-ttu-id="ba399-464">**Platí pro:** objekty BLOB, tabulek</span><span class="sxs-lookup"><span data-stu-id="ba399-464">**Applicable to:** Blobs, Tables</span></span>

### <a name="desttypeblob--table"></a><span data-ttu-id="ba399-465">/ DestType: "Blob" | "Tabulka"</span><span class="sxs-lookup"><span data-stu-id="ba399-465">/DestType:"Blob" | "Table"</span></span>
<span data-ttu-id="ba399-466">Určuje, že `destination` prostředek je k dispozici v místním vývojovém prostředí, spouštění v emulátoru úložiště objektu blob.</span><span class="sxs-lookup"><span data-stu-id="ba399-466">Specifies that the `destination` resource is a blob available in the local development environment, running in the storage emulator.</span></span>

<span data-ttu-id="ba399-467">**Platí pro:** objekty BLOB, tabulek</span><span class="sxs-lookup"><span data-stu-id="ba399-467">**Applicable to:** Blobs, Tables</span></span>

### <a name="pkrskey1key2key3"></a><span data-ttu-id="ba399-468">/ PKRS: "key&#1;key&#2; klíč&#3;..."</span><span class="sxs-lookup"><span data-stu-id="ba399-468">/PKRS:"key1#key2#key3#..."</span></span>
<span data-ttu-id="ba399-469">Rozdělí rozsah klíče oddílu Povolit export dat v tabulce současně, což zvyšuje rychlost operace exportu.</span><span class="sxs-lookup"><span data-stu-id="ba399-469">Splits the partition key range to enable exporting table data in parallel, which increases the speed of the export operation.</span></span>

<span data-ttu-id="ba399-470">Pokud není tato možnost zadána, AzCopy používá jedno vlákno pro export entity tabulky.</span><span class="sxs-lookup"><span data-stu-id="ba399-470">If this option is not specified, then AzCopy uses a single thread to export table entities.</span></span> <span data-ttu-id="ba399-471">Například pokud uživatel zadá /PKRS: "aa #bb", pak AzCopy spustí tři souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="ba399-471">For example, if the user specifies /PKRS:"aa#bb", then AzCopy starts three concurrent operations.</span></span>

<span data-ttu-id="ba399-472">Každé operace exportuje mezi tři rozsahy klíčů oddílu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="ba399-472">Each operation exports one of three partition key ranges, as shown below:</span></span>

  <span data-ttu-id="ba399-473">[klíč prvního oddílu, aa)</span><span class="sxs-lookup"><span data-stu-id="ba399-473">[first-partition-key, aa)</span></span>

  <span data-ttu-id="ba399-474">[aa, bb)</span><span class="sxs-lookup"><span data-stu-id="ba399-474">[aa, bb)</span></span>

  <span data-ttu-id="ba399-475">[bb, klíč oddílu poslední]</span><span class="sxs-lookup"><span data-stu-id="ba399-475">[bb, last-partition-key]</span></span>

<span data-ttu-id="ba399-476">**Platí pro:** tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-476">**Applicable to:** Tables</span></span>

### <a name="splitsizefile-size"></a><span data-ttu-id="ba399-477">/ SplitSize: "velikost souboru"</span><span class="sxs-lookup"><span data-stu-id="ba399-477">/SplitSize:"file-size"</span></span>
<span data-ttu-id="ba399-478">Určuje, v exportovaném souboru rozdělení velikost v MB, je minimální povolená hodnota je 32.</span><span class="sxs-lookup"><span data-stu-id="ba399-478">Specifies the exported file split size in MB, the minimal value allowed is 32.</span></span>

<span data-ttu-id="ba399-479">Pokud není tato možnost zadána, AzCopy exportuje data tabulky do jednoho souboru.</span><span class="sxs-lookup"><span data-stu-id="ba399-479">If this option is not specified, AzCopy exports table data to a single file.</span></span>

<span data-ttu-id="ba399-480">Pokud data tabulky se exportují do objektu blob a velikost exportovaný soubor dosáhne 200 GB limit pro velikost objektu blob, pak AzCopy rozdělí v exportovaném souboru, i když není tato možnost zadána.</span><span class="sxs-lookup"><span data-stu-id="ba399-480">If the table data is exported to a blob, and the exported file size reaches the 200 GB limit for blob size, then AzCopy splits the exported file, even if this option is not specified.</span></span>

<span data-ttu-id="ba399-481">**Platí pro:** tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-481">**Applicable to:** Tables</span></span>

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a><span data-ttu-id="ba399-482">/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"</span><span class="sxs-lookup"><span data-stu-id="ba399-482">/EntityOperation:"InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"</span></span>
<span data-ttu-id="ba399-483">Určuje chování importu dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="ba399-483">Specifies the table data import behavior.</span></span>

* <span data-ttu-id="ba399-484">InsertOrSkip - přeskočí stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce.</span><span class="sxs-lookup"><span data-stu-id="ba399-484">InsertOrSkip - Skips an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="ba399-485">InsertOrMerge - sloučí existující entitu nebo vloží novou entitu, pokud neexistuje v tabulce.</span><span class="sxs-lookup"><span data-stu-id="ba399-485">InsertOrMerge - Merges an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="ba399-486">InsertOrReplace - nahrazuje stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce.</span><span class="sxs-lookup"><span data-stu-id="ba399-486">InsertOrReplace - Replaces an existing entity or inserts a new entity if it does not exist in the table.</span></span>

<span data-ttu-id="ba399-487">**Platí pro:** tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-487">**Applicable to:** Tables</span></span>

### <a name="manifestmanifest-file"></a><span data-ttu-id="ba399-488">/ Manifest: "manifest – soubor"</span><span class="sxs-lookup"><span data-stu-id="ba399-488">/Manifest:"manifest-file"</span></span>
<span data-ttu-id="ba399-489">Určuje soubor manifestu pro tabulky exportovat a importovat operaci.</span><span class="sxs-lookup"><span data-stu-id="ba399-489">Specifies the manifest file for the table export and import operation.</span></span>

<span data-ttu-id="ba399-490">Tato možnost je volitelné během exportu, AzCopy vygeneruje soubor manifestu pomocí předdefinovaných název, pokud není tato možnost zadána.</span><span class="sxs-lookup"><span data-stu-id="ba399-490">This option is optional during the export operation, AzCopy generates a manifest file with predefined name if this option is not specified.</span></span>

<span data-ttu-id="ba399-491">Tato možnost je povinná během operace importu pro vyhledání datových souborů.</span><span class="sxs-lookup"><span data-stu-id="ba399-491">This option is required during the import operation for locating the data files.</span></span>

<span data-ttu-id="ba399-492">**Platí pro:** tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-492">**Applicable to:** Tables</span></span>

### <a name="synccopy"></a><span data-ttu-id="ba399-493">/ SyncCopy</span><span class="sxs-lookup"><span data-stu-id="ba399-493">/SyncCopy</span></span>
<span data-ttu-id="ba399-494">Označuje, zda synchronně kopírování objektů BLOB nebo soubory mezi dva koncové body Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="ba399-494">Indicates whether to synchronously copy blobs or files between two Azure Storage endpoints.</span></span>

<span data-ttu-id="ba399-495">AzCopy ve výchozím nastavení používá asynchronní kopie straně serveru.</span><span class="sxs-lookup"><span data-stu-id="ba399-495">AzCopy by default uses server-side asynchronous copy.</span></span> <span data-ttu-id="ba399-496">Zadejte tuto možnost, chcete-li provést synchronní kopie, která stáhne objektů BLOB nebo soubory k místní paměti a odesílá je do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ba399-496">Specify this option to perform a synchronous copy, which downloads blobs or files to local memory and then uploads them to Azure Storage.</span></span>

<span data-ttu-id="ba399-497">Tuto možnost můžete použít při kopírování souborů v rámci úložiště objektů Blob, úložiště File, nebo z úložiště objektů Blob k úložišti souborů nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="ba399-497">You can use this option when copying files within Blob storage, within File storage, or from Blob storage to File storage or vice versa.</span></span>

<span data-ttu-id="ba399-498">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-498">**Applicable to:** Blobs, Files</span></span>

### <a name="setcontenttypecontent-type"></a><span data-ttu-id="ba399-499">/ SetContentType: "content-type"</span><span class="sxs-lookup"><span data-stu-id="ba399-499">/SetContentType:"content-type"</span></span>
<span data-ttu-id="ba399-500">Určuje typ MIME pro cílové objekty BLOB nebo soubory obsahu.</span><span class="sxs-lookup"><span data-stu-id="ba399-500">Specifies the MIME content type for destination blobs or files.</span></span>

<span data-ttu-id="ba399-501">AzCopy nastaví typ obsahu pro objekt blob nebo soubor na application/octet-stream ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ba399-501">AzCopy sets the content type for a blob or file to application/octet-stream by default.</span></span> <span data-ttu-id="ba399-502">Typ obsahu pro všechny objekty BLOB nebo soubory můžete nastavit a explicitně zadat hodnotu pro tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="ba399-502">You can set the content type for all blobs or files by explicitly specifying a value for this option.</span></span>

<span data-ttu-id="ba399-503">Pokud zadáte tuto možnost bez hodnoty, AzCopy nastaví jednotlivých objektů blob nebo typ obsahu souboru podle jeho přípony.</span><span class="sxs-lookup"><span data-stu-id="ba399-503">If you specify this option without a value, then AzCopy sets each blob or file's content type according to its file extension.</span></span>

<span data-ttu-id="ba399-504">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="ba399-504">**Applicable to:** Blobs, Files</span></span>

### <a name="payloadformatjson--csv"></a><span data-ttu-id="ba399-505">/ PayloadFormat: "JSON" | "CSV"</span><span class="sxs-lookup"><span data-stu-id="ba399-505">/PayloadFormat:"JSON" | "CSV"</span></span>
<span data-ttu-id="ba399-506">Určuje formát souboru exportovaná data tabulky.</span><span class="sxs-lookup"><span data-stu-id="ba399-506">Specifies the format of the table exported data file.</span></span>

<span data-ttu-id="ba399-507">Pokud není tato možnost zadána, exportuje AzCopy ve výchozím nastavení tabulky datový soubor ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ba399-507">If this option is not specified, by default AzCopy exports table data file in JSON format.</span></span>

<span data-ttu-id="ba399-508">**Platí pro:** tabulky</span><span class="sxs-lookup"><span data-stu-id="ba399-508">**Applicable to:** Tables</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="ba399-509">Známé problémy a doporučené postupy</span><span class="sxs-lookup"><span data-stu-id="ba399-509">Known Issues and Best Practices</span></span>
### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="ba399-510">Limit souběžných zápisy při kopírování dat</span><span class="sxs-lookup"><span data-stu-id="ba399-510">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="ba399-511">Při kopírování objektů BLOB nebo soubory s AzCopy, mějte na paměti, že jiná aplikace může být úprava dat během kopírování ho.</span><span class="sxs-lookup"><span data-stu-id="ba399-511">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying the data while you are copying it.</span></span> <span data-ttu-id="ba399-512">Pokud je to možné Ujistěte se, které chcete kopírovat data nemění při kopírování.</span><span class="sxs-lookup"><span data-stu-id="ba399-512">If possible, ensure that the data you are copying is not being modified during the copy operation.</span></span> <span data-ttu-id="ba399-513">Například při kopírování virtuálního pevného disku přidružený virtuální počítač Azure, ujistěte se, že žádné další aplikace jsou aktuálně zápis na virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="ba399-513">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing to the VHD.</span></span> <span data-ttu-id="ba399-514">Dobrým způsobem, jak to udělat, je leasing prostředků, které se mají zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="ba399-514">A good way to do this is by leasing the resource to be copied.</span></span> <span data-ttu-id="ba399-515">Alternativně můžete nejprve vytvořte snímek virtuálního pevného disku a poté zkopírujte snímku.</span><span class="sxs-lookup"><span data-stu-id="ba399-515">Alternately, you can create a snapshot of the VHD first and then copy the snapshot.</span></span>

<span data-ttu-id="ba399-516">Pokud nelze zabránit jiné aplikace z zápis do objektů BLOB nebo soubory, když se kopírují, pak mějte na paměti, že v době dokončení úlohy, kopírované prostředky buď již nemá úplné parita s prostředky zdroje.</span><span class="sxs-lookup"><span data-stu-id="ba399-516">If you cannot prevent other applications from writing to blobs or files while they are being copied, then keep in mind that by the time the job finishes, the copied resources may no longer have full parity with the source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="ba399-517">Jedna instance nástroje AzCopy spusťte na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="ba399-517">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="ba399-518">AzCopy je navržen chcete maximalizovat využití prostředků vašeho počítače urychlit přenos dat, doporučujeme spustit pouze jedna instance nástroje AzCopy na jednom počítači a zadejte možnost `/NC` Pokud potřebujete více souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="ba399-518">AzCopy is designed to maximize the utilization of your machine resource to accelerate the data transfer, we recommend you run only one AzCopy instance on one machine, and specify the option `/NC` if you need more concurrent operations.</span></span> <span data-ttu-id="ba399-519">Další podrobnosti, zadejte `AzCopy /?:NC` na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="ba399-519">For more details, type `AzCopy /?:NC` at the command line.</span></span>

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a><span data-ttu-id="ba399-520">Povolit algoritmus MD5 kompatibilní s FIPS pro AzCopy když jste "použití standardu FIPS kompatibilní s algoritmy pro šifrování, hašování a podpisování".</span><span class="sxs-lookup"><span data-stu-id="ba399-520">Enable FIPS compliant MD5 algorithms for AzCopy when you "Use FIPS compliant algorithms for encryption, hashing and signing".</span></span>
<span data-ttu-id="ba399-521">AzCopy ve výchozím nastavení používá k výpočtu MD5 při kopírování objektů implementace rozhraní .NET MD5, ale existují některé požadavky na zabezpečení, které je třeba povolit nastavení MD5 kompatibilní s FIPS AzCopy.</span><span class="sxs-lookup"><span data-stu-id="ba399-521">AzCopy by default uses .NET MD5 implementation to calculate the MD5 when copying objects, but there are some security requirements that need AzCopy to enable FIPS compliant MD5 setting.</span></span>

<span data-ttu-id="ba399-522">Můžete vytvořit soubor app.config `AzCopy.exe.config` s vlastností `AzureStorageUseV1MD5` a umístí jej vyhraďte s AzCopy.exe.</span><span class="sxs-lookup"><span data-stu-id="ba399-522">You can create an app.config file `AzCopy.exe.config` with property `AzureStorageUseV1MD5` and put it aside with AzCopy.exe.</span></span>

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

<span data-ttu-id="ba399-523">Pro vlastnost "AzureStorageUseV1MD5" • True - používá výchozí hodnota, AzCopy implementace rozhraní .NET MD5.</span><span class="sxs-lookup"><span data-stu-id="ba399-523">For property "AzureStorageUseV1MD5" • True - The default value, AzCopy uses .NET MD5 implementation.</span></span>
<span data-ttu-id="ba399-524">• False – AzCopy používá algoritmus MD5 kompatibilní se standardem FIPS.</span><span class="sxs-lookup"><span data-stu-id="ba399-524">• False – AzCopy uses FIPS compliant MD5 algorithm.</span></span>

<span data-ttu-id="ba399-525">Všimněte si, že na počítač se systémem Windows ve výchozím nastavení je zakázáno algoritmy splňující standard FIPS, můžete v okně Spustit zadejte secpol.msc a zkontrolujte tento přepínač v nastavení zabezpečení -> Místní zásady -> zabezpečení -> – Možnosti kryptografie systému: použití vyhovující standardu FIPS pro šifrování, hašování a podpisování.</span><span class="sxs-lookup"><span data-stu-id="ba399-525">Note that FIPS compliant algorithms is disabled by default on your Windows machine, you can type secpol.msc in your Run window and check this switch at Security Setting->Local Policy->Security Options->System cryptography: Use FIPS compliant algorithms for encryption, hashing and signing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba399-526">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba399-526">Next steps</span></span>
<span data-ttu-id="ba399-527">Další informace o Azure Storage a AzCopy najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="ba399-527">For more information about Azure Storage and AzCopy, see the following resources:</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="ba399-528">Dokumentace k Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="ba399-528">Azure Storage documentation:</span></span>
* [<span data-ttu-id="ba399-529">Úvod do Azure Storage</span><span class="sxs-lookup"><span data-stu-id="ba399-529">Introduction to Azure Storage</span></span>](../storage-introduction.md)
* [<span data-ttu-id="ba399-530">Používání úložiště Blob z rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ba399-530">How to use Blob storage from .NET</span></span>](../blobs/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="ba399-531">Jak používat úložiště File z rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ba399-531">How to use File storage from .NET</span></span>](../storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="ba399-532">Používání úložiště Table z rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ba399-532">How to use Table storage from .NET</span></span>](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [<span data-ttu-id="ba399-533">Jak vytvořit, spravovat nebo odstranit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="ba399-533">How to create, manage, or delete a storage account</span></span>](../storage-create-storage-account.md)
* [<span data-ttu-id="ba399-534">Přenos dat pomocí nástroje AzCopy v systému Linux</span><span class="sxs-lookup"><span data-stu-id="ba399-534">Transfer data with AzCopy on Linux</span></span>](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="ba399-535">Příspěvky blogu Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="ba399-535">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="ba399-536">Představení náhled knihovny přesun dat úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="ba399-536">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="ba399-537">AzCopy: Úvod synchronní kopie a vlastní typ obsahu</span><span class="sxs-lookup"><span data-stu-id="ba399-537">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="ba399-538">AzCopy: Uvedení obecné dostupnosti AzCopy 3.0 a AzCopy 4.0 verze preview s podporou tabulky a souboru</span><span class="sxs-lookup"><span data-stu-id="ba399-538">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="ba399-539">AzCopy: Optimalizovaných pro rozsáhlé kopie scénáře</span><span class="sxs-lookup"><span data-stu-id="ba399-539">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="ba399-540">AzCopy: Podpora pro geograficky redundantní úložiště s přístupem pro čtení</span><span class="sxs-lookup"><span data-stu-id="ba399-540">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="ba399-541">AzCopy: Přenos dat pomocí SAS token a znovu spustit režim</span><span class="sxs-lookup"><span data-stu-id="ba399-541">AzCopy: Transfer data with re-startable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="ba399-542">AzCopy: Objekt Blob kopírování mezi účet pomocí</span><span class="sxs-lookup"><span data-stu-id="ba399-542">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="ba399-543">AzCopy: Nahrávání nebo stahování souborů pro objekty BLOB Azure</span><span class="sxs-lookup"><span data-stu-id="ba399-543">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

