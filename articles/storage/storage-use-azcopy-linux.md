---
title: "aaaCopy nebo přesunout data tooAzure úložiště s AzCopy v systému Linux | Microsoft Docs"
description: "Použijte hello AzCopy na Linux nástroj toomove nebo kopírování dat tooor z objektu blob a soubor obsahu. Kopírování dat tooAzure úložiště z místních souborů, nebo zkopírujte data v rámci nebo mezi účty úložiště. Vaše data tooAzure úložiště snadno migrujte."
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
ms.openlocfilehash: dccb03c9e8cc3ea661494e7834f307b0e3e30cb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-azcopy-on-linux"></a><span data-ttu-id="8affa-105">Přenos dat pomocí nástroje AzCopy v systému Linux</span><span class="sxs-lookup"><span data-stu-id="8affa-105">Transfer data with AzCopy on Linux</span></span>
<span data-ttu-id="8affa-106">V systému Linux AzCopy je nástroj příkazového řádku pro kopírování tooand dat z úložiště objektů Blob v Microsoft Azure a soubor pomocí jednoduchých příkazů optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="8affa-106">AzCopy on Linux is a command-line utility designed for copying data tooand from Microsoft Azure Blob and File storage using simple commands with optimal performance.</span></span> <span data-ttu-id="8affa-107">Data můžete zkopírovat z jednoho objektu tooanother v rámci účtu úložiště nebo mezi účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="8affa-107">You can copy data from one object tooanother within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="8affa-108">Existují dvě verze nástroje AzCopy, které si můžete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="8affa-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="8affa-109">AzCopy v systému Linux je sestaven pomocí rozhraní .NET Framework Core, které cílí platformy Linux nabídky stylu POSIX možnosti příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="8affa-109">AzCopy on Linux is built with .NET Core Framework, which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="8affa-110">[AzCopy v systému Windows](storage-use-azcopy.md) je obsažena v rozhraní .NET Framework a nabízí možnosti příkazového řádku Windows stylu.</span><span class="sxs-lookup"><span data-stu-id="8affa-110">[AzCopy on Windows](storage-use-azcopy.md) is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="8affa-111">Tento článek se zabývá AzCopy v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="8affa-111">This article covers AzCopy on Linux.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="8affa-112">Stáhněte a nainstalujte AzCopy</span><span class="sxs-lookup"><span data-stu-id="8affa-112">Download and install AzCopy</span></span>
### <a name="installation-on-linux"></a><span data-ttu-id="8affa-113">Instalace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="8affa-113">Installation on Linux</span></span>

<span data-ttu-id="8affa-114">AzCopy v systému Linux vyžaduje základní rozhraní .NET framework na platformě hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-114">AzCopy on Linux requires .NET Core framework on hello platform.</span></span> <span data-ttu-id="8affa-115">Najdete pokyny k instalaci hello na hello [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) stránky.</span><span class="sxs-lookup"><span data-stu-id="8affa-115">See hello installation instructions on hello [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) page.</span></span>

<span data-ttu-id="8affa-116">Jako příklad nainstalujme na Ubuntu 16.10 .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8affa-116">As an example, let's install .NET Core on Ubuntu 16.10.</span></span> <span data-ttu-id="8affa-117">Hello nejnovější Průvodce instalací, najdete v článku [.NET Core v systému Linux](https://www.microsoft.com/net/core#linuxubuntu) instalační stránka.</span><span class="sxs-lookup"><span data-stu-id="8affa-117">For hello latest installation guide, visit [.NET Core on Linux](https://www.microsoft.com/net/core#linuxubuntu) installation page.</span></span>


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.3
```

<span data-ttu-id="8affa-118">Po instalaci .NET Core, stáhněte a nainstalujte AzCopy.</span><span class="sxs-lookup"><span data-stu-id="8affa-118">Once you have installed .NET Core, download and install AzCopy.</span></span>

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

<span data-ttu-id="8affa-119">Po instalaci nástroje AzCopy v systému Linux, můžete odebrat soubory extrahovány hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-119">You can remove hello extracted files once AzCopy on Linux is installed.</span></span> <span data-ttu-id="8affa-120">Případně pokud nemáte oprávnění superuživatele, můžete také spustit AzCopy pomocí skriptu prostředí hello 'azcopy' v hello extrahovanou složku.</span><span class="sxs-lookup"><span data-stu-id="8affa-120">Alternatively if you do not have superuser privileges, you can also run AzCopy using hello shell script 'azcopy' in hello extracted folder.</span></span> 

### <a name="alternative-installation-on-ubuntu"></a><span data-ttu-id="8affa-121">Alternativní instalace na Ubuntu</span><span class="sxs-lookup"><span data-stu-id="8affa-121">Alternative Installation on Ubuntu</span></span>

<span data-ttu-id="8affa-122">**Ubuntu 14.04**</span><span class="sxs-lookup"><span data-stu-id="8affa-122">**Ubuntu 14.04**</span></span>

<span data-ttu-id="8affa-123">Přidání výstižný zdroje pro platformu .net Core:</span><span class="sxs-lookup"><span data-stu-id="8affa-123">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="8affa-124">Přidejte výstižný zdroj pro produkt úložiště Linux společnosti Microsoft a nainstalujte AzCopy:</span><span class="sxs-lookup"><span data-stu-id="8affa-124">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

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

<span data-ttu-id="8affa-125">**Ubuntu 16.04**</span><span class="sxs-lookup"><span data-stu-id="8affa-125">**Ubuntu 16.04**</span></span>

<span data-ttu-id="8affa-126">Přidání výstižný zdroje pro platformu .net Core:</span><span class="sxs-lookup"><span data-stu-id="8affa-126">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="8affa-127">Přidejte výstižný zdroj pro produkt úložiště Linux společnosti Microsoft a nainstalujte AzCopy:</span><span class="sxs-lookup"><span data-stu-id="8affa-127">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

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

<span data-ttu-id="8affa-128">**Ubuntu 16.10**</span><span class="sxs-lookup"><span data-stu-id="8affa-128">**Ubuntu 16.10**</span></span>

<span data-ttu-id="8affa-129">Přidání výstižný zdroje pro platformu .net Core:</span><span class="sxs-lookup"><span data-stu-id="8affa-129">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="8affa-130">Přidejte výstižný zdroj pro produkt úložiště Linux společnosti Microsoft a nainstalujte AzCopy:</span><span class="sxs-lookup"><span data-stu-id="8affa-130">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

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

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="8affa-131">Zápis vaše první příkaz AzCopy</span><span class="sxs-lookup"><span data-stu-id="8affa-131">Writing your first AzCopy command</span></span>
<span data-ttu-id="8affa-132">Hello základní syntaxe pro příkazy AzCopy je:</span><span class="sxs-lookup"><span data-stu-id="8affa-132">hello basic syntax for AzCopy commands is:</span></span>

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

<span data-ttu-id="8affa-133">Hello následující příklady ukazují různé scénáře pro kopírování data tooand z Microsoft Azure Blobs a soubory.</span><span class="sxs-lookup"><span data-stu-id="8affa-133">hello following examples demonstrate various scenarios for copying data tooand from Microsoft Azure Blobs and Files.</span></span> <span data-ttu-id="8affa-134">Odkazovat toohello `azcopy --help` nabídky podrobné vysvětlení hello parametrů použitých v každém vzorku.</span><span class="sxs-lookup"><span data-stu-id="8affa-134">Refer toohello `azcopy --help` menu for a detailed explanation of hello parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="8affa-135">Objekt BLOB: stažení</span><span class="sxs-lookup"><span data-stu-id="8affa-135">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="8affa-136">Stáhnout jediného objektu blob</span><span class="sxs-lookup"><span data-stu-id="8affa-136">Download single blob</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="8affa-137">Pokud složka hello `/mnt/myfiles` neexistuje, AzCopy ji vytvoří a stáhne `abc.txt ` do nové složky hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-137">If hello folder `/mnt/myfiles` does not exist, AzCopy creates it and downloads `abc.txt ` into hello new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="8affa-138">Stáhnout jediného objektu blob ze sekundární oblasti</span><span class="sxs-lookup"><span data-stu-id="8affa-138">Download single blob from secondary region</span></span>

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="8affa-139">Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.</span><span class="sxs-lookup"><span data-stu-id="8affa-139">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="8affa-140">Stažení všech objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="8affa-140">Download all blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="8affa-141">Předpokládejme hello následující objekty BLOB se nacházejí v zadaném kontejneru hello:</span><span class="sxs-lookup"><span data-stu-id="8affa-141">Assume hello following blobs reside in hello specified container:</span></span>  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

<span data-ttu-id="8affa-142">Po stažení operaci hello hello directory `/mnt/myfiles` zahrnuje hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="8affa-142">After hello download operation, hello directory `/mnt/myfiles` includes hello following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

<span data-ttu-id="8affa-143">Pokud nezadáte možnost `--recursive`, budou staženy žádné objektů blob.</span><span class="sxs-lookup"><span data-stu-id="8affa-143">If you do not specify option `--recursive`, no blob will be downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="8affa-144">Stáhnout objekty BLOB se zadanou předponou.</span><span class="sxs-lookup"><span data-stu-id="8affa-144">Download blobs with specified prefix</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

<span data-ttu-id="8affa-145">Předpokládejme hello následující objekty BLOB se nacházejí v zadaném kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-145">Assume hello following blobs reside in hello specified container.</span></span> <span data-ttu-id="8affa-146">Všechny objekty BLOB začínající předponou hello `a` staženy.</span><span class="sxs-lookup"><span data-stu-id="8affa-146">All blobs beginning with hello prefix `a` are downloaded.</span></span>

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

<span data-ttu-id="8affa-147">Po stažení operaci hello hello složky `/mnt/myfiles` zahrnuje hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="8affa-147">After hello download operation, hello folder `/mnt/myfiles` includes hello following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

<span data-ttu-id="8affa-148">Předpona Hello platí toohello virtuální adresář, který tvoří první část hello hello název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="8affa-148">hello prefix applies toohello virtual directory, which forms hello first part of hello blob name.</span></span> <span data-ttu-id="8affa-149">Ve výše uvedeném příkladu hello hello virtuální adresář neodpovídá zadané předpony hello, takže se stáhne žádný objekt blob.</span><span class="sxs-lookup"><span data-stu-id="8affa-149">In hello example shown above, hello virtual directory does not match hello specified prefix, so no blob is downloaded.</span></span> <span data-ttu-id="8affa-150">Kromě toho pokud hello možnost `--recursive` není zadán, AzCopy nestáhne žádné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="8affa-150">In addition, if hello option `--recursive` is not specified, AzCopy does not download any blobs.</span></span>

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a><span data-ttu-id="8affa-151">Nastavit čas poslední úpravy hello z exportované soubory toobe stejná hodnota jako hello zdroj objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="8affa-151">Set hello last-modified time of exported files toobe same as hello source blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

<span data-ttu-id="8affa-152">Můžete také vyloučit objekty BLOB z operace nástroje download hello podle jejich čas poslední změny.</span><span class="sxs-lookup"><span data-stu-id="8affa-152">You can also exclude blobs from hello download operation based on their last-modified time.</span></span> <span data-ttu-id="8affa-153">Příklad: Pokud chcete tooexclude objekty BLOB, jejichž čas poslední změny je hello stejnou nebo novější než hello cílový soubor, přidejte hello `--exclude-newer` možnost:</span><span class="sxs-lookup"><span data-stu-id="8affa-153">For example, if you want tooexclude blobs whose last modified time is hello same or newer than hello destination file, add hello `--exclude-newer` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

<span data-ttu-id="8affa-154">Nebo pokud chcete tooexclude objekty BLOB, jejichž čas poslední změny je hello stejné nebo starší než hello cílový soubor, přidejte hello `--exclude-older` možnost:</span><span class="sxs-lookup"><span data-stu-id="8affa-154">Or if you want tooexclude blobs whose last modified time is hello same or older than hello destination file, add hello `--exclude-older` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a><span data-ttu-id="8affa-155">Objekt BLOB: nahrát</span><span class="sxs-lookup"><span data-stu-id="8affa-155">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="8affa-156">Odeslat jedním souborem</span><span class="sxs-lookup"><span data-stu-id="8affa-156">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="8affa-157">Pokud hello zadaný cílový kontejner neexistuje, AzCopy ji vytvoří, a nahrávání hello souboru do ní.</span><span class="sxs-lookup"><span data-stu-id="8affa-157">If hello specified destination container does not exist, AzCopy creates it and uploads hello file into it.</span></span>

### <a name="upload-single-file-toovirtual-directory"></a><span data-ttu-id="8affa-158">Nahrát toovirtual directory jedním souborem</span><span class="sxs-lookup"><span data-stu-id="8affa-158">Upload single file toovirtual directory</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="8affa-159">Pokud hello zadaný virtuální adresář neexistuje, AzCopy odešle hello souboru tooinclude hello virtuální adresář v název objektu blob hello (*například*, `vd/abc.txt` v předchozím příkladu hello).</span><span class="sxs-lookup"><span data-stu-id="8affa-159">If hello specified virtual directory does not exist, AzCopy uploads hello file tooinclude hello virtual directory in hello blob name (*e.g.*, `vd/abc.txt` in hello example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="8affa-160">Odeslat všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="8affa-160">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="8affa-161">Zadání `--recursive` nahrávání hello obsah hello zadaný adresář tooBlob úložiště rekurzivně, což znamená, že jsou také odeslány všechny podsložky a jejich soubory.</span><span class="sxs-lookup"><span data-stu-id="8affa-161">Specifying option `--recursive` uploads hello contents of hello specified directory tooBlob storage recursively, meaning that all subfolders and their files are uploaded as well.</span></span> <span data-ttu-id="8affa-162">Například předpokládejme hello následující soubory jsou umístěny ve složce `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="8affa-162">For instance, assume hello following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="8affa-163">Po operaci odeslání hello hello kontejner obsahuje hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="8affa-163">After hello upload operation, hello container includes hello following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="8affa-164">Když hello možnost `--recursive` není zadán, jen hello následující tři soubory jsou odeslány:</span><span class="sxs-lookup"><span data-stu-id="8affa-164">When hello option `--recursive` is not specified, only hello following three files are uploaded:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="8affa-165">Nahrání souborů odpovídající zadanému vzoru</span><span class="sxs-lookup"><span data-stu-id="8affa-165">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

<span data-ttu-id="8affa-166">Předpokládejme hello následující soubory jsou umístěny ve složce `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="8affa-166">Assume hello following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="8affa-167">Po operaci odeslání hello hello kontejner obsahuje hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="8affa-167">After hello upload operation, hello container includes hello following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="8affa-168">Když hello možnost `--recursive` není zadán, AzCopy přeskočí soubory, které se nacházejí v podadresářích:</span><span class="sxs-lookup"><span data-stu-id="8affa-168">When hello option `--recursive` is not specified, AzCopy skips files that are in sub-directories:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="8affa-169">Zadejte hello obsahu typ MIME cílový objekt blob</span><span class="sxs-lookup"><span data-stu-id="8affa-169">Specify hello MIME content type of a destination blob</span></span>
<span data-ttu-id="8affa-170">Ve výchozím nastavení, AzCopy nastaví typ obsahu hello cílový objekt blob příliš`application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="8affa-170">By default, AzCopy sets hello content type of a destination blob too`application/octet-stream`.</span></span> <span data-ttu-id="8affa-171">Však můžete explicitně zadat typ obsahu hello prostřednictvím možnosti hello `--set-content-type [content-type]`.</span><span class="sxs-lookup"><span data-stu-id="8affa-171">However, you can explicitly specify hello content type via hello option `--set-content-type [content-type]`.</span></span> <span data-ttu-id="8affa-172">Tuto syntaxi nastaví hello typ obsahu pro všechny objekty BLOB v operaci odeslání.</span><span class="sxs-lookup"><span data-stu-id="8affa-172">This syntax sets hello content type for all blobs in an upload operation.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

<span data-ttu-id="8affa-173">Pokud hello možnost `--set-content-type` je pak AzCopy nastaví jednotlivých objektů blob nebo soubor zadaný bez hodnoty, je typ obsahu podle tooits příponu souboru.</span><span class="sxs-lookup"><span data-stu-id="8affa-173">If hello option `--set-content-type` is specified without a value, then AzCopy sets each blob or file's content type according tooits file extension.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a><span data-ttu-id="8affa-174">Objekt BLOB: kopírování</span><span class="sxs-lookup"><span data-stu-id="8affa-174">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="8affa-175">Zkopírujte jediného objektu blob v rámci účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="8affa-175">Copy single blob within Storage account</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key> \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="8affa-176">Při kopírování objektu blob bez – možnosti synchronní kopie, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="8affa-176">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="8affa-177">Zkopírujte jediného objektu blob mezi různými účty úložiště</span><span class="sxs-lookup"><span data-stu-id="8affa-177">Copy single blob across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="8affa-178">Při kopírování objektu blob bez – možnosti synchronní kopie, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="8affa-178">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a><span data-ttu-id="8affa-179">Zkopírujte jediného objektu blob ze sekundární oblasti tooprimary oblasti</span><span class="sxs-lookup"><span data-stu-id="8affa-179">Copy single blob from secondary region tooprimary region</span></span>

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="8affa-180">Všimněte si, že je nutné mít přístup pro čtení geograficky redundantní úložiště s povoleným.</span><span class="sxs-lookup"><span data-stu-id="8affa-180">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="8affa-181">Zkopírujte jediného objektu blob a jeho snímky mezi různými účty úložiště</span><span class="sxs-lookup"><span data-stu-id="8affa-181">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

<span data-ttu-id="8affa-182">Po operaci kopírování hello hello cílový kontejner obsahuje hello blob a jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="8affa-182">After hello copy operation, hello target container includes hello blob and its snapshots.</span></span> <span data-ttu-id="8affa-183">Hello kontejneru zahrnuje následující hello objektů blob a jeho snímky:</span><span class="sxs-lookup"><span data-stu-id="8affa-183">hello container includes hello following blob and its snapshots:</span></span>

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="8affa-184">Synchronně kopírovat mezi různými účty úložiště objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="8affa-184">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="8affa-185">AzCopy ve výchozím nastavení zkopíruje data mezi dva koncové body úložiště asynchronně.</span><span class="sxs-lookup"><span data-stu-id="8affa-185">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="8affa-186">Proto se zkopíruje spustí operace kopírování hello hello pozadí využití kapacity přebytečné šířky pásma, která nemá žádné SLA z hlediska jak rychlé objekt blob.</span><span class="sxs-lookup"><span data-stu-id="8affa-186">Therefore, hello copy operation runs in hello background using spare bandwidth capacity that has no SLA in terms of how fast a blob is copied.</span></span> 

<span data-ttu-id="8affa-187">Hello `--sync-copy` možnost zajistí, že operace kopírování hello získá konzistentní rychlost.</span><span class="sxs-lookup"><span data-stu-id="8affa-187">hello `--sync-copy` option ensures that hello copy operation gets consistent speed.</span></span> <span data-ttu-id="8affa-188">AzCopy provede synchronní kopie hello tak, že stáhnete objekty BLOB hello toocopy z hello zadaný zdroj toolocal paměti a odesílání je toohello cíl úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="8affa-188">AzCopy performs hello synchronous copy by downloading hello blobs toocopy from hello specified source toolocal memory, and then uploading them toohello Blob storage destination.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

<span data-ttu-id="8affa-189">`--sync-copy`může generovat další odchozí náklady porovnání tooasynchronous kopie.</span><span class="sxs-lookup"><span data-stu-id="8affa-189">`--sync-copy` might generate additional egress cost compared tooasynchronous copy.</span></span> <span data-ttu-id="8affa-190">Hello doporučenému přístupu je tato možnost ve virtuálním počítači Azure, který je v hello toouse stejné oblasti jako vaše zdrojové úložiště účet tooavoid odchozí náklady.</span><span class="sxs-lookup"><span data-stu-id="8affa-190">hello recommended approach is toouse this option in an Azure VM, that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="8affa-191">Soubor: stažení</span><span class="sxs-lookup"><span data-stu-id="8affa-191">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="8affa-192">Stáhnout jedním souborem</span><span class="sxs-lookup"><span data-stu-id="8affa-192">Download single file</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="8affa-193">Pokud hello zadaný zdroj je sdílenou složku Azure a pak buď zadejte hello přesný název souboru, (*například* `abc.txt`) toodownload jeden soubor, nebo zadejte možnost `--recursive` toodownload všechny soubory ve sdílené složce hello rekurzivní.</span><span class="sxs-lookup"><span data-stu-id="8affa-193">If hello specified source is an Azure file share, then you must either specify hello exact file name, (*e.g.* `abc.txt`) toodownload a single file, or specify option `--recursive` toodownload all files in hello share recursively.</span></span> <span data-ttu-id="8affa-194">Probíhá pokus toospecify vzor souborů i možnost `--recursive` společně dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="8affa-194">Attempting toospecify both a file pattern and option `--recursive` together results in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="8affa-195">Stáhnout všechny soubory</span><span class="sxs-lookup"><span data-stu-id="8affa-195">Download all files</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="8affa-196">Všimněte si, že všechny prázdné složky nestahují.</span><span class="sxs-lookup"><span data-stu-id="8affa-196">Note that any empty folders are not downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="8affa-197">Soubor: nahrát</span><span class="sxs-lookup"><span data-stu-id="8affa-197">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="8affa-198">Odeslat jedním souborem</span><span class="sxs-lookup"><span data-stu-id="8affa-198">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="8affa-199">Odeslat všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="8affa-199">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="8affa-200">Všimněte si, že nejsou uloženy žádné prázdné složky.</span><span class="sxs-lookup"><span data-stu-id="8affa-200">Note that any empty folders are not uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="8affa-201">Nahrání souborů odpovídající zadanému vzoru</span><span class="sxs-lookup"><span data-stu-id="8affa-201">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a><span data-ttu-id="8affa-202">Soubor: kopírování</span><span class="sxs-lookup"><span data-stu-id="8affa-202">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="8affa-203">Kopírování mezi sdílenými složkami</span><span class="sxs-lookup"><span data-stu-id="8affa-203">Copy across file shares</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="8affa-204">Při kopírování souboru mezi sdílenými složkami [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="8affa-204">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-tooblob"></a><span data-ttu-id="8affa-205">Zkopírujte z tooblob sdílené složky souborů</span><span class="sxs-lookup"><span data-stu-id="8affa-205">Copy from file share tooblob</span></span>

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="8affa-206">Při kopírování souboru ze souboru tooblob sdílenou složku, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="8affa-206">When you copy a file from file share tooblob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-blob-toofile-share"></a><span data-ttu-id="8affa-207">Zkopírujte ze složky toofile objektů blob</span><span class="sxs-lookup"><span data-stu-id="8affa-207">Copy from blob toofile share</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="8affa-208">Při kopírování souboru ze sdílené složky toofile objektů blob, [serverové kopie](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operace.</span><span class="sxs-lookup"><span data-stu-id="8affa-208">When you copy a file from blob toofile share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="8affa-209">Synchronně kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="8affa-209">Synchronously copy files</span></span>
<span data-ttu-id="8affa-210">Můžete zadat hello `--sync-copy` možnost toocopy dat z úložiště File tooFile úložiště, ze souboru úložiště tooBlob úložiště a z úložiště objektů Blob tooFile úložiště synchronně.</span><span class="sxs-lookup"><span data-stu-id="8affa-210">You can specify hello `--sync-copy` option toocopy data from File Storage tooFile Storage, from File Storage tooBlob Storage and from Blob Storage tooFile Storage synchronously.</span></span> <span data-ttu-id="8affa-211">AzCopy ve stahování hello zdroje dat toolocal paměti a odesílá je toodestination spustí tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="8affa-211">AzCopy runs this operation by downloading hello source data toolocal memory, and then uploading it toodestination.</span></span> <span data-ttu-id="8affa-212">V takovém případě platí standardní výstupní náklady.</span><span class="sxs-lookup"><span data-stu-id="8affa-212">In this case, standard egress cost applies.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

<span data-ttu-id="8affa-213">Při kopírování ze souboru úložiště tooBlob úložiště, typu blob výchozí hello je objekt blob bloku, může uživatel zadat možnost `/BlobType:page` toochange hello cílový objekt blob typu.</span><span class="sxs-lookup"><span data-stu-id="8affa-213">When copying from File Storage tooBlob Storage, hello default blob type is block blob, user can specify option `/BlobType:page` toochange hello destination blob type.</span></span>

<span data-ttu-id="8affa-214">Všimněte si, že `--sync-copy` může generovat další odchozí náklady porovnáním různých tooasynchronous kopie.</span><span class="sxs-lookup"><span data-stu-id="8affa-214">Note that `--sync-copy` might generate additional egress cost comparing tooasynchronous copy.</span></span> <span data-ttu-id="8affa-215">Hello doporučenému přístupu je tato možnost ve virtuálním počítači Azure, který je v hello toouse stejné oblasti jako vaše zdrojové úložiště účet tooavoid odchozí náklady.</span><span class="sxs-lookup"><span data-stu-id="8affa-215">hello recommended approach is toouse this option in an Azure VM, that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="other-azcopy-features"></a><span data-ttu-id="8affa-216">Další funkce AzCopy</span><span class="sxs-lookup"><span data-stu-id="8affa-216">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a><span data-ttu-id="8affa-217">Kopírovat pouze data, která neexistuje v cílovém umístění hello</span><span class="sxs-lookup"><span data-stu-id="8affa-217">Only copy data that doesn't exist in hello destination</span></span>
<span data-ttu-id="8affa-218">Hello `--exclude-older` a `--exclude-newer` parametry umožňují tooexclude starší nebo novější zdroj prostředky z kopírování, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="8affa-218">hello `--exclude-older` and `--exclude-newer` parameters allow you tooexclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="8affa-219">Pokud chcete pouze prostředky toocopy zdroje, které neexistují v cílovém umístění hello, můžete zadat oba parametry v hello AzCopy příkaz:</span><span class="sxs-lookup"><span data-stu-id="8affa-219">If you only want toocopy source resources that don't exist in hello destination, you can specify both parameters in hello AzCopy command:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-toospecify-command-line-parameters"></a><span data-ttu-id="8affa-220">Používání konfiguraci souboru toospecify parametrů příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="8affa-220">Use a configuration file toospecify command-line parameters</span></span>

```azcopy
azcopy --config-file "azcopy-config.ini"
```

<span data-ttu-id="8affa-221">Parametry příkazového řádku AzCopy můžete zahrnout v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="8affa-221">You can include any AzCopy command-line parameters in a configuration file.</span></span> <span data-ttu-id="8affa-222">AzCopy procesy hello parametry v souboru hello jako v případě, kdyby byly zadány na příkazovém řádku hello, provádění přímé nahrazení s hello obsah souboru hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-222">AzCopy processes hello parameters in hello file as if they had been specified on hello command line, performing a direct substitution with hello contents of hello file.</span></span>

<span data-ttu-id="8affa-223">Předpokládejme, konfigurační soubor s názvem `copyoperation`, který obsahuje následující řádky hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-223">Assume a configuration file named `copyoperation`, that contains hello following lines.</span></span> <span data-ttu-id="8affa-224">Každý parametr AzCopy můžete zadat na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="8affa-224">Each AzCopy parameter can be specified on a single line.</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

<span data-ttu-id="8affa-225">nebo na samostatné řádky:</span><span class="sxs-lookup"><span data-stu-id="8affa-225">or on separate lines:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

<span data-ttu-id="8affa-226">AzCopy selže, pokud rozdělení hello parametr mezi dvěma čárami, jak je vidět tady pro hello `--source-key` parametr:</span><span class="sxs-lookup"><span data-stu-id="8affa-226">AzCopy fails if you split hello parameter across two lines, as shown here for hello `--source-key` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="8affa-227">Zadejte sdílený přístupový podpis (SAS)</span><span class="sxs-lookup"><span data-stu-id="8affa-227">Specify a shared access signature (SAS)</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-sas <SAS1> \
    --dest-sas <SAS2> \
    --include abc.txt
```

<span data-ttu-id="8affa-228">Můžete také zadat SAS na kontejneru hello URI:</span><span class="sxs-lookup"><span data-stu-id="8affa-228">You can also specify a SAS on hello container URI:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

<span data-ttu-id="8affa-229">Všimněte si, že AzCopy aktuálně podporuje jenom hello [SAS účtu](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="8affa-229">Note that AzCopy currently only supports hello [Account SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

### <a name="journal-file-folder"></a><span data-ttu-id="8affa-230">Složky pro soubor deníku</span><span class="sxs-lookup"><span data-stu-id="8affa-230">Journal file folder</span></span>
<span data-ttu-id="8affa-231">Při každém vydání tooAzCopy příkaz zkontroluje existenci souboru deníku ve výchozí složce hello nebo jestli existuje ve složce, kterou jste zadali pomocí této možnosti.</span><span class="sxs-lookup"><span data-stu-id="8affa-231">Each time you issue a command tooAzCopy, it checks whether a journal file exists in hello default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="8affa-232">Pokud soubor deníku hello neexistuje ani na jednom místě, AzCopy zpracovává operaci hello jako nové a generuje nový soubor deníku.</span><span class="sxs-lookup"><span data-stu-id="8affa-232">If hello journal file does not exist in either place, AzCopy treats hello operation as new and generates a new journal file.</span></span>

<span data-ttu-id="8affa-233">Pokud hello deníku soubor neexistuje, AzCopy kontroluje, zda text hello příkazový řádek, který můžete vložit odpovídá hello příkazového řádku v souboru deníku hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-233">If hello journal file does exist, AzCopy checks whether hello command line that you input matches hello command line in hello journal file.</span></span> <span data-ttu-id="8affa-234">Pokud se dva příkazy na příkazových řádcích hello shodují, obnoví AzCopy hello nedokončené operace.</span><span class="sxs-lookup"><span data-stu-id="8affa-234">If hello two command lines match, AzCopy resumes hello incomplete operation.</span></span> <span data-ttu-id="8affa-235">Pokud se neshodují, AzCopy vyzve uživatele tooeither přepsat hello deníku souboru toostart novou operaci, nebo toocancel hello aktuální operace.</span><span class="sxs-lookup"><span data-stu-id="8affa-235">If they do not match, AzCopy prompts user tooeither overwrite hello journal file toostart a new operation, or toocancel hello current operation.</span></span>

<span data-ttu-id="8affa-236">Pokud chcete, aby toouse hello výchozí umístění souboru deníku hello:</span><span class="sxs-lookup"><span data-stu-id="8affa-236">If you want toouse hello default location for hello journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

<span data-ttu-id="8affa-237">Pokud není možnost `--resume`, nebo zadejte možnost `--resume` bez hello cesta ke složce, jako v příkladu nahoře, AzCopy hello deníku soubor vytvoří v hello výchozího umístění, což je `~\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="8affa-237">If you omit option `--resume`, or specify option `--resume` without hello folder path, as shown above, AzCopy creates hello journal file in hello default location, which is `~\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="8affa-238">Pokud hello deníku soubor již existuje, AzCopy obnoví hello operaci na základě souboru deníku hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-238">If hello journal file already exists, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="8affa-239">Pokud chcete, aby toospecify vlastní umístění souboru deníku hello:</span><span class="sxs-lookup"><span data-stu-id="8affa-239">If you want toospecify a custom location for hello journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

<span data-ttu-id="8affa-240">Tento příklad vytvoří soubor deníku hello, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="8affa-240">This example creates hello journal file if it does not already exist.</span></span> <span data-ttu-id="8affa-241">Pokud neexistuje, AzCopy obnoví hello operaci na základě souboru deníku hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-241">If it does exist, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="8affa-242">Pokud chcete tooresume operace AzCopy, opakujte hello stejný příkaz.</span><span class="sxs-lookup"><span data-stu-id="8affa-242">If you want tooresume an AzCopy operation, repeat hello same command.</span></span> <span data-ttu-id="8affa-243">AzCopy v systému Linux potom zobrazí výzvu k potvrzení:</span><span class="sxs-lookup"><span data-stu-id="8affa-243">AzCopy on Linux then will prompt for confirmation:</span></span>

```azcopy
Incomplete operation with same command line detected at hello journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want tooresume hello operation? Choose Yes tooresume, choose No toooverwrite hello journal toostart a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a><span data-ttu-id="8affa-244">Výstupní podrobné protokoly</span><span class="sxs-lookup"><span data-stu-id="8affa-244">Output verbose logs</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a><span data-ttu-id="8affa-245">Zadejte hello počet souběžných operací toostart</span><span class="sxs-lookup"><span data-stu-id="8affa-245">Specify hello number of concurrent operations toostart</span></span>
<span data-ttu-id="8affa-246">Možnost `--parallel-level` určuje hello počet souběžných kopie operací.</span><span class="sxs-lookup"><span data-stu-id="8affa-246">Option `--parallel-level` specifies hello number of concurrent copy operations.</span></span> <span data-ttu-id="8affa-247">Ve výchozím nastavení spustí AzCopy počet propustnost přenosu dat hello tooincrease souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="8affa-247">By default, AzCopy starts a certain number of concurrent operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="8affa-248">Hello počet souběžných operací osm časy hello počet procesorů, které máte.</span><span class="sxs-lookup"><span data-stu-id="8affa-248">hello number of concurrent operations is equal eight times hello number of processors you have.</span></span> <span data-ttu-id="8affa-249">Pokud používáte AzCopy přes síť s malou šířkou pásma, můžete zadat nižší číslo pro – paralelní úrovni tooavoid selhání kvůli konfliktům prostředků.</span><span class="sxs-lookup"><span data-stu-id="8affa-249">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for --parallel-level tooavoid failure caused by resource competition.</span></span>

[!TIP]
><span data-ttu-id="8affa-250">Úplný seznam hello tooview AzCopy parametrů, podívejte se na 'azcopy – Nápověda' nabídky.</span><span class="sxs-lookup"><span data-stu-id="8affa-250">tooview hello complete list of AzCopy parameters, check out 'azcopy --help' menu.</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="8affa-251">Známé problémy a doporučené postupy</span><span class="sxs-lookup"><span data-stu-id="8affa-251">Known issues and best practices</span></span>
### <a name="error-net-core-is-not-found-in-hello-system"></a><span data-ttu-id="8affa-252">Chyba: .NET Core nebyl nalezen v systému hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-252">Error: .NET Core is not found in hello system.</span></span>
<span data-ttu-id="8affa-253">Pokud se vyskytne chyba s oznámením, že .NET Core není nainstalována v systému hello, hello cesta toohello .NET Core binární `dotnet` pravděpodobně chybí.</span><span class="sxs-lookup"><span data-stu-id="8affa-253">If you encounter an error stating that .NET Core is not installed in hello system, hello PATH toohello .NET Core binary `dotnet` may be missing.</span></span>

<span data-ttu-id="8affa-254">V pořadí tooaddress tento problém, najít binární .NET Core hello systému hello:</span><span class="sxs-lookup"><span data-stu-id="8affa-254">In order tooaddress this issue, find hello .NET Core binary in hello system:</span></span>
```bash
sudo find / -name dotnet
```

<span data-ttu-id="8affa-255">Tento příkaz vrátí hello cesta toohello dotnet binární.</span><span class="sxs-lookup"><span data-stu-id="8affa-255">This returns hello path toohello dotnet binary.</span></span> 

    /opt/rh/rh-dotnetcore11/root/usr/bin/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/shared/Microsoft.NETCore.App/1.1.2/dotnet

<span data-ttu-id="8affa-256">Nyní přidejte proměnné PATH toohello této cesty.</span><span class="sxs-lookup"><span data-stu-id="8affa-256">Now add this path toohello PATH variable.</span></span> <span data-ttu-id="8affa-257">Sudo upravte secure_path toocontain hello cesta toohello dotnet binární:</span><span class="sxs-lookup"><span data-stu-id="8affa-257">For sudo, edit secure_path toocontain hello path toohello dotnet binary:</span></span>
```bash 
sudo visudo
### Append hello path found in hello preceding example too'secure_path' variable
```

<span data-ttu-id="8affa-258">V tomto příkladu secure_path proměnná přečte jako:</span><span class="sxs-lookup"><span data-stu-id="8affa-258">In this example, secure_path variable reads as:</span></span>

```
secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/opt/rh/rh-dotnetcore11/root/usr/bin/
```

<span data-ttu-id="8affa-259">Pro aktuálního uživatele hello upravte.bash_profile/.profile tooinclude hello cesta toohello dotnet binární v proměnné PATH</span><span class="sxs-lookup"><span data-stu-id="8affa-259">For hello current user, edit .bash_profile/.profile tooinclude hello path toohello dotnet binary in PATH variable</span></span> 
```bash
vi ~/.bash_profile
### Append hello path found in hello preceding example too'PATH' variable
```

<span data-ttu-id="8affa-260">Ověřte, zda .NET Core je teď v CESTĚ:</span><span class="sxs-lookup"><span data-stu-id="8affa-260">Verify that .NET Core is now in PATH:</span></span>
```bash
which dotnet
sudo which dotnet
```

### <a name="error-installing-azcopy"></a><span data-ttu-id="8affa-261">Chyba při instalaci nástroje AzCopy</span><span class="sxs-lookup"><span data-stu-id="8affa-261">Error Installing AzCopy</span></span>
<span data-ttu-id="8affa-262">Pokud narazíte na potíže s instalací AzCopy, můžete se pokusit toorun AzCopy pomocí skriptu bash hello v hello extrahovat `azcopy` složky.</span><span class="sxs-lookup"><span data-stu-id="8affa-262">If you encounter issues with AzCopy installation, you may try toorun AzCopy using hello bash script in hello extracted `azcopy` folder.</span></span>

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="8affa-263">Limit souběžných zápisy při kopírování dat</span><span class="sxs-lookup"><span data-stu-id="8affa-263">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="8affa-264">Při kopírování objektů BLOB nebo soubory s AzCopy, mějte na paměti, že jiná aplikace může být upravovat hello data během kopírování ho.</span><span class="sxs-lookup"><span data-stu-id="8affa-264">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying hello data while you are copying it.</span></span> <span data-ttu-id="8affa-265">Pokud je to možné Ujistěte se, že hello dat, který chcete kopírovat nemění během operace kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-265">If possible, ensure that hello data you are copying is not being modified during hello copy operation.</span></span> <span data-ttu-id="8affa-266">Například při kopírování virtuálního pevného disku přidružený virtuální počítač Azure, ujistěte se, že žádné další aplikace jsou aktuálně zápis toohello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="8affa-266">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing toohello VHD.</span></span> <span data-ttu-id="8affa-267">Dobře toodo, jedná se o leasing toobe prostředků hello zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="8affa-267">A good way toodo this is by leasing hello resource toobe copied.</span></span> <span data-ttu-id="8affa-268">Alternativně můžete nejprve vytvořte snímek hello virtuální pevný disk a poté zkopírujte hello snímku.</span><span class="sxs-lookup"><span data-stu-id="8affa-268">Alternately, you can create a snapshot of hello VHD first and then copy hello snapshot.</span></span>

<span data-ttu-id="8affa-269">Pokud nelze zabránit jiné aplikace z zápis tooblobs nebo soubory, když jsou kopírovány, pak mějte na paměti, která úlohou hello hello čas dokončení, hello kopírované prostředky buď již nemá úplné parita s prostředky zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-269">If you cannot prevent other applications from writing tooblobs or files while they are being copied, then keep in mind that by hello time hello job finishes, hello copied resources may no longer have full parity with hello source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="8affa-270">Jedna instance nástroje AzCopy spusťte na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="8affa-270">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="8affa-271">AzCopy je navrženou toomaximize hello využití přenosu dat tooaccelerate hello váš počítač prostředků, doporučujeme spustit pouze jedna instance nástroje AzCopy na jednom počítači a zadejte možnost hello `--parallel-level` Pokud potřebujete více souběžných operací.</span><span class="sxs-lookup"><span data-stu-id="8affa-271">AzCopy is designed toomaximize hello utilization of your machine resource tooaccelerate hello data transfer, we recommend you run only one AzCopy instance on one machine, and specify hello option `--parallel-level` if you need more concurrent operations.</span></span> <span data-ttu-id="8affa-272">Další podrobnosti, zadejte `AzCopy --help parallel-level` v příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="8affa-272">For more details, type `AzCopy --help parallel-level` at hello command line.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8affa-273">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8affa-273">Next steps</span></span>
<span data-ttu-id="8affa-274">Další informace o Azure Storage a AzCopy najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="8affa-274">For more information about Azure Storage and AzCopy, see hello following resources:</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="8affa-275">Dokumentace k Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="8affa-275">Azure Storage documentation:</span></span>
* [<span data-ttu-id="8affa-276">Úvod tooAzure úložiště</span><span class="sxs-lookup"><span data-stu-id="8affa-276">Introduction tooAzure Storage</span></span>](storage-introduction.md)
* [<span data-ttu-id="8affa-277">Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="8affa-277">Create a storage account</span></span>](storage-create-storage-account.md)
* [<span data-ttu-id="8affa-278">Spravovat objekty BLOB pomocí Průzkumníka úložiště</span><span class="sxs-lookup"><span data-stu-id="8affa-278">Manage blobs with Storage Explorer</span></span>](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs)
* [<span data-ttu-id="8affa-279">Použití hello 2.0 rozhraní příkazového řádku Azure s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="8affa-279">Using hello Azure CLI 2.0 with Azure Storage</span></span>](storage-azure-cli.md)
* [<span data-ttu-id="8affa-280">Jak toouse úložiště objektů Blob z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="8affa-280">How toouse Blob storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="8affa-281">Jak toouse úložiště Blob z Javy</span><span class="sxs-lookup"><span data-stu-id="8affa-281">How toouse Blob storage from Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="8affa-282">Jak toouse úložiště objektů Blob z Node.js</span><span class="sxs-lookup"><span data-stu-id="8affa-282">How toouse Blob storage from Node.js</span></span>](storage-nodejs-how-to-use-blob-storage.md)
* [<span data-ttu-id="8affa-283">Jak toouse úložiště objektů Blob z Pythonu</span><span class="sxs-lookup"><span data-stu-id="8affa-283">How toouse Blob storage from Python</span></span>](storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="8affa-284">Příspěvky blogu Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="8affa-284">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="8affa-285">Uvedení AzCopy na Linux Preview</span><span class="sxs-lookup"><span data-stu-id="8affa-285">Announcing AzCopy on Linux Preview</span></span>](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [<span data-ttu-id="8affa-286">Představení náhled knihovny přesun dat úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="8affa-286">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="8affa-287">AzCopy: Úvod synchronní kopie a vlastní typ obsahu</span><span class="sxs-lookup"><span data-stu-id="8affa-287">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="8affa-288">AzCopy: Uvedení obecné dostupnosti AzCopy 3.0 a AzCopy 4.0 verze preview s podporou tabulky a souboru</span><span class="sxs-lookup"><span data-stu-id="8affa-288">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="8affa-289">AzCopy: Optimalizovaných pro rozsáhlé kopie scénáře</span><span class="sxs-lookup"><span data-stu-id="8affa-289">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="8affa-290">AzCopy: Podpora pro geograficky redundantní úložiště s přístupem pro čtení</span><span class="sxs-lookup"><span data-stu-id="8affa-290">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="8affa-291">AzCopy: Přenos dat pomocí režimu s možností restartování a tokenu SAS</span><span class="sxs-lookup"><span data-stu-id="8affa-291">AzCopy: Transfer data with restartable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="8affa-292">AzCopy: Objekt Blob kopírování mezi účet pomocí</span><span class="sxs-lookup"><span data-stu-id="8affa-292">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="8affa-293">AzCopy: Nahrávání nebo stahování souborů pro objekty BLOB Azure</span><span class="sxs-lookup"><span data-stu-id="8affa-293">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

