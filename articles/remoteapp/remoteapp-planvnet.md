---
title: "aaaHow tooplan virtuální sítě pro kolekci Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak tooplan virtuální sítě pro kolekci Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: mghosh1616
manager: mbaldwin
ms.assetid: ad9aff0e-f374-49c0-951d-4a7be1c36de0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: d7eeefc3c66815b18f9338e2e428585e6f81a12a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooplan-your-virtual-network-for-azure-remoteapp"></a>Jak tooplan virtuální sítě pro Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Tento dokument popisuje, jak tooset virtuální sítě Azure (VNET) a hello podsíť pro Azure RemoteApp. Pokud jste obeznámeni s virtuální sítě Azure, je to funkci, která pomáhá vám toovirtualize síťové infrastruktury toohello cloudu a toocreate hybridní řešení s Azure a místních prostředků. Další informace si můžete přečíst [tady](../virtual-network/virtual-networks-overview.md).

Pokud chcete zásady zabezpečení toodefine provozu (příchozí i odchozí) ve virtuální síti kterého nasazujete Azure RemoteApp, důrazně doporučujeme vytvořit samostatnou podsíť pro Azure RemoteApp hello ostatních nasazení v hello Azure virtuální síť. Další informace o tom, jak ve vaší virtuální Azure toodefine zásady zabezpečení sítě podsíť, přečtěte si prosím [co je skupina zabezpečení sítě (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Typy kolekce Azure Remoteappu s virtuální sítě Azure
Hello následující grafiky zobrazte hello dvě možnosti jinou kolekci při toouse virtuální sítě.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure RemoteApp cloudové kolekce s virtuální sítě
 ![Azure RemoteApp - cloudové kolekce s virtuální sítě](./media/remoteapp-planvpn/ra-cloudvpn.png)

Reprezentuje kolekci Azure RemoteApp, kde jsou všechny prostředky hello, hostitele relace vzdálené aplikace RemoteApp hello nutné tooaccess nasazené v Azure. Mohou být v hello stejnou virtuální síť jako hello virtuální sítě vzdálené aplikace RemoteApp nebo jiný virtuální sítě v Azure.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Hybridní kolekce Azure RemoteApp s virtuální sítě
![Azure RemoteApp - hybridní kolekci s virtuální sítě](./media/remoteapp-planvpn/ra-hybridvpn.png)

Reprezentuje kolekci Azure RemoteApp, kde některé hello prostředky, třeba hostitele relace vzdálené aplikace RemoteApp hello tooaccess jsou nasadit místně. Hello virtuální sítě vzdálené aplikace RemoteApp je propojené toohello místní síti pomocí technologie Azure hybridní jako site-to-site VPN nebo Express Route.

## <a name="how-hello-system-works"></a>Jak funguje hello systému
V části hello zahrnuje nasadí Azure RemoteApp podsíť virtuální sítě toohello virtuální počítače Azure (s vaší nahraný obrázek), který jste si zvolili během zřizování. Pokud jste se rozhodli pro hybridní kolekce, pokusíme tooresolve hello plně kvalifikovaný název domény řadiče domény hello, které jste zadali do hello zřizování pracovního postupu s hello server DNS poskytovaný ve virtuální síti hello.  
Pokud se připojujete tooan existující virtuální síť, ujistěte se, že tooexpose hello nezbytné porty ve skupinách zabezpečení sítě v Azure Remoteappu podsíť. 

Doporučujeme použít [dostatečně velké na podsíť pro Azure RemoteApp](remoteapp-vnetsizing.md). podsíť/8 je Hello největší podporovaná Azure virtuální sítí (pomocí definice podsítě CIDR). Podsíť musí být dostatečně velký tooaccommodate všech hello Azure RemoteApp virtuálních počítačů během škálování při další uživatelé přistupují k aplikacím hello. 

Následují hello věcí, které budete potřebovat tooenable na podsíť virtuální sítě: 

1. Odchozí přenosy z podsítě hello má být povoleno na toocommunicate 10175 10101-Document rozsah portů s jedním z interních služeb Azure RemoteApp hello.
2. Odchozí přenosy, které má být povoleno z vaší podsítě tooconnect tooAzure úložiště na portu 443
3. Pokud máte služby hostované v Azure Active Directory, zajistěte, aby že žádné virtuální počítače v podsíti virtuální sítě hello pro Azure RemoteApp je řadič domény nemůže tooconnect toothat. Hello DNS ve virtuální síti hello by měl být schopný tooresolve hello plně kvalifikovaný název domény tohoto řadiče domény.

## <a name="virtual-network-with-forced-tunneling"></a>Virtuální síť s vynucené tunelování
[Vynuceného tunelování](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) je nyní podporován pro všechny nové kolekce Azure Remoteappu. Aktuálně nepodporujeme hello migrace existující kolekci toosupport vynuceného tunelování.  Toodelete budou mít všechny existující kolekce pomocí hello virtuální sítě jsou propojení tooAzure vzdálené aplikace RemoteApp a vytvořit nové jeden tooget vynuceného tunelování na kolekce povolena. 

