---
title: "aaaWhat je seznam řízení přístupu síti Azure?"
description: "Další informace o seznamy řízení přístupu v Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 83d66c84-8f6b-4388-8767-cd2de3e72d76
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: a2681af035d162362771e6df9864bbe838f16352
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-an-endpoint-access-control-list"></a>Co je seznam řízení přístupu koncový bod?

> [!IMPORTANT]
> Azure má dva různé [modely nasazení](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) pro vytváření a práci s prostředky: Resource Manager a Klasický model. Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model nasazení Resource Manager hello. 

Seznam řízení přístupu (ACL) koncového bodu je prvkem zvýšení zabezpečení, která je k dispozici pro vaše nasazení Azure. Seznam ACL poskytuje hello možnost tooselectively povolení nebo zakážou provoz pro koncový bod virtuálního počítače. Tato funkce filtrování paketů poskytuje další úroveň zabezpečení. Můžete zadat sítě ACL pro koncové body pouze. Nelze zadat seznam ACL pro virtuální síť nebo podsíť konkrétní obsažené ve virtuální síti. Toouse skupin zabezpečení sítě (Nsg) se doporučuje namísto seznamy ACL, kdykoli je to možné. toolearn Další informace o skupin Nsg, najdete v části [přehled skupiny zabezpečení sítě](virtual-networks-nsg.md)

Seznamy ACL můžete nakonfigurovat pomocí prostředí PowerShell nebo hello portálu Azure. tooconfigure seznamu ACL sítě pomocí prostředí PowerShell, najdete v části [seznamy řízení přístupu pro správu pro koncové body pomocí prostředí PowerShell](virtual-networks-acl-powershell.md). hello tooconfigure sítě ACL pomocí portálu Azure najdete [jak tooset koncové body tooa virtuálního počítače](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Pomocí seznamů ACL sítě, můžete provést následující hello:

* Selektivně povolit nebo zakázat příchozí přenosů na základě vzdálené podsítě IPv4 adresu rozsahu tooa vstupní koncový bod virtuálního počítače.
* Zakázaných IP adresy
* Vytvořením několika pravidel pro koncový bod virtuálního počítače
* Použít pravidlo řazení tooensure hello správnou sadu pravidel jsou použity na koncový bod daným virtuálním počítačem (nejnižší toohighest)
* Zadejte seznam ACL pro určité vzdálené podsítě IPv4 adresu.

V tématu hello [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) článku omezení řízení přístupu.

## <a name="how-acls-work"></a>Jak fungují seznamy ACL
Seznam ACL je objekt, který obsahuje seznam pravidel. Při vytvoření seznamu ACL a použijte ho tooa koncový bod virtuálního počítače, filtrování paketů probíhá na hello uzlu hostitele virtuálního počítače. To znamená, že hello provoz z vzdálené IP adresy se filtrují podle hello uzlu hostitele pro odpovídající pravidla seznamu ACL místo na vašem virtuálním počítači. Virtuální počítač zabrání výdaje hello drahocenný cyklů procesoru na filtrování paketů.

Při vytváření virtuálního počítače, výchozí seznamu ACL je uvést do místní tooblock veškerý příchozí provoz. Ale pokud je pro (port 3389) vytvořen koncový bod, pak hello výchozí seznam ACL je upravit tooallow veškerý příchozí provoz pro tento koncový bod. Příchozí přenosy z jakékoli vzdálené podsítě je povolena pak toothat koncový bod a zřizování žádné brány firewall se vyžaduje. Všechny ostatní porty jsou zablokovány pro příchozí provoz, pokud jsou koncové body vytvořené pro tyto porty. Ve výchozím nastavení je povoleno odchozí provoz.

**Příklad seznamu ACL výchozí tabulky**

| **Pravidlo č** | **Vzdálené podsítě** | **Koncový bod** | **Povolení nebo odepření** |
| --- | --- | --- | --- |
| 100 |0.0.0.0/0 |3389 |Povolit |

## <a name="permit-and-deny"></a>Povolit a odepřít
Můžete selektivně povolit nebo odepřít síťových přenosů pro vstupní koncový bod virtuálního počítače vytvořením pravidel, které určují "Povolit" nebo "deny". Je důležité toonote, že ve výchozím nastavení, když je vytvořen koncový bod, jsou povoleny všechny přenosy toohello koncový bod. Z tohoto důvodu je důležité toounderstand jak toocreate pravidla povolení nebo odmítnutí a umístěte je v hello správné pořadí podle priority, pokud chcete, aby podrobnou kontrolu nad hello sítě provoz, který zvolíte hello tooallow tooreach-koncový bod virtuálního počítače.

Tooconsider body:

1. **Žádné ACL –** ve výchozím nastavení při vytvoření koncový bod jsme povolit všechny pro koncový bod hello.
2. **Povolit -** při přidávání jednoho nebo více "Povolit" rozsahy jsou odepření všechny ostatní rozsahy ve výchozím nastavení. Jenom pakety z hello povolené rozsah IP adres bude možné toocommunicate s hello koncový bod virtuálního počítače.
3. **Odepřít -** při přidávání jednoho nebo více "deny" rozsahy jsou umožňující všechny rozsahy provozu ve výchozím nastavení.
4. **Kombinace povolení a Odepřít -** můžete použít kombinaci "povolení" a "deny", pokud chcete toocarve na konkrétní IP adresu v rozsahu toobe povolen nebo odepřen přístup.

## <a name="rules-and-rule-precedence"></a>Priorita pravidla a pravidla
Seznamy ACL sítě můžete nastavit na konkrétní virtuální počítač koncové body. Můžete například zadat síť seznamu ACL pro koncový bod RDP vytvořit na virtuálním počítači, jaké zámky dolů přístup pro určité IP adresy. Hello následující tabulka uvádí způsob toogrant přístup toopublic virtuálních IP adres (VIP) některé rozsah toopermit přístupu pro protokol RDP. Jiné vzdálené IP je odepřen. Jsme podle *nejnižší má přednost před* pravidlo pořadí.

### <a name="multiple-rules"></a>Více pravidel
V příkladu hello níže, pokud chcete, aby tooallow koncový bod RDP toohello přístup pouze z dva veřejné rozsahy IPv4 adres (65.0.0.0/8 a 159.0.0.0/8), lze dosáhnout zadáním dva *povolit* pravidla. V takovém případě od protokolu RDP pro virtuální počítač ve výchozím nastavení je vhodné toolock dolů portu RDP toohello přístupu na základě vzdálené podsítě. Hello následující příklad ukazuje způsob toogrant přístup toopublic virtuálních IP adres (VIP) některé rozsah toopermit přístupu pro protokol RDP. Jiné vzdálené IP je odepřen. Toto funguje, protože sítě seznamy ACL se dá nastavit pro koncový bod konkrétní virtuální počítač a ve výchozím nastavení je odepřen přístup.

**Příklad – více pravidel**

| **Pravidlo č** | **Vzdálené podsítě** | **Koncový bod** | **Povolení nebo odepření** |
| --- | --- | --- | --- |
| 100 |65.0.0.0/8 |3389 |Povolit |
| 200 |159.0.0.0/8 |3389 |Povolit |

### <a name="rule-order"></a>Pořadí pravidel
Vzhledem k tomu, že pro koncový bod lze zadat více pravidel, musí být tooorganize pravidla způsob, jak v pořadí toodetermine pravidlo, které mají přednost. pořadí pravidel Hello určuje prioritu. Sítě postupujte podle seznamů řízení přístupu *nejnižší má přednost před* pravidlo pořadí. V následující příklad hello, koncový bod hello na portu 80 selektivně udělí přístup tooonly určitých rozsahů IP adres. tooconfigure, budeme mít pravidlo odepření (pravidlo \# 100) pro adresy v prostoru 175.1.0.1/24 hello. Druhé pravidlo je potom zadat s prioritou, povolí přístup tooall další adresy v rámci 175.0.0.0/8 200.

**Příklad – pravidla priorit**

| **Pravidlo č** | **Vzdálené podsítě** | **Koncový bod** | **Povolení nebo odepření** |
| --- | --- | --- | --- |
| 100 |175.1.0.1/24 |80 |Odepřít |
| 200 |175.0.0.0/8 |80 |Povolit |

## <a name="network-acls-and-load-balanced-sets"></a>Seznamy ACL sítě a nastaví vyrovnáváním zatížení
Seznamy ACL sítě lze na sadu koncový bod Vyrovnávání zatížení. Pokud je seznam ACL zadaná pro skupinu s vyrovnáváním zatížení, hello síť seznamu ACL je použité tooall virtuálních počítačů v dané sadě skupinu s vyrovnáváním zatížení. Například pokud skupinu s vyrovnáváním zatížení je vytvořen "Port 80" a skupinu s vyrovnáváním zatížení hello obsahuje 3 virtuální počítače, hello sítě ACL vytvořen koncový bod "Port 80" jedné virtuální počítač bude automaticky platit toohello ostatních virtuálních počítačů.

![Seznamy ACL sítě a nastaví vyrovnáváním zatížení](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Další kroky
[Správa seznamů řízení přístupu pro koncové body pomocí prostředí PowerShell](virtual-networks-acl-powershell.md)

