---
title: "Správa clusterů HDInsight připojený k doméně - Azure | Microsoft Docs"
description: "Zjistěte, jak Správa clusterů HDInsight připojený k doméně"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 9b56ce6cc5bdd3b8d48d047751e978cad08598e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a><span data-ttu-id="febb4-103">Správa clusterů HDInsight připojený k doméně (Preview)</span><span class="sxs-lookup"><span data-stu-id="febb4-103">Manage Domain-joined HDInsight clusters (Preview)</span></span>
<span data-ttu-id="febb4-104">Další uživatelé a role v doméně HDInsight a Správa clusterů HDInsight připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="febb4-104">Learn the users and the roles in Domain-joined HDInsight, and how to manage domain-joined HDInsight clusters.</span></span>

## <a name="users-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="febb4-105">Uživatelé clustery HDInsight připojený k doméně</span><span class="sxs-lookup"><span data-stu-id="febb4-105">Users of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="febb4-106">Cluster služby HDInsight, který není připojený k doméně má dva uživatelské účty, které jsou vytvořeny při vytvoření clusteru:</span><span class="sxs-lookup"><span data-stu-id="febb4-106">An HDInsight cluster that is not domain-joined has two user accounts that are created during the cluster creation:</span></span>

* <span data-ttu-id="febb4-107">**Správce Ambari**: Tento účet je také označován jako *Hadoop uživatele* nebo *HTTP uživatele*.</span><span class="sxs-lookup"><span data-stu-id="febb4-107">**Ambari admin**: This account is also known as *Hadoop user* or *HTTP user*.</span></span> <span data-ttu-id="febb4-108">Tento účet lze použít pro přihlášení k Ambari na https://&lt;clustername >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="febb4-108">This account can be used to log on to Ambari at https://&lt;clustername>.azurehdinsight.net.</span></span> <span data-ttu-id="febb4-109">Můžete ho také použít ke spouštění dotazů na zobrazení Ambari, spouštění úloh prostřednictvím externích nástrojů (tj. prostředí PowerShell, Templeton, Visual Studio) a ověření pomocí ovladače Hive ODBC a nástrojů BI (tj. aplikace Excel, PowerBI nebo Tableau).</span><span class="sxs-lookup"><span data-stu-id="febb4-109">It can also be used to run queries on Ambari views, execute jobs via external tools (i.e. PowerShell, Templeton, Visual Studio), and authenticate with the Hive ODBC driver and BI tools (i.e. Excel, PowerBI, or Tableau).</span></span>
* <span data-ttu-id="febb4-110">**Uživatel SSH**: Tento účet je možné pomocí protokolu SSH a spuštěním příkazu "sudo" příkazů.</span><span class="sxs-lookup"><span data-stu-id="febb4-110">**SSH user**:  This account can be used with SSH, and execute sudo commands.</span></span> <span data-ttu-id="febb4-111">Má kořenový oprávnění pro virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="febb4-111">It has root privileges to the Linux VMs.</span></span>

<span data-ttu-id="febb4-112">Cluster HDInsight připojený k doméně má tři nové uživatele kromě správce Ambari a uživatel SSH.</span><span class="sxs-lookup"><span data-stu-id="febb4-112">A domain-joined HDInsight cluster has three new users in addition to Ambari Admin and SSH user.</span></span>

* <span data-ttu-id="febb4-113">**Správce škálu**: Tento účet je místní účet správce Apache škálu.</span><span class="sxs-lookup"><span data-stu-id="febb4-113">**Ranger admin**:  This account is the local Apache Ranger admin account.</span></span> <span data-ttu-id="febb4-114">Není uživatele domény služby active directory.</span><span class="sxs-lookup"><span data-stu-id="febb4-114">It is not an active directory domain user.</span></span> <span data-ttu-id="febb4-115">Tento účet slouží k nastavení zásad a zajistit další uživatelé, správci nebo delegovaní správci (takže tito uživatelé mohou spravovat zásady).</span><span class="sxs-lookup"><span data-stu-id="febb4-115">This account can be used to setup policies and make other users admins or delegated admins (so that those users can manage policies).</span></span> <span data-ttu-id="febb4-116">Ve výchozím nastavení je uživatelské jméno *správce* a heslo je stejné jako heslo správce Ambari.</span><span class="sxs-lookup"><span data-stu-id="febb4-116">By default, the username is *admin* and the password is the same as the Ambari admin password.</span></span> <span data-ttu-id="febb4-117">Na stránce nastavení v škálu lze aktualizovat heslo.</span><span class="sxs-lookup"><span data-stu-id="febb4-117">The password can be updated from the Settings page in Ranger.</span></span>
* <span data-ttu-id="febb4-118">**Uživatel domény s oprávněními správce clusteru**: Tento účet je určený jako správce clusteru Hadoop, včetně Ambari a škálu uživatele domény služby active directory.</span><span class="sxs-lookup"><span data-stu-id="febb4-118">**Cluster admin domain user**: This account is an active directory domain user designated as the Hadoop cluster admin including Ambari and Ranger.</span></span> <span data-ttu-id="febb4-119">Při vytváření clusteru, je třeba zadat pověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="febb4-119">You must provide this user’s credentials during cluster creation.</span></span> <span data-ttu-id="febb4-120">Tento uživatel má následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="febb4-120">This user has the following privileges:</span></span>

  * <span data-ttu-id="febb4-121">Připojení počítače k doméně a umístěte je v rámci organizační jednotky, které zadáte během vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="febb4-121">Join machines to the domain and place them within the OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="febb4-122">Vytvořte objekty služby v rámci organizační jednotky, které zadáte během vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="febb4-122">Create service principals within the OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="febb4-123">Vytvořte reverzní záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="febb4-123">Create reverse DNS entries.</span></span>

    <span data-ttu-id="febb4-124">Všimněte si, že tito ostatní uživatelé AD také mají tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="febb4-124">Note the other AD users also have these privileges.</span></span>

    <span data-ttu-id="febb4-125">Existují některé koncové body v rámci clusteru (například Templeton), které nejsou spravovány nástrojem škálu a proto nejsou zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="febb4-125">There are some end points within the cluster (for example, Templeton) which are not managed by Ranger, and hence are not secure.</span></span> <span data-ttu-id="febb4-126">Tyto koncové body jsou uzamčené pro všechny uživatele kromě domény uživatele Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="febb4-126">These end points are locked down for all users except the cluster admin domain user.</span></span>
* <span data-ttu-id="febb4-127">**Regulární**: při vytváření clusteru, můžete zadat více skupin služby active directory.</span><span class="sxs-lookup"><span data-stu-id="febb4-127">**Regular**: During cluster creation, you can provide multiple active directory groups.</span></span> <span data-ttu-id="febb4-128">Uživatelé v těchto skupinách se budou synchronizovat s škálu a Ambari.</span><span class="sxs-lookup"><span data-stu-id="febb4-128">The users in these groups will be synced to Ranger and Ambari.</span></span> <span data-ttu-id="febb4-129">Tito uživatelé jsou uživatelé domény a budou mít přístup jenom spravovaná škálu koncových bodů (například Hiveserver2).</span><span class="sxs-lookup"><span data-stu-id="febb4-129">These users are domain users and will have access to only Ranger-managed endpoints (for example, Hiveserver2).</span></span> <span data-ttu-id="febb4-130">Všechny zásady RBAC a auditování bude pro tyto uživatele.</span><span class="sxs-lookup"><span data-stu-id="febb4-130">All the RBAC policies and auditing will be applicable to these users.</span></span>

## <a name="roles-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="febb4-131">Role clusterů HDInsight připojený k doméně</span><span class="sxs-lookup"><span data-stu-id="febb4-131">Roles of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="febb4-132">HDInsight připojený k doméně mají následující role:</span><span class="sxs-lookup"><span data-stu-id="febb4-132">Domain-joined HDInsight have the following roles:</span></span>

* <span data-ttu-id="febb4-133">Správce clusteru</span><span class="sxs-lookup"><span data-stu-id="febb4-133">Cluster Administrator</span></span>
* <span data-ttu-id="febb4-134">Operátor clusteru</span><span class="sxs-lookup"><span data-stu-id="febb4-134">Cluster Operator</span></span>
* <span data-ttu-id="febb4-135">Správce služeb</span><span class="sxs-lookup"><span data-stu-id="febb4-135">Service Administrator</span></span>
* <span data-ttu-id="febb4-136">Operátor služby</span><span class="sxs-lookup"><span data-stu-id="febb4-136">Service Operator</span></span>
* <span data-ttu-id="febb4-137">Uživatel clusteru</span><span class="sxs-lookup"><span data-stu-id="febb4-137">Cluster User</span></span>

<span data-ttu-id="febb4-138">**Chcete-li zjistit, jaká oprávnění z těchto rolí**</span><span class="sxs-lookup"><span data-stu-id="febb4-138">**To see the permissions of these roles**</span></span>

1. <span data-ttu-id="febb4-139">Otevřete uživatelské rozhraní pro správu Ambari.</span><span class="sxs-lookup"><span data-stu-id="febb4-139">Open the Ambari Management UI.</span></span>  <span data-ttu-id="febb4-140">V tématu [otevřete uživatelské rozhraní pro správu Ambari](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="febb4-140">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="febb4-141">V levé nabídce klikněte na tlačítko **role**.</span><span class="sxs-lookup"><span data-stu-id="febb4-141">From the left menu, click **Roles**.</span></span>
3. <span data-ttu-id="febb4-142">Klepněte na modré otazník zobrazíte oprávnění:</span><span class="sxs-lookup"><span data-stu-id="febb4-142">Click the blue question mark to see the permissions:</span></span>

    ![Připojené k doméně oprávnění role HDInsight](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a><span data-ttu-id="febb4-144">Otevřete uživatelské rozhraní pro správu Ambari</span><span class="sxs-lookup"><span data-stu-id="febb4-144">Open the Ambari Management UI</span></span>
1. <span data-ttu-id="febb4-145">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="febb4-145">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="febb4-146">Otevřete v okně clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="febb4-146">Open your HDInsight cluster in a blade.</span></span> <span data-ttu-id="febb4-147">V tématu [seznamu a zobrazit clustery](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="febb4-147">See [List and show clusters](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span></span>
3. <span data-ttu-id="febb4-148">Klikněte na tlačítko **řídicí panel** z hlavní nabídky pro otevření Ambari.</span><span class="sxs-lookup"><span data-stu-id="febb4-148">Click **Dashboard** from the top menu to open Ambari.</span></span>
4. <span data-ttu-id="febb4-149">Přihlaste se k použití clusteru správce domény uživatelské jméno a heslo Ambari.</span><span class="sxs-lookup"><span data-stu-id="febb4-149">Log on to Ambari using the cluster administrator domain user name and password.</span></span>
5. <span data-ttu-id="febb4-150">Klikněte **správce** rozevírací nabídce z horní pravým dolním a potom klikněte na **spravovat Ambari**.</span><span class="sxs-lookup"><span data-stu-id="febb4-150">Click the **Admin** dropdown menu from the upper right corner, and then click **Manage Ambari**.</span></span>

    ![Připojené k doméně HDInsight spravovat Ambari](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    <span data-ttu-id="febb4-152">Uživatelské rozhraní bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="febb4-152">The UI looks like:</span></span>

    ![Připojené k doméně HDInsight Ambari uživatelské rozhraní pro správu](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a><span data-ttu-id="febb4-154">Seznam uživatelů domény synchronizované z Active Directory</span><span class="sxs-lookup"><span data-stu-id="febb4-154">List the domain users synchronized from your Active Directory</span></span>
1. <span data-ttu-id="febb4-155">Otevřete uživatelské rozhraní pro správu Ambari.</span><span class="sxs-lookup"><span data-stu-id="febb4-155">Open the Ambari Management UI.</span></span>  <span data-ttu-id="febb4-156">V tématu [otevřete uživatelské rozhraní pro správu Ambari](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="febb4-156">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="febb4-157">V levé nabídce klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="febb4-157">From the left menu, click **Users**.</span></span> <span data-ttu-id="febb4-158">Zobrazí se všichni uživatelé, které jsou synchronizované z vaší služby Active Directory do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="febb4-158">You shall see all the users synced from your Active Directory to the HDInsight cluster.</span></span>

    ![Připojené k doméně seznamu uživatelé uživatelské rozhraní HDInsight Ambari správy](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a><span data-ttu-id="febb4-160">Seznam skupiny v doméně synchronizované z Active Directory</span><span class="sxs-lookup"><span data-stu-id="febb4-160">List the domain groups synchronized from your Active Directory</span></span>
1. <span data-ttu-id="febb4-161">Otevřete uživatelské rozhraní pro správu Ambari.</span><span class="sxs-lookup"><span data-stu-id="febb4-161">Open the Ambari Management UI.</span></span>  <span data-ttu-id="febb4-162">V tématu [otevřete uživatelské rozhraní pro správu Ambari](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="febb4-162">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="febb4-163">V levé nabídce klikněte na tlačítko **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="febb4-163">From the left menu, click **Groups**.</span></span> <span data-ttu-id="febb4-164">Zobrazí se všechny skupiny, které jsou synchronizované z vaší služby Active Directory do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="febb4-164">You shall see all the groups synced from your Active Directory to the HDInsight cluster.</span></span>

    ![Připojené k doméně seznam skupin pro HDInsight Ambari správu uživatelského rozhraní](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a><span data-ttu-id="febb4-166">Konfigurace zobrazení Hive oprávnění</span><span class="sxs-lookup"><span data-stu-id="febb4-166">Configure Hive Views permissions</span></span>
1. <span data-ttu-id="febb4-167">Otevřete uživatelské rozhraní pro správu Ambari.</span><span class="sxs-lookup"><span data-stu-id="febb4-167">Open the Ambari Management UI.</span></span>  <span data-ttu-id="febb4-168">V tématu [otevřete uživatelské rozhraní pro správu Ambari](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="febb4-168">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="febb4-169">V levé nabídce klikněte na tlačítko **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="febb4-169">From the left menu, click **Views**.</span></span>
3. <span data-ttu-id="febb4-170">Klikněte na tlačítko **HIVE** zobrazíte podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="febb4-170">Click **HIVE** to show the details.</span></span>

    ![Připojené k doméně správy HDInsight Ambari Hive zobrazení uživatelského rozhraní](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. <span data-ttu-id="febb4-172">Klikněte **zobrazení Hive** odkaz pro konfiguraci zobrazení Hive.</span><span class="sxs-lookup"><span data-stu-id="febb4-172">Click the **Hive View** link to configure Hive Views.</span></span>
5. <span data-ttu-id="febb4-173">Přejděte dolů k položce **oprávnění** části.</span><span class="sxs-lookup"><span data-stu-id="febb4-173">Scroll down to the **Permissions** section.</span></span>

    ![Konfigurace oprávnění připojený k doméně správy HDInsight Ambari Hive zobrazení uživatelského rozhraní](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. <span data-ttu-id="febb4-175">Klikněte na tlačítko **přidat uživatele** nebo **přidat skupinu**a pak zadejte uživatele nebo skupiny, které můžete použít zobrazení Hive.</span><span class="sxs-lookup"><span data-stu-id="febb4-175">Click **Add User** or **Add Group**, and then specify the users or groups that can use Hive Views.</span></span>

## <a name="configure-users-for-the-roles"></a><span data-ttu-id="febb4-176">Konfigurovat uživatele pro role</span><span class="sxs-lookup"><span data-stu-id="febb4-176">Configure users for the roles</span></span>
 <span data-ttu-id="febb4-177">Seznam rolí a oprávnění k nim najdete v sekci [clustery HDInsight role z doméně](#roles-of-domain---joined-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="febb4-177">To see a list of roles and their permissions, see [Roles of Domain-joined HDInsight clusters](#roles-of-domain---joined-hdinsight-clusters).</span></span>

1. <span data-ttu-id="febb4-178">Otevřete uživatelské rozhraní pro správu Ambari.</span><span class="sxs-lookup"><span data-stu-id="febb4-178">Open the Ambari Management UI.</span></span>  <span data-ttu-id="febb4-179">V tématu [otevřete uživatelské rozhraní pro správu Ambari](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="febb4-179">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="febb4-180">V levé nabídce klikněte na tlačítko **role**.</span><span class="sxs-lookup"><span data-stu-id="febb4-180">From the left menu, click **Roles**.</span></span>
3. <span data-ttu-id="febb4-181">Klikněte na tlačítko **přidat uživatele** nebo **přidat skupinu** přiřadit uživatelů a skupin pro různé role.</span><span class="sxs-lookup"><span data-stu-id="febb4-181">Click **Add User** or **Add Group** to assign users and groups to different roles.</span></span>

## <a name="next-steps"></a><span data-ttu-id="febb4-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="febb4-182">Next steps</span></span>
* <span data-ttu-id="febb4-183">Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="febb4-183">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="febb4-184">Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).</span><span class="sxs-lookup"><span data-stu-id="febb4-184">For configuring Hive policies and run Hive queries, see [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md).</span></span>
* <span data-ttu-id="febb4-185">Spuštění dotazů Hive pomocí protokolu SSH v clusterech HDInsight připojený k doméně, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="febb4-185">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
