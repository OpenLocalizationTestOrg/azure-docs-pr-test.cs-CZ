---
title: "Seznam portů a adres URL do seznamu povolených IP adres pro Azure RemoteApp nasazený ve virtuální síti zákazníka | Microsoft Docs"
description: "Další informace, které porty a adresy URL budete muset nakonfigurovat pro komunikaci prostřednictvím Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: mghosh1616
manager: mbaldwin
ms.assetid: 5a001ff7-14c9-47fa-9b39-78fd5a5f0250
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c17ff8d5441ca92f7b893edb541a1e9730c2a847
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Seznam portů a adres URL, k povolení přístupu pro Azure RemoteApp nasazení u zákazníka virtuální sítě
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Pokud nasazujete kolekci Azure Remoteappu Cloudová nebo hybridní ve virtuální síti (VNET), přečtěte si následující informace o portu. Další informace o virtuálních sítí, přečtěte si [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md). Pokud jste vytvořili skupinu zabezpečení sítě (NSG) provoz směřující do virtuální síťové prostředky v kolekci, ujistěte se, že následující porty jsou dostupné a povolené prostřednictvím zásad zabezpečení ve virtuální síti. Další informace o skupinách zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě? (NSG) ](../virtual-network/virtual-networks-nsg.md).

## <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure RemoteApp podsíť potřebuje přístup k těchto koncových bodů a adresy URL:
* *. servicebus.windows.net
* *. servicebus.net
* https://*.RemoteApp.windowsazure.com  
* https://www.RemoteApp.windowsazure.com 
* https://*RemoteApp.windowsazure.com  
* https://*.Core.Windows.NET  
* Odchozí: TCP: TCP: 443, 9351, 9352, 10175 10101-Document 
* Volitelné – UDP: 10201 10275  

## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp klienti potřebovat přístup ke těchto koncových bodů a adresy URL:
Klienti I rozumí stolní počítače, zařízení atd který lidé použít pro připojení k aplikace nasazené v kolekci Azure RemoteApp.

* https://telemetry.RemoteApp.windowsazure.com  
* https://*.RemoteApp.windowsazure.com (volitelné porty UDP jsou pro tuto adresu) 
* https://login.windows.net  
* https://login.microsoftonline.com  
* https://www.RemoteApp.windowsazure.com 
* https://*.Core.Windows.NET  
* Odchozí: TCP: 443  
* Volitelné - UDP: 3391 

