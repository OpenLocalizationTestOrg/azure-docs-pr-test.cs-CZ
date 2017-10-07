---
title: aaaRespond toosecurity incidenty s Azure Security Center | Microsoft Docs
description: "Tento dokument popisuje, jak toouse Azure Security Center scénář reakcí na incidenty."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 8af12f1c-4dce-4212-8ac4-170d4313492d
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: aaf50c0c7e774d03d517c3fd11686dbae48dd29b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-security-center-for-an-incident-response"></a>Využití Azure Security Center při reakci na incidenty
Mnoho organizací informace jak toorespond toosecurity incidenty až po souvisejících s narušením útoku. tooreduce náklady a poškození, je důležité toohave odpověď incidentu plánování na místě před útokem probíhá. Azure Security Center můžete využít v různých fázích reakce na incidenty.

## <a name="incident-response-planning"></a>Plánování reakce na incidenty
Efektivní plán závisí na tři základní možnosti: je možné tooprotect, zjistit a reagovat toothreats. Ochrana je o brání incidenty, zjišťování se o identifikaci hrozby již v rané fázi a odpověď je o vyřazení hello útočník a obnovení systémy toomitigate hello ovlivňuje narušení.

Tento článek použije fázích reakcí na incidenty zabezpečení hello z hello [odpověď zabezpečení společnosti Microsoft Azure v cloudu hello](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) článek, jak je znázorněno v následujícím diagramu hello:

![Životní cyklus reakce na incidenty](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Security Center můžete použít během fáze detekovat, hodnocení a Diagnostikujte hello. Zde jsou příklady jak Security Center může být užitečné při tři fáze počáteční reakce na incidenty hello:

* **Zjištění**: Přečtěte si první indikace hello šetření událostí.
  * Příklad: Zkontrolujte hello počáteční ověření, že byla vyvolána výstraha zabezpečení s vysokou prioritou na řídicím panelu Security Center hello.
* **Vyhodnocení**: provedení počáteční assessment tooobtain hello Další informace o hello podezřelou aktivitu.
  * Příklad: získáte další informace o výstraha zabezpečení hello.
* **Diagnostika**: Provedení technického vyšetřování a určení strategií pro zadržení, zmírnění škod a alternativní řešení.
  * Příklad: postupujte podle kroků nápravy hello popsaného v konkrétní zabezpečení výstrahy Security Center.

Hello scénář, který následuje ukazuje, jak tooleverage Security Center během hello detekovat, hodnocení a Diagnostikujte nebo reakce fázích incidentu zabezpečení. V Security Center představuje [incident zabezpečení](security-center-incident.md) souhrn všech výstrah pro určitý prostředek, které odpovídají schématům modelu [Kill Chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/). Incident se zobrazí v hello [výstrahy zabezpečení](security-center-managing-and-responding-alerts.md) dlaždice a okno. Incident odhalí hello seznamu související výstrahy, což umožňuje tooobtain můžete další informace o každém výskytu. Security Center přináší to také samostatné výstrahy zabezpečení, které může být také použít tootrack dolů podezřelou aktivitu.

## <a name="scenario"></a>Scénář
Contoso nedávno migrace některé z jejich tooAzure místní prostředky, včetně některých úloh obchodní založené na virtuální počítač a databází SQL. Nyní má Hlavní tým reakce na incidenty zabezpečení počítačů (CSIRT) společnosti Contoso problém s vyšetřováním problémů zabezpečení kvůli tomu, že analytické funkce zabezpečení nejsou integrované se současnými nástroji reakce na incidenty. Tento kvůli nedostupnosti integrace se objevuje problém během hello zjistit fázi (příliš mnoho chybných přijetí), a také během hello hodnocení a Diagnostikujte fázích. V rámci této migrace se rozhodli tooopt v pro toohelp Security Center je řešení tohoto problému.

Hello první fáze této migrace dokončení po jejich zařazený nemá všechny prostředky a řešit všechny hello doporučení zabezpečení ze služby Security Center. Contoso CSIRT je hello ústředním bodem pro práci s incidenty zabezpečení počítače. tým Hello se skládá z ve skupině uživatelů s odpovědnosti pro práci s incidentu zabezpečení. Členové týmu Hello jasně tooensure povinností, který se nechá žádné oblasti odpovědi zjištěných definovali.

Za účelem hello tohoto scénáře vytvoříme toofocus na hello role hello osoby, které jsou součástí Contoso CSIRT následující:

![Životní cyklus reakce na incidenty](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Judy pracuje v oddělení zabezpečení. Mezi její oblasti odpovědnosti patří:

* Monitorování a reagovat na hrozby toosecurity kolem hello hodiny.
* Narůstajícím toohello vlastník úloh v cloudu nebo analytik zabezpečení podle potřeby.

Sam je analytik zabezpečení a mezi jeho povinnosti patří:

* Vyšetřování útoků.
* Napravování výstrah.
* Práce s toodetermine vlastníky úloh a použití způsoby zmírnění rizik.

Jak vidíte, Monika a Sam mají různé odpovědnosti a musí fungují společně tooshare Security Center informace.

## <a name="recommended-solution"></a>Doporučené řešení
Vzhledem k tomu, že Monika a Sam mají různé role, že budete používat různé oblasti Security Center tooobtain důležité informace pro jejich denní aktivity. Judy bude používat jako součást denního monitorování **Výstrahy zabezpečení**.

![Výstrahy zabezpečení](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Monika použije výstrahy zabezpečení během hello rozpoznat a fáze hodnocení. Po dokončení počáteční assessment hello Monika Jana může eskalovat hello problém tooSam Pokud je potřeba další šetření. V tomto okamžiku Sam použije hello informací poskytnutých pomocí služby Security Center, někdy ve spojení s jinými zdroji dat. toomove toohello Diagnostikujte fáze.

## <a name="how-tooimplement-this-solution"></a>Jak tooimplement toto řešení
toosee způsob, jakým byste použili Azure Security Center ve scénáři s reakcí na incidenty, jsme budete postupujte podle kroků Monika je ve fázích hello detekovat a hodnocení a zjistěte, jaké Sam toodiagnose hello problém.

### <a name="detect-and-assess-incident-response-stages"></a>Fáze reakce na incidenty Detekce a Vyhodnocení
Monika přihlášení toohello portál Azure a funguje v konzole hello Security Center. V rámci jeho denně monitorování aktivit Jana spuštěna kontrola zabezpečení s vysokou prioritou hello výstrahy provedením následujících kroků:

1. Klikněte na tlačítko hello **výstrahy zabezpečení** dlaždice a přístup hello **výstrahy zabezpečení** okno.
    ![Okno Výstrahy zabezpečení](./media/security-center-incident-response/security-center-incident-response-fig4.png)

   > [!NOTE]
   > Za účelem hello tohoto scénáře je Monika probíhající tooperform posouzení na výstraha aktivity hello škodlivý SQL, jak je vidět v předchozích obrázek hello.
   >
   >
2. Klikněte na tlačítko hello **škodlivý SQL aktivity** výstrahy a zkontrolujte prostředky hello napadení v hello **škodlivý SQL aktivity** okno: ![podrobnosti incidentu](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    V tomto okně Monika trvat poznámky týkající se prostředků hello napadení, jak se stalo mnohokrát tento útok, a pokud byla zjištěna.
3. Klikněte na tlačítko hello **napadení prostředků** tooobtain Další informace o tento útok.

Po přečtení hello popis, Monika přesvědčeni, že to není falešně pozitivní a že Jana by měl Eskalovat tento případu tooSam.

### <a name="diagnose-incident-response-stage"></a>Fáze reakce na incidenty Diagnostika
SAM obdrží hello případ od Monika a spustí kontrola hello nápravy kroky, které navrhované Security Center.

![Životní cyklus reakce na incidenty](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Další zdroje
Hello reakcí na incidenty team můžete také využít výhod hello [Security Center Power BI](security-center-powerbi.md) schopností toosee různé typy sestav. Tyto sestavy můžete mu pomoct při další šetření toovisualize, analyzovat a filtrovat doporučení a výstrahy zabezpečení. Pro společnosti, které používají své informace o zabezpečení a řešení pro správu (SIEM) událost během procesu šetření hello, mohou také [Security Center integrovat řešení pro jejich](security-center-integrating-alerts-with-log-integration.md). Můžete také integrovat protokolů auditu Azure a virtuální počítač (VM) událostí zabezpečení pomocí hello [nástroj integrace Azure protokol](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/). tooinvestigate útoku, tyto informace můžete použít ve spojení s hello informace, které poskytuje Security Center.

## <a name="conclusion"></a>Závěr
Ty tým, než dojde k incidentu je velmi důležité tooyour organizace a ovlivní pozitivně zpracování incidentů. Který hello nástroje toomonitor prostředků může pomoci tento tým tootake přesné kroky tooremediate incidentu zabezpečení. Security Center [možností detekce](security-center-detection-capabilities.md) můžete pomůže IT tooquickly reakce toosecurity incidenty a problémy se zabezpečením.
