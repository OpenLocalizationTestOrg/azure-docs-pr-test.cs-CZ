---
title: "aaaManage Azure Backup trezory a servery Azure pomocí modelu nasazení classic hello | Microsoft Docs"
description: "Použijte tento kurz toolearn jak toomanage Azure Backup trezory a servery."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: f175eb12-0905-437f-91fd-eaee03ab6e81
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: markgal;
ms.openlocfilehash: 6c38b04f4a76604bfd639a9b2d58237ce44e2386
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-hello-classic-deployment-model"></a><span data-ttu-id="fc85c-103">Správa trezoru zálohování Azure a servery pomocí modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="fc85c-103">Manage Azure Backup vaults and servers using hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fc85c-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fc85c-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="fc85c-105">Classic</span><span class="sxs-lookup"><span data-stu-id="fc85c-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="fc85c-106">V tomto článku najdete přehled úlohy zálohování správy hello, které jsou k dispozici prostřednictvím hello portál Azure classic a hello Microsoft Azure Backup agent.</span><span class="sxs-lookup"><span data-stu-id="fc85c-106">In this article you'll find an overview of hello backup management tasks available through hello Azure classic portal and hello Microsoft Azure Backup agent.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fc85c-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fc85c-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="fc85c-108">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="fc85c-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="fc85c-109">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="fc85c-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fc85c-110">Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup.</span><span class="sxs-lookup"><span data-stu-id="fc85c-110">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="fc85c-111">Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="fc85c-111">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="fc85c-112">Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="fc85c-112">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="fc85c-113">Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fc85c-113">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="fc85c-114">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="fc85c-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="fc85c-115">Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="fc85c-115">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="fc85c-116">Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="fc85c-116">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="fc85c-117">Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="fc85c-117">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="management-portal-tasks"></a><span data-ttu-id="fc85c-118">Úlohy správy portálu</span><span class="sxs-lookup"><span data-stu-id="fc85c-118">Management portal tasks</span></span>
1. <span data-ttu-id="fc85c-119">Přihlaste se toohello [portálu pro správu](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fc85c-119">Sign in toohello [Management Portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="fc85c-120">Klikněte na tlačítko **služeb zotavení**, pak klikněte na tlačítko hello název úložiště záloh tooview hello rychlý Start stránky.</span><span class="sxs-lookup"><span data-stu-id="fc85c-120">Click **Recovery Services**, then click hello name of backup vault tooview hello Quick Start page.</span></span>

    ![Služby zotavení](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

<span data-ttu-id="fc85c-122">Výběrem možnosti hello hello horní části stránky rychlý Start hello uvidíte hello dostupné úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="fc85c-122">By selecting hello options at hello top of hello Quick Start page, you can see hello available management tasks.</span></span>

![Spravovat karty](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a><span data-ttu-id="fc85c-124">Řídicí panel</span><span class="sxs-lookup"><span data-stu-id="fc85c-124">Dashboard</span></span>
<span data-ttu-id="fc85c-125">Vyberte **řídicí panel** toosee hello přehled využití pro hello server.</span><span class="sxs-lookup"><span data-stu-id="fc85c-125">Select **Dashboard** toosee hello usage overview for hello server.</span></span> <span data-ttu-id="fc85c-126">Hello **přehled využití** zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="fc85c-126">hello **usage overview** includes:</span></span>

* <span data-ttu-id="fc85c-127">Hello počet serverů Windows registrovaných toocloud</span><span class="sxs-lookup"><span data-stu-id="fc85c-127">hello number of Windows Servers registered toocloud</span></span>
* <span data-ttu-id="fc85c-128">Hello počet virtuálních počítačů Azure chráněna v cloudu</span><span class="sxs-lookup"><span data-stu-id="fc85c-128">hello number of Azure virtual machines protected in cloud</span></span>
* <span data-ttu-id="fc85c-129">Celkový úložný Hello využívat v Azure</span><span class="sxs-lookup"><span data-stu-id="fc85c-129">hello total storage consumed in Azure</span></span>
* <span data-ttu-id="fc85c-130">Hello stav posledních úloh</span><span class="sxs-lookup"><span data-stu-id="fc85c-130">hello status of recent jobs</span></span>

<span data-ttu-id="fc85c-131">Dole hello hello řídicí panel můžete provádět následující úlohy hello:</span><span class="sxs-lookup"><span data-stu-id="fc85c-131">At hello bottom of hello Dashboard you can perform hello following tasks:</span></span>

* <span data-ttu-id="fc85c-132">**Správa certifikátů** – Pokud certifikát byl server hello použité tooregister, můžete použít tento certifikát tooupdate hello.</span><span class="sxs-lookup"><span data-stu-id="fc85c-132">**Manage certificate** - If a certificate was used tooregister hello server, then use this tooupdate hello certificate.</span></span> <span data-ttu-id="fc85c-133">Pokud používáte přihlašovací údaje trezoru, nepoužívejte **Správa certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-133">If you are using vault credentials, do not use **Manage certificate**.</span></span>
* <span data-ttu-id="fc85c-134">**Odstranit** -odstraní hello aktuální úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="fc85c-134">**Delete** - Deletes hello current backup vault.</span></span> <span data-ttu-id="fc85c-135">Pokud se už používá úložiště záloh, můžete ho odstranit toofree do prostoru úložiště.</span><span class="sxs-lookup"><span data-stu-id="fc85c-135">If a backup vault is no longer being used, you can delete it toofree up storage space.</span></span> <span data-ttu-id="fc85c-136">**Odstranit** je aktivní jenom po z trezoru hello byly odstraněny všechny registrované servery.</span><span class="sxs-lookup"><span data-stu-id="fc85c-136">**Delete** is only enabled after all registered servers have been deleted from hello vault.</span></span>

![Úlohy zálohování řídicí panel](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a><span data-ttu-id="fc85c-138">Registrovaných položek</span><span class="sxs-lookup"><span data-stu-id="fc85c-138">Registered items</span></span>
<span data-ttu-id="fc85c-139">Vyberte **registrované položky** tooview hello názvy hello serverů, které jsou registrovány toothis trezoru.</span><span class="sxs-lookup"><span data-stu-id="fc85c-139">Select **Registered Items** tooview hello names of hello servers that are registered toothis vault.</span></span>

![Registrovaných položek](./media/backup-azure-manage-windows-server-classic/registered-items.png)

<span data-ttu-id="fc85c-141">Hello **typ** výchozí nastavení filtru tooAzure virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fc85c-141">hello **Type** filter defaults tooAzure Virtual Machine.</span></span> <span data-ttu-id="fc85c-142">Vyberte tooview hello názvy hello serverů, které jsou registrované toothis trezoru **systému Windows server** z hello rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="fc85c-142">tooview hello names of hello servers that are registered toothis vault, select **Windows server** from hello drop down menu.</span></span>

<span data-ttu-id="fc85c-143">Odsud můžete provádět následující úlohy hello:</span><span class="sxs-lookup"><span data-stu-id="fc85c-143">From here you can perform hello following tasks:</span></span>

* <span data-ttu-id="fc85c-144">**Povolit opakovanou registraci** – Pokud je vybraná tato možnost pro server, můžete použít hello **Průvodce registrací** hello na místním Microsoft Azure Backup agent tooregister hello serveru pomocí úložiště záloh hello podruhé .</span><span class="sxs-lookup"><span data-stu-id="fc85c-144">**Allow Re-registration** - When this option is selected for a server you can use hello **Registration Wizard** in hello on-premises Microsoft Azure Backup agent tooregister hello server with hello backup vault a second time.</span></span> <span data-ttu-id="fc85c-145">Může být nutné toore registrace z důvodu chyby tooan v certifikátu hello nebo pokud server měl toobe znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="fc85c-145">You might need toore-register due tooan error in hello certificate or if a server had toobe rebuilt.</span></span>
* <span data-ttu-id="fc85c-146">**Odstranit** -odstraní server z trezoru záloh hello.</span><span class="sxs-lookup"><span data-stu-id="fc85c-146">**Delete** - Deletes a server from hello backup vault.</span></span> <span data-ttu-id="fc85c-147">Všechny hello uložená data přidružená k hello serveru budou okamžitě odstraněna.</span><span class="sxs-lookup"><span data-stu-id="fc85c-147">All of hello stored data associated with hello server is deleted immediately.</span></span>

    ![Úlohy registrovaných položek](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a><span data-ttu-id="fc85c-149">Chráněné položky</span><span class="sxs-lookup"><span data-stu-id="fc85c-149">Protected items</span></span>
<span data-ttu-id="fc85c-150">Vyberte **chráněné položky** tooview hello položky, které byly zálohovány ze serverů hello.</span><span class="sxs-lookup"><span data-stu-id="fc85c-150">Select **Protected Items** tooview hello items that have been backed up from hello servers.</span></span>

![Chráněné položky](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a><span data-ttu-id="fc85c-152">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="fc85c-152">Configure</span></span>
<span data-ttu-id="fc85c-153">Z hello **konfigurace** kartě můžete vybrat možnost redundance hello odpovídající úložiště.</span><span class="sxs-lookup"><span data-stu-id="fc85c-153">From hello **Configure** tab you can select hello appropriate storage redundancy option.</span></span> <span data-ttu-id="fc85c-154">Hello nejlepší čas tooselect hello úložiště redundance možnost je hned po vytvoření trezoru a předtím, než všechny počítače jsou registrované tooit.</span><span class="sxs-lookup"><span data-stu-id="fc85c-154">hello best time tooselect hello storage redundancy option is right after creating a vault and before any machines are registered tooit.</span></span>

> [!WARNING]
> <span data-ttu-id="fc85c-155">Jakmile položku už registrovaný toohello trezoru, možnost redundance úložiště hello je uzamčen a nemůže být upraven.</span><span class="sxs-lookup"><span data-stu-id="fc85c-155">Once an item has been registered toohello vault, hello storage redundancy option is locked and cannot be modified.</span></span>
>
>

![Konfigurace](./media/backup-azure-manage-windows-server-classic/configure.png)

<span data-ttu-id="fc85c-157">Najdete v tomto článku Další informace [redundance úložiště](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="fc85c-157">See this article for more information about [storage redundancy](../storage/common/storage-redundancy.md).</span></span>

## <a name="microsoft-azure-backup-agent-tasks"></a><span data-ttu-id="fc85c-158">Úlohy agenta Microsoft Azure Backup</span><span class="sxs-lookup"><span data-stu-id="fc85c-158">Microsoft Azure Backup agent tasks</span></span>
### <a name="console"></a><span data-ttu-id="fc85c-159">Konzola</span><span class="sxs-lookup"><span data-stu-id="fc85c-159">Console</span></span>
<span data-ttu-id="fc85c-160">Otevřete hello **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="fc85c-160">Open hello **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Agenta zálohování](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

<span data-ttu-id="fc85c-162">Z hello **akce** k dispozici na hello napravo od hello agenta zálohování konzoly můžete provádět následující úlohy správy hello:</span><span class="sxs-lookup"><span data-stu-id="fc85c-162">From hello **Actions** available at hello right of hello backup agent console you can perform hello following management tasks:</span></span>

* <span data-ttu-id="fc85c-163">Registrace serveru</span><span class="sxs-lookup"><span data-stu-id="fc85c-163">Register Server</span></span>
* <span data-ttu-id="fc85c-164">Plán zálohování</span><span class="sxs-lookup"><span data-stu-id="fc85c-164">Schedule Backup</span></span>
* <span data-ttu-id="fc85c-165">Zálohovat nyní</span><span class="sxs-lookup"><span data-stu-id="fc85c-165">Back Up now</span></span>
* <span data-ttu-id="fc85c-166">Změnit vlastnosti</span><span class="sxs-lookup"><span data-stu-id="fc85c-166">Change Properties</span></span>

![Konzole akce agenta.](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> <span data-ttu-id="fc85c-168">příliš**obnovit Data**, najdete v části [obnovit soubory tooa Windows server nebo klientský počítač systému Windows](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="fc85c-168">too**Recover Data**, see [Restore files tooa Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

### <a name="modify-an-existing-backup"></a><span data-ttu-id="fc85c-169">Upravit existující zálohy</span><span class="sxs-lookup"><span data-stu-id="fc85c-169">Modify an existing backup</span></span>
1. <span data-ttu-id="fc85c-170">V rámci Microsoft Azure Backup agent hello klikněte na tlačítko **plánem zálohování**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-170">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. <span data-ttu-id="fc85c-172">V hello **Průvodce plánem zálohování** nechte hello **provádět změny toobackup položky nebo časy** možnost a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-172">In hello **Schedule Backup Wizard** leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Upravit naplánovaného zálohování](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="fc85c-174">Pokud chcete tooadd nebo změnit položky na hello **vyberte položky tooBackup** obrazovce klikněte na tlačítko **přidat položky**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-174">If you want tooadd or change items, on hello **Select Items tooBackup** screen click **Add Items**.</span></span>

    <span data-ttu-id="fc85c-175">Můžete také nastavit **nastavení vyloučení** z této stránky v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="fc85c-175">You can also set **Exclusion Settings** from this page in hello wizard.</span></span> <span data-ttu-id="fc85c-176">Pokud chcete tooexclude soubory nebo typy souborů číst hello postup pro přidání [nastavení vyloučení](#exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="fc85c-176">If you want tooexclude files or file types read hello procedure for adding [exclusion settings](#exclusion-settings).</span></span>
4. <span data-ttu-id="fc85c-177">Vyberte hello soubory a složky tooback nahoru a klikněte na **nevadí**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-177">Select hello files and folders you want tooback up and click **Okay**.</span></span>

    ![Přidání položek](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. <span data-ttu-id="fc85c-179">Zadejte hello **plán zálohování** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-179">Specify hello **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="fc85c-180">Můžete naplánovat denní (probíhající maximálně 3krát denně) nebo týdenní zálohování.</span><span class="sxs-lookup"><span data-stu-id="fc85c-180">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Zadejte plán zálohování](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="fc85c-182">Zadat plán zálohování hello je podrobně popsány v tomto [článku](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="fc85c-182">Specifying hello backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >
6. <span data-ttu-id="fc85c-183">Vyberte hello **zásady uchovávání informací** pro záložní kopii hello a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-183">Select hello **Retention Policy** for hello backup copy and click **Next**.</span></span>

    ![Vyberte vaše zásady uchovávání informací](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. <span data-ttu-id="fc85c-185">Na hello **potvrzení** obrazovce zkontrolovat hello informace a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-185">On hello **Confirmation** screen review hello information and click **Finish**.</span></span>
8. <span data-ttu-id="fc85c-186">Po dokončení Průvodce hello vytváření hello **plán zálohování**, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-186">Once hello wizard finishes creating hello **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="fc85c-187">Po úpravě ochrany, můžete ověřit, zálohování, ke které spouštějí správně podle budete toohello **úlohy** kartě a potvrzující, že změny se projeví v hello zálohování úloh.</span><span class="sxs-lookup"><span data-stu-id="fc85c-187">After modifying protection, you can confirm that backups are triggering correctly by going toohello **Jobs** tab and confirming that changes are reflected in hello backup jobs.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="fc85c-188">Povolit omezení šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="fc85c-188">Enable Network Throttling</span></span>
<span data-ttu-id="fc85c-189">agent Azure Backup Hello poskytuje omezování karta, která vám umožní toocontrol použití šířky pásma sítě při přenosu dat.</span><span class="sxs-lookup"><span data-stu-id="fc85c-189">hello Azure Backup agent provides a Throttling tab which allows you toocontrol how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="fc85c-190">Tento ovládací prvek může být užitečné, pokud potřebujete tooback dat během pracovní doby, ale nechcete, aby proces zálohování toointerfere hello s ostatní internetový provoz.</span><span class="sxs-lookup"><span data-stu-id="fc85c-190">This control can be helpful if you need tooback up data during work hours but do not want hello backup process toointerfere with other internet traffic.</span></span> <span data-ttu-id="fc85c-191">Omezování dat přenos platí tooback nahoru a obnovit aktivity.</span><span class="sxs-lookup"><span data-stu-id="fc85c-191">Throttling of data transfer applies tooback up and restore activities.</span></span>  

<span data-ttu-id="fc85c-192">tooenable omezení:</span><span class="sxs-lookup"><span data-stu-id="fc85c-192">tooenable throttling:</span></span>

1. <span data-ttu-id="fc85c-193">V hello **agenta zálohování**, klikněte na tlačítko **změnit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-193">In hello **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="fc85c-194">Vyberte hello **povolit využití šířky pásma Internetu u operací zálohování omezení** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="fc85c-194">Select hello **Enable internet bandwidth usage throttling for backup operations** checkbox.</span></span>

    ![Omezení šířky pásma sítě](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. <span data-ttu-id="fc85c-196">Jakmile jste povolili omezování, zadejte hello povolený šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-196">Once you have enabled throttling, specify hello allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="fc85c-197">hodnoty šířky pásma Hello začít až 512 kilobitů za sekundu (Kbps) a můžete přejít do too1023 megabajtů za sekundu (Mbps).</span><span class="sxs-lookup"><span data-stu-id="fc85c-197">hello bandwidth values begin at 512 kilobytes per second (Kbps) and can go up too1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="fc85c-198">Můžete také určit hello zahájení a dokončení pro **pracovní hodiny**, a které dny v týdnu hello jsou považovány za pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="fc85c-198">You can also designate hello start and finish for **Work hours**, and which days of hello week are considered Work days.</span></span> <span data-ttu-id="fc85c-199">čas Hello mimo určené pracovní hodiny hello je považovány toobe mimo pracovní dobu.</span><span class="sxs-lookup"><span data-stu-id="fc85c-199">hello time outside of hello designated Work hours is considered toobe non-work hours.</span></span>
4. <span data-ttu-id="fc85c-200">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-200">Click **OK**.</span></span>

## <a name="exclusion-settings"></a><span data-ttu-id="fc85c-201">Nastavení vyloučení</span><span class="sxs-lookup"><span data-stu-id="fc85c-201">Exclusion settings</span></span>
1. <span data-ttu-id="fc85c-202">Otevřete hello **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="fc85c-202">Open hello **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Otevřete agenta zálohování](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. <span data-ttu-id="fc85c-204">V rámci Microsoft Azure Backup agent hello klikněte na tlačítko **plánem zálohování**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-204">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. <span data-ttu-id="fc85c-206">V Průvodci plánování zálohování hello nechte hello **provádět změny toobackup položky nebo časy** možnost a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-206">In hello Schedule Backup Wizard leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Změna plánu](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="fc85c-208">Klikněte na tlačítko **nastavení vyloučení**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-208">Click **Exclusions Settings**.</span></span>

    ![Vyberte položky tooexclude](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. <span data-ttu-id="fc85c-210">Klikněte na tlačítko **Přidat vyloučení**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-210">Click **Add Exclusion**.</span></span>

    ![Přidat vyloučení](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. <span data-ttu-id="fc85c-212">Vyberte umístění hello a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-212">Select hello location and then, click **OK**.</span></span>

    ![Vyberte umístění pro vyloučení](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. <span data-ttu-id="fc85c-214">Přidat příponu souboru hello v hello **typ souboru** pole.</span><span class="sxs-lookup"><span data-stu-id="fc85c-214">Add hello file extension in hello **File Type** field.</span></span>

    ![Vyloučit podle typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    <span data-ttu-id="fc85c-216">Přidání rozšíření MP3</span><span class="sxs-lookup"><span data-stu-id="fc85c-216">Adding an .mp3 extension</span></span>

    ![Příklad typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    <span data-ttu-id="fc85c-218">tooadd jiné rozšíření, klikněte na tlačítko **Přidat vyloučení** a zadejte jinou příponu typu souboru (Přidání rozšíření .jpeg).</span><span class="sxs-lookup"><span data-stu-id="fc85c-218">tooadd another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Další příklad typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. <span data-ttu-id="fc85c-220">Pokud jste přidali všechny hello rozšíření, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-220">When you've added all hello extensions, click **OK**.</span></span>
9. <span data-ttu-id="fc85c-221">Kliknutím na pokračovat prostřednictvím hello Průvodce plánem zálohování **Další** dokud hello **potvrzovací stránku**, pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="fc85c-221">Continue through hello Schedule Backup Wizard by clicking **Next** until hello **Confirmation page**, then click **Finish**.</span></span>

    ![Vyloučení potvrzení](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a><span data-ttu-id="fc85c-223">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc85c-223">Next steps</span></span>
* [<span data-ttu-id="fc85c-224">Obnovení systému Windows Server nebo klienta Windows z Azure</span><span class="sxs-lookup"><span data-stu-id="fc85c-224">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="fc85c-225">toolearn Další informace o zálohování Azure, najdete v části [Přehled zálohování Azure](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="fc85c-225">toolearn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="fc85c-226">Navštivte hello [fóru služby Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="fc85c-226">Visit hello [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
