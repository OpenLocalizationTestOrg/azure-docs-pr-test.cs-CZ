---
title: "Virtuální počítač restartováním nebo změnou velikosti problémy v Azure | Microsoft Docs"
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
ms.openlocfilehash: e49322dbccf4ec1157bc7e3a109175869b53518d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-linux-vm-in-azure"></a>Řešení potíží s nasazením s restartováním nebo změnou velikosti existující virtuální počítač s Linuxem v Azure
Při pokusu o spuštění virtuálního počítače pro zastavený Azure (VM), nebo přizpůsobit existující virtuální počítač Azure je běžnou chybou, které zaznamenáte chybu přidělení. Tato chyba nastává clusteru nebo oblast buď nemá k dispozici prostředky nebo nemůže podporovat požadovaná velikost virtuálního počítače.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Shromážděte aktivity protokolů
Pokud chcete spustit Poradce při potížích, shromážděte protokoly aktivity k identifikaci chyby související s problém. Následující odkazy obsahují podrobné informace o procesu:

[Zobrazení operací nasazení](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Zobrazit protokoly aktivity ke správě prostředků Azure](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problém: Chyba při spouštění zastaveného virtuálního počítače
Pokoušíte se spustit zastaveného virtuálního počítače, ale získat došlo k chybě přidělení.

### <a name="cause"></a>Příčina
Požadavek na spuštění zastaveného virtuálního počítače musí být pokus v původním clusteru, který je hostitelem cloudové služby. Cluster nemá volné místo dostupné ke splnění tohoto požadavku.

### <a name="resolution"></a>Řešení
* Zastavte všechny virtuální počítače v sadě dostupnosti a znovu spusťte každý virtuální počítač.
  
  1. Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.
  2. Po zastavení všech virtuálních počítačích, vyberte jednotlivé zastaven virtuálních počítačů a klikněte na příkaz spustit.
* Opakujte žádost restartovat později.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problém: Chyba při změně velikosti stávajícího virtuálního počítače
Pokoušíte se změnit velikost existující virtuální počítač ale získat došlo k chybě přidělení.

### <a name="cause"></a>Příčina
Žádost o změně velikosti virtuálního počítače musí být pokus v původním clusteru, který je hostitelem cloudové služby. Cluster však nepodporuje požadovaná velikost virtuálního počítače.

### <a name="resolution"></a>Řešení
* Opakujte tuto žádost pomocí menší velikost virtuálního počítače.
* Pokud velikost požadovaný virtuální počítač nelze změnit:
  
  1. Zastavte všechny virtuální počítače v sadě dostupnosti.
     
     * Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.
  2. Po zastavení všech virtuálních počítačů, změňte velikost požadovaný virtuální počítač na větší velikost.
  3. Vyberte změněnou velikostí virtuálního počítače a klikněte na **spustit**, a následné spuštění všech virtuálních počítačích zastaven.

## <a name="next-steps"></a>Další kroky
Pokud dojde k potížím při vytváření nového virtuálního počítače s Linuxem v Azure, najdete v části [řešení potíží s nasazením s vytvoření nového virtuálního počítače Linux v Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

