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
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-storage-client-tools"></a><span data-ttu-id="97d0b-103">Klientské nástroje pro Azure Storage</span><span class="sxs-lookup"><span data-stu-id="97d0b-103">Azure Storage Client Tools</span></span>
<span data-ttu-id="97d0b-104">Uživatelé Azure Storage se často chtějí mít možnost k zobrazení nebo interakci s jejich daty pomocí klienta nástroje Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="97d0b-104">Users of Azure Storage frequently want to be able to view/interact with their data using an Azure Storage client tool.</span></span> <span data-ttu-id="97d0b-105">V následujících tabulkách jsme seznam několik nástrojů, které vám umožní to.</span><span class="sxs-lookup"><span data-stu-id="97d0b-105">In the tables below, we list a number of tools that allow you to do this.</span></span> <span data-ttu-id="97d0b-106">Můžeme uvést "X" v každém bloku pokud poskytuje možnost vytvořit výčet nebo přístup k abstrakci data.</span><span class="sxs-lookup"><span data-stu-id="97d0b-106">We put an "X" in each block if it provides the ability to either enumerate and/or access the data abstraction.</span></span> <span data-ttu-id="97d0b-107">V tabulce také ukazuje, zda je volné nástroje, či nikoliv.</span><span class="sxs-lookup"><span data-stu-id="97d0b-107">The table also shows if the tools is free or not.</span></span> <span data-ttu-id="97d0b-108">"Zkušební verze" znamená, že je bezplatná zkušební verze, ale kompletní produkt není volné.</span><span class="sxs-lookup"><span data-stu-id="97d0b-108">"Trial" indicates that there is a free trial, but the full product is not free.</span></span> <span data-ttu-id="97d0b-109">"Y/N" označuje, že verze je k dispozici zdarma, zatímco jiné verze je možné zakoupit.</span><span class="sxs-lookup"><span data-stu-id="97d0b-109">"Y/N" indicates that a version is available for free, while a different version is available for purchase.</span></span>

<span data-ttu-id="97d0b-110">Připravili jsme vám pouze snímek dostupných nástrojích klienta Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="97d0b-110">We've only provided a snapshot of the available Azure Storage client tools.</span></span> <span data-ttu-id="97d0b-111">Tyto nástroje mohou nadále můžete rozvíjet a rozšiřovat funkcí.</span><span class="sxs-lookup"><span data-stu-id="97d0b-111">These tools may continue to evolve and grow in functionality.</span></span> <span data-ttu-id="97d0b-112">Pokud jsou opravy nebo aktualizace, uveďte poznámky a dejte nám vědět.</span><span class="sxs-lookup"><span data-stu-id="97d0b-112">If there are corrections or updates, please leave a comment to let us know.</span></span> <span data-ttu-id="97d0b-113">Totéž platí, že pokud znáte nástrojů, které by měla být zde - rádi bychom je přidat.</span><span class="sxs-lookup"><span data-stu-id="97d0b-113">The same is true if you know of tools that ought to be here - we'd be happy to add them.</span></span>

<span data-ttu-id="97d0b-114">**Nástroje pro klienta úložiště Microsoft Azure**</span><span class="sxs-lookup"><span data-stu-id="97d0b-114">**Microsoft Azure Storage Client Tools**</span></span>

<table>
  <tr>
    <th rowspan="2"><span data-ttu-id="97d0b-115">Nástroj klienta úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="97d0b-115">Azure Storage Client Tool</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-116">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="97d0b-116">Block Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-117">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="97d0b-117">Page Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-118">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="97d0b-118">Append Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-119">Tabulky</span><span class="sxs-lookup"><span data-stu-id="97d0b-119">Tables</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-120">Fronty</span><span class="sxs-lookup"><span data-stu-id="97d0b-120">Queues</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-121">Soubory</span><span class="sxs-lookup"><span data-stu-id="97d0b-121">Files</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-122">Free</span><span class="sxs-lookup"><span data-stu-id="97d0b-122">Free</span></span></th>
    <th colspan="4"><span data-ttu-id="97d0b-123">Platforma</span><span class="sxs-lookup"><span data-stu-id="97d0b-123">Platform</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-124">Web</span><span class="sxs-lookup"><span data-stu-id="97d0b-124">Web</span></span></td>
    <td><span data-ttu-id="97d0b-125">Windows</span><span class="sxs-lookup"><span data-stu-id="97d0b-125">Windows</span></span></td>
    <td><span data-ttu-id="97d0b-126">OSX</span><span class="sxs-lookup"><span data-stu-id="97d0b-126">OSX</span></span></td>
    <td><span data-ttu-id="97d0b-127">Linux</span><span class="sxs-lookup"><span data-stu-id="97d0b-127">Linux</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-128"><a href="https://azure.microsoft.com/features/azure-portal/">Portál Microsoft Azure</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-128"><a href="https://azure.microsoft.com/features/azure-portal/">Microsoft Azure Portal</a></span></span></td>
    <td><span data-ttu-id="97d0b-129">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-129">X</span></span></td>
    <td><span data-ttu-id="97d0b-130">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-130">X</span></span></td>
    <td><span data-ttu-id="97d0b-131">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-131">X</span></span></td>
    <td><span data-ttu-id="97d0b-132">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-132">X</span></span></td>
    <td><span data-ttu-id="97d0b-133">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-133">X</span></span></td>
    <td><span data-ttu-id="97d0b-134">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-134">X</span></span></td>
    <td><span data-ttu-id="97d0b-135">Ano</span><span class="sxs-lookup"><span data-stu-id="97d0b-135">Y</span></span></td>
    <td><span data-ttu-id="97d0b-136">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-136">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-137"><a href="http://storageexplorer.com/">Microsoft Azure Storage Explorer</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-137"><a href="http://storageexplorer.com/">Microsoft Azure Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="97d0b-138">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-138">X</span></span></td>
    <td><span data-ttu-id="97d0b-139">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-139">X</span></span></td>
    <td><span data-ttu-id="97d0b-140">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-140">X</span></span></td>
    <td><span data-ttu-id="97d0b-141">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-141">X</span></span></td>
    <td><span data-ttu-id="97d0b-142">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-142">X</span></span></td>
    <td><span data-ttu-id="97d0b-143">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-143">X</span></span></td>
    <td><span data-ttu-id="97d0b-144">Ano</span><span class="sxs-lookup"><span data-stu-id="97d0b-144">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-145">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-145">X</span></span></td>
    <td><span data-ttu-id="97d0b-146">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-146">X</span></span></td>
    <td><span data-ttu-id="97d0b-147">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-147">X</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-148"><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">Průzkumník serveru Microsoft Visual Studio</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-148"><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">Microsoft Visual Studio Server Explorer</a></span></span></td>
    <td><span data-ttu-id="97d0b-149">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-149">X</span></span></td>
    <td><span data-ttu-id="97d0b-150">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-150">X</span></span></td>
    <td><span data-ttu-id="97d0b-151">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-151">X</span></span></td>
    <td><span data-ttu-id="97d0b-152">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-152">X</span></span></td>
    <td><span data-ttu-id="97d0b-153">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-153">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-154">Ano</span><span class="sxs-lookup"><span data-stu-id="97d0b-154">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-155">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-155">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
</table>

<span data-ttu-id="97d0b-156">**Úložiště Azure třetích stran klienta nástroje**</span><span class="sxs-lookup"><span data-stu-id="97d0b-156">**Third-Party Azure Storage Client Tools**</span></span>

<span data-ttu-id="97d0b-157">Jsme nebyly ověřeny funkce nebo kvality nárokován následující nástroje třetích stran a jejich seznam neznamená neznamená společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="97d0b-157">We have not verified the functionality or quality claimed by the following third-party tools and their listing does not imply an endorsement by Microsoft.</span></span>

<table>
  <tr>
    <th rowspan="2"><span data-ttu-id="97d0b-158">Nástroj klienta úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="97d0b-158">Azure Storage Client Tool</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-159">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="97d0b-159">Block Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-160">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="97d0b-160">Page Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-161">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="97d0b-161">Append Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-162">Tabulky</span><span class="sxs-lookup"><span data-stu-id="97d0b-162">Tables</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-163">Fronty</span><span class="sxs-lookup"><span data-stu-id="97d0b-163">Queues</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-164">Soubory</span><span class="sxs-lookup"><span data-stu-id="97d0b-164">Files</span></span></th>
    <th rowspan="2"><span data-ttu-id="97d0b-165">Free</span><span class="sxs-lookup"><span data-stu-id="97d0b-165">Free</span></span></th>
    <th colspan="4"><span data-ttu-id="97d0b-166">Platforma</span><span class="sxs-lookup"><span data-stu-id="97d0b-166">Platform</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-167">Web</span><span class="sxs-lookup"><span data-stu-id="97d0b-167">Web</span></span></td>
    <td><span data-ttu-id="97d0b-168">Windows</span><span class="sxs-lookup"><span data-stu-id="97d0b-168">Windows</span></span></td>
    <td><span data-ttu-id="97d0b-169">OSX</span><span class="sxs-lookup"><span data-stu-id="97d0b-169">OSX</span></span></td>
    <td><span data-ttu-id="97d0b-170">Linux</span><span class="sxs-lookup"><span data-stu-id="97d0b-170">Linux</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-171"><a href="http://www.cloudportam.com/">Portam cloudu</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-171"><a href="http://www.cloudportam.com/">Cloud Portam</a></span></span></td>
    <td><span data-ttu-id="97d0b-172">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-172">X</span></span></td>
    <td><span data-ttu-id="97d0b-173">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-173">X</span></span></td>
    <td><span data-ttu-id="97d0b-174">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-174">X</span></span></td>
    <td><span data-ttu-id="97d0b-175">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-175">X</span></span></td>
    <td><span data-ttu-id="97d0b-176">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-176">X</span></span></td>
    <td><span data-ttu-id="97d0b-177">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-177">X</span></span></td>
    <td><span data-ttu-id="97d0b-178">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="97d0b-178">Trial</span></span></td>
    <td><span data-ttu-id="97d0b-179">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-179">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-180"><a href="http://www.cerebrata.com/products/azure-management-studio/introduction">Cerabrata: Azure Management Studio</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-180"><a href="http://www.cerebrata.com/products/azure-management-studio/introduction">Cerabrata: Azure Management Studio</a></span></span></td>
    <td><span data-ttu-id="97d0b-181">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-181">X</span></span></td>
    <td><span data-ttu-id="97d0b-182">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-182">X</span></span></td>
    <td><span data-ttu-id="97d0b-183">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-183">X</span></span></td>
    <td><span data-ttu-id="97d0b-184">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-184">X</span></span></td>
    <td><span data-ttu-id="97d0b-185">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-185">X</span></span></td>
    <td><span data-ttu-id="97d0b-186">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-186">X</span></span></td>
    <td><span data-ttu-id="97d0b-187">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="97d0b-187">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-188">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-188">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-189"><a href="http://www.cerebrata.com/products/azure-explorer/introduction">Cerabrata: Průzkumník Azure</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-189"><a href="http://www.cerebrata.com/products/azure-explorer/introduction">Cerabrata: Azure Explorer</a></span></span></td>
    <td><span data-ttu-id="97d0b-190">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-190">X</span></span></td>
    <td><span data-ttu-id="97d0b-191">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-191">X</span></span></td>
    <td><span data-ttu-id="97d0b-192">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-192">X</span></span></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="97d0b-193">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-193">X</span></span></td>
    <td><span data-ttu-id="97d0b-194">Ano</span><span class="sxs-lookup"><span data-stu-id="97d0b-194">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-195">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-195">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-196"><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Storage Explorer</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-196"><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="97d0b-197">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-197">X</span></span></td>
    <td><span data-ttu-id="97d0b-198">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-198">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-199">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-199">X</span></span></td>
    <td><span data-ttu-id="97d0b-200">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-200">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-201">Ano</span><span class="sxs-lookup"><span data-stu-id="97d0b-201">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-202">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-202">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-203"><a href="http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx">CloudBerry Explorer</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-203"><a href="http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx">CloudBerry Explorer</a></span></span></td>
    <td><span data-ttu-id="97d0b-204">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-204">X</span></span></td>
    <td><span data-ttu-id="97d0b-205">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-205">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="97d0b-206">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-206">X</span></span></td>
    <td><span data-ttu-id="97d0b-207">A/N</span><span class="sxs-lookup"><span data-stu-id="97d0b-207">Y/N</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-208">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-208">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-209"><a href="http://www.gapotchenko.com/cloudcombine">Combine cloudu</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-209"><a href="http://www.gapotchenko.com/cloudcombine">Cloud Combine</a></span></span></td>
    <td><span data-ttu-id="97d0b-210">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-210">X</span></span></td>
    <td><span data-ttu-id="97d0b-211">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-211">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-212">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-212">X</span></span></td>
    <td><span data-ttu-id="97d0b-213">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-213">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-214">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="97d0b-214">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-215">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-215">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-216"><a href="http://clumsyleaf.com">ClumsyLeaf: TableXplorer AzureXplorer, CloudXplorer,</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-216"><a href="http://clumsyleaf.com">ClumsyLeaf: AzureXplorer, CloudXplorer, TableXplorer</a></span></span></td>
    <td><span data-ttu-id="97d0b-217">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-217">X</span></span></td>
    <td><span data-ttu-id="97d0b-218">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-218">X</span></span></td>
    <td><span data-ttu-id="97d0b-219">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-219">X</span></span></td>
    <td><span data-ttu-id="97d0b-220">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-220">X</span></span></td>
    <td><span data-ttu-id="97d0b-221">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-221">X</span></span></td>
    <td><span data-ttu-id="97d0b-222">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-222">X</span></span></td>
    <td><span data-ttu-id="97d0b-223">Ano</span><span class="sxs-lookup"><span data-stu-id="97d0b-223">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-224">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-224">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-225"><a href="http://www.gladinet.com/Azure-Storage/index.htm">Gladinet cloudu</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-225"><a href="http://www.gladinet.com/Azure-Storage/index.htm">Gladinet Cloud</a></span></span></td>
    <td><span data-ttu-id="97d0b-226">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-226">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="97d0b-227">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="97d0b-227">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-228">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-228">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-229"><a href="http://storageexplorer.codeplex.com/">Službě Azure Web Storage Exploreru</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-229"><a href="http://storageexplorer.codeplex.com/">Azure Web Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="97d0b-230">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-230">X</span></span></td>
    <td><span data-ttu-id="97d0b-231">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-231">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-232">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-232">X</span></span></td>
    <td><span data-ttu-id="97d0b-233">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-233">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-234">Ano</span><span class="sxs-lookup"><span data-stu-id="97d0b-234">Y</span></span></td>
    <td><span data-ttu-id="97d0b-235">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-235">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="97d0b-236"><a href="https://zudio.co/">Zudio</a></span><span class="sxs-lookup"><span data-stu-id="97d0b-236"><a href="https://zudio.co/">Zudio</a></span></span></td>
    <td><span data-ttu-id="97d0b-237">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-237">X</span></span></td>
    <td><span data-ttu-id="97d0b-238">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-238">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="97d0b-239">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-239">X</span></span></td>
    <td><span data-ttu-id="97d0b-240">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-240">X</span></span></td>
    <td><span data-ttu-id="97d0b-241">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-241">X</span></span></td>
    <td><span data-ttu-id="97d0b-242">Zkušební verze</span><span class="sxs-lookup"><span data-stu-id="97d0b-242">Trial</span></span></td>
    <td><span data-ttu-id="97d0b-243">X</span><span class="sxs-lookup"><span data-stu-id="97d0b-243">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>
