---
title: "aaaTroubleshoot YARN pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi toocommon dotazy týkající se práce s Apache Hadoop YARN a Azure HDInsight."
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
ms.openlocfilehash: 800d9738cb27e05a64db470ee58565af3b85aa99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a><span data-ttu-id="b5eb2-104">Řešení potíží YARN pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="b5eb2-104">Troubleshoot YARN by using Azure HDInsight</span></span>

<span data-ttu-id="b5eb2-105">Další informace o hello nejčastější problémy a jejich řešení při práci s Apache Hadoop YARN datové části v Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-105">Learn about hello top issues and their resolutions when working with Apache Hadoop YARN payloads in Apache Ambari.</span></span>

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a><span data-ttu-id="b5eb2-106">Jak vytvořit novou frontu YARN v clusteru</span><span class="sxs-lookup"><span data-stu-id="b5eb2-106">How do I create a new YARN queue on a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="b5eb2-107">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="b5eb2-107">Resolution steps</span></span> 

<span data-ttu-id="b5eb2-108">Použití hello následující kroky v Ambari toocreate novou frontu YARN a pak vyvážit přidělení kapacity hello mezi všechny fronty hello.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-108">Use hello following steps in Ambari toocreate a new YARN queue, and then balance hello capacity allocation among all hello queues.</span></span> 

<span data-ttu-id="b5eb2-109">V tomto příkladu dvě existující fronty (**výchozí** a **thriftsvr**) i se změnilo z 50 % kapacity too25 % kapacity, která umožňuje hello nové fronty (spark) 50 % kapacity.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-109">In this example, two existing queues (**default** and **thriftsvr**) both are changed from 50% capacity too25% capacity, which gives hello new queue (spark) 50% capacity.</span></span>
| <span data-ttu-id="b5eb2-110">Fronta</span><span class="sxs-lookup"><span data-stu-id="b5eb2-110">Queue</span></span> | <span data-ttu-id="b5eb2-111">Kapacita</span><span class="sxs-lookup"><span data-stu-id="b5eb2-111">Capacity</span></span> | <span data-ttu-id="b5eb2-112">Maximální kapacita</span><span class="sxs-lookup"><span data-stu-id="b5eb2-112">Maximum capacity</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b5eb2-113">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b5eb2-113">default</span></span> | <span data-ttu-id="b5eb2-114">25 %</span><span class="sxs-lookup"><span data-stu-id="b5eb2-114">25%</span></span> | <span data-ttu-id="b5eb2-115">50%</span><span class="sxs-lookup"><span data-stu-id="b5eb2-115">50%</span></span> |
| <span data-ttu-id="b5eb2-116">thrftsvr</span><span class="sxs-lookup"><span data-stu-id="b5eb2-116">thrftsvr</span></span> | <span data-ttu-id="b5eb2-117">25 %</span><span class="sxs-lookup"><span data-stu-id="b5eb2-117">25%</span></span> | <span data-ttu-id="b5eb2-118">50%</span><span class="sxs-lookup"><span data-stu-id="b5eb2-118">50%</span></span> |
| <span data-ttu-id="b5eb2-119">Spark</span><span class="sxs-lookup"><span data-stu-id="b5eb2-119">spark</span></span> | <span data-ttu-id="b5eb2-120">50%</span><span class="sxs-lookup"><span data-stu-id="b5eb2-120">50%</span></span> | <span data-ttu-id="b5eb2-121">50%</span><span class="sxs-lookup"><span data-stu-id="b5eb2-121">50%</span></span> |

1. <span data-ttu-id="b5eb2-122">Vyberte hello **zobrazení Ambari** ikonu a vyberte hello vzor mřížky.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-122">Select hello **Ambari Views** icon, and then select hello grid pattern.</span></span> <span data-ttu-id="b5eb2-123">Potom vyberte **správce front YARN**.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-123">Next, select **YARN Queue Manager**.</span></span>

    ![Vyberte ikonu pro zobrazení Ambari hello](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. <span data-ttu-id="b5eb2-125">Vyberte hello **výchozí** fronty.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-125">Select hello **default** queue.</span></span>

    ![Vyberte výchozí fronty hello](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. <span data-ttu-id="b5eb2-127">Pro hello **výchozí** fronty, změňte hello **kapacity** z 50 % too25 %.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-127">For hello **default** queue, change hello **capacity** from 50% too25%.</span></span> <span data-ttu-id="b5eb2-128">Pro hello **thriftsvr** fronty, změňte hello **kapacity** too25 %.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-128">For hello **thriftsvr** queue, change hello **capacity** too25%.</span></span>

    ![Změna hello kapacitu too25 % pro hello výchozí a thriftsvr fronty](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. <span data-ttu-id="b5eb2-130">Vyberte toocreate novou frontu, **přidat fronty**.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-130">toocreate a new queue, select **Add Queue**.</span></span>

    ![Vyberte Přidat fronty](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. <span data-ttu-id="b5eb2-132">Název hello novou frontu.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-132">Name hello new queue.</span></span>

    ![Název fronty hello Spark](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. <span data-ttu-id="b5eb2-134">Nechte hello **kapacity** hodnoty na 50 % a pak vyberte hello **akce** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-134">Leave hello **capacity** values at 50%, and then select hello **Actions** button.</span></span>

    ![Kliknutím na tlačítko akce hello](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. <span data-ttu-id="b5eb2-136">Vyberte **uložte a aktualizujte fronty**.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-136">Select **Save and Refresh Queues**.</span></span>

    ![Vyberte uložte a aktualizujte fronty](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

<span data-ttu-id="b5eb2-138">Tyto změny se projeví okamžitě na hello uživatelském rozhraní YARN plánovače.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-138">These changes are visible immediately on hello YARN Scheduler UI.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="b5eb2-139">Další čtení</span><span class="sxs-lookup"><span data-stu-id="b5eb2-139">Additional reading</span></span>

- [<span data-ttu-id="b5eb2-140">YARN CapacityScheduler</span><span class="sxs-lookup"><span data-stu-id="b5eb2-140">YARN CapacityScheduler</span></span>](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a><span data-ttu-id="b5eb2-141">Jak lze stáhnout protokoly YARN z clusteru</span><span class="sxs-lookup"><span data-stu-id="b5eb2-141">How do I download YARN logs from a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="b5eb2-142">Kroky řešení</span><span class="sxs-lookup"><span data-stu-id="b5eb2-142">Resolution steps</span></span> 

1. <span data-ttu-id="b5eb2-143">Připojte toohello clusteru HDInsight pomocí klienta Secure Shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="b5eb2-143">Connect toohello HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="b5eb2-144">Další informace najdete v tématu [další čtení](#additional-reading-2).</span><span class="sxs-lookup"><span data-stu-id="b5eb2-144">For more information, see [Additional reading](#additional-reading-2).</span></span>

2. <span data-ttu-id="b5eb2-145">toolist všechny hello ID aplikace hello YARN aplikací, které jsou aktuálně spuštěny, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-145">toolist all hello application IDs of hello YARN applications that are currently running, run hello following command:</span></span>

    ```apache
    yarn top
    ```
    <span data-ttu-id="b5eb2-146">Hello ID jsou uvedeny v hello **APPLICATIONID** sloupce.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-146">hello IDs are listed in hello **APPLICATIONID** column.</span></span> <span data-ttu-id="b5eb2-147">Protokoly můžete stáhnout z hello **APPLICATIONID** sloupce.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-147">You can download logs from hello **APPLICATIONID** column.</span></span>

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

3. <span data-ttu-id="b5eb2-148">kontejner protokoly YARN toodownload pro všechny servery aplikace, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-148">toodownload YARN container logs for all application masters, use hello following command:</span></span>
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    <span data-ttu-id="b5eb2-149">Tento příkaz vytvoří soubor protokolu s názvem amlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-149">This command creates a log file named amlogs.txt.</span></span> 

4. <span data-ttu-id="b5eb2-150">kontejner protokoly YARN toodownload pro pouze hello nejnovější aplikace hlavní server, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-150">toodownload YARN container logs for only hello latest application master, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    <span data-ttu-id="b5eb2-151">Tento příkaz vytvoří soubor protokolu s názvem latestamlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-151">This command creates a log file named latestamlogs.txt.</span></span> 

4. <span data-ttu-id="b5eb2-152">kontejner protokoly YARN toodownload pro hello první dvě aplikace hlavních serverů, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-152">toodownload YARN container logs for hello first two application masters, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    <span data-ttu-id="b5eb2-153">Tento příkaz vytvoří soubor protokolu s názvem first2amlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-153">This command creates a log file named first2amlogs.txt.</span></span> 

5. <span data-ttu-id="b5eb2-154">použít všechny protokoly YARN kontejneru, toodownload hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-154">toodownload all YARN container logs, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    <span data-ttu-id="b5eb2-155">Tento příkaz vytvoří soubor protokolu s názvem logs.txt.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-155">This command creates a log file named logs.txt.</span></span> 

6. <span data-ttu-id="b5eb2-156">toodownload hello kontejneru protokolu YARN pro specifický kontejner, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-156">toodownload hello YARN container log for a specific container, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    <span data-ttu-id="b5eb2-157">Tento příkaz vytvoří soubor protokolu s názvem containerlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-157">This command creates a log file named containerlogs.txt.</span></span>

### <span data-ttu-id="b5eb2-158"><a name="additional-reading-2"></a>Další čtení</span><span class="sxs-lookup"><span data-stu-id="b5eb2-158"><a name="additional-reading-2"></a>Additional reading</span></span>

- [<span data-ttu-id="b5eb2-159">Připojit tooHDInsight (Hadoop) pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="b5eb2-159">Connect tooHDInsight (Hadoop) by using SSH</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [<span data-ttu-id="b5eb2-160">Apache Hadoop YARN koncepty a aplikací</span><span class="sxs-lookup"><span data-stu-id="b5eb2-160">Apache Hadoop YARN concepts and applications</span></span>](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)







