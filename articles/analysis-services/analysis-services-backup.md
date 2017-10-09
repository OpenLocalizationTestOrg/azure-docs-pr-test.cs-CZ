---
title: "databáze služby Analysis Services aaaAzure zálohování a obnovení | Microsoft Docs"
description: "Popisuje, jak toobackup a obnovení Azure Analysis Services databáze."
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
ms.openlocfilehash: cf0a782d237a95fdfa5ef628f998bd053aac0d9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-restore"></a><span data-ttu-id="107a6-103">Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="107a6-103">Backup and restore</span></span>

<span data-ttu-id="107a6-104">Zálohování databází tabulkový model v Azure Analysis Services je mnohem hello stejné jako u místní Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="107a6-104">Backing up tabular model databases in Azure Analysis Services is much hello same as for on-premises Analysis Services.</span></span> <span data-ttu-id="107a6-105">Hello základní rozdíl je, kam můžete ukládat soubory zálohy.</span><span class="sxs-lookup"><span data-stu-id="107a6-105">hello primary difference is where you store your backup files.</span></span> <span data-ttu-id="107a6-106">Záložní soubory musí být uložena tooa kontejneru v [účtu úložiště Azure](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="107a6-106">Backup files must be saved tooa container in an [Azure storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="107a6-107">Můžete použít účet úložiště a kontejneru, který už máte, nebo je lze vytvořit při konfiguraci nastavení úložiště pro svůj server.</span><span class="sxs-lookup"><span data-stu-id="107a6-107">You can use a storage account and container you already have, or they can be created when configuring storage settings for your server.</span></span>

> [!NOTE]
> <span data-ttu-id="107a6-108">Vytvoření účtu úložiště, může mít za následek začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="107a6-108">Creating a storage account can result in a new billable service.</span></span> <span data-ttu-id="107a6-109">Další, najdete v části toolearn [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/blobs/).</span><span class="sxs-lookup"><span data-stu-id="107a6-109">toolearn more, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/blobs/).</span></span>
> 
> 

<span data-ttu-id="107a6-110">Zálohy se ukládají s příponou abf.</span><span class="sxs-lookup"><span data-stu-id="107a6-110">Backups are saved with an abf extension.</span></span> <span data-ttu-id="107a6-111">Pro tabulkové modely v paměti jsou uloženy data modelu a metadata.</span><span class="sxs-lookup"><span data-stu-id="107a6-111">For in-memory tabular models, both model data and metadata are stored.</span></span> <span data-ttu-id="107a6-112">Tabulkové modely DirectQuery ukládají pouze metadata modelu.</span><span class="sxs-lookup"><span data-stu-id="107a6-112">For DirectQuery tabular models, only model metadata is stored.</span></span> <span data-ttu-id="107a6-113">Zálohy lze komprimovat a šifrování, v závislosti na zvolených možností hello.</span><span class="sxs-lookup"><span data-stu-id="107a6-113">Backups can be compressed and encrypted, depending on hello options you choose.</span></span> 



## <a name="configure-storage-settings"></a><span data-ttu-id="107a6-114">Konfigurovat nastavení úložiště</span><span class="sxs-lookup"><span data-stu-id="107a6-114">Configure storage settings</span></span>
<span data-ttu-id="107a6-115">Před zahájením zálohování, musíte pro váš server tooconfigure nastavení úložiště.</span><span class="sxs-lookup"><span data-stu-id="107a6-115">Before backing up, you need tooconfigure storage settings for your server.</span></span>


### <a name="tooconfigure-storage-settings"></a><span data-ttu-id="107a6-116">Nastavení úložiště tooconfigure</span><span class="sxs-lookup"><span data-stu-id="107a6-116">tooconfigure storage settings</span></span>
1.  <span data-ttu-id="107a6-117">Na portálu Azure > **nastavení**, klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="107a6-117">In Azure portal > **Settings**, click **Backup**.</span></span>

    ![Zálohy v nastavení](./media/analysis-services-backup/aas-backup-backups.png)

2.  <span data-ttu-id="107a6-119">Klikněte na tlačítko **povoleno**, pak klikněte na tlačítko **nastavení úložiště**.</span><span class="sxs-lookup"><span data-stu-id="107a6-119">Click **Enabled**, then click **Storage Settings**.</span></span>

    ![Povolení](./media/analysis-services-backup/aas-backup-enable.png)

3. <span data-ttu-id="107a6-121">Vyberte účet úložiště nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="107a6-121">Select your storage account or create a new one.</span></span>

4. <span data-ttu-id="107a6-122">Vyberte kontejner nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="107a6-122">Select a container or create a new one.</span></span>

    ![Vyberte kontejner](./media/analysis-services-backup/aas-backup-container.png)

5. <span data-ttu-id="107a6-124">Uložte nastavení zálohování.</span><span class="sxs-lookup"><span data-stu-id="107a6-124">Save your backup settings.</span></span>

    ![Uložit nastavení zálohování](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a><span data-ttu-id="107a6-126">Zálohování</span><span class="sxs-lookup"><span data-stu-id="107a6-126">Backup</span></span>

### <a name="toobackup-by-using-ssms"></a><span data-ttu-id="107a6-127">toobackup pomocí aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="107a6-127">toobackup by using SSMS</span></span>

1. <span data-ttu-id="107a6-128">V aplikaci SSMS, klikněte pravým tlačítkem na databázi > **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="107a6-128">In SSMS, right-click a database > **Back Up**.</span></span>

2. <span data-ttu-id="107a6-129">V **příkaz Backup Database** > **záložní soubor**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="107a6-129">In **Backup Database** > **Backup file**, click **Browse**.</span></span>

3. <span data-ttu-id="107a6-130">V hello **uložit soubor jako** dialogové okno, ověřte cestu ke složce hello a potom zadejte název pro záložní soubor hello.</span><span class="sxs-lookup"><span data-stu-id="107a6-130">In hello **Save file as** dialog, verify hello folder path, and then type a name for hello backup file.</span></span> 

4. <span data-ttu-id="107a6-131">V hello **příkaz Backup Database** dialogové okno, vyberte možnosti.</span><span class="sxs-lookup"><span data-stu-id="107a6-131">In hello **Backup Database** dialog, select options.</span></span>

    <span data-ttu-id="107a6-132">**Povolit souboru přepsat** – vyberte tuto možnost toooverwrite záložní soubory hello stejný název.</span><span class="sxs-lookup"><span data-stu-id="107a6-132">**Allow file overwrite** - Select this option toooverwrite backup files of hello same name.</span></span> <span data-ttu-id="107a6-133">Pokud tuto možnost nevyberete, uložíte soubor hello nemůže mít hello stejný název jako soubor, který již existuje v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="107a6-133">If this option is not selected, hello file you are saving cannot have hello same name as a file that already exists in hello same location.</span></span>

    <span data-ttu-id="107a6-134">**Použít komprese** – vyberte tento záložní soubor možnost toocompress hello.</span><span class="sxs-lookup"><span data-stu-id="107a6-134">**Apply compression** - Select this option toocompress hello backup file.</span></span> <span data-ttu-id="107a6-135">Komprimované záložní soubory ušetřit místo na disku, ale vyžadují mírně zvýší využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="107a6-135">Compressed backup files save disk space, but require slightly higher CPU utilization.</span></span> 

    <span data-ttu-id="107a6-136">**Šifrování záložní soubor** – vyberte tento záložní soubor možnost tooencrypt hello.</span><span class="sxs-lookup"><span data-stu-id="107a6-136">**Encrypt backup file** - Select this option tooencrypt hello backup file.</span></span> <span data-ttu-id="107a6-137">Tato možnost vyžaduje záložní soubor uživatel zadal heslo toosecure hello.</span><span class="sxs-lookup"><span data-stu-id="107a6-137">This option requires a user-supplied password toosecure hello backup file.</span></span> <span data-ttu-id="107a6-138">heslo Hello brání čtení hello zálohování dat jiným způsobem než operaci obnovení.</span><span class="sxs-lookup"><span data-stu-id="107a6-138">hello password prevents reading of hello backup data any other means than a restore operation.</span></span> <span data-ttu-id="107a6-139">Pokud si zvolíte tooencrypt zálohy, uložte hello heslo na bezpečné místo.</span><span class="sxs-lookup"><span data-stu-id="107a6-139">If you choose tooencrypt backups, store hello password in a safe location.</span></span>

5. <span data-ttu-id="107a6-140">Klikněte na tlačítko **OK** toocreate a uložit záložní soubor hello.</span><span class="sxs-lookup"><span data-stu-id="107a6-140">Click **OK** toocreate and save hello backup file.</span></span>


### <a name="powershell"></a><span data-ttu-id="107a6-141">PowerShell</span><span class="sxs-lookup"><span data-stu-id="107a6-141">PowerShell</span></span>
<span data-ttu-id="107a6-142">Použití [zálohování ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) rutiny.</span><span class="sxs-lookup"><span data-stu-id="107a6-142">Use [Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet.</span></span>

## <a name="restore"></a><span data-ttu-id="107a6-143">Obnovení</span><span class="sxs-lookup"><span data-stu-id="107a6-143">Restore</span></span>
<span data-ttu-id="107a6-144">Při obnovování, záložní soubor musí být v účtu úložiště hello, kterou jste nakonfigurovali pro váš server.</span><span class="sxs-lookup"><span data-stu-id="107a6-144">When restoring, your backup file must be in hello storage account you've configured for your server.</span></span> <span data-ttu-id="107a6-145">Pokud potřebujete toomove záložní soubor z účtu místní tooyour umístění úložiště, použijte [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) nebo hello [AzCopy](../storage/common/storage-use-azcopy.md) nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="107a6-145">If you need toomove a backup file from an on-premises location tooyour storage account, use [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) or hello [AzCopy](../storage/common/storage-use-azcopy.md) command-line utility.</span></span> 



> [!NOTE]
> <span data-ttu-id="107a6-146">Pokud se obnovení ze serveru místní, musíte odebrat všechny uživatele domény hello z rolí hello model a přidat je zpět toohello role jako uživatelů Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="107a6-146">If you're restoring from an on-premises server, you must remove all hello domain users from hello model's roles and add them back toohello roles as Azure Active Directory users.</span></span>
> 
> 

### <a name="toorestore-by-using-ssms"></a><span data-ttu-id="107a6-147">toorestore pomocí aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="107a6-147">toorestore by using SSMS</span></span>

1. <span data-ttu-id="107a6-148">V aplikaci SSMS, klikněte pravým tlačítkem na databázi > **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="107a6-148">In SSMS, right-click a database > **Restore**.</span></span>

2. <span data-ttu-id="107a6-149">V hello **příkaz Backup Database** dialogové okno, v **záložní soubor**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="107a6-149">In hello **Backup Database** dialog, in **Backup file**, click **Browse**.</span></span>

3. <span data-ttu-id="107a6-150">V hello **najít soubory databáze** dialogové okno, vyberte hello soubor má toorestore.</span><span class="sxs-lookup"><span data-stu-id="107a6-150">In hello **Locate Database Files** dialog, select hello file you want toorestore.</span></span>

4. <span data-ttu-id="107a6-151">V **obnovte databázi**, vyberte databázi hello.</span><span class="sxs-lookup"><span data-stu-id="107a6-151">In **Restore database**, select hello database.</span></span>

5. <span data-ttu-id="107a6-152">Zadejte možnosti.</span><span class="sxs-lookup"><span data-stu-id="107a6-152">Specify options.</span></span> <span data-ttu-id="107a6-153">Možnosti zabezpečení se musí shodovat hello možnosti zálohování, který jste použili při zálohování.</span><span class="sxs-lookup"><span data-stu-id="107a6-153">Security options must match hello backup options you used when backing up.</span></span>


### <a name="powershell"></a><span data-ttu-id="107a6-154">PowerShell</span><span class="sxs-lookup"><span data-stu-id="107a6-154">PowerShell</span></span>

<span data-ttu-id="107a6-155">Použití [obnovení ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) rutiny.</span><span class="sxs-lookup"><span data-stu-id="107a6-155">Use [Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet.</span></span>


## <a name="related-information"></a><span data-ttu-id="107a6-156">Související informace</span><span class="sxs-lookup"><span data-stu-id="107a6-156">Related information</span></span>

[<span data-ttu-id="107a6-157">Účty úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="107a6-157">Azure storage accounts</span></span>](../storage/common/storage-create-storage-account.md)  
<span data-ttu-id="107a6-158">[Vysoká dostupnost](analysis-services-bcdr.md)   </span><span class="sxs-lookup"><span data-stu-id="107a6-158">[High availability](analysis-services-bcdr.md)   </span></span>  
[<span data-ttu-id="107a6-159">Správa služby Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="107a6-159">Manage Azure Analysis Services</span></span>](analysis-services-manage.md)
