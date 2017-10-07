---
title: "aaaList porty a adresy URL toowhitelist pro Azure RemoteApp nasazený ve virtuální síti zákazníka | Microsoft Docs"
description: "Další informace, které porty a adresy URL tooconfigure budete potřebovat pro komunikaci prostřednictvím Azure RemoteApp."
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
ms.openlocfilehash: 039866f7b64ac763ca833d66031ade3def1d3543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="list-of-ports-and-urls-toopermit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Seznam porty a adresy URL toopermit přístupu pro Azure RemoteApp nasazení u zákazníka virtuální sítě
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Pokud nasazujete kolekci Azure Remoteappu Cloudová nebo hybridní ve virtuální síti (VNET), přečtěte si následující informace o portu hello. Další informace o virtuálních sítí, přečtěte si [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md). Pokud jste vytvořili skupinu zabezpečení sítě (NSG) omezení provozu toohello virtuální síťové prostředky v kolekci, ujistěte se, že hello následující porty jsou dostupné a povolené prostřednictvím zásad zabezpečení hello ve virtuální síti hello. Další informace o skupinách zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě? (NSG) ](../virtual-network/virtual-networks-nsg.md).

## <a name="azure-remoteapp-subnet-needs-access-toothese-endpoints-and-urls"></a>Azure RemoteApp podsíť musí mít přístup toothese koncových bodů a adresy URL:
* *. servicebus.windows.net
* *. servicebus.net
* https://*.RemoteApp.windowsazure.com  
* https://www.RemoteApp.windowsazure.com 
* https://*RemoteApp.windowsazure.com  
* https://*.Core.Windows.NET  
* Odchozí: TCP: TCP: 443, 9351, 9352, 10175 10101-Document 
* Volitelné – UDP: 10201 10275  

## <a name="azure-remoteapp-clients-need-access-toothese-endpoints-and-urls"></a>Azure RemoteApp klienti potřebovat přístup k toothese koncových bodů a adresy URL:
Klienti, které I znamenat hello stolních počítačů, zařízení atd který lidé použití tooconnect toohello aplikace nasazené v hello kolekci Azure Remoteappu.

* https://telemetry.RemoteApp.windowsazure.com  
* https://*.RemoteApp.windowsazure.com (volitelné porty UDP hello jsou pro tuto adresu) 
* https://login.windows.net  
* https://login.microsoftonline.com  
* https://www.RemoteApp.windowsazure.com 
* https://*.Core.Windows.NET  
* Odchozí: TCP: 443  
* Volitelné - UDP: 3391 

