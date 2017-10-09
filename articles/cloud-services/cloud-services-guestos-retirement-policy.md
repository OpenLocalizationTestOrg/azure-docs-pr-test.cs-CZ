---
title: "Průvodce aaaSupportability a vyřazení zásady pro Azure hostovaného operačního systému | Microsoft Docs"
description: "Poskytuje informace o co bude podpory společnosti Microsoft Pokud jde o toohello Azure hostovaného operačního systému používá cloudové služby."
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 919dd781-4dc6-4e50-bda8-9632966c5458
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 5/26/2017
ms.author: raiye
ms.openlocfilehash: 6a86c42ac95d12bbf116d900b7afb26fc3fe34e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure zásad možnosti podpory a vyřazení hostovaného operačního systému
Hello informace na této stránce má vztah toohello Azure hostovaný operační systém ([hostovaného operačního systému](cloud-services-guestos-update-matrix.md)) pro cloudové služby worker a webová role (PaaS). Nevztahuje se tooVirtual počítače (IaaS).

Společnost Microsoft nemá publikovaný [zásady podpory pro hello hostovaného operačního systému](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Hello stránky, které právě čtete teď popisuje, jak je implementována hello zásad.

zásady Hello

1. Microsoft bude podporovat **alespoň hello nejnovější dva druhy hello hostovaného operačního systému**. Při vyřazení rodinu, mají zákazníci 12 měsíců od hello oficiální vyřazení datum tooupdate tooa novější podporované hostovaného operačního systému rodiny.
2. Microsoft bude podporovat **alespoň hello nejnovější dvě verze hello podporované hostovaného operačního systému rodiny**.
3. Microsoft bude podporovat **alespoň hello nejnovější dvě verze hello Azure SDK**. Když verzi hello SDK byl vyřazen z provozu, bude mít zákazníci 12 měsíců od hello oficiální vyřazení datum tooupdate tooa novější verze.

V některých případech může být podporována více než dvě řady nebo verze. Informace o podpoře oficiální hostovaného operačního systému se zobrazí na hello [verzí hostovaného operačního systému Azure a kompatibilních sad SDK](cloud-services-guestos-update-matrix.md).

## <a name="when-a-guest-os-family-or-version-is-retired"></a>Když je vyřazeno rodiny hostovaného operačního systému nebo verze
Nové hostovaného operačního systému **rodiny** uvádíme zopakovat po vydání hello nové oficiální verze operačního systému Windows Server hello. Vždy, když je zavádí nové rodiny hostovaného operačního systému, bude Microsoft vyřazení hello nejstarší hostovaného operačního systému rodiny.

Nové hostovaného operačního systému **verze** vydávají o každý měsíc tooincorporate hello nejnovější střediska MSRC aktualizace. Z důvodu hello pravidelných měsíčních aktualizací, je obvykle zakázané 60 dnů po jeho vydání verze hostovaného operačního systému. Tato aktivita udržuje alespoň dvě verze hostovaného operačního systému pro každou skupinu, která je k dispozici pro použití.

### <a name="process-during-a-guest-os-family-retirement"></a>Proces při vyřazení rodiny hostovaného operačního systému
Jakmile se oznámí vyřazení hello, zákazníci mají 12 měsíce "přechodu" období před starší rodiny hello se oficiálně vyřadí z provozu. Tento čas přechodu může prodloužit uvážení hello společnosti Microsoft. Aktualizace budou odeslány na hello [verzí hostovaného operačního systému Azure a kompatibilních sad SDK](cloud-services-guestos-update-matrix.md).

Proces postupného vyřazení zahájíte půl (6) měsíců do období přechodu hello. Během této doby:

1. Microsoft oznámí zákazníkům hello vyřazení.
2. Hello novější verzi sady Azure SDK hello nebudou podporovat hello vyřazeno hostovaného operačního systému rodiny.
3. Nová nasazení a redeployments cloudové služby nebudou povolena, v hello vyřazeno řady

Microsoft bude nová verze hostovaného operačního systému toointroduce zařadit nejnovější aktualizace střediska MSRC hello dokud hello poslední den hello přechod období, označuje jako "datum vypršení platnosti" hello. Na datum vypršení platnosti hello bude se Nepodporovaná cloudové služby, které jsou pořád běží pod hello smlouva Azure SLA. Společnost Microsoft nemá hello uvážení tooforce upgradovat, odstraňte nebo zastavte tyto služby po tomto datu.

### <a name="process-during-a-guest-os-version-retirement"></a>Proces při vyřazení verze operačního systému hosta
Pokud zákazníci nastavit jejich aktualizace tooautomatically hostovaného operačního systému, mají nikdy tooworry o práci s verzí hostovaného operačního systému. Se budou vždy používat nejnovější verzi operačního systému hosta hello.

Verze operačního systému hosta vydávají každý měsíc. Z důvodu hello míra regulární verzí každá verze má pevnou životnost.

Na 60 dnů do hello životnost, verze je "*zakázáno*". "Zakázáno" znamená, že danou verzi hello se odebere z portálu hello. verze Hello můžete nastavit už z hello CSCFG konfiguračního souboru. Existující nasazení jsou spuštěny. Ale nebudou povolena nová nasazení a nasazení aktualizací tooexisting kódu a konfigurace.

Verze operačního systému hosta hello delší dobu, po tom, aby "Zakázat", "*vyprší platnost*" a vynutit upgradovat a nastavení aktualizace hello tooautomatically hostovaného operačního systému v hello budoucí jsou všechny instalace této verze. Vypršení platnosti se provádí v dávkách tak dobu hello z postižení tooexpiration se může lišit.

Tato období se provádí delší v přechody zákazníka tooease uvážení společnosti Microsoft. Všechny změny budou oznamovat na hello [verzí hostovaného operačního systému Azure a kompatibilních sad SDK](cloud-services-guestos-update-matrix.md).

### <a name="notifications-during-retirement"></a>Oznámení při vyřazení
* **Rodiny vyřazení** <br>Microsoft použije příspěvcích na blogu a portálu oznámení. Zákazníci, kteří stále používají vyřazeno hostovaného operačního systému rodiny informováni prostřednictvím Správci služeb tooassigned přímou komunikaci (e-mailu, portálu zprávy, telefonního hovoru). Všechny změny budou odeslány toothis stránku a hello informační kanál RSS uvedené na začátku hello tuto stránku.
* **Vyřazení verze** <br>Všechny změny a hello kalendářních dat, které k nim dojde, budou odeslány toothis stránku a hello informační kanál RSS uvedené na začátku hello tuto stránku, včetně verze, zakázán nebo vypršení platnosti. Správci služby bude dostávat e-maily, pokud mají nasazení spuštění na zakázáno verze operačního systému hosta nebo rodiny. Načasování Hello těchto e-mailů se může lišit. Obecně jsou alespoň měsíc předcházející postižení, i když tento časování není oficiální SLA.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
**Jak zmírnit dopady hello migrace**

Doporučujeme vám, že používáte nejnovější rodiny hostovaného operačního systému pro navrhování cloudové služby.

1. Spusťte již v rané fázi plánování vaší rodiny novější tooa migrace.
2. Nastavte tootest dočasné testovací nasazení cloudové služby na nové rodiny hello spuštěna.
3. Nastavit vaší verzí hostovaného operačního systému příliš**automatické** (osVersion = * v hello [.cscfg](cloud-services-model-and-package.md#cscfg) souboru) tak hello migrace toonew hostovaného operačního systému verze probíhá automaticky.

**Co když Moje webová aplikace vyžaduje hlubší integrace s hello operačního systému?**

Pokud architektura vaší webové aplikace, závisí na základní funkce hello operačního systému, použijte možnosti podporované platformy, jako [spuštění úlohy](cloud-services-startup-tasks.md) nebo jiných mechanismů rozšíření. Alternativně můžete také použít [virtuální počítače Azure](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS – infrastruktura jako služba), kde je zodpovědná za údržbu hello základní operační systém.

## <a name="next-steps"></a>Další kroky
Zkontrolujte hello nejnovější [uvolní hostovaného operačního systému](cloud-services-guestos-update-matrix.md).
