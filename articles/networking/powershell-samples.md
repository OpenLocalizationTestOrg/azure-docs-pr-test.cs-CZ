---
title: "Ukázky Azure PowerShell | Microsoft Docs"
description: "Ukázky Azure PowerShell"
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/24/2017
ms.author: georgewallace
ms.openlocfilehash: 0bca4fb6874bd265f0ae9faeb4219abeb4ffb6d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples-for-networking"></a><span data-ttu-id="ac8de-103">Ukázky pro sítě Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac8de-103">Azure PowerShell Samples for networking</span></span>

<span data-ttu-id="ac8de-104">Následující tabulka obsahuje odkazy na skripty, které jsou vytvořené pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ac8de-104">The following table includes links to scripts built using Azure PowerShell.</span></span>

| | |
|-|-|
|<span data-ttu-id="ac8de-105">**Připojení mezi prostředky Azure**</span><span class="sxs-lookup"><span data-stu-id="ac8de-105">**Connectivity between Azure resources**</span></span>||
| [<span data-ttu-id="ac8de-106">Vytvoření virtuální sítě pro vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="ac8de-106">Create a virtual network for multi-tier applications</span></span>](./scripts/virtual-network-powershell-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="ac8de-107">Vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="ac8de-107">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="ac8de-108">Provoz do front-endu podsítě je omezený na protokolu HTTP, zatímco provozu do podsítě back-end je omezený na SQL, portu 1433.</span><span class="sxs-lookup"><span data-stu-id="ac8de-108">Traffic to the front-end subnet is limited to HTTP, while traffic to the back-end subnet is limited to SQL, port 1433.</span></span> |
| [<span data-ttu-id="ac8de-109">Peer dvě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="ac8de-109">Peer two virtual networks</span></span>](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="ac8de-110">Vytvoří a připojí dvou virtuálních sítí ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="ac8de-110">Creates and connects two virtual networks in the same region.</span></span> |
| [<span data-ttu-id="ac8de-111">Směrovat provoz prostřednictvím sítě virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="ac8de-111">Route traffic through a network virtual appliance</span></span>](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="ac8de-112">Vytvoří virtuální síť s podsítí front-end a back-end a virtuální počítač, který je schopen směrovat provoz mezi dvěma podsítěmi.</span><span class="sxs-lookup"><span data-stu-id="ac8de-112">Creates a virtual network with front-end and back-end subnets and a VM that is able to route traffic between the two subnets.</span></span> |
| [<span data-ttu-id="ac8de-113">Filtrovat příchozí a odchozí provoz sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ac8de-113">Filter inbound and outbound VM network traffic</span></span>](./scripts/virtual-network-powershell-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="ac8de-114">Vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="ac8de-114">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="ac8de-115">Příchozí síťový provoz do front-endu podsítí je omezený na protokolu HTTP a HTTPS...</span><span class="sxs-lookup"><span data-stu-id="ac8de-115">Inbound network traffic to the front-end subnet is limited to HTTP and HTTPS..</span></span> <span data-ttu-id="ac8de-116">Odchozí přenosy k Internetu z podsítě back-end není povoleno.</span><span class="sxs-lookup"><span data-stu-id="ac8de-116">Outbound traffic to the Internet from the back-end subnet is not permitted.</span></span> |
|<span data-ttu-id="ac8de-117">**Načíst vyrovnávání a provoz směrem**</span><span class="sxs-lookup"><span data-stu-id="ac8de-117">**Load balancing and traffic direction**</span></span>||
| [<span data-ttu-id="ac8de-118">Přenosy Vyrovnávání zatížení pro virtuální počítače pro vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="ac8de-118">Load balance traffic to VMs for high availability</span></span>](./scripts/load-balancer-windows-powershell-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="ac8de-119">Vytvoří několik virtuálních počítačů v s vysokou dostupností a konfigurace skupinu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="ac8de-119">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="ac8de-120">Více webů na virtuálních počítačích můžete vyrovnávat zatížení</span><span class="sxs-lookup"><span data-stu-id="ac8de-120">Load balance multiple websites on VMs</span></span>](./scripts/load-balancer-windows-powershell-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="ac8de-121">Vytvoří dva virtuální počítače s víc konfigurací IP adres, připojený k Azure skupiny dostupnosti, přístupný prostřednictvím Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="ac8de-121">Creates two VMs with multiple IP configurations, joined to an Azure Availability Set, accessible through an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="ac8de-122">Přímé provoz v několika oblastech aplikace vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="ac8de-122">Direct traffic across multiple regions for high application availability</span></span>](./scripts/traffic-manager-powershell-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  <span data-ttu-id="ac8de-123">Vytvoří dvě plány služby app, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="ac8de-123">Creates two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> |
| | |
