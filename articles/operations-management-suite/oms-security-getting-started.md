---
title: "aaaGetting začít s Operations Management Suite zabezpečení a Audit řešení | Microsoft Docs"
description: "Tento dokument pomůže tooget můžete začít s Operations Management Suite zabezpečení a Audit toomonitor možnosti řešení hybridního cloudu."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 754796ef-a43e-468a-86c9-04a2eda55b5b
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: get-started-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.openlocfilehash: 5cb3e5dbb3e60f9702a34c9413ddc1bf2b14b411
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Začínáme s řešením Zabezpečení a audit v Operations Management Suite
Tento dokument vám umožní rychle začít používat řešení Zabezpečení a audit v Operations Management Suite (OMS) a provede vás jednotlivými možnostmi.

## <a name="what-is-oms"></a>Co je OMS?
Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur. Další informace o OMS, přečtěte si článek hello [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>Řídicí panel Zabezpečení a audit v OMS
Hello OMS zabezpečení a Audit řešení poskytuje komplexní pohled na vaše organizace IT postavení zabezpečení s integrovaný vyhledávací dotazy pro významné problémy, které vyžadují vaši pozornost. Hello **zabezpečení a Audit** řídicí panel je hello domovskou obrazovku všem související toosecurity v OMS. Poskytuje podrobný přehled o stavu zabezpečení hello vašich počítačů. Zahrnuje také možnost tooview hello všechny události z hello za posledních 24 hodin, 7 dní, nebo jakékoli jiné vlastní časový rámec. tooaccess hello **zabezpečení a Audit** řídicího panelu, postupujte takto:

1. V hello **Microsoft Operations Management Suite** klikněte na hlavním řídicím **nastavení** dlaždice v levé hello.
2. V hello **nastavení** okno, v části **řešení** klikněte na tlačítko **zabezpečení a Audit** možnost.
3. Hello **zabezpečení a Audit** řídicího panelu se zobrazí:
   
    ![Řídicí panel Zabezpečení a audit v OMS](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Pokud přistupujete k tento řídicí panel pro hello poprvé a nemáte zařízení monitorovat pomocí OMS, nebude možné hello dlaždice naplnit data získaná z hello agenta. Po instalaci agenta hello může trvat některé toopopulate čas, proto se zobrazí původně pravděpodobně chybí některá data jako stále nahráváte toohello cloudu.  V takovém případě je normální toosee některé dlaždice bez konkrétní informace. Čtení [počítače se systémem Windows připojit přímo tooOMS](https://technet.microsoft.com/library/mt484108.aspx) Další informace o tom, agent OMS tooinstall v systému Windows a [tooOMS počítače připojit Linux](https://technet.microsoft.com/library/mt622052.aspx) Další informace o tom, tooperform této úlohy v systému Linux.

> [!NOTE]
> Hello agent shromažďuje informace hello na základě hello aktuální událostí, které jsou povolené, a pro instanci název počítače, IP adresy a uživatelského jména. Nejsou ale shromažďovány žádné soubory, dokumenty, databáze nebo soukromá data.   
> 
> 

Řešení jsou kolekce pravidel pro logiku, vizualizaci a získávání dat, které řeší klíčové problémy zákazníků. Zabezpečení a audit je jedním z nich, další mohou být přidána samostatně. Přečíst článek hello [přidat řešení](https://technet.microsoft.com/library/mt674635.aspx) Další informace o tom, tooadd nové řešení.

řídicí panel OMS zabezpečení a Audit Hello je uspořádány do čtyř hlavních kategorií:

* **Zabezpečení domény**: v této oblasti, nebudete moct toofurther prozkoumat záznamy zabezpečení v čase, přístup k posouzením malwaru, aktualizovat hodnocení, zabezpečení sítě, informace o přístupu a identit, počítače s událostmi zabezpečení a rychle mají řídicí panel Security Center tooAzure přístup.
* **Významné problémy**: tuto možnost povolit, tooquickly identifikovat hello počet aktivní problémy a hello závažnost těchto problémů.
* **Detekce (Preview)**: umožňuje vzorů útoků tooidentify vizualizací výstrahy zabezpečení, která je uloženo na vaše prostředky.
* **Hrozby Intelligence**: umožňuje vzorů útoků tooidentify vizualizací hello celkový počet servery s odchozím škodlivým provozem IP, typu hello zjištění ohrožení a mapu, která ukazuje, kde jsou tyto IP adresy pocházejících z. 
* **Běžné dotazy zabezpečení**: tuto možnost nabízí, můžete seznam nejčastějších zabezpečení hello dotazy, které můžete použít toomonitor prostředí. Po kliknutí na jednu z těchto dotazů, otevře se hello **vyhledávání** okno s hello výsledky pro tento dotaz.

> [!NOTE]
> Další informace o tom, jak OMS zajišťuje ochranu vašich dat, obsahuje článek Jak OMS chrání vaše data.
> 
> 

## <a name="security-domains"></a>Domény zabezpečení
Při monitorování prostředků, je důležité toobe možné tooquickly přístup hello aktuální stav vašeho prostředí. Je však také možné tootrack důležité toobe back se událostí, které nastaly v minulosti hello, který může vést lepší pochopení tooa, co se děje ve vašem prostředí v určitých bodu v čase. 

> [!NOTE]
> uchovávání dat se podle toohello OMS cenový plán. Další informace najdete v článku hello [Microsoft Operations Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) stránce s cenami.
> 
> 

Scénáře incidentu odpovědi a forenzních šetření přímo využívat hello výsledky, které jsou k dispozici v hello **záznamy zabezpečení v průběhu času** dlaždici.

![Záznamy zabezpečení v průběhu času](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Když kliknete na tuto dlaždici, hello **vyhledávání** se otevře okno zobrazující výsledku dotazu pro **události zabezpečení** (typ = SecurityEvents) s daty na základě hello posledních sedmi dnů, jak je uvedeno níže:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Záznamy zabezpečení v průběhu času](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

výsledek hledání Hello je rozdělený do dvou panelů: levém podokně hello vám dává rozpis hello počet událostí zabezpečení, které nebyly nalezeny, ve kterých byly nalezeny tyto události, hello počet účtů, které byly zjištěny v těchto počítačích a typy hello počítače hello aktivity. Hello pravém podokně poskytuje celkový počet výsledků hello a chronologickém zobrazení událostí zabezpečení hello aktivitou počítače hello název a události. Můžete také kliknout na **zobrazit další** tooview další podrobnosti o této události, jako jsou data události hello, hello ID události a zdroj události hello.

> [!NOTE]
> Další informace o vyhledávacích dotazech OMS najdete v [Referenční příručce k vyhledávání pro službu OMS](https://technet.microsoft.com/library/mt450427.aspx).
> 
> 

### <a name="antimalware-assessment"></a>Posouzení antimalwaru
Tato možnost dovoluje tooquickly můžete identifikovat počítače s nedostatečnou ochranou a počítače, které jsou nekompromitovali úsek malwaru. Malware vyhodnocení stavu a zjištěných hrozeb na hello monitorované servery jsou přečteny a pak hello data se odešlou toohello OMS služby v cloudu hello ke zpracování. Servery se zjištěnými hrozbami a servery s nedostatečnou ochranou se zobrazují na řídicím panelu malwaru assessment hello, která je přístupná po kliknutí na tlačítko hello **antimalwarových Assessment** dlaždici. 

![Posouzení malwaru](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Stejně jako všechny ostatní za provozu dlaždice k dispozici v OMS řídicího panelu, když na ni kliknete, hello **vyhledávání** otevře okno s výsledek dotazu hello. Pro tuto možnost, pokud kliknete v hello **nevykazují** možnost pod **stav ochrany**, budete mít hello výsledek dotazu, který ukazuje tato položka jeden, který obsahuje název hello počítače a jeho pořadí, jako vidíte níže:

![Výsledky vyhledávání](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [!NOTE]
> *pořadí* je úrovni poskytnutí tooreflect hello stav ochrany hello (on, off, aktualizovat, atd.) a hrozeb, které se nacházejí. S, jako číslo agregace toomake pomáhá.
> 
> 

Pokud kliknete na název počítače hello, budete mít hello chronologickém zobrazení hello stav ochrany pro tento počítač. To je velmi užitečné pro scénáře, ve kterých je nutné toounderstand Pokud antimalwarových hello je nainstalovaný a v určitém okamžiku byla odebrána.   

### <a name="update-assessment"></a>Posouzení aktualizací
Tato možnost dovoluje tooquickly můžete určit hello problémy se zabezpečením toopotential celkové ohrožení a jestli nebo jak důležité aktualizace jsou pro vaše prostředí. OMS zabezpečení a Audit řešení jenom zadejte hello vizualizace těchto aktualizací, hello reálná data pocházejí z [řešení pro správu aktualizací](oms-solution-update-management.md), což je jiný modul v OMS. Tady je příklad hello aktualizací:

![Aktualizace systému](./media/oms-security-getting-started/oms-getting-started-fig6-new.png)

> [!NOTE]
> Další informace o řešení Správa aktualizací najdete v tématu [Řešení Správa aktualizací v OMS](oms-solution-update-management.md).
> 
> 

### <a name="identity-and-access"></a>Identita a přístup
Identita by měla být hello řízení roviny pro vaši firmu, ochrana identity by měla být nejvyšší prioritu. V posledních hello byly okruhu kolem organizace a byly tyto hranice mezi hello primární Obranným hranice, v současné době s více dat a další aplikace přesunutí toohello cloudu stane hello identity nové hraniční hello. 

> [!NOTE]
> v současné době hello dat je založena pouze na data události zabezpečení přihlášení (událost ID 4624) v Office 365 přihlášení budoucí hello a také data služby Azure AD budou zahrnuty.
> 
> 

Pomocí monitorování aktivit vaší identity je bude možné tootake proaktivní akce před incident trvá místní nebo reaktivní akce toostop pokus útoku. Hello **identit a přístupu** řídicí panel poskytuje přehled o stavu vaší identity, včetně hello počet neúspěšných pokusů o přihlášení toolog na hello uživatelský účet, který jste použili při těmito pokusy účty, které byly uzamčen, účty se změněným nebo resetování hesla a je momentálně počet účtů, které se protokolují v. 

Když kliknete na hello **identit a přístupu** dlaždice, zobrazí se následující řídicí panel hello:

![Identita a přístup](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

Hello informace, které jsou k dispozici v tomto řídicím panelu můžete okamžitě pracovníkům tooidentify potenciální podezřelou aktivitu. Například 338 pokusy o toolog na nejsou jako **správce** a 100 % tyto pokusy se nezdařilo. Může to znamenat útok hrubou silou na tento účet. Pokud kliknete na tento účet bude získat další informace, které vám pomůže toodetermine hello cílový prostředek pro tento útok potenciální:

![Výsledky vyhledávání](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Hello podrobné sestavy obsahuje důležité informace o této události, včetně: hello cílového počítače, hello typ přihlášení (v této případu přihlášením k síti), hello aktivity (v tomto případu případě 4625) a komplexní časová osa každém pokusu o. 

### <a name="computers"></a>Počítače
Tuto dlaždici lze použít tooaccess všechny počítače, které mají aktivně události zabezpečení. Po kliknutí na tuto dlaždici uvidíte hello seznam počítačů s událostí zabezpečení a hello počet událostí na každém počítači:

![Počítače](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Můžete pokračovat kliknutím na každém počítači šetření a kontrolovat hello události zabezpečení, které byly příznakem.

### <a name="threat-intelligence"></a>Analýza hrozeb

Správci IT pomocí hello analýzou hrozeb možnost k dispozici v OMS zabezpečení a Audit můžete identifikovat bezpečnostní hrozby proti hello prostředí, například, zjistit, zda určitý počítač je součástí botnet. Uzly v botnet počítačů se může stát, když útočníci nedovoleným způsobem nainstaluje malware, který tajně připojí tento příkaz toohello počítače a řízení. Může také identifikovat potenciální hrozby přicházející z alternativních komunikačních kanálů, jako je například darknet. Další informace o analýzou hrozeb načtením [monitorování a odpovídá toosecurity výstrahy nástroje Operations Management Suite zabezpečení a Audit řešení](oms-security-responding-alerts.md) článku.

V některých scénářích si můžete všimnout potenciálně škodlivé IP adresy, ke které přistupoval jeden monitorovaný počítač:

![mapa analýzy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

Tato výstraha a ostatní uživatele v hello stejnou kategorií, jsou generovány prostřednictvím zabezpečení OMS s využitím [analýzou hrozeb Microsoftu](https://youtu.be/O4WtxgUrDc8). Hello analýzou hrozeb dat je shromažďovaná společností Microsoft a také zakoupených od začátku poskytovatelů intelligence hrozeb. Tato data se často aktualizuje a přizpůsobit přesunutí toofast hrozeb. Z důvodu tooits povahy, měly být spojeny s další zdroje informací o zabezpečení při [příčin](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) výstrahy zabezpečení. 

### <a name="baseline-assessment"></a>Vyhodnocení standardních hodnot

Společnost Microsoft, spolu s organizacemi z oboru a vládními organizacemi po celém světě, definuje konfiguraci systému Windows, která představuje vysoce zabezpečená nastavení serverů. Tuto konfiguraci tvoří sada klíčů registru, nastavení zásad auditu, nastavení zásad zabezpečení a společností Microsoft doporučené hodnoty pro tato nastavení. Tato sada pravidel se označuje jako standardní hodnoty zabezpečení. Další informace o této možnosti najdete v tématu [Vyhodnocování standardních hodnot v řešení Zabezpečení a audit v Operations Management Suite](oms-security-baseline.md).

### <a name="azure-security-center"></a>Azure Security Center
Tato dlaždice je v podstatě řídicí panel Azure Security Center tooaccess zástupce. Další informace o tomto řešení najdete v článku [Začínáme s Azure Security Center](../security-center/security-center-get-started.md).

## <a name="notable-issues"></a>Významné problémy
Hello hlavní záměr této skupiny z možností je tooprovide rychlý přehled o hello problémy, které máte ve vašem prostředí, je zařazením kritická, upozornění a informativní. Hello aktivní problém typ dlaždice je vizualizaci tyto problémy, ale jeho vám neumožňuje tooexplore další podrobnosti o nich, pro které je třeba toouse hello dolní části této dlaždice, který má název hello hello problému (název), kolik objekty měly to umožní (počet) a jak kritické je (ZÁVAŽNOST).

![Významné problémy](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Uvidíte, že byly tyto problémy již zahrnut v různých oblastech hello **zabezpečení domény** skupinu, která zvyšuje hello záměr tohoto zobrazení: vizualizovat hello nejdůležitější problémy ve vašem prostředí z jednoho místa.

## <a name="detections-preview"></a>Zjištění (Preview)
Hello hlavní záměr této možnosti je tooallow IT tooquickly identifikovat potenciální hrozby tootheir prostředí prostřednictvím a hello závažnost této hrozby.

![Analýza hrozeb](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Tuto možnost můžete také použít během [reakcí na incidenty šetření](https://blogs.msdn.microsoft.com/azuresecurity/2016/11/30/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) tooperform hello hodnocení a získat další informace o útoku hello.

> [!NOTE]
> Další informace o tom, toouse OMS pro reakcí na incidenty, si pusťte toto video: [jak tooLeverage hello Azure Security Center & Microsoft Operations Management Suite pro odpověď Incident](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703).
> 
> 

## <a name="threat-intelligence"></a>Analýza hrozeb
Hello nové hrozby intelligence část řešení zabezpečení a Audit hello vizualizuje vzorů možných útoků hello několika způsoby: Celkový počet servery s odchozím škodlivým provozem IP Dobrý den, hello typ zjištění ohrožení a mapu, která ukazuje, kde těchto IP adres pocházejících z. Můžete pracovat s hello mapy a klikněte na hello IP adresy pro další informace.

Žlutý pushpins na mapě hello znamenat příchozí provoz z škodlivé IP adresy. Není pro servery, které jsou zveřejněné toohello internet toosee příchozí škodlivý přenos, ale doporučujeme posoudit tyto pokusy o toomake se, že žádný z nich bylo úspěšné. Tyto indikátory jsou založené na protokolech služby IIS, WireData a brány Windows Firewall.  

![Analýza hrozeb](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Běžné dotazy zabezpečení
Hello seznam běžných dotazů zabezpečení k dispozici může být užitečné informace můžete toorapidly přístup k prostředku a upravit ho na základě potřeb vašeho prostředí. Mezi běžné dotazy patří:

* Všechny aktivity zabezpečení
* Aktivity související se zabezpečením na počítači hello "computer01.contoso.com" (nahraďte názvem svého počítače)
* Aktivity související se zabezpečením na počítači hello "computer01.contoso.com" pro účet "Administrator" (nahraďte názvem svého počítače a účtu)
* Aktivity přihlášení k počítači
* Účty, pod kterými byl na počítači ukončen antimalware Microsoftu
* Počítače, na kterých se ukončil antimalwarový proces od Microsoftu hello
* Počítače, na kterých spuštěn program "hash.exe" (nahraďte název procesu podle potřeby)
* Všechny názvy procesů, které byly spuštěny
* Aktivity přihlášení podle účtu
* Účty, které se vzdáleně připojily na počítači hello "computer01.contoso.com" (nahraďte názvem svého počítače)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste přináší tooOMS zabezpečení a Audit řešení. Další informace o zabezpečení OMS toolearn najdete hello následující články:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a výstrahy tooSecurity odpovídá v Operations Management Suite zabezpečení a Audit řešení](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)

