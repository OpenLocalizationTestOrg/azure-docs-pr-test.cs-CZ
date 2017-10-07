---
title: "aaaInternet čelí přehled nástroje pro vyrovnávání zatížení | Microsoft Docs"
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
ms.openlocfilehash: 3514f945d69ec576ed256cdd01069491e3e43936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="internet-facing-load-balancer-overview"></a><span data-ttu-id="9bc2a-104">Přehled nástroje pro vyrovnávání zatížení přístupných Internetu</span><span class="sxs-lookup"><span data-stu-id="9bc2a-104">Internet facing load balancer overview</span></span>

<span data-ttu-id="9bc2a-105">Nástroje pro vyrovnávání zatížení Azure mapuje hello veřejnou IP adresu a číslo portu příchozí provoz toohello privátní IP adresu a číslo portu hello virtuálního počítače a naopak přenosům hello odpověď z hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-105">Azure load balancer maps hello public IP address and port number of incoming traffic toohello private IP address and port number of hello virtual machine and vice versa for hello response traffic from hello virtual machine.</span></span> <span data-ttu-id="9bc2a-106">Pravidla Vyrovnávání zatížení povolit toodistribute specifické typy přenosů mezi několik virtuálních počítačů nebo služeb.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-106">Load balancing rules allow you toodistribute specific types of traffic between multiple virtual machines or services.</span></span> <span data-ttu-id="9bc2a-107">Můžete například šíří hello zátěž webové žádosti o provozu mezi více webové servery nebo webové role.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-107">For example, you can spread hello load of web request traffic across multiple web servers or web roles.</span></span>

<span data-ttu-id="9bc2a-108">Pro cloudovou službu, která obsahuje instance webové role nebo rolí pracovního procesu můžete definovat veřejný koncový bod v souboru definice (.csdef) služby hello.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-108">For a cloud service that contains instances of web roles or worker roles, you can define a public endpoint in hello service definition (.csdef) file.</span></span>

<span data-ttu-id="9bc2a-109">Hello *servicedefinition.csdef* soubor obsahuje hello konfigurace koncového bodu a až budete mít více instancí role pro nasazení role web nebo worker, bude pro vyrovnávání zatížení hello nastavená.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-109">hello *servicedefinition.csdef* file contains hello endpoint configuration and when you have multiple role instances for a web or worker role deployment, hello load balancer will be setup for it.</span></span> <span data-ttu-id="9bc2a-110">nasazení cloudu Hello způsob tooadd instance tooyour mění hello počet instancí v konfiguračním souboru služby hello (.csfg).</span><span class="sxs-lookup"><span data-stu-id="9bc2a-110">hello way tooadd instances tooyour cloud deployment is changing hello instance count on hello service configuration file (.csfg).</span></span>

<span data-ttu-id="9bc2a-111">Hello následující obrázek znázorňuje koncový bod Vyrovnávání zatížení sítě pro webový provoz, která je sdílena mezi tři virtuální počítače pro hello veřejné a privátní port TCP 80.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-111">hello following figure shows a load-balanced endpoint for web traffic that is shared among three virtual machines for hello public and private TCP port of 80.</span></span> <span data-ttu-id="9bc2a-112">Tyto tři virtuální počítače jsou v sadě s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-112">These three virtual machines are in a load-balanced set.</span></span>

![Příklad nástroje pro vyrovnávání zatížení veřejnou](./media/load-balancer-internet-overview/IC727496.png)

<span data-ttu-id="9bc2a-114">Obrázek 1 – endpoint pro webový provoz s vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="9bc2a-114">Figure 1 - Load-balanced endpoint for web traffic</span></span>

<span data-ttu-id="9bc2a-115">Pokud internetoví klienti odesílat žádosti o webovou stránku toohello veřejnou IP adresu hello cloudové služby na portu TCP 80, distribuuje hello Vyrovnávání zatížení Azure hello požadavky mezi hello tři virtuální počítače v sadě hello Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-115">When Internet clients send web page requests toohello public IP address of hello cloud service on TCP port 80, hello Azure Load Balancer distributes hello requests between hello three virtual machines in hello load-balanced set.</span></span> <span data-ttu-id="9bc2a-116">Další informace o algoritmy Vyrovnávání zatížení, najdete v části hello [stránku přehled nástroje pro vyrovnávání zatížení](load-balancer-overview.md#load-balancer-features).</span><span class="sxs-lookup"><span data-stu-id="9bc2a-116">For more information about load balancer algorithms, see hello [load balancer overview page](load-balancer-overview.md#load-balancer-features).</span></span>

<span data-ttu-id="9bc2a-117">Ve výchozím nastavení nástroj pro vyrovnávání zatížení Azure distribuuje síťový provoz mezi více instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-117">By default, Azure Load Balancer distributes network traffic equally among multiple virtual machine instances.</span></span> <span data-ttu-id="9bc2a-118">Můžete také nakonfigurovat spřažení relace, další informace najdete v tématu [režim distribuce nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md).</span><span class="sxs-lookup"><span data-stu-id="9bc2a-118">You can also configure session affinity, For more information, see [load balancer distribution mode](load-balancer-distribution-mode.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9bc2a-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9bc2a-119">Next steps</span></span>

<span data-ttu-id="9bc2a-120">Další informace o [nástroj pro vyrovnávání zatížení pro vnitřní](load-balancer-internal-overview.md) toobetter pochopit, jaké nástroj pro vyrovnávání zatížení je lepší přizpůsobení pro vaše nasazení cloudu.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-120">Learn about [Internal load balancer](load-balancer-internal-overview.md) toobetter understand which load balancer is a better fit for your cloud deployment.</span></span>

<span data-ttu-id="9bc2a-121">Můžete také [začínáte s vytvářením internetovým nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) a konfigurace, jaký druh [režim distribuce](load-balancer-distribution-mode.md) konkrétní zatížení vyrovnávání síťové přenosy chování.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-121">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for an specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="9bc2a-122">Pokud aplikace potřebuje tookeep připojení zachování připojení pro servery za službou Vyrovnávání zatížení, můžete lépe porozumět o [nečinnosti nastavení časového limitu TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="9bc2a-122">If your application needs tookeep connections alive for servers behind a load balancer, you can understand more about [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="9bc2a-123">Pokud používáte nástroj pro vyrovnávání zatížení Azure ho pomůže toolearn o chování nečinné připojení.</span><span class="sxs-lookup"><span data-stu-id="9bc2a-123">It will help toolearn about idle connection behavior when you are using Azure Load Balancer.</span></span>
