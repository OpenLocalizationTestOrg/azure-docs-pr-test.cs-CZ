---
title: "aaaConnect tooAzure Data Lake Store z virtuální sítě | Microsoft Docs"
description: "Připojení z virtuální sítě Azure tooAzure Data Lake Store"
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: c695dcf49fe4e1a87a90729cf085a938f3b51fe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-azure-data-lake-store-from-vms-within-an-azure-vnet"></a>Přístup k Azure Data Lake Store z virtuálních počítačů v rámci síť Azure
Azure Data Lake Store je PaaS služba, která běží na veřejné internetové IP adresy. Jakýkoli server, zda se může připojit toohello veřejného Internetu může obvykle připojit koncové body Data Lake Store tooAzure také. Ve výchozím nastavení všechny virtuální počítače, které jsou v virtuálních sítí Azure přístup k hello Internetu a proto můžete přístup k Azure Data Lake Store. Je však možné tooconfigure virtuální počítače ve virtuální síti toonot mít toohello přístup k Internetu. Pro tyto virtuální počítače přístup k tooAzure Data Lake Store je omezená také. Blokování veřejný přístup k Internetu pro virtuální počítače ve virtuálních sítí Azure lze provést pomocí kteréhokoli hello následující postup.

* Podle konfigurace skupin zabezpečení sítě (NSG)
* Nakonfigurováním uživatele definované trasy (UDR)
* Při výměně tras přes protokol BGP (odvětví standardní protokol dynamického směrování) ExpressRoute je při použití tohoto bloku přístup toohello Internetu

V tomto článku se dozvíte, jak tooenable přistupovat k toohello Azure Data Lake Store z virtuálních počítačů Azure, které byly tooaccess s omezeným přístupem, které prostředky pomocí jedné ze tří metod hello uvedené výše.

## <a name="enabling-connectivity-tooazure-data-lake-store-from-vms-with-restricted-connectivity"></a>Povolení připojení tooAzure Data Lake Store z virtuálních počítačů s připojením s omezeným přístupem
tooaccess úložiště Azure Data Lake z těchto virtuálních počítačů, je nutné nakonfigurovat tooaccess hello IP adresu, kde je k dispozici hello účtu Azure Data Lake Store. Můžete identifikovat hello IP adresy pro vaše účty Data Lake Store pomocí překladu názvů DNS hello vaše účty (`<account>.azuredatalakestore.net`). To můžete použít nástroje, jako **nslookup**. Otevřete příkazový řádek ve vašem počítači a spusťte následující příkaz hello.

    nslookup mydatastore.azuredatalakestore.net

výstup Hello se podobá následující hello. Hello hodnoty s **adresu** vlastnost je hello IP adresu, která je spojená s vaším účtem Data Lake Store.

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a>Povolení připojení z virtuálních počítačů omezený pomocí NSG
Při použití pravidla NSG tooblock přístup toohello Internetu, potom můžete vytvořit další NSG, který umožňuje přístup toohello Data Lake Store IP adresu. Další informace o pravidla NSG je k dispozici na [co je skupina zabezpečení sítě?](../virtual-network/virtual-networks-nsg.md). Pokyny, jak zjistit, skupiny Nsg toocreate [jak hello skupiny Nsg toomanage pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a>Povolení připojení z virtuálních počítačů omezený pomocí UDR nebo ExpressRoute
Když jsou trasy, udr nebo trasy protokolu BGP vyměňují toohello použité tooblock přístup k Internetu, musí trasu speciální toobe nakonfigurovaná tak, aby virtuální počítače v těchto podsítích přístup koncových bodů Data Lake Store. Další informace najdete v tématu [co jsou trasy definované uživatelem?](../virtual-network/virtual-networks-udr-overview.md). Pokyny pro vytvoření udr, najdete v části [udr vytvořit ve službě Správce prostředků](../virtual-network/virtual-network-create-udr-arm-ps.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a>Povolení připojení z virtuálních počítačů omezený pomocí ExpressRoute
Když je nakonfigurovaná okruh ExpressRoute, hello na místní servery přistupovat k Data Lake Store prostřednictvím veřejného partnerského vztahu. Další informace o konfiguraci ExpressRoute pro veřejný partnerský vztah je k dispozici na [nejčastější dotazy k ExpressRoute](../expressroute/expressroute-faqs.md).

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
* [Zabezpečení dat uložených v Azure Data Lake Store](data-lake-store-security-overview.md)

