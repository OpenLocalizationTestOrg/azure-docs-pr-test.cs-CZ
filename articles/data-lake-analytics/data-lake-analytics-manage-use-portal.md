---
title: "hello aaaManage Azure Data Lake Analytics pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toomanage počtech Data Lake Analytics, datové zdroje, uživatelé a úlohy."
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
ms.openlocfilehash: f63ccdfae79772c92e92462194e8cdc636a73dc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-by-using-hello-azure-portal"></a><span data-ttu-id="843aa-103">Správa Azure Data Lake Analytics pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="843aa-103">Manage Azure Data Lake Analytics by using hello Azure portal</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="843aa-104">Zjistěte, jak hello toomanage účtů Azure Data Lake Analytics, účet zdroje dat, uživatele a úlohy pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="843aa-104">Learn how toomanage Azure Data Lake Analytics accounts, account data sources, users, and jobs by using hello Azure portal.</span></span> <span data-ttu-id="843aa-105">témata týkající se řízení toosee o pomocí jiných nástrojů, klikněte na karty v horní části hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="843aa-105">toosee management topics about using other tools, click a tab at hello top of hello page.</span></span>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a><span data-ttu-id="843aa-106">Správa účtů Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="843aa-106">Manage Data Lake Analytics accounts</span></span>

### <a name="create-an-account"></a><span data-ttu-id="843aa-107">Vytvoření účtu</span><span class="sxs-lookup"><span data-stu-id="843aa-107">Create an account</span></span>

1. <span data-ttu-id="843aa-108">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="843aa-108">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="843aa-109">Klikněte na **Nový** > **Inteligentní funkce a analýzy** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="843aa-109">Click **New** > **Intelligence + analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="843aa-110">Vyberte hodnoty pro hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="843aa-110">Select values for hello following items:</span></span> 
   1. <span data-ttu-id="843aa-111">**Název**: název hello hello účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-111">**Name**: hello name of hello Data Lake Analytics account.</span></span>
   2. <span data-ttu-id="843aa-112">**Předplatné**: hello předplatné Azure použité pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-112">**Subscription**: hello Azure subscription used for hello account.</span></span>
   3. <span data-ttu-id="843aa-113">**Skupina prostředků**: Skupina prostředků Azure hello v účtu, který toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-113">**Resource Group**: hello Azure resource group in which toocreate hello account.</span></span> 
   4. <span data-ttu-id="843aa-114">**Umístění**: hello datové centrum Azure pro účet Data Lake Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-114">**Location**: hello Azure datacenter for hello Data Lake Analytics account.</span></span> 
   5. <span data-ttu-id="843aa-115">**Data Lake Store**: hello výchozí úložiště toobe použité pro účet Data Lake Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-115">**Data Lake Store**: hello default store toobe used for hello Data Lake Analytics account.</span></span> <span data-ttu-id="843aa-116">účet Azure Data Lake Store Hello a hello účet musí být v Data Lake Analytics hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="843aa-116">hello Azure Data Lake Store account and hello Data Lake Analytics account must be in hello same location.</span></span>
4. <span data-ttu-id="843aa-117">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="843aa-117">Click **Create**.</span></span> 

### <a name="delete-a-data-lake-analytics-account"></a><span data-ttu-id="843aa-118">Odstranění účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="843aa-118">Delete a Data Lake Analytics account</span></span>

<span data-ttu-id="843aa-119">Před odstraněním účtu Data Lake Analytics, odstraňte jeho výchozí účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="843aa-119">Before you delete a Data Lake Analytics account, delete its default Data Lake Store account.</span></span>

1. <span data-ttu-id="843aa-120">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-120">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-121">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="843aa-121">Click **Delete**.</span></span>
3. <span data-ttu-id="843aa-122">Zadejte název účtu hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-122">Type hello account name.</span></span>
4. <span data-ttu-id="843aa-123">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="843aa-123">Click **Delete**.</span></span>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a><span data-ttu-id="843aa-124">Správa zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="843aa-124">Manage data sources</span></span>

<span data-ttu-id="843aa-125">Data Lake Analytics podporuje hello následující zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="843aa-125">Data Lake Analytics supports hello following data sources:</span></span>

* <span data-ttu-id="843aa-126">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="843aa-126">Data Lake Store</span></span>
* <span data-ttu-id="843aa-127">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="843aa-127">Azure Storage</span></span>

<span data-ttu-id="843aa-128">Můžete použít zdroje dat toobrowse Průzkumníku dat a provádět operace správy základního souboru.</span><span class="sxs-lookup"><span data-stu-id="843aa-128">You can use Data Explorer toobrowse data sources and perform basic file management operations.</span></span> 

### <a name="add-a-data-source"></a><span data-ttu-id="843aa-129">Přidání zdroje dat</span><span class="sxs-lookup"><span data-stu-id="843aa-129">Add a data source</span></span>

1. <span data-ttu-id="843aa-130">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-130">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-131">Klikněte na tlačítko **zdroje dat**.</span><span class="sxs-lookup"><span data-stu-id="843aa-131">Click **Data Sources**.</span></span>
3. <span data-ttu-id="843aa-132">Klikněte na tlačítko **přidat zdroj dat**.</span><span class="sxs-lookup"><span data-stu-id="843aa-132">Click **Add Data Source**.</span></span>
    
   * <span data-ttu-id="843aa-133">tooadd účtu Data Lake Store, potřebujete účet hello názvem a přístupovým toohello účet toobe možné tooquery ho.</span><span class="sxs-lookup"><span data-stu-id="843aa-133">tooadd a Data Lake Store account, you need hello account name and access toohello account toobe able tooquery it.</span></span>
   * <span data-ttu-id="843aa-134">tooadd úložiště objektů Blob v Azure, potřebujete účet úložiště hello a klíč účtu hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-134">tooadd Azure Blob storage, you need hello storage account and hello account key.</span></span> <span data-ttu-id="843aa-135">toofind účtu úložiště toohello je, přejděte na portál hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-135">toofind them, go toohello storage account in hello portal.</span></span>

## <a name="set-up-firewall-rules"></a><span data-ttu-id="843aa-136">Nastavení pravidel brány firewall</span><span class="sxs-lookup"><span data-stu-id="843aa-136">Set up firewall rules</span></span>

<span data-ttu-id="843aa-137">Data Lake Analytics toofurther uzamčení tooyour přístup k účtu Data Lake Analytics můžete použít na úrovni sítě hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-137">You can use Data Lake Analytics toofurther lock down access tooyour Data Lake Analytics account at hello network level.</span></span> <span data-ttu-id="843aa-138">Můžete povolit bránu firewall, zadejte IP adresu nebo zadejte rozsah IP adres pro klienty důvěryhodné.</span><span class="sxs-lookup"><span data-stu-id="843aa-138">You can enable a firewall, specify an IP address, or define an IP address range for your trusted clients.</span></span> <span data-ttu-id="843aa-139">Povolíte-li tato opatření, můžete připojit pouze klienti, kteří mají hello IP adresy v rozsahu hello definované toohello úložiště.</span><span class="sxs-lookup"><span data-stu-id="843aa-139">After you enable these measures, only clients that have hello IP addresses within hello defined range can connect toohello store.</span></span>

<span data-ttu-id="843aa-140">Pokud jinými službami Azure, jako je Azure Data Factory nebo virtuální počítače, připojit toohello účet Data Lake Analytics, ujistěte se, že **povolit služby Azure** je zapnuta **na**.</span><span class="sxs-lookup"><span data-stu-id="843aa-140">If other Azure services, like Azure Data Factory or VMs, connect toohello Data Lake Analytics account, make sure that **Allow Azure Services** is turned **On**.</span></span> 

### <a name="set-up-a-firewall-rule"></a><span data-ttu-id="843aa-141">Nastavit pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="843aa-141">Set up a firewall rule</span></span>

1. <span data-ttu-id="843aa-142">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-142">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-143">V nabídce hello na levé straně hello, klikněte na tlačítko **brány Firewall**.</span><span class="sxs-lookup"><span data-stu-id="843aa-143">On hello menu on hello left, click **Firewall**.</span></span>

## <a name="add-a-new-user"></a><span data-ttu-id="843aa-144">Přidání nového uživatele</span><span class="sxs-lookup"><span data-stu-id="843aa-144">Add a new user</span></span>

<span data-ttu-id="843aa-145">Můžete použít hello **Průvodce přidáním uživatele** tooeasily zřídit nové uživatele Data Lake.</span><span class="sxs-lookup"><span data-stu-id="843aa-145">You can use hello **Add User Wizard** tooeasily provision new Data Lake users.</span></span>

1. <span data-ttu-id="843aa-146">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-146">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-147">Na levé části hello **Začínáme**, klikněte na tlačítko **Průvodce přidáním uživatele**.</span><span class="sxs-lookup"><span data-stu-id="843aa-147">On hello left, under **Getting Started**, click **Add User Wizard**.</span></span>
3. <span data-ttu-id="843aa-148">Vyberte uživatele a pak klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="843aa-148">Select a user, and then click **Select**.</span></span>
4. <span data-ttu-id="843aa-149">Vyberte roli a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="843aa-149">Select a role, and then click **Select**.</span></span> <span data-ttu-id="843aa-150">tooset si nové toouse vývojáře Azure Data Lake, vyberte hello **Data Lake Analytics vývojáře** role.</span><span class="sxs-lookup"><span data-stu-id="843aa-150">tooset up a new developer toouse Azure Data Lake, select hello **Data Lake Analytics Developer** role.</span></span>
5. <span data-ttu-id="843aa-151">Vyberte seznamy řízení přístupu (ACL) hello databází hello U-SQL.</span><span class="sxs-lookup"><span data-stu-id="843aa-151">Select hello access control lists (ACLs) for hello U-SQL databases.</span></span> <span data-ttu-id="843aa-152">Až budete spokojeni s vaší volby, klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="843aa-152">When you're satisfied with your choices, click **Select**.</span></span>
6. <span data-ttu-id="843aa-153">Vyberte hello seznamy řízení přístupu pro soubory.</span><span class="sxs-lookup"><span data-stu-id="843aa-153">Select hello ACLs for files.</span></span> <span data-ttu-id="843aa-154">Pro výchozí úložiště hello, neměnit hello seznamy ACL pro kořenovou složku hello "/" a ve složce/System hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-154">For hello default store, don't change hello ACLs for hello root folder "/" and for hello /system folder.</span></span> <span data-ttu-id="843aa-155">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="843aa-155">Click **Select**.</span></span>
7. <span data-ttu-id="843aa-156">Zkontrolujte všechny vybrané změny a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="843aa-156">Review all your selected changes, and then click **Run**.</span></span>
8. <span data-ttu-id="843aa-157">Po dokončení Průvodce hello klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="843aa-157">When hello wizard is finished, click **Done**.</span></span>

## <a name="manage-role-based-access-control"></a><span data-ttu-id="843aa-158">Správa řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="843aa-158">Manage Role-Based Access Control</span></span>

<span data-ttu-id="843aa-159">Jako jinými službami Azure můžete použít toocontrol řízení přístupu na základě Role (RBAC), jak uživatelé pracují se službou hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-159">Like other Azure services, you can use Role-Based Access Control (RBAC) toocontrol how users interact with hello service.</span></span>

<span data-ttu-id="843aa-160">standardní role RBAC Hello mít hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="843aa-160">hello standard RBAC roles have hello following capabilities:</span></span>
* <span data-ttu-id="843aa-161">**Vlastník**: můžete odeslat úlohy sledování úloh, zrušit úlohy z libovolného uživatele a nakonfigurovat účet hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-161">**Owner**: Can submit jobs, monitor jobs, cancel jobs from any user, and configure hello account.</span></span>
* <span data-ttu-id="843aa-162">**Přispěvatel**: můžete odeslat úlohy sledování úloh, zrušit úlohy z libovolného uživatele a nakonfigurovat účet hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-162">**Contributor**: Can submit jobs, monitor jobs, cancel jobs from any user, and configure hello account.</span></span>
* <span data-ttu-id="843aa-163">**Čtečka**: můžete sledovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="843aa-163">**Reader**: Can monitor jobs.</span></span>

<span data-ttu-id="843aa-164">Pomocí hello Data Lake Analytics vývojáře role tooenable U-SQL vývojáři toouse hello Data Lake Analytics služby.</span><span class="sxs-lookup"><span data-stu-id="843aa-164">Use hello Data Lake Analytics Developer role tooenable U-SQL developers toouse hello Data Lake Analytics service.</span></span> <span data-ttu-id="843aa-165">Můžete použít role Data Lake Analytics vývojáře hello:</span><span class="sxs-lookup"><span data-stu-id="843aa-165">You can use hello Data Lake Analytics Developer role to:</span></span>
* <span data-ttu-id="843aa-166">Odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="843aa-166">Submit jobs.</span></span>
* <span data-ttu-id="843aa-167">Monitorování úlohy stavu a hello průběh úlohy, odeslané žádný uživatel.</span><span class="sxs-lookup"><span data-stu-id="843aa-167">Monitor job status and hello progress of jobs submitted by any user.</span></span>
* <span data-ttu-id="843aa-168">V tématu hello skriptů U-SQL z úlohy, odeslané žádný uživatel.</span><span class="sxs-lookup"><span data-stu-id="843aa-168">See hello U-SQL scripts from jobs submitted by any user.</span></span>
* <span data-ttu-id="843aa-169">Zrušte jenom vlastní úlohy.</span><span class="sxs-lookup"><span data-stu-id="843aa-169">Cancel only your own jobs.</span></span>

### <a name="add-users-or-security-groups-tooa-data-lake-analytics-account"></a><span data-ttu-id="843aa-170">Přidejte uživatele nebo skupiny tooa zabezpečení účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="843aa-170">Add users or security groups tooa Data Lake Analytics account</span></span>

1. <span data-ttu-id="843aa-171">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-171">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-172">Klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="843aa-172">Click **Access control (IAM)** > **Add**.</span></span>
3. <span data-ttu-id="843aa-173">Vyberte roli.</span><span class="sxs-lookup"><span data-stu-id="843aa-173">Select a role.</span></span>
4. <span data-ttu-id="843aa-174">Přidání uživatele.</span><span class="sxs-lookup"><span data-stu-id="843aa-174">Add a user.</span></span>
5. <span data-ttu-id="843aa-175">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="843aa-175">Click **OK**.</span></span>

>[!NOTE]
><span data-ttu-id="843aa-176">Pokud uživatel nebo skupina zabezpečení potřebuje toosubmit úlohy, budou také potřebovat oprávnění na účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-176">If a user or a security group needs toosubmit jobs, they also need permission on hello store account.</span></span> <span data-ttu-id="843aa-177">Další informace najdete v tématu [zabezpečení dat uložených v Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).</span><span class="sxs-lookup"><span data-stu-id="843aa-177">For more information, see [Secure data stored in Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).</span></span>
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a><span data-ttu-id="843aa-178">Správa úloh</span><span class="sxs-lookup"><span data-stu-id="843aa-178">Manage jobs</span></span>

### <a name="submit-a-job"></a><span data-ttu-id="843aa-179">Odeslání úlohy</span><span class="sxs-lookup"><span data-stu-id="843aa-179">Submit a job</span></span>

1. <span data-ttu-id="843aa-180">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-180">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>

2. <span data-ttu-id="843aa-181">Klikněte na tlačítko **nová úloha**.</span><span class="sxs-lookup"><span data-stu-id="843aa-181">Click **New Job**.</span></span> <span data-ttu-id="843aa-182">Pro každou úlohu konfigurace:</span><span class="sxs-lookup"><span data-stu-id="843aa-182">For each job,  configure:</span></span>

    1. <span data-ttu-id="843aa-183">**Název úlohy**: název hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="843aa-183">**Job Name**: hello name of hello job.</span></span>
    2. <span data-ttu-id="843aa-184">**Priorita**: nižší čísla mají vyšší prioritu.</span><span class="sxs-lookup"><span data-stu-id="843aa-184">**Priority**: Lower numbers have higher priority.</span></span> <span data-ttu-id="843aa-185">Pokud dvě úlohy jsou zařazeny do fronty, spustí se první hello, jeden s nižší hodnotou priority.</span><span class="sxs-lookup"><span data-stu-id="843aa-185">If two jobs are queued, hello one with lower priority value runs first.</span></span>
    3. <span data-ttu-id="843aa-186">**Paralelismus**: hello maximální počet výpočetních procesů tooreserve pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="843aa-186">**Parallelism**: hello maximum number of compute processes tooreserve for this job.</span></span>

3. <span data-ttu-id="843aa-187">Klikněte na **Odeslat úlohu**.</span><span class="sxs-lookup"><span data-stu-id="843aa-187">Click **Submit Job**.</span></span>

### <a name="monitor-jobs"></a><span data-ttu-id="843aa-188">Monitorování úloh</span><span class="sxs-lookup"><span data-stu-id="843aa-188">Monitor jobs</span></span>

1. <span data-ttu-id="843aa-189">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-189">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-190">Klikněte na tlačítko **vidět všechny úlohy**.</span><span class="sxs-lookup"><span data-stu-id="843aa-190">Click **View All Jobs**.</span></span> <span data-ttu-id="843aa-191">Se zobrazí seznam všech hello aktivní a nedávno dokončení úloh v účtu hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-191">A list of all hello active and recently finished jobs in hello account is shown.</span></span>
3. <span data-ttu-id="843aa-192">Volitelně klikněte na **filtru** toohelp najít hello úloh podle **časový rozsah**, **název úlohy**, a **Autor** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="843aa-192">Optionally, click **Filter** toohelp you find hello jobs by **Time Range**, **Job Name**, and **Author** values.</span></span> 

### <a name="monitoring-pipeline-jobs"></a><span data-ttu-id="843aa-193">Sledování úloh kanálu</span><span class="sxs-lookup"><span data-stu-id="843aa-193">Monitoring pipeline jobs</span></span>
<span data-ttu-id="843aa-194">Úlohy, které jsou součástí kanálu fungují společně, obvykle postupně tooaccomplish na konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="843aa-194">Jobs that are part of a pipeline work together, usually sequentially, tooaccomplish a specific scenario.</span></span> <span data-ttu-id="843aa-195">Například můžete mít kanál, který odstraní, extrahuje, transformuje, agreguje využití přehledů zákazníka.</span><span class="sxs-lookup"><span data-stu-id="843aa-195">For example, you can have a pipeline that cleans, extracts, transforms, aggregates usage for customer insights.</span></span> <span data-ttu-id="843aa-196">Úlohy kanálu se identifikují pomocí vlastnosti "Kanál" hello, pokud hello úloha byla odeslána.</span><span class="sxs-lookup"><span data-stu-id="843aa-196">Pipeline jobs are identified using hello "Pipeline" property when hello job was submitted.</span></span> <span data-ttu-id="843aa-197">Pomocí ADF V2 naplánované úlohy mít tato vlastnost vyplní automaticky.</span><span class="sxs-lookup"><span data-stu-id="843aa-197">Jobs scheduled using ADF V2 will automatically have this property populated.</span></span> 

<span data-ttu-id="843aa-198">tooview seznam úloh U-SQL, které jsou součástí kanály:</span><span class="sxs-lookup"><span data-stu-id="843aa-198">tooview a list of U-SQL jobs that are part of pipelines:</span></span> 

1. <span data-ttu-id="843aa-199">V hello portálu Azure přejděte tooyour účtů Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-199">In hello Azure portal, go tooyour Data Lake Analytics accounts.</span></span>
2. <span data-ttu-id="843aa-200">Klikněte na tlačítko **úlohy Insights**.</span><span class="sxs-lookup"><span data-stu-id="843aa-200">Click **Job Insights**.</span></span> <span data-ttu-id="843aa-201">Hello "Všechny úlohy" karta bude uvedena, zobrazující seznam spuštění ve frontě a skončila úlohy.</span><span class="sxs-lookup"><span data-stu-id="843aa-201">hello "All Jobs" tab will be defaulted, showing a list of running, queued, and ended jobs.</span></span>
3. <span data-ttu-id="843aa-202">Klikněte na tlačítko hello **kanálu úlohy** kartě. Zobrazí se seznam úloh kanálu spolu s souhrnných statistik pro každý kanál.</span><span class="sxs-lookup"><span data-stu-id="843aa-202">Click hello **Pipeline Jobs** tab. A list of pipeline jobs will be shown along with aggregated statistics for each pipeline.</span></span>

### <a name="monitoring-recurring-jobs"></a><span data-ttu-id="843aa-203">Monitorování opakované úlohy</span><span class="sxs-lookup"><span data-stu-id="843aa-203">Monitoring recurring jobs</span></span>
<span data-ttu-id="843aa-204">Opakování úlohy je ten, který má hello stejné obchodní logiku, ale používá jiné vstupní data pokaždé, když ji spustí.</span><span class="sxs-lookup"><span data-stu-id="843aa-204">A recurring job is one that has hello same business logic but uses different input data every time it runs.</span></span> <span data-ttu-id="843aa-205">V ideálním případě opakované úlohy doporučujeme vždy úspěšné a mají relativně stabilní čas provádění; monitorování těchto chování pomůže zajistit, že úloha hello je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="843aa-205">Ideally, recurring jobs should always succeed, and have relatively stable execution time; monitoring these behaviors will help ensure hello job is healthy.</span></span> <span data-ttu-id="843aa-206">Opakované úlohy se identifikují pomocí vlastnosti "Recurrence" hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-206">Recurring jobs are identified using hello "Recurrence" property.</span></span> <span data-ttu-id="843aa-207">Pomocí ADF V2 naplánované úlohy mít tato vlastnost vyplní automaticky.</span><span class="sxs-lookup"><span data-stu-id="843aa-207">Jobs scheduled using ADF V2 will automatically have this property populated.</span></span>

<span data-ttu-id="843aa-208">seznam úloh U-SQL, které jsou opakovaného tooview:</span><span class="sxs-lookup"><span data-stu-id="843aa-208">tooview a list of U-SQL jobs that are recurring:</span></span> 

1. <span data-ttu-id="843aa-209">V hello portálu Azure přejděte tooyour účtů Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-209">In hello Azure portal, go tooyour Data Lake Analytics accounts.</span></span>
2. <span data-ttu-id="843aa-210">Klikněte na tlačítko **úlohy Insights**.</span><span class="sxs-lookup"><span data-stu-id="843aa-210">Click **Job Insights**.</span></span> <span data-ttu-id="843aa-211">Hello "Všechny úlohy" karta bude uvedena, zobrazující seznam spuštění ve frontě a skončila úlohy.</span><span class="sxs-lookup"><span data-stu-id="843aa-211">hello "All Jobs" tab will be defaulted, showing a list of running, queued, and ended jobs.</span></span>
3. <span data-ttu-id="843aa-212">Klikněte na tlačítko hello **opakovaných úloh** kartě. Společně s souhrnných statistik pro každou úlohu opakující se zobrazí seznam opakované úlohy.</span><span class="sxs-lookup"><span data-stu-id="843aa-212">Click hello **Recurring Jobs** tab. A list of recurring jobs will be shown along with aggregated statistics for each recurring job.</span></span>

## <a name="manage-policies"></a><span data-ttu-id="843aa-213">Správa zásad</span><span class="sxs-lookup"><span data-stu-id="843aa-213">Manage policies</span></span>

### <a name="account-level-policies"></a><span data-ttu-id="843aa-214">Zásady na úrovni účtu</span><span class="sxs-lookup"><span data-stu-id="843aa-214">Account-level policies</span></span>

<span data-ttu-id="843aa-215">Tyto zásady použít tooall úloh v účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-215">These policies apply tooall jobs in a Data Lake Analytics account.</span></span>

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a><span data-ttu-id="843aa-216">Maximální počet Austrálie v účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="843aa-216">Maximum number of AUs in a Data Lake Analytics account</span></span>
<span data-ttu-id="843aa-217">Zásady řídí hello celkový počet jednotek Analytics (Austrálie) můžete použít váš účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-217">A policy controls hello total number of Analytics Units (AUs) your Data Lake Analytics account can use.</span></span> <span data-ttu-id="843aa-218">Ve výchozím nastavení je nastavena hodnota hello too250.</span><span class="sxs-lookup"><span data-stu-id="843aa-218">By default, hello value is set too250.</span></span> <span data-ttu-id="843aa-219">Například pokud je tato hodnota nastavená too250 Austrálie, vám může mít jednu úlohu s 250 tooit Austrálie přiřazené nebo běží s 25 10 úlohy Austrálie každý.</span><span class="sxs-lookup"><span data-stu-id="843aa-219">For example, if this value is set too250 AUs, you can have one job running with 250 AUs assigned tooit, or 10 jobs running with 25 AUs each.</span></span> <span data-ttu-id="843aa-220">Další úlohy, které jsou odeslány jsou zařazeny do fronty, dokud se všechny spuštěné úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-220">Additional jobs that are submitted are queued until hello running jobs are finished.</span></span> <span data-ttu-id="843aa-221">Po dokončení probíhající úlohy jsou Austrálie jsou uvolněny pro hello zařazených do fronty úloh toorun.</span><span class="sxs-lookup"><span data-stu-id="843aa-221">When running jobs are finished, AUs are freed up for hello queued jobs toorun.</span></span>

<span data-ttu-id="843aa-222">toochange hello počet Austrálie pro váš účet Data Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="843aa-222">toochange hello number of AUs for your Data Lake Analytics account:</span></span>

1. <span data-ttu-id="843aa-223">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-223">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-224">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="843aa-224">Click **Properties**.</span></span>
3. <span data-ttu-id="843aa-225">V části **maximální Austrálie**, přesuňte jezdec tooselect hello hodnotu nebo zadejte hodnotu hello hello textového pole.</span><span class="sxs-lookup"><span data-stu-id="843aa-225">Under **Maximum AUs**, move hello slider tooselect a value, or enter hello value in hello text box.</span></span> 
4. <span data-ttu-id="843aa-226">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="843aa-226">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="843aa-227">Pokud budete potřebovat víc než hello výchozí (250) Austrálie, hello portálu, klikněte na tlačítko **podpora + nápovědy** toosubmit žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="843aa-227">If you need more than hello default (250) AUs, in hello portal, click **Help+Support** toosubmit a support request.</span></span> <span data-ttu-id="843aa-228">je možné zvýšit počet Hello Austrálie k dispozici v účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-228">hello number of AUs available in your Data Lake Analytics account can be increased.</span></span>
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a><span data-ttu-id="843aa-229">Maximální počet úloh, které můžou běžet současně</span><span class="sxs-lookup"><span data-stu-id="843aa-229">Maximum number of jobs that can run simultaneously</span></span>
<span data-ttu-id="843aa-230">Zásady řídí, kolik úlohy můžete spustit na hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="843aa-230">A policy controls how many jobs can run at hello same time.</span></span> <span data-ttu-id="843aa-231">Ve výchozím nastavení je tato hodnota nastavena too20.</span><span class="sxs-lookup"><span data-stu-id="843aa-231">By default, this value is set too20.</span></span> <span data-ttu-id="843aa-232">Pokud vaše Data Lake Analytics má Austrálie k dispozici, nové úlohy jsou naplánované toorun až hello celkový počet spuštěných úloh nedosáhne hello hodnoty těchto zásad.</span><span class="sxs-lookup"><span data-stu-id="843aa-232">If your Data Lake Analytics has AUs available, new jobs are scheduled toorun immediately until hello total number of running jobs reaches hello value of this policy.</span></span> <span data-ttu-id="843aa-233">Když se dostanete hello maximální počet úloh, které můžou běžet současně, následné úlohy jsou zařazeny do fronty v pořadí podle priority, dokud jeden nebo více spuštěné úlohy dokončení (v závislosti na dostupnosti AU).</span><span class="sxs-lookup"><span data-stu-id="843aa-233">When you reach hello maximum number of jobs that can run simultaneously, subsequent jobs are queued in priority order until one or more running jobs complete (depending on AU availability).</span></span>

<span data-ttu-id="843aa-234">toochange hello počet úloh, které můžou běžet současně:</span><span class="sxs-lookup"><span data-stu-id="843aa-234">toochange hello number of jobs that can run simultaneously:</span></span>

1. <span data-ttu-id="843aa-235">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-235">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-236">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="843aa-236">Click **Properties**.</span></span>
3. <span data-ttu-id="843aa-237">V části **maximální číslo z spuštění úlohy**, přesuňte jezdec tooselect hello hodnotu nebo zadejte hodnotu hello hello textového pole.</span><span class="sxs-lookup"><span data-stu-id="843aa-237">Under **Maximum Number of Running Jobs**, move hello slider tooselect a value, or enter hello value in hello text box.</span></span> 
4. <span data-ttu-id="843aa-238">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="843aa-238">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="843aa-239">Pokud potřebujete více než hello výchozí (20) počet úloh, hello portálu, klikněte na tlačítko toorun **podpora + nápovědy** toosubmit žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="843aa-239">If you need toorun more than hello default (20) number of jobs, in hello portal, click **Help+Support** toosubmit a support request.</span></span> <span data-ttu-id="843aa-240">může být zvýšena Hello počet úloh, které můžou běžet současně v účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-240">hello number of jobs that can run simultaneously in your Data Lake Analytics account can be increased.</span></span>
>

#### <a name="how-long-tookeep-job-metadata-and-resources"></a><span data-ttu-id="843aa-241">Jak dlouho metadata tookeep úlohy a prostředky</span><span class="sxs-lookup"><span data-stu-id="843aa-241">How long tookeep job metadata and resources</span></span> 
<span data-ttu-id="843aa-242">Když uživatelé spustí úloh U-SQL, hello služby Data Lake Analytics uchovává všechny související soubory.</span><span class="sxs-lookup"><span data-stu-id="843aa-242">When your users run U-SQL jobs, hello Data Lake Analytics service retains all related files.</span></span> <span data-ttu-id="843aa-243">Související soubory zahrnují hello U-SQL skriptu, soubory knihoven DLL hello odkazovaný ve skriptu hello U-SQL, kompilované prostředky a statistiky.</span><span class="sxs-lookup"><span data-stu-id="843aa-243">Related files include hello U-SQL script, hello DLL files referenced in hello U-SQL script, compiled resources, and statistics.</span></span> <span data-ttu-id="843aa-244">Hello soubory jsou ve složce /system/ hello hello výchozí účet Azure Data Lake Storage.</span><span class="sxs-lookup"><span data-stu-id="843aa-244">hello files are in hello /system/ folder of hello default Azure Data Lake Storage account.</span></span> <span data-ttu-id="843aa-245">Tato zásada určuje, jak dlouho tyto prostředky jsou uložené před automaticky odstraní (hello výchozí hodnota je 30 dní).</span><span class="sxs-lookup"><span data-stu-id="843aa-245">This policy controls how long these resources are stored before they are automatically deleted (hello default is 30 days).</span></span> <span data-ttu-id="843aa-246">Tyto soubory můžete použít pro ladění a optimalizace výkonu úloh, které budete znovu spustit v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-246">You can use these files for debugging, and for performance-tuning of jobs that you'll rerun in hello future.</span></span>

<span data-ttu-id="843aa-247">toochange jak dlouho metadata tookeep úlohy a prostředky:</span><span class="sxs-lookup"><span data-stu-id="843aa-247">toochange how long tookeep job metadata and resources:</span></span>

1. <span data-ttu-id="843aa-248">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-248">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-249">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="843aa-249">Click **Properties**.</span></span>
3. <span data-ttu-id="843aa-250">V části **tooRetain dní úloha se dotazuje**, přesuňte jezdec tooselect hello hodnotu nebo zadejte hodnotu hello hello textového pole.</span><span class="sxs-lookup"><span data-stu-id="843aa-250">Under **Days tooRetain Job Queries**, move hello slider tooselect a value, or enter hello value in hello text box.</span></span>  
4. <span data-ttu-id="843aa-251">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="843aa-251">Click **Save**.</span></span>

### <a name="job-level-policies"></a><span data-ttu-id="843aa-252">Zásady na úrovni úlohy</span><span class="sxs-lookup"><span data-stu-id="843aa-252">Job-level policies</span></span>
<span data-ttu-id="843aa-253">Pomocí zásad na úrovni úlohy, můžete řídit hello maximální Austrálie a maximální priority, kterou jednotlivé uživatele (nebo členy určité skupiny zabezpečení) můžete nastavit na úlohy, které se odesílají hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-253">With job-level policies, you can control hello maximum AUs and hello maximum priority that individual users (or members of specific security groups) can set on jobs that they submit.</span></span> <span data-ttu-id="843aa-254">Tato umožňuje řídit náklady hello způsobené uživatele.</span><span class="sxs-lookup"><span data-stu-id="843aa-254">This lets you control hello costs incurred by users.</span></span> <span data-ttu-id="843aa-255">Umožňuje vám také může mít vliv hello ovládací prvek naplánovaných úlohách na s vysokou prioritou provozní úlohy, které jsou spuštěny v hello stejný účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-255">It also lets you control hello effect that scheduled jobs might have on high-priority production jobs that are running in hello same Data Lake Analytics account.</span></span>

<span data-ttu-id="843aa-256">Data Lake Analytics má dvě zásady, které můžete nastavit na úrovni hello úlohy:</span><span class="sxs-lookup"><span data-stu-id="843aa-256">Data Lake Analytics has two policies that you can set at hello job level:</span></span>

* <span data-ttu-id="843aa-257">**AU limit na jednu úlohu**: uživatelé lze odeslat pouze úlohy, které jste si toothis počet Austrálie.</span><span class="sxs-lookup"><span data-stu-id="843aa-257">**AU limit per job**: Users can only submit jobs that have up toothis number of AUs.</span></span> <span data-ttu-id="843aa-258">Ve výchozím nastavení je tento limit hello stejné jako maximální limit AU hello hello účtu.</span><span class="sxs-lookup"><span data-stu-id="843aa-258">By default, this limit is hello same as hello maximum AU limit for hello account.</span></span>
* <span data-ttu-id="843aa-259">**Priorita**: uživatelé lze odeslat pouze úlohy, které mají hodnotu nižší než nebo rovna toothis priority.</span><span class="sxs-lookup"><span data-stu-id="843aa-259">**Priority**: Users can only submit jobs that have a priority lower than or equal toothis value.</span></span> <span data-ttu-id="843aa-260">Všimněte si, že vyšší číslo znamená s nižší prioritou.</span><span class="sxs-lookup"><span data-stu-id="843aa-260">Note that a higher number means a lower priority.</span></span> <span data-ttu-id="843aa-261">Ve výchozím nastavení je nastavena too1, což je nejvyšší možná priorita hello.</span><span class="sxs-lookup"><span data-stu-id="843aa-261">By default, this is set too1, which is hello highest possible priority.</span></span>

<span data-ttu-id="843aa-262">Existuje výchozí zásada, nastavte pro každý účet.</span><span class="sxs-lookup"><span data-stu-id="843aa-262">There is a default policy set on every account.</span></span> <span data-ttu-id="843aa-263">Hello výchozí zásada tooall uživatelé hello účtu.</span><span class="sxs-lookup"><span data-stu-id="843aa-263">hello default policy applies tooall users of hello account.</span></span> <span data-ttu-id="843aa-264">Další zásady můžete nastavit pro konkrétní uživatele a skupiny.</span><span class="sxs-lookup"><span data-stu-id="843aa-264">You can set additional policies for specific users and groups.</span></span> 

> [!NOTE]
> <span data-ttu-id="843aa-265">Zásady na úrovni účtu a zásady na úrovni úlohy použít současně.</span><span class="sxs-lookup"><span data-stu-id="843aa-265">Account-level policies and job-level policies apply simultaneously.</span></span>
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a><span data-ttu-id="843aa-266">Přidání zásad pro konkrétního uživatele nebo skupiny</span><span class="sxs-lookup"><span data-stu-id="843aa-266">Add a policy for a specific user or group</span></span>

1. <span data-ttu-id="843aa-267">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-267">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-268">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="843aa-268">Click **Properties**.</span></span>
3. <span data-ttu-id="843aa-269">V části **úlohy odeslání omezení**, klikněte na tlačítko hello **přidat zásadu** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="843aa-269">Under **Job Submission Limits**, click hello **Add Policy** button.</span></span> <span data-ttu-id="843aa-270">Potom vyberte nebo zadejte hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="843aa-270">Then, select or enter hello following settings:</span></span>
    1. <span data-ttu-id="843aa-271">**Název zásady výpočetní**: Zadejte název zásady, tooremind o účelu hello hello zásad.</span><span class="sxs-lookup"><span data-stu-id="843aa-271">**Compute Policy Name**: Enter a policy name, tooremind you of hello purpose of hello policy.</span></span>
    2. <span data-ttu-id="843aa-272">**Vyberte uživatele nebo skupiny**: Vyberte hello uživatele nebo skupiny, tato zásada se vztahuje na.</span><span class="sxs-lookup"><span data-stu-id="843aa-272">**Select User or Group**: Select hello user or group this policy applies to.</span></span>
    3. <span data-ttu-id="843aa-273">**Nastavit hello Limit AU úloh**: nastaveného limitu hello AU, která se použije toohello vybrané uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="843aa-273">**Set hello Job AU Limit**: Set hello AU limit that applies toohello selected user or group.</span></span>
    4. <span data-ttu-id="843aa-274">**Nastavit hello Priority Limit**: nastavte hello priority limit, která se použije toohello vybrané uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="843aa-274">**Set hello Priority Limit**: Set hello priority limit that applies toohello selected user or group.</span></span>

4. <span data-ttu-id="843aa-275">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="843aa-275">Click **Ok**.</span></span>

5. <span data-ttu-id="843aa-276">nové zásady Hello je uvedena v hello **výchozí** zásad v části tabulky **úlohy odeslání omezení**.</span><span class="sxs-lookup"><span data-stu-id="843aa-276">hello new policy is listed in hello **Default** policy table, under **Job Submission Limits**.</span></span> 

#### <a name="delete-or-edit-an-existing-policy"></a><span data-ttu-id="843aa-277">Odstraňte nebo upravte existující zásady</span><span class="sxs-lookup"><span data-stu-id="843aa-277">Delete or edit an existing policy</span></span>

1. <span data-ttu-id="843aa-278">V hello portálu Azure přejděte tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="843aa-278">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="843aa-279">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="843aa-279">Click **Properties**.</span></span>
3. <span data-ttu-id="843aa-280">V části **úlohy odeslání omezení**, najít hello zásady chcete tooedit.</span><span class="sxs-lookup"><span data-stu-id="843aa-280">Under **Job Submission Limits**, find hello policy you want tooedit.</span></span>
4.  <span data-ttu-id="843aa-281">toosee hello **odstranit** a **upravit** klikněte na možnosti v hello pravou krajní sloupec v tabulce hello **...** .</span><span class="sxs-lookup"><span data-stu-id="843aa-281">toosee hello **Delete** and **Edit** options, in hello rightmost column of hello table, click **...**.</span></span>

### <a name="additional-resources-for-job-policies"></a><span data-ttu-id="843aa-282">Další prostředky pro úlohy zásady</span><span class="sxs-lookup"><span data-stu-id="843aa-282">Additional resources for job policies</span></span>
* [<span data-ttu-id="843aa-283">Příspěvek blogu přehled zásad</span><span class="sxs-lookup"><span data-stu-id="843aa-283">Policy overview blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [<span data-ttu-id="843aa-284">Zásady na úrovni účtu příspěvku na blogu</span><span class="sxs-lookup"><span data-stu-id="843aa-284">Account-level policies blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [<span data-ttu-id="843aa-285">Zásady na úrovni úlohy příspěvku na blogu</span><span class="sxs-lookup"><span data-stu-id="843aa-285">Job-level policies blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a><span data-ttu-id="843aa-286">Další kroky</span><span class="sxs-lookup"><span data-stu-id="843aa-286">Next steps</span></span>

* [<span data-ttu-id="843aa-287">Přehled Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="843aa-287">Overview of Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="843aa-288">Začínáme s Data Lake Analytics pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="843aa-288">Get started with Data Lake Analytics by using hello Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="843aa-289">Správa Azure Data Lake Analytics pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="843aa-289">Manage Azure Data Lake Analytics by using Azure PowerShell</span></span>](data-lake-analytics-manage-use-powershell.md)

