---
title: "Pomocí agenta Azure Backup k zálohování souborů a složek | Microsoft Docs"
description: "Zálohování Windows souborů a složek do Azure pomocí Microsoft Azure Backup agent. Vytvoření trezoru služeb zotavení, nainstalujte agenta zálohování, definování zásad zálohování a spusťte prvotní zálohování souborů a složek."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "Trezor záloh; zálohování na Windows server. zálohování systému windows."
ms.assetid: 7f5b1943-b3c1-4ddb-8fb7-3560533c68d5
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: b95dc0a83d8e5618effb573353f419e1837d30c5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-a-windows-server-or-client-to-azure-using-the-resource-manager-deployment-model"></a><span data-ttu-id="c150d-105">Zálohování klienta nebo Windows Serveru do Azure s využitím modelu nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c150d-105">Back up a Windows Server or client to Azure using the Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c150d-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c150d-106">Azure portal</span></span>](backup-configure-vault.md)
> * [<span data-ttu-id="c150d-107">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="c150d-107">Classic portal</span></span>](backup-configure-vault-classic.md)
>
>

<span data-ttu-id="c150d-108">Tento článek vysvětluje, jak zálohování Windows serveru (nebo klienta Windows) souborů a složek do Azure s Azure Backup pomocí modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c150d-108">This article explains how to back up your Windows Server (or Windows client) files and folders to Azure with Azure Backup using the Resource Manager deployment model.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/backup-deployment-models.md)]

![Kroky procesu zálohování](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a><span data-ttu-id="c150d-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c150d-110">Before you start</span></span>
<span data-ttu-id="c150d-111">K zálohování serveru nebo klienta do Azure, potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c150d-111">To back up a server or client to Azure, you need an Azure account.</span></span> <span data-ttu-id="c150d-112">Pokud nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="c150d-112">If you don't have one, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="c150d-113">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="c150d-113">Create a Recovery Services vault</span></span>
<span data-ttu-id="c150d-114">Trezor služeb zotavení je entita, která ukládá všechny vytvořené zálohy a body obnovení, které vytvoříte v čase.</span><span class="sxs-lookup"><span data-stu-id="c150d-114">A Recovery Services vault is an entity that stores all the backups and recovery points you create over time.</span></span> <span data-ttu-id="c150d-115">Trezor služeb zotavení obsahuje také zásady zálohování, které jsou nastavené u chráněných souborů a složek.</span><span class="sxs-lookup"><span data-stu-id="c150d-115">The Recovery Services vault also contains the backup policy applied to the protected files and folders.</span></span> <span data-ttu-id="c150d-116">Při vytváření trezoru služeb zotavení musí také vybrat možnost redundance příslušné úložiště.</span><span class="sxs-lookup"><span data-stu-id="c150d-116">When you create a Recovery Services vault, you should also select the appropriate storage redundancy option.</span></span>

### <a name="to-create-a-recovery-services-vault"></a><span data-ttu-id="c150d-117">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="c150d-117">To create a Recovery Services vault</span></span>
1. <span data-ttu-id="c150d-118">Pokud jste to ještě neudělali, přihlaste se k [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c150d-118">If you haven't already done so, sign in to the [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="c150d-119">V nabídce centra klikněte na **Procházet**, v seznamu prostředků zadejte **Recovery Services** a klikněte na **Trezory služby Recovery Services**.</span><span class="sxs-lookup"><span data-stu-id="c150d-119">On the Hub menu, click **More services** and in the list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="c150d-121">Pokud předplatné obsahuje trezory služby Recovery Services, jsou tyto trezory uvedené v seznamu.</span><span class="sxs-lookup"><span data-stu-id="c150d-121">If there are recovery services vaults in the subscription, the vaults are listed.</span></span>

3. <span data-ttu-id="c150d-122">V nabídce **Trezory Recovery Services** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="c150d-122">On the **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="c150d-124">Otevře se okno trezoru Recovery Services s výzvou k vyplnění polí **Název**, **Předplatné**, **Skupina prostředků** a **Oblast**.</span><span class="sxs-lookup"><span data-stu-id="c150d-124">The Recovery Services vault blade opens, prompting you to provide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="c150d-126">Jako **Název** zadejte popisný název pro identifikaci trezoru.</span><span class="sxs-lookup"><span data-stu-id="c150d-126">For **Name**, enter a friendly name to identify the vault.</span></span> <span data-ttu-id="c150d-127">Název musí být jedinečný v rámci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c150d-127">The name needs to be unique for the Azure subscription.</span></span> <span data-ttu-id="c150d-128">Zadejte název v rozsahu 2 až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="c150d-128">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="c150d-129">Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="c150d-129">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="c150d-130">V části **Předplatné** z rozevírací nabídky vyberte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="c150d-130">In the **Subscription** section, use the drop-down menu to choose the Azure subscription.</span></span> <span data-ttu-id="c150d-131">Pokud používáte jenom jedno předplatné, zobrazí se toto předplatné a můžete přejít k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="c150d-131">If you use only one subscription, that subscription appears and you can skip to the next step.</span></span> <span data-ttu-id="c150d-132">Pokud si nejste jisti, jaké předplatné použít, použijte výchozí (nebo navrhované) předplatné.</span><span class="sxs-lookup"><span data-stu-id="c150d-132">If you are not sure which subscription to use, use the default (or suggested) subscription.</span></span> <span data-ttu-id="c150d-133">Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="c150d-133">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="c150d-134">V části **Skupina prostředků**:</span><span class="sxs-lookup"><span data-stu-id="c150d-134">In the **Resource group** section:</span></span>

    * <span data-ttu-id="c150d-135">vyberte **Vytvořit novou**, pokud chcete vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c150d-135">select **Create new** if you want to create a new Resource group.</span></span>
    <span data-ttu-id="c150d-136">Nebo</span><span class="sxs-lookup"><span data-stu-id="c150d-136">Or</span></span>
    * <span data-ttu-id="c150d-137">vyberte **Použít existující** a kliknutím na rozevírací nabídku zobrazte seznam dostupných skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="c150d-137">select **Use existing** and click the drop-down menu to see the available list of Resource groups.</span></span>

  <span data-ttu-id="c150d-138">Kompletní informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c150d-138">For complete information on Resource groups, see the [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="c150d-139">Klikněte na **Oblast** a vyberte zeměpisnou oblast trezoru.</span><span class="sxs-lookup"><span data-stu-id="c150d-139">Click **Location** to select the geographic region for the vault.</span></span> <span data-ttu-id="c150d-140">Tato volba určuje geografickou oblast, kam jsou zasílaná vaše zálohovaná data.</span><span class="sxs-lookup"><span data-stu-id="c150d-140">This choice determines the geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="c150d-141">V dolní části okna trezoru služby Recovery Services klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c150d-141">At the bottom of the Recovery Services vault blade, click **Create**.</span></span>

  <span data-ttu-id="c150d-142">Vytvoření trezoru služby Recovery Services může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="c150d-142">It can take several minutes for the Recovery Services vault to be created.</span></span> <span data-ttu-id="c150d-143">Sledujte oznámení o stavu v pravé horní části portálu.</span><span class="sxs-lookup"><span data-stu-id="c150d-143">Monitor the status notifications in the upper right-hand area of the portal.</span></span> <span data-ttu-id="c150d-144">Když je trezor vytvořený, zobrazí se v seznamu trezorů Služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="c150d-144">Once your vault is created, it appears in the list of Recovery Services vaults.</span></span> <span data-ttu-id="c150d-145">Pokud se trezor nezobrazí ani po několika minutách, klikněte na **Obnovit**.</span><span class="sxs-lookup"><span data-stu-id="c150d-145">If after several minutes you don't see your vault, click **Refresh**.</span></span>

  ![Kliknutí na tlačítko Obnovit](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

  <span data-ttu-id="c150d-147">Jakmile se trezor zobrazí v seznamu trezorů služby Recovery Services, jste připraveni nastavit redundanci úložiště.</span><span class="sxs-lookup"><span data-stu-id="c150d-147">Once you see your vault in the list of Recovery Services vaults, you are ready to set the storage redundancy.</span></span>


### <a name="set-storage-redundancy"></a><span data-ttu-id="c150d-148">Sada redundance úložiště</span><span class="sxs-lookup"><span data-stu-id="c150d-148">Set storage redundancy</span></span>
<span data-ttu-id="c150d-149">Při prvním vytvoření trezoru Služeb zotavení určíte, jak má být úložiště replikované.</span><span class="sxs-lookup"><span data-stu-id="c150d-149">When you first create a Recovery Services vault you determine how storage is replicated.</span></span>

1. <span data-ttu-id="c150d-150">V okně **Trezory služby Recovery Services** klikněte na nový trezor.</span><span class="sxs-lookup"><span data-stu-id="c150d-150">From the **Recovery Services vaults** blade, click the new vault.</span></span>

    ![Výběr nového trezoru ze seznamu trezorů služby Recovery Services](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="c150d-152">Po výběru trezoru se okno **Trezory služby Recovery Services** zúží a otevře se okno Nastavení (*s názvem trezoru v horní části*) a okno s podrobnostmi o trezoru.</span><span class="sxs-lookup"><span data-stu-id="c150d-152">When you select the vault, the **Recovery Services vault** blade narrows, and the Settings blade (*which has the name of the vault at the top*) and the vault details blade open.</span></span>

    ![Zobrazení konfigurace úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)

2. <span data-ttu-id="c150d-154">V okně Nastavení nového trezoru pomocí vertikálního posuvníku přejděte dolů do části Správa a klikněte na **Infrastruktura zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c150d-154">In the new vault's Settings blade, use the vertical slide to scroll down to the Manage section, and click **Backup Infrastructure**.</span></span>

  <span data-ttu-id="c150d-155">Otevře se okno Infrastruktura zálohování.</span><span class="sxs-lookup"><span data-stu-id="c150d-155">The Backup Infrastructure blade opens.</span></span>

3. <span data-ttu-id="c150d-156">V okně Infrastruktura zálohování klikněte na **Konfigurace zálohování**. Otevře se okno **Konfigurace zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c150d-156">In the Backup Infrastructure blade, click **Backup Configuration** to open the **Backup Configuration** blade.</span></span>

  ![Nastavení konfigurace úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)

4. <span data-ttu-id="c150d-158">Zvolte vhodnou možnost replikace pro svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="c150d-158">Choose the appropriate storage replication option for your vault.</span></span>

  ![volby konfigurace úložiště](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

  <span data-ttu-id="c150d-160">Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="c150d-160">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="c150d-161">Pokud používáte Azure jako primární koncový bod úložiště záloh, pokračujte v používání **geograficky redundantního** úložiště.</span><span class="sxs-lookup"><span data-stu-id="c150d-161">If you use Azure as a primary backup storage endpoint, continue to use **Geo-redundant**.</span></span> <span data-ttu-id="c150d-162">Pokud Azure nepoužíváte jako primární koncový bod úložiště záloh, vyberte **Místně redundantní** – snížíte tím náklady na úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c150d-162">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces the Azure storage costs.</span></span> <span data-ttu-id="c150d-163">Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště najdete v tomto [přehledu redundance úložiště](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="c150d-163">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="c150d-164">Teď, když jste vytvořili trezor, připravte infrastrukturu k zálohování souborů a složek, stahování a instalace agenta služeb zotavení Microsoft Azure, stáhnete pověření k úložišti a následným použitím tyto přihlašovací údaje k registraci agenta s úložištěm.</span><span class="sxs-lookup"><span data-stu-id="c150d-164">Now that you've created a vault, prepare your infrastructure to back up files and folders by downloading and installing the Microsoft Azure Recovery Services agent, downloading vault credentials, and then using those credentials to register the agent with the vault.</span></span>

## <a name="configure-the-vault"></a><span data-ttu-id="c150d-165">Konfigurace trezoru</span><span class="sxs-lookup"><span data-stu-id="c150d-165">Configure the vault</span></span>

1. <span data-ttu-id="c150d-166">V okně trezoru služby Recovery Services (pro trezor, který jste právě vytvořili) klikněte v části Začínáme na **Zálohovat** a potom v okně **Začínáme se zálohováním** vyberte **Cíl zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c150d-166">On the Recovery Services vault blade (for the vault you just created), in the Getting Started section, click **Backup**, then on the **Getting Started with Backup** blade, select **Backup goal**.</span></span>

  ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

  <span data-ttu-id="c150d-168">Otevře se okno **Cíl zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c150d-168">The **Backup Goal** blade opens.</span></span> <span data-ttu-id="c150d-169">Pokud byl dříve nakonfigurován trezor služeb zotavení, pak se **cíl zálohování** okna otevře po kliknutí na tlačítko **zálohování** na služeb zotavení trezoru okno.</span><span class="sxs-lookup"><span data-stu-id="c150d-169">If the Recovery Services vault has been previously configured, then the **Backup Goal** blades opens when you click **Backup** on the Recovery Services vault blade.</span></span>

  ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="c150d-171">V rozevírací nabídce **Kde běží vaše úlohy?** vyberte **Místní**.</span><span class="sxs-lookup"><span data-stu-id="c150d-171">From the **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

  <span data-ttu-id="c150d-172">Možnost **Místní** jste vybrali proto, že počítačem s Windows Serverem nebo Windows je fyzický počítač, který není v Azure.</span><span class="sxs-lookup"><span data-stu-id="c150d-172">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="c150d-173">V nabídce **Co chcete zálohovat?** vyberte **Soubory a složky** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c150d-173">From the **What do you want to backup?** menu, select **Files and folders**, and click **OK**.</span></span>

  ![Konfigurace souborů a složek](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

  <span data-ttu-id="c150d-175">Po kliknutí na OK se vedle položky **Cíl zálohování** zobrazí zaškrtnutí a otevře se okno **Připravit infrastrukturu**.</span><span class="sxs-lookup"><span data-stu-id="c150d-175">After clicking OK, a checkmark appears next to **Backup goal**, and the **Prepare infrastructure** blade opens.</span></span>

  ![Cíl zálohování je nakonfigurovaný, teď se připraví infrastruktura](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="c150d-177">V okně **Připravit infrastrukturu** klikněte na **Stáhnout agenta pro Windows Server nebo klienta Windows**.</span><span class="sxs-lookup"><span data-stu-id="c150d-177">On the **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

  ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

  <span data-ttu-id="c150d-179">Pokud používáte Windows Server Essential, vyberte ke stažení agenta pro Windows Server Essential.</span><span class="sxs-lookup"><span data-stu-id="c150d-179">If you are using Windows Server Essential, then choose to download the agent for Windows Server Essential.</span></span> <span data-ttu-id="c150d-180">Místní nabídka zobrazí výzvu ke spuštění nebo uložení MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="c150d-180">A pop-up menu prompts you to run or save MARSAgentInstaller.exe.</span></span>

  ![Dialogové okno MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="c150d-182">V místní nabídce stahování klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c150d-182">In the download pop-up menu, click **Save**.</span></span>

  <span data-ttu-id="c150d-183">Ve výchozím nastavení se soubor **MARSagentinstaller.exe** uloží do složky Stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="c150d-183">By default, the **MARSagentinstaller.exe** file is saved to your Downloads folder.</span></span> <span data-ttu-id="c150d-184">Po dokončení instalačního programu se zobrazí automaticky otevírané okno s dotazem, jestli chcete spustit instalační program nebo otevřít složku.</span><span class="sxs-lookup"><span data-stu-id="c150d-184">When the installer completes, you will see a pop-up asking if you want to run the installer, or open the folder.</span></span>

  ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

  <span data-ttu-id="c150d-186">Agenta ještě nemusíte instalovat.</span><span class="sxs-lookup"><span data-stu-id="c150d-186">You don't need to install the agent yet.</span></span> <span data-ttu-id="c150d-187">Agenta můžete nainstalovat po stažení přihlašovacích údajů trezoru.</span><span class="sxs-lookup"><span data-stu-id="c150d-187">You can install the agent after you have downloaded the vault credentials.</span></span>

6. <span data-ttu-id="c150d-188">V okně **Připravit infrastrukturu** klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="c150d-188">On the **Prepare infrastructure** blade, click **Download**.</span></span>

  ![stažení přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

  <span data-ttu-id="c150d-190">Přihlašovací údaje trezoru se stáhnou do složky Stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="c150d-190">The vault credentials download to your Downloads folder.</span></span> <span data-ttu-id="c150d-191">Po dokončení stahování přihlašovacích údajů trezoru se zobrazí automaticky otevírané okno s dotazem, jestli chcete přihlašovací údaje otevřít nebo uložit.</span><span class="sxs-lookup"><span data-stu-id="c150d-191">After the vault credentials finish downloading, you see a pop-up asking if you want to open or save the credentials.</span></span> <span data-ttu-id="c150d-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c150d-192">Click **Save**.</span></span> <span data-ttu-id="c150d-193">Pokud omylem kliknete **Otevřít**, nechte dialogové okno, které se pokusí otevřít přihlašovací údaje trezoru, zobrazit chybu.</span><span class="sxs-lookup"><span data-stu-id="c150d-193">If you accidentally click **Open**, let the dialog that attempts to open the vault credentials, fail.</span></span> <span data-ttu-id="c150d-194">Přihlašovací údaje trezoru nejde otevřít.</span><span class="sxs-lookup"><span data-stu-id="c150d-194">You cannot open the vault credentials.</span></span> <span data-ttu-id="c150d-195">Přejděte k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="c150d-195">Proceed to the next step.</span></span> <span data-ttu-id="c150d-196">Přihlašovací údaje trezoru jsou ve složce Stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="c150d-196">The vault credentials are in the Downloads folder.</span></span>   

  ![dokončené stahování přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a><span data-ttu-id="c150d-198">Instalace a registrace agenta</span><span class="sxs-lookup"><span data-stu-id="c150d-198">Install and register the agent</span></span>

> [!NOTE]
> <span data-ttu-id="c150d-199">Povolení zálohování prostřednictvím webu Azure Portal ještě není dostupné.</span><span class="sxs-lookup"><span data-stu-id="c150d-199">Enabling backup through the Azure portal is not available, yet.</span></span> <span data-ttu-id="c150d-200">K zálohování svých souborů a složek použijte agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="c150d-200">Use the Microsoft Azure Recovery Services Agent to back up your files and folders.</span></span>
>

1. <span data-ttu-id="c150d-201">Ve složce Stažené soubory (nebo ve složce, kterou jste vybrali pro stahování) vyhledejte soubor **MARSagentinstaller.exe** a dvakrát na něj klikněte.</span><span class="sxs-lookup"><span data-stu-id="c150d-201">Locate and double-click the **MARSagentinstaller.exe** from the Downloads folder (or other saved location).</span></span>

  <span data-ttu-id="c150d-202">Instalační program v průběhu extrakce, instalace a registrace agenta Recovery Services zobrazuje řadu zpráv.</span><span class="sxs-lookup"><span data-stu-id="c150d-202">The installer provides a series of messages as it extracts, installs, and registers the Recovery Services agent.</span></span>

  ![spuštění přihlašovacích údajů instalačního programu agenta Recovery Services](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="c150d-204">Dokončete Průvodce instalací agenta Služeb zotavení Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c150d-204">Complete the Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="c150d-205">K dokončení průvodce budete muset:</span><span class="sxs-lookup"><span data-stu-id="c150d-205">To complete the wizard, you need to:</span></span>

  * <span data-ttu-id="c150d-206">Vybrat umístění instalace a složky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c150d-206">Choose a location for the installation and cache folder.</span></span>
  * <span data-ttu-id="c150d-207">Zadat informace o serveru proxy, pokud pro připojování k internetu používáte server proxy.</span><span class="sxs-lookup"><span data-stu-id="c150d-207">Provide your proxy server info if you use a proxy server to connect to the internet.</span></span>
  * <span data-ttu-id="c150d-208">Zadat svoje uživatelské jméno a heslo, pokud používáte ověřený server proxy.</span><span class="sxs-lookup"><span data-stu-id="c150d-208">Provide your user name and password details if you use an authenticated proxy.</span></span>
  * <span data-ttu-id="c150d-209">Zadat stažené přihlašovací údaje trezoru.</span><span class="sxs-lookup"><span data-stu-id="c150d-209">Provide the downloaded vault credentials</span></span>
  * <span data-ttu-id="c150d-210">Uložit šifrovací heslo na bezpečné místo.</span><span class="sxs-lookup"><span data-stu-id="c150d-210">Save the encryption passphrase in a secure location.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c150d-211">Pokud heslo ztratíte nebo zapomenete, Microsoft vám nemůže pomoci obnovit zálohovaná data.</span><span class="sxs-lookup"><span data-stu-id="c150d-211">If you lose or forget the passphrase, Microsoft cannot help recover the backup data.</span></span> <span data-ttu-id="c150d-212">Uložte soubor na bezpečné místo.</span><span class="sxs-lookup"><span data-stu-id="c150d-212">Save the file in a secure location.</span></span> <span data-ttu-id="c150d-213">Je požadováno pro obnovení zálohy.</span><span class="sxs-lookup"><span data-stu-id="c150d-213">It is required to restore a backup.</span></span>
  >
  >

<span data-ttu-id="c150d-214">Agent je nyní nainstalovaný a váš počítač je registrovaný k trezoru.</span><span class="sxs-lookup"><span data-stu-id="c150d-214">The agent is now installed and your machine is registered to the vault.</span></span> <span data-ttu-id="c150d-215">Jste připraveni nakonfigurovat a naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="c150d-215">You're ready to configure and schedule your backup.</span></span>

## <a name="network-and-connectivity-requirements"></a><span data-ttu-id="c150d-216">Požadavky na síť a připojení</span><span class="sxs-lookup"><span data-stu-id="c150d-216">Network and Connectivity Requirements</span></span>

<span data-ttu-id="c150d-217">Pokud váš počítač nebo na proxy server má omezený přístup k Internetu, ujistěte se, že nastavení brány firewall na počítači nebo na proxy serveru jsou nakonfigurované na povolit následující adresy URL:</span><span class="sxs-lookup"><span data-stu-id="c150d-217">If your machine/proxy has limited internet access, ensure that firewall settings on the machine/proxy are configured to allow the following URLs:</span></span> <br>
    1. <span data-ttu-id="c150d-218">www.msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="c150d-218">www.msftncsi.com</span></span>
    2. <span data-ttu-id="c150d-219">*.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="c150d-219">*.Microsoft.com</span></span>
    3. <span data-ttu-id="c150d-220">*.WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="c150d-220">*.WindowsAzure.com</span></span>
    4. <span data-ttu-id="c150d-221">*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="c150d-221">*.microsoftonline.com</span></span>
    5. <span data-ttu-id="c150d-222">*.windows.ne</span><span class="sxs-lookup"><span data-stu-id="c150d-222">*.windows.ne</span></span>


## <a name="create-the-backup-policy"></a><span data-ttu-id="c150d-223">Vytvoření zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c150d-223">Create the backup policy</span></span>
<span data-ttu-id="c150d-224">Zásady zálohování je naplánovat, kdy jsou pořizovány body obnovení a dobu, kterou se uchovají body obnovení.</span><span class="sxs-lookup"><span data-stu-id="c150d-224">The backup policy is the schedule when recovery points are taken, and the length of time the recovery points are retained.</span></span> <span data-ttu-id="c150d-225">Pomocí aplikace Microsoft Azure Backup agent můžete vytvořit zásady zálohování pro soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="c150d-225">Use the Microsoft Azure Backup agent to create the backup policy for files and folders.</span></span>

### <a name="to-create-a-backup-schedule"></a><span data-ttu-id="c150d-226">Chcete-li vytvořit plán zálohování</span><span class="sxs-lookup"><span data-stu-id="c150d-226">To create a backup schedule</span></span>
1. <span data-ttu-id="c150d-227">Otevřete Microsoft Azure Backup agent.</span><span class="sxs-lookup"><span data-stu-id="c150d-227">Open the Microsoft Azure Backup agent.</span></span> <span data-ttu-id="c150d-228">Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.</span><span class="sxs-lookup"><span data-stu-id="c150d-228">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Spuštění agenta Azure Backup](./media/backup-configure-vault/snap-in-search.png)
2. <span data-ttu-id="c150d-230">V agentu Backup **akce** podokně klikněte na tlačítko **plánem zálohování** spustíte Průvodce plánem zálohování.</span><span class="sxs-lookup"><span data-stu-id="c150d-230">In the Backup agent's **Actions** pane, click **Schedule Backup** to launch the Schedule Backup Wizard.</span></span>

    ![Naplánování zálohování Windows Serveru](./media/backup-configure-vault/schedule-first-backup.png)

3. <span data-ttu-id="c150d-232">Na **Začínáme** stránky průvodce plánem zálohování, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c150d-232">On the **Getting started** page of the Schedule Backup Wizard, click **Next**.</span></span>
4. <span data-ttu-id="c150d-233">Na **výběr položek k zálohování** klikněte na tlačítko **přidat položky**.</span><span class="sxs-lookup"><span data-stu-id="c150d-233">On the **Select Items to Backup** page, click **Add Items**.</span></span>

  <span data-ttu-id="c150d-234">Otevře se dialogové okno Vybrat položky.</span><span class="sxs-lookup"><span data-stu-id="c150d-234">The Select Items dialog opens.</span></span>

5. <span data-ttu-id="c150d-235">Vyberte soubory a složky, které chcete chránit a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c150d-235">Select the files and folders that you want to protect, and then click **OK**.</span></span>
6. <span data-ttu-id="c150d-236">V **výběr položek k zálohování** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c150d-236">In the **Select Items to Backup** page, click **Next**.</span></span>
7. <span data-ttu-id="c150d-237">Na **zadání plánu zálohování** , zadat plán zálohování a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c150d-237">On the **Specify Backup Schedule** page, specify the backup schedule and click **Next**.</span></span>

    <span data-ttu-id="c150d-238">Můžete naplánovat denní (probíhající maximálně třikrát za den) nebo týdenní zálohování.</span><span class="sxs-lookup"><span data-stu-id="c150d-238">You can schedule daily (at a maximum rate of three times per day) or weekly backups.</span></span>

    ![Položky k zálohování z Windows Serveru](./media/backup-configure-vault/specify-backup-schedule-close.png)

   > [!NOTE]
   > <span data-ttu-id="c150d-240">Další informace o tom, jak zadat plán zálohování, naleznete v článku [Use Azure Backup to replace your tape infrastructure](backup-azure-backup-cloud-as-tape.md) (Použití služby Azure Backup k nahrazení páskové infrastruktury).</span><span class="sxs-lookup"><span data-stu-id="c150d-240">For more information about how to specify the backup schedule, see the article [Use Azure Backup to replace your tape infrastructure](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >

8. <span data-ttu-id="c150d-241">Na **výběr zásady uchovávání informací** vyberte zásady uchovávání informací konkrétní pro zálohování kopírováním a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c150d-241">On the **Select Retention Policy** page, choose the specific retention policies the for the backup copy and click **Next**.</span></span>

    <span data-ttu-id="c150d-242">Zásady uchovávání informací Určuje dobu, která je uložena záloha.</span><span class="sxs-lookup"><span data-stu-id="c150d-242">The retention policy specifies the duration which the backup is stored.</span></span> <span data-ttu-id="c150d-243">Místo zadání „ploché zásady“ pro všechny body zálohy můžete zadat různé zásady uchovávání informací v závislosti na tom, kdy dochází k zálohování.</span><span class="sxs-lookup"><span data-stu-id="c150d-243">Rather than just specifying a “flat policy” for all backup points, you can specify different retention policies based on when the backup occurs.</span></span> <span data-ttu-id="c150d-244">Podle svých potřeb můžete upravit denní, týdenní, měsíční a roční zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="c150d-244">You can modify the daily, weekly, monthly, and yearly retention policies to meet your needs.</span></span>
9. <span data-ttu-id="c150d-245">Na stránce Výběr typu prvotní zálohy vyberte typ prvotní zálohy.</span><span class="sxs-lookup"><span data-stu-id="c150d-245">On the Choose Initial Backup Type page, choose the initial backup type.</span></span> <span data-ttu-id="c150d-246">Ponechejte vybranou možnost **Automaticky přes síť** a poté klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c150d-246">Leave the option **Automatically over the network** selected, and then click **Next**.</span></span>

    <span data-ttu-id="c150d-247">Zálohovat můžete automaticky přes síť nebo offline.</span><span class="sxs-lookup"><span data-stu-id="c150d-247">You can back up automatically over the network, or you can back up offline.</span></span> <span data-ttu-id="c150d-248">Zbývající část tohoto článku popisuje proces automatického zálohování.</span><span class="sxs-lookup"><span data-stu-id="c150d-248">The remainder of this article describes the process for backing up automatically.</span></span> <span data-ttu-id="c150d-249">Pokud upřednostňujete offline zálohování, přečtěte si článek [Pracovní postup offline zálohování v Azure Backup](backup-azure-backup-import-export.md), kde naleznete další informace.</span><span class="sxs-lookup"><span data-stu-id="c150d-249">If you prefer to do an offline backup, review the article [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) for additional information.</span></span>
10. <span data-ttu-id="c150d-250">Na stránce Potvrzení zkontrolujte informace a poté klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c150d-250">On the Confirmation page, review the information, and then click **Finish**.</span></span>
11. <span data-ttu-id="c150d-251">Až průvodce dokončí vytváření plánu zálohování, klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="c150d-251">After the wizard finishes creating the backup schedule, click **Close**.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="c150d-252">Povolit omezení šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="c150d-252">Enable network throttling</span></span>
<span data-ttu-id="c150d-253">Agent služby Microsoft Azure Backup poskytuje, omezení šířky pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="c150d-253">The Microsoft Azure Backup agent provides network throttling.</span></span> <span data-ttu-id="c150d-254">Omezení využití šířky pásma sítě při přenosu dat ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="c150d-254">Throttling controls how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="c150d-255">Tento ovládací prvek může být užitečné, pokud potřebujete zálohovat data v pracovní době, ale nechcete, aby proces zálohování narušoval ostatní internetový provoz.</span><span class="sxs-lookup"><span data-stu-id="c150d-255">This control can be helpful if you need to back up data during work hours but do not want the backup process to interfere with other Internet traffic.</span></span> <span data-ttu-id="c150d-256">Omezení se vztahuje na zálohování a obnovení aktivity.</span><span class="sxs-lookup"><span data-stu-id="c150d-256">Throttling applies to back up and restore activities.</span></span>

> [!NOTE]
> <span data-ttu-id="c150d-257">Omezení šířky pásma sítě není k dispozici v systému Windows Server 2008 R2 SP1, Windows Server 2008 SP2 nebo Windows 7 (s aktualizací service Pack).</span><span class="sxs-lookup"><span data-stu-id="c150d-257">Network throttling is not available on Windows Server 2008 R2 SP1, Windows Server 2008 SP2, or Windows 7 (with service packs).</span></span> <span data-ttu-id="c150d-258">Omezení šířky funkce sítě Azure Backup zapojí Quality of Service (QoS) místního operačního systému.</span><span class="sxs-lookup"><span data-stu-id="c150d-258">The Azure Backup network throttling feature engages Quality of Service (QoS) on the local operating system.</span></span> <span data-ttu-id="c150d-259">I když Azure Backup dokáže ochránit těchto operačních systémů, verze technologie QoS, které jsou k dispozici na těchto platformách nefunguje s omezení sítě Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="c150d-259">Though Azure Backup can protect these operating systems, the version of QoS available on these platforms doesn't work with Azure Backup network throttling.</span></span> <span data-ttu-id="c150d-260">Omezení šířky pásma sítě můžete použít na jiných [podporované operační systémy](backup-azure-backup-faq.md).</span><span class="sxs-lookup"><span data-stu-id="c150d-260">Network throttling can be used on all other [supported operating systems](backup-azure-backup-faq.md).</span></span>
>
>

<span data-ttu-id="c150d-261">**Chcete-li povolit omezení šířky pásma sítě**</span><span class="sxs-lookup"><span data-stu-id="c150d-261">**To enable network throttling**</span></span>

1. <span data-ttu-id="c150d-262">V agentovi nástroje Microsoft Azure Backup, klikněte na tlačítko **změnit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c150d-262">In the Microsoft Azure Backup agent, click **Change Properties**.</span></span>

    ![Změnit vlastnosti](./media/backup-configure-vault/change-properties.png)
2. <span data-ttu-id="c150d-264">Na **omezování** vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="c150d-264">On the **Throttling** tab, select the **Enable internet bandwidth usage throttling for backup operations** check box.</span></span>

    ![Omezení šířky pásma sítě](./media/backup-configure-vault/throttling-dialog.png)
3. <span data-ttu-id="c150d-266">Po povolení omezení šířky pásma, zadejte povolenou šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.</span><span class="sxs-lookup"><span data-stu-id="c150d-266">After you have enabled throttling, specify the allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="c150d-267">Hodnoty šířky pásma začít ve 512 kilobitů za sekundu (Kbps) a můžete přejít až 1,023 MB za sekundu (MBps).</span><span class="sxs-lookup"><span data-stu-id="c150d-267">The bandwidth values begin at 512 kilobits per second (Kbps) and can go up to 1,023 megabytes per second (MBps).</span></span> <span data-ttu-id="c150d-268">Můžete také určit spuštění a dokončení pro **pracovní hodiny**, a které dny v týdnu, jsou považovány pracovní dny.</span><span class="sxs-lookup"><span data-stu-id="c150d-268">You can also designate the start and finish for **Work hours**, and which days of the week are considered work days.</span></span> <span data-ttu-id="c150d-269">Hodiny mimo určené práce, kterou jsou považovány za hodiny mimo pracovní hodiny.</span><span class="sxs-lookup"><span data-stu-id="c150d-269">Hours outside of designated work hours are considered non-work hours.</span></span>
4. <span data-ttu-id="c150d-270">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c150d-270">Click **OK**.</span></span>

### <a name="to-back-up-files-and-folders-for-the-first-time"></a><span data-ttu-id="c150d-271">První zálohování souborů a složek</span><span class="sxs-lookup"><span data-stu-id="c150d-271">To back up files and folders for the first time</span></span>
1. <span data-ttu-id="c150d-272">V agenta zálohování, klikněte na **zálohovat nyní** dokončit prvotní synchronizaci přes síť.</span><span class="sxs-lookup"><span data-stu-id="c150d-272">In the backup agent, click **Back Up Now** to complete the initial seeding over the network.</span></span>

    ![Zálohovat nyní ve Windows Serveru](./media/backup-configure-vault/backup-now.png)
2. <span data-ttu-id="c150d-274">Na stránce Potvrzení zkontrolujte nastavení, které Průvodce Zálohování nyní použije k zálohování počítače.</span><span class="sxs-lookup"><span data-stu-id="c150d-274">On the Confirmation page, review the settings that the Back Up Now Wizard will use to back up the machine.</span></span> <span data-ttu-id="c150d-275">Poté klikněte na **Zálohovat**.</span><span class="sxs-lookup"><span data-stu-id="c150d-275">Then click **Back Up**.</span></span>
3. <span data-ttu-id="c150d-276">Průvodce zavřete kliknutím na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="c150d-276">Click **Close** to close the wizard.</span></span> <span data-ttu-id="c150d-277">Pokud to uděláte před dokončením procesu zálohování, průvodce zůstane spuštěný v pozadí.</span><span class="sxs-lookup"><span data-stu-id="c150d-277">If you do this before the backup process finishes, the wizard continues to run in the background.</span></span>

<span data-ttu-id="c150d-278">Po dokončení prvotní zálohy se v konzole Zálohování zobrazí stav **Úloha byla dokončena**.</span><span class="sxs-lookup"><span data-stu-id="c150d-278">After the initial backup is completed, the **Job completed** status appears in the Backup console.</span></span>

![Dokončení IR](./media/backup-configure-vault/ircomplete.png)

## <a name="questions"></a><span data-ttu-id="c150d-280">Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="c150d-280">Questions?</span></span>
<span data-ttu-id="c150d-281">Máte-li nějaké dotazy nebo pokud víte o funkci, kterou byste uvítali, [odešlete nám svůj názor](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="c150d-281">If you have questions, or if there is any feature that you would like to see included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c150d-282">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c150d-282">Next steps</span></span>
<span data-ttu-id="c150d-283">Další informace o zálohování virtuálních počítačů nebo dalších úloh najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="c150d-283">For additional information about backing up VMs or other workloads, see:</span></span>

* <span data-ttu-id="c150d-284">Teď, když jste zálohovali své soubory a složky, můžete [spravovat svoje trezory a servery](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="c150d-284">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="c150d-285">Potřebujete-li obnovit zálohu, použijte tento článek k [obnovení souborů na počítač se systémem Windows](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="c150d-285">If you need to restore a backup, use this article to [restore files to a Windows machine](backup-azure-restore-windows-server.md).</span></span>
