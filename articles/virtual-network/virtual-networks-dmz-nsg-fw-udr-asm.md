---
title: "aaaDMZ příklad – sestavení DMZ tooProtect sítím pomocí brány Firewall, UDR a NSG | Microsoft Docs"
description: "Sestavení DMZ s bránou Firewall, uživatelem definované směrování (UDR) a skupiny zabezpečení sítě (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: cc121f9cd5fe3c3e9ac2c70fbb7d982a80bb345d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-3--build-a-dmz-tooprotect-networks-with-a-firewall-udr-and-nsg"></a>Příklad 3 – vytvoření DMZ tooProtect sítím pomocí brány Firewall, UDR a NSG
[Vrátí toohello stránku osvědčené postupy zabezpečení hranic][HOME]

Tento příklad vytvoří DMZ s bránou firewall, čtyři servery windows, uživatele definované směrování, předávání IP adres a skupin zabezpečení sítě. Je také provede všechny relevantní příkazy tooprovide hello podrobnější vysvětlení jednotlivých kroků. K dispozici je také provozu scénář části tooprovide na podrobné podrobné jak provoz pokračuje prostřednictvím hello vrstev obrany ve hello hraniční sítě. Nakonec v části odkazy hello je kompletní kód hello a instrukce toobuild tento tootest prostředí a experimentů pomocí různé scénáře. 

![DMZ obousměrně s hodnocení chyb zabezpečení, NSG a UDR][1]

## <a name="environment-setup"></a>Nastavení prostředí
V tomto příkladu je předplatné, které obsahuje hello následující:

* Tři cloudové služby: "SecSvc001", "FrontEnd001" a "BackEnd001"
* Virtuální síť "CorpNetwork", s tři podsítě: "SecNet", "FrontEnd" a "Back-end"
* Virtuální zařízení sítě, v tomto příkladu bránu firewall, připojení toohello SecNet podsítě
* Windows Server, který představuje server webových aplikací ("IIS01")
* Dva windows servery, které představují aplikace zpět ukončení servery ("AppVM01", "AppVM02")
* Windows server, který představuje server DNS ("DNS01")

V části hello odkazy níže je skript prostředí PowerShell, který bude vytvořit většinu hello prostředí popsané výše. Vytváření hello virtuálních počítačů a virtuálních sítí, i když provádějí hello ukázkový skript, nejsou popsané podrobně v tomto dokumentu.

toobuild hello prostředí:

1. Uložit hello sítě konfigurační soubor xml v oddíle odkazy hello (aktualizovat název, umístění a IP adresy toomatch hello zadané scénář)
2. Aktualizace hello uživatelské proměnné ve hello skriptu toomatch hello prostředí hello skriptu je toobe spouštění (odběry, názvy služeb atd.)
3. Spuštění skriptu hello v prostředí PowerShell

**Poznámka:**: oblast hello označený ve hello skript prostředí PowerShell musí odpovídat hello oblast označený ve hello sítě konfigurační soubor xml.

Po úspěšném spuštění skriptu hello mohou být přijata hello kroků po skriptu:

1. Nastavit hello pravidla brány firewall, najdete v části hello níže, s názvem: popis pravidla brány Firewall.
2. Volitelně můžete v části odkazy hello jsou dva skripty tooset hello webového serveru a aplikačního serveru s tooallow jednoduché webové aplikace testování s touto konfigurací hraniční sítě.

Po úspěšném spuštění skriptu hello hello pravidla brány firewall třeba toobe dokončit, najdete v části hello s názvem: pravidla brány Firewall.

## <a name="user-defined-routing-udr"></a>Uživatelem definované směrování (UDR)
Ve výchozím nastavení jsou hello následující systémové trasy definované jako:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Hello VNETLocal je vždy hello definované adresu prefix(es) Dobrý den virtuální sítě pro tuto konkrétní síť (ie se změní ze tooVNet virtuální sítě v závislosti na tom, jak je definována každý konkrétní virtuální sítě). systémové trasy zbývající Hello statické a výchozí jak je uvedeno výše.

Jako prioritu trasy se zpracovávají prostřednictvím metody hello nejdelší shody předpony (LPM), proto hello nejvíce trasy v tabulce hello by použijí tooa zadané cílové adresy.

Přenos (například toohello DNS01 server, 10.0.2.4) určený pro hello místní sítě (10.0.0.0/16) by se tedy směrován přes hello virtuální síť tooits cílové kvůli toohello 10.0.0.0/16 trasy. Jinými slovy pro 10.0.2.4, trasy 10.0.0.0/16 hello je hello nejvíce trasy, i když hello 10.0.0.0/8 a 0.0.0.0/0 také může použít, ale vzhledem k tomu, že jsou méně specifické neovlivňuje tento provoz. Proto hello provoz too10.0.2.4 by měla mít další segment z hello virtuální místní síť a jednoduše trasy toohello cílový.

Pokud se provoz určený pro 10.1.1.1 například, nebude platit hello 10.0.0.0/16 trasy, ale hello 10.0.0.0/8 by hello nejvíce a by tento provoz hello vyřadit ("černé dírkového"), protože hello další směrování je Null. 

Pokud cílový hello nepoužil tooany hello Null předpony nebo hello VNETLocal předpony, postupujte podle jeho by hello nejméně specifická směrování, 0.0.0.0/0 a směrovat odhlásit toohello Internetu jako další segment hello a proto si Azure a internet okraj.

Pokud ve směrovací tabulce hello existují dva identické předpony, hello následuje hello pořadí podle priority na základě atributu "zdroj" hello trasy:

1. "VirtualAppliance" = tabulku ručně přidaná toohello uživatelem definovaná trasa
2. "Brána VPN" = dynamické směrování protokolu BGP (při použití s hybridní sítě), přidal protokol dynamické sítě, tyto trasy v průběhu času mění jako dynamický protokol hello automaticky odráží změny v peered sítě
3. "Výchozí" = hello systémové trasy, jak je znázorněno v výše uvedené tabulce směrování hello hello místní virtuální síť a hello statické záznamy.

> [!NOTE]
> Teď můžete použít uživatele definované směrování (UDR) s ExpressRoute a VPN Gateway tooforce odchozí a příchozí mezi různými místy provoz toobe tooa směrované sítě virtuální zařízení (hodnocení chyb zabezpečení).
> 
> 

#### <a name="creating-hello-local-routes"></a>Vytváření hello místní trasy
V tomto příkladu jsou potřeba dvou směrovacích tabulek, jeden pro každé podsítě front-endové a back-end hello. Každá tabulka je načtena s statické trasy, které jsou vhodné pro danou podsíť hello. Za účelem hello tohoto příkladu každá tabulka měla tři trasy:

1. Provozu místních podsítí s žádné další směrování definované tooallow místní podsíti provozu toobypass hello brány firewall
2. Virtuální síťový provoz s další směrování definovat jako bránu firewall, přepíše se hello výchozí pravidlo, které přímo umožňuje místní tooroute provoz virtuální sítě
3. Veškerý zbývající provoz (0/0) s další směrování definován jako hello brány firewall

Jakmile jsou vytvořeny hello směrovacích tabulek nejsou vázané tootheir podsítě. Pro hello front-endu podsíť směrovací tabulky po vytvoření a vázaný toohello podsíť by měla vypadat takto:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


V tomto příkladu hello následující příkazy, jsou použité toobuild hello směrovací tabulka, přidejte trasu definovanou uživatelem a pak vytvořte vazbu hello trasy tabulky tooa podsítě (Poznámka; všechny položky pod počínaje znak dolaru (např: $BESubnet) jsou proměnné definované uživatelem z hello skript v Hello odkaz části tohoto dokumentu):

1. První hello základní směrovací tabulky musí být vytvořeny. Tento fragment kódu ukazuje vytvoření hello hello tabulky pro podsíť hello back-end. Ve skriptu hello je vytvořen pro podsíť Frontend hello také odpovídající tabulku.
   
     Nové AzureRouteTable-název $BERouteTableName.
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. Po vytvoření hello směrovací tabulka se dá přidat trasy definované uživatelem konkrétní. V tomto uvádíme veškerý provoz (0.0.0.0/0) budou směrovány přes virtuální zařízení hello (proměnné, $VMIP [0], je použít toopass v přiřazené při vytvoření virtuálního zařízení hello dříve ve skriptu hello hello IP adresy). Ve skriptu hello se také vytvoří odpovídající pravidlo v tabulce front-endu hello.
   
     Get-AzureRouteTable $BERouteTableName | `
   
         Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. přepíše Hello výše položka trasy hello výchozí "0.0.0.0/0" trasu, ale stále existuje, který by umožnil hello výchozí 10.0.0.0/16 pravidlo provozu v rámci virtuální sítě tooroute hello přímo toohello cíl a toohello virtuální síťové zařízení. toocorrect, je nutné přidat toto chování hello postupujte podle pravidlo.
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. V tomto okamžiku je volba toobe, provedeny. S hello výše dvě trasy bude směrovat veškerý provoz toohello brány firewall pro vyhodnocení, i provoz v rámci jedné podsíti. To může být požaduje, ale tooallow provozu v rámci podsítě tooroute místně bez zásahu hello brány firewall, lze přidat třetí, velmi konkrétní pravidlo. Tato trasa stavy, které libovolná adresa destine pro místní podsíti hello můžete právě směrovat existuje přímo (NextHopType = VNETLocal).
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. S hello směrovací tabulky vytvořeny a naplněny s trasy definované uživatelem, nakonec hello tabulky musí být nyní vázané tooa podsítě. Ve skriptu hello hello front-endu směrovací tabulka je také vázané toohello podsítě front-endu. Zde je hello vazby skript pro podsíť hello back-end.
   
     Set-AzureSubnetRouteTable - VirtualNetworkName $VNetName.
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>Předávání IP
Funkce doprovodné tooUDR je předávání IP. Toto je, že nastavení na virtuální zařízení, který umožní tooreceive provozu není konkrétně řešit toohello zařízení a pak předávat této cílové ultimate tooits provoz.

Jako příklad Pokud provoz z AppVM01 provede požadavek toohello DNS01 server, by UDR směrovat tato toohello brána firewall. S povoleno předávání IP bude akceptovat hello zařízení (10.0.0.4) hello provoz pro cíl DNS01 hello (10.0.2.4) a tooits ultimate cílové (10.0.2.4), předá. Bez předávání IP adres na hello brány Firewall povolená by se provoz přijímat hello zařízení Přestože hello směrovací tabulka má hello brány firewall jako další segment hello. 

> [!IMPORTANT]
> Je důležité tooremember tooenable předávání IP adres ve spojení s směrování definovaného uživatele.
> 
> 

Nastavení předávání IP adres je jeden příkaz a lze provést v okamžiku vytvoření virtuálního počítače. Pro hello toku tento fragment kódu hello příklad je hello konci hello skriptu a seskupuje hello UDR příkazy:

1. Volání hello instance virtuálního počítače, který je virtuálního zařízení, v takovém případě hello brány firewall a povolení předávání IP adres (Poznámka; libovolnou položku v red počínaje znak dolaru (např: $VMName[0]) je uživatelem definované proměnné ze skriptu hello v hello odkaz části tohoto dokumentu. Hello nula v hranatých závorkách [0], představuje hello první virtuální počítač v poli hello virtuálních počítačů, pro hello příklad skriptu toowork bez úprav, brány firewall hello hello, musí být první virtuální počítač (VM 0)):
   
     Get-AzureVM-název $VMName [0] - ServiceName $ServiceName [0] | `
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Skupiny zabezpečení sítě (NSG)
V tomto příkladu je skupina NSG vytvořené a pak načten s jedním pravidlem. Tato skupina je pak vázaný jenom toohello front-endové a back-end podsítě (ne hello SecNet). Deklarativně se sestavuje hello následující pravidlo:

1. Jakýkoli přenos (všechny porty) z Internetu toohello hello celý virtuální síť (všechny podsítě) byl odepřen.

I když v tomto příkladu se používají skupiny Nsg, je hlavním účelem jako vrstva sekundární ochranu proti ruční chybné konfigurace. Chceme tooblock všechny příchozí provoz z hello internet tooeither hello front-end nebo back-end podsítě, provoz by měl pouze procházet skrz hello SecNet podsíť toohello brány firewall (a pak v případě vhodné na toohello front-end nebo back-end podsítě). Plus s pravidly UDR hello v místě, přenosy, který do hello front-end nebo back-end podsítí by přesměrováni na toohello brány firewall (Děkujeme tooUDR). brány firewall Hello by to zobrazit jako asymetrický toku a by vyřadit hello odchozí přenosy. Proto existují tři vrstvy zabezpečení chránící hello front-endu a back-end podsítě; 1) bez otevřete koncových bodů na hello FrontEnd001 a BackEnd001 cloudové služby, 2) skupiny Nsg odepření přenosy z Internetu, brána firewall 3) hello vyřazení asymetrické provoz hello.

Jeden bod zajímavé týkající se hello skupinu zabezpečení sítě v tomto příkladu je, že obsahuje pouze jedno pravidlo, viz následující obrázek, který je toodeny internetové přenosy toohello celý virtuální síti, která bude zahrnovat podsítě zabezpečení hello. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet `
        from hello Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Ale protože hello NSG je pouze vazbu toohello front-endu a back-end podsítě, hello pravidlo není zpracován na provoz příchozí toohello zabezpečení podsítě. V důsledku toho Přestože pravidla NSG hello uvádí žádné internetové přenosy tooany adresy na hello virtuální síť, protože hello NSG se nikdy vázán podsítě zabezpečení toohello, bude přenos toohello zabezpečení podsítě.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Pravidla brány firewall
V bráně firewall hello předávání pravidla potřebovat toobe vytvořili. Vzhledem k tomu, že brána firewall hello je blokování nebo předávání všechny vstupní, výstupní a intra-VNet provoz, je potřeba řada pravidla brány firewall. Navíc se veškerý příchozí provoz setkají hello služby zabezpečení veřejnou IP adresu (v jiné porty), toobe zpracuje bránou hello firewall. Osvědčeným postupem je toodiagram hello logické toky před nastavením přepracování tooavoid pravidla brány firewall a podsítí hello později. Hello následující obrázek je logickém zobrazení hello pravidla brány firewall v tomto příkladu:

![Logickém zobrazení hello pravidla brány Firewall][2]

> [!NOTE]
> Podle hello virtuální síťové zařízení používá, se liší hello porty pro správu. V tomto příkladu, které se odkazuje Barracuda NextGen Firewall, který používá porty 22, 801 a 807. Přečtěte si hello zařízení dodavatele dokumentaci toofind hello přesný porty používané pro správu zařízení hello používá.
> 
> 

### <a name="logical-rule-description"></a>Popis logické pravidla
V hello logického diagramu výše se nezobrazí podsítě zabezpečení hello vzhledem k tomu, že brána firewall hello je hello pouze prostředků na této podsíti a tohoto diagramu se zobrazuje hello pravidla brány firewall a jak se logicky povolit nebo odepřít tok přenosů dat a není hello skutečné směrované cesty. Navíc hello externí porty vybraný pro hello provoz protokolu RDP jsou porty vyšší pohyboval (8014 – 8026) a byly vybrané toosomewhat zarovnané s hello poslední dva oktety hello místní IP adresu pro snazší čitelnost (například místní server adresu 10.0.1.4 přidružen port externí 8014), ale může použít jakékoli vyšší-konfliktní porty.

V tomto příkladu budeme potřebovat 7 typy pravidel, tyto typy pravidel jsou popsány takto:

* Externí pravidla (pro příchozí provoz):
  1. Pravidlo brány firewall správy: Toto pravidlo přesměrování aplikace umožňuje provoz toopass toohello správu porty hello síťové virtuální zařízení.
  2. Pravidla protokolu RDP (pro každý server systému windows): tyto čtyři pravidla (jeden pro každý server) vám umožní správu hello jednotlivé servery prostřednictvím protokolu RDP. To může také seskupeny do jedno pravidlo v závislosti na možnosti hello hello sítě používá virtuální zařízení.
  3. Pravidla pro provoz aplikace: Existují dvě pravidla pro provoz aplikace, hello nejprve pro hello front-end webový provoz a hello druhý pro přenosy back-end hello (např webový server toodata vrstva). Konfigurace Hello pravidla bude záviset na hello síťovou architekturu (kde jsou umístěné vaše servery) a tok přenosů dat (které směr hello přenosy dat a používaných portů).
     * první pravidlo Hello vám umožní hello skutečné aplikace provoz tooreach hello aplikačního serveru. Při hello ostatní pravidla povolit pro zabezpečení, správy, atd., jsou aplikace pravidla povolit externích uživatelů nebo služeb tooaccess aplikace hello. V tomto příkladu je jednom webovém serveru na portu 80, proto jediné aplikace pravidlo firewallu přesměruje příchozí provoz toohello externí IP, toohello webové servery interní IP adresu. Hello přesměrování provozu relace by být NAT měl toohello interního serveru.
     * Hello je druhé pravidlo pro provoz aplikace hello back-end pravidlo tooallow hello Webový Server tootalk toohello AppVM01 serveru (ale ne AppVM02) prostřednictvím libovolný port.
* Vnitřní pravidla (pro provoz intra-VNet)
  1. TooInternet odchozí pravidlo: Toto pravidlo povolí přenosy z jakékoli sítě toopass toohello vybrané sítě. Toto pravidlo je obvykle výchozí pravidlo už v bráně firewall hello, ale v zakázaném stavu. Toto pravidlo by měly být povoleny v tomto příkladu.
  2. Pravidlo DNS: Toto pravidlo umožňuje pouze (port 53) provoz toopass toohello DNS server DNS. Toto pravidlo pro toto prostředí, které se většina přenos dat z hello front-endu toohello back-end je blokován, konkrétně umožňuje DNS z jakékoli místní podsítě.
  3. Podsíť tooSubnet pravidlo: Toto pravidlo je tooallow jakýkoli server na hello back end podsíť tooconnect tooany server na hello podsítě front end (ale není hello zpětné).
* Pohotovostního pravidlo (pro provoz, který nesplňuje některé z výše uvedených hello):
  1. Všechny přenosy pravidlo odepřít: To by mělo být vždy hello konečné pravidlo (z hlediska priorita) a jako takový Pokud přenosy dat se nezdaří toomatch žádné hello předcházející pravidla, která se zahodí tímto pravidlem. Toto je výchozí pravidlo a obvykle aktivaci žádné je obecně nutné provést změny.

> [!TIP]
> Na hello druhý aplikace pravidlo pro provoz jakéhokoli portu je povolen pro snadné tohoto příkladu, skutečné scénář hello nejvíce konkrétní port a rozsahy adres musí být použité tooreduce hello prostor pro útok toto pravidlo.
> 
> 

<br />

> [!IMPORTANT]
> Jakmile všechny hello výše pravidla vytvořeny, je důležité tooreview hello prioritu každé pravidlo tooensure provozu se povoluje nebo odepírá podle potřeby. V tomto příkladu jsou hello pravidla v pořadí podle priority. Je snadno toobe zamknout z brány firewall hello kvůli toomis seřazených pravidel. Minimálně zkontrolujte, zda hello správy pro bránu firewall hello samotné vždy hello pravidlo absolutní nejvyšší prioritou.
> 
> 

### <a name="rule-prerequisites"></a>Pravidla požadavků
Jeden požadovaných hello virtuální počítač spuštěný hello brány firewall jsou veřejné koncové body. Pro přenosy tooprocess hello brány firewall musí být otevřený hello odpovídající veřejné koncové body. Existují tři typy přenosů dat v tomto příkladu; 1) správu provoz toocontrol hello bránu firewall a pravidla brány firewall, servery windows hello toocontrol provoz protokolu RDP 2) a provoz 3) aplikace. Toto jsou tři sloupce hello typů přenosů v horním hello polovinu logickém zobrazení pravidla brány firewall hello výše.

> [!IMPORTANT]
> Je zde klíče takeway tooremember, **všechny** provoz se odešlou přes bránu firewall hello. Ano tooremote plochy toohello IIS01 serveru, i když je v hello Front End cloudové služby a v hello podsítě Front End, tooaccess tento server, které je nutné zadat tooRDP toohello brány firewall na portu 8014 a pak umožnit hello brány firewall tooroute hello RDP požadavek interně toohello IIS01 portu RDP. Hello portál Azure "připojit" tlačítko nebude fungovat, protože neexistuje žádná přímá cesta tooIIS01 protokolu RDP (jde o hello portálu uvidí). To znamená, všechna připojení z hello Internetu bude toohello služby zabezpečení a Port, například secscv001.cloudapp.net:xxxx (referenční dokumentace hello výše diagram hello mapování portů externí, interní IP a Port).
> 
> 

Koncový bod lze otevřít buď v době hello vytvoření virtuálního počítače nebo odeslat sestavení je provést v hello ukázkový skript a znázorněném na tento fragment kódu (Poznámka; všechny položky počínaje znak dolaru (např: $VMName[$i]) je uživatelem definované proměnné ze skriptu hello v hello referen část CE v tomto dokumentu. Hello "$i" v hranatých závorkách [$i] představuje hello pole počet konkrétní virtuální počítač v matici virtuálních počítačů):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

I když se zobrazují zde není jasně kvůli toohello použití proměnných, ale koncové body jsou **pouze** otevřít na hello zabezpečení cloudové služby. Toto je tooensure, že se zpracovává veškerý příchozí provoz (směrovat, NAT měl, vynechané) bránou hello firewall.

Klient správy bude potřebovat toobe nainstalovaný na počítači toomanage hello firewall a vytvoření konfigurací hello potřeby. V tom, jak toomanage hello zařízení najdete v článku dodavatele hello dokumentace z brány firewall (nebo jiné hodnocení chyb zabezpečení). Hello zbytek této části a hello další části, vytváření pravidel brány Firewall, popíše hello konfigurace brány firewall hello, samostatně, prostřednictvím klienta pro správu dodavatelé hello (tzn. ne hello portál Azure nebo PowerShell).

Pokyny pro stažení klienta a připojování toohello Barracuda použité v tomto příkladu naleznete zde: [Barracuda NG správce](https://techlib.barracuda.com/NG61/NGAdmin)

Po přihlášení na hello brány firewall, ale před vytvořením pravidel brány firewall, existují dvě třídy požadovaných objektů, které může usnadnit vytváření pravidel hello; Objekty, síť a služby.

V tomto příkladu tři objekty pojmenované síti musí být definované (jeden pro podsíť Frontend hello a hello back-end podsíť, také síťového objektu pro hello IP adresu serveru DNS hello). toocreate pojmenované síti; od hello Barracuda NG správce klienta řídicí panel, přejděte na kartě Konfigurace toohello, v hello provozní konfigurační oddíl klikněte Ruleset, pak klikněte na tlačítko "Sítě" v části nabídky hello objekty brány Firewall a pak klikněte na nový v nabídce Upravit sítě hello. objekt Hello sítě může být nyní vytvořen přidáním hello název a předponu hello:

![Vytvoření objektu front-endové síti][3]

Tím se vytvoří pojmenované sítě pro podsíť FrontEnd hello, podobně jako objekt měl být vytvořen pro hello back-end i podsíť. Nyní hello podsítě lze snadněji odkazovat podle názvu v pravidlech brány firewall hello.

Pro hello objekt serveru DNS:

![Vytvořit objekt serveru DNS][4]

V pravidle DNS později v dokumentu hello použije tento jeden odkaz na IP adresu.

Hello druhý požadované objekty jsou objekty služby. Toto bude reprezentovat hello porty pro připojení RDP pro každý server. Vzhledem k tomu, že hello existující objekt služby RDP je vázané toohello, výchozí port protokolu RDP, 3389, nové služby lze vytvořit tooallow provoz z externí porty hello (8014-8026). nové porty Hello také nelze přidat existující službu RDP toohello, ale pro usnadnění ukázku, můžete vytvořit jednotlivých pravidel pro každý server. toocreate nové pravidlo protokolu RDP pro server; od hello Barracuda NG správce klienta řídicí panel, přejděte na kartě Konfigurace toohello, v hello provozní konfiguraci části klikněte na tlačítko Ruleset, pak klikněte na tlačítko "Služby" v části hello objekty brány Firewall nabídky, přejděte dolů hello seznam služeb a vyberte hello Služba "RDP". Klikněte pravým tlačítkem a vyberte kopie, pak klikněte pravým tlačítkem a vyberte vložení. Je nyní objekt služby RDP Copy1, který lze upravovat. Klikněte pravým tlačítkem na RDP Copy1 a vyberte upravit, hello upravit objekt služby objeví se okno si, jak je vidět tady:

![Kopii výchozí pravidlo protokolu RDP][5]

Hello hodnoty mohou být potom upravenou toorepresent hello služba protokolu RDP pro určitý server. Pro AppVM01 hello nad výchozí pravidlo protokolu RDP by měl být upravené tooreflect nový název služby, popis, a používá externí Port protokolu RDP v hello diagram obrázek 8 (Poznámka: porty hello se změnil z hello RDP výchozí 3389 portu externí toohello, které se používá pro tento konkrétní server, v případě hello AppVM01 hello externí portu je 8025) hello upravené služby jsou uvedeny níže:

![Pravidlo AppVM01][6]

Tento proces musí být opakovaných toocreate služby protokolu RDP pro hello zbývající servery; AppVM02, DNS01 a IIS01. Vytvoření Hello tyto služby budou vytvoření pravidla hello jednodušší a zřejmější v další části hello.

> [!NOTE]
> Služby protokolu RDP pro hello brány Firewall není potřeba dvou důvodů; je 1) první hello brána firewall virtuálních počítačů založenými na systému Linux obrázku tak, aby použila SSH port 22 pro správu virtuálních počítačů místo protokolu RDP a 2) port 22, a dva další správu porty jsou povolené v první pravidlo správy hello popsané dál tooallow pro možnosti připojení správy.
> 
> 

### <a name="firewall-rules-creation"></a>Vytvoření pravidla brány firewall
Existují tři typy pravidel brány firewall použité v tomto příkladu, všechny mají odlišné ikony:

pravidlo přesměrování aplikace Hello: ![přesměrování ikona aplikace][7]

pravidlo NAT cílové Hello: ![ikonu cílové NAT][8]

pravidlo průchodu Hello: ![předat ikonu][9]

Další informace o těchto pravidel lze najít na hello Barracuda webové stránky.

toocreate hello následující pravidla (nebo ověřit existující výchozí pravidla), od hello Barracuda NG správce klienta řídicí panel, přejděte na kartě Konfigurace toohello, v hello provozní konfigurace oddíl, klikněte na Ruleset. Mřížka názvem, zobrazí "Hlavní pravidla" hello existující aktivní a deaktivované pravidla v této bráně firewall. V horním pravém rohu hello tento mřížky je malý, zelená "+" tlačítko, klikněte na tento toocreate nové pravidlo (Poznámka: Brána firewall může být "uzamčen" změny, pokud se zobrazí tlačítko označené "Zámek" a jsou nelze toocreate nebo upravit pravidla, klikněte na toto tlačítko příliš "odemknout" hello se pravidlo t a povolení úprav). Pokud chcete tooedit existující pravidlo, vyberte toto pravidlo, klikněte pravým tlačítkem a vyberte Upravit pravidlo.

Jakmile pravidel se vytvořit nebo upravit, musí být nabídnutých toohello brány firewall a pak se aktivuje, pokud to neuděláte hello pravidlo změny se neprojeví. Hello nabízení a aktivace proces je popsán níže hello podrobnosti pravidlo popisy.

Specifikace Hello každé pravidlo vyžaduje toocomplete v tomto příkladu jsou popsány následovně:

* **Brány firewall pravidla správy**: Tato aplikace přesměrování pravidlo umožňuje provoz toopass toohello správu porty hello sítě virtuální zařízení, v tomto příkladu Barracuda NextGen Firewall. Hello správu porty jsou 801, 807 a volitelně 22. Hello externí i interní porty jsou hello stejné (tj. žádné překlad port). Toto pravidlo, instalační program-MGMT-přístup, je výchozí pravidlo a povolena ve výchozím nastavení (Barracuda NextGen Firewall verze 6.1).
  
    ![Pravidlo brány firewall správy][10]

> [!TIP]
> Hello zdroj adresní prostor v tomto pravidle je všechny, pokud hello správu rozsahů IP adres jsou známé, tento zúžíte by také snížit hello útoku prostor toohello správu porty.
> 
> 

* **Pravidla RDP**: pravidla NAT tyto cílové vám umožní správu hello jednotlivé servery prostřednictvím protokolu RDP.
  Toto pravidlo existují čtyři toocreate kritické potřebná pole:
  
  1. Zdroj – tooallow RDP z libovolného místa, odkaz hello "Žádný" se používá v pole hello zdroje.
  2. Služba – použít odpovídající objekt služby vytvořený v tomto případě "AppVM01 RDP" hello, externí porty hello přesměrování toohello servery místní IP adresu a tooport 3386 (hello výchozí port protokolu RDP). Tato konkrétní pravidlo je tooAppVM01 přístup RDP.
  3. Cíl – musí být hello *místního portu v bráně firewall hello*, "IP místní server DHCP 1" nebo eth0, pokud se používá statické IP adresy. Hello řadová číslovka (eth0, eth1 atd.) může být odlišné, pokud vaše síťové zařízení má více místní rozhraní. Je to hello port odesílá hello brány firewall z (může být hello stejné jako hello přijetí port), skutečný směrované cíl hello je v poli cílového seznamu hello.
  4. Přesměrování – v této části informuje virtuální zařízení hello kde tooultimately přesměrování tento provoz. Nejjednodušší přesměrování Hello je tooplace hello IP a portu (volitelné) v poli cílového seznamu hello. Pokud není port je použité hello cílový port na hello příchozí požadavek bude používat (ie žádné překlad), pokud je port určený hello port bude také NAT by spolu s hello IP adres.
     
     ![Pravidlo brány firewall protokolu RDP][11]
     
     Celkem čtyři pravidla RDP potřebovat toobe vytvořit: 
     
     | Název pravidla | Server | Služba | Cílového seznamu |
     | --- | --- | --- | --- |
     | RDP IIS01 |IIS01 |IIS01 PROTOKOLU RDP |10.0.1.4:3389 |
     | RDP DNS01 |DNS01 |DNS01 PROTOKOLU RDP |10.0.2.4:3389 |
     | RDP AppVM01 |AppVM01 |AppVM01 protokolu RDP |10.0.2.5:3389 |
     | RDP AppVM02 |AppVM02 |AppVm02 protokolu RDP |10.0.2.6:3389 |

> [!TIP]
> Zužující hello oboru hello zdroje a pole služby se zmenší prostor pro útoky hello. Hello nejvíc omezenou oboru, který vám umožní funkce je třeba použít.
> 
> 

* **Pravidla pro provoz aplikace**: existují dvě pravidla pro provoz aplikace, hello nejprve pro hello front-end webový provoz a hello druhý pro přenosy back-end hello (např webový server toodata vrstva). Tato pravidla bude záviset na hello síťovou architekturu (kde jsou umístěné vaše servery) a tok přenosů dat (které směr hello přenosy dat a používaných portů).
  
    Nejprve popsané je pravidlo hello front-endu pro webový provoz:
  
    ![Pravidla brány firewall na webu][12]
  
    Toto pravidlo NAT cílové umožňuje hello skutečné aplikace provoz tooreach hello aplikačnímu serveru. Při hello ostatní pravidla povolit pro zabezpečení, správy, atd., jsou aplikace pravidla povolit externích uživatelů nebo služeb tooaccess aplikace hello. V tomto příkladu je jednom webovém serveru na portu 80, proto hello jediné aplikace pravidlo firewallu přesměruje příchozí provoz toohello externí IP, toohello webové servery interní IP adresu.
  
    **Poznámka:**: že neexistuje žádná portu přiřazené do pole cílového seznamu hello, proto hello příchozí port 80 (nebo 443 pro hello služby vybrali) bude použita v hello přesměrování hello webového serveru. Pokud hello webový server naslouchá na jiný port, například port 8080, hello cílového seznamu pole může být tooallow aktualizované too10.0.1.4:8080 hello Port také přesměrování.
  
    Dobrý den, je další pravidlo pro provoz aplikace hello back-end pravidlo tooallow hello Webový Server tootalk toohello AppVM01 serveru (ale ne AppVM02) prostřednictvím jakékoli služby:
  
    ![Pravidlo brány firewall AppVM01][13]
  
    Toto pravidlo průchodu umožňuje žádné serveru IIS na hello front-endu podsíť tooreach hello AppVM01 (IP adresa 10.0.2.5) na libovolném portu pomocí jakékoli protokol tooaccess dat, které hello webové aplikace.
  
    V tento snímek obrazovky "\<explicitní dest\>" se používá v hello cílového pole toosignify 10.0.2.5 jako cíl hello. To může být buď explicitní znázorněné nebo názvem objektu sítě (stejně jako ve hello požadavky pro hello DNS server). Toto je až toohello Správce brány firewall hello jako toowhich metoda se použije. dvakrát klikněte na první prázdný řádek hello pod tooadd 10.0.2.5 jako Explict Desitnation \<explicitní dest\> a zadejte adresu hello v okně hello, která se objeví.
  
    S tímto pravidlem předat je potřeba žádné NAT vzhledem k tomu, že toto je interní provoz, takže hello metodu připojení lze nastavit příliš "Ne překládat pomocí SNAT".
  
    **Poznámka:**: hello zdrojové síti v tomto pravidle je jakýkoli prostředek na podsíť FrontEnd hello, pokud budou existovat jenom jedna nebo známé konkrétní počet webových serverů, objekt síťový prostředek může být vytvořena toobe konkrétnější toothose přesnou IP adresy místo Hello celé podsítě front-endu.

> [!TIP]
> Toto pravidlo používá službu hello "Žádné" toomake hello jednodušší toosetup ukázkové aplikace a používat, to také umožní ICMPv4 (ping) v jediné pravidlo. Je to ale není doporučený postup. Hello porty a protokoly ("služby") by měl být zúžit toohello minimální možné, že umožňuje aplikaci operaci tooreduce hello útok napříč tuto hranici.
> 
> 

<br />

> [!TIP]
> I když toto pravidlo zobrazuje odkaz na explicitní dest používá, je třeba použít jednotný přístup v rámci konfigurace brány firewall hello. Doporučuje se použít tento hello s názvem objektu sítě v rámci pro snazší čitelnost a podpoře. explicitní Hello-dest je použité tooshow sem pouze odkaz na alternativní metoda a se obecně nedoporučuje (hlavně u komplexní konfigurace).
> 
> 

* **TooInternet odchozí pravidlo**: předat toto pravidlo povolí provoz z jakékoli zdroj toopass toohello vybrané cílové sítě. Toto pravidlo je výchozí pravidlo obvykle již v hello Barracuda NextGen firewall, ale je v zakázaném stavu. Pravým tlačítkem myši na toto pravidlo můžete přístup k příkazu aktivovat pravidlo hello. pravidlo Hello tady uvedené byl upravený tooadd hello dvě místní podsítě, které byly vytvořeny jako odkazy v hello požadovaných části tohoto dokumentu toohello zdrojového atributu tohoto pravidla.
  
    ![Odchozí pravidlo brány firewall][14]
* **Pravidlo DNS**: předat toto pravidlo umožňuje pouze (port 53) provoz toopass toohello DNS server DNS. Pro toto prostředí, které se většina přenos dat z hello front-endu toohello back-end je blokován konkrétně toto pravidlo umožňuje DNS.
  
    ![Pravidlo brány firewall DNS][15]
  
    **Poznámka:**: na této obrazovce je součástí snímek hello metodu připojení. Vzhledem k tomu, že toto pravidlo je pro interní IP toointernal IP adresu provoz, není třeba žádné NATing, tento hello způsob připojení nastaven příliš "Ne překládat pomocí SNAT" pro toto pravidlo průchodu.
* **Podsíť tooSubnet pravidlo**: předat toto pravidlo je výchozí pravidlo, které byl aktivován a upravené tooallow jakýkoli server na hello zpět končí na podsíť tooconnect tooany server hello podsítě front end. Toto pravidlo je všechny interní provoz, takže hello metodu připojení lze nastavit tooNo překládat pomocí SNAT.
  
    ![Pravidlo brány firewall Intra-VNet][16]
  
    **Poznámka:**: zaškrtávací políčko hello obousměrný nastavení není kontrolován (ani je zaregistrováno většina pravidla), to je důležité pro toto pravidlo, umožňuje tato pravidla "jeden směrové", připojení lze inicializovat z hello back-end podsíť toohello front-endu sítě, ale není hello zpětného. Pokud toto zaškrtávací políčko je zaškrtnuto, by toto pravidlo povolit obousměrný přenos, který je z našich logického diagramu není žádoucí.
* **Všechny přenosy pravidlo Odepřít**: to by mělo být vždy hello konečné pravidlo (z hlediska priorita), a jako takový Pokud přenosem toků selže toomatch některé z předchozích pravidel hello ho budou vynechána tímto pravidlem. Toto je výchozí pravidlo a obvykle aktivaci žádné je obecně nutné provést změny. 
  
    ![Pravidlo brány firewall Odepřít][17]

> [!IMPORTANT]
> Jakmile všechny hello výše pravidla vytvořeny, je důležité tooreview hello prioritu každé pravidlo tooensure provozu se povoluje nebo odepírá podle potřeby. V tomto příkladu jsou hello pravidla v hello pořadí, ve kterém by se zobrazit v hello hlavní mřížku předávání pravidla v hello Barracuda klienta pro správu.
> 
> 

## <a name="rule-activation"></a>Aktivace pravidla
Hello ruleset hello ruleset upravit toohello specifikaci diagram hello logiku, musí být odeslán toohello brány firewall a pak se aktivuje.

![Aktivace pravidla brány firewall][18]

V pravém horním rohu hello hello správy klienta jsou součástí clusteru tlačítka. Klikněte na tlačítko hello "odeslat změny" tlačítko toosend hello upravit pravidla brány firewall toohello a pak klikněte na tlačítko "Aktivovat" hello.

Tento příklad prostředí sestavení je s hello aktivace sada pravidel brány firewall pro hello dokončena.

## <a name="traffic-scenarios"></a>Provoz scénáře
> [!IMPORTANT]
> Klíče takeway je tooremember, **všechny** provoz se odešlou přes bránu firewall hello. Ano tooremote plochy toohello IIS01 serveru, i když je v hello Front End cloudové služby a v hello podsítě Front End, tooaccess tento server, které je nutné zadat tooRDP toohello brány firewall na portu 8014 a pak umožnit hello brány firewall tooroute hello RDP požadavek interně toohello IIS01 portu RDP. Hello portál Azure "připojit" tlačítko nebude fungovat, protože neexistuje žádná přímá cesta tooIIS01 protokolu RDP (jde o hello portálu uvidí). To znamená, všechna připojení z hello Internetu bude toohello služby zabezpečení a Port, například secscv001.cloudapp.net:xxxx.
> 
> 

Pro tyto scénáře hello následující pravidla brány firewall musí být k dispozici:

1. Správa brány firewall
2. TooIIS01 protokolu RDP
3. TooDNS01 protokolu RDP
4. TooAppVM01 protokolu RDP
5. TooAppVM02 protokolu RDP
6. Provoz aplikace toohello Web
7. TooAppVM01 provoz aplikace
8. Odchozí toohello Internetu
9. TooDNS01 front-endu
10. Přenosy mezi podsítěmi (back-end toofront end pouze)
11. Odepřít vše

Hello skutečné brány firewall ruleset bude mít s největší pravděpodobností mnoha dalším pravidlům v přidání toothese, pravidla hello na jakékoli dané brány firewall bude také obsahovat čísla jinou prioritu než těch, které jsou zde uvedeny hello. Tento seznam a přidružená čísla jsou tooprovide relevance mezi právě tyto 11 pravidla a relativní Priorita hello jedna z nich. Jinými slovy; hello "RDP tooIIS01" na hello skutečné brány firewall, může být číslo pravidla 5, ale také je nad pravidlo "RDP tooDNS01" hello, které by zarovnané s hello záměr v tomto seznamu a pravidel "Správa brány Firewall" hello. seznam Hello bude také pomáhají při hello níže scénáře povolení jako stručný výtah; například "Pravidlo FW 9 (DNS)". Jako stručný výtah hello čtyři pravidla RDP bude souhrnně nazývané také, "hello RDP pravidla" po nesouvisejícími tooRDP hello provoz scénář.

Odvolat také, že skupiny zabezpečení sítě jsou na místě pro příchozí přenosy z Internetu na hello front-endu a back-end podsítě.

#### <a name="allowed-internet-tooweb-server"></a>(Povoleno) Internet tooWeb serveru
1. Internet uživatelské požadavky HTTP stránky z SecSvc001.CloudApp.Net (Internet čelí cloudové služby)
2. Cloudové služby předává přenos prostřednictvím otevřených koncových bodů na portu 80 toofirewall rozhraní na 10.0.0.4:80
3. Žádné skupiny NSG přiřazené podsítě tooSecurity, tak pravidla NSG systému povolit provoz toofirewall
4. Provoz dotkne interní IP adresu brány firewall hello (10.0.1.4)
5. Brány firewall zahájí zpracování pravidla:
   1. Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo
   2. Nemusíte použít, přesunout toonext pravidlo FW pravidla 2 až 5 (RDP pravidla)
   3. FW pravidla 6 (aplikace: webové) použít, provoz je povolený, zařízení brány firewall NAT. ho too10.0.1.4 (IIS01)
6. podsíť Frontend Hello zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (bloku Internet) (Tento provoz byl NAT by bránou hello firewall, proto hello zdrojové adresy je nyní hello brány firewall, která je v podsíti hello zabezpečení a prohlížet podsíť Frontend hello "místní" provoz toobe NSG a je proto povoleno), přesunout toonext pravidlo
   2. Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG
7. IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku hello
8. IIS01 pokusí tooinitiates tooAppVM01 relace FTP v podsíti back-end
9. Hello UDR trasy na podsíť Frontend provede další segment hello hello brány firewall
10. Žádná odchozí pravidla na podsíť Frontend provoz je povolený.
11. Brány firewall zahájí zpracování pravidla:
    1. Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo
    2. Nemusíte použít FW pravidlo 2 až 5 (RDP pravidla), přesunete toonext pravidlo
    3. FW pravidla 6 (aplikace: webové) není použít, přesuňte toonext pravidlo
    4. FW pravidla 7 (aplikace: back-end) použít, provoz je povolený, brány firewall předává too10.0.2.5 provoz (AppVM01)
12. podsíť back-end Hello zahájí zpracování příchozí pravidlo:
    1. Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo
    2. Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG
13. AppVM01 obdrží požadavek na hello a inicializuje hello relace a odpovídá
14. Hello UDR trasy v podsíti back-end umožňuje další segment hello hello brány firewall
15. Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na hello back-end podsíť hello odpovědi je povoleno
16. Protože to vrací provoz na bránu firewall navázanou relaci hello předá hello odpověď zpět toohello webový server (IIS01)
17. Podsíť frontend zahájí zpracování příchozí pravidlo:
    1. Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo
    2. Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG
18. server služby IIS Hello obdrží odpověď hello, dokončení transakce hello s AppVM01 a potom dokončí vytváření hello odpověď HTTP, tato odpověď HTTP odeslán toohello žadatele
19. Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na hello odpovědi hello podsítě front-endu je povoleno
20. Hello HTTP odpovědi přístupů hello brány firewall a vzhledem k tomu, že toto je tooan odpovědi hello navázat relaci NAT přijat bránou hello firewall
21. brány firewall Hello pak přesměruje hello odpověď zpět toohello Internet uživatele
22. Vzhledem k tomu, že neexistují žádné odchozí pravidla NSG nebo UDR směrování na podsíť Frontend hello je povoleno hello odpovědi a hello Internet uživatel obdrží webovou stránku hello požadovaný.

#### <a name="allowed-internet-rdp-toobackend"></a>(Povoleno) TooBackend Internet RDP
1. Správce serveru na Internetu požadavky tooAppVM01 relace RDP prostřednictvím SecSvc001.CloudApp.Net:8025, kde 8025 je číslo portu hello přiřazeného uživatele pro pravidlo brány firewall "RDP tooAppVM01" hello
2. Hello Cloudová služba předá provoz prostřednictvím hello otevřených koncových bodů na portu 8025 toofirewall rozhraní na 10.0.0.4:8025
3. Žádné skupiny NSG přiřazené podsítě tooSecurity, tak pravidla NSG systému povolit provoz toofirewall
4. Brány firewall zahájí zpracování pravidla:
   1. Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo
   2. Netýká FW pravidla 2 (RDP IIS), přesunete toonext pravidlo
   3. Netýká FW pravidla 3 (RDP DNS01), přesunete toonext pravidlo
   4. Použít FW pravidla 4 (RDP AppVM01), provoz je povolený, zařízení brány firewall NAT. ho too10.0.2.5:3386 (portu RDP na AppVM01)
5. podsíť back-end Hello zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (bloku Internet) (Tento provoz byl NAT by bránou hello firewall, proto hello zdrojové adresy je nyní hello brány firewall, která je v podsíti hello zabezpečení a prohlížet hello back-end podsítě provoz "local" toobe NSG a je proto povoleno), přesunout toonext pravidlo
   2. Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG
6. AppVM01 naslouchá pro provoz protokolu RDP a odpovídá
7. Žádná odchozí pravidla NSG použít výchozí pravidla a návratový provoz je povolený
8. UDR trasy odchozí provoz brány firewall toohello jako další segment hello
9. Protože to vrací provoz na bránu firewall navázanou relaci hello předá hello odpověď zpět toohello internet uživatele
10. Je povoleno relaci protokolu RDP.
11. AppVM01 vyzve k zadání názvu heslo uživatele

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Povoleno) Webový Server DNS vyhledávání na serveru DNS
1. Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale potřebuje tooresolve hello adresu.
2. Hello konfiguraci sítě pro virtuální síť seznamy hello DNS01 (10.0.2.4 v podsíti hello back-end) jako primární server DNS hello IIS01 odešle tooDNS01 požadavek DNS hello
3. UDR trasy odchozí provoz brány firewall toohello jako další segment hello
4. Podsíť Frontend vázané toohello žádná odchozí pravidla NSG se provoz je povolený
5. Brány firewall zahájí zpracování pravidla:
   1. Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo
   2. Nemusíte použít FW pravidlo 2 až 5 (RDP pravidla), přesunete toonext pravidlo
   3. Nemusíte použít, přesuňte toonext pravidlo FW pravidla 6 a 7 (pravidla aplikace)
   4. Netýká FW pravidlo 8 (tooInternet), přesunete toonext pravidlo
   5. Použít FW pravidlo 9 (DNS), provoz je povolený, brány firewall předává too10.0.2.4 provoz (DNS01)
6. podsíť back-end Hello zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo
   2. Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG
7. DNS server obdrží požadavek hello
8. DNS server nemá hello adresu do mezipaměti a požádá kořenový server DNS na hello Internetu
9. UDR trasy odchozí provoz brány firewall toohello jako další segment hello
10. Žádná odchozí pravidla NSG na podsítě back-end provoz je povolený.
11. Brány firewall zahájí zpracování pravidla:
    1. Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo
    2. Nemusíte použít FW pravidlo 2 až 5 (RDP pravidla), přesunete toonext pravidlo
    3. Nemusíte použít, přesuňte toonext pravidlo FW pravidla 6 a 7 (pravidla aplikace)
    4. Použít pravidlo 8 FW (tooInternet), provoz je povolený, relace je překládat pomocí SNAT out tooroot server DNS na Internetu hello
12. Server DNS pro Internet odpoví, vzhledem k tomu, že tuto relaci bylo inicializováno hello brány firewall, hello odpověď byla přijata bránou hello firewall
13. Protože se jedná navázanou relaci, brány firewall hello předává toohello hello odpověď inicializace serveru DNS01
14. podsíť back-end Hello zahájí zpracování příchozí pravidlo:
    1. Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo
    2. Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG
15. Hello DNS server obdrží a ukládá do mezipaměti odpovědi hello a poté odpoví back tooIIS01 toohello úvodního požadavku
16. Hello UDR trasy v podsíti back-end umožňuje další segment hello hello brány firewall
17. Neexistují žádná odchozí pravidla NSG v podsíti hello back-end, provoz je povolený
18. Toto je navázanou relaci v bráně firewall hello, odpověď hello předá serveru IIS back toohello hello brány firewall
19. Podsíť frontend zahájí zpracování příchozí pravidlo:
    1. Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí
    2. Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený
20. IIS01 obdrží odpověď hello z DNS01

#### <a name="allowed-backend-server-toofrontend-server"></a>(Povoleno) Back-end serveru tooFrontend serveru
1. Správce přihlášený tooAppVM02 prostřednictvím protokolu RDP požádá o soubor přímo ze serveru IIS01 hello pomocí Průzkumníka souborů systému windows
2. Hello UDR trasy v podsíti back-end umožňuje další segment hello hello brány firewall
3. Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na hello back-end podsíť hello odpovědi je povoleno
4. Brány firewall zahájí zpracování pravidla:
   1. Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo
   2. Nemusíte použít FW pravidlo 2 až 5 (RDP pravidla), přesunete toonext pravidlo
   3. Nemusíte použít, přesuňte toonext pravidlo FW pravidla 6 a 7 (pravidla aplikace)
   4. Netýká FW pravidlo 8 (tooInternet), přesunete toonext pravidlo
   5. Netýká FW pravidlo 9 (DNS), přesunete toonext pravidlo
   6. Použít pravidlo 10 FW (Intra-podsítě), provoz je povolený, brány firewall předá too10.0.1.4 provoz (IIS01)
5. Podsíť frontend zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo
   2. Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG
6. Za předpokladu, že správné ověření a autorizaci, IIS01 přijme požadavek hello a odpoví
7. Hello UDR trasy na podsíť Frontend provede další segment hello hello brány firewall
8. Vzhledem k tomu, že neexistují žádná odchozí pravidla NSG na hello odpovědi hello podsítě front-endu je povoleno
9. Protože se jedná o existující relaci v bráně firewall hello této odpovědi je povoleno a brány firewall hello vrátí tooAppVM02 odpovědi hello
10. Back-end podsíť zahájí zpracování příchozí pravidlo:
    1. Netýká NSG pravidlo 1 (bloku Internet), přesunete toonext pravidlo
    2. Výchozí pravidla NSG povolit podsíť toosubnet provoz, provoz je povolený, zastavit zpracování pravidel NSG
11. AppVM02 obdrží odpověď hello

#### <a name="denied-internet-direct-tooweb-server"></a>(Byl odepřen) Internet přímé tooWeb serveru
1. Uživatel Internetu pokusí tooaccess hello webový server, IIS01, prostřednictvím hello FrontEnd001.CloudApp.Net služby
2. Vzhledem k tomu, že jsou pro přenos HTTP otevřené žádné koncové body, se nebude předávat hello cloudové služby a nebude kontaktovat hello server
3. Pokud z nějakého důvodu byly otevřené hello koncových bodů, by tento provoz blokovat hello NSG (bloku Internet) na podsíť Frontend hello
4. Nakonec hello front-endu podsíť UDR trasy byste odesílali všechny odchozí přenosy z brány firewall toohello IIS01 jako další segment hello a brány firewall hello by tato objeví jako asymetrický provoz a vyřadit hello odchozí odpovědi zacílí existují aspoň tři nezávislá vrstev Defense mezi hello Internetu a IIS01 přes jeho Cloudová služba, která brání Neautorizováno nevhodných přístupu.

#### <a name="denied-internet-toobackend-server"></a>(Byl odepřen) Internet tooBackend serveru
1. Uživatel Internetu pokusí tooaccess souboru na AppVM01 prostřednictvím hello BackEnd001.CloudApp.Net služby
2. Vzhledem k tomu, že jsou pro sdílené složky otevřené žádné koncové body, se nebude předat hello cloudové služby a nebude kontaktovat hello server
3. Pokud z nějakého důvodu byly otevřené hello koncových bodů, by tento provoz blokovat hello NSG (bloku Internet)
4. Nakonec hello UDR trasy byste odesílali všechny odchozí přenosy z brány firewall toohello AppVM01 jako další segment hello a brány firewall hello by tato objeví jako asymetrický provoz a vyřadit hello odchozí odpovědi zacílí existují aspoň tři nezávislá vrstev obrany mezi Hello Internetu a AppVM01 přes jeho Cloudová služba, která brání Neautorizováno nevhodných přístupu.

#### <a name="denied-frontend-server-toobackend-server"></a>(Byl odepřen) Front-endu server tooBackend serveru
1. Předpokládejme, IIS01 došlo k ohrožení a běží škodlivý kód při tooscan hello back-end podsíť servery.
2. Hello front-endu podsíť UDR trasy byste odesílali všechny odchozí přenosy z brány firewall toohello IIS01 jako další segment hello. Toto není něco, co může být změněna pomocí hello ohrožené virtuálních počítačů.
3. brány firewall Hello by zpracovat hello provoz, pokud byl požadavek hello tooAppVM01 nebo toohello serveru DNS pro vyhledávání DNS, které provoz může být potenciálně povolený bránou firewall hello (z důvodu tooFW pravidla 7 a 9). Všechny ostatní přenosy by se zablokovaly podle FW pravidlo 11 (odmítnout vše).
4. Pokud rozšířené detekce hrozeb byl povolen v bráně firewall hello (který není zahrnuté v tomto dokumentu, najdete v dokumentaci dodavatele hello pro vaše konkrétní síťové zařízení advanced threat možnosti), dokonce i provoz, který bude mít možnost podle hello základní předávání pravidla popsané v tomto dokumentu může zabránit, pokud provoz hello obsažené známé podpisům a vzorů, které příznak pravidlo rozšířené hrozba.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Byl odepřen) Vyhledávání DNS pro Internet na serveru DNS
1. Uživatel Internetu pokusí toolookup interní DNS záznam na DNS01 BackEnd001.CloudApp.Net pomocí služby 
2. Vzhledem k tomu, že jsou pro přenosy DNS otevřené žádné koncové body, se nebude předávat hello cloudové služby a nebude kontaktovat hello server
3. Pokud z nějakého důvodu byly otevřené hello koncových bodů, by tento provoz blokovat pravidla NSG hello (bloku Internet) na podsíť Frontend hello
4. Nakonec hello back-end podsíť UDR trasy byste odesílali všechny odchozí přenosy z brány firewall toohello DNS01 jako další segment hello a brány firewall hello by tato objeví jako asymetrický provoz a vyřadit hello odchozí odpovědi zacílí existují aspoň tři nezávislá vrstev Defense mezi hello Internetu a DNS01 přes jeho Cloudová služba, která brání Neautorizováno nevhodných přístupu.

#### <a name="denied-internet-toosql-access-through-firewall"></a>(Byl odepřen) Přístup k Internetu tooSQL prostřednictvím brány Firewall
1. Internet uživatel požádá o dat SQL z SecSvc001.CloudApp.Net (Internet čelí cloudové služby)
2. Vzhledem k tomu, že nejsou otevřené pro SQL koncové body, se nebude předat hello cloudové služby a nebude dosáhnout hello brány firewall
3. Pokud z nějakého důvodu byly otevřené koncové body SQL, brány firewall hello se začne zpracování pravidla:
   1. Netýká FW pravidla 1 (FW Mgmt), přesunete toonext pravidlo
   2. Nemusíte použít, přesunout toonext pravidlo FW pravidla 2 až 5 (RDP pravidla)
   3. Nemusíte použít, přesunout pravidlo toonext FW pravidla 6 a 7 (pravidla pro aplikace)
   4. Netýká FW pravidlo 8 (tooInternet), přesunete toonext pravidlo
   5. Netýká FW pravidlo 9 (DNS), přesunete toonext pravidlo
   6. Netýká FW pravidlo 10 (Intra-podsítě), přesunete toonext pravidlo
   7. Použít FW pravidlo 11 (odmítnout vše), provoz je zpracování blokované, zastavení pravidla

## <a name="references"></a>Odkazy
### <a name="main-script-and-network-config"></a>Hlavní skript a konfiguraci sítě
Uložte hello úplné skript v souboru skriptu prostředí PowerShell. Uložte do souboru s názvem "NetworkConf2.xml" hello konfiguraci sítě.
Podle potřeby změňte hello proměnné definované uživatelem. Spusťte skript hello a potom postupujte podle hello brány Firewall pravidla instalace instrukce výše.

#### <a name="full-script"></a>Úplné skriptu
Tento skript bude na základě proměnných uživatelsky definované hello:

1. Připojit tooan předplatného Azure
2. Vytvořit nový účet úložiště
3. Vytvořit novou virtuální síť a tři podsítě, jak jsou definovány v souboru konfigurace sítě hello
4. Sestavení pět virtuálních počítačů brány firewall 1 a 4 windows server virtuálních počítačů
5. Konfigurace, včetně UDR:
   1. Vytváření dva nové směrovací tabulky
   2. Přidání tras toohello tabulek
   3. Vytvoření vazby tabulky tooappropriate podsítě
6. Povolení předávání IP adres na hello hodnocení chyb zabezpečení
7. Konfigurace, včetně NSG:
   1. Vytváření skupina NSG
   2. Přidávání pravidla
   3. Vazba hello NSG toohello příslušné podsítě

Tento skript prostředí PowerShell je vhodné spustit místně na Internetu připojený počítač nebo server.

> [!IMPORTANT]
> Když tento skript se spustí, může být upozornění nebo ostatní informační zprávy, které pop v prostředí PowerShell. Pouze chybové zprávy červeně jsou příčinou problém.
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on hello FrontEnd Subnet
       - Three Servers on hello BackEnd Subnet
       - IP Forwading from hello FireWall out toohello internet
       - User Defined Routing FrontEnd and BackEnd Subnets toohello NVA

      Before running script, ensure hello network configuration file is created in
      hello directory referenced by $NetworkConfigFile variable (or update the
      variable tooreflect hello path and file name of hello config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind hello appropriate
      layer(s) of protection. This script serves as an example of some of hello techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and hello appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

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
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: tooensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be hello NVA.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

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

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "hello SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use" -ForegroundColor Green}

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
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
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
                # Note: All traffic goes through hello firewall, so we'll need tooset up all ports here.
                #       Also, hello firewall will be redirecting traffic tooa new IP and Port in a forwarding
                #       rule, so all of these endpoint have hello same public and local port and hello firewall
                #       will do hello mapping, NATing, and/or redirection as declared in hello firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create hello Route Tables
        Write-Host "Creating hello Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes tooRoute Tables
        Write-Host "Adding Routes toohello Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Assoicate hello Route Tables with hello Subnets
        Write-Host "Binding Route Tables toohello Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on hello Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign hello NSG tootwo Subnets
        # hello NSG is *not* bound toohello Security Subnet. hello result
        # is that internet traffic flows only toohello Security subnet
        # since hello NSG bound toohello Frontend and Backback subnets
        # will Deny internet traffic toothose subnets.
        Write-Host "Binding hello NSG tootwo subnets" -ForegroundColor Cyan
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
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
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
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "DMZ obousměrně s hodnocení chyb zabezpečení, NSG a UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logickém zobrazení hello pravidla brány Firewall"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Vytvoření objektu front-endové síti"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Vytvořit objekt serveru DNS"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopii výchozí pravidlo protokolu RDP"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "Pravidlo AppVM01"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Ikona aplikace přesměrování"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Ikona cílové NAT"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Ikona průchodu"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Pravidlo brány firewall správy"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Pravidlo brány firewall protokolu RDP"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Pravidla brány firewall na webu"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Pravidlo brány firewall AppVM01"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Odchozí pravidlo brány firewall"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Pravidlo brány firewall DNS"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Pravidlo brány firewall Intra-VNet"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Pravidlo brány firewall Odepřít"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Aktivace pravidla brány firewall"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
