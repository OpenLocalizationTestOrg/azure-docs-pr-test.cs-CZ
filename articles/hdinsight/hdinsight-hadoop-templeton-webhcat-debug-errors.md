---
title: "Rady pro pochopení a řešení chyb WebHCat v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak k o běžné chyby vrácené WebHCat v HDInsight a způsob jejich řešení."
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
ms.openlocfilehash: 6d8162e0d64ec9fc42690392b7c822593c0c2767
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a><span data-ttu-id="a46e4-103">Rady pro pochopení a řešení chyb oznámených z WebHCat v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a46e4-103">Understand and resolve errors received from WebHCat on HDInsight</span></span>

<span data-ttu-id="a46e4-104">Další informace o chyb oznámených při použití WebHCat s HDInsight a způsob jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="a46e4-104">Learn about errors received when using WebHCat with HDInsight, and how to resolve them.</span></span> <span data-ttu-id="a46e4-105">WebHCat se používá interně pomocí klienta nástroje, jako je Azure PowerShell a nástrojů Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a46e4-105">WebHCat is used internally by client-side tools such as Azure PowerShell and the Data Lake Tools for Visual Studio.</span></span>

## <a name="what-is-webhcat"></a><span data-ttu-id="a46e4-106">Co je WebHCat</span><span class="sxs-lookup"><span data-stu-id="a46e4-106">What is WebHCat</span></span>

<span data-ttu-id="a46e4-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) je rozhraní REST API pro [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tabulka a vrstva správy úložiště pro Hadoop.</span><span class="sxs-lookup"><span data-stu-id="a46e4-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) is a REST API for [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), a table, and storage management layer for Hadoop.</span></span> <span data-ttu-id="a46e4-108">WebHCat je povoleno ve výchozím nastavení v clusterech prostředí HDInsight a různé nástroje používá k odesílání úloh, bez přihlášení do clusteru můžete získat stav úlohy, atd.</span><span class="sxs-lookup"><span data-stu-id="a46e4-108">WebHCat is enabled by default on HDInsight clusters, and is used by various tools to submit jobs, get job status, etc. without logging in to the cluster.</span></span>

## <a name="modifying-configuration"></a><span data-ttu-id="a46e4-109">Změny konfigurace</span><span class="sxs-lookup"><span data-stu-id="a46e4-109">Modifying configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a46e4-110">Několik chyb uvedených v tomto dokumentu dojít, protože byla překročena maximální nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="a46e4-110">Several of the errors listed in this document occur because a configured maximum has been exceeded.</span></span> <span data-ttu-id="a46e4-111">Při řešení krok uvádí, že můžete změnit hodnotu, musí používat jednu z těchto k provedení změn:</span><span class="sxs-lookup"><span data-stu-id="a46e4-111">When the resolution step mentions that you can change a value, you must use one of the following to perform the change:</span></span>

* <span data-ttu-id="a46e4-112">Pro **Windows** clustery: použijte akci skriptu ke konfiguraci hodnota během vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="a46e4-112">For **Windows** clusters: Use a script action to configure the value during cluster creation.</span></span> <span data-ttu-id="a46e4-113">Další informace najdete v tématu [vývoj akcí skriptů](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="a46e4-113">For more information, see [Develop script actions](hdinsight-hadoop-script-actions.md).</span></span>

* <span data-ttu-id="a46e4-114">Pro **Linux** clustery: použití Ambari (web nebo REST API) změňte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a46e4-114">For **Linux** clusters: Use Ambari (web or REST API) to modify the value.</span></span> <span data-ttu-id="a46e4-115">Další informace najdete v tématu [spravovat HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="a46e4-115">For more information, see [Manage HDInsight using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a46e4-116">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="a46e4-116">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a46e4-117">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a46e4-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="default-configuration"></a><span data-ttu-id="a46e4-118">Výchozí konfigurace</span><span class="sxs-lookup"><span data-stu-id="a46e4-118">Default configuration</span></span>

<span data-ttu-id="a46e4-119">Pokud se překročí následující výchozí hodnoty, může snížit výkon WebHCat nebo způsobit chyby:</span><span class="sxs-lookup"><span data-stu-id="a46e4-119">If the following default values are exceeded, it can degrade WebHCat performance or cause errors:</span></span>

| <span data-ttu-id="a46e4-120">Nastavení</span><span class="sxs-lookup"><span data-stu-id="a46e4-120">Setting</span></span> | <span data-ttu-id="a46e4-121">Výsledek</span><span class="sxs-lookup"><span data-stu-id="a46e4-121">What it does</span></span> | <span data-ttu-id="a46e4-122">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="a46e4-122">Default value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a46e4-123">[yarn.Scheduler.Capacity.maximum – aplikace][maximum-applications]</span><span class="sxs-lookup"><span data-stu-id="a46e4-123">[yarn.scheduler.capacity.maximum-applications][maximum-applications]</span></span> |<span data-ttu-id="a46e4-124">Maximální počet úloh, které mohou být souběžně aktivní (čekající na vyřízení nebo spuštěné)</span><span class="sxs-lookup"><span data-stu-id="a46e4-124">The maximum number of jobs that can be active concurrently (pending or running)</span></span> |<span data-ttu-id="a46e4-125">10 000</span><span class="sxs-lookup"><span data-stu-id="a46e4-125">10,000</span></span> |
| <span data-ttu-id="a46e4-126">[templeton.Exec.Max-procs][max-procs]</span><span class="sxs-lookup"><span data-stu-id="a46e4-126">[templeton.exec.max-procs][max-procs]</span></span> |<span data-ttu-id="a46e4-127">Maximální počet požadavků, které lze zpracovat současně</span><span class="sxs-lookup"><span data-stu-id="a46e4-127">The maximum number of requests that can be served concurrently</span></span> |<span data-ttu-id="a46e4-128">20</span><span class="sxs-lookup"><span data-stu-id="a46e4-128">20</span></span> |
| <span data-ttu-id="a46e4-129">[mapreduce.jobhistory.Max stáří ms][max-age-ms]</span><span class="sxs-lookup"><span data-stu-id="a46e4-129">[mapreduce.jobhistory.max-age-ms][max-age-ms]</span></span> |<span data-ttu-id="a46e4-130">Počet dní, které úlohy historii jsou uchovány</span><span class="sxs-lookup"><span data-stu-id="a46e4-130">The number of days that job history are retained</span></span> |<span data-ttu-id="a46e4-131">7 dní</span><span class="sxs-lookup"><span data-stu-id="a46e4-131">7 days</span></span> |

## <a name="too-many-requests"></a><span data-ttu-id="a46e4-132">Příliš mnoho požadavků</span><span class="sxs-lookup"><span data-stu-id="a46e4-132">Too many requests</span></span>

<span data-ttu-id="a46e4-133">**Kód stavu HTTP**: 429</span><span class="sxs-lookup"><span data-stu-id="a46e4-133">**HTTP Status code**: 429</span></span>

| <span data-ttu-id="a46e4-134">Příčina</span><span class="sxs-lookup"><span data-stu-id="a46e4-134">Cause</span></span> | <span data-ttu-id="a46e4-135">Řešení</span><span class="sxs-lookup"><span data-stu-id="a46e4-135">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="a46e4-136">Překročili jste maximální souběžných požadavků zpracovaných WebHCat za minutu (výchozí 20)</span><span class="sxs-lookup"><span data-stu-id="a46e4-136">You have exceeded the maximum concurrent requests served by WebHCat per minute (default 20)</span></span> |<span data-ttu-id="a46e4-137">Snížení velikosti pracovní zátěže zajistit, že neodešlete více než maximální počet souběžných požadavků nebo zvyšte limit počtu souběžných požadavků úpravou `templeton.exec.max-procs`.</span><span class="sxs-lookup"><span data-stu-id="a46e4-137">Reduce your workload to ensure that you do not submit more than the maximum number of concurrent requests or increase the concurrent request limit by modifying `templeton.exec.max-procs`.</span></span> <span data-ttu-id="a46e4-138">Další informace najdete v tématu [Modifying konfigurace](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="a46e4-138">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |

## <a name="server-unavailable"></a><span data-ttu-id="a46e4-139">Server není dostupný.</span><span class="sxs-lookup"><span data-stu-id="a46e4-139">Server unavailable</span></span>

<span data-ttu-id="a46e4-140">**Kód stavu HTTP**: 503</span><span class="sxs-lookup"><span data-stu-id="a46e4-140">**HTTP Status code**: 503</span></span>

| <span data-ttu-id="a46e4-141">Příčina</span><span class="sxs-lookup"><span data-stu-id="a46e4-141">Cause</span></span> | <span data-ttu-id="a46e4-142">Řešení</span><span class="sxs-lookup"><span data-stu-id="a46e4-142">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="a46e4-143">Tento kód stavu obvykle dojde během převzetí služeb při selhání mezi primárním a sekundárním HeadNode pro cluster</span><span class="sxs-lookup"><span data-stu-id="a46e4-143">This status code usually occurs during failover between the primary and secondary HeadNode for the cluster</span></span> |<span data-ttu-id="a46e4-144">Počkejte 2 minuty a potom operaci opakujte</span><span class="sxs-lookup"><span data-stu-id="a46e4-144">Wait two minutes, then retry the operation</span></span> |

## <a name="bad-request-content-could-not-find-job"></a><span data-ttu-id="a46e4-145">Chybný požadavek obsahu: Nelze najít úlohu</span><span class="sxs-lookup"><span data-stu-id="a46e4-145">Bad request Content: Could not find job</span></span>

<span data-ttu-id="a46e4-146">**Kód stavu HTTP**: 400</span><span class="sxs-lookup"><span data-stu-id="a46e4-146">**HTTP Status code**: 400</span></span>

| <span data-ttu-id="a46e4-147">Příčina</span><span class="sxs-lookup"><span data-stu-id="a46e4-147">Cause</span></span> | <span data-ttu-id="a46e4-148">Řešení</span><span class="sxs-lookup"><span data-stu-id="a46e4-148">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="a46e4-149">Podrobnosti úlohy byla vyčištěna podle historie úlohy čisticí</span><span class="sxs-lookup"><span data-stu-id="a46e4-149">Job details have been cleaned up by the job history cleaner</span></span> |<span data-ttu-id="a46e4-150">Výchozí doba uchování historie úlohy je 7 dní.</span><span class="sxs-lookup"><span data-stu-id="a46e4-150">The default retention period for job history is 7 days.</span></span> <span data-ttu-id="a46e4-151">Výchozí dobu uchování lze změnit úpravou `mapreduce.jobhistory.max-age-ms`.</span><span class="sxs-lookup"><span data-stu-id="a46e4-151">The default retention period can be changed by modifying `mapreduce.jobhistory.max-age-ms`.</span></span> <span data-ttu-id="a46e4-152">Další informace najdete v tématu [Modifying konfigurace](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="a46e4-152">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |
| <span data-ttu-id="a46e4-153">Úlohy byl ukončen z důvodu selhání</span><span class="sxs-lookup"><span data-stu-id="a46e4-153">Job has been killed due to a failover</span></span> |<span data-ttu-id="a46e4-154">Opakujte odeslání úlohy pro až dvě minuty.</span><span class="sxs-lookup"><span data-stu-id="a46e4-154">Retry job submission for up to two minutes</span></span> |
| <span data-ttu-id="a46e4-155">Neplatné id práce. byl použit.</span><span class="sxs-lookup"><span data-stu-id="a46e4-155">An Invalid job id was used</span></span> |<span data-ttu-id="a46e4-156">Kontrola, zda je správný id úlohy</span><span class="sxs-lookup"><span data-stu-id="a46e4-156">Check if the job id is correct</span></span> |

## <a name="bad-gateway"></a><span data-ttu-id="a46e4-157">Chybná brána</span><span class="sxs-lookup"><span data-stu-id="a46e4-157">Bad gateway</span></span>

<span data-ttu-id="a46e4-158">**Kód stavu HTTP**: 502</span><span class="sxs-lookup"><span data-stu-id="a46e4-158">**HTTP Status code**: 502</span></span>

| <span data-ttu-id="a46e4-159">Příčina</span><span class="sxs-lookup"><span data-stu-id="a46e4-159">Cause</span></span> | <span data-ttu-id="a46e4-160">Řešení</span><span class="sxs-lookup"><span data-stu-id="a46e4-160">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="a46e4-161">Uvolňování paměti interní dochází v rámci procesu WebHCat</span><span class="sxs-lookup"><span data-stu-id="a46e4-161">Internal garbage collection is occurring within the WebHCat process</span></span> |<span data-ttu-id="a46e4-162">Počkejte uvolňování paměti pro dokončení nebo restartujte službu WebHCat</span><span class="sxs-lookup"><span data-stu-id="a46e4-162">Wait for garbage collection to finish or restart the WebHCat service</span></span> |
| <span data-ttu-id="a46e4-163">Časový limit čekání na odpověď ze služby ResourceManager.</span><span class="sxs-lookup"><span data-stu-id="a46e4-163">Time out waiting on a response from the ResourceManager service.</span></span> <span data-ttu-id="a46e4-164">K této chybě může dojít, když počet aktivních aplikací přejde nakonfigurované maximum (výchozí 10 000)</span><span class="sxs-lookup"><span data-stu-id="a46e4-164">This error can occur when the number of active applications goes the configured maximum (default 10,000)</span></span> |<span data-ttu-id="a46e4-165">Počkejte aktuálně spuštěné úlohy k dokončení, nebo zvyšte limit souběžných úloh úpravou `yarn.scheduler.capacity.maximum-applications`.</span><span class="sxs-lookup"><span data-stu-id="a46e4-165">Wait for currently running jobs to complete or increase the concurrent job limit by modifying `yarn.scheduler.capacity.maximum-applications`.</span></span> <span data-ttu-id="a46e4-166">Další informace najdete v tématu [Modifying konfigurace](#modifying-configuration) části.</span><span class="sxs-lookup"><span data-stu-id="a46e4-166">For more information, see the [Modifying configuration](#modifying-configuration) section.</span></span> |
| <span data-ttu-id="a46e4-167">Pokus o načtení všech úloh prostřednictvím [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) volání při `Fields` je nastaven na`*`</span><span class="sxs-lookup"><span data-stu-id="a46e4-167">Attempting to retrieve all jobs through the [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) call while `Fields` is set to `*`</span></span> |<span data-ttu-id="a46e4-168">Nenačítat *všechny* podrobnosti úlohy.</span><span class="sxs-lookup"><span data-stu-id="a46e4-168">Do not retrieve *all* job details.</span></span> <span data-ttu-id="a46e4-169">Místo toho použijte `jobid` načíst podrobnosti úlohy pouze větší než id určité úlohy.</span><span class="sxs-lookup"><span data-stu-id="a46e4-169">Instead use `jobid` to retrieve details for jobs only greater than certain job id.</span></span> <span data-ttu-id="a46e4-170">Nebo, nepoužívejte`Fields`</span><span class="sxs-lookup"><span data-stu-id="a46e4-170">Or, do not use `Fields`</span></span> |
| <span data-ttu-id="a46e4-171">Služba WebHCat je mimo provoz během převzetí služeb při selhání HeadNode</span><span class="sxs-lookup"><span data-stu-id="a46e4-171">The WebHCat service is down during HeadNode failover</span></span> |<span data-ttu-id="a46e4-172">Počkejte dvou minut a opakujte operaci</span><span class="sxs-lookup"><span data-stu-id="a46e4-172">Wait for two minutes and retry the operation</span></span> |
| <span data-ttu-id="a46e4-173">Existuje více než 500 čekající úlohy, odeslané prostřednictvím WebHCat</span><span class="sxs-lookup"><span data-stu-id="a46e4-173">There are more than 500 pending jobs submitted through WebHCat</span></span> |<span data-ttu-id="a46e4-174">Počkejte na dokončení aktuálně čeká na provedení úloh před odesláním další úlohy</span><span class="sxs-lookup"><span data-stu-id="a46e4-174">Wait until currently pending jobs have completed before submitting more jobs</span></span> |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
