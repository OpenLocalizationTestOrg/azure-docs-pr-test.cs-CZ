---
title: "Volání, aktivační události nebo vnořit pracovních s koncovými body HTTP - Azure Logic Apps | Microsoft Docs"
description: "Nastavit koncové body HTTP zavolat, aktivaci nebo vnořit pracovní postupy pro Azure Logic Apps"
services: logic-apps
keywords: "pracovní postupy, koncových bodů protokolu HTTP"
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: c92692db23ac59f67890e26cce6b2d3272e8901d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a><span data-ttu-id="70044-104">Volání, aktivaci nebo vnořit pracovních s koncovými body HTTP v aplikacích logiky</span><span class="sxs-lookup"><span data-stu-id="70044-104">Call, trigger, or nest workflows with HTTP endpoints in logic apps</span></span>

<span data-ttu-id="70044-105">Synchronní koncové body HTTP můžou nativně zpřístupnit jako triggery pro aplikace logiky, mohli aktivovat nebo volání aplikace logiky prostřednictvím adresy URL.</span><span class="sxs-lookup"><span data-stu-id="70044-105">You can natively expose synchronous HTTP endpoints as triggers on logic apps so that you can trigger or call your logic apps through a URL.</span></span> <span data-ttu-id="70044-106">Pracovní postupy lze také vnořit ve vašich logic apps pomocí vzor s koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="70044-106">You can also nest workflows in your logic apps by using a pattern of callable endpoints.</span></span>

<span data-ttu-id="70044-107">Pokud chcete vytvořit koncových bodů protokolu HTTP, můžete přidat tyto triggery, aby aplikace logiky můžete přijímat příchozí požadavky:</span><span class="sxs-lookup"><span data-stu-id="70044-107">To create HTTP endpoints, you can add these triggers so that your logic apps can receive incoming requests:</span></span>

* [<span data-ttu-id="70044-108">Požadavek</span><span class="sxs-lookup"><span data-stu-id="70044-108">Request</span></span>](../connectors/connectors-native-reqres.md)

* [<span data-ttu-id="70044-109">Rozhraní API připojení Webhooku</span><span class="sxs-lookup"><span data-stu-id="70044-109">API Connection Webhook</span></span>](logic-apps-workflow-actions-triggers.md#api-connection-trigger)

* [<span data-ttu-id="70044-110">HTTP Webhook</span><span class="sxs-lookup"><span data-stu-id="70044-110">HTTP Webhook</span></span>](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > <span data-ttu-id="70044-111">I když pomocí našich ukázkách **požadavku** aktivační událost, můžete používat kterýkoli z uvedených aktivačních událostí protokolu HTTP, a všechny zásady stejně jako se vztahují na jiné typy aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="70044-111">Although our examples use the **Request** trigger, you can use any of the listed HTTP triggers, and all principles identically apply to the other trigger types.</span></span>

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a><span data-ttu-id="70044-112">Nastavit koncový bod HTTP pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="70044-112">Set up an HTTP endpoint for your logic app</span></span>

<span data-ttu-id="70044-113">Chcete-li vytvořit koncový bod protokolu HTTP, přidejte aktivační událost, která může přijímat příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="70044-113">To create an HTTP endpoint, add a trigger that can receive incoming requests.</span></span>

1. <span data-ttu-id="70044-114">Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal").</span><span class="sxs-lookup"><span data-stu-id="70044-114">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="70044-115">Přejděte do aplikace logiky a otevřete návrhář aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="70044-115">Go to your logic app, and open Logic App Designer.</span></span>

2. <span data-ttu-id="70044-116">Přidejte aktivační událost, která umožní aplikaci logiky přijímat příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="70044-116">Add a trigger that lets your logic app receive incoming requests.</span></span> <span data-ttu-id="70044-117">Například přidat **požadavku** aktivační svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="70044-117">For example, add the **Request** trigger to your logic app.</span></span>

3.  <span data-ttu-id="70044-118">V části **požadavku schématu JSON textu**, můžete volitelně zadat schématu JSON pro datová část (data), které očekáváte, že aktivační událost pro příjem.</span><span class="sxs-lookup"><span data-stu-id="70044-118">Under **Request Body JSON Schema**, you can optionally enter a JSON schema for the payload (data) that you expect the trigger to receive.</span></span>

    <span data-ttu-id="70044-119">Toto schéma návrháře používá ke generování tokenů, které aplikace logiky můžete využívat, analyzovat a předání dat z aktivace prostřednictvím pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="70044-119">The designer uses this schema for generating tokens that your logic app can use to consume, parse, and pass data from the trigger through your workflow.</span></span> 
    <span data-ttu-id="70044-120">Další informace o [tokeny vygenerovat z JSON schémata](#generated-tokens).</span><span class="sxs-lookup"><span data-stu-id="70044-120">More about [tokens generated from JSON schemas](#generated-tokens).</span></span>

    <span data-ttu-id="70044-121">V tomto příkladu zadejte schéma v Návrháři zobrazeny:</span><span class="sxs-lookup"><span data-stu-id="70044-121">For this example, enter the schema shown in the designer:</span></span>

    ```json
    {
      "type": "object",
      "properties": {
        "address": {
          "type": "string"
        }
      },
      "required": [
        "address"
      ]
    }
    ```

    ![Přidat akci požadavku][1]

    > [!TIP]
    > 
    > <span data-ttu-id="70044-123">Schéma pro ukázku datové části JSON můžete vygenerovat z nástroje, jako je [jsonschema.net](http://jsonschema.net/), nebo v **požadavku** aktivační události tak, že zvolíte **datová část ukázky použít ke generování schématu**.</span><span class="sxs-lookup"><span data-stu-id="70044-123">You can generate a schema for a sample JSON payload from a tool like [jsonschema.net](http://jsonschema.net/), or in the **Request** trigger by choosing **Use sample payload to generate schema**.</span></span> 
    > <span data-ttu-id="70044-124">Zadejte vaše datová část ukázky a vyberte **provádí**.</span><span class="sxs-lookup"><span data-stu-id="70044-124">Enter your sample payload, and choose **Done**.</span></span>

    <span data-ttu-id="70044-125">Například tato ukázková datová část:</span><span class="sxs-lookup"><span data-stu-id="70044-125">For example, this sample payload:</span></span>

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    <span data-ttu-id="70044-126">generuje toto schéma:</span><span class="sxs-lookup"><span data-stu-id="70044-126">generates this schema:</span></span>

    ```json
    }
       "type": "object",
       "properties": {
          "address": {
             "type": "string" 
          }
       }
    }
    ```

4.  <span data-ttu-id="70044-127">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="70044-127">Save your logic app.</span></span> <span data-ttu-id="70044-128">V části **HTTP POST na tuto adresu URL**, nyní byste měli najít adresu URL generovaného zpětného volání, jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="70044-128">Under **HTTP POST to this URL**, you should now find a generated callback URL, like this example:</span></span>

    ![Adresa URL generovaného zpětného volání pro koncový bod](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    <span data-ttu-id="70044-130">Tato adresa URL obsahuje klíč sdíleného přístupového podpisu (SAS) v parametry dotazu, které se používají pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="70044-130">This URL contains a Shared Access Signature (SAS) key in the query parameters that are used for authentication.</span></span> 
    <span data-ttu-id="70044-131">Můžete také získat adresu URL koncového bodu protokolu HTTP z vaší přehled aplikace logiky na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="70044-131">You can also get the HTTP endpoint URL from your logic app overview in the Azure portal.</span></span> <span data-ttu-id="70044-132">V části **historie aktivační událost**, vyberte aktivační událost:</span><span class="sxs-lookup"><span data-stu-id="70044-132">Under **Trigger History**, select your trigger:</span></span>

    ![Získat adresu URL koncového bodu protokolu HTTP z portálu Azure][2]

    <span data-ttu-id="70044-134">Nebo můžete získat adresu URL tím, že toto volání:</span><span class="sxs-lookup"><span data-stu-id="70044-134">Or you can get the URL by making this call:</span></span>

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-the-http-method-for-your-trigger"></a><span data-ttu-id="70044-135">Změnit metodu HTTP aktivační události</span><span class="sxs-lookup"><span data-stu-id="70044-135">Change the HTTP method for your trigger</span></span>

<span data-ttu-id="70044-136">Ve výchozím nastavení **požadavku** aktivační událost očekává požadavek HTTP POST, ale můžete použít jinou metodu HTTP.</span><span class="sxs-lookup"><span data-stu-id="70044-136">By default, the **Request** trigger expects an HTTP POST request, but you can use a different HTTP method.</span></span> 

> [!NOTE]
> <span data-ttu-id="70044-137">Můžete zadat typ pouze jednu metodu.</span><span class="sxs-lookup"><span data-stu-id="70044-137">You can specify only one method type.</span></span>

1. <span data-ttu-id="70044-138">Na vaše **požadavku** aktivovat, zvolte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="70044-138">On your **Request** trigger, choose **Show advanced options**.</span></span>

2. <span data-ttu-id="70044-139">Otevřete **metoda** seznamu.</span><span class="sxs-lookup"><span data-stu-id="70044-139">Open the **Method** list.</span></span> <span data-ttu-id="70044-140">V tomto příkladu vyberte **získat** , aby bylo později otestovat adresu URL vašeho koncového bodu protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="70044-140">For this example, select **GET** so that you can test your HTTP endpoint's URL later.</span></span>

    > [!NOTE]
    > <span data-ttu-id="70044-141">Můžete vybrat jinou metodu HTTP, nebo zadat vlastní metodu pro aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="70044-141">You can select any other HTTP method, or specify a custom method for your own logic app.</span></span>

    ![Změnit metodu HTTP](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a><span data-ttu-id="70044-143">Přijměte parametry prostřednictvím vaše adresa URL koncového bodu protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="70044-143">Accept parameters through your HTTP endpoint URL</span></span>

<span data-ttu-id="70044-144">Pokud chcete, aby vaše adresa URL koncového bodu protokolu HTTP tak, aby přijímal parametry, přizpůsobit vaší aktivační událost relativní cestu.</span><span class="sxs-lookup"><span data-stu-id="70044-144">When you want your HTTP endpoint URL to accept parameters, customize your trigger's relative path.</span></span>

1. <span data-ttu-id="70044-145">Na vaše **požadavku** aktivovat, zvolte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="70044-145">On your **Request** trigger, choose **Show advanced options**.</span></span> 

2. <span data-ttu-id="70044-146">V části **metoda**, zadejte metodu protokolu HTTP, který chcete žádost o použít.</span><span class="sxs-lookup"><span data-stu-id="70044-146">Under **Method**, specify the HTTP method that you want your request to use.</span></span> <span data-ttu-id="70044-147">V tomto příkladu vyberte **získat** metodu, pokud jste tak ještě neučinili, tak, aby bylo možné otestovat adresu URL vašeho koncového bodu protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="70044-147">For this example, select the **GET** method, if you haven't already, so that you can test your HTTP endpoint's URL.</span></span>

      > [!NOTE]
      > <span data-ttu-id="70044-148">Když zadáte relativní cesta aktivační události, musíte zadat metodu HTTP také explicitně aktivační události.</span><span class="sxs-lookup"><span data-stu-id="70044-148">When you specify a relative path for your trigger, you must also explicitly specify an HTTP method for your trigger.</span></span>

3. <span data-ttu-id="70044-149">V části **relativní cestu**, zadejte relativní cestu pro parametr, který by měl přijmout adresu URL, například `customers/{customerID}`.</span><span class="sxs-lookup"><span data-stu-id="70044-149">Under **Relative path**, specify the relative path for the parameter that your URL should accept, for example, `customers/{customerID}`.</span></span>

    ![Zadejte metodu HTTP a relativní cestu pro parametr](./media/logic-apps-http-endpoint/relativeurl.png)

4. <span data-ttu-id="70044-151">Chcete-li použít parametr, přidejte **odpovědi** akce do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="70044-151">To use the parameter, add a **Response** action to your logic app.</span></span> <span data-ttu-id="70044-152">(V části aktivační událost, zvolte **nový krok** > **přidat akci** > **odpovědi**)</span><span class="sxs-lookup"><span data-stu-id="70044-152">(Under your trigger, choose **New step** > **Add an action** > **Response**)</span></span> 

5. <span data-ttu-id="70044-153">Do odpovědi **textu**, zahrnout token pro parametr, který jste zadali v relativní cestě vaší aktivační události.</span><span class="sxs-lookup"><span data-stu-id="70044-153">In your response's **Body**, include the token for the parameter that you specified in your trigger's relative path.</span></span>

    <span data-ttu-id="70044-154">Chcete-li například vrátit `Hello {customerID}`, aktualizovat vaši odpověď **textu** s `Hello {customerID token}`.</span><span class="sxs-lookup"><span data-stu-id="70044-154">For example, to return `Hello {customerID}`, update your response's **Body** with `Hello {customerID token}`.</span></span> 
    <span data-ttu-id="70044-155">Seznamu dynamického obsahu by měl zobrazit a zobrazit `customerID` tokenu jste ji vybrat.</span><span class="sxs-lookup"><span data-stu-id="70044-155">The dynamic content list should appear and show the `customerID` token for you to select.</span></span>

    ![Přidání parametrů do odpovědi](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    <span data-ttu-id="70044-157">Vaše **textu** by měl vypadat jako tento příklad:</span><span class="sxs-lookup"><span data-stu-id="70044-157">Your **Body** should look like this example:</span></span>

    ![Text odpovědi s parametrem](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. <span data-ttu-id="70044-159">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="70044-159">Save your logic app.</span></span> 

    <span data-ttu-id="70044-160">Vaše adresa URL koncového bodu protokolu HTTP nyní zahrnuje relativní cesta, například:</span><span class="sxs-lookup"><span data-stu-id="70044-160">Your HTTP endpoint URL now includes the relative path, for example:</span></span> 

    <span data-ttu-id="70044-161">https & # 58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...</span><span class="sxs-lookup"><span data-stu-id="70044-161">https&#58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...</span></span>

7. <span data-ttu-id="70044-162">Pokud chcete otestovat váš koncový bod protokolu HTTP, zkopírujte a vložte adresu URL aktualizované do jiného okna prohlížeče, ale nahraďte `{customerID}` s `123456`, a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="70044-162">To test your HTTP endpoint, copy and paste the updated URL into another browser window, but replace `{customerID}` with `123456`, and press Enter.</span></span>

    <span data-ttu-id="70044-163">Váš prohlížeč by měl zobrazit tento text:</span><span class="sxs-lookup"><span data-stu-id="70044-163">Your browser should show this text:</span></span> 

    `Hello 123456`

<a name="generated-tokens"></a>
### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a><span data-ttu-id="70044-164">Tokeny vygenerovat z JSON schémata pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="70044-164">Tokens generated from JSON schemas for your logic app</span></span>

<span data-ttu-id="70044-165">Když zadáte schématu JSON ve vaší **požadavku** aktivační událost, návrháře logiku aplikace generuje tokeny pro vlastnosti v tomto schématu.</span><span class="sxs-lookup"><span data-stu-id="70044-165">When you provide a JSON schema in your **Request** trigger, the Logic App Designer generates tokens for properties in that schema.</span></span> <span data-ttu-id="70044-166">Pak můžete tyto tokeny pro předávání dat prostřednictvím pracovní postup aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="70044-166">You can then use those tokens for passing data through your logic app workflow.</span></span>

<span data-ttu-id="70044-167">Pro tento příklad, pokud přidáte `title` a `name` vlastnosti vašeho schématu JSON, jejich tokeny jsou nyní k dispozici pro použití v dalších krocích pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="70044-167">For this example, if you add the `title` and `name` properties to your JSON schema, their tokens are now available to use in later workflow steps.</span></span> 

<span data-ttu-id="70044-168">Tady je kompletní schématu JSON:</span><span class="sxs-lookup"><span data-stu-id="70044-168">Here is the complete JSON schema:</span></span>

```json
{
   "type": "object",
   "properties": {
      "address": {
         "type": "string"
      },
      "title": {
         "type": "string"
      },
      "name": {
         "type": "string"
      }
   },
   "required": [
      "address",
      "title",
      "name"
   ]
}
```

## <a name="create-nested-workflows-for-logic-apps"></a><span data-ttu-id="70044-169">Vytvoření vnořené pracovní postupy pro logic apps</span><span class="sxs-lookup"><span data-stu-id="70044-169">Create nested workflows for logic apps</span></span>

<span data-ttu-id="70044-170">Pracovní postupy lze vnořit ve vaší aplikaci logiky můžete přidat další logiku aplikace, které může přijímat požadavky.</span><span class="sxs-lookup"><span data-stu-id="70044-170">You can nest workflows in your logic app by adding other logic apps that can receive requests.</span></span> <span data-ttu-id="70044-171">Pokud chcete tyto aplikace logiky, přidejte **Azure Logic Apps – zvolte pracovní postup Logic Apps** akce aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="70044-171">To include these logic apps, add the **Azure Logic Apps - Choose a Logic Apps workflow** action to your trigger.</span></span> <span data-ttu-id="70044-172">Pak můžete vybrat z oprávněné logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="70044-172">You can then select from eligible logic apps.</span></span>

![Přidat další aplikace logiky](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a><span data-ttu-id="70044-174">Volání nebo aktivovat aplikace logiky prostřednictvím koncových bodů protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="70044-174">Call or trigger logic apps through HTTP endpoints</span></span>

<span data-ttu-id="70044-175">Jakmile vytvoříte koncový bod protokolu HTTP, můžete aktivovat svou aplikaci logiky prostřednictvím `POST` metodu pro úplnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="70044-175">After you create your HTTP endpoint, you can trigger your logic app through a `POST` method to the full URL.</span></span> <span data-ttu-id="70044-176">Logiku aplikace mají integrovanou podporu pro koncové body přímý přístup.</span><span class="sxs-lookup"><span data-stu-id="70044-176">Logic apps have built-in support for direct-access endpoints.</span></span>

## <a name="reference-content-from-an-incoming-request"></a><span data-ttu-id="70044-177">Odkaz na obsah z příchozího požadavku</span><span class="sxs-lookup"><span data-stu-id="70044-177">Reference content from an incoming request</span></span>

<span data-ttu-id="70044-178">Pokud je typ obsahu je `application/json`, vlastnosti, můžete odkazovat z příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="70044-178">If the content's type is `application/json`, you can reference properties from the incoming request.</span></span> <span data-ttu-id="70044-179">Obsah, jinak je považována za binární jedné jednotky, které můžete předat pro jiná rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="70044-179">Otherwise, content is treated as a single binary unit that you can pass to other APIs.</span></span> <span data-ttu-id="70044-180">Chcete-li tento obsah uvnitř pracovního postupu, je nutné převést obsah.</span><span class="sxs-lookup"><span data-stu-id="70044-180">To reference this content inside the workflow, you must convert that content.</span></span> <span data-ttu-id="70044-181">Například pokud předáte `application/xml` obsahu, můžete použít `@xpath()` pro extrakci XPath nebo `@json()` pro převod XML do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="70044-181">For example, if you pass `application/xml` content, you can use `@xpath()` for an XPath extraction, or `@json()` for converting XML to JSON.</span></span> <span data-ttu-id="70044-182">Další informace o [práce s typy obsahu](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="70044-182">Learn about [working with content types](../logic-apps/logic-apps-content-type.md).</span></span>

<span data-ttu-id="70044-183">Chcete-li získat výstup z příchozí žádosti, můžete použít `@triggerOutputs()` funkce.</span><span class="sxs-lookup"><span data-stu-id="70044-183">To get the output from an incoming request, you can use the `@triggerOutputs()` function.</span></span> <span data-ttu-id="70044-184">Výstup může vypadat jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="70044-184">The output might look like this example:</span></span>

```json
{
    "headers": {
        "content-type" : "application/json"
    },
    "body": {
        "myProperty" : "property value"
    }
}
```

<span data-ttu-id="70044-185">Pro přístup k `body` vlastnost můžete konkrétně použít `@triggerBody()` zástupce.</span><span class="sxs-lookup"><span data-stu-id="70044-185">To access the `body` property specifically, you can use the `@triggerBody()` shortcut.</span></span> 

## <a name="respond-to-requests"></a><span data-ttu-id="70044-186">Reaguje na požadavky.</span><span class="sxs-lookup"><span data-stu-id="70044-186">Respond to requests</span></span>

<span data-ttu-id="70044-187">Můžete chtít reagovat na určité požadavky, které začínají aplikace logiky vrácením obsah volajícímu.</span><span class="sxs-lookup"><span data-stu-id="70044-187">You might want to respond to certain requests that start a logic app by returning content to the caller.</span></span> <span data-ttu-id="70044-188">Chcete-li vytvořit stavový kód, záhlaví a text na vaši odpověď, můžete použít **odpovědi** akce.</span><span class="sxs-lookup"><span data-stu-id="70044-188">To construct the status code, header, and body for your response, you can use the **Response** action.</span></span> <span data-ttu-id="70044-189">Tato akce může vyskytovat kdekoli v aplikaci logiky, nikoli pouze na konci pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="70044-189">This action can appear anywhere in your logic app, not just at the end of your workflow.</span></span>

> [!NOTE] 
> <span data-ttu-id="70044-190">Pokud svou aplikaci logiky neobsahuje **odpovědi**, odpoví koncový bod HTTP *okamžitě* s **202 platných** stavu.</span><span class="sxs-lookup"><span data-stu-id="70044-190">If your logic app doesn't include a **Response**, the HTTP endpoint responds *immediately* with a **202 Accepted** status.</span></span> <span data-ttu-id="70044-191">Pro původní žádost se získat odpověď, také všechny kroky potřebné pro odpověď musí dokončit v rámci [časový limit požadavku](./logic-apps-limits-and-config.md) Pokud volat pracovní postup jako vnořené logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="70044-191">Also, for the original request to get the response, all steps required for the response must finish within the [request timeout limit](./logic-apps-limits-and-config.md) unless you call the workflow as a nested logic app.</span></span> <span data-ttu-id="70044-192">V případě žádná odpověď v rámci tohoto limitu příchozího požadavku vyprší časový limit a obdrží odpověď HTTP **408 časový limit klienta**.</span><span class="sxs-lookup"><span data-stu-id="70044-192">If no response happens within this limit, the incoming request times out and receives the HTTP response **408 Client timeout**.</span></span> <span data-ttu-id="70044-193">Pro vnořené logic apps pokračuje aplikace logiky nadřazené čekat na odpověď, dokud nebude dokončena, bez ohledu na to, jak dlouho se vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="70044-193">For nested logic apps, the parent logic app continues to wait for a response until completed, regardless of how much time is required.</span></span>

### <a name="construct-the-response"></a><span data-ttu-id="70044-194">Vytvoření odpovědi</span><span class="sxs-lookup"><span data-stu-id="70044-194">Construct the response</span></span>

<span data-ttu-id="70044-195">V textu odpovědi může obsahovat více než jedno záhlaví a libovolný typ obsahu.</span><span class="sxs-lookup"><span data-stu-id="70044-195">You can include more than one header and any type of content in the response body.</span></span> <span data-ttu-id="70044-196">V našem příkladu odpovědi hlavičku Určuje, že odpověď má typ obsahu `application/json`.</span><span class="sxs-lookup"><span data-stu-id="70044-196">In our example response, the header specifies that the response has content type `application/json`.</span></span> <span data-ttu-id="70044-197">a text obsahuje `title` a `name`, podle schématu JSON aktualizované dříve **požadavku** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="70044-197">and the body contains `title` and `name`, based on the JSON schema updated previously for the **Request** trigger.</span></span>

![Akce odpovědi HTTP][3]

<span data-ttu-id="70044-199">Odpovědi mít tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="70044-199">Responses have these properties:</span></span>

| <span data-ttu-id="70044-200">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="70044-200">Property</span></span> | <span data-ttu-id="70044-201">Popis</span><span class="sxs-lookup"><span data-stu-id="70044-201">Description</span></span> |
| --- | --- |
| <span data-ttu-id="70044-202">statusCode</span><span class="sxs-lookup"><span data-stu-id="70044-202">statusCode</span></span> |<span data-ttu-id="70044-203">Určuje kód stavu HTTP pro reagovat na příchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="70044-203">Specifies the HTTP status code for responding to the incoming request.</span></span> <span data-ttu-id="70044-204">Tento kód může být jakýkoli platný stavový kód, který začíná 2xx, 4xx nebo 5xx.</span><span class="sxs-lookup"><span data-stu-id="70044-204">This code can be any valid status code that starts with 2xx, 4xx, or 5xx.</span></span> <span data-ttu-id="70044-205">Stavové kódy 3xx však nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="70044-205">However, 3xx status codes are not permitted.</span></span> |
| <span data-ttu-id="70044-206">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="70044-206">headers</span></span> |<span data-ttu-id="70044-207">Definuje libovolný počet hlaviček, které chcete zahrnout do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="70044-207">Defines any number of headers to include in the response.</span></span> |
| <span data-ttu-id="70044-208">Text</span><span class="sxs-lookup"><span data-stu-id="70044-208">body</span></span> |<span data-ttu-id="70044-209">Určuje objekt textu, který může být řetězec, objekt JSON nebo i binární obsah na něj odkazovat z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="70044-209">Specifies a body object that can be a string, a JSON object, or even binary content referenced from a previous step.</span></span> |

<span data-ttu-id="70044-210">Tady je co schématu JSON teď pro vypadá **odpovědi** akce:</span><span class="sxs-lookup"><span data-stu-id="70044-210">Here's what the JSON schema looks like now for the **Response** action:</span></span>

``` json
"Response": {
   "inputs": {
      "body": {
         "title": "@{triggerBody()?['title']}",
         "name": "@{triggerBody()?['name']}"
      },
      "headers": {
           "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {},
   "type": "Response"
}
```

> [!TIP]
> <span data-ttu-id="70044-211">Chcete-li zobrazit úplnou definici JSON pro svou aplikaci logiky na návrháře aplikace logiky, zvolte **Code zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="70044-211">To view the complete JSON definition for your logic app, on the Logic App Designer, choose **Code view**.</span></span>

## <a name="q--a"></a><span data-ttu-id="70044-212">Dotazy a odpovědi</span><span class="sxs-lookup"><span data-stu-id="70044-212">Q & A</span></span>

#### <a name="q-what-about-url-security"></a><span data-ttu-id="70044-213">Otázka: co o adresu URL zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="70044-213">Q: What about URL security?</span></span>

<span data-ttu-id="70044-214">Odpověď: azure bezpečně generuje logiku aplikace zpětného volání adresy URL pomocí sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="70044-214">A: Azure securely generates logic app callback URLs using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="70044-215">Tento podpis projdou jako parametr dotazu a musí být ověřené před svou aplikaci logiky můžete aktivovat.</span><span class="sxs-lookup"><span data-stu-id="70044-215">This signature passes through as a query parameter and must be validated before your logic app can fire.</span></span> <span data-ttu-id="70044-216">Podpis jedinečná kombinace tajný klíč na aplikaci logiky, název aktivační události a operace, které se provádí pomocí vygeneruje Azure.</span><span class="sxs-lookup"><span data-stu-id="70044-216">Azure generates the signature using a unique combination of a secret key per logic app, the trigger name, and the operation that's performed.</span></span> <span data-ttu-id="70044-217">Pokud má uživatel přístup ke klíči tajný logiku aplikace, se proto nelze generovat platný podpis.</span><span class="sxs-lookup"><span data-stu-id="70044-217">So unless someone has access to the secret logic app key, they cannot generate a valid signature.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="70044-218">Pro produkční a zabezpečených systémů důrazně doporučujeme před volání aplikace logiky přímo z prohlížeče, protože:</span><span class="sxs-lookup"><span data-stu-id="70044-218">For production and secure systems, we strongly recommend against calling your logic app directly from the browser because:</span></span>
   > 
   > * <span data-ttu-id="70044-219">Sdílený přístupový klíč se zobrazí v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="70044-219">The shared access key appears in the URL.</span></span>
   > * <span data-ttu-id="70044-220">Nelze spravovat zabezpečené obsahu zásady z důvodu sdílených domén přes aplikaci logiky odběratele.</span><span class="sxs-lookup"><span data-stu-id="70044-220">You can't manage secure content policies due to shared domains across Logic App customers.</span></span>

#### <a name="q-can-i-configure-http-endpoints-further"></a><span data-ttu-id="70044-221">Otázka: je možné nakonfigurovat další koncové body HTTP?</span><span class="sxs-lookup"><span data-stu-id="70044-221">Q: Can I configure HTTP endpoints further?</span></span>

<span data-ttu-id="70044-222">Odpověď: Ano koncových bodů protokolu HTTP podporují pokročilejší konfigurace prostřednictvím [ **API Management**](../api-management/api-management-key-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="70044-222">A: Yes, HTTP endpoints support more advanced configuration through [**API Management**](../api-management/api-management-key-concepts.md).</span></span> <span data-ttu-id="70044-223">Tato služba také nabízí možnost můžete konzistentně spravovat všechna vaše rozhraní API, včetně aplikace logiky, nastavit vlastní názvy domén, použít další metody ověřování a informace, například:</span><span class="sxs-lookup"><span data-stu-id="70044-223">This service also offers the capability for you to consistently manage all your APIs, including logic apps, set up custom domain names, use more authentication methods, and more, for example:</span></span>

* [<span data-ttu-id="70044-224">Změnit metodu požadavku</span><span class="sxs-lookup"><span data-stu-id="70044-224">Change the request method</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [<span data-ttu-id="70044-225">Segmenty adres URL žádosti o změnu</span><span class="sxs-lookup"><span data-stu-id="70044-225">Change the URL segments of the request</span></span>](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* <span data-ttu-id="70044-226">Nastavení domény API Management do [portál Azure](https://portal.azure.com/ "portálu Azure")</span><span class="sxs-lookup"><span data-stu-id="70044-226">Set up your API Management domains in the [Azure portal](https://portal.azure.com/ "Azure portal")</span></span>
* <span data-ttu-id="70044-227">Nastavte zásady pro kontrolu pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="70044-227">Set up policy to check for Basic authentication</span></span>

#### <a name="q-what-changed-when-the-schema-migrated-from-the-december-1-2014-preview"></a><span data-ttu-id="70044-228">Otázka: co změní schéma migraci z verze preview 1 prosince 2014?</span><span class="sxs-lookup"><span data-stu-id="70044-228">Q: What changed when the schema migrated from the December 1, 2014 preview?</span></span>

<span data-ttu-id="70044-229">Odpověď: Zde je souhrn o tyto změny:</span><span class="sxs-lookup"><span data-stu-id="70044-229">A: Here's a summary about these changes:</span></span>

| <span data-ttu-id="70044-230">Preview 1 prosince 2014</span><span class="sxs-lookup"><span data-stu-id="70044-230">December 1, 2014 preview</span></span> | <span data-ttu-id="70044-231">1. června 2016</span><span class="sxs-lookup"><span data-stu-id="70044-231">June 1, 2016</span></span> |
| --- | --- |
| <span data-ttu-id="70044-232">Klikněte na tlačítko **naslouchací proces protokolu HTTP** aplikace API</span><span class="sxs-lookup"><span data-stu-id="70044-232">Click **HTTP Listener** API App</span></span> |<span data-ttu-id="70044-233">Klikněte na tlačítko **ruční aktivační událost** (žádná aplikace API povinné)</span><span class="sxs-lookup"><span data-stu-id="70044-233">Click **Manual trigger** (no API App required)</span></span> |
| <span data-ttu-id="70044-234">Naslouchací proces protokolu HTTP nastavení "*automaticky odešle odpověď*"</span><span class="sxs-lookup"><span data-stu-id="70044-234">HTTP Listener setting "*Sends response automatically*"</span></span> |<span data-ttu-id="70044-235">Buď zahrnout **odpovědi** akce nebo není v definici pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="70044-235">Either include a **Response** action or not in the workflow definition</span></span> |
| <span data-ttu-id="70044-236">Konfigurace ověřování Basic nebo OAuth</span><span class="sxs-lookup"><span data-stu-id="70044-236">Configure Basic or OAuth authentication</span></span> |<span data-ttu-id="70044-237">přes správu rozhraní API</span><span class="sxs-lookup"><span data-stu-id="70044-237">via API Management</span></span> |
| <span data-ttu-id="70044-238">Konfigurace metody HTTP</span><span class="sxs-lookup"><span data-stu-id="70044-238">Configure HTTP method</span></span> |<span data-ttu-id="70044-239">V části **zobrazit rozšířené možnosti**, zvolte metodu HTTP</span><span class="sxs-lookup"><span data-stu-id="70044-239">Under **Show advanced options**, choose an HTTP method</span></span> |
| <span data-ttu-id="70044-240">Nakonfigurujte relativní cestu</span><span class="sxs-lookup"><span data-stu-id="70044-240">Configure relative path</span></span> |<span data-ttu-id="70044-241">V části **zobrazit rozšířené možnosti**, přidejte relativní cestu</span><span class="sxs-lookup"><span data-stu-id="70044-241">Under **Show advanced options**, add a relative path</span></span> |
| <span data-ttu-id="70044-242">Referenční příchozí textu prostřednictvím`@triggerOutputs().body.Content`</span><span class="sxs-lookup"><span data-stu-id="70044-242">Reference the incoming body through `@triggerOutputs().body.Content`</span></span> |<span data-ttu-id="70044-243">Referenční dokumentace prostřednictvím`@triggerOutputs().body`</span><span class="sxs-lookup"><span data-stu-id="70044-243">Reference through `@triggerOutputs().body`</span></span> |
| <span data-ttu-id="70044-244">**Odeslání odpovědi HTTP** akce naslouchací proces protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="70044-244">**Send HTTP response** action on the HTTP Listener</span></span> |<span data-ttu-id="70044-245">Klikněte na tlačítko **odpovědět na požadavek HTTP** (žádná aplikace API povinné)</span><span class="sxs-lookup"><span data-stu-id="70044-245">Click **Respond to HTTP request** (no API App required)</span></span> |

## <a name="get-help"></a><span data-ttu-id="70044-246">Podpora</span><span class="sxs-lookup"><span data-stu-id="70044-246">Get help</span></span>

<span data-ttu-id="70044-247">Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="70044-247">To ask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="70044-248">Pokud chcete pomoci při vylepšování Azure Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Azure Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="70044-248">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="70044-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70044-249">Next steps</span></span>

* [<span data-ttu-id="70044-250">Vytváření definic aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="70044-250">Author logic app definitions</span></span>](./logic-apps-author-definitions.md)
* [<span data-ttu-id="70044-251">Zpracování chyb a výjimek</span><span class="sxs-lookup"><span data-stu-id="70044-251">Handle errors and exceptions</span></span>](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png
