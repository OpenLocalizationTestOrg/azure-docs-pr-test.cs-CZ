---
title: "aaaBack až Windows soubory a složky tooAzure (Resource Manager) | Microsoft Docs"
description: "Přečtěte si tooback až Windows tooAzure soubory a složky v nasazení Resource Manager."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "jak toobackup; jak tooback; zálohování souborů a složek"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 8/15/2017
ms.author: markgal;
ms.openlocfilehash: 07d6580a84d5092ed2c61bf86ff5fcb148423ef2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="first-look-back-up-files-and-folders-in-resource-manager-deployment"></a><span data-ttu-id="95800-104">První pohled: Zálohování souborů a složek v nasazení podle modelu Resource Manager</span><span class="sxs-lookup"><span data-stu-id="95800-104">First look: back up files and folders in Resource Manager deployment</span></span>
<span data-ttu-id="95800-105">Tento článek vysvětluje, jak soubory a složky tooAzure tooback Windows serveru (nebo počítače se systémem Windows) pomocí nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="95800-105">This article explains how tooback up your Windows Server (or Windows computer) files and folders tooAzure using a Resource Manager deployment.</span></span> <span data-ttu-id="95800-106">Je kurz určený toowalk vás provede základy hello.</span><span class="sxs-lookup"><span data-stu-id="95800-106">It's a tutorial intended toowalk you through hello basics.</span></span> <span data-ttu-id="95800-107">Pokud chcete tooget spuštěna pomocí služby Azure Backup, jste hello správném místě.</span><span class="sxs-lookup"><span data-stu-id="95800-107">If you want tooget started using Azure Backup, you're in hello right place.</span></span>

<span data-ttu-id="95800-108">Pokud chcete, aby tooknow Další informace o zálohování Azure, přečtěte si to [přehled](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="95800-108">If you want tooknow more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="95800-109">Pokud předplatné Azure nemáte, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/), který vám umožní přístup ke službám Azure.</span><span class="sxs-lookup"><span data-stu-id="95800-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="95800-110">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="95800-110">Create a recovery services vault</span></span>
<span data-ttu-id="95800-111">tooback soubory a složky, musíte toocreate trezoru služeb zotavení v hello oblasti, kde chcete toostore hello data.</span><span class="sxs-lookup"><span data-stu-id="95800-111">tooback up your files and folders, you need toocreate a Recovery Services vault in hello region where you want toostore hello data.</span></span> <span data-ttu-id="95800-112">Budete také potřebovat toodetermine způsob replikace úložiště.</span><span class="sxs-lookup"><span data-stu-id="95800-112">You also need toodetermine how you want your storage replicated.</span></span>

### <a name="toocreate-a-recovery-services-vault"></a><span data-ttu-id="95800-113">toocreate trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="95800-113">toocreate a Recovery Services vault</span></span>
1. <span data-ttu-id="95800-114">Pokud jste tak již neučinili, přihlaste se toohello [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="95800-114">If you haven't already done so, sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="95800-115">V nabídce centra hello, klikněte na tlačítko **další služby** a v hello seznamu prostředků zadejte **služeb zotavení** a klikněte na tlačítko **trezory služeb zotavení**.</span><span class="sxs-lookup"><span data-stu-id="95800-115">On hello Hub menu, click **More services** and in hello list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="95800-117">Pokud jsou v rámci předplatného hello trezory služeb zotavení, jsou uvedeny hello trezorů.</span><span class="sxs-lookup"><span data-stu-id="95800-117">If there are recovery services vaults in hello subscription, hello vaults are listed.</span></span>
3. <span data-ttu-id="95800-118">Na hello **trezory služeb zotavení** nabídky, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="95800-118">On hello **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="95800-120">Služby zotavení Hello otevře se okno trezoru výzvou tooprovide **název**, **předplatné**, **skupiny prostředků**, a **umístění**.</span><span class="sxs-lookup"><span data-stu-id="95800-120">hello Recovery Services vault blade opens, prompting you tooprovide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="95800-122">Pro **název**, zadejte popisný název tooidentify hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="95800-122">For **Name**, enter a friendly name tooidentify hello vault.</span></span> <span data-ttu-id="95800-123">Název Hello musí toobe jedinečný pro hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="95800-123">hello name needs toobe unique for hello Azure subscription.</span></span> <span data-ttu-id="95800-124">Zadejte název v rozsahu 2 až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="95800-124">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="95800-125">Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="95800-125">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="95800-126">V hello **předplatné** pomocí hello rozevírací nabídky toochoose hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="95800-126">In hello **Subscription** section, use hello drop-down menu toochoose hello Azure subscription.</span></span> <span data-ttu-id="95800-127">Pokud chcete použít jenom jedno předplatné, zobrazí se toto předplatné a toohello další krok můžete vynechat.</span><span class="sxs-lookup"><span data-stu-id="95800-127">If you use only one subscription, that subscription appears and you can skip toohello next step.</span></span> <span data-ttu-id="95800-128">Pokud si nejste jisti, jaké předplatné toouse, použít výchozí hello (nebo navrhované) předplatné.</span><span class="sxs-lookup"><span data-stu-id="95800-128">If you are not sure which subscription toouse, use hello default (or suggested) subscription.</span></span> <span data-ttu-id="95800-129">Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="95800-129">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="95800-130">V hello **skupiny prostředků** části:</span><span class="sxs-lookup"><span data-stu-id="95800-130">In hello **Resource group** section:</span></span>

    * <span data-ttu-id="95800-131">Vyberte **vytvořit nový** Pokud chcete, aby toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="95800-131">select **Create new** if you want toocreate a new Resource group.</span></span>
    <span data-ttu-id="95800-132">Nebo</span><span class="sxs-lookup"><span data-stu-id="95800-132">Or</span></span>
    * <span data-ttu-id="95800-133">Vyberte **použít existující** a klikněte na tlačítko hello rozevírací nabídky toosee hello seznam dostupných skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="95800-133">select **Use existing** and click hello drop-down menu toosee hello available list of Resource groups.</span></span>

  <span data-ttu-id="95800-134">Úplné informace o skupinách prostředků najdete v tématu hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="95800-134">For complete information on Resource groups, see hello [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="95800-135">Klikněte na tlačítko **umístění** tooselect hello zeměpisnou oblast trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="95800-135">Click **Location** tooselect hello geographic region for hello vault.</span></span> <span data-ttu-id="95800-136">Tato volba určuje hello zeměpisnou oblast, kde vaše zálohovaná data se odesílají.</span><span class="sxs-lookup"><span data-stu-id="95800-136">This choice determines hello geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="95800-137">Hello dolní části hello okno trezoru služeb zotavení, klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="95800-137">At hello bottom of hello Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="95800-138">Můžete to trvat několik minut, než hello toobe vytvořit trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="95800-138">It can take several minutes for hello Recovery Services vault toobe created.</span></span> <span data-ttu-id="95800-139">Sledujte oznámení stavu hello v horním pravém oblasti hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="95800-139">Monitor hello status notifications in hello upper right-hand area of hello portal.</span></span> <span data-ttu-id="95800-140">Po vytvoření svůj trezor se zobrazí v seznamu hello trezorů služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="95800-140">Once your vault is created, it appears in hello list of Recovery Services vaults.</span></span> <span data-ttu-id="95800-141">Pokud se trezor nezobrazí ani po několika minutách, klikněte na **Obnovit**.</span><span class="sxs-lookup"><span data-stu-id="95800-141">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Kliknutí na tlačítko Obnovit](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="95800-143">Jakmile se zobrazí váš trezor hello seznamu trezorů služeb zotavení, jste redundance úložiště připravené tooset hello.</span><span class="sxs-lookup"><span data-stu-id="95800-143">Once you see your vault in hello list of Recovery Services vaults, you are ready tooset hello storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-hello-vault"></a><span data-ttu-id="95800-144">Nastavit redundance úložiště pro trezor hello</span><span class="sxs-lookup"><span data-stu-id="95800-144">Set storage redundancy for hello vault</span></span>
<span data-ttu-id="95800-145">Při vytváření trezoru služeb zotavení, ujistěte se, že redundance úložiště nakonfigurovaných hello požadovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="95800-145">When you create a Recovery Services vault, make sure storage redundancy is configured hello way you want.</span></span>

1. <span data-ttu-id="95800-146">Z hello **trezory služeb zotavení** okně klikněte na tlačítko Nový trezor hello.</span><span class="sxs-lookup"><span data-stu-id="95800-146">From hello **Recovery Services vaults** blade, click hello new vault.</span></span>

    ![Vyberte nový trezor hello ze seznamu hello trezor služeb zotavení](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="95800-148">Když vyberete hello trezoru, hello **trezor služeb zotavení** narrows okno a okno nastavení hello (*jehož hello název trezoru hello v horní části hello*) a otevřete okno Podrobnosti trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="95800-148">When you select hello vault, hello **Recovery Services vault** blade narrows, and hello Settings blade (*which has hello name of hello vault at hello top*) and hello vault details blade open.</span></span>

    ![Zobrazení hello konfiguraci úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="95800-150">V okně Nastavení hello nový trezor, použijte hello svislé snímku tooscroll dolů toohello části Správa a klikněte na tlačítko **infrastruktura zálohování**.</span><span class="sxs-lookup"><span data-stu-id="95800-150">In hello new vault's Settings blade, use hello vertical slide tooscroll down toohello Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="95800-151">Otevře se okno infrastruktura zálohování Hello.</span><span class="sxs-lookup"><span data-stu-id="95800-151">hello Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="95800-152">V okně infrastruktura zálohování hello, klikněte na tlačítko **konfigurace zálohování** tooopen hello **konfigurace zálohování** okno.</span><span class="sxs-lookup"><span data-stu-id="95800-152">In hello Backup Infrastructure blade, click **Backup Configuration** tooopen hello **Backup Configuration** blade.</span></span>

    ![Nastavit hello konfiguraci úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="95800-154">Zvolte hello vhodnou možnost replikace pro svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="95800-154">Choose hello appropriate storage replication option for your vault.</span></span>

    ![volby konfigurace úložiště](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="95800-156">Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="95800-156">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="95800-157">Pokud používáte Azure jako koncový bod primární úložiště záloh, pokračujte toouse **geograficky redundantní**.</span><span class="sxs-lookup"><span data-stu-id="95800-157">If you use Azure as a primary backup storage endpoint, continue toouse **Geo-redundant**.</span></span> <span data-ttu-id="95800-158">Pokud nepoužíváte Azure jako koncový bod primární úložiště záloh, zvolte **místně redundantní**, což snižuje náklady na úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="95800-158">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces hello Azure storage costs.</span></span> <span data-ttu-id="95800-159">Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště najdete v tomto [přehledu redundance úložiště](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="95800-159">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="95800-160">Teď když jste vytvořili trezor, nakonfigurujte pro něj zálohování souborů a složek.</span><span class="sxs-lookup"><span data-stu-id="95800-160">Now that you've created a vault, configure it for backing up files and folders.</span></span>

## <a name="configure-hello-vault"></a><span data-ttu-id="95800-161">Konfigurace hello trezoru</span><span class="sxs-lookup"><span data-stu-id="95800-161">Configure hello vault</span></span>
1. <span data-ttu-id="95800-162">Na hello okno trezoru služeb zotavení (pro hello trezor jste právě vytvořili), v části Začínáme hello, klikněte na tlačítko **zálohování**, pak na hello **Začínáme se zálohováním** vyberte  **Cíle zálohování**.</span><span class="sxs-lookup"><span data-stu-id="95800-162">On hello Recovery Services vault blade (for hello vault you just created), in hello Getting Started section, click **Backup**, then on hello **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="95800-164">Hello **cíl zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="95800-164">hello **Backup Goal** blade opens.</span></span>

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="95800-166">Z hello **kde běží vaše úloha?** rozevírací nabídky vyberte **místní**.</span><span class="sxs-lookup"><span data-stu-id="95800-166">From hello **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="95800-167">Možnost **Místní** jste vybrali proto, že počítačem s Windows Serverem nebo Windows je fyzický počítač, který není v Azure.</span><span class="sxs-lookup"><span data-stu-id="95800-167">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="95800-168">Z hello **co chcete toobackup?** nabídce vyberte možnost **soubory a složky**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="95800-168">From hello **What do you want toobackup?** menu, select **Files and folders**, and click **OK**.</span></span>

    ![Konfigurace souborů a složek](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

    <span data-ttu-id="95800-170">Po kliknutí na tlačítko OK, objeví se zaškrtnutí další příliš**cíl zálohování**a hello **připravit infrastrukturu** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="95800-170">After clicking OK, a checkmark appears next too**Backup goal**, and hello **Prepare infrastructure** blade opens.</span></span>

    ![Cíl zálohování je nakonfigurovaný, teď se připraví infrastruktura](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="95800-172">Na hello **připravit infrastrukturu** okně klikněte na tlačítko **stáhnout agenta pro Windows Server nebo klienta Windows**.</span><span class="sxs-lookup"><span data-stu-id="95800-172">On hello **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="95800-174">Pokud používáte Windows Server nezbytné, zvolte toodownload hello agenta pro Windows Server nezbytné.</span><span class="sxs-lookup"><span data-stu-id="95800-174">If you are using Windows Server Essential, then choose toodownload hello agent for Windows Server Essential.</span></span> <span data-ttu-id="95800-175">Místní nabídky vás vyzve k toorun nebo uložit MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="95800-175">A pop-up menu prompts you toorun or save MARSAgentInstaller.exe.</span></span>

    ![Dialogové okno MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="95800-177">V místní nabídce hello stahování, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="95800-177">In hello download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="95800-178">Ve výchozím nastavení, hello **MARSagentinstaller.exe** soubor je uložen tooyour složky se staženými soubory.</span><span class="sxs-lookup"><span data-stu-id="95800-178">By default, hello **MARSagentinstaller.exe** file is saved tooyour Downloads folder.</span></span> <span data-ttu-id="95800-179">Po dokončení instalačního programu hello, zobrazí se automaticky otevírané okno s dotazem, pokud má instalační program hello toorun nebo otevřete složku hello.</span><span class="sxs-lookup"><span data-stu-id="95800-179">When hello installer completes, you will see a pop-up asking if you want toorun hello installer, or open hello folder.</span></span>

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="95800-181">Tooinstall hello agent ještě nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="95800-181">You don't need tooinstall hello agent yet.</span></span> <span data-ttu-id="95800-182">Po stažení hello přihlašovací údaje trezoru můžete nainstalovat agenta hello.</span><span class="sxs-lookup"><span data-stu-id="95800-182">You can install hello agent after you have downloaded hello vault credentials.</span></span>

6. <span data-ttu-id="95800-183">Na hello **připravit infrastrukturu** okně klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="95800-183">On hello **Prepare infrastructure** blade, click **Download**.</span></span>

    ![stažení přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="95800-185">přihlašovací údaje trezoru Hello tooyour stahování složka pro stahování.</span><span class="sxs-lookup"><span data-stu-id="95800-185">hello vault credentials download tooyour Downloads folder.</span></span> <span data-ttu-id="95800-186">Po dokončení stahování přihlašovací údaje trezoru hello zobrazí automaticky otevírané okno s dotazem, pokud mají být tooopen nebo uložit přihlašovací údaje hello.</span><span class="sxs-lookup"><span data-stu-id="95800-186">After hello vault credentials finish downloading, you see a pop-up asking if you want tooopen or save hello credentials.</span></span> <span data-ttu-id="95800-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="95800-187">Click **Save**.</span></span> <span data-ttu-id="95800-188">Pokud omylem kliknete **otevřete**, umožní hello dialog, který se pokusí přihlašovací údaje trezoru hello tooopen, nezdaří.</span><span class="sxs-lookup"><span data-stu-id="95800-188">If you accidentally click **Open**, let hello dialog that attempts tooopen hello vault credentials, fail.</span></span> <span data-ttu-id="95800-189">Nelze otevřít přihlašovací údaje trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="95800-189">You cannot open hello vault credentials.</span></span> <span data-ttu-id="95800-190">Pokračujte dalším krokem toohello.</span><span class="sxs-lookup"><span data-stu-id="95800-190">Proceed toohello next step.</span></span> <span data-ttu-id="95800-191">přihlašovací údaje trezoru Hello jsou ve složce pro stahování hello.</span><span class="sxs-lookup"><span data-stu-id="95800-191">hello vault credentials are in hello Downloads folder.</span></span>   

    ![dokončené stahování přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a><span data-ttu-id="95800-193">Instalace a registrace agenta hello</span><span class="sxs-lookup"><span data-stu-id="95800-193">Install and register hello agent</span></span>

> [!NOTE]
> <span data-ttu-id="95800-194">Povolení zálohování prostřednictvím portálu Azure hello ještě není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="95800-194">Enabling backup through hello Azure portal is not available, yet.</span></span> <span data-ttu-id="95800-195">Použijte agenta služeb zotavení Microsoft Azure tooback hello soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="95800-195">Use hello Microsoft Azure Recovery Services Agent tooback up your files and folders.</span></span>
>

1. <span data-ttu-id="95800-196">Vyhledejte a dvakrát klikněte na hello **MARSagentinstaller.exe** z hello stahování složky (nebo jiného uloženého umístění).</span><span class="sxs-lookup"><span data-stu-id="95800-196">Locate and double-click hello **MARSagentinstaller.exe** from hello Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="95800-197">Instalační program Hello poskytuje řadu zprávy jako extrahuje, nainstaluje a zaregistruje hello agenta služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="95800-197">hello installer provides a series of messages as it extracts, installs, and registers hello Recovery Services agent.</span></span>

    ![spuštění přihlašovacích údajů instalačního programu agenta Recovery Services](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="95800-199">Hello dokončení Průvodce instalací agenta služeb zotavení Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="95800-199">Complete hello Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="95800-200">toocomplete hello průvodce, budete muset:</span><span class="sxs-lookup"><span data-stu-id="95800-200">toocomplete hello wizard, you need to:</span></span>

   * <span data-ttu-id="95800-201">Vyberte umístění složky pro instalaci a mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="95800-201">Choose a location for hello installation and cache folder.</span></span>
   * <span data-ttu-id="95800-202">Pokud používáte proxy serveru tooconnect toohello zadejte informace o serveru vašeho proxy Internetu.</span><span class="sxs-lookup"><span data-stu-id="95800-202">Provide your proxy server info if you use a proxy server tooconnect toohello internet.</span></span>
   * <span data-ttu-id="95800-203">Zadat svoje uživatelské jméno a heslo, pokud používáte ověřený server proxy.</span><span class="sxs-lookup"><span data-stu-id="95800-203">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="95800-204">Zadejte přihlašovací údaje trezoru stáhnout hello</span><span class="sxs-lookup"><span data-stu-id="95800-204">Provide hello downloaded vault credentials</span></span>
   * <span data-ttu-id="95800-205">Šifrovací přístupové heslo hello uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="95800-205">Save hello encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="95800-206">Pokud ztratíte nebo zapomenete heslo hello, Microsoft vám nemůže pomoci obnovit zálohovaná data hello.</span><span class="sxs-lookup"><span data-stu-id="95800-206">If you lose or forget hello passphrase, Microsoft cannot help recover hello backup data.</span></span> <span data-ttu-id="95800-207">Hello soubor uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="95800-207">Save hello file in a secure location.</span></span> <span data-ttu-id="95800-208">Je požadovaná toorestore zálohu.</span><span class="sxs-lookup"><span data-stu-id="95800-208">It is required toorestore a backup.</span></span>
     >
     >

<span data-ttu-id="95800-209">Hello agent je nyní nainstalovaný a váš počítač je registrovaný toohello trezoru.</span><span class="sxs-lookup"><span data-stu-id="95800-209">hello agent is now installed and your machine is registered toohello vault.</span></span> <span data-ttu-id="95800-210">Máte připravené tooconfigure a naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="95800-210">You're ready tooconfigure and schedule your backup.</span></span>

## <a name="network-and-connectivity-requirements"></a><span data-ttu-id="95800-211">Požadavky na síť a připojení</span><span class="sxs-lookup"><span data-stu-id="95800-211">Network and Connectivity Requirements</span></span>

<span data-ttu-id="95800-212">Pokud váš počítač nebo na proxy server má omezený přístup k Internetu, ujistěte se, že nastavení brány firewall na hello mcahine a proxy jsou nakonfigurované tooallow hello následující adresy URL:</span><span class="sxs-lookup"><span data-stu-id="95800-212">If your machine/proxy has limited internet access, ensure that firewall settings on hello mcahine/proxy are configured tooallow hello following URLs:</span></span> <br>
    1. <span data-ttu-id="95800-213">www.msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="95800-213">www.msftncsi.com</span></span>
    2. <span data-ttu-id="95800-214">*.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="95800-214">*.Microsoft.com</span></span>
    3. <span data-ttu-id="95800-215">*.WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="95800-215">*.WindowsAzure.com</span></span>
    4. <span data-ttu-id="95800-216">*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="95800-216">*.microsoftonline.com</span></span>
    5. <span data-ttu-id="95800-217">*.windows.ne</span><span class="sxs-lookup"><span data-stu-id="95800-217">*.windows.ne</span></span>

## <a name="back-up-your-files-and-folders"></a><span data-ttu-id="95800-218">Zálohování souborů a složek</span><span class="sxs-lookup"><span data-stu-id="95800-218">Back up your files and folders</span></span>
<span data-ttu-id="95800-219">Hello prvotní záloha zahrnuje dvě klíčové úlohy:</span><span class="sxs-lookup"><span data-stu-id="95800-219">hello initial backup includes two key tasks:</span></span>

* <span data-ttu-id="95800-220">Plán zálohování hello</span><span class="sxs-lookup"><span data-stu-id="95800-220">Schedule hello backup</span></span>
* <span data-ttu-id="95800-221">Zálohování souborů a složek pro hello poprvé</span><span class="sxs-lookup"><span data-stu-id="95800-221">Back up files and folders for hello first time</span></span>

<span data-ttu-id="95800-222">toocomplete hello prvotní zálohování, agenta služeb zotavení Microsoft Azure použijte hello.</span><span class="sxs-lookup"><span data-stu-id="95800-222">toocomplete hello initial backup, use hello Microsoft Azure Recovery Services agent.</span></span>

### <a name="tooschedule-hello-backup-job"></a><span data-ttu-id="95800-223">Úloha zálohování tooschedule hello</span><span class="sxs-lookup"><span data-stu-id="95800-223">tooschedule hello backup job</span></span>
1. <span data-ttu-id="95800-224">Otevřete agenta služeb zotavení Microsoft Azure hello.</span><span class="sxs-lookup"><span data-stu-id="95800-224">Open hello Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="95800-225">Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.</span><span class="sxs-lookup"><span data-stu-id="95800-225">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Spuštění agenta služeb zotavení Azure hello](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)
2. <span data-ttu-id="95800-227">V hello agenta služeb zotavení, klikněte na **plánem zálohování**.</span><span class="sxs-lookup"><span data-stu-id="95800-227">In hello Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Naplánování zálohování Windows Serveru](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)
3. <span data-ttu-id="95800-229">Na hello Začínáme stránku hello Průvodce plánem zálohování, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="95800-229">On hello Getting started page of hello Schedule Backup Wizard, click **Next**.</span></span>
4. <span data-ttu-id="95800-230">Na stránce tooBackup hello vyberte položky, klikněte na **přidat položky**.</span><span class="sxs-lookup"><span data-stu-id="95800-230">On hello Select Items tooBackup page, click **Add Items**.</span></span>
5. <span data-ttu-id="95800-231">Vyberte hello soubory a složky, které chcete tooback nahoru a pak klikněte na tlačítko **nevadí**.</span><span class="sxs-lookup"><span data-stu-id="95800-231">Select hello files and folders that you want tooback up, and then click **Okay**.</span></span>
6. <span data-ttu-id="95800-232">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="95800-232">Click **Next**.</span></span>
7. <span data-ttu-id="95800-233">Na hello **zadání plánu zálohování** zadejte hello **plán zálohování** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="95800-233">On hello **Specify Backup Schedule** page, specify hello **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="95800-234">Můžete naplánovat denní (probíhající maximálně třikrát za den) nebo týdenní zálohování.</span><span class="sxs-lookup"><span data-stu-id="95800-234">You can schedule daily (at a maximum rate of three times per day) or weekly backups.</span></span>

    ![Položky k zálohování z Windows Serveru](./media/backup-try-azure-backup-in-10-mins/specify-backup-schedule-close.png)

   > [!NOTE]
   > <span data-ttu-id="95800-236">Další informace o tom, jak toospecify hello plán zálohování, najdete v článku hello [pomocí Azure Backup tooreplace infrastruktury pásky](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="95800-236">For more information about how toospecify hello backup schedule, see hello article [Use Azure Backup tooreplace your tape infrastructure](backup-azure-backup-cloud-as-tape.md).</span></span>
   >

8. <span data-ttu-id="95800-237">Na hello **výběr zásady uchovávání informací** stránky, vyberte hello **zásady uchovávání informací** pro záložní kopii hello.</span><span class="sxs-lookup"><span data-stu-id="95800-237">On hello **Select Retention Policy** page, select hello **Retention Policy** for hello backup copy.</span></span>

    <span data-ttu-id="95800-238">zásady uchovávání informací Hello Určuje, jak dlouho je uložen hello zálohovaná data.</span><span class="sxs-lookup"><span data-stu-id="95800-238">hello retention policy specifies how long hello backup data is stored.</span></span> <span data-ttu-id="95800-239">Místo zadání "ploché zásady" pro všechny body zálohy, můžete zadat různé zásady uchovávání informací podle toho, kdy dochází k zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="95800-239">Rather than specifying a “flat policy” for all backup points, you can specify different retention policies based on when hello backup occurs.</span></span> <span data-ttu-id="95800-240">Můžete upravit hello uchování denní, týdenní, měsíční a roční zásady toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="95800-240">You can modify hello daily, weekly, monthly, and yearly retention policies toomeet your needs.</span></span>
9. <span data-ttu-id="95800-241">Na stránce Výběr typu prvotní zálohy hello vyberte typ prvotní zálohy hello.</span><span class="sxs-lookup"><span data-stu-id="95800-241">On hello Choose Initial Backup Type page, choose hello initial backup type.</span></span> <span data-ttu-id="95800-242">Ponechte možnost hello **automaticky přes síť hello** vybrané a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="95800-242">Leave hello option **Automatically over hello network** selected, and then click **Next**.</span></span>

    <span data-ttu-id="95800-243">Můžete zálohovat automaticky přes síť hello nebo můžete zálohovat do offline režimu.</span><span class="sxs-lookup"><span data-stu-id="95800-243">You can back up automatically over hello network, or you can back up offline.</span></span> <span data-ttu-id="95800-244">Hello zbývající část tohoto článku popisuje proces hello zálohování automaticky.</span><span class="sxs-lookup"><span data-stu-id="95800-244">hello remainder of this article describes hello process for backing up automatically.</span></span> <span data-ttu-id="95800-245">Pokud dáváte přednost toodo zálohu do offline režimu, přečtěte si článek hello [pracovní postup Offline zálohování v Azure Backup](backup-azure-backup-import-export.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="95800-245">If you prefer toodo an offline backup, review hello article [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) for additional information.</span></span>
10. <span data-ttu-id="95800-246">Na stránce pro potvrzení hello, zkontrolujte hello informace a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="95800-246">On hello Confirmation page, review hello information, and then click **Finish**.</span></span>
11. <span data-ttu-id="95800-247">Po dokončení Průvodce hello vytváření hello plán zálohování, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="95800-247">After hello wizard finishes creating hello backup schedule, click **Close**.</span></span>

### <a name="tooback-up-files-and-folders-for-hello-first-time"></a><span data-ttu-id="95800-248">tooback soubory a složky pro hello poprvé</span><span class="sxs-lookup"><span data-stu-id="95800-248">tooback up files and folders for hello first time</span></span>
1. <span data-ttu-id="95800-249">V hello agenta služeb zotavení, klikněte na **zálohovat nyní** toocomplete hello počáteční synchronizace replik indexů přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="95800-249">In hello Recovery Services agent, click **Back Up Now** toocomplete hello initial seeding over hello network.</span></span>

    ![Zálohovat nyní ve Windows Serveru](./media/backup-try-azure-backup-in-10-mins/backup-now.png)
2. <span data-ttu-id="95800-251">Na stránce pro potvrzení hello zkontrolujte hello nastavení, která hello zpět si teď Průvodce použije tooback hello počítač.</span><span class="sxs-lookup"><span data-stu-id="95800-251">On hello Confirmation page, review hello settings that hello Back Up Now Wizard will use tooback up hello machine.</span></span> <span data-ttu-id="95800-252">Poté klikněte na **Zálohovat**.</span><span class="sxs-lookup"><span data-stu-id="95800-252">Then click **Back Up**.</span></span>
3. <span data-ttu-id="95800-253">Klikněte na tlačítko **Zavřít** tooclose hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="95800-253">Click **Close** tooclose hello wizard.</span></span> <span data-ttu-id="95800-254">Pokud hello průvodce zavřete před dokončením procesu zálohování hello, bude pokračovat hello Průvodce toorun hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="95800-254">If you close hello wizard before hello backup process finishes, hello wizard continues toorun in hello background.</span></span>

<span data-ttu-id="95800-255">Po dokončení prvotní zálohování hello hello **úloha byla dokončena** stav se zobrazí v konzole zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="95800-255">After hello initial backup is completed, hello **Job completed** status appears in hello Backup console.</span></span>

![Dokončení IR](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a><span data-ttu-id="95800-257">Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="95800-257">Questions?</span></span>
<span data-ttu-id="95800-258">Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="95800-258">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="95800-259">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95800-259">Next steps</span></span>
* <span data-ttu-id="95800-260">Zdroj dalších informací o [zálohování počítačů se systémem Windows](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="95800-260">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="95800-261">Teď, když jste zálohovali své soubory a složky, můžete [spravovat svoje trezory a servery](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="95800-261">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="95800-262">Pokud potřebujete toorestore zálohu, použijte tento článek příliš[obnovit soubory tooa Windows počítač](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="95800-262">If you need toorestore a backup, use this article too[restore files tooa Windows machine](backup-azure-restore-windows-server.md).</span></span>
