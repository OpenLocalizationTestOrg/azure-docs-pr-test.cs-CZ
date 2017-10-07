---
title: "aaaWhat toodo v události hello narušení služby Azure, které mají vliv virtuálních sítí Azure | Microsoft Docs"
description: "Zjistěte, jaké toodo v události hello narušení služby Azure, které mají vliv virtuálních sítí Azure."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: db022d2a042d255cf8ec6afb68cd8436aeecfe08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-network--business-continuity"></a>Virtuální síť – kontinuita podnikových procesů
## <a name="overview"></a>Přehled
Virtuální síť (VNet) je logická reprezentace sítě v cloudu hello. Umožňuje vám toodefine vlastní privátní IP adresy místo a segment hello sítě do podsítě. Virtuální sítě slouží jako toohost hranice důvěryhodnosti svoje výpočetní prostředky, jako jsou virtuální počítače Azure a cloudových služeb (webové/role pracovního procesu). Virtuální síť, která umožňuje přímé privátní IP komunikaci mezi hello prostředky v ní umístěné. Virtuální síť může být také propojené tooan do místní sítě prostřednictvím jednoho z hello hybridní možnosti, jako je například brána sítě VPN nebo ExpressRoute.

Virtuální síť, která je vytvořena v rámci oboru hello oblasti. Virtuální sítě můžete vytvořit pomocí stejné adresní prostor ve dvou různých oblastech (tj. v oblasti USA – východ a oblasti USA – Západ, ale nemůže připojit, je tooone jiné přímo). 

## <a name="business-continuity"></a>Kontinuita podnikových procesů
Může dojít k několika různými způsoby, že vaše aplikace může být přerušeny. S daným regionem může být zcela zkrácené kvůli tooa přírodní katastrofě nebo částečné po havárii z důvodu selhání tooa více zařízení nebo služeb. Hello dopad na hello virtuální sítě služby se liší v každé z těchto situacích.

**Otázka: co můžete dělat v případě hello výpadku tooan celé oblasti? tj. Pokud oblast je zcela konečného kvůli tooa přírodní katastrofě? Co se stane toohello, který hostuje virtuální sítě v oblasti hello?**

Odpověď: hello virtuální sítě a hello zdroje v hello vliv oblast zůstane během hello čas přerušení služby hello nedostupné.

![Diagram jednoduché virtuální sítě](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**Otázka: co se dá toodo znovu vytvořit hello stejné virtuální síti v jiné oblasti?**

Odpověď: virtuální síť (VNet) je poměrně jednoduché prostředků. Můžete vyvolat rozhraní API Správce Azure toocreate virtuální síť s hello stejné adresní prostor v jiné oblasti. toore-vytvořit hello stejné prostředí, která byla součástí hello vliv oblast, je třeba toore volání rozhraní API toomake-nasazení služby Cloud Services (webové/role pracovního procesu) a virtuální počítače, které jste měli. Bude mít i toospin až brána sítě VPN a připojení tooyour do místní sítě, pokud jste měli místní připojení (například v hybridním nasazení).

Hello pokyny pro vytvoření virtuální sítě najdete [zde](virtual-networks-create-vnet-arm-pportal.md). 

**Otázka: je možné repliku virtuální sítě v dané oblasti se znovu vytvořit v jiné oblasti předem?**

Odpověď: Ano, můžete vytvořit dvě virtuální sítě pomocí hello stejnou privátní IP adresní prostor a prostředky ve dvou různých oblastech dříve času. Pokud zákazník byl hostování internetové služby v hello virtuální sítě, může mít nastavit Traffic Manager toogeo směrování provozu toohello oblast, která je aktivní. Zákazník se ale nemůže připojit dvě virtuální sítě s hello stejné adres místo tootheir do místní sítě jako by to způsobilo směrování problémy. V době hello havárii a ztrátě virtuální sítě v jedné oblasti, může zákazník připojit hello jiné virtuální sítě v hello dostupné oblasti s odpovídající adresy místo tootheir do místní sítě.

Hello pokyny pro vytvoření virtuální sítě najdete [zde](virtual-networks-create-vnet-arm-pportal.md).

