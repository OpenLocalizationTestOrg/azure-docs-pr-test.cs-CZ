---
title: "Správa prostředků účtu Batch pomocí klientské knihovny pro platformu .NET – Azure | Microsoft Docs"
description: "Vytvořit, odstranit a upravit prostředků účtu Azure Batch pomocí knihovny Batch Management .NET."
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
ms.openlocfilehash: eafde9258222a2ab09ade2e366f9cc595a303dec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-batch-accounts-and-quotas-with-the-batch-management-client-library-for-net"></a><span data-ttu-id="6a98d-103">Spravovat účty Batch a kvóty pomocí klientské knihovny správy Batch pro .NET</span><span class="sxs-lookup"><span data-stu-id="6a98d-103">Manage Batch accounts and quotas with the Batch Management client library for .NET</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6a98d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6a98d-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="6a98d-105">Knihovna Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="6a98d-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
> 
> 

<span data-ttu-id="6a98d-106">Můžete snížit údržby režie v aplikacích Azure Batch pomocí [rozhraní Batch Management .NET] [ api_mgmt_net] knihovna k automatizaci vytváření účtu Batch, odstranění, správu klíčů a kvóty zjišťování.</span><span class="sxs-lookup"><span data-stu-id="6a98d-106">You can lower maintenance overhead in your Azure Batch applications by using the [Batch Management .NET][api_mgmt_net] library to automate Batch account creation, deletion, key management, and quota discovery.</span></span>

* <span data-ttu-id="6a98d-107">**Vytvářet a odstraňovat účty Batch** v libovolné oblasti.</span><span class="sxs-lookup"><span data-stu-id="6a98d-107">**Create and delete Batch accounts** within any region.</span></span> <span data-ttu-id="6a98d-108">Pokud například jako nezávislý dodavatel softwaru (ISV) z nich poskytovat služby pro klienty ve kterých každý je přiřazen samostatný účet Batch pro účely fakturace, můžete přidat možnosti vytváření a odstraňování účtu na portál zákazníka.</span><span class="sxs-lookup"><span data-stu-id="6a98d-108">If, as an independent software vendor (ISV) for example, you provide a service for your clients in which each is assigned a separate Batch account for billing purposes, you can add account creation and deletion capabilities to your customer portal.</span></span>
* <span data-ttu-id="6a98d-109">**Získat a obnovit klíče účtu** prostřednictvím kódu programu pro všechny účty Batch.</span><span class="sxs-lookup"><span data-stu-id="6a98d-109">**Retrieve and regenerate account keys** programmatically for any of your Batch accounts.</span></span> <span data-ttu-id="6a98d-110">To vám může pomoct souladu se zásadami zabezpečení, které vynucují pravidelné výměny nebo vypršení platnosti klíče účtu.</span><span class="sxs-lookup"><span data-stu-id="6a98d-110">This can help you comply with security policies that enforce periodic rollover or expiry of account keys.</span></span> <span data-ttu-id="6a98d-111">Pokud máte několik účty Batch v různých oblastech Azure, automatizaci procesu tato změna se zvyšuje efektivita vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="6a98d-111">When you have several Batch accounts in various Azure regions, automation of this rollover process increases your solution's efficiency.</span></span>
* <span data-ttu-id="6a98d-112">**Zkontrolujte účet kvóty** a určení, které účty Batch mají jaké limity průběhu zkušební verze a chyby.</span><span class="sxs-lookup"><span data-stu-id="6a98d-112">**Check account quotas** and take the trial-and-error guesswork out of determining which Batch accounts have what limits.</span></span> <span data-ttu-id="6a98d-113">Kontrolou kvóty vašeho účtu před spuštěním úlohy vytváření fondů nebo přidání výpočetní uzly, můžete upravit proaktivně kde nebo když tyto výpočetní prostředky jsou vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="6a98d-113">By checking your account quotas before starting jobs, creating pools, or adding compute nodes, you can proactively adjust where or when these compute resources are created.</span></span> <span data-ttu-id="6a98d-114">Můžete určit, které účty vyžadují, že kvóta zvyšuje před přiděluje další prostředky v těchto účtů.</span><span class="sxs-lookup"><span data-stu-id="6a98d-114">You can determine which accounts require quota increases before allocating additional resources in those accounts.</span></span>
* <span data-ttu-id="6a98d-115">**Kombinování funkcí jinými službami Azure** pro správu plnohodnotné prostředí – pomocí rozhraní Batch Management .NET [Azure Active Directory][aad_about]a [Azure Resource Manager] [ resman_overview] společně ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6a98d-115">**Combine features of other Azure services** for a full-featured management experience--by using Batch Management .NET, [Azure Active Directory][aad_about], and the [Azure Resource Manager][resman_overview] together in the same application.</span></span> <span data-ttu-id="6a98d-116">Pomocí těchto funkcí a jejich rozhraní API mohou poskytnout prostředí hladký ověřování, umožňuje vytvářet a odstraňovat skupiny prostředků a možnosti, které jsou popsané výše začátku do konce řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="6a98d-116">By using these features and their APIs, you can provide a frictionless authentication experience, the ability to create and delete resource groups, and the capabilities that are described above for an end-to-end management solution.</span></span>

> [!NOTE]
> <span data-ttu-id="6a98d-117">Když tento článek se zaměřuje na programové správy účtů Batch, klíče a kvóty, je mnoho tyto aktivity provést pomocí [portál Azure][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="6a98d-117">While this article focuses on the programmatic management of your Batch accounts, keys, and quotas, you can perform many of these activities by using the [Azure portal][azure_portal].</span></span> <span data-ttu-id="6a98d-118">Další informace najdete v tématu [vytvoření účtu Azure Batch pomocí portálu Azure](batch-account-create-portal.md) a [kvóty a omezení pro službu Azure Batch](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="6a98d-118">For more information, see [Create an Azure Batch account using the Azure portal](batch-account-create-portal.md) and [Quotas and limits for the Azure Batch service](batch-quota-limit.md).</span></span>
> 
> 

## <a name="create-and-delete-batch-accounts"></a><span data-ttu-id="6a98d-119">Vytvářet a odstraňovat účty Batch</span><span class="sxs-lookup"><span data-stu-id="6a98d-119">Create and delete Batch accounts</span></span>
<span data-ttu-id="6a98d-120">Jak je uvedeno, jeden z primární funkce rozhraní API pro správu Batch je vytvářet a odstraňovat účty Batch v oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="6a98d-120">As mentioned, one of the primary features of the Batch Management API is to create and delete Batch accounts in an Azure region.</span></span> <span data-ttu-id="6a98d-121">Chcete-li to provést, použijte [BatchManagementClient.Account.CreateAsync] [ net_create] a [DeleteAsync][net_delete], nebo jejich synchronní protějšky.</span><span class="sxs-lookup"><span data-stu-id="6a98d-121">To do so, use [BatchManagementClient.Account.CreateAsync][net_create] and [DeleteAsync][net_delete], or their synchronous counterparts.</span></span>

<span data-ttu-id="6a98d-122">Následující fragment kódu vytvoří účet, nově vytvořený účet získá od služby Batch a odstraní ji.</span><span class="sxs-lookup"><span data-stu-id="6a98d-122">The following code snippet creates an account, obtains the newly created account from the Batch service, and then deletes it.</span></span> <span data-ttu-id="6a98d-123">V tento fragment kódu a jiné v tomto článku `batchManagementClient` je plně inicializovaném instanci [BatchManagementClient][net_mgmt_client].</span><span class="sxs-lookup"><span data-stu-id="6a98d-123">In this snippet and the others in this article, `batchManagementClient` is a fully initialized instance of [BatchManagementClient][net_mgmt_client].</span></span>

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [!NOTE]
> <span data-ttu-id="6a98d-124">Vyžadují aplikace, které používají knihovnu rozhraní Batch Management .NET a její třída BatchManagementClient **Správce služeb** nebo **spolusprávce** přístupu k odběru, který vlastní účet Batch, který chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="6a98d-124">Applications that use the Batch Management .NET library and its BatchManagementClient class require **service administrator** or **coadministrator** access to the subscription that owns the Batch account to be managed.</span></span> <span data-ttu-id="6a98d-125">Další informace najdete v tématu [Azure Active Directory](#azure-active-directory) části a [AccountManagement] [ acct_mgmt_sample] ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="6a98d-125">For more information, see the [Azure Active Directory](#azure-active-directory) section and the [AccountManagement][acct_mgmt_sample] code sample.</span></span>
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a><span data-ttu-id="6a98d-126">Získat a obnovit klíče účtu</span><span class="sxs-lookup"><span data-stu-id="6a98d-126">Retrieve and regenerate account keys</span></span>
<span data-ttu-id="6a98d-127">Získat účet primární a sekundární klíče z jakékoli účtu Batch v rámci vašeho předplatného pomocí [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="6a98d-127">Obtain primary and secondary account keys from any Batch account within your subscription by using [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="6a98d-128">Tyto klíče můžete vygenerovat pomocí [RegenerateKeyAsync][net_regenerate_keys].</span><span class="sxs-lookup"><span data-stu-id="6a98d-128">You can regenerate those keys by using [RegenerateKeyAsync][net_regenerate_keys].</span></span>

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [!TIP]
> <span data-ttu-id="6a98d-129">Zjednodušená připojení pracovního postupu můžete vytvořit pro správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="6a98d-129">You can create a streamlined connection workflow for your management applications.</span></span> <span data-ttu-id="6a98d-130">Nejdřív získat klíč účtu pro účet Batch, které chcete spravovat pomocí [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="6a98d-130">First, obtain an account key for the Batch account you wish to manage with [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="6a98d-131">Poté použijte tento klíč při inicializaci knihovny Batch .NET [BatchSharedKeyCredentials] [ net_sharedkeycred] třídy, která se používá při inicializaci [BatchClient][net_batch_client].</span><span class="sxs-lookup"><span data-stu-id="6a98d-131">Then, use this key when initializing the Batch .NET library's [BatchSharedKeyCredentials][net_sharedkeycred] class, which is used when initializing [BatchClient][net_batch_client].</span></span>
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a><span data-ttu-id="6a98d-132">Zkontrolujte předplatné a kvóty účtu Batch</span><span class="sxs-lookup"><span data-stu-id="6a98d-132">Check Azure subscription and Batch account quotas</span></span>
<span data-ttu-id="6a98d-133">Předplatná Azure a jednotlivé služby Azure, jako jsou všechny služby Batch mají výchozí kvóty, které omezí počet určitých entit v nich.</span><span class="sxs-lookup"><span data-stu-id="6a98d-133">Azure subscriptions and the individual Azure services like Batch all have default quotas that limit the number of certain entities within them.</span></span> <span data-ttu-id="6a98d-134">Výchozí kvóty pro předplatná Azure, najdete v části [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="6a98d-134">For the default quotas for Azure subscriptions, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="6a98d-135">Výchozí kvóty služby Batch, najdete v části [kvóty a omezení pro službu Azure Batch](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="6a98d-135">For the default quotas of the Batch service, see [Quotas and limits for the Azure Batch service](batch-quota-limit.md).</span></span> <span data-ttu-id="6a98d-136">Pomocí knihovny Batch Management .NET můžete zkontrolovat těchto kvót ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="6a98d-136">By using the Batch Management .NET library, you can check these quotas in your applications.</span></span> <span data-ttu-id="6a98d-137">Můžete provést rozhodnutí o přidělení, než přidáte účty nebo výpočetní prostředky, jako jsou fondy a výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="6a98d-137">This enables you to make allocation decisions before you add accounts or compute resources like pools and compute nodes.</span></span>

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a><span data-ttu-id="6a98d-138">Zkontrolujte předplatné Azure pro kvóty účtu Batch</span><span class="sxs-lookup"><span data-stu-id="6a98d-138">Check an Azure subscription for Batch account quotas</span></span>
<span data-ttu-id="6a98d-139">Před vytvořením účtu Batch v oblasti, můžete zkontrolovat vašeho předplatného Azure, pokud chcete zobrazit, zda budete moci přidat účet v této oblasti.</span><span class="sxs-lookup"><span data-stu-id="6a98d-139">Before creating a Batch account in a region, you can check your Azure subscription to see whether you are able to add an account in that region.</span></span>

<span data-ttu-id="6a98d-140">V následujícím fragmentu kódu, nejprve používáme [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] získat kolekce všechny účty Batch, které jsou v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="6a98d-140">In the code snippet below, we first use [BatchManagementClient.Account.ListAsync][net_mgmt_listaccounts] to get a collection of all Batch accounts that are within a subscription.</span></span> <span data-ttu-id="6a98d-141">Po získání jsme tuto kolekci jsme určit, kolik účty jsou v cílové oblasti.</span><span class="sxs-lookup"><span data-stu-id="6a98d-141">Once we've obtained this collection, we determine how many accounts are in the target region.</span></span> <span data-ttu-id="6a98d-142">Využijeme [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] získat kvóta účtu Batch a určit, kolik účty (pokud existuje) můžete vytvořit v této oblasti.</span><span class="sxs-lookup"><span data-stu-id="6a98d-142">Then we use [BatchManagementClient.Subscriptions][net_mgmt_subscriptions] to obtain the Batch account quota and determine how many accounts (if any) can be created in that region.</span></span>

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

<span data-ttu-id="6a98d-143">Ve výše uvedeném fragmentu `creds` je instance [TokenCloudCredentials][azure_tokencreds].</span><span class="sxs-lookup"><span data-stu-id="6a98d-143">In the snippet above, `creds` is an instance of [TokenCloudCredentials][azure_tokencreds].</span></span> <span data-ttu-id="6a98d-144">Příklad vytvoření tohoto objektu najdete v sekci [AccountManagement] [ acct_mgmt_sample] ukázka kódu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="6a98d-144">To see an example of creating this object, see the [AccountManagement][acct_mgmt_sample] code sample on GitHub.</span></span>

### <a name="check-a-batch-account-for-compute-resource-quotas"></a><span data-ttu-id="6a98d-145">Zkontrolujte účet Batch u výpočetních prostředků kvóty</span><span class="sxs-lookup"><span data-stu-id="6a98d-145">Check a Batch account for compute resource quotas</span></span>
<span data-ttu-id="6a98d-146">Před zvýšením výpočetní prostředky ve vašem řešení Batch, můžete zkontrolovat, aby prostředky, které chcete přidělit nesmí překročit kvóty účtu.</span><span class="sxs-lookup"><span data-stu-id="6a98d-146">Before increasing compute resources in your Batch solution, you can check to ensure the resources you want to allocate won't exceed the account's quotas.</span></span> <span data-ttu-id="6a98d-147">V následujícím fragmentu kódu, jsme tisknout informace o kvótě pro dávkový účet s názvem `mybatchaccount`.</span><span class="sxs-lookup"><span data-stu-id="6a98d-147">In the code snippet below, we print the quota information for the Batch account named `mybatchaccount`.</span></span> <span data-ttu-id="6a98d-148">Ve vaší vlastní aplikaci můžete tyto informace použít k určení, zda účet dokáže zpracovat další prostředky, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="6a98d-148">In your own application, you could use such information to determine whether the account can handle the additional resources to be created.</span></span>

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [!IMPORTANT]
> <span data-ttu-id="6a98d-149">Existuje výchozí kvóty pro předplatná Azure a služby, řada těchto mezních hodnot mohou být vyvolány po vydání požadavku v [portál Azure][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="6a98d-149">While there are default quotas for Azure subscriptions and services, many of these limits can be raised by issuing a request in the [Azure portal][azure_portal].</span></span> <span data-ttu-id="6a98d-150">Například v tématu [kvóty a omezení pro službu Azure Batch](batch-quota-limit.md) pokyny pro zvýšení vaší kvóty účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="6a98d-150">For example, see [Quotas and limits for the Azure Batch service](batch-quota-limit.md) for instructions on increasing your Batch account quotas.</span></span>
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a><span data-ttu-id="6a98d-151">Používat Azure AD s Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="6a98d-151">Use Azure AD with Batch Management .NET</span></span>

<span data-ttu-id="6a98d-152">Knihovně rozhraní Batch Management .NET je klient poskytovatel prostředků Azure a je použít v kombinaci s [Azure Resource Manager] [ resman_overview] ke správě prostředků účtu prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="6a98d-152">The Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] to manage account resources programmatically.</span></span> <span data-ttu-id="6a98d-153">Azure AD je vyžadovaný k ověření požadavky prostřednictvím všechny klienty poskytovatele prostředků Azure, včetně knihovny Batch Management .NET a prostřednictvím [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="6a98d-153">Azure AD is required to authenticate requests made through any Azure resource provider client, including the Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span> <span data-ttu-id="6a98d-154">Informace o používání služby Azure AD pomocí knihovny Batch Management .NET najdete v tématu [pomocí Azure Active Directory k ověření řešení Batch](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="6a98d-154">For information about using Azure AD with the Batch Management .NET library, see [Use Azure Active Directory to authenticate Batch solutions](batch-aad-auth.md).</span></span> 

## <a name="sample-project-on-github"></a><span data-ttu-id="6a98d-155">Ukázkový projekt na Githubu</span><span class="sxs-lookup"><span data-stu-id="6a98d-155">Sample project on GitHub</span></span>

<span data-ttu-id="6a98d-156">Pokud chcete zobrazit rozhraní Batch Management .NET v akci, podívejte se [AccountManagment] [ acct_mgmt_sample] ukázkového projektu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="6a98d-156">To see Batch Management .NET in action, check out the [AccountManagment][acct_mgmt_sample] sample project on GitHub.</span></span> <span data-ttu-id="6a98d-157">Ukázkovou aplikaci AccountManagment ukazuje následující operace:</span><span class="sxs-lookup"><span data-stu-id="6a98d-157">The AccountManagment sample application demonstrates the following operations:</span></span>

1. <span data-ttu-id="6a98d-158">Získat token zabezpečení ze služby Azure AD pomocí [ADAL][aad_adal].</span><span class="sxs-lookup"><span data-stu-id="6a98d-158">Acquire a security token from Azure AD by using [ADAL][aad_adal].</span></span> <span data-ttu-id="6a98d-159">Pokud již není přihlášený uživatel, bude vyzván k zadání přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="6a98d-159">If the user is not already signed in, they are prompted for their Azure credentials.</span></span>
2. <span data-ttu-id="6a98d-160">Pomocí tokenu zabezpečení získaného z Azure AD, vytvářet [SubscriptionClient] [ resman_subclient] dotazu Azure seznam předplatných přidružené k účtu.</span><span class="sxs-lookup"><span data-stu-id="6a98d-160">With the security token obtained from Azure AD, create a [SubscriptionClient][resman_subclient] to query Azure for a list of subscriptions associated with the account.</span></span> <span data-ttu-id="6a98d-161">Uživatel může vybrat odběr, v seznamu, pokud obsahuje více než jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="6a98d-161">The user can select a subscription from the list if it contains more than one subscription.</span></span>
3. <span data-ttu-id="6a98d-162">Získání přihlašovacích údajů, které jsou přidružené k vybranému předplatnému.</span><span class="sxs-lookup"><span data-stu-id="6a98d-162">Get credentials associated with the selected subscription.</span></span>
4. <span data-ttu-id="6a98d-163">Vytvoření [ResourceManagementClient] [ resman_client] objekt pomocí přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="6a98d-163">Create a [ResourceManagementClient][resman_client] object by using the credentials.</span></span>
5. <span data-ttu-id="6a98d-164">Použití [ResourceManagementClient] [ resman_client] objekt, který chcete vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6a98d-164">Use a [ResourceManagementClient][resman_client] object to create a resource group.</span></span>
6. <span data-ttu-id="6a98d-165">Použití [BatchManagementClient] [ net_mgmt_client] objekt pro provedení několik operací účtu Batch:</span><span class="sxs-lookup"><span data-stu-id="6a98d-165">Use a [BatchManagementClient][net_mgmt_client] object to perform several Batch account operations:</span></span>
   * <span data-ttu-id="6a98d-166">Vytvořte účet Batch do nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6a98d-166">Create a Batch account in the new resource group.</span></span>
   * <span data-ttu-id="6a98d-167">Nově vytvořený účet získáte ze služby Batch.</span><span class="sxs-lookup"><span data-stu-id="6a98d-167">Get the newly created account from the Batch service.</span></span>
   * <span data-ttu-id="6a98d-168">Tisk klíče účtu pro nový účet.</span><span class="sxs-lookup"><span data-stu-id="6a98d-168">Print the account keys for the new account.</span></span>
   * <span data-ttu-id="6a98d-169">Znovu vygenerujte nový primární klíč pro účet.</span><span class="sxs-lookup"><span data-stu-id="6a98d-169">Regenerate a new primary key for the account.</span></span>
   * <span data-ttu-id="6a98d-170">Tisk – informace o kvótě pro účet.</span><span class="sxs-lookup"><span data-stu-id="6a98d-170">Print the quota information for the account.</span></span>
   * <span data-ttu-id="6a98d-171">Tisk – informace o kvótě pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="6a98d-171">Print the quota information for the subscription.</span></span>
   * <span data-ttu-id="6a98d-172">Tisk – všechny účty v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="6a98d-172">Print all accounts within the subscription.</span></span>
   * <span data-ttu-id="6a98d-173">Nově vytvořený účet odstraňte.</span><span class="sxs-lookup"><span data-stu-id="6a98d-173">Delete newly created account.</span></span>
7. <span data-ttu-id="6a98d-174">Odstraňte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6a98d-174">Delete the resource group.</span></span>

<span data-ttu-id="6a98d-175">Před odstraněním nově vytvořenou skupinu účtů a prostředků Batch, lze je zobrazit v [portál Azure][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="6a98d-175">Before deleting the newly created Batch account and resource group, you can view them in the [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="6a98d-176">K úspěšnému spuštění ukázkové aplikace, musíte nejprve zaregistrovat ji pomocí vašeho klienta Azure AD na portálu Azure a udělte oprávnění na rozhraní API služby Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6a98d-176">To run the sample application successfully, you must first register it with your Azure AD tenant in the Azure portal and grant permissions to the Azure Resource Manager API.</span></span> <span data-ttu-id="6a98d-177">Postupujte podle kroků uvedených v [řešení pro správu Batch ověření pomocí služby Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="6a98d-177">Follow the steps provided in [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span>


<span data-ttu-id="6a98d-178">[aad_about]: ../active-directory/active-directory-whatis.md "Co je Azure Active Directory?"</span><span class="sxs-lookup"><span data-stu-id="6a98d-178">[aad_about]: ../active-directory/active-directory-whatis.md "What is Azure Active Directory?"</span></span>
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
<span data-ttu-id="6a98d-179">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Scénáře ověřování pro Azure AD"</span><span class="sxs-lookup"><span data-stu-id="6a98d-179">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentication Scenarios for Azure AD"</span></span>
<span data-ttu-id="6a98d-180">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrace aplikací s Azure Active Directory"</span><span class="sxs-lookup"><span data-stu-id="6a98d-180">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrating Applications with Azure Active Directory"</span></span>
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
