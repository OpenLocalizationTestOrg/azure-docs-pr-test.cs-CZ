---
title: "Podporované zdroje dat v Azure Data Catalog | Microsoft Docs"
description: "V tomto článku jsou uvedeny specifikace zdrojů dat aktuálně podporované."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: jstevens
editor: 
tags: 
ms.assetid: fd4345ca-2ed8-4c5e-9c4b-f954be2fc9f9
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: d6867c73bc6ea3c238cceef8a68466d451f3365c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="supported-data-sources-in-azure-data-catalog"></a><span data-ttu-id="2cdca-103">Podporované zdroje dat v Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="2cdca-103">Supported data sources in Azure Data Catalog</span></span>

<span data-ttu-id="2cdca-104">Metadata můžete publikovat pomocí veřejné rozhraní API nebo klikněte na-jednou registrace Nástroje, nebo když ručně zadáte informace přímo do Azure Data Catalog webový portál.</span><span class="sxs-lookup"><span data-stu-id="2cdca-104">You can publish metadata by using a public API or a click-once registration tool, or by manually entering information directly to the Azure Data Catalog web portal.</span></span> <span data-ttu-id="2cdca-105">Následující tabulka shrnuje všechny zdroje dat, které jsou podporovány v katalogu dnes a možnosti publikování pro každou.</span><span class="sxs-lookup"><span data-stu-id="2cdca-105">The following table summarizes all data sources that are supported by the catalog today, and the publishing capabilities for each.</span></span> <span data-ttu-id="2cdca-106">Externí data nástroje, které všechny zdroje dat, můžete spustit z našich zkušeností portál "open in" jsou také uvedeny.</span><span class="sxs-lookup"><span data-stu-id="2cdca-106">Also listed are the external data tools that each data source can launch from our portal "open-in" experience.</span></span> <span data-ttu-id="2cdca-107">Druhá tabulka obsahuje další technické specifikace každé připojení vlastnosti zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="2cdca-107">The second table contains a more technical specification of each data-source connection property.</span></span>


## <a name="list-of-supported-data-sources"></a><span data-ttu-id="2cdca-108">Seznam podporovaných zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="2cdca-108">List of supported data sources</span></span>

<table>
    <tr>
       <td><span data-ttu-id="2cdca-109"><b>Objekt zdroje dat</b></span><span class="sxs-lookup"><span data-stu-id="2cdca-109"><b>Data source object</b></span></span></td>
       <td><span data-ttu-id="2cdca-110"><b>Rozhraní API</b></span><span class="sxs-lookup"><span data-stu-id="2cdca-110"><b>API</b></span></span></td>
       <td><span data-ttu-id="2cdca-111"><b>Ruční zadání</b></span><span class="sxs-lookup"><span data-stu-id="2cdca-111"><b>Manual entry</b></span></span></td>
       <td><span data-ttu-id="2cdca-112"><b>Nástroj pro registraci</b></span><span class="sxs-lookup"><span data-stu-id="2cdca-112"><b>Registration tool</b></span></span></td>
       <td><span data-ttu-id="2cdca-113"><b>Nástroje pro Open in</b></span><span class="sxs-lookup"><span data-stu-id="2cdca-113"><b>Open-in tools</b></span></span></td>
       <td><span data-ttu-id="2cdca-114"><b>Poznámky k</b></span><span class="sxs-lookup"><span data-stu-id="2cdca-114"><b>Notes</b></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-115">Adresář Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2cdca-115">Azure Data Lake Store directory</span></span></td>
      <td><span data-ttu-id="2cdca-116">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-116">✓</span></span></td>
      <td><span data-ttu-id="2cdca-117">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-117">✓</span></span></td>
      <td><span data-ttu-id="2cdca-118">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-118">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-119">Soubor Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2cdca-119">Azure Data Lake Store file</span></span></td>
      <td><span data-ttu-id="2cdca-120">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-120">✓</span></span></td>
      <td><span data-ttu-id="2cdca-121">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-121">✓</span></span></td>
      <td><span data-ttu-id="2cdca-122">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-122">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-123">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="2cdca-123">Azure Blob storage</span></span></td>
      <td><span data-ttu-id="2cdca-124">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-124">✓</span></span></td>
      <td><span data-ttu-id="2cdca-125">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-125">✓</span></span></td>
      <td><span data-ttu-id="2cdca-126">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-126">✓</span></span></td>
      <td><span data-ttu-id="2cdca-127"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-127"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-128">Adresář úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="2cdca-128">Azure Storage directory</span></span></td>
      <td><span data-ttu-id="2cdca-129">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-129">✓</span></span></td>
      <td><span data-ttu-id="2cdca-130">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-130">✓</span></span></td>
      <td><span data-ttu-id="2cdca-131">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-131">✓</span></span></td>
      <td><span data-ttu-id="2cdca-132"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-132"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-133">Tabulky Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2cdca-133">Azure Storage table</span></span></td>
      <td><span data-ttu-id="2cdca-134">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-134">✓</span></span></td>
      <td><span data-ttu-id="2cdca-135">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-135">✓</span></span></td>
      <td><span data-ttu-id="2cdca-136">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-136">✓</span></span></td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-137">HDFS adresáře</span><span class="sxs-lookup"><span data-stu-id="2cdca-137">HDFS directory</span></span></td>
      <td><span data-ttu-id="2cdca-138">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-138">✓</span></span></td>
      <td><span data-ttu-id="2cdca-139">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-139">✓</span></span></td>
      <td><span data-ttu-id="2cdca-140">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-140">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-141">HDFS souboru</span><span class="sxs-lookup"><span data-stu-id="2cdca-141">HDFS file</span></span></td>
      <td><span data-ttu-id="2cdca-142">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-142">✓</span></span></td>
      <td><span data-ttu-id="2cdca-143">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-143">✓</span></span></td>
      <td><span data-ttu-id="2cdca-144">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-144">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-145">Tabulku Hive</span><span class="sxs-lookup"><span data-stu-id="2cdca-145">Hive table</span></span></td>
      <td><span data-ttu-id="2cdca-146">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-146">✓</span></span></td>
      <td><span data-ttu-id="2cdca-147">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-147">✓</span></span></td>
      <td><span data-ttu-id="2cdca-148">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-148">✓</span></span></td>
      <td><span data-ttu-id="2cdca-149"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-149"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-150">Zobrazení Hive</span><span class="sxs-lookup"><span data-stu-id="2cdca-150">Hive view</span></span></td>
      <td><span data-ttu-id="2cdca-151">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-151">✓</span></span></td>
      <td><span data-ttu-id="2cdca-152">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-152">✓</span></span></td>
      <td><span data-ttu-id="2cdca-153">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-153">✓</span></span></td>
      <td><span data-ttu-id="2cdca-154"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-154"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-155">Tabulka MySQL</span><span class="sxs-lookup"><span data-stu-id="2cdca-155">MySQL table</span></span></td>
      <td><span data-ttu-id="2cdca-156">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-156">✓</span></span></td>
      <td><span data-ttu-id="2cdca-157">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-157">✓</span></span></td>
      <td><span data-ttu-id="2cdca-158">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-158">✓</span></span></td>
      <td><span data-ttu-id="2cdca-159"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-159"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-160">Zobrazení MySQL</span><span class="sxs-lookup"><span data-stu-id="2cdca-160">MySQL view</span></span></td>
      <td><span data-ttu-id="2cdca-161">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-161">✓</span></span></td>
      <td><span data-ttu-id="2cdca-162">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-162">✓</span></span></td>
      <td><span data-ttu-id="2cdca-163">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-163">✓</span></span></td>
      <td><span data-ttu-id="2cdca-164"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-164"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-165">Tabulka databáze Oracle</span><span class="sxs-lookup"><span data-stu-id="2cdca-165">Oracle Database table</span></span></td>
      <td><span data-ttu-id="2cdca-166">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-166">✓</span></span></td>
      <td><span data-ttu-id="2cdca-167">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-167">✓</span></span></td>
      <td><span data-ttu-id="2cdca-168">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-168">✓</span></span></td>
      <td><span data-ttu-id="2cdca-169"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-169"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-170">Zobrazení databáze Oracle</span><span class="sxs-lookup"><span data-stu-id="2cdca-170">Oracle Database view</span></span></td>
      <td><span data-ttu-id="2cdca-171">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-171">✓</span></span></td>
      <td><span data-ttu-id="2cdca-172">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-172">✓</span></span></td>
      <td><span data-ttu-id="2cdca-173">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-173">✓</span></span></td>
      <td><span data-ttu-id="2cdca-174"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-174"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-175">Jiné (Obecné asset)</span><span class="sxs-lookup"><span data-stu-id="2cdca-175">Other (generic asset)</span></span></td>
      <td><span data-ttu-id="2cdca-176">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-176">✓</span></span></td>
      <td><span data-ttu-id="2cdca-177">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-177">✓</span></span></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-178">Tabulky Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2cdca-178">Azure SQL Data Warehouse table</span></span></td>
      <td><span data-ttu-id="2cdca-179">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-179">✓</span></span></td>
      <td><span data-ttu-id="2cdca-180">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-180">✓</span></span></td>
      <td><span data-ttu-id="2cdca-181">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-181">✓</span></span></td>
      <td><span data-ttu-id="2cdca-182"><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-182"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-183">Zobrazení SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2cdca-183">SQL Data Warehouse view</span></span></td>
      <td><span data-ttu-id="2cdca-184">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-184">✓</span></span></td>
      <td><span data-ttu-id="2cdca-185">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-185">✓</span></span></td>
      <td><span data-ttu-id="2cdca-186">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-186">✓</span></span></td>
      <td><span data-ttu-id="2cdca-187"><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-187"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-188">Dimenze služby SQL Server Analysis Services</span><span class="sxs-lookup"><span data-stu-id="2cdca-188">SQL Server Analysis Services dimension</span></span></td>
      <td><span data-ttu-id="2cdca-189">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-189">✓</span></span></td>
      <td><span data-ttu-id="2cdca-190">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-190">✓</span></span></td>
      <td><span data-ttu-id="2cdca-191">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-191">✓</span></span></td>
      <td><span data-ttu-id="2cdca-192"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-192"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-193">SQL Server Analysis Services klíčového ukazatele výkonu</span><span class="sxs-lookup"><span data-stu-id="2cdca-193">SQL Server Analysis Services KPI</span></span></td>
      <td><span data-ttu-id="2cdca-194">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-194">✓</span></span></td>
      <td><span data-ttu-id="2cdca-195">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-195">✓</span></span></td>
      <td><span data-ttu-id="2cdca-196">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-196">✓</span></span></td>
      <td><span data-ttu-id="2cdca-197"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-197"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-198">Míra SQL Server Analysis Services</span><span class="sxs-lookup"><span data-stu-id="2cdca-198">SQL Server Analysis Services measure</span></span></td>
      <td><span data-ttu-id="2cdca-199">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-199">✓</span></span></td>
      <td><span data-ttu-id="2cdca-200">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-200">✓</span></span></td>
      <td><span data-ttu-id="2cdca-201">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-201">✓</span></span></td>
      <td><span data-ttu-id="2cdca-202"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-202"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-203">Tabulky SQL Server Analysis Services</span><span class="sxs-lookup"><span data-stu-id="2cdca-203">SQL Server Analysis Services table</span></span></td>
      <td><span data-ttu-id="2cdca-204">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-204">✓</span></span></td>
      <td><span data-ttu-id="2cdca-205">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-205">✓</span></span></td>
      <td><span data-ttu-id="2cdca-206">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-206">✓</span></span></td>
      <td><span data-ttu-id="2cdca-207"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-207"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-208">Sestavy služby SQL Server Reporting Services</span><span class="sxs-lookup"><span data-stu-id="2cdca-208">SQL Server Reporting Services report</span></span></td>
      <td><span data-ttu-id="2cdca-209">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-209">✓</span></span></td>
      <td><span data-ttu-id="2cdca-210">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-210">✓</span></span></td>
      <td><span data-ttu-id="2cdca-211">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-211">✓</span></span></td>
      <td><span data-ttu-id="2cdca-212"><font size=2>Prohlížeč</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-212"><font size=2>Browser</font></span></span></td>
      <td><span data-ttu-id="2cdca-213"><font size=2>Pouze servery v nativním režimu. Režim serveru SharePoint není podporována.</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-213"><font size=2>Native mode servers only. SharePoint mode is not supported.</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-214">Tabulka systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-214">SQL Server table</span></span></td>
      <td><span data-ttu-id="2cdca-215">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-215">✓</span></span></td>
      <td><span data-ttu-id="2cdca-216">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-216">✓</span></span></td>
      <td><span data-ttu-id="2cdca-217">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-217">✓</span></span></td>
      <td><span data-ttu-id="2cdca-218"><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-218"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-219">Zobrazení systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-219">SQL Server view</span></span></td>
      <td><span data-ttu-id="2cdca-220">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-220">✓</span></span></td>
      <td><span data-ttu-id="2cdca-221">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-221">✓</span></span></td>
      <td><span data-ttu-id="2cdca-222">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-222">✓</span></span></td>
      <td><span data-ttu-id="2cdca-223"><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-223"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-224">Tabulka Teradata</span><span class="sxs-lookup"><span data-stu-id="2cdca-224">Teradata table</span></span></td>
      <td><span data-ttu-id="2cdca-225">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-225">✓</span></span></td>
      <td><span data-ttu-id="2cdca-226">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-226">✓</span></span></td>
      <td><span data-ttu-id="2cdca-227">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-227">✓</span></span></td>
      <td><span data-ttu-id="2cdca-228"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-228"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-229">Zobrazení Teradata</span><span class="sxs-lookup"><span data-stu-id="2cdca-229">Teradata view</span></span></td>
      <td><span data-ttu-id="2cdca-230">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-230">✓</span></span></td>
      <td><span data-ttu-id="2cdca-231">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-231">✓</span></span></td>
      <td><span data-ttu-id="2cdca-232">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-232">✓</span></span></td>
      <td><span data-ttu-id="2cdca-233"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-233"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-234">Zobrazení SAP HANA</span><span class="sxs-lookup"><span data-stu-id="2cdca-234">SAP HANA view</span></span></td>
      <td><span data-ttu-id="2cdca-235">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-235">✓</span></span></td>
      <td><span data-ttu-id="2cdca-236">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-236">✓</span></span></td>
      <td><span data-ttu-id="2cdca-237">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-237">✓</span></span></td>
      <td><span data-ttu-id="2cdca-238"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-238"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-239">Tabulka DB2</span><span class="sxs-lookup"><span data-stu-id="2cdca-239">DB2 table</span></span></td>
      <td><span data-ttu-id="2cdca-240">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-240">✓</span></span></td>
      <td><span data-ttu-id="2cdca-241">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-241">✓</span></span></td>
      <td><span data-ttu-id="2cdca-242">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-242">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-243">Zobrazení DB2</span><span class="sxs-lookup"><span data-stu-id="2cdca-243">DB2 view</span></span></td>
      <td><span data-ttu-id="2cdca-244">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-244">✓</span></span></td>
      <td><span data-ttu-id="2cdca-245">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-245">✓</span></span></td>
      <td><span data-ttu-id="2cdca-246">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-246">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-247">Soubor systému souborů</span><span class="sxs-lookup"><span data-stu-id="2cdca-247">File system file</span></span></td>
      <td><span data-ttu-id="2cdca-248">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-248">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-249">Adresář serveru FTP</span><span class="sxs-lookup"><span data-stu-id="2cdca-249">FTP directory</span></span></td>
      <td><span data-ttu-id="2cdca-250">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-250">✓</span></span></td>
      <td><span data-ttu-id="2cdca-251">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-251">✓</span></span></td>
      <td><span data-ttu-id="2cdca-252">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-252">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-253">Soubor FTP</span><span class="sxs-lookup"><span data-stu-id="2cdca-253">FTP file</span></span></td>
      <td><span data-ttu-id="2cdca-254">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-254">✓</span></span></td>
      <td><span data-ttu-id="2cdca-255">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-255">✓</span></span></td>
      <td><span data-ttu-id="2cdca-256">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-256">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-257">Sestavy HTTP</span><span class="sxs-lookup"><span data-stu-id="2cdca-257">HTTP report</span></span></td>
      <td><span data-ttu-id="2cdca-258">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-258">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-259">Koncový bod HTTP</span><span class="sxs-lookup"><span data-stu-id="2cdca-259">HTTP endpoint</span></span></td>
      <td><span data-ttu-id="2cdca-260">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-260">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-261">Soubor protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="2cdca-261">HTTP file</span></span></td>
      <td><span data-ttu-id="2cdca-262">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-262">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-263">Sada entit OData</span><span class="sxs-lookup"><span data-stu-id="2cdca-263">OData entity set</span></span></td>
      <td><span data-ttu-id="2cdca-264">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-264">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-265">Funkce OData</span><span class="sxs-lookup"><span data-stu-id="2cdca-265">OData function</span></span></td>
      <td><span data-ttu-id="2cdca-266">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-266">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-267">Tabulka PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="2cdca-267">PostgreSQL table</span></span></td>
      <td><span data-ttu-id="2cdca-268">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-268">✓</span></span></td>
      <td><span data-ttu-id="2cdca-269">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-269">✓</span></span></td>
      <td><span data-ttu-id="2cdca-270">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-270">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-271">PostgreSQL zobrazení</span><span class="sxs-lookup"><span data-stu-id="2cdca-271">PostgreSQL view</span></span></td>
      <td><span data-ttu-id="2cdca-272">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-272">✓</span></span></td>
      <td><span data-ttu-id="2cdca-273">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-273">✓</span></span></td>
      <td><span data-ttu-id="2cdca-274">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-274">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-275">Zobrazení SAP HANA</span><span class="sxs-lookup"><span data-stu-id="2cdca-275">SAP HANA view</span></span></td>
      <td><span data-ttu-id="2cdca-276">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-276">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td> <span data-ttu-id="2cdca-277">Objekt služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="2cdca-277">Salesforce object</span></span></td>
      <td><span data-ttu-id="2cdca-278">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-278">✓</span></span></td>
      <td><span data-ttu-id="2cdca-279">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-279">✓</span></span></td>
      <td><span data-ttu-id="2cdca-280">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-280">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-281">Seznam serveru SharePoint</span><span class="sxs-lookup"><span data-stu-id="2cdca-281">SharePoint list</span></span> </td>
      <td><span data-ttu-id="2cdca-282">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-282">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-283">Azure Cosmos DB kolekce</span><span class="sxs-lookup"><span data-stu-id="2cdca-283">Azure Cosmos DB collection</span></span></td>
      <td><span data-ttu-id="2cdca-284">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-284">✓</span></span></td>
      <td><span data-ttu-id="2cdca-285">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-285">✓</span></span></td>
      <td><span data-ttu-id="2cdca-286">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-286">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-287">Obecná tabulka rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="2cdca-287">Generic ODBC table</span></span></td>
      <td><span data-ttu-id="2cdca-288">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-288">✓</span></span></td>
      <td><span data-ttu-id="2cdca-289">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-289">✓</span></span></td>
      <td><span data-ttu-id="2cdca-290">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-290">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-291">Obecná zobrazení rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="2cdca-291">Generic ODBC view</span></span></td>
      <td><span data-ttu-id="2cdca-292">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-292">✓</span></span></td>
      <td><span data-ttu-id="2cdca-293">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-293">✓</span></span></td>
      <td><span data-ttu-id="2cdca-294">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-294">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-295">Tabulka Cassandra</span><span class="sxs-lookup"><span data-stu-id="2cdca-295">Cassandra table</span></span></td>
      <td><span data-ttu-id="2cdca-296">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-296">✓</span></span></td>
      <td><span data-ttu-id="2cdca-297">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-297">✓</span></span></td>
      <td><span data-ttu-id="2cdca-298">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-298">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="2cdca-299"><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-299"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-300">Cassandra zobrazení</span><span class="sxs-lookup"><span data-stu-id="2cdca-300">Cassandra view</span></span></td>
      <td><span data-ttu-id="2cdca-301">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-301">✓</span></span></td>
      <td><span data-ttu-id="2cdca-302">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-302">✓</span></span></td>
      <td><span data-ttu-id="2cdca-303">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-303">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="2cdca-304"><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-304"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-305">Tabulka Sybase</span><span class="sxs-lookup"><span data-stu-id="2cdca-305">Sybase table</span></span></td>
      <td><span data-ttu-id="2cdca-306">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-306">✓</span></span></td>
      <td><span data-ttu-id="2cdca-307">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-307">✓</span></span></td>
      <td><span data-ttu-id="2cdca-308">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-308">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-309">Zobrazení Sybase</span><span class="sxs-lookup"><span data-stu-id="2cdca-309">Sybase view</span></span></td>
      <td><span data-ttu-id="2cdca-310">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-310">✓</span></span></td>
      <td><span data-ttu-id="2cdca-311">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-311">✓</span></span></td>
      <td><span data-ttu-id="2cdca-312">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-312">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-313">Tabulka MongoDB</span><span class="sxs-lookup"><span data-stu-id="2cdca-313">MongoDB table</span></span></td>
      <td><span data-ttu-id="2cdca-314">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-314">✓</span></span></td>
      <td><span data-ttu-id="2cdca-315">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-315">✓</span></span></td>
      <td><span data-ttu-id="2cdca-316">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-316">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="2cdca-317"><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-317"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-318">Zobrazení MongoDB</span><span class="sxs-lookup"><span data-stu-id="2cdca-318">MongoDB view</span></span></td>
      <td><span data-ttu-id="2cdca-319">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-319">✓</span></span></td>
      <td><span data-ttu-id="2cdca-320">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-320">✓</span></span></td>
      <td><span data-ttu-id="2cdca-321">✓</span><span class="sxs-lookup"><span data-stu-id="2cdca-321">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="2cdca-322"><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></span><span class="sxs-lookup"><span data-stu-id="2cdca-322"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
</table>

<span data-ttu-id="2cdca-323">Pokud potřebujete podporu pro další zdroje, podat žádost o funkci [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="2cdca-323">If you need support for additional sources, submit a feature request to the [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).</span></span>


## <a name="data-source-reference-specification"></a><span data-ttu-id="2cdca-324">Specifikace odkaz na zdroj dat</span><span class="sxs-lookup"><span data-stu-id="2cdca-324">Data-source reference specification</span></span>
> [!NOTE]
> <span data-ttu-id="2cdca-325">**DSL struktura** sloupci v následující tabulce jsou uvedeny pouze vlastnosti připojení pro kontejner objektů a dat "address" používané Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="2cdca-325">The **DSL structure** column in the following table lists only the connection properties for "address" property bag that are used by Azure Data Catalog.</span></span> <span data-ttu-id="2cdca-326">To znamená balík vlastností "address" může obsahovat další vlastnosti připojení zdroje dat, které Azure Data Catalog potrvají, ale nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="2cdca-326">That is, "address" property bag can contain other connection properties of the data source which Azure Data Catalog persists, but does not use.</span></span>

<table>
    <tr>
       <td><span data-ttu-id="2cdca-327"><b>Typ zdroje</b></span><span class="sxs-lookup"><span data-stu-id="2cdca-327"><b>Source type</b></span></span></td>
       <td><span data-ttu-id="2cdca-328"><b>Typ prostředku</b></span><span class="sxs-lookup"><span data-stu-id="2cdca-328"><b>Asset type</b></span></span></td>
       <td><span data-ttu-id="2cdca-329"><b>Typy objektů</b></span><span class="sxs-lookup"><span data-stu-id="2cdca-329"><b>Object types</b></span></span></td>
       <td><span data-ttu-id="2cdca-330"><b>Struktura DSL<b></span><span class="sxs-lookup"><span data-stu-id="2cdca-330"><b>DSL structure<b></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-331">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2cdca-331">Azure Data Lake Store</span></span></td>
      <td><span data-ttu-id="2cdca-332">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-332">Container</span></span></td>
      <td><span data-ttu-id="2cdca-333">Data Lake</span><span class="sxs-lookup"><span data-stu-id="2cdca-333">Data Lake</span></span></td>
      <td><span data-ttu-id="2cdca-334">
        <font size=2>Protokol: webhdfs</span><span class="sxs-lookup"><span data-stu-id="2cdca-334">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="2cdca-335">Ověřování: {základní, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-335">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="2cdca-336">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-336">Address:</span></span> <br><span data-ttu-id="2cdca-337">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-337">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-338">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2cdca-338">Azure Data Lake Store</span></span></td>
      <td><span data-ttu-id="2cdca-339">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-339">Table</span></span></td>
      <td><span data-ttu-id="2cdca-340">Adresář, soubor</span><span class="sxs-lookup"><span data-stu-id="2cdca-340">Directory, file</span></span></td>
      <td><span data-ttu-id="2cdca-341">
        <font size=2>Protokol: webhdfs</span><span class="sxs-lookup"><span data-stu-id="2cdca-341">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="2cdca-342">Ověřování: {základní, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-342">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="2cdca-343">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-343">Address:</span></span> <br><span data-ttu-id="2cdca-344">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-344">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-345">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2cdca-345">Azure Storage</span></span></td>
      <td><span data-ttu-id="2cdca-346">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-346">Container</span></span></td>
      <td><span data-ttu-id="2cdca-347">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-347">Container</span></span></td>
      <td><span data-ttu-id="2cdca-348">
        <font size=2>Protokol:-objektů BLOB služby azure</span><span class="sxs-lookup"><span data-stu-id="2cdca-348">
        <font size=2> Protocol: azure-blobs</span></span> <br><span data-ttu-id="2cdca-349">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="2cdca-349">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2cdca-350">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-350">Address:</span></span> <br><span data-ttu-id="2cdca-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény</span><span class="sxs-lookup"><span data-stu-id="2cdca-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="2cdca-352">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet</span><span class="sxs-lookup"><span data-stu-id="2cdca-352">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="2cdca-353">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontejner</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-353">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-354">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2cdca-354">Azure Storage</span></span></td>
      <td><span data-ttu-id="2cdca-355">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-355">Table</span></span></td>
      <td><span data-ttu-id="2cdca-356">Objekt BLOB, adresář</span><span class="sxs-lookup"><span data-stu-id="2cdca-356">Blob, directory</span></span></td>
      <td><span data-ttu-id="2cdca-357">
        <font size=2>Protokol:-objektů BLOB služby azure</span><span class="sxs-lookup"><span data-stu-id="2cdca-357">
        <font size=2> Protocol: azure-blobs</span></span> <br><span data-ttu-id="2cdca-358">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="2cdca-358">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2cdca-359">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-359">Address:</span></span> <br><span data-ttu-id="2cdca-360">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény</span><span class="sxs-lookup"><span data-stu-id="2cdca-360">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="2cdca-361">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet</span><span class="sxs-lookup"><span data-stu-id="2cdca-361">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="2cdca-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container</span></span> <br><span data-ttu-id="2cdca-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Jméno</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-364">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2cdca-364">Azure Storage</span></span></td>
      <td><span data-ttu-id="2cdca-365">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-365">Container</span></span></td>
      <td><span data-ttu-id="2cdca-366">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-366">Container</span></span></td>
      <td><span data-ttu-id="2cdca-367">
        <font size=2>Protokol: azure – tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-367">
        <font size=2> Protocol: azure-tables</span></span> <br><span data-ttu-id="2cdca-368">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="2cdca-368">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2cdca-369">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-369">Address:</span></span> <br><span data-ttu-id="2cdca-370">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény</span><span class="sxs-lookup"><span data-stu-id="2cdca-370">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="2cdca-371">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-371">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-372">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2cdca-372">Azure Storage</span></span></td>
      <td><span data-ttu-id="2cdca-373">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-373">Table</span></span></td>
      <td><span data-ttu-id="2cdca-374">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-374">Table</span></span></td>
      <td><span data-ttu-id="2cdca-375">
        <font size=2>Protokol: azure – tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-375">
        <font size=2> Protocol: azure-tables</span></span> <br><span data-ttu-id="2cdca-376">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="2cdca-376">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2cdca-377">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-377">Address:</span></span> <br><span data-ttu-id="2cdca-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény</span><span class="sxs-lookup"><span data-stu-id="2cdca-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="2cdca-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet</span><span class="sxs-lookup"><span data-stu-id="2cdca-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="2cdca-380">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Jméno</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-380">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-381">Cosmos</span><span class="sxs-lookup"><span data-stu-id="2cdca-381">Cosmos</span></span></td>
      <td><span data-ttu-id="2cdca-382">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-382">Container</span></span></td>
      <td><span data-ttu-id="2cdca-383">Virtuální cluster</span><span class="sxs-lookup"><span data-stu-id="2cdca-383">Virtual cluster</span></span></td>
      <td><span data-ttu-id="2cdca-384">
        <font size=2>Protokol: cosmos</span><span class="sxs-lookup"><span data-stu-id="2cdca-384">
        <font size=2> Protocol: cosmos</span></span> <br><span data-ttu-id="2cdca-385">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-385">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-386">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-386">Address:</span></span> <br><span data-ttu-id="2cdca-387">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-387">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-388">Cosmos</span><span class="sxs-lookup"><span data-stu-id="2cdca-388">Cosmos</span></span></td>
      <td><span data-ttu-id="2cdca-389">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-389">Table</span></span></td>
      <td><span data-ttu-id="2cdca-390">Datový proud, sadu datového proudu, zobrazení</span><span class="sxs-lookup"><span data-stu-id="2cdca-390">Stream, stream set, view</span></span></td>
      <td><span data-ttu-id="2cdca-391">
        <font size=2>Protokol: cosmos</span><span class="sxs-lookup"><span data-stu-id="2cdca-391">
        <font size=2> Protocol: cosmos</span></span> <br><span data-ttu-id="2cdca-392">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-392">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-393">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-393">Address:</span></span> <br><span data-ttu-id="2cdca-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-395">Datazen</span><span class="sxs-lookup"><span data-stu-id="2cdca-395">Datazen</span></span></td>
      <td><span data-ttu-id="2cdca-396">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-396">Container</span></span></td>
      <td><span data-ttu-id="2cdca-397">Lokality</span><span class="sxs-lookup"><span data-stu-id="2cdca-397">Site</span></span></td>
      <td><span data-ttu-id="2cdca-398">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="2cdca-398">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2cdca-399">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-399">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2cdca-400">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-400">Address:</span></span> <br><span data-ttu-id="2cdca-401">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-401">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-402">Datazen</span><span class="sxs-lookup"><span data-stu-id="2cdca-402">Datazen</span></span></td>
      <td><span data-ttu-id="2cdca-403">Sestava</span><span class="sxs-lookup"><span data-stu-id="2cdca-403">Report</span></span></td>
      <td><span data-ttu-id="2cdca-404">Sestavy, řídicí panel</span><span class="sxs-lookup"><span data-stu-id="2cdca-404">Report, dashboard</span></span></td>
      <td><span data-ttu-id="2cdca-405">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="2cdca-405">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2cdca-406">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-406">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2cdca-407">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-407">Address:</span></span> <br><span data-ttu-id="2cdca-408">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-408">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-409">DB2</span><span class="sxs-lookup"><span data-stu-id="2cdca-409">DB2</span></span></td>
      <td><span data-ttu-id="2cdca-410">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-410">Container</span></span></td>
      <td><span data-ttu-id="2cdca-411">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-411">Database</span></span></td>
      <td><span data-ttu-id="2cdca-412">
        <font size=2>Protokol: db2</span><span class="sxs-lookup"><span data-stu-id="2cdca-412">
        <font size=2> Protocol: db2</span></span> <br><span data-ttu-id="2cdca-413">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-413">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-414">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-414">Address:</span></span> <br><span data-ttu-id="2cdca-415">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-415">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-416">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-416">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-417">DB2</span><span class="sxs-lookup"><span data-stu-id="2cdca-417">DB2</span></span></td>
      <td><span data-ttu-id="2cdca-418">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-418">Table</span></span></td>
      <td><span data-ttu-id="2cdca-419">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-419">Table, view</span></span></td>
      <td><span data-ttu-id="2cdca-420">
        <font size=2>Protokol: db2</span><span class="sxs-lookup"><span data-stu-id="2cdca-420">
        <font size=2> Protocol: db2</span></span> <br><span data-ttu-id="2cdca-421">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-421">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-422">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-422">Address:</span></span> <br><span data-ttu-id="2cdca-423">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-423">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-424">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-424">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-425">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="2cdca-425">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2cdca-426">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-426">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-427">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="2cdca-427">File system</span></span></td>
      <td><span data-ttu-id="2cdca-428">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-428">Table</span></span></td>
      <td><span data-ttu-id="2cdca-429">File</span><span class="sxs-lookup"><span data-stu-id="2cdca-429">File</span></span></td>
      <td><span data-ttu-id="2cdca-430">
        <font size=2>Protokol: soubor</span><span class="sxs-lookup"><span data-stu-id="2cdca-430">
        <font size=2> Protocol: file</span></span> <br><span data-ttu-id="2cdca-431">Ověřování: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-431">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="2cdca-432">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-432">Address:</span></span> <br><span data-ttu-id="2cdca-433">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cesta</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-433">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-434">FTP</span><span class="sxs-lookup"><span data-stu-id="2cdca-434">FTP</span></span></td>
      <td><span data-ttu-id="2cdca-435">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-435">Table</span></span></td>
      <td><span data-ttu-id="2cdca-436">Adresář, soubor</span><span class="sxs-lookup"><span data-stu-id="2cdca-436">Directory, file</span></span></td>
      <td><span data-ttu-id="2cdca-437">
        <font size=2>Protokol: ftp</span><span class="sxs-lookup"><span data-stu-id="2cdca-437">
        <font size=2> Protocol: ftp</span></span> <br><span data-ttu-id="2cdca-438">Ověřování: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-438">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="2cdca-439">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-439">Address:</span></span> <br><span data-ttu-id="2cdca-440">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-440">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-441">Hadoop Distributed File System</span><span class="sxs-lookup"><span data-stu-id="2cdca-441">Hadoop Distributed File System</span></span></td>
      <td><span data-ttu-id="2cdca-442">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-442">Container</span></span></td>
      <td><span data-ttu-id="2cdca-443">Cluster</span><span class="sxs-lookup"><span data-stu-id="2cdca-443">Cluster</span></span></td>
      <td><span data-ttu-id="2cdca-444">
        <font size=2>Protokol: webhdfs</span><span class="sxs-lookup"><span data-stu-id="2cdca-444">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="2cdca-445">Ověřování: {základní, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-445">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="2cdca-446">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-446">Address:</span></span> <br><span data-ttu-id="2cdca-447">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-447">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-448">Hadoop Distributed File System</span><span class="sxs-lookup"><span data-stu-id="2cdca-448">Hadoop Distributed File System</span></span></td>
      <td><span data-ttu-id="2cdca-449">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-449">Table</span></span></td>
      <td><span data-ttu-id="2cdca-450">Adresář, soubor</span><span class="sxs-lookup"><span data-stu-id="2cdca-450">Directory, file</span></span></td>
      <td><span data-ttu-id="2cdca-451">
        <font size=2>Protokol: webhdfs</span><span class="sxs-lookup"><span data-stu-id="2cdca-451">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="2cdca-452">Ověřování: {základní, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-452">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="2cdca-453">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-453">Address:</span></span> <br><span data-ttu-id="2cdca-454">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-454">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-455">Hive</span><span class="sxs-lookup"><span data-stu-id="2cdca-455">Hive</span></span></td>
      <td><span data-ttu-id="2cdca-456">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-456">Container</span></span></td>
      <td><span data-ttu-id="2cdca-457">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-457">Database</span></span></td>
      <td><span data-ttu-id="2cdca-458">
        <font size=2>Protokol: hive</span><span class="sxs-lookup"><span data-stu-id="2cdca-458">
        <font size=2> Protocol: hive</span></span> <br><span data-ttu-id="2cdca-459">Ověřování: {HDInsight, basic, uživatelské jméno, none}</span><span class="sxs-lookup"><span data-stu-id="2cdca-459">Authentication: {HDInsight, basic, username, none}</span></span> <br><span data-ttu-id="2cdca-460">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-460">Address:</span></span> <br><span data-ttu-id="2cdca-461">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-461">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-462">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-462">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-463">connectionProperties:</span><span class="sxs-lookup"><span data-stu-id="2cdca-463">connectionProperties:</span></span> <br><span data-ttu-id="2cdca-464">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-464">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-465">Hive</span><span class="sxs-lookup"><span data-stu-id="2cdca-465">Hive</span></span></td>
      <td><span data-ttu-id="2cdca-466">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-466">Table</span></span></td>
      <td><span data-ttu-id="2cdca-467">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-467">Table, view</span></span></td>
      <td><span data-ttu-id="2cdca-468">
        <font size=2>Protokol: hive</span><span class="sxs-lookup"><span data-stu-id="2cdca-468">
        <font size=2> Protocol: hive</span></span> <br><span data-ttu-id="2cdca-469">Ověřování: {HDInsight, basic, uživatelské jméno, none}</span><span class="sxs-lookup"><span data-stu-id="2cdca-469">Authentication: {HDInsight, basic, username, none}</span></span> <br><span data-ttu-id="2cdca-470">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-470">Address:</span></span> <br><span data-ttu-id="2cdca-471">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-471">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-472">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-472">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-473">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="2cdca-473">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2cdca-474">connectionProperties:</span><span class="sxs-lookup"><span data-stu-id="2cdca-474">connectionProperties:</span></span> <br><span data-ttu-id="2cdca-475">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-475">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-476">HTTP</span><span class="sxs-lookup"><span data-stu-id="2cdca-476">HTTP</span></span></td>
      <td><span data-ttu-id="2cdca-477">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-477">Container</span></span></td>
      <td><span data-ttu-id="2cdca-478">Lokality</span><span class="sxs-lookup"><span data-stu-id="2cdca-478">Site</span></span></td>
      <td><span data-ttu-id="2cdca-479">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="2cdca-479">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2cdca-480">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-480">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2cdca-481">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-481">Address:</span></span> <br><span data-ttu-id="2cdca-482">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-482">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-483">HTTP</span><span class="sxs-lookup"><span data-stu-id="2cdca-483">HTTP</span></span></td>
      <td><span data-ttu-id="2cdca-484">Sestava</span><span class="sxs-lookup"><span data-stu-id="2cdca-484">Report</span></span></td>
      <td><span data-ttu-id="2cdca-485">Sestavy, řídicí panel</span><span class="sxs-lookup"><span data-stu-id="2cdca-485">Report, dashboard</span></span></td>
      <td><span data-ttu-id="2cdca-486">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="2cdca-486">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2cdca-487">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-487">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2cdca-488">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-488">Address:</span></span> <br><span data-ttu-id="2cdca-489">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-489">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-490">HTTP</span><span class="sxs-lookup"><span data-stu-id="2cdca-490">HTTP</span></span></td>
      <td><span data-ttu-id="2cdca-491">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-491">Table</span></span></td>
      <td><span data-ttu-id="2cdca-492">Koncový bod, soubor</span><span class="sxs-lookup"><span data-stu-id="2cdca-492">Endpoint, file</span></span></td>
      <td><span data-ttu-id="2cdca-493">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="2cdca-493">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2cdca-494">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-494">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2cdca-495">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-495">Address:</span></span> <br><span data-ttu-id="2cdca-496">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-496">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-497">MySQL</span><span class="sxs-lookup"><span data-stu-id="2cdca-497">MySQL</span></span></td>
      <td><span data-ttu-id="2cdca-498">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-498">Container</span></span></td>
      <td><span data-ttu-id="2cdca-499">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-499">Database</span></span></td>
      <td><span data-ttu-id="2cdca-500">
        <font size=2>Protokol: mysql</span><span class="sxs-lookup"><span data-stu-id="2cdca-500">
        <font size=2> Protocol: mysql</span></span> <br><span data-ttu-id="2cdca-501">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-501">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-502">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-502">Address:</span></span> <br><span data-ttu-id="2cdca-503">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-503">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-504">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-504">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-505">MySQL</span><span class="sxs-lookup"><span data-stu-id="2cdca-505">MySQL</span></span></td>
      <td><span data-ttu-id="2cdca-506">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-506">Table</span></span></td>
      <td><span data-ttu-id="2cdca-507">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-507">Table, view</span></span></td>
      <td><span data-ttu-id="2cdca-508">
        <font size=2>Protokol: mysql</span><span class="sxs-lookup"><span data-stu-id="2cdca-508">
        <font size=2> Protocol: mysql</span></span> <br><span data-ttu-id="2cdca-509">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-509">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-510">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-510">Address:</span></span> <br><span data-ttu-id="2cdca-511">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-511">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-512">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-512">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-513">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-513">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-514">OData</span><span class="sxs-lookup"><span data-stu-id="2cdca-514">OData</span></span></td>
      <td><span data-ttu-id="2cdca-515">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-515">Container</span></span></td>
      <td><span data-ttu-id="2cdca-516">Kontejneru entit</span><span class="sxs-lookup"><span data-stu-id="2cdca-516">Entity container</span></span></td>
      <td><span data-ttu-id="2cdca-517">
        <font size=2>Protokol: odata</span><span class="sxs-lookup"><span data-stu-id="2cdca-517">
        <font size=2> Protocol: odata</span></span> <br><span data-ttu-id="2cdca-518">Ověřování: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-518">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="2cdca-519">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-519">Address:</span></span> <br><span data-ttu-id="2cdca-520">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-520">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-521">OData</span><span class="sxs-lookup"><span data-stu-id="2cdca-521">OData</span></span></td>
      <td><span data-ttu-id="2cdca-522">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-522">Table</span></span></td>
      <td><span data-ttu-id="2cdca-523">Sady entit, – funkce</span><span class="sxs-lookup"><span data-stu-id="2cdca-523">Entity set, function</span></span></td>
      <td><span data-ttu-id="2cdca-524">
        <font size=2>Protokol: odata</span><span class="sxs-lookup"><span data-stu-id="2cdca-524">
        <font size=2> Protocol: odata</span></span> <br><span data-ttu-id="2cdca-525">Ověřování: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-525">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="2cdca-526">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-526">Address:</span></span> <br><span data-ttu-id="2cdca-527">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</span><span class="sxs-lookup"><span data-stu-id="2cdca-527">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="2cdca-528">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;prostředek</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-528">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; resource </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-529">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="2cdca-529">Oracle Database</span></span></td>
      <td><span data-ttu-id="2cdca-530">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-530">Container</span></span></td>
      <td><span data-ttu-id="2cdca-531">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-531">Database</span></span></td>
      <td><span data-ttu-id="2cdca-532">
        <font size=2>Protokol: oracle</span><span class="sxs-lookup"><span data-stu-id="2cdca-532">
        <font size=2> Protocol: oracle</span></span> <br><span data-ttu-id="2cdca-533">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-533">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-534">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-534">Address:</span></span> <br><span data-ttu-id="2cdca-535">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-535">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-536">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-536">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-537">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="2cdca-537">Oracle Database</span></span></td>
      <td><span data-ttu-id="2cdca-538">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-538">Table</span></span></td>
      <td><span data-ttu-id="2cdca-539">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-539">Table, view</span></span></td>
      <td><span data-ttu-id="2cdca-540">
        <font size=2>Protokol: oracle</span><span class="sxs-lookup"><span data-stu-id="2cdca-540">
        <font size=2> Protocol: oracle</span></span> <br><span data-ttu-id="2cdca-541">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-541">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-542">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-542">Address:</span></span> <br><span data-ttu-id="2cdca-543">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-543">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-544">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-544">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-545">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="2cdca-545">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2cdca-546">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-546">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-547">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="2cdca-547">PostgreSQL</span></span></td>
      <td><span data-ttu-id="2cdca-548">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-548">Container</span></span></td>
      <td><span data-ttu-id="2cdca-549">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-549">Database</span></span></td>
      <td><span data-ttu-id="2cdca-550">
        <font size=2>Protokol: postgresql</span><span class="sxs-lookup"><span data-stu-id="2cdca-550">
        <font size=2> Protocol: postgresql</span></span> <br><span data-ttu-id="2cdca-551">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-551">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-552">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-552">Address:</span></span> <br><span data-ttu-id="2cdca-553">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-553">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-554">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-554">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-555">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="2cdca-555">PostgreSQL</span></span></td>
      <td><span data-ttu-id="2cdca-556">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-556">Table</span></span></td>
      <td><span data-ttu-id="2cdca-557">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-557">Table, view</span></span></td>
      <td><span data-ttu-id="2cdca-558">
        <font size=2>Protokol: postgresql</span><span class="sxs-lookup"><span data-stu-id="2cdca-558">
        <font size=2> Protocol: postgresql</span></span> <br><span data-ttu-id="2cdca-559">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-559">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-560">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-560">Address:</span></span> <br><span data-ttu-id="2cdca-561">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-561">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-562">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-562">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-563">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="2cdca-563">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2cdca-564">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-564">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-565">Power BI</span><span class="sxs-lookup"><span data-stu-id="2cdca-565">Power BI</span></span></td>
      <td><span data-ttu-id="2cdca-566">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-566">Container</span></span></td>
      <td><span data-ttu-id="2cdca-567">Lokality</span><span class="sxs-lookup"><span data-stu-id="2cdca-567">Site</span></span></td>
      <td><span data-ttu-id="2cdca-568">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="2cdca-568">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2cdca-569">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-569">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2cdca-570">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-570">Address:</span></span> <br><span data-ttu-id="2cdca-571">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-571">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-572">Power BI</span><span class="sxs-lookup"><span data-stu-id="2cdca-572">Power BI</span></span></td>
      <td><span data-ttu-id="2cdca-573">Sestava</span><span class="sxs-lookup"><span data-stu-id="2cdca-573">Report</span></span></td>
      <td><span data-ttu-id="2cdca-574">Sestavy, řídicí panel</span><span class="sxs-lookup"><span data-stu-id="2cdca-574">Report, dashboard</span></span></td>
      <td><span data-ttu-id="2cdca-575">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="2cdca-575">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2cdca-576">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-576">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2cdca-577">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-577">Address:</span></span> <br><span data-ttu-id="2cdca-578">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-578">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-579">Power Query</span><span class="sxs-lookup"><span data-stu-id="2cdca-579">Power Query</span></span></td>
      <td><span data-ttu-id="2cdca-580">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-580">Table</span></span></td>
      <td><span data-ttu-id="2cdca-581">Data hybridní webové aplikace</span><span class="sxs-lookup"><span data-stu-id="2cdca-581">Data mashup</span></span></td>
      <td><span data-ttu-id="2cdca-582">
        <font size=2>Protokol: power dotazu</span><span class="sxs-lookup"><span data-stu-id="2cdca-582">
        <font size=2> Protocol: power-query</span></span> <br><span data-ttu-id="2cdca-583">Ověřování: {oauth}</span><span class="sxs-lookup"><span data-stu-id="2cdca-583">Authentication: {oauth}</span></span> <br><span data-ttu-id="2cdca-584">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-584">Address:</span></span> <br><span data-ttu-id="2cdca-585">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-585">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-586">Salesforce</span><span class="sxs-lookup"><span data-stu-id="2cdca-586">Salesforce</span></span></td>
      <td><span data-ttu-id="2cdca-587">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-587">Table</span></span></td>
      <td><span data-ttu-id="2cdca-588">Objekt</span><span class="sxs-lookup"><span data-stu-id="2cdca-588">Object</span></span></td>
      <td><span data-ttu-id="2cdca-589">
        <font size=2>Protokol: salesforce-com</span><span class="sxs-lookup"><span data-stu-id="2cdca-589">
        <font size=2> Protocol: salesforce-com</span></span> <br><span data-ttu-id="2cdca-590">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-590">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-591">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-591">Address:</span></span> <br><span data-ttu-id="2cdca-592">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer</span><span class="sxs-lookup"><span data-stu-id="2cdca-592">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; loginServer</span></span> <br><span data-ttu-id="2cdca-593">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;– Třída</span><span class="sxs-lookup"><span data-stu-id="2cdca-593">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class</span></span> <br><span data-ttu-id="2cdca-594">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Název položky</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-594">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; itemName </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-595">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="2cdca-595">SAP HANA</span></span></td>
      <td><span data-ttu-id="2cdca-596">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-596">Container</span></span></td>
      <td><span data-ttu-id="2cdca-597">Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-597">Server</span></span></td>
      <td><span data-ttu-id="2cdca-598">
        <font size=2>Protokol: sap hana-sql</span><span class="sxs-lookup"><span data-stu-id="2cdca-598">
        <font size=2> Protocol: sap-hana-sql</span></span> <br><span data-ttu-id="2cdca-599">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-599">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-600">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-600">Address:</span></span> <br><span data-ttu-id="2cdca-601">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-601">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-602">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="2cdca-602">SAP HANA</span></span></td>
      <td><span data-ttu-id="2cdca-603">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-603">Table</span></span></td>
      <td><span data-ttu-id="2cdca-604">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="2cdca-604">View</span></span></td>
      <td><span data-ttu-id="2cdca-605">
        <font size=2>Protokol: sap hana-sql</span><span class="sxs-lookup"><span data-stu-id="2cdca-605">
        <font size=2> Protocol: sap-hana-sql</span></span> <br><span data-ttu-id="2cdca-606">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-606">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-607">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-607">Address:</span></span> <br><span data-ttu-id="2cdca-608">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-608">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-609">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="2cdca-609">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2cdca-610">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-610">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-611">SharePoint</span><span class="sxs-lookup"><span data-stu-id="2cdca-611">SharePoint</span></span></td>
      <td><span data-ttu-id="2cdca-612">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-612">Table</span></span></td>
      <td><span data-ttu-id="2cdca-613">Seznam</span><span class="sxs-lookup"><span data-stu-id="2cdca-613">List</span></span></td>
      <td><span data-ttu-id="2cdca-614">
        <font size=2>Protokolů: sharepoint – seznam</span><span class="sxs-lookup"><span data-stu-id="2cdca-614">
        <font size=2> Protocol: sharepoint-list</span></span> <br><span data-ttu-id="2cdca-615">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-615">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-616">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-616">Address:</span></span> <br><span data-ttu-id="2cdca-617">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-617">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-618">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2cdca-618">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="2cdca-619">Příkaz</span><span class="sxs-lookup"><span data-stu-id="2cdca-619">Command</span></span></td>
      <td><span data-ttu-id="2cdca-620">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="2cdca-620">Stored procedure</span></span></td>
      <td><span data-ttu-id="2cdca-621">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="2cdca-621">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2cdca-622">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-622">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-623">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-623">Address:</span></span> <br><span data-ttu-id="2cdca-624">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-624">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-625">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-625">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-626">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="2cdca-626">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2cdca-627">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-627">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-628">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2cdca-628">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="2cdca-629">TableValuedFunction</span><span class="sxs-lookup"><span data-stu-id="2cdca-629">TableValuedFunction</span></span></td>
      <td><span data-ttu-id="2cdca-630">Funkce vracející tabulku</span><span class="sxs-lookup"><span data-stu-id="2cdca-630">Table-valued function</span></span></td>
      <td><span data-ttu-id="2cdca-631">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="2cdca-631">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2cdca-632">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-632">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-633">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-633">Address:</span></span> <br><span data-ttu-id="2cdca-634">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-634">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-635">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-635">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-636">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="2cdca-636">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2cdca-637">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-637">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-638">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2cdca-638">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="2cdca-639">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-639">Container</span></span></td>
      <td><span data-ttu-id="2cdca-640">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-640">Database</span></span></td>
      <td><span data-ttu-id="2cdca-641">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="2cdca-641">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2cdca-642">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-642">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-643">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-643">Address:</span></span> <br><span data-ttu-id="2cdca-644">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-644">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-645">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-645">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-646">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2cdca-646">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="2cdca-647">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-647">Table</span></span></td>
      <td><span data-ttu-id="2cdca-648">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-648">Table, view</span></span></td>
      <td><span data-ttu-id="2cdca-649">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="2cdca-649">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2cdca-650">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-650">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-651">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-651">Address:</span></span> <br><span data-ttu-id="2cdca-652">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-652">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-653">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-653">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-654">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="2cdca-654">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2cdca-655">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-655">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-656">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-656">SQL Server</span></span></td>
      <td><span data-ttu-id="2cdca-657">Příkaz</span><span class="sxs-lookup"><span data-stu-id="2cdca-657">Command</span></span></td>
      <td><span data-ttu-id="2cdca-658">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="2cdca-658">Stored procedure</span></span></td>
      <td><span data-ttu-id="2cdca-659">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="2cdca-659">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2cdca-660">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-660">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-661">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-661">Address:</span></span> <br><span data-ttu-id="2cdca-662">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-662">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-663">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-663">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-664">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="2cdca-664">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2cdca-665">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-665">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-666">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-666">SQL Server</span></span></td>
      <td><span data-ttu-id="2cdca-667">TableValuedFunction</span><span class="sxs-lookup"><span data-stu-id="2cdca-667">TableValuedFunction</span></span></td>
      <td><span data-ttu-id="2cdca-668">Funkce vracející tabulku</span><span class="sxs-lookup"><span data-stu-id="2cdca-668">Table-valued function</span></span></td>
      <td><span data-ttu-id="2cdca-669">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="2cdca-669">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2cdca-670">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-670">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-671">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-671">Address:</span></span> <br><span data-ttu-id="2cdca-672">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-672">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-673">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-673">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-674">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="2cdca-674">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2cdca-675">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-675">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-676">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-676">SQL Server</span></span></td>
      <td><span data-ttu-id="2cdca-677">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-677">Container</span></span></td>
      <td><span data-ttu-id="2cdca-678">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-678">Database</span></span></td>
      <td><span data-ttu-id="2cdca-679">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="2cdca-679">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2cdca-680">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-680">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-681">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-681">Address:</span></span> <br><span data-ttu-id="2cdca-682">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-682">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-683">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-683">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-684">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-684">SQL Server</span></span></td>
      <td><span data-ttu-id="2cdca-685">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-685">Table</span></span></td>
      <td><span data-ttu-id="2cdca-686">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-686">Table, view</span></span></td>
      <td><span data-ttu-id="2cdca-687">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="2cdca-687">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2cdca-688">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-688">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-689">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-689">Address:</span></span> <br><span data-ttu-id="2cdca-690">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-690">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-691">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-691">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-692">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="2cdca-692">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2cdca-693">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-693">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-694">SQL Server Analysis Services multidimensional</span><span class="sxs-lookup"><span data-stu-id="2cdca-694">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="2cdca-695">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-695">Container</span></span></td>
      <td><span data-ttu-id="2cdca-696">Model</span><span class="sxs-lookup"><span data-stu-id="2cdca-696">Model</span></span></td>
      <td><span data-ttu-id="2cdca-697">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="2cdca-697">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2cdca-698">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="2cdca-698">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2cdca-699">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-699">Address:</span></span> <br><span data-ttu-id="2cdca-700">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-700">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-701">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-701">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-702">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-702">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-703">SQL Server Analysis Services multidimensional</span><span class="sxs-lookup"><span data-stu-id="2cdca-703">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="2cdca-704">KLÍČOVÝ UKAZATEL VÝKONU</span><span class="sxs-lookup"><span data-stu-id="2cdca-704">KPI</span></span></td>
      <td><span data-ttu-id="2cdca-705">KLÍČOVÝ UKAZATEL VÝKONU</span><span class="sxs-lookup"><span data-stu-id="2cdca-705">KPI</span></span></td>
      <td><span data-ttu-id="2cdca-706">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="2cdca-706">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2cdca-707">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="2cdca-707">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2cdca-708">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-708">Address:</span></span> <br><span data-ttu-id="2cdca-709">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-709">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-710">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-710">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-711">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="2cdca-711">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2cdca-712">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="2cdca-712">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2cdca-713">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {klíčový ukazatel výkonu}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-713">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-714">SQL Server Analysis Services multidimensional</span><span class="sxs-lookup"><span data-stu-id="2cdca-714">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="2cdca-715">Míra</span><span class="sxs-lookup"><span data-stu-id="2cdca-715">Measure</span></span></td>
      <td><span data-ttu-id="2cdca-716">Míra</span><span class="sxs-lookup"><span data-stu-id="2cdca-716">Measure</span></span></td>
      <td><span data-ttu-id="2cdca-717">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="2cdca-717">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2cdca-718">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="2cdca-718">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2cdca-719">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-719">Address:</span></span> <br><span data-ttu-id="2cdca-720">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-720">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-721">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-721">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-722">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="2cdca-722">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2cdca-723">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="2cdca-723">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2cdca-724">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-724">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-725">SQL Server Analysis Services multidimensional</span><span class="sxs-lookup"><span data-stu-id="2cdca-725">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="2cdca-726">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-726">Table</span></span></td>
      <td><span data-ttu-id="2cdca-727">Dimenze</span><span class="sxs-lookup"><span data-stu-id="2cdca-727">Dimension</span></span></td>
      <td><span data-ttu-id="2cdca-728">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="2cdca-728">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2cdca-729">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="2cdca-729">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2cdca-730">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-730">Address:</span></span> <br><span data-ttu-id="2cdca-731">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-731">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-732">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-732">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-733">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="2cdca-733">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2cdca-734">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="2cdca-734">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2cdca-735">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {{dimenze}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-735">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Dimension} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-736">SQL Server Analysis Services tabulkové</span><span class="sxs-lookup"><span data-stu-id="2cdca-736">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="2cdca-737">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-737">Container</span></span></td>
      <td><span data-ttu-id="2cdca-738">Model</span><span class="sxs-lookup"><span data-stu-id="2cdca-738">Model</span></span></td>
      <td><span data-ttu-id="2cdca-739">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="2cdca-739">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2cdca-740">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="2cdca-740">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2cdca-741">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-741">Address:</span></span> <br><span data-ttu-id="2cdca-742">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-742">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-743">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-743">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-744">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-744">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-745">SQL Server Analysis Services tabulkové</span><span class="sxs-lookup"><span data-stu-id="2cdca-745">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="2cdca-746">KLÍČOVÝ UKAZATEL VÝKONU</span><span class="sxs-lookup"><span data-stu-id="2cdca-746">KPI</span></span></td>
      <td><span data-ttu-id="2cdca-747">KLÍČOVÝ UKAZATEL VÝKONU</span><span class="sxs-lookup"><span data-stu-id="2cdca-747">KPI</span></span></td>
      <td><span data-ttu-id="2cdca-748">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="2cdca-748">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2cdca-749">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="2cdca-749">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2cdca-750">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-750">Address:</span></span> <br><span data-ttu-id="2cdca-751">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-751">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-752">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-752">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-753">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="2cdca-753">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2cdca-754">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="2cdca-754">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2cdca-755">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {klíčový ukazatel výkonu}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-755">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-756">SQL Server Analysis Services tabulkové</span><span class="sxs-lookup"><span data-stu-id="2cdca-756">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="2cdca-757">Míra</span><span class="sxs-lookup"><span data-stu-id="2cdca-757">Measure</span></span></td>
      <td><span data-ttu-id="2cdca-758">Míra</span><span class="sxs-lookup"><span data-stu-id="2cdca-758">Measure</span></span></td>
      <td><span data-ttu-id="2cdca-759">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="2cdca-759">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2cdca-760">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="2cdca-760">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2cdca-761">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-761">Address:</span></span> <br><span data-ttu-id="2cdca-762">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-762">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-763">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-763">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-764">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="2cdca-764">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2cdca-765">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="2cdca-765">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2cdca-766">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-766">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-767">SQL Server Analysis Services tabulkové</span><span class="sxs-lookup"><span data-stu-id="2cdca-767">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="2cdca-768">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-768">Table</span></span></td>
      <td><span data-ttu-id="2cdca-769">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-769">Table</span></span></td>
      <td><span data-ttu-id="2cdca-770">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="2cdca-770">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2cdca-771">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="2cdca-771">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2cdca-772">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-772">Address:</span></span> <br><span data-ttu-id="2cdca-773">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-773">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-774">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-774">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-775">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="2cdca-775">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2cdca-776">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="2cdca-776">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2cdca-777">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Table}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-777">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Table} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-778">SQL Server Reporting Services</span><span class="sxs-lookup"><span data-stu-id="2cdca-778">SQL Server Reporting Services</span></span></td>
      <td><span data-ttu-id="2cdca-779">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-779">Container</span></span></td>
      <td><span data-ttu-id="2cdca-780">Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-780">Server</span></span></td>
      <td><span data-ttu-id="2cdca-781">
        <font size=2>Protokol: reporting services</span><span class="sxs-lookup"><span data-stu-id="2cdca-781">
        <font size=2> Protocol: reporting-services</span></span> <br><span data-ttu-id="2cdca-782">Ověřování: {windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-782">Authentication: {windows}</span></span> <br><span data-ttu-id="2cdca-783">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-783">Address:</span></span> <br><span data-ttu-id="2cdca-784">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-784">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-785">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze: {ReportingService2010}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-785">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-786">SQL Server Reporting Services</span><span class="sxs-lookup"><span data-stu-id="2cdca-786">SQL Server Reporting Services</span></span></td>
      <td><span data-ttu-id="2cdca-787">Sestava</span><span class="sxs-lookup"><span data-stu-id="2cdca-787">Report</span></span></td>
      <td><span data-ttu-id="2cdca-788">Sestava</span><span class="sxs-lookup"><span data-stu-id="2cdca-788">Report</span></span></td>
      <td><span data-ttu-id="2cdca-789">
        <font size=2>Protokol: reporting services</span><span class="sxs-lookup"><span data-stu-id="2cdca-789">
        <font size=2> Protocol: reporting-services</span></span> <br><span data-ttu-id="2cdca-790">Ověřování: {windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-790">Authentication: {windows}</span></span> <br><span data-ttu-id="2cdca-791">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-791">Address:</span></span> <br><span data-ttu-id="2cdca-792">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-792">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-793">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cesta</span><span class="sxs-lookup"><span data-stu-id="2cdca-793">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path</span></span> <br><span data-ttu-id="2cdca-794">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze: {ReportingService2010}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-794">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-795">Teradata</span><span class="sxs-lookup"><span data-stu-id="2cdca-795">Teradata</span></span></td>
      <td><span data-ttu-id="2cdca-796">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-796">Container</span></span></td>
      <td><span data-ttu-id="2cdca-797">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-797">Database</span></span></td>
      <td><span data-ttu-id="2cdca-798">
        <font size=2>Protokol: teradata</span><span class="sxs-lookup"><span data-stu-id="2cdca-798">
        <font size=2> Protocol: teradata</span></span> <br><span data-ttu-id="2cdca-799">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-799">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-800">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-800">Address:</span></span> <br><span data-ttu-id="2cdca-801">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-801">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-802">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-802">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-803">Teradata</span><span class="sxs-lookup"><span data-stu-id="2cdca-803">Teradata</span></span></td>
      <td><span data-ttu-id="2cdca-804">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-804">Table</span></span></td>
      <td><span data-ttu-id="2cdca-805">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-805">Table, view</span></span></td>
      <td><span data-ttu-id="2cdca-806">
        <font size=2>Protokol: teradata</span><span class="sxs-lookup"><span data-stu-id="2cdca-806">
        <font size=2> Protocol: teradata</span></span> <br><span data-ttu-id="2cdca-807">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-807">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2cdca-808">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-808">Address:</span></span> <br><span data-ttu-id="2cdca-809">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-809">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-810">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-810">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-811">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-811">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-812">SQL Server Master Data Services</span><span class="sxs-lookup"><span data-stu-id="2cdca-812">SQL Server Master Data Services</span></span></td>
      <td><span data-ttu-id="2cdca-813">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-813">Container</span></span></td>
      <td><span data-ttu-id="2cdca-814">Model</span><span class="sxs-lookup"><span data-stu-id="2cdca-814">Model</span></span></td>
      <td><span data-ttu-id="2cdca-815">
        <font size="2">Protokol: mssql-mds</span><span class="sxs-lookup"><span data-stu-id="2cdca-815">
        <font size="2"> Protocol: mssql-mds</span></span> <br><span data-ttu-id="2cdca-816">Ověřování: {windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-816">Authentication: {windows}</span></span> <br><span data-ttu-id="2cdca-817">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-817">Address:</span></span> <br><span data-ttu-id="2cdca-818">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</span><span class="sxs-lookup"><span data-stu-id="2cdca-818">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="2cdca-819">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="2cdca-819">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2cdca-820">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-820">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-821">SQL Server Master Data Services</span><span class="sxs-lookup"><span data-stu-id="2cdca-821">SQL Server Master Data Services</span></span></td>
      <td><span data-ttu-id="2cdca-822">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-822">Table</span></span></td>
      <td><span data-ttu-id="2cdca-823">Entita</span><span class="sxs-lookup"><span data-stu-id="2cdca-823">Entity</span></span></td>
      <td><span data-ttu-id="2cdca-824">
        <font size="2">Protokol: mssql-mds</span><span class="sxs-lookup"><span data-stu-id="2cdca-824">
        <font size="2"> Protocol: mssql-mds</span></span> <br><span data-ttu-id="2cdca-825">Ověřování: {windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-825">Authentication: {windows}</span></span> <br><span data-ttu-id="2cdca-826">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-826">Address:</span></span> <br><span data-ttu-id="2cdca-827">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</span><span class="sxs-lookup"><span data-stu-id="2cdca-827">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="2cdca-828">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="2cdca-828">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2cdca-829">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze</span><span class="sxs-lookup"><span data-stu-id="2cdca-829">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version</span></span> <br><span data-ttu-id="2cdca-830">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;entity</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-830">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; entity </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-831">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2cdca-831">Azure Cosmos DB</span></span></td>
      <td><span data-ttu-id="2cdca-832">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-832">Container</span></span></td>
      <td><span data-ttu-id="2cdca-833">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-833">Database</span></span></td>
      <td><span data-ttu-id="2cdca-834">
        <font size=2>Protokol: dokument db</span><span class="sxs-lookup"><span data-stu-id="2cdca-834">
        <font size=2> Protocol: document-db</span></span> <br><span data-ttu-id="2cdca-835">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="2cdca-835">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2cdca-836">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-836">Address:</span></span> <br><span data-ttu-id="2cdca-837">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</span><span class="sxs-lookup"><span data-stu-id="2cdca-837">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="2cdca-838">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-838">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-839">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2cdca-839">Azure Cosmos DB</span></span></td>
      <td><span data-ttu-id="2cdca-840">Kolekce</span><span class="sxs-lookup"><span data-stu-id="2cdca-840">Collection</span></span></td>
      <td><span data-ttu-id="2cdca-841">Kolekce</span><span class="sxs-lookup"><span data-stu-id="2cdca-841">Collection</span></span></td>
      <td><span data-ttu-id="2cdca-842">
        <font size=2>Protokol: dokument db</span><span class="sxs-lookup"><span data-stu-id="2cdca-842">
        <font size=2> Protocol: document-db</span></span> <br><span data-ttu-id="2cdca-843">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="2cdca-843">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2cdca-844">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-844">Address:</span></span> <br><span data-ttu-id="2cdca-845">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</span><span class="sxs-lookup"><span data-stu-id="2cdca-845">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="2cdca-846">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-846">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-847">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kolekce</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-847">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; collection </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-848">Obecná rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="2cdca-848">Generic ODBC</span></span></td>
      <td><span data-ttu-id="2cdca-849">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-849">Container</span></span></td>
      <td><span data-ttu-id="2cdca-850">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-850">Database</span></span></td>
      <td><span data-ttu-id="2cdca-851">
        <font size=2>Protokol: rozhraní odbc</span><span class="sxs-lookup"><span data-stu-id="2cdca-851">
        <font size=2> Protocol: odbc</span></span> <br><span data-ttu-id="2cdca-852">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-852">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-853">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-853">Address:</span></span> <br><span data-ttu-id="2cdca-854">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Možnosti</span><span class="sxs-lookup"><span data-stu-id="2cdca-854">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options</span></span> <br><span data-ttu-id="2cdca-855">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-855">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-856">Obecná rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="2cdca-856">Generic ODBC</span></span></td>
      <td><span data-ttu-id="2cdca-857">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-857">Table</span></span></td>
      <td><span data-ttu-id="2cdca-858">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-858">Table, View</span></span></td>
      <td><span data-ttu-id="2cdca-859">
        <font size=2>Protokol: rozhraní odbc</span><span class="sxs-lookup"><span data-stu-id="2cdca-859">
        <font size=2> Protocol: odbc</span></span> <br><span data-ttu-id="2cdca-860">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-860">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-861">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-861">Address:</span></span> <br><span data-ttu-id="2cdca-862">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Možnosti</span><span class="sxs-lookup"><span data-stu-id="2cdca-862">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options</span></span> <br><span data-ttu-id="2cdca-863">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-863">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-864">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="2cdca-864">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2cdca-865">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-865">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-866">Sybase</span><span class="sxs-lookup"><span data-stu-id="2cdca-866">Sybase</span></span></td>
      <td><span data-ttu-id="2cdca-867">Kontejner</span><span class="sxs-lookup"><span data-stu-id="2cdca-867">Container</span></span></td>
      <td><span data-ttu-id="2cdca-868">Databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-868">Database</span></span></td>
      <td><span data-ttu-id="2cdca-869">
        <font size=2>protokol: sybase</span><span class="sxs-lookup"><span data-stu-id="2cdca-869">
        <font size=2> protocol: sybase</span></span> <br><span data-ttu-id="2cdca-870">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-870">authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-871">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-871">address:</span></span> <br><span data-ttu-id="2cdca-872">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-872">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-873">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-873">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-874">Sybase</span><span class="sxs-lookup"><span data-stu-id="2cdca-874">Sybase</span></span></td>
      <td><span data-ttu-id="2cdca-875">Table</span><span class="sxs-lookup"><span data-stu-id="2cdca-875">Table</span></span></td>
      <td><span data-ttu-id="2cdca-876">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="2cdca-876">Table, View</span></span></td>
      <td><span data-ttu-id="2cdca-877">
        <font size=2>protokol: sybase</span><span class="sxs-lookup"><span data-stu-id="2cdca-877">
        <font size=2> protocol: sybase</span></span> <br><span data-ttu-id="2cdca-878">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="2cdca-878">authentication: {basic, windows}</span></span> <br><span data-ttu-id="2cdca-879">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-879">address:</span></span> <br><span data-ttu-id="2cdca-880">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="2cdca-880">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2cdca-881">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="2cdca-881">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2cdca-882">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="2cdca-882">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2cdca-883">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-883">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2cdca-884">Jiné (žádná z výše uvedených)</span><span class="sxs-lookup"><span data-stu-id="2cdca-884">Other (none of the above)</span></span></td>
      <td>\*</td>
      <td>\*</td>
      <td><span data-ttu-id="2cdca-885">
        <font size=2>Protokol: Obecné – prostředek</span><span class="sxs-lookup"><span data-stu-id="2cdca-885">
        <font size=2> Protocol: generic-asset</span></span> <br><span data-ttu-id="2cdca-886">Adresa:</span><span class="sxs-lookup"><span data-stu-id="2cdca-886">Address:</span></span> <br><span data-ttu-id="2cdca-887">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID</font>
      </span><span class="sxs-lookup"><span data-stu-id="2cdca-887">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; assetId </font>
      </span></span></td>
    </tr>
</table>
