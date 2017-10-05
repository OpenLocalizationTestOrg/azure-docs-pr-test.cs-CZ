---
title: "Plánování virtuální sítě pro kolekci Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak chcete virtuální sítě pro kolekci Azure RemoteApp."
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
ms.openlocfilehash: 1eb8115b13fb18074b4c4726b69e3d9faf387c32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>Postup plánování virtuální sítě Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Tento dokument popisuje, jak nastavit virtuální sítě Azure (VNET) a podsíť pro Azure RemoteApp. Pokud jste obeznámeni s virtuální sítě Azure, je to funkci, která vám pomůže při virtualizaci infrastruktura vaší sítě do cloudu a vytvářet hybridní řešení s Azure a místních prostředků. Další informace si můžete přečíst [tady](../virtual-network/virtual-networks-overview.md).

Pokud chcete definovat zásady zabezpečení pro provozu (příchozí i odchozí) ve virtuální síti kterého nasazujete Azure RemoteApp, důrazně doporučujeme vytvořit samostatnou podsíť pro Azure RemoteApp od zbytku nasazeních ve službě Azure virtuální síť. Další informace o tom, jak definovat zásady zabezpečení na podsíť virtuální sítě Azure, přečtěte si prosím [co je skupina zabezpečení sítě (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Typy kolekce Azure Remoteappu s virtuální sítě Azure
Na následujících obrázcích zobrazit dvě možnosti jinou kolekci, pokud chcete používat virtuální síť.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure RemoteApp cloudové kolekce s virtuální sítě
 ![Azure RemoteApp - cloudové kolekce s virtuální sítě](./media/remoteapp-planvpn/ra-cloudvpn.png)

Reprezentuje kolekci Azure RemoteApp, kde jsou všechny prostředky, které relaci Remoteappu hostitelé potřebují přístup k nasazené v Azure. Mohou být ve stejné virtuální síti jako virtuální sítě vzdálené aplikace RemoteApp nebo jiný virtuální sítě v Azure.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Hybridní kolekce Azure RemoteApp s virtuální sítě
![Azure RemoteApp - hybridní kolekci s virtuální sítě](./media/remoteapp-planvpn/ra-hybridvpn.png)

Reprezentuje kolekci Azure RemoteApp, kde některé prostředky, které potřebují přístup k hostitele relace vzdálené aplikace RemoteApp jsou nasadit místně. Virtuální sítě vzdálené aplikace RemoteApp se propojí k místní síti pomocí technologie Azure hybridní jako site-to-site VPN nebo Express Route.

## <a name="how-the-system-works"></a>Jak funguje v systému
Azure RemoteApp skrytě nasadí virtuální počítače Azure (s nahrané image) do podsítě virtuální sítě, který jste si zvolili během zřizování. Pokud jste se rozhodli pro hybridní kolekce, pokusíme se přeložit plně kvalifikovaný název domény řadiče domény, které jste zadali v pracovním postupu zřizování s zadaný server DNS ve virtuální síti.  
Pokud se připojujete k existující virtuální síť, ujistěte se, zda je vystavit nezbytné porty ve skupinách zabezpečení sítě v Azure Remoteappu podsíť. 

Doporučujeme použít [dostatečně velké na podsíť pro Azure RemoteApp](remoteapp-vnetsizing.md). Největší nepodporuje Azure virtuální sítě je podsíť/8 (pomocí definice podsítě CIDR). Podsíť musí být dostatečně velký pro uložení všech Azure RemoteApp virtuálních počítačích během škálování při další uživatelé přistupují k aplikace. 

Toto je věcí, které budete muset povolit na podsíť virtuální sítě: 

1. Odchozí přenosy z podsítě má být povoleno na rozsah portů 10175 10101-Document ke komunikaci s jedním z interních služeb Azure RemoteApp.
2. Odchozí přenosy se má povolit z podsíť pro připojení k Azure Storage na portu 443
3. Pokud máte služby hostované v Azure Active Directory, ujistěte se, že žádné virtuální počítače v podsíti virtuální sítě pro Azure RemoteApp je možné se připojit k tomuto řadiči domény. DNS ve virtuální síti musí umět přeložit plně kvalifikovaný název domény tohoto řadiče domény.

## <a name="virtual-network-with-forced-tunneling"></a>Virtuální síť s vynucené tunelování
[Vynuceného tunelování](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) je nyní podporován pro všechny nové kolekce Azure Remoteappu. Aktuálně nepodporujeme migrace existující kolekci pro podporu vynucené tunelování.  Budete muset odstranit všechny existující kolekce pomocí virtuální sítě, které se připojujete k Azure Remoteappu a vytvořte novou získat vynucené tunelování na kolekce povolena. 

