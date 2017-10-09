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
ms.openlocfilehash: 254439c15797862dded5d80028b8780ad163b2b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-azure-importexport-tool"></a>Řešení potíží s hello nástroj Azure Import/Export
Hello nástroj Microsoft Azure Import/Export vrací chybové zprávy, pokud běží na problémy. Toto téma uvádí některé běžné problémy, které mohou uživatelé do.  
  
## <a name="a-copy-session-fails-what-i-should-do"></a>Relaci kopie selže, co mám udělat?  
 Při kopírování relace selže, existují dvě možnosti:  
  
 Pokud chyba hello opakovatelná, například pokud hello síťové sdílené složky byla ve stavu offline pro malou období a nyní je zpět do režimu online, můžete obnovit hello kopie relace. Pokud hello chyba není opakovatelná, například pokud jste zadali hello nesprávný zdrojový soubor adresář v hello parametry příkazového řádku, musíte tooabort hello kopie relace. V tématu [Příprava pevné disky pro úlohy importu](../storage-import-export-tool-preparing-hard-drives-import-v1.md) Další informace o obnovení a přerušení kopírování relací.  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a>Nelze obnovit nebo k přerušení relace kopírování.  
 Pokud relace kopie hello se hello první relaci kopie pro jednotku, pak by měl stavu hello chybová zpráva: "hello první relaci kopírování se nedá obnovit nebo přerušena." V takovém případě můžete odstranit původní soubor deníku hello a znovu spusťte příkaz hello.  
  
 Pokud relaci kopie není hello první z nich pro jednotku, může být vždy obnovení nebo přerušena.  
  
## <a name="i-lost-hello-journal-file-can-i-still-create-hello-job"></a>Ztrátou hello deníku souboru, je možné stále vytvořit úlohu hello?  
 Hello deníku soubor pro jednotku obsahuje veškeré informace o hello kopírování dat toothis jednotky a je potřebné tooadd jednotka toohello další soubory a bude použité toocreate úlohy importu. Pokud soubor deníku hello dojde ke ztrátě, bude mít tooredo všechny relace hello kopie pro jednotku hello.  
  
## <a name="next-steps"></a>Další kroky
 
* [Nastavení nástroje azure import/export hello](../storage-import-export-tool-setup-v1.md)   
* [Příprava pevných disků pro úlohu importu](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kontrola stavu úlohy s použitím kopií souborů protokolu](../storage-import-export-tool-reviewing-job-status-v1.md)   
* [Oprava úlohy importu](../storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Oprava úlohy exportu](../storage-import-export-tool-repairing-an-export-job-v1.md)
