---
title: "Spravovat servery a trezory služeb zotavení Azure | Microsoft Docs"
description: "Pomocí tohoto kurzu se dozvíte, jak spravovat servery a trezory služeb zotavení Azure."
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
ms.openlocfilehash: 5922e308f5c205a07bd329c28322ae82cea0e1fa
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-and-manage-azure-recovery-services-vaults-and-servers-for-windows-machines"></a><span data-ttu-id="4e9a1-103">Monitorování a správa serverů a trezorů služby Azure Recovery Services pro počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="4e9a1-103">Monitor and manage Azure recovery services vaults and servers for Windows machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4e9a1-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4e9a1-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="4e9a1-105">Classic</span><span class="sxs-lookup"><span data-stu-id="4e9a1-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="4e9a1-106">V tomto článku najdete přehled zálohování monitorování a Správa úloh k dispozici prostřednictvím portálu Azure a Microsoft Azure Backup agent.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-106">In this article you'll find an overview of the backup monitor and management tasks available through the Azure portal and the Microsoft Azure Backup agent.</span></span> <span data-ttu-id="4e9a1-107">Tento článek předpokládá už máte předplatné Azure a vytvořili alespoň jeden trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-107">This article assumes you already have an Azure subscription and have created at least one Recovery Services vault.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a><span data-ttu-id="4e9a1-108">Otevřete trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="4e9a1-108">Open a Recovery Services vault</span></span>

<span data-ttu-id="4e9a1-109">Řídícím panelu trezoru služeb zotavení se zobrazuje podrobnosti o nebo atributů trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-109">The Recovery Services vault dashboard shows you the details or attributes of a Recovery Services vault.</span></span>

1. <span data-ttu-id="4e9a1-110">Přihlaste se k [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-110">Sign in to the [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="4e9a1-111">V nabídce centra klikněte na tlačítko **více služeb**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-111">On the Hub menu, click **More Services**.</span></span>

    ![Otevřete seznam krok trezory služeb zotavení 1](./media/backup-azure-manage-windows-server/open-rs-vault-list.png) <br/>

3. <span data-ttu-id="4e9a1-113">Chcete otevřít trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-113">You want to open a Recovery Services vault.</span></span> <span data-ttu-id="4e9a1-114">V dialogovém okně začněte psát **služeb zotavení**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-114">In the dialog box, start typing **Recovery Services**.</span></span> <span data-ttu-id="4e9a1-115">Během zadávání se seznam bude filtrovat podle zadávaného textu.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-115">As you begin typing, the list will filter based on your input.</span></span> <span data-ttu-id="4e9a1-116">Klikněte na tlačítko **trezory služeb zotavení** k zobrazení seznamu trezorů služeb zotavení v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-116">Click **Recovery Services vaults** to display the list of Recovery Services vaults in your subscription.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-manage-windows-server/browse-to-rs-vaults-2.png) <br/>

    <span data-ttu-id="4e9a1-118">Otevře se seznam trezorů služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-118">The list of Recovery Services vaults opens.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. <span data-ttu-id="4e9a1-120">Ze seznamu trezorů vyberte název trezoru služeb zotavení, který chcete otevřít.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-120">From the list of vaults, select the name of the Recovery Services vault you want to open.</span></span> <span data-ttu-id="4e9a1-121">Otevře se okno řídicího panelu trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-121">The Recovery Services vault dashboard blade opens.</span></span>

    ![panelu trezoru služeb zotavení](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    <span data-ttu-id="4e9a1-123">Teď, když máte otevřený trezor služeb zotavení, vyzkoušejte některý z monitorování nebo Správa úloh.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-123">Now that you have opened the Recovery Services vault, try any of the monitoring or management tasks.</span></span>

## <a name="monitor-backup-jobs-and-alerts"></a><span data-ttu-id="4e9a1-124">Úlohy zálohování monitorování a výstrahy</span><span class="sxs-lookup"><span data-stu-id="4e9a1-124">Monitor backup jobs and alerts</span></span>

<span data-ttu-id="4e9a1-125">Úlohy a výstrahy na řídicím panelu trezoru služeb zotavení, kde uvidíte můžete monitorovat:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-125">You monitor jobs and alerts from the Recovery Services vault dashboard, where you see:</span></span>

* <span data-ttu-id="4e9a1-126">Podrobnosti výstrahy zálohy</span><span class="sxs-lookup"><span data-stu-id="4e9a1-126">Backup alerts details</span></span>
* <span data-ttu-id="4e9a1-127">Soubory a složky, jakož i Azure virtuální počítače chráněné v cloudu</span><span class="sxs-lookup"><span data-stu-id="4e9a1-127">Files and folders, as well as Azure virtual machines protected in the cloud</span></span>
* <span data-ttu-id="4e9a1-128">Celkový úložný využívat v Azure</span><span class="sxs-lookup"><span data-stu-id="4e9a1-128">Total storage consumed in Azure</span></span>
* <span data-ttu-id="4e9a1-129">Stav úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="4e9a1-129">Backup job status</span></span>

![Úlohy zálohování řídicí panel](./media/backup-azure-manage-windows-server/dashboard-tiles.png)

<span data-ttu-id="4e9a1-131">Informace v každé z těchto dlaždic kliknutím otevřete přidružené okno, kde budete spravovat související úlohy.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-131">Clicking the information in each of these tiles will open the associated blade where you manage related tasks.</span></span>

<span data-ttu-id="4e9a1-132">Z horní části řídicího panelu:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-132">From the top of the Dashboard:</span></span>

* <span data-ttu-id="4e9a1-133">Nastavení poskytuje přístup k dispozici úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-133">Settings provides access available backup tasks.</span></span>
* <span data-ttu-id="4e9a1-134">Zálohování – pomáhá zálohujete nové soubory a složky (nebo virtuálních počítačích Azure) do trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-134">Backup - helps you back up new files and folders (or Azure VMs) to the Recovery Services vault.</span></span>
* <span data-ttu-id="4e9a1-135">Odstranění -: Pokud se už používá trezor služeb zotavení, můžete odstranit uvolnění prostoru úložiště.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-135">Delete - If a recovery services vault is no longer being used, you can delete it to free up storage space.</span></span> <span data-ttu-id="4e9a1-136">Odstranění je aktivní jenom po odstranění všech chráněných serverech z trezoru.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-136">Delete is only enabled after all protected servers have been deleted from the vault.</span></span>

![Úlohy zálohování řídicí panel](./media/backup-azure-manage-windows-server/dashboard-tasks.png)

## <a name="alerts-for-backups-using-azure-backup-agent"></a><span data-ttu-id="4e9a1-138">Výstrahy pro zálohování pomocí agenta Azure backup:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-138">Alerts for backups using Azure backup agent:</span></span>
| <span data-ttu-id="4e9a1-139">Úroveň výstrahy</span><span class="sxs-lookup"><span data-stu-id="4e9a1-139">Alert Level</span></span> | <span data-ttu-id="4e9a1-140">Zasílání upozornění</span><span class="sxs-lookup"><span data-stu-id="4e9a1-140">Alerts sent</span></span> |
| --- | --- |
| <span data-ttu-id="4e9a1-141">Kritické</span><span class="sxs-lookup"><span data-stu-id="4e9a1-141">Critical</span></span> |<span data-ttu-id="4e9a1-142">Selhání zálohování, obnovení selhání</span><span class="sxs-lookup"><span data-stu-id="4e9a1-142">Backup failure, recovery failure</span></span> |
| <span data-ttu-id="4e9a1-143">Upozornění</span><span class="sxs-lookup"><span data-stu-id="4e9a1-143">Warning</span></span> |<span data-ttu-id="4e9a1-144">Zálohování dokončeno s varováním (při méně než sto soubory nejsou zálohovány kvůli problémům s poškození a více než jeden milión soubory jsou zálohovány úspěšně)</span><span class="sxs-lookup"><span data-stu-id="4e9a1-144">Backup completed with warnings (when fewer than one hundred files are not backed up due to corruption issues, and more than one million files are successfully backed up)</span></span> |
| <span data-ttu-id="4e9a1-145">Informační</span><span class="sxs-lookup"><span data-stu-id="4e9a1-145">Informational</span></span> |<span data-ttu-id="4e9a1-146">Žádný</span><span class="sxs-lookup"><span data-stu-id="4e9a1-146">None</span></span> |

## <a name="manage-backup-alerts"></a><span data-ttu-id="4e9a1-147">Správa výstrah zálohování</span><span class="sxs-lookup"><span data-stu-id="4e9a1-147">Manage Backup alerts</span></span>
<span data-ttu-id="4e9a1-148">Klikněte na tlačítko **zálohování výstrahy** dlaždici otevřete **zálohování výstrahy** okno a spravovat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-148">Click the **Backup Alerts** tile to open the **Backup Alerts** blade and manage alerts.</span></span>

![Výstrahy zálohy](./media/backup-azure-manage-windows-server/manage-backup-alerts.png)

<span data-ttu-id="4e9a1-150">Na dlaždici zálohování výstrahy zobrazuje číslo:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-150">The Backup Alerts tile shows you the number of:</span></span>

* <span data-ttu-id="4e9a1-151">kritické výstrahy nepřeložené za posledních 24 hodin</span><span class="sxs-lookup"><span data-stu-id="4e9a1-151">critical alerts unresolved in last 24 hours</span></span>
* <span data-ttu-id="4e9a1-152">upozorňující výstrahy nepřeložené za posledních 24 hodin</span><span class="sxs-lookup"><span data-stu-id="4e9a1-152">warning alerts unresolved in last 24 hours</span></span>

<span data-ttu-id="4e9a1-153">Kliknutím na každý z těchto odkazů přejdete k **zálohování výstrahy** okno s filtrované zobrazení tyto výstrahy (kritický nebo upozornění).</span><span class="sxs-lookup"><span data-stu-id="4e9a1-153">Clicking on each of these links takes you to the **Backup Alerts** blade with a filtered view of these alerts (critical or warning).</span></span>

<span data-ttu-id="4e9a1-154">V okně Zálohování výstrahy můžete:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-154">From the Backup Alerts blade, you:</span></span>

* <span data-ttu-id="4e9a1-155">Vyberte příslušné informace, které chcete zahrnout do upozornění.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-155">Choose the appropriate information to include with your alerts.</span></span>

    ![Zvolit sloupce](./media/backup-azure-manage-windows-server/choose-alerts-colunms.png)
* <span data-ttu-id="4e9a1-157">Filtrování výstrah pro závažnost, stavu a počáteční nebo koncové časy.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-157">Filter alerts for severity, status and start/end times.</span></span>

    ![Filtrování výstrah](./media/backup-azure-manage-windows-server/filter-alerts.png)
* <span data-ttu-id="4e9a1-159">Konfigurace oznámení pro závažností, četnosti a příjemce, a také zapnout výstrahy nebo vypnout.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-159">Configure notifications for severity, frequency and recipients, as well as turn alerts on or off.</span></span>

    ![Filtrování výstrah](./media/backup-azure-manage-windows-server/configure-notifications.png)

<span data-ttu-id="4e9a1-161">Pokud **za výstrahy** je vybrán jako **oznámení** frekvence dojde k žádné seskupení nebo snížení e-mailů.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-161">If **Per Alert** is selected as the **Notify** frequency no grouping or reduction in emails occurs.</span></span> <span data-ttu-id="4e9a1-162">Každá výstraha výsledkem 1 oznámení.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-162">Every alert results in 1 notification.</span></span> <span data-ttu-id="4e9a1-163">Toto je výchozí nastavení a e-mailové řešení je také odesílat okamžitě.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-163">This is the default setting and the resolution email is also sent out immediately.</span></span>

<span data-ttu-id="4e9a1-164">Pokud **hodinové Digest** je vybrán jako **oznámení** nepřeložené frekvenci, jeden e-mail je odeslán uživateli zprávu, které nejsou nové výstrahy generované za poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-164">If **Hourly Digest** is selected as the **Notify** frequency one email is sent to the user telling them that there are unresolved new alerts generated in the last hour.</span></span> <span data-ttu-id="4e9a1-165">E-mailu s řešení odeslání na konci za hodinu.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-165">A resolution email is sent out at the end of the hour.</span></span>

<span data-ttu-id="4e9a1-166">Můžete odesílat upozornění pro následující úrovně závažnosti:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-166">Alerts can be sent for the following severity levels:</span></span>

* <span data-ttu-id="4e9a1-167">Kritické</span><span class="sxs-lookup"><span data-stu-id="4e9a1-167">critical</span></span>
* <span data-ttu-id="4e9a1-168">Upozornění</span><span class="sxs-lookup"><span data-stu-id="4e9a1-168">warning</span></span>
* <span data-ttu-id="4e9a1-169">Informace o</span><span class="sxs-lookup"><span data-stu-id="4e9a1-169">information</span></span>

<span data-ttu-id="4e9a1-170">Deaktivovat výstrahy s **deaktivovat** tlačítka v okně podrobností úlohy.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-170">You inactivate the alert with the **inactivate** button in the job details blade.</span></span> <span data-ttu-id="4e9a1-171">Po kliknutí na tlačítko deaktivovat, můžete zadat poznámky k řešení.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-171">When you click inactivate, you can provide resolution notes.</span></span>

<span data-ttu-id="4e9a1-172">Vyberte sloupce, které se mají zobrazit jako součást výstraha s **zvolit sloupce** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-172">You choose the columns you want to appear as part of the alert with the **Choose columns** button.</span></span>

> [!NOTE]
> <span data-ttu-id="4e9a1-173">Z **nastavení** okně Spravovat výstrahy zálohy výběrem **monitorování a sestavy > Výstrahy a události > výstrahy zálohy** a pak levým na **filtru** nebo  **Konfigurace oznámení**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-173">From the **Settings** blade, you manage backup alerts by selecting **Monitoring and Reports > Alerts and Events > Backup Alerts** and then clicking **Filter** or **Configure Notifications**.</span></span>
>
>

## <a name="manage-backup-items"></a><span data-ttu-id="4e9a1-174">Správa zálohování položek</span><span class="sxs-lookup"><span data-stu-id="4e9a1-174">Manage Backup items</span></span>
<span data-ttu-id="4e9a1-175">Správa místního zálohování je teď dostupná v portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-175">Managing on-premises backups is now available in the management portal.</span></span> <span data-ttu-id="4e9a1-176">V části zálohování řídicího panelu **položky zálohování** dlaždice se zobrazuje číslo položky zálohování chráněných do trezoru.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-176">In the Backup section of the dashboard, the **Backup Items** tile shows the number of backup items protected to the vault.</span></span>

<span data-ttu-id="4e9a1-177">Klikněte na tlačítko **složek souborů** v zálohování položek dlaždici.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-177">Click **File-Folders** in the Backup Items tile.</span></span>

![Dlaždice položky zálohování](./media/backup-azure-manage-windows-server/backup-items-tile.png)

<span data-ttu-id="4e9a1-179">Otevře se okno položky zálohování s filtrem nastavena na soubor a složka, kde uvidíte každé konkrétní zálohy položka v seznamu.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-179">The Backup Items blade opens with the filter set to File-Folder where you see each specific backup item listed.</span></span>

![Zálohování položek](./media/backup-azure-manage-windows-server/backup-item-list.png)

<span data-ttu-id="4e9a1-181">Pokud vyberete konkrétní zálohování položku ze seznamu, zobrazí se základní podrobnosti o této položce.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-181">If you select a specific backup item from the list, you see the essential details for that item.</span></span>

> [!NOTE]
> <span data-ttu-id="4e9a1-182">Z **nastavení** okně Spravovat soubory a složky tak, že vyberete **chráněné položky > Zálohování položek** a potom vyberete **složek souborů** z rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-182">From the **Settings** blade, you manage files and folders by selecting **Protected Items > Backup Items** and then selecting **File-Folders** from the drop down menu.</span></span>
>
>

![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/backup-files-and-folders.png)

## <a name="manage-backup-jobs"></a><span data-ttu-id="4e9a1-184">Při správě úloh zálohování</span><span class="sxs-lookup"><span data-stu-id="4e9a1-184">Manage Backup jobs</span></span>
<span data-ttu-id="4e9a1-185">Úlohy zálohování pro místní (Pokud je na místním serveru zálohování do Azure) a jsou viditelné v řídicím panelu zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-185">Backup jobs for both on-premises (when the on-premises server is backing up to Azure) and Azure backups are visible in the dashboard.</span></span>

<span data-ttu-id="4e9a1-186">V části zálohování řídicí panel dlaždice úlohy zálohování znázorňuje počet úloh:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-186">In the Backup section of the dashboard, the Backup job tile shows the number of jobs:</span></span>

* <span data-ttu-id="4e9a1-187">v průběhu</span><span class="sxs-lookup"><span data-stu-id="4e9a1-187">in progress</span></span>
* <span data-ttu-id="4e9a1-188">selhání v posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-188">failed in the last 24 hours.</span></span>

<span data-ttu-id="4e9a1-189">Chcete-li spravovat vaše úlohy zálohování, klikněte na tlačítko **úlohy zálohování** dlaždici, což otevře okno úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-189">To manage your backup jobs, click the **Backup Jobs** tile, which opens the Backup Jobs blade.</span></span>

![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/backup-jobs.png)

<span data-ttu-id="4e9a1-191">Upravte informace k dispozici v okně úlohy zálohování se **zvolit sloupce** tlačítka v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-191">You modify the information available in the Backup Jobs blade with the **Choose columns** button at the top of the page.</span></span>

<span data-ttu-id="4e9a1-192">Použití **filtru** tlačítko můžete vybrat mezi soubory a složky a záloha virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-192">Use the **Filter** button to select between Files and folders and Azure virtual machine backup.</span></span>

<span data-ttu-id="4e9a1-193">Pokud nevidíte zálohovaný soubory a složky, klikněte na tlačítko **filtru** tlačítka v horní části stránky a vyberte **soubory a složky** nabídce typu položky.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-193">If you don't see your backed up files and folders, click **Filter** button at the top of the page and select **Files and folders** from the Item Type menu.</span></span>

> [!NOTE]
> <span data-ttu-id="4e9a1-194">Z **nastavení** okně Spravovat úlohy zálohování tak, že vyberete **monitorování a sestavy > úlohy > úlohy zálohování** a potom vyberete **složek souborů** z rozevíracího seznamu nabídky.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-194">From the **Settings** blade, you manage backup jobs by selecting **Monitoring and Reports > Jobs > Backup Jobs** and then selecting **File-Folders** from the drop down menu.</span></span>
>
>

## <a name="monitor-backup-usage"></a><span data-ttu-id="4e9a1-195">Sledování využití zálohování</span><span class="sxs-lookup"><span data-stu-id="4e9a1-195">Monitor Backup usage</span></span>
<span data-ttu-id="4e9a1-196">V části zálohování řídicího panelu zobrazí dlaždice využití zálohování úložiště využívat v Azure.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-196">In the Backup section of the dashboard, the Backup Usage tile shows the storage consumed in Azure.</span></span> <span data-ttu-id="4e9a1-197">Využití úložiště je k dispozici pro:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-197">Storage usage is provided for:</span></span>

* <span data-ttu-id="4e9a1-198">Využití úložiště LRS cloudu přidruženého k úložišti</span><span class="sxs-lookup"><span data-stu-id="4e9a1-198">Cloud LRS storage usage associated with the vault</span></span>
* <span data-ttu-id="4e9a1-199">Využití úložiště GRS cloudu přidruženého k úložišti</span><span class="sxs-lookup"><span data-stu-id="4e9a1-199">Cloud GRS storage usage associated with the vault</span></span>

## <a name="manage-your-production-servers"></a><span data-ttu-id="4e9a1-200">Spravovat vaše produkční servery</span><span class="sxs-lookup"><span data-stu-id="4e9a1-200">Manage your production servers</span></span>
<span data-ttu-id="4e9a1-201">Chcete-li spravovat provozních serverů, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-201">To manage your production servers, click **Settings**.</span></span>

<span data-ttu-id="4e9a1-202">V části Správa klikněte na **infrastruktura zálohování > produkční servery**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-202">Under Manage click **Backup infrastructure > Production Servers**.</span></span>

<span data-ttu-id="4e9a1-203">Zobrazí okno provozních serverů všechny dostupné provozních serverů.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-203">The Production Servers blade lists of all your available production servers.</span></span> <span data-ttu-id="4e9a1-204">Klikněte na server v seznamu a otevřete tak podrobnosti serveru.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-204">Click on a server in the list to open the server details.</span></span>

![Chráněné položky](./media/backup-azure-manage-windows-server/production-server-list.png)


## <a name="open-the-azure-backup-agent"></a><span data-ttu-id="4e9a1-206">Otevřete agenta Azure Backup</span><span class="sxs-lookup"><span data-stu-id="4e9a1-206">Open the Azure Backup agent</span></span>
<span data-ttu-id="4e9a1-207">Otevřete **Microsoft Azure Backup agent** (můžete najít tak, že váš počítač pro *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="4e9a1-207">Open the **Microsoft Azure Backup agent** (you find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/snap-in-search.png)

<span data-ttu-id="4e9a1-209">Z **akce** k dispozici na pravé straně konzole agenta zálohování, můžete provádět následující úlohy správy:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-209">From the **Actions** available at the right of the backup agent console you perform the following management tasks:</span></span>

* <span data-ttu-id="4e9a1-210">Registrace serveru</span><span class="sxs-lookup"><span data-stu-id="4e9a1-210">Register Server</span></span>
* <span data-ttu-id="4e9a1-211">Plán zálohování</span><span class="sxs-lookup"><span data-stu-id="4e9a1-211">Schedule Backup</span></span>
* <span data-ttu-id="4e9a1-212">Zálohovat nyní</span><span class="sxs-lookup"><span data-stu-id="4e9a1-212">Back Up now</span></span>
* <span data-ttu-id="4e9a1-213">Změnit vlastnosti</span><span class="sxs-lookup"><span data-stu-id="4e9a1-213">Change Properties</span></span>

![Microsoft Azure Backup agent konzoly akce](./media/backup-azure-manage-windows-server/console-actions.png)

> [!NOTE]
> <span data-ttu-id="4e9a1-215">K **obnovit Data**, najdete v části [obnovit soubory do systému Windows server nebo klientský počítač systému Windows](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="4e9a1-215">To **Recover Data**, see [Restore files to a Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

## <a name="modify-the-backup-schedule"></a><span data-ttu-id="4e9a1-216">Upravte plán zálohování</span><span class="sxs-lookup"><span data-stu-id="4e9a1-216">Modify the backup schedule</span></span>
1. <span data-ttu-id="4e9a1-217">V agentovi nástroje Microsoft Azure Backup klikněte na tlačítko **plánem zálohování**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-217">In the Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/schedule-backup.png)
2. <span data-ttu-id="4e9a1-219">V **Průvodce plánem zálohování** ponechte **provádět změny položky zálohování nebo časy** možnost a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-219">In the **Schedule Backup Wizard** leave the **Make changes to backup items or times** option selected and click **Next**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="4e9a1-221">Pokud chcete přidat nebo změnit položky, na **výběr položek k zálohování** obrazovce klikněte na tlačítko **přidat položky**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-221">If you want to add or change items, on the **Select Items to Backup** screen click **Add Items**.</span></span>

    <span data-ttu-id="4e9a1-222">Můžete také nastavit **nastavení vyloučení** z této stránky v průvodci.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-222">You can also set **Exclusion Settings** from this page in the wizard.</span></span> <span data-ttu-id="4e9a1-223">Pokud chcete vyloučit soubory nebo typy souborů, přečtěte si postup pro přidání [nastavení vyloučení](#manage-exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="4e9a1-223">If you want to exclude files or file types read the procedure for adding [exclusion settings](#manage-exclusion-settings).</span></span>
4. <span data-ttu-id="4e9a1-224">Vyberte soubory a složky, které chcete zálohovat a klikněte na tlačítko **nevadí**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-224">Select the files and folders you want to back up and click **Okay**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/add-items-modify.png)
5. <span data-ttu-id="4e9a1-226">Zadejte **plán zálohování** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-226">Specify the **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="4e9a1-227">Můžete naplánovat denní (probíhající maximálně 3krát denně) nebo týdenní zálohování.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-227">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Položky k zálohování z Windows Serveru](./media/backup-azure-manage-windows-server/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="4e9a1-229">Zadat plán zálohování je podrobně popsány v tomto [článku](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="4e9a1-229">Specifying the backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >

6. <span data-ttu-id="4e9a1-230">Vyberte **zásady uchovávání informací** pro záložní kopii a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-230">Select the **Retention Policy** for the backup copy and click **Next**.</span></span>

    ![Položky k zálohování z Windows Serveru](./media/backup-azure-manage-windows-server/select-retention-policy-modify.png)
7. <span data-ttu-id="4e9a1-232">Na **potvrzení** obrazovky Zkontrolujte zadané informace a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-232">On the **Confirmation** screen review the information and click **Finish**.</span></span>
8. <span data-ttu-id="4e9a1-233">Až průvodce dokončí vytváření **plán zálohování**, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-233">Once the wizard finishes creating the **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="4e9a1-234">Po úpravě ochrany, můžete ověřit, že zálohy spouštějí správně přechodem na **úlohy** kartě a potvrzující, že změny se projeví v úloh zálohování.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-234">After modifying protection, you can confirm that backups are triggering correctly by going to the **Jobs** tab and confirming that changes are reflected in the backup jobs.</span></span>

## <a name="enable-network-throttling"></a><span data-ttu-id="4e9a1-235">Povolit omezení šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="4e9a1-235">Enable Network Throttling</span></span>

<span data-ttu-id="4e9a1-236">Agent Azure Backup poskytuje omezování karta, která umožňuje řídit použití šířky pásma sítě při přenosu dat.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-236">The Azure Backup agent provides a Throttling tab which allows you to control how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="4e9a1-237">Tento ovládací prvek může být užitečné, pokud potřebujete zálohovat data v pracovní době, ale nechcete, aby proces zálohování narušoval ostatní internetový provoz.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-237">This control can be helpful if you need to back up data during work hours but do not want the backup process to interfere with other internet traffic.</span></span> <span data-ttu-id="4e9a1-238">Omezování dat přenos platí pro zálohování a obnovení aktivity.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-238">Throttling of data transfer applies to back up and restore activities.</span></span>  

<span data-ttu-id="4e9a1-239">Povolit omezení:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-239">To enable throttling:</span></span>

1. <span data-ttu-id="4e9a1-240">V **agenta zálohování**, klikněte na tlačítko **změnit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-240">In the **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="4e9a1-241">Na ** omezení karty, vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-241">On the **Throttling tab, select **Enable internet bandwidth usage throttling for backup operations**.</span></span>

    ![Omezení šířky pásma sítě](./media/backup-azure-manage-windows-server/throttling-dialog.png)

    <span data-ttu-id="4e9a1-243">Jakmile jste povolili omezování, zadejte povolenou šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-243">Once you have enabled throttling, specify the allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="4e9a1-244">Hodnoty šířky pásma začít až 512 kilobitů za sekundu (Kbps) a můžete přejít až 1023 MB za sekundu (Mbps).</span><span class="sxs-lookup"><span data-stu-id="4e9a1-244">The bandwidth values begin at 512 kilobytes per second (Kbps) and can go up to 1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="4e9a1-245">Můžete také určit spuštění a dokončení pro **pracovní hodiny**, a které dny v týdnu, jsou považovány za pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-245">You can also designate the start and finish for **Work hours**, and which days of the week are considered Work days.</span></span> <span data-ttu-id="4e9a1-246">Mimo pracovní dobu v určené době se považuje za mimo pracovní dobu.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-246">The time outside of the designated Work hours is considered to be non-work hours.</span></span>
3. <span data-ttu-id="4e9a1-247">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-247">Click **OK**.</span></span>

## <a name="manage-exclusion-settings"></a><span data-ttu-id="4e9a1-248">Spravovat nastavení vyloučení</span><span class="sxs-lookup"><span data-stu-id="4e9a1-248">Manage exclusion settings</span></span>
1. <span data-ttu-id="4e9a1-249">Otevřete **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="4e9a1-249">Open the **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/snap-in-search.png)
2. <span data-ttu-id="4e9a1-251">V agentovi nástroje Microsoft Azure Backup klikněte na tlačítko **plánem zálohování**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-251">In the Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/schedule-backup.png)
3. <span data-ttu-id="4e9a1-253">V ponechejte Průvodce plánem zálohování **provádět změny položky zálohování nebo časy** možnost a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-253">In the Schedule Backup Wizard leave the **Make changes to backup items or times** option selected and click **Next**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="4e9a1-255">Klikněte na tlačítko **nastavení vyloučení**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-255">Click **Exclusions Settings**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclusion-settings.png)
5. <span data-ttu-id="4e9a1-257">Klikněte na tlačítko **Přidat vyloučení**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-257">Click **Add Exclusion**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/add-exclusion.png)
6. <span data-ttu-id="4e9a1-259">Vyberte umístění a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-259">Select the location and then, click **OK**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclusion-location.png)
7. <span data-ttu-id="4e9a1-261">Přidat příponu souboru v **typ souboru** pole.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-261">Add the file extension in the **File Type** field.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclude-file-type.png)

    <span data-ttu-id="4e9a1-263">Přidání rozšíření MP3</span><span class="sxs-lookup"><span data-stu-id="4e9a1-263">Adding an .mp3 extension</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclude-mp3.png)

    <span data-ttu-id="4e9a1-265">Chcete-li přidat jiné rozšíření, klikněte na tlačítko **Přidat vyloučení** a zadejte jinou příponu typu souboru (Přidání rozšíření .jpeg).</span><span class="sxs-lookup"><span data-stu-id="4e9a1-265">To add another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclude-jpg.png)
8. <span data-ttu-id="4e9a1-267">Pokud jste přidali všechna rozšíření, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-267">When you've added all the extensions, click **OK**.</span></span>
9. <span data-ttu-id="4e9a1-268">Pomocí Průvodce plánem zálohování nadále pokračovat kliknutím **Další** dokud **potvrzovací stránku**, pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-268">Continue through the Schedule Backup Wizard by clicking **Next** until the **Confirmation page**, then click **Finish**.</span></span>

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/finish-exclusions.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="4e9a1-270">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="4e9a1-270">Frequently asked questions</span></span>
<span data-ttu-id="4e9a1-271">**OTÁZKA Č. 1. Úloha zálohování stav zobrazuje jako dokončené v aplikace Azure backup agent, proč není ho získat projeví okamžitě v portálu?**</span><span class="sxs-lookup"><span data-stu-id="4e9a1-271">**Q1. The backup job status shows as completed in the Azure backup agent, why doesn't it get reflected immediately in portal?**</span></span>

<span data-ttu-id="4e9a1-272">Odpověď č. 1:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-272">A1.</span></span> <span data-ttu-id="4e9a1-273">Existuje na maximální zpoždění 15 minut mezi stav úlohy zálohování se v Azure backup agent a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-273">There is at maximum delay of 15 mins between the backup job status reflected in the Azure backup agent and the Azure portal.</span></span>

<span data-ttu-id="4e9a1-274">**Q.2 při selhání úlohy zálohování, jak dlouho trvá vygeneroval výstrahu?**</span><span class="sxs-lookup"><span data-stu-id="4e9a1-274">**Q.2 When a backup job fails, how long does it take to raise an alert?**</span></span>

<span data-ttu-id="4e9a1-275">A.2 výstraha se vyvolá v rámci 20 minut selhání zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-275">A.2 An alert is raised within 20 mins of the Azure backup failure.</span></span>

<span data-ttu-id="4e9a1-276">**3. ČTVRTLETÍ. Je případ, kdy e-mailu neodešle, pokud jsou nakonfigurované oznámení?**</span><span class="sxs-lookup"><span data-stu-id="4e9a1-276">**Q3. Is there a case where an email won’t be sent if notifications are configured?**</span></span>

<span data-ttu-id="4e9a1-277">Odpověď 3.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-277">A3.</span></span> <span data-ttu-id="4e9a1-278">Níže jsou oznámení nebude odeslána aby výstrahy nepůsobily rušivě. v případech:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-278">Below are the cases when the notification will not be sent in order to reduce the alert noise:</span></span>

* <span data-ttu-id="4e9a1-279">Pokud oznámení jsou nakonfigurovány každou hodinu a výstraha je vyvolána a vyřešit v rámci hodiny</span><span class="sxs-lookup"><span data-stu-id="4e9a1-279">If notifications are configured hourly and an alert is raised and resolved within the hour</span></span>
* <span data-ttu-id="4e9a1-280">Úloha je zrušena.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-280">Job is canceled.</span></span>
* <span data-ttu-id="4e9a1-281">Druhý úloha zálohování se nezdařila, protože probíhá úloha původní zálohování.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-281">Second backup job failed because original backup job is in progress.</span></span>

## <a name="troubleshooting-monitoring-issues"></a><span data-ttu-id="4e9a1-282">Monitorování řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4e9a1-282">Troubleshooting Monitoring Issues</span></span>
<span data-ttu-id="4e9a1-283">**Problém:** úlohy a výstrahy z agenta Azure Backup se nezobrazí na portálu.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-283">**Issue:** Jobs and/or alerts from the Azure Backup agent do not appear in the portal.</span></span>

<span data-ttu-id="4e9a1-284">**Řešení potíží:** procesu ```OBRecoveryServicesManagementAgent```, odešle data úlohy a výstrahy do služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-284">**Troubleshooting steps:** The process, ```OBRecoveryServicesManagementAgent```, sends the job and alert data to the Azure Backup service.</span></span> <span data-ttu-id="4e9a1-285">Příležitostně tento proces může zablokovat nebo vypnutí.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-285">Occasionally this process can become stuck or shutdown.</span></span>

1. <span data-ttu-id="4e9a1-286">Chcete-li ověřit, proces neběží, otevřete **Správce úloh** a zkontrolujte, zda ```OBRecoveryServicesManagementAgent``` proces běží.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-286">To verify the process is not running, open **Task Manager** and check if the ```OBRecoveryServicesManagementAgent``` process is running.</span></span>
2. <span data-ttu-id="4e9a1-287">Za předpokladu, že proces neběží, otevřete **ovládací panely** a procházet seznam služeb.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-287">Assuming that the process is not running, open **Control Panel** and browse the list of services.</span></span> <span data-ttu-id="4e9a1-288">Spustit nebo restartovat **správy agenta služeb zotavení Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="4e9a1-288">Start or restart **Microsoft Azure Recovery Services Management Agent**.</span></span>

    <span data-ttu-id="4e9a1-289">Další informace vyhledejte protokoly na:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-289">For further information, browse the logs at:</span></span><br/><span data-ttu-id="4e9a1-290">
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Například:</span><span class="sxs-lookup"><span data-stu-id="4e9a1-290">
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*` For example:</span></span><br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a><span data-ttu-id="4e9a1-291">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e9a1-291">Next steps</span></span>
* [<span data-ttu-id="4e9a1-292">Obnovení systému Windows Server nebo klienta Windows z Azure</span><span class="sxs-lookup"><span data-stu-id="4e9a1-292">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="4e9a1-293">Další informace o zálohování Azure, najdete v části [Přehled zálohování Azure](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="4e9a1-293">To learn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="4e9a1-294">Přejděte [fórum Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="4e9a1-294">Visit the [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
