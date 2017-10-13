---
title: "Síťová řešení pro sledování výkonu v Azure Log Analytics | Microsoft Docs"
description: "Sledování výkonu sítě v Azure Log Analytics vám pomůže s monitorováním výkonu vaší sítě-in téměř real-bránu ke zjištění a vyhledejte síťové kritické body."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.openlocfilehash: c6568e491429f6046ab164ab5eacd0ae5846e201
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="network-performance-monitor-solution-in-log-analytics"></a>Síťová řešení pro sledování výkonu v analýzy protokolů

![Symbol sledování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-symbol.png)

Tento dokument popisuje, jak nastavit a používání řešení monitorování výkonu sítě v analýzy protokolů, které vám pomůže s monitorováním výkonu vaší sítě-in téměř real-bránu ke zjištění a vyhledejte sítě kritické body. Pomocí tohoto řešení pro sledování výkonu sítě může sledovat ztrátě a čekací doba mezi dvěma sítěmi, podsítě nebo servery. Sledování výkonu sítě zjistí problémy se síťovým jako blackholing provoz, směrování chyb a problémů, které metody monitorování konvenční sítě nejsou schopna zjistit. Sledování výkonu sítě generuje výstrahy a upozorní jak a kdy je nedodržení prahovou hodnotu pro síťové propojení. Tyto prahové hodnoty je možné zjistit automaticky systém nebo můžete nakonfigurovat, aby uživatelé používali vlastní pravidla výstrah. Sledování výkonu sítě zajišťuje možno včas zjistit problémy s výkonem sítě a lokalizováno zdroj problému na určitém segmentu sítě nebo zařízení.

Můžete zjistit problémy se síťovým s řídicí panel řešení, která zobrazuje souhrnné informace o vaší síti, včetně poslední události stavu sítě, není v pořádku síťová propojení a propojení podsítí, se kterým se setkávají ztráta paketů vysoké a latenci. Vám může procházení do síťové propojení, chcete-li zobrazit aktuální stav propojení podsítí, jakož i-uzly odkazy. Můžete také zobrazit Historický trend ztrátě a latence na síť, podsíť a na úrovni mezi uzly. Můžete zjistit problémy přechodný síťový zobrazením historických trendů grafy pro ztráta paketů a latence a vyhledejte síťové kritických bodů na mapě topologie. Interaktivní topologie graf vám umožňuje vizualizovat tras do segmentu směrování sítě a určete zdroj problému. Třeba dalších řešení můžete použít vyhledávání protokolu pro různé požadavky na analytics vytvořit vlastní sestavy založené na data shromažďovaná společností sledování výkonu sítě.

Toto řešení využívá syntetické transakce jako primární mechanismus ke zjištění chyb sítě. Ano můžete použít bez ohledu na dodavatele nebo model specifické síťové zařízení. Funguje napříč místními cloudu (IaaS) a hybridní prostředí. Řešení automaticky rozpozná síťové topologie a různé tras ve vaší síti.

Monitorování produkty typickou síťovou soustředit na monitorování stavu síťových zařízení (směrovače, přepínače atd.), ale nezadáte přehledy skutečné kvality síťové připojení mezi dvěma body, které se nástroj Sledování výkonu sítě.

### <a name="using-the-solution-standalone"></a>Pomocí samostatné řešení
Pokud chcete monitorovat kvalitu síťové připojení mezi jejich kritickým úlohám, sítě, datových centrech nebo lokalitách office a pak je můžete použít řešení monitorování výkonu síťového samostatně pro sledování stavu připojení mezi:

* víc datových centrech nebo office lokalit, které jsou připojené pomocí veřejné nebo soukromé síti
* kritickým úlohám, které jsou spuštěné obchodních aplikací
* veřejné služby cloudového jako Microsoft Azure nebo Amazon Web Services (AWS) a místní sítě, pokud máte IaaS (VM) k dispozici a budete mít brány konfigurován, aby umožňoval komunikaci mezi místními sítěmi a sítěmi cloudu
* Při použití Express Route Azure a místní sítě

### <a name="using-the-solution-with-other-networking-tools"></a>Řešení pomocí jiné síťové nástroje
Pokud chcete monitorovat řádek obchodní aplikace, můžete jako doprovodné řešení k jiné síti nástroje řešení monitorování výkonu sítě. Pomalé síti může vést k pomalé aplikace a sledování výkonu sítě může vám pomůže prozkoumat problémy s výkonem aplikace, které jsou způsobeny základní problémům se sítí. Protože řešení nevyžaduje žádné přístup k síťovým zařízením, nemusí správce aplikací závisí na síťové tým k zadání informací o síti jak ovlivňuje aplikace.

Navíc pokud již investovat do jiné sítě, nástroje pro sledování, pak řešení můžete doplnit tyto nástroje protože většina tradiční řešení pro monitorování sítě neposkytuje přehledy metriky výkonu začátku do konce sítě jako ztrátě a latenci.  Sledování výkonu sítě řešení může pomoct této mezeru.

## <a name="installing-and-configuring-agents-for-the-solution"></a>Instalování a konfigurování agentů pro řešení
Použijte k instalaci agentů na základní procesy [počítače se systémem Windows se připojit k analýze protokolů](log-analytics-windows-agents.md) a [připojení nástroje Operations Manager k analýze protokolů](log-analytics-om-agents.md).

> [!NOTE]
> Budete muset nainstalovat aspoň 2 agenty, abyste měli dostatek dat ke zjišťování a monitorování síťových prostředků. V opačném řešení zůstane v konfiguraci stavu, dokud instalaci a konfiguraci dalších agentů.
>
>

### <a name="where-to-install-the-agents"></a>Umístění instalace agentů
Před instalací agentů, vezměte v úvahu topologii vaší sítě a které části sítě, kterou chcete sledovat. Doporučujeme nainstalovat více než jeden agenta pro každou podsíť, kterou chcete sledovat. Jinými slovy pro každou podsíť, kterou chcete monitorovat, zvolte jeden nebo víc serverech nebo virtuálních počítačů a nainstalujte agenta na ně.

Pokud si nejste jistí o topologii sítě, nainstalujte agenty na serverech s kritickým úlohám, které chcete monitorovat výkon sítě. Například můžete chtít udržování přehledu o síťové připojení mezi webového serveru a serveru se systémem SQL Server. V tomto příkladu by instalace agenta na obou serverech.

Agenti monitorují připojení k síti (odkazy) mezi hostiteli – není hostitelů sami. Tím, že ke sledování síťového propojení, je nutné nainstalovat agenty na oba koncové body tento odkaz.

### <a name="configure-agents"></a>Konfigurace agentů

Pokud máte v úmyslu používat protokol ICMP pro syntetické transakce, budete muset povolit následující pravidla brány firewall pro spolehlivě využitím ICMP:

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow
```


Pokud máte v úmyslu použít protokol TCP, je třeba otevřít porty brány firewall pro počítače, aby mohl komunikovat agenty. Je třeba stažení a spuštění [skript prostředí PowerShell EnableRules.ps1](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) bez parametrů v okně prostředí PowerShell s oprávněními správce.

Tento skript vytvoří klíčům registru požadovaným nástroj Sledování výkonu sítě a vytvoří pravidla brány firewall systému Windows povolit agentů k vytvoření připojení TCP mezi sebou. Klíče registru vytvořený skript také určete, zda protokoly pro ladění a cesta k souboru protokoly protokolu. Definuje také agent TCP port používaný pro komunikaci. Hodnoty pro tyto klíče se nastaví automaticky ve skriptu, takže byste neměli měnit ručně tyto klíče.

Port otevřít ve výchozím nastavení je 8084. Můžete použít vlastní port poskytnutím parametru `portNumber` skriptu. Však stejný port by měl použít na všech počítačích, kde je skript spuštěn.

> [!NOTE]
> Skript EnableRules.ps1 nakonfiguruje pravidla brány firewall systému Windows pouze v počítači, kde je skript spuštěn. Pokud máte síťovou bránu firewall, měli byste si ověřit, že umožňuje, aby provoz určený pro port TCP používán sledování výkonu sítě.
>
>

## <a name="configuring-the-solution"></a>Konfiguruje se řešení
Použijte následující informace k instalaci a konfiguraci řešení.

1. Řešení pro sledování výkonu sítě získá data z počítačů se systémem Windows Server 2008 s aktualizací SP 1 nebo novější a Windows 7 SP1 nebo novější, které jsou stejné požadavky jako Microsoft Monitoring Agent (MMA). Agenti NPM můžete spustit také na ploše nebo klientských operačních systémech Windows (Windows 10, Windows 8.1, Windows 8 a Windows 7).
    >[!NOTE]
    >Agenti pro operační systémy Windows server podporují TCP a ICMP jako protokoly pro syntetické transakce. Agenti pro klientské operační systémy Windows, ale podporují pouze ICMP jako protokol pro syntetické transakce.

2. Přidat řešení monitorování výkonu sítě do pracovního prostoru z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).  
   ![Symbol sledování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. V portálu OMS, zobrazí se nová dlaždice s názvem **sledování výkonu sítě** se zprávou *řešení vyžaduje další konfiguraci*. Budete muset konfigurovat řešení přidat sítě na základě podsítě a uzly, které jsou zjištěny agenty. Klikněte na tlačítko **sledování výkonu sítě** pro spuštění konfigurace výchozí sítí.  
   ![řešení vyžaduje další konfiguraci.](./media/log-analytics-network-performance-monitor/npm-config.png)

### <a name="configure-the-solution-with-a-default-network"></a>Konfigurace řešení s výchozí sítě
Na stránce konfigurace se zobrazí jediná síť s názvem **výchozí**. Když nebyly definovány žádné sítě, všechny automaticky zjistit podsítě jsou umístěny v síti výchozí.

Vždy, když vytvoříte síť, přidáte podsíť a této podsíti se odebere z výchozí sítí. Pokud odstraníte síť, všechny jeho podsítě se automaticky vrátí k výchozí síti.

Jinými slovy výchozí sítí je kontejner pro všechny podsítě, které nejsou obsaženy v žádné uživatelem definované síti. Nelze upravit nebo odstranit výchozí sítí. Vždy zůstává v systému. Ale můžete vytvořit libovolný počet sítí, jako je třeba.

Ve většině případů podsítí ve vaší organizaci budou uspořádané do víc než jedna síť a je třeba vytvořit jednu nebo více sítí k logickému seskupení podsítě.

### <a name="create-new-networks"></a>Vytvořit nové sítě
Síť v nástroji Sledování výkonu sítě je kontejner pro podsítě. Vytvoření sítě s libovolný název a přidejte podsítě k síti. Například můžete vytvořit síť s názvem *Building1* a poté přidejte podsítě, nebo můžete vytvořit síť s názvem *DMZ* a poté přidejte všechny podsítě, které patří do demilitarizovaná zóna k této síti.

#### <a name="to-create-a-new-network"></a>Chcete-li vytvořit nové sítě
1. Klikněte na tlačítko **sítě přidat** a pak zadejte název sítě a popis.
2. Vyberte jednu nebo více podsítí a potom klikněte na **přidat**.
3. Klikněte na tlačítko **Uložit** konfiguraci uložíte.  
   ![Přidejte síť](./media/log-analytics-network-performance-monitor/npm-add-network.png)

### <a name="wait-for-data-aggregation"></a>Počkejte agregace dat
Po uložení konfigurace poprvé, řešení spustí shromažďování paketu ztrátě a latence informací o síti mezi uzly, na kterém jsou nainstalované agenty. Tento proces může chvíli trvat, někdy 30 minut. Při tomto stavu, dlaždici sledování výkonu sítě na stránce Přehled zobrazí zpráva s oznámením *agregace dat v procesu*.

![probíhající agregace dat](./media/log-analytics-network-performance-monitor/npm-aggregation.png)

Když odeslal data, se zobrazí data zobrazující dlaždice aktualizovat nástroj Sledování výkonu sítě.

![Dlaždice monitorování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-tile.png)

Klikněte na dlaždici zobrazení řídicího panelu monitorování výkonu sítě.

![Řídicí panel monitorování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Upravte nastavení monitorování pro podsítě
Všechny podsítě, kde byl nainstalován alespoň jeden agent jsou uvedeny na **podsítě** na stránce konfigurace kartu.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Pokud chcete povolit nebo zakázat monitorování pro konkrétní podsítě
1. Vyberte nebo zaškrtněte políčko vedle položky **podsítí ID** a potom ověřte, že **použijte pro monitorování** je vybrané nebo nezaškrtnuté, podle potřeby. Můžete zaškrtněte nebo zrušte více podsítí. Pokud je zakázáno, nejsou monitorována podsítě jako agenti se aktualizují na zastavení otestováním jiné agenty.
2. Vyberte uzly, které chcete monitorovat pro konkrétní podsíť podsítí výběrem ze seznamu a přesun požadované uzly mezi seznamy obsahující sledována a sledované uzly.
   Můžete přidat vlastní **popis** do podsítí, pokud chcete.
3. Klikněte na tlačítko **Uložit** konfiguraci uložíte.  
   ![Upravit podsíť](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Zvolte uzlů k monitorování
Všechny uzly, které nemá agenta nainstalovaného na jejich jsou uvedeny v **uzly** kartě.

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Pokud chcete povolit nebo zakázat monitorování pro uzly
1. Vyberte nebo zrušte uzly, které chcete sledovat nebo zastavení monitorování.
2. Klikněte na tlačítko **použijte pro monitorování**, nebo vymazat, podle potřeby.
3. Klikněte na **Uložit**.  
   ![Povolit monitorování uzlu](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)

### <a name="set-monitoring-rules"></a>Sada pravidel monitorování
Sledování výkonu sítě generuje události stavu týkající se možností připojení mezi dvěma uzly nebo podsíť nebo síť odkazy při porušení zabezpečení nebyla prahovou hodnotu. Tyto prahové hodnoty je možné zjistit automaticky v systému nebo je můžete nastavit vlastní pravidla výstrah.

*Výchozí pravidlo* je vytvořeno systémem a vytvoří událost stavu vždy, když ke ztrátě nebo latenci mezi žádném páru sítí nebo podsítí odkazy narušení prahovou hodnotu naučili systému. Můžete vytvořit vlastní pravidla monitorování a zakázat výchozí pravidlo

#### <a name="to-create-custom-monitoring-rules"></a>Chcete-li vytvořit vlastní pravidla monitorování
1. Klikněte na tlačítko **přidat pravidlo** v **monitorování** kartě a zadejte název pravidla a popis.
2. Vyberte dvojice síť nebo podsíť odkazy na monitorování seznamů.
3. Nejprve vyberte síť, ve kterém se nachází první podsíť/s, které vás zajímají z rozevíracího seznamu síť a potom vyberte z rozevíracího seznamu odpovídající podsítí podsítí/s.
   Vyberte **všechny podsítě** Pokud chcete monitorovat všechny podsítě v síťové propojení. Vyberte podobně jako jiných podsítí nebo s zájmu. A, můžete kliknout na **přidat výjimky** vyloučit monitorování pro konkrétní podsítí odkazy z výběru, které jste udělali.
4. Volba mezi ICMP a TCP protokoly pro provádění syntetické transakce.
5. Pokud nechcete, aby k vytvoření události stavu pro položky, které jste vybrali, poté zrušte **povolit sledování stavu na odkazy předmětem toto pravidlo**.
6. Vyberte sledování podmínek.
   Můžete nastavit vlastní prahové hodnoty pro generování událostí stavu zadáním prahové hodnoty. Vždy, když je hodnota stavu překročí jeho vybraná prahová hodnota pro dvojici vybrané sítě nebo podsítí, je generována událost stavu.
7. Klikněte na tlačítko **Uložit** konfiguraci uložíte.  
   ![Vytvoření vlastního pravidla monitorování](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)

Po uložení monitorování pravidlo, můžete integrovat daného pravidla s správu výstrah kliknutím **vytvoření výstrahy**. Pravidlo výstrahy se automaticky vytvoří pomocí vyhledávacího dotazu a další potřebné parametry automaticky vyplněné. Pravidlo výstrahy může přijímat e mailové výstrahy, kromě existující výstrahy v rámci NPM. Výstrahy můžete spustit taky nápravné akce pomocí runbooků nebo můžete integrovat stávajících řešení pro správu služby pomocí webhooků. Můžete kliknout na **Spravovat výstrahy** upravit nastavení výstrah.

### <a name="choose-the-right-protocol-icmp-or-tcp"></a>Vyberte správné protokol ICMP nebo TCP

Monitorování výkonu v síti (NPM) používá k výpočtu metriky výkonu sítě jako paketu ztrátě a odkaz latence syntetické transakce. To lépe pochopit, zvažte agenta NPM připojené k jednom konci síťového propojení. Tento agent NPM odesílá pakety testu druhý Agent NPM připojené k jiné konec sítě. Druhý agenta odpoví odpovědí s odpovědí na žádost. Tento proces se opakuje několikrát. Měřením počet odpovědi a čas potřebný k přijetí každé odpovědi prvního agenta NPM vyhodnocuje odkaz latence a zahazování paketů.

Formát, velikost a pořadí tyto pakety je určen podle protokol, který zvolíte, při vytváření pravidla monitorování. Na základě protokolu paketů, mezilehlých síťových zařízení (směrovače, přepínače atd.) může zpracovat tyto pakety jinak. V důsledku toho zvoleného protokol ovlivní přesnost výsledků. A, vaše volba protokol také určuje, zda je nutné provést jakékoli ruční kroky, poté, co nasadíte řešení NPM.

NPM nabízí volba mezi protokoly ICMP a TCP pro provádění syntetické transakce.
Pokud si zvolíte ICMP, při vytváření pravidla syntetické transakce, agenti NPM ODEZVU protokolu ICMP zprávy použít k výpočtu latence sítě a ztráta paketů. ODEZVU protokolu ICMP používá stejnou zprávu, kterou posílá konvenční nástroj Ping. Použijete-li jako protokol TCP, NPM agenti odesílají paket TCP SYN přes síť. Následují TCP handshake dokončení a potom odebírání připojení pomocí RVNÍ paketů.

#### <a name="points-to-consider-before-choosing-the-protocol"></a>Body, které je třeba zvážit před volbou protokol
Před vyberte protokol, který má používat, zvažte následující informace:

##### <a name="discovering-multiple-network-routes"></a>Zjišťování několik síťových tras
TCP je přesnější při zjišťování víc tras a je menší počet agentů v jednotlivých podsítích. Jeden nebo dva agenty pomocí protokolu TCP můžete například zjistit všechny redundantní cesty mezi podsítěmi. Musíte však několik agenty pomocí protokolu ICMP a dosáhnout tak podobné. Pomocí protokolu ICMP, pokud máte *N* počet tras mezi dvěma podsítěmi, budete potřebovat víc než 5*N* agentů ve zdrojové nebo cílové podsíti.

##### <a name="accuracy-of-results"></a>Přesnost výsledků
Směrovače a přepínače zpravidla přiřadit ODEZVU protokolu ICMP pakety ve srovnání s TCP pakety s nižší prioritou. V některých situacích při síťová zařízení jsou velkém zatížení, data získat TCP přesněji odráží ztrátě a latence, které aplikace. K tomu dochází, protože většina dat aplikace probíhá přes protokol TCP. V takových případech ICMP poskytuje ve srovnání s TCP méně přesné výsledky.

##### <a name="firewall-configuration"></a>Konfigurace brány firewall
Protokolu TCP vyžaduje, aby TCP pakety odesílají na cílový port. Výchozí port je používán agenty NPM je 8084, ale toto můžete změnit, když konfigurujete agenty. Ano je potřeba zajistit, že síťové brány firewall nebo pravidla NSG (v Azure) jsou umožňuje přenosy na portu. Budete také muset Ujistěte se, že místní brány firewall na počítačích, kde jsou nainstalováni agenti konfigurace umožňuje provozu na tomto portu.

Konfigurace pravidel brány firewall na počítačích s Windows, ale budete muset ručně nakonfigurovat bránu firewall vaší sítě můžete použít skripty prostředí PowerShell.

Naproti tomu ICMP nefunguje pomocí portu. Ve většině scénářů organizace ICMP je povolen přenos přes bránu firewall umožňují použijte nástroj Diagnostika sítě jako nástroj Ping. Takže pokud jeden počítač z jiné, může odeslat příkaz Ping, můžete použít protokol ICMP bez nutnosti ruční konfigurace brány firewall.

> [!NOTE]
> V případě, že si nejste jisti, jaký protokol pro použití, zvolte začínat ICMP. Pokud nejste s výsledky spokojeni, můžete vždy přepnout TCP později.


#### <a name="how-to-switch-the-protocol"></a>Jak přepínat protokol

Pokud jste se rozhodli použít ICMP během nasazení, můžete přepnout TCP kdykoli úpravou výchozí pravidlo monitorování.

##### <a name="to-edit-the-default-monitoring-rule"></a>Chcete-li upravit výchozí pravidlo monitorování
1.  Přejděte na **síťový výkon** > **monitorování** > **konfigurace** > **monitorování** a pak klikněte na **výchozí pravidlo**.
2.  Přejděte k položce **protokol** a vyberte protokol, který chcete použít.
3.  Klikněte na tlačítko **Uložit** použití nastavení.

I když výchozí pravidlo používá určitý protokol, můžete vytvořit nová pravidla s jiný protokol. Můžete například vytvořit směs pravidla, kde některé z pravidel pomocí protokolu ICMP a jiný používá protokol TCP.




## <a name="data-collection-details"></a>Podrobnosti kolekce dat
Sledování výkonu sítě používá TCP SYN. SYNACK ACK handshake paketů při výběru TCP a odpověď odezvy ICMP ODEZVU protokolu ICMP při výběru jako protokol ICMP ke shromažďování informací ztrátě a latenci. Příkaz Traceroute slouží také k získání informací o topologii.

Následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se údaje pro sledování výkonu sítě.

| Platforma | Přímé agenta | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP metodou handshake/ODEZVU protokolu ICMP každých 5 sekund, data odeslána každé 3 minuty |

Toto řešení využívá k posouzení stavu sítě syntetické transakce. OMS agenty nainstalované v různých bodu v síťových paketů TCP exchange nebo odezvu protokolu ICMP (v závislosti na protokol vybraná pro monitorování) sebou. V procesu agenty další operace round-trip čas a paket ztrátě případných. Každý agent pravidelně provádí taky trasování cesty a jiné agenty pro najít všechny různé trasy v síti, která musí být testována. Na základě těchto dat můžete agenty odvodit latence sítě a obrázků ke ztrátě paketů. Testy jsou opakovat každých pět sekund a data se shromažďují pro dobu 3 minut pomocí agentů před nahráním do služby analýzy protokolů.

> [!NOTE]
> I když agenti komunikaci mezi sebou často, generují velký objem síťového provozu není při provádění testů. Agenti spoléhat jenom na pakety handshake TCP SYN. SYNACK ACK k určení ztrátě a čekací doba – žádná data, které se vyměňují paketů. Během tohoto procesu agenty vzájemně komunikovat pouze v případě potřeby a komunikační topologie agenta je optimalizována pro snížení síťový provoz.
>
>

## <a name="using-the-solution"></a>Použití řešení
Tato část vysvětluje všechny řídicí panel funkce a jejich použití.

### <a name="solution-overview-tile"></a>Dlaždice přehled řešení
Poté, co jste povolili nástroj Sledování výkonu sítě řešení, dlaždice řešení na stránce Přehled OMS poskytuje rychlý přehled o stavu sítě. Zobrazuje prstencový graf zobrazující počet propojení podsítí v pořádku a není v pořádku. Když kliknete na dlaždici, otevře se řídicí panel řešení.

![Dlaždice monitorování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-tile.png)

### <a name="network-performance-monitor-solution-dashboard"></a>Řídicí panel řešení pro sledování výkonu sítě
**Shrnutí sítě** okno se zobrazí souhrn sítě spolu s jejich relativní velikost. Následují dlaždice zobrazuje celkový počet propojení sítě, podsítě odkazy a cesty v systému (cesta obsahuje IP adresy dva hostitele s agenty a všech segmentů směrování mezi nimi).

**Události stavu sítě horní** okno obsahuje seznam nejaktuálnějších událostí, stavu a výstrahy v systému a čas, vzhledem k tomu, že událost byla aktivní. Událost stavu nebo výstraha je vygenerována vždy, když je ztráta paketů nebo latenci sítě nebo podsítí odkazu překročí určitou prahovou hodnotu.

**Horní není v pořádku síťová propojení** okno zobrazuje seznam není v pořádku síťová propojení. Jedná se o sítě odkazy, které mají jednu nebo více událostí nežádoucí stavu pro ně v tuto chvíli.

**Horní propojení podsítí s nejvíce ztrátou** a **propojení podsítí s nejvíce latencí** oken zobrazit propojení nejvyšší podsítí ve ztráta paketů a top propojení podsítí s latencí v uvedeném pořadí. Na určitých síťová propojení může očekává vysokou latencí nebo určitou část ztráta paketů. Tyto odkazy zobrazí v horní deset seznamy, ale nejsou označen jako chybný.

**Běžné dotazy** okno obsahuje sadu vyhledávací dotazy, které načíst monitorování dat přímo nezpracovaná sítě. Tyto dotazy můžete použít jako východisko pro vytvoření vlastní dotazy pro vytváření přizpůsobených sestav.

![Řídicí panel monitorování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="drill-down-for-depth"></a>Procházení pro hloubku
Můžete kliknout na různých odkazy na řídicí panel řešení k podrobnostem hlubší do všech oblastí zájmu. Například pokud se zobrazí výstraha nebo není v pořádku síťové propojení se zobrazují na řídicím panelu, můžete můžete kliknutím na prošetřily. Budete přesměrováni na stránku, která uvádí všechna propojení podsítí pro konkrétní síťové propojení. Bude moct zobrazit stav, latence a ztráta stav každé propojení podsítí a rychle zjistěte jaké propojení podsítí jsou příčinou problému. Poté můžete kliknout na **zobrazení uzlu odkazy** zobrazíte všechny odkazy na uzlu odkazu podsíť není v pořádku. Potom můžete zobrazit jednotlivé uzly – odkazy a odkazy uzlů ve špatném stavu.

Můžete kliknout na **zobrazení topologie** zobrazíte topologii směrování směrování trasy mezi zdrojovým a cílovým uzly. Není v pořádku směrování nebo směrování se zobrazí červeně, aby mohli rychle identifikovat problém pro konkrétní části sítě.

![procházení dat](./media/log-analytics-network-performance-monitor/npm-drill.png)

### <a name="network-state-recorder"></a>Zapisovač stavu sítě

Každé zobrazení zobrazí snímek stavu vaší sítě na určitém místě v čase. Ve výchozím nastavení se zobrazí poslední stav. Na panelu v horní části stránky zobrazí bod v čase, pro který se zobrazuje stav. Můžete přejít zpět v čase a zobrazit snímek stavu vaší sítě tak, že kliknete na panelu na **akce**. Také můžete k povolení nebo zakázání automatického obnovení pro všechny stránky, zatímco zobrazit nejnovější stav.

![stav sítě](./media/log-analytics-network-performance-monitor/network-state.png)

#### <a name="trend-charts"></a>Grafy trendů
V každé úrovni, kterou jste procházení, uvidíte trend ztrátě a latence pro síťové propojení. Grafy trendů jsou také k dispozici pro podsítí a uzel odkazy. Časový interval pro rozhraní graph k vykreslení pomocí ovládacího prvku čas v horní části grafu, můžete změnit.

Grafy trendů zobrazit Historický přehled výkonu síťového propojení. Některé problémy s sítě jsou přechodné ve své podstatě a bude obtížné zachytit pouze na základě aktuálního stavu sítě. Je to proto, že problémy můžete rychle surface a zmizí před nikým oznámení, jenom na objevit znova později v čase. Tyto přechodné problémy může být složité pro správce aplikace také protože ty problémy často prostor jako nevysvětlitelné zvyšování doba odezvy aplikace, i když plynulejší se zobrazují všechny součásti aplikace.

Tyto druhy problémů můžete snadno zjistit prohlížením graf trendů, kde se problém objeví jako nečekané Špička v latence sítě nebo ke ztrátě paketů.

![Graf trendů](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Topologie směrování směrování mapy
Sledování výkonu sítě zobrazuje topologii směrování směrování trasy mezi dvěma uzly na mapě interaktivní topologie. Mapy topologie můžete zobrazit výběrem uzlu odkazu a potom kliknutím na **zobrazení topologie**. Můžete také zobrazit mapy topologie kliknutím **cesty** dlaždici na řídicím panelu. Když kliknete na tlačítko **cesty** na řídicím panelu, budete muset z panelu vlevo vyberte zdrojový a cílový uzel a potom klikněte na **výkresu** k vykreslení trasy mezi dvěma uzly.

Topologie mapa zobrazuje, kolik trasy jsou mezi dva uzly a co cesty trvat datových paketů. Kritické body sítě jsou označen červeně na mapě topologie. Chyba síťové připojení nebo chyba síťové zařízení, můžete vyhledat prohlížením red barevnou elementy na mapě topologie.

Když kliknete na uzel nebo hover nad ním na mapě topologie, se zobrazí vlastnosti uzlu jako plně kvalifikovaný název domény a IP adresa. Klikněte na tlačítko směrování, pokud chcete zobrazit, že je IP adresa. Můžete filtrovat pomocí filtrů v podokně Akce sbalitelné konkrétní trasy. A také zjednodušit síťové topologie skrytím střední směrování pomocí posuvníku v podokně Akce. Můžete zvětšení nebo mimo mapy topologie pomocí kolečko myši.

Upozorňujeme, že topologie, zobrazí se v mapě je topologie vrstvy 3 a neobsahuje vrstvy 2 zařízení a připojení.

![Topologie směrování směrování mapy](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Lokalizace chyby
Sledování výkonu sítě je schopna nalézt problémových míst v síti bez připojení k síťovým zařízením. Podle data, která shromažďuje ze sítě a použitím pokročilé algoritmy v grafu sítě, sledování výkonu sítě umožňuje pravděpodobnosti odhad části sítě, které se nejpravděpodobněji při zdroj problému.

Tento přístup je užitečné pro určení problémových míst v síti, kdy přístup k směrování není k dispozici, protože není třeba žádná data, shromáždit ze síťových zařízení, jako jsou směrovače nebo přepínače. To je také užitečné, pokud směrování mezi dva uzly nejsou v administrativní řízení. Přesměrování může být například směrovače poskytovatele internetových služeb.

### <a name="log-analytics-search"></a>Hledání analýzy protokolů
Všechna data, která je graficky zveřejněné prostřednictvím řídicího panelu monitorování výkonu sítě a procházení stránek je také k dispozici nativně v analýzy protokolů hledání. Můžete zadávat dotazy na data pomocí dotazovacího jazyka pro vyhledávání a export dat do aplikace Excel nebo PowerBI vytvořit vlastní sestavy. **Běžné dotazy** okno v řídicím panelu obsahuje některé užitečné dotazy, které můžete použít jako výchozí bod pro vytvoření vlastní dotazy a sestavy.

![vyhledávací dotazy](./media/log-analytics-network-performance-monitor/npm-queries.png)

## <a name="investigate-the-root-cause-of-a-health-alert"></a>Zjistěte základní příčinu stavu výstrahy
Teď, když jste si přečetli o sledování výkonu sítě, podíváme se na jednoduchý vyšetřování příčiny stavu události.

1. Na stránce Přehled získáte rychlý snímek stavu sítě pomocí sledování **sledování výkonu sítě** dlaždici. Všimněte si, že mimo odkazy 6 podsítě monitorovány, jsou 2 není v pořádku. To zaručuje, šetření. Klikněte na dlaždici zobrazení řídicího panelu řešení.  
   ![Dlaždice monitorování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-investigation01.png)
2. Následující příklad obrázek můžete si všimnout, že existuje událost stavu síťového propojení, která není v pořádku. Rozhodnete zkoumat problém a klikněte na **DMZ2 DMZ1** síťové propojení a zjistěte, kořenu problému.  
   ![Příklad odkazu není v pořádku sítě](./media/log-analytics-network-performance-monitor/npm-investigation02.png)
3. Na stránce podrobností se zobrazí všechna propojení podsítí v **DMZ2 DMZ1** síťového propojení. Můžete si všimnout, že pro obě podsítí odkazy, latence překročila prahovou hodnotu, což není v pořádku síťové propojení. Zobrazí se také latence trendy podsítí odkazy. Můžete použít volbu čas ovládacího prvku grafu a zaměřit se na požadované časové rozmezí. Najdete v průběhu dne, kdy latence bylo dosaženo jeho ve špičce. Je možné později článek vyhledat v protokolech pro toto časové období můžete prozkoumat problém. Klikněte na tlačítko **zobrazení uzlu odkazy** k podrobnostem Další.  
   ![Příklad odkazy podsíť není v pořádku](./media/log-analytics-network-performance-monitor/npm-investigation03.png)
4. Podobně jako na předchozí stránku, na stránce podrobností pro konkrétní podsítí propojení uvedeny dolů jeho základní uzlu odkazy. Můžete provádět akce podobně jako zde, stejně jako v předchozím kroku. Klikněte na tlačítko **zobrazení topologie** zobrazíte topologie mezi 2 uzly.  
   ![Příklad propojení uzlů ve špatném stavu](./media/log-analytics-network-performance-monitor/npm-investigation04.png)
5. Všechny cesty mezi 2 vybrané uzly jsou zobrazeny v mapě topologie. Můžete vizualizovat topologii směrování směrování trasy mezi dvěma uzly na mapě topologie. Poskytuje přehledné informace o tom, kolik tras mezi dva uzly a co cesty trvá datových paketů. Kritické body sítě jsou označeny červenou barvu. Chyba síťové připojení nebo chyba síťové zařízení, můžete vyhledat prohlížením red barevnou elementy na mapě topologie.  
   ![Příklad zobrazení není v pořádku topologie](./media/log-analytics-network-performance-monitor/npm-investigation05.png)
6. Mohou být zjišťovány ztrátu, latenci a počet skoků v každá cesta **akce** podokně. Posuvník slouží k zobrazení podrobností o těchto cestách není v pořádku.  Pomocí filtrů můžete vybrat cesty s směrování není v pořádku, aby se vykreslí topologie pro pouze vybrané cesty. Kolečko myši můžete zvětšit nebo zmenšit mapy topologie.

   V pod obrázkem jasně uvidíte hlavní příčinu problému oblasti, které mají určitou část sítě podle cesty a segmentů směrování v červenou barvu. Kliknutím na uzel v mapě topologie zjistí vlastnosti uzlu, včetně plně kvalifikovaný název domény a IP adresu. Kliknutím na směrování zobrazuje IP adresu směrování.  
   ![není v pořádku topologii – příklad podrobnosti cesty](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby

- **UserVoice** -účtováním pro sledování výkonu sítě funkce, které chcete, abychom mohli pracovat na vašich nápadů. Navštivte naše [stránku UserVoice](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring).
- **Připojení k naší kohorty** -zajímají nás vždy nutnosti připojení k naší kohorty nové zákazníky. V rámci ho budete využívat nové funkce a Pomozte nám vylepšit sledování výkonu sítě. Pokud vás zajímá připojení, vyplnění to [rychlé průzkum](https://aka.ms/npmcohort).

## <a name="next-steps"></a>Další kroky
* [V protokolech Hledat](log-analytics-log-searches.md) zobrazíte podrobné sítě záznamů dat výkonu.
