---
title: "aaaVM restartováním nebo změnou velikosti problémy v Azure | Microsoft Docs"
description: "Řešení problémů nasazení Resource Manager s restartováním nebo změnou velikosti existující virtuální počítač Linux v Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 51f1610c-0290-464a-97d4-c2e485c7ae7f
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.workload: required
ms.date: 01/10/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d9ff76b64b6646b04565eb93110759c4ebc858e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-linux-vm-in-azure"></a>Řešení potíží s nasazením s restartováním nebo změnou velikosti existující virtuální počítač s Linuxem v Azure
Zkuste toostart ukončeno virtuálního počítače (virtuální počítač Azure) nebo přizpůsobit existující virtuální počítač Azure, hello běžnou chybou, které zaznamenáte při došlo k chybě přidělení. Tato chyba nastává hello clusteru nebo oblast buď nemá k dispozici prostředky nebo nelze podporu hello požadovaná velikost virtuálního počítače.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Shromážděte aktivity protokolů
řešení potíží, toostart shromažďování hello aktivity zaznamená tooidentify hello chyby související s problémem hello. Hello následující odkazy obsahují podrobné informace o procesu hello:

[Zobrazení operací nasazení](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Zobrazit protokoly toomanage aktivity Azure prostředky](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problém: Chyba při spouštění zastaveného virtuálního počítače
Zkuste toostart zastaveného virtuálního počítače, ale získat došlo k chybě přidělení.

### <a name="cause"></a>Příčina
žádost Hello toostart hello zastavena virtuálních počítačů má toobe na hello původní cluster, který je hostitelem hello cloudové služby. Hello cluster však nebude mít volné místo k dispozici toofulfill hello požadavku.

### <a name="resolution"></a>Řešení
* Zastavte všechny hello virtuálních počítačů v hello dostupnosti nastavení a pak restartujte každý virtuální počítač.
  
  1. Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.
  2. Po všech hello zastavení virtuálních počítačů, vyberte jednotlivé virtuální počítače hello zastavena a klikněte na příkaz spustit.
* Opakujte žádost restartování hello později.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problém: Chyba při změně velikosti stávajícího virtuálního počítače
Zkuste tooresize existující virtuální počítač ale získat došlo k chybě přidělení.

### <a name="cause"></a>Příčina
Hello tooresize hello virtuálních počítačů má toobe pokusem o zpracování požadavku v původním clusteru hello dané hostitelů hello cloudové služby. Ale hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače.

### <a name="resolution"></a>Řešení
* Opakujte žádost hello pomocí menší velikost virtuálního počítače.
* Pokud hello požadovaná velikost hello že virtuálních počítačů nelze změnit:
  
  1. Zastavte všechny virtuální počítače hello v sadě dostupnosti hello.
     
     * Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.
  2. Po všech hello zastavení virtuálních počítačů, změna velikosti větší velikost tooa hello potřeby virtuálního počítače.
  3. Vyberte hello po změně velikosti virtuálních počítačů a klikněte na tlačítko **spustit**, a pak spusťte každý hello zastavena virtuálních počítačů.

## <a name="next-steps"></a>Další kroky
Pokud dojde k potížím při vytváření nového virtuálního počítače s Linuxem v Azure, najdete v části [řešení potíží s nasazením s vytvoření nového virtuálního počítače Linux v Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

