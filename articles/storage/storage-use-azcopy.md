---
title: "aaaCopy nebo přesunout data tooAzure úložiště s AzCopy v systému Windows | Microsoft Docs"
description: "Použijte hello AzCopy v systému Windows nástroj toomove nebo kopírování dat tooor z objektu blob, table a obsah souboru. Kopírování dat tooAzure úložiště z místních souborů, nebo zkopírujte data v rámci nebo mezi účty úložiště. Vaše data tooAzure úložiště snadno migrujte."
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
ms.openlocfilehash: a77db84c3a3e06f0ad4e87d02b14a5c62ed8d9ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-hello-azcopy-on-windows"></a><span data-ttu-id="9aafb-105">Přenos dat pomocí hello AzCopy v systému Windows</span><span class="sxs-lookup"><span data-stu-id="9aafb-105">Transfer data with hello AzCopy on Windows</span></span>
<span data-ttu-id="9aafb-106">AzCopy je nástroj příkazového řádku pro kopírování tooand dat z úložiště objektů Blob v Microsoft Azure, souboru a tabulky pomocí jednoduchých příkazů optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="9aafb-106">AzCopy is a command-line utility designed for copying data tooand from Microsoft Azure Blob, File, and Table storage using simple commands with optimal performance.</span></span> <span data-ttu-id="9aafb-107">Data můžete zkopírovat z jednoho objektu tooanother v rámci účtu úložiště nebo mezi účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="9aafb-107">You can copy data from one object tooanother within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="9aafb-108">Existují dvě verze nástroje AzCopy, které si můžete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="9aafb-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="9aafb-109">AzCopy v systému Windows je obsažena v rozhraní .NET Framework a nabízí možnosti příkazového řádku Windows styl.</span><span class="sxs-lookup"><span data-stu-id="9aafb-109">AzCopy on Windows is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="9aafb-110">[AzCopy v systému Linux](storage-use-azcopy-linux.md) sestavena pomocí rozhraní .NET Framework Core, které cílí platformy Linux nabídky stylu POSIX možnosti příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-110">[AzCopy on Linux](storage-use-azcopy-linux.md) is built with .NET Core Framework which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="9aafb-111">Tento článek se zabývá AzCopy v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9aafb-111">This article covers AzCopy on Windows.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="9aafb-112">Stáhněte a nainstalujte AzCopy</span><span class="sxs-lookup"><span data-stu-id="9aafb-112">Download and install AzCopy</span></span>
### <a name="azcopy-on-windows"></a><span data-ttu-id="9aafb-113">AzCopy ve Windows</span><span class="sxs-lookup"><span data-stu-id="9aafb-113">AzCopy on Windows</span></span>
<span data-ttu-id="9aafb-114">Stáhnout hello [nejnovější verzi AzCopy v systému Windows](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="9aafb-114">Download hello [latest version of AzCopy on Windows](http://aka.ms/downloadazcopy).</span></span>

#### <a name="installation-on-windows"></a><span data-ttu-id="9aafb-115">Instalace v systému Windows</span><span class="sxs-lookup"><span data-stu-id="9aafb-115">Installation on Windows</span></span>
<span data-ttu-id="9aafb-116">Po instalaci nástroje AzCopy v systému Windows pomocí instalačního programu hello, otevřete okno příkazového řádku a přejděte toohello instalační adresář AzCopy ve vašem počítači – kde hello `AzCopy.exe` spustitelný soubor se nachází.</span><span class="sxs-lookup"><span data-stu-id="9aafb-116">After installing AzCopy on Windows using hello installer, open a command window and navigate toohello AzCopy installation directory on your computer - where hello `AzCopy.exe` executable is located.</span></span> <span data-ttu-id="9aafb-117">V případě potřeby můžete přidat hello AzCopy tooyour systému cesta k umístění instalace.</span><span class="sxs-lookup"><span data-stu-id="9aafb-117">If desired, you can add hello AzCopy installation location tooyour system path.</span></span> <span data-ttu-id="9aafb-118">Ve výchozím nastavení, AzCopy je nainstalován příliš`%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` nebo `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-118">By default, AzCopy is installed too`%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` or `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span></span>

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="9aafb-119">Zápis vaše první příkaz AzCopy</span><span class="sxs-lookup"><span data-stu-id="9aafb-119">Writing your first AzCopy command</span></span>
<span data-ttu-id="9aafb-120">Hello základní syntaxe pro příkazy AzCopy je:</span><span class="sxs-lookup"><span data-stu-id="9aafb-120">hello basic syntax for AzCopy commands is:</span></span>

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

<span data-ttu-id="9aafb-121">Hello následující příklady ukazují celou řadu scénářů kopírování data tooand z tabulek, souborů a objektů BLOB služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9aafb-121">hello following examples demonstrate a variety of scenarios for copying data tooand from Microsoft Azure Blobs, Files, and Tables.</span></span> <span data-ttu-id="9aafb-122">Odkazovat toohello [AzCopy parametry](#azcopy-parameters) části Podrobné vysvětlení hello parametrů použitých v každém vzorku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-122">Refer toohello [AzCopy Parameters](#azcopy-parameters) section for a detailed explanation of hello parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="9aafb-123">Objekt BLOB: stažení</span><span class="sxs-lookup"><span data-stu-id="9aafb-123">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="9aafb-124">Stáhnout jediného objektu blob</span><span class="sxs-lookup"><span data-stu-id="9aafb-124">Download single blob</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="9aafb-125">Všimněte si, že pokud hello složky `C:\myfolder` neexistuje, bude AzCopy jej vytvořte a stáhnout `abc.txt ` do nové složky hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-125">Note that if hello folder `C:\myfolder` does not exist, AzCopy will create it and download `abc.txt ` into hello new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="9aafb-126">Stáhnout jediného objektu blob ze sekundární oblasti</span><span class="sxs-lookup"><span data-stu-id="9aafb-126">Download single blob from secondary region</span></span>

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="9aafb-127">Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.</span><span class="sxs-lookup"><span data-stu-id="9aafb-127">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="9aafb-128">Stažení všech objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="9aafb-128">Download all blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="9aafb-129">Předpokládejme hello následující objekty BLOB se nacházejí v zadaném kontejneru hello:</span><span class="sxs-lookup"><span data-stu-id="9aafb-129">Assume hello following blobs reside in hello specified container:</span></span>  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="9aafb-130">Po stažení operaci hello hello directory `C:\myfolder` bude obsahovat hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="9aafb-130">After hello download operation, hello directory `C:\myfolder` will include hello following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

<span data-ttu-id="9aafb-131">Pokud nezadáte možnost `/S`, budou staženy žádné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="9aafb-131">If you do not specify option `/S`, no blobs will be downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="9aafb-132">Stáhnout objekty BLOB se zadanou předponou.</span><span class="sxs-lookup"><span data-stu-id="9aafb-132">Download blobs with specified prefix</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

<span data-ttu-id="9aafb-133">Předpokládejme hello následující objekty BLOB se nacházejí v zadaném kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-133">Assume hello following blobs reside in hello specified container.</span></span> <span data-ttu-id="9aafb-134">Všechny objekty BLOB začínající předponou hello `a` budou staženy:</span><span class="sxs-lookup"><span data-stu-id="9aafb-134">All blobs beginning with hello prefix `a` will be downloaded:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="9aafb-135">Po stažení operaci hello hello složky `C:\myfolder` bude obsahovat hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="9aafb-135">After hello download operation, hello folder `C:\myfolder` will include hello following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

<span data-ttu-id="9aafb-136">Předpona Hello platí toohello virtuální adresář, který tvoří první část hello hello název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9aafb-136">hello prefix applies toohello virtual directory, which forms hello first part of hello blob name.</span></span> <span data-ttu-id="9aafb-137">Ve výše uvedeném příkladu hello virtuální adresář hello neodpovídá zadané předpony hello, takže nebude stažen.</span><span class="sxs-lookup"><span data-stu-id="9aafb-137">In hello example shown above, hello virtual directory does not match hello specified prefix, so it is not downloaded.</span></span> <span data-ttu-id="9aafb-138">Kromě toho pokud hello možnost `\S` není zadán, AzCopy nebude stáhnout všechny objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="9aafb-138">In addition, if hello option `\S` is not specified, AzCopy will not download any blobs.</span></span>

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a><span data-ttu-id="9aafb-139">Nastavit čas poslední úpravy hello z exportované soubory toobe stejná hodnota jako hello zdroj objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="9aafb-139">Set hello last-modified time of exported files toobe same as hello source blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

<span data-ttu-id="9aafb-140">Můžete také vyloučit objekty BLOB z operace nástroje download hello podle jejich čas poslední změny.</span><span class="sxs-lookup"><span data-stu-id="9aafb-140">You can also exclude blobs from hello download operation based on their last-modified time.</span></span> <span data-ttu-id="9aafb-141">Příklad: Pokud chcete tooexclude objekty BLOB, jejichž čas poslední změny je hello stejnou nebo novější než hello cílový soubor, přidejte hello `/XN` možnost:</span><span class="sxs-lookup"><span data-stu-id="9aafb-141">For example, if you want tooexclude blobs whose last modified time is hello same or newer than hello destination file, add hello `/XN` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

<span data-ttu-id="9aafb-142">Nebo pokud chcete tooexclude objekty BLOB, jejichž čas poslední změny je hello stejné nebo starší než hello cílový soubor, přidejte hello `/XO` možnost:</span><span class="sxs-lookup"><span data-stu-id="9aafb-142">Or if you want tooexclude blobs whose last modified time is hello same or older than hello destination file, add hello `/XO` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a><span data-ttu-id="9aafb-143">Objekt BLOB: nahrát</span><span class="sxs-lookup"><span data-stu-id="9aafb-143">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="9aafb-144">Odeslat jedním souborem</span><span class="sxs-lookup"><span data-stu-id="9aafb-144">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="9aafb-145">Pokud hello zadaný cílový kontejner neexistuje, bude AzCopy jej vytvořte a nahrajte soubor hello do ní.</span><span class="sxs-lookup"><span data-stu-id="9aafb-145">If hello specified destination container does not exist, AzCopy will create it and upload hello file into it.</span></span>

### <a name="upload-single-file-toovirtual-directory"></a><span data-ttu-id="9aafb-146">Nahrát toovirtual directory jedním souborem</span><span class="sxs-lookup"><span data-stu-id="9aafb-146">Upload single file toovirtual directory</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="9aafb-147">Pokud hello zadaný virtuální adresář neexistuje, bude AzCopy nahrávat hello tooinclude hello virtuální adresář souboru ve svém názvu (*například*, `vd/abc.txt` v předchozím příkladu hello).</span><span class="sxs-lookup"><span data-stu-id="9aafb-147">If hello specified virtual directory does not exist, AzCopy will upload hello file tooinclude hello virtual directory in its name (*e.g.*, `vd/abc.txt` in hello example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="9aafb-148">Odeslat všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="9aafb-148">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

<span data-ttu-id="9aafb-149">Zadání `/S` nahrávání hello obsah hello zadaný adresář tooBlob úložiště rekurzivně, což znamená, že všechny podsložky a jejich soubory, nebude možné odesílat i.</span><span class="sxs-lookup"><span data-stu-id="9aafb-149">Specifying option `/S` uploads hello contents of hello specified directory tooBlob storage recursively, meaning that all subfolders and their files will be uploaded as well.</span></span> <span data-ttu-id="9aafb-150">Například předpokládejme hello následující soubory jsou umístěny ve složce `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="9aafb-150">For instance, assume hello following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="9aafb-151">Po operaci odeslání hello hello kontejner bude zahrnovat hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="9aafb-151">After hello upload operation, hello container will include hello following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="9aafb-152">Pokud nezadáte možnost `/S`, AzCopy nebude odesílat rekurzivně.</span><span class="sxs-lookup"><span data-stu-id="9aafb-152">If you do not specify option `/S`, AzCopy will not upload recursively.</span></span> <span data-ttu-id="9aafb-153">Po operaci odeslání hello hello kontejner bude zahrnovat hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="9aafb-153">After hello upload operation, hello container will include hello following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="9aafb-154">Nahrání souborů odpovídající zadanému vzoru</span><span class="sxs-lookup"><span data-stu-id="9aafb-154">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

<span data-ttu-id="9aafb-155">Předpokládejme hello následující soubory jsou umístěny ve složce `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="9aafb-155">Assume hello following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="9aafb-156">Po operaci odeslání hello hello kontejner bude zahrnovat hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="9aafb-156">After hello upload operation, hello container will include hello following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="9aafb-157">Pokud nezadáte možnost `/S`, AzCopy odešlete pouze objekty BLOB, které nejsou jsou umístěny ve virtuálním adresáři:</span><span class="sxs-lookup"><span data-stu-id="9aafb-157">If you do not specify option `/S`, AzCopy will only upload blobs that don't reside in a virtual directory:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="9aafb-158">Zadejte hello obsahu typ MIME cílový objekt blob</span><span class="sxs-lookup"><span data-stu-id="9aafb-158">Specify hello MIME content type of a destination blob</span></span>
<span data-ttu-id="9aafb-159">Ve výchozím nastavení, AzCopy nastaví typ obsahu hello cílový objekt blob příliš`application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-159">By default, AzCopy sets hello content type of a destination blob too`application/octet-stream`.</span></span> <span data-ttu-id="9aafb-160">Počínaje verzí 3.1.0, lze explicitně zadat typ obsahu hello prostřednictvím možnosti hello `/SetContentType:[content-type]`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-160">Beginning with version 3.1.0, you can explicitly specify hello content type via hello option `/SetContentType:[content-type]`.</span></span> <span data-ttu-id="9aafb-161">Tuto syntaxi nastaví hello typ obsahu pro všechny objekty BLOB v operaci odeslání.</span><span class="sxs-lookup"><span data-stu-id="9aafb-161">This syntax sets hello content type for all blobs in an upload operation.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

<span data-ttu-id="9aafb-162">Pokud zadáte `/SetContentType` bez hodnoty, pak AzCopy nastaví jednotlivých objektů blob nebo typ obsahu souboru podle tooits příponu souboru.</span><span class="sxs-lookup"><span data-stu-id="9aafb-162">If you specify `/SetContentType` without a value, then AzCopy will set each blob or file's content type according tooits file extension.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="blob-copy"></a><span data-ttu-id="9aafb-163">Objekt BLOB: kopírování</span><span class="sxs-lookup"><span data-stu-id="9aafb-163">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="9aafb-164">Zkopírujte jediného objektu blob v rámci účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="9aafb-164">Copy single blob within Storage account</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="9aafb-165">Při kopírování objektu blob v účtu úložiště, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="9aafb-165">When you copy a blob within a Storage account, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="9aafb-166">Zkopírujte jediného objektu blob mezi různými účty úložiště</span><span class="sxs-lookup"><span data-stu-id="9aafb-166">Copy single blob across Storage accounts</span></span>

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="9aafb-167">Při kopírování objektu blob mezi různými účty úložiště [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="9aafb-167">When you copy a blob across Storage accounts, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a><span data-ttu-id="9aafb-168">Zkopírujte jediného objektu blob ze sekundární oblasti tooprimary oblasti</span><span class="sxs-lookup"><span data-stu-id="9aafb-168">Copy single blob from secondary region tooprimary region</span></span>

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="9aafb-169">Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.</span><span class="sxs-lookup"><span data-stu-id="9aafb-169">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="9aafb-170">Zkopírujte jediného objektu blob a jeho snímky mezi různými účty úložiště</span><span class="sxs-lookup"><span data-stu-id="9aafb-170">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

<span data-ttu-id="9aafb-171">Po operaci kopírování hello bude obsahovat hello cílový kontejner objektů blob hello a jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="9aafb-171">After hello copy operation, hello target container will include hello blob and its snapshots.</span></span> <span data-ttu-id="9aafb-172">Za předpokladu, že objektu blob hello v předchozím příkladu hello má dva snímky, hello kontejner bude zahrnovat následující hello objektů blob a snímky:</span><span class="sxs-lookup"><span data-stu-id="9aafb-172">Assuming hello blob in hello example above has two snapshots, hello container will include hello following blob and snapshots:</span></span>

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="9aafb-173">Synchronně kopírovat mezi různými účty úložiště objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="9aafb-173">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="9aafb-174">AzCopy ve výchozím nastavení zkopíruje data mezi dva koncové body úložiště asynchronně.</span><span class="sxs-lookup"><span data-stu-id="9aafb-174">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="9aafb-175">Proto se operace kopírování hello spustí hello pozadí využití kapacity přebytečné šířky pásma, který má žádné SLA z hlediska jak rychlé se zkopírují do objektu blob a AzCopy bude průběžně kontrolovat stav kopie hello, dokud kopírování hello je dokončena nebo se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="9aafb-175">Therefore, hello copy operation will run in hello background using spare bandwidth capacity that has no SLA in terms of how fast a blob will be copied, and AzCopy will periodically check hello copy status until hello copying is completed or failed.</span></span>

<span data-ttu-id="9aafb-176">Hello `/SyncCopy` možnost zajistí, že operace kopírování hello získá konzistentní rychlost.</span><span class="sxs-lookup"><span data-stu-id="9aafb-176">hello `/SyncCopy` option ensures that hello copy operation will get consistent speed.</span></span> <span data-ttu-id="9aafb-177">AzCopy provede synchronní kopie hello tak, že stáhnete objekty BLOB hello toocopy z hello zadaný zdroj toolocal paměti a odesílání je toohello cíl úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="9aafb-177">AzCopy performs hello synchronous copy by downloading hello blobs toocopy from hello specified source toolocal memory, and then uploading them toohello Blob storage destination.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

<span data-ttu-id="9aafb-178">`/SyncCopy`může generovat další odchozí náklady porovnání tooasynchronous kopie hello doporučenému přístupu je tato možnost ve virtuálním počítači Azure, který je v hello toouse stejné oblasti jako vaše zdrojové úložiště účet tooavoid odchozí náklady.</span><span class="sxs-lookup"><span data-stu-id="9aafb-178">`/SyncCopy` might generate additional egress cost compared tooasynchronous copy, hello recommended approach is toouse this option in an Azure VM that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="9aafb-179">Soubor: stažení</span><span class="sxs-lookup"><span data-stu-id="9aafb-179">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="9aafb-180">Stáhnout jedním souborem</span><span class="sxs-lookup"><span data-stu-id="9aafb-180">Download single file</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="9aafb-181">Pokud hello zadaný zdroj je sdílenou složku Azure a pak buď zadejte hello přesný název souboru, (*například* `abc.txt`) toodownload jeden soubor, nebo zadejte možnost `/S` toodownload všechny soubory ve sdílené složce hello rekurzivní.</span><span class="sxs-lookup"><span data-stu-id="9aafb-181">If hello specified source is an Azure file share, then you must either specify hello exact file name, (*e.g.* `abc.txt`) toodownload a single file, or specify option `/S` toodownload all files in hello share recursively.</span></span> <span data-ttu-id="9aafb-182">Probíhá pokus toospecify vzor souborů i možnost `/S` společně dojde chybě.</span><span class="sxs-lookup"><span data-stu-id="9aafb-182">Attempting toospecify both a file pattern and option `/S` together will result in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="9aafb-183">Stáhnout všechny soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-183">Download all files</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="9aafb-184">Všimněte si, že všechny prázdné složky nebudou staženy.</span><span class="sxs-lookup"><span data-stu-id="9aafb-184">Note that any empty folders will not be downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="9aafb-185">Soubor: nahrát</span><span class="sxs-lookup"><span data-stu-id="9aafb-185">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="9aafb-186">Odeslat jedním souborem</span><span class="sxs-lookup"><span data-stu-id="9aafb-186">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="9aafb-187">Odeslat všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="9aafb-187">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

<span data-ttu-id="9aafb-188">Všimněte si, že všechny prázdné složky nebude odeslána.</span><span class="sxs-lookup"><span data-stu-id="9aafb-188">Note that any empty folders will not be uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="9aafb-189">Nahrání souborů odpovídající zadanému vzoru</span><span class="sxs-lookup"><span data-stu-id="9aafb-189">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="file-copy"></a><span data-ttu-id="9aafb-190">Soubor: kopírování</span><span class="sxs-lookup"><span data-stu-id="9aafb-190">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="9aafb-191">Kopírování mezi sdílenými složkami</span><span class="sxs-lookup"><span data-stu-id="9aafb-191">Copy across file shares</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="9aafb-192">Při kopírování souboru mezi sdílenými složkami [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="9aafb-192">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-tooblob"></a><span data-ttu-id="9aafb-193">Zkopírujte z tooblob sdílené složky souborů</span><span class="sxs-lookup"><span data-stu-id="9aafb-193">Copy from file share tooblob</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="9aafb-194">Při kopírování souboru ze souboru tooblob sdílenou složku, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="9aafb-194">When you copy a file from file share tooblob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>


### <a name="copy-from-blob-toofile-share"></a><span data-ttu-id="9aafb-195">Zkopírujte ze složky toofile objektů blob</span><span class="sxs-lookup"><span data-stu-id="9aafb-195">Copy from blob toofile share</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="9aafb-196">Při kopírování souboru ze sdílené složky toofile objektů blob, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="9aafb-196">When you copy a file from blob toofile share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="9aafb-197">Synchronně kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="9aafb-197">Synchronously copy files</span></span>
<span data-ttu-id="9aafb-198">Můžete zadat hello `/SyncCopy` možnost toocopy data z úložiště File tooFile úložiště, ze souboru úložiště tooBlob úložiště a z úložiště objektů Blob tooFile úložiště synchronně, AzCopy k tomu stahování hello zdroje dat toolocal paměti a nahrajte ho znovu toodestination.</span><span class="sxs-lookup"><span data-stu-id="9aafb-198">You can specify hello `/SyncCopy` option toocopy data from File Storage tooFile Storage, from File Storage tooBlob Storage and from Blob Storage tooFile Storage synchronously, AzCopy does this by downloading hello source data toolocal memory and upload it again toodestination.</span></span> <span data-ttu-id="9aafb-199">Standardní výstupní náklady budou platit.</span><span class="sxs-lookup"><span data-stu-id="9aafb-199">Standard egress cost will apply.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

<span data-ttu-id="9aafb-200">Při kopírování ze souboru úložiště tooBlob úložiště, typu blob výchozí hello je objekt blob bloku, může uživatel zadat možnost `/BlobType:page` toochange hello cílový objekt blob typu.</span><span class="sxs-lookup"><span data-stu-id="9aafb-200">When copying from File Storage tooBlob Storage, hello default blob type is block blob, user can specify option `/BlobType:page` toochange hello destination blob type.</span></span>

<span data-ttu-id="9aafb-201">Všimněte si, že `/SyncCopy` může generovat další odchozí náklady porovnáním různých tooasynchronous kopírování, hello doporučený přístup je tato možnost v hello virtuálního počítače Azure, který je v hello toouse stejné oblasti jako vaše zdrojové úložiště účet tooavoid odchozí náklady.</span><span class="sxs-lookup"><span data-stu-id="9aafb-201">Note that `/SyncCopy` might generate additional egress cost comparing tooasynchronous copy, hello recommended approach is toouse this option in hello Azure VM which is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="table-export"></a><span data-ttu-id="9aafb-202">Tabulka: Export</span><span class="sxs-lookup"><span data-stu-id="9aafb-202">Table: Export</span></span>
### <a name="export-table"></a><span data-ttu-id="9aafb-203">Export tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-203">Export table</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

<span data-ttu-id="9aafb-204">AzCopy zapíše soubor manifestu toohello zadanou cílovou složku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-204">AzCopy writes a manifest file toohello specified destination folder.</span></span> <span data-ttu-id="9aafb-205">Soubor manifestu Hello se používá v hello import proces toolocate hello potřebná data souborů a provádět ověřování dat.</span><span class="sxs-lookup"><span data-stu-id="9aafb-205">hello manifest file is used in hello import process toolocate hello necessary data files and perform data validation.</span></span> <span data-ttu-id="9aafb-206">Soubor manifestu Hello používá hello následující zásady vytváření názvů ve výchozím nastavení:</span><span class="sxs-lookup"><span data-stu-id="9aafb-206">hello manifest file uses hello following naming convention by default:</span></span>

    <account name>_<table name>_<timestamp>.manifest

<span data-ttu-id="9aafb-207">Uživatele můžete také zadat možnost hello `/Manifest:<manifest file name>` název souboru manifestu tooset hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-207">User can also specify hello option `/Manifest:<manifest file name>` tooset hello manifest file name.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a><span data-ttu-id="9aafb-208">Export rozdělené do několika souborů</span><span class="sxs-lookup"><span data-stu-id="9aafb-208">Split export into multiple files</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

<span data-ttu-id="9aafb-209">Používá AzCopy *svazku index* v hello rozdělení dat názvy souborů toodistinguish více souborů.</span><span class="sxs-lookup"><span data-stu-id="9aafb-209">AzCopy uses a *volume index* in hello split data file names toodistinguish multiple files.</span></span> <span data-ttu-id="9aafb-210">index svazku Hello se skládá ze dvou částí *index klíče rozsahu oddílu* a *rozdělení souboru index*.</span><span class="sxs-lookup"><span data-stu-id="9aafb-210">hello volume index consists of two parts, a *partition key range index* and a *split file index*.</span></span> <span data-ttu-id="9aafb-211">Obě indexy jsou od nuly.</span><span class="sxs-lookup"><span data-stu-id="9aafb-211">Both indexes are zero-based.</span></span>

<span data-ttu-id="9aafb-212">index klíče rozsahu oddílu Hello bude 0, pokud uživatel není nastavena možnost `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-212">hello partition key range index will be 0 if user does not specify option `/PKRS`.</span></span>

<span data-ttu-id="9aafb-213">Předpokládejme například, AzCopy generuje dva soubory dat po hello uživatel zadá možnost `/SplitSize`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-213">For instance, suppose AzCopy generates two data files after hello user specifies option `/SplitSize`.</span></span> <span data-ttu-id="9aafb-214">Hello Výsledná data názvy souborů, může být:</span><span class="sxs-lookup"><span data-stu-id="9aafb-214">hello resulting data file names might be:</span></span>

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

<span data-ttu-id="9aafb-215">Všimněte si, že hello minimální možné hodnoty pro možnost `/SplitSize` je 32 MB.</span><span class="sxs-lookup"><span data-stu-id="9aafb-215">Note that hello minimum possible value for option `/SplitSize` is 32MB.</span></span> <span data-ttu-id="9aafb-216">Pokud hello zadaný cíl je úložiště objektů Blob, budou AzCopy rozdělení hello datový soubor po jeho velikosti dosáhnou hello blob velikost omezenou (200GB), bez ohledu na tom, jestli možnost `/SplitSize` byla zadána uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-216">If hello specified destination is Blob storage, AzCopy will split hello data file once its sizes reaches hello blob size limitation (200GB), regardless of whether option `/SplitSize` has been specified by hello user.</span></span>

### <a name="export-table-toojson-or-csv-data-file-format"></a><span data-ttu-id="9aafb-217">Export tabulky tooJSON nebo datový formát souboru CSV</span><span class="sxs-lookup"><span data-stu-id="9aafb-217">Export table tooJSON or CSV data file format</span></span>
<span data-ttu-id="9aafb-218">AzCopy ve výchozím nastavení exportuje tabulky tooJSON datových souborů.</span><span class="sxs-lookup"><span data-stu-id="9aafb-218">AzCopy by default exports tables tooJSON data files.</span></span> <span data-ttu-id="9aafb-219">Můžete zadat možnost hello `/PayloadFormat:JSON|CSV` tooexport hello tabulky jako JSON nebo CSV.</span><span class="sxs-lookup"><span data-stu-id="9aafb-219">You can specify hello option `/PayloadFormat:JSON|CSV` tooexport hello tables as JSON or CSV.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

<span data-ttu-id="9aafb-220">Při zadávání Formát datové části hello sdíleného svazku clusteru, AzCopy bude také generovat soubor schématu s příponou `.schema.csv` pro každý datový soubor.</span><span class="sxs-lookup"><span data-stu-id="9aafb-220">When specifying hello CSV payload format, AzCopy will also generate a schema file with file extension `.schema.csv` for each data file.</span></span>

### <a name="export-table-entities-concurrently"></a><span data-ttu-id="9aafb-221">Export tabulky souběžně entity</span><span class="sxs-lookup"><span data-stu-id="9aafb-221">Export table entities concurrently</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

<span data-ttu-id="9aafb-222">AzCopy se spustí souběžných operací tooexport entity, když uživatel hello určuje možnost `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-222">AzCopy will start concurrent operations tooexport entities when hello user specifies option `/PKRS`.</span></span> <span data-ttu-id="9aafb-223">Každé operace exportuje jeden rozsah klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="9aafb-223">Each operation exports one partition key range.</span></span>

<span data-ttu-id="9aafb-224">Všimněte si, že hello počet souběžných operací se řídí možnost `/NC`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-224">Note that hello number of concurrent operations is also controlled by option `/NC`.</span></span> <span data-ttu-id="9aafb-225">AzCopy používá jako výchozí hodnota hello hello počet jader procesorů `/NC` při kopírování entity tabulky, i když `/NC` nebyl zadán.</span><span class="sxs-lookup"><span data-stu-id="9aafb-225">AzCopy uses hello number of core processors as hello default value of `/NC` when copying table entities, even if `/NC` was not specified.</span></span> <span data-ttu-id="9aafb-226">Když uživatel hello určuje možnost `/PKRS`, AzCopy používá menší hello dvě hodnoty - oddílu rozsahy klíčů versus implicitně nebo explicitně zadané souběžných operací - toodetermine hello počtu souběžných operací toostart hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-226">When hello user specifies option `/PKRS`, AzCopy uses hello smaller of hello two values - partition key ranges versus implicitly or explicitly specified concurrent operations - toodetermine hello number of concurrent operations toostart.</span></span> <span data-ttu-id="9aafb-227">Další podrobnosti, zadejte `AzCopy /?:NC` v příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-227">For more details, type `AzCopy /?:NC` at hello command line.</span></span>

### <a name="export-table-tooblob"></a><span data-ttu-id="9aafb-228">Export tabulky tooblob</span><span class="sxs-lookup"><span data-stu-id="9aafb-228">Export table tooblob</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

<span data-ttu-id="9aafb-229">AzCopy vygeneruje soubor dat JSON do kontejneru objektů blob hello následující zásady vytváření názvů:</span><span class="sxs-lookup"><span data-stu-id="9aafb-229">AzCopy will generate a JSON data file into hello blob container with following naming convention:</span></span>

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

<span data-ttu-id="9aafb-230">Soubor dat JSON Hello generované následuje hello Formát datové části pro – minimální metadata.</span><span class="sxs-lookup"><span data-stu-id="9aafb-230">hello generated JSON data file follows hello payload format for minimal metadata.</span></span> <span data-ttu-id="9aafb-231">Podrobnosti o této Formát datové části v tématu [Formát datové části pro operace služby s tabulkou](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span><span class="sxs-lookup"><span data-stu-id="9aafb-231">For details on this payload format, see [Payload Format for Table Service Operations](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span></span>

<span data-ttu-id="9aafb-232">Upozorňujeme, že při exportu tabulky tooblobs, AzCopy bude stažení hello tabulka entity toolocal dočasná data souborů a potom nahrát objekt blob se tyto entity toohello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-232">Note that when exporting tables tooblobs, AzCopy will download hello Table entities toolocal temporary data files and then upload those entities toohello blob.</span></span> <span data-ttu-id="9aafb-233">Tyto soubory dočasná data, která jsou umístěny do hello deníku soubor, složku s cestou výchozí hello "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", můžete zadat možnost/toochange [deníku – soubor a složka] Z: hello umístění složky se souborem deníku a proto změnit umístění souborů hello dočasná data.</span><span class="sxs-lookup"><span data-stu-id="9aafb-233">These temporary data files are put into hello journal file folder with hello default path "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", you can specify option /Z:[journal-file-folder] toochange hello journal file folder location and thus change hello temporary data files location.</span></span> <span data-ttu-id="9aafb-234">Hello dočasná data, velikost souborů je určeno podle velikosti entity tabulky a hello velikost, který jste zadali pomocí /SplitSize možnost hello, i když hello dočasných dat souboru v místním disku budou odstraněny okamžitě poté, co byl nahrán objekt blob toohello, Zkontrolujte prosím, že jste máte dostatek místa toostore místního disku tyto soubory dočasná data před jejich odstraněním.</span><span class="sxs-lookup"><span data-stu-id="9aafb-234">hello temporary data files' size is decided by your table entities' size and hello size you specified with hello option /SplitSize, although hello temporary data file in local disk will be deleted instantly once it has been uploaded toohello blob, please make sure you have enough local disk space toostore these temporary data files before they are deleted.</span></span>

## <a name="table-import"></a><span data-ttu-id="9aafb-235">Tabulka: Import</span><span class="sxs-lookup"><span data-stu-id="9aafb-235">Table: Import</span></span>
### <a name="import-table"></a><span data-ttu-id="9aafb-236">Tabulky import</span><span class="sxs-lookup"><span data-stu-id="9aafb-236">Import table</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

<span data-ttu-id="9aafb-237">Hello možnost `/EntityOperation` Určuje, jak hello tooinsert entity do tabulky.</span><span class="sxs-lookup"><span data-stu-id="9aafb-237">hello option `/EntityOperation` indicates how tooinsert entities into hello table.</span></span> <span data-ttu-id="9aafb-238">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="9aafb-238">Possible values are:</span></span>

* <span data-ttu-id="9aafb-239">`InsertOrSkip`: Přeskočí stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-239">`InsertOrSkip`: Skips an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="9aafb-240">`InsertOrMerge`: Sloučí existující entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-240">`InsertOrMerge`: Merges an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="9aafb-241">`InsertOrReplace`: Nahrazuje stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-241">`InsertOrReplace`: Replaces an existing entity or inserts a new entity if it does not exist in hello table.</span></span>

<span data-ttu-id="9aafb-242">Všimněte si, že nelze zadat možnost `/PKRS` ve scénáři import hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-242">Note that you cannot specify option `/PKRS` in hello import scenario.</span></span> <span data-ttu-id="9aafb-243">Na rozdíl od hello export scénář, ve kterém musíte zadat možnost `/PKRS` toostart souběžných operací, AzCopy ve výchozím nastavení spustí souběžných operací při importu tabulky.</span><span class="sxs-lookup"><span data-stu-id="9aafb-243">Unlike hello export scenario, in which you must specify option `/PKRS` toostart concurrent operations, AzCopy will by default start concurrent operations when you import a table.</span></span> <span data-ttu-id="9aafb-244">Hello výchozí počet souběžných operací spuštění je rovna toohello počet procesorů jádra; Můžete však zadat jiný počet souběžných s možností `/NC`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-244">hello default number of concurrent operations started is equal toohello number of core processors; however, you can specify a different number of concurrent with option `/NC`.</span></span> <span data-ttu-id="9aafb-245">Další podrobnosti, zadejte `AzCopy /?:NC` v příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-245">For more details, type `AzCopy /?:NC` at hello command line.</span></span>

<span data-ttu-id="9aafb-246">Všimněte si, že AzCopy podporuje pouze import pro formát JSON, není sdílený svazek clusteru.</span><span class="sxs-lookup"><span data-stu-id="9aafb-246">Note that AzCopy only supports importing for JSON, not CSV.</span></span> <span data-ttu-id="9aafb-247">AzCopy nepodporuje import tabulky z vytvořené uživatelem JSON a manifest soubory.</span><span class="sxs-lookup"><span data-stu-id="9aafb-247">AzCopy does not support table imports from user-created JSON and manifest files.</span></span> <span data-ttu-id="9aafb-248">Oba tyto soubory musí pocházet z tabulky exportního AzCopy.</span><span class="sxs-lookup"><span data-stu-id="9aafb-248">Both of these files must come from an AzCopy table export.</span></span> <span data-ttu-id="9aafb-249">chyby tooavoid, neprovádějte žádné změny hello exportovat JSON nebo soubor manifestu.</span><span class="sxs-lookup"><span data-stu-id="9aafb-249">tooavoid errors, please do not modify hello exported JSON or manifest file.</span></span>

### <a name="import-entities-tootable-using-blobs"></a><span data-ttu-id="9aafb-250">Import entity tootable použití objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="9aafb-250">Import entities tootable using blobs</span></span>
<span data-ttu-id="9aafb-251">Předpokládejme, kontejner objektů Blob obsahuje následující hello: souboru A JSON představující Azure Table a jeho doprovodné soubor manifestu.</span><span class="sxs-lookup"><span data-stu-id="9aafb-251">Assume a Blob container contains hello following: A JSON file representing an Azure Table and its accompanying manifest file.</span></span>

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

<span data-ttu-id="9aafb-252">Můžete spustit následující příkaz tooimport entity do tabulky pomocí souboru manifestu hello v tomto kontejneru objektů blob hello:</span><span class="sxs-lookup"><span data-stu-id="9aafb-252">You can run hello following command tooimport entities into a table using hello manifest file in that blob container:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a><span data-ttu-id="9aafb-253">Další funkce AzCopy</span><span class="sxs-lookup"><span data-stu-id="9aafb-253">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a><span data-ttu-id="9aafb-254">Kopírovat pouze data, která neexistuje v cílovém umístění hello</span><span class="sxs-lookup"><span data-stu-id="9aafb-254">Only copy data that doesn't exist in hello destination</span></span>
<span data-ttu-id="9aafb-255">Hello `/XO` a `/XN` parametry umožňují tooexclude starší nebo novější zdroj prostředky z kopírování, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="9aafb-255">hello `/XO` and `/XN` parameters allow you tooexclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="9aafb-256">Pokud chcete pouze prostředky toocopy zdroje, které neexistují v cílovém umístění hello, můžete zadat oba parametry v hello AzCopy příkaz:</span><span class="sxs-lookup"><span data-stu-id="9aafb-256">If you only want toocopy source resources that don't exist in hello destination, you can specify both parameters in hello AzCopy command:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

<span data-ttu-id="9aafb-257">Všimněte si, že to není podporováno hello zdrojové nebo cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="9aafb-257">Note that this is not supported when either hello source or destination is a table.</span></span>

### <a name="use-a-response-file-toospecify-command-line-parameters"></a><span data-ttu-id="9aafb-258">Použít parametrů souboru odpovědí toospecify příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9aafb-258">Use a response file toospecify command-line parameters</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

<span data-ttu-id="9aafb-259">Parametry příkazového řádku AzCopy můžete zahrnout do souboru odpovědí.</span><span class="sxs-lookup"><span data-stu-id="9aafb-259">You can include any AzCopy command-line parameters in a response file.</span></span> <span data-ttu-id="9aafb-260">AzCopy procesy hello parametry v souboru hello jako v případě, kdyby byly zadány na příkazovém řádku hello, provádění přímé nahrazení s hello obsah souboru hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-260">AzCopy processes hello parameters in hello file as if they had been specified on hello command line, performing a direct substitution with hello contents of hello file.</span></span>

<span data-ttu-id="9aafb-261">Předpokládejme, soubor odpovědí s názvem `copyoperation.txt`, který obsahuje následující řádky hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-261">Assume a response file named `copyoperation.txt`, that contains hello following lines.</span></span> <span data-ttu-id="9aafb-262">Každý parametr AzCopy můžete zadat na jeden řádek</span><span class="sxs-lookup"><span data-stu-id="9aafb-262">Each AzCopy parameter can be specified on a single line</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

<span data-ttu-id="9aafb-263">nebo na samostatné řádky:</span><span class="sxs-lookup"><span data-stu-id="9aafb-263">or on separate lines:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

<span data-ttu-id="9aafb-264">AzCopy se nezdaří, pokud rozdělení hello parametr mezi dvěma čárami, jak je vidět tady pro hello `/sourcekey` parametr:</span><span class="sxs-lookup"><span data-stu-id="9aafb-264">AzCopy will fail if you split hello parameter across two lines, as shown here for hello `/sourcekey` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-toospecify-command-line-parameters"></a><span data-ttu-id="9aafb-265">Používání více odpovědi soubory toospecify parametrů příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9aafb-265">Use multiple response files toospecify command-line parameters</span></span>
<span data-ttu-id="9aafb-266">Předpokládejme, soubor odpovědí s názvem `source.txt` kontejner zdroj, který určuje:</span><span class="sxs-lookup"><span data-stu-id="9aafb-266">Assume a response file named `source.txt` that specifies a source container:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer

<span data-ttu-id="9aafb-267">A soubor odpovědí s názvem `dest.txt` specifikuje cílovou složku v systému souborů hello:</span><span class="sxs-lookup"><span data-stu-id="9aafb-267">And a response file named `dest.txt` that specifies a destination folder in hello file system:</span></span>

    /Dest:C:\myfolder

<span data-ttu-id="9aafb-268">A soubor odpovědí s názvem `options.txt` určující možnosti AzCopy:</span><span class="sxs-lookup"><span data-stu-id="9aafb-268">And a response file named `options.txt` that specifies options for AzCopy:</span></span>

    /S /Y

<span data-ttu-id="9aafb-269">toocall AzCopy s těmito soubory odpovědí, což jsou umístěny v adresáři `C:\responsefiles`, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="9aafb-269">toocall AzCopy with these response files, all of which reside in a directory `C:\responsefiles`, use this command:</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

<span data-ttu-id="9aafb-270">AzCopy zpracovává tento příkaz stejně, jako kdyby jste zahrnuli všechny hello jednotlivé parametry na příkazovém řádku hello:</span><span class="sxs-lookup"><span data-stu-id="9aafb-270">AzCopy processes this command just as it would if you included all of hello individual parameters on hello command line:</span></span>

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="9aafb-271">Zadejte sdílený přístupový podpis (SAS)</span><span class="sxs-lookup"><span data-stu-id="9aafb-271">Specify a shared access signature (SAS)</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

<span data-ttu-id="9aafb-272">Můžete také zadat SAS na kontejneru hello URI:</span><span class="sxs-lookup"><span data-stu-id="9aafb-272">You can also specify a SAS on hello container URI:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a><span data-ttu-id="9aafb-273">Složky pro soubor deníku</span><span class="sxs-lookup"><span data-stu-id="9aafb-273">Journal file folder</span></span>
<span data-ttu-id="9aafb-274">Při každém vydání tooAzCopy příkaz zkontroluje existenci souboru deníku ve výchozí složce hello nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti.</span><span class="sxs-lookup"><span data-stu-id="9aafb-274">Each time you issue a command tooAzCopy, it checks whether a journal file exists in hello default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="9aafb-275">Pokud soubor deníku hello neexistuje ani na jednom místě, AzCopy zpracovává operaci hello jako nové a generuje nový soubor deníku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-275">If hello journal file does not exist in either place, AzCopy treats hello operation as new and generates a new journal file.</span></span>

<span data-ttu-id="9aafb-276">Pokud soubor deníku hello neexistuje, AzCopy zkontroluje, zda hello příkazového řádku, které můžete vložit odpovídá hello příkazového řádku v souboru deníku hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-276">If hello journal file does exist, AzCopy will check whether hello command line that you input matches hello command line in hello journal file.</span></span> <span data-ttu-id="9aafb-277">Pokud se dva příkazy na příkazových řádcích hello shodují, obnoví AzCopy hello nedokončené operace.</span><span class="sxs-lookup"><span data-stu-id="9aafb-277">If hello two command lines match, AzCopy resumes hello incomplete operation.</span></span> <span data-ttu-id="9aafb-278">Pokud se neshodují, bude výzvami tooeither přepsat hello deníku souboru toostart operaci nového nebo toocancel hello aktuální operace.</span><span class="sxs-lookup"><span data-stu-id="9aafb-278">If they do not match, you will be prompted tooeither overwrite hello journal file toostart a new operation, or toocancel hello current operation.</span></span>

<span data-ttu-id="9aafb-279">Pokud chcete, aby toouse hello výchozí umístění souboru deníku hello:</span><span class="sxs-lookup"><span data-stu-id="9aafb-279">If you want toouse hello default location for hello journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

<span data-ttu-id="9aafb-280">Pokud není možnost `/Z`, nebo zadejte možnost `/Z` bez hello cesta ke složce, jako v příkladu nahoře, AzCopy hello deníku soubor vytvoří v hello výchozího umístění, což je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-280">If you omit option `/Z`, or specify option `/Z` without hello folder path, as shown above, AzCopy creates hello journal file in hello default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="9aafb-281">Pokud hello deníku soubor již existuje, AzCopy obnoví hello operaci na základě souboru deníku hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-281">If hello journal file already exists, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="9aafb-282">Pokud chcete, aby toospecify vlastní umístění souboru deníku hello:</span><span class="sxs-lookup"><span data-stu-id="9aafb-282">If you want toospecify a custom location for hello journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

<span data-ttu-id="9aafb-283">Tento příklad vytvoří soubor deníku hello, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="9aafb-283">This example creates hello journal file if it does not already exist.</span></span> <span data-ttu-id="9aafb-284">Pokud neexistuje, AzCopy obnoví hello operaci na základě souboru deníku hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-284">If it does exist, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="9aafb-285">Pokud chcete, aby tooresume AzCopy operace:</span><span class="sxs-lookup"><span data-stu-id="9aafb-285">If you want tooresume an AzCopy operation:</span></span>

```azcopy
AzCopy /Z:C:\journalfolder\
```

<span data-ttu-id="9aafb-286">Tento příklad obnoví hello poslední operace, které se pravděpodobně nezdařila toocomplete.</span><span class="sxs-lookup"><span data-stu-id="9aafb-286">This example resumes hello last operation, which may have failed toocomplete.</span></span>

### <a name="generate-a-log-file"></a><span data-ttu-id="9aafb-287">Generování souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="9aafb-287">Generate a log file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

<span data-ttu-id="9aafb-288">Pokud zadáte možnost `/V` bez zadání protokolu podrobné toohello cesta k souboru, potom AzCopy vytvoří soubor protokolu hello v hello výchozího umístění, což je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-288">If you specify option `/V` without providing a file path toohello verbose log, then AzCopy creates hello log file in hello default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span>

<span data-ttu-id="9aafb-289">Jinak můžete vytvořit soubor protokolu v do vlastního umístění:</span><span class="sxs-lookup"><span data-stu-id="9aafb-289">Otherwise, you can create an log file in a custom location:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

<span data-ttu-id="9aafb-290">Všimněte si, že pokud zadáte relativní cestu následující možnost `/V`, jako například `/V:test/azcopy1.log`, pak hello podrobného protokolování se vytvoří v hello aktuální pracovní adresář v podsložce s názvem `test`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-290">Note that if you specify a relative path following option `/V`, such as `/V:test/azcopy1.log`, then hello verbose log is created in hello current working directory within a subfolder named `test`.</span></span>

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a><span data-ttu-id="9aafb-291">Zadejte hello počet souběžných operací toostart</span><span class="sxs-lookup"><span data-stu-id="9aafb-291">Specify hello number of concurrent operations toostart</span></span>
<span data-ttu-id="9aafb-292">Možnost `/NC` určuje hello počet souběžných kopie operací.</span><span class="sxs-lookup"><span data-stu-id="9aafb-292">Option `/NC` specifies hello number of concurrent copy operations.</span></span> <span data-ttu-id="9aafb-293">Ve výchozím nastavení spustí AzCopy počet propustnost přenosu dat hello tooincrease souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="9aafb-293">By default, AzCopy starts a certain number of concurrent operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="9aafb-294">Pro operace s tabulkou hello počet souběžných operací je rovna toohello počet procesorů, které máte.</span><span class="sxs-lookup"><span data-stu-id="9aafb-294">For Table operations, hello number of concurrent operations is equal toohello number of processors you have.</span></span> <span data-ttu-id="9aafb-295">Pro objekt Blob a soubor operace, hello počet souběžných operací je rovnat 8 časy hello počet procesorů, které máte.</span><span class="sxs-lookup"><span data-stu-id="9aafb-295">For Blob and File operations, hello number of concurrent operations is equal 8 times hello number of processors you have.</span></span> <span data-ttu-id="9aafb-296">Pokud používáte AzCopy přes síť s malou šířkou pásma, můžete zadat nižší číslo pro /NC tooavoid selhání kvůli konfliktům prostředků.</span><span class="sxs-lookup"><span data-stu-id="9aafb-296">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for /NC tooavoid failure caused by resource competition.</span></span>

### <a name="run-azcopy-against-azure-storage-emulator"></a><span data-ttu-id="9aafb-297">Spusťte nástroj AzCopy emulátoru úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="9aafb-297">Run AzCopy against Azure Storage Emulator</span></span>
<span data-ttu-id="9aafb-298">AzCopy můžete spustit proti hello [emulátoru úložiště Azure](storage-use-emulator.md) pro objekty BLOB:</span><span class="sxs-lookup"><span data-stu-id="9aafb-298">You can run AzCopy against hello [Azure Storage Emulator](storage-use-emulator.md) for Blobs:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

<span data-ttu-id="9aafb-299">a tabulky:</span><span class="sxs-lookup"><span data-stu-id="9aafb-299">and Tables:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a><span data-ttu-id="9aafb-300">Parametry AzCopy</span><span class="sxs-lookup"><span data-stu-id="9aafb-300">AzCopy Parameters</span></span>
<span data-ttu-id="9aafb-301">Parametry pro AzCopy jsou popsané níže.</span><span class="sxs-lookup"><span data-stu-id="9aafb-301">Parameters for AzCopy are described below.</span></span> <span data-ttu-id="9aafb-302">Můžete také zadat jednu z následujících příkazů z příkazového řádku hello nápovědu pomocí nástroje AzCopy hello:</span><span class="sxs-lookup"><span data-stu-id="9aafb-302">You can also type one of hello following commands from hello command line for help in using AzCopy:</span></span>

* <span data-ttu-id="9aafb-303">Podrobnou nápovědu příkazového řádku pro AzCopy:`AzCopy /?`</span><span class="sxs-lookup"><span data-stu-id="9aafb-303">For detailed command-line help for AzCopy: `AzCopy /?`</span></span>
* <span data-ttu-id="9aafb-304">Další informace o všech AzCopy parametr:`AzCopy /?:SourceKey`</span><span class="sxs-lookup"><span data-stu-id="9aafb-304">For detailed help with any AzCopy parameter: `AzCopy /?:SourceKey`</span></span>
* <span data-ttu-id="9aafb-305">Příklady příkazového řádku:`AzCopy /?:Samples`</span><span class="sxs-lookup"><span data-stu-id="9aafb-305">For command-line examples: `AzCopy /?:Samples`</span></span>

### <a name="sourcesource"></a><span data-ttu-id="9aafb-306">/ Zdroj: "zdroj"</span><span class="sxs-lookup"><span data-stu-id="9aafb-306">/Source:"source"</span></span>
<span data-ttu-id="9aafb-307">Určuje hello zdrojová data z které toocopy.</span><span class="sxs-lookup"><span data-stu-id="9aafb-307">Specifies hello source data from which toocopy.</span></span> <span data-ttu-id="9aafb-308">Hello zdroj může být adresář systému souborů, kontejner objektů blob, virtuální adresář objektů blob, sdílené složky úložiště, adresáři se souborem úložiště nebo Azure table.</span><span class="sxs-lookup"><span data-stu-id="9aafb-308">hello source can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="9aafb-309">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-309">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destdestination"></a><span data-ttu-id="9aafb-310">/ Cíle: "cílové"</span><span class="sxs-lookup"><span data-stu-id="9aafb-310">/Dest:"destination"</span></span>
<span data-ttu-id="9aafb-311">Určuje cílový toocopy hello k.</span><span class="sxs-lookup"><span data-stu-id="9aafb-311">Specifies hello destination toocopy to.</span></span> <span data-ttu-id="9aafb-312">Hello cílem může být adresář systému souborů, kontejner objektů blob, virtuální adresář objektů blob, sdílené složky úložiště, adresáři se souborem úložiště nebo Azure table.</span><span class="sxs-lookup"><span data-stu-id="9aafb-312">hello destination can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="9aafb-313">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-313">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="patternfile-pattern"></a><span data-ttu-id="9aafb-314">/ Vzor: "vzor souborů"</span><span class="sxs-lookup"><span data-stu-id="9aafb-314">/Pattern:"file-pattern"</span></span>
<span data-ttu-id="9aafb-315">Určuje vzor souborů, která určuje, které toocopy soubory.</span><span class="sxs-lookup"><span data-stu-id="9aafb-315">Specifies a file pattern that indicates which files toocopy.</span></span> <span data-ttu-id="9aafb-316">chování Hello hello /Pattern parametru je určen podle umístění hello hello zdroje dat a hello přítomnost hello rekurzivní v uživatelském režimu.</span><span class="sxs-lookup"><span data-stu-id="9aafb-316">hello behavior of hello /Pattern parameter is determined by hello location of hello source data, and hello presence of hello recursive mode option.</span></span> <span data-ttu-id="9aafb-317">Prostřednictvím možnosti parametrem/s. je zadán rekurzivní režim</span><span class="sxs-lookup"><span data-stu-id="9aafb-317">Recursive mode is specified via option /S.</span></span>

<span data-ttu-id="9aafb-318">Pokud hello zadaný zdrojový adresář v systému souborů hello, pak platí standardní zástupné znaky, a zadat šablonu souboru hello je shoda na základě souborů v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-318">If hello specified source is a directory in hello file system, then standard wildcards are in effect, and hello file pattern provided is matched against files within hello directory.</span></span> <span data-ttu-id="9aafb-319">Pokud možnost, který je zadán parametr, pak AzCopy taky odpovídá zadanému vzoru hello proti všechny soubory ve všech podsložek pod adresářem hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-319">If option /S is specified, then AzCopy also matches hello specified pattern against all files in any subfolders beneath hello directory.</span></span>

<span data-ttu-id="9aafb-320">Pokud je zadaný zdroj hello kontejner objektů blob nebo virtuální adresář, nejsou použity zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="9aafb-320">If hello specified source is a blob container or virtual directory, then wildcards are not applied.</span></span> <span data-ttu-id="9aafb-321">Pokud je možnost, který je zadán parametr, pak AzCopy hello zadaný soubor vzor interpretuje jako předpona objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9aafb-321">If option /S is specified, then AzCopy interprets hello specified file pattern as a blob prefix.</span></span> <span data-ttu-id="9aafb-322">Pokud možnosti, kterou není zadán parametr, pak AzCopy odpovídá hello vzor souborů s názvy objektů blob přesný.</span><span class="sxs-lookup"><span data-stu-id="9aafb-322">If option /S is not specified, then AzCopy matches hello file pattern against exact blob names.</span></span>

<span data-ttu-id="9aafb-323">Pokud hello zadaný zdroj je sdílenou složku Azure a pak buď zadejte hello přesný název souboru, (například abc.txt) toocopy jeden soubor, nebo zadejte možnost /S toocopy všechny soubory v rekurzivně hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="9aafb-323">If hello specified source is an Azure file share, then you must either specify hello exact file name, (e.g. abc.txt) toocopy a single file, or specify option /S toocopy all files in hello share recursively.</span></span> <span data-ttu-id="9aafb-324">Pokus toospecify vzor souborů i možnost /S společně bude mít za následek chybu.</span><span class="sxs-lookup"><span data-stu-id="9aafb-324">Attempting toospecify both a file pattern and option /S together will result in an error.</span></span>

<span data-ttu-id="9aafb-325">AzCopy používá, když hello/Source je kontejner objektů blob nebo virtuální adresář objektů blob a hello používá velká a malá písmena odpovídající ve všech ostatních případech porovnávání malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="9aafb-325">AzCopy uses case-sensitive matching when hello /Source is a blob container or blob virtual directory, and uses case-insensitive matching in all hello other cases.</span></span>

<span data-ttu-id="9aafb-326">Hello výchozí soubor vzor použít, pokud je zadána žádná vzor souborů je *.*</span><span class="sxs-lookup"><span data-stu-id="9aafb-326">hello default file pattern used when no file pattern is specified is *.*</span></span> <span data-ttu-id="9aafb-327">pro umístění systému souborů nebo prázdnou předponu pro umístění služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9aafb-327">for a file system location or an empty prefix for an Azure Storage location.</span></span> <span data-ttu-id="9aafb-328">Zadání více vzorů souborů není podporováno.</span><span class="sxs-lookup"><span data-stu-id="9aafb-328">Specifying multiple file patterns is not supported.</span></span>

<span data-ttu-id="9aafb-329">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-329">**Applicable to:** Blobs, Files</span></span>

### <a name="destkeystorage-key"></a><span data-ttu-id="9aafb-330">/ DestKey: "klíč úložiště"</span><span class="sxs-lookup"><span data-stu-id="9aafb-330">/DestKey:"storage-key"</span></span>
<span data-ttu-id="9aafb-331">Určuje klíč účtu úložiště hello hello cílovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-331">Specifies hello storage account key for hello destination resource.</span></span>

<span data-ttu-id="9aafb-332">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-332">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destsassas-token"></a><span data-ttu-id="9aafb-333">/ DestSAS: "tokenu sas"</span><span class="sxs-lookup"><span data-stu-id="9aafb-333">/DestSAS:"sas-token"</span></span>
<span data-ttu-id="9aafb-334">Určuje sdíleného přístupového podpisu (SAS) s oprávněními ke čtení a zápisu pro cíl hello (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="9aafb-334">Specifies a Shared Access Signature (SAS) with READ and WRITE permissions for hello destination (if applicable).</span></span> <span data-ttu-id="9aafb-335">Obklopit hello SAS s dvojitými uvozovkami, protože pravděpodobně obsahuje speciální znaky příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-335">Surround hello SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="9aafb-336">Pokud hello cílovému prostředku je kontejner objektů blob, sdílené složky nebo tabulky, můžete buď zadat tuto možnost, za nímž následuje tokenu SAS hello nebo jako součást hello cílový objekt blob kontejner, sdílené složky nebo identifikátor URI tabulky bez této možnosti můžete zadat hello SAS.</span><span class="sxs-lookup"><span data-stu-id="9aafb-336">If hello destination resource is a blob container, file share or table, you can either specify this option followed by hello SAS token, or you can specify hello SAS as part of hello destination blob container, file share or table's URI, without this option.</span></span>

<span data-ttu-id="9aafb-337">Pokud hello zdrojové a cílové jsou oba objekty BLOB, pak hello cílový objekt blob se musí nacházet v rámci hello stejný jako zdrojový objekt blob hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="9aafb-337">If hello source and destination are both blobs, then hello destination blob must reside within hello same storage account as hello source blob.</span></span>

<span data-ttu-id="9aafb-338">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-338">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcekeystorage-key"></a><span data-ttu-id="9aafb-339">/ SourceKey: "klíč úložiště"</span><span class="sxs-lookup"><span data-stu-id="9aafb-339">/SourceKey:"storage-key"</span></span>
<span data-ttu-id="9aafb-340">Určuje klíč účtu úložiště hello hello zdrojovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-340">Specifies hello storage account key for hello source resource.</span></span>

<span data-ttu-id="9aafb-341">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-341">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcesassas-token"></a><span data-ttu-id="9aafb-342">/ SourceSAS: "tokenu sas"</span><span class="sxs-lookup"><span data-stu-id="9aafb-342">/SourceSAS:"sas-token"</span></span>
<span data-ttu-id="9aafb-343">Určuje sdíleného přístupového podpisu oprávnění ke čtení a seznamu zdroje hello (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="9aafb-343">Specifies a Shared Access Signature with READ and LIST permissions for hello source (if applicable).</span></span> <span data-ttu-id="9aafb-344">Obklopit hello SAS s dvojitými uvozovkami, protože pravděpodobně obsahuje speciální znaky příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-344">Surround hello SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="9aafb-345">Pokud hello zdroje prostředků je kontejner objektů blob a je k dispozici klíč ani SAS, budou kontejner objektů blob hello číst přes anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="9aafb-345">If hello source resource is a blob container, and neither a key nor a SAS is provided, then hello blob container will be read via anonymous access.</span></span>

<span data-ttu-id="9aafb-346">Pokud je zdroj hello sdílené složky nebo tabulka, je třeba zadat klíč nebo SAS.</span><span class="sxs-lookup"><span data-stu-id="9aafb-346">If hello source is a file share or table, a key or a SAS must be provided.</span></span>

<span data-ttu-id="9aafb-347">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-347">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="s"></a><span data-ttu-id="9aafb-348">/S</span><span class="sxs-lookup"><span data-stu-id="9aafb-348">/S</span></span>
<span data-ttu-id="9aafb-349">Určuje režim rekurzivní operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="9aafb-349">Specifies recursive mode for copy operations.</span></span> <span data-ttu-id="9aafb-350">V režimu rekurzivní AzCopy zkopíruje všechny objekty BLOB nebo soubory, které odpovídají hello zadaný soubor vzor, včetně programů v podsložkách.</span><span class="sxs-lookup"><span data-stu-id="9aafb-350">In recursive mode, AzCopy will copy all blobs or files that match hello specified file pattern, including those in subfolders.</span></span>

<span data-ttu-id="9aafb-351">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-351">**Applicable to:** Blobs, Files</span></span>

### <a name="blobtypeblock--page--append"></a><span data-ttu-id="9aafb-352">/ BlobType: "blokem" | "stránka" | "připojit"</span><span class="sxs-lookup"><span data-stu-id="9aafb-352">/BlobType:"block" | "page" | "append"</span></span>
<span data-ttu-id="9aafb-353">Určuje, zda text hello cílový objekt blob je objekt blob bloku, objektů blob stránky nebo doplňovací objekt blob.</span><span class="sxs-lookup"><span data-stu-id="9aafb-353">Specifies whether hello destination blob is a block blob, a page blob, or an append blob.</span></span> <span data-ttu-id="9aafb-354">Tuto možnost lze použít pouze v případě, že nahráváte do objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9aafb-354">This option is applicable only when you are uploading a blob.</span></span> <span data-ttu-id="9aafb-355">Jinak je generována chyba.</span><span class="sxs-lookup"><span data-stu-id="9aafb-355">Otherwise, an error is generated.</span></span> <span data-ttu-id="9aafb-356">Pokud hello cílový objekt blob a není tato možnost zadána, ve výchozím nastavení, AzCopy vytvoří objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-356">If hello destination is a blob and this option is not specified, by default, AzCopy creates a block blob.</span></span>

<span data-ttu-id="9aafb-357">**Platí pro:** objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="9aafb-357">**Applicable to:** Blobs</span></span>

### <a name="checkmd5"></a><span data-ttu-id="9aafb-358">/ CheckMD5</span><span class="sxs-lookup"><span data-stu-id="9aafb-358">/CheckMD5</span></span>
<span data-ttu-id="9aafb-359">Vypočítá hodnotu hash MD5 pro stažená data a ověří, zda hodnota hash MD5 hello uložené v objektu blob hello nebo vlastnost MD5 obsah souboru odpovídá hello vypočítat hodnotu hash.</span><span class="sxs-lookup"><span data-stu-id="9aafb-359">Calculates an MD5 hash for downloaded data and verifies that hello MD5 hash stored in hello blob or file's Content-MD5 property matches hello calculated hash.</span></span> <span data-ttu-id="9aafb-360">Kontrola Hello MD5 je vypnuta ve výchozím nastavení, takže při stahování dat je třeba zadat tuto možnost tooperform hello MD5 kontrolu.</span><span class="sxs-lookup"><span data-stu-id="9aafb-360">hello MD5 check is turned off by default, so you must specify this option tooperform hello MD5 check when downloading data.</span></span>

<span data-ttu-id="9aafb-361">Všimněte si, že Azure Storage není zárukou toho, že hello hodnota hash MD5 uložené pro objekt blob hello nebo souboru je aktuální.</span><span class="sxs-lookup"><span data-stu-id="9aafb-361">Note that Azure Storage doesn't guarantee that hello MD5 hash stored for hello blob or file is up-to-date.</span></span> <span data-ttu-id="9aafb-362">Klienta odpovědnost tooupdate hello MD5 je vždy, když se mění hello blob nebo souboru.</span><span class="sxs-lookup"><span data-stu-id="9aafb-362">It is client's responsibility tooupdate hello MD5 whenever hello blob or file is modified.</span></span>

<span data-ttu-id="9aafb-363">AzCopy vždy nastaví hello obsah MD5 vlastnost pro objektů blob v Azure nebo soubor po jeho nahrání toohello služby.</span><span class="sxs-lookup"><span data-stu-id="9aafb-363">AzCopy always sets hello Content-MD5 property for an Azure blob or file after uploading it toohello service.</span></span>  

<span data-ttu-id="9aafb-364">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-364">**Applicable to:** Blobs, Files</span></span>

### <a name="snapshot"></a><span data-ttu-id="9aafb-365">/ Snímku</span><span class="sxs-lookup"><span data-stu-id="9aafb-365">/Snapshot</span></span>
<span data-ttu-id="9aafb-366">Určuje, zda tootransfer snímky.</span><span class="sxs-lookup"><span data-stu-id="9aafb-366">Indicates whether tootransfer snapshots.</span></span> <span data-ttu-id="9aafb-367">Tato možnost je platná, pouze pokud je zdroj hello objekt blob.</span><span class="sxs-lookup"><span data-stu-id="9aafb-367">This option is only valid when hello source is a blob.</span></span>

<span data-ttu-id="9aafb-368">Hello přenášená blob snímky jsou přejmenovány v tomto formátu: .extension název objektu blob (snímku čas)</span><span class="sxs-lookup"><span data-stu-id="9aafb-368">hello transferred blob snapshots are renamed in this format: blob-name (snapshot-time).extension</span></span>

<span data-ttu-id="9aafb-369">Ve výchozím nastavení nebudou zkopírovány snímky.</span><span class="sxs-lookup"><span data-stu-id="9aafb-369">By default, snapshots are not copied.</span></span>

<span data-ttu-id="9aafb-370">**Platí pro:** objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="9aafb-370">**Applicable to:** Blobs</span></span>

### <a name="vverbose-log-file"></a><span data-ttu-id="9aafb-371">/ V: [podrobné souboru protokolu]</span><span class="sxs-lookup"><span data-stu-id="9aafb-371">/V:[verbose-log-file]</span></span>
<span data-ttu-id="9aafb-372">Výstupy podrobné stavové zprávy do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="9aafb-372">Outputs verbose status messages into a log file.</span></span>

<span data-ttu-id="9aafb-373">Ve výchozím nastavení, je hello podrobný soubor protokolu s názvem AzCopyVerbose.log v `%LocalAppData%\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="9aafb-373">By default, hello verbose log file is named AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="9aafb-374">Pokud zadáte existující umístění souborů pro tuto možnost, bude podrobného protokolování hello připojením toothat souboru.</span><span class="sxs-lookup"><span data-stu-id="9aafb-374">If you specify an existing file location for this option, hello verbose log will be appended toothat file.</span></span>  

<span data-ttu-id="9aafb-375">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-375">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="zjournal-file-folder"></a><span data-ttu-id="9aafb-376">/ Z: [deníku – soubor a složka]</span><span class="sxs-lookup"><span data-stu-id="9aafb-376">/Z:[journal-file-folder]</span></span>
<span data-ttu-id="9aafb-377">Určuje složku souboru deníku pro operace obnovení.</span><span class="sxs-lookup"><span data-stu-id="9aafb-377">Specifies a journal file folder for resuming an operation.</span></span>

<span data-ttu-id="9aafb-378">AzCopy vždy podporuje obnovení, pokud operace byla přerušena.</span><span class="sxs-lookup"><span data-stu-id="9aafb-378">AzCopy always supports resuming if an operation has been interrupted.</span></span>

<span data-ttu-id="9aafb-379">Pokud není tato možnost zadána, nebo je zadán bez cestu ke složce, pak AzCopy vytvoří v umístění hello výchozí, což je % LocalAppData%\Microsoft\Azure\AzCopy hello deníku souboru.</span><span class="sxs-lookup"><span data-stu-id="9aafb-379">If this option is not specified, or it is specified without a folder path, then AzCopy will create hello journal file in hello default location, which is %LocalAppData%\Microsoft\Azure\AzCopy.</span></span>

<span data-ttu-id="9aafb-380">Při každém vydání tooAzCopy příkaz zkontroluje existenci souboru deníku ve výchozí složce hello nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti.</span><span class="sxs-lookup"><span data-stu-id="9aafb-380">Each time you issue a command tooAzCopy, it checks whether a journal file exists in hello default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="9aafb-381">Pokud soubor deníku hello neexistuje ani na jednom místě, AzCopy zpracovává operaci hello jako nové a generuje nový soubor deníku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-381">If hello journal file does not exist in either place, AzCopy treats hello operation as new and generates a new journal file.</span></span>

<span data-ttu-id="9aafb-382">Pokud soubor deníku hello neexistuje, AzCopy zkontroluje, zda hello příkazového řádku, které můžete vložit odpovídá hello příkazového řádku v souboru deníku hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-382">If hello journal file does exist, AzCopy will check whether hello command line that you input matches hello command line in hello journal file.</span></span> <span data-ttu-id="9aafb-383">Pokud se dva příkazy na příkazových řádcích hello shodují, obnoví AzCopy hello nedokončené operace.</span><span class="sxs-lookup"><span data-stu-id="9aafb-383">If hello two command lines match, AzCopy resumes hello incomplete operation.</span></span> <span data-ttu-id="9aafb-384">Pokud se neshodují, bude výzvami tooeither přepsat hello deníku souboru toostart operaci nového nebo toocancel hello aktuální operace.</span><span class="sxs-lookup"><span data-stu-id="9aafb-384">If they do not match, you will be prompted tooeither overwrite hello journal file toostart a new operation, or toocancel hello current operation.</span></span>

<span data-ttu-id="9aafb-385">soubor deníku Hello se odstraní po úspěšném dokončení operace hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-385">hello journal file is deleted upon successful completion of hello operation.</span></span>

<span data-ttu-id="9aafb-386">Všimněte si, že obnovení ze souboru deníku vytvořeného v předchozí verzi AzCopy operace není podporována.</span><span class="sxs-lookup"><span data-stu-id="9aafb-386">Note that resuming an operation from a journal file created by a previous version of AzCopy is not supported.</span></span>

<span data-ttu-id="9aafb-387">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-387">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="parameter-file"></a><span data-ttu-id="9aafb-388">/@:"Parameter-File"</span><span class="sxs-lookup"><span data-stu-id="9aafb-388">/@:"parameter-file"</span></span>
<span data-ttu-id="9aafb-389">Určuje soubor, který obsahuje parametry.</span><span class="sxs-lookup"><span data-stu-id="9aafb-389">Specifies a file that contains parameters.</span></span> <span data-ttu-id="9aafb-390">AzCopy procesy hello parametry v souboru hello stejně, jako kdyby kdyby byly zadány na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-390">AzCopy processes hello parameters in hello file just as if they had been specified on hello command line.</span></span>

<span data-ttu-id="9aafb-391">V souboru odpovědí můžete zadat několik parametrů na jeden řádek nebo zadejte každý parametr na samostatném řádku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-391">In a response file, you can either specify multiple parameters on a single line, or specify each parameter on its own line.</span></span> <span data-ttu-id="9aafb-392">Všimněte si, že jednotlivé parametr nemůže zahrnovat více řádků.</span><span class="sxs-lookup"><span data-stu-id="9aafb-392">Note that an individual parameter cannot span multiple lines.</span></span>

<span data-ttu-id="9aafb-393">Soubory odezvy může zahrnovat komentáře řádky, které začínají symbolem # hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-393">Response files can include comments lines that begin with hello # symbol.</span></span>

<span data-ttu-id="9aafb-394">Můžete zadat několik souborů odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9aafb-394">You can specify multiple response files.</span></span> <span data-ttu-id="9aafb-395">Všimněte si však, že AzCopy nepodporuje soubory vnořené odezvy.</span><span class="sxs-lookup"><span data-stu-id="9aafb-395">However, note that AzCopy does not support nested response files.</span></span>

<span data-ttu-id="9aafb-396">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-396">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="y"></a><span data-ttu-id="9aafb-397">/Y</span><span class="sxs-lookup"><span data-stu-id="9aafb-397">/Y</span></span>
<span data-ttu-id="9aafb-398">Potlačí všechny výzvy potvrzení AzCopy.</span><span class="sxs-lookup"><span data-stu-id="9aafb-398">Suppresses all AzCopy confirmation prompts.</span></span>

<span data-ttu-id="9aafb-399">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-399">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="l"></a><span data-ttu-id="9aafb-400">/ L</span><span class="sxs-lookup"><span data-stu-id="9aafb-400">/L</span></span>
<span data-ttu-id="9aafb-401">Určuje operaci výpis pouze; žádná data budou zkopírována.</span><span class="sxs-lookup"><span data-stu-id="9aafb-401">Specifies a listing operation only; no data is copied.</span></span>

<span data-ttu-id="9aafb-402">AzCopy bude interpretovat hello pomocí této možnosti simulace pro spuštěné hello příkazového řádku bez možnosti/l a počet, kolik objekty se zkopírují, můžete zadat možnost /V v hello stejný čas toocheck objektů, které se mají zkopírovat hello podrobného protokolování.</span><span class="sxs-lookup"><span data-stu-id="9aafb-402">AzCopy will interpret hello using of this option as a simulation for running hello command line without this option /L and count how many objects will be copied, you can specify option /V at hello same time toocheck which objects will be copied in hello verbose log.</span></span>

<span data-ttu-id="9aafb-403">chování Hello tato možnost je určena také hello umístění hello zdroje dat a hello přítomnost hello rekurzivní možnost /S a soubor vzor možnost režimu /Pattern.</span><span class="sxs-lookup"><span data-stu-id="9aafb-403">hello behavior of this option is also determined by hello location of hello source data and hello presence of hello recursive mode option /S and file pattern option /Pattern.</span></span>

<span data-ttu-id="9aafb-404">AzCopy vyžaduje oprávnění seznamu a přečtěte si toto umístění zdroje, při použití této možnosti.</span><span class="sxs-lookup"><span data-stu-id="9aafb-404">AzCopy requires LIST and READ permission of this source location when using this option.</span></span>

<span data-ttu-id="9aafb-405">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-405">**Applicable to:** Blobs, Files</span></span>

### <a name="mt"></a><span data-ttu-id="9aafb-406">/ MT</span><span class="sxs-lookup"><span data-stu-id="9aafb-406">/MT</span></span>
<span data-ttu-id="9aafb-407">Nastaví čas hello stažený soubor poslední změny toobe hello stejný jako zdrojový objekt blob hello nebo souboru.</span><span class="sxs-lookup"><span data-stu-id="9aafb-407">Sets hello downloaded file's last-modified time toobe hello same as hello source blob or file's.</span></span>

<span data-ttu-id="9aafb-408">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-408">**Applicable to:** Blobs, Files</span></span>

### <a name="xn"></a><span data-ttu-id="9aafb-409">/XN</span><span class="sxs-lookup"><span data-stu-id="9aafb-409">/XN</span></span>
<span data-ttu-id="9aafb-410">Vyloučí novější zdrojovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-410">Excludes a newer source resource.</span></span> <span data-ttu-id="9aafb-411">Pokud hello čas poslední změny zdroje hello hello stejnou nebo novější, než cílový se nezkopírují Hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="9aafb-411">hello resource will not be copied if hello last modified time of hello source is hello same or newer than destination.</span></span>

<span data-ttu-id="9aafb-412">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-412">**Applicable to:** Blobs, Files</span></span>

### <a name="xo"></a><span data-ttu-id="9aafb-413">/XO</span><span class="sxs-lookup"><span data-stu-id="9aafb-413">/XO</span></span>
<span data-ttu-id="9aafb-414">Vyloučí starší zdrojovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-414">Excludes an older source resource.</span></span> <span data-ttu-id="9aafb-415">Pokud hello čas poslední změny zdroje hello hello stejné nebo starší než cílový se nezkopírují Hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="9aafb-415">hello resource will not be copied if hello last modified time of hello source is hello same or older than destination.</span></span>

<span data-ttu-id="9aafb-416">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-416">**Applicable to:** Blobs, Files</span></span>

### <a name="a"></a><span data-ttu-id="9aafb-417">/A</span><span class="sxs-lookup"><span data-stu-id="9aafb-417">/A</span></span>
<span data-ttu-id="9aafb-418">Ukládání pouze soubory, které mají nastaven atribut archivu hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-418">Uploads only files that have hello Archive attribute set.</span></span>

<span data-ttu-id="9aafb-419">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-419">**Applicable to:** Blobs, Files</span></span>

### <a name="iarashcnetoi"></a><span data-ttu-id="9aafb-420">/ IA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="9aafb-420">/IA:[RASHCNETOI]</span></span>
<span data-ttu-id="9aafb-421">Nahrávání pouze soubory, které žádné hello zadané sady atributů.</span><span class="sxs-lookup"><span data-stu-id="9aafb-421">Uploads only files that have any of hello specified attributes set.</span></span>

<span data-ttu-id="9aafb-422">Dostupné atributy patří:</span><span class="sxs-lookup"><span data-stu-id="9aafb-422">Available attributes include:</span></span>

* <span data-ttu-id="9aafb-423">R = soubory jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="9aafb-423">R = Read-only files</span></span>
* <span data-ttu-id="9aafb-424">A = soubory připravené k archivaci</span><span class="sxs-lookup"><span data-stu-id="9aafb-424">A = Files ready for archiving</span></span>
* <span data-ttu-id="9aafb-425">S = systémové soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-425">S = System files</span></span>
* <span data-ttu-id="9aafb-426">H = skryté soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-426">H = Hidden files</span></span>
* <span data-ttu-id="9aafb-427">C = komprimované soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-427">C = Compressed files</span></span>
* <span data-ttu-id="9aafb-428">N = normální soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-428">N = Normal files</span></span>
* <span data-ttu-id="9aafb-429">E = šifrovaných souborů</span><span class="sxs-lookup"><span data-stu-id="9aafb-429">E = Encrypted files</span></span>
* <span data-ttu-id="9aafb-430">T = dočasné soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-430">T = Temporary files</span></span>
* <span data-ttu-id="9aafb-431">O = Offline soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-431">O = Offline files</span></span>
* <span data-ttu-id="9aafb-432">I = není indexované soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-432">I = Non-indexed files</span></span>

<span data-ttu-id="9aafb-433">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-433">**Applicable to:** Blobs, Files</span></span>

### <a name="xarashcnetoi"></a><span data-ttu-id="9aafb-434">/ XA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="9aafb-434">/XA:[RASHCNETOI]</span></span>
<span data-ttu-id="9aafb-435">Vyloučí soubory, které žádné hello zadané sady atributů.</span><span class="sxs-lookup"><span data-stu-id="9aafb-435">Excludes files that have any of hello specified attributes set.</span></span>

<span data-ttu-id="9aafb-436">Dostupné atributy patří:</span><span class="sxs-lookup"><span data-stu-id="9aafb-436">Available attributes include:</span></span>

* <span data-ttu-id="9aafb-437">R = soubory jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="9aafb-437">R = Read-only files</span></span>
* <span data-ttu-id="9aafb-438">A = soubory připravené k archivaci</span><span class="sxs-lookup"><span data-stu-id="9aafb-438">A = Files ready for archiving</span></span>
* <span data-ttu-id="9aafb-439">S = systémové soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-439">S = System files</span></span>
* <span data-ttu-id="9aafb-440">H = skryté soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-440">H = Hidden files</span></span>
* <span data-ttu-id="9aafb-441">C = komprimované soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-441">C = Compressed files</span></span>
* <span data-ttu-id="9aafb-442">N = normální soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-442">N = Normal files</span></span>
* <span data-ttu-id="9aafb-443">E = šifrovaných souborů</span><span class="sxs-lookup"><span data-stu-id="9aafb-443">E = Encrypted files</span></span>
* <span data-ttu-id="9aafb-444">T = dočasné soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-444">T = Temporary files</span></span>
* <span data-ttu-id="9aafb-445">O = Offline soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-445">O = Offline files</span></span>
* <span data-ttu-id="9aafb-446">I = není indexované soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-446">I = Non-indexed files</span></span>

<span data-ttu-id="9aafb-447">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-447">**Applicable to:** Blobs, Files</span></span>

### <a name="delimiterdelimiter"></a><span data-ttu-id="9aafb-448">/ Oddělovač: "oddělovač.</span><span class="sxs-lookup"><span data-stu-id="9aafb-448">/Delimiter:"delimiter"</span></span>
<span data-ttu-id="9aafb-449">Označuje, že hello oddělovací znak použít toodelimit virtuálních adresářů v názvu objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9aafb-449">Indicates hello delimiter character used toodelimit virtual directories in a blob name.</span></span>

<span data-ttu-id="9aafb-450">Ve výchozím nastavení používá AzCopy / jako hello oddělovací znak.</span><span class="sxs-lookup"><span data-stu-id="9aafb-450">By default, AzCopy uses / as hello delimiter character.</span></span> <span data-ttu-id="9aafb-451">Ale AzCopy podporuje používání libovolného znaku běžné (například @, #, nebo %) jako oddělovač.</span><span class="sxs-lookup"><span data-stu-id="9aafb-451">However, AzCopy supports using any common character (such as @, #, or %) as a delimiter.</span></span> <span data-ttu-id="9aafb-452">Pokud potřebujete tooinclude jednu z těchto speciálních znaků v hello příkazového řádku, uzavřete název souboru hello s dvojité uvozovky.</span><span class="sxs-lookup"><span data-stu-id="9aafb-452">If you need tooinclude one of these special characters on hello command line, enclose hello file name with double quotes.</span></span>

<span data-ttu-id="9aafb-453">Tato možnost platí pouze pro stahování objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="9aafb-453">This option is only applicable for downloading blobs.</span></span>

<span data-ttu-id="9aafb-454">**Platí pro:** objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="9aafb-454">**Applicable to:** Blobs</span></span>

### <a name="ncnumber-of-concurrent-operations"></a><span data-ttu-id="9aafb-455">/ NC: "číslo z souběžných operace"</span><span class="sxs-lookup"><span data-stu-id="9aafb-455">/NC:"number-of-concurrent-operations"</span></span>
<span data-ttu-id="9aafb-456">Určuje hello počet souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="9aafb-456">Specifies hello number of concurrent operations.</span></span>

<span data-ttu-id="9aafb-457">AzCopy ve výchozím nastavení spustí počet propustnost přenosu dat hello tooincrease souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="9aafb-457">AzCopy by default starts a certain number of concurrent operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="9aafb-458">Všimněte si, že velký počet souběžných operací v prostředí s malou šířkou pásma může zahlcovat hello síťové připojení a zabránit hello operací z plně dokončení.</span><span class="sxs-lookup"><span data-stu-id="9aafb-458">Note that large number of concurrent operations in a low-bandwidth environment may overwhelm hello network connection and prevent hello operations from fully completing.</span></span> <span data-ttu-id="9aafb-459">Omezení souběžných operací podle skutečné dostupnou šířku pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="9aafb-459">Throttle concurrent operations based on actual available network bandwidth.</span></span>

<span data-ttu-id="9aafb-460">Hello horní limit pro souběžných operací je 512.</span><span class="sxs-lookup"><span data-stu-id="9aafb-460">hello upper limit for concurrent operations is 512.</span></span>

<span data-ttu-id="9aafb-461">**Platí pro:** objekty BLOB, soubory, tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-461">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcetypeblob--table"></a><span data-ttu-id="9aafb-462">/ SourceType: "Blob" | "Tabulka"</span><span class="sxs-lookup"><span data-stu-id="9aafb-462">/SourceType:"Blob" | "Table"</span></span>
<span data-ttu-id="9aafb-463">Určuje, že hello `source` prostředek je k dispozici v hello místní vývojové prostředí, spouštění v emulátoru úložiště hello objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9aafb-463">Specifies that hello `source` resource is a blob available in hello local development environment, running in hello storage emulator.</span></span>

<span data-ttu-id="9aafb-464">**Platí pro:** objekty BLOB, tabulek</span><span class="sxs-lookup"><span data-stu-id="9aafb-464">**Applicable to:** Blobs, Tables</span></span>

### <a name="desttypeblob--table"></a><span data-ttu-id="9aafb-465">/ DestType: "Blob" | "Tabulka"</span><span class="sxs-lookup"><span data-stu-id="9aafb-465">/DestType:"Blob" | "Table"</span></span>
<span data-ttu-id="9aafb-466">Určuje, že hello `destination` prostředek je k dispozici v hello místní vývojové prostředí, spouštění v emulátoru úložiště hello objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9aafb-466">Specifies that hello `destination` resource is a blob available in hello local development environment, running in hello storage emulator.</span></span>

<span data-ttu-id="9aafb-467">**Platí pro:** objekty BLOB, tabulek</span><span class="sxs-lookup"><span data-stu-id="9aafb-467">**Applicable to:** Blobs, Tables</span></span>

### <a name="pkrskey1key2key3"></a><span data-ttu-id="9aafb-468">/ PKRS: "key&#1;key&#2; klíč&#3;..."</span><span class="sxs-lookup"><span data-stu-id="9aafb-468">/PKRS:"key1#key2#key3#..."</span></span>
<span data-ttu-id="9aafb-469">Rozdělení hello tooenable klíče rozsahu oddílu Export dat v tabulce současně, což zvyšuje rychlost hello operace exportu hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-469">Splits hello partition key range tooenable exporting table data in parallel, which increases hello speed of hello export operation.</span></span>

<span data-ttu-id="9aafb-470">Pokud není tato možnost zadána, použije AzCopy jedním vláknem tooexport tabulka entity.</span><span class="sxs-lookup"><span data-stu-id="9aafb-470">If this option is not specified, then AzCopy uses a single thread tooexport table entities.</span></span> <span data-ttu-id="9aafb-471">Například pokud hello uživatel zadává /PKRS: "aa #bb", pak AzCopy spustí tři souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="9aafb-471">For example, if hello user specifies /PKRS:"aa#bb", then AzCopy starts three concurrent operations.</span></span>

<span data-ttu-id="9aafb-472">Každé operace exportuje mezi tři rozsahy klíčů oddílu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="9aafb-472">Each operation exports one of three partition key ranges, as shown below:</span></span>

  <span data-ttu-id="9aafb-473">[klíč prvního oddílu, aa)</span><span class="sxs-lookup"><span data-stu-id="9aafb-473">[first-partition-key, aa)</span></span>

  <span data-ttu-id="9aafb-474">[aa, bb)</span><span class="sxs-lookup"><span data-stu-id="9aafb-474">[aa, bb)</span></span>

  <span data-ttu-id="9aafb-475">[bb, klíč oddílu poslední]</span><span class="sxs-lookup"><span data-stu-id="9aafb-475">[bb, last-partition-key]</span></span>

<span data-ttu-id="9aafb-476">**Platí pro:** tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-476">**Applicable to:** Tables</span></span>

### <a name="splitsizefile-size"></a><span data-ttu-id="9aafb-477">/ SplitSize: "velikost souboru"</span><span class="sxs-lookup"><span data-stu-id="9aafb-477">/SplitSize:"file-size"</span></span>
<span data-ttu-id="9aafb-478">Určuje text hello exportovaný soubor rozdělení velikost v MB, hello minimální povolená hodnota je 32.</span><span class="sxs-lookup"><span data-stu-id="9aafb-478">Specifies hello exported file split size in MB, hello minimal value allowed is 32.</span></span>

<span data-ttu-id="9aafb-479">Pokud není tato možnost zadána, bude AzCopy exportovat soubor toosingle dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="9aafb-479">If this option is not specified, AzCopy will export table data toosingle file.</span></span>

<span data-ttu-id="9aafb-480">Pokud data tabulky hello je tooa exportovaný blob a hello exportovaný soubor dosáhne velikosti hello 200 GB limit pro velikost objektu blob, pak AzCopy rozdělí hello exportovaný soubor, i když není tato možnost zadána.</span><span class="sxs-lookup"><span data-stu-id="9aafb-480">If hello table data is exported tooa blob, and hello exported file size reaches hello 200 GB limit for blob size, then AzCopy will split hello exported file, even if this option is not specified.</span></span>

<span data-ttu-id="9aafb-481">**Platí pro:** tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-481">**Applicable to:** Tables</span></span>

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a><span data-ttu-id="9aafb-482">/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"</span><span class="sxs-lookup"><span data-stu-id="9aafb-482">/EntityOperation:"InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"</span></span>
<span data-ttu-id="9aafb-483">Určuje chování importu dat tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-483">Specifies hello table data import behavior.</span></span>

* <span data-ttu-id="9aafb-484">InsertOrSkip - přeskočí stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-484">InsertOrSkip - Skips an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="9aafb-485">InsertOrMerge - sloučí existující entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-485">InsertOrMerge - Merges an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="9aafb-486">InsertOrReplace - nahrazuje stávající entitu nebo vloží novou entitu, pokud neexistuje v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-486">InsertOrReplace - Replaces an existing entity or inserts a new entity if it does not exist in hello table.</span></span>

<span data-ttu-id="9aafb-487">**Platí pro:** tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-487">**Applicable to:** Tables</span></span>

### <a name="manifestmanifest-file"></a><span data-ttu-id="9aafb-488">/ Manifest: "manifest – soubor"</span><span class="sxs-lookup"><span data-stu-id="9aafb-488">/Manifest:"manifest-file"</span></span>
<span data-ttu-id="9aafb-489">Určuje soubor manifestu hello pro tabulky hello exportovat a importovat operaci.</span><span class="sxs-lookup"><span data-stu-id="9aafb-489">Specifies hello manifest file for hello table export and import operation.</span></span>

<span data-ttu-id="9aafb-490">Tato možnost je volitelné během operace exportu hello, AzCopy vygeneruje soubor manifestu se předdefinované názvem, pokud není tato možnost zadána.</span><span class="sxs-lookup"><span data-stu-id="9aafb-490">This option is optional during hello export operation, AzCopy will generate a manifest file with predefined name if this option is not specified.</span></span>

<span data-ttu-id="9aafb-491">Tato možnost je povinná během operace importu hello týkající se umísťování hello datových souborů.</span><span class="sxs-lookup"><span data-stu-id="9aafb-491">This option is required during hello import operation for locating hello data files.</span></span>

<span data-ttu-id="9aafb-492">**Platí pro:** tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-492">**Applicable to:** Tables</span></span>

### <a name="synccopy"></a><span data-ttu-id="9aafb-493">/ SyncCopy</span><span class="sxs-lookup"><span data-stu-id="9aafb-493">/SyncCopy</span></span>
<span data-ttu-id="9aafb-494">Určuje, zda toosynchronously kopírování objektů BLOB nebo soubory mezi dva koncové body Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9aafb-494">Indicates whether toosynchronously copy blobs or files between two Azure Storage endpoints.</span></span>

<span data-ttu-id="9aafb-495">AzCopy ve výchozím nastavení používá asynchronní kopie straně serveru.</span><span class="sxs-lookup"><span data-stu-id="9aafb-495">AzCopy by default uses server-side asynchronous copy.</span></span> <span data-ttu-id="9aafb-496">Zadejte tuto možnost tooperform synchronního kopírovat, které soubory ke stažení objektů BLOB nebo soubory toolocal paměti a odesílá je tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="9aafb-496">Specify this option tooperform a synchronous copy, which downloads blobs or files toolocal memory and then uploads them tooAzure Storage.</span></span>

<span data-ttu-id="9aafb-497">Tuto možnost můžete použít při kopírování souborů v rámci úložiště objektů Blob, úložiště File, nebo z objektu Blob úložiště tooFile úložiště nebo naopak naopak.</span><span class="sxs-lookup"><span data-stu-id="9aafb-497">You can use this option when copying files within Blob storage, within File storage, or from Blob storage tooFile storage or vice versa.</span></span>

<span data-ttu-id="9aafb-498">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-498">**Applicable to:** Blobs, Files</span></span>

### <a name="setcontenttypecontent-type"></a><span data-ttu-id="9aafb-499">/ SetContentType: "content-type"</span><span class="sxs-lookup"><span data-stu-id="9aafb-499">/SetContentType:"content-type"</span></span>
<span data-ttu-id="9aafb-500">Určuje typ obsahu hello MIME pro cílové objekty BLOB nebo soubory.</span><span class="sxs-lookup"><span data-stu-id="9aafb-500">Specifies hello MIME content type for destination blobs or files.</span></span>

<span data-ttu-id="9aafb-501">Nastaví AzCopy hello typ obsahu pro objekt blob nebo soubor tooapplication/octet-stream ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9aafb-501">AzCopy sets hello content type for a blob or file tooapplication/octet-stream by default.</span></span> <span data-ttu-id="9aafb-502">Můžete nastavit hello typ obsahu pro všechny objekty BLOB nebo soubory a explicitně zadat hodnotu pro tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="9aafb-502">You can set hello content type for all blobs or files by explicitly specifying a value for this option.</span></span>

<span data-ttu-id="9aafb-503">Pokud zadáte tuto možnost bez hodnoty, bude nastavte AzCopy, každý objekt blob nebo typ obsahu souboru podle tooits příponu souboru.</span><span class="sxs-lookup"><span data-stu-id="9aafb-503">If you specify this option without a value, then AzCopy will set each blob or file's content type according tooits file extension.</span></span>

<span data-ttu-id="9aafb-504">**Platí pro:** objekty BLOB, soubory</span><span class="sxs-lookup"><span data-stu-id="9aafb-504">**Applicable to:** Blobs, Files</span></span>

### <a name="payloadformatjson--csv"></a><span data-ttu-id="9aafb-505">/ PayloadFormat: "JSON" | "CSV"</span><span class="sxs-lookup"><span data-stu-id="9aafb-505">/PayloadFormat:"JSON" | "CSV"</span></span>
<span data-ttu-id="9aafb-506">Určuje formát hello hello tabulky exportovaná data souboru.</span><span class="sxs-lookup"><span data-stu-id="9aafb-506">Specifies hello format of hello table exported data file.</span></span>

<span data-ttu-id="9aafb-507">Pokud není tato možnost zadána, exportuje AzCopy ve výchozím nastavení tabulky datový soubor ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="9aafb-507">If this option is not specified, by default AzCopy exports table data file in JSON format.</span></span>

<span data-ttu-id="9aafb-508">**Platí pro:** tabulky</span><span class="sxs-lookup"><span data-stu-id="9aafb-508">**Applicable to:** Tables</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="9aafb-509">Známé problémy a doporučené postupy</span><span class="sxs-lookup"><span data-stu-id="9aafb-509">Known Issues and Best Practices</span></span>
### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="9aafb-510">Limit souběžných zápisy při kopírování dat</span><span class="sxs-lookup"><span data-stu-id="9aafb-510">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="9aafb-511">Při kopírování objektů BLOB nebo soubory s AzCopy, mějte na paměti, že jiná aplikace může být upravovat hello data během kopírování ho.</span><span class="sxs-lookup"><span data-stu-id="9aafb-511">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying hello data while you are copying it.</span></span> <span data-ttu-id="9aafb-512">Pokud je to možné Ujistěte se, že hello dat, který chcete kopírovat nemění během operace kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-512">If possible, ensure that hello data you are copying is not being modified during hello copy operation.</span></span> <span data-ttu-id="9aafb-513">Například při kopírování virtuálního pevného disku přidružený virtuální počítač Azure, ujistěte se, že žádné další aplikace jsou aktuálně zápis toohello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-513">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing toohello VHD.</span></span> <span data-ttu-id="9aafb-514">Dobře toodo, jedná se o leasing toobe prostředků hello zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="9aafb-514">A good way toodo this is by leasing hello resource toobe copied.</span></span> <span data-ttu-id="9aafb-515">Alternativně můžete nejprve vytvořte snímek hello virtuální pevný disk a poté zkopírujte hello snímku.</span><span class="sxs-lookup"><span data-stu-id="9aafb-515">Alternately, you can create a snapshot of hello VHD first and then copy hello snapshot.</span></span>

<span data-ttu-id="9aafb-516">Pokud nelze zabránit jiné aplikace z zápis tooblobs nebo soubory, když jsou kopírovány, pak mějte na paměti, která úlohou hello hello čas dokončení, hello kopírované prostředky buď již nemá úplné parita s prostředky zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-516">If you cannot prevent other applications from writing tooblobs or files while they are being copied, then keep in mind that by hello time hello job finishes, hello copied resources may no longer have full parity with hello source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="9aafb-517">Jedna instance nástroje AzCopy spusťte na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="9aafb-517">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="9aafb-518">AzCopy je navrženou toomaximize hello využití přenosu dat tooaccelerate hello váš počítač prostředků, doporučujeme spustit pouze jedna instance nástroje AzCopy na jednom počítači a zadejte možnost hello `/NC` Pokud potřebujete více souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="9aafb-518">AzCopy is designed toomaximize hello utilization of your machine resource tooaccelerate hello data transfer, we recommend you run only one AzCopy instance on one machine, and specify hello option `/NC` if you need more concurrent operations.</span></span> <span data-ttu-id="9aafb-519">Další podrobnosti, zadejte `AzCopy /?:NC` v příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="9aafb-519">For more details, type `AzCopy /?:NC` at hello command line.</span></span>

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a><span data-ttu-id="9aafb-520">Povolit algoritmus MD5 kompatibilní s FIPS pro AzCopy když jste "použití standardu FIPS kompatibilní s algoritmy pro šifrování, hašování a podpisování".</span><span class="sxs-lookup"><span data-stu-id="9aafb-520">Enable FIPS compliant MD5 algorithms for AzCopy when you "Use FIPS compliant algorithms for encryption, hashing and signing".</span></span>
<span data-ttu-id="9aafb-521">AzCopy ve výchozím nastavení používá rozhraní .NET MD5 implementace toocalculate hello MD5 při kopírování objektů, ale existují některé požadavky na zabezpečení, které je třeba AzCopy tooenable kompatibilní MD5 nastavení FIPS.</span><span class="sxs-lookup"><span data-stu-id="9aafb-521">AzCopy by default uses .NET MD5 implementation toocalculate hello MD5 when copying objects, but there are some security requirements that need AzCopy tooenable FIPS compliant MD5 setting.</span></span>

<span data-ttu-id="9aafb-522">Můžete vytvořit soubor app.config `AzCopy.exe.config` s vlastností `AzureStorageUseV1MD5` a umístí jej vyhraďte s AzCopy.exe.</span><span class="sxs-lookup"><span data-stu-id="9aafb-522">You can create an app.config file `AzCopy.exe.config` with property `AzureStorageUseV1MD5` and put it aside with AzCopy.exe.</span></span>

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

<span data-ttu-id="9aafb-523">Pro vlastnost "AzureStorageUseV1MD5" • True – hello výchozí hodnota AzCopy použije implementace rozhraní .NET MD5.</span><span class="sxs-lookup"><span data-stu-id="9aafb-523">For property "AzureStorageUseV1MD5" • True - hello default value, AzCopy will use .NET MD5 implementation.</span></span>
<span data-ttu-id="9aafb-524">• False – AzCopy použije algoritmus MD5 kompatibilní se standardem FIPS.</span><span class="sxs-lookup"><span data-stu-id="9aafb-524">• False – AzCopy will use FIPS compliant MD5 algorithm.</span></span>

<span data-ttu-id="9aafb-525">Všimněte si, že na počítač se systémem Windows ve výchozím nastavení je zakázáno algoritmy splňující standard FIPS, můžete v okně Spustit zadejte secpol.msc a zkontrolujte tento přepínač v nastavení zabezpečení -> Místní zásady -> zabezpečení -> – Možnosti kryptografie systému: použití vyhovující standardu FIPS pro šifrování, hašování a podpisování.</span><span class="sxs-lookup"><span data-stu-id="9aafb-525">Note that FIPS compliant algorithms is disabled by default on your Windows machine, you can type secpol.msc in your Run window and check this switch at Security Setting->Local Policy->Security Options->System cryptography: Use FIPS compliant algorithms for encryption, hashing and signing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9aafb-526">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9aafb-526">Next steps</span></span>
<span data-ttu-id="9aafb-527">Další informace o Azure Storage a AzCopy najdete v části toohello následující prostředky.</span><span class="sxs-lookup"><span data-stu-id="9aafb-527">For more information about Azure Storage and AzCopy, refer toohello following resources.</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="9aafb-528">Dokumentace k Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="9aafb-528">Azure Storage documentation:</span></span>
* [<span data-ttu-id="9aafb-529">Úvod tooAzure úložiště</span><span class="sxs-lookup"><span data-stu-id="9aafb-529">Introduction tooAzure Storage</span></span>](storage-introduction.md)
* [<span data-ttu-id="9aafb-530">Jak toouse úložiště Blob z rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9aafb-530">How toouse Blob storage from .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="9aafb-531">Jak toouse úložiště File z rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9aafb-531">How toouse File storage from .NET</span></span>](storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="9aafb-532">Jak toouse úložiště Table z rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9aafb-532">How toouse Table storage from .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="9aafb-533">Jak toocreate, spravovat nebo odstranit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="9aafb-533">How toocreate, manage, or delete a storage account</span></span>](storage-create-storage-account.md)
* [<span data-ttu-id="9aafb-534">Přenos dat pomocí nástroje AzCopy v systému Linux</span><span class="sxs-lookup"><span data-stu-id="9aafb-534">Transfer data with AzCopy on Linux</span></span>](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="9aafb-535">Příspěvky blogu Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="9aafb-535">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="9aafb-536">Představení náhled knihovny přesun dat úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="9aafb-536">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="9aafb-537">AzCopy: Úvod synchronní kopie a vlastní typ obsahu</span><span class="sxs-lookup"><span data-stu-id="9aafb-537">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="9aafb-538">AzCopy: Uvedení obecné dostupnosti AzCopy 3.0 a AzCopy 4.0 verze preview s podporou tabulky a souboru</span><span class="sxs-lookup"><span data-stu-id="9aafb-538">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="9aafb-539">AzCopy: Optimalizovaných pro rozsáhlé kopie scénáře</span><span class="sxs-lookup"><span data-stu-id="9aafb-539">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="9aafb-540">AzCopy: Podpora pro geograficky redundantní úložiště s přístupem pro čtení</span><span class="sxs-lookup"><span data-stu-id="9aafb-540">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="9aafb-541">AzCopy: Přenos dat pomocí SAS token a znovu spustit režim</span><span class="sxs-lookup"><span data-stu-id="9aafb-541">AzCopy: Transfer data with re-startable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="9aafb-542">AzCopy: Objekt Blob kopírování mezi účet pomocí</span><span class="sxs-lookup"><span data-stu-id="9aafb-542">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="9aafb-543">AzCopy: Nahrávání nebo stahování souborů pro objekty BLOB Azure</span><span class="sxs-lookup"><span data-stu-id="9aafb-543">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

