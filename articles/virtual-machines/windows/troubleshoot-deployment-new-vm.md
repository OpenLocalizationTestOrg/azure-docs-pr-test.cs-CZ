---
title: "aaaTroubleshoot nasazení virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Řešení problémů nasazení Resource Manager, když vytvoříte nový virtuální počítač Windows v Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c27d4f63b67db2d1c9f117f05a2eba9ef130ef37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a>Řešení problémů nasazení, při vytváření nového virtuálního počítače Windows v Azure
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Nejdůležitější problémy
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

Pro další problémy při nasazení virtuálního počítače a dotazy, najdete v části [potíží nasazení systému Windows virtuálního počítače v Azure](troubleshoot-deploy-vm.md).

## <a name="collect-activity-logs"></a>Shromážděte aktivity protokolů
řešení potíží, toostart shromažďování hello aktivity zaznamená tooidentify hello chyby související s problémem hello. Hello následující odkazy obsahují podrobné informace o procesu toofollow hello.

[Zobrazení operací nasazení](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Zobrazit protokoly toomanage aktivity Azure prostředky](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**/ Y:** Pokud hello operační systém je Windows zobecněn a je nahrál nebo zachytit pomocí hello zobecněn nastavení, pak nebudou všechny chyby. Podobně pokud hello operačního systému se specializuje Windows a je nahrál nebo zachytit pomocí hello speciální nastavení, pak nebude existovat žádné chyby.

**Odešlete chyby:**

**N<sup>1</sup>:** Pokud hello operačního systému Windows zobecněn, a nahraje jako specializovaný, budete mít zřizování chyba časového limitu s hello virtuálních počítačů zablokované ve úvodní obrazovka při prvním zapnutí.

**N<sup>2</sup>:** Pokud hello operačního systému Windows specializuje a nahraje jako zobecněn, zobrazí se zřizování chyba selhání s hello virtuálních počítačů zablokované ve úvodní obrazovka při prvním zapnutí, protože hello nový virtuální počítač je spuštěn s původní počítač hello název, uživatelské jméno a heslo.

**Řešení**

použít obě tyto chyby tooresolve [přidat AzureRmVhd tooupload hello původní virtuální pevný disk](https://msdn.microsoft.com/library/mt603554.aspx), k dispozici místně, s hello stejné nastavení jako hello operačního systému (zobecněn/specializuje). tooupload jako zobecněn, nezapomeňte nejprve toorun nástroje sysprep.

**Zaznamenat chyby:**

**N<sup>3</sup>:** Pokud hello operačního systému Windows zobecněn, a se zaznamená jako specializovaný, zobrazí se zřizování vypršení časového limitu protože hello původní virtuální počítač je nepoužitelný je označen jako zobecněn.

**N<sup>4</sup>:** Pokud hello operačního systému Windows specializuje a z něj zachytí jako zobecněn, zřizování selhání chyba se zobrazí, protože hello nový virtuální počítač je spuštěn s hello původní název počítače, uživatelské jméno a heslo. Navíc hello původní virtuální počítač je nepoužitelný, protože je označeno jako specializované.

**Řešení**

tooresolve obě tyto chyby odstranit hello aktuální image z portálu hello a [kopii z hello aktuální virtuální pevné disky](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) s hello stejné nastavení jako hello operačního systému (zobecněn/specializuje).

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Problém: Vlastní nebo Galerie/marketplace obrázku; došlo k chybě přidělení
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
Pokud dojde k potížím při spuštění zastaveného virtuálního počítače Windows nebo přizpůsobit existující virtuální počítač Windows v Azure, najdete v části [problémy při nasazení Resource Manager řešení potíží s restartováním nebo změnou velikosti existující virtuální počítač Windows v Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

