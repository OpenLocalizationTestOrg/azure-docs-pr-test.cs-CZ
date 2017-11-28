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
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a><span data-ttu-id="d7fa0-103">Určení nastavení DNS v konfiguračním souboru služby</span><span class="sxs-lookup"><span data-stu-id="d7fa0-103">Specifying DNS Settings in a Service Configuration File</span></span>
## <a name="dns-elements"></a><span data-ttu-id="d7fa0-104">Elementy DNS</span><span class="sxs-lookup"><span data-stu-id="d7fa0-104">DNS elements</span></span>
<span data-ttu-id="d7fa0-105">Konfigurační soubor služby může obsahovat element DnsServers seznam adres IPv4 pro servery hello systému DNS (Domain Name), které budou používat služby hello.</span><span class="sxs-lookup"><span data-stu-id="d7fa0-105">A service configuration file may contain a DnsServers element with a list of IPv4 addresses for hello Domain Name System (DNS) servers that hello service will use.</span></span> <span data-ttu-id="d7fa0-106">Nastavení v konfiguračním souboru služby hello mají přednost před nastavením v konfiguračním souboru na hello sítě.</span><span class="sxs-lookup"><span data-stu-id="d7fa0-106">Settings in hello service configuration file take precedence over settings in hello network configuration file.</span></span> <span data-ttu-id="d7fa0-107">Další informace najdete v tématu [schéma konfigurace služby Azure (.cscfg souboru)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7fa0-107">For more information, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span></span>

<span data-ttu-id="d7fa0-108">**NetworkConfiguration element**</span><span class="sxs-lookup"><span data-stu-id="d7fa0-108">**NetworkConfiguration element**</span></span>

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> <span data-ttu-id="d7fa0-109">Hello **název** atribut v hello **Server_dns** element se používají pouze jako název odkazu.</span><span class="sxs-lookup"><span data-stu-id="d7fa0-109">hello **name** attribute in hello **DnsServer** element is used only as a reference name.</span></span> <span data-ttu-id="d7fa0-110">Nereprezentuje hello název hostitele pro hello DNS server.</span><span class="sxs-lookup"><span data-stu-id="d7fa0-110">It does not represent hello host name for hello DNS server.</span></span> <span data-ttu-id="d7fa0-111">Každý **Server_dns** hodnota atributu musí být jedinečný v rámci celé předplatné Microsoft Azure hello.</span><span class="sxs-lookup"><span data-stu-id="d7fa0-111">Each **DnsServer** attribute value must be unique across hello entire Microsoft Azure subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="d7fa0-112">Viz také</span><span class="sxs-lookup"><span data-stu-id="d7fa0-112">See Also</span></span>
[<span data-ttu-id="d7fa0-113">Schéma konfigurace služby Azure (.cscfg)</span><span class="sxs-lookup"><span data-stu-id="d7fa0-113">Azure Service Configuration Schema (.cscfg)</span></span>](https://msdn.microsoft.com/library/windowsazure/ee758710)

[<span data-ttu-id="d7fa0-114">Schéma konfigurace virtuální sítě Azure</span><span class="sxs-lookup"><span data-stu-id="d7fa0-114">Azure Virtual Network Configuration Schema</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

[<span data-ttu-id="d7fa0-115">Konfigurace virtuální sítě pomocí konfiguračních souborů síť</span><span class="sxs-lookup"><span data-stu-id="d7fa0-115">Configure a Virtual Network Using Network Configuration Files</span></span>](http://go.microsoft.com/fwlink/?LinkId=248094)

[<span data-ttu-id="d7fa0-116">Informace o nastavení virtuální sítě v hello portálu pro správu</span><span class="sxs-lookup"><span data-stu-id="d7fa0-116">About Virtual Network settings in hello Management Portal</span></span>](http://go.microsoft.com/fwlink/?LinkId=248092)

