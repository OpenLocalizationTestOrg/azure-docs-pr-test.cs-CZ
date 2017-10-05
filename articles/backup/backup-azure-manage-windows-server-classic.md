---
title: "Spravovat servery Azure pomocí modelu nasazení classic a trezory Azure Backup | Microsoft Docs"
description: "Pomocí tohoto kurzu se dozvíte, jak ke správě trezoru zálohování Azure a serverů."
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
ms.openlocfilehash: 91451b2cdc42ed05ef7c1ba9c66ad5b4b45dd788
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a><span data-ttu-id="bbd66-103">Správa serverů a trezorů Azure Backup s využitím klasického modelu nasazení</span><span class="sxs-lookup"><span data-stu-id="bbd66-103">Manage Azure Backup vaults and servers using the classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bbd66-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="bbd66-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="bbd66-105">Classic</span><span class="sxs-lookup"><span data-stu-id="bbd66-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="bbd66-106">V tomto článku najdete přehled úlohy zálohování správy, který je k dispozici prostřednictvím portálu Azure classic a Microsoft Azure Backup agent.</span><span class="sxs-lookup"><span data-stu-id="bbd66-106">In this article you'll find an overview of the backup management tasks available through the Azure classic portal and the Microsoft Azure Backup agent.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bbd66-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="bbd66-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="bbd66-108">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="bbd66-108">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="bbd66-109">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="bbd66-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bbd66-110">Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="bbd66-110">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="bbd66-111">Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="bbd66-111">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="bbd66-112">Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="bbd66-112">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="bbd66-113">Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup.</span><span class="sxs-lookup"><span data-stu-id="bbd66-113">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="bbd66-114">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="bbd66-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="bbd66-115">Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="bbd66-115">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="bbd66-116">Nebudete mít přístup k datům záloh na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="bbd66-116">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="bbd66-117">Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="bbd66-117">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="management-portal-tasks"></a><span data-ttu-id="bbd66-118">Úlohy správy portálu</span><span class="sxs-lookup"><span data-stu-id="bbd66-118">Management portal tasks</span></span>
1. <span data-ttu-id="bbd66-119">Přihlaste se k [portálu pro správu](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="bbd66-119">Sign in to the [Management Portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="bbd66-120">Klikněte na tlačítko **služeb zotavení**, pak klikněte na název úložiště záloh, chcete-li zobrazit stránku rychlý Start.</span><span class="sxs-lookup"><span data-stu-id="bbd66-120">Click **Recovery Services**, then click the name of backup vault to view the Quick Start page.</span></span>

    ![Služby zotavení](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

<span data-ttu-id="bbd66-122">Výběrem možnosti v horní části stránky rychlý Start, uvidíte dostupné úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="bbd66-122">By selecting the options at the top of the Quick Start page, you can see the available management tasks.</span></span>

![Spravovat karty](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a><span data-ttu-id="bbd66-124">Řídicí panel</span><span class="sxs-lookup"><span data-stu-id="bbd66-124">Dashboard</span></span>
<span data-ttu-id="bbd66-125">Vyberte **řídicí panel** zobrazíte přehled využití pro server.</span><span class="sxs-lookup"><span data-stu-id="bbd66-125">Select **Dashboard** to see the usage overview for the server.</span></span> <span data-ttu-id="bbd66-126">**Přehled využití** zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="bbd66-126">The **usage overview** includes:</span></span>

* <span data-ttu-id="bbd66-127">Počet serverů, Windows zaregistrovat do cloudu</span><span class="sxs-lookup"><span data-stu-id="bbd66-127">The number of Windows Servers registered to cloud</span></span>
* <span data-ttu-id="bbd66-128">Počet virtuálních počítačů Azure chráněna v cloudu</span><span class="sxs-lookup"><span data-stu-id="bbd66-128">The number of Azure virtual machines protected in cloud</span></span>
* <span data-ttu-id="bbd66-129">Celkový úložný využívat v Azure</span><span class="sxs-lookup"><span data-stu-id="bbd66-129">The total storage consumed in Azure</span></span>
* <span data-ttu-id="bbd66-130">Stav posledních úloh</span><span class="sxs-lookup"><span data-stu-id="bbd66-130">The status of recent jobs</span></span>

<span data-ttu-id="bbd66-131">V dolní části řídicího panelu můžete provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="bbd66-131">At the bottom of the Dashboard you can perform the following tasks:</span></span>

* <span data-ttu-id="bbd66-132">**Správa certifikátů** – Pokud certifikát byl použit k registraci serveru a pak aktualizuje certifikát.</span><span class="sxs-lookup"><span data-stu-id="bbd66-132">**Manage certificate** - If a certificate was used to register the server, then use this to update the certificate.</span></span> <span data-ttu-id="bbd66-133">Pokud používáte přihlašovací údaje trezoru, nepoužívejte **Správa certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-133">If you are using vault credentials, do not use **Manage certificate**.</span></span>
* <span data-ttu-id="bbd66-134">**Odstranit** -odstraní aktuální úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="bbd66-134">**Delete** - Deletes the current backup vault.</span></span> <span data-ttu-id="bbd66-135">Pokud se už používá úložiště záloh, chcete-li odstranit pro uvolnění prostoru úložiště.</span><span class="sxs-lookup"><span data-stu-id="bbd66-135">If a backup vault is no longer being used, you can delete it to free up storage space.</span></span> <span data-ttu-id="bbd66-136">**Odstranit** je aktivní jenom po všechny registrované servery byly odstraněny z trezoru.</span><span class="sxs-lookup"><span data-stu-id="bbd66-136">**Delete** is only enabled after all registered servers have been deleted from the vault.</span></span>

![Úlohy zálohování řídicí panel](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a><span data-ttu-id="bbd66-138">Registrovaných položek</span><span class="sxs-lookup"><span data-stu-id="bbd66-138">Registered items</span></span>
<span data-ttu-id="bbd66-139">Vyberte **registrované položky** k zobrazení názvů serverů, které jsou registrovány k tomuto úložišti.</span><span class="sxs-lookup"><span data-stu-id="bbd66-139">Select **Registered Items** to view the names of the servers that are registered to this vault.</span></span>

![Registrovaných položek](./media/backup-azure-manage-windows-server-classic/registered-items.png)

<span data-ttu-id="bbd66-141">**Typ** filtrovat výchozí hodnoty pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="bbd66-141">The **Type** filter defaults to Azure Virtual Machine.</span></span> <span data-ttu-id="bbd66-142">Pokud chcete zobrazit názvy serverů, které jsou registrovány k tomuto trezoru, vyberte **systému Windows server** z rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="bbd66-142">To view the names of the servers that are registered to this vault, select **Windows server** from the drop down menu.</span></span>

<span data-ttu-id="bbd66-143">Odsud můžete provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="bbd66-143">From here you can perform the following tasks:</span></span>

* <span data-ttu-id="bbd66-144">**Povolit opakovanou registraci** – Pokud je vybraná tato možnost pro server, můžete použít **Průvodce registrací** v Microsoft Azure Backup agent místní registrace serveru u služby backup trezoru ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="bbd66-144">**Allow Re-registration** - When this option is selected for a server you can use the **Registration Wizard** in the on-premises Microsoft Azure Backup agent to register the server with the backup vault a second time.</span></span> <span data-ttu-id="bbd66-145">Možná budete muset znovu registrovat z důvodu chyby v certifikátu, nebo pokud server museli znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="bbd66-145">You might need to re-register due to an error in the certificate or if a server had to be rebuilt.</span></span>
* <span data-ttu-id="bbd66-146">**Odstranit** -odstraní server z trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="bbd66-146">**Delete** - Deletes a server from the backup vault.</span></span> <span data-ttu-id="bbd66-147">Všechna uložená data přidružené k serveru budou okamžitě odstraněna.</span><span class="sxs-lookup"><span data-stu-id="bbd66-147">All of the stored data associated with the server is deleted immediately.</span></span>

    ![Úlohy registrovaných položek](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a><span data-ttu-id="bbd66-149">Chráněné položky</span><span class="sxs-lookup"><span data-stu-id="bbd66-149">Protected items</span></span>
<span data-ttu-id="bbd66-150">Vyberte **chráněné položky** Chcete-li zobrazit položky, které byly zálohovány ze serverů.</span><span class="sxs-lookup"><span data-stu-id="bbd66-150">Select **Protected Items** to view the items that have been backed up from the servers.</span></span>

![Chráněné položky](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a><span data-ttu-id="bbd66-152">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="bbd66-152">Configure</span></span>
<span data-ttu-id="bbd66-153">Z **konfigurace** kartě můžete vybrat možnost redundance odpovídající úložiště.</span><span class="sxs-lookup"><span data-stu-id="bbd66-153">From the **Configure** tab you can select the appropriate storage redundancy option.</span></span> <span data-ttu-id="bbd66-154">Nyní můžete vybrat možnost redundance úložiště je správné po vytvoření trezoru a předtím, než všechny počítače, které jsou registrovány k němu.</span><span class="sxs-lookup"><span data-stu-id="bbd66-154">The best time to select the storage redundancy option is right after creating a vault and before any machines are registered to it.</span></span>

> [!WARNING]
> <span data-ttu-id="bbd66-155">Jakmile položku byl registrovaný k úložišti, možnost redundance úložiště je uzamčen a nemůže být upraven.</span><span class="sxs-lookup"><span data-stu-id="bbd66-155">Once an item has been registered to the vault, the storage redundancy option is locked and cannot be modified.</span></span>
>
>

![Konfigurace](./media/backup-azure-manage-windows-server-classic/configure.png)

<span data-ttu-id="bbd66-157">Najdete v tomto článku Další informace [redundance úložiště](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="bbd66-157">See this article for more information about [storage redundancy](../storage/common/storage-redundancy.md).</span></span>

## <a name="microsoft-azure-backup-agent-tasks"></a><span data-ttu-id="bbd66-158">Úlohy agenta Microsoft Azure Backup</span><span class="sxs-lookup"><span data-stu-id="bbd66-158">Microsoft Azure Backup agent tasks</span></span>
### <a name="console"></a><span data-ttu-id="bbd66-159">Konzola</span><span class="sxs-lookup"><span data-stu-id="bbd66-159">Console</span></span>
<span data-ttu-id="bbd66-160">Otevřete **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="bbd66-160">Open the **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Agenta zálohování](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

<span data-ttu-id="bbd66-162">Z **akce** k dispozici na pravé straně konzole agenta zálohování můžete provádět následující úlohy správy:</span><span class="sxs-lookup"><span data-stu-id="bbd66-162">From the **Actions** available at the right of the backup agent console you can perform the following management tasks:</span></span>

* <span data-ttu-id="bbd66-163">Registrace serveru</span><span class="sxs-lookup"><span data-stu-id="bbd66-163">Register Server</span></span>
* <span data-ttu-id="bbd66-164">Plán zálohování</span><span class="sxs-lookup"><span data-stu-id="bbd66-164">Schedule Backup</span></span>
* <span data-ttu-id="bbd66-165">Zálohovat nyní</span><span class="sxs-lookup"><span data-stu-id="bbd66-165">Back Up now</span></span>
* <span data-ttu-id="bbd66-166">Změnit vlastnosti</span><span class="sxs-lookup"><span data-stu-id="bbd66-166">Change Properties</span></span>

![Konzole akce agenta.](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> <span data-ttu-id="bbd66-168">K **obnovit Data**, najdete v části [obnovit soubory do systému Windows server nebo klientský počítač systému Windows](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="bbd66-168">To **Recover Data**, see [Restore files to a Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

### <a name="modify-an-existing-backup"></a><span data-ttu-id="bbd66-169">Upravit existující zálohy</span><span class="sxs-lookup"><span data-stu-id="bbd66-169">Modify an existing backup</span></span>
1. <span data-ttu-id="bbd66-170">V agentovi nástroje Microsoft Azure Backup klikněte na tlačítko **plánem zálohování**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-170">In the Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. <span data-ttu-id="bbd66-172">V **Průvodce plánem zálohování** ponechte **provádět změny položky zálohování nebo časy** možnost a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-172">In the **Schedule Backup Wizard** leave the **Make changes to backup items or times** option selected and click **Next**.</span></span>

    ![Upravit naplánovaného zálohování](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="bbd66-174">Pokud chcete přidat nebo změnit položky, na **výběr položek k zálohování** obrazovce klikněte na tlačítko **přidat položky**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-174">If you want to add or change items, on the **Select Items to Backup** screen click **Add Items**.</span></span>

    <span data-ttu-id="bbd66-175">Můžete také nastavit **nastavení vyloučení** z této stránky v průvodci.</span><span class="sxs-lookup"><span data-stu-id="bbd66-175">You can also set **Exclusion Settings** from this page in the wizard.</span></span> <span data-ttu-id="bbd66-176">Pokud chcete vyloučit soubory nebo typy souborů, přečtěte si postup pro přidání [nastavení vyloučení](#exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="bbd66-176">If you want to exclude files or file types read the procedure for adding [exclusion settings](#exclusion-settings).</span></span>
4. <span data-ttu-id="bbd66-177">Vyberte soubory a složky, které chcete zálohovat a klikněte na tlačítko **nevadí**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-177">Select the files and folders you want to back up and click **Okay**.</span></span>

    ![Přidání položek](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. <span data-ttu-id="bbd66-179">Zadejte **plán zálohování** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-179">Specify the **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="bbd66-180">Můžete naplánovat denní (probíhající maximálně 3krát denně) nebo týdenní zálohování.</span><span class="sxs-lookup"><span data-stu-id="bbd66-180">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Zadejte plán zálohování](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="bbd66-182">Zadat plán zálohování je podrobně popsány v tomto [článku](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="bbd66-182">Specifying the backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >
6. <span data-ttu-id="bbd66-183">Vyberte **zásady uchovávání informací** pro záložní kopii a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-183">Select the **Retention Policy** for the backup copy and click **Next**.</span></span>

    ![Vyberte vaše zásady uchovávání informací](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. <span data-ttu-id="bbd66-185">Na **potvrzení** obrazovky Zkontrolujte zadané informace a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-185">On the **Confirmation** screen review the information and click **Finish**.</span></span>
8. <span data-ttu-id="bbd66-186">Až průvodce dokončí vytváření **plán zálohování**, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-186">Once the wizard finishes creating the **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="bbd66-187">Po úpravě ochrany, můžete ověřit, že zálohy spouštějí správně přechodem na **úlohy** kartě a potvrzující, že změny se projeví v úloh zálohování.</span><span class="sxs-lookup"><span data-stu-id="bbd66-187">After modifying protection, you can confirm that backups are triggering correctly by going to the **Jobs** tab and confirming that changes are reflected in the backup jobs.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="bbd66-188">Povolit omezení šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="bbd66-188">Enable Network Throttling</span></span>
<span data-ttu-id="bbd66-189">Agent Azure Backup poskytuje omezování karta, která umožňuje řídit použití šířky pásma sítě při přenosu dat.</span><span class="sxs-lookup"><span data-stu-id="bbd66-189">The Azure Backup agent provides a Throttling tab which allows you to control how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="bbd66-190">Tento ovládací prvek může být užitečné, pokud potřebujete zálohovat data v pracovní době, ale nechcete, aby proces zálohování narušoval ostatní internetový provoz.</span><span class="sxs-lookup"><span data-stu-id="bbd66-190">This control can be helpful if you need to back up data during work hours but do not want the backup process to interfere with other internet traffic.</span></span> <span data-ttu-id="bbd66-191">Omezování dat přenos platí pro zálohování a obnovení aktivity.</span><span class="sxs-lookup"><span data-stu-id="bbd66-191">Throttling of data transfer applies to back up and restore activities.</span></span>  

<span data-ttu-id="bbd66-192">Povolit omezení:</span><span class="sxs-lookup"><span data-stu-id="bbd66-192">To enable throttling:</span></span>

1. <span data-ttu-id="bbd66-193">V **agenta zálohování**, klikněte na tlačítko **změnit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-193">In the **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="bbd66-194">Vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="bbd66-194">Select the **Enable internet bandwidth usage throttling for backup operations** checkbox.</span></span>

    ![Omezení šířky pásma sítě](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. <span data-ttu-id="bbd66-196">Jakmile jste povolili omezování, zadejte povolenou šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-196">Once you have enabled throttling, specify the allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="bbd66-197">Hodnoty šířky pásma začít až 512 kilobitů za sekundu (Kbps) a můžete přejít až 1023 MB za sekundu (Mbps).</span><span class="sxs-lookup"><span data-stu-id="bbd66-197">The bandwidth values begin at 512 kilobytes per second (Kbps) and can go up to 1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="bbd66-198">Můžete také určit spuštění a dokončení pro **pracovní hodiny**, a které dny v týdnu, jsou považovány za pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="bbd66-198">You can also designate the start and finish for **Work hours**, and which days of the week are considered Work days.</span></span> <span data-ttu-id="bbd66-199">Mimo pracovní dobu v určené době se považuje za mimo pracovní dobu.</span><span class="sxs-lookup"><span data-stu-id="bbd66-199">The time outside of the designated Work hours is considered to be non-work hours.</span></span>
4. <span data-ttu-id="bbd66-200">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-200">Click **OK**.</span></span>

## <a name="exclusion-settings"></a><span data-ttu-id="bbd66-201">Nastavení vyloučení</span><span class="sxs-lookup"><span data-stu-id="bbd66-201">Exclusion settings</span></span>
1. <span data-ttu-id="bbd66-202">Otevřete **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="bbd66-202">Open the **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Otevřete agenta zálohování](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. <span data-ttu-id="bbd66-204">V agentovi nástroje Microsoft Azure Backup klikněte na tlačítko **plánem zálohování**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-204">In the Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. <span data-ttu-id="bbd66-206">V ponechejte Průvodce plánem zálohování **provádět změny položky zálohování nebo časy** možnost a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-206">In the Schedule Backup Wizard leave the **Make changes to backup items or times** option selected and click **Next**.</span></span>

    ![Změna plánu](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="bbd66-208">Klikněte na tlačítko **nastavení vyloučení**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-208">Click **Exclusions Settings**.</span></span>

    ![Vyberte položky, které chcete vyloučit](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. <span data-ttu-id="bbd66-210">Klikněte na tlačítko **Přidat vyloučení**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-210">Click **Add Exclusion**.</span></span>

    ![Přidat vyloučení](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. <span data-ttu-id="bbd66-212">Vyberte umístění a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-212">Select the location and then, click **OK**.</span></span>

    ![Vyberte umístění pro vyloučení](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. <span data-ttu-id="bbd66-214">Přidat příponu souboru v **typ souboru** pole.</span><span class="sxs-lookup"><span data-stu-id="bbd66-214">Add the file extension in the **File Type** field.</span></span>

    ![Vyloučit podle typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    <span data-ttu-id="bbd66-216">Přidání rozšíření MP3</span><span class="sxs-lookup"><span data-stu-id="bbd66-216">Adding an .mp3 extension</span></span>

    ![Příklad typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    <span data-ttu-id="bbd66-218">Chcete-li přidat jiné rozšíření, klikněte na tlačítko **Přidat vyloučení** a zadejte jinou příponu typu souboru (Přidání rozšíření .jpeg).</span><span class="sxs-lookup"><span data-stu-id="bbd66-218">To add another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Další příklad typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. <span data-ttu-id="bbd66-220">Pokud jste přidali všechna rozšíření, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-220">When you've added all the extensions, click **OK**.</span></span>
9. <span data-ttu-id="bbd66-221">Pomocí Průvodce plánem zálohování nadále pokračovat kliknutím **Další** dokud **potvrzovací stránku**, pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="bbd66-221">Continue through the Schedule Backup Wizard by clicking **Next** until the **Confirmation page**, then click **Finish**.</span></span>

    ![Vyloučení potvrzení](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a><span data-ttu-id="bbd66-223">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bbd66-223">Next steps</span></span>
* [<span data-ttu-id="bbd66-224">Obnovení systému Windows Server nebo klienta Windows z Azure</span><span class="sxs-lookup"><span data-stu-id="bbd66-224">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="bbd66-225">Další informace o zálohování Azure, najdete v části [Přehled zálohování Azure](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="bbd66-225">To learn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="bbd66-226">Přejděte [fórum Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="bbd66-226">Visit the [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
