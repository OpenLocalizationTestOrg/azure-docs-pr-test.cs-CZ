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
ms.openlocfilehash: 804d4ea7d1b3b07b6d224855c7adb90bdfe24022
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Požadavky sítě pro Azure AD Domain Services
## <a name="how-tooselect-an-azure-virtual-network"></a>Jak tooselect virtuální sítě Azure
Hello následující pokyny vám pomohou vybrat toouse virtuální síť s Azure AD Domain Services.

### <a name="type-of-azure-virtual-network"></a>Typ virtuální sítě Azure
* Můžete povolit Azure AD Domain Services ve klasické virtuální síti Azure.
* Azure AD Domain Services **nelze povolit v virtuální sítě vytvořené pomocí Azure Resource Manager**.
* Můžete se připojit virtuální sítě Resource Manager tooa klasickou virtuální síť ve které je povolili službu Azure AD Domain Services. Následně můžete použít Azure AD Domain Services ve virtuální síti založené na správci prostředků hello. Další informace najdete v tématu hello [připojení k síti](active-directory-ds-networking.md#network-connectivity) části.
* **Regionálních virtuálních sítí**: Pokud máte v plánu toouse existující virtuální síť, zajistěte, aby byl regionální virtuální síť.

  * Virtuální sítě, které používají starší verze skupiny vztahů mechanismus hello nelze použít s Azure AD Domain Services.
  * toouse Azure AD Domain Services, [migrovat starší verze virtuálních sítí tooregional virtuálních sítí](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

### <a name="azure-region-for-hello-virtual-network"></a>Oblast Azure pro virtuální síť hello
* Vaše Azure AD Domain Services spravované domény je nasazena v hello jako hello virtuální síť, kterou zvolíte tooenable hello služby ve stejné oblasti Azure.
* Vyberte virtuální síť v oblasti Azure podporované službou Azure AD Domain Services.
* V tématu hello [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/) tooknow stránku hello oblastí Azure, ve kterých je dostupná služba Azure AD Domain Services.

### <a name="requirements-for-hello-virtual-network"></a>Požadavky pro virtuální síť hello
* **Bezkontaktní tooyour Azure úlohy**: Vyberte hello virtuální sítě, které aktuálně hostitelem virtuálních počítačů, které potřebují přístup k tooAzure AD Domain Services.
* **Vlastní nebo přineste vlastní servery DNS**: Ujistěte se, že neexistují žádné vlastní servery DNS, které jsou nakonfigurované pro virtuální síť hello.
* **Existující domény s hello stejným názvem domény**: Ujistěte se, že nemáte existující domény s hello stejným názvem domény k dispozici ve virtuální síti. Například předpokládejme, že máte doméně s názvem "contoso.com" již k dispozici na vybranou virtuální síť hello. Později, zkuste tooenable spravované doméně služby Azure AD Domain Services s hello stejný název domény ("contoso.com") ve virtuální síti. Dojde k chybě při pokusu o tooenable Azure AD Domain Services. Toto selhání je z důvodu konfliktu tooname pro název domény hello ve virtuální síti. V takovém případě musíte použít jiný název tooset zálohu vaší spravované domény služby Azure AD Domain Services. Alternativně můžete zrušte zřizovat hello existující domény a poté pokračujte tooenable Azure AD Domain Services.

> [!WARNING]
> Po povolení služby hello nemůžete přesunout Domain Services tooa jinou virtuální síť.
>
>

## <a name="network-security-groups-and-subnet-design"></a>Skupiny zabezpečení sítě a podsítě návrhu
A [skupina zabezpečení sítě (NSG)](../virtual-network/virtual-networks-nsg.md) obsahuje seznam pravidel seznamu řízení přístupu (ACL), která povolují nebo odpírají síťový provoz tooyour instance virtuálních počítačů ve virtuální síti. Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti. Pokud je skupina NSG přidružená k podsíti, pravidla seznamu ACL hello platí tooall hello instance virtuálních počítačů v této podsíti. Kromě toho tooan provoz jednotlivých virtuálních počítačů, je možné omezit tím, že přidružíte skupinu NSG další přímo toothat virtuálních počítačů.

![Doporučené podsíť návrhu](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

### <a name="best-practices-for-choosing-a-subnet"></a>Osvědčené postupy pro výběr podsíť
* Nasazení služby Azure AD Domain Services tooa **oddělit vyhrazené podsíť** v rámci virtuální sítě Azure.
* Nelze použít skupiny Nsg toohello vyhrazené podsítě vaší spravované domény. Pokud musíte použít skupiny Nsg toohello vyhrazené podsíť, ujistěte se, můžete **nezadávejte blokovat hello porty požadované tooservice a spravovat vaše doména**.
* Neomezují zbytečně hello počet IP adres, které jsou k dispozici v rámci podsítě hello vyhrazené vaší spravované domény. Toto omezení zabrání hello služby zpřístupnění dva řadiče domény vaší spravované domény.
* **Nepovolujte Azure AD Domain Services v podsíti brány hello** virtuální sítě.

> [!WARNING]
> Pokud přidružíte skupinu NSG s podsítí, ve kterém služba Azure AD Domain Services je povoleno, může přerušit tooservice možnost společnosti Microsoft a spravovat hello domény. Kromě toho synchronizace mezi vašeho klienta Azure AD a vaší spravované domény dojde k narušení. **Hello SLA nevztahuje toodeployments kde skupinu NSG použil blokující Azure AD Domain Services z aktualizace a Správa domény.**
>
>

### <a name="ports-required-for-azure-ad-domain-services"></a>Porty vyžadované pro Azure AD Domain Services
Hello následující porty jsou povinné pro tooservice Azure AD Domain Services a udržovat vaší spravované domény. Zajistěte, aby tyto porty nejsou blokováno pro hello podsíť, ve kterém jste povolili vaší spravované domény.

| Číslo portu | Účel |
| --- | --- |
| 443 |Synchronizace se klientovi služby Azure AD |
| 3389 |Správa vaší domény |
| 5986 |Správa vaší domény |
| 636 |Zabezpečený LDAP (LDAPS) přístup tooyour spravované doméně |

### <a name="sample-nsg-for-virtual-networks-with-azure-ad-domain-services"></a>Ukázka NSG pro virtuální sítě s Azure AD Domain Services
Hello následující tabulka znázorňuje ukázku NSG můžete nakonfigurovat pro virtuální síť s spravované doméně služby Azure AD Domain Services. Toto pravidlo umožňuje příchozí provoz z hello výše zadaný porty tooensure vaší spravované domény zůstane opravit, aktualizovat a společnost Microsoft se dá sledovat. Hello výchozí 'DenyAll' pravidlo tooall další příchozí provoz z hello Internetu.

Kromě toho hello NSG také ukazuje, jak hello toolock dolů zabezpečený LDAP přístup přes internet. Přeskočit toto pravidlo, pokud jste nepovolili zabezpečený LDAP tooyour spravované domény přístupu přes hello Internetu. Hello NSG obsahuje sadu pravidel, která povolí příchozí LDAPS přístup přes port TCP 636 pouze ze zadané sady IP adres. Hello NSG pravidlo tooallow LDAPS přístup prostřednictvím hello má vyšší prioritu než hello pravidla DenyAll NSG internet ze zadaných IP adres.

![Hello ukázka NSG toosecure LDAPS přístup prostřednictvím Internetu](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Další informace** - [vytvořit skupinu zabezpečení sítě](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).


## <a name="network-connectivity"></a>Připojení k síti
Spravované doméně služby Azure AD Domain Services lze povolit pouze v rámci jedné klasické virtuální sítě v Azure. Virtuální sítě vytvořené pomocí Azure Resource Manager nejsou podporovány.

### <a name="scenarios-for-connecting-azure-networks"></a>Scénáře pro připojení sítě Azure
Připojte virtuální sítě Azure toouse hello spravované domény v některém z hello následující scénáře nasazení:

#### <a name="use-hello-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Použití hello spravované domény ve více než jeden klasické virtuální síti Azure
Další toohello Azure klasické virtuální sítě Azure se můžete připojit klasickou virtuální síť, ve kterém jste povolili službu Azure AD Domain Services. Toto připojení VPN můžete toouse hello spravované doméně pomocí úlohy nasazené v jiných virtuálních sítí.

![Připojení klasické virtuální sítě](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-hello-managed-domain-in-a-resource-manager-based-virtual-network"></a>Použití hello spravované domény ve virtuální síti na základě Resource Manager
Toohello virtuální sítě pomocí Správce prostředků Azure se můžete připojit klasickou virtuální síť, ve kterém jste povolili službu Azure AD Domain Services. Toto připojení umožňuje toouse hello spravované doméně pomocí úlohy nasazené ve virtuální síti založené na správci prostředků hello.

![Připojení k virtuální síti tooclassic Resource Manager](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>Možnosti připojení sítě
* **Připojení VNet-to-VNet pomocí připojení site-to-site VPN**: propojení virtuální sítě tooanother virtuální síť (VNet-to-VNet) je podobné tooconnecting umístění lokality tooan místní virtuální sítě. Oba typy připojení využívají bránu tooprovide sítě VPN přes zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE.

    ![Připojení k virtuální síti s použitím brány VPN](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Další informace - připojení virtuální sítě pomocí brány sítě VPN](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* **Připojení VNet-to-VNet pomocí virtuální sítě, partnerský vztah**: partnerský vztah virtuální sítě je mechanismus, který připojí dvou virtuálních sítí v hello stejné oblasti prostřednictvím hello Azure páteřní síti. Po peered, hello dvě virtuální sítě se zobrazí jako jedna pro všechny účely připojení. Jsou i nadále spravovány jako samostatné prostředky, ale virtuální počítače v těchto virtuálních sítích spolu mohou komunikovat přímo pomocí privátních IP adres.

    ![Připojení k virtuální síti pomocí partnerského vztahu](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Další informace - virtuální sítě partnerského vztahu](../virtual-network/virtual-network-peering-overview.md)

<br>

## <a name="related-content"></a>Související obsah
* [Partnerský vztah virtuální síť Azure](../virtual-network/virtual-network-peering-overview.md)
* [Nakonfigurujte připojení VNet-to-VNet pro model nasazení classic hello](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Skupiny zabezpečení sítě Azure](../virtual-network/virtual-networks-nsg.md)
* [Vytvořit skupinu zabezpečení sítě](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
