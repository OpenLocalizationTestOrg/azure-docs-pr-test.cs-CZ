---
title: "prostředky účtu Batch aaaManage s hello Klientská knihovna pro .NET – Azure | Microsoft Docs"
description: "Vytvořit, odstranit a upravit prostředků účtu Azure Batch pomocí knihovny Batch Management .NET hello."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/24/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 638d8129f3841ffcfb2e10f5d531a319bcbb7701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-accounts-and-quotas-with-hello-batch-management-client-library-for-net"></a><span data-ttu-id="97311-103">Spravovat účty Batch a s klientské knihovny Batch Management hello kvóty pro rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="97311-103">Manage Batch accounts and quotas with hello Batch Management client library for .NET</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="97311-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="97311-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="97311-105">Knihovna Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="97311-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
> 
> 

<span data-ttu-id="97311-106">Můžete snížit údržby režie v aplikacích Azure Batch pomocí hello [rozhraní Batch Management .NET] [ api_mgmt_net] vytvořením účtu Batch tooautomate knihovny, odstranění, správu klíčů a kvóty zjišťování.</span><span class="sxs-lookup"><span data-stu-id="97311-106">You can lower maintenance overhead in your Azure Batch applications by using hello [Batch Management .NET][api_mgmt_net] library tooautomate Batch account creation, deletion, key management, and quota discovery.</span></span>

* <span data-ttu-id="97311-107">**Vytvářet a odstraňovat účty Batch** v libovolné oblasti.</span><span class="sxs-lookup"><span data-stu-id="97311-107">**Create and delete Batch accounts** within any region.</span></span> <span data-ttu-id="97311-108">Pokud například jako nezávislý dodavatel softwaru (ISV) z nich poskytovat služby pro klienty ve kterých každý je přiřazen samostatný účet Batch pro účely fakturace, můžete přidat účet vytváření a odstraňování možnosti tooyour zákaznický portál.</span><span class="sxs-lookup"><span data-stu-id="97311-108">If, as an independent software vendor (ISV) for example, you provide a service for your clients in which each is assigned a separate Batch account for billing purposes, you can add account creation and deletion capabilities tooyour customer portal.</span></span>
* <span data-ttu-id="97311-109">**Získat a obnovit klíče účtu** prostřednictvím kódu programu pro všechny účty Batch.</span><span class="sxs-lookup"><span data-stu-id="97311-109">**Retrieve and regenerate account keys** programmatically for any of your Batch accounts.</span></span> <span data-ttu-id="97311-110">To vám může pomoct souladu se zásadami zabezpečení, které vynucují pravidelné výměny nebo vypršení platnosti klíče účtu.</span><span class="sxs-lookup"><span data-stu-id="97311-110">This can help you comply with security policies that enforce periodic rollover or expiry of account keys.</span></span> <span data-ttu-id="97311-111">Pokud máte několik účty Batch v různých oblastech Azure, automatizaci procesu tato změna se zvyšuje efektivita vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="97311-111">When you have several Batch accounts in various Azure regions, automation of this rollover process increases your solution's efficiency.</span></span>
* <span data-ttu-id="97311-112">**Zkontrolujte účet kvóty** a proveďte hello zkušební verze a error složité a určení, které účty Batch mají jaké limity.</span><span class="sxs-lookup"><span data-stu-id="97311-112">**Check account quotas** and take hello trial-and-error guesswork out of determining which Batch accounts have what limits.</span></span> <span data-ttu-id="97311-113">Kontrolou kvóty vašeho účtu před spuštěním úlohy vytváření fondů nebo přidání výpočetní uzly, můžete upravit proaktivně kde nebo když tyto výpočetní prostředky jsou vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="97311-113">By checking your account quotas before starting jobs, creating pools, or adding compute nodes, you can proactively adjust where or when these compute resources are created.</span></span> <span data-ttu-id="97311-114">Můžete určit, které účty vyžadují, že kvóta zvyšuje před přiděluje další prostředky v těchto účtů.</span><span class="sxs-lookup"><span data-stu-id="97311-114">You can determine which accounts require quota increases before allocating additional resources in those accounts.</span></span>
* <span data-ttu-id="97311-115">**Kombinování funkcí jinými službami Azure** pro správu plnohodnotné prostředí – pomocí rozhraní Batch Management .NET [Azure Active Directory][aad_about]a hello [Azure Správce prostředků] [ resman_overview] společně v hello stejná aplikace.</span><span class="sxs-lookup"><span data-stu-id="97311-115">**Combine features of other Azure services** for a full-featured management experience--by using Batch Management .NET, [Azure Active Directory][aad_about], and hello [Azure Resource Manager][resman_overview] together in hello same application.</span></span> <span data-ttu-id="97311-116">Pomocí těchto funkcí a jejich rozhraní API můžete poskytnout prostředí hladký ověřování, hello možnost toocreate a odstranění skupiny prostředků a hello možností, které jsou popsané výše začátku do konce řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="97311-116">By using these features and their APIs, you can provide a frictionless authentication experience, hello ability toocreate and delete resource groups, and hello capabilities that are described above for an end-to-end management solution.</span></span>

> [!NOTE]
> <span data-ttu-id="97311-117">Když tento článek se zaměřuje na hello programové správy účtů Batch, klíče a kvóty, je mnoho tyto aktivity provést pomocí hello [portál Azure][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="97311-117">While this article focuses on hello programmatic management of your Batch accounts, keys, and quotas, you can perform many of these activities by using hello [Azure portal][azure_portal].</span></span> <span data-ttu-id="97311-118">Další informace najdete v tématu [vytvoření účtu Azure Batch pomocí portálu Azure hello](batch-account-create-portal.md) a [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="97311-118">For more information, see [Create an Azure Batch account using hello Azure portal](batch-account-create-portal.md) and [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span>
> 
> 

## <a name="create-and-delete-batch-accounts"></a><span data-ttu-id="97311-119">Vytvářet a odstraňovat účty Batch</span><span class="sxs-lookup"><span data-stu-id="97311-119">Create and delete Batch accounts</span></span>
<span data-ttu-id="97311-120">Jak je uvedeno, jeden z primární funkce hello hello Batch Management API je toocreate a odstraňte účty Batch v oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="97311-120">As mentioned, one of hello primary features of hello Batch Management API is toocreate and delete Batch accounts in an Azure region.</span></span> <span data-ttu-id="97311-121">Ano, použít toodo [BatchManagementClient.Account.CreateAsync] [ net_create] a [DeleteAsync][net_delete], nebo jejich synchronní protějšky.</span><span class="sxs-lookup"><span data-stu-id="97311-121">toodo so, use [BatchManagementClient.Account.CreateAsync][net_create] and [DeleteAsync][net_delete], or their synchronous counterparts.</span></span>

<span data-ttu-id="97311-122">Hello následující fragment kódu vytvoří účet, získá hello nově vytvořený účet z hello služby Batch a odstraní ji.</span><span class="sxs-lookup"><span data-stu-id="97311-122">hello following code snippet creates an account, obtains hello newly created account from hello Batch service, and then deletes it.</span></span> <span data-ttu-id="97311-123">V této fragmentu kódu a ostatní hello v tomto článku `batchManagementClient` je plně inicializovaném instanci [BatchManagementClient][net_mgmt_client].</span><span class="sxs-lookup"><span data-stu-id="97311-123">In this snippet and hello others in this article, `batchManagementClient` is a fully initialized instance of [BatchManagementClient][net_mgmt_client].</span></span>

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get hello new account from hello Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete hello account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [!NOTE]
> <span data-ttu-id="97311-124">Vyžadují aplikace, které používají knihovnu hello rozhraní Batch Management .NET a její třída BatchManagementClient **Správce služeb** nebo **spolusprávce** přístup toohello předplatné, které vlastní hello Spravované toobe účtu batch.</span><span class="sxs-lookup"><span data-stu-id="97311-124">Applications that use hello Batch Management .NET library and its BatchManagementClient class require **service administrator** or **coadministrator** access toohello subscription that owns hello Batch account toobe managed.</span></span> <span data-ttu-id="97311-125">Další informace najdete v tématu hello [Azure Active Directory](#azure-active-directory) části a hello [AccountManagement] [ acct_mgmt_sample] ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="97311-125">For more information, see hello [Azure Active Directory](#azure-active-directory) section and hello [AccountManagement][acct_mgmt_sample] code sample.</span></span>
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a><span data-ttu-id="97311-126">Získat a obnovit klíče účtu</span><span class="sxs-lookup"><span data-stu-id="97311-126">Retrieve and regenerate account keys</span></span>
<span data-ttu-id="97311-127">Získat účet primární a sekundární klíče z jakékoli účtu Batch v rámci vašeho předplatného pomocí [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="97311-127">Obtain primary and secondary account keys from any Batch account within your subscription by using [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="97311-128">Tyto klíče můžete vygenerovat pomocí [RegenerateKeyAsync][net_regenerate_keys].</span><span class="sxs-lookup"><span data-stu-id="97311-128">You can regenerate those keys by using [RegenerateKeyAsync][net_regenerate_keys].</span></span>

```csharp
// Get and print hello primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate hello primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [!TIP]
> <span data-ttu-id="97311-129">Zjednodušená připojení pracovního postupu můžete vytvořit pro správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="97311-129">You can create a streamlined connection workflow for your management applications.</span></span> <span data-ttu-id="97311-130">Nejdřív získat klíčem účtu pro účet Batch hello chcete toomanage s [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="97311-130">First, obtain an account key for hello Batch account you wish toomanage with [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="97311-131">Poté použijte tento klíč při inicializaci knihovny Batch .NET hello [BatchSharedKeyCredentials] [ net_sharedkeycred] třídy, která se používá při inicializaci [BatchClient] [net_batch_client].</span><span class="sxs-lookup"><span data-stu-id="97311-131">Then, use this key when initializing hello Batch .NET library's [BatchSharedKeyCredentials][net_sharedkeycred] class, which is used when initializing [BatchClient][net_batch_client].</span></span>
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a><span data-ttu-id="97311-132">Zkontrolujte předplatné a kvóty účtu Batch</span><span class="sxs-lookup"><span data-stu-id="97311-132">Check Azure subscription and Batch account quotas</span></span>
<span data-ttu-id="97311-133">Předplatná Azure a hello jednotlivých služeb Azure jako všechny služby Batch mají výchozí kvóty, které omezují počet hello určitých entit v nich.</span><span class="sxs-lookup"><span data-stu-id="97311-133">Azure subscriptions and hello individual Azure services like Batch all have default quotas that limit hello number of certain entities within them.</span></span> <span data-ttu-id="97311-134">Hello výchozí kvóty pro předplatná Azure, najdete v části [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="97311-134">For hello default quotas for Azure subscriptions, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="97311-135">Výchozí kvóty hello Dobrý den služby Batch, najdete v části [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="97311-135">For hello default quotas of hello Batch service, see [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span> <span data-ttu-id="97311-136">Pomocí knihovny Batch Management .NET hello můžete zkontrolovat těchto kvót ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="97311-136">By using hello Batch Management .NET library, you can check these quotas in your applications.</span></span> <span data-ttu-id="97311-137">To vám umožní rozhodnutí o přidělení toomake předtím, než přidáte účty nebo výpočetní prostředky, jako jsou fondy a výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="97311-137">This enables you toomake allocation decisions before you add accounts or compute resources like pools and compute nodes.</span></span>

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a><span data-ttu-id="97311-138">Zkontrolujte předplatné Azure pro kvóty účtu Batch</span><span class="sxs-lookup"><span data-stu-id="97311-138">Check an Azure subscription for Batch account quotas</span></span>
<span data-ttu-id="97311-139">Před vytvořením účtu Batch v oblasti, můžete zkontrolovat toosee vaše předplatné Azure, ať už jste možnost tooadd účtu v daném regionu.</span><span class="sxs-lookup"><span data-stu-id="97311-139">Before creating a Batch account in a region, you can check your Azure subscription toosee whether you are able tooadd an account in that region.</span></span>

<span data-ttu-id="97311-140">V následující fragment kódu hello, nejprve používáme [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] tooget kolekce všechny účty Batch, které jsou v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="97311-140">In hello code snippet below, we first use [BatchManagementClient.Account.ListAsync][net_mgmt_listaccounts] tooget a collection of all Batch accounts that are within a subscription.</span></span> <span data-ttu-id="97311-141">Po získání jsme tuto kolekci jsme určit, kolik účty jsou v hello cílová oblast.</span><span class="sxs-lookup"><span data-stu-id="97311-141">Once we've obtained this collection, we determine how many accounts are in hello target region.</span></span> <span data-ttu-id="97311-142">Využijeme [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] tooobtain hello kvóta účtu Batch a určit, kolik účty (pokud existuje) můžete vytvořit v této oblasti.</span><span class="sxs-lookup"><span data-stu-id="97311-142">Then we use [BatchManagementClient.Subscriptions][net_mgmt_subscriptions] tooobtain hello Batch account quota and determine how many accounts (if any) can be created in that region.</span></span>

```csharp
// Get a collection of all Batch accounts within hello subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within hello target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get hello account quota for hello specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in hello target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in hello {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

<span data-ttu-id="97311-143">Ve výše, hello fragmentu `creds` je instance [TokenCloudCredentials][azure_tokencreds].</span><span class="sxs-lookup"><span data-stu-id="97311-143">In hello snippet above, `creds` is an instance of [TokenCloudCredentials][azure_tokencreds].</span></span> <span data-ttu-id="97311-144">toosee příklad vytvořením tohoto objektu, najdete v části hello [AccountManagement] [ acct_mgmt_sample] ukázka kódu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="97311-144">toosee an example of creating this object, see hello [AccountManagement][acct_mgmt_sample] code sample on GitHub.</span></span>

### <a name="check-a-batch-account-for-compute-resource-quotas"></a><span data-ttu-id="97311-145">Zkontrolujte účet Batch u výpočetních prostředků kvóty</span><span class="sxs-lookup"><span data-stu-id="97311-145">Check a Batch account for compute resource quotas</span></span>
<span data-ttu-id="97311-146">Před zvýšením výpočetní prostředky ve vašem řešení Batch, můžete zkontrolovat tooensure hello prostředky můžete určit, že tooallocate nesmí překročit kvót účtu hello.</span><span class="sxs-lookup"><span data-stu-id="97311-146">Before increasing compute resources in your Batch solution, you can check tooensure hello resources you want tooallocate won't exceed hello account's quotas.</span></span> <span data-ttu-id="97311-147">V následující fragment kódu hello, jsme tisknout informace o kvótě hello pro hello s názvem účtu Batch `mybatchaccount`.</span><span class="sxs-lookup"><span data-stu-id="97311-147">In hello code snippet below, we print hello quota information for hello Batch account named `mybatchaccount`.</span></span> <span data-ttu-id="97311-148">Ve vaší vlastní aplikaci můžete použít takové informace toodetermine zda hello účtu může zpracovat toobe hello další prostředky vytvořit.</span><span class="sxs-lookup"><span data-stu-id="97311-148">In your own application, you could use such information toodetermine whether hello account can handle hello additional resources toobe created.</span></span>

```csharp
// First obtain hello Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print hello compute resource quotas for hello account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [!IMPORTANT]
> <span data-ttu-id="97311-149">Existuje výchozí kvóty pro předplatná Azure a služby, řada těchto mezních hodnot mohou být vyvolány po vydání požadavku v hello [portál Azure][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="97311-149">While there are default quotas for Azure subscriptions and services, many of these limits can be raised by issuing a request in hello [Azure portal][azure_portal].</span></span> <span data-ttu-id="97311-150">Například v tématu [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md) pokyny pro zvýšení vaší kvóty účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="97311-150">For example, see [Quotas and limits for hello Azure Batch service](batch-quota-limit.md) for instructions on increasing your Batch account quotas.</span></span>
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a><span data-ttu-id="97311-151">Používat Azure AD s Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="97311-151">Use Azure AD with Batch Management .NET</span></span>

<span data-ttu-id="97311-152">Knihovna rozhraní Batch Management .NET Hello je klient poskytovatel prostředků Azure a je použít v kombinaci s [Azure Resource Manager] [ resman_overview] toomanage účet prostředků prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="97311-152">hello Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] toomanage account resources programmatically.</span></span> <span data-ttu-id="97311-153">Azure AD je požadovaná tooauthenticate požadavky prostřednictvím všechny klienty poskytovatele prostředků Azure, včetně hello knihovny Batch Management .NET a prostřednictvím [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="97311-153">Azure AD is required tooauthenticate requests made through any Azure resource provider client, including hello Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span> <span data-ttu-id="97311-154">Informace o používání služby Azure AD pomocí knihovny Batch Management .NET hello najdete v tématu [řešení Batch pomocí Azure Active Directory tooauthenticate](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="97311-154">For information about using Azure AD with hello Batch Management .NET library, see [Use Azure Active Directory tooauthenticate Batch solutions](batch-aad-auth.md).</span></span> 

## <a name="sample-project-on-github"></a><span data-ttu-id="97311-155">Ukázkový projekt na Githubu</span><span class="sxs-lookup"><span data-stu-id="97311-155">Sample project on GitHub</span></span>

<span data-ttu-id="97311-156">toosee rozhraní Batch Management .NET v akci, podívejte se na hello [AccountManagment] [ acct_mgmt_sample] ukázkového projektu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="97311-156">toosee Batch Management .NET in action, check out hello [AccountManagment][acct_mgmt_sample] sample project on GitHub.</span></span> <span data-ttu-id="97311-157">Hello AccountManagment ukázkovou aplikaci ukazuje hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="97311-157">hello AccountManagment sample application demonstrates hello following operations:</span></span>

1. <span data-ttu-id="97311-158">Získat token zabezpečení ze služby Azure AD pomocí [ADAL][aad_adal].</span><span class="sxs-lookup"><span data-stu-id="97311-158">Acquire a security token from Azure AD by using [ADAL][aad_adal].</span></span> <span data-ttu-id="97311-159">Pokud již není přihlášený uživatel hello, bude vyzván k zadání přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="97311-159">If hello user is not already signed in, they are prompted for their Azure credentials.</span></span>
2. <span data-ttu-id="97311-160">Pomocí tokenu zabezpečení hello získaného z Azure AD, vytvářet [SubscriptionClient] [ resman_subclient] tooquery Azure seznam předplatná spojená s účtem hello.</span><span class="sxs-lookup"><span data-stu-id="97311-160">With hello security token obtained from Azure AD, create a [SubscriptionClient][resman_subclient] tooquery Azure for a list of subscriptions associated with hello account.</span></span> <span data-ttu-id="97311-161">Hello uživatele můžete vybrat odběr, ze seznamu hello, pokud obsahuje více než jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="97311-161">hello user can select a subscription from hello list if it contains more than one subscription.</span></span>
3. <span data-ttu-id="97311-162">Získání přihlašovacích údajů, které jsou přidružené k předplatnému hello vybrané.</span><span class="sxs-lookup"><span data-stu-id="97311-162">Get credentials associated with hello selected subscription.</span></span>
4. <span data-ttu-id="97311-163">Vytvoření [ResourceManagementClient] [ resman_client] objekt pomocí přihlašovacích údajů hello.</span><span class="sxs-lookup"><span data-stu-id="97311-163">Create a [ResourceManagementClient][resman_client] object by using hello credentials.</span></span>
5. <span data-ttu-id="97311-164">Použití [ResourceManagementClient] [ resman_client] objektu toocreate skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="97311-164">Use a [ResourceManagementClient][resman_client] object toocreate a resource group.</span></span>
6. <span data-ttu-id="97311-165">Použití [BatchManagementClient] [ net_mgmt_client] objektu tooperform několik operací účtu Batch:</span><span class="sxs-lookup"><span data-stu-id="97311-165">Use a [BatchManagementClient][net_mgmt_client] object tooperform several Batch account operations:</span></span>
   * <span data-ttu-id="97311-166">Vytvořte účet Batch v hello novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="97311-166">Create a Batch account in hello new resource group.</span></span>
   * <span data-ttu-id="97311-167">Získáte hello nově vytvořený účet z hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="97311-167">Get hello newly created account from hello Batch service.</span></span>
   * <span data-ttu-id="97311-168">Tisk hello klíče účtu pro nový účet hello.</span><span class="sxs-lookup"><span data-stu-id="97311-168">Print hello account keys for hello new account.</span></span>
   * <span data-ttu-id="97311-169">Znovu vygenerujte nový primární klíč pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="97311-169">Regenerate a new primary key for hello account.</span></span>
   * <span data-ttu-id="97311-170">Tisk – informace o kvótě hello hello účtu.</span><span class="sxs-lookup"><span data-stu-id="97311-170">Print hello quota information for hello account.</span></span>
   * <span data-ttu-id="97311-171">Tisk – informace o kvótě hello hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="97311-171">Print hello quota information for hello subscription.</span></span>
   * <span data-ttu-id="97311-172">Tisk – všechny účty v rámci předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="97311-172">Print all accounts within hello subscription.</span></span>
   * <span data-ttu-id="97311-173">Nově vytvořený účet odstraňte.</span><span class="sxs-lookup"><span data-stu-id="97311-173">Delete newly created account.</span></span>
7. <span data-ttu-id="97311-174">Odstraňte skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="97311-174">Delete hello resource group.</span></span>

<span data-ttu-id="97311-175">Před odstraněním skupiny účtů a prostředků Batch hello nově vytvořený, zobrazí se jim v hello [portál Azure][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="97311-175">Before deleting hello newly created Batch account and resource group, you can view them in hello [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="97311-176">Ukázková aplikace hello toorun úspěšně, nejprve je nutné zaregistrovat ho s klientovi Azure AD v hello Azure portal a udělit oprávnění toohello rozhraní API Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="97311-176">toorun hello sample application successfully, you must first register it with your Azure AD tenant in hello Azure portal and grant permissions toohello Azure Resource Manager API.</span></span> <span data-ttu-id="97311-177">Postupujte podle kroků hello součástí [řešení pro správu Batch ověření pomocí služby Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="97311-177">Follow hello steps provided in [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span>


[aad_about]: ../active-directory/active-directory-whatis.md "Co je Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Scénáře ověřování pro Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrace aplikací s Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
