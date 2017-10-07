---
title: "aaaHow tootroubleshoot běžné problémy při vytváření virtuálního pevného disku | Microsoft Docs"
description: "Řešení potíží s odpovědi toocommon otázky a problémy při vytváření virtuálního pevného disku."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: 
editor: 
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: e4ff09a979bdf575badff2d33f2299abb17c947d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-common-issues-encountered-during-vhd-creation"></a>Jak běžné problémy tootroubleshoot došlo při vytváření virtuálního pevného disku
Tento článek je zadal toohelp Azure Marketplace vydavatele nebo spolusprávcem, který může zaznamenat problém nebo mít běžné otázky při publikování nebo správu jejich solution(s) virtuálního počítače.

1. Změna hello název hostitele hello
   
    Po vytvoření virtuálního počítače nelze aktualizovat uživatele hello název hostitele hello.
2. Jak tooreset hello služby Vzdálená plocha nebo jeho heslo přihlášení?
   
   * [Referenční informace pro virtuální počítač s Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [Referenční informace pro virtuální počítač s Linuxem](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. Jak toogenerate nové ssh certifikáty?
   
   Naleznete odkaz toohello: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
4. Jak tooconfigure certifikát sítě VPN otevřené?
   
   Naleznete odkaz toohello: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)
5. Co je hello zásady podpory pro spuštění serverového softwaru společnosti Microsoft v prostředí virtuálního počítače Microsoft Azure hello (infrastruktura jako služba)?
   
   Naleznete odkaz toohello: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
6. Mají virtuální počítače žádné jedinečný identifikátor?
   
   Azure kóduje jedinečné ID virtuálního počítače Azure v každé virtuální počítač. Najdete v podrobnostech v tomto blogu a dokumentaci.
7. Do virtuálního počítače, jak lze spravovat rozšíření vlastních skriptů hello hello spuštění úlohy?
   
   Naleznete odkaz toohello: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)
8. Jak toocreate virtuální počítač z Azure pomocí portálu hello hello virtuálního pevného disku, který je odeslán toopremium úložiště?
   
   Tato funkce ještě nepodporujeme.
9. Je podporováno 32bitovou aplikaci v Azure Marketplace hello?
   
   Informace o zásadách podpory hello naleznete odkaz toohello: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
10. Pokaždé, když se pokouším toocreate bitovou kopii z mé virtuálních pevných disků, zobrazí chybová zpráva hello ". Virtuální pevný disk je už registrovaný s úložištěm image jako prostředek hello "v prostředí PowerShell. Nebyl vytvořen žádný obrázek před ani nebyl nalezen žádný obrázek s tímto názvem v Azure. Jak to lze vyřešit?
    
    K tomu obvykle dojít, pokud uživatel hello zřízení virtuálního počítače z tento virtuální pevný disk a na tento virtuální pevný disk je uzamčen. Zkontrolujte, že neexistuje žádný virtuální počítač přidělené z tento virtuální pevný disk. Pokud chyba hello zůstane zachován a pak vyvolejte lístek podpory pomocí tohoto odkazu nebo z hello publikování portál týkající se to (podrobnosti jsou uvedeny v hello odpovědí otázky 11).
