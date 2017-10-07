---
title: typy adres aaaIP v Azure | Microsoft Docs
description: "Další informace o veřejných a privátních IP adresách v Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 402d3707c00f0b3bf3ef1febd5ade66223da74bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a>Typy IP adres a metody přidělování v Azure
Můžete přiřadit IP adresy tooAzure prostředky toocommunicate s další prostředky Azure, v místní síti a Internetu hello. Existují dva typy IP adres, které můžete v Azure využít:

* **Veřejné IP adresy**: používají ke komunikaci s hello Internetu, včetně Azure veřejně přístupných služeb
* **Privátní IP adresy**: používá pro komunikaci v rámci virtuální sítě Azure (VNet) a místní sítě, pokud používáte brány VPN nebo tooextend okruh ExpressRoute tooAzure vaší sítě.

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).  Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](virtual-network-ip-addresses-overview-classic.md).
> 

Pokud jste obeznámeni s modelem nasazení classic hello, zkontrolujte hello [rozdíly v IP adresách mezi classic a Resource Manager](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Veřejné IP adresy
Veřejné IP adresy umožňují prostředkům Azure toocommunicate s veřejnými službami Internet a Azure, jako [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [databází SQL](../sql-database/sql-database-technical-overview.md), a [úložiště Azure](../storage/common/storage-introduction.md).

[Veřejná IP](resource-groups-networking.md#public-ip-address) adresa v Azure Resource Manageru je prostředek, který má svoje vlastní vlastnosti. Prostředek veřejné IP adresy můžete přidružit s žádným z hello následující prostředky:

* Virtuální počítače
* Internetové nástroje pro vyrovnávání zatížení
* VPN Gateway
* Application Gateway

### <a name="allocation-method"></a>Metoda přidělování
Existují dvě metody, ve kterých se přidělit IP adresu tooa *veřejné* prostředek IP - *dynamické* nebo *statické*. Metoda přidělení výchozí Hello je *dynamické*, kde je IP adresa **není** přidělené v době hello jeho vytvoření. Místo toho hello veřejná IP adresa je přidělen při spuštění (nebo vytvoření) hello přidružené prostředků (např. virtuální počítač nebo službu Vyrovnávání zatížení). Hello IP adresa se neuvolní při zastavení (nebo odstranit) hello prostředků. To způsobí, že hello IP adresu toochange při zastavení a spuštění prostředku.

tooensure hello IP adresu pro zůstane technologie hello přidružených prostředků hello stejné, můžete nastavit metoda přidělení hello explicitně příliš*statické*. V tomto případě se IP adresa přiřadí okamžitě. Jeho vydání jenom v případě, že můžete odstranit prostředek hello nebo změnit jeho metoda přidělení příliš*dynamické*.

> [!NOTE]
> I když nastavíte hello metoda přidělení příliš*statické*, nelze zadat hello skutečné IP adresu přiřazenou toohello *prostředek veřejné IP*. Místo toho získá přidělit z fondu dostupné IP adresy v hello umístění Azure hello prostředek vytvoří v.
>

Statické veřejné IP adresy se obvykle používají v hello následující scénáře:

* Koncoví uživatelé potřebují toocommunicate pravidla brány firewall tooupdate s vašich prostředků Azure.
* Překlad názvů DNS, kde by změna IP adresy vyžadovala aktualizace záznamů A.
* Vaše prostředky Azure komunikují s ostatními aplikacemi nebo službami, které využívají model zabezpečení založený na IP adresách.
* Použijete adresu IP propojené tooan certifikáty SSL.

> [!NOTE]
> Hello seznam rozsahů IP adres, ze kterých mají veřejné IP adresy (dynamický nebo statický) při přidělování prostředků tooAzure je publikován v [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653).
>

### <a name="dns-hostname-resolution"></a>Překlad názvů hostitelů DNS
Můžete zadat název název domény DNS pro prostředek veřejné IP, která vytvoří mapování pro *domainnamelabel*. *umístění*. cloudapp.azure.com toohello veřejnou IP adresu v hello spravovat Azure DNS servery. Například pokud vytvoříte prostředek veřejné IP s **contoso** jako *domainnamelabel* v hello **západní USA** Azure *umístění*, hello plně kvalifikovaný název domény (FQDN) **contoso.westus.cloudapp.azure.com** vyřešit toohello veřejnou IP adresu prostředku hello. Můžete použít tento plně kvalifikovaný název domény toocreate záznam CNAME vlastní doménu směřující toohello veřejnou IP adresu v Azure.

> [!IMPORTANT]
> Každý vytvořený popisek názvu domény musí být v rámci příslušného umístění Azure jedinečný.  
>

### <a name="virtual-machines"></a>Virtuální počítače
Můžete přidružit veřejnou IP adresu s [Windows](../virtual-machines/windows/overview.md) nebo [Linux](../virtual-machines/virtual-machines-linux-about.md) virtuální počítač podle jeho přiřazení tooits **síťové rozhraní**. V případě hello virtuálního počítače s více síťovými rozhraními, můžete je přiřadit toohello *primární* pouze síťové rozhraní. Můžete přiřadit dynamický nebo statickou veřejnou IP adresu tooa virtuálních počítačů.

### <a name="internet-facing-load-balancers"></a>Internetové nástroje pro vyrovnávání zatížení
Veřejnou IP adresu s můžete přidružit [Vyrovnávání zatížení Azure](../load-balancer/load-balancer-overview.md), přiřazením toohello nástroj pro vyrovnávání zatížení **front-endu** konfigurace. Tato veřejná IP adresa slouží jako virtuální IP adresa (VIP) s vyrovnáváním zatížení. Můžete přiřadit dynamický nebo statický veřejnou IP adresu tooa Vyrovnávání zatížení front-endu. Je také možné přiřadit více veřejné IP adresy tooa nástroj pro vyrovnávání zatížení front-end, který umožňuje [více virtuálních IP adres](../load-balancer/load-balancer-multivip.md) scénáře jako prostředí s více klienty s weby založené na protokolu SSL.

### <a name="vpn-gateways"></a>VPN Gateway
[Služba Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) je použité tooconnect virtuální sítě Azure (VNet) tooother sítě Azure Vnet nebo tooan místní síť. Budete potřebovat tooassign veřejné tooits adresu IP **konfigurace protokolu IP** tooenable ho toocommunicate s hello vzdálené sítě. V současné době můžete přiřadit jenom *dynamické* veřejnou IP adresu tooa VPN gateway.

### <a name="application-gateways"></a>Application Gateway
Veřejnou IP adresu můžete přidružit Azure [Application Gateway](../application-gateway/application-gateway-introduction.md), přiřazením toohello brány **front-endu** konfigurace. Tato veřejná IP adresa slouží jako virtuální IP adresa (VIP) s vyrovnáváním zatížení. V současné době můžete přiřadit jenom *dynamické* veřejnou IP adresu tooan aplikace brány front-endovou konfiguraci.

### <a name="at-a-glance"></a>Přehledně
Následující tabulka Hello ukazuje hello určitou vlastnost, pomocí kterého může být veřejné IP adresy přidružené tooa nejvyšší úrovně prostředků a metody možné přidělení hello (dynamická nebo statická), které lze použít.

| Prostředek nejvyšší úrovně | Přidružení IP adresy | Dynamická | Statická |
| --- | --- | --- | --- |
| Virtuální počítač |Síťové rozhraní |Ano |Ano |
| Nástroj pro vyrovnávání zatížení |Konfigurace front-endu |Ano |Ano |
| VPN Gateway |Konfigurace protokolu IP brány |Ano |Ne |
| Application Gateway |Konfigurace front-endu |Ano |Ne |

## <a name="private-ip-addresses"></a>Privátní IP adresy
Povolit toocommunicate prostředků Azure s jiným prostředkům v privátní IP adresy [virtuální sítě](virtual-networks-overview.md) nebo k místní síti prostřednictvím brány sítě VPN nebo okruhem ExpressRoute, bez použití Internetu dostupná IP adresa.

V modelu nasazení Azure Resource Manager hello je privátní IP adresy přidružené toohello následující typy prostředků Azure:

* Virtuální počítače
* Interní nástroje pro vyrovnávání zatížení
* Application Gateway

### <a name="allocation-method"></a>Metoda přidělování
Privátní IP adresa je přidělena z adresy hello je připojen rozsah hello podsíť toowhich hello prostředku. rozsah adres Hello samotné podsítě hello je součástí rozsah adres hello VNet.

Existují dvě metody přidělení privátní IP adresy: *dynamická* a *statická*. Metoda přidělení výchozí Hello je *dynamické*, kde je automaticky přidělit hello IP adresu z podsítě hello prostředků (pomocí protokolu DHCP). Tuto IP adresu můžou změnit při zastavení a spuštění hello prostředků.

Můžete nastavit hello metoda přidělení příliš*statické* tooensure hello IP adresa zůstane hello stejné. V takovém případě musíte také tooprovide platnou IP adresu, která je součástí hello prostředků podsítě.

Statické privátní IP adresy se obvykle používají pro:

* Virtuální počítače, které slouží jako řadiče domény nebo servery DNS.
* Prostředky, které vyžadují pravidla brány firewall s využitím IP adres.
* Prostředky, ke kterým se přistupuje z jiných aplikací nebo prostředků prostřednictvím IP adresy.

### <a name="virtual-machines"></a>Virtuální počítače
Privátní IP adresy je přiřazen toohello **síťové rozhraní** z [Windows](../virtual-machines/windows/overview.md) nebo [Linux](../virtual-machines/virtual-machines-linux-about.md) virtuálních počítačů. V případě virtuálního počítače s několika síťovými rozhraními se privátní IP adresa přiřadí každému z těchto rozhraní. Metoda přidělení hello můžete zadat jako dynamické nebo statické pro síťové rozhraní.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Překlad názvů hostitelů interních služeb DNS (pro virtuální počítače)
Všechny virtuální počítače Azure jsou ve výchozím nastavení nakonfigurované se [servery DNS spravovanými Azure](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) (pokud explicitně nenakonfigurujete vlastní servery DNS). Tyto servery DNS zadejte interní překlad adres pro virtuální počítače, které se nacházejí v hello stejné virtuální síti.

Když vytvoříte virtuální počítač, se přidá mapování pro název hostitele hello tooits privátní IP adresu toohello spravovat Azure DNS servery. V případě více síťového rozhraní virtuálního počítače, je namapovaný hello hostname toohello privátní IP adresu hello primární síťové rozhraní.

Virtuální počítače nakonfigurované servery spravovat Azure DNS bude mít tooresolve hello hostnames všechny virtuální počítače v rámci virtuální sítě tootheir soukromých IP adres.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interní nástroje pro vyrovnávání a Application Gateway
Můžete přiřadit privátní toohello adresu IP **front-endu** konfigurace [Azure interní nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-internal-overview.md) (ILB) nebo [Azure Application Gateway](../application-gateway/application-gateway-introduction.md). Tato privátní IP adresa slouží jako vnitřní koncový bod, přístupné pouze toohello prostředků v rámci svých virtuálních sítí (VNet) a vzdáleným sítím hello připojení toohello virtuální sítě. Můžete přiřadit buď dynamická nebo statická privátní IP adresu toohello front-endu konfiguraci.

### <a name="at-a-glance"></a>Přehledně
Následující tabulka Hello ukazuje hello určitou vlastnost, pomocí kterého může být privátní IP adresy přidružené tooa nejvyšší úrovně prostředků a metody možné přidělení hello (dynamická nebo statická), které lze použít.

| Prostředek nejvyšší úrovně | Přidružení IP adresy | Dynamická | Statická |
| --- | --- | --- | --- |
| Virtuální počítač |Síťové rozhraní |Ano |Ano |
| Nástroj pro vyrovnávání zatížení |Konfigurace front-endu |Ano |Ano |
| Application Gateway |Konfigurace front-endu |Ano |Ano |

## <a name="limits"></a>Omezení
Hello omezení vynucená pro IP adresy jsou uvedené v hello kompletní [omezení pro sítě](../azure-subscription-service-limits.md#networking-limits) v Azure. Tato omezení platí pro jednotlivé oblasti a jednotlivá předplatná. Můžete [obraťte se na podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooincrease hello výchozí omezení až toohello maximální limit podle obchodních potřeb.

## <a name="pricing"></a>Ceny
Za veřejné IP adresy se může účtovat nominální poplatek. toolearn Další informace o IP adres, ceny v Azure, zkontrolujte hello [IP adresu ceny](https://azure.microsoft.com/pricing/details/ip-addresses) stránky.

## <a name="next-steps"></a>Další kroky
* [Nasadit virtuální počítač se statickou veřejnou IP adresu pomocí hello portálu Azure](virtual-network-deploy-static-pip-arm-portal.md)
* [Nasazení virtuálního počítače se statickou veřejnou IP adresou pomocí šablony](virtual-network-deploy-static-pip-arm-template.md)
* [Nasadit virtuální počítač se statickou privátní IP adresu pomocí hello portálu Azure](virtual-networks-static-private-ip-arm-pportal.md)
