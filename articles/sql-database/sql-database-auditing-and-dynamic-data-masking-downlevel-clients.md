---
title: "Tabulky auditování, přesměrování TDS IP koncové body pro databázi SQL Azure a | Microsoft Docs"
description: "Další informace o auditování, TDS přesměrování a IP koncový bod změny při implementaci tabulky auditování ve službě Azure SQL Database."
services: sql-database
documentationcenter: 
author: giladm
manager: jhubbard
editor: 
ms.assetid: 4ef19ed1-e798-43a2-ad99-0e563f93ab53
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: giladm
ms.openlocfilehash: d4a7e6658ec65a70bd7e07859e2a69acee58b7b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-table-auditing"></a><span data-ttu-id="953c3-103">SQL Database – podpora klientů nižší úrovně a změní koncový bod IP adresy pro auditování tabulka</span><span class="sxs-lookup"><span data-stu-id="953c3-103">SQL Database -  Downlevel clients support and IP endpoint changes for Table Auditing</span></span>

> [!IMPORTANT]
> <span data-ttu-id="953c3-104">Tento dokument se vztahuje pouze na auditování tabulka, která je **nyní zastaralé**.</span><span class="sxs-lookup"><span data-stu-id="953c3-104">This document applies only to Table Auditing, which is **now deprecated**.</span></span><br>
> <span data-ttu-id="953c3-105">Použijte prosím nový [auditování objektů Blob](sql-database-auditing.md) metoda, která **nemá** vyžadovat úpravy řetězec připojení klientů nižší úrovně.</span><span class="sxs-lookup"><span data-stu-id="953c3-105">Please use the new [Blob Auditing](sql-database-auditing.md) method, which **does not** require downlevel client connection string modifications.</span></span> <span data-ttu-id="953c3-106">Další informace o auditování objektů Blob najdete v [začít pracovat s auditování databáze SQL](sql-database-auditing.md).</span><span class="sxs-lookup"><span data-stu-id="953c3-106">Additional info on Blob Auditing can be found in [Get started with SQL database auditing](sql-database-auditing.md).</span></span>

<span data-ttu-id="953c3-107">[Databáze auditování](sql-database-auditing.md) automaticky spolupracuje se službou SQL klientů, které podporují přesměrování TDS.</span><span class="sxs-lookup"><span data-stu-id="953c3-107">[Database Auditing](sql-database-auditing.md) works automatically with SQL clients that support TDS redirection.</span></span> <span data-ttu-id="953c3-108">Všimněte si, že přesměrování nelze použít při použití metody auditování objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="953c3-108">Note that redirection does not apply when using the Blob Auditing method.</span></span>

## <span data-ttu-id="953c3-109"><a id="subheading-1"></a>Podpora klientů nižší úrovně</span><span class="sxs-lookup"><span data-stu-id="953c3-109"><a id="subheading-1"></a>Downlevel clients support</span></span>
<span data-ttu-id="953c3-110">Jakýkoli klient, který implementuje TDS 7.4 také podporuje přesměrování.</span><span class="sxs-lookup"><span data-stu-id="953c3-110">Any client which implements TDS 7.4 should also support redirection.</span></span> <span data-ttu-id="953c3-111">Výjimky zahrnují JDBC 4.0, ve kterém funkci přesměrování není plně podporována a Tedious pro Node.JS, ve které přesměrování nebyla implementována.</span><span class="sxs-lookup"><span data-stu-id="953c3-111">Exceptions to this include JDBC 4.0 in which the redirection feature is not fully supported and Tedious for Node.JS in which redirection was not implemented.</span></span>

<span data-ttu-id="953c3-112">Pro "Klienty nižší úrovně" tj. by měl být které podporu TDS 7.3 a pod - server plně kvalifikovaný název domény pro připojení řetězec verze upraven:</span><span class="sxs-lookup"><span data-stu-id="953c3-112">For "Downlevel clients", i.e. which support TDS version 7.3 and below - the server FQDN in the connection string should be modified:</span></span>

<span data-ttu-id="953c3-113">Původní server plně kvalifikovaný název domény v připojovacím řetězci: <*název serveru*>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="953c3-113">Original server FQDN in the connection string: <*server name*>.database.windows.net</span></span>

<span data-ttu-id="953c3-114">Upravené plně kvalifikovaný název domény serveru v připojovacím řetězci: <*název serveru*> .database. **zabezpečené**. windows.net</span><span class="sxs-lookup"><span data-stu-id="953c3-114">Modified server FQDN in the connection string: <*server name*>.database.**secure**.windows.net</span></span>

<span data-ttu-id="953c3-115">Částečný seznam "Klienty nižší úrovně" zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="953c3-115">A partial list of "Downlevel clients" includes:</span></span>

* <span data-ttu-id="953c3-116">Rozhraní .NET 4.0 a níže,</span><span class="sxs-lookup"><span data-stu-id="953c3-116">.NET 4.0 and below,</span></span>
* <span data-ttu-id="953c3-117">ODBC 10.0 a níže.</span><span class="sxs-lookup"><span data-stu-id="953c3-117">ODBC 10.0 and below.</span></span>
* <span data-ttu-id="953c3-118">JDBC (při JDBC podporuje TDS 7.4, TDS přesměrování funkce není podporována plně)</span><span class="sxs-lookup"><span data-stu-id="953c3-118">JDBC (while JDBC does support TDS 7.4, the TDS redirection feature is not fully supported)</span></span>
* <span data-ttu-id="953c3-119">Zdlouhavé (pro platformu Node.JS)</span><span class="sxs-lookup"><span data-stu-id="953c3-119">Tedious (for Node.JS)</span></span>

<span data-ttu-id="953c3-120">**Poznámka:** výše uvedeném serveru úpravy plně kvalifikovaného názvu domény může být užitečné také pro použití zásad auditování na úrovni SQL serveru bez potřebu konfiguraci krok v každé databázi (dočasné zmírnění).</span><span class="sxs-lookup"><span data-stu-id="953c3-120">**Remark:** The above server FDQN modification may be useful also for applying a SQL Server Level Auditing policy without a need for a configuration step in each database (Temporary mitigation).</span></span>

## <span data-ttu-id="953c3-121"><a id="subheading-2"></a>Koncový bod IP adresy se změní při zapínání auditování</span><span class="sxs-lookup"><span data-stu-id="953c3-121"><a id="subheading-2"></a>IP endpoint changes when enabling Auditing</span></span>
<span data-ttu-id="953c3-122">Upozorňujeme, že když povolíte auditování tabulka, se změní koncový bod IP adresy vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="953c3-122">Please note that when you enable Table Auditing, the IP endpoint of your database will change.</span></span> <span data-ttu-id="953c3-123">Pokud máte možnost nastavení striktní brány firewall, aktualizujte prosím tato nastavení brány firewall odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="953c3-123">If you have strict firewall settings, please update those firewall settings accordingly.</span></span>

<span data-ttu-id="953c3-124">Nový koncový bod IP databáze bude záviset na oblasti databáze:</span><span class="sxs-lookup"><span data-stu-id="953c3-124">The new database IP endpoint will depend on the database region:</span></span>

| <span data-ttu-id="953c3-125">Oblast databáze</span><span class="sxs-lookup"><span data-stu-id="953c3-125">Database Region</span></span> | <span data-ttu-id="953c3-126">Možné koncové body IP</span><span class="sxs-lookup"><span data-stu-id="953c3-126">Possible IP endpoints</span></span> |
| --- | --- |
| <span data-ttu-id="953c3-127">Čína – sever</span><span class="sxs-lookup"><span data-stu-id="953c3-127">China North</span></span> |<span data-ttu-id="953c3-128">139.217.29.176, 139.217.28.254</span><span class="sxs-lookup"><span data-stu-id="953c3-128">139.217.29.176, 139.217.28.254</span></span> |
| <span data-ttu-id="953c3-129">Čína – východ</span><span class="sxs-lookup"><span data-stu-id="953c3-129">China East</span></span> |<span data-ttu-id="953c3-130">42.159.245.65, 42.159.246.245</span><span class="sxs-lookup"><span data-stu-id="953c3-130">42.159.245.65, 42.159.246.245</span></span> |
| <span data-ttu-id="953c3-131">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="953c3-131">Australia East</span></span> |<span data-ttu-id="953c3-132">104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94</span><span class="sxs-lookup"><span data-stu-id="953c3-132">104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94</span></span> |
| <span data-ttu-id="953c3-133">Austrálie – jihovýchod</span><span class="sxs-lookup"><span data-stu-id="953c3-133">Australia Southeast</span></span> |<span data-ttu-id="953c3-134">191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130</span><span class="sxs-lookup"><span data-stu-id="953c3-134">191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130</span></span> |
| <span data-ttu-id="953c3-135">Brazílie – jih</span><span class="sxs-lookup"><span data-stu-id="953c3-135">Brazil South</span></span> |<span data-ttu-id="953c3-136">104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191</span><span class="sxs-lookup"><span data-stu-id="953c3-136">104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191</span></span> |
| <span data-ttu-id="953c3-137">Střed USA</span><span class="sxs-lookup"><span data-stu-id="953c3-137">Central US</span></span> |<span data-ttu-id="953c3-138">104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176</span><span class="sxs-lookup"><span data-stu-id="953c3-138">104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176</span></span> |
| <span data-ttu-id="953c3-139">Střed USA EUAP</span><span class="sxs-lookup"><span data-stu-id="953c3-139">Central US EUAP</span></span> |<span data-ttu-id="953c3-140">52.180.178.16, 52.180.176.190</span><span class="sxs-lookup"><span data-stu-id="953c3-140">52.180.178.16, 52.180.176.190</span></span> |
| <span data-ttu-id="953c3-141">Východní Asie</span><span class="sxs-lookup"><span data-stu-id="953c3-141">East Asia</span></span> |<span data-ttu-id="953c3-142">23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245</span><span class="sxs-lookup"><span data-stu-id="953c3-142">23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245</span></span> |
| <span data-ttu-id="953c3-143">Východní USA 2</span><span class="sxs-lookup"><span data-stu-id="953c3-143">East US 2</span></span> |<span data-ttu-id="953c3-144">104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50</span><span class="sxs-lookup"><span data-stu-id="953c3-144">104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50</span></span> |
| <span data-ttu-id="953c3-145">Východ USA</span><span class="sxs-lookup"><span data-stu-id="953c3-145">East US</span></span> |<span data-ttu-id="953c3-146">23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44</span><span class="sxs-lookup"><span data-stu-id="953c3-146">23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44</span></span> |
| <span data-ttu-id="953c3-147">EUAP východní USA</span><span class="sxs-lookup"><span data-stu-id="953c3-147">East US EUAP</span></span> |<span data-ttu-id="953c3-148">52.225.190.86, 52.225.191.187</span><span class="sxs-lookup"><span data-stu-id="953c3-148">52.225.190.86, 52.225.191.187</span></span> |
| <span data-ttu-id="953c3-149">Střed Indie</span><span class="sxs-lookup"><span data-stu-id="953c3-149">Central India</span></span> |<span data-ttu-id="953c3-150">104.211.98.219, 104.211.103.71</span><span class="sxs-lookup"><span data-stu-id="953c3-150">104.211.98.219, 104.211.103.71</span></span> |
| <span data-ttu-id="953c3-151">Indie – jih</span><span class="sxs-lookup"><span data-stu-id="953c3-151">South India</span></span> |<span data-ttu-id="953c3-152">104.211.227.102, 104.211.225.157</span><span class="sxs-lookup"><span data-stu-id="953c3-152">104.211.227.102, 104.211.225.157</span></span> |
| <span data-ttu-id="953c3-153">Indie – západ</span><span class="sxs-lookup"><span data-stu-id="953c3-153">West India</span></span> |<span data-ttu-id="953c3-154">104.211.161.152, 104.211.162.21</span><span class="sxs-lookup"><span data-stu-id="953c3-154">104.211.161.152, 104.211.162.21</span></span> |
| <span data-ttu-id="953c3-155">Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="953c3-155">Japan East</span></span> |<span data-ttu-id="953c3-156">104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196</span><span class="sxs-lookup"><span data-stu-id="953c3-156">104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196</span></span> |
| <span data-ttu-id="953c3-157">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="953c3-157">Japan West</span></span> |<span data-ttu-id="953c3-158">104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198</span><span class="sxs-lookup"><span data-stu-id="953c3-158">104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198</span></span> |
| <span data-ttu-id="953c3-159">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="953c3-159">North Central US</span></span> |<span data-ttu-id="953c3-160">191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231</span><span class="sxs-lookup"><span data-stu-id="953c3-160">191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231</span></span> |
| <span data-ttu-id="953c3-161">Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="953c3-161">North Europe</span></span> |<span data-ttu-id="953c3-162">104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176</span><span class="sxs-lookup"><span data-stu-id="953c3-162">104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176</span></span> |
| <span data-ttu-id="953c3-163">Střed USA – jih</span><span class="sxs-lookup"><span data-stu-id="953c3-163">South Central US</span></span> |<span data-ttu-id="953c3-164">191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66</span><span class="sxs-lookup"><span data-stu-id="953c3-164">191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66</span></span> |
| <span data-ttu-id="953c3-165">Jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="953c3-165">Southeast Asia</span></span> |<span data-ttu-id="953c3-166">104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113</span><span class="sxs-lookup"><span data-stu-id="953c3-166">104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113</span></span> |
| <span data-ttu-id="953c3-167">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="953c3-167">West Europe</span></span> |<span data-ttu-id="953c3-168">104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20</span><span class="sxs-lookup"><span data-stu-id="953c3-168">104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20</span></span> |
| <span data-ttu-id="953c3-169">Západní USA</span><span class="sxs-lookup"><span data-stu-id="953c3-169">West US</span></span> |<span data-ttu-id="953c3-170">191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91</span><span class="sxs-lookup"><span data-stu-id="953c3-170">191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91</span></span> |
| <span data-ttu-id="953c3-171">Západní USA 2</span><span class="sxs-lookup"><span data-stu-id="953c3-171">West US 2</span></span> |<span data-ttu-id="953c3-172">13.66.224.156, 13.66.227.8</span><span class="sxs-lookup"><span data-stu-id="953c3-172">13.66.224.156, 13.66.227.8</span></span> |
| <span data-ttu-id="953c3-173">Západní střed USA</span><span class="sxs-lookup"><span data-stu-id="953c3-173">West Central US</span></span> |<span data-ttu-id="953c3-174">52.161.29.186, 52.161.27.213</span><span class="sxs-lookup"><span data-stu-id="953c3-174">52.161.29.186, 52.161.27.213</span></span> |
| <span data-ttu-id="953c3-175">Střední Kanada</span><span class="sxs-lookup"><span data-stu-id="953c3-175">Canada Central</span></span> |<span data-ttu-id="953c3-176">13.88.248.106, 13.88.248.110</span><span class="sxs-lookup"><span data-stu-id="953c3-176">13.88.248.106, 13.88.248.110</span></span> |
| <span data-ttu-id="953c3-177">Východní Kanada</span><span class="sxs-lookup"><span data-stu-id="953c3-177">Canada East</span></span> |<span data-ttu-id="953c3-178">40.86.227.82, 40.86.225.194</span><span class="sxs-lookup"><span data-stu-id="953c3-178">40.86.227.82, 40.86.225.194</span></span> |
| <span data-ttu-id="953c3-179">Spojené království – sever</span><span class="sxs-lookup"><span data-stu-id="953c3-179">UK North</span></span> |<span data-ttu-id="953c3-180">13.87.101.18, 13.87.100.232</span><span class="sxs-lookup"><span data-stu-id="953c3-180">13.87.101.18, 13.87.100.232</span></span> |
| <span data-ttu-id="953c3-181">Spojené království – jih 2</span><span class="sxs-lookup"><span data-stu-id="953c3-181">UK South 2</span></span> |<span data-ttu-id="953c3-182">13.87.32.202, 13.87.32.226</span><span class="sxs-lookup"><span data-stu-id="953c3-182">13.87.32.202, 13.87.32.226</span></span> |