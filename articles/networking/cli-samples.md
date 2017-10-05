---
title: "Ukázky rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Ukázky rozhraní příkazového řádku Azure"
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: kumud
ms.openlocfilehash: 7977460f61bfdabd399e45e86d9bbf2e5083992b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cli-samples-for-networking"></a><span data-ttu-id="e3f3a-103">Ukázek Azure CLI pro sítě</span><span class="sxs-lookup"><span data-stu-id="e3f3a-103">Azure CLI Samples for networking</span></span>

<span data-ttu-id="e3f3a-104">Následující tabulka obsahuje odkazy na bash skripty, které jsou vytvořené pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-104">The following table includes links to bash scripts built using the Azure CLI.</span></span>

| | |
|-|-|
|<span data-ttu-id="e3f3a-105">**Připojení mezi prostředky Azure**</span><span class="sxs-lookup"><span data-stu-id="e3f3a-105">**Connectivity between Azure resources**</span></span>||
| [<span data-ttu-id="e3f3a-106">Vytvoření virtuální sítě pro vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="e3f3a-106">Create a virtual network for multi-tier applications</span></span>](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="e3f3a-107">Vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-107">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="e3f3a-108">Provoz do front-endu podsítě je omezen na protokolu HTTP a SSH, zatímco provozu do podsítě back-end je omezený na MySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-108">Traffic to the front-end subnet is limited to HTTP and SSH, while traffic to the back-end subnet is limited to MySQL, port 3306.</span></span> |
| [<span data-ttu-id="e3f3a-109">Peer dvě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="e3f3a-109">Peer two virtual networks</span></span>](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="e3f3a-110">Vytvoří a připojí dvou virtuálních sítí ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-110">Creates and connects two virtual networks in the same region.</span></span> |
| [<span data-ttu-id="e3f3a-111">Směrovat provoz prostřednictvím sítě virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="e3f3a-111">Route traffic through a network virtual appliance</span></span>](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="e3f3a-112">Vytvoří virtuální síť s podsítí front-end a back-end a virtuální počítač, který je schopen směrovat provoz mezi dvěma podsítěmi.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-112">Creates a virtual network with front-end and back-end subnets and a VM that is able to route traffic between the two subnets.</span></span> |
| [<span data-ttu-id="e3f3a-113">Filtrovat příchozí a odchozí provoz sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="e3f3a-113">Filter inbound and outbound VM network traffic</span></span>](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="e3f3a-114">Vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-114">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="e3f3a-115">Příchozí síťový provoz do front-endu podsítí je omezený na protokolu HTTP, HTTPS a SSH.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-115">Inbound network traffic to the front-end subnet is limited to HTTP, HTTPS and SSH.</span></span> <span data-ttu-id="e3f3a-116">Odchozí přenosy k Internetu z podsítě back-end není povoleno.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-116">Outbound traffic to the Internet from the back-end subnet is not permitted.</span></span> |
|<span data-ttu-id="e3f3a-117">**Načíst vyrovnávání a provoz směrem**</span><span class="sxs-lookup"><span data-stu-id="e3f3a-117">**Load balancing and traffic direction**</span></span>||
| [<span data-ttu-id="e3f3a-118">Přenosy Vyrovnávání zatížení pro virtuální počítače pro vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="e3f3a-118">Load balance traffic to VMs for high availability</span></span>](./scripts/load-balancer-linux-cli-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="e3f3a-119">Vytvoří několik virtuálních počítačů v s vysokou dostupností a konfigurace skupinu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-119">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="e3f3a-120">Více webů na virtuálních počítačích můžete vyrovnávat zatížení</span><span class="sxs-lookup"><span data-stu-id="e3f3a-120">Load balance multiple websites on VMs</span></span>](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="e3f3a-121">Vytvoří dva virtuální počítače s víc konfigurací IP adres, připojený k Azure skupiny dostupnosti, přístupný prostřednictvím Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-121">Creates two VMs with multiple IP configurations, joined to an Azure Availability Set, accessible through an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="e3f3a-122">Přímé provoz v několika oblastech aplikace vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="e3f3a-122">Direct traffic across multiple regions for high application availability</span></span>](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  <span data-ttu-id="e3f3a-123">Vytvoří dvě plány služby app, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="e3f3a-123">Creates two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> |
| | |
