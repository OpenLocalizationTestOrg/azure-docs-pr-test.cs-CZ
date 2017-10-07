---
title: "aaaOperations Management Suite zabezpečení a Audit řešení základní | Microsoft Docs"
description: "Tento dokument popisuje, jak toouse OMS zabezpečení a Audit řešení tooperform směrného plánu hodnocení všech monitorovaných počítačů za účelem dodržování předpisů a zabezpečení."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: ea52408cb9d2598728fe3826a946067e1c99318f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Vyhodnocování standardních hodnot v řešení Zabezpečení a audit pro Operations Management Suite
Tento dokument vám pomůže toouse [Operations Management Suite (OMS) zabezpečení a Audit řešení](operations-management-suite-overview.md) směrného plánu vyhodnocení možnosti tooaccess hello zabezpečené stavu monitorovaných prostředků.

## <a name="what-is-baseline-assessment"></a>Co je vyhodnocování standardních hodnot?
Společnost Microsoft, spolu s organizacemi z oboru a vládními organizacemi po celém světě, definuje konfiguraci systému Windows, která představuje vysoce zabezpečená nastavení serverů. Tuto konfiguraci tvoří sada klíčů registru, nastavení zásad auditu, nastavení zásad zabezpečení a společností Microsoft doporučené hodnoty pro tato nastavení. Tato sada pravidel se označuje jako standardní hodnoty zabezpečení. Schopnost vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS umožňuje bezproblémovou kontrolu dodržování předpisů na všech vašich počítačích. 

Existují tři typy pravidel:

* **Pravidla registru**: kontrolují, jestli jsou správně nastavené klíče registru.
* **Pravidla zásad auditu**: pravidla týkající se vašich zásad auditu.
* **Pravidla zásad zabezpečení**: pravidla týkající hello uživatelských oprávnění na počítači hello.

> [!NOTE]
> Čtení [použití OMS zabezpečení tooassess hello standardních hodnot konfigurace zabezpečení](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) stručný přehled této funkce.
> 
> 

## <a name="security-baseline-assessment"></a>Vyhodnocování standardních hodnot zabezpečení
Můžete zkontrolovat vaše aktuální vyhodnocení směrného plánu zabezpečení pro všechny počítače, které jsou monitorovány pomocí OMS zabezpečení a Audit hello řídicím panelu.  Provést následující kroky tooaccess hello zabezpečení směrného plánu řídicí panel assessment hello:

1. V hello **Microsoft Operations Management Suite** hlavní řídicí panel, klikněte na tlačítko **zabezpečení a Audit** dlaždici.
2. V hello **zabezpečení a Audit** řídicí panel, klikněte na tlačítko **směrného plánu vyhodnocení** pod **zabezpečení domény**. Hello **vyhodnocení směrného plánu zabezpečení** řídicího panelu se zobrazí, jak ukazuje následující obrázek hello:
   
    ![Vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Tento řídicí panel je rozdělen na tři hlavní oblasti:

* **Počítače porovnání toobaseline**: Tato část poskytuje souhrn hello počet počítačů, které byly a hello procento počítačů, které předávají hello hodnocení. Poskytuje také hello top 10 počítačů a hello procento výsledek pro vyhodnocení hello.
* **Vyžaduje stav pravidel**: Tato část obsahuje hello záměrné toobring povědomí o hello se nezdařilo pravidla podle závažnosti, se nezdařilo pravidla podle typu. Podle toohello první graf, které můžete rychle zjistit, zda většina hello se nezdařilo pravidla jsou důležité, nebo ne. Také poskytuje seznam hello top 10 neúspěšných pravidel a jejich závažnosti. Hello druhý graf znázorňuje hello typ pravidla, které selhaly během hodnocení hello. 
* **Počítače s chybějícími směrného plánu vyhodnocení**: Tato část obsahuje seznam počítačů hello, které nejsou přístupné z důvodu chyby nebo nekompatibilita toooperating systému. 

### <a name="accessing-computers-compared-toobaseline"></a>Přístup k počítačům ve srovnání toobaseline
V ideálním případě všechny počítače se musí splňovat hello vyhodnocení směrného plánu zabezpečení. Nicméně se dá očekávat, že v některých případech tomu tak není. V rámci procesu správy hello zabezpečení je důležité tooinclude kontrola hello počítačů, které selhaly toopass všechny testy vyhodnocení zabezpečení. Toovisualize rychlý způsob, který je výběrem možnosti hello **počítače získat přístup k** umístěný v hello **počítače porovnání toobaseline** části. Zobrazí hello protokolu vyhledávání výsledek zobrazující hello seznam počítačů, jako ukazuje v hello následující obrazovka:

![Výsledky hledání dostupných počítačů](./media/oms-security-baseline/oms-security-baseline-fig2.png)

výsledek hledání Hello se zobrazuje ve formátu tabulky, kde hello první sloupec má název počítače hello a druhá barva hello hello počet pravidel, která se nezdařila. tooretrieve hello informace týkající se hello typ pravidla, která se nezdařila, klikněte v hello počet neúspěšných pravidel kromě hello název počítače. Měli byste vidět výsledek podobné toohello, ukazuje následující obrázek hello:

![Podrobnosti o výsledku hledání dostupných počítačů](./media/oms-security-baseline/oms-security-baseline-fig3.png)

V této výsledek hledání máte celkem hello pravidel přístupu, hello počet kritické pravidla, která se nezdařila, hello pravidla upozornění a hello pravidla informace se nezdařilo.

### <a name="accessing-required-rules-status"></a>Přístup ke stavu požadovaných pravidel
Po získání hello informace týkající se hello procento počet počítačů s úspěšně dokončenou hello hodnocení, můžete další informace o pravidlech, u kterých se nedaří podle závažnost toohello tooobtain. Tato vizualizace pomůže tooprioritize počítače, které by měl být řešit první tooensure budou kompatibilní v další assessment hello. Pozastavte ukazatel myši nad hello důležitou součástí hello graf umístěný v hello **se nezdařilo pravidla podle závažnosti** dlaždici v části **vyžaduje stav pravidel** a klikněte na něj. Měli byste vidět výsledek podobné toohello následující obrazovka:

![Podrobnosti o pravidlech, která selhala, podle závažnosti](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

Výsledek tohoto protokolu se zobrazuje hello typ pravidla směrného plánu, který selhal, hello popis tohoto pravidla a ID společné konfigurace – výčet (CCE) hello tohoto pravidla zabezpečení. Tyto atributy musí být tento problém v cílovém počítači hello dostatek tooperform toofix opravné akce.

> [!NOTE]
> Další informace o CCE přístup hello [National Vulnerability databáze](https://nvd.nist.gov/cce/index.cfm).
> 
> 

### <a name="accessing-computers-missing-baseline-assessment"></a>Přístup k počítačům, u nichž nedošlo k vyhodnocení standardních hodnot
OMS podporuje hello členem domény a profil základní řadič domény na Windows Server 2008 R2 až tooWindows Server 2012 R2. Standardní hodnoty systému Windows Server 2016 ještě nejsou dokončené, ale budou přidány okamžitě po publikování. Všechny ostatní operační systémy, zkontrolovat pomocí OMS zabezpečení a Audit hodnocení směrného plánu se zobrazí pod hello **počítače s chybějícími směrného plánu vyhodnocení** části.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli o vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS. Další informace o zabezpečení OMS toolearn najdete hello následující články:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a výstrahy tooSecurity odpovídá v Operations Management Suite zabezpečení a Audit řešení](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)

