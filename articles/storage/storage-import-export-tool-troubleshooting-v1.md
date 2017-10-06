---
title: "aaaTroubleshooting hello nástroj Azure Import/Export | Microsoft Docs"
description: "Další informace o některých hello běžné problémy, proto při použití hello nástroj Azure Import/Export a jak toohandle je."
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
ms.openlocfilehash: 5445cefe2703edf4d9d285f761433b7b66d8cb6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-azure-importexport-tool"></a><span data-ttu-id="4837b-103">Řešení potíží s hello nástroj Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="4837b-103">Troubleshooting hello Azure Import/Export Tool</span></span>
<span data-ttu-id="4837b-104">Hello nástroj Microsoft Azure Import/Export vrací chybové zprávy, pokud běží na problémy.</span><span class="sxs-lookup"><span data-stu-id="4837b-104">hello Microsoft Azure Import/Export Tool returns error messages if it runs into issues.</span></span> <span data-ttu-id="4837b-105">Toto téma uvádí některé běžné problémy, které mohou uživatelé do.</span><span class="sxs-lookup"><span data-stu-id="4837b-105">This topic lists some common issues that users may run into.</span></span>  
  
## <a name="a-copy-session-fails-what-i-should-do"></a><span data-ttu-id="4837b-106">Relaci kopie selže, co mám udělat?</span><span class="sxs-lookup"><span data-stu-id="4837b-106">A copy session fails, what I should do?</span></span>  
 <span data-ttu-id="4837b-107">Při kopírování relace selže, existují dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="4837b-107">When a copy session fails, there are two options:</span></span>  
  
 <span data-ttu-id="4837b-108">Pokud chyba hello opakovatelná, například pokud hello síťové sdílené složky byla ve stavu offline pro malou období a nyní je zpět do režimu online, můžete obnovit hello kopie relace.</span><span class="sxs-lookup"><span data-stu-id="4837b-108">If hello error is retryable, for example if hello network share was offline for a short period and now is back online, you can resume hello copy session.</span></span> <span data-ttu-id="4837b-109">Pokud hello chyba není opakovatelná, například pokud jste zadali hello nesprávný zdrojový soubor adresář v hello parametry příkazového řádku, musíte tooabort hello kopie relace.</span><span class="sxs-lookup"><span data-stu-id="4837b-109">If hello error is not retryable, for example if you specified hello wrong source file directory in hello command line parameters, you need tooabort hello copy session.</span></span> <span data-ttu-id="4837b-110">V tématu [Příprava pevné disky pro úlohy importu](storage-import-export-tool-preparing-hard-drives-import-v1.md) Další informace o obnovení a přerušení kopírování relací.</span><span class="sxs-lookup"><span data-stu-id="4837b-110">See [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import-v1.md) for more information about resuming and aborting copy sessions.</span></span>  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a><span data-ttu-id="4837b-111">Nelze obnovit nebo k přerušení relace kopírování.</span><span class="sxs-lookup"><span data-stu-id="4837b-111">I can't resume or abort a copy session.</span></span>  
 <span data-ttu-id="4837b-112">Pokud relace kopie hello se hello první relaci kopie pro jednotku, pak by měl stavu hello chybová zpráva: "hello první relaci kopírování se nedá obnovit nebo přerušena."</span><span class="sxs-lookup"><span data-stu-id="4837b-112">If hello copy session is hello first copy session for a drive, then hello error message should state: "hello first copy session cannot be resumed or aborted."</span></span> <span data-ttu-id="4837b-113">V takovém případě můžete odstranit původní soubor deníku hello a znovu spusťte příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="4837b-113">In this case, you can delete hello old journal file and rerun hello command.</span></span>  
  
 <span data-ttu-id="4837b-114">Pokud relaci kopie není hello první z nich pro jednotku, může být vždy obnovení nebo přerušena.</span><span class="sxs-lookup"><span data-stu-id="4837b-114">If a copy session is not hello first one for a drive, it can always be resumed or aborted.</span></span>  
  
## <a name="i-lost-hello-journal-file-can-i-still-create-hello-job"></a><span data-ttu-id="4837b-115">Ztrátou hello deníku souboru, je možné stále vytvořit úlohu hello?</span><span class="sxs-lookup"><span data-stu-id="4837b-115">I lost hello journal file, can I still create hello job?</span></span>  
 <span data-ttu-id="4837b-116">Hello deníku soubor pro jednotku obsahuje veškeré informace o hello kopírování dat toothis jednotky a je potřebné tooadd jednotka toohello další soubory a bude použité toocreate úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="4837b-116">hello journal file for a drive contains hello complete information of copying data toothis drive, and it is needed tooadd more files toohello drive and will be used toocreate an import job.</span></span> <span data-ttu-id="4837b-117">Pokud soubor deníku hello dojde ke ztrátě, bude mít tooredo všechny relace hello kopie pro jednotku hello.</span><span class="sxs-lookup"><span data-stu-id="4837b-117">If hello journal file is lost, you will have tooredo all hello copy sessions for hello drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="4837b-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4837b-118">Next steps</span></span>
 
* [<span data-ttu-id="4837b-119">Nastavení nástroje azure import/export hello</span><span class="sxs-lookup"><span data-stu-id="4837b-119">Setting up hello azure import/export tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="4837b-120">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="4837b-120">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="4837b-121">Kontrola stavu úlohy s použitím kopií souborů protokolu</span><span class="sxs-lookup"><span data-stu-id="4837b-121">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="4837b-122">Oprava úlohy importu</span><span class="sxs-lookup"><span data-stu-id="4837b-122">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="4837b-123">Oprava úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="4837b-123">Repairing an export job</span></span>](storage-import-export-tool-repairing-an-export-job-v1.md)
