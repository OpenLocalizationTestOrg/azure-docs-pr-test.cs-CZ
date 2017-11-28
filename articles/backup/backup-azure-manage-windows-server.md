---
title: aaaManage Azure recovery services trezory a servery | Microsoft Docs
description: "Použijte tento kurz toolearn jak trezory služeb zotavení Azure toomanage a servery."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: 4eea984b-7ed6-4600-ac60-99d2e9cb6d8a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markgal
ms.openlocfilehash: b4c35c86faa0828b3c63a13b85c095c0cbaba50e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-recovery-services-vaults-and-servers-for-windows-machines"></a><span data-ttu-id="acff1-103">Monitorování a správa serverů a trezorů služby Azure Recovery Services pro počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="acff1-103">Monitor and manage Azure recovery services vaults and servers for Windows machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="acff1-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="acff1-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="acff1-105">Classic</span><span class="sxs-lookup"><span data-stu-id="acff1-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="acff1-106">V tomto článku najdete přehled hello zálohování monitorování a Správa úloh k dispozici prostřednictvím hello Azure portal a hello Microsoft Azure Backup agent.</span><span class="sxs-lookup"><span data-stu-id="acff1-106">In this article you'll find an overview of hello backup monitor and management tasks available through hello Azure portal and hello Microsoft Azure Backup agent.</span></span> <span data-ttu-id="acff1-107">Tento článek předpokládá už máte předplatné Azure a vytvořili alespoň jeden trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="acff1-107">This article assumes you already have an Azure subscription and have created at least one Recovery Services vault.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a><span data-ttu-id="acff1-108">Otevřete trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="acff1-108">Open a Recovery Services vault</span></span>

<span data-ttu-id="acff1-109">panelu trezoru služeb zotavení Hello se zobrazuje podrobnosti o hello nebo atributů trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="acff1-109">hello Recovery Services vault dashboard shows you hello details or attributes of a Recovery Services vault.</span></span>

1. <span data-ttu-id="acff1-110">Přihlaste se toohello [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="acff1-110">Sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="acff1-111">V nabídce centra hello, klikněte na tlačítko **více služeb**.</span><span class="sxs-lookup"><span data-stu-id="acff1-111">On hello Hub menu, click **More Services**.</span></span>

    ![Otevřete seznam krok trezory služeb zotavení 1](./media/backup-azure-manage-windows-server/open-rs-vault-list.png) <br/>

3. <span data-ttu-id="acff1-113">Chcete tooopen trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="acff1-113">You want tooopen a Recovery Services vault.</span></span> <span data-ttu-id="acff1-114">V dialogovém okně hello, začněte psát **služeb zotavení**.</span><span class="sxs-lookup"><span data-stu-id="acff1-114">In hello dialog box, start typing **Recovery Services**.</span></span> <span data-ttu-id="acff1-115">Během zadávání hello seznam bude filtrovat podle vašeho zadání.</span><span class="sxs-lookup"><span data-stu-id="acff1-115">As you begin typing, hello list will filter based on your input.</span></span> <span data-ttu-id="acff1-116">Klikněte na tlačítko **trezory služeb zotavení** toodisplay hello seznam služeb zotavení trezory ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="acff1-116">Click **Recovery Services vaults** toodisplay hello list of Recovery Services vaults in your subscription.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-manage-windows-server/browse-to-rs-vaults-2.png) <br/>

    <span data-ttu-id="acff1-118">Otevře se Hello seznamu trezorů služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="acff1-118">hello list of Recovery Services vaults opens.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. <span data-ttu-id="acff1-120">Hello seznamu trezorů vyberte název hello hello chcete tooopen trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="acff1-120">From hello list of vaults, select hello name of hello Recovery Services vault you want tooopen.</span></span> <span data-ttu-id="acff1-121">Otevře se okno řídicího panelu trezoru služeb zotavení Hello.</span><span class="sxs-lookup"><span data-stu-id="acff1-121">hello Recovery Services vault dashboard blade opens.</span></span>

    ![panelu trezoru služeb zotavení](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    <span data-ttu-id="acff1-123">Teď, když máte otevřený trezor služeb zotavení hello, vyzkoušejte některý z hello monitorování nebo Správa úloh.</span><span class="sxs-lookup"><span data-stu-id="acff1-123">Now that you have opened hello Recovery Services vault, try any of hello monitoring or management tasks.</span></span>

## <a name="monitor-backup-jobs-and-alerts"></a><span data-ttu-id="acff1-124">Úlohy zálohování monitorování a výstrahy</span><span class="sxs-lookup"><span data-stu-id="acff1-124">Monitor backup jobs and alerts</span></span>

<span data-ttu-id="acff1-125">Sledování úloh a výstrahy z hello trezor služeb zotavení řídicí panel, kde uvidíte:</span><span class="sxs-lookup"><span data-stu-id="acff1-125">You monitor jobs and alerts from hello Recovery Services vault dashboard, where you see:</span></span>

* <span data-ttu-id="acff1-126">Podrobnosti výstrahy zálohy</span><span class="sxs-lookup"><span data-stu-id="acff1-126">Backup alerts details</span></span>
* <span data-ttu-id="acff1-127">Soubory a složky, jakož i virtuální počítače Azure chráněna v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="acff1-127">Files and folders, as well as Azure virtual machines protected in hello cloud</span></span>
* <span data-ttu-id="acff1-128">Celkový úložný využívat v Azure</span><span class="sxs-lookup"><span data-stu-id="acff1-128">Total storage consumed in Azure</span></span>
* <span data-ttu-id="acff1-129">Stav úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="acff1-129">Backup job status</span></span>

![Úlohy zálohování řídicí panel](./media/backup-azure-manage-windows-server/dashboard-tiles.png)

<span data-ttu-id="acff1-131">Kliknutím na hello informace v každé z těchto dlaždic otevřete hello přidružené okno, kde budete spravovat související úlohy.</span><span class="sxs-lookup"><span data-stu-id="acff1-131">Clicking hello information in each of these tiles will open hello associated blade where you manage related tasks.</span></span>

<span data-ttu-id="acff1-132">Z hello horní části hello řídicí panel:</span><span class="sxs-lookup"><span data-stu-id="acff1-132">From hello top of hello Dashboard:</span></span>

* <span data-ttu-id="acff1-133">Nastavení poskytuje přístup k dispozici úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="acff1-133">Settings provides access available backup tasks.</span></span>
* <span data-ttu-id="acff1-134">Zálohování – pomáhá zálohujete nové soubory a složky (nebo virtuálních počítačích Azure) služeb zotavení toohello trezoru.</span><span class="sxs-lookup"><span data-stu-id="acff1-134">Backup - helps you back up new files and folders (or Azure VMs) toohello Recovery Services vault.</span></span>
* <span data-ttu-id="acff1-135">Odstranění – Pokud obnovení služeb trezoru se už používá, chcete-li odstranit toofree do prostoru úložiště.</span><span class="sxs-lookup"><span data-stu-id="acff1-135">Delete - If a recovery services vault is no longer being used, you can delete it toofree up storage space.</span></span> <span data-ttu-id="acff1-136">Odstranění je aktivní jenom po odstranění všech chráněných serverech z trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="acff1-136">Delete is only enabled after all protected servers have been deleted from hello vault.</span></span>

![Úlohy zálohování řídicí panel](./media/backup-azure-manage-windows-server/dashboard-tasks.png)

## <a name="alerts-for-backups-using-azure-backup-agent"></a><span data-ttu-id="acff1-138">Výstrahy pro zálohování pomocí agenta Azure backup:</span><span class="sxs-lookup"><span data-stu-id="acff1-138">Alerts for backups using Azure backup agent:</span></span>
| <span data-ttu-id="acff1-139">Úroveň výstrahy</span><span class="sxs-lookup"><span data-stu-id="acff1-139">Alert Level</span></span> | <span data-ttu-id="acff1-140">Zasílání upozornění</span><span class="sxs-lookup"><span data-stu-id="acff1-140">Alerts sent</span></span> |
| --- | --- |
| <span data-ttu-id="acff1-141">Kritické</span><span class="sxs-lookup"><span data-stu-id="acff1-141">Critical</span></span> |<span data-ttu-id="acff1-142">Selhání zálohování, obnovení selhání</span><span class="sxs-lookup"><span data-stu-id="acff1-142">Backup failure, recovery failure</span></span> |
| <span data-ttu-id="acff1-143">Upozornění</span><span class="sxs-lookup"><span data-stu-id="acff1-143">Warning</span></span> |<span data-ttu-id="acff1-144">Zálohování dokončeno s varováním (při méně než sto soubory nejsou zálohovány z důvodu problémů toocorruption a více než jeden milión soubory jsou zálohovány úspěšně)</span><span class="sxs-lookup"><span data-stu-id="acff1-144">Backup completed with warnings (when fewer than one hundred files are not backed up due toocorruption issues, and more than one million files are successfully backed up)</span></span> |
| <span data-ttu-id="acff1-145">Informační</span><span class="sxs-lookup"><span data-stu-id="acff1-145">Informational</span></span> |<span data-ttu-id="acff1-146">Žádný</span><span class="sxs-lookup"><span data-stu-id="acff1-146">None</span></span> |

## <a name="manage-backup-alerts"></a><span data-ttu-id="acff1-147">Správa výstrah zálohování</span><span class="sxs-lookup"><span data-stu-id="acff1-147">Manage Backup alerts</span></span>
<span data-ttu-id="acff1-148">Klikněte na tlačítko hello **zálohování výstrahy** dlaždice tooopen hello **zálohování výstrahy** okno a spravovat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="acff1-148">Click hello **Backup Alerts** tile tooopen hello **Backup Alerts** blade and manage alerts.</span></span>

![Výstrahy zálohy](./media/backup-azure-manage-windows-server/manage-backup-alerts.png)

<span data-ttu-id="acff1-150">Hello zálohování výstrahy, které ukazuje, dlaždice hello počet:</span><span class="sxs-lookup"><span data-stu-id="acff1-150">hello Backup Alerts tile shows you hello number of:</span></span>

* <span data-ttu-id="acff1-151">kritické výstrahy nepřeložené za posledních 24 hodin</span><span class="sxs-lookup"><span data-stu-id="acff1-151">critical alerts unresolved in last 24 hours</span></span>
* <span data-ttu-id="acff1-152">upozorňující výstrahy nepřeložené za posledních 24 hodin</span><span class="sxs-lookup"><span data-stu-id="acff1-152">warning alerts unresolved in last 24 hours</span></span>

<span data-ttu-id="acff1-153">Kliknutím na každý z těchto odkazů trvá toohello **zálohování výstrahy** okno s filtrované zobrazení tyto výstrahy (kritický nebo upozornění).</span><span class="sxs-lookup"><span data-stu-id="acff1-153">Clicking on each of these links takes you toohello **Backup Alerts** blade with a filtered view of these alerts (critical or warning).</span></span>

<span data-ttu-id="acff1-154">V okně Zálohování výstrahy hello můžete:</span><span class="sxs-lookup"><span data-stu-id="acff1-154">From hello Backup Alerts blade, you:</span></span>

* <span data-ttu-id="acff1-155">Vyberte příslušné informace o tooinclude hello s oznámení.</span><span class="sxs-lookup"><span data-stu-id="acff1-155">Choose hello appropriate information tooinclude with your alerts.</span></span>

    ![Zvolit sloupce](./media/backup-azure-manage-windows-server/choose-alerts-colunms.png)
* <span data-ttu-id="acff1-157">Filtrování výstrah pro závažnost, stavu a počáteční nebo koncové časy.</span><span class="sxs-lookup"><span data-stu-id="acff1-157">Filter alerts for severity, status and start/end times.</span></span>

    ![Filtrování výstrah](./media/backup-azure-manage-windows-server/filter-alerts.png)
* <span data-ttu-id="acff1-159">Konfigurace oznámení pro závažností, četnosti a příjemce, a také zapnout výstrahy nebo vypnout.</span><span class="sxs-lookup"><span data-stu-id="acff1-159">Configure notifications for severity, frequency and recipients, as well as turn alerts on or off.</span></span>

    ![Filtrování výstrah](./media/backup-azure-manage-windows-server/configure-notifications.png)

<span data-ttu-id="acff1-161">Pokud **za výstrahy** je vybrán jako hello **oznámení** frekvence dojde k žádné seskupení nebo snížení e-mailů.</span><span class="sxs-lookup"><span data-stu-id="acff1-161">If **Per Alert** is selected as hello **Notify** frequency no grouping or reduction in emails occurs.</span></span> <span data-ttu-id="acff1-162">Každá výstraha výsledkem 1 oznámení.</span><span class="sxs-lookup"><span data-stu-id="acff1-162">Every alert results in 1 notification.</span></span> <span data-ttu-id="acff1-163">Toto je výchozí nastavení hello a e-mailové řešení hello je také odesílat okamžitě.</span><span class="sxs-lookup"><span data-stu-id="acff1-163">This is hello default setting and hello resolution email is also sent out immediately.</span></span>

<span data-ttu-id="acff1-164">Pokud **hodinové Digest** je vybrán jako hello **oznámení** nepřeložené frekvenci, jeden e-mail je odeslán toohello uživatele upozorněním, které nejsou nové výstrahy generované v hello poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="acff1-164">If **Hourly Digest** is selected as hello **Notify** frequency one email is sent toohello user telling them that there are unresolved new alerts generated in hello last hour.</span></span> <span data-ttu-id="acff1-165">E-mailu s řešení odeslání na konci hello hello hodina.</span><span class="sxs-lookup"><span data-stu-id="acff1-165">A resolution email is sent out at hello end of hello hour.</span></span>

<span data-ttu-id="acff1-166">Můžete odesílat upozornění pro hello následující úrovně závažnosti:</span><span class="sxs-lookup"><span data-stu-id="acff1-166">Alerts can be sent for hello following severity levels:</span></span>

* <span data-ttu-id="acff1-167">Kritické</span><span class="sxs-lookup"><span data-stu-id="acff1-167">critical</span></span>
* <span data-ttu-id="acff1-168">Upozornění</span><span class="sxs-lookup"><span data-stu-id="acff1-168">warning</span></span>
* <span data-ttu-id="acff1-169">Informace o</span><span class="sxs-lookup"><span data-stu-id="acff1-169">information</span></span>

<span data-ttu-id="acff1-170">Deaktivovat výstrahy hello s hello **deaktivovat** tlačítka na okno Podrobnosti úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="acff1-170">You inactivate hello alert with hello **inactivate** button in hello job details blade.</span></span> <span data-ttu-id="acff1-171">Po kliknutí na tlačítko deaktivovat, můžete zadat poznámky k řešení.</span><span class="sxs-lookup"><span data-stu-id="acff1-171">When you click inactivate, you can provide resolution notes.</span></span>

<span data-ttu-id="acff1-172">Vyberte sloupce hello chcete tooappear jako součást hello výstraha s hello **zvolit sloupce** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="acff1-172">You choose hello columns you want tooappear as part of hello alert with hello **Choose columns** button.</span></span>

> [!NOTE]
> <span data-ttu-id="acff1-173">Z hello **nastavení** okně Spravovat výstrahy zálohy výběrem **monitorování a sestavy > Výstrahy a události > výstrahy zálohy** a pak levým na **filtru** nebo  **Konfigurace oznámení**.</span><span class="sxs-lookup"><span data-stu-id="acff1-173">From hello **Settings** blade, you manage backup alerts by selecting **Monitoring and Reports > Alerts and Events > Backup Alerts** and then clicking **Filter** or **Configure Notifications**.</span></span>
>
>

## <a name="manage-backup-items"></a><span data-ttu-id="acff1-174">Správa zálohování položek</span><span class="sxs-lookup"><span data-stu-id="acff1-174">Manage Backup items</span></span>
<span data-ttu-id="acff1-175">Správa místního zálohování je nyní k dispozici na portálu pro správu hello.</span><span class="sxs-lookup"><span data-stu-id="acff1-175">Managing on-premises backups is now available in hello management portal.</span></span> <span data-ttu-id="acff1-176">V části zálohování hello řídicího panelu hello hello **položky zálohování** dlaždice ukazuje počet hello zálohování položek, které chráněné toohello trezoru.</span><span class="sxs-lookup"><span data-stu-id="acff1-176">In hello Backup section of hello dashboard, hello **Backup Items** tile shows hello number of backup items protected toohello vault.</span></span>

<span data-ttu-id="acff1-177">Klikněte na tlačítko **složek souborů** v hello dlaždici zálohování položek.</span><span class="sxs-lookup"><span data-stu-id="acff1-177">Click **File-Folders** in hello Backup Items tile.</span></span>

![Dlaždice položky zálohování](./media/backup-azure-manage-windows-server/backup-items-tile.png)

<span data-ttu-id="acff1-179">Hello položky zálohování typu, otevře se okno s hello filtrovat sadu tooFile složku, kde uvidíte každé konkrétní zálohy položka v seznamu.</span><span class="sxs-lookup"><span data-stu-id="acff1-179">hello Backup Items blade opens with hello filter set tooFile-Folder where you see each specific backup item listed.</span></span>

![Zálohování položek](./media/backup-azure-manage-windows-server/backup-item-list.png)

<span data-ttu-id="acff1-181">Pokud vyberete konkrétní zálohování položku ze seznamu hello, uvidíte hello základní podrobnosti pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="acff1-181">If you select a specific backup item from hello list, you see hello essential details for that item.</span></span>

> [!NOTE]
> <span data-ttu-id="acff1-182">Z hello **nastavení** okně Spravovat soubory a složky tak, že vyberete **chráněné položky > Zálohování položek** a potom vyberete **složek souborů** z hello rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="acff1-182">From hello **Settings** blade, you manage files and folders by selecting **Protected Items > Backup Items** and then selecting **File-Folders** from hello drop down menu.</span></span>
>
>

![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/backup-files-and-folders.png)

## <a name="manage-backup-jobs"></a><span data-ttu-id="acff1-184">Při správě úloh zálohování</span><span class="sxs-lookup"><span data-stu-id="acff1-184">Manage Backup jobs</span></span>
<span data-ttu-id="acff1-185">Úlohy zálohování pro místní (při hello na místním serveru je zálohování tooAzure) a zálohování Azure se zobrazují na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="acff1-185">Backup jobs for both on-premises (when hello on-premises server is backing up tooAzure) and Azure backups are visible in hello dashboard.</span></span>

<span data-ttu-id="acff1-186">Ve hello zálohování části hello řídicího panelu zobrazí dlaždice úlohy zálohování hello hello počet úloh:</span><span class="sxs-lookup"><span data-stu-id="acff1-186">In hello Backup section of hello dashboard, hello Backup job tile shows hello number of jobs:</span></span>

* <span data-ttu-id="acff1-187">v průběhu</span><span class="sxs-lookup"><span data-stu-id="acff1-187">in progress</span></span>
* <span data-ttu-id="acff1-188">hello posledních 24 hodin se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="acff1-188">failed in hello last 24 hours.</span></span>

<span data-ttu-id="acff1-189">toomanage vaše úlohy zálohování, klikněte na tlačítko hello **úlohy zálohování** dlaždici, což otevře okno úlohy zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="acff1-189">toomanage your backup jobs, click hello **Backup Jobs** tile, which opens hello Backup Jobs blade.</span></span>

![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/backup-jobs.png)

<span data-ttu-id="acff1-191">Upravte hello informace, které jsou k dispozici v okně úlohy zálohování hello s hello **zvolit sloupce** tlačítko hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="acff1-191">You modify hello information available in hello Backup Jobs blade with hello **Choose columns** button at hello top of hello page.</span></span>

<span data-ttu-id="acff1-192">Použití hello **filtru** tlačítko tooselect mezi soubory a složky a záloha virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="acff1-192">Use hello **Filter** button tooselect between Files and folders and Azure virtual machine backup.</span></span>

<span data-ttu-id="acff1-193">Pokud nevidíte zálohovaný soubory a složky, klikněte na tlačítko **filtru** tlačítka v horní části hello hello stránku a vyberte **soubory a složky** z hello typ položky nabídky.</span><span class="sxs-lookup"><span data-stu-id="acff1-193">If you don't see your backed up files and folders, click **Filter** button at hello top of hello page and select **Files and folders** from hello Item Type menu.</span></span>

> [!NOTE]
> <span data-ttu-id="acff1-194">Z hello **nastavení** okně Spravovat úlohy zálohování tak, že vyberete **monitorování a sestavy > úlohy > úlohy zálohování** a potom vyberete **složek souborů** rozevíracím hello nabídku.</span><span class="sxs-lookup"><span data-stu-id="acff1-194">From hello **Settings** blade, you manage backup jobs by selecting **Monitoring and Reports > Jobs > Backup Jobs** and then selecting **File-Folders** from hello drop down menu.</span></span>
>
>

## <a name="monitor-backup-usage"></a><span data-ttu-id="acff1-195">Sledování využití zálohování</span><span class="sxs-lookup"><span data-stu-id="acff1-195">Monitor Backup usage</span></span>
<span data-ttu-id="acff1-196">Ve hello zálohování části hello řídicího panelu zobrazí dlaždice využití zálohování hello hello úložiště využívat v Azure.</span><span class="sxs-lookup"><span data-stu-id="acff1-196">In hello Backup section of hello dashboard, hello Backup Usage tile shows hello storage consumed in Azure.</span></span> <span data-ttu-id="acff1-197">Využití úložiště je k dispozici pro:</span><span class="sxs-lookup"><span data-stu-id="acff1-197">Storage usage is provided for:</span></span>

* <span data-ttu-id="acff1-198">Využití úložiště LRS cloudu přidružený k trezoru hello</span><span class="sxs-lookup"><span data-stu-id="acff1-198">Cloud LRS storage usage associated with hello vault</span></span>
* <span data-ttu-id="acff1-199">Využití úložiště GRS cloudu přidružený k trezoru hello</span><span class="sxs-lookup"><span data-stu-id="acff1-199">Cloud GRS storage usage associated with hello vault</span></span>

## <a name="manage-your-production-servers"></a><span data-ttu-id="acff1-200">Spravovat vaše produkční servery</span><span class="sxs-lookup"><span data-stu-id="acff1-200">Manage your production servers</span></span>
<span data-ttu-id="acff1-201">Klikněte na provozních serverů, toomanage **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="acff1-201">toomanage your production servers, click **Settings**.</span></span>

<span data-ttu-id="acff1-202">V části Správa klikněte na **infrastruktura zálohování > produkční servery**.</span><span class="sxs-lookup"><span data-stu-id="acff1-202">Under Manage click **Backup infrastructure > Production Servers**.</span></span>

<span data-ttu-id="acff1-203">Zobrazí okno Hello provozních serverů všechny dostupné provozních serverů.</span><span class="sxs-lookup"><span data-stu-id="acff1-203">hello Production Servers blade lists of all your available production servers.</span></span> <span data-ttu-id="acff1-204">Klikněte na server v tooopen hello hello seznam-podrobnosti o serveru.</span><span class="sxs-lookup"><span data-stu-id="acff1-204">Click on a server in hello list tooopen hello server details.</span></span>

![Chráněné položky](./media/backup-azure-manage-windows-server/production-server-list.png)


## <a name="open-hello-azure-backup-agent"></a><span data-ttu-id="acff1-206">Otevřete hello agenta Azure Backup</span><span class="sxs-lookup"><span data-stu-id="acff1-206">Open hello Azure Backup agent</span></span>
<span data-ttu-id="acff1-207">Otevřete hello **Microsoft Azure Backup agent** (můžete najít tak, že váš počítač pro *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="acff1-207">Open hello **Microsoft Azure Backup agent** (you find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/snap-in-search.png)

<span data-ttu-id="acff1-209">Z hello **akce** k dispozici na hello napravo od konzoly agenta zálohování hello provádět následující úlohy správy hello:</span><span class="sxs-lookup"><span data-stu-id="acff1-209">From hello **Actions** available at hello right of hello backup agent console you perform hello following management tasks:</span></span>

* <span data-ttu-id="acff1-210">Registrace serveru</span><span class="sxs-lookup"><span data-stu-id="acff1-210">Register Server</span></span>
* <span data-ttu-id="acff1-211">Plán zálohování</span><span class="sxs-lookup"><span data-stu-id="acff1-211">Schedule Backup</span></span>
* <span data-ttu-id="acff1-212">Zálohovat nyní</span><span class="sxs-lookup"><span data-stu-id="acff1-212">Back Up now</span></span>
* <span data-ttu-id="acff1-213">Změnit vlastnosti</span><span class="sxs-lookup"><span data-stu-id="acff1-213">Change Properties</span></span>

![Microsoft Azure Backup agent konzoly akce](./media/backup-azure-manage-windows-server/console-actions.png)

> [!NOTE]
> <span data-ttu-id="acff1-215">příliš**obnovit Data**, najdete v části [obnovit soubory tooa Windows server nebo klientský počítač systému Windows](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="acff1-215">too**Recover Data**, see [Restore files tooa Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

## <a name="modify-hello-backup-schedule"></a><span data-ttu-id="acff1-216">Upravte plán zálohování hello</span><span class="sxs-lookup"><span data-stu-id="acff1-216">Modify hello backup schedule</span></span>
1. <span data-ttu-id="acff1-217">V rámci Microsoft Azure Backup agent hello klikněte na tlačítko **plánem zálohování**.</span><span class="sxs-lookup"><span data-stu-id="acff1-217">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/schedule-backup.png)
2. <span data-ttu-id="acff1-219">V hello **Průvodce plánem zálohování** nechte hello **provádět změny toobackup položky nebo časy** možnost a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="acff1-219">In hello **Schedule Backup Wizard** leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="acff1-221">Pokud chcete tooadd nebo změnit položky na hello **vyberte položky tooBackup** obrazovce klikněte na tlačítko **přidat položky**.</span><span class="sxs-lookup"><span data-stu-id="acff1-221">If you want tooadd or change items, on hello **Select Items tooBackup** screen click **Add Items**.</span></span>

    <span data-ttu-id="acff1-222">Můžete také nastavit **nastavení vyloučení** z této stránky v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="acff1-222">You can also set **Exclusion Settings** from this page in hello wizard.</span></span> <span data-ttu-id="acff1-223">Pokud chcete tooexclude soubory nebo typy souborů číst hello postup pro přidání [nastavení vyloučení](#manage-exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="acff1-223">If you want tooexclude files or file types read hello procedure for adding [exclusion settings](#manage-exclusion-settings).</span></span>
4. <span data-ttu-id="acff1-224">Vyberte hello soubory a složky tooback nahoru a klikněte na **nevadí**.</span><span class="sxs-lookup"><span data-stu-id="acff1-224">Select hello files and folders you want tooback up and click **Okay**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/add-items-modify.png)
5. <span data-ttu-id="acff1-226">Zadejte hello **plán zálohování** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="acff1-226">Specify hello **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="acff1-227">Můžete naplánovat denní (probíhající maximálně 3krát denně) nebo týdenní zálohování.</span><span class="sxs-lookup"><span data-stu-id="acff1-227">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Položky k zálohování z Windows Serveru](./media/backup-azure-manage-windows-server/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="acff1-229">Zadat plán zálohování hello je podrobně popsány v tomto [článku](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="acff1-229">Specifying hello backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >

6. <span data-ttu-id="acff1-230">Vyberte hello **zásady uchovávání informací** pro záložní kopii hello a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="acff1-230">Select hello **Retention Policy** for hello backup copy and click **Next**.</span></span>

    ![Položky k zálohování z Windows Serveru](./media/backup-azure-manage-windows-server/select-retention-policy-modify.png)
7. <span data-ttu-id="acff1-232">Na hello **potvrzení** obrazovce zkontrolovat hello informace a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="acff1-232">On hello **Confirmation** screen review hello information and click **Finish**.</span></span>
8. <span data-ttu-id="acff1-233">Po dokončení Průvodce hello vytváření hello **plán zálohování**, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="acff1-233">Once hello wizard finishes creating hello **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="acff1-234">Po úpravě ochrany, můžete ověřit, zálohování, ke které spouštějí správně podle budete toohello **úlohy** kartě a potvrzující, že změny se projeví v hello zálohování úloh.</span><span class="sxs-lookup"><span data-stu-id="acff1-234">After modifying protection, you can confirm that backups are triggering correctly by going toohello **Jobs** tab and confirming that changes are reflected in hello backup jobs.</span></span>

## <a name="enable-network-throttling"></a><span data-ttu-id="acff1-235">Povolit omezení šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="acff1-235">Enable Network Throttling</span></span>

<span data-ttu-id="acff1-236">agent Azure Backup Hello poskytuje omezování karta, která vám umožní toocontrol použití šířky pásma sítě při přenosu dat.</span><span class="sxs-lookup"><span data-stu-id="acff1-236">hello Azure Backup agent provides a Throttling tab which allows you toocontrol how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="acff1-237">Tento ovládací prvek může být užitečné, pokud potřebujete tooback dat během pracovní doby, ale nechcete, aby proces zálohování toointerfere hello s ostatní internetový provoz.</span><span class="sxs-lookup"><span data-stu-id="acff1-237">This control can be helpful if you need tooback up data during work hours but do not want hello backup process toointerfere with other internet traffic.</span></span> <span data-ttu-id="acff1-238">Omezování dat přenos platí tooback nahoru a obnovit aktivity.</span><span class="sxs-lookup"><span data-stu-id="acff1-238">Throttling of data transfer applies tooback up and restore activities.</span></span>  

<span data-ttu-id="acff1-239">tooenable omezení:</span><span class="sxs-lookup"><span data-stu-id="acff1-239">tooenable throttling:</span></span>

1. <span data-ttu-id="acff1-240">V hello **agenta zálohování**, klikněte na tlačítko **změnit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="acff1-240">In hello **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="acff1-241">Na hello ** omezení karty, vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení**.</span><span class="sxs-lookup"><span data-stu-id="acff1-241">On hello **Throttling tab, select **Enable internet bandwidth usage throttling for backup operations**.</span></span>

    ![Omezení šířky pásma sítě](./media/backup-azure-manage-windows-server/throttling-dialog.png)

    <span data-ttu-id="acff1-243">Jakmile jste povolili omezování, zadejte hello povolený šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.</span><span class="sxs-lookup"><span data-stu-id="acff1-243">Once you have enabled throttling, specify hello allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="acff1-244">hodnoty šířky pásma Hello začít až 512 kilobitů za sekundu (Kbps) a můžete přejít do too1023 megabajtů za sekundu (Mbps).</span><span class="sxs-lookup"><span data-stu-id="acff1-244">hello bandwidth values begin at 512 kilobytes per second (Kbps) and can go up too1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="acff1-245">Můžete také určit hello zahájení a dokončení pro **pracovní hodiny**, a které dny v týdnu hello jsou považovány za pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="acff1-245">You can also designate hello start and finish for **Work hours**, and which days of hello week are considered Work days.</span></span> <span data-ttu-id="acff1-246">čas Hello mimo určené pracovní hodiny hello je považovány toobe mimo pracovní dobu.</span><span class="sxs-lookup"><span data-stu-id="acff1-246">hello time outside of hello designated Work hours is considered toobe non-work hours.</span></span>
3. <span data-ttu-id="acff1-247">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="acff1-247">Click **OK**.</span></span>

## <a name="manage-exclusion-settings"></a><span data-ttu-id="acff1-248">Spravovat nastavení vyloučení</span><span class="sxs-lookup"><span data-stu-id="acff1-248">Manage exclusion settings</span></span>
1. <span data-ttu-id="acff1-249">Otevřete hello **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="acff1-249">Open hello **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/snap-in-search.png)
2. <span data-ttu-id="acff1-251">V rámci Microsoft Azure Backup agent hello klikněte na tlačítko **plánem zálohování**.</span><span class="sxs-lookup"><span data-stu-id="acff1-251">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/schedule-backup.png)
3. <span data-ttu-id="acff1-253">V Průvodci plánování zálohování hello nechte hello **provádět změny toobackup položky nebo časy** možnost a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="acff1-253">In hello Schedule Backup Wizard leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="acff1-255">Klikněte na tlačítko **nastavení vyloučení**.</span><span class="sxs-lookup"><span data-stu-id="acff1-255">Click **Exclusions Settings**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclusion-settings.png)
5. <span data-ttu-id="acff1-257">Klikněte na tlačítko **Přidat vyloučení**.</span><span class="sxs-lookup"><span data-stu-id="acff1-257">Click **Add Exclusion**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/add-exclusion.png)
6. <span data-ttu-id="acff1-259">Vyberte umístění hello a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="acff1-259">Select hello location and then, click **OK**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclusion-location.png)
7. <span data-ttu-id="acff1-261">Přidat příponu souboru hello v hello **typ souboru** pole.</span><span class="sxs-lookup"><span data-stu-id="acff1-261">Add hello file extension in hello **File Type** field.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclude-file-type.png)

    <span data-ttu-id="acff1-263">Přidání rozšíření MP3</span><span class="sxs-lookup"><span data-stu-id="acff1-263">Adding an .mp3 extension</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclude-mp3.png)

    <span data-ttu-id="acff1-265">tooadd jiné rozšíření, klikněte na tlačítko **Přidat vyloučení** a zadejte jinou příponu typu souboru (Přidání rozšíření .jpeg).</span><span class="sxs-lookup"><span data-stu-id="acff1-265">tooadd another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclude-jpg.png)
8. <span data-ttu-id="acff1-267">Pokud jste přidali všechny hello rozšíření, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="acff1-267">When you've added all hello extensions, click **OK**.</span></span>
9. <span data-ttu-id="acff1-268">Kliknutím na pokračovat prostřednictvím hello Průvodce plánem zálohování **Další** dokud hello **potvrzovací stránku**, pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="acff1-268">Continue through hello Schedule Backup Wizard by clicking **Next** until hello **Confirmation page**, then click **Finish**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/finish-exclusions.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="acff1-270">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="acff1-270">Frequently asked questions</span></span>
<span data-ttu-id="acff1-271">**OTÁZKA Č. 1. Stav úlohy zálohování Hello zobrazuje jako dokončené v hello Azure backup agent, proč není ho získat projeví okamžitě v portálu?**</span><span class="sxs-lookup"><span data-stu-id="acff1-271">**Q1. hello backup job status shows as completed in hello Azure backup agent, why doesn't it get reflected immediately in portal?**</span></span>

<span data-ttu-id="acff1-272">Odpověď č. 1:</span><span class="sxs-lookup"><span data-stu-id="acff1-272">A1.</span></span> <span data-ttu-id="acff1-273">Existuje v maximální zpoždění 15 minut mezi hello stav úlohy zálohování se v hello Azure backup agent a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="acff1-273">There is at maximum delay of 15 mins between hello backup job status reflected in hello Azure backup agent and hello Azure portal.</span></span>

<span data-ttu-id="acff1-274">**Q.2 při selhání úlohy zálohování, jak dlouho trvá tooraise výstrahu?**</span><span class="sxs-lookup"><span data-stu-id="acff1-274">**Q.2 When a backup job fails, how long does it take tooraise an alert?**</span></span>

<span data-ttu-id="acff1-275">A.2 výstraha se vyvolá v rámci 20 minut hello Azure selhání zálohování.</span><span class="sxs-lookup"><span data-stu-id="acff1-275">A.2 An alert is raised within 20 mins of hello Azure backup failure.</span></span>

<span data-ttu-id="acff1-276">**3. ČTVRTLETÍ. Je případ, kdy e-mailu neodešle, pokud jsou nakonfigurované oznámení?**</span><span class="sxs-lookup"><span data-stu-id="acff1-276">**Q3. Is there a case where an email won’t be sent if notifications are configured?**</span></span>

<span data-ttu-id="acff1-277">Odpověď 3.</span><span class="sxs-lookup"><span data-stu-id="acff1-277">A3.</span></span> <span data-ttu-id="acff1-278">Níže jsou hello případech při hello upozornění nebude odesláno v pořadí tooreduce hello výstrahy nepůsobily:</span><span class="sxs-lookup"><span data-stu-id="acff1-278">Below are hello cases when hello notification will not be sent in order tooreduce hello alert noise:</span></span>

* <span data-ttu-id="acff1-279">Pokud oznámení jsou nakonfigurovány každou hodinu a výstrahu, je vyvolána a vyřešené v rámci hello hodinu</span><span class="sxs-lookup"><span data-stu-id="acff1-279">If notifications are configured hourly and an alert is raised and resolved within hello hour</span></span>
* <span data-ttu-id="acff1-280">Úloha je zrušena.</span><span class="sxs-lookup"><span data-stu-id="acff1-280">Job is canceled.</span></span>
* <span data-ttu-id="acff1-281">Druhý úloha zálohování se nezdařila, protože probíhá úloha původní zálohování.</span><span class="sxs-lookup"><span data-stu-id="acff1-281">Second backup job failed because original backup job is in progress.</span></span>

## <a name="troubleshooting-monitoring-issues"></a><span data-ttu-id="acff1-282">Monitorování řešení potíží</span><span class="sxs-lookup"><span data-stu-id="acff1-282">Troubleshooting Monitoring Issues</span></span>
<span data-ttu-id="acff1-283">**Problém:** úlohy a výstrahy z agenta Azure Backup hello se nezobrazí na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="acff1-283">**Issue:** Jobs and/or alerts from hello Azure Backup agent do not appear in hello portal.</span></span>

<span data-ttu-id="acff1-284">**Řešení potíží:** hello procesu ```OBRecoveryServicesManagementAgent```, odešle hello úlohy a výstrahy toohello dat služby zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="acff1-284">**Troubleshooting steps:** hello process, ```OBRecoveryServicesManagementAgent```, sends hello job and alert data toohello Azure Backup service.</span></span> <span data-ttu-id="acff1-285">Příležitostně tento proces může zablokovat nebo vypnutí.</span><span class="sxs-lookup"><span data-stu-id="acff1-285">Occasionally this process can become stuck or shutdown.</span></span>

1. <span data-ttu-id="acff1-286">proces hello tooverify neběží, otevřete **Správce úloh** a zkontrolujte, jestli hello ```OBRecoveryServicesManagementAgent``` proces běží.</span><span class="sxs-lookup"><span data-stu-id="acff1-286">tooverify hello process is not running, open **Task Manager** and check if hello ```OBRecoveryServicesManagementAgent``` process is running.</span></span>
2. <span data-ttu-id="acff1-287">Za předpokladu, že proces hello neběží, otevřete **ovládací panely** a procházet hello seznam služeb.</span><span class="sxs-lookup"><span data-stu-id="acff1-287">Assuming that hello process is not running, open **Control Panel** and browse hello list of services.</span></span> <span data-ttu-id="acff1-288">Spustit nebo restartovat **správy agenta služeb zotavení Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="acff1-288">Start or restart **Microsoft Azure Recovery Services Management Agent**.</span></span>

    <span data-ttu-id="acff1-289">Další informace vyhledejte hello protokoly na:</span><span class="sxs-lookup"><span data-stu-id="acff1-289">For further information, browse hello logs at:</span></span><br/><span data-ttu-id="acff1-290">
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Například:</span><span class="sxs-lookup"><span data-stu-id="acff1-290">
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*` For example:</span></span><br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a><span data-ttu-id="acff1-291">Další kroky</span><span class="sxs-lookup"><span data-stu-id="acff1-291">Next steps</span></span>
* [<span data-ttu-id="acff1-292">Obnovení systému Windows Server nebo klienta Windows z Azure</span><span class="sxs-lookup"><span data-stu-id="acff1-292">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="acff1-293">toolearn Další informace o zálohování Azure, najdete v části [Přehled zálohování Azure](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="acff1-293">toolearn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="acff1-294">Navštivte hello [fóru služby Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="acff1-294">Visit hello [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
