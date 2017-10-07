---
title: "obnovení aaaDiagnostics a chyby pro úlohy Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak služba tooenable podrobné protokolování pro Microsoft Azure Import/Export úloh."
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
ms.openlocfilehash: 48164279e7904c78fed802aa3cff66e589c3f12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a><span data-ttu-id="c0004-103">Diagnostiky a zotavení po chybě pro úlohy Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="c0004-103">Diagnostics and error recovery for Azure Import/Export jobs</span></span>
<span data-ttu-id="c0004-104">Pro každou jednotku zpracovat vytvoří hello služba Azure Import/Export protokol chyb v hello přidruženého účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c0004-104">For each drive processed, hello Azure Import/Export service creates an error log in hello associated storage account.</span></span> <span data-ttu-id="c0004-105">Můžete také povolit podrobné protokolování tak nastavení hello `LogLevel` vlastnost příliš`Verbose` při volání metody hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) nebo [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace.</span><span class="sxs-lookup"><span data-stu-id="c0004-105">You can also enable verbose logging by setting hello `LogLevel` property too`Verbose` when calling hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operations.</span></span>

 <span data-ttu-id="c0004-106">Ve výchozím nastavení, protokoly zapisují tooa kontejner s názvem `waimportexport`.</span><span class="sxs-lookup"><span data-stu-id="c0004-106">By default, logs are written tooa container named `waimportexport`.</span></span> <span data-ttu-id="c0004-107">Můžete zadat jiný název nastavení hello `DiagnosticsPath` vlastnost při volání metody hello `Put Job` nebo `Update Job Properties` operace.</span><span class="sxs-lookup"><span data-stu-id="c0004-107">You can specify a different name by setting hello `DiagnosticsPath` property when calling hello `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="c0004-108">Hello protokoly se ukládají jako objekty BLOB bloku s hello následující zásady vytváření názvů: `waies/jobname_driveid_timestamp_logtype.xml`.</span><span class="sxs-lookup"><span data-stu-id="c0004-108">hello logs are stored as block blobs with hello following naming convention: `waies/jobname_driveid_timestamp_logtype.xml`.</span></span>

 <span data-ttu-id="c0004-109">Můžete načíst hello URI hello protokoly pro úlohu pomocí volání hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operaci.</span><span class="sxs-lookup"><span data-stu-id="c0004-109">You can retrieve hello URI of hello logs for a job by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="c0004-110">Hello identifikátor URI pro hello podrobného protokolování se vrátí v hello `VerboseLogUri` vlastnosti pro každou jednotku, zatímco hello identifikátor URI pro protokol chyb hello je vrácený v hello `ErrorLogUri` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c0004-110">hello URI for hello verbose log is returned in hello `VerboseLogUri` property for each drive, while hello URI for hello error log is returned in hello `ErrorLogUri` property.</span></span>

<span data-ttu-id="c0004-111">Můžete použít hello protokolování dat tooidentify hello následující problémy.</span><span class="sxs-lookup"><span data-stu-id="c0004-111">You can use hello logging data tooidentify hello following issues.</span></span>

## <a name="drive-errors"></a><span data-ttu-id="c0004-112">Jednotka chyby</span><span class="sxs-lookup"><span data-stu-id="c0004-112">Drive errors</span></span>

<span data-ttu-id="c0004-113">Hello následující položky jsou klasifikovány jako chyby jednotky:</span><span class="sxs-lookup"><span data-stu-id="c0004-113">hello following items are classified as drive errors:</span></span>

-   <span data-ttu-id="c0004-114">Soubor manifestu chyby v přístupu ke službám nebo čtení hello</span><span class="sxs-lookup"><span data-stu-id="c0004-114">Errors in accessing or reading hello manifest file</span></span>

-   <span data-ttu-id="c0004-115">Nesprávný klíče nástroje BitLocker</span><span class="sxs-lookup"><span data-stu-id="c0004-115">Incorrect BitLocker keys</span></span>

-   <span data-ttu-id="c0004-116">Jednotky pro čtení a zápis chyby</span><span class="sxs-lookup"><span data-stu-id="c0004-116">Drive read/write errors</span></span>

## <a name="blob-errors"></a><span data-ttu-id="c0004-117">Objekt BLOB chyby</span><span class="sxs-lookup"><span data-stu-id="c0004-117">Blob errors</span></span>

<span data-ttu-id="c0004-118">Hello následující položky jsou klasifikovány jako chyby objektů blob:</span><span class="sxs-lookup"><span data-stu-id="c0004-118">hello following items are classified as blob errors:</span></span>

-   <span data-ttu-id="c0004-119">Objekt blob nesprávnou či konfliktní nebo názvy</span><span class="sxs-lookup"><span data-stu-id="c0004-119">Incorrect or conflicting blob or names</span></span>

-   <span data-ttu-id="c0004-120">Chybějící soubory</span><span class="sxs-lookup"><span data-stu-id="c0004-120">Missing files</span></span>

-   <span data-ttu-id="c0004-121">Nebyl nalezen objekt BLOB</span><span class="sxs-lookup"><span data-stu-id="c0004-121">Blob not found</span></span>

-   <span data-ttu-id="c0004-122">Zkrácený soubory (hello soubory na disku hello jsou menší, než je zadáno v manifestu hello)</span><span class="sxs-lookup"><span data-stu-id="c0004-122">Truncated files (hello files on hello disk are smaller than specified in hello manifest)</span></span>

-   <span data-ttu-id="c0004-123">Poškozený soubor obsahu (pro úlohy importu, zjistila se neshoda MD5 kontrolního součtu)</span><span class="sxs-lookup"><span data-stu-id="c0004-123">Corrupted file content (for import jobs, detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="c0004-124">Soubory metadat a vlastnost poškozená blob (zjistila se neshoda MD5 kontrolního součtu)</span><span class="sxs-lookup"><span data-stu-id="c0004-124">Corrupted blob metadata and property files (detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="c0004-125">Nesprávné schéma pro vlastnosti objektů blob hello a soubory metadat</span><span class="sxs-lookup"><span data-stu-id="c0004-125">Incorrect schema for hello blob properties and/or metadata files</span></span>

<span data-ttu-id="c0004-126">Můžou nastat případy, kdy některé části úlohu import nebo export není úspěšně dokončil, zatímco hello celkové stále dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="c0004-126">There may be cases where some parts of an import or export job do not complete successfully, while hello overall job still completes.</span></span> <span data-ttu-id="c0004-127">V takovém případě můžete buď odeslání nebo stažení hello chybí částí hello dat přes síť, nebo můžete vytvořit nové data hello tootransfer úlohy.</span><span class="sxs-lookup"><span data-stu-id="c0004-127">In this case, you can either upload or download hello missing pieces of hello data over network, or you can create a new job tootransfer hello data.</span></span> <span data-ttu-id="c0004-128">V tématu hello [Azure Import/Export nástroj odkaz](storage-import-export-tool-how-to-v1.md) toolearn jak toorepair hello dat přes síť.</span><span class="sxs-lookup"><span data-stu-id="c0004-128">See hello [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) toolearn how toorepair hello data over network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0004-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0004-129">Next steps</span></span>

* [<span data-ttu-id="c0004-130">Pomocí REST API služby importu a exportu hello</span><span class="sxs-lookup"><span data-stu-id="c0004-130">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
