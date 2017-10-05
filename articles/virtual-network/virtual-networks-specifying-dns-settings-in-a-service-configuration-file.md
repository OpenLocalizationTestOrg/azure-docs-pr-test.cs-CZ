---
title: "Určení nastavení DNS v konfiguračním souboru služby | Microsoft Docs"
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
ms.openlocfilehash: 0fba2ea06827aff29a7a092933edb8120d668b29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a><span data-ttu-id="c89e3-103">Určení nastavení DNS v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="c89e3-103">Specifying DNS Settings in a Service Configuration File</span></span>
## <a name="dns-elements"></a><span data-ttu-id="c89e3-104">Elementy DNS</span><span class="sxs-lookup"><span data-stu-id="c89e3-104">DNS elements</span></span>
<span data-ttu-id="c89e3-105">Konfigurační soubor služby může obsahovat element DnsServers seznam adres IPv4 pro servery systému DNS (Domain Name), které budou používat službu.</span><span class="sxs-lookup"><span data-stu-id="c89e3-105">A service configuration file may contain a DnsServers element with a list of IPv4 addresses for the Domain Name System (DNS) servers that the service will use.</span></span> <span data-ttu-id="c89e3-106">Nastavení v konfiguračním souboru služby mají přednost před nastavením v konfiguračním souboru na síti.</span><span class="sxs-lookup"><span data-stu-id="c89e3-106">Settings in the service configuration file take precedence over settings in the network configuration file.</span></span> <span data-ttu-id="c89e3-107">Další informace najdete v tématu [schéma konfigurace služby Azure (.cscfg souboru)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="c89e3-107">For more information, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span></span>

<span data-ttu-id="c89e3-108">**NetworkConfiguration element**</span><span class="sxs-lookup"><span data-stu-id="c89e3-108">**NetworkConfiguration element**</span></span>

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> <span data-ttu-id="c89e3-109">**Název** atribut **Server_dns** element se používají pouze jako název odkazu.</span><span class="sxs-lookup"><span data-stu-id="c89e3-109">The **name** attribute in the **DnsServer** element is used only as a reference name.</span></span> <span data-ttu-id="c89e3-110">Nereprezentuje název hostitele pro DNS server.</span><span class="sxs-lookup"><span data-stu-id="c89e3-110">It does not represent the host name for the DNS server.</span></span> <span data-ttu-id="c89e3-111">Každý **Server_dns** hodnota atributu musí být jedinečný v rámci celé předplatné Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c89e3-111">Each **DnsServer** attribute value must be unique across the entire Microsoft Azure subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="c89e3-112">Viz také</span><span class="sxs-lookup"><span data-stu-id="c89e3-112">See Also</span></span>
[<span data-ttu-id="c89e3-113">Schéma konfigurace služby Azure (.cscfg)</span><span class="sxs-lookup"><span data-stu-id="c89e3-113">Azure Service Configuration Schema (.cscfg)</span></span>](https://msdn.microsoft.com/library/windowsazure/ee758710)

[<span data-ttu-id="c89e3-114">Schéma konfigurace virtuální sítě Azure</span><span class="sxs-lookup"><span data-stu-id="c89e3-114">Azure Virtual Network Configuration Schema</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

[<span data-ttu-id="c89e3-115">Konfigurace virtuální sítě pomocí konfiguračních souborů síť</span><span class="sxs-lookup"><span data-stu-id="c89e3-115">Configure a Virtual Network Using Network Configuration Files</span></span>](http://go.microsoft.com/fwlink/?LinkId=248094)

[<span data-ttu-id="c89e3-116">Informace o nastavení virtuální sítě v portálu pro správu</span><span class="sxs-lookup"><span data-stu-id="c89e3-116">About Virtual Network settings in the Management Portal</span></span>](http://go.microsoft.com/fwlink/?LinkId=248092)

