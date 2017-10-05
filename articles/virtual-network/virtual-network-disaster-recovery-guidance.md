---
title: "Co dělat v případě přerušení služby Azure služby virtuálních sítí Azure, které mají vliv | Microsoft Docs"
description: "Zjistěte, co dělat v případě přerušení služby Azure služby virtuálních sítí Azure, které mají vliv."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: 4e125406d2e798138c45e3fbbf61a610afab69fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="virtual-network--business-continuity"></a><span data-ttu-id="23e1d-103">Virtuální síť – kontinuita podnikových procesů</span><span class="sxs-lookup"><span data-stu-id="23e1d-103">Virtual Network – Business Continuity</span></span>
## <a name="overview"></a><span data-ttu-id="23e1d-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="23e1d-104">Overview</span></span>
<span data-ttu-id="23e1d-105">Virtuální síť (VNet) je logická reprezentace sítě v cloudu.</span><span class="sxs-lookup"><span data-stu-id="23e1d-105">A Virtual Network (VNet) is a logical representation of your network in the cloud.</span></span> <span data-ttu-id="23e1d-106">Umožňuje definovat vlastní prostor privátní IP adresy a segmentovat do podsítí v síti.</span><span class="sxs-lookup"><span data-stu-id="23e1d-106">It allows you to define your own private IP address space and segment the network into subnets.</span></span> <span data-ttu-id="23e1d-107">Virtuální sítě slouží jako hranice vztahu důvěryhodnosti pro hostování svoje výpočetní prostředky, jako jsou virtuální počítače Azure a cloudových služeb (webové/role pracovního procesu).</span><span class="sxs-lookup"><span data-stu-id="23e1d-107">VNets serves as a trust boundary to host your compute resources such as Azure Virtual Machines and Cloud Services (web/worker roles).</span></span> <span data-ttu-id="23e1d-108">Virtuální síť, která umožňuje přímé privátní IP komunikaci mezi prostředky v ní umístěné.</span><span class="sxs-lookup"><span data-stu-id="23e1d-108">A VNet allows direct private IP communication between the resources hosted in it.</span></span> <span data-ttu-id="23e1d-109">Virtuální sítě lze také propojit k místní síti prostřednictvím jednoho z hybridní možnosti, jako je brána sítě VPN nebo ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="23e1d-109">A Virtual Network can also be linked to an on-premises network through one of the hybrid options such as a VPN Gateway or ExpressRoute.</span></span>

<span data-ttu-id="23e1d-110">Virtuální síť, která je vytvořena v rámci oboru oblasti.</span><span class="sxs-lookup"><span data-stu-id="23e1d-110">A VNet is created within the scope of a region.</span></span> <span data-ttu-id="23e1d-111">Virtuální sítě můžete vytvořit pomocí stejné adresní prostor ve dvou různých oblastech (tj. v oblasti USA – východ a oblasti USA – Západ, ale nemůže připojit je k sobě navzájem přímo).</span><span class="sxs-lookup"><span data-stu-id="23e1d-111">You can create VNets with same address space in two different regions (i.e. US East and US West but cannot connect them to one another directly).</span></span> 

## <a name="business-continuity"></a><span data-ttu-id="23e1d-112">Kontinuita podnikových procesů</span><span class="sxs-lookup"><span data-stu-id="23e1d-112">Business Continuity</span></span>
<span data-ttu-id="23e1d-113">Může dojít k několika různými způsoby, že vaše aplikace může být přerušeny.</span><span class="sxs-lookup"><span data-stu-id="23e1d-113">There could be several different ways that your application could be disrupted.</span></span> <span data-ttu-id="23e1d-114">S daným regionem může být zcela zkrácené z důvodu přírodní katastrofě nebo částečné po havárii z důvodu chyby více zařízení nebo služeb.</span><span class="sxs-lookup"><span data-stu-id="23e1d-114">A given region could be completely cut off due to a natural disaster or a partial disaster due to a failure of multiple devices/services.</span></span> <span data-ttu-id="23e1d-115">Dopad na službu virtuální sítě se liší v každé z těchto situacích.</span><span class="sxs-lookup"><span data-stu-id="23e1d-115">The impact on the VNet service is different in each of these situations.</span></span>

<span data-ttu-id="23e1d-116">**Otázka: co můžete dělat v případě výpadku pro celou oblast? tj. Pokud oblast je zcela konečného kvůli přírodní katastrofě? Co se stane k virtuálním sítím, které jsou hostované v oblasti?**</span><span class="sxs-lookup"><span data-stu-id="23e1d-116">**Q: What do you do in the event of an outage to an entire region? i.e. if a region is completely cutoff due to a natural disaster? What happens to the Virtual Networks hosted in the region?**</span></span>

<span data-ttu-id="23e1d-117">Odpověď: virtuální sítě a prostředky v oblasti ovlivněných nepřístupná během doby přerušení služby.</span><span class="sxs-lookup"><span data-stu-id="23e1d-117">A: The Virtual Network and the resources in the affected region remains inaccessible during the time of the service disruption.</span></span>

![Diagram jednoduché virtuální sítě](./media/virtual-network-disaster-recovery-guidance/vnet.png)

<span data-ttu-id="23e1d-119">**Otázka: co se dá znovu vytvořit v jiné oblasti stejnou virtuální síť?**</span><span class="sxs-lookup"><span data-stu-id="23e1d-119">**Q: What can I to do re-create the same Virtual Network in a different region?**</span></span>

<span data-ttu-id="23e1d-120">Odpověď: virtuální síť (VNet) je poměrně jednoduché prostředků.</span><span class="sxs-lookup"><span data-stu-id="23e1d-120">A: Virtual Network (VNet) is fairly lightweight resource.</span></span> <span data-ttu-id="23e1d-121">Můžete volat rozhraní API služby Azure k vytvoření virtuální sítě s stejné adresní prostor v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="23e1d-121">You can invoke Azure APIs to create a VNet with the same address space in a different region.</span></span> <span data-ttu-id="23e1d-122">Znovu vytvořit stejné prostředí, ve kterém se nacházel v ovlivněných oblasti, budete muset provádět volání rozhraní API služby Cloud Services (webové/role pracovního procesu) a virtuální počítače, které jste měli znovu nasadit.</span><span class="sxs-lookup"><span data-stu-id="23e1d-122">To re-create the same environment that was present in the affected region, you have to make API calls to re-deploy your Cloud Services (web/worker roles) and Virtual Machines that you had.</span></span> <span data-ttu-id="23e1d-123">Bude také muset číselníku bránu VPN a připojit k síti na pracovišti, pokud jste měli místní připojení (například v hybridním nasazení).</span><span class="sxs-lookup"><span data-stu-id="23e1d-123">You will also have to spin up a VPN Gateway and connect to your on-premises network if you had on-premises connectivity (such as in a hybrid deployment).</span></span>

<span data-ttu-id="23e1d-124">Pokyny pro vytvoření virtuální sítě najdete [zde](virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="23e1d-124">The instructions for creating a VNet are found [here](virtual-networks-create-vnet-arm-pportal.md).</span></span> 

<span data-ttu-id="23e1d-125">**Otázka: je možné repliku virtuální sítě v dané oblasti se znovu vytvořit v jiné oblasti předem?**</span><span class="sxs-lookup"><span data-stu-id="23e1d-125">**Q: Can a replica of a VNet in a given region be re-created in another region ahead of time?**</span></span>

<span data-ttu-id="23e1d-126">Odpověď: Ano, můžete vytvořit dvě virtuální sítě pomocí stejné prostor privátní IP adresy a prostředky ve dvou různých oblastech předem.</span><span class="sxs-lookup"><span data-stu-id="23e1d-126">A: Yes, you can create two VNets using the same private IP address space and resources in two different regions ahead of time.</span></span> <span data-ttu-id="23e1d-127">Pokud zákazník byl hostování internetové služby ve virtuální síti, se může nastavit až Traffic Manageru můžete provoz směrovat geografické oblasti, která je aktivní.</span><span class="sxs-lookup"><span data-stu-id="23e1d-127">If a customer was hosting internet facing services in the VNet, they could have set up Traffic Manager to geo-route traffic to the region that is active.</span></span> <span data-ttu-id="23e1d-128">Zákazník však nemůže spojit dvě virtuální sítě se stejné adresního prostoru svojí místní síti jako by to způsobilo směrování problémy.</span><span class="sxs-lookup"><span data-stu-id="23e1d-128">However, a customer cannot connect two VNets with the same address space to their on-premises network as it would cause routing issues.</span></span> <span data-ttu-id="23e1d-129">V době havárie a ztrátě virtuální sítě v jedné oblasti může zákazník připojit další virtuální síť v oblasti k dispozici s odpovídajícím adresního prostoru svojí místní síti.</span><span class="sxs-lookup"><span data-stu-id="23e1d-129">At the time of a disaster and loss of a VNet in one region, a customer can connect the other VNet in the available region with matching address space to their on-premises network.</span></span>

<span data-ttu-id="23e1d-130">Pokyny pro vytvoření virtuální sítě najdete [zde](virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="23e1d-130">The instructions for creating a VNet are found [here](virtual-networks-create-vnet-arm-pportal.md).</span></span>

