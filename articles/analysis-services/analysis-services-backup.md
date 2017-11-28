---
title: "Azure zálohování databáze služby Analysis Services a obnovení | Microsoft Docs"
description: "Popisuje, jak zálohovat a obnovit databázi služby Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: bffa481a498b130ef1f2388a5ba856da5d164ee0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="backup-and-restore"></a><span data-ttu-id="574ff-103">Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="574ff-103">Backup and restore</span></span>

<span data-ttu-id="574ff-104">Zálohování databází tabulkový model v Azure Analysis Services je podobný jako místní služba Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="574ff-104">Backing up tabular model databases in Azure Analysis Services is much the same as for on-premises Analysis Services.</span></span> <span data-ttu-id="574ff-105">Základní rozdíl je, kam můžete ukládat soubory zálohy.</span><span class="sxs-lookup"><span data-stu-id="574ff-105">The primary difference is where you store your backup files.</span></span> <span data-ttu-id="574ff-106">Záložní soubory musí být uložena do kontejneru v [účtu úložiště Azure](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="574ff-106">Backup files must be saved to a container in an [Azure storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="574ff-107">Můžete použít účet úložiště a kontejneru, který už máte, nebo je lze vytvořit při konfiguraci nastavení úložiště pro svůj server.</span><span class="sxs-lookup"><span data-stu-id="574ff-107">You can use a storage account and container you already have, or they can be created when configuring storage settings for your server.</span></span>

> [!NOTE]
> <span data-ttu-id="574ff-108">Vytvoření účtu úložiště, může mít za následek začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="574ff-108">Creating a storage account can result in a new billable service.</span></span> <span data-ttu-id="574ff-109">Další informace najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/blobs/).</span><span class="sxs-lookup"><span data-stu-id="574ff-109">To learn more, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/blobs/).</span></span>
> 
> 

<span data-ttu-id="574ff-110">Zálohy se ukládají s příponou abf.</span><span class="sxs-lookup"><span data-stu-id="574ff-110">Backups are saved with an abf extension.</span></span> <span data-ttu-id="574ff-111">Pro tabulkové modely v paměti jsou uloženy data modelu a metadata.</span><span class="sxs-lookup"><span data-stu-id="574ff-111">For in-memory tabular models, both model data and metadata are stored.</span></span> <span data-ttu-id="574ff-112">Tabulkové modely DirectQuery ukládají pouze metadata modelu.</span><span class="sxs-lookup"><span data-stu-id="574ff-112">For DirectQuery tabular models, only model metadata is stored.</span></span> <span data-ttu-id="574ff-113">Zálohy lze komprimovat a šifrování, v závislosti na možnostech, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="574ff-113">Backups can be compressed and encrypted, depending on the options you choose.</span></span> 



## <a name="configure-storage-settings"></a><span data-ttu-id="574ff-114">Konfigurovat nastavení úložiště</span><span class="sxs-lookup"><span data-stu-id="574ff-114">Configure storage settings</span></span>
<span data-ttu-id="574ff-115">Před zahájením zálohování, musíte nakonfigurovat nastavení úložiště pro svůj server.</span><span class="sxs-lookup"><span data-stu-id="574ff-115">Before backing up, you need to configure storage settings for your server.</span></span>


### <a name="to-configure-storage-settings"></a><span data-ttu-id="574ff-116">Konfigurovat nastavení úložiště</span><span class="sxs-lookup"><span data-stu-id="574ff-116">To configure storage settings</span></span>
1.  <span data-ttu-id="574ff-117">Na portálu Azure > **nastavení**, klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="574ff-117">In Azure portal > **Settings**, click **Backup**.</span></span>

    ![Zálohy v nastavení](./media/analysis-services-backup/aas-backup-backups.png)

2.  <span data-ttu-id="574ff-119">Klikněte na tlačítko **povoleno**, pak klikněte na tlačítko **nastavení úložiště**.</span><span class="sxs-lookup"><span data-stu-id="574ff-119">Click **Enabled**, then click **Storage Settings**.</span></span>

    ![Povolení](./media/analysis-services-backup/aas-backup-enable.png)

3. <span data-ttu-id="574ff-121">Vyberte účet úložiště nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="574ff-121">Select your storage account or create a new one.</span></span>

4. <span data-ttu-id="574ff-122">Vyberte kontejner nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="574ff-122">Select a container or create a new one.</span></span>

    ![Vyberte kontejner](./media/analysis-services-backup/aas-backup-container.png)

5. <span data-ttu-id="574ff-124">Uložte nastavení zálohování.</span><span class="sxs-lookup"><span data-stu-id="574ff-124">Save your backup settings.</span></span>

    ![Uložit nastavení zálohování](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a><span data-ttu-id="574ff-126">Zálohování</span><span class="sxs-lookup"><span data-stu-id="574ff-126">Backup</span></span>

### <a name="to-backup-by-using-ssms"></a><span data-ttu-id="574ff-127">K zálohování pomocí aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="574ff-127">To backup by using SSMS</span></span>

1. <span data-ttu-id="574ff-128">V aplikaci SSMS, klikněte pravým tlačítkem na databázi > **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="574ff-128">In SSMS, right-click a database > **Back Up**.</span></span>

2. <span data-ttu-id="574ff-129">V **příkaz Backup Database** > **záložní soubor**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="574ff-129">In **Backup Database** > **Backup file**, click **Browse**.</span></span>

3. <span data-ttu-id="574ff-130">V **uložit soubor jako** dialogové okno, ověřte cestu ke složce a potom zadejte název souboru zálohy.</span><span class="sxs-lookup"><span data-stu-id="574ff-130">In the **Save file as** dialog, verify the folder path, and then type a name for the backup file.</span></span> 

4. <span data-ttu-id="574ff-131">V **příkaz Backup Database** dialogové okno, vyberte možnosti.</span><span class="sxs-lookup"><span data-stu-id="574ff-131">In the **Backup Database** dialog, select options.</span></span>

    <span data-ttu-id="574ff-132">**Povolit souboru přepsat** – vyberte tuto možnost, chcete-li přepsat záložní soubory se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="574ff-132">**Allow file overwrite** - Select this option to overwrite backup files of the same name.</span></span> <span data-ttu-id="574ff-133">Pokud není vybraná tato možnost, soubor, který chcete uložit nemůže mít stejný název jako soubor, který již existuje ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="574ff-133">If this option is not selected, the file you are saving cannot have the same name as a file that already exists in the same location.</span></span>

    <span data-ttu-id="574ff-134">**Použít komprese** – vyberte tuto možnost, chcete-li komprimovat soubor zálohy.</span><span class="sxs-lookup"><span data-stu-id="574ff-134">**Apply compression** - Select this option to compress the backup file.</span></span> <span data-ttu-id="574ff-135">Komprimované záložní soubory ušetřit místo na disku, ale vyžadují mírně zvýší využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="574ff-135">Compressed backup files save disk space, but require slightly higher CPU utilization.</span></span> 

    <span data-ttu-id="574ff-136">**Šifrování záložní soubor** – vyberte tuto možnost, šifrování záložní soubor.</span><span class="sxs-lookup"><span data-stu-id="574ff-136">**Encrypt backup file** - Select this option to encrypt the backup file.</span></span> <span data-ttu-id="574ff-137">Tato možnost vyžaduje uživatel zadal heslo pro záložní soubor zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="574ff-137">This option requires a user-supplied password to secure the backup file.</span></span> <span data-ttu-id="574ff-138">Heslo zabraňuje čtení zálohovaných dat jiným způsobem než operaci obnovení.</span><span class="sxs-lookup"><span data-stu-id="574ff-138">The password prevents reading of the backup data any other means than a restore operation.</span></span> <span data-ttu-id="574ff-139">Pokud zvolíte možnost šifrování záloh, uložte heslo na bezpečné místo.</span><span class="sxs-lookup"><span data-stu-id="574ff-139">If you choose to encrypt backups, store the password in a safe location.</span></span>

5. <span data-ttu-id="574ff-140">Klikněte na tlačítko **OK** vytvořit a uložit záložní soubor.</span><span class="sxs-lookup"><span data-stu-id="574ff-140">Click **OK** to create and save the backup file.</span></span>


### <a name="powershell"></a><span data-ttu-id="574ff-141">PowerShell</span><span class="sxs-lookup"><span data-stu-id="574ff-141">PowerShell</span></span>
<span data-ttu-id="574ff-142">Použití [zálohování ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) rutiny.</span><span class="sxs-lookup"><span data-stu-id="574ff-142">Use [Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet.</span></span>

## <a name="restore"></a><span data-ttu-id="574ff-143">Obnovení</span><span class="sxs-lookup"><span data-stu-id="574ff-143">Restore</span></span>
<span data-ttu-id="574ff-144">Při obnovování, záložní soubor musí být v účtu úložiště, kterou jste nakonfigurovali pro váš server.</span><span class="sxs-lookup"><span data-stu-id="574ff-144">When restoring, your backup file must be in the storage account you've configured for your server.</span></span> <span data-ttu-id="574ff-145">Pokud potřebujete přesunout soubor zálohy z místního umístění účtu úložiště, použijte [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) nebo [AzCopy](../storage/common/storage-use-azcopy.md) nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="574ff-145">If you need to move a backup file from an on-premises location to your storage account, use [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) or the [AzCopy](../storage/common/storage-use-azcopy.md) command-line utility.</span></span> 



> [!NOTE]
> <span data-ttu-id="574ff-146">Pokud se obnovení ze serveru místní, musíte odebrat všechny domény uživatele z role modelu a přidat zpět role jako uživatelů Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="574ff-146">If you're restoring from an on-premises server, you must remove all the domain users from the model's roles and add them back to the roles as Azure Active Directory users.</span></span>
> 
> 

### <a name="to-restore-by-using-ssms"></a><span data-ttu-id="574ff-147">Chcete-li obnovit pomocí aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="574ff-147">To restore by using SSMS</span></span>

1. <span data-ttu-id="574ff-148">V aplikaci SSMS, klikněte pravým tlačítkem na databázi > **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="574ff-148">In SSMS, right-click a database > **Restore**.</span></span>

2. <span data-ttu-id="574ff-149">V **příkaz Backup Database** dialogové okno, v **záložní soubor**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="574ff-149">In the **Backup Database** dialog, in **Backup file**, click **Browse**.</span></span>

3. <span data-ttu-id="574ff-150">V **najít soubory databáze** dialogovém okně, vyberte soubor, který chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="574ff-150">In the **Locate Database Files** dialog, select the file you want to restore.</span></span>

4. <span data-ttu-id="574ff-151">V **obnovte databázi**, vyberte databázi.</span><span class="sxs-lookup"><span data-stu-id="574ff-151">In **Restore database**, select the database.</span></span>

5. <span data-ttu-id="574ff-152">Zadejte možnosti.</span><span class="sxs-lookup"><span data-stu-id="574ff-152">Specify options.</span></span> <span data-ttu-id="574ff-153">Možnosti zabezpečení se musí shodovat možnosti zálohování, které jste použili při zálohování.</span><span class="sxs-lookup"><span data-stu-id="574ff-153">Security options must match the backup options you used when backing up.</span></span>


### <a name="powershell"></a><span data-ttu-id="574ff-154">PowerShell</span><span class="sxs-lookup"><span data-stu-id="574ff-154">PowerShell</span></span>

<span data-ttu-id="574ff-155">Použití [obnovení ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) rutiny.</span><span class="sxs-lookup"><span data-stu-id="574ff-155">Use [Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet.</span></span>


## <a name="related-information"></a><span data-ttu-id="574ff-156">Související informace</span><span class="sxs-lookup"><span data-stu-id="574ff-156">Related information</span></span>

[<span data-ttu-id="574ff-157">Účty úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="574ff-157">Azure storage accounts</span></span>](../storage/common/storage-create-storage-account.md)  
<span data-ttu-id="574ff-158">[Vysoká dostupnost](analysis-services-bcdr.md)   </span><span class="sxs-lookup"><span data-stu-id="574ff-158">[High availability](analysis-services-bcdr.md)   </span></span>  
[<span data-ttu-id="574ff-159">Správa služby Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="574ff-159">Manage Azure Analysis Services</span></span>](analysis-services-manage.md)
