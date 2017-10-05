---
title: "Řešení potíží YARN pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi na časté otázky týkající se práce s Apache Hadoop YARN a Azure HDInsight."
keywords: "Azure HDInsight, YARN – nejčastější dotazy, řešení potíží s průvodce, časté otázky"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: F76786A9-99AB-4B85-9B15-CA03528FC4CD
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.openlocfilehash: 63f2d88ad59661b7fbcffd0aaeb94c58d40bdb73
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a><span data-ttu-id="b9328-104">Řešení potíží YARN pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9328-104">Troubleshoot YARN by using Azure HDInsight</span></span>

<span data-ttu-id="b9328-105">Další informace o hlavních problémů a jejich řešení při práci s Apache Hadoop YARN datové části v Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="b9328-105">Learn about the top issues and their resolutions when working with Apache Hadoop YARN payloads in Apache Ambari.</span></span>

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a><span data-ttu-id="b9328-106">Jak vytvořit novou frontu YARN v clusteru</span><span class="sxs-lookup"><span data-stu-id="b9328-106">How do I create a new YARN queue on a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="b9328-107">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="b9328-107">Resolution steps</span></span> 

<span data-ttu-id="b9328-108">Vytvořte novou frontu YARN pomocí následujících kroků v Ambari a pak vyvážit přidělení kapacity mezi všechny fronty.</span><span class="sxs-lookup"><span data-stu-id="b9328-108">Use the following steps in Ambari to create a new YARN queue, and then balance the capacity allocation among all the queues.</span></span> 

<span data-ttu-id="b9328-109">V tomto příkladu dvě existující fronty (**výchozí** a **thriftsvr**) obě se změnil z 50 % kapacity na kapacity 25 %, která poskytuje nové kapacity 50 % fronty (spark).</span><span class="sxs-lookup"><span data-stu-id="b9328-109">In this example, two existing queues (**default** and **thriftsvr**) both are changed from 50% capacity to 25% capacity, which gives the new queue (spark) 50% capacity.</span></span>
| <span data-ttu-id="b9328-110">Fronta</span><span class="sxs-lookup"><span data-stu-id="b9328-110">Queue</span></span> | <span data-ttu-id="b9328-111">Kapacita</span><span class="sxs-lookup"><span data-stu-id="b9328-111">Capacity</span></span> | <span data-ttu-id="b9328-112">Maximální kapacita</span><span class="sxs-lookup"><span data-stu-id="b9328-112">Maximum capacity</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b9328-113">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b9328-113">default</span></span> | <span data-ttu-id="b9328-114">25 %</span><span class="sxs-lookup"><span data-stu-id="b9328-114">25%</span></span> | <span data-ttu-id="b9328-115">50%</span><span class="sxs-lookup"><span data-stu-id="b9328-115">50%</span></span> |
| <span data-ttu-id="b9328-116">thrftsvr</span><span class="sxs-lookup"><span data-stu-id="b9328-116">thrftsvr</span></span> | <span data-ttu-id="b9328-117">25 %</span><span class="sxs-lookup"><span data-stu-id="b9328-117">25%</span></span> | <span data-ttu-id="b9328-118">50%</span><span class="sxs-lookup"><span data-stu-id="b9328-118">50%</span></span> |
| <span data-ttu-id="b9328-119">Spark</span><span class="sxs-lookup"><span data-stu-id="b9328-119">spark</span></span> | <span data-ttu-id="b9328-120">50%</span><span class="sxs-lookup"><span data-stu-id="b9328-120">50%</span></span> | <span data-ttu-id="b9328-121">50%</span><span class="sxs-lookup"><span data-stu-id="b9328-121">50%</span></span> |

1. <span data-ttu-id="b9328-122">Vyberte **zobrazení Ambari** ikonu a potom vyberte vzoru mřížky.</span><span class="sxs-lookup"><span data-stu-id="b9328-122">Select the **Ambari Views** icon, and then select the grid pattern.</span></span> <span data-ttu-id="b9328-123">Potom vyberte **správce front YARN**.</span><span class="sxs-lookup"><span data-stu-id="b9328-123">Next, select **YARN Queue Manager**.</span></span>

    ![Vyberte ikonu zobrazení Ambari](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. <span data-ttu-id="b9328-125">Vyberte **výchozí** fronty.</span><span class="sxs-lookup"><span data-stu-id="b9328-125">Select the **default** queue.</span></span>

    ![Vyberte výchozí fronty](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. <span data-ttu-id="b9328-127">Pro **výchozí** fronty, změňte **kapacity** z 50 % 25 %.</span><span class="sxs-lookup"><span data-stu-id="b9328-127">For the **default** queue, change the **capacity** from 50% to 25%.</span></span> <span data-ttu-id="b9328-128">Pro **thriftsvr** fronty, změňte **kapacity** 25 %.</span><span class="sxs-lookup"><span data-stu-id="b9328-128">For the **thriftsvr** queue, change the **capacity** to 25%.</span></span>

    ![Změňte kapacitu na 25 % pro výchozí a thriftsvr fronty](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. <span data-ttu-id="b9328-130">Chcete-li vytvořit novou frontu, vyberte **přidat fronty**.</span><span class="sxs-lookup"><span data-stu-id="b9328-130">To create a new queue, select **Add Queue**.</span></span>

    ![Vyberte Přidat fronty](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. <span data-ttu-id="b9328-132">Název nové fronty.</span><span class="sxs-lookup"><span data-stu-id="b9328-132">Name the new queue.</span></span>

    ![Název fronty Spark](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. <span data-ttu-id="b9328-134">Ponechte **kapacity** hodnoty na 50 % a potom vyberte **akce** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b9328-134">Leave the **capacity** values at 50%, and then select the **Actions** button.</span></span>

    ![Kliknutím na tlačítko akce](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. <span data-ttu-id="b9328-136">Vyberte **uložte a aktualizujte fronty**.</span><span class="sxs-lookup"><span data-stu-id="b9328-136">Select **Save and Refresh Queues**.</span></span>

    ![Vyberte uložte a aktualizujte fronty](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

<span data-ttu-id="b9328-138">Tyto změny se projeví okamžitě v Uživatelském rozhraní YARN plánovače.</span><span class="sxs-lookup"><span data-stu-id="b9328-138">These changes are visible immediately on the YARN Scheduler UI.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="b9328-139">Další čtení</span><span class="sxs-lookup"><span data-stu-id="b9328-139">Additional reading</span></span>

- [<span data-ttu-id="b9328-140">YARN CapacityScheduler</span><span class="sxs-lookup"><span data-stu-id="b9328-140">YARN CapacityScheduler</span></span>](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a><span data-ttu-id="b9328-141">Jak lze stáhnout protokoly YARN z clusteru</span><span class="sxs-lookup"><span data-stu-id="b9328-141">How do I download YARN logs from a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="b9328-142">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="b9328-142">Resolution steps</span></span> 

1. <span data-ttu-id="b9328-143">Připojte se ke clusteru HDInsight pomocí klienta Secure Shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="b9328-143">Connect to the HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="b9328-144">Další informace najdete v tématu [další čtení](#additional-reading-2).</span><span class="sxs-lookup"><span data-stu-id="b9328-144">For more information, see [Additional reading](#additional-reading-2).</span></span>

2. <span data-ttu-id="b9328-145">Pro zobrazení seznamu všech ID aplikace YARN aplikací, které jsou aktuálně spuštěny, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b9328-145">To list all the application IDs of the YARN applications that are currently running, run the following command:</span></span>

    ```apache
    yarn top
    ```
    <span data-ttu-id="b9328-146">ID, které jsou uvedeny v **APPLICATIONID** sloupce.</span><span class="sxs-lookup"><span data-stu-id="b9328-146">The IDs are listed in the **APPLICATIONID** column.</span></span> <span data-ttu-id="b9328-147">Protokoly z si můžete stáhnout **APPLICATIONID** sloupce.</span><span class="sxs-lookup"><span data-stu-id="b9328-147">You can download logs from the **APPLICATIONID** column.</span></span>

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

3. <span data-ttu-id="b9328-148">Ke stažení protokolů YARN kontejner pro všechny servery aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b9328-148">To download YARN container logs for all application masters, use the following command:</span></span>
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    <span data-ttu-id="b9328-149">Tento příkaz vytvoří soubor protokolu s názvem amlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="b9328-149">This command creates a log file named amlogs.txt.</span></span> 

4. <span data-ttu-id="b9328-150">Ke stažení protokolů YARN kontejner pro pouze nejnovější hlavní aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b9328-150">To download YARN container logs for only the latest application master, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    <span data-ttu-id="b9328-151">Tento příkaz vytvoří soubor protokolu s názvem latestamlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="b9328-151">This command creates a log file named latestamlogs.txt.</span></span> 

4. <span data-ttu-id="b9328-152">Ke stažení protokolů YARN kontejner pro první předlohy dvě aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b9328-152">To download YARN container logs for the first two application masters, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    <span data-ttu-id="b9328-153">Tento příkaz vytvoří soubor protokolu s názvem first2amlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="b9328-153">This command creates a log file named first2amlogs.txt.</span></span> 

5. <span data-ttu-id="b9328-154">Chcete-li stáhnout všechny protokoly YARN kontejner, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b9328-154">To download all YARN container logs, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    <span data-ttu-id="b9328-155">Tento příkaz vytvoří soubor protokolu s názvem logs.txt.</span><span class="sxs-lookup"><span data-stu-id="b9328-155">This command creates a log file named logs.txt.</span></span> 

6. <span data-ttu-id="b9328-156">Pokud chcete stáhnout protokol YARN kontejner pro specifický kontejner, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b9328-156">To download the YARN container log for a specific container, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    <span data-ttu-id="b9328-157">Tento příkaz vytvoří soubor protokolu s názvem containerlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="b9328-157">This command creates a log file named containerlogs.txt.</span></span>

### <span data-ttu-id="b9328-158"><a name="additional-reading-2"></a>Další čtení</span><span class="sxs-lookup"><span data-stu-id="b9328-158"><a name="additional-reading-2"></a>Additional reading</span></span>

- [<span data-ttu-id="b9328-159">Připojení k HDInsight (Hadoop) pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="b9328-159">Connect to HDInsight (Hadoop) by using SSH</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [<span data-ttu-id="b9328-160">Apache Hadoop YARN koncepty a aplikací</span><span class="sxs-lookup"><span data-stu-id="b9328-160">Apache Hadoop YARN concepts and applications</span></span>](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)







