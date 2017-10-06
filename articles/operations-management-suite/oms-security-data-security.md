---
title: "aaaOperations Management Suite zabezpečení a Audit řešení Data zabezpečení | Microsoft Docs"
description: "Tento dokument popisuje způsob správy a ochrany dat v řešení Zabezpečení a audit pro Operations Management Suite."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 9c4181b3b491e4f7f0c57d7252eca78a819722d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Zabezpečení dat v řešení Zabezpečení a audit pro Operations Management Suite
toohelp zákazníkům zabránit, zjistit a reagovat toothreats, [Operations Management Suite (OMS) zabezpečení a Audit řešení](operations-management-suite-overview.md) shromažďuje a zpracovává data o vašich prostředků, která zahrnuje:

* Protokol událostí zabezpečení
* Události Trasování událostí pro Windows
* Události auditování AppLocker
* Protokol brány Windows Firewall
* Události Rozšířené analýzy hrozeb
* Výsledky základního vyhodnocování
* Výsledky antimalwarového vyhodnocování
* Výsledky vyhodnocování aktualizací a oprav
* Datové proudy systémové protokoly, které jsou explicitně povoleno na hello agenta

Provedeme silné závazky tooprotect hello ochrany osobních údajů a zabezpečení tato data. Microsoft dodržuje pravidla dodržování předpisů a zabezpečení toostrict – z kódování toooperating služby.
Tento článek popisuje způsob správy a ochrany dat v řešení Zabezpečení a audit v OMS.

## <a name="data-sources"></a>Zdroje dat
OMS zabezpečení a Audit řešení analyzovat data ze své virtuální počítače a fyzické počítače, kde je nainstalován hello OMS Agent. Řešení Zabezpečení a audit v OMS může shromažďovat informace o konfiguraci událostí zabezpečení, jako jsou například protokoly událostí a auditu systému Windows, protokoly služby IIS a zprávy syslog. Mezi příklady těchto dat patří: typ a verze operačního systému, spuštěné procesy, název počítače, IP adresy, přihlášený uživatel a ID tenanta.  

## <a name="data-protection"></a>Ochrana dat
**Oddělení dat**: Data se ukládají na jednotlivých součástí v rámci služby hello logicky samostatné. Všechna data jsou označená podle organizace. Toto značení přetrvává v průběhu cyklu hello dat a je požadováno v jednotlivých vrstvách služby hello. 

**Přístup k datům**: tooprovide doporučení zabezpečení a prozkoumat potenciální ohrožení zabezpečení, pracovníky společnosti Microsoft může přistupovat k informacím shromažďovaných či analyzovat služby. Jsme splňovat toohello [Microsoft Online Services podmínky](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) a [prohlášení o ochraně osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), který stavu, že Microsoft nebude používat Data zákazníků nebo odvození informací z něj jakémukoliv inzerování nebo podobné obchodní účely. doporučení zabezpečení tooprovide a prozkoumat potenciální ohrožení zabezpečení, pracovníky společnosti Microsoft může přistupovat k informacím shromažďovaných či analyzovat služby. Data zákazníků používáme pouze jako potřebné tooprovide vám Azure služby, včetně účely, které jsou kompatibilní s poskytování těchto služeb. Je-li zachovat všechna práva tooyour vlastní data.

**Data použití**: Společnost Microsoft používá vzory a analýzou hrozeb vidět napříč více klienty tooenhance naše funkce prevence a detekce; jsme učinit v souladu s závazky týkajícími se ochrany osobních údajů hello popsané v našem [ochrany osobních údajů Příkaz](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [!NOTE]
> Umístění dat je konfigurována na úrovni pracovní prostor OMS hello, při vytváření pracovního prostoru hello, která je součástí hello počáteční OMS zabezpečení a Audit proces konfigurace.
> 
> 

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli, jakým způsobem probíhá správa a ochrana dat v OMS. toolearn informace o OMS zabezpečení a Audit řešení, najdete v části:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a výstrahy tooSecurity odpovídá v Operations Management Suite zabezpečení a Audit řešení](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)

