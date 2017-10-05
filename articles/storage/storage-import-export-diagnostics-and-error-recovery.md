---
title: "Diagnostiky a zotavení po chybě pro úlohy Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak povolit podrobné protokolování pro úlohy služby Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 096cc795-9af6-4335-9fe8-fffa9f239a17
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 0068aae9d6780aa41a070db0eb191d0d5a165d21
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a><span data-ttu-id="c6407-103">Diagnostiky a zotavení po chybě pro úlohy Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="c6407-103">Diagnostics and error recovery for Azure Import/Export jobs</span></span>
<span data-ttu-id="c6407-104">Pro každou jednotku zpracovat vytvoří služba Azure Import/Export protokol chyb v přidruženého účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c6407-104">For each drive processed, the Azure Import/Export service creates an error log in the associated storage account.</span></span> <span data-ttu-id="c6407-105">Můžete také povolit podrobné protokolování nastavením `LogLevel` vlastnost `Verbose` při volání metody [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) nebo [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace.</span><span class="sxs-lookup"><span data-stu-id="c6407-105">You can also enable verbose logging by setting the `LogLevel` property to `Verbose` when calling the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operations.</span></span>

 <span data-ttu-id="c6407-106">Ve výchozím nastavení se protokoly zapisují do kontejner s názvem `waimportexport`.</span><span class="sxs-lookup"><span data-stu-id="c6407-106">By default, logs are written to a container named `waimportexport`.</span></span> <span data-ttu-id="c6407-107">Můžete zadat jiný název nastavení `DiagnosticsPath` vlastnost při volání metody `Put Job` nebo `Update Job Properties` operace.</span><span class="sxs-lookup"><span data-stu-id="c6407-107">You can specify a different name by setting the `DiagnosticsPath` property when calling the `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="c6407-108">Protokoly se ukládají jako objekty BLOB bloku se podle následující konvence: `waies/jobname_driveid_timestamp_logtype.xml`.</span><span class="sxs-lookup"><span data-stu-id="c6407-108">The logs are stored as block blobs with the following naming convention: `waies/jobname_driveid_timestamp_logtype.xml`.</span></span>

 <span data-ttu-id="c6407-109">Identifikátor URI protokoly pro úlohu můžete načíst pomocí volání [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operaci.</span><span class="sxs-lookup"><span data-stu-id="c6407-109">You can retrieve the URI of the logs for a job by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="c6407-110">Identifikátor URI pro podrobného protokolování se vrátí v `VerboseLogUri` vlastnosti pro každou jednotku, když je identifikátor URI v protokolu chyb vrácený v `ErrorLogUri` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c6407-110">The URI for the verbose log is returned in the `VerboseLogUri` property for each drive, while the URI for the error log is returned in the `ErrorLogUri` property.</span></span>

<span data-ttu-id="c6407-111">Data protokolování můžete použít k identifikaci následující problémy.</span><span class="sxs-lookup"><span data-stu-id="c6407-111">You can use the logging data to identify the following issues.</span></span>

## <a name="drive-errors"></a><span data-ttu-id="c6407-112">Jednotka chyby</span><span class="sxs-lookup"><span data-stu-id="c6407-112">Drive errors</span></span>

<span data-ttu-id="c6407-113">Následující položky jsou klasifikovány jako chyby jednotky:</span><span class="sxs-lookup"><span data-stu-id="c6407-113">The following items are classified as drive errors:</span></span>

-   <span data-ttu-id="c6407-114">Chyby v přístupu ke službám nebo načítání souboru manifestu</span><span class="sxs-lookup"><span data-stu-id="c6407-114">Errors in accessing or reading the manifest file</span></span>

-   <span data-ttu-id="c6407-115">Nesprávný klíče nástroje BitLocker</span><span class="sxs-lookup"><span data-stu-id="c6407-115">Incorrect BitLocker keys</span></span>

-   <span data-ttu-id="c6407-116">Jednotky pro čtení a zápis chyby</span><span class="sxs-lookup"><span data-stu-id="c6407-116">Drive read/write errors</span></span>

## <a name="blob-errors"></a><span data-ttu-id="c6407-117">Objekt BLOB chyby</span><span class="sxs-lookup"><span data-stu-id="c6407-117">Blob errors</span></span>

<span data-ttu-id="c6407-118">Následující položky jsou klasifikovány jako chyby objektů blob:</span><span class="sxs-lookup"><span data-stu-id="c6407-118">The following items are classified as blob errors:</span></span>

-   <span data-ttu-id="c6407-119">Objekt blob nesprávnou či konfliktní nebo názvy</span><span class="sxs-lookup"><span data-stu-id="c6407-119">Incorrect or conflicting blob or names</span></span>

-   <span data-ttu-id="c6407-120">Chybějící soubory</span><span class="sxs-lookup"><span data-stu-id="c6407-120">Missing files</span></span>

-   <span data-ttu-id="c6407-121">Nebyl nalezen objekt BLOB</span><span class="sxs-lookup"><span data-stu-id="c6407-121">Blob not found</span></span>

-   <span data-ttu-id="c6407-122">Zkrácený soubory (soubory na disku je menší, než je zadáno v manifestu)</span><span class="sxs-lookup"><span data-stu-id="c6407-122">Truncated files (the files on the disk are smaller than specified in the manifest)</span></span>

-   <span data-ttu-id="c6407-123">Poškozený soubor obsahu (pro úlohy importu, zjistila se neshoda MD5 kontrolního součtu)</span><span class="sxs-lookup"><span data-stu-id="c6407-123">Corrupted file content (for import jobs, detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="c6407-124">Soubory metadat a vlastnost poškozená blob (zjistila se neshoda MD5 kontrolního součtu)</span><span class="sxs-lookup"><span data-stu-id="c6407-124">Corrupted blob metadata and property files (detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="c6407-125">Nesprávné schéma pro vlastnosti objektů blob a soubory metadat</span><span class="sxs-lookup"><span data-stu-id="c6407-125">Incorrect schema for the blob properties and/or metadata files</span></span>

<span data-ttu-id="c6407-126">Můžou nastat případy, kde některé části úlohu import nebo export není úspěšně dokončil, zatímco stále dokončení celkové úlohy.</span><span class="sxs-lookup"><span data-stu-id="c6407-126">There may be cases where some parts of an import or export job do not complete successfully, while the overall job still completes.</span></span> <span data-ttu-id="c6407-127">V takovém případě můžete buď odeslání nebo stažení části chybějí dat přes síť, nebo můžete vytvořit novou úlohu k přenosu dat.</span><span class="sxs-lookup"><span data-stu-id="c6407-127">In this case, you can either upload or download the missing pieces of the data over network, or you can create a new job to transfer the data.</span></span> <span data-ttu-id="c6407-128">Najdete v článku [Azure Import/Export nástroj odkaz](storage-import-export-tool-how-to-v1.md) se dozvíte, jak opravit data přes síť.</span><span class="sxs-lookup"><span data-stu-id="c6407-128">See the [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) to learn how to repair the data over network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6407-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6407-129">Next steps</span></span>

* [<span data-ttu-id="c6407-130">Pomocí REST API služby importu a exportu</span><span class="sxs-lookup"><span data-stu-id="c6407-130">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
