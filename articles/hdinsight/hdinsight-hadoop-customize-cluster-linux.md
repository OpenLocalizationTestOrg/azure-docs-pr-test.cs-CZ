---
title: "Přizpůsobení clusterů HDInsight pomocí akcí skriptů - Azure | Microsoft Docs"
description: "Přidáte vlastní komponenty ke clusterům HDInsight se systémem Linux pomocí akcí skriptů. Akce skriptů jsou skripty Bash, které lze použít k přizpůsobení konfigurace clusteru nebo přidání další služby a nástroje, jako je Hue, Solr nebo R."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48e85f53-87c1-474f-b767-ca772238cc13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: 0c5d00b6cb9f68a1a0e474f81c969eb1b5654c67
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-action"></a><span data-ttu-id="a15b5-104">Přizpůsobení clusterů HDInsight se systémem Linux pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="a15b5-104">Customize Linux-based HDInsight clusters using Script Action</span></span>

<span data-ttu-id="a15b5-105">HDInsight nabízí možnost konfigurace názvem **akce skriptu** vlastní skripty, které přizpůsobují clusteru, který spustí.</span><span class="sxs-lookup"><span data-stu-id="a15b5-105">HDInsight provides a configuration option called **Script Action** that invokes custom scripts that customize the cluster.</span></span> <span data-ttu-id="a15b5-106">Tyto skripty se používají k instalaci dalších součástí a změně nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a15b5-106">These scripts are used to install additional components and change configuration settings.</span></span> <span data-ttu-id="a15b5-107">Akce skriptu lze během nebo po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-107">Script actions can be used during or after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a15b5-108">Možnost používat akcí skriptů v již spuštěného clusteru je pouze k dispozici pro clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="a15b5-108">The ability to use script actions on an already running cluster is only available for Linux-based HDInsight clusters.</span></span>
>
> <span data-ttu-id="a15b5-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="a15b5-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a15b5-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a15b5-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="a15b5-111">Akce skriptu lze také publikovat na webu Azure Marketplace jako aplikace HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-111">Script actions can also be published to the Azure Marketplace as an HDInsight application.</span></span> <span data-ttu-id="a15b5-112">Některé příklady v tomto dokumentu ukazují, jak můžete instalovat aplikace HDInsight pomocí skriptu akce příkazů z prostředí PowerShell a .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="a15b5-112">Some of the examples in this document show how you can install an HDInsight application using script action commands from PowerShell and the .NET SDK.</span></span> <span data-ttu-id="a15b5-113">Další informace o aplikace HDInsight naleznete v tématu [HDInsight publikování aplikace do Azure Marketplace](hdinsight-apps-publish-applications.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-113">For more information on HDInsight applications, see [Publish HDInsight applications into the Azure Marketplace](hdinsight-apps-publish-applications.md).</span></span>

## <a name="permissions"></a><span data-ttu-id="a15b5-114">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="a15b5-114">Permissions</span></span>

<span data-ttu-id="a15b5-115">Pokud používáte cluster HDInsight připojený k doméně, jsou k dispozici dva Ambari oprávnění, které jsou potřebné k pomocí akcí skriptů v clusteru:</span><span class="sxs-lookup"><span data-stu-id="a15b5-115">If you are using a domain-joined HDInsight cluster, there are two Ambari permissions that are required when using script actions with the cluster:</span></span>

* <span data-ttu-id="a15b5-116">**AMBARI. Spustit\_vlastní\_příkaz**: role správce Ambari toto oprávnění má ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a15b5-116">**AMBARI.RUN\_CUSTOM\_COMMAND**: The Ambari Administrator role has this permission by default.</span></span>
* <span data-ttu-id="a15b5-117">**CLUSTER. Spustit\_vlastní\_příkaz**: obě Správce clusteru HDInsight a Ambari správce toto oprávnění mají ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a15b5-117">**CLUSTER.RUN\_CUSTOM\_COMMAND**: Both the HDInsight Cluster Administrator and Ambari Administrator have this permission by default.</span></span>

<span data-ttu-id="a15b5-118">Další informace o práci s oprávněními v doméně prostředí HDInsight najdete v tématu [Správa clusterů HDInsight připojený k doméně](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-118">For more information on working with permissions with domain-joined HDInsight, see [Manage domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>

## <a name="access-control"></a><span data-ttu-id="a15b5-119">Řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="a15b5-119">Access control</span></span>

<span data-ttu-id="a15b5-120">Pokud si nejste správce nebo vlastníka předplatného Azure, váš účet musí mít alespoň **Přispěvatel** přístup ke skupině prostředků, která obsahuje clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-120">If you are not the administrator/owner of your Azure subscription, your account must have at least **Contributor** access to the resource group that contains the HDInsight cluster.</span></span>

<span data-ttu-id="a15b5-121">Kromě toho při vytváření clusteru služby HDInsight, někdo s alespoň **Přispěvatel** přístup k předplatnému Azure musí již dříve zaregistrovali zprostředkovatele pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-121">Additionally, if you are creating an HDInsight cluster, someone with at least **Contributor** access to the Azure subscription must have previously registered the provider for HDInsight.</span></span> <span data-ttu-id="a15b5-122">Registrace zprostředkovatele se provede, když uživatel s přístupem Přispěvatel poprvé vytvoří prostředek v rámci příslušného předplatného.</span><span class="sxs-lookup"><span data-stu-id="a15b5-122">Provider registration happens when a user with Contributor access to the subscription creates a resource for the first time on the subscription.</span></span> <span data-ttu-id="a15b5-123">Stejného výsledku lze i bez vytvoření prostředku dosáhnout [registrací poskytovatele prostřednictvím REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).</span><span class="sxs-lookup"><span data-stu-id="a15b5-123">It can also be accomplished without creating a resource by [registering a provider using REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).</span></span>

<span data-ttu-id="a15b5-124">Další informace o práci se správou přístupu najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="a15b5-124">For more information on working with access management, see the following documents:</span></span>

* [<span data-ttu-id="a15b5-125">Začínáme se správou přístupu na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a15b5-125">Get started with access management in the Azure portal</span></span>](../active-directory/role-based-access-control-what-is.md)
* [<span data-ttu-id="a15b5-126">Použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="a15b5-126">Use role assignments to manage access to your Azure subscription resources</span></span>](../active-directory/role-based-access-control-configure.md)

## <a name="understanding-script-actions"></a><span data-ttu-id="a15b5-127">Vysvětlení akcí skriptů</span><span class="sxs-lookup"><span data-stu-id="a15b5-127">Understanding Script Actions</span></span>

<span data-ttu-id="a15b5-128">Akce skriptu je jednoduše Bash skript, který zadáte identifikátorů URI a parametry.</span><span class="sxs-lookup"><span data-stu-id="a15b5-128">A Script Action is simply a Bash script that you provide a URI to, and parameters for.</span></span> <span data-ttu-id="a15b5-129">Skript se spustí na uzlech v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-129">The script runs on nodes in the HDInsight cluster.</span></span> <span data-ttu-id="a15b5-130">Dále jsou vlastnosti a funkce akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-130">The following are characteristics and features of script actions.</span></span>

* <span data-ttu-id="a15b5-131">Musí být uložen v identifikátoru URI, který je přístupný z clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-131">Must be stored on a URI that is accessible from the HDInsight cluster.</span></span> <span data-ttu-id="a15b5-132">Toto jsou možné úložiště umístění:</span><span class="sxs-lookup"><span data-stu-id="a15b5-132">The following are possible storage locations:</span></span>

    * <span data-ttu-id="a15b5-133">**Azure Data Lake Store** účet, který je přístupný pro HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="a15b5-133">An **Azure Data Lake Store** account that is accessible by the HDInsight cluster.</span></span> <span data-ttu-id="a15b5-134">Informace o používání Azure Data Lake Store s HDInsight naleznete v tématu [vytvoření clusteru HDInsight s Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-134">For information on using Azure Data Lake Store with HDInsight, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

        <span data-ttu-id="a15b5-135">Při použití skriptu uložených v Data Lake Store, formát identifikátoru URI je `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.</span><span class="sxs-lookup"><span data-stu-id="a15b5-135">When using a script stored in Data Lake Store, the URI format is `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.</span></span>

        > [!NOTE]
        > <span data-ttu-id="a15b5-136">Objekt služby, které HDInsight používá pro přístup k Data Lake Store, musí mít přístup pro čtení do skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-136">The service principal HDInsight uses to access Data Lake Store must have read access to the script.</span></span>

    * <span data-ttu-id="a15b5-137">Objekt blob v **účet úložiště Azure** který je buď primární, nebo další účet úložiště pro HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="a15b5-137">A blob in an **Azure Storage account** that is either the primary or additional storage account for the HDInsight cluster.</span></span> <span data-ttu-id="a15b5-138">HDInsight je udělen přístup k oběma typy účtů úložiště při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-138">HDInsight is granted access to both of these types of storage accounts during cluster creation.</span></span>

    * <span data-ttu-id="a15b5-139">Soubor veřejného sdílení služby objektů Blob v Azure, Githubu, OneDrive, Dropbox, např.</span><span class="sxs-lookup"><span data-stu-id="a15b5-139">A public file sharing service such as Azure Blob, GitHub, OneDrive, Dropbox, etc.</span></span>

        <span data-ttu-id="a15b5-140">Příklad najdete v části identifikátory URI, [příklad skript akce skripty](#example-script-action-scripts) části.</span><span class="sxs-lookup"><span data-stu-id="a15b5-140">For example URIs, see the [Example script action scripts](#example-script-action-scripts) section.</span></span>

        > [!WARNING]
        > <span data-ttu-id="a15b5-141">HDInsight podporuje pouze __pro obecné účely__ účty Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a15b5-141">HDInsight only supports __General-purpose__ Azure Storage accounts.</span></span> <span data-ttu-id="a15b5-142">Nepodporuje aktuálně __úložiště objektů Blob__ typ účtu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-142">It does not currently support the __Blob storage__ account type.</span></span>

* <span data-ttu-id="a15b5-143">Může být omezené **spustili pouze určité typy uzlů**, příklad hlavních uzlech nebo uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-143">Can be restricted to **run on only certain node types**, for example head nodes or worker nodes.</span></span>

  > [!NOTE]
  > <span data-ttu-id="a15b5-144">Při použití s HDInsight Premium, můžete zadat, aby skript by měl být použit hraničního uzlu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-144">When used with HDInsight Premium, you can specify that the script should be used on the edge node.</span></span>

* <span data-ttu-id="a15b5-145">Může být **trvalé** nebo **ad hoc**.</span><span class="sxs-lookup"><span data-stu-id="a15b5-145">Can be **persisted** or **ad hoc**.</span></span>

    <span data-ttu-id="a15b5-146">**Trvalé** skripty se použijí k pracovním uzlům přidat do clusteru po spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-146">**Persisted** scripts are applied to worker nodes added to the cluster after the script runs.</span></span> <span data-ttu-id="a15b5-147">Například při rozšiřování prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-147">For example, when scaling up the cluster.</span></span>

    <span data-ttu-id="a15b5-148">Trvalá akce se skripty může také použít změny na jiný typ uzlu, jako je například hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-148">A persisted script might also apply changes to another node type, such as a head node.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="a15b5-149">Trvalé akce se skripty musí mít jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="a15b5-149">Persisted script actions must have a unique name.</span></span>

    <span data-ttu-id="a15b5-150">**Ad hoc** nejsou trvalé skripty.</span><span class="sxs-lookup"><span data-stu-id="a15b5-150">**Ad hoc** scripts are not persisted.</span></span> <span data-ttu-id="a15b5-151">Uplatňují nejsou k pracovním uzlům přidat do clusteru po má skript spustili.</span><span class="sxs-lookup"><span data-stu-id="a15b5-151">They are not applied to worker nodes added to the cluster after the script has ran.</span></span> <span data-ttu-id="a15b5-152">Následně můžete zvýšit úroveň ad hoc skriptu do trvalého skriptu nebo snížení úrovně trvalého skriptu do ad hoc skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-152">You can subsequently promote an ad hoc script to a persisted script, or demote a persisted script to an ad hoc script.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="a15b5-153">Skript akce, které používají při vytváření clusteru jsou automaticky nastavené jako trvalé.</span><span class="sxs-lookup"><span data-stu-id="a15b5-153">Script actions used during cluster creation are automatically persisted.</span></span>
  >
  > <span data-ttu-id="a15b5-154">Skripty, které nejsou selhání jako trvalé, i když konkrétně znamenat, že by měl být.</span><span class="sxs-lookup"><span data-stu-id="a15b5-154">Scripts that fail are not persisted, even if you specifically indicate that they should be.</span></span>

* <span data-ttu-id="a15b5-155">Můžete přijmout **parametry** používané ve skriptu během provádění.</span><span class="sxs-lookup"><span data-stu-id="a15b5-155">Can accept **parameters** that are used by the script during execution.</span></span>
* <span data-ttu-id="a15b5-156">Spustit s **kořenový úrovně oprávnění** na uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-156">Run with **root level privileges** on the cluster nodes.</span></span>
* <span data-ttu-id="a15b5-157">Umožňuje použít **portál Azure**, **prostředí Azure PowerShell**, **rozhraní příkazového řádku Azure**, nebo **HDInsight .NET SDK**</span><span class="sxs-lookup"><span data-stu-id="a15b5-157">Can be used through the **Azure portal**, **Azure PowerShell**, **Azure CLI**, or **HDInsight .NET SDK**</span></span>

<span data-ttu-id="a15b5-158">Cluster uchovává historii všechny skripty, které mají byly spuštěny.</span><span class="sxs-lookup"><span data-stu-id="a15b5-158">The cluster keeps a history of all scripts that have been ran.</span></span> <span data-ttu-id="a15b5-159">Historie je užitečné, když potřebujete zjistit ID skript pro operace povýšení nebo degradování.</span><span class="sxs-lookup"><span data-stu-id="a15b5-159">The history is useful when you need to find the ID of a script for promotion or demotion operations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a15b5-160">Neexistuje žádný způsob automatického vrátit zpět změny provedené akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-160">There is no automatic way to undo the changes made by a script action.</span></span> <span data-ttu-id="a15b5-161">Buď ručně změny nebo zadejte skript, který je obrátí.</span><span class="sxs-lookup"><span data-stu-id="a15b5-161">Either manually reverse the changes or provide a script that reverses them.</span></span>


### <a name="script-action-in-the-cluster-creation-process"></a><span data-ttu-id="a15b5-162">Akce skriptu v procesu vytváření clusteru</span><span class="sxs-lookup"><span data-stu-id="a15b5-162">Script Action in the cluster creation process</span></span>

<span data-ttu-id="a15b5-163">Skript akce, které používají při vytváření clusteru se mírně liší od skript, který u stávajícího clusteru byla spuštěna akce:</span><span class="sxs-lookup"><span data-stu-id="a15b5-163">Script Actions used during cluster creation are slightly different from script actions ran on an existing cluster:</span></span>

* <span data-ttu-id="a15b5-164">Skript je **automaticky trvalou**.</span><span class="sxs-lookup"><span data-stu-id="a15b5-164">The script is **automatically persisted**.</span></span>
* <span data-ttu-id="a15b5-165">A **selhání** ve skriptu, může způsobit selhání procesu vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-165">A **failure** in the script can cause the cluster creation process to fail.</span></span>

<span data-ttu-id="a15b5-166">Následující diagram znázorňuje, když během procesu vytváření provedení akce skriptu:</span><span class="sxs-lookup"><span data-stu-id="a15b5-166">The following diagram illustrates when Script Action is executed during the creation process:</span></span>

<span data-ttu-id="a15b5-167">![Přizpůsobení cluster HDInsight a fáze při vytváření clusteru][img-hdi-cluster-states]</span><span class="sxs-lookup"><span data-stu-id="a15b5-167">![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]</span></span>

<span data-ttu-id="a15b5-168">Skript se spustí při HDInsight je konfigurován.</span><span class="sxs-lookup"><span data-stu-id="a15b5-168">The script runs while HDInsight is being configured.</span></span> <span data-ttu-id="a15b5-169">V této fázi se skript spustí paralelně na všechny zadané uzly v clusteru a spustí s oprávněními kořenové na uzlech.</span><span class="sxs-lookup"><span data-stu-id="a15b5-169">At this stage, the script runs in parallel on all the specified nodes in the cluster, and runs with root privileges on the nodes.</span></span>

> [!NOTE]
> <span data-ttu-id="a15b5-170">Protože skript se spustí s kořenové úrovně oprávnění na uzlech clusteru, můžete provádět operace, jako je spouštění a zastavování služeb, včetně služby související s Hadoop.</span><span class="sxs-lookup"><span data-stu-id="a15b5-170">Because the script runs with root level privilege on the cluster nodes, you can perform operations like stopping and starting services, including Hadoop-related services.</span></span> <span data-ttu-id="a15b5-171">Pokud zastavíte služby, je třeba zkontrolovat, že služba Ambari a další služby související s Hadoop jsou spuštěny před dokončení spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-171">If you stop services, you must ensure that the Ambari service and other Hadoop-related services are up and running before the script finishes running.</span></span> <span data-ttu-id="a15b5-172">Tyto služby jsou nezbytné pro úspěšně zjistit stav a stav clusteru při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="a15b5-172">These services are required to successfully determine the health and state of the cluster while it is being created.</span></span>


<span data-ttu-id="a15b5-173">Při vytváření clusteru můžete použít různé akce skriptu najednou.</span><span class="sxs-lookup"><span data-stu-id="a15b5-173">During cluster creation, you can use multiple script actions at once.</span></span> <span data-ttu-id="a15b5-174">Tyto skripty jsou spouštěny v pořadí, ve kterém byly zadány.</span><span class="sxs-lookup"><span data-stu-id="a15b5-174">These scripts are invoked in the order in which they were specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a15b5-175">Akce skriptu musí dokončit během 60 minut nebo vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-175">Script actions must complete within 60 minutes, or timeout.</span></span> <span data-ttu-id="a15b5-176">Při zřizování clusteru, bude skript spuštěn souběžně jiné procesy instalací a konfigurací.</span><span class="sxs-lookup"><span data-stu-id="a15b5-176">During cluster provisioning, the script runs concurrently with other setup and configuration processes.</span></span> <span data-ttu-id="a15b5-177">Soutěž o zdroje, jako je například šířky pásma procesoru čas nebo v síti může způsobit skript trvá déle než ve vašem vývojovém prostředí dokončit.</span><span class="sxs-lookup"><span data-stu-id="a15b5-177">Competition for resources such as CPU time or network bandwidth may cause the script to take longer to finish than it does in your development environment.</span></span>
>
> <span data-ttu-id="a15b5-178">Chcete-li minimalizovat čas potřebný pro spuštění skriptu, vyhněte se úlohy, jako je stažení a kompilování aplikací ze zdroje.</span><span class="sxs-lookup"><span data-stu-id="a15b5-178">To minimize the time it takes to run the script, avoid tasks such as downloading and compiling applications from source.</span></span> <span data-ttu-id="a15b5-179">Předem zkompilovat aplikace a uložení binárního souboru ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a15b5-179">Pre-compile applications and store the binary in Azure Storage.</span></span>


### <a name="script-action-on-a-running-cluster"></a><span data-ttu-id="a15b5-180">Akce skriptu na spuštěného clusteru</span><span class="sxs-lookup"><span data-stu-id="a15b5-180">Script action on a running cluster</span></span>

<span data-ttu-id="a15b5-181">Na rozdíl od skript, který byla spuštěna akce používají při vytváření clusteru, selhání ve skriptu na již spuštěnému clusteru automaticky nezpůsobí clusteru ke změně stavu selhání.</span><span class="sxs-lookup"><span data-stu-id="a15b5-181">Unlike script actions used during cluster creation, a failure in a script ran on an already running cluster does not automatically cause the cluster to change to a failed state.</span></span> <span data-ttu-id="a15b5-182">Po dokončení skriptu clusteru by měl vrátit do stavu "spuštění".</span><span class="sxs-lookup"><span data-stu-id="a15b5-182">Once a script completes, the cluster should return to a "running" state.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a15b5-183">I v případě, že má cluster spuštěném stavu., mohou obsahovat nefunkční věcí skript, který selhal.</span><span class="sxs-lookup"><span data-stu-id="a15b5-183">Even if the cluster has a 'running' state, the failed script may have broken things.</span></span> <span data-ttu-id="a15b5-184">Skript může například odstranit soubory, které cluster potřebuje.</span><span class="sxs-lookup"><span data-stu-id="a15b5-184">For example, a script could delete files needed by the cluster.</span></span>
>
> <span data-ttu-id="a15b5-185">Akce skriptů spusťte s oprávněními kořenové, proto byste měli porozumět, skript se před každým jejím použitím v clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-185">Scripts actions run with root privileges, so you should make sure that you understand what a script does before applying it to your cluster.</span></span>

<span data-ttu-id="a15b5-186">Při použití skript do clusteru, je stav clusteru změní z **systémem** k **platných**, pak **HDInsight konfigurace**a nakonec zpět do  **Spuštění** pro úspěšné skripty.</span><span class="sxs-lookup"><span data-stu-id="a15b5-186">When applying a script to a cluster, the cluster state changes to from **Running** to **Accepted**, then **HDInsight configuration**, and finally back to **Running** for successful scripts.</span></span> <span data-ttu-id="a15b5-187">Stav skriptu je zaznamenána v historii akcí skriptu a tyto informace můžete použít k určení, zda skript byla úspěšná nebo neúspěšná.</span><span class="sxs-lookup"><span data-stu-id="a15b5-187">The script status is logged in the script action history, and you can use this information to determine whether the script succeeded or failed.</span></span> <span data-ttu-id="a15b5-188">Například `Get-AzureRmHDInsightScriptActionHistory` rutiny prostředí PowerShell můžete použít k zobrazení stavu skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-188">For example, the `Get-AzureRmHDInsightScriptActionHistory` PowerShell cmdlet can be used to view the status of a script.</span></span> <span data-ttu-id="a15b5-189">Vrací informace podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="a15b5-189">It returns information similar to the following text:</span></span>

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!NOTE]
> <span data-ttu-id="a15b5-190">Pokud jste změnili heslo uživatele (správce) clusteru po vytvoření clusteru, skript, který spustil akce pro tento cluster mohou selhat.</span><span class="sxs-lookup"><span data-stu-id="a15b5-190">If you have changed the cluster user (admin) password after the cluster was created, script actions ran against this cluster may fail.</span></span> <span data-ttu-id="a15b5-191">Pokud máte jakékoli trvalé akce se skripty této cílové uzly pracovního procesu, tyto skripty se pravděpodobně nezdaří při změně měřítka clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-191">If you have any persisted script actions that target worker nodes, these scripts may fail when you scale the cluster.</span></span>

## <a name="example-script-action-scripts"></a><span data-ttu-id="a15b5-192">Příklad akce skriptu skripty</span><span class="sxs-lookup"><span data-stu-id="a15b5-192">Example Script Action scripts</span></span>

<span data-ttu-id="a15b5-193">Skript akce skripty můžete použít následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="a15b5-193">Script Action scripts can be used through the following utilities:</span></span>

* <span data-ttu-id="a15b5-194">portál Azure</span><span class="sxs-lookup"><span data-stu-id="a15b5-194">Azure portal</span></span>
* <span data-ttu-id="a15b5-195">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a15b5-195">Azure PowerShell</span></span>
* <span data-ttu-id="a15b5-196">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a15b5-196">Azure CLI</span></span>
* <span data-ttu-id="a15b5-197">Sada HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="a15b5-197">HDInsight .NET SDK</span></span>

<span data-ttu-id="a15b5-198">HDInsight poskytuje skripty, které v clusterech HDInsight nainstalovat následující součásti:</span><span class="sxs-lookup"><span data-stu-id="a15b5-198">HDInsight provides scripts to install the following components on HDInsight clusters:</span></span>

| <span data-ttu-id="a15b5-199">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a15b5-199">Name</span></span> | <span data-ttu-id="a15b5-200">Skript</span><span class="sxs-lookup"><span data-stu-id="a15b5-200">Script</span></span> |
| --- | --- |
| <span data-ttu-id="a15b5-201">**Přidejte účet služby Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="a15b5-201">**Add an Azure Storage account**</span></span> |<span data-ttu-id="a15b5-202">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxaddstorageaccountv01/Add-Storage-Account-v01.SH. V tématu [přidejte další úložiště do clusteru HDInsight](hdinsight-hadoop-add-storage.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-202">https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh. See [Add additional storage to an HDInsight cluster](hdinsight-hadoop-add-storage.md).</span></span> |
| <span data-ttu-id="a15b5-203">**Instalace aplikace Hue**</span><span class="sxs-lookup"><span data-stu-id="a15b5-203">**Install Hue**</span></span> |<span data-ttu-id="a15b5-204">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-uber-v02.SH. V tématu [instalace a použití clusterů v HDInsight Hue](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-204">https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. See [Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> |
| <span data-ttu-id="a15b5-205">**Nainstalujte Presto**</span><span class="sxs-lookup"><span data-stu-id="a15b5-205">**Install Presto**</span></span> |<span data-ttu-id="a15b5-206">https://RAW.githubusercontent.com/hdinsight/Presto-hdinsight/Master/installpresto.SH. V tématu [instalace a použití clusterů v HDInsight Presto](hdinsight-hadoop-install-presto.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-206">https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh. See [Install and use Presto on HDInsight clusters](hdinsight-hadoop-install-presto.md).</span></span> |
| <span data-ttu-id="a15b5-207">**Nainstalujte Solr**</span><span class="sxs-lookup"><span data-stu-id="a15b5-207">**Install Solr**</span></span> |<span data-ttu-id="a15b5-208">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsolrconfigactionv01/solr-Installer-v01.SH. V tématu [instalace a použití clusterů v HDInsight Solr](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-208">https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh. See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span> |
| <span data-ttu-id="a15b5-209">**Nainstalujte Giraph**</span><span class="sxs-lookup"><span data-stu-id="a15b5-209">**Install Giraph**</span></span> |<span data-ttu-id="a15b5-210">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxgiraphconfigactionv01/giraph-Installer-v01.SH. V tématu [instalace a použití clusterů v HDInsight Giraph](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-210">https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh. See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> |
| <span data-ttu-id="a15b5-211">**Předběžné načtení knihovny Hive**</span><span class="sxs-lookup"><span data-stu-id="a15b5-211">**Pre-load Hive libraries**</span></span> |<span data-ttu-id="a15b5-212">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsetupcustomhivelibsv01/Setup-customhivelibs-v01.SH. V tématu [přidat Hive knihovny v clusterech HDInsight](hdinsight-hadoop-add-hive-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-212">https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh. See [Add Hive libraries on HDInsight clusters](hdinsight-hadoop-add-hive-libraries.md).</span></span> |
| <span data-ttu-id="a15b5-213">**Instalace nebo aktualizace Mono**</span><span class="sxs-lookup"><span data-stu-id="a15b5-213">**Install or update Mono**</span></span> | <span data-ttu-id="a15b5-214">https://hdiconfigactions.BLOB.Core.Windows.NET/Install-mono/Install-mono.bash.</span><span class="sxs-lookup"><span data-stu-id="a15b5-214">https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash.</span></span> <span data-ttu-id="a15b5-215">V tématu [instalace nebo aktualizace Mono v HDInsight](hdinsight-hadoop-install-mono.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-215">See [Install or update Mono on HDInsight](hdinsight-hadoop-install-mono.md).</span></span> |

## <a name="use-a-script-action-during-cluster-creation"></a><span data-ttu-id="a15b5-216">Použití akce skriptu při vytváření clusteru</span><span class="sxs-lookup"><span data-stu-id="a15b5-216">Use a Script Action during cluster creation</span></span>

<span data-ttu-id="a15b5-217">Tato část obsahuje příklady o různých způsobech skriptových akcí můžete při vytváření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-217">This section provides examples on the different ways you can use script actions when creating an HDInsight cluster.</span></span>

### <a name="use-a-script-action-during-cluster-creation-from-the-azure-portal"></a><span data-ttu-id="a15b5-218">Použití akce skriptu při vytváření clusteru z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a15b5-218">Use a Script Action during cluster creation from the Azure portal</span></span>

1. <span data-ttu-id="a15b5-219">Zahájení vytváření clusteru, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-219">Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="a15b5-220">Zastavit, když přejdete __clusteru Souhrn__ části.</span><span class="sxs-lookup"><span data-stu-id="a15b5-220">Stop when you reach the __Cluster summary__ section.</span></span>

2. <span data-ttu-id="a15b5-221">Z __clusteru Souhrn__ vyberte __upravit__ propojení pro __upřesňující nastavení__.</span><span class="sxs-lookup"><span data-stu-id="a15b5-221">From the __Cluster summary__ section, select the __edit__ link for __Advanced settings__.</span></span>

    ![Upřesnit nastavení odkaz](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. <span data-ttu-id="a15b5-223">Z __upřesňující nastavení__ vyberte __skript akce__.</span><span class="sxs-lookup"><span data-stu-id="a15b5-223">From the __Advanced settings__ section, select __Script actions__.</span></span> <span data-ttu-id="a15b5-224">Z __skript akce__ vyberte __+ nové odeslání__</span><span class="sxs-lookup"><span data-stu-id="a15b5-224">From the __Script actions__ section, select __+ Submit new__</span></span>

    ![Odeslat novou akci skriptu](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. <span data-ttu-id="a15b5-226">Použití __vyberte skript__ položka předem vytvořené skriptů.</span><span class="sxs-lookup"><span data-stu-id="a15b5-226">Use the __Select a script__ entry to select a pre-made script.</span></span> <span data-ttu-id="a15b5-227">Chcete-li použít vlastní skript, vyberte __vlastní__ a pak zadejte __název__ a __Bash skriptu URI__ vašeho skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-227">To use a custom script, select __Custom__ and then provide the __Name__ and __Bash script URI__ for your script.</span></span>

    ![Skript přidáte ve formuláři vyberte skript](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    <span data-ttu-id="a15b5-229">Následující tabulka popisuje prvky na formuláři:</span><span class="sxs-lookup"><span data-stu-id="a15b5-229">The following table describes the elements on the form:</span></span>

    | <span data-ttu-id="a15b5-230">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a15b5-230">Property</span></span> | <span data-ttu-id="a15b5-231">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a15b5-231">Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="a15b5-232">Vyberte skript</span><span class="sxs-lookup"><span data-stu-id="a15b5-232">Select a script</span></span> | <span data-ttu-id="a15b5-233">Chcete-li použít vlastní skript, vyberte __vlastní__.</span><span class="sxs-lookup"><span data-stu-id="a15b5-233">To use your own script, select __Custom__.</span></span> <span data-ttu-id="a15b5-234">Vyberte jednu z zadaný skripty, jinak hodnota.</span><span class="sxs-lookup"><span data-stu-id="a15b5-234">Otherwise, select one of the provided scripts.</span></span> |
    | <span data-ttu-id="a15b5-235">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a15b5-235">Name</span></span> |<span data-ttu-id="a15b5-236">Zadejte název akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-236">Specify a name for the script action.</span></span> |
    | <span data-ttu-id="a15b5-237">Skript bash identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="a15b5-237">Bash script URI</span></span> |<span data-ttu-id="a15b5-238">Zadejte identifikátor URI skriptu, která je volána, chcete-li přizpůsobit clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-238">Specify the URI to the script that is invoked to customize the cluster.</span></span> |
    | <span data-ttu-id="a15b5-239">HEAD nebo Worker nebo Zookeeper</span><span class="sxs-lookup"><span data-stu-id="a15b5-239">Head/Worker/Zookeeper</span></span> |<span data-ttu-id="a15b5-240">Zadejte uzly (**Head**, **pracovní**, nebo **ZooKeeper**) podle kterého se spouští skript přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="a15b5-240">Specify the nodes (**Head**, **Worker**, or **ZooKeeper**) on which the customization script is run.</span></span> |
    | <span data-ttu-id="a15b5-241">Parametry</span><span class="sxs-lookup"><span data-stu-id="a15b5-241">Parameters</span></span> |<span data-ttu-id="a15b5-242">Zadejte parametry, pokud se vyžadují skriptem.</span><span class="sxs-lookup"><span data-stu-id="a15b5-242">Specify the parameters, if required by the script.</span></span> |

    <span data-ttu-id="a15b5-243">Použití __zachovat tuto akci skriptu__ položka zajistit, že skript se použije během operace škálování.</span><span class="sxs-lookup"><span data-stu-id="a15b5-243">Use the __Persist this script action__ entry to ensure that the script is applied during scaling operations.</span></span>

5. <span data-ttu-id="a15b5-244">Vyberte __vytvořit__ Uložte skript.</span><span class="sxs-lookup"><span data-stu-id="a15b5-244">Select __Create__ to save the script.</span></span> <span data-ttu-id="a15b5-245">Pak můžete použít __+ odeslání nové__ přidat další skript.</span><span class="sxs-lookup"><span data-stu-id="a15b5-245">You can then use __+ Submit new__ to add another script.</span></span>

    ![Více akcí skriptů](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    <span data-ttu-id="a15b5-247">Po dokončení přidávání skriptů, použijte __vyberte__ tlačítko a potom __Další__ tlačítko se vraťte do __clusteru Souhrn__ části.</span><span class="sxs-lookup"><span data-stu-id="a15b5-247">When you are done adding scripts, use the __Select__ button, and then the __Next__ button to return to the __Cluster summary__ section.</span></span>

3. <span data-ttu-id="a15b5-248">Pokud chcete vytvořit cluster, vyberte __vytvořit__ z __clusteru Souhrn__ výběr.</span><span class="sxs-lookup"><span data-stu-id="a15b5-248">To create the cluster, select __Create__ from the __Cluster summary__ selection.</span></span>

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a><span data-ttu-id="a15b5-249">Použití akce skriptu z šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="a15b5-249">Use a Script Action from Azure Resource Manager templates</span></span>

<span data-ttu-id="a15b5-250">Příklady v této části ukazují, jak použít skript akce pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a15b5-250">The examples in this section demonstrate how to use script actions with Azure Resource Manager templates.</span></span>

#### <a name="before-you-begin"></a><span data-ttu-id="a15b5-251">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a15b5-251">Before you begin</span></span>

* <span data-ttu-id="a15b5-252">Informace o konfiguraci pracovní stanice ke spouštění rutin prostředí HDInsight Powershell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a15b5-252">For information about configuring a workstation to run HDInsight Powershell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="a15b5-253">Pokyny k vytvoření šablony najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-253">For instructions on how to create templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="a15b5-254">Pokud jste ještě nepoužívali prostředí Azure PowerShell s Resource Managerem, přečtěte si téma [použití Azure Powershellu s Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-254">If you have not previously used Azure PowerShell with Resource Manager, see [Using Azure PowerShell with Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

#### <a name="create-clusters-using-script-action"></a><span data-ttu-id="a15b5-255">Vytvořit clustery pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="a15b5-255">Create clusters using Script Action</span></span>

1. <span data-ttu-id="a15b5-256">Následující šablony zkopírujte do umístění v počítači.</span><span class="sxs-lookup"><span data-stu-id="a15b5-256">Copy the following template to a location on your computer.</span></span> <span data-ttu-id="a15b5-257">Tato šablona nainstaluje Giraph headnodes a pracovní uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-257">This template installs Giraph on the headnodes and worker nodes in the cluster.</span></span> <span data-ttu-id="a15b5-258">Můžete také ověřit, zda šablona JSON je platný.</span><span class="sxs-lookup"><span data-stu-id="a15b5-258">You can also verify if the JSON template is valid.</span></span> <span data-ttu-id="a15b5-259">Vložit obsah do šablony [JSONLint](http://jsonlint.com/), online ověření nástroj JSON.</span><span class="sxs-lookup"><span data-stu-id="a15b5-259">Paste your template content into [JSONLint](http://jsonlint.com/), an online JSON validation tool.</span></span>

            {
            "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "clusterLocation": {
                    "type": "string",
                    "defaultValue": "West US",
                    "allowedValues": [ "West US" ]
                },
                "clusterName": {
                    "type": "string"
                },
                "clusterUserName": {
                    "type": "string",
                    "defaultValue": "admin"
                },
                "clusterUserPassword": {
                    "type": "securestring"
                },
                "sshUserName": {
                    "type": "string",
                    "defaultValue": "username"
                },
                "sshPassword": {
                    "type": "securestring"
                },
                "clusterStorageAccountName": {
                    "type": "string"
                },
                "clusterStorageAccountResourceGroup": {
                    "type": "string"
                },
                "clusterStorageType": {
                    "type": "string",
                    "defaultValue": "Standard_LRS",
                    "allowedValues": [
                        "Standard_LRS",
                        "Standard_GRS",
                        "Standard_ZRS"
                    ]
                },
                "clusterStorageAccountContainer": {
                    "type": "string"
                },
                "clusterHeadNodeCount": {
                    "type": "int",
                    "defaultValue": 1
                },
                "clusterWorkerNodeCount": {
                    "type": "int",
                    "defaultValue": 2
                }
            },
            "variables": {
            },
            "resources": [
                {
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-05-01-preview",
                    "dependsOn": [ ],
                    "tags": { },
                    "properties": {
                        "accountType": "[parameters('clusterStorageType')]"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-03-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Storage/storageAccounts/', parameters('clusterStorageAccountName'))]"
                    ],
                    "tags": { },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "hadoop",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterUserPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [
                                {
                                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                    "isDefault": true,
                                    "container": "[parameters('clusterStorageAccountContainer')]",
                                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                                }
                            ]
                        },
                        "computeProfile": {
                            "roles": [
                                {
                                    "name": "headnode",
                                    "targetInstanceCount": "[parameters('clusterHeadNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installGiraph",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                },
                                {
                                    "name": "workernode",
                                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installR",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                }
                            ]
                        }
                    }
                }
            ],
            "outputs": {
                "cluster":{
                    "type" : "object",
                    "value" : "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                }
            }
        }
2. <span data-ttu-id="a15b5-260">Spuštění prostředí Azure PowerShell a přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="a15b5-260">Start Azure PowerShell and Log in to your Azure account.</span></span> <span data-ttu-id="a15b5-261">Po zadání přihlašovacích údajů, příkaz vrátí informace o vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-261">After providing your credentials, the command returns information about your account.</span></span>

        Add-AzureRmAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...
3. <span data-ttu-id="a15b5-262">Pokud máte více předplatných, zadejte ID předplatného, které chcete použít pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="a15b5-262">If you have multiple subscriptions, provide the subscription ID you wish to use for deployment.</span></span>

        Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

    > [!NOTE]
    > <span data-ttu-id="a15b5-263">Můžete použít `Get-AzureRmSubscription` získat seznam Všechna předplatná spojená s vaším účtem, který obsahuje ID předplatného pro každé z nich.</span><span class="sxs-lookup"><span data-stu-id="a15b5-263">You can use `Get-AzureRmSubscription` to get a list of all subscriptions associated with your account, which includes the subscription ID for each one.</span></span>

4. <span data-ttu-id="a15b5-264">Pokud nemáte existující skupinu prostředků, vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a15b5-264">If you do not have an existing resource group, create a resource group.</span></span> <span data-ttu-id="a15b5-265">Zadejte název skupiny prostředků a umístění, které potřebujete pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="a15b5-265">Provide the name of the resource group and location that you need for your solution.</span></span> <span data-ttu-id="a15b5-266">Se vrátí souhrn novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a15b5-266">A summary of the new resource group is returned.</span></span>

        New-AzureRmResourceGroup -Name myresourcegroup -Location "West US"

        ResourceGroupName : myresourcegroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. <span data-ttu-id="a15b5-267">Chcete-li vytvořit nasazení skupiny prostředků, spusťte **New-AzureRmResourceGroupDeployment** příkaz a zadejte potřebné parametry.</span><span class="sxs-lookup"><span data-stu-id="a15b5-267">To create a deployment for your resource group, run the **New-AzureRmResourceGroupDeployment** command and provide the necessary parameters.</span></span> <span data-ttu-id="a15b5-268">Parametry patří následující data:</span><span class="sxs-lookup"><span data-stu-id="a15b5-268">The parameters include the following data:</span></span>

    * <span data-ttu-id="a15b5-269">Název pro vaše nasazení</span><span class="sxs-lookup"><span data-stu-id="a15b5-269">A name for your deployment</span></span>
    * <span data-ttu-id="a15b5-270">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a15b5-270">The name of your resource group</span></span>
    * <span data-ttu-id="a15b5-271">Cesta nebo adresa URL šablonu, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a15b5-271">The path or URL to the template you created.</span></span>

  <span data-ttu-id="a15b5-272">Pokud vaše šablona vyžaduje žádné parametry, musíte zadat také tyto parametry.</span><span class="sxs-lookup"><span data-stu-id="a15b5-272">If your template requires any parameters, you must pass those parameters as well.</span></span> <span data-ttu-id="a15b5-273">V takovém případě akce skriptu k instalaci R na clusteru nevyžaduje žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="a15b5-273">In this case, the script action to install R on the cluster does not require any parameters.</span></span>

        New-AzureRmResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>

    <span data-ttu-id="a15b5-274">Zobrazí se výzva k zadání hodnot pro parametry definované v šabloně.</span><span class="sxs-lookup"><span data-stu-id="a15b5-274">You are prompted to provide values for the parameters defined in the template.</span></span>

1. <span data-ttu-id="a15b5-275">Když nasazený skupinu prostředků, zobrazí se souhrn nasazení.</span><span class="sxs-lookup"><span data-stu-id="a15b5-275">When the resource group has been deployed, a summary of the deployment is displayed.</span></span>

          DeploymentName    : mydeployment
          ResourceGroupName : myresourcegroup
          ProvisioningState : Succeeded
          Timestamp         : 8/14/2017 7:00:27 PM
          Mode              : Incremental
          ...

2. <span data-ttu-id="a15b5-276">Pokud se nasazení nezdaří, můžete použít následující rutiny získejte informace o chyby.</span><span class="sxs-lookup"><span data-stu-id="a15b5-276">If your deployment fails, you can use the following cmdlets to get information about the failures.</span></span>

        Get-AzureRmResourceGroupDeployment -ResourceGroupName myresourcegroup -ProvisioningState Failed

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a><span data-ttu-id="a15b5-277">Použití akce skriptu při vytváření clusteru z prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a15b5-277">Use a Script Action during cluster creation from Azure PowerShell</span></span>

<span data-ttu-id="a15b5-278">V této části používáme [přidat AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) rutiny k vyvolání skripty pomocí akce skriptu k přizpůsobení clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-278">In this section, we use the [Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) cmdlet to invoke scripts by using Script Action to customize a cluster.</span></span> <span data-ttu-id="a15b5-279">Než budete pokračovat, ujistěte se, jste nainstalovali a nakonfigurovali Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a15b5-279">Before proceeding, make sure you have installed and configured Azure PowerShell.</span></span> <span data-ttu-id="a15b5-280">Informace o konfiguraci pracovní stanice ke spouštění rutin prostředí HDInsight PowerShell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a15b5-280">For information about configuring a workstation to run HDInsight PowerShell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="a15b5-281">Následující skript ukazuje, jak se má použít akci skriptu při vytváření clusteru pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a15b5-281">The following script demonstrates how to apply a script action when creating a cluster using PowerShell:</span></span>

<span data-ttu-id="a15b5-282">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]</span><span class="sxs-lookup"><span data-stu-id="a15b5-282">[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]</span></span>

<span data-ttu-id="a15b5-283">To může trvat několik minut, než je vytvořen cluster.</span><span class="sxs-lookup"><span data-stu-id="a15b5-283">It can take several minutes before the cluster is created.</span></span>

### <a name="use-a-script-action-during-cluster-creation-from-the-hdinsight-net-sdk"></a><span data-ttu-id="a15b5-284">Použití akce skriptu při vytváření clusteru ze sady SDK rozhraní .NET HDInsight</span><span class="sxs-lookup"><span data-stu-id="a15b5-284">Use a Script Action during cluster creation from the HDInsight .NET SDK</span></span>

<span data-ttu-id="a15b5-285">.NET SDK služby HDInsight poskytuje klientské knihovny, které usnadňuje práci s HDInsight z aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="a15b5-285">The HDInsight .NET SDK provides client libraries that makes it easier to work with HDInsight from a .NET application.</span></span> <span data-ttu-id="a15b5-286">Ukázka kódu, najdete v části [vytvořit systémem Linux clusterů v HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).</span><span class="sxs-lookup"><span data-stu-id="a15b5-286">For a code sample, see [Create Linux-based clusters in HDInsight using the .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).</span></span>

## <a name="apply-a-script-action-to-a-running-cluster"></a><span data-ttu-id="a15b5-287">Použít akci skriptu do clusteru s podporou spuštěná</span><span class="sxs-lookup"><span data-stu-id="a15b5-287">Apply a Script Action to a running cluster</span></span>

<span data-ttu-id="a15b5-288">V této části zjistěte, jak lze aplikovat akce skriptu do clusteru s podporou spuštěné.</span><span class="sxs-lookup"><span data-stu-id="a15b5-288">In this section, learn how to apply script actions to a running cluster.</span></span>

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-portal"></a><span data-ttu-id="a15b5-289">Použít akci skriptu do clusteru s podporou spuštěné z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a15b5-289">Apply a Script Action to a running cluster from the Azure portal</span></span>

1. <span data-ttu-id="a15b5-290">Z [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-290">From the [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span>

2. <span data-ttu-id="a15b5-291">Přehled clusteru HDInsight, vyberte **akcí skriptů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="a15b5-291">From the HDInsight cluster overview, select the **Script Actions** tile.</span></span>

    ![Dlaždice akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > <span data-ttu-id="a15b5-293">Můžete také vybrat **všechna nastavení** a pak vyberte **akcí skriptů** v části nastavení.</span><span class="sxs-lookup"><span data-stu-id="a15b5-293">You can also select **All settings** and then select **Script Actions** from the Settings section.</span></span>

3. <span data-ttu-id="a15b5-294">Z horní části akcí skriptů, vyberte **odeslání nové**.</span><span class="sxs-lookup"><span data-stu-id="a15b5-294">From the top of the Script Actions section, select **Submit new**.</span></span>

    ![Přidat skript do clusteru s podporou spuštěná](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. <span data-ttu-id="a15b5-296">Použití __vyberte skript__ položka předem vytvořené skriptů.</span><span class="sxs-lookup"><span data-stu-id="a15b5-296">Use the __Select a script__ entry to select a pre-made script.</span></span> <span data-ttu-id="a15b5-297">Chcete-li použít vlastní skript, vyberte __vlastní__ a pak zadejte __název__ a __Bash skriptu URI__ vašeho skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-297">To use a custom script, select __Custom__ and then provide the __Name__ and __Bash script URI__ for your script.</span></span>

    ![Skript přidáte ve formuláři vyberte skript](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    <span data-ttu-id="a15b5-299">Následující tabulka popisuje prvky na formuláři:</span><span class="sxs-lookup"><span data-stu-id="a15b5-299">The following table describes the elements on the form:</span></span>

    | <span data-ttu-id="a15b5-300">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a15b5-300">Property</span></span> | <span data-ttu-id="a15b5-301">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a15b5-301">Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="a15b5-302">Vyberte skript</span><span class="sxs-lookup"><span data-stu-id="a15b5-302">Select a script</span></span> | <span data-ttu-id="a15b5-303">Chcete-li použít vlastní skript, vyberte __vlastní__.</span><span class="sxs-lookup"><span data-stu-id="a15b5-303">To use your own script, select __custom__.</span></span> <span data-ttu-id="a15b5-304">Jinak vyberte možnost poskytnutého skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-304">Otherwise, select a provided script.</span></span> |
    | <span data-ttu-id="a15b5-305">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a15b5-305">Name</span></span> |<span data-ttu-id="a15b5-306">Zadejte název akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-306">Specify a name for the script action.</span></span> |
    | <span data-ttu-id="a15b5-307">Skript bash identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="a15b5-307">Bash script URI</span></span> |<span data-ttu-id="a15b5-308">Zadejte identifikátor URI skriptu, která je volána, chcete-li přizpůsobit clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-308">Specify the URI to the script that is invoked to customize the cluster.</span></span> |
    | <span data-ttu-id="a15b5-309">HEAD nebo Worker nebo Zookeeper</span><span class="sxs-lookup"><span data-stu-id="a15b5-309">Head/Worker/Zookeeper</span></span> |<span data-ttu-id="a15b5-310">Zadejte uzly (**Head**, **pracovní**, nebo **ZooKeeper**) podle kterého se spouští skript přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="a15b5-310">Specify the nodes (**Head**, **Worker**, or **ZooKeeper**) on which the customization script is run.</span></span> |
    | <span data-ttu-id="a15b5-311">Parametry</span><span class="sxs-lookup"><span data-stu-id="a15b5-311">Parameters</span></span> |<span data-ttu-id="a15b5-312">Zadejte parametry, pokud se vyžadují skriptem.</span><span class="sxs-lookup"><span data-stu-id="a15b5-312">Specify the parameters, if required by the script.</span></span> |

    <span data-ttu-id="a15b5-313">Použití __zachovat tuto akci skriptu__ položku a ujistěte se, skript se použije během operace škálování.</span><span class="sxs-lookup"><span data-stu-id="a15b5-313">Use the __Persist this script action__ entry to make sure the script is applied during scaling operations.</span></span>

5. <span data-ttu-id="a15b5-314">Nakonec použijte **vytvořit** tlačítko použít skript do clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-314">Finally, use the **Create** button to apply the script to the cluster.</span></span>

### <a name="apply-a-script-action-to-a-running-cluster-from-azure-powershell"></a><span data-ttu-id="a15b5-315">Použít akci skriptu do clusteru s podporou spuštěné z prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a15b5-315">Apply a Script Action to a running cluster from Azure PowerShell</span></span>

<span data-ttu-id="a15b5-316">Než budete pokračovat, ujistěte se, jste nainstalovali a nakonfigurovali Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a15b5-316">Before proceeding, make sure you have installed and configured Azure PowerShell.</span></span> <span data-ttu-id="a15b5-317">Informace o konfiguraci pracovní stanice ke spouštění rutin prostředí HDInsight PowerShell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a15b5-317">For information about configuring a workstation to run HDInsight PowerShell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="a15b5-318">Následující příklad ukazuje, jak se má použít akce skriptu do spuštěného clusteru:</span><span class="sxs-lookup"><span data-stu-id="a15b5-318">The following example demonstrates how to apply a script action to a running cluster:</span></span>

<span data-ttu-id="a15b5-319">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]</span><span class="sxs-lookup"><span data-stu-id="a15b5-319">[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]</span></span>

<span data-ttu-id="a15b5-320">Po dokončení operace, zobrazí se informace podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="a15b5-320">Once the operation completes, you receive information similar to the following text:</span></span>

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-cli"></a><span data-ttu-id="a15b5-321">Použít akci skriptu do clusteru s podporou spuštěné z rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a15b5-321">Apply a Script Action to a running cluster from the Azure CLI</span></span>

<span data-ttu-id="a15b5-322">Než budete pokračovat, ujistěte se, je nainstalován a nakonfigurován rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="a15b5-322">Before proceeding, make sure you have installed and configured the Azure CLI.</span></span> <span data-ttu-id="a15b5-323">Další informace najdete v tématu [nainstalovat Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-323">For more information, see [Install the Azure CLI](../cli-install-nodejs.md).</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. <span data-ttu-id="a15b5-324">Chcete-li přepnout do režimu Azure Resource Manager, použijte následující příkaz na příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="a15b5-324">To switch to Azure Resource Manager mode, use the following command at the command line:</span></span>

        azure config mode arm

2. <span data-ttu-id="a15b5-325">Použijte následující k ověření vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="a15b5-325">Use the following to authenticate to your Azure subscription.</span></span>

        azure login

3. <span data-ttu-id="a15b5-326">Použijte následující příkaz pro použití akce skriptu do clusteru s podporou spuštěná</span><span class="sxs-lookup"><span data-stu-id="a15b5-326">Use the following command to apply a script action to a running cluster</span></span>

        azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>

    <span data-ttu-id="a15b5-327">Pokud nezadáte parametry pro tento příkaz, zobrazí se výzva pro ně.</span><span class="sxs-lookup"><span data-stu-id="a15b5-327">If you omit parameters for this command, you are prompted for them.</span></span> <span data-ttu-id="a15b5-328">Pokud skript určete s `-u` přijímá parametry, můžete je zadat pomocí `-p` parametr.</span><span class="sxs-lookup"><span data-stu-id="a15b5-328">If the script you specify with `-u` accepts parameters, you can specify them using the `-p` parameter.</span></span>

    <span data-ttu-id="a15b5-329">Uzel platné typy jsou `headnode`, `workernode`, a `zookeeper`.</span><span class="sxs-lookup"><span data-stu-id="a15b5-329">Valid node types are `headnode`, `workernode`, and `zookeeper`.</span></span> <span data-ttu-id="a15b5-330">Pokud skript má být použita na více typy uzlů, určit typy oddělených ';'.</span><span class="sxs-lookup"><span data-stu-id="a15b5-330">If the script should be applied to multiple node types, specify the types separated by a ';'.</span></span> <span data-ttu-id="a15b5-331">Například, `-n headnode;workernode`.</span><span class="sxs-lookup"><span data-stu-id="a15b5-331">For example, `-n headnode;workernode`.</span></span>

    <span data-ttu-id="a15b5-332">Chcete-li zachovat skript, přidejte `--persistOnSuccess`.</span><span class="sxs-lookup"><span data-stu-id="a15b5-332">To persist the script, add the `--persistOnSuccess`.</span></span> <span data-ttu-id="a15b5-333">Vám může také zachovat skript později pomocí `azure hdinsight script-action persisted set`.</span><span class="sxs-lookup"><span data-stu-id="a15b5-333">You can also persist the script later by using `azure hdinsight script-action persisted set`.</span></span>

    <span data-ttu-id="a15b5-334">Po dokončení úlohy, zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="a15b5-334">Once the job completes, you receive output similar to the following text:</span></span>

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-to-a-running-cluster-using-rest-api"></a><span data-ttu-id="a15b5-335">Použít akci skriptu do clusteru s podporou spuštěná pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="a15b5-335">Apply a Script Action to a running cluster using REST API</span></span>

<span data-ttu-id="a15b5-336">V tématu [spuštění akcí skriptů v clusteru s podporou spuštěné](https://msdn.microsoft.com/library/azure/mt668441.aspx).</span><span class="sxs-lookup"><span data-stu-id="a15b5-336">See [Run Script Actions on a running cluster](https://msdn.microsoft.com/library/azure/mt668441.aspx).</span></span>

### <a name="apply-a-script-action-to-a-running-cluster-from-the-hdinsight-net-sdk"></a><span data-ttu-id="a15b5-337">Pro cluster běžící akce skriptu ze sady SDK rozhraní .NET HDInsight</span><span class="sxs-lookup"><span data-stu-id="a15b5-337">Apply a Script Action to a running cluster from the HDInsight .NET SDK</span></span>

<span data-ttu-id="a15b5-338">Příklad použití sady .NET SDK skripty do clusteru, naleznete v části [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span><span class="sxs-lookup"><span data-stu-id="a15b5-338">For an example of using the .NET SDK to apply scripts to a cluster, see [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span></span>

## <a name="view-history-promote-and-demote-script-actions"></a><span data-ttu-id="a15b5-339">Zobrazit historii, povýšení a snížení akcí skriptů</span><span class="sxs-lookup"><span data-stu-id="a15b5-339">View history, promote, and demote Script Actions</span></span>

### <a name="using-the-azure-portal"></a><span data-ttu-id="a15b5-340">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a15b5-340">Using the Azure portal</span></span>

1. <span data-ttu-id="a15b5-341">Z [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-341">From the [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span>

2. <span data-ttu-id="a15b5-342">Přehled clusteru HDInsight, vyberte **akcí skriptů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="a15b5-342">From the HDInsight cluster overview, select the **Script Actions** tile.</span></span>

    ![Dlaždice akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > <span data-ttu-id="a15b5-344">Můžete také vybrat **všechna nastavení** a pak vyberte **akcí skriptů** v části nastavení.</span><span class="sxs-lookup"><span data-stu-id="a15b5-344">You can also select **All settings** and then select **Script Actions** from the Settings section.</span></span>

4. <span data-ttu-id="a15b5-345">Historie skripty pro tento cluster se zobrazí v části Akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-345">A history of scripts for this cluster is displayed on the Script Actions section.</span></span> <span data-ttu-id="a15b5-346">Tyto informace zahrnují seznam trvalou skripty.</span><span class="sxs-lookup"><span data-stu-id="a15b5-346">This information includes a list of persisted scripts.</span></span> <span data-ttu-id="a15b5-347">Na tomto snímku obrazovky uvidíte, že Solr byl skript spustili na tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-347">In the screenshot below, you can see that the Solr script has been ran on this cluster.</span></span> <span data-ttu-id="a15b5-348">Na snímku obrazovky nezobrazuje žádné trvalé skripty.</span><span class="sxs-lookup"><span data-stu-id="a15b5-348">The screenshot does not show any persisted scripts.</span></span>

    ![Oddíl akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. <span data-ttu-id="a15b5-350">Výběr skript z historie zobrazí v části Vlastnosti pro tento skript.</span><span class="sxs-lookup"><span data-stu-id="a15b5-350">Selecting a script from the history displays the Properties section for this script.</span></span> <span data-ttu-id="a15b5-351">Z horní části obrazovky se můžete znovu spustit skript nebo povyšte ho.</span><span class="sxs-lookup"><span data-stu-id="a15b5-351">From the top of the screen, you can rerun the script or promote it.</span></span>

    ![Vlastnosti akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. <span data-ttu-id="a15b5-353">Můžete také **...**  napravo od položky v oddílu akce skriptu k provádění akcí.</span><span class="sxs-lookup"><span data-stu-id="a15b5-353">You can also use the **...** to the right of entries on the Script Actions section to perform actions.</span></span>

    ![Skript akce... využití](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a><span data-ttu-id="a15b5-355">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="a15b5-355">Using Azure PowerShell</span></span>

| <span data-ttu-id="a15b5-356">Použijte následující...</span><span class="sxs-lookup"><span data-stu-id="a15b5-356">Use the following...</span></span> | <span data-ttu-id="a15b5-357">K...</span><span class="sxs-lookup"><span data-stu-id="a15b5-357">To ...</span></span> |
| --- | --- |
| <span data-ttu-id="a15b5-358">Get-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="a15b5-358">Get-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="a15b5-359">Načíst informace o trvalé akce se skripty</span><span class="sxs-lookup"><span data-stu-id="a15b5-359">Retrieve information on persisted script actions</span></span> |
| <span data-ttu-id="a15b5-360">Get-AzureRmHDInsightScriptActionHistory</span><span class="sxs-lookup"><span data-stu-id="a15b5-360">Get-AzureRmHDInsightScriptActionHistory</span></span> |<span data-ttu-id="a15b5-361">Načíst historii akcí skriptu použít v clusteru nebo podrobnosti specifického skriptu</span><span class="sxs-lookup"><span data-stu-id="a15b5-361">Retrieve a history of script actions applied to the cluster, or details for a specific script</span></span> |
| <span data-ttu-id="a15b5-362">Set-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="a15b5-362">Set-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="a15b5-363">Zvýší úroveň akci ad hoc skriptu pro akci trvalého skriptu</span><span class="sxs-lookup"><span data-stu-id="a15b5-363">Promotes an ad hoc script action to a persisted script action</span></span> |
| <span data-ttu-id="a15b5-364">Remove-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="a15b5-364">Remove-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="a15b5-365">Sníží úroveň akcí trvalého skriptu na ad hoc akci</span><span class="sxs-lookup"><span data-stu-id="a15b5-365">Demotes a persisted script action to an ad hoc action</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="a15b5-366">Pomocí `Remove-AzureRmHDInsightPersistedScriptAction` nevrátí zpět akci, kterou provádí skript.</span><span class="sxs-lookup"><span data-stu-id="a15b5-366">Using `Remove-AzureRmHDInsightPersistedScriptAction` does not undo the actions performed by a script.</span></span> <span data-ttu-id="a15b5-367">Tato rutina odebere jenom příznak trvalý.</span><span class="sxs-lookup"><span data-stu-id="a15b5-367">This cmdlet only removes the persisted flag.</span></span>

<span data-ttu-id="a15b5-368">Následující ukázkový skript ukazuje použití rutiny povýšit, pak snížení úrovně skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-368">The following example script demonstrates using the cmdlets to promote, then demote a script.</span></span>

<span data-ttu-id="a15b5-369">[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]</span><span class="sxs-lookup"><span data-stu-id="a15b5-369">[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]</span></span>

### <a name="using-the-azure-cli"></a><span data-ttu-id="a15b5-370">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a15b5-370">Using the Azure CLI</span></span>

| <span data-ttu-id="a15b5-371">Použijte následující...</span><span class="sxs-lookup"><span data-stu-id="a15b5-371">Use the following...</span></span> | <span data-ttu-id="a15b5-372">K...</span><span class="sxs-lookup"><span data-stu-id="a15b5-372">To ...</span></span> |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |<span data-ttu-id="a15b5-373">Načtení seznamu akcí trvalého skriptu</span><span class="sxs-lookup"><span data-stu-id="a15b5-373">Retrieve a list of persisted script actions</span></span> |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |<span data-ttu-id="a15b5-374">Načíst informace o konkrétní trvalého skriptu akce</span><span class="sxs-lookup"><span data-stu-id="a15b5-374">Retrieve information on a specific persisted script action</span></span> |
| `azure hdinsight script-action history list <clustername>` |<span data-ttu-id="a15b5-375">Načíst historii akcí skriptu použít do clusteru</span><span class="sxs-lookup"><span data-stu-id="a15b5-375">Retrieve a history of script actions applied to the cluster</span></span> |
| `azure hdinsight script-action history show <clustername> <scriptname>` |<span data-ttu-id="a15b5-376">Načíst informace o konkrétní skript akce</span><span class="sxs-lookup"><span data-stu-id="a15b5-376">Retrieve information on a specific script action</span></span> |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |<span data-ttu-id="a15b5-377">Zvýší úroveň akci ad hoc skriptu pro akci trvalého skriptu</span><span class="sxs-lookup"><span data-stu-id="a15b5-377">Promotes an ad hoc script action to a persisted script action</span></span> |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |<span data-ttu-id="a15b5-378">Sníží úroveň akcí trvalého skriptu na ad hoc akci</span><span class="sxs-lookup"><span data-stu-id="a15b5-378">Demotes a persisted script action to an ad hoc action</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="a15b5-379">Pomocí `azure hdinsight script-action persisted delete` nevrátí zpět akci, kterou provádí skript.</span><span class="sxs-lookup"><span data-stu-id="a15b5-379">Using `azure hdinsight script-action persisted delete` does not undo the actions performed by a script.</span></span> <span data-ttu-id="a15b5-380">Tato rutina odebere jenom příznak trvalý.</span><span class="sxs-lookup"><span data-stu-id="a15b5-380">This cmdlet only removes the persisted flag.</span></span>

### <a name="using-the-hdinsight-net-sdk"></a><span data-ttu-id="a15b5-381">Pomocí HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="a15b5-381">Using the HDInsight .NET SDK</span></span>

<span data-ttu-id="a15b5-382">Příklad použití sady .NET SDK k načtení historie skriptu z clusteru, zvýšení úrovně nebo snížení úrovně skriptů najdete v tématu [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span><span class="sxs-lookup"><span data-stu-id="a15b5-382">For an example of using the .NET SDK to retrieve script history from a cluster, promote or demote scripts, see [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span></span>

> [!NOTE]
> <span data-ttu-id="a15b5-383">Tento příklad také ukazuje, jak instalace aplikace HDInsight pomocí sady .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="a15b5-383">This example also demonstrates how to install an HDInsight application using the .NET SDK.</span></span>

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a><span data-ttu-id="a15b5-384">Podpora pro open-source softwaru použít na clustery HDInsight</span><span class="sxs-lookup"><span data-stu-id="a15b5-384">Support for open-source software used on HDInsight clusters</span></span>

<span data-ttu-id="a15b5-385">Služba Microsoft Azure HDInsight používá prostředí technologie open source vytvořen kolem Hadoop.</span><span class="sxs-lookup"><span data-stu-id="a15b5-385">The Microsoft Azure HDInsight service uses an ecosystem of open-source technologies formed around Hadoop.</span></span> <span data-ttu-id="a15b5-386">Microsoft Azure poskytuje obecné úroveň podpory pro technologie open source.</span><span class="sxs-lookup"><span data-stu-id="a15b5-386">Microsoft Azure provides a general level of support for open-source technologies.</span></span> <span data-ttu-id="a15b5-387">Další informace najdete v tématu **podporu oboru** části [nejčastější dotazy týkající se podpory Azure web](https://azure.microsoft.com/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="a15b5-387">For more information, see the **Support Scope** section of the [Azure Support FAQ website](https://azure.microsoft.com/support/faq/).</span></span> <span data-ttu-id="a15b5-388">Služba HDInsight poskytuje další úroveň podpory pro integrované komponenty.</span><span class="sxs-lookup"><span data-stu-id="a15b5-388">The HDInsight service provides an additional level of support for built-in components.</span></span>

<span data-ttu-id="a15b5-389">Existují dva typy open-source komponent, které jsou k dispozici ve službě HDInsight:</span><span class="sxs-lookup"><span data-stu-id="a15b5-389">There are two types of open-source components that are available in the HDInsight service:</span></span>

* <span data-ttu-id="a15b5-390">**Integrované komponenty** -tyto komponenty jsou předinstalované na clustery HDInsight a poskytují základní funkce služby clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-390">**Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of the cluster.</span></span> <span data-ttu-id="a15b5-391">Například YARN ResourceManager, Hive dotazovací jazyk (HiveQL) a knihovně Mahout patří do této kategorie.</span><span class="sxs-lookup"><span data-stu-id="a15b5-391">For example, YARN ResourceManager, the Hive query language (HiveQL), and the Mahout library belong to this category.</span></span> <span data-ttu-id="a15b5-392">Úplný seznam součástí clusteru je k dispozici v [co je nového ve verzích clusterů systému Hadoop poskytovaných v HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-392">A full list of cluster components is available in [What's new in the Hadoop cluster versions provided by HDInsight](hdinsight-component-versioning.md).</span></span>
* <span data-ttu-id="a15b5-393">**Vlastní komponenty** -, jako uživatel clusteru, můžete nainstalovat nebo použít v vaše úlohy žádné součásti k dispozici v komunitě nebo vytvořené vámi.</span><span class="sxs-lookup"><span data-stu-id="a15b5-393">**Custom components** - You, as a user of the cluster, can install or use in your workload any component available in the community or created by you.</span></span>

> [!WARNING]
> <span data-ttu-id="a15b5-394">Součásti, které jsou součástí clusteru HDInsight jsou plně podporovány.</span><span class="sxs-lookup"><span data-stu-id="a15b5-394">Components provided with the HDInsight cluster are fully supported.</span></span> <span data-ttu-id="a15b5-395">Microsoft Support pomáhá izolovat a vyřešení problémů týkajících se těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="a15b5-395">Microsoft Support helps to isolate and resolve issues related to these components.</span></span>
>
> <span data-ttu-id="a15b5-396">Vlastní komponenty získat vyvineme podporu k pomoci při další řešení problému.</span><span class="sxs-lookup"><span data-stu-id="a15b5-396">Custom components receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="a15b5-397">Podporu společnosti Microsoft může být schopni vyřešit problém nebo mohou požádat, abyste zaujmout dostupné kanály pro technologie s otevřeným zdrojem, kterých se nachází hluboké znalosti pro tuto technologii.</span><span class="sxs-lookup"><span data-stu-id="a15b5-397">Microsoft support may be able to resolve the issue OR they may ask you to engage available channels for the open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="a15b5-398">Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a15b5-398">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>

<span data-ttu-id="a15b5-399">Služba HDInsight poskytuje několik způsobů, jak používat vlastní komponenty.</span><span class="sxs-lookup"><span data-stu-id="a15b5-399">The HDInsight service provides several ways to use custom components.</span></span> <span data-ttu-id="a15b5-400">Stejnou úroveň podpory platí bez ohledu na to, jak je součást použít nebo nainstalované v clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-400">The same level of support applies, regardless of how a component is used or installed on the cluster.</span></span> <span data-ttu-id="a15b5-401">Následující seznam popisuje nejběžnější způsoby vlastní komponenty lze v clusterech HDInsight:</span><span class="sxs-lookup"><span data-stu-id="a15b5-401">The following list describes the most common ways that custom components can be used on HDInsight clusters:</span></span>

1. <span data-ttu-id="a15b5-402">Úloha odeslání - Hadoop nebo jiné typy úloh, které spustit nebo používat vlastní komponenty lze odeslat do clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-402">Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted to the cluster.</span></span>

2. <span data-ttu-id="a15b5-403">Přizpůsobení clusteru – při vytváření clusteru, můžete zadat další nastavení a vlastní součásti, které jsou nainstalovány na uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-403">Cluster customization - During cluster creation, you can specify additional settings and custom components that are installed on the cluster nodes.</span></span>

3. <span data-ttu-id="a15b5-404">Ukázky - oblíbených vlastní součásti, Microsoft a ostatní mohou poskytnout ukázky použití těchto součástí v clusterech HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-404">Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on the HDInsight clusters.</span></span> <span data-ttu-id="a15b5-405">Tyto soubory jsou uvedeny bez podpory.</span><span class="sxs-lookup"><span data-stu-id="a15b5-405">These samples are provided without support.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a15b5-406">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a15b5-406">Troubleshooting</span></span>

<span data-ttu-id="a15b5-407">Webovému uživatelskému rozhraní Ambari slouží k zobrazení informací zaznamenaných akcí skriptů.</span><span class="sxs-lookup"><span data-stu-id="a15b5-407">You can use Ambari web UI to view information logged by script actions.</span></span> <span data-ttu-id="a15b5-408">Pokud skript selže při vytváření clusteru, protokoly jsou také k dispozici v výchozí účet úložiště přidružený ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-408">If the script fails during cluster creation, the logs are also available in the default storage account associated with the cluster.</span></span> <span data-ttu-id="a15b5-409">Tato část obsahuje informace o tom, jak načíst protokolů pomocí obou těchto možností.</span><span class="sxs-lookup"><span data-stu-id="a15b5-409">This section provides information on how to retrieve the logs using both these options.</span></span>

### <a name="using-the-ambari-web-ui"></a><span data-ttu-id="a15b5-410">Pomocí Ambari webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="a15b5-410">Using the Ambari Web UI</span></span>

1. <span data-ttu-id="a15b5-411">V prohlížeči přejděte na https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="a15b5-411">In your browser, navigate to https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="a15b5-412">Nahraďte název clusteru s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-412">Replace CLUSTERNAME with the name of your HDInsight cluster.</span></span>

    <span data-ttu-id="a15b5-413">Po zobrazení výzvy zadejte název účtu správce (správce) a heslo pro cluster.</span><span class="sxs-lookup"><span data-stu-id="a15b5-413">When prompted, enter the admin account name (admin) and password for the cluster.</span></span> <span data-ttu-id="a15b5-414">Možná budete muset znovu zadat přihlašovací údaje správce v webového formuláře.</span><span class="sxs-lookup"><span data-stu-id="a15b5-414">You may have to reenter the admin credentials in a web form.</span></span>

2. <span data-ttu-id="a15b5-415">Z panelu v horní části stránky, vyberte **ops** položku.</span><span class="sxs-lookup"><span data-stu-id="a15b5-415">From the bar at the top of the page, select the **ops** entry.</span></span> <span data-ttu-id="a15b5-416">Zobrazí se seznam aktuální a předchozí operace provedené na clusteru prostřednictvím Ambari.</span><span class="sxs-lookup"><span data-stu-id="a15b5-416">A list of current and previous operations performed on the cluster through Ambari is displayed.</span></span>

    ![Panel uživatelského rozhraní Ambari web s ops vybrané](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. <span data-ttu-id="a15b5-418">Najděte položky, které mají **spustit\_customscriptaction** v **Operations** sloupce.</span><span class="sxs-lookup"><span data-stu-id="a15b5-418">Find the entries that have **run\_customscriptaction** in the **Operations** column.</span></span> <span data-ttu-id="a15b5-419">Tyto položky se vytvoří při spuštění akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-419">These entries are created when the Script Actions run.</span></span>

    ![Snímek obrazovky operací](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    <span data-ttu-id="a15b5-421">Chcete-li zobrazit výstup STDOUT a STDERR, vyberte položku run\customscriptaction a procházení odkazy.</span><span class="sxs-lookup"><span data-stu-id="a15b5-421">To view the STDOUT and STDERR output, select the run\customscriptaction entry and drill down through the links.</span></span> <span data-ttu-id="a15b5-422">Tento výstup se vygeneruje, když bude skript spuštěn a můžou obsahovat užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="a15b5-422">This output is generated when the script runs, and may contain useful information.</span></span>

### <a name="access-logs-from-the-default-storage-account"></a><span data-ttu-id="a15b5-423">Přístup k protokolům z výchozí účet úložiště</span><span class="sxs-lookup"><span data-stu-id="a15b5-423">Access logs from the default storage account</span></span>

<span data-ttu-id="a15b5-424">Pokud se vytvoření clusteru se nezdaří z důvodu chyby akce skriptu, protokoly jsou přístupné z účtu úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-424">If the cluster creation fails due to a script action error, the logs can be accessed from the cluster storage account.</span></span>

* <span data-ttu-id="a15b5-425">Protokoly úložiště jsou dostupné v `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.</span><span class="sxs-lookup"><span data-stu-id="a15b5-425">The storage logs are available at `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.</span></span>

    ![Snímek obrazovky operací](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    <span data-ttu-id="a15b5-427">V tomto adresáři protokoly jsou headnode, workernode a uzly zookeeper uspořádány samostatně.</span><span class="sxs-lookup"><span data-stu-id="a15b5-427">Under this directory, the logs are organized separately for headnode, workernode, and zookeeper nodes.</span></span> <span data-ttu-id="a15b5-428">Tady je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="a15b5-428">Some examples are:</span></span>

    * <span data-ttu-id="a15b5-429">**Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="a15b5-429">**Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`</span></span>

    * <span data-ttu-id="a15b5-430">**Pracovního uzlu** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="a15b5-430">**Worker node** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`</span></span>

    * <span data-ttu-id="a15b5-431">**Zookeeper uzlu** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="a15b5-431">**Zookeeper node** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`</span></span>

* <span data-ttu-id="a15b5-432">Účet úložiště se nahraje všechny stdout a stderr odpovídající hostitele.</span><span class="sxs-lookup"><span data-stu-id="a15b5-432">All stdout and stderr of the corresponding host is uploaded to the storage account.</span></span> <span data-ttu-id="a15b5-433">Existuje **výstup -\*.txt** a **chyby -\*.txt** pro jednotlivé akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-433">There is one **output-\*.txt** and **errors-\*.txt** for each script action.</span></span> <span data-ttu-id="a15b5-434">*.Txt výstupní soubor obsahuje informace o identifikátoru URI skript, který nebyl spustit na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="a15b5-434">The output-*.txt file contains information about the URI of the script that got run on the host.</span></span> <span data-ttu-id="a15b5-435">Například</span><span class="sxs-lookup"><span data-stu-id="a15b5-435">For example</span></span>

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* <span data-ttu-id="a15b5-436">Je možné, opakovaně vytvořit cluster akce skriptu se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="a15b5-436">It's possible that you repeatedly create a script action cluster with the same name.</span></span> <span data-ttu-id="a15b5-437">V takovém případě můžete odlišit relevantní protokoly na základě názvu složky datum.</span><span class="sxs-lookup"><span data-stu-id="a15b5-437">In such case, you can distinguish the relevant logs based on the DATE folder name.</span></span> <span data-ttu-id="a15b5-438">Struktura složek pro cluster s podporou (mycluster) na různá data vytvořit například zobrazí podobná následující položky protokolu:</span><span class="sxs-lookup"><span data-stu-id="a15b5-438">For example, the folder structure for a cluster (mycluster) created on different dates appears similar to the following log entries:</span></span>

    <span data-ttu-id="a15b5-439">`\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`</span><span class="sxs-lookup"><span data-stu-id="a15b5-439">`\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`</span></span>

* <span data-ttu-id="a15b5-440">Pokud vytvoříte cluster akce skriptu se stejným názvem ve stejný den, můžete k identifikaci příslušných protokolových souborů jedinečnou předponu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-440">If you create a script action cluster with the same name on the same day, you can use the unique prefix to identify the relevant log files.</span></span>

* <span data-ttu-id="a15b5-441">Pokud vytvoříte cluster s podporou téměř 12:00 AM (půlnoc), je možné, že soubory protokolu rozprostřít do dvou dnů.</span><span class="sxs-lookup"><span data-stu-id="a15b5-441">If you create a cluster near 12:00AM (midnight), it's possible that the log files span across two days.</span></span> <span data-ttu-id="a15b5-442">V takových případech uvidíte dvě složky na jiné datum pro stejného clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-442">In such cases, you see two different date folders for the same cluster.</span></span>

* <span data-ttu-id="a15b5-443">Nahrávání souborů protokolu ke kontejneru výchozí může trvat až 5 minut, zejména u velkých clusterech.</span><span class="sxs-lookup"><span data-stu-id="a15b5-443">Uploading log files to the default container can take up to 5 mins, especially for large clusters.</span></span> <span data-ttu-id="a15b5-444">Ano Pokud chcete získat přístup v protokolech, nedoporučuje se mazat okamžitě clusteru neúspěšné provedení akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-444">So, if you want to access the logs, you should not immediately delete the cluster if a script action fails.</span></span>

### <a name="ambari-watchdog"></a><span data-ttu-id="a15b5-445">Ambari sledovacího zařízení</span><span class="sxs-lookup"><span data-stu-id="a15b5-445">Ambari watchdog</span></span>

> [!WARNING]
> <span data-ttu-id="a15b5-446">Neměňte heslo pro Ambari sledovací zařízení (hdinsightwatchdog) v clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="a15b5-446">Do not change the password for the Ambari Watchdog (hdinsightwatchdog) on your Linux-based HDInsight cluster.</span></span> <span data-ttu-id="a15b5-447">Změna hesla pro tento účet dělí umožňuje spouštění nového akcí skriptů v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-447">Changing the password for this account breaks the ability to run new script actions on the HDInsight cluster.</span></span>

### <a name="cant-import-name-blobservice"></a><span data-ttu-id="a15b5-448">Nelze importovat název BlobService</span><span class="sxs-lookup"><span data-stu-id="a15b5-448">Can't import name BlobService</span></span>

<span data-ttu-id="a15b5-449">__Příznaky__: selhání akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-449">__Symptoms__: The script action fails.</span></span> <span data-ttu-id="a15b5-450">Při operaci zobrazení Ambari, zobrazí se text podobná následující chybě:</span><span class="sxs-lookup"><span data-stu-id="a15b5-450">Text similar to the following error is displayed when you view the operation in Ambari:</span></span>

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

<span data-ttu-id="a15b5-451">__Příčina__: k této chybě dojde, pokud upgradujete klienta Python Azure Storage, který je součástí clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a15b5-451">__Cause__: This error occurs if you upgrade the Python Azure Storage client that is included with the HDInsight cluster.</span></span> <span data-ttu-id="a15b5-452">HDInsight očekává klienta Azure Storage 0.20.0.</span><span class="sxs-lookup"><span data-stu-id="a15b5-452">HDInsight expects Azure Storage client 0.20.0.</span></span>

<span data-ttu-id="a15b5-453">__Řešení__: Chcete-li tuto chybu vyřešit, ručně připojit k každého uzlu clusteru pomocí `ssh` a znovu nainstalujte požadovanou verzi klienta správný úložiště pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="a15b5-453">__Resolution__: To resolve this error, manually connect to each cluster node using `ssh` and use the following command to reinstall the correct storage client version:</span></span>

```
sudo pip install azure-storage==0.20.0
```

<span data-ttu-id="a15b5-454">Informace o připojení ke clusteru pomocí protokolu SSH naleznete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="a15b5-454">For information on connecting to the cluster with SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a><span data-ttu-id="a15b5-455">Historie nezobrazí skripty použité při vytváření clusteru</span><span class="sxs-lookup"><span data-stu-id="a15b5-455">History doesn't show scripts used during cluster creation</span></span>

<span data-ttu-id="a15b5-456">Pokud váš cluster byl vytvořen ještě před 15. března 2016, nemusíte vidět položku v historii akcí skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-456">If your cluster was created before March 15, 2016, you may not see an entry in Script Action history.</span></span> <span data-ttu-id="a15b5-457">Pokud změníte velikost clusteru po 15. března 2016, skripty, pomocí při vytváření clusteru zobrazí v historii, jako jsou nastavení použita na nové uzly v clusteru jako součást operace změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="a15b5-457">If you resize the cluster after March 15, 2016, the scripts using during cluster creation appear in history as they are applied to new nodes in the cluster as part of the resize operation.</span></span>

<span data-ttu-id="a15b5-458">Existují dvě výjimky:</span><span class="sxs-lookup"><span data-stu-id="a15b5-458">There are two exceptions:</span></span>

* <span data-ttu-id="a15b5-459">Pokud je váš cluster vytvořený před 1. září 2015.</span><span class="sxs-lookup"><span data-stu-id="a15b5-459">If your cluster was created before September 1, 2015.</span></span> <span data-ttu-id="a15b5-460">Toto datum je při zavedených akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="a15b5-460">This date is when Script Actions were introduced.</span></span> <span data-ttu-id="a15b5-461">Žádného clusteru vytvořené před tímto datem nelze použít akcí skriptů pro vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="a15b5-461">Any cluster created before this date could not have used Script Actions for cluster creation.</span></span>

* <span data-ttu-id="a15b5-462">Pokud používají různé akce skriptu při vytváření clusteru a používá stejný název pro několik skriptů, nebo stejný název, stejným identifikátorem URI, ale různé parametry pro několik skriptů.</span><span class="sxs-lookup"><span data-stu-id="a15b5-462">If you used multiple Script Actions during cluster creation, and used the same name for multiple scripts, or the same name, same URI, but different parameters for multiple scripts.</span></span> <span data-ttu-id="a15b5-463">V těchto případech dojít k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="a15b5-463">In these cases, you receive the following error:</span></span>

    <span data-ttu-id="a15b5-464">Žádné nový skript, který může být akce byla spuštěna na tomto clusteru z důvodu konfliktu názvů skript v existující skripty.</span><span class="sxs-lookup"><span data-stu-id="a15b5-464">No new script actions can be ran on this cluster due to conflicting script names in existing scripts.</span></span> <span data-ttu-id="a15b5-465">Vytvoření skriptu názvy uvedených v clusteru musí být všechny jedinečné.</span><span class="sxs-lookup"><span data-stu-id="a15b5-465">Script names provided at cluster create must be all unique.</span></span> <span data-ttu-id="a15b5-466">Existující skripty jsou spuštěné v změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="a15b5-466">Existing scripts are ran on resize.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a15b5-467">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a15b5-467">Next steps</span></span>

* [<span data-ttu-id="a15b5-468">Vývoj skriptů akce skriptu pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="a15b5-468">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions-linux.md)
* [<span data-ttu-id="a15b5-469">Nainstalovat a používat Solr clustery prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="a15b5-469">Install and use Solr on HDInsight clusters</span></span>](hdinsight-hadoop-solr-install-linux.md)
* [<span data-ttu-id="a15b5-470">Nainstalovat a používat Giraph clustery prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="a15b5-470">Install and use Giraph on HDInsight clusters</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="a15b5-471">Přidejte další úložiště do clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="a15b5-471">Add additional storage to an HDInsight cluster</span></span>](hdinsight-hadoop-add-storage.md)

<span data-ttu-id="a15b5-472">[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Fáze při vytváření clusteru"</span><span class="sxs-lookup"><span data-stu-id="a15b5-472">[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Stages during cluster creation"</span></span>
