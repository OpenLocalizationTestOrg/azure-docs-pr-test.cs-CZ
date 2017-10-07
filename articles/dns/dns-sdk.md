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
# <a name="create-dns-zones-and-record-sets-using-hello-net-sdk"></a>Vytvoření zóny DNS a sad záznamů pomocí hello .NET SDK

Operace toocreate, odstranění nebo aktualizaci zóny DNS, sady záznamů a záznamy můžete automatizovat pomocí sady SDK DNS Knihovna správy DNS .NET. Úplný projekt Visual Studio je k dispozici [sem.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Vytvořit hlavní účet služby

Obvykle je programový přístup k prostředkům tooAzure poskytnuto prostřednictvím vyhrazený účet, nikoli pověření uživatele. Tyto vyhrazené účty se označují jako účty instanční objekt. toouse hello Azure DNS SDK ukázkový projekt, je nejprve nutné toocreate hlavní účet služby a přiřaďte ho hello správná oprávnění.

1. Postupujte podle [tyto pokyny](../azure-resource-manager/resource-group-authenticate-service-principal.md) toocreate hlavního účtu služby (hello Azure DNS SDK ukázkový projekt předpokládá ověřování založené na heslech.)
2. Vytvoření skupiny prostředků ([tady je způsob](../azure-resource-manager/resource-group-template-deploy-portal.md)).
3. Použití Azure RBAC toogrant hello hlavní účet, Přispěvatel zóny DNS, oprávnění toohello skupiny prostředků služby ([tady je způsob](../active-directory/role-based-access-control-configure.md).)
4. Pokud používáte hello Azure DNS SDK ukázkový projekt, upravte soubor "program.cs" hello následujícím způsobem:

   * Vložte hello správné hodnoty pro hello tenantId, clientId (také označované jako ID účtu), tajný klíč (heslo hlavní účet služby) a ID předplatného jako použít v kroku 1.
   * Zadejte název skupiny prostředků hello vybrané v kroku 2.
   * Zadejte název zóny DNS podle svého výběru.

## <a name="nuget-packages-and-namespace-declarations"></a>Balíčky NuGet a deklarace oboru názvů

toouse hello Azure DNS .NET SDK, je nutné tooinstall hello **Knihovna správy Azure DNS** balíček NuGet a další požadované balíčky Azure.

1. V **Visual Studio**, otevřete projekt nebo nový projekt.
2. Přejděte příliš**nástroje**  **>**  **Správce balíčků NuGet**  **>**  **spravovat balíčky NuGet pro Řešení...** .
3. Klikněte na tlačítko **Procházet**, povolit hello **zahrnout předběžné verze** zaškrtávací políčko a typ **Microsoft.Azure.Management.Dns** hello vyhledávacího pole.
4. Vyberte balíček hello a klikněte na tlačítko **nainstalovat** tooadd ho tooyour projektu sady Visual Studio.
5. Opakujte postup hello výše hello tooalso instalace následujících balíčků: **Microsoft.Rest.ClientRuntime.Azure.Authentication** a **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Přidání deklarací oboru názvů

Přidejte následující deklarace oborů názvů hello

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-hello-dns-management-client"></a>Inicializace klienta pro správu DNS hello

Hello *DnsManagementClient* obsahuje hello metody a vlastnosti, které jsou nezbytné pro správu zón DNS a sady záznamů.  Hello následující kód přihlásí toohello hlavní účet služby a vytvoří objekt DnsManagementClient.

```cs
// Build hello service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a>Vytvořit nebo aktualizovat zónu DNS

toocreate zónu DNS, nejdřív "Zóna" je vytvořen objekt toocontain hello DNS zóny parametry. Protože zóny DNS nejsou propojené tooa určité oblasti, hello umístění se nastaví too'global'. V tomto příkladu [Azure Resource Manager 'značka'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) je taky přidaný toohello zóny.

tooactually vytvoření nebo aktualizace hello zóny v Azure DNS, hello zóny objekt obsahující parametry zóny hello je předán toohello *DnsManagementClient.Zones.CreateOrUpdateAsyc* metoda.

> [!NOTE]
> DnsManagementClient podporuje tři režimy činnosti: synchronní ('CreateOrUpdate'), asynchronní ('CreateOrUpdateAsync'), nebo asynchronní přístup toohello HTTP odpovědi (CreateOrUpdateWithHttpMessagesAsync).  Můžete použít některý z těchto režimů, v závislosti na potřebách vaší aplikace.

Azure DNS podporuje optimistickou metodu souběžného, nazývá [značky etag binárním rozsáhlým](dns-getstarted-create-dnszone.md). V tomto příkladu zadání "*" pro záhlaví hello 'If-None-Match' řídí toocreate Azure DNS zóny DNS Pokud ještě neexistuje.  Hello volání selže, pokud zónu se hello zadaným názvem již existuje v hello danou skupinu prostředků.

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

## <a name="create-dns-record-sets-and-records"></a>Vytvoření sady záznamů DNS a záznamů

Záznamy DNS jsou spravovány jako sadu záznamů. Sada záznamů je sada záznamů s hello stejný název a zaznamenejte type v rámci zóny.  Název sady záznamů Hello je relativní toohello název zóny, není hello plně kvalifikovaný název DNS.

toocreate nebo aktualizace sada záznamů, objekt parametry "Sady záznamů" se vytvoří a předán příliš*DnsManagementClient.RecordSets.CreateOrUpdateAsync*. Jako s zóny DNS, existují tři režimy operace: synchronní ('CreateOrUpdate'), asynchronní ('CreateOrUpdateAsync'), nebo asynchronní přístup toohello HTTP odpovědi (CreateOrUpdateWithHttpMessagesAsync).

Stejně jako u zóny DNS operací na sady záznamů zahrnují podporu pro optimistickou metodu souběžného.  V tomto příkladu protože nejsou zadány 'If-Match' ani 'If-None-Match', hello sady záznamů je vytvořen vždy.  Toto volání přepíše všechny existující sady záznamů s hello stejný název a zaznamenejte typ v této zóně DNS.

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

## <a name="get-zones-and-record-sets"></a>Získat zóny a sady záznamů

Hello *DnsManagementClient.Zones.Get* a *DnsManagementClient.RecordSets.Get* metody načtení jednotlivých zóny a sady záznamů v uvedeném pořadí. Sady záznamů jsou identifikovány jejich typu, názvu a hello zóny nebo skupině prostředků, které existují v. Zóny jsou identifikovány jejich název a hello skupinu prostředků, které existují v.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a>Aktualizovat existující sady záznamů

tooupdate existujícího záznamu DNS nastaven, nejdřív načíst sadu záznamů hello, pak aktualizace hello záznam nastavit obsah a pak odeslat změnu hello.  V tomto příkladu určíme hello načíst sady záznamů v parametru "If-Match" hello "Etag' z hello. Hello volání selže, pokud souběžnou operací změnil hello sady záznamů v hello té doby.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record toohello local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update hello record set in Azure DNS
// Note: ETAG check specified, update will be rejected if hello record set has changed in hello meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a>Seznam zón a sady záznamů

toolist zón, použijte hello *DnsManagementClient.Zones.List...*  metody, které podporují výpis buď zón v dané skupiny prostředků nebo zón v sad záznamů toolist konkrétního předplatného Azure (v rámci prostředků skupiny.), použijte *DnsManagementClient.RecordSets.List...*  metody, které podporují výpis všech sad záznamů v dané zóně nebo pouze tyto sady záznamů určitého typu.

Poznamenejte si při výpisu zóny a může čísla stránek vložena sady záznamů, které výsledků.  Následující příklad ukazuje, jak Hello tooiterate prostřednictvím hello stránkách s výsledky. (Velikostí stránky uměle malý (2) je použité tooforce stránkování, v praxi tento parametr by měl být vynechán a hello používá výchozí velikost stránky.)

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

## <a name="next-steps"></a>Další kroky

Stáhnout hello [.NET SDK služby Azure DNS ukázkového projektu](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), který obsahuje další příklady jak toouse hello DNS .NET SDK služby Azure, včetně příkladů pro jiné typy záznamů DNS.
