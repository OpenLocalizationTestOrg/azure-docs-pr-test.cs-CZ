---
title: "aaaNetwork sledování výkonu řešení v Azure Log Analytics | Microsoft Docs"
description: "Sledování výkonu sítě v Azure Log Analytics pomáhá sledovat hello výkonu vaší sítě-in téměř real bránu toodetect a vyhledejte kritická místa výkonu sítě."
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
ms.openlocfilehash: e074948221fdd003c640861d759c4ce69ced1cd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-performance-monitor-solution-in-log-analytics"></a>Síťová řešení pro sledování výkonu v analýzy protokolů

![Symbol sledování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-symbol.png)

Tento dokument popisuje, jak tooset-up a používání hello řešením pro monitorování výkonu sítě v analýzy protokolů, které vám pomůže sledovat výkon hello vaší sítě-in téměř real bránu toodetect a vyhledejte kritická místa výkonu sítě. S hello řešení pro sledování výkonu sítě můžete monitorovat hello ztrátě a čekací doba mezi dvěma sítěmi, podsítě nebo servery. Sledování výkonu sítě zjistí problémy se síťovým jako blackholing provoz, směrování chyb a problémů, že monitorování metody konvenční sítě nejsou možné toodetect. Sledování výkonu sítě generuje výstrahy a upozorní jak a kdy je nedodržení prahovou hodnotu pro síťové propojení. Tyto prahové hodnoty je možné zjistit automaticky hello systému nebo je můžete nakonfigurovat toouse vlastní pravidla výstrah. Sledování výkonu sítě zajišťuje možno včas zjistit problémy s výkonem sítě a lokalizováno hello zdroje hello problém tooa určitém segmentu sítě nebo zařízení.

Můžete zjistit problémy se síťovým s hello řídicí panel řešení, která zobrazuje souhrnné informace o vaší síti, včetně poslední události stavu sítě, není v pořádku síťová propojení a propojení podsítí, se kterým se setkávají ztráta paketů vysoké a latenci. Vám může procházení do síťové propojení tooview hello aktuální stav propojení podsítí, jakož i-uzly odkazy. Můžete také zobrazit hello historických trendů ztrátě a čekací doba, s hello sítě, podsítí a úroveň mezi uzly. Můžete zjistit problémy přechodný síťový zobrazením historických trendů grafy pro ztráta paketů a latence a vyhledejte síťové kritických bodů na mapě topologie. Graf interaktivní topologie Hello vám umožní toovisualize hello segmentu směrování síťové trasy a určete hello zdroj problému hello. Třeba dalších řešení, můžete použít hledání protokolů pro analýzu různé požadavky na toocreate vlastní sestavy založené na hello data shromažďovaná společností sledování výkonu sítě.

řešení Hello používá jako chyb sítě primární mechanismus toodetect syntetické transakce. Ano můžete použít bez ohledu na dodavatele nebo model specifické síťové zařízení. Funguje napříč místními cloudu (IaaS) a hybridní prostředí. Hello řešení automaticky vyhledá hello síťové topologie a různé tras ve vaší síti.

Typickou síťovou monitorování produkty se zaměřují na monitorování hello síti stavu zařízení (směrovače, přepínače atd.), ale neposkytuje přehledy hello skutečné kvalitu síťové připojení mezi dvěma body, které se nástroj Sledování výkonu sítě.

### <a name="using-hello-solution-standalone"></a>Pomocí samostatné řešení hello
Pokud chcete toomonitor hello kvalitu síťové připojení mezi jejich kritickým úlohám, sítě, datových centrech nebo lokalitách office a pak je můžete použít hello sledování výkonu sítě řešení samostatně toomonitor stav připojení mezi:

* víc datových centrech nebo office lokalit, které jsou připojené pomocí veřejné nebo soukromé síti
* kritickým úlohám, které jsou spuštěné obchodních aplikací
* služby veřejného cloudu, jako je Microsoft Azure nebo Amazon Web Services (AWS) a místními sítěmi, pokud máte IaaS (VM) k dispozici a budete mít brány nakonfigurované tooallow komunikace mezi místními sítěmi a sítěmi cloudu
* Při použití Express Route Azure a místní sítě

### <a name="using-hello-solution-with-other-networking-tools"></a>Hello řešení pomocí jiné síťové nástroje
Pokud chcete toomonitor řádek obchodní aplikace, můžete jako nástroje doprovodné řešení tooother hello řešení pro sledování výkonu sítě. Pomalé síti může způsobit tooslow aplikace a sledování výkonu sítě může vám pomůže prozkoumat problémy s výkonem aplikace, které jsou způsobeny základní problémům se sítí. Protože řešení hello nevyžaduje žádné zařízení toonetwork přístup, Správce aplikací hello nepotřebuje toorely na síťové informace tooprovide team o jak ovlivňuje hello síťových aplikací.

Navíc pokud již investovat do jiné sítě, nástroje pro sledování, pak hello řešení můžete doplnit tyto nástroje protože většina tradiční řešení pro monitorování sítě neposkytuje přehledy metriky výkonu začátku do konce sítě jako ztrátě a latenci.  Hello řešení pro sledování výkonu sítě může pomoci, že mezeru.

## <a name="installing-and-configuring-agents-for-hello-solution"></a>Instalování a konfigurování agentů pro řešení hello
Použít hello basic procesy tooinstall agentů v [tooLog počítače připojit Windows Analytics](log-analytics-windows-agents.md) a [tooLog připojení nástroje Operations Manager Analytics](log-analytics-om-agents.md).

> [!NOTE]
> Budete potřebovat tooinstall minimálně 2 agentů v pořadí toohave toodiscover dostatek dat a monitorování síťových prostředků. Jinak hodnota hello řešení zůstane v konfiguraci stavu, dokud je nainstalovat a nakonfigurovat další agenty.
>
>

### <a name="where-tooinstall-hello-agents"></a>Kde tooinstall hello agentů
Před instalací agentů, vezměte v úvahu hello topologie sítě a které části hello sítě, kterou chcete toomonitor. Doporučujeme nainstalovat více než jeden agenta pro každou podsíť, které chcete toomonitor. Jinými slovy pro každou podsíť, které chcete toomonitor, zvolte jeden nebo víc serverech nebo virtuálních počítačů a nainstalujte agenta hello na ně.

Pokud si nejste jistí o hello topologii vaší sítě, nainstalujte na serverech s kritickým úlohám, kam chcete výkon sítě hello toomonitor hello agenty. Například můžete sledovat tookeep připojení k síti mezi webového serveru a serveru se systémem SQL Server. V tomto příkladu by instalace agenta na obou serverech.

Agenti monitorují připojení k síti (odkazy) mezi hostiteli – není hostitelů hello sami. Ano, toomonitor síťového propojení, musíte nainstalovat agenty na oba koncové body tento odkaz.

### <a name="configure-agents"></a>Konfigurace agentů

Pokud máte v úmyslu protokol ICMP hello toouse pro syntetické transakce, je třeba tooenable hello následující pravidla brány firewall pro spolehlivě využitím ICMP:

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow
```


Pokud máte v úmyslu toouse hello protokolu TCP potřebujete tooopen porty brány firewall pro tyto tooensure počítače, který může komunikovat agenty. Potřebujete toodownload a pak spusťte hello [skript prostředí PowerShell EnableRules.ps1](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) bez parametrů v okně prostředí PowerShell s oprávněními správce.

skript Hello vytvoří klíčům registru požadovaným podle hello sledování výkonu sítě a připojení TCP pravidla tooallow agenti toocreate vytvoří brány Windows firewall mezi sebou. klíče registru Hello vytvořen skriptem hello také určit, zda toolog hello ladění protokoly a hello cesta k souboru protokoly hello. Definuje také hello agenta TCP port používaný pro komunikaci. Hello hodnoty pro tyto klíče se nastaví automaticky skriptem hello, takže byste neměli měnit ručně tyto klíče.

port Hello otevřít ve výchozím nastavení je 8084. Můžete použít vlastní port poskytnutím parametru hello `portNumber` toohello skriptu. Však hello stejný port by měl použít na všech počítačích hello kde spuštění skriptu hello.

> [!NOTE]
> Hello EnableRules.ps1 skript nakonfiguruje pravidla brány firewall systému Windows pouze v počítači hello, kde je hello skript spuštěn. Pokud máte síťovou bránu firewall, měli byste si ověřit, že umožňuje, aby provoz určený pro port TCP hello používá nástroj Sledování výkonu sítě.
>
>

## <a name="configuring-hello-solution"></a>Konfigurace řešení hello
Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.

1. Hello řešení pro sledování výkonu sítě získá data z počítačů se systémem Windows Server 2008 SP 1 nebo novější a Windows 7 SP1 nebo novější, které jsou text hello stejné požadavky jako hello Microsoft Monitoring Agent (MMA). Agenti NPM můžete spustit také na ploše nebo klientských operačních systémech Windows (Windows 10, Windows 8.1, Windows 8 a Windows 7).
    >[!NOTE]
    >Hello agentů pro operační systémy Windows server podporují TCP a ICMP jako hello protokoly pro syntetické transakce. Hello agentů pro klientské operační systémy Windows, ale podporují pouze ICMP jako hello protokolu pro syntetické transakce.

2. Přidat hello sledování výkonu sítě řešení tooyour prostoru z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).  
   ![Symbol sledování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. Na portálu OMS hello, zobrazí se nová dlaždice s názvem **sledování výkonu sítě** s uvítací zprávu *řešení vyžaduje další konfiguraci*. Budete potřebovat tooconfigure hello řešení tooadd sítí založených na podsítě a uzly, které jsou zjištěny agenty. Klikněte na tlačítko **sledování výkonu sítě** toostart konfigurace sítě výchozí hello.  
   ![řešení vyžaduje další konfiguraci.](./media/log-analytics-network-performance-monitor/npm-config.png)

### <a name="configure-hello-solution-with-a-default-network"></a>Konfigurace řešení hello pomocí výchozí sítě
Na stránce konfigurace hello, uvidíte jedné sítě s názvem **výchozí**. Když nebyly definovány žádné sítě, podsítě automaticky zjistit všechny hello jsou umístěny v síti výchozí hello.

Vždy, když vytvoříte síť, můžete přidat podsíť tooit a této podsíti se odebere z hello výchozí sítě. Pokud odstraníte síť, všechny její podsítě se automaticky vrátí toohello výchozí sítí.

Jinými slovy hello výchozí síť je hello kontejner pro všechny hello podsítě, které nejsou obsaženy v žádné uživatelem definované síti. Nelze upravit nebo odstranit hello výchozí sítí. Vždy zůstává v systému hello. Ale můžete vytvořit libovolný počet sítí, jako je třeba.

Ve většině případů hello podsítě ve vaší organizaci budou uspořádané do víc než jedna síť a je třeba vytvořit jednu nebo více sítí toologically seskupit podsítě.

### <a name="create-new-networks"></a>Vytvořit nové sítě
Síť v nástroji Sledování výkonu sítě je kontejner pro podsítě. Vytvoření sítě s libovolný název a přidejte podsítě toohello sítě. Například můžete vytvořit síť s názvem *Building1* a poté přidejte podsítě, nebo můžete vytvořit síť s názvem *DMZ* a poté přidejte všechny podsítě, které patří sítě toothis toodemilitarized zóny.

#### <a name="toocreate-a-new-network"></a>toocreate novou síť
1. Klikněte na tlačítko **sítě přidat** a pak zadejte hello síťový název a popis.
2. Vyberte jednu nebo více podsítí a potom klikněte na **přidat**.
3. Klikněte na tlačítko **Uložit** toosave hello konfigurace.  
   ![Přidejte síť](./media/log-analytics-network-performance-monitor/npm-add-network.png)

### <a name="wait-for-data-aggregation"></a>Počkejte agregace dat
Po uložení konfigurace hello poprvé, řešení hello spustí shromažďování paketu ztrátě a latence informací o síti mezi uzly hello, kde jsou nainstalováni agenti. Tento proces může chvíli trvat, někdy 30 minut. Při tomto stavu, hello sledování výkonu sítě dlaždice na stránku přehled hello zobrazí zpráva s oznámením *agregace dat v procesu*.

![probíhající agregace dat](./media/log-analytics-network-performance-monitor/npm-aggregation.png)

Po odeslání dat hello, uvidíte hello dat znázorňující dlaždice aktualizovat nástroj Sledování výkonu sítě.

![Dlaždice monitorování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-tile.png)

Klikněte na řídicí panel hello dlaždice tooview hello sledování výkonu sítě.

![Řídicí panel monitorování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Upravte nastavení monitorování pro podsítě
Všechny podsítě, kde byl nainstalován alespoň jeden agent jsou uvedeny na hello **podsítě** ve konfigurační stránku hello.

#### <a name="tooenable-or-disable-monitoring-for-particular-subnetworks"></a>tooenable nebo zakázat monitorování pro konkrétní podsítě
1. Zaškrtněte nebo zrušte hello pole Další toohello **podsítí ID** a potom ověřte, že **použijte pro monitorování** je vybrané nebo nezaškrtnuté, podle potřeby. Můžete zaškrtněte nebo zrušte více podsítí. Pokud je zakázáno, podsítě nemonitoruje jako hello agenti budou aktualizované toostop otestováním jiné agenty.
2. Vyberte hello uzly mají toomonitor pro konkrétní podsítí hello podsítí výběrem ze seznamu hello a přesunutí hello požadované uzly mezi seznamy hello obsahující sledována a sledované uzly.
   Můžete přidat vlastní **popis** toohello podsítí, pokud chcete.
3. Klikněte na tlačítko **Uložit** toosave hello konfigurace.  
   ![Upravit podsíť](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-toomonitor"></a>Zvolte toomonitor uzly
Všechny uzly hello, které nemá agenta nainstalovaného na jejich jsou uvedeny v hello **uzly** kartě.

#### <a name="tooenable-or-disable-monitoring-for-nodes"></a>tooenable nebo zakázat monitorování pro uzly
1. Vyberte nebo zrušte hello uzly mají toomonitor nebo zastavení monitorování.
2. Klikněte na tlačítko **použijte pro monitorování**, nebo vymazat, podle potřeby.
3. Klikněte na **Uložit**.  
   ![Povolit monitorování uzlu](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)

### <a name="set-monitoring-rules"></a>Sada pravidel monitorování
Sledování výkonu sítě generuje události stavu informace o hello připojení mezi dvěma uzly nebo podsíť nebo síť odkazy při porušení zabezpečení nebyla prahovou hodnotu. Tyto prahové hodnoty je možné zjistit automaticky hello systému nebo je můžete nastavit vlastní pravidla výstrah.

Hello *výchozí pravidlo* je vytvořen systémem hello a vytvoří událost stavu vždy, když ke ztrátě nebo latenci mezi žádném páru sítí nebo podsítí odkazy narušení hello naučili systému prahovou hodnotu. Můžete zvolit toodisable hello výchozí pravidlo a vytvořit vlastní pravidla monitorování

#### <a name="toocreate-custom-monitoring-rules"></a>vlastní pravidla sledování toocreate
1. Klikněte na tlačítko **přidat pravidlo** v hello **monitorování** kartě a zadejte název pravidla hello a popis.
2. Vyberte hello pár síť nebo podsíť odkazy toomonitor seznamech hello.
3. Nejdřív vyberte hello sítě, na které hello první podsíť/s zájmu obsažen z rozevíracího seznamu hello sítě a pak vyberte hello podsítí/s z hello odpovídající rozevírací seznam podsítí.
   Vyberte **všechny podsítě** Pokud chcete, aby toomonitor všechny podsítě v síťové propojení hello. Podobně vyberte hello jiných podsítí/s, které vás zajímají. A, můžete kliknout na **přidat výjimky** tooexclude monitorování pro konkrétní podsítí odkazy z výběru hello udělali.
4. Volba mezi ICMP a TCP protokoly pro provádění syntetické transakce.
5. Pokud nechcete, aby události stavu toocreate hello položek, které jste vybrali, zrušte zaškrtnutí **povolit sledování stavu propojení hello předmětem toto pravidlo**.
6. Vyberte sledování podmínek.
   Můžete nastavit vlastní prahové hodnoty pro generování událostí stavu zadáním prahové hodnoty. Vždy, když hodnota hello hello podmínky překročí jeho vybraná prahová hodnota pro dvojici hello vybrané sítě nebo podsítí, je generována událost stavu.
7. Klikněte na tlačítko **Uložit** toosave hello konfigurace.  
   ![Vytvoření vlastního pravidla monitorování](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)

Po uložení monitorování pravidlo, můžete integrovat daného pravidla s správu výstrah kliknutím **vytvoření výstrahy**. Pravidlo výstrahy se automaticky vytvoří s hello vyhledávací dotaz a další potřebné parametry automaticky vyplněné. Pomocí pravidlo výstrahy, můžete dostávat e mailové upozornění, kromě toohello existující výstrahy v rámci NPM. Výstrahy můžete spustit taky nápravné akce pomocí runbooků nebo můžete integrovat stávajících řešení pro správu služby pomocí webhooků. Můžete kliknout na **Spravovat výstrahy** nastavení výstrah tooedit hello.

### <a name="choose-hello-right-protocol-icmp-or-tcp"></a>Zvolte hello správné protokol ICMP nebo TCP

Monitorování výkonu v síti (NPM) používá metriky výkonu sítě toocalculate syntetické transakce jako latence paketu ztrátě a odkaz. toounderstand to lépe, vezměte v úvahu NPM agenta připojené tooone konci síťového propojení. Tato NPM agent zasílá testu pakety tooa druhý NPM agenta připojené tooanother konec hello sítě. Hello druhý agenta odpovědi s odpovědí na žádost. Tento proces se opakuje několikrát. Podle měření počtu hello odpovědi a doba trvání tooreceive jednotlivé odpovědi, hello prvního NPM agenta vyhodnocuje odkaz latence a zahazování paketů.

Hello formát, velikost a pořadí tyto pakety je určen podle hello protokol, který zvolíte, při vytváření pravidla monitorování. Na základě protokolu hello paketů, hello zprostředkující síťových zařízení (směrovače, přepínače atd.) může zpracovat tyto pakety jinak. V důsledku toho zvoleného protokol ovlivňuje hello přesnost výsledků hello. A, vaše volba protokol také určuje, zda je nutné provést jakékoli ruční kroky, poté, co nasadíte řešení NPM hello.

NPM nabízí že Hello volba mezi protokoly ICMP a TCP pro provádění syntetické transakce.
Pokud si zvolíte ICMP, při vytváření pravidla syntetické transakce, použijte hello NPM agenti ODEZVU protokolu ICMP zprávy toocalculate hello sítě latence a ztráta paketů. ODEZVU protokolu ICMP hello používá stejné zprávy, který poslal hello konvenční nástroj Ping. Použijete-li jako hello protokol TCP, NPM agenti odesílají paket TCP SYN přes síť hello. Následují TCP handshake dokončení a potom odebírání hello připojení pomocí RVNÍ paketů.

#### <a name="points-tooconsider-before-choosing-hello-protocol"></a>Body tooconsider před volbou hello protokolu
Vezměte v úvahu následující informace, než vyberete protokol toouse hello:

##### <a name="discovering-multiple-network-routes"></a>Zjišťování několik síťových tras
TCP je přesnější při zjišťování víc tras a je menší počet agentů v jednotlivých podsítích. Jeden nebo dva agenty pomocí protokolu TCP můžete například zjistit všechny redundantní cesty mezi podsítěmi. Musíte však několik agenty pomocí protokolu ICMP tooachieve podobné výsledky. Pomocí protokolu ICMP, pokud máte *N* počet tras mezi dvěma podsítěmi, budete potřebovat víc než 5*N* agentů ve zdrojové nebo cílové podsíti.

##### <a name="accuracy-of-results"></a>Přesnost výsledků
Směrovače a přepínače zpravidla tooassign tooICMP ECHO pakety porovnání tooTCP pakety s nižší prioritou. V některých situacích při síťová zařízení jsou velkém zatížení, hello dat získat TCP přesněji odráží hello ztrátě a latence, které aplikace. K tomu dochází, protože většina provoz aplikace hello probíhá přes protokol TCP. V takových případech poskytuje ICMP tooTCP méně přesné výsledky porovnání.

##### <a name="firewall-configuration"></a>Konfigurace brány firewall
Protokolu TCP vyžaduje, aby TCP pakety odesílají tooa cílový port. Výchozí port Hello používá NPM agentů je 8084, ale toto můžete změnit, když konfigurujete agenty. Ano je nutné tooensure, že síťové brány firewall nebo pravidla NSG (v Azure) jsou umožňuje přenosy na portu hello. Musíte taky toomake opravdu nakonfigurované hello místní branou firewall na hello počítačů, kde jsou nainstalováni agenti tooallow provozu na tomto portu.

Můžete použít skripty prostředí PowerShell, tooconfigure pravidla brány firewall na počítačích s Windows, ale potřebujete tooconfigure bránu firewall vaší sítě ručně.

Naproti tomu ICMP nefunguje pomocí portu. Ve většině scénářů organizace je prostřednictvím tooallow brány firewall hello toouse nástroj pro diagnostiku sítě chcete hello nástroj Ping povolené přenosy protokolu ICMP. Ano Pokud může odeslat příkaz Ping jeden počítač z jiné, pak můžete protokol ICMP hello bez nutnosti tooconfigure brány firewall ručně.

> [!NOTE]
> V případě, že si nejste jisti, jaký protokol toouse, zvolte toostart ICMP s. Pokud nejste s hello výsledky spokojeni, můžete vždy přepnout tooTCP později.


#### <a name="how-tooswitch-hello-protocol"></a>Jak tooswitch hello protokolu

Pokud jste zvolili toouse ICMP během nasazení, můžete kdykoli přepnout tooTCP úpravou hello výchozí pravidlo monitorování.

##### <a name="tooedit-hello-default-monitoring-rule"></a>tooedit hello výchozí pravidlo monitorování
1.  Přejděte příliš**výkon sítě** > **monitorování** > **konfigurace** > **monitorování** a pak klikněte na **výchozí pravidlo**.
2.  Posuňte se toohello **protokol** a vyberte hello protokolu, které chcete toouse.
3.  Klikněte na tlačítko **Uložit** tooapply hello nastavení.

I když výchozí pravidlo hello používá určitý protokol, můžete vytvořit nová pravidla s jiný protokol. Můžete například vytvořit směs pravidla, kde některé hello pravidel pomocí protokolu ICMP a jiný používá protokol TCP.




## <a name="data-collection-details"></a>Podrobnosti kolekce dat
Sledování výkonu sítě používá TCP SYN. SYNACK ACK handshake paketů v případě TCP je zvolená a odpověď odezvy ICMP ODEZVU ICMP je zvolen jako hello toocollect ztrátě a latence informace o protokolu. Příkaz Traceroute je také informace o topologii použité tooget.

Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se údaje pro sledování výkonu sítě.

| Platforma | Přímé agenta | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP metodou handshake/ODEZVU protokolu ICMP každých 5 sekund, data odeslána každé 3 minuty |

řešení Hello používá syntetické transakce tooassess hello stavu sítě hello. OMS agenty nainstalované v různých okamžiku pakety hello sítě exchange TCP nebo odezvu protokolu ICMP (v závislosti na protokolu hello vybraná pro monitorování) sebou. V procesu hello agenti informace hello odezvy čas a paket dojít ke ztrátě, pokud existuje. Každý agent pravidelně provádí taky trasování trasy tooother agenti toofind všechny hello různé trasy v hello síť, která musí být testována. Na základě těchto dat můžete agenty hello odvodit hello latence sítě a obrázků ke ztrátě paketů. Hello testů jsou opakovat každých pět sekund a data se shromažďují pro dobu 3 minut hello agenty před nahráním ho toohello analýzy protokolů služby.

> [!NOTE]
> I když agenti komunikaci mezi sebou často, generují velký objem síťového provozu není při provádění testů hello. Agenty spoléhat jenom na TCP SYN. SYNACK ACK handshake pakety toodetermine hello ztrátě a čekací doba – žádná data, které se vyměňují paketů. Během tohoto procesu agenty vzájemně komunikovat pouze v případě potřeby a je optimalizován hello agenta komunikační topologie tooreduce síťový provoz.
>
>

## <a name="using-hello-solution"></a>Pomocí řešení hello
Tato část vysvětluje všechny funkce hello řídicího panelu a jak toouse je.

### <a name="solution-overview-tile"></a>Dlaždice přehled řešení
Poté, co jste povolili hello sledování výkonu sítě řešení, dlaždice hello řešení na stránce Přehled OMS hello poskytuje rychlý přehled o stavu sítě hello. Zobrazuje prstencový graf zobrazující hello počet propojení podsítí v pořádku a není v pořádku. Když kliknete na dlaždici hello, otevře se řídicí panel řešení hello.

![Dlaždice monitorování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-tile.png)

### <a name="network-performance-monitor-solution-dashboard"></a>Řídicí panel řešení pro sledování výkonu sítě
Hello **shrnutí sítě** okno se zobrazí souhrn hello sítě spolu s jejich relativní velikost. Následují dlaždice zobrazuje celkový počet propojení sítě, podsítě odkazy a cesty v systému hello (cesta obsahuje IP adresy hello dva hostitele s agenty a všech segmentů směrování hello mezi nimi).

Hello **události stavu sítě horní** okno obsahuje seznam nejaktuálnějších událostí, stavu a výstrahy v systému hello a hello čas od hello událostí byl aktivní. Událost stavu nebo výstraha je vygenerována vždy, když je ztráta paketů hello nebo latenci sítě nebo podsítí odkazu překročí určitou prahovou hodnotu.

Hello **horní není v pořádku síťová propojení** okno zobrazuje seznam není v pořádku síťová propojení. Toto jsou hello síťové spojení, která mají jeden nebo více událostí nežádoucí stavu pro ně momentálně hello.

Hello **horní propojení podsítí s nejvíce ztrátou** a **propojení podsítí s nejvíce latencí** oken zobrazit propojení podsítí nejvyšší hello podle ztráta paketů a top propojení podsítí s latencí v uvedeném pořadí. Na určitých síťová propojení může očekává vysokou latencí nebo určitou část ztráta paketů. Tyto odkazy zobrazí v horní deset seznamy hello, ale nejsou označen jako chybný.

Hello **běžné dotazy** okno obsahuje sadu vyhledávací dotazy, které načíst monitorování dat přímo nezpracovaná sítě. Tyto dotazy můžete použít jako východisko pro vytvoření vlastní dotazy pro vytváření přizpůsobených sestav.

![Řídicí panel monitorování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="drill-down-for-depth"></a>Procházení pro hloubku
Můžete kliknout na různých odkazy na hello řešení řídicí panel toodrill rozbalovací hlubší do všech oblastí zájmu. Například pokud se zobrazí výstraha nebo není v pořádku síťové propojení se zobrazují na řídicím panelu hello, můžete kliknutím tooinvestigate Další. Budete přesměrováni tooa stránky, která uvádí všechna propojení podsítí hello pro konkrétní síťové propojení hello. Bude hello možné toosee stav, latence a ztráta stav každé propojení podsítí a rychle zjistěte jaké propojení podsítí způsobují hello problém. Poté můžete kliknout na **zobrazení uzlu odkazy** toosee všechna propojení uzlů hello pro podsíť není v pořádku hello propojení. Potom můžete zobrazit jednotlivé uzly – odkazy a odkazy hello uzlů ve špatném stavu.

Můžete kliknout na **zobrazení topologie** tooview hello segmentu směrování topologie hello tras mezi hello zdrojové a cílové uzly. Hello není v pořádku směrování nebo směrování jsou zobrazené červeně, aby mohli rychle identifikovat konkrétní části tooa hello problém sítě hello.

![procházení dat](./media/log-analytics-network-performance-monitor/npm-drill.png)

### <a name="network-state-recorder"></a>Zapisovač stavu sítě

Každé zobrazení zobrazí snímek stavu vaší sítě na určitém místě v čase. Ve výchozím nastavení se zobrazí nejnovější stav hello. Hello řádek v hello horní části stránky hello zobrazuje hello bodu v čase, pro který se zobrazuje stav hello. Můžete toogo zpět v snímek hello čas a zobrazení stavu vaší sítě kliknutím na panelu hello na **akce**. Můžete také zvolit tooenable nebo průběhu zobrazit nejnovější stav hello vypnout automatické aktualizace pro všechny stránky.

![stav sítě](./media/log-analytics-network-performance-monitor/network-state.png)

#### <a name="trend-charts"></a>Grafy trendů
V každé úrovni, kterou jste procházení, uvidíte hello trend ztrátě a latence pro síťové propojení. Grafy trendů jsou také k dispozici pro podsítí a uzel odkazy. Hello časový interval pro tooplot grafu hello můžete změnit pomocí ovládacího prvku hello čas v horní části hello hello grafu.

Grafy trendů zobrazit Historický přehled hello výkon síťového propojení. Některé problémy s sítě jsou přechodné ve své podstatě a by být pevný toocatch pouze na základě aktuálního stavu sítě hello hello. Je to proto, že problémy můžete rychle surface a zmizí před nikým oznámení, jenom tooreappear později v čase. Tyto přechodné problémy může být složité pro správce aplikace také protože ty problémy často prostor jako nevysvětlitelné zvyšování doba odezvy aplikace, i když všechny součásti aplikace zobrazí toorun bez problémů.

Tyto druhy problémů můžete snadno zjistit prohlížením graf trendů, kde hello problém se zobrazí jako nečekané Špička v latenci nebo paketu ztráty sítě.

![Graf trendů](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Topologie směrování směrování mapy
Sledování výkonu ukazuje hello topologie směrování směrování tras mezi dvěma uzly na mapě interaktivní topologie sítě. Hello topologickou mapu můžete zobrazit výběrem uzlu odkazu a potom kliknutím na **zobrazení topologie**. Navíc můžete zobrazit hello topologickou mapu kliknutím **cesty** dlaždici na řídicím panelu hello. Když kliknete na tlačítko **cesty** na řídicím panelu hello, budete mít tooselect hello zdrojové a cílové uzly z hello levém panelu a pak klikněte na **vykreslení** tooplot hello trasy mezi hello dva uzly.

mapy topologie Hello zobrazí je kolik směrování mezi hello dva uzly a jaké cesty hello dat trvat paketů. Kritické body sítě jsou označen červeně na mapě topologie hello. Chyba síťové připojení nebo chyba síťové zařízení, můžete vyhledat prohlížením red barevnou elementy na mapě topologie hello.

Po kliknutí na uzel nebo hover nad ním na mapě topologie hello, se zobrazí vlastnosti hello hello uzlu jako plně kvalifikovaný název domény a IP adresa. Klikněte na tlačítko směrování toosee, je IP adresa. Můžete toofilter konkrétní trasy pomocí filtrů hello v podokně Akce sbalitelné hello. A také zjednodušit hello síťové topologie skrytím hello střední směrování pomocí posuvníku hello v podokně Akce hello. Můžete zvětšení nebo mimo hello topologickou mapu pomocí kolečko myši.

Poznamenejte si tuto topologii hello vidět v mapě hello je topologie vrstvy 3 a neobsahuje vrstvy 2 zařízení a připojení.

![Topologie směrování směrování mapy](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Lokalizace chyby
Sledování výkonu sítě je možné toofind hello sítě kritická místa bez připojení toohello síťových zařízení. Sledování výkonu sítě podle hello dat, který shromáždí z hello sítě a použitím pokročilé algoritmy na hello sítě grafu, díky pravděpodobnosti odhad hello části sítě, které jsou pravděpodobně hello zdroj problému hello.

Tento přístup je užitečné toodetermine hello sítě kritická místa, když toohops přístup není k dispozici, protože není třeba žádné toobe data shromážděná z hello síťová zařízení, jako jsou směrovače nebo přepínače. To je také užitečné, pokud hello směrování mezi dva uzly nejsou v administrativní řízení. Hello segmentů směrování může být například směrovače poskytovatele internetových služeb.

### <a name="log-analytics-search"></a>Hledání analýzy protokolů
Všechna data, která je k dispozici graficky prostřednictvím hello sledování výkonu sítě řídicí panel a procházení stránek je také k dispozici nativně v analýzy protokolů hledání. Můžete zadávat dotazy na data hello pomocí hello vyhledávání dotazu jazyka a vytvářejte vlastní sestavy tak, že vyexportujete hello data tooExcel nebo PowerBI. Hello **běžné dotazy** okno na řídicím panelu hello obsahuje některé užitečné dotazy, které můžete použít jako výchozí bod pro vytvoření vlastní dotazy a sestavy hello.

![vyhledávací dotazy](./media/log-analytics-network-performance-monitor/npm-queries.png)

## <a name="investigate-hello-root-cause-of-a-health-alert"></a>Prozkoumat hello příčiny stavu výstrahy
Teď, když jste si přečetli o sledování výkonu sítě, podíváme se na jednoduchý vyšetřování hello příčin stavu události.

1. Na stránce Přehled hello získáte rychlý snímek stavu hello sítě pomocí sledování hello **sledování výkonu sítě** dlaždici. Všimněte si, že mimo hello 6 podsítě odkazů, který je monitorován, 2 jsou není v pořádku. To zaručuje, šetření. Klikněte na tlačítko hello dlaždice tooview hello řídicí panel řešení.  
   ![Dlaždice monitorování výkonu sítě](./media/log-analytics-network-performance-monitor/npm-investigation01.png)
2. V následující obrázek příkladu hello můžete si všimnout, že existuje událost stavu síťového propojení, která není v pořádku. Rozhodněte, tooinvestigate hello problém a klikněte na hello **DMZ2 DMZ1** toofind odkaz sítě se kořenový hello hello problému.  
   ![Příklad odkazu není v pořádku sítě](./media/log-analytics-network-performance-monitor/npm-investigation02.png)
3. na stránce procházení Hello se zobrazí všechna propojení podsítí hello v **DMZ2 DMZ1** síťového propojení. Můžete si všimnout, že pro obě hello propojení podsítí, latence hello překročila prahovou hodnotu hello provedení hello síťové propojení není v pořádku. Zobrazí se také hello latence trendy obou propojení podsítí hello. Můžete použít hello čas ovládacího prvku pro výběr v grafu toofocus hello na hello potřebný časový rozsah. Zobrazí čas hello hello den, kdy latence bylo dosaženo jeho ve špičce. Je možné později článek vyhledat hello protokoly pro tento čas období tooinvestigate hello problém. Klikněte na tlačítko **zobrazení uzlu odkazy** toodrill rozbalovací Další.  
   ![Příklad odkazy podsíť není v pořádku](./media/log-analytics-network-performance-monitor/npm-investigation03.png)
4. Podobně jako toohello předchozí stránce, stránku hello procházení pro konkrétní podsítí odkaz hello uvádí dolů jeho základní uzlu odkazy. Můžete provádět akce podobně jako zde, stejně jako v předchozím kroku hello. Klikněte na tlačítko **zobrazení topologie** tooview hello topologie mezi uzly hello 2.  
   ![Příklad propojení uzlů ve špatném stavu](./media/log-analytics-network-performance-monitor/npm-investigation04.png)
5. Všechny cesty hello mezi hello 2 vybrané uzly jsou zobrazeny v mapě topologie hello. Můžete vizualizovat topologie směrování směrování hello tras mezi dvěma uzly na mapě topologie hello. Poskytuje přehledné informace o tom, kolik tras mezi dvěma uzly hello a jaké cesty hello datových paketů trvá. Kritické body sítě jsou označeny červenou barvu. Chyba síťové připojení nebo chyba síťové zařízení, můžete vyhledat prohlížením red barevnou elementy na mapě topologie hello.  
   ![Příklad zobrazení není v pořádku topologie](./media/log-analytics-network-performance-monitor/npm-investigation05.png)
6. Hello ztrátě, latence a hello počet segmentů směrování v každé cestě můžete zkontrolovat v hello **akce** podokně. Použijte hello scrollbar tooview hello podrobnosti o těchto cestách není v pořádku.  Použití hello filtry tooselect hello cesty směrování není v pořádku hello se vykreslí tak, aby hello topologie pro hello pouze vybrané cesty. Můžete vytvořit vaše toozoom kolečka myši nebo zrušení hello topologickou mapu.

   V hello pod obrázkem jasně uvidíte hello příčiny hello problém oblasti toohello určitou část hello sítě prohlížením hello cesty a segmentů směrování v červenou barvu. Kliknutím na uzel v mapě topologie hello odhalí hello vlastnosti hello uzlu, včetně hello plně kvalifikovaný název domény a IP adresy. Kliknutím na směrování ukazuje hello IP adresu hello směrování.  
   ![není v pořádku topologii – příklad podrobnosti cesty](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby

- **UserVoice** -vašich nápadů můžete post, pro který má nám toowork na sledování výkonu sítě. Navštivte naše [stránku UserVoice](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring).
- **Připojení k naší kohorty** -zajímají nás vždy nutnosti připojení k naší kohorty nové zákazníky. V rámci ho budete využívat funkce toonew a Pomozte nám vylepšit sledování výkonu sítě. Pokud vás zajímá připojení, vyplnění to [rychlé průzkum](https://aka.ms/npmcohort).

## <a name="next-steps"></a>Další kroky
* [V protokolech Hledat](log-analytics-log-searches.md) tooview podrobné záznamy dat výkonu sítě.
