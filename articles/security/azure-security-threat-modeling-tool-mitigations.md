---
title: "aaaMitigations - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "Stránka jejich zmírnění pro hello Microsoft Threat modelování nástroj zvýraznění možná řešení toohello nejvíce zveřejněné generována hrozeb."
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
ms.openlocfilehash: 000c0980d976b09fc9287e582e3776efaf7e390c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-threat-modeling-tool-mitigations"></a>Nástroje pro modelování hrozeb Microsoft jejich zmírnění

Hello nástroj modelování hrozeb je element jádra systému hello Microsoft životního cyklu SDL (Security Development). Umožňuje softwaru architects tooidentify a zmírnit potenciální potíže se zabezpečením již v rané fázi, když jsou poměrně jednoduché a nákladově efektivní tooresolve. V důsledku toho výrazně snižuje hello celkové náklady na vývoj. Také jsme chtěli hello nástroj s odborníky nesouvisející se zabezpečením na paměti, snadněji modelování hrozeb pro všechny vývojáře tím, že poskytuje jasné pokyny o vytvoření a analýza hrozeb modelů.

Navštivte hello  **[nástroj modelování hrozeb](./azure-security-threat-modeling-tool.md)**  tooget spustit ještě dnes!

## <a name="mitigation-categories"></a>Zmírnění dopadů kategorií

způsoby zmírnění hrozeb modelování nástroj Hello jsou rozdělené podle toohello webové aplikace zabezpečení rámce, který se skládá z následujících hello:

| Kategorie | Popis |
| -------- | ----------- |
| **[Auditování a protokolování](./azure-security-threat-modeling-tool-auditing-and-logging.md)** | Kdo to udělal co a kdy? Protokolování a auditování odkazovat toohow vaší aplikace záznamy související se zabezpečením události |
| **[Ověřování](./azure-security-threat-modeling-tool-authentication.md)** | Kdo jsi? Ověřování je proces hello prokáže, kde entity hello identity jinou entitou, obvykle prostřednictvím přihlašovacích údajů, jako je například uživatelské jméno a heslo |
| **[Autorizace](./azure-security-threat-modeling-tool-authorization.md)** | Co můžete dělat? Autorizace je, jak aplikace nabízí řízení přístupu pro prostředky a operace |
| **[Zabezpečení komunikace](./azure-security-threat-modeling-tool-communication-security.md)** | Kdo jste mluvení do? Zabezpečení komunikace zajistí, že je jako zabezpečené nejblíže veškerá komunikace provést |
| **[Správa konfigurací](./azure-security-threat-modeling-tool-configuration-management.md)** | Kdo aplikaci spustit jako? Je databáze, která připojení k? Jak je spravována aplikace? Jak jsou zabezpečené tato nastavení? Správa konfigurace odkazuje toohow vaše aplikace zpracovává tyto provozní problémy |
| **[Kryptografie](./azure-security-threat-modeling-tool-cryptography.md)** | Jak se udržuje tajné klíče (utajení)? Jak jste kontroly pravopisu systému úmyslnému poškození dat nebo knihovny (integrita)? Jak jste poskytli semen náhodných hodnot, které musí být kryptograficky silnou? Kryptografie odkazuje toohow aplikace vynucuje důvěrnost a integrita |
| **[Správa výjimek](./azure-security-threat-modeling-tool-exception-management.md)** | Při volání metody ve vaší aplikaci nezdaří, jakým způsobem aplikace? Kolik můžete odhalit? Vrátíte popisný chyba informace tooend uživatelů? Předat volající back toohello výjimka cenné informace? Aplikaci řádně nezdaří? |
| **[Ověření vstupu](./azure-security-threat-modeling-tool-input-validation.md)** | Jak poznáte, že aplikace přijímá vstupní hello je platný a bezpečný? Ověření vstupu odkazuje toohow aplikace filtry, scrubs nebo odmítne vstup před další zpracování. Zvažte chovaly vstupu prostřednictvím vstupní body a kódování výstup prostřednictvím ukončovací body. Důvěřujete data ze zdroje jako databáze a sdílené složky? |
| **[Citlivá Data](./azure-security-threat-modeling-tool-sensitive-data.md)** | Jak aplikace zpracovávat citlivá data? Citlivá data odkazuje toohow vaše aplikace zpracuje všechna data, která musí být chráněny v paměti, přes síť hello nebo v trvalé úložiště |
| **[Správa relací](./azure-security-threat-modeling-tool-session-management.md)** | Jak aplikace zpracování a ochranu uživatelských relací? Relaci odkazuje tooa řadu související interakce mezi uživateli a webové aplikace |

To pomáhá identifikovat:

* Kde jsou hello nejběžnější chyby vzniklé
* Kde jsou hello nejvíce řešitelné vylepšení

V důsledku toho použijte tyto kategorie toofocus a stanovení priorit práce zabezpečení, takže pokud víte, že hello současných převažujících problémy se zabezpečením, ke kterým došlo v hello vstupní ověřování, ověřování a autorizace kategorií, můžete spustit existuje. Další informace najdete v článku  **[tento patentová odkaz](https://www.google.com/patents/US7818788)**

## <a name="next-steps"></a>Další kroky

Navštivte  **[Threat modelování hrozeb nástroj](./azure-security-threat-modeling-tool-threats.md)**  toolearn Další informace o hello hrozeb, které používá nástroj hello kategorie toogenerate možné návrhu hrozeb.
