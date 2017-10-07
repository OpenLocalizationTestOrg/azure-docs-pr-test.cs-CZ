---
title: "aaaUnderstand a vyřešte chyby WebHCat na HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak tooabout běžné chyby vrácené WebHCat v HDInsight a tooresolve je."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1b3d94b1-207d-4550-aece-21dc45485549
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 0071a1e9ed448ae146b93c8f4f518e31b95d27c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a><span data-ttu-id="da000-103">Rady pro pochopení a řešení chyb oznámených z WebHCat v HDInsight</span><span class="sxs-lookup"><span data-stu-id="da000-103">Understand and resolve errors received from WebHCat on HDInsight</span></span>

<span data-ttu-id="da000-104">Další informace o chyb oznámených při použití WebHCat s HDInsight a jak tooresolve je.</span><span class="sxs-lookup"><span data-stu-id="da000-104">Learn about errors received when using WebHCat with HDInsight, and how tooresolve them.</span></span> <span data-ttu-id="da000-105">WebHCat se používá interně pomocí klienta nástroje, jako je Azure PowerShell a hello nástrojů Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="da000-105">WebHCat is used internally by client-side tools such as Azure PowerShell and hello Data Lake Tools for Visual Studio.</span></span>

## <a name="what-is-webhcat"></a><span data-ttu-id="da000-106">Co je WebHCat</span><span class="sxs-lookup"><span data-stu-id="da000-106">What is WebHCat</span></span>

<span data-ttu-id="da000-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) je rozhraní REST API pro [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tabulka a vrstva správy úložiště pro Hadoop.</span><span class="sxs-lookup"><span data-stu-id="da000-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) is a REST API for [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), a table, and storage management layer for Hadoop.</span></span> <span data-ttu-id="da000-108">WebHCat je povoleno ve výchozím nastavení v clusterech prostředí HDInsight a používají různé úlohy toosubmit nástroje, bez přihlášení toohello clusteru můžete získat stav úlohy, atd.</span><span class="sxs-lookup"><span data-stu-id="da000-108">WebHCat is enabled by default on HDInsight clusters, and is used by various tools toosubmit jobs, get job status, etc. without logging in toohello cluster.</span></span>

## <a name="modifying-configuration"></a><span data-ttu-id="da000-109">Změny konfigurace</span><span class="sxs-lookup"><span data-stu-id="da000-109">Modifying configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="da000-110">Řadu hello chyby uvedené v tomto dokumentu dojít, protože byla překročena maximální nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="da000-110">Several of hello errors listed in this document occur because a configured maximum has been exceeded.</span></span> <span data-ttu-id="da000-111">Pokud krok řešení hello uvádí, že můžete změnit hodnotu, musí používat jednu z hello následující změnu tooperform hello:</span><span class="sxs-lookup"><span data-stu-id="da000-111">When hello resolution step mentions that you can change a value, you must use one of hello following tooperform hello change:</span></span>

* <span data-ttu-id="da000-112">Pro **Windows** clustery: použijte hodnotu hello tooconfigure akce skriptu při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="da000-112">For **Windows** clusters: Use a script action tooconfigure hello value during cluster creation.</span></span> <span data-ttu-id="da000-113">Další informace najdete v tématu [vývoj akcí skriptů](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="da000-113">For more information, see [Develop script actions](hdinsight-hadoop-script-actions.md).</span></span>

* <span data-ttu-id="da000-114">Pro **Linux** clustery: hodnota hello toomodify pomocí Ambari (web nebo REST API).</span><span class="sxs-lookup"><span data-stu-id="da000-114">For **Linux** clusters: Use Ambari (web or REST API) toomodify hello value.</span></span> <span data-ttu-id="da000-115">Další informace najdete v tématu [spravovat HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="da000-115">For more information, see [Manage HDInsight using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="da000-116">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="da000-116">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="da000-117">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="da000-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="default-configuration"></a><span data-ttu-id="da000-118">Výchozí konfigurace</span><span class="sxs-lookup"><span data-stu-id="da000-118">Default configuration</span></span>

<span data-ttu-id="da000-119">Pokud se překročí hello následující výchozí hodnoty, může snížit výkon WebHCat nebo způsobit chyby:</span><span class="sxs-lookup"><span data-stu-id="da000-119">If hello following default values are exceeded, it can degrade WebHCat performance or cause errors:</span></span>

| <span data-ttu-id="da000-120">Nastavení</span><span class="sxs-lookup"><span data-stu-id="da000-120">Setting</span></span> | <span data-ttu-id="da000-121">Výsledek</span><span class="sxs-lookup"><span data-stu-id="da000-121">What it does</span></span> | <span data-ttu-id="da000-122">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="da000-122">Default value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="da000-123">[yarn.Scheduler.Capacity.maximum – aplikace][maximum-applications]</span><span class="sxs-lookup"><span data-stu-id="da000-123">[yarn.scheduler.capacity.maximum-applications][maximum-applications]</span></span> |<span data-ttu-id="da000-124">maximální počet úloh, které mohou být souběžně aktivní Hello (čekající na vyřízení nebo spuštěné)</span><span class="sxs-lookup"><span data-stu-id="da000-124">hello maximum number of jobs that can be active concurrently (pending or running)</span></span> |<span data-ttu-id="da000-125">10 000</span><span class="sxs-lookup"><span data-stu-id="da000-125">10,000</span></span> |
| <span data-ttu-id="da000-126">[templeton.Exec.Max-procs][max-procs]</span><span class="sxs-lookup"><span data-stu-id="da000-126">[templeton.exec.max-procs][max-procs]</span></span> |<span data-ttu-id="da000-127">maximální počet požadavků, které lze zpracovat současně Hello</span><span class="sxs-lookup"><span data-stu-id="da000-127">hello maximum number of requests that can be served concurrently</span></span> |<span data-ttu-id="da000-128">20</span><span class="sxs-lookup"><span data-stu-id="da000-128">20</span></span> |
| <span data-ttu-id="da000-129">[mapreduce.jobhistory.Max stáří ms][max-age-ms]</span><span class="sxs-lookup"><span data-stu-id="da000-129">[mapreduce.jobhistory.max-age-ms][max-age-ms]</span></span> |<span data-ttu-id="da000-130">Hello počet dní, které úlohy historii jsou uchovány</span><span class="sxs-lookup"><span data-stu-id="da000-130">hello number of days that job history are retained</span></span> |<span data-ttu-id="da000-131">7 dní</span><span class="sxs-lookup"><span data-stu-id="da000-131">7 days</span></span> |

## <a name="too-many-requests"></a><span data-ttu-id="da000-132">Příliš mnoho požadavků</span><span class="sxs-lookup"><span data-stu-id="da000-132">Too many requests</span></span>

<span data-ttu-id="da000-133">**Kód stavu HTTP**: 429</span><span class="sxs-lookup"><span data-stu-id="da000-133">**HTTP Status code**: 429</span></span>

| <span data-ttu-id="da000-134">Příčina</span><span class="sxs-lookup"><span data-stu-id="da000-134">Cause</span></span> | <span data-ttu-id="da000-135">Řešení</span><span class="sxs-lookup"><span data-stu-id="da000-135">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="da000-136">Překročili jste maximální souběžných požadavků hello zpracovaných za minutu (výchozí 20) WebHCat</span><span class="sxs-lookup"><span data-stu-id="da000-136">You have exceeded hello maximum concurrent requests served by WebHCat per minute (default 20)</span></span> |<span data-ttu-id="da000-137">Snižte vaše zatížení tooensure, že nemáte odeslat více než hello maximální počet souběžných požadavků nebo zvyšte limit počtu souběžných požadavků hello úpravou `templeton.exec.max-procs`.</span><span class="sxs-lookup"><span data-stu-id="da000-137">Reduce your workload tooensure that you do not submit more than hello maximum number of concurrent requests or increase hello concurrent request limit by modifying `templeton.exec.max-procs`.</span></span> <span data-ttu-id="da000-138">Další informace najdete v tématu [Modifying konfigurace](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="da000-138">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |

## <a name="server-unavailable"></a><span data-ttu-id="da000-139">Server není dostupný.</span><span class="sxs-lookup"><span data-stu-id="da000-139">Server unavailable</span></span>

<span data-ttu-id="da000-140">**Kód stavu HTTP**: 503</span><span class="sxs-lookup"><span data-stu-id="da000-140">**HTTP Status code**: 503</span></span>

| <span data-ttu-id="da000-141">Příčina</span><span class="sxs-lookup"><span data-stu-id="da000-141">Cause</span></span> | <span data-ttu-id="da000-142">Řešení</span><span class="sxs-lookup"><span data-stu-id="da000-142">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="da000-143">Tento kód stavu obvykle dojde během převzetí služeb při selhání mezi hello primární a sekundární HeadNode pro hello cluster</span><span class="sxs-lookup"><span data-stu-id="da000-143">This status code usually occurs during failover between hello primary and secondary HeadNode for hello cluster</span></span> |<span data-ttu-id="da000-144">Počkejte 2 minuty a potom opakujte operaci hello</span><span class="sxs-lookup"><span data-stu-id="da000-144">Wait two minutes, then retry hello operation</span></span> |

## <a name="bad-request-content-could-not-find-job"></a><span data-ttu-id="da000-145">Chybný požadavek obsahu: Nelze najít úlohu</span><span class="sxs-lookup"><span data-stu-id="da000-145">Bad request Content: Could not find job</span></span>

<span data-ttu-id="da000-146">**Kód stavu HTTP**: 400</span><span class="sxs-lookup"><span data-stu-id="da000-146">**HTTP Status code**: 400</span></span>

| <span data-ttu-id="da000-147">Příčina</span><span class="sxs-lookup"><span data-stu-id="da000-147">Cause</span></span> | <span data-ttu-id="da000-148">Řešení</span><span class="sxs-lookup"><span data-stu-id="da000-148">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="da000-149">Podrobnosti úlohy byla vyčištěna podle historie úlohy hello čisticí</span><span class="sxs-lookup"><span data-stu-id="da000-149">Job details have been cleaned up by hello job history cleaner</span></span> |<span data-ttu-id="da000-150">Hello výchozí dobu uchování historie úlohy je 7 dní.</span><span class="sxs-lookup"><span data-stu-id="da000-150">hello default retention period for job history is 7 days.</span></span> <span data-ttu-id="da000-151">výchozí dobu uchování Hello lze změnit úpravou `mapreduce.jobhistory.max-age-ms`.</span><span class="sxs-lookup"><span data-stu-id="da000-151">hello default retention period can be changed by modifying `mapreduce.jobhistory.max-age-ms`.</span></span> <span data-ttu-id="da000-152">Další informace najdete v tématu [Modifying konfigurace](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="da000-152">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |
| <span data-ttu-id="da000-153">Úlohy byl ukončen z důvodu tooa převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="da000-153">Job has been killed due tooa failover</span></span> |<span data-ttu-id="da000-154">Opakujte odeslání úlohy pro až tootwo minut</span><span class="sxs-lookup"><span data-stu-id="da000-154">Retry job submission for up tootwo minutes</span></span> |
| <span data-ttu-id="da000-155">Neplatné id práce. byl použit.</span><span class="sxs-lookup"><span data-stu-id="da000-155">An Invalid job id was used</span></span> |<span data-ttu-id="da000-156">Kontrola, zda je správný hello id úlohy</span><span class="sxs-lookup"><span data-stu-id="da000-156">Check if hello job id is correct</span></span> |

## <a name="bad-gateway"></a><span data-ttu-id="da000-157">Chybná brána</span><span class="sxs-lookup"><span data-stu-id="da000-157">Bad gateway</span></span>

<span data-ttu-id="da000-158">**Kód stavu HTTP**: 502</span><span class="sxs-lookup"><span data-stu-id="da000-158">**HTTP Status code**: 502</span></span>

| <span data-ttu-id="da000-159">Příčina</span><span class="sxs-lookup"><span data-stu-id="da000-159">Cause</span></span> | <span data-ttu-id="da000-160">Řešení</span><span class="sxs-lookup"><span data-stu-id="da000-160">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="da000-161">Uvolňování paměti interní dochází v rámci hello proces WebHCat</span><span class="sxs-lookup"><span data-stu-id="da000-161">Internal garbage collection is occurring within hello WebHCat process</span></span> |<span data-ttu-id="da000-162">Počkejte toofinish kolekce paměti nebo restartujte službu WebHCat hello</span><span class="sxs-lookup"><span data-stu-id="da000-162">Wait for garbage collection toofinish or restart hello WebHCat service</span></span> |
| <span data-ttu-id="da000-163">Časový limit čekání na odpověď od hello ResourceManager služby.</span><span class="sxs-lookup"><span data-stu-id="da000-163">Time out waiting on a response from hello ResourceManager service.</span></span> <span data-ttu-id="da000-164">K této chybě může dojít, když hello počet aktivních aplikací přejde maximální hello nakonfigurovaný (výchozí 10 000)</span><span class="sxs-lookup"><span data-stu-id="da000-164">This error can occur when hello number of active applications goes hello configured maximum (default 10,000)</span></span> |<span data-ttu-id="da000-165">Počkejte aktuálně spuštěných úloh toocomplete nebo zvyšte limit souběžných úloh hello úpravou `yarn.scheduler.capacity.maximum-applications`.</span><span class="sxs-lookup"><span data-stu-id="da000-165">Wait for currently running jobs toocomplete or increase hello concurrent job limit by modifying `yarn.scheduler.capacity.maximum-applications`.</span></span> <span data-ttu-id="da000-166">Další informace najdete v tématu hello [Modifying konfigurace](#modifying-configuration) části.</span><span class="sxs-lookup"><span data-stu-id="da000-166">For more information, see hello [Modifying configuration](#modifying-configuration) section.</span></span> |
| <span data-ttu-id="da000-167">Probíhá pokus tooretrieve všechny úlohy prostřednictvím hello [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) volání při `Fields` je nastaven příliš`*`</span><span class="sxs-lookup"><span data-stu-id="da000-167">Attempting tooretrieve all jobs through hello [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) call while `Fields` is set too`*`</span></span> |<span data-ttu-id="da000-168">Nenačítat *všechny* podrobnosti úlohy.</span><span class="sxs-lookup"><span data-stu-id="da000-168">Do not retrieve *all* job details.</span></span> <span data-ttu-id="da000-169">Místo toho použít `jobid` tooretrieve podrobnosti úlohy pouze větší než id určité úlohy. Nebo, nepoužívejte`Fields`</span><span class="sxs-lookup"><span data-stu-id="da000-169">Instead use `jobid` tooretrieve details for jobs only greater than certain job id. Or, do not use `Fields`</span></span> |
| <span data-ttu-id="da000-170">Hello WebHCat služby je mimo provoz během převzetí služeb při selhání HeadNode</span><span class="sxs-lookup"><span data-stu-id="da000-170">hello WebHCat service is down during HeadNode failover</span></span> |<span data-ttu-id="da000-171">Počkejte dvou minut a opakujte operaci hello</span><span class="sxs-lookup"><span data-stu-id="da000-171">Wait for two minutes and retry hello operation</span></span> |
| <span data-ttu-id="da000-172">Existuje více než 500 čekající úlohy, odeslané prostřednictvím WebHCat</span><span class="sxs-lookup"><span data-stu-id="da000-172">There are more than 500 pending jobs submitted through WebHCat</span></span> |<span data-ttu-id="da000-173">Počkejte na dokončení aktuálně čeká na provedení úloh před odesláním další úlohy</span><span class="sxs-lookup"><span data-stu-id="da000-173">Wait until currently pending jobs have completed before submitting more jobs</span></span> |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
