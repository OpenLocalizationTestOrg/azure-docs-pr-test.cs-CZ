---
title: "aaaOpen porty tooa virtuální počítač pomocí hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak tooopen port / create tooyour koncový bod virtuálního počítače s Windows v modelu nasazení resource manager hello hello portálu Azure"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: aba789c65254651899aa688f256fe616c3d0126d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-tooa-virtual-machine-with-hello-azure-portal"></a>Jak tooopen porty tooa virtuální počítač s hello portálu Azure
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Rychlé příkazy
Můžete také [proveďte tyto kroky, pomocí Azure PowerShell](nsg-quickstart-powershell.md).

Nejprve vytvořte skupinu zabezpečení vaší sítě. Vyberte skupinu prostředků hello portálu, zvolte **přidat**, vyhledejte a vyberte **skupinu zabezpečení sítě**:

![Přidat skupinu zabezpečení sítě](./media/nsg-quickstart-portal/add-nsg.png)

Zadejte název pro vaší skupinu zabezpečení sítě, vyberte nebo vytvořte skupinu prostředků a vyberte umístění. Vyberte **vytvořit** po dokončení:

![Vytvořit skupinu zabezpečení sítě](./media/nsg-quickstart-portal/create-nsg.png)

Vyberte novou skupinu zabezpečení sítě. Vyberte 'příchozí pravidla zabezpečení, a pak vyberte hello **přidat** tlačítko toocreate pravidlo:

![Přidat příchozí pravidlo](./media/nsg-quickstart-portal/add-inbound-rule.png)

Zvolte společného **služby** z rozevírací nabídky hello, jako například *HTTP*. Můžete také vybrat *vlastní* tooprovide toouse specifického portu. V případě potřeby změňte prioritu hello nebo název. Hello prioritu má vliv pořadí hello, ve kterém jsou použity pravidla - hello nižší hello číselnou hodnotu, hello starší hello pravidlo se použije. Můžete také vybrat **Upřesnit** hello horní části této obrazovky tooenter konkrétní zdroje rozsah bloku nebo portu IP, třeba. Až budete připravení, vyberte **OK** toocreate hello pravidlo:

![Vytvoření příchozího pravidla](./media/nsg-quickstart-portal/create-inbound-rule.png)

Posledním krokem je tooassociate skupiny zabezpečení sítě se podsíť nebo konkrétní síťové rozhraní. Umožňuje přidružit hello skupinu zabezpečení sítě s podsítí. Vyberte **podsítě**, zvolte **přidružit**:

![Přidružte skupinu zabezpečení sítě s podsítí](./media/nsg-quickstart-portal/associate-subnet.png)

Vyberte virtuální síť a pak vyberte příslušnou podsítí hello:

![Přidružení skupiny zabezpečení sítě pomocí virtuální sítě](./media/nsg-quickstart-portal/select-vnet-subnet.png)

Nyní jste vytvořili skupinu zabezpečení sítě, vytvořit vstupní pravidlo umožňující přenosy na portu 80 a spojené s podsítí. Všechny virtuální počítače připojit toothat podsítě jsou dostupné na portu 80.

## <a name="more-information-on-network-security-groups"></a>Další informace o skupinách zabezpečení sítě
Hello zde rychlé příkazy umožní tooget nahoru a spuštěna s průchodu tooyour přenosy virtuálních počítačů. Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu tooyour prostředky. Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Pro vysokou dostupnost webové aplikace měli byste umístit virtuální počítače za pro vyrovnávání zatížení Azure. Nástroj pro vyrovnávání zatížení Hello distribuuje provoz tooVMs ke skupině zabezpečení sítě, která poskytuje filtrování provozu. Další informace najdete v tématu [jak tooload vyrovnávání Linux virtuálního počítače v Azure toocreate vysoce dostupné aplikace](tutorial-load-balancer.md).

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vytvořili přenosem tooallow HTTP jednoduché pravidlo. Můžete najít informace o vytváření podrobnější prostředí v hello následující články:

* [Přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)
* [Co je skupina zabezpečení sítě (NSG)?](../../virtual-network/virtual-networks-nsg.md)