---
title: "Zadání nastavení DNS v konfiguračním souboru virtuální sítě | Microsoft Docs"
description: "Postup změny nastavení serveru DNS ve virtuální síti pomocí konfiguračního souboru virtuální síť v klasickém modelu nasazení"
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
ms.openlocfilehash: ec33268915a1888509834ce6a5b2bc782a12ce4a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a><span data-ttu-id="1b572-103">Zadání nastavení DNS v konfiguračním souboru virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="1b572-103">Specifying DNS settings in a virtual network configuration file</span></span>
<span data-ttu-id="1b572-104">Soubor konfigurace sítě má dva elementy, které můžete použít k určení nastavení systému DNS (Domain Name): **DnsServers** a **DnsServerRef**.</span><span class="sxs-lookup"><span data-stu-id="1b572-104">A network configuration file has two elements that you can use to specify Domain Name System (DNS) settings: **DnsServers** and **DnsServerRef**.</span></span> <span data-ttu-id="1b572-105">Můžete přidat seznam serverů DNS zadáním jejich IP adresy a odkazovat na názvy **DnsServers** element.</span><span class="sxs-lookup"><span data-stu-id="1b572-105">You can add a list of DNS servers by specifying their IP addresses and reference names to the **DnsServers** element.</span></span> <span data-ttu-id="1b572-106">Pak můžete použít **DnsServerRef** elementu, který chcete určit, které položky serveru DNS z elementu DnsServers budou použity pro jiné síťové lokality v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1b572-106">You can then use a **DnsServerRef** element to specify which DNS server entries from the DnsServers element are used for different network sites within your virtual network.</span></span>

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="1b572-107">Tento článek se týká modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="1b572-107">This article covers the classic deployment model.</span></span>

<span data-ttu-id="1b572-108">Soubor konfigurace sítě může obsahovat následující prvky.</span><span class="sxs-lookup"><span data-stu-id="1b572-108">The network configuration file may contain the following elements.</span></span> <span data-ttu-id="1b572-109">Název každého prvku je propojený na stránku, která poskytuje další informace o nastavení hodnoty elementu.</span><span class="sxs-lookup"><span data-stu-id="1b572-109">The title of each element is linked to a page that provides additional information about the element value settings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1b572-110">Informace o tom, jak nakonfigurovat konfiguračního souboru sítě najdete v tématu [konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="1b572-110">For information about how to configure the network configuration file, see [Configure a Virtual Network Using a Network Configuration File](virtual-networks-using-network-configuration-file.md).</span></span> <span data-ttu-id="1b572-111">Informace o jednotlivých prvků obsažené v souboru konfigurace sítě najdete v tématu [Azure schéma konfigurace virtuální sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b572-111">For information about each element contained in the network configuration file, see [Azure Virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
> 
> 

[<span data-ttu-id="1b572-112">DNS Element</span><span class="sxs-lookup"><span data-stu-id="1b572-112">Dns Element</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> <span data-ttu-id="1b572-113">**Název** atribut **Server_dns** element se používají pouze jako odkaz pro **DnsServerRef** element.</span><span class="sxs-lookup"><span data-stu-id="1b572-113">The **name** attribute in the **DnsServer** element is used only as a reference for the **DnsServerRef** element.</span></span> <span data-ttu-id="1b572-114">Nereprezentuje název hostitele pro DNS server.</span><span class="sxs-lookup"><span data-stu-id="1b572-114">It does not represent the host name for the DNS server.</span></span> <span data-ttu-id="1b572-115">Každý **Server_dns** hodnota atributu musí být jedinečný v rámci celé předplatné Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="1b572-115">Each **DnsServer** attribute value must be unique across the entire Microsoft Azure subscription</span></span>
> 
> 

[<span data-ttu-id="1b572-116">Element lokality virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="1b572-116">Virtual Network Sites Element</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> <span data-ttu-id="1b572-117">Chcete-li toto nastavení pro element virtuální sítě, se musí být dříve definovaný v elementu DNS.</span><span class="sxs-lookup"><span data-stu-id="1b572-117">In order to specify this setting for the Virtual Network Sites element, it must be previously defined in the DNS element.</span></span> <span data-ttu-id="1b572-118">DnsServerRef *název* v virtuálních síťových webů element musí odkazovat na název hodnota zadaný v elementu DNS pro server DNS *název*.</span><span class="sxs-lookup"><span data-stu-id="1b572-118">The DnsServerRef *name* in the Virtual Network Sites element must refer to a name value specified in the DNS element for DnsServer *name*.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="1b572-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b572-119">Next steps</span></span>
* <span data-ttu-id="1b572-120">Pochopení [schéma konfigurace virtuální sítě Azure](http://go.microsoft.com/fwlink/?LinkId=248093).</span><span class="sxs-lookup"><span data-stu-id="1b572-120">Understand the [Azure Virtual Network Configuration Schema](http://go.microsoft.com/fwlink/?LinkId=248093).</span></span>
* <span data-ttu-id="1b572-121">Pochopení [schéma konfigurace Azure Service](https://msdn.microsoft.com/library/windowsazure/ee758710).</span><span class="sxs-lookup"><span data-stu-id="1b572-121">Understand the [Azure Service Configuration Schema](https://msdn.microsoft.com/library/windowsazure/ee758710).</span></span>
* <span data-ttu-id="1b572-122">[Konfigurace virtuální sítě pomocí konfiguračních souborů síť](virtual-networks-using-network-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="1b572-122">[Configure a virtual network using Network configuration files](virtual-networks-using-network-configuration-file.md).</span></span>

