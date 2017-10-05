---
title: "Řešení potíží s nástroj Azure Import/Export | Microsoft Docs"
description: "Další informace o některé běžné problémy, proto při použití nástroje Azure Import/Export a postupy pro jejich zpracování."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: b91ca5eb-c557-460a-9afc-0590b38471f9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 7bfda602dbc0ea47828a7c9243b8b9b09ec78432
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-the-azure-importexport-tool"></a><span data-ttu-id="24f84-103">Řešení potíží s nástrojem Azure pro import/export</span><span class="sxs-lookup"><span data-stu-id="24f84-103">Troubleshooting the Azure Import/Export Tool</span></span>
<span data-ttu-id="24f84-104">Nástroj Microsoft Azure Import/Export vrací chybové zprávy, pokud běží na problémy.</span><span class="sxs-lookup"><span data-stu-id="24f84-104">The Microsoft Azure Import/Export Tool returns error messages if it runs into issues.</span></span> <span data-ttu-id="24f84-105">Toto téma uvádí některé běžné problémy, které mohou uživatelé do.</span><span class="sxs-lookup"><span data-stu-id="24f84-105">This topic lists some common issues that users may run into.</span></span>  
  
## <a name="a-copy-session-fails-what-i-should-do"></a><span data-ttu-id="24f84-106">Relaci kopie selže, co mám udělat?</span><span class="sxs-lookup"><span data-stu-id="24f84-106">A copy session fails, what I should do?</span></span>  
 <span data-ttu-id="24f84-107">Při kopírování relace selže, existují dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="24f84-107">When a copy session fails, there are two options:</span></span>  
  
 <span data-ttu-id="24f84-108">Pokud je chyba opakovatelná, například pokud sdílené síťové složce byla offline na krátkou dobu a nyní je zpět do režimu online, můžete obnovit kopii relace.</span><span class="sxs-lookup"><span data-stu-id="24f84-108">If the error is retryable, for example if the network share was offline for a short period and now is back online, you can resume the copy session.</span></span> <span data-ttu-id="24f84-109">Pokud chyba není opakovatelná, například pokud jste zadali nesprávný zdrojový adresář souboru v parametrech příkazového řádku, musíte k přerušení relace kopírování.</span><span class="sxs-lookup"><span data-stu-id="24f84-109">If the error is not retryable, for example if you specified the wrong source file directory in the command line parameters, you need to abort the copy session.</span></span> <span data-ttu-id="24f84-110">V tématu [Příprava pevné disky pro úlohy importu](../storage-import-export-tool-preparing-hard-drives-import-v1.md) Další informace o obnovení a přerušení kopírování relací.</span><span class="sxs-lookup"><span data-stu-id="24f84-110">See [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import-v1.md) for more information about resuming and aborting copy sessions.</span></span>  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a><span data-ttu-id="24f84-111">Nelze obnovit nebo k přerušení relace kopírování.</span><span class="sxs-lookup"><span data-stu-id="24f84-111">I can't resume or abort a copy session.</span></span>  
 <span data-ttu-id="24f84-112">Pokud relace kopírování se má první relace kopie pro jednotku, pak by měl stavu chybová zpráva: "má první relace kopírování se nedá obnovit nebo přerušena."</span><span class="sxs-lookup"><span data-stu-id="24f84-112">If the copy session is the first copy session for a drive, then the error message should state: "The first copy session cannot be resumed or aborted."</span></span> <span data-ttu-id="24f84-113">V takovém případě můžete odstranit původní soubor deníku a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="24f84-113">In this case, you can delete the old journal file and rerun the command.</span></span>  
  
 <span data-ttu-id="24f84-114">Pokud relaci kopie není první z nich pro jednotku, může být vždy obnovení nebo přerušena.</span><span class="sxs-lookup"><span data-stu-id="24f84-114">If a copy session is not the first one for a drive, it can always be resumed or aborted.</span></span>  
  
## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a><span data-ttu-id="24f84-115">I ztráty deník souboru, je možné stále vytvořit úlohu?</span><span class="sxs-lookup"><span data-stu-id="24f84-115">I lost the journal file, can I still create the job?</span></span>  
 <span data-ttu-id="24f84-116">Soubor deníku pro jednotku obsahuje kompletní informace o kopírování dat na tuto jednotku, a je potřeba přidat další soubory na disk a budou použity k vytvoření úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="24f84-116">The journal file for a drive contains the complete information of copying data to this drive, and it is needed to add more files to the drive and will be used to create an import job.</span></span> <span data-ttu-id="24f84-117">Pokud soubor deníku dojde ke ztrátě, budete muset znovu provést všechny kopie relace pro jednotku.</span><span class="sxs-lookup"><span data-stu-id="24f84-117">If the journal file is lost, you will have to redo all the copy sessions for the drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="24f84-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24f84-118">Next steps</span></span>
 
* [<span data-ttu-id="24f84-119">Nastavení nástroje azure import/export</span><span class="sxs-lookup"><span data-stu-id="24f84-119">Setting up the azure import/export tool</span></span>](../storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="24f84-120">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="24f84-120">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="24f84-121">Kontrola stavu úlohy s použitím kopií souborů protokolu</span><span class="sxs-lookup"><span data-stu-id="24f84-121">Reviewing job status with copy log files</span></span>](../storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="24f84-122">Oprava úlohy importu</span><span class="sxs-lookup"><span data-stu-id="24f84-122">Repairing an import job</span></span>](../storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="24f84-123">Oprava úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="24f84-123">Repairing an export job</span></span>](../storage-import-export-tool-repairing-an-export-job-v1.md)
