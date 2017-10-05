---
title: "Přehled protokolu IPv6 pro vyrovnávání zatížení Azure | Microsoft Docs"
description: "Principy podporu IPv6 pro vyrovnávání zatížení Azure a virtuálních počítačů s vyrovnáváním zatížení."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
keywords: "protokol IPv6, nástroje pro vyrovnávání zatížení azure, duálním zásobníkem, veřejnou IP adresu, nativní protokol ipv6, mobilní, iot"
ms.assetid: 6a1d583f-a305-40fd-a94b-fa42e1943bbb
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: kumud
ms.openlocfilehash: 8cca857314ecf37ef51700fd25aef228515ecd0a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a><span data-ttu-id="59090-104">Přehled protokolu IPv6 pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="59090-104">Overview of IPv6 for Azure Load Balancer</span></span>

<span data-ttu-id="59090-105">Internetové služby Vyrovnávání zatížení můžete nasadit pomocí adresy IPv6.</span><span class="sxs-lookup"><span data-stu-id="59090-105">Internet-facing load balancers can be deployed with an IPv6 address.</span></span> <span data-ttu-id="59090-106">Kromě připojení pomocí protokolu IPv4 to umožňuje následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="59090-106">In addition to IPv4 connectivity, this enables the following capabilities:</span></span>

* <span data-ttu-id="59090-107">Nativní začátku do konce IPv6 připojení mezi klienty veřejného Internetu a virtuální počítače (VM) Azure prostřednictvím nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="59090-107">Native end-to-end IPv6 connectivity between public Internet clients and Azure Virtual Machines (VMs) through the load balancer.</span></span>
* <span data-ttu-id="59090-108">Nativní klient server IPv6 odchozí připojení mezi virtuální počítače a veřejné klienty používající Internetu IPv6.</span><span class="sxs-lookup"><span data-stu-id="59090-108">Native end-to-end IPv6 outbound connectivity between VMs and public Internet IPv6-enabled clients.</span></span>

<span data-ttu-id="59090-109">Následující obrázek znázorňuje IPv6 funkce nástroje pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="59090-109">The following picture illustrates the IPv6 functionality for Azure Load Balancer.</span></span>

![Nástroje pro vyrovnávání zatížení Azure s IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

<span data-ttu-id="59090-111">Po nasazení klientem IPv4 nebo IPv6 povolené Internetu může komunikovat s veřejné adresy IPv4 nebo IPv6 (nebo názvy hostitelů) Azure internetové služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="59090-111">Once deployed, an IPv4 or IPv6-enabled Internet client can communicate with the public IPv4 or IPv6 addresses (or hostnames) of the Azure Internet-facing Load Balancer.</span></span> <span data-ttu-id="59090-112">Nástroje pro vyrovnávání zatížení směrování paketů IPv6 privátní IPv6 adresy virtuálních počítačů pomocí překlad síťových adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="59090-112">The load balancer routes the IPv6 packets to the private IPv6 addresses of the VMs using network address translation (NAT).</span></span> <span data-ttu-id="59090-113">IPv6 Internet klient nemůže komunikovat přímo s adresou IPv6 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="59090-113">The IPv6 Internet client cannot communicate directly with the IPv6 address of the VMs.</span></span>

## <a name="features"></a><span data-ttu-id="59090-114">Funkce</span><span class="sxs-lookup"><span data-stu-id="59090-114">Features</span></span>

<span data-ttu-id="59090-115">Poskytuje nativní podporu IPv6 pro virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="59090-115">Native IPv6 support for VMs deployed via Azure Resource Manager provides:</span></span>

1. <span data-ttu-id="59090-116">Vyrovnávání zatížení služby IPv6 pro IPv6 klientů na Internetu</span><span class="sxs-lookup"><span data-stu-id="59090-116">Load-balanced IPv6 services for IPv6 clients on the Internet</span></span>
2. <span data-ttu-id="59090-117">Nativní protokol IPv6 a IPv4 koncových bodů na virtuálních počítačích ("dva skládaný")</span><span class="sxs-lookup"><span data-stu-id="59090-117">Native IPv6 and IPv4 endpoints on VMs ("dual stacked")</span></span>
3. <span data-ttu-id="59090-118">Příchozí a odchozí spouštěná nativní IPv6 připojení</span><span class="sxs-lookup"><span data-stu-id="59090-118">Inbound and outbound-initiated native IPv6 connections</span></span>
4. <span data-ttu-id="59090-119">Podporované protokoly, jako je například TCP, UDP a HTTP (S) povolte celou řadu architektury služby</span><span class="sxs-lookup"><span data-stu-id="59090-119">Supported protocols such as TCP, UDP, and HTTP(S) enable a full range of service architectures</span></span>

## <a name="benefits"></a><span data-ttu-id="59090-120">Výhody</span><span class="sxs-lookup"><span data-stu-id="59090-120">Benefits</span></span>

<span data-ttu-id="59090-121">Tato funkce umožňuje následující klíčové výhody:</span><span class="sxs-lookup"><span data-stu-id="59090-121">This functionality enables the following key benefits:</span></span>

* <span data-ttu-id="59090-122">Splňují předpisy government vyžadující, že nové aplikace být dostupný klientům pouze protokol IPv6</span><span class="sxs-lookup"><span data-stu-id="59090-122">Meet government regulations requiring that new applications be accessible to IPv6-only clients</span></span>
* <span data-ttu-id="59090-123">Povolit mobilní a Internet věcí (IOT) vývojářům používat duální skládaný virtuální počítače Azure (IPv4 + IPv6) Chcete-li vyřešit rostoucí mobile & trhů IOT</span><span class="sxs-lookup"><span data-stu-id="59090-123">Enable mobile and Internet of things (IOT) developers to use dual-stacked (IPv4+IPv6) Azure Virtual Machines to address the growing mobile & IOT markets</span></span>

## <a name="details-and-limitations"></a><span data-ttu-id="59090-124">Podrobnosti a omezení</span><span class="sxs-lookup"><span data-stu-id="59090-124">Details and limitations</span></span>

<span data-ttu-id="59090-125">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="59090-125">Details</span></span>

* <span data-ttu-id="59090-126">Služba Azure DNS obsahuje záznamy, název IPv4 A i IPv6 AAAA a odpoví oba záznamy pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="59090-126">The Azure DNS service contains both IPv4 A and IPv6 AAAA name records and responds with both records for the load balancer.</span></span> <span data-ttu-id="59090-127">Klient zvolí které adresy (IPv4 nebo IPv6) ke komunikaci s.</span><span class="sxs-lookup"><span data-stu-id="59090-127">The client chooses which address (IPv4 or IPv6) to communicate with.</span></span>
* <span data-ttu-id="59090-128">Když virtuální počítač zahájí připojení do veřejných zařízení připojená k Internetu IPv6, Virtuálního počítače zdrojovou adresu IPv6 je síťová adresa přeložit (NAT) na veřejnou adresu IPv6 služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="59090-128">When a VM initiates a connection to a public Internet IPv6-connected device, the VM's source IPv6 address is network address translated (NAT) to the public IPv6 address of the load balancer.</span></span>
* <span data-ttu-id="59090-129">Virtuální počítače s operačním systémem Linux musí být nakonfigurované pro příjem IPv6 IP adresu prostřednictvím DHCP.</span><span class="sxs-lookup"><span data-stu-id="59090-129">VMs running the Linux operating system must be configured to receive an IPv6 IP address via DHCP.</span></span> <span data-ttu-id="59090-130">Mnoho Linux obrázků v galerii Azure jsou již nakonfigurována na podporovat protokol IPv6 bez úprav.</span><span class="sxs-lookup"><span data-stu-id="59090-130">Many of the Linux images in the Azure Gallery are already configured to support IPv6 without modification.</span></span> <span data-ttu-id="59090-131">Další informace najdete v tématu [DHCPv6 konfiguraci pro virtuální počítače s Linuxem](load-balancer-ipv6-for-linux.md)</span><span class="sxs-lookup"><span data-stu-id="59090-131">For more information, see [Configuring DHCPv6 for Linux VMs](load-balancer-ipv6-for-linux.md)</span></span>
* <span data-ttu-id="59090-132">Pokud chcete použít test stavu se nástroj pro vyrovnávání zatížení, vytvořit test paměti IPv4 a jeho použití s IPv4 a IPv6 koncové body.</span><span class="sxs-lookup"><span data-stu-id="59090-132">If you choose to use a health probe with your load balancer, create an IPv4 probe and use it with both the IPv4 and IPv6 endpoints.</span></span> <span data-ttu-id="59090-133">Pokud službu na virtuální počítač přestane fungovat, koncové body IPv4 i IPv6 se vyjímají z otočení.</span><span class="sxs-lookup"><span data-stu-id="59090-133">If the service on your VM goes down, both the IPv4 and IPv6 endpoints are taken out of rotation.</span></span>

<span data-ttu-id="59090-134">Omezení</span><span class="sxs-lookup"><span data-stu-id="59090-134">Limitations</span></span>

* <span data-ttu-id="59090-135">Nelze přidat pravidla Vyrovnávání zatížení IPv6 na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="59090-135">You cannot add IPv6 load balancing rules in the Azure portal.</span></span> <span data-ttu-id="59090-136">Pravidla lze vytvořit pouze pomocí šablony, rozhraní příkazového řádku, prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="59090-136">The rules can only be created through the template, CLI, PowerShell.</span></span>
* <span data-ttu-id="59090-137">Nemusí upgradovat stávající virtuální počítače použít adresu IPv6.</span><span class="sxs-lookup"><span data-stu-id="59090-137">You may not upgrade existing VMs to use IPv6 addresses.</span></span> <span data-ttu-id="59090-138">Je nutné nasadit nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="59090-138">You must deploy new VMs.</span></span>
* <span data-ttu-id="59090-139">Jedna adresa IPv6 lze přiřadit k jedno síťové rozhraní v jednotlivých virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="59090-139">A single IPv6 address can be assigned to a single network interface in each VM.</span></span>
* <span data-ttu-id="59090-140">Veřejné adresy IPv6 nelze přiřadit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="59090-140">The public IPv6 addresses cannot be assigned to a VM.</span></span> <span data-ttu-id="59090-141">Můžete lze přiřadit pouze k nástroji pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="59090-141">They can only be assigned to a load balancer.</span></span>
* <span data-ttu-id="59090-142">Nelze konfigurovat zpětné vyhledávání DNS pro vaše veřejné adresy IPv6.</span><span class="sxs-lookup"><span data-stu-id="59090-142">You cannot configure the reverse DNS lookup for your public IPv6 addresses.</span></span>
* <span data-ttu-id="59090-143">Virtuální počítače s adresami IPv6 nemohou být členy cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="59090-143">The VMs with the IPv6 addresses cannot be members of an Azure Cloud Service.</span></span> <span data-ttu-id="59090-144">Tyto může být připojen k virtuální síti Azure (VNet) a vzájemně komunikovat prostřednictvím jejich adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="59090-144">They can be connected to an Azure Virtual Network (VNet) and communicate with each other over their IPv4 addresses.</span></span>
* <span data-ttu-id="59090-145">Privátní IPv6 adresy můžou být nasazené na jednotlivé virtuální počítače ve skupině prostředků, ale nelze nasadit do skupiny prostředků prostřednictvím sady škálování.</span><span class="sxs-lookup"><span data-stu-id="59090-145">Private IPv6 addresses can be deployed on individual VMs in a resource group but cannot be deployed into a resource group via Scale Sets.</span></span>
* <span data-ttu-id="59090-146">Virtuální počítače Azure se nemůže připojit přes protokol IPv6 pro ostatní virtuální počítače, ostatní služby Azure nebo místní zařízení.</span><span class="sxs-lookup"><span data-stu-id="59090-146">Azure VMs cannot connect over IPv6 to other VMs, other Azure services, or on-premises devices.</span></span> <span data-ttu-id="59090-147">Nástroje pro vyrovnávání zatížení Azure mohou pouze komunikovat přes protokol IPv6.</span><span class="sxs-lookup"><span data-stu-id="59090-147">They can only communicate with the Azure load balancer over IPv6.</span></span> <span data-ttu-id="59090-148">Však může komunikovat s tyto prostředky, které pomocí protokolu IPv4.</span><span class="sxs-lookup"><span data-stu-id="59090-148">However, they can communicate with these other resources using IPv4.</span></span>
* <span data-ttu-id="59090-149">Skupina zabezpečení sítě (NSG) ochrany pro protokol IPv4 je podporována v nasazeních duální sada protokolů (IPv4 + IPv6).</span><span class="sxs-lookup"><span data-stu-id="59090-149">Network Security Group (NSG) protection for IPv4 is supported in dual-stack (IPv4+IPv6) deployments.</span></span> <span data-ttu-id="59090-150">Skupiny Nsg se nevztahují ke koncovým bodům protokol IPv6.</span><span class="sxs-lookup"><span data-stu-id="59090-150">NSGs do not apply to the IPv6 endpoints.</span></span>
* <span data-ttu-id="59090-151">Koncový bod IPv6 adresy ve virtuálním počítači není přístup přímo k Internetu.</span><span class="sxs-lookup"><span data-stu-id="59090-151">The IPv6 endpoint on the VM is not exposed directly to the internet.</span></span> <span data-ttu-id="59090-152">Je za službou Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="59090-152">It is behind a load balancer.</span></span> <span data-ttu-id="59090-153">Pouze porty, které jsou určené v pravidla nástroje pro vyrovnávání zatížení jsou přístupné přes protokol IPv6.</span><span class="sxs-lookup"><span data-stu-id="59090-153">Only the ports specified in the load balancer rules are accessible over IPv6.</span></span>
* <span data-ttu-id="59090-154">Změna parametr IdleTimeout pro protokol IPv6 je **aktuálně není podporována**.</span><span class="sxs-lookup"><span data-stu-id="59090-154">Changing the IdleTimeout parameter for IPv6 is **currently not supported**.</span></span> <span data-ttu-id="59090-155">Výchozí hodnota je 4 minut.</span><span class="sxs-lookup"><span data-stu-id="59090-155">The default is four minutes.</span></span>
* <span data-ttu-id="59090-156">Změna parametr loadDistributionMethod pro protokol IPv6 je **aktuálně není podporována**.</span><span class="sxs-lookup"><span data-stu-id="59090-156">Changing the loadDistributionMethod parameter for IPv6 is **currently not supported**.</span></span>
* <span data-ttu-id="59090-157">Vyhrazené IP adresy IPv6 (kde IPAllocationMethod = statické) jsou **aktuálně není podporována**.</span><span class="sxs-lookup"><span data-stu-id="59090-157">Reserved IPv6 IPs (where IPAllocationMethod = static) are **currently not supported**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59090-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59090-158">Next steps</span></span>

<span data-ttu-id="59090-159">Informace o nasazení Vyrovnávání zatížení s IPv6.</span><span class="sxs-lookup"><span data-stu-id="59090-159">Learn how to deploy a load balancer with IPv6.</span></span>

* [<span data-ttu-id="59090-160">Dostupnost podle oblasti IPv6</span><span class="sxs-lookup"><span data-stu-id="59090-160">Availability of IPv6 by region</span></span>](https://go.microsoft.com/fwlink/?linkid=828357)
* [<span data-ttu-id="59090-161">Nasazení Vyrovnávání zatížení s IPv6 pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="59090-161">Deploy a load balancer with IPv6 using a template</span></span>](load-balancer-ipv6-internet-template.md)
* [<span data-ttu-id="59090-162">Nasazení Vyrovnávání zatížení s IPv6 pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="59090-162">Deploy a load balancer with IPv6 using Azure PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
* [<span data-ttu-id="59090-163">Nasazení Vyrovnávání zatížení s IPv6 pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="59090-163">Deploy a load balancer with IPv6 using Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
