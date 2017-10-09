---
title: "aaaCall, aktivaci nebo vnořit pracovních s koncovými body HTTP - Azure Logic Apps | Microsoft Docs"
description: "Nastavení toocall koncové body HTTP, aktivační události nebo vnoření pracovní postupy pro Azure Logic Apps"
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
ms.openlocfilehash: 072a314c3bff75ab7696f86bb063bb7c03c4ae89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a><span data-ttu-id="99e71-104">Volání, aktivaci nebo vnořit pracovních s koncovými body HTTP v aplikacích logiky</span><span class="sxs-lookup"><span data-stu-id="99e71-104">Call, trigger, or nest workflows with HTTP endpoints in logic apps</span></span>

<span data-ttu-id="99e71-105">Synchronní koncové body HTTP můžou nativně zpřístupnit jako triggery pro aplikace logiky, mohli aktivovat nebo volání aplikace logiky prostřednictvím adresy URL.</span><span class="sxs-lookup"><span data-stu-id="99e71-105">You can natively expose synchronous HTTP endpoints as triggers on logic apps so that you can trigger or call your logic apps through a URL.</span></span> <span data-ttu-id="99e71-106">Pracovní postupy lze také vnořit ve vašich logic apps pomocí vzor s koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="99e71-106">You can also nest workflows in your logic apps by using a pattern of callable endpoints.</span></span>

<span data-ttu-id="99e71-107">Koncové body HTTP toocreate, můžete přidat tyto triggery tak, aby aplikace logiky může přijímat příchozí požadavky:</span><span class="sxs-lookup"><span data-stu-id="99e71-107">toocreate HTTP endpoints, you can add these triggers so that your logic apps can receive incoming requests:</span></span>

* [<span data-ttu-id="99e71-108">Požadavek</span><span class="sxs-lookup"><span data-stu-id="99e71-108">Request</span></span>](../connectors/connectors-native-reqres.md)

* [<span data-ttu-id="99e71-109">Rozhraní API připojení Webhooku</span><span class="sxs-lookup"><span data-stu-id="99e71-109">API Connection Webhook</span></span>](logic-apps-workflow-actions-triggers.md#api-connection-trigger)

* [<span data-ttu-id="99e71-110">HTTP Webhook</span><span class="sxs-lookup"><span data-stu-id="99e71-110">HTTP Webhook</span></span>](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > <span data-ttu-id="99e71-111">I když příklady použití hello **požadavku** aktivační událost, můžete použít některou z hello uvedené aktivace protokolu HTTP a všechny zásady stejně jako vztahují toohello jiné typy aktivační události.</span><span class="sxs-lookup"><span data-stu-id="99e71-111">Although our examples use hello **Request** trigger, you can use any of hello listed HTTP triggers, and all principles identically apply toohello other trigger types.</span></span>

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a><span data-ttu-id="99e71-112">Nastavit koncový bod HTTP pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="99e71-112">Set up an HTTP endpoint for your logic app</span></span>

<span data-ttu-id="99e71-113">toocreate koncový bod protokolu HTTP, přidejte aktivační událost, která může přijímat příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="99e71-113">toocreate an HTTP endpoint, add a trigger that can receive incoming requests.</span></span>

1. <span data-ttu-id="99e71-114">Přihlaste se toohello [portál Azure](https://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="99e71-114">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="99e71-115">Přejděte aplikace logiky tooyour a otevřete návrhář aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="99e71-115">Go tooyour logic app, and open Logic App Designer.</span></span>

2. <span data-ttu-id="99e71-116">Přidejte aktivační událost, která umožní aplikaci logiky přijímat příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="99e71-116">Add a trigger that lets your logic app receive incoming requests.</span></span> <span data-ttu-id="99e71-117">Například přidejte hello **požadavku** aplikace logiky tooyour aktivační události.</span><span class="sxs-lookup"><span data-stu-id="99e71-117">For example, add hello **Request** trigger tooyour logic app.</span></span>

3.  <span data-ttu-id="99e71-118">V části **požadavku schématu JSON textu**, můžete volitelně zadat schématu JSON pro datové hello (data), které předpokládáte tooreceive hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="99e71-118">Under **Request Body JSON Schema**, you can optionally enter a JSON schema for hello payload (data) that you expect hello trigger tooreceive.</span></span>

    <span data-ttu-id="99e71-119">Návrhář Hello používá toto schéma pro generování tokenů, aplikace logiky můžete tooconsume, analýzy a předejte data z hello aktivační události prostřednictvím pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="99e71-119">hello designer uses this schema for generating tokens that your logic app can use tooconsume, parse, and pass data from hello trigger through your workflow.</span></span> 
    <span data-ttu-id="99e71-120">Další informace o [tokeny vygenerovat z JSON schémata](#generated-tokens).</span><span class="sxs-lookup"><span data-stu-id="99e71-120">More about [tokens generated from JSON schemas](#generated-tokens).</span></span>

    <span data-ttu-id="99e71-121">V tomto příkladu zadejte schéma hello v Návrháři hello zobrazeny:</span><span class="sxs-lookup"><span data-stu-id="99e71-121">For this example, enter hello schema shown in hello designer:</span></span>

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

    ![Přidat akci požadavku hello][1]

    > [!TIP]
    > 
    > <span data-ttu-id="99e71-123">Schéma pro ukázku datové části JSON můžete vygenerovat z nástroje, jako je [jsonschema.net](http://jsonschema.net/), nebo v hello **požadavku** aktivační události tak, že zvolíte **použití ukázkové datové části toogenerate schématu**.</span><span class="sxs-lookup"><span data-stu-id="99e71-123">You can generate a schema for a sample JSON payload from a tool like [jsonschema.net](http://jsonschema.net/), or in hello **Request** trigger by choosing **Use sample payload toogenerate schema**.</span></span> 
    > <span data-ttu-id="99e71-124">Zadejte vaše datová část ukázky a vyberte **provádí**.</span><span class="sxs-lookup"><span data-stu-id="99e71-124">Enter your sample payload, and choose **Done**.</span></span>

    <span data-ttu-id="99e71-125">Například tato ukázková datová část:</span><span class="sxs-lookup"><span data-stu-id="99e71-125">For example, this sample payload:</span></span>

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    <span data-ttu-id="99e71-126">generuje toto schéma:</span><span class="sxs-lookup"><span data-stu-id="99e71-126">generates this schema:</span></span>

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

4.  <span data-ttu-id="99e71-127">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="99e71-127">Save your logic app.</span></span> <span data-ttu-id="99e71-128">V části **HTTP POST toothis URL**, nyní byste měli najít adresu URL generovaného zpětného volání, jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="99e71-128">Under **HTTP POST toothis URL**, you should now find a generated callback URL, like this example:</span></span>

    ![Adresa URL generovaného zpětného volání pro koncový bod](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    <span data-ttu-id="99e71-130">Tato adresa URL obsahuje klíč sdíleného přístupového podpisu (SAS) v hello parametry dotazu, které se používají pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="99e71-130">This URL contains a Shared Access Signature (SAS) key in hello query parameters that are used for authentication.</span></span> 
    <span data-ttu-id="99e71-131">Můžete také získat adresu URL koncového bodu hello HTTP z vaší přehledu aplikace logiky v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="99e71-131">You can also get hello HTTP endpoint URL from your logic app overview in hello Azure portal.</span></span> <span data-ttu-id="99e71-132">V části **historie aktivační událost**, vyberte aktivační událost:</span><span class="sxs-lookup"><span data-stu-id="99e71-132">Under **Trigger History**, select your trigger:</span></span>

    ![Získat adresu URL koncového bodu protokolu HTTP z portálu Azure][2]

    <span data-ttu-id="99e71-134">Nebo můžete získat adresu URL hello tím, že toto volání:</span><span class="sxs-lookup"><span data-stu-id="99e71-134">Or you can get hello URL by making this call:</span></span>

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-hello-http-method-for-your-trigger"></a><span data-ttu-id="99e71-135">Změnit metodu HTTP hello aktivační události</span><span class="sxs-lookup"><span data-stu-id="99e71-135">Change hello HTTP method for your trigger</span></span>

<span data-ttu-id="99e71-136">Ve výchozím nastavení, hello **požadavku** aktivační událost očekává požadavek HTTP POST, ale můžete použít jinou metodu HTTP.</span><span class="sxs-lookup"><span data-stu-id="99e71-136">By default, hello **Request** trigger expects an HTTP POST request, but you can use a different HTTP method.</span></span> 

> [!NOTE]
> <span data-ttu-id="99e71-137">Můžete zadat typ pouze jednu metodu.</span><span class="sxs-lookup"><span data-stu-id="99e71-137">You can specify only one method type.</span></span>

1. <span data-ttu-id="99e71-138">Na vaše **požadavku** aktivovat, zvolte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="99e71-138">On your **Request** trigger, choose **Show advanced options**.</span></span>

2. <span data-ttu-id="99e71-139">Otevřete hello **metoda** seznamu.</span><span class="sxs-lookup"><span data-stu-id="99e71-139">Open hello **Method** list.</span></span> <span data-ttu-id="99e71-140">V tomto příkladu vyberte **získat** , aby bylo později otestovat adresu URL vašeho koncového bodu protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="99e71-140">For this example, select **GET** so that you can test your HTTP endpoint's URL later.</span></span>

    > [!NOTE]
    > <span data-ttu-id="99e71-141">Můžete vybrat jinou metodu HTTP, nebo zadat vlastní metodu pro aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="99e71-141">You can select any other HTTP method, or specify a custom method for your own logic app.</span></span>

    ![Změnit metodu HTTP](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a><span data-ttu-id="99e71-143">Přijměte parametry prostřednictvím vaše adresa URL koncového bodu protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="99e71-143">Accept parameters through your HTTP endpoint URL</span></span>

<span data-ttu-id="99e71-144">Když chcete parametry tooaccept koncový bod adresy URL protokolu HTTP, přizpůsobit vaší aktivační událost relativní cestu.</span><span class="sxs-lookup"><span data-stu-id="99e71-144">When you want your HTTP endpoint URL tooaccept parameters, customize your trigger's relative path.</span></span>

1. <span data-ttu-id="99e71-145">Na vaše **požadavku** aktivovat, zvolte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="99e71-145">On your **Request** trigger, choose **Show advanced options**.</span></span> 

2. <span data-ttu-id="99e71-146">V části **metoda**, zadejte metodu hello HTTP, které chcete toouse váš požadavek.</span><span class="sxs-lookup"><span data-stu-id="99e71-146">Under **Method**, specify hello HTTP method that you want your request toouse.</span></span> <span data-ttu-id="99e71-147">V tomto příkladu vyberte hello **získat** metodu, pokud jste tak ještě neučinili, tak, aby bylo možné otestovat adresu URL vašeho koncového bodu protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="99e71-147">For this example, select hello **GET** method, if you haven't already, so that you can test your HTTP endpoint's URL.</span></span>

      > [!NOTE]
      > <span data-ttu-id="99e71-148">Když zadáte relativní cesta aktivační události, musíte zadat metodu HTTP také explicitně aktivační události.</span><span class="sxs-lookup"><span data-stu-id="99e71-148">When you specify a relative path for your trigger, you must also explicitly specify an HTTP method for your trigger.</span></span>

3. <span data-ttu-id="99e71-149">V části **relativní cestu**, zadejte relativní cestu hello hello parametru, který by měl přijímat vaše adresa URL, například `customers/{customerID}`.</span><span class="sxs-lookup"><span data-stu-id="99e71-149">Under **Relative path**, specify hello relative path for hello parameter that your URL should accept, for example, `customers/{customerID}`.</span></span>

    ![Zadejte metodu HTTP hello a relativní cestu pro parametr](./media/logic-apps-http-endpoint/relativeurl.png)

4. <span data-ttu-id="99e71-151">toouse hello parametr, přidejte **odpovědi** aplikace logiky tooyour akce.</span><span class="sxs-lookup"><span data-stu-id="99e71-151">toouse hello parameter, add a **Response** action tooyour logic app.</span></span> <span data-ttu-id="99e71-152">(V části aktivační událost, zvolte **nový krok** > **přidat akci** > **odpovědi**)</span><span class="sxs-lookup"><span data-stu-id="99e71-152">(Under your trigger, choose **New step** > **Add an action** > **Response**)</span></span> 

5. <span data-ttu-id="99e71-153">Do odpovědi **textu**, zahrnout hello token pro hello parametr, který jste zadali v relativní cestě vaší aktivační události.</span><span class="sxs-lookup"><span data-stu-id="99e71-153">In your response's **Body**, include hello token for hello parameter that you specified in your trigger's relative path.</span></span>

    <span data-ttu-id="99e71-154">Například tooreturn `Hello {customerID}`, aktualizovat vaši odpověď **textu** s `Hello {customerID token}`.</span><span class="sxs-lookup"><span data-stu-id="99e71-154">For example, tooreturn `Hello {customerID}`, update your response's **Body** with `Hello {customerID token}`.</span></span> 
    <span data-ttu-id="99e71-155">Hello dynamického obsahu seznamu by měl zobrazit a zobrazit hello `customerID` tokenu pro tooselect je.</span><span class="sxs-lookup"><span data-stu-id="99e71-155">hello dynamic content list should appear and show hello `customerID` token for you tooselect.</span></span>

    ![Přidat parametr tooresponse textu](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    <span data-ttu-id="99e71-157">Vaše **textu** by měl vypadat jako tento příklad:</span><span class="sxs-lookup"><span data-stu-id="99e71-157">Your **Body** should look like this example:</span></span>

    ![Text odpovědi s parametrem](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. <span data-ttu-id="99e71-159">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="99e71-159">Save your logic app.</span></span> 

    <span data-ttu-id="99e71-160">Vaše adresa URL koncového bodu protokolu HTTP nyní zahrnuje hello relativní cesta, například:</span><span class="sxs-lookup"><span data-stu-id="99e71-160">Your HTTP endpoint URL now includes hello relative path, for example:</span></span> 

    <span data-ttu-id="99e71-161">https & # 58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...</span><span class="sxs-lookup"><span data-stu-id="99e71-161">https&#58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...</span></span>

7. <span data-ttu-id="99e71-162">tootest koncový bod protokolu HTTP, kopírování a vložení hello aktualizované adresu URL do jiného okna prohlížeče, ale nahraďte `{customerID}` s `123456`, a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="99e71-162">tootest your HTTP endpoint, copy and paste hello updated URL into another browser window, but replace `{customerID}` with `123456`, and press Enter.</span></span>

    <span data-ttu-id="99e71-163">Váš prohlížeč by měl zobrazit tento text:</span><span class="sxs-lookup"><span data-stu-id="99e71-163">Your browser should show this text:</span></span> 

    `Hello 123456`

<a name="generated-tokens"></a>
### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a><span data-ttu-id="99e71-164">Tokeny vygenerovat z JSON schémata pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="99e71-164">Tokens generated from JSON schemas for your logic app</span></span>

<span data-ttu-id="99e71-165">Když zadáte schématu JSON ve vaší **požadavku** aktivovat, hello logiku aplikace Návrhář generuje tokeny pro vlastnosti v tomto schématu.</span><span class="sxs-lookup"><span data-stu-id="99e71-165">When you provide a JSON schema in your **Request** trigger, hello Logic App Designer generates tokens for properties in that schema.</span></span> <span data-ttu-id="99e71-166">Pak můžete tyto tokeny pro předávání dat prostřednictvím pracovní postup aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="99e71-166">You can then use those tokens for passing data through your logic app workflow.</span></span>

<span data-ttu-id="99e71-167">Pro tento příklad, pokud přidáte hello `title` a `name` vlastnosti schématu JSON tooyour, jejich tokeny jsou nyní k dispozici toouse v dalších krocích pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="99e71-167">For this example, if you add hello `title` and `name` properties tooyour JSON schema, their tokens are now available toouse in later workflow steps.</span></span> 

<span data-ttu-id="99e71-168">Tady je hello dokončení schématu JSON:</span><span class="sxs-lookup"><span data-stu-id="99e71-168">Here is hello complete JSON schema:</span></span>

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

## <a name="create-nested-workflows-for-logic-apps"></a><span data-ttu-id="99e71-169">Vytvoření vnořené pracovní postupy pro logic apps</span><span class="sxs-lookup"><span data-stu-id="99e71-169">Create nested workflows for logic apps</span></span>

<span data-ttu-id="99e71-170">Pracovní postupy lze vnořit ve vaší aplikaci logiky můžete přidat další logiku aplikace, které může přijímat požadavky.</span><span class="sxs-lookup"><span data-stu-id="99e71-170">You can nest workflows in your logic app by adding other logic apps that can receive requests.</span></span> <span data-ttu-id="99e71-171">tooinclude těchto aplikací logiky, přidejte hello **Azure Logic Apps – zvolte pracovní postup Logic Apps** tooyour akce aktivační události.</span><span class="sxs-lookup"><span data-stu-id="99e71-171">tooinclude these logic apps, add hello **Azure Logic Apps - Choose a Logic Apps workflow** action tooyour trigger.</span></span> <span data-ttu-id="99e71-172">Pak můžete vybrat z oprávněné logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="99e71-172">You can then select from eligible logic apps.</span></span>

![Přidat další aplikace logiky](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a><span data-ttu-id="99e71-174">Volání nebo aktivovat aplikace logiky prostřednictvím koncových bodů protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="99e71-174">Call or trigger logic apps through HTTP endpoints</span></span>

<span data-ttu-id="99e71-175">Jakmile vytvoříte koncový bod protokolu HTTP, můžete aktivovat svou aplikaci logiky prostřednictvím `POST` metoda toohello úplnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="99e71-175">After you create your HTTP endpoint, you can trigger your logic app through a `POST` method toohello full URL.</span></span> <span data-ttu-id="99e71-176">Logiku aplikace mají integrovanou podporu pro koncové body přímý přístup.</span><span class="sxs-lookup"><span data-stu-id="99e71-176">Logic apps have built-in support for direct-access endpoints.</span></span>

## <a name="reference-content-from-an-incoming-request"></a><span data-ttu-id="99e71-177">Odkaz na obsah z příchozího požadavku</span><span class="sxs-lookup"><span data-stu-id="99e71-177">Reference content from an incoming request</span></span>

<span data-ttu-id="99e71-178">Pokud je typ obsahu hello je `application/json`, vlastnosti, můžete odkazovat z příchozího požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="99e71-178">If hello content's type is `application/json`, you can reference properties from hello incoming request.</span></span> <span data-ttu-id="99e71-179">Jinak hodnota obsahu považovány za jednu jednotku binární, můžete předat tooother rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="99e71-179">Otherwise, content is treated as a single binary unit that you can pass tooother APIs.</span></span> <span data-ttu-id="99e71-180">Tento obsah uvnitř pracovního postupu hello tooreference, je nutné převést obsah.</span><span class="sxs-lookup"><span data-stu-id="99e71-180">tooreference this content inside hello workflow, you must convert that content.</span></span> <span data-ttu-id="99e71-181">Například pokud předáte `application/xml` obsahu, můžete použít `@xpath()` pro extrakci XPath nebo `@json()` pro převod XML tooJSON.</span><span class="sxs-lookup"><span data-stu-id="99e71-181">For example, if you pass `application/xml` content, you can use `@xpath()` for an XPath extraction, or `@json()` for converting XML tooJSON.</span></span> <span data-ttu-id="99e71-182">Další informace o [práce s typy obsahu](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="99e71-182">Learn about [working with content types](../logic-apps/logic-apps-content-type.md).</span></span>

<span data-ttu-id="99e71-183">tooget hello výstup z příchozího požadavku, můžete použít hello `@triggerOutputs()` funkce.</span><span class="sxs-lookup"><span data-stu-id="99e71-183">tooget hello output from an incoming request, you can use hello `@triggerOutputs()` function.</span></span> <span data-ttu-id="99e71-184">výstup Hello může vypadat jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="99e71-184">hello output might look like this example:</span></span>

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

<span data-ttu-id="99e71-185">tooaccess hello `body` vlastnost můžete konkrétně použít hello `@triggerBody()` zástupce.</span><span class="sxs-lookup"><span data-stu-id="99e71-185">tooaccess hello `body` property specifically, you can use hello `@triggerBody()` shortcut.</span></span> 

## <a name="respond-toorequests"></a><span data-ttu-id="99e71-186">Odpověď toorequests</span><span class="sxs-lookup"><span data-stu-id="99e71-186">Respond toorequests</span></span>

<span data-ttu-id="99e71-187">Můžete chtít toorespond toocertain požadavků, které spustí aplikaci logiky vrácením obsahu toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="99e71-187">You might want toorespond toocertain requests that start a logic app by returning content toohello caller.</span></span> <span data-ttu-id="99e71-188">tooconstruct hello stavový kód, záhlaví a text na vaši odpověď, můžete použít hello **odpovědi** akce.</span><span class="sxs-lookup"><span data-stu-id="99e71-188">tooconstruct hello status code, header, and body for your response, you can use hello **Response** action.</span></span> <span data-ttu-id="99e71-189">Tato akce může vyskytovat kdekoli v aplikaci logiky, nikoli pouze na konci hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="99e71-189">This action can appear anywhere in your logic app, not just at hello end of your workflow.</span></span>

> [!NOTE] 
> <span data-ttu-id="99e71-190">Pokud svou aplikaci logiky neobsahuje **odpovědi**, odpoví koncový bod hello HTTP *okamžitě* s **202 platných** stavu.</span><span class="sxs-lookup"><span data-stu-id="99e71-190">If your logic app doesn't include a **Response**, hello HTTP endpoint responds *immediately* with a **202 Accepted** status.</span></span> <span data-ttu-id="99e71-191">Navíc se pro hello původní žádost tooget hello odpověď, a všechny kroky potřebné k odpovědi hello musí dokončit hello [časový limit požadavku](./logic-apps-limits-and-config.md) Pokud volání hello pracovního postupu jako vnořené logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="99e71-191">Also, for hello original request tooget hello response, all steps required for hello response must finish within hello [request timeout limit](./logic-apps-limits-and-config.md) unless you call hello workflow as a nested logic app.</span></span> <span data-ttu-id="99e71-192">V případě žádná odpověď v rámci tohoto limitu hello příchozího požadavku vyprší časový limit a obdrží odpověď hello HTTP **408 časový limit klienta**.</span><span class="sxs-lookup"><span data-stu-id="99e71-192">If no response happens within this limit, hello incoming request times out and receives hello HTTP response **408 Client timeout**.</span></span> <span data-ttu-id="99e71-193">Pro vnořené logic apps pokračuje aplikace logiky nadřazené hello toowait pro odpověď, dokud nebude dokončena, bez ohledu na to, jak dlouho se vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="99e71-193">For nested logic apps, hello parent logic app continues toowait for a response until completed, regardless of how much time is required.</span></span>

### <a name="construct-hello-response"></a><span data-ttu-id="99e71-194">Konstrukce hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="99e71-194">Construct hello response</span></span>

<span data-ttu-id="99e71-195">Můžete zahrnout více než jedno záhlaví a libovolný typ obsahu odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="99e71-195">You can include more than one header and any type of content in hello response body.</span></span> <span data-ttu-id="99e71-196">V našem příkladu odpovědi hello záhlaví Určuje, že hello odpověď má typ obsahu `application/json`.</span><span class="sxs-lookup"><span data-stu-id="99e71-196">In our example response, hello header specifies that hello response has content type `application/json`.</span></span> <span data-ttu-id="99e71-197">a hello text obsahuje `title` a `name`, podle schématu JSON hello aktualizované dříve hello **požadavku** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="99e71-197">and hello body contains `title` and `name`, based on hello JSON schema updated previously for hello **Request** trigger.</span></span>

![Akce odpovědi HTTP][3]

<span data-ttu-id="99e71-199">Odpovědi mít tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="99e71-199">Responses have these properties:</span></span>

| <span data-ttu-id="99e71-200">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="99e71-200">Property</span></span> | <span data-ttu-id="99e71-201">Popis</span><span class="sxs-lookup"><span data-stu-id="99e71-201">Description</span></span> |
| --- | --- |
| <span data-ttu-id="99e71-202">statusCode</span><span class="sxs-lookup"><span data-stu-id="99e71-202">statusCode</span></span> |<span data-ttu-id="99e71-203">Určuje kód stavu hello HTTP pro odpovídá toohello příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="99e71-203">Specifies hello HTTP status code for responding toohello incoming request.</span></span> <span data-ttu-id="99e71-204">Tento kód může být jakýkoli platný stavový kód, který začíná 2xx, 4xx nebo 5xx.</span><span class="sxs-lookup"><span data-stu-id="99e71-204">This code can be any valid status code that starts with 2xx, 4xx, or 5xx.</span></span> <span data-ttu-id="99e71-205">Stavové kódy 3xx však nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="99e71-205">However, 3xx status codes are not permitted.</span></span> |
| <span data-ttu-id="99e71-206">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="99e71-206">headers</span></span> |<span data-ttu-id="99e71-207">Definuje libovolný počet tooinclude hlavičky odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="99e71-207">Defines any number of headers tooinclude in hello response.</span></span> |
| <span data-ttu-id="99e71-208">Text</span><span class="sxs-lookup"><span data-stu-id="99e71-208">body</span></span> |<span data-ttu-id="99e71-209">Určuje objekt textu, který může být řetězec, objekt JSON nebo i binární obsah na něj odkazovat z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="99e71-209">Specifies a body object that can be a string, a JSON object, or even binary content referenced from a previous step.</span></span> |

<span data-ttu-id="99e71-210">Tady je co schématu JSON hello vypadá jako teď pro hello **odpovědi** akce:</span><span class="sxs-lookup"><span data-stu-id="99e71-210">Here's what hello JSON schema looks like now for hello **Response** action:</span></span>

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
> <span data-ttu-id="99e71-211">Zvolte tooview hello dokončení JSON definici aplikace logiky na hello návrhář aplikace na základě logiky, **Code zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="99e71-211">tooview hello complete JSON definition for your logic app, on hello Logic App Designer, choose **Code view**.</span></span>

## <a name="q--a"></a><span data-ttu-id="99e71-212">Dotazy a odpovědi</span><span class="sxs-lookup"><span data-stu-id="99e71-212">Q & A</span></span>

#### <a name="q-what-about-url-security"></a><span data-ttu-id="99e71-213">Otázka: co o adresu URL zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="99e71-213">Q: What about URL security?</span></span>

<span data-ttu-id="99e71-214">Odpověď: azure bezpečně generuje logiku aplikace zpětného volání adresy URL pomocí sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="99e71-214">A: Azure securely generates logic app callback URLs using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="99e71-215">Tento podpis projdou jako parametr dotazu a musí být ověřené před svou aplikaci logiky můžete aktivovat.</span><span class="sxs-lookup"><span data-stu-id="99e71-215">This signature passes through as a query parameter and must be validated before your logic app can fire.</span></span> <span data-ttu-id="99e71-216">Podpis hello jedinečná kombinace tajný klíč na aplikaci logiky, název aktivační události hello a hello operace, které se provádí pomocí vygeneruje Azure.</span><span class="sxs-lookup"><span data-stu-id="99e71-216">Azure generates hello signature using a unique combination of a secret key per logic app, hello trigger name, and hello operation that's performed.</span></span> <span data-ttu-id="99e71-217">Pokud má uživatel přístup toohello tajný logiku aplikace klíče, se proto nelze generovat platný podpis.</span><span class="sxs-lookup"><span data-stu-id="99e71-217">So unless someone has access toohello secret logic app key, they cannot generate a valid signature.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="99e71-218">Pro produkční a zabezpečených systémů důrazně doporučujeme před volání přímo z prohlížeče hello aplikace logiky, protože:</span><span class="sxs-lookup"><span data-stu-id="99e71-218">For production and secure systems, we strongly recommend against calling your logic app directly from hello browser because:</span></span>
   > 
   > * <span data-ttu-id="99e71-219">Hello sdílený přístupový klíč se zobrazí v adrese URL hello.</span><span class="sxs-lookup"><span data-stu-id="99e71-219">hello shared access key appears in hello URL.</span></span>
   > * <span data-ttu-id="99e71-220">Nelze spravovat zabezpečení obsahu zásady z důvodu tooshared domén přes aplikaci logiky odběratele.</span><span class="sxs-lookup"><span data-stu-id="99e71-220">You can't manage secure content policies due tooshared domains across Logic App customers.</span></span>

#### <a name="q-can-i-configure-http-endpoints-further"></a><span data-ttu-id="99e71-221">Otázka: je možné nakonfigurovat další koncové body HTTP?</span><span class="sxs-lookup"><span data-stu-id="99e71-221">Q: Can I configure HTTP endpoints further?</span></span>

<span data-ttu-id="99e71-222">Odpověď: Ano koncových bodů protokolu HTTP podporují pokročilejší konfigurace prostřednictvím [ **API Management**](../api-management/api-management-key-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="99e71-222">A: Yes, HTTP endpoints support more advanced configuration through [**API Management**](../api-management/api-management-key-concepts.md).</span></span> <span data-ttu-id="99e71-223">Tato služba také nabízí možnost hello pro tooconsistently můžete spravovat všechna vaše rozhraní API, včetně aplikace logiky, nastavit vlastní názvy domén, použít další metody ověřování a informace, například:</span><span class="sxs-lookup"><span data-stu-id="99e71-223">This service also offers hello capability for you tooconsistently manage all your APIs, including logic apps, set up custom domain names, use more authentication methods, and more, for example:</span></span>

* [<span data-ttu-id="99e71-224">Metoda žádosti o změnu hello</span><span class="sxs-lookup"><span data-stu-id="99e71-224">Change hello request method</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [<span data-ttu-id="99e71-225">Segmenty adres URL hello hello žádosti o změnu</span><span class="sxs-lookup"><span data-stu-id="99e71-225">Change hello URL segments of hello request</span></span>](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* <span data-ttu-id="99e71-226">Nastavení domény API Management do hello [portál Azure](https://portal.azure.com/ "portálu Azure")</span><span class="sxs-lookup"><span data-stu-id="99e71-226">Set up your API Management domains in hello [Azure portal](https://portal.azure.com/ "Azure portal")</span></span>
* <span data-ttu-id="99e71-227">Nastavení zásad toocheck pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="99e71-227">Set up policy toocheck for Basic authentication</span></span>

#### <a name="q-what-changed-when-hello-schema-migrated-from-hello-december-1-2014-preview"></a><span data-ttu-id="99e71-228">Otázka: co se změnilo při hello schématu migraci z verze preview 1 prosince 2014 hello?</span><span class="sxs-lookup"><span data-stu-id="99e71-228">Q: What changed when hello schema migrated from hello December 1, 2014 preview?</span></span>

<span data-ttu-id="99e71-229">Odpověď: Zde je souhrn o tyto změny:</span><span class="sxs-lookup"><span data-stu-id="99e71-229">A: Here's a summary about these changes:</span></span>

| <span data-ttu-id="99e71-230">Preview 1 prosince 2014</span><span class="sxs-lookup"><span data-stu-id="99e71-230">December 1, 2014 preview</span></span> | <span data-ttu-id="99e71-231">1. června 2016</span><span class="sxs-lookup"><span data-stu-id="99e71-231">June 1, 2016</span></span> |
| --- | --- |
| <span data-ttu-id="99e71-232">Klikněte na tlačítko **naslouchací proces protokolu HTTP** aplikace API</span><span class="sxs-lookup"><span data-stu-id="99e71-232">Click **HTTP Listener** API App</span></span> |<span data-ttu-id="99e71-233">Klikněte na tlačítko **ruční aktivační událost** (žádná aplikace API povinné)</span><span class="sxs-lookup"><span data-stu-id="99e71-233">Click **Manual trigger** (no API App required)</span></span> |
| <span data-ttu-id="99e71-234">Naslouchací proces protokolu HTTP nastavení "*automaticky odešle odpověď*"</span><span class="sxs-lookup"><span data-stu-id="99e71-234">HTTP Listener setting "*Sends response automatically*"</span></span> |<span data-ttu-id="99e71-235">Buď zahrnout **odpovědi** akce nebo není v definici pracovního postupu hello</span><span class="sxs-lookup"><span data-stu-id="99e71-235">Either include a **Response** action or not in hello workflow definition</span></span> |
| <span data-ttu-id="99e71-236">Konfigurace ověřování Basic nebo OAuth</span><span class="sxs-lookup"><span data-stu-id="99e71-236">Configure Basic or OAuth authentication</span></span> |<span data-ttu-id="99e71-237">přes správu rozhraní API</span><span class="sxs-lookup"><span data-stu-id="99e71-237">via API Management</span></span> |
| <span data-ttu-id="99e71-238">Konfigurace metody HTTP</span><span class="sxs-lookup"><span data-stu-id="99e71-238">Configure HTTP method</span></span> |<span data-ttu-id="99e71-239">V části **zobrazit rozšířené možnosti**, zvolte metodu HTTP</span><span class="sxs-lookup"><span data-stu-id="99e71-239">Under **Show advanced options**, choose an HTTP method</span></span> |
| <span data-ttu-id="99e71-240">Nakonfigurujte relativní cestu</span><span class="sxs-lookup"><span data-stu-id="99e71-240">Configure relative path</span></span> |<span data-ttu-id="99e71-241">V části **zobrazit rozšířené možnosti**, přidejte relativní cestu</span><span class="sxs-lookup"><span data-stu-id="99e71-241">Under **Show advanced options**, add a relative path</span></span> |
| <span data-ttu-id="99e71-242">Referenční dokumentace příchozí textu hello prostřednictvím`@triggerOutputs().body.Content`</span><span class="sxs-lookup"><span data-stu-id="99e71-242">Reference hello incoming body through `@triggerOutputs().body.Content`</span></span> |<span data-ttu-id="99e71-243">Referenční dokumentace prostřednictvím`@triggerOutputs().body`</span><span class="sxs-lookup"><span data-stu-id="99e71-243">Reference through `@triggerOutputs().body`</span></span> |
| <span data-ttu-id="99e71-244">**Odeslání odpovědi HTTP** akce hello naslouchací proces protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="99e71-244">**Send HTTP response** action on hello HTTP Listener</span></span> |<span data-ttu-id="99e71-245">Klikněte na tlačítko **reakce tooHTTP požadavku** (žádná aplikace API povinné)</span><span class="sxs-lookup"><span data-stu-id="99e71-245">Click **Respond tooHTTP request** (no API App required)</span></span> |

## <a name="get-help"></a><span data-ttu-id="99e71-246">Podpora</span><span class="sxs-lookup"><span data-stu-id="99e71-246">Get help</span></span>

<span data-ttu-id="99e71-247">tooask otázky, odpovědi na otázky a zjistěte, jaké další Azure Logic Apps uživatelé dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="99e71-247">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="99e71-248">toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="99e71-248">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="99e71-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99e71-249">Next steps</span></span>

* [<span data-ttu-id="99e71-250">Vytváření definic aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="99e71-250">Author logic app definitions</span></span>](./logic-apps-author-definitions.md)
* [<span data-ttu-id="99e71-251">Zpracování chyb a výjimek</span><span class="sxs-lookup"><span data-stu-id="99e71-251">Handle errors and exceptions</span></span>](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png
