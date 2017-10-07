---
title: "aaaNetwork skupiny zabezpečení v Azure | Microsoft Docs"
description: "Zjistěte, jak probíhá tooisolate a řízení provozu v rámci virtuálních sítí pomocí brány firewall hello distribuované v Azure pomocí skupin zabezpečení sítě."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 20e850fc-6456-4b5f-9a3f-a8379b052bc9
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
ms.openlocfilehash: 3528ce833dab17977327c3c9ae0e78316e5e6a05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="filter-network-traffic-with-network-security-groups"></a>Filtrování provozu sítě s použitím skupin zabezpečení sítě

Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, které povolí nebo zakáže přenášení tooresources provoz sítě připojené tooAzure virtuálních sítí (VNet). Skupiny Nsg můžou být přidružené toosubnets, jednotlivé virtuální počítače (klasické), nebo jednotlivých síťových rozhraní (NIC) připojených tooVMs (Resource Manager). Pokud skupinu NSG přidružená tooa podsíť, hello pravidla použít tooall prostředky připojené toohello podsítě. Provoz se dá dál omezit tím, že také přidružíte NSG tooa virtuálního počítače nebo síťový adaptér.

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek popisuje použití obou modelů, ale Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

## <a name="nsg-resource"></a>Prostředek NSG
Skupiny Nsg obsahují následující vlastnosti hello:

| Vlastnost | Popis | Omezení | Požadavky |
| --- | --- | --- | --- |
| Name (Název) |Název hello NSG |Musí být jedinečný v rámci oblasti hello.<br/>Může obsahovat písmena, číslice, podtržítka, tečky a pomlčky.<br/>Musí začínat písmenem nebo číslicí.<br/>Musí končit písmenem, číslicí nebo podtržítkem.<br/>Nesmí být delší než 80 znaků. |Vzhledem k tomu, že potřebujete může toocreate víc skupin Ngs, ujistěte se, že máte zásady vytváření názvů, který umožňuje snadno tooidentify hello funkce skupin Nsg. |
| Oblast |Azure [oblast](https://azure.microsoft.com/regions) kde hello NSG se má vytvořit. |Skupiny Nsg můžou být jenom přidružené tooresources v rámci hello stejné oblasti jako hello NSG. |toolearn o tom, kolik skupin Nsg můžete mít na oblast, přečtěte si hello [Azure omezuje](../azure-subscription-service-limits.md#virtual-networking-limits-classic) článku.|
| Skupina prostředků |Hello [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) hello NSG v existuje. |Ačkoli skupina NSG existuje ve skupině prostředků, může být přidružené tooresources v libovolné skupině prostředků, tak dlouho, dokud prostředek hello je součástí hello stejné oblasti Azure jako hello NSG. |Skupiny prostředků jsou použité toomanage více prostředky společně jako jednotky nasazení.<br/>Může být vhodné seskupit hello NSG s prostředky, které je přidružené k. |
| Pravidla |Pravidla pro příchozí a odchozí provoz, která definují, jaký provoz je povolený nebo odepřený. | |V tématu hello [pravidla NSG](#Nsg-rules) tohoto článku. |

> [!NOTE]
> Seznamy ACL založené na koncových a zabezpečení sítě, které skupiny nejsou podporovány na hello stejné instanci virtuálního počítače. Pokud chcete toouse skupinu NSG a již seznamu ACL koncového bodu je v místě, odeberte nejprve hello koncový bod seznamu ACL. jak tooremove ACL, číst toolearn hello [Správa řízení seznamy přístupu (ACL) pro koncové body pomocí prostředí PowerShell](virtual-networks-acl-powershell.md) článku.
> 

### <a name="nsg-rules"></a>Pravidla NSG
Pravidla NSG obsahují následující vlastnosti hello:

| Vlastnost | Popis | Omezení | Požadavky |
| --- | --- | --- | --- |
| **Název** |Název pravidla hello. |Musí být jedinečný v rámci oblasti hello.<br/>Může obsahovat písmena, číslice, podtržítka, tečky a pomlčky.<br/>Musí začínat písmenem nebo číslicí.<br/>Musí končit písmenem, číslicí nebo podtržítkem.<br/>Nesmí být delší než 80 znaků. |Může mít několik pravidel ve skupině NGS, proto se ujistěte, že podle zásady vytváření názvů, který vám umožní tooidentify hello funkci pravidla. |
| **Protokol** |Protokol toomatch pro pravidlo hello. |TCP, UDP nebo * |Pomocí * jako protokolu zahrnuje protokol ICMP (pouze provoz typu East-West), stejně jako i jako UDP a TCP a může snížit hello počet pravidel, které potřebujete.<br/>Na hello stejný čas, pomocí * může být příliš široké přístup, proto se doporučuje použít * pouze v případě potřeby. |
| **Rozsah zdrojových portů** |Zdroj toomatch rozsah portů pro pravidlo hello. |Jedno číslo portu od 1 too65535, rozsah portů (Příklad: 1 – 65 535), nebo * (pro všechny porty). |Zdrojové porty můžou být dočasné. Pokud váš klientský program nepoužívá konkrétní port, ve většině případů použijte hodnotu *.<br/>Zkuste rozsahy portů toouse tolik, kolik možné tooavoid hello potřebovat pro více pravidel.<br/>Není možné seskupit s použitím čárky více portů nebo rozsahů portů. |
| **Rozsah cílových portů** |Cílový port rozsah toomatch pro pravidlo hello. |Jedno číslo portu od 1 too65535, rozsah portů (Příklad: 1 – 65 535), nebo \* (pro všechny porty). |Zkuste rozsahy portů toouse tolik, kolik možné tooavoid hello potřebovat pro více pravidel.<br/>Není možné seskupit s použitím čárky více portů nebo rozsahů portů. |
| **Předpona zdrojové adresy** |Zdroj adresy předpona nebo značka, toomatch pro pravidlo hello. |Jedna IP adresa (např. 10.10.10.10), podsíť IP (např. 192.168.1.0/24), [výchozí značka](#default-tags) nebo * (pro všechny adresy). |Zvažte použití rozsahů, výchozích značek a * tooreduce hello počet pravidel. |
| **Předpona cílové adresy** |Cílové adresy předpona nebo značka, toomatch pro pravidlo hello. | Jedna IP adresa (např. 10.10.10.10), podsíť IP (např. 192.168.1.0/24), [výchozí značka](#default-tags) nebo * (pro všechny adresy). |Zvažte použití rozsahů, výchozích značek a * tooreduce hello počet pravidel. |
| **Směr** |Směr provozu toomatch pro pravidlo hello. |Příchozí nebo odchozí. |Pravidla pro příchozí a odchozí provoz se zpracovávají odděleně podle směru. |
| **Priorita** |Pravidla se kontrolují v hello pořadí podle priority. Jakmile se pravidlo aplikuje, u dalších pravidel se už shoda nekontroluje. | Číslo v rozsahu od 100 do 4096. | Zvažte, vytváření pravidel zadat priority v krocích po 100 pro každé pravidlo tooleave místo pro nové pravidel, že můžete vytvořit v hello budoucí. |
| **Přístup** |Typ tooapply přístup, pokud odpovídá hello pravidlo. | Povolení nebo odepření. | Uvědomte si, že pokud se pro paket nenajde pravidlo povolení, hello paket se zahodí. |

Skupiny zabezpečení sítě obsahují dvě sady pravidel: pro příchozí provoz a pro odchozí provoz. Hello Priorita pravidla musí být jedinečný v rámci jednotlivých sad. 

![Zpracování pravidel NSG](./media/virtual-network-nsg-overview/figure3.png) 

předchozí obrázek Hello ukazuje, jak se zpracovávají pravidla NSG.

### <a name="default-tags"></a>Výchozí značky
Výchozí značky jsou identifikátory poskytované systémem tooaddress kategorie IP adres. Výchozí značky můžete použít v hello **předpona zdrojové adresy** a **předpona cílové adresy** vlastnosti žádné pravidlo. Existují tři výchozí značky, které můžete použít:

* **Virtuální síť** (Resource Manager) (**VIRTUAL_NETWORK** pro classic): Tato značka zahrnuje hello adresního prostoru virtuální sítě (rozsahy CIDR definované v Azure), všechny připojené místní adresní prostory a připojené Virtuálních sítí Azure (místní sítě).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** v případě klasického modelu): Tato značka označuje nástroj pro vyrovnávání zatížení infrastruktury Azure. značka Hello překládá tooan pocházejí IP datové centrum Azure, kde testy stavu Azure.
* **Internet** (Resource Manager) (**INTERNET** pro classic): Tato značka označuje hello prostor IP adres, který je mimo virtuální síť hello a dostupný prostřednictvím veřejného Internetu. rozsah Hello zahrnuje hello [veřejný prostor IP adres vlastněný Azure](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="default-rules"></a>Výchozí pravidla
Všechny skupiny NSG obsahují sadu výchozích pravidel. nelze odstranit Hello výchozí pravidla, ale protože je jim přiřazená nejnižší priorita hello, dají se přepsat pravidly hello, které vytvoříte. 

Hello výchozí pravidla povolit a zakázat přenosy následujícím způsobem:
- **Virtuální síť:** Provoz směřující z virtuální sítě a do ní je povolený v příchozím i odchozím směru.
- **Internet:** Je povolen odchozí provoz, ale příchozí provoz je blokován.
- **Nástroj pro vyrovnávání zatížení:** povolit Azure zatížení vyrovnávání tooprobe hello stavu virtuálních počítačů a instancí rolí. Pokud nepoužíváte sadu s vyrovnáváním zatížení, můžete toto pravidlo přepsat.

**Příchozí výchozí pravidla**

| Name (Název) | Priorita | Zdrojová IP adresa | Zdrojový port | Cílová IP adresa | Cílový port | Protocol (Protokol) | Access |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AllowVNetInBound |65000 | VirtualNetwork | * | VirtualNetwork | * | * | Povolit |
| AllowAzureLoadBalancerInBound | 65001 | AzureLoadBalancer | * | * | * | * | Povolit |
| DenyAllInBound |65500 | * | * | * | * | * | Odepřít |

**Odchozí výchozí pravidla**

| Name (Název) | Priorita | Zdrojová IP adresa | Zdrojový port | Cílová IP adresa | Cílový port | Protocol (Protokol) | Access |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AllowVnetOutBound | 65000 | VirtualNetwork | * | VirtualNetwork | * | * | Povolit |
| AllowVnetOutBound | 65001 | * | * | Internet | * | * | Povolit |
| DenyAllOutBound | 65500 | * | * | * | * | * | Odepřít |

## <a name="associating-nsgs"></a>Přidružení skupin NSG
Můžete přidružit tooVMs NSG, síťové adaptéry a podsítě, v závislosti na modelu hello nasazení, který používáte, následujícím způsobem:

* **Virtuální počítač (pouze klasické):** pravidla zabezpečení se používají tooall provoz do/z hello virtuálních počítačů. 
* **Síťový adaptér (pouze Resource Manager):** pravidla zabezpečení je přidružen k tooall provoz do/z hello seskupování hello NSG. Ve virtuálním počítači více síťovými Kartami, můžete použít různé (nebo hello stejné) NSG tooeach seskupování jednotlivě. 
* **Podsíť (Resource Manager a klasický):** pravidla zabezpečení se používají tooany provoz do/z všechny prostředky připojené toohello virtuální sítě.

Můžete přidružit jiné skupiny Nsg tooa virtuálního počítače (nebo síťové KARTĚ, v závislosti na modelu nasazení hello) a hello podsítě, která síťovou kartu nebo virtuální počítač je připojený k. Pravidla zabezpečení jsou použité toohello přenosy podle priority v jednotlivých skupinách NSG v hello následující pořadí:

- **Příchozí provoz**

  1. **Skupina NSG použitá toosubnet:** Pokud podsíť NSG má odpovídající provoz toodeny pravidlo, hello paket se zahodí.

  2. **Skupina NSG použije tooNIC** (Resource Manager) nebo virtuální počítač (klasický): Pokud NSG VM\NIC má odpovídající pravidlo, které odepře provoz, dojde ke ztrátě paketů na hello VM\NIC, i když podsíť NSG má odpovídající pravidlo, které umožňuje přenos.

- **Odchozí provoz**

  1. **Skupina NSG použije tooNIC** (Resource Manager) nebo virtuální počítač (klasický): Pokud je skupina NSG VM\NIC má odpovídající pravidlo, které odepře provoz, dojde ke ztrátě paketů.

  2. **Skupina NSG použitá toosubnet:** Pokud podsíť NSG má odpovídající pravidlo, které odepře provoz, dojde ke ztrátě paketů, i když je skupina NSG VM\NIC má odpovídající pravidlo, které umožňuje přenos.

> [!NOTE]
> I když můžete přidružit jen jedné podsíti tooa, virtuální počítač nebo síťové KARTĚ; NSG můžete přidružit hello stejné tooas NSG mnoho prostředků tak, jak chcete.
>

## <a name="implementation"></a>Implementace
Skupiny Nsg můžete implementovat v hello Resource Manager nebo model nasazení classic pomocí hello následující nástroje:

| Nástroj pro nasazení | Classic | Resource Manager |
| --- | --- | --- |
| portál Azure   | Ano | [Ano](virtual-networks-create-nsg-arm-pportal.md) |
| PowerShell     | [Ano](virtual-networks-create-nsg-classic-ps.md) | [Ano](virtual-networks-create-nsg-arm-ps.md) |
| Rozhraní příkazového řádku Azure CLI **V1**   | [Ano](virtual-networks-create-nsg-classic-cli.md) | [Ano](virtual-networks-create-nsg-cli-nodejs.md) |
| Rozhraní příkazového řádku Azure CLI **V2**   | Ne | [Ano](virtual-networks-create-nsg-arm-cli.md) |
| Šablona Azure Resource Manageru   | Ne  | [Ano](virtual-networks-create-nsg-arm-template.md) |

## <a name="planning"></a>Plánování
Před implementací skupin Nsg, je třeba hello tooanswer následující otázky:

1. Jakých typů prostředků chcete toofilter tooor provoz z? Můžete připojovat prostředky, jako jsou síťové karty (Resource Manager), virtuální počítače (klasický model), cloudové služby, prostředí aplikačních služeb a škálovací sady virtuálních počítačů. 
2. Jsou hello zdroje, které má toofilter provoz do a z připojených toosubnets v existující virtuální sítě?

Další informace o plánování zabezpečení sítě v Azure, najdete v tématu hello [cloudové služby a zabezpečení sítě](../best-practices-network-security.md) článku. 

## <a name="design-considerations"></a>Na co dát pozor při navrhování
Jakmile znáte odpovědi hello otázky toohello v hello [plánování](#Planning) , projděte si následující části před definovat skupiny Nsg hello:

### <a name="limits"></a>Omezení
Existují omezení toohello počet skupin Nsg můžete mít v předplatném a počet pravidel na skupinu NSG. Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.

### <a name="vnet-and-subnet-design"></a>Navrhování virtuálních sítí a podsítí
Vzhledem k tomu, že skupiny Nsg můžou být použité toosubnets, můžete minimalizovat hello počet skupin Nsg svoje prostředky seskupíte podle podsítí a použitím toosubnets skupiny Nsg.  Pokud se rozhodnete tooapply skupiny Nsg toosubnets, můžete zjistit, že existující virtuální sítě a podsítě, které máte nebyly definovaná pomocí skupin Nsg v paměti. Může potřebovat toodefine nové virtuální sítě a podsítě toosupport váš návrh skupin NSG a nasaďte nové prostředky tooyour nových podsítí. Potom můžete třeba definovat toomove strategie migrace existující prostředky toohello nové podsítě. 

### <a name="special-rules"></a>Zvláštní pravidla
Pokud zablokujete provoz povolený hello následující pravidla, infrastrukturu nemůže komunikovat se základními službami Azure:

* **Virtuální IP adresa uzlu hostitele hello:** základní služby infrastruktury, například DHCP, DNS a sledování stavu poskytované prostřednictvím virtualizované hostitele hello IP adresu 168.63.129.16. Tato veřejná IP adresa patří tooMicrosoft a je hello jediná virtualizovaná IP adresa pro tento účel použít ve všech oblastech. Tato IP adresa mapuje toohello fyzickou IP adresu počítače serveru hello (uzlu hostitele) hostování hello virtuálních počítačů. Hello uzel hostitele funguje jako hello přenos DHCP, rekurzivní překladač služby DNS hello a zdroj testu hello hello zatížení sondu nástroje pro vyrovnávání stavu a test stavu počítače hello. Komunikace toothis IP adresa není útoku.
* **Licencování (Služba správy klíčů):** Image systému Windows spuštěné ve virtuálních počítačích musí být licencované. licencování tooensure žádost se odesílají toohello servery hostitele služby správy klíčů, které takové dotazy zpracovávají. Hello požadavku odchozí přes port 1688.

### <a name="icmp-traffic"></a>Provoz protokolu ICMP
Hello aktuální pravidla NSG povolují pouze protokoly *TCP* nebo *UDP*. Pro *ICMP* neexistuje žádná konkrétní značka. Provoz protokolu ICMP je však povoleno v rámci virtuální sítě pomocí hello AllowVNetInBound výchozí pravidla, která umožňuje tooand provoz z jakéhokoli portu a protokolu v rámci hello virtuální sítě.

### <a name="subnets"></a>Podsítě
* Vezměte v úvahu hello počet úrovní vaše úlohy vyžadují. Každá úroveň se dá izolovat pomocí podsítě, s podsítí toohello skupina NSG se použije. 
* Pokud potřebujete tooimplement podsíť pro bránu VPN, nebo okruh ExpressRoute, proveďte **není** použít podsíť toothat NSG. Pokud byste tak učinili, může dojít k selhání připojení mezi virtuálními sítěmi nebo mezi více místy. 
* Pokud potřebujete tooimplement virtuální síťové zařízení (hodnocení chyb zabezpečení), připojení hello hodnocení chyb zabezpečení tooits vlastní podsítě a vytvořte tooand trasy definované uživatelem (UDR) z hello hodnocení chyb zabezpečení. Můžete implementovat úrovni podsítě a NSG toofilter provoz do/z této podsíti. Další informace o udr, přečtěte si hello toolearn [trasy definované uživatelem](virtual-networks-udr-overview.md) článku.

### <a name="load-balancers"></a>Nástroje pro vyrovnávání zatížení
* Zvažte hello zatížení vyrovnávání a síťových adres pravidla pro překlad (NAT) pro každý nástroj pro vyrovnávání zatížení používá každého z vašich zatížení. Pravidla NAT jsou vázané tooa fond back-end, který obsahuje síťové adaptéry (Resource Manager) nebo instance rolí virtuálních počítačů nebo cloudových služeb (klasické). Zvažte vytvoření skupina NSG pro každý fond back-end umožňuje pouze provoz mapovaný prostřednictvím hello pravidel implementovaných v nástrojích pro vyrovnávání zatížení hello. Vytvořit skupinu NSG pro každý fond back-end zaručuje, že provoz přicházející fond back-end toohello přímo (nikoli prostřednictvím nástroje pro vyrovnávání zatížení hello), je také filtrovat.
* V nasazení classic vytvoříte koncové body, které mapují porty tooports nástroje pro vyrovnávání zatížení virtuálních počítačů nebo instancí rolí. Můžete taky vytvořit vlastní jednotlivý veřejně přístupný nástroj pro vyrovnávání zatížení prostřednictvím Resource Manageru. Hello cílovým portem příchozího provozu je skutečný port hello ve hello virtuálního počítače nebo instance role není hello port vystavené Vyrovnávání zatížení. Hello zdrojového portu a adresu pro připojení toohello hello, virtuální počítač je na port a adresa hello vzdálený počítač v hello Internetu, není hello port a adresa zpřístupněné nástrojem pro vyrovnávání zatížení hello.
* Když vytvoříte skupiny Nsg toofilter provozu procházejícího interním zátěže (ILB), jsou hello zdrojový port a rozsah adres použijí z hello pocházející počítače, není pro vyrovnávání zatížení hello. Rozsah cílových portů a adres Hello jsou hello cílového počítače, není pro vyrovnávání zatížení hello.

### <a name="other"></a>Ostatní
* Seznamy řízení přístupu na základě koncový bod (ACL) a skupiny Nsg nejsou podporovány na hello stejné instanci virtuálního počítače. Pokud chcete toouse skupinu NSG a již seznamu ACL koncového bodu je v místě, odeberte nejprve hello koncový bod seznamu ACL. Informace o tooremove na koncový bod seznamu ACL, najdete v části hello [spravovat seznamy ACL koncových bodů](virtual-networks-acl-powershell.md) článku.
* Ve službě Správce prostředků, můžete tooa NSG přidruženou síťovou kartu pro virtuální počítače s více síťových adaptérů správu tooenable (vzdálený přístup) na základě na síťový adaptér. Přidružení skupiny Nsg tooeach jedinečný síťový adaptér umožňuje oddělení typů přenosů na síťových adaptérů.
* Podobně jako toohello použití služby Vyrovnávání zatížení, když filtrujete provoz z jiných virtuálních sítí, je nutné použít hello zdrojový rozsah adres hello vzdáleného počítače, není hello brány připojující hello virtuální sítě.
* Mnoho služeb Azure nemůže být připojený tooVNets. Pokud prostředek Azure není tooa připojené virtuální sítě, nelze použít prostředek NSG provoz toofilter toohello.  Přečtěte si dokumentaci hello hello služeb pomocí toodetermine zda hello služba může být tooa připojené virtuální sítě.

## <a name="sample-deployment"></a>Ukázka nasazení
aplikace hello tooillustrate hello informací v tomto článku, vezměte v úvahu běžný scénář ukazuje následující obrázek hello dvouvrstvé aplikace:

![Skupiny NSG](./media/virtual-network-nsg-overview/figure1.png)

Jak je vidět v diagramu hello, hello *Web1* a *Web2* virtuální počítače jsou připojené toohello *front-endu* podsíť a hello *DB1* a *DB2* virtuální počítače jsou připojené toohello *back-end* podsítě.  Obě podsítě jsou součástí hello *TestVNet* virtuální sítě. Hello součásti aplikace každý spustit v rámci tooa virtuálního počítače Azure, které jsou připojené virtuální sítě. scénář Hello má hello následující požadavky:

1. Oddělení provozu mezi hello WEB a servery DB.
2. Pravidla přesměrovat provoz z hello webové servery tooall nástroje pro vyrovnávání zatížení na portu 80 Vyrovnávání zatížení.
3. Načtěte vyrovnávání NAT pravidla předat dál provoz přicházející do Vyrovnávání zatížení hello na 50001 tooport portu 3389 na hello WEB1 virtuálních počítačů.
4. Žádný přístup toohello front-end nebo back-end virtuálních počítačů z hello Internetu, s výjimkou požadavky 2 a 3.
5. Žádná odchozí přístup k Internetu z webového nebo DB serverů hello.
6. Tooport 3389 kterýkoli webový server je povolen přístup z podsítě front-endu hello.
7. Přístup z podsítě front-endu hello je povolen tooport 3389 žádné Databázového serveru.
8. Ve všech serverech DB tooport 1433 je povolen přístup z podsítě front-endu hello.
9. Oddělení provozu správy (port 3389) a provozu databáze (1433) na různých síťových kartách na serverech DB.

Požadavky 1 – 6 (s výjimkou požadavky 3 a 4) jsou všechny uzavřeného toosubnet mezery. Hello následující skupiny Nsg požadavkům hello předchozí, a současně minimalizujete její hello počet skupin Nsg vyžaduje:

### <a name="frontend"></a>FrontEnd
**Pravidla pro příchozí provoz**

| Pravidlo | Access | Priorita | Zdrojový rozsah adres | Zdrojový port | Cílový rozsah adres | Cílový port | Protocol (Protokol) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-HTTP-Internet | Povolit | 100 | Internet | * | * | 80 | TCP |
| Allow-Inbound-RDP-Internet | Povolit | 200 | Internet | * | * | 3389 | TCP |
| Deny-Inbound-All | Odepřít | 300 | Internet | * | * | * | TCP |

**Pravidla pro odchozí provoz**

| Pravidlo | Access | Priorita | Zdrojový rozsah adres | Zdrojový port | Cílový rozsah adres | Cílový port | Protocol (Protokol) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All |Odepřít |100 | * | * | Internet | * | * |

### <a name="backend"></a>BackEnd
**Pravidla pro příchozí provoz**

| Pravidlo | Access | Priorita | Zdrojový rozsah adres | Zdrojový port | Cílový rozsah adres | Cílový port | Protocol (Protokol) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All | Odepřít | 100 | Internet | * | * | * | * |

**Pravidla pro odchozí provoz**

| Pravidlo | Access | Priorita | Zdrojový rozsah adres | Zdrojový port | Cílový rozsah adres | Cílový port | Protocol (Protokol) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All | Odepřít | 100 | * | * | Internet | * | * |

Hello následující skupiny Nsg se vytvářejí a související tooNICs v hello následující virtuální počítače:

### <a name="web1"></a>WEB1
**Pravidla pro příchozí provoz**

| Pravidlo | Access | Priorita | Zdrojový rozsah adres | Zdrojový port | Cílový rozsah adres | Cílový port | Protocol (Protokol) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-RDP-Internet | Povolit | 100 | Internet | * | * | 3389 | TCP |
| Allow-Inbound-HTTP-Internet | Povolit | 200 | Internet | * | * | 80 | TCP |

> [!NOTE]
> Hello zdrojový rozsah adres pro předchozí pravidla hello je **Internet**, není hello virtuální IP adresy pro vyrovnávání zatížení hello. Hello zdrojový port je *, a nikoli 500001. Pravidla NAT u nástroje pro vyrovnávání zatížení nejsou hello stejné jako zabezpečení pravidla NSG. Pravidla NSG zabezpečení jsou vždy související toohello původního zdroje a konečného cíle provozu, **není** hello Vyrovnávání zatížení mezi dvěma hello. 
> 
> 

### <a name="web2"></a>WEB2
**Pravidla pro příchozí provoz**

| Pravidlo | Access | Priorita | Zdrojový rozsah adres | Zdrojový port | Cílový rozsah adres | Cílový port | Protocol (Protokol) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Inbound-RDP-Internet | Odepřít | 100 | Internet | * | * | 3389 | TCP |
| Allow-Inbound-HTTP-Internet | Povolit | 200 | Internet | * | * | 80 | TCP |

### <a name="db-servers-management-nic"></a>Servery DB (síťová karta pro správu)
**Pravidla pro příchozí provoz**

| Pravidlo | Access | Priorita | Zdrojový rozsah adres | Zdrojový port | Cílový rozsah adres | Cílový port | Protocol (Protokol) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-RDP-Front-end | Povolit | 100 | 192.168.1.0/24 | * | * | 3389 | TCP |

### <a name="db-servers-database-traffic-nic"></a>Servery DB (síťová karta pro provoz databáze)
**Pravidla pro příchozí provoz**

| Pravidlo | Access | Priorita | Zdrojový rozsah adres | Zdrojový port | Cílový rozsah adres | Cílový port | Protocol (Protokol) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-SQL-Front-end | Povolit | 100 | 192.168.1.0/24 | * | * | 1433 | TCP |

Vzhledem k tomu, že některé skupiny Nsg hello jsou přidružené tooindividual síťové adaptéry, jsou hello pravidla pro prostředky nasazené prostřednictvím Resource Manager. Pravidla pro podsíť a síťovou kartu se kombinují podle toho, jak jsou přidružena. 

## <a name="next-steps"></a>Další kroky
* [Nasazení skupin zabezpečení sítě (Resource Manager)](virtual-networks-create-nsg-arm-pportal.md).
* [Nasazení skupin zabezpečení sítě (klasický model)](virtual-networks-create-nsg-classic-ps.md).
* [Správa protokolů NSG](virtual-network-nsg-manage-log.md).
* [Řešení potíží týkajících se skupin zabezpečení sítě] (virtual-network-nsg-troubleshoot-portal.md)
