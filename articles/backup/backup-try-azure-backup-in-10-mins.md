---
title: "Zálohování souborů a složek z Windows do Azure (Resource Manager) | Dokumentace Microsoftu"
description: "Postup zálohování souborů a složek z Windows do Azure v nasazení podle modelu Resource Manager."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "postup zálohování; zálohování souborů a složek"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 8/15/2017
ms.author: markgal;
ms.openlocfilehash: b21edb70eca3ec9552dc157ee3bb658d243b8fcd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="first-look-back-up-files-and-folders-in-resource-manager-deployment"></a><span data-ttu-id="d5dde-104">První pohled: Zálohování souborů a složek v nasazení podle modelu Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d5dde-104">First look: back up files and folders in Resource Manager deployment</span></span>
<span data-ttu-id="d5dde-105">Tento článek vysvětluje, jak postupovat při zálohování souborů a složek z Windows Serveru (nebo z počítače s Windows) do Azure pomocí nasazení podle modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d5dde-105">This article explains how to back up your Windows Server (or Windows computer) files and folders to Azure using a Resource Manager deployment.</span></span> <span data-ttu-id="d5dde-106">Tento kurz vás má provést základy.</span><span class="sxs-lookup"><span data-stu-id="d5dde-106">It's a tutorial intended to walk you through the basics.</span></span> <span data-ttu-id="d5dde-107">Chcete-li začít používat Azure Backup, jste na správném místě.</span><span class="sxs-lookup"><span data-stu-id="d5dde-107">If you want to get started using Azure Backup, you're in the right place.</span></span>

<span data-ttu-id="d5dde-108">Chcete-li se dozvědět více o Azure Backup, přečtěte si tento [přehled](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="d5dde-108">If you want to know more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="d5dde-109">Pokud předplatné Azure nemáte, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/), který vám umožní přístup ke službám Azure.</span><span class="sxs-lookup"><span data-stu-id="d5dde-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="d5dde-110">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="d5dde-110">Create a recovery services vault</span></span>
<span data-ttu-id="d5dde-111">Chcete-li zálohovat svoje soubory a složky, musíte vytvořit trezor Služeb zotavení v oblasti, kde chcete data ukládat.</span><span class="sxs-lookup"><span data-stu-id="d5dde-111">To back up your files and folders, you need to create a Recovery Services vault in the region where you want to store the data.</span></span> <span data-ttu-id="d5dde-112">Musíte také určit způsob replikace úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5dde-112">You also need to determine how you want your storage replicated.</span></span>

### <a name="to-create-a-recovery-services-vault"></a><span data-ttu-id="d5dde-113">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="d5dde-113">To create a Recovery Services vault</span></span>
1. <span data-ttu-id="d5dde-114">Pokud jste to ještě neudělali, přihlaste se k [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="d5dde-114">If you haven't already done so, sign in to the [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="d5dde-115">V nabídce centra klikněte na **Procházet**, v seznamu prostředků zadejte **Recovery Services** a klikněte na **Trezory služby Recovery Services**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-115">On the Hub menu, click **More services** and in the list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="d5dde-117">Pokud předplatné obsahuje trezory služby Recovery Services, jsou tyto trezory uvedené v seznamu.</span><span class="sxs-lookup"><span data-stu-id="d5dde-117">If there are recovery services vaults in the subscription, the vaults are listed.</span></span>
3. <span data-ttu-id="d5dde-118">V nabídce **Trezory Recovery Services** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-118">On the **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="d5dde-120">Otevře se okno trezoru Recovery Services s výzvou k vyplnění polí **Název**, **Předplatné**, **Skupina prostředků** a **Oblast**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-120">The Recovery Services vault blade opens, prompting you to provide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="d5dde-122">Jako **Název** zadejte popisný název pro identifikaci trezoru.</span><span class="sxs-lookup"><span data-stu-id="d5dde-122">For **Name**, enter a friendly name to identify the vault.</span></span> <span data-ttu-id="d5dde-123">Název musí být jedinečný v rámci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="d5dde-123">The name needs to be unique for the Azure subscription.</span></span> <span data-ttu-id="d5dde-124">Zadejte název v rozsahu 2 až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="d5dde-124">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="d5dde-125">Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="d5dde-125">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="d5dde-126">V části **Předplatné** z rozevírací nabídky vyberte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="d5dde-126">In the **Subscription** section, use the drop-down menu to choose the Azure subscription.</span></span> <span data-ttu-id="d5dde-127">Pokud používáte jenom jedno předplatné, zobrazí se toto předplatné a můžete přejít k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="d5dde-127">If you use only one subscription, that subscription appears and you can skip to the next step.</span></span> <span data-ttu-id="d5dde-128">Pokud si nejste jisti, jaké předplatné použít, použijte výchozí (nebo navrhované) předplatné.</span><span class="sxs-lookup"><span data-stu-id="d5dde-128">If you are not sure which subscription to use, use the default (or suggested) subscription.</span></span> <span data-ttu-id="d5dde-129">Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="d5dde-129">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="d5dde-130">V části **Skupina prostředků**:</span><span class="sxs-lookup"><span data-stu-id="d5dde-130">In the **Resource group** section:</span></span>

    * <span data-ttu-id="d5dde-131">vyberte **Vytvořit novou**, pokud chcete vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d5dde-131">select **Create new** if you want to create a new Resource group.</span></span>
    <span data-ttu-id="d5dde-132">Nebo</span><span class="sxs-lookup"><span data-stu-id="d5dde-132">Or</span></span>
    * <span data-ttu-id="d5dde-133">vyberte **Použít existující** a kliknutím na rozevírací nabídku zobrazte seznam dostupných skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="d5dde-133">select **Use existing** and click the drop-down menu to see the available list of Resource groups.</span></span>

  <span data-ttu-id="d5dde-134">Kompletní informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5dde-134">For complete information on Resource groups, see the [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="d5dde-135">Klikněte na **Oblast** a vyberte zeměpisnou oblast trezoru.</span><span class="sxs-lookup"><span data-stu-id="d5dde-135">Click **Location** to select the geographic region for the vault.</span></span> <span data-ttu-id="d5dde-136">Tato volba určuje geografickou oblast, kam jsou zasílaná vaše zálohovaná data.</span><span class="sxs-lookup"><span data-stu-id="d5dde-136">This choice determines the geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="d5dde-137">V dolní části okna trezoru služby Recovery Services klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-137">At the bottom of the Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="d5dde-138">Vytvoření trezoru služby Recovery Services může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="d5dde-138">It can take several minutes for the Recovery Services vault to be created.</span></span> <span data-ttu-id="d5dde-139">Sledujte oznámení o stavu v pravé horní části portálu.</span><span class="sxs-lookup"><span data-stu-id="d5dde-139">Monitor the status notifications in the upper right-hand area of the portal.</span></span> <span data-ttu-id="d5dde-140">Když je trezor vytvořený, zobrazí se v seznamu trezorů Služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="d5dde-140">Once your vault is created, it appears in the list of Recovery Services vaults.</span></span> <span data-ttu-id="d5dde-141">Pokud se trezor nezobrazí ani po několika minutách, klikněte na **Obnovit**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-141">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Kliknutí na tlačítko Obnovit](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="d5dde-143">Jakmile se trezor zobrazí v seznamu trezorů služby Recovery Services, jste připraveni nastavit redundanci úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5dde-143">Once you see your vault in the list of Recovery Services vaults, you are ready to set the storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-the-vault"></a><span data-ttu-id="d5dde-144">Nastavení redundance úložiště pro trezor</span><span class="sxs-lookup"><span data-stu-id="d5dde-144">Set storage redundancy for the vault</span></span>
<span data-ttu-id="d5dde-145">Při vytváření trezoru služby Recovery Services se ujistěte, že je redundance úložiště nakonfigurována požadovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="d5dde-145">When you create a Recovery Services vault, make sure storage redundancy is configured the way you want.</span></span>

1. <span data-ttu-id="d5dde-146">V okně **Trezory služby Recovery Services** klikněte na nový trezor.</span><span class="sxs-lookup"><span data-stu-id="d5dde-146">From the **Recovery Services vaults** blade, click the new vault.</span></span>

    ![Výběr nového trezoru ze seznamu trezorů služby Recovery Services](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="d5dde-148">Po výběru trezoru se okno **Trezory služby Recovery Services** zúží a otevře se okno Nastavení (*s názvem trezoru v horní části*) a okno s podrobnostmi o trezoru.</span><span class="sxs-lookup"><span data-stu-id="d5dde-148">When you select the vault, the **Recovery Services vault** blade narrows, and the Settings blade (*which has the name of the vault at the top*) and the vault details blade open.</span></span>

    ![Zobrazení konfigurace úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="d5dde-150">V okně Nastavení nového trezoru pomocí vertikálního posuvníku přejděte dolů do části Správa a klikněte na **Infrastruktura zálohování**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-150">In the new vault's Settings blade, use the vertical slide to scroll down to the Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="d5dde-151">Otevře se okno Infrastruktura zálohování.</span><span class="sxs-lookup"><span data-stu-id="d5dde-151">The Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="d5dde-152">V okně Infrastruktura zálohování klikněte na **Konfigurace zálohování**. Otevře se okno **Konfigurace zálohování**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-152">In the Backup Infrastructure blade, click **Backup Configuration** to open the **Backup Configuration** blade.</span></span>

    ![Nastavení konfigurace úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="d5dde-154">Zvolte vhodnou možnost replikace pro svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="d5dde-154">Choose the appropriate storage replication option for your vault.</span></span>

    ![volby konfigurace úložiště](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="d5dde-156">Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5dde-156">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="d5dde-157">Pokud používáte Azure jako primární koncový bod úložiště záloh, pokračujte v používání **geograficky redundantního** úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5dde-157">If you use Azure as a primary backup storage endpoint, continue to use **Geo-redundant**.</span></span> <span data-ttu-id="d5dde-158">Pokud Azure nepoužíváte jako primární koncový bod úložiště záloh, vyberte **Místně redundantní** – snížíte tím náklady na úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d5dde-158">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces the Azure storage costs.</span></span> <span data-ttu-id="d5dde-159">Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště najdete v tomto [přehledu redundance úložiště](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="d5dde-159">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="d5dde-160">Teď když jste vytvořili trezor, nakonfigurujte pro něj zálohování souborů a složek.</span><span class="sxs-lookup"><span data-stu-id="d5dde-160">Now that you've created a vault, configure it for backing up files and folders.</span></span>

## <a name="configure-the-vault"></a><span data-ttu-id="d5dde-161">Konfigurace trezoru</span><span class="sxs-lookup"><span data-stu-id="d5dde-161">Configure the vault</span></span>
1. <span data-ttu-id="d5dde-162">V okně trezoru služby Recovery Services (pro trezor, který jste právě vytvořili) klikněte v části Začínáme na **Zálohovat** a potom v okně **Začínáme se zálohováním** vyberte **Cíl zálohování**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-162">On the Recovery Services vault blade (for the vault you just created), in the Getting Started section, click **Backup**, then on the **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="d5dde-164">Otevře se okno **Cíl zálohování**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-164">The **Backup Goal** blade opens.</span></span>

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="d5dde-166">V rozevírací nabídce **Kde běží vaše úlohy?** vyberte **Místní**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-166">From the **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="d5dde-167">Možnost **Místní** jste vybrali proto, že počítačem s Windows Serverem nebo Windows je fyzický počítač, který není v Azure.</span><span class="sxs-lookup"><span data-stu-id="d5dde-167">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="d5dde-168">V nabídce **Co chcete zálohovat?** vyberte **Soubory a složky** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-168">From the **What do you want to backup?** menu, select **Files and folders**, and click **OK**.</span></span>

    ![Konfigurace souborů a složek](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

    <span data-ttu-id="d5dde-170">Po kliknutí na OK se vedle položky **Cíl zálohování** zobrazí zaškrtnutí a otevře se okno **Připravit infrastrukturu**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-170">After clicking OK, a checkmark appears next to **Backup goal**, and the **Prepare infrastructure** blade opens.</span></span>

    ![Cíl zálohování je nakonfigurovaný, teď se připraví infrastruktura](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="d5dde-172">V okně **Připravit infrastrukturu** klikněte na **Stáhnout agenta pro Windows Server nebo klienta Windows**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-172">On the **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="d5dde-174">Pokud používáte Windows Server Essential, vyberte ke stažení agenta pro Windows Server Essential.</span><span class="sxs-lookup"><span data-stu-id="d5dde-174">If you are using Windows Server Essential, then choose to download the agent for Windows Server Essential.</span></span> <span data-ttu-id="d5dde-175">Místní nabídka zobrazí výzvu ke spuštění nebo uložení MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="d5dde-175">A pop-up menu prompts you to run or save MARSAgentInstaller.exe.</span></span>

    ![Dialogové okno MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="d5dde-177">V místní nabídce stahování klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-177">In the download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="d5dde-178">Ve výchozím nastavení se soubor **MARSagentinstaller.exe** uloží do složky Stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="d5dde-178">By default, the **MARSagentinstaller.exe** file is saved to your Downloads folder.</span></span> <span data-ttu-id="d5dde-179">Po dokončení instalačního programu se zobrazí automaticky otevírané okno s dotazem, jestli chcete spustit instalační program nebo otevřít složku.</span><span class="sxs-lookup"><span data-stu-id="d5dde-179">When the installer completes, you will see a pop-up asking if you want to run the installer, or open the folder.</span></span>

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="d5dde-181">Agenta ještě nemusíte instalovat.</span><span class="sxs-lookup"><span data-stu-id="d5dde-181">You don't need to install the agent yet.</span></span> <span data-ttu-id="d5dde-182">Agenta můžete nainstalovat po stažení přihlašovacích údajů trezoru.</span><span class="sxs-lookup"><span data-stu-id="d5dde-182">You can install the agent after you have downloaded the vault credentials.</span></span>

6. <span data-ttu-id="d5dde-183">V okně **Připravit infrastrukturu** klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-183">On the **Prepare infrastructure** blade, click **Download**.</span></span>

    ![stažení přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="d5dde-185">Přihlašovací údaje trezoru se stáhnou do složky Stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="d5dde-185">The vault credentials download to your Downloads folder.</span></span> <span data-ttu-id="d5dde-186">Po dokončení stahování přihlašovacích údajů trezoru se zobrazí automaticky otevírané okno s dotazem, jestli chcete přihlašovací údaje otevřít nebo uložit.</span><span class="sxs-lookup"><span data-stu-id="d5dde-186">After the vault credentials finish downloading, you see a pop-up asking if you want to open or save the credentials.</span></span> <span data-ttu-id="d5dde-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-187">Click **Save**.</span></span> <span data-ttu-id="d5dde-188">Pokud omylem kliknete **Otevřít**, nechte dialogové okno, které se pokusí otevřít přihlašovací údaje trezoru, zobrazit chybu.</span><span class="sxs-lookup"><span data-stu-id="d5dde-188">If you accidentally click **Open**, let the dialog that attempts to open the vault credentials, fail.</span></span> <span data-ttu-id="d5dde-189">Přihlašovací údaje trezoru nejde otevřít.</span><span class="sxs-lookup"><span data-stu-id="d5dde-189">You cannot open the vault credentials.</span></span> <span data-ttu-id="d5dde-190">Přejděte k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="d5dde-190">Proceed to the next step.</span></span> <span data-ttu-id="d5dde-191">Přihlašovací údaje trezoru jsou ve složce Stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="d5dde-191">The vault credentials are in the Downloads folder.</span></span>   

    ![dokončené stahování přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a><span data-ttu-id="d5dde-193">Instalace a registrace agenta</span><span class="sxs-lookup"><span data-stu-id="d5dde-193">Install and register the agent</span></span>

> [!NOTE]
> <span data-ttu-id="d5dde-194">Povolení zálohování prostřednictvím webu Azure Portal ještě není dostupné.</span><span class="sxs-lookup"><span data-stu-id="d5dde-194">Enabling backup through the Azure portal is not available, yet.</span></span> <span data-ttu-id="d5dde-195">K zálohování svých souborů a složek použijte agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="d5dde-195">Use the Microsoft Azure Recovery Services Agent to back up your files and folders.</span></span>
>

1. <span data-ttu-id="d5dde-196">Ve složce Stažené soubory (nebo ve složce, kterou jste vybrali pro stahování) vyhledejte soubor **MARSagentinstaller.exe** a dvakrát na něj klikněte.</span><span class="sxs-lookup"><span data-stu-id="d5dde-196">Locate and double-click the **MARSagentinstaller.exe** from the Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="d5dde-197">Instalační program v průběhu extrakce, instalace a registrace agenta Recovery Services zobrazuje řadu zpráv.</span><span class="sxs-lookup"><span data-stu-id="d5dde-197">The installer provides a series of messages as it extracts, installs, and registers the Recovery Services agent.</span></span>

    ![spuštění přihlašovacích údajů instalačního programu agenta Recovery Services](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="d5dde-199">Dokončete Průvodce instalací agenta Služeb zotavení Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d5dde-199">Complete the Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="d5dde-200">K dokončení průvodce budete muset:</span><span class="sxs-lookup"><span data-stu-id="d5dde-200">To complete the wizard, you need to:</span></span>

   * <span data-ttu-id="d5dde-201">Vybrat umístění instalace a složky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d5dde-201">Choose a location for the installation and cache folder.</span></span>
   * <span data-ttu-id="d5dde-202">Zadat informace o serveru proxy, pokud pro připojování k internetu používáte server proxy.</span><span class="sxs-lookup"><span data-stu-id="d5dde-202">Provide your proxy server info if you use a proxy server to connect to the internet.</span></span>
   * <span data-ttu-id="d5dde-203">Zadat svoje uživatelské jméno a heslo, pokud používáte ověřený server proxy.</span><span class="sxs-lookup"><span data-stu-id="d5dde-203">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="d5dde-204">Zadat stažené přihlašovací údaje trezoru.</span><span class="sxs-lookup"><span data-stu-id="d5dde-204">Provide the downloaded vault credentials</span></span>
   * <span data-ttu-id="d5dde-205">Uložit šifrovací heslo na bezpečné místo.</span><span class="sxs-lookup"><span data-stu-id="d5dde-205">Save the encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d5dde-206">Pokud heslo ztratíte nebo zapomenete, Microsoft vám nemůže pomoci obnovit zálohovaná data.</span><span class="sxs-lookup"><span data-stu-id="d5dde-206">If you lose or forget the passphrase, Microsoft cannot help recover the backup data.</span></span> <span data-ttu-id="d5dde-207">Uložte soubor na bezpečné místo.</span><span class="sxs-lookup"><span data-stu-id="d5dde-207">Save the file in a secure location.</span></span> <span data-ttu-id="d5dde-208">Je požadováno pro obnovení zálohy.</span><span class="sxs-lookup"><span data-stu-id="d5dde-208">It is required to restore a backup.</span></span>
     >
     >

<span data-ttu-id="d5dde-209">Agent je nyní nainstalovaný a váš počítač je registrovaný k trezoru.</span><span class="sxs-lookup"><span data-stu-id="d5dde-209">The agent is now installed and your machine is registered to the vault.</span></span> <span data-ttu-id="d5dde-210">Jste připraveni nakonfigurovat a naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="d5dde-210">You're ready to configure and schedule your backup.</span></span>

## <a name="network-and-connectivity-requirements"></a><span data-ttu-id="d5dde-211">Požadavky na síť a připojení</span><span class="sxs-lookup"><span data-stu-id="d5dde-211">Network and Connectivity Requirements</span></span>

<span data-ttu-id="d5dde-212">Pokud má váš počítač nebo proxy server omezený přístup k internetu, ujistěte se, že nastavení brány firewall na počítači nebo proxy serveru jsou nakonfigurována tak, aby povolovala následující adresy URL:</span><span class="sxs-lookup"><span data-stu-id="d5dde-212">If your machine/proxy has limited internet access, ensure that firewall settings on the mcahine/proxy are configured to allow the following URLs:</span></span> <br>
    1. <span data-ttu-id="d5dde-213">www.msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="d5dde-213">www.msftncsi.com</span></span>
    2. <span data-ttu-id="d5dde-214">*.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="d5dde-214">*.Microsoft.com</span></span>
    3. <span data-ttu-id="d5dde-215">*.WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="d5dde-215">*.WindowsAzure.com</span></span>
    4. <span data-ttu-id="d5dde-216">*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="d5dde-216">*.microsoftonline.com</span></span>
    5. <span data-ttu-id="d5dde-217">*.windows.ne</span><span class="sxs-lookup"><span data-stu-id="d5dde-217">*.windows.ne</span></span>

## <a name="back-up-your-files-and-folders"></a><span data-ttu-id="d5dde-218">Zálohování souborů a složek</span><span class="sxs-lookup"><span data-stu-id="d5dde-218">Back up your files and folders</span></span>
<span data-ttu-id="d5dde-219">Prvotní záloha zahrnuje dvě klíčové úlohy:</span><span class="sxs-lookup"><span data-stu-id="d5dde-219">The initial backup includes two key tasks:</span></span>

* <span data-ttu-id="d5dde-220">Naplánování zálohování</span><span class="sxs-lookup"><span data-stu-id="d5dde-220">Schedule the backup</span></span>
* <span data-ttu-id="d5dde-221">První zálohování souborů a složek</span><span class="sxs-lookup"><span data-stu-id="d5dde-221">Back up files and folders for the first time</span></span>

<span data-ttu-id="d5dde-222">K dokončení prvotního zálohování použijte agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="d5dde-222">To complete the initial backup, use the Microsoft Azure Recovery Services agent.</span></span>

### <a name="to-schedule-the-backup-job"></a><span data-ttu-id="d5dde-223">Naplánování úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="d5dde-223">To schedule the backup job</span></span>
1. <span data-ttu-id="d5dde-224">Otevřete agenta Služeb zotavení Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d5dde-224">Open the Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="d5dde-225">Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.</span><span class="sxs-lookup"><span data-stu-id="d5dde-225">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Spuštění agenta Služeb zotavení Azure](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)
2. <span data-ttu-id="d5dde-227">V agentu Služeb zotavení klikněte na **Naplánovat zálohování**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-227">In the Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Naplánování zálohování Windows Serveru](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)
3. <span data-ttu-id="d5dde-229">Na stránce Začínáme v Průvodci plánování zálohování klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-229">On the Getting started page of the Schedule Backup Wizard, click **Next**.</span></span>
4. <span data-ttu-id="d5dde-230">Na stránce Výběr položek k zálohování klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-230">On the Select Items to Backup page, click **Add Items**.</span></span>
5. <span data-ttu-id="d5dde-231">Vyberte soubory a složky, které chcete zálohovat, a poté klikněte na **V pořádku**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-231">Select the files and folders that you want to back up, and then click **Okay**.</span></span>
6. <span data-ttu-id="d5dde-232">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-232">Click **Next**.</span></span>
7. <span data-ttu-id="d5dde-233">Na stránce **Zadání plánu zálohování** zadejte **plán zálohování** a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-233">On the **Specify Backup Schedule** page, specify the **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="d5dde-234">Můžete naplánovat denní (probíhající maximálně třikrát za den) nebo týdenní zálohování.</span><span class="sxs-lookup"><span data-stu-id="d5dde-234">You can schedule daily (at a maximum rate of three times per day) or weekly backups.</span></span>

    ![Položky k zálohování z Windows Serveru](./media/backup-try-azure-backup-in-10-mins/specify-backup-schedule-close.png)

   > [!NOTE]
   > <span data-ttu-id="d5dde-236">Další informace o tom, jak zadat plán zálohování, naleznete v článku [Use Azure Backup to replace your tape infrastructure](backup-azure-backup-cloud-as-tape.md) (Použití služby Azure Backup k nahrazení páskové infrastruktury).</span><span class="sxs-lookup"><span data-stu-id="d5dde-236">For more information about how to specify the backup schedule, see the article [Use Azure Backup to replace your tape infrastructure](backup-azure-backup-cloud-as-tape.md).</span></span>
   >

8. <span data-ttu-id="d5dde-237">Na stránce **Výběr zásady uchovávání informací** vyberte **zásadu uchovávání informací** pro záložní kopii.</span><span class="sxs-lookup"><span data-stu-id="d5dde-237">On the **Select Retention Policy** page, select the **Retention Policy** for the backup copy.</span></span>

    <span data-ttu-id="d5dde-238">Zásada uchovávání informací určuje, jak dlouho budou zálohovaná data uložená.</span><span class="sxs-lookup"><span data-stu-id="d5dde-238">The retention policy specifies how long the backup data is stored.</span></span> <span data-ttu-id="d5dde-239">Místo zadání „ploché zásady“ pro všechny body zálohy můžete zadat různé zásady uchovávání informací v závislosti na tom, kdy dochází k zálohování.</span><span class="sxs-lookup"><span data-stu-id="d5dde-239">Rather than specifying a “flat policy” for all backup points, you can specify different retention policies based on when the backup occurs.</span></span> <span data-ttu-id="d5dde-240">Podle svých potřeb můžete upravit denní, týdenní, měsíční a roční zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="d5dde-240">You can modify the daily, weekly, monthly, and yearly retention policies to meet your needs.</span></span>
9. <span data-ttu-id="d5dde-241">Na stránce Výběr typu prvotní zálohy vyberte typ prvotní zálohy.</span><span class="sxs-lookup"><span data-stu-id="d5dde-241">On the Choose Initial Backup Type page, choose the initial backup type.</span></span> <span data-ttu-id="d5dde-242">Ponechejte vybranou možnost **Automaticky přes síť** a poté klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-242">Leave the option **Automatically over the network** selected, and then click **Next**.</span></span>

    <span data-ttu-id="d5dde-243">Zálohovat můžete automaticky přes síť nebo offline.</span><span class="sxs-lookup"><span data-stu-id="d5dde-243">You can back up automatically over the network, or you can back up offline.</span></span> <span data-ttu-id="d5dde-244">Zbývající část tohoto článku popisuje proces automatického zálohování.</span><span class="sxs-lookup"><span data-stu-id="d5dde-244">The remainder of this article describes the process for backing up automatically.</span></span> <span data-ttu-id="d5dde-245">Pokud upřednostňujete offline zálohování, přečtěte si článek [Pracovní postup offline zálohování v Azure Backup](backup-azure-backup-import-export.md), kde naleznete další informace.</span><span class="sxs-lookup"><span data-stu-id="d5dde-245">If you prefer to do an offline backup, review the article [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) for additional information.</span></span>
10. <span data-ttu-id="d5dde-246">Na stránce Potvrzení zkontrolujte informace a poté klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-246">On the Confirmation page, review the information, and then click **Finish**.</span></span>
11. <span data-ttu-id="d5dde-247">Až průvodce dokončí vytváření plánu zálohování, klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-247">After the wizard finishes creating the backup schedule, click **Close**.</span></span>

### <a name="to-back-up-files-and-folders-for-the-first-time"></a><span data-ttu-id="d5dde-248">První zálohování souborů a složek</span><span class="sxs-lookup"><span data-stu-id="d5dde-248">To back up files and folders for the first time</span></span>
1. <span data-ttu-id="d5dde-249">Chcete-li dokončit prvotní synchronizaci přes síť, v agentu Služeb zotavení klikněte na **Zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-249">In the Recovery Services agent, click **Back Up Now** to complete the initial seeding over the network.</span></span>

    ![Zálohovat nyní ve Windows Serveru](./media/backup-try-azure-backup-in-10-mins/backup-now.png)
2. <span data-ttu-id="d5dde-251">Na stránce Potvrzení zkontrolujte nastavení, které Průvodce Zálohování nyní použije k zálohování počítače.</span><span class="sxs-lookup"><span data-stu-id="d5dde-251">On the Confirmation page, review the settings that the Back Up Now Wizard will use to back up the machine.</span></span> <span data-ttu-id="d5dde-252">Poté klikněte na **Zálohovat**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-252">Then click **Back Up**.</span></span>
3. <span data-ttu-id="d5dde-253">Průvodce zavřete kliknutím na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-253">Click **Close** to close the wizard.</span></span> <span data-ttu-id="d5dde-254">Pokud průvodce zavřete před dokončením procesu zálohování, průvodce zůstane spuštěný na pozadí.</span><span class="sxs-lookup"><span data-stu-id="d5dde-254">If you close the wizard before the backup process finishes, the wizard continues to run in the background.</span></span>

<span data-ttu-id="d5dde-255">Po dokončení prvotní zálohy se v konzole Zálohování zobrazí stav **Úloha byla dokončena**.</span><span class="sxs-lookup"><span data-stu-id="d5dde-255">After the initial backup is completed, the **Job completed** status appears in the Backup console.</span></span>

![Dokončení IR](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a><span data-ttu-id="d5dde-257">Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="d5dde-257">Questions?</span></span>
<span data-ttu-id="d5dde-258">Máte-li nějaké dotazy nebo pokud víte o funkci, kterou byste uvítali, [odešlete nám svůj názor](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="d5dde-258">If you have questions, or if there is any feature that you would like to see included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5dde-259">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5dde-259">Next steps</span></span>
* <span data-ttu-id="d5dde-260">Zdroj dalších informací o [zálohování počítačů se systémem Windows](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="d5dde-260">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="d5dde-261">Teď, když jste zálohovali své soubory a složky, můžete [spravovat svoje trezory a servery](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="d5dde-261">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="d5dde-262">Potřebujete-li obnovit zálohu, použijte tento článek k [obnovení souborů na počítač se systémem Windows](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="d5dde-262">If you need to restore a backup, use this article to [restore files to a Windows machine](backup-azure-restore-windows-server.md).</span></span>
