---
title: aaaStart s Apache Kafka - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toocreate Kafka Apache clusteru v Azure HDInsight. Zjistěte, jak toocreate témata, odběratele a příjemce."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: b93299d88dc2cf9a9764662509308ff75fd74474
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="start-with-apache-kafka-preview-on-hdinsight"></a><span data-ttu-id="82954-104">Začínáme s Apache Kafka (Preview) v prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="82954-104">Start with Apache Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="82954-105">Zjistěte, jak toocreate a používat [Apache Kafka](https://kafka.apache.org) clusteru v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82954-105">Learn how toocreate and use an [Apache Kafka](https://kafka.apache.org) cluster on Azure HDInsight.</span></span> <span data-ttu-id="82954-106">Kafka je opensourcová distribuovaná streamovací platforma, která je dostupná pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82954-106">Kafka is an open-source, distributed streaming platform that is available with HDInsight.</span></span> <span data-ttu-id="82954-107">Často se používá jako zprostředkovatel zprávu, protože poskytuje podobné funkce tooa publikování a odběru fronta zpráv.</span><span class="sxs-lookup"><span data-stu-id="82954-107">It is often used as a message broker, as it provides functionality similar tooa publish-subscribe message queue.</span></span>

> [!NOTE]
> <span data-ttu-id="82954-108">Aktuálně jsou pro HDInsight dostupné dvě verze Kafka: 0.9.0 (HDInsight 3.4) a 0.10.0 (HDInsight 3.5 a 3.6).</span><span class="sxs-lookup"><span data-stu-id="82954-108">There are currently two versions of Kafka available with HDInsight; 0.9.0 (HDInsight 3.4) and 0.10.0 (HDInsight 3.5 and 3.6).</span></span> <span data-ttu-id="82954-109">Hello kroky v tomto dokumentu předpokládají, že používáte Kafka na HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="82954-109">hello steps in this document assume that you are using Kafka on HDInsight 3.6.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a><span data-ttu-id="82954-110">Vytvoření clusteru Kafka</span><span class="sxs-lookup"><span data-stu-id="82954-110">Create a Kafka cluster</span></span>

<span data-ttu-id="82954-111">Použijte následující postup toocreate Kafka na clusteru HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="82954-111">Use hello following steps toocreate a Kafka on HDInsight cluster:</span></span>

1. <span data-ttu-id="82954-112">Z hello [portál Azure](https://portal.azure.com), vyberte **+ nový**, **Intelligence + analýzy**a potom vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="82954-112">From hello [Azure portal](https://portal.azure.com), select **+ NEW**, **Intelligence + Analytics**, and then select **HDInsight**.</span></span>
   
    ![Vytvoření clusteru HDInsight](./media/hdinsight-apache-kafka-get-started/create-hdinsight.png)

2. <span data-ttu-id="82954-114">Z **Základy**, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="82954-114">From **Basics**, enter hello following information:</span></span>

    * <span data-ttu-id="82954-115">**Název clusteru**: název hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82954-115">**Cluster Name**: hello name of hello HDInsight cluster.</span></span>
    * <span data-ttu-id="82954-116">**Předplatné**: Vyberte předplatné toouse hello.</span><span class="sxs-lookup"><span data-stu-id="82954-116">**Subscription**: Select hello subscription toouse.</span></span>
    * <span data-ttu-id="82954-117">**Uživatelské jméno přihlášení clusteru** a **clusteru přihlašovacího hesla**: hello přihlášení při přístupu k hello clusteru přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="82954-117">**Cluster login username** and **Cluster login password**: hello login when accessing hello cluster over HTTPS.</span></span> <span data-ttu-id="82954-118">Tyto přihlašovací údaje služby tooaccess například hello webové uživatelské rozhraní Ambari nebo REST API používáte.</span><span class="sxs-lookup"><span data-stu-id="82954-118">You use these credentials tooaccess services such as hello Ambari Web UI or REST API.</span></span>
    * <span data-ttu-id="82954-119">**Secure Shell (SSH) username**: hello přihlášení, které se používá při získávání hello clusteru prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="82954-119">**Secure Shell (SSH) username**: hello login used when accessing hello cluster over SSH.</span></span> <span data-ttu-id="82954-120">Ve výchozím nastavení je hello hello heslo stejné jako heslo pro přihlášení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="82954-120">By default hello password is hello same as hello cluster login password.</span></span>
    * <span data-ttu-id="82954-121">**Skupina prostředků**: hello prostředků skupiny toocreate hello clusteru v.</span><span class="sxs-lookup"><span data-stu-id="82954-121">**Resource Group**: hello resource group toocreate hello cluster in.</span></span>
    * <span data-ttu-id="82954-122">**Umístění**: hello oblast Azure toocreate hello clusteru v.</span><span class="sxs-lookup"><span data-stu-id="82954-122">**Location**: hello Azure region toocreate hello cluster in.</span></span>
   
 ![Výběr předplatného](./media/hdinsight-apache-kafka-get-started/hdinsight-basic-configuration.png)

3. <span data-ttu-id="82954-124">Vyberte **clusteru typ**, a pak sadu hello následující hodnoty z **konfigurace clusteru**:</span><span class="sxs-lookup"><span data-stu-id="82954-124">Select **Cluster type**, and then set hello following values from **Cluster configuration**:</span></span>
   
    * <span data-ttu-id="82954-125">**Typ clusteru:** Kafka</span><span class="sxs-lookup"><span data-stu-id="82954-125">**Cluster Type**: Kafka</span></span>

    * <span data-ttu-id="82954-126">**Verze:** Kafka 0.10.0 (HDI 3.6)</span><span class="sxs-lookup"><span data-stu-id="82954-126">**Version**: Kafka 0.10.0 (HDI 3.6)</span></span>

    * <span data-ttu-id="82954-127">**Úroveň clusteru:** Standard</span><span class="sxs-lookup"><span data-stu-id="82954-127">**Cluster Tier**: Standard</span></span>
     
 <span data-ttu-id="82954-128">Nakonec použijte hello **vyberte** tlačítko toosave nastavení.</span><span class="sxs-lookup"><span data-stu-id="82954-128">Finally, use hello **Select** button toosave settings.</span></span>
     
 ![Výběr typu clusteru](./media/hdinsight-apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. <span data-ttu-id="82954-130">Po výběru typu hello clusteru, použijte hello __vyberte__ tooset hello clusteru typ tlačítka.</span><span class="sxs-lookup"><span data-stu-id="82954-130">After selecting hello cluster type, use hello __Select__ button tooset hello cluster type.</span></span> <span data-ttu-id="82954-131">Pak pomocí hello __Další__ tlačítko toofinish základní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="82954-131">Next, use hello __Next__ button toofinish basic configuration.</span></span>

5. <span data-ttu-id="82954-132">V části **Úložiště** vyberte nebo vytvořte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="82954-132">From **Storage**, select or create a Storage account.</span></span> <span data-ttu-id="82954-133">Hello kroky v tomto dokumentu ponechte hello ostatní pole na výchozí hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="82954-133">For hello steps in this document, leave hello other fields at hello default values.</span></span> <span data-ttu-id="82954-134">Použití hello __Další__ tlačítko toosave úložiště konfigurace.</span><span class="sxs-lookup"><span data-stu-id="82954-134">Use hello __Next__ button toosave storage configuration.</span></span>

    ![Nastavit hello nastavení účtu úložiště pro HDInsight](./media/hdinsight-apache-kafka-get-started/set-hdinsight-storage-account.png)

6. <span data-ttu-id="82954-136">Z __aplikace (volitelné)__, vyberte __Další__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="82954-136">From __Applications (optional)__, select __Next__ toocontinue.</span></span> <span data-ttu-id="82954-137">Pro tento příklad se nepožadují žádné aplikace.</span><span class="sxs-lookup"><span data-stu-id="82954-137">No applications are required for this example.</span></span>

7. <span data-ttu-id="82954-138">Z __velikost clusteru__, vyberte __Další__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="82954-138">From __Cluster size__, select __Next__ toocontinue.</span></span>

    > [!WARNING]
    > <span data-ttu-id="82954-139">dostupnost tooguarantee Kafka v HDInsight, cluster musí obsahovat aspoň tři uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="82954-139">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span>

    ![Sada hello Kafka velikost clusteru](./media/hdinsight-apache-kafka-get-started/kafka-cluster-size.png)

    > [!NOTE]
    > <span data-ttu-id="82954-141">Hello **disků na pracovního uzlu** vstupní ovládací prvky hello škálovatelnost Kafka v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82954-141">hello **disks per worker node** entry controls hello scalability of Kafka on HDInsight.</span></span> <span data-ttu-id="82954-142">Další informace najdete v tématu věnovaném [konfiguraci úložiště a škálovatelnosti Kafka v HDInsightu](hdinsight-apache-kafka-scalability.md).</span><span class="sxs-lookup"><span data-stu-id="82954-142">For more information, see [Configure storage and scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md).</span></span>

8. <span data-ttu-id="82954-143">Z __upřesňující nastavení__, vyberte __Další__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="82954-143">From __Advanced settings__, select __Next__ toocontinue.</span></span>

9. <span data-ttu-id="82954-144">Z hello **Souhrn**, zkontrolujte konfiguraci hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="82954-144">From hello **Summary**, review hello configuration for hello cluster.</span></span> <span data-ttu-id="82954-145">Použití hello __upravit__ odkazy toochange všechna nastavení, která jsou nesprávné.</span><span class="sxs-lookup"><span data-stu-id="82954-145">Use hello __Edit__ links toochange any settings that are incorrect.</span></span> <span data-ttu-id="82954-146">Nakonec použijte the__Create__ tlačítko toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="82954-146">Finally, use the__Create__ button toocreate hello cluster.</span></span>
   
    ![Souhrn konfigurace clusteru](./media/hdinsight-apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > <span data-ttu-id="82954-148">To může trvat až too20 minut toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="82954-148">It can take up too20 minutes toocreate hello cluster.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="82954-149">Připojte toohello cluster</span><span class="sxs-lookup"><span data-stu-id="82954-149">Connect toohello cluster</span></span>

> [!IMPORTANT]
> <span data-ttu-id="82954-150">Při provádění hello následující kroky, je potřeba použít klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="82954-150">When performing hello following steps, you must use an SSH client.</span></span> <span data-ttu-id="82954-151">Další informace najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82954-151">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

<span data-ttu-id="82954-152">Z vašeho klienta pomocí protokolu SSH tooconnect toohello clusteru:</span><span class="sxs-lookup"><span data-stu-id="82954-152">From your client, use SSH tooconnect toohello cluster:</span></span>

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

<span data-ttu-id="82954-153">Nahraďte **SSHUSER** s uživatelským jménem SSH hello, které jste zadali při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="82954-153">Replace **SSHUSER** with hello SSH username you provided during cluster creation.</span></span> <span data-ttu-id="82954-154">Nahraďte **CLUSTERNAME** s názvem hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="82954-154">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>

<span data-ttu-id="82954-155">Po zobrazení výzvy zadejte heslo hello, která jste použili pro hello účtu SSH.</span><span class="sxs-lookup"><span data-stu-id="82954-155">When prompted, enter hello password you used for hello SSH account.</span></span>

<span data-ttu-id="82954-156">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="82954-156">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="82954-157"><a id="getkafkainfo"></a>Získat informace o hostiteli hello Zookeeper a zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="82954-157"><a id="getkafkainfo"></a>Get hello Zookeeper and Broker host information</span></span>

<span data-ttu-id="82954-158">Při práci s Kafka, musíte znát dvě hodnoty hostitele; Hello *Zookeeper* hostitelů a hello *zprostředkovatele* hostitele.</span><span class="sxs-lookup"><span data-stu-id="82954-158">When working with Kafka, you must know two host values; hello *Zookeeper* hosts and hello *Broker* hosts.</span></span> <span data-ttu-id="82954-159">Tito hostitelé se používají s hello Kafka rozhraní API a řadu hello nástroje, které jsou dodávány s Kafka.</span><span class="sxs-lookup"><span data-stu-id="82954-159">These hosts are used with hello Kafka API and many of hello utilities that ship with Kafka.</span></span>

<span data-ttu-id="82954-160">Použijte následující proměnné prostředí toocreate kroky, které obsahují informace o hostiteli hello hello.</span><span class="sxs-lookup"><span data-stu-id="82954-160">Use hello following steps toocreate environment variables that contain hello host information.</span></span> <span data-ttu-id="82954-161">Tyto proměnné prostředí se používají v hello kroky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82954-161">These environment variables are used in hello steps in this document.</span></span>

1. <span data-ttu-id="82954-162">Z clusteru toohello připojení SSH, použijte následující hello příkaz tooinstall hello `jq` nástroj.</span><span class="sxs-lookup"><span data-stu-id="82954-162">From an SSH connection toohello cluster, use hello following command tooinstall hello `jq` utility.</span></span> <span data-ttu-id="82954-163">Tento nástroj je použité tooparse dokumentů JSON a je užitečný při načítání informací o hostiteli hello zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="82954-163">This utility is used tooparse JSON documents, and is useful in retrieving hello broker host information:</span></span>
   
    ```bash
    sudo apt -y install jq
    ```

2. <span data-ttu-id="82954-164">proměnné prostředí hello tooset s informacemi o načítají Ambari hello použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="82954-164">tooset hello environment variables with information retrieved from Ambari, use hello following commands:</span></span>

    ```bash
    CLUSTERNAME='your cluster name'
    PASSWORD='your cluster password'
    export KAFKAZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="82954-165">Nastavit `CLUSTERNAME=` toohello název hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="82954-165">Set `CLUSTERNAME=` toohello name of hello Kafka cluster.</span></span> <span data-ttu-id="82954-166">Nastavit `PASSWORD=` toohello heslo pro přihlášení (správce) můžete použít při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="82954-166">Set `PASSWORD=` toohello login (admin) password you used when creating hello cluster.</span></span>

    <span data-ttu-id="82954-167">Hello následující text je příkladem hello obsah `$KAFKAZKHOSTS`:</span><span class="sxs-lookup"><span data-stu-id="82954-167">hello following text is an example of hello contents of `$KAFKAZKHOSTS`:</span></span>
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    <span data-ttu-id="82954-168">Hello následující text je příkladem hello obsah `$KAFKABROKERS`:</span><span class="sxs-lookup"><span data-stu-id="82954-168">hello following text is an example of hello contents of `$KAFKABROKERS`:</span></span>
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > <span data-ttu-id="82954-169">Hello `cut` příkaz je použité tootrim hello seznam záznamy hostitele tootwo hostitele.</span><span class="sxs-lookup"><span data-stu-id="82954-169">hello `cut` command is used tootrim hello list of hosts tootwo host entries.</span></span> <span data-ttu-id="82954-170">Při vytváření příjemce Kafka nebo proto, že nepotřebujete tooprovide hello úplný seznam hostitelů.</span><span class="sxs-lookup"><span data-stu-id="82954-170">You do not need tooprovide hello full list of hosts when creating a Kafka consumer or producer.</span></span>
   
    > [!WARNING]
    > <span data-ttu-id="82954-171">Nespoléhejte na hello vrácené informace z této relace tooalways být přesná.</span><span class="sxs-lookup"><span data-stu-id="82954-171">Do not rely on hello information returned from this session tooalways be accurate.</span></span> <span data-ttu-id="82954-172">Pokud jste škálování hello clusteru, nové zprostředkovatelé přidání nebo odebrání.</span><span class="sxs-lookup"><span data-stu-id="82954-172">If you scale hello cluster, new brokers are added or removed.</span></span> <span data-ttu-id="82954-173">Pokud dojde k chybě a je nahrazený uzlu, může změnit název hostitele hello hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="82954-173">If a failure occurs and a node is replaced, hello host name for hello node may change.</span></span>
    >
    > <span data-ttu-id="82954-174">Měli načítat informace hello Zookeeper a zprostředkovatele hostitelů krátce před použít tooensure máte platné informace.</span><span class="sxs-lookup"><span data-stu-id="82954-174">You should retrieve hello Zookeeper and broker hosts information shortly before you use it tooensure you have valid information.</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="82954-175">Vytvoření tématu</span><span class="sxs-lookup"><span data-stu-id="82954-175">Create a topic</span></span>

<span data-ttu-id="82954-176">Kafka ukládá datové proudy do kategorií označovaných jako *témata*.</span><span class="sxs-lookup"><span data-stu-id="82954-176">Kafka stores streams of data in categories called *topics*.</span></span> <span data-ttu-id="82954-177">SSH připojení tooa clusteru headnode můžete skript, který se Kafka toocreate téma:</span><span class="sxs-lookup"><span data-stu-id="82954-177">From An SSH connection tooa cluster headnode, use a script provided with Kafka toocreate a topic:</span></span>

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

<span data-ttu-id="82954-178">Tento příkaz se připojí pomocí hello hostitele informace uložené v tooZookeeper `$KAFKAZKHOSTS`a poté vytvořit téma Kafka s názvem **testování**.</span><span class="sxs-lookup"><span data-stu-id="82954-178">This command connects tooZookeeper using hello host information stored in `$KAFKAZKHOSTS`, and then create Kafka topic named **test**.</span></span> <span data-ttu-id="82954-179">Můžete ověřit, že tohoto tématu hello byla vytvořena pomocí hello následující skript toolist témata:</span><span class="sxs-lookup"><span data-stu-id="82954-179">You can verify that hello topic was created by using hello following script toolist topics:</span></span>

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

<span data-ttu-id="82954-180">Hello výstup tohoto příkazu obsahuje seznam Kafka témat, která obsahuje hello **testování** tématu.</span><span class="sxs-lookup"><span data-stu-id="82954-180">hello output of this command lists Kafka topics, which contains hello **test** topic.</span></span>

## <a name="produce-and-consume-records"></a><span data-ttu-id="82954-181">Produkce a konzumace záznamů</span><span class="sxs-lookup"><span data-stu-id="82954-181">Produce and consume records</span></span>

<span data-ttu-id="82954-182">Kafka ukládá *záznamy* v tématech.</span><span class="sxs-lookup"><span data-stu-id="82954-182">Kafka stores *records* in topics.</span></span> <span data-ttu-id="82954-183">Záznamy jsou vytvářeny *producenty* a spotřebovávány *konzumenty*.</span><span class="sxs-lookup"><span data-stu-id="82954-183">Records are produced by *producers*, and consumed by *consumers*.</span></span> <span data-ttu-id="82954-184">Producenti načítají záznamy ze *zprostředkovatelů* Kafka.</span><span class="sxs-lookup"><span data-stu-id="82954-184">Producers retrieve records from Kafka *brokers*.</span></span> <span data-ttu-id="82954-185">Každý pracovní uzel v clusteru HDInsight je zprostředkovatelem Kafka.</span><span class="sxs-lookup"><span data-stu-id="82954-185">Each worker node in your HDInsight cluster is a Kafka broker.</span></span>

<span data-ttu-id="82954-186">Použijte následující postup toostore záznamy do téma test hello jste vytvořili dříve a pak je pomocí příjemce číst hello:</span><span class="sxs-lookup"><span data-stu-id="82954-186">Use hello following steps toostore records into hello test topic you created earlier, and then read them using a consumer:</span></span>

1. <span data-ttu-id="82954-187">Z relace SSH hello použijte skript, který se Kafka toowrite záznamy toohello tématu:</span><span class="sxs-lookup"><span data-stu-id="82954-187">From hello SSH session, use a script provided with Kafka toowrite records toohello topic:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    <span data-ttu-id="82954-188">Výzva toohello nebudou zobrazeny po tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="82954-188">You are not returned toohello prompt after this command.</span></span> <span data-ttu-id="82954-189">Místo toho zadejte několik textové zprávy a pak použijte **kombinaci kláves Ctrl + C** toostop odesílání toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="82954-189">Instead, type a few text messages and then use **Ctrl + C** toostop sending toohello topic.</span></span> <span data-ttu-id="82954-190">Každý řádek se odešle jako samostatný záznam.</span><span class="sxs-lookup"><span data-stu-id="82954-190">Each line is sent as a separate record.</span></span>

2. <span data-ttu-id="82954-191">Použijte skript, který se záznamy tooread Kafka hello tématu:</span><span class="sxs-lookup"><span data-stu-id="82954-191">Use a script provided with Kafka tooread records from hello topic:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    <span data-ttu-id="82954-192">Tento příkaz načte hello záznamů z tématu hello a zobrazí je.</span><span class="sxs-lookup"><span data-stu-id="82954-192">This command retrieves hello records from hello topic and displays them.</span></span> <span data-ttu-id="82954-193">Pomocí `--from-beginning` informuje hello příjemce toostart od začátku hello hello datového proudu, proto jsou načteny všechny záznamy.</span><span class="sxs-lookup"><span data-stu-id="82954-193">Using `--from-beginning` tells hello consumer toostart from hello beginning of hello stream, so all records are retrieved.</span></span>

3. <span data-ttu-id="82954-194">Použití __kombinaci kláves Ctrl + C__ toostop hello příjemce.</span><span class="sxs-lookup"><span data-stu-id="82954-194">Use __Ctrl + C__ toostop hello consumer.</span></span>

## <a name="producer-and-consumer-api"></a><span data-ttu-id="82954-195">Rozhraní API pro producenta a konzumenta</span><span class="sxs-lookup"><span data-stu-id="82954-195">Producer and consumer API</span></span>

<span data-ttu-id="82954-196">Můžete také programově vytvářet a využívat záznamů pomocí hello [rozhraní API Kafka](http://kafka.apache.org/documentation#api).</span><span class="sxs-lookup"><span data-stu-id="82954-196">You can also programmatically produce and consume records using hello [Kafka APIs](http://kafka.apache.org/documentation#api).</span></span> <span data-ttu-id="82954-197">toobuild producent Java a příjemce, použijte hello postupem z vývojového prostředí.</span><span class="sxs-lookup"><span data-stu-id="82954-197">toobuild a Java producer and consumer, use hello following steps from your development environment.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="82954-198">Musíte mít hello následující součásti nainstalované ve vašem vývojovém prostředí:</span><span class="sxs-lookup"><span data-stu-id="82954-198">You must have hello following components installed in your development environment:</span></span>
>
> * <span data-ttu-id="82954-199">[Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) nebo ekvivalentní, například OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="82954-199">[Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or an equivalent, such as OpenJDK.</span></span>
>
> * [<span data-ttu-id="82954-200">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="82954-200">Apache Maven</span></span>](http://maven.apache.org/)
>
> * <span data-ttu-id="82954-201">Klient SSH a hello `scp` příkaz.</span><span class="sxs-lookup"><span data-stu-id="82954-201">An SSH client and hello `scp` command.</span></span> <span data-ttu-id="82954-202">Další informace najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82954-202">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

1. <span data-ttu-id="82954-203">Stáhnout hello příklady z [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).</span><span class="sxs-lookup"><span data-stu-id="82954-203">Download hello examples from [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).</span></span> <span data-ttu-id="82954-204">Například hello producent/příjemce použití hello projektu v hello `Producer-Consumer` adresáře.</span><span class="sxs-lookup"><span data-stu-id="82954-204">For hello producer/consumer example, use hello project in hello `Producer-Consumer` directory.</span></span> <span data-ttu-id="82954-205">Tento příklad obsahuje hello následující třídy:</span><span class="sxs-lookup"><span data-stu-id="82954-205">This example contains hello following classes:</span></span>
   
    * <span data-ttu-id="82954-206">**Spustit** -spustí hello příjemce nebo výrobce.</span><span class="sxs-lookup"><span data-stu-id="82954-206">**Run** - starts either hello consumer or producer.</span></span>

    * <span data-ttu-id="82954-207">**Producent** -tématu toohello úložiště 1 000 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="82954-207">**Producer** - stores 1,000,000 records toohello topic.</span></span>

    * <span data-ttu-id="82954-208">**Příjemce** -čte záznamů z tématu hello.</span><span class="sxs-lookup"><span data-stu-id="82954-208">**Consumer** - reads records from hello topic.</span></span>

2. <span data-ttu-id="82954-209">umístění adresáře toohello hello změnit toocreate jar balíčku, `Producer-Consumer` hello adresáře a použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="82954-209">toocreate a jar package, change directories toohello location of hello `Producer-Consumer` directory and use hello following command:</span></span>

    ```
    mvn clean package
    ```

    <span data-ttu-id="82954-210">Tento příkaz vytvoří adresář s názvem `target`, který bude obsahovat soubor s názvem `kafka-producer-consumer-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="82954-210">This command creates a directory named `target`, that contains a file named `kafka-producer-consumer-1.0-SNAPSHOT.jar`.</span></span>

3. <span data-ttu-id="82954-211">Použití hello následující příkazy toocopy hello `kafka-producer-consumer-1.0-SNAPSHOT.jar` clusteru HDInsight tooyour souboru:</span><span class="sxs-lookup"><span data-stu-id="82954-211">Use hello following commands toocopy hello `kafka-producer-consumer-1.0-SNAPSHOT.jar` file tooyour HDInsight cluster:</span></span>
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    <span data-ttu-id="82954-212">Nahraďte **SSHUSER** hello uživatele SSH pro váš cluster a nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="82954-212">Replace **SSHUSER** with hello SSH user for your cluster, and replace **CLUSTERNAME** with hello name of your cluster.</span></span> <span data-ttu-id="82954-213">Po zobrazení výzvy zadejte hello heslo pro uživatele SSH hello.</span><span class="sxs-lookup"><span data-stu-id="82954-213">When prompted enter hello password for hello SSH user.</span></span>

4. <span data-ttu-id="82954-214">Jednou hello `scp` příkaz dokončení kopírování hello souboru, připojit toohello clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="82954-214">Once hello `scp` command finishes copying hello file, connect toohello cluster using SSH.</span></span> <span data-ttu-id="82954-215">Použijte následující příkaz toowrite záznamy toohello test tématu hello:</span><span class="sxs-lookup"><span data-stu-id="82954-215">Use hello following command toowrite records toohello test topic:</span></span>

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

5. <span data-ttu-id="82954-216">Po dokončení procesu hello, použijte následující příkaz tooread hello tématu hello:</span><span class="sxs-lookup"><span data-stu-id="82954-216">Once hello process has finished, use hello following command tooread from hello topic:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    <span data-ttu-id="82954-217">Zobrazí se záznamy Hello číst, spolu s počtem záznamů.</span><span class="sxs-lookup"><span data-stu-id="82954-217">hello records read, along with a count of records, is displayed.</span></span> <span data-ttu-id="82954-218">Může se zobrazit několik více než 1 000 000 protokolována jako jste poslali několik záznamů toohello tématu pomocí skriptu v jednom z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="82954-218">You may see a few more than 1,000,000 logged as you sent several records toohello topic using a script in an earlier step.</span></span>

6. <span data-ttu-id="82954-219">Použití __kombinaci kláves Ctrl + C__ tooexit hello příjemce.</span><span class="sxs-lookup"><span data-stu-id="82954-219">Use __Ctrl + C__ tooexit hello consumer.</span></span>

### <a name="multiple-consumers"></a><span data-ttu-id="82954-220">Víc současných konzumentů</span><span class="sxs-lookup"><span data-stu-id="82954-220">Multiple consumers</span></span>

<span data-ttu-id="82954-221">Konzumenti Kafka při čtení záznamů používají skupiny konzumentů.</span><span class="sxs-lookup"><span data-stu-id="82954-221">Kafka consumers use a consumer group when reading records.</span></span> <span data-ttu-id="82954-222">Pomocí více příjemců hello stejné skupiny má za následek zatížení vyvážit čtení z tématu.</span><span class="sxs-lookup"><span data-stu-id="82954-222">Using hello same group with multiple consumers results in load balanced reads from a topic.</span></span> <span data-ttu-id="82954-223">Každý příjemce ve skupině hello obdrží část hello záznamy.</span><span class="sxs-lookup"><span data-stu-id="82954-223">Each consumer in hello group receives a portion of hello records.</span></span> <span data-ttu-id="82954-224">toosee, který tento proces v akci, hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="82954-224">toosee this process in action, use hello following steps:</span></span>

1. <span data-ttu-id="82954-225">Otevřete nový SSH relace toohello cluster tak, že máte dva z nich.</span><span class="sxs-lookup"><span data-stu-id="82954-225">Open a new SSH session toohello cluster, so that you have two of them.</span></span> <span data-ttu-id="82954-226">V každé relaci použít hello následující toostart hello příjemce se stejným ID skupiny příjemců:</span><span class="sxs-lookup"><span data-stu-id="82954-226">In each session, use hello following toostart a consumer with hello same consumer group ID:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    <span data-ttu-id="82954-227">Tento příkaz spustí příjemce pomocí ID skupiny hello `mygroup`.</span><span class="sxs-lookup"><span data-stu-id="82954-227">This command starts a consumer using hello group ID `mygroup`.</span></span>

    > [!NOTE]
    > <span data-ttu-id="82954-228">Používání příkazů hello v hello [získat informace o hostiteli hello Zookeeper a zprostředkovatele](#getkafkainfo) části tooset `$KAFKABROKERS` pro tuto relaci SSH.</span><span class="sxs-lookup"><span data-stu-id="82954-228">Use hello commands in hello [Get hello Zookeeper and Broker host information](#getkafkainfo) section tooset `$KAFKABROKERS` for this SSH session.</span></span>

2. <span data-ttu-id="82954-229">Podívejte se na jako každou záznamy hello počty relace, který obdrží z tématu hello.</span><span class="sxs-lookup"><span data-stu-id="82954-229">Watch as each session counts hello records it receives from hello topic.</span></span> <span data-ttu-id="82954-230">Hello celkem obě relace by měl být hello stejné, jako jste dříve získali od jednoho příjemce.</span><span class="sxs-lookup"><span data-stu-id="82954-230">hello total of both sessions should be hello same as you received previously from one consumer.</span></span>

<span data-ttu-id="82954-231">Spotřeba klienty během hello stejnou skupinu je zpracováván prostřednictvím hello oddílů pro téma hello.</span><span class="sxs-lookup"><span data-stu-id="82954-231">Consumption by clients within hello same group is handled through hello partitions for hello topic.</span></span> <span data-ttu-id="82954-232">Pro hello `test` tématu vytvořený, má osm oddíl.</span><span class="sxs-lookup"><span data-stu-id="82954-232">For hello `test` topic created earlier, it has eight partitions.</span></span> <span data-ttu-id="82954-233">Pokud otevřete osm relace SSH a spouštět příjemce v všechny relace, každý příjemce čte záznamů z jednoho oddílu pro téma hello.</span><span class="sxs-lookup"><span data-stu-id="82954-233">If you open eight SSH sessions and launch a consumer in all sessions, each consumer reads records from a single partition for hello topic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="82954-234">Ve skupině příjemců nemůže být víc instancí konzumentů než má téma oddílů.</span><span class="sxs-lookup"><span data-stu-id="82954-234">There cannot be more consumer instances in a consumer group than partitions.</span></span> <span data-ttu-id="82954-235">V tomto příkladu jeden příjemce skupina může obsahovat až tooeight příjemci vzhledem k tomu, který je hello počet oddílů v tématu hello.</span><span class="sxs-lookup"><span data-stu-id="82954-235">In this example, one consumer group can contain up tooeight consumers since that is hello number of partitions in hello topic.</span></span> <span data-ttu-id="82954-236">Nebo můžete mít více skupin konzumentů, každou s maximálně osmi konzumenty.</span><span class="sxs-lookup"><span data-stu-id="82954-236">Or you can have multiple consumer groups, each with no more than eight consumers.</span></span>

<span data-ttu-id="82954-237">Záznamy, které jsou uložené v Kafka se ukládají do hello pořadí, ve kterém jsou přijaty v rámci oddílu.</span><span class="sxs-lookup"><span data-stu-id="82954-237">Records stored in Kafka are stored in hello order they are received within a partition.</span></span> <span data-ttu-id="82954-238">tooachieve v seřazené doručení pro záznamy *v rámci oddílu*, vytvořte skupinu uživatelů, kde hello počet instancí příjemce odpovídá hello počet oddílů.</span><span class="sxs-lookup"><span data-stu-id="82954-238">tooachieve in-ordered delivery for records *within a partition*, create a consumer group where hello number of consumer instances matches hello number of partitions.</span></span> <span data-ttu-id="82954-239">tooachieve v seřazené doručení pro záznamy *v rámci tématu hello*, vytvořte skupinu uživatelů s instancí jenom jednoho příjemce.</span><span class="sxs-lookup"><span data-stu-id="82954-239">tooachieve in-ordered delivery for records *within hello topic*, create a consumer group with only one consumer instance.</span></span>

## <a name="streaming-api"></a><span data-ttu-id="82954-240">API pro streamování</span><span class="sxs-lookup"><span data-stu-id="82954-240">Streaming API</span></span>

<span data-ttu-id="82954-241">Hello streamování rozhraní API se přidal ve verzi 0.10.0; tooKafka starší verze spoléhají na Apache Spark nebo Storm pro zpracování datového proudu.</span><span class="sxs-lookup"><span data-stu-id="82954-241">hello streaming API was added tooKafka in version 0.10.0; earlier versions rely on Apache Spark or Storm for stream processing.</span></span>

1. <span data-ttu-id="82954-242">Pokud jste tak již neučinili, stáhněte si hello příklady z [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) tooyour vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="82954-242">If you haven't already done so, download hello examples from [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) tooyour development environment.</span></span> <span data-ttu-id="82954-243">Pro hello streamování příklad, použití hello projektu v hello `streaming` adresáře.</span><span class="sxs-lookup"><span data-stu-id="82954-243">For hello streaming example, use hello project in hello `streaming` directory.</span></span>
   
    <span data-ttu-id="82954-244">Tento projekt obsahuje pouze jednu třídu, `Stream`, který čte záznamů z hello `test` téma, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="82954-244">This project contains only one class, `Stream`, which reads records from hello `test` topic created previously.</span></span> <span data-ttu-id="82954-245">Počty slov hello číst a vysílá každého slova a počet tooa tématu, s názvem `wordcounts`.</span><span class="sxs-lookup"><span data-stu-id="82954-245">It counts hello words read, and emits each word and count tooa topic named `wordcounts`.</span></span> <span data-ttu-id="82954-246">Hello `wordcounts` tématu se vytvoří v pozdější fázi v této části.</span><span class="sxs-lookup"><span data-stu-id="82954-246">hello `wordcounts` topic is created in a later step in this section.</span></span>

2. <span data-ttu-id="82954-247">Z příkazového řádku hello ve vašem vývojovém prostředí, změnit umístění adresáře toohello hello `Streaming` adresář a potom hello použijte následující příkaz toocreate jar balíčku:</span><span class="sxs-lookup"><span data-stu-id="82954-247">From hello command line in your development environment, change directories toohello location of hello `Streaming` directory, and then use hello following command toocreate a jar package:</span></span>

    ```bash
    mvn clean package
    ```

    <span data-ttu-id="82954-248">Tento příkaz vytvoří adresář s názvem `target`, který bude obsahovat soubor s názvem `kafka-streaming-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="82954-248">This command creates a directory named `target`, which contains a file named `kafka-streaming-1.0-SNAPSHOT.jar`.</span></span>

3. <span data-ttu-id="82954-249">Použití hello následující příkazy toocopy hello `kafka-streaming-1.0-SNAPSHOT.jar` clusteru HDInsight tooyour souboru:</span><span class="sxs-lookup"><span data-stu-id="82954-249">Use hello following commands toocopy hello `kafka-streaming-1.0-SNAPSHOT.jar` file tooyour HDInsight cluster:</span></span>
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    <span data-ttu-id="82954-250">Nahraďte **SSHUSER** hello uživatele SSH pro váš cluster a nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="82954-250">Replace **SSHUSER** with hello SSH user for your cluster, and replace **CLUSTERNAME** with hello name of your cluster.</span></span> <span data-ttu-id="82954-251">Po zobrazení výzvy zadejte hello heslo pro uživatele SSH hello.</span><span class="sxs-lookup"><span data-stu-id="82954-251">When prompted enter hello password for hello SSH user.</span></span>

4. <span data-ttu-id="82954-252">Jednou hello `scp` příkaz dokončení kopírování hello souboru, připojit toohello clusteru pomocí protokolu SSH a pak použijte následující příkaz toocreate hello hello `wordcounts` tématu:</span><span class="sxs-lookup"><span data-stu-id="82954-252">Once hello `scp` command finishes copying hello file, connect toohello cluster using SSH, and then use hello following command toocreate hello `wordcounts` topic:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. <span data-ttu-id="82954-253">V dalším kroku spuštění hello vysílání datového proudu proces použitím hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="82954-253">Next, start hello streaming process by using hello following command:</span></span>
   
    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    <span data-ttu-id="82954-254">Tento příkaz spustí hello streamování proces hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="82954-254">This command starts hello streaming process in hello background.</span></span>

6. <span data-ttu-id="82954-255">Použití hello následující příkaz toosend zprávy toohello `test` tématu.</span><span class="sxs-lookup"><span data-stu-id="82954-255">Use hello following command toosend messages toohello `test` topic.</span></span> <span data-ttu-id="82954-256">Tyto zprávy jsou zpracovávány hello streamování příklad:</span><span class="sxs-lookup"><span data-stu-id="82954-256">These messages are processed by hello streaming example:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. <span data-ttu-id="82954-257">Hello použijte následující příkaz tooview hello výstupu, který je zapsán toohello `wordcounts` téma podle hello streamování proces:</span><span class="sxs-lookup"><span data-stu-id="82954-257">Use hello following command tooview hello output that is written toohello `wordcounts` topic by hello streaming process:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > <span data-ttu-id="82954-258">tooview hello data, musíte upozornit hello příjemce tooprint hello klíč a hello deserializátor toouse pro hello klíč a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="82954-258">tooview hello data, you must tell hello consumer tooprint hello key and hello deserializer toouse for hello key and value.</span></span> <span data-ttu-id="82954-259">Název klíče Hello je hello word a hodnota klíče hello obsahuje počet hello.</span><span class="sxs-lookup"><span data-stu-id="82954-259">hello key name is hello word, and hello key value contains hello count.</span></span>
   
    <span data-ttu-id="82954-260">Hello výstup je podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="82954-260">hello output is similar toohello following text:</span></span>
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
        a       13805
        snow    13637
   
    > [!NOTE]
    > <span data-ttu-id="82954-261">počet Hello zvýší pokaždé, když je zjištěna.</span><span class="sxs-lookup"><span data-stu-id="82954-261">hello count increments each time a word is encountered.</span></span>

7. <span data-ttu-id="82954-262">Použít hello __kombinaci kláves Ctrl + C__ tooexit hello příjemce, a pak použijte hello `fg` příkaz toobring hello streamování pozadí úloh back toohello popředí.</span><span class="sxs-lookup"><span data-stu-id="82954-262">Use hello __Ctrl + C__ tooexit hello consumer, then use hello `fg` command toobring hello streaming background task back toohello foreground.</span></span> <span data-ttu-id="82954-263">Použití __kombinaci kláves Ctrl + C__ tooexit jej také.</span><span class="sxs-lookup"><span data-stu-id="82954-263">Use __Ctrl + C__ tooexit it also.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="82954-264">Odstranění clusteru hello</span><span class="sxs-lookup"><span data-stu-id="82954-264">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="82954-265">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="82954-265">Troubleshoot</span></span>

<span data-ttu-id="82954-266">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="82954-266">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="82954-267">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82954-267">Next steps</span></span>

<span data-ttu-id="82954-268">V tomto dokumentu jste se naučili základy hello pracovat s Apache Kafka v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82954-268">In this document, you have learned hello basics of working with Apache Kafka on HDInsight.</span></span> <span data-ttu-id="82954-269">Použijte následující informace o práci s Kafka toolearn hello:</span><span class="sxs-lookup"><span data-stu-id="82954-269">Use hello following toolearn more about working with Kafka:</span></span>

* [<span data-ttu-id="82954-270">Zajištění vysoké dostupnosti dat s využitím Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="82954-270">Ensure high availability of your data with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-high-availability.md)
* [<span data-ttu-id="82954-271">Zvýšení škálovatelnosti konfigurací spravovaných disků s využitím Kafka v HDInsightu</span><span class="sxs-lookup"><span data-stu-id="82954-271">Increase scalability by configuring managed disks with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-scalability.md)
* <span data-ttu-id="82954-272">[Dokumentace Apache Kafka](http://kafka.apache.org/documentation.html) na webu kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="82954-272">[Apache Kafka documentation](http://kafka.apache.org/documentation.html) at kafka.apache.org.</span></span>
* [<span data-ttu-id="82954-273">Použít MirrorMaker toocreate repliku Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="82954-273">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="82954-274">Použití Apache Stormu se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="82954-274">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="82954-275">Použití Apache Sparku se systémem Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="82954-275">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="82954-276">Připojení tooKafka přes virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="82954-276">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
