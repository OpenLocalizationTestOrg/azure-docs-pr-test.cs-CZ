---
title: "aaaNetwork mapování mezi dvěma oblastí Azure ve službě Azure Site Recovery | Microsoft Docs"
description: "Azure Site Recovery koordinuje hello replikace, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Další informace o převzetí služeb při selhání tooAzure nebo do sekundárního datacentra."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 4f80c44e3f94eaf446bc01a7041d91fe34aa78d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-mapping-between-two-azure-regions"></a>Mapování sítě mezi dvěma oblastmi Azure


Tento článek popisuje, jak toomap Azure virtuální sítě dvou oblastí Azure mezi sebou. Mapování sítě zajišťuje, když replikovaného virtuálního počítače je vytvořen v cílové hello oblast Azure, je vytvořený na hello virtuální síť, která je sítě namapované toovirtual hello zdrojového virtuálního počítače.  

## <a name="prerequisites"></a>Požadavky
Předtím, než je mapovat sítě, ujistěte se, jste vytvořili [virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md) v obou zdroje a cíle oblastech Azure.

## <a name="map-networks"></a>Mapování sítě

toomap virtuální síť Azure v jedné oblasti Azure tooanother virtuální sítě v jiné oblasti, přejděte tooSite infrastruktura Recovery -> mapování sítě (pro virtuální počítače Azure) a vytvořit mapování sítě.

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


Následující příklad virtuální počítač běží ve východní Asie oblasti a je v hello replikují tooSoutheast Asie.

Vyberte hello zdrojová a Cílová síť a pak klikněte na tlačítko OK toocreate mapování sítě z východní Asie tooSoutheast Asie.

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Hello stejnou věc toocreate mapování sítě z jihovýchodní Asie tooEast Asie.  
![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a>Mapování sítě při povolení replikace

Pokud mapování sítě není provést, pokud jsou replikace virtuálního počítače pro hello první čas formuláře jedné oblasti Azure tooanother, pak můžete vybrat cílovou síť jako součást hello stejný postup. Site Recovery vytvoří mapování sítě z oblasti tootarget oblast zdroje a cílová oblast toosource oblast založené na tomto výběru.   

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Ve výchozím nastavení, Site Recovery vytvoří síť v hello cílová oblast, která je stejná toohello zdrojové síti a přidáním '-automatické obnovení systému, jako název toohello příponu hello zdroje sítě. Kliknutím na tlačítko Přizpůsobit můžete již vytvořené síťové.

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


Pokud již probíhá hello mapování sítě, nelze změnit hello cílová virtuální síť při povolení replikace. toochange, upravte existující mapování sítě.  

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Pokud změníte mapování sítě z oblasti-1 tooregion-2, nezapomeňte že upravit mapování sítě hello z oblasti 2 tooregion-1 také.
>
>


## <a name="subnet-selection"></a>Výběr podsítě
Podsíť hello cílového virtuálního počítače je vybrána na základě názvu hello hello podsítě hello zdrojového virtuálního počítače. Pokud podsíť hello je stejný jako u hello zdrojového virtuálního počítače k dispozici v cílové síti hello, název a který je vybrán pro hello cílového virtuálního počítače. Pokud neexistuje žádná podsíť s hello stejný název v hello cílové síti, pak abecedně první podsíť je vybrána jako hello cílové podsíti. Tato podsíť můžete upravit tak, že přejdete tooCompute a nastavení sítě hello virtuálního počítače.

![Upravit podsíť](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP adresa

IP adresa pro každý hello síťového rozhraní hello cílového virtuálního počítače je zvolen následujícím způsobem:

### <a name="dhcp"></a>DHCP
Síťové rozhraní hello hello zdrojového virtuálního počítače používá protokol DHCP, pak síťové rozhraní hello cílový virtuální počítač je také nastavena jako DHCP.

### <a name="static-ip"></a>Statická IP adresa
Pokud síťové rozhraní hello hello zdrojový virtuální počítač používá statickou IP adresu, pak síťové rozhraní hello cílový virtuální počítač je také nastavit toouse statickou IP adresu. Statická IP adresa je zvolen následujícím způsobem:

#### <a name="same-address-space"></a>Stejné adresní prostor

Pokud hello hello zdroj podsíť a podsíť hello cíl stejný adresní prostor, cílová IP adresa je nastavena stejné jako IP hello hello síťového rozhraní hello zdrojového virtuálního počítače. Pokud není k dispozici stejnou IP Adresou, některé dostupnou IP adresu nastavena jako hello cílová IP adresa.

#### <a name="different-address-space"></a>Jiným adresním prostorem

Pokud hello zdroj podsíť a podsíť cíl hello jiným adresním prostorem, cílová IP adresa je nastaven jako dostupnou IP adresu v hello cílové podsíti.

Hello cílová IP adresa na každé rozhraní sítě můžete upravit tak, že přejdete tooCompute a nastavení sítě hello virtuálního počítače.

## <a name="next-steps"></a>Další kroky

- Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md).
