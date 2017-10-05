---
title: "Správa Azure Data Lake Analytics pomocí portálu Azure | Microsoft Docs"
description: "Naučte se spravovat počtech, zdroje dat, uživatele a úlohy Data Lake Analytics."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: e49d1a0e0ccc6567d0a6841817667717ff5dba76
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-data-lake-analytics-by-using-the-azure-portal"></a><span data-ttu-id="cd24b-103">Správa Azure Data Lake Analytics pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cd24b-103">Manage Azure Data Lake Analytics by using the Azure portal</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="cd24b-104">Naučte se spravovat účty, účet zdroje dat, uživatele a úlohy Azure Data Lake Analytics pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cd24b-104">Learn how to manage Azure Data Lake Analytics accounts, account data sources, users, and jobs by using the Azure portal.</span></span> <span data-ttu-id="cd24b-105">Témata týkající se řízení o pomocí jiných nástrojů zobrazíte kliknutím na karty v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="cd24b-105">To see management topics about using other tools, click a tab at the top of the page.</span></span>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a><span data-ttu-id="cd24b-106">Správa účtů Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cd24b-106">Manage Data Lake Analytics accounts</span></span>

### <a name="create-an-account"></a><span data-ttu-id="cd24b-107">Vytvoření účtu</span><span class="sxs-lookup"><span data-stu-id="cd24b-107">Create an account</span></span>

1. <span data-ttu-id="cd24b-108">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cd24b-108">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="cd24b-109">Klikněte na **Nový** > **Inteligentní funkce a analýzy** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-109">Click **New** > **Intelligence + analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="cd24b-110">Vyberte hodnoty pro následující položky:</span><span class="sxs-lookup"><span data-stu-id="cd24b-110">Select values for the following items:</span></span> 
   1. <span data-ttu-id="cd24b-111">**Název**: název účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-111">**Name**: The name of the Data Lake Analytics account.</span></span>
   2. <span data-ttu-id="cd24b-112">**Předplatné**: předplatné Azure použité pro účet.</span><span class="sxs-lookup"><span data-stu-id="cd24b-112">**Subscription**: The Azure subscription used for the account.</span></span>
   3. <span data-ttu-id="cd24b-113">**Skupina prostředků**: Skupina prostředků Azure, ve které chcete vytvořit účet.</span><span class="sxs-lookup"><span data-stu-id="cd24b-113">**Resource Group**: The Azure resource group in which to create the account.</span></span> 
   4. <span data-ttu-id="cd24b-114">**Umístění**: datové centrum Azure pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-114">**Location**: The Azure datacenter for the Data Lake Analytics account.</span></span> 
   5. <span data-ttu-id="cd24b-115">**Data Lake Store**: výchozí úložiště, který má být použit pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-115">**Data Lake Store**: The default store to be used for the Data Lake Analytics account.</span></span> <span data-ttu-id="cd24b-116">Účet Azure Data Lake Store a účet Data Lake Analytics musí být ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="cd24b-116">The Azure Data Lake Store account and the Data Lake Analytics account must be in the same location.</span></span>
4. <span data-ttu-id="cd24b-117">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-117">Click **Create**.</span></span> 

### <a name="delete-a-data-lake-analytics-account"></a><span data-ttu-id="cd24b-118">Odstranění účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cd24b-118">Delete a Data Lake Analytics account</span></span>

<span data-ttu-id="cd24b-119">Před odstraněním účtu Data Lake Analytics, odstraňte jeho výchozí účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="cd24b-119">Before you delete a Data Lake Analytics account, delete its default Data Lake Store account.</span></span>

1. <span data-ttu-id="cd24b-120">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-120">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-121">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-121">Click **Delete**.</span></span>
3. <span data-ttu-id="cd24b-122">Zadejte název účtu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-122">Type the account name.</span></span>
4. <span data-ttu-id="cd24b-123">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-123">Click **Delete**.</span></span>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a><span data-ttu-id="cd24b-124">Správa zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="cd24b-124">Manage data sources</span></span>

<span data-ttu-id="cd24b-125">Data Lake Analytics podporuje následující zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="cd24b-125">Data Lake Analytics supports the following data sources:</span></span>

* <span data-ttu-id="cd24b-126">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="cd24b-126">Data Lake Store</span></span>
* <span data-ttu-id="cd24b-127">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="cd24b-127">Azure Storage</span></span>

<span data-ttu-id="cd24b-128">Průzkumník dat můžete procházet zdroje dat a provádět operace správy základního souboru.</span><span class="sxs-lookup"><span data-stu-id="cd24b-128">You can use Data Explorer to browse data sources and perform basic file management operations.</span></span> 

### <a name="add-a-data-source"></a><span data-ttu-id="cd24b-129">Přidání zdroje dat</span><span class="sxs-lookup"><span data-stu-id="cd24b-129">Add a data source</span></span>

1. <span data-ttu-id="cd24b-130">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-130">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-131">Klikněte na tlačítko **zdroje dat**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-131">Click **Data Sources**.</span></span>
3. <span data-ttu-id="cd24b-132">Klikněte na tlačítko **přidat zdroj dat**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-132">Click **Add Data Source**.</span></span>
    
   * <span data-ttu-id="cd24b-133">Přidání účtu Data Lake Store, potřebujete název účtu a přístup k účtu mohli dotaz ho.</span><span class="sxs-lookup"><span data-stu-id="cd24b-133">To add a Data Lake Store account, you need the account name and access to the account to be able to query it.</span></span>
   * <span data-ttu-id="cd24b-134">Přidat úložiště objektů Blob v Azure, budete potřebovat účet úložiště a klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-134">To add Azure Blob storage, you need the storage account and the account key.</span></span> <span data-ttu-id="cd24b-135">Kde je najít, přejděte k účtu úložiště na portálu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-135">To find them, go to the storage account in the portal.</span></span>

## <a name="set-up-firewall-rules"></a><span data-ttu-id="cd24b-136">Nastavení pravidel brány firewall</span><span class="sxs-lookup"><span data-stu-id="cd24b-136">Set up firewall rules</span></span>

<span data-ttu-id="cd24b-137">Data Lake Analytics můžete další uzamčení přístup ke svému účtu Data Lake Analytics na úrovni sítě.</span><span class="sxs-lookup"><span data-stu-id="cd24b-137">You can use Data Lake Analytics to further lock down access to your Data Lake Analytics account at the network level.</span></span> <span data-ttu-id="cd24b-138">Můžete povolit bránu firewall, zadejte IP adresu nebo zadejte rozsah IP adres pro klienty důvěryhodné.</span><span class="sxs-lookup"><span data-stu-id="cd24b-138">You can enable a firewall, specify an IP address, or define an IP address range for your trusted clients.</span></span> <span data-ttu-id="cd24b-139">Povolíte-li tato opatření, pouze klienti, kteří mají IP adresy v definovaném rozsahu může připojit k úložišti.</span><span class="sxs-lookup"><span data-stu-id="cd24b-139">After you enable these measures, only clients that have the IP addresses within the defined range can connect to the store.</span></span>

<span data-ttu-id="cd24b-140">Pokud jinými službami Azure, jako je Azure Data Factory nebo virtuální počítače, připojení k účtu Data Lake Analytics, ujistěte se, že **povolit služby Azure** je zapnuta **na**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-140">If other Azure services, like Azure Data Factory or VMs, connect to the Data Lake Analytics account, make sure that **Allow Azure Services** is turned **On**.</span></span> 

### <a name="set-up-a-firewall-rule"></a><span data-ttu-id="cd24b-141">Nastavit pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="cd24b-141">Set up a firewall rule</span></span>

1. <span data-ttu-id="cd24b-142">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-142">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-143">V nabídce na levé straně klikněte na tlačítko **brány Firewall**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-143">On the menu on the left, click **Firewall**.</span></span>

## <a name="add-a-new-user"></a><span data-ttu-id="cd24b-144">Přidání nového uživatele</span><span class="sxs-lookup"><span data-stu-id="cd24b-144">Add a new user</span></span>

<span data-ttu-id="cd24b-145">Můžete použít **Průvodce přidáním uživatele** snadno zřídit nové uživatele Data Lake.</span><span class="sxs-lookup"><span data-stu-id="cd24b-145">You can use the **Add User Wizard** to easily provision new Data Lake users.</span></span>

1. <span data-ttu-id="cd24b-146">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-146">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-147">Na levé straně v části **Začínáme**, klikněte na tlačítko **Průvodce přidáním uživatele**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-147">On the left, under **Getting Started**, click **Add User Wizard**.</span></span>
3. <span data-ttu-id="cd24b-148">Vyberte uživatele a pak klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-148">Select a user, and then click **Select**.</span></span>
4. <span data-ttu-id="cd24b-149">Vyberte roli a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-149">Select a role, and then click **Select**.</span></span> <span data-ttu-id="cd24b-150">Chcete-li nastavit nový vývojář používat Azure Data Lake, vyberte **Data Lake Analytics vývojáře** role.</span><span class="sxs-lookup"><span data-stu-id="cd24b-150">To set up a new developer to use Azure Data Lake, select the **Data Lake Analytics Developer** role.</span></span>
5. <span data-ttu-id="cd24b-151">Vyberte seznamy řízení přístupu (ACL) pro databáze U-SQL.</span><span class="sxs-lookup"><span data-stu-id="cd24b-151">Select the access control lists (ACLs) for the U-SQL databases.</span></span> <span data-ttu-id="cd24b-152">Až budete spokojeni s vaší volby, klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-152">When you're satisfied with your choices, click **Select**.</span></span>
6. <span data-ttu-id="cd24b-153">Vyberte seznamy ACL pro soubory.</span><span class="sxs-lookup"><span data-stu-id="cd24b-153">Select the ACLs for files.</span></span> <span data-ttu-id="cd24b-154">Pro výchozí úložiště neměnit seznamy ACL pro kořenovou složku "/" a ve složce/System.</span><span class="sxs-lookup"><span data-stu-id="cd24b-154">For the default store, don't change the ACLs for the root folder "/" and for the /system folder.</span></span> <span data-ttu-id="cd24b-155">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-155">Click **Select**.</span></span>
7. <span data-ttu-id="cd24b-156">Zkontrolujte všechny vybrané změny a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-156">Review all your selected changes, and then click **Run**.</span></span>
8. <span data-ttu-id="cd24b-157">Po dokončení průvodce klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-157">When the wizard is finished, click **Done**.</span></span>

## <a name="manage-role-based-access-control"></a><span data-ttu-id="cd24b-158">Správa řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="cd24b-158">Manage Role-Based Access Control</span></span>

<span data-ttu-id="cd24b-159">Jako jinými službami Azure můžete řídit, jak uživatelé komunikovat se službou řízení přístupu na základě Role (RBAC).</span><span class="sxs-lookup"><span data-stu-id="cd24b-159">Like other Azure services, you can use Role-Based Access Control (RBAC) to control how users interact with the service.</span></span>

<span data-ttu-id="cd24b-160">Standardní role RBAC mají následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="cd24b-160">The standard RBAC roles have the following capabilities:</span></span>
* <span data-ttu-id="cd24b-161">**Vlastník**: můžete odeslat úlohy sledování úloh, zrušte úlohy z libovolného uživatele a nakonfigurovat účet.</span><span class="sxs-lookup"><span data-stu-id="cd24b-161">**Owner**: Can submit jobs, monitor jobs, cancel jobs from any user, and configure the account.</span></span>
* <span data-ttu-id="cd24b-162">**Přispěvatel**: můžete odeslat úlohy sledování úloh, zrušte úlohy z libovolného uživatele a nakonfigurovat účet.</span><span class="sxs-lookup"><span data-stu-id="cd24b-162">**Contributor**: Can submit jobs, monitor jobs, cancel jobs from any user, and configure the account.</span></span>
* <span data-ttu-id="cd24b-163">**Čtečka**: můžete sledovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd24b-163">**Reader**: Can monitor jobs.</span></span>

<span data-ttu-id="cd24b-164">Pomocí role Data Lake Analytics Developer a umožňuje vývojářům používat službu Data Lake Analytics U-SQL.</span><span class="sxs-lookup"><span data-stu-id="cd24b-164">Use the Data Lake Analytics Developer role to enable U-SQL developers to use the Data Lake Analytics service.</span></span> <span data-ttu-id="cd24b-165">Můžete použít roli Data Lake Analytics vývojáře tak, aby:</span><span class="sxs-lookup"><span data-stu-id="cd24b-165">You can use the Data Lake Analytics Developer role to:</span></span>
* <span data-ttu-id="cd24b-166">Odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd24b-166">Submit jobs.</span></span>
* <span data-ttu-id="cd24b-167">Monitorování stavu úlohy a průběh úlohy, odeslané žádný uživatel.</span><span class="sxs-lookup"><span data-stu-id="cd24b-167">Monitor job status and the progress of jobs submitted by any user.</span></span>
* <span data-ttu-id="cd24b-168">V tématu skriptů U-SQL z úlohy, odeslané žádný uživatel.</span><span class="sxs-lookup"><span data-stu-id="cd24b-168">See the U-SQL scripts from jobs submitted by any user.</span></span>
* <span data-ttu-id="cd24b-169">Zrušte jenom vlastní úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd24b-169">Cancel only your own jobs.</span></span>

### <a name="add-users-or-security-groups-to-a-data-lake-analytics-account"></a><span data-ttu-id="cd24b-170">Přidat uživatele nebo skupiny zabezpečení do účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cd24b-170">Add users or security groups to a Data Lake Analytics account</span></span>

1. <span data-ttu-id="cd24b-171">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-171">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-172">Klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-172">Click **Access control (IAM)** > **Add**.</span></span>
3. <span data-ttu-id="cd24b-173">Vyberte roli.</span><span class="sxs-lookup"><span data-stu-id="cd24b-173">Select a role.</span></span>
4. <span data-ttu-id="cd24b-174">Přidání uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd24b-174">Add a user.</span></span>
5. <span data-ttu-id="cd24b-175">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-175">Click **OK**.</span></span>

>[!NOTE]
><span data-ttu-id="cd24b-176">Pokud uživatele nebo skupinu zabezpečení potřebuje k odesílání úloh, potřebují také mít oprávnění pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="cd24b-176">If a user or a security group needs to submit jobs, they also need permission on the store account.</span></span> <span data-ttu-id="cd24b-177">Další informace najdete v tématu [zabezpečení dat uložených v Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).</span><span class="sxs-lookup"><span data-stu-id="cd24b-177">For more information, see [Secure data stored in Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).</span></span>
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a><span data-ttu-id="cd24b-178">Správa úloh</span><span class="sxs-lookup"><span data-stu-id="cd24b-178">Manage jobs</span></span>

### <a name="submit-a-job"></a><span data-ttu-id="cd24b-179">Odeslání úlohy</span><span class="sxs-lookup"><span data-stu-id="cd24b-179">Submit a job</span></span>

1. <span data-ttu-id="cd24b-180">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-180">In the Azure portal, go to your Data Lake Analytics account.</span></span>

2. <span data-ttu-id="cd24b-181">Klikněte na tlačítko **nová úloha**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-181">Click **New Job**.</span></span> <span data-ttu-id="cd24b-182">Pro každou úlohu konfigurace:</span><span class="sxs-lookup"><span data-stu-id="cd24b-182">For each job,  configure:</span></span>

    1. <span data-ttu-id="cd24b-183">**Název úlohy**: název úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd24b-183">**Job Name**: The name of the job.</span></span>
    2. <span data-ttu-id="cd24b-184">**Priorita**: nižší čísla mají vyšší prioritu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-184">**Priority**: Lower numbers have higher priority.</span></span> <span data-ttu-id="cd24b-185">Pokud dvě úlohy jsou zařazeny do fronty, spustí se první kategorií s nižší hodnotou priority.</span><span class="sxs-lookup"><span data-stu-id="cd24b-185">If two jobs are queued, the one with lower priority value runs first.</span></span>
    3. <span data-ttu-id="cd24b-186">**Paralelismus**: maximální počet výpočetních procesů můžete vyhradit pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-186">**Parallelism**: The maximum number of compute processes to reserve for this job.</span></span>

3. <span data-ttu-id="cd24b-187">Klikněte na **Odeslat úlohu**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-187">Click **Submit Job**.</span></span>

### <a name="monitor-jobs"></a><span data-ttu-id="cd24b-188">Monitorování úloh</span><span class="sxs-lookup"><span data-stu-id="cd24b-188">Monitor jobs</span></span>

1. <span data-ttu-id="cd24b-189">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-189">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-190">Klikněte na tlačítko **vidět všechny úlohy**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-190">Click **View All Jobs**.</span></span> <span data-ttu-id="cd24b-191">Se zobrazí seznam všech aktivní a nedávno dokončení úloh v účtu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-191">A list of all the active and recently finished jobs in the account is shown.</span></span>
3. <span data-ttu-id="cd24b-192">Volitelně klikněte na **filtru** a umožňují najít úloh podle **časový rozsah**, **název úlohy**, a **Autor** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cd24b-192">Optionally, click **Filter** to help you find the jobs by **Time Range**, **Job Name**, and **Author** values.</span></span> 

### <a name="monitoring-pipeline-jobs"></a><span data-ttu-id="cd24b-193">Sledování úloh kanálu</span><span class="sxs-lookup"><span data-stu-id="cd24b-193">Monitoring pipeline jobs</span></span>
<span data-ttu-id="cd24b-194">Úlohy, které jsou součástí kanálu fungovat společně, obvykle postupně k provedení určitého scénáře.</span><span class="sxs-lookup"><span data-stu-id="cd24b-194">Jobs that are part of a pipeline work together, usually sequentially, to accomplish a specific scenario.</span></span> <span data-ttu-id="cd24b-195">Například můžete mít kanál, který odstraní, extrahuje, transformuje, agreguje využití přehledů zákazníka.</span><span class="sxs-lookup"><span data-stu-id="cd24b-195">For example, you can have a pipeline that cleans, extracts, transforms, aggregates usage for customer insights.</span></span> <span data-ttu-id="cd24b-196">Úlohy kanálu se identifikují pomocí vlastnosti "Kanál", pokud úloha byla odeslána.</span><span class="sxs-lookup"><span data-stu-id="cd24b-196">Pipeline jobs are identified using the "Pipeline" property when the job was submitted.</span></span> <span data-ttu-id="cd24b-197">Pomocí ADF V2 naplánované úlohy mít tato vlastnost vyplní automaticky.</span><span class="sxs-lookup"><span data-stu-id="cd24b-197">Jobs scheduled using ADF V2 will automatically have this property populated.</span></span> 

<span data-ttu-id="cd24b-198">Chcete-li zobrazit seznam úloh U-SQL, které jsou součástí kanály:</span><span class="sxs-lookup"><span data-stu-id="cd24b-198">To view a list of U-SQL jobs that are part of pipelines:</span></span> 

1. <span data-ttu-id="cd24b-199">V portálu Azure přejděte do své účty Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-199">In the Azure portal, go to your Data Lake Analytics accounts.</span></span>
2. <span data-ttu-id="cd24b-200">Klikněte na tlačítko **úlohy Insights**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-200">Click **Job Insights**.</span></span> <span data-ttu-id="cd24b-201">Na kartě "Všechny úlohy" bude použita jako výchozí, zobrazující seznam spuštěná, zařazených do fronty a skončila úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd24b-201">The "All Jobs" tab will be defaulted, showing a list of running, queued, and ended jobs.</span></span>
3. <span data-ttu-id="cd24b-202">Klikněte **kanálu úlohy** kartě. Zobrazí se seznam úloh kanálu spolu s souhrnných statistik pro každý kanál.</span><span class="sxs-lookup"><span data-stu-id="cd24b-202">Click the **Pipeline Jobs** tab. A list of pipeline jobs will be shown along with aggregated statistics for each pipeline.</span></span>

### <a name="monitoring-recurring-jobs"></a><span data-ttu-id="cd24b-203">Monitorování opakované úlohy</span><span class="sxs-lookup"><span data-stu-id="cd24b-203">Monitoring recurring jobs</span></span>
<span data-ttu-id="cd24b-204">Opakování úlohy je ten, který má stejné obchodní logiku, ale používá jiné vstupní data pokaždé, když ji spustí.</span><span class="sxs-lookup"><span data-stu-id="cd24b-204">A recurring job is one that has the same business logic but uses different input data every time it runs.</span></span> <span data-ttu-id="cd24b-205">V ideálním případě opakované úlohy doporučujeme vždy úspěšné a mají relativně stabilní čas provádění; monitorování těchto chování pomůže zajistit, že úloha je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="cd24b-205">Ideally, recurring jobs should always succeed, and have relatively stable execution time; monitoring these behaviors will help ensure the job is healthy.</span></span> <span data-ttu-id="cd24b-206">Opakované úlohy se identifikují pomocí vlastnosti "Recurrence".</span><span class="sxs-lookup"><span data-stu-id="cd24b-206">Recurring jobs are identified using the "Recurrence" property.</span></span> <span data-ttu-id="cd24b-207">Pomocí ADF V2 naplánované úlohy mít tato vlastnost vyplní automaticky.</span><span class="sxs-lookup"><span data-stu-id="cd24b-207">Jobs scheduled using ADF V2 will automatically have this property populated.</span></span>

<span data-ttu-id="cd24b-208">Chcete-li zobrazit seznam úloh U-SQL, které jsou opakování:</span><span class="sxs-lookup"><span data-stu-id="cd24b-208">To view a list of U-SQL jobs that are recurring:</span></span> 

1. <span data-ttu-id="cd24b-209">V portálu Azure přejděte do své účty Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-209">In the Azure portal, go to your Data Lake Analytics accounts.</span></span>
2. <span data-ttu-id="cd24b-210">Klikněte na tlačítko **úlohy Insights**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-210">Click **Job Insights**.</span></span> <span data-ttu-id="cd24b-211">Na kartě "Všechny úlohy" bude použita jako výchozí, zobrazující seznam spuštěná, zařazených do fronty a skončila úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd24b-211">The "All Jobs" tab will be defaulted, showing a list of running, queued, and ended jobs.</span></span>
3. <span data-ttu-id="cd24b-212">Klikněte **opakovaných úloh** kartě. Společně s souhrnných statistik pro každou úlohu opakující se zobrazí seznam opakované úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd24b-212">Click the **Recurring Jobs** tab. A list of recurring jobs will be shown along with aggregated statistics for each recurring job.</span></span>

## <a name="manage-policies"></a><span data-ttu-id="cd24b-213">Správa zásad</span><span class="sxs-lookup"><span data-stu-id="cd24b-213">Manage policies</span></span>

### <a name="account-level-policies"></a><span data-ttu-id="cd24b-214">Zásady na úrovni účtu</span><span class="sxs-lookup"><span data-stu-id="cd24b-214">Account-level policies</span></span>

<span data-ttu-id="cd24b-215">Tyto zásady platí pro všechny úlohy v účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-215">These policies apply to all jobs in a Data Lake Analytics account.</span></span>

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a><span data-ttu-id="cd24b-216">Maximální počet Austrálie v účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cd24b-216">Maximum number of AUs in a Data Lake Analytics account</span></span>
<span data-ttu-id="cd24b-217">Zásady řídí celkový počet jednotek Analytics (Austrálie) můžete použít váš účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-217">A policy controls the total number of Analytics Units (AUs) your Data Lake Analytics account can use.</span></span> <span data-ttu-id="cd24b-218">Ve výchozím nastavení je hodnota nastavena na 250.</span><span class="sxs-lookup"><span data-stu-id="cd24b-218">By default, the value is set to 250.</span></span> <span data-ttu-id="cd24b-219">Například, pokud je tato hodnota nastavena na 250 Austrálie, může mít jednu úlohu s 250 Austrálie přiřazené nebo běží s 25 10 úlohy Austrálie každý.</span><span class="sxs-lookup"><span data-stu-id="cd24b-219">For example, if this value is set to 250 AUs, you can have one job running with 250 AUs assigned to it, or 10 jobs running with 25 AUs each.</span></span> <span data-ttu-id="cd24b-220">Další úlohy, které jsou odeslány jsou zařazeny do fronty, dokud se všechny spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd24b-220">Additional jobs that are submitted are queued until the running jobs are finished.</span></span> <span data-ttu-id="cd24b-221">Po dokončení probíhající úlohy jsou Austrálie jsou uvolněny pro spuštění ve frontě úloh.</span><span class="sxs-lookup"><span data-stu-id="cd24b-221">When running jobs are finished, AUs are freed up for the queued jobs to run.</span></span>

<span data-ttu-id="cd24b-222">Chcete-li změnit počet Austrálie pro váš účet Data Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="cd24b-222">To change the number of AUs for your Data Lake Analytics account:</span></span>

1. <span data-ttu-id="cd24b-223">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-223">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-224">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-224">Click **Properties**.</span></span>
3. <span data-ttu-id="cd24b-225">V části **maximální Austrálie**, přesuňte jezdec vyberte hodnotu nebo zadejte hodnotu v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="cd24b-225">Under **Maximum AUs**, move the slider to select a value, or enter the value in the text box.</span></span> 
4. <span data-ttu-id="cd24b-226">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-226">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="cd24b-227">Pokud potřebujete více než výchozí (250) Austrálie, na portálu, klikněte na tlačítko **podpora + nápovědy** odeslat žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-227">If you need more than the default (250) AUs, in the portal, click **Help+Support** to submit a support request.</span></span> <span data-ttu-id="cd24b-228">Je možné zvýšit počet Austrálie k dispozici v účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-228">The number of AUs available in your Data Lake Analytics account can be increased.</span></span>
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a><span data-ttu-id="cd24b-229">Maximální počet úloh, které můžou běžet současně</span><span class="sxs-lookup"><span data-stu-id="cd24b-229">Maximum number of jobs that can run simultaneously</span></span>
<span data-ttu-id="cd24b-230">Zásady určuje, kolik úlohy můžete spustit ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-230">A policy controls how many jobs can run at the same time.</span></span> <span data-ttu-id="cd24b-231">Ve výchozím nastavení je tato hodnota nastavena na hodnotu 20.</span><span class="sxs-lookup"><span data-stu-id="cd24b-231">By default, this value is set to 20.</span></span> <span data-ttu-id="cd24b-232">Pokud vaše Data Lake Analytics má Austrálie k dispozici, nové úlohy jsou naplánovány ke spuštění až celkový počet spuštěných úloh nedosáhne hodnoty těchto zásad.</span><span class="sxs-lookup"><span data-stu-id="cd24b-232">If your Data Lake Analytics has AUs available, new jobs are scheduled to run immediately until the total number of running jobs reaches the value of this policy.</span></span> <span data-ttu-id="cd24b-233">Když se dostanete maximální počet úloh, které můžou běžet současně, následné úlohy jsou zařazeny do fronty v pořadí podle priority, dokud jeden nebo více spuštěné úlohy dokončení (v závislosti na dostupnosti AU).</span><span class="sxs-lookup"><span data-stu-id="cd24b-233">When you reach the maximum number of jobs that can run simultaneously, subsequent jobs are queued in priority order until one or more running jobs complete (depending on AU availability).</span></span>

<span data-ttu-id="cd24b-234">Chcete-li změnit počet úloh, které můžou běžet současně:</span><span class="sxs-lookup"><span data-stu-id="cd24b-234">To change the number of jobs that can run simultaneously:</span></span>

1. <span data-ttu-id="cd24b-235">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-235">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-236">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-236">Click **Properties**.</span></span>
3. <span data-ttu-id="cd24b-237">V části **maximální číslo z spuštění úlohy**, přesuňte jezdec vyberte hodnotu nebo zadejte hodnotu v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="cd24b-237">Under **Maximum Number of Running Jobs**, move the slider to select a value, or enter the value in the text box.</span></span> 
4. <span data-ttu-id="cd24b-238">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-238">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="cd24b-239">Pokud potřebujete více než výchozí (20) počet úloh, spusťte na portálu, klikněte na tlačítko **podpora + nápovědy** odeslat žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-239">If you need to run more than the default (20) number of jobs, in the portal, click **Help+Support** to submit a support request.</span></span> <span data-ttu-id="cd24b-240">Je možné zvýšit počet úloh, které můžou běžet současně v účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-240">The number of jobs that can run simultaneously in your Data Lake Analytics account can be increased.</span></span>
>

#### <a name="how-long-to-keep-job-metadata-and-resources"></a><span data-ttu-id="cd24b-241">Jak dlouho chcete zachovat metadata úlohy a prostředky</span><span class="sxs-lookup"><span data-stu-id="cd24b-241">How long to keep job metadata and resources</span></span> 
<span data-ttu-id="cd24b-242">Když uživatelé spustí úloh U-SQL, službě Data Lake Analytics uchovává všechny související soubory.</span><span class="sxs-lookup"><span data-stu-id="cd24b-242">When your users run U-SQL jobs, the Data Lake Analytics service retains all related files.</span></span> <span data-ttu-id="cd24b-243">Související soubory zahrnují skript U-SQL, soubory knihoven DLL, kterou se odkazuje v skript U-SQL, kompilované prostředků a statistiky.</span><span class="sxs-lookup"><span data-stu-id="cd24b-243">Related files include the U-SQL script, the DLL files referenced in the U-SQL script, compiled resources, and statistics.</span></span> <span data-ttu-id="cd24b-244">Soubory jsou ve složce /system/ výchozí účet Azure Data Lake Storage.</span><span class="sxs-lookup"><span data-stu-id="cd24b-244">The files are in the /system/ folder of the default Azure Data Lake Storage account.</span></span> <span data-ttu-id="cd24b-245">Tato zásada určuje, jak dlouho tyto prostředky jsou uložené před automaticky odstraní (výchozí hodnota je 30 dní).</span><span class="sxs-lookup"><span data-stu-id="cd24b-245">This policy controls how long these resources are stored before they are automatically deleted (the default is 30 days).</span></span> <span data-ttu-id="cd24b-246">Tyto soubory můžete použít pro ladění a optimalizace výkonu úloh, které budete v budoucnu spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-246">You can use these files for debugging, and for performance-tuning of jobs that you'll rerun in the future.</span></span>

<span data-ttu-id="cd24b-247">Chcete-li změnit jak dlouho Pokud chcete zachovat metadata úlohy a prostředky:</span><span class="sxs-lookup"><span data-stu-id="cd24b-247">To change how long to keep job metadata and resources:</span></span>

1. <span data-ttu-id="cd24b-248">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-248">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-249">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-249">Click **Properties**.</span></span>
3. <span data-ttu-id="cd24b-250">V části **dnů zachovat úloha se dotazuje**, přesuňte jezdec vyberte hodnotu nebo zadejte hodnotu v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="cd24b-250">Under **Days to Retain Job Queries**, move the slider to select a value, or enter the value in the text box.</span></span>  
4. <span data-ttu-id="cd24b-251">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-251">Click **Save**.</span></span>

### <a name="job-level-policies"></a><span data-ttu-id="cd24b-252">Zásady na úrovni úlohy</span><span class="sxs-lookup"><span data-stu-id="cd24b-252">Job-level policies</span></span>
<span data-ttu-id="cd24b-253">Pomocí zásad na úrovni úlohy můžete určit maximální Austrálie a maximální priority, kterou jednotlivé uživatele (nebo členy určité skupiny zabezpečení) můžete nastavit na úlohy, které se odesílají.</span><span class="sxs-lookup"><span data-stu-id="cd24b-253">With job-level policies, you can control the maximum AUs and the maximum priority that individual users (or members of specific security groups) can set on jobs that they submit.</span></span> <span data-ttu-id="cd24b-254">Díky tomu můžete řídit náklady uživatelé.</span><span class="sxs-lookup"><span data-stu-id="cd24b-254">This lets you control the costs incurred by users.</span></span> <span data-ttu-id="cd24b-255">To také umožňuje řízení o tom, že naplánované úlohy může mít na produkční s vysokou prioritou úloh, které jsou spuštěné ve stejném účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-255">It also lets you control the effect that scheduled jobs might have on high-priority production jobs that are running in the same Data Lake Analytics account.</span></span>

<span data-ttu-id="cd24b-256">Data Lake Analytics má dvě zásady, které můžete nastavit na úrovni úlohy:</span><span class="sxs-lookup"><span data-stu-id="cd24b-256">Data Lake Analytics has two policies that you can set at the job level:</span></span>

* <span data-ttu-id="cd24b-257">**AU limit na jednu úlohu**: uživatelé lze odeslat pouze úlohy, které je nutné tento počet Austrálie.</span><span class="sxs-lookup"><span data-stu-id="cd24b-257">**AU limit per job**: Users can only submit jobs that have up to this number of AUs.</span></span> <span data-ttu-id="cd24b-258">Ve výchozím nastavení tento limit je stejná jako maximální limit AU pro účet.</span><span class="sxs-lookup"><span data-stu-id="cd24b-258">By default, this limit is the same as the maximum AU limit for the account.</span></span>
* <span data-ttu-id="cd24b-259">**Priorita**: uživatelé lze odeslat pouze úlohy, které mají nižší než nebo rovna hodnotě tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-259">**Priority**: Users can only submit jobs that have a priority lower than or equal to this value.</span></span> <span data-ttu-id="cd24b-260">Všimněte si, že vyšší číslo znamená s nižší prioritou.</span><span class="sxs-lookup"><span data-stu-id="cd24b-260">Note that a higher number means a lower priority.</span></span> <span data-ttu-id="cd24b-261">Ve výchozím nastavení to je nastavena na hodnotu 1, což je nejvyšší možná priorita.</span><span class="sxs-lookup"><span data-stu-id="cd24b-261">By default, this is set to 1, which is the highest possible priority.</span></span>

<span data-ttu-id="cd24b-262">Existuje výchozí zásada, nastavte pro každý účet.</span><span class="sxs-lookup"><span data-stu-id="cd24b-262">There is a default policy set on every account.</span></span> <span data-ttu-id="cd24b-263">Výchozí zásady platí pro všechny uživatele účtu.</span><span class="sxs-lookup"><span data-stu-id="cd24b-263">The default policy applies to all users of the account.</span></span> <span data-ttu-id="cd24b-264">Další zásady můžete nastavit pro konkrétní uživatele a skupiny.</span><span class="sxs-lookup"><span data-stu-id="cd24b-264">You can set additional policies for specific users and groups.</span></span> 

> [!NOTE]
> <span data-ttu-id="cd24b-265">Zásady na úrovni účtu a zásady na úrovni úlohy použít současně.</span><span class="sxs-lookup"><span data-stu-id="cd24b-265">Account-level policies and job-level policies apply simultaneously.</span></span>
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a><span data-ttu-id="cd24b-266">Přidání zásad pro konkrétního uživatele nebo skupiny</span><span class="sxs-lookup"><span data-stu-id="cd24b-266">Add a policy for a specific user or group</span></span>

1. <span data-ttu-id="cd24b-267">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-267">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-268">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-268">Click **Properties**.</span></span>
3. <span data-ttu-id="cd24b-269">V části **úlohy odeslání omezení**, klikněte **přidat zásadu** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd24b-269">Under **Job Submission Limits**, click the **Add Policy** button.</span></span> <span data-ttu-id="cd24b-270">Potom vyberte nebo zadejte následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="cd24b-270">Then, select or enter the following settings:</span></span>
    1. <span data-ttu-id="cd24b-271">**Název zásady výpočetní**: Zadejte název zásady, a tak poznáte, účelu zásad.</span><span class="sxs-lookup"><span data-stu-id="cd24b-271">**Compute Policy Name**: Enter a policy name, to remind you of the purpose of the policy.</span></span>
    2. <span data-ttu-id="cd24b-272">**Vyberte uživatele nebo skupiny**: Vyberte uživatele nebo skupiny, tato zásada se vztahuje na.</span><span class="sxs-lookup"><span data-stu-id="cd24b-272">**Select User or Group**: Select the user or group this policy applies to.</span></span>
    3. <span data-ttu-id="cd24b-273">**Nastavit Limit AU úlohy**: omezit AU, která se vztahuje na vybrané uživatele nebo skupiny.</span><span class="sxs-lookup"><span data-stu-id="cd24b-273">**Set the Job AU Limit**: Set the AU limit that applies to the selected user or group.</span></span>
    4. <span data-ttu-id="cd24b-274">**Nastavit Priority Limit**: Omezit prioritu, která se vztahuje na vybrané uživatele nebo skupiny.</span><span class="sxs-lookup"><span data-stu-id="cd24b-274">**Set the Priority Limit**: Set the priority limit that applies to the selected user or group.</span></span>

4. <span data-ttu-id="cd24b-275">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-275">Click **Ok**.</span></span>

5. <span data-ttu-id="cd24b-276">Nové zásady, je uvedena ve **výchozí** zásad v části tabulky **úlohy odeslání omezení**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-276">The new policy is listed in the **Default** policy table, under **Job Submission Limits**.</span></span> 

#### <a name="delete-or-edit-an-existing-policy"></a><span data-ttu-id="cd24b-277">Odstraňte nebo upravte existující zásady</span><span class="sxs-lookup"><span data-stu-id="cd24b-277">Delete or edit an existing policy</span></span>

1. <span data-ttu-id="cd24b-278">V portálu Azure přejděte do účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cd24b-278">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="cd24b-279">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="cd24b-279">Click **Properties**.</span></span>
3. <span data-ttu-id="cd24b-280">V části **úlohy odeslání omezení**, vyhledejte zásadu, kterou chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="cd24b-280">Under **Job Submission Limits**, find the policy you want to edit.</span></span>
4.  <span data-ttu-id="cd24b-281">Chcete-li zobrazit **odstranit** a **upravit** možnosti v polí v pravém sloupci tabulky, klikněte na tlačítko **...** .</span><span class="sxs-lookup"><span data-stu-id="cd24b-281">To see the **Delete** and **Edit** options, in the rightmost column of the table, click **...**.</span></span>

### <a name="additional-resources-for-job-policies"></a><span data-ttu-id="cd24b-282">Další prostředky pro úlohy zásady</span><span class="sxs-lookup"><span data-stu-id="cd24b-282">Additional resources for job policies</span></span>
* [<span data-ttu-id="cd24b-283">Příspěvek blogu přehled zásad</span><span class="sxs-lookup"><span data-stu-id="cd24b-283">Policy overview blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [<span data-ttu-id="cd24b-284">Zásady na úrovni účtu příspěvku na blogu</span><span class="sxs-lookup"><span data-stu-id="cd24b-284">Account-level policies blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [<span data-ttu-id="cd24b-285">Zásady na úrovni úlohy příspěvku na blogu</span><span class="sxs-lookup"><span data-stu-id="cd24b-285">Job-level policies blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a><span data-ttu-id="cd24b-286">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd24b-286">Next steps</span></span>

* [<span data-ttu-id="cd24b-287">Přehled Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cd24b-287">Overview of Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="cd24b-288">Začínáme s Data Lake Analytics pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cd24b-288">Get started with Data Lake Analytics by using the Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="cd24b-289">Správa Azure Data Lake Analytics pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cd24b-289">Manage Azure Data Lake Analytics by using Azure PowerShell</span></span>](data-lake-analytics-manage-use-powershell.md)

