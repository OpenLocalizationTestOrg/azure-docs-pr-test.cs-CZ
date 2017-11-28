---
title: "aaaRun Azure Batch úlohy začátku do konce bez nutnosti psaní kódu (Preview) | Microsoft Docs"
description: "Vytvoření šablony souborů pro hello rozhraní příkazového řádku Azure toocreate Batch fondy, úlohy a úkoly."
services: batch
author: mscurrell
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: na
ms.topic: article
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: markscu
ms.openlocfilehash: c0434d09766451f6ba516efbad949834711ee819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a><span data-ttu-id="ec94e-103">Použití šablon rozhraní příkazového řádku služby Batch a přenos souborů (Preview)</span><span class="sxs-lookup"><span data-stu-id="ec94e-103">Use Azure Batch CLI Templates and File Transfer (Preview)</span></span>

<span data-ttu-id="ec94e-104">Pomocí hello rozhraní příkazového řádku Azure je možné toorun dávkových úloh bez nutnosti psaní kódu.</span><span class="sxs-lookup"><span data-stu-id="ec94e-104">Using hello Azure CLI it is possible toorun Batch jobs without writing code.</span></span>

<span data-ttu-id="ec94e-105">Soubory šablon můžete vytvořit a použít s hello rozhraní příkazového řádku Azure, umožňující fondy, úlohy a úlohy toobe vytvořit dávky.</span><span class="sxs-lookup"><span data-stu-id="ec94e-105">Template files can be created and used with hello Azure CLI that allow Batch pools, jobs, and tasks toobe created.</span></span> <span data-ttu-id="ec94e-106">Vstupní soubory úlohy můžete snadno nahrán do účtu úložiště hello přidruženého k hello dávkového účtu a úlohy výstupní soubory stáhnout.</span><span class="sxs-lookup"><span data-stu-id="ec94e-106">Job input files can be easily uploaded to hello storage account associated with hello Batch account and job output files downloaded.</span></span>

## <a name="overview"></a><span data-ttu-id="ec94e-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="ec94e-107">Overview</span></span>

<span data-ttu-id="ec94e-108">Rozšíření toohello rozhraní příkazového řádku Azure umožňuje Batch toobe používá k kompletní uživatelé, kteří nejsou vývojáři.</span><span class="sxs-lookup"><span data-stu-id="ec94e-108">An extension toohello Azure CLI enables Batch toobe used end-to-end by users who are not developers.</span></span> <span data-ttu-id="ec94e-109">Fond lze vytvořit, nahrán vstupních dat, úlohy a přidružených úloh vytvoření a hello výsledný výstup stažených dat – žádný kód potřeby hello rozhraní příkazového řádku používá přímo, nebo se integrovaný do skriptů.</span><span class="sxs-lookup"><span data-stu-id="ec94e-109">A pool can be created, input data uploaded, jobs and associated tasks created, and hello resulting output data downloaded – no code required, hello CLI being used directly or being integrated into scripts.</span></span>

<span data-ttu-id="ec94e-110">Vytvoření šablony batch ve hello [existující podporu Batch v hello rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) umožňuje soubory JSON toospecify hodnoty vlastností pro vytvoření hello fondy, úlohy, úlohy a další položky.</span><span class="sxs-lookup"><span data-stu-id="ec94e-110">Batch templates build on hello [existing Batch support in hello Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) that allows JSON files toospecify property values for hello creation of pools, jobs, tasks, and other items.</span></span> <span data-ttu-id="ec94e-111">S šablonami Batch hello následující funkce byly přidány prostřednictvím co je možné pomocí soubory JSON hello:</span><span class="sxs-lookup"><span data-stu-id="ec94e-111">With Batch templates, hello following capabilities are added over what is possible with hello JSON files:</span></span>

-   <span data-ttu-id="ec94e-112">Parametry lze definovat.</span><span class="sxs-lookup"><span data-stu-id="ec94e-112">Parameters can be defined.</span></span> <span data-ttu-id="ec94e-113">Když se použije šablona hello, jsou pouze hodnoty parametrů hello zadaný toocreate hello položku se dalších hodnot vlastností položky specifikované v textu hello šablony.</span><span class="sxs-lookup"><span data-stu-id="ec94e-113">When hello template is used, only hello parameter values are specified toocreate hello item, with other item property values being specified in hello template body.</span></span> <span data-ttu-id="ec94e-114">Uživatel, který jste srozuměni s tím, že Batch a toobe aplikace spouštěné dávky můžete vytvořit šablony, fond, úlohy a hodnoty vlastností úlohy.</span><span class="sxs-lookup"><span data-stu-id="ec94e-114">A user who understands Batch and the applications toobe run by Batch can create templates, specifying pool, job, and task property values.</span></span> <span data-ttu-id="ec94e-115">Uživatel méně neznáte Batch nebo aplikace jenom potřebuje toospecify hello hodnoty pro parametry definované hello.</span><span class="sxs-lookup"><span data-stu-id="ec94e-115">A user less familiar with Batch and/or the applications only needs toospecify hello values for hello defined parameters.</span></span>

-   <span data-ttu-id="ec94e-116">Vytvořit objekty pro vytváření úloh úlohy nebo další úkoly spojené s úlohou, zabraňující hello nutné pro mnoho toobe definice úloh vytvoření a výrazně zjednodušení odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="ec94e-116">Job task factories create one or more tasks associated with a job, avoiding hello need for many task definitions toobe created and significantly simplifying job submission.</span></span>


<span data-ttu-id="ec94e-117">Vstupní datové soubory potřebovat toobe zadaná pro úlohy a často vytváří výstupní datové soubory.</span><span class="sxs-lookup"><span data-stu-id="ec94e-117">Input data files need toobe supplied for jobs and output data files are often produced.</span></span> <span data-ttu-id="ec94e-118">Je přidružený účet úložiště, ve výchozím nastavení, ke každé dávky účet a soubory lze snadno přenášená tooand z tohoto účtu úložiště pomocí rozhraní příkazového řádku bez kódování a vyžadují přihlašovací údaje žádné úložiště.</span><span class="sxs-lookup"><span data-stu-id="ec94e-118">A storage account is associated, by default, with each Batch account and files can be easily transferred tooand from this storage account using the CLI, with no coding and no storage credentials required.</span></span>

<span data-ttu-id="ec94e-119">Například [ffmpeg](http://ffmpeg.org/) je Oblíbené aplikace, která zpracovává soubory audia a videa.</span><span class="sxs-lookup"><span data-stu-id="ec94e-119">For example, [ffmpeg](http://ffmpeg.org/) is a popular application that processes audio and video files.</span></span> <span data-ttu-id="ec94e-120">Hello rozhraní příkazového řádku Azure Batch se dá použít tooinvoke ffmpeg tootranscode zdroj video soubory toodifferent řešení.</span><span class="sxs-lookup"><span data-stu-id="ec94e-120">hello Azure Batch CLI can be used tooinvoke ffmpeg tootranscode source video files toodifferent resolutions.</span></span>

-   <span data-ttu-id="ec94e-121">Je-li vytvořit šablonu fondu.</span><span class="sxs-lookup"><span data-stu-id="ec94e-121">A pool template is created.</span></span> <span data-ttu-id="ec94e-122">uživatel Hello vytváření šablony hello zná, jak toocall hello ffmpeg aplikace a požadavky na jeho; Určí hello odpovídající operačního systému, počítač velikost, jak ffmpeg je nainstalovaná (z balíčku aplikace nebo pomocí Správce balíčků, například) a dalších fond hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="ec94e-122">hello user creating hello template knows how toocall hello ffmpeg application and its requirements; they specify hello appropriate OS, VM size, how ffmpeg is installed (from an application package or using a package manager, for example), and other pool property values.</span></span> <span data-ttu-id="ec94e-123">Parametry jsou vytvořeny, takže pokud se použije šablona hello, pouze id fondu hello a počet virtuálních počítačů musí být toobe zadán.</span><span class="sxs-lookup"><span data-stu-id="ec94e-123">Parameters are created so when hello template is used, only hello pool id and number of VMs need toobe specified.</span></span>

-   <span data-ttu-id="ec94e-124">Je-li vytvořit šablonu úlohy.</span><span class="sxs-lookup"><span data-stu-id="ec94e-124">A job template is created.</span></span> <span data-ttu-id="ec94e-125">Šablona vytváření hello Hello uživatel zná jak ffmpeg potřebám toobe vyvolány tootranscode zdroj videa tooa jiné rozlišení a určuje příkazový řádek úkolu hello; také věděli, že je složka obsahující hello zdrojové video soubory, se úloha vyžaduje pro vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="ec94e-125">hello user creating hello template knows how ffmpeg needs toobe invoked tootranscode source video tooa different resolution and specifies hello task command line; they also know that there is a folder containing hello source video files, with a task required per input file.</span></span>

-   <span data-ttu-id="ec94e-126">Koncový uživatel se sadou tootranscode video soubory nejprve vytvoří fond pomocí šablony hello fondu, zadáte jenom id fondu hello a počet virtuálních počítačů, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="ec94e-126">An end user with a set of video files tootranscode first creates a pool using hello pool template, specifying only hello pool id and number of VMs required.</span></span> <span data-ttu-id="ec94e-127">Potom můžete nahrát hello zdrojové soubory tootranscode.</span><span class="sxs-lookup"><span data-stu-id="ec94e-127">They can then upload hello source files tootranscode.</span></span> <span data-ttu-id="ec94e-128">Úlohy lze pak odeslat pomocí šablony úlohy hello, zadáte jenom id fondu hello a umístění zdrojových souborů hello nahrát.</span><span class="sxs-lookup"><span data-stu-id="ec94e-128">A job can then be submitted using hello job template, specifying only hello pool id and location of hello source files uploaded.</span></span> <span data-ttu-id="ec94e-129">jeden úkol za vstupní soubor generován se vytvoří Hello dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="ec94e-129">hello Batch job is created, with one task per input file being generated.</span></span> <span data-ttu-id="ec94e-130">Nakonec hello převodu výstupní soubory lze stáhnout.</span><span class="sxs-lookup"><span data-stu-id="ec94e-130">Finally, hello transcoded output files can be download.</span></span>

## <a name="installation"></a><span data-ttu-id="ec94e-131">Instalace</span><span class="sxs-lookup"><span data-stu-id="ec94e-131">Installation</span></span>

<span data-ttu-id="ec94e-132">Možnosti Hello šablonu a soubor přenosu vyžadují rozšíření toobe nainstalována.</span><span class="sxs-lookup"><span data-stu-id="ec94e-132">hello template and file transfer capabilities require an extension toobe installed.</span></span>

<span data-ttu-id="ec94e-133">Pokyny, jak zjistit, tooinstall hello rozhraní příkazového řádku Azure [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ec94e-133">For instructions on how tooinstall hello Azure CLI see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="ec94e-134">Jednou hello byla nainstalována rozhraní příkazového řádku Azure, hello Batch rozšíření můžete nainstalovat pomocí rozhraní příkazového řádku následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ec94e-134">Once hello Azure CLI has been installed, hello Batch extension can be installed using the following CLI command:</span></span>

```azurecli
az component update --add batch-extensions --allow-third-party
```

<span data-ttu-id="ec94e-135">Další informace o hello Batch rozšíření najdete v tématu [Microsoft Azure Batch CLI rozšíření pro Windows, Mac a Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).</span><span class="sxs-lookup"><span data-stu-id="ec94e-135">For more information about hello Batch extension, see [Microsoft Azure Batch CLI Extensions for Windows, Mac and Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).</span></span>

## <a name="templates"></a><span data-ttu-id="ec94e-136">Šablony</span><span class="sxs-lookup"><span data-stu-id="ec94e-136">Templates</span></span>

<span data-ttu-id="ec94e-137">Hello rozhraní příkazového řádku Azure Batch umožňuje položek, jako jsou fondy, úlohy a úlohy toobe vytvořit zadáním soubor JSON obsahující názvy a hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="ec94e-137">hello Azure Batch CLI allows items such as pools, jobs, and tasks toobe created by specifying a JSON file containing property names and values.</span></span> <span data-ttu-id="ec94e-138">Například:</span><span class="sxs-lookup"><span data-stu-id="ec94e-138">For example:</span></span>

```azurecli
az batch pool create –-json-file AppPool.json
```

<span data-ttu-id="ec94e-139">Šablony Azure Batch jsou podobné tooAzure šablony Resource Manageru, v syntaxi a funkce.</span><span class="sxs-lookup"><span data-stu-id="ec94e-139">Azure Batch templates are similar tooAzure Resource Manager templates, in functionality and syntax.</span></span> <span data-ttu-id="ec94e-140">Jsou soubory JSON, které obsahují položky názvy a hodnoty vlastností, ale přidat hello následující hlavní koncepty:</span><span class="sxs-lookup"><span data-stu-id="ec94e-140">They are JSON files that contain item property names and values, but add hello following main concepts:</span></span>

-   <span data-ttu-id="ec94e-141">**Parametry**</span><span class="sxs-lookup"><span data-stu-id="ec94e-141">**Parameters**</span></span>

    -   <span data-ttu-id="ec94e-142">Povolí vlastnost toobe hodnoty zadané v části textu, s pouze hodnoty parametrů toobe zadán, pokud je hello šablony, která potřebuje.</span><span class="sxs-lookup"><span data-stu-id="ec94e-142">Allow property values toobe specified in a body section, with only parameter values needing toobe supplied when hello template is used.</span></span> <span data-ttu-id="ec94e-143">Například hello dokončení definice pro fond může být umístěny v textu hello a jenom jeden parametr definovaný pro id fondu; pouze řetězec id fondu proto musí toocreate toobe zadaný fond.</span><span class="sxs-lookup"><span data-stu-id="ec94e-143">For example, hello complete definition for a pool could be placed in hello body and only one parameter defined for pool id; only a pool id string therefore needs toobe supplied toocreate a pool.</span></span>
        
    -   <span data-ttu-id="ec94e-144">Šablona textu Hello můžete vytvořené uživatelem s znalosti Batch a toobe aplikace hello spusťte dávkou; Pokud je hello šablony je nutné zadat pouze hodnoty pro parametry definované Autor hello.</span><span class="sxs-lookup"><span data-stu-id="ec94e-144">hello template body can be authored by someone with knowledge of Batch and hello applications toobe run by Batch; only values for hello author-defined parameters must be supplied when hello template is used.</span></span> <span data-ttu-id="ec94e-145">Uživatel bez hello podrobný Batch nebo znalostní bázi aplikace lze tedy použít šablony.</span><span class="sxs-lookup"><span data-stu-id="ec94e-145">A user without hello in-depth Batch and/or application knowledge can therefore use the templates.</span></span>

-   <span data-ttu-id="ec94e-146">**Proměnné**</span><span class="sxs-lookup"><span data-stu-id="ec94e-146">**Variables**</span></span>

    -   <span data-ttu-id="ec94e-147">Povolit parametr jednoduché nebo komplexní hodnoty toobe zadaný v jednom místě a použít na jeden nebo více místech v textu hello šablony.</span><span class="sxs-lookup"><span data-stu-id="ec94e-147">Allow simple or complex parameter values toobe specified in one place and used in one or more places in hello template body.</span></span> <span data-ttu-id="ec94e-148">Proměnné můžete zjednodušit a zmenšit velikost hello hello šablony, a také zkontrolujte více udržovatelný tak, že jeden vlastnosti toochange umístění, jehož hodnota může změnit.</span><span class="sxs-lookup"><span data-stu-id="ec94e-148">Variables can simplify and reduce hello size of hello template, as well as make it more maintainable by having one location toochange properties whose value may change.</span></span>

-   <span data-ttu-id="ec94e-149">**Konstrukce vyšší úrovně**</span><span class="sxs-lookup"><span data-stu-id="ec94e-149">**Higher-level constructs**</span></span>

    -   <span data-ttu-id="ec94e-150">Některé konstrukce vyšší úrovně jsou k dispozici v hello šablony, které ještě nejsou k dispozici v hello rozhraní API služby Batch.</span><span class="sxs-lookup"><span data-stu-id="ec94e-150">Some higher-level constructs are available in hello template that are not yet available in hello Batch APIs.</span></span> <span data-ttu-id="ec94e-151">Například objekt pro vytváření úloh lze definovat v šabloně úlohy, která vytvoří více úloh pro úlohu hello pomocí společná definice úlohy.</span><span class="sxs-lookup"><span data-stu-id="ec94e-151">For example, a task factory can be defined in a job template that creates multiple tasks for hello job using a common task definition.</span></span> <span data-ttu-id="ec94e-152">Tyto konstrukce brání hello nutné toocode dynamicky vytvořte několik souborů JSON, jako je například jeden soubor pro úlohy, a také soubory tooinstall aplikací prostřednictvím Správce balíčků, například vytvořit skript.</span><span class="sxs-lookup"><span data-stu-id="ec94e-152">These constructs avoid hello need toocode to dynamically create multiple JSON files, such as one file per task, as well as create script files tooinstall applications via a package manager, for example.</span></span>

    -   <span data-ttu-id="ec94e-153">V některých bodu, kde použít, že může být tyto konstrukce přidat toothe Batch služby a k dispozici v hello rozhraní API služby Batch, uživatelská atd.</span><span class="sxs-lookup"><span data-stu-id="ec94e-153">At some point, where applicable, these constructs may be added toothe Batch service and available in hello Batch APIs, UIs, etc.</span></span>

### <a name="pool-templates"></a><span data-ttu-id="ec94e-154">Šablony fondu</span><span class="sxs-lookup"><span data-stu-id="ec94e-154">Pool templates</span></span>

<span data-ttu-id="ec94e-155">Kromě toho hello fondu šablony podporuje možnosti standardní šablona toohello parametry a proměnné, hello následující konstrukce vyšší úrovně:</span><span class="sxs-lookup"><span data-stu-id="ec94e-155">In addition toohello standard template capabilities of parameters and variables, hello following higher-level constructs are supported by hello pool template:</span></span>

-   <span data-ttu-id="ec94e-156">**Odkazy na balíček**</span><span class="sxs-lookup"><span data-stu-id="ec94e-156">**Package references**</span></span>

    -   <span data-ttu-id="ec94e-157">Volitelně umožňuje uzly toopool toobe zkopírovat softwaru pomocí balíčku správci.</span><span class="sxs-lookup"><span data-stu-id="ec94e-157">Optionally allows software toobe copied toopool nodes by using package managers.</span></span> <span data-ttu-id="ec94e-158">nejsou zadány Hello package manager a id balíčku.</span><span class="sxs-lookup"><span data-stu-id="ec94e-158">hello package manager and package id are specified.</span></span> <span data-ttu-id="ec94e-159">Je možné toodeclare jeden nebo více balíčků zabraňuje hello nutné toocreate skript, který získá hello požadované balíčky, nainstalujete hello skript a spusťte skript hello na všech uzlech fondu.</span><span class="sxs-lookup"><span data-stu-id="ec94e-159">Being able toodeclare one or more packages avoids hello need toocreate a script that gets hello required packages, install hello script, and run hello script on each pool node.</span></span>

<span data-ttu-id="ec94e-160">Následující Hello je příkladem šablonu, která vytvoří fond virtuálních počítačů Linux s ffmpeg nainstalovaná a pouze vyžaduje fondu id řetězec a hello počet toouse toobe zadaný virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="ec94e-160">hello following is an example of a template that creates a pool of Linux VMs with ffmpeg installed and only requires a pool id string and hello number of VMs toobe supplied toouse:</span></span>

```json
{
    "parameters": {
        "nodeCount": {
            "type": "int",
            "metadata": {
                "description": "hello number of pool nodes"
            }
        },
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "hello pool id "
            }
        }
    },
    "pool": {
        "type": "Microsoft.Batch/batchAccounts/pools",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('poolId')]",
            "virtualMachineConfiguration": {
                "imageReference": {
                    "publisher": "Canonical",
                    "offer": "UbuntuServer",
                    "sku": "16.04.0-LTS",
                    "version": "latest"
                },
                "nodeAgentSKUId": "batch.node.ubuntu 16.04"
            },
            "vmSize": "STANDARD_D3_V2",
            "targetDedicatedNodes": "[parameters('nodeCount')]",
            "enableAutoScale": false,
            "maxTasksPerNode": 1,
            "packageReferences": [
                {
                    "type": "aptPackage",
                    "id": "ffmpeg"
                }
            ]
        }
    }
}
```

<span data-ttu-id="ec94e-161">Pokud se název souboru šablony hello _fondu ffmpeg.json_, pak hello šablony by být volána následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ec94e-161">If hello template file was named _pool-ffmpeg.json_, then hello template would be invoked as follows:</span></span>

```azurecli
az batch pool create --template pool-ffmpeg.json
```

### <a name="job-templates"></a><span data-ttu-id="ec94e-162">Šablony úloh</span><span class="sxs-lookup"><span data-stu-id="ec94e-162">Job templates</span></span>

<span data-ttu-id="ec94e-163">Kromě toho šablony úlohy hello podporuje možnosti standardní šablona toohello parametry a proměnné, hello následující konstrukce vyšší úrovně:</span><span class="sxs-lookup"><span data-stu-id="ec94e-163">In addition toohello standard template capabilities of parameters and variables, hello following higher-level constructs are supported by hello job template:</span></span>

-   <span data-ttu-id="ec94e-164">**Objekt pro vytváření úloh**</span><span class="sxs-lookup"><span data-stu-id="ec94e-164">**Task factory**</span></span>

    -   <span data-ttu-id="ec94e-165">Vytvoří více úkolů pro úlohu z definice jeden úkol.</span><span class="sxs-lookup"><span data-stu-id="ec94e-165">Creates multiple tasks for a job from one task definition.</span></span> <span data-ttu-id="ec94e-166">Tři typy objektu pro vytváření úloh jsou podporovány – čištění oblouku úloh na soubor a kolekce úloh.</span><span class="sxs-lookup"><span data-stu-id="ec94e-166">Three types of task factory are supported – parametric sweep, task per file, and task collection.</span></span>

<span data-ttu-id="ec94e-167">Hello následuje příklad šablony, která vytvoří úlohu, která používá ffmpeg k tooone video soubory MP4 převod z dvě nižší rozlišení, s vytvořenými pro jednotlivé zdroje videosoubor úloh:</span><span class="sxs-lookup"><span data-stu-id="ec94e-167">hello following is an example of a template that creates a job that uses ffmpeg to transcode MP4 video files tooone of two lower resolutions, with one task created per source video file:</span></span>

```json
{
    "parameters": {
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "hello name of Azure Batch pool which runs hello job"
            }
        },
        "jobId": {
            "type": "string",
            "metadata": {
                "description": "hello name of Azure Batch job"
            }
        },
        "resolution": {
            "type": "string",
            "defaultValue": "428x240",
            "allowedValues": [
                "428x240",
                "854x480"
            ],
            "metadata": {
                "description": "Target video resolution"
            }
        }
    },
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('jobId')]",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "[parameters('poolId')]"
            },
            "taskFactory": {
                "type": "taskPerFile",
                "source": { 
                    "fileGroup": "ffmpeg-input"
                },
                "repeatTask": {
                    "commandLine": "ffmpeg -i {fileName} -y -s [parameters('resolution')] -strict -2 {fileNameWithoutExtension}_[parameters('resolution')].mp4",
                    "resourceFiles": [
                        {
                            "blobSource": "{url}",
                            "filePath": "{fileName}"
                        }
                    ],
                    "outputFiles": [
                        {
                            "filePattern": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                            "destination": {
                                "autoStorage": {
                                    "path": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                                    "fileGroup": "ffmpeg-output"
                                }
                            },
                            "uploadOptions": {
                                "uploadCondition": "TaskSuccess"
                            }
                        }
                    ]
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```

<span data-ttu-id="ec94e-168">Pokud se název souboru šablony hello _úlohy ffmpeg.json_, pak hello šablony by být volána následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ec94e-168">If hello template file was named _job-ffmpeg.json_, then hello template would be invoked as follows:</span></span>

```azurecli
az batch job create --template job-ffmpeg.json
```

## <a name="file-groups-and-file-transfer"></a><span data-ttu-id="ec94e-169">Skupiny souborů a přenos souborů</span><span class="sxs-lookup"><span data-stu-id="ec94e-169">File groups and file transfer</span></span>

<span data-ttu-id="ec94e-170">Většina úloh a úloh vyžadují vstupní soubory a vytvoří výstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="ec94e-170">Most jobs and tasks require input files and produce output files.</span></span> <span data-ttu-id="ec94e-171">Obě vstupní soubory a výstupní soubory obvykle nutné toobe přenést z uzlu toohello hello klienta nebo z klienta toohello hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="ec94e-171">Both input files and output files typically need toobe transferred, either from hello client toohello node, or from hello node toohello client.</span></span> <span data-ttu-id="ec94e-172">Hello rozšíření rozhraní příkazového řádku Azure Batch abstrahuje přenos nepřítomnosti souborů a využívá hello účet úložiště, který se vytvoří ve výchozím nastavení pro každý účet Batch.</span><span class="sxs-lookup"><span data-stu-id="ec94e-172">hello Azure Batch CLI extension abstracts away file transfer and utilizes hello storage account that is created by default for each Batch account.</span></span>

<span data-ttu-id="ec94e-173">Skupinu souborů znamená zároveň tooa kontejneru, který je vytvořen v hello účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ec94e-173">A file group equates tooa container that is created in hello Azure storage account.</span></span> <span data-ttu-id="ec94e-174">Skupina souborů Hello může mít podsložky.</span><span class="sxs-lookup"><span data-stu-id="ec94e-174">hello file group may have subfolders.</span></span>

<span data-ttu-id="ec94e-175">Hello rozšíření rozhraní příkazového řádku Batch poskytuje příkazy pro odesílání souborů ze skupiny klientů tooa zadaný soubor a stahování souborů z hello zadaný soubor skupiny tooa klienta.</span><span class="sxs-lookup"><span data-stu-id="ec94e-175">hello Batch CLI extension provides commands for uploading files from client tooa specified file group and downloading files from hello specified file group tooa client.</span></span>

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

<span data-ttu-id="ec94e-176">Fond a úlohy šablony povolí soubory uložené v souboru skupiny toobe zadané pro kopírování na uzly fondu nebo vypnout fond uzlů zpět tooa skupiny souborů.</span><span class="sxs-lookup"><span data-stu-id="ec94e-176">Pool and job templates allow files stored in file groups toobe specified for copy onto pool nodes or off pool nodes back tooa file group.</span></span> <span data-ttu-id="ec94e-177">Například v šabloně úlohy zadali dřív hello souboru skupiny "ffmpeg – vstup" je zadán pro vytváření úloh hello jako hello umístění hello zdrojových souborů video zkopírovat dolů na ni hello uzel pro překódování; Hello souboru skupiny "ffmpeg-output" se používá jako hello umístění, kde se hello převodu výstupní soubory zkopírovat toofrom hello uzlu se systémem každý úkol.</span><span class="sxs-lookup"><span data-stu-id="ec94e-177">For example, in the job template specified previously, hello file group “ffmpeg-input” is specified for hello task factory as hello location of hello source video files copied down onto hello node for transcoding; hello file group “ffmpeg-output” is used as hello location where hello transcoded output files are copied toofrom hello node running each task.</span></span>

## <a name="summary"></a><span data-ttu-id="ec94e-178">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ec94e-178">Summary</span></span>

<span data-ttu-id="ec94e-179">Podpora přenos šablonu a soubor aktuálně byly přidány pouze toohello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="ec94e-179">Template and file transfer support have currently been added only toohello Azure CLI.</span></span> <span data-ttu-id="ec94e-180">cílem Hello je cílová skupina hello tooexpand, který můžete použít toousers Batch, který není nutné toodevelop kódu pomocí hello rozhraní API služby Batch, například výzkumných pracovníků, IT uživatelé a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ec94e-180">hello goal is tooexpand hello audience that can use Batch toousers who do not need toodevelop code using hello Batch APIs, such as researchers, IT users, and so on.</span></span> <span data-ttu-id="ec94e-181">Bez kódování, uživatelům se znalostí Azure Batch a hello toobe aplikace spouštěné dávky můžete vytvořit šablony pro vytvoření fondu a úlohy.</span><span class="sxs-lookup"><span data-stu-id="ec94e-181">Without coding, users with knowledge of Azure, Batch, and hello applications toobe run by Batch can create templates for pool and job creation.</span></span> <span data-ttu-id="ec94e-182">Parametry šablony uživatelé bez podrobné údaje o Batch a hello aplikace pomocí šablony hello.</span><span class="sxs-lookup"><span data-stu-id="ec94e-182">With template parameters, users without detailed knowledge of Batch and hello applications can use hello templates.</span></span>

<span data-ttu-id="ec94e-183">Vyzkoušet hello Batch rozšíření pro hello rozhraní příkazového řádku Azure a poskytnout nám žádné zpětnou vazbu nebo návrhy, buď v hello komentáře k tomuto článku nebo prostřednictvím hello [fórum Azure Batch](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).</span><span class="sxs-lookup"><span data-stu-id="ec94e-183">Try out hello Batch extension for hello Azure CLI and provide us with any feedback or suggestions, either in hello comments for this article or via hello [Azure Batch forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec94e-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec94e-184">Next steps</span></span>

- <span data-ttu-id="ec94e-185">Najdete v příspěvku blogu pro šablony hello Batch: [hello úloh systémem Azure Batch pomocí rozhraní příkazového řádku Azure – vyžaduje žádný kód](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).</span><span class="sxs-lookup"><span data-stu-id="ec94e-185">See hello Batch templates blog post: [Running Azure Batch jobs using hello Azure CLI – no code required](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).</span></span>
- <span data-ttu-id="ec94e-186">Podrobnou dokumentaci instalaci a použití, ukázky a zdrojového kódu jsou k dispozici v hello [úložiště Azure GitHub](https://github.com/Azure/azure-batch-cli-extensions).</span><span class="sxs-lookup"><span data-stu-id="ec94e-186">Detailed installation and usage documentation, samples, and source code are available in hello [Azure GitHub repository](https://github.com/Azure/azure-batch-cli-extensions).</span></span>
