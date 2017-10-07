---
title: "zóny DNS aaaCreate a sady záznamů v Azure DNS pomocí hello .NET SDK | Microsoft Docs"
description: "Jak zóny DNS toocreate sad záznamů a záznamů v Azure DNS pomocí hello .NET SDK."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: eed99b87-f4d4-4fbf-a926-263f7e30b884
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2016
ms.author: jonatul
ms.openlocfilehash: e3bab98b13b787427219acb7ec55e53490512fe7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-dns-zones-and-record-sets-using-hello-net-sdk"></a><span data-ttu-id="54960-103">Vytvoření zóny DNS a sad záznamů pomocí hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="54960-103">Create DNS zones and record sets using hello .NET SDK</span></span>

<span data-ttu-id="54960-104">Operace toocreate, odstranění nebo aktualizaci zóny DNS, sady záznamů a záznamy můžete automatizovat pomocí sady SDK DNS Knihovna správy DNS .NET.</span><span class="sxs-lookup"><span data-stu-id="54960-104">You can automate operations toocreate, delete, or update DNS zones, record sets, and records by using DNS SDK with .NET DNS Management library.</span></span> <span data-ttu-id="54960-105">Úplný projekt Visual Studio je k dispozici [sem.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)</span><span class="sxs-lookup"><span data-stu-id="54960-105">A full Visual Studio project is available [here.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)</span></span>

## <a name="create-a-service-principal-account"></a><span data-ttu-id="54960-106">Vytvořit hlavní účet služby</span><span class="sxs-lookup"><span data-stu-id="54960-106">Create a service principal account</span></span>

<span data-ttu-id="54960-107">Obvykle je programový přístup k prostředkům tooAzure poskytnuto prostřednictvím vyhrazený účet, nikoli pověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="54960-107">Typically, programmatic access tooAzure resources is granted via a dedicated account rather than your own user credentials.</span></span> <span data-ttu-id="54960-108">Tyto vyhrazené účty se označují jako účty instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="54960-108">These dedicated accounts are called 'service principal' accounts.</span></span> <span data-ttu-id="54960-109">toouse hello Azure DNS SDK ukázkový projekt, je nejprve nutné toocreate hlavní účet služby a přiřaďte ho hello správná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="54960-109">toouse hello Azure DNS SDK sample project, you first need toocreate a service principal account and assign it hello correct permissions.</span></span>

1. <span data-ttu-id="54960-110">Postupujte podle [tyto pokyny](../azure-resource-manager/resource-group-authenticate-service-principal.md) toocreate hlavního účtu služby (hello Azure DNS SDK ukázkový projekt předpokládá ověřování založené na heslech.)</span><span class="sxs-lookup"><span data-stu-id="54960-110">Follow [these instructions](../azure-resource-manager/resource-group-authenticate-service-principal.md) toocreate a service principal account (hello Azure DNS SDK sample project assumes password-based authentication.)</span></span>
2. <span data-ttu-id="54960-111">Vytvoření skupiny prostředků ([tady je způsob](../azure-resource-manager/resource-group-template-deploy-portal.md)).</span><span class="sxs-lookup"><span data-stu-id="54960-111">Create a resource group ([here's how](../azure-resource-manager/resource-group-template-deploy-portal.md)).</span></span>
3. <span data-ttu-id="54960-112">Použití Azure RBAC toogrant hello hlavní účet, Přispěvatel zóny DNS, oprávnění toohello skupiny prostředků služby ([tady je způsob](../active-directory/role-based-access-control-configure.md).)</span><span class="sxs-lookup"><span data-stu-id="54960-112">Use Azure RBAC toogrant hello service principal account 'DNS Zone Contributor' permissions toohello resource group ([here's how](../active-directory/role-based-access-control-configure.md).)</span></span>
4. <span data-ttu-id="54960-113">Pokud používáte hello Azure DNS SDK ukázkový projekt, upravte soubor "program.cs" hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="54960-113">If using hello Azure DNS SDK sample project, edit hello 'program.cs' file as follows:</span></span>

   * <span data-ttu-id="54960-114">Vložte hello správné hodnoty pro hello tenantId, clientId (také označované jako ID účtu), tajný klíč (heslo hlavní účet služby) a ID předplatného jako použít v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="54960-114">Insert hello correct values for hello tenantId, clientId (also known as account ID), secret (service principal account password) and subscriptionId as used in step 1.</span></span>
   * <span data-ttu-id="54960-115">Zadejte název skupiny prostředků hello vybrané v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="54960-115">Enter hello resource group name chosen in step 2.</span></span>
   * <span data-ttu-id="54960-116">Zadejte název zóny DNS podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="54960-116">Enter a DNS zone name of your choice.</span></span>

## <a name="nuget-packages-and-namespace-declarations"></a><span data-ttu-id="54960-117">Balíčky NuGet a deklarace oboru názvů</span><span class="sxs-lookup"><span data-stu-id="54960-117">NuGet packages and namespace declarations</span></span>

<span data-ttu-id="54960-118">toouse hello Azure DNS .NET SDK, je nutné tooinstall hello **Knihovna správy Azure DNS** balíček NuGet a další požadované balíčky Azure.</span><span class="sxs-lookup"><span data-stu-id="54960-118">toouse hello Azure DNS .NET SDK, you need tooinstall hello **Azure DNS Management Library** NuGet package and other required Azure packages.</span></span>

1. <span data-ttu-id="54960-119">V **Visual Studio**, otevřete projekt nebo nový projekt.</span><span class="sxs-lookup"><span data-stu-id="54960-119">In **Visual Studio**, open a project or new project.</span></span>
2. <span data-ttu-id="54960-120">Přejděte příliš**nástroje**  **>**  **Správce balíčků NuGet**  **>**  **spravovat balíčky NuGet pro Řešení...** .</span><span class="sxs-lookup"><span data-stu-id="54960-120">Go too**Tools** **>** **NuGet Package Manager** **>** **Manage NuGet Packages for Solution...**.</span></span>
3. <span data-ttu-id="54960-121">Klikněte na tlačítko **Procházet**, povolit hello **zahrnout předběžné verze** zaškrtávací políčko a typ **Microsoft.Azure.Management.Dns** hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="54960-121">Click **Browse**, enable hello **Include prerelease** checkbox, and type **Microsoft.Azure.Management.Dns** in hello search box.</span></span>
4. <span data-ttu-id="54960-122">Vyberte balíček hello a klikněte na tlačítko **nainstalovat** tooadd ho tooyour projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54960-122">Select hello package and click **Install** tooadd it tooyour Visual Studio project.</span></span>
5. <span data-ttu-id="54960-123">Opakujte postup hello výše hello tooalso instalace následujících balíčků: **Microsoft.Rest.ClientRuntime.Azure.Authentication** a **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="54960-123">Repeat hello process above tooalso install hello following packages: **Microsoft.Rest.ClientRuntime.Azure.Authentication** and **Microsoft.Azure.Management.ResourceManager**.</span></span>

## <a name="add-namespace-declarations"></a><span data-ttu-id="54960-124">Přidání deklarací oboru názvů</span><span class="sxs-lookup"><span data-stu-id="54960-124">Add namespace declarations</span></span>

<span data-ttu-id="54960-125">Přidejte následující deklarace oborů názvů hello</span><span class="sxs-lookup"><span data-stu-id="54960-125">Add hello following namespace declarations</span></span>

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-hello-dns-management-client"></a><span data-ttu-id="54960-126">Inicializace klienta pro správu DNS hello</span><span class="sxs-lookup"><span data-stu-id="54960-126">Initialize hello DNS management client</span></span>

<span data-ttu-id="54960-127">Hello *DnsManagementClient* obsahuje hello metody a vlastnosti, které jsou nezbytné pro správu zón DNS a sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="54960-127">hello *DnsManagementClient* contains hello methods and properties necessary for managing DNS zones and recordsets.</span></span>  <span data-ttu-id="54960-128">Hello následující kód přihlásí toohello hlavní účet služby a vytvoří objekt DnsManagementClient.</span><span class="sxs-lookup"><span data-stu-id="54960-128">hello following code logs in toohello service principal account and creates a DnsManagementClient object.</span></span>

```cs
// Build hello service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a><span data-ttu-id="54960-129">Vytvořit nebo aktualizovat zónu DNS</span><span class="sxs-lookup"><span data-stu-id="54960-129">Create or update a DNS zone</span></span>

<span data-ttu-id="54960-130">toocreate zónu DNS, nejdřív "Zóna" je vytvořen objekt toocontain hello DNS zóny parametry.</span><span class="sxs-lookup"><span data-stu-id="54960-130">toocreate a DNS zone, first a "Zone" object is created toocontain hello DNS zone parameters.</span></span> <span data-ttu-id="54960-131">Protože zóny DNS nejsou propojené tooa určité oblasti, hello umístění se nastaví too'global'.</span><span class="sxs-lookup"><span data-stu-id="54960-131">Because DNS zones are not linked tooa specific region, hello location is set too'global'.</span></span> <span data-ttu-id="54960-132">V tomto příkladu [Azure Resource Manager 'značka'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) je taky přidaný toohello zóny.</span><span class="sxs-lookup"><span data-stu-id="54960-132">In this example, an [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) is also added toohello zone.</span></span>

<span data-ttu-id="54960-133">tooactually vytvoření nebo aktualizace hello zóny v Azure DNS, hello zóny objekt obsahující parametry zóny hello je předán toohello *DnsManagementClient.Zones.CreateOrUpdateAsyc* metoda.</span><span class="sxs-lookup"><span data-stu-id="54960-133">tooactually create or update hello zone in Azure DNS, hello zone object containing hello zone parameters is passed toohello *DnsManagementClient.Zones.CreateOrUpdateAsyc* method.</span></span>

> [!NOTE]
> <span data-ttu-id="54960-134">DnsManagementClient podporuje tři režimy činnosti: synchronní ('CreateOrUpdate'), asynchronní ('CreateOrUpdateAsync'), nebo asynchronní přístup toohello HTTP odpovědi (CreateOrUpdateWithHttpMessagesAsync).</span><span class="sxs-lookup"><span data-stu-id="54960-134">DnsManagementClient supports three modes of operation: synchronous ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), or asynchronous with access toohello HTTP response ('CreateOrUpdateWithHttpMessagesAsync').</span></span>  <span data-ttu-id="54960-135">Můžete použít některý z těchto režimů, v závislosti na potřebách vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="54960-135">You can choose any of these modes, depending on your application needs.</span></span>

<span data-ttu-id="54960-136">Azure DNS podporuje optimistickou metodu souběžného, nazývá [značky etag binárním rozsáhlým](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="54960-136">Azure DNS supports optimistic concurrency, called [Etags](dns-getstarted-create-dnszone.md).</span></span> <span data-ttu-id="54960-137">V tomto příkladu zadání "*" pro záhlaví hello 'If-None-Match' řídí toocreate Azure DNS zóny DNS Pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="54960-137">In this example, specifying "*" for hello 'If-None-Match' header tells Azure DNS toocreate a DNS zone if one does not already exist.</span></span>  <span data-ttu-id="54960-138">Hello volání selže, pokud zónu se hello zadaným názvem již existuje v hello danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="54960-138">hello call fails if a zone with hello given name already exists in hello given resource group.</span></span>

```cs
// Create zone parameters
var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"

// Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
dnsZoneParams.Tags = new Dictionary<string, string>();
dnsZoneParams.Tags.Add("dept", "finance");

// Create hello actual zone.
// Note: Uses 'If-None-Match *' ETAG check, so will fail if hello zone exists already.
// Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
// Note: For getting hello http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");
```

## <a name="create-dns-record-sets-and-records"></a><span data-ttu-id="54960-139">Vytvoření sady záznamů DNS a záznamů</span><span class="sxs-lookup"><span data-stu-id="54960-139">Create DNS record sets and records</span></span>

<span data-ttu-id="54960-140">Záznamy DNS jsou spravovány jako sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="54960-140">DNS records are managed as a record set.</span></span> <span data-ttu-id="54960-141">Sada záznamů je sada záznamů s hello stejný název a zaznamenejte type v rámci zóny.</span><span class="sxs-lookup"><span data-stu-id="54960-141">A record set is a set of records with hello same name and record type within a zone.</span></span>  <span data-ttu-id="54960-142">Název sady záznamů Hello je relativní toohello název zóny, není hello plně kvalifikovaný název DNS.</span><span class="sxs-lookup"><span data-stu-id="54960-142">hello record set name is relative toohello zone name, not hello fully qualified DNS name.</span></span>

<span data-ttu-id="54960-143">toocreate nebo aktualizace sada záznamů, objekt parametry "Sady záznamů" se vytvoří a předán příliš*DnsManagementClient.RecordSets.CreateOrUpdateAsync*.</span><span class="sxs-lookup"><span data-stu-id="54960-143">toocreate or update a record set, a "RecordSet" parameters object is created and passed too*DnsManagementClient.RecordSets.CreateOrUpdateAsync*.</span></span> <span data-ttu-id="54960-144">Jako s zóny DNS, existují tři režimy operace: synchronní ('CreateOrUpdate'), asynchronní ('CreateOrUpdateAsync'), nebo asynchronní přístup toohello HTTP odpovědi (CreateOrUpdateWithHttpMessagesAsync).</span><span class="sxs-lookup"><span data-stu-id="54960-144">As with DNS zones, there are three modes of operation: synchronous ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), or asynchronous with access toohello HTTP response ('CreateOrUpdateWithHttpMessagesAsync').</span></span>

<span data-ttu-id="54960-145">Stejně jako u zóny DNS operací na sady záznamů zahrnují podporu pro optimistickou metodu souběžného.</span><span class="sxs-lookup"><span data-stu-id="54960-145">As with DNS zones, operations on record sets include support for optimistic concurrency.</span></span>  <span data-ttu-id="54960-146">V tomto příkladu protože nejsou zadány 'If-Match' ani 'If-None-Match', hello sady záznamů je vytvořen vždy.</span><span class="sxs-lookup"><span data-stu-id="54960-146">In this example, since neither 'If-Match' nor 'If-None-Match' are specified, hello record set is always created.</span></span>  <span data-ttu-id="54960-147">Toto volání přepíše všechny existující sady záznamů s hello stejný název a zaznamenejte typ v této zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="54960-147">This call overwrites any existing record set with hello same name and record type in this DNS zone.</span></span>

```cs
// Create record set parameters
var recordSetParams = new RecordSet();
recordSetParams.TTL = 3600;

// Add records toohello record set parameter object.  In this case, we'll add a record of type 'A'
recordSetParams.ARecords = new List<ARecord>();
recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

// Add metadata toohello record set.  Similar tooAzure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
recordSetParams.Metadata = new Dictionary<string, string>();
recordSetParams.Metadata.Add("user", "Mary");

// Create hello actual record set in Azure DNS
// Note: no ETAG checks specified, will overwrite existing record set if one exists
var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);
```

## <a name="get-zones-and-record-sets"></a><span data-ttu-id="54960-148">Získat zóny a sady záznamů</span><span class="sxs-lookup"><span data-stu-id="54960-148">Get zones and record sets</span></span>

<span data-ttu-id="54960-149">Hello *DnsManagementClient.Zones.Get* a *DnsManagementClient.RecordSets.Get* metody načtení jednotlivých zóny a sady záznamů v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="54960-149">hello *DnsManagementClient.Zones.Get* and *DnsManagementClient.RecordSets.Get* methods retrieve individual zones and record sets, respectively.</span></span> <span data-ttu-id="54960-150">Sady záznamů jsou identifikovány jejich typu, názvu a hello zóny nebo skupině prostředků, které existují v.</span><span class="sxs-lookup"><span data-stu-id="54960-150">RecordSets are identified by their type, name, and hello zone and resource group they exist in.</span></span> <span data-ttu-id="54960-151">Zóny jsou identifikovány jejich název a hello skupinu prostředků, které existují v.</span><span class="sxs-lookup"><span data-stu-id="54960-151">Zones are identified by their name and hello resource group they exist in.</span></span>

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a><span data-ttu-id="54960-152">Aktualizovat existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="54960-152">Update an existing record set</span></span>

<span data-ttu-id="54960-153">tooupdate existujícího záznamu DNS nastaven, nejdřív načíst sadu záznamů hello, pak aktualizace hello záznam nastavit obsah a pak odeslat změnu hello.</span><span class="sxs-lookup"><span data-stu-id="54960-153">tooupdate an existing DNS record set, first retrieve hello record set, then update hello record set contents, then submit hello change.</span></span>  <span data-ttu-id="54960-154">V tomto příkladu určíme hello načíst sady záznamů v parametru "If-Match" hello "Etag' z hello.</span><span class="sxs-lookup"><span data-stu-id="54960-154">In this example, we specify hello 'Etag' from hello retrieved record set in hello 'If-Match' parameter.</span></span> <span data-ttu-id="54960-155">Hello volání selže, pokud souběžnou operací změnil hello sady záznamů v hello té doby.</span><span class="sxs-lookup"><span data-stu-id="54960-155">hello call fails if a concurrent operation has modified hello record set in hello meantime.</span></span>

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record toohello local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update hello record set in Azure DNS
// Note: ETAG check specified, update will be rejected if hello record set has changed in hello meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a><span data-ttu-id="54960-156">Seznam zón a sady záznamů</span><span class="sxs-lookup"><span data-stu-id="54960-156">List zones and record sets</span></span>

<span data-ttu-id="54960-157">toolist zón, použijte hello *DnsManagementClient.Zones.List...*  metody, které podporují výpis buď zón v dané skupiny prostředků nebo zón v sad záznamů toolist konkrétního předplatného Azure (v rámci prostředků skupiny.), použijte *DnsManagementClient.RecordSets.List...*  metody, které podporují výpis všech sad záznamů v dané zóně nebo pouze tyto sady záznamů určitého typu.</span><span class="sxs-lookup"><span data-stu-id="54960-157">toolist zones, use hello *DnsManagementClient.Zones.List...* methods, which support listing either all zones in a given resource group or all zones in a given Azure subscription (across resource groups.) toolist record sets, use *DnsManagementClient.RecordSets.List...* methods, which support either listing all record sets in a given zone or only those record sets of a specific type.</span></span>

<span data-ttu-id="54960-158">Poznamenejte si při výpisu zóny a může čísla stránek vložena sady záznamů, které výsledků.</span><span class="sxs-lookup"><span data-stu-id="54960-158">Note when listing zones and record sets that results may be paginated.</span></span>  <span data-ttu-id="54960-159">Následující příklad ukazuje, jak Hello tooiterate prostřednictvím hello stránkách s výsledky.</span><span class="sxs-lookup"><span data-stu-id="54960-159">hello following example shows how tooiterate through hello pages of results.</span></span> <span data-ttu-id="54960-160">(Velikostí stránky uměle malý (2) je použité tooforce stránkování, v praxi tento parametr by měl být vynechán a hello používá výchozí velikost stránky.)</span><span class="sxs-lookup"><span data-stu-id="54960-160">(An artificially small page size of '2' is used tooforce paging; in practice this parameter should be omitted and hello default page size used.)</span></span>

```cs
// Note: in this demo, we'll use a very small page size (2 record sets) toodemonstrate paging
// In practice, tooimprove performance you would use a large page size or just use hello system default
int recordSets = 0;
var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
recordSets += page.Count();

while (page.NextPageLink != null)
{
    page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
    recordSets += page.Count();
}
```

## <a name="next-steps"></a><span data-ttu-id="54960-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54960-161">Next steps</span></span>

<span data-ttu-id="54960-162">Stáhnout hello [.NET SDK služby Azure DNS ukázkového projektu](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), který obsahuje další příklady jak toouse hello DNS .NET SDK služby Azure, včetně příkladů pro jiné typy záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="54960-162">Download hello [Azure DNS .NET SDK sample project](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), which includes further examples of how toouse hello Azure DNS .NET SDK, including examples for other DNS record types.</span></span>
