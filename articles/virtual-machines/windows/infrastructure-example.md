---
title: "aaaExample návod infrastruktury Azure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro nasazení infrastruktury příklad v Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7032b586-e4e5-4954-952f-fdfc03fc1980
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd6b6e904404bea0b5be37dfce6d60039daae636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a><span data-ttu-id="40868-103">Příklad infrastruktury Azure návod pro virtuální počítače Windows</span><span class="sxs-lookup"><span data-stu-id="40868-103">Example Azure infrastructure walkthrough for Windows VMs</span></span>

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

<span data-ttu-id="40868-104">Tento článek vás provede vytváření infrastruktury příklad aplikace.</span><span class="sxs-lookup"><span data-stu-id="40868-104">This article walks through building out an example application infrastructure.</span></span> <span data-ttu-id="40868-105">Jsme podrobnosti navrhování infrastruktury pro jednoduché online obchodu, která spojuje všechny hello pokyny a rozhodnutí, která kolem názvů, skupiny dostupnosti, virtuální sítě a nástroje pro vyrovnávání zatížení a ve skutečnosti nasazení virtuálních počítačů (VM).</span><span class="sxs-lookup"><span data-stu-id="40868-105">We detail designing an infrastructure for a simple on-line store that brings together all hello guidelines and decisions around naming conventions, availability sets, virtual networks and load balancers, and actually deploying your virtual machines (VMs).</span></span>

## <a name="example-workload"></a><span data-ttu-id="40868-106">Příklad úloh</span><span class="sxs-lookup"><span data-stu-id="40868-106">Example workload</span></span>
<span data-ttu-id="40868-107">Společnosti Adventure Works Cycles chce toobuild aplikace online úložiště v Azure, který se skládá z:</span><span class="sxs-lookup"><span data-stu-id="40868-107">Adventure Works Cycles wants toobuild an on-line store application in Azure that consists of:</span></span>

* <span data-ttu-id="40868-108">Dva servery služby IIS se spuštěnou hello klienta front-endu v webovou vrstvu</span><span class="sxs-lookup"><span data-stu-id="40868-108">Two IIS servers running hello client front-end in a web tier</span></span>
* <span data-ttu-id="40868-109">Zpracování dat a objednávky v aplikační vrstvě dva servery služby IIS</span><span class="sxs-lookup"><span data-stu-id="40868-109">Two IIS servers processing data and orders in an application tier</span></span>
* <span data-ttu-id="40868-110">Dvě instance Microsoft SQL Server se skupinami dostupnosti AlwaysOn (dva servery SQL a určujícího uzlu většina) pro ukládání dat produktu a objednávky v databázové vrstvy</span><span class="sxs-lookup"><span data-stu-id="40868-110">Two Microsoft SQL Server instances with AlwaysOn availability groups (two SQL Servers and a majority node witness) for storing product data and orders in a database tier</span></span>
* <span data-ttu-id="40868-111">Dva řadiče domény služby Active Directory pro účty zákazníků a všichni dodavatelé v vrstvou ověřování</span><span class="sxs-lookup"><span data-stu-id="40868-111">Two Active Directory domain controllers for customer accounts and suppliers in an authentication tier</span></span>
* <span data-ttu-id="40868-112">Všechny servery hello se nacházejí v dvě podsítě:</span><span class="sxs-lookup"><span data-stu-id="40868-112">All hello servers are located in two subnets:</span></span>
  * <span data-ttu-id="40868-113">podsíť front-end pro webové servery hello</span><span class="sxs-lookup"><span data-stu-id="40868-113">a front-end subnet for hello web servers</span></span> 
  * <span data-ttu-id="40868-114">back-end podsíť pro aplikační servery hello clusteru SQL a řadiče domény</span><span class="sxs-lookup"><span data-stu-id="40868-114">a back-end subnet for hello application servers, SQL cluster, and domain controllers</span></span>

![Diagram různých vrstev pro infrastrukturu aplikace](./media/infrastructure-example/example-tiers.png)

<span data-ttu-id="40868-116">Zabezpečené příchozí webové přenosy musí být vyrovnávání zatížení mezi hello webové servery jako zákazníci procházet hello online úložiště.</span><span class="sxs-lookup"><span data-stu-id="40868-116">Incoming secure web traffic must be load-balanced among hello web servers as customers browse hello on-line store.</span></span> <span data-ttu-id="40868-117">Pořadí zpracování přenosů dat ve formuláři hello HTTP požadavků z webové hello, servery musí být rozložena mezi hello aplikační servery.</span><span class="sxs-lookup"><span data-stu-id="40868-117">Order processing traffic in hello form of HTTP requests from hello web servers must be balanced among hello application servers.</span></span> <span data-ttu-id="40868-118">Kromě toho musí být navrženy hello infrastruktury pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="40868-118">Additionally, hello infrastructure must be designed for high availability.</span></span>

<span data-ttu-id="40868-119">Výsledný návrhu Hello musí obsahovat:</span><span class="sxs-lookup"><span data-stu-id="40868-119">hello resulting design must incorporate:</span></span>

* <span data-ttu-id="40868-120">Předplatné Azure a účet</span><span class="sxs-lookup"><span data-stu-id="40868-120">An Azure subscription and account</span></span>
* <span data-ttu-id="40868-121">Jedna skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="40868-121">A single resource group</span></span>
* <span data-ttu-id="40868-122">Azure Managed Disks</span><span class="sxs-lookup"><span data-stu-id="40868-122">Azure Managed Disks</span></span>
* <span data-ttu-id="40868-123">Virtuální síť se dvěma podsítěmi</span><span class="sxs-lookup"><span data-stu-id="40868-123">A virtual network with two subnets</span></span>
* <span data-ttu-id="40868-124">Sady dostupnosti pro hello virtuálních počítačů s podobnou roli</span><span class="sxs-lookup"><span data-stu-id="40868-124">Availability sets for hello VMs with a similar role</span></span>
* <span data-ttu-id="40868-125">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="40868-125">Virtual machines</span></span>

<span data-ttu-id="40868-126">Všechny výše uvedené hello postupujte tyto zásady vytváření názvů:</span><span class="sxs-lookup"><span data-stu-id="40868-126">All hello above follow these naming conventions:</span></span>

* <span data-ttu-id="40868-127">Adventure Works Cycles používá **[IT zatížení]-[umístění] – [prostředků Azure]** jako předponu</span><span class="sxs-lookup"><span data-stu-id="40868-127">Adventure Works Cycles uses **[IT workload]-[location]-[Azure resource]** as a prefix</span></span>
  * <span data-ttu-id="40868-128">V tomto příkladu "**azos**" (online úložiště Azure) je hello název úlohy IT a "**použít**" (východní USA 2) je umístění hello</span><span class="sxs-lookup"><span data-stu-id="40868-128">For this example, "**azos**" (Azure On-line Store) is hello IT workload name and "**use**" (East US 2) is hello location</span></span>
* <span data-ttu-id="40868-129">Virtuální sítě pomocí AZOS. POUŽIJTE VN**[číslo]**</span><span class="sxs-lookup"><span data-stu-id="40868-129">Virtual networks use AZOS-USE-VN**[number]**</span></span>
* <span data-ttu-id="40868-130">Pomocí sad dostupnosti azos-použít-jako-**[role]**</span><span class="sxs-lookup"><span data-stu-id="40868-130">Availability sets use azos-use-as-**[role]**</span></span>
* <span data-ttu-id="40868-131">Názvy virtuálních počítačů pomocí azos-použít-vm -**[vmname]**</span><span class="sxs-lookup"><span data-stu-id="40868-131">Virtual machine names use azos-use-vm-**[vmname]**</span></span>

## <a name="azure-subscriptions-and-accounts"></a><span data-ttu-id="40868-132">Předplatná Azure a účty</span><span class="sxs-lookup"><span data-stu-id="40868-132">Azure subscriptions and accounts</span></span>
<span data-ttu-id="40868-133">Společnosti Adventure Works Cycles je pomocí své podnikové předplatné s názvem společnosti Adventure Works podnikové předplatné, tooprovide fakturace pro tuto úlohu IT.</span><span class="sxs-lookup"><span data-stu-id="40868-133">Adventure Works Cycles is using their Enterprise subscription, named Adventure Works Enterprise Subscription, tooprovide billing for this IT workload.</span></span>

## <a name="storage"></a><span data-ttu-id="40868-134">Úložiště</span><span class="sxs-lookup"><span data-stu-id="40868-134">Storage</span></span>
<span data-ttu-id="40868-135">Společnosti Adventure Works Cycles určit, že by měli používat Azure spravované disky.</span><span class="sxs-lookup"><span data-stu-id="40868-135">Adventure Works Cycles determined that they should use Azure Managed Disks.</span></span> <span data-ttu-id="40868-136">Při vytváření virtuálních počítačů, se používají i vrstvy úložiště k dispozici úložiště:</span><span class="sxs-lookup"><span data-stu-id="40868-136">When creating VMs, both storage available storage tiers are used:</span></span>

* <span data-ttu-id="40868-137">**Standardní úložiště** pro hello webové servery, aplikační servery a řadiče domény a jejich datových disků.</span><span class="sxs-lookup"><span data-stu-id="40868-137">**Standard storage** for hello web servers, application servers, and domain controllers and their data disks.</span></span>
* <span data-ttu-id="40868-138">**Storage úrovně Premium** pro virtuální počítače hello SQL serveru a jejich datových disků.</span><span class="sxs-lookup"><span data-stu-id="40868-138">**Premium storage** for hello SQL Server VMs and their data disks.</span></span>

## <a name="virtual-network-and-subnets"></a><span data-ttu-id="40868-139">Virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="40868-139">Virtual network and subnets</span></span>
<span data-ttu-id="40868-140">Protože hello virtuální sítě nemusí probíhající připojení toohello Adventure pracovní Cycles do místní sítě, budou se rozhodla ve virtuální síti jenom pro cloud.</span><span class="sxs-lookup"><span data-stu-id="40868-140">Because hello virtual network does not need ongoing connectivity toohello Adventure Work Cycles on-premises network, they decided on a cloud-only virtual network.</span></span>

<span data-ttu-id="40868-141">Vytvářely jenom pro cloud virtuální síť s hello následující nastavení pomocí portálu Azure hello:</span><span class="sxs-lookup"><span data-stu-id="40868-141">They created a cloud-only virtual network with hello following settings using hello Azure portal:</span></span>

* <span data-ttu-id="40868-142">Název: AZOS-použití VN01</span><span class="sxs-lookup"><span data-stu-id="40868-142">Name: AZOS-USE-VN01</span></span>
* <span data-ttu-id="40868-143">Umístění: Východní USA 2</span><span class="sxs-lookup"><span data-stu-id="40868-143">Location: East US 2</span></span>
* <span data-ttu-id="40868-144">Adresní prostor virtuální sítě: 10.0.0.0/8</span><span class="sxs-lookup"><span data-stu-id="40868-144">Virtual network address space: 10.0.0.0/8</span></span>
* <span data-ttu-id="40868-145">První podsíť:</span><span class="sxs-lookup"><span data-stu-id="40868-145">First subnet:</span></span>
  * <span data-ttu-id="40868-146">Název: front-endu</span><span class="sxs-lookup"><span data-stu-id="40868-146">Name: FrontEnd</span></span>
  * <span data-ttu-id="40868-147">Adresní prostor: 10.0.1.0/24</span><span class="sxs-lookup"><span data-stu-id="40868-147">Address space: 10.0.1.0/24</span></span>
* <span data-ttu-id="40868-148">Druhou podsíť:</span><span class="sxs-lookup"><span data-stu-id="40868-148">Second subnet:</span></span>
  * <span data-ttu-id="40868-149">Název: back-end</span><span class="sxs-lookup"><span data-stu-id="40868-149">Name: BackEnd</span></span>
  * <span data-ttu-id="40868-150">Adresní prostor: 10.0.2.0/24</span><span class="sxs-lookup"><span data-stu-id="40868-150">Address space: 10.0.2.0/24</span></span>

## <a name="availability-sets"></a><span data-ttu-id="40868-151">Skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="40868-151">Availability sets</span></span>
<span data-ttu-id="40868-152">toomaintain vysokou dostupnost všechny čtyři úrovně jejich online úložiště společnosti Adventure Works Cycles rozhodla na čtyři skupiny dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="40868-152">toomaintain high availability of all four tiers of their on-line store, Adventure Works Cycles decided on four availability sets:</span></span>

* <span data-ttu-id="40868-153">**azos použít jako webový** pro webové servery hello</span><span class="sxs-lookup"><span data-stu-id="40868-153">**azos-use-as-web** for hello web servers</span></span>
* <span data-ttu-id="40868-154">**azos používání jako aplikace** pro hello aplikační servery</span><span class="sxs-lookup"><span data-stu-id="40868-154">**azos-use-as-app** for hello application servers</span></span>
* <span data-ttu-id="40868-155">**azos použijte jako sql** pro servery SQL Server hello</span><span class="sxs-lookup"><span data-stu-id="40868-155">**azos-use-as-sql** for hello SQL Servers</span></span>
* <span data-ttu-id="40868-156">**azos použijte jako dc** pro řadiče domény hello</span><span class="sxs-lookup"><span data-stu-id="40868-156">**azos-use-as-dc** for hello domain controllers</span></span>

## <a name="virtual-machines"></a><span data-ttu-id="40868-157">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="40868-157">Virtual machines</span></span>
<span data-ttu-id="40868-158">Společnosti Adventure Works Cycles se rozhodli hello následující názvy pro své virtuální počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="40868-158">Adventure Works Cycles decided on hello following names for their Azure VMs:</span></span>

* <span data-ttu-id="40868-159">**azos použití virtuálních počítačů web01** pro první webový server hello</span><span class="sxs-lookup"><span data-stu-id="40868-159">**azos-use-vm-web01** for hello first web server</span></span>
* <span data-ttu-id="40868-160">**azos použití virtuálních počítačů web02** pro hello druhého webového serveru</span><span class="sxs-lookup"><span data-stu-id="40868-160">**azos-use-vm-web02** for hello second web server</span></span>
* <span data-ttu-id="40868-161">**azos použití virtuálních počítačů app01** pro první aplikační server hello</span><span class="sxs-lookup"><span data-stu-id="40868-161">**azos-use-vm-app01** for hello first application server</span></span>
* <span data-ttu-id="40868-162">**azos použití virtuálních počítačů app02** pro druhý server aplikace hello</span><span class="sxs-lookup"><span data-stu-id="40868-162">**azos-use-vm-app02** for hello second application server</span></span>
* <span data-ttu-id="40868-163">**azos použití virtuálních počítačů sql01** pro hello první server pro systém SQL Server v clusteru hello</span><span class="sxs-lookup"><span data-stu-id="40868-163">**azos-use-vm-sql01** for hello first SQL Server server in hello cluster</span></span>
* <span data-ttu-id="40868-164">**azos použití virtuálních počítačů sql02** pro hello druhý server SQL Server v clusteru hello</span><span class="sxs-lookup"><span data-stu-id="40868-164">**azos-use-vm-sql02** for hello second SQL Server server in hello cluster</span></span>
* <span data-ttu-id="40868-165">**azos použití virtuálních počítačů dc01** pro první řadič domény hello</span><span class="sxs-lookup"><span data-stu-id="40868-165">**azos-use-vm-dc01** for hello first domain controller</span></span>
* <span data-ttu-id="40868-166">**azos použití virtuálních počítačů dc02** pro řadič domény druhé hello</span><span class="sxs-lookup"><span data-stu-id="40868-166">**azos-use-vm-dc02** for hello second domain controller</span></span>

<span data-ttu-id="40868-167">Zde je hello Výsledná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="40868-167">Here is hello resulting configuration.</span></span>

![Infrastruktura konečné aplikace nasazené v Azure](./media/infrastructure-example/example-config.png)

<span data-ttu-id="40868-169">Tato konfigurace zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="40868-169">This configuration incorporates:</span></span>

* <span data-ttu-id="40868-170">Čistě cloudové virtuální síť se dvěma podsítěmi (front-endové a back-end)</span><span class="sxs-lookup"><span data-stu-id="40868-170">A cloud-only virtual network with two subnets (FrontEnd and BackEnd)</span></span>
* <span data-ttu-id="40868-171">Disky Azure spravované s disky, Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="40868-171">Azure Managed Disks with both Standard and Premium disks</span></span>
* <span data-ttu-id="40868-172">Čtyři skupiny dostupnosti, jeden pro každou vrstvu úložiště online hello</span><span class="sxs-lookup"><span data-stu-id="40868-172">Four availability sets, one for each tier of hello on-line store</span></span>
* <span data-ttu-id="40868-173">Hello virtuálních počítačů pro hello čtyři úrovně</span><span class="sxs-lookup"><span data-stu-id="40868-173">hello virtual machines for hello four tiers</span></span>
* <span data-ttu-id="40868-174">Externí s vyrovnáváním zatížení založený na protokolu HTTPS webových přenosů z hello Internet toohello webové servery</span><span class="sxs-lookup"><span data-stu-id="40868-174">An external load balanced set for HTTPS-based web traffic from hello Internet toohello web servers</span></span>
* <span data-ttu-id="40868-175">K interní s vyrovnáváním zatížení nezašifrované webových přenosů z hello webové servery toohello aplikačních serverů</span><span class="sxs-lookup"><span data-stu-id="40868-175">An internal load balanced set for unencrypted web traffic from hello web servers toohello application servers</span></span>
* <span data-ttu-id="40868-176">Jedna skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="40868-176">A single resource group</span></span>