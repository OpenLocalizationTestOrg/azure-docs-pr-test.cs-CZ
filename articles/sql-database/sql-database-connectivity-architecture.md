---
title: "Architektura připojení k Azure SQL Database | Microsoft Docs"
description: "Tento dokument popisuje architekturu připojení k Azure SQLDB z Azure nebo z mimo Azure."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/05/2017
ms.author: carlrab
ms.openlocfilehash: 8a1dd89c9e82483184ceb5d767190a5a5044265d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-sql-database-connectivity-architecture"></a><span data-ttu-id="1eecc-103">Architektura připojení k databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="1eecc-103">Azure SQL Database Connectivity Architecture</span></span> 

<span data-ttu-id="1eecc-104">Tento článek popisuje architekturu připojení k databázi SQL Azure a vysvětluje, jak různé součásti funkci, aby se směrovat provoz na instanci databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="1eecc-104">This article explains the Azure SQL Database connectivity architecture and explains how the different components function to direct traffic to your instance of Azure SQL Database.</span></span> <span data-ttu-id="1eecc-105">Tyto Azure SQL Database připojení funkce komponent směrovat síťový provoz do databáze Azure s klienty připojení v rámci Azure a klienti připojení z mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="1eecc-105">These Azure SQL Database connectivity components function to direct network traffic to the Azure database with clients connecting from within Azure and with clients connecting from outside of Azure.</span></span> <span data-ttu-id="1eecc-106">Tento článek také obsahuje skript ukázky změnit, jak dojde k připojení a důležité informace související s změny výchozího nastavení připojení.</span><span class="sxs-lookup"><span data-stu-id="1eecc-106">This article also provides script samples to change how connectivity occurs, and the considerations related to changing the default connectivity settings.</span></span> <span data-ttu-id="1eecc-107">Pokud po přečtení tohoto článku existují nějaké dotazy, obraťte se na Dhruv v dmalik@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="1eecc-107">If there are any questions after reading this article, please contact Dhruv at dmalik@microsoft.com.</span></span> 

## <a name="connectivity-architecture"></a><span data-ttu-id="1eecc-108">Architektura připojení</span><span class="sxs-lookup"><span data-stu-id="1eecc-108">Connectivity architecture</span></span>

<span data-ttu-id="1eecc-109">Následující diagram představuje podrobný přehled architektury připojení k databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="1eecc-109">The following diagram provides a high-level overview of the Azure SQL Database connectivity architecture.</span></span> 

![Přehled architektury](./media/sql-database-connectivity-architecture/architecture-overview.png)


<span data-ttu-id="1eecc-111">Následující kroky popisují, jak se naváže připojení k databázi Azure SQL přes Azure SQL Database softwaru-Vyrovnávání zatížení (SLB) a bránu Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1eecc-111">The following steps describe how a connection is established to an Azure SQL database through the Azure SQL Database software load-balancer (SLB) and the Azure SQL Database gateway.</span></span>

- <span data-ttu-id="1eecc-112">Klienti v rámci Azure nebo mimo Azure připojit k SLB, která má veřejnou IP adresu a naslouchá na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="1eecc-112">Clients within Azure or outside of Azure connect to the SLB, which has a public IP address and listens on port 1433.</span></span>
- <span data-ttu-id="1eecc-113">SLB přesměruje přenosy na bráně Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1eecc-113">The SLB directs traffic to the Azure SQL Database gateway.</span></span>
- <span data-ttu-id="1eecc-114">Brána přesměruje provoz na správný proxy middleware.</span><span class="sxs-lookup"><span data-stu-id="1eecc-114">The gateway redirects the traffic to the correct proxy middleware.</span></span>
- <span data-ttu-id="1eecc-115">Proxy middleware přesměruje přenosy dat k příslušné databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="1eecc-115">The proxy middleware redirects the traffic to the appropriate Azure SQL database.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1eecc-116">Každou z těchto součástí je distribuovat útok na dostupnost služby (Denial) ochrany integrované v síti a vrstva aplikací.</span><span class="sxs-lookup"><span data-stu-id="1eecc-116">Each of these components has distributed denial of service (DDoS) protection built-in at the network and the app layer.</span></span>
>

## <a name="connectivity-from-within-azure"></a><span data-ttu-id="1eecc-117">Připojení z v rámci Azure</span><span class="sxs-lookup"><span data-stu-id="1eecc-117">Connectivity from within Azure</span></span>

<span data-ttu-id="1eecc-118">Pokud se připojujete ze v rámci Azure, vaše připojení mají zásady připojení **přesměrování** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1eecc-118">If you are connecting from within Azure, your connections have a connection policy of **Redirect** by default.</span></span> <span data-ttu-id="1eecc-119">Zásady **přesměrování** znamená, že připojení po TCP je vytvoření relace k databázi Azure SQL relaci klienta se pak přesměrují na proxy middleware s změnu cílové virtuální IP adresa od bránu Azure SQL Database a který middleware proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="1eecc-119">A policy of **Redirect** means that connections after the TCP session is established to the Azure SQL database, the client session is then redirected to the proxy middleware with a change to the destination virtual IP from that of the Azure SQL Database gateway to that of the proxy middleware.</span></span> <span data-ttu-id="1eecc-120">Následně všechny následující pakety toku přímo prostřednictvím proxy serveru middlewaru, obcházení brány Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1eecc-120">Thereafter, all subsequent packets flow directly via the proxy middleware, bypassing the Azure SQL Database gateway.</span></span> <span data-ttu-id="1eecc-121">Následující diagram znázorňuje tento tok přenosů.</span><span class="sxs-lookup"><span data-stu-id="1eecc-121">The following diagram illustrates this traffic flow.</span></span>

![Přehled architektury](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a><span data-ttu-id="1eecc-123">Připojení z mimo Azure</span><span class="sxs-lookup"><span data-stu-id="1eecc-123">Connectivity from outside of Azure</span></span>

<span data-ttu-id="1eecc-124">Pokud se připojujete ze mimo Azure, vaše připojení mají zásady připojení **Proxy** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1eecc-124">If you are connecting from outside Azure, your connections have a connection policy of **Proxy** by default.</span></span> <span data-ttu-id="1eecc-125">Zásady **Proxy** znamená, že je vytvoření relace TCP prostřednictvím brány Azure SQL Database a všechny následující pakety toku prostřednictvím brány.</span><span class="sxs-lookup"><span data-stu-id="1eecc-125">A policy of **Proxy** means that the TCP session is established via the Azure SQL Database gateway and all subsequent packets flow via the gateway.</span></span> <span data-ttu-id="1eecc-126">Následující diagram znázorňuje tento tok přenosů.</span><span class="sxs-lookup"><span data-stu-id="1eecc-126">The following diagram illustrates this traffic flow.</span></span>

![Přehled architektury](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a><span data-ttu-id="1eecc-128">Azure SQL Database brány IP adresy</span><span class="sxs-lookup"><span data-stu-id="1eecc-128">Azure SQL Database gateway IP addresses</span></span>

<span data-ttu-id="1eecc-129">Pro připojení k databázi Azure SQL z místních prostředků, musíte povolit odchozí přenosy sítě k bráně Azure SQL Database pro vaši oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="1eecc-129">To connect to an Azure SQL database from on-premises resources, you need to allow outbound network traffic to the Azure SQL Database gateway for your Azure region.</span></span> <span data-ttu-id="1eecc-130">Připojení, jdou pouze prostřednictvím brány při připojení v režimu proxy serveru, který je výchozím nastavením, pokud se připojujete z místních prostředků.</span><span class="sxs-lookup"><span data-stu-id="1eecc-130">Your connections only go via the gateway when connecting in Proxy mode, which is the default when connecting from on-premises resources.</span></span>

<span data-ttu-id="1eecc-131">Následující tabulka uvádí primární a sekundární IP adresy brány Azure SQL Database pro všechny datové oblasti.</span><span class="sxs-lookup"><span data-stu-id="1eecc-131">The following table lists the primary and secondary IPs of the Azure SQL Database gateway for all data regions.</span></span> <span data-ttu-id="1eecc-132">Pro některé oblasti jsou dvě IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1eecc-132">For some regions, there are two IP addresses.</span></span> <span data-ttu-id="1eecc-133">V těchto oblastech primární IP adresa je aktuální IP adresu brány, a druhou IP adresu je IP adresa převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="1eecc-133">In these regions, the primary IP address is the current IP address of the gateway and the second IP address is a failover IP address.</span></span> <span data-ttu-id="1eecc-134">Převzetí služeb při selhání adresa je adresa, na kterou jsme může přesunout server, abyste mohli udržovat vysokou dostupnost služeb.</span><span class="sxs-lookup"><span data-stu-id="1eecc-134">The failover address is the address to which we might move your server to keep the service availability high.</span></span> <span data-ttu-id="1eecc-135">Pro tyto oblasti doporučujeme povolit odchozí na IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1eecc-135">For these regions, we recommend that you allow outbound to both the IP addresses.</span></span> <span data-ttu-id="1eecc-136">Druhou IP adresu je vlastnictví společnosti Microsoft a nepřijímá požadavky na žádné služby, až po aktivaci Azure SQL Database tak, aby přijímal připojení.</span><span class="sxs-lookup"><span data-stu-id="1eecc-136">The second IP address is owned by Microsoft and does not listen in on any services until it is activated by Azure SQL Database to accept connections.</span></span>

| <span data-ttu-id="1eecc-137">Název oblasti</span><span class="sxs-lookup"><span data-stu-id="1eecc-137">Region Name</span></span> | <span data-ttu-id="1eecc-138">Primární adresa IP</span><span class="sxs-lookup"><span data-stu-id="1eecc-138">Primary IP address</span></span> | <span data-ttu-id="1eecc-139">Sekundární adresa IP</span><span class="sxs-lookup"><span data-stu-id="1eecc-139">Secondary IP address</span></span> |
| --- | --- |--- |
| <span data-ttu-id="1eecc-140">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="1eecc-140">Australia East</span></span> | <span data-ttu-id="1eecc-141">191.238.66.109</span><span class="sxs-lookup"><span data-stu-id="1eecc-141">191.238.66.109</span></span> | <span data-ttu-id="1eecc-142">13.75.149.87</span><span class="sxs-lookup"><span data-stu-id="1eecc-142">13.75.149.87</span></span> |
| <span data-ttu-id="1eecc-143">Austrálie – jihovýchod</span><span class="sxs-lookup"><span data-stu-id="1eecc-143">Australia South East</span></span> | <span data-ttu-id="1eecc-144">191.239.192.109</span><span class="sxs-lookup"><span data-stu-id="1eecc-144">191.239.192.109</span></span> | <span data-ttu-id="1eecc-145">13.73.109.251</span><span class="sxs-lookup"><span data-stu-id="1eecc-145">13.73.109.251</span></span> |
| <span data-ttu-id="1eecc-146">Brazílie – jih</span><span class="sxs-lookup"><span data-stu-id="1eecc-146">Brazil South</span></span> | <span data-ttu-id="1eecc-147">104.41.11.5</span><span class="sxs-lookup"><span data-stu-id="1eecc-147">104.41.11.5</span></span> | |    
| <span data-ttu-id="1eecc-148">Střední Kanada</span><span class="sxs-lookup"><span data-stu-id="1eecc-148">Canada Central</span></span> | <span data-ttu-id="1eecc-149">40.85.224.249</span><span class="sxs-lookup"><span data-stu-id="1eecc-149">40.85.224.249</span></span> | |    
| <span data-ttu-id="1eecc-150">Východní Kanada</span><span class="sxs-lookup"><span data-stu-id="1eecc-150">Canada East</span></span> | <span data-ttu-id="1eecc-151">40.86.226.166</span><span class="sxs-lookup"><span data-stu-id="1eecc-151">40.86.226.166</span></span> | |
| <span data-ttu-id="1eecc-152">Střed USA</span><span class="sxs-lookup"><span data-stu-id="1eecc-152">Central US</span></span> | <span data-ttu-id="1eecc-153">23.99.160.139</span><span class="sxs-lookup"><span data-stu-id="1eecc-153">23.99.160.139</span></span> | <span data-ttu-id="1eecc-154">13.67.215.62</span><span class="sxs-lookup"><span data-stu-id="1eecc-154">13.67.215.62</span></span> |
| <span data-ttu-id="1eecc-155">Východní Asie</span><span class="sxs-lookup"><span data-stu-id="1eecc-155">East Asia</span></span> | <span data-ttu-id="1eecc-156">191.234.2.139</span><span class="sxs-lookup"><span data-stu-id="1eecc-156">191.234.2.139</span></span> | <span data-ttu-id="1eecc-157">52.175.33.150</span><span class="sxs-lookup"><span data-stu-id="1eecc-157">52.175.33.150</span></span> |
| <span data-ttu-id="1eecc-158">Východní USA 1</span><span class="sxs-lookup"><span data-stu-id="1eecc-158">East US 1</span></span> | <span data-ttu-id="1eecc-159">191.238.6.43</span><span class="sxs-lookup"><span data-stu-id="1eecc-159">191.238.6.43</span></span> | <span data-ttu-id="1eecc-160">40.121.158.30</span><span class="sxs-lookup"><span data-stu-id="1eecc-160">40.121.158.30</span></span> |
| <span data-ttu-id="1eecc-161">Východní USA 2</span><span class="sxs-lookup"><span data-stu-id="1eecc-161">East US 2</span></span> | <span data-ttu-id="1eecc-162">191.239.224.107</span><span class="sxs-lookup"><span data-stu-id="1eecc-162">191.239.224.107</span></span> | <span data-ttu-id="1eecc-163">40.79.84.180</span><span class="sxs-lookup"><span data-stu-id="1eecc-163">40.79.84.180</span></span> |
| <span data-ttu-id="1eecc-164">Indie – střed</span><span class="sxs-lookup"><span data-stu-id="1eecc-164">India Central</span></span> | <span data-ttu-id="1eecc-165">104.211.96.159</span><span class="sxs-lookup"><span data-stu-id="1eecc-165">104.211.96.159</span></span>  | |   
| <span data-ttu-id="1eecc-166">Indie – jih</span><span class="sxs-lookup"><span data-stu-id="1eecc-166">India South</span></span> | <span data-ttu-id="1eecc-167">104.211.224.146</span><span class="sxs-lookup"><span data-stu-id="1eecc-167">104.211.224.146</span></span>  | |
| <span data-ttu-id="1eecc-168">Indie – západ</span><span class="sxs-lookup"><span data-stu-id="1eecc-168">India West</span></span> | <span data-ttu-id="1eecc-169">104.211.160.80</span><span class="sxs-lookup"><span data-stu-id="1eecc-169">104.211.160.80</span></span> | |
| <span data-ttu-id="1eecc-170">Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="1eecc-170">Japan East</span></span> | <span data-ttu-id="1eecc-171">191.237.240.43</span><span class="sxs-lookup"><span data-stu-id="1eecc-171">191.237.240.43</span></span> | <span data-ttu-id="1eecc-172">13.78.61.196</span><span class="sxs-lookup"><span data-stu-id="1eecc-172">13.78.61.196</span></span> |
| <span data-ttu-id="1eecc-173">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="1eecc-173">Japan West</span></span> | <span data-ttu-id="1eecc-174">191.238.68.11</span><span class="sxs-lookup"><span data-stu-id="1eecc-174">191.238.68.11</span></span> | <span data-ttu-id="1eecc-175">104.214.148.156</span><span class="sxs-lookup"><span data-stu-id="1eecc-175">104.214.148.156</span></span> |
| <span data-ttu-id="1eecc-176">Korea – střed</span><span class="sxs-lookup"><span data-stu-id="1eecc-176">Korea Central</span></span> | <span data-ttu-id="1eecc-177">52.231.32.42</span><span class="sxs-lookup"><span data-stu-id="1eecc-177">52.231.32.42</span></span> | |
| <span data-ttu-id="1eecc-178">Korea – jih</span><span class="sxs-lookup"><span data-stu-id="1eecc-178">Korea South</span></span> | <span data-ttu-id="1eecc-179">52.231.200.86</span><span class="sxs-lookup"><span data-stu-id="1eecc-179">52.231.200.86</span></span> |  |
| <span data-ttu-id="1eecc-180">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="1eecc-180">North Central US</span></span> | <span data-ttu-id="1eecc-181">23.98.55.75</span><span class="sxs-lookup"><span data-stu-id="1eecc-181">23.98.55.75</span></span> | <span data-ttu-id="1eecc-182">23.96.178.199</span><span class="sxs-lookup"><span data-stu-id="1eecc-182">23.96.178.199</span></span> |
| <span data-ttu-id="1eecc-183">Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="1eecc-183">North Europe</span></span> | <span data-ttu-id="1eecc-184">191.235.193.75</span><span class="sxs-lookup"><span data-stu-id="1eecc-184">191.235.193.75</span></span> | <span data-ttu-id="1eecc-185">40.113.93.91</span><span class="sxs-lookup"><span data-stu-id="1eecc-185">40.113.93.91</span></span> |
| <span data-ttu-id="1eecc-186">Střed USA – jih</span><span class="sxs-lookup"><span data-stu-id="1eecc-186">South Central US</span></span> | <span data-ttu-id="1eecc-187">23.98.162.75</span><span class="sxs-lookup"><span data-stu-id="1eecc-187">23.98.162.75</span></span> | <span data-ttu-id="1eecc-188">13.66.62.124</span><span class="sxs-lookup"><span data-stu-id="1eecc-188">13.66.62.124</span></span> |
| <span data-ttu-id="1eecc-189">Jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="1eecc-189">South East Asia</span></span> | <span data-ttu-id="1eecc-190">23.100.117.95</span><span class="sxs-lookup"><span data-stu-id="1eecc-190">23.100.117.95</span></span> | <span data-ttu-id="1eecc-191">104.43.15.0</span><span class="sxs-lookup"><span data-stu-id="1eecc-191">104.43.15.0</span></span> |
| <span data-ttu-id="1eecc-192">Spojené království – sever</span><span class="sxs-lookup"><span data-stu-id="1eecc-192">UK North</span></span> | <span data-ttu-id="1eecc-193">13.87.97.210</span><span class="sxs-lookup"><span data-stu-id="1eecc-193">13.87.97.210</span></span> | |
| <span data-ttu-id="1eecc-194">Spojené království – jih 1</span><span class="sxs-lookup"><span data-stu-id="1eecc-194">UK South 1</span></span> | <span data-ttu-id="1eecc-195">51.140.184.11</span><span class="sxs-lookup"><span data-stu-id="1eecc-195">51.140.184.11</span></span> | |    
| <span data-ttu-id="1eecc-196">Spojené království – jih 2</span><span class="sxs-lookup"><span data-stu-id="1eecc-196">UK South 2</span></span> | <span data-ttu-id="1eecc-197">13.87.34.7</span><span class="sxs-lookup"><span data-stu-id="1eecc-197">13.87.34.7</span></span> | |
| <span data-ttu-id="1eecc-198">Spojené království – západ</span><span class="sxs-lookup"><span data-stu-id="1eecc-198">UK West</span></span> | <span data-ttu-id="1eecc-199">51.141.8.11</span><span class="sxs-lookup"><span data-stu-id="1eecc-199">51.141.8.11</span></span>  | |
| <span data-ttu-id="1eecc-200">Západní střed USA</span><span class="sxs-lookup"><span data-stu-id="1eecc-200">West Central US</span></span> | <span data-ttu-id="1eecc-201">13.78.145.25</span><span class="sxs-lookup"><span data-stu-id="1eecc-201">13.78.145.25</span></span> | |
| <span data-ttu-id="1eecc-202">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="1eecc-202">West Europe</span></span> | <span data-ttu-id="1eecc-203">191.237.232.75</span><span class="sxs-lookup"><span data-stu-id="1eecc-203">191.237.232.75</span></span> | <span data-ttu-id="1eecc-204">40.68.37.158</span><span class="sxs-lookup"><span data-stu-id="1eecc-204">40.68.37.158</span></span> |
| <span data-ttu-id="1eecc-205">Západní USA 1</span><span class="sxs-lookup"><span data-stu-id="1eecc-205">West US 1</span></span> | <span data-ttu-id="1eecc-206">23.99.34.75</span><span class="sxs-lookup"><span data-stu-id="1eecc-206">23.99.34.75</span></span> | <span data-ttu-id="1eecc-207">104.42.238.205</span><span class="sxs-lookup"><span data-stu-id="1eecc-207">104.42.238.205</span></span> |
| <span data-ttu-id="1eecc-208">Západní USA 2</span><span class="sxs-lookup"><span data-stu-id="1eecc-208">West US 2</span></span> | <span data-ttu-id="1eecc-209">13.66.226.202</span><span class="sxs-lookup"><span data-stu-id="1eecc-209">13.66.226.202</span></span>  | |
||||

## <a name="change-azure-sql-database-connection-policy"></a><span data-ttu-id="1eecc-210">Změnit zásady připojení databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="1eecc-210">Change Azure SQL Database connection policy</span></span>

<span data-ttu-id="1eecc-211">Můžete změnit zásady připojení databáze SQL Azure pro server Azure SQL Database [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span><span class="sxs-lookup"><span data-stu-id="1eecc-211">To change the Azure SQL Database connection policy for an Azure SQL Database server, use the [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span></span> 

- <span data-ttu-id="1eecc-212">Pokud vaše zásady připojení je nastavený na **Proxy**, všechny síťové pakety toku prostřednictvím brány Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1eecc-212">If your connection policy is set to **Proxy**, all network packets flow via the Azure SQL Database gateway.</span></span> <span data-ttu-id="1eecc-213">U tohoto nastavení musíte povolit odchozí pouze IP brány Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1eecc-213">For this setting, you need to allow outbound to only the Azure SQL Database gateway IP.</span></span> <span data-ttu-id="1eecc-214">Pomocí nastavení **Proxy** má více latenci než nastavení **přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="1eecc-214">Using a setting of **Proxy** has more latency than a setting of **Redirect**.</span></span> 
- <span data-ttu-id="1eecc-215">Pokud je nastavení zásady vaší připojení **přesměrování**, všechny síťové pakety toku přímo k proxy serveru middleware.</span><span class="sxs-lookup"><span data-stu-id="1eecc-215">If your connection policy is setting **Redirect**, all network packets flow directly to the middleware proxy.</span></span> <span data-ttu-id="1eecc-216">U tohoto nastavení musíte povolit odchozí do víc IP adres.</span><span class="sxs-lookup"><span data-stu-id="1eecc-216">For this setting, you need to allow outbound to multiple IPs.</span></span> 

## <a name="script-to-change-connection-settings"></a><span data-ttu-id="1eecc-217">Chcete-li změnit nastavení připojení skriptu</span><span class="sxs-lookup"><span data-stu-id="1eecc-217">Script to change connection settings</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1eecc-218">Tento skript vyžaduje [modul Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="1eecc-218">This script requires the [Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>
>

<span data-ttu-id="1eecc-219">Následující skript prostředí PowerShell ukazuje, jak změnit zásady připojení.</span><span class="sxs-lookup"><span data-stu-id="1eecc-219">The following PowerShell script shows how to change the connection policy.</span></span>

```powershell
import-module azureRm
Login-AzureRmAccount

$tenantId =  #your AAD tenant ID
$subscriptionId = #Azure SubscriptionID
$uri = #AAD uri
$authUrl = "https://login.microsoftonline.com/$tenantId"
$serverName = #sqldb server name 
$resourceGroupName=#sqldb resource group
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl

$result = $AuthContext.AcquireToken("https://management.core.windows.net/",
$clientId,
[Uri]$uri, 
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#getting the current connection property
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#setting the property to ‘Proxy’
$connectionType=”Proxy” <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="next-steps"></a><span data-ttu-id="1eecc-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1eecc-220">Next steps</span></span>

- <span data-ttu-id="1eecc-221">Informace o tom, jak změnit zásady připojení Azure SQL Database pro databázi Azure SQL server najdete v tématu [vytvoření nebo aktualizace serveru připojení zásady pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span><span class="sxs-lookup"><span data-stu-id="1eecc-221">For information on how to change the Azure SQL Database connection policy for an Azure SQL Database server, see [Create or Update Server Connection Policy using the REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span></span>
- <span data-ttu-id="1eecc-222">Informace o chování Azure SQL Database připojení pro klienty, kteří používají ADO.NET 4.5 nebo novější verze najdete v tématu [porty nad rámec 1433 pro technologii ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="1eecc-222">For information about Azure SQL Database connection behavior for clients that use ADO.NET 4.5 or a later version, see [Ports beyond 1433 for ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>
- <span data-ttu-id="1eecc-223">Obecná aplikace vývoj přehled informace najdete v tématu [přehled vývoje aplikace databáze SQL](sql-database-develop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1eecc-223">For general application development overview information, see [SQL Database Application Development Overview](sql-database-develop-overview.md).</span></span>
