---
title: "aaaAzure podporu správce prostředků nástroje pro vyrovnávání zatížení | Microsoft Docs"
description: "Pomocí prostředí powershell pro službu Vyrovnávání zatížení s Azure Resource Manager. Pomocí šablony pro vyrovnávání zatížení"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: d0394f11-ee5a-4407-9d86-79c936297265
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3c02d9382de00fefe6af8f4f8a2b7b62b344f285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a><span data-ttu-id="47923-104">Podporu správce prostředků Azure pomocí nástroje pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="47923-104">Using Azure Resource Manager Support with Azure Load Balancer</span></span>

<span data-ttu-id="47923-105">Azure Resource Manager je architektura hello upřednostňované správy pro služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="47923-105">Azure Resource Manager is hello preferred management framework for services in Azure.</span></span> <span data-ttu-id="47923-106">Azure nástroj pro vyrovnávání zatížení můžete spravovat pomocí rozhraní API založené na Azure Resource Manager a nástroje.</span><span class="sxs-lookup"><span data-stu-id="47923-106">Azure Load Balancer can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="concepts"></a><span data-ttu-id="47923-107">Koncepty</span><span class="sxs-lookup"><span data-stu-id="47923-107">Concepts</span></span>

<span data-ttu-id="47923-108">S Resource Managerem se nástroj pro vyrovnávání zatížení Azure obsahuje hello následující podřízené prostředky:</span><span class="sxs-lookup"><span data-stu-id="47923-108">With Resource Manager, Azure Load Balancer contains hello following child resources:</span></span>

* <span data-ttu-id="47923-109">Konfiguraci front-end IP adresy – nástroj pro vyrovnávání zatížení může obsahovat jednu nebo více adres IP front-endu známé jako virtuální IP adresy (VIP).</span><span class="sxs-lookup"><span data-stu-id="47923-109">Front-end IP configuration – a Load balancer can include one or more front end IP addresses, otherwise known as a virtual IPs (VIPs).</span></span> <span data-ttu-id="47923-110">Tyto IP adresy sloužit jako příchozího provozu hello.</span><span class="sxs-lookup"><span data-stu-id="47923-110">These IP addresses serve as ingress for hello traffic.</span></span>
* <span data-ttu-id="47923-111">Fond adres back-end – Toto jsou IP adresy přidružené hello virtuálního počítače budou distribuována toowhich zatížení sítě rozhraní karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="47923-111">Back-end address pool – these are IP addresses associated with hello virtual machine Network Interface Card (NIC) toowhich load will be distributed.</span></span>
* <span data-ttu-id="47923-112">Vyrovnávání zatížení pravidla – vlastnost rule mapuje danou front-end IP a portu kombinace tooa sadu back-end IP adresy a portu kombinaci.</span><span class="sxs-lookup"><span data-stu-id="47923-112">Load balancing rules – a rule property maps a given front end IP and port combination tooa set of back end IP addresses and port combination.</span></span> <span data-ttu-id="47923-113">Nástroj pro vyrovnávání zatížení může mít několik pravidel vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="47923-113">A single load balancer can have multiple load balancing rules.</span></span> <span data-ttu-id="47923-114">Každé pravidlo je kombinací front-endové IP adresy a portu a back-endové IP adresy a portu přidružených k virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="47923-114">Each rule is a combination of a front-end IP and port and back-end IP and port associated with VMs.</span></span>
* <span data-ttu-id="47923-115">Sondy – povolit sondy jste tookeep sledovat hello stav instancí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="47923-115">Probes – probes enable you tookeep track of hello health of VM instances.</span></span> <span data-ttu-id="47923-116">Pokud selže test stavu, instance virtuálního počítače hello se provedou mimo otočení automaticky.</span><span class="sxs-lookup"><span data-stu-id="47923-116">If a health probe fails, hello VM instance will be taken out of rotation automatically.</span></span>
* <span data-ttu-id="47923-117">Příchozích pravidel NAT pravidla – definování pravidel NAT hello příchozí provoz v IP front-endu hello a distribuované toohello zpět koncovou IP.</span><span class="sxs-lookup"><span data-stu-id="47923-117">Inbound NAT rules – NAT rules defining hello inbound traffic flowing through hello front end IP and distributed toohello back end IP.</span></span>

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a><span data-ttu-id="47923-118">Šablony Rychlý start</span><span class="sxs-lookup"><span data-stu-id="47923-118">Quickstart templates</span></span>

<span data-ttu-id="47923-119">Azure Resource Manager můžete tooprovision vaší aplikace s použitím deklarativní šablony.</span><span class="sxs-lookup"><span data-stu-id="47923-119">Azure Resource Manager allows you tooprovision your applications using a declarative template.</span></span> <span data-ttu-id="47923-120">S jednou šablonou můžete nasadit několik služeb společně s jejich závislostmi.</span><span class="sxs-lookup"><span data-stu-id="47923-120">In a single template, you can deploy multiple services along with their dependencies.</span></span> <span data-ttu-id="47923-121">Použít hello stejné šablony toorepeatedly nasazení aplikace během každé fáze životního cyklu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="47923-121">You use hello same template toorepeatedly deploy your application during every stage of hello application lifecycle.</span></span>

<span data-ttu-id="47923-122">Šablony může obsahovat definice pro virtuální počítače, virtuální sítě, skupiny dostupnosti, síťových rozhraní (NIC), účty úložiště, nástroje pro vyrovnávání zatížení, skupiny zabezpečení sítě a veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="47923-122">Templates can include definitions for Virtual Machines, Virtual Networks, Availability Sets, Network Interfaces (NICs), Storage Accounts, Load Balancers, Network Security Groups, and Public IPs.</span></span> <span data-ttu-id="47923-123">Pomocí šablony můžete vytvářet vše potřebné pro komplexní aplikace.</span><span class="sxs-lookup"><span data-stu-id="47923-123">With templates you can create everything you need for a complex application.</span></span> <span data-ttu-id="47923-124">soubor šablony Hello lze zkontrolovat do systému správy obsahu pro verzí a spolupráci.</span><span class="sxs-lookup"><span data-stu-id="47923-124">hello template file can be checked into content management system for version control and collaboration.</span></span>

[<span data-ttu-id="47923-125">Další informace o šablonách</span><span class="sxs-lookup"><span data-stu-id="47923-125">Learn more about templates</span></span>](../azure-resource-manager/resource-manager-template-walkthrough.md)

[<span data-ttu-id="47923-126">Další informace o síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="47923-126">Learn more about Network Resources</span></span>](../virtual-network/resource-groups-networking.md)

<span data-ttu-id="47923-127">Šablony rychlý start používá nástroj pro vyrovnávání zatížení Azure lze nalézt v [úložiště GitHub](https://github.com/Azure/azure-quickstart-templates) hostování sadu komunity generované šablon.</span><span class="sxs-lookup"><span data-stu-id="47923-127">Quickstart templates using Azure Load Balancer can be found in a [GitHub repository](https://github.com/Azure/azure-quickstart-templates) hosting a set of community generated templates.</span></span>

<span data-ttu-id="47923-128">Příklady šablon:</span><span class="sxs-lookup"><span data-stu-id="47923-128">Examples of templates:</span></span>

* [<span data-ttu-id="47923-129">2 virtuální počítače v Vyrovnávání zatížení a pravidel Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="47923-129">2 VMs in a Load Balancer and load balancing rules</span></span>](http://go.microsoft.com/fwlink/?LinkId=544799)
* [<span data-ttu-id="47923-130">2 virtuální počítače ve virtuální síti s pravidly interní nástroj pro vyrovnávání zatížení a nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="47923-130">2 VMs in a VNET with an Internal Load Balancer and Load Balancer rules</span></span>](http://go.microsoft.com/fwlink/?LinkId=544800)
* [<span data-ttu-id="47923-131">2 virtuální počítače ve službě Vyrovnávání zatížení a nakonfigurovat pravidla NAT na hello LB</span><span class="sxs-lookup"><span data-stu-id="47923-131">2 VMs in a Load Balancer and configure NAT rules on hello LB</span></span>](http://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a><span data-ttu-id="47923-132">Nastavení služby Vyrovnávání zatížení Azure, PowerShell nebo rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="47923-132">Setting up Azure Load Balancer with a PowerShell or CLI</span></span>

<span data-ttu-id="47923-133">Začínáme s rutiny Azure Resource Manager, nástroje příkazového řádku a rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="47923-133">Get started with Azure Resource Manager cmdlets, command line tools, and REST APIs</span></span>

* <span data-ttu-id="47923-134">[Rutiny Azure sítě](https://msdn.microsoft.com/library/azure/mt163510.aspx) lze použít toocreate nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="47923-134">[Azure Networking Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) can be used toocreate a Load Balancer.</span></span>
* [<span data-ttu-id="47923-135">Jak toocreate Vyrovnávání zatížení pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="47923-135">How toocreate a load balancer using Azure Resource Manager</span></span>](load-balancer-get-started-ilb-arm-ps.md)
* [<span data-ttu-id="47923-136">Správa prostředků Azure pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="47923-136">Using hello Azure CLI with Azure Resource Management</span></span>](../xplat-cli-azure-resource-manager.md)
* [<span data-ttu-id="47923-137">Rozhraní REST API nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="47923-137">Load Balancer REST APIs</span></span>](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a><span data-ttu-id="47923-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47923-138">Next steps</span></span>

<span data-ttu-id="47923-139">Můžete také [začínáte s vytvářením internetovým nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) a konfigurace, jaký druh [režim distribuce](load-balancer-distribution-mode.md) pro konkrétní zatížení vyrovnávání síťové přenosy chování.</span><span class="sxs-lookup"><span data-stu-id="47923-139">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for a specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="47923-140">Zjistěte, jak toomanage [nečinnosti nastavení časového limitu TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="47923-140">Learn how toomanage [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="47923-141">To je důležité, pokud aplikace potřebuje tookeep připojení zachování připojení pro servery za službou Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="47923-141">This is important when your application needs tookeep connections alive for servers behind a load balancer.</span></span>
