---
title: "problémů s nasazením cloudové služby aaaTroubleshoot | Microsoft Docs"
description: "Existuje několik běžných problémů, které může spustit do při nasazování tooAzure cloudové služby. Tento článek obsahuje řešení toosome z nich."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: a18ae415-0d1c-4bc4-ab6c-c1ddea02c870
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: 15aea4f2b2913d95f3378b2e9762b232531f3c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-service-deployment-problems"></a>Cloudové služby řešení problémů s nasazením
Když nasadíte tooAzure balíčku aplikace služby cloudu, můžete získat informace o nasazení hello od hello **vlastnosti** podokně hello portálu Azure. Můžete použít hello podrobnosti v této toohelp podokně řešení problémů s hello cloudové služby a tato informace tooAzure podpory můžete zadat při otevření nové žádosti o podporu.

Můžete najít hello **vlastnosti** podokně následujícím způsobem:

* V hello portálu Azure, klikněte na tlačítko hello nasazení cloudové služby, klikněte na **všechna nastavení**a potom klikněte na **vlastnosti**.
* V hello portál Azure classic, klikněte na tlačítko hello nasazení cloudové služby, klikněte na **řídicí panel**, v pravém dolním rohu hello hello stránky (v části **rychlého přehledu**). Upozorňujeme, že neexistuje žádný štítek "Vlastnosti" v tomto podokně.

> [!NOTE]
> Můžete kopírovat obsah hello hello **vlastnosti** schránky toohello podokně kliknutím na ikonu hello v pravém horním rohu hello hello podokna.
>
>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Problém: Nelze získat přístup k webu, ale my nasazení je spuštěna a všechny instance rolí jsou připravené
odkaz adresu URL webu Hello uvedené na portálu hello nezahrnuje hello portu. Hello výchozí port pro weby je 80. Pokud je vaše aplikace nakonfigurovaná toorun v jiný port, musíte přidat hello správný port číslo toohello adresy URL při přístupu k webu hello.

1. V hello portálu Azure klikněte na tlačítko hello nasazení cloudové služby.
2. V hello **vlastnosti** podokně hello portál Azure, zkontrolujte hello porty pro instance rolí hello (v části **vstupní koncové body**).
3. Pokud není hello portu 80, přidejte hello správný port hodnotu toohello URL při přístupu k aplikaci hello. toospecify jiný než výchozí port, zadejte adresu URL hello, následovaným dvojtečkou (:), za nímž následuje hello číslo portu, bez mezer.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Problém: Instance Moje rolí recyklované bez mi jakékoli akce
Služba opravy probíhá automaticky, když zjistí problém uzlů Azure a proto přesune uzly toonew instancí role. Když k tomu dojde, může se zobrazit instance role recyklace automaticky. toofind se v případě služby opravy k chybě:

1. V hello portálu Azure klikněte na tlačítko hello nasazení cloudové služby.
2. V hello **vlastnosti** podokně hello portál Azure, zkontrolujte informace hello a zjistit, zda službou opravy došlo během hello dobu, po kterou zjištěnými recyklace rolí hello.

Role se také recyklujte přibližně jednou za měsíc během hostitelským operačním systémem a aktualizací hostovaného operačního systému.  
Další informace najdete v tématu hello blogu [restartuje kvůli Role Instance tooOS upgrady](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Problém: nelze provést prohození virtuálních IP adres a zobrazí chybová zpráva
Prohození virtuálních IP adres není povolená, pokud probíhá nasazení aktualizace. Aktualizace nasazení může dojít automaticky při:

* Nové hostovaný operační systém je k dispozici a jsou nakonfigurovány pro automatické aktualizace.
* Služba opravy nastane.

toofind out-li automatické aktualizaci nebrání v provádění prohození virtuálních IP adres:

1. V hello portálu Azure klikněte na tlačítko hello nasazení cloudové služby.
2. V hello **vlastnosti** podokně hello portál Azure, podívejte se na hodnotu hello **stav**. Pokud je **připraven**, zkontrolujte **poslední operace** toosee, pokud jeden nedávno došlo, které by mohly bránit swap hello VIP.
3. Opakujte kroky 1 a 2 pro produkční nasazení hello.
4. Pokud je v procesu automatické aktualizace, počkejte na její toofinish před snažíme toodo hello VIP prohození.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Problém: Instanci role je opakování ve smyčce mezi spuštěno, Probíhá inicializace, zaneprázdněn a zastaveno
Tato podmínka by mohla indikovat potíže s kódem aplikace, balíčkem nebo konfiguračním souborem. V takovém případě byste měli mít toosee hello stav změna každých několik minut a hello portálu Azure může být uvedeno něco podobného jako **recyklace**, **zaneprázdněn**, nebo **Probíhá inicializace**. To znamená, že je něco není v pořádku s hello aplikace, která je uchovávání hello role instance spuštění.

Další informace o tom, tootroubleshoot u tohoto problému najdete v příspěvku blogu hello [Azure PaaS výpočetní diagnostická Data](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) a [běžné problémy této role toorecycle Příčina](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Problém: Moje aplikace přestala pracovat
1. V hello portálu Azure klikněte na instanci role hello.
2. V hello **vlastnosti** podokně hello portál Azure, zvažte následující podmínky tooresolve hello váš problém:
   * Pokud nedávno zastavil instanci role hello (můžete zkontrolovat hodnotu hello **Abort počet**), může aktualizovat hello nasazení. Pokud hello role instance obnoví funguje na svůj vlastní Počkejte, než toosee.
   * Pokud je hello role instance **zaneprázdněn**, zkontrolujte toosee kódu vaší aplikace, pokud hello [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) událost je zpracovávána. Může být nutné tooadd nebo odstraňte některé kód, který zpracovává této události.
   * Projít hello diagnostických dat a řešení potíží s scénáře v hello příspěvku na blogu [Azure PaaS výpočetní diagnostická Data](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

> [!WARNING]
> Pokud jste recyklovat cloudové služby, resetujete hello vlastnosti pro nasazení hello efektivně mazání hello informace pro původní problém hello.
>
>

## <a name="next-steps"></a>Další kroky
Zobrazení [řešení potíží s články](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pro cloudové služby.

v tématu jak problémy tootroubleshoot cloudové služby role pomocí Azure PaaS počítače diagnostická data toolearn [řady blogu kevina Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
