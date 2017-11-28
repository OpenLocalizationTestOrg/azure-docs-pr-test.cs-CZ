---
title: "clustery HDInsight připojený k doméně aaaManage - Azure | Microsoft Docs"
description: "Zjistěte, jak toomanage připojený k doméně clusterů HDInsight"
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
ms.openlocfilehash: 233ddf0953e981f9a24b77d9dde194d590e5e6d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a><span data-ttu-id="1de75-103">Správa clusterů HDInsight připojený k doméně (Preview)</span><span class="sxs-lookup"><span data-stu-id="1de75-103">Manage Domain-joined HDInsight clusters (Preview)</span></span>
<span data-ttu-id="1de75-104">Další informace hello uživatelů a rolí hello v doméně HDInsight a jak toomanage připojený k doméně clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1de75-104">Learn hello users and hello roles in Domain-joined HDInsight, and how toomanage domain-joined HDInsight clusters.</span></span>

## <a name="users-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="1de75-105">Uživatelé clustery HDInsight připojený k doméně</span><span class="sxs-lookup"><span data-stu-id="1de75-105">Users of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="1de75-106">Cluster služby HDInsight, který není připojený k doméně má dva uživatelské účty, které jsou vytvořené během vytváření clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="1de75-106">An HDInsight cluster that is not domain-joined has two user accounts that are created during hello cluster creation:</span></span>

* <span data-ttu-id="1de75-107">**Správce Ambari**: Tento účet je také označován jako *Hadoop uživatele* nebo *HTTP uživatele*.</span><span class="sxs-lookup"><span data-stu-id="1de75-107">**Ambari admin**: This account is also known as *Hadoop user* or *HTTP user*.</span></span> <span data-ttu-id="1de75-108">Tento účet lze použít toolog na tooAmbari na https://&lt;clustername >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="1de75-108">This account can be used toolog on tooAmbari at https://&lt;clustername>.azurehdinsight.net.</span></span> <span data-ttu-id="1de75-109">Můžete také být použité toorun dotazy na zobrazení Ambari, spusťte úloh prostřednictvím externích nástrojů (tj. prostředí PowerShell, Templeton, Visual Studio) a ověřit pomocí ovladače Hive ODBC hello a nástrojů BI (tj. aplikace Excel, PowerBI nebo Tableau).</span><span class="sxs-lookup"><span data-stu-id="1de75-109">It can also be used toorun queries on Ambari views, execute jobs via external tools (i.e. PowerShell, Templeton, Visual Studio), and authenticate with hello Hive ODBC driver and BI tools (i.e. Excel, PowerBI, or Tableau).</span></span>
* <span data-ttu-id="1de75-110">**Uživatel SSH**: Tento účet je možné pomocí protokolu SSH a spuštěním příkazu "sudo" příkazů.</span><span class="sxs-lookup"><span data-stu-id="1de75-110">**SSH user**:  This account can be used with SSH, and execute sudo commands.</span></span> <span data-ttu-id="1de75-111">Má toohello oprávnění kořenové virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="1de75-111">It has root privileges toohello Linux VMs.</span></span>

<span data-ttu-id="1de75-112">Cluster HDInsight připojený k doméně má tři nové uživatele v přidání tooAmbari správce a SSH uživatele.</span><span class="sxs-lookup"><span data-stu-id="1de75-112">A domain-joined HDInsight cluster has three new users in addition tooAmbari Admin and SSH user.</span></span>

* <span data-ttu-id="1de75-113">**Správce škálu**: Tento účet je hello místní účet správce Apache škálu.</span><span class="sxs-lookup"><span data-stu-id="1de75-113">**Ranger admin**:  This account is hello local Apache Ranger admin account.</span></span> <span data-ttu-id="1de75-114">Není uživatele domény služby active directory.</span><span class="sxs-lookup"><span data-stu-id="1de75-114">It is not an active directory domain user.</span></span> <span data-ttu-id="1de75-115">Tento účet můžete být použité toosetup zásady a provádět další uživatelé, správci nebo delegovaní správci (tak, aby tito uživatelé mohou spravovat zásady).</span><span class="sxs-lookup"><span data-stu-id="1de75-115">This account can be used toosetup policies and make other users admins or delegated admins (so that those users can manage policies).</span></span> <span data-ttu-id="1de75-116">Ve výchozím nastavení, je uživatelské jméno hello *správce* a hello heslo je hello stejná hodnota jako hello Ambari heslo správce.</span><span class="sxs-lookup"><span data-stu-id="1de75-116">By default, hello username is *admin* and hello password is hello same as hello Ambari admin password.</span></span> <span data-ttu-id="1de75-117">Hello heslo lze aktualizovat ze stránky nastavení hello škálu.</span><span class="sxs-lookup"><span data-stu-id="1de75-117">hello password can be updated from hello Settings page in Ranger.</span></span>
* <span data-ttu-id="1de75-118">**Uživatel domény s oprávněními správce clusteru**: Tento účet je označen jako hello Správce clusteru Hadoop včetně Ambari a škálu uživatele domény služby active directory.</span><span class="sxs-lookup"><span data-stu-id="1de75-118">**Cluster admin domain user**: This account is an active directory domain user designated as hello Hadoop cluster admin including Ambari and Ranger.</span></span> <span data-ttu-id="1de75-119">Při vytváření clusteru, je třeba zadat pověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="1de75-119">You must provide this user’s credentials during cluster creation.</span></span> <span data-ttu-id="1de75-120">Tento uživatel má hello následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="1de75-120">This user has hello following privileges:</span></span>

  * <span data-ttu-id="1de75-121">Připojení počítače toohello doméně a umístěte je v rámci hello organizační jednotky, které zadáte během vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="1de75-121">Join machines toohello domain and place them within hello OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="1de75-122">Vytvořte objekty služby v rámci hello organizační jednotky, které zadáte během vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="1de75-122">Create service principals within hello OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="1de75-123">Vytvořte reverzní záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="1de75-123">Create reverse DNS entries.</span></span>

    <span data-ttu-id="1de75-124">Poznámka: hello jiných uživatelů AD také mít tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1de75-124">Note hello other AD users also have these privileges.</span></span>

    <span data-ttu-id="1de75-125">Existují některé koncové body v rámci clusteru hello (například Templeton), které nejsou spravovány nástrojem škálu a proto nejsou zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="1de75-125">There are some end points within hello cluster (for example, Templeton) which are not managed by Ranger, and hence are not secure.</span></span> <span data-ttu-id="1de75-126">Tyto koncové body jsou uzamčené pro všechny uživatele kromě správce domény hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="1de75-126">These end points are locked down for all users except hello cluster admin domain user.</span></span>
* <span data-ttu-id="1de75-127">**Regulární**: při vytváření clusteru, můžete zadat více skupin služby active directory.</span><span class="sxs-lookup"><span data-stu-id="1de75-127">**Regular**: During cluster creation, you can provide multiple active directory groups.</span></span> <span data-ttu-id="1de75-128">Hello uživatelé v těchto skupinách nebudou synchronizovaná tooRanger a Ambari.</span><span class="sxs-lookup"><span data-stu-id="1de75-128">hello users in these groups will be synced tooRanger and Ambari.</span></span> <span data-ttu-id="1de75-129">Tito uživatelé jsou uživatelé domény a budou mít přístup tooonly škálu spravované koncové body (například Hiveserver2).</span><span class="sxs-lookup"><span data-stu-id="1de75-129">These users are domain users and will have access tooonly Ranger-managed endpoints (for example, Hiveserver2).</span></span> <span data-ttu-id="1de75-130">Všechny hello RBAC zásady a auditování bude toothese příslušné uživatele.</span><span class="sxs-lookup"><span data-stu-id="1de75-130">All hello RBAC policies and auditing will be applicable toothese users.</span></span>

## <a name="roles-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="1de75-131">Role clusterů HDInsight připojený k doméně</span><span class="sxs-lookup"><span data-stu-id="1de75-131">Roles of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="1de75-132">HDInsight připojený k doméně mají hello následující role:</span><span class="sxs-lookup"><span data-stu-id="1de75-132">Domain-joined HDInsight have hello following roles:</span></span>

* <span data-ttu-id="1de75-133">Správce clusteru</span><span class="sxs-lookup"><span data-stu-id="1de75-133">Cluster Administrator</span></span>
* <span data-ttu-id="1de75-134">Operátor clusteru</span><span class="sxs-lookup"><span data-stu-id="1de75-134">Cluster Operator</span></span>
* <span data-ttu-id="1de75-135">Správce služeb</span><span class="sxs-lookup"><span data-stu-id="1de75-135">Service Administrator</span></span>
* <span data-ttu-id="1de75-136">Operátor služby</span><span class="sxs-lookup"><span data-stu-id="1de75-136">Service Operator</span></span>
* <span data-ttu-id="1de75-137">Uživatel clusteru</span><span class="sxs-lookup"><span data-stu-id="1de75-137">Cluster User</span></span>

<span data-ttu-id="1de75-138">**oprávnění hello toosee z těchto rolí**</span><span class="sxs-lookup"><span data-stu-id="1de75-138">**toosee hello permissions of these roles**</span></span>

1. <span data-ttu-id="1de75-139">Otevřete hello uživatelského rozhraní Ambari správy.</span><span class="sxs-lookup"><span data-stu-id="1de75-139">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="1de75-140">V tématu [otevřete hello uživatelského rozhraní Ambari správu](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="1de75-140">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="1de75-141">V levé nabídce hello, klikněte na **role**.</span><span class="sxs-lookup"><span data-stu-id="1de75-141">From hello left menu, click **Roles**.</span></span>
3. <span data-ttu-id="1de75-142">Klikněte na tlačítko hello blue otazník toosee hello oprávnění:</span><span class="sxs-lookup"><span data-stu-id="1de75-142">Click hello blue question mark toosee hello permissions:</span></span>

    ![Připojené k doméně oprávnění role HDInsight](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-hello-ambari-management-ui"></a><span data-ttu-id="1de75-144">Otevřete hello Ambari správu uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="1de75-144">Open hello Ambari Management UI</span></span>
1. <span data-ttu-id="1de75-145">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1de75-145">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="1de75-146">Otevřete v okně clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1de75-146">Open your HDInsight cluster in a blade.</span></span> <span data-ttu-id="1de75-147">V tématu [seznamu a zobrazit clustery](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="1de75-147">See [List and show clusters](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span></span>
3. <span data-ttu-id="1de75-148">Klikněte na tlačítko **řídicí panel** z hlavní nabídky tooopen hello Ambari.</span><span class="sxs-lookup"><span data-stu-id="1de75-148">Click **Dashboard** from hello top menu tooopen Ambari.</span></span>
4. <span data-ttu-id="1de75-149">Přihlaste se tooAmbari pomocí hello clusteru správce domény uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="1de75-149">Log on tooAmbari using hello cluster administrator domain user name and password.</span></span>
5. <span data-ttu-id="1de75-150">Klikněte na tlačítko hello **správce** z hello pravém horním rohu a pak klikněte na rozevírací nabídku **spravovat Ambari**.</span><span class="sxs-lookup"><span data-stu-id="1de75-150">Click hello **Admin** dropdown menu from hello upper right corner, and then click **Manage Ambari**.</span></span>

    ![Připojené k doméně HDInsight spravovat Ambari](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    <span data-ttu-id="1de75-152">Hello uživatelského rozhraní vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1de75-152">hello UI looks like:</span></span>

    ![Připojené k doméně HDInsight Ambari uživatelské rozhraní pro správu](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-hello-domain-users-synchronized-from-your-active-directory"></a><span data-ttu-id="1de75-154">Seznam uživatelů domény hello synchronizované z Active Directory</span><span class="sxs-lookup"><span data-stu-id="1de75-154">List hello domain users synchronized from your Active Directory</span></span>
1. <span data-ttu-id="1de75-155">Otevřete hello uživatelského rozhraní Ambari správy.</span><span class="sxs-lookup"><span data-stu-id="1de75-155">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="1de75-156">V tématu [otevřete hello uživatelského rozhraní Ambari správu](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="1de75-156">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="1de75-157">V levé nabídce hello, klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="1de75-157">From hello left menu, click **Users**.</span></span> <span data-ttu-id="1de75-158">Zobrazí se všichni uživatelé hello synchronizované z vašeho clusteru HDInsight toohello služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1de75-158">You shall see all hello users synced from your Active Directory toohello HDInsight cluster.</span></span>

    ![Připojené k doméně seznamu uživatelé uživatelské rozhraní HDInsight Ambari správy](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-hello-domain-groups-synchronized-from-your-active-directory"></a><span data-ttu-id="1de75-160">Seznam hello doménové skupiny synchronizovány ze služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="1de75-160">List hello domain groups synchronized from your Active Directory</span></span>
1. <span data-ttu-id="1de75-161">Otevřete hello uživatelského rozhraní Ambari správy.</span><span class="sxs-lookup"><span data-stu-id="1de75-161">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="1de75-162">V tématu [otevřete hello uživatelského rozhraní Ambari správu](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="1de75-162">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="1de75-163">V levé nabídce hello, klikněte na **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="1de75-163">From hello left menu, click **Groups**.</span></span> <span data-ttu-id="1de75-164">Zobrazí se všechny skupiny hello synchronizované z vašeho clusteru HDInsight toohello služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1de75-164">You shall see all hello groups synced from your Active Directory toohello HDInsight cluster.</span></span>

    ![Připojené k doméně seznam skupin pro HDInsight Ambari správu uživatelského rozhraní](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a><span data-ttu-id="1de75-166">Konfigurace zobrazení Hive oprávnění</span><span class="sxs-lookup"><span data-stu-id="1de75-166">Configure Hive Views permissions</span></span>
1. <span data-ttu-id="1de75-167">Otevřete hello uživatelského rozhraní Ambari správy.</span><span class="sxs-lookup"><span data-stu-id="1de75-167">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="1de75-168">V tématu [otevřete hello uživatelského rozhraní Ambari správu](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="1de75-168">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="1de75-169">V levé nabídce hello, klikněte na **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="1de75-169">From hello left menu, click **Views**.</span></span>
3. <span data-ttu-id="1de75-170">Klikněte na tlačítko **HIVE** tooshow hello podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1de75-170">Click **HIVE** tooshow hello details.</span></span>

    ![Připojené k doméně správy HDInsight Ambari Hive zobrazení uživatelského rozhraní](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. <span data-ttu-id="1de75-172">Klikněte na tlačítko hello **zobrazení Hive** propojit zobrazení Hive tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="1de75-172">Click hello **Hive View** link tooconfigure Hive Views.</span></span>
5. <span data-ttu-id="1de75-173">Projděte dolů toohello **oprávnění** části.</span><span class="sxs-lookup"><span data-stu-id="1de75-173">Scroll down toohello **Permissions** section.</span></span>

    ![Konfigurace oprávnění připojený k doméně správy HDInsight Ambari Hive zobrazení uživatelského rozhraní](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. <span data-ttu-id="1de75-175">Klikněte na tlačítko **přidat uživatele** nebo **přidat skupinu**a pak zadejte hello uživatele nebo skupiny, které můžete použít zobrazení Hive.</span><span class="sxs-lookup"><span data-stu-id="1de75-175">Click **Add User** or **Add Group**, and then specify hello users or groups that can use Hive Views.</span></span>

## <a name="configure-users-for-hello-roles"></a><span data-ttu-id="1de75-176">Konfigurovat uživatele pro role hello</span><span class="sxs-lookup"><span data-stu-id="1de75-176">Configure users for hello roles</span></span>
 <span data-ttu-id="1de75-177">toosee seznam rolí a jejich oprávnění, najdete v části [clustery HDInsight role z doméně](#roles-of-domain---joined-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="1de75-177">toosee a list of roles and their permissions, see [Roles of Domain-joined HDInsight clusters](#roles-of-domain---joined-hdinsight-clusters).</span></span>

1. <span data-ttu-id="1de75-178">Otevřete hello uživatelského rozhraní Ambari správy.</span><span class="sxs-lookup"><span data-stu-id="1de75-178">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="1de75-179">V tématu [otevřete hello uživatelského rozhraní Ambari správu](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="1de75-179">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="1de75-180">V levé nabídce hello, klikněte na **role**.</span><span class="sxs-lookup"><span data-stu-id="1de75-180">From hello left menu, click **Roles**.</span></span>
3. <span data-ttu-id="1de75-181">Klikněte na tlačítko **přidat uživatele** nebo **přidat skupinu** tooassign uživatelů a skupin toodifferent role.</span><span class="sxs-lookup"><span data-stu-id="1de75-181">Click **Add User** or **Add Group** tooassign users and groups toodifferent roles.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1de75-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1de75-182">Next steps</span></span>
* <span data-ttu-id="1de75-183">Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1de75-183">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="1de75-184">Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).</span><span class="sxs-lookup"><span data-stu-id="1de75-184">For configuring Hive policies and run Hive queries, see [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md).</span></span>
* <span data-ttu-id="1de75-185">Spuštění dotazů Hive pomocí protokolu SSH v clusterech HDInsight připojený k doméně, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="1de75-185">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
