---
title: "Kontrola stavu úlohy Azure Import/Export - v1 | Microsoft Docs"
description: "Naučte se používat soubory protokolů vytvořené při spuštění úlohy import nebo export a zjistit stav úlohy importu a exportu."
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
ms.openlocfilehash: 621e41df127fded6ec6fe1f71e86cb8630965a70
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a><span data-ttu-id="ced64-103">Kontrola stavu úlohy Azure Import/Export s kopírovat soubory protokolu</span><span class="sxs-lookup"><span data-stu-id="ced64-103">Reviewing Azure Import/Export job status with copy log files</span></span>
<span data-ttu-id="ced64-104">Při zpracovávání službou Microsoft Azure Import/Export jednotky, které jsou přidružené úloze importu nebo exportu, zapíše kopírovat soubory protokolů pro účet úložiště, který nebo ze kterého se import nebo export objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="ced64-104">When the Microsoft Azure Import/Export service processes drives associated with an import or export job, it writes copy log files to the storage account to or from which you are importing or exporting blobs.</span></span> <span data-ttu-id="ced64-105">Soubor protokolu obsahuje podrobné informace o stavu o každý soubor, který byl importovat nebo exportovat.</span><span class="sxs-lookup"><span data-stu-id="ced64-105">The log file contains detailed status about each file that was imported or exported.</span></span> <span data-ttu-id="ced64-106">Adresa URL ke každému souboru protokolu kopie je vrácena, pokud dotaz na stav dokončené úlohy; v tématu [Get Job](/rest/api/storageservices/Get-Job3) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ced64-106">The URL to each copy log file is returned when you query the status of a completed job; see [Get Job](/rest/api/storageservices/Get-Job3) for more information.</span></span>  

## <a name="example-urls"></a><span data-ttu-id="ced64-107">Příklad adresy URL</span><span class="sxs-lookup"><span data-stu-id="ced64-107">Example URLs</span></span>

<span data-ttu-id="ced64-108">Následuje příklad adresy URL pro kopírování souborů protokolu pro úlohy importu se dvě jednotky:</span><span class="sxs-lookup"><span data-stu-id="ced64-108">The following are example URLs for copy log files for an import job with two drives:</span></span>  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 <span data-ttu-id="ced64-109">V tématu [službu Import/Export formát souboru protokolu](storage-import-export-file-format-log.md) pro formát kopírování protokolů a úplný seznam stavových kódů.</span><span class="sxs-lookup"><span data-stu-id="ced64-109">See [Import/Export service Log File Format](storage-import-export-file-format-log.md) for the format of copy logs and the full list of status codes.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="ced64-110">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ced64-110">Next steps</span></span>
 
 * [<span data-ttu-id="ced64-111">Nastavení nástroje Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="ced64-111">Setting Up the Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
 * [<span data-ttu-id="ced64-112">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="ced64-112">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [<span data-ttu-id="ced64-113">Oprava úlohy importu</span><span class="sxs-lookup"><span data-stu-id="ced64-113">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [<span data-ttu-id="ced64-114">Oprava úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="ced64-114">Repairing an export job</span></span>](storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [<span data-ttu-id="ced64-115">Řešení potíží s nástrojem Azure pro import/export</span><span class="sxs-lookup"><span data-stu-id="ced64-115">Troubleshooting the Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
