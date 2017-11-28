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
# <a name="secure-access-tooyour-logic-apps"></a><span data-ttu-id="c8f4a-103">Zabezpečení přístupu k tooyour logic apps</span><span class="sxs-lookup"><span data-stu-id="c8f4a-103">Secure access tooyour logic apps</span></span>

<span data-ttu-id="c8f4a-104">Existuje mnoho toohelp dostupné nástroje, které se zabezpečit svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-104">There are many tools available toohelp you secure your logic app.</span></span>

* <span data-ttu-id="c8f4a-105">Zabezpečení přístupu tootrigger aplikace logiky (HTTP žádosti o aktivační události)</span><span class="sxs-lookup"><span data-stu-id="c8f4a-105">Securing access tootrigger a logic app (HTTP Request Trigger)</span></span>
* <span data-ttu-id="c8f4a-106">Zabezpečení toomanage přístup, upravit nebo číst aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="c8f4a-106">Securing access toomanage, edit, or read a logic app</span></span>
* <span data-ttu-id="c8f4a-107">Zabezpečení přístupu toocontents vstupy a výstupy pro spuštění</span><span class="sxs-lookup"><span data-stu-id="c8f4a-107">Securing access toocontents of inputs and outputs for a run</span></span>
* <span data-ttu-id="c8f4a-108">Zabezpečení parametry nebo vstupy v rámci akce v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="c8f4a-108">Securing parameters or inputs within actions in a workflow</span></span>
* <span data-ttu-id="c8f4a-109">Zabezpečení přístupu tooservices, který přijímání požadavků z pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="c8f4a-109">Securing access tooservices that receive requests from a workflow</span></span>

## <a name="secure-access-tootrigger"></a><span data-ttu-id="c8f4a-110">Tootrigger zabezpečený přístup</span><span class="sxs-lookup"><span data-stu-id="c8f4a-110">Secure access tootrigger</span></span>

<span data-ttu-id="c8f4a-111">Pokud pracujete s aplikace logiky, která se spustí na požadavek HTTP ([požadavku](../connectors/connectors-native-reqres.md) nebo [Webhooku](../connectors/connectors-native-webhook.md)), můžete omezit přístup tak, aby pouze autorizovaní klienti můžete aktivovat aplikaci logiky hello.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-111">When you work with a logic app that fires on an HTTP Request ([Request](../connectors/connectors-native-reqres.md) or [Webhook](../connectors/connectors-native-webhook.md)), you can restrict access so that only authorized clients can fire hello logic app.</span></span> <span data-ttu-id="c8f4a-112">Všechny požadavky do aplikace logiky jsou zašifrovány a zabezpečené pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-112">All requests into a logic app are encrypted and secured via SSL.</span></span>

### <a name="shared-access-signature"></a><span data-ttu-id="c8f4a-113">Sdílený přístupový podpis</span><span class="sxs-lookup"><span data-stu-id="c8f4a-113">Shared Access Signature</span></span>

<span data-ttu-id="c8f4a-114">Obsahuje všechny koncové body požadavek pro aplikaci logiky [sdíleného přístupového podpisu (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) jako součást hello URL.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-114">Every request endpoint for a logic app includes a [Shared Access Signature (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) as part of hello URL.</span></span> <span data-ttu-id="c8f4a-115">Obsahuje každou adresu URL `sp`, `sv`, a `sig` parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-115">Each URL contains a `sp`, `sv`, and `sig` query parameter.</span></span> <span data-ttu-id="c8f4a-116">Oprávnění jsou určené `sp`, a odpovídat tooHTTP metody povolena, `sv` je verze použitá toogenerate hello, a `sig` je použité tooauthenticate tootrigger přístup.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-116">Permissions are specified by `sp`, and correspond tooHTTP methods allowed, `sv` is hello version used toogenerate, and `sig` is used tooauthenticate access tootrigger.</span></span> <span data-ttu-id="c8f4a-117">podpis Hello je generována pomocí algoritmu hello SHA256 s tajným klíčem na všechny vlastnosti a cesty adresy URL hello.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-117">hello signature is generated using hello SHA256 algorithm with a secret key on all hello URL paths and properties.</span></span> <span data-ttu-id="c8f4a-118">tajný klíč Hello nikdy zveřejněné a publikovali kde je udržována šifrovaná a uložené jako součást hello logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-118">hello secret key is never exposed and published, and is kept encrypted and stored as part of hello logic app.</span></span> <span data-ttu-id="c8f4a-119">Aplikace logiky pouze autorizuje aktivační události, které obsahují platný podpis vytvořené pomocí hello tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-119">Your logic app only authorizes triggers that contain a valid signature created with hello secret key.</span></span>

#### <a name="regenerate-access-keys"></a><span data-ttu-id="c8f4a-120">Opětovné vygenerování přístupových klíčů</span><span class="sxs-lookup"><span data-stu-id="c8f4a-120">Regenerate access keys</span></span>

<span data-ttu-id="c8f4a-121">Můžete vygenerovat nový zabezpečený klíč v kdykoli prostřednictvím portálu hello REST API nebo Azure.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-121">You can regenerate a new secure key at anytime through hello REST API or Azure portal.</span></span> <span data-ttu-id="c8f4a-122">Všechny aktuální adresy URL, které byly vygenerovány dříve pomocí starého klíče hello jsou zneplatněné a už autorizovaným toofire hello logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-122">All current URLs that were generated previously using hello old key are invalidated and no longer authorized toofire hello logic app.</span></span>

1. <span data-ttu-id="c8f4a-123">V hello portálu Azure otevřete aplikaci logiky hello chcete tooregenerate klíč</span><span class="sxs-lookup"><span data-stu-id="c8f4a-123">In hello Azure portal, open hello logic app you want tooregenerate a key</span></span>
1. <span data-ttu-id="c8f4a-124">Klikněte na tlačítko hello **přístupové klíče** položky nabídky v části **nastavení**</span><span class="sxs-lookup"><span data-stu-id="c8f4a-124">Click hello **Access Keys** menu item under **Settings**</span></span>
1. <span data-ttu-id="c8f4a-125">Zvolte hello klíče tooregenerate a proces dokončení hello</span><span class="sxs-lookup"><span data-stu-id="c8f4a-125">Choose hello key tooregenerate and complete hello process</span></span>

<span data-ttu-id="c8f4a-126">Adresy URL, jež načíst po opětovné generování jsou podepsané hello nový přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-126">URLs you retrieve after regeneration are signed with hello new access key.</span></span>

#### <a name="creating-callback-urls-with-an-expiration-date"></a><span data-ttu-id="c8f4a-127">Vytvoření adresy URL zpětné volání s datum vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="c8f4a-127">Creating callback URLs with an expiration date</span></span>

<span data-ttu-id="c8f4a-128">Pokud adresa URL hello sdílíte s ostatními stranami, konkrétní klíče a data vypršení platnosti podle potřeby můžete vygenerovat adresy URL.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-128">If you are sharing hello URL with other parties, you can generate URLs with specific keys and expiration dates as needed.</span></span> <span data-ttu-id="c8f4a-129">Potom můžete bezproblémově vrátit klíče, nebo zkontrolujte toofire přístup k aplikaci je omezená tooa určité časové rozpětí.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-129">You can then seamlessly roll keys, or ensure access toofire an app is restricted tooa certain timespan.</span></span> <span data-ttu-id="c8f4a-130">Můžete zadat datum vypršení platnosti pro adresu URL prostřednictvím hello [aplikace logiky REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):</span><span class="sxs-lookup"><span data-stu-id="c8f4a-130">You can specify an expiration date for a URL through hello [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):</span></span>

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

<span data-ttu-id="c8f4a-131">V textu hello zahrnout hello vlastnost `NotAfter` jako řetězec data JSON, který vrátí adresu URL zpětné volání, která je platný pouze dokud hello `NotAfter` datum a čas.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-131">In hello body, include hello property `NotAfter` as a JSON date string, which returns a callback URL that is only valid until hello `NotAfter` date and time.</span></span>

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a><span data-ttu-id="c8f4a-132">Vytvoření adresy URL pomocí primární nebo sekundární tajný klíč</span><span class="sxs-lookup"><span data-stu-id="c8f4a-132">Creating URLs with primary or secondary secret key</span></span>

<span data-ttu-id="c8f4a-133">Při generování nebo seznam adres URL zpětného volání pro aktivační procedury založené na požadavku, můžete také zadat adresu URL, která klíče toouse toosign hello.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-133">When you generate or list callback URLs for request-based triggers, you can also specify which key toouse toosign hello URL.</span></span>  <span data-ttu-id="c8f4a-134">Můžete generovat adresu URL podepsaný konkrétního klíče prostřednictvím hello [aplikace logiky REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c8f4a-134">You can generate a URL signed by a specific key through hello [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) as follows:</span></span>

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

<span data-ttu-id="c8f4a-135">V textu hello zahrnout hello vlastnost `KeyType` jako `Primary` nebo `Secondary`.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-135">In hello body, include hello property `KeyType` as either `Primary` or `Secondary`.</span></span>  <span data-ttu-id="c8f4a-136">Vrátí adresu URL podepsaný hello zabezpečeného zadaný klíč.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-136">This returns a URL signed by hello secure key specified.</span></span>

### <a name="restrict-incoming-ip-addresses"></a><span data-ttu-id="c8f4a-137">Omezit příchozí IP adresy</span><span class="sxs-lookup"><span data-stu-id="c8f4a-137">Restrict incoming IP addresses</span></span>

<span data-ttu-id="c8f4a-138">Toohello sdílený přístupový podpis, kromě toho můžete toorestrict volání aplikace logiky pouze z konkrétní klientů.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-138">In addition toohello Shared Access Signature, you may wish toorestrict calling a logic app only from specific clients.</span></span>  <span data-ttu-id="c8f4a-139">Například pokud budete spravovat váš koncový bod pomocí Azure API Management, můžete omezit hello logiku aplikace tooonly přijímat žádosti o hello, pokud hello požadavek pochází z hello IP adresa instance API Management.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-139">For example, if you manage your endpoint through Azure API Management, you can restrict hello logic app tooonly accept hello request when hello request comes from hello API Management instance IP address.</span></span>

<span data-ttu-id="c8f4a-140">Toto nastavení lze konfigurovat v nastavení aplikace logiky hello:</span><span class="sxs-lookup"><span data-stu-id="c8f4a-140">This setting can be configured within hello logic app settings:</span></span>

1. <span data-ttu-id="c8f4a-141">V hello portálu Azure otevřete aplikaci logiky hello chcete omezení podle IP adresy tooadd</span><span class="sxs-lookup"><span data-stu-id="c8f4a-141">In hello Azure portal, open hello logic app you want tooadd IP address restrictions</span></span>
1. <span data-ttu-id="c8f4a-142">Klikněte na tlačítko hello **konfigurace řízení přístupu** položky nabídky v části **nastavení**</span><span class="sxs-lookup"><span data-stu-id="c8f4a-142">Click hello **Access control configuration** menu item under **Settings**</span></span>
1. <span data-ttu-id="c8f4a-143">Zadejte seznam hello přijímat aktivační událost hello toobe pro rozsahy IP adres</span><span class="sxs-lookup"><span data-stu-id="c8f4a-143">Specify hello list of IP address ranges toobe accepted by hello trigger</span></span>

<span data-ttu-id="c8f4a-144">Platný rozsah IP trvá hello formátu `192.168.1.1/255`.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-144">A valid IP range takes hello format `192.168.1.1/255`.</span></span> <span data-ttu-id="c8f4a-145">Pokud chcete hello logiku aplikace ještě efektivněji tooonly jako aplikace vnořené logiky, vyberte hello **jenom jiné aplikace logiky** možnost.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-145">If you want hello logic app tooonly fire as a nested logic app, select hello **Only other logic apps** option.</span></span> <span data-ttu-id="c8f4a-146">Tato možnost zapíše prostředek toohello prázdné pole, což znamená, že jen volání z hello vlastní služby ještě efektivněji (nadřazené logiku aplikace) úspěšně.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-146">This option writes an empty array toohello resource, meaning only calls from hello service itself (parent logic apps) fire successfully.</span></span>

> [!NOTE]
> <span data-ttu-id="c8f4a-147">Pořád se dá spustit aplikaci logiky s aktivační událost požadavku prostřednictvím hello REST API nebo správu `/triggers/{triggerName}/run` bez ohledu na IP.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-147">You can still run a logic app with a request trigger through hello REST API / Management `/triggers/{triggerName}/run` regardless of IP.</span></span> <span data-ttu-id="c8f4a-148">Tento scénář vyžaduje ověřování proti hello REST API služby Azure a všechny události se zobrazí v hello protokol auditování Azure.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-148">This scenario requires authentication against hello Azure REST API, and all events would appear in hello Azure Audit Log.</span></span> <span data-ttu-id="c8f4a-149">Zásady řízení přístupu sadu odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-149">Set access control policies accordingly.</span></span>

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a><span data-ttu-id="c8f4a-150">Nastavení rozsahy IP adres v definici prostředků hello</span><span class="sxs-lookup"><span data-stu-id="c8f4a-150">Setting IP ranges on hello resource definition</span></span>

<span data-ttu-id="c8f4a-151">Pokud používáte [šablonu nasazení](logic-apps-create-deploy-template.md) tooautomate vaše nasazení hello nastavení rozsahu IP adres se dá konfigurovat na hello prostředků šablony.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-151">If you are using a [deployment template](logic-apps-create-deploy-template.md) tooautomate your deployments, hello IP range settings can be configured on hello resource template.</span></span>  

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

### <a name="adding-azure-active-directory-oauth-or-other-security"></a><span data-ttu-id="c8f4a-152">Přidání služby Azure Active Directory, OAuth nebo jiných zabezpečení</span><span class="sxs-lookup"><span data-stu-id="c8f4a-152">Adding Azure Active Directory, OAuth, or other security</span></span>

<span data-ttu-id="c8f4a-153">Další autorizace protokoly nad aplikace logiky, tooadd [Azure API Management](https://azure.microsoft.com/services/api-management/) nabízí bohaté monitorování, zabezpečení, zásady a dokumentace pro libovolný koncový bod s hello schopností tooexpose aplikace logiky jako rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-153">tooadd more authorization protocols on top of a logic app, [Azure API Management](https://azure.microsoft.com/services/api-management/) offers rich monitoring, security, policy, and documentation for any endpoint with hello capability tooexpose a logic app as an API.</span></span> <span data-ttu-id="c8f4a-154">Azure API Management můžou zpřístupnit koncový bod veřejné nebo soukromé hello logiku aplikace, která by mohla používat Azure Active Directory, certifikát, OAuth nebo jiné standardy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-154">Azure API Management can expose a public or private endpoint for hello logic app, which could use Azure Active Directory, certificate, OAuth, or other security standards.</span></span> <span data-ttu-id="c8f4a-155">Po přijetí žádost o službě Azure API Management předává hello požadavek toohello aplikace logiky (provádění všechny potřebné transformace ani neukládají omezení).</span><span class="sxs-lookup"><span data-stu-id="c8f4a-155">When a request is received, Azure API Management forwards hello request toohello logic app (performing any needed transformations or restrictions in-flight).</span></span> <span data-ttu-id="c8f4a-156">Můžete použít hello příchozí rozsah IP adres, nastavení na hello logiku aplikace tooonly Povolit aktivaci hello logiku aplikace toobe ze správy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-156">You can use hello incoming IP range settings on hello logic app tooonly allow hello logic app toobe triggered from API Management.</span></span>

## <a name="secure-access-toomanage-or-edit-logic-apps"></a><span data-ttu-id="c8f4a-157">Zabezpečení přístupu k toomanage nebo úpravy aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="c8f4a-157">Secure access toomanage or edit logic apps</span></span>

<span data-ttu-id="c8f4a-158">Operace toomanagement přístup na aplikace logiky můžete omezit tak, aby pouze konkrétní uživatele nebo skupiny jsou možné tooperform operace u prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-158">You can restrict access toomanagement operations on a logic app so that only specific users or groups are able tooperform operations on hello resource.</span></span> <span data-ttu-id="c8f4a-159">Aplikace logiky použít hello Azure [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) funkci a je možné přizpůsobit prostřednictvím hello stejných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-159">Logic apps use hello Azure [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) feature, and can be customized with hello same tools.</span></span>  <span data-ttu-id="c8f4a-160">Existuje několik integrovaných rolí, které jsou členy vaše předplatné tooas dobře:</span><span class="sxs-lookup"><span data-stu-id="c8f4a-160">There are a few built-in roles you can assign members of your subscription tooas well:</span></span>

* <span data-ttu-id="c8f4a-161">**Přispěvatel aplikace logiky** – poskytuje přístup tooview, edit a aktualizace aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-161">**Logic App Contributor** - Provides access tooview, edit, and update a logic app.</span></span>  <span data-ttu-id="c8f4a-162">Nelze odebrat hello prostředků nebo provádět operace správy.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-162">Cannot remove hello resource or perform admin operations.</span></span>
* <span data-ttu-id="c8f4a-163">**Operátor aplikace logiky** – můžete zobrazit aplikace logiky hello a historie spouštění a povolit nebo zakázat.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-163">**Logic App Operator** - Can view hello logic app and run history, and enable/disable.</span></span>  <span data-ttu-id="c8f4a-164">Nelze upravit nebo aktualizovat definice hello.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-164">Cannot edit or update hello definition.</span></span>

<span data-ttu-id="c8f4a-165">Můžete také použít [Zámek prostředků Azure](../azure-resource-manager/resource-group-lock-resources.md) tooprevent měnit nebo odstraňovat aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-165">You can also use [Azure Resource Lock](../azure-resource-manager/resource-group-lock-resources.md) tooprevent changing or deleting logic apps.</span></span> <span data-ttu-id="c8f4a-166">Tato možnost je výrobní zdroje cenné tooprevent z změn nebo odstranění.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-166">This capability is valuable tooprevent production resources from changes or deletions.</span></span>

## <a name="secure-access-toocontents-of-hello-run-history"></a><span data-ttu-id="c8f4a-167">Zabezpečený přístup toocontents Dobrý den, historie spouštění</span><span class="sxs-lookup"><span data-stu-id="c8f4a-167">Secure access toocontents of hello run history</span></span>

<span data-ttu-id="c8f4a-168">Můžete omezit přístup toocontents vstupy nebo výstupy z rozsahů IP adres předchozí spustí toospecific.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-168">You can restrict access toocontents of inputs or outputs from previous runs toospecific IP address ranges.</span></span>  

<span data-ttu-id="c8f4a-169">Všechna data v rámci spuštění pracovního postupu se šifrují během přenosu i v klidu.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-169">All data within a workflow run is encrypted in transit and at rest.</span></span> <span data-ttu-id="c8f4a-170">Při volání toorun historie je služba hello ověří požadavek hello a poskytuje odkazy toohello požadavku a odpovědi vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-170">When a call toorun history is made, hello service authenticates hello request and provides links toohello request and response inputs and outputs.</span></span> <span data-ttu-id="c8f4a-171">Tento odkaz může být chráněný tak pouze požadavky tooview obsah z určené rozsah IP adres vrátit hello obsah.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-171">This link can be protected so only requests tooview content from a designated IP address range return hello contents.</span></span> <span data-ttu-id="c8f4a-172">Této funkci můžete použít pro ovládací prvek další přístup.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-172">You can use this capability for additional access control.</span></span> <span data-ttu-id="c8f4a-173">Můžete zadat IP adresu jako i `0.0.0.0` , nemá nikdo přístup vstupy/výstupy.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-173">You could even specify an IP address like `0.0.0.0` so no one could access inputs/outputs.</span></span> <span data-ttu-id="c8f4a-174">Pouze uživatel s oprávněními správce může odebrat toto omezení poskytuje možnost hello obsah tooworkflow 'v běhu' přístup.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-174">Only someone with admin permissions could remove this restriction, providing hello possibility for 'just-in-time' access tooworkflow contents.</span></span>

<span data-ttu-id="c8f4a-175">Toto nastavení lze nakonfigurovat v rámci nastavení prostředků hello hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="c8f4a-175">This setting can be configured within hello resource settings of hello Azure portal:</span></span>

1. <span data-ttu-id="c8f4a-176">V hello portálu Azure otevřete aplikaci logiky hello chcete omezení podle IP adresy tooadd</span><span class="sxs-lookup"><span data-stu-id="c8f4a-176">In hello Azure portal, open hello logic app you want tooadd IP address restrictions</span></span>
1. <span data-ttu-id="c8f4a-177">Klikněte na tlačítko hello **konfigurace řízení přístupu** položky nabídky v části **nastavení**</span><span class="sxs-lookup"><span data-stu-id="c8f4a-177">Click hello **Access control configuration** menu item under **Settings**</span></span>
1. <span data-ttu-id="c8f4a-178">Zadejte seznam rozsahů IP adres pro přístup k toocontent na hello</span><span class="sxs-lookup"><span data-stu-id="c8f4a-178">Specify hello list of IP address ranges for access toocontent</span></span>

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a><span data-ttu-id="c8f4a-179">Nastavení rozsahy IP adres v definici prostředků hello</span><span class="sxs-lookup"><span data-stu-id="c8f4a-179">Setting IP ranges on hello resource definition</span></span>

<span data-ttu-id="c8f4a-180">Pokud používáte [šablonu nasazení](logic-apps-create-deploy-template.md) tooautomate vaše nasazení hello nastavení rozsahu IP adres se dá konfigurovat na hello prostředků šablony.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-180">If you are using a [deployment template](logic-apps-create-deploy-template.md) tooautomate your deployments, hello IP range settings can be configured on hello resource template.</span></span>  

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

## <a name="secure-parameters-and-inputs-within-a-workflow"></a><span data-ttu-id="c8f4a-181">Zabezpečení parametry a vstupy v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="c8f4a-181">Secure parameters and inputs within a workflow</span></span>

<span data-ttu-id="c8f4a-182">Můžete chtít tooparameterize některých aspektů definice pracovního postupu pro nasazení v prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-182">You might want tooparameterize some aspects of a workflow definition for deployment across environments.</span></span> <span data-ttu-id="c8f4a-183">Některé parametry mohou být také zabezpečené parametry, které nechcete, aby tooappear při úpravě pracovního postupu, jako je například ID klienta a tajný klíč klienta pro [ověřování Azure Active Directory](../connectors/connectors-native-http.md#authentication) akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-183">Also, some parameters might be secure parameters you don't want tooappear when editing a workflow, such as a client ID and client secret for [Azure Active Directory authentication](../connectors/connectors-native-http.md#authentication) of an HTTP action.</span></span>

### <a name="using-parameters-and-secure-parameters"></a><span data-ttu-id="c8f4a-184">Použití parametrů a parametrů zabezpečení</span><span class="sxs-lookup"><span data-stu-id="c8f4a-184">Using parameters and secure parameters</span></span>

<span data-ttu-id="c8f4a-185">tooaccess hello hodnoty parametru prostředků v době běhu hello [jazyk definic workflowů](http://aka.ms/logicappsdocs) poskytuje `@parameters()` operaci.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-185">tooaccess hello value of a resource parameter at runtime, hello [workflow definition language](http://aka.ms/logicappsdocs) provides a `@parameters()` operation.</span></span> <span data-ttu-id="c8f4a-186">Můžete také [zadejte parametry v šabloně nasazení prostředků hello](../azure-resource-manager/resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="c8f4a-186">Also, you can [specify parameters in hello resource deployment template](../azure-resource-manager/resource-group-authoring-templates.md#parameters).</span></span> <span data-ttu-id="c8f4a-187">Ale pokud zadáte hello typem parametru jako `securestring`, parametr hello se zbytkem hello hello definice prostředků nejsou k dispozici a nebudou přístupné, a to zobrazením hello prostředků po nasazení.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-187">But if you specify hello parameter type as `securestring`, hello parameter won't be returned with hello rest of hello resource definition, and won't be accessible by viewing hello resource after deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="c8f4a-188">Pokud vaše parametr se používá v záhlaví hello nebo obsahu požadavku, může být parametr hello viditelné hello spustit historie a odchozí požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-188">If your parameter is used in hello headers or body of a request, hello parameter might be visible by accessing hello run history and outgoing HTTP request.</span></span> <span data-ttu-id="c8f4a-189">Ujistěte se, že tooset váš přístup k obsahu zásady odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-189">Make sure tooset your content access policies accordingly.</span></span>
> <span data-ttu-id="c8f4a-190">Nikdy jsou viditelné prostřednictvím vstupů nebo výstupů autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-190">Authorization headers are never visible through inputs or outputs.</span></span> <span data-ttu-id="c8f4a-191">Takže pokud tajný klíč hello používá existuje, tajný klíč hello není dá načíst.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-191">So if hello secret is being used there, hello secret is not retrievable.</span></span>

#### <a name="resource-deployment-template-with-secrets"></a><span data-ttu-id="c8f4a-192">Šablonu nasazení prostředků s tajné klíče</span><span class="sxs-lookup"><span data-stu-id="c8f4a-192">Resource deployment template with secrets</span></span>

<span data-ttu-id="c8f4a-193">Hello následující příklad ukazuje, nasazení, který odkazuje na zabezpečené parametr `secret` za běhu.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-193">hello following example shows a deployment that references a secure parameter of `secret` at runtime.</span></span> <span data-ttu-id="c8f4a-194">V souboru samostatné parametry, můžete zadat hodnotu hello prostředí pro hello `secret`, nebo použijte [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) tooretrieve tajných klíčů v nasazení čas.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-194">In a separate parameters file, you could specify hello environment value for hello `secret`, or use [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) tooretrieve secrets at deploy time.</span></span>

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

## <a name="secure-access-tooservices-receiving-requests-from-a-workflow"></a><span data-ttu-id="c8f4a-195">Zabezpečení přístupu k příjmu požadavků tooservices z pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="c8f4a-195">Secure access tooservices receiving requests from a workflow</span></span>

<span data-ttu-id="c8f4a-196">Zabezpečené že aplikace logiky hello žádné koncový bod potřebuje tooaccess jsou toohelp mnoha způsoby.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-196">There are many ways toohelp secure any endpoint hello logic app needs tooaccess.</span></span>

### <a name="using-authentication-on-outbound-requests"></a><span data-ttu-id="c8f4a-197">Pomocí ověřování pro odchozí požadavky</span><span class="sxs-lookup"><span data-stu-id="c8f4a-197">Using authentication on outbound requests</span></span>

<span data-ttu-id="c8f4a-198">Při práci s HTTP, HTTP + Swagger (otevřené rozhraní API) nebo akce Webhooku, můžete přidat odesílány žádosti o ověření toohello.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-198">When working with an HTTP, HTTP + Swagger (Open API), or Webhook action, you can add authentication toohello request being sent.</span></span> <span data-ttu-id="c8f4a-199">Můžete uvést základní ověřování, ověřování pomocí certifikátu nebo ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-199">You could include basic authentication, certificate authentication, or Azure Active Directory authentication.</span></span> <span data-ttu-id="c8f4a-200">Podrobnosti o tom, jak tooconfigure toto ověřování naleznete [v tomto článku](../connectors/connectors-native-http.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="c8f4a-200">Details on how tooconfigure this authentication can be found [in this article](../connectors/connectors-native-http.md#authentication).</span></span>

### <a name="restricting-access-toologic-app-ip-addresses"></a><span data-ttu-id="c8f4a-201">Omezení přístupu toologic aplikace IP adresy</span><span class="sxs-lookup"><span data-stu-id="c8f4a-201">Restricting access toologic app IP addresses</span></span>

<span data-ttu-id="c8f4a-202">Všechna volání z aplikace logiky pocházet z konkrétní sadu IP adres podle oblastí.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-202">All calls from logic apps come from a specific set of IP addresses per region.</span></span> <span data-ttu-id="c8f4a-203">Můžete přidat další filtrování tooonly přijímání požadavků z ty, které určí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-203">You can add additional filtering tooonly accept requests from those designated IP addresses.</span></span> <span data-ttu-id="c8f4a-204">Seznam těchto IP adres najdete v tématu [logiku aplikace omezení a konfigurace](logic-apps-limits-and-config.md#configuration).</span><span class="sxs-lookup"><span data-stu-id="c8f4a-204">For a list of those IP addresses, see [logic app limits and configuration](logic-apps-limits-and-config.md#configuration).</span></span>

### <a name="on-premises-connectivity"></a><span data-ttu-id="c8f4a-205">Místní připojení</span><span class="sxs-lookup"><span data-stu-id="c8f4a-205">On-premises connectivity</span></span>

<span data-ttu-id="c8f4a-206">Aplikace logiky poskytují integrace s několika služeb tooprovide zabezpečený a spolehlivý místní komunikace.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-206">Logic apps provide integration with several services tooprovide secure and reliable on-premises communication.</span></span>

#### <a name="on-premises-data-gateway"></a><span data-ttu-id="c8f4a-207">Místní brána dat</span><span class="sxs-lookup"><span data-stu-id="c8f4a-207">On-premises data gateway</span></span>

<span data-ttu-id="c8f4a-208">Mnoho spravovaných konektorů pro logic apps poskytuje zabezpečené připojení tooon místních systémů, včetně systému souborů, SQL, SharePoint, DB2 a další.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-208">Many managed connectors for logic apps provide secure connectivity tooon-premises systems, including File System, SQL, SharePoint, DB2, and more.</span></span> <span data-ttu-id="c8f4a-209">Brána Hello předává data z místního zdroje na šifrované kanály prostřednictvím hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-209">hello gateway relays data from on-premises sources on encrypted channels through hello Azure Service Bus.</span></span> <span data-ttu-id="c8f4a-210">Veškerý provoz pochází jako zabezpečené odchozí provoz z agenta brány hello.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-210">All traffic originates as secure outbound traffic from hello gateway agent.</span></span> <span data-ttu-id="c8f4a-211">Další informace o [fungování brány dat hello](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="c8f4a-211">Learn more about [how hello data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span>

#### <a name="azure-api-management"></a><span data-ttu-id="c8f4a-212">Azure API Management</span><span class="sxs-lookup"><span data-stu-id="c8f4a-212">Azure API Management</span></span>

<span data-ttu-id="c8f4a-213">[Azure API Management](https://azure.microsoft.com/services/api-management/) možnosti místní připojení, včetně integrace site-to-site VPN a ExpressRoute pro zabezpečené systémy tooon místní proxy server a komunikace.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-213">[Azure API Management](https://azure.microsoft.com/services/api-management/) has on-premises connectivity options, including site-to-site VPN and ExpressRoute integration for secured proxy and communication tooon-premises systems.</span></span> <span data-ttu-id="c8f4a-214">V hello návrhář aplikace logiky můžete rychle vybrat rozhraní API zveřejněné z Azure API Management v rámci pracovního postupu, poskytující rychlý přístup tooon místních systémů.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-214">In hello Logic App Designer, you can quickly select an API exposed from Azure API Management within a workflow, providing quick access tooon-premises systems.</span></span>

#### <a name="hybrid-connections-from-azure-app-service"></a><span data-ttu-id="c8f4a-215">Hybridní připojení z Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c8f4a-215">Hybrid connections from Azure App Service</span></span>

<span data-ttu-id="c8f4a-216">Funkce hello místní hybridního připojení můžete použít pro rozhraní API služby Azure a webové aplikace toocommunicate místní.</span><span class="sxs-lookup"><span data-stu-id="c8f4a-216">You can use hello on-premises hybrid connection feature for Azure API and Web apps toocommunicate on-premises.</span></span>  <span data-ttu-id="c8f4a-217">Informace o hybridní připojení a o tom, jak lze nalézt tooconfigure [v tomto článku](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c8f4a-217">Details on hybrid connections and how tooconfigure can be found [in this article](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8f4a-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8f4a-218">Next steps</span></span>
[<span data-ttu-id="c8f4a-219">Vytvoření šablony nasazení</span><span class="sxs-lookup"><span data-stu-id="c8f4a-219">Create a deployment template</span></span>](logic-apps-create-deploy-template.md)  
[<span data-ttu-id="c8f4a-220">Zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="c8f4a-220">Exception handling</span></span>](logic-apps-exception-handling.md)  
[<span data-ttu-id="c8f4a-221">Monitorování aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="c8f4a-221">Monitor your logic apps</span></span>](logic-apps-monitor-your-logic-apps.md)  
[<span data-ttu-id="c8f4a-222">Diagnostikování selhání aplikace logiky a problémy</span><span class="sxs-lookup"><span data-stu-id="c8f4a-222">Diagnosing logic app failures and issues</span></span>](logic-apps-diagnosing-failures.md)  
