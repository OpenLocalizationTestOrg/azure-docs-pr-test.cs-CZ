---
title: "aaaSpecifying nastavení DNS v konfiguračním souboru služby | Microsoft Docs"
description: "určení vlastních nastavení DNS pro virtuální síť pomocí konfigurační soubor služby"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: jdial
ms.openlocfilehash: f192e33566dd8e669da04e6378a0c8e4b0b35ecc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Určení nastavení DNS v konfiguračním souboru služby
## <a name="dns-elements"></a>Elementy DNS
Konfigurační soubor služby může obsahovat element DnsServers seznam adres IPv4 pro servery hello systému DNS (Domain Name), které budou používat služby hello. Nastavení v konfiguračním souboru služby hello mají přednost před nastavením v konfiguračním souboru na hello sítě. Další informace najdete v tématu [schéma konfigurace služby Azure (.cscfg souboru)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> Hello **název** atribut v hello **Server_dns** element se používají pouze jako název odkazu. Nereprezentuje hello název hostitele pro hello DNS server. Každý **Server_dns** hodnota atributu musí být jedinečný v rámci celé předplatné Microsoft Azure hello.
> 
> 

## <a name="see-also"></a>Viz také
[Schéma konfigurace služby Azure (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Schéma konfigurace virtuální sítě Azure](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfigurace virtuální sítě pomocí konfiguračních souborů síť](http://go.microsoft.com/fwlink/?LinkId=248094)

[Informace o nastavení virtuální sítě v hello portálu pro správu](http://go.microsoft.com/fwlink/?LinkId=248092)

