---
title: "aaaExample návod infrastruktury Azure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro nasazení infrastruktury příklad v Azure."
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
ms.openlocfilehash: b6b307c0112203aa83e1fada81966fc265397a40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-linux-vms"></a><span data-ttu-id="cc804-103">Příklad infrastruktury Azure návod pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="cc804-103">Example Azure infrastructure walkthrough for Linux VMs</span></span>

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="cc804-104">Tento článek vás provede vytváření infrastruktury příklad aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc804-104">This article walks through building out an example application infrastructure.</span></span> <span data-ttu-id="cc804-105">Jsme podrobnosti navrhování infrastruktury pro jednoduché online obchodu, která spojuje všechny hello pokyny a rozhodnutí, která kolem názvů, skupiny dostupnosti, virtuální sítě a nástroje pro vyrovnávání zatížení a ve skutečnosti nasazení virtuálních počítačů (VM).</span><span class="sxs-lookup"><span data-stu-id="cc804-105">We detail designing an infrastructure for a simple on-line store that brings together all hello guidelines and decisions around naming conventions, availability sets, virtual networks and load balancers, and actually deploying your virtual machines (VMs).</span></span>

## <a name="example-workload"></a><span data-ttu-id="cc804-106">Příklad úloh</span><span class="sxs-lookup"><span data-stu-id="cc804-106">Example workload</span></span>
<span data-ttu-id="cc804-107">Společnosti Adventure Works Cycles chce toobuild aplikace online úložiště v Azure, který se skládá z:</span><span class="sxs-lookup"><span data-stu-id="cc804-107">Adventure Works Cycles wants toobuild an on-line store application in Azure that consists of:</span></span>

* <span data-ttu-id="cc804-108">Dva servery nginx spuštěným klientem hello front-endu v webovou vrstvu</span><span class="sxs-lookup"><span data-stu-id="cc804-108">Two nginx servers running hello client front-end in a web tier</span></span>
* <span data-ttu-id="cc804-109">Dva servery nginx zpracování dat a objednávky v aplikační vrstvě</span><span class="sxs-lookup"><span data-stu-id="cc804-109">Two nginx servers processing data and orders in an application tier</span></span>
* <span data-ttu-id="cc804-110">Dva MongoDB servery součástí horizontálně dělené clusteru pro ukládání dat produktu a objednávky v databázové vrstvy</span><span class="sxs-lookup"><span data-stu-id="cc804-110">Two MongoDB servers part of a sharded cluster for storing product data and orders in a database tier</span></span>
* <span data-ttu-id="cc804-111">Dva řadiče domény služby Active Directory pro účty zákazníků a všichni dodavatelé v vrstvou ověřování</span><span class="sxs-lookup"><span data-stu-id="cc804-111">Two Active Directory domain controllers for customer accounts and suppliers in an authentication tier</span></span>
* <span data-ttu-id="cc804-112">Všechny servery hello se nacházejí v dvě podsítě:</span><span class="sxs-lookup"><span data-stu-id="cc804-112">All hello servers are located in two subnets:</span></span>
  * <span data-ttu-id="cc804-113">podsíť front-end pro webové servery hello</span><span class="sxs-lookup"><span data-stu-id="cc804-113">a front-end subnet for hello web servers</span></span> 
  * <span data-ttu-id="cc804-114">back-end podsíť pro aplikační servery hello MongoDB clusteru a řadiče domény</span><span class="sxs-lookup"><span data-stu-id="cc804-114">a back-end subnet for hello application servers, MongoDB cluster, and domain controllers</span></span>

![Diagram různých vrstev pro infrastrukturu aplikace](./media/infrastructure-example/example-tiers.png)

<span data-ttu-id="cc804-116">Zabezpečené příchozí webové přenosy musí být vyrovnávání zatížení mezi hello webové servery jako zákazníci procházet hello online úložiště.</span><span class="sxs-lookup"><span data-stu-id="cc804-116">Incoming secure web traffic must be load-balanced among hello web servers as customers browse hello on-line store.</span></span> <span data-ttu-id="cc804-117">Pořadí zpracování přenosů dat ve formuláři hello HTTP požadavků webová hello servery musí být vyrovnávání zatížení mezi servery aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cc804-117">Order processing traffic in hello form of HTTP requests from hello web servers must be load-balanced among hello application servers.</span></span> <span data-ttu-id="cc804-118">Kromě toho musí být navrženy hello infrastruktury pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="cc804-118">Additionally, hello infrastructure must be designed for high availability.</span></span>

<span data-ttu-id="cc804-119">Výsledný návrhu Hello musí obsahovat:</span><span class="sxs-lookup"><span data-stu-id="cc804-119">hello resulting design must incorporate:</span></span>

* <span data-ttu-id="cc804-120">Předplatné Azure a účet</span><span class="sxs-lookup"><span data-stu-id="cc804-120">An Azure subscription and account</span></span>
* <span data-ttu-id="cc804-121">Jedna skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="cc804-121">A single resource group</span></span>
* <span data-ttu-id="cc804-122">Azure Managed Disks</span><span class="sxs-lookup"><span data-stu-id="cc804-122">Azure Managed Disks</span></span>
* <span data-ttu-id="cc804-123">Virtuální síť se dvěma podsítěmi</span><span class="sxs-lookup"><span data-stu-id="cc804-123">A virtual network with two subnets</span></span>
* <span data-ttu-id="cc804-124">Sady dostupnosti pro hello virtuálních počítačů s podobnou roli</span><span class="sxs-lookup"><span data-stu-id="cc804-124">Availability sets for hello VMs with a similar role</span></span>
* <span data-ttu-id="cc804-125">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="cc804-125">Virtual machines</span></span>

<span data-ttu-id="cc804-126">Všechny výše uvedené hello postupujte tyto zásady vytváření názvů:</span><span class="sxs-lookup"><span data-stu-id="cc804-126">All hello above follow these naming conventions:</span></span>

* <span data-ttu-id="cc804-127">Adventure Works Cycles používá **[IT zatížení]-[umístění] – [prostředků Azure]** jako předponu</span><span class="sxs-lookup"><span data-stu-id="cc804-127">Adventure Works Cycles uses **[IT workload]-[location]-[Azure resource]** as a prefix</span></span>
  * <span data-ttu-id="cc804-128">V tomto příkladu "**azos**" (online úložiště Azure) je hello název úlohy IT a "**použít**" (východní USA 2) je umístění hello</span><span class="sxs-lookup"><span data-stu-id="cc804-128">For this example, "**azos**" (Azure On-line Store) is hello IT workload name and "**use**" (East US 2) is hello location</span></span>
* <span data-ttu-id="cc804-129">Virtuální sítě pomocí AZOS. POUŽIJTE VN**[číslo]**</span><span class="sxs-lookup"><span data-stu-id="cc804-129">Virtual networks use AZOS-USE-VN**[number]**</span></span>
* <span data-ttu-id="cc804-130">Pomocí sad dostupnosti azos-použít-jako-**[role]**</span><span class="sxs-lookup"><span data-stu-id="cc804-130">Availability sets use azos-use-as-**[role]**</span></span>
* <span data-ttu-id="cc804-131">Názvy virtuálních počítačů pomocí azos-použít-vm -**[vmname]**</span><span class="sxs-lookup"><span data-stu-id="cc804-131">Virtual machine names use azos-use-vm-**[vmname]**</span></span>

## <a name="azure-subscriptions-and-accounts"></a><span data-ttu-id="cc804-132">Předplatná Azure a účty</span><span class="sxs-lookup"><span data-stu-id="cc804-132">Azure subscriptions and accounts</span></span>
<span data-ttu-id="cc804-133">Společnosti Adventure Works Cycles je pomocí své podnikové předplatné s názvem společnosti Adventure Works podnikové předplatné, tooprovide fakturace pro tuto úlohu IT.</span><span class="sxs-lookup"><span data-stu-id="cc804-133">Adventure Works Cycles is using their Enterprise subscription, named Adventure Works Enterprise Subscription, tooprovide billing for this IT workload.</span></span>

## <a name="storage"></a><span data-ttu-id="cc804-134">Úložiště</span><span class="sxs-lookup"><span data-stu-id="cc804-134">Storage</span></span>
<span data-ttu-id="cc804-135">Společnosti Adventure Works Cycles určit, že by měli používat Azure spravované disky.</span><span class="sxs-lookup"><span data-stu-id="cc804-135">Adventure Works Cycles determined that they should use Azure Managed Disks.</span></span> <span data-ttu-id="cc804-136">Při vytváření virtuálních počítačů, se používají i vrstvy úložiště k dispozici úložiště:</span><span class="sxs-lookup"><span data-stu-id="cc804-136">When creating VMs, both storage available storage tiers are used:</span></span>

* <span data-ttu-id="cc804-137">**Standardní úložiště** pro hello webové servery, aplikační servery a řadiče domény a jejich datových disků.</span><span class="sxs-lookup"><span data-stu-id="cc804-137">**Standard storage** for hello web servers, application servers, and domain controllers and their data disks.</span></span>
* <span data-ttu-id="cc804-138">**Storage úrovně Premium** hello MongoDB horizontálně dělené clusteru serverů a jejich datových disků.</span><span class="sxs-lookup"><span data-stu-id="cc804-138">**Premium storage** for hello MongoDB sharded cluster servers and their data disks.</span></span>

## <a name="virtual-network-and-subnets"></a><span data-ttu-id="cc804-139">Virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="cc804-139">Virtual network and subnets</span></span>
<span data-ttu-id="cc804-140">Protože hello virtuální sítě nemusí probíhající připojení toohello Adventure pracovní Cycles do místní sítě, budou se rozhodla ve virtuální síti jenom pro cloud.</span><span class="sxs-lookup"><span data-stu-id="cc804-140">Because hello virtual network does not need ongoing connectivity toohello Adventure Work Cycles on-premises network, they decided on a cloud-only virtual network.</span></span>

<span data-ttu-id="cc804-141">Vytvářely jenom pro cloud virtuální síť s hello následující nastavení pomocí portálu Azure hello:</span><span class="sxs-lookup"><span data-stu-id="cc804-141">They created a cloud-only virtual network with hello following settings using hello Azure portal:</span></span>

* <span data-ttu-id="cc804-142">Název: AZOS-použití VN01</span><span class="sxs-lookup"><span data-stu-id="cc804-142">Name: AZOS-USE-VN01</span></span>
* <span data-ttu-id="cc804-143">Umístění: Východní USA 2</span><span class="sxs-lookup"><span data-stu-id="cc804-143">Location: East US 2</span></span>
* <span data-ttu-id="cc804-144">Adresní prostor virtuální sítě: 10.0.0.0/8</span><span class="sxs-lookup"><span data-stu-id="cc804-144">Virtual network address space: 10.0.0.0/8</span></span>
* <span data-ttu-id="cc804-145">První podsíť:</span><span class="sxs-lookup"><span data-stu-id="cc804-145">First subnet:</span></span>
  * <span data-ttu-id="cc804-146">Název: front-endu</span><span class="sxs-lookup"><span data-stu-id="cc804-146">Name: FrontEnd</span></span>
  * <span data-ttu-id="cc804-147">Adresní prostor: 10.0.1.0/24</span><span class="sxs-lookup"><span data-stu-id="cc804-147">Address space: 10.0.1.0/24</span></span>
* <span data-ttu-id="cc804-148">Druhou podsíť:</span><span class="sxs-lookup"><span data-stu-id="cc804-148">Second subnet:</span></span>
  * <span data-ttu-id="cc804-149">Název: back-end</span><span class="sxs-lookup"><span data-stu-id="cc804-149">Name: BackEnd</span></span>
  * <span data-ttu-id="cc804-150">Adresní prostor: 10.0.2.0/24</span><span class="sxs-lookup"><span data-stu-id="cc804-150">Address space: 10.0.2.0/24</span></span>

## <a name="availability-sets"></a><span data-ttu-id="cc804-151">Skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="cc804-151">Availability sets</span></span>
<span data-ttu-id="cc804-152">toomaintain vysokou dostupnost všechny čtyři úrovně jejich online úložiště společnosti Adventure Works Cycles rozhodla na čtyři skupiny dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="cc804-152">toomaintain high availability of all four tiers of their on-line store, Adventure Works Cycles decided on four availability sets:</span></span>

* <span data-ttu-id="cc804-153">**azos použít jako webový** pro webové servery hello</span><span class="sxs-lookup"><span data-stu-id="cc804-153">**azos-use-as-web** for hello web servers</span></span>
* <span data-ttu-id="cc804-154">**azos používání jako aplikace** pro hello aplikační servery</span><span class="sxs-lookup"><span data-stu-id="cc804-154">**azos-use-as-app** for hello application servers</span></span>
* <span data-ttu-id="cc804-155">**azos použijte jako db** pro hello servery v clusteru horizontálně dělené MongoDB hello</span><span class="sxs-lookup"><span data-stu-id="cc804-155">**azos-use-as-db** for hello servers in hello MongoDB sharded cluster</span></span>
* <span data-ttu-id="cc804-156">**azos použijte jako dc** pro řadiče domény hello</span><span class="sxs-lookup"><span data-stu-id="cc804-156">**azos-use-as-dc** for hello domain controllers</span></span>

## <a name="virtual-machines"></a><span data-ttu-id="cc804-157">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="cc804-157">Virtual machines</span></span>
<span data-ttu-id="cc804-158">Společnosti Adventure Works Cycles se rozhodli hello následující názvy pro své virtuální počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="cc804-158">Adventure Works Cycles decided on hello following names for their Azure VMs:</span></span>

* <span data-ttu-id="cc804-159">**azos použití virtuálních počítačů web01** pro první webový server hello</span><span class="sxs-lookup"><span data-stu-id="cc804-159">**azos-use-vm-web01** for hello first web server</span></span>
* <span data-ttu-id="cc804-160">**azos použití virtuálních počítačů web02** pro hello druhého webového serveru</span><span class="sxs-lookup"><span data-stu-id="cc804-160">**azos-use-vm-web02** for hello second web server</span></span>
* <span data-ttu-id="cc804-161">**azos použití virtuálních počítačů app01** pro první aplikační server hello</span><span class="sxs-lookup"><span data-stu-id="cc804-161">**azos-use-vm-app01** for hello first application server</span></span>
* <span data-ttu-id="cc804-162">**azos použití virtuálních počítačů app02** pro druhý server aplikace hello</span><span class="sxs-lookup"><span data-stu-id="cc804-162">**azos-use-vm-app02** for hello second application server</span></span>
* <span data-ttu-id="cc804-163">**azos použití virtuálních počítačů db01** hello první MongoDB serveru v clusteru hello</span><span class="sxs-lookup"><span data-stu-id="cc804-163">**azos-use-vm-db01** for hello first MongoDB server in hello cluster</span></span>
* <span data-ttu-id="cc804-164">**azos použití virtuálních počítačů db02** hello druhý MongoDB serveru v clusteru hello</span><span class="sxs-lookup"><span data-stu-id="cc804-164">**azos-use-vm-db02** for hello second MongoDB server in hello cluster</span></span>
* <span data-ttu-id="cc804-165">**azos použití virtuálních počítačů dc01** pro první řadič domény hello</span><span class="sxs-lookup"><span data-stu-id="cc804-165">**azos-use-vm-dc01** for hello first domain controller</span></span>
* <span data-ttu-id="cc804-166">**azos použití virtuálních počítačů dc02** pro řadič domény druhé hello</span><span class="sxs-lookup"><span data-stu-id="cc804-166">**azos-use-vm-dc02** for hello second domain controller</span></span>

<span data-ttu-id="cc804-167">Zde je hello Výsledná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cc804-167">Here is hello resulting configuration.</span></span>

![Infrastruktura konečné aplikace nasazené v Azure](./media/infrastructure-example/example-config.png)

<span data-ttu-id="cc804-169">Tato konfigurace zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="cc804-169">This configuration incorporates:</span></span>

* <span data-ttu-id="cc804-170">Čistě cloudové virtuální síť se dvěma podsítěmi (front-endové a back-end)</span><span class="sxs-lookup"><span data-stu-id="cc804-170">A cloud-only virtual network with two subnets (FrontEnd and BackEnd)</span></span>
* <span data-ttu-id="cc804-171">Spravované disky systému Azure pomocí disků Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="cc804-171">Azure Managed Disks using both Standard and Premium disks</span></span>
* <span data-ttu-id="cc804-172">Čtyři skupiny dostupnosti, jeden pro každou vrstvu úložiště online hello</span><span class="sxs-lookup"><span data-stu-id="cc804-172">Four availability sets, one for each tier of hello on-line store</span></span>
* <span data-ttu-id="cc804-173">Hello virtuálních počítačů pro hello čtyři úrovně</span><span class="sxs-lookup"><span data-stu-id="cc804-173">hello virtual machines for hello four tiers</span></span>
* <span data-ttu-id="cc804-174">Externí s vyrovnáváním zatížení založený na protokolu HTTPS webových přenosů z hello Internet toohello webové servery</span><span class="sxs-lookup"><span data-stu-id="cc804-174">An external load balanced set for HTTPS-based web traffic from hello Internet toohello web servers</span></span>
* <span data-ttu-id="cc804-175">K interní s vyrovnáváním zatížení nezašifrované webových přenosů z hello webové servery toohello aplikačních serverů</span><span class="sxs-lookup"><span data-stu-id="cc804-175">An internal load balanced set for unencrypted web traffic from hello web servers toohello application servers</span></span>
* <span data-ttu-id="cc804-176">Jedna skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="cc804-176">A single resource group</span></span>
