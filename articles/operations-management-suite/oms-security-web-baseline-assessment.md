---
title: "Vyhodnocení směrného plánu v Operations Management Suite zabezpečení a Audit řešení základní aaaWeb | Microsoft Docs"
description: "Tento dokument popisuje, jak toouse webové vyhodnocení směrného plánu v OMS zabezpečení a Audit řešení tooperform směrného plánu hodnocení všech monitorovaných webových serverů za účelem dodržování předpisů a zabezpečení."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: yurid
ms.openlocfilehash: dafa9d3d93fae31748306b60ee40b285dd59c802
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Vyhodnocování standardních hodnot webu v řešení Zabezpečení a audit pro Operations Management Suite
Tento dokument vám pomůže používat OMS zabezpečení a Audit webové směrného plánu vyhodnocení možnosti tooaccess hello zabezpečené stavu monitorovaných prostředků.

## <a name="what-is-web-baseline-assessment"></a>Co je vyhodnocování standardních hodnot webu?
Zabezpečení v OMS v současné době umožňuje vyhodnocování standardních hodnot zabezpečení pro operační systémy. Prohledá hello nastavení operačního systému serverů každých 24 hodin a poskytuje pohled na potenciálně citlivé nastavení. Další informace najdete v tématu [Vyhodnocování standardních hodnot v řešení Zabezpečení a audit pro Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/oms-security-baseline).

cílem Hello hello assessment webové směrného plánu je nastavení toofind potenciálně citlivé webového serveru. Hello tři primární zdroje pro hello webové standardních hodnot konfigurace jsou: Konfigurace rozhraní .NET, ASP.NET a služby IIS.  Stejně jako hello vyhodnocení směrného plánu operační systém, zabezpečení OMS přechází tooscan vaše webových serverů, každý 24hrs a zadejte pohled na stav zabezpečení z nich.  V Internetové informační služby (IIS), konfigurace jsou vysoce přizpůsobitelné, která umožňuje různé webu nebo aplikace toobe úrovně přepsat. skener Hello kontroluje hello nastavení na úrovni jednotlivých aplikací nebo na webu v přidání toohello výchozí kořenové úrovni. To vám pomůže tooidentify potenciálně citlivé nastavení a rychle napravit, včetně Naše doporučení pro taková nastavení.

>[!NOTE] 
>Můžete si stáhnout hello běžné identifikátory konfigurace a standardních pravidel používané OMS zabezpečení v tomto [stránky](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335?redir=0).


## <a name="web-security-baseline-assessment"></a>Vyhodnocování standardních hodnot zabezpečení webu

Pro tuto verzi preview je přístupný prostřednictvím hello možnost hledání OMS a hello OMS zabezpečení a Audit řídicí panel funkce hello. Postupujte podle kroků hello níže tooperform hello přidělena dotazu:

1. V hello **Microsoft Operations Management Suite** hlavní řídicí panel, klikněte na tlačítko **zabezpečení a Audit** dlaždici.
2. V hello **zabezpečení a Audit** řídicí panel, uvidíte hello základní webové perspektivy další toohello OS směrného plánu perspektivy.
   
    ![Vyhodnocování standardních hodnot zabezpečení webu v řešení OMS Security and Audit](./media/oms-security-web-baseline/oms-security-web-baseline-fig5.png)

3. Hello levém podokně se zobrazují hello počet standardních hodnot toohello webové servery porovnání hello průměrnou procentuální hodnotu pravidla, které předávají ve všech serverech hello vyhodnotit a hello seznam serverů, které byly vyhodnoceny.
4. Hello v pravém podokně zobrazí hello jedinečný pravidla, která se nezdařila v důsledku *závažnost*, a *typ pravidla*. Kliknutím na žádné z pravidel hello pravém podokně se zobrazí hello podrobnosti o tomto pravidle. Příklad je uveden v hello pod obrázkem. pravidlo Hello, který se vyhodnotí je uveden v části *nastavení pravidla*. Hello *AzId* pole, které je jedinečný identifikátor pro každé pravidlo, které společnost Microsoft používá pro sledování hello standardních pravidel. Kromě toho toothat uživatelé mohou vidět hello *očekávaný výsledek* (společnosti Microsoft Doporučená hodnota), a další podrobnosti týkající se zabezpečení dopad hello hello pravidla.
    
    ![Dotaz](./media/oms-security-web-baseline/oms-security-web-baseline-fig6.png)

5. Můžete vytvořit své vlastní dotazy tooreview hello výsledky. 

Hello první dotaz, který můžete použít se hello **souhrn hodnocení základní webové**. V hello **Begin vyhledávání zde** pole, zadejte tento dotaz: *typ = SecurityBaselineSummary BaselineType = webové*. Hello následuje Ukázka výstupu:

![Výsledek dotazu](./media/oms-security-web-baseline/oms-security-web-baseline-fig7.png)

>[!NOTE] 
>V tomto dotazu každý záznam označuje souhrn vyhodnocení na jednom serveru.

Jakmile jste na hello **hledání protokolů**, tooobtain různé dotazy můžete zadat další informace o vyhodnocení směrného plánu webové hello. Kromě toho toohello předchozího dotazu, můžete také použít hello následující těch, které jsou v této verzi preview:

**Vyhodnocení pravidel standardních hodnot webu:** Každý záznam představuje jedno vyhodnocení pravidla standardních hodnot webu na jednom serveru. Obsahuje všechna data v případě selhání pravidla hello *SitePath* na které hello pravidlo bylo vyhodnoceno, hello *očekávaný výsledek*a hello *skutečný výsledek*.

Dotaz: *Type=SecurityBaseline BaselineType=Web AnalyzeResult=Failed*

![Výsledek dotazu 2](./media/oms-security-web-baseline/oms-security-web-baseline-fig8.png)

**Zobrazit všechny výsledky pro určitý server**: Tento dotaz zobrazí jak toosee výsledků konkrétní serveru: dotaz: *typ = SecurityBaseline BaselineType = počítač webového = BaselineTestVM1*

![Výsledek dotazu 3](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Tyto záznamy nebo dotazy toocreate můžete použít vlastní řídicí panely, sestavy nebo výstrahy. Zde je ukázka ovládacího prvku uživatelského rozhraní, můžete přidat řídicí panel tooyour. Dozvíte, jak toovisualize svá data pomocí návrháře zobrazení OMS [zde](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/). úvodní obrazovka dole je příklad jak hello dlaždici bude vypadat podobně jako Jakmile provedete toto vlastní nastavení.

![řídicí panel](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli o vyhodnocování standardních hodnot webu v řešení OMS Security and Audit. Další informace o zabezpečení OMS toolearn najdete hello následující články:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a výstrahy tooSecurity odpovídá v Operations Management Suite zabezpečení a Audit řešení](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)

