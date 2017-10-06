---
title: "vývoj akcí aaaScript s HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Bash skriptů toocustomize clustery HDInsight se systémem Linux. Hello funkce akce skriptu HDInsight umožňuje skripty toorun během nebo po vytvoření clusteru. Skripty můžete být použité toochange nastavení konfigurace clusteru nebo nainstalovat další software."
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
ms.openlocfilehash: 1f504b00365df5f4cfb3ae19ad55ff7630342650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="script-action-development-with-hdinsight"></a><span data-ttu-id="555b7-105">Vývoj akcí skriptů v prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="555b7-105">Script action development with HDInsight</span></span>

<span data-ttu-id="555b7-106">Zjistěte, jak pomocí clusteru HDInsight Bash toocustomize skripty.</span><span class="sxs-lookup"><span data-stu-id="555b7-106">Learn how toocustomize your HDInsight cluster using Bash scripts.</span></span> <span data-ttu-id="555b7-107">Akce skriptů jsou způsob toocustomize HDInsight během nebo po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="555b7-107">Script actions are a way toocustomize HDInsight during or after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="555b7-108">Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="555b7-108">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="555b7-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="555b7-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="555b7-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="555b7-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="what-are-script-actions"></a><span data-ttu-id="555b7-111">Jaké jsou akce skriptu</span><span class="sxs-lookup"><span data-stu-id="555b7-111">What are script actions</span></span>

<span data-ttu-id="555b7-112">Skript akce se skripty Bash, které Azure je spuštěná na změny konfigurace toomake uzly clusteru hello nebo instalovat software.</span><span class="sxs-lookup"><span data-stu-id="555b7-112">Script actions are Bash scripts that Azure runs on hello cluster nodes toomake configuration changes or install software.</span></span> <span data-ttu-id="555b7-113">Akce skriptu se spustí jako kořenového adresáře a poskytuje plný přístup uzly clusteru toohello práva.</span><span class="sxs-lookup"><span data-stu-id="555b7-113">A script action is executed as root, and provides full access rights toohello cluster nodes.</span></span>

<span data-ttu-id="555b7-114">Skript akce se dá použít prostřednictvím hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="555b7-114">Script actions can be applied through hello following methods:</span></span>

| <span data-ttu-id="555b7-115">Pomocí této metody tooapply skript...</span><span class="sxs-lookup"><span data-stu-id="555b7-115">Use this method tooapply a script...</span></span> | <span data-ttu-id="555b7-116">Při vytvoření clusteru...</span><span class="sxs-lookup"><span data-stu-id="555b7-116">During cluster creation...</span></span> | <span data-ttu-id="555b7-117">Na clusteru s podporou spuštěné...</span><span class="sxs-lookup"><span data-stu-id="555b7-117">On a running cluster...</span></span> |
| --- |:---:|:---:|
| <span data-ttu-id="555b7-118">portál Azure</span><span class="sxs-lookup"><span data-stu-id="555b7-118">Azure portal</span></span> |<span data-ttu-id="555b7-119">✓</span><span class="sxs-lookup"><span data-stu-id="555b7-119">✓</span></span> |<span data-ttu-id="555b7-120">✓</span><span class="sxs-lookup"><span data-stu-id="555b7-120">✓</span></span> |
| <span data-ttu-id="555b7-121">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="555b7-121">Azure PowerShell</span></span> |<span data-ttu-id="555b7-122">✓</span><span class="sxs-lookup"><span data-stu-id="555b7-122">✓</span></span> |<span data-ttu-id="555b7-123">✓</span><span class="sxs-lookup"><span data-stu-id="555b7-123">✓</span></span> |
| <span data-ttu-id="555b7-124">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="555b7-124">Azure CLI</span></span> |&nbsp; |<span data-ttu-id="555b7-125">✓</span><span class="sxs-lookup"><span data-stu-id="555b7-125">✓</span></span> |
| <span data-ttu-id="555b7-126">Sada HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="555b7-126">HDInsight .NET SDK</span></span> |<span data-ttu-id="555b7-127">✓</span><span class="sxs-lookup"><span data-stu-id="555b7-127">✓</span></span> |<span data-ttu-id="555b7-128">✓</span><span class="sxs-lookup"><span data-stu-id="555b7-128">✓</span></span> |
| <span data-ttu-id="555b7-129">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="555b7-129">Azure Resource Manager Template</span></span> |<span data-ttu-id="555b7-130">✓</span><span class="sxs-lookup"><span data-stu-id="555b7-130">✓</span></span> |&nbsp; |

<span data-ttu-id="555b7-131">Další informace o použití akce skriptu tooapply těchto metod najdete v části [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="555b7-131">For more information on using these methods tooapply script actions, see [Customize HDInsight clusters using script actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="555b7-132"><a name="bestPracticeScripting"></a>Osvědčené postupy pro vývoj skriptů</span><span class="sxs-lookup"><span data-stu-id="555b7-132"><a name="bestPracticeScripting"></a>Best practices for script development</span></span>

<span data-ttu-id="555b7-133">Při vývoji vlastních skriptů pro cluster služby HDInsight, existuje několik osvědčených postupů tookeep nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="555b7-133">When you develop a custom script for an HDInsight cluster, there are several best practices tookeep in mind:</span></span>

* [<span data-ttu-id="555b7-134">Cílová verze Hadoop hello</span><span class="sxs-lookup"><span data-stu-id="555b7-134">Target hello Hadoop version</span></span>](#bPS1)
* [<span data-ttu-id="555b7-135">Cíl hello verze operačního systému</span><span class="sxs-lookup"><span data-stu-id="555b7-135">Target hello OS Version</span></span>](#bps10)
* [<span data-ttu-id="555b7-136">Zadejte, že stabilní odkazy tooscript prostředky</span><span class="sxs-lookup"><span data-stu-id="555b7-136">Provide stable links tooscript resources</span></span>](#bPS2)
* [<span data-ttu-id="555b7-137">Použití předem zkompilovat prostředky</span><span class="sxs-lookup"><span data-stu-id="555b7-137">Use pre-compiled resources</span></span>](#bPS4)
* [<span data-ttu-id="555b7-138">Zajistěte, aby hello clusteru přizpůsobení skriptu idempotent</span><span class="sxs-lookup"><span data-stu-id="555b7-138">Ensure that hello cluster customization script is idempotent</span></span>](#bPS3)
* [<span data-ttu-id="555b7-139">Ujistěte se, vysokou dostupnost clusteru architektury hello</span><span class="sxs-lookup"><span data-stu-id="555b7-139">Ensure high availability of hello cluster architecture</span></span>](#bPS5)
* [<span data-ttu-id="555b7-140">Konfigurace úložiště objektů Azure Blob toouse hello vlastní komponenty</span><span class="sxs-lookup"><span data-stu-id="555b7-140">Configure hello custom components toouse Azure Blob storage</span></span>](#bPS6)
* [<span data-ttu-id="555b7-141">Zápis tooSTDOUT informace a STDERR</span><span class="sxs-lookup"><span data-stu-id="555b7-141">Write information tooSTDOUT and STDERR</span></span>](#bPS7)
* [<span data-ttu-id="555b7-142">Ukládat soubory ve formátu ASCII s LF konce řádků</span><span class="sxs-lookup"><span data-stu-id="555b7-142">Save files as ASCII with LF line endings</span></span>](#bps8)
* [<span data-ttu-id="555b7-143">Použití opakování logiku toorecover z přechodné chyby</span><span class="sxs-lookup"><span data-stu-id="555b7-143">Use retry logic toorecover from transient errors</span></span>](#bps9)

> [!IMPORTANT]
> <span data-ttu-id="555b7-144">Akce skriptu musíte dokončit během 60 minut nebo hello selže.</span><span class="sxs-lookup"><span data-stu-id="555b7-144">Script actions must complete within 60 minutes or hello process fails.</span></span> <span data-ttu-id="555b7-145">Při zřizování uzlu, hello skript spustí souběžně jiné procesy instalací a konfigurací.</span><span class="sxs-lookup"><span data-stu-id="555b7-145">During node provisioning, hello script runs concurrently with other setup and configuration processes.</span></span> <span data-ttu-id="555b7-146">Soutěž o zdroje, jako je například šířky pásma procesoru čas nebo v síti může způsobit hello skriptu tootake delší toofinish než ve vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="555b7-146">Competition for resources such as CPU time or network bandwidth may cause hello script tootake longer toofinish than it does in your development environment.</span></span>

### <span data-ttu-id="555b7-147"><a name="bPS1"></a>Cílová verze Hadoop hello</span><span class="sxs-lookup"><span data-stu-id="555b7-147"><a name="bPS1"></a>Target hello Hadoop version</span></span>

<span data-ttu-id="555b7-148">Různé verze HDInsight mají různé verze služby Hadoop a nainstalovány součásti.</span><span class="sxs-lookup"><span data-stu-id="555b7-148">Different versions of HDInsight have different versions of Hadoop services and components installed.</span></span> <span data-ttu-id="555b7-149">Pokud váš skript očekává na konkrétní verzi služeb nebo součásti, měli používat jenom hello skriptu hello verzi HDInsight, které obsahuje hello požadované součásti.</span><span class="sxs-lookup"><span data-stu-id="555b7-149">If your script expects a specific version of a service or component, you should only use hello script with hello version of HDInsight that includes hello required components.</span></span> <span data-ttu-id="555b7-150">Informace můžete najít na verze součástí, které jsou součástí HDInsight pomocí hello [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="555b7-150">You can find information on component versions included with HDInsight using hello [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

### <span data-ttu-id="555b7-151"><a name="bps10"></a>Cílová verze hello operačního systému</span><span class="sxs-lookup"><span data-stu-id="555b7-151"><a name="bps10"></a> Target hello OS version</span></span>

<span data-ttu-id="555b7-152">HDInsight se systémem Linux je založena na hello distribuční Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="555b7-152">Linux-based HDInsight is based on hello Ubuntu Linux distribution.</span></span> <span data-ttu-id="555b7-153">Různé verze HDInsight využívají různé verze Ubuntu, což může změnit chování vašeho skriptu.</span><span class="sxs-lookup"><span data-stu-id="555b7-153">Different versions of HDInsight rely on different versions of Ubuntu, which may change how your script behaves.</span></span> <span data-ttu-id="555b7-154">Například vychází HDInsight 3.4 a starší verze Ubuntu, které používají Upstart.</span><span class="sxs-lookup"><span data-stu-id="555b7-154">For example, HDInsight 3.4 and earlier are based on Ubuntu versions that use Upstart.</span></span> <span data-ttu-id="555b7-155">Ubuntu 16.04, který používá Systemd podle verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="555b7-155">Version 3.5 is based on Ubuntu 16.04, which uses Systemd.</span></span> <span data-ttu-id="555b7-156">Systemd a Upstart závisí na jiné příkazy, takže váš skript zasílány toowork s oběma.</span><span class="sxs-lookup"><span data-stu-id="555b7-156">Systemd and Upstart rely on different commands, so your script should be written toowork with both.</span></span>

<span data-ttu-id="555b7-157">Další důležité rozdíl mezi HDInsight 3.4 a 3.5 je, že `JAVA_HOME` nyní body tooJava 8.</span><span class="sxs-lookup"><span data-stu-id="555b7-157">Another important difference between HDInsight 3.4 and 3.5 is that `JAVA_HOME` now points tooJava 8.</span></span>

<span data-ttu-id="555b7-158">Verze hello operačního systému můžete zkontrolovat pomocí `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="555b7-158">You can check hello OS version by using `lsb_release`.</span></span> <span data-ttu-id="555b7-159">Hello následující kód ukazuje, jak toodetermine Pokud hello skriptu běží na Ubuntu 14 nebo 16:</span><span class="sxs-lookup"><span data-stu-id="555b7-159">hello following code demonstrates how toodetermine if hello script is running on Ubuntu 14 or 16:</span></span>

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

<span data-ttu-id="555b7-160">Můžete najít hello úplné skript, který obsahuje tyto fragmenty kódu v https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.</span><span class="sxs-lookup"><span data-stu-id="555b7-160">You can find hello full script that contains these snippets at https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.</span></span>

<span data-ttu-id="555b7-161">Hello verzi Ubuntu, který se používá v prostředí HDInsight, naleznete v části hello [verze komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="555b7-161">For hello version of Ubuntu that is used by HDInsight, see hello [HDInsight component version](hdinsight-component-versioning.md) document.</span></span>

<span data-ttu-id="555b7-162">toounderstand hello rozdíly mezi Systemd a Upstart, najdete v části [Systemd pro uživatele Upstart](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span><span class="sxs-lookup"><span data-stu-id="555b7-162">toounderstand hello differences between Systemd and Upstart, see [Systemd for Upstart users](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span></span>

### <span data-ttu-id="555b7-163"><a name="bPS2"></a>Zadejte, že stabilní odkazy tooscript prostředky</span><span class="sxs-lookup"><span data-stu-id="555b7-163"><a name="bPS2"></a>Provide stable links tooscript resources</span></span>

<span data-ttu-id="555b7-164">Hello skript a související prostředky musí zůstat k dispozici v rámci životnosti hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="555b7-164">hello script and associated resources must remain available throughout hello lifetime of hello cluster.</span></span> <span data-ttu-id="555b7-165">Pokud Přidání nových uzlů clusteru toohello během operace škálování, je nutné tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="555b7-165">These resources are required if new nodes are added toohello cluster during scaling operations.</span></span>

<span data-ttu-id="555b7-166">Hello osvědčeným postupem je toodownload a archivaci vše v účtu Azure Storage na vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="555b7-166">hello best practice is toodownload and archive everything in an Azure Storage account on your subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="555b7-167">účet úložiště Hello musí být hello výchozí účet úložiště pro kontejner veřejné, jen pro čtení nebo hello clusteru na jiný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="555b7-167">hello storage account used must be hello default storage account for hello cluster or a public, read-only container on any other storage account.</span></span>

<span data-ttu-id="555b7-168">Například hello ukázky od společnosti Microsoft jsou uloženy v hello [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="555b7-168">For example, hello samples provided by Microsoft are stored in hello [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) storage account.</span></span> <span data-ttu-id="555b7-169">Toto je veřejný, jen pro čtení kontejner udržovat týmem HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-169">This is a public, read-only container maintained by hello HDInsight team.</span></span>

### <span data-ttu-id="555b7-170"><a name="bPS4"></a>Použití předem zkompilovat prostředky</span><span class="sxs-lookup"><span data-stu-id="555b7-170"><a name="bPS4"></a>Use pre-compiled resources</span></span>

<span data-ttu-id="555b7-171">tooreduce hello trvá toorun hello skriptu, když, vyhněte se operace, které zkompilovat prostředky ze zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="555b7-171">tooreduce hello time it takes toorun hello script, avoid operations that compile resources from source code.</span></span> <span data-ttu-id="555b7-172">Například předem zkompilovat prostředky a uložit je do objektu blob účtu Azure Storage v hello stejném datovém centru jako HDInsight.</span><span class="sxs-lookup"><span data-stu-id="555b7-172">For example, pre-compile resources and store them in an Azure Storage account blob in hello same data center as HDInsight.</span></span>

### <span data-ttu-id="555b7-173"><a name="bPS3"></a>Zajistěte, aby hello clusteru přizpůsobení skriptu idempotent</span><span class="sxs-lookup"><span data-stu-id="555b7-173"><a name="bPS3"></a>Ensure that hello cluster customization script is idempotent</span></span>

<span data-ttu-id="555b7-174">Skripty musí být idempotent.</span><span class="sxs-lookup"><span data-stu-id="555b7-174">Scripts must be idempotent.</span></span> <span data-ttu-id="555b7-175">Pokud hello skript spustí vícekrát, by měla vrátit hello stejné stavu pokaždé, když toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="555b7-175">If hello script runs multiple times, it should return hello cluster toohello same state every time.</span></span>

<span data-ttu-id="555b7-176">Například skript, který upravuje konfigurační soubory by neměly přidat duplicitní položky, pokud byl spuštěn vícekrát.</span><span class="sxs-lookup"><span data-stu-id="555b7-176">For example, a script that modifies configuration files should not add duplicate entries if ran multiple times.</span></span>

### <span data-ttu-id="555b7-177"><a name="bPS5"></a>Ujistěte se, vysokou dostupnost clusteru architektury hello</span><span class="sxs-lookup"><span data-stu-id="555b7-177"><a name="bPS5"></a>Ensure high availability of hello cluster architecture</span></span>

<span data-ttu-id="555b7-178">Linuxové clustery HDInsight poskytují dva head uzly, které jsou aktivní v rámci clusteru hello a spusťte skript akce v obou uzlech.</span><span class="sxs-lookup"><span data-stu-id="555b7-178">Linux-based HDInsight clusters provide two head nodes that are active within hello cluster, and script actions run on both nodes.</span></span> <span data-ttu-id="555b7-179">Pokud součásti hello očekává pouze jeden hlavní uzel, neinstalujte součásti hello v obou uzlech head.</span><span class="sxs-lookup"><span data-stu-id="555b7-179">If hello components you install expect only one head node, do not install hello components on both head nodes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="555b7-180">Služby, které jsou k dispozici jako součást služby HDInsight jsou navrženou toofail přes mezi dvěma uzly head hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="555b7-180">Services provided as part of HDInsight are designed toofail over between hello two head nodes as needed.</span></span> <span data-ttu-id="555b7-181">Tato funkce není rozšířeno toocustom součásti nainstalované prostřednictvím akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="555b7-181">This functionality is not extended toocustom components installed through script actions.</span></span> <span data-ttu-id="555b7-182">Pokud potřebujete vysokou dostupnost pro vlastní součásti, je nutné implementovat vlastní mechanismus převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="555b7-182">If you need high availability for custom components, you must implement your own failover mechanism.</span></span>

### <span data-ttu-id="555b7-183"><a name="bPS6"></a>Konfigurace úložiště objektů Azure Blob toouse hello vlastní komponenty</span><span class="sxs-lookup"><span data-stu-id="555b7-183"><a name="bPS6"></a>Configure hello custom components toouse Azure Blob storage</span></span>

<span data-ttu-id="555b7-184">Součásti, které instalujete na clusteru hello může mít výchozí konfigurace, který používá Hadoop Distributed File System (HDFS) úložiště.</span><span class="sxs-lookup"><span data-stu-id="555b7-184">Components that you install on hello cluster might have a default configuration that uses Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="555b7-185">HDInsight používá jako hello výchozí úložiště Azure Storage nebo Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="555b7-185">HDInsight uses either Azure Storage or Data Lake Store as hello default storage.</span></span> <span data-ttu-id="555b7-186">Zadejte oba systém HDFS kompatibilní souborů, která je uchována data i v případě odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-186">Both provide an HDFS compatible file system that persists data even if hello cluster is deleted.</span></span> <span data-ttu-id="555b7-187">Může být nutné tooconfigure součásti instalujete toouse WASB nebo ADL místo HDFS.</span><span class="sxs-lookup"><span data-stu-id="555b7-187">You may need tooconfigure components you install toouse WASB or ADL instead of HDFS.</span></span>

<span data-ttu-id="555b7-188">Pro většinu operací není nutné systém souborů toospecify hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-188">For most operations, you do not need toospecify hello file system.</span></span> <span data-ttu-id="555b7-189">Například následující hello zkopíruje soubor giraph-examples.jar hello z úložiště toocluster systému hello místního souboru:</span><span class="sxs-lookup"><span data-stu-id="555b7-189">For example, hello following copies hello giraph-examples.jar file from hello local file system toocluster storage:</span></span>

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

<span data-ttu-id="555b7-190">V tomto příkladu hello `hdfs` příkaz transparentně používá úložiště clusteru výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-190">In this example, hello `hdfs` command transparently uses hello default cluster storage.</span></span> <span data-ttu-id="555b7-191">Pro některé operace pravděpodobně bude třeba toospecify hello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="555b7-191">For some operations, you may need toospecify hello URI.</span></span> <span data-ttu-id="555b7-192">Například `adl:///example/jars` pro Data Lake Store nebo `wasb:///example/jars` pro Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="555b7-192">For example, `adl:///example/jars` for Data Lake Store or `wasb:///example/jars` for Azure Storage.</span></span>

### <span data-ttu-id="555b7-193"><a name="bPS7"></a>Zápis tooSTDOUT informace a STDERR</span><span class="sxs-lookup"><span data-stu-id="555b7-193"><a name="bPS7"></a>Write information tooSTDOUT and STDERR</span></span>

<span data-ttu-id="555b7-194">HDInsight zaznamená výstup skriptu, který je zapsaný tooSTDOUT a STDERR.</span><span class="sxs-lookup"><span data-stu-id="555b7-194">HDInsight logs script output that is written tooSTDOUT and STDERR.</span></span> <span data-ttu-id="555b7-195">Můžete zobrazit tyto informace pomocí webovému uživatelskému rozhraní Ambari hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-195">You can view this information using hello Ambari web UI.</span></span>

> [!NOTE]
> <span data-ttu-id="555b7-196">Ambari je dostupné pouze při hello cluster se úspěšně vytvořil.</span><span class="sxs-lookup"><span data-stu-id="555b7-196">Ambari is only available if hello cluster is successfully created.</span></span> <span data-ttu-id="555b7-197">Pokud použijete akci skriptu během vytváření clusteru a vytvoření selže, najdete v části řešení potíží v hello [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) pro další způsoby, jak přístup k informacím o protokolu.</span><span class="sxs-lookup"><span data-stu-id="555b7-197">If you use a script action during cluster creation, and creation fails, see hello troubleshooting section [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) for other ways of accessing logged information.</span></span>

<span data-ttu-id="555b7-198">Většina nástrojů a instalační balíčky již zapsat informace tooSTDOUT a STDERR, ale může být vhodné tooadd další protokolování.</span><span class="sxs-lookup"><span data-stu-id="555b7-198">Most utilities and installation packages already write information tooSTDOUT and STDERR, however you may want tooadd additional logging.</span></span> <span data-ttu-id="555b7-199">toosend text tooSTDOUT, použijte `echo`.</span><span class="sxs-lookup"><span data-stu-id="555b7-199">toosend text tooSTDOUT, use `echo`.</span></span> <span data-ttu-id="555b7-200">Například:</span><span class="sxs-lookup"><span data-stu-id="555b7-200">For example:</span></span>

```bash
echo "Getting ready tooinstall Foo"
```

<span data-ttu-id="555b7-201">Ve výchozím nastavení `echo` zasílá hello tooSTDOUT řetězec.</span><span class="sxs-lookup"><span data-stu-id="555b7-201">By default, `echo` sends hello string tooSTDOUT.</span></span> <span data-ttu-id="555b7-202">toodirect ho tooSTDERR, přidejte `>&2` před `echo`.</span><span class="sxs-lookup"><span data-stu-id="555b7-202">toodirect it tooSTDERR, add `>&2` before `echo`.</span></span> <span data-ttu-id="555b7-203">Například:</span><span class="sxs-lookup"><span data-stu-id="555b7-203">For example:</span></span>

```bash
>&2 echo "An error occurred installing Foo"
```

<span data-ttu-id="555b7-204">To přesměruje informace místo zapsané tooSTDOUT tooSTDERR (2).</span><span class="sxs-lookup"><span data-stu-id="555b7-204">This redirects information written tooSTDOUT tooSTDERR (2) instead.</span></span> <span data-ttu-id="555b7-205">Další informace o přesměrování vstupů/výstupů, najdete v části [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span><span class="sxs-lookup"><span data-stu-id="555b7-205">For more information on IO redirection, see [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span></span>

<span data-ttu-id="555b7-206">Další informace o zobrazení informací zaznamenaných akcí skriptů naleznete v tématu [clusterů HDInsight přizpůsobit pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span><span class="sxs-lookup"><span data-stu-id="555b7-206">For more information on viewing information logged by script actions, see [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span></span>

### <span data-ttu-id="555b7-207"><a name="bps8"></a>Ukládat soubory ve formátu ASCII s LF konce řádků</span><span class="sxs-lookup"><span data-stu-id="555b7-207"><a name="bps8"></a> Save files as ASCII with LF line endings</span></span>

<span data-ttu-id="555b7-208">Skripty bash být uloženy ve formě formátu ASCII, se ukončila příkazem LF řádky.</span><span class="sxs-lookup"><span data-stu-id="555b7-208">Bash scripts should be stored as ASCII format, with lines terminated by LF.</span></span> <span data-ttu-id="555b7-209">Soubory, které jsou uloženy jako UTF-8, nebo použijte Line FEED jako hello řádku ukončuje může selhat s hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="555b7-209">Files that are stored as UTF-8, or use CRLF as hello line ending may fail with hello following error:</span></span>

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <span data-ttu-id="555b7-210"><a name="bps9"></a>Použití opakování logiku toorecover z přechodné chyby</span><span class="sxs-lookup"><span data-stu-id="555b7-210"><a name="bps9"></a> Use retry logic toorecover from transient errors</span></span>

<span data-ttu-id="555b7-211">Při stahování souborů, instalace balíčků pomocí výstižný get nebo jiné akce, které přenášet data přes hello internet, hello akce se nemusí zdařit kvůli tootransient chyby sítě.</span><span class="sxs-lookup"><span data-stu-id="555b7-211">When downloading files, installing packages using apt-get, or other actions that transmit data over hello internet, hello action may fail due tootransient networking errors.</span></span> <span data-ttu-id="555b7-212">Hello vzdálený prostředek, který komunikuje se může být například v procesu hello neúspěšného přes tooa zálohování uzlu.</span><span class="sxs-lookup"><span data-stu-id="555b7-212">For example, hello remote resource you are communicating with may be in hello process of failing over tooa backup node.</span></span>

<span data-ttu-id="555b7-213">toomake vaše odolné tootransient chyby skriptu, můžete implementovat logiku opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="555b7-213">toomake your script resilient tootransient errors, you can implement retry logic.</span></span> <span data-ttu-id="555b7-214">Následující funkce Hello ukazuje, jak logika opakovaných pokusů tooimplement.</span><span class="sxs-lookup"><span data-stu-id="555b7-214">hello following function demonstrates how tooimplement retry logic.</span></span> <span data-ttu-id="555b7-215">Se pokusí hello operaci třikrát než selže.</span><span class="sxs-lookup"><span data-stu-id="555b7-215">It retries hello operation three times before failing.</span></span>

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

<span data-ttu-id="555b7-216">Hello následující příklady ukazují, jak toouse tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="555b7-216">hello following examples demonstrate how toouse this function.</span></span>

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <span data-ttu-id="555b7-217"><a name="helpermethods"></a>Pomocné metody pro vlastní skripty</span><span class="sxs-lookup"><span data-stu-id="555b7-217"><a name="helpermethods"></a>Helper methods for custom scripts</span></span>

<span data-ttu-id="555b7-218">Pomocné metody akcí skriptů jsou nástroje, které můžete použít při zápisu vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="555b7-218">Script action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="555b7-219">Tyto metody jsou součástí[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) skriptu.</span><span class="sxs-lookup"><span data-stu-id="555b7-219">These methods are contained in the[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) script.</span></span> <span data-ttu-id="555b7-220">Použijte následující toodownload hello a použít je jako součást vašeho skriptu:</span><span class="sxs-lookup"><span data-stu-id="555b7-220">Use hello following toodownload and use them as part of your script:</span></span>

```bash
# Import hello helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

<span data-ttu-id="555b7-221">Hello následující pomocné rutiny, které jsou k dispozici pro použití ve vašem skriptu:</span><span class="sxs-lookup"><span data-stu-id="555b7-221">hello following helpers available for use in your script:</span></span>

| <span data-ttu-id="555b7-222">Použití pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="555b7-222">Helper usage</span></span> | <span data-ttu-id="555b7-223">Popis</span><span class="sxs-lookup"><span data-stu-id="555b7-223">Description</span></span> |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |<span data-ttu-id="555b7-224">Stáhne soubor hello zdroj URI toohello zadaná cesta k souboru.</span><span class="sxs-lookup"><span data-stu-id="555b7-224">Downloads a file from hello source URI toohello specified file path.</span></span> <span data-ttu-id="555b7-225">Ve výchozím nastavení je nepřepíše existující soubor.</span><span class="sxs-lookup"><span data-stu-id="555b7-225">By default, it does not overwrite an existing file.</span></span> |
| `untar_file TARFILE DESTDIR` |<span data-ttu-id="555b7-226">Extrahuje vkládání soubor (pomocí `-xf`) toohello cílový adresář.</span><span class="sxs-lookup"><span data-stu-id="555b7-226">Extracts a tar file (using `-xf`) toohello destination directory.</span></span> |
| `test_is_headnode` |<span data-ttu-id="555b7-227">Pokud byla spuštěna hlavního uzlu clusteru, návratový 1; jinak hodnota 0.</span><span class="sxs-lookup"><span data-stu-id="555b7-227">If ran on a cluster head node, return 1; otherwise, 0.</span></span> |
| `test_is_datanode` |<span data-ttu-id="555b7-228">Pokud aktuální uzel hello je uzel dat (pracovník), vrátí 1; jinak hodnota 0.</span><span class="sxs-lookup"><span data-stu-id="555b7-228">If hello current node is a data (worker) node, return a 1; otherwise, 0.</span></span> |
| `test_is_first_datanode` |<span data-ttu-id="555b7-229">Pokud je aktuální uzel hello hello první dat (pracovník) uzlu (s názvem workernode0) vrátí 1; jinak hodnota 0.</span><span class="sxs-lookup"><span data-stu-id="555b7-229">If hello current node is hello first data (worker) node (named workernode0) return a 1; otherwise, 0.</span></span> |
| `get_headnodes` |<span data-ttu-id="555b7-230">Vrátí hello plně kvalifikovaný název domény hello headnodes v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-230">Return hello fully qualified domain name of hello headnodes in hello cluster.</span></span> <span data-ttu-id="555b7-231">Názvy jsou oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="555b7-231">Names are comma delimited.</span></span> <span data-ttu-id="555b7-232">Chyba se vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="555b7-232">An empty string is returned on error.</span></span> |
| `get_primary_headnode` |<span data-ttu-id="555b7-233">Získá hello plně kvalifikovaný název domény primární headnode hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-233">Gets hello fully qualified domain name of hello primary headnode.</span></span> <span data-ttu-id="555b7-234">Chyba se vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="555b7-234">An empty string is returned on error.</span></span> |
| `get_secondary_headnode` |<span data-ttu-id="555b7-235">Získá hello plně kvalifikovaný název domény sekundární headnode hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-235">Gets hello fully qualified domain name of hello secondary headnode.</span></span> <span data-ttu-id="555b7-236">Chyba se vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="555b7-236">An empty string is returned on error.</span></span> |
| `get_primary_headnode_number` |<span data-ttu-id="555b7-237">Získá číselnou příponu hello primární headnode hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-237">Gets hello numeric suffix of hello primary headnode.</span></span> <span data-ttu-id="555b7-238">Chyba se vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="555b7-238">An empty string is returned on error.</span></span> |
| `get_secondary_headnode_number` |<span data-ttu-id="555b7-239">Získá číselnou příponu hello sekundární headnode hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-239">Gets hello numeric suffix of hello secondary headnode.</span></span> <span data-ttu-id="555b7-240">Chyba se vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="555b7-240">An empty string is returned on error.</span></span> |

## <span data-ttu-id="555b7-241"><a name="commonusage"></a>Obecné vzory využití</span><span class="sxs-lookup"><span data-stu-id="555b7-241"><a name="commonusage"></a>Common usage patterns</span></span>

<span data-ttu-id="555b7-242">Tato část obsahuje pokyny k implementaci některé hello běžné využití vzory, které se mohou vyskytnout při zápisu vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="555b7-242">This section provides guidance on implementing some of hello common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="passing-parameters-tooa-script"></a><span data-ttu-id="555b7-243">Předávání parametrů tooa skriptu</span><span class="sxs-lookup"><span data-stu-id="555b7-243">Passing parameters tooa script</span></span>

<span data-ttu-id="555b7-244">V některých případech může váš skript vyžadovat parametry.</span><span class="sxs-lookup"><span data-stu-id="555b7-244">In some cases, your script may require parameters.</span></span> <span data-ttu-id="555b7-245">Například můžete potřebovat heslo správce hello k hello clusteru při použití hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="555b7-245">For example, you may need hello admin password for hello cluster when using hello Ambari REST API.</span></span>

<span data-ttu-id="555b7-246">Parametry předané toohello skriptu se označují jako *poziční parametry*a jsou přiřazeny příliš`$1` prvního parametru hello `$2` pro hello druhé a proto v.</span><span class="sxs-lookup"><span data-stu-id="555b7-246">Parameters passed toohello script are known as *positional parameters*, and are assigned too`$1` for hello first parameter, `$2` for hello second, and so-on.</span></span> <span data-ttu-id="555b7-247">`$0`obsahuje název hello samotný skript hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-247">`$0` contains hello name of hello script itself.</span></span>

<span data-ttu-id="555b7-248">Hodnoty předány jako parametry toohello skriptu by se měla uvádět v jednoduchých uvozovkách (').</span><span class="sxs-lookup"><span data-stu-id="555b7-248">Values passed toohello script as parameters should be enclosed by single quotes (').</span></span> <span data-ttu-id="555b7-249">Tím zajistíte, že hello předaná hodnota je považován za literál.</span><span class="sxs-lookup"><span data-stu-id="555b7-249">Doing so ensures that hello passed value is treated as a literal.</span></span>

### <a name="setting-environment-variables"></a><span data-ttu-id="555b7-250">Nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="555b7-250">Setting environment variables</span></span>

<span data-ttu-id="555b7-251">Nastavení proměnné prostředí provádí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="555b7-251">Setting an environment variable is performed by hello following statement:</span></span>

    VARIABLENAME=value

<span data-ttu-id="555b7-252">Kde NÁZEVPROMĚNNÉ je hello název proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-252">Where VARIABLENAME is hello name of hello variable.</span></span> <span data-ttu-id="555b7-253">použití proměnné, hello tooaccess `$VARIABLENAME`.</span><span class="sxs-lookup"><span data-stu-id="555b7-253">tooaccess hello variable, use `$VARIABLENAME`.</span></span> <span data-ttu-id="555b7-254">Například tooassign hodnotu poskytovanou infrastrukturou poziční parametr jako proměnnou prostředí s názvem heslo, využije hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="555b7-254">For example, tooassign a value provided by a positional parameter as an environment variable named PASSWORD, you would use hello following statement:</span></span>

    PASSWORD=$1

<span data-ttu-id="555b7-255">Pak použít informace o dalších access toohello `$PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="555b7-255">Subsequent access toohello information could then use `$PASSWORD`.</span></span>

<span data-ttu-id="555b7-256">Proměnné prostředí nastavit v rámci skriptu hello existovat pouze v rámci oboru hello hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="555b7-256">Environment variables set within hello script only exist within hello scope of hello script.</span></span> <span data-ttu-id="555b7-257">V některých případech může být nutné tooadd systémové proměnné, které se uchová po dokončení skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-257">In some cases, you may need tooadd system-wide environment variables that will persist after hello script has finished.</span></span> <span data-ttu-id="555b7-258">proměnné prostředí systémové tooadd, přidat proměnnou hello příliš`/etc/environment`.</span><span class="sxs-lookup"><span data-stu-id="555b7-258">tooadd system-wide environment variables, add hello variable too`/etc/environment`.</span></span> <span data-ttu-id="555b7-259">Například hello následující příkaz přidá `HADOOP_CONF_DIR`:</span><span class="sxs-lookup"><span data-stu-id="555b7-259">For example, hello following statement adds `HADOOP_CONF_DIR`:</span></span>

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a><span data-ttu-id="555b7-260">Přístup k toolocations, kde jsou uloženy vlastní skripty hello</span><span class="sxs-lookup"><span data-stu-id="555b7-260">Access toolocations where hello custom scripts are stored</span></span>

<span data-ttu-id="555b7-261">Skripty použité toocustomize cluster potřebuje toobe uložené v jednom z následujících umístění hello:</span><span class="sxs-lookup"><span data-stu-id="555b7-261">Scripts used toocustomize a cluster needs toobe stored in one of hello following locations:</span></span>

* <span data-ttu-id="555b7-262">__Účet úložiště Azure__ který je přidružen hello cluster.</span><span class="sxs-lookup"><span data-stu-id="555b7-262">An __Azure Storage account__ that is associated with hello cluster.</span></span>

* <span data-ttu-id="555b7-263">__Účtu další úložiště__ přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="555b7-263">An __additional storage account__ associated with hello cluster.</span></span>

* <span data-ttu-id="555b7-264">A __veřejně čitelné URI__.</span><span class="sxs-lookup"><span data-stu-id="555b7-264">A __publicly readable URI__.</span></span> <span data-ttu-id="555b7-265">Například adresu URL toodata uložené na OneDrive, Dropbox nebo jiný soubor, který je hostitelem služby.</span><span class="sxs-lookup"><span data-stu-id="555b7-265">For example, a URL toodata stored on OneDrive, Dropbox, or other file hosting service.</span></span>

* <span data-ttu-id="555b7-266">__Účtu Azure Data Lake Store__ který je přidružen hello HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="555b7-266">An __Azure Data Lake Store account__ that is associated with hello HDInsight cluster.</span></span> <span data-ttu-id="555b7-267">Další informace o používání Azure Data Lake Store s HDInsight naleznete v tématu [vytvoření clusteru HDInsight s Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="555b7-267">For more information on using Azure Data Lake Store with HDInsight, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="555b7-268">Hello služby hlavní HDInsight používá tooaccess Data Lake Store, musí mít přístup pro čtení toohello skriptu.</span><span class="sxs-lookup"><span data-stu-id="555b7-268">hello service principal HDInsight uses tooaccess Data Lake Store must have read access toohello script.</span></span>

<span data-ttu-id="555b7-269">Prostředky využívané třídou hello skriptu musí být také veřejně dostupné.</span><span class="sxs-lookup"><span data-stu-id="555b7-269">Resources used by hello script must also be publicly available.</span></span>

<span data-ttu-id="555b7-270">Ukládání souborů hello v účtu úložiště Azure nebo Azure Data Lake Store poskytuje rychlý přístup, jako i v rámci hello síť Azure.</span><span class="sxs-lookup"><span data-stu-id="555b7-270">Storing hello files in an Azure Storage account or Azure Data Lake Store provides fast access, as both within hello Azure network.</span></span>

> [!NOTE]
> <span data-ttu-id="555b7-271">Hello URI formátu, který používá tooreference hello skriptu se liší podle hello služba používá.</span><span class="sxs-lookup"><span data-stu-id="555b7-271">hello URI format used tooreference hello script differs depending on hello service being used.</span></span> <span data-ttu-id="555b7-272">Pro účty úložiště přidružené ke clusteru HDInsight hello, použijte `wasb://` nebo `wasbs://`.</span><span class="sxs-lookup"><span data-stu-id="555b7-272">For storage accounts associated with hello HDInsight cluster, use `wasb://` or `wasbs://`.</span></span> <span data-ttu-id="555b7-273">Veřejně čitelné identifikátory URI, použijte `http://` nebo `https://`.</span><span class="sxs-lookup"><span data-stu-id="555b7-273">For publicly readable URIs, use `http://` or `https://`.</span></span> <span data-ttu-id="555b7-274">Pro Data Lake Store, použijte `adl://`.</span><span class="sxs-lookup"><span data-stu-id="555b7-274">For Data Lake Store, use `adl://`.</span></span>

### <a name="checking-hello-operating-system-version"></a><span data-ttu-id="555b7-275">Kontrola, zda text hello verze operačního systému</span><span class="sxs-lookup"><span data-stu-id="555b7-275">Checking hello operating system version</span></span>

<span data-ttu-id="555b7-276">Různé verze HDInsight spoléhají na určité verze Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="555b7-276">Different versions of HDInsight rely on specific versions of Ubuntu.</span></span> <span data-ttu-id="555b7-277">Mohou existovat rozdíly mezi verzemi operačního systému, které musí vyhledat ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="555b7-277">There may be differences between OS versions that you must check for in your script.</span></span> <span data-ttu-id="555b7-278">Například může být nutné tooinstall binární soubor, který je vázané toohello verze Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="555b7-278">For example, you may need tooinstall a binary that is tied toohello version of Ubuntu.</span></span>

<span data-ttu-id="555b7-279">verze toocheck hello operačního systému, použijte `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="555b7-279">toocheck hello OS version, use `lsb_release`.</span></span> <span data-ttu-id="555b7-280">Například následující skript hello ukazuje, jak tooreference konkrétní vkládání souborů v závislosti na hello verze operačního systému:</span><span class="sxs-lookup"><span data-stu-id="555b7-280">For example, hello following script demonstrates how tooreference a specific tar file depending on hello OS version:</span></span>

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

## <span data-ttu-id="555b7-281"><a name="deployScript"></a>Kontrolní seznam pro nasazení akce skriptu</span><span class="sxs-lookup"><span data-stu-id="555b7-281"><a name="deployScript"></a>Checklist for deploying a script action</span></span>

<span data-ttu-id="555b7-282">Zde jsou hello kroky, které jsme trvalo při přípravě toodeploy tyto skripty:</span><span class="sxs-lookup"><span data-stu-id="555b7-282">Here are hello steps we took when preparing toodeploy these scripts:</span></span>

* <span data-ttu-id="555b7-283">Uveďte hello soubory, které obsahují vlastní skripty hello na místě, které je přístupné uzly clusteru hello během nasazení.</span><span class="sxs-lookup"><span data-stu-id="555b7-283">Put hello files that contain hello custom scripts in a place that is accessible by hello cluster nodes during deployment.</span></span> <span data-ttu-id="555b7-284">Například hello výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="555b7-284">For example, hello default storage for hello cluster.</span></span> <span data-ttu-id="555b7-285">Soubory můžete také uloženy ve veřejně čitelné hostitelských služeb.</span><span class="sxs-lookup"><span data-stu-id="555b7-285">Files can also be stored in publicly readable hosting services.</span></span>
* <span data-ttu-id="555b7-286">Ověřte, zda text hello skriptu impotent.</span><span class="sxs-lookup"><span data-stu-id="555b7-286">Verify that hello script is impotent.</span></span> <span data-ttu-id="555b7-287">To uděláte toobe hello skript spustit několikrát na hello, stejný uzel.</span><span class="sxs-lookup"><span data-stu-id="555b7-287">Doing so allows hello script toobe executed multiple times on hello same node.</span></span>
* <span data-ttu-id="555b7-288">Použití dočasný soubor directory TMP tookeep hello stažené soubory používané hello skripty a pak vyčištění je po mají spouštět skripty.</span><span class="sxs-lookup"><span data-stu-id="555b7-288">Use a temporary file directory /tmp tookeep hello downloaded files used by hello scripts and then clean them up after scripts have executed.</span></span>
* <span data-ttu-id="555b7-289">Pokud se změní nastavení na úrovni operačního systému nebo soubory konfigurace služby Hadoop, můžete chtít toorestart služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="555b7-289">If OS-level settings or Hadoop service configuration files are changed, you may want toorestart HDInsight services.</span></span>

## <span data-ttu-id="555b7-290"><a name="runScriptAction"></a>Jak toorun akce skriptu</span><span class="sxs-lookup"><span data-stu-id="555b7-290"><a name="runScriptAction"></a>How toorun a script action</span></span>

<span data-ttu-id="555b7-291">Můžete použít skript akce toocustomize clusterů HDInsight pomocí hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="555b7-291">You can use script actions toocustomize HDInsight clusters using hello following methods:</span></span>

* <span data-ttu-id="555b7-292">portál Azure</span><span class="sxs-lookup"><span data-stu-id="555b7-292">Azure portal</span></span>
* <span data-ttu-id="555b7-293">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="555b7-293">Azure PowerShell</span></span>
* <span data-ttu-id="555b7-294">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="555b7-294">Azure Resource Manager templates</span></span>
* <span data-ttu-id="555b7-295">Hello HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="555b7-295">hello HDInsight .NET SDK.</span></span>

<span data-ttu-id="555b7-296">Další informace o použití jednotlivých metod najdete v tématu [jak toouse skript akce](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="555b7-296">For more information on using each method, see [How toouse script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="555b7-297"><a name="sampleScripts"></a>Ukázky vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="555b7-297"><a name="sampleScripts"></a>Custom script samples</span></span>

<span data-ttu-id="555b7-298">Společnost Microsoft poskytuje ukázkové skripty tooinstall součásti v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="555b7-298">Microsoft provides sample scripts tooinstall components on an HDInsight cluster.</span></span> <span data-ttu-id="555b7-299">V tématu hello následující odkazy na další příklad akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="555b7-299">See hello following links for more example script actions.</span></span>

* [<span data-ttu-id="555b7-300">Nainstalovat a používat Hue clustery prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="555b7-300">Install and use Hue on HDInsight clusters</span></span>](hdinsight-hadoop-hue-linux.md)
* [<span data-ttu-id="555b7-301">Nainstalovat a používat Solr clustery prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="555b7-301">Install and use Solr on HDInsight clusters</span></span>](hdinsight-hadoop-solr-install-linux.md)
* [<span data-ttu-id="555b7-302">Nainstalovat a používat Giraph clustery prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="555b7-302">Install and use Giraph on HDInsight clusters</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="555b7-303">Nainstalovat nebo upgradovat Mono na clustery HDInsight</span><span class="sxs-lookup"><span data-stu-id="555b7-303">Install or upgrade Mono on HDInsight clusters</span></span>](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a><span data-ttu-id="555b7-304">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="555b7-304">Troubleshooting</span></span>

<span data-ttu-id="555b7-305">Hello následují chyb, které se můžete setkat při používání skripty, které jste si vytvořili:</span><span class="sxs-lookup"><span data-stu-id="555b7-305">hello following are errors you may encounter when using scripts you have developed:</span></span>

<span data-ttu-id="555b7-306">**Chyba**: `$'\r': command not found`.</span><span class="sxs-lookup"><span data-stu-id="555b7-306">**Error**: `$'\r': command not found`.</span></span> <span data-ttu-id="555b7-307">Někdy následuje `syntax error: unexpected end of file`.</span><span class="sxs-lookup"><span data-stu-id="555b7-307">Sometimes followed by `syntax error: unexpected end of file`.</span></span>

<span data-ttu-id="555b7-308">*Příčina*: k této chybě dojde, pokud hello řádků ve skriptu končit Line FEED.</span><span class="sxs-lookup"><span data-stu-id="555b7-308">*Cause*: This error is caused when hello lines in a script end with CRLF.</span></span> <span data-ttu-id="555b7-309">Systémy UNIX očekávat pouze LF jako hello ukončování řádků.</span><span class="sxs-lookup"><span data-stu-id="555b7-309">Unix systems expect only LF as hello line ending.</span></span>

<span data-ttu-id="555b7-310">Tento problém nejčastěji nastane, když je skript hello vytvořené v prostředí Windows jako Line FEED je běžné řádku ukončení pro mnoho textové editory v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="555b7-310">This problem most often occurs when hello script is authored on a Windows environment, as CRLF is a common line ending for many text editors on Windows.</span></span>

<span data-ttu-id="555b7-311">*Řešení*: Pokud je možnost v textovém editoru, vyberte formát operačního systému Unix nebo LF pro ukončování řádků hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-311">*Resolution*: If it is an option in your text editor, select Unix format or LF for hello line ending.</span></span> <span data-ttu-id="555b7-312">Můžete také použít následující příkazy na Unix systému toochange hello Line FEED tooan LF hello:</span><span class="sxs-lookup"><span data-stu-id="555b7-312">You may also use hello following commands on a Unix system toochange hello CRLF tooan LF:</span></span>

> [!NOTE]
> <span data-ttu-id="555b7-313">Hello následující příkazy jsou přibližně ekvivalentem v tom, že by se měl změnit tooLF konců řádku Line FEED hello.</span><span class="sxs-lookup"><span data-stu-id="555b7-313">hello following commands are roughly equivalent in that they should change hello CRLF line endings tooLF.</span></span> <span data-ttu-id="555b7-314">Vyberte jeden podle hello nástrojů dostupných v systému.</span><span class="sxs-lookup"><span data-stu-id="555b7-314">Select one based on hello utilities available on your system.</span></span>

| <span data-ttu-id="555b7-315">Příkaz</span><span class="sxs-lookup"><span data-stu-id="555b7-315">Command</span></span> | <span data-ttu-id="555b7-316">Poznámky</span><span class="sxs-lookup"><span data-stu-id="555b7-316">Notes</span></span> |
| --- | --- |
| `unix2dos -b INFILE` |<span data-ttu-id="555b7-317">původní soubor Hello je zálohovaný s. BAK rozšíření</span><span class="sxs-lookup"><span data-stu-id="555b7-317">hello original file is backed up with a .BAK extension</span></span> |
| `tr -d '\r' < INFILE > OUTFILE` |<span data-ttu-id="555b7-318">VÝSTUPNÍ_SOUBOR obsahuje verzi s pouze LF zakončení</span><span class="sxs-lookup"><span data-stu-id="555b7-318">OUTFILE contains a version with only LF endings</span></span> |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | <span data-ttu-id="555b7-319">Upraví soubor hello přímo</span><span class="sxs-lookup"><span data-stu-id="555b7-319">Modifies hello file directly</span></span> |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |<span data-ttu-id="555b7-320">VÝSTUPNÍ_SOUBOR obsahuje verzi s pouze LF zakončení.</span><span class="sxs-lookup"><span data-stu-id="555b7-320">OUTFILE contains a version with only LF endings.</span></span> |

<span data-ttu-id="555b7-321">**Chyba**: `line 1: #!/usr/bin/env: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="555b7-321">**Error**: `line 1: #!/usr/bin/env: No such file or directory`.</span></span>

<span data-ttu-id="555b7-322">*Příčina*: Tato chyba nastane, když hello skriptu se uložil jako UTF-8 s značky pořadí bajtů (BOM).</span><span class="sxs-lookup"><span data-stu-id="555b7-322">*Cause*: This error occurs when hello script was saved as UTF-8 with a Byte Order Mark (BOM).</span></span>

<span data-ttu-id="555b7-323">*Řešení*: uložit hello soubor ve formátu ASCII nebo jako UTF-8 bez Kusovník.</span><span class="sxs-lookup"><span data-stu-id="555b7-323">*Resolution*: Save hello file either as ASCII, or as UTF-8 without a BOM.</span></span> <span data-ttu-id="555b7-324">Můžete také použít následující příkaz na Linux nebo Unix systému toocreate soubor bez hello BOM hello:</span><span class="sxs-lookup"><span data-stu-id="555b7-324">You may also use hello following command on a Linux or Unix system toocreate a file without hello BOM:</span></span>

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

<span data-ttu-id="555b7-325">Nahraďte `INFILE` souborem hello obsahující hello BOM.</span><span class="sxs-lookup"><span data-stu-id="555b7-325">Replace `INFILE` with hello file containing hello BOM.</span></span> <span data-ttu-id="555b7-326">`OUTFILE`musí být nový název souboru, který obsahuje skript hello bez hello BOM.</span><span class="sxs-lookup"><span data-stu-id="555b7-326">`OUTFILE` should be a new file name, which contains hello script without hello BOM.</span></span>

## <span data-ttu-id="555b7-327"><a name="seeAlso"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="555b7-327"><a name="seeAlso"></a>Next steps</span></span>

* <span data-ttu-id="555b7-328">Zjistěte, jak příliš[clusterů HDInsight přizpůsobit pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="555b7-328">Learn how too[Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
* <span data-ttu-id="555b7-329">Použití hello [referenční informace sady SDK rozhraní .NET HDInsight](https://msdn.microsoft.com/library/mt271028.aspx) toolearn Další informace o vytvoření aplikace .NET, které spravují HDInsight</span><span class="sxs-lookup"><span data-stu-id="555b7-329">Use hello [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx) toolearn more about creating .NET applications that manage HDInsight</span></span>
* <span data-ttu-id="555b7-330">Použití hello [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) toolearn jak toouse REST tooperform akce správy v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="555b7-330">Use hello [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) toolearn how toouse REST tooperform management actions on HDInsight clusters.</span></span>
