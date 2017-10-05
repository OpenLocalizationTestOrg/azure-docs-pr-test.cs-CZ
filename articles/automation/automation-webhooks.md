---
title: "Počínaje webhook, jehož runbook služby Azure Automation | Microsoft Docs"
description: "Webhook, která umožňuje klientovi spuštění sady runbook ve službě Azure Automation z volání protokolu HTTP.  Tento článek popisuje, jak vytvořit webhook, jehož a postup volání jednoho spuštění runbooku."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9b20237c-a593-4299-bbdc-35c47ee9e55d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: 6c65427fcd18e41a90dfb872aa9525f758b17b87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a><span data-ttu-id="a70eb-104">Počínaje webhook, jehož runbook služby automatizace Azure</span><span class="sxs-lookup"><span data-stu-id="a70eb-104">Starting an Azure Automation runbook with a webhook</span></span>
<span data-ttu-id="a70eb-105">A *webhooku* umožňuje spustit konkrétní runbook ve službě Azure Automation prostřednictvím jedné žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="a70eb-105">A *webhook* allows you to start a particular runbook in Azure Automation through a single HTTP request.</span></span> <span data-ttu-id="a70eb-106">To umožňuje externích služeb, jako je například Visual Studio Team Services, GitHub, analýzy protokolů Microsoft Operations Management Suite nebo vlastních aplikací ke spouštění sad runbook bez implementace úplné řešení pomocí rozhraní API služby Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a70eb-106">This allows external services such as Visual Studio Team Services, GitHub, Microsoft Operations Management Suite Log Analytics, or custom applications to start runbooks without implementing a full solution using the Azure Automation API.</span></span>  
<span data-ttu-id="a70eb-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span><span class="sxs-lookup"><span data-stu-id="a70eb-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span></span>

<span data-ttu-id="a70eb-108">Můžete porovnat webhooky k dalším metodám spuštění sady runbook [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="a70eb-108">You can compare webhooks to other methods of starting a runbook in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md)</span></span>

## <a name="details-of-a-webhook"></a><span data-ttu-id="a70eb-109">Podrobnosti o webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="a70eb-109">Details of a webhook</span></span>
<span data-ttu-id="a70eb-110">Následující tabulka popisuje vlastnosti, které je nutné nakonfigurovat pro webhook, jehož.</span><span class="sxs-lookup"><span data-stu-id="a70eb-110">The following table describes the properties that you must configure for a webhook.</span></span>

| <span data-ttu-id="a70eb-111">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a70eb-111">Property</span></span> | <span data-ttu-id="a70eb-112">Popis</span><span class="sxs-lookup"><span data-stu-id="a70eb-112">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a70eb-113">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="a70eb-113">Name</span></span> |<span data-ttu-id="a70eb-114">Můžete zadat libovolný název, který chcete použít pro webhook, jehož vzhledem k tomu, že to není vystavený klienta.</span><span class="sxs-lookup"><span data-stu-id="a70eb-114">You can provide any name you want for a webhook since this is not exposed to the client.</span></span>  <span data-ttu-id="a70eb-115">Používá se pouze pro vás k identifikaci sady runbook ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a70eb-115">It is only used for you to identify the runbook in Azure Automation.</span></span> <br>  <span data-ttu-id="a70eb-116">Jako osvědčený postup musíte získat webhooku název související klientovi, který bude používat.</span><span class="sxs-lookup"><span data-stu-id="a70eb-116">As a best practice, you should give the webhook a name related to the client that will use it.</span></span> |
| <span data-ttu-id="a70eb-117">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="a70eb-117">URL</span></span> |<span data-ttu-id="a70eb-118">Adresa URL webhooku je jedinečnou adresu, která volá klienta pomocí metody POST protokolu HTTP pro spuštění sady runbook propojené s webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-118">The URL of the webhook is the unique address that a client calls with an HTTP POST to start the runbook linked to the webhook.</span></span>  <span data-ttu-id="a70eb-119">Generuje se automaticky při vytvoření webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-119">It is automatically generated when you create the webhook.</span></span>  <span data-ttu-id="a70eb-120">Nelze zadat vlastní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a70eb-120">You cannot specify a custom URL.</span></span> <br> <br>  <span data-ttu-id="a70eb-121">Adresa URL obsahuje token zabezpečení, který umožňuje sady runbook vyvolat systému třetích stran se žádné další ověřování.</span><span class="sxs-lookup"><span data-stu-id="a70eb-121">The URL contains a security token that allows the runbook to be invoked by a third party system with no further authentication.</span></span> <span data-ttu-id="a70eb-122">Z tohoto důvodu by zpracovávat jako heslo.</span><span class="sxs-lookup"><span data-stu-id="a70eb-122">For this reason, it should be treated like a password.</span></span>  <span data-ttu-id="a70eb-123">Z bezpečnostních důvodů můžete jenom zobrazit adresu URL na portálu Azure v době, kdy je vytvoření webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-123">For security reasons, you can only view the URL in the Azure portal at the time the webhook is created.</span></span> <span data-ttu-id="a70eb-124">Upozorňujeme ale, adresu URL na bezpečné místo pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="a70eb-124">You should note the URL in a secure location for future use.</span></span> |
| <span data-ttu-id="a70eb-125">Datum vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="a70eb-125">Expiration date</span></span> |<span data-ttu-id="a70eb-126">Stejně jako certifikát má každý webhooku datum vypršení platnosti, po kterém již slouží.</span><span class="sxs-lookup"><span data-stu-id="a70eb-126">Like a certificate, each webhook has an expiration date at which time it can no longer be used.</span></span>  <span data-ttu-id="a70eb-127">Po vytvoření webhooku můžete upravit toto datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="a70eb-127">This expiration date can be modified after the webhook is created.</span></span> |
| <span data-ttu-id="a70eb-128">Povoleno</span><span class="sxs-lookup"><span data-stu-id="a70eb-128">Enabled</span></span> |<span data-ttu-id="a70eb-129">Webhook, jehož je ve výchozím nastavení povolena, když je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="a70eb-129">A webhook is enabled by default when it is created.</span></span>  <span data-ttu-id="a70eb-130">Pokud je nastavena na zakázáno, pak žádný klient bude moct používat.</span><span class="sxs-lookup"><span data-stu-id="a70eb-130">If you set it to Disabled, then no client will be able to use it.</span></span>  <span data-ttu-id="a70eb-131">Můžete nastavit **povoleno** vlastnost při vytvoření webhooku nebo kdykoli po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="a70eb-131">You can set the **Enabled** property when you create the webhook or anytime once it is created.</span></span> |

### <a name="parameters"></a><span data-ttu-id="a70eb-132">Parametry</span><span class="sxs-lookup"><span data-stu-id="a70eb-132">Parameters</span></span>
<span data-ttu-id="a70eb-133">Webhook, jehož můžete definovat hodnoty pro parametry runbooku, které se použijí při spuštění runbooku pomocí tohoto webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-133">A webhook can define values for runbook parameters that are used when the runbook is started by that webhook.</span></span> <span data-ttu-id="a70eb-134">Webhook musí obsahovat hodnoty všech povinných parametrů runbooku a můžou obsahovat hodnoty pro volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="a70eb-134">The webhook must include values for any mandatory parameters of the runbook and may include values for optional parameters.</span></span> <span data-ttu-id="a70eb-135">Hodnotu parametru, který je nakonfigurován tak, aby webhook, jehož lze změnit i po vytvoření webhoook.</span><span class="sxs-lookup"><span data-stu-id="a70eb-135">A parameter value configured to a webhook can be modified even after creating the webhoook.</span></span> <span data-ttu-id="a70eb-136">Více webhooky propojené s jedné sady runbook můžete použít jiné hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="a70eb-136">Multiple webhooks linked to a single runbook can each use different parameter values.</span></span>

<span data-ttu-id="a70eb-137">Při spuštění sady runbook pomocí webhook, jehož klienta, je nejde přepsat hodnot parametrů definovaných v webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-137">When a client starts a runbook using a webhook, it cannot override the parameter values defined in the webhook.</span></span>  <span data-ttu-id="a70eb-138">Přijímat data z klienta, sada runbook může přijmout jeden parametr s názvem **$WebhookData** typu [object], která bude obsahovat data, která zahrnuje klient v požadavku POST.</span><span class="sxs-lookup"><span data-stu-id="a70eb-138">To receive data from the client, the runbook can accept a single parameter called **$WebhookData** of type [object] that will contain data that the client includes in the POST request.</span></span>

![Vlastnosti Webhookdata](media/automation-webhooks/webhook-data-properties.png)

<span data-ttu-id="a70eb-140">**$WebhookData** objektu bude mít následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="a70eb-140">The **$WebhookData** object will have the following properties:</span></span>

| <span data-ttu-id="a70eb-141">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a70eb-141">Property</span></span> | <span data-ttu-id="a70eb-142">Popis</span><span class="sxs-lookup"><span data-stu-id="a70eb-142">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a70eb-143">WebhookName</span><span class="sxs-lookup"><span data-stu-id="a70eb-143">WebhookName</span></span> |<span data-ttu-id="a70eb-144">Název webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-144">The name of the webhook.</span></span> |
| <span data-ttu-id="a70eb-145">RequestHeader</span><span class="sxs-lookup"><span data-stu-id="a70eb-145">RequestHeader</span></span> |<span data-ttu-id="a70eb-146">Hodnota hash tabulku obsahující hlavičky příchozí požadavek POST.</span><span class="sxs-lookup"><span data-stu-id="a70eb-146">Hash table containing the headers of the incoming POST request.</span></span> |
| <span data-ttu-id="a70eb-147">RequestBody</span><span class="sxs-lookup"><span data-stu-id="a70eb-147">RequestBody</span></span> |<span data-ttu-id="a70eb-148">Text příchozí požadavek POST.</span><span class="sxs-lookup"><span data-stu-id="a70eb-148">The body of the incoming POST request.</span></span>  <span data-ttu-id="a70eb-149">To zachová veškeré formátování, jako je například řetězec, formát JSON, XML, nebo data formuláře kódovaná v řetězci.</span><span class="sxs-lookup"><span data-stu-id="a70eb-149">This will retain any formatting such as string, JSON, XML, or form encoded data.</span></span> <span data-ttu-id="a70eb-150">Sada runbook musí být napsané pro práci s formátem dat, kterou je očekáván.</span><span class="sxs-lookup"><span data-stu-id="a70eb-150">The runbook must be written to work with the data format that is expected.</span></span> |

<span data-ttu-id="a70eb-151">Neexistuje žádná konfigurace webhooku potřebné k podpoře **$WebhookData** parametr a sada runbook není potřeba ji přijmout.</span><span class="sxs-lookup"><span data-stu-id="a70eb-151">There is no configuration of the webhook required to support the **$WebhookData** parameter, and the runbook is not required to accept it.</span></span>  <span data-ttu-id="a70eb-152">Pokud sada runbook nedefinuje parametr, se ignoruje všechny podrobnosti o požadavek odeslaný z klienta.</span><span class="sxs-lookup"><span data-stu-id="a70eb-152">If the runbook does not define the parameter, then any details of the request sent from the client is ignored.</span></span>

<span data-ttu-id="a70eb-153">Pokud zadáte hodnotu pro $WebhookData při vytváření webhooku, že hodnota bude elementem při webhooku spuštění sady runbook s daty z klienta žádosti, i v případě, že klient neobsahuje žádná data v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-153">If you specify a value for $WebhookData when you create the webhook, that value will be overriden when the webhook starts the runbook with the data from the client POST request, even if the client does not include any data in the request body.</span></span>  <span data-ttu-id="a70eb-154">Pokud spustíte runbook, který má $WebhookData pomocí jiné metody než webhook, jehož, je zadat hodnotu pro $Webhookdata, rozpozná pomocí sady runbook.</span><span class="sxs-lookup"><span data-stu-id="a70eb-154">If you start a runbook that has $WebhookData using a method other than a webhook, you can provide a value for $Webhookdata that will be recognized by the runbook.</span></span>  <span data-ttu-id="a70eb-155">Tato hodnota by měla být objekt se stejným [vlastnosti](#details-of-a-webhook) jako $Webhookdata tak, aby sada runbook může správně s ním pracovat, jako kdyby byla práce s skutečné WebhookData předaná webhook, jehož.</span><span class="sxs-lookup"><span data-stu-id="a70eb-155">This value should be an object with the same [properties](#details-of-a-webhook) as $Webhookdata so that the runbook can properly work with it as if it was working with actual WebhookData passed by a webhook.</span></span>

<span data-ttu-id="a70eb-156">Například pokud se spouštění následující sady runbook na portálu Azure a chcete předat některé ukázkové WebhookData pro testování, protože WebhookData je objekt, se mají být předány jako JSON v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a70eb-156">For example, if you are starting the following runbook from the Azure Portal and want to pass some sample WebhookData for testing, since WebhookData is an object, it should be passed as JSON in the UI.</span></span>

![Parametr WebhookData z uživatelského rozhraní](media/automation-webhooks/WebhookData-parameter-from-UI.png)

<span data-ttu-id="a70eb-158">Výše uvedené sady runbook, pokud máte následující vlastnosti pro parametr WebhookData:</span><span class="sxs-lookup"><span data-stu-id="a70eb-158">For the above runbook, if you have the following properties for the WebhookData parameter:</span></span>

1. <span data-ttu-id="a70eb-159">WebhookName: *MyWebhook*</span><span class="sxs-lookup"><span data-stu-id="a70eb-159">WebhookName: *MyWebhook*</span></span>
2. <span data-ttu-id="a70eb-160">RequestHeader: *z = testovacího uživatele*</span><span class="sxs-lookup"><span data-stu-id="a70eb-160">RequestHeader: *From=Test User*</span></span>
3. <span data-ttu-id="a70eb-161">RequestBody: *["VM1", "Virtuálního počítače 2"]*</span><span class="sxs-lookup"><span data-stu-id="a70eb-161">RequestBody: *[“VM1”, “VM2”]*</span></span>

<span data-ttu-id="a70eb-162">By pak předejte následující hodnotu JSON v uživatelském rozhraní pro parametr WebhookData:</span><span class="sxs-lookup"><span data-stu-id="a70eb-162">Then you would pass the following JSON value in the UI for the WebhookData parameter:</span></span>  

* <span data-ttu-id="a70eb-163">{"WebhookName": "MyWebhook", "RequestHeader": {"Od": "Testovací uživatele"}, "RequestBody": "[\"VM1\",\"virtuálního počítače 2\"]"}</span><span class="sxs-lookup"><span data-stu-id="a70eb-163">{"WebhookName":"MyWebhook", "RequestHeader":{"From":"Test User"}, "RequestBody":"[\"VM1\",\"VM2\"]"}</span></span>

![Parametr WebhookData zahájení z uživatelského rozhraní](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> <span data-ttu-id="a70eb-165">Hodnoty všech vstupních parametrů se protokolují pod úloha sady runbook.</span><span class="sxs-lookup"><span data-stu-id="a70eb-165">The values of all input parameters are logged with the runbook job.</span></span>  <span data-ttu-id="a70eb-166">To znamená, že žádný vstup klientem v požadavku webhooku bude protokolu a k dispozici všem uživatelům s přístupem k automatizaci úloh.</span><span class="sxs-lookup"><span data-stu-id="a70eb-166">This means that any input provided by the client in the webhook request will be logged and available to anyone with access to the automation job.</span></span>  <span data-ttu-id="a70eb-167">Z tohoto důvodu byste měli být opatrní při voláních webhooku včetně citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="a70eb-167">For this reason, you should be cautious about including sensitive information in webhook calls.</span></span>
>

## <a name="security"></a><span data-ttu-id="a70eb-168">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="a70eb-168">Security</span></span>
<span data-ttu-id="a70eb-169">Zabezpečení webhooku spoléhá na ochranu osobních údajů jeho adresa URL, který obsahuje token zabezpečení, který mohou být volána.</span><span class="sxs-lookup"><span data-stu-id="a70eb-169">The security of a webhook relies on the privacy of its URL which contains a security token that allows it to be invoked.</span></span> <span data-ttu-id="a70eb-170">Služby Azure Automation neprovede žádné ověřování na žádost, dokud se provádí na správnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a70eb-170">Azure Automation does not perform any authentication on the request as long as it is made to the correct URL.</span></span> <span data-ttu-id="a70eb-171">Z tohoto důvodu webhooky nepoužívejte pro sady runbook, které provádějí vysoce citlivých funkce bez použití alternativní způsob ověření požadavku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-171">For this reason, webhooks should not be used for runbooks that perform highly sensitive functions without using an alternate means of validating the request.</span></span>

<span data-ttu-id="a70eb-172">Můžete zahrnout logiky v rámci sady runbook k určení, zda byla volána podle webhook, jehož kontrolou **WebhookName** vlastnost parametru $WebhookData.</span><span class="sxs-lookup"><span data-stu-id="a70eb-172">You can include logic within the runbook to determine that it was called by a webhook by checking the **WebhookName** property of the $WebhookData parameter.</span></span> <span data-ttu-id="a70eb-173">Sada runbook může provést další ověření tak, že vyhledá konkrétní informace v **RequestHeader** nebo **RequestBody** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a70eb-173">The runbook could perform further validation by looking for particular information in the **RequestHeader** or **RequestBody** properties.</span></span>

<span data-ttu-id="a70eb-174">Další strategie se se runbook provést některé ověření externí podmínka v případě, že přijala žádost o webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-174">Another strategy is to have the runbook perform some validation of an external condition when it received a webhook request.</span></span>  <span data-ttu-id="a70eb-175">Představte si třeba sadu runbook, která je volána metodou Githubu, kdykoli dojde k nové potvrzení úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="a70eb-175">For example, consider a runbook that is called by GitHub whenever there is a new commit to a GitHub repository.</span></span>  <span data-ttu-id="a70eb-176">Runbook se může připojit k Githubu k ověření, že nový potvrzení ve skutečnosti jenom vyskytla než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="a70eb-176">The runbook might connect to GitHub to validate that a new commit had actually just occurred before continuing.</span></span>

## <a name="creating-a-webhook"></a><span data-ttu-id="a70eb-177">Vytváření webhooku</span><span class="sxs-lookup"><span data-stu-id="a70eb-177">Creating a webhook</span></span>
<span data-ttu-id="a70eb-178">Použijte následující postup k vytvoření nové webhooku propojit k sadě runbook na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a70eb-178">Use the following procedure to create a new webhook linked to a runbook in the Azure portal.</span></span>

1. <span data-ttu-id="a70eb-179">Z **sady Runbook okno** na portálu Azure klikněte na tlačítko runbook, která webhooku spustí zobrazíte její okno podrobností.</span><span class="sxs-lookup"><span data-stu-id="a70eb-179">From the **Runbooks blade** in the Azure portal, click the runbook that the webhook will start to view its detail blade.</span></span>
2. <span data-ttu-id="a70eb-180">Klikněte na tlačítko **Webhooku** v horní části okna otevřete **přidat Webhooku** okno.</span><span class="sxs-lookup"><span data-stu-id="a70eb-180">Click **Webhook** at the top of the blade to open the **Add Webhook** blade.</span></span> <br><span data-ttu-id="a70eb-181">
   ![Tlačítko Webhooky](media/automation-webhooks/webhooks-button.png)</span><span class="sxs-lookup"><span data-stu-id="a70eb-181">
![Webhooks button](media/automation-webhooks/webhooks-button.png)</span></span>
3. <span data-ttu-id="a70eb-182">Klikněte na tlačítko **vytvořit nové webhooku** otevřete **okno Vytvoření webhooku**.</span><span class="sxs-lookup"><span data-stu-id="a70eb-182">Click **Create new webhook** to open the **Create webhook blade**.</span></span>
4. <span data-ttu-id="a70eb-183">Zadejte **název**, **datum vypršení platnosti** webhooku a jestli se má povolit.</span><span class="sxs-lookup"><span data-stu-id="a70eb-183">Specify a **Name**, **Expiration Date** for the webhook and whether it should be enabled.</span></span> <span data-ttu-id="a70eb-184">V tématu [podrobnosti o webhook, jehož](#details-of-a-webhook) pro další informace o těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="a70eb-184">See [Details of a webhook](#details-of-a-webhook) for more information these properties.</span></span>
5. <span data-ttu-id="a70eb-185">Kliknutím na ikonu kopírování a stisknutím Ctrl + C zkopírujte adresu URL webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-185">Click the copy icon and press Ctrl+C to copy the URL of the webhook.</span></span>  <span data-ttu-id="a70eb-186">Potom zaznamenejte jej na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="a70eb-186">Then record it in a safe place.</span></span>  <span data-ttu-id="a70eb-187">**Po vytvoření webhooku nelze znovu načíst adresu URL.**</span><span class="sxs-lookup"><span data-stu-id="a70eb-187">**Once you create the webhook, you cannot retrieve the URL again.**</span></span> <br><span data-ttu-id="a70eb-188">
   ![Adresa URL Webhooku](media/automation-webhooks/copy-webhook-url.png)</span><span class="sxs-lookup"><span data-stu-id="a70eb-188">
![Webhook URL](media/automation-webhooks/copy-webhook-url.png)</span></span>
6. <span data-ttu-id="a70eb-189">Klikněte na tlačítko **parametry** zadat hodnoty pro parametry runbooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-189">Click **Parameters** to provide values for the runbook parameters.</span></span>  <span data-ttu-id="a70eb-190">Pokud má runbook povinné parametry, pak nebude možné k vytvoření webhooku, pokud jsou zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a70eb-190">If the runbook has mandatory parameters, then you will not be able to create the webhook unless values are provided.</span></span>
7. <span data-ttu-id="a70eb-191">Klikněte na tlačítko **vytvořit** k vytvoření webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-191">Click **Create** to create the webhook.</span></span>

## <a name="using-a-webhook"></a><span data-ttu-id="a70eb-192">Pomocí webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="a70eb-192">Using a webhook</span></span>
<span data-ttu-id="a70eb-193">Klientské aplikace používat webhook, jehož po jeho vytvoření, vydá HTTP POST s adresou URL pro webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-193">To use a webhook after it has been created, your client application must issue an HTTP POST with the URL for the webhook.</span></span>  <span data-ttu-id="a70eb-194">Syntaxe webhooku bude v následujícím formátu.</span><span class="sxs-lookup"><span data-stu-id="a70eb-194">The syntax of the webhook will be in the following format.</span></span>

    http://<Webhook Server>/token?=<Token Value>

<span data-ttu-id="a70eb-195">Klient se zobrazí jednu z následujících návratové kódy z žádosti.</span><span class="sxs-lookup"><span data-stu-id="a70eb-195">The client will receive one of the following return codes from the POST request.</span></span>  

| <span data-ttu-id="a70eb-196">Kód</span><span class="sxs-lookup"><span data-stu-id="a70eb-196">Code</span></span> | <span data-ttu-id="a70eb-197">Text</span><span class="sxs-lookup"><span data-stu-id="a70eb-197">Text</span></span> | <span data-ttu-id="a70eb-198">Popis</span><span class="sxs-lookup"><span data-stu-id="a70eb-198">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="a70eb-199">202</span><span class="sxs-lookup"><span data-stu-id="a70eb-199">202</span></span> |<span data-ttu-id="a70eb-200">Přijmout</span><span class="sxs-lookup"><span data-stu-id="a70eb-200">Accepted</span></span> |<span data-ttu-id="a70eb-201">Žádost byla přijata, a sada runbook byla úspěšně zařazených do fronty.</span><span class="sxs-lookup"><span data-stu-id="a70eb-201">The request was accepted, and the runbook was successfully queued.</span></span> |
| <span data-ttu-id="a70eb-202">400</span><span class="sxs-lookup"><span data-stu-id="a70eb-202">400</span></span> |<span data-ttu-id="a70eb-203">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="a70eb-203">Bad Request</span></span> |<span data-ttu-id="a70eb-204">Požadavek nebyl přijat pro jednu z následujících důvodů.</span><span class="sxs-lookup"><span data-stu-id="a70eb-204">The request was not accepted for one of the following reasons.</span></span> <ul> <li><span data-ttu-id="a70eb-205">Webhook vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="a70eb-205">The webhook has expired.</span></span></li> <li><span data-ttu-id="a70eb-206">Webhook je zakázána.</span><span class="sxs-lookup"><span data-stu-id="a70eb-206">The webhook is disabled.</span></span></li> <li><span data-ttu-id="a70eb-207">Token v adrese URL je neplatný.</span><span class="sxs-lookup"><span data-stu-id="a70eb-207">The token in the URL is invalid.</span></span></li>  </ul> |
| <span data-ttu-id="a70eb-208">404</span><span class="sxs-lookup"><span data-stu-id="a70eb-208">404</span></span> |<span data-ttu-id="a70eb-209">Nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="a70eb-209">Not Found</span></span> |<span data-ttu-id="a70eb-210">Požadavek nebyl přijat pro jednu z následujících důvodů.</span><span class="sxs-lookup"><span data-stu-id="a70eb-210">The request was not accepted for one of the following reasons.</span></span> <ul> <li><span data-ttu-id="a70eb-211">Webhook nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="a70eb-211">The webhook was not found.</span></span></li> <li><span data-ttu-id="a70eb-212">Sada runbook nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="a70eb-212">The runbook was not found.</span></span></li> <li><span data-ttu-id="a70eb-213">Účet nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="a70eb-213">The account was not found.</span></span></li>  </ul> |
| <span data-ttu-id="a70eb-214">500</span><span class="sxs-lookup"><span data-stu-id="a70eb-214">500</span></span> |<span data-ttu-id="a70eb-215">Vnitřní chyba serveru</span><span class="sxs-lookup"><span data-stu-id="a70eb-215">Internal Server Error</span></span> |<span data-ttu-id="a70eb-216">Adresa URL byla platná, ale došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="a70eb-216">The URL was valid, but an error occurred.</span></span>  <span data-ttu-id="a70eb-217">Odešlete požadavek znovu.</span><span class="sxs-lookup"><span data-stu-id="a70eb-217">Please resubmit the request.</span></span> |

<span data-ttu-id="a70eb-218">Za předpokladu, že požadavek je úspěšné, webhooku odpovědi obsahuje id úlohy ve formátu JSON následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="a70eb-218">Assuming the request is successful, the webhook response contains the job id in JSON format as follows.</span></span> <span data-ttu-id="a70eb-219">Bude obsahovat id jedné úlohy, ale formát JSON umožňuje potenciální budoucí vylepšení.</span><span class="sxs-lookup"><span data-stu-id="a70eb-219">It will contain a single job id, but the JSON format allows for potential future enhancements.</span></span>

    {"JobIds":["<JobId>"]}  

<span data-ttu-id="a70eb-220">Klient nemůže zjistit po dokončení úlohy runbooku nebo její stav dokončení od webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-220">The client cannot determine when the runbook job completes or its completion status from the webhook.</span></span>  <span data-ttu-id="a70eb-221">Může zjistit, tyto informace id úlohy pomocí jiné metody, jako [prostředí Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) nebo [rozhraní API služby Azure Automation](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span><span class="sxs-lookup"><span data-stu-id="a70eb-221">It can determine this information using the job id with another method such as [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) or the [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span></span>

### <a name="example"></a><span data-ttu-id="a70eb-222">Příklad</span><span class="sxs-lookup"><span data-stu-id="a70eb-222">Example</span></span>
<span data-ttu-id="a70eb-223">Následující příklad používá ke spuštění sady runbook s webhook, jehož prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a70eb-223">The following example uses Windows PowerShell to start a runbook with a webhook.</span></span>  <span data-ttu-id="a70eb-224">Upozorňujeme, že jakýkoli jazyk, který může odeslat požadavek HTTP, můžete použít webhooku; Prostředí Windows PowerShell se právě používá jako příklad sem.</span><span class="sxs-lookup"><span data-stu-id="a70eb-224">Note that any language that can make an HTTP request can use a webhook; Windows PowerShell is just used here as an example.</span></span>

<span data-ttu-id="a70eb-225">Sada runbook očekává seznam virtuálních počítačů, které jsou ve formátu JSON v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-225">The runbook is expecting a list of virtual machines formatted in JSON in the body of the request.</span></span> <span data-ttu-id="a70eb-226">Můžeme také jsou včetně informací o kdo je spuštění sady runbook a datum a čas, že je právě spuštěna v hlavičce požadavku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-226">We also are including information about who is starting the runbook and the date and time it is being started in the header of the request.</span></span>      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


<span data-ttu-id="a70eb-227">Následující obrázek znázorňuje informace hlavičky (pomocí [Fiddler](http://www.telerik.com/fiddler) trasování) z této žádosti.</span><span class="sxs-lookup"><span data-stu-id="a70eb-227">The following image shows the header information (using a [Fiddler](http://www.telerik.com/fiddler) trace) from this request.</span></span> <span data-ttu-id="a70eb-228">To zahrnuje standardní hlavičky požadavku HTTP kromě vlastní datum a z hlavičky, které jsme přidali.</span><span class="sxs-lookup"><span data-stu-id="a70eb-228">This includes standard headers of an HTTP request in addition to the custom Date and From headers that we added.</span></span>  <span data-ttu-id="a70eb-229">Každý z těchto hodnot je k dispozici pro sadu runbook v **RequestHeaders** vlastnost **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="a70eb-229">Each of these values is available to the runbook in the **RequestHeaders** property of **WebhookData**.</span></span>

![Tlačítko Webhooky](media/automation-webhooks/webhook-request-headers.png)

<span data-ttu-id="a70eb-231">Následující obrázek ukazuje text žádosti (pomocí [Fiddler](http://www.telerik.com/fiddler) trasování), je k dispozici runbooku **RequestBody** vlastnost **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="a70eb-231">The following image shows the body of the request (using a [Fiddler](http://www.telerik.com/fiddler) trace)  that is available to the runbook in the **RequestBody** property of **WebhookData**.</span></span> <span data-ttu-id="a70eb-232">To je naformátován jako JSON, protože formát, který je zahrnutý v textu požadavku, který byl.</span><span class="sxs-lookup"><span data-stu-id="a70eb-232">This is formatted as JSON because that was the format that was included in the body of the request.</span></span>     

![Tlačítko Webhooky](media/automation-webhooks/webhook-request-body.png)

<span data-ttu-id="a70eb-234">Následující obrázek ukazuje žádost odesílány z prostředí Windows PowerShell a výsledné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a70eb-234">The following image shows the request being sent from Windows PowerShell and the resulting response.</span></span>  <span data-ttu-id="a70eb-235">Id úlohy je extrahována z odpovědi a převedeno na řetězec.</span><span class="sxs-lookup"><span data-stu-id="a70eb-235">The job id is extracted from the response and converted to a string.</span></span>

![Tlačítko Webhooky](media/automation-webhooks/webhook-request-response.png)

<span data-ttu-id="a70eb-237">Následující vzorový runbook přijme předchozí příklad žádost a spustí virtuální počítače zadaný v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-237">The following sample runbook accepts the previous example request and starts the virtual machines specified in the request body.</span></span>

    workflow Test-StartVirtualMachinesFromWebhook
    {
        param (
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {

            # Collect properties of WebhookData
            $WebhookName     =     $WebhookData.WebhookName
            $WebhookHeaders =     $WebhookData.RequestHeader
            $WebhookBody     =     $WebhookData.RequestBody

            # Collect individual headers. VMList converted from JSON.
            $From = $WebhookHeaders.From
            $VMList = ConvertFrom-Json -InputObject $WebhookBody
            Write-Output "Runbook started from webhook $WebhookName by $From."

            # Authenticate to Azure resources
            $Cred = Get-AutomationPSCredential -Name 'MyAzureCredential'
            Add-AzureAccount -Credential $Cred

            # Start each virtual machine
            foreach ($VM in $VMList)
            {
                $VMName = $VM.Name
                Write-Output "Starting $VMName"
                Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName
            }
        }
        else {
            Write-Error "Runbook mean to be started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-to-azure-alerts"></a><span data-ttu-id="a70eb-238">Spouštění sad runbook v reakci na výstrahy Azure</span><span class="sxs-lookup"><span data-stu-id="a70eb-238">Starting runbooks in response to Azure alerts</span></span>
<span data-ttu-id="a70eb-239">Webhooku povoleno sady runbook lze reagovat na [Azure výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="a70eb-239">Webhook-enabled runbooks can be used to react to [Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="a70eb-240">Shromažďování statistik jako výkon, dostupnost a využití pomocí Azure výstrahy můžete sledovat prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="a70eb-240">Resources in Azure can be monitored by collecting the statistics like performance, availability and usage with the help of Azure alerts.</span></span> <span data-ttu-id="a70eb-241">Obdržíte výstrahu založenou na monitorování metriky nebo události pro vaše prostředky Azure, účty služby Automation aktuálně podporují pouze metriky.</span><span class="sxs-lookup"><span data-stu-id="a70eb-241">You can receive an alert based on monitoring metrics or events for your Azure resources, currently Automation Accounts support only metrics.</span></span> <span data-ttu-id="a70eb-242">Pokud hodnota zadané metriky překročí prahovou hodnotu, přiřazené nebo nakonfigurovanou událost se aktivuje oznámení je odeslána do Správce služby nebo spolusprávci tuto výstrahu vyřešíte, další informace o metriky a události naleznete [Azure výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="a70eb-242">When the value of a specified metric exceeds the threshold assigned or if the configured event is triggered then a notification is sent to the service admin or co-admins to resolve the alert, for more information on metrics and events please refer to [Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="a70eb-243">Kromě použití Azure výstrahy jako systém oznámení, můžete také ji sady runbook v reakci na výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a70eb-243">Besides using Azure alerts as a notification system, you can also kick off runbooks in response to alerts.</span></span> <span data-ttu-id="a70eb-244">Automatizace Azure poskytuje možnost spustit webhooku povoleno sady runbook s Azure výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a70eb-244">Azure Automation provides the capability to run webhook-enabled runbooks with Azure alerts.</span></span> <span data-ttu-id="a70eb-245">Pokud metriky překročí nakonfigurovanou prahovou hodnotu pravidlo výstrahy stane aktivní a aktivuje webhook automatizace, který pak provede sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="a70eb-245">When a metric exceeds the configured threshold value then the alert rule becomes active and triggers the automation webhook which in turn executes the runbook.</span></span>

![Webhooky](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a><span data-ttu-id="a70eb-247">Kontext výstrahy</span><span class="sxs-lookup"><span data-stu-id="a70eb-247">Alert context</span></span>
<span data-ttu-id="a70eb-248">Vezměte v úvahu prostředek služby Azure, jako je například virtuální počítač, využití procesoru tohoto počítače je jedním z klíčových metriku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-248">Consider an Azure resource such as a virtual machine, CPU utilization of this machine is one of the key performance metric.</span></span> <span data-ttu-id="a70eb-249">Pokud využití procesoru je 100 % nebo více než určité dlouhou dobu, můžete chtít restartovat virtuální počítač na opravě problému.</span><span class="sxs-lookup"><span data-stu-id="a70eb-249">If the CPU utilization is 100% or more than a certain amount for long period of time, you might want to restart the virtual machine to fix the problem.</span></span> <span data-ttu-id="a70eb-250">To lze vyřešit tak, že nakonfigurujete pravidlo výstrahy pro virtuální počítač a toto pravidlo trvá procento využití procesoru jako jeho metriku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-250">This can be solved by configuring an alert rule to the virtual machine and this rule takes CPU percentage as its metric.</span></span> <span data-ttu-id="a70eb-251">Procento využití procesoru v tomto poli je právě prováděné jako příklad, ale existuje mnoho další metriky, které můžete nakonfigurovat na vašich prostředků Azure a restartování virtuálního počítače je akce, která se provede chcete tento problém vyřešit, můžete nakonfigurovat sadu runbook k provádění dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="a70eb-251">CPU percentage here is just taken as an example but there are many other metrics that you can configure to your Azure resources and restarting the virtual machine is an action that is taken to fix this issue, you can configure the runbook to take other actions.</span></span>

<span data-ttu-id="a70eb-252">Pokud toto pravidlo výstrahy se změní na aktivní a aktivuje runbook webhooku povolena, odešle kontext výstrahy do sady runbook.</span><span class="sxs-lookup"><span data-stu-id="a70eb-252">When this the alert rule becomes active and triggers the webhook-enabled runbook, it sends the alert context to the runbook.</span></span> <span data-ttu-id="a70eb-253">[Kontext výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) obsahuje podrobnosti, včetně **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** a **časové razítko** které jsou požadovány pro sadu runbook k identifikaci prostředku, na kterém je provedením akce.</span><span class="sxs-lookup"><span data-stu-id="a70eb-253">[Alert context](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) contains details including **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** and **Timestamp** which are required for the runbook to identify the resource on which it will be taking action.</span></span> <span data-ttu-id="a70eb-254">Výstrahy kontextu vložené v části textu **WebhookData** objekt posílá sady runbook a je přístupný pomocí **Webhook.RequestBody** vlastnost</span><span class="sxs-lookup"><span data-stu-id="a70eb-254">Alert context is embedded in the body part of the **WebhookData** object sent to the runbook and it can be accessed with **Webhook.RequestBody** property</span></span>

### <a name="example"></a><span data-ttu-id="a70eb-255">Příklad</span><span class="sxs-lookup"><span data-stu-id="a70eb-255">Example</span></span>
<span data-ttu-id="a70eb-256">Vytvoření virtuálního počítače Azure ve vašem předplatném a přidružení [výstrahu, kterou chcete sledovat metriku procento procesoru](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="a70eb-256">Create an Azure virtual machine in your subscription and associate an [alert to monitor CPU percentage metric](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="a70eb-257">Během vytváření výstrahy zkontrolujte, zda že vyplnění pole webhooku s adresou URL webhooku, který byl vygenerován při vytváření webhooku.</span><span class="sxs-lookup"><span data-stu-id="a70eb-257">While creating the alert make sure you populate the webhook field with the URL of the webhook which was generated while creating the webhook.</span></span>

<span data-ttu-id="a70eb-258">Následující vzorový runbook se aktivuje, když pravidlo výstrahy se změní na aktivní a shromáždí parametry kontext výstrahy, které jsou požadovány pro sadu runbook k identifikaci prostředku, na kterém je provedením akce.</span><span class="sxs-lookup"><span data-stu-id="a70eb-258">The following sample runbook is triggered when the alert rule becomes active and it collects the Alert context parameters which are required for the runbook to identify the resource on which it will be taking action.</span></span>

    workflow Invoke-RunbookUsingAlerts
    {
        param (      
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {   
            # Collect properties of WebhookData.
            $WebhookName    =   $WebhookData.WebhookName
            $WebhookBody    =   $WebhookData.RequestBody
            $WebhookHeaders =   $WebhookData.RequestHeader

            # Outputs information on the webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain the WebhookBody containing the AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain the AlertContext     
            $AlertContext = [object]$WebhookBody.context

            # Some selected AlertContext information
            Write-Output "`nALERT CONTEXT DATA"
            Write-Output "==================="
            Write-Output $AlertContext.name
            Write-Output $AlertContext.subscriptionId
            Write-Output $AlertContext.resourceGroupName
            Write-Output $AlertContext.resourceName
            Write-Output $AlertContext.resourceType
            Write-Output $AlertContext.resourceId
            Write-Output $AlertContext.timestamp

            # Act on the AlertContext data, in our case restarting the VM.
            # Authenticate to your Azure subscription using Organization ID to be able to restart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check the status property of the VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart the VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant to only be started from a webhook."  
        }  
    }



## <a name="next-steps"></a><span data-ttu-id="a70eb-259">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a70eb-259">Next steps</span></span>
* <span data-ttu-id="a70eb-260">Podrobnosti o různých způsobech spuštění sady runbook najdete v tématu [spuštění sady Runbook](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="a70eb-260">For details on different ways to start a runbook, see [Starting a Runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="a70eb-261">Informace o zobrazení stavu úlohy Runbooku, najdete v části [spuštění sady Runbook ve službě Azure Automation](automation-runbook-execution.md).</span><span class="sxs-lookup"><span data-stu-id="a70eb-261">For information on viewing the Status of a Runbook Job, refer to [Runbook execution in Azure Automation](automation-runbook-execution.md).</span></span>
* <span data-ttu-id="a70eb-262">Naučte se používat Azure Automation provést určitou akci u výstrahy Azure, najdete v tématu [napravit výstrahy virtuálních počítačů Azure pomocí Runbooků Automation](automation-azure-vm-alert-integration.md).</span><span class="sxs-lookup"><span data-stu-id="a70eb-262">To learn how to use Azure Automation to take action on Azure Alerts, see [Remediate Azure VM Alerts with Automation Runbooks](automation-azure-vm-alert-integration.md).</span></span>
