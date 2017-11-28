---
title: "Analýza platformy: Apache Storm porovnání tooStream Analytics | Microsoft Docs"
description: "Získáte pokyny výběr cloudové platformy analýzy pomocí tooStream porovnání Apache Storm Analytics. Pochopit funkce a rozdíly."
keywords: "analytické platformě, analýzy platformy, Cloudová platforma analýzy, storm porovnání"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: b9aac017-9866-4d0a-b98f-6f03881e9339
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/27/2017
ms.author: samacha
ms.openlocfilehash: 5a0ec5b2439596f0da962f04b776472031660062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="choosing-a-streaming-analytics-platform-comparing-apache-storm-and-azure-stream-analytics"></a><span data-ttu-id="32ad7-105">Výběr streamování analytické platformě: porovnání Apache Storm a Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="32ad7-105">Choosing a streaming analytics platform: comparing Apache Storm and Azure Stream Analytics</span></span>
<span data-ttu-id="32ad7-106">Azure poskytuje několik řešení pro analýzu dat: [Azure streamování Analytics](https://docs.microsoft.com/azure/stream-analytics/) a [Apache Storm v Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-storm/).</span><span class="sxs-lookup"><span data-stu-id="32ad7-106">Azure provides multiple solutions for analyzing streaming data: [Azure Streaming Analytics](https://docs.microsoft.com/azure/stream-analytics/) and [Apache Storm on Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-storm/).</span></span> <span data-ttu-id="32ad7-107">Obě platformy analytics zadejte hello výhod PaaS řešení.</span><span class="sxs-lookup"><span data-stu-id="32ad7-107">Both analytics platforms provide hello benefits of a PaaS solution.</span></span> <span data-ttu-id="32ad7-108">Ale hello platformy mít některé významné rozdíly v jejich schopnosti stejně jako v tom, jak konfigurovat a spravovat je.</span><span class="sxs-lookup"><span data-stu-id="32ad7-108">But hello platforms have some significant differences in their capabilities as well as in how you configure and manage them.</span></span> 

<span data-ttu-id="32ad7-109">Tento článek obsahuje porovnání funkcí toohelp, vybraných mezi Apache Storm a Azure Stream Analytics jako cloudové platformy analytics vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="32ad7-109">This article provides a side-by-side comparison of features toohelp you choose between Apache Storm and Azure Stream Analytics as a cloud analytics platform.</span></span> 

## <a name="general-features"></a><span data-ttu-id="32ad7-110">Obecné funkce</span><span class="sxs-lookup"><span data-stu-id="32ad7-110">General features</span></span>

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-111">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-111">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="32ad7-112">
                    <strong>Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-112">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="32ad7-113">
                    <strong>Apache Storm v HDInsight</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-113">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-114">
                    <strong>Otevřít zdroj?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-114">
                    <strong>Open source?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-115">Ne.</span><span class="sxs-lookup"><span data-stu-id="32ad7-115">No.</span></span> <span data-ttu-id="32ad7-116">Azure Stream Analytics je Microsoft nechráněný nabídky.</span><span class="sxs-lookup"><span data-stu-id="32ad7-116">Azure Stream Analytics is a Microsoft proprietary offering.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-117">Ano.</span><span class="sxs-lookup"><span data-stu-id="32ad7-117">Yes.</span></span> <span data-ttu-id="32ad7-118">Apache Storm je technologie Apache licenci.</span><span class="sxs-lookup"><span data-stu-id="32ad7-118">Apache Storm is an Apache licensed technology.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-119">
                    <strong>Podporu společnosti Microsoft?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-119">
                    <strong>Microsoft support?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-120">Ano</span><span class="sxs-lookup"><span data-stu-id="32ad7-120">Yes</span></span> </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-121">Ano</span><span class="sxs-lookup"><span data-stu-id="32ad7-121">Yes</span></span> </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-122">
                    <strong>Požadavky na hardware</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-122">
                    <strong>Hardware requirements</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-123">Žádné.</span><span class="sxs-lookup"><span data-stu-id="32ad7-123">None.</span></span> <span data-ttu-id="32ad7-124">Azure Stream Analytics je služba Azure.</span><span class="sxs-lookup"><span data-stu-id="32ad7-124">Azure Stream Analytics is an Azure Service.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-125">Žádné.</span><span class="sxs-lookup"><span data-stu-id="32ad7-125">None.</span></span> <span data-ttu-id="32ad7-126">Apache Storm je služba Azure.</span><span class="sxs-lookup"><span data-stu-id="32ad7-126">Apache Storm is an Azure Service.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-127">
                    <strong>Nejvyšší úrovně jednotky</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-127">
                    <strong>Top-level unit</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-128">Uživatelé nasazení a monitorování úloh streamování.</span><span class="sxs-lookup"><span data-stu-id="32ad7-128">Users deploy and monitor streaming jobs.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-129">Uživatelé nasazení a monitorování celý cluster, který může být hostitelem více úloh Storm, jakož i jiné úlohy (včetně batch).</span><span class="sxs-lookup"><span data-stu-id="32ad7-129">Users deploy and monitor a whole cluster, which can host multiple Storm jobs as well as other workloads (including batch).</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-130">
                    <strong>Ceny</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-130">
                    <strong>Pricing</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-131">Za cenu podle objemu dat, zpracování a hello počet jednotek streamování požadované za hodinu které hello úloha běží.</span><span class="sxs-lookup"><span data-stu-id="32ad7-131">Priced by volume of data processed and hello number of streaming units required per hour that hello job is running.</span></span> 
                </p>
                    <p><span data-ttu-id="32ad7-132">Další informace najdete v tématu <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics ceny</a>.</span><span class="sxs-lookup"><span data-stu-id="32ad7-132">For more information, see <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics Pricing</a>.</span></span></p>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-133">jednotka Hello od nákupu je založen na clusteru a je účtován na základě v době hello hello clusteru je spuštěn, nezávisle na úlohy nasazení.</span><span class="sxs-lookup"><span data-stu-id="32ad7-133">hello unit of purchase is cluster-based, and is charged based on hello time hello cluster is running, independent of jobs deployed.</span></span>
                </p>
                <p>
<span data-ttu-id="32ad7-134">Další informace najdete v tématu <a href="http://azure.microsoft.com/pricing/details/hdinsight/">HDInsight ceny</a>.</span><span class="sxs-lookup"><span data-stu-id="32ad7-134">For more information, see <a href="http://azure.microsoft.com/pricing/details/hdinsight/">HDInsight pricing</a>.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="authoring"></a><span data-ttu-id="32ad7-135">Vytváření obsahu</span><span class="sxs-lookup"><span data-stu-id="32ad7-135">Authoring</span></span>

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-136">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-136">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="32ad7-137">
                    <strong>Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-137">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="32ad7-138">
                    <strong>Apache Storm v HDInsight</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-138">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-139">
                    <strong>Možnosti: DSL SQL?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-139">
                    <strong>Capabilities: SQL DSL?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-140">Ano.</span><span class="sxs-lookup"><span data-stu-id="32ad7-140">Yes.</span></span> <span data-ttu-id="32ad7-141">Stream Analytics poskytuje podobné jazyku SQL jazyk pro vytváření transformace.</span><span class="sxs-lookup"><span data-stu-id="32ad7-141">Stream Analytics provides a SQL-like language for creating transformations.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-142">Ne.</span><span class="sxs-lookup"><span data-stu-id="32ad7-142">No.</span></span> <span data-ttu-id="32ad7-143">Uživatelé psaní kódu v jazyce Java nebo C#, nebo pomocí rozhraní API Trident.</span><span class="sxs-lookup"><span data-stu-id="32ad7-143">Users write code in Java or C#, or use Trident APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-144">
                    <strong>Možnosti: Dočasné operátory?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-144">
                    <strong>Capabilities: Temporal operators?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-145">Ve výchozím nastavení jsou podporovány agregací v časových oknech a dočasných spojení.</span><span class="sxs-lookup"><span data-stu-id="32ad7-145">Windowed aggregates and temporal joins are supported by default.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-146">Dočasné operátory musí být implementované uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="32ad7-146">Temporal operators must be implemented by hello user.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-147">
                    <strong>Vývojové prostředí</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-147">
                    <strong>Development experience</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-148">Uživatelé mohou vytvořit, ladění a monitorujte úlohy prostřednictvím hello portál Azure, pomocí ukázkových dat, které jsou odvozené od živý datový proud.</span><span class="sxs-lookup"><span data-stu-id="32ad7-148">Users can create, debug, and monitor jobs through hello Azure portal, using sample data derived from a live stream.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-149">Uživatelé pomocí rozhraní .NET můžete vyvíjet, ladit a sledovat pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32ad7-149">Users using .NET can develop, debug, and monitor through Visual Studio.</span></span> <span data-ttu-id="32ad7-150">Uživatele, kteří používají Java nebo jiných jazyků, můžete použít hello IDE si sami vyberou.</span><span class="sxs-lookup"><span data-stu-id="32ad7-150">Users using Java or other languages can use hello IDE of their choice.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-151">
                    <strong>Podpora ladění</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-151">
                    <strong>Debugging support</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-152">Protokoly stavu a operací základní úlohy jsou k dispozici toohelp ladění.</span><span class="sxs-lookup"><span data-stu-id="32ad7-152">Basic job status and operations logs are available toohelp debug.</span></span> <span data-ttu-id="32ad7-153">Stream Analytics aktuálně neumožňuje uživatelům zadejte, který obsah nebo kolik obsahu je zahrnuta v protokolech hello (tj. podrobné režim).</span><span class="sxs-lookup"><span data-stu-id="32ad7-153">Stream Analytics currently does not let users specify what content or how much content is included in hello logs (i.e., verbose mode).</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-154">Podrobné protokoly jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="32ad7-154">Detailed logs are available.</span></span> <span data-ttu-id="32ad7-155">Uživatelé můžou používat protokoly v sadě Visual Studio nebo protokolování v clusteru toohello a přístup k hello protokoly přímo.</span><span class="sxs-lookup"><span data-stu-id="32ad7-155">Users can access logs in Visual Studio or by logging in toohello cluster and accessing hello logs directly.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-156">
                    <strong>Rozšiřitelnost pomocí vlastního kódu?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-156">
                    <strong>Extensibility using custom code?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-157">Částečně podporovat funkce JavaScript UDF.</span><span class="sxs-lookup"><span data-stu-id="32ad7-157">Partially support with JavaScript UDFs.</span></span> <span data-ttu-id="32ad7-158">Další informace najdete v tématu <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">integrace jazyka JavaScript UDF</a>.</span><span class="sxs-lookup"><span data-stu-id="32ad7-158">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">JavaScript UDF integration</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-159">Ano.</span><span class="sxs-lookup"><span data-stu-id="32ad7-159">Yes.</span></span> <span data-ttu-id="32ad7-160">Uživatele můžete psát vlastní kód v C#, Java nebo dalších jazyků na Storm podporována.</span><span class="sxs-lookup"><span data-stu-id="32ad7-160">Users can write custom code in C#, Java, or any other language supported on Storm.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="data-sources-inputs-and-outputs"></a><span data-ttu-id="32ad7-161">Zdroje dat (vstupy) a výstupy</span><span class="sxs-lookup"><span data-stu-id="32ad7-161">Data sources (inputs) and outputs</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-162">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-162">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="32ad7-163">
                    <strong>Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-163">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="32ad7-164">
                    <strong>Apache Storm v HDInsight</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-164">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-165">
                 <strong>Vstupní datové zdroje</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-165">
                 <strong>Input data sources</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="32ad7-166">Úložiště Azure Event Hubs a objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="32ad7-166">Azure Event Hubs and Azure Blob storage.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-167">Konektory jsou k dispozici pro Azure Event Hubs, Azure Service Bus, Kafka a další.</span><span class="sxs-lookup"><span data-stu-id="32ad7-167">Connectors are available for Azure Event Hubs, Azure Service Bus, Kafka, and more.</span></span> <span data-ttu-id="32ad7-168">Uživatelé mohou vytvářet další konektorů pomocí vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="32ad7-168">Users can create additional connectors using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-169">
                    <strong>Vstupní data formátů</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-169">
                    <strong>Input data formats</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-170">Avro, formát JSON, CSV</span><span class="sxs-lookup"><span data-stu-id="32ad7-170">Avro, JSON, CSV</span></span> </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-171">Uživatele můžete implementovat libovolném formátu pomocí vlastního kódu.</span><span class="sxs-lookup"><span data-stu-id="32ad7-171">Users can implement any format using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-172">
                    <strong>Výstupy</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-172">
                    <strong>Outputs</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-173">Úloha streamování může mít několik výstupů.</span><span class="sxs-lookup"><span data-stu-id="32ad7-173">A streaming job can have multiple outputs.</span></span> <span data-ttu-id="32ad7-174">Podporované výstupy jsou Azure Event Hubs, Azure Blob úložiště, Azure Table storage, Azure SQL DB a Power BI.</span><span class="sxs-lookup"><span data-stu-id="32ad7-174">Supported outputs are Azure Event Hubs, Azure Blob storage, Azure Table storage, Azure SQL DB, and Power BI.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-175">Storm podporuje mnoho výstupy v topologii a každý výstup může mít vlastní logiky pro příjem dat zpracování.</span><span class="sxs-lookup"><span data-stu-id="32ad7-175">Storm supports many outputs in a topology, and each output can have custom logic for downstream processing.</span></span> <span data-ttu-id="32ad7-176">Storm obsahuje konektory pro Power BI, Azure Event Hubs, úložiště objektů Blob v Azure, Azure Cosmos DB, SQL a HBase.</span><span class="sxs-lookup"><span data-stu-id="32ad7-176">Storm includes connectors for Power BI, Azure Event Hubs, Azure Blob storage, Azure Cosmos DB, SQL, and HBase.</span></span> <span data-ttu-id="32ad7-177">Uživatelé mohou vytvářet další konektorů pomocí vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="32ad7-177">Users can create additional connectors using custom code.</span></span>    
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-178">
                    <strong>Kódování dat formátů</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-178">
                    <strong>Data-encoding formats</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-179">Data musí být naformátován pomocí znakové sady UTF-8.</span><span class="sxs-lookup"><span data-stu-id="32ad7-179">Data must be formatted using UTF-8.</span></span>
                </p>
            </td>   
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-180">Uživatele můžete implementovat formát pro kódování data pomocí vlastního kódu.</span><span class="sxs-lookup"><span data-stu-id="32ad7-180">Users can implement any data encoding format using custom code.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="management-and-operations"></a><span data-ttu-id="32ad7-181">Operace a Správa</span><span class="sxs-lookup"><span data-stu-id="32ad7-181">Management and operations</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-182">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-182">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="32ad7-183">
                    <strong>Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-183">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="32ad7-184">
                    <strong>Apache Storm v HDInsight</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-184">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-185">
                    <strong>Úloha nasazení modelu</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-185">
                    <strong>Job deployment model</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-186">Portál Azure, PowerShell a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="32ad7-186">Azure portal, PowerShell, and REST APIs.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-187">Portál Azure, prostředí PowerShell, sady Visual Studio a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="32ad7-187">Azure portal, PowerShell, Visual Studio, and REST APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-188">
                    <strong>Monitorování (statistiky, čítače atd.)</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-188">
                    <strong>Monitoring (stats, counters, etc.)</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-189">Monitorování je implementovaná pomocí portálu Azure a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="32ad7-189">Monitoring is implemented using Azure portal and REST APIs.</span></span> <span data-ttu-id="32ad7-190">Uživatelé také mohou nakonfigurovat Azure výstrahy.</span><span class="sxs-lookup"><span data-stu-id="32ad7-190">Users can also configure Azure alerts.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-191">Monitorování je implementovaná pomocí hello uživatelské rozhraní Storm a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="32ad7-191">Monitoring is implemented using hello Storm UI and REST APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-192">
                    <strong>Škálovatelnost</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-192">
                    <strong>Scalability</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-193">Škálovatelnost je určen podle hello počet jednotek streamování (SUs) pro každou úlohu.</span><span class="sxs-lookup"><span data-stu-id="32ad7-193">Scalability is determined by hello number of Streaming Units (SUs) for each job.</span></span> <span data-ttu-id="32ad7-194">Jednotlivé jednotky streamování zpracovává až too1 MB za sekundu, s maximální počet jednotek 50.</span><span class="sxs-lookup"><span data-stu-id="32ad7-194">Each Streaming Unit processes up too1 MB/second, with a maximum 50 units.</span></span> <span data-ttu-id="32ad7-195">Další informace najdete v tématu <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">škálování tooincrease propustnost</a>.</span><span class="sxs-lookup"><span data-stu-id="32ad7-195">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">Scale tooincrease throughput</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-196">Škálovatelnost je určen podle hello počet uzlů v clusteru HDInsight Storm hello.</span><span class="sxs-lookup"><span data-stu-id="32ad7-196">Scalability is determined by hello number of nodes in hello HDInsight Storm cluster.</span></span> <span data-ttu-id="32ad7-197">horní limit Hello hello počet uzlů je definována Azure kvótu hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="32ad7-197">hello top limit on hello number of nodes is defined by hello user's Azure quota.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-198">
                    <strong>Omezení zpracování dat</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-198">
                    <strong>Data processing limits</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-199">Uživatelé mohou zvýšit zpracování dat nebo optimalizovat náklady zvýšením nebo snížením hello počet jednotek streamování s maximální limit je 1 GB za sekundu.</span><span class="sxs-lookup"><span data-stu-id="32ad7-199">Users can increase data processing or optimize costs by increasing or decreasing hello number of Streaming Units, with an upper limit of 1 GB/second.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-200">Uživatele můžete škálovat velikost clusteru nahoru nebo dolů.</span><span class="sxs-lookup"><span data-stu-id="32ad7-200">Users can scale cluster size up or down.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-201">
                    <strong>Zastavení nebo obnovení</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-201">
                    <strong>Stop/Resume</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-202">Zastavení a obnovení z poslední místo byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="32ad7-202">Stop and resume from last place stopped.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-203">Zastavení a obnovení z poslední umístit zastaven podle vodoznak.</span><span class="sxs-lookup"><span data-stu-id="32ad7-203">Stop and resume from last place stopped based on a watermark.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-204">
                    <strong>Aktualizace služby a framework</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-204">
                    <strong>Service and framework update</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-205">Automatické opravy bez výpadků.</span><span class="sxs-lookup"><span data-stu-id="32ad7-205">Automatic patching with no downtime.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-206">Automatické opravy bez výpadků.</span><span class="sxs-lookup"><span data-stu-id="32ad7-206">Automatic patching with no downtime.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-207">
                    <strong>Kontinuita podnikových procesů pomocí vysoce dostupné služby s zaručenou SLA</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-207">
                    <strong>Business continuity through a Highly Available Service with guaranteed SLAs</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <ul>
                <li><span data-ttu-id="32ad7-208">Smlouva SLA na úrovni 99,9 % doby provozu</span><span class="sxs-lookup"><span data-stu-id="32ad7-208">SLA of 99.9% uptime</span></span></li>
                <li><span data-ttu-id="32ad7-209">Automatické obnovení po selhání</span><span class="sxs-lookup"><span data-stu-id="32ad7-209">Auto-recovery from failures</span></span></li>
                <li><span data-ttu-id="32ad7-210">Předdefinované obnovení stavová dočasné operátory</span><span class="sxs-lookup"><span data-stu-id="32ad7-210">Built-in recovery of stateful temporal operators</span></span></li>
                </ul>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-211">Smlouva SLA na úrovni 99,9 % doby provozu clusteru Storm hello.</span><span class="sxs-lookup"><span data-stu-id="32ad7-211">SLA of 99.9% uptime of hello Storm cluster.</span></span> 
                </p>
                <p>
<span data-ttu-id="32ad7-212">Apache Storm je odolný proti chybám streamování platforma.</span><span class="sxs-lookup"><span data-stu-id="32ad7-212">Apache Storm is a fault-tolerant streaming platform.</span></span> <span data-ttu-id="32ad7-213">Je však tooensure odpovědnost hello uživatele, aby datové proudy úlohy spustit bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="32ad7-213">However, it is hello user's responsibility tooensure that streaming jobs run uninterrupted.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="advanced-features"></a><span data-ttu-id="32ad7-214">Pokročilé funkce</span><span class="sxs-lookup"><span data-stu-id="32ad7-214">Advanced features</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-215">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-215">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="32ad7-216">
                    <strong>Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-216">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="32ad7-217">
                    <strong>Apache Storm v HDInsight</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-217">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-218">
                    <strong>Pozdní příchodem a na více systémů pořadí zpracování událostí</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-218">
                    <strong>Late arrival and out-of-order event handling</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-219">Konfigurovat integrovaných zásad můžete změnit pořadí událostí, odstraňte události nebo upravit čas události.</span><span class="sxs-lookup"><span data-stu-id="32ad7-219">Built-in configurable policies can reorder events, drop events, or adjust event time.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-220">Uživatelé musí implementovat logiku toohandle tento scénář.</span><span class="sxs-lookup"><span data-stu-id="32ad7-220">Users must implement logic toohandle this scenario.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-221">
                    <strong>Referenční data</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-221">
                    <strong>Reference data</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-222">Referenční data jsou k dispozici z Azure Blob storage s maximálně 100 MB mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="32ad7-222">Reference data is available from Azure Blob storage with a maximum of 100 MB of in-memory cache.</span></span> <span data-ttu-id="32ad7-223">Referenční data se aktualizují službou hello.</span><span class="sxs-lookup"><span data-stu-id="32ad7-223">Reference data is refreshed by hello service.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-224">Žádné omezení velikosti dat.</span><span class="sxs-lookup"><span data-stu-id="32ad7-224">No limits on data size.</span></span> <span data-ttu-id="32ad7-225">Konektory jsou k dispozici pro HBase, Azure Cosmos DB, SQL Server a Azure.</span><span class="sxs-lookup"><span data-stu-id="32ad7-225">Connectors are available for HBase, Azure Cosmos DB, SQL Server, and Azure.</span></span> <span data-ttu-id="32ad7-226">Uživatelé mohou vytvářet další konektorů pomocí vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="32ad7-226">Users can create additional connectors using custom code.</span></span> <span data-ttu-id="32ad7-227">Referenční data musí aktualizovat pomocí vlastního kódu.</span><span class="sxs-lookup"><span data-stu-id="32ad7-227">Reference data must be refreshed using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="32ad7-228">
                    <strong>Integrace s Machine Learning</strong>
                </span><span class="sxs-lookup"><span data-stu-id="32ad7-228">
                    <strong>Integration with Machine Learning</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="32ad7-229">Publikovat Azure Machine Learning modely můžete nakonfigurovat jako funkce při vytvoření úlohy.</span><span class="sxs-lookup"><span data-stu-id="32ad7-229">Published Azure Machine Learning models can be configured as functions during job creation.</span></span> <span data-ttu-id="32ad7-230">Další informace najdete v tématu <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">škálování pro Machine Learning funkce</a>.</span><span class="sxs-lookup"><span data-stu-id="32ad7-230">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">Scale for Machine Learning functions</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="32ad7-231">K dispozici prostřednictvím funkce Bolts Storm.</span><span class="sxs-lookup"><span data-stu-id="32ad7-231">Available through Storm Bolts.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>
