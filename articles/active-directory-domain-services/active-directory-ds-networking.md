---
title: "Azure AD Domain Services: Pokyny pro sítě | Microsoft Docs"
description: "Požadavky sítě pro Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 23a857a5-2720-400a-ab9b-1ba61e7b145a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: maheshu
ms.openlocfilehash: 8306c1ff72d348f5f327b79617e1422a78e26bdb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Požadavky sítě pro Azure AD Domain Services
## <a name="how-to-select-an-azure-virtual-network"></a>Jak vybrat virtuální síť Azure
Následující pokyny vám pomohou vybrat virtuální sítě pro použití se službou Azure AD Domain Services.

### <a name="type-of-azure-virtual-network"></a>Typ virtuální sítě Azure
* Můžete povolit Azure AD Domain Services ve klasické virtuální síti Azure.
* Azure AD Domain Services **nelze povolit v virtuální sítě vytvořené pomocí Azure Resource Manager**.
* Virtuální síť využívající Resource Manager můžete připojit k klasickou virtuální síť, ve které je povolili službu Azure AD Domain Services. Následně můžete použít Azure AD Domain Services ve virtuální síti na základě Resource Manager. Další informace najdete v tématu [připojení k síti](active-directory-ds-networking.md#network-connectivity) části.
* **Regionálních virtuálních sítí**: Pokud máte v úmyslu použít existující virtuální síť, zajistěte, aby byl regionální virtuální síť.

  * Virtuální sítě, které používají starší verzi mechanizmu skupiny vztahů nelze použít se službou Azure AD Domain Services.
  * Chcete-li používat Azure AD Domain Services, [migrovat starší verze virtuálních sítí do regionálních virtuálních sítí](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

### <a name="azure-region-for-the-virtual-network"></a>Oblast Azure pro virtuální síť
* Vaše Azure AD Domain Services spravované domény je nasazená ve stejné oblasti Azure jako virtuální síť rozhodnete povolit služby.
* Vyberte virtuální síť v oblasti Azure podporované službou Azure AD Domain Services.
* Na stránce [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/) se dozvíte, pro jaké oblasti je dostupná služba Azure AD Domain Services.

### <a name="requirements-for-the-virtual-network"></a>Požadavky pro virtuální síť
* **Blízkosti Azure úlohy**: vyberte virtuální síť, které aktuálně hostitelem virtuálních počítačů, kteří potřebují přístup k Azure AD Domain Services.
* **Vlastní nebo přineste vlastní servery DNS**: Ujistěte se, že neexistují žádné vlastní servery DNS, které jsou nakonfigurované pro virtuální síť.
* **Stávajících doménách se stejným názvem domény**: Ujistěte se, že nemáte existující doménu se stejným názvem domény k dispozici ve virtuální síti. Například předpokládejme, že již máte doménu „contoso.com“ k dispozici na vybrané virtuální síti. Později Zkuste povolit spravované doméně služby Azure AD Domain Services s se stejným názvem domény (to znamená "contoso.com") ve virtuální síti. Dojde k chybě při pokusu o povolení služby Azure AD Domain Services. Toto selhání je z důvodu konfliktu názvů pro název domény ve virtuální síti. V této situaci musíte použít jiný název pro nastavení domény spravované službou Azure AD Domain Services. Případně můžete zrušit existující doménu a poté pokračovat v povolení služby Azure AD Domain Services.

> [!WARNING]
> Domain Services nelze přesunout do jiné virtuální síti, poté, co povolíte službu.
>
>

## <a name="network-security-groups-and-subnet-design"></a>Skupiny zabezpečení sítě a podsítě návrhu
A [skupina zabezpečení sítě (NSG)](../virtual-network/virtual-networks-nsg.md) obsahuje seznam pravidel seznamu řízení přístupu (ACL), která povolují nebo odpírají síťový provoz instancím virtuálních počítačů ve virtuální síti. Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti. Pokud je skupina zabezpečení sítě přidružená k podsíti, pravidla seznamu ACL platí pro všechny instance virtuálních počítačů v této podsíti. Kromě toho je možné omezit provoz do konkrétního virtuálního počítače další tím, že přidružíte skupinu NSG přímo do tohoto virtuálního počítače.

![Doporučené podsíť návrhu](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

### <a name="best-practices-for-choosing-a-subnet"></a>Osvědčené postupy pro výběr podsíť
* Nasazení Azure AD Domain Services **oddělit vyhrazené podsíť** v rámci virtuální sítě Azure.
* Neplatí skupiny Nsg k podsíti vyhrazené vaší spravované domény. Pokud musíte použít skupiny Nsg na vyhrazené podsíť, ujistěte se, můžete **nezadávejte blokovat porty vyžadované pro služby a spravovat vaše doména**.
* Neomezují zbytečně počet IP adres, které jsou k dispozici v rámci podsítě vyhrazené vaší spravované domény. Toto omezení zabrání službu zpřístupnění dva řadiče domény vaší spravované domény.
* **Nepovolujte Azure AD Domain Services v podsíti brány** virtuální sítě.

> [!WARNING]
> Pokud přidružíte skupinu NSG s podsítí, ve kterém služba Azure AD Domain Services je povoleno, mohou narušit možnost služeb a spravovat doménu společnosti Microsoft. Kromě toho synchronizace mezi vašeho klienta Azure AD a vaší spravované domény dojde k narušení. **Smlouva SLA se nevztahuje na nasazení, kde skupinu NSG použil blokující Azure AD Domain Services z aktualizace a Správa domény.**
>
>

### <a name="ports-required-for-azure-ad-domain-services"></a>Porty vyžadované pro Azure AD Domain Services
Následující porty jsou povinné pro Azure AD Domain Services do služby a údržbu vaší spravované domény. Zajistěte, aby tyto porty nejsou blokováno pro podsíť, ve kterém jste povolili vaší spravované domény.

| Číslo portu | Účel |
| --- | --- |
| 443 |Synchronizace se klientovi služby Azure AD |
| 3389 |Správa vaší domény |
| 5986 |Správa vaší domény |
| 636 |Zabezpečený LDAP (LDAPS) přístup k spravované doméně |

### <a name="sample-nsg-for-virtual-networks-with-azure-ad-domain-services"></a>Ukázka NSG pro virtuální sítě s Azure AD Domain Services
Následující tabulka znázorňuje ukázku NSG můžete nakonfigurovat pro virtuální síť s spravované doméně služby Azure AD Domain Services. Toto pravidlo umožňuje příchozí provoz z výše zadaný portů, aby vaše spravované domény zůstane opravit, aktualizovat a společnost Microsoft se dá sledovat. Výchozí pravidlo, DenyAll, platí pro všechny ostatní příchozí přenosy z Internetu.

Kromě toho NSG také ukazuje, jak zamknout zabezpečený LDAP přístup přes internet. Přeskočte toto pravidlo, pokud jste nepovolili zabezpečený LDAP přístup k vaší spravované domény přes internet. NSG obsahuje sadu pravidel, která povolí příchozí LDAPS přístup přes port TCP 636 pouze ze zadané sady IP adres. Pravidla NSG pro povolení LDAPS přístupu přes internet ze zadaných IP adres má vyšší prioritu než skupina NSG DenyAll pravidlo.

![Ukázka skupiny NSG k zabezpečení přístupu k LDAPS přes internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Další informace** - [vytvořit skupinu zabezpečení sítě](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).


## <a name="network-connectivity"></a>Připojení k síti
Spravované doméně služby Azure AD Domain Services lze povolit pouze v rámci jedné klasické virtuální sítě v Azure. Virtuální sítě vytvořené pomocí Azure Resource Manager nejsou podporovány.

### <a name="scenarios-for-connecting-azure-networks"></a>Scénáře pro připojení sítě Azure
Připojte virtuální sítě Azure používat spravované doméně v některém z těchto scénářích nasazení:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Používat spravované domény ve více než jeden klasické virtuální síti Azure
Další Azure klasické virtuální sítě můžete připojit k Azure klasickou virtuální síť, ve kterém jste povolili službu Azure AD Domain Services. Toto připojení VPN umožňuje spravované doméně pomocí úlohy nasazené v jiných virtuálních sítí.

![Připojení klasické virtuální sítě](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Používat spravované domény ve virtuální síti na základě Resource Manager
Virtuální síť využívající Resource Manager můžete připojit k Azure klasickou virtuální síť, ve kterém jste povolili službu Azure AD Domain Services. Toto připojení umožňuje spravované doméně pomocí úlohy nasazené ve virtuální síti na základě Resource Manager.

![Správce prostředků klasickou virtuální síť připojení](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>Možnosti připojení sítě
* **Připojení VNet-to-VNet pomocí připojení site-to-site VPN**: propojení virtuální sítě s jinou virtuální sítí (VNet-to-VNet) je podobné propojení virtuální sítě do umístění místního webu. Oba typy připojení využívají bránu VPN k poskytnutí zabezpečeného tunelového propojení prostřednictvím protokolu IPsec/IKE.

    ![Připojení k virtuální síti s použitím brány VPN](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Další informace - připojení virtuální sítě pomocí brány sítě VPN](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* **Připojení VNet-to-VNet pomocí virtuální sítě, partnerský vztah**: partnerský vztah virtuální sítě je mechanismus, který se připojuje prostřednictvím Azure páteřní síti dvou virtuálních sítí ve stejné oblasti. Po navázání partnerského vztahu se tyto dvě virtuální sítě pro všechny účely připojení jeví jako jedna síť. Jsou i nadále spravovány jako samostatné prostředky, ale virtuální počítače v těchto virtuálních sítích spolu mohou komunikovat přímo pomocí privátních IP adres.

    ![Připojení k virtuální síti pomocí partnerského vztahu](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Další informace - virtuální sítě partnerského vztahu](../virtual-network/virtual-network-peering-overview.md)

<br>

## <a name="related-content"></a>Související obsah
* [Partnerský vztah virtuální síť Azure](../virtual-network/virtual-network-peering-overview.md)
* [Nakonfigurujte připojení VNet-to-VNet pro model nasazení classic](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Skupiny zabezpečení sítě Azure](../virtual-network/virtual-networks-nsg.md)
* [Vytvořit skupinu zabezpečení sítě](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
