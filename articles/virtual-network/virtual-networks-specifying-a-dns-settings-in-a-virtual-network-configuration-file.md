---
title: "aaaSpecifying nastavení DNS v konfiguračním souboru virtuální sítě | Microsoft Docs"
description: "Jak v modelu nasazení classic hello soubor toochange nastavení serveru DNS ve virtuální síti pomocí konfigurace virtuální sítě"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a8905927-92ac-42b5-8c33-8e42c000692c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.openlocfilehash: d53a658773e1c930b5a28a701db0be9edd26565e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Zadání nastavení DNS v konfiguračním souboru virtuální sítě
Soubor konfigurace sítě má dva elementy, které můžete použít nastavení systému DNS (Domain Name) toospecify: **DnsServers** a **DnsServerRef**. Můžete přidat seznam serverů DNS zadáním jejich IP adresy a odkazovat na názvy toohello **DnsServers** element. Pak můžete použít **DnsServerRef** toospecify elementu, které položky serveru DNS z prvku DnsServers hello se používají pro jiné síťové lokality v rámci virtuální sítě.

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení classic hello.

Hello sítě konfigurační soubor může obsahovat hello následující prvky. Název Hello jednotlivých prvků je propojené tooa stránka, která poskytuje další informace o elementu hello nastavení hodnoty.

> [!IMPORTANT]
> Informace o tom, jak tooconfigure hello sítě konfigurační soubor najdete v tématu [konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md). Informace o jednotlivých prvků obsažené v konfiguračním souboru na hello sítě najdete v tématu [Azure schéma konfigurace virtuální sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx).
> 
> 

[DNS Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> Hello **název** atribut v hello **Server_dns** element se používají pouze jako odkaz pro hello **DnsServerRef** element. Nereprezentuje hello název hostitele pro hello DNS server. Každý **Server_dns** hodnota atributu musí být jedinečný v rámci celé předplatné Microsoft Azure hello
> 
> 

[Element lokality virtuální sítě](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> V pořadí toospecify toto nastavení pro element hello virtuální síťové lokality, musí být dříve definován v elementu DNS hello. Hello DnsServerRef *název* v hello virtuální síťové weby musí odkazovat element tooa název hodnota zadaná v elementu hello DNS pro server DNS *název*.
> 
> 

## <a name="next-steps"></a>Další kroky
* Pochopení hello [Azure schéma konfigurace virtuální sítě](http://go.microsoft.com/fwlink/?LinkId=248093).
* Pochopení hello [schéma konfigurace služby Azure](https://msdn.microsoft.com/library/windowsazure/ee758710).
* [Konfigurace virtuální sítě pomocí konfiguračních souborů síť](virtual-networks-using-network-configuration-file.md).

