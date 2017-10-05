---
title: "Zabezpečený přístup k Azure Logic Apps | Microsoft Docs"
description: "Přidejte zabezpečení pro ochranu přístupu k aktivační události, vstupy a výstupy, parametrů akcí a služeb používaných u pracovních postupů v Azure Logic Apps."
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
ms.openlocfilehash: 0528d660f590e106f61729f10f8f68da3fe58cb7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="secure-access-to-your-logic-apps"></a><span data-ttu-id="95a6f-103">Zabezpečený přístup k aplikacím logiky</span><span class="sxs-lookup"><span data-stu-id="95a6f-103">Secure access to your logic apps</span></span>

<span data-ttu-id="95a6f-104">Existují celou řadu nástrojů, které jsou k dispozici a pomáhá vám zabezpečit svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="95a6f-104">There are many tools available to help you secure your logic app.</span></span>

* <span data-ttu-id="95a6f-105">Zabezpečení přístupu k aktivaci aplikace logiky (HTTP žádosti o aktivační události)</span><span class="sxs-lookup"><span data-stu-id="95a6f-105">Securing access to trigger a logic app (HTTP Request Trigger)</span></span>
* <span data-ttu-id="95a6f-106">Zabezpečení přístupu ke správě, upravit nebo číst aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="95a6f-106">Securing access to manage, edit, or read a logic app</span></span>
* <span data-ttu-id="95a6f-107">Zabezpečení přístupu k obsahu vstupy a výstupy pro spuštění</span><span class="sxs-lookup"><span data-stu-id="95a6f-107">Securing access to contents of inputs and outputs for a run</span></span>
* <span data-ttu-id="95a6f-108">Zabezpečení parametry nebo vstupy v rámci akce v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="95a6f-108">Securing parameters or inputs within actions in a workflow</span></span>
* <span data-ttu-id="95a6f-109">Zabezpečení přístupu ke službám, které přijímají požadavky z pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="95a6f-109">Securing access to services that receive requests from a workflow</span></span>

## <a name="secure-access-to-trigger"></a><span data-ttu-id="95a6f-110">Zabezpečený přístup k aktivaci</span><span class="sxs-lookup"><span data-stu-id="95a6f-110">Secure access to trigger</span></span>

<span data-ttu-id="95a6f-111">Pokud pracujete s aplikace logiky, která se spustí na požadavek HTTP ([požadavku](../connectors/connectors-native-reqres.md) nebo [Webhooku](../connectors/connectors-native-webhook.md)), můžete omezit přístup tak, aby pouze autorizovaní klienti můžete aktivovat aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="95a6f-111">When you work with a logic app that fires on an HTTP Request ([Request](../connectors/connectors-native-reqres.md) or [Webhook](../connectors/connectors-native-webhook.md)), you can restrict access so that only authorized clients can fire the logic app.</span></span> <span data-ttu-id="95a6f-112">Všechny požadavky do aplikace logiky jsou zašifrovány a zabezpečené pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="95a6f-112">All requests into a logic app are encrypted and secured via SSL.</span></span>

### <a name="shared-access-signature"></a><span data-ttu-id="95a6f-113">Sdílený přístupový podpis</span><span class="sxs-lookup"><span data-stu-id="95a6f-113">Shared Access Signature</span></span>

<span data-ttu-id="95a6f-114">Obsahuje všechny koncové body požadavek pro aplikaci logiky [sdíleného přístupového podpisu (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) jako část adresy URL.</span><span class="sxs-lookup"><span data-stu-id="95a6f-114">Every request endpoint for a logic app includes a [Shared Access Signature (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) as part of the URL.</span></span> <span data-ttu-id="95a6f-115">Obsahuje každou adresu URL `sp`, `sv`, a `sig` parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="95a6f-115">Each URL contains a `sp`, `sv`, and `sig` query parameter.</span></span> <span data-ttu-id="95a6f-116">Oprávnění jsou určené `sp`a odpovídat metody HTTP povolena, `sv` je verze použitá ke generování, a `sig` se používá k ověření přístupu k aktivaci.</span><span class="sxs-lookup"><span data-stu-id="95a6f-116">Permissions are specified by `sp`, and correspond to HTTP methods allowed, `sv` is the version used to generate, and `sig` is used to authenticate access to trigger.</span></span> <span data-ttu-id="95a6f-117">Podpis je generována pomocí algoritmus SHA256 s tajným klíčem na všechny cesty adresy URL a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="95a6f-117">The signature is generated using the SHA256 algorithm with a secret key on all the URL paths and properties.</span></span> <span data-ttu-id="95a6f-118">Tajný klíč se nikdy zveřejněné a publikovali kde je udržována šifrovaná a uložené jako součást logiky aplikace.</span><span class="sxs-lookup"><span data-stu-id="95a6f-118">The secret key is never exposed and published, and is kept encrypted and stored as part of the logic app.</span></span> <span data-ttu-id="95a6f-119">Aplikace logiky pouze autorizuje aktivační události, které obsahují platný podpis vytvořen s tajným klíčem.</span><span class="sxs-lookup"><span data-stu-id="95a6f-119">Your logic app only authorizes triggers that contain a valid signature created with the secret key.</span></span>

#### <a name="regenerate-access-keys"></a><span data-ttu-id="95a6f-120">Opětovné vygenerování přístupových klíčů</span><span class="sxs-lookup"><span data-stu-id="95a6f-120">Regenerate access keys</span></span>

<span data-ttu-id="95a6f-121">Můžete vygenerovat nový zabezpečený klíč v kdykoli prostřednictvím portálu REST API nebo Azure.</span><span class="sxs-lookup"><span data-stu-id="95a6f-121">You can regenerate a new secure key at anytime through the REST API or Azure portal.</span></span> <span data-ttu-id="95a6f-122">Všechny aktuální adresy URL, které byly vygenerovány dříve pomocí starého klíče jsou zrušena a už nemáte oprávnění k aktivovat aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="95a6f-122">All current URLs that were generated previously using the old key are invalidated and no longer authorized to fire the logic app.</span></span>

1. <span data-ttu-id="95a6f-123">Na portálu Azure otevřete aplikaci logiky, kterou chcete znovu vygenerovat klíč</span><span class="sxs-lookup"><span data-stu-id="95a6f-123">In the Azure portal, open the logic app you want to regenerate a key</span></span>
1. <span data-ttu-id="95a6f-124">Klikněte **přístupové klíče** položky nabídky v části **nastavení**</span><span class="sxs-lookup"><span data-stu-id="95a6f-124">Click the **Access Keys** menu item under **Settings**</span></span>
1. <span data-ttu-id="95a6f-125">Vyberte klíč znovu vygenerovat a dokončete proces</span><span class="sxs-lookup"><span data-stu-id="95a6f-125">Choose the key to regenerate and complete the process</span></span>

<span data-ttu-id="95a6f-126">Adresy URL, jež načíst po opětovné generování jsou podepsány pomocí nového přístupového klíče.</span><span class="sxs-lookup"><span data-stu-id="95a6f-126">URLs you retrieve after regeneration are signed with the new access key.</span></span>

#### <a name="creating-callback-urls-with-an-expiration-date"></a><span data-ttu-id="95a6f-127">Vytvoření adresy URL zpětné volání s datum vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="95a6f-127">Creating callback URLs with an expiration date</span></span>

<span data-ttu-id="95a6f-128">Pokud sdílíte s ostatními stranami adresu URL, konkrétní klíče a data vypršení platnosti podle potřeby můžete vygenerovat adresy URL.</span><span class="sxs-lookup"><span data-stu-id="95a6f-128">If you are sharing the URL with other parties, you can generate URLs with specific keys and expiration dates as needed.</span></span> <span data-ttu-id="95a6f-129">Potom můžete bezproblémově vrátit klíče, nebo zajistěte, aby byl přístup k fire aplikace je omezený na určité časové rozpětí.</span><span class="sxs-lookup"><span data-stu-id="95a6f-129">You can then seamlessly roll keys, or ensure access to fire an app is restricted to a certain timespan.</span></span> <span data-ttu-id="95a6f-130">Můžete zadat datum vypršení platnosti pro adresu URL prostřednictvím [aplikace logiky REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):</span><span class="sxs-lookup"><span data-stu-id="95a6f-130">You can specify an expiration date for a URL through the [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):</span></span>

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

<span data-ttu-id="95a6f-131">V textu, obsahovat vlastnost `NotAfter` jako řetězec data JSON, který vrátí adresu URL zpětné volání, která je platná, dokud pouze `NotAfter` datum a čas.</span><span class="sxs-lookup"><span data-stu-id="95a6f-131">In the body, include the property `NotAfter` as a JSON date string, which returns a callback URL that is only valid until the `NotAfter` date and time.</span></span>

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a><span data-ttu-id="95a6f-132">Vytvoření adresy URL pomocí primární nebo sekundární tajný klíč</span><span class="sxs-lookup"><span data-stu-id="95a6f-132">Creating URLs with primary or secondary secret key</span></span>

<span data-ttu-id="95a6f-133">Při generování nebo seznam adres URL zpětného volání pro aktivační procedury založené na požadavku, můžete určete, který klíč sloužící k podepisování adresu URL.</span><span class="sxs-lookup"><span data-stu-id="95a6f-133">When you generate or list callback URLs for request-based triggers, you can also specify which key to use to sign the URL.</span></span>  <span data-ttu-id="95a6f-134">Můžete generovat adresu URL podepsány pomocí konkrétního klíče [aplikace logiky REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="95a6f-134">You can generate a URL signed by a specific key through the [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) as follows:</span></span>

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

<span data-ttu-id="95a6f-135">V textu, obsahovat vlastnost `KeyType` jako `Primary` nebo `Secondary`.</span><span class="sxs-lookup"><span data-stu-id="95a6f-135">In the body, include the property `KeyType` as either `Primary` or `Secondary`.</span></span>  <span data-ttu-id="95a6f-136">Vrátí adresu URL podepsaný zabezpečeného zadaný klíč.</span><span class="sxs-lookup"><span data-stu-id="95a6f-136">This returns a URL signed by the secure key specified.</span></span>

### <a name="restrict-incoming-ip-addresses"></a><span data-ttu-id="95a6f-137">Omezit příchozí IP adresy</span><span class="sxs-lookup"><span data-stu-id="95a6f-137">Restrict incoming IP addresses</span></span>

<span data-ttu-id="95a6f-138">Kromě sdíleného přístupového podpisu můžete chtít omezit volání aplikace logiky pouze z konkrétní klientů.</span><span class="sxs-lookup"><span data-stu-id="95a6f-138">In addition to the Shared Access Signature, you may wish to restrict calling a logic app only from specific clients.</span></span>  <span data-ttu-id="95a6f-139">Například pokud budete spravovat váš koncový bod pomocí Azure API Management, můžete omezit aplikace logiky pouze žádost přijmout, když požadavek pochází z IP adresy instance API Management.</span><span class="sxs-lookup"><span data-stu-id="95a6f-139">For example, if you manage your endpoint through Azure API Management, you can restrict the logic app to only accept the request when the request comes from the API Management instance IP address.</span></span>

<span data-ttu-id="95a6f-140">Toto nastavení lze konfigurovat v nastavení aplikace logiky:</span><span class="sxs-lookup"><span data-stu-id="95a6f-140">This setting can be configured within the logic app settings:</span></span>

1. <span data-ttu-id="95a6f-141">Na portálu Azure otevřete aplikaci logiky, kterou chcete přidat omezení podle IP adresy</span><span class="sxs-lookup"><span data-stu-id="95a6f-141">In the Azure portal, open the logic app you want to add IP address restrictions</span></span>
1. <span data-ttu-id="95a6f-142">Klikněte **konfigurace řízení přístupu** položky nabídky v části **nastavení**</span><span class="sxs-lookup"><span data-stu-id="95a6f-142">Click the **Access control configuration** menu item under **Settings**</span></span>
1. <span data-ttu-id="95a6f-143">Zadejte seznam rozsahů IP adres třeba přijmout, který má aktivační procedura</span><span class="sxs-lookup"><span data-stu-id="95a6f-143">Specify the list of IP address ranges to be accepted by the trigger</span></span>

<span data-ttu-id="95a6f-144">Platný rozsah IP má formát `192.168.1.1/255`.</span><span class="sxs-lookup"><span data-stu-id="95a6f-144">A valid IP range takes the format `192.168.1.1/255`.</span></span> <span data-ttu-id="95a6f-145">Pokud chcete aplikaci logiky má pouze provést jako aplikace vnořené logiky, vyberte **jenom jiné aplikace logiky** možnost.</span><span class="sxs-lookup"><span data-stu-id="95a6f-145">If you want the logic app to only fire as a nested logic app, select the **Only other logic apps** option.</span></span> <span data-ttu-id="95a6f-146">Tato možnost zapíše prázdné pole k prostředku význam jenom volání ze samotné (nadřazené logiku aplikace) služby úspěšně provést.</span><span class="sxs-lookup"><span data-stu-id="95a6f-146">This option writes an empty array to the resource, meaning only calls from the service itself (parent logic apps) fire successfully.</span></span>

> [!NOTE]
> <span data-ttu-id="95a6f-147">Pořád se dá spustit aplikaci logiky s aktivační událost žádosti přes rozhraní REST API nebo správu `/triggers/{triggerName}/run` bez ohledu na IP.</span><span class="sxs-lookup"><span data-stu-id="95a6f-147">You can still run a logic app with a request trigger through the REST API / Management `/triggers/{triggerName}/run` regardless of IP.</span></span> <span data-ttu-id="95a6f-148">Tento scénář vyžaduje ověřování proti REST API služby Azure a zobrazí všechny události v protokolu auditování Azure.</span><span class="sxs-lookup"><span data-stu-id="95a6f-148">This scenario requires authentication against the Azure REST API, and all events would appear in the Azure Audit Log.</span></span> <span data-ttu-id="95a6f-149">Zásady řízení přístupu sadu odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="95a6f-149">Set access control policies accordingly.</span></span>

#### <a name="setting-ip-ranges-on-the-resource-definition"></a><span data-ttu-id="95a6f-150">Nastavení rozsahy IP adres v definici prostředků</span><span class="sxs-lookup"><span data-stu-id="95a6f-150">Setting IP ranges on the resource definition</span></span>

<span data-ttu-id="95a6f-151">Pokud používáte [šablonu nasazení](logic-apps-create-deploy-template.md) k automatizaci nasazení, může být rozsah nastavení IP adresy nakonfigurované v šabloně prostředků.</span><span class="sxs-lookup"><span data-stu-id="95a6f-151">If you are using a [deployment template](logic-apps-create-deploy-template.md) to automate your deployments, the IP range settings can be configured on the resource template.</span></span>  

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

### <a name="adding-azure-active-directory-oauth-or-other-security"></a><span data-ttu-id="95a6f-152">Přidání služby Azure Active Directory, OAuth nebo jiných zabezpečení</span><span class="sxs-lookup"><span data-stu-id="95a6f-152">Adding Azure Active Directory, OAuth, or other security</span></span>

<span data-ttu-id="95a6f-153">Chcete-li přidat další protokoly autorizace nad aplikace logiky, [Azure API Management](https://azure.microsoft.com/services/api-management/) nabízí rozšířené možnosti monitorování, zabezpečení, zásady a dokumentaci pro libovolný koncový bod se schopností vystavit aplikace logiky jako rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="95a6f-153">To add more authorization protocols on top of a logic app, [Azure API Management](https://azure.microsoft.com/services/api-management/) offers rich monitoring, security, policy, and documentation for any endpoint with the capability to expose a logic app as an API.</span></span> <span data-ttu-id="95a6f-154">Azure API Management můžou zpřístupnit veřejných nebo privátních koncový bod pro aplikaci logiky, která může použít Azure Active Directory, certifikát, OAuth nebo jiné standardy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="95a6f-154">Azure API Management can expose a public or private endpoint for the logic app, which could use Azure Active Directory, certificate, OAuth, or other security standards.</span></span> <span data-ttu-id="95a6f-155">Po přijetí žádost o službě Azure API Management předává požadavek do aplikace logiky (provádění všechny potřebné transformace ani neukládají omezení).</span><span class="sxs-lookup"><span data-stu-id="95a6f-155">When a request is received, Azure API Management forwards the request to the logic app (performing any needed transformations or restrictions in-flight).</span></span> <span data-ttu-id="95a6f-156">Příchozí nastavení rozsahu IP na aplikaci logiky můžete použít možnost Povolit jenom aplikace logiky, aby se spouštěly ze správy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="95a6f-156">You can use the incoming IP range settings on the logic app to only allow the logic app to be triggered from API Management.</span></span>

## <a name="secure-access-to-manage-or-edit-logic-apps"></a><span data-ttu-id="95a6f-157">Zabezpečený přístup ke správě nebo upravovat aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="95a6f-157">Secure access to manage or edit logic apps</span></span>

<span data-ttu-id="95a6f-158">Pro operace správy v aplikaci logiky můžete omezit přístup tak, aby pouze konkrétní uživatele nebo skupiny se můžou k provádění operací na prostředek.</span><span class="sxs-lookup"><span data-stu-id="95a6f-158">You can restrict access to management operations on a logic app so that only specific users or groups are able to perform operations on the resource.</span></span> <span data-ttu-id="95a6f-159">Použití aplikace logiky Azure [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) funkci a můžete přizpůsobit pomocí stejných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="95a6f-159">Logic apps use the Azure [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) feature, and can be customized with the same tools.</span></span>  <span data-ttu-id="95a6f-160">Existuje několik integrovaných rolí, které můžete přiřadit také členy předplatného:</span><span class="sxs-lookup"><span data-stu-id="95a6f-160">There are a few built-in roles you can assign members of your subscription to as well:</span></span>

* <span data-ttu-id="95a6f-161">**Přispěvatel aplikace logiky** – poskytuje přístup k zobrazení, upravte a aktualizujte aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="95a6f-161">**Logic App Contributor** - Provides access to view, edit, and update a logic app.</span></span>  <span data-ttu-id="95a6f-162">Nelze odebrat prostředek nebo provádět operace správy.</span><span class="sxs-lookup"><span data-stu-id="95a6f-162">Cannot remove the resource or perform admin operations.</span></span>
* <span data-ttu-id="95a6f-163">**Operátor aplikace logiky** – můžete zobrazit aplikaci logiky a historie spouštění a povolit nebo zakázat.</span><span class="sxs-lookup"><span data-stu-id="95a6f-163">**Logic App Operator** - Can view the logic app and run history, and enable/disable.</span></span>  <span data-ttu-id="95a6f-164">Nelze upravit nebo aktualizovat definice.</span><span class="sxs-lookup"><span data-stu-id="95a6f-164">Cannot edit or update the definition.</span></span>

<span data-ttu-id="95a6f-165">Můžete také použít [Zámek prostředků Azure](../azure-resource-manager/resource-group-lock-resources.md) aby se zabránilo změně nebo odstranění aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="95a6f-165">You can also use [Azure Resource Lock](../azure-resource-manager/resource-group-lock-resources.md) to prevent changing or deleting logic apps.</span></span> <span data-ttu-id="95a6f-166">Tato funkce se hodí v situaci zabránit produkční prostředky z změn nebo odstranění.</span><span class="sxs-lookup"><span data-stu-id="95a6f-166">This capability is valuable to prevent production resources from changes or deletions.</span></span>

## <a name="secure-access-to-contents-of-the-run-history"></a><span data-ttu-id="95a6f-167">Zabezpečený přístup k obsahu historie spouštění</span><span class="sxs-lookup"><span data-stu-id="95a6f-167">Secure access to contents of the run history</span></span>

<span data-ttu-id="95a6f-168">Můžete omezit přístup k obsahu vstupy nebo výstupy z předchozích spuštění na konkrétní rozsahy IP adres.</span><span class="sxs-lookup"><span data-stu-id="95a6f-168">You can restrict access to contents of inputs or outputs from previous runs to specific IP address ranges.</span></span>  

<span data-ttu-id="95a6f-169">Všechna data v rámci spuštění pracovního postupu se šifrují během přenosu i v klidu.</span><span class="sxs-lookup"><span data-stu-id="95a6f-169">All data within a workflow run is encrypted in transit and at rest.</span></span> <span data-ttu-id="95a6f-170">Při volání do historie spouštění služby ověří žádost a poskytuje odkazy na žádosti a odpovědi vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="95a6f-170">When a call to run history is made, the service authenticates the request and provides links to the request and response inputs and outputs.</span></span> <span data-ttu-id="95a6f-171">Tento odkaz se dají chránit tak obsah vrátí pouze požadavky na Zobrazit obsah z určené rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="95a6f-171">This link can be protected so only requests to view content from a designated IP address range return the contents.</span></span> <span data-ttu-id="95a6f-172">Této funkci můžete použít pro ovládací prvek další přístup.</span><span class="sxs-lookup"><span data-stu-id="95a6f-172">You can use this capability for additional access control.</span></span> <span data-ttu-id="95a6f-173">Můžete zadat IP adresu jako i `0.0.0.0` , nemá nikdo přístup vstupy/výstupy.</span><span class="sxs-lookup"><span data-stu-id="95a6f-173">You could even specify an IP address like `0.0.0.0` so no one could access inputs/outputs.</span></span> <span data-ttu-id="95a6f-174">Pouze uživatel s oprávněními správce může odebrat toto omezení poskytuje možnost pro 'v běhu' přístup k obsahu pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="95a6f-174">Only someone with admin permissions could remove this restriction, providing the possibility for 'just-in-time' access to workflow contents.</span></span>

<span data-ttu-id="95a6f-175">Toto nastavení lze nakonfigurovat v rámci nastavení prostředků na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="95a6f-175">This setting can be configured within the resource settings of the Azure portal:</span></span>

1. <span data-ttu-id="95a6f-176">Na portálu Azure otevřete aplikaci logiky, kterou chcete přidat omezení podle IP adresy</span><span class="sxs-lookup"><span data-stu-id="95a6f-176">In the Azure portal, open the logic app you want to add IP address restrictions</span></span>
1. <span data-ttu-id="95a6f-177">Klikněte **konfigurace řízení přístupu** položky nabídky v části **nastavení**</span><span class="sxs-lookup"><span data-stu-id="95a6f-177">Click the **Access control configuration** menu item under **Settings**</span></span>
1. <span data-ttu-id="95a6f-178">Zadejte seznam rozsahů IP adres pro přístup k obsahu</span><span class="sxs-lookup"><span data-stu-id="95a6f-178">Specify the list of IP address ranges for access to content</span></span>

#### <a name="setting-ip-ranges-on-the-resource-definition"></a><span data-ttu-id="95a6f-179">Nastavení rozsahy IP adres v definici prostředků</span><span class="sxs-lookup"><span data-stu-id="95a6f-179">Setting IP ranges on the resource definition</span></span>

<span data-ttu-id="95a6f-180">Pokud používáte [šablonu nasazení](logic-apps-create-deploy-template.md) k automatizaci nasazení, může být rozsah nastavení IP adresy nakonfigurované v šabloně prostředků.</span><span class="sxs-lookup"><span data-stu-id="95a6f-180">If you are using a [deployment template](logic-apps-create-deploy-template.md) to automate your deployments, the IP range settings can be configured on the resource template.</span></span>  

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

## <a name="secure-parameters-and-inputs-within-a-workflow"></a><span data-ttu-id="95a6f-181">Zabezpečení parametry a vstupy v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="95a6f-181">Secure parameters and inputs within a workflow</span></span>

<span data-ttu-id="95a6f-182">Můžete chtít Parametrizace některých aspektů definice pracovního postupu pro nasazení v prostředí.</span><span class="sxs-lookup"><span data-stu-id="95a6f-182">You might want to parameterize some aspects of a workflow definition for deployment across environments.</span></span> <span data-ttu-id="95a6f-183">Některé parametry mohou být také zabezpečené parametry, které nechcete, aby se objevily při úpravě pracovního postupu, jako je například ID klienta a tajný klíč klienta pro [ověřování Azure Active Directory](../connectors/connectors-native-http.md#authentication) akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="95a6f-183">Also, some parameters might be secure parameters you don't want to appear when editing a workflow, such as a client ID and client secret for [Azure Active Directory authentication](../connectors/connectors-native-http.md#authentication) of an HTTP action.</span></span>

### <a name="using-parameters-and-secure-parameters"></a><span data-ttu-id="95a6f-184">Použití parametrů a parametrů zabezpečení</span><span class="sxs-lookup"><span data-stu-id="95a6f-184">Using parameters and secure parameters</span></span>

<span data-ttu-id="95a6f-185">Pro přístup k hodnotě parametru prostředků v době běhu [jazyk definic workflowů](http://aka.ms/logicappsdocs) poskytuje `@parameters()` operaci.</span><span class="sxs-lookup"><span data-stu-id="95a6f-185">To access the value of a resource parameter at runtime, the [workflow definition language](http://aka.ms/logicappsdocs) provides a `@parameters()` operation.</span></span> <span data-ttu-id="95a6f-186">Můžete také [zadejte parametry v šabloně nasazení prostředků](../azure-resource-manager/resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="95a6f-186">Also, you can [specify parameters in the resource deployment template](../azure-resource-manager/resource-group-authoring-templates.md#parameters).</span></span> <span data-ttu-id="95a6f-187">Ale pokud zadáte typem parametru jako `securestring`, parametr se zbytkem definice prostředku nejsou k dispozici a nebudou přístupné, a to pomocí zobrazení po nasazení.</span><span class="sxs-lookup"><span data-stu-id="95a6f-187">But if you specify the parameter type as `securestring`, the parameter won't be returned with the rest of the resource definition, and won't be accessible by viewing the resource after deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="95a6f-188">Pokud vaše parametr se používá v záhlaví nebo obsahu požadavku, může být parametr viditelné historie spouštění a odchozí požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="95a6f-188">If your parameter is used in the headers or body of a request, the parameter might be visible by accessing the run history and outgoing HTTP request.</span></span> <span data-ttu-id="95a6f-189">Ujistěte se, že odpovídajícím způsobem nastavit zásady váš přístup k obsahu.</span><span class="sxs-lookup"><span data-stu-id="95a6f-189">Make sure to set your content access policies accordingly.</span></span>
> <span data-ttu-id="95a6f-190">Nikdy jsou viditelné prostřednictvím vstupů nebo výstupů autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="95a6f-190">Authorization headers are never visible through inputs or outputs.</span></span> <span data-ttu-id="95a6f-191">Proto pokud tajný klíč se používá existuje, tajný klíč se dá načíst.</span><span class="sxs-lookup"><span data-stu-id="95a6f-191">So if the secret is being used there, the secret is not retrievable.</span></span>

#### <a name="resource-deployment-template-with-secrets"></a><span data-ttu-id="95a6f-192">Šablonu nasazení prostředků s tajné klíče</span><span class="sxs-lookup"><span data-stu-id="95a6f-192">Resource deployment template with secrets</span></span>

<span data-ttu-id="95a6f-193">Následující příklad ukazuje, nasazení, který odkazuje na zabezpečené parametr `secret` za běhu.</span><span class="sxs-lookup"><span data-stu-id="95a6f-193">The following example shows a deployment that references a secure parameter of `secret` at runtime.</span></span> <span data-ttu-id="95a6f-194">V souboru samostatné parametry, můžete zadat hodnotu pro prostředí `secret`, nebo použijte [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) načíst tajné klíče v nasazení čas.</span><span class="sxs-lookup"><span data-stu-id="95a6f-194">In a separate parameters file, you could specify the environment value for the `secret`, or use [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) to retrieve secrets at deploy time.</span></span>

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
                "body": "This is the request"
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

## <a name="secure-access-to-services-receiving-requests-from-a-workflow"></a><span data-ttu-id="95a6f-195">Zabezpečený přístup k přijímání požadavků z pracovního postupu služby</span><span class="sxs-lookup"><span data-stu-id="95a6f-195">Secure access to services receiving requests from a workflow</span></span>

<span data-ttu-id="95a6f-196">Existuje mnoho způsobů pomáhají zabezpečit žádný koncový bod, který potřebuje přístup k aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="95a6f-196">There are many ways to help secure any endpoint the logic app needs to access.</span></span>

### <a name="using-authentication-on-outbound-requests"></a><span data-ttu-id="95a6f-197">Pomocí ověřování pro odchozí požadavky</span><span class="sxs-lookup"><span data-stu-id="95a6f-197">Using authentication on outbound requests</span></span>

<span data-ttu-id="95a6f-198">Při práci s HTTP, HTTP + Swagger (otevřené rozhraní API) nebo akce Webhooku, můžete přidat ověřování na žádost o odeslání.</span><span class="sxs-lookup"><span data-stu-id="95a6f-198">When working with an HTTP, HTTP + Swagger (Open API), or Webhook action, you can add authentication to the request being sent.</span></span> <span data-ttu-id="95a6f-199">Můžete uvést základní ověřování, ověřování pomocí certifikátu nebo ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="95a6f-199">You could include basic authentication, certificate authentication, or Azure Active Directory authentication.</span></span> <span data-ttu-id="95a6f-200">Podrobnosti o tom, jak nakonfigurovat toto ověřování najdete [v tomto článku](../connectors/connectors-native-http.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="95a6f-200">Details on how to configure this authentication can be found [in this article](../connectors/connectors-native-http.md#authentication).</span></span>

### <a name="restricting-access-to-logic-app-ip-addresses"></a><span data-ttu-id="95a6f-201">Omezení přístupu k logiku aplikace IP adresy</span><span class="sxs-lookup"><span data-stu-id="95a6f-201">Restricting access to logic app IP addresses</span></span>

<span data-ttu-id="95a6f-202">Všechna volání z aplikace logiky pocházet z konkrétní sadu IP adres podle oblastí.</span><span class="sxs-lookup"><span data-stu-id="95a6f-202">All calls from logic apps come from a specific set of IP addresses per region.</span></span> <span data-ttu-id="95a6f-203">Můžete přidat další filtrování pouze přijímání požadavků z těchto určené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="95a6f-203">You can add additional filtering to only accept requests from those designated IP addresses.</span></span> <span data-ttu-id="95a6f-204">Seznam těchto IP adres najdete v tématu [logiku aplikace omezení a konfigurace](logic-apps-limits-and-config.md#configuration).</span><span class="sxs-lookup"><span data-stu-id="95a6f-204">For a list of those IP addresses, see [logic app limits and configuration](logic-apps-limits-and-config.md#configuration).</span></span>

### <a name="on-premises-connectivity"></a><span data-ttu-id="95a6f-205">Místní připojení</span><span class="sxs-lookup"><span data-stu-id="95a6f-205">On-premises connectivity</span></span>

<span data-ttu-id="95a6f-206">Aplikace logiky poskytují integraci se službou několik zajistit zabezpečený a spolehlivý místní komunikace.</span><span class="sxs-lookup"><span data-stu-id="95a6f-206">Logic apps provide integration with several services to provide secure and reliable on-premises communication.</span></span>

#### <a name="on-premises-data-gateway"></a><span data-ttu-id="95a6f-207">Místní brána dat</span><span class="sxs-lookup"><span data-stu-id="95a6f-207">On-premises data gateway</span></span>

<span data-ttu-id="95a6f-208">Mnoho spravovaných konektorů pro logic apps poskytuje zabezpečené připojení k místním systémům, včetně systému souborů, SQL, SharePoint, DB2 a další.</span><span class="sxs-lookup"><span data-stu-id="95a6f-208">Many managed connectors for logic apps provide secure connectivity to on-premises systems, including File System, SQL, SharePoint, DB2, and more.</span></span> <span data-ttu-id="95a6f-209">Brána předává data z místního zdroje na šifrované kanály přes Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="95a6f-209">The gateway relays data from on-premises sources on encrypted channels through the Azure Service Bus.</span></span> <span data-ttu-id="95a6f-210">Veškerý provoz pochází jako zabezpečené odchozí provoz z agenta brány.</span><span class="sxs-lookup"><span data-stu-id="95a6f-210">All traffic originates as secure outbound traffic from the gateway agent.</span></span> <span data-ttu-id="95a6f-211">Další informace o [fungování bránu dat](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="95a6f-211">Learn more about [how the data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span>

#### <a name="azure-api-management"></a><span data-ttu-id="95a6f-212">Azure API Management</span><span class="sxs-lookup"><span data-stu-id="95a6f-212">Azure API Management</span></span>

<span data-ttu-id="95a6f-213">[Azure API Management](https://azure.microsoft.com/services/api-management/) možnosti místní připojení, včetně site-to-site VPN a ExpressRoute integrace pro zabezpečené proxy a komunikaci místních systémů.</span><span class="sxs-lookup"><span data-stu-id="95a6f-213">[Azure API Management](https://azure.microsoft.com/services/api-management/) has on-premises connectivity options, including site-to-site VPN and ExpressRoute integration for secured proxy and communication to on-premises systems.</span></span> <span data-ttu-id="95a6f-214">V návrháři aplikace logiky můžete rychle vybrat rozhraní API zveřejněné z Azure API Management v rámci pracovního postupu, že poskytuje rychlý přístup k místním systémům.</span><span class="sxs-lookup"><span data-stu-id="95a6f-214">In the Logic App Designer, you can quickly select an API exposed from Azure API Management within a workflow, providing quick access to on-premises systems.</span></span>

#### <a name="hybrid-connections-from-azure-app-service"></a><span data-ttu-id="95a6f-215">Hybridní připojení z Azure App Service</span><span class="sxs-lookup"><span data-stu-id="95a6f-215">Hybrid connections from Azure App Service</span></span>

<span data-ttu-id="95a6f-216">Pro komunikaci v místě, můžete použít funkci Místní hybridní připojení pro rozhraní API služby Azure a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="95a6f-216">You can use the on-premises hybrid connection feature for Azure API and Web apps to communicate on-premises.</span></span>  <span data-ttu-id="95a6f-217">Naleznete informace o hybridních připojení a o tom, jak nakonfigurovat [v tomto článku](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="95a6f-217">Details on hybrid connections and how to configure can be found [in this article](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="95a6f-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95a6f-218">Next steps</span></span>
[<span data-ttu-id="95a6f-219">Vytvoření šablony nasazení</span><span class="sxs-lookup"><span data-stu-id="95a6f-219">Create a deployment template</span></span>](logic-apps-create-deploy-template.md)  
[<span data-ttu-id="95a6f-220">Zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="95a6f-220">Exception handling</span></span>](logic-apps-exception-handling.md)  
[<span data-ttu-id="95a6f-221">Monitorování aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="95a6f-221">Monitor your logic apps</span></span>](logic-apps-monitor-your-logic-apps.md)  
[<span data-ttu-id="95a6f-222">Diagnostikování selhání aplikace logiky a problémy</span><span class="sxs-lookup"><span data-stu-id="95a6f-222">Diagnosing logic app failures and issues</span></span>](logic-apps-diagnosing-failures.md)  
