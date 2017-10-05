---
title: "Zálohování stavu systému Windows Azure | Microsoft Docs"
description: "Postup zálohování stavu systému Windows Server nebo Windows počítačů do Azure."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "postup zálohování; zálohování souborů a složek"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: 6fbd96935f444d8b0c6d068ebd0d28e612f19816
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a><span data-ttu-id="f2976-104">Zálohování stavu systému Windows v nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f2976-104">Back up Windows system state in Resource Manager deployment</span></span>
<span data-ttu-id="f2976-105">Tento článek vysvětluje, jak zálohovat stav systému Windows Server do Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-105">This article explains how to back up your Windows Server system state to Azure.</span></span> <span data-ttu-id="f2976-106">Tento kurz vás má provést základy.</span><span class="sxs-lookup"><span data-stu-id="f2976-106">It's a tutorial intended to walk you through the basics.</span></span>

<span data-ttu-id="f2976-107">Chcete-li se dozvědět více o Azure Backup, přečtěte si tento [přehled](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="f2976-107">If you want to know more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="f2976-108">Pokud předplatné Azure nemáte, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/), který vám umožní přístup ke službám Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="f2976-109">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="f2976-109">Create a recovery services vault</span></span>
<span data-ttu-id="f2976-110">Chcete-li zálohovat svoje soubory a složky, musíte vytvořit trezor Služeb zotavení v oblasti, kde chcete data ukládat.</span><span class="sxs-lookup"><span data-stu-id="f2976-110">To back up your files and folders, you need to create a Recovery Services vault in the region where you want to store the data.</span></span> <span data-ttu-id="f2976-111">Musíte také určit způsob replikace úložiště.</span><span class="sxs-lookup"><span data-stu-id="f2976-111">You also need to determine how you want your storage replicated.</span></span>

### <a name="to-create-a-recovery-services-vault"></a><span data-ttu-id="f2976-112">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="f2976-112">To create a Recovery Services vault</span></span>
1. <span data-ttu-id="f2976-113">Pokud jste to ještě neudělali, přihlaste se k [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-113">If you haven't already done so, sign in to the [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="f2976-114">V nabídce centra klikněte na **Procházet**, v seznamu prostředků zadejte **Recovery Services** a klikněte na **Trezory služby Recovery Services**.</span><span class="sxs-lookup"><span data-stu-id="f2976-114">On the Hub menu, click **More services** and in the list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="f2976-116">Pokud předplatné obsahuje trezory služby Recovery Services, jsou tyto trezory uvedené v seznamu.</span><span class="sxs-lookup"><span data-stu-id="f2976-116">If there are recovery services vaults in the subscription, the vaults are listed.</span></span>
3. <span data-ttu-id="f2976-117">V nabídce **Trezory Recovery Services** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f2976-117">On the **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="f2976-119">Otevře se okno trezoru Recovery Services s výzvou k vyplnění polí **Název**, **Předplatné**, **Skupina prostředků** a **Oblast**.</span><span class="sxs-lookup"><span data-stu-id="f2976-119">The Recovery Services vault blade opens, prompting you to provide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="f2976-121">Jako **Název** zadejte popisný název pro identifikaci trezoru.</span><span class="sxs-lookup"><span data-stu-id="f2976-121">For **Name**, enter a friendly name to identify the vault.</span></span> <span data-ttu-id="f2976-122">Název musí být jedinečný v rámci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-122">The name needs to be unique for the Azure subscription.</span></span> <span data-ttu-id="f2976-123">Zadejte název v rozsahu 2 až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="f2976-123">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="f2976-124">Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="f2976-124">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="f2976-125">V části **Předplatné** z rozevírací nabídky vyberte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-125">In the **Subscription** section, use the drop-down menu to choose the Azure subscription.</span></span> <span data-ttu-id="f2976-126">Pokud používáte jenom jedno předplatné, zobrazí se toto předplatné a můžete přejít k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="f2976-126">If you use only one subscription, that subscription appears and you can skip to the next step.</span></span> <span data-ttu-id="f2976-127">Pokud si nejste jisti, jaké předplatné použít, použijte výchozí (nebo navrhované) předplatné.</span><span class="sxs-lookup"><span data-stu-id="f2976-127">If you are not sure which subscription to use, use the default (or suggested) subscription.</span></span> <span data-ttu-id="f2976-128">Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-128">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="f2976-129">V části **Skupina prostředků**:</span><span class="sxs-lookup"><span data-stu-id="f2976-129">In the **Resource group** section:</span></span>

    * <span data-ttu-id="f2976-130">vyberte **Vytvořit novou**, pokud chcete vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f2976-130">select **Create new** if you want to create a Resource group.</span></span>
    <span data-ttu-id="f2976-131">Nebo</span><span class="sxs-lookup"><span data-stu-id="f2976-131">Or</span></span>
    * <span data-ttu-id="f2976-132">vyberte **Použít existující** a kliknutím na rozevírací nabídku zobrazte seznam dostupných skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="f2976-132">select **Use existing** and click the drop-down menu to see the available list of Resource groups.</span></span>

  <span data-ttu-id="f2976-133">Kompletní informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f2976-133">For complete information on Resource groups, see the [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="f2976-134">Klikněte na **Oblast** a vyberte zeměpisnou oblast trezoru.</span><span class="sxs-lookup"><span data-stu-id="f2976-134">Click **Location** to select the geographic region for the vault.</span></span> <span data-ttu-id="f2976-135">Tato volba určuje geografickou oblast, kam jsou zasílaná vaše zálohovaná data.</span><span class="sxs-lookup"><span data-stu-id="f2976-135">This choice determines the geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="f2976-136">V dolní části okna trezoru služby Recovery Services klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f2976-136">At the bottom of the Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="f2976-137">Vytvoření trezoru služby Recovery Services může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="f2976-137">It can take several minutes for the Recovery Services vault to be created.</span></span> <span data-ttu-id="f2976-138">Sledujte oznámení o stavu v pravé horní části portálu.</span><span class="sxs-lookup"><span data-stu-id="f2976-138">Monitor the status notifications in the upper right-hand area of the portal.</span></span> <span data-ttu-id="f2976-139">Když je trezor vytvořený, zobrazí se v seznamu trezorů Služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="f2976-139">Once your vault is created, it appears in the list of Recovery Services vaults.</span></span> <span data-ttu-id="f2976-140">Pokud se trezor nezobrazí ani po několika minutách, klikněte na **Obnovit**.</span><span class="sxs-lookup"><span data-stu-id="f2976-140">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Kliknutí na tlačítko Obnovit](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="f2976-142">Jakmile se trezor zobrazí v seznamu trezorů služby Recovery Services, jste připraveni nastavit redundanci úložiště.</span><span class="sxs-lookup"><span data-stu-id="f2976-142">Once you see your vault in the list of Recovery Services vaults, you are ready to set the storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-the-vault"></a><span data-ttu-id="f2976-143">Nastavení redundance úložiště pro trezor</span><span class="sxs-lookup"><span data-stu-id="f2976-143">Set storage redundancy for the vault</span></span>
<span data-ttu-id="f2976-144">Při vytváření trezoru služby Recovery Services se ujistěte, že je redundance úložiště nakonfigurována požadovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="f2976-144">When you create a Recovery Services vault, make sure storage redundancy is configured the way you want.</span></span>

1. <span data-ttu-id="f2976-145">V okně **Trezory služby Recovery Services** klikněte na nový trezor.</span><span class="sxs-lookup"><span data-stu-id="f2976-145">From the **Recovery Services vaults** blade, click the new vault.</span></span>

    ![Výběr nového trezoru ze seznamu trezorů služby Recovery Services](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="f2976-147">Po výběru trezoru se okno **Trezory služby Recovery Services** zúží a otevře se okno Nastavení (*s názvem trezoru v horní části*) a okno s podrobnostmi o trezoru.</span><span class="sxs-lookup"><span data-stu-id="f2976-147">When you select the vault, the **Recovery Services vault** blade narrows, and the Settings blade (*which has the name of the vault at the top*) and the vault details blade open.</span></span>

    ![Zobrazení konfigurace úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="f2976-149">V okně Nastavení nového trezoru pomocí vertikálního posuvníku přejděte dolů do části Správa a klikněte na **Infrastruktura zálohování**.</span><span class="sxs-lookup"><span data-stu-id="f2976-149">In the new vault's Settings blade, use the vertical slide to scroll down to the Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="f2976-150">Otevře se okno Infrastruktura zálohování.</span><span class="sxs-lookup"><span data-stu-id="f2976-150">The Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="f2976-151">V okně Infrastruktura zálohování klikněte na **Konfigurace zálohování**. Otevře se okno **Konfigurace zálohování**.</span><span class="sxs-lookup"><span data-stu-id="f2976-151">In the Backup Infrastructure blade, click **Backup Configuration** to open the **Backup Configuration** blade.</span></span>

    ![Nastavení konfigurace úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="f2976-153">Zvolte vhodnou možnost replikace pro svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="f2976-153">Choose the appropriate storage replication option for your vault.</span></span>

    ![volby konfigurace úložiště](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="f2976-155">Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="f2976-155">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="f2976-156">Pokud používáte Azure jako primární koncový bod úložiště záloh, pokračujte v používání **geograficky redundantního** úložiště.</span><span class="sxs-lookup"><span data-stu-id="f2976-156">If you use Azure as a primary backup storage endpoint, continue to use **Geo-redundant**.</span></span> <span data-ttu-id="f2976-157">Pokud Azure nepoužíváte jako primární koncový bod úložiště záloh, vyberte **Místně redundantní** – snížíte tím náklady na úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-157">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces the Azure storage costs.</span></span> <span data-ttu-id="f2976-158">Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště najdete v tomto [přehledu redundance úložiště](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="f2976-158">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="f2976-159">Teď, když jste vytvořili trezor, můžete ho nakonfigurujte pro zálohování stavu systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f2976-159">Now that you've created a vault, configure it for backing up Windows System State.</span></span>

## <a name="configure-the-vault"></a><span data-ttu-id="f2976-160">Konfigurace trezoru</span><span class="sxs-lookup"><span data-stu-id="f2976-160">Configure the vault</span></span>
1. <span data-ttu-id="f2976-161">V okně trezoru služby Recovery Services (pro trezor, který jste právě vytvořili) klikněte v části Začínáme na **Zálohovat** a potom v okně **Začínáme se zálohováním** vyberte **Cíl zálohování**.</span><span class="sxs-lookup"><span data-stu-id="f2976-161">On the Recovery Services vault blade (for the vault you just created), in the Getting Started section, click **Backup**, then on the **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="f2976-163">Otevře se okno **Cíl zálohování**.</span><span class="sxs-lookup"><span data-stu-id="f2976-163">The **Backup Goal** blade opens.</span></span>

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="f2976-165">V rozevírací nabídce **Kde běží vaše úlohy?** vyberte **Místní**.</span><span class="sxs-lookup"><span data-stu-id="f2976-165">From the **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="f2976-166">Možnost **Místní** jste vybrali proto, že počítačem s Windows Serverem nebo Windows je fyzický počítač, který není v Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-166">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="f2976-167">Z **co chcete zálohovat?** nabídce vyberte možnost **stav systému**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2976-167">From the **What do you want to backup?** menu, select **System State**, and click **OK**.</span></span>

    ![Konfigurace souborů a složek](./media/backup-azure-system-state/backup-goal-system-state.png)

    <span data-ttu-id="f2976-169">Po kliknutí na OK se vedle položky **Cíl zálohování** zobrazí zaškrtnutí a otevře se okno **Připravit infrastrukturu**.</span><span class="sxs-lookup"><span data-stu-id="f2976-169">After clicking OK, a checkmark appears next to **Backup goal**, and the **Prepare infrastructure** blade opens.</span></span>

    ![Cíl zálohování je nakonfigurovaný, teď se připraví infrastruktura](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="f2976-171">V okně **Připravit infrastrukturu** klikněte na **Stáhnout agenta pro Windows Server nebo klienta Windows**.</span><span class="sxs-lookup"><span data-stu-id="f2976-171">On the **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="f2976-173">Pokud používáte Windows Server Essential, vyberte ke stažení agenta pro Windows Server Essential.</span><span class="sxs-lookup"><span data-stu-id="f2976-173">If you are using Windows Server Essential, then choose to download the agent for Windows Server Essential.</span></span> <span data-ttu-id="f2976-174">Místní nabídka zobrazí výzvu ke spuštění nebo uložení MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="f2976-174">A pop-up menu prompts you to run or save MARSAgentInstaller.exe.</span></span>

    ![Dialogové okno MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="f2976-176">V místní nabídce stahování klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f2976-176">In the download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="f2976-177">Ve výchozím nastavení se soubor **MARSagentinstaller.exe** uloží do složky Stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="f2976-177">By default, the **MARSagentinstaller.exe** file is saved to your Downloads folder.</span></span> <span data-ttu-id="f2976-178">Po dokončení instalačního programu se zobrazí automaticky otevírané okno s dotazem, jestli chcete spustit instalační program nebo otevřít složku.</span><span class="sxs-lookup"><span data-stu-id="f2976-178">When the installer completes, you will see a pop-up asking if you want to run the installer, or open the folder.</span></span>

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="f2976-180">Agenta ještě nemusíte instalovat.</span><span class="sxs-lookup"><span data-stu-id="f2976-180">You don't need to install the agent yet.</span></span> <span data-ttu-id="f2976-181">Agenta můžete nainstalovat po stažení přihlašovacích údajů trezoru.</span><span class="sxs-lookup"><span data-stu-id="f2976-181">You can install the agent after you have downloaded the vault credentials.</span></span>

6. <span data-ttu-id="f2976-182">V okně **Připravit infrastrukturu** klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="f2976-182">On the **Prepare infrastructure** blade, click **Download**.</span></span>

    ![stažení přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="f2976-184">Přihlašovací údaje trezoru se stáhnou do složky Stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="f2976-184">The vault credentials download to your Downloads folder.</span></span> <span data-ttu-id="f2976-185">Po dokončení stahování přihlašovacích údajů trezoru se zobrazí automaticky otevírané okno s dotazem, jestli chcete přihlašovací údaje otevřít nebo uložit.</span><span class="sxs-lookup"><span data-stu-id="f2976-185">After the vault credentials finish downloading, you see a pop-up asking if you want to open or save the credentials.</span></span> <span data-ttu-id="f2976-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f2976-186">Click **Save**.</span></span> <span data-ttu-id="f2976-187">Pokud omylem kliknete **Otevřít**, nechte dialogové okno, které se pokusí otevřít přihlašovací údaje trezoru, zobrazit chybu.</span><span class="sxs-lookup"><span data-stu-id="f2976-187">If you accidentally click **Open**, let the dialog that attempts to open the vault credentials, fail.</span></span> <span data-ttu-id="f2976-188">Přihlašovací údaje trezoru nejde otevřít.</span><span class="sxs-lookup"><span data-stu-id="f2976-188">You cannot open the vault credentials.</span></span> <span data-ttu-id="f2976-189">Přejděte k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="f2976-189">Proceed to the next step.</span></span> <span data-ttu-id="f2976-190">Přihlašovací údaje trezoru jsou ve složce Stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="f2976-190">The vault credentials are in the Downloads folder.</span></span>   

    ![dokončené stahování přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a><span data-ttu-id="f2976-192">Instalace a registrace agenta</span><span class="sxs-lookup"><span data-stu-id="f2976-192">Install and register the agent</span></span>

> [!NOTE]
> <span data-ttu-id="f2976-193">Povolení zálohování prostřednictvím webu Azure Portal ještě není dostupné.</span><span class="sxs-lookup"><span data-stu-id="f2976-193">Enabling backup through the Azure portal is not available, yet.</span></span> <span data-ttu-id="f2976-194">Agent služeb zotavení Microsoft Azure pomocí zálohování stavu systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="f2976-194">Use the Microsoft Azure Recovery Services Agent to back up Windows Server System State.</span></span>
>

1. <span data-ttu-id="f2976-195">Ve složce Stažené soubory (nebo ve složce, kterou jste vybrali pro stahování) vyhledejte soubor **MARSagentinstaller.exe** a dvakrát na něj klikněte.</span><span class="sxs-lookup"><span data-stu-id="f2976-195">Locate and double-click the **MARSagentinstaller.exe** from the Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="f2976-196">Instalační program v průběhu extrakce, instalace a registrace agenta Recovery Services zobrazuje řadu zpráv.</span><span class="sxs-lookup"><span data-stu-id="f2976-196">The installer provides a series of messages as it extracts, installs, and registers the Recovery Services agent.</span></span>

    ![spuštění přihlašovacích údajů instalačního programu agenta Recovery Services](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="f2976-198">Dokončete Průvodce instalací agenta Služeb zotavení Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-198">Complete the Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="f2976-199">K dokončení průvodce budete muset:</span><span class="sxs-lookup"><span data-stu-id="f2976-199">To complete the wizard, you need to:</span></span>

   * <span data-ttu-id="f2976-200">Vybrat umístění instalace a složky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f2976-200">Choose a location for the installation and cache folder.</span></span>
   * <span data-ttu-id="f2976-201">Zadat informace o serveru proxy, pokud pro připojování k internetu používáte server proxy.</span><span class="sxs-lookup"><span data-stu-id="f2976-201">Provide your proxy server info if you use a proxy server to connect to the internet.</span></span>
   * <span data-ttu-id="f2976-202">Zadat svoje uživatelské jméno a heslo, pokud používáte ověřený server proxy.</span><span class="sxs-lookup"><span data-stu-id="f2976-202">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="f2976-203">Zadat stažené přihlašovací údaje trezoru.</span><span class="sxs-lookup"><span data-stu-id="f2976-203">Provide the downloaded vault credentials</span></span>
   * <span data-ttu-id="f2976-204">Uložit šifrovací heslo na bezpečné místo.</span><span class="sxs-lookup"><span data-stu-id="f2976-204">Save the encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="f2976-205">Pokud heslo ztratíte nebo zapomenete, Microsoft vám nemůže pomoci obnovit zálohovaná data.</span><span class="sxs-lookup"><span data-stu-id="f2976-205">If you lose or forget the passphrase, Microsoft cannot help recover the backup data.</span></span> <span data-ttu-id="f2976-206">Uložte soubor na bezpečné místo.</span><span class="sxs-lookup"><span data-stu-id="f2976-206">Save the file in a secure location.</span></span> <span data-ttu-id="f2976-207">Je požadováno pro obnovení zálohy.</span><span class="sxs-lookup"><span data-stu-id="f2976-207">It is required to restore a backup.</span></span>
     >
     >

<span data-ttu-id="f2976-208">Agent je nyní nainstalovaný a váš počítač je registrovaný k trezoru.</span><span class="sxs-lookup"><span data-stu-id="f2976-208">The agent is now installed and your machine is registered to the vault.</span></span> <span data-ttu-id="f2976-209">Jste připraveni nakonfigurovat a naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="f2976-209">You're ready to configure and schedule your backup.</span></span>

## <a name="back-up-windows-server-system-state-preview"></a><span data-ttu-id="f2976-210">Zálohování stavu systému Windows Server (Preview)</span><span class="sxs-lookup"><span data-stu-id="f2976-210">Back up Windows Server System State (Preview)</span></span>
<span data-ttu-id="f2976-211">Prvotní záloha zahrnuje tři úkoly:</span><span class="sxs-lookup"><span data-stu-id="f2976-211">The initial backup includes three tasks:</span></span>

* <span data-ttu-id="f2976-212">Povolit zálohování stavu systému pomocí agenta Azure Backup</span><span class="sxs-lookup"><span data-stu-id="f2976-212">Enable System State Backup using the Azure Backup agent</span></span>
* <span data-ttu-id="f2976-213">Naplánování zálohování</span><span class="sxs-lookup"><span data-stu-id="f2976-213">Schedule the backup</span></span>
* <span data-ttu-id="f2976-214">První zálohování souborů a složek</span><span class="sxs-lookup"><span data-stu-id="f2976-214">Back up files and folders for the first time</span></span>

<span data-ttu-id="f2976-215">K dokončení prvotního zálohování použijte agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="f2976-215">To complete the initial backup, use the Microsoft Azure Recovery Services agent.</span></span>

### <a name="to-enable-system-state-backup-using-the-azure-backup-agent"></a><span data-ttu-id="f2976-216">K povolení zálohování stavu systému pomocí agenta Azure Backup</span><span class="sxs-lookup"><span data-stu-id="f2976-216">To enable System State backup using the Azure Backup agent</span></span>

1. <span data-ttu-id="f2976-217">V relaci prostředí PowerShell pomocí následujícího příkazu zastavte modul zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-217">In a PowerShell session, run the following command to stop the Azure Backup engine.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="f2976-218">Otevřete registr systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f2976-218">Open the Windows Registry.</span></span>

  ```
  PS C:\> regedit.exe
  ```

3. <span data-ttu-id="f2976-219">Přidejte následující klíč registru se zadanou hodnotou DWord.</span><span class="sxs-lookup"><span data-stu-id="f2976-219">Add the following registry key with the specified DWord Value.</span></span>

  | <span data-ttu-id="f2976-220">Cesta k registru</span><span class="sxs-lookup"><span data-stu-id="f2976-220">Registry path</span></span> | <span data-ttu-id="f2976-221">Klíč registru</span><span class="sxs-lookup"><span data-stu-id="f2976-221">Registry key</span></span> | <span data-ttu-id="f2976-222">Přidejte hodnotu DWord</span><span class="sxs-lookup"><span data-stu-id="f2976-222">DWord value</span></span> |
  |---------------|--------------|-------------|
  | <span data-ttu-id="f2976-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="f2976-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="f2976-224">TurnOffSSBFeature</span><span class="sxs-lookup"><span data-stu-id="f2976-224">TurnOffSSBFeature</span></span> | <span data-ttu-id="f2976-225">2</span><span class="sxs-lookup"><span data-stu-id="f2976-225">2</span></span> |

4. <span data-ttu-id="f2976-226">Restartujte modul zálohování spuštěním následujícího příkazu v příkazovém řádku se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="f2976-226">Restart the Backup engine by executing the following command in an elevated command prompt.</span></span>

  ```
  PS C:\> Net start obengine
  ```

### <a name="to-schedule-the-backup-job"></a><span data-ttu-id="f2976-227">Naplánování úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="f2976-227">To schedule the backup job</span></span>

1. <span data-ttu-id="f2976-228">Otevřete agenta Služeb zotavení Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f2976-228">Open the Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="f2976-229">Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.</span><span class="sxs-lookup"><span data-stu-id="f2976-229">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Spuštění agenta Služeb zotavení Azure](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. <span data-ttu-id="f2976-231">V agentu Služeb zotavení klikněte na **Naplánovat zálohování**.</span><span class="sxs-lookup"><span data-stu-id="f2976-231">In the Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Naplánování zálohování Windows Serveru](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. <span data-ttu-id="f2976-233">Na stránce Začínáme v Průvodci plánování zálohování klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f2976-233">On the Getting started page of the Schedule Backup Wizard, click **Next**.</span></span>

4. <span data-ttu-id="f2976-234">Na stránce Výběr položek k zálohování klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f2976-234">On the Select Items to Backup page, click **Add Items**.</span></span>

5. <span data-ttu-id="f2976-235">Vyberte **stav systému** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2976-235">Select **System State** and then click **OK**.</span></span>

6. <span data-ttu-id="f2976-236">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f2976-236">Click **Next**.</span></span>

7. <span data-ttu-id="f2976-237">Plán zálohování stavu systému a jejich uchovávání se automaticky nastaví na zálohování každou neděli ve 21:00:00 místního času a dobu uchování je nastaven na 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="f2976-237">The System State Backup and Retention schedule is automatically set to back up every Sunday at 9:00 PM local time, and the retention period is set to 60 days.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f2976-238">Zálohování a uchovávání zásad stavu systému je automaticky nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="f2976-238">System State backup and retention policy is automatically configured.</span></span> <span data-ttu-id="f2976-239">Pokud zálohujete soubory a složky kromě stavu systému Windows Server, zadejte jenom zálohování a uchovávání zásady pro soubor zálohy z průvodce.</span><span class="sxs-lookup"><span data-stu-id="f2976-239">If you back up Files and Folders in addition to the Windows Server System State, specify only the Backup and Retention policy for file backups from the wizard.</span></span> 
   >

8. <span data-ttu-id="f2976-240">Na stránce Potvrzení zkontrolujte informace a poté klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f2976-240">On the Confirmation page, review the information, and then click **Finish**.</span></span>

9. <span data-ttu-id="f2976-241">Až průvodce dokončí vytváření plánu zálohování, klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="f2976-241">After the wizard finishes creating the backup schedule, click **Close**.</span></span>

### <a name="to-back-up-windows-server-system-state-for-the-first-time"></a><span data-ttu-id="f2976-242">Zálohování stavu systému Windows Server poprvé</span><span class="sxs-lookup"><span data-stu-id="f2976-242">To back up Windows Server System State for the first time</span></span>

1. <span data-ttu-id="f2976-243">Ujistěte se, že neexistují žádné čekající aktualizace pro systém Windows Server, které vyžadují restart počítače.</span><span class="sxs-lookup"><span data-stu-id="f2976-243">Make sure there are no pending updates for Windows Server that require a reboot.</span></span>

2. <span data-ttu-id="f2976-244">Chcete-li dokončit prvotní synchronizaci přes síť, v agentu Služeb zotavení klikněte na **Zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="f2976-244">In the Recovery Services agent, click **Back Up Now** to complete the initial seeding over the network.</span></span>

    ![Zálohovat nyní ve Windows Serveru](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. <span data-ttu-id="f2976-246">Na stránce Potvrzení zkontrolujte nastavení, které Průvodce Zálohování nyní použije k zálohování počítače.</span><span class="sxs-lookup"><span data-stu-id="f2976-246">On the Confirmation page, review the settings that the Back Up Now Wizard will use to back up the machine.</span></span> <span data-ttu-id="f2976-247">Poté klikněte na **Zálohovat**.</span><span class="sxs-lookup"><span data-stu-id="f2976-247">Then click **Back Up**.</span></span>

4. <span data-ttu-id="f2976-248">Průvodce zavřete kliknutím na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="f2976-248">Click **Close** to close the wizard.</span></span> <span data-ttu-id="f2976-249">Pokud průvodce zavřete před dokončením procesu zálohování, průvodce zůstane spuštěný na pozadí.</span><span class="sxs-lookup"><span data-stu-id="f2976-249">If you close the wizard before the backup process finishes, the wizard continues to run in the background.</span></span>

5. <span data-ttu-id="f2976-250">Pokud zálohujete soubory a složky na serveru, kromě stav systému Windows Server, Průvodce zálohování nyní pouze zálohovat soubory.</span><span class="sxs-lookup"><span data-stu-id="f2976-250">If you back up Files and Folders on your server, in addition to the Windows Server System State, the Backup Now wizard will only back up files.</span></span> <span data-ttu-id="f2976-251">K provedení zálohování do ad hoc stavu systému, použijte následující příkaz Powershellu:</span><span class="sxs-lookup"><span data-stu-id="f2976-251">To perform an ad hoc System State back up, use the following PowerShell command:</span></span>

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  <span data-ttu-id="f2976-252">Po dokončení prvotní zálohy se v konzole Zálohování zobrazí stav **Úloha byla dokončena**.</span><span class="sxs-lookup"><span data-stu-id="f2976-252">After the initial backup is completed, the **Job completed** status appears in the Backup console.</span></span>

  ![Dokončení IR](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="f2976-254">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="f2976-254">Frequently asked questions</span></span>

<span data-ttu-id="f2976-255">Následující otázky a odpovědi poskytovat doplňující informace.</span><span class="sxs-lookup"><span data-stu-id="f2976-255">The following questions and answers provide supplementary information.</span></span>

### <a name="what-is-the-staging-volume"></a><span data-ttu-id="f2976-256">Co je pracovní svazek?</span><span class="sxs-lookup"><span data-stu-id="f2976-256">What is the Staging Volume?</span></span>

<span data-ttu-id="f2976-257">Svazek pracovní představuje zprostředkující umístění, kde nativně dostupné, Windows Server Backup zpracuje zálohování stavu systému.</span><span class="sxs-lookup"><span data-stu-id="f2976-257">The Staging Volume represents the intermediate location where the natively available, Windows Server Backup stages the System State Backup.</span></span> <span data-ttu-id="f2976-258">Azure Backup agent pak komprimuje a šifruje zprostředkující záloha a odešle ji přes zabezpečený protokol HTTPS nakonfigurovaná trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="f2976-258">Azure Backup agent then compresses and encrypts this intermediate backup and sends it via secure HTTPS Protocol to the configured Recovery Services Vault.</span></span> <span data-ttu-id="f2976-259">**Důrazně doporučujeme že vytvořit pracovní svazek na svazku bez operačního systému Windows. Pokud zjistíte problémy s zálohování stavu systému, kontrola umístění svazku pracovní je prvním krokem řešení potíží.**</span><span class="sxs-lookup"><span data-stu-id="f2976-259">**We strongly recommended you establish the Staging Volume in a non-Windows-OS volume. If you observe problems with System State Backups, checking the location of your Staging Volume is the first troubleshooting step.**</span></span> 

### <a name="how-can-i-change-the-staging-volume-path-specified-in-the-azure-backup-agent"></a><span data-ttu-id="f2976-260">Jak je možné změnit cestu svazku pracovní zadaný v agenta Azure Backup?</span><span class="sxs-lookup"><span data-stu-id="f2976-260">How can I change the Staging Volume Path specified in the Azure Backup agent?</span></span>

<span data-ttu-id="f2976-261">Pracovní svazek umístěn ve složce mezipaměti ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f2976-261">The Staging Volume is located in the cache folder by default.</span></span> 

1. <span data-ttu-id="f2976-262">Chcete-li změnit toto umístění, použijte následující příkaz (v příkazovém řádku se zvýšenými):</span><span class="sxs-lookup"><span data-stu-id="f2976-262">To change this location, use the following command (in an elevated command prompt):</span></span>
  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="f2976-263">Aktualizujte následující položky registru s cestou do nové složky pracovní svazku.</span><span class="sxs-lookup"><span data-stu-id="f2976-263">Then update the following registry entries with the path to the new Staging Volume folder.</span></span>

  |<span data-ttu-id="f2976-264">Cesta k registru</span><span class="sxs-lookup"><span data-stu-id="f2976-264">Registry path</span></span>|<span data-ttu-id="f2976-265">Klíč registru</span><span class="sxs-lookup"><span data-stu-id="f2976-265">Registry key</span></span>|<span data-ttu-id="f2976-266">Hodnota</span><span class="sxs-lookup"><span data-stu-id="f2976-266">Value</span></span>|
  |-------------|------------|-----|
  |<span data-ttu-id="f2976-267">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="f2976-267">HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="f2976-268">SSBStagingPath</span><span class="sxs-lookup"><span data-stu-id="f2976-268">SSBStagingPath</span></span> | <span data-ttu-id="f2976-269">nový pracovní umístění svazku</span><span class="sxs-lookup"><span data-stu-id="f2976-269">new staging volume location</span></span> |

<span data-ttu-id="f2976-270">Cesta pracovní velká a malá písmena a musí být přesně stejnou malá a velká písmena jako co na serveru existuje.</span><span class="sxs-lookup"><span data-stu-id="f2976-270">The Staging Path is case sensitive and must be the exact same casing as what exists on the server.</span></span> 

3. <span data-ttu-id="f2976-271">Jakmile změníte cestu svazku pracovní, restartujte modul zálohování:</span><span class="sxs-lookup"><span data-stu-id="f2976-271">Once you change the Staging volume path, restart the Backup engine:</span></span>
  ```
  PS C:\> Net start obengine
  ```
4. <span data-ttu-id="f2976-272">Chcete-li vyzvednutí změněné cestu, otevřete agenta služeb zotavení Microsoft Azure a spustit ad hoc zálohování stavu systému.</span><span class="sxs-lookup"><span data-stu-id="f2976-272">To pick up the changed path, open the Microsoft Azure Recovery Services agent and trigger an ad hoc backup of System State.</span></span>

### <a name="why-is-the-system-state-default-retention-set-to-60-days"></a><span data-ttu-id="f2976-273">Proč je uchování výchozí stavu systému nastaven na 60 dnů?</span><span class="sxs-lookup"><span data-stu-id="f2976-273">Why is the System State default retention set to 60 days?</span></span>

<span data-ttu-id="f2976-274">Životnost zálohu stavu systému je stejné jako "životnosti objektů označených jako neplatné" nastavení pro roli systému Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f2976-274">The useful life of a system state backup is the same as the "tombstone lifetime" setting for the Windows Server Active Directory role.</span></span> <span data-ttu-id="f2976-275">Výchozí hodnota pro položku životnosti objektů označených jako neplatné je 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="f2976-275">The default value for the tombstone lifetime entry is 60 days.</span></span> <span data-ttu-id="f2976-276">Tuto hodnotu můžete nastavit u objektu konfigurace služby Directory (NTDS).</span><span class="sxs-lookup"><span data-stu-id="f2976-276">This value can be set on the Directory Service (NTDS) config object.</span></span>

### <a name="how-do-i-change-the-default-backup-and-retention-policy-for-system-state"></a><span data-ttu-id="f2976-277">Jak změnit výchozí zálohování a zásadami uchovávání informací pro stav systému?</span><span class="sxs-lookup"><span data-stu-id="f2976-277">How do I change the default Backup and Retention Policy for System State?</span></span>

<span data-ttu-id="f2976-278">Chcete-li změnit výchozí zálohování a zásadami uchovávání informací pro stav systému:</span><span class="sxs-lookup"><span data-stu-id="f2976-278">To change the default Backup and Retention Policy for System State:</span></span>
1. <span data-ttu-id="f2976-279">Zastavte modul zálohování.</span><span class="sxs-lookup"><span data-stu-id="f2976-279">Stop the Backup engine.</span></span> <span data-ttu-id="f2976-280">Spusťte následující příkaz z příkazového řádku se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="f2976-280">Run the following command from an elevated command prompt.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="f2976-281">Přidat nebo aktualizovat následující položky klíče registru HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.</span><span class="sxs-lookup"><span data-stu-id="f2976-281">Add or update the following registry key entries in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.</span></span>

  |<span data-ttu-id="f2976-282">Název registru</span><span class="sxs-lookup"><span data-stu-id="f2976-282">Registry Name</span></span>|<span data-ttu-id="f2976-283">Popis</span><span class="sxs-lookup"><span data-stu-id="f2976-283">Description</span></span>|<span data-ttu-id="f2976-284">Hodnota</span><span class="sxs-lookup"><span data-stu-id="f2976-284">Value</span></span>|
  |-------------|-----------|-----|
  |<span data-ttu-id="f2976-285">SSBScheduleTime</span><span class="sxs-lookup"><span data-stu-id="f2976-285">SSBScheduleTime</span></span>|<span data-ttu-id="f2976-286">Slouží ke konfiguraci čas zálohování.</span><span class="sxs-lookup"><span data-stu-id="f2976-286">Used to configure the time of the backup.</span></span> <span data-ttu-id="f2976-287">Výchozí hodnota je 21: 00 místního času.</span><span class="sxs-lookup"><span data-stu-id="f2976-287">Default is 9PM local time.</span></span>|<span data-ttu-id="f2976-288">DWord: Formátu hh: mm (decimální): pro příklad. 2130 21:30:00 místního času</span><span class="sxs-lookup"><span data-stu-id="f2976-288">DWord: Format HHMM (Decimal) for example 2130 for 9:30PM local time</span></span>|
  |<span data-ttu-id="f2976-289">SSBScheduleDays</span><span class="sxs-lookup"><span data-stu-id="f2976-289">SSBScheduleDays</span></span>|<span data-ttu-id="f2976-290">Slouží ke konfiguraci dní, kdy musí být prováděno zálohování stavu systému v určeném čase.</span><span class="sxs-lookup"><span data-stu-id="f2976-290">Used to configure the days when System State Backup must be performed at the specified time.</span></span> <span data-ttu-id="f2976-291">Jednotlivé číslic Zadejte dny v týdnu.</span><span class="sxs-lookup"><span data-stu-id="f2976-291">Individual digits specify days of the week.</span></span> <span data-ttu-id="f2976-292">představuje neděli 0, 1 je pondělí, a tak dále.</span><span class="sxs-lookup"><span data-stu-id="f2976-292">0 represents Sunday, 1 is Monday, and so on.</span></span> <span data-ttu-id="f2976-293">Výchozí den pro zálohování je neděli.</span><span class="sxs-lookup"><span data-stu-id="f2976-293">Default day for backup is Sunday.</span></span>|<span data-ttu-id="f2976-294">DWord: dny v týdnu, chcete-li spustit zálohování (decimální): Příklad 1 230 plány zálohování v pondělí, úterý, středu a neděle.</span><span class="sxs-lookup"><span data-stu-id="f2976-294">DWord: days of the week to run backup (decimal) for example 1230 schedules backups on Monday, Tuesday, Wednesday, and Sunday.</span></span>|
  |<span data-ttu-id="f2976-295">SSBRetentionDays</span><span class="sxs-lookup"><span data-stu-id="f2976-295">SSBRetentionDays</span></span>|<span data-ttu-id="f2976-296">Slouží ke konfiguraci počet dní do uchovávat zálohu.</span><span class="sxs-lookup"><span data-stu-id="f2976-296">Used to configure the days to retain backup.</span></span> <span data-ttu-id="f2976-297">Výchozí hodnota je 60.</span><span class="sxs-lookup"><span data-stu-id="f2976-297">Default value is 60.</span></span> <span data-ttu-id="f2976-298">Maximální povolená hodnota je 180.</span><span class="sxs-lookup"><span data-stu-id="f2976-298">Maximum allowed value is 180.</span></span>|<span data-ttu-id="f2976-299">DWord: Uchovávat zálohu (decimální): dnů.</span><span class="sxs-lookup"><span data-stu-id="f2976-299">DWord: Days to retain backup (decimal).</span></span>|

3. <span data-ttu-id="f2976-300">Použijte následující příkaz k restartování stroje zálohování.</span><span class="sxs-lookup"><span data-stu-id="f2976-300">Use the following command to restart the backup engine.</span></span>
    ```
    PS C:\> Net start obengine
    ```

4. <span data-ttu-id="f2976-301">Otevřete agenta služeb zotavení Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f2976-301">Open the Microsoft Recovery Services agent.</span></span>

5. <span data-ttu-id="f2976-302">Klikněte na tlačítko **plánem zálohování** a pak klikněte na **Další** až se změny projeví.</span><span class="sxs-lookup"><span data-stu-id="f2976-302">Click **Schedule Backup** and then click **Next** until you see the changes reflected.</span></span>

6. <span data-ttu-id="f2976-303">Klikněte na tlačítko **Dokončit** aby se změny projevily.</span><span class="sxs-lookup"><span data-stu-id="f2976-303">Click **Finish** to apply the changes.</span></span>


## <a name="questions"></a><span data-ttu-id="f2976-304">Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="f2976-304">Questions?</span></span>
<span data-ttu-id="f2976-305">Máte-li nějaké dotazy nebo pokud víte o funkci, kterou byste uvítali, [odešlete nám svůj názor](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="f2976-305">If you have questions, or if there is any feature that you would like to see included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2976-306">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f2976-306">Next steps</span></span>
* <span data-ttu-id="f2976-307">Zdroj dalších informací o [zálohování počítačů se systémem Windows](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="f2976-307">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="f2976-308">Teď, když jste zálohovali své soubory a složky, můžete [spravovat svoje trezory a servery](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="f2976-308">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="f2976-309">Potřebujete-li obnovit zálohu, použijte tento článek k [obnovení souborů na počítač se systémem Windows](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="f2976-309">If you need to restore a backup, use this article to [restore files to a Windows machine](backup-azure-restore-windows-server.md).</span></span>
