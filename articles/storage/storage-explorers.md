---
title: "aaaTools pro práci s Azure Storage | Microsoft Docs"
description: "Seznam nástrojů, které vám umožňují tooview nebo práci s daty Azure Storage."
services: storage
documentationcenter: 
author: dineshmurthy
manager: jahogg
editor: tysonn
ms.assetid: e4748642-98c4-437e-b0ed-4f9641c2e894
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: dineshmurthy
ms.openlocfilehash: 3308de2153099a05a676ab1d76426bd932e8a96c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-client-tools"></a><span data-ttu-id="29b1c-103">Klientské nástroje pro Azure Storage</span><span class="sxs-lookup"><span data-stu-id="29b1c-103">Azure Storage Client Tools</span></span>
<span data-ttu-id="29b1c-104">Uživatelé Azure Storage často mají toobe možné tooview nebo interakci s jejich daty pomocí klienta nástroje Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="29b1c-104">Users of Azure Storage frequently want toobe able tooview/interact with their data using an Azure Storage client tool.</span></span> <span data-ttu-id="29b1c-105">V následujících tabulkách hello jsme seznam několik nástrojů, které vám umožňují toodo to.</span><span class="sxs-lookup"><span data-stu-id="29b1c-105">In hello tables below, we list a number of tools that allow you toodo this.</span></span> <span data-ttu-id="29b1c-106">Můžeme uvést "X" v každém bloku pokud poskytuje možnost hello tooeither výčet nebo přístup hello data abstraction.</span><span class="sxs-lookup"><span data-stu-id="29b1c-106">We put an "X" in each block if it provides hello ability tooeither enumerate and/or access hello data abstraction.</span></span> <span data-ttu-id="29b1c-107">Hello tabulka také ukazuje, zda je volné hello nástroje, či nikoliv.</span><span class="sxs-lookup"><span data-stu-id="29b1c-107">hello table also shows if hello tools is free or not.</span></span> <span data-ttu-id="29b1c-108">"Zkušební verze" znamená, že je bezplatná zkušební verze, ale kompletní produkt hello není volné.</span><span class="sxs-lookup"><span data-stu-id="29b1c-108">"Trial" indicates that there is a free trial, but hello full product is not free.</span></span> <span data-ttu-id="29b1c-109">"Y/N" označuje, že verze je k dispozici zdarma, zatímco jiné verze je možné zakoupit.</span><span class="sxs-lookup"><span data-stu-id="29b1c-109">"Y/N" indicates that a version is available for free, while a different version is available for purchase.</span></span>

<span data-ttu-id="29b1c-110">Připravili jsme vám pouze snímek hello dostupných nástrojích klienta Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="29b1c-110">We've only provided a snapshot of hello available Azure Storage client tools.</span></span> <span data-ttu-id="29b1c-111">Tyto nástroje může pokračovat tooevolve a zvýší funkcí.</span><span class="sxs-lookup"><span data-stu-id="29b1c-111">These tools may continue tooevolve and grow in functionality.</span></span> <span data-ttu-id="29b1c-112">Pokud jsou opravy nebo aktualizace, nechejte prosím toolet komentáře, které nám vědět.</span><span class="sxs-lookup"><span data-stu-id="29b1c-112">If there are corrections or updates, please leave a comment toolet us know.</span></span> <span data-ttu-id="29b1c-113">Hello stejné je hodnota true, pokud víte, nástrojů, které by mělo být toobe Zde – jsme by radostí tooadd je.</span><span class="sxs-lookup"><span data-stu-id="29b1c-113">hello same is true if you know of tools that ought toobe here - we'd be happy tooadd them.</span></span>

<span data-ttu-id="29b1c-114">**Nástroje pro klienta úložiště Microsoft Azure**</span><span class="sxs-lookup"><span data-stu-id="29b1c-114">**Microsoft Azure Storage Client Tools**</span></span>

<table>
  <tr>
    <th rowspan="2"><span data-ttu-id="29b1c-115">Nástroj klienta úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="29b1c-115">Azure Storage Client Tool</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-116">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="29b1c-116">Block Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-117">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="29b1c-117">Page Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-118">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="29b1c-118">Append Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-119">Tabulky</span><span class="sxs-lookup"><span data-stu-id="29b1c-119">Tables</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-120">Fronty</span><span class="sxs-lookup"><span data-stu-id="29b1c-120">Queues</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-121">Soubory</span><span class="sxs-lookup"><span data-stu-id="29b1c-121">Files</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-122">Free</span><span class="sxs-lookup"><span data-stu-id="29b1c-122">Free</span></span></th>
    <th colspan="4"><span data-ttu-id="29b1c-123">Platforma</span><span class="sxs-lookup"><span data-stu-id="29b1c-123">Platform</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-124">Web</span><span class="sxs-lookup"><span data-stu-id="29b1c-124">Web</span></span></td>
    <td><span data-ttu-id="29b1c-125">Windows</span><span class="sxs-lookup"><span data-stu-id="29b1c-125">Windows</span></span></td>
    <td><span data-ttu-id="29b1c-126">OSX</span><span class="sxs-lookup"><span data-stu-id="29b1c-126">OSX</span></span></td>
    <td><span data-ttu-id="29b1c-127">Linux</span><span class="sxs-lookup"><span data-stu-id="29b1c-127">Linux</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-128"><a href="https://azure.microsoft.com/features/azure-portal/">Portál Microsoft Azure</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-128"><a href="https://azure.microsoft.com/features/azure-portal/">Microsoft Azure Portal</a></span></span></td>
    <td><span data-ttu-id="29b1c-129">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-129">X</span></span></td>
    <td><span data-ttu-id="29b1c-130">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-130">X</span></span></td>
    <td><span data-ttu-id="29b1c-131">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-131">X</span></span></td>
    <td><span data-ttu-id="29b1c-132">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-132">X</span></span></td>
    <td><span data-ttu-id="29b1c-133">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-133">X</span></span></td>
    <td><span data-ttu-id="29b1c-134">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-134">X</span></span></td>
    <td><span data-ttu-id="29b1c-135">Ano</span><span class="sxs-lookup"><span data-stu-id="29b1c-135">Y</span></span></td>
    <td><span data-ttu-id="29b1c-136">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-136">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-137"><a href="http://storageexplorer.com/">Microsoft Azure Storage Explorer</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-137"><a href="http://storageexplorer.com/">Microsoft Azure Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="29b1c-138">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-138">X</span></span></td>
    <td><span data-ttu-id="29b1c-139">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-139">X</span></span></td>
    <td><span data-ttu-id="29b1c-140">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-140">X</span></span></td>
    <td><span data-ttu-id="29b1c-141">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-141">X</span></span></td>
    <td><span data-ttu-id="29b1c-142">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-142">X</span></span></td>
    <td><span data-ttu-id="29b1c-143">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-143">X</span></span></td>
    <td><span data-ttu-id="29b1c-144">Ano</span><span class="sxs-lookup"><span data-stu-id="29b1c-144">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-145">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-145">X</span></span></td>
    <td><span data-ttu-id="29b1c-146">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-146">X</span></span></td>
    <td><span data-ttu-id="29b1c-147">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-147">X</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-148"><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">Průzkumník serveru Microsoft Visual Studio</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-148"><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">Microsoft Visual Studio Server Explorer</a></span></span></td>
    <td><span data-ttu-id="29b1c-149">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-149">X</span></span></td>
    <td><span data-ttu-id="29b1c-150">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-150">X</span></span></td>
    <td><span data-ttu-id="29b1c-151">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-151">X</span></span></td>
    <td><span data-ttu-id="29b1c-152">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-152">X</span></span></td>
    <td><span data-ttu-id="29b1c-153">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-153">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-154">Ano</span><span class="sxs-lookup"><span data-stu-id="29b1c-154">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-155">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-155">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
</table>

<span data-ttu-id="29b1c-156">**Úložiště Azure třetích stran klienta nástroje**</span><span class="sxs-lookup"><span data-stu-id="29b1c-156">**Third-Party Azure Storage Client Tools**</span></span>

<span data-ttu-id="29b1c-157">Nebyly jsme ověřit, že funkce hello nebo kvality nárokován hello nástroje třetích stran a jejich seznam neznamená neznamená společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="29b1c-157">We have not verified hello functionality or quality claimed by hello following third-party tools and their listing does not imply an endorsement by Microsoft.</span></span>

<table>
  <tr>
    <th rowspan="2"><span data-ttu-id="29b1c-158">Nástroj klienta úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="29b1c-158">Azure Storage Client Tool</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-159">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="29b1c-159">Block Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-160">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="29b1c-160">Page Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-161">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="29b1c-161">Append Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-162">Tabulky</span><span class="sxs-lookup"><span data-stu-id="29b1c-162">Tables</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-163">Fronty</span><span class="sxs-lookup"><span data-stu-id="29b1c-163">Queues</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-164">Soubory</span><span class="sxs-lookup"><span data-stu-id="29b1c-164">Files</span></span></th>
    <th rowspan="2"><span data-ttu-id="29b1c-165">Free</span><span class="sxs-lookup"><span data-stu-id="29b1c-165">Free</span></span></th>
    <th colspan="4"><span data-ttu-id="29b1c-166">Platforma</span><span class="sxs-lookup"><span data-stu-id="29b1c-166">Platform</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-167">Web</span><span class="sxs-lookup"><span data-stu-id="29b1c-167">Web</span></span></td>
    <td><span data-ttu-id="29b1c-168">Windows</span><span class="sxs-lookup"><span data-stu-id="29b1c-168">Windows</span></span></td>
    <td><span data-ttu-id="29b1c-169">OSX</span><span class="sxs-lookup"><span data-stu-id="29b1c-169">OSX</span></span></td>
    <td><span data-ttu-id="29b1c-170">Linux</span><span class="sxs-lookup"><span data-stu-id="29b1c-170">Linux</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-171"><a href="http://www.cloudportam.com/">Portam cloudu</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-171"><a href="http://www.cloudportam.com/">Cloud Portam</a></span></span></td>
    <td><span data-ttu-id="29b1c-172">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-172">X</span></span></td>
    <td><span data-ttu-id="29b1c-173">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-173">X</span></span></td>
    <td><span data-ttu-id="29b1c-174">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-174">X</span></span></td>
    <td><span data-ttu-id="29b1c-175">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-175">X</span></span></td>
    <td><span data-ttu-id="29b1c-176">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-176">X</span></span></td>
    <td><span data-ttu-id="29b1c-177">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-177">X</span></span></td>
    <td><span data-ttu-id="29b1c-178">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="29b1c-178">Trial</span></span></td>
    <td><span data-ttu-id="29b1c-179">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-179">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-180"><a href="http://www.cerebrata.com/products/azure-management-studio/introduction">Cerabrata: Azure Management Studio</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-180"><a href="http://www.cerebrata.com/products/azure-management-studio/introduction">Cerabrata: Azure Management Studio</a></span></span></td>
    <td><span data-ttu-id="29b1c-181">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-181">X</span></span></td>
    <td><span data-ttu-id="29b1c-182">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-182">X</span></span></td>
    <td><span data-ttu-id="29b1c-183">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-183">X</span></span></td>
    <td><span data-ttu-id="29b1c-184">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-184">X</span></span></td>
    <td><span data-ttu-id="29b1c-185">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-185">X</span></span></td>
    <td><span data-ttu-id="29b1c-186">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-186">X</span></span></td>
    <td><span data-ttu-id="29b1c-187">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="29b1c-187">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-188">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-188">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-189"><a href="http://www.cerebrata.com/products/azure-explorer/introduction">Cerabrata: Průzkumník Azure</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-189"><a href="http://www.cerebrata.com/products/azure-explorer/introduction">Cerabrata: Azure Explorer</a></span></span></td>
    <td><span data-ttu-id="29b1c-190">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-190">X</span></span></td>
    <td><span data-ttu-id="29b1c-191">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-191">X</span></span></td>
    <td><span data-ttu-id="29b1c-192">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-192">X</span></span></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="29b1c-193">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-193">X</span></span></td>
    <td><span data-ttu-id="29b1c-194">Ano</span><span class="sxs-lookup"><span data-stu-id="29b1c-194">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-195">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-195">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-196"><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Storage Explorer</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-196"><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="29b1c-197">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-197">X</span></span></td>
    <td><span data-ttu-id="29b1c-198">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-198">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-199">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-199">X</span></span></td>
    <td><span data-ttu-id="29b1c-200">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-200">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-201">Ano</span><span class="sxs-lookup"><span data-stu-id="29b1c-201">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-202">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-202">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-203"><a href="http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx">CloudBerry Explorer</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-203"><a href="http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx">CloudBerry Explorer</a></span></span></td>
    <td><span data-ttu-id="29b1c-204">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-204">X</span></span></td>
    <td><span data-ttu-id="29b1c-205">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-205">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="29b1c-206">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-206">X</span></span></td>
    <td><span data-ttu-id="29b1c-207">A/N</span><span class="sxs-lookup"><span data-stu-id="29b1c-207">Y/N</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-208">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-208">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-209"><a href="http://www.gapotchenko.com/cloudcombine">Combine cloudu</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-209"><a href="http://www.gapotchenko.com/cloudcombine">Cloud Combine</a></span></span></td>
    <td><span data-ttu-id="29b1c-210">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-210">X</span></span></td>
    <td><span data-ttu-id="29b1c-211">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-211">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-212">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-212">X</span></span></td>
    <td><span data-ttu-id="29b1c-213">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-213">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-214">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="29b1c-214">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-215">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-215">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-216"><a href="http://clumsyleaf.com">ClumsyLeaf: TableXplorer AzureXplorer, CloudXplorer,</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-216"><a href="http://clumsyleaf.com">ClumsyLeaf: AzureXplorer, CloudXplorer, TableXplorer</a></span></span></td>
    <td><span data-ttu-id="29b1c-217">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-217">X</span></span></td>
    <td><span data-ttu-id="29b1c-218">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-218">X</span></span></td>
    <td><span data-ttu-id="29b1c-219">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-219">X</span></span></td>
    <td><span data-ttu-id="29b1c-220">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-220">X</span></span></td>
    <td><span data-ttu-id="29b1c-221">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-221">X</span></span></td>
    <td><span data-ttu-id="29b1c-222">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-222">X</span></span></td>
    <td><span data-ttu-id="29b1c-223">Ano</span><span class="sxs-lookup"><span data-stu-id="29b1c-223">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-224">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-224">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-225"><a href="http://www.gladinet.com/Azure-Storage/index.htm">Gladinet cloudu</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-225"><a href="http://www.gladinet.com/Azure-Storage/index.htm">Gladinet Cloud</a></span></span></td>
    <td><span data-ttu-id="29b1c-226">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-226">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="29b1c-227">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="29b1c-227">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-228">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-228">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-229"><a href="http://storageexplorer.codeplex.com/">Službě Azure Web Storage Exploreru</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-229"><a href="http://storageexplorer.codeplex.com/">Azure Web Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="29b1c-230">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-230">X</span></span></td>
    <td><span data-ttu-id="29b1c-231">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-231">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-232">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-232">X</span></span></td>
    <td><span data-ttu-id="29b1c-233">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-233">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-234">Ano</span><span class="sxs-lookup"><span data-stu-id="29b1c-234">Y</span></span></td>
    <td><span data-ttu-id="29b1c-235">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-235">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="29b1c-236"><a href="https://zudio.co/">Zudio</a></span><span class="sxs-lookup"><span data-stu-id="29b1c-236"><a href="https://zudio.co/">Zudio</a></span></span></td>
    <td><span data-ttu-id="29b1c-237">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-237">X</span></span></td>
    <td><span data-ttu-id="29b1c-238">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-238">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="29b1c-239">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-239">X</span></span></td>
    <td><span data-ttu-id="29b1c-240">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-240">X</span></span></td>
    <td><span data-ttu-id="29b1c-241">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-241">X</span></span></td>
    <td><span data-ttu-id="29b1c-242">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="29b1c-242">Trial</span></span></td>
    <td><span data-ttu-id="29b1c-243">X</span><span class="sxs-lookup"><span data-stu-id="29b1c-243">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>
