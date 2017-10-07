---
title: "aaaIntegrate aplikace pomocí Azure Virtual Network"
description: "Ukazuje, jak tooconnect aplikace v Azure App Service tooa nový nebo existující virtuální síť Azure"
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: cephalin
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: ccompy
ms.openlocfilehash: a93c504481400245b02220b541a008a7c874d10a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integrace aplikace pomocí virtuální sítě Azure
Tento dokument popisuje funkce integrace virtuální sítě Azure App Service hello a ukazuje, jak tooset ho nahoru s aplikacemi ve [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Pokud jste obeznámeni s virtuální sítí Azure (virtuální sítě), je to funkci, která vám umožní tooplace, které mnoho vašich prostředků Azure v Internetu jiných routeable síti, která můžete řídit přístup k. Tyto sítě pak může být připojené tooyour místními sítěmi pomocí různých technologií, sítě VPN. Další informace o virtuálních sítí Azure, toolearn začínat hello informace zde: [Přehled virtuálních sítí Azure][VNETOverview]. 

Hello Azure App Service má dva formuláře. 

1. Hello víceklientské systémy, které podporují hello plný rozsah cenových plánů
2. Hello prostředí App Service (App Service Environment) premium funkci, která nasadí do virtuální sítě. 

Tento dokument projde integrace virtuální sítě a není App Service Environment. Pokud chcete další informace o funkci hello App Service Environment toolearn, začínat hello informace zde: [App Service Environment ÚVOD][ASEintro].

Integrace virtuální sítě poskytuje tooresources přístup k vaší webové aplikace ve virtuální síti, ale neposkytne soukromý přístup tooyour webovou aplikaci z hello virtuální sítě. Přístup privátního lokality odkazuje toomaking vaší aplikace, které jsou dostupné jenom z privátní sítě, jako z v rámci virtuální sítě Azure. Přístup privátního lokality je dostupná pouze s App Service Environment nakonfigurovaný s interní nástroj pro vyrovnávání zatížení (ILB). Podrobnosti o používání App Service Environment ILB, začínat hello článku zde: [vytváření a používání App Service Environment ILB][ILBASE]. 

Běžný scénář, kde byste použili integrace virtuální sítě se povoluje přístup z vaší webové aplikace tooa databáze nebo webové služby spuštěné na virtuálním počítači ve vaší virtuální síti Azure. Díky integraci virtuální síť nepotřebujete tooexpose veřejný koncový bod pro aplikace na vašem virtuálním počítači ale můžete místo toho použít hello privátní Internetu jiných směrovatelné adresy. 

funkce integrace virtuální sítě Hello:

* vyžaduje standardní, Premium nebo izolované ceny plán 
* funguje s Classic nebo virtuální sítě Resource Manageru 
* podporuje TCP a UDP
* funguje s aplikacemi pro webové, mobilní a rozhraní API
* umožňuje aplikaci tooconnect tooonly 1 virtuální sítě v čase
* umožňuje až toofive virtuálních sítí toobe integrována se službou plán služby App Service 
* Umožňuje hello toobe stejné virtuální sítě používá více aplikacemi v plán služby App Service
* podporuje SLA 99,9 % kvůli toohello SLA na hello brány virtuální sítě

Existují některé věci, které virtuální sítě integrace nepodporuje, včetně:

* připojení na jednotku
* Integrace služby AD 
* Pro rozhraní NetBios
* přístup privátního lokality

### <a name="getting-started"></a>Začínáme
Zde jsou některé věci tookeep nezapomeňte před připojením virtuální sítě tooa webové aplikace:

* Integrace virtuální sítě funguje pouze s aplikacemi ve **standardní**, **Premium**, nebo **izolovaná** ceny plánu. Pokud povolíte funkci hello a pak vertikálně váš plán služby App Service tooan nepodporovaný ceny plán aplikace ztratit toohello jejich připojení virtuální sítě, které používají. 
* Pokud cílový virtuální síti již existuje, musí mít povoleny v rámci brány dynamického směrování, než může být připojené tooan aplikace VPN point-to-site. Pokud vaše brána je nakonfigurovaná se statickým směrováním, nelze povolit point-to-site virtuální privátní sítě (VPN).
* Hello virtuální síť musí být v hello stejnému předplatnému jako Plan(ASP) služby vaší aplikace. 
* Hello aplikace, které se integrují s virtuální sítě pomocí hello DNS, který je zadán pro tuto virtuální síť.
* Ve výchozím nastavení aplikace integrační pouze směrovat provoz do virtuální sítě podle hello tras, které jsou definovány ve vaší virtuální síti. 

## <a name="enabling-vnet-integration"></a>Povolení integrace virtuální sítě
Tento dokument se zaměřuje především na pomocí hello portál Azure pro integraci virtuální sítě. tooenable integrace virtuální sítě s vaší aplikací pomocí prostředí PowerShell, postupujte podle pokynů hello zde: [připojit vaše aplikace tooyour virtuální sítě pomocí prostředí PowerShell][IntPowershell].

Máte možnost tooconnect hello vaší aplikace tooa nový nebo existující virtuální síť. Pokud vytvoříte novou síť jako součást svoji integraci, pak kromě vytváření toojust hello virtuální síť, brány dynamického směrování je pro vás předem nakonfigurovaná a je povolená síť VPN tooSite bodu. 

> [!NOTE]
> Konfigurace nového integrace virtuální sítě může trvat několik minut. 
> 
> 

tooenable integrace virtuální sítě, otevřete v aplikaci nastavení a pak vyberte síť. Hello uživatelského rozhraní, které se otevře nabízí tři možnosti sítě. Tento průvodce bude jenom do integrace virtuální sítě, když hybridní připojení a prostředí App Service jsou popsány dále v tomto dokumentu. 

Pokud aplikace není ve správné ceny plán hello, hello uživatelského rozhraní můžete tooscale váš plán tooa vyšší cenové plán, podle vašeho výběru.

![][1]

### <a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Povolení integrace virtuální sítě s existující virtuální síť
Hello uživatelského rozhraní integrace virtuální sítě vám umožní tooselect ze seznamu vaší virtuální sítě. Hello virtuální sítě Classic znamenat, že jsou například slovem hello "Klasickém" v závorkách další toohello název virtuální sítě. Hello seznam je řazen tak, aby hello virtuální sítě Resource Manager jsou uvedeny jako první. V hello se zobrazí obrázek zobrazené níže, lze vybrat pouze jeden virtuální sítě. Tady je několik důvodů, virtuální sítě můžete šedě včetně:

* Hello virtuální sítě je v jiné předplatné, který má přístup k účtu
* Hello virtuální síť nemá povoleno tooSite bodu
* Hello virtuální síť nemá brány dynamického směrování

![][2]

integrace tooenable klikněte jednoduše na hello chcete toointegrate s virtuální sítě. Po výběru hello virtuální síť, aplikace se automaticky restartuje pro hello změny tootake vliv. 

##### <a name="enable-point-toosite-in-a-classic-vnet"></a>Povolit tooSite bodu v klasické virtuální sítě
Pokud virtuální síť nemá bránu ani má bod tooSite, pak máte tooset této aktualizované první. toodo to pro klasické virtuální sítě přejděte toohello [portál Azure] [ AzurePortal] a zprovoznit hello seznam virtuální Networks(classic). Zde klikněte na síť hello má toointegrate s a klikněte na hello big pole v části Essentials názvem připojení k síti VPN. Tady můžete vytvořit toosite bodu VPN a i mějte ho vytvořit bránu. Po projít hello toosite bod s prostředím vytvoření brány je asi 30 minut, než je připraven. 

![][8]

##### <a name="enabling-point-toosite-in-a-resource-manager-vnet"></a>Povolení tooSite bodu ve virtuální síti Resource Manager
tooconfigure virtuální síť Resource Manager, brána a tooSite bodu, můžete použít buď PowerShell podle postupu uvedeného zde [konfigurace Point-to-Site připojení tooa virtuální sítě pomocí prostředí PowerShell] [ V2VNETP2S] nebo použijte hello portálu Azure podle postupu uvedeného zde [konfigurace Point-to-Site připojení tooa virtuální sítě pomocí portálu Azure hello][V2VNETPortal]. Hello uživatelského rozhraní tooperform tato funkce ještě není k dispozici. Mějte na paměti, je nutné, toocreate certifikáty pro konfiguraci bodu tooSite hello. Probíhá konfigurace automaticky po připojení vaší WebApp toohello virtuální sítě. 

### <a name="creating-a-pre-configured-vnet"></a>Vytvoření předem konfigurovaný virtuální sítě
Chcete-li toocreate nové virtuální sítě, který je nakonfigurovaný s brány a Point-to-Site, pak hello uživatelského rozhraní sítě služby App Service má hello schopností toodo této, ale jenom pro virtuální sítě Resource Manager. Pokud chcete toocreate klasické virtuální sítě s bránou a Point-to-Site, musíte toodo to ručně hello sítě uživatelském rozhraní. 

toocreate virtuální sítě Resource Manageru prostřednictvím hello integrace rozhraní virtuální sítě, jednoduše vyberte **vytvořit novou virtuální síť** a poskytují:

* Název virtuální sítě
* Blok adres virtuální sítě
* Název podsítě.
* Blok adres podsítě
* Blok adres brány
* Blok adres Point-to-Site

Pokud chcete tuto virtuální síť tooconnect tooany jinými sítěmi, pak byste neměli výběr prostor IP adres, který se překrývá s tyto sítě. 

> [!NOTE]
> Vytvoření virtuální sítě Resource Manageru s bránou trvá nějakých 30 minut a aktuálně Neintegruje hello virtuální sítě s vaší aplikací. Po vytvoření virtuální sítě s bránou hello potřebovat toocome back tooyour aplikace uživatelského rozhraní integrace virtuální sítě a vyberte nové sítě VNet.
> 
> 

![][3]

Virtuálních sítí Azure se obvykle vytvoří v rámci adres privátní sítě. Ve výchozí hello integrace virtuální sítě funkce přesměruje přenosy určené do vaší virtuální sítě pro tyto rozsahy IP adres. rozsahy Hello privátních IP adres jsou:

* 10.0.0.0/8 – to je hello stejné jako 10.0.0.0 - 10.255.255.255
* 172.16.0.0/12 – to je hello stejné jako 172.16.0.0 - 172.31.255.255 
* 192.168.0.0/16 - Toto je hello stejné jako 192.168.0.0 - 192.168.255.255

Hello adresní prostor sítě VNet musí toobe zadat v notaci CIDR. Pokud jste obeznámeni s notaci CIDR, je metoda pro zadání bloky adres pomocí IP adresy a celé číslo, které představuje maska sítě hello. Jako rychlý přehled zvažte, že 10.1.0.0/24 by 256 adresy a 10.1.0.0/25 by 128 adresy. Adresu IPv4 s /32 by být právě 1 adresa. 

Pokud nastavíte hello informace o serveru DNS, pak, bude nastavena pro vaši virtuální síť. Po vytvoření virtuální sítě můžete upravit tyto informace z hello virtuální síť uživatelského prostředí. Pokud změníte hello DNS hello virtuální síť, musíte tooperform synchronizace síťové operace.

Když vytvoříte klasické virtuální sítě pomocí hello uživatelského rozhraní integrace virtuální sítě, vytvoří virtuální síť v hello stejné skupině prostředků jako aplikace. 

## <a name="how-hello-system-works"></a>Jak funguje hello systému
V části hello zahrnuje sestavení tato funkce nad Point-to-Site VPN technologie tooconnect vaší aplikace tooyour virtuální sítě. Aplikace v Azure App Service musí mít architektura víceklientské systému, který vylučuje zřizování aplikace přímo ve virtuální síti, jak se provádí s virtuálními počítači. Díky podpoře technologie point-to-site je omezená síťový přístup toojust hello virtuálního počítače hostování aplikace hello. Přístup k síti toohello je další omezený na těchto hostitelích aplikace tak, aby vaše aplikace přístup jenom k hello sítě, můžete je nakonfigurovat tooaccess. 

![][4]

Pokud jste nenakonfigurovali DNS server s vaší virtuální sítě, vaše aplikace bude nutné toouse IP adresy tooreach prostředku v hello virtuální sítě. Při použití IP adresy, mějte na paměti, že hello hlavní výhody použití této možnosti je, že umožňuje toouse hello privátní adresy v rámci vaší privátní sítě. Pokud nastavíte aplikace si toouse veřejné IP adresy pro jeden virtuální počítače, pak nepoužíváte funkce hello integrace virtuální sítě a komunikují přes hello Internetu.

## <a name="managing-hello-vnet-integrations"></a>Správa integrace hello virtuální sítě
Hello možnost tooconnect a odpojit tooa virtuální sítě je na úrovni aplikace. Operace, které můžou ovlivnit hello virtuální síť integrace mezi více aplikacemi jsou na úrovni ASP. Z uživatelského rozhraní, které se zobrazí na úrovni aplikace hello hello můžete získat podrobnosti o ve virtuální síti. Většina hello stejné informace se také zobrazí v hello ASP úrovně. 

![][5]

Na stránce hello stav funkce sítě se zobrazí, pokud je vaše aplikace tooyour připojené virtuální sítě. Pokud vaše Brána virtuální sítě je vypnutý z jakéhokoliv důvodu, potom zobrazí jako není připojen. 

Hello informace, že máte k dispozici tooyou nyní v aplikaci hello stejné úrovně uživatelského rozhraní integrace virtuální sítě je hello jako hello podrobné informace, které můžete získat z hello ASP. Zde jsou tyto položky:

* Název virtuální sítě - tento odkaz otevře hello virtuální síť Azure uživatelského rozhraní
* Umístění – tento údaj zohledňuje hello umístění virtuální sítě. Je možné toointegrate s virtuální sítě v jiném umístění.
* Stav certifikátu – existují certifikáty používané toosecure hello VPN připojení mezi aplikace hello a hello virtuální sítě. Tento údaj zohledňuje testovací tooensure, jsou synchronizované.
* Stav brány - by měla být jako vašich bran dolů z jakéhokoliv důvodu pak aplikace nemají přístup k prostředkům v hello virtuální sítě. 
* Adresní prostor sítě VNet - to je hello adresní prostor IP adres pro virtuální síť. 
* Bod tooSite adresní prostor - Toto je hello bodu toosite adresní prostor IP adres pro virtuální síť. Vaše aplikace zobrazí komunikace jako pocházející z jednoho z hello IP adresy v tento adresní prostor. 
* Lokality toosite adresní prostor – můžete použít lokality tooSite VPN tooconnect vaší virtuální sítě tooyour místní prostředky nebo tooother virtuální sítě. Pokud máte nakonfigurovaný a rozsahy IP hello definovaný s zde znázorňuje připojení k síti VPN.
* Servery DNS – Pokud máte nakonfigurované virtuální sítě, servery DNS, pak jsou zde uvedeny.
* IP adresy směrované toohello virtuální síť – nejsou seznam IP adres, které virtuální sítě má definovaný pro směrování a zde se zobrazí tyto adresy. 

Hello pouze operace, které můžete provést v zobrazení aplikace hello integrace vaší virtuální sítě je toodisconnect aplikaci z hello je aktuálně připojen k virtuální síti. toodo tento jednoduše klikněte na tlačítko Odpojit v horní části hello. Tato akce nezmění vaši virtuální síť. Hello virtuální sítě a jeho konfigurace, včetně brány hello zůstává beze změny. Pokud pak chcete toodelete virtuální síť, musíte toofirst odstranění hello prostředky v ní včetně hello brány. 

Hello zobrazení plánu služby App Service má několik dalších operací. Je také přístupný jinak než z aplikace hello. tooreach hello uživatelského rozhraní sítě ASP jednoduše otevřete uživatelské rozhraní ASP a projděte dolů. Je prvek uživatelského rozhraní s názvem stav funkce sítě. Nabízí některé dílčí podrobnosti ohledně integrace vaší virtuální sítě. Kliknutím na toto uživatelské rozhraní otevře hello síťové funkce stav uživatelského rozhraní. Pokud pak kliknete na "Kliknutím sem toomanage", hello uživatelské rozhraní, které jsou uvedené hello otevře integrace virtuální sítě v této ASP.

![][6]

Při vyhledávání v umístění hello hello virtuální sítě budete nástroj integrovat, je dobré tooremember Hello umístění hello ASP. Hello virtuální sítě je v jiném umístění jste daleko vyšší pravděpodobnost problémy s latencí toosee. 

Hello integrovat virtuální sítě je připomenutí na tom, kolik virtuálních sítí, že vaše aplikace jsou integrované s v této ASP a kolik můžete mít. 

toosee přidána na každý virtuální síť, jednoduše klikněte na hello virtuální sítě, které vás zajímají podrobnosti. Kromě toho toohello podrobnosti, které jste si poznamenali dříve, můžete také zobrazí seznam aplikací hello v této ASP, které používají tuto virtuální síť. 

S ohledem tooactions dispozici jsou dvě primární akce. Hello je nejdřív hello možnost tooadd trasy, jež řídí odchozího provozu z vaší aplikace do virtuální sítě. druhou akci Hello je hello možnost toosync certifikáty a informace o síti.

![][7]

**Směrování** jako si předtím poznamenali hello tras, které jsou definovány ve vaší virtuální síti se, co se používá pro směrují přenosy do virtuální sítě z vaší aplikace. Když místo, kde Zákazníci má toosend další odchozí provoz z aplikace do hello virtuální síť a je tato možnost je poskytována existují některé používá. Co se stane toohello provozu, po které si zákazník hello toohow nakonfiguruje své virtuální síti. 

**Certifikáty** hello stav certifikátu odráží kontrolu prováděného hello toovalidate služby App Service, jsou stále vhodné hello certifikáty, které se používá pro hello připojení VPN. Když je povolené integrace virtuální sítě, pokud je to hello první toothat integrace virtuální sítě ze všech aplikací v této ASP nejsou požadované exchange certifikáty tooensure hello zabezpečení hello připojení. Společně s certifikáty hello se nám získat konfiguraci DNS hello, trasy a další podobné věci, které popisují hello sítě.
Pokud tyto certifikáty nebo informace o síti je změnit, musíte tooclick "Synchronizace sítě". **Poznámka:**: Po kliknutí na tlačítko "Synchronizace síť", pak způsobit kratší výpadek připojení mezi vaší aplikace a virtuální síť. Při restartování aplikace není hello ztráty připojení může způsobit vaše lokality toonot funkce správně. 

## <a name="accessing-on-premises-resources"></a>Přístup k místní prostředky
Jednou z výhod hello hello funkce integrace virtuální sítě je, že pokud je připojený virtuální síť tooyour místní síť s lokality tooSite VPN pak vaše aplikace může mít přístup k prostředkům místně tooyour z vaší aplikace. Pro tento toowork i když může být nutné tooupdate směruje vaší místní brány sítě VPN s hello pro bod tooSite rozsahu IP adres. Když hello lokality tooSite VPN je nejdřív nastavit skripty hello použit tooconfigure měli nastavit cesty včetně tooSite bodu VPN. Pokud přidáte hello tooSite bodu VPN po vytvoření vaší lokality tooSite sítě VPN, budete potřebovat tooupdate hello trasy ručně. Podrobnosti o tom, jak se toodo, který se liší u každé brány a nejsou zde popsané. 

> [!NOTE]
> funkce integrace virtuální sítě Hello Neintegruje aplikace s virtuální sítě, který má bránu ExpressRoute. I když hello bránu ExpressRoute je nakonfigurovaný v [koexistence režimu] [ VPNERCoex] hello integrace virtuální síť nefunguje. Pokud potřebujete tooaccess prostředkům prostřednictvím připojení ExpressRoute, pak můžete použít [App Service Environment][ASE], která se spouští ve vaší virtuální síti.
> 
> 

## <a name="pricing-details"></a>Podrobnosti o cenách
Existuje několik ceny drobné odlišnosti, kterých byste měli vědět, když používáte funkce hello integrace virtuální sítě. Existují 3 související poplatky toohello používání této funkce:

* Cenová úroveň požadavků ASP
* Náklady na přenos dat
* Brána sítě VPN náklady.

Pro vaše aplikace může toouse toobe této funkce, které potřebují toobe v Standard nebo Premium aplikace služby plánování. Další podrobnosti uvidíte na zde uvedené náklady: [App Service – ceny][ASPricing]. 

Z důvodu toohello způsob bodu tooSite jsou zpracovávány sítě VPN, budete mít vždy zpoplatněny pro odchozí data prostřednictvím připojení k virtuální síti integrace i v případě hello virtuální sítě je v hello stejné datové centrum. toosee co jsou tyto poplatky, podívejte se sem: [přenosu informace o cenách datových][DataPricing]. 

poslední položky Hello je hello náklady hello brány virtuální sítě. Pokud nepotřebujete hello brány na něco jiného, například lokality tooSite sítě VPN, pak platíte pro funkci integrace virtuální sítě hello toosupport brány. Podrobnosti jsou na těchto náklady zde: [ceny služby VPN Gateway][VNETPricing]. 

## <a name="troubleshooting"></a>Řešení potíží
Při funkce hello je snadno tooset nahoru, neznamená, že budou vaše zkušenosti problém volné. Měli narazíte na problémy s přístupem k dispozici požadovaný koncový bod se některé nástroje můžete použít tootest připojení z konzoly aplikace hello. Existují dvě konzoly prostředí, které můžete použít. Jedna je z konzoly Kudu hello a hello jiných hello konzoly, která můžete uživatele oslovit ve hello portálu Azure. tooget toohello Kudu konzoly z vaší aplikace přejděte tooTools -> Kudu. Toto je stejný hello jako přejdete příliš [sitename]. scm.azurewebsites.net. Jakmile toohello kartě tooget této otevře jednoduše přejděte toohello ladění konzoly portálu hostované konzoly Azure z vaší aplikace. poté přejděte tooTools -> Konzola. 

#### <a name="tools"></a>Nástroje
ping Hello nástrojů, nslookup a tracert nebude fungovat prostřednictvím konzoly hello z důvodu omezení toosecurity. toofill hello existuje void byly dvou samostatných nástrojů, které jsou přidány. V pořadí tootest DNS funkce jsme přidali nástroje s názvem nameresolver.exe. Hello syntaxe je:

    nameresolver.exe hostname [optional: DNS Server]

Můžete použít názvy nameresolver toocheck hello hostitelů, které závisí aplikace na. Tímto způsobem můžete otestovat, pokud máte nic nemá nakonfigurovaný s DNS nebo možná nemáte přístup tooyour DNS server.

Další nástroje Hello umožňuje tootest pro kombinace TCP připojení tooa hostitele a portu. Tento nástroj se nazývá tcpping.exe a hello syntaxe je:

    tcpping.exe hostname [optional: port]

Nástroj tcpping Hello zjistíte, pokud lze kontaktovat konkrétního hostitele a port. Jenom je zobrazují úspěch pokud: aplikace naslouchá na kombinace hello hostitele a portu, a je přístup k síti z vaší aplikace toohello zadaného hostitele a portu.

#### <a name="debugging-access-toovnet-hosted-resources"></a>Ladění tooVNet přístup k hostovaným prostředkům
Existuje několik věcí, které může zabránit aplikaci v dosažení konkrétního hostitele a port. Většinu času hello představuje jednu ze tří věcí:

* **Je brána firewall v hello způsob** Pokud máte bránu firewall způsobem hello, se setkají hello TCP vypršení časového limitu. Který v tomto případě je 21 sekund. Použijte hello tcpping nástroj tootest připojení. Překročení časového limitu TCP dají kvůli toomany věcí mimo brány firewall, ale existuje spustit. 
* **DNS není dostupný** hello DNS vypršení časového limitu je tři sekundy na jeden DNS server. Pokud máte dva servery DNS, je časový limit hello 6 sekund. Nameresolver toosee použijte, pokud služba DNS pracuje. Mějte na paměti, že nslookup nelze použít jako, který nepoužívá hello DNS jsou vaše virtuální síť nakonfigurované.
* **Neplatný rozsah P2S IP** rozsah IP adres hello toosite bodu musí toobe v dokumentu RFC 1918 hello rozsahy privátních IP (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255). Pokud hello rozsah IP adres používá mimo který, nebude fungovat věcí. 

Pokud váš problém neobsahují odpověď na ty položky, vyhledejte první pro hello jednoduché věcmi, jako jsou: 

* Zobrazit hello brány jako používán hello portál?
* Certifikáty zobrazit, že je synchronizace?
* Kdokoliv změnit konfiguraci sítě hello bez provádění "Sítě synchronizace" v soubory ASP hello vliv? 

Pokud vaše brána je vypnutý, pak ho převeďte zálohování. Pokud vaše certifikáty nejsou synchronizované, přejděte toohello ASP zobrazení integrace vaší virtuální sítě a stiskněte tlačítko "Synchronizace sítě". Pokud máte podezření, že byl konfiguraci virtuální sítě změny provedené tooyour a jeho nebyl synchronizace by se vaše soubory ASP, pak přejděte toohello ASP zobrazení integrace vaší virtuální sítě a stiskněte tlačítko "Sítě synchronizace" těsně jako připomenutí, protože to způsobuje kratší výpadek s připojením k virtuální síti a aplikace . 

Pokud všechny, je v pořádku, je třeba toodig za chvilku podrobněji:

* Jsou zde všechny ostatní aplikace pomocí integrace virtuální sítě tooreach prostředky v hello stejnou virtuální síť? 
* Můžete přejít toohello aplikaci konzoly a použít tcpping tooreach žádné další prostředky ve vaší virtuální síti? 

Pokud platí některá z výše uvedených hello, pak je dobře vaše integrace virtuální sítě a problém hello je někde jinde. Toto je, kde získá toobe více výzvu vzhledem k tomu, že neexistuje žádná toosee jednoduchý způsob, proč nelze připojit hostitele a portu. Mezi hello příčiny patří:

* máte bránu firewall na hostiteli brání přístupu toohello aplikace portu z rozsahu IP adres toosite bodu. Při překročení podsítě často vyžaduje veřejný přístup.
* Cílový hostitel je mimo provoz
* aplikace je mimo provoz
* jste měli hello nesprávný IP adresu nebo název hostitele
* vaše aplikace naslouchá na jiný port než jste očekávali. Zkontrolovat to můžete tak, že přejdete na tomto hostiteli a pomocí "netstat - aon" z hello příkazový řádek. To se dozvíte, co proces ID naslouchá na port. 
* Vaše skupiny zabezpečení sítě jsou nakonfigurované tak, že brání tomu, aby přístup tooyour aplikace hostitele a portu z rozsahu IP adres toosite bodu

Mějte na paměti, že nevíte jaké IP v rozsahu tooSite bodu IP adres, aplikace bude používat, proto musíte tooallow přístup z hello celý rozsah adres. 

Další ladění krokům patří:

* Přihlaste se jiným virtuálním Počítačem v tooreach vaší virtuální sítí a pokus o prostředku hostitele: port z ní. Existují některé nástroje ping TCP musí být pro tento účel můžete použít nebo i pokud můžete použít telnet. Hello účelem tady je právě toodetermine Pokud připojení je k dispozici z jiných tohoto virtuálního počítače. 
* Otevřete aplikaci na jiným virtuálním Počítačem a testování přístup toothat hostitele a portu z konzoly hello z vaší aplikace.

#### <a name="on-premises-resources"></a>Místní prostředky ####
Pokud vaše aplikace nebudou moci připojit místní prostředky, hello první věcí, kterou byste měli zkontrolovat, je-li se lze připojit prostředku ve vaší virtuální síti. Pokud to funguje, zkuste tooreach hello místní aplikace z virtuálního počítače v hello virtuální sítě. Můžete použít protokol telnet nebo nástroj ping protokolu TCP. Pokud virtuální počítač nelze dosáhnout místnímu prostředku a potom zkontrolujte, zda že funguje připojení VPN tooSite lokality. Pokud funguje, zkontrolujte hello stejné operace si předtím poznamenali a také konfigurace brány místní hello a stav. 

Teď Pokud hostované virtuální sítě virtuálních počítačů dosáhnout v místním systému, ale aplikace nelze pak hello důvod je jedním z následujících hello:

* trasy nejsou nakonfigurováni vaší rozsahy IP bodu toosite ve vaší místní brány
* Vaše skupiny zabezpečení sítě blokují přístup k bodu tooSite rozsahu IP adres
* vaše místní brány firewall, blokují přenosy z tohoto bodu tooSite rozsahu IP adres
* Máte Route(UDR) definované uživatele ve vaší virtuální síti, která zabraňuje provozu na základě tooSite bodu přístup do místní sítě

## <a name="hybrid-connections-and-app-service-environments"></a>Hybridní připojení a služby App Service Environment
Existují tři funkce, které umožňují přístup k prostředkům hostované tooVNet. Jsou:

* Integrace virtuální sítě
* Hybridní připojení
* Prostředí App Service

Hybridní připojení vyžaduje tooinstall přenosového agenta názvem hello hybridní připojení Manager(HCM) ve vaší síti. Hello HCM musí mít tooconnect tooAzure toobe a také tooyour aplikace. Toto řešení je obzvláště skvělé z vzdálené sítě, například v místní síti nebo i jiného cloudu hostovaný sítě, protože nevyžaduje internet přístupném koncovém bodu. Hello HCM lze spustit pouze v systému Windows a může obsahovat maximálně toofive instancí spuštěných tooprovide vysokou dostupnost. Hybridní připojení, když pouze podporuje TCP a každý koncový bod HC má toomatch tooa konkrétní hostitele a portu kombinaci. 

funkce služby App Service Environment Hello umožňuje toorun instanci hello Azure App Service ve vaší virtuální síti. To umožňuje vaší aplikace přístup k prostředkům ve vaší virtuální síti bez jakékoli dodatečné kroky. Některé z hello jiné službě App Service Environment výhody, které můžete použít na základě Dv2 pracovníci s až too14 GB paměti RAM. Další výhodou je, že je možné škálovat hello systému toomeet vašim potřebám. Na rozdíl od hello víceklientské prostředí, kde vaše ASP je omezená too20 instance v App Service Environment můžete postupně škálovat too100 ASP instancí. Jedním z hello věcí, kterou poskytuje App Service Environment, který není v rámci integrace virtuální sítě je, že služby App Service Environment může fungovat s připojení VPN pomocí ExpressRoute. 

Zatímco je, že některé použít případu překrývají, žádný z těchto funkcí můžete nahradit všechny hello ostatní. Zároveň budete vědět, jaké funkce toouse je vázané tooyour potřebám. Například:

* Pokud se vývojáře a jednoduše má toorun webu v Azure a mají přístup hello databáze na pracovní stanici hello v kanceláři, pak toouse co nejjednodušší hello je hybridní připojení. 
* Pokud jsou velké organizace, která chce tooput velký počet webových vlastností ve veřejném cloudu hello a spravovat ve vlastní síti, pak budete chtít toogo s hello App Service Environment. 
* Pokud máte několik služby App Service hostované aplikace a jednoduše má tooaccess prostředky ve vaší virtuální síti, pak se toogo hello způsob integrace virtuální sítě. 

Kromě případů použití hello, jsou některé jednoduchost související aspekty. Pokud virtuální sítě je již připojen tooyour místní síti, pak pomocí integrace virtuální sítě nebo služby App Service Environment se o snadný způsob tooconsume místních prostředků. Na hello druhé straně, pokud není virtuální sítě připojen tooyour do místní sítě, pak je mnoho, které další režie tooset až toosite site VPN s vaší virtuální sítě ve srovnání s instalací hello HCM. 

I mimo hello funkční rozdíly, jsou také ceny rozdíly. funkce služby App Service Environment Hello je služba Premium nabídky ale nabízí hello většina možnosti konfigurace sítě kromě tooother funkcí. Integrace virtuální sítě můžete použít s Standard nebo Premium soubory ASP a je ideální pro bezpečné použití prostředky ve virtuální síti ze hello víceklientské služby App Service. Hybridní připojení se aktuálně závisí na BizTalk účtu, který má cenové úrovně, které spustit volné a potom získat progresivně nákladnější na základě hello množství, které potřebujete. Pokud jde tooworking v sítích s mnoha Přestože, neexistuje žádné jiné funkce, jako je hybridní připojení, která můžete povolit tooaccess prostředků do samostatných sítí a více než 100. 

<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://docs.microsoft.com/azure/app-service/app-service-environment/intro
[ILBASE]: http://docs.microsoft.com/azure/app-service/app-service-environment/create-ilb-ase
[V2VNETPortal]: https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal
[VPNERCoex]: http://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-coexist-resource-manager
[ASE]: http://docs.microsoft.com/azure/app-service/app-service-environment/intro
