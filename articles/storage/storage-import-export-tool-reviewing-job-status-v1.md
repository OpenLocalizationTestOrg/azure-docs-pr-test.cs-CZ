---
title: "aaaReviewing stav úlohy Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak toouse hello protokolu soubory vytvořené při hello importovat nebo exportovat úloha byla spuštěna toosee hello stav úlohy importu a exportu hello."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: c69d1d69-6403-4eee-9949-0185faeecfa1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.openlocfilehash: 378bc9a7ef6bfe65209413c8c4134f313a2c0d79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a><span data-ttu-id="1f7f3-103">Kontrola stavu úlohy Azure Import/Export s kopírovat soubory protokolu</span><span class="sxs-lookup"><span data-stu-id="1f7f3-103">Reviewing Azure Import/Export job status with copy log files</span></span>
<span data-ttu-id="1f7f3-104">Když hello služby Microsoft Azure Import/Export zpracovává jednotky, které jsou přidružené úloze importu nebo exportu, zapíše kopie protokolu soubory toohello úložiště účet tooor ze kterého jsou import nebo export objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="1f7f3-104">When hello Microsoft Azure Import/Export service processes drives associated with an import or export job, it writes copy log files toohello storage account tooor from which you are importing or exporting blobs.</span></span> <span data-ttu-id="1f7f3-105">Hello protokolový soubor obsahuje podrobné informace o stavu o každý soubor, který byl importovat nebo exportovat.</span><span class="sxs-lookup"><span data-stu-id="1f7f3-105">hello log file contains detailed status about each file that was imported or exported.</span></span> <span data-ttu-id="1f7f3-106">soubor protokolu kopie tooeach URL Hello je vrácena, pokud dotaz hello stav dokončené úlohy; v tématu [Get Job](/rest/api/storageservices/Get-Job3) Další informace.</span><span class="sxs-lookup"><span data-stu-id="1f7f3-106">hello URL tooeach copy log file is returned when you query hello status of a completed job; see [Get Job](/rest/api/storageservices/Get-Job3) for more information.</span></span>  

## <a name="example-urls"></a><span data-ttu-id="1f7f3-107">Příklad adresy URL</span><span class="sxs-lookup"><span data-stu-id="1f7f3-107">Example URLs</span></span>

<span data-ttu-id="1f7f3-108">Příklad adresy URL pro kopírování souborů protokolu pro úlohy importu se dvěma disky se Hello následující:</span><span class="sxs-lookup"><span data-stu-id="1f7f3-108">hello following are example URLs for copy log files for an import job with two drives:</span></span>  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 <span data-ttu-id="1f7f3-109">V tématu [službu Import/Export formát souboru protokolu](storage-import-export-file-format-log.md) pro formát hello kopírování protokolů a hello úplný seznam stavových kódů.</span><span class="sxs-lookup"><span data-stu-id="1f7f3-109">See [Import/Export service Log File Format](storage-import-export-file-format-log.md) for hello format of copy logs and hello full list of status codes.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="1f7f3-110">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f7f3-110">Next steps</span></span>
 
 * [<span data-ttu-id="1f7f3-111">Hello nastavení se nástroj Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="1f7f3-111">Setting Up hello Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
 * [<span data-ttu-id="1f7f3-112">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="1f7f3-112">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [<span data-ttu-id="1f7f3-113">Oprava úlohy importu</span><span class="sxs-lookup"><span data-stu-id="1f7f3-113">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [<span data-ttu-id="1f7f3-114">Oprava úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="1f7f3-114">Repairing an export job</span></span>](storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [<span data-ttu-id="1f7f3-115">Řešení potíží s hello nástroj Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="1f7f3-115">Troubleshooting hello Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
