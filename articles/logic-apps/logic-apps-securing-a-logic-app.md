---
title: "aaaSecure přístup tooAzure Logic Apps | Microsoft Docs"
description: "Přidejte zabezpečení pro ochranu přístupu tootriggers, vstupy a výstupy, parametrů akcí a služeb používaných u pracovních postupů v Azure Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/22/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: abda2179e4cc2d2295cd8332ec017c848a456264
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-access-tooyour-logic-apps"></a>Zabezpečení přístupu k tooyour logic apps

Existuje mnoho toohelp dostupné nástroje, které se zabezpečit svou aplikaci logiky.

* Zabezpečení přístupu tootrigger aplikace logiky (HTTP žádosti o aktivační události)
* Zabezpečení toomanage přístup, upravit nebo číst aplikace logiky
* Zabezpečení přístupu toocontents vstupy a výstupy pro spuštění
* Zabezpečení parametry nebo vstupy v rámci akce v pracovním postupu
* Zabezpečení přístupu tooservices, který přijímání požadavků z pracovního postupu

## <a name="secure-access-tootrigger"></a>Tootrigger zabezpečený přístup

Pokud pracujete s aplikace logiky, která se spustí na požadavek HTTP ([požadavku](../connectors/connectors-native-reqres.md) nebo [Webhooku](../connectors/connectors-native-webhook.md)), můžete omezit přístup tak, aby pouze autorizovaní klienti můžete aktivovat aplikaci logiky hello. Všechny požadavky do aplikace logiky jsou zašifrovány a zabezpečené pomocí protokolu SSL.

### <a name="shared-access-signature"></a>Sdílený přístupový podpis

Obsahuje všechny koncové body požadavek pro aplikaci logiky [sdíleného přístupového podpisu (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) jako součást hello URL. Obsahuje každou adresu URL `sp`, `sv`, a `sig` parametr dotazu. Oprávnění jsou určené `sp`, a odpovídat tooHTTP metody povolena, `sv` je verze použitá toogenerate hello, a `sig` je použité tooauthenticate tootrigger přístup. podpis Hello je generována pomocí algoritmu hello SHA256 s tajným klíčem na všechny vlastnosti a cesty adresy URL hello. tajný klíč Hello nikdy zveřejněné a publikovali kde je udržována šifrovaná a uložené jako součást hello logiku aplikace. Aplikace logiky pouze autorizuje aktivační události, které obsahují platný podpis vytvořené pomocí hello tajný klíč.

#### <a name="regenerate-access-keys"></a>Opětovné vygenerování přístupových klíčů

Můžete vygenerovat nový zabezpečený klíč v kdykoli prostřednictvím portálu hello REST API nebo Azure. Všechny aktuální adresy URL, které byly vygenerovány dříve pomocí starého klíče hello jsou zneplatněné a už autorizovaným toofire hello logiku aplikace.

1. V hello portálu Azure otevřete aplikaci logiky hello chcete tooregenerate klíč
1. Klikněte na tlačítko hello **přístupové klíče** položky nabídky v části **nastavení**
1. Zvolte hello klíče tooregenerate a proces dokončení hello

Adresy URL, jež načíst po opětovné generování jsou podepsané hello nový přístupový klíč.

#### <a name="creating-callback-urls-with-an-expiration-date"></a>Vytvoření adresy URL zpětné volání s datum vypršení platnosti

Pokud adresa URL hello sdílíte s ostatními stranami, konkrétní klíče a data vypršení platnosti podle potřeby můžete vygenerovat adresy URL. Potom můžete bezproblémově vrátit klíče, nebo zkontrolujte toofire přístup k aplikaci je omezená tooa určité časové rozpětí. Můžete zadat datum vypršení platnosti pro adresu URL prostřednictvím hello [aplikace logiky REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

V textu hello zahrnout hello vlastnost `NotAfter` jako řetězec data JSON, který vrátí adresu URL zpětné volání, která je platný pouze dokud hello `NotAfter` datum a čas.

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a>Vytvoření adresy URL pomocí primární nebo sekundární tajný klíč

Při generování nebo seznam adres URL zpětného volání pro aktivační procedury založené na požadavku, můžete také zadat adresu URL, která klíče toouse toosign hello.  Můžete generovat adresu URL podepsaný konkrétního klíče prostřednictvím hello [aplikace logiky REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) následujícím způsobem:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

V textu hello zahrnout hello vlastnost `KeyType` jako `Primary` nebo `Secondary`.  Vrátí adresu URL podepsaný hello zabezpečeného zadaný klíč.

### <a name="restrict-incoming-ip-addresses"></a>Omezit příchozí IP adresy

Toohello sdílený přístupový podpis, kromě toho můžete toorestrict volání aplikace logiky pouze z konkrétní klientů.  Například pokud budete spravovat váš koncový bod pomocí Azure API Management, můžete omezit hello logiku aplikace tooonly přijímat žádosti o hello, pokud hello požadavek pochází z hello IP adresa instance API Management.

Toto nastavení lze konfigurovat v nastavení aplikace logiky hello:

1. V hello portálu Azure otevřete aplikaci logiky hello chcete omezení podle IP adresy tooadd
1. Klikněte na tlačítko hello **konfigurace řízení přístupu** položky nabídky v části **nastavení**
1. Zadejte seznam hello přijímat aktivační událost hello toobe pro rozsahy IP adres

Platný rozsah IP trvá hello formátu `192.168.1.1/255`. Pokud chcete hello logiku aplikace ještě efektivněji tooonly jako aplikace vnořené logiky, vyberte hello **jenom jiné aplikace logiky** možnost. Tato možnost zapíše prostředek toohello prázdné pole, což znamená, že jen volání z hello vlastní služby ještě efektivněji (nadřazené logiku aplikace) úspěšně.

> [!NOTE]
> Pořád se dá spustit aplikaci logiky s aktivační událost požadavku prostřednictvím hello REST API nebo správu `/triggers/{triggerName}/run` bez ohledu na IP. Tento scénář vyžaduje ověřování proti hello REST API služby Azure a všechny události se zobrazí v hello protokol auditování Azure. Zásady řízení přístupu sadu odpovídajícím způsobem.

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a>Nastavení rozsahy IP adres v definici prostředků hello

Pokud používáte [šablonu nasazení](logic-apps-create-deploy-template.md) tooautomate vaše nasazení hello nastavení rozsahu IP adres se dá konfigurovat na hello prostředků šablony.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "triggers": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}

```

### <a name="adding-azure-active-directory-oauth-or-other-security"></a>Přidání služby Azure Active Directory, OAuth nebo jiných zabezpečení

Další autorizace protokoly nad aplikace logiky, tooadd [Azure API Management](https://azure.microsoft.com/services/api-management/) nabízí bohaté monitorování, zabezpečení, zásady a dokumentace pro libovolný koncový bod s hello schopností tooexpose aplikace logiky jako rozhraní API. Azure API Management můžou zpřístupnit koncový bod veřejné nebo soukromé hello logiku aplikace, která by mohla používat Azure Active Directory, certifikát, OAuth nebo jiné standardy zabezpečení. Po přijetí žádost o službě Azure API Management předává hello požadavek toohello aplikace logiky (provádění všechny potřebné transformace ani neukládají omezení). Můžete použít hello příchozí rozsah IP adres, nastavení na hello logiku aplikace tooonly Povolit aktivaci hello logiku aplikace toobe ze správy rozhraní API.

## <a name="secure-access-toomanage-or-edit-logic-apps"></a>Zabezpečení přístupu k toomanage nebo úpravy aplikace logiky

Operace toomanagement přístup na aplikace logiky můžete omezit tak, aby pouze konkrétní uživatele nebo skupiny jsou možné tooperform operace u prostředku hello. Aplikace logiky použít hello Azure [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) funkci a je možné přizpůsobit prostřednictvím hello stejných nástrojů.  Existuje několik integrovaných rolí, které jsou členy vaše předplatné tooas dobře:

* **Přispěvatel aplikace logiky** – poskytuje přístup tooview, edit a aktualizace aplikace logiky.  Nelze odebrat hello prostředků nebo provádět operace správy.
* **Operátor aplikace logiky** – můžete zobrazit aplikace logiky hello a historie spouštění a povolit nebo zakázat.  Nelze upravit nebo aktualizovat definice hello.

Můžete také použít [Zámek prostředků Azure](../azure-resource-manager/resource-group-lock-resources.md) tooprevent měnit nebo odstraňovat aplikace logiky. Tato možnost je výrobní zdroje cenné tooprevent z změn nebo odstranění.

## <a name="secure-access-toocontents-of-hello-run-history"></a>Zabezpečený přístup toocontents Dobrý den, historie spouštění

Můžete omezit přístup toocontents vstupy nebo výstupy z rozsahů IP adres předchozí spustí toospecific.  

Všechna data v rámci spuštění pracovního postupu se šifrují během přenosu i v klidu. Při volání toorun historie je služba hello ověří požadavek hello a poskytuje odkazy toohello požadavku a odpovědi vstupy a výstupy. Tento odkaz může být chráněný tak pouze požadavky tooview obsah z určené rozsah IP adres vrátit hello obsah. Této funkci můžete použít pro ovládací prvek další přístup. Můžete zadat IP adresu jako i `0.0.0.0` , nemá nikdo přístup vstupy/výstupy. Pouze uživatel s oprávněními správce může odebrat toto omezení poskytuje možnost hello obsah tooworkflow 'v běhu' přístup.

Toto nastavení lze nakonfigurovat v rámci nastavení prostředků hello hello portálu Azure:

1. V hello portálu Azure otevřete aplikaci logiky hello chcete omezení podle IP adresy tooadd
1. Klikněte na tlačítko hello **konfigurace řízení přístupu** položky nabídky v části **nastavení**
1. Zadejte seznam rozsahů IP adres pro přístup k toocontent na hello

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a>Nastavení rozsahy IP adres v definici prostředků hello

Pokud používáte [šablonu nasazení](logic-apps-create-deploy-template.md) tooautomate vaše nasazení hello nastavení rozsahu IP adres se dá konfigurovat na hello prostředků šablony.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "contents": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}
```

## <a name="secure-parameters-and-inputs-within-a-workflow"></a>Zabezpečení parametry a vstupy v pracovním postupu

Můžete chtít tooparameterize některých aspektů definice pracovního postupu pro nasazení v prostředí. Některé parametry mohou být také zabezpečené parametry, které nechcete, aby tooappear při úpravě pracovního postupu, jako je například ID klienta a tajný klíč klienta pro [ověřování Azure Active Directory](../connectors/connectors-native-http.md#authentication) akce HTTP.

### <a name="using-parameters-and-secure-parameters"></a>Použití parametrů a parametrů zabezpečení

tooaccess hello hodnoty parametru prostředků v době běhu hello [jazyk definic workflowů](http://aka.ms/logicappsdocs) poskytuje `@parameters()` operaci. Můžete také [zadejte parametry v šabloně nasazení prostředků hello](../azure-resource-manager/resource-group-authoring-templates.md#parameters). Ale pokud zadáte hello typem parametru jako `securestring`, parametr hello se zbytkem hello hello definice prostředků nejsou k dispozici a nebudou přístupné, a to zobrazením hello prostředků po nasazení.

> [!NOTE]
> Pokud vaše parametr se používá v záhlaví hello nebo obsahu požadavku, může být parametr hello viditelné hello spustit historie a odchozí požadavek HTTP. Ujistěte se, že tooset váš přístup k obsahu zásady odpovídajícím způsobem.
> Nikdy jsou viditelné prostřednictvím vstupů nebo výstupů autorizační hlavičky. Takže pokud tajný klíč hello používá existuje, tajný klíč hello není dá načíst.

#### <a name="resource-deployment-template-with-secrets"></a>Šablonu nasazení prostředků s tajné klíče

Hello následující příklad ukazuje, nasazení, který odkazuje na zabezpečené parametr `secret` za běhu. V souboru samostatné parametry, můžete zadat hodnotu hello prostředí pro hello `secret`, nebo použijte [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) tooretrieve tajných klíčů v nasazení čas.

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "secretDeploymentParam": {
      "type": "securestring"
    }
  },
  "variables": {},
  "resources": [
    {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
        "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "actions": {
            "Call_External_API": {
              "type": "http",
              "inputs": {
                "headers": {
                  "Authorization": "@parameters('secret')"
                },
                "body": "This is hello request"
              },
              "runAfter": {}
            }
          },
          "parameters": {
            "secret": {
              "type": "SecureString"
            }
          },
          "triggers": {
            "manual": {
              "type": "Request",
              "kind": "Http",
              "inputs": {
                "schema": {}
              }
            }
          },
          "contentVersion": "1.0.0.0",
          "outputs": {}
        },
        "parameters": {
          "secret": {
            "value": "[parameters('secretDeploymentParam')]"
          }
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="secure-access-tooservices-receiving-requests-from-a-workflow"></a>Zabezpečení přístupu k příjmu požadavků tooservices z pracovního postupu

Zabezpečené že aplikace logiky hello žádné koncový bod potřebuje tooaccess jsou toohelp mnoha způsoby.

### <a name="using-authentication-on-outbound-requests"></a>Pomocí ověřování pro odchozí požadavky

Při práci s HTTP, HTTP + Swagger (otevřené rozhraní API) nebo akce Webhooku, můžete přidat odesílány žádosti o ověření toohello. Můžete uvést základní ověřování, ověřování pomocí certifikátu nebo ověřování Azure Active Directory. Podrobnosti o tom, jak tooconfigure toto ověřování naleznete [v tomto článku](../connectors/connectors-native-http.md#authentication).

### <a name="restricting-access-toologic-app-ip-addresses"></a>Omezení přístupu toologic aplikace IP adresy

Všechna volání z aplikace logiky pocházet z konkrétní sadu IP adres podle oblastí. Můžete přidat další filtrování tooonly přijímání požadavků z ty, které určí IP adresy. Seznam těchto IP adres najdete v tématu [logiku aplikace omezení a konfigurace](logic-apps-limits-and-config.md#configuration).

### <a name="on-premises-connectivity"></a>Místní připojení

Aplikace logiky poskytují integrace s několika služeb tooprovide zabezpečený a spolehlivý místní komunikace.

#### <a name="on-premises-data-gateway"></a>Místní brána dat

Mnoho spravovaných konektorů pro logic apps poskytuje zabezpečené připojení tooon místních systémů, včetně systému souborů, SQL, SharePoint, DB2 a další. Brána Hello předává data z místního zdroje na šifrované kanály prostřednictvím hello Azure Service Bus. Veškerý provoz pochází jako zabezpečené odchozí provoz z agenta brány hello. Další informace o [fungování brány dat hello](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) možnosti místní připojení, včetně integrace site-to-site VPN a ExpressRoute pro zabezpečené systémy tooon místní proxy server a komunikace. V hello návrhář aplikace logiky můžete rychle vybrat rozhraní API zveřejněné z Azure API Management v rámci pracovního postupu, poskytující rychlý přístup tooon místních systémů.

#### <a name="hybrid-connections-from-azure-app-service"></a>Hybridní připojení z Azure App Service

Funkce hello místní hybridního připojení můžete použít pro rozhraní API služby Azure a webové aplikace toocommunicate místní.  Informace o hybridní připojení a o tom, jak lze nalézt tooconfigure [v tomto článku](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="next-steps"></a>Další kroky
[Vytvoření šablony nasazení](logic-apps-create-deploy-template.md)  
[Zpracování výjimek](logic-apps-exception-handling.md)  
[Monitorování aplikací logiky](logic-apps-monitor-your-logic-apps.md)  
[Diagnostikování selhání aplikace logiky a problémy](logic-apps-diagnosing-failures.md)  
