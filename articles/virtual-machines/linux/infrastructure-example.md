---
title: "Příklad infrastruktury Azure návod | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro nasazení infrastruktury příklad v Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 281fc2c0-b533-45fa-81a3-728c0049c73d
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b18be0d81d6fad7328edb47c9b69af4eecd3b971
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-linux-vms"></a><span data-ttu-id="7e304-103">Příklad infrastruktury Azure návod pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="7e304-103">Example Azure infrastructure walkthrough for Linux VMs</span></span>

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="7e304-104">Tento článek vás provede vytváření infrastruktury příklad aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e304-104">This article walks through building out an example application infrastructure.</span></span> <span data-ttu-id="7e304-105">Jsme podrobnosti navrhování infrastruktury pro jednoduché online obchodu, která spojuje všechny pokyny a rozhodnutí, která kolem názvů, skupiny dostupnosti, virtuální sítě a nástroje pro vyrovnávání zatížení a ve skutečnosti nasazení virtuálních počítačů (VM).</span><span class="sxs-lookup"><span data-stu-id="7e304-105">We detail designing an infrastructure for a simple on-line store that brings together all the guidelines and decisions around naming conventions, availability sets, virtual networks and load balancers, and actually deploying your virtual machines (VMs).</span></span>

## <a name="example-workload"></a><span data-ttu-id="7e304-106">Příklad úloh</span><span class="sxs-lookup"><span data-stu-id="7e304-106">Example workload</span></span>
<span data-ttu-id="7e304-107">Společnosti Adventure Works Cycles chce sestavit aplikaci online úložiště v Azure, který se skládá z:</span><span class="sxs-lookup"><span data-stu-id="7e304-107">Adventure Works Cycles wants to build an on-line store application in Azure that consists of:</span></span>

* <span data-ttu-id="7e304-108">Dva servery nginx spuštění front-endu do vrstvy webového klienta</span><span class="sxs-lookup"><span data-stu-id="7e304-108">Two nginx servers running the client front-end in a web tier</span></span>
* <span data-ttu-id="7e304-109">Dva servery nginx zpracování dat a objednávky v aplikační vrstvě</span><span class="sxs-lookup"><span data-stu-id="7e304-109">Two nginx servers processing data and orders in an application tier</span></span>
* <span data-ttu-id="7e304-110">Dva MongoDB servery součástí horizontálně dělené clusteru pro ukládání dat produktu a objednávky v databázové vrstvy</span><span class="sxs-lookup"><span data-stu-id="7e304-110">Two MongoDB servers part of a sharded cluster for storing product data and orders in a database tier</span></span>
* <span data-ttu-id="7e304-111">Dva řadiče domény služby Active Directory pro účty zákazníků a všichni dodavatelé v vrstvou ověřování</span><span class="sxs-lookup"><span data-stu-id="7e304-111">Two Active Directory domain controllers for customer accounts and suppliers in an authentication tier</span></span>
* <span data-ttu-id="7e304-112">Všechny servery se nacházejí v dvě podsítě:</span><span class="sxs-lookup"><span data-stu-id="7e304-112">All the servers are located in two subnets:</span></span>
  * <span data-ttu-id="7e304-113">podsíť front-end pro webové servery</span><span class="sxs-lookup"><span data-stu-id="7e304-113">a front-end subnet for the web servers</span></span> 
  * <span data-ttu-id="7e304-114">back-end podsíť pro aplikační servery, MongoDB clusteru a řadiče domény</span><span class="sxs-lookup"><span data-stu-id="7e304-114">a back-end subnet for the application servers, MongoDB cluster, and domain controllers</span></span>

![Diagram různých vrstev pro infrastrukturu aplikace](./media/infrastructure-example/example-tiers.png)

<span data-ttu-id="7e304-116">Zabezpečené příchozí webové přenosy musí být vyrovnávání zatížení mezi webovými servery jako zákazníci procházet online úložiště.</span><span class="sxs-lookup"><span data-stu-id="7e304-116">Incoming secure web traffic must be load-balanced among the web servers as customers browse the on-line store.</span></span> <span data-ttu-id="7e304-117">Pořadí zpracování přenosů dat ve formátu HTTP požadavků z webu, které servery musí být vyrovnávání zatížení mezi servery aplikací.</span><span class="sxs-lookup"><span data-stu-id="7e304-117">Order processing traffic in the form of HTTP requests from the web servers must be load-balanced among the application servers.</span></span> <span data-ttu-id="7e304-118">Kromě toho musí být navrženy infrastruktury pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="7e304-118">Additionally, the infrastructure must be designed for high availability.</span></span>

<span data-ttu-id="7e304-119">Výsledný návrhu musí obsahovat:</span><span class="sxs-lookup"><span data-stu-id="7e304-119">The resulting design must incorporate:</span></span>

* <span data-ttu-id="7e304-120">Předplatné Azure a účet</span><span class="sxs-lookup"><span data-stu-id="7e304-120">An Azure subscription and account</span></span>
* <span data-ttu-id="7e304-121">Jedna skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="7e304-121">A single resource group</span></span>
* <span data-ttu-id="7e304-122">Azure Managed Disks</span><span class="sxs-lookup"><span data-stu-id="7e304-122">Azure Managed Disks</span></span>
* <span data-ttu-id="7e304-123">Virtuální síť se dvěma podsítěmi</span><span class="sxs-lookup"><span data-stu-id="7e304-123">A virtual network with two subnets</span></span>
* <span data-ttu-id="7e304-124">Sady dostupnosti pro virtuální počítače s podobnou roli</span><span class="sxs-lookup"><span data-stu-id="7e304-124">Availability sets for the VMs with a similar role</span></span>
* <span data-ttu-id="7e304-125">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="7e304-125">Virtual machines</span></span>

<span data-ttu-id="7e304-126">Všechny výše použijte tyto zásady vytváření názvů:</span><span class="sxs-lookup"><span data-stu-id="7e304-126">All the above follow these naming conventions:</span></span>

* <span data-ttu-id="7e304-127">Adventure Works Cycles používá **[IT zatížení]-[umístění] – [prostředků Azure]** jako předponu</span><span class="sxs-lookup"><span data-stu-id="7e304-127">Adventure Works Cycles uses **[IT workload]-[location]-[Azure resource]** as a prefix</span></span>
  * <span data-ttu-id="7e304-128">V tomto příkladu "**azos**" (online úložiště Azure) je název úlohy IT a "**použít**" (východní USA 2) je umístění</span><span class="sxs-lookup"><span data-stu-id="7e304-128">For this example, "**azos**" (Azure On-line Store) is the IT workload name and "**use**" (East US 2) is the location</span></span>
* <span data-ttu-id="7e304-129">Virtuální sítě pomocí AZOS. POUŽIJTE VN**[číslo]**</span><span class="sxs-lookup"><span data-stu-id="7e304-129">Virtual networks use AZOS-USE-VN**[number]**</span></span>
* <span data-ttu-id="7e304-130">Pomocí sad dostupnosti azos-použít-jako-**[role]**</span><span class="sxs-lookup"><span data-stu-id="7e304-130">Availability sets use azos-use-as-**[role]**</span></span>
* <span data-ttu-id="7e304-131">Názvy virtuálních počítačů pomocí azos-použít-vm -**[vmname]**</span><span class="sxs-lookup"><span data-stu-id="7e304-131">Virtual machine names use azos-use-vm-**[vmname]**</span></span>

## <a name="azure-subscriptions-and-accounts"></a><span data-ttu-id="7e304-132">Předplatná Azure a účty</span><span class="sxs-lookup"><span data-stu-id="7e304-132">Azure subscriptions and accounts</span></span>
<span data-ttu-id="7e304-133">Společnosti Adventure Works Cycles je pomocí své podnikové předplatné s názvem společnosti Adventure Works podnikové předplatné, zajistit fakturace pro tuto úlohu IT.</span><span class="sxs-lookup"><span data-stu-id="7e304-133">Adventure Works Cycles is using their Enterprise subscription, named Adventure Works Enterprise Subscription, to provide billing for this IT workload.</span></span>

## <a name="storage"></a><span data-ttu-id="7e304-134">Úložiště</span><span class="sxs-lookup"><span data-stu-id="7e304-134">Storage</span></span>
<span data-ttu-id="7e304-135">Společnosti Adventure Works Cycles určit, že by měli používat Azure spravované disky.</span><span class="sxs-lookup"><span data-stu-id="7e304-135">Adventure Works Cycles determined that they should use Azure Managed Disks.</span></span> <span data-ttu-id="7e304-136">Při vytváření virtuálních počítačů, se používají i vrstvy úložiště k dispozici úložiště:</span><span class="sxs-lookup"><span data-stu-id="7e304-136">When creating VMs, both storage available storage tiers are used:</span></span>

* <span data-ttu-id="7e304-137">**Standardní úložiště** pro webové servery, aplikační servery a řadiče domény a jejich datových disků.</span><span class="sxs-lookup"><span data-stu-id="7e304-137">**Standard storage** for the web servers, application servers, and domain controllers and their data disks.</span></span>
* <span data-ttu-id="7e304-138">**Storage úrovně Premium** pro MongoDB horizontálně dělené clusterových serverů a jejich datových disků.</span><span class="sxs-lookup"><span data-stu-id="7e304-138">**Premium storage** for the MongoDB sharded cluster servers and their data disks.</span></span>

## <a name="virtual-network-and-subnets"></a><span data-ttu-id="7e304-139">Virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="7e304-139">Virtual network and subnets</span></span>
<span data-ttu-id="7e304-140">Vzhledem k tomu, že virtuální sítě nemusí probíhající připojení k síti společnosti Adventure pracovní cykly místní, rozhodla ve virtuální síti jenom pro cloud.</span><span class="sxs-lookup"><span data-stu-id="7e304-140">Because the virtual network does not need ongoing connectivity to the Adventure Work Cycles on-premises network, they decided on a cloud-only virtual network.</span></span>

<span data-ttu-id="7e304-141">Vytvářely jenom pro cloud virtuální síť s následujícím nastavením pomocí portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="7e304-141">They created a cloud-only virtual network with the following settings using the Azure portal:</span></span>

* <span data-ttu-id="7e304-142">Název: AZOS-použití VN01</span><span class="sxs-lookup"><span data-stu-id="7e304-142">Name: AZOS-USE-VN01</span></span>
* <span data-ttu-id="7e304-143">Umístění: Východní USA 2</span><span class="sxs-lookup"><span data-stu-id="7e304-143">Location: East US 2</span></span>
* <span data-ttu-id="7e304-144">Adresní prostor virtuální sítě: 10.0.0.0/8</span><span class="sxs-lookup"><span data-stu-id="7e304-144">Virtual network address space: 10.0.0.0/8</span></span>
* <span data-ttu-id="7e304-145">První podsíť:</span><span class="sxs-lookup"><span data-stu-id="7e304-145">First subnet:</span></span>
  * <span data-ttu-id="7e304-146">Název: front-endu</span><span class="sxs-lookup"><span data-stu-id="7e304-146">Name: FrontEnd</span></span>
  * <span data-ttu-id="7e304-147">Adresní prostor: 10.0.1.0/24</span><span class="sxs-lookup"><span data-stu-id="7e304-147">Address space: 10.0.1.0/24</span></span>
* <span data-ttu-id="7e304-148">Druhou podsíť:</span><span class="sxs-lookup"><span data-stu-id="7e304-148">Second subnet:</span></span>
  * <span data-ttu-id="7e304-149">Název: back-end</span><span class="sxs-lookup"><span data-stu-id="7e304-149">Name: BackEnd</span></span>
  * <span data-ttu-id="7e304-150">Adresní prostor: 10.0.2.0/24</span><span class="sxs-lookup"><span data-stu-id="7e304-150">Address space: 10.0.2.0/24</span></span>

## <a name="availability-sets"></a><span data-ttu-id="7e304-151">Skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="7e304-151">Availability sets</span></span>
<span data-ttu-id="7e304-152">Aby se zachovala vysokou dostupnost všechny čtyři úrovně jejich online úložiště, se rozhodli společnosti Adventure Works Cycles čtyři skupiny dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="7e304-152">To maintain high availability of all four tiers of their on-line store, Adventure Works Cycles decided on four availability sets:</span></span>

* <span data-ttu-id="7e304-153">**azos použít jako webový** pro webové servery</span><span class="sxs-lookup"><span data-stu-id="7e304-153">**azos-use-as-web** for the web servers</span></span>
* <span data-ttu-id="7e304-154">**azos používání jako aplikace** pro aplikační servery</span><span class="sxs-lookup"><span data-stu-id="7e304-154">**azos-use-as-app** for the application servers</span></span>
* <span data-ttu-id="7e304-155">**azos použijte jako db** pro servery v clusteru horizontálně dělené MongoDB</span><span class="sxs-lookup"><span data-stu-id="7e304-155">**azos-use-as-db** for the servers in the MongoDB sharded cluster</span></span>
* <span data-ttu-id="7e304-156">**azos použijte jako dc** pro řadiče domény</span><span class="sxs-lookup"><span data-stu-id="7e304-156">**azos-use-as-dc** for the domain controllers</span></span>

## <a name="virtual-machines"></a><span data-ttu-id="7e304-157">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="7e304-157">Virtual machines</span></span>
<span data-ttu-id="7e304-158">Následující názvy pro své virtuální počítače Azure se rozhodli společnosti Adventure Works Cycles:</span><span class="sxs-lookup"><span data-stu-id="7e304-158">Adventure Works Cycles decided on the following names for their Azure VMs:</span></span>

* <span data-ttu-id="7e304-159">**azos použití virtuálních počítačů web01** pro prvního webového serveru</span><span class="sxs-lookup"><span data-stu-id="7e304-159">**azos-use-vm-web01** for the first web server</span></span>
* <span data-ttu-id="7e304-160">**azos použití virtuálních počítačů web02** pro druhého webového serveru</span><span class="sxs-lookup"><span data-stu-id="7e304-160">**azos-use-vm-web02** for the second web server</span></span>
* <span data-ttu-id="7e304-161">**azos použití virtuálních počítačů app01** pro první server pro aplikace</span><span class="sxs-lookup"><span data-stu-id="7e304-161">**azos-use-vm-app01** for the first application server</span></span>
* <span data-ttu-id="7e304-162">**azos použití virtuálních počítačů app02** pro druhý server aplikace</span><span class="sxs-lookup"><span data-stu-id="7e304-162">**azos-use-vm-app02** for the second application server</span></span>
* <span data-ttu-id="7e304-163">**azos použití virtuálních počítačů db01** pro první MongoDB serveru v clusteru</span><span class="sxs-lookup"><span data-stu-id="7e304-163">**azos-use-vm-db01** for the first MongoDB server in the cluster</span></span>
* <span data-ttu-id="7e304-164">**azos použití virtuálních počítačů db02** pro druhý server MongoDB v clusteru</span><span class="sxs-lookup"><span data-stu-id="7e304-164">**azos-use-vm-db02** for the second MongoDB server in the cluster</span></span>
* <span data-ttu-id="7e304-165">**azos použití virtuálních počítačů dc01** pro první řadič domény</span><span class="sxs-lookup"><span data-stu-id="7e304-165">**azos-use-vm-dc01** for the first domain controller</span></span>
* <span data-ttu-id="7e304-166">**azos použití virtuálních počítačů dc02** pro druhý řadič domény</span><span class="sxs-lookup"><span data-stu-id="7e304-166">**azos-use-vm-dc02** for the second domain controller</span></span>

<span data-ttu-id="7e304-167">Zde je Výsledná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7e304-167">Here is the resulting configuration.</span></span>

![Infrastruktura konečné aplikace nasazené v Azure](./media/infrastructure-example/example-config.png)

<span data-ttu-id="7e304-169">Tato konfigurace zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="7e304-169">This configuration incorporates:</span></span>

* <span data-ttu-id="7e304-170">Čistě cloudové virtuální síť se dvěma podsítěmi (front-endové a back-end)</span><span class="sxs-lookup"><span data-stu-id="7e304-170">A cloud-only virtual network with two subnets (FrontEnd and BackEnd)</span></span>
* <span data-ttu-id="7e304-171">Spravované disky systému Azure pomocí disků Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="7e304-171">Azure Managed Disks using both Standard and Premium disks</span></span>
* <span data-ttu-id="7e304-172">Čtyři skupiny dostupnosti, jeden pro každou vrstvu úložiště online</span><span class="sxs-lookup"><span data-stu-id="7e304-172">Four availability sets, one for each tier of the on-line store</span></span>
* <span data-ttu-id="7e304-173">Virtuální počítače pro čtyři vrstvy</span><span class="sxs-lookup"><span data-stu-id="7e304-173">The virtual machines for the four tiers</span></span>
* <span data-ttu-id="7e304-174">Externí s vyrovnáváním zatížení pro založený na protokolu HTTPS webové přenosy z Internetu webových serverů</span><span class="sxs-lookup"><span data-stu-id="7e304-174">An external load balanced set for HTTPS-based web traffic from the Internet to the web servers</span></span>
* <span data-ttu-id="7e304-175">K interní s vyrovnáváním zatížení pro nezašifrované webový provoz z webových serverů na aplikační servery</span><span class="sxs-lookup"><span data-stu-id="7e304-175">An internal load balanced set for unencrypted web traffic from the web servers to the application servers</span></span>
* <span data-ttu-id="7e304-176">Jedna skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="7e304-176">A single resource group</span></span>
