---
title: "aaaAzure architektura připojení k SQL Database | Microsoft Docs"
description: "Tento dokument popisuje hello Azure SQLDB připojení architektura z Azure nebo z mimo Azure."
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
ms.openlocfilehash: 917df6d88a16f1b841b617fb2a53025b4d14d034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-connectivity-architecture"></a><span data-ttu-id="60f8d-103">Architektura připojení k databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="60f8d-103">Azure SQL Database Connectivity Architecture</span></span> 

<span data-ttu-id="60f8d-104">Tento článek popisuje architekturu připojení k databázi SQL Azure hello a vysvětluje, jak funkce různých komponent hello toodirect provoz tooyour instance databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="60f8d-104">This article explains hello Azure SQL Database connectivity architecture and explains how hello different components function toodirect traffic tooyour instance of Azure SQL Database.</span></span> <span data-ttu-id="60f8d-105">Tyto součásti připojení k databázi SQL Azure fungovat toodirect síťový provoz toohello databáze Azure s klienty připojení v rámci Azure a klienti připojení z mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="60f8d-105">These Azure SQL Database connectivity components function toodirect network traffic toohello Azure database with clients connecting from within Azure and with clients connecting from outside of Azure.</span></span> <span data-ttu-id="60f8d-106">Tento článek také obsahuje skript ukázky toochange jak dojde k připojení a hello aspekty související nastavení připojení k výchozí toochanging hello.</span><span class="sxs-lookup"><span data-stu-id="60f8d-106">This article also provides script samples toochange how connectivity occurs, and hello considerations related toochanging hello default connectivity settings.</span></span> <span data-ttu-id="60f8d-107">Pokud po přečtení tohoto článku existují nějaké dotazy, obraťte se na Dhruv v dmalik@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="60f8d-107">If there are any questions after reading this article, please contact Dhruv at dmalik@microsoft.com.</span></span> 

## <a name="connectivity-architecture"></a><span data-ttu-id="60f8d-108">Architektura připojení</span><span class="sxs-lookup"><span data-stu-id="60f8d-108">Connectivity architecture</span></span>

<span data-ttu-id="60f8d-109">Následující diagram Hello poskytuje podrobný přehled hello architektura připojení k databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="60f8d-109">hello following diagram provides a high-level overview of hello Azure SQL Database connectivity architecture.</span></span> 

![Přehled architektury](./media/sql-database-connectivity-architecture/architecture-overview.png)


<span data-ttu-id="60f8d-111">Hello následující kroky popisují, jak připojení je navázáno tooan Azure SQL database prostřednictvím hello Azure SQL Database softwaru Vyrovnávání zatížení (SLB) a brány Azure SQL Database hello.</span><span class="sxs-lookup"><span data-stu-id="60f8d-111">hello following steps describe how a connection is established tooan Azure SQL database through hello Azure SQL Database software load-balancer (SLB) and hello Azure SQL Database gateway.</span></span>

- <span data-ttu-id="60f8d-112">Klienti v rámci Azure nebo mimo Azure připojit toohello SLB, která má veřejnou IP adresu a naslouchá na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="60f8d-112">Clients within Azure or outside of Azure connect toohello SLB, which has a public IP address and listens on port 1433.</span></span>
- <span data-ttu-id="60f8d-113">Hello SLB směrovat provoz toohello Azure SQL Database brány.</span><span class="sxs-lookup"><span data-stu-id="60f8d-113">hello SLB directs traffic toohello Azure SQL Database gateway.</span></span>
- <span data-ttu-id="60f8d-114">Hello brány přesměruje hello provoz toohello správné proxy middleware.</span><span class="sxs-lookup"><span data-stu-id="60f8d-114">hello gateway redirects hello traffic toohello correct proxy middleware.</span></span>
- <span data-ttu-id="60f8d-115">Hello proxy middleware přesměruje hello provoz toohello odpovídající Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="60f8d-115">hello proxy middleware redirects hello traffic toohello appropriate Azure SQL database.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="60f8d-116">Každou z těchto součástí je distribuovat útok na dostupnost služby (Denial) ochrany integrované v síti hello a vrstva aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="60f8d-116">Each of these components has distributed denial of service (DDoS) protection built-in at hello network and hello app layer.</span></span>
>

## <a name="connectivity-from-within-azure"></a><span data-ttu-id="60f8d-117">Připojení z v rámci Azure</span><span class="sxs-lookup"><span data-stu-id="60f8d-117">Connectivity from within Azure</span></span>

<span data-ttu-id="60f8d-118">Pokud se připojujete ze v rámci Azure, vaše připojení mají zásady připojení **přesměrování** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="60f8d-118">If you are connecting from within Azure, your connections have a connection policy of **Redirect** by default.</span></span> <span data-ttu-id="60f8d-119">Zásady **přesměrování** znamená, že připojení po relace TCP hello je navázáno toohello Azure SQL database, relaci klienta hello se pak přesměruje toohello proxy middlewaru s změnu toohello cílové virtuální IP adresy z který toothat brány Azure SQL Database hello hello proxy middlewaru.</span><span class="sxs-lookup"><span data-stu-id="60f8d-119">A policy of **Redirect** means that connections after hello TCP session is established toohello Azure SQL database, hello client session is then redirected toohello proxy middleware with a change toohello destination virtual IP from that of hello Azure SQL Database gateway toothat of hello proxy middleware.</span></span> <span data-ttu-id="60f8d-120">Následně všechny následující pakety toku přímo prostřednictvím middlewaru hello proxy, obcházení brány Azure SQL Database hello.</span><span class="sxs-lookup"><span data-stu-id="60f8d-120">Thereafter, all subsequent packets flow directly via hello proxy middleware, bypassing hello Azure SQL Database gateway.</span></span> <span data-ttu-id="60f8d-121">Hello následující diagram znázorňuje tento tok přenosů.</span><span class="sxs-lookup"><span data-stu-id="60f8d-121">hello following diagram illustrates this traffic flow.</span></span>

![Přehled architektury](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a><span data-ttu-id="60f8d-123">Připojení z mimo Azure</span><span class="sxs-lookup"><span data-stu-id="60f8d-123">Connectivity from outside of Azure</span></span>

<span data-ttu-id="60f8d-124">Pokud se připojujete ze mimo Azure, vaše připojení mají zásady připojení **Proxy** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="60f8d-124">If you are connecting from outside Azure, your connections have a connection policy of **Proxy** by default.</span></span> <span data-ttu-id="60f8d-125">Zásady **Proxy** hello znamená, že je vytvořena relace TCP hello prostřednictvím brány Azure SQL Database hello se všechny následující pakety toku prostřednictvím brány.</span><span class="sxs-lookup"><span data-stu-id="60f8d-125">A policy of **Proxy** means that hello TCP session is established via hello Azure SQL Database gateway and all subsequent packets flow via hello gateway.</span></span> <span data-ttu-id="60f8d-126">Hello následující diagram znázorňuje tento tok přenosů.</span><span class="sxs-lookup"><span data-stu-id="60f8d-126">hello following diagram illustrates this traffic flow.</span></span>

![Přehled architektury](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a><span data-ttu-id="60f8d-128">Azure SQL Database brány IP adresy</span><span class="sxs-lookup"><span data-stu-id="60f8d-128">Azure SQL Database gateway IP addresses</span></span>

<span data-ttu-id="60f8d-129">databáze Azure SQL tooconnect tooan z místních prostředků, musíte tooallow odchozí síťový provoz toohello Azure SQL Database brány pro vaši oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="60f8d-129">tooconnect tooan Azure SQL database from on-premises resources, you need tooallow outbound network traffic toohello Azure SQL Database gateway for your Azure region.</span></span> <span data-ttu-id="60f8d-130">Připojení, jdou pouze prostřednictvím brány hello při připojení v režimu proxy serveru, který je výchozí hello při připojení z místních prostředků.</span><span class="sxs-lookup"><span data-stu-id="60f8d-130">Your connections only go via hello gateway when connecting in Proxy mode, which is hello default when connecting from on-premises resources.</span></span>

<span data-ttu-id="60f8d-131">Následující tabulka seznamy Hello hello primární a sekundární IP adresy brány Azure SQL Database hello pro všechny datové oblasti.</span><span class="sxs-lookup"><span data-stu-id="60f8d-131">hello following table lists hello primary and secondary IPs of hello Azure SQL Database gateway for all data regions.</span></span> <span data-ttu-id="60f8d-132">Pro některé oblasti jsou dvě IP adresy.</span><span class="sxs-lookup"><span data-stu-id="60f8d-132">For some regions, there are two IP addresses.</span></span> <span data-ttu-id="60f8d-133">V těchto oblastech hello primární IP adresa je hello aktuální IP adresu brány hello a druhou IP adresu hello je IP adresa převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="60f8d-133">In these regions, hello primary IP address is hello current IP address of hello gateway and hello second IP address is a failover IP address.</span></span> <span data-ttu-id="60f8d-134">Adresa Hello převzetí služeb při selhání je toowhich adresu hello jsme může váš server tookeep hello dostupnost služby vysoké přesunout.</span><span class="sxs-lookup"><span data-stu-id="60f8d-134">hello failover address is hello address toowhich we might move your server tookeep hello service availability high.</span></span> <span data-ttu-id="60f8d-135">Pro tyto oblasti doporučujeme povolit odchozí tooboth hello IP adresy.</span><span class="sxs-lookup"><span data-stu-id="60f8d-135">For these regions, we recommend that you allow outbound tooboth hello IP addresses.</span></span> <span data-ttu-id="60f8d-136">Hello druhou IP adresu je vlastnictví společnosti Microsoft a nepřijímá požadavky na žádné služby, dokud je aktivován Azure SQL Database tooaccept připojení.</span><span class="sxs-lookup"><span data-stu-id="60f8d-136">hello second IP address is owned by Microsoft and does not listen in on any services until it is activated by Azure SQL Database tooaccept connections.</span></span>

| <span data-ttu-id="60f8d-137">Název oblasti</span><span class="sxs-lookup"><span data-stu-id="60f8d-137">Region Name</span></span> | <span data-ttu-id="60f8d-138">Primární adresa IP</span><span class="sxs-lookup"><span data-stu-id="60f8d-138">Primary IP address</span></span> | <span data-ttu-id="60f8d-139">Sekundární adresa IP</span><span class="sxs-lookup"><span data-stu-id="60f8d-139">Secondary IP address</span></span> |
| --- | --- |--- |
| <span data-ttu-id="60f8d-140">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="60f8d-140">Australia East</span></span> | <span data-ttu-id="60f8d-141">191.238.66.109</span><span class="sxs-lookup"><span data-stu-id="60f8d-141">191.238.66.109</span></span> | <span data-ttu-id="60f8d-142">13.75.149.87</span><span class="sxs-lookup"><span data-stu-id="60f8d-142">13.75.149.87</span></span> |
| <span data-ttu-id="60f8d-143">Austrálie – jihovýchod</span><span class="sxs-lookup"><span data-stu-id="60f8d-143">Australia South East</span></span> | <span data-ttu-id="60f8d-144">191.239.192.109</span><span class="sxs-lookup"><span data-stu-id="60f8d-144">191.239.192.109</span></span> | <span data-ttu-id="60f8d-145">13.73.109.251</span><span class="sxs-lookup"><span data-stu-id="60f8d-145">13.73.109.251</span></span> |
| <span data-ttu-id="60f8d-146">Brazílie – jih</span><span class="sxs-lookup"><span data-stu-id="60f8d-146">Brazil South</span></span> | <span data-ttu-id="60f8d-147">104.41.11.5</span><span class="sxs-lookup"><span data-stu-id="60f8d-147">104.41.11.5</span></span> | |    
| <span data-ttu-id="60f8d-148">Střední Kanada</span><span class="sxs-lookup"><span data-stu-id="60f8d-148">Canada Central</span></span> | <span data-ttu-id="60f8d-149">40.85.224.249</span><span class="sxs-lookup"><span data-stu-id="60f8d-149">40.85.224.249</span></span> | |    
| <span data-ttu-id="60f8d-150">Východní Kanada</span><span class="sxs-lookup"><span data-stu-id="60f8d-150">Canada East</span></span> | <span data-ttu-id="60f8d-151">40.86.226.166</span><span class="sxs-lookup"><span data-stu-id="60f8d-151">40.86.226.166</span></span> | |
| <span data-ttu-id="60f8d-152">Střed USA</span><span class="sxs-lookup"><span data-stu-id="60f8d-152">Central US</span></span> | <span data-ttu-id="60f8d-153">23.99.160.139</span><span class="sxs-lookup"><span data-stu-id="60f8d-153">23.99.160.139</span></span> | <span data-ttu-id="60f8d-154">13.67.215.62</span><span class="sxs-lookup"><span data-stu-id="60f8d-154">13.67.215.62</span></span> |
| <span data-ttu-id="60f8d-155">Východní Asie</span><span class="sxs-lookup"><span data-stu-id="60f8d-155">East Asia</span></span> | <span data-ttu-id="60f8d-156">191.234.2.139</span><span class="sxs-lookup"><span data-stu-id="60f8d-156">191.234.2.139</span></span> | <span data-ttu-id="60f8d-157">52.175.33.150</span><span class="sxs-lookup"><span data-stu-id="60f8d-157">52.175.33.150</span></span> |
| <span data-ttu-id="60f8d-158">Východní USA 1</span><span class="sxs-lookup"><span data-stu-id="60f8d-158">East US 1</span></span> | <span data-ttu-id="60f8d-159">191.238.6.43</span><span class="sxs-lookup"><span data-stu-id="60f8d-159">191.238.6.43</span></span> | <span data-ttu-id="60f8d-160">40.121.158.30</span><span class="sxs-lookup"><span data-stu-id="60f8d-160">40.121.158.30</span></span> |
| <span data-ttu-id="60f8d-161">Východní USA 2</span><span class="sxs-lookup"><span data-stu-id="60f8d-161">East US 2</span></span> | <span data-ttu-id="60f8d-162">191.239.224.107</span><span class="sxs-lookup"><span data-stu-id="60f8d-162">191.239.224.107</span></span> | <span data-ttu-id="60f8d-163">40.79.84.180</span><span class="sxs-lookup"><span data-stu-id="60f8d-163">40.79.84.180</span></span> |
| <span data-ttu-id="60f8d-164">Indie – střed</span><span class="sxs-lookup"><span data-stu-id="60f8d-164">India Central</span></span> | <span data-ttu-id="60f8d-165">104.211.96.159</span><span class="sxs-lookup"><span data-stu-id="60f8d-165">104.211.96.159</span></span>  | |   
| <span data-ttu-id="60f8d-166">Indie – jih</span><span class="sxs-lookup"><span data-stu-id="60f8d-166">India South</span></span> | <span data-ttu-id="60f8d-167">104.211.224.146</span><span class="sxs-lookup"><span data-stu-id="60f8d-167">104.211.224.146</span></span>  | |
| <span data-ttu-id="60f8d-168">Indie – západ</span><span class="sxs-lookup"><span data-stu-id="60f8d-168">India West</span></span> | <span data-ttu-id="60f8d-169">104.211.160.80</span><span class="sxs-lookup"><span data-stu-id="60f8d-169">104.211.160.80</span></span> | |
| <span data-ttu-id="60f8d-170">Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="60f8d-170">Japan East</span></span> | <span data-ttu-id="60f8d-171">191.237.240.43</span><span class="sxs-lookup"><span data-stu-id="60f8d-171">191.237.240.43</span></span> | <span data-ttu-id="60f8d-172">13.78.61.196</span><span class="sxs-lookup"><span data-stu-id="60f8d-172">13.78.61.196</span></span> |
| <span data-ttu-id="60f8d-173">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="60f8d-173">Japan West</span></span> | <span data-ttu-id="60f8d-174">191.238.68.11</span><span class="sxs-lookup"><span data-stu-id="60f8d-174">191.238.68.11</span></span> | <span data-ttu-id="60f8d-175">104.214.148.156</span><span class="sxs-lookup"><span data-stu-id="60f8d-175">104.214.148.156</span></span> |
| <span data-ttu-id="60f8d-176">Korea – střed</span><span class="sxs-lookup"><span data-stu-id="60f8d-176">Korea Central</span></span> | <span data-ttu-id="60f8d-177">52.231.32.42</span><span class="sxs-lookup"><span data-stu-id="60f8d-177">52.231.32.42</span></span> | |
| <span data-ttu-id="60f8d-178">Korea – jih</span><span class="sxs-lookup"><span data-stu-id="60f8d-178">Korea South</span></span> | <span data-ttu-id="60f8d-179">52.231.200.86</span><span class="sxs-lookup"><span data-stu-id="60f8d-179">52.231.200.86</span></span> |  |
| <span data-ttu-id="60f8d-180">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="60f8d-180">North Central US</span></span> | <span data-ttu-id="60f8d-181">23.98.55.75</span><span class="sxs-lookup"><span data-stu-id="60f8d-181">23.98.55.75</span></span> | <span data-ttu-id="60f8d-182">23.96.178.199</span><span class="sxs-lookup"><span data-stu-id="60f8d-182">23.96.178.199</span></span> |
| <span data-ttu-id="60f8d-183">Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="60f8d-183">North Europe</span></span> | <span data-ttu-id="60f8d-184">191.235.193.75</span><span class="sxs-lookup"><span data-stu-id="60f8d-184">191.235.193.75</span></span> | <span data-ttu-id="60f8d-185">40.113.93.91</span><span class="sxs-lookup"><span data-stu-id="60f8d-185">40.113.93.91</span></span> |
| <span data-ttu-id="60f8d-186">Střed USA – jih</span><span class="sxs-lookup"><span data-stu-id="60f8d-186">South Central US</span></span> | <span data-ttu-id="60f8d-187">23.98.162.75</span><span class="sxs-lookup"><span data-stu-id="60f8d-187">23.98.162.75</span></span> | <span data-ttu-id="60f8d-188">13.66.62.124</span><span class="sxs-lookup"><span data-stu-id="60f8d-188">13.66.62.124</span></span> |
| <span data-ttu-id="60f8d-189">Jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="60f8d-189">South East Asia</span></span> | <span data-ttu-id="60f8d-190">23.100.117.95</span><span class="sxs-lookup"><span data-stu-id="60f8d-190">23.100.117.95</span></span> | <span data-ttu-id="60f8d-191">104.43.15.0</span><span class="sxs-lookup"><span data-stu-id="60f8d-191">104.43.15.0</span></span> |
| <span data-ttu-id="60f8d-192">Spojené království – sever</span><span class="sxs-lookup"><span data-stu-id="60f8d-192">UK North</span></span> | <span data-ttu-id="60f8d-193">13.87.97.210</span><span class="sxs-lookup"><span data-stu-id="60f8d-193">13.87.97.210</span></span> | |
| <span data-ttu-id="60f8d-194">Spojené království – jih 1</span><span class="sxs-lookup"><span data-stu-id="60f8d-194">UK South 1</span></span> | <span data-ttu-id="60f8d-195">51.140.184.11</span><span class="sxs-lookup"><span data-stu-id="60f8d-195">51.140.184.11</span></span> | |    
| <span data-ttu-id="60f8d-196">Spojené království – jih 2</span><span class="sxs-lookup"><span data-stu-id="60f8d-196">UK South 2</span></span> | <span data-ttu-id="60f8d-197">13.87.34.7</span><span class="sxs-lookup"><span data-stu-id="60f8d-197">13.87.34.7</span></span> | |
| <span data-ttu-id="60f8d-198">Spojené království – západ</span><span class="sxs-lookup"><span data-stu-id="60f8d-198">UK West</span></span> | <span data-ttu-id="60f8d-199">51.141.8.11</span><span class="sxs-lookup"><span data-stu-id="60f8d-199">51.141.8.11</span></span>  | |
| <span data-ttu-id="60f8d-200">Západní střed USA</span><span class="sxs-lookup"><span data-stu-id="60f8d-200">West Central US</span></span> | <span data-ttu-id="60f8d-201">13.78.145.25</span><span class="sxs-lookup"><span data-stu-id="60f8d-201">13.78.145.25</span></span> | |
| <span data-ttu-id="60f8d-202">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="60f8d-202">West Europe</span></span> | <span data-ttu-id="60f8d-203">191.237.232.75</span><span class="sxs-lookup"><span data-stu-id="60f8d-203">191.237.232.75</span></span> | <span data-ttu-id="60f8d-204">40.68.37.158</span><span class="sxs-lookup"><span data-stu-id="60f8d-204">40.68.37.158</span></span> |
| <span data-ttu-id="60f8d-205">Západní USA 1</span><span class="sxs-lookup"><span data-stu-id="60f8d-205">West US 1</span></span> | <span data-ttu-id="60f8d-206">23.99.34.75</span><span class="sxs-lookup"><span data-stu-id="60f8d-206">23.99.34.75</span></span> | <span data-ttu-id="60f8d-207">104.42.238.205</span><span class="sxs-lookup"><span data-stu-id="60f8d-207">104.42.238.205</span></span> |
| <span data-ttu-id="60f8d-208">Západní USA 2</span><span class="sxs-lookup"><span data-stu-id="60f8d-208">West US 2</span></span> | <span data-ttu-id="60f8d-209">13.66.226.202</span><span class="sxs-lookup"><span data-stu-id="60f8d-209">13.66.226.202</span></span>  | |
||||

## <a name="change-azure-sql-database-connection-policy"></a><span data-ttu-id="60f8d-210">Změnit zásady připojení databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="60f8d-210">Change Azure SQL Database connection policy</span></span>

<span data-ttu-id="60f8d-211">toochange hello Azure SQL Database zásady připojení pro server Azure SQL Database, použijte hello [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span><span class="sxs-lookup"><span data-stu-id="60f8d-211">toochange hello Azure SQL Database connection policy for an Azure SQL Database server, use hello [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span></span> 

- <span data-ttu-id="60f8d-212">Pokud vaše zásady připojení je nastaven příliš**Proxy**, všechny síťové pakety toku prostřednictvím brány Azure SQL Database hello.</span><span class="sxs-lookup"><span data-stu-id="60f8d-212">If your connection policy is set too**Proxy**, all network packets flow via hello Azure SQL Database gateway.</span></span> <span data-ttu-id="60f8d-213">U tohoto nastavení musíte IP brány tooallow odchozí tooonly hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="60f8d-213">For this setting, you need tooallow outbound tooonly hello Azure SQL Database gateway IP.</span></span> <span data-ttu-id="60f8d-214">Pomocí nastavení **Proxy** má více latenci než nastavení **přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="60f8d-214">Using a setting of **Proxy** has more latency than a setting of **Redirect**.</span></span> 
- <span data-ttu-id="60f8d-215">Pokud je nastavení zásady vaší připojení **přesměrování**, všech síťových paketů toku přímo toohello middleware proxy.</span><span class="sxs-lookup"><span data-stu-id="60f8d-215">If your connection policy is setting **Redirect**, all network packets flow directly toohello middleware proxy.</span></span> <span data-ttu-id="60f8d-216">U tohoto nastavení musíte odchozí toomultiple tooallow IP adresy.</span><span class="sxs-lookup"><span data-stu-id="60f8d-216">For this setting, you need tooallow outbound toomultiple IPs.</span></span> 

## <a name="script-toochange-connection-settings"></a><span data-ttu-id="60f8d-217">Nastavení připojení toochange skriptu</span><span class="sxs-lookup"><span data-stu-id="60f8d-217">Script toochange connection settings</span></span>

> [!IMPORTANT]
> <span data-ttu-id="60f8d-218">Tento skript vyžaduje hello [modul Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="60f8d-218">This script requires hello [Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>
>

<span data-ttu-id="60f8d-219">Hello následující skript prostředí PowerShell ukazuje, jak toochange hello zásady připojení.</span><span class="sxs-lookup"><span data-stu-id="60f8d-219">hello following PowerShell script shows how toochange hello connection policy.</span></span>

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

#getting hello current connection property
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#setting hello property too‘Proxy’
$connectionType=”Proxy” <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="next-steps"></a><span data-ttu-id="60f8d-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60f8d-220">Next steps</span></span>

- <span data-ttu-id="60f8d-221">Informace o tom, jak toochange hello Azure SQL Database zásady připojení pro server Azure SQL Database, najdete v části [vytvořit nebo aktualizovat zásady připojení serveru pomocí rozhraní REST API hello](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span><span class="sxs-lookup"><span data-stu-id="60f8d-221">For information on how toochange hello Azure SQL Database connection policy for an Azure SQL Database server, see [Create or Update Server Connection Policy using hello REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span></span>
- <span data-ttu-id="60f8d-222">Informace o chování Azure SQL Database připojení pro klienty, kteří používají ADO.NET 4.5 nebo novější verze najdete v tématu [porty nad rámec 1433 pro technologii ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="60f8d-222">For information about Azure SQL Database connection behavior for clients that use ADO.NET 4.5 or a later version, see [Ports beyond 1433 for ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>
- <span data-ttu-id="60f8d-223">Obecná aplikace vývoj přehled informace najdete v tématu [přehled vývoje aplikace databáze SQL](sql-database-develop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="60f8d-223">For general application development overview information, see [SQL Database Application Development Overview](sql-database-develop-overview.md).</span></span>
