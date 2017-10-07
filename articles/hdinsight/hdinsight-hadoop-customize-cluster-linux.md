---
title: "pomocí akcí skriptů - Azure clusterů HDInsight aaaCustomize | Microsoft Docs"
description: "Přidáte volitelné součásti, které na základě tooLinux HDInsight clustery pomocí akcí skriptů. Akce skriptů jsou skripty Bash, které lze použít toocustomize konfigurace clusteru hello nebo přidejte další služby a nástroje, jako je Hue, Solr nebo R."
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
ms.openlocfilehash: ff22680a8a50b21985f6941f1edaf1dcf863d13f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-action"></a><span data-ttu-id="9450b-104">Přizpůsobení clusterů HDInsight se systémem Linux pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="9450b-104">Customize Linux-based HDInsight clusters using Script Action</span></span>

<span data-ttu-id="9450b-105">HDInsight nabízí možnost konfigurace názvem **akce skriptu** vlastní skripty, které přizpůsobují hello clusteru, který spustí.</span><span class="sxs-lookup"><span data-stu-id="9450b-105">HDInsight provides a configuration option called **Script Action** that invokes custom scripts that customize hello cluster.</span></span> <span data-ttu-id="9450b-106">Tyto skripty jsou použité tooinstall další součásti a změňte nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9450b-106">These scripts are used tooinstall additional components and change configuration settings.</span></span> <span data-ttu-id="9450b-107">Akce skriptu lze během nebo po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-107">Script actions can be used during or after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9450b-108">Hello možnost toouse akcí skriptů v již spuštěného clusteru je dostupná pouze pro clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="9450b-108">hello ability toouse script actions on an already running cluster is only available for Linux-based HDInsight clusters.</span></span>
>
> <span data-ttu-id="9450b-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9450b-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="9450b-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="9450b-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="9450b-111">Skript akce, může být také publikované toohello Azure Marketplace jako aplikace HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9450b-111">Script actions can also be published toohello Azure Marketplace as an HDInsight application.</span></span> <span data-ttu-id="9450b-112">Některé příklady hello v tomto dokumentu ukazují, jak můžete instalovat aplikace HDInsight pomocí skriptu akce příkazů z prostředí PowerShell a hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="9450b-112">Some of hello examples in this document show how you can install an HDInsight application using script action commands from PowerShell and hello .NET SDK.</span></span> <span data-ttu-id="9450b-113">Další informace o aplikace HDInsight naleznete v tématu [HDInsight publikování aplikace do Azure Marketplace hello](hdinsight-apps-publish-applications.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-113">For more information on HDInsight applications, see [Publish HDInsight applications into hello Azure Marketplace](hdinsight-apps-publish-applications.md).</span></span>

## <a name="permissions"></a><span data-ttu-id="9450b-114">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="9450b-114">Permissions</span></span>

<span data-ttu-id="9450b-115">Pokud používáte cluster HDInsight připojený k doméně, jsou k dispozici dva Ambari oprávnění, které jsou požadovány při použití akce skriptu s clusterem hello:</span><span class="sxs-lookup"><span data-stu-id="9450b-115">If you are using a domain-joined HDInsight cluster, there are two Ambari permissions that are required when using script actions with hello cluster:</span></span>

* <span data-ttu-id="9450b-116">**AMBARI. Spustit\_vlastní\_příkaz**: role správce Ambari hello toto oprávnění má ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9450b-116">**AMBARI.RUN\_CUSTOM\_COMMAND**: hello Ambari Administrator role has this permission by default.</span></span>
* <span data-ttu-id="9450b-117">**CLUSTER. Spustit\_vlastní\_příkaz**: obě hello Správce clusteru HDInsight a Ambari správce toto oprávnění mají ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9450b-117">**CLUSTER.RUN\_CUSTOM\_COMMAND**: Both hello HDInsight Cluster Administrator and Ambari Administrator have this permission by default.</span></span>

<span data-ttu-id="9450b-118">Další informace o práci s oprávněními v doméně prostředí HDInsight najdete v tématu [Správa clusterů HDInsight připojený k doméně](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-118">For more information on working with permissions with domain-joined HDInsight, see [Manage domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>

## <a name="access-control"></a><span data-ttu-id="9450b-119">Řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="9450b-119">Access control</span></span>

<span data-ttu-id="9450b-120">Pokud si nejste hello správce nebo vlastníka předplatného Azure, váš účet musí mít alespoň **Přispěvatel** skupinu prostředků toohello přístupu, která obsahuje clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-120">If you are not hello administrator/owner of your Azure subscription, your account must have at least **Contributor** access toohello resource group that contains hello HDInsight cluster.</span></span>

<span data-ttu-id="9450b-121">Kromě toho při vytváření clusteru služby HDInsight, někdo s alespoň **Přispěvatel** přístup toohello předplatné musí již dříve zaregistrovali hello zprostředkovatele pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9450b-121">Additionally, if you are creating an HDInsight cluster, someone with at least **Contributor** access toohello Azure subscription must have previously registered hello provider for HDInsight.</span></span> <span data-ttu-id="9450b-122">Registrace zprostředkovatele se stane, když uživatel s předplatným toohello přístup Přispěvatel vytvoří prostředek pro hello poprvé v předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-122">Provider registration happens when a user with Contributor access toohello subscription creates a resource for hello first time on hello subscription.</span></span> <span data-ttu-id="9450b-123">Stejného výsledku lze i bez vytvoření prostředku dosáhnout [registrací poskytovatele prostřednictvím REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).</span><span class="sxs-lookup"><span data-stu-id="9450b-123">It can also be accomplished without creating a resource by [registering a provider using REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).</span></span>

<span data-ttu-id="9450b-124">Další informace o práci s správu přístupu najdete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="9450b-124">For more information on working with access management, see hello following documents:</span></span>

* [<span data-ttu-id="9450b-125">Začínáme se správou přístupu v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9450b-125">Get started with access management in hello Azure portal</span></span>](../active-directory/role-based-access-control-what-is.md)
* [<span data-ttu-id="9450b-126">Používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky</span><span class="sxs-lookup"><span data-stu-id="9450b-126">Use role assignments toomanage access tooyour Azure subscription resources</span></span>](../active-directory/role-based-access-control-configure.md)

## <a name="understanding-script-actions"></a><span data-ttu-id="9450b-127">Vysvětlení akcí skriptů</span><span class="sxs-lookup"><span data-stu-id="9450b-127">Understanding Script Actions</span></span>

<span data-ttu-id="9450b-128">Akce skriptu je jednoduše Bash skript, který zadáte identifikátorů URI a parametry.</span><span class="sxs-lookup"><span data-stu-id="9450b-128">A Script Action is simply a Bash script that you provide a URI to, and parameters for.</span></span> <span data-ttu-id="9450b-129">Hello skript se spustí na uzlech v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-129">hello script runs on nodes in hello HDInsight cluster.</span></span> <span data-ttu-id="9450b-130">Hello následují vlastnosti a funkce akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-130">hello following are characteristics and features of script actions.</span></span>

* <span data-ttu-id="9450b-131">Musí být uložen v identifikátoru URI, který je přístupný z clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-131">Must be stored on a URI that is accessible from hello HDInsight cluster.</span></span> <span data-ttu-id="9450b-132">Hello následují umístění možné úložiště:</span><span class="sxs-lookup"><span data-stu-id="9450b-132">hello following are possible storage locations:</span></span>

    * <span data-ttu-id="9450b-133">**Azure Data Lake Store** účet, který je přístupný hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9450b-133">An **Azure Data Lake Store** account that is accessible by hello HDInsight cluster.</span></span> <span data-ttu-id="9450b-134">Informace o používání Azure Data Lake Store s HDInsight naleznete v tématu [vytvoření clusteru HDInsight s Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-134">For information on using Azure Data Lake Store with HDInsight, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

        <span data-ttu-id="9450b-135">Při použití skriptu uložených v Data Lake Store, formát identifikátoru URI hello je `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.</span><span class="sxs-lookup"><span data-stu-id="9450b-135">When using a script stored in Data Lake Store, hello URI format is `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.</span></span>

        > [!NOTE]
        > <span data-ttu-id="9450b-136">Hello služby hlavní HDInsight používá tooaccess Data Lake Store, musí mít přístup pro čtení toohello skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-136">hello service principal HDInsight uses tooaccess Data Lake Store must have read access toohello script.</span></span>

    * <span data-ttu-id="9450b-137">Objekt blob v **účet úložiště Azure** který je buď účet hello primární nebo další úložiště pro hello HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="9450b-137">A blob in an **Azure Storage account** that is either hello primary or additional storage account for hello HDInsight cluster.</span></span> <span data-ttu-id="9450b-138">HDInsight se udělí přístup tooboth tyto typy účtů úložiště při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-138">HDInsight is granted access tooboth of these types of storage accounts during cluster creation.</span></span>

    * <span data-ttu-id="9450b-139">Soubor veřejného sdílení služby objektů Blob v Azure, Githubu, OneDrive, Dropbox, např.</span><span class="sxs-lookup"><span data-stu-id="9450b-139">A public file sharing service such as Azure Blob, GitHub, OneDrive, Dropbox, etc.</span></span>

        <span data-ttu-id="9450b-140">Například identifikátory URI, najdete v části hello [příklad skript akce skripty](#example-script-action-scripts) části.</span><span class="sxs-lookup"><span data-stu-id="9450b-140">For example URIs, see hello [Example script action scripts](#example-script-action-scripts) section.</span></span>

        > [!WARNING]
        > <span data-ttu-id="9450b-141">HDInsight podporuje pouze __pro obecné účely__ účty Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9450b-141">HDInsight only supports __General-purpose__ Azure Storage accounts.</span></span> <span data-ttu-id="9450b-142">Aktuálně nepodporuje hello __úložiště objektů Blob__ typ účtu.</span><span class="sxs-lookup"><span data-stu-id="9450b-142">It does not currently support hello __Blob storage__ account type.</span></span>

* <span data-ttu-id="9450b-143">Je možné omezit příliš**spustili pouze určité typy uzlů**, příklad hlavních uzlech nebo uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="9450b-143">Can be restricted too**run on only certain node types**, for example head nodes or worker nodes.</span></span>

  > [!NOTE]
  > <span data-ttu-id="9450b-144">Při použití s HDInsight Premium, můžete zadat, že hello skriptu by měl používat na hello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="9450b-144">When used with HDInsight Premium, you can specify that hello script should be used on hello edge node.</span></span>

* <span data-ttu-id="9450b-145">Může být **trvalé** nebo **ad hoc**.</span><span class="sxs-lookup"><span data-stu-id="9450b-145">Can be **persisted** or **ad hoc**.</span></span>

    <span data-ttu-id="9450b-146">**Trvalé** skriptů jsou použité tooworker uzly přidané toohello clusteru po spuštění skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-146">**Persisted** scripts are applied tooworker nodes added toohello cluster after hello script runs.</span></span> <span data-ttu-id="9450b-147">Například při rozšiřování prostředků clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-147">For example, when scaling up hello cluster.</span></span>

    <span data-ttu-id="9450b-148">Typ uzlu tooanother změny, jako je například hlavního uzlu může platit taky trvalého skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-148">A persisted script might also apply changes tooanother node type, such as a head node.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="9450b-149">Trvalé akce se skripty musí mít jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="9450b-149">Persisted script actions must have a unique name.</span></span>

    <span data-ttu-id="9450b-150">**Ad hoc** nejsou trvalé skripty.</span><span class="sxs-lookup"><span data-stu-id="9450b-150">**Ad hoc** scripts are not persisted.</span></span> <span data-ttu-id="9450b-151">Nejsou se použité tooworker uzly přidané toohello clusteru po má spustili hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-151">They are not applied tooworker nodes added toohello cluster after hello script has ran.</span></span> <span data-ttu-id="9450b-152">Následně můžete povýšit ad hoc skriptů tooa jako trvalý, skript nebo snížení úrovně skript ad hoc tooan trvalého skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-152">You can subsequently promote an ad hoc script tooa persisted script, or demote a persisted script tooan ad hoc script.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="9450b-153">Skript akce, které používají při vytváření clusteru jsou automaticky nastavené jako trvalé.</span><span class="sxs-lookup"><span data-stu-id="9450b-153">Script actions used during cluster creation are automatically persisted.</span></span>
  >
  > <span data-ttu-id="9450b-154">Skripty, které nejsou selhání jako trvalé, i když konkrétně znamenat, že by měl být.</span><span class="sxs-lookup"><span data-stu-id="9450b-154">Scripts that fail are not persisted, even if you specifically indicate that they should be.</span></span>

* <span data-ttu-id="9450b-155">Můžete přijmout **parametry** používané skriptem hello během provádění.</span><span class="sxs-lookup"><span data-stu-id="9450b-155">Can accept **parameters** that are used by hello script during execution.</span></span>
* <span data-ttu-id="9450b-156">Spustit s **kořenový úrovně oprávnění** na uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-156">Run with **root level privileges** on hello cluster nodes.</span></span>
* <span data-ttu-id="9450b-157">Je možné prostřednictvím hello **portál Azure**, **prostředí Azure PowerShell**, **rozhraní příkazového řádku Azure**, nebo **HDInsight .NET SDK**</span><span class="sxs-lookup"><span data-stu-id="9450b-157">Can be used through hello **Azure portal**, **Azure PowerShell**, **Azure CLI**, or **HDInsight .NET SDK**</span></span>

<span data-ttu-id="9450b-158">Hello clusteru uchovává historii všechny skripty, které mají byly spuštěny.</span><span class="sxs-lookup"><span data-stu-id="9450b-158">hello cluster keeps a history of all scripts that have been ran.</span></span> <span data-ttu-id="9450b-159">Historie Hello je užitečné, když potřebujete toofind hello ID skriptu pro operace povýšení nebo degradování.</span><span class="sxs-lookup"><span data-stu-id="9450b-159">hello history is useful when you need toofind hello ID of a script for promotion or demotion operations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9450b-160">Neexistuje žádný způsob, jak automatické tooundo hello změny provedené akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-160">There is no automatic way tooundo hello changes made by a script action.</span></span> <span data-ttu-id="9450b-161">Buď ručně reverse hello změny nebo zadejte skript, který je obrátí.</span><span class="sxs-lookup"><span data-stu-id="9450b-161">Either manually reverse hello changes or provide a script that reverses them.</span></span>


### <a name="script-action-in-hello-cluster-creation-process"></a><span data-ttu-id="9450b-162">Akce skriptu v procesu vytváření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="9450b-162">Script Action in hello cluster creation process</span></span>

<span data-ttu-id="9450b-163">Skript akce, které používají při vytváření clusteru se mírně liší od skript, který u stávajícího clusteru byla spuštěna akce:</span><span class="sxs-lookup"><span data-stu-id="9450b-163">Script Actions used during cluster creation are slightly different from script actions ran on an existing cluster:</span></span>

* <span data-ttu-id="9450b-164">je skript Hello **automaticky trvalou**.</span><span class="sxs-lookup"><span data-stu-id="9450b-164">hello script is **automatically persisted**.</span></span>
* <span data-ttu-id="9450b-165">A **selhání** v hello skript může způsobit toofail procesu vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-165">A **failure** in hello script can cause hello cluster creation process toofail.</span></span>

<span data-ttu-id="9450b-166">Hello následující diagram znázorňuje, pokud je během procesu vytváření hello provést akce skriptu:</span><span class="sxs-lookup"><span data-stu-id="9450b-166">hello following diagram illustrates when Script Action is executed during hello creation process:</span></span>

<span data-ttu-id="9450b-167">![Přizpůsobení cluster HDInsight a fáze při vytváření clusteru][img-hdi-cluster-states]</span><span class="sxs-lookup"><span data-stu-id="9450b-167">![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]</span></span>

<span data-ttu-id="9450b-168">Hello skript se spustí při HDInsight je konfigurován.</span><span class="sxs-lookup"><span data-stu-id="9450b-168">hello script runs while HDInsight is being configured.</span></span> <span data-ttu-id="9450b-169">V této fázi hello hello skript se spustí paralelně na všechny uzly zadané v hello clusteru a spustí s oprávněními kořenové na uzlech hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-169">At this stage, hello script runs in parallel on all hello specified nodes in hello cluster, and runs with root privileges on hello nodes.</span></span>

> [!NOTE]
> <span data-ttu-id="9450b-170">Protože hello skript se spustí s kořenové úrovně oprávnění na uzlech clusteru hello, můžete provádět operace, jako je spouštění a zastavování služeb, včetně služby související s Hadoop.</span><span class="sxs-lookup"><span data-stu-id="9450b-170">Because hello script runs with root level privilege on hello cluster nodes, you can perform operations like stopping and starting services, including Hadoop-related services.</span></span> <span data-ttu-id="9450b-171">Pokud zastavíte služby, je třeba zkontrolovat, že služba hello Ambari a další související s Hadoop služby jsou spuštěny před hello skriptu dokončení spuštění.</span><span class="sxs-lookup"><span data-stu-id="9450b-171">If you stop services, you must ensure that hello Ambari service and other Hadoop-related services are up and running before hello script finishes running.</span></span> <span data-ttu-id="9450b-172">Tyto služby jsou potřeba toosuccessfully určit hello stav a stav hello clusteru při jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="9450b-172">These services are required toosuccessfully determine hello health and state of hello cluster while it is being created.</span></span>


<span data-ttu-id="9450b-173">Při vytváření clusteru můžete použít různé akce skriptu najednou.</span><span class="sxs-lookup"><span data-stu-id="9450b-173">During cluster creation, you can use multiple script actions at once.</span></span> <span data-ttu-id="9450b-174">Tyto skripty jsou vyvolány v hello pořadí, ve kterém byly zadány.</span><span class="sxs-lookup"><span data-stu-id="9450b-174">These scripts are invoked in hello order in which they were specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9450b-175">Akce skriptu musí dokončit během 60 minut nebo vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="9450b-175">Script actions must complete within 60 minutes, or timeout.</span></span> <span data-ttu-id="9450b-176">Při zřizování clusteru hello skript spustí souběžně jiné procesy instalací a konfigurací.</span><span class="sxs-lookup"><span data-stu-id="9450b-176">During cluster provisioning, hello script runs concurrently with other setup and configuration processes.</span></span> <span data-ttu-id="9450b-177">Soutěž o zdroje, jako je například šířky pásma procesoru čas nebo v síti může způsobit hello skriptu tootake delší toofinish než ve vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="9450b-177">Competition for resources such as CPU time or network bandwidth may cause hello script tootake longer toofinish than it does in your development environment.</span></span>
>
> <span data-ttu-id="9450b-178">toominimize hello trvá toorun hello skriptu, když, vyhněte se úlohy, jako je stažení a kompilování aplikací ze zdroje.</span><span class="sxs-lookup"><span data-stu-id="9450b-178">toominimize hello time it takes toorun hello script, avoid tasks such as downloading and compiling applications from source.</span></span> <span data-ttu-id="9450b-179">Předem zkompilovat aplikací a ukládat hello binární ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9450b-179">Pre-compile applications and store hello binary in Azure Storage.</span></span>


### <a name="script-action-on-a-running-cluster"></a><span data-ttu-id="9450b-180">Akce skriptu na spuštěného clusteru</span><span class="sxs-lookup"><span data-stu-id="9450b-180">Script action on a running cluster</span></span>

<span data-ttu-id="9450b-181">Na rozdíl od skript, který byla spuštěna akce používají při vytváření clusteru, selhání ve skriptu na již spuštěnému clusteru automaticky nezpůsobí stav hello clusteru toochange tooa se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="9450b-181">Unlike script actions used during cluster creation, a failure in a script ran on an already running cluster does not automatically cause hello cluster toochange tooa failed state.</span></span> <span data-ttu-id="9450b-182">Po dokončení skriptu hello clusteru by měl vrátit tooa "spuštěný".</span><span class="sxs-lookup"><span data-stu-id="9450b-182">Once a script completes, hello cluster should return tooa "running" state.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9450b-183">I v případě, že má hello cluster spuštěném stavu., hello selhání skriptu mohou obsahovat nefunkční věcí.</span><span class="sxs-lookup"><span data-stu-id="9450b-183">Even if hello cluster has a 'running' state, hello failed script may have broken things.</span></span> <span data-ttu-id="9450b-184">Skript může například odstranit soubory potřebné hello cluster.</span><span class="sxs-lookup"><span data-stu-id="9450b-184">For example, a script could delete files needed by hello cluster.</span></span>
>
> <span data-ttu-id="9450b-185">Skripty akce spustit s oprávněními kořenové, ujistěte se, že chápete, skript se před každým jejím použitím tooyour clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-185">Scripts actions run with root privileges, so you should make sure that you understand what a script does before applying it tooyour cluster.</span></span>

<span data-ttu-id="9450b-186">Při použití skriptu tooa clusteru, změní stav clusteru hello toofrom **systémem** příliš**platných**, pak **HDInsight konfigurace**a nakonec zpět příliš**Systémem** pro úspěšné skripty.</span><span class="sxs-lookup"><span data-stu-id="9450b-186">When applying a script tooa cluster, hello cluster state changes toofrom **Running** too**Accepted**, then **HDInsight configuration**, and finally back too**Running** for successful scripts.</span></span> <span data-ttu-id="9450b-187">Stav skriptu Hello je zaznamenána v historii akcí skriptu hello a pomocí této toodetermine informace, zda skript hello byla úspěšná nebo neúspěšná.</span><span class="sxs-lookup"><span data-stu-id="9450b-187">hello script status is logged in hello script action history, and you can use this information toodetermine whether hello script succeeded or failed.</span></span> <span data-ttu-id="9450b-188">Například hello `Get-AzureRmHDInsightScriptActionHistory` rutiny prostředí PowerShell může být stav hello použité tooview skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-188">For example, hello `Get-AzureRmHDInsightScriptActionHistory` PowerShell cmdlet can be used tooview hello status of a script.</span></span> <span data-ttu-id="9450b-189">Vrátí informace podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="9450b-189">It returns information similar toohello following text:</span></span>

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!NOTE]
> <span data-ttu-id="9450b-190">Pokud jste změnili heslo uživatele (správce) hello clusteru po vytvoření clusteru hello, skript, který spustil akce pro tento cluster mohou selhat.</span><span class="sxs-lookup"><span data-stu-id="9450b-190">If you have changed hello cluster user (admin) password after hello cluster was created, script actions ran against this cluster may fail.</span></span> <span data-ttu-id="9450b-191">Pokud máte jakékoli trvalé akce se skripty této cílové uzly pracovního procesu, tyto skripty se pravděpodobně nezdaří při změně měřítka hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-191">If you have any persisted script actions that target worker nodes, these scripts may fail when you scale hello cluster.</span></span>

## <a name="example-script-action-scripts"></a><span data-ttu-id="9450b-192">Příklad akce skriptu skripty</span><span class="sxs-lookup"><span data-stu-id="9450b-192">Example Script Action scripts</span></span>

<span data-ttu-id="9450b-193">Skript akce skripty můžete použít hello následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="9450b-193">Script Action scripts can be used through hello following utilities:</span></span>

* <span data-ttu-id="9450b-194">portál Azure</span><span class="sxs-lookup"><span data-stu-id="9450b-194">Azure portal</span></span>
* <span data-ttu-id="9450b-195">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9450b-195">Azure PowerShell</span></span>
* <span data-ttu-id="9450b-196">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9450b-196">Azure CLI</span></span>
* <span data-ttu-id="9450b-197">Sada HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="9450b-197">HDInsight .NET SDK</span></span>

<span data-ttu-id="9450b-198">HDInsight poskytuje hello tooinstall skripty v clusterech HDInsight následující součásti:</span><span class="sxs-lookup"><span data-stu-id="9450b-198">HDInsight provides scripts tooinstall hello following components on HDInsight clusters:</span></span>

| <span data-ttu-id="9450b-199">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9450b-199">Name</span></span> | <span data-ttu-id="9450b-200">Skript</span><span class="sxs-lookup"><span data-stu-id="9450b-200">Script</span></span> |
| --- | --- |
| <span data-ttu-id="9450b-201">**Přidejte účet služby Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="9450b-201">**Add an Azure Storage account**</span></span> |<span data-ttu-id="9450b-202">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxaddstorageaccountv01/Add-Storage-Account-v01.SH. V tématu [clusteru HDInsight tooan přidat další úložiště](hdinsight-hadoop-add-storage.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-202">https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh. See [Add additional storage tooan HDInsight cluster](hdinsight-hadoop-add-storage.md).</span></span> |
| <span data-ttu-id="9450b-203">**Instalace aplikace Hue**</span><span class="sxs-lookup"><span data-stu-id="9450b-203">**Install Hue**</span></span> |<span data-ttu-id="9450b-204">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-uber-v02.SH. V tématu [instalace a použití clusterů v HDInsight Hue](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-204">https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. See [Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> |
| <span data-ttu-id="9450b-205">**Nainstalujte Presto**</span><span class="sxs-lookup"><span data-stu-id="9450b-205">**Install Presto**</span></span> |<span data-ttu-id="9450b-206">https://RAW.githubusercontent.com/hdinsight/Presto-hdinsight/Master/installpresto.SH. V tématu [instalace a použití clusterů v HDInsight Presto](hdinsight-hadoop-install-presto.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-206">https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh. See [Install and use Presto on HDInsight clusters](hdinsight-hadoop-install-presto.md).</span></span> |
| <span data-ttu-id="9450b-207">**Nainstalujte Solr**</span><span class="sxs-lookup"><span data-stu-id="9450b-207">**Install Solr**</span></span> |<span data-ttu-id="9450b-208">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsolrconfigactionv01/solr-Installer-v01.SH. V tématu [instalace a použití clusterů v HDInsight Solr](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-208">https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh. See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span> |
| <span data-ttu-id="9450b-209">**Nainstalujte Giraph**</span><span class="sxs-lookup"><span data-stu-id="9450b-209">**Install Giraph**</span></span> |<span data-ttu-id="9450b-210">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxgiraphconfigactionv01/giraph-Installer-v01.SH. V tématu [instalace a použití clusterů v HDInsight Giraph](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-210">https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh. See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> |
| <span data-ttu-id="9450b-211">**Předběžné načtení knihovny Hive**</span><span class="sxs-lookup"><span data-stu-id="9450b-211">**Pre-load Hive libraries**</span></span> |<span data-ttu-id="9450b-212">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsetupcustomhivelibsv01/Setup-customhivelibs-v01.SH. V tématu [přidat Hive knihovny v clusterech HDInsight](hdinsight-hadoop-add-hive-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-212">https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh. See [Add Hive libraries on HDInsight clusters](hdinsight-hadoop-add-hive-libraries.md).</span></span> |
| <span data-ttu-id="9450b-213">**Instalace nebo aktualizace Mono**</span><span class="sxs-lookup"><span data-stu-id="9450b-213">**Install or update Mono**</span></span> | <span data-ttu-id="9450b-214">https://hdiconfigactions.BLOB.Core.Windows.NET/Install-mono/Install-mono.bash.</span><span class="sxs-lookup"><span data-stu-id="9450b-214">https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash.</span></span> <span data-ttu-id="9450b-215">V tématu [instalace nebo aktualizace Mono v HDInsight](hdinsight-hadoop-install-mono.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-215">See [Install or update Mono on HDInsight](hdinsight-hadoop-install-mono.md).</span></span> |

## <a name="use-a-script-action-during-cluster-creation"></a><span data-ttu-id="9450b-216">Použití akce skriptu při vytváření clusteru</span><span class="sxs-lookup"><span data-stu-id="9450b-216">Use a Script Action during cluster creation</span></span>

<span data-ttu-id="9450b-217">Tato část obsahuje příklady o různých způsobech hello skriptových akcí můžete při vytváření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9450b-217">This section provides examples on hello different ways you can use script actions when creating an HDInsight cluster.</span></span>

### <a name="use-a-script-action-during-cluster-creation-from-hello-azure-portal"></a><span data-ttu-id="9450b-218">Použití akce skriptu při vytváření clusteru z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9450b-218">Use a Script Action during cluster creation from hello Azure portal</span></span>

1. <span data-ttu-id="9450b-219">Zahájení vytváření clusteru, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-219">Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="9450b-220">Zastavit při dosažení hello __clusteru Souhrn__ části.</span><span class="sxs-lookup"><span data-stu-id="9450b-220">Stop when you reach hello __Cluster summary__ section.</span></span>

2. <span data-ttu-id="9450b-221">Z hello __clusteru Souhrn__ části, vyberte hello __upravit__ propojení pro __upřesňující nastavení__.</span><span class="sxs-lookup"><span data-stu-id="9450b-221">From hello __Cluster summary__ section, select hello __edit__ link for __Advanced settings__.</span></span>

    ![Upřesnit nastavení odkaz](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. <span data-ttu-id="9450b-223">Z hello __upřesňující nastavení__ vyberte __skript akce__.</span><span class="sxs-lookup"><span data-stu-id="9450b-223">From hello __Advanced settings__ section, select __Script actions__.</span></span> <span data-ttu-id="9450b-224">Z hello __skript akce__ vyberte __+ nové odeslání__</span><span class="sxs-lookup"><span data-stu-id="9450b-224">From hello __Script actions__ section, select __+ Submit new__</span></span>

    ![Odeslat novou akci skriptu](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. <span data-ttu-id="9450b-226">Použití hello __vyberte skript__ položka tooselect předem vytvořené skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-226">Use hello __Select a script__ entry tooselect a pre-made script.</span></span> <span data-ttu-id="9450b-227">Vyberte toouse vlastní skript, __vlastní__ a pak zadejte hello __název__ a __Bash skriptu URI__ vašeho skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-227">toouse a custom script, select __Custom__ and then provide hello __Name__ and __Bash script URI__ for your script.</span></span>

    ![Skript přidáte tak v podobě vyberte skript hello](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    <span data-ttu-id="9450b-229">Hello následující tabulka popisuje elementy hello ve formuláři hello:</span><span class="sxs-lookup"><span data-stu-id="9450b-229">hello following table describes hello elements on hello form:</span></span>

    | <span data-ttu-id="9450b-230">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9450b-230">Property</span></span> | <span data-ttu-id="9450b-231">Hodnota</span><span class="sxs-lookup"><span data-stu-id="9450b-231">Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="9450b-232">Vyberte skript</span><span class="sxs-lookup"><span data-stu-id="9450b-232">Select a script</span></span> | <span data-ttu-id="9450b-233">toouse vlastního skriptu, vyberte __vlastní__.</span><span class="sxs-lookup"><span data-stu-id="9450b-233">toouse your own script, select __Custom__.</span></span> <span data-ttu-id="9450b-234">Vyberte jednu z hello k dispozici, jinak hodnota skripty.</span><span class="sxs-lookup"><span data-stu-id="9450b-234">Otherwise, select one of hello provided scripts.</span></span> |
    | <span data-ttu-id="9450b-235">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9450b-235">Name</span></span> |<span data-ttu-id="9450b-236">Zadejte název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-236">Specify a name for hello script action.</span></span> |
    | <span data-ttu-id="9450b-237">Skript bash identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="9450b-237">Bash script URI</span></span> |<span data-ttu-id="9450b-238">Zadejte hello URI toohello skript, který je vyvolaná toocustomize hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-238">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span> |
    | <span data-ttu-id="9450b-239">HEAD nebo Worker nebo Zookeeper</span><span class="sxs-lookup"><span data-stu-id="9450b-239">Head/Worker/Zookeeper</span></span> |<span data-ttu-id="9450b-240">Zadejte hello uzly (**Head**, **pracovní**, nebo **ZooKeeper**) na úpravy hello je skript spuštěn.</span><span class="sxs-lookup"><span data-stu-id="9450b-240">Specify hello nodes (**Head**, **Worker**, or **ZooKeeper**) on which hello customization script is run.</span></span> |
    | <span data-ttu-id="9450b-241">Parametry</span><span class="sxs-lookup"><span data-stu-id="9450b-241">Parameters</span></span> |<span data-ttu-id="9450b-242">Zadejte parametry hello, pokud to vyžaduje hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-242">Specify hello parameters, if required by hello script.</span></span> |

    <span data-ttu-id="9450b-243">Použití hello __zachovat tuto akci skriptu__ tooensure položku, která hello skriptu se použije během operace škálování.</span><span class="sxs-lookup"><span data-stu-id="9450b-243">Use hello __Persist this script action__ entry tooensure that hello script is applied during scaling operations.</span></span>

5. <span data-ttu-id="9450b-244">Vyberte __vytvořit__ toosave hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-244">Select __Create__ toosave hello script.</span></span> <span data-ttu-id="9450b-245">Pak můžete použít __+ odeslání nové__ tooadd další skript.</span><span class="sxs-lookup"><span data-stu-id="9450b-245">You can then use __+ Submit new__ tooadd another script.</span></span>

    ![Více akcí skriptů](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    <span data-ttu-id="9450b-247">Po dokončení přidávání skriptů, použijte hello __vyberte__ tlačítko a pak hello __Další__ tlačítko tooreturn toohello __clusteru Souhrn__ části.</span><span class="sxs-lookup"><span data-stu-id="9450b-247">When you are done adding scripts, use hello __Select__ button, and then hello __Next__ button tooreturn toohello __Cluster summary__ section.</span></span>

3. <span data-ttu-id="9450b-248">toocreate hello cluster, vyberte __vytvořit__ z hello __clusteru Souhrn__ výběr.</span><span class="sxs-lookup"><span data-stu-id="9450b-248">toocreate hello cluster, select __Create__ from hello __Cluster summary__ selection.</span></span>

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a><span data-ttu-id="9450b-249">Použití akce skriptu z šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="9450b-249">Use a Script Action from Azure Resource Manager templates</span></span>

<span data-ttu-id="9450b-250">Příklady Hello v této části ukazují, jak toouse skript akce pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9450b-250">hello examples in this section demonstrate how toouse script actions with Azure Resource Manager templates.</span></span>

#### <a name="before-you-begin"></a><span data-ttu-id="9450b-251">Než začnete</span><span class="sxs-lookup"><span data-stu-id="9450b-251">Before you begin</span></span>

* <span data-ttu-id="9450b-252">Informace o konfiguraci pracovní stanice toorun rutiny prostředí HDInsight Powershell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9450b-252">For information about configuring a workstation toorun HDInsight Powershell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="9450b-253">Pokyny najdete v části toocreate šablony [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-253">For instructions on how toocreate templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="9450b-254">Pokud jste ještě nepoužívali prostředí Azure PowerShell s Resource Managerem, přečtěte si téma [použití Azure Powershellu s Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-254">If you have not previously used Azure PowerShell with Resource Manager, see [Using Azure PowerShell with Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

#### <a name="create-clusters-using-script-action"></a><span data-ttu-id="9450b-255">Vytvořit clustery pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="9450b-255">Create clusters using Script Action</span></span>

1. <span data-ttu-id="9450b-256">Zkopírujte hello následující šablony tooa umístění ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9450b-256">Copy hello following template tooa location on your computer.</span></span> <span data-ttu-id="9450b-257">Tato šablona nainstaluje Giraph hello headnodes a pracovní uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-257">This template installs Giraph on hello headnodes and worker nodes in hello cluster.</span></span> <span data-ttu-id="9450b-258">Můžete také ověřit, zda šablona hello JSON je platný.</span><span class="sxs-lookup"><span data-stu-id="9450b-258">You can also verify if hello JSON template is valid.</span></span> <span data-ttu-id="9450b-259">Vložit obsah do šablony [JSONLint](http://jsonlint.com/), online ověření nástroj JSON.</span><span class="sxs-lookup"><span data-stu-id="9450b-259">Paste your template content into [JSONLint](http://jsonlint.com/), an online JSON validation tool.</span></span>

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
2. <span data-ttu-id="9450b-260">Otevřete prostředí Azure PowerShell a přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="9450b-260">Start Azure PowerShell and Log in tooyour Azure account.</span></span> <span data-ttu-id="9450b-261">Po zadání přihlašovacích údajů, hello příkaz vrátí informace o vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="9450b-261">After providing your credentials, hello command returns information about your account.</span></span>

        Add-AzureRmAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...
3. <span data-ttu-id="9450b-262">Pokud máte více předplatných, zadejte ID předplatného hello chcete toouse pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="9450b-262">If you have multiple subscriptions, provide hello subscription ID you wish toouse for deployment.</span></span>

        Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

    > [!NOTE]
    > <span data-ttu-id="9450b-263">Můžete použít `Get-AzureRmSubscription` tooget seznam Všechna předplatná spojená s vaším účtem, který zahrnuje hello ID odběru pro každé z nich.</span><span class="sxs-lookup"><span data-stu-id="9450b-263">You can use `Get-AzureRmSubscription` tooget a list of all subscriptions associated with your account, which includes hello subscription ID for each one.</span></span>

4. <span data-ttu-id="9450b-264">Pokud nemáte existující skupinu prostředků, vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="9450b-264">If you do not have an existing resource group, create a resource group.</span></span> <span data-ttu-id="9450b-265">Zadejte název hello hello skupinu prostředků a umístění, které potřebujete pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="9450b-265">Provide hello name of hello resource group and location that you need for your solution.</span></span> <span data-ttu-id="9450b-266">Se vrátí souhrn hello novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="9450b-266">A summary of hello new resource group is returned.</span></span>

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

5. <span data-ttu-id="9450b-267">toocreate nasazení skupiny prostředků, spusťte hello **New-AzureRmResourceGroupDeployment** příkaz a zadejte potřebné parametry hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-267">toocreate a deployment for your resource group, run hello **New-AzureRmResourceGroupDeployment** command and provide hello necessary parameters.</span></span> <span data-ttu-id="9450b-268">Parametry Hello patří hello následující data:</span><span class="sxs-lookup"><span data-stu-id="9450b-268">hello parameters include hello following data:</span></span>

    * <span data-ttu-id="9450b-269">Název pro vaše nasazení</span><span class="sxs-lookup"><span data-stu-id="9450b-269">A name for your deployment</span></span>
    * <span data-ttu-id="9450b-270">Hello název vaší skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="9450b-270">hello name of your resource group</span></span>
    * <span data-ttu-id="9450b-271">Hello cesta nebo adresa URL toohello šablonu, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9450b-271">hello path or URL toohello template you created.</span></span>

  <span data-ttu-id="9450b-272">Pokud vaše šablona vyžaduje žádné parametry, musíte zadat také tyto parametry.</span><span class="sxs-lookup"><span data-stu-id="9450b-272">If your template requires any parameters, you must pass those parameters as well.</span></span> <span data-ttu-id="9450b-273">V takovém případě hello skript akce tooinstall R na clusteru hello nevyžaduje žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="9450b-273">In this case, hello script action tooinstall R on hello cluster does not require any parameters.</span></span>

        New-AzureRmResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>

    <span data-ttu-id="9450b-274">Jste výzvami tooprovide hodnoty pro parametry hello definované v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-274">You are prompted tooprovide values for hello parameters defined in hello template.</span></span>

1. <span data-ttu-id="9450b-275">Pokud skupina prostředků hello nasazen, se zobrazí shrnutí nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-275">When hello resource group has been deployed, a summary of hello deployment is displayed.</span></span>

          DeploymentName    : mydeployment
          ResourceGroupName : myresourcegroup
          ProvisioningState : Succeeded
          Timestamp         : 8/14/2017 7:00:27 PM
          Mode              : Incremental
          ...

2. <span data-ttu-id="9450b-276">Pokud vaše nasazení selže, můžete použít následující rutiny tooget informace o selhání hello hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-276">If your deployment fails, you can use hello following cmdlets tooget information about hello failures.</span></span>

        Get-AzureRmResourceGroupDeployment -ResourceGroupName myresourcegroup -ProvisioningState Failed

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a><span data-ttu-id="9450b-277">Použití akce skriptu při vytváření clusteru z prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9450b-277">Use a Script Action during cluster creation from Azure PowerShell</span></span>

<span data-ttu-id="9450b-278">V této části používáme hello [přidat AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) rutiny tooinvoke skripty pomocí akce skriptu toocustomize cluster.</span><span class="sxs-lookup"><span data-stu-id="9450b-278">In this section, we use hello [Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) cmdlet tooinvoke scripts by using Script Action toocustomize a cluster.</span></span> <span data-ttu-id="9450b-279">Než budete pokračovat, ujistěte se, jste nainstalovali a nakonfigurovali Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9450b-279">Before proceeding, make sure you have installed and configured Azure PowerShell.</span></span> <span data-ttu-id="9450b-280">Informace o konfiguraci pracovní stanice toorun rutiny prostředí HDInsight PowerShell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9450b-280">For information about configuring a workstation toorun HDInsight PowerShell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="9450b-281">ukazuje, Hello následující skript jak tooapply akce skriptu při vytváření clusteru pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="9450b-281">hello following script demonstrates how tooapply a script action when creating a cluster using PowerShell:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

<span data-ttu-id="9450b-282">Může trvat několik minut, než se vytvoří hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-282">It can take several minutes before hello cluster is created.</span></span>

### <a name="use-a-script-action-during-cluster-creation-from-hello-hdinsight-net-sdk"></a><span data-ttu-id="9450b-283">Použití akce skriptu při vytváření clusteru z hello HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="9450b-283">Use a Script Action during cluster creation from hello HDInsight .NET SDK</span></span>

<span data-ttu-id="9450b-284">Hello HDInsight .NET SDK poskytuje klientské knihovny, které umožňuje snazší toowork s HDInsight z aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="9450b-284">hello HDInsight .NET SDK provides client libraries that makes it easier toowork with HDInsight from a .NET application.</span></span> <span data-ttu-id="9450b-285">Ukázka kódu, najdete v části [hello vytvořit systémem Linux clusterů v HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).</span><span class="sxs-lookup"><span data-stu-id="9450b-285">For a code sample, see [Create Linux-based clusters in HDInsight using hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).</span></span>

## <a name="apply-a-script-action-tooa-running-cluster"></a><span data-ttu-id="9450b-286">Použití akce skriptu tooa, spuštění clusteru</span><span class="sxs-lookup"><span data-stu-id="9450b-286">Apply a Script Action tooa running cluster</span></span>

<span data-ttu-id="9450b-287">V této části se dozvíte, jak tooapply skript akce tooa spuštění clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-287">In this section, learn how tooapply script actions tooa running cluster.</span></span>

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-portal"></a><span data-ttu-id="9450b-288">Použití akce skriptu tooa, spuštění clusteru z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9450b-288">Apply a Script Action tooa running cluster from hello Azure portal</span></span>

1. <span data-ttu-id="9450b-289">Z hello [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9450b-289">From hello [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span>

2. <span data-ttu-id="9450b-290">Přehled clusteru HDInsight hello, vyberte hello **akcí skriptů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="9450b-290">From hello HDInsight cluster overview, select hello **Script Actions** tile.</span></span>

    ![Dlaždice akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > <span data-ttu-id="9450b-292">Můžete také vybrat **všechna nastavení** a pak vyberte **akcí skriptů** z hello v oddílu nastavení.</span><span class="sxs-lookup"><span data-stu-id="9450b-292">You can also select **All settings** and then select **Script Actions** from hello Settings section.</span></span>

3. <span data-ttu-id="9450b-293">Hello horní části hello oddílu akce skriptu, vyberte **odeslání nové**.</span><span class="sxs-lookup"><span data-stu-id="9450b-293">From hello top of hello Script Actions section, select **Submit new**.</span></span>

    ![Přidat skript tooa, spuštění clusteru](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. <span data-ttu-id="9450b-295">Použití hello __vyberte skript__ položka tooselect předem vytvořené skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-295">Use hello __Select a script__ entry tooselect a pre-made script.</span></span> <span data-ttu-id="9450b-296">Vyberte toouse vlastní skript, __vlastní__ a pak zadejte hello __název__ a __Bash skriptu URI__ vašeho skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-296">toouse a custom script, select __Custom__ and then provide hello __Name__ and __Bash script URI__ for your script.</span></span>

    ![Skript přidáte tak v podobě vyberte skript hello](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    <span data-ttu-id="9450b-298">Hello následující tabulka popisuje elementy hello ve formuláři hello:</span><span class="sxs-lookup"><span data-stu-id="9450b-298">hello following table describes hello elements on hello form:</span></span>

    | <span data-ttu-id="9450b-299">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9450b-299">Property</span></span> | <span data-ttu-id="9450b-300">Hodnota</span><span class="sxs-lookup"><span data-stu-id="9450b-300">Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="9450b-301">Vyberte skript</span><span class="sxs-lookup"><span data-stu-id="9450b-301">Select a script</span></span> | <span data-ttu-id="9450b-302">toouse vlastního skriptu, vyberte __vlastní__.</span><span class="sxs-lookup"><span data-stu-id="9450b-302">toouse your own script, select __custom__.</span></span> <span data-ttu-id="9450b-303">Jinak vyberte možnost poskytnutého skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-303">Otherwise, select a provided script.</span></span> |
    | <span data-ttu-id="9450b-304">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9450b-304">Name</span></span> |<span data-ttu-id="9450b-305">Zadejte název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-305">Specify a name for hello script action.</span></span> |
    | <span data-ttu-id="9450b-306">Skript bash identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="9450b-306">Bash script URI</span></span> |<span data-ttu-id="9450b-307">Zadejte hello URI toohello skript, který je vyvolaná toocustomize hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-307">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span> |
    | <span data-ttu-id="9450b-308">HEAD nebo Worker nebo Zookeeper</span><span class="sxs-lookup"><span data-stu-id="9450b-308">Head/Worker/Zookeeper</span></span> |<span data-ttu-id="9450b-309">Zadejte hello uzly (**Head**, **pracovní**, nebo **ZooKeeper**) na úpravy hello je skript spuštěn.</span><span class="sxs-lookup"><span data-stu-id="9450b-309">Specify hello nodes (**Head**, **Worker**, or **ZooKeeper**) on which hello customization script is run.</span></span> |
    | <span data-ttu-id="9450b-310">Parametry</span><span class="sxs-lookup"><span data-stu-id="9450b-310">Parameters</span></span> |<span data-ttu-id="9450b-311">Zadejte parametry hello, pokud to vyžaduje hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-311">Specify hello parameters, if required by hello script.</span></span> |

    <span data-ttu-id="9450b-312">Použití hello __zachovat tuto akci skriptu__ položka toomake zda hello skriptu se použije během operace škálování.</span><span class="sxs-lookup"><span data-stu-id="9450b-312">Use hello __Persist this script action__ entry toomake sure hello script is applied during scaling operations.</span></span>

5. <span data-ttu-id="9450b-313">Nakonec použijte hello **vytvořit** tlačítko tooapply hello skriptu toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-313">Finally, use hello **Create** button tooapply hello script toohello cluster.</span></span>

### <a name="apply-a-script-action-tooa-running-cluster-from-azure-powershell"></a><span data-ttu-id="9450b-314">Použití akce skriptu tooa, spuštění clusteru z prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9450b-314">Apply a Script Action tooa running cluster from Azure PowerShell</span></span>

<span data-ttu-id="9450b-315">Než budete pokračovat, ujistěte se, jste nainstalovali a nakonfigurovali Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9450b-315">Before proceeding, make sure you have installed and configured Azure PowerShell.</span></span> <span data-ttu-id="9450b-316">Informace o konfiguraci pracovní stanice toorun rutiny prostředí HDInsight PowerShell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9450b-316">For information about configuring a workstation toorun HDInsight PowerShell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="9450b-317">Hello následující příklad ukazuje, jak tooapply clusteru spuštěná tooa akce skriptu:</span><span class="sxs-lookup"><span data-stu-id="9450b-317">hello following example demonstrates how tooapply a script action tooa running cluster:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

<span data-ttu-id="9450b-318">Po dokončení operace hello, zobrazí se informace podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="9450b-318">Once hello operation completes, you receive information similar toohello following text:</span></span>

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-cli"></a><span data-ttu-id="9450b-319">Použití akce skriptu tooa, spuštění clusteru z hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="9450b-319">Apply a Script Action tooa running cluster from hello Azure CLI</span></span>

<span data-ttu-id="9450b-320">Než budete pokračovat, ujistěte se, je nainstalován a nakonfigurován hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="9450b-320">Before proceeding, make sure you have installed and configured hello Azure CLI.</span></span> <span data-ttu-id="9450b-321">Další informace najdete v tématu [hello instalace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-321">For more information, see [Install hello Azure CLI](../cli-install-nodejs.md).</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. <span data-ttu-id="9450b-322">tooswitch tooAzure režimu Resource Manager, použijte následující příkaz v příkazovém řádku hello hello:</span><span class="sxs-lookup"><span data-stu-id="9450b-322">tooswitch tooAzure Resource Manager mode, use hello following command at hello command line:</span></span>

        azure config mode arm

2. <span data-ttu-id="9450b-323">Použijte hello následující tooauthenticate tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="9450b-323">Use hello following tooauthenticate tooyour Azure subscription.</span></span>

        azure login

3. <span data-ttu-id="9450b-324">Použijte následující příkaz tooapply tooa akce skriptu spuštění clusteru hello</span><span class="sxs-lookup"><span data-stu-id="9450b-324">Use hello following command tooapply a script action tooa running cluster</span></span>

        azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>

    <span data-ttu-id="9450b-325">Pokud nezadáte parametry pro tento příkaz, zobrazí se výzva pro ně.</span><span class="sxs-lookup"><span data-stu-id="9450b-325">If you omit parameters for this command, you are prompted for them.</span></span> <span data-ttu-id="9450b-326">Pokud hello skriptu zadejte s `-u` přijímá parametry, můžete je pomocí hello `-p` parametr.</span><span class="sxs-lookup"><span data-stu-id="9450b-326">If hello script you specify with `-u` accepts parameters, you can specify them using hello `-p` parameter.</span></span>

    <span data-ttu-id="9450b-327">Uzel platné typy jsou `headnode`, `workernode`, a `zookeeper`.</span><span class="sxs-lookup"><span data-stu-id="9450b-327">Valid node types are `headnode`, `workernode`, and `zookeeper`.</span></span> <span data-ttu-id="9450b-328">Pokud skript hello by měla být použitá toomultiple typy uzlů, určit typy hello oddělených ';'.</span><span class="sxs-lookup"><span data-stu-id="9450b-328">If hello script should be applied toomultiple node types, specify hello types separated by a ';'.</span></span> <span data-ttu-id="9450b-329">Například, `-n headnode;workernode`.</span><span class="sxs-lookup"><span data-stu-id="9450b-329">For example, `-n headnode;workernode`.</span></span>

    <span data-ttu-id="9450b-330">toopersist hello skript, přidejte hello `--persistOnSuccess`.</span><span class="sxs-lookup"><span data-stu-id="9450b-330">toopersist hello script, add hello `--persistOnSuccess`.</span></span> <span data-ttu-id="9450b-331">Vám může také zachovat skriptu hello později pomocí `azure hdinsight script-action persisted set`.</span><span class="sxs-lookup"><span data-stu-id="9450b-331">You can also persist hello script later by using `azure hdinsight script-action persisted set`.</span></span>

    <span data-ttu-id="9450b-332">Po dokončení úlohy hello, zobrazí se výstup podobný toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="9450b-332">Once hello job completes, you receive output similar toohello following text:</span></span>

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-tooa-running-cluster-using-rest-api"></a><span data-ttu-id="9450b-333">Použití akce skriptu tooa, spuštění clusteru pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="9450b-333">Apply a Script Action tooa running cluster using REST API</span></span>

<span data-ttu-id="9450b-334">V tématu [spuštění akcí skriptů v clusteru s podporou spuštěné](https://msdn.microsoft.com/library/azure/mt668441.aspx).</span><span class="sxs-lookup"><span data-stu-id="9450b-334">See [Run Script Actions on a running cluster](https://msdn.microsoft.com/library/azure/mt668441.aspx).</span></span>

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-hdinsight-net-sdk"></a><span data-ttu-id="9450b-335">Použití akce skriptu tooa, spuštění clusteru z hello HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="9450b-335">Apply a Script Action tooa running cluster from hello HDInsight .NET SDK</span></span>

<span data-ttu-id="9450b-336">Příklad použití hello .NET SDK tooapply skripty tooa clusteru, naleznete v části [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span><span class="sxs-lookup"><span data-stu-id="9450b-336">For an example of using hello .NET SDK tooapply scripts tooa cluster, see [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span></span>

## <a name="view-history-promote-and-demote-script-actions"></a><span data-ttu-id="9450b-337">Zobrazit historii, povýšení a snížení akcí skriptů</span><span class="sxs-lookup"><span data-stu-id="9450b-337">View history, promote, and demote Script Actions</span></span>

### <a name="using-hello-azure-portal"></a><span data-ttu-id="9450b-338">Pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9450b-338">Using hello Azure portal</span></span>

1. <span data-ttu-id="9450b-339">Z hello [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9450b-339">From hello [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span>

2. <span data-ttu-id="9450b-340">Přehled clusteru HDInsight hello, vyberte hello **akcí skriptů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="9450b-340">From hello HDInsight cluster overview, select hello **Script Actions** tile.</span></span>

    ![Dlaždice akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > <span data-ttu-id="9450b-342">Můžete také vybrat **všechna nastavení** a pak vyberte **akcí skriptů** z hello v oddílu nastavení.</span><span class="sxs-lookup"><span data-stu-id="9450b-342">You can also select **All settings** and then select **Script Actions** from hello Settings section.</span></span>

4. <span data-ttu-id="9450b-343">Historie skripty pro tento cluster se zobrazí na hello oddílu akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-343">A history of scripts for this cluster is displayed on hello Script Actions section.</span></span> <span data-ttu-id="9450b-344">Tyto informace zahrnují seznam trvalou skripty.</span><span class="sxs-lookup"><span data-stu-id="9450b-344">This information includes a list of persisted scripts.</span></span> <span data-ttu-id="9450b-345">V hello následující snímek obrazovky uvidíte, že hello Solr byl skript spustili na tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-345">In hello screenshot below, you can see that hello Solr script has been ran on this cluster.</span></span> <span data-ttu-id="9450b-346">snímek obrazovky Hello nezobrazuje žádné trvalé skripty.</span><span class="sxs-lookup"><span data-stu-id="9450b-346">hello screenshot does not show any persisted scripts.</span></span>

    ![Oddíl akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. <span data-ttu-id="9450b-348">Výběr skript z historie hello zobrazí hello oddíl vlastností pro tento skript.</span><span class="sxs-lookup"><span data-stu-id="9450b-348">Selecting a script from hello history displays hello Properties section for this script.</span></span> <span data-ttu-id="9450b-349">Z hello horní části obrazovky hello se můžete znovu spustit skript hello nebo povyšte ho.</span><span class="sxs-lookup"><span data-stu-id="9450b-349">From hello top of hello screen, you can rerun hello script or promote it.</span></span>

    ![Vlastnosti akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. <span data-ttu-id="9450b-351">Můžete taky hello **...**  toohello napravo od položky na hello akcí skriptů části tooperform akce.</span><span class="sxs-lookup"><span data-stu-id="9450b-351">You can also use hello **...** toohello right of entries on hello Script Actions section tooperform actions.</span></span>

    ![Skript akce... využití](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a><span data-ttu-id="9450b-353">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="9450b-353">Using Azure PowerShell</span></span>

| <span data-ttu-id="9450b-354">Použijte hello...</span><span class="sxs-lookup"><span data-stu-id="9450b-354">Use hello following...</span></span> | <span data-ttu-id="9450b-355">příliš...</span><span class="sxs-lookup"><span data-stu-id="9450b-355">too...</span></span> |
| --- | --- |
| <span data-ttu-id="9450b-356">Get-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="9450b-356">Get-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="9450b-357">Načíst informace o trvalé akce se skripty</span><span class="sxs-lookup"><span data-stu-id="9450b-357">Retrieve information on persisted script actions</span></span> |
| <span data-ttu-id="9450b-358">Get-AzureRmHDInsightScriptActionHistory</span><span class="sxs-lookup"><span data-stu-id="9450b-358">Get-AzureRmHDInsightScriptActionHistory</span></span> |<span data-ttu-id="9450b-359">Načtení historie skript akce, které jsou použity toohello clusteru nebo podrobnosti specifického skriptu</span><span class="sxs-lookup"><span data-stu-id="9450b-359">Retrieve a history of script actions applied toohello cluster, or details for a specific script</span></span> |
| <span data-ttu-id="9450b-360">Set-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="9450b-360">Set-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="9450b-361">Zvýší úroveň ad hoc tooa akce skriptu trvalé akce skriptu</span><span class="sxs-lookup"><span data-stu-id="9450b-361">Promotes an ad hoc script action tooa persisted script action</span></span> |
| <span data-ttu-id="9450b-362">Remove-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="9450b-362">Remove-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="9450b-363">Sníží úroveň akci ad hoc tooan akcí trvalého skriptu</span><span class="sxs-lookup"><span data-stu-id="9450b-363">Demotes a persisted script action tooan ad hoc action</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="9450b-364">Pomocí `Remove-AzureRmHDInsightPersistedScriptAction` nezruší hello akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-364">Using `Remove-AzureRmHDInsightPersistedScriptAction` does not undo hello actions performed by a script.</span></span> <span data-ttu-id="9450b-365">Tato rutina odebere pouze trvalou příznak hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-365">This cmdlet only removes hello persisted flag.</span></span>

<span data-ttu-id="9450b-366">ukazuje, jak pomocí rutin toopromote hello Hello následující ukázkový skript a potom snížení úrovně skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-366">hello following example script demonstrates using hello cmdlets toopromote, then demote a script.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="using-hello-azure-cli"></a><span data-ttu-id="9450b-367">Pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="9450b-367">Using hello Azure CLI</span></span>

| <span data-ttu-id="9450b-368">Použijte hello...</span><span class="sxs-lookup"><span data-stu-id="9450b-368">Use hello following...</span></span> | <span data-ttu-id="9450b-369">příliš...</span><span class="sxs-lookup"><span data-stu-id="9450b-369">too...</span></span> |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |<span data-ttu-id="9450b-370">Načtení seznamu akcí trvalého skriptu</span><span class="sxs-lookup"><span data-stu-id="9450b-370">Retrieve a list of persisted script actions</span></span> |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |<span data-ttu-id="9450b-371">Načíst informace o konkrétní trvalého skriptu akce</span><span class="sxs-lookup"><span data-stu-id="9450b-371">Retrieve information on a specific persisted script action</span></span> |
| `azure hdinsight script-action history list <clustername>` |<span data-ttu-id="9450b-372">Načtení historie skript akce, které jsou použity toohello clusteru</span><span class="sxs-lookup"><span data-stu-id="9450b-372">Retrieve a history of script actions applied toohello cluster</span></span> |
| `azure hdinsight script-action history show <clustername> <scriptname>` |<span data-ttu-id="9450b-373">Načíst informace o konkrétní skript akce</span><span class="sxs-lookup"><span data-stu-id="9450b-373">Retrieve information on a specific script action</span></span> |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |<span data-ttu-id="9450b-374">Zvýší úroveň ad hoc tooa akce skriptu trvalé akce skriptu</span><span class="sxs-lookup"><span data-stu-id="9450b-374">Promotes an ad hoc script action tooa persisted script action</span></span> |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |<span data-ttu-id="9450b-375">Sníží úroveň akci ad hoc tooan akcí trvalého skriptu</span><span class="sxs-lookup"><span data-stu-id="9450b-375">Demotes a persisted script action tooan ad hoc action</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="9450b-376">Pomocí `azure hdinsight script-action persisted delete` nezruší hello akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-376">Using `azure hdinsight script-action persisted delete` does not undo hello actions performed by a script.</span></span> <span data-ttu-id="9450b-377">Tato rutina odebere pouze trvalou příznak hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-377">This cmdlet only removes hello persisted flag.</span></span>

### <a name="using-hello-hdinsight-net-sdk"></a><span data-ttu-id="9450b-378">Pomocí hello HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="9450b-378">Using hello HDInsight .NET SDK</span></span>

<span data-ttu-id="9450b-379">Příklad použití hello .NET SDK tooretrieve skriptu historie z clusteru, zvýšení úrovně nebo snížení úrovně skriptů najdete v tématu [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span><span class="sxs-lookup"><span data-stu-id="9450b-379">For an example of using hello .NET SDK tooretrieve script history from a cluster, promote or demote scripts, see [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span></span>

> [!NOTE]
> <span data-ttu-id="9450b-380">Tento příklad také ukazuje, jak hello tooinstall aplikace HDInsight pomocí sady .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="9450b-380">This example also demonstrates how tooinstall an HDInsight application using hello .NET SDK.</span></span>

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a><span data-ttu-id="9450b-381">Podpora pro open-source softwaru použít na clustery HDInsight</span><span class="sxs-lookup"><span data-stu-id="9450b-381">Support for open-source software used on HDInsight clusters</span></span>

<span data-ttu-id="9450b-382">Hello služby Microsoft Azure HDInsight používá prostředí technologie open source vytvořen kolem Hadoop.</span><span class="sxs-lookup"><span data-stu-id="9450b-382">hello Microsoft Azure HDInsight service uses an ecosystem of open-source technologies formed around Hadoop.</span></span> <span data-ttu-id="9450b-383">Microsoft Azure poskytuje obecné úroveň podpory pro technologie open source.</span><span class="sxs-lookup"><span data-stu-id="9450b-383">Microsoft Azure provides a general level of support for open-source technologies.</span></span> <span data-ttu-id="9450b-384">Další informace najdete v tématu hello **podporu oboru** části hello [nejčastější dotazy týkající se podpory Azure web](https://azure.microsoft.com/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="9450b-384">For more information, see hello **Support Scope** section of hello [Azure Support FAQ website](https://azure.microsoft.com/support/faq/).</span></span> <span data-ttu-id="9450b-385">Hello služba HDInsight poskytuje další úroveň podpory pro integrované komponenty.</span><span class="sxs-lookup"><span data-stu-id="9450b-385">hello HDInsight service provides an additional level of support for built-in components.</span></span>

<span data-ttu-id="9450b-386">Existují dva typy open-source komponent, které jsou k dispozici v hello služby HDInsight:</span><span class="sxs-lookup"><span data-stu-id="9450b-386">There are two types of open-source components that are available in hello HDInsight service:</span></span>

* <span data-ttu-id="9450b-387">**Integrované komponenty** -tyto komponenty jsou předinstalované na clustery HDInsight a poskytují základní funkce služby hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-387">**Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of hello cluster.</span></span> <span data-ttu-id="9450b-388">Například YARN ResourceManager, hello Hive dotazovací jazyk (HiveQL) a knihovna Mahout hello patří toothis kategorie.</span><span class="sxs-lookup"><span data-stu-id="9450b-388">For example, YARN ResourceManager, hello Hive query language (HiveQL), and hello Mahout library belong toothis category.</span></span> <span data-ttu-id="9450b-389">Úplný seznam součástí clusteru je k dispozici v [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-389">A full list of cluster components is available in [What's new in hello Hadoop cluster versions provided by HDInsight](hdinsight-component-versioning.md).</span></span>
* <span data-ttu-id="9450b-390">**Vlastní komponenty** -, jako uživatel hello clusteru, můžete nainstalovat nebo použít v vaše úlohy žádné součásti k dispozici v komunitě hello nebo vytvořené vámi.</span><span class="sxs-lookup"><span data-stu-id="9450b-390">**Custom components** - You, as a user of hello cluster, can install or use in your workload any component available in hello community or created by you.</span></span>

> [!WARNING]
> <span data-ttu-id="9450b-391">Součásti, které jsou součástí clusteru HDInsight hello jsou plně podporovány.</span><span class="sxs-lookup"><span data-stu-id="9450b-391">Components provided with hello HDInsight cluster are fully supported.</span></span> <span data-ttu-id="9450b-392">Microsoft Support pomáhá tooisolate a vyřešit problémy související toothese součásti.</span><span class="sxs-lookup"><span data-stu-id="9450b-392">Microsoft Support helps tooisolate and resolve issues related toothese components.</span></span>
>
> <span data-ttu-id="9450b-393">Vlastní komponenty přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-393">Custom components receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="9450b-394">Podporu společnosti Microsoft může být schopný tooresolve hello problém nebo může vás vyzvou tooengage dostupné kanály pro technologiích s otevřeným zdrojem hello kterých se nachází hluboké znalosti pro tuto technologii.</span><span class="sxs-lookup"><span data-stu-id="9450b-394">Microsoft support may be able tooresolve hello issue OR they may ask you tooengage available channels for hello open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="9450b-395">Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="9450b-395">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>

<span data-ttu-id="9450b-396">Hello služba HDInsight poskytuje několik způsobů toouse vlastní součásti.</span><span class="sxs-lookup"><span data-stu-id="9450b-396">hello HDInsight service provides several ways toouse custom components.</span></span> <span data-ttu-id="9450b-397">Dobrý den, které se vztahuje stejnou úroveň podpory, bez ohledu na to, jak je součást použít nebo nainstalovat na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-397">hello same level of support applies, regardless of how a component is used or installed on hello cluster.</span></span> <span data-ttu-id="9450b-398">Hello následující seznam popisuje hello nejběžnější způsoby, jak je možné vlastní komponenty v clusterech HDInsight:</span><span class="sxs-lookup"><span data-stu-id="9450b-398">hello following list describes hello most common ways that custom components can be used on HDInsight clusters:</span></span>

1. <span data-ttu-id="9450b-399">Úloha odeslání - Hadoop nebo jiné typy úloh, které spustit nebo používat vlastní komponenty může být odeslaná toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-399">Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted toohello cluster.</span></span>

2. <span data-ttu-id="9450b-400">Přizpůsobení clusteru – při vytváření clusteru, můžete zadat další nastavení a vlastní součásti, které jsou nainstalovány na uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-400">Cluster customization - During cluster creation, you can specify additional settings and custom components that are installed on hello cluster nodes.</span></span>

3. <span data-ttu-id="9450b-401">Ukázky - oblíbených vlastní součásti, Microsoft a ostatní mohou poskytnout ukázky použití těchto součástí v clusterech HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-401">Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on hello HDInsight clusters.</span></span> <span data-ttu-id="9450b-402">Tyto soubory jsou uvedeny bez podpory.</span><span class="sxs-lookup"><span data-stu-id="9450b-402">These samples are provided without support.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="9450b-403">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="9450b-403">Troubleshooting</span></span>

<span data-ttu-id="9450b-404">Můžete použít Ambari webového uživatelského rozhraní tooview informací zaznamenaných akcí skriptů.</span><span class="sxs-lookup"><span data-stu-id="9450b-404">You can use Ambari web UI tooview information logged by script actions.</span></span> <span data-ttu-id="9450b-405">Pokud skript hello selže při vytváření clusteru, hello protokoly jsou také k dispozici v hello výchozí účet úložiště přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-405">If hello script fails during cluster creation, hello logs are also available in hello default storage account associated with hello cluster.</span></span> <span data-ttu-id="9450b-406">Tato část obsahuje informace o tom, jak tooretrieve hello protokolů pomocí obou těchto možností.</span><span class="sxs-lookup"><span data-stu-id="9450b-406">This section provides information on how tooretrieve hello logs using both these options.</span></span>

### <a name="using-hello-ambari-web-ui"></a><span data-ttu-id="9450b-407">Pomocí hello webové uživatelské rozhraní Ambari</span><span class="sxs-lookup"><span data-stu-id="9450b-407">Using hello Ambari Web UI</span></span>

1. <span data-ttu-id="9450b-408">V prohlížeči přejděte toohttps://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="9450b-408">In your browser, navigate toohttps://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="9450b-409">Nahraďte název clusteru s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9450b-409">Replace CLUSTERNAME with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="9450b-410">Po zobrazení výzvy zadejte název účtu správce hello (správce) a heslo pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="9450b-410">When prompted, enter hello admin account name (admin) and password for hello cluster.</span></span> <span data-ttu-id="9450b-411">Přihlašovací údaje správce hello tooreenter může mít v webového formuláře.</span><span class="sxs-lookup"><span data-stu-id="9450b-411">You may have tooreenter hello admin credentials in a web form.</span></span>

2. <span data-ttu-id="9450b-412">Z panelu hello hello horní části stránky hello, vyberte hello **ops** položku.</span><span class="sxs-lookup"><span data-stu-id="9450b-412">From hello bar at hello top of hello page, select hello **ops** entry.</span></span> <span data-ttu-id="9450b-413">Zobrazí se seznam aktuální a předchozí operace provedené na clusteru hello prostřednictvím Ambari.</span><span class="sxs-lookup"><span data-stu-id="9450b-413">A list of current and previous operations performed on hello cluster through Ambari is displayed.</span></span>

    ![Panel uživatelského rozhraní Ambari web s ops vybrané](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. <span data-ttu-id="9450b-415">Najít hello položky, které mají **spustit\_customscriptaction** v hello **Operations** sloupce.</span><span class="sxs-lookup"><span data-stu-id="9450b-415">Find hello entries that have **run\_customscriptaction** in hello **Operations** column.</span></span> <span data-ttu-id="9450b-416">Tyto položky se vytvoří při spuštění akcí skriptů hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-416">These entries are created when hello Script Actions run.</span></span>

    ![Snímek obrazovky operací](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    <span data-ttu-id="9450b-418">tooview hello STDOUT a STDERR výstup, vyberte položku run\customscriptaction hello a procházení hello odkazy.</span><span class="sxs-lookup"><span data-stu-id="9450b-418">tooview hello STDOUT and STDERR output, select hello run\customscriptaction entry and drill down through hello links.</span></span> <span data-ttu-id="9450b-419">Tento výstup se vygeneruje, když hello skript spustí a můžou obsahovat užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="9450b-419">This output is generated when hello script runs, and may contain useful information.</span></span>

### <a name="access-logs-from-hello-default-storage-account"></a><span data-ttu-id="9450b-420">Přístup k protokolům z hello výchozí účet úložiště</span><span class="sxs-lookup"><span data-stu-id="9450b-420">Access logs from hello default storage account</span></span>

<span data-ttu-id="9450b-421">Pokud se vytvoření clusteru hello nezdaří z důvodu tooa skript akce chyba, hello protokoly jsou přístupné z účtu úložiště clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-421">If hello cluster creation fails due tooa script action error, hello logs can be accessed from hello cluster storage account.</span></span>

* <span data-ttu-id="9450b-422">Hello protokol úložiště jsou k dispozici v `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.</span><span class="sxs-lookup"><span data-stu-id="9450b-422">hello storage logs are available at `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.</span></span>

    ![Snímek obrazovky operací](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    <span data-ttu-id="9450b-424">V tomto adresáři hello protokoly jsou headnode, workernode a uzly zookeeper uspořádány samostatně.</span><span class="sxs-lookup"><span data-stu-id="9450b-424">Under this directory, hello logs are organized separately for headnode, workernode, and zookeeper nodes.</span></span> <span data-ttu-id="9450b-425">Tady je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="9450b-425">Some examples are:</span></span>

    * <span data-ttu-id="9450b-426">**Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="9450b-426">**Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`</span></span>

    * <span data-ttu-id="9450b-427">**Pracovního uzlu** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="9450b-427">**Worker node** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`</span></span>

    * <span data-ttu-id="9450b-428">**Zookeeper uzlu** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="9450b-428">**Zookeeper node** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`</span></span>

* <span data-ttu-id="9450b-429">Všechny stdout a stderr hello odpovídající hostitele je nahrán toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="9450b-429">All stdout and stderr of hello corresponding host is uploaded toohello storage account.</span></span> <span data-ttu-id="9450b-430">Existuje **výstup -\*.txt** a **chyby -\*.txt** pro jednotlivé akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-430">There is one **output-\*.txt** and **errors-\*.txt** for each script action.</span></span> <span data-ttu-id="9450b-431">Hello *.txt výstupní soubor obsahuje informace o hello URI hello skriptu, který nebyl spuštěn na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-431">hello output-*.txt file contains information about hello URI of hello script that got run on hello host.</span></span> <span data-ttu-id="9450b-432">Například</span><span class="sxs-lookup"><span data-stu-id="9450b-432">For example</span></span>

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* <span data-ttu-id="9450b-433">Je možné, opakovaně vytvořit cluster akce skriptu s hello stejný název.</span><span class="sxs-lookup"><span data-stu-id="9450b-433">It's possible that you repeatedly create a script action cluster with hello same name.</span></span> <span data-ttu-id="9450b-434">V takovém případě můžete odlišit hello relevantní protokoly na základě názvu složky datum hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-434">In such case, you can distinguish hello relevant logs based on hello DATE folder name.</span></span> <span data-ttu-id="9450b-435">Struktura složek hello pro cluster s podporou (mycluster) vytvořen na různá data se například zobrazí podobné toohello následující položky protokolu:</span><span class="sxs-lookup"><span data-stu-id="9450b-435">For example, hello folder structure for a cluster (mycluster) created on different dates appears similar toohello following log entries:</span></span>

    <span data-ttu-id="9450b-436">`\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`</span><span class="sxs-lookup"><span data-stu-id="9450b-436">`\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`</span></span>

* <span data-ttu-id="9450b-437">Pokud vytvoříte cluster akce skriptu s hello stejný název v hello stejný den, můžete použít hello jedinečnou předponu tooidentify hello příslušných protokolových souborů.</span><span class="sxs-lookup"><span data-stu-id="9450b-437">If you create a script action cluster with hello same name on hello same day, you can use hello unique prefix tooidentify hello relevant log files.</span></span>

* <span data-ttu-id="9450b-438">Pokud vytvoříte cluster s podporou téměř 12:00 AM (půlnoc), je možné, že soubory protokolu hello rozprostřít do dvou dnů.</span><span class="sxs-lookup"><span data-stu-id="9450b-438">If you create a cluster near 12:00AM (midnight), it's possible that hello log files span across two days.</span></span> <span data-ttu-id="9450b-439">V takových případech uvidíte dvě složky na jiné datum pro hello stejného clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-439">In such cases, you see two different date folders for hello same cluster.</span></span>

* <span data-ttu-id="9450b-440">Odesílání protokolů soubory toohello výchozí kontejner může trvat až minut too5, zejména u velkých clusterech.</span><span class="sxs-lookup"><span data-stu-id="9450b-440">Uploading log files toohello default container can take up too5 mins, especially for large clusters.</span></span> <span data-ttu-id="9450b-441">Ano Pokud chcete tooaccess hello protokoly, nedoporučuje se mazat okamžitě hello clusteru neúspěšné provedení akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-441">So, if you want tooaccess hello logs, you should not immediately delete hello cluster if a script action fails.</span></span>

### <a name="ambari-watchdog"></a><span data-ttu-id="9450b-442">Ambari sledovacího zařízení</span><span class="sxs-lookup"><span data-stu-id="9450b-442">Ambari watchdog</span></span>

> [!WARNING]
> <span data-ttu-id="9450b-443">Neměňte heslo hello hello Ambari sledovací zařízení (hdinsightwatchdog) v clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="9450b-443">Do not change hello password for hello Ambari Watchdog (hdinsightwatchdog) on your Linux-based HDInsight cluster.</span></span> <span data-ttu-id="9450b-444">Změna hello heslo pro tento účet dělí hello možnost toorun nové akcí skriptů v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-444">Changing hello password for this account breaks hello ability toorun new script actions on hello HDInsight cluster.</span></span>

### <a name="cant-import-name-blobservice"></a><span data-ttu-id="9450b-445">Nelze importovat název BlobService</span><span class="sxs-lookup"><span data-stu-id="9450b-445">Can't import name BlobService</span></span>

<span data-ttu-id="9450b-446">__Příznaky__: hello selhání akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-446">__Symptoms__: hello script action fails.</span></span> <span data-ttu-id="9450b-447">Při zobrazení hello operaci Ambari, zobrazí se text podobné toohello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="9450b-447">Text similar toohello following error is displayed when you view hello operation in Ambari:</span></span>

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

<span data-ttu-id="9450b-448">__Příčina__: k této chybě dojde, pokud upgradujete klienta hello Python Azure Storage, který je součástí clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="9450b-448">__Cause__: This error occurs if you upgrade hello Python Azure Storage client that is included with hello HDInsight cluster.</span></span> <span data-ttu-id="9450b-449">HDInsight očekává klienta Azure Storage 0.20.0.</span><span class="sxs-lookup"><span data-stu-id="9450b-449">HDInsight expects Azure Storage client 0.20.0.</span></span>

<span data-ttu-id="9450b-450">__Řešení__: tooresolve tato chyba, ručně připojte pomocí uzlu clusteru tooeach `ssh` a hello použijte následující příkaz tooreinstall hello verze klienta pro správné úložiště:</span><span class="sxs-lookup"><span data-stu-id="9450b-450">__Resolution__: tooresolve this error, manually connect tooeach cluster node using `ssh` and use hello following command tooreinstall hello correct storage client version:</span></span>

```
sudo pip install azure-storage==0.20.0
```

<span data-ttu-id="9450b-451">Informace o připojování toohello clusteru pomocí protokolu SSH naleznete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="9450b-451">For information on connecting toohello cluster with SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a><span data-ttu-id="9450b-452">Historie nezobrazí skripty použité při vytváření clusteru</span><span class="sxs-lookup"><span data-stu-id="9450b-452">History doesn't show scripts used during cluster creation</span></span>

<span data-ttu-id="9450b-453">Pokud váš cluster byl vytvořen ještě před 15. března 2016, nemusíte vidět položku v historii akcí skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-453">If your cluster was created before March 15, 2016, you may not see an entry in Script Action history.</span></span> <span data-ttu-id="9450b-454">Změníte-li velikost clusteru hello po 15. března 2016, hello skriptů pomocí při vytváření clusteru se zobrazí v historii jako uplatňují se, že toonew uzly v clusteru hello jako součást hello změnit velikost operace.</span><span class="sxs-lookup"><span data-stu-id="9450b-454">If you resize hello cluster after March 15, 2016, hello scripts using during cluster creation appear in history as they are applied toonew nodes in hello cluster as part of hello resize operation.</span></span>

<span data-ttu-id="9450b-455">Existují dvě výjimky:</span><span class="sxs-lookup"><span data-stu-id="9450b-455">There are two exceptions:</span></span>

* <span data-ttu-id="9450b-456">Pokud je váš cluster vytvořený před 1. září 2015.</span><span class="sxs-lookup"><span data-stu-id="9450b-456">If your cluster was created before September 1, 2015.</span></span> <span data-ttu-id="9450b-457">Toto datum je při zavedených akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="9450b-457">This date is when Script Actions were introduced.</span></span> <span data-ttu-id="9450b-458">Žádného clusteru vytvořené před tímto datem nelze použít akcí skriptů pro vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="9450b-458">Any cluster created before this date could not have used Script Actions for cluster creation.</span></span>

* <span data-ttu-id="9450b-459">Pokud používají různé akce skriptu při vytváření clusteru a používá stejný název pro několik skriptů nebo hello hello stejný název, stejným identifikátorem URI, ale různé parametry pro několik skriptů.</span><span class="sxs-lookup"><span data-stu-id="9450b-459">If you used multiple Script Actions during cluster creation, and used hello same name for multiple scripts, or hello same name, same URI, but different parameters for multiple scripts.</span></span> <span data-ttu-id="9450b-460">V těchto případech se zobrazí hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="9450b-460">In these cases, you receive hello following error:</span></span>

    <span data-ttu-id="9450b-461">Žádné nový skript, který může být akce byla spuštěna na tomto clusteru z důvodu tooconflicting skriptu názvy v existující skripty.</span><span class="sxs-lookup"><span data-stu-id="9450b-461">No new script actions can be ran on this cluster due tooconflicting script names in existing scripts.</span></span> <span data-ttu-id="9450b-462">Vytvoření skriptu názvy uvedených v clusteru musí být všechny jedinečné.</span><span class="sxs-lookup"><span data-stu-id="9450b-462">Script names provided at cluster create must be all unique.</span></span> <span data-ttu-id="9450b-463">Existující skripty jsou spuštěné v změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="9450b-463">Existing scripts are ran on resize.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9450b-464">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9450b-464">Next steps</span></span>

* [<span data-ttu-id="9450b-465">Vývoj skriptů akce skriptu pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="9450b-465">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions-linux.md)
* [<span data-ttu-id="9450b-466">Nainstalovat a používat Solr clustery prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="9450b-466">Install and use Solr on HDInsight clusters</span></span>](hdinsight-hadoop-solr-install-linux.md)
* [<span data-ttu-id="9450b-467">Nainstalovat a používat Giraph clustery prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="9450b-467">Install and use Giraph on HDInsight clusters</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="9450b-468">Přidejte další úložiště clusteru HDInsight tooan</span><span class="sxs-lookup"><span data-stu-id="9450b-468">Add additional storage tooan HDInsight cluster</span></span>](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Fáze při vytváření clusteru"
