---
title: "aaaOverview Oracle zotavení po havárii v prostředí Azure | Microsoft Docs"
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
ms.openlocfilehash: 1fa69e1ba044b46b27695fec92fd9ca82df796f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a><span data-ttu-id="deb25-103">Zotavení po havárii pro databázi Oracle Database 12c v prostředí Azure</span><span class="sxs-lookup"><span data-stu-id="deb25-103">Disaster recovery for an Oracle Database 12c database in an Azure environment</span></span>

## <a name="assumptions"></a><span data-ttu-id="deb25-104">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="deb25-104">Assumptions</span></span>

- <span data-ttu-id="deb25-105">Máte představu o Oracle Data Guard návrhu a prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="deb25-105">You have an understanding of Oracle Data Guard design and Azure environments.</span></span>


## <a name="goals"></a><span data-ttu-id="deb25-106">Cíle</span><span class="sxs-lookup"><span data-stu-id="deb25-106">Goals</span></span>
- <span data-ttu-id="deb25-107">Návrh hello topologie a konfigurace, které splňují vaše požadavky na zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="deb25-107">Design hello topology and configuration that meet your disaster recovery (DR) requirements.</span></span>

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a><span data-ttu-id="deb25-108">Scénář 1: Primární a zotavení po Havárii weby na Azure</span><span class="sxs-lookup"><span data-stu-id="deb25-108">Scenario 1: Primary and DR sites on Azure</span></span>

<span data-ttu-id="deb25-109">Zákazník má Oracle databáze sadu nahoru v primární lokalitě hello.</span><span class="sxs-lookup"><span data-stu-id="deb25-109">A customer has an Oracle database set up on hello primary site.</span></span> <span data-ttu-id="deb25-110">Zotavení po Havárii A lokalita je v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="deb25-110">A DR site is in a different region.</span></span> <span data-ttu-id="deb25-111">Zákazník Hello používá Oracle Data Guard pro rychlé obnovení mezi těmito lokalitami.</span><span class="sxs-lookup"><span data-stu-id="deb25-111">hello customer uses Oracle Data Guard for quick recovery between these sites.</span></span> <span data-ttu-id="deb25-112">primární lokality Hello má také sekundární databáze pro vytváření sestav a jiné účely.</span><span class="sxs-lookup"><span data-stu-id="deb25-112">hello primary site also has a secondary database for reporting and other uses.</span></span> 

### <a name="topology"></a><span data-ttu-id="deb25-113">topologie</span><span class="sxs-lookup"><span data-stu-id="deb25-113">Topology</span></span>

<span data-ttu-id="deb25-114">Zde je souhrn hello instalace nástroje Azure:</span><span class="sxs-lookup"><span data-stu-id="deb25-114">Here is a summary of hello Azure setup:</span></span>

- <span data-ttu-id="deb25-115">Dvěma lokalitami (primární lokalitou a lokalitou zotavení po Havárii)</span><span class="sxs-lookup"><span data-stu-id="deb25-115">Two sites (a primary site and a DR site)</span></span>
- <span data-ttu-id="deb25-116">Dvě virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="deb25-116">Two virtual networks</span></span>
- <span data-ttu-id="deb25-117">Dvě databáze Oracle se Data Guard (primární i pohotovostní režim)</span><span class="sxs-lookup"><span data-stu-id="deb25-117">Two Oracle databases with Data Guard (primary and standby)</span></span>
- <span data-ttu-id="deb25-118">Dvě databáze Oracle se Golden brány nebo Data Guard (pouze primární lokalita)</span><span class="sxs-lookup"><span data-stu-id="deb25-118">Two Oracle databases with Golden Gate or Data Guard (primary site only)</span></span>
- <span data-ttu-id="deb25-119">Dvě aplikace služby, jeden primární a jeden v lokalitě hello zotavení po Havárii</span><span class="sxs-lookup"><span data-stu-id="deb25-119">Two application services, one primary and one on hello DR site</span></span>
- <span data-ttu-id="deb25-120">*Nastavení dostupnosti,* sloužící pro databázi a aplikace služby v primární lokalitě hello</span><span class="sxs-lookup"><span data-stu-id="deb25-120">An *availability set,* which is used for database and application service on hello primary site</span></span>
- <span data-ttu-id="deb25-121">Jeden jumpbox v každé lokalitě, která omezuje přístup toohello privátní síti a umožňuje pouze přihlášení správce</span><span class="sxs-lookup"><span data-stu-id="deb25-121">One jumpbox on each site, which restricts access toohello private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="deb25-122">Jumpbox, služba aplikace, databáze a brány VPN oddělené podsítě</span><span class="sxs-lookup"><span data-stu-id="deb25-122">A jumpbox, application service, database, and VPN gateway on separate subnets</span></span>
- <span data-ttu-id="deb25-123">Skupina NSG vynucená u aplikace a databáze podsítě</span><span class="sxs-lookup"><span data-stu-id="deb25-123">NSG enforced on application and database subnets</span></span>

![Snímek obrazovky stránky topologie hello zotavení po Havárii](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a><span data-ttu-id="deb25-125">Scénář 2: Primární lokality na místě a zotavení po Havárii webu v Azure</span><span class="sxs-lookup"><span data-stu-id="deb25-125">Scenario 2: Primary site on-premises and DR site on Azure</span></span>

<span data-ttu-id="deb25-126">Zákazník má nastavení databáze Oracle místní (primární lokalita).</span><span class="sxs-lookup"><span data-stu-id="deb25-126">A customer has an on-premises Oracle database setup (primary site).</span></span> <span data-ttu-id="deb25-127">Zotavení po Havárii A lokalita je v Azure.</span><span class="sxs-lookup"><span data-stu-id="deb25-127">A DR site is on Azure.</span></span> <span data-ttu-id="deb25-128">Oracle Data Guard se používá pro rychlé obnovení mezi těmito lokalitami.</span><span class="sxs-lookup"><span data-stu-id="deb25-128">Oracle Data Guard is used for quick recovery between these sites.</span></span> <span data-ttu-id="deb25-129">primární lokality Hello má také sekundární databáze pro vytváření sestav a jiné účely.</span><span class="sxs-lookup"><span data-stu-id="deb25-129">hello primary site also has a secondary database for reporting and other uses.</span></span> 

<span data-ttu-id="deb25-130">Existují dva přístupy pro tento instalační program.</span><span class="sxs-lookup"><span data-stu-id="deb25-130">There are two approaches for this setup.</span></span>

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-hello-firewall"></a><span data-ttu-id="deb25-131">Způsob 1: Přímé připojení mezi místními a Azure, vyžadování v hello firewall otevřete porty TCP</span><span class="sxs-lookup"><span data-stu-id="deb25-131">Approach 1: Direct connections between on-premises and Azure, requiring open TCP ports on hello firewall</span></span> 

<span data-ttu-id="deb25-132">Přímé připojení nedoporučujeme, protože potenciálně zobrazovat toohello porty TCP hello mimo world.</span><span class="sxs-lookup"><span data-stu-id="deb25-132">We don't recommend direct connections because they expose hello TCP ports toohello outside world.</span></span>

#### <a name="topology"></a><span data-ttu-id="deb25-133">topologie</span><span class="sxs-lookup"><span data-stu-id="deb25-133">Topology</span></span>

<span data-ttu-id="deb25-134">Následuje souhrn hello instalace nástroje Azure:</span><span class="sxs-lookup"><span data-stu-id="deb25-134">Following is a summary of hello Azure setup:</span></span>

- <span data-ttu-id="deb25-135">Jeden DR lokality</span><span class="sxs-lookup"><span data-stu-id="deb25-135">One DR site</span></span> 
- <span data-ttu-id="deb25-136">Jedna virtuální síť</span><span class="sxs-lookup"><span data-stu-id="deb25-136">One virtual network</span></span>
- <span data-ttu-id="deb25-137">Jedna databáze Oracle s Data Guard (aktivní)</span><span class="sxs-lookup"><span data-stu-id="deb25-137">One Oracle database with Data Guard (active)</span></span>
- <span data-ttu-id="deb25-138">Jednu aplikaci služby v lokalitě hello zotavení po Havárii</span><span class="sxs-lookup"><span data-stu-id="deb25-138">One application service on hello DR site</span></span>
- <span data-ttu-id="deb25-139">Jeden jumpbox, která omezuje přístup toohello privátní síti a umožňuje pouze přihlášení správce</span><span class="sxs-lookup"><span data-stu-id="deb25-139">One jumpbox, which restricts access toohello private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="deb25-140">Jumpbox, služba aplikace, databáze a brány VPN oddělené podsítě</span><span class="sxs-lookup"><span data-stu-id="deb25-140">A jumpbox, application service, database, and VPN gateway on separate subnets</span></span>
- <span data-ttu-id="deb25-141">Skupina NSG vynucená u aplikace a databáze podsítě</span><span class="sxs-lookup"><span data-stu-id="deb25-141">NSG enforced on application and database subnets</span></span>
- <span data-ttu-id="deb25-142">Tooallow zásady nebo pravidla NSG příchozí TCP port 1521 (nebo na uživatelem definované port)</span><span class="sxs-lookup"><span data-stu-id="deb25-142">An NSG policy/rule tooallow inbound TCP port 1521 (or a user-defined port)</span></span>
- <span data-ttu-id="deb25-143">Skupina NSG pravidlo pro zásady toorestrict pouze hello IP adresa nebo adresy místní (DB nebo aplikace) tooaccess hello virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="deb25-143">An NSG policy/rule toorestrict only hello IP address/addresses on-premises (DB or application) tooaccess hello virtual network</span></span>

![Snímek obrazovky stránky topologie hello zotavení po Havárii](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a><span data-ttu-id="deb25-145">Způsob 2: Site-to-site VPN</span><span class="sxs-lookup"><span data-stu-id="deb25-145">Approach 2: Site-to-site VPN</span></span>
<span data-ttu-id="deb25-146">Site-to-site VPN je lepší přístup.</span><span class="sxs-lookup"><span data-stu-id="deb25-146">Site-to-site VPN is a better approach.</span></span> <span data-ttu-id="deb25-147">Další informace o nastavení sítě VPN najdete v tématu [vytvoření virtuální sítě pomocí připojení Site-to-Site VPN pomocí rozhraní příkazového řádku](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span><span class="sxs-lookup"><span data-stu-id="deb25-147">For more information about setting up a VPN, see [Create a virtual network with a Site-to-Site VPN connection using CLI](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span></span>

#### <a name="topology"></a><span data-ttu-id="deb25-148">topologie</span><span class="sxs-lookup"><span data-stu-id="deb25-148">Topology</span></span>

<span data-ttu-id="deb25-149">Následuje souhrn hello instalace nástroje Azure:</span><span class="sxs-lookup"><span data-stu-id="deb25-149">Following is a summary of hello Azure setup:</span></span>

- <span data-ttu-id="deb25-150">Jeden DR lokality</span><span class="sxs-lookup"><span data-stu-id="deb25-150">One DR site</span></span> 
- <span data-ttu-id="deb25-151">Jedna virtuální síť</span><span class="sxs-lookup"><span data-stu-id="deb25-151">One virtual network</span></span> 
- <span data-ttu-id="deb25-152">Jedna databáze Oracle s Data Guard (aktivní)</span><span class="sxs-lookup"><span data-stu-id="deb25-152">One Oracle database with Data Guard (active)</span></span>
- <span data-ttu-id="deb25-153">Jednu aplikaci služby v lokalitě hello zotavení po Havárii</span><span class="sxs-lookup"><span data-stu-id="deb25-153">One application service on hello DR site</span></span>
- <span data-ttu-id="deb25-154">Jeden jumpbox, která omezuje přístup toohello privátní síti a umožňuje pouze přihlášení správce</span><span class="sxs-lookup"><span data-stu-id="deb25-154">One jumpbox, which restricts access toohello private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="deb25-155">Jumpbox, služba aplikace, databáze a brány VPN jsou v samostatných podsítích</span><span class="sxs-lookup"><span data-stu-id="deb25-155">A jumpbox, application service, database, and VPN gateway are on separate subnets</span></span>
- <span data-ttu-id="deb25-156">Skupina NSG vynucená u aplikace a databáze podsítě</span><span class="sxs-lookup"><span data-stu-id="deb25-156">NSG enforced on application and database subnets</span></span>
- <span data-ttu-id="deb25-157">Připojení Site-to-site VPN mezi místními a Azure</span><span class="sxs-lookup"><span data-stu-id="deb25-157">Site-to-site VPN connection between on-premises and Azure</span></span>

![Snímek obrazovky stránky topologie hello zotavení po Havárii](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a><span data-ttu-id="deb25-159">Další čtení</span><span class="sxs-lookup"><span data-stu-id="deb25-159">Additional reading</span></span>

- [<span data-ttu-id="deb25-160">Návrh a implementaci databáze Oracle na Azure</span><span class="sxs-lookup"><span data-stu-id="deb25-160">Design and implement an Oracle database on Azure</span></span>](oracle-design.md)
- [<span data-ttu-id="deb25-161">Konfigurace Oracle Data Guard</span><span class="sxs-lookup"><span data-stu-id="deb25-161">Configure Oracle Data Guard</span></span>](configure-oracle-dataguard.md)
- [<span data-ttu-id="deb25-162">Konfigurace brány Golden Oracle</span><span class="sxs-lookup"><span data-stu-id="deb25-162">Configure Oracle Golden Gate</span></span>](configure-oracle-golden-gate.md)
- [<span data-ttu-id="deb25-163">Oracle zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="deb25-163">Oracle backup and recovery</span></span>](oracle-backup-recovery.md)


## <a name="next-steps"></a><span data-ttu-id="deb25-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="deb25-164">Next steps</span></span>

- [<span data-ttu-id="deb25-165">Kurz: Vytvoření vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="deb25-165">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)
- [<span data-ttu-id="deb25-166">Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="deb25-166">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
