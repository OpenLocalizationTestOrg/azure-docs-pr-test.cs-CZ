---
title: "aaaOperations Management Suite zabezpečení a Audit řešení webové základní | Microsoft Docs"
description: "Tento dokument popisuje, jak toouse OMS zabezpečení a Audit tooperform řešení webové směrného plánu hodnocení všech monitorovaných webových serverů za účelem dodržování předpisů a zabezpečení."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: 8aa87fa404ff97ab549dda3f9bebb75766055963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Vyhodnocování standardních hodnot webu v řešení Zabezpečení a audit pro Operations Management Suite
Tento dokument vám pomůže toouse [Operations Management Suite (OMS) zabezpečení a Audit řešení](operations-management-suite-overview.md) webové funkce tooaccess hello zabezpečené stavu monitorovaných prostředků vyhodnocení směrného plánu.

## <a name="what-is-web-baseline-assessment"></a>Co je vyhodnocování standardních hodnot webu?
Zabezpečení v OMS v současné době umožňuje vyhodnocování standardních hodnot zabezpečení pro operační systémy. Prohledá hello nastavení operačního systému serverů každých 24 hodin a poskytuje pohled na potenciálně citlivé nastavení. Další informace najdete v tématu [Vyhodnocování standardních hodnot v řešení Zabezpečení a audit pro Operations Management Suite](oms-security-baseline.md).

cílem Hello hello webové směrného plánu hodnocení je nastavení toofind potenciálně citlivé webového serveru. Hello tři primární zdroje pro hello webové standardních hodnot konfigurace jsou: Konfigurace rozhraní .NET, ASP.NET a služby IIS.  Stejně jako hello vyhodnocení směrného plánu operační systém, zabezpečení OMS přechází tooscan vaše webových serverů, každý 24hrs a zadejte pohled na stav zabezpečení z nich.  V Internetové informační služby (IIS), konfigurace jsou vysoce přizpůsobitelné, která umožňuje různé webu nebo aplikace toobe úrovně přepsat. skener Hello kontroluje hello nastavení na úrovni jednotlivých aplikací nebo na webu v přidání toohello výchozí kořenové úrovni. To vám pomůže tooidentify potenciální ohrožení zabezpečení nastavení umístění a rychle napravit.


## <a name="web-security-baseline-assessment"></a>Vyhodnocování standardních hodnot zabezpečení webu
Pro tuto verzi preview bude tato funkce toobe získat přístup pomocí možnost hledání OMS hello. Postupujte podle kroků hello níže tooperform hello přidělena dotazu:

1. V hello **Microsoft Operations Management Suite** klikněte na hlavním řídicím **zabezpečení a Audit** dlaždici.
2. V hello **zabezpečení a Audit** řídicí panel, klikněte na tlačítko **hledání protokolů** tlačítko.
3. Hello první dotaz, který můžete použít se hello **souhrn hodnocení základní webové**. V hello **Begin vyhledávání zde** pole, zadejte tento dotaz: typ*= SecurityBaselineSummary BaselineType = webové*. Hello následující obrazovka má ukázkový výstup:

![Souhrn vyhodnocení standardních hodnot webu](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> V tomto dotazu každý záznam označuje souhrn vyhodnocení na jednom serveru.

Jakmile jste na hello **hledání protokolů**, tooobtain různé dotazy můžete zadat další informace o vyhodnocení směrného plánu webové hello. Kromě toho toohello předchozího dotazu, můžete také použít hello následující těch, které jsou v této verzi preview.

**Vyhodnocení pravidel standardních hodnot webu:** Každý záznam představuje jedno vyhodnocení pravidla standardních hodnot webu na jednom serveru. Obsahuje všechna data pro pravidlo hello, umístění, hello očekávaný výsledek a skutečný výsledek hello.

**Dotaz:** Type*=SecurityBaseline BaselineType=web*

![Vyhodnocení pravidel standardních hodnot webu](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

**Zobrazit všechny výsledky pro určitý server**: Tento dotaz zobrazí jak toosee výsledků konkrétní serveru.

**Dotaz:***Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*

![Všechny výsledky](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Také můžete tyto záznamy nebo dotazy toocreate vlastní řídicí panely, sestavy nebo výstrahy. úvodní obrazovka níže má kontrolu uživatelského rozhraní, můžete přidat tooyour řídicí panel. Dozvíte, jak toovisualize svá data pomocí návrháře zobrazení OMS [zde](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/). úvodní obrazovka dole je příklad jak hello dlaždici bude vypadat podobně jako Jakmile provedete toto vlastní nastavení.

![Ukázka uživatelského rozhraní](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> Pokud chcete tooknow hello nastavení, které jsou zaškrtnutá políčka pro vyhodnocení směrného plánu hello, si můžete stáhnout [této tabulky aplikace Excel](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) obsahující tato nastavení.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli o vyhodnocování standardních hodnot webu v řešení Zabezpečení a audit pro OMS. Další informace o zabezpečení OMS toolearn najdete hello následující články:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a výstrahy tooSecurity odpovídá v Operations Management Suite zabezpečení a Audit řešení](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)

