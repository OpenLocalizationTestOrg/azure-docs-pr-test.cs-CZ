---
title: "aaaProtect osobních dat pomocí Azure Security Center | Microsoft Docs"
description: "Ochrana osobních dat pomocí Azure security center"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 8e2c893d13318392f47fa912089d52618f9e7b45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a>Ochrana osobních údajů z narušení a útoky: Azure Security Center

Tento článek vám pomůže pochopit, jak toouse Azure Security Center tooprotect osobních dat od poruší a útoky.

## <a name="scenario"></a>Scénář 

Velké výletních společnosti, centrálou v USA, hello je rozšířit jeho itineráře toooffer operace v hello Středozemního a baltský moři, jakož i hello Britské ostrovy. toohelp přitom získala menší výletních Víceřádkový na základě v Itálii, Německo, Dánsko a hello Spojené království

Hello společnost používá Microsoft Azure toostore firemní data v cloudu hello. To zahrnuje osobní identifikovatelné údaje, jako je například jména, adresy, telefonních čísel a informace o kreditní kartě. Také obsahuje informace o lidských zdrojů, jako:

- Adresy
- telefonní čísla
- daň identifikační čísla
- lékařské informace

Hello výletních řádku také udržuje velké databáze potřebu a věrnost program členů. Zaměstnanci společnosti přístupu hello síti ze vzdálených pobočkách a agenty cesta hello společnosti nachází kolem hello, world mít přístup k prostředkům společnosti toosome.
Osobní data se přenáší po síti hello mezi těmito umístění a hello Microsoft datového centra.

## <a name="problem-statement"></a>Popis problému

Společnost Hello je zajímá hello riziko útoků na svých prostředků Azure. Chtějí tooprevent ohrožení zaměstnanců a zákazníků osob toounauthorized osobní data. Chtějí pokyny k zabránění a odpovědi nebo nápravná, jak efektivní způsob toomonitor hello probíhající zabezpečení cloudových prostředků.
Potřebují silné linií obrany před dnešní sofistikovaná a uspořádány útočníkům.

## <a name="company-goal"></a>Cílem společnosti

Jedním z cílů společnosti hello tooensure hello ochrany osobních údajů zaměstnanců a zákazníků osobních údajů je chrání před hrozbami. Jedním ze svých cílů je toorespond okamžitě toosigns porušení toomitigate hello dopad. Vyžaduje způsob tooassess hello aktuální stav zabezpečení, identifikovat citlivé konfigurace a opravit.

## <a name="solutions"></a>Řešení

Microsoft Azure Security Center (ASC) poskytuje do řešení pro správu zásad a monitorování integrované zabezpečení. Zajišťuje threat snadno použitelné a efektivní funkce prevence, zjišťování a odpovědi.

### <a name="prevention"></a>Prevention (Prevence)

ASC pomáhá zabránit narušení tím, že vám zásady zabezpečení tooset, poskytovat přístup za běhu a implementace doporučení zabezpečení.

Zásady zabezpečení definuje hello sadu ovládacích prvků, které jsou doporučené pro prostředky v rámci hello zadaný odběr. Přístup jenom na dobu lze použít toolock dolů tooyour příchozí přenosy virtuálních počítačích Azure, snižuje tooattacks ohrožení. Doporučení zabezpečení jsou vytvořené pomocí ASC po analýze hello stav zabezpečení vašich prostředků Azure.

#### <a name="how-do-i-set-security-policies-in-asc"></a>Jak nastavit zásady zabezpečení v ASC?

Zásady zabezpečení můžete nakonfigurovat pro každé předplatné. toomodify zásady zabezpečení, musí být roli vlastníka nebo přispěvatele daného předplatného. V hello portálu Azure hello následující:

1. Vyberte **zásad** hello ASC řídicím panelu.

2. Vyberte předplatné hello, na kterém chcete tooenable hello zásady.

3. Zvolte **zásada Zabránění** tooconfigure zásady podle předplatného. **Shromažďování dat z virtuálních počítačů** by mělo být nastavené příliš**na.**

4. V hello **zásada Zabránění** možnosti, vyberte **na** tooenable hello zabezpečení doporučení, které jsou relevantní pro předplatné hello.

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

Podrobné pokyny a vysvětlení jednotlivých doporučení hello zásady, které je možné povolit, najdete v části [nastavení zásad zabezpečení v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).

#### <a name="how-do-i-configure-just-in-time-access-jit"></a>Jak lze nakonfigurovat pouze v přístup čas (JIT)?

Pokud je povoleno JIT, Security Center zamyká tooyour příchozí přenosy virtuálních počítačů Azure ve vytvoření pravidla NSG. Vybrat hello porty na toowhich hello virtuálních počítačů bude uzamčené příchozí přenosy. toouse JIT přístup, hello následující:

1. Vyberte hello **těsně čas virtuálních počítačů přístup k dlaždici** v okně ASC hello.

2. Vyberte hello **doporučeno** kartě.

3. V části **virtuální počítače**, vyberte hello virtuálních počítačů, které chcete tooenable. To umístí další tooa zaškrtnutí virtuálních počítačů. 
4. Vyberte **povolení JIT** na virtuálních počítačích.
5. Vyberte **Uložit**.

Potom můžete zobrazit hello výchozí porty, které doporučuje ASC povolený pro JIT. Můžete také přidávat a konfigurovat nový port, na kterém chcete tooenable hello pouze v době řešení. Hello **těsně v čas virtuálních počítačů přístup** dlaždice v hello Security Center zobrazuje hello počet virtuálních počítačů nakonfigurovaná pro přístup za běhu. Také ukazuje hello počet požadavků schválené přístup provedené v hello minulého týdne.

![](media/protect-personal-data-azure-security-center/jit-vm.png)

Návod, jak toodo, a další informace o těsně v čas přístup, zobrazit [spravovat přístup k virtuálním počítačům pomocí právě v čase.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

#### <a name="how-do-i-implement-asc-security-recommendations"></a>Jak implementace doporučení zabezpečení ASC?

Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení. Hello doporučení vás provede procesem hello konfigurace hello potřebné ovládací prvky. 
1. Vyberte hello **doporučení** dlaždici na řídicím panelu ASC hello.

2. Zobrazit hello doporučení, které jsou uvedeny ve formátu tabulky, kde každý řádek představuje jeden doporučení.

3. Vyberte toofilter doporučení **filtru** a vyberte hodnoty závažnosti a stavu hello chcete toosee.

4. toodismiss doporučení, která se nedá použít, můžete klikněte pravým tlačítkem a vyberte **přeskočení.**

5. Vyhodnoťte, které doporučení by měla být použije první.

6. Použijte hello doporučení v pořadí podle priority.

Seznam možných doporučení a návodů na postupy najdete v části tooapply, [Správa doporučení zabezpečení v Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)

### <a name="detection-and-response"></a>Detekce a reakce

Detekce a reakce přejděte společně, jak chcete toorespond co nejdříve po zjištěno ohrožení.
Detekce hrozeb ASC funguje tak, že automaticky shromažďování informací o zabezpečení z vaše prostředky Azure, sítě hello a připojených partnerských řešení. ASC můžete rychle aktualizovat své detekční algoritmy, protože útočníci vydání nové a stále sofistikované zneužití. Podrobnější informace o tom, jak funguje služba detekce hrozeb je ASC, najdete v části [funkce zjišťování služby Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).

#### <a name="how-do-i-manage-and-respond-toosecurity-alerts"></a>Jak spravovat a reagovat výstrahy toosecurity?

Seznam upřednostňovaných výstrah zabezpečení se zobrazí v Centru zabezpečení společně s hello informace, které potřebujete tooquickly prozkoumat hello problém. Security Center také obsahuje doporučení, jak tooremediate útoku. toomanage výstrahy zabezpečení, proveďte následující hello:

1. Vyberte hello **výstrahy zabezpečení** dlaždici na řídicím panelu ASC hello. To obsahuje podrobnosti pro každou výstrahu.

2. Vyberte podle data, stav a závažnost výstrahy toofilter **filtru** a potom vyberte hodnoty hello chcete toosee.

3. Výstraha tooan toorespond, vyberte ho a zkontrolujte hello informace a potom vyberte hello prostředek, který byl napaden.

4. V hello **popis** poli se zobrazí podrobnosti, včetně doporučené nápravy.

![](media/protect-personal-data-azure-security-center/security-alerts.png)

Podrobné pokyny, odpovídá toosecurity výstrahy, najdete v části [správy a zda odpovídá toosecurity výstrahy v Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

Další informace v prošetřování výstrah zabezpečení hello společnosti může integrovat ASC výstrahy vlastní řešení SIEM pomocí [integrace se službou Azure protokolu](https://aka.ms/AzLog).

#### <a name="how-do-i-manage-security-incidents"></a>Jak lze spravovat incidenty zabezpečení?

V ASC je incidentu zabezpečení agregaci všechny výstrahy pro prostředek, které zarovnat kill řetězu vzory. Další informace o každém výskytu bude hello seznamu související výstrahy, což umožňuje vám tooobtain odhalit incidentu. Hello dlaždice výstrahy zabezpečení a okno zobrazí incidenty.

tooreview a spravovat incidenty zabezpečení, hello následující:

1. Vyberte hello **výstrahy zabezpečení** dlaždici. Pokud je zjištěn bezpečnostního incidentu, zobrazí se v grafu výstrahy zabezpečení hello. Bude mít ikonu, která se liší od dalších výstrah.

2. Vyberte hello incident toosee další podrobnosti o této incidentu zabezpečení. Další podrobnosti jsou jeho úplný popis jeho závažnost, jeho aktuální stav, hello napadení prostředků, postup nápravy hello hello incident a hello výstrahy, které byly součástí tohoto incidentu.

Můžete filtrovat toosee **pouze incidenty**, **výstrahy pouze**, nebo **i**.

#### <a name="how-do-i-access-hello-threat-intelligence-report"></a>Přístupu hello Threat Intelligence sestavy

ASC analyzují informace z více zdrojů tooidentify hrozeb. reakce na incidenty týmy tooassist prozkoumat ohrožení a oprava, Security Center obsahuje sestavu intelligence hrozba, která obsahuje informace o hello hrozba, která byla zjištěna.

Security Center má tři typy sestav hrozeb, které se může lišit na útok.
jsou k dispozici Hello sestavy:

- Sestava skupiny aktivit: obsahuje přímý dives do útočníci, jejich cíle a svoji.

- Sestava kampaně: zaměřuje se na podrobnosti o konkrétní útoku kampaně.

- Souhrnná sestava Threat: obsahuje všechny položky v předchozí dva sestavy hello.

Tento typ informací je velmi užitečná během procesu hello reakcí na incidenty, kde dojde k probíhající šetření toounderstand hello zdroji hello útoku, hello útočníka motivace a jaké toodo toomitigate-li tento problém přesunutí předat dál.

1. tooaccess hello analýzou hrozeb sestavy, hello následující:

2. Vyberte hello **výstrahy zabezpečení** dlaždici na řídicím panelu ASC hello.

3. Vyberte výstrahu zabezpečení hello, pro které chcete tooobtain Další informace.

4. V hello **sestavy** pole, klikněte na tlačítko hello odkaz toohello threat intelligence sestavy.

5. Otevře se soubor PDF hello, kterou si můžete stáhnout.

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

Další informace o hello ASC threat intelligence sestav najdete v tématu [Azure Security Center Threat Intelligence sestavy.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

### <a name="assessment"></a>Hodnocení

toohelp s testování, hodnocení a vyhodnocení lepšímu zabezpečení ASC poskytuje pro vyhodnocení integrované ohrožení zabezpečení s Qualys cloudu agentů, jako součást své doporučení součásti virtuálního počítače.

Hello Qualys agent hlásí ohrožení zabezpečení dat toohello Qualys platformy správy, které pak odešle ohrožení zabezpečení a sledování dat stavu zpět tooASC. Hello doporučení tooadd řešení pro vyhodnocení ohrožení zabezpečení se zobrazí v hello **doporučení** okno na řídicí panel ASC hello.

Po instalaci řešení pro vyhodnocení hello ohrožení zabezpečení na hello cílovém virtuálním počítači Security Center kontroly hello toodetect virtuálních počítačů a zjištění chyb zabezpečení, systém a aplikace. Zjistil problémy jsou uvedeny v části hello **virtuální počítače doporučení** možnost.

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a>Jak implementovat řešení pro vyhodnocení ohrožení zabezpečení? 

Pokud virtuální počítač nemá řešení pro vyhodnocení integrované ohrožení zabezpečení už nasazená, Security Center doporučuje ji nainstalovat.

1. Hello ASC řídicím panelu na hello **doporučení** vyberte **přidat řešení pro vyhodnocení ohrožení zabezpečení.**

2. Vyberte virtuální počítače hello místo, kam chcete řešení pro vyhodnocení tooinstall hello ohrožení zabezpečení.

3. Klikněte na **nainstalovat na virtuální počítače [počet].**

4. Vyberte jedno partnerské řešení v Azure Marketplace hello nebo pod **použít existující řešení** vyberte **Qualys.**

5. Můžete zapnout hello nastavení automatické aktualizace zapnout nebo vypnout v hello **partnerských řešení** okno.

Další pokyny najdete v části tooimplement řešení pro vyhodnocení ohrožení zabezpečení [vyhodnocení ohrožení zabezpečení v Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

## <a name="next-steps"></a>Další kroky

- [Úvodní příručka Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [Úvod tooAzure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [Integrace Azure Security Center výstrahy s integrací protokolů Azure](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [Nárůst Azure Security Center s vyhodnocení integrované ohrožení zabezpečení](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
