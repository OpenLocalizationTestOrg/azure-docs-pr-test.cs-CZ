---
title: "aaaStarting runbook služby automatizace Azure s webhook, jehož | Microsoft Docs"
description: "Webhook, která umožňuje toostart klienta sady runbook ve službě Azure Automation z volání protokolu HTTP.  Tento článek popisuje, jak toocreate webhook, jehož a jak toocall jeden toostart sady runbook."
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
ms.openlocfilehash: ca6cde66b3784ceb5d0bc5921cee87aea74cb150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a><span data-ttu-id="bd5b1-104">Počínaje webhook, jehož runbook služby automatizace Azure</span><span class="sxs-lookup"><span data-stu-id="bd5b1-104">Starting an Azure Automation runbook with a webhook</span></span>
<span data-ttu-id="bd5b1-105">A *webhooku* vám umožní toostart konkrétní runbook ve službě Azure Automation prostřednictvím jedné žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-105">A *webhook* allows you toostart a particular runbook in Azure Automation through a single HTTP request.</span></span> <span data-ttu-id="bd5b1-106">To umožňuje externích služeb, jako je například Visual Studio Team Services, GitHub, analýzy protokolů Microsoft Operations Management Suite nebo vlastních aplikací toostart runbooků bez implementace úplné řešení pomocí hello rozhraní API služby Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-106">This allows external services such as Visual Studio Team Services, GitHub, Microsoft Operations Management Suite Log Analytics, or custom applications toostart runbooks without implementing a full solution using hello Azure Automation API.</span></span>  
<span data-ttu-id="bd5b1-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span><span class="sxs-lookup"><span data-stu-id="bd5b1-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span></span>

<span data-ttu-id="bd5b1-108">Můžete porovnat webhooky tooother metody spuštění sady runbook [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="bd5b1-108">You can compare webhooks tooother methods of starting a runbook in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md)</span></span>

## <a name="details-of-a-webhook"></a><span data-ttu-id="bd5b1-109">Podrobnosti o webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="bd5b1-109">Details of a webhook</span></span>
<span data-ttu-id="bd5b1-110">Hello následující tabulka popisuje hello vlastnosti, které je nutné nakonfigurovat webhooku.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-110">hello following table describes hello properties that you must configure for a webhook.</span></span>

| <span data-ttu-id="bd5b1-111">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="bd5b1-111">Property</span></span> | <span data-ttu-id="bd5b1-112">Popis</span><span class="sxs-lookup"><span data-stu-id="bd5b1-112">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="bd5b1-113">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="bd5b1-113">Name</span></span> |<span data-ttu-id="bd5b1-114">Můžete zadat, že libovolný název, který chcete použít pro webhook, jehož vzhledem k tomu, že toto nejsou viditelné toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-114">You can provide any name you want for a webhook since this is not exposed toohello client.</span></span>  <span data-ttu-id="bd5b1-115">Toto pravidlo se používá pouze pro jste tooidentify hello sady runbook ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-115">It is only used for you tooidentify hello runbook in Azure Automation.</span></span> <br>  <span data-ttu-id="bd5b1-116">Jako osvědčený postup musíte získat hello webhooku klienta související toohello název, který bude používat.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-116">As a best practice, you should give hello webhook a name related toohello client that will use it.</span></span> |
| <span data-ttu-id="bd5b1-117">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="bd5b1-117">URL</span></span> |<span data-ttu-id="bd5b1-118">Adresa URL webhooku hello Hello je toohello webhooku propojené hello jedinečnou adresu, klient volá s runbook hello toostart služby HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-118">hello URL of hello webhook is hello unique address that a client calls with an HTTP POST toostart hello runbook linked toohello webhook.</span></span>  <span data-ttu-id="bd5b1-119">Generuje se automaticky při vytvoření webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-119">It is automatically generated when you create hello webhook.</span></span>  <span data-ttu-id="bd5b1-120">Nelze zadat vlastní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-120">You cannot specify a custom URL.</span></span> <br> <br>  <span data-ttu-id="bd5b1-121">Adresa URL Hello obsahuje token zabezpečení, který umožňuje toobe hello runbook vyvolané systémem třetích stran s další ověřování.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-121">hello URL contains a security token that allows hello runbook toobe invoked by a third party system with no further authentication.</span></span> <span data-ttu-id="bd5b1-122">Z tohoto důvodu by zpracovávat jako heslo.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-122">For this reason, it should be treated like a password.</span></span>  <span data-ttu-id="bd5b1-123">Z bezpečnostních důvodů můžete pouze hello zobrazení, které se vytvoří adresu URL v portálu Azure v hello čas hello webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-123">For security reasons, you can only view hello URL in hello Azure portal at hello time hello webhook is created.</span></span> <span data-ttu-id="bd5b1-124">Upozorňujeme ale, adresa URL hello na bezpečné místo pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-124">You should note hello URL in a secure location for future use.</span></span> |
| <span data-ttu-id="bd5b1-125">Datum vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="bd5b1-125">Expiration date</span></span> |<span data-ttu-id="bd5b1-126">Stejně jako certifikát má každý webhooku datum vypršení platnosti, po kterém již slouží.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-126">Like a certificate, each webhook has an expiration date at which time it can no longer be used.</span></span>  <span data-ttu-id="bd5b1-127">Toto datum vypršení platnosti můžete změnit po vytvoření webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-127">This expiration date can be modified after hello webhook is created.</span></span> |
| <span data-ttu-id="bd5b1-128">Povoleno</span><span class="sxs-lookup"><span data-stu-id="bd5b1-128">Enabled</span></span> |<span data-ttu-id="bd5b1-129">Webhook, jehož je ve výchozím nastavení povolena, když je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-129">A webhook is enabled by default when it is created.</span></span>  <span data-ttu-id="bd5b1-130">Pokud ji nastavíte tooDisabled, pak žádný klient bude mít toouse ho.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-130">If you set it tooDisabled, then no client will be able toouse it.</span></span>  <span data-ttu-id="bd5b1-131">Můžete nastavit hello **povoleno** vlastnost při vytváření webhooku hello nebo kdykoli po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-131">You can set hello **Enabled** property when you create hello webhook or anytime once it is created.</span></span> |

### <a name="parameters"></a><span data-ttu-id="bd5b1-132">Parametry</span><span class="sxs-lookup"><span data-stu-id="bd5b1-132">Parameters</span></span>
<span data-ttu-id="bd5b1-133">Webhook, jehož můžete definovat hodnoty pro parametry runbooku, které se použijí při spuštění runbooku hello podle tohoto webhooku.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-133">A webhook can define values for runbook parameters that are used when hello runbook is started by that webhook.</span></span> <span data-ttu-id="bd5b1-134">Hello webhooku musí obsahovat hodnoty všech povinných parametrů hello runbooku a můžou obsahovat hodnoty pro volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-134">hello webhook must include values for any mandatory parameters of hello runbook and may include values for optional parameters.</span></span> <span data-ttu-id="bd5b1-135">Webhook, jehož parametr nakonfigurovaná hodnota tooa můžete změnit i po vytvoření hello webhoook.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-135">A parameter value configured tooa webhook can be modified even after creating hello webhoook.</span></span> <span data-ttu-id="bd5b1-136">Více webhooky propojené tooa jedné sady runbook můžete použít jiné hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-136">Multiple webhooks linked tooa single runbook can each use different parameter values.</span></span>

<span data-ttu-id="bd5b1-137">Při spuštění sady runbook pomocí webhook, jehož klienta, je nejde přepsat hello hodnot parametrů definovaných v webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-137">When a client starts a runbook using a webhook, it cannot override hello parameter values defined in hello webhook.</span></span>  <span data-ttu-id="bd5b1-138">tooreceive data z klienta hello, hello runbook může přijmout jeden parametr s názvem **$WebhookData** typu [object], která bude obsahovat data tohoto klienta hello zahrnuje v požadavku POST hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-138">tooreceive data from hello client, hello runbook can accept a single parameter called **$WebhookData** of type [object] that will contain data that hello client includes in hello POST request.</span></span>

![Vlastnosti Webhookdata](media/automation-webhooks/webhook-data-properties.png)

<span data-ttu-id="bd5b1-140">Hello **$WebhookData** objektu bude mít hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="bd5b1-140">hello **$WebhookData** object will have hello following properties:</span></span>

| <span data-ttu-id="bd5b1-141">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="bd5b1-141">Property</span></span> | <span data-ttu-id="bd5b1-142">Popis</span><span class="sxs-lookup"><span data-stu-id="bd5b1-142">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="bd5b1-143">WebhookName</span><span class="sxs-lookup"><span data-stu-id="bd5b1-143">WebhookName</span></span> |<span data-ttu-id="bd5b1-144">Název webhooku hello Hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-144">hello name of hello webhook.</span></span> |
| <span data-ttu-id="bd5b1-145">RequestHeader</span><span class="sxs-lookup"><span data-stu-id="bd5b1-145">RequestHeader</span></span> |<span data-ttu-id="bd5b1-146">Hodnota hash tabulku obsahující hello hlavičky hello příchozí požadavek POST.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-146">Hash table containing hello headers of hello incoming POST request.</span></span> |
| <span data-ttu-id="bd5b1-147">RequestBody</span><span class="sxs-lookup"><span data-stu-id="bd5b1-147">RequestBody</span></span> |<span data-ttu-id="bd5b1-148">Hello textu hello příchozích požadavků POST.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-148">hello body of hello incoming POST request.</span></span>  <span data-ttu-id="bd5b1-149">To zachová veškeré formátování, jako je například řetězec, formát JSON, XML, nebo data formuláře kódovaná v řetězci.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-149">This will retain any formatting such as string, JSON, XML, or form encoded data.</span></span> <span data-ttu-id="bd5b1-150">Hello runbook musí být napsané toowork s hello data formátu, který se očekává.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-150">hello runbook must be written toowork with hello data format that is expected.</span></span> |

<span data-ttu-id="bd5b1-151">Neexistuje žádná konfigurace hello webhook vyžaduje toosupport hello **$WebhookData** parametr, a hello runbook není požadovaná tooaccept ho.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-151">There is no configuration of hello webhook required toosupport hello **$WebhookData** parameter, and hello runbook is not required tooaccept it.</span></span>  <span data-ttu-id="bd5b1-152">Pokud hello runbook nedefinuje hello parametr, žádné podrobnosti požadavku hello odeslaný klientem hello ignorován.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-152">If hello runbook does not define hello parameter, then any details of hello request sent from hello client is ignored.</span></span>

<span data-ttu-id="bd5b1-153">Pokud zadáte hodnotu pro $WebhookData při vytváření webhooku hello, tato hodnota bude elementem při hello webhooku spuštění sady runbook hello hello daty z požadavku POST hello klienta, i když hello klienta neobsahuje žádná data v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-153">If you specify a value for $WebhookData when you create hello webhook, that value will be overriden when hello webhook starts hello runbook with hello data from hello client POST request, even if hello client does not include any data in hello request body.</span></span>  <span data-ttu-id="bd5b1-154">Pokud spustíte runbook, který má $WebhookData pomocí jiné metody než webhook, jehož, je zadat hodnotu pro $Webhookdata, rozpozná pomocí hello runbook.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-154">If you start a runbook that has $WebhookData using a method other than a webhook, you can provide a value for $Webhookdata that will be recognized by hello runbook.</span></span>  <span data-ttu-id="bd5b1-155">Tato hodnota by měla být objekt s hello stejné [vlastnosti](#details-of-a-webhook) jako $Webhookdata, jako kdyby byla práce s skutečné WebhookData předaná webhook, jehož mohli dané sady runbook hello správně pracovat s ním.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-155">This value should be an object with hello same [properties](#details-of-a-webhook) as $Webhookdata so that hello runbook can properly work with it as if it was working with actual WebhookData passed by a webhook.</span></span>

<span data-ttu-id="bd5b1-156">Například pokud začínáte hello následujících runbook z hello portálu Azure a chcete toopass některé ukázkové WebhookData pro testování, protože WebhookData je objekt, se mají být předány jako JSON v hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-156">For example, if you are starting hello following runbook from hello Azure Portal and want toopass some sample WebhookData for testing, since WebhookData is an object, it should be passed as JSON in hello UI.</span></span>

![Parametr WebhookData z uživatelského rozhraní](media/automation-webhooks/WebhookData-parameter-from-UI.png)

<span data-ttu-id="bd5b1-158">Pro hello výše sady runbook, pokud máte hello následující vlastnosti pro parametr WebhookData hello:</span><span class="sxs-lookup"><span data-stu-id="bd5b1-158">For hello above runbook, if you have hello following properties for hello WebhookData parameter:</span></span>

1. <span data-ttu-id="bd5b1-159">WebhookName: *MyWebhook*</span><span class="sxs-lookup"><span data-stu-id="bd5b1-159">WebhookName: *MyWebhook*</span></span>
2. <span data-ttu-id="bd5b1-160">RequestHeader: *z = testovacího uživatele*</span><span class="sxs-lookup"><span data-stu-id="bd5b1-160">RequestHeader: *From=Test User*</span></span>
3. <span data-ttu-id="bd5b1-161">RequestBody: *["VM1", "Virtuálního počítače 2"]*</span><span class="sxs-lookup"><span data-stu-id="bd5b1-161">RequestBody: *[“VM1”, “VM2”]*</span></span>

<span data-ttu-id="bd5b1-162">Potom by úspěšně prošel zpracováním hello následující hodnota JSON v hello uživatelského rozhraní pro parametr WebhookData hello:</span><span class="sxs-lookup"><span data-stu-id="bd5b1-162">Then you would pass hello following JSON value in hello UI for hello WebhookData parameter:</span></span>  

* <span data-ttu-id="bd5b1-163">{"WebhookName": "MyWebhook", "RequestHeader": {"Od": "Testovací uživatele"}, "RequestBody": "[\"VM1\",\"virtuálního počítače 2\"]"}</span><span class="sxs-lookup"><span data-stu-id="bd5b1-163">{"WebhookName":"MyWebhook", "RequestHeader":{"From":"Test User"}, "RequestBody":"[\"VM1\",\"VM2\"]"}</span></span>

![Parametr WebhookData zahájení z uživatelského rozhraní](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> <span data-ttu-id="bd5b1-165">Hello hodnoty všech vstupních parametrů se protokolují pod hello úlohy runbooku.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-165">hello values of all input parameters are logged with hello runbook job.</span></span>  <span data-ttu-id="bd5b1-166">To znamená, že žádný vstup hello klientem v požadavku webhooku hello bude protokolu a k dispozici tooanyone s úlohu automatizace toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-166">This means that any input provided by hello client in hello webhook request will be logged and available tooanyone with access toohello automation job.</span></span>  <span data-ttu-id="bd5b1-167">Z tohoto důvodu byste měli být opatrní při voláních webhooku včetně citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-167">For this reason, you should be cautious about including sensitive information in webhook calls.</span></span>
>

## <a name="security"></a><span data-ttu-id="bd5b1-168">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="bd5b1-168">Security</span></span>
<span data-ttu-id="bd5b1-169">Hello zabezpečení webhook, jehož spoléhá na hello soukromí jeho adresa URL, který obsahuje token zabezpečení, který umožní toobe vyvolat.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-169">hello security of a webhook relies on hello privacy of its URL which contains a security token that allows it toobe invoked.</span></span> <span data-ttu-id="bd5b1-170">Služby Azure Automation neprovede žádné ověřování na požádání hello tak dlouho, dokud je k toohello správnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-170">Azure Automation does not perform any authentication on hello request as long as it is made toohello correct URL.</span></span> <span data-ttu-id="bd5b1-171">Z tohoto důvodu webhooky nepoužívejte pro sady runbook, které provádějí vysoce citlivých funkce bez použití alternativní způsob ověřování hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-171">For this reason, webhooks should not be used for runbooks that perform highly sensitive functions without using an alternate means of validating hello request.</span></span>

<span data-ttu-id="bd5b1-172">Můžete zahrnout logiky v rámci hello runbook toodetermine webhook, jehož ho byl volány kontrolou hello **WebhookName** vlastnosti parametru hello $WebhookData.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-172">You can include logic within hello runbook toodetermine that it was called by a webhook by checking hello **WebhookName** property of hello $WebhookData parameter.</span></span> <span data-ttu-id="bd5b1-173">Hello runbook může provést další ověření tak, že vyhledá konkrétní informace v hello **RequestHeader** nebo **RequestBody** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-173">hello runbook could perform further validation by looking for particular information in hello **RequestHeader** or **RequestBody** properties.</span></span>

<span data-ttu-id="bd5b1-174">Další strategie je toohave hello runbook provést některé ověření externí podmínka v případě, že přijala žádost o webhooku.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-174">Another strategy is toohave hello runbook perform some validation of an external condition when it received a webhook request.</span></span>  <span data-ttu-id="bd5b1-175">Představte si třeba sadu runbook, která je volána metodou Githubu, kdykoli dojde k nové úložiště GitHub tooa potvrzení.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-175">For example, consider a runbook that is called by GitHub whenever there is a new commit tooa GitHub repository.</span></span>  <span data-ttu-id="bd5b1-176">Hello runbook se může připojit toovalidate tooGitHub, které nové potvrzení měl ve skutečnosti jenom došlo než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-176">hello runbook might connect tooGitHub toovalidate that a new commit had actually just occurred before continuing.</span></span>

## <a name="creating-a-webhook"></a><span data-ttu-id="bd5b1-177">Vytváření webhooku</span><span class="sxs-lookup"><span data-stu-id="bd5b1-177">Creating a webhook</span></span>
<span data-ttu-id="bd5b1-178">Použijte následující postup toocreate nová sada runbook tooa webhooku propojené v hello portál Azure hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-178">Use hello following procedure toocreate a new webhook linked tooa runbook in hello Azure portal.</span></span>

1. <span data-ttu-id="bd5b1-179">Z hello **sady Runbook okno** v hello portálu Azure, klikněte na hello runbook, který hello webhooku se spustí tooview její okno podrobností.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-179">From hello **Runbooks blade** in hello Azure portal, click hello runbook that hello webhook will start tooview its detail blade.</span></span>
2. <span data-ttu-id="bd5b1-180">Klikněte na tlačítko **Webhooku** hello horní části hello okno tooopen hello **přidat Webhooku** okno.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-180">Click **Webhook** at hello top of hello blade tooopen hello **Add Webhook** blade.</span></span> <br><span data-ttu-id="bd5b1-181">
   ![Tlačítko Webhooky](media/automation-webhooks/webhooks-button.png)</span><span class="sxs-lookup"><span data-stu-id="bd5b1-181">
![Webhooks button](media/automation-webhooks/webhooks-button.png)</span></span>
3. <span data-ttu-id="bd5b1-182">Klikněte na tlačítko **vytvořit nové webhooku** tooopen hello **okno Vytvoření webhooku**.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-182">Click **Create new webhook** tooopen hello **Create webhook blade**.</span></span>
4. <span data-ttu-id="bd5b1-183">Zadejte **název**, **datum vypršení platnosti** hello webhooku a jestli se má povolit.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-183">Specify a **Name**, **Expiration Date** for hello webhook and whether it should be enabled.</span></span> <span data-ttu-id="bd5b1-184">V tématu [podrobnosti o webhook, jehož](#details-of-a-webhook) pro další informace o těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-184">See [Details of a webhook](#details-of-a-webhook) for more information these properties.</span></span>
5. <span data-ttu-id="bd5b1-185">Klikněte na ikonu kopírování hello a stiskněte kombinaci kláves Ctrl + C toocopy hello URL webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-185">Click hello copy icon and press Ctrl+C toocopy hello URL of hello webhook.</span></span>  <span data-ttu-id="bd5b1-186">Potom zaznamenejte jej na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-186">Then record it in a safe place.</span></span>  <span data-ttu-id="bd5b1-187">**Po vytvoření webhooku hello nelze znovu načíst hello URL.**</span><span class="sxs-lookup"><span data-stu-id="bd5b1-187">**Once you create hello webhook, you cannot retrieve hello URL again.**</span></span> <br><span data-ttu-id="bd5b1-188">
   ![Adresa URL Webhooku](media/automation-webhooks/copy-webhook-url.png)</span><span class="sxs-lookup"><span data-stu-id="bd5b1-188">
![Webhook URL](media/automation-webhooks/copy-webhook-url.png)</span></span>
6. <span data-ttu-id="bd5b1-189">Klikněte na tlačítko **parametry** tooprovide hodnoty pro parametry runbooku hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-189">Click **Parameters** tooprovide values for hello runbook parameters.</span></span>  <span data-ttu-id="bd5b1-190">Pokud má hello runbook povinné parametry, pak nebude možné toocreate hello webhooku Pokud hodnoty jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-190">If hello runbook has mandatory parameters, then you will not be able toocreate hello webhook unless values are provided.</span></span>
7. <span data-ttu-id="bd5b1-191">Klikněte na tlačítko **vytvořit** webhooku toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-191">Click **Create** toocreate hello webhook.</span></span>

## <a name="using-a-webhook"></a><span data-ttu-id="bd5b1-192">Pomocí webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="bd5b1-192">Using a webhook</span></span>
<span data-ttu-id="bd5b1-193">toouse webhooku po jeho vytvoření klientské aplikace musíte vydat HTTP POST s adresou URL hello webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-193">toouse a webhook after it has been created, your client application must issue an HTTP POST with hello URL for hello webhook.</span></span>  <span data-ttu-id="bd5b1-194">Syntaxe Hello hello webhooku bude v hello formátu.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-194">hello syntax of hello webhook will be in hello following format.</span></span>

    http://<Webhook Server>/token?=<Token Value>

<span data-ttu-id="bd5b1-195">Hello klienta se zobrazí jednu z následujících návratové kódy z požadavku POST hello hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-195">hello client will receive one of hello following return codes from hello POST request.</span></span>  

| <span data-ttu-id="bd5b1-196">Kód</span><span class="sxs-lookup"><span data-stu-id="bd5b1-196">Code</span></span> | <span data-ttu-id="bd5b1-197">Text</span><span class="sxs-lookup"><span data-stu-id="bd5b1-197">Text</span></span> | <span data-ttu-id="bd5b1-198">Popis</span><span class="sxs-lookup"><span data-stu-id="bd5b1-198">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="bd5b1-199">202</span><span class="sxs-lookup"><span data-stu-id="bd5b1-199">202</span></span> |<span data-ttu-id="bd5b1-200">Přijmout</span><span class="sxs-lookup"><span data-stu-id="bd5b1-200">Accepted</span></span> |<span data-ttu-id="bd5b1-201">Hello žádost byla přijata, a hello runbook byla úspěšně zařazených do fronty.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-201">hello request was accepted, and hello runbook was successfully queued.</span></span> |
| <span data-ttu-id="bd5b1-202">400</span><span class="sxs-lookup"><span data-stu-id="bd5b1-202">400</span></span> |<span data-ttu-id="bd5b1-203">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="bd5b1-203">Bad Request</span></span> |<span data-ttu-id="bd5b1-204">Hello požadavku nebyl přijat pro jednu z následujících důvodů hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-204">hello request was not accepted for one of hello following reasons.</span></span> <ul> <li><span data-ttu-id="bd5b1-205">Hello webhooku vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-205">hello webhook has expired.</span></span></li> <li><span data-ttu-id="bd5b1-206">Hello webhooku je zakázána.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-206">hello webhook is disabled.</span></span></li> <li><span data-ttu-id="bd5b1-207">token Hello v adrese URL hello je neplatný.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-207">hello token in hello URL is invalid.</span></span></li>  </ul> |
| <span data-ttu-id="bd5b1-208">404</span><span class="sxs-lookup"><span data-stu-id="bd5b1-208">404</span></span> |<span data-ttu-id="bd5b1-209">Nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="bd5b1-209">Not Found</span></span> |<span data-ttu-id="bd5b1-210">Hello požadavku nebyl přijat pro jednu z následujících důvodů hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-210">hello request was not accepted for one of hello following reasons.</span></span> <ul> <li><span data-ttu-id="bd5b1-211">Hello webhooku nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-211">hello webhook was not found.</span></span></li> <li><span data-ttu-id="bd5b1-212">Hello runbook nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-212">hello runbook was not found.</span></span></li> <li><span data-ttu-id="bd5b1-213">nebyl nalezen účet Hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-213">hello account was not found.</span></span></li>  </ul> |
| <span data-ttu-id="bd5b1-214">500</span><span class="sxs-lookup"><span data-stu-id="bd5b1-214">500</span></span> |<span data-ttu-id="bd5b1-215">Vnitřní chyba serveru</span><span class="sxs-lookup"><span data-stu-id="bd5b1-215">Internal Server Error</span></span> |<span data-ttu-id="bd5b1-216">Adresa URL Hello byla platná, ale došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-216">hello URL was valid, but an error occurred.</span></span>  <span data-ttu-id="bd5b1-217">Odešlete žádost hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-217">Please resubmit hello request.</span></span> |

<span data-ttu-id="bd5b1-218">Za předpokladu, že je požadavek hello úspěšné, hello webhooku odpovědi obsahuje id úlohy hello ve formátu JSON následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-218">Assuming hello request is successful, hello webhook response contains hello job id in JSON format as follows.</span></span> <span data-ttu-id="bd5b1-219">Bude obsahovat jednu úlohu s id, ale umožňuje potenciální budoucí vylepšení hello formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-219">It will contain a single job id, but hello JSON format allows for potential future enhancements.</span></span>

    {"JobIds":["<JobId>"]}  

<span data-ttu-id="bd5b1-220">Hello klienta nemůže zjistit po dokončení úlohy runbooku hello nebo její stav dokončení od webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-220">hello client cannot determine when hello runbook job completes or its completion status from hello webhook.</span></span>  <span data-ttu-id="bd5b1-221">Může zjistit, tyto informace id úlohy hello pomocí jiné metody, jako [prostředí Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) nebo hello [rozhraní API služby Azure Automation](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd5b1-221">It can determine this information using hello job id with another method such as [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) or hello [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span></span>

### <a name="example"></a><span data-ttu-id="bd5b1-222">Příklad</span><span class="sxs-lookup"><span data-stu-id="bd5b1-222">Example</span></span>
<span data-ttu-id="bd5b1-223">Následující ukázka Hello používá prostředí Windows PowerShell toostart sady runbook s webhook, jehož.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-223">hello following example uses Windows PowerShell toostart a runbook with a webhook.</span></span>  <span data-ttu-id="bd5b1-224">Upozorňujeme, že jakýkoli jazyk, který může odeslat požadavek HTTP, můžete použít webhooku; Prostředí Windows PowerShell se právě používá jako příklad sem.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-224">Note that any language that can make an HTTP request can use a webhook; Windows PowerShell is just used here as an example.</span></span>

<span data-ttu-id="bd5b1-225">Hello runbook očekává seznam virtuálních počítačů, které jsou ve formátu JSON v textu hello hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-225">hello runbook is expecting a list of virtual machines formatted in JSON in hello body of hello request.</span></span> <span data-ttu-id="bd5b1-226">Můžeme také jsou včetně informací o který zahajuje hello runbook a hello datum a čas, že je právě spuštěna v záhlaví hello hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-226">We also are including information about who is starting hello runbook and hello date and time it is being started in hello header of hello request.</span></span>      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


<span data-ttu-id="bd5b1-227">Hello následující obrázek znázorňuje informace hlavičky hello (pomocí [Fiddler](http://www.telerik.com/fiddler) trasování) z této žádosti.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-227">hello following image shows hello header information (using a [Fiddler](http://www.telerik.com/fiddler) trace) from this request.</span></span> <span data-ttu-id="bd5b1-228">To zahrnuje standardní hlavičky požadavku HTTP v přidání toohello vlastní hodnota data a z hlavičky, které jsme přidali.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-228">This includes standard headers of an HTTP request in addition toohello custom Date and From headers that we added.</span></span>  <span data-ttu-id="bd5b1-229">Každý z těchto hodnot je k dispozici toohello runbook v hello **RequestHeaders** vlastnost **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-229">Each of these values is available toohello runbook in hello **RequestHeaders** property of **WebhookData**.</span></span>

![Tlačítko Webhooky](media/automation-webhooks/webhook-request-headers.png)

<span data-ttu-id="bd5b1-231">Hello následující obrázek ukazuje hello textu hello požadavku (pomocí [Fiddler](http://www.telerik.com/fiddler) trasování) který je k dispozici toohello runbook v hello **RequestBody** vlastnost **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-231">hello following image shows hello body of hello request (using a [Fiddler](http://www.telerik.com/fiddler) trace)  that is available toohello runbook in hello **RequestBody** property of **WebhookData**.</span></span> <span data-ttu-id="bd5b1-232">To je naformátován jako JSON, protože hello formátu, který je zahrnutý v textu hello hello požadavku, který byl.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-232">This is formatted as JSON because that was hello format that was included in hello body of hello request.</span></span>     

![Tlačítko Webhooky](media/automation-webhooks/webhook-request-body.png)

<span data-ttu-id="bd5b1-234">Hello následující obrázek znázorňuje hello požadavek odesílány z prostředí Windows PowerShell a výsledné odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-234">hello following image shows hello request being sent from Windows PowerShell and hello resulting response.</span></span>  <span data-ttu-id="bd5b1-235">id úlohy Hello je extrahovat z řetězec převedený tooa a hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-235">hello job id is extracted from hello response and converted tooa string.</span></span>

![Tlačítko Webhooky](media/automation-webhooks/webhook-request-response.png)

<span data-ttu-id="bd5b1-237">Hello následující vzorový runbook přijímá hello předchozí příklad požadavek a spustí hello virtuální počítače zadaný v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-237">hello following sample runbook accepts hello previous example request and starts hello virtual machines specified in hello request body.</span></span>

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

            # Authenticate tooAzure resources
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
            Write-Error "Runbook mean toobe started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-tooazure-alerts"></a><span data-ttu-id="bd5b1-238">Spouštění sad runbook v odpovědi tooAzure výstrahy</span><span class="sxs-lookup"><span data-stu-id="bd5b1-238">Starting runbooks in response tooAzure alerts</span></span>
<span data-ttu-id="bd5b1-239">Webhooku povoleno sady runbook může být příliš použité tooreact[Azure výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="bd5b1-239">Webhook-enabled runbooks can be used tooreact too[Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="bd5b1-240">Shromažďování statistik hello jako výkon, dostupnost a využití hello pomoci Azure výstrah můžete sledovat prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-240">Resources in Azure can be monitored by collecting hello statistics like performance, availability and usage with hello help of Azure alerts.</span></span> <span data-ttu-id="bd5b1-241">Obdržíte výstrahu založenou na monitorování metriky nebo události pro vaše prostředky Azure, účty služby Automation aktuálně podporují pouze metriky.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-241">You can receive an alert based on monitoring metrics or events for your Azure resources, currently Automation Accounts support only metrics.</span></span> <span data-ttu-id="bd5b1-242">Pokud hello hodnota zadané metriky překročí prahovou hodnotu hello přiřazené nebo pokud hello nakonfigurované událost se aktivuje, pak jsou oznámení odesílána toohello služby správce nebo spolusprávci tooresolve hello výstraha, další informace o metrikách a události naleznete příliš[ Azure výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="bd5b1-242">When hello value of a specified metric exceeds hello threshold assigned or if hello configured event is triggered then a notification is sent toohello service admin or co-admins tooresolve hello alert, for more information on metrics and events please refer too[Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="bd5b1-243">Kromě použití Azure výstrahy jako systém oznámení, můžete také ji sady runbook v tooalerts odpovědi.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-243">Besides using Azure alerts as a notification system, you can also kick off runbooks in response tooalerts.</span></span> <span data-ttu-id="bd5b1-244">Azure Automation nabízí hello schopností toorun webhooku povoleno sady runbook pomocí Azure výstrahy.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-244">Azure Automation provides hello capability toorun webhook-enabled runbooks with Azure alerts.</span></span> <span data-ttu-id="bd5b1-245">Pokud překročí metriky hello nakonfigurovat prahová hodnota pravidlo výstrahy hello stane aktivní a aktivační události hello webhook automatizace, který pak provede hello runbook.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-245">When a metric exceeds hello configured threshold value then hello alert rule becomes active and triggers hello automation webhook which in turn executes hello runbook.</span></span>

![Webhooky](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a><span data-ttu-id="bd5b1-247">Kontext výstrahy</span><span class="sxs-lookup"><span data-stu-id="bd5b1-247">Alert context</span></span>
<span data-ttu-id="bd5b1-248">Vezměte v úvahu prostředek služby Azure, jako je například virtuální počítač, využití procesoru tohoto počítače je jedním z klíčových metrika hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-248">Consider an Azure resource such as a virtual machine, CPU utilization of this machine is one of hello key performance metric.</span></span> <span data-ttu-id="bd5b1-249">Pokud hello využití procesoru je 100 % nebo více než určité dlouhou dobu, můžete chtít toorestart hello virtuálního počítače toofix hello problém.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-249">If hello CPU utilization is 100% or more than a certain amount for long period of time, you might want toorestart hello virtual machine toofix hello problem.</span></span> <span data-ttu-id="bd5b1-250">To lze vyřešit tak, že nakonfigurujete se pravidlo výstrahy toohello virtuální počítač a toto pravidlo trvá procento využití procesoru jako jeho metriku.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-250">This can be solved by configuring an alert rule toohello virtual machine and this rule takes CPU percentage as its metric.</span></span> <span data-ttu-id="bd5b1-251">Procento využití procesoru v tomto poli je právě prováděné jako příklad, ale existuje mnoho metriky tooyour Azure můžete nakonfigurovat prostředky a restartování virtuálního počítače hello je akce, která je provedena toofix tento problém hello runbook tootake můžete nakonfigurovat další akce.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-251">CPU percentage here is just taken as an example but there are many other metrics that you can configure tooyour Azure resources and restarting hello virtual machine is an action that is taken toofix this issue, you can configure hello runbook tootake other actions.</span></span>

<span data-ttu-id="bd5b1-252">Když pravidlo výstrahy hello stane aktivní a hello aktivační události webhooku povoleno sady runbook, odešle runbook toohello hello kontext výstrahy.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-252">When this hello alert rule becomes active and triggers hello webhook-enabled runbook, it sends hello alert context toohello runbook.</span></span> <span data-ttu-id="bd5b1-253">[Kontext výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) obsahuje podrobnosti, včetně **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** a **časové razítko** které jsou požadovány pro hello runbook tooidentify hello prostředků, na kterém je provedením akce.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-253">[Alert context](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) contains details including **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** and **Timestamp** which are required for hello runbook tooidentify hello resource on which it will be taking action.</span></span> <span data-ttu-id="bd5b1-254">Výstrahy kontextu vložené v části textu hello hello **WebhookData** objektu odeslané toohello runbook a je přístupný pomocí **Webhook.RequestBody** vlastnost</span><span class="sxs-lookup"><span data-stu-id="bd5b1-254">Alert context is embedded in hello body part of hello **WebhookData** object sent toohello runbook and it can be accessed with **Webhook.RequestBody** property</span></span>

### <a name="example"></a><span data-ttu-id="bd5b1-255">Příklad</span><span class="sxs-lookup"><span data-stu-id="bd5b1-255">Example</span></span>
<span data-ttu-id="bd5b1-256">Vytvoření virtuálního počítače Azure ve vašem předplatném a přidružení [výstrahy toomonitor procesoru procento metrika](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="bd5b1-256">Create an Azure virtual machine in your subscription and associate an [alert toomonitor CPU percentage metric](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="bd5b1-257">Při vytváření hello výstrahy zkontrolujte, zda že naplnit hello webhooku pole s adresou URL hello hello webhooku, který byl vygenerován při vytváření webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-257">While creating hello alert make sure you populate hello webhook field with hello URL of hello webhook which was generated while creating hello webhook.</span></span>

<span data-ttu-id="bd5b1-258">Hello následujícím vzorovém runbooku se aktivuje, když pravidlo výstrahy hello stane aktivní a shromáždí hello kontext výstrahy parametry, které jsou vyžadovány pro hello runbook tooidentify hello prostředků, na kterém je provedením akce.</span><span class="sxs-lookup"><span data-stu-id="bd5b1-258">hello following sample runbook is triggered when hello alert rule becomes active and it collects hello Alert context parameters which are required for hello runbook tooidentify hello resource on which it will be taking action.</span></span>

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

            # Outputs information on hello webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain hello WebhookBody containing hello AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain hello AlertContext     
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

            # Act on hello AlertContext data, in our case restarting hello VM.
            # Authenticate tooyour Azure subscription using Organization ID toobe able toorestart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check hello status property of hello VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart hello VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant tooonly be started from a webhook."  
        }  
    }



## <a name="next-steps"></a><span data-ttu-id="bd5b1-259">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd5b1-259">Next steps</span></span>
* <span data-ttu-id="bd5b1-260">Podrobnosti na různé způsoby toostart sady runbook najdete v tématu [spuštění sady Runbook](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="bd5b1-260">For details on different ways toostart a runbook, see [Starting a Runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="bd5b1-261">Informace o hello zobrazení stavu úlohy Runbooku, najdete v části příliš[spuštění sady Runbook ve službě Azure Automation](automation-runbook-execution.md).</span><span class="sxs-lookup"><span data-stu-id="bd5b1-261">For information on viewing hello Status of a Runbook Job, refer too[Runbook execution in Azure Automation](automation-runbook-execution.md).</span></span>
* <span data-ttu-id="bd5b1-262">toolearn jak toouse akce tootake Azure Automation v Azure výstrahy, najdete v části [napravit výstrahy virtuálních počítačů Azure pomocí Runbooků Automation](automation-azure-vm-alert-integration.md).</span><span class="sxs-lookup"><span data-stu-id="bd5b1-262">toolearn how toouse Azure Automation tootake action on Azure Alerts, see [Remediate Azure VM Alerts with Automation Runbooks](automation-azure-vm-alert-integration.md).</span></span>
