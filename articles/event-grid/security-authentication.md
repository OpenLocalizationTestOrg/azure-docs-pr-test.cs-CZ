---
title: "aaaAzure mřížky událostí zabezpečení a ověřování"
description: "Popisuje mřížky událostí Azure a jeho koncepty."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/14/2017
ms.author: babanisa
ms.openlocfilehash: 74f692c7e3a30856c3a80cbbfe82a26bf4c1c9b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-security-and-authentication"></a>Události zabezpečení mřížky a ověřování 

Azure mřížky událostí má tři typy ověřování:

* Odběry událostí
* Publikování události
* Doručení události Webhooku

## <a name="webhook-event-delivery"></a>Události Webhooku doručení

Webhooky jsou jedním z mnoha způsoby tooreceive události v reálném čase z Azure událostí mřížky.

Pokaždé, když je nový připravené toobe událostí doručit, odešle událost mřížky požadavek HTTP s tooyour Webhooku s hello událostí v textu hello.

Při registraci svůj vlastní koncový bod Webhooku s událostí mřížky, se vám pošle požadavek POST s kódem jednoduché ověření v pořadí tooprove koncový bod vlastnictví. Vaše aplikace musí toorespond podle zobrazování back hello ověřovacího kódu. Událost mřížky nebude poskytovat události tooWebHook koncových bodů, které nebyly hello ověření proběhlo úspěšně.
 
### <a name="validation-details"></a>Podrobnosti o ověření:

* V době hello aktualizace vytvoření odběru událostí odešle událost mřížky "SubscriptionValidationEvent" události toohello cílového koncového bodu.
* Hello události obsahuje hodnotu hlavičky "Ověření: typ události".
* textu Hello události má hello stejného schématu jako ostatní události událostí mřížky.
* data události Hello například zahrnuje vlastnost "ValidationCode" s náhodně generované řetězec "ValidationCode: acb13...".

V modulu snap-in vlastnictví koncový bod tooprove pořadí odezvu back hello ověření kódu např "ValidationResponse: acb13...".

Nakonec je důležité toonote mřížky událostí Azure podporuje pouze HTTPS webhooku koncové body.
## <a name="event-subscription"></a>Odběru událostí

toosubscribe tooan událostí, musíte mít hello **Microsoft.EventGrid/EventSubscriptions/Write** požadováno oprávnění na hello prostředků. Je nutné toto oprávnění, protože píšete nové předplatné v oboru hello hello prostředku. Hello požadované liší v závislosti na tom, jestli se odběru tooa systému téma nebo vlastních prostředků. Oba typy jsou popsané v této části.

### <a name="system-topics-azure-service-publishers"></a>Témata týkající se systému (vydavateli služby Azure)

Pro systém témata budete potřebovat oprávnění toowrite nové předplatné událostí v oboru hello hello prostředků publikování hello události. Formát Hello hello prostředku je:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Například toosubscribe tooan událostí na účet úložiště s názvem **UCET**, potřebujete oprávnění Microsoft.EventGrid/EventSubscriptions/Write hello na:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Vlastní témata

Pro vlastní témata budete potřebovat oprávnění toowrite nové předplatné událostí v oboru hello hello mřížky události tématu. Formát Hello hello prostředku je:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Například toosubscribe tooa vlastní téma s názvem **mytopic**, potřebujete oprávnění Microsoft.EventGrid/EventSubscriptions/Write hello na:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Téma publikování

Témata týkající se použití sdíleného přístupového podpisu (SAS) nebo ověření pomocí klíče. Doporučujeme, abyste SAS, ale ověření pomocí klíče poskytuje jednoduché programování a je kompatibilní s mnoha existující webhooku vydavatelů. 

Můžete zahrnout hello ověřování hodnota v hlavičce protokolu HTTP hello. SAS, použijte **tokenu sas Æg** pro hodnotu hlavičky hello. Pro ověření pomocí klíče, použít **klíč sas Æg** pro hodnotu hlavičky hello.

### <a name="key-authentication"></a>Ověření pomocí klíče

Ověření pomocí klíče je hello nejjednodušší formu ověření. Použijte formát hello:`aeg-sas-key: <your key>`

Například předáte klíč se: 

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>Tokeny SAS

Tokeny SAS pro událost mřížky zahrnují hello prostředků, čas vypršení platnosti a podpis. Hello formát tokenu SAS hello je: `r={resource}&e={expiration}&s={signature}`.

Hello prostředků je hello cesta pro téma toowhich hello odesílání událostí. Například je cesta platná prostředku:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Vygenerování hello podpisu z klíče.

Například platná **tokenu sas Æg** hodnota je:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
``` 

Hello následující příklad vytvoří SAS token pro použití s mřížky událostí:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    string encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString());

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Správa řízení přístupu

Azure mřížky událostí umožňuje vám toocontrol hello úroveň přístupu zadané toodifferent uživatelé toodo různé operace správy, jako je například seznam událostí odběry, vytvořit nové a generovat klíče. Událost mřížky využívá Azure na základě Role přístupu zkontrolujte (RBAC) pro tento.

### <a name="operation-types"></a>Typy operací

Azure událostí mřížky podporuje hello následující akce:

* Microsoft.EventGrid/*/read 
* Microsoft.EventGrid/*/write 
* Microsoft.EventGrid/*/delete 
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action 
* Microsoft.EventGrid/topics/listKeys/action 
* Microsoft.EventGrid/topics/regenerateKey/action

Hello poslední tři operace návratový potenciálně tajné informace, které získá filtrované mimo normální operace čtení. Je vhodné pro vás toorestrict přístup toothese operace. Můžete vytvořit vlastní role pomocí [prostředí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [rozhraní příkazového řádku Azure (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md)a hello [REST API](../active-directory/role-based-access-control-manage-access-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Vynucování Role na základě kontroly přístupu (RBAC)

Použijte následující postup tooenforce RBAC pro různé uživatele hello:

#### <a name="create-a-custom-role-definition-file-json"></a>Vytvořte soubor definice vlastních rolí (.json)

Hello následují definice rolí událostí mřížky ukázkové, které uživatelům umožňují tooperform různé sady akcí.

**EventGridReadOnlyRole.json**: Povolit jenom pro čtení jenom operace.
```json
{ 
  "Name": "Event grid read only role", 
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856", 
  "IsCustom": true, 
  "Description": "Event grid read only role", 
  "Actions": [ 
    "Microsoft.EventGrid/*/read" 
  ], 
  "NotActions": [ 
  ], 
  "AssignableScopes": [ 
    "/subscriptions/<Subscription Id>" 
  ] 
}
```

**EventGridNoDeleteListKeysRole.json**: Povolit akce s omezeným přístupem po ale zakáže akce odstranění.
```json
{ 
  "Name": "Event grid No Delete Listkeys role", 
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174", 
  "IsCustom": true, 
  "Description": "Event grid No Delete Listkeys role", 
  "Actions": [     
    "Microsoft.EventGrid/*/write", 
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action" 
    "Microsoft.EventGrid/topics/listkeys/action", 
    "Microsoft.EventGrid/topics/regenerateKey/action" 
  ], 
  "NotActions": [ 
    "Microsoft.EventGrid/*/delete" 
  ], 
  "AssignableScopes": [ 
    "/subscriptions/<Subscription id>" 
  ] 
}
```
 
**EventGridContributorRole.json**: umožňuje všechny akce mřížky událostí.  
```json
{ 
  "Name": "Event grid contributor role", 
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514", 
  "IsCustom": true, 
  "Description": "Event grid contributor role", 
  "Actions": [ 
    "Microsoft.EventGrid/*/write", 
    "Microsoft.EventGrid/*/delete", 
    "Microsoft.EventGrid/topics/listkeys/action", 
    "Microsoft.EventGrid/topics/regenerateKey/action", 
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action" 
  ], 
  "NotActions": [], 
  "AssignableScopes": [ 
    "/subscriptions/d48566a8-2428-4a6c-8347-9675d09fb851" 
  ] 
} 
```

#### <a name="install-and-login-tooazure-cli"></a>Instalace a přihlášení tooAzure rozhraní příkazového řádku

* Azure CLI verze 0.8.8 nebo novější. tooinstall hello nejnovější verzi a přidružit ho ke svému předplatnému Azure, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).
* Azure Resource Manager v rozhraní příkazového řádku Azure. Přejděte příliš[hello pomocí rozhraní příkazového řádku Azure s hello Resource Manager](../xplat-cli-azure-resource-manager.md) další podrobnosti.

#### <a name="create-a-custom-role"></a>Vytvořit vlastní roli

toocreate vlastní roli, použijte:

    azure role create --inputfile <file path>

#### <a name="assign-hello-role-tooa-user"></a>Přiřadit uživatele tooa role hello


    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>


## <a name="next-steps"></a>Další kroky

* TooEvent Úvod mřížky, najdete v části [o mřížky událostí](overview.md)
