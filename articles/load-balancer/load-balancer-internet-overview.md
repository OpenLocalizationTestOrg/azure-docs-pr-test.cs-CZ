---
title: "Přehled nástroje pro vyrovnávání zatížení Internetové | Microsoft Docs"
description: "Přehled pro internetový bod nástroj pro vyrovnávání zatížení a jejich funkce. Jak funguje nástroj pro vyrovnávání zatížení pro Azure pomocí virtuální počítače a cloudové služby."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 529b37aa-a45c-41d1-8877-fee8cc1fa375
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: c420b38fbe8054bc4b701f89ebc417677ca47a27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="internet-facing-load-balancer-overview"></a><span data-ttu-id="6b41d-104">Přehled nástroje pro vyrovnávání zatížení přístupných Internetu</span><span class="sxs-lookup"><span data-stu-id="6b41d-104">Internet facing load balancer overview</span></span>

<span data-ttu-id="6b41d-105">Nástroje pro vyrovnávání zatížení Azure mapuje veřejnou IP adresu a port číslo příchozí provoz privátní IP adresu a číslo portu virtuálního počítače a naopak přenosům odpověď z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6b41d-105">Azure load balancer maps the public IP address and port number of incoming traffic to the private IP address and port number of the virtual machine and vice versa for the response traffic from the virtual machine.</span></span> <span data-ttu-id="6b41d-106">Pravidla Vyrovnávání zatížení umožňují distribuovat specifické typy přenosů mezi několik virtuálních počítačů nebo služeb.</span><span class="sxs-lookup"><span data-stu-id="6b41d-106">Load balancing rules allow you to distribute specific types of traffic between multiple virtual machines or services.</span></span> <span data-ttu-id="6b41d-107">Můžete například šíří zatížení webový požadavek provoz napříč více webové servery nebo webové role.</span><span class="sxs-lookup"><span data-stu-id="6b41d-107">For example, you can spread the load of web request traffic across multiple web servers or web roles.</span></span>

<span data-ttu-id="6b41d-108">Pro cloudovou službu, která obsahuje instance webové role nebo rolí pracovního procesu můžete definovat veřejný koncový bod v souboru definice (.csdef) služby.</span><span class="sxs-lookup"><span data-stu-id="6b41d-108">For a cloud service that contains instances of web roles or worker roles, you can define a public endpoint in the service definition (.csdef) file.</span></span>

<span data-ttu-id="6b41d-109">*Servicedefinition.csdef* soubor obsahuje konfiguraci koncového bodu a až budete mít více instancí role pro nasazení role web nebo worker, instalační program pro něj bude nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6b41d-109">The *servicedefinition.csdef* file contains the endpoint configuration and when you have multiple role instances for a web or worker role deployment, the load balancer will be setup for it.</span></span> <span data-ttu-id="6b41d-110">Počet instancí v konfiguračním souboru služby (.csfg) mění způsob přidání instancí do cloudu nasazení.</span><span class="sxs-lookup"><span data-stu-id="6b41d-110">The way to add instances to your cloud deployment is changing the instance count on the service configuration file (.csfg).</span></span>

<span data-ttu-id="6b41d-111">Následující obrázek znázorňuje koncový bod Vyrovnávání zatížení sítě pro webový provoz, která je sdílena mezi tři virtuální počítače pro veřejné a privátní port TCP 80.</span><span class="sxs-lookup"><span data-stu-id="6b41d-111">The following figure shows a load-balanced endpoint for web traffic that is shared among three virtual machines for the public and private TCP port of 80.</span></span> <span data-ttu-id="6b41d-112">Tyto tři virtuální počítače jsou v sadě s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="6b41d-112">These three virtual machines are in a load-balanced set.</span></span>

![Příklad nástroje pro vyrovnávání zatížení veřejnou](./media/load-balancer-internet-overview/IC727496.png)

<span data-ttu-id="6b41d-114">Obrázek 1 – endpoint pro webový provoz s vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="6b41d-114">Figure 1 - Load-balanced endpoint for web traffic</span></span>

<span data-ttu-id="6b41d-115">Pokud internetoví klienti odesílat žádosti o webovou stránku na veřejnou IP adresu cloudové služby na portu TCP 80, Vyrovnávání zatížení Azure distribuuje požadavky mezi tři virtuální počítače v sadě s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="6b41d-115">When Internet clients send web page requests to the public IP address of the cloud service on TCP port 80, the Azure Load Balancer distributes the requests between the three virtual machines in the load-balanced set.</span></span> <span data-ttu-id="6b41d-116">Další informace o algoritmy Vyrovnávání zatížení, najdete v článku [stránku přehled nástroje pro vyrovnávání zatížení](load-balancer-overview.md#load-balancer-features).</span><span class="sxs-lookup"><span data-stu-id="6b41d-116">For more information about load balancer algorithms, see the [load balancer overview page](load-balancer-overview.md#load-balancer-features).</span></span>

<span data-ttu-id="6b41d-117">Ve výchozím nastavení nástroj pro vyrovnávání zatížení Azure distribuuje síťový provoz mezi více instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6b41d-117">By default, Azure Load Balancer distributes network traffic equally among multiple virtual machine instances.</span></span> <span data-ttu-id="6b41d-118">Můžete také nakonfigurovat spřažení relace, další informace najdete v tématu [režim distribuce nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md).</span><span class="sxs-lookup"><span data-stu-id="6b41d-118">You can also configure session affinity, For more information, see [load balancer distribution mode](load-balancer-distribution-mode.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b41d-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b41d-119">Next steps</span></span>

<span data-ttu-id="6b41d-120">Další informace o [nástroj pro vyrovnávání zatížení pro vnitřní](load-balancer-internal-overview.md) abyste lépe pochopili, které nástroj pro vyrovnávání zatížení je lepší přizpůsobení pro vaše nasazení cloudu.</span><span class="sxs-lookup"><span data-stu-id="6b41d-120">Learn about [Internal load balancer](load-balancer-internal-overview.md) to better understand which load balancer is a better fit for your cloud deployment.</span></span>

<span data-ttu-id="6b41d-121">Můžete také [začínáte s vytvářením internetovým nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) a konfigurace, jaký druh [režim distribuce](load-balancer-distribution-mode.md) konkrétní zatížení vyrovnávání síťové přenosy chování.</span><span class="sxs-lookup"><span data-stu-id="6b41d-121">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for an specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="6b41d-122">Pokud vaše aplikace potřebuje udržovat aktivní připojení serverů, které jsou za nástrojem pro vyrovnávání zatížení, můžete zjistit více o [nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="6b41d-122">If your application needs to keep connections alive for servers behind a load balancer, you can understand more about [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="6b41d-123">Pomůže vám to pochopit chování nečinného připojení při používání služby Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="6b41d-123">It will help to learn about idle connection behavior when you are using Azure Load Balancer.</span></span>
