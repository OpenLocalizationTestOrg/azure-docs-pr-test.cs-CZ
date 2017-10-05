---
title: "Upgrade clusteru HDInsight na novější verzi-Azure | Microsoft Docs"
description: "Zjistěte, jak ke clusteru HDInsight se Upgrade na novější verzi."
services: hdinsight
documentationcenter: 
author: bhanupr
manager: asadk
editor: bhanupr
ms.assetid: 60eb573c-e639-4815-9fc6-ea8b106d8dbc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2017
ms.author: bhanupr
ms.openlocfilehash: fa2e37bd922690322ccc3d8f68128180d013b701
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a><span data-ttu-id="e79a2-103">Upgrade clusteru HDInsight na novější verzi</span><span class="sxs-lookup"><span data-stu-id="e79a2-103">Upgrade HDInsight cluster to a newer version</span></span>
<span data-ttu-id="e79a2-104">Využívat výhody nejnovějších funkcí HDInsight, doporučujeme, abyste clustery HDInsight upgradovali na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="e79a2-104">To take advantage of the latest HDInsight features, we recommend that HDInsight clusters be upgraded to latest version.</span></span> <span data-ttu-id="e79a2-105">Postupujte podle níže pokyny k upgradu vaší HDInsight clusteru verze.</span><span class="sxs-lookup"><span data-stu-id="e79a2-105">Follow the below guidelines to upgrade your HDInsight cluster versions.</span></span>

> [!NOTE]
> <span data-ttu-id="e79a2-106">Clustery HDInsight verze 3.2 nebo 3.3 se blíží datum vyřazení.</span><span class="sxs-lookup"><span data-stu-id="e79a2-106">HDInsight clusters version 3.2 and 3.3 are nearing retirement date.</span></span> <span data-ttu-id="e79a2-107">Informace o podporovanou verzi HDInsight, naleznete v části [HDInsight verze součástí](hdinsight-component-versioning.md#supported-hdinsight-versions).</span><span class="sxs-lookup"><span data-stu-id="e79a2-107">For information on supported version of HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md#supported-hdinsight-versions).</span></span>
>
>

## <a name="upgrade-tasks"></a><span data-ttu-id="e79a2-108">Úlohy upgradu</span><span class="sxs-lookup"><span data-stu-id="e79a2-108">Upgrade tasks</span></span>
<span data-ttu-id="e79a2-109">Pracovní postup pro upgrade clusteru HDInsight je následující.</span><span class="sxs-lookup"><span data-stu-id="e79a2-109">The workflow to upgrade HDInsight Cluster is as follows.</span></span>

![Diagram pracovní postup upgradu](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. <span data-ttu-id="e79a2-111">Přečtěte si každá část tohoto dokumentu pochopit změny, které mohou být vyžadovány při upgradu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e79a2-111">Read each section of this document to understand changes that may be required when upgrading your HDInsight cluster.</span></span>
2. <span data-ttu-id="e79a2-112">Vytvořte cluster s podporou prostředí testovací a kvalita záruky.</span><span class="sxs-lookup"><span data-stu-id="e79a2-112">Create a cluster as a test/quality assurance environment.</span></span> <span data-ttu-id="e79a2-113">Další informace týkající se vytvoření clusteru najdete v tématu [Naučte se vytvářet clustery HDInsight se systémem Linux](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="e79a2-113">For more information on creating a cluster, see [Learn how to create Linux-based HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
3. <span data-ttu-id="e79a2-114">Zkopírujte existující úlohy, zdroje dat a jímky do nového prostředí.</span><span class="sxs-lookup"><span data-stu-id="e79a2-114">Copy existing jobs, data sources, and sinks to the new environment.</span></span> <span data-ttu-id="e79a2-115">V tématu [kopírování dat do testovacího prostředí](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e79a2-115">See [Copy Data To Test Environment](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) for more details.</span></span>
4. <span data-ttu-id="e79a2-116">Provádění testů pro ověření a ujistěte se, že vaše úlohy fungují v novém clusteru podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="e79a2-116">Perform validation testing to make sure that your jobs work as expected on the new cluster.</span></span>


<span data-ttu-id="e79a2-117">Jakmile si ověříte, že vše funguje podle očekávání, naplánujte dobu odstávky migrace.</span><span class="sxs-lookup"><span data-stu-id="e79a2-117">Once you have verified that everything works as expected, schedule downtime for the migration.</span></span> <span data-ttu-id="e79a2-118">Během této výpadky proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="e79a2-118">During this downtime, do the following actions:</span></span>

1.  <span data-ttu-id="e79a2-119">Zálohujte všechna přechodný data uložená místně na uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="e79a2-119">Back up any transient data stored locally on the cluster nodes.</span></span> <span data-ttu-id="e79a2-120">Například pokud máte data uložená přímo na hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="e79a2-120">For example, if you have data stored directly on a head node.</span></span>
2.  <span data-ttu-id="e79a2-121">Odstraňte existující cluster.</span><span class="sxs-lookup"><span data-stu-id="e79a2-121">Delete the existing cluster.</span></span>
3.  <span data-ttu-id="e79a2-122">Vytvoření clusteru ve stejné podsíti virtuální sítě s nejnovější (nebo podporovaných) verze HDI pomocí stejné výchozí úložiště dat, které používá předchozích clusterů.</span><span class="sxs-lookup"><span data-stu-id="e79a2-122">Create a cluster in the same VNET subnet with latest (or supported) HDI version using the same default data store that the previous cluster used.</span></span> <span data-ttu-id="e79a2-123">To umožňuje nového clusteru pokračovat v práci s existující provozní data.</span><span class="sxs-lookup"><span data-stu-id="e79a2-123">This allows the new cluster to continue working against your existing production data.</span></span>
4.  <span data-ttu-id="e79a2-124">Importujte přechodný data, která jste zálohovali.</span><span class="sxs-lookup"><span data-stu-id="e79a2-124">Import any transient data you backed up.</span></span>
5.  <span data-ttu-id="e79a2-125">Spuštění úlohy nebo pokračovat ve zpracovávání pomocí nového clusteru.</span><span class="sxs-lookup"><span data-stu-id="e79a2-125">Start jobs/continue processing using the new cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e79a2-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e79a2-126">Next Steps</span></span>
* [<span data-ttu-id="e79a2-127">Naučte se vytvářet clustery HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="e79a2-127">Learn how to create Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="e79a2-128">Připojení k HDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="e79a2-128">Connect to HDInsight using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)
* [<span data-ttu-id="e79a2-129">Spravovat cluster se systémem Linux pomocí nástroje Ambari</span><span class="sxs-lookup"><span data-stu-id="e79a2-129">Manage a Linux-based cluster using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)

