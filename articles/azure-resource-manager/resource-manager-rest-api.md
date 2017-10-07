---
title: aaaResource Manager REST API | Microsoft Docs
description: "Přehled hello ověřování rozhraní REST API Resource Manager a příklady použití"
services: azure-resource-manager
documentationcenter: na
author: navalev
manager: timlt
editor: 
ms.assetid: e8d7a1d2-1e82-4212-8288-8697341408c5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: navale;tomfitz;
ms.openlocfilehash: 3ccc3575c5e06c41f2fdc5317711980fc6a2f649
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resource-manager-rest-apis"></a>Rozhraní REST API pro Správce prostředků
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure Portal](resource-group-portal.md) 
> * [REST API](resource-manager-rest-api.md)
> 
> 

Za každé volání tooAzure Resource Manager za každých nasazené šablony za každý účet úložiště nejsou jeden nebo více volání toohello Azure Resource Manager na rozhraní RESTful API. Toto téma je věnovaná toothose rozhraní API a jak je můžete volat bez použití vůbec žádné sady SDK. Tento přístup je užitečné, pokud chcete plnou kontrolu nad tooAzure požadavky, nebo pokud hello SDK pro preferovaný jazyk není k dispozici nebo nepodporuje hello operace, které potřebujete.

Tento článek neprochází každé rozhraní API, které je vystaven v Azure, ale spíš používá jako příklady, jak připojit toothem některé operace. Jakmile porozumíte hello základy, si můžete přečíst hello [referenci rozhraní API REST Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) toofind podrobné informace o tom, jak toouse hello zbytku hello rozhraní API.

## <a name="authentication"></a>Authentication
Ověřování pro službu správce prostředků se zpracovává pomocí Azure Active Directory (AD). tooconnect tooany rozhraní API, musíte nejdřív tooauthenticate s Azure AD tooreceive ověřovací token, který můžete předat na tooevery požadavku. Jak jsme jsou popisující čistý volání přímo toohello rozhraní REST API, budeme předpokládat, že nechcete, aby tooauthenticate vyzváni k zadání uživatelského jména a hesla. Také předpokládáme, že nepoužíváte dvou mechanismů Multi-Factor authentication. Proto vytvoříme, co se nazývá aplikaci Azure AD a instanční objekt, které jsou používané toolog v. Mějte na paměti, že Azure AD podporují několik postupů ověřování a všechny z nich, ale může být použité tooretrieve Tento ověřovací token, které potřebujeme pro následné žádosti rozhraní API.
Postupujte podle [aplikace vytvořit Azure AD a instanční objekt](resource-group-create-service-principal-portal.md) pokyny krok za krokem.

### <a name="generating-an-access-token"></a>Generování Token přístupu
Volání se nachází v login.microsoftonline.com tooAzure AD, je potřeba ověřování služby Azure AD. tooauthenticate, je třeba toohave hello následující informace:

* ID klienta Azure AD (hello název používáte toolog v služby Azure AD, často hello stejné jako vaší společnosti, ale není vyžadováno)
* ID aplikace (prováděné během kroku vytváření aplikace hello Azure AD)
* Heslo (který jste vybrali při vytváření hello aplikace Azure AD)

V hello následující požadavek HTTP Ujistěte se, že tooreplace "Azure AD ID klienta", "ID aplikace" a "Password" s hello správné hodnoty.

**Obecná žádost HTTP:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... (Pokud je ověřování úspěšné) bude mít za následek odpovědi podobné toohello následující odpověď:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(hello access_token v hello předcházející odpověď byla zkrácená tooincrease čitelnost)

**Generování přístupového tokenu pomocí Bash:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Generování přístupového tokenu pomocí prostředí PowerShell:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Hello odpovědi obsahuje přístupový token, informace o tom, jak dlouho tento token je platný a informace o jaké prostředků můžete použít tento token.
Hello přístupový token, který jste obdrželi v hello předchozí volání protokolu HTTP musí být předán v pro všechny žádosti o toohello rozhraní API Správce prostředků. Předejte ji jako hodnotu záhlaví s názvem "Autorizace" s hodnotou hello "Nosiče YOUR_ACCESS_TOKEN". Všimněte si hello mezery mezi "Nosiče" a přístupový token.

Jak je vidět z hello výše HTTP výsledek hello token je platný pro určitou dobu dobu, během které by měly ukládat do mezipaměti a opakovaně používat tento stejný token. I když je možné tooauthenticate služby Azure AD u jednotlivých volání API, by bylo velmi neefektivní.

## <a name="calling-resource-manager-rest-apis"></a>Rozhraní API REST volání Resource Manager
Toto téma se používá jenom několik rozhraní API tooexplain hello základní použití operace REST hello. Informace o všech operacích hello najdete v tématu [rozhraní API REST Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).

### <a name="list-all-subscriptions"></a>Zobrazí seznam všech odběrů
Jedním z nejjednodušší operace hello, které můžete provést je hello toolist dostupných se předplatných, kterým můžete přistupovat. V hello následující požadavek uvidíte, jak se předávají hello přístupový token v jako hlavičku:

(Nahraďte YOUR_ACCESS_TOKEN vaše skutečné přístup k tokenu.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... a v důsledku toho získat seznam předplatných, že je tento objekt zabezpečení služby povolené tooaccess

(ID odběru byla zkrácena čitelnější)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Zobrazí seznam všech skupin prostředků v konkrétní předplatné
Všechny prostředky, které jsou k dispozici hello rozhraní API Resource Manager jsou vnořeny uvnitř skupiny prostředků. Resource Manager můžete dotazovat pro existující skupiny prostředků ve vašem předplatném pomocí hello následující požadavek HTTP GET. Všimněte si, jak hello ID předplatného se předávají v jako součást hello URL této doby.

(Nahraďte YOUR_ACCESS_TOKEN a ID_ODBĚRU ID skutečného přístupu token a předplatného)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Hello odpovědi získáte závisí na tom, jestli máte definované žádné skupiny prostředků a pokud ano, kolik.

(ID odběru byla zkrácena čitelnější)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Zatím jsme jste pouze byla dotazování hello rozhraní API Resource Manager informace. Je čas vytvoření některé prostředky a Začněme tím hello nejjednodušší ze všech, skupinu prostředků. Hello následující požadavek HTTP vytvoří skupinu prostředků v oblast nebo umístění podle vaší volby a přidá tooit značky.

(Nahraďte YOUR_ACCESS_TOKEN, ID_ODBĚRU, RESOURCE_GROUP_NAME se skutečné tokenu přístupu, ID předplatného a název skupiny prostředků, které chcete toocreate hello)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Pokud bylo úspěšné, můžete získat odpověď, který je podobný toohello následující odpověď:

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Úspěšně jste vytvořili skupinu prostředků v Azure. Blahopřejeme!

### <a name="deploy-resources-tooa-resource-group-using-a-resource-manager-template"></a>Nasazení skupiny prostředků tooa prostředky pomocí šablony Resource Manageru
S Resource Managerem, můžete nasadit své prostředky pomocí šablony. Šablona definuje několik prostředků a jejich závislosti. Pro tento oddíl předpokladu, že jste se seznámili s šablony Resource Manageru a jsme právě ukazují, jak volat rozhraní API hello toomake toostart nasazení. Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).

Nasazení šablony není liší množství toohow volání jiná rozhraní API. Jeden aspekt důležité je, že nasazení šablony může trvat poměrně dlouhou dobu. volání rozhraní API Hello právě vrátí, a je funkční tooyou jako tooquery vývojáře pro stav nasazení toofind hello se po dokončení nasazení hello. Další informace najdete v tématu [sledovat asynchronní operace Azure](resource-manager-async-operations.md).

V tomto příkladu používáme k dispozici na veřejně vystavené šablonu [Githubu](https://github.com/Azure/azure-quickstart-templates). Hello šablonu, kterou používáme nasadí oblast západní USA toohello virtuálního počítače s Linuxem. I když tento příklad používá šablonu k dispozici v veřejného úložiště jako GitHub, můžete místo toho předat kompletní šablonou hello jako součást požadavku hello. Všimněte si, že jsme zadáním hodnot parametru v hello žádosti, které se používají v rámci hello nasadit šablonu.

(Nahraďte ID_ODBĚRU, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD a DNS_NAME_FOR_PUBLIC_IP toovalues vhodné pro vaši žádost o)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Hello dlouho JSON odpovědi na tento požadavek byl čitelnější vynechání tooimprove této dokumentace. Hello odpovědi obsahuje informace o hello podle šablony nasazení, který jste vytvořili.

## <a name="next-steps"></a>Další kroky

- toolearn o zpracování asynchronní operace REST, najdete v části [sledovat asynchronní operace Azure](resource-manager-async-operations.md).
