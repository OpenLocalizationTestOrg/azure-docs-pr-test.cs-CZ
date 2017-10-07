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
# <a name="azure-cli-samples-for-networking"></a>Ukázek Azure CLI pro sítě

Hello následující tabulka obsahuje odkazy toobash skripty vyvíjené hello rozhraní příkazového řádku Azure.

| | |
|-|-|
|**Připojení mezi prostředky Azure**||
| [Vytvoření virtuální sítě pro vícevrstvé aplikace](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Vytvoří virtuální síť s podsítí front-end a back-end. Podsíť front-end toohello provozu je omezená tooHTTP a SSH, při provozu toohello back-end podsíť je omezená tooMySQL, port 3306. |
| [Peer dvě virtuální sítě](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Vytvoří a připojí dvou virtuálních sítí v hello stejné oblasti. |
| [Směrovat provoz prostřednictvím sítě virtuálního zařízení](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | Vytvoří virtuální síť s podsítí front-end a back-end a virtuální počítač, který je možné tooroute provoz mezi hello dvě podsítě. |
| [Filtrovat příchozí a odchozí provoz sítě virtuálních počítačů](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Vytvoří virtuální síť s podsítí front-end a back-end. Příchozí síťový provoz toohello front-end podsíť je omezená tooHTTP, HTTPS a SSH. Odchozí přenosy toohello Internetu z podsítě hello back-end není povoleno. |
|**Načíst vyrovnávání a provoz směrem**||
| [Načíst vyrovnávat přenosy tooVMs pro zajištění vysoké dostupnosti](./scripts/load-balancer-linux-cli-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | Vytvoří několik virtuálních počítačů v s vysokou dostupností a konfigurace skupinu s vyrovnáváním zatížení. |
| [Více webů na virtuálních počítačích můžete vyrovnávat zatížení](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Vytvoří dva virtuální počítače s víc konfigurací IP adres, připojený k tooan Azure skupiny dostupnosti, přístupný prostřednictvím Vyrovnávání zatížení Azure. |
| [Přímé provoz v několika oblastech aplikace vysokou dostupnost](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  Vytvoří dvě plány služby app, dva webové aplikace, profil správce provozu a dva koncové body správce provozu. |
| | |
