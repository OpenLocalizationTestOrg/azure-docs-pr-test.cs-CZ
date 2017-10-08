---
title: "aaaInfrastructure a připojení tooSAP HANA v Azure (velké instance) | Microsoft Docs"
description: "Nakonfigurujte požadované připojení infrastruktury toouse SAP HANA v Azure (velké instance)."
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
ms.openlocfilehash: 0af34fbd82413bf63981036a76eaa264d8299ec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-large-instances-infrastructure-and-connectivity-on-azure"></a>Infrastruktura SAP HANA (velké instance) a připojení v Azure 

Definice předem před čtením této příručky. V [přehled SAP HANA (velké instance) a architektura v Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) zavedli jsme dvěma různými třídami velké Instance HANA jednotek s:

- S72, S72m, S144, S144m, S192 a S192m, které označujeme tooas hello "Typ I třídy" z jednotky SKU.
- S384, S384m, S384xm, S576, S768 a S960, které označujeme tooas hello "Typu II třídou" SKU.

specifikátory třídy Hello jsou probíhající toobe používaných v celém hello HANA velké Instance tooeventually dokumentaci najdete toodifferent možnosti a požadavky založené na HANA velké Instance SKU.

Další definice, které jsme používané jsou:
- **Velké Instance razítka:** zásobníku infrastruktury hardwaru, který je SAP HANA TDI certifikaci a vyhrazené instance SAP HANA toorun v rámci Azure.
- **SAP HANA v Azure (velké instance):** oficiální název hello nabídka v Azure toorun HANA instance v na SAP HANA TDI certifikaci hardwaru, která je nasazena v velké Instance razítka v různých oblastech Azure. Hello související termín **HANA velké Instance** je zkratka pro SAP HANA v Azure (velké instance) a je široce používá tato příručka technického nasazení.
 

Po zakoupení hello SAP HANA v Azure (velké instance) je dokončené mezi vámi a hello týmu účtů Microsoft enterprise, hello následující informace vyžadují Microsoft toodeploy HANA velké Instance jednotky:

- Jméno zákazníka
- Firemní kontaktní údaje (včetně e-mailová adresa a telefonní číslo)
- Technické kontaktní informace (včetně e-mailová adresa a telefonní číslo)
- Technické síťové kontaktní informace (včetně e-mailová adresa a telefonní číslo)
- Oblast Azure nasazení (západní USA, Východ USA, Austrálie – východ, Austrálie – jihovýchod, západní Evropa a Severní Evropa od července 
- 2017)
- Potvrďte SAP HANA na Azure (velké instance) SKU (konfigurace)
- Jak již podrobně hello přehled a architektura dokumentu pro velké instance HANA, pro každou oblast Azure, který je nasazován na:
    - / 29 rozsah IP adres pro ER P2P připojení, který připojení virtuální sítě Azure tooHANA velké instancí
    - / 24 používá blok CIDR pro hello HANA velké instance serveru fond IP adres
- Hello IP adresa rozsahu hodnoty používané v atributu adresní prostor sítě VNet hello každý virtuální síť Azure, která se připojuje toohello HANA velké instancí
- Data pro každou HANA velké instancí systému:
  - Požadované hostname - ideálně s plně kvalifikovaný název domény.
  - Požadovanou IP adresu pro jednotku velké Instance HANA hello mimo hello rozsah adres fondu IP serverů - mějte na paměti této hello prvních 30 IP adresy ve fondu serverů IP rozsah adres jsou vyhrazené pro interní použití v rámci instancí velké HANA hello
  - SAP HANA SID název instance SAP HANA hello (požadované toocreate hello potřeby související SAP HANA disku svazky). Hello HANA SID je nezbytné k vytváření hello oprávnění pro <sidadm> na hello svazků systému souborů NFS, které jsou získávání jednotky připojené toohello HANA velké Instance. Také je používán jako jeden z hello název součástí hello diskové svazky, které získat připojené. Pokud chcete toorun více než jednu instanci HANA na jednotce hello, musíte toolist více HANA identifikátory SID. Každé z nich získá samostatnou sadu svazky, které jsou přiřazeny.
  - Hello groupid hello hana sidadm má uživatel v hello operační systém Linux je požadovaná toocreate hello potřeby související SAP HANA diskové svazky. Hello SAP HANA instalace obvykle vytvoří hello sapsys skupinu s id skupiny 1001. Hello hana sidadm uživatel je součástí této skupiny
  - Hello userid hello hana sidadm má uživatel v hello operační systém Linux je požadovaná toocreate hello potřeby související SAP HANA diskové svazky. Pokud používáte více instancí HANA na jednotce hello, je třeba toolist všechny hello <sid>uživatelé adm 
- ID předplatného Azure pro hello předplatného Azure toowhich SAP HANA v instancích velké HANA Azure budou toobe přímo připojené. Toto ID předplatného odkazuje hello předplatné Azure, který se bude, že toobe zpoplatněn hello jednotka HANA velké Instance.

Po zadání hello informace, Microsoft zřizuje SAP HANA v Azure (velké instance) a vrátí hello informace nezbytné toolink vaší sítě Azure Vnet tooHANA velké instancí a tooaccess hello velké Instance HANA jednotky.

## <a name="connecting-azure-vms-toohana-large-instances"></a>Připojení virtuálních počítačích Azure tooHANA velké instancí

Jak již bylo uvedeno v [přehled SAP HANA (velké instance) a architektura v Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) hello minimální nasazení velké instancí HANA s hello SAP aplikační vrstvu v Azure vypadá jako:

![Virtuální síť Azure připojené tooSAP HANA na Azure (velké instance) a na místě](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Vyhledávání blíže na hello straně virtuální síť Azure, Uvědomujeme potřebu hello:

- definice Hello virtuální síť Azure, který je toodeploy toobe používá probíhající hello SAP aplikační vrstvu do hello virtuální počítače.
- Který automaticky znamená, že výchozí podsíť v hello se definuje virtuální síť Azure, která je skutečně hello jeden použité toodeploy hello virtuálních počítačů do.
- Hello virtuální síť Azure, který se vytvoří musí toohave alespoň jednu podsíť virtuálních počítačů a jednu podsíť brány ExpressRoute. Tyto podsítě by se měla přiřadit hello rozsahy IP adres jako zadaný a popsaných v následující části hello.

Ano Podíváme se poněkud blíže do hello vytvoření virtuální sítě Azure pro velké instance HANA

### <a name="creating-hello-azure-vnet-for-hana-large-instances"></a>Vytváření hello virtuální síť Azure pro velké instance HANA

>[!Note]
>Hello virtuální síť Azure pro velké instanci HANA musí být vytvořen pomocí modelu nasazení Azure Resource Manager hello. Hello starší nasazení Azure modelu, všeobecně známé jako model nasazení classic nepodporuje hello velké Instance HANA řešení.

Hello virtuální sítě můžete vytvořit pomocí hello portálu Azure, PowerShell, šablony Azure nebo rozhraní příkazového řádku Azure (viz [vytvoření virtuální sítě pomocí portálu Azure hello](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). V následujícím příkladu hello podíváme do virtuální sítě vytvořené pomocí hello portálu Azure.

Pokud jsme viděl hello Definice o virtuální síť Azure prostřednictvím hello portálu Azure, podíváme se na některé z definice hello a jak těch, které se týkají toowhat jsme seznam různých rozsahů IP adres. Jak jsme mluvíme o hello **adresní prostor**, jsme znamenat hello adresní prostor, který hello virtuální síť Azure je povoleno toouse. Tento adresní prostor je také hello rozsah adres, který hello používá virtuální síť pro šíření trasy protokolu BGP. To **adresní prostor** můžete zobrazit tady:

![Zobrazí adresu místa Azure VNet v hello portálu Azure](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

V případě hello předcházející s 10.16.0.0/16 byl zadán hello virtuální síť Azure místo velké a široké rozsah toouse adres IP. Znamená, že všechny hello rozsahů adres IP následné podsítí v rámci této virtuální sítě může mít jejich rozsahů v rámci tohoto adresního prostoru. Obvykle nejsou doporučujeme rozsah velkých adres pro jednu virtuální síť v Azure. Ale získání další krok, podíváme se na hello podsítě definované v hello virtuální sítě Azure:

![Azure podsítě virtuální sítě a jejich rozsahy IP adres](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Jak vidíte, podíváme na virtuální síť s první podsíť virtuálních počítačů (zde nazývané "default") a podsíť s názvem "GatewaySubnet".
V následující části hello, označujeme rozsah IP adres toohello hello podsítě, která se nazývá "default" hello grafického jako **rozsah adres IP podsítě virtuálních počítačů Azure**. V následujících částech hello, rozsah IP adres hello podsíť brány toohello označujeme jako **rozsah adres IP adresu podsítě brány virtuální sítě**. 

V případě hello prokázat hello dvou grafika výše, zobrazí tento hello **adresní prostor sítě VNet** se vztahuje na, hello **rozsah adres IP podsítě virtuálních počítačů Azure** a hello **adresa IP podsíť brány virtuální sítě rozsah**. 

V ostatních případech, které vyžadují tooconserve nebo být konkrétní rozsahy IP adres, můžete omezit hello **adresní prostor sítě VNet** rozsahů konkrétní toohello virtuální sítě používá každá podsíť. V takovém případě můžete definovat hello **adresní prostor sítě VNet** z virtuální sítě jako více konkrétní rozsahy, jak je vidět tady:

![Azure adresní prostor sítě VNet dva prostory](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

V takovém případě hello **adresní prostor sítě VNet** má dva prostory definované. Tyto dva prostory jsou identické toohello rozsahy IP adres definované pro **rozsah adres IP podsítě virtuálních počítačů Azure** a hello **rozsah adres IP adresu podsítě brány virtuální sítě.**

Můžete použít všechny standardní pojmenování, které chcete pro tyto podsítě klienta (podsítě virtuálních počítačů). Ale **musí být vždy jedna a pouze jedna podsíť brány pro každý virtuální síť** která se připojuje toohello SAP HANA v okruhu ExpressRoute Azure (velké instance). A **tuto podsíť brány musí vždy mít název "GatewaySubnet"** tooensure správné umístění hello bránu ExpressRoute.

> [!WARNING] 
> Je důležité, že tuto podsíť brány hello vždy je s názvem "GatewaySubnet".

Může použít více podsítí virtuálních počítačů i využitím rozsahy nesouvislé adres. Jak je uvedeno nahoře, tyto rozsahy adres musí být pokryté komponentami hello, ale **adresní prostor sítě VNet** hello virtuální sítě v souhrnné podobě nebo v seznamu hello přesnou rozsahy hello podsítí virtuálních počítačů a hello podsíť brány.

Shrnutí hello důležitých faktů o virtuální síť Azure, propojující tooHANA velké instancí:

- Je třeba toosubmit tooMicrosoft hello **adresní prostor sítě VNet** při provádění je počáteční nasazení HANA velké instancí. 
- Hello **adresní prostor sítě VNet** může být jeden větší rozsah, které pokrývá hello rozsah pro virtuální počítač Azure podsítě IP adres rozsahy a rozsah adres IP adresu podsítě brány virtuální sítě hello.
- Nebo můžete odeslat jako **adresní prostor sítě VNet** rozsahy rozsahy adres IP podsíť virtuálních počítačů a rozsah adres IP adresu podsítě brány virtuální sítě hello více rozsahů, které se týkají hello různých IP adres.
- Hello definované **adresní prostor sítě VNet** je použité šíření směrování protokolu BGP.
- musí být Hello název podsítě brány hello: **"GatewaySubnet".**
- Hello **adresní prostor sítě VNet** slouží jako filtr na hello velké Instance HANA straně tooallow nebo zakáže jednotky velké Instance HANA toohello provoz z Azure. Pokud hello informace o směrování protokolu BGP z hello virtuální síť Azure a rozsahů IP adres hello nakonfigurovaný pro filtrování na straně HANA velké Instance hello neshodují, mohou se vyskytnout potíže v připojení.
- Existují některé podrobnosti o hello podsíť brány, které jsou popsané níže v části "připojení virtuální sítě tooHANA velké Instance ExpressRoute.



### <a name="different-ip-address-ranges-toobe-defined"></a>Rozsahy toobe definované jinou IP adresu 

Již zavedli jsme některé hello IP adresy rozsahů nezbytné toodeploy HANA velké instancí v předchozích částech. Ale existují některé další rozsahy IP adres, které jsou důležité. Přejděte prostřednictvím některé další podrobnosti. Hello následující adresy IP, z nichž všechny potřebovat toobe odeslat tooMicrosoft potřebovat toobe definované před odesláním požadavku pro počáteční nasazení:

- **Virtuální síť adresní prostor:** už zavedená dříve, nebo jsou hello IP adresu range(s) jste přiřadili (nebo plánování tooassign) tooyour místo parametr adresa v hello virtuální sítě Azure (VNet) připojení toohello velké Instance SAP HANA prostředí. Doporučuje se, že tento parametr adresní prostor je Víceřádkový hodnota tvořená hello podsíť virtuálních počítačů Azure rozsahy a hello rozsahu podsítě brány Azure, jak je znázorněno v hello grafiky dříve. Tento rozsah musí nepřekrývají s místními nebo rozsahů adres IP fondu serverů nebo ER P2P. Jak tooget je, že tyto rozsahy IP adres? Poskytovatel tým nebo služby podnikové síti by měl poskytovat jeden nebo více IP adres rozsahy, které nebo nejsou použity uvnitř vaší sítě. Příklad: Pokud je vaše podsíť virtuálních počítačů Azure (viz výše) 10.0.1.0/24 a podsítě brány Azure (viz následující) je 10.0.2.0/28, pak váš adresní prostor virtuální sítě Azure se doporučuje toobe dva řádky; 10.0.1.0/24 a 10.0.2.0/28. Přestože se dají agregovat hodnoty hello adresní prostor, je vhodné odpovídající je toohello podsíť rozsahy rozsahů v rámci větší adresní prostory v hello budoucí tooavoid náhodnému opakované použití nepoužívané IP jinde adres ve vaší síti. **Hello virtuální síť adresní prostor je rozsah IP adres, které musí toobe odeslána tooMicrosoft při požadující počáteční nasazení**

- **Azure rozsah adres IP podsítě virtuálních počítačů:** hello jeden přiřadili (nebo plánování tooassign) je rozsah adres tuto adresu IP, jak je popsáno dříve již parametr podsíť virtuální sítě Azure toohello v prostředí SAP HANA velké Instance toohello připojení virtuální sítě Azure . Tento rozsah IP adres je použité tooassign IP adresy tooyour virtuálních počítačích Azure. Hello IP adresy mimo tento rozsah mohou tooconnect tooyour servery velké Instance SAP HANA. V případě potřeby lze několik podsítí virtuálních počítačů Azure. A/24 blok CIDR se společnost Microsoft doporučuje pro každou podsíť virtuálních počítačů Azure. Tento rozsah adres musí být součástí hello hodnoty používané v hello adresní prostor sítě VNet Azure. Jak tooget tento rozsah IP adres? Poskytovatel tým nebo služby podnikové síti by měl poskytovat rozsah IP adres, který není právě používána uvnitř vaší sítě.

- **Rozsah adres IP adresu podsítě brány virtuální sítě:** v závislosti na hello funkce, které máte v plánu toouse, hello doporučená velikost by byla:
   - Brány úrovně Ultra-performance ExpressRoute: / 26 blok adres - vyžaduje pro typ II třídu SKU
   - Koexistenci se připojení VPN a ExpressRoute pomocí vysoce výkonné brány ExpressRoute (nebo menší): / 27 blok adres
   - Další situace: / 28 blok adres. Tento rozsah adres musí být součástí hello hodnoty používané v hodnoty "Adresní prostor virtuální sítě" hello. Tento rozsah adres musí být součástí hello hodnoty používané v hello adresní prostor sítě VNet Azure hodnoty, je nutné, aby toosubmit tooMicrosoft. Jak tooget tento rozsah IP adres? Poskytovatel tým nebo služby podnikové síti by měl poskytovat rozsah IP adres, který není právě používána uvnitř vaší sítě. 

- **Rozsah pro připojení k síti ER P2P adres:** tento rozsah je hello rozsah IP adres pro připojení k SAP HANA velké Instance ExpressRoute (ER) P2P. Tento rozsah IP adres musí být/29 rozsah CIDR IP adres. Tento rozsah musí nepřekrývají s vaší místní nebo jiné IP Azure rozsahy adres. Tento rozsah IP adres je použité tooset hello ER připojením z vašich serverů brány ExpressRoute virtuální síť Azure toohello velké Instance SAP HANA. Jak tooget tento rozsah IP adres? Poskytovatel tým nebo služby podnikové síti by měl poskytovat rozsah IP adres, který není právě používána uvnitř vaší sítě. **Tento rozsah je rozsah IP adres, které musí toobe odeslána tooMicrosoft při požadující počáteční nasazení**
  
- **Rozsah adres fondu IP serveru:** tento rozsah IP adres je použité tooassign hello jednotlivé IP adresy tooHANA velké instance serverů. Hello doporučená velikost podsítě je/24 CIDR blokovat – ale pokud potřeby může být menší minimální tooa poskytnout 64 IP adresy. Z tohoto rozsahu hello prvních 30 IP adresy jsou vyhrazené pro použití společností Microsoft. Zkontrolujte, zda že je tento fakt pozornost při výběru hello velikost hello rozsahu. Tento rozsah musí nepřekrývají s vaší místní nebo jiné Azure IP adresy. Jak tooget tento rozsah IP adres? Poskytovatel tým nebo služby podnikové síti by měl poskytovat rozsah IP adres, který není právě používána uvnitř vaší sítě. / 24 (doporučeno) jedinečný CIDR blokovat toobe slouží k nastavení hello konkrétní IP adresy potřebné pro SAP HANA v Azure (velké instance). **Tento rozsah je rozsah IP adres, které musí toobe odeslána tooMicrosoft při požadující počáteční nasazení**
 
Když potřebujete toodefine a plánování rozsahů adres IP hello výše, ne všechny je nutné tooMicrosoft toobe přenosu. toosummarize hello výše, hello rozsahy IP adres jsou požadované tooname tooMicrosoft jsou:

- Azure VNet adresu mezerou (mezerami)
- Rozsah adres pro připojení k síti ER P2P
- Rozsah adres fondu IP serveru

Přidání dalších virtuálních sítí, potřebujete tooconnect tooHANA velké instancí, vyžaduje, abyste toosubmit hello nové Azure adresní prostor sítě VNet přidáváte tooMicrosoft. 

Tady je příklad hello různých rozsahů a rozsahů některé příkladu potřebujete tooconfigure a nakonec zadejte tooMicrosoft. Jak vidíte, hello hodnotu hello adresní prostor sítě VNet Azure nejsou agregovány v prvním příkladu hello, není však definována z rozsahů hello hello první virtuální počítač Azure podsítě rozsah IP adres a rozsah adres IP adresu podsítě brány virtuální sítě hello. Použití více podsítí virtuálních počítačů v rámci hello virtuální síť Azure, by pracovní odpovídajícím způsobem nakonfigurováním a odesílání hello další IP rozsahů adres hello další podsítí virtuálních počítačů v rámci hello adresní prostor sítě VNet Azure.

![Rozsahy IP adres na minimální nasazení Azure (velké instance) vyžadují v SAP HANA](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Máte také možnost hello agregování dat hello odeslání tooMicrosoft. V takovém případě hello adresní prostor virtuální sítě Azure hello by obsahovat jenom jeden prostor. Použití hello rozsahů IP adres používaný v příkladu hello dříve. Tento agregované virtuální síť adresní prostor by měl vypadat jako:

![Druhá možnost rozsahů IP adres na minimální nasazení Azure (velké instance) vyžadují v SAP HANA](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Jak je uvedeno výše, místo dvou menší rozsahy, které definované hello adresním prostoru hello virtuální síť Azure, máme jeden větší rozsah, které pokrývá 4096 IP adresy. Takové velké definice hello adresní prostor zanechává některé místo velkých oblastí nepoužívá. Vzhledem k tomu, že hodnoty adresní prostor sítě VNet hello se používají pro šíření trasy protokolu BGP, využití hello nepoužívané rozsahy místní nebo jinde v síti může způsobit problémy s směrování. Proto je doporučené tookeep hello adresní prostor naprosto v souladu s adresního prostoru hello skutečné podsítě použít. V případě potřeby, aniž by docházelo k výpadku na hello virtuální síť, můžete vždy přidat nové hodnoty adresní prostor později.
 
> [!IMPORTANT] 
> Každou IP adresu adresní prostor sítě VNet Azure rozsah ER – P2P, IP fondu serverů, musí **není** navzájem překrývají nebo použít jinou oblast jinde v síti; každý musí být diskrétní, a jako hello dvou grafika nesmí být starší zobrazit, podsíť jinou oblast. Dojde-li překrytí mezi rozsahy, nemusí hello virtuální síť Azure připojit toohello okruh ExpressRoute.

### <a name="next-steps-after-address-ranges-have-been-defined"></a>Další kroky po nebyly definovány rozsahy adres
Po definování rozsahů IP adres hello hello níže uvedených aktivit potřebovat toohappen:

1. Odešlete hello rozsahy IP adres pro adresní prostor sítě VNet Azure, hello ER P2P připojení a rozsah adres fondu IP serveru, spolu s další data, která je uveden na začátku hello hello dokumentu. V tomto okamžiku můžete také začít toocreate hello virtuální sítě a podsítě virtuálních počítačů hello. 
2. Okruh Express Route se vytvoří Microsoft mezi předplatného Azure a hello HANA velké Instance razítka.
3. Síť klienta se vytvoří na hello velké Instance razítka společností Microsoft.
4. Microsoft nakonfiguruje sítě v hello SAP HANA na tooaccept infrastrukturu Azure (velké instance) IP adresy z Azure VNet adresního prostoru komunikující s instancemi velké HANA.
5. V závislosti na hello zakoupený konkrétní SAP HANA na Azure SKU (velké instance) Microsoft přiřadí výpočetní jednotka v síti klientů, přidělit a připojte úložiště a nainstalujte hello operační systém (SUSE nebo Red Hat Linux). IP adresy pro tyto jednotky se vyjímají z hello Server fond IP adres rozsah odeslána tooMicrosoft.

Na konci hello hello procesu nasazení poskytuje Microsoft hello následující tooyou dat:
- Informace potřebné tooconnect váš okruh ExpressRoute toohello Azure VNet(s) propojující sítě Azure Vnet tooHANA velké instancí:
     - Autorizace klíče
     - ExpressRoute PeerID
- Data tooaccess velké instancí HANA po můžete vytvořit okruh ExpressRoute a virtuální sítí VNet Azure.

Můžete také vyhledat hello pořadí propojení HANA velké instancí v dokumentu hello [ukončení tooEnd instalační program pro velké instance SAP HANA](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). V ukázkové nasazení v tomto dokumentu jsou uvedeny řadu hello následující kroky. 


## <a name="connecting-a-vnet-toohana-large-instance-expressroute"></a>Připojení VNet tooHANA velké Instance ExpressRoute

Jako definovaný všechny rozsahy IP adres hello a teď získali zpět z Microsoft hello data, můžete spustit připojování hello virtuální sítě, které jste vytvořili před tooHANA velké instancí. Jednou hello, které se vytvoří virtuální síť Azure, bránu ExpressRoute musí být vytvořeny na hello virtuální síť toolink hello virtuální síť toohello okruh ExpressRoute, který se připojuje toohello zákazníka klienta s časovým razítkem velké Instance hello.

> [!NOTE] 
> Tento krok může trvat až minut toocomplete too30, jako novou bránu hello je vytvořen v hello určené předplatného Azure a potom zadat toohello připojené virtuální sítě Azure.

Pokud brána už existuje, zkontrolujte, zda je bránu ExpressRoute nebo ne. Pokud ne, hello brány musí být odstraněn a znovu vytvořen jako bránu ExpressRoute. Pokud bránu ExpressRoute je již navázat, protože hello virtuální síť Azure je již připojen toohello okruh ExpressRoute pro připojení k místní síti, pokračujte toohello níže uvedené části propojování virtuálních sítí.

- Použijte buď hello (Nový) [portál Azure](https://portal.azure.com/), nebo prostředí PowerShell toocreate bránu ExpressRoute VPN připojený tooyour virtuální sítě.
  - Pokud používáte hello portálu Azure, přidejte nový **brány virtuální sítě** a pak vyberte **ExpressRoute** jako typ brány hello.
  - Pokud jste zvolili namísto toho prostředí PowerShell, nejdřív stáhnout a použít nejnovější hello [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) tooensure zajistit optimální práci. Následující příkazy Hello vytvořte bránu ExpressRoute. texty Hello před sebou  _$_  jsou proměnné definované uživatelem, že toobe potřeba aktualizovat pomocí vaší konkrétní informace.

```PowerShell
# These Values should already exist, update toomatch your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used toocreate hello gateway, update for how you wish hello GW components toobe named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

# These Commands create hello Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

V tomto příkladu se použila hello HighPerformance skladová položka brány. Vaše možnosti jsou HighPerformance nebo UltraPerformance jako hello jenom jednotky SKU brány, které jsou podporovány pro SAP HANA v Azure (velké instance).

> [!IMPORTANT]
> Pro velké instancí HANA hello SKU typy S384 S384m, S384xm, S576, S768 a S960 (typ třídy II SKU), hello využití hello skladová položka brány UltraPerformance je povinný.

### <a name="linking-vnets"></a>Propojení virtuální sítě

Nyní, když hello virtuální síť Azure bránu ExpressRoute, můžete použít informace o ověření hello poskytované Microsoft tooconnect hello ExpressRoute brány toohello SAP HANA v okruhu ExpressRoute Azure (velké instance) pro toto připojení vytvořit. Tento krok můžete provést pomocí hello portál Azure nebo Powershellu. doporučuje se Hello portálu, ale jsou pokyny pro PowerShell následující. 

- Můžete spustit následující příkazy pro každou bránu virtuální sítě pomocí různých AuthGUID pro každé připojení hello. první dvě položky Hello zobrazené v následující skript hello pocházet z hello informace od společnosti Microsoft. Navíc hello AuthGUID je specifický pro každý virtuální sítě a jeho brány. Znamená, že pokud chcete tooadd jinou virtuální síť Azure, je třeba tooget jiné AuthID pro váš okruh ExpressRoute, který se připojuje HANA velké instance do Azure. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define hello name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between hello ER Circuit and your Gateway using hello Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Pokud chcete tooconnect hello brány toomultiple okruhy ExpressRoute, které jsou spojeny s předplatným, musíte tooexecute tento krok více než jednou. Například pravděpodobně budete tooconnect hello stejnému okruhu ExpressRoute toohello brány virtuální sítě, která se připojuje hello tooyour virtuální sítě ve svojí místní síti.

## <a name="adding-more-ip-addresses-or-subnets"></a>Přidání další IP adresy nebo podsítě

Při přidávání více IP adresy nebo podsítě, použijte buď hello portál Azure, PowerShell nebo rozhraní příkazového řádku.

Hello doporučení v tomto případě je namísto generování nový rozsah agregované tooadd hello nového rozsahu IP adres jako nový rozsah toohello adresní prostor sítě VNet. V obou případech musíte toosubmit této změny tooMicrosoft tooallow připojení z této nové IP adresa rozsahu toohello velké Instance HANA jednotky v vašeho klienta. Můžete otevřít podporu Azure požadavek tooget hello novou virtuální síť adresní prostor přidat. Jakmile se zobrazí potvrzení, proveďte další kroky hello.

toocreate podsíť další z hello portál Azure, najdete v článku hello [vytvoření virtuální sítě pomocí portálu Azure hello](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), a toocreate z prostředí PowerShell, najdete v části [vytvoření virtuální sítě pomocí prostředí PowerShell](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="adding-vnets"></a>Přidávání virtuálních sítí

Po počátečním připojení jeden nebo více virtuálních sítí Azure, můžete chtít tooadd další ty, které přístup k SAP HANA v Azure (velké instance). Nejprve odeslání žádosti o podporu Azure, v této žádosti zahrnují hello konkrétní informace identifikující hello konkrétní nasazení Azure i hello IP adres místo rozsahy Dobrý den adresní prostor sítě VNet Azure. SAP HANA na Azure Service Management pak poskytuje hello potřebné informace, které potřebujete tooconnect hello další virtuální sítě a ExpressRoute. Pro každý virtuální síť musíte jedinečný toohello tooconnect autorizační klíč velké instancí tooHANA okruh ExpressRoute.

Kroky tooadd novou virtuální síť Azure:

1. Dokončení hello prvním krokem v procesu připojování hello najdete v tématu hello _vytváření virtuální sítě Azure_ části.
2. Dokončení hello druhý krok v procesu připojování hello najdete v tématu hello _vytváření podsítě brány_ části.
3. tooconnect další virtuální sítě toohello okruh ExpressRoute Instance velké HANA, otevřete žádost podporu Azure s informace na hello nové sítě VNet a požádat o nový klíč autorizace.
4. Po upozornění, že autorizace hello dokončení, použijte informace o ověření poskytovaný společností Microsoft hello toocomplete hello třetí krok v procesu připojování hello, najdete v části hello _propojování virtuálních sítí_ části.

## <a name="increasing-expressroute-circuit-bandwidth"></a>Zvýšení šířku pásma okruhu ExpressRoute

Poraďte se s SAP HANA na Správa služby Azure. Pokud jste doporučené tooincrease hello šířka pásma hello SAP HANA v okruhu ExpressRoute Azure (velké instance), vytvoření žádosti o podporu Azure. (Můžete požádat o zvýšení pro jeden okruh pásma až tooa maximálně 10 GB/s.) Poté se zobrazí oznámení po dokončení operace hello; žádná další akce potřeba tooenable Tato vyšší rychlost v Azure.

## <a name="adding-an-additional-expressroute-circuit"></a>Přidání další okruhu ExpressRoute

Poraďte se s SAP HANA na Azure Service Management, pokud je doporučeno, že je potřeba další okruh ExpressRoute, ujistěte se, toocreate žádosti o podporu Azure nové okruh ExpressRoute (a tooget autorizace informace tooconnect tooit). Hello adresní prostor, který se bude použít na text hello, virtuálních sítí musí být definován před provedením hello požadavek, aby SAP HANA na Azure Service Management tooprovide autorizace.

Jakmile se vytvoří nový okruh hello a hello SAP HANA na Azure Service Management configuration se dokončí, budete tooreceive oznámení s hello informace, které potřebujete tooproceed. Postupujte podle kroků hello výše uvedeného pro vytvoření a připojení hello nové virtuální sítě toothis další okruh. Nejste možné tooconnect sítě Azure Vnet toothis další okruh by již připojeni tooanother SAP HANA na okruh ExpressRoute Azure (velké Instance) v hello stejné oblasti Azure.

## <a name="deleting-a-subnet"></a>Odstraňování podsíť

je možné tooremove podsíť virtuální sítě buď hello portál Azure, PowerShell nebo rozhraní příkazového řádku. V případě, že vaše Azure VNet IP adresa rozsahu nebo Azure adresní prostor sítě VNet byla agregované rozsahu, neexistuje žádný postupujte podle službu jste se společností Microsoft. Kromě tohoto hello virtuální sítě stále šíření BGP trasy adresní prostor, který zahrnuje hello odstranit podsíť. Pokud jste definovali hello Azure VNet IP adresa rozsahu nebo Azure adresní prostor sítě VNet jako víc rozsahů IP adres které z nich byl přiřazené tooyour odstranit podsíť, měli byste odstranit, mimo váš adresní prostor virtuální sítě a následně informuje SAP HANA na Azure Service Management tooremove z hello rozsahy tohoto SAP HANA v Azure (velké instance) je povolen toocommunicate s.

Když není k dispozici ještě konkrétní, vyhrazené Azure.com pokyny k odebrání podsítě, hello proces odebrání podsítě je hello reverse hello procesu pro jejich přidáním. Najdete v článku hello [vytvoření virtuální sítě pomocí portálu Azure hello](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace o vytvoření podsítě.

## <a name="deleting-a-vnet"></a>Odstranění virtuální sítě

Při odstraňování virtuální síť, použijte buď hello portál Azure, PowerShell nebo rozhraní příkazového řádku. SAP HANA na Azure Service Management Odebere existující autorizací hello na hello SAP HANA na okruh Azure ExpressRoute (velké instance) a odeberte hello Azure VNet IP adresa rozsahu nebo Azure adresní prostor sítě VNet pro hello komunikaci s instancí velké HANA.

Po odebrání hello virtuální sítě, otevřete podporu Azure požadavek tooprovide hello adresního prostoru IP adres range(s) toobe odebrat.

Když není k dispozici ještě konkrétní, vyhrazené Azure.com pokyny k odebrání virtuálních sítí, hello proces odebrání virtuální sítě je hello reverse hello procesu pro přidání, který je popsaný výše. Najdete v článcích hello [vytvoření virtuální sítě pomocí portálu Azure hello](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a [vytvoření virtuální sítě pomocí prostředí PowerShell](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace o vytváření virtuální sítě.

tooensure všechno, co je odebrána, odstranit hello následující položky:

- Hello připojení ExpressRoute, brány virtuální sítě, veřejnou IP adresu brány virtuální sítě a virtuální sítí VNet

## <a name="deleting-an-expressroute-circuit"></a>Odstraňování okruhu ExpressRoute

tooremove další SAP HANA v okruhu ExpressRoute Azure (velké instance), otevřete žádost o podporu Azure s SAP HANA na Azure Service Management a požadavku, že hello okruh, měla by být odstraněna. V rámci hello předplatné Azure můžete odstranit nebo ponechat hello virtuální síť podle potřeby. Ale je nutné odstranit hello připojení mezi hello okruh ExpressRoute velké instancí HANA a hello propojit brány virtuální sítě.

Pokud také chcete tooremove virtuální sítě, postupujte podle pokynů hello na odstranění virtuální sítě ve výše uvedené části hello.


