---
title: "Vývoj akcí skriptů v HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Naučte se používat skripty Bash přizpůsobit clustery HDInsight se systémem Linux. Funkci akce skriptu HDInsight umožňuje spouštět skripty během nebo po vytvoření clusteru. Skripty lze změnit nastavení konfigurace clusteru nebo nainstalovat další software."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 7f1a0bd8c7e60770d376f10eaea136a55c632c5e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="script-action-development-with-hdinsight"></a><span data-ttu-id="1be4c-105">Vývoj akcí skriptů v prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="1be4c-105">Script action development with HDInsight</span></span>

<span data-ttu-id="1be4c-106">Zjistěte, jak přizpůsobit pomocí skriptů Bash clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1be4c-106">Learn how to customize your HDInsight cluster using Bash scripts.</span></span> <span data-ttu-id="1be4c-107">Akce skriptů jsou způsob, jak přizpůsobit HDInsight během nebo po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="1be4c-107">Script actions are a way to customize HDInsight during or after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1be4c-108">Kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="1be4c-108">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="1be4c-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="1be4c-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="1be4c-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="1be4c-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="what-are-script-actions"></a><span data-ttu-id="1be4c-111">Jaké jsou akce skriptu</span><span class="sxs-lookup"><span data-stu-id="1be4c-111">What are script actions</span></span>

<span data-ttu-id="1be4c-112">Akce skriptů jsou skripty Bash, které Azure je spuštěná na uzlech clusteru provést změny konfigurace nebo instalace softwaru.</span><span class="sxs-lookup"><span data-stu-id="1be4c-112">Script actions are Bash scripts that Azure runs on the cluster nodes to make configuration changes or install software.</span></span> <span data-ttu-id="1be4c-113">Akce skriptu se spustí jako kořenového adresáře a poskytuje úplná přístupová práva pro uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="1be4c-113">A script action is executed as root, and provides full access rights to the cluster nodes.</span></span>

<span data-ttu-id="1be4c-114">Skript akce se dá použít prostřednictvím následujících metod:</span><span class="sxs-lookup"><span data-stu-id="1be4c-114">Script actions can be applied through the following methods:</span></span>

| <span data-ttu-id="1be4c-115">Pomocí této metody můžete použít skript...</span><span class="sxs-lookup"><span data-stu-id="1be4c-115">Use this method to apply a script...</span></span> | <span data-ttu-id="1be4c-116">Při vytvoření clusteru...</span><span class="sxs-lookup"><span data-stu-id="1be4c-116">During cluster creation...</span></span> | <span data-ttu-id="1be4c-117">Na clusteru s podporou spuštěné...</span><span class="sxs-lookup"><span data-stu-id="1be4c-117">On a running cluster...</span></span> |
| --- |:---:|:---:|
| <span data-ttu-id="1be4c-118">portál Azure</span><span class="sxs-lookup"><span data-stu-id="1be4c-118">Azure portal</span></span> |<span data-ttu-id="1be4c-119">✓</span><span class="sxs-lookup"><span data-stu-id="1be4c-119">✓</span></span> |<span data-ttu-id="1be4c-120">✓</span><span class="sxs-lookup"><span data-stu-id="1be4c-120">✓</span></span> |
| <span data-ttu-id="1be4c-121">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1be4c-121">Azure PowerShell</span></span> |<span data-ttu-id="1be4c-122">✓</span><span class="sxs-lookup"><span data-stu-id="1be4c-122">✓</span></span> |<span data-ttu-id="1be4c-123">✓</span><span class="sxs-lookup"><span data-stu-id="1be4c-123">✓</span></span> |
| <span data-ttu-id="1be4c-124">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1be4c-124">Azure CLI</span></span> |&nbsp; |<span data-ttu-id="1be4c-125">✓</span><span class="sxs-lookup"><span data-stu-id="1be4c-125">✓</span></span> |
| <span data-ttu-id="1be4c-126">Sada HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="1be4c-126">HDInsight .NET SDK</span></span> |<span data-ttu-id="1be4c-127">✓</span><span class="sxs-lookup"><span data-stu-id="1be4c-127">✓</span></span> |<span data-ttu-id="1be4c-128">✓</span><span class="sxs-lookup"><span data-stu-id="1be4c-128">✓</span></span> |
| <span data-ttu-id="1be4c-129">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="1be4c-129">Azure Resource Manager Template</span></span> |<span data-ttu-id="1be4c-130">✓</span><span class="sxs-lookup"><span data-stu-id="1be4c-130">✓</span></span> |&nbsp; |

<span data-ttu-id="1be4c-131">Další informace o použití těchto metod akcí skriptů naleznete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1be4c-131">For more information on using these methods to apply script actions, see [Customize HDInsight clusters using script actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="1be4c-132"><a name="bestPracticeScripting"></a>Osvědčené postupy pro vývoj skriptů</span><span class="sxs-lookup"><span data-stu-id="1be4c-132"><a name="bestPracticeScripting"></a>Best practices for script development</span></span>

<span data-ttu-id="1be4c-133">Při vývoji vlastních skriptů pro cluster služby HDInsight, existuje několik doporučených postupech pro mějte na paměti:</span><span class="sxs-lookup"><span data-stu-id="1be4c-133">When you develop a custom script for an HDInsight cluster, there are several best practices to keep in mind:</span></span>

* [<span data-ttu-id="1be4c-134">Cílová verze Hadoop</span><span class="sxs-lookup"><span data-stu-id="1be4c-134">Target the Hadoop version</span></span>](#bPS1)
* [<span data-ttu-id="1be4c-135">Cílové verze operačního systému</span><span class="sxs-lookup"><span data-stu-id="1be4c-135">Target the OS Version</span></span>](#bps10)
* [<span data-ttu-id="1be4c-136">Zadejte stabilní odkazy na zdroje skriptu</span><span class="sxs-lookup"><span data-stu-id="1be4c-136">Provide stable links to script resources</span></span>](#bPS2)
* [<span data-ttu-id="1be4c-137">Použití předem zkompilovat prostředky</span><span class="sxs-lookup"><span data-stu-id="1be4c-137">Use pre-compiled resources</span></span>](#bPS4)
* [<span data-ttu-id="1be4c-138">Zajistěte, aby byl skript přizpůsobení clusteru idempotent</span><span class="sxs-lookup"><span data-stu-id="1be4c-138">Ensure that the cluster customization script is idempotent</span></span>](#bPS3)
* [<span data-ttu-id="1be4c-139">Zajištění vysoké dostupnosti architektury clusteru</span><span class="sxs-lookup"><span data-stu-id="1be4c-139">Ensure high availability of the cluster architecture</span></span>](#bPS5)
* [<span data-ttu-id="1be4c-140">Konfigurace vlastní součásti, které budou používat úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="1be4c-140">Configure the custom components to use Azure Blob storage</span></span>](#bPS6)
* [<span data-ttu-id="1be4c-141">Zapisuje informace do STDOUT a STDERR</span><span class="sxs-lookup"><span data-stu-id="1be4c-141">Write information to STDOUT and STDERR</span></span>](#bPS7)
* [<span data-ttu-id="1be4c-142">Ukládat soubory ve formátu ASCII s LF konce řádků</span><span class="sxs-lookup"><span data-stu-id="1be4c-142">Save files as ASCII with LF line endings</span></span>](#bps8)
* [<span data-ttu-id="1be4c-143">Logika opakovaných pokusů použít k obnovení z přechodné chyby</span><span class="sxs-lookup"><span data-stu-id="1be4c-143">Use retry logic to recover from transient errors</span></span>](#bps9)

> [!IMPORTANT]
> <span data-ttu-id="1be4c-144">Akce skriptu musíte dokončit během 60 minut nebo proces selže.</span><span class="sxs-lookup"><span data-stu-id="1be4c-144">Script actions must complete within 60 minutes or the process fails.</span></span> <span data-ttu-id="1be4c-145">Při zřizování uzlu, bude skript spuštěn souběžně jiné procesy instalací a konfigurací.</span><span class="sxs-lookup"><span data-stu-id="1be4c-145">During node provisioning, the script runs concurrently with other setup and configuration processes.</span></span> <span data-ttu-id="1be4c-146">Soutěž o zdroje, jako je například šířky pásma procesoru čas nebo v síti může způsobit skript trvá déle než ve vašem vývojovém prostředí dokončit.</span><span class="sxs-lookup"><span data-stu-id="1be4c-146">Competition for resources such as CPU time or network bandwidth may cause the script to take longer to finish than it does in your development environment.</span></span>

### <span data-ttu-id="1be4c-147"><a name="bPS1"></a>Cílová verze Hadoop</span><span class="sxs-lookup"><span data-stu-id="1be4c-147"><a name="bPS1"></a>Target the Hadoop version</span></span>

<span data-ttu-id="1be4c-148">Různé verze HDInsight mají různé verze služby Hadoop a nainstalovány součásti.</span><span class="sxs-lookup"><span data-stu-id="1be4c-148">Different versions of HDInsight have different versions of Hadoop services and components installed.</span></span> <span data-ttu-id="1be4c-149">Pokud váš skript očekává na konkrétní verzi služeb nebo součásti, byste měli skript používat jenom k verzi HDInsight, který obsahuje požadované součásti.</span><span class="sxs-lookup"><span data-stu-id="1be4c-149">If your script expects a specific version of a service or component, you should only use the script with the version of HDInsight that includes the required components.</span></span> <span data-ttu-id="1be4c-150">Můžete najít informace o verze součástí, které jsou součástí HDInsight pomocí [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-150">You can find information on component versions included with HDInsight using the [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

### <span data-ttu-id="1be4c-151"><a name="bps10"></a>Cílové verze operačního systému</span><span class="sxs-lookup"><span data-stu-id="1be4c-151"><a name="bps10"></a> Target the OS version</span></span>

<span data-ttu-id="1be4c-152">HDInsight se systémem Linux je založena na rozdělení Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="1be4c-152">Linux-based HDInsight is based on the Ubuntu Linux distribution.</span></span> <span data-ttu-id="1be4c-153">Různé verze HDInsight využívají různé verze Ubuntu, což může změnit chování vašeho skriptu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-153">Different versions of HDInsight rely on different versions of Ubuntu, which may change how your script behaves.</span></span> <span data-ttu-id="1be4c-154">Například vychází HDInsight 3.4 a starší verze Ubuntu, které používají Upstart.</span><span class="sxs-lookup"><span data-stu-id="1be4c-154">For example, HDInsight 3.4 and earlier are based on Ubuntu versions that use Upstart.</span></span> <span data-ttu-id="1be4c-155">Ubuntu 16.04, který používá Systemd podle verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="1be4c-155">Version 3.5 is based on Ubuntu 16.04, which uses Systemd.</span></span> <span data-ttu-id="1be4c-156">Systemd a Upstart závisí na jiné příkazy, tak budou zasílány váš skript pro práci s obě.</span><span class="sxs-lookup"><span data-stu-id="1be4c-156">Systemd and Upstart rely on different commands, so your script should be written to work with both.</span></span>

<span data-ttu-id="1be4c-157">Další důležité rozdíl mezi HDInsight 3.4 a 3.5 je, že `JAVA_HOME` nyní odkazuje na Java 8.</span><span class="sxs-lookup"><span data-stu-id="1be4c-157">Another important difference between HDInsight 3.4 and 3.5 is that `JAVA_HOME` now points to Java 8.</span></span>

<span data-ttu-id="1be4c-158">Verze operačního systému můžete zkontrolovat pomocí `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-158">You can check the OS version by using `lsb_release`.</span></span> <span data-ttu-id="1be4c-159">Následující kód ukazuje, jak určit, zda je skript spuštěn na Ubuntu 14 nebo 16:</span><span class="sxs-lookup"><span data-stu-id="1be4c-159">The following code demonstrates how to determine if the script is running on Ubuntu 14 or 16:</span></span>

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

<span data-ttu-id="1be4c-160">Můžete získat úplné skript, který obsahuje tyto fragmenty kódu v https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.</span><span class="sxs-lookup"><span data-stu-id="1be4c-160">You can find the full script that contains these snippets at https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.</span></span>

<span data-ttu-id="1be4c-161">Verze Ubuntu, který se používá v prostředí HDInsight, najdete v článku [verze komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-161">For the version of Ubuntu that is used by HDInsight, see the [HDInsight component version](hdinsight-component-versioning.md) document.</span></span>

<span data-ttu-id="1be4c-162">Chcete-li pochopit rozdíly mezi Systemd a Upstart, přečtěte si téma [Systemd pro uživatele Upstart](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span><span class="sxs-lookup"><span data-stu-id="1be4c-162">To understand the differences between Systemd and Upstart, see [Systemd for Upstart users](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span></span>

### <span data-ttu-id="1be4c-163"><a name="bPS2"></a>Zadejte stabilní odkazy na zdroje skriptu</span><span class="sxs-lookup"><span data-stu-id="1be4c-163"><a name="bPS2"></a>Provide stable links to script resources</span></span>

<span data-ttu-id="1be4c-164">Skript a související prostředky musí zůstat k dispozici v rámci dobu životnosti clusteru.</span><span class="sxs-lookup"><span data-stu-id="1be4c-164">The script and associated resources must remain available throughout the lifetime of the cluster.</span></span> <span data-ttu-id="1be4c-165">Pokud Přidání nových uzlů do clusteru během operace škálování, je nutné tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="1be4c-165">These resources are required if new nodes are added to the cluster during scaling operations.</span></span>

<span data-ttu-id="1be4c-166">Osvědčeným postupem je ke stažení a archivaci vše v účtu Azure Storage na vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="1be4c-166">The best practice is to download and archive everything in an Azure Storage account on your subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1be4c-167">Účet úložiště používané musí být výchozí účet úložiště pro cluster nebo kontejner veřejné, jen pro čtení na jiný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="1be4c-167">The storage account used must be the default storage account for the cluster or a public, read-only container on any other storage account.</span></span>

<span data-ttu-id="1be4c-168">Například ukázky od společnosti Microsoft jsou uloženy v [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="1be4c-168">For example, the samples provided by Microsoft are stored in the [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) storage account.</span></span> <span data-ttu-id="1be4c-169">Toto je veřejný, jen pro čtení kontejner udržovat týmem HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1be4c-169">This is a public, read-only container maintained by the HDInsight team.</span></span>

### <span data-ttu-id="1be4c-170"><a name="bPS4"></a>Použití předem zkompilovat prostředky</span><span class="sxs-lookup"><span data-stu-id="1be4c-170"><a name="bPS4"></a>Use pre-compiled resources</span></span>

<span data-ttu-id="1be4c-171">Pokud chcete zkrátit dobu potřebnou pro spuštění skriptu, vyhněte se operace, které zkompilovat prostředky ze zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-171">To reduce the time it takes to run the script, avoid operations that compile resources from source code.</span></span> <span data-ttu-id="1be4c-172">Například předem zkompilovat prostředky a uložit je do objektu blob účet úložiště Azure ve stejném datovém centru jako HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1be4c-172">For example, pre-compile resources and store them in an Azure Storage account blob in the same data center as HDInsight.</span></span>

### <span data-ttu-id="1be4c-173"><a name="bPS3"></a>Zajistěte, aby byl skript přizpůsobení clusteru idempotent</span><span class="sxs-lookup"><span data-stu-id="1be4c-173"><a name="bPS3"></a>Ensure that the cluster customization script is idempotent</span></span>

<span data-ttu-id="1be4c-174">Skripty musí být idempotent.</span><span class="sxs-lookup"><span data-stu-id="1be4c-174">Scripts must be idempotent.</span></span> <span data-ttu-id="1be4c-175">Pokud se skript spustí vícekrát, měla by vrátit clusteru do stejného stavu pokaždé, když.</span><span class="sxs-lookup"><span data-stu-id="1be4c-175">If the script runs multiple times, it should return the cluster to the same state every time.</span></span>

<span data-ttu-id="1be4c-176">Například skript, který upravuje konfigurační soubory by neměly přidat duplicitní položky, pokud byl spuštěn vícekrát.</span><span class="sxs-lookup"><span data-stu-id="1be4c-176">For example, a script that modifies configuration files should not add duplicate entries if ran multiple times.</span></span>

### <span data-ttu-id="1be4c-177"><a name="bPS5"></a>Zajištění vysoké dostupnosti architektury clusteru</span><span class="sxs-lookup"><span data-stu-id="1be4c-177"><a name="bPS5"></a>Ensure high availability of the cluster architecture</span></span>

<span data-ttu-id="1be4c-178">Linuxové clustery HDInsight poskytují dva head uzly, které jsou aktivní v rámci clusteru, a spusťte akcí skriptů v obou uzlech.</span><span class="sxs-lookup"><span data-stu-id="1be4c-178">Linux-based HDInsight clusters provide two head nodes that are active within the cluster, and script actions run on both nodes.</span></span> <span data-ttu-id="1be4c-179">Pokud součásti, které nainstalujete očekává pouze jeden hlavní uzel, neinstalujte součásti v obou uzlech head.</span><span class="sxs-lookup"><span data-stu-id="1be4c-179">If the components you install expect only one head node, do not install the components on both head nodes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1be4c-180">Služby, které jsou k dispozici jako součást služby HDInsight jsou navrženy pro převzetí služeb při selhání mezi dvěma uzly head podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="1be4c-180">Services provided as part of HDInsight are designed to fail over between the two head nodes as needed.</span></span> <span data-ttu-id="1be4c-181">Tato funkce není rozšířené k vlastní součásti nainstalované prostřednictvím akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-181">This functionality is not extended to custom components installed through script actions.</span></span> <span data-ttu-id="1be4c-182">Pokud potřebujete vysokou dostupnost pro vlastní součásti, je nutné implementovat vlastní mechanismus převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="1be4c-182">If you need high availability for custom components, you must implement your own failover mechanism.</span></span>

### <span data-ttu-id="1be4c-183"><a name="bPS6"></a>Konfigurace vlastní součásti, které budou používat úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="1be4c-183"><a name="bPS6"></a>Configure the custom components to use Azure Blob storage</span></span>

<span data-ttu-id="1be4c-184">Součásti, které můžete nainstalovat na cluster může mít výchozí konfigurace, který používá Hadoop Distributed File System (HDFS) úložiště.</span><span class="sxs-lookup"><span data-stu-id="1be4c-184">Components that you install on the cluster might have a default configuration that uses Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="1be4c-185">HDInsight používá jako výchozí úložiště Azure Storage nebo Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="1be4c-185">HDInsight uses either Azure Storage or Data Lake Store as the default storage.</span></span> <span data-ttu-id="1be4c-186">Zadejte oba systém HDFS kompatibilní souborů, která je uchována data i v případě odstranění clusteru.</span><span class="sxs-lookup"><span data-stu-id="1be4c-186">Both provide an HDFS compatible file system that persists data even if the cluster is deleted.</span></span> <span data-ttu-id="1be4c-187">Musíte nakonfigurovat komponenty instalaci pro použití WASB nebo ADL místo HDFS.</span><span class="sxs-lookup"><span data-stu-id="1be4c-187">You may need to configure components you install to use WASB or ADL instead of HDFS.</span></span>

<span data-ttu-id="1be4c-188">Pro většinu operací není potřeba zadat systému souborů.</span><span class="sxs-lookup"><span data-stu-id="1be4c-188">For most operations, you do not need to specify the file system.</span></span> <span data-ttu-id="1be4c-189">Například následující zkopíruje giraph-examples.jar soubor z místního systému souborů do úložiště clusteru:</span><span class="sxs-lookup"><span data-stu-id="1be4c-189">For example, the following copies the giraph-examples.jar file from the local file system to cluster storage:</span></span>

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

<span data-ttu-id="1be4c-190">V tomto příkladu `hdfs` příkaz transparentně používá výchozí úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="1be4c-190">In this example, the `hdfs` command transparently uses the default cluster storage.</span></span> <span data-ttu-id="1be4c-191">Pro některé operace budete možná muset zadat identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="1be4c-191">For some operations, you may need to specify the URI.</span></span> <span data-ttu-id="1be4c-192">Například `adl:///example/jars` pro Data Lake Store nebo `wasb:///example/jars` pro Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="1be4c-192">For example, `adl:///example/jars` for Data Lake Store or `wasb:///example/jars` for Azure Storage.</span></span>

### <span data-ttu-id="1be4c-193"><a name="bPS7"></a>Zapisuje informace do STDOUT a STDERR</span><span class="sxs-lookup"><span data-stu-id="1be4c-193"><a name="bPS7"></a>Write information to STDOUT and STDERR</span></span>

<span data-ttu-id="1be4c-194">HDInsight zaznamená výstup skriptu, který je zapsán do STDOUT a STDERR.</span><span class="sxs-lookup"><span data-stu-id="1be4c-194">HDInsight logs script output that is written to STDOUT and STDERR.</span></span> <span data-ttu-id="1be4c-195">Můžete zobrazit tyto informace pomocí Ambari webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1be4c-195">You can view this information using the Ambari web UI.</span></span>

> [!NOTE]
> <span data-ttu-id="1be4c-196">Ambari je dostupné pouze při cluster se úspěšně vytvořil.</span><span class="sxs-lookup"><span data-stu-id="1be4c-196">Ambari is only available if the cluster is successfully created.</span></span> <span data-ttu-id="1be4c-197">Pokud použijete akci skriptu během vytváření clusteru a vytvoření selže, najdete v části řešení potíží [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) pro další způsoby, jak přístup k informacím o protokolu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-197">If you use a script action during cluster creation, and creation fails, see the troubleshooting section [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) for other ways of accessing logged information.</span></span>

<span data-ttu-id="1be4c-198">Většina nástrojů a instalační balíčky již zapisovat informace do STDOUT a STDERR, ale můžete chtít přidat další protokolování.</span><span class="sxs-lookup"><span data-stu-id="1be4c-198">Most utilities and installation packages already write information to STDOUT and STDERR, however you may want to add additional logging.</span></span> <span data-ttu-id="1be4c-199">Chcete-li odeslat text do STDOUT, použijte `echo`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-199">To send text to STDOUT, use `echo`.</span></span> <span data-ttu-id="1be4c-200">Například:</span><span class="sxs-lookup"><span data-stu-id="1be4c-200">For example:</span></span>

```bash
echo "Getting ready to install Foo"
```

<span data-ttu-id="1be4c-201">Ve výchozím nastavení `echo` odešle řetězec STDOUT.</span><span class="sxs-lookup"><span data-stu-id="1be4c-201">By default, `echo` sends the string to STDOUT.</span></span> <span data-ttu-id="1be4c-202">Pro přesměrování je na STDERR, přidejte `>&2` před `echo`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-202">To direct it to STDERR, add `>&2` before `echo`.</span></span> <span data-ttu-id="1be4c-203">Například:</span><span class="sxs-lookup"><span data-stu-id="1be4c-203">For example:</span></span>

```bash
>&2 echo "An error occurred installing Foo"
```

<span data-ttu-id="1be4c-204">To přesměruje informace místo zapsána do STDOUT do STDERR (2).</span><span class="sxs-lookup"><span data-stu-id="1be4c-204">This redirects information written to STDOUT to STDERR (2) instead.</span></span> <span data-ttu-id="1be4c-205">Další informace o přesměrování vstupů/výstupů, najdete v části [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span><span class="sxs-lookup"><span data-stu-id="1be4c-205">For more information on IO redirection, see [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span></span>

<span data-ttu-id="1be4c-206">Další informace o zobrazení informací zaznamenaných akcí skriptů naleznete v tématu [clusterů HDInsight přizpůsobit pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span><span class="sxs-lookup"><span data-stu-id="1be4c-206">For more information on viewing information logged by script actions, see [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span></span>

### <span data-ttu-id="1be4c-207"><a name="bps8"></a>Ukládat soubory ve formátu ASCII s LF konce řádků</span><span class="sxs-lookup"><span data-stu-id="1be4c-207"><a name="bps8"></a> Save files as ASCII with LF line endings</span></span>

<span data-ttu-id="1be4c-208">Skripty bash být uloženy ve formě formátu ASCII, se ukončila příkazem LF řádky.</span><span class="sxs-lookup"><span data-stu-id="1be4c-208">Bash scripts should be stored as ASCII format, with lines terminated by LF.</span></span> <span data-ttu-id="1be4c-209">Soubory, které jsou uloženy jako UTF-8, nebo použijte Line FEED jako ukončuje řádku může selhat s následující chybou:</span><span class="sxs-lookup"><span data-stu-id="1be4c-209">Files that are stored as UTF-8, or use CRLF as the line ending may fail with the following error:</span></span>

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <span data-ttu-id="1be4c-210"><a name="bps9"></a>Logika opakovaných pokusů použít k obnovení z přechodné chyby</span><span class="sxs-lookup"><span data-stu-id="1be4c-210"><a name="bps9"></a> Use retry logic to recover from transient errors</span></span>

<span data-ttu-id="1be4c-211">Při stahování souborů, instalace balíčků pomocí výstižný get nebo jiné akce, které přenášet data přes internet, akce se pravděpodobně nezdaří z důvodu přechodné chyby sítě.</span><span class="sxs-lookup"><span data-stu-id="1be4c-211">When downloading files, installing packages using apt-get, or other actions that transmit data over the internet, the action may fail due to transient networking errors.</span></span> <span data-ttu-id="1be4c-212">Vzdálený prostředek, který komunikuje se například může být právě převzetí služeb při selhání zálohování uzlu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-212">For example, the remote resource you are communicating with may be in the process of failing over to a backup node.</span></span>

<span data-ttu-id="1be4c-213">Chcete-li váš skript odolné přechodné chyby, můžete implementovat logiku opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="1be4c-213">To make your script resilient to transient errors, you can implement retry logic.</span></span> <span data-ttu-id="1be4c-214">Následující funkce ukazuje, jak implementovat logiku opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="1be4c-214">The following function demonstrates how to implement retry logic.</span></span> <span data-ttu-id="1be4c-215">Se pokusí operaci třikrát než selže.</span><span class="sxs-lookup"><span data-stu-id="1be4c-215">It retries the operation three times before failing.</span></span>

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

<span data-ttu-id="1be4c-216">Následující příklady ukazují, jak použít tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="1be4c-216">The following examples demonstrate how to use this function.</span></span>

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <span data-ttu-id="1be4c-217"><a name="helpermethods"></a>Pomocné metody pro vlastní skripty</span><span class="sxs-lookup"><span data-stu-id="1be4c-217"><a name="helpermethods"></a>Helper methods for custom scripts</span></span>

<span data-ttu-id="1be4c-218">Pomocné metody akcí skriptů jsou nástroje, které můžete použít při zápisu vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="1be4c-218">Script action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="1be4c-219">Tyto metody jsou součástí[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) skriptu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-219">These methods are contained in the[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) script.</span></span> <span data-ttu-id="1be4c-220">Stáhnout a použít je jako součást vašeho skriptu, použijte následující:</span><span class="sxs-lookup"><span data-stu-id="1be4c-220">Use the following to download and use them as part of your script:</span></span>

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

<span data-ttu-id="1be4c-221">Následující pomocníky dostupné k použití ve vašem skriptu:</span><span class="sxs-lookup"><span data-stu-id="1be4c-221">The following helpers available for use in your script:</span></span>

| <span data-ttu-id="1be4c-222">Použití pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="1be4c-222">Helper usage</span></span> | <span data-ttu-id="1be4c-223">Popis</span><span class="sxs-lookup"><span data-stu-id="1be4c-223">Description</span></span> |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |<span data-ttu-id="1be4c-224">Stahování souboru ze zdroje URI do zadaná cesta k souboru.</span><span class="sxs-lookup"><span data-stu-id="1be4c-224">Downloads a file from the source URI to the specified file path.</span></span> <span data-ttu-id="1be4c-225">Ve výchozím nastavení je nepřepíše existující soubor.</span><span class="sxs-lookup"><span data-stu-id="1be4c-225">By default, it does not overwrite an existing file.</span></span> |
| `untar_file TARFILE DESTDIR` |<span data-ttu-id="1be4c-226">Extrahuje vkládání soubor (pomocí `-xf`) do cílového adresáře.</span><span class="sxs-lookup"><span data-stu-id="1be4c-226">Extracts a tar file (using `-xf`) to the destination directory.</span></span> |
| `test_is_headnode` |<span data-ttu-id="1be4c-227">Pokud byla spuštěna hlavního uzlu clusteru, návratový 1; jinak hodnota 0.</span><span class="sxs-lookup"><span data-stu-id="1be4c-227">If ran on a cluster head node, return 1; otherwise, 0.</span></span> |
| `test_is_datanode` |<span data-ttu-id="1be4c-228">Pokud je aktuální uzel uzlem dat (pracovník), vrátí 1; jinak hodnota 0.</span><span class="sxs-lookup"><span data-stu-id="1be4c-228">If the current node is a data (worker) node, return a 1; otherwise, 0.</span></span> |
| `test_is_first_datanode` |<span data-ttu-id="1be4c-229">Pokud je aktuální uzel první data (pracovník) uzlu (s názvem workernode0) vrátí 1; jinak hodnota 0.</span><span class="sxs-lookup"><span data-stu-id="1be4c-229">If the current node is the first data (worker) node (named workernode0) return a 1; otherwise, 0.</span></span> |
| `get_headnodes` |<span data-ttu-id="1be4c-230">Vrátí název plně kvalifikované domény headnodes v clusteru.</span><span class="sxs-lookup"><span data-stu-id="1be4c-230">Return the fully qualified domain name of the headnodes in the cluster.</span></span> <span data-ttu-id="1be4c-231">Názvy jsou oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="1be4c-231">Names are comma delimited.</span></span> <span data-ttu-id="1be4c-232">Chyba se vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="1be4c-232">An empty string is returned on error.</span></span> |
| `get_primary_headnode` |<span data-ttu-id="1be4c-233">Získá název plně kvalifikované domény primární headnode.</span><span class="sxs-lookup"><span data-stu-id="1be4c-233">Gets the fully qualified domain name of the primary headnode.</span></span> <span data-ttu-id="1be4c-234">Chyba se vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="1be4c-234">An empty string is returned on error.</span></span> |
| `get_secondary_headnode` |<span data-ttu-id="1be4c-235">Získá název plně kvalifikované domény sekundární headnode.</span><span class="sxs-lookup"><span data-stu-id="1be4c-235">Gets the fully qualified domain name of the secondary headnode.</span></span> <span data-ttu-id="1be4c-236">Chyba se vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="1be4c-236">An empty string is returned on error.</span></span> |
| `get_primary_headnode_number` |<span data-ttu-id="1be4c-237">Získá číselnou příponou primární headnode.</span><span class="sxs-lookup"><span data-stu-id="1be4c-237">Gets the numeric suffix of the primary headnode.</span></span> <span data-ttu-id="1be4c-238">Chyba se vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="1be4c-238">An empty string is returned on error.</span></span> |
| `get_secondary_headnode_number` |<span data-ttu-id="1be4c-239">Získá číselnou příponou sekundární headnode.</span><span class="sxs-lookup"><span data-stu-id="1be4c-239">Gets the numeric suffix of the secondary headnode.</span></span> <span data-ttu-id="1be4c-240">Chyba se vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="1be4c-240">An empty string is returned on error.</span></span> |

## <span data-ttu-id="1be4c-241"><a name="commonusage"></a>Obecné vzory využití</span><span class="sxs-lookup"><span data-stu-id="1be4c-241"><a name="commonusage"></a>Common usage patterns</span></span>

<span data-ttu-id="1be4c-242">Tato část obsahuje pokyny k implementaci některých běžných vzorů využití, které se mohou vyskytnout při zápisu vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="1be4c-242">This section provides guidance on implementing some of the common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="passing-parameters-to-a-script"></a><span data-ttu-id="1be4c-243">Předávání parametrů skriptu</span><span class="sxs-lookup"><span data-stu-id="1be4c-243">Passing parameters to a script</span></span>

<span data-ttu-id="1be4c-244">V některých případech může váš skript vyžadovat parametry.</span><span class="sxs-lookup"><span data-stu-id="1be4c-244">In some cases, your script may require parameters.</span></span> <span data-ttu-id="1be4c-245">Například můžete potřebovat heslo správce pro cluster při pomocí nástroje Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="1be4c-245">For example, you may need the admin password for the cluster when using the Ambari REST API.</span></span>

<span data-ttu-id="1be4c-246">Parametry předané do skriptu se označují jako *poziční parametry*a jsou přiřazeny k `$1` pro první parametr `$2` pro druhý a proto v.</span><span class="sxs-lookup"><span data-stu-id="1be4c-246">Parameters passed to the script are known as *positional parameters*, and are assigned to `$1` for the first parameter, `$2` for the second, and so-on.</span></span> <span data-ttu-id="1be4c-247">`$0`obsahuje název samotný skript.</span><span class="sxs-lookup"><span data-stu-id="1be4c-247">`$0` contains the name of the script itself.</span></span>

<span data-ttu-id="1be4c-248">Hodnoty předány do skriptu jako parametry by měl být uzavřen v jednoduchých uvozovkách (').</span><span class="sxs-lookup"><span data-stu-id="1be4c-248">Values passed to the script as parameters should be enclosed by single quotes (').</span></span> <span data-ttu-id="1be4c-249">Tím zajistíte, že předaný hodnota je považována za literál.</span><span class="sxs-lookup"><span data-stu-id="1be4c-249">Doing so ensures that the passed value is treated as a literal.</span></span>

### <a name="setting-environment-variables"></a><span data-ttu-id="1be4c-250">Nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="1be4c-250">Setting environment variables</span></span>

<span data-ttu-id="1be4c-251">Nastavení proměnné prostředí provádí následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1be4c-251">Setting an environment variable is performed by the following statement:</span></span>

    VARIABLENAME=value

<span data-ttu-id="1be4c-252">Kde NÁZEVPROMĚNNÉ je název proměnné.</span><span class="sxs-lookup"><span data-stu-id="1be4c-252">Where VARIABLENAME is the name of the variable.</span></span> <span data-ttu-id="1be4c-253">Chcete-li získat přístup k proměnné, použijte `$VARIABLENAME`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-253">To access the variable, use `$VARIABLENAME`.</span></span> <span data-ttu-id="1be4c-254">Například pokud chcete přiřadit hodnotu poskytované poziční parametr jako proměnnou prostředí s názvem heslo, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1be4c-254">For example, to assign a value provided by a positional parameter as an environment variable named PASSWORD, you would use the following statement:</span></span>

    PASSWORD=$1

<span data-ttu-id="1be4c-255">Poté můžete použít následující přístup k informacím `$PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-255">Subsequent access to the information could then use `$PASSWORD`.</span></span>

<span data-ttu-id="1be4c-256">Proměnné prostředí nastavit v rámci skriptu existovat pouze v rámci oboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-256">Environment variables set within the script only exist within the scope of the script.</span></span> <span data-ttu-id="1be4c-257">V některých případech musíte přidat systémové proměnné, které se uchová po dokončení skriptu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-257">In some cases, you may need to add system-wide environment variables that will persist after the script has finished.</span></span> <span data-ttu-id="1be4c-258">Přidání proměnné systémového prostředí, přidat proměnnou `/etc/environment`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-258">To add system-wide environment variables, add the variable to `/etc/environment`.</span></span> <span data-ttu-id="1be4c-259">Například následující příkaz přidá `HADOOP_CONF_DIR`:</span><span class="sxs-lookup"><span data-stu-id="1be4c-259">For example, the following statement adds `HADOOP_CONF_DIR`:</span></span>

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a><span data-ttu-id="1be4c-260">Přístup k umístění, kde jsou uloženy vlastní skripty</span><span class="sxs-lookup"><span data-stu-id="1be4c-260">Access to locations where the custom scripts are stored</span></span>

<span data-ttu-id="1be4c-261">Skripty použít pro přizpůsobení clusteru, musí být uložen v jedné z následujících umístění:</span><span class="sxs-lookup"><span data-stu-id="1be4c-261">Scripts used to customize a cluster needs to be stored in one of the following locations:</span></span>

* <span data-ttu-id="1be4c-262">__Účet úložiště Azure__ který je přidružen cluster.</span><span class="sxs-lookup"><span data-stu-id="1be4c-262">An __Azure Storage account__ that is associated with the cluster.</span></span>

* <span data-ttu-id="1be4c-263">__Účtu další úložiště__ přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="1be4c-263">An __additional storage account__ associated with the cluster.</span></span>

* <span data-ttu-id="1be4c-264">A __veřejně čitelné URI__.</span><span class="sxs-lookup"><span data-stu-id="1be4c-264">A __publicly readable URI__.</span></span> <span data-ttu-id="1be4c-265">Například adresa URL pro data uložená na OneDrive, Dropbox nebo jiný soubor, který je hostitelem služby.</span><span class="sxs-lookup"><span data-stu-id="1be4c-265">For example, a URL to data stored on OneDrive, Dropbox, or other file hosting service.</span></span>

* <span data-ttu-id="1be4c-266">__Účtu Azure Data Lake Store__ který je přidružen HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="1be4c-266">An __Azure Data Lake Store account__ that is associated with the HDInsight cluster.</span></span> <span data-ttu-id="1be4c-267">Další informace o používání Azure Data Lake Store s HDInsight naleznete v tématu [vytvoření clusteru HDInsight s Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1be4c-267">For more information on using Azure Data Lake Store with HDInsight, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="1be4c-268">Objekt služby, které HDInsight používá pro přístup k Data Lake Store, musí mít přístup pro čtení do skriptu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-268">The service principal HDInsight uses to access Data Lake Store must have read access to the script.</span></span>

<span data-ttu-id="1be4c-269">Prostředky používané ve skriptu musí být také veřejně dostupné.</span><span class="sxs-lookup"><span data-stu-id="1be4c-269">Resources used by the script must also be publicly available.</span></span>

<span data-ttu-id="1be4c-270">Ukládání souborů v účtu úložiště Azure nebo Azure Data Lake Store poskytuje rychlý přístup, jako obě sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="1be4c-270">Storing the files in an Azure Storage account or Azure Data Lake Store provides fast access, as both within the Azure network.</span></span>

> [!NOTE]
> <span data-ttu-id="1be4c-271">Formát identifikátoru URI, slouží k odkazování skript se liší v závislosti na službě, kterou právě používá.</span><span class="sxs-lookup"><span data-stu-id="1be4c-271">The URI format used to reference the script differs depending on the service being used.</span></span> <span data-ttu-id="1be4c-272">Pro účty úložiště přidružené ke clusteru HDInsight, použijte `wasb://` nebo `wasbs://`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-272">For storage accounts associated with the HDInsight cluster, use `wasb://` or `wasbs://`.</span></span> <span data-ttu-id="1be4c-273">Veřejně čitelné identifikátory URI, použijte `http://` nebo `https://`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-273">For publicly readable URIs, use `http://` or `https://`.</span></span> <span data-ttu-id="1be4c-274">Pro Data Lake Store, použijte `adl://`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-274">For Data Lake Store, use `adl://`.</span></span>

### <a name="checking-the-operating-system-version"></a><span data-ttu-id="1be4c-275">Kontrola verze operačního systému</span><span class="sxs-lookup"><span data-stu-id="1be4c-275">Checking the operating system version</span></span>

<span data-ttu-id="1be4c-276">Různé verze HDInsight spoléhají na určité verze Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-276">Different versions of HDInsight rely on specific versions of Ubuntu.</span></span> <span data-ttu-id="1be4c-277">Mohou existovat rozdíly mezi verzemi operačního systému, které musí vyhledat ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-277">There may be differences between OS versions that you must check for in your script.</span></span> <span data-ttu-id="1be4c-278">Potřebujete například nainstalovat binární soubor, který je vázaný na verze Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-278">For example, you may need to install a binary that is tied to the version of Ubuntu.</span></span>

<span data-ttu-id="1be4c-279">Pokud chcete zkontrolovat verzi operačního systému, použijte `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-279">To check the OS version, use `lsb_release`.</span></span> <span data-ttu-id="1be4c-280">Například následující skript ukazuje, jak odkazovat na konkrétní vkládání soubor v závislosti na verzi operačního systému:</span><span class="sxs-lookup"><span data-stu-id="1be4c-280">For example, the following script demonstrates how to reference a specific tar file depending on the OS version:</span></span>

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <span data-ttu-id="1be4c-281"><a name="deployScript"></a>Kontrolní seznam pro nasazení akce skriptu</span><span class="sxs-lookup"><span data-stu-id="1be4c-281"><a name="deployScript"></a>Checklist for deploying a script action</span></span>

<span data-ttu-id="1be4c-282">Zde jsou kroky, které jsme trvalo při přípravě nasazení těchto skriptů:</span><span class="sxs-lookup"><span data-stu-id="1be4c-282">Here are the steps we took when preparing to deploy these scripts:</span></span>

* <span data-ttu-id="1be4c-283">Uveďte soubory, které obsahují vlastní skripty na místě, která je přístupná na uzlech clusteru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="1be4c-283">Put the files that contain the custom scripts in a place that is accessible by the cluster nodes during deployment.</span></span> <span data-ttu-id="1be4c-284">Například výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="1be4c-284">For example, the default storage for the cluster.</span></span> <span data-ttu-id="1be4c-285">Soubory můžete také uloženy ve veřejně čitelné hostitelských služeb.</span><span class="sxs-lookup"><span data-stu-id="1be4c-285">Files can also be stored in publicly readable hosting services.</span></span>
* <span data-ttu-id="1be4c-286">Ověřte, že skript impotent.</span><span class="sxs-lookup"><span data-stu-id="1be4c-286">Verify that the script is impotent.</span></span> <span data-ttu-id="1be4c-287">To uděláte skript, který chcete spustit několikrát na stejném uzlu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-287">Doing so allows the script to be executed multiple times on the same node.</span></span>
* <span data-ttu-id="1be4c-288">Pomocí TMP dočasný soubor directory zachovat stažené soubory, které skripty používají a pak vyčištění je po mají spouštět skripty.</span><span class="sxs-lookup"><span data-stu-id="1be4c-288">Use a temporary file directory /tmp to keep the downloaded files used by the scripts and then clean them up after scripts have executed.</span></span>
* <span data-ttu-id="1be4c-289">Pokud se změní nastavení na úrovni operačního systému nebo soubory konfigurace služby Hadoop, můžete restartovat služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1be4c-289">If OS-level settings or Hadoop service configuration files are changed, you may want to restart HDInsight services.</span></span>

## <span data-ttu-id="1be4c-290"><a name="runScriptAction"></a>Jak spustit akci skriptu</span><span class="sxs-lookup"><span data-stu-id="1be4c-290"><a name="runScriptAction"></a>How to run a script action</span></span>

<span data-ttu-id="1be4c-291">Akce skriptu můžete použít k přizpůsobení clusterů HDInsight pomocí následujících metod:</span><span class="sxs-lookup"><span data-stu-id="1be4c-291">You can use script actions to customize HDInsight clusters using the following methods:</span></span>

* <span data-ttu-id="1be4c-292">portál Azure</span><span class="sxs-lookup"><span data-stu-id="1be4c-292">Azure portal</span></span>
* <span data-ttu-id="1be4c-293">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1be4c-293">Azure PowerShell</span></span>
* <span data-ttu-id="1be4c-294">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="1be4c-294">Azure Resource Manager templates</span></span>
* <span data-ttu-id="1be4c-295">.NET SDK služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1be4c-295">The HDInsight .NET SDK.</span></span>

<span data-ttu-id="1be4c-296">Další informace o použití jednotlivých metod najdete v tématu [způsob použití akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1be4c-296">For more information on using each method, see [How to use script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="1be4c-297"><a name="sampleScripts"></a>Ukázky vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="1be4c-297"><a name="sampleScripts"></a>Custom script samples</span></span>

<span data-ttu-id="1be4c-298">Společnost Microsoft poskytuje ukázkové skripty pro instalaci součásti v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1be4c-298">Microsoft provides sample scripts to install components on an HDInsight cluster.</span></span> <span data-ttu-id="1be4c-299">V následujících tématech pro další příklad akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="1be4c-299">See the following links for more example script actions.</span></span>

* [<span data-ttu-id="1be4c-300">Nainstalovat a používat Hue clustery prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="1be4c-300">Install and use Hue on HDInsight clusters</span></span>](hdinsight-hadoop-hue-linux.md)
* [<span data-ttu-id="1be4c-301">Nainstalovat a používat Solr clustery prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="1be4c-301">Install and use Solr on HDInsight clusters</span></span>](hdinsight-hadoop-solr-install-linux.md)
* [<span data-ttu-id="1be4c-302">Nainstalovat a používat Giraph clustery prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="1be4c-302">Install and use Giraph on HDInsight clusters</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="1be4c-303">Nainstalovat nebo upgradovat Mono na clustery HDInsight</span><span class="sxs-lookup"><span data-stu-id="1be4c-303">Install or upgrade Mono on HDInsight clusters</span></span>](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a><span data-ttu-id="1be4c-304">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="1be4c-304">Troubleshooting</span></span>

<span data-ttu-id="1be4c-305">Tady jsou chyby, se můžete setkat při používání skripty, které jste si vytvořili:</span><span class="sxs-lookup"><span data-stu-id="1be4c-305">The following are errors you may encounter when using scripts you have developed:</span></span>

<span data-ttu-id="1be4c-306">**Chyba**: `$'\r': command not found`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-306">**Error**: `$'\r': command not found`.</span></span> <span data-ttu-id="1be4c-307">Někdy následuje `syntax error: unexpected end of file`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-307">Sometimes followed by `syntax error: unexpected end of file`.</span></span>

<span data-ttu-id="1be4c-308">*Příčina*: k této chybě dojde, pokud řádky ve skriptu končit Line FEED.</span><span class="sxs-lookup"><span data-stu-id="1be4c-308">*Cause*: This error is caused when the lines in a script end with CRLF.</span></span> <span data-ttu-id="1be4c-309">Systémy UNIX očekávat pouze LF jako ukončování řádků.</span><span class="sxs-lookup"><span data-stu-id="1be4c-309">Unix systems expect only LF as the line ending.</span></span>

<span data-ttu-id="1be4c-310">Tento problém nejčastěji nastane, když skript je vytvořené v prostředí Windows, protože Line FEED je běžné řádku ukončení pro mnoho textové editory v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="1be4c-310">This problem most often occurs when the script is authored on a Windows environment, as CRLF is a common line ending for many text editors on Windows.</span></span>

<span data-ttu-id="1be4c-311">*Řešení*: Pokud je možnost v textovém editoru, vyberte formát systému Unix nebo LF pro ukončování řádků.</span><span class="sxs-lookup"><span data-stu-id="1be4c-311">*Resolution*: If it is an option in your text editor, select Unix format or LF for the line ending.</span></span> <span data-ttu-id="1be4c-312">Ke změně Line FEED LF může také použít následující příkazy v systému Unix:</span><span class="sxs-lookup"><span data-stu-id="1be4c-312">You may also use the following commands on a Unix system to change the CRLF to an LF:</span></span>

> [!NOTE]
> <span data-ttu-id="1be4c-313">V tom, že by se měl změnit konce řádků Line FEED na LF jsou přibližně ekvivalentem následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="1be4c-313">The following commands are roughly equivalent in that they should change the CRLF line endings to LF.</span></span> <span data-ttu-id="1be4c-314">Vyberte jeden podle nástrojů dostupných v systému.</span><span class="sxs-lookup"><span data-stu-id="1be4c-314">Select one based on the utilities available on your system.</span></span>

| <span data-ttu-id="1be4c-315">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1be4c-315">Command</span></span> | <span data-ttu-id="1be4c-316">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1be4c-316">Notes</span></span> |
| --- | --- |
| `unix2dos -b INFILE` |<span data-ttu-id="1be4c-317">Původní soubor je zálohovaný s. BAK rozšíření</span><span class="sxs-lookup"><span data-stu-id="1be4c-317">The original file is backed up with a .BAK extension</span></span> |
| `tr -d '\r' < INFILE > OUTFILE` |<span data-ttu-id="1be4c-318">VÝSTUPNÍ_SOUBOR obsahuje verzi s pouze LF zakončení</span><span class="sxs-lookup"><span data-stu-id="1be4c-318">OUTFILE contains a version with only LF endings</span></span> |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | <span data-ttu-id="1be4c-319">Upraví soubor přímo</span><span class="sxs-lookup"><span data-stu-id="1be4c-319">Modifies the file directly</span></span> |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |<span data-ttu-id="1be4c-320">VÝSTUPNÍ_SOUBOR obsahuje verzi s pouze LF zakončení.</span><span class="sxs-lookup"><span data-stu-id="1be4c-320">OUTFILE contains a version with only LF endings.</span></span> |

<span data-ttu-id="1be4c-321">**Chyba**: `line 1: #!/usr/bin/env: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="1be4c-321">**Error**: `line 1: #!/usr/bin/env: No such file or directory`.</span></span>

<span data-ttu-id="1be4c-322">*Příčina*: k této chybě dojde, když skript se uložil jako UTF-8 s značky pořadí bajtů (BOM).</span><span class="sxs-lookup"><span data-stu-id="1be4c-322">*Cause*: This error occurs when the script was saved as UTF-8 with a Byte Order Mark (BOM).</span></span>

<span data-ttu-id="1be4c-323">*Řešení*: uložte soubor ve formátu ASCII nebo jako UTF-8 bez Kusovník.</span><span class="sxs-lookup"><span data-stu-id="1be4c-323">*Resolution*: Save the file either as ASCII, or as UTF-8 without a BOM.</span></span> <span data-ttu-id="1be4c-324">Vytvoření souboru bez Kusovníku může také použít následující příkaz v systému Linux nebo Unix:</span><span class="sxs-lookup"><span data-stu-id="1be4c-324">You may also use the following command on a Linux or Unix system to create a file without the BOM:</span></span>

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

<span data-ttu-id="1be4c-325">Nahraďte `INFILE` v souboru s Kusovníku.</span><span class="sxs-lookup"><span data-stu-id="1be4c-325">Replace `INFILE` with the file containing the BOM.</span></span> <span data-ttu-id="1be4c-326">`OUTFILE`musí být nový název souboru, který obsahuje skript bez Kusovníku.</span><span class="sxs-lookup"><span data-stu-id="1be4c-326">`OUTFILE` should be a new file name, which contains the script without the BOM.</span></span>

## <span data-ttu-id="1be4c-327"><a name="seeAlso"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="1be4c-327"><a name="seeAlso"></a>Next steps</span></span>

* <span data-ttu-id="1be4c-328">Zjistěte, jak [clusterů HDInsight přizpůsobit pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="1be4c-328">Learn how to [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
* <span data-ttu-id="1be4c-329">Použití [referenční informace sady SDK rozhraní .NET HDInsight](https://msdn.microsoft.com/library/mt271028.aspx) Další informace o vytváření aplikací rozhraní .NET, které spravují HDInsight</span><span class="sxs-lookup"><span data-stu-id="1be4c-329">Use the [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx) to learn more about creating .NET applications that manage HDInsight</span></span>
* <span data-ttu-id="1be4c-330">Použít [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) se dozvíte, jak provádět akce správy v clusterech prostředí HDInsight pomocí REST.</span><span class="sxs-lookup"><span data-stu-id="1be4c-330">Use the [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) to learn how to use REST to perform management actions on HDInsight clusters.</span></span>
