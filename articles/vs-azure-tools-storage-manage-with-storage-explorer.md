---
title: "aaaGet spuštění pomocí Storage Exploreru (Preview) | Microsoft Docs"
description: "Správa prostředků úložiště Azure Storage pomocí Storage Exploreru (Preview)"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/17/2017
ms.author: kraigb
ms.openlocfilehash: 57737b51baace92858eb07c7dbc3139bd7e041f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-storage-explorer-preview"></a><span data-ttu-id="a425f-103">Začínáme se Storage Explorerem (Preview)</span><span class="sxs-lookup"><span data-stu-id="a425f-103">Get started with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="a425f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a425f-104">Overview</span></span>
<span data-ttu-id="a425f-105">Azure Storage Explorer (Preview) je samostatná aplikace, která vám umožní tooeasily práci s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="a425f-105">Azure Storage Explorer (Preview) is a standalone app that enables you tooeasily work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="a425f-106">V tomto článku se dozvíte hello různých způsobů připojení tooand Správa účtům Azure storage.</span><span class="sxs-lookup"><span data-stu-id="a425f-106">In this article, you learn hello various ways of connecting tooand managing your Azure storage accounts.</span></span>

![Microsoft Azure Storage Explorer (Preview)][15]

## <a name="prerequisites"></a><span data-ttu-id="a425f-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a425f-108">Prerequisites</span></span>
* [<span data-ttu-id="a425f-109">Stažení a instalace Storage Exploreru (Preview)</span><span class="sxs-lookup"><span data-stu-id="a425f-109">Download and install Storage Explorer (Preview)</span></span>](http://www.storageexplorer.com)

## <a name="connect-tooa-storage-account-or-service"></a><span data-ttu-id="a425f-110">Připojit tooa účtu úložiště nebo službě</span><span class="sxs-lookup"><span data-stu-id="a425f-110">Connect tooa storage account or service</span></span>
<span data-ttu-id="a425f-111">Storage Explorer (Preview) nabízí několik způsobů tooconnect toostorage účty.</span><span class="sxs-lookup"><span data-stu-id="a425f-111">Storage Explorer (Preview) provides several ways tooconnect toostorage accounts.</span></span> <span data-ttu-id="a425f-112">Můžete například provést následující věci:</span><span class="sxs-lookup"><span data-stu-id="a425f-112">For example, you can:</span></span>
* <span data-ttu-id="a425f-113">Připojte toostorage účty přidružené k vašemu předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-113">Connect toostorage accounts associated with your Azure subscriptions.</span></span>
* <span data-ttu-id="a425f-114">Připojte toostorage účty a služby, které jsou sdíleny z jiných předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-114">Connect toostorage accounts and services that are shared from other Azure subscriptions.</span></span>
* <span data-ttu-id="a425f-115">Připojit tooand spravovat místní úložiště pomocí hello emulátoru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-115">Connect tooand manage local storage by using hello Azure Storage Emulator.</span></span> 

<span data-ttu-id="a425f-116">Kromě toho můžete pracovat s účty úložiště v globálním i národním Azure:</span><span class="sxs-lookup"><span data-stu-id="a425f-116">In addition, you can work with storage accounts in global and national Azure:</span></span>

* <span data-ttu-id="a425f-117">[Připojit tooan předplatného Azure](#connect-to-an-azure-subscription): spravovat prostředky úložiště, které patří tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-117">[Connect tooan Azure subscription](#connect-to-an-azure-subscription): Manage storage resources that belong tooyour Azure subscription.</span></span>
* <span data-ttu-id="a425f-118">[Práce s místním vývojovém úložiště](#work-with-local-development-storage): spravovat místní úložiště pomocí hello emulátoru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-118">[Work with local development storage](#work-with-local-development-storage): Manage local storage by using hello Azure Storage Emulator.</span></span>
* <span data-ttu-id="a425f-119">[Připojte úložiště tooexternal](#attach-or-detach-an-external-storage-account): spravovat prostředky úložiště, které patří tooanother předplatného Azure nebo které jsou v rámci Azure národních cloudů pomocí názvu, klíč a koncové body účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-119">[Attach tooexternal storage](#attach-or-detach-an-external-storage-account): Manage storage resources that belong tooanother Azure subscription or that are under national Azure clouds by using hello storage account's name, key, and endpoints.</span></span>
* <span data-ttu-id="a425f-120">[Připojte účet úložiště pomocí SAS](#attach-storage-account-using-sas): spravovat prostředky úložiště, které patří tooanother předplatného Azure pomocí sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="a425f-120">[Attach a storage account by using an SAS](#attach-storage-account-using-sas): Manage storage resources that belong tooanother Azure subscription by using a shared access signature (SAS).</span></span>
* <span data-ttu-id="a425f-121">[Připojení služby pomocí SAS](#attach-service-using-sas): spravovat konkrétní službu úložiště (kontejner objektů blob, fronty nebo tabulky) patřící tooanother předplatného Azure pomocí SAS.</span><span class="sxs-lookup"><span data-stu-id="a425f-121">[Attach a service by using an SAS](#attach-service-using-sas): Manage a specific storage service (blob container, queue, or table) that belongs tooanother Azure subscription by using an SAS.</span></span>

## <a name="connect-tooan-azure-subscription"></a><span data-ttu-id="a425f-122">Připojit tooan předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="a425f-122">Connect tooan Azure subscription</span></span>
> [!NOTE]
> <span data-ttu-id="a425f-123">Pokud nemáte účet Azure, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="a425f-123">If you don't have an Azure account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
>
>

1. <span data-ttu-id="a425f-124">V nástroji Storage Explorer (Preview) vyberte **Azure Account Settings** (Nastavení účtu Azure).</span><span class="sxs-lookup"><span data-stu-id="a425f-124">In Storage Explorer (Preview), select **Azure Account settings**.</span></span>

    ![Nastavení účtu Azure][0]

2. <span data-ttu-id="a425f-126">Hello levém podokně zobrazí všechny účty Microsoft hello, které jste přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a425f-126">hello left pane displays all hello Microsoft accounts you've signed in to.</span></span> <span data-ttu-id="a425f-127">tooconnect tooanother účtu, vyberte možnost **přidat účet**a pak postupujte podle pokynů toosign hello pomocí účtu Microsoft, který je přidružen alespoň jeden aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-127">tooconnect tooanother account, select **Add an account**, and then follow hello instructions toosign in with a Microsoft account that is associated with at least one active Azure subscription.</span></span>

    >[!NOTE]
    ><span data-ttu-id="a425f-128">Připojení toonational Azure (například Azure v Německu, Azure Government a Azure China prostřednictvím přihlášení) není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="a425f-128">Connecting toonational Azure (such as Azure Germany, Azure Government, and Azure China via sign-in) is currently not supported.</span></span> <span data-ttu-id="a425f-129">Najdete v části hello "připojení nebo odpojení externího účtu úložiště" části jak účty Azure storage toonational tooconnect.</span><span class="sxs-lookup"><span data-stu-id="a425f-129">See hello "Attach or detach an external storage account" section for how tooconnect toonational Azure storage accounts.</span></span>

3. <span data-ttu-id="a425f-130">Poté, co se úspěšně přihlásíte účtem Microsoft, hello vlevo, že hello se zobrazí v podokně předplatná Azure, které jsou přidružené k tomuto účtu.</span><span class="sxs-lookup"><span data-stu-id="a425f-130">After you successfully sign in with a Microsoft account, hello left pane is populated with hello Azure subscriptions associated with that account.</span></span> <span data-ttu-id="a425f-131">Vyberte hello předplatná Azure, které chcete toowork s a potom vyberte **použít**.</span><span class="sxs-lookup"><span data-stu-id="a425f-131">Select hello Azure subscriptions that you want toowork with, and then select **Apply**.</span></span> <span data-ttu-id="a425f-132">(Výběr **všechny odběry** přepínačů výběr všech nebo žádných hello uvedených předplatných Azure.)</span><span class="sxs-lookup"><span data-stu-id="a425f-132">(Selecting **All subscriptions** toggles selecting all or none of hello listed Azure subscriptions.)</span></span>

    ![Výběr předplatných Azure][3]  
    <span data-ttu-id="a425f-134">Hello levém podokně zobrazí hello účty úložiště přidružené ke hello vybraná předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-134">hello left pane displays hello storage accounts associated with hello selected Azure subscriptions.</span></span>

    ![Vybraná předplatná Azure][4]

## <a name="connect-tooan-azure-stack-subscription"></a><span data-ttu-id="a425f-136">Připojení odběru tooan Azure zásobníku</span><span class="sxs-lookup"><span data-stu-id="a425f-136">Connect tooan Azure Stack subscription</span></span>

<span data-ttu-id="a425f-137">Informace o předplatném Azure zásobníku připojování tooan najdete v tématu [připojit Storage Explorer tooan předplatné Azure zásobníku](azure-stack/azure-stack-storage-connect-se.md).</span><span class="sxs-lookup"><span data-stu-id="a425f-137">For information about connecting tooan Azure Stack subscription, see [Connect Storage Explorer tooan Azure Stack subscription](azure-stack/azure-stack-storage-connect-se.md).</span></span>

## <a name="work-with-local-development-storage"></a><span data-ttu-id="a425f-138">Práce s místním vývojovým úložištěm</span><span class="sxs-lookup"><span data-stu-id="a425f-138">Work with local development storage</span></span>
<span data-ttu-id="a425f-139">Pomocí Storage Exploreru (Preview) můžete pracovat s místním úložištěm pomocí emulátoru úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-139">With Storage Explorer (Preview), you can work against local storage by using hello Azure Storage Emulator.</span></span> <span data-ttu-id="a425f-140">Tento přístup umožňuje zapisovat kód proti a testovací úložiště, aniž byste museli mít nasazený v Azure, účet úložiště, protože hello účet úložiště je emulovaných podle hello emulátoru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-140">This approach lets you write code against and test storage without necessarily having a storage account deployed on Azure, because hello storage account is being emulated by hello Azure Storage Emulator.</span></span>

> [!NOTE]
> <span data-ttu-id="a425f-141">Hello emulátoru úložiště Azure v současné době podporuje pouze pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="a425f-141">hello Azure Storage Emulator is currently supported only for Windows.</span></span>
>
>

1. <span data-ttu-id="a425f-142">V levém podokně hello Storage Exploreru (Preview) rozbalte hello **(místní a připojené)** > **účty úložiště** > **(vývoj)** uzlu.</span><span class="sxs-lookup"><span data-stu-id="a425f-142">In hello left pane of Storage Explorer (Preview), expand hello **(Local and Attached)** > **Storage Accounts** > **(Development)** node.</span></span>

    ![Místní vývojový uzel][21]

2. <span data-ttu-id="a425f-144">Pokud jste ještě nenainstalovali hello emulátoru úložiště Azure, jste výzvami toodo Ano prostřednictvím informačního panelu.</span><span class="sxs-lookup"><span data-stu-id="a425f-144">If you have not yet installed hello Azure Storage Emulator, you are prompted toodo so via an infobar.</span></span> <span data-ttu-id="a425f-145">Pokud se zobrazí informační panel hello, vyberte **stažení hello nejnovější verzi**a pak nainstalujte emulátor hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-145">If hello infobar is displayed, select **Download hello latest version**, and then install hello emulator.</span></span>

    ![Výzva ke stažení emulátoru úložiště Azure Storage][22]

3. <span data-ttu-id="a425f-147">Po nainstalování emulátoru hello můžete vytvořit a pracovat s místní objekty BLOB, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="a425f-147">After hello emulator is installed, you can create and work with local blobs, queues, and tables.</span></span> <span data-ttu-id="a425f-148">toolearn jak toowork s účtem úložiště pro každý typ, najdete v jednom z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="a425f-148">toolearn how toowork with each storage account type, see one of hello following:</span></span>

    * [<span data-ttu-id="a425f-149">Správa prostředků Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="a425f-149">Manage Azure blob storage resources</span></span>](vs-azure-tools-storage-explorer-blobs.md)
    * <span data-ttu-id="a425f-150">Správa prostředků Azure File Share Storage: *Připravuje se*</span><span class="sxs-lookup"><span data-stu-id="a425f-150">Manage Azure file share storage resources: *Coming soon*</span></span>
    * <span data-ttu-id="a425f-151">Správa prostředků Azure Queue Storage: *Připravuje se*</span><span class="sxs-lookup"><span data-stu-id="a425f-151">Manage Azure queue storage resources: *Coming soon*</span></span>
    * <span data-ttu-id="a425f-152">Správa prostředků Azure Table Storage: *Připravuje se*</span><span class="sxs-lookup"><span data-stu-id="a425f-152">Manage Azure table storage resources: *Coming soon*</span></span>

## <a name="attach-or-detach-an-external-storage-account"></a><span data-ttu-id="a425f-153">Připojení nebo odpojení externího účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="a425f-153">Attach or detach an external storage account</span></span>
<span data-ttu-id="a425f-154">Pomocí Storage Exploreru (Preview) můžete připojit tooexternal účty úložiště tak, aby účty úložiště můžete snadno sdílet.</span><span class="sxs-lookup"><span data-stu-id="a425f-154">With Storage Explorer (Preview), you can attach tooexternal storage accounts so that storage accounts can be easily shared.</span></span> <span data-ttu-id="a425f-155">Tato část vysvětluje, jak tooattach too(and detach from) externím účtům úložiště.</span><span class="sxs-lookup"><span data-stu-id="a425f-155">This section explains how tooattach too(and detach from) external storage accounts.</span></span>

### <a name="get-hello-storage-account-credentials"></a><span data-ttu-id="a425f-156">Získání přihlašovacích údajů účtu úložiště hello</span><span class="sxs-lookup"><span data-stu-id="a425f-156">Get hello storage account credentials</span></span>
<span data-ttu-id="a425f-157">tooshare externího účtu úložiště, hello vlastník tohoto účtu musíte nejprve získat přihlašovací údaje hello (název účtu a klíč) pro účet hello a potom sdílet informace s hello osobě, která chce tooattach toothat (externímu) účtu.</span><span class="sxs-lookup"><span data-stu-id="a425f-157">tooshare an external storage account, hello owner of that account must first get hello credentials (account name and key) for hello account and then share that information with hello person who wants tooattach toothat (external) account.</span></span> <span data-ttu-id="a425f-158">Přihlašovací údaje účtu úložiště hello prostřednictvím hello portálu Azure můžete získat pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="a425f-158">You can obtain hello storage account credentials via hello Azure portal by doing hello following:</span></span>

1. <span data-ttu-id="a425f-159">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a425f-159">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="a425f-160">Vyberte **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="a425f-160">Select **Browse**.</span></span>

3. <span data-ttu-id="a425f-161">Vyberte **Účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="a425f-161">Select **Storage Accounts**.</span></span>

4. <span data-ttu-id="a425f-162">Na hello **účty úložiště** okně, vyberte hello požadovaného účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a425f-162">On hello **Storage Accounts** blade, select hello desired storage account.</span></span>

5. <span data-ttu-id="a425f-163">Na hello **nastavení** okně hello vybraný účet úložiště, vyberte **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="a425f-163">On hello **Settings** blade for hello selected storage account, select **Access keys**.</span></span>

    ![Možnost Přístupové klíče][5]

6. <span data-ttu-id="a425f-165">Na hello **přístupové klíče** okno, kopie hello **název účtu úložiště** a **key1** hodnoty pro použití při připojení toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a425f-165">On hello **Access keys** blade, copy hello **Storage account name** and **key1** values for use when attaching toohello storage account.</span></span>

    ![Přístupové klíče][6]

### <a name="attach-tooan-external-storage-account"></a><span data-ttu-id="a425f-167">Připojte tooan externího účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="a425f-167">Attach tooan external storage account</span></span>
<span data-ttu-id="a425f-168">tooattach tooan externí účet úložiště, je třeba název a klíč účtu hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-168">tooattach tooan external storage account, you need hello account's name and key.</span></span> <span data-ttu-id="a425f-169">část "Get hello přihlašovacích údajů účtu úložiště" Hello vysvětluje, jak hello tooobtain tyto hodnoty z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-169">hello "Get hello storage account credentials" section explains how tooobtain these values from hello Azure portal.</span></span> <span data-ttu-id="a425f-170">Ale hello portálu, se nazývá klíč účtu hello **key1**.</span><span class="sxs-lookup"><span data-stu-id="a425f-170">However, in hello portal, hello account key is called **key1**.</span></span> <span data-ttu-id="a425f-171">Takže pokud Storage Explorer (Preview) požaduje klíč účtu, zadáte hello **key1** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a425f-171">So where Storage Explorer (Preview) asks for an account key, you enter hello **key1** value.</span></span>

1. <span data-ttu-id="a425f-172">Ve Storage Exploreru (Preview), vyberte **připojení úložiště tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="a425f-172">In Storage Explorer (Preview), select **Connect tooAzure storage**.</span></span>

    ![Možnost úložiště tooAzure připojení][23]

2. <span data-ttu-id="a425f-174">V hello **připojit tooAzure úložiště** dialogovém okně zadejte klíč účtu hello (hello **key1** hodnotu z hello portál Azure) a potom vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="a425f-174">In hello **Connect tooAzure Storage** dialog box, specify hello account key (hello **key1** value from hello Azure portal), and then select **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a425f-175">Můžete zadat připojovací řetězec úložiště hello z účtu úložiště na national Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-175">You can enter hello storage connection string from a storage account on national Azure.</span></span> <span data-ttu-id="a425f-176">Účty úložiště Německo tooconnect tooAzure, zadejte například připojovací řetězce podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="a425f-176">For example, tooconnect tooAzure Germany storage accounts, enter connection strings similar toohello following:</span></span> 
    >
    >* <span data-ttu-id="a425f-177">DefaultEndpointsProtocol=https</span><span class="sxs-lookup"><span data-stu-id="a425f-177">DefaultEndpointsProtocol=https</span></span>
    >* <span data-ttu-id="a425f-178">AccountName=cawatest03</span><span class="sxs-lookup"><span data-stu-id="a425f-178">AccountName=cawatest03</span></span>
    >* <span data-ttu-id="a425f-179">AccountKey=<klíč_účtu_úložiště></span><span class="sxs-lookup"><span data-stu-id="a425f-179">AccountKey=<storage_account_key></span></span>
    >* <span data-ttu-id="a425f-180">EndpointSuffix=core.cloudapi.de</span><span class="sxs-lookup"><span data-stu-id="a425f-180">EndpointSuffix=core.cloudapi.de</span></span>
    
    ><span data-ttu-id="a425f-181">Můžete získat připojovací řetězec hello hello Azure portálu v hello stejný způsobem popsaným v hello části "Získání přihlašovacích údajů účtu úložiště hello".</span><span class="sxs-lookup"><span data-stu-id="a425f-181">You can get hello connection string from hello Azure portal in hello same way as described in hello "Get hello storage account credentials" section.</span></span>

    ![TooAzure úložiště dialogové okno připojení][24]

3. <span data-ttu-id="a425f-183">V hello **připojit externí úložiště** dialogové okno, v hello **název účtu** pole, zadejte název účtu úložiště hello, zadejte další požadované nastavení a potom vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="a425f-183">In hello **Attach External Storage** dialog box, in hello **Account name** box, enter hello storage account name, specify any other desired settings, and then select **Next**.</span></span>

    ![Dialogové okno Připojit externí úložiště][8]

4. <span data-ttu-id="a425f-185">V hello **připojení Souhrn** dialogové okno pole, zkontrolujte informace o hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-185">In hello **Connection Summary** dialog box, verify hello information.</span></span> <span data-ttu-id="a425f-186">Pokud chcete toochange cokoli, vyberte **zpět** a znovu zadejte hello požadovaného nastavení.</span><span class="sxs-lookup"><span data-stu-id="a425f-186">If you want toochange anything, select **Back** and reenter hello desired settings.</span></span> 

5. <span data-ttu-id="a425f-187">Vyberte **Connect** (Připojit).</span><span class="sxs-lookup"><span data-stu-id="a425f-187">Select **Connect**.</span></span>

6. <span data-ttu-id="a425f-188">Po úspěšně připojen, hello externí účet úložiště zobrazí s **(External)** připojí toohello název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a425f-188">After it is successfully connected, hello external storage account is displayed with **(External)** appended toohello storage account name.</span></span>

    ![Výsledek připojení tooan externího účtu úložiště][9]

### <a name="detach-from-an-external-storage-account"></a><span data-ttu-id="a425f-190">Odpojení od externího účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="a425f-190">Detach from an external storage account</span></span>
1. <span data-ttu-id="a425f-191">Klikněte pravým tlačítkem na hello externího účtu úložiště má toodetach a pak vyberte **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="a425f-191">Right-click hello external storage account that you want toodetach, and then select **Detach**.</span></span>

    ![Možnost Odpojit od úložiště][10]

2. <span data-ttu-id="a425f-193">V potvrzovací zprávě hello vyberte **Ano** tooconfirm hello odpojení od hello externí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a425f-193">In hello confirmation message, select **Yes** tooconfirm hello detachment from hello external storage account.</span></span>

## <a name="attach-a-storage-account-by-using-an-sas"></a><span data-ttu-id="a425f-194">Připojení účtu úložiště pomocí sdíleného přístupového podpisu (SAS)</span><span class="sxs-lookup"><span data-stu-id="a425f-194">Attach a storage account by using an SAS</span></span>
<span data-ttu-id="a425f-195">[SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) umožňuje Dobrý den, správce předplatného Azure udělit dočasný přístup účtu úložiště tooa bez nutnosti tooprovide přihlašovací údaje předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="a425f-195">An [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) lets hello admin of an Azure subscription grant temporary access tooa storage account without having tooprovide Azure subscription credentials.</span></span>

<span data-ttu-id="a425f-196">tooillustrate tento scénář umožňuje vyslovte tohoto uživatele je správce předplatného Azure a uživatele chce tooaccess b tooallow účtu úložiště po omezenou dobu s určitými oprávněními:</span><span class="sxs-lookup"><span data-stu-id="a425f-196">tooillustrate this scenario, let's say that UserA is an admin of an Azure subscription, and UserA wants tooallow UserB tooaccess a storage account for a limited time with certain permissions:</span></span>

1. <span data-ttu-id="a425f-197">Uživatele a vygeneruje SAS (tvořený hello připojovací řetězec pro účet úložiště hello) pro určité časové období a s hello požadovaného oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a425f-197">UserA generates an SAS (consisting of hello connection string for hello storage account) for a specific time period and with hello desired permissions.</span></span>

2. <span data-ttu-id="a425f-198">Sdílené složky uživatele hello SAS s osobou hello (v našem příkladu b), který chce, aby se účet úložiště toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="a425f-198">UserA shares hello SAS with hello person (UserB, in our example) who wants access toohello storage account.</span></span>  

3. <span data-ttu-id="a425f-199">Uživatel b se pomocí Storage Exploreru (Preview) tooattach toohello účet, který patří tooUserA pomocí hello zadaný SAS.</span><span class="sxs-lookup"><span data-stu-id="a425f-199">UserB uses Storage Explorer (Preview) tooattach toohello account that belongs tooUserA by using hello supplied SAS.</span></span>

### <a name="get-an-sas-for-hello-account-you-want-tooshare"></a><span data-ttu-id="a425f-200">Získat SAS pro hello účet, že který má tooshare</span><span class="sxs-lookup"><span data-stu-id="a425f-200">Get an SAS for hello account you want tooshare</span></span>
1. <span data-ttu-id="a425f-201">Ve Storage Exploreru (Preview), klikněte pravým tlačítkem na účet úložiště hello chcete sdílet a potom vyberte **sdíleného přístupového podpisu**.</span><span class="sxs-lookup"><span data-stu-id="a425f-201">In Storage Explorer (Preview), right-click hello storage account you want share, and then select **Get Shared Access Signature**.</span></span>

    ![Možnost místní nabídky Získat sdílený přístupový podpis][13]

2. <span data-ttu-id="a425f-203">V hello **sdíleného přístupového podpisu** dialogovém okně zadejte hello časový rámec a oprávnění, které chcete použít pro účet hello a pak vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a425f-203">In hello **Shared Access Signature** dialog box, specify hello time frame and permissions that you want for hello account, and then select **Create**.</span></span>

    <span data-ttu-id="a425f-204">![Dialogové okno Získat SAS][14]</span><span class="sxs-lookup"><span data-stu-id="a425f-204">![Get SAS dialog box][14]</span></span>  
    <span data-ttu-id="a425f-205">Hello **sdíleného přístupového podpisu** dialogové okno a zobrazí hello SAS.</span><span class="sxs-lookup"><span data-stu-id="a425f-205">hello **Shared Access Signature** dialog box opens and displays hello SAS.</span></span>

3. <span data-ttu-id="a425f-206">Další toohello **připojovací řetězec**, vyberte **kopie** toocopy ho toohello schránky a potom vyberte **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="a425f-206">Next toohello **Connection String**, select **Copy** toocopy it toohello clipboard, and then select **Close**.</span></span>

### <a name="attach-toohello-shared-account-by-using-hello-sas"></a><span data-ttu-id="a425f-207">Připojte toohello sdíleného účtu pomocí hello SAS</span><span class="sxs-lookup"><span data-stu-id="a425f-207">Attach toohello shared account by using hello SAS</span></span>
1. <span data-ttu-id="a425f-208">Ve Storage Exploreru (Preview), vyberte **připojení úložiště tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="a425f-208">In Storage Explorer (Preview), select **Connect tooAzure storage**.</span></span>

    ![Možnost úložiště tooAzure připojení][23]

2. <span data-ttu-id="a425f-210">V hello **připojit tooAzure úložiště** dialogové okno, zadejte hello připojovací řetězec a pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="a425f-210">In hello **Connect tooAzure Storage** dialog box, specify hello connection string, and then select **Next**.</span></span>

    ![TooAzure úložiště dialogové okno připojení][24]

3. <span data-ttu-id="a425f-212">V hello **připojení Souhrn** dialogové okno pole, zkontrolujte informace o hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-212">In hello **Connection Summary** dialog box, verify hello information.</span></span> <span data-ttu-id="a425f-213">Vyberte změny toomake **zpět**a pak zadejte nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-213">toomake changes, select **Back**, and then enter hello settings you want.</span></span> 

4. <span data-ttu-id="a425f-214">Vyberte **Connect** (Připojit).</span><span class="sxs-lookup"><span data-stu-id="a425f-214">Select **Connect**.</span></span>

5. <span data-ttu-id="a425f-215">Po je připojen, hello účet úložiště zobrazí s **(SAS)** připojí toohello název účtu, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="a425f-215">After it is attached, hello storage account is displayed with **(SAS)** appended toohello account name that you supplied.</span></span>

    ![Výsledek připojené tooan účtu pomocí SAS][17]

## <a name="attach-a-service-by-using-an-sas"></a><span data-ttu-id="a425f-217">Připojení služby pomocí sdíleného přístupového podpisu (SAS)</span><span class="sxs-lookup"><span data-stu-id="a425f-217">Attach a service by using an SAS</span></span>
<span data-ttu-id="a425f-218">část "Připojit pomocí SAS účtu úložiště" Hello vysvětluje, jak správce předplatného Azure udělit dočasný přístup účtu úložiště tooa generování a sdílení SAS pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-218">hello "Attach a storage account by using an SAS" section explains how an Azure subscription admin can grant temporary access tooa storage account by generating and sharing an SAS for hello storage account.</span></span> <span data-ttu-id="a425f-219">Podobně je možné sdílený přístupový podpis (SAS) vygenerovat pro konkrétní službu (kontejner objektů blob, frontu nebo tabulku) v rámci účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a425f-219">Similarly, an SAS can be generated for a specific service (blob container, queue, or table) within a storage account.</span></span>  

### <a name="generate-an-sas-for-hello-service-that-you-want-tooshare"></a><span data-ttu-id="a425f-220">Generovat SAS, které chcete tooshare služby hello</span><span class="sxs-lookup"><span data-stu-id="a425f-220">Generate an SAS for hello service that you want tooshare</span></span>
<span data-ttu-id="a425f-221">V tomto kontextu může být službou kontejner objektů blob, fronta nebo tabulka.</span><span class="sxs-lookup"><span data-stu-id="a425f-221">In this context, a service can be a blob container, queue, or table.</span></span> <span data-ttu-id="a425f-222">toogenerate hello SAS služby uvedené v tématu:</span><span class="sxs-lookup"><span data-stu-id="a425f-222">toogenerate hello SAS for a listed service, see:</span></span>

* [<span data-ttu-id="a425f-223">Získat hello SAS pro kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="a425f-223">Get hello SAS for a blob container</span></span>](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* <span data-ttu-id="a425f-224">Získat hello SAS pro sdílené složky: *již brzy*</span><span class="sxs-lookup"><span data-stu-id="a425f-224">Get hello SAS for a file share: *Coming soon*</span></span>
* <span data-ttu-id="a425f-225">Získat hello SAS pro frontu: *již brzy*</span><span class="sxs-lookup"><span data-stu-id="a425f-225">Get hello SAS for a queue: *Coming soon*</span></span>
* <span data-ttu-id="a425f-226">Získat hello SAS pro tabulku: *již brzy*</span><span class="sxs-lookup"><span data-stu-id="a425f-226">Get hello SAS for a table: *Coming soon*</span></span>

### <a name="attach-toohello-shared-account-service-by-using-hello-sas"></a><span data-ttu-id="a425f-227">Připojení toohello sdíleného účtu služby pomocí hello SAS</span><span class="sxs-lookup"><span data-stu-id="a425f-227">Attach toohello shared account service by using hello SAS</span></span>
1. <span data-ttu-id="a425f-228">Ve Storage Exploreru (Preview), vyberte **připojení úložiště tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="a425f-228">In Storage Explorer (Preview), select **Connect tooAzure storage**.</span></span>

    ![Možnost úložiště tooAzure připojení][23]

2. <span data-ttu-id="a425f-230">V hello **připojit tooAzure úložiště** dialogové okno, zadejte hello identifikátor URI pro SAS a pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="a425f-230">In hello **Connect tooAzure Storage** dialog box, specify hello SAS URI, and then select **Next**.</span></span>

    ![TooAzure úložiště dialogové okno připojení][24]

3. <span data-ttu-id="a425f-232">V hello **připojení Souhrn** dialogové okno pole, zkontrolujte informace o hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-232">In hello **Connection Summary** dialog box, verify hello information.</span></span> <span data-ttu-id="a425f-233">Vyberte změny toomake **zpět**a pak zadejte nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-233">toomake changes, select **Back**, and then enter hello settings you want.</span></span> 

4. <span data-ttu-id="a425f-234">Vyberte **Connect** (Připojit).</span><span class="sxs-lookup"><span data-stu-id="a425f-234">Select **Connect**.</span></span>

5. <span data-ttu-id="a425f-235">Po je připojen, hello nově připojená služba zobrazí v části hello **(Service SAS)** uzlu.</span><span class="sxs-lookup"><span data-stu-id="a425f-235">After it is attached, hello newly attached service is displayed under hello **(Service SAS)** node.</span></span>

    ![Výsledek připojení tooa sdílené služby pomocí SAS][20]

## <a name="search-for-storage-accounts"></a><span data-ttu-id="a425f-237">Vyhledávání účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="a425f-237">Search for storage accounts</span></span>
<span data-ttu-id="a425f-238">Pokud máte dlouhý seznam účtů úložiště, je rychlý způsob toolocate konkrétní účet úložiště toouse hello vyhledávacího pole hello horní části levého podokna hello.</span><span class="sxs-lookup"><span data-stu-id="a425f-238">If you have a long list of storage accounts, a quick way toolocate a particular storage account is toouse hello search box at hello top of hello left pane.</span></span>

<span data-ttu-id="a425f-239">Při psaní do vyhledávacího pole hello hello levé podokno účty úložiště hello zobrazí, které odpovídají hello hledané hodnotě, kterou jste zadali až toothat bodu.</span><span class="sxs-lookup"><span data-stu-id="a425f-239">As you type in hello search box, hello left pane displays hello storage accounts that match hello search value you've entered up toothat point.</span></span> <span data-ttu-id="a425f-240">Například vyhledávání pro úložiště všechny účty, jejichž název obsahuje **tarcher** se zobrazí v hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="a425f-240">For example, a search for all storage accounts whose name contains **tarcher** is shown in hello following screenshot:</span></span>

![Vyhledávání účtu úložiště][11]

## <a name="next-steps"></a><span data-ttu-id="a425f-242">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a425f-242">Next steps</span></span>
* [<span data-ttu-id="a425f-243">Správa prostředků služby Azure Blob Storage pomocí Storage Exploreru (Preview)</span><span class="sxs-lookup"><span data-stu-id="a425f-243">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>](vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png
