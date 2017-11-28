---
title: "aaaBack až tooAzure stavu systému Windows | Microsoft Docs"
description: "Přečtěte si tooback hello stav systému Windows Server nebo tooAzure počítače Windows."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "jak toobackup; jak tooback; zálohování souborů a složek"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: be5d4be81af981c10de82add9fe962a730753cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a><span data-ttu-id="5e847-104">Zálohování stavu systému Windows v nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5e847-104">Back up Windows system state in Resource Manager deployment</span></span>
<span data-ttu-id="5e847-105">Tento článek vysvětluje, jak tooback systému Windows Server stavu tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5e847-105">This article explains how tooback up your Windows Server system state tooAzure.</span></span> <span data-ttu-id="5e847-106">Je kurz určený toowalk vás provede základy hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-106">It's a tutorial intended toowalk you through hello basics.</span></span>

<span data-ttu-id="5e847-107">Pokud chcete, aby tooknow Další informace o zálohování Azure, přečtěte si to [přehled](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="5e847-107">If you want tooknow more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="5e847-108">Pokud předplatné Azure nemáte, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/), který vám umožní přístup ke službám Azure.</span><span class="sxs-lookup"><span data-stu-id="5e847-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="5e847-109">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="5e847-109">Create a recovery services vault</span></span>
<span data-ttu-id="5e847-110">tooback soubory a složky, musíte toocreate trezoru služeb zotavení v hello oblasti, kde chcete toostore hello data.</span><span class="sxs-lookup"><span data-stu-id="5e847-110">tooback up your files and folders, you need toocreate a Recovery Services vault in hello region where you want toostore hello data.</span></span> <span data-ttu-id="5e847-111">Budete také potřebovat toodetermine způsob replikace úložiště.</span><span class="sxs-lookup"><span data-stu-id="5e847-111">You also need toodetermine how you want your storage replicated.</span></span>

### <a name="toocreate-a-recovery-services-vault"></a><span data-ttu-id="5e847-112">toocreate trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="5e847-112">toocreate a Recovery Services vault</span></span>
1. <span data-ttu-id="5e847-113">Pokud jste tak již neučinili, přihlaste se toohello [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="5e847-113">If you haven't already done so, sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="5e847-114">V nabídce centra hello, klikněte na tlačítko **další služby** a v hello seznamu prostředků zadejte **služeb zotavení** a klikněte na tlačítko **trezory služeb zotavení**.</span><span class="sxs-lookup"><span data-stu-id="5e847-114">On hello Hub menu, click **More services** and in hello list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="5e847-116">Pokud jsou v rámci předplatného hello trezory služeb zotavení, jsou uvedeny hello trezorů.</span><span class="sxs-lookup"><span data-stu-id="5e847-116">If there are recovery services vaults in hello subscription, hello vaults are listed.</span></span>
3. <span data-ttu-id="5e847-117">Na hello **trezory služeb zotavení** nabídky, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5e847-117">On hello **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="5e847-119">Služby zotavení Hello otevře se okno trezoru výzvou tooprovide **název**, **předplatné**, **skupiny prostředků**, a **umístění**.</span><span class="sxs-lookup"><span data-stu-id="5e847-119">hello Recovery Services vault blade opens, prompting you tooprovide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="5e847-121">Pro **název**, zadejte popisný název tooidentify hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="5e847-121">For **Name**, enter a friendly name tooidentify hello vault.</span></span> <span data-ttu-id="5e847-122">Název Hello musí toobe jedinečný pro hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="5e847-122">hello name needs toobe unique for hello Azure subscription.</span></span> <span data-ttu-id="5e847-123">Zadejte název v rozsahu 2 až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="5e847-123">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="5e847-124">Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="5e847-124">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="5e847-125">V hello **předplatné** pomocí hello rozevírací nabídky toochoose hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="5e847-125">In hello **Subscription** section, use hello drop-down menu toochoose hello Azure subscription.</span></span> <span data-ttu-id="5e847-126">Pokud chcete použít jenom jedno předplatné, zobrazí se toto předplatné a toohello další krok můžete vynechat.</span><span class="sxs-lookup"><span data-stu-id="5e847-126">If you use only one subscription, that subscription appears and you can skip toohello next step.</span></span> <span data-ttu-id="5e847-127">Pokud si nejste jisti, jaké předplatné toouse, použít výchozí hello (nebo navrhované) předplatné.</span><span class="sxs-lookup"><span data-stu-id="5e847-127">If you are not sure which subscription toouse, use hello default (or suggested) subscription.</span></span> <span data-ttu-id="5e847-128">Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="5e847-128">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="5e847-129">V hello **skupiny prostředků** části:</span><span class="sxs-lookup"><span data-stu-id="5e847-129">In hello **Resource group** section:</span></span>

    * <span data-ttu-id="5e847-130">Vyberte **vytvořit nový** Pokud chcete, aby toocreate skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5e847-130">select **Create new** if you want toocreate a Resource group.</span></span>
    <span data-ttu-id="5e847-131">Nebo</span><span class="sxs-lookup"><span data-stu-id="5e847-131">Or</span></span>
    * <span data-ttu-id="5e847-132">Vyberte **použít existující** a klikněte na tlačítko hello rozevírací nabídky toosee hello seznam dostupných skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="5e847-132">select **Use existing** and click hello drop-down menu toosee hello available list of Resource groups.</span></span>

  <span data-ttu-id="5e847-133">Úplné informace o skupinách prostředků najdete v tématu hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5e847-133">For complete information on Resource groups, see hello [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="5e847-134">Klikněte na tlačítko **umístění** tooselect hello zeměpisnou oblast trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-134">Click **Location** tooselect hello geographic region for hello vault.</span></span> <span data-ttu-id="5e847-135">Tato volba určuje hello zeměpisnou oblast, kde vaše zálohovaná data se odesílají.</span><span class="sxs-lookup"><span data-stu-id="5e847-135">This choice determines hello geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="5e847-136">Hello dolní části hello okno trezoru služeb zotavení, klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5e847-136">At hello bottom of hello Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="5e847-137">Můžete to trvat několik minut, než hello toobe vytvořit trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="5e847-137">It can take several minutes for hello Recovery Services vault toobe created.</span></span> <span data-ttu-id="5e847-138">Sledujte oznámení stavu hello v horním pravém oblasti hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="5e847-138">Monitor hello status notifications in hello upper right-hand area of hello portal.</span></span> <span data-ttu-id="5e847-139">Po vytvoření svůj trezor se zobrazí v seznamu hello trezorů služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="5e847-139">Once your vault is created, it appears in hello list of Recovery Services vaults.</span></span> <span data-ttu-id="5e847-140">Pokud se trezor nezobrazí ani po několika minutách, klikněte na **Obnovit**.</span><span class="sxs-lookup"><span data-stu-id="5e847-140">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Kliknutí na tlačítko Obnovit](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="5e847-142">Jakmile se zobrazí váš trezor hello seznamu trezorů služeb zotavení, jste redundance úložiště připravené tooset hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-142">Once you see your vault in hello list of Recovery Services vaults, you are ready tooset hello storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-hello-vault"></a><span data-ttu-id="5e847-143">Nastavit redundance úložiště pro trezor hello</span><span class="sxs-lookup"><span data-stu-id="5e847-143">Set storage redundancy for hello vault</span></span>
<span data-ttu-id="5e847-144">Při vytváření trezoru služeb zotavení, ujistěte se, že redundance úložiště nakonfigurovaných hello požadovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="5e847-144">When you create a Recovery Services vault, make sure storage redundancy is configured hello way you want.</span></span>

1. <span data-ttu-id="5e847-145">Z hello **trezory služeb zotavení** okně klikněte na tlačítko Nový trezor hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-145">From hello **Recovery Services vaults** blade, click hello new vault.</span></span>

    ![Vyberte nový trezor hello ze seznamu hello trezor služeb zotavení](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="5e847-147">Když vyberete hello trezoru, hello **trezor služeb zotavení** narrows okno a okno nastavení hello (*jehož hello název trezoru hello v horní části hello*) a otevřete okno Podrobnosti trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-147">When you select hello vault, hello **Recovery Services vault** blade narrows, and hello Settings blade (*which has hello name of hello vault at hello top*) and hello vault details blade open.</span></span>

    ![Zobrazení hello konfiguraci úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="5e847-149">V okně Nastavení hello nový trezor, použijte hello svislé snímku tooscroll dolů toohello části Správa a klikněte na tlačítko **infrastruktura zálohování**.</span><span class="sxs-lookup"><span data-stu-id="5e847-149">In hello new vault's Settings blade, use hello vertical slide tooscroll down toohello Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="5e847-150">Otevře se okno infrastruktura zálohování Hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-150">hello Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="5e847-151">V okně infrastruktura zálohování hello, klikněte na tlačítko **konfigurace zálohování** tooopen hello **konfigurace zálohování** okno.</span><span class="sxs-lookup"><span data-stu-id="5e847-151">In hello Backup Infrastructure blade, click **Backup Configuration** tooopen hello **Backup Configuration** blade.</span></span>

    ![Nastavit hello konfiguraci úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="5e847-153">Zvolte hello vhodnou možnost replikace pro svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="5e847-153">Choose hello appropriate storage replication option for your vault.</span></span>

    ![volby konfigurace úložiště](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="5e847-155">Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="5e847-155">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="5e847-156">Pokud používáte Azure jako koncový bod primární úložiště záloh, pokračujte toouse **geograficky redundantní**.</span><span class="sxs-lookup"><span data-stu-id="5e847-156">If you use Azure as a primary backup storage endpoint, continue toouse **Geo-redundant**.</span></span> <span data-ttu-id="5e847-157">Pokud nepoužíváte Azure jako koncový bod primární úložiště záloh, zvolte **místně redundantní**, což snižuje náklady na úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-157">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces hello Azure storage costs.</span></span> <span data-ttu-id="5e847-158">Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště najdete v tomto [přehledu redundance úložiště](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="5e847-158">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="5e847-159">Teď, když jste vytvořili trezor, můžete ho nakonfigurujte pro zálohování stavu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e847-159">Now that you've created a vault, configure it for backing up Windows System State.</span></span>

## <a name="configure-hello-vault"></a><span data-ttu-id="5e847-160">Konfigurace hello trezoru</span><span class="sxs-lookup"><span data-stu-id="5e847-160">Configure hello vault</span></span>
1. <span data-ttu-id="5e847-161">Na hello okno trezoru služeb zotavení (pro hello trezor jste právě vytvořili), v části Začínáme hello, klikněte na tlačítko **zálohování**, pak na hello **Začínáme se zálohováním** vyberte  **Cíle zálohování**.</span><span class="sxs-lookup"><span data-stu-id="5e847-161">On hello Recovery Services vault blade (for hello vault you just created), in hello Getting Started section, click **Backup**, then on hello **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="5e847-163">Hello **cíl zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="5e847-163">hello **Backup Goal** blade opens.</span></span>

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="5e847-165">Z hello **kde běží vaše úloha?** rozevírací nabídky vyberte **místní**.</span><span class="sxs-lookup"><span data-stu-id="5e847-165">From hello **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="5e847-166">Možnost **Místní** jste vybrali proto, že počítačem s Windows Serverem nebo Windows je fyzický počítač, který není v Azure.</span><span class="sxs-lookup"><span data-stu-id="5e847-166">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="5e847-167">Z hello **co chcete toobackup?** nabídce vyberte možnost **stav systému**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e847-167">From hello **What do you want toobackup?** menu, select **System State**, and click **OK**.</span></span>

    ![Konfigurace souborů a složek](./media/backup-azure-system-state/backup-goal-system-state.png)

    <span data-ttu-id="5e847-169">Po kliknutí na tlačítko OK, objeví se zaškrtnutí další příliš**cíl zálohování**a hello **připravit infrastrukturu** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="5e847-169">After clicking OK, a checkmark appears next too**Backup goal**, and hello **Prepare infrastructure** blade opens.</span></span>

    ![Cíl zálohování je nakonfigurovaný, teď se připraví infrastruktura](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="5e847-171">Na hello **připravit infrastrukturu** okně klikněte na tlačítko **stáhnout agenta pro Windows Server nebo klienta Windows**.</span><span class="sxs-lookup"><span data-stu-id="5e847-171">On hello **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="5e847-173">Pokud používáte Windows Server nezbytné, zvolte toodownload hello agenta pro Windows Server nezbytné.</span><span class="sxs-lookup"><span data-stu-id="5e847-173">If you are using Windows Server Essential, then choose toodownload hello agent for Windows Server Essential.</span></span> <span data-ttu-id="5e847-174">Místní nabídky vás vyzve k toorun nebo uložit MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="5e847-174">A pop-up menu prompts you toorun or save MARSAgentInstaller.exe.</span></span>

    ![Dialogové okno MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="5e847-176">V místní nabídce hello stahování, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5e847-176">In hello download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="5e847-177">Ve výchozím nastavení, hello **MARSagentinstaller.exe** soubor je uložen tooyour složky se staženými soubory.</span><span class="sxs-lookup"><span data-stu-id="5e847-177">By default, hello **MARSagentinstaller.exe** file is saved tooyour Downloads folder.</span></span> <span data-ttu-id="5e847-178">Po dokončení instalačního programu hello, zobrazí se automaticky otevírané okno s dotazem, pokud má instalační program hello toorun nebo otevřete složku hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-178">When hello installer completes, you will see a pop-up asking if you want toorun hello installer, or open hello folder.</span></span>

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="5e847-180">Tooinstall hello agent ještě nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="5e847-180">You don't need tooinstall hello agent yet.</span></span> <span data-ttu-id="5e847-181">Po stažení hello přihlašovací údaje trezoru můžete nainstalovat agenta hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-181">You can install hello agent after you have downloaded hello vault credentials.</span></span>

6. <span data-ttu-id="5e847-182">Na hello **připravit infrastrukturu** okně klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="5e847-182">On hello **Prepare infrastructure** blade, click **Download**.</span></span>

    ![stažení přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="5e847-184">přihlašovací údaje trezoru Hello tooyour stahování složka pro stahování.</span><span class="sxs-lookup"><span data-stu-id="5e847-184">hello vault credentials download tooyour Downloads folder.</span></span> <span data-ttu-id="5e847-185">Po dokončení stahování přihlašovací údaje trezoru hello zobrazí automaticky otevírané okno s dotazem, pokud mají být tooopen nebo uložit přihlašovací údaje hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-185">After hello vault credentials finish downloading, you see a pop-up asking if you want tooopen or save hello credentials.</span></span> <span data-ttu-id="5e847-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5e847-186">Click **Save**.</span></span> <span data-ttu-id="5e847-187">Pokud omylem kliknete **otevřete**, umožní hello dialog, který se pokusí přihlašovací údaje trezoru hello tooopen, nezdaří.</span><span class="sxs-lookup"><span data-stu-id="5e847-187">If you accidentally click **Open**, let hello dialog that attempts tooopen hello vault credentials, fail.</span></span> <span data-ttu-id="5e847-188">Nelze otevřít přihlašovací údaje trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-188">You cannot open hello vault credentials.</span></span> <span data-ttu-id="5e847-189">Pokračujte dalším krokem toohello.</span><span class="sxs-lookup"><span data-stu-id="5e847-189">Proceed toohello next step.</span></span> <span data-ttu-id="5e847-190">přihlašovací údaje trezoru Hello jsou ve složce pro stahování hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-190">hello vault credentials are in hello Downloads folder.</span></span>   

    ![dokončené stahování přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a><span data-ttu-id="5e847-192">Instalace a registrace agenta hello</span><span class="sxs-lookup"><span data-stu-id="5e847-192">Install and register hello agent</span></span>

> [!NOTE]
> <span data-ttu-id="5e847-193">Povolení zálohování prostřednictvím portálu Azure hello ještě není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5e847-193">Enabling backup through hello Azure portal is not available, yet.</span></span> <span data-ttu-id="5e847-194">Použijte agenta služeb zotavení Microsoft Azure tooback hello zálohu stavu systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="5e847-194">Use hello Microsoft Azure Recovery Services Agent tooback up Windows Server System State.</span></span>
>

1. <span data-ttu-id="5e847-195">Vyhledejte a dvakrát klikněte na hello **MARSagentinstaller.exe** z hello stahování složky (nebo jiného uloženého umístění).</span><span class="sxs-lookup"><span data-stu-id="5e847-195">Locate and double-click hello **MARSagentinstaller.exe** from hello Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="5e847-196">Instalační program Hello poskytuje řadu zprávy jako extrahuje, nainstaluje a zaregistruje hello agenta služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="5e847-196">hello installer provides a series of messages as it extracts, installs, and registers hello Recovery Services agent.</span></span>

    ![spuštění přihlašovacích údajů instalačního programu agenta Recovery Services](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="5e847-198">Hello dokončení Průvodce instalací agenta služeb zotavení Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5e847-198">Complete hello Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="5e847-199">toocomplete hello průvodce, budete muset:</span><span class="sxs-lookup"><span data-stu-id="5e847-199">toocomplete hello wizard, you need to:</span></span>

   * <span data-ttu-id="5e847-200">Vyberte umístění složky pro instalaci a mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-200">Choose a location for hello installation and cache folder.</span></span>
   * <span data-ttu-id="5e847-201">Pokud používáte proxy serveru tooconnect toohello zadejte informace o serveru vašeho proxy Internetu.</span><span class="sxs-lookup"><span data-stu-id="5e847-201">Provide your proxy server info if you use a proxy server tooconnect toohello internet.</span></span>
   * <span data-ttu-id="5e847-202">Zadat svoje uživatelské jméno a heslo, pokud používáte ověřený server proxy.</span><span class="sxs-lookup"><span data-stu-id="5e847-202">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="5e847-203">Zadejte přihlašovací údaje trezoru stáhnout hello</span><span class="sxs-lookup"><span data-stu-id="5e847-203">Provide hello downloaded vault credentials</span></span>
   * <span data-ttu-id="5e847-204">Šifrovací přístupové heslo hello uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="5e847-204">Save hello encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="5e847-205">Pokud ztratíte nebo zapomenete heslo hello, Microsoft vám nemůže pomoci obnovit zálohovaná data hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-205">If you lose or forget hello passphrase, Microsoft cannot help recover hello backup data.</span></span> <span data-ttu-id="5e847-206">Hello soubor uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="5e847-206">Save hello file in a secure location.</span></span> <span data-ttu-id="5e847-207">Je požadovaná toorestore zálohu.</span><span class="sxs-lookup"><span data-stu-id="5e847-207">It is required toorestore a backup.</span></span>
     >
     >

<span data-ttu-id="5e847-208">Hello agent je nyní nainstalovaný a váš počítač je registrovaný toohello trezoru.</span><span class="sxs-lookup"><span data-stu-id="5e847-208">hello agent is now installed and your machine is registered toohello vault.</span></span> <span data-ttu-id="5e847-209">Máte připravené tooconfigure a naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="5e847-209">You're ready tooconfigure and schedule your backup.</span></span>

## <a name="back-up-windows-server-system-state-preview"></a><span data-ttu-id="5e847-210">Zálohování stavu systému Windows Server (Preview)</span><span class="sxs-lookup"><span data-stu-id="5e847-210">Back up Windows Server System State (Preview)</span></span>
<span data-ttu-id="5e847-211">Hello prvotní záloha zahrnuje tři úkoly:</span><span class="sxs-lookup"><span data-stu-id="5e847-211">hello initial backup includes three tasks:</span></span>

* <span data-ttu-id="5e847-212">Povolit zálohování stavu systému pomocí agenta Azure Backup hello</span><span class="sxs-lookup"><span data-stu-id="5e847-212">Enable System State Backup using hello Azure Backup agent</span></span>
* <span data-ttu-id="5e847-213">Plán zálohování hello</span><span class="sxs-lookup"><span data-stu-id="5e847-213">Schedule hello backup</span></span>
* <span data-ttu-id="5e847-214">Zálohování souborů a složek pro hello poprvé</span><span class="sxs-lookup"><span data-stu-id="5e847-214">Back up files and folders for hello first time</span></span>

<span data-ttu-id="5e847-215">toocomplete hello prvotní zálohování, agenta služeb zotavení Microsoft Azure použijte hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-215">toocomplete hello initial backup, use hello Microsoft Azure Recovery Services agent.</span></span>

### <a name="tooenable-system-state-backup-using-hello-azure-backup-agent"></a><span data-ttu-id="5e847-216">zálohování stavu systému tooenable pomocí agenta Azure Backup hello</span><span class="sxs-lookup"><span data-stu-id="5e847-216">tooenable System State backup using hello Azure Backup agent</span></span>

1. <span data-ttu-id="5e847-217">V relaci prostředí PowerShell spusťte následující příkaz toostop hello Azure Backup modul hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-217">In a PowerShell session, run hello following command toostop hello Azure Backup engine.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="5e847-218">Otevřete hello registru systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e847-218">Open hello Windows Registry.</span></span>

  ```
  PS C:\> regedit.exe
  ```

3. <span data-ttu-id="5e847-219">Přidejte hodnotu DWORD s hodnotou zadanou hello následující klíč registru s hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-219">Add hello following registry key with hello specified DWord Value.</span></span>

  | <span data-ttu-id="5e847-220">Cesta k registru</span><span class="sxs-lookup"><span data-stu-id="5e847-220">Registry path</span></span> | <span data-ttu-id="5e847-221">Klíč registru</span><span class="sxs-lookup"><span data-stu-id="5e847-221">Registry key</span></span> | <span data-ttu-id="5e847-222">Přidejte hodnotu DWord</span><span class="sxs-lookup"><span data-stu-id="5e847-222">DWord value</span></span> |
  |---------------|--------------|-------------|
  | <span data-ttu-id="5e847-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="5e847-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="5e847-224">TurnOffSSBFeature</span><span class="sxs-lookup"><span data-stu-id="5e847-224">TurnOffSSBFeature</span></span> | <span data-ttu-id="5e847-225">2</span><span class="sxs-lookup"><span data-stu-id="5e847-225">2</span></span> |

4. <span data-ttu-id="5e847-226">Restartujte hello modul Backup spuštěním hello následující příkaz v příkazovém řádku se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="5e847-226">Restart hello Backup engine by executing hello following command in an elevated command prompt.</span></span>

  ```
  PS C:\> Net start obengine
  ```

### <a name="tooschedule-hello-backup-job"></a><span data-ttu-id="5e847-227">Úloha zálohování tooschedule hello</span><span class="sxs-lookup"><span data-stu-id="5e847-227">tooschedule hello backup job</span></span>

1. <span data-ttu-id="5e847-228">Otevřete agenta služeb zotavení Microsoft Azure hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-228">Open hello Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="5e847-229">Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.</span><span class="sxs-lookup"><span data-stu-id="5e847-229">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Spuštění agenta služeb zotavení Azure hello](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. <span data-ttu-id="5e847-231">V hello agenta služeb zotavení, klikněte na **plánem zálohování**.</span><span class="sxs-lookup"><span data-stu-id="5e847-231">In hello Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Naplánování zálohování Windows Serveru](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. <span data-ttu-id="5e847-233">Na hello Začínáme stránku hello Průvodce plánem zálohování, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="5e847-233">On hello Getting started page of hello Schedule Backup Wizard, click **Next**.</span></span>

4. <span data-ttu-id="5e847-234">Na stránce tooBackup hello vyberte položky, klikněte na **přidat položky**.</span><span class="sxs-lookup"><span data-stu-id="5e847-234">On hello Select Items tooBackup page, click **Add Items**.</span></span>

5. <span data-ttu-id="5e847-235">Vyberte **stav systému** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e847-235">Select **System State** and then click **OK**.</span></span>

6. <span data-ttu-id="5e847-236">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5e847-236">Click **Next**.</span></span>

7. <span data-ttu-id="5e847-237">Hello plán zálohování stavu systému a jejich uchovávání bude automaticky nastavena tooback až každých neděle ve 21:00:00 místního času, a je nastavit dobu uchování hello too60 dnů.</span><span class="sxs-lookup"><span data-stu-id="5e847-237">hello System State Backup and Retention schedule is automatically set tooback up every Sunday at 9:00 PM local time, and hello retention period is set too60 days.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5e847-238">Zálohování a uchovávání zásad stavu systému je automaticky nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="5e847-238">System State backup and retention policy is automatically configured.</span></span> <span data-ttu-id="5e847-239">Pokud zálohujete soubory a složky kromě stav systému serveru systému Windows toohello, zadejte pouze hello zálohování a uchovávání zásady pro soubor zálohy z Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-239">If you back up Files and Folders in addition toohello Windows Server System State, specify only hello Backup and Retention policy for file backups from hello wizard.</span></span> 
   >

8. <span data-ttu-id="5e847-240">Na stránce pro potvrzení hello, zkontrolujte hello informace a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="5e847-240">On hello Confirmation page, review hello information, and then click **Finish**.</span></span>

9. <span data-ttu-id="5e847-241">Po dokončení Průvodce hello vytváření hello plán zálohování, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="5e847-241">After hello wizard finishes creating hello backup schedule, click **Close**.</span></span>

### <a name="tooback-up-windows-server-system-state-for-hello-first-time"></a><span data-ttu-id="5e847-242">tooback zálohu stavu systému Windows Server pro hello poprvé</span><span class="sxs-lookup"><span data-stu-id="5e847-242">tooback up Windows Server System State for hello first time</span></span>

1. <span data-ttu-id="5e847-243">Ujistěte se, že neexistují žádné čekající aktualizace pro systém Windows Server, které vyžadují restart počítače.</span><span class="sxs-lookup"><span data-stu-id="5e847-243">Make sure there are no pending updates for Windows Server that require a reboot.</span></span>

2. <span data-ttu-id="5e847-244">V hello agenta služeb zotavení, klikněte na **zálohovat nyní** toocomplete hello počáteční synchronizace replik indexů přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-244">In hello Recovery Services agent, click **Back Up Now** toocomplete hello initial seeding over hello network.</span></span>

    ![Zálohovat nyní ve Windows Serveru](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. <span data-ttu-id="5e847-246">Na stránce pro potvrzení hello zkontrolujte hello nastavení, která hello zpět si teď Průvodce použije tooback hello počítač.</span><span class="sxs-lookup"><span data-stu-id="5e847-246">On hello Confirmation page, review hello settings that hello Back Up Now Wizard will use tooback up hello machine.</span></span> <span data-ttu-id="5e847-247">Poté klikněte na **Zálohovat**.</span><span class="sxs-lookup"><span data-stu-id="5e847-247">Then click **Back Up**.</span></span>

4. <span data-ttu-id="5e847-248">Klikněte na tlačítko **Zavřít** tooclose hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="5e847-248">Click **Close** tooclose hello wizard.</span></span> <span data-ttu-id="5e847-249">Pokud hello průvodce zavřete před dokončením procesu zálohování hello, bude pokračovat hello Průvodce toorun hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="5e847-249">If you close hello wizard before hello backup process finishes, hello wizard continues toorun in hello background.</span></span>

5. <span data-ttu-id="5e847-250">Pokud zálohujete soubory a složky na serveru, kromě stav systému serveru systému Windows toohello, Průvodce zálohování nyní hello pouze zálohovat soubory.</span><span class="sxs-lookup"><span data-stu-id="5e847-250">If you back up Files and Folders on your server, in addition toohello Windows Server System State, hello Backup Now wizard will only back up files.</span></span> <span data-ttu-id="5e847-251">tooperform ad hoc stav systému zálohovat, použijte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="5e847-251">tooperform an ad hoc System State back up, use hello following PowerShell command:</span></span>

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  <span data-ttu-id="5e847-252">Po dokončení prvotní zálohování hello hello **úloha byla dokončena** stav se zobrazí v konzole zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-252">After hello initial backup is completed, hello **Job completed** status appears in hello Backup console.</span></span>

  ![Dokončení IR](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="5e847-254">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="5e847-254">Frequently asked questions</span></span>

<span data-ttu-id="5e847-255">Hello následující otázky a odpovědi poskytovat doplňující informace.</span><span class="sxs-lookup"><span data-stu-id="5e847-255">hello following questions and answers provide supplementary information.</span></span>

### <a name="what-is-hello-staging-volume"></a><span data-ttu-id="5e847-256">Co je hello pracovní svazku?</span><span class="sxs-lookup"><span data-stu-id="5e847-256">What is hello Staging Volume?</span></span>

<span data-ttu-id="5e847-257">Hello pracovní svazku představuje hello zprostředkující umístění, kde hello nativně dostupné, Windows Server Backup zpracuje hello zálohování stavu systému.</span><span class="sxs-lookup"><span data-stu-id="5e847-257">hello Staging Volume represents hello intermediate location where hello natively available, Windows Server Backup stages hello System State Backup.</span></span> <span data-ttu-id="5e847-258">Azure Backup agent pak komprimuje a šifruje zprostředkující záloha a odešle, že prostřednictvím zabezpečeného toohello protokol HTTPS nakonfigurovaná trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="5e847-258">Azure Backup agent then compresses and encrypts this intermediate backup and sends it via secure HTTPS Protocol toohello configured Recovery Services Vault.</span></span> <span data-ttu-id="5e847-259">**Důrazně doporučujeme že vytvořit hello pracovní svazek na svazku bez operačního systému Windows. Pokud zjistíte problémy s zálohování stavu systému, kontrola hello umístění svazku pracovní je prvním krokem řešení potíží hello.**</span><span class="sxs-lookup"><span data-stu-id="5e847-259">**We strongly recommended you establish hello Staging Volume in a non-Windows-OS volume. If you observe problems with System State Backups, checking hello location of your Staging Volume is hello first troubleshooting step.**</span></span> 

### <a name="how-can-i-change-hello-staging-volume-path-specified-in-hello-azure-backup-agent"></a><span data-ttu-id="5e847-260">Jak můžete změnit hello cesty ke svazku pracovní zadaný v hello agenta Azure Backup?</span><span class="sxs-lookup"><span data-stu-id="5e847-260">How can I change hello Staging Volume Path specified in hello Azure Backup agent?</span></span>

<span data-ttu-id="5e847-261">Hello pracovní svazku se nachází ve složce mezipaměti hello ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="5e847-261">hello Staging Volume is located in hello cache folder by default.</span></span> 

1. <span data-ttu-id="5e847-262">toochange toto umístění hello použijte následující příkaz (v příkazovém řádku se zvýšenými):</span><span class="sxs-lookup"><span data-stu-id="5e847-262">toochange this location, use hello following command (in an elevated command prompt):</span></span>
  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="5e847-263">Aktualizujte hello následující položky registru s hello cesta toohello nový svazek pracovní složky.</span><span class="sxs-lookup"><span data-stu-id="5e847-263">Then update hello following registry entries with hello path toohello new Staging Volume folder.</span></span>

  |<span data-ttu-id="5e847-264">Cesta k registru</span><span class="sxs-lookup"><span data-stu-id="5e847-264">Registry path</span></span>|<span data-ttu-id="5e847-265">Klíč registru</span><span class="sxs-lookup"><span data-stu-id="5e847-265">Registry key</span></span>|<span data-ttu-id="5e847-266">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5e847-266">Value</span></span>|
  |-------------|------------|-----|
  |<span data-ttu-id="5e847-267">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="5e847-267">HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="5e847-268">SSBStagingPath</span><span class="sxs-lookup"><span data-stu-id="5e847-268">SSBStagingPath</span></span> | <span data-ttu-id="5e847-269">nový pracovní umístění svazku</span><span class="sxs-lookup"><span data-stu-id="5e847-269">new staging volume location</span></span> |

<span data-ttu-id="5e847-270">Hello cesta pracovní velká a malá písmena a musí být hello přesně stejnou velká a malá písmena jako co existuje v serveru pro hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-270">hello Staging Path is case sensitive and must be hello exact same casing as what exists on hello server.</span></span> 

3. <span data-ttu-id="5e847-271">Jakmile změníte cestu svazku pracovní hello, restartujte modul Backup hello:</span><span class="sxs-lookup"><span data-stu-id="5e847-271">Once you change hello Staging volume path, restart hello Backup engine:</span></span>
  ```
  PS C:\> Net start obengine
  ```
4. <span data-ttu-id="5e847-272">toopick hello změnit cestu, otevřete hello agenta služeb zotavení Microsoft Azure a aktivační události ad hoc zálohu stavu systému.</span><span class="sxs-lookup"><span data-stu-id="5e847-272">toopick up hello changed path, open hello Microsoft Azure Recovery Services agent and trigger an ad hoc backup of System State.</span></span>

### <a name="why-is-hello-system-state-default-retention-set-too60-days"></a><span data-ttu-id="5e847-273">Proč je stav systému hello výchozí uchování nastavit dny too60?</span><span class="sxs-lookup"><span data-stu-id="5e847-273">Why is hello System State default retention set too60 days?</span></span>

<span data-ttu-id="5e847-274">Hello životnosti zálohu stavu systému je hello stejné jako nastavení hello "životnosti objektů označených jako neplatné" pro roli systému Windows Server Active Directory hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-274">hello useful life of a system state backup is hello same as hello "tombstone lifetime" setting for hello Windows Server Active Directory role.</span></span> <span data-ttu-id="5e847-275">Hello výchozí hodnota pro položku životnosti objektů označených jako neplatné hello je 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="5e847-275">hello default value for hello tombstone lifetime entry is 60 days.</span></span> <span data-ttu-id="5e847-276">Tuto hodnotu lze nastavit u objektu konfigurace služby Directory (NTDS) hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-276">This value can be set on hello Directory Service (NTDS) config object.</span></span>

### <a name="how-do-i-change-hello-default-backup-and-retention-policy-for-system-state"></a><span data-ttu-id="5e847-277">Jak změnit výchozí hello zálohování a zásadami uchovávání informací pro stav systému?</span><span class="sxs-lookup"><span data-stu-id="5e847-277">How do I change hello default Backup and Retention Policy for System State?</span></span>

<span data-ttu-id="5e847-278">toochange hello výchozí zálohování a zásadami uchovávání informací pro stav systému:</span><span class="sxs-lookup"><span data-stu-id="5e847-278">toochange hello default Backup and Retention Policy for System State:</span></span>
1. <span data-ttu-id="5e847-279">Zastavte modul zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-279">Stop hello Backup engine.</span></span> <span data-ttu-id="5e847-280">Spusťte následující příkaz z příkazového řádku se zvýšenými oprávněními hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-280">Run hello following command from an elevated command prompt.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="5e847-281">Přidat nebo aktualizovat hello následující položky klíče registru v HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.</span><span class="sxs-lookup"><span data-stu-id="5e847-281">Add or update hello following registry key entries in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.</span></span>

  |<span data-ttu-id="5e847-282">Název registru</span><span class="sxs-lookup"><span data-stu-id="5e847-282">Registry Name</span></span>|<span data-ttu-id="5e847-283">Popis</span><span class="sxs-lookup"><span data-stu-id="5e847-283">Description</span></span>|<span data-ttu-id="5e847-284">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5e847-284">Value</span></span>|
  |-------------|-----------|-----|
  |<span data-ttu-id="5e847-285">SSBScheduleTime</span><span class="sxs-lookup"><span data-stu-id="5e847-285">SSBScheduleTime</span></span>|<span data-ttu-id="5e847-286">Použít tooconfigure hello čas hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="5e847-286">Used tooconfigure hello time of hello backup.</span></span> <span data-ttu-id="5e847-287">Výchozí hodnota je 21: 00 místního času.</span><span class="sxs-lookup"><span data-stu-id="5e847-287">Default is 9PM local time.</span></span>|<span data-ttu-id="5e847-288">DWord: Formátu hh: mm (decimální): pro příklad. 2130 21:30:00 místního času</span><span class="sxs-lookup"><span data-stu-id="5e847-288">DWord: Format HHMM (Decimal) for example 2130 for 9:30PM local time</span></span>|
  |<span data-ttu-id="5e847-289">SSBScheduleDays</span><span class="sxs-lookup"><span data-stu-id="5e847-289">SSBScheduleDays</span></span>|<span data-ttu-id="5e847-290">Použít tooconfigure hello dní při zálohování stavu systému, musí se provést na hello zadat čas.</span><span class="sxs-lookup"><span data-stu-id="5e847-290">Used tooconfigure hello days when System State Backup must be performed at hello specified time.</span></span> <span data-ttu-id="5e847-291">Jednotlivé číslic zadejte počet dnů v týdnu hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-291">Individual digits specify days of hello week.</span></span> <span data-ttu-id="5e847-292">představuje neděli 0, 1 je pondělí, a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5e847-292">0 represents Sunday, 1 is Monday, and so on.</span></span> <span data-ttu-id="5e847-293">Výchozí den pro zálohování je neděli.</span><span class="sxs-lookup"><span data-stu-id="5e847-293">Default day for backup is Sunday.</span></span>|<span data-ttu-id="5e847-294">DWord: počet dnů hello týden toorun zálohování (decimální): Příklad 1 230 plány zálohování v pondělí, úterý, středu a neděli.</span><span class="sxs-lookup"><span data-stu-id="5e847-294">DWord: days of hello week toorun backup (decimal) for example 1230 schedules backups on Monday, Tuesday, Wednesday, and Sunday.</span></span>|
  |<span data-ttu-id="5e847-295">SSBRetentionDays</span><span class="sxs-lookup"><span data-stu-id="5e847-295">SSBRetentionDays</span></span>|<span data-ttu-id="5e847-296">Použít tooconfigure hello dnů tooretain zálohování.</span><span class="sxs-lookup"><span data-stu-id="5e847-296">Used tooconfigure hello days tooretain backup.</span></span> <span data-ttu-id="5e847-297">Výchozí hodnota je 60.</span><span class="sxs-lookup"><span data-stu-id="5e847-297">Default value is 60.</span></span> <span data-ttu-id="5e847-298">Maximální povolená hodnota je 180.</span><span class="sxs-lookup"><span data-stu-id="5e847-298">Maximum allowed value is 180.</span></span>|<span data-ttu-id="5e847-299">DWord: Zálohování tooretain v dní (decimal).</span><span class="sxs-lookup"><span data-stu-id="5e847-299">DWord: Days tooretain backup (decimal).</span></span>|

3. <span data-ttu-id="5e847-300">Použijte následující příkaz toorestart hello modulu zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-300">Use hello following command toorestart hello backup engine.</span></span>
    ```
    PS C:\> Net start obengine
    ```

4. <span data-ttu-id="5e847-301">Otevřete agenta služeb zotavení Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="5e847-301">Open hello Microsoft Recovery Services agent.</span></span>

5. <span data-ttu-id="5e847-302">Klikněte na tlačítko **plánem zálohování** a pak klikněte na **Další** dokud neuvidíte hello změny projeví.</span><span class="sxs-lookup"><span data-stu-id="5e847-302">Click **Schedule Backup** and then click **Next** until you see hello changes reflected.</span></span>

6. <span data-ttu-id="5e847-303">Klikněte na tlačítko **Dokončit** tooapply hello změny.</span><span class="sxs-lookup"><span data-stu-id="5e847-303">Click **Finish** tooapply hello changes.</span></span>


## <a name="questions"></a><span data-ttu-id="5e847-304">Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="5e847-304">Questions?</span></span>
<span data-ttu-id="5e847-305">Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="5e847-305">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e847-306">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e847-306">Next steps</span></span>
* <span data-ttu-id="5e847-307">Zdroj dalších informací o [zálohování počítačů se systémem Windows](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="5e847-307">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="5e847-308">Teď, když jste zálohovali své soubory a složky, můžete [spravovat svoje trezory a servery](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="5e847-308">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="5e847-309">Pokud potřebujete toorestore zálohu, použijte tento článek příliš[obnovit soubory tooa Windows počítač](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="5e847-309">If you need toorestore a backup, use this article too[restore files tooa Windows machine](backup-azure-restore-windows-server.md).</span></span>
