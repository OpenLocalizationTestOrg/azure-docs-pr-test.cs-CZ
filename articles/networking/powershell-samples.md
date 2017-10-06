---
title: "aaaAzure ukázky PowerShell | Microsoft Docs"
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
ms.openlocfilehash: 130a6e755691b46a9549cad5acaa5bde4fe38e95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples-for-networking"></a><span data-ttu-id="7e212-103">Ukázky pro sítě Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e212-103">Azure PowerShell Samples for networking</span></span>

<span data-ttu-id="7e212-104">Hello následující tabulka obsahuje odkazy tooscripts vytvořené pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7e212-104">hello following table includes links tooscripts built using Azure PowerShell.</span></span>

| | |
|-|-|
|<span data-ttu-id="7e212-105">**Připojení mezi prostředky Azure**</span><span class="sxs-lookup"><span data-stu-id="7e212-105">**Connectivity between Azure resources**</span></span>||
| [<span data-ttu-id="7e212-106">Vytvoření virtuální sítě pro vícevrstvé aplikace</span><span class="sxs-lookup"><span data-stu-id="7e212-106">Create a virtual network for multi-tier applications</span></span>](./scripts/virtual-network-powershell-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7e212-107">Vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="7e212-107">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="7e212-108">Podsíť front-end toohello provozu je omezená tooHTTP, při provozu toohello back-end podsíť je omezená tooSQL, port 1433.</span><span class="sxs-lookup"><span data-stu-id="7e212-108">Traffic toohello front-end subnet is limited tooHTTP, while traffic toohello back-end subnet is limited tooSQL, port 1433.</span></span> |
| [<span data-ttu-id="7e212-109">Peer dvě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="7e212-109">Peer two virtual networks</span></span>](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7e212-110">Vytvoří a připojí dvou virtuálních sítí v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="7e212-110">Creates and connects two virtual networks in hello same region.</span></span> |
| [<span data-ttu-id="7e212-111">Směrovat provoz prostřednictvím sítě virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="7e212-111">Route traffic through a network virtual appliance</span></span>](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7e212-112">Vytvoří virtuální síť s podsítí front-end a back-end a virtuální počítač, který je možné tooroute provoz mezi hello dvě podsítě.</span><span class="sxs-lookup"><span data-stu-id="7e212-112">Creates a virtual network with front-end and back-end subnets and a VM that is able tooroute traffic between hello two subnets.</span></span> |
| [<span data-ttu-id="7e212-113">Filtrovat příchozí a odchozí provoz sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="7e212-113">Filter inbound and outbound VM network traffic</span></span>](./scripts/virtual-network-powershell-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7e212-114">Vytvoří virtuální síť s podsítí front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="7e212-114">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="7e212-115">Příchozí síťový provoz toohello front-end podsíť je omezená tooHTTP a HTTPS...</span><span class="sxs-lookup"><span data-stu-id="7e212-115">Inbound network traffic toohello front-end subnet is limited tooHTTP and HTTPS..</span></span> <span data-ttu-id="7e212-116">Odchozí přenosy toohello Internetu z podsítě hello back-end není povoleno.</span><span class="sxs-lookup"><span data-stu-id="7e212-116">Outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> |
|<span data-ttu-id="7e212-117">**Načíst vyrovnávání a provoz směrem**</span><span class="sxs-lookup"><span data-stu-id="7e212-117">**Load balancing and traffic direction**</span></span>||
| [<span data-ttu-id="7e212-118">Načíst vyrovnávat přenosy tooVMs pro zajištění vysoké dostupnosti</span><span class="sxs-lookup"><span data-stu-id="7e212-118">Load balance traffic tooVMs for high availability</span></span>](./scripts/load-balancer-windows-powershell-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7e212-119">Vytvoří několik virtuálních počítačů v s vysokou dostupností a konfigurace skupinu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="7e212-119">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="7e212-120">Více webů na virtuálních počítačích můžete vyrovnávat zatížení</span><span class="sxs-lookup"><span data-stu-id="7e212-120">Load balance multiple websites on VMs</span></span>](./scripts/load-balancer-windows-powershell-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7e212-121">Vytvoří dva virtuální počítače s víc konfigurací IP adres, připojený k tooan Azure skupiny dostupnosti, přístupný prostřednictvím Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="7e212-121">Creates two VMs with multiple IP configurations, joined tooan Azure Availability Set, accessible through an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="7e212-122">Přímé provoz v několika oblastech aplikace vysokou dostupnost</span><span class="sxs-lookup"><span data-stu-id="7e212-122">Direct traffic across multiple regions for high application availability</span></span>](./scripts/traffic-manager-powershell-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  <span data-ttu-id="7e212-123">Vytvoří dvě plány služby app, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="7e212-123">Creates two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> |
| | |
