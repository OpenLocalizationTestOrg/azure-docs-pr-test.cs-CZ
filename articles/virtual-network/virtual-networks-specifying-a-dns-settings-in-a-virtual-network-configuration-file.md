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
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a><span data-ttu-id="1470b-103">Zadání nastavení DNS v konfiguračním souboru virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="1470b-103">Specifying DNS settings in a virtual network configuration file</span></span>
<span data-ttu-id="1470b-104">Soubor konfigurace sítě má dva elementy, které můžete použít nastavení systému DNS (Domain Name) toospecify: **DnsServers** a **DnsServerRef**.</span><span class="sxs-lookup"><span data-stu-id="1470b-104">A network configuration file has two elements that you can use toospecify Domain Name System (DNS) settings: **DnsServers** and **DnsServerRef**.</span></span> <span data-ttu-id="1470b-105">Můžete přidat seznam serverů DNS zadáním jejich IP adresy a odkazovat na názvy toohello **DnsServers** element.</span><span class="sxs-lookup"><span data-stu-id="1470b-105">You can add a list of DNS servers by specifying their IP addresses and reference names toohello **DnsServers** element.</span></span> <span data-ttu-id="1470b-106">Pak můžete použít **DnsServerRef** toospecify elementu, které položky serveru DNS z prvku DnsServers hello se používají pro jiné síťové lokality v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1470b-106">You can then use a **DnsServerRef** element toospecify which DNS server entries from hello DnsServers element are used for different network sites within your virtual network.</span></span>

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="1470b-107">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="1470b-107">This article covers hello classic deployment model.</span></span>

<span data-ttu-id="1470b-108">Hello sítě konfigurační soubor může obsahovat hello následující prvky.</span><span class="sxs-lookup"><span data-stu-id="1470b-108">hello network configuration file may contain hello following elements.</span></span> <span data-ttu-id="1470b-109">Název Hello jednotlivých prvků je propojené tooa stránka, která poskytuje další informace o elementu hello nastavení hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1470b-109">hello title of each element is linked tooa page that provides additional information about hello element value settings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1470b-110">Informace o tom, jak tooconfigure hello sítě konfigurační soubor najdete v tématu [konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="1470b-110">For information about how tooconfigure hello network configuration file, see [Configure a Virtual Network Using a Network Configuration File](virtual-networks-using-network-configuration-file.md).</span></span> <span data-ttu-id="1470b-111">Informace o jednotlivých prvků obsažené v konfiguračním souboru na hello sítě najdete v tématu [Azure schéma konfigurace virtuální sítě](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="1470b-111">For information about each element contained in hello network configuration file, see [Azure Virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
> 
> 

[<span data-ttu-id="1470b-112">DNS Element</span><span class="sxs-lookup"><span data-stu-id="1470b-112">Dns Element</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> <span data-ttu-id="1470b-113">Hello **název** atribut v hello **Server_dns** element se používají pouze jako odkaz pro hello **DnsServerRef** element.</span><span class="sxs-lookup"><span data-stu-id="1470b-113">hello **name** attribute in hello **DnsServer** element is used only as a reference for hello **DnsServerRef** element.</span></span> <span data-ttu-id="1470b-114">Nereprezentuje hello název hostitele pro hello DNS server.</span><span class="sxs-lookup"><span data-stu-id="1470b-114">It does not represent hello host name for hello DNS server.</span></span> <span data-ttu-id="1470b-115">Každý **Server_dns** hodnota atributu musí být jedinečný v rámci celé předplatné Microsoft Azure hello</span><span class="sxs-lookup"><span data-stu-id="1470b-115">Each **DnsServer** attribute value must be unique across hello entire Microsoft Azure subscription</span></span>
> 
> 

[<span data-ttu-id="1470b-116">Element lokality virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="1470b-116">Virtual Network Sites Element</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> <span data-ttu-id="1470b-117">V pořadí toospecify toto nastavení pro element hello virtuální síťové lokality, musí být dříve definován v elementu DNS hello.</span><span class="sxs-lookup"><span data-stu-id="1470b-117">In order toospecify this setting for hello Virtual Network Sites element, it must be previously defined in hello DNS element.</span></span> <span data-ttu-id="1470b-118">Hello DnsServerRef *název* v hello virtuální síťové weby musí odkazovat element tooa název hodnota zadaná v elementu hello DNS pro server DNS *název*.</span><span class="sxs-lookup"><span data-stu-id="1470b-118">hello DnsServerRef *name* in hello Virtual Network Sites element must refer tooa name value specified in hello DNS element for DnsServer *name*.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="1470b-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1470b-119">Next steps</span></span>
* <span data-ttu-id="1470b-120">Pochopení hello [Azure schéma konfigurace virtuální sítě](http://go.microsoft.com/fwlink/?LinkId=248093).</span><span class="sxs-lookup"><span data-stu-id="1470b-120">Understand hello [Azure Virtual Network Configuration Schema](http://go.microsoft.com/fwlink/?LinkId=248093).</span></span>
* <span data-ttu-id="1470b-121">Pochopení hello [schéma konfigurace služby Azure](https://msdn.microsoft.com/library/windowsazure/ee758710).</span><span class="sxs-lookup"><span data-stu-id="1470b-121">Understand hello [Azure Service Configuration Schema](https://msdn.microsoft.com/library/windowsazure/ee758710).</span></span>
* <span data-ttu-id="1470b-122">[Konfigurace virtuální sítě pomocí konfiguračních souborů síť](virtual-networks-using-network-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="1470b-122">[Configure a virtual network using Network configuration files](virtual-networks-using-network-configuration-file.md).</span></span>

