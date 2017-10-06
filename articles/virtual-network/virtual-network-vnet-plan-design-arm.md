---
title: "aaaAzure virtuální síť (VNet) plánování a návrhu průvodce | Microsoft Docs"
description: "Zjistěte, jak v Azure tooplan a návrh virtuální sítě podle potřeb izolace, připojení a umístění."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 3a4a9aea-7608-4d2e-bb3c-40de2e537200
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/08/2016
ms.author: jdial
ms.openlocfilehash: f3ffadf8cf254f64b1f86b44f90315d2bc679f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="plan-and-design-azure-virtual-networks"></a>Naplánujte a navrhněte Azure Virtual Network
Vytvoření virtuální sítě tooexperiment s je dostatečně snadné, ale pravděpodobné, budete nasazovat více virtuálních sítí přes čas toosupport hello provozním potřebám vaší organizace. S některými plánování a návrhu bude se mít toodeploy virtuální sítě a připojte hello zdrojů, které potřebujete efektivněji. Pokud nejste obeznámeni s virtuální sítě, se doporučuje, můžete [Další informace o virtuálních sítí](virtual-networks-overview.md) a [jak toodeploy](virtual-networks-create-vnet-arm-pportal.md) jeden než budete pokračovat.

## <a name="plan"></a>Plánování
Je nutné pro úspěch důkladné znalosti týkající se předplatná Azure, oblasti a síťovým prostředkům. Hello seznam důležitých informací níže můžete použít jako počáteční bod. Jakmile porozumíte tyto aspekty, můžete definovat hello požadavky pro návrh vaší sítě.

### <a name="considerations"></a>Požadavky
Před přijetím hello plánování níže uvedené otázky, zvažte následující hello:

* Všechny objekty, které vytvoříte v Azure se skládá z jedné nebo více prostředků. Virtuální počítač (VM) je prostředek, hello síťového adaptéru rozhraní (NIC) používá virtuální počítač je prostředek, hello veřejnou IP adresu použitou adaptérem je prostředek, hello virtuální síť hello síťový adaptér je připojený toois prostředku.
* Vytvořit prostředky v rámci [oblast Azure](https://azure.microsoft.com/regions/#services) a předplatné. Prostředky lze pouze připojené tooa virtuální síť, která existuje v hello stejné oblasti a předplatné hello prostředek je ve.
* Virtuální sítě tooeach jiné můžete připojit pomocí:
    * **[Partnerský vztah virtuální sítě](virtual-network-peering-overview.md)**: hello virtuálních sítí musí existovat v hello stejné oblasti Azure. Šířku pásma mezi prostředky v peered virtuální sítě je hello stejné jako v případě, že hello prostředky byly připojené toohello stejné virtuální síti.
    * **Azure [brány VPN](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)**: hello virtuální sítě může existovat v hello stejné, nebo v různých oblastech Azure. Šířku pásma mezi prostředky ve virtuálních sítích, které se připojují prostřednictvím brány VPN je omezena šířka pásma hello hello brány VPN.
* Virtuální sítě tooyour do místní sítě můžete připojit pomocí jedné z hello [možnosti připojení](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) v Azure k dispozici.
* Různých prostředků můžete seskupit do [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups), takže je jednodušší toomanage hello prostředků jako jednotku. Skupina prostředků může obsahovat prostředky z několika oblastí, tak dlouho, dokud hello zdroje patřit toohello stejného předplatného.

### <a name="define-requirements"></a>Definujte požadavky
Hello otázky níže použijte jako výchozí bod pro návrh vaší sítě Azure.    

1. Co Azure umístění budete používat toohost virtuální sítě?
2. Potřebujete tooprovide komunikace mezi těmito Azure místy?
3. Potřebujete tooprovide komunikace mezi vaší VNet(s) Azure a vašich datových centrech místně?
4. Kolik infrastruktury jako služby (IaaS) VMs, cloudových služeb rolí a proveďte webové aplikace, které potřebujete pro vaše řešení?
5. Potřebujete, aby provoz tooisolate podle skupiny virtuálních počítačů (tj. front-endu webové servery a servery databáze back-end)?
6. Potřebujete toocontrol tok přenosů dat pomocí virtuální zařízení?
7. Uživatelé třeba různé sady oprávnění toodifferent dělat prostředků Azure?

### <a name="understand-vnet-and-subnet-properties"></a>Pochopení virtuálních sítí a podsítí vlastnosti
Virtuální sítě a podsítě prostředky pomohl definovat hranici zabezpečení pro úlohy běžící v Azure. Virtuální síť je charakterizovaná kolekce adresní prostory, které jsou definované jako bloků CIDR.

> [!NOTE]
> Správci sítě se seznámíte s notaci CIDR. Pokud nejste obeznámeni s CIDR, [Další informace o](http://whatismyipaddress.com/cidr).
>
>

Virtuální sítě obsahovat hello následující vlastnosti.

| Vlastnost | Popis | Omezení |
| --- | --- | --- |
| **Jméno** |Název virtuální sítě |Řetězec se too80 znaků. Může obsahovat písmena, číslice, podtržítka, tečky a pomlčky. Musí začínat písmenem nebo číslicí. Musí končit písmenem, číslicí nebo podtržítkem. Můžete obsahuje velká nebo malá písmena. |
| **location** |Umístění Azure (také odkazované tooas oblasti). |Musí mít, jednu hello platná umístění Azure. |
| **adresní prostor** |Kolekce předpon adres, které tvoří hello virtuální sítě v zápisu CIDR. |Musí být pole platné bloky adres CIDR, včetně rozsahů veřejných IP adres. |
| **podsítě** |Kolekce podsítě, které tvoří hello virtuální sítě |viz následující tabulka vlastnosti podsítě hello. |
| **dhcpOptions** |Objekt, který obsahuje jeden požadovaná vlastnost s názvem **dnsServers**. | |
| **dnsServers** |Pole serverů DNS používaných hello virtuální sítě. Pokud není zadaný žádný server, se používá Azure interní překlad adres. |Musí být pole nahoru too10 servery DNS, podle IP adresy. |

Podsíť je prostředkem podřízené virtuální sítě, a pomáhá definovat segmenty adresní prostory v rámci blok CIDR pomocí předpony IP adres. Síťové adaptéry lze přidat toosubnets a připojené tooVMs, poskytuje připojení pro různé úlohy.

Podsítě obsahovat hello následující vlastnosti.

| Vlastnost | Popis | Omezení |
| --- | --- | --- |
| **Jméno** |Název podsítě |Řetězec se too80 znaků. Může obsahovat písmena, číslice, podtržítka, tečky a pomlčky. Musí začínat písmenem nebo číslicí. Musí končit písmenem, číslicí nebo podtržítkem. Můžete obsahuje velká nebo malá písmena. |
| **location** |Umístění Azure (také odkazované tooas oblasti). |Musí mít, jednu hello platná umístění Azure. |
| **addressPrefix** |Jedna adresa předponu, která tvoří hello podsíť v notaci CIDR |Musí být jeden blok CIDR, která je součástí jednoho ze hello VNet adresních prostorů. |
| **skupinu zabezpečení sítě** |Skupina NSG použitá toohello podsítě | |
| **routeTable** |Směrovací tabulka použita toohello podsítě | |
| **Konfigurace IP adresy** |Kolekce objektů konfigurace IP používané síťové adaptéry připojené toohello podsítě | |

### <a name="name-resolution"></a>Překlad adres
Ve výchozím nastavení, virtuální sítě používá [Azure překlad](virtual-networks-name-resolution-for-vms-and-role-instances.md) tooresolve názvy uvnitř hello virtuální sítě a na hello veřejného Internetu. Ale pokud připojíte vaší virtuální sítě tooyour místní datových center, musíte tooprovide [serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md) tooresolve názvy mezi vaší sítí.  

### <a name="limits"></a>Omezení
Zkontrolujte síťové omezení hello v hello [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) tooensure článek, který váš návrh není v konfliktu s žádným z hello omezení. Některá omezení je možné zvýšit otevřením lístku podpory.

### <a name="role-based-access-control-rbac"></a>Řízení přístupu na základě role (RBAC)
Můžete použít [Azure RBAC](../active-directory/role-based-access-built-in-roles.md) toocontrol hello úroveň přístupu k různým uživatelům mít toodifferent prostředky v Azure. Tímto způsobem můžete oddělit hello práci váš tým podle svých potřeb.

Pokud jde o virtuální sítě jsou problémem, uživatelé v hello **Přispěvatel sítě** role mít plnou kontrolu nad prostředky virtuální sítě Azure Resource Manager. Podobně, uživatelé v hello **Classic Přispěvatel sítě** role mít plnou kontrolu nad klasické virtuální síťové prostředky.

> [!NOTE]
> Můžete také [vytvářet vlastní role](../active-directory/role-based-access-control-configure.md) tooseparate vaše požadavky na správu.
>
>

## <a name="design"></a>Návrh
Jakmile znáte odpovědi hello otázky toohello v hello [plánování](#Plan) , projděte si následující hello před definováním vaší virtuální sítě.

### <a name="number-of-subscriptions-and-vnets"></a>Počet odběrů a virtuální sítě
Měli byste zvážit vytvoření více virtuálních sítí v hello následující scénáře:

* **Virtuální počítače, které je třeba toobe umístěny v různých umístěních Azure**. Virtuální sítě v Azure jsou místní. Jejich nemůžou zahrnovat umístění. Proto musíte mít alespoň jeden virtuální sítě pro každý Azure umístění, do kterého chcete virtuální počítače toohost v.
* **Úlohy, které je třeba toobe zcela izolované od sebe navzájem**. Můžete vytvořit samostatné virtuální sítě, hello použití této i stejné adresními prostory IP adres, tooisolate různých zátěží od sebe navzájem.

Mějte na paměti, které jsou hello limity, které se zobrazí nad na oblast na předplatné. To znamená, že používáte více předplatných tooincrease hello omezení prostředků, které je možné uchovávat v Azure. Můžete použít site-to-site VPN nebo okruhem ExpressRoute tooconnect virtuálních sítí patřících do různých předplatných.

### <a name="subscription-and-vnet-design-patterns"></a>Předplatné a vzory návrhu virtuální sítě
Následující tabulka Hello uvádí některé běžné vzory návrhu pro použití odběry a virtuální sítě.

| Scénář | Diagram | Odborníci na | Nevýhody |
| --- | --- | --- | --- |
| Jednomu předplatnému, dvě virtuální sítě na aplikace |![Jednoho předplatného](./media/virtual-network-vnet-plan-design-arm/figure1.png) |Toomanage jenom jedno předplatné. |Maximální počet virtuálních sítí na oblast Azure. Potom musíte více odběrů. Zkontrolujte hello [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku. |
| S jedním odběrem na aplikaci, dvě virtuální sítě na aplikace |![Jednoho předplatného](./media/virtual-network-vnet-plan-design-arm/figure2.png) |Využívá jenom dva virtuální sítě na jedno předplatné. |Těžší toomanage, pokud jsou moc velký počet aplikací. |
| S jedním odběrem na organizační jednotku, dvě virtuální sítě, jednu aplikaci. |![Jednoho předplatného](./media/virtual-network-vnet-plan-design-arm/figure3.png) |Vyrovnávat mezi počet odběrů a virtuální sítě. |Maximální počet virtuálních sítí na organizační jednotku (předplatné). Zkontrolujte hello [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku. |
| S jedním odběrem na organizační jednotku, dvě virtuální sítě na skupinu aplikací. |![Jednoho předplatného](./media/virtual-network-vnet-plan-design-arm/figure4.png) |Vyrovnávat mezi počet odběrů a virtuální sítě. |Aplikace musí být izolovat pomocí podsítě a skupin Nsg. |

### <a name="number-of-subnets"></a>Počet podsítí
Je třeba zvážit několik podsítí ve virtuální síti v hello následující scénáře:

* **Není dostatek privátních IP adres pro všechny síťové adaptéry v podsíti**. Pokud váš adresní prostor podsítě neobsahuje dost IP adres pro hello počet síťových adaptérů v podsíti hello, musíte toocreate několik podsítí. Mějte na paměti, který Azure si vyhrazuje 5 soukromé IP adresy z každé podsítě, která se nedá použít: hello první a poslední adresy hello adresního prostoru (adresa podsítě hello a vícesměrové vysílání) a 3 adresy toobe používá interně (pro účely DHCP a DNS).
* **Zabezpečení.** Můžete použít skupiny tooseparate podsítě virtuálních počítačů od sebe navzájem pro úlohy, které mají více vrstvu struktury a použít jiný [skupin zabezpečení (Nsg) sítě](virtual-networks-nsg.md#subnets) pro tyto podsítě.
* **Hybridní připojení**. Okruhy ExpressRoute a bran VPN můžete použít příliš[připojit](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) vaší virtuální sítě tooone jiný, a tooyour místní data center(s). Okruhy ExpressRoute a bran VPN vyžadují podsíť vlastní toobe vytvořili.
* **Virtuální zařízení**. Virtuální zařízení, jako jsou brány firewall, akcelerátorů WAN nebo brány sítě VPN můžete použít ve službě Azure VNet. Pokud tak učiníte, musíte příliš[směrování provozu](virtual-networks-udr-overview.md) toothose zařízení a jejich zjištění ve vlastní podsíti.

### <a name="subnet-and-nsg-design-patterns"></a>Podsíť a NSG vzory návrhu
Následující tabulka Hello uvádí některé běžné vzory návrhu pro používání podsítě.

| Scénář | Diagram | Odborníci na | Nevýhody |
| --- | --- | --- | --- |
| Jedna podsíť, počet skupin Nsg na aplikační vrstvu, jednotlivé aplikace |![Jedna podsíť](./media/virtual-network-vnet-plan-design-arm/figure5.png) |Toomanage jenom jednu podsíť. |Více skupin Nsg nezbytné tooisolate každou aplikaci. |
| Jednu podsíť jednu aplikaci, počet skupin Nsg na aplikační vrstvu |![Podsíť jednotlivé aplikace](./media/virtual-network-vnet-plan-design-arm/figure6.png) |Méně toomanage skupiny Nsg. |Více toomanage podsítě. |
| Jednu podsíť pro aplikační vrstvu, počet skupin Nsg na aplikaci. |![Podsíť jednu vrstvu](./media/virtual-network-vnet-plan-design-arm/figure7.png) |Rovnováha mezi Nsg a počet podsítí. |Maximální počet skupin Nsg na jedno předplatné. Zkontrolujte hello [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku. |
| Jednu podsíť pro aplikační vrstvu, jednu aplikaci, počet skupin Nsg na podsítě |![Podsíť jednu vrstvu jednotlivé aplikace](./media/virtual-network-vnet-plan-design-arm/figure8.png) |Případně zmenšete počet skupin Nsg. |Více toomanage podsítě. |

## <a name="sample-design"></a>Ukázka návrhu
aplikace hello tooillustrate hello informací v tomto článku, zvažte následující scénář hello.

Pracujete pro společnost, která má 2 datových centrech v Severní Americe a dvě datových centrech Evropa. Jste identifikovali 6 různých zákazníků, kterým čelí aplikace udržované 2 jiné organizační jednotky, které chcete toomigrate tooAzure jako pilotní nasazení. Hello základní architekturu pro aplikace hello jsou následující:

* App1, počítači App2, App3 a App4 jsou webové aplikace hostované na serverech Linux s Ubuntu. Každou aplikaci připojí tooa samostatné aplikační server, který je hostitelem služby RESTful na servery se systémem Linux. služeb RESTful Hello připojit databáze MySQL tooa back-end.
* App5 a App6 jsou webové aplikace hostované na serverech Windows se systémem Windows Server 2012 R2. Každá aplikace připojí tooa databáze systému SQL Server back-end.
* Všechny aplikace jsou teď hostované v jedné z hello firemních datových centrech v Severní Americe.
* Hello místní datových centrech použít hello 10.0.0.0/8 adresní prostor.

Je třeba toodesign řešení virtuální sítě, které splňuje hello následující požadavky:

* Jednotlivé obchodní jednotky by neměla mít vliv spotřeby prostředků z jiných organizačních jednotek.
* Měli byste minimalizovat hello množství virtuální sítě a podsítě správu toomake jednodušší.
* Jednotlivé obchodní jednotky by měl mít jeden testovacím/vývojovém virtuální sítě používá pro všechny aplikace.
* Každá aplikace je hostitelem 2 různých datových center Azure na kontinentě (Severní Americe a Evropě).
* Každá aplikace je naprosto izolované od sebe navzájem.
* Každá aplikace můžete získat přístup k zákazníkům prostřednictvím hello Internet pomocí protokolu HTTP.
* Každá aplikace přístupná uživatelů připojených toohello místní datových centrech pomocí tunelového propojení šifrované.
* Připojení místní tooon datových centrech měli používat existující zařízení VPN.
* Hello společnosti sítě skupina by měla mít plnou kontrolu nad hello konfiguraci virtuální sítě.
* Vývojáři v jednotlivé obchodní jednotky by měly být jenom podsítě tooexisting možné toodeploy virtuálních počítačů.
* Všechny aplikace bude migrovat, protože se jedná o tooAzure (navýšení a shift).
* Hello databází v každé lokalitě musí replikovat tooother Azure umístění jednou denně.
* Každá aplikace by měl použít 5 front-endu webové servery, 2 aplikační servery (v případě potřeby) a 2 databázovým serverům.

### <a name="plan"></a>Plánování
Měli byste začít návrhu, plánování zodpovězením hello dotaz hello [definovat požadavky](#Define-requirements) části, jak je uvedeno níže.

1. Co Azure umístění budete používat toohost virtuální sítě?

    2 umístění v Severní Americe a 2 umístění v Evropě. Měli byste vybrat algoritmů založených na fyzické umístění existující místní datacentrech hello. Tímto způsobem připojení z vaší tooAzure fyzických umístění bude mít lepší latence.
2. Potřebujete tooprovide komunikace mezi těmito Azure místy?

    Ano. Vzhledem k tomu, že replikované tooall umístění musí být databáze hello.
3. Potřebujete tooprovide komunikace mezi vaší VNet(s) Azure a vaše místní data center(s)?

    Ano. Vzhledem k tomu, že uživatelé připojeni toohello místní datových centrech musí být schopný tooaccess hello aplikací prostřednictvím tunelového propojení šifrované.
4. Tom, kolik virtuálních počítačů IaaS potřebujete pro vaše řešení?

    200 virtuálních počítačů IaaS. App1, počítači App2, App3 a App4 vyžadují 5 webové servery každé, 2 aplikace každý server a 2 databázovým serverům. Je celkem 9 virtuálních počítačů IaaS na aplikaci nebo 36 virtuální počítače IaaS. App5 a App6 vyžadují 5 webové servery a 2 databázovým serverům. Je celkem 7 virtuální počítače IaaS na aplikaci nebo 14 virtuální počítače IaaS. Proto musíte 50 virtuální počítače IaaS pro všechny aplikace v každé oblasti Azure. Vzhledem k tomu, že potřebujeme toouse 4 oblasti, budou existovat 200 virtuálních počítačů IaaS.

    Budete také potřebovat tooprovide servery DNS v každé virtuální síť, nebo název místní data centrech tooresolve mezi virtuální počítače Azure IaaS a v místní síti.
5. Potřebujete, aby provoz tooisolate podle skupiny virtuálních počítačů (tj. front-endu webové servery a servery databáze back-end)?

    Ano. Každá aplikace musí být vzájemně zcela izolované a každý aplikační vrstvu navíc by měl mít izolované.
6. Potřebujete toocontrol tok přenosů dat pomocí virtuální zařízení?

    Ne. Virtuální zařízení můžou být použité tooprovide větší kontrolu nad tok přenosů, včetně podrobnější protokolování roviny data.
7. Uživatelé třeba různé sady oprávnění toodifferent dělat prostředků Azure?

    Ano. Hello sítě tým musí úplné řízení na nastavení virtuální sítě hello, když vývojáři by měly být jenom možnost toodeploy toopre existující podsítě virtuálních počítačů.

### <a name="design"></a>Návrh
Postupujte podle návrhu hello odběry, virtuální sítě, podsítě a skupin Nsg. Zde probereme skupin Nsg, ale měli další informace o [skupiny Nsg](virtual-networks-nsg.md) před dokončením návrhu.

**Počet odběrů a virtuální sítě**

Hello následující požadavky jsou související toosubscriptions a virtuální sítě:

* Jednotlivé obchodní jednotky by neměla mít vliv spotřeby prostředků z jiných organizačních jednotek.
* Měli byste minimalizovat objem hello virtuální sítě a podsítě.
* Jednotlivé obchodní jednotky by měl mít jeden testovacím/vývojovém virtuální sítě používá pro všechny aplikace.
* Každá aplikace je hostitelem 2 různých datových center Azure na kontinentě (Severní Americe a Evropě).

Na základě těchto požadavků, potřebujete předplatné pro jednotlivé obchodní jednotky. Tímto způsobem, spotřebu prostředků z organizační jednotky nebude započítávat limity pro jiné organizační jednotky. A vzhledem k tomu, že chcete toominimize hello počet virtuálních sítí, měli byste zvážit použití hello **s jedním odběrem na organizační jednotku, dvě virtuální sítě na skupinu aplikací** vzor, jak vidíte níže.

![Jednoho předplatného](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Budete také potřebovat toospecify hello adresní prostor pro každý virtuální síť. Vzhledem k tomu, že budete potřebovat připojení mezi hello místní datových centrech hello oblastí Azure, použít pro virtuální sítě Azure hello adresního prostoru nelze kolidovat s hello do místní sítě a prostor adres hello používá každý virtuální sítě nesmí kolidovat s jiné existující virtuální sítě. Hello adresní prostory v tabulce hello toosatisfy jste mohli používat tyto požadavky.  

| **Předplatné** | **Virtuální síť** | **Oblast Azure** | **Adresní prostor** |
| --- | --- | --- | --- |
| BU1 |ProdBU1US1 |Západní USA |172.16.0.0/16 |
| BU1 |ProdBU1US2 |Východ USA |172.17.0.0/16 |
| BU1 |ProdBU1EU1 |Severní Evropa |172.18.0.0/16 |
| BU1 |ProdBU1EU2 |Západní Evropa |172.19.0.0/16 |
| BU1 |TestDevBU1 |Západní USA |172.20.0.0/16 |
| BU2 |TestDevBU2 |Západní USA |172.21.0.0/16 |
| BU2 |ProdBU2US1 |Západní USA |172.22.0.0/16 |
| BU2 |ProdBU2US2 |Východ USA |172.23.0.0/16 |
| BU2 |ProdBU2EU1 |Severní Evropa |172.24.0.0/16 |
| BU2 |ProdBU2EU2 |Západní Evropa |172.25.0.0/16 |

**Počet skupin Nsg a podsítí**

Hello následující požadavky jsou skupiny Nsg a související toosubnets:

* Měli byste minimalizovat objem hello virtuální sítě a podsítě.
* Každá aplikace je naprosto izolované od sebe navzájem.
* Každá aplikace můžete získat přístup k zákazníkům prostřednictvím hello Internet pomocí protokolu HTTP.
* Každá aplikace přístupná uživatelů připojených toohello místní datových centrech pomocí tunelového propojení šifrované.
* Připojení místní tooon datových centrech měli používat existující zařízení VPN.
* Hello databází v každé lokalitě musí replikovat tooother Azure umístění jednou denně.

Na základě těchto požadavků, může použít jednu podsíť pro aplikační vrstvu a použít skupiny Nsg toofilter provoz na aplikaci. Tímto způsobem můžete mít pouze 3 podsítě v každé virtuální sítě (front-endu, aplikační vrstvu a datová vrstva) a jedna skupina NSG na aplikaci na podsíť. V takovém případě měli byste zvážit použití hello **jednu podsíť pro aplikační vrstvu, počet skupin Nsg na aplikaci** vzoru návrhu. Hello obrázek níže ukazuje použití hello tohoto vzoru návrhu hello představující hello **ProdBU1US1** virtuální sítě.

![Jednu podsíť jednu vrstvu, jedna skupina NSG na aplikaci na vrstvu](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Ale musíte taky toocreate další podsítě pro připojení k síti VPN hello mezi hello virtuálních sítí a vaše místní datových centrech. A je třeba toospecify hello adresní prostor pro každou podsíť. Hello obrázek níže ukazuje ukázkové řešení pro **ProdBU1US1** virtuální sítě. Tento scénář by replikovat pro každý virtuální síť. Každá barva představuje jinou aplikaci.

![Ukázka virtuální sítě](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Řízení přístupu**

Hello tyto požadavky jsou související tooaccess ovládacího prvku:

* Hello společnosti sítě skupina by měla mít plnou kontrolu nad hello konfiguraci virtuální sítě.
* Vývojáři v jednotlivé obchodní jednotky by měly být jenom podsítě tooexisting možné toodeploy virtuálních počítačů.

Na základě těchto požadavků, může přidat uživatele z hello sítě team toohello předdefinované **Přispěvatel sítě** role v každém předplatném; a vytvářet vlastní role pro hello vývojáři aplikace v každé předplatné poskytnutí je práva tooadd virtuální počítače tooexisting podsítě.

## <a name="next-steps"></a>Další kroky
* [Nasadit virtuální síť](virtual-networks-create-vnet-arm-template-click.md) závislosti na scénáři.
* Pochopit, jak příliš[vyrovnávat zatížení](../load-balancer/load-balancer-overview.md) virtuální počítače IaaS a [spravovat směrování nad několika oblastmi Azure](../traffic-manager/traffic-manager-overview.md).
* Další informace o [skupiny Nsg a jak tooplan a návrh](virtual-networks-nsg.md) řešení s NSG.
* Další informace o vaší [mezi různými místy a možnosti připojení virtuální sítě](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti).
