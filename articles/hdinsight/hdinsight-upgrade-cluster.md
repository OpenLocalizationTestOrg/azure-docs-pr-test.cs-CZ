---
title: "novější verze aaaUpgrade HDInsight clusteru tooa-Azure | Microsoft Docs"
description: "Zjistěte, jak tooUpgrade HDInsight clusteru tooa novější verze."
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
ms.openlocfilehash: 5fff3c9bc88dfbcbc1ccb0188accdfbbec3a62f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-hdinsight-cluster-tooa-newer-version"></a><span data-ttu-id="a1009-103">Upgrade clusteru HDInsight tooa novější verze</span><span class="sxs-lookup"><span data-stu-id="a1009-103">Upgrade HDInsight cluster tooa newer version</span></span>
<span data-ttu-id="a1009-104">tootake výhod hello nejnovější funkce HDInsight, doporučujeme, aby clustery HDInsight se toolatest upgradovaná verze.</span><span class="sxs-lookup"><span data-stu-id="a1009-104">tootake advantage of hello latest HDInsight features, we recommend that HDInsight clusters be upgraded toolatest version.</span></span> <span data-ttu-id="a1009-105">Postupujte podle hello níže pokyny tooupgrade vaší verze clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a1009-105">Follow hello below guidelines tooupgrade your HDInsight cluster versions.</span></span>

> [!NOTE]
> <span data-ttu-id="a1009-106">Clustery HDInsight verze 3.2 nebo 3.3 se blíží datum vyřazení.</span><span class="sxs-lookup"><span data-stu-id="a1009-106">HDInsight clusters version 3.2 and 3.3 are nearing retirement date.</span></span> <span data-ttu-id="a1009-107">Informace o podporovanou verzi HDInsight, naleznete v části [HDInsight verze součástí](hdinsight-component-versioning.md#supported-hdinsight-versions).</span><span class="sxs-lookup"><span data-stu-id="a1009-107">For information on supported version of HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md#supported-hdinsight-versions).</span></span>
>
>

## <a name="upgrade-tasks"></a><span data-ttu-id="a1009-108">Úlohy upgradu</span><span class="sxs-lookup"><span data-stu-id="a1009-108">Upgrade tasks</span></span>
<span data-ttu-id="a1009-109">pracovní postup tooupgrade Hello clusteru HDInsight je následující.</span><span class="sxs-lookup"><span data-stu-id="a1009-109">hello workflow tooupgrade HDInsight Cluster is as follows.</span></span>

![Diagram pracovní postup upgradu](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. <span data-ttu-id="a1009-111">Číst každá část tohoto dokumentu toounderstand změny, které mohou být vyžadovány při upgradu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a1009-111">Read each section of this document toounderstand changes that may be required when upgrading your HDInsight cluster.</span></span>
2. <span data-ttu-id="a1009-112">Vytvořte cluster s podporou prostředí testovací a kvalita záruky.</span><span class="sxs-lookup"><span data-stu-id="a1009-112">Create a cluster as a test/quality assurance environment.</span></span> <span data-ttu-id="a1009-113">Další informace týkající se vytvoření clusteru najdete v tématu [zjistěte, jak toocreate HDInsight se systémem Linux clusterů](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="a1009-113">For more information on creating a cluster, see [Learn how toocreate Linux-based HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
3. <span data-ttu-id="a1009-114">Kopie existující úlohy, zdroje dat a jímky toohello nové prostředí.</span><span class="sxs-lookup"><span data-stu-id="a1009-114">Copy existing jobs, data sources, and sinks toohello new environment.</span></span> <span data-ttu-id="a1009-115">V tématu [kopírovat Data tooTest prostředí](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a1009-115">See [Copy Data tooTest Environment](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) for more details.</span></span>
4. <span data-ttu-id="a1009-116">Provedení ověření testování toomake jistotu, že vaše úlohy fungují podle očekávání na novém clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a1009-116">Perform validation testing toomake sure that your jobs work as expected on hello new cluster.</span></span>


<span data-ttu-id="a1009-117">Jakmile si ověříte, že vše funguje podle očekávání, naplánujte dobu odstávky hello migrace.</span><span class="sxs-lookup"><span data-stu-id="a1009-117">Once you have verified that everything works as expected, schedule downtime for hello migration.</span></span> <span data-ttu-id="a1009-118">Během této výpadků hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="a1009-118">During this downtime, do hello following actions:</span></span>

1.  <span data-ttu-id="a1009-119">Zálohujte všechna přechodný data uložená místně na uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="a1009-119">Back up any transient data stored locally on hello cluster nodes.</span></span> <span data-ttu-id="a1009-120">Například pokud máte data uložená přímo na hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="a1009-120">For example, if you have data stored directly on a head node.</span></span>
2.  <span data-ttu-id="a1009-121">Odstraňte existující cluster hello.</span><span class="sxs-lookup"><span data-stu-id="a1009-121">Delete hello existing cluster.</span></span>
3.  <span data-ttu-id="a1009-122">Vytvoření clusteru s podporou v hello stejné sítě VNET podsíť s nejnovější (nebo podporovaných) HDI verze pomocí hello stejné výchozí datové úložiště, které hello předchozí cluster používá.</span><span class="sxs-lookup"><span data-stu-id="a1009-122">Create a cluster in hello same VNET subnet with latest (or supported) HDI version using hello same default data store that hello previous cluster used.</span></span> <span data-ttu-id="a1009-123">To umožňuje hello nového clusteru toocontinue pracovat se vaše stávající provozními daty.</span><span class="sxs-lookup"><span data-stu-id="a1009-123">This allows hello new cluster toocontinue working against your existing production data.</span></span>
4.  <span data-ttu-id="a1009-124">Importujte přechodný data, která jste zálohovali.</span><span class="sxs-lookup"><span data-stu-id="a1009-124">Import any transient data you backed up.</span></span>
5.  <span data-ttu-id="a1009-125">Spuštění úlohy nebo pokračovat ve zpracovávání pomocí hello nového clusteru.</span><span class="sxs-lookup"><span data-stu-id="a1009-125">Start jobs/continue processing using hello new cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1009-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a1009-126">Next Steps</span></span>
* [<span data-ttu-id="a1009-127">Zjistěte, jak toocreate HDInsight se systémem Linux clusterů</span><span class="sxs-lookup"><span data-stu-id="a1009-127">Learn how toocreate Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="a1009-128">Připojit tooHDInsight pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="a1009-128">Connect tooHDInsight using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)
* [<span data-ttu-id="a1009-129">Spravovat cluster se systémem Linux pomocí nástroje Ambari</span><span class="sxs-lookup"><span data-stu-id="a1009-129">Manage a Linux-based cluster using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)

