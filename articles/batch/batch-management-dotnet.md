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
# <a name="manage-batch-accounts-and-quotas-with-hello-batch-management-client-library-for-net"></a>Spravovat účty Batch a s klientské knihovny Batch Management hello kvóty pro rozhraní .NET

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Knihovna Batch Management .NET](batch-management-dotnet.md)
> 
> 

Můžete snížit údržby režie v aplikacích Azure Batch pomocí hello [rozhraní Batch Management .NET] [ api_mgmt_net] vytvořením účtu Batch tooautomate knihovny, odstranění, správu klíčů a kvóty zjišťování.

* **Vytvářet a odstraňovat účty Batch** v libovolné oblasti. Pokud například jako nezávislý dodavatel softwaru (ISV) z nich poskytovat služby pro klienty ve kterých každý je přiřazen samostatný účet Batch pro účely fakturace, můžete přidat účet vytváření a odstraňování možnosti tooyour zákaznický portál.
* **Získat a obnovit klíče účtu** prostřednictvím kódu programu pro všechny účty Batch. To vám může pomoct souladu se zásadami zabezpečení, které vynucují pravidelné výměny nebo vypršení platnosti klíče účtu. Pokud máte několik účty Batch v různých oblastech Azure, automatizaci procesu tato změna se zvyšuje efektivita vaše řešení.
* **Zkontrolujte účet kvóty** a proveďte hello zkušební verze a error složité a určení, které účty Batch mají jaké limity. Kontrolou kvóty vašeho účtu před spuštěním úlohy vytváření fondů nebo přidání výpočetní uzly, můžete upravit proaktivně kde nebo když tyto výpočetní prostředky jsou vytvořeny. Můžete určit, které účty vyžadují, že kvóta zvyšuje před přiděluje další prostředky v těchto účtů.
* **Kombinování funkcí jinými službami Azure** pro správu plnohodnotné prostředí – pomocí rozhraní Batch Management .NET [Azure Active Directory][aad_about]a hello [Azure Správce prostředků] [ resman_overview] společně v hello stejná aplikace. Pomocí těchto funkcí a jejich rozhraní API můžete poskytnout prostředí hladký ověřování, hello možnost toocreate a odstranění skupiny prostředků a hello možností, které jsou popsané výše začátku do konce řešení pro správu.

> [!NOTE]
> Když tento článek se zaměřuje na hello programové správy účtů Batch, klíče a kvóty, je mnoho tyto aktivity provést pomocí hello [portál Azure][azure_portal]. Další informace najdete v tématu [vytvoření účtu Azure Batch pomocí portálu Azure hello](batch-account-create-portal.md) a [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md).
> 
> 

## <a name="create-and-delete-batch-accounts"></a>Vytvářet a odstraňovat účty Batch
Jak je uvedeno, jeden z primární funkce hello hello Batch Management API je toocreate a odstraňte účty Batch v oblasti Azure. Ano, použít toodo [BatchManagementClient.Account.CreateAsync] [ net_create] a [DeleteAsync][net_delete], nebo jejich synchronní protějšky.

Hello následující fragment kódu vytvoří účet, získá hello nově vytvořený účet z hello služby Batch a odstraní ji. V této fragmentu kódu a ostatní hello v tomto článku `batchManagementClient` je plně inicializovaném instanci [BatchManagementClient][net_mgmt_client].

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
> Vyžadují aplikace, které používají knihovnu hello rozhraní Batch Management .NET a její třída BatchManagementClient **Správce služeb** nebo **spolusprávce** přístup toohello předplatné, které vlastní hello Spravované toobe účtu batch. Další informace najdete v tématu hello [Azure Active Directory](#azure-active-directory) části a hello [AccountManagement] [ acct_mgmt_sample] ukázka kódu.
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a>Získat a obnovit klíče účtu
Získat účet primární a sekundární klíče z jakékoli účtu Batch v rámci vašeho předplatného pomocí [ListKeysAsync][net_list_keys]. Tyto klíče můžete vygenerovat pomocí [RegenerateKeyAsync][net_regenerate_keys].

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
> Zjednodušená připojení pracovního postupu můžete vytvořit pro správu aplikací. Nejdřív získat klíčem účtu pro účet Batch hello chcete toomanage s [ListKeysAsync][net_list_keys]. Poté použijte tento klíč při inicializaci knihovny Batch .NET hello [BatchSharedKeyCredentials] [ net_sharedkeycred] třídy, která se používá při inicializaci [BatchClient] [net_batch_client].
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Zkontrolujte předplatné a kvóty účtu Batch
Předplatná Azure a hello jednotlivých služeb Azure jako všechny služby Batch mají výchozí kvóty, které omezují počet hello určitých entit v nich. Hello výchozí kvóty pro předplatná Azure, najdete v části [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md). Výchozí kvóty hello Dobrý den služby Batch, najdete v části [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md). Pomocí knihovny Batch Management .NET hello můžete zkontrolovat těchto kvót ve svých aplikacích. To vám umožní rozhodnutí o přidělení toomake předtím, než přidáte účty nebo výpočetní prostředky, jako jsou fondy a výpočetní uzly.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Zkontrolujte předplatné Azure pro kvóty účtu Batch
Před vytvořením účtu Batch v oblasti, můžete zkontrolovat toosee vaše předplatné Azure, ať už jste možnost tooadd účtu v daném regionu.

V následující fragment kódu hello, nejprve používáme [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] tooget kolekce všechny účty Batch, které jsou v rámci předplatného. Po získání jsme tuto kolekci jsme určit, kolik účty jsou v hello cílová oblast. Využijeme [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] tooobtain hello kvóta účtu Batch a určit, kolik účty (pokud existuje) můžete vytvořit v této oblasti.

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

Ve výše, hello fragmentu `creds` je instance [TokenCloudCredentials][azure_tokencreds]. toosee příklad vytvořením tohoto objektu, najdete v části hello [AccountManagement] [ acct_mgmt_sample] ukázka kódu na Githubu.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Zkontrolujte účet Batch u výpočetních prostředků kvóty
Před zvýšením výpočetní prostředky ve vašem řešení Batch, můžete zkontrolovat tooensure hello prostředky můžete určit, že tooallocate nesmí překročit kvót účtu hello. V následující fragment kódu hello, jsme tisknout informace o kvótě hello pro hello s názvem účtu Batch `mybatchaccount`. Ve vaší vlastní aplikaci můžete použít takové informace toodetermine zda hello účtu může zpracovat toobe hello další prostředky vytvořit.

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
> Existuje výchozí kvóty pro předplatná Azure a služby, řada těchto mezních hodnot mohou být vyvolány po vydání požadavku v hello [portál Azure][azure_portal]. Například v tématu [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md) pokyny pro zvýšení vaší kvóty účtu Batch.
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a>Používat Azure AD s Batch Management .NET

Knihovna rozhraní Batch Management .NET Hello je klient poskytovatel prostředků Azure a je použít v kombinaci s [Azure Resource Manager] [ resman_overview] toomanage účet prostředků prostřednictvím kódu programu. Azure AD je požadovaná tooauthenticate požadavky prostřednictvím všechny klienty poskytovatele prostředků Azure, včetně hello knihovny Batch Management .NET a prostřednictvím [Azure Resource Manager][resman_overview]. Informace o používání služby Azure AD pomocí knihovny Batch Management .NET hello najdete v tématu [řešení Batch pomocí Azure Active Directory tooauthenticate](batch-aad-auth.md). 

## <a name="sample-project-on-github"></a>Ukázkový projekt na Githubu

toosee rozhraní Batch Management .NET v akci, podívejte se na hello [AccountManagment] [ acct_mgmt_sample] ukázkového projektu na Githubu. Hello AccountManagment ukázkovou aplikaci ukazuje hello následující operace:

1. Získat token zabezpečení ze služby Azure AD pomocí [ADAL][aad_adal]. Pokud již není přihlášený uživatel hello, bude vyzván k zadání přihlašovacích údajů Azure.
2. Pomocí tokenu zabezpečení hello získaného z Azure AD, vytvářet [SubscriptionClient] [ resman_subclient] tooquery Azure seznam předplatná spojená s účtem hello. Hello uživatele můžete vybrat odběr, ze seznamu hello, pokud obsahuje více než jedno předplatné.
3. Získání přihlašovacích údajů, které jsou přidružené k předplatnému hello vybrané.
4. Vytvoření [ResourceManagementClient] [ resman_client] objekt pomocí přihlašovacích údajů hello.
5. Použití [ResourceManagementClient] [ resman_client] objektu toocreate skupinu prostředků.
6. Použití [BatchManagementClient] [ net_mgmt_client] objektu tooperform několik operací účtu Batch:
   * Vytvořte účet Batch v hello novou skupinu prostředků.
   * Získáte hello nově vytvořený účet z hello služby Batch.
   * Tisk hello klíče účtu pro nový účet hello.
   * Znovu vygenerujte nový primární klíč pro účet hello.
   * Tisk – informace o kvótě hello hello účtu.
   * Tisk – informace o kvótě hello hello předplatného.
   * Tisk – všechny účty v rámci předplatného hello.
   * Nově vytvořený účet odstraňte.
7. Odstraňte skupinu prostředků hello.

Před odstraněním skupiny účtů a prostředků Batch hello nově vytvořený, zobrazí se jim v hello [portál Azure][azure_portal]:

Ukázková aplikace hello toorun úspěšně, nejprve je nutné zaregistrovat ho s klientovi Azure AD v hello Azure portal a udělit oprávnění toohello rozhraní API Správce prostředků Azure. Postupujte podle kroků hello součástí [řešení pro správu Batch ověření pomocí služby Active Directory](batch-aad-auth-management.md).


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
