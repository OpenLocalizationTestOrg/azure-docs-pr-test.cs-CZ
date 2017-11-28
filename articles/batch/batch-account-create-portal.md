---
title: "aaaCreate účtu Batch na portálu Azure hello | Microsoft Docs"
description: "Zjistěte, jak toocreate Azure Batch účet v hello Azure portálu toorun rozsáhlé paralelní úlohy v cloudu hello"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2176f88ba0a1a3298023de8f520d46ef28a664b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-portal"></a><span data-ttu-id="9eecd-103">Vytvoření účtu Batch se hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9eecd-103">Create a Batch account with hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9eecd-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9eecd-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="9eecd-105">Knihovna Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="9eecd-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
>
>

<span data-ttu-id="9eecd-106">Zjistěte, jak toocreate Azure Batch účet v hello [portál Azure][azure_portal]a vyberte vlastnosti účtu hello, splňující výpočetního scénáře.</span><span class="sxs-lookup"><span data-stu-id="9eecd-106">Learn how toocreate an Azure Batch account in hello [Azure portal][azure_portal], and choose hello account properties that fit your compute scenario.</span></span> <span data-ttu-id="9eecd-107">Zjistěte, kde jako vlastnosti důležité účtu toofind přístupových klíčů a adresy URL účtu.</span><span class="sxs-lookup"><span data-stu-id="9eecd-107">Learn where toofind important account properties like access keys and account URLs.</span></span>

<span data-ttu-id="9eecd-108">Pro informace o účty Batch a scénáře viz hello [funkci přehled](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="9eecd-108">For background about Batch accounts and scenarios, see hello [feature overview](batch-api-basics.md).</span></span>



## <a name="create-a-batch-account"></a><span data-ttu-id="9eecd-109">Vytvoření účtu Batch</span><span class="sxs-lookup"><span data-stu-id="9eecd-109">Create a Batch account</span></span>

<span data-ttu-id="9eecd-110">Pomocí portálu toocreate hello účtu Batch v jednom z hello dva *fond přidělení režimy*: **služba Batch** režimu nebo hello novější **uživatele předplatné** režimu, který vyžaduje více konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9eecd-110">Use hello portal toocreate a Batch account in one of hello two *pool allocation modes*: **Batch service** mode or hello newer **user subscription** mode, which requires more configuration.</span></span> <span data-ttu-id="9eecd-111">Informace o těchto dvou režimů najdete v tématu hello [funkci přehled](batch-api-basics.md#account).</span><span class="sxs-lookup"><span data-stu-id="9eecd-111">For information about these two modes, see hello [feature overview](batch-api-basics.md#account).</span></span> <span data-ttu-id="9eecd-112">Funkce hello uživatelského předplatné režimu naleznete také hello [příspěvku na blogu](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span><span class="sxs-lookup"><span data-stu-id="9eecd-112">For features of hello user subscription mode, see also hello [blog post](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span></span>

## <a name="batch-service-mode"></a><span data-ttu-id="9eecd-113">Režim služby Batch</span><span class="sxs-lookup"><span data-stu-id="9eecd-113">Batch service mode</span></span>



1. <span data-ttu-id="9eecd-114">Přihlaste se toohello [portál Azure][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="9eecd-114">Sign in toohello [Azure portal][azure_portal].</span></span>
2. <span data-ttu-id="9eecd-115">Klikněte na **Nový** > **Compute** > **Batch Service**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-115">Click **New** > **Compute** > **Batch Service**.</span></span>

    ![Batch v hello Marketplace.][marketplace_portal]
3. <span data-ttu-id="9eecd-117">Hello **nový účet Batch** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="9eecd-117">hello **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="9eecd-118">V tématu Popis hello níže jednotlivých prvků okna.</span><span class="sxs-lookup"><span data-stu-id="9eecd-118">See hello descriptions below of each blade element.</span></span>

    ![Vytvoření účtu Batch][account_portal]

    <span data-ttu-id="9eecd-120">a.</span><span class="sxs-lookup"><span data-stu-id="9eecd-120">a.</span></span> <span data-ttu-id="9eecd-121">**Název účtu**: název účtu Batch hello zvolíte, musí být jedinečný v rámci hello oblast Azure, kde se má vytvořit účet hello (viz **umístění** níže).</span><span class="sxs-lookup"><span data-stu-id="9eecd-121">**Account name**: hello Batch account name you choose must be unique within hello Azure region where hello account is created (see **Location** below).</span></span> <span data-ttu-id="9eecd-122">název účtu Hello může obsahovat jenom malá písmena nebo číslice a musí být 3 až 24 znaků.</span><span class="sxs-lookup"><span data-stu-id="9eecd-122">hello account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="9eecd-123">b.</span><span class="sxs-lookup"><span data-stu-id="9eecd-123">b.</span></span> <span data-ttu-id="9eecd-124">**Předplatné**: hello předplatné, ve které toocreate hello účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-124">**Subscription**: hello subscription in which toocreate hello Batch account.</span></span> <span data-ttu-id="9eecd-125">Pokud máte jenom jedno předplatné, bude ve výchozím nastavení vybrané.</span><span class="sxs-lookup"><span data-stu-id="9eecd-125">If you have only one subscription, it is selected by default.</span></span>

    <span data-ttu-id="9eecd-126">c.</span><span class="sxs-lookup"><span data-stu-id="9eecd-126">c.</span></span> <span data-ttu-id="9eecd-127">**Režim přidělování fondů:** Vyberte možnost **Služba Batch**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-127">**Pool allocation mode**: Select **Batch service**.</span></span>

    <span data-ttu-id="9eecd-128">c.</span><span class="sxs-lookup"><span data-stu-id="9eecd-128">c.</span></span> <span data-ttu-id="9eecd-129">**Skupina prostředků**: Vyberte existující skupinu prostředků vašeho nového účtu Batch, popřípadě si vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="9eecd-129">**Resource group**: Select an existing resource group for your new Batch account, or optionally create a new one.</span></span>

    <span data-ttu-id="9eecd-130">d.</span><span class="sxs-lookup"><span data-stu-id="9eecd-130">d.</span></span> <span data-ttu-id="9eecd-131">**Umístění**: hello oblast Azure, ve které toocreate hello účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-131">**Location**: hello Azure region in which toocreate hello Batch account.</span></span> <span data-ttu-id="9eecd-132">Jako možnosti se zobrazí pouze hello oblasti podporuje vaše předplatné a skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="9eecd-132">Only hello regions supported by your subscription and resource group are displayed as options.</span></span>

    <span data-ttu-id="9eecd-133">e.</span><span class="sxs-lookup"><span data-stu-id="9eecd-133">e.</span></span> <span data-ttu-id="9eecd-134">**Účet úložiště** (volitelné): Účet úložiště Azure pro obecné účely, který přidružíte k účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-134">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="9eecd-135">Toto nastavení se doporučuje pro většinu účtů Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-135">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="9eecd-136">Další informace najdete v části [Propojený účet Azure Storage](#linked-azure-storage-account) níže v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="9eecd-136">See [Linked Azure Storage account](#linked-azure-storage-account) later in this article for more details.</span></span>

4. <span data-ttu-id="9eecd-137">Klikněte na tlačítko **vytvořit** toocreate hello účtu.</span><span class="sxs-lookup"><span data-stu-id="9eecd-137">Click **Create** toocreate hello account.</span></span>

   <span data-ttu-id="9eecd-138">portál Hello označuje, že nasazení právě probíhá.</span><span class="sxs-lookup"><span data-stu-id="9eecd-138">hello portal indicates deployment is in progress.</span></span> <span data-ttu-id="9eecd-139">Po dokončení se v části **Oznámení** zobrazí zpráva **Nasazení se podařila**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-139">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>

## <a name="user-subscription-mode"></a><span data-ttu-id="9eecd-140">Režim předplatného uživatele</span><span class="sxs-lookup"><span data-stu-id="9eecd-140">User subscription mode</span></span>

### <a name="allow-azure-batch-tooaccess-hello-subscription-one-time-operation"></a><span data-ttu-id="9eecd-141">Povolit Azure Batch tooaccess hello předplatné (jednorázová operace)</span><span class="sxs-lookup"><span data-stu-id="9eecd-141">Allow Azure Batch tooaccess hello subscription (one-time operation)</span></span>
<span data-ttu-id="9eecd-142">Při vytváření účtu Batch první v uživatelském režimu předplatné, proveďte následující kroky tooregister hello předplatného pomocí služby Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-142">When creating your first Batch account in user subscription mode, perform hello following steps tooregister your subscription with Batch.</span></span> <span data-ttu-id="9eecd-143">(Pokud jste dříve to, přeskočte toohello další části.)</span><span class="sxs-lookup"><span data-stu-id="9eecd-143">(If you previously did this, skip toohello next section.)</span></span>

1. <span data-ttu-id="9eecd-144">Přihlaste se toohello [portál Azure][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="9eecd-144">Sign in toohello [Azure portal][azure_portal].</span></span>

2. <span data-ttu-id="9eecd-145">Klikněte na tlačítko **více služeb** > **odběry**a klikněte na předplatné hello chcete toouse pro hello účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-145">Click **More Services** > **Subscriptions**, and click hello subscription you want toouse for hello Batch account.</span></span>

3. <span data-ttu-id="9eecd-146">V hello **předplatné** okně klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-146">In hello **Subscription** blade, click **Access control (IAM)** > **Add**.</span></span>

    ![Řízení přístupu pro předplatné][subscription_access]

4. <span data-ttu-id="9eecd-148">Na hello **přidat oprávnění** okně, vyberte hello **Přispěvatel** role, vyhledejte hello Batch API.</span><span class="sxs-lookup"><span data-stu-id="9eecd-148">On hello **Add permissions** blade, select hello **Contributor** role, search for hello Batch API.</span></span> <span data-ttu-id="9eecd-149">Vyhledávání pro každý z těchto řetězců vyhledejte hello rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="9eecd-149">Search for each of these strings until you find hello API:</span></span>
    1. <span data-ttu-id="9eecd-150">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-150">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="9eecd-151">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-151">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="9eecd-152">Novější tenanti služby Azure AD mohou používat tento název.</span><span class="sxs-lookup"><span data-stu-id="9eecd-152">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="9eecd-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** se hello ID hello Batch API.</span><span class="sxs-lookup"><span data-stu-id="9eecd-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** is hello ID for hello Batch API.</span></span> 

5. <span data-ttu-id="9eecd-154">Po nalezení hello Batch API, vyberte ho a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-154">Once you find hello Batch API, select it and click **Save**.</span></span>

    ![Přidání oprávnění pro službu Batch][add_permission]

### <a name="create-a-key-vault"></a><span data-ttu-id="9eecd-156">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="9eecd-156">Create a key vault</span></span>
<span data-ttu-id="9eecd-157">V uživatelském režimu předplatné, se vyžaduje klíče trezoru služby Azure, patří toothe stejné skupině prostředků jako toobe účtu Batch hello vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9eecd-157">In user subscription mode, an Azure key vault is required that belongs toothe same resource group as hello Batch account toobe created.</span></span> <span data-ttu-id="9eecd-158">Zkontrolujte, zda je skupina prostředků hello v oblasti, kde je Batch [k dispozici](https://azure.microsoft.com/regions/services/) a které podporuje vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="9eecd-158">Make sure hello resource group is in a region where Batch is [available](https://azure.microsoft.com/regions/services/) and which your subscription supports.</span></span>

1. <span data-ttu-id="9eecd-159">V hello [portál Azure][azure_portal], klikněte na tlačítko **nový** > **zabezpečení a identita** > **Key Vault** .</span><span class="sxs-lookup"><span data-stu-id="9eecd-159">In hello [Azure portal][azure_portal], click **New** > **Security + Identity** > **Key Vault**.</span></span>

2. <span data-ttu-id="9eecd-160">V hello **vytvořit Key Vault** okno, zadejte název trezoru klíčů hello a vytvořte skupinu prostředků v hello oblasti, které chcete použít pro účet Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-160">In hello **Create Key Vault** blade, enter a name for hello key vault, and create a resource group in hello region you want for your Batch account.</span></span> <span data-ttu-id="9eecd-161">Nechte hello zbývající nastavení na výchozí hodnoty a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-161">Leave hello remaining settings at default values, then click **Create**.</span></span>

### <a name="create-a-batch-account"></a><span data-ttu-id="9eecd-162">Vytvoření účtu Batch</span><span class="sxs-lookup"><span data-stu-id="9eecd-162">Create a Batch account</span></span>

1. <span data-ttu-id="9eecd-163">V hello [portál Azure][azure_portal], klikněte na tlačítko **nový** > **výpočetní** > **služba Batch**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-163">In hello [Azure portal][azure_portal], click **New** > **Compute** > **Batch Service**.</span></span>

    ![Batch v hello Marketplace.][marketplace_portal]
3. <span data-ttu-id="9eecd-165">Hello **nový účet Batch** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="9eecd-165">hello **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="9eecd-166">V tématu Popis hello níže jednotlivých prvků okna.</span><span class="sxs-lookup"><span data-stu-id="9eecd-166">See hello descriptions below of each blade element.</span></span>

    ![Vytvoření účtu Batch][account_portal_byos]

    <span data-ttu-id="9eecd-168">a.</span><span class="sxs-lookup"><span data-stu-id="9eecd-168">a.</span></span> <span data-ttu-id="9eecd-169">**Název účtu**: název účtu Batch hello zvolíte, musí být jedinečný v rámci hello oblast Azure, kde se má vytvořit účet hello (viz **umístění** níže).</span><span class="sxs-lookup"><span data-stu-id="9eecd-169">**Account name**: hello Batch account name you choose must be unique within hello Azure region where hello account is created (see **Location** below).</span></span> <span data-ttu-id="9eecd-170">název účtu Hello může obsahovat jenom malá písmena nebo číslice a musí být 3 až 24 znaků.</span><span class="sxs-lookup"><span data-stu-id="9eecd-170">hello account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="9eecd-171">b.</span><span class="sxs-lookup"><span data-stu-id="9eecd-171">b.</span></span> <span data-ttu-id="9eecd-172">**Předplatné**: Pokud máte více než jedno předplatné, vyberte předplatné hello, který jste zaregistrovali hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-172">**Subscription**: If you have more than one subscription, select hello subscription that you registered with hello Batch service.</span></span>

    <span data-ttu-id="9eecd-173">c.</span><span class="sxs-lookup"><span data-stu-id="9eecd-173">c.</span></span> <span data-ttu-id="9eecd-174">**Režim přidělování fondů:** Vyberte **Předplatné uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-174">**Pool allocation mode**: Select **User subscription**.</span></span>

    <span data-ttu-id="9eecd-175">d.</span><span class="sxs-lookup"><span data-stu-id="9eecd-175">d.</span></span> <span data-ttu-id="9eecd-176">**Trezor klíčů**: Vyberte hello trezoru klíčů jste vytvořili pro účet Batch v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="9eecd-176">**Key vault**: Select hello key vault you created for your Batch account in hello previous section.</span></span> <span data-ttu-id="9eecd-177">Volitelně můžete vytvořit nový trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="9eecd-177">Optionally, create a new key vault.</span></span> <span data-ttu-id="9eecd-178">Po výběru hello trezoru, vyberte hello políčko toogrant Azure Batch přístup toohello klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="9eecd-178">After selecting hello vault, select hello checkbox toogrant Azure Batch access toohello key vault.</span></span>

    <span data-ttu-id="9eecd-179">c.</span><span class="sxs-lookup"><span data-stu-id="9eecd-179">c.</span></span> <span data-ttu-id="9eecd-180">**Skupina prostředků**: Skupina prostředků vyberte hello, ve které jste vytvořili trezor klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="9eecd-180">**Resource group**: Select hello resource group in which you  created hello key vault.</span></span>

    <span data-ttu-id="9eecd-181">d.</span><span class="sxs-lookup"><span data-stu-id="9eecd-181">d.</span></span> <span data-ttu-id="9eecd-182">**Umístění**: hello oblast Azure, ve které jste vytvořili trezor klíčů hello pro účet Batch hello.</span><span class="sxs-lookup"><span data-stu-id="9eecd-182">**Location**: hello Azure region in which you created hello key vault for hello Batch account.</span></span>

    <span data-ttu-id="9eecd-183">e.</span><span class="sxs-lookup"><span data-stu-id="9eecd-183">e.</span></span> <span data-ttu-id="9eecd-184">**Účet úložiště** (volitelné): Účet úložiště Azure pro obecné účely, který přidružíte k účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-184">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="9eecd-185">Toto nastavení se doporučuje pro většinu účtů Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-185">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="9eecd-186">Další informace najdete v části [Propojený účet Azure Storage](#linked-azure-storage-account) dole.</span><span class="sxs-lookup"><span data-stu-id="9eecd-186">See [Linked Azure Storage account](#linked-azure-storage-account) below for more details.</span></span>

4. <span data-ttu-id="9eecd-187">Klikněte na tlačítko **vytvořit** toocreate hello účtu.</span><span class="sxs-lookup"><span data-stu-id="9eecd-187">Click **Create** toocreate hello account.</span></span>

   <span data-ttu-id="9eecd-188">portál Hello označuje, že nasazení právě probíhá.</span><span class="sxs-lookup"><span data-stu-id="9eecd-188">hello portal indicates deployment is in progress.</span></span> <span data-ttu-id="9eecd-189">Po dokončení se v části **Oznámení** zobrazí zpráva **Nasazení se podařila**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-189">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>



## <a name="view-batch-account-properties"></a><span data-ttu-id="9eecd-190">Zobrazení vlastností účtu Batch</span><span class="sxs-lookup"><span data-stu-id="9eecd-190">View Batch account properties</span></span>
<span data-ttu-id="9eecd-191">Po vytvoření účtu hello můžete otevřít hello **okno účtu Batch** tooaccess jeho nastavení a vlastností.</span><span class="sxs-lookup"><span data-stu-id="9eecd-191">Once hello account has been created, you can open hello **Batch account blade** tooaccess its settings and properties.</span></span> <span data-ttu-id="9eecd-192">Všechna nastavení účtu a vlastnosti mají přístup pomocí hello levé nabídce okna účtu Batch hello.</span><span class="sxs-lookup"><span data-stu-id="9eecd-192">You can access all account settings and properties by using hello left menu of hello Batch account blade.</span></span>

![Okno účtu Batch na webu Azure Portal][account_blade]

* <span data-ttu-id="9eecd-194">**Adresa URL účtu batch**: když budete vyvíjet aplikace s hello [rozhraní API služby Batch](batch-apis-tools.md#azure-accounts-for-batch-development), budete potřebovat tooaccess adresa URL účtu prostředky služby Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-194">**Batch account URL**: When you develop an application with hello [Batch APIs](batch-apis-tools.md#azure-accounts-for-batch-development), you'll need an account URL tooaccess your Batch resources.</span></span> <span data-ttu-id="9eecd-195">Adresa URL účtu Batch má hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="9eecd-195">A Batch account URL has hello following format:</span></span>

    `https://<account_name>.<region>.batch.azure.com`

![Adresa URL účtu Batch na portálu][account_url]

* <span data-ttu-id="9eecd-197">**Přístupové klíče** (režim služby Batch): tooyour tooauthenticate přístup k účtu Batch z vaší aplikace, budete potřebovat přístupový klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="9eecd-197">**Access keys** (Batch service mode): tooauthenticate access tooyour Batch account from your application, you'll need an account access key.</span></span> <span data-ttu-id="9eecd-198">(Toto nastavení není k dispozici v režimu předplatného uživatele, pokud používáte ověřování pomocí Azure Active Directory.)</span><span class="sxs-lookup"><span data-stu-id="9eecd-198">(This setting is not available in user subscription mode, where you use Azure Active Directory authentication.)</span></span>

    <span data-ttu-id="9eecd-199">tooview nebo znovu vygenerovat přístupové klíče účtu Batch, zadejte `keys` v levé nabídce hello **vyhledávání** pole v okně účtu Batch hello a pak vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-199">tooview or regenerate your Batch account's access keys, enter `keys` in hello left menu **Search** box on hello Batch account blade, then select **Keys**.</span></span>

    ![Klíče účtu Batch na webu Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a><span data-ttu-id="9eecd-201">Propojený účet Azure Storage</span><span class="sxs-lookup"><span data-stu-id="9eecd-201">Linked Azure Storage account</span></span>

<span data-ttu-id="9eecd-202">Volitelně můžete propojit pro obecné účely tooyour účtu Azure Storage účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-202">You can optionally link a general-purpose Azure Storage account tooyour Batch account.</span></span> <span data-ttu-id="9eecd-203">Hello [balíčky aplikací](batch-application-packages.md) služby Batch používá úložiště objektů Blob v Azure, stejně jako hello [Batch .NET konvence souboru](batch-task-output.md) knihovny.</span><span class="sxs-lookup"><span data-stu-id="9eecd-203">hello [application packages](batch-application-packages.md) feature of Batch uses Azure Blob storage, as does hello [Batch File Conventions .NET](batch-task-output.md) library.</span></span> <span data-ttu-id="9eecd-204">Tyto volitelné funkce vám pomohou při nasazení aplikace hello spuštěné úkoly služby Batch a zachování hello dat, které vytvářejí.</span><span class="sxs-lookup"><span data-stu-id="9eecd-204">These optional features assist you in deploying hello applications that your Batch tasks run, and persisting hello data they produce.</span></span>

<span data-ttu-id="9eecd-205">Doporučujeme vytvořit nový účet úložiště pro výhradní použití vaším účtem Batch.</span><span class="sxs-lookup"><span data-stu-id="9eecd-205">We recommend that you create a new Storage account exclusively for use by your Batch account.</span></span>

![Vytvoření účtu úložiště pro obecné účely][storage_account]

> [!NOTE]
> <span data-ttu-id="9eecd-207">Azure Batch aktuálně podporuje pouze hello obecný typ účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eecd-207">Azure Batch currently supports only hello general-purpose Storage account type.</span></span> <span data-ttu-id="9eecd-208">Tento typ účtu je popsaný v kroku 5, [Vytvoření účtu úložiště] (../storage/common/storage-create-storage-account.md#create-a-storage-account), v tématu [O účtech úložiště Azure](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="9eecd-208">This account type is described in step 5, [Create a storage account] (../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
>
>

> [!WARNING]
> <span data-ttu-id="9eecd-209">Dávejte pozor při opakovaném generování přístupových klíčů hello propojeného účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eecd-209">Be careful when regenerating hello access keys of a linked Storage account.</span></span> <span data-ttu-id="9eecd-210">Obnovit pouze jeden klíč účtu úložiště a klikněte na tlačítko **synchronizace klíčů** na hello propojené okně účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eecd-210">Regenerate only one Storage account key and click **Sync Keys** on hello linked Storage account blade.</span></span> <span data-ttu-id="9eecd-211">Hello počkejte pět minut tooallow hello klíče toopropagate toohello výpočetních uzlů ve fondech, pak znovu vygenerujte a synchronizujte Další klíč v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="9eecd-211">Wait five minutes tooallow hello keys toopropagate toohello compute nodes in your pools, then regenerate and synchronize hello other key if necessary.</span></span> <span data-ttu-id="9eecd-212">Pokud byste znovu generovali oba klíče v hello stejný čas, výpočetní uzly nebudou moct toosynchronize ani jeden klíč a dojde ke ztrátě přístupu účtu úložiště toohello.</span><span class="sxs-lookup"><span data-stu-id="9eecd-212">If you regenerate both keys at hello same time, your compute nodes will not be able toosynchronize either key, and they will lose access toohello Storage account.</span></span>
>
>

<span data-ttu-id="9eecd-213">![Obnovování klíčů účtu úložiště][4]</span><span class="sxs-lookup"><span data-stu-id="9eecd-213">![Regenerating storage account keys][4]</span></span>

## <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="9eecd-214">Kvóty a omezení služby Batch</span><span class="sxs-lookup"><span data-stu-id="9eecd-214">Batch service quotas and limits</span></span>
<span data-ttu-id="9eecd-215">Potřeba si uvědomit, že jako s předplatným Azure a jiné Azure services, určité [kvóty a omezení](batch-quota-limit.md) použít tooBatch účty.</span><span class="sxs-lookup"><span data-stu-id="9eecd-215">Please be aware that as with your Azure subscription and other Azure services, certain [quotas and limits](batch-quota-limit.md) apply tooBatch accounts.</span></span> <span data-ttu-id="9eecd-216">Aktuální kvóty pro účet Batch se zobrazují hello portálu v účtu hello **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="9eecd-216">Current quotas for a Batch account appear in hello portal in hello account **Properties**.</span></span>

![Kvóty účtu Batch na webu Azure Portal][quotas]



<span data-ttu-id="9eecd-218">Kromě toho řada těchto kvót může být zvýšena jednoduše se žádost o podporu volné produktu ve hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9eecd-218">Additionally, many of these quotas can be increased simply with a free product support request submitted in hello Azure portal.</span></span> <span data-ttu-id="9eecd-219">V tématu [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md) podrobnosti o žádají o zvýšení kvóty.</span><span class="sxs-lookup"><span data-stu-id="9eecd-219">See [Quotas and limits for hello Azure Batch service](batch-quota-limit.md) for details on requesting quota increases.</span></span>

## <a name="other-batch-account-management-options"></a><span data-ttu-id="9eecd-220">Další možnosti správy účtu Batch</span><span class="sxs-lookup"><span data-stu-id="9eecd-220">Other Batch account management options</span></span>
<span data-ttu-id="9eecd-221">Kromě toho toousing hello portálu Azure, můžete také vytvořit a spravovat účty Batch s hello následující:</span><span class="sxs-lookup"><span data-stu-id="9eecd-221">In addition toousing hello Azure portal, you can also create and manage Batch accounts with hello following:</span></span>

* [<span data-ttu-id="9eecd-222">Rutiny PowerShellu pro účty Batch</span><span class="sxs-lookup"><span data-stu-id="9eecd-222">Batch PowerShell cmdlets</span></span>](batch-powershell-cmdlets-get-started.md)
* [<span data-ttu-id="9eecd-223">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9eecd-223">Azure CLI</span></span>](batch-cli-get-started.md)
* [<span data-ttu-id="9eecd-224">Knihovna Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="9eecd-224">Batch Management .NET</span></span>](batch-management-dotnet.md)

## <a name="next-steps"></a><span data-ttu-id="9eecd-225">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9eecd-225">Next steps</span></span>
* <span data-ttu-id="9eecd-226">V tématu hello [přehled funkcí Batch](batch-api-basics.md) toolearn Další informace o konceptech služby Batch a funkce.</span><span class="sxs-lookup"><span data-stu-id="9eecd-226">See hello [Batch feature overview](batch-api-basics.md) toolearn more about Batch service concepts and features.</span></span> <span data-ttu-id="9eecd-227">Hello článek popisuje hello primární prostředky služby Batch například fondy, výpočetní uzly, úlohy a úkoly a nabízí přehled hello funkcí služby, které umožňují spouštění velkých výpočetních úloh.</span><span class="sxs-lookup"><span data-stu-id="9eecd-227">hello article discusses hello primary Batch resources such as pools, compute nodes, jobs, and tasks, and provides an overview of hello service's features that enable large-scale compute workload execution.</span></span>
* <span data-ttu-id="9eecd-228">Další informace hello základy vývoje aplikací s povolenými Batch pomocí hello [klientské knihovny Batch .NET](batch-dotnet-get-started.md) nebo [Python](batch-python-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="9eecd-228">Learn hello basics of developing a Batch-enabled application using hello [Batch .NET client library](batch-dotnet-get-started.md) or [Python](batch-python-tutorial.md).</span></span> <span data-ttu-id="9eecd-229">Tyto články úvodní vás provede funkční aplikaci, která používá tooexecute služby Batch hello zatížení v několika výpočetních uzlech a zahrnuje použití služby Azure Storage pro zatížení přípravě a načítání souborů.</span><span class="sxs-lookup"><span data-stu-id="9eecd-229">These introductory articles guide you through a working application that uses hello Batch service tooexecute a workload on multiple compute nodes, and includes using Azure Storage for workload file staging and retrieval.</span></span>

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Obnovování klíčů účtu úložiště"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[account_portal_byos]: ./media/batch-account-create-portal/batch_acct_portal_byos.png
