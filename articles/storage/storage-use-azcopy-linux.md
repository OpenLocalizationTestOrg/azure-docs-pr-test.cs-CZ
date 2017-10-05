---
title: "Zkopírovat nebo přesunout data do služby Azure Storage s AzCopy v systému Linux | Microsoft Docs"
description: "Pomocí AzCopy na nástroj Linux přesunutí nebo zkopírování dat do nebo z objektu blob a obsahu souborů. Kopírování dat do úložiště Azure z místních souborů, nebo zkopírujte data v rámci nebo mezi účty úložiště. Snadno migrujte data do úložiště Azure."
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
ms.date: 05/11/2017
ms.author: seguler
ms.openlocfilehash: d17f63dcee590529756d48d699f78b3fb30f973c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="transfer-data-with-azcopy-on-linux"></a><span data-ttu-id="503ba-105">Přenos dat pomocí nástroje AzCopy v systému Linux</span><span class="sxs-lookup"><span data-stu-id="503ba-105">Transfer data with AzCopy on Linux</span></span>
<span data-ttu-id="503ba-106">V systému Linux AzCopy je nástroj příkazového řádku pro kopírování dat do a z úložiště objektů Blob v Microsoft Azure a soubor pomocí jednoduchých příkazů optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="503ba-106">AzCopy on Linux is a command-line utility designed for copying data to and from Microsoft Azure Blob and File storage using simple commands with optimal performance.</span></span> <span data-ttu-id="503ba-107">Data můžete zkopírovat z jednoho objektu do druhého v rámci účtu úložiště nebo mezi účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="503ba-107">You can copy data from one object to another within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="503ba-108">Existují dvě verze nástroje AzCopy, které si můžete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="503ba-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="503ba-109">AzCopy v systému Linux je sestaven pomocí rozhraní .NET Framework Core, které cílí platformy Linux nabídky stylu POSIX možnosti příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="503ba-109">AzCopy on Linux is built with .NET Core Framework, which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="503ba-110">[AzCopy v systému Windows](storage-use-azcopy.md) je obsažena v rozhraní .NET Framework a nabízí možnosti příkazového řádku Windows stylu.</span><span class="sxs-lookup"><span data-stu-id="503ba-110">[AzCopy on Windows](storage-use-azcopy.md) is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="503ba-111">Tento článek se zabývá AzCopy v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="503ba-111">This article covers AzCopy on Linux.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="503ba-112">Stáhněte a nainstalujte AzCopy</span><span class="sxs-lookup"><span data-stu-id="503ba-112">Download and install AzCopy</span></span>
### <a name="installation-on-linux"></a><span data-ttu-id="503ba-113">Instalace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="503ba-113">Installation on Linux</span></span>

<span data-ttu-id="503ba-114">AzCopy v systému Linux vyžaduje základní rozhraní .NET framework na platformě.</span><span class="sxs-lookup"><span data-stu-id="503ba-114">AzCopy on Linux requires .NET Core framework on the platform.</span></span> <span data-ttu-id="503ba-115">Najdete v pokynech k instalaci na [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) stránky.</span><span class="sxs-lookup"><span data-stu-id="503ba-115">See the installation instructions on the [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) page.</span></span>

<span data-ttu-id="503ba-116">Jako příklad nainstalujme na Ubuntu 16.10 .NET Core.</span><span class="sxs-lookup"><span data-stu-id="503ba-116">As an example, let's install .NET Core on Ubuntu 16.10.</span></span> <span data-ttu-id="503ba-117">Nejnovější Průvodce instalací, najdete v článku [.NET Core v systému Linux](https://www.microsoft.com/net/core#linuxubuntu) instalační stránka.</span><span class="sxs-lookup"><span data-stu-id="503ba-117">For the latest installation guide, visit [.NET Core on Linux](https://www.microsoft.com/net/core#linuxubuntu) installation page.</span></span>


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.3
```

<span data-ttu-id="503ba-118">Po instalaci .NET Core, stáhněte a nainstalujte AzCopy.</span><span class="sxs-lookup"><span data-stu-id="503ba-118">Once you have installed .NET Core, download and install AzCopy.</span></span>

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

<span data-ttu-id="503ba-119">Po instalaci nástroje AzCopy v systému Linux můžete odebrat extrahované soubory.</span><span class="sxs-lookup"><span data-stu-id="503ba-119">You can remove the extracted files once AzCopy on Linux is installed.</span></span> <span data-ttu-id="503ba-120">Případně pokud nemáte oprávnění superuživatele, můžete také spustit pomocí skriptu prostředí 'azcopy' ve složce extrahované AzCopy.</span><span class="sxs-lookup"><span data-stu-id="503ba-120">Alternatively if you do not have superuser privileges, you can also run AzCopy using the shell script 'azcopy' in the extracted folder.</span></span> 

### <a name="alternative-installation-on-ubuntu"></a><span data-ttu-id="503ba-121">Alternativní instalace na Ubuntu</span><span class="sxs-lookup"><span data-stu-id="503ba-121">Alternative Installation on Ubuntu</span></span>

<span data-ttu-id="503ba-122">**Ubuntu 14.04**</span><span class="sxs-lookup"><span data-stu-id="503ba-122">**Ubuntu 14.04**</span></span>

<span data-ttu-id="503ba-123">Přidání výstižný zdroje pro platformu .net Core:</span><span class="sxs-lookup"><span data-stu-id="503ba-123">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="503ba-124">Přidejte výstižný zdroj pro produkt úložiště Linux společnosti Microsoft a nainstalujte AzCopy:</span><span class="sxs-lookup"><span data-stu-id="503ba-124">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

```bash
curl https://packages.microsoft.com/config/ubuntu/14.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

<span data-ttu-id="503ba-125">**Ubuntu 16.04**</span><span class="sxs-lookup"><span data-stu-id="503ba-125">**Ubuntu 16.04**</span></span>

<span data-ttu-id="503ba-126">Přidání výstižný zdroje pro platformu .net Core:</span><span class="sxs-lookup"><span data-stu-id="503ba-126">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="503ba-127">Přidejte výstižný zdroj pro produkt úložiště Linux společnosti Microsoft a nainstalujte AzCopy:</span><span class="sxs-lookup"><span data-stu-id="503ba-127">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

```bash
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

<span data-ttu-id="503ba-128">**Ubuntu 16.10**</span><span class="sxs-lookup"><span data-stu-id="503ba-128">**Ubuntu 16.10**</span></span>

<span data-ttu-id="503ba-129">Přidání výstižný zdroje pro platformu .net Core:</span><span class="sxs-lookup"><span data-stu-id="503ba-129">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="503ba-130">Přidejte výstižný zdroj pro produkt úložiště Linux společnosti Microsoft a nainstalujte AzCopy:</span><span class="sxs-lookup"><span data-stu-id="503ba-130">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

```bash
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="503ba-131">Zápis vaše první příkaz AzCopy</span><span class="sxs-lookup"><span data-stu-id="503ba-131">Writing your first AzCopy command</span></span>
<span data-ttu-id="503ba-132">Základní syntaxe pro příkazy AzCopy je:</span><span class="sxs-lookup"><span data-stu-id="503ba-132">The basic syntax for AzCopy commands is:</span></span>

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

<span data-ttu-id="503ba-133">Následující příklady ukazují různé scénáře pro kopírování dat do a z Microsoft Azure Blobs a soubory.</span><span class="sxs-lookup"><span data-stu-id="503ba-133">The following examples demonstrate various scenarios for copying data to and from Microsoft Azure Blobs and Files.</span></span> <span data-ttu-id="503ba-134">Odkazovat `azcopy --help` nabídky podrobné vysvětlení parametrů použitých v každém vzorku.</span><span class="sxs-lookup"><span data-stu-id="503ba-134">Refer to the `azcopy --help` menu for a detailed explanation of the parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="503ba-135">Objekt BLOB: stažení</span><span class="sxs-lookup"><span data-stu-id="503ba-135">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="503ba-136">Stáhnout jediného objektu blob</span><span class="sxs-lookup"><span data-stu-id="503ba-136">Download single blob</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="503ba-137">Pokud složka `/mnt/myfiles` neexistuje, AzCopy ji vytvoří a stáhne `abc.txt ` do nové složky.</span><span class="sxs-lookup"><span data-stu-id="503ba-137">If the folder `/mnt/myfiles` does not exist, AzCopy creates it and downloads `abc.txt ` into the new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="503ba-138">Stáhnout jediného objektu blob ze sekundární oblasti</span><span class="sxs-lookup"><span data-stu-id="503ba-138">Download single blob from secondary region</span></span>

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="503ba-139">Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.</span><span class="sxs-lookup"><span data-stu-id="503ba-139">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="503ba-140">Stažení všech objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="503ba-140">Download all blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="503ba-141">Předpokládejme, že následující objekty BLOB se nacházejí v zadaném kontejneru:</span><span class="sxs-lookup"><span data-stu-id="503ba-141">Assume the following blobs reside in the specified container:</span></span>  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

<span data-ttu-id="503ba-142">Po stažení operaci adresáři `/mnt/myfiles` zahrnuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="503ba-142">After the download operation, the directory `/mnt/myfiles` includes the following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

<span data-ttu-id="503ba-143">Pokud nezadáte možnost `--recursive`, budou staženy žádné objektů blob.</span><span class="sxs-lookup"><span data-stu-id="503ba-143">If you do not specify option `--recursive`, no blob will be downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="503ba-144">Stáhnout objekty BLOB se zadanou předponou.</span><span class="sxs-lookup"><span data-stu-id="503ba-144">Download blobs with specified prefix</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

<span data-ttu-id="503ba-145">Předpokládejme, že následující objekty BLOB se nacházejí v zadaném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="503ba-145">Assume the following blobs reside in the specified container.</span></span> <span data-ttu-id="503ba-146">Všechny objekty BLOB začínající předponou `a` staženy.</span><span class="sxs-lookup"><span data-stu-id="503ba-146">All blobs beginning with the prefix `a` are downloaded.</span></span>

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

<span data-ttu-id="503ba-147">Po stažení operaci složce `/mnt/myfiles` zahrnuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="503ba-147">After the download operation, the folder `/mnt/myfiles` includes the following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

<span data-ttu-id="503ba-148">Předpona, která se vztahuje na virtuální adresář, který tvoří první část názvu objektu blob.</span><span class="sxs-lookup"><span data-stu-id="503ba-148">The prefix applies to the virtual directory, which forms the first part of the blob name.</span></span> <span data-ttu-id="503ba-149">Ve výše uvedeném příkladu virtuální adresář neodpovídá zadané předpony, takže se stáhne žádné objektů blob.</span><span class="sxs-lookup"><span data-stu-id="503ba-149">In the example shown above, the virtual directory does not match the specified prefix, so no blob is downloaded.</span></span> <span data-ttu-id="503ba-150">Kromě toho pokud možnost `--recursive` není zadán, AzCopy nestáhne žádné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="503ba-150">In addition, if the option `--recursive` is not specified, AzCopy does not download any blobs.</span></span>

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a><span data-ttu-id="503ba-151">Nastavte čas poslední úpravy exportované soubory na stejnou hodnotu jako zdroj objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="503ba-151">Set the last-modified time of exported files to be same as the source blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

<span data-ttu-id="503ba-152">Můžete také vyloučit objekty BLOB z operace stažení podle jejich čas poslední změny.</span><span class="sxs-lookup"><span data-stu-id="503ba-152">You can also exclude blobs from the download operation based on their last-modified time.</span></span> <span data-ttu-id="503ba-153">Pokud chcete vyloučit objekty BLOB, jejichž poslední úpravy času je například stejnou nebo novější, než cílový soubor, přidejte `--exclude-newer` možnost:</span><span class="sxs-lookup"><span data-stu-id="503ba-153">For example, if you want to exclude blobs whose last modified time is the same or newer than the destination file, add the `--exclude-newer` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

<span data-ttu-id="503ba-154">Nebo pokud chcete vyloučit objekty BLOB, jejichž poslední úpravy času je na stejné nebo starší než cílový soubor, přidejte `--exclude-older` možnost:</span><span class="sxs-lookup"><span data-stu-id="503ba-154">Or if you want to exclude blobs whose last modified time is the same or older than the destination file, add the `--exclude-older` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a><span data-ttu-id="503ba-155">Objekt BLOB: nahrát</span><span class="sxs-lookup"><span data-stu-id="503ba-155">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="503ba-156">Odeslat jedním souborem</span><span class="sxs-lookup"><span data-stu-id="503ba-156">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="503ba-157">Pokud zadaný cílový kontejner neexistuje, AzCopy ji vytvoří a odešle soubor do ní.</span><span class="sxs-lookup"><span data-stu-id="503ba-157">If the specified destination container does not exist, AzCopy creates it and uploads the file into it.</span></span>

### <a name="upload-single-file-to-virtual-directory"></a><span data-ttu-id="503ba-158">Jediný soubor nahrát do virtuálního adresáře</span><span class="sxs-lookup"><span data-stu-id="503ba-158">Upload single file to virtual directory</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="503ba-159">Pokud zadaný virtuální adresář neexistuje, AzCopy nahrávání souboru v názvu objektu blob virtuálního adresáře (*například*, `vd/abc.txt` v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="503ba-159">If the specified virtual directory does not exist, AzCopy uploads the file to include the virtual directory in the blob name (*e.g.*, `vd/abc.txt` in the example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="503ba-160">Odeslat všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="503ba-160">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="503ba-161">Zadání `--recursive` odešle obsah zadaného adresáře k rekurzivnímu úložiště objektů Blob, což znamená, že jsou také odeslány všechny podsložky a jejich soubory.</span><span class="sxs-lookup"><span data-stu-id="503ba-161">Specifying option `--recursive` uploads the contents of the specified directory to Blob storage recursively, meaning that all subfolders and their files are uploaded as well.</span></span> <span data-ttu-id="503ba-162">Například předpokládejme následující soubory jsou umístěny ve složce `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="503ba-162">For instance, assume the following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="503ba-163">Po operaci odeslání kontejneru obsahuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="503ba-163">After the upload operation, the container includes the following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="503ba-164">Pokud možnost `--recursive` není zadán, jsou odeslány pouze následující tři soubory:</span><span class="sxs-lookup"><span data-stu-id="503ba-164">When the option `--recursive` is not specified, only the following three files are uploaded:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="503ba-165">Nahrání souborů odpovídající zadanému vzoru</span><span class="sxs-lookup"><span data-stu-id="503ba-165">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

<span data-ttu-id="503ba-166">Předpokládejme následující soubory jsou umístěny ve složce `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="503ba-166">Assume the following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="503ba-167">Po operaci odeslání kontejneru obsahuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="503ba-167">After the upload operation, the container includes the following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="503ba-168">Pokud možnost `--recursive` není zadán, AzCopy přeskočí soubory, které se nacházejí v podadresářích:</span><span class="sxs-lookup"><span data-stu-id="503ba-168">When the option `--recursive` is not specified, AzCopy skips files that are in sub-directories:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="503ba-169">Zadejte typ MIME obsahu cílový objekt blob</span><span class="sxs-lookup"><span data-stu-id="503ba-169">Specify the MIME content type of a destination blob</span></span>
<span data-ttu-id="503ba-170">Ve výchozím nastavení, AzCopy nastaví typ obsahu cílový objekt blob do `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="503ba-170">By default, AzCopy sets the content type of a destination blob to `application/octet-stream`.</span></span> <span data-ttu-id="503ba-171">Však můžete explicitně zadat typ obsahu prostřednictvím možnosti `--set-content-type [content-type]`.</span><span class="sxs-lookup"><span data-stu-id="503ba-171">However, you can explicitly specify the content type via the option `--set-content-type [content-type]`.</span></span> <span data-ttu-id="503ba-172">Tuto syntaxi nastaví typ obsahu pro všechny objekty BLOB v operaci odeslání.</span><span class="sxs-lookup"><span data-stu-id="503ba-172">This syntax sets the content type for all blobs in an upload operation.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

<span data-ttu-id="503ba-173">Pokud možnost `--set-content-type` je zadán bez hodnoty AzCopy nastaví jednotlivých objektů blob nebo typ obsahu souboru podle jeho přípony.</span><span class="sxs-lookup"><span data-stu-id="503ba-173">If the option `--set-content-type` is specified without a value, then AzCopy sets each blob or file's content type according to its file extension.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a><span data-ttu-id="503ba-174">Objekt BLOB: kopírování</span><span class="sxs-lookup"><span data-stu-id="503ba-174">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="503ba-175">Zkopírujte jediného objektu blob v rámci účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="503ba-175">Copy single blob within Storage account</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key> \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="503ba-176">Při kopírování objektu blob bez – možnosti synchronní kopie, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="503ba-176">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="503ba-177">Zkopírujte jediného objektu blob mezi různými účty úložiště</span><span class="sxs-lookup"><span data-stu-id="503ba-177">Copy single blob across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="503ba-178">Při kopírování objektu blob bez – možnosti synchronní kopie, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="503ba-178">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a><span data-ttu-id="503ba-179">Zkopírujte jediného objektu blob ze sekundární oblasti primární oblasti</span><span class="sxs-lookup"><span data-stu-id="503ba-179">Copy single blob from secondary region to primary region</span></span>

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="503ba-180">Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.</span><span class="sxs-lookup"><span data-stu-id="503ba-180">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="503ba-181">Zkopírujte jediného objektu blob a jeho snímky mezi různými účty úložiště</span><span class="sxs-lookup"><span data-stu-id="503ba-181">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

<span data-ttu-id="503ba-182">Po operaci kopírování zahrnuje cílový kontejner objektu blob a jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="503ba-182">After the copy operation, the target container includes the blob and its snapshots.</span></span> <span data-ttu-id="503ba-183">Kontejner obsahuje následující objektu blob a jeho snímky:</span><span class="sxs-lookup"><span data-stu-id="503ba-183">The container includes the following blob and its snapshots:</span></span>

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="503ba-184">Synchronně kopírovat mezi různými účty úložiště objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="503ba-184">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="503ba-185">AzCopy ve výchozím nastavení zkopíruje data mezi dva koncové body úložiště asynchronně.</span><span class="sxs-lookup"><span data-stu-id="503ba-185">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="503ba-186">Operace kopírování se spustí v pozadí využití kapacity přebytečné šířky pásma, která nemá žádné SLA z hlediska jak rychlé objekt blob je proto zkopírovány.</span><span class="sxs-lookup"><span data-stu-id="503ba-186">Therefore, the copy operation runs in the background using spare bandwidth capacity that has no SLA in terms of how fast a blob is copied.</span></span> 

<span data-ttu-id="503ba-187">`--sync-copy` Možnost zajistí, že operace kopírování získá konzistentní rychlost.</span><span class="sxs-lookup"><span data-stu-id="503ba-187">The `--sync-copy` option ensures that the copy operation gets consistent speed.</span></span> <span data-ttu-id="503ba-188">AzCopy provede synchronní kopie stažením objektů blob pro kopírování ze zadaného zdroje do místní paměti a uložte je do cílového umístění úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="503ba-188">AzCopy performs the synchronous copy by downloading the blobs to copy from the specified source to local memory, and then uploading them to the Blob storage destination.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

<span data-ttu-id="503ba-189">`--sync-copy`může generovat další odchozí nákladů ve srovnání s asynchronní kopírování.</span><span class="sxs-lookup"><span data-stu-id="503ba-189">`--sync-copy` might generate additional egress cost compared to asynchronous copy.</span></span> <span data-ttu-id="503ba-190">Doporučený přístup je pro tuto možnost použijte virtuální počítač Azure, který je ve stejné oblasti jako váš účet úložiště zdroj předejdete odchozí náklady.</span><span class="sxs-lookup"><span data-stu-id="503ba-190">The recommended approach is to use this option in an Azure VM, that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="503ba-191">Soubor: stažení</span><span class="sxs-lookup"><span data-stu-id="503ba-191">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="503ba-192">Stáhnout jedním souborem</span><span class="sxs-lookup"><span data-stu-id="503ba-192">Download single file</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="503ba-193">Pokud je zadaný zdroj sdílenou složku Azure, pak buď musíte zadat přesný název souboru, (*například* `abc.txt`) ke stažení jeden soubor nebo zadejte možnost `--recursive` ke stažení všechny soubory ve sdílené složce rekurzivně.</span><span class="sxs-lookup"><span data-stu-id="503ba-193">If the specified source is an Azure file share, then you must either specify the exact file name, (*e.g.* `abc.txt`) to download a single file, or specify option `--recursive` to download all files in the share recursively.</span></span> <span data-ttu-id="503ba-194">Probíhá pokus o zadat šablonu souboru a možnost `--recursive` společně dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="503ba-194">Attempting to specify both a file pattern and option `--recursive` together results in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="503ba-195">Stáhnout všechny soubory</span><span class="sxs-lookup"><span data-stu-id="503ba-195">Download all files</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="503ba-196">Všimněte si, že všechny prázdné složky nestahují.</span><span class="sxs-lookup"><span data-stu-id="503ba-196">Note that any empty folders are not downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="503ba-197">Soubor: nahrát</span><span class="sxs-lookup"><span data-stu-id="503ba-197">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="503ba-198">Odeslat jedním souborem</span><span class="sxs-lookup"><span data-stu-id="503ba-198">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="503ba-199">Odeslat všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="503ba-199">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="503ba-200">Všimněte si, že nejsou uloženy žádné prázdné složky.</span><span class="sxs-lookup"><span data-stu-id="503ba-200">Note that any empty folders are not uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="503ba-201">Nahrání souborů odpovídající zadanému vzoru</span><span class="sxs-lookup"><span data-stu-id="503ba-201">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a><span data-ttu-id="503ba-202">Soubor: kopírování</span><span class="sxs-lookup"><span data-stu-id="503ba-202">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="503ba-203">Kopírování mezi sdílenými složkami</span><span class="sxs-lookup"><span data-stu-id="503ba-203">Copy across file shares</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="503ba-204">Při kopírování souboru mezi sdílenými složkami [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="503ba-204">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-to-blob"></a><span data-ttu-id="503ba-205">Kopírování ze sdílené složky do objektu blob</span><span class="sxs-lookup"><span data-stu-id="503ba-205">Copy from file share to blob</span></span>

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="503ba-206">Při kopírování souboru ze sdílené složky do objektu blob, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="503ba-206">When you copy a file from file share to blob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-blob-to-file-share"></a><span data-ttu-id="503ba-207">Zkopírujte z objektu blob do sdílené složky</span><span class="sxs-lookup"><span data-stu-id="503ba-207">Copy from blob to file share</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="503ba-208">Při kopírování souboru z objektu blob do sdílené složky [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="503ba-208">When you copy a file from blob to file share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="503ba-209">Synchronně kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="503ba-209">Synchronously copy files</span></span>
<span data-ttu-id="503ba-210">Můžete zadat `--sync-copy` možnost ke zkopírování dat z úložiště File k úložišti souborů ze souboru úložiště do úložiště objektů Blob a z úložiště objektů Blob k úložišti souborů synchronně.</span><span class="sxs-lookup"><span data-stu-id="503ba-210">You can specify the `--sync-copy` option to copy data from File Storage to File Storage, from File Storage to Blob Storage and from Blob Storage to File Storage synchronously.</span></span> <span data-ttu-id="503ba-211">AzCopy spustí tuto operaci stahování zdrojová data do místní paměti a odesílá je do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="503ba-211">AzCopy runs this operation by downloading the source data to local memory, and then uploading it to destination.</span></span> <span data-ttu-id="503ba-212">V takovém případě platí standardní výstupní náklady.</span><span class="sxs-lookup"><span data-stu-id="503ba-212">In this case, standard egress cost applies.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

<span data-ttu-id="503ba-213">Při kopírování ze souboru úložiště do úložiště objektů Blob, výchozí typ objektu blob je objekt blob bloku, může uživatel zadat možnost `/BlobType:page` Chcete-li změnit typ cílového objektu blob.</span><span class="sxs-lookup"><span data-stu-id="503ba-213">When copying from File Storage to Blob Storage, the default blob type is block blob, user can specify option `/BlobType:page` to change the destination blob type.</span></span>

<span data-ttu-id="503ba-214">Všimněte si, že `--sync-copy` může generovat další odchozí náklady srovnáním asynchronní kopírování.</span><span class="sxs-lookup"><span data-stu-id="503ba-214">Note that `--sync-copy` might generate additional egress cost comparing to asynchronous copy.</span></span> <span data-ttu-id="503ba-215">Doporučený přístup je pro tuto možnost použijte virtuální počítač Azure, který je ve stejné oblasti jako váš účet úložiště zdroj předejdete odchozí náklady.</span><span class="sxs-lookup"><span data-stu-id="503ba-215">The recommended approach is to use this option in an Azure VM, that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="other-azcopy-features"></a><span data-ttu-id="503ba-216">Další funkce AzCopy</span><span class="sxs-lookup"><span data-stu-id="503ba-216">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a><span data-ttu-id="503ba-217">Kopírovat pouze data, která neexistuje v cílovém</span><span class="sxs-lookup"><span data-stu-id="503ba-217">Only copy data that doesn't exist in the destination</span></span>
<span data-ttu-id="503ba-218">`--exclude-older` a `--exclude-newer` parametry umožňují vyloučit prostředky starší nebo novější zdroj z kopírování, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="503ba-218">The `--exclude-older` and `--exclude-newer` parameters allow you to exclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="503ba-219">Pokud chcete kopírovat zdroje prostředky, které neexistují v cílovém, můžete zadat oba parametry v příkazu AzCopy:</span><span class="sxs-lookup"><span data-stu-id="503ba-219">If you only want to copy source resources that don't exist in the destination, you can specify both parameters in the AzCopy command:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-to-specify-command-line-parameters"></a><span data-ttu-id="503ba-220">Zadejte parametry příkazového řádku pomocí konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="503ba-220">Use a configuration file to specify command-line parameters</span></span>

```azcopy
azcopy --config-file "azcopy-config.ini"
```

<span data-ttu-id="503ba-221">Parametry příkazového řádku AzCopy můžete zahrnout v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="503ba-221">You can include any AzCopy command-line parameters in a configuration file.</span></span> <span data-ttu-id="503ba-222">AzCopy zpracovává parametry v souboru jako v případě, kdyby byly zadány na příkazovém řádku, provádění přímé nahrazení s obsahem souboru.</span><span class="sxs-lookup"><span data-stu-id="503ba-222">AzCopy processes the parameters in the file as if they had been specified on the command line, performing a direct substitution with the contents of the file.</span></span>

<span data-ttu-id="503ba-223">Předpokládejme, konfigurační soubor s názvem `copyoperation`, který obsahuje následující řádky.</span><span class="sxs-lookup"><span data-stu-id="503ba-223">Assume a configuration file named `copyoperation`, that contains the following lines.</span></span> <span data-ttu-id="503ba-224">Každý parametr AzCopy můžete zadat na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="503ba-224">Each AzCopy parameter can be specified on a single line.</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

<span data-ttu-id="503ba-225">nebo na samostatné řádky:</span><span class="sxs-lookup"><span data-stu-id="503ba-225">or on separate lines:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

<span data-ttu-id="503ba-226">AzCopy selže, pokud rozložení parametru mezi dvěma čárami, jak je vidět tady pro `--source-key` parametr:</span><span class="sxs-lookup"><span data-stu-id="503ba-226">AzCopy fails if you split the parameter across two lines, as shown here for the `--source-key` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="503ba-227">Zadejte sdílený přístupový podpis (SAS)</span><span class="sxs-lookup"><span data-stu-id="503ba-227">Specify a shared access signature (SAS)</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-sas <SAS1> \
    --dest-sas <SAS2> \
    --include abc.txt
```

<span data-ttu-id="503ba-228">Můžete také určit SAS URI kontejneru:</span><span class="sxs-lookup"><span data-stu-id="503ba-228">You can also specify a SAS on the container URI:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

<span data-ttu-id="503ba-229">Všimněte si, že AzCopy aktuálně podporuje jenom [SAS účtu](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="503ba-229">Note that AzCopy currently only supports the [Account SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

### <a name="journal-file-folder"></a><span data-ttu-id="503ba-230">Složky pro soubor deníku</span><span class="sxs-lookup"><span data-stu-id="503ba-230">Journal file folder</span></span>
<span data-ttu-id="503ba-231">Pokaždé, když příkaz azcopy, zkontroluje existenci souboru deníku ve výchozí složce, nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti.</span><span class="sxs-lookup"><span data-stu-id="503ba-231">Each time you issue a command to AzCopy, it checks whether a journal file exists in the default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="503ba-232">Pokud soubor deníku neexistuje ani na jednom místě, AzCopy zpracovává operaci jako nové a generuje nový soubor deníku.</span><span class="sxs-lookup"><span data-stu-id="503ba-232">If the journal file does not exist in either place, AzCopy treats the operation as new and generates a new journal file.</span></span>

<span data-ttu-id="503ba-233">Pokud soubor deníku neexistuje, AzCopy kontroluje, zda příkazového řádku, který můžete vložit odpovídá příkazového řádku v souboru deníku.</span><span class="sxs-lookup"><span data-stu-id="503ba-233">If the journal file does exist, AzCopy checks whether the command line that you input matches the command line in the journal file.</span></span> <span data-ttu-id="503ba-234">Pokud se dva příkazové řádky shodují, obnoví AzCopy neúplné operaci.</span><span class="sxs-lookup"><span data-stu-id="503ba-234">If the two command lines match, AzCopy resumes the incomplete operation.</span></span> <span data-ttu-id="503ba-235">Pokud se neshodují, AzCopy vyzve uživatele k buď přepsání souboru deníku spustit novou operaci, nebo zrušit aktuální operace.</span><span class="sxs-lookup"><span data-stu-id="503ba-235">If they do not match, AzCopy prompts user to either overwrite the journal file to start a new operation, or to cancel the current operation.</span></span>

<span data-ttu-id="503ba-236">Pokud chcete použít výchozí umístění pro soubor deníku:</span><span class="sxs-lookup"><span data-stu-id="503ba-236">If you want to use the default location for the journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

<span data-ttu-id="503ba-237">Pokud není možnost `--resume`, nebo zadejte možnost `--resume` bez cesta ke složce, jako v příkladu nahoře, AzCopy vytvoří soubor deníku ve výchozím umístění, což je `~\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="503ba-237">If you omit option `--resume`, or specify option `--resume` without the folder path, as shown above, AzCopy creates the journal file in the default location, which is `~\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="503ba-238">Pokud soubor deníku již existuje, AzCopy obnoví operaci na základě souboru deníku.</span><span class="sxs-lookup"><span data-stu-id="503ba-238">If the journal file already exists, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="503ba-239">Pokud chcete určit vlastní umístění souboru deníku:</span><span class="sxs-lookup"><span data-stu-id="503ba-239">If you want to specify a custom location for the journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

<span data-ttu-id="503ba-240">Tento příklad vytvoří soubor deníku, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="503ba-240">This example creates the journal file if it does not already exist.</span></span> <span data-ttu-id="503ba-241">Pokud neexistuje, AzCopy obnoví operaci na základě souboru deníku.</span><span class="sxs-lookup"><span data-stu-id="503ba-241">If it does exist, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="503ba-242">Pokud chcete pokračovat v činnosti AzCopy, zopakujte stejný příkaz.</span><span class="sxs-lookup"><span data-stu-id="503ba-242">If you want to resume an AzCopy operation, repeat the same command.</span></span> <span data-ttu-id="503ba-243">AzCopy v systému Linux potom zobrazí výzvu k potvrzení:</span><span class="sxs-lookup"><span data-stu-id="503ba-243">AzCopy on Linux then will prompt for confirmation:</span></span>

```azcopy
Incomplete operation with same command line detected at the journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want to resume the operation? Choose Yes to resume, choose No to overwrite the journal to start a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a><span data-ttu-id="503ba-244">Výstupní podrobné protokoly</span><span class="sxs-lookup"><span data-stu-id="503ba-244">Output verbose logs</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-the-number-of-concurrent-operations-to-start"></a><span data-ttu-id="503ba-245">Zadejte počet souběžných operací spuštění</span><span class="sxs-lookup"><span data-stu-id="503ba-245">Specify the number of concurrent operations to start</span></span>
<span data-ttu-id="503ba-246">Možnost `--parallel-level` určuje počet souběžných kopie operací.</span><span class="sxs-lookup"><span data-stu-id="503ba-246">Option `--parallel-level` specifies the number of concurrent copy operations.</span></span> <span data-ttu-id="503ba-247">Ve výchozím nastavení spustí AzCopy počet souběžných operací, pokud chcete zvýšit propustnost dat přenosu.</span><span class="sxs-lookup"><span data-stu-id="503ba-247">By default, AzCopy starts a certain number of concurrent operations to increase the data transfer throughput.</span></span> <span data-ttu-id="503ba-248">Počet souběžných operací osm časy počet procesorů, které máte.</span><span class="sxs-lookup"><span data-stu-id="503ba-248">The number of concurrent operations is equal eight times the number of processors you have.</span></span> <span data-ttu-id="503ba-249">Pokud používáte AzCopy přes síť s malou šířkou pásma, můžete na nižší číslo pro – paralelní úrovni, aby se zabránilo selhání kvůli konfliktům prostředků.</span><span class="sxs-lookup"><span data-stu-id="503ba-249">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for --parallel-level to avoid failure caused by resource competition.</span></span>

[!TIP]
><span data-ttu-id="503ba-250">Pokud chcete zobrazit úplný seznam parametrů AzCopy, podívejte se na 'azcopy – Nápověda' nabídky.</span><span class="sxs-lookup"><span data-stu-id="503ba-250">To view the complete list of AzCopy parameters, check out 'azcopy --help' menu.</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="503ba-251">Známé problémy a doporučené postupy</span><span class="sxs-lookup"><span data-stu-id="503ba-251">Known issues and best practices</span></span>
### <a name="error-net-core-is-not-found-in-the-system"></a><span data-ttu-id="503ba-252">Chyba: .NET Core není v systému nalezena.</span><span class="sxs-lookup"><span data-stu-id="503ba-252">Error: .NET Core is not found in the system.</span></span>
<span data-ttu-id="503ba-253">Pokud se vyskytne chyba s oznámením, že .NET Core není nainstalována v systému, cesta k .NET Core binární `dotnet` pravděpodobně chybí.</span><span class="sxs-lookup"><span data-stu-id="503ba-253">If you encounter an error stating that .NET Core is not installed in the system, the PATH to the .NET Core binary `dotnet` may be missing.</span></span>

<span data-ttu-id="503ba-254">Chcete-li tento problém vyřešit, najděte .NET Core binární v systému:</span><span class="sxs-lookup"><span data-stu-id="503ba-254">In order to address this issue, find the .NET Core binary in the system:</span></span>
```bash
sudo find / -name dotnet
```

<span data-ttu-id="503ba-255">To vrací cestu ke dotnet binární.</span><span class="sxs-lookup"><span data-stu-id="503ba-255">This returns the path to the dotnet binary.</span></span> 

    /opt/rh/rh-dotnetcore11/root/usr/bin/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/shared/Microsoft.NETCore.App/1.1.2/dotnet

<span data-ttu-id="503ba-256">Nyní přidáte tuto cestu do proměnné PATH.</span><span class="sxs-lookup"><span data-stu-id="503ba-256">Now add this path to the PATH variable.</span></span> <span data-ttu-id="503ba-257">Sudo upravte secure_path obsahuje cestu k binární dotnet:</span><span class="sxs-lookup"><span data-stu-id="503ba-257">For sudo, edit secure_path to contain the path to the dotnet binary:</span></span>
```bash 
sudo visudo
### Append the path found in the preceding example to 'secure_path' variable
```

<span data-ttu-id="503ba-258">V tomto příkladu secure_path proměnná přečte jako:</span><span class="sxs-lookup"><span data-stu-id="503ba-258">In this example, secure_path variable reads as:</span></span>

```
secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/opt/rh/rh-dotnetcore11/root/usr/bin/
```

<span data-ttu-id="503ba-259">Pro aktuálního uživatele upravte.bash_profile/.profile k zahrnovat cestu k binární v proměnné PATH dotnet.</span><span class="sxs-lookup"><span data-stu-id="503ba-259">For the current user, edit .bash_profile/.profile to include the path to the dotnet binary in PATH variable</span></span> 
```bash
vi ~/.bash_profile
### Append the path found in the preceding example to 'PATH' variable
```

<span data-ttu-id="503ba-260">Ověřte, zda .NET Core je teď v CESTĚ:</span><span class="sxs-lookup"><span data-stu-id="503ba-260">Verify that .NET Core is now in PATH:</span></span>
```bash
which dotnet
sudo which dotnet
```

### <a name="error-installing-azcopy"></a><span data-ttu-id="503ba-261">Chyba při instalaci nástroje AzCopy</span><span class="sxs-lookup"><span data-stu-id="503ba-261">Error Installing AzCopy</span></span>
<span data-ttu-id="503ba-262">Pokud narazíte na potíže s instalací AzCopy, můžete se pokusit spustit AzCopy pomocí skriptů bash ve extrahované `azcopy` složky.</span><span class="sxs-lookup"><span data-stu-id="503ba-262">If you encounter issues with AzCopy installation, you may try to run AzCopy using the bash script in the extracted `azcopy` folder.</span></span>

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="503ba-263">Limit souběžných zápisy při kopírování dat</span><span class="sxs-lookup"><span data-stu-id="503ba-263">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="503ba-264">Při kopírování objektů BLOB nebo soubory s AzCopy, mějte na paměti, že jiná aplikace může být úprava dat během kopírování ho.</span><span class="sxs-lookup"><span data-stu-id="503ba-264">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying the data while you are copying it.</span></span> <span data-ttu-id="503ba-265">Pokud je to možné Ujistěte se, které chcete kopírovat data nemění při kopírování.</span><span class="sxs-lookup"><span data-stu-id="503ba-265">If possible, ensure that the data you are copying is not being modified during the copy operation.</span></span> <span data-ttu-id="503ba-266">Například při kopírování virtuálního pevného disku přidružený virtuální počítač Azure, ujistěte se, že žádné další aplikace jsou aktuálně zápis na virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="503ba-266">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing to the VHD.</span></span> <span data-ttu-id="503ba-267">Dobrým způsobem, jak to udělat, je leasing prostředků, které se mají zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="503ba-267">A good way to do this is by leasing the resource to be copied.</span></span> <span data-ttu-id="503ba-268">Alternativně můžete nejprve vytvořte snímek virtuálního pevného disku a poté zkopírujte snímku.</span><span class="sxs-lookup"><span data-stu-id="503ba-268">Alternately, you can create a snapshot of the VHD first and then copy the snapshot.</span></span>

<span data-ttu-id="503ba-269">Pokud nelze zabránit jiné aplikace z zápis do objektů BLOB nebo soubory, když se kopírují, pak mějte na paměti, že v době dokončení úlohy, kopírované prostředky buď již nemá úplné parita s prostředky zdroje.</span><span class="sxs-lookup"><span data-stu-id="503ba-269">If you cannot prevent other applications from writing to blobs or files while they are being copied, then keep in mind that by the time the job finishes, the copied resources may no longer have full parity with the source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="503ba-270">Jedna instance nástroje AzCopy spusťte na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="503ba-270">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="503ba-271">AzCopy je navržen chcete maximalizovat využití prostředků vašeho počítače urychlit přenos dat, doporučujeme spustit pouze jedna instance nástroje AzCopy na jednom počítači a zadejte možnost `--parallel-level` Pokud potřebujete více souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="503ba-271">AzCopy is designed to maximize the utilization of your machine resource to accelerate the data transfer, we recommend you run only one AzCopy instance on one machine, and specify the option `--parallel-level` if you need more concurrent operations.</span></span> <span data-ttu-id="503ba-272">Další podrobnosti, zadejte `AzCopy --help parallel-level` na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="503ba-272">For more details, type `AzCopy --help parallel-level` at the command line.</span></span>

## <a name="next-steps"></a><span data-ttu-id="503ba-273">Další kroky</span><span class="sxs-lookup"><span data-stu-id="503ba-273">Next steps</span></span>
<span data-ttu-id="503ba-274">Další informace o Azure Storage a AzCopy najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="503ba-274">For more information about Azure Storage and AzCopy, see the following resources:</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="503ba-275">Dokumentace k Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="503ba-275">Azure Storage documentation:</span></span>
* [<span data-ttu-id="503ba-276">Úvod do Azure Storage</span><span class="sxs-lookup"><span data-stu-id="503ba-276">Introduction to Azure Storage</span></span>](storage-introduction.md)
* [<span data-ttu-id="503ba-277">Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="503ba-277">Create a storage account</span></span>](storage-create-storage-account.md)
* [<span data-ttu-id="503ba-278">Spravovat objekty BLOB pomocí Průzkumníka úložiště</span><span class="sxs-lookup"><span data-stu-id="503ba-278">Manage blobs with Storage Explorer</span></span>](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs)
* [<span data-ttu-id="503ba-279">Použití Azure CLI 2.0 s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="503ba-279">Using the Azure CLI 2.0 with Azure Storage</span></span>](storage-azure-cli.md)
* [<span data-ttu-id="503ba-280">Používání úložiště Blob z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="503ba-280">How to use Blob storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="503ba-281">Používání úložiště Blob z Javy</span><span class="sxs-lookup"><span data-stu-id="503ba-281">How to use Blob storage from Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="503ba-282">Používání úložiště Blob z Node.js</span><span class="sxs-lookup"><span data-stu-id="503ba-282">How to use Blob storage from Node.js</span></span>](storage-nodejs-how-to-use-blob-storage.md)
* [<span data-ttu-id="503ba-283">Používání úložiště Blob z Pythonu</span><span class="sxs-lookup"><span data-stu-id="503ba-283">How to use Blob storage from Python</span></span>](storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="503ba-284">Příspěvky blogu Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="503ba-284">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="503ba-285">Uvedení AzCopy na Linux Preview</span><span class="sxs-lookup"><span data-stu-id="503ba-285">Announcing AzCopy on Linux Preview</span></span>](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [<span data-ttu-id="503ba-286">Představení náhled knihovny přesun dat úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="503ba-286">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="503ba-287">AzCopy: Úvod synchronní kopie a vlastní typ obsahu</span><span class="sxs-lookup"><span data-stu-id="503ba-287">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="503ba-288">AzCopy: Uvedení obecné dostupnosti AzCopy 3.0 a AzCopy 4.0 verze preview s podporou tabulky a souboru</span><span class="sxs-lookup"><span data-stu-id="503ba-288">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="503ba-289">AzCopy: Optimalizovaných pro rozsáhlé kopie scénáře</span><span class="sxs-lookup"><span data-stu-id="503ba-289">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="503ba-290">AzCopy: Podpora pro geograficky redundantní úložiště s přístupem pro čtení</span><span class="sxs-lookup"><span data-stu-id="503ba-290">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="503ba-291">AzCopy: Přenos dat pomocí režimu s možností restartování a tokenu SAS</span><span class="sxs-lookup"><span data-stu-id="503ba-291">AzCopy: Transfer data with restartable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="503ba-292">AzCopy: Objekt Blob kopírování mezi účet pomocí</span><span class="sxs-lookup"><span data-stu-id="503ba-292">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="503ba-293">AzCopy: Nahrávání nebo stahování souborů pro objekty BLOB Azure</span><span class="sxs-lookup"><span data-stu-id="503ba-293">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

