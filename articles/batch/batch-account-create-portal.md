---
title: "Vytvoření účtu Batch na webu Azure Portal | Dokumentace Microsoftu"
description: "Naučte se vytvořit účet Azure Batch na webu Azure Portal, abyste mohli spouštět velké paralelní úlohy v cloudu."
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
ms.openlocfilehash: 520d1d42d35b25db1a35d4317e9eb616cf5de565
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-batch-account-with-the-azure-portal"></a><span data-ttu-id="b2aad-103">Vytvoření účtu Batch pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b2aad-103">Create a Batch account with the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b2aad-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b2aad-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="b2aad-105">Knihovna Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="b2aad-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
>
>

<span data-ttu-id="b2aad-106">Naučte se vytvořit účet Azure Batch na webu [Azure Portal][azure_portal] a zvolit vlastnosti účtu odpovídající výpočetnímu scénáři.</span><span class="sxs-lookup"><span data-stu-id="b2aad-106">Learn how to create an Azure Batch account in the [Azure portal][azure_portal], and choose the account properties that fit your compute scenario.</span></span> <span data-ttu-id="b2aad-107">Zjistěte, kde najít důležité vlastnosti účtu, jako jsou přístupové klíče a adresy URL účtu.</span><span class="sxs-lookup"><span data-stu-id="b2aad-107">Learn where to find important account properties like access keys and account URLs.</span></span>

<span data-ttu-id="b2aad-108">Informace o scénářích a účtech Batch najdete v [přehledu funkcí](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="b2aad-108">For background about Batch accounts and scenarios, see the [feature overview](batch-api-basics.md).</span></span>



## <a name="create-a-batch-account"></a><span data-ttu-id="b2aad-109">Vytvoření účtu Batch</span><span class="sxs-lookup"><span data-stu-id="b2aad-109">Create a Batch account</span></span>

<span data-ttu-id="b2aad-110">Prostřednictvím portálu můžete vytvořit účet Batch v jednom ze dvou *režimu přidělování fondů*: v režimu **služby Batch** nebo v novějším režimu **předplatného uživatele**, který vyžaduje další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b2aad-110">Use the portal to create a Batch account in one of the two *pool allocation modes*: **Batch service** mode or the newer **user subscription** mode, which requires more configuration.</span></span> <span data-ttu-id="b2aad-111">Informace o těchto dvou režimech najdete v [přehledu funkcí](batch-api-basics.md#account).</span><span class="sxs-lookup"><span data-stu-id="b2aad-111">For information about these two modes, see the [feature overview](batch-api-basics.md#account).</span></span> <span data-ttu-id="b2aad-112">Funkce režimu předplatného uživatele najdete také v [příspěvku na blogu](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span><span class="sxs-lookup"><span data-stu-id="b2aad-112">For features of the user subscription mode, see also the [blog post](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span></span>

## <a name="batch-service-mode"></a><span data-ttu-id="b2aad-113">Režim služby Batch</span><span class="sxs-lookup"><span data-stu-id="b2aad-113">Batch service mode</span></span>



1. <span data-ttu-id="b2aad-114">Přihlaste se na web [Azure Portal][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="b2aad-114">Sign in to the [Azure portal][azure_portal].</span></span>
2. <span data-ttu-id="b2aad-115">Klikněte na **Nový** > **Compute** > **Batch Service**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-115">Click **New** > **Compute** > **Batch Service**.</span></span>

    ![Batch na webu Marketplace][marketplace_portal]
3. <span data-ttu-id="b2aad-117">Zobrazí se okno **Nový účet Batch**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-117">The **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="b2aad-118">Níže jsou uvedeny popisy jednotlivých prvků okna.</span><span class="sxs-lookup"><span data-stu-id="b2aad-118">See the descriptions below of each blade element.</span></span>

    ![Vytvoření účtu Batch][account_portal]

    <span data-ttu-id="b2aad-120">a.</span><span class="sxs-lookup"><span data-stu-id="b2aad-120">a.</span></span> <span data-ttu-id="b2aad-121">**Název účtu:** Název účtu Batch, který zvolíte, musí být jedinečný v oblasti Azure, ve které se nový účet vytvoří (viz **Umístění** níže).</span><span class="sxs-lookup"><span data-stu-id="b2aad-121">**Account name**: The Batch account name you choose must be unique within the Azure region where the account is created (see **Location** below).</span></span> <span data-ttu-id="b2aad-122">Název účtu může obsahovat jenom malá písmena nebo čísla a musí být dlouhý 3 až 24 znaků.</span><span class="sxs-lookup"><span data-stu-id="b2aad-122">The account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="b2aad-123">b.</span><span class="sxs-lookup"><span data-stu-id="b2aad-123">b.</span></span> <span data-ttu-id="b2aad-124">**Předplatné**: Předplatné, ve kterém chcete účet Batch vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b2aad-124">**Subscription**: The subscription in which to create the Batch account.</span></span> <span data-ttu-id="b2aad-125">Pokud máte jenom jedno předplatné, bude ve výchozím nastavení vybrané.</span><span class="sxs-lookup"><span data-stu-id="b2aad-125">If you have only one subscription, it is selected by default.</span></span>

    <span data-ttu-id="b2aad-126">c.</span><span class="sxs-lookup"><span data-stu-id="b2aad-126">c.</span></span> <span data-ttu-id="b2aad-127">**Režim přidělování fondů:** Vyberte možnost **Služba Batch**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-127">**Pool allocation mode**: Select **Batch service**.</span></span>

    <span data-ttu-id="b2aad-128">c.</span><span class="sxs-lookup"><span data-stu-id="b2aad-128">c.</span></span> <span data-ttu-id="b2aad-129">**Skupina prostředků**: Vyberte existující skupinu prostředků vašeho nového účtu Batch, popřípadě si vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="b2aad-129">**Resource group**: Select an existing resource group for your new Batch account, or optionally create a new one.</span></span>

    <span data-ttu-id="b2aad-130">d.</span><span class="sxs-lookup"><span data-stu-id="b2aad-130">d.</span></span> <span data-ttu-id="b2aad-131">**Umístění**: Oblast Azure, ve které chcete účet Batch vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b2aad-131">**Location**: The Azure region in which to create the Batch account.</span></span> <span data-ttu-id="b2aad-132">Jako možnosti se zobrazí jenom oblasti, které podporuje vaše předplatné a skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="b2aad-132">Only the regions supported by your subscription and resource group are displayed as options.</span></span>

    <span data-ttu-id="b2aad-133">e.</span><span class="sxs-lookup"><span data-stu-id="b2aad-133">e.</span></span> <span data-ttu-id="b2aad-134">**Účet úložiště** (volitelné): Účet úložiště Azure pro obecné účely, který přidružíte k účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-134">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="b2aad-135">Toto nastavení se doporučuje pro většinu účtů Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-135">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="b2aad-136">Další informace najdete v části [Propojený účet Azure Storage](#linked-azure-storage-account) níže v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="b2aad-136">See [Linked Azure Storage account](#linked-azure-storage-account) later in this article for more details.</span></span>

4. <span data-ttu-id="b2aad-137">Kliknutím na **Vytvořit** vytvořte účet.</span><span class="sxs-lookup"><span data-stu-id="b2aad-137">Click **Create** to create the account.</span></span>

   <span data-ttu-id="b2aad-138">Na portálu se zobrazí zpráva o tom, že nasazování probíhá.</span><span class="sxs-lookup"><span data-stu-id="b2aad-138">The portal indicates deployment is in progress.</span></span> <span data-ttu-id="b2aad-139">Po dokončení se v části **Oznámení** zobrazí zpráva **Nasazení se podařila**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-139">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>

## <a name="user-subscription-mode"></a><span data-ttu-id="b2aad-140">Režim předplatného uživatele</span><span class="sxs-lookup"><span data-stu-id="b2aad-140">User subscription mode</span></span>

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a><span data-ttu-id="b2aad-141">Povolení přístupu k předplatnému pro Azure Batch (jednorázová operace)</span><span class="sxs-lookup"><span data-stu-id="b2aad-141">Allow Azure Batch to access the subscription (one-time operation)</span></span>
<span data-ttu-id="b2aad-142">Při vytváření prvního účtu Batch v režimu předplatného uživatele proveďte následující kroky, abyste předplatné zaregistrovali ve službě Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-142">When creating your first Batch account in user subscription mode, perform the following steps to register your subscription with Batch.</span></span> <span data-ttu-id="b2aad-143">(Pokud jste tento postup již provedli, přejděte k další části.)</span><span class="sxs-lookup"><span data-stu-id="b2aad-143">(If you previously did this, skip to the next section.)</span></span>

1. <span data-ttu-id="b2aad-144">Přihlaste se na web [Azure Portal][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="b2aad-144">Sign in to the [Azure portal][azure_portal].</span></span>

2. <span data-ttu-id="b2aad-145">Klikněte na **Další služby** > **Předplatné** a klikněte na předplatné, které chcete pro účet Batch použít.</span><span class="sxs-lookup"><span data-stu-id="b2aad-145">Click **More Services** > **Subscriptions**, and click the subscription you want to use for the Batch account.</span></span>

3. <span data-ttu-id="b2aad-146">V okně **Předplatné** klikněte na **Řízení přístupu (IAM)** > **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-146">In the **Subscription** blade, click **Access control (IAM)** > **Add**.</span></span>

    ![Řízení přístupu pro předplatné][subscription_access]

4. <span data-ttu-id="b2aad-148">V okně **Přidejte oprávnění** vyberte roli **Přispěvatel** a vyhledejte rozhraní API služby Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-148">On the **Add permissions** blade, select the **Contributor** role, search for the Batch API.</span></span> <span data-ttu-id="b2aad-149">Hledejte každý z těchto řetězců, dokud nenajdete rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="b2aad-149">Search for each of these strings until you find the API:</span></span>
    1. <span data-ttu-id="b2aad-150">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-150">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="b2aad-151">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-151">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="b2aad-152">Novější tenanti služby Azure AD mohou používat tento název.</span><span class="sxs-lookup"><span data-stu-id="b2aad-152">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="b2aad-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** je ID rozhraní API služby Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** is the ID for the Batch API.</span></span> 

5. <span data-ttu-id="b2aad-154">Jakmile najdete rozhraní API služby Batch, vyberte ho a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-154">Once you find the Batch API, select it and click **Save**.</span></span>

    ![Přidání oprávnění pro službu Batch][add_permission]

### <a name="create-a-key-vault"></a><span data-ttu-id="b2aad-156">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="b2aad-156">Create a key vault</span></span>
<span data-ttu-id="b2aad-157">V režimu předplatného uživatele je vyžadován trezor klíčů Azure, který patří do stejné skupiny prostředků jako účet Batch, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b2aad-157">In user subscription mode, an Azure key vault is required that belongs to the same resource group as the Batch account to be created.</span></span> <span data-ttu-id="b2aad-158">Ověřte, že se skupina prostředků nachází v oblasti, kde je služba Batch [k dispozici](https://azure.microsoft.com/regions/services/) a která podporuje vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="b2aad-158">Make sure the resource group is in a region where Batch is [available](https://azure.microsoft.com/regions/services/) and which your subscription supports.</span></span>

1. <span data-ttu-id="b2aad-159">Na webu [Azure Portal][azure_portal] klikněte na **Nový** > **Zabezpečení + identita** > **Trezor klíčů**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-159">In the [Azure portal][azure_portal], click **New** > **Security + Identity** > **Key Vault**.</span></span>

2. <span data-ttu-id="b2aad-160">V okně **Vytvořit trezor klíčů** zadejte název pro trezor klíčů a vytvořte skupinu prostředků v oblasti, kterou chcete použít pro účet Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-160">In the **Create Key Vault** blade, enter a name for the key vault, and create a resource group in the region you want for your Batch account.</span></span> <span data-ttu-id="b2aad-161">Pro zbývající nastavení ponechejte výchozí hodnoty a pak klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-161">Leave the remaining settings at default values, then click **Create**.</span></span>

### <a name="create-a-batch-account"></a><span data-ttu-id="b2aad-162">Vytvoření účtu Batch</span><span class="sxs-lookup"><span data-stu-id="b2aad-162">Create a Batch account</span></span>

1. <span data-ttu-id="b2aad-163">Na webu [Azure Portal][azure_portal] klikněte na **Nový** > **Compute** > **Služba Batch**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-163">In the [Azure portal][azure_portal], click **New** > **Compute** > **Batch Service**.</span></span>

    ![Batch na webu Marketplace][marketplace_portal]
3. <span data-ttu-id="b2aad-165">Zobrazí se okno **Nový účet Batch**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-165">The **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="b2aad-166">Níže jsou uvedeny popisy jednotlivých prvků okna.</span><span class="sxs-lookup"><span data-stu-id="b2aad-166">See the descriptions below of each blade element.</span></span>

    ![Vytvoření účtu Batch][account_portal_byos]

    <span data-ttu-id="b2aad-168">a.</span><span class="sxs-lookup"><span data-stu-id="b2aad-168">a.</span></span> <span data-ttu-id="b2aad-169">**Název účtu:** Název účtu Batch, který zvolíte, musí být jedinečný v oblasti Azure, ve které se nový účet vytvoří (viz **Umístění** níže).</span><span class="sxs-lookup"><span data-stu-id="b2aad-169">**Account name**: The Batch account name you choose must be unique within the Azure region where the account is created (see **Location** below).</span></span> <span data-ttu-id="b2aad-170">Název účtu může obsahovat jenom malá písmena nebo čísla a musí být dlouhý 3 až 24 znaků.</span><span class="sxs-lookup"><span data-stu-id="b2aad-170">The account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="b2aad-171">b.</span><span class="sxs-lookup"><span data-stu-id="b2aad-171">b.</span></span> <span data-ttu-id="b2aad-172">**Předplatné:** Máte-li více předplatných, vyberte předplatné, které jste zaregistrovali ve službě Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-172">**Subscription**: If you have more than one subscription, select the subscription that you registered with the Batch service.</span></span>

    <span data-ttu-id="b2aad-173">c.</span><span class="sxs-lookup"><span data-stu-id="b2aad-173">c.</span></span> <span data-ttu-id="b2aad-174">**Režim přidělování fondů:** Vyberte **Předplatné uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-174">**Pool allocation mode**: Select **User subscription**.</span></span>

    <span data-ttu-id="b2aad-175">d.</span><span class="sxs-lookup"><span data-stu-id="b2aad-175">d.</span></span> <span data-ttu-id="b2aad-176">**Trezor klíčů:** Vyberte trezor klíčů, který jste vytvořili pro účet Batch v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="b2aad-176">**Key vault**: Select the key vault you created for your Batch account in the previous section.</span></span> <span data-ttu-id="b2aad-177">Volitelně můžete vytvořit nový trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="b2aad-177">Optionally, create a new key vault.</span></span> <span data-ttu-id="b2aad-178">Po výběru trezoru zaškrtnutím políčka udělte službě Azure Batch přístup k trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="b2aad-178">After selecting the vault, select the checkbox to grant Azure Batch access to the key vault.</span></span>

    <span data-ttu-id="b2aad-179">c.</span><span class="sxs-lookup"><span data-stu-id="b2aad-179">c.</span></span> <span data-ttu-id="b2aad-180">**Skupina prostředků:** Vyberte skupinu prostředků, ve které jste vytvořili trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="b2aad-180">**Resource group**: Select the resource group in which you  created the key vault.</span></span>

    <span data-ttu-id="b2aad-181">d.</span><span class="sxs-lookup"><span data-stu-id="b2aad-181">d.</span></span> <span data-ttu-id="b2aad-182">**Umístění:** Oblast Azure, ve které jste vytvořili trezor klíčů pro účet Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-182">**Location**: The Azure region in which you created the key vault for the Batch account.</span></span>

    <span data-ttu-id="b2aad-183">e.</span><span class="sxs-lookup"><span data-stu-id="b2aad-183">e.</span></span> <span data-ttu-id="b2aad-184">**Účet úložiště** (volitelné): Účet úložiště Azure pro obecné účely, který přidružíte k účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-184">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="b2aad-185">Toto nastavení se doporučuje pro většinu účtů Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-185">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="b2aad-186">Další informace najdete v části [Propojený účet Azure Storage](#linked-azure-storage-account) dole.</span><span class="sxs-lookup"><span data-stu-id="b2aad-186">See [Linked Azure Storage account](#linked-azure-storage-account) below for more details.</span></span>

4. <span data-ttu-id="b2aad-187">Kliknutím na **Vytvořit** vytvořte účet.</span><span class="sxs-lookup"><span data-stu-id="b2aad-187">Click **Create** to create the account.</span></span>

   <span data-ttu-id="b2aad-188">Na portálu se zobrazí zpráva o tom, že nasazování probíhá.</span><span class="sxs-lookup"><span data-stu-id="b2aad-188">The portal indicates deployment is in progress.</span></span> <span data-ttu-id="b2aad-189">Po dokončení se v části **Oznámení** zobrazí zpráva **Nasazení se podařila**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-189">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>



## <a name="view-batch-account-properties"></a><span data-ttu-id="b2aad-190">Zobrazení vlastností účtu Batch</span><span class="sxs-lookup"><span data-stu-id="b2aad-190">View Batch account properties</span></span>
<span data-ttu-id="b2aad-191">Po vytvoření účtu můžete otevřít **okno účtu Batch** pro přístup k jeho nastavením a vlastnostem.</span><span class="sxs-lookup"><span data-stu-id="b2aad-191">Once the account has been created, you can open the **Batch account blade** to access its settings and properties.</span></span> <span data-ttu-id="b2aad-192">Všechna nastavení a vlastnosti účtu jsou přístupná pomocí levé nabídky okna účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-192">You can access all account settings and properties by using the left menu of the Batch account blade.</span></span>

![Okno účtu Batch na webu Azure Portal][account_blade]

* <span data-ttu-id="b2aad-194">**Adresa URL účtu Batch**: Při vývoji aplikace pomocí [rozhraní API služby Batch](batch-apis-tools.md#azure-accounts-for-batch-development) budete pro přístup k prostředkům Batch potřebovat adresu URL účtu.</span><span class="sxs-lookup"><span data-stu-id="b2aad-194">**Batch account URL**: When you develop an application with the [Batch APIs](batch-apis-tools.md#azure-accounts-for-batch-development), you'll need an account URL to access your Batch resources.</span></span> <span data-ttu-id="b2aad-195">Adresa URL účtu Batch má následující formát:</span><span class="sxs-lookup"><span data-stu-id="b2aad-195">A Batch account URL has the following format:</span></span>

    `https://<account_name>.<region>.batch.azure.com`

![Adresa URL účtu Batch na portálu][account_url]

* <span data-ttu-id="b2aad-197">**Přístupové klíče** (režim služby Batch): Pro ověření přístupu k účtu Batch z vaší aplikace budete potřebovat přístupový klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="b2aad-197">**Access keys** (Batch service mode): To authenticate access to your Batch account from your application, you'll need an account access key.</span></span> <span data-ttu-id="b2aad-198">(Toto nastavení není k dispozici v režimu předplatného uživatele, pokud používáte ověřování pomocí Azure Active Directory.)</span><span class="sxs-lookup"><span data-stu-id="b2aad-198">(This setting is not available in user subscription mode, where you use Azure Active Directory authentication.)</span></span>

    <span data-ttu-id="b2aad-199">Chcete-li zobrazit nebo obnovit přístupový klíč účtu Batch, zadejte `keys` do pole **Hledat** v levé nabídce v okně účtu Batch a vyberte položku **Klíče**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-199">To view or regenerate your Batch account's access keys, enter `keys` in the left menu **Search** box on the Batch account blade, then select **Keys**.</span></span>

    ![Klíče účtu Batch na webu Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a><span data-ttu-id="b2aad-201">Propojený účet Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b2aad-201">Linked Azure Storage account</span></span>

<span data-ttu-id="b2aad-202">Volitelně můžete s účtem Batch propojit účet úložiště Azure pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="b2aad-202">You can optionally link a general-purpose Azure Storage account to your Batch account.</span></span> <span data-ttu-id="b2aad-203">Funkce [balíčků aplikací](batch-application-packages.md) služby Batch využívá úložiště objektů blob v Azure, stejně jako knihovna [.NET Batch File Conventions](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="b2aad-203">The [application packages](batch-application-packages.md) feature of Batch uses Azure Blob storage, as does the [Batch File Conventions .NET](batch-task-output.md) library.</span></span> <span data-ttu-id="b2aad-204">Tyto volitelné funkce vám pomohou při nasazení aplikací spouštěných vašimi úkoly Batch a při zachování jimi vytvářených dat.</span><span class="sxs-lookup"><span data-stu-id="b2aad-204">These optional features assist you in deploying the applications that your Batch tasks run, and persisting the data they produce.</span></span>

<span data-ttu-id="b2aad-205">Doporučujeme vytvořit nový účet úložiště pro výhradní použití vaším účtem Batch.</span><span class="sxs-lookup"><span data-stu-id="b2aad-205">We recommend that you create a new Storage account exclusively for use by your Batch account.</span></span>

![Vytvoření účtu úložiště pro obecné účely][storage_account]

> [!NOTE]
> <span data-ttu-id="b2aad-207">Azure Batch momentálně podporuje pouze typ účtu úložiště pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="b2aad-207">Azure Batch currently supports only the general-purpose Storage account type.</span></span> <span data-ttu-id="b2aad-208">Tento typ účtu je popsaný v kroku 5, [Vytvoření účtu úložiště] (../storage/common/storage-create-storage-account.md#create-a-storage-account), v tématu [O účtech úložiště Azure](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="b2aad-208">This account type is described in step 5, [Create a storage account] (../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
>
>

> [!WARNING]
> <span data-ttu-id="b2aad-209">Při obnovování přístupových klíčů v propojeném účtu úložiště buďte velmi opatrní.</span><span class="sxs-lookup"><span data-stu-id="b2aad-209">Be careful when regenerating the access keys of a linked Storage account.</span></span> <span data-ttu-id="b2aad-210">Obnovte vždy jen jeden klíč účtu úložiště a v okně propojeného účtu úložiště klikněte na možnost **Synchronizovat klíče**.</span><span class="sxs-lookup"><span data-stu-id="b2aad-210">Regenerate only one Storage account key and click **Sync Keys** on the linked Storage account blade.</span></span> <span data-ttu-id="b2aad-211">Počkejte 5 minut, aby se klíče rozšířily do výpočetních uzlů ve fondech, a potom v případě potřeby znovu vygenerujte a synchronizujte další klíč.</span><span class="sxs-lookup"><span data-stu-id="b2aad-211">Wait five minutes to allow the keys to propagate to the compute nodes in your pools, then regenerate and synchronize the other key if necessary.</span></span> <span data-ttu-id="b2aad-212">Pokud byste obnovili (znovu vygenerovali) oba klíče najednou, výpočetní uzly by nedokázaly synchronizovat ani jeden klíč a vy byste ztratili přístup k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b2aad-212">If you regenerate both keys at the same time, your compute nodes will not be able to synchronize either key, and they will lose access to the Storage account.</span></span>
>
>

<span data-ttu-id="b2aad-213">![Obnovování klíčů účtu úložiště][4]</span><span class="sxs-lookup"><span data-stu-id="b2aad-213">![Regenerating storage account keys][4]</span></span>

## <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="b2aad-214">Kvóty a omezení služby Batch</span><span class="sxs-lookup"><span data-stu-id="b2aad-214">Batch service quotas and limits</span></span>
<span data-ttu-id="b2aad-215">Pamatujte, že podobně jako pro předplatné a další služby Azure, i pro účty Batch platí určité [kvóty a omezení](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="b2aad-215">Please be aware that as with your Azure subscription and other Azure services, certain [quotas and limits](batch-quota-limit.md) apply to Batch accounts.</span></span> <span data-ttu-id="b2aad-216">Aktuální kvóty účtu Batch se zobrazují na portálu ve **vlastnostech** účtu.</span><span class="sxs-lookup"><span data-stu-id="b2aad-216">Current quotas for a Batch account appear in the portal in the account **Properties**.</span></span>

![Kvóty účtu Batch na webu Azure Portal][quotas]



<span data-ttu-id="b2aad-218">Mnohé z těchto kvót je také možné zvýšit jednoduchým požadavkem na bezplatnou podporu, odeslaným pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b2aad-218">Additionally, many of these quotas can be increased simply with a free product support request submitted in the Azure portal.</span></span> <span data-ttu-id="b2aad-219">Podrobnosti o vyžádání zvýšení kvóty viz [Kvóty a omezení pro službu Azure Batch](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="b2aad-219">See [Quotas and limits for the Azure Batch service](batch-quota-limit.md) for details on requesting quota increases.</span></span>

## <a name="other-batch-account-management-options"></a><span data-ttu-id="b2aad-220">Další možnosti správy účtu Batch</span><span class="sxs-lookup"><span data-stu-id="b2aad-220">Other Batch account management options</span></span>
<span data-ttu-id="b2aad-221">Kromě webu Azure Portal můžete účty Batch vytvářet a spravovat následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="b2aad-221">In addition to using the Azure portal, you can also create and manage Batch accounts with the following:</span></span>

* [<span data-ttu-id="b2aad-222">Rutiny PowerShellu pro účty Batch</span><span class="sxs-lookup"><span data-stu-id="b2aad-222">Batch PowerShell cmdlets</span></span>](batch-powershell-cmdlets-get-started.md)
* [<span data-ttu-id="b2aad-223">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b2aad-223">Azure CLI</span></span>](batch-cli-get-started.md)
* [<span data-ttu-id="b2aad-224">Knihovna Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="b2aad-224">Batch Management .NET</span></span>](batch-management-dotnet.md)

## <a name="next-steps"></a><span data-ttu-id="b2aad-225">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2aad-225">Next steps</span></span>
* <span data-ttu-id="b2aad-226">Další informace o koncepcích a funkcích služby Batch najdete v článku [Přehled funkcí Batch](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="b2aad-226">See the [Batch feature overview](batch-api-basics.md) to learn more about Batch service concepts and features.</span></span> <span data-ttu-id="b2aad-227">Článek popisuje primární prostředky služby Batch, například fondy, výpočetní uzly a úlohy a nabízí přehled funkcí služby, které umožňují spouštění velkých výpočetních úloh.</span><span class="sxs-lookup"><span data-stu-id="b2aad-227">The article discusses the primary Batch resources such as pools, compute nodes, jobs, and tasks, and provides an overview of the service's features that enable large-scale compute workload execution.</span></span>
* <span data-ttu-id="b2aad-228">Seznamte se se základy vývoje aplikací s podporou služby Batch pomocí [klientské knihovny Batch .NET](batch-dotnet-get-started.md) nebo [Pythonu](batch-python-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="b2aad-228">Learn the basics of developing a Batch-enabled application using the [Batch .NET client library](batch-dotnet-get-started.md) or [Python](batch-python-tutorial.md).</span></span> <span data-ttu-id="b2aad-229">Tyto úvodní články vás provedou funkční aplikací, která používá službu Batch ke spouštění úlohy na několika výpočetních uzlech, a představí vám použití služby Azure Storage k přípravě a načítání souborů úloh.</span><span class="sxs-lookup"><span data-stu-id="b2aad-229">These introductory articles guide you through a working application that uses the Batch service to execute a workload on multiple compute nodes, and includes using Azure Storage for workload file staging and retrieval.</span></span>

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
