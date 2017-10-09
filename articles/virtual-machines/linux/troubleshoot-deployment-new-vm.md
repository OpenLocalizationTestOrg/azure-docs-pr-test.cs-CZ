---
title: "virtuální počítač s Linuxem nasazení RM aaaTroubleshoot | Microsoft Docs"
description: "Řešení problémů nasazení Resource Manager, když vytvoříte nový virtuální počítač Linux v Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: 2dd7f1855bba75d86eb90f88e6d573cd42fd8d87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Řešení problémů nasazení Resource Manager pomocí vytvoření nového virtuálního počítače Linux v Azure
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Nejdůležitější problémy
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Pro další problémy při nasazení virtuálního počítače a dotazy, najdete v části [potíží nasazení Linux virtuálního počítače v Azure](troubleshoot-deploy-vm.md).
## <a name="collect-activity-logs"></a>Shromážděte aktivity protokolů
řešení potíží, toostart shromažďování hello aktivity zaznamená tooidentify hello chyby související s problémem hello. Hello následující odkazy obsahují podrobné informace o procesu toofollow hello.

[Zobrazení operací nasazení](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Zobrazit protokoly toomanage aktivity Azure prostředky](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**/ Y:** Pokud hello operačního systému Linux zobecněn a se nahrál nebo zachytit pomocí hello zobecněn nastavení, potom nebudou všechny chyby. Podobně pokud hello operačního systému Linux specializuje a je nahrál nebo zachytit pomocí hello speciální nastavení, pak nebude existovat žádné chyby.

**Odešlete chyby:**

**N<sup>1</sup>:** Pokud hello operačního systému Linux zobecněn, a nahraje jako specializovaný, zobrazí se zřizování vypršení časového limitu protože hello virtuálních počítačů zasekl ve fázi zřizování hello.

**N<sup>2</sup>:** Pokud hello operačního systému Linux specializuje a nahraje jako zobecněn, zřizování selhání chyba se zobrazí, protože hello nový virtuální počítač je spuštěn s hello původní název počítače, uživatelské jméno a heslo.

**Řešení:**

tooresolve obě tyto chyby, odeslat hello původní virtuální pevný disk, k dispozici místní, s hello stejné nastavení jako hello operačního systému (zobecněn/specializuje). tooupload jako zobecněn, mějte na paměti, toorun-nejprve zrušit jejich zřízení.

**Zaznamenat chyby:**

**N<sup>3</sup>:** Pokud hello operačního systému Linux zobecněn, a se zaznamená jako specializovaný, zobrazí se zřizování vypršení časového limitu protože hello původní virtuální počítač je nepoužitelný je označen jako zobecněn.

**N<sup>4</sup>:** Pokud hello operačního systému Linux specializuje a z něj zachytí jako zobecněn, zřizování selhání chyba se zobrazí, protože hello nový virtuální počítač je spuštěn s hello původní název počítače, uživatelské jméno a heslo. Navíc hello původní virtuální počítač je nepoužitelný, protože je označeno jako specializované.

**Řešení:**

tooresolve obě tyto chyby odstranit hello aktuální image z portálu hello a [kopii z hello aktuální virtuální pevné disky](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) s hello stejné nastavení jako hello operačního systému (zobecněn/specializuje).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problém: Vlastní nebo Galerie / marketplace obrázku; došlo k chybě přidělení
Tato chyba nastane v situacích, když hello novou žádost o virtuální počítač je definovaného tooa cluster, který nepodporuje požadovanou velikost virtuálního počítače hello, nebo nemá požadavek hello tooaccommodate dostupné volné místo.

**Příčina 1:** hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače.

**Řešení 1:**

* Opakujte žádost hello pomocí menší velikost virtuálního počítače.
* Pokud hello požadovaná velikost hello že virtuálních počítačů nelze změnit:
  * Zastavte všechny virtuální počítače hello v sadě dostupnosti hello.
    Klikněte na tlačítko **skupiny prostředků** > *vaší skupiny prostředků* > **prostředky** > *vaše skupina dostupnosti* > **virtuální počítače** > *virtuálního počítače* > **Zastavit**.
  * Po všech hello zastavit virtuální počítače, vytvořte hello nový virtuální počítač v hello potřeby velikost.
  * Spusťte nejprve hello nový virtuální počítač a potom vyberete jednotlivé hello zastavena virtuální počítače a klikněte na tlačítko **spustit**.

**Příčina 2:** hello cluster nebude mít uvolnění prostředků.

**Řešení 2:**

* Opakujte žádost hello později.
* Pokud můžete technologie hello nový virtuální počítač součástí jiné dostupnosti nastavit
  * Vytvoření nového virtuálního počítače v sadě dostupnosti jinou (v hello stejné oblasti).
  * Přidat nový virtuální počítač toohello hello stejné virtuální síti.

## <a name="next-steps"></a>Další kroky
Pokud dojde k potížím při spuštění zastavený virtuální počítač s Linuxem nebo změnit jeho velikost existující virtuální počítač s Linuxem v Azure, najdete v části [problémy při nasazení Resource Manager řešení potíží s restartováním nebo změnou velikosti existující virtuální počítač Linux v Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

