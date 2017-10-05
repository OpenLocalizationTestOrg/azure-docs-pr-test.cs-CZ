---
title: "Nástroje pro práci s Azure Storage | Microsoft Docs"
description: "Seznam nástrojů, které vám umožní zobrazit nebo práci s daty Azure Storage."
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
ms.openlocfilehash: 620efda06d8225b21b6bb9b104b79061ebb6515c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-client-tools"></a><span data-ttu-id="539a7-103">Klientské nástroje pro Azure Storage</span><span class="sxs-lookup"><span data-stu-id="539a7-103">Azure Storage Client Tools</span></span>
<span data-ttu-id="539a7-104">Uživatelé Azure Storage se často chtějí mít možnost k zobrazení nebo interakci s jejich daty pomocí klienta nástroje Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="539a7-104">Users of Azure Storage frequently want to be able to view/interact with their data using an Azure Storage client tool.</span></span> <span data-ttu-id="539a7-105">V následujících tabulkách jsme seznam několik nástrojů, které vám umožní to.</span><span class="sxs-lookup"><span data-stu-id="539a7-105">In the tables below, we list a number of tools that allow you to do this.</span></span> <span data-ttu-id="539a7-106">Můžeme uvést "X" v každém bloku pokud poskytuje možnost vytvořit výčet nebo přístup k abstrakci data.</span><span class="sxs-lookup"><span data-stu-id="539a7-106">We put an "X" in each block if it provides the ability to either enumerate and/or access the data abstraction.</span></span> <span data-ttu-id="539a7-107">V tabulce také ukazuje, zda je volné nástroje, či nikoliv.</span><span class="sxs-lookup"><span data-stu-id="539a7-107">The table also shows if the tools is free or not.</span></span> <span data-ttu-id="539a7-108">"Zkušební verze" znamená, že je bezplatná zkušební verze, ale kompletní produkt není volné.</span><span class="sxs-lookup"><span data-stu-id="539a7-108">"Trial" indicates that there is a free trial, but the full product is not free.</span></span> <span data-ttu-id="539a7-109">"Y/N" označuje, že verze je k dispozici zdarma, zatímco jiné verze je možné zakoupit.</span><span class="sxs-lookup"><span data-stu-id="539a7-109">"Y/N" indicates that a version is available for free, while a different version is available for purchase.</span></span>

<span data-ttu-id="539a7-110">Připravili jsme vám pouze snímek dostupných nástrojích klienta Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="539a7-110">We've only provided a snapshot of the available Azure Storage client tools.</span></span> <span data-ttu-id="539a7-111">Tyto nástroje mohou nadále můžete rozvíjet a rozšiřovat funkcí.</span><span class="sxs-lookup"><span data-stu-id="539a7-111">These tools may continue to evolve and grow in functionality.</span></span> <span data-ttu-id="539a7-112">Pokud jsou opravy nebo aktualizace, uveďte poznámky a dejte nám vědět.</span><span class="sxs-lookup"><span data-stu-id="539a7-112">If there are corrections or updates, please leave a comment to let us know.</span></span> <span data-ttu-id="539a7-113">Totéž platí, že pokud znáte nástrojů, které by měla být zde - rádi bychom je přidat.</span><span class="sxs-lookup"><span data-stu-id="539a7-113">The same is true if you know of tools that ought to be here - we'd be happy to add them.</span></span>

<span data-ttu-id="539a7-114">**Nástroje pro klienta úložiště Microsoft Azure**</span><span class="sxs-lookup"><span data-stu-id="539a7-114">**Microsoft Azure Storage Client Tools**</span></span>

<table>
  <tr>
    <th rowspan="2"><span data-ttu-id="539a7-115">Nástroj klienta úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="539a7-115">Azure Storage Client Tool</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-116">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="539a7-116">Block Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-117">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="539a7-117">Page Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-118">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="539a7-118">Append Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-119">Tabulky</span><span class="sxs-lookup"><span data-stu-id="539a7-119">Tables</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-120">Fronty</span><span class="sxs-lookup"><span data-stu-id="539a7-120">Queues</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-121">Soubory</span><span class="sxs-lookup"><span data-stu-id="539a7-121">Files</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-122">Free</span><span class="sxs-lookup"><span data-stu-id="539a7-122">Free</span></span></th>
    <th colspan="4"><span data-ttu-id="539a7-123">Platforma</span><span class="sxs-lookup"><span data-stu-id="539a7-123">Platform</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-124">Web</span><span class="sxs-lookup"><span data-stu-id="539a7-124">Web</span></span></td>
    <td><span data-ttu-id="539a7-125">Windows</span><span class="sxs-lookup"><span data-stu-id="539a7-125">Windows</span></span></td>
    <td><span data-ttu-id="539a7-126">OSX</span><span class="sxs-lookup"><span data-stu-id="539a7-126">OSX</span></span></td>
    <td><span data-ttu-id="539a7-127">Linux</span><span class="sxs-lookup"><span data-stu-id="539a7-127">Linux</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-128"><a href="https://azure.microsoft.com/features/azure-portal/">Portál Microsoft Azure</a></span><span class="sxs-lookup"><span data-stu-id="539a7-128"><a href="https://azure.microsoft.com/features/azure-portal/">Microsoft Azure Portal</a></span></span></td>
    <td><span data-ttu-id="539a7-129">X</span><span class="sxs-lookup"><span data-stu-id="539a7-129">X</span></span></td>
    <td><span data-ttu-id="539a7-130">X</span><span class="sxs-lookup"><span data-stu-id="539a7-130">X</span></span></td>
    <td><span data-ttu-id="539a7-131">X</span><span class="sxs-lookup"><span data-stu-id="539a7-131">X</span></span></td>
    <td><span data-ttu-id="539a7-132">X</span><span class="sxs-lookup"><span data-stu-id="539a7-132">X</span></span></td>
    <td><span data-ttu-id="539a7-133">X</span><span class="sxs-lookup"><span data-stu-id="539a7-133">X</span></span></td>
    <td><span data-ttu-id="539a7-134">X</span><span class="sxs-lookup"><span data-stu-id="539a7-134">X</span></span></td>
    <td><span data-ttu-id="539a7-135">Ano</span><span class="sxs-lookup"><span data-stu-id="539a7-135">Y</span></span></td>
    <td><span data-ttu-id="539a7-136">X</span><span class="sxs-lookup"><span data-stu-id="539a7-136">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-137"><a href="http://storageexplorer.com/">Microsoft Azure Storage Explorer</a></span><span class="sxs-lookup"><span data-stu-id="539a7-137"><a href="http://storageexplorer.com/">Microsoft Azure Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="539a7-138">X</span><span class="sxs-lookup"><span data-stu-id="539a7-138">X</span></span></td>
    <td><span data-ttu-id="539a7-139">X</span><span class="sxs-lookup"><span data-stu-id="539a7-139">X</span></span></td>
    <td><span data-ttu-id="539a7-140">X</span><span class="sxs-lookup"><span data-stu-id="539a7-140">X</span></span></td>
    <td><span data-ttu-id="539a7-141">X</span><span class="sxs-lookup"><span data-stu-id="539a7-141">X</span></span></td>
    <td><span data-ttu-id="539a7-142">X</span><span class="sxs-lookup"><span data-stu-id="539a7-142">X</span></span></td>
    <td><span data-ttu-id="539a7-143">X</span><span class="sxs-lookup"><span data-stu-id="539a7-143">X</span></span></td>
    <td><span data-ttu-id="539a7-144">Ano</span><span class="sxs-lookup"><span data-stu-id="539a7-144">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-145">X</span><span class="sxs-lookup"><span data-stu-id="539a7-145">X</span></span></td>
    <td><span data-ttu-id="539a7-146">X</span><span class="sxs-lookup"><span data-stu-id="539a7-146">X</span></span></td>
    <td><span data-ttu-id="539a7-147">X</span><span class="sxs-lookup"><span data-stu-id="539a7-147">X</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-148"><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">Průzkumník serveru Microsoft Visual Studio</a></span><span class="sxs-lookup"><span data-stu-id="539a7-148"><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">Microsoft Visual Studio Server Explorer</a></span></span></td>
    <td><span data-ttu-id="539a7-149">X</span><span class="sxs-lookup"><span data-stu-id="539a7-149">X</span></span></td>
    <td><span data-ttu-id="539a7-150">X</span><span class="sxs-lookup"><span data-stu-id="539a7-150">X</span></span></td>
    <td><span data-ttu-id="539a7-151">X</span><span class="sxs-lookup"><span data-stu-id="539a7-151">X</span></span></td>
    <td><span data-ttu-id="539a7-152">X</span><span class="sxs-lookup"><span data-stu-id="539a7-152">X</span></span></td>
    <td><span data-ttu-id="539a7-153">X</span><span class="sxs-lookup"><span data-stu-id="539a7-153">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-154">Ano</span><span class="sxs-lookup"><span data-stu-id="539a7-154">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-155">X</span><span class="sxs-lookup"><span data-stu-id="539a7-155">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
</table>

<span data-ttu-id="539a7-156">**Úložiště Azure třetích stran klienta nástroje**</span><span class="sxs-lookup"><span data-stu-id="539a7-156">**Third-Party Azure Storage Client Tools**</span></span>

<span data-ttu-id="539a7-157">Jsme nebyly ověřeny funkce nebo kvality nárokován následující nástroje třetích stran a jejich seznam neznamená neznamená společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="539a7-157">We have not verified the functionality or quality claimed by the following third-party tools and their listing does not imply an endorsement by Microsoft.</span></span>

<table>
  <tr>
    <th rowspan="2"><span data-ttu-id="539a7-158">Nástroj klienta úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="539a7-158">Azure Storage Client Tool</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-159">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="539a7-159">Block Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-160">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="539a7-160">Page Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-161">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="539a7-161">Append Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-162">Tabulky</span><span class="sxs-lookup"><span data-stu-id="539a7-162">Tables</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-163">Fronty</span><span class="sxs-lookup"><span data-stu-id="539a7-163">Queues</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-164">Soubory</span><span class="sxs-lookup"><span data-stu-id="539a7-164">Files</span></span></th>
    <th rowspan="2"><span data-ttu-id="539a7-165">Free</span><span class="sxs-lookup"><span data-stu-id="539a7-165">Free</span></span></th>
    <th colspan="4"><span data-ttu-id="539a7-166">Platforma</span><span class="sxs-lookup"><span data-stu-id="539a7-166">Platform</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-167">Web</span><span class="sxs-lookup"><span data-stu-id="539a7-167">Web</span></span></td>
    <td><span data-ttu-id="539a7-168">Windows</span><span class="sxs-lookup"><span data-stu-id="539a7-168">Windows</span></span></td>
    <td><span data-ttu-id="539a7-169">OSX</span><span class="sxs-lookup"><span data-stu-id="539a7-169">OSX</span></span></td>
    <td><span data-ttu-id="539a7-170">Linux</span><span class="sxs-lookup"><span data-stu-id="539a7-170">Linux</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-171"><a href="http://www.cloudportam.com/">Portam cloudu</a></span><span class="sxs-lookup"><span data-stu-id="539a7-171"><a href="http://www.cloudportam.com/">Cloud Portam</a></span></span></td>
    <td><span data-ttu-id="539a7-172">X</span><span class="sxs-lookup"><span data-stu-id="539a7-172">X</span></span></td>
    <td><span data-ttu-id="539a7-173">X</span><span class="sxs-lookup"><span data-stu-id="539a7-173">X</span></span></td>
    <td><span data-ttu-id="539a7-174">X</span><span class="sxs-lookup"><span data-stu-id="539a7-174">X</span></span></td>
    <td><span data-ttu-id="539a7-175">X</span><span class="sxs-lookup"><span data-stu-id="539a7-175">X</span></span></td>
    <td><span data-ttu-id="539a7-176">X</span><span class="sxs-lookup"><span data-stu-id="539a7-176">X</span></span></td>
    <td><span data-ttu-id="539a7-177">X</span><span class="sxs-lookup"><span data-stu-id="539a7-177">X</span></span></td>
    <td><span data-ttu-id="539a7-178">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="539a7-178">Trial</span></span></td>
    <td><span data-ttu-id="539a7-179">X</span><span class="sxs-lookup"><span data-stu-id="539a7-179">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-180"><a href="http://www.cerebrata.com/products/azure-management-studio/introduction">Cerabrata: Azure Management Studio</a></span><span class="sxs-lookup"><span data-stu-id="539a7-180"><a href="http://www.cerebrata.com/products/azure-management-studio/introduction">Cerabrata: Azure Management Studio</a></span></span></td>
    <td><span data-ttu-id="539a7-181">X</span><span class="sxs-lookup"><span data-stu-id="539a7-181">X</span></span></td>
    <td><span data-ttu-id="539a7-182">X</span><span class="sxs-lookup"><span data-stu-id="539a7-182">X</span></span></td>
    <td><span data-ttu-id="539a7-183">X</span><span class="sxs-lookup"><span data-stu-id="539a7-183">X</span></span></td>
    <td><span data-ttu-id="539a7-184">X</span><span class="sxs-lookup"><span data-stu-id="539a7-184">X</span></span></td>
    <td><span data-ttu-id="539a7-185">X</span><span class="sxs-lookup"><span data-stu-id="539a7-185">X</span></span></td>
    <td><span data-ttu-id="539a7-186">X</span><span class="sxs-lookup"><span data-stu-id="539a7-186">X</span></span></td>
    <td><span data-ttu-id="539a7-187">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="539a7-187">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-188">X</span><span class="sxs-lookup"><span data-stu-id="539a7-188">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-189"><a href="http://www.cerebrata.com/products/azure-explorer/introduction">Cerabrata: Průzkumník Azure</a></span><span class="sxs-lookup"><span data-stu-id="539a7-189"><a href="http://www.cerebrata.com/products/azure-explorer/introduction">Cerabrata: Azure Explorer</a></span></span></td>
    <td><span data-ttu-id="539a7-190">X</span><span class="sxs-lookup"><span data-stu-id="539a7-190">X</span></span></td>
    <td><span data-ttu-id="539a7-191">X</span><span class="sxs-lookup"><span data-stu-id="539a7-191">X</span></span></td>
    <td><span data-ttu-id="539a7-192">X</span><span class="sxs-lookup"><span data-stu-id="539a7-192">X</span></span></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="539a7-193">X</span><span class="sxs-lookup"><span data-stu-id="539a7-193">X</span></span></td>
    <td><span data-ttu-id="539a7-194">Ano</span><span class="sxs-lookup"><span data-stu-id="539a7-194">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-195">X</span><span class="sxs-lookup"><span data-stu-id="539a7-195">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-196"><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Storage Explorer</a></span><span class="sxs-lookup"><span data-stu-id="539a7-196"><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="539a7-197">X</span><span class="sxs-lookup"><span data-stu-id="539a7-197">X</span></span></td>
    <td><span data-ttu-id="539a7-198">X</span><span class="sxs-lookup"><span data-stu-id="539a7-198">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-199">X</span><span class="sxs-lookup"><span data-stu-id="539a7-199">X</span></span></td>
    <td><span data-ttu-id="539a7-200">X</span><span class="sxs-lookup"><span data-stu-id="539a7-200">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-201">Ano</span><span class="sxs-lookup"><span data-stu-id="539a7-201">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-202">X</span><span class="sxs-lookup"><span data-stu-id="539a7-202">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-203"><a href="http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx">CloudBerry Explorer</a></span><span class="sxs-lookup"><span data-stu-id="539a7-203"><a href="http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx">CloudBerry Explorer</a></span></span></td>
    <td><span data-ttu-id="539a7-204">X</span><span class="sxs-lookup"><span data-stu-id="539a7-204">X</span></span></td>
    <td><span data-ttu-id="539a7-205">X</span><span class="sxs-lookup"><span data-stu-id="539a7-205">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="539a7-206">X</span><span class="sxs-lookup"><span data-stu-id="539a7-206">X</span></span></td>
    <td><span data-ttu-id="539a7-207">A/N</span><span class="sxs-lookup"><span data-stu-id="539a7-207">Y/N</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-208">X</span><span class="sxs-lookup"><span data-stu-id="539a7-208">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-209"><a href="http://www.gapotchenko.com/cloudcombine">Combine cloudu</a></span><span class="sxs-lookup"><span data-stu-id="539a7-209"><a href="http://www.gapotchenko.com/cloudcombine">Cloud Combine</a></span></span></td>
    <td><span data-ttu-id="539a7-210">X</span><span class="sxs-lookup"><span data-stu-id="539a7-210">X</span></span></td>
    <td><span data-ttu-id="539a7-211">X</span><span class="sxs-lookup"><span data-stu-id="539a7-211">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-212">X</span><span class="sxs-lookup"><span data-stu-id="539a7-212">X</span></span></td>
    <td><span data-ttu-id="539a7-213">X</span><span class="sxs-lookup"><span data-stu-id="539a7-213">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-214">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="539a7-214">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-215">X</span><span class="sxs-lookup"><span data-stu-id="539a7-215">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-216"><a href="http://clumsyleaf.com">ClumsyLeaf: TableXplorer AzureXplorer, CloudXplorer,</a></span><span class="sxs-lookup"><span data-stu-id="539a7-216"><a href="http://clumsyleaf.com">ClumsyLeaf: AzureXplorer, CloudXplorer, TableXplorer</a></span></span></td>
    <td><span data-ttu-id="539a7-217">X</span><span class="sxs-lookup"><span data-stu-id="539a7-217">X</span></span></td>
    <td><span data-ttu-id="539a7-218">X</span><span class="sxs-lookup"><span data-stu-id="539a7-218">X</span></span></td>
    <td><span data-ttu-id="539a7-219">X</span><span class="sxs-lookup"><span data-stu-id="539a7-219">X</span></span></td>
    <td><span data-ttu-id="539a7-220">X</span><span class="sxs-lookup"><span data-stu-id="539a7-220">X</span></span></td>
    <td><span data-ttu-id="539a7-221">X</span><span class="sxs-lookup"><span data-stu-id="539a7-221">X</span></span></td>
    <td><span data-ttu-id="539a7-222">X</span><span class="sxs-lookup"><span data-stu-id="539a7-222">X</span></span></td>
    <td><span data-ttu-id="539a7-223">Ano</span><span class="sxs-lookup"><span data-stu-id="539a7-223">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-224">X</span><span class="sxs-lookup"><span data-stu-id="539a7-224">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-225"><a href="http://www.gladinet.com/Azure-Storage/index.htm">Gladinet cloudu</a></span><span class="sxs-lookup"><span data-stu-id="539a7-225"><a href="http://www.gladinet.com/Azure-Storage/index.htm">Gladinet Cloud</a></span></span></td>
    <td><span data-ttu-id="539a7-226">X</span><span class="sxs-lookup"><span data-stu-id="539a7-226">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="539a7-227">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="539a7-227">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-228">X</span><span class="sxs-lookup"><span data-stu-id="539a7-228">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-229"><a href="http://storageexplorer.codeplex.com/">Službě Azure Web Storage Exploreru</a></span><span class="sxs-lookup"><span data-stu-id="539a7-229"><a href="http://storageexplorer.codeplex.com/">Azure Web Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="539a7-230">X</span><span class="sxs-lookup"><span data-stu-id="539a7-230">X</span></span></td>
    <td><span data-ttu-id="539a7-231">X</span><span class="sxs-lookup"><span data-stu-id="539a7-231">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-232">X</span><span class="sxs-lookup"><span data-stu-id="539a7-232">X</span></span></td>
    <td><span data-ttu-id="539a7-233">X</span><span class="sxs-lookup"><span data-stu-id="539a7-233">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-234">Ano</span><span class="sxs-lookup"><span data-stu-id="539a7-234">Y</span></span></td>
    <td><span data-ttu-id="539a7-235">X</span><span class="sxs-lookup"><span data-stu-id="539a7-235">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="539a7-236"><a href="https://zudio.co/">Zudio</a></span><span class="sxs-lookup"><span data-stu-id="539a7-236"><a href="https://zudio.co/">Zudio</a></span></span></td>
    <td><span data-ttu-id="539a7-237">X</span><span class="sxs-lookup"><span data-stu-id="539a7-237">X</span></span></td>
    <td><span data-ttu-id="539a7-238">X</span><span class="sxs-lookup"><span data-stu-id="539a7-238">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="539a7-239">X</span><span class="sxs-lookup"><span data-stu-id="539a7-239">X</span></span></td>
    <td><span data-ttu-id="539a7-240">X</span><span class="sxs-lookup"><span data-stu-id="539a7-240">X</span></span></td>
    <td><span data-ttu-id="539a7-241">X</span><span class="sxs-lookup"><span data-stu-id="539a7-241">X</span></span></td>
    <td><span data-ttu-id="539a7-242">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="539a7-242">Trial</span></span></td>
    <td><span data-ttu-id="539a7-243">X</span><span class="sxs-lookup"><span data-stu-id="539a7-243">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>
