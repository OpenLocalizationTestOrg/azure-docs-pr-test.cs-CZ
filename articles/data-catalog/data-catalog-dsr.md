---
title: aaaSupported zdroje dat v Azure Data Catalog | Microsoft Docs
description: "V tomto článku jsou uvedeny specifikace zdrojů dat aktuálně podporované hello."
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
ms.openlocfilehash: 4bfcabf31bf9fd781c939a5026abc42a5407df32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="supported-data-sources-in-azure-data-catalog"></a><span data-ttu-id="01eb8-103">Podporované zdroje dat v Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="01eb8-103">Supported data sources in Azure Data Catalog</span></span>

<span data-ttu-id="01eb8-104">Metadata můžete publikovat pomocí veřejné rozhraní API nebo kliknutí-jednou nástroj pro registraci, nebo ručním zadáním informace přímo toohello Azure Data Catalog webový portál.</span><span class="sxs-lookup"><span data-stu-id="01eb8-104">You can publish metadata by using a public API or a click-once registration tool, or by manually entering information directly toohello Azure Data Catalog web portal.</span></span> <span data-ttu-id="01eb8-105">Hello následující tabulka shrnuje všechny zdroje dat, které jsou podporovány katalogem hello dnes a hello možnosti publikování pro každou.</span><span class="sxs-lookup"><span data-stu-id="01eb8-105">hello following table summarizes all data sources that are supported by hello catalog today, and hello publishing capabilities for each.</span></span> <span data-ttu-id="01eb8-106">Jsou také uvedeny hello externích dat nástroje, které všechny zdroje dat, můžete spustit z našich zkušeností portál "open in".</span><span class="sxs-lookup"><span data-stu-id="01eb8-106">Also listed are hello external data tools that each data source can launch from our portal "open-in" experience.</span></span> <span data-ttu-id="01eb8-107">Druhá tabulka Hello obsahuje specifikaci odbornější každé připojení vlastnosti zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="01eb8-107">hello second table contains a more technical specification of each data-source connection property.</span></span>


## <a name="list-of-supported-data-sources"></a><span data-ttu-id="01eb8-108">Seznam podporovaných zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="01eb8-108">List of supported data sources</span></span>

<table>
    <tr>
       <td><span data-ttu-id="01eb8-109"><b>Objekt zdroje dat</b></span><span class="sxs-lookup"><span data-stu-id="01eb8-109"><b>Data source object</b></span></span></td>
       <td><span data-ttu-id="01eb8-110"><b>Rozhraní API</b></span><span class="sxs-lookup"><span data-stu-id="01eb8-110"><b>API</b></span></span></td>
       <td><span data-ttu-id="01eb8-111"><b>Ruční zadání</b></span><span class="sxs-lookup"><span data-stu-id="01eb8-111"><b>Manual entry</b></span></span></td>
       <td><span data-ttu-id="01eb8-112"><b>Nástroj pro registraci</b></span><span class="sxs-lookup"><span data-stu-id="01eb8-112"><b>Registration tool</b></span></span></td>
       <td><span data-ttu-id="01eb8-113"><b>Nástroje pro Open in</b></span><span class="sxs-lookup"><span data-stu-id="01eb8-113"><b>Open-in tools</b></span></span></td>
       <td><span data-ttu-id="01eb8-114"><b>Poznámky k</b></span><span class="sxs-lookup"><span data-stu-id="01eb8-114"><b>Notes</b></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-115">Adresář Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="01eb8-115">Azure Data Lake Store directory</span></span></td>
      <td><span data-ttu-id="01eb8-116">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-116">✓</span></span></td>
      <td><span data-ttu-id="01eb8-117">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-117">✓</span></span></td>
      <td><span data-ttu-id="01eb8-118">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-118">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-119">Soubor Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="01eb8-119">Azure Data Lake Store file</span></span></td>
      <td><span data-ttu-id="01eb8-120">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-120">✓</span></span></td>
      <td><span data-ttu-id="01eb8-121">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-121">✓</span></span></td>
      <td><span data-ttu-id="01eb8-122">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-122">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-123">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="01eb8-123">Azure Blob storage</span></span></td>
      <td><span data-ttu-id="01eb8-124">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-124">✓</span></span></td>
      <td><span data-ttu-id="01eb8-125">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-125">✓</span></span></td>
      <td><span data-ttu-id="01eb8-126">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-126">✓</span></span></td>
      <td><span data-ttu-id="01eb8-127"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-127"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-128">Adresář úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="01eb8-128">Azure Storage directory</span></span></td>
      <td><span data-ttu-id="01eb8-129">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-129">✓</span></span></td>
      <td><span data-ttu-id="01eb8-130">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-130">✓</span></span></td>
      <td><span data-ttu-id="01eb8-131">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-131">✓</span></span></td>
      <td><span data-ttu-id="01eb8-132"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-132"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-133">Tabulky Azure Storage</span><span class="sxs-lookup"><span data-stu-id="01eb8-133">Azure Storage table</span></span></td>
      <td><span data-ttu-id="01eb8-134">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-134">✓</span></span></td>
      <td><span data-ttu-id="01eb8-135">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-135">✓</span></span></td>
      <td><span data-ttu-id="01eb8-136">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-136">✓</span></span></td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-137">HDFS adresáře</span><span class="sxs-lookup"><span data-stu-id="01eb8-137">HDFS directory</span></span></td>
      <td><span data-ttu-id="01eb8-138">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-138">✓</span></span></td>
      <td><span data-ttu-id="01eb8-139">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-139">✓</span></span></td>
      <td><span data-ttu-id="01eb8-140">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-140">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-141">HDFS souboru</span><span class="sxs-lookup"><span data-stu-id="01eb8-141">HDFS file</span></span></td>
      <td><span data-ttu-id="01eb8-142">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-142">✓</span></span></td>
      <td><span data-ttu-id="01eb8-143">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-143">✓</span></span></td>
      <td><span data-ttu-id="01eb8-144">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-144">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-145">Tabulku Hive</span><span class="sxs-lookup"><span data-stu-id="01eb8-145">Hive table</span></span></td>
      <td><span data-ttu-id="01eb8-146">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-146">✓</span></span></td>
      <td><span data-ttu-id="01eb8-147">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-147">✓</span></span></td>
      <td><span data-ttu-id="01eb8-148">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-148">✓</span></span></td>
      <td><span data-ttu-id="01eb8-149"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-149"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-150">Zobrazení Hive</span><span class="sxs-lookup"><span data-stu-id="01eb8-150">Hive view</span></span></td>
      <td><span data-ttu-id="01eb8-151">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-151">✓</span></span></td>
      <td><span data-ttu-id="01eb8-152">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-152">✓</span></span></td>
      <td><span data-ttu-id="01eb8-153">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-153">✓</span></span></td>
      <td><span data-ttu-id="01eb8-154"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-154"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-155">Tabulka MySQL</span><span class="sxs-lookup"><span data-stu-id="01eb8-155">MySQL table</span></span></td>
      <td><span data-ttu-id="01eb8-156">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-156">✓</span></span></td>
      <td><span data-ttu-id="01eb8-157">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-157">✓</span></span></td>
      <td><span data-ttu-id="01eb8-158">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-158">✓</span></span></td>
      <td><span data-ttu-id="01eb8-159"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-159"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-160">Zobrazení MySQL</span><span class="sxs-lookup"><span data-stu-id="01eb8-160">MySQL view</span></span></td>
      <td><span data-ttu-id="01eb8-161">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-161">✓</span></span></td>
      <td><span data-ttu-id="01eb8-162">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-162">✓</span></span></td>
      <td><span data-ttu-id="01eb8-163">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-163">✓</span></span></td>
      <td><span data-ttu-id="01eb8-164"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-164"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-165">Tabulka databáze Oracle</span><span class="sxs-lookup"><span data-stu-id="01eb8-165">Oracle Database table</span></span></td>
      <td><span data-ttu-id="01eb8-166">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-166">✓</span></span></td>
      <td><span data-ttu-id="01eb8-167">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-167">✓</span></span></td>
      <td><span data-ttu-id="01eb8-168">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-168">✓</span></span></td>
      <td><span data-ttu-id="01eb8-169"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-169"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-170">Zobrazení databáze Oracle</span><span class="sxs-lookup"><span data-stu-id="01eb8-170">Oracle Database view</span></span></td>
      <td><span data-ttu-id="01eb8-171">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-171">✓</span></span></td>
      <td><span data-ttu-id="01eb8-172">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-172">✓</span></span></td>
      <td><span data-ttu-id="01eb8-173">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-173">✓</span></span></td>
      <td><span data-ttu-id="01eb8-174"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-174"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-175">Jiné (Obecné asset)</span><span class="sxs-lookup"><span data-stu-id="01eb8-175">Other (generic asset)</span></span></td>
      <td><span data-ttu-id="01eb8-176">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-176">✓</span></span></td>
      <td><span data-ttu-id="01eb8-177">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-177">✓</span></span></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-178">Tabulky Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="01eb8-178">Azure SQL Data Warehouse table</span></span></td>
      <td><span data-ttu-id="01eb8-179">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-179">✓</span></span></td>
      <td><span data-ttu-id="01eb8-180">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-180">✓</span></span></td>
      <td><span data-ttu-id="01eb8-181">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-181">✓</span></span></td>
      <td><span data-ttu-id="01eb8-182"><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-182"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-183">Zobrazení SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="01eb8-183">SQL Data Warehouse view</span></span></td>
      <td><span data-ttu-id="01eb8-184">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-184">✓</span></span></td>
      <td><span data-ttu-id="01eb8-185">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-185">✓</span></span></td>
      <td><span data-ttu-id="01eb8-186">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-186">✓</span></span></td>
      <td><span data-ttu-id="01eb8-187"><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-187"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-188">Dimenze služby SQL Server Analysis Services</span><span class="sxs-lookup"><span data-stu-id="01eb8-188">SQL Server Analysis Services dimension</span></span></td>
      <td><span data-ttu-id="01eb8-189">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-189">✓</span></span></td>
      <td><span data-ttu-id="01eb8-190">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-190">✓</span></span></td>
      <td><span data-ttu-id="01eb8-191">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-191">✓</span></span></td>
      <td><span data-ttu-id="01eb8-192"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-192"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-193">SQL Server Analysis Services klíčového ukazatele výkonu</span><span class="sxs-lookup"><span data-stu-id="01eb8-193">SQL Server Analysis Services KPI</span></span></td>
      <td><span data-ttu-id="01eb8-194">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-194">✓</span></span></td>
      <td><span data-ttu-id="01eb8-195">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-195">✓</span></span></td>
      <td><span data-ttu-id="01eb8-196">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-196">✓</span></span></td>
      <td><span data-ttu-id="01eb8-197"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-197"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-198">Míra SQL Server Analysis Services</span><span class="sxs-lookup"><span data-stu-id="01eb8-198">SQL Server Analysis Services measure</span></span></td>
      <td><span data-ttu-id="01eb8-199">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-199">✓</span></span></td>
      <td><span data-ttu-id="01eb8-200">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-200">✓</span></span></td>
      <td><span data-ttu-id="01eb8-201">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-201">✓</span></span></td>
      <td><span data-ttu-id="01eb8-202"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-202"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-203">Tabulky SQL Server Analysis Services</span><span class="sxs-lookup"><span data-stu-id="01eb8-203">SQL Server Analysis Services table</span></span></td>
      <td><span data-ttu-id="01eb8-204">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-204">✓</span></span></td>
      <td><span data-ttu-id="01eb8-205">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-205">✓</span></span></td>
      <td><span data-ttu-id="01eb8-206">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-206">✓</span></span></td>
      <td><span data-ttu-id="01eb8-207"><font size=2>Aplikace Excel, Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-207"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-208">Sestavy služby SQL Server Reporting Services</span><span class="sxs-lookup"><span data-stu-id="01eb8-208">SQL Server Reporting Services report</span></span></td>
      <td><span data-ttu-id="01eb8-209">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-209">✓</span></span></td>
      <td><span data-ttu-id="01eb8-210">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-210">✓</span></span></td>
      <td><span data-ttu-id="01eb8-211">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-211">✓</span></span></td>
      <td><span data-ttu-id="01eb8-212"><font size=2>Prohlížeč</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-212"><font size=2>Browser</font></span></span></td>
      <td><span data-ttu-id="01eb8-213"><font size=2>Pouze servery v nativním režimu. Režim serveru SharePoint není podporována.</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-213"><font size=2>Native mode servers only. SharePoint mode is not supported.</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-214">Tabulka systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-214">SQL Server table</span></span></td>
      <td><span data-ttu-id="01eb8-215">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-215">✓</span></span></td>
      <td><span data-ttu-id="01eb8-216">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-216">✓</span></span></td>
      <td><span data-ttu-id="01eb8-217">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-217">✓</span></span></td>
      <td><span data-ttu-id="01eb8-218"><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-218"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-219">Zobrazení systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-219">SQL Server view</span></span></td>
      <td><span data-ttu-id="01eb8-220">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-220">✓</span></span></td>
      <td><span data-ttu-id="01eb8-221">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-221">✓</span></span></td>
      <td><span data-ttu-id="01eb8-222">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-222">✓</span></span></td>
      <td><span data-ttu-id="01eb8-223"><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-223"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-224">Tabulka Teradata</span><span class="sxs-lookup"><span data-stu-id="01eb8-224">Teradata table</span></span></td>
      <td><span data-ttu-id="01eb8-225">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-225">✓</span></span></td>
      <td><span data-ttu-id="01eb8-226">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-226">✓</span></span></td>
      <td><span data-ttu-id="01eb8-227">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-227">✓</span></span></td>
      <td><span data-ttu-id="01eb8-228"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-228"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-229">Zobrazení Teradata</span><span class="sxs-lookup"><span data-stu-id="01eb8-229">Teradata view</span></span></td>
      <td><span data-ttu-id="01eb8-230">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-230">✓</span></span></td>
      <td><span data-ttu-id="01eb8-231">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-231">✓</span></span></td>
      <td><span data-ttu-id="01eb8-232">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-232">✓</span></span></td>
      <td><span data-ttu-id="01eb8-233"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-233"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-234">Zobrazení SAP HANA</span><span class="sxs-lookup"><span data-stu-id="01eb8-234">SAP HANA view</span></span></td>
      <td><span data-ttu-id="01eb8-235">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-235">✓</span></span></td>
      <td><span data-ttu-id="01eb8-236">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-236">✓</span></span></td>
      <td><span data-ttu-id="01eb8-237">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-237">✓</span></span></td>
      <td><span data-ttu-id="01eb8-238"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-238"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-239">Tabulka DB2</span><span class="sxs-lookup"><span data-stu-id="01eb8-239">DB2 table</span></span></td>
      <td><span data-ttu-id="01eb8-240">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-240">✓</span></span></td>
      <td><span data-ttu-id="01eb8-241">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-241">✓</span></span></td>
      <td><span data-ttu-id="01eb8-242">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-242">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-243">Zobrazení DB2</span><span class="sxs-lookup"><span data-stu-id="01eb8-243">DB2 view</span></span></td>
      <td><span data-ttu-id="01eb8-244">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-244">✓</span></span></td>
      <td><span data-ttu-id="01eb8-245">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-245">✓</span></span></td>
      <td><span data-ttu-id="01eb8-246">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-246">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-247">Soubor systému souborů</span><span class="sxs-lookup"><span data-stu-id="01eb8-247">File system file</span></span></td>
      <td><span data-ttu-id="01eb8-248">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-248">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-249">Adresář serveru FTP</span><span class="sxs-lookup"><span data-stu-id="01eb8-249">FTP directory</span></span></td>
      <td><span data-ttu-id="01eb8-250">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-250">✓</span></span></td>
      <td><span data-ttu-id="01eb8-251">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-251">✓</span></span></td>
      <td><span data-ttu-id="01eb8-252">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-252">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-253">Soubor FTP</span><span class="sxs-lookup"><span data-stu-id="01eb8-253">FTP file</span></span></td>
      <td><span data-ttu-id="01eb8-254">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-254">✓</span></span></td>
      <td><span data-ttu-id="01eb8-255">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-255">✓</span></span></td>
      <td><span data-ttu-id="01eb8-256">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-256">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-257">Sestavy HTTP</span><span class="sxs-lookup"><span data-stu-id="01eb8-257">HTTP report</span></span></td>
      <td><span data-ttu-id="01eb8-258">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-258">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-259">Koncový bod HTTP</span><span class="sxs-lookup"><span data-stu-id="01eb8-259">HTTP endpoint</span></span></td>
      <td><span data-ttu-id="01eb8-260">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-260">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-261">Soubor protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="01eb8-261">HTTP file</span></span></td>
      <td><span data-ttu-id="01eb8-262">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-262">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-263">Sada entit OData</span><span class="sxs-lookup"><span data-stu-id="01eb8-263">OData entity set</span></span></td>
      <td><span data-ttu-id="01eb8-264">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-264">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-265">Funkce OData</span><span class="sxs-lookup"><span data-stu-id="01eb8-265">OData function</span></span></td>
      <td><span data-ttu-id="01eb8-266">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-266">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-267">Tabulka PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="01eb8-267">PostgreSQL table</span></span></td>
      <td><span data-ttu-id="01eb8-268">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-268">✓</span></span></td>
      <td><span data-ttu-id="01eb8-269">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-269">✓</span></span></td>
      <td><span data-ttu-id="01eb8-270">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-270">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-271">PostgreSQL zobrazení</span><span class="sxs-lookup"><span data-stu-id="01eb8-271">PostgreSQL view</span></span></td>
      <td><span data-ttu-id="01eb8-272">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-272">✓</span></span></td>
      <td><span data-ttu-id="01eb8-273">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-273">✓</span></span></td>
      <td><span data-ttu-id="01eb8-274">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-274">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-275">Zobrazení SAP HANA</span><span class="sxs-lookup"><span data-stu-id="01eb8-275">SAP HANA view</span></span></td>
      <td><span data-ttu-id="01eb8-276">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-276">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td> <span data-ttu-id="01eb8-277">Objekt služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="01eb8-277">Salesforce object</span></span></td>
      <td><span data-ttu-id="01eb8-278">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-278">✓</span></span></td>
      <td><span data-ttu-id="01eb8-279">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-279">✓</span></span></td>
      <td><span data-ttu-id="01eb8-280">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-280">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-281">Seznam serveru SharePoint</span><span class="sxs-lookup"><span data-stu-id="01eb8-281">SharePoint list</span></span> </td>
      <td><span data-ttu-id="01eb8-282">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-282">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-283">Azure Cosmos DB kolekce</span><span class="sxs-lookup"><span data-stu-id="01eb8-283">Azure Cosmos DB collection</span></span></td>
      <td><span data-ttu-id="01eb8-284">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-284">✓</span></span></td>
      <td><span data-ttu-id="01eb8-285">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-285">✓</span></span></td>
      <td><span data-ttu-id="01eb8-286">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-286">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-287">Obecná tabulka rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="01eb8-287">Generic ODBC table</span></span></td>
      <td><span data-ttu-id="01eb8-288">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-288">✓</span></span></td>
      <td><span data-ttu-id="01eb8-289">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-289">✓</span></span></td>
      <td><span data-ttu-id="01eb8-290">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-290">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-291">Obecná zobrazení rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="01eb8-291">Generic ODBC view</span></span></td>
      <td><span data-ttu-id="01eb8-292">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-292">✓</span></span></td>
      <td><span data-ttu-id="01eb8-293">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-293">✓</span></span></td>
      <td><span data-ttu-id="01eb8-294">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-294">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-295">Tabulka Cassandra</span><span class="sxs-lookup"><span data-stu-id="01eb8-295">Cassandra table</span></span></td>
      <td><span data-ttu-id="01eb8-296">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-296">✓</span></span></td>
      <td><span data-ttu-id="01eb8-297">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-297">✓</span></span></td>
      <td><span data-ttu-id="01eb8-298">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-298">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="01eb8-299"><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-299"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-300">Cassandra zobrazení</span><span class="sxs-lookup"><span data-stu-id="01eb8-300">Cassandra view</span></span></td>
      <td><span data-ttu-id="01eb8-301">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-301">✓</span></span></td>
      <td><span data-ttu-id="01eb8-302">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-302">✓</span></span></td>
      <td><span data-ttu-id="01eb8-303">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-303">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="01eb8-304"><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-304"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-305">Tabulka Sybase</span><span class="sxs-lookup"><span data-stu-id="01eb8-305">Sybase table</span></span></td>
      <td><span data-ttu-id="01eb8-306">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-306">✓</span></span></td>
      <td><span data-ttu-id="01eb8-307">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-307">✓</span></span></td>
      <td><span data-ttu-id="01eb8-308">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-308">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-309">Zobrazení Sybase</span><span class="sxs-lookup"><span data-stu-id="01eb8-309">Sybase view</span></span></td>
      <td><span data-ttu-id="01eb8-310">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-310">✓</span></span></td>
      <td><span data-ttu-id="01eb8-311">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-311">✓</span></span></td>
      <td><span data-ttu-id="01eb8-312">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-312">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-313">Tabulka MongoDB</span><span class="sxs-lookup"><span data-stu-id="01eb8-313">MongoDB table</span></span></td>
      <td><span data-ttu-id="01eb8-314">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-314">✓</span></span></td>
      <td><span data-ttu-id="01eb8-315">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-315">✓</span></span></td>
      <td><span data-ttu-id="01eb8-316">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-316">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="01eb8-317"><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-317"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-318">Zobrazení MongoDB</span><span class="sxs-lookup"><span data-stu-id="01eb8-318">MongoDB view</span></span></td>
      <td><span data-ttu-id="01eb8-319">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-319">✓</span></span></td>
      <td><span data-ttu-id="01eb8-320">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-320">✓</span></span></td>
      <td><span data-ttu-id="01eb8-321">✓</span><span class="sxs-lookup"><span data-stu-id="01eb8-321">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="01eb8-322"><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></span><span class="sxs-lookup"><span data-stu-id="01eb8-322"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
</table>

<span data-ttu-id="01eb8-323">Pokud potřebujete podporu pro další zdroje, odeslání toohello žádosti o funkce [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="01eb8-323">If you need support for additional sources, submit a feature request toohello [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).</span></span>


## <a name="data-source-reference-specification"></a><span data-ttu-id="01eb8-324">Specifikace odkaz na zdroj dat</span><span class="sxs-lookup"><span data-stu-id="01eb8-324">Data-source reference specification</span></span>
> [!NOTE]
> <span data-ttu-id="01eb8-325">Hello **DSL struktura** sloupec v hello následující tabulka uvádí jenom hello vlastnosti připojení pro kontejner objektů a dat "address" používané Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="01eb8-325">hello **DSL structure** column in hello following table lists only hello connection properties for "address" property bag that are used by Azure Data Catalog.</span></span> <span data-ttu-id="01eb8-326">To znamená balík vlastností "address" může obsahovat další vlastnosti připojení hello zdroje dat, které Azure Data Catalog potrvají, ale nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="01eb8-326">That is, "address" property bag can contain other connection properties of hello data source which Azure Data Catalog persists, but does not use.</span></span>

<table>
    <tr>
       <td><span data-ttu-id="01eb8-327"><b>Typ zdroje</b></span><span class="sxs-lookup"><span data-stu-id="01eb8-327"><b>Source type</b></span></span></td>
       <td><span data-ttu-id="01eb8-328"><b>Typ prostředku</b></span><span class="sxs-lookup"><span data-stu-id="01eb8-328"><b>Asset type</b></span></span></td>
       <td><span data-ttu-id="01eb8-329"><b>Typy objektů</b></span><span class="sxs-lookup"><span data-stu-id="01eb8-329"><b>Object types</b></span></span></td>
       <td><span data-ttu-id="01eb8-330"><b>Struktura DSL<b></span><span class="sxs-lookup"><span data-stu-id="01eb8-330"><b>DSL structure<b></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-331">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="01eb8-331">Azure Data Lake Store</span></span></td>
      <td><span data-ttu-id="01eb8-332">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-332">Container</span></span></td>
      <td><span data-ttu-id="01eb8-333">Data Lake</span><span class="sxs-lookup"><span data-stu-id="01eb8-333">Data Lake</span></span></td>
      <td><span data-ttu-id="01eb8-334">
        <font size=2>Protokol: webhdfs</span><span class="sxs-lookup"><span data-stu-id="01eb8-334">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="01eb8-335">Ověřování: {základní, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-335">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="01eb8-336">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-336">Address:</span></span> <br><span data-ttu-id="01eb8-337">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-337">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-338">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="01eb8-338">Azure Data Lake Store</span></span></td>
      <td><span data-ttu-id="01eb8-339">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-339">Table</span></span></td>
      <td><span data-ttu-id="01eb8-340">Adresář, soubor</span><span class="sxs-lookup"><span data-stu-id="01eb8-340">Directory, file</span></span></td>
      <td><span data-ttu-id="01eb8-341">
        <font size=2>Protokol: webhdfs</span><span class="sxs-lookup"><span data-stu-id="01eb8-341">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="01eb8-342">Ověřování: {základní, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-342">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="01eb8-343">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-343">Address:</span></span> <br><span data-ttu-id="01eb8-344">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-344">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-345">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="01eb8-345">Azure Storage</span></span></td>
      <td><span data-ttu-id="01eb8-346">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-346">Container</span></span></td>
      <td><span data-ttu-id="01eb8-347">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-347">Container</span></span></td>
      <td><span data-ttu-id="01eb8-348">
        <font size=2>Protokol:-objektů BLOB služby azure</span><span class="sxs-lookup"><span data-stu-id="01eb8-348">
        <font size=2> Protocol: azure-blobs</span></span> <br><span data-ttu-id="01eb8-349">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="01eb8-349">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="01eb8-350">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-350">Address:</span></span> <br><span data-ttu-id="01eb8-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény</span><span class="sxs-lookup"><span data-stu-id="01eb8-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="01eb8-352">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet</span><span class="sxs-lookup"><span data-stu-id="01eb8-352">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="01eb8-353">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontejner</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-353">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-354">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="01eb8-354">Azure Storage</span></span></td>
      <td><span data-ttu-id="01eb8-355">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-355">Table</span></span></td>
      <td><span data-ttu-id="01eb8-356">Objekt BLOB, adresář</span><span class="sxs-lookup"><span data-stu-id="01eb8-356">Blob, directory</span></span></td>
      <td><span data-ttu-id="01eb8-357">
        <font size=2>Protokol:-objektů BLOB služby azure</span><span class="sxs-lookup"><span data-stu-id="01eb8-357">
        <font size=2> Protocol: azure-blobs</span></span> <br><span data-ttu-id="01eb8-358">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="01eb8-358">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="01eb8-359">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-359">Address:</span></span> <br><span data-ttu-id="01eb8-360">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény</span><span class="sxs-lookup"><span data-stu-id="01eb8-360">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="01eb8-361">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet</span><span class="sxs-lookup"><span data-stu-id="01eb8-361">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="01eb8-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container</span></span> <br><span data-ttu-id="01eb8-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Jméno</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-364">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="01eb8-364">Azure Storage</span></span></td>
      <td><span data-ttu-id="01eb8-365">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-365">Container</span></span></td>
      <td><span data-ttu-id="01eb8-366">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-366">Container</span></span></td>
      <td><span data-ttu-id="01eb8-367">
        <font size=2>Protokol: azure – tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-367">
        <font size=2> Protocol: azure-tables</span></span> <br><span data-ttu-id="01eb8-368">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="01eb8-368">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="01eb8-369">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-369">Address:</span></span> <br><span data-ttu-id="01eb8-370">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény</span><span class="sxs-lookup"><span data-stu-id="01eb8-370">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="01eb8-371">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-371">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-372">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="01eb8-372">Azure Storage</span></span></td>
      <td><span data-ttu-id="01eb8-373">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-373">Table</span></span></td>
      <td><span data-ttu-id="01eb8-374">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-374">Table</span></span></td>
      <td><span data-ttu-id="01eb8-375">
        <font size=2>Protokol: azure – tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-375">
        <font size=2> Protocol: azure-tables</span></span> <br><span data-ttu-id="01eb8-376">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="01eb8-376">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="01eb8-377">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-377">Address:</span></span> <br><span data-ttu-id="01eb8-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény</span><span class="sxs-lookup"><span data-stu-id="01eb8-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="01eb8-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet</span><span class="sxs-lookup"><span data-stu-id="01eb8-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="01eb8-380">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Jméno</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-380">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-381">Cosmos</span><span class="sxs-lookup"><span data-stu-id="01eb8-381">Cosmos</span></span></td>
      <td><span data-ttu-id="01eb8-382">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-382">Container</span></span></td>
      <td><span data-ttu-id="01eb8-383">Virtuální cluster</span><span class="sxs-lookup"><span data-stu-id="01eb8-383">Virtual cluster</span></span></td>
      <td><span data-ttu-id="01eb8-384">
        <font size=2>Protokol: cosmos</span><span class="sxs-lookup"><span data-stu-id="01eb8-384">
        <font size=2> Protocol: cosmos</span></span> <br><span data-ttu-id="01eb8-385">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-385">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-386">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-386">Address:</span></span> <br><span data-ttu-id="01eb8-387">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-387">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-388">Cosmos</span><span class="sxs-lookup"><span data-stu-id="01eb8-388">Cosmos</span></span></td>
      <td><span data-ttu-id="01eb8-389">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-389">Table</span></span></td>
      <td><span data-ttu-id="01eb8-390">Datový proud, sadu datového proudu, zobrazení</span><span class="sxs-lookup"><span data-stu-id="01eb8-390">Stream, stream set, view</span></span></td>
      <td><span data-ttu-id="01eb8-391">
        <font size=2>Protokol: cosmos</span><span class="sxs-lookup"><span data-stu-id="01eb8-391">
        <font size=2> Protocol: cosmos</span></span> <br><span data-ttu-id="01eb8-392">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-392">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-393">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-393">Address:</span></span> <br><span data-ttu-id="01eb8-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-395">Datazen</span><span class="sxs-lookup"><span data-stu-id="01eb8-395">Datazen</span></span></td>
      <td><span data-ttu-id="01eb8-396">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-396">Container</span></span></td>
      <td><span data-ttu-id="01eb8-397">Lokality</span><span class="sxs-lookup"><span data-stu-id="01eb8-397">Site</span></span></td>
      <td><span data-ttu-id="01eb8-398">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="01eb8-398">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="01eb8-399">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-399">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="01eb8-400">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-400">Address:</span></span> <br><span data-ttu-id="01eb8-401">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-401">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-402">Datazen</span><span class="sxs-lookup"><span data-stu-id="01eb8-402">Datazen</span></span></td>
      <td><span data-ttu-id="01eb8-403">Sestava</span><span class="sxs-lookup"><span data-stu-id="01eb8-403">Report</span></span></td>
      <td><span data-ttu-id="01eb8-404">Sestavy, řídicí panel</span><span class="sxs-lookup"><span data-stu-id="01eb8-404">Report, dashboard</span></span></td>
      <td><span data-ttu-id="01eb8-405">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="01eb8-405">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="01eb8-406">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-406">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="01eb8-407">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-407">Address:</span></span> <br><span data-ttu-id="01eb8-408">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-408">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-409">DB2</span><span class="sxs-lookup"><span data-stu-id="01eb8-409">DB2</span></span></td>
      <td><span data-ttu-id="01eb8-410">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-410">Container</span></span></td>
      <td><span data-ttu-id="01eb8-411">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-411">Database</span></span></td>
      <td><span data-ttu-id="01eb8-412">
        <font size=2>Protokol: db2</span><span class="sxs-lookup"><span data-stu-id="01eb8-412">
        <font size=2> Protocol: db2</span></span> <br><span data-ttu-id="01eb8-413">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-413">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-414">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-414">Address:</span></span> <br><span data-ttu-id="01eb8-415">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-415">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-416">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-416">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-417">DB2</span><span class="sxs-lookup"><span data-stu-id="01eb8-417">DB2</span></span></td>
      <td><span data-ttu-id="01eb8-418">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-418">Table</span></span></td>
      <td><span data-ttu-id="01eb8-419">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-419">Table, view</span></span></td>
      <td><span data-ttu-id="01eb8-420">
        <font size=2>Protokol: db2</span><span class="sxs-lookup"><span data-stu-id="01eb8-420">
        <font size=2> Protocol: db2</span></span> <br><span data-ttu-id="01eb8-421">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-421">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-422">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-422">Address:</span></span> <br><span data-ttu-id="01eb8-423">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-423">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-424">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-424">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-425">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="01eb8-425">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="01eb8-426">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-426">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-427">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="01eb8-427">File system</span></span></td>
      <td><span data-ttu-id="01eb8-428">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-428">Table</span></span></td>
      <td><span data-ttu-id="01eb8-429">File</span><span class="sxs-lookup"><span data-stu-id="01eb8-429">File</span></span></td>
      <td><span data-ttu-id="01eb8-430">
        <font size=2>Protokol: soubor</span><span class="sxs-lookup"><span data-stu-id="01eb8-430">
        <font size=2> Protocol: file</span></span> <br><span data-ttu-id="01eb8-431">Ověřování: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-431">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="01eb8-432">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-432">Address:</span></span> <br><span data-ttu-id="01eb8-433">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cesta</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-433">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-434">FTP</span><span class="sxs-lookup"><span data-stu-id="01eb8-434">FTP</span></span></td>
      <td><span data-ttu-id="01eb8-435">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-435">Table</span></span></td>
      <td><span data-ttu-id="01eb8-436">Adresář, soubor</span><span class="sxs-lookup"><span data-stu-id="01eb8-436">Directory, file</span></span></td>
      <td><span data-ttu-id="01eb8-437">
        <font size=2>Protokol: ftp</span><span class="sxs-lookup"><span data-stu-id="01eb8-437">
        <font size=2> Protocol: ftp</span></span> <br><span data-ttu-id="01eb8-438">Ověřování: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-438">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="01eb8-439">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-439">Address:</span></span> <br><span data-ttu-id="01eb8-440">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-440">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-441">Hadoop Distributed File System</span><span class="sxs-lookup"><span data-stu-id="01eb8-441">Hadoop Distributed File System</span></span></td>
      <td><span data-ttu-id="01eb8-442">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-442">Container</span></span></td>
      <td><span data-ttu-id="01eb8-443">Cluster</span><span class="sxs-lookup"><span data-stu-id="01eb8-443">Cluster</span></span></td>
      <td><span data-ttu-id="01eb8-444">
        <font size=2>Protokol: webhdfs</span><span class="sxs-lookup"><span data-stu-id="01eb8-444">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="01eb8-445">Ověřování: {základní, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-445">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="01eb8-446">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-446">Address:</span></span> <br><span data-ttu-id="01eb8-447">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-447">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-448">Hadoop Distributed File System</span><span class="sxs-lookup"><span data-stu-id="01eb8-448">Hadoop Distributed File System</span></span></td>
      <td><span data-ttu-id="01eb8-449">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-449">Table</span></span></td>
      <td><span data-ttu-id="01eb8-450">Adresář, soubor</span><span class="sxs-lookup"><span data-stu-id="01eb8-450">Directory, file</span></span></td>
      <td><span data-ttu-id="01eb8-451">
        <font size=2>Protokol: webhdfs</span><span class="sxs-lookup"><span data-stu-id="01eb8-451">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="01eb8-452">Ověřování: {základní, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-452">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="01eb8-453">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-453">Address:</span></span> <br><span data-ttu-id="01eb8-454">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-454">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-455">Hive</span><span class="sxs-lookup"><span data-stu-id="01eb8-455">Hive</span></span></td>
      <td><span data-ttu-id="01eb8-456">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-456">Container</span></span></td>
      <td><span data-ttu-id="01eb8-457">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-457">Database</span></span></td>
      <td><span data-ttu-id="01eb8-458">
        <font size=2>Protokol: hive</span><span class="sxs-lookup"><span data-stu-id="01eb8-458">
        <font size=2> Protocol: hive</span></span> <br><span data-ttu-id="01eb8-459">Ověřování: {HDInsight, basic, uživatelské jméno, none}</span><span class="sxs-lookup"><span data-stu-id="01eb8-459">Authentication: {HDInsight, basic, username, none}</span></span> <br><span data-ttu-id="01eb8-460">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-460">Address:</span></span> <br><span data-ttu-id="01eb8-461">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-461">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-462">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-462">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-463">connectionProperties:</span><span class="sxs-lookup"><span data-stu-id="01eb8-463">connectionProperties:</span></span> <br><span data-ttu-id="01eb8-464">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-464">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-465">Hive</span><span class="sxs-lookup"><span data-stu-id="01eb8-465">Hive</span></span></td>
      <td><span data-ttu-id="01eb8-466">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-466">Table</span></span></td>
      <td><span data-ttu-id="01eb8-467">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-467">Table, view</span></span></td>
      <td><span data-ttu-id="01eb8-468">
        <font size=2>Protokol: hive</span><span class="sxs-lookup"><span data-stu-id="01eb8-468">
        <font size=2> Protocol: hive</span></span> <br><span data-ttu-id="01eb8-469">Ověřování: {HDInsight, basic, uživatelské jméno, none}</span><span class="sxs-lookup"><span data-stu-id="01eb8-469">Authentication: {HDInsight, basic, username, none}</span></span> <br><span data-ttu-id="01eb8-470">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-470">Address:</span></span> <br><span data-ttu-id="01eb8-471">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-471">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-472">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-472">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-473">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="01eb8-473">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="01eb8-474">connectionProperties:</span><span class="sxs-lookup"><span data-stu-id="01eb8-474">connectionProperties:</span></span> <br><span data-ttu-id="01eb8-475">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-475">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-476">HTTP</span><span class="sxs-lookup"><span data-stu-id="01eb8-476">HTTP</span></span></td>
      <td><span data-ttu-id="01eb8-477">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-477">Container</span></span></td>
      <td><span data-ttu-id="01eb8-478">Lokality</span><span class="sxs-lookup"><span data-stu-id="01eb8-478">Site</span></span></td>
      <td><span data-ttu-id="01eb8-479">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="01eb8-479">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="01eb8-480">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-480">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="01eb8-481">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-481">Address:</span></span> <br><span data-ttu-id="01eb8-482">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-482">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-483">HTTP</span><span class="sxs-lookup"><span data-stu-id="01eb8-483">HTTP</span></span></td>
      <td><span data-ttu-id="01eb8-484">Sestava</span><span class="sxs-lookup"><span data-stu-id="01eb8-484">Report</span></span></td>
      <td><span data-ttu-id="01eb8-485">Sestavy, řídicí panel</span><span class="sxs-lookup"><span data-stu-id="01eb8-485">Report, dashboard</span></span></td>
      <td><span data-ttu-id="01eb8-486">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="01eb8-486">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="01eb8-487">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-487">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="01eb8-488">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-488">Address:</span></span> <br><span data-ttu-id="01eb8-489">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-489">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-490">HTTP</span><span class="sxs-lookup"><span data-stu-id="01eb8-490">HTTP</span></span></td>
      <td><span data-ttu-id="01eb8-491">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-491">Table</span></span></td>
      <td><span data-ttu-id="01eb8-492">Koncový bod, soubor</span><span class="sxs-lookup"><span data-stu-id="01eb8-492">Endpoint, file</span></span></td>
      <td><span data-ttu-id="01eb8-493">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="01eb8-493">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="01eb8-494">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-494">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="01eb8-495">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-495">Address:</span></span> <br><span data-ttu-id="01eb8-496">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-496">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-497">MySQL</span><span class="sxs-lookup"><span data-stu-id="01eb8-497">MySQL</span></span></td>
      <td><span data-ttu-id="01eb8-498">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-498">Container</span></span></td>
      <td><span data-ttu-id="01eb8-499">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-499">Database</span></span></td>
      <td><span data-ttu-id="01eb8-500">
        <font size=2>Protokol: mysql</span><span class="sxs-lookup"><span data-stu-id="01eb8-500">
        <font size=2> Protocol: mysql</span></span> <br><span data-ttu-id="01eb8-501">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-501">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-502">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-502">Address:</span></span> <br><span data-ttu-id="01eb8-503">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-503">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-504">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-504">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-505">MySQL</span><span class="sxs-lookup"><span data-stu-id="01eb8-505">MySQL</span></span></td>
      <td><span data-ttu-id="01eb8-506">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-506">Table</span></span></td>
      <td><span data-ttu-id="01eb8-507">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-507">Table, view</span></span></td>
      <td><span data-ttu-id="01eb8-508">
        <font size=2>Protokol: mysql</span><span class="sxs-lookup"><span data-stu-id="01eb8-508">
        <font size=2> Protocol: mysql</span></span> <br><span data-ttu-id="01eb8-509">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-509">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-510">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-510">Address:</span></span> <br><span data-ttu-id="01eb8-511">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-511">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-512">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-512">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-513">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-513">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-514">OData</span><span class="sxs-lookup"><span data-stu-id="01eb8-514">OData</span></span></td>
      <td><span data-ttu-id="01eb8-515">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-515">Container</span></span></td>
      <td><span data-ttu-id="01eb8-516">Kontejneru entit</span><span class="sxs-lookup"><span data-stu-id="01eb8-516">Entity container</span></span></td>
      <td><span data-ttu-id="01eb8-517">
        <font size=2>Protokol: odata</span><span class="sxs-lookup"><span data-stu-id="01eb8-517">
        <font size=2> Protocol: odata</span></span> <br><span data-ttu-id="01eb8-518">Ověřování: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-518">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="01eb8-519">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-519">Address:</span></span> <br><span data-ttu-id="01eb8-520">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-520">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-521">OData</span><span class="sxs-lookup"><span data-stu-id="01eb8-521">OData</span></span></td>
      <td><span data-ttu-id="01eb8-522">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-522">Table</span></span></td>
      <td><span data-ttu-id="01eb8-523">Sady entit, – funkce</span><span class="sxs-lookup"><span data-stu-id="01eb8-523">Entity set, function</span></span></td>
      <td><span data-ttu-id="01eb8-524">
        <font size=2>Protokol: odata</span><span class="sxs-lookup"><span data-stu-id="01eb8-524">
        <font size=2> Protocol: odata</span></span> <br><span data-ttu-id="01eb8-525">Ověřování: {none, basic, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-525">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="01eb8-526">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-526">Address:</span></span> <br><span data-ttu-id="01eb8-527">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</span><span class="sxs-lookup"><span data-stu-id="01eb8-527">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="01eb8-528">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;prostředek</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-528">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; resource </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-529">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="01eb8-529">Oracle Database</span></span></td>
      <td><span data-ttu-id="01eb8-530">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-530">Container</span></span></td>
      <td><span data-ttu-id="01eb8-531">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-531">Database</span></span></td>
      <td><span data-ttu-id="01eb8-532">
        <font size=2>Protokol: oracle</span><span class="sxs-lookup"><span data-stu-id="01eb8-532">
        <font size=2> Protocol: oracle</span></span> <br><span data-ttu-id="01eb8-533">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-533">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-534">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-534">Address:</span></span> <br><span data-ttu-id="01eb8-535">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-535">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-536">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-536">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-537">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="01eb8-537">Oracle Database</span></span></td>
      <td><span data-ttu-id="01eb8-538">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-538">Table</span></span></td>
      <td><span data-ttu-id="01eb8-539">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-539">Table, view</span></span></td>
      <td><span data-ttu-id="01eb8-540">
        <font size=2>Protokol: oracle</span><span class="sxs-lookup"><span data-stu-id="01eb8-540">
        <font size=2> Protocol: oracle</span></span> <br><span data-ttu-id="01eb8-541">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-541">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-542">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-542">Address:</span></span> <br><span data-ttu-id="01eb8-543">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-543">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-544">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-544">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-545">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="01eb8-545">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="01eb8-546">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-546">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-547">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="01eb8-547">PostgreSQL</span></span></td>
      <td><span data-ttu-id="01eb8-548">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-548">Container</span></span></td>
      <td><span data-ttu-id="01eb8-549">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-549">Database</span></span></td>
      <td><span data-ttu-id="01eb8-550">
        <font size=2>Protokol: postgresql</span><span class="sxs-lookup"><span data-stu-id="01eb8-550">
        <font size=2> Protocol: postgresql</span></span> <br><span data-ttu-id="01eb8-551">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-551">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-552">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-552">Address:</span></span> <br><span data-ttu-id="01eb8-553">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-553">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-554">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-554">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-555">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="01eb8-555">PostgreSQL</span></span></td>
      <td><span data-ttu-id="01eb8-556">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-556">Table</span></span></td>
      <td><span data-ttu-id="01eb8-557">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-557">Table, view</span></span></td>
      <td><span data-ttu-id="01eb8-558">
        <font size=2>Protokol: postgresql</span><span class="sxs-lookup"><span data-stu-id="01eb8-558">
        <font size=2> Protocol: postgresql</span></span> <br><span data-ttu-id="01eb8-559">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-559">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-560">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-560">Address:</span></span> <br><span data-ttu-id="01eb8-561">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-561">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-562">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-562">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-563">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="01eb8-563">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="01eb8-564">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-564">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-565">Power BI</span><span class="sxs-lookup"><span data-stu-id="01eb8-565">Power BI</span></span></td>
      <td><span data-ttu-id="01eb8-566">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-566">Container</span></span></td>
      <td><span data-ttu-id="01eb8-567">Lokality</span><span class="sxs-lookup"><span data-stu-id="01eb8-567">Site</span></span></td>
      <td><span data-ttu-id="01eb8-568">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="01eb8-568">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="01eb8-569">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-569">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="01eb8-570">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-570">Address:</span></span> <br><span data-ttu-id="01eb8-571">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-571">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-572">Power BI</span><span class="sxs-lookup"><span data-stu-id="01eb8-572">Power BI</span></span></td>
      <td><span data-ttu-id="01eb8-573">Sestava</span><span class="sxs-lookup"><span data-stu-id="01eb8-573">Report</span></span></td>
      <td><span data-ttu-id="01eb8-574">Sestavy, řídicí panel</span><span class="sxs-lookup"><span data-stu-id="01eb8-574">Report, dashboard</span></span></td>
      <td><span data-ttu-id="01eb8-575">
        <font size=2>Protokol: http</span><span class="sxs-lookup"><span data-stu-id="01eb8-575">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="01eb8-576">Ověřování: {none, basic, windows, oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-576">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="01eb8-577">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-577">Address:</span></span> <br><span data-ttu-id="01eb8-578">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-578">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-579">Power Query</span><span class="sxs-lookup"><span data-stu-id="01eb8-579">Power Query</span></span></td>
      <td><span data-ttu-id="01eb8-580">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-580">Table</span></span></td>
      <td><span data-ttu-id="01eb8-581">Data hybridní webové aplikace</span><span class="sxs-lookup"><span data-stu-id="01eb8-581">Data mashup</span></span></td>
      <td><span data-ttu-id="01eb8-582">
        <font size=2>Protokol: power dotazu</span><span class="sxs-lookup"><span data-stu-id="01eb8-582">
        <font size=2> Protocol: power-query</span></span> <br><span data-ttu-id="01eb8-583">Ověřování: {oauth}</span><span class="sxs-lookup"><span data-stu-id="01eb8-583">Authentication: {oauth}</span></span> <br><span data-ttu-id="01eb8-584">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-584">Address:</span></span> <br><span data-ttu-id="01eb8-585">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-585">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-586">Salesforce</span><span class="sxs-lookup"><span data-stu-id="01eb8-586">Salesforce</span></span></td>
      <td><span data-ttu-id="01eb8-587">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-587">Table</span></span></td>
      <td><span data-ttu-id="01eb8-588">Objekt</span><span class="sxs-lookup"><span data-stu-id="01eb8-588">Object</span></span></td>
      <td><span data-ttu-id="01eb8-589">
        <font size=2>Protokol: salesforce-com</span><span class="sxs-lookup"><span data-stu-id="01eb8-589">
        <font size=2> Protocol: salesforce-com</span></span> <br><span data-ttu-id="01eb8-590">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-590">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-591">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-591">Address:</span></span> <br><span data-ttu-id="01eb8-592">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer</span><span class="sxs-lookup"><span data-stu-id="01eb8-592">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; loginServer</span></span> <br><span data-ttu-id="01eb8-593">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;– Třída</span><span class="sxs-lookup"><span data-stu-id="01eb8-593">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class</span></span> <br><span data-ttu-id="01eb8-594">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Název položky</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-594">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; itemName </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-595">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="01eb8-595">SAP HANA</span></span></td>
      <td><span data-ttu-id="01eb8-596">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-596">Container</span></span></td>
      <td><span data-ttu-id="01eb8-597">Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-597">Server</span></span></td>
      <td><span data-ttu-id="01eb8-598">
        <font size=2>Protokol: sap hana-sql</span><span class="sxs-lookup"><span data-stu-id="01eb8-598">
        <font size=2> Protocol: sap-hana-sql</span></span> <br><span data-ttu-id="01eb8-599">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-599">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-600">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-600">Address:</span></span> <br><span data-ttu-id="01eb8-601">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-601">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-602">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="01eb8-602">SAP HANA</span></span></td>
      <td><span data-ttu-id="01eb8-603">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-603">Table</span></span></td>
      <td><span data-ttu-id="01eb8-604">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="01eb8-604">View</span></span></td>
      <td><span data-ttu-id="01eb8-605">
        <font size=2>Protokol: sap hana-sql</span><span class="sxs-lookup"><span data-stu-id="01eb8-605">
        <font size=2> Protocol: sap-hana-sql</span></span> <br><span data-ttu-id="01eb8-606">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-606">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-607">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-607">Address:</span></span> <br><span data-ttu-id="01eb8-608">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-608">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-609">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="01eb8-609">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="01eb8-610">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-610">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-611">SharePoint</span><span class="sxs-lookup"><span data-stu-id="01eb8-611">SharePoint</span></span></td>
      <td><span data-ttu-id="01eb8-612">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-612">Table</span></span></td>
      <td><span data-ttu-id="01eb8-613">Seznam</span><span class="sxs-lookup"><span data-stu-id="01eb8-613">List</span></span></td>
      <td><span data-ttu-id="01eb8-614">
        <font size=2>Protokolů: sharepoint – seznam</span><span class="sxs-lookup"><span data-stu-id="01eb8-614">
        <font size=2> Protocol: sharepoint-list</span></span> <br><span data-ttu-id="01eb8-615">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-615">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-616">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-616">Address:</span></span> <br><span data-ttu-id="01eb8-617">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-617">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-618">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="01eb8-618">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="01eb8-619">Příkaz</span><span class="sxs-lookup"><span data-stu-id="01eb8-619">Command</span></span></td>
      <td><span data-ttu-id="01eb8-620">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="01eb8-620">Stored procedure</span></span></td>
      <td><span data-ttu-id="01eb8-621">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="01eb8-621">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="01eb8-622">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-622">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-623">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-623">Address:</span></span> <br><span data-ttu-id="01eb8-624">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-624">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-625">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-625">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-626">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="01eb8-626">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="01eb8-627">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-627">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-628">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="01eb8-628">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="01eb8-629">TableValuedFunction</span><span class="sxs-lookup"><span data-stu-id="01eb8-629">TableValuedFunction</span></span></td>
      <td><span data-ttu-id="01eb8-630">Funkce vracející tabulku</span><span class="sxs-lookup"><span data-stu-id="01eb8-630">Table-valued function</span></span></td>
      <td><span data-ttu-id="01eb8-631">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="01eb8-631">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="01eb8-632">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-632">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-633">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-633">Address:</span></span> <br><span data-ttu-id="01eb8-634">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-634">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-635">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-635">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-636">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="01eb8-636">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="01eb8-637">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-637">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-638">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="01eb8-638">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="01eb8-639">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-639">Container</span></span></td>
      <td><span data-ttu-id="01eb8-640">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-640">Database</span></span></td>
      <td><span data-ttu-id="01eb8-641">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="01eb8-641">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="01eb8-642">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-642">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-643">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-643">Address:</span></span> <br><span data-ttu-id="01eb8-644">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-644">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-645">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-645">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-646">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="01eb8-646">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="01eb8-647">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-647">Table</span></span></td>
      <td><span data-ttu-id="01eb8-648">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-648">Table, view</span></span></td>
      <td><span data-ttu-id="01eb8-649">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="01eb8-649">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="01eb8-650">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-650">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-651">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-651">Address:</span></span> <br><span data-ttu-id="01eb8-652">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-652">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-653">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-653">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-654">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="01eb8-654">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="01eb8-655">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-655">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-656">SQL Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-656">SQL Server</span></span></td>
      <td><span data-ttu-id="01eb8-657">Příkaz</span><span class="sxs-lookup"><span data-stu-id="01eb8-657">Command</span></span></td>
      <td><span data-ttu-id="01eb8-658">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="01eb8-658">Stored procedure</span></span></td>
      <td><span data-ttu-id="01eb8-659">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="01eb8-659">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="01eb8-660">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-660">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-661">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-661">Address:</span></span> <br><span data-ttu-id="01eb8-662">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-662">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-663">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-663">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-664">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="01eb8-664">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="01eb8-665">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-665">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-666">SQL Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-666">SQL Server</span></span></td>
      <td><span data-ttu-id="01eb8-667">TableValuedFunction</span><span class="sxs-lookup"><span data-stu-id="01eb8-667">TableValuedFunction</span></span></td>
      <td><span data-ttu-id="01eb8-668">Funkce vracející tabulku</span><span class="sxs-lookup"><span data-stu-id="01eb8-668">Table-valued function</span></span></td>
      <td><span data-ttu-id="01eb8-669">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="01eb8-669">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="01eb8-670">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-670">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-671">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-671">Address:</span></span> <br><span data-ttu-id="01eb8-672">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-672">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-673">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-673">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-674">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="01eb8-674">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="01eb8-675">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-675">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-676">SQL Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-676">SQL Server</span></span></td>
      <td><span data-ttu-id="01eb8-677">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-677">Container</span></span></td>
      <td><span data-ttu-id="01eb8-678">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-678">Database</span></span></td>
      <td><span data-ttu-id="01eb8-679">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="01eb8-679">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="01eb8-680">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-680">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-681">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-681">Address:</span></span> <br><span data-ttu-id="01eb8-682">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-682">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-683">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-683">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-684">SQL Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-684">SQL Server</span></span></td>
      <td><span data-ttu-id="01eb8-685">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-685">Table</span></span></td>
      <td><span data-ttu-id="01eb8-686">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-686">Table, view</span></span></td>
      <td><span data-ttu-id="01eb8-687">
        <font size=2>Protokol: tds.</span><span class="sxs-lookup"><span data-stu-id="01eb8-687">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="01eb8-688">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-688">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-689">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-689">Address:</span></span> <br><span data-ttu-id="01eb8-690">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-690">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-691">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-691">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-692">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="01eb8-692">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="01eb8-693">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-693">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-694">SQL Server Analysis Services multidimensional</span><span class="sxs-lookup"><span data-stu-id="01eb8-694">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="01eb8-695">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-695">Container</span></span></td>
      <td><span data-ttu-id="01eb8-696">Model</span><span class="sxs-lookup"><span data-stu-id="01eb8-696">Model</span></span></td>
      <td><span data-ttu-id="01eb8-697">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="01eb8-697">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="01eb8-698">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="01eb8-698">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="01eb8-699">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-699">Address:</span></span> <br><span data-ttu-id="01eb8-700">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-700">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-701">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-701">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-702">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-702">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-703">SQL Server Analysis Services multidimensional</span><span class="sxs-lookup"><span data-stu-id="01eb8-703">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="01eb8-704">KLÍČOVÝ UKAZATEL VÝKONU</span><span class="sxs-lookup"><span data-stu-id="01eb8-704">KPI</span></span></td>
      <td><span data-ttu-id="01eb8-705">KLÍČOVÝ UKAZATEL VÝKONU</span><span class="sxs-lookup"><span data-stu-id="01eb8-705">KPI</span></span></td>
      <td><span data-ttu-id="01eb8-706">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="01eb8-706">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="01eb8-707">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="01eb8-707">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="01eb8-708">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-708">Address:</span></span> <br><span data-ttu-id="01eb8-709">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-709">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-710">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-710">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-711">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="01eb8-711">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="01eb8-712">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="01eb8-712">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="01eb8-713">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {klíčový ukazatel výkonu}</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-713">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-714">SQL Server Analysis Services multidimensional</span><span class="sxs-lookup"><span data-stu-id="01eb8-714">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="01eb8-715">Míra</span><span class="sxs-lookup"><span data-stu-id="01eb8-715">Measure</span></span></td>
      <td><span data-ttu-id="01eb8-716">Míra</span><span class="sxs-lookup"><span data-stu-id="01eb8-716">Measure</span></span></td>
      <td><span data-ttu-id="01eb8-717">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="01eb8-717">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="01eb8-718">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="01eb8-718">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="01eb8-719">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-719">Address:</span></span> <br><span data-ttu-id="01eb8-720">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-720">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-721">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-721">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-722">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="01eb8-722">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="01eb8-723">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="01eb8-723">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="01eb8-724">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-724">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-725">SQL Server Analysis Services multidimensional</span><span class="sxs-lookup"><span data-stu-id="01eb8-725">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="01eb8-726">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-726">Table</span></span></td>
      <td><span data-ttu-id="01eb8-727">Dimenze</span><span class="sxs-lookup"><span data-stu-id="01eb8-727">Dimension</span></span></td>
      <td><span data-ttu-id="01eb8-728">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="01eb8-728">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="01eb8-729">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="01eb8-729">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="01eb8-730">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-730">Address:</span></span> <br><span data-ttu-id="01eb8-731">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-731">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-732">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-732">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-733">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="01eb8-733">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="01eb8-734">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="01eb8-734">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="01eb8-735">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {{dimenze}</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-735">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Dimension} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-736">SQL Server Analysis Services tabulkové</span><span class="sxs-lookup"><span data-stu-id="01eb8-736">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="01eb8-737">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-737">Container</span></span></td>
      <td><span data-ttu-id="01eb8-738">Model</span><span class="sxs-lookup"><span data-stu-id="01eb8-738">Model</span></span></td>
      <td><span data-ttu-id="01eb8-739">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="01eb8-739">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="01eb8-740">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="01eb8-740">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="01eb8-741">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-741">Address:</span></span> <br><span data-ttu-id="01eb8-742">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-742">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-743">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-743">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-744">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-744">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-745">SQL Server Analysis Services tabulkové</span><span class="sxs-lookup"><span data-stu-id="01eb8-745">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="01eb8-746">KLÍČOVÝ UKAZATEL VÝKONU</span><span class="sxs-lookup"><span data-stu-id="01eb8-746">KPI</span></span></td>
      <td><span data-ttu-id="01eb8-747">KLÍČOVÝ UKAZATEL VÝKONU</span><span class="sxs-lookup"><span data-stu-id="01eb8-747">KPI</span></span></td>
      <td><span data-ttu-id="01eb8-748">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="01eb8-748">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="01eb8-749">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="01eb8-749">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="01eb8-750">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-750">Address:</span></span> <br><span data-ttu-id="01eb8-751">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-751">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-752">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-752">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-753">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="01eb8-753">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="01eb8-754">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="01eb8-754">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="01eb8-755">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {klíčový ukazatel výkonu}</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-755">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-756">SQL Server Analysis Services tabulkové</span><span class="sxs-lookup"><span data-stu-id="01eb8-756">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="01eb8-757">Míra</span><span class="sxs-lookup"><span data-stu-id="01eb8-757">Measure</span></span></td>
      <td><span data-ttu-id="01eb8-758">Míra</span><span class="sxs-lookup"><span data-stu-id="01eb8-758">Measure</span></span></td>
      <td><span data-ttu-id="01eb8-759">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="01eb8-759">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="01eb8-760">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="01eb8-760">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="01eb8-761">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-761">Address:</span></span> <br><span data-ttu-id="01eb8-762">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-762">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-763">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-763">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-764">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="01eb8-764">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="01eb8-765">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="01eb8-765">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="01eb8-766">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-766">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-767">SQL Server Analysis Services tabulkové</span><span class="sxs-lookup"><span data-stu-id="01eb8-767">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="01eb8-768">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-768">Table</span></span></td>
      <td><span data-ttu-id="01eb8-769">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-769">Table</span></span></td>
      <td><span data-ttu-id="01eb8-770">
        <font size=2>Protokolu: analýza služby</span><span class="sxs-lookup"><span data-stu-id="01eb8-770">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="01eb8-771">Ověřování: {windows, basic, anonymní, none}</span><span class="sxs-lookup"><span data-stu-id="01eb8-771">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="01eb8-772">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-772">Address:</span></span> <br><span data-ttu-id="01eb8-773">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-773">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-774">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-774">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-775">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="01eb8-775">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="01eb8-776">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="01eb8-776">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="01eb8-777">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Table}</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-777">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Table} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-778">SQL Server Reporting Services</span><span class="sxs-lookup"><span data-stu-id="01eb8-778">SQL Server Reporting Services</span></span></td>
      <td><span data-ttu-id="01eb8-779">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-779">Container</span></span></td>
      <td><span data-ttu-id="01eb8-780">Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-780">Server</span></span></td>
      <td><span data-ttu-id="01eb8-781">
        <font size=2>Protokol: reporting services</span><span class="sxs-lookup"><span data-stu-id="01eb8-781">
        <font size=2> Protocol: reporting-services</span></span> <br><span data-ttu-id="01eb8-782">Ověřování: {windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-782">Authentication: {windows}</span></span> <br><span data-ttu-id="01eb8-783">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-783">Address:</span></span> <br><span data-ttu-id="01eb8-784">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-784">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-785">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze: {ReportingService2010}</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-785">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-786">SQL Server Reporting Services</span><span class="sxs-lookup"><span data-stu-id="01eb8-786">SQL Server Reporting Services</span></span></td>
      <td><span data-ttu-id="01eb8-787">Sestava</span><span class="sxs-lookup"><span data-stu-id="01eb8-787">Report</span></span></td>
      <td><span data-ttu-id="01eb8-788">Sestava</span><span class="sxs-lookup"><span data-stu-id="01eb8-788">Report</span></span></td>
      <td><span data-ttu-id="01eb8-789">
        <font size=2>Protokol: reporting services</span><span class="sxs-lookup"><span data-stu-id="01eb8-789">
        <font size=2> Protocol: reporting-services</span></span> <br><span data-ttu-id="01eb8-790">Ověřování: {windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-790">Authentication: {windows}</span></span> <br><span data-ttu-id="01eb8-791">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-791">Address:</span></span> <br><span data-ttu-id="01eb8-792">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-792">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-793">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cesta</span><span class="sxs-lookup"><span data-stu-id="01eb8-793">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path</span></span> <br><span data-ttu-id="01eb8-794">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze: {ReportingService2010}</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-794">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-795">Teradata</span><span class="sxs-lookup"><span data-stu-id="01eb8-795">Teradata</span></span></td>
      <td><span data-ttu-id="01eb8-796">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-796">Container</span></span></td>
      <td><span data-ttu-id="01eb8-797">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-797">Database</span></span></td>
      <td><span data-ttu-id="01eb8-798">
        <font size=2>Protokol: teradata</span><span class="sxs-lookup"><span data-stu-id="01eb8-798">
        <font size=2> Protocol: teradata</span></span> <br><span data-ttu-id="01eb8-799">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-799">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-800">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-800">Address:</span></span> <br><span data-ttu-id="01eb8-801">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-801">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-802">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-802">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-803">Teradata</span><span class="sxs-lookup"><span data-stu-id="01eb8-803">Teradata</span></span></td>
      <td><span data-ttu-id="01eb8-804">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-804">Table</span></span></td>
      <td><span data-ttu-id="01eb8-805">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-805">Table, view</span></span></td>
      <td><span data-ttu-id="01eb8-806">
        <font size=2>Protokol: teradata</span><span class="sxs-lookup"><span data-stu-id="01eb8-806">
        <font size=2> Protocol: teradata</span></span> <br><span data-ttu-id="01eb8-807">Ověřování: {protokol, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-807">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="01eb8-808">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-808">Address:</span></span> <br><span data-ttu-id="01eb8-809">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-809">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-810">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-810">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-811">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-811">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-812">SQL Server Master Data Services</span><span class="sxs-lookup"><span data-stu-id="01eb8-812">SQL Server Master Data Services</span></span></td>
      <td><span data-ttu-id="01eb8-813">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-813">Container</span></span></td>
      <td><span data-ttu-id="01eb8-814">Model</span><span class="sxs-lookup"><span data-stu-id="01eb8-814">Model</span></span></td>
      <td><span data-ttu-id="01eb8-815">
        <font size="2">Protokol: mssql-mds</span><span class="sxs-lookup"><span data-stu-id="01eb8-815">
        <font size="2"> Protocol: mssql-mds</span></span> <br><span data-ttu-id="01eb8-816">Ověřování: {windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-816">Authentication: {windows}</span></span> <br><span data-ttu-id="01eb8-817">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-817">Address:</span></span> <br><span data-ttu-id="01eb8-818">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</span><span class="sxs-lookup"><span data-stu-id="01eb8-818">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="01eb8-819">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="01eb8-819">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="01eb8-820">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-820">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-821">SQL Server Master Data Services</span><span class="sxs-lookup"><span data-stu-id="01eb8-821">SQL Server Master Data Services</span></span></td>
      <td><span data-ttu-id="01eb8-822">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-822">Table</span></span></td>
      <td><span data-ttu-id="01eb8-823">Entita</span><span class="sxs-lookup"><span data-stu-id="01eb8-823">Entity</span></span></td>
      <td><span data-ttu-id="01eb8-824">
        <font size="2">Protokol: mssql-mds</span><span class="sxs-lookup"><span data-stu-id="01eb8-824">
        <font size="2"> Protocol: mssql-mds</span></span> <br><span data-ttu-id="01eb8-825">Ověřování: {windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-825">Authentication: {windows}</span></span> <br><span data-ttu-id="01eb8-826">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-826">Address:</span></span> <br><span data-ttu-id="01eb8-827">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</span><span class="sxs-lookup"><span data-stu-id="01eb8-827">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="01eb8-828">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</span><span class="sxs-lookup"><span data-stu-id="01eb8-828">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="01eb8-829">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze</span><span class="sxs-lookup"><span data-stu-id="01eb8-829">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version</span></span> <br><span data-ttu-id="01eb8-830">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;entity</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-830">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; entity </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-831">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="01eb8-831">Azure Cosmos DB</span></span></td>
      <td><span data-ttu-id="01eb8-832">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-832">Container</span></span></td>
      <td><span data-ttu-id="01eb8-833">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-833">Database</span></span></td>
      <td><span data-ttu-id="01eb8-834">
        <font size=2>Protokol: dokument db</span><span class="sxs-lookup"><span data-stu-id="01eb8-834">
        <font size=2> Protocol: document-db</span></span> <br><span data-ttu-id="01eb8-835">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="01eb8-835">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="01eb8-836">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-836">Address:</span></span> <br><span data-ttu-id="01eb8-837">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</span><span class="sxs-lookup"><span data-stu-id="01eb8-837">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="01eb8-838">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-838">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-839">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="01eb8-839">Azure Cosmos DB</span></span></td>
      <td><span data-ttu-id="01eb8-840">Kolekce</span><span class="sxs-lookup"><span data-stu-id="01eb8-840">Collection</span></span></td>
      <td><span data-ttu-id="01eb8-841">Kolekce</span><span class="sxs-lookup"><span data-stu-id="01eb8-841">Collection</span></span></td>
      <td><span data-ttu-id="01eb8-842">
        <font size=2>Protokol: dokument db</span><span class="sxs-lookup"><span data-stu-id="01eb8-842">
        <font size=2> Protocol: document-db</span></span> <br><span data-ttu-id="01eb8-843">Ověřování: {azure přístupový klíč}</span><span class="sxs-lookup"><span data-stu-id="01eb8-843">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="01eb8-844">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-844">Address:</span></span> <br><span data-ttu-id="01eb8-845">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</span><span class="sxs-lookup"><span data-stu-id="01eb8-845">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="01eb8-846">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-846">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-847">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kolekce</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-847">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; collection </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-848">Obecná rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="01eb8-848">Generic ODBC</span></span></td>
      <td><span data-ttu-id="01eb8-849">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-849">Container</span></span></td>
      <td><span data-ttu-id="01eb8-850">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-850">Database</span></span></td>
      <td><span data-ttu-id="01eb8-851">
        <font size=2>Protokol: rozhraní odbc</span><span class="sxs-lookup"><span data-stu-id="01eb8-851">
        <font size=2> Protocol: odbc</span></span> <br><span data-ttu-id="01eb8-852">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-852">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-853">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-853">Address:</span></span> <br><span data-ttu-id="01eb8-854">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Možnosti</span><span class="sxs-lookup"><span data-stu-id="01eb8-854">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options</span></span> <br><span data-ttu-id="01eb8-855">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-855">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-856">Obecná rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="01eb8-856">Generic ODBC</span></span></td>
      <td><span data-ttu-id="01eb8-857">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-857">Table</span></span></td>
      <td><span data-ttu-id="01eb8-858">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-858">Table, View</span></span></td>
      <td><span data-ttu-id="01eb8-859">
        <font size=2>Protokol: rozhraní odbc</span><span class="sxs-lookup"><span data-stu-id="01eb8-859">
        <font size=2> Protocol: odbc</span></span> <br><span data-ttu-id="01eb8-860">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-860">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-861">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-861">Address:</span></span> <br><span data-ttu-id="01eb8-862">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Možnosti</span><span class="sxs-lookup"><span data-stu-id="01eb8-862">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options</span></span> <br><span data-ttu-id="01eb8-863">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-863">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-864">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</span><span class="sxs-lookup"><span data-stu-id="01eb8-864">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="01eb8-865">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-865">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-866">Sybase</span><span class="sxs-lookup"><span data-stu-id="01eb8-866">Sybase</span></span></td>
      <td><span data-ttu-id="01eb8-867">Kontejner</span><span class="sxs-lookup"><span data-stu-id="01eb8-867">Container</span></span></td>
      <td><span data-ttu-id="01eb8-868">Databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-868">Database</span></span></td>
      <td><span data-ttu-id="01eb8-869">
        <font size=2>protokol: sybase</span><span class="sxs-lookup"><span data-stu-id="01eb8-869">
        <font size=2> protocol: sybase</span></span> <br><span data-ttu-id="01eb8-870">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-870">authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-871">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-871">address:</span></span> <br><span data-ttu-id="01eb8-872">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-872">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-873">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-873">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-874">Sybase</span><span class="sxs-lookup"><span data-stu-id="01eb8-874">Sybase</span></span></td>
      <td><span data-ttu-id="01eb8-875">Table</span><span class="sxs-lookup"><span data-stu-id="01eb8-875">Table</span></span></td>
      <td><span data-ttu-id="01eb8-876">Zobrazení tabulky</span><span class="sxs-lookup"><span data-stu-id="01eb8-876">Table, View</span></span></td>
      <td><span data-ttu-id="01eb8-877">
        <font size=2>protokol: sybase</span><span class="sxs-lookup"><span data-stu-id="01eb8-877">
        <font size=2> protocol: sybase</span></span> <br><span data-ttu-id="01eb8-878">Ověřování: {základní, windows}</span><span class="sxs-lookup"><span data-stu-id="01eb8-878">authentication: {basic, windows}</span></span> <br><span data-ttu-id="01eb8-879">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-879">address:</span></span> <br><span data-ttu-id="01eb8-880">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</span><span class="sxs-lookup"><span data-stu-id="01eb8-880">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="01eb8-881">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</span><span class="sxs-lookup"><span data-stu-id="01eb8-881">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="01eb8-882">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</span><span class="sxs-lookup"><span data-stu-id="01eb8-882">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="01eb8-883">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-883">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="01eb8-884">Jiné (žádná z výše uvedených hello)</span><span class="sxs-lookup"><span data-stu-id="01eb8-884">Other (none of hello above)</span></span></td>
      <td>\*</td>
      <td>\*</td>
      <td><span data-ttu-id="01eb8-885">
        <font size=2>Protokol: Obecné – prostředek</span><span class="sxs-lookup"><span data-stu-id="01eb8-885">
        <font size=2> Protocol: generic-asset</span></span> <br><span data-ttu-id="01eb8-886">Adresa:</span><span class="sxs-lookup"><span data-stu-id="01eb8-886">Address:</span></span> <br><span data-ttu-id="01eb8-887">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID</font>
      </span><span class="sxs-lookup"><span data-stu-id="01eb8-887">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; assetId </font>
      </span></span></td>
    </tr>
</table>
