---
title: "Konfigurace plánu pro clustery HDInsight se systémem Linux - Azure opravy operačního systému | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat plán pro clustery HDInsight se systémem Linux opravy operačního systému."
services: hdinsight
documentationcenter: 
author: bprakash
manager: asadk
editor: bprakash
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: bhanupr
ms.openlocfilehash: af3c5a19ae8e2e606e4b0506f9f6dddb41192e40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="os-patching-for-hdinsight"></a><span data-ttu-id="d9943-103">Opravy chyb pro HDInsight operačního systému</span><span class="sxs-lookup"><span data-stu-id="d9943-103">OS patching for HDInsight</span></span> 
<span data-ttu-id="d9943-104">Služba se spravuje Hadoop HDInsight má na starosti opravy operačního systému základní virtuální počítače, které jsou používané clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d9943-104">As a managed Hadoop service, HDInsight takes care of patching the OS of the underlying VMs used by HDInsight clusters.</span></span> <span data-ttu-id="d9943-105">Od 1. srpna 2016 jsme změnili oprav zásady hostovaného operačního systému pro clustery HDInsight se systémem Linux (verze 3.4 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="d9943-105">As of August 1, 2016, we have changed the guest OS patching policy for Linux-based HDInsight clusters (version 3.4 or greater).</span></span> <span data-ttu-id="d9943-106">Cílem nové zásady je výrazně snížit počet restartování z důvodu opravy.</span><span class="sxs-lookup"><span data-stu-id="d9943-106">The goal of the new policy is to significantly reduce the number of reboots due to patching.</span></span> <span data-ttu-id="d9943-107">Nové zásady budou nadále opravy virtuálních počítačů (VM) v clusterech Linux každé pondělí a čtvrtek začínající na 12: 00 UTC postupný způsobem mezi uzly v jakémkoliv daného clusteru.</span><span class="sxs-lookup"><span data-stu-id="d9943-107">The new policy will continue to patch virtual machines (VMs) on Linux clusters every Monday or Thursday starting at 12AM UTC in a staggered fashion across nodes in any given cluster.</span></span> <span data-ttu-id="d9943-108">Všechny daného virtuálního počítače se však pouze restartuje maximálně jednou za 30 dní z důvodu opravy operačního systému hosta.</span><span class="sxs-lookup"><span data-stu-id="d9943-108">However, any given VM will only reboot at most once every 30 days due to guest OS patching.</span></span> <span data-ttu-id="d9943-109">Kromě toho první restartování pro nově vytvořený cluster neprovede dřív než 30 dní od data vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="d9943-109">In addition, the first reboot for a newly created cluster will not happen sooner than 30 days from the cluster creation date.</span></span> <span data-ttu-id="d9943-110">Opravy bude platit, jakmile jsou virtuální počítače restartovat.</span><span class="sxs-lookup"><span data-stu-id="d9943-110">Patches will be effective once the VMs are rebooted.</span></span>

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a><span data-ttu-id="d9943-111">Postup konfigurace operačního systému opravy plán pro clustery HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="d9943-111">How to configure the OS patching schedule for Linux-based HDInsight clusters</span></span>
<span data-ttu-id="d9943-112">Virtuální počítače v clusteru služby HDInsight muset občas restartovat, aby se opravy důležité zabezpečení můžete nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="d9943-112">The virtual machines in an HDInsight cluster need to be rebooted occasionally so that important security patches can be installed.</span></span> <span data-ttu-id="d9943-113">Od 1. srpna 2016 se restartují nových clusterů HDInsight se systémem Linux (verze 3.4 nebo novější,), pomocí následujícího plánu:</span><span class="sxs-lookup"><span data-stu-id="d9943-113">As of August 1, 2016, new Linux-based HDInsight clusters (version 3.4 or greater,) are rebooted using the following schedule:</span></span>

1. <span data-ttu-id="d9943-114">Virtuální počítač v clusteru můžete pouze po restartování počítače opravy maximálně jednou v období 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="d9943-114">A virtual machine in the cluster can only reboot for patches at most, once within a 30-day period.</span></span>
2. <span data-ttu-id="d9943-115">Počínaje 12: 00 UTC dojde k restartování.</span><span class="sxs-lookup"><span data-stu-id="d9943-115">The reboot occurs starting at 12AM UTC.</span></span>
3. <span data-ttu-id="d9943-116">Proces restartování rozložena mezi virtuální počítače v clusteru, takže clusteru je stále k dispozici během procesu restartování.</span><span class="sxs-lookup"><span data-stu-id="d9943-116">The reboot process is staggered across virtual machines in the cluster, so the cluster is still available during the reboot process.</span></span>
4. <span data-ttu-id="d9943-117">První restartování pro nově vytvořený cluster neprovede dřív než 30 dní od data vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="d9943-117">The first reboot for a newly created cluster will not happen sooner than 30 days after the cluster creation date.</span></span>

<span data-ttu-id="d9943-118">Pomocí akce skriptu popsaného v tomto článku, můžete upravit OS opravy plán následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d9943-118">Using the script action described in this article, you can modify the OS patching schedule as follows:</span></span>
1. <span data-ttu-id="d9943-119">Povolit nebo zakázat automatické restartování počítače</span><span class="sxs-lookup"><span data-stu-id="d9943-119">Enable or disable automatic reboots</span></span>
2. <span data-ttu-id="d9943-120">Sada četnost restartuje (ve dnech mezi jednotlivými restartováními)</span><span class="sxs-lookup"><span data-stu-id="d9943-120">Set the frequency of reboots (days between reboots)</span></span>
3. <span data-ttu-id="d9943-121">Nastavit den v týdnu, kdy dojde k restartu</span><span class="sxs-lookup"><span data-stu-id="d9943-121">Set the day of the week when a reboot occurs</span></span>

> [!NOTE]
> <span data-ttu-id="d9943-122">Tato akce skriptu budou fungovat jenom s clustery HDInsight se systémem Linux, které jsou vytvořené po 1. srpna 2016.</span><span class="sxs-lookup"><span data-stu-id="d9943-122">This script action will only work with Linux-based HDInsight clusters created after August 1st, 2016.</span></span> <span data-ttu-id="d9943-123">Opravy bude platit pouze v případě, že jsou virtuální počítače restartovat.</span><span class="sxs-lookup"><span data-stu-id="d9943-123">Patches will be effective only when VMs are rebooted.</span></span> 
>

## <a name="how-to-use-the-script"></a><span data-ttu-id="d9943-124">Jak používat skript</span><span class="sxs-lookup"><span data-stu-id="d9943-124">How to use the script</span></span> 

<span data-ttu-id="d9943-125">Při použití tohoto skriptu vyžaduje následující informace:</span><span class="sxs-lookup"><span data-stu-id="d9943-125">When using this script requires the following information:</span></span>
1. <span data-ttu-id="d9943-126">Umístění skriptu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.</span><span class="sxs-lookup"><span data-stu-id="d9943-126">The script location: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.</span></span>
    <span data-ttu-id="d9943-127">HDInsight používá tento identifikátor URI k vyhledání a spuštění skriptu na všechny virtuální počítače v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d9943-127">HDInsight uses this URI to find and run the script on all the virtual machines in the cluster.</span></span>
  
2. <span data-ttu-id="d9943-128">Typy uzlů clusteru, které skript se použije pro: headnode, workernode, zookeeper.</span><span class="sxs-lookup"><span data-stu-id="d9943-128">The cluster node types that the script is applied to: headnode, workernode, zookeeper.</span></span> <span data-ttu-id="d9943-129">Tento skript je nutné použít na všechny typy uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d9943-129">This script must be applied to all node types in the cluster.</span></span> <span data-ttu-id="d9943-130">Pokud není použita pro typ uzlu, virtuálních počítačů pro daný typ uzlu bude nadále používat předchozí oprav plán.</span><span class="sxs-lookup"><span data-stu-id="d9943-130">If it is not applied to a node type, then the virtual machines for that node type will continue to use the previous patching schedule.</span></span>


3.  <span data-ttu-id="d9943-131">Parametr: Tento skript přijímá tři číselné parametry:</span><span class="sxs-lookup"><span data-stu-id="d9943-131">Parameter: This script accepts three numeric parameters:</span></span>

    | <span data-ttu-id="d9943-132">Parametr</span><span class="sxs-lookup"><span data-stu-id="d9943-132">Parameter</span></span> | <span data-ttu-id="d9943-133">Definice</span><span class="sxs-lookup"><span data-stu-id="d9943-133">Definition</span></span> |
    | --- | --- |
    | <span data-ttu-id="d9943-134">Povolit nebo zakázat automatické restartování počítače</span><span class="sxs-lookup"><span data-stu-id="d9943-134">Enable/disable automatic reboots</span></span> |<span data-ttu-id="d9943-135">0 nebo 1.</span><span class="sxs-lookup"><span data-stu-id="d9943-135">0 or 1.</span></span> <span data-ttu-id="d9943-136">Hodnota 0 zakáže automatické restartování počítače během 1 povolí automatické restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="d9943-136">A value of 0 disables automatic reboots while 1 enables automatic reboots.</span></span> |
    | <span data-ttu-id="d9943-137">frekvence</span><span class="sxs-lookup"><span data-stu-id="d9943-137">Frequency</span></span> |<span data-ttu-id="d9943-138">7 až 90 (včetně).</span><span class="sxs-lookup"><span data-stu-id="d9943-138">7 to 90 (inclusive).</span></span> <span data-ttu-id="d9943-139">Počet dní, které se má čekat před restartováním virtuálních počítačů pro opravy, které vyžadují restart počítače.</span><span class="sxs-lookup"><span data-stu-id="d9943-139">The number of days to wait before rebooting the virtual machines for patches that require a reboot.</span></span> |
    | <span data-ttu-id="d9943-140">Den v týdnu</span><span class="sxs-lookup"><span data-stu-id="d9943-140">Day of week</span></span> |<span data-ttu-id="d9943-141">1 až 7 (včetně).</span><span class="sxs-lookup"><span data-stu-id="d9943-141">1 to 7 (inclusive).</span></span> <span data-ttu-id="d9943-142">Hodnota 1 znamená restartování provedeno v pondělí a 7 označuje Sunday.For příklad, pomocí parametrů 1 60 2 výsledky v automaticky restartuje každých 60 dnů (maximálně) úterý.</span><span class="sxs-lookup"><span data-stu-id="d9943-142">A value of 1 indicates the reboot should occur on a Monday, and 7 indicates a Sunday.For example, using parameters of 1 60 2 results in automatic reboots every 60 days (at most) on Tuesday.</span></span> |
    | <span data-ttu-id="d9943-143">Trvalost</span><span class="sxs-lookup"><span data-stu-id="d9943-143">Persistence</span></span> |<span data-ttu-id="d9943-144">Při použití akce skriptu do existujícího clusteru, můžete skript označit jako trvalý.</span><span class="sxs-lookup"><span data-stu-id="d9943-144">When applying a script action to an existing cluster, you can mark the script as persisted.</span></span> <span data-ttu-id="d9943-145">Trvalý skripty se použijí při přidání nového workernodes ke clusteru prostřednictvím operace škálování.</span><span class="sxs-lookup"><span data-stu-id="d9943-145">Persisted scripts are applied when new workernodes are added to the cluster through scaling operations.</span></span> |

> [!NOTE]
> <span data-ttu-id="d9943-146">Tento skript je třeba označit jako trvalý při použití existujícího clusteru.</span><span class="sxs-lookup"><span data-stu-id="d9943-146">You must mark this script as persisted when applying to an existing cluster.</span></span> <span data-ttu-id="d9943-147">Všechny nové uzly, které jsou vytvořené pomocí operace škálování, jinak hodnota použije výchozí opravy plán.</span><span class="sxs-lookup"><span data-stu-id="d9943-147">Otherwise, any new nodes created through scaling      operations will use the default patching schedule.</span></span>
<span data-ttu-id="d9943-148">Pokud použijete skript jako součást procesu vytváření clusteru, je automaticky trvalá.</span><span class="sxs-lookup"><span data-stu-id="d9943-148">If you apply the script as part of the cluster creation process, it is persisted automatically.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="d9943-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9943-149">Next steps</span></span>

<span data-ttu-id="d9943-150">Konkrétní kroky pomocí akce skriptu, naleznete v následujících částech [HDInsight se systémem Linuz přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md):</span><span class="sxs-lookup"><span data-stu-id="d9943-150">For specific steps on using the script action, see the following sections in the [Customize Linuz-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md):</span></span>

* [<span data-ttu-id="d9943-151">Použití akce skriptu při vytváření clusteru</span><span class="sxs-lookup"><span data-stu-id="d9943-151">Use a script action during cluster creation</span></span>](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [<span data-ttu-id="d9943-152">Použít akci skriptu do clusteru s podporou spuštěná</span><span class="sxs-lookup"><span data-stu-id="d9943-152">Apply a script action to a running cluster</span></span>](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
