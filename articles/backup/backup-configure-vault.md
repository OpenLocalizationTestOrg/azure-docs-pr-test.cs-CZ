---
title: "tooback agenta Azure Backup aaaUse soubory a složky | Microsoft Docs"
description: "Použijte hello Microsoft Azure Backup agent tooback až tooAzure soubory a složky systému Windows. Vytvoření trezoru služeb zotavení, nainstalujte agenta zálohování hello, definovat zásady zálohování hello a spusťte prvotní zálohování hello hello souborů a složek."
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
ms.openlocfilehash: be203c24841971872b5c6e7cf260a2fa5c58ac47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-windows-server-or-client-tooazure-using-hello-resource-manager-deployment-model"></a><span data-ttu-id="33d5b-105">Zálohování Windows serveru nebo klienta tooAzure pomocí modelu nasazení Resource Manager hello</span><span class="sxs-lookup"><span data-stu-id="33d5b-105">Back up a Windows Server or client tooAzure using hello Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="33d5b-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="33d5b-106">Azure portal</span></span>](backup-configure-vault.md)
> * [<span data-ttu-id="33d5b-107">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="33d5b-107">Classic portal</span></span>](backup-configure-vault-classic.md)
>
>

<span data-ttu-id="33d5b-108">Tento článek vysvětluje, jak hello soubory a složky tooAzure tooback Windows serveru (nebo klienta Windows) s Azure Backup pomocí modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="33d5b-108">This article explains how tooback up your Windows Server (or Windows client) files and folders tooAzure with Azure Backup using hello Resource Manager deployment model.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/backup-deployment-models.md)]

![Kroky procesu zálohování](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a><span data-ttu-id="33d5b-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="33d5b-110">Before you start</span></span>
<span data-ttu-id="33d5b-111">tooback serveru nebo klienta tooAzure, potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="33d5b-111">tooback up a server or client tooAzure, you need an Azure account.</span></span> <span data-ttu-id="33d5b-112">Pokud nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="33d5b-112">If you don't have one, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="33d5b-113">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="33d5b-113">Create a Recovery Services vault</span></span>
<span data-ttu-id="33d5b-114">Trezor služeb zotavení je entita, která ukládá všechny hello zálohy a body obnovení, které vytvoříte v čase.</span><span class="sxs-lookup"><span data-stu-id="33d5b-114">A Recovery Services vault is an entity that stores all hello backups and recovery points you create over time.</span></span> <span data-ttu-id="33d5b-115">Hello trezor služeb zotavení obsahuje také hello zásady zálohování použije toohello chráněné soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="33d5b-115">hello Recovery Services vault also contains hello backup policy applied toohello protected files and folders.</span></span> <span data-ttu-id="33d5b-116">Při vytváření trezoru služeb zotavení, je také možnost hello příslušné úložiště redundance.</span><span class="sxs-lookup"><span data-stu-id="33d5b-116">When you create a Recovery Services vault, you should also select hello appropriate storage redundancy option.</span></span>

### <a name="toocreate-a-recovery-services-vault"></a><span data-ttu-id="33d5b-117">toocreate trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="33d5b-117">toocreate a Recovery Services vault</span></span>
1. <span data-ttu-id="33d5b-118">Pokud jste tak již neučinili, přihlaste se toohello [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="33d5b-118">If you haven't already done so, sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="33d5b-119">V nabídce centra hello, klikněte na tlačítko **další služby** a v hello seznamu prostředků zadejte **služeb zotavení** a klikněte na tlačítko **trezory služeb zotavení**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-119">On hello Hub menu, click **More services** and in hello list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="33d5b-121">Pokud jsou v rámci předplatného hello trezory služeb zotavení, jsou uvedeny hello trezorů.</span><span class="sxs-lookup"><span data-stu-id="33d5b-121">If there are recovery services vaults in hello subscription, hello vaults are listed.</span></span>

3. <span data-ttu-id="33d5b-122">Na hello **trezory služeb zotavení** nabídky, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-122">On hello **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="33d5b-124">Služby zotavení Hello otevře se okno trezoru výzvou tooprovide **název**, **předplatné**, **skupiny prostředků**, a **umístění**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-124">hello Recovery Services vault blade opens, prompting you tooprovide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="33d5b-126">Pro **název**, zadejte popisný název tooidentify hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="33d5b-126">For **Name**, enter a friendly name tooidentify hello vault.</span></span> <span data-ttu-id="33d5b-127">Název Hello musí toobe jedinečný pro hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="33d5b-127">hello name needs toobe unique for hello Azure subscription.</span></span> <span data-ttu-id="33d5b-128">Zadejte název v rozsahu 2 až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="33d5b-128">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="33d5b-129">Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="33d5b-129">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="33d5b-130">V hello **předplatné** pomocí hello rozevírací nabídky toochoose hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="33d5b-130">In hello **Subscription** section, use hello drop-down menu toochoose hello Azure subscription.</span></span> <span data-ttu-id="33d5b-131">Pokud chcete použít jenom jedno předplatné, zobrazí se toto předplatné a toohello další krok můžete vynechat.</span><span class="sxs-lookup"><span data-stu-id="33d5b-131">If you use only one subscription, that subscription appears and you can skip toohello next step.</span></span> <span data-ttu-id="33d5b-132">Pokud si nejste jisti, jaké předplatné toouse, použít výchozí hello (nebo navrhované) předplatné.</span><span class="sxs-lookup"><span data-stu-id="33d5b-132">If you are not sure which subscription toouse, use hello default (or suggested) subscription.</span></span> <span data-ttu-id="33d5b-133">Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="33d5b-133">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="33d5b-134">V hello **skupiny prostředků** části:</span><span class="sxs-lookup"><span data-stu-id="33d5b-134">In hello **Resource group** section:</span></span>

    * <span data-ttu-id="33d5b-135">Vyberte **vytvořit nový** Pokud chcete, aby toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="33d5b-135">select **Create new** if you want toocreate a new Resource group.</span></span>
    <span data-ttu-id="33d5b-136">Nebo</span><span class="sxs-lookup"><span data-stu-id="33d5b-136">Or</span></span>
    * <span data-ttu-id="33d5b-137">Vyberte **použít existující** a klikněte na tlačítko hello rozevírací nabídky toosee hello seznam dostupných skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="33d5b-137">select **Use existing** and click hello drop-down menu toosee hello available list of Resource groups.</span></span>

  <span data-ttu-id="33d5b-138">Úplné informace o skupinách prostředků najdete v tématu hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="33d5b-138">For complete information on Resource groups, see hello [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="33d5b-139">Klikněte na tlačítko **umístění** tooselect hello zeměpisnou oblast trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-139">Click **Location** tooselect hello geographic region for hello vault.</span></span> <span data-ttu-id="33d5b-140">Tato volba určuje hello zeměpisnou oblast, kde vaše zálohovaná data se odesílají.</span><span class="sxs-lookup"><span data-stu-id="33d5b-140">This choice determines hello geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="33d5b-141">Hello dolní části hello okno trezoru služeb zotavení, klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-141">At hello bottom of hello Recovery Services vault blade, click **Create**.</span></span>

  <span data-ttu-id="33d5b-142">Můžete to trvat několik minut, než hello toobe vytvořit trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="33d5b-142">It can take several minutes for hello Recovery Services vault toobe created.</span></span> <span data-ttu-id="33d5b-143">Sledujte oznámení stavu hello v horním pravém oblasti hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="33d5b-143">Monitor hello status notifications in hello upper right-hand area of hello portal.</span></span> <span data-ttu-id="33d5b-144">Po vytvoření svůj trezor se zobrazí v seznamu hello trezorů služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="33d5b-144">Once your vault is created, it appears in hello list of Recovery Services vaults.</span></span> <span data-ttu-id="33d5b-145">Pokud se trezor nezobrazí ani po několika minutách, klikněte na **Obnovit**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-145">If after several minutes you don't see your vault, click **Refresh**.</span></span>

  ![Kliknutí na tlačítko Obnovit](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

  <span data-ttu-id="33d5b-147">Jakmile se zobrazí váš trezor hello seznamu trezorů služeb zotavení, jste redundance úložiště připravené tooset hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-147">Once you see your vault in hello list of Recovery Services vaults, you are ready tooset hello storage redundancy.</span></span>


### <a name="set-storage-redundancy"></a><span data-ttu-id="33d5b-148">Sada redundance úložiště</span><span class="sxs-lookup"><span data-stu-id="33d5b-148">Set storage redundancy</span></span>
<span data-ttu-id="33d5b-149">Při prvním vytvoření trezoru Služeb zotavení určíte, jak má být úložiště replikované.</span><span class="sxs-lookup"><span data-stu-id="33d5b-149">When you first create a Recovery Services vault you determine how storage is replicated.</span></span>

1. <span data-ttu-id="33d5b-150">Z hello **trezory služeb zotavení** okně klikněte na tlačítko Nový trezor hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-150">From hello **Recovery Services vaults** blade, click hello new vault.</span></span>

    ![Vyberte nový trezor hello ze seznamu hello trezor služeb zotavení](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="33d5b-152">Když vyberete hello trezoru, hello **trezor služeb zotavení** narrows okno a okno nastavení hello (*jehož hello název trezoru hello v horní části hello*) a otevřete okno Podrobnosti trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-152">When you select hello vault, hello **Recovery Services vault** blade narrows, and hello Settings blade (*which has hello name of hello vault at hello top*) and hello vault details blade open.</span></span>

    ![Zobrazení hello konfiguraci úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)

2. <span data-ttu-id="33d5b-154">V okně Nastavení hello nový trezor, použijte hello svislé snímku tooscroll dolů toohello části Správa a klikněte na tlačítko **infrastruktura zálohování**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-154">In hello new vault's Settings blade, use hello vertical slide tooscroll down toohello Manage section, and click **Backup Infrastructure**.</span></span>

  <span data-ttu-id="33d5b-155">Otevře se okno infrastruktura zálohování Hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-155">hello Backup Infrastructure blade opens.</span></span>

3. <span data-ttu-id="33d5b-156">V okně infrastruktura zálohování hello, klikněte na tlačítko **konfigurace zálohování** tooopen hello **konfigurace zálohování** okno.</span><span class="sxs-lookup"><span data-stu-id="33d5b-156">In hello Backup Infrastructure blade, click **Backup Configuration** tooopen hello **Backup Configuration** blade.</span></span>

  ![Nastavit hello konfiguraci úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)

4. <span data-ttu-id="33d5b-158">Zvolte hello vhodnou možnost replikace pro svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="33d5b-158">Choose hello appropriate storage replication option for your vault.</span></span>

  ![volby konfigurace úložiště](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

  <span data-ttu-id="33d5b-160">Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="33d5b-160">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="33d5b-161">Pokud používáte Azure jako koncový bod primární úložiště záloh, pokračujte toouse **geograficky redundantní**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-161">If you use Azure as a primary backup storage endpoint, continue toouse **Geo-redundant**.</span></span> <span data-ttu-id="33d5b-162">Pokud nepoužíváte Azure jako koncový bod primární úložiště záloh, zvolte **místně redundantní**, což snižuje náklady na úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-162">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces hello Azure storage costs.</span></span> <span data-ttu-id="33d5b-163">Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště najdete v tomto [přehledu redundance úložiště](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="33d5b-163">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="33d5b-164">Teď, když jste vytvořili trezor, Příprava stahování a instalace agenta služeb zotavení Microsoft Azure hello, stáhnete pověření k úložišti a potom pomocí těchto pověření tooregister hello agenta s vaší infrastruktury tooback soubory a složky Trezor Hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-164">Now that you've created a vault, prepare your infrastructure tooback up files and folders by downloading and installing hello Microsoft Azure Recovery Services agent, downloading vault credentials, and then using those credentials tooregister hello agent with hello vault.</span></span>

## <a name="configure-hello-vault"></a><span data-ttu-id="33d5b-165">Konfigurace hello trezoru</span><span class="sxs-lookup"><span data-stu-id="33d5b-165">Configure hello vault</span></span>

1. <span data-ttu-id="33d5b-166">Na hello okno trezoru služeb zotavení (pro hello trezor jste právě vytvořili), v části Začínáme hello, klikněte na tlačítko **zálohování**, pak na hello **Začínáme se zálohováním** vyberte  **Cíle zálohování**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-166">On hello Recovery Services vault blade (for hello vault you just created), in hello Getting Started section, click **Backup**, then on hello **Getting Started with Backup** blade, select **Backup goal**.</span></span>

  ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

  <span data-ttu-id="33d5b-168">Hello **cíl zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="33d5b-168">hello **Backup Goal** blade opens.</span></span> <span data-ttu-id="33d5b-169">Pokud byl dříve nakonfigurován hello trezor služeb zotavení, pak hello **cíl zálohování** okna otevře po kliknutí na tlačítko **zálohování** na služeb zotavení hello trezoru okno.</span><span class="sxs-lookup"><span data-stu-id="33d5b-169">If hello Recovery Services vault has been previously configured, then hello **Backup Goal** blades opens when you click **Backup** on hello Recovery Services vault blade.</span></span>

  ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="33d5b-171">Z hello **kde běží vaše úloha?** rozevírací nabídky vyberte **místní**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-171">From hello **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

  <span data-ttu-id="33d5b-172">Možnost **Místní** jste vybrali proto, že počítačem s Windows Serverem nebo Windows je fyzický počítač, který není v Azure.</span><span class="sxs-lookup"><span data-stu-id="33d5b-172">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="33d5b-173">Z hello **co chcete toobackup?** nabídce vyberte možnost **soubory a složky**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-173">From hello **What do you want toobackup?** menu, select **Files and folders**, and click **OK**.</span></span>

  ![Konfigurace souborů a složek](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

  <span data-ttu-id="33d5b-175">Po kliknutí na tlačítko OK, objeví se zaškrtnutí další příliš**cíl zálohování**a hello **připravit infrastrukturu** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="33d5b-175">After clicking OK, a checkmark appears next too**Backup goal**, and hello **Prepare infrastructure** blade opens.</span></span>

  ![Cíl zálohování je nakonfigurovaný, teď se připraví infrastruktura](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="33d5b-177">Na hello **připravit infrastrukturu** okně klikněte na tlačítko **stáhnout agenta pro Windows Server nebo klienta Windows**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-177">On hello **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

  ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

  <span data-ttu-id="33d5b-179">Pokud používáte Windows Server nezbytné, zvolte toodownload hello agenta pro Windows Server nezbytné.</span><span class="sxs-lookup"><span data-stu-id="33d5b-179">If you are using Windows Server Essential, then choose toodownload hello agent for Windows Server Essential.</span></span> <span data-ttu-id="33d5b-180">Místní nabídky vás vyzve k toorun nebo uložit MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="33d5b-180">A pop-up menu prompts you toorun or save MARSAgentInstaller.exe.</span></span>

  ![Dialogové okno MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="33d5b-182">V místní nabídce hello stahování, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-182">In hello download pop-up menu, click **Save**.</span></span>

  <span data-ttu-id="33d5b-183">Ve výchozím nastavení, hello **MARSagentinstaller.exe** soubor je uložen tooyour složky se staženými soubory.</span><span class="sxs-lookup"><span data-stu-id="33d5b-183">By default, hello **MARSagentinstaller.exe** file is saved tooyour Downloads folder.</span></span> <span data-ttu-id="33d5b-184">Po dokončení instalačního programu hello, zobrazí se automaticky otevírané okno s dotazem, pokud má instalační program hello toorun nebo otevřete složku hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-184">When hello installer completes, you will see a pop-up asking if you want toorun hello installer, or open hello folder.</span></span>

  ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

  <span data-ttu-id="33d5b-186">Tooinstall hello agent ještě nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="33d5b-186">You don't need tooinstall hello agent yet.</span></span> <span data-ttu-id="33d5b-187">Po stažení hello přihlašovací údaje trezoru můžete nainstalovat agenta hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-187">You can install hello agent after you have downloaded hello vault credentials.</span></span>

6. <span data-ttu-id="33d5b-188">Na hello **připravit infrastrukturu** okně klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-188">On hello **Prepare infrastructure** blade, click **Download**.</span></span>

  ![stažení přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

  <span data-ttu-id="33d5b-190">přihlašovací údaje trezoru Hello tooyour stahování složka pro stahování.</span><span class="sxs-lookup"><span data-stu-id="33d5b-190">hello vault credentials download tooyour Downloads folder.</span></span> <span data-ttu-id="33d5b-191">Po dokončení stahování přihlašovací údaje trezoru hello zobrazí automaticky otevírané okno s dotazem, pokud mají být tooopen nebo uložit přihlašovací údaje hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-191">After hello vault credentials finish downloading, you see a pop-up asking if you want tooopen or save hello credentials.</span></span> <span data-ttu-id="33d5b-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-192">Click **Save**.</span></span> <span data-ttu-id="33d5b-193">Pokud omylem kliknete **otevřete**, umožní hello dialog, který se pokusí přihlašovací údaje trezoru hello tooopen, nezdaří.</span><span class="sxs-lookup"><span data-stu-id="33d5b-193">If you accidentally click **Open**, let hello dialog that attempts tooopen hello vault credentials, fail.</span></span> <span data-ttu-id="33d5b-194">Nelze otevřít přihlašovací údaje trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-194">You cannot open hello vault credentials.</span></span> <span data-ttu-id="33d5b-195">Pokračujte dalším krokem toohello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-195">Proceed toohello next step.</span></span> <span data-ttu-id="33d5b-196">přihlašovací údaje trezoru Hello jsou ve složce pro stahování hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-196">hello vault credentials are in hello Downloads folder.</span></span>   

  ![dokončené stahování přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a><span data-ttu-id="33d5b-198">Instalace a registrace agenta hello</span><span class="sxs-lookup"><span data-stu-id="33d5b-198">Install and register hello agent</span></span>

> [!NOTE]
> <span data-ttu-id="33d5b-199">Povolení zálohování prostřednictvím portálu Azure hello ještě není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="33d5b-199">Enabling backup through hello Azure portal is not available, yet.</span></span> <span data-ttu-id="33d5b-200">Použijte agenta služeb zotavení Microsoft Azure tooback hello soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="33d5b-200">Use hello Microsoft Azure Recovery Services Agent tooback up your files and folders.</span></span>
>

1. <span data-ttu-id="33d5b-201">Vyhledejte a dvakrát klikněte na hello **MARSagentinstaller.exe** z hello stahování složky (nebo jiného uloženého umístění).</span><span class="sxs-lookup"><span data-stu-id="33d5b-201">Locate and double-click hello **MARSagentinstaller.exe** from hello Downloads folder (or other saved location).</span></span>

  <span data-ttu-id="33d5b-202">Instalační program Hello poskytuje řadu zprávy jako extrahuje, nainstaluje a zaregistruje hello agenta služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="33d5b-202">hello installer provides a series of messages as it extracts, installs, and registers hello Recovery Services agent.</span></span>

  ![spuštění přihlašovacích údajů instalačního programu agenta Recovery Services](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="33d5b-204">Hello dokončení Průvodce instalací agenta služeb zotavení Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="33d5b-204">Complete hello Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="33d5b-205">toocomplete hello průvodce, budete muset:</span><span class="sxs-lookup"><span data-stu-id="33d5b-205">toocomplete hello wizard, you need to:</span></span>

  * <span data-ttu-id="33d5b-206">Vyberte umístění složky pro instalaci a mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-206">Choose a location for hello installation and cache folder.</span></span>
  * <span data-ttu-id="33d5b-207">Pokud používáte proxy serveru tooconnect toohello zadejte informace o serveru vašeho proxy Internetu.</span><span class="sxs-lookup"><span data-stu-id="33d5b-207">Provide your proxy server info if you use a proxy server tooconnect toohello internet.</span></span>
  * <span data-ttu-id="33d5b-208">Zadat svoje uživatelské jméno a heslo, pokud používáte ověřený server proxy.</span><span class="sxs-lookup"><span data-stu-id="33d5b-208">Provide your user name and password details if you use an authenticated proxy.</span></span>
  * <span data-ttu-id="33d5b-209">Zadejte přihlašovací údaje trezoru stáhnout hello</span><span class="sxs-lookup"><span data-stu-id="33d5b-209">Provide hello downloaded vault credentials</span></span>
  * <span data-ttu-id="33d5b-210">Šifrovací přístupové heslo hello uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="33d5b-210">Save hello encryption passphrase in a secure location.</span></span>

  > [!NOTE]
  > <span data-ttu-id="33d5b-211">Pokud ztratíte nebo zapomenete heslo hello, Microsoft vám nemůže pomoci obnovit zálohovaná data hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-211">If you lose or forget hello passphrase, Microsoft cannot help recover hello backup data.</span></span> <span data-ttu-id="33d5b-212">Hello soubor uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="33d5b-212">Save hello file in a secure location.</span></span> <span data-ttu-id="33d5b-213">Je požadovaná toorestore zálohu.</span><span class="sxs-lookup"><span data-stu-id="33d5b-213">It is required toorestore a backup.</span></span>
  >
  >

<span data-ttu-id="33d5b-214">Hello agent je nyní nainstalovaný a váš počítač je registrovaný toohello trezoru.</span><span class="sxs-lookup"><span data-stu-id="33d5b-214">hello agent is now installed and your machine is registered toohello vault.</span></span> <span data-ttu-id="33d5b-215">Máte připravené tooconfigure a naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="33d5b-215">You're ready tooconfigure and schedule your backup.</span></span>

## <a name="network-and-connectivity-requirements"></a><span data-ttu-id="33d5b-216">Požadavky na síť a připojení</span><span class="sxs-lookup"><span data-stu-id="33d5b-216">Network and Connectivity Requirements</span></span>

<span data-ttu-id="33d5b-217">Pokud váš počítač nebo na proxy server má omezený přístup k Internetu, zajistěte, aby nastavení brány firewall na počítač nebo proxy server hello jsou nakonfigurované tooallow hello následující adresy URL:</span><span class="sxs-lookup"><span data-stu-id="33d5b-217">If your machine/proxy has limited internet access, ensure that firewall settings on hello machine/proxy are configured tooallow hello following URLs:</span></span> <br>
    1. <span data-ttu-id="33d5b-218">www.msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="33d5b-218">www.msftncsi.com</span></span>
    2. <span data-ttu-id="33d5b-219">*.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="33d5b-219">*.Microsoft.com</span></span>
    3. <span data-ttu-id="33d5b-220">*.WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="33d5b-220">*.WindowsAzure.com</span></span>
    4. <span data-ttu-id="33d5b-221">*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="33d5b-221">*.microsoftonline.com</span></span>
    5. <span data-ttu-id="33d5b-222">*.windows.ne</span><span class="sxs-lookup"><span data-stu-id="33d5b-222">*.windows.ne</span></span>


## <a name="create-hello-backup-policy"></a><span data-ttu-id="33d5b-223">Vytvořit zásady zálohování hello</span><span class="sxs-lookup"><span data-stu-id="33d5b-223">Create hello backup policy</span></span>
<span data-ttu-id="33d5b-224">zásady zálohování Hello je plán hello, pokud body obnovení jsou pořízeny a hello dobu, kterou se uchovají body obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-224">hello backup policy is hello schedule when recovery points are taken, and hello length of time hello recovery points are retained.</span></span> <span data-ttu-id="33d5b-225">Použijte hello Microsoft Azure Backup agent toocreate hello zásady zálohování pro soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="33d5b-225">Use hello Microsoft Azure Backup agent toocreate hello backup policy for files and folders.</span></span>

### <a name="toocreate-a-backup-schedule"></a><span data-ttu-id="33d5b-226">toocreate plán zálohování</span><span class="sxs-lookup"><span data-stu-id="33d5b-226">toocreate a backup schedule</span></span>
1. <span data-ttu-id="33d5b-227">Otevřete hello Microsoft Azure Backup agent.</span><span class="sxs-lookup"><span data-stu-id="33d5b-227">Open hello Microsoft Azure Backup agent.</span></span> <span data-ttu-id="33d5b-228">Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.</span><span class="sxs-lookup"><span data-stu-id="33d5b-228">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Spuštění agenta Azure Backup hello](./media/backup-configure-vault/snap-in-search.png)
2. <span data-ttu-id="33d5b-230">V agentu Backup hello **akce** podokně klikněte na tlačítko **plánem zálohování** toolaunch hello Průvodce plánem zálohování.</span><span class="sxs-lookup"><span data-stu-id="33d5b-230">In hello Backup agent's **Actions** pane, click **Schedule Backup** toolaunch hello Schedule Backup Wizard.</span></span>

    ![Naplánování zálohování Windows Serveru](./media/backup-configure-vault/schedule-first-backup.png)

3. <span data-ttu-id="33d5b-232">Na hello **Začínáme** stránku hello Průvodce plánem zálohování, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-232">On hello **Getting started** page of hello Schedule Backup Wizard, click **Next**.</span></span>
4. <span data-ttu-id="33d5b-233">Na hello **vyberte položky tooBackup** klikněte na tlačítko **přidat položky**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-233">On hello **Select Items tooBackup** page, click **Add Items**.</span></span>

  <span data-ttu-id="33d5b-234">Otevře se dialogové okno Vybrat položky Hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-234">hello Select Items dialog opens.</span></span>

5. <span data-ttu-id="33d5b-235">Vyberte hello soubory a složky mají tooprotect a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-235">Select hello files and folders that you want tooprotect, and then click **OK**.</span></span>
6. <span data-ttu-id="33d5b-236">V hello **vyberte položky tooBackup** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-236">In hello **Select Items tooBackup** page, click **Next**.</span></span>
7. <span data-ttu-id="33d5b-237">Na hello **zadání plánu zálohování** zadejte hello plán zálohování a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-237">On hello **Specify Backup Schedule** page, specify hello backup schedule and click **Next**.</span></span>

    <span data-ttu-id="33d5b-238">Můžete naplánovat denní (probíhající maximálně třikrát za den) nebo týdenní zálohování.</span><span class="sxs-lookup"><span data-stu-id="33d5b-238">You can schedule daily (at a maximum rate of three times per day) or weekly backups.</span></span>

    ![Položky k zálohování z Windows Serveru](./media/backup-configure-vault/specify-backup-schedule-close.png)

   > [!NOTE]
   > <span data-ttu-id="33d5b-240">Další informace o tom, jak toospecify hello plán zálohování, najdete v článku hello [pomocí Azure Backup tooreplace infrastruktury pásky](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="33d5b-240">For more information about how toospecify hello backup schedule, see hello article [Use Azure Backup tooreplace your tape infrastructure](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >

8. <span data-ttu-id="33d5b-241">Na hello **výběr zásady uchovávání informací** , zvolte hello zásady hello konkrétní uchování pro záložní kopii hello a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-241">On hello **Select Retention Policy** page, choose hello specific retention policies hello for hello backup copy and click **Next**.</span></span>

    <span data-ttu-id="33d5b-242">Hello zásady uchovávání informací Určuje dobu hello které hello záloha uložená.</span><span class="sxs-lookup"><span data-stu-id="33d5b-242">hello retention policy specifies hello duration which hello backup is stored.</span></span> <span data-ttu-id="33d5b-243">Místo zadání "ploché zásady" pro všechny body zálohy, můžete zadat různé zásady uchovávání informací podle toho, kdy dochází k zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-243">Rather than just specifying a “flat policy” for all backup points, you can specify different retention policies based on when hello backup occurs.</span></span> <span data-ttu-id="33d5b-244">Můžete upravit hello uchování denní, týdenní, měsíční a roční zásady toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="33d5b-244">You can modify hello daily, weekly, monthly, and yearly retention policies toomeet your needs.</span></span>
9. <span data-ttu-id="33d5b-245">Na stránce Výběr typu prvotní zálohy hello vyberte typ prvotní zálohy hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-245">On hello Choose Initial Backup Type page, choose hello initial backup type.</span></span> <span data-ttu-id="33d5b-246">Ponechte možnost hello **automaticky přes síť hello** vybrané a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-246">Leave hello option **Automatically over hello network** selected, and then click **Next**.</span></span>

    <span data-ttu-id="33d5b-247">Můžete zálohovat automaticky přes síť hello nebo můžete zálohovat do offline režimu.</span><span class="sxs-lookup"><span data-stu-id="33d5b-247">You can back up automatically over hello network, or you can back up offline.</span></span> <span data-ttu-id="33d5b-248">Hello zbývající část tohoto článku popisuje proces hello zálohování automaticky.</span><span class="sxs-lookup"><span data-stu-id="33d5b-248">hello remainder of this article describes hello process for backing up automatically.</span></span> <span data-ttu-id="33d5b-249">Pokud dáváte přednost toodo zálohu do offline režimu, přečtěte si článek hello [pracovní postup Offline zálohování v Azure Backup](backup-azure-backup-import-export.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="33d5b-249">If you prefer toodo an offline backup, review hello article [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) for additional information.</span></span>
10. <span data-ttu-id="33d5b-250">Na stránce pro potvrzení hello, zkontrolujte hello informace a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-250">On hello Confirmation page, review hello information, and then click **Finish**.</span></span>
11. <span data-ttu-id="33d5b-251">Po dokončení Průvodce hello vytváření hello plán zálohování, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-251">After hello wizard finishes creating hello backup schedule, click **Close**.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="33d5b-252">Povolit omezení šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="33d5b-252">Enable network throttling</span></span>
<span data-ttu-id="33d5b-253">Microsoft Azure Backup agent Hello poskytuje, omezení šířky pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="33d5b-253">hello Microsoft Azure Backup agent provides network throttling.</span></span> <span data-ttu-id="33d5b-254">Omezení využití šířky pásma sítě při přenosu dat ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="33d5b-254">Throttling controls how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="33d5b-255">Tento ovládací prvek může být užitečné, pokud potřebujete tooback dat během pracovní doby, ale nechcete, aby proces zálohování toointerfere hello s ostatní internetový provoz.</span><span class="sxs-lookup"><span data-stu-id="33d5b-255">This control can be helpful if you need tooback up data during work hours but do not want hello backup process toointerfere with other Internet traffic.</span></span> <span data-ttu-id="33d5b-256">Omezení platí tooback zálohu a obnovte aktivity.</span><span class="sxs-lookup"><span data-stu-id="33d5b-256">Throttling applies tooback up and restore activities.</span></span>

> [!NOTE]
> <span data-ttu-id="33d5b-257">Omezení šířky pásma sítě není k dispozici v systému Windows Server 2008 R2 SP1, Windows Server 2008 SP2 nebo Windows 7 (s aktualizací service Pack).</span><span class="sxs-lookup"><span data-stu-id="33d5b-257">Network throttling is not available on Windows Server 2008 R2 SP1, Windows Server 2008 SP2, or Windows 7 (with service packs).</span></span> <span data-ttu-id="33d5b-258">omezení šířky funkce sítě Azure Backup Hello zapojí Quality of Service (QoS) hello místního operačního systému.</span><span class="sxs-lookup"><span data-stu-id="33d5b-258">hello Azure Backup network throttling feature engages Quality of Service (QoS) on hello local operating system.</span></span> <span data-ttu-id="33d5b-259">I když Azure Backup dokáže ochránit těchto operačních systémů, hello verze technologie QoS, které jsou k dispozici na těchto platformách nefunguje s omezení sítě Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="33d5b-259">Though Azure Backup can protect these operating systems, hello version of QoS available on these platforms doesn't work with Azure Backup network throttling.</span></span> <span data-ttu-id="33d5b-260">Omezení šířky pásma sítě můžete použít na jiných [podporované operační systémy](backup-azure-backup-faq.md).</span><span class="sxs-lookup"><span data-stu-id="33d5b-260">Network throttling can be used on all other [supported operating systems](backup-azure-backup-faq.md).</span></span>
>
>

<span data-ttu-id="33d5b-261">**omezení tooenable sítě**</span><span class="sxs-lookup"><span data-stu-id="33d5b-261">**tooenable network throttling**</span></span>

1. <span data-ttu-id="33d5b-262">V rámci Microsoft Azure Backup agent hello, klikněte na tlačítko **změnit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-262">In hello Microsoft Azure Backup agent, click **Change Properties**.</span></span>

    ![Změnit vlastnosti](./media/backup-configure-vault/change-properties.png)
2. <span data-ttu-id="33d5b-264">Na hello **omezování** karty, vyberte hello **povolit využití šířky pásma Internetu u operací zálohování omezení** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="33d5b-264">On hello **Throttling** tab, select hello **Enable internet bandwidth usage throttling for backup operations** check box.</span></span>

    ![Omezení šířky pásma sítě](./media/backup-configure-vault/throttling-dialog.png)
3. <span data-ttu-id="33d5b-266">Po povolení omezení šířky pásma, zadejte hello povolený šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-266">After you have enabled throttling, specify hello allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="33d5b-267">hodnoty šířky pásma Hello začít ve 512 kilobitů za sekundu (Kbps) a můžete přejít do too1 023 megabajtů za sekundu (MBps).</span><span class="sxs-lookup"><span data-stu-id="33d5b-267">hello bandwidth values begin at 512 kilobits per second (Kbps) and can go up too1,023 megabytes per second (MBps).</span></span> <span data-ttu-id="33d5b-268">Můžete také určit hello zahájení a dokončení pro **pracovní hodiny**, a které dny v týdnu hello jsou považovány pracovní dny.</span><span class="sxs-lookup"><span data-stu-id="33d5b-268">You can also designate hello start and finish for **Work hours**, and which days of hello week are considered work days.</span></span> <span data-ttu-id="33d5b-269">Hodiny mimo určené práce, kterou jsou považovány za hodiny mimo pracovní hodiny.</span><span class="sxs-lookup"><span data-stu-id="33d5b-269">Hours outside of designated work hours are considered non-work hours.</span></span>
4. <span data-ttu-id="33d5b-270">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-270">Click **OK**.</span></span>

### <a name="tooback-up-files-and-folders-for-hello-first-time"></a><span data-ttu-id="33d5b-271">tooback soubory a složky pro hello poprvé</span><span class="sxs-lookup"><span data-stu-id="33d5b-271">tooback up files and folders for hello first time</span></span>
1. <span data-ttu-id="33d5b-272">V hello agenta zálohování, klikněte na **zálohovat nyní** toocomplete hello počáteční synchronizace replik indexů přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-272">In hello backup agent, click **Back Up Now** toocomplete hello initial seeding over hello network.</span></span>

    ![Zálohovat nyní ve Windows Serveru](./media/backup-configure-vault/backup-now.png)
2. <span data-ttu-id="33d5b-274">Na stránce pro potvrzení hello zkontrolujte hello nastavení, která hello zpět si teď Průvodce použije tooback hello počítač.</span><span class="sxs-lookup"><span data-stu-id="33d5b-274">On hello Confirmation page, review hello settings that hello Back Up Now Wizard will use tooback up hello machine.</span></span> <span data-ttu-id="33d5b-275">Poté klikněte na **Zálohovat**.</span><span class="sxs-lookup"><span data-stu-id="33d5b-275">Then click **Back Up**.</span></span>
3. <span data-ttu-id="33d5b-276">Klikněte na tlačítko **Zavřít** tooclose hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="33d5b-276">Click **Close** tooclose hello wizard.</span></span> <span data-ttu-id="33d5b-277">Pokud to uděláte před dokončením procesu zálohování hello, bude pokračovat hello Průvodce toorun hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="33d5b-277">If you do this before hello backup process finishes, hello wizard continues toorun in hello background.</span></span>

<span data-ttu-id="33d5b-278">Po dokončení prvotní zálohování hello hello **úloha byla dokončena** stav se zobrazí v konzole zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="33d5b-278">After hello initial backup is completed, hello **Job completed** status appears in hello Backup console.</span></span>

![Dokončení IR](./media/backup-configure-vault/ircomplete.png)

## <a name="questions"></a><span data-ttu-id="33d5b-280">Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="33d5b-280">Questions?</span></span>
<span data-ttu-id="33d5b-281">Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="33d5b-281">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="33d5b-282">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33d5b-282">Next steps</span></span>
<span data-ttu-id="33d5b-283">Další informace o zálohování virtuálních počítačů nebo dalších úloh najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="33d5b-283">For additional information about backing up VMs or other workloads, see:</span></span>

* <span data-ttu-id="33d5b-284">Teď, když jste zálohovali své soubory a složky, můžete [spravovat svoje trezory a servery](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="33d5b-284">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="33d5b-285">Pokud potřebujete toorestore zálohu, použijte tento článek příliš[obnovit soubory tooa Windows počítač](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="33d5b-285">If you need toorestore a backup, use this article too[restore files tooa Windows machine](backup-azure-restore-windows-server.md).</span></span>
