---
title: "Tipy pro používání Hadoop v HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Získáte implementace tipy pro použití systémem Linux HDInsight (Hadoop) clusterů ve známém prostředí Linux spuštěné v cloudu Azure."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c41c611c-5798-4c14-81cc-bed1e26b5609
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 8c6ff4a6b8617cda9b12be060c7c7bed62cb3f44
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="information-about-using-hdinsight-on-linux"></a><span data-ttu-id="c97c4-103">Informace o používání HDInsight v Linuxu</span><span class="sxs-lookup"><span data-stu-id="c97c4-103">Information about using HDInsight on Linux</span></span>

<span data-ttu-id="c97c4-104">Azure clustery HDInsight poskytují Hadoop na známém prostředí Linux, běží v cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="c97c4-104">Azure HDInsight clusters provide Hadoop on a familiar Linux environment, running in the Azure cloud.</span></span> <span data-ttu-id="c97c4-105">Pro většinu věcí by měly fungovat přesně jako jiná instalace Hadoop na Linuxu.</span><span class="sxs-lookup"><span data-stu-id="c97c4-105">For most things, it should work exactly as any other Hadoop-on-Linux installation.</span></span> <span data-ttu-id="c97c4-106">Tento dokument volá určité rozdíly, které byste měli vědět.</span><span class="sxs-lookup"><span data-stu-id="c97c4-106">This document calls out specific differences that you should be aware of.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c97c4-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="c97c4-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c97c4-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="c97c4-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c97c4-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c97c4-109">Prerequisites</span></span>

<span data-ttu-id="c97c4-110">Řada kroků v tomto dokumentu použít následující nástroje, které je třeba nainstalovat do systému.</span><span class="sxs-lookup"><span data-stu-id="c97c4-110">Many of the steps in this document use the following utilities, which may need to be installed on your system.</span></span>

* <span data-ttu-id="c97c4-111">[cURL](https://curl.haxx.se/) – používá ke komunikaci s webových služeb</span><span class="sxs-lookup"><span data-stu-id="c97c4-111">[cURL](https://curl.haxx.se/) - used to communicate with web-based services</span></span>
* <span data-ttu-id="c97c4-112">[jq](https://stedolan.github.io/jq/) – používané k analýze dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="c97c4-112">[jq](https://stedolan.github.io/jq/) - used to parse JSON documents</span></span>
* <span data-ttu-id="c97c4-113">[Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) (preview) – umožňuje vzdáleně spravovat služby Azure</span><span class="sxs-lookup"><span data-stu-id="c97c4-113">[Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) (preview) - used to remotely manage Azure services</span></span>

## <a name="users"></a><span data-ttu-id="c97c4-114">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="c97c4-114">Users</span></span>

<span data-ttu-id="c97c4-115">Pokud [připojený k doméně](hdinsight-domain-joined-introduction.md), měli byste zvážit HDInsight **jednoho uživatele** systému.</span><span class="sxs-lookup"><span data-stu-id="c97c4-115">Unless [domain-joined](hdinsight-domain-joined-introduction.md), HDInsight should be considered a **single-user** system.</span></span> <span data-ttu-id="c97c4-116">Jeden uživatelský účet SSH je vytvořen s clusteru s úrovně oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="c97c4-116">A single SSH user account is created with the cluster, with administrator level permissions.</span></span> <span data-ttu-id="c97c4-117">Můžete vytvořit další účty SSH, ale také mají přístup správce ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-117">Additional SSH accounts can be created, but they also have administrator access to the cluster.</span></span>

<span data-ttu-id="c97c4-118">Připojené k doméně HDInsight podporuje více uživatelů a podrobnější nastavení oprávnění a role.</span><span class="sxs-lookup"><span data-stu-id="c97c4-118">Domain-joined HDInsight supports multiple users and more granular permission and role settings.</span></span> <span data-ttu-id="c97c4-119">Další informace najdete v tématu [clustery HDInsight spravovat doméně](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="c97c4-119">For more information, see [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>

## <a name="domain-names"></a><span data-ttu-id="c97c4-120">Názvy domén</span><span class="sxs-lookup"><span data-stu-id="c97c4-120">Domain names</span></span>

<span data-ttu-id="c97c4-121">Plně kvalifikovaný název domény (FQDN) pro použití při připojování ke clusteru z Internetu je  **&lt;clustername >. azurehdinsight.net** nebo (pro SSH pouze)  **&lt;clustername-ssh >. azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="c97c4-121">The fully qualified domain name (FQDN) to use when connecting to the cluster from the internet is **&lt;clustername>.azurehdinsight.net** or (for SSH only) **&lt;clustername-ssh>.azurehdinsight.net**.</span></span>

<span data-ttu-id="c97c4-122">Každý uzel v clusteru interně, má název, který se přiřadí během konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-122">Internally, each node in the cluster has a name that is assigned during cluster configuration.</span></span> <span data-ttu-id="c97c4-123">Názvy clusterů, najdete v tématu **hostitele** stránky na webové uživatelské rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="c97c4-123">To find the cluster names, see the **Hosts** page on the Ambari Web UI.</span></span> <span data-ttu-id="c97c4-124">K zobrazení seznamu hostitelů z Ambari REST API můžete použít také následující:</span><span class="sxs-lookup"><span data-stu-id="c97c4-124">You can also use the following to return a list of hosts from the Ambari REST API:</span></span>

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

<span data-ttu-id="c97c4-125">Nahraďte **heslo** s heslem účtu správce a **CLUSTERNAME** s názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-125">Replace **PASSWORD** with the password of the admin account, and **CLUSTERNAME** with the name of your cluster.</span></span> <span data-ttu-id="c97c4-126">Tento příkaz vrátí dokument JSON, který obsahuje seznam hostitele v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-126">This command returns a JSON document that contains a list of the hosts in the cluster.</span></span> <span data-ttu-id="c97c4-127">Jq slouží k extrakci `host_name` hodnota elementu pro každého hostitele.</span><span class="sxs-lookup"><span data-stu-id="c97c4-127">Jq is used to extract the `host_name` element value for each host.</span></span>

<span data-ttu-id="c97c4-128">Pokud potřebujete najít název uzlu pro konkrétní službu, můžete dotazovat Ambari pro danou součást.</span><span class="sxs-lookup"><span data-stu-id="c97c4-128">If you need to find the name of the node for a specific service, you can query Ambari for that component.</span></span> <span data-ttu-id="c97c4-129">Například pokud chcete najít hostitele pro název uzlu HDFS, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c97c4-129">For example, to find the hosts for the HDFS name node, use the following command:</span></span>

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

<span data-ttu-id="c97c4-130">Tento příkaz vrátí dokumentu JSON s popisem služby, a potom jq lze posunout pouze `host_name` hodnotu pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="c97c4-130">This command returns a JSON document describing the service, and then jq pulls out only the `host_name` value for the hosts.</span></span>

## <a name="remote-access-to-services"></a><span data-ttu-id="c97c4-131">Vzdálený přístup ke službám</span><span class="sxs-lookup"><span data-stu-id="c97c4-131">Remote access to services</span></span>

* <span data-ttu-id="c97c4-132">**Ambari (web)** -https://&lt;clustername >. azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="c97c4-132">**Ambari (web)** - https://&lt;clustername>.azurehdinsight.net</span></span>

    <span data-ttu-id="c97c4-133">Ověřování pomocí clusteru správce uživatele a heslo a potom se přihlaste k Ambari.</span><span class="sxs-lookup"><span data-stu-id="c97c4-133">Authenticate by using the cluster administrator user and password, and then log in to Ambari.</span></span>

    <span data-ttu-id="c97c4-134">Ověřování je ve formátu prostého textu – vždycky používají protokol HTTPS k zajištění, že připojení je bezpečné.</span><span class="sxs-lookup"><span data-stu-id="c97c4-134">Authentication is plaintext - always use HTTPS to help ensure that the connection is secure.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c97c4-135">Některé z webu uživatelská rozhraní, které jsou k dispozici prostřednictvím Ambari přístup uzly používá název interní domény.</span><span class="sxs-lookup"><span data-stu-id="c97c4-135">Some of the web UIs available through Ambari access nodes using an internal domain name.</span></span> <span data-ttu-id="c97c4-136">Interní doméně názvy nejsou veřejně přístupné přes internet.</span><span class="sxs-lookup"><span data-stu-id="c97c4-136">Internal domain names are not publicly accessible over the internet.</span></span> <span data-ttu-id="c97c4-137">Při pokusu o přístup k některé funkce přes Internet, může se zobrazit chyby "server nebyl nalezen".</span><span class="sxs-lookup"><span data-stu-id="c97c4-137">You may receive "server not found" errors when trying to access some features over the Internet.</span></span>
    >
    > <span data-ttu-id="c97c4-138">Pokud chcete používat všechny funkce webovému uživatelskému rozhraní Ambari, pomocí tunelového propojení SSH pro přenosy webového proxy serveru k hlavnímu uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-138">To use the full functionality of the Ambari web UI, use an SSH tunnel to proxy web traffic to the cluster head node.</span></span> <span data-ttu-id="c97c4-139">V tématu [používání tunelového propojení SSH pro přístup k webovému uživatelskému rozhraní Ambari, ResourceManager, JobHistory, NameNode, Oozie a jiným webovým uživatelská rozhraní](hdinsight-linux-ambari-ssh-tunnel.md)</span><span class="sxs-lookup"><span data-stu-id="c97c4-139">See [Use SSH Tunneling to access Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, and other web UIs](hdinsight-linux-ambari-ssh-tunnel.md)</span></span>

* <span data-ttu-id="c97c4-140">**Ambari (REST)** -https://&lt;clustername >.azurehdinsight.net/ambari</span><span class="sxs-lookup"><span data-stu-id="c97c4-140">**Ambari (REST)** - https://&lt;clustername>.azurehdinsight.net/ambari</span></span>

    > [!NOTE]
    > <span data-ttu-id="c97c4-141">Ověřování pomocí clusteru správce uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="c97c4-141">Authenticate by using the cluster administrator user and password.</span></span>
    >
    > <span data-ttu-id="c97c4-142">Ověřování je ve formátu prostého textu – vždycky používají protokol HTTPS k zajištění, že připojení je bezpečné.</span><span class="sxs-lookup"><span data-stu-id="c97c4-142">Authentication is plaintext - always use HTTPS to help ensure that the connection is secure.</span></span>

* <span data-ttu-id="c97c4-143">**WebHCat (Templeton)** -https://&lt;clustername >.azurehdinsight.net/templeton</span><span class="sxs-lookup"><span data-stu-id="c97c4-143">**WebHCat (Templeton)** - https://&lt;clustername>.azurehdinsight.net/templeton</span></span>

    > [!NOTE]
    > <span data-ttu-id="c97c4-144">Ověřování pomocí clusteru správce uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="c97c4-144">Authenticate by using the cluster administrator user and password.</span></span>
    >
    > <span data-ttu-id="c97c4-145">Ověřování je ve formátu prostého textu – vždycky používají protokol HTTPS k zajištění, že připojení je bezpečné.</span><span class="sxs-lookup"><span data-stu-id="c97c4-145">Authentication is plaintext - always use HTTPS to help ensure that the connection is secure.</span></span>

* <span data-ttu-id="c97c4-146">**SSH** - &lt;clustername >-ssh.azurehdinsight.net na port 22 a 23.</span><span class="sxs-lookup"><span data-stu-id="c97c4-146">**SSH** - &lt;clustername>-ssh.azurehdinsight.net on port 22 or 23.</span></span> <span data-ttu-id="c97c4-147">Port 22 se používá pro připojení k primární headnode, přičemž 23 se používá pro připojení k sekundární.</span><span class="sxs-lookup"><span data-stu-id="c97c4-147">Port 22 is used to connect to the primary headnode, while 23 is used to connect to the secondary.</span></span> <span data-ttu-id="c97c4-148">Další informace o hlavních uzlech naleznete v tématu [Dostupnost a spolehlivost Hadoop clusterů v HDInsight](hdinsight-high-availability-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c97c4-148">For more information on the head nodes, see [Availability and reliability of Hadoop clusters in HDInsight](hdinsight-high-availability-linux.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="c97c4-149">Hlavního uzlu clusteru můžete přistupovat z počítače klienta pouze prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="c97c4-149">You can only access the cluster head nodes through SSH from a client machine.</span></span> <span data-ttu-id="c97c4-150">Po připojení se pak dostanete pracovní uzly z headnode pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="c97c4-150">Once connected, you can then access the worker nodes by using SSH from a headnode.</span></span>

## <a name="file-locations"></a><span data-ttu-id="c97c4-151">Umístění souborů</span><span class="sxs-lookup"><span data-stu-id="c97c4-151">File locations</span></span>

<span data-ttu-id="c97c4-152">Soubory související s Hadoop naleznete na uzly clusteru na `/usr/hdp`.</span><span class="sxs-lookup"><span data-stu-id="c97c4-152">Hadoop-related files can be found on the cluster nodes at `/usr/hdp`.</span></span> <span data-ttu-id="c97c4-153">Tento adresář obsahuje následující podadresáře:</span><span class="sxs-lookup"><span data-stu-id="c97c4-153">This directory contains the following subdirectories:</span></span>

* <span data-ttu-id="c97c4-154">**2.2.4.9-1**: název adresáře je verze softwaru Hortonworks Data Platform, používá HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c97c4-154">**2.2.4.9-1**: The directory name is the version of the Hortonworks Data Platform used by HDInsight.</span></span> <span data-ttu-id="c97c4-155">Číslo v clusteru může být jiný než ten, který tady.</span><span class="sxs-lookup"><span data-stu-id="c97c4-155">The number on your cluster may be different than the one listed here.</span></span>
* <span data-ttu-id="c97c4-156">**aktuální**: obsahuje odkazy na podadresáře v tomto adresáři **2.2.4.9-1** adresáře.</span><span class="sxs-lookup"><span data-stu-id="c97c4-156">**current**: This directory contains links to subdirectories under the **2.2.4.9-1** directory.</span></span> <span data-ttu-id="c97c4-157">Tento adresář existuje, tak, aby je nemuseli pamatovat číslo verze.</span><span class="sxs-lookup"><span data-stu-id="c97c4-157">This directory exists so that you don't have to remember the version number.</span></span>

<span data-ttu-id="c97c4-158">Příklad dat a souborů JAR naleznete na Hadoop Distributed File System na `/example` a`/HdiSamples`</span><span class="sxs-lookup"><span data-stu-id="c97c4-158">Example data and JAR files can be found on Hadoop Distributed File System at `/example` and `/HdiSamples`</span></span>

## <a name="hdfs-azure-storage-and-data-lake-store"></a><span data-ttu-id="c97c4-159">HDFS, úložiště Azure a Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c97c4-159">HDFS, Azure Storage, and Data Lake Store</span></span>

<span data-ttu-id="c97c4-160">Ve většině distribuce Hadoop HDFS je zajištěna podpora místní úložiště na počítačích v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-160">In most Hadoop distributions, HDFS is backed by local storage on the machines in the cluster.</span></span> <span data-ttu-id="c97c4-161">Používá místní úložiště může být drahé pro cloudové řešení kde budou účtovat každou hodinu nebo minutu pro výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="c97c4-161">Using local storage can be costly for a cloud-based solution where you are charged hourly or by minute for compute resources.</span></span>

<span data-ttu-id="c97c4-162">HDInsight používá jako výchozí úložiště buď objektů BLOB v Azure Storage nebo Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c97c4-162">HDInsight uses either blobs in Azure Storage or Azure Data Lake Store as the default store.</span></span> <span data-ttu-id="c97c4-163">Tyto služby poskytovat následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c97c4-163">These services provide the following benefits:</span></span>

* <span data-ttu-id="c97c4-164">Levných dlouhodobé uložení</span><span class="sxs-lookup"><span data-stu-id="c97c4-164">Cheap long-term storage</span></span>
* <span data-ttu-id="c97c4-165">Usnadnění přístupu z externích služeb například weby, nástroje pro odeslání nebo stažení souboru, různých sadách SDK jazyka a webových prohlížečů</span><span class="sxs-lookup"><span data-stu-id="c97c4-165">Accessibility from external services such as websites, file upload/download utilities, various language SDKs, and web browsers</span></span>

> [!WARNING]
> <span data-ttu-id="c97c4-166">HDInsight podporuje pouze __pro obecné účely__ účty Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c97c4-166">HDInsight only supports __General-purpose__ Azure Storage accounts.</span></span> <span data-ttu-id="c97c4-167">Nepodporuje aktuálně __úložiště objektů Blob__ typ účtu.</span><span class="sxs-lookup"><span data-stu-id="c97c4-167">It does not currently support the __Blob storage__ account type.</span></span>

<span data-ttu-id="c97c4-168">Účet úložiště Azure mohou být uloženy až 4.75 TB, i když jednotlivé objekty BLOB (nebo soubory z hlediska HDInsight) může použít pouze až 195 GB.</span><span class="sxs-lookup"><span data-stu-id="c97c4-168">An Azure Storage account can hold up to 4.75 TB, though individual blobs (or files from an HDInsight perspective) can only go up to 195 GB.</span></span> <span data-ttu-id="c97c4-169">Azure Data Lake Store dynamicky růst, aby udržení bilión soubory s jednotlivé soubory větší než petabajty.</span><span class="sxs-lookup"><span data-stu-id="c97c4-169">Azure Data Lake Store can grow dynamically to hold trillions of files, with individual files greater than a petabyte.</span></span> <span data-ttu-id="c97c4-170">Další informace najdete v tématu [vysvětlení objektů blob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) a [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span><span class="sxs-lookup"><span data-stu-id="c97c4-170">For more information, see [Understanding blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) and [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span></span>

<span data-ttu-id="c97c4-171">Pokud používáte Azure Storage nebo Data Lake Store, nemusíte provádět žádné zvláštní z prostředí HDInsight k přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="c97c4-171">When using either Azure Storage or Data Lake Store, you don't have to do anything special from HDInsight to access the data.</span></span> <span data-ttu-id="c97c4-172">Například následující příkaz seznam souborů `/example/data` složky bez ohledu na to, jestli je uložené v Azure Storage nebo Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="c97c4-172">For example, the following command lists files in the `/example/data` folder regardless of whether it is stored on Azure Storage or Data Lake Store:</span></span>

    hdfs dfs -ls /example/data

### <a name="uri-and-scheme"></a><span data-ttu-id="c97c4-173">Identifikátor URI a schéma</span><span class="sxs-lookup"><span data-stu-id="c97c4-173">URI and scheme</span></span>

<span data-ttu-id="c97c4-174">Některé příkazy mohou vyžadovat zadejte schéma jako součást identifikátoru URI při přístupu k souboru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-174">Some commands may require you to specify the scheme as part of the URI when accessing a file.</span></span> <span data-ttu-id="c97c4-175">Například komponentu Storm HDFS vyžaduje, abyste zadejte schéma.</span><span class="sxs-lookup"><span data-stu-id="c97c4-175">For example, the Storm-HDFS component requires you to specify the scheme.</span></span> <span data-ttu-id="c97c4-176">Pokud používáte jiné než výchozí úložiště (úložiště přidat jako "Další" úložiště do clusteru), musíte vždycky použít schéma jako součást identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="c97c4-176">When using non-default storage (storage added as "additional" storage to the cluster), you must always use the scheme as part of the URI.</span></span>

<span data-ttu-id="c97c4-177">Při použití __Azure Storage__, použijte jednu z následujících schémat URI:</span><span class="sxs-lookup"><span data-stu-id="c97c4-177">When using __Azure Storage__, use one of the following URI schemes:</span></span>

* <span data-ttu-id="c97c4-178">`wasb:///`: Přístup k úložišti výchozí pomocí nešifrovaná komunikace.</span><span class="sxs-lookup"><span data-stu-id="c97c4-178">`wasb:///`: Access default storage using unencrypted communication.</span></span>

* <span data-ttu-id="c97c4-179">`wasbs:///`: Přístup k úložišti výchozí pomocí šifrovanou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="c97c4-179">`wasbs:///`: Access default storage using encrypted communication.</span></span>  <span data-ttu-id="c97c4-180">Schéma wasbs je podporováno pouze z HDInsight verze 3.6 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="c97c4-180">The wasbs scheme is supported only from HDInsight version 3.6 onwards.</span></span>

* <span data-ttu-id="c97c4-181">`wasb://<container-name>@<account-name>.blob.core.windows.net/`: Použít při komunikaci s účtem storage jiné než výchozí.</span><span class="sxs-lookup"><span data-stu-id="c97c4-181">`wasb://<container-name>@<account-name>.blob.core.windows.net/`: Used when communicating with a non-default storage account.</span></span> <span data-ttu-id="c97c4-182">Například pokud máte účet další úložiště nebo pokud přístup k datům uložený v účtu úložiště veřejně přístupná.</span><span class="sxs-lookup"><span data-stu-id="c97c4-182">For example, when you have an additional storage account or when accessing data stored in a publicly accessible storage account.</span></span>

<span data-ttu-id="c97c4-183">Při použití __Data Lake Store__, použijte jednu z následujících schémat URI:</span><span class="sxs-lookup"><span data-stu-id="c97c4-183">When using __Data Lake Store__, use one of the following URI schemes:</span></span>

* <span data-ttu-id="c97c4-184">`adl:///`: Přístup k výchozí Data Lake Store pro cluster.</span><span class="sxs-lookup"><span data-stu-id="c97c4-184">`adl:///`: Access the default Data Lake Store for the cluster.</span></span>

* <span data-ttu-id="c97c4-185">`adl://<storage-name>.azuredatalakestore.net/`: Použít při komunikaci s jiné než výchozí Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c97c4-185">`adl://<storage-name>.azuredatalakestore.net/`: Used when communicating with a non-default Data Lake Store.</span></span> <span data-ttu-id="c97c4-186">Také se používá pro přístup k datům mimo kořenový adresář clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c97c4-186">Also used to access data outside the root directory of your HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c97c4-187">Při použití Data Lake Store jako výchozí úložiště pro HDInsight, je nutné zadat cestu v rámci úložiště, které chcete použít jako kořen úložiště HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c97c4-187">When using Data Lake Store as the default store for HDInsight, you must specify a path within the store to use as the root of HDInsight storage.</span></span> <span data-ttu-id="c97c4-188">Výchozí cesta je `/clusters/<cluster-name>/`.</span><span class="sxs-lookup"><span data-stu-id="c97c4-188">The default path is `/clusters/<cluster-name>/`.</span></span>
>
> <span data-ttu-id="c97c4-189">Při použití `/` nebo `adl:///` přístup k datům přístupné pouze data uložená v kořenovém adresáři (například `/clusters/<cluster-name>/`) clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-189">When using `/` or `adl:///` to access data, you can only access data stored in the root (for example, `/clusters/<cluster-name>/`) of the cluster.</span></span> <span data-ttu-id="c97c4-190">Pro přístup k datům libovolné místo v úložišti, použijte `adl://<storage-name>.azuredatalakestore.net/` formátu.</span><span class="sxs-lookup"><span data-stu-id="c97c4-190">To access data anywhere in the store, use the `adl://<storage-name>.azuredatalakestore.net/` format.</span></span>

### <a name="what-storage-is-the-cluster-using"></a><span data-ttu-id="c97c4-191">Jaké úložiště clusteru používá</span><span class="sxs-lookup"><span data-stu-id="c97c4-191">What storage is the cluster using</span></span>

<span data-ttu-id="c97c4-192">Ambari slouží k načtení výchozí konfiguraci úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="c97c4-192">You can use Ambari to retrieve the default storage configuration for the cluster.</span></span> <span data-ttu-id="c97c4-193">Použijte následující příkaz k načtení pomocí curl HDFS informace o konfiguraci a filtrovat pomocí [jq](https://stedolan.github.io/jq/):</span><span class="sxs-lookup"><span data-stu-id="c97c4-193">Use the following command to retrieve HDFS configuration information using curl, and filter it using [jq](https://stedolan.github.io/jq/):</span></span>

```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'```

> [!NOTE]
> <span data-ttu-id="c97c4-194">Vrátí první konfigurace platí na serveru (`service_config_version=1`), která obsahuje tyto informace.</span><span class="sxs-lookup"><span data-stu-id="c97c4-194">This returns the first configuration applied to the server (`service_config_version=1`), which contains this information.</span></span> <span data-ttu-id="c97c4-195">Budete muset všechny verze konfigurace vyhledat nejnovější seznam.</span><span class="sxs-lookup"><span data-stu-id="c97c4-195">You may need to list all configuration versions to find the latest one.</span></span>

<span data-ttu-id="c97c4-196">Tento příkaz vrátí hodnotu podobná následující identifikátory URI:</span><span class="sxs-lookup"><span data-stu-id="c97c4-196">This command returns a value similar to the following URIs:</span></span>

* <span data-ttu-id="c97c4-197">`wasb://<container-name>@<account-name>.blob.core.windows.net`Pokud používáte účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c97c4-197">`wasb://<container-name>@<account-name>.blob.core.windows.net` if using an Azure Storage account.</span></span>

    <span data-ttu-id="c97c4-198">Název účtu je název účtu úložiště Azure, zatímco název kontejneru je kontejneru blobů, který je kořenem úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-198">The account name is the name of the Azure Storage account, while the container name is the blob container that is the root of the cluster storage.</span></span>

* <span data-ttu-id="c97c4-199">`adl://home`Pokud používáte Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c97c4-199">`adl://home` if using Azure Data Lake Store.</span></span> <span data-ttu-id="c97c4-200">Název Data Lake Store, použijte následující volání REST:</span><span class="sxs-lookup"><span data-stu-id="c97c4-200">To get the Data Lake Store name, use the following REST call:</span></span>

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'```

    <span data-ttu-id="c97c4-201">Tento příkaz vrátí následující název hostitele: `<data-lake-store-account-name>.azuredatalakestore.net`.</span><span class="sxs-lookup"><span data-stu-id="c97c4-201">This command returns the following host name: `<data-lake-store-account-name>.azuredatalakestore.net`.</span></span>

    <span data-ttu-id="c97c4-202">Adresář v rámci úložiště, který je kořenem pro HDInsight, použijte následující volání REST:</span><span class="sxs-lookup"><span data-stu-id="c97c4-202">To get the directory within the store that is the root for HDInsight, use the following REST call:</span></span>

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'```

    <span data-ttu-id="c97c4-203">Tento příkaz vrátí cestu podobná následující cestu: `/clusters/<hdinsight-cluster-name>/`.</span><span class="sxs-lookup"><span data-stu-id="c97c4-203">This command returns a path similar to the following path: `/clusters/<hdinsight-cluster-name>/`.</span></span>

<span data-ttu-id="c97c4-204">Můžete také najít informace o úložiště pomocí portálu Azure pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="c97c4-204">You can also find the storage information using the Azure portal by using the following steps:</span></span>

1. <span data-ttu-id="c97c4-205">V [portál Azure](https://portal.azure.com/), vyberte clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c97c4-205">In the [Azure portal](https://portal.azure.com/), select your HDInsight cluster.</span></span>

2. <span data-ttu-id="c97c4-206">Z **vlastnosti** vyberte **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="c97c4-206">From the **Properties** section, select **Storage Accounts**.</span></span> <span data-ttu-id="c97c4-207">Zobrazí se informace úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="c97c4-207">The storage information for the cluster is displayed.</span></span>

### <a name="how-do-i-access-files-from-outside-hdinsight"></a><span data-ttu-id="c97c4-208">Přístupu soubory z mimo HDInsight</span><span class="sxs-lookup"><span data-stu-id="c97c4-208">How do I access files from outside HDInsight</span></span>

<span data-ttu-id="c97c4-209">Existují různé způsoby pro přístup k datům z mimo HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="c97c4-209">There are a various ways to access data from outside the HDInsight cluster.</span></span> <span data-ttu-id="c97c4-210">Následuje několik odkazů na nástroje a sady SDK, které lze použít pro práci s daty:</span><span class="sxs-lookup"><span data-stu-id="c97c4-210">The following are a few links to utilities and SDKs that can be used to work with your data:</span></span>

<span data-ttu-id="c97c4-211">Pokud používáte __Azure Storage__, najdete v následujících tématech pro způsoby, můžete přístup k datům:</span><span class="sxs-lookup"><span data-stu-id="c97c4-211">If using __Azure Storage__, see the following links for ways that you can access your data:</span></span>

* <span data-ttu-id="c97c4-212">[Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2): příkazy rozhraní příkazového řádku pro práci s Azure.</span><span class="sxs-lookup"><span data-stu-id="c97c4-212">[Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2): Command-Line interface commands for working with Azure.</span></span> <span data-ttu-id="c97c4-213">Po instalaci, pomocí `az storage` příkaz nápovědu k použití úložiště, nebo `az storage blob` pro objekt blob specifické příkazy.</span><span class="sxs-lookup"><span data-stu-id="c97c4-213">After installing, use the `az storage` command for help on using storage, or `az storage blob` for blob-specific commands.</span></span>
* <span data-ttu-id="c97c4-214">[blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): python script pro práci s objekty BLOB ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c97c4-214">[blobxfer.py](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): A python script for working with blobs in Azure Storage.</span></span>
* <span data-ttu-id="c97c4-215">Různých sadách SDK:</span><span class="sxs-lookup"><span data-stu-id="c97c4-215">Various SDKs:</span></span>

    * [<span data-ttu-id="c97c4-216">Java</span><span class="sxs-lookup"><span data-stu-id="c97c4-216">Java</span></span>](https://github.com/Azure/azure-sdk-for-java)
    * [<span data-ttu-id="c97c4-217">Node.js</span><span class="sxs-lookup"><span data-stu-id="c97c4-217">Node.js</span></span>](https://github.com/Azure/azure-sdk-for-node)
    * [<span data-ttu-id="c97c4-218">PHP</span><span class="sxs-lookup"><span data-stu-id="c97c4-218">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php)
    * [<span data-ttu-id="c97c4-219">Python</span><span class="sxs-lookup"><span data-stu-id="c97c4-219">Python</span></span>](https://github.com/Azure/azure-sdk-for-python)
    * [<span data-ttu-id="c97c4-220">Ruby</span><span class="sxs-lookup"><span data-stu-id="c97c4-220">Ruby</span></span>](https://github.com/Azure/azure-sdk-for-ruby)
    * [<span data-ttu-id="c97c4-221">.NET</span><span class="sxs-lookup"><span data-stu-id="c97c4-221">.NET</span></span>](https://github.com/Azure/azure-sdk-for-net)
    * [<span data-ttu-id="c97c4-222">Rozhraní REST API pro Storage</span><span class="sxs-lookup"><span data-stu-id="c97c4-222">Storage REST API</span></span>](https://msdn.microsoft.com/library/azure/dd135733.aspx)

<span data-ttu-id="c97c4-223">Pokud používáte __Azure Data Lake Store__, najdete v následujících tématech pro způsoby, můžete přístup k datům:</span><span class="sxs-lookup"><span data-stu-id="c97c4-223">If using __Azure Data Lake Store__, see the following links for ways that you can access your data:</span></span>

* [<span data-ttu-id="c97c4-224">Webový prohlížeč</span><span class="sxs-lookup"><span data-stu-id="c97c4-224">Web browser</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* [<span data-ttu-id="c97c4-225">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c97c4-225">PowerShell</span></span>](../data-lake-store/data-lake-store-get-started-powershell.md)
* [<span data-ttu-id="c97c4-226">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c97c4-226">Azure CLI 2.0</span></span>](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [<span data-ttu-id="c97c4-227">Rozhraní REST API WebHDFS</span><span class="sxs-lookup"><span data-stu-id="c97c4-227">WebHDFS REST API</span></span>](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [<span data-ttu-id="c97c4-228">Nástroje data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c97c4-228">Data Lake Tools for Visual Studio</span></span>](https://www.microsoft.com/download/details.aspx?id=49504)
* [<span data-ttu-id="c97c4-229">.NET</span><span class="sxs-lookup"><span data-stu-id="c97c4-229">.NET</span></span>](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [<span data-ttu-id="c97c4-230">Java</span><span class="sxs-lookup"><span data-stu-id="c97c4-230">Java</span></span>](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [<span data-ttu-id="c97c4-231">Python</span><span class="sxs-lookup"><span data-stu-id="c97c4-231">Python</span></span>](../data-lake-store/data-lake-store-get-started-python.md)

## <span data-ttu-id="c97c4-232"><a name="scaling"></a>Škálování clusteru</span><span class="sxs-lookup"><span data-stu-id="c97c4-232"><a name="scaling"></a>Scaling your cluster</span></span>

<span data-ttu-id="c97c4-233">Funkce škálování clusteru umožňuje dynamicky měnit počet uzlů dat používá cluster.</span><span class="sxs-lookup"><span data-stu-id="c97c4-233">The cluster scaling feature allows you to dynamically change the number of data nodes used by a cluster.</span></span> <span data-ttu-id="c97c4-234">Můžete provádět operace škálování při dalších úloh nebo procesů běží na clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-234">You can perform scaling operations while other jobs or processes are running on a cluster.</span></span>

<span data-ttu-id="c97c4-235">Typy jiného clusteru se týká škálování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c97c4-235">The different cluster types are affected by scaling as follows:</span></span>

* <span data-ttu-id="c97c4-236">**Hadoop**: při škálování se počet uzlů v clusteru, jsou restartovány některé služby v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-236">**Hadoop**: When scaling down the number of nodes in a cluster, some of the services in the cluster are restarted.</span></span> <span data-ttu-id="c97c4-237">Operace škálování může způsobit úlohy spuštěná nebo čeká na dokončení selhání po dokončení operace škálování.</span><span class="sxs-lookup"><span data-stu-id="c97c4-237">Scaling operations can cause jobs running or pending to fail at the completion of the scaling operation.</span></span> <span data-ttu-id="c97c4-238">Úlohy můžete znovu odeslat, po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="c97c4-238">You can resubmit the jobs once the operation is complete.</span></span>
* <span data-ttu-id="c97c4-239">**HBase**: místní servery jsou automaticky vyváženy během několika minut po dokončení operace škálování.</span><span class="sxs-lookup"><span data-stu-id="c97c4-239">**HBase**: Regional servers are automatically balanced within a few minutes after completion of the scaling operation.</span></span> <span data-ttu-id="c97c4-240">Chcete-li ručně vyvážit místní servery, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c97c4-240">To manually balance regional servers, use the following steps:</span></span>

    1. <span data-ttu-id="c97c4-241">Připojte ke clusteru HDInsight pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="c97c4-241">Connect to the HDInsight cluster using SSH.</span></span> <span data-ttu-id="c97c4-242">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c97c4-242">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

    2. <span data-ttu-id="c97c4-243">Ke spuštění prostředí HBase, použijte následující:</span><span class="sxs-lookup"><span data-stu-id="c97c4-243">Use the following to start the HBase shell:</span></span>

            hbase shell

    3. <span data-ttu-id="c97c4-244">Jakmile se načetl prostředí HBase, použijte následující postupy ručně vyvážit místní servery:</span><span class="sxs-lookup"><span data-stu-id="c97c4-244">Once the HBase shell has loaded, use the following to manually balance the regional servers:</span></span>

            balancer

* <span data-ttu-id="c97c4-245">**Storm**: musíte znovu vyvážit všechny spuštěné topologie Storm po nebyla provedena škálování operace.</span><span class="sxs-lookup"><span data-stu-id="c97c4-245">**Storm**: You should rebalance any running Storm topologies after a scaling operation has been performed.</span></span> <span data-ttu-id="c97c4-246">Vyrovnává umožňuje topologii paralelismus nastavení na základě nové počtu uzlů v clusteru se znovu.</span><span class="sxs-lookup"><span data-stu-id="c97c4-246">Rebalancing allows the topology to readjust parallelism settings based on the new number of nodes in the cluster.</span></span> <span data-ttu-id="c97c4-247">Znovu vyvážit spuštěné topologie, použijte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="c97c4-247">To rebalance running topologies, use one of the following options:</span></span>

    * <span data-ttu-id="c97c4-248">**SSH**: připojení k serveru a znovu vyvážit topologii pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="c97c4-248">**SSH**: Connect to the server and use the following command to rebalance a topology:</span></span>

            storm rebalance TOPOLOGYNAME

        <span data-ttu-id="c97c4-249">Můžete také zadat parametry k přepsání pomocné parametry paralelismus původně poskytované topologii.</span><span class="sxs-lookup"><span data-stu-id="c97c4-249">You can also specify parameters to override the parallelism hints originally provided by the topology.</span></span> <span data-ttu-id="c97c4-250">Například `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` změní konfiguraci topologie 5 pracovních procesů, 3 vykonavatelů pro komponentu blue spout a 10 vykonavatelů pro komponentu žlutý bolt.</span><span class="sxs-lookup"><span data-stu-id="c97c4-250">For example, `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` reconfigures the topology to 5 worker processes, 3 executors for the blue-spout component, and 10 executors for the yellow-bolt component.</span></span>

    * <span data-ttu-id="c97c4-251">**Uživatelské rozhraní Storm**: Chcete-li znovu vyvážit topologii pomocí uživatelského rozhraní Storm použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="c97c4-251">**Storm UI**: Use the following steps to rebalance a topology using the Storm UI.</span></span>

        1. <span data-ttu-id="c97c4-252">Otevřete **https://CLUSTERNAME.azurehdinsight.net/stormui** ve webovém prohlížeči, kde CLUSTERNAME představuje název clusteru Storm.</span><span class="sxs-lookup"><span data-stu-id="c97c4-252">Open **https://CLUSTERNAME.azurehdinsight.net/stormui** in your web browser, where CLUSTERNAME is the name of your Storm cluster.</span></span> <span data-ttu-id="c97c4-253">Pokud se zobrazí výzva, zadejte název správce (správce) clusteru HDInsight a heslo, které jste zadali při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-253">If prompted, enter the HDInsight cluster administrator (admin) name and password you specified when creating the cluster.</span></span>
        2. <span data-ttu-id="c97c4-254">Vyberte topologii chcete znovu vyvážit a pak vyberte **znovu vyvážit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c97c4-254">Select the topology you wish to rebalance, then select the **Rebalance** button.</span></span> <span data-ttu-id="c97c4-255">Zadejte zpoždění před provedením operace obnovte rovnováhu.</span><span class="sxs-lookup"><span data-stu-id="c97c4-255">Enter the delay before the rebalance operation is performed.</span></span>

<span data-ttu-id="c97c4-256">Konkrétní informace o škálování clusteru HDInsight naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="c97c4-256">For specific information on scaling your HDInsight cluster, see:</span></span>

* [<span data-ttu-id="c97c4-257">Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c97c4-257">Manage Hadoop clusters in HDInsight by using the Azure portal</span></span>](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [<span data-ttu-id="c97c4-258">Správa clusterů systému Hadoop v HDInsight pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c97c4-258">Manage Hadoop clusters in HDInsight by using Azure PowerShell</span></span>](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a><span data-ttu-id="c97c4-259">Jak nainstalovat Hue (nebo jiné součásti Hadoop)?</span><span class="sxs-lookup"><span data-stu-id="c97c4-259">How do I install Hue (or other Hadoop component)?</span></span>

<span data-ttu-id="c97c4-260">HDInsight je spravované služby.</span><span class="sxs-lookup"><span data-stu-id="c97c4-260">HDInsight is a managed service.</span></span> <span data-ttu-id="c97c4-261">Pokud Azure zjistí problém s clusterem, může uzel selhání odstranit a vytvořit uzel ho nahradit.</span><span class="sxs-lookup"><span data-stu-id="c97c4-261">If Azure detects a problem with the cluster, it may delete the failing node and create a node to replace it.</span></span> <span data-ttu-id="c97c4-262">Pokud nainstalujete ručně věcí v clusteru, nejsou trvalé v případě této operace.</span><span class="sxs-lookup"><span data-stu-id="c97c4-262">If you manually install things on the cluster, they are not persisted when this operation occurs.</span></span> <span data-ttu-id="c97c4-263">Místo toho použijte [akce skriptu HDInsight](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="c97c4-263">Instead, use [HDInsight Script Actions](hdinsight-hadoop-customize-cluster.md).</span></span> <span data-ttu-id="c97c4-264">Akce skriptu lze provést následující změny:</span><span class="sxs-lookup"><span data-stu-id="c97c4-264">A script action can be used to make the following changes:</span></span>

* <span data-ttu-id="c97c4-265">Nainstalujte a nakonfigurujte službu nebo webové stránky, jako je například Spark nebo Hue.</span><span class="sxs-lookup"><span data-stu-id="c97c4-265">Install and configure a service or web site such as Spark or Hue.</span></span>
* <span data-ttu-id="c97c4-266">Instalace a konfigurace komponenty, která vyžaduje změny konfigurace ve více uzlech v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-266">Install and configure a component that requires configuration changes on multiple nodes in the cluster.</span></span> <span data-ttu-id="c97c4-267">Například proměnnou požadované prostředí, vytváření adresář protokolování nebo vytvoření konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-267">For example, a required environment variable, creating of a logging directory, or creation of a configuration file.</span></span>

<span data-ttu-id="c97c4-268">Akce skriptů jsou skripty Bash.</span><span class="sxs-lookup"><span data-stu-id="c97c4-268">Script Actions are Bash scripts.</span></span> <span data-ttu-id="c97c4-269">Tyto skripty spustit při zřizování clusteru a lze nainstalovat a nakonfigurovat další součásti v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c97c4-269">The scripts run during cluster provisioning, and can be used to install and configure additional components on the cluster.</span></span> <span data-ttu-id="c97c4-270">Příklad skriptů jsou k dispozici pro instalaci následující součásti:</span><span class="sxs-lookup"><span data-stu-id="c97c4-270">Example scripts are provided for installing the following components:</span></span>

* [<span data-ttu-id="c97c4-271">HUE</span><span class="sxs-lookup"><span data-stu-id="c97c4-271">Hue</span></span>](hdinsight-hadoop-hue-linux.md)
* [<span data-ttu-id="c97c4-272">Giraph</span><span class="sxs-lookup"><span data-stu-id="c97c4-272">Giraph</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="c97c4-273">Solr</span><span class="sxs-lookup"><span data-stu-id="c97c4-273">Solr</span></span>](hdinsight-hadoop-solr-install-linux.md)

<span data-ttu-id="c97c4-274">Informace o vývoji vlastních akcí skriptů naleznete v tématu [Vývoj akcí skriptů v prostředí HDInsight](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c97c4-274">For information on developing your own Script Actions, see [Script Action development with HDInsight](hdinsight-hadoop-script-actions-linux.md).</span></span>

### <a name="jar-files"></a><span data-ttu-id="c97c4-275">Souborů JAR</span><span class="sxs-lookup"><span data-stu-id="c97c4-275">Jar files</span></span>

<span data-ttu-id="c97c4-276">Některé technologie Hadoop jsou uvedeny v nezávislý jar souborů, které obsahují funkce, které jsou použity jako součást úlohy MapReduce, nebo z uvnitř Pig nebo Hive.</span><span class="sxs-lookup"><span data-stu-id="c97c4-276">Some Hadoop technologies are provided in self-contained jar files that contain functions used as part of a MapReduce job, or from inside Pig or Hive.</span></span> <span data-ttu-id="c97c4-277">Když tyto můžete nainstalovat pomocí akcí skriptů, často nevyžadují žádné nastavení a mohou být nahrán do clusteru po zřízení a použít přímo.</span><span class="sxs-lookup"><span data-stu-id="c97c4-277">While these can be installed using Script Actions, they often don't require any setup and can be uploaded to the cluster after provisioning and used directly.</span></span> <span data-ttu-id="c97c4-278">Pokud chcete, aby zajistil, že součást odolává obnovování clusteru, můžete uložit na soubor jar ve výchozím nastavení úložiště pro váš cluster (WASB nebo ADL).</span><span class="sxs-lookup"><span data-stu-id="c97c4-278">If you want to make sure the component survives reimaging of the cluster, you can store the jar file in the default storage for your cluster (WASB or ADL).</span></span>

<span data-ttu-id="c97c4-279">Například, pokud chcete použít nejnovější verzi [DataFu](http://datafu.incubator.apache.org/), můžete stáhnout jar obsahující projekt a nahrajte ho do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c97c4-279">For example, if you want to use the latest version of [DataFu](http://datafu.incubator.apache.org/), you can download a jar containing the project and upload it to the HDInsight cluster.</span></span> <span data-ttu-id="c97c4-280">Potom postupujte podle dokumentace DataFu o tom, jak používat z Pig nebo Hive.</span><span class="sxs-lookup"><span data-stu-id="c97c4-280">Then follow the DataFu documentation on how to use it from Pig or Hive.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c97c4-281">Některé součásti, které jsou samostatné jar soubory jsou k dispozici s HDInsight, ale nejsou v cestě.</span><span class="sxs-lookup"><span data-stu-id="c97c4-281">Some components that are standalone jar files are provided with HDInsight, but are not in the path.</span></span> <span data-ttu-id="c97c4-282">Pokud hledáte konkrétní součást, můžete ji najít v clusteru těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="c97c4-282">If you are looking for a specific component, you can use the follow to search for it on your cluster:</span></span>
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> <span data-ttu-id="c97c4-283">Tento příkaz vrátí cestu odpovídající jar soubory.</span><span class="sxs-lookup"><span data-stu-id="c97c4-283">This command returns the path of any matching jar files.</span></span>

<span data-ttu-id="c97c4-284">Použití jiné verze součásti, nahrajte verze potřebujete a použít v úlohách.</span><span class="sxs-lookup"><span data-stu-id="c97c4-284">To use a different version of a component, upload the version you need and use it in your jobs.</span></span>

> [!WARNING]
> <span data-ttu-id="c97c4-285">Součásti, které jsou součástí clusteru HDInsight jsou plně podporované a Microsoft Support pomáhá izolovat a vyřešení problémů týkajících se těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="c97c4-285">Components provided with the HDInsight cluster are fully supported and Microsoft Support helps to isolate and resolve issues related to these components.</span></span>
>
> <span data-ttu-id="c97c4-286">Vlastní komponenty získat vyvineme podporu k pomoci při další řešení problému.</span><span class="sxs-lookup"><span data-stu-id="c97c4-286">Custom components receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="c97c4-287">To může způsobit řešení problému nebo s žádostí o zapojení dostupné kanály pro technologie s otevřeným zdrojem, kterých se nachází hluboké znalosti pro tuto technologii.</span><span class="sxs-lookup"><span data-stu-id="c97c4-287">This might result in resolving the issue OR asking you to engage available channels for the open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="c97c4-288">Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="c97c4-288">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span></span> <span data-ttu-id="c97c4-289">Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="c97c4-289">Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c97c4-290">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c97c4-290">Next steps</span></span>

* [<span data-ttu-id="c97c4-291">Migrace z HDInsight se systémem Windows do systémem Linux</span><span class="sxs-lookup"><span data-stu-id="c97c4-291">Migrate from Windows-based HDInsight to Linux-based</span></span>](hdinsight-migrate-from-windows-to-linux.md)
* [<span data-ttu-id="c97c4-292">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="c97c4-292">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="c97c4-293">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="c97c4-293">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="c97c4-294">Použití úloh MapReduce se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="c97c4-294">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
