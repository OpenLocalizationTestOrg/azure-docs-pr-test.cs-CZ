---
title: "aaaTroubleshooting a monitorování systému SAP HANA v Azure (velké instance) | Microsoft Docs"
description: "Řešení potíží a monitorovat SAP HANA na Azure (velké instance)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1f1cd35820e227fd99af495431cd4b826aa53600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a>Jak tootroubleshoot a monitorování SAP HANA (velké instance) na Azure


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a>Monitorování v SAP HANA v Azure (velké instance)

SAP HANA v Azure (velké instance) se neliší od jiných IaaS nasazení – stačí toomonitor co hello operační systém a aplikace hello je dělat a jak tyto využívat hello následující prostředky:

- Procesor
- Memory (Paměť)
- Šířka pásma sítě
- Místo na disku

Jak pomocí Azure Virtual Machines, je nutné toofigure out jestli postačí hello třídy prostředků s názvem výše, nebo jestli tyto získat vyčerpané. Tady je podrobněji na každém z různých tříd hello:

**Využití prostředků procesoru:** hello poměr, která SAP definována pro určité úlohy proti HANA je vynucené toomake, že by měl být dostatek procesoru prostředky k dispozici toowork prostřednictvím hello data, která je uložená v paměti. Nicméně může být případech, kde HANA spotřebovává velké množství procesoru zpracování dotazů kvůli toomissing indexy nebo podobné problémy. To znamená, že je třeba sledovat využití prostředků procesoru hello HANA instance velké jednotky, jakož i spotřebovávají hello konkrétní HANA služby prostředků procesoru.

**Využití paměti:** je důležité toomonitor z v rámci HANA i mimo HANA na jednotce hello. V rámci HANA sledujte, jak hello dat spotřebovává přidělené paměti v toostay pořadí v rámci hello požadované změny velikosti pokyny SAP HANA. Chcete taky toomonitor využití paměti na hello velké Instance úrovně toomake se, že další nainstalovaný jiný HANA softwaru není využívat příliš mnoho paměti a proto pokouší s HANA paměti.

**Šířka pásma sítě:** brány virtuální sítě Azure hello je omezena šířka pásma dat Přesun do hello virtuální síť Azure, takže je vhodné hello toomonitor hello data přijatá všechny virtuální počítače Azure v rámci virtuální sítě toofigure na tom, jak zavřít jsou toohello omezení hello Azure Brána SKU, které jste vybrali. Na jednotce hello HANA velké Instance zkontrolujte smysl toomonitor příchozí a odchozí síťový provoz i a sledovat tookeep hello svazků, které jsou zpracovávány v čase.

**Místo na disku:** využívání místa na disku obvykle zvyšuje v čase. Existuje mnoho důvodů pro to, ale většina všech: datový svazek se zvyšuje, provádění zálohování transakčního protokolu, ukládání trasovacích souborů a provádění úložiště snímků. Proto je důležité toomonitor využití místa na disku a spravovat místo na disku hello spojené s jednotkou HANA velké Instance hello.

## <a name="monitoring-and-troubleshooting-from-hana-side"></a>Monitorování a řešení potíží s ze strany HANA

V pořadí tooeffectively analyzovat problémy související tooSAP HANA v Azure (velké instance), je užitečné toonarrow dolů hello hlavní příčinu problému. SAP publikovali velké množství toohelp dokumentaci vám.

Použít nejčastější dotazy související tooSAP HANA výkonu najdete v následující poznámky k SAP hello:

- [Poznámka SAP #2222200 – nejčastější dotazy: SAP HANA sítě](https://launchpad.support.sap.com/#/notes/2222200)
- [Poznámka SAP #2100040 – nejčastější dotazy: SAP HANA procesoru](https://launchpad.support.sap.com/#/notes/0002100040)
- [Poznámka SAP #199997 – nejčastější dotazy: SAP HANA paměti](https://launchpad.support.sap.com/#/notes/2177064)
- [Poznámka SAP #200000 – nejčastější dotazy: Optimalizace výkonu SAP HANA](https://launchpad.support.sap.com/#/notes/2000000)
- [Poznámka SAP #199930 – nejčastější dotazy: SAP HANA vstupně-výstupních operací analýzy](https://launchpad.support.sap.com/#/notes/1999930)
- [Poznámka SAP #2177064 – nejčastější dotazy: SAP HANA službu restartovat a dojde k chybě](https://launchpad.support.sap.com/#/notes/2177064)

**Výstrahy SAP HANA**

Jako první krok protokolech hello aktuální SAP HANA výstrahy. V systému SAP HANA Studio přejděte příliš**konzole pro správu: upozornění: Zobrazit: všechny výstrahy**. Na této kartě se zobrazí všechny výstrahy SAP HANA pro konkrétní hodnoty (Volná fyzická paměť, využití procesoru atd.), které spadají mimo hello nastavit minimální a maximální prahové hodnoty. Ve výchozím nastavení kontroluje se automaticky aktualizují každých 15 minut.

![V systému SAP HANA Studio přejděte tooAdministration konzoly: upozornění: zobrazení: všechny výstrahy](./media/troubleshooting-monitoring/image1-show-alerts.png)

**VYUŽITÍ PROCESORU**

Pro aktivaci z důvodu nastavení prahové hodnoty tooimproper výstrahu řešení je tooreset toohello výchozí hodnota nebo přijatelnější prahovou hodnotu.

![Obnovit výchozí hodnotu toohello nebo přijatelnější prahová hodnota](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

Hello následující upozornění může znamenat problémy prostředků procesoru:

- Využití procesoru hostitele (výstrah 5)
- Poslední operace úložného bodu (výstrah 28)
- Doba trvání uloženého bodu (výstrah 54)

Můžete si všimnout vysoké spotřeby procesoru ve vaší databázi SAP HANA z jednoho z následujících hello:

- Výstrahy 5 (využití procesoru hostitele) se vyvolá pro aktuální nebo posledních využití procesoru
- Hello zobrazené na úvodní obrazovka přehled využití procesoru

![Zobrazí využití procesoru na úvodní obrazovka Přehled](./media/troubleshooting-monitoring/image3-cpu-usage.png)

Hello zatížení graf může zobrazit vysoké využití procesoru, nebo vysoké spotřebu v posledních hello:

![Hello zatížení graf může zobrazit vysoké využití procesoru, nebo vysoké spotřebu v posledních hello](./media/troubleshooting-monitoring/image4-load-graph.png)

Výstraha aktivuje kvůli využití toohigh procesoru může být způsobeno několik důvodů, včetně, mimo jiné: provádění určitých transakcí, načítání dat, ukotvených úloh, dlouho běžící příkazů jazyka SQL a výkonu chybných dotazů (například s BW na HANA datové krychle).

Odkazovat toohello [řešení potíží s SAP HANA: související způsobí, že využití procesoru a řešení](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) lokality podrobné kroky řešení problémů.

**Operační systém**

Jedním z nejdůležitějších hello kontrol pro SAP HANA v systému Linux je toomake jistotu, že transparentní obrovské stránky je zakázáno najdete v tématu [SAP Poznámka #2131662 – transparentní obrovské stránky (THP) na serverech SAP HANA](https://launchpad.support.sap.com/#/notes/2131662).

- Můžete zkontrolovat, zda transparentní obrovské stránky jsou povoleny prostřednictvím hello následující příkaz Linux: **cat /sys/kernel/mm/transparent\_hugepage/povoleno**
- Pokud _vždy_ je uzavřený v závorkách, jak je uvedeno níže, znamená, že jsou povoleny hello transparentní obrovské stránky: [vždy] madvise nikdy; Pokud _nikdy_ je uzavřený v závorkách, jak je uvedeno níže, znamená to, že hello průhledný Velký stránky jsou zakázány: vždy madvise [nikdy]

Následující příkaz Linux Hello by měla vrátit nic: **ot. / min - qa | grep ulimit.** Pokud se zobrazí _ulimit_ je nainstalovaný, odinstalovat okamžitě.

**Paměť**

Můžete pozorovat tato šířka hello paměti přidělené hello SAP HANA databáze je vyšší, než se očekávalo. Hello následující výstrahy signalizují potíže s velké množství paměti:

- Využití hostitele fyzické paměti (výstrahy 1)
- Využití paměti název serveru (upozornění 12)
- Celkové využití paměti úložiště sloupec tabulky (výstrah 40)
- Využití paměti služby (výstrah 43)
- Využití paměti hlavní úložiště sloupec úložiště tabulek (45 výstrah)
- Soubory modulu runtime výpisu (výstrah 46)

Odkazovat toohello [řešení potíží s SAP HANA: problémy s pamětí](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) lokality podrobné kroky řešení problémů.

**Síť**

Odkazovat příliš[SAP Poznámka #2081065 – řešení potíží s SAP HANA sítě](https://launchpad.support.sap.com/#/notes/2081065) a proveďte kroky v této Poznámka SAP řešení potíží se sítí hello.

1. Analýza odezvy čas mezi serverem a klientem.
  A. Spuštění skriptu SQL hello [ _HANA\_sítě\_klienti_](https://launchpad.support.sap.com/#/notes/1969700)_._
  
2. Analýza internodium komunikace.
  A. Spuštění skriptu SQL [ _HANA\_sítě\_služby_](https://launchpad.support.sap.com/#/notes/1969700)_._

3. Spusťte příkaz Linux **ifconfig** (hello výstup ukazuje, pokud se vyskytnou ztráty paketů).
4. Spusťte příkaz Linux **tcpdump**.

Můžete také použít s otevřeným zdrojem hello [IPERF](https://iperf.fr/) nástroje (nebo podobnou) výkon sítě toomeasure reálné aplikaci.

Odkazovat toohello [řešení potíží s SAP HANA: výkon sítě a problémů s připojením k](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) lokality podrobné kroky řešení problémů.

**Úložiště**

Z hlediska koncový uživatel aplikace (nebo hello systém jako celek) běží pomalu, neodpovídá nebo můžete i zdá se, že toohang Pokud dochází k potížím s výkonem vstupně-výstupní operace. V hello **svazky** kartě v SAP HANA Studio uvidíte hello připojené svazky a svazky, které jsou používány jednotlivých služeb.

![Na kartě hello svazky v SAP HANA Studio vidíte, že hello připojené svazky a svazky, které jsou používány každé služby](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

Připojené svazky v dolní části hello úvodní obrazovka, které se zobrazí podrobnosti o hello svazků, jako jsou soubory a vstupně-výstupních operací statistiky.

![Připojené svazky v dolní části hello úvodní obrazovka, které se zobrazí podrobnosti o hello svazků, jako jsou soubory a vstupně-výstupních operací statistiky](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

Odkazovat toohello [řešení potíží s SAP HANA: vstupně-výstupních operací souvisejících hlavní příčiny a řešení](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) a [řešení potíží s SAP HANA: disku související hlavní příčiny a řešení](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) lokality podrobné kroky řešení problémů.

**Diagnostické nástroje**

Provést kontrolu stavu SAP HANA prostřednictvím HANA\_konfigurace\_Minichecks. Tento nástroj vrátí potenciálně kritickým technických problémů, které by měl mít bylo vyvoláno už jako výstrahy v SAP HANA Studio.

Odkazovat příliš[SAP Poznámka #1969700 – kolekce příkaz SQL pro SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) a stáhnout hello SQL Statements.zip souboru připojené toothat Poznámka. Uložte tento soubor .zip na místní pevný disk hello.

V nástroji Studio SAP HANA na hello **informace o systému** kartě, klikněte pravým tlačítkem na hello **název** sloupce a vyberte **příkazů SQL Import**.

![V nástroji Studio SAP HANA na kartě hello informace o systému klikněte pravým tlačítkem na název sloupce hello a vyberte Import SQL příkazy](./media/troubleshooting-monitoring/image7-import-statements-a.png)

Vyberte hello SQL Statements.zip souboru uložené místně a budou importovány do složky s hello odpovídajících příkazů SQL. V tomto okamžiku hello mnoho různých diagnostiky kontroly můžete spustit pomocí těchto příkazů SQL.

Například nároky na šířku pásma replikaci systému SAP HANA tootest, klikněte pravým tlačítkem na hello **šířky pásma** příkaz pod **replikace: šířky pásma** a vyberte **otevřete** v konzole SQL.

příkaz SQL Hello otevře povolení toobe vstupních parametrů (změna oddílu), změnit a potom spuštěn.

![příkaz SQL Hello otevře změnit a potom spuštěn povolení toobe vstupních parametrů (Změna část)](./media/troubleshooting-monitoring/image8-import-statements-b.png)

Dalším příkladem je klikněte pravým tlačítkem na v příkazech hello pod **replikace: Přehled**. Vyberte **Execute** hello místní nabídce:

![Dalším příkladem je klikněte pravým tlačítkem na v příkazech hello pod replikace: Přehled. Vyberte možnost spustit místní nabídce hello](./media/troubleshooting-monitoring/image9-import-statements-c.png)

Výsledkem je informace, které pomáhají při řešení problémů:

![Tato akce způsobí informace, které pomáhají při řešení problémů](./media/troubleshooting-monitoring/image10-import-statements-d.png)

Hello stejné pro HANA\_konfigurace\_Minichecks a zkontrolujte všechny _X_ značky v hello _C_ sloupce (kritické).

Ukázka výstupu:

**HANA\_konfigurace\_MiniChecks\_Rev102.01 + 1** pro kontroluje obecné SAP HANA.

![HANA\_konfigurace\_MiniChecks\_Rev102.01 + 1 pro obecné kontroly SAP HANA](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

**HANA\_služby\_přehled** přehled co SAP HANA aktuálně běží služby.

![HANA\_služby\_přehled přehled co SAP HANA aktuálně běží služby](./media/troubleshooting-monitoring/image12-services-overview.png)

**HANA\_služby\_statistiky** pro SAP HANA služby informace (procesor, paměť, atd.).

![HANA\_služby\_statistiku pro SAP HANA informace o služby ](./media/troubleshooting-monitoring/image13-services-statistics.png)

**HANA\_konfigurace\_přehled\_Rev110 +** obecné informace o instanci hello SAP HANA.

![HANA\_konfigurace\_přehled\_Rev110 + obecné informace o instanci hello SAP HANA](./media/troubleshooting-monitoring/image14-configuration-overview.png)

**HANA\_konfigurace\_parametry\_Rev70 +** toocheck parametry SAP HANA.

![HANA\_konfigurace\_parametry\_parametry SAP HANA toocheck Rev70 +](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

