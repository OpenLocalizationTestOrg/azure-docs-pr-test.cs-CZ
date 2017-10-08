---
title: "typy adres aaaIP v Azure (klasický) | Microsoft Docs"
description: "Další informace o veřejné a privátní IP adresy (klasické) v Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 2f8664ab-2daf-43fa-bbeb-be9773efc978
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
ms.openlocfilehash: 7e09a5ad5b5f2d55329152b5d843de3c4455d1b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ip-address-types-and-allocation-methods-classic-in-azure"></a>IP adresa typy a metody přidělování (klasické) v Azure
Můžete přiřadit IP adresy tooAzure prostředky toocommunicate s další prostředky Azure, v místní síti a Internetu hello. Existují dva typy IP adres můžete použít v Azure: veřejné a soukromé.

Veřejné IP adresy se používají ke komunikaci s hello Internetu, včetně Azure veřejně přístupných služeb.

Privátní IP adresy se používají ke komunikaci v rámci virtuální sítě Azure (VNet), cloudové služby a v místní síti, pokud používáte brány VPN nebo tooextend okruh ExpressRoute tooAzure vaší sítě.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).  Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala Resource Manager. Další informace o IP adresy ve službě Správce prostředků podle čtení hello [IP adresy](virtual-network-ip-addresses-overview-arm.md) článku.

## <a name="public-ip-addresses"></a>Veřejné IP adresy
Veřejné IP adresy umožňují prostředkům Azure toocommunicate s veřejnými službami Internet a Azure, jako [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [databází SQL](../sql-database/sql-database-technical-overview.md), a [úložiště Azure](../storage/common/storage-introduction.md).

Veřejná IP adresa je spojena s hello následující typy prostředků:

* Cloud Services
* Virtuální počítače IaaS (VM)
* Instance rolí PaaS
* VPN Gateway
* Application Gateway

### <a name="allocation-method"></a>Metoda přidělování
Veřejná IP adresa musí toobe přiřazené tooan prostředků Azure, je *dynamicky* přidělit z fondu k dispozici veřejnou IP adresu v rámci prostředků hello umístění hello vytvořen. Tato IP adresa je vydala při zastavení hello prostředků. V případě, že cloudové služby, se to stane, když jsou zastaveny všechny instance role hello, který se vyhnout pomocí *statické* (vyhrazené) IP adresu (najdete v části [cloudové služby](#Cloud-services)).

> [!NOTE]
> Hello seznam rozsahů IP adres, ze kterých se veřejné IP adresy přidělené prostředky tooAzure je publikován v [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

### <a name="dns-hostname-resolution"></a>Překlad názvů hostitelů DNS
Při vytváření virtuálního počítače IaaS nebo cloudové služby, je nutné tooprovide název DNS cloudové služby, které je jedinečné v rámci všechny prostředky v Azure. Tím se vytvoří mapování v hello servery spravovat Azure DNS pro *dnsname*. cloudapp.net toohello veřejnou IP adresu prostředku hello. Například při vytváření cloudové služby s názvem služby DNS služby cloud **contoso**, hello plně kvalifikovaný název domény (FQDN) **contoso.cloudapp.net** vyřešit tooa veřejnou IP adresu (VIP) Hello cloudové služby. Můžete použít tento plně kvalifikovaný název domény toocreate záznam CNAME vlastní doménu směřující toohello veřejnou IP adresu v Azure.

### <a name="cloud-services"></a>Cloud Services
Cloudové služby má vždy veřejných IP adres odkazované tooas virtuální adresy IP (VIP). Koncové body můžete vytvořit v cloudové služby tooassociate jiné porty v hello VIP toointernal porty na virtuální počítače a instance rolí v rámci hello cloudové služby. 

Cloudové služby může obsahovat více virtuálních počítačů IaaS nebo instancí rolí PaaS, všechny zveřejněné prostřednictvím hello stejné cloudové služby VIP. Je také možné přiřadit [více VIP tooa Cloudová služba](../load-balancer/load-balancer-multivip.md), která umožňuje scénáře více virtuálních IP adres jako víceklientské prostředí s weby založené na protokolu SSL.

Můžete zajistit hello veřejnou IP adresu cloudové služby zůstává stejná, i v případě, že všechny hello instance role jsou zastaveny, pomocí hello *statické* veřejnou IP adresu, označují tooas [vyhrazenou IP adresu](virtual-networks-reserved-public-ip.md). Můžete vytvořit prostředek statické IP (vyhrazené) v určitém umístění a přiřaďte ho tooany cloudové služby v rámci daného umístění. Nelze zadat IP adresu skutečného hello pro hello vyhrazené IP, se přidělí z fondu dostupné IP adresy v hello umístění, je vytvořena. Tato IP adresa se neuvolní, dokud neodstraníte explicitně.

Statické (vyhrazené) veřejné IP adresy se obvykle používají ve scénářích hello, kde je Cloudová služba:

* vyžaduje nastavení toobe pravidel brány firewall koncových uživatelů.
* závisí na externí překlad názvů DNS, a dynamických IP by vyžadovaly aktualizace záznamů.
* využívá externí webové služby, které používají model zabezpečení na základě IP.
* používá protokol SSL certifikáty propojené tooan IP adresu.

> [!NOTE]
> Při vytváření klasické virtuální počítač, kontejner *Cloudová služba* se vytvoří v Azure, který má virtuální adresy IP (VIP). Při vytváření hello se provádí prostřednictvím portálu, výchozí protokolu RDP nebo SSH *koncový bod* je nakonfigurovaný portálem hello, takže můžete připojit toohello virtuálních počítačů prostřednictvím hello cloudové služby virtuálních IP adres. Nelze vyhradit VIP této cloudové služby, které efektivně poskytuje vyhrazené toohello tooconnect IP adresy virtuálních počítačů. Další porty můžete otevřít tak, že nakonfigurujete další koncové body.
> 
> 

### <a name="iaas-vms-and-paas-role-instances"></a>Virtuální počítače IaaS a instancí rolí PaaS
Můžete přiřadit veřejné IP adres přímo tooan IaaS [virtuálních počítačů](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo instance role PaaS v rámci cloudové služby. Toto je odkazované tooas úrovni instance veřejnou IP adresu ([splnění](virtual-networks-instance-level-public-ip.md)). Tato veřejná IP adresa může být pouze dynamické.

> [!NOTE]
> To se liší od hello VIP hello cloudové služby, který slouží jako kontejner pro instance rolí virtuálních počítačů IaaS nebo PaaS, vzhledem k tomu, že cloudové služby může obsahovat více virtuálních počítačů IaaS nebo instancí rolí PaaS, všechny zveřejněné prostřednictvím hello stejné cloudové služby virtuální IP adresy.
> 
> 

### <a name="vpn-gateways"></a>VPN Gateway
A [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) můžou být použité tooconnect službě Azure VNet tooother sítě Azure Vnet nebo místní sítě. Brána sítě VPN je přiřazen veřejnou IP adresu *dynamicky*, který umožňuje komunikaci se vzdáleným sítím hello.

### <a name="application-gateways"></a>Application Gateway
Azure [Aplikační brána](../application-gateway/application-gateway-introduction.md) lze použít pro Layer7 Vyrovnávání zatížení tooroute síťový provoz založený na protokolu HTTP. Aplikační brána je přiřazen veřejnou IP adresu *dynamicky*, který slouží jako hello VIP pro vyrovnávání zatížení sítě.

### <a name="at-a-glance"></a>První pohled
Hello tabulka níže uvádí každý typ prostředku s hello možné přidělení metody (dynamický nebo statický) a možnost tooassign víc veřejných IP adres.

| Prostředek | Dynamická | Statická | Několik IP adres |
| --- | --- | --- | --- |
| Cloudové služby |Ano |Ano |Ano |
| Instance role virtuálních počítačů IaaS nebo PaaS |Ano |Ne |Ne |
| VPN Gateway |Ano |Ne |Ne |
| Application Gateway |Ano |Ne |Ne |

## <a name="private-ip-addresses"></a>Privátní IP adresy
Privátní IP adresy umožňují toocommunicate prostředků Azure s jiným prostředkům v cloudové službě nebo [virtuální sítě](virtual-networks-overview.md)(VNet), nebo tooon místní sítě (prostřednictvím brány sítě VPN nebo okruh ExpressRoute), bez použití Internet dostupnou IP adresu.

V modelu nasazení Azure classic je možné privátní IP adresu přiřadit toohello následující prostředky Azure:

* Virtuální počítače IaaS a instancí rolí PaaS
* Interní nástroj pro vyrovnávání zatížení
* Application Gateway

### <a name="iaas-vms-and-paas-role-instances"></a>Virtuální počítače IaaS a instancí rolí PaaS
Virtuální počítače (VM) vytvořené pomocí modelu nasazení classic hello jsou vždy umístěny v cloudové službě, podobně jako tooPaaS instancí rolí. chování Hello soukromé IP adresy jsou proto podobné pro tyto prostředky.

Je důležité toonote, které můžou být Cloudová služba nasadit dvěma způsoby:

* Jako *samostatné* Cloudová služba, pokud to není v rámci virtuální sítě.
* V rámci virtuální sítě.

#### <a name="allocation-method"></a>Metoda přidělování
Pro *samostatné* Cloudová služba, prostředky get privátní IP adresy přidělené *dynamicky* z privátní IP adresy datové centrum Azure hello rozsahu adres. Lze jej použít pouze pro komunikaci s ostatních virtuálních počítačů v rámci hello stejné cloudové služby. Tuto IP adresu lze změnit, pokud prostředek hello zastavit a spustit.

V případě, že cloudové služby nasadit v rámci virtuální sítě, získat prostředky privátní IP adresy přidělené z rozsahu adres hello hello přidružených podsítí (jako je zadaný v konfiguraci sítě). Tento privátní IP adresy lze použít pro komunikaci mezi všechny virtuální počítače v rámci hello virtuální sítě.

Kromě toho v případě cloudové služby v rámci virtuální sítě, je přidělená privátní IP adresy *dynamicky* (pomocí protokolu DHCP) ve výchozím nastavení. Ho lze změnit, pokud prostředek hello zastavit a spustit. hello tooensure hello IP adresa zůstane stejný, musíte metoda přidělení hello tooset příliš*statické*a zadejte platnou IP adresu v rozsahu adres odpovídající hello.

Statické privátní IP adresy se obvykle používají pro:

* Virtuální počítače, které slouží jako řadiče domény nebo servery DNS.
* Virtuální počítače, které jsou potřebné pravidla brány firewall pomocí IP adresy.
* Virtuální počítače spuštěné služby přistupují jiné aplikace pomocí adresy IP.

#### <a name="internal-dns-hostname-resolution"></a>Interní překlad DNS názvu hostitele
Všechny instance rolí PaaS a virtuální počítače Azure jsou nakonfigurovány s [servery spravovat Azure DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) ve výchozím nastavení, pokud explicitně nakonfigurovat vlastní servery DNS. Tyto servery DNS zadejte interní překlad adres pro virtuální počítače a instance rolí, které se nacházejí v hello stejnou virtuální síť nebo cloudové služby.

Když vytvoříte virtuální počítač, se přidá mapování pro název hostitele hello tooits privátní IP adresu toohello spravovat Azure DNS servery. V případě více síťovými Kartami virtuálního počítače, je namapovaný hello hostname toohello privátní IP adresu hello primární síťový adaptér. Tyto informace o mapování je však omezená tooresources v rámci hello stejné cloudové služby nebo virtuální sítě.

Pro *samostatné* Cloudová služba, bude možné tooresolve hostnames všech instancí virtuálních počítačů nebo rolí v rámci hello stejné Cloudová služba jenom. V případě cloudové služby v rámci virtuální sítě bude možné tooresolve názvy hostitelů všech instancí virtuálních počítačů nebo rolí hello v rámci hello virtuální sítě.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interní nástroje pro vyrovnávání a Application Gateway
Můžete přiřadit privátní toohello adresu IP **front-endu** konfigurace [Azure interní nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-internal-overview.md) (ILB) nebo [Azure Application Gateway](../application-gateway/application-gateway-introduction.md). Tato privátní IP adresa slouží jako vnitřní koncový bod, přístupné pouze toohello prostředků v rámci svých virtuálních sítí (VNet) a vzdáleným sítím hello připojení toohello virtuální sítě. Můžete přiřadit buď dynamická nebo statická privátní IP adresu toohello front-endu konfiguraci. Můžete také přiřadit více privátní IP adresy tooenable vip více scénářů.

### <a name="at-a-glance"></a>První pohled
Hello tabulka níže uvádí každý typ prostředku s hello možné přidělení metody (dynamický nebo statický) a možnost tooassign více privátních IP adres.

| Prostředek | Dynamická | Statická | Několik IP adres |
| --- | --- | --- | --- |
| Virtuální počítač (v *samostatné* Cloudová služba) |Ano |Ano |Ano |
| PaaS role instance (v *samostatné* Cloudová služba) |Ano |Ne |Ano |
| Instance role virtuálního počítače nebo PaaS (ve virtuální síti) |Ano |Ano |Ano |
| Nástroje pro vyrovnávání zatížení interní front-end |Ano |Ano |Ano |
| Front-endu aplikace brány |Ano |Ano |Ano |

## <a name="limits"></a>Omezení
Následující tabulka Hello ukazuje hello omezení vynucená pro IP adresách v Azure za předplatné. Můžete [obraťte se na podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooincrease hello výchozí omezení až toohello maximální limit podle obchodních potřeb.

|  | Výchozí omezení | Maximální omezení |
| --- | --- | --- |
| Veřejné IP adresy (dynamické) |5 |kontaktovat podporu |
| Vyhrazené veřejné IP adresy |20 |kontaktovat podporu |
| Veřejné VIP jedno nasazení (Cloudová služba) |5 |kontaktovat podporu |
| Privátní virtuální adresy IP (ILB) na jedno nasazení (Cloudová služba) |1 |1 |

Ujistěte se, abyste si přečetli hello úplnou sadu [omezení pro sítě](../azure-subscription-service-limits.md#networking-limits) v Azure.

## <a name="pricing"></a>Ceny
Ve většině případů jsou volné veřejné IP adresy. Není nominální poplatků toouse další nebo statické veřejné IP adresy. Musíte rozumět hello [struktura cenách pro veřejné IP adresy](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Rozdíly mezi Resource Manager a nasazení classic
Níže je uvedeno porovnání IP adresování funkcí ve Správci prostředků a modelu nasazení classic hello.

|  | Prostředek | Classic | Resource Manager |
| --- | --- | --- | --- |
| **Veřejná IP adresa** |***VIRTUÁLNÍ POČÍTAČ*** |Odkazované tooas splnění (jenom dynamické) |Označuje tooas veřejnou IP adresu (dynamický nebo statický) |
|  ||Přiřazené tooan virtuálních počítačů IaaS nebo PaaS role instance |Síťový adaptér přidružený toohello Virtuálního počítače | |
|  |***Internet přístupných nástroj pro vyrovnávání zatížení*** |Označuje tooas VIP (dynamické) nebo vyhrazené IP adresy (statické) |Označuje tooas veřejnou IP adresu (dynamický nebo statický) | |
|  ||Přiřazené tooa cloudové služby |Konfigurace front-endu služby Vyrovnávání zatížení přidružené toohello | |
|  | | | |
| **Privátní IP adresa** |***VIRTUÁLNÍ POČÍTAČ*** |Odkazované tooas a vyhrazené IP adresy |Označuje tooas privátní IP adresy |
|  ||Přiřazené tooan virtuálních počítačů IaaS nebo PaaS role instance |Síťový adaptér přiřazené toohello Virtuálního počítače | |
|  |***Interní vyrovnávání zátěže (ILB)*** |Přiřazené toohello ILB (dynamický nebo statický) |Konfigurace front-endu přiřazené toohello ILB (dynamická nebo statická) | |

## <a name="next-steps"></a>Další kroky
* [Nasadit virtuální počítač se statickou privátní IP adresou](virtual-networks-static-private-ip-classic-pportal.md) pomocí hello portálu Azure.

