---
title: "aaaThreats - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "Stránka kategorie hrozeb pro hello Microsoft Threat modelování Tool, obsahující kategorie pro všechny vystavené generována hrozeb."
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 0bd51f81370b6385ff1ac9769e34fc089e1dfc9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-threat-modeling-tool-threats"></a>Nástroje pro modelování hrozeb Microsoft hrozeb

Hello nástroj modelování hrozeb je element jádra systému hello Microsoft životního cyklu SDL (Security Development). Umožňuje softwaru architects tooidentify a zmírnit potenciální potíže se zabezpečením již v rané fázi, když jsou poměrně jednoduché a nákladově efektivní tooresolve. V důsledku toho výrazně snižuje hello celkové náklady na vývoj. Také jsme chtěli hello nástroj s odborníky nesouvisející se zabezpečením na paměti, snadněji modelování hrozeb pro všechny vývojáře tím, že poskytuje jasné pokyny o vytvoření a analýza hrozeb modelů.

> Navštivte hello  **[nástroj modelování hrozeb](./azure-security-threat-modeling-tool.md)**  tooget spustit ještě dnes!

Hello nástroj modelování hrozeb lze zodpovědět určité otázky, jako je například hello ty, které jsou níže:

* Jak můžete změnit útočník hello ověřovací data?
* Co je hello vliv, pokud útočník může číst data uživatelského profilu hello?
* Co se stane, pokud byl odepřen přístup toohello databáze profilů uživatelů?

## <a name="stride-model"></a>STRIDE modelu

Nápověda toobetter formulovali tyto druhy odkazováno otázky, Microsoft používá hello STRIDE model, který rozděluje různých typů hrozeb a zjednodušuje hello celkového zabezpečení konverzace.

| Kategorie | Popis |
| -------- | ----------- |
| **Falšování identity** | Zahrnuje nelegálního přístupu a pak použití informace o ověřování druhého uživatele, jako je například uživatelské jméno a heslo |
| **Manipulaci** | Zahrnuje hello škodlivý úpravu data. Mezi příklady patří neoprávněné změny toopersistent dat, jako jsou uloženy v databázi a změnou hello dat, jak ho toků mezi dvěma počítači přes síť otevřete například hello Internetu |
| **Popírání odpovědnosti** | Přidružená k uživatelům, kteří odepřít provádění akce bez jiných stran, v opačném případě nutnosti jakékoli tooprove způsob – například uživatel provede neplatnou operaci v systému, jehož hello možnost tootrace hello zakazuje operace. Bez Odvolatelnost odkazuje toohello možnost hrozeb odvolatelnost toocounter systému. Uživatel, který zakoupí položku může mít například toosign pro položku hello po přijetí. pak použijte hello podepsané přijetí jako důkaz, že tento uživatel hello hello balíček obdrží může Hello dodavatele |
| **Zpřístupnění informací** | Zahrnuje hello vystavení tooindividuals informace, kteří nejsou by měl přístup tooit toohave – například hello schopnost uživatelé tooread soubor, který nebyly udělen přístup k nebo hello možnost útočníkům tooread dat během přenosu mezi dvěma počítači |
| **Odmítnutí služby** | Útok na dostupnost služby (DoS) odmítnutí služby toovalid uživatele – například tím, že webový server dočasně nedostupné nebo nepoužitelné. Je nutné chránit proti určitých typů DoS hrozby jednoduše tooimprove systému dostupnost a spolehlivost |
| **Zvýšení úrovně oprávnění** | Neprivilegovaný uživatelský získá privilegovaného přístupu a tím má dostatečný přístup toocompromise nebo zrušení hello celý systém. Zvýšení úrovně oprávnění hrozeb zahrnují tyto situace, ve kterých má útočník efektivně procházejí všechny obrany systému a stávají součástí samotného nebezpečnou situaci skutečně hello důvěryhodné systému |

## <a name="next-steps"></a>Další kroky

Pokračovat příliš**[jejich zmírnění nástroj pro modelování hrozeb](./azure-security-threat-modeling-tool-mitigations.md)**  toolearn hello různé způsoby zmírnit tyto hrozby s Azure.
