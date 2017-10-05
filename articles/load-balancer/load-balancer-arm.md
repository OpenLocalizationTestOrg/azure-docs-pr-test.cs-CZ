---
title: "Podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení | Microsoft Docs"
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
ms.openlocfilehash: d06c924f384a2684b5a91c202039c581796c1091
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a><span data-ttu-id="1d03e-104">Podporu správce prostředků Azure pomocí nástroje pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="1d03e-104">Using Azure Resource Manager Support with Azure Load Balancer</span></span>

<span data-ttu-id="1d03e-105">Azure Resource Manager je architektura upřednostňované správy pro služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="1d03e-105">Azure Resource Manager is the preferred management framework for services in Azure.</span></span> <span data-ttu-id="1d03e-106">Azure nástroj pro vyrovnávání zatížení můžete spravovat pomocí rozhraní API založené na Azure Resource Manager a nástroje.</span><span class="sxs-lookup"><span data-stu-id="1d03e-106">Azure Load Balancer can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="concepts"></a><span data-ttu-id="1d03e-107">Koncepty</span><span class="sxs-lookup"><span data-stu-id="1d03e-107">Concepts</span></span>

<span data-ttu-id="1d03e-108">S Resource Managerem se nástroj pro vyrovnávání zatížení Azure obsahuje následující podřízené prostředky:</span><span class="sxs-lookup"><span data-stu-id="1d03e-108">With Resource Manager, Azure Load Balancer contains the following child resources:</span></span>

* <span data-ttu-id="1d03e-109">Konfiguraci front-end IP adresy – nástroj pro vyrovnávání zatížení může obsahovat jednu nebo více adres IP front-endu známé jako virtuální IP adresy (VIP).</span><span class="sxs-lookup"><span data-stu-id="1d03e-109">Front-end IP configuration – a Load balancer can include one or more front end IP addresses, otherwise known as a virtual IPs (VIPs).</span></span> <span data-ttu-id="1d03e-110">Tyto IP adresy slouží jako vstup pro přenos.</span><span class="sxs-lookup"><span data-stu-id="1d03e-110">These IP addresses serve as ingress for the traffic.</span></span>
* <span data-ttu-id="1d03e-111">Fond adres back-end – Toto jsou IP adresy, které jsou spojené s virtuální počítač síťové rozhraní karta (NIC) do které budou distribuována zatížení.</span><span class="sxs-lookup"><span data-stu-id="1d03e-111">Back-end address pool – these are IP addresses associated with the virtual machine Network Interface Card (NIC) to which load will be distributed.</span></span>
* <span data-ttu-id="1d03e-112">Pravidla Vyrovnávání zatížení – vlastnost rule mapuje danou front-end IP a portu kombinace na sadu back-end IP adresy a portu.</span><span class="sxs-lookup"><span data-stu-id="1d03e-112">Load balancing rules – a rule property maps a given front end IP and port combination to a set of back end IP addresses and port combination.</span></span> <span data-ttu-id="1d03e-113">Nástroj pro vyrovnávání zatížení může mít několik pravidel vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="1d03e-113">A single load balancer can have multiple load balancing rules.</span></span> <span data-ttu-id="1d03e-114">Každé pravidlo je kombinací front-endové IP adresy a portu a back-endové IP adresy a portu přidružených k virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="1d03e-114">Each rule is a combination of a front-end IP and port and back-end IP and port associated with VMs.</span></span>
* <span data-ttu-id="1d03e-115">Sondy – sondy umožňují udržování přehledu o stavu instance virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1d03e-115">Probes – probes enable you to keep track of the health of VM instances.</span></span> <span data-ttu-id="1d03e-116">Pokud selže test stavu, instance virtuálního počítače se provedou mimo otočení automaticky.</span><span class="sxs-lookup"><span data-stu-id="1d03e-116">If a health probe fails, the VM instance will be taken out of rotation automatically.</span></span>
* <span data-ttu-id="1d03e-117">Příchozí pravidla NAT – příchozí provoz prostřednictvím před definováním pravidel NAT ukončení IP a distribuovat do back-end IP.</span><span class="sxs-lookup"><span data-stu-id="1d03e-117">Inbound NAT rules – NAT rules defining the inbound traffic flowing through the front end IP and distributed to the back end IP.</span></span>

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a><span data-ttu-id="1d03e-118">Šablony Rychlý start</span><span class="sxs-lookup"><span data-stu-id="1d03e-118">Quickstart templates</span></span>

<span data-ttu-id="1d03e-119">Azure Resource Manager umožňuje zřizovat aplikace pomocí deklarativní šablony.</span><span class="sxs-lookup"><span data-stu-id="1d03e-119">Azure Resource Manager allows you to provision your applications using a declarative template.</span></span> <span data-ttu-id="1d03e-120">S jednou šablonou můžete nasadit několik služeb společně s jejich závislostmi.</span><span class="sxs-lookup"><span data-stu-id="1d03e-120">In a single template, you can deploy multiple services along with their dependencies.</span></span> <span data-ttu-id="1d03e-121">Stejnou šablonu můžete použít k opakovanému nasazení aplikace během každé fáze životního cyklu této aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d03e-121">You use the same template to repeatedly deploy your application during every stage of the application lifecycle.</span></span>

<span data-ttu-id="1d03e-122">Šablony může obsahovat definice pro virtuální počítače, virtuální sítě, skupiny dostupnosti, síťových rozhraní (NIC), účty úložiště, nástroje pro vyrovnávání zatížení, skupiny zabezpečení sítě a veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1d03e-122">Templates can include definitions for Virtual Machines, Virtual Networks, Availability Sets, Network Interfaces (NICs), Storage Accounts, Load Balancers, Network Security Groups, and Public IPs.</span></span> <span data-ttu-id="1d03e-123">Pomocí šablony můžete vytvářet vše potřebné pro komplexní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d03e-123">With templates you can create everything you need for a complex application.</span></span> <span data-ttu-id="1d03e-124">Soubor šablony lze zkontrolovat do systému správy obsahu pro verzí a spolupráci.</span><span class="sxs-lookup"><span data-stu-id="1d03e-124">The template file can be checked into content management system for version control and collaboration.</span></span>

[<span data-ttu-id="1d03e-125">Další informace o šablonách</span><span class="sxs-lookup"><span data-stu-id="1d03e-125">Learn more about templates</span></span>](../azure-resource-manager/resource-manager-template-walkthrough.md)

[<span data-ttu-id="1d03e-126">Další informace o síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="1d03e-126">Learn more about Network Resources</span></span>](../virtual-network/resource-groups-networking.md)

<span data-ttu-id="1d03e-127">Šablony rychlý start používá nástroj pro vyrovnávání zatížení Azure lze nalézt v [úložiště GitHub](https://github.com/Azure/azure-quickstart-templates) hostování sadu komunity generované šablon.</span><span class="sxs-lookup"><span data-stu-id="1d03e-127">Quickstart templates using Azure Load Balancer can be found in a [GitHub repository](https://github.com/Azure/azure-quickstart-templates) hosting a set of community generated templates.</span></span>

<span data-ttu-id="1d03e-128">Příklady šablon:</span><span class="sxs-lookup"><span data-stu-id="1d03e-128">Examples of templates:</span></span>

* [<span data-ttu-id="1d03e-129">2 virtuální počítače v Vyrovnávání zatížení a pravidel Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1d03e-129">2 VMs in a Load Balancer and load balancing rules</span></span>](http://go.microsoft.com/fwlink/?LinkId=544799)
* [<span data-ttu-id="1d03e-130">2 virtuální počítače ve virtuální síti s pravidly interní nástroj pro vyrovnávání zatížení a nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1d03e-130">2 VMs in a VNET with an Internal Load Balancer and Load Balancer rules</span></span>](http://go.microsoft.com/fwlink/?LinkId=544800)
* [<span data-ttu-id="1d03e-131">2 virtuální počítače ve službě Vyrovnávání zatížení a nakonfigurovat na LB pravidla NAT</span><span class="sxs-lookup"><span data-stu-id="1d03e-131">2 VMs in a Load Balancer and configure NAT rules on the LB</span></span>](http://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a><span data-ttu-id="1d03e-132">Nastavení služby Vyrovnávání zatížení Azure, PowerShell nebo rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="1d03e-132">Setting up Azure Load Balancer with a PowerShell or CLI</span></span>

<span data-ttu-id="1d03e-133">Začínáme s rutiny Azure Resource Manager, nástroje příkazového řádku a rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="1d03e-133">Get started with Azure Resource Manager cmdlets, command line tools, and REST APIs</span></span>

* <span data-ttu-id="1d03e-134">[Rutiny Azure sítě](https://msdn.microsoft.com/library/azure/mt163510.aspx) slouží k vytvoření služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="1d03e-134">[Azure Networking Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) can be used to create a Load Balancer.</span></span>
* [<span data-ttu-id="1d03e-135">Jak vytvořit nástroj pro vyrovnávání zatížení pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1d03e-135">How to create a load balancer using Azure Resource Manager</span></span>](load-balancer-get-started-ilb-arm-ps.md)
* [<span data-ttu-id="1d03e-136">Správa prostředků Azure pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1d03e-136">Using the Azure CLI with Azure Resource Management</span></span>](../xplat-cli-azure-resource-manager.md)
* [<span data-ttu-id="1d03e-137">Rozhraní REST API nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1d03e-137">Load Balancer REST APIs</span></span>](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a><span data-ttu-id="1d03e-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d03e-138">Next steps</span></span>

<span data-ttu-id="1d03e-139">Můžete také [začínáte s vytvářením internetovým nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) a konfigurace, jaký druh [režim distribuce](load-balancer-distribution-mode.md) pro konkrétní zatížení vyrovnávání síťové přenosy chování.</span><span class="sxs-lookup"><span data-stu-id="1d03e-139">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for a specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="1d03e-140">Naučte se spravovat [nečinnosti nastavení časového limitu TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="1d03e-140">Learn how to manage [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="1d03e-141">To je důležité, pokud aplikace potřebuje k zachování připojení připojení pro servery za službou Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="1d03e-141">This is important when your application needs to keep connections alive for servers behind a load balancer.</span></span>
