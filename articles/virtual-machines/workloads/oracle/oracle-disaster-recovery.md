---
title: "Přehled Oracle zotavení po havárii v prostředí Azure | Microsoft Docs"
description: "Na scénář zotavení po havárii pro databázi Oracle Database 12c v prostředí Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/2/2017
ms.author: rclaus
ms.openlocfilehash: f17ebb2b74cd7ad872f88483ed7cdb4f239ee069
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a><span data-ttu-id="f10da-103">Zotavení po havárii pro databázi Oracle Database 12c v prostředí Azure</span><span class="sxs-lookup"><span data-stu-id="f10da-103">Disaster recovery for an Oracle Database 12c database in an Azure environment</span></span>

## <a name="assumptions"></a><span data-ttu-id="f10da-104">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="f10da-104">Assumptions</span></span>

- <span data-ttu-id="f10da-105">Máte představu o Oracle Data Guard návrhu a prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="f10da-105">You have an understanding of Oracle Data Guard design and Azure environments.</span></span>


## <a name="goals"></a><span data-ttu-id="f10da-106">Cíle</span><span class="sxs-lookup"><span data-stu-id="f10da-106">Goals</span></span>
- <span data-ttu-id="f10da-107">Návrh topologie a konfigurace, které splňují vaše požadavky na zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="f10da-107">Design the topology and configuration that meet your disaster recovery (DR) requirements.</span></span>

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a><span data-ttu-id="f10da-108">Scénář 1: Primární a zotavení po Havárii weby na Azure</span><span class="sxs-lookup"><span data-stu-id="f10da-108">Scenario 1: Primary and DR sites on Azure</span></span>

<span data-ttu-id="f10da-109">Zákazník má Oracle databáze sadu nahoru v primární lokalitě.</span><span class="sxs-lookup"><span data-stu-id="f10da-109">A customer has an Oracle database set up on the primary site.</span></span> <span data-ttu-id="f10da-110">Zotavení po Havárii A lokalita je v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="f10da-110">A DR site is in a different region.</span></span> <span data-ttu-id="f10da-111">Zákazník používá Oracle Data Guard pro rychlé obnovení mezi těmito lokalitami.</span><span class="sxs-lookup"><span data-stu-id="f10da-111">The customer uses Oracle Data Guard for quick recovery between these sites.</span></span> <span data-ttu-id="f10da-112">Primární lokalita má také sekundární databáze pro vytváření sestav a jiné účely.</span><span class="sxs-lookup"><span data-stu-id="f10da-112">The primary site also has a secondary database for reporting and other uses.</span></span> 

### <a name="topology"></a><span data-ttu-id="f10da-113">topologie</span><span class="sxs-lookup"><span data-stu-id="f10da-113">Topology</span></span>

<span data-ttu-id="f10da-114">Zde je souhrn nastavení Azure:</span><span class="sxs-lookup"><span data-stu-id="f10da-114">Here is a summary of the Azure setup:</span></span>

- <span data-ttu-id="f10da-115">Dvěma lokalitami (primární lokalitou a lokalitou zotavení po Havárii)</span><span class="sxs-lookup"><span data-stu-id="f10da-115">Two sites (a primary site and a DR site)</span></span>
- <span data-ttu-id="f10da-116">Dvě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f10da-116">Two virtual networks</span></span>
- <span data-ttu-id="f10da-117">Dvě databáze Oracle se Data Guard (primární i pohotovostní režim)</span><span class="sxs-lookup"><span data-stu-id="f10da-117">Two Oracle databases with Data Guard (primary and standby)</span></span>
- <span data-ttu-id="f10da-118">Dvě databáze Oracle se Golden brány nebo Data Guard (pouze primární lokalita)</span><span class="sxs-lookup"><span data-stu-id="f10da-118">Two Oracle databases with Golden Gate or Data Guard (primary site only)</span></span>
- <span data-ttu-id="f10da-119">Dvě aplikace služby, jeden primární a jeden v lokalitě zotavení po Havárii</span><span class="sxs-lookup"><span data-stu-id="f10da-119">Two application services, one primary and one on the DR site</span></span>
- <span data-ttu-id="f10da-120">*Nastavení dostupnosti,* sloužící pro databázi a aplikace služby v primární lokalitě</span><span class="sxs-lookup"><span data-stu-id="f10da-120">An *availability set,* which is used for database and application service on the primary site</span></span>
- <span data-ttu-id="f10da-121">Jeden jumpbox v každé lokalitě, která omezuje přístup k privátní síti a umožňuje pouze přihlášení správce</span><span class="sxs-lookup"><span data-stu-id="f10da-121">One jumpbox on each site, which restricts access to the private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="f10da-122">Jumpbox, služba aplikace, databáze a brány VPN oddělené podsítě</span><span class="sxs-lookup"><span data-stu-id="f10da-122">A jumpbox, application service, database, and VPN gateway on separate subnets</span></span>
- <span data-ttu-id="f10da-123">Skupina NSG vynucená u aplikace a databáze podsítě</span><span class="sxs-lookup"><span data-stu-id="f10da-123">NSG enforced on application and database subnets</span></span>

![Snímek obrazovky stránky topologie zotavení po Havárii](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a><span data-ttu-id="f10da-125">Scénář 2: Primární lokality na místě a zotavení po Havárii webu v Azure</span><span class="sxs-lookup"><span data-stu-id="f10da-125">Scenario 2: Primary site on-premises and DR site on Azure</span></span>

<span data-ttu-id="f10da-126">Zákazník má nastavení databáze Oracle místní (primární lokalita).</span><span class="sxs-lookup"><span data-stu-id="f10da-126">A customer has an on-premises Oracle database setup (primary site).</span></span> <span data-ttu-id="f10da-127">Zotavení po Havárii A lokalita je v Azure.</span><span class="sxs-lookup"><span data-stu-id="f10da-127">A DR site is on Azure.</span></span> <span data-ttu-id="f10da-128">Oracle Data Guard se používá pro rychlé obnovení mezi těmito lokalitami.</span><span class="sxs-lookup"><span data-stu-id="f10da-128">Oracle Data Guard is used for quick recovery between these sites.</span></span> <span data-ttu-id="f10da-129">Primární lokalita má také sekundární databáze pro vytváření sestav a jiné účely.</span><span class="sxs-lookup"><span data-stu-id="f10da-129">The primary site also has a secondary database for reporting and other uses.</span></span> 

<span data-ttu-id="f10da-130">Existují dva přístupy pro tento instalační program.</span><span class="sxs-lookup"><span data-stu-id="f10da-130">There are two approaches for this setup.</span></span>

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-the-firewall"></a><span data-ttu-id="f10da-131">Způsob 1: Přímé připojení mezi místními a Azure, vyžadování otevřené porty protokolu TCP v bráně firewall.</span><span class="sxs-lookup"><span data-stu-id="f10da-131">Approach 1: Direct connections between on-premises and Azure, requiring open TCP ports on the firewall</span></span> 

<span data-ttu-id="f10da-132">Přímé připojení nedoporučujeme, protože potenciálně zobrazovat porty TCP k vnějším světem.</span><span class="sxs-lookup"><span data-stu-id="f10da-132">We don't recommend direct connections because they expose the TCP ports to the outside world.</span></span>

#### <a name="topology"></a><span data-ttu-id="f10da-133">topologie</span><span class="sxs-lookup"><span data-stu-id="f10da-133">Topology</span></span>

<span data-ttu-id="f10da-134">Následuje souhrn nastavení Azure:</span><span class="sxs-lookup"><span data-stu-id="f10da-134">Following is a summary of the Azure setup:</span></span>

- <span data-ttu-id="f10da-135">Jeden DR lokality</span><span class="sxs-lookup"><span data-stu-id="f10da-135">One DR site</span></span> 
- <span data-ttu-id="f10da-136">Jedna virtuální síť</span><span class="sxs-lookup"><span data-stu-id="f10da-136">One virtual network</span></span>
- <span data-ttu-id="f10da-137">Jedna databáze Oracle s Data Guard (aktivní)</span><span class="sxs-lookup"><span data-stu-id="f10da-137">One Oracle database with Data Guard (active)</span></span>
- <span data-ttu-id="f10da-138">Služba jednu aplikaci na webu zotavení po Havárii</span><span class="sxs-lookup"><span data-stu-id="f10da-138">One application service on the DR site</span></span>
- <span data-ttu-id="f10da-139">Jeden jumpbox, která omezuje přístup k privátní síti a umožňuje pouze přihlášení správce</span><span class="sxs-lookup"><span data-stu-id="f10da-139">One jumpbox, which restricts access to the private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="f10da-140">Jumpbox, služba aplikace, databáze a brány VPN oddělené podsítě</span><span class="sxs-lookup"><span data-stu-id="f10da-140">A jumpbox, application service, database, and VPN gateway on separate subnets</span></span>
- <span data-ttu-id="f10da-141">Skupina NSG vynucená u aplikace a databáze podsítě</span><span class="sxs-lookup"><span data-stu-id="f10da-141">NSG enforced on application and database subnets</span></span>
- <span data-ttu-id="f10da-142">Skupina NSG zásad nebo pravidlo povolit příchozí port TCP 1521 (nebo na uživatelem definované port)</span><span class="sxs-lookup"><span data-stu-id="f10da-142">An NSG policy/rule to allow inbound TCP port 1521 (or a user-defined port)</span></span>
- <span data-ttu-id="f10da-143">Skupina NSG zásad nebo pravidlo omezit pouze IP adresa nebo adresy místní (DB nebo aplikace) pro přístup k virtuální síti</span><span class="sxs-lookup"><span data-stu-id="f10da-143">An NSG policy/rule to restrict only the IP address/addresses on-premises (DB or application) to access the virtual network</span></span>

![Snímek obrazovky stránky topologie zotavení po Havárii](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a><span data-ttu-id="f10da-145">Způsob 2: Site-to-site VPN</span><span class="sxs-lookup"><span data-stu-id="f10da-145">Approach 2: Site-to-site VPN</span></span>
<span data-ttu-id="f10da-146">Site-to-site VPN je lepší přístup.</span><span class="sxs-lookup"><span data-stu-id="f10da-146">Site-to-site VPN is a better approach.</span></span> <span data-ttu-id="f10da-147">Další informace o nastavení sítě VPN najdete v tématu [vytvoření virtuální sítě pomocí připojení Site-to-Site VPN pomocí rozhraní příkazového řádku](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span><span class="sxs-lookup"><span data-stu-id="f10da-147">For more information about setting up a VPN, see [Create a virtual network with a Site-to-Site VPN connection using CLI](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span></span>

#### <a name="topology"></a><span data-ttu-id="f10da-148">topologie</span><span class="sxs-lookup"><span data-stu-id="f10da-148">Topology</span></span>

<span data-ttu-id="f10da-149">Následuje souhrn nastavení Azure:</span><span class="sxs-lookup"><span data-stu-id="f10da-149">Following is a summary of the Azure setup:</span></span>

- <span data-ttu-id="f10da-150">Jeden DR lokality</span><span class="sxs-lookup"><span data-stu-id="f10da-150">One DR site</span></span> 
- <span data-ttu-id="f10da-151">Jedna virtuální síť</span><span class="sxs-lookup"><span data-stu-id="f10da-151">One virtual network</span></span> 
- <span data-ttu-id="f10da-152">Jedna databáze Oracle s Data Guard (aktivní)</span><span class="sxs-lookup"><span data-stu-id="f10da-152">One Oracle database with Data Guard (active)</span></span>
- <span data-ttu-id="f10da-153">Služba jednu aplikaci na webu zotavení po Havárii</span><span class="sxs-lookup"><span data-stu-id="f10da-153">One application service on the DR site</span></span>
- <span data-ttu-id="f10da-154">Jeden jumpbox, která omezuje přístup k privátní síti a umožňuje pouze přihlášení správce</span><span class="sxs-lookup"><span data-stu-id="f10da-154">One jumpbox, which restricts access to the private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="f10da-155">Jumpbox, služba aplikace, databáze a brány VPN jsou v samostatných podsítích</span><span class="sxs-lookup"><span data-stu-id="f10da-155">A jumpbox, application service, database, and VPN gateway are on separate subnets</span></span>
- <span data-ttu-id="f10da-156">Skupina NSG vynucená u aplikace a databáze podsítě</span><span class="sxs-lookup"><span data-stu-id="f10da-156">NSG enforced on application and database subnets</span></span>
- <span data-ttu-id="f10da-157">Připojení Site-to-site VPN mezi místními a Azure</span><span class="sxs-lookup"><span data-stu-id="f10da-157">Site-to-site VPN connection between on-premises and Azure</span></span>

![Snímek obrazovky stránky topologie zotavení po Havárii](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a><span data-ttu-id="f10da-159">Další čtení</span><span class="sxs-lookup"><span data-stu-id="f10da-159">Additional reading</span></span>

- [<span data-ttu-id="f10da-160">Návrh a implementaci databáze Oracle na Azure</span><span class="sxs-lookup"><span data-stu-id="f10da-160">Design and implement an Oracle database on Azure</span></span>](oracle-design.md)
- [<span data-ttu-id="f10da-161">Konfigurace Oracle Data Guard</span><span class="sxs-lookup"><span data-stu-id="f10da-161">Configure Oracle Data Guard</span></span>](configure-oracle-dataguard.md)
- [<span data-ttu-id="f10da-162">Konfigurace brány Golden Oracle</span><span class="sxs-lookup"><span data-stu-id="f10da-162">Configure Oracle Golden Gate</span></span>](configure-oracle-golden-gate.md)
- [<span data-ttu-id="f10da-163">Oracle zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="f10da-163">Oracle backup and recovery</span></span>](oracle-backup-recovery.md)


## <a name="next-steps"></a><span data-ttu-id="f10da-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f10da-164">Next steps</span></span>

- [<span data-ttu-id="f10da-165">Kurz: Vytvoření vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="f10da-165">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)
- [<span data-ttu-id="f10da-166">Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f10da-166">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
