---
title: "aaaAzure ukázky rozhraní příkazového řádku | Microsoft Docs"
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
ms.openlocfilehash: 8001b7e72480cfd0122325f7fb81c32aaad072d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-networking"></a><span data-ttu-id="4c3c5-103">Ukázek Azure CLI pro sítě</span><span class="sxs-lookup"><span data-stu-id="4c3c5-103">Azure CLI Samples for networking</span></span>

<span data-ttu-id="4c3c5-104">Hello následující tabulka obsahuje odkazy toobash skripty vyvíjené hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-104">hello following table includes links toobash scripts built using hello Azure CLI.</span></span>

| | |
|-|-|
|<span data-ttu-id="4c3c5-105">**Připojení mezi prostředky Azure**</span><span class="sxs-lookup"><span data-stu-id="4c3c5-105">**Connectivity between Azure resources**</span></span>||
| [<span data-ttu-id="4c3c5-106">Vytvoření virtuální sítě pro vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="4c3c5-106">Create a virtual network for multi-tier applications</span></span>](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="4c3c5-107">Vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-107">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="4c3c5-108">Podsíť front-end toohello provozu je omezená tooHTTP a SSH, při provozu toohello back-end podsíť je omezená tooMySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-108">Traffic toohello front-end subnet is limited tooHTTP and SSH, while traffic toohello back-end subnet is limited tooMySQL, port 3306.</span></span> |
| [<span data-ttu-id="4c3c5-109">Peer dvě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="4c3c5-109">Peer two virtual networks</span></span>](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="4c3c5-110">Vytvoří a připojí dvou virtuálních sítí v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-110">Creates and connects two virtual networks in hello same region.</span></span> |
| [<span data-ttu-id="4c3c5-111">Směrovat provoz prostřednictvím sítě virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="4c3c5-111">Route traffic through a network virtual appliance</span></span>](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="4c3c5-112">Vytvoří virtuální síť s podsítí front-end a back-end a virtuální počítač, který je možné tooroute provoz mezi hello dvě podsítě.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-112">Creates a virtual network with front-end and back-end subnets and a VM that is able tooroute traffic between hello two subnets.</span></span> |
| [<span data-ttu-id="4c3c5-113">Filtrovat příchozí a odchozí provoz sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4c3c5-113">Filter inbound and outbound VM network traffic</span></span>](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="4c3c5-114">Vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-114">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="4c3c5-115">Příchozí síťový provoz toohello front-end podsíť je omezená tooHTTP, HTTPS a SSH.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-115">Inbound network traffic toohello front-end subnet is limited tooHTTP, HTTPS and SSH.</span></span> <span data-ttu-id="4c3c5-116">Odchozí přenosy toohello Internetu z podsítě hello back-end není povoleno.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-116">Outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> |
|<span data-ttu-id="4c3c5-117">**Načíst vyrovnávání a provoz směrem**</span><span class="sxs-lookup"><span data-stu-id="4c3c5-117">**Load balancing and traffic direction**</span></span>||
| [<span data-ttu-id="4c3c5-118">Načíst vyrovnávat přenosy tooVMs pro zajištění vysoké dostupnosti</span><span class="sxs-lookup"><span data-stu-id="4c3c5-118">Load balance traffic tooVMs for high availability</span></span>](./scripts/load-balancer-linux-cli-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="4c3c5-119">Vytvoří několik virtuálních počítačů v s vysokou dostupností a konfigurace skupinu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-119">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="4c3c5-120">Více webů na virtuálních počítačích můžete vyrovnávat zatížení</span><span class="sxs-lookup"><span data-stu-id="4c3c5-120">Load balance multiple websites on VMs</span></span>](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="4c3c5-121">Vytvoří dva virtuální počítače s víc konfigurací IP adres, připojený k tooan Azure skupiny dostupnosti, přístupný prostřednictvím Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-121">Creates two VMs with multiple IP configurations, joined tooan Azure Availability Set, accessible through an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="4c3c5-122">Přímé provoz v několika oblastech aplikace vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="4c3c5-122">Direct traffic across multiple regions for high application availability</span></span>](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  <span data-ttu-id="4c3c5-123">Vytvoří dvě plány služby app, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="4c3c5-123">Creates two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> |
| | |
