---
title: "aaaDMZ příklad – vytváření DMZ tooprotect aplikací s bránou Firewall a skupin Nsg | Microsoft Docs"
description: "Sestavení DMZ s bránou Firewall a skupiny zabezpečení sítě (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: 18f116dc3897567bff14a509ae8c13f449182bfb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-2--build-a-dmz-tooprotect-applications-with-a-firewall-and-nsgs"></a>Příklad 2 – vytváření DMZ tooprotect aplikací s bránou Firewall a skupiny Nsg
[Vrátí toohello stránku osvědčené postupy zabezpečení hranic][HOME]

Tento příklad vytvoří DMZ s bránou firewall, čtyři windows serverů a skupin zabezpečení sítě. Je také provede všechny relevantní příkazy tooprovide hello podrobnější vysvětlení jednotlivých kroků. K dispozici je také provozu scénář části tooprovide na podrobné podrobné jak provoz pokračuje prostřednictvím hello vrstev obrany ve hello hraniční sítě. Nakonec v části odkazy hello je kompletní kód hello a instrukce toobuild tento tootest prostředí a experimentů pomocí různé scénáře. 

![Příchozí DMZ s hodnocení chyb zabezpečení a NSG][1]

## <a name="environment-description"></a>Popis prostředí
V tomto příkladu je předplatné, které obsahuje hello následující:

* Dva cloudové služby: "FrontEnd001" a "BackEnd001"
* Virtuální síť "CorpNetwork" se dvěma podsítěmi: "FrontEnd" a "Back-end"
* Jednu skupinu zabezpečení sítě, která je použitá tooboth podsítě
* Virtuální zařízení sítě, v tomto příkladu Barracuda NextGen Firewall, připojení toohello podsíť Frontend
* Windows Server, který představuje server webových aplikací ("IIS01")
* Dva windows servery, které představují aplikace zpět ukončení servery ("AppVM01", "AppVM02")
* Windows server, který představuje server DNS ("DNS01")

> [!NOTE]
> I když tento příklad používá Barracuda NextGen Firewall, řadu hello různých síťových virtuálních zařízení může být použita pro tento příklad.
> 
> 

V části hello odkazy níže je skript prostředí PowerShell, který bude vytvořit většinu hello prostředí popsané výše. Vytváření hello virtuálních počítačů a virtuálních sítí, i když provádějí hello ukázkový skript, nejsou popsané podrobně v tomto dokumentu.

toobuild hello prostředí:

1. Uložit hello sítě konfigurační soubor xml v oddíle odkazy hello (aktualizovat název, umístění a IP adresy toomatch hello zadané scénář)
2. Aktualizace hello uživatelské proměnné ve hello skriptu toomatch hello prostředí hello skriptu je toobe spouštění (odběry, názvy služeb atd.)
3. Spuštění skriptu hello v prostředí PowerShell

**Poznámka:**: oblast hello označený ve hello skript prostředí PowerShell musí odpovídat hello oblast označený ve hello sítě konfigurační soubor xml.

Po úspěšném spuštění skriptu hello mohou být přijata hello kroků po skriptu:

1. Nastavit hello pravidla brány firewall, najdete v části hello níže, s názvem: pravidla brány Firewall.
2. Volitelně můžete v části odkazy hello jsou dva skripty tooset hello webového serveru a aplikačního serveru s tooallow jednoduché webové aplikace testování s touto konfigurací hraniční sítě.

Hello další části se dozvíte většinu hello skripty příkazy relativní tooNetwork skupiny zabezpečení.

## <a name="network-security-groups-nsg"></a>Skupiny zabezpečení sítě (NSG)
V tomto příkladu je skupina NSG vytvořené a pak načtená šesti pravidla. 

> [!TIP]
> Obecně platí musí nejprve vytvořit konkrétní pravidel "Povolit" a pak poslední hello více obecná pravidla "Deny". Hello prioritou stanoví, která pravidla se vyhodnocují jako první. Jakmile provoz nenajde tooapply tooa konkrétní pravidlo, jsou vyhodnotit žádná další pravidla. Pravidla NSG můžete použít buď v hello příchozí nebo odchozí směr (z hlediska hello hello podsítě).
> 
> 

Deklarativně jsou pro příchozí provoz sestavuje hello následující pravidla:

1. Interní DNS provoz (port 53) je povolený
2. Provoz protokolu RDP (portu 3389) z Internetu tooany hello virtuálních počítačů je povoleno.
3. Je povolen přenos HTTP (port 80) z Internetu toohello hello hodnocení chyb zabezpečení sítě (brány firewall)
4. Jakýkoli přenos (všechny porty) z IIS01 tooAppVM1 je povolen.
5. Jakýkoli přenos (všechny porty) z Internetu toohello hello celý virtuální síť (obě podsítě) byl odepřen.
6. Jakýkoli přenos (všechny porty) z hello front-endu podsíť toohello back-end podsítě byl odepřen.

Pomocí těchto pravidel vázané tooeach podsíti, pokud požadavek HTTP byl příchozí z hello Internet toohello webový server, obě pravidla 3 (Povolit) a 5 (Odepřít) by použít, ale vzhledem k tomu, že pravidlo 3 má vyšší prioritu jenom by použít a pravidlo 5 nebude možné uplatnit. Proto hello HTTP požadavku bude mít možnost toohello brány firewall. Pokud tento stejný provoz pokoušel tooreach hello DNS01 server, pravidlo 5 (Odepřít) bude, že hello první tooapply hello přenosů dat a nebude možné toopass toohello serveru. Pravidla 6 (Odepřít) bloky hello front-endu podsíť z rozhovoru toohello back-end podsítě (s výjimkou povolené přenosy v pravidlech 1 a 4), to v případě, že útočník ohrožení hello webovou aplikaci na hello front-endu, útočník hello být chrání síť back-end hello omezený přístup toohello back-end "chráněna" sítě (jenom tooresources zveřejněné na serveru AppVM01 hello).

Je výchozí odchozí pravidlo, které umožňuje provozu toohello Internetu. V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla. toolock dolů provoz v obou směrech směrování definovaná uživatele je vyžadován, to je prozkoumali v různých příkladu, který lze nalézt v hello [hlavní zabezpečení hranic dokumentu][HOME].

pravidla NSG Hello výše popsané jsou velmi podobné pravidla NSG toohello v [Příklad 1 – Vytvoření jednoduché DMZ pomocí skupin Nsg][Example1]. Přečtěte si hello popis NSG v tomto dokumentu pro podrobný pohled na každé pravidlo NSG a jeho atributy.

## <a name="firewall-rules"></a>Pravidla brány firewall
Klient správy bude potřebovat toobe nainstalovaný na počítači toomanage hello firewall a vytvoření konfigurací hello potřeby. V tom, jak toomanage hello zařízení najdete v článku dodavatele hello dokumentace z brány firewall (nebo jiné hodnocení chyb zabezpečení). Hello zbytek této části se popisují hello konfigurace brány firewall hello, samostatně, prostřednictvím klienta pro správu dodavatelé hello (tzn. ne hello portál Azure nebo PowerShell).

Pokyny pro stažení klienta a připojování toohello Barracuda použité v tomto příkladu naleznete zde: [Barracuda NG správce](https://techlib.barracuda.com/NG61/NGAdmin)

V bráně firewall hello předávání pravidla potřebovat toobe vytvořili. Vzhledem k tomu, že v tomto příkladu pouze směruje provoz v vázané toohello brána firewall pro přístup k Internetu a pak toohello webový server, je potřeba jenom jeden předávání pravidlo NAT. Na hello Barracuda NextGen Firewall používá v hello tento příklad bude pravidlo NAT cílové tento provoz pravidlo toopass ("Dst NAT").

toocreate hello následující pravidla (nebo ověřit existující výchozí pravidla), od hello Barracuda NG správce klienta řídicí panel, přejděte na kartě Konfigurace toohello, v hello provozní konfigurace oddíl, klikněte na Ruleset. Mřížka názvem, zobrazí "Hlavní pravidla" hello existující aktivní a deaktivované pravidla v bráně firewall hello. V horním pravém rohu hello tento mřížky je malý, zelená "+" tlačítko, klikněte na tento toocreate nové pravidlo (Poznámka: Brána firewall může být "uzamčen" změny, pokud se zobrazí tlačítko označené "Zamknout" a jsou nelze toocreate nebo upravit pravidla, klikněte na toto tlačítko příliš "odemknutí" hello Sada pravidel pro a povolení úprav). Pokud chcete tooedit existující pravidlo, vyberte toto pravidlo, klikněte pravým tlačítkem a vyberte Upravit pravidlo.

Vytvořit nové pravidlo a zadejte název, jako je například "WebTraffic". 

Ikona pravidlo NAT cílové Hello vypadá takto: ![ikonu cílové NAT][2]

Hello pravidlo samotné by vypadat přibližně takto:

![Pravidlo brány firewall][3]

Zde žádné příchozí adresa, hello přístupů k pokusu o tooreach protokolu HTTP (port 80 nebo 443 pro protokol HTTPS) brány Firewall budou odeslána hello "Místní IP adresy DHCP1" rozhraní a přesměrovaného toohello webového serveru pomocí hello 10.0.1.5 IP adresu brány Firewall na. Vzhledem k tomu, že rovinu hello přenosy na portu 80 a probíhající toohello webový server na portu 80 bylo potřeba žádná změna portu. Dobrý den, kterou je cílového seznamu může 10.0.1.5:8080 Pokud naslouchali naše Webový Server na portu 8080 proto překladu však hello příchozí port 80 na hello brány firewall tooinbound portu 8080 na webovém serveru hello.

Metoda připojení musí také být označeny, pro hello cílové pravidlo z Internetu, hello "Dynamické překládat pomocí SNAT" je nejvhodnější. 

I když existuje pouze jedno pravidlo je důležité, aby jeho priorita je nastavena správně. Pokud v mřížce hello všech pravidel v bráně firewall hello toto nové pravidlo na dolní hello (pod pravidlo "BLOCKALL" hello) vrátí nikdy se do play. Zkontrolujte, zda pravidlo hello nově vytvořený pro webový provoz výše hello BLOCKALL pravidlo.

Po vytvoření pravidla hello musí být nabídnutých toohello brány firewall a pak se aktivuje, pokud to neuděláte hello pravidlo změny se neprojeví. Hello nabízení a aktivace proces je popsán v další části hello.

## <a name="rule-activation"></a>Aktivace pravidla
S hello ruleset upravit tooadd toto pravidlo, musí být hello ruleset odeslán toohello brány firewall a aktivovat.

![Aktivace pravidla brány firewall][4]

V pravém horním rohu hello hello správy klienta jsou součástí clusteru tlačítka. Klikněte na tlačítko hello "odeslat změny" tlačítko toosend hello upravit pravidla brány firewall toohello a pak klikněte na tlačítko "Aktivovat" hello.

Tento příklad prostředí sestavení je s hello aktivace sada pravidel brány firewall pro hello dokončena. Volitelně hello post sestavení v hello odkazy na části lze spustit skripty tooadd aplikace toothis prostředí tootest hello níže provoz scénáře.

> [!IMPORTANT]
> Je důležité toorealize hello webový server nebude přímo přístupů. Pokud prohlížeč požaduje stránku HTTP z FrontEnd001.CloudApp.Net, hello HTTP koncový bod (port 80) předává tento provoz toohello firewall není hello webový server. Hello brány firewall, pak podle pravidla toohello vytvořili výše, zařízení NAT, které požadují toohello Webový Server.
> 
> 

## <a name="traffic-scenarios"></a>Provoz scénáře
#### <a name="allowed-web-tooweb-server-through-firewall"></a>(Povoleno) TooWeb Webový Server přes bránu Firewall
1. Internet uživatelské požadavky HTTP stránky z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)
2. Cloudové služby předává přenos prostřednictvím otevřených koncových bodů na portu 80 toofirewall místní rozhraní na 10.0.1.4:80
3. Podsíť frontend zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
   2. Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo
   3. Použít NSG pravidla 3 (tooFirewall Internetu), Probíhá zpracování povolených, zastavení pravidla
4. Provoz dotkne interní IP adresu brány firewall hello (10.0.1.4)
5. To je provoz na portu 80, přesměruje ho toohello webový server IIS01 najdete v části pravidla brány firewall předávání
6. IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku hello
7. IIS01 hello systému SQL Server na AppVM01 vyzve k zadání informací
8. Žádná odchozí pravidla na podsíť Frontend provoz je povolený.
9. podsíť back-end Hello zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
   2. Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo
   3. Skupina NSG pravidla 3 (Internet tooFirewall) netýká, přesuňte toonext pravidlo
   4. Použít NSG pravidla 4 (IIS01 tooAppVM01), Probíhá zpracování povolených, zastavení pravidla
10. AppVM01 přijímá hello dotazu SQL a odpoví
11. Vzhledem k tomu, že neexistují žádná odchozí pravidla na hello back-end podsíť hello odpovědi je povoleno
12. Podsíť frontend zahájí zpracování příchozí pravidlo:
    1. Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí
    2. Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený.
13. server služby IIS Hello obdrží odpověď hello SQL a dokončí hello odpovědi HTTP a odešle toohello žadatele
14. Vzhledem k tomu, že je relace NAT od hello brány firewall, cíl odpovědi hello (původně) je pro hello brány Firewall
15. brány firewall Hello obdrží odpověď hello z hello Webový Server a předává zpět toohello Internet uživatele
16. Vzhledem k tomu, že nejsou žádná odchozí pravidla na hello odpovědi hello podsítě front-endu je povolen a hello Internet uživatel obdrží webovou stránku hello požadovaný.

#### <a name="allowed-rdp-toobackend"></a>(Povoleno) TooBackend protokolu RDP
1. Správce serveru na Internetu požadavky tooAppVM01 relace protokolu RDP na BackEnd001.CloudApp.Net:xxxxx kde xxxxx je číslo portu hello náhodně přiřadí tooAppVM01 protokolu RDP (portu přiřazené hello naleznete na hello portálu Azure nebo pomocí prostředí PowerShell)
2. Od hello brány Firewall naslouchá jenom na hello FrontEnd001.CloudApp.Net adresu není zapojená tento tok provozu
3. Back-end podsíť zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
   2. Použít NSG pravidlo 2 (RDP), Probíhá zpracování povolených, zastavení pravidla
4. Žádná odchozí pravidla použít výchozí pravidla a návratový provoz je povolený
5. Je povoleno relaci protokolu RDP.
6. AppVM01 vyzve k zadání názvu heslo uživatele

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Povoleno) Webový Server DNS vyhledávání na serveru DNS
1. Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale potřebuje tooresolve hello adresu.
2. Hello konfiguraci sítě pro virtuální síť seznamy hello DNS01 (10.0.2.4 v podsíti hello back-end) jako primární server DNS hello IIS01 odešle tooDNS01 požadavek DNS hello
3. Žádná odchozí pravidla na podsíť Frontend provoz je povolený.
4. Back-end podsíť zahájí zpracování příchozí pravidlo:
   1. Použít NSG pravidlo 1 (DNS), Probíhá zpracování povolených, zastavení pravidla
5. DNS server obdrží požadavek hello
6. DNS server nemá hello adresu do mezipaměti a požádá kořenový server DNS na hello Internetu
7. Žádná odchozí pravidla na back-end podsítě provoz je povolený.
8. Server DNS pro Internet odpoví, vzhledem k tomu, že tuto relaci bylo zahájeno interně, je povoleno hello odpovědi
9. DNS server ukládá do mezipaměti odpovědi hello a odpoví back tooIIS01 toohello úvodního požadavku
10. Žádná odchozí pravidla na back-end podsítě provoz je povolený.
11. Podsíť frontend zahájí zpracování příchozí pravidlo:
    1. Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí
    2. Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený
12. IIS01 obdrží odpověď hello z DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Povoleno) Přístup k souboru webového serveru na AppVM01
1. IIS01 požádá o soubor na AppVM01
2. Žádná odchozí pravidla na podsíť Frontend provoz je povolený.
3. podsíť back-end Hello zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
   2. Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo
   3. Skupina NSG pravidla 3 (Internet tooFirewall) netýká, přesuňte toonext pravidlo
   4. Použít NSG pravidla 4 (IIS01 tooAppVM01), Probíhá zpracování povolených, zastavení pravidla
4. AppVM01 obdrží požadavek na hello a odpoví souboru (za předpokladu, že je autorizovaný přístup)
5. Vzhledem k tomu, že neexistují žádná odchozí pravidla na hello back-end podsíť hello odpovědi je povoleno
6. Podsíť frontend zahájí zpracování příchozí pravidlo:
   1. Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí
   2. Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený.
7. server služby IIS Hello obdrží soubor hello

#### <a name="denied-web-direct-tooweb-server"></a>(Byl odepřen) Přímé tooWeb webového serveru
Od hello Webový Server, IIS01 a hello brány Firewall jsou v hello sdílejí stejné cloudové služby hello stejnou veřejnou IP adresu. Proto by přesměrovat přenosy HTTP toohello brány firewall. Při požadavku hello by úspěšně zprostředkoval, nemůže přejít přímo toohello Webový Server, je předaná, tak, jak má, prostřednictvím hello brány Firewall nejdřív. V tématu hello prvního scénáře v této části pro tok přenosů hello.

#### <a name="denied-web-toobackend-server"></a>(Byl odepřen) TooBackend webového serveru
1. Uživatel Internetu pokusí tooaccess souboru na AppVM01 prostřednictvím hello BackEnd001.CloudApp.Net služby
2. Vzhledem k tomu, že jsou pro sdílené složky otevřené žádné koncové body, se nebude předat hello cloudové služby a nebude kontaktovat hello server
3. Pokud z nějakého důvodu byly otevřené hello koncových bodů, by tento provoz blokovat pravidla NSG 5 (Internet tooVNet)

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Byl odepřen) Vyhledávání DNS web na serveru DNS
1. Uživatel Internetu pokusí toolookup interní DNS záznam na DNS01 prostřednictvím hello BackEnd001.CloudApp.Net služby
2. Vzhledem k tomu, že jsou pro DNS otevřené žádné koncové body, se nebude předat hello cloudové služby a nebude kontaktovat hello server
3. Pokud z nějakého důvodu byly otevřené hello koncových bodů, pravidla NSG 5 (tooVNet Internetu) by blokovat tento provoz (Poznámka: Tento pravidlo 1 (DNS) nebude použít dvou důvodů, první zdrojové adresy hello je hello internet, toto pravidlo se vztahuje pouze na toohello virtuální místní síť jako hello také zdroje To je pravidlo povolení, takže by nikdy zakážou provoz)

#### <a name="denied-web-toosql-access-through-firewall"></a>(Byl odepřen) Webový přístup k tooSQL přes bránu Firewall
1. Internet uživatel požádá o dat SQL z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)
2. Vzhledem k tomu, že nejsou otevřené pro SQL koncové body, se nebude předat hello cloudové služby a nebude dosáhnout hello brány firewall
3. Pokud z nějakého důvodu byly otevřené koncových bodů, podsíť Frontend hello zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
   2. Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo
   3. Použít NSG pravidla 2 (tooFirewall Internetu), Probíhá zpracování povolených, zastavení pravidla
4. Provoz dotkne interní IP adresu brány firewall hello (10.0.1.4)
5. Brány firewall pro SQL nemá žádná pravidla pro předávání a vyřazuje hello provoz

## <a name="conclusion"></a>Závěr
Toto je poměrně splněny následující způsob ochraně vaší aplikace s bránou firewall a izolace hello back-end podsíť z příchozí přenosy.

Další příklady a přehled hranice zabezpečení sítě najdete [sem][HOME].

## <a name="references"></a>Odkazy
### <a name="main-script-and-network-config"></a>Hlavní skript a konfiguraci sítě
Uložte hello úplné skript v souboru skriptu prostředí PowerShell. Uložte do souboru s názvem "NetworkConf2.xml" hello konfiguraci sítě.
Podle potřeby změňte hello proměnné definované uživatelem. Spusťte skript hello a potom postupujte podle hello brány Firewall pravidla instalace instrukce výše.

#### <a name="full-script"></a>Úplné skriptu
Tento skript bude na základě proměnných uživatelsky definované hello:

1. Připojit tooan předplatného Azure
2. Vytvořit nový účet úložiště
3. Vytvořit novou virtuální síť a dvě podsítě, jak jsou definovány v souboru konfigurace sítě hello
4. Sestavení 4 windows server virtuálních počítačů
5. Konfigurace, včetně NSG:
   * Vytváření skupina NSG
   * Sestavování s pravidly
   * Vazba hello NSG toohello příslušné podsítě

Tento skript prostředí PowerShell je vhodné spustit místně na Internetu připojený počítač nebo server.

> [!IMPORTANT]
> Když tento skript se spustí, může být upozornění nebo ostatní informační zprávy, které pop v prostředí PowerShell. Pouze chybové zprávy červeně jsou příčinou problém.
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on hello FrontEnd Subnet (plus hello NVA on hello FrontEnd subnet)
       - Three Servers on hello BackEnd Subnet
       - Network Security Groups tooallow/deny traffic patterns as declared

      Before running script, ensure hello network configuration file is created in
      hello directory referenced by $NetworkConfigFile variable (or update the
      variable tooreflect hello path and file name of hello config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      hello appropriate layer(s) of protection. This script serves as an example of some
      of hello techniques that can be used, but should not be used for all scenarios. You
      are responsible tooassess your security needs and hello appropriate protections
      needed, and then effectively implement those protections.

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password toobe used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes tooreflect your subscription and services
      # Invalid options will fail in hello validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: tooensure proper NSG Rule creation later in this script:
        #       - hello Web Server must be VM 1
        #       - hello AppVM1 Server must be VM 2
        #       - hello DNS server must be VM 4
        #
        #       Otherwise hello NSG rules in hello last section of this
        #       script will need toobe changed toomatch hello modified
        #       VM array numbers ($i) so hello NSG Rule IP addresses
        #       are aligned toohello associated VM IP addresses.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

        # VM 2 - hello First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - hello Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - hello DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer tooNew Storage Account
        Write-Host "Updating Subscription Pointer tooNew Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation varible is correct and hello netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see hello above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all hello EndPoints we'll need once we're up and running
                # Note: Web traffic goes through hello firewall, so we'll need tooset up a HTTP endpoint.
                #       Also, hello firewall will be redirecting web traffic tooa new IP and Port in a
                #       forwarding rule, so hello HTTP endpoint here will have hello same public and local
                #       port and hello firewall will do hello NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }

    # Configure NSG
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) too$($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *

        # Assign hello NSG toohello Subnets
            Write-Host "Binding hello NSG tooboth subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on hello IIS Server)
      # Install Backend resource (Run Post-Build Script on hello AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a>Soubor konfigurace sítě
Uložte tento soubor xml s aktualizované umístění a přidáte odkaz toothis hello souboru toohello $NetworkConfigFile proměnné ve skriptu hello výše.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Ukázkové skripty aplikace
Pokud chcete tooinstall ukázková aplikace pro toto a další příklady DMZ jeden bylo zadáno v hello následující odkaz: [ukázkový skript aplikace][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Příchozí DMZ s NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Ikona cílové NAT"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Pravidlo brány firewall"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Aktivace pravidla brány firewall"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
