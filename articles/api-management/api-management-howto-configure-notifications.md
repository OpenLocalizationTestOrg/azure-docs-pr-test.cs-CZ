---
title: "aaaConfigure oznámení a e-mailových šablon ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak tooconfigure oznámení a e-mailových šablon ve službě Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: ee25f26d-4752-433b-af9c-3817db38aed5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: dc23289c25a1641992b73cb955099b3f207b6968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-notifications-and-email-templates-in-azure-api-management"></a><span data-ttu-id="3b563-103">Jak tooconfigure oznámení a e-mailových šablon ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="3b563-103">How tooconfigure notifications and email templates in Azure API Management</span></span>
<span data-ttu-id="3b563-104">Služba API Management nabízí možnost hello tooconfigure oznámení pro konkrétní události a tooconfigure hello e-mailové šablony, které jsou používané toocommunicate s hello správci a vývojáři instance služby API Management.</span><span class="sxs-lookup"><span data-stu-id="3b563-104">API Management provides hello ability tooconfigure notifications for specific events, and tooconfigure hello email templates that are used toocommunicate with hello administrators and developers of an API Management instance.</span></span> <span data-ttu-id="3b563-105">Toto téma ukazuje, jak tooconfigure oznámení pro hello k dispozici události a poskytuje přehled o konfiguraci e-mailových šablon hello používá pro tyto události.</span><span class="sxs-lookup"><span data-stu-id="3b563-105">This topic shows how tooconfigure notifications for hello available events, and provides an overview of configuring hello email templates used for these events.</span></span>

## <span data-ttu-id="3b563-106"><a name="publisher-notifications"></a>Konfigurace vydavatele oznámení</span><span class="sxs-lookup"><span data-stu-id="3b563-106"><a name="publisher-notifications"> </a>Configure publisher notifications</span></span>
<span data-ttu-id="3b563-107">tooconfigure oznámení, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="3b563-107">tooconfigure notifications, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="3b563-108">Tím přejdete portál vydavatele toohello API Management.</span><span class="sxs-lookup"><span data-stu-id="3b563-108">This takes you toohello API Management publisher portal.</span></span>

![Portál vydavatele][api-management-management-console]

> [!NOTE] 
> <span data-ttu-id="3b563-110">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="3b563-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

<span data-ttu-id="3b563-111">Klikněte na tlačítko **oznámení** z hello **API Management** nabídce hello zbývajících tooview hello k dispozici oznámení.</span><span class="sxs-lookup"><span data-stu-id="3b563-111">Click **Notifications** from hello **API Management** menu on hello left tooview hello available notifications.</span></span>

![Vydavatele oznámení][api-management-publisher-notifications]

<span data-ttu-id="3b563-113">Následující seznam událostí Hello lze nakonfigurovat pro oznámení.</span><span class="sxs-lookup"><span data-stu-id="3b563-113">hello following list of events can be configured for notifications.</span></span>

* <span data-ttu-id="3b563-114">**Žádostí o odběr (které vyžadují schválení)** – hello příjemců zadané e-mailu a uživatelé obdrží e-mailová oznámení o odběru požadavky pro rozhraní API produkty, které vyžadují schválení.</span><span class="sxs-lookup"><span data-stu-id="3b563-114">**Subscription requests (requiring approval)** - hello specified email recipients and users will receive email notifications about subscription requests for API products requiring approval.</span></span>
* <span data-ttu-id="3b563-115">**Nová předplatná** – hello příjemců zadané e-mailu a uživatelé obdrží e-mailová oznámení o nových předplatných rozhraní API produktu.</span><span class="sxs-lookup"><span data-stu-id="3b563-115">**New subscriptions** - hello specified email recipients and users will receive email notifications about new API product subscriptions.</span></span>
* <span data-ttu-id="3b563-116">**Žádosti o aplikace Galerie** – hello příjemců zadané e-mailu a uživatelé obdrží e-mailová oznámení, když nové aplikace jsou odeslaná toohello galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="3b563-116">**Application gallery requests** - hello specified email recipients and users will receive email notifications when new applications are submitted toohello application gallery.</span></span>
* <span data-ttu-id="3b563-117">**SKRYTÁ** – hello příjemců zadané e-mailu a uživatelé budou obdrží e-mailu skrytá kopii všech e-maily odesílané toodevelopers.</span><span class="sxs-lookup"><span data-stu-id="3b563-117">**BCC** - hello specified email recipients and users will receive email blind carbon copies of all emails sent toodevelopers.</span></span>
* <span data-ttu-id="3b563-118">**Nový problém nebo komentář** – hello příjemců zadané e-mailu a uživatelé obdrží oznámení e-mailu, když nový problém nebo komentář je odesíláno na portál pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="3b563-118">**New issue or comment** - hello specified email recipients and users will receive email notifications when a new issue or comment is submitted on hello developer portal.</span></span>
* <span data-ttu-id="3b563-119">**Zprávu zavřete účet** – hello příjemců zadané e-mailu a uživatelé obdrží e-mailová oznámení po uzavření účtu.</span><span class="sxs-lookup"><span data-stu-id="3b563-119">**Close account message** - hello specified email recipients and users will receive email notifications when an account is closed.</span></span>
* <span data-ttu-id="3b563-120">**Blížící limit kvóty předplatného** – hello následující příjemců e-mailu a uživatelé obdrží e-mailová oznámení, když využití předplatné získá zavřít toousage kvóty.</span><span class="sxs-lookup"><span data-stu-id="3b563-120">**Approaching subscription quota limit** - hello following email recipients and users will receive email notifications when subscription usage gets close toousage quota.</span></span>

<span data-ttu-id="3b563-121">U každé události lze zadat příjemců e-mailu pomocí hello e-mailovou adresu textového pole nebo můžete vybrat uživatele ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="3b563-121">For each event, you can specify email recipients using hello email address text box or you can select users from a list.</span></span>

<span data-ttu-id="3b563-122">toospecify hello e-mailové adresy toobe oznámení, zadejte je do hello e-mailová adresa.</span><span class="sxs-lookup"><span data-stu-id="3b563-122">toospecify hello email addresses toobe notified, enter them in hello email address text box.</span></span> <span data-ttu-id="3b563-123">Pokud máte více e-mailové adresy, oddělte je čárkami.</span><span class="sxs-lookup"><span data-stu-id="3b563-123">If you have multiple email addresses, separate them using commas.</span></span>

![Příjemců oznámení][api-management-email-addresses]

<span data-ttu-id="3b563-125">toospecify hello uživatelé toobe oznámení, klikněte na tlačítko **přidat příjemce**, zaškrtněte políčko hello vedle hello uživatelé toobe oznámení a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b563-125">toospecify hello users toobe notified, click **add recipient**, check hello box beside hello users toobe notified, and click **OK**.</span></span>

> [!NOTE] 
> <span data-ttu-id="3b563-126">V seznamu hello jsou zobrazeny pouze správci.</span><span class="sxs-lookup"><span data-stu-id="3b563-126">Only administrators are displayed in hello list.</span></span>


<span data-ttu-id="3b563-127">Po dokončení konfigurace hello příjemců oznámení, klikněte na tlačítko **Uložit** tooapply hello aktualizovat příjemců oznámení.</span><span class="sxs-lookup"><span data-stu-id="3b563-127">After configuring hello notification recipients, click **Save** tooapply hello updated notification recipients.</span></span>

> [!NOTE] 
> <span data-ttu-id="3b563-128">Pokud přejdete pryč z hello **vydavatele oznámení** portál vydavatele hello karta upozorní vás, pokud existují neuložené změny.</span><span class="sxs-lookup"><span data-stu-id="3b563-128">If you navigate away from hello **Publisher Notifications** tab hello publisher portal alerts you if there are unsaved changes.</span></span>


## <span data-ttu-id="3b563-129"><a name="email-templates"></a>Konfigurovat e-mailových šablon</span><span class="sxs-lookup"><span data-stu-id="3b563-129"><a name="email-templates"> </a>Configure email templates</span></span>
<span data-ttu-id="3b563-130">API Management poskytuje e-mailových šablon pro hello e-mailové zprávy, které se odesílají v kurzu hello správě a používání služby hello.</span><span class="sxs-lookup"><span data-stu-id="3b563-130">API Management provides email templates for hello email messages that are sent in hello course of administering and using hello service.</span></span> <span data-ttu-id="3b563-131">Následující e-mailových šablon Hello jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3b563-131">hello following email templates are provided.</span></span>

* <span data-ttu-id="3b563-132">Odeslání aplikace Galerie schválení</span><span class="sxs-lookup"><span data-stu-id="3b563-132">Application gallery submission approved</span></span>
* <span data-ttu-id="3b563-133">Písmeno farewell vývojáře</span><span class="sxs-lookup"><span data-stu-id="3b563-133">Developer farewell letter</span></span>
* <span data-ttu-id="3b563-134">Limit kvóty vývojáře blíží oznámení</span><span class="sxs-lookup"><span data-stu-id="3b563-134">Developer quota limit approaching notification</span></span>
* <span data-ttu-id="3b563-135">Pozvat uživatele</span><span class="sxs-lookup"><span data-stu-id="3b563-135">Invite user</span></span>
* <span data-ttu-id="3b563-136">Nový komentář přidat tooan problém</span><span class="sxs-lookup"><span data-stu-id="3b563-136">New comment added tooan issue</span></span>
* <span data-ttu-id="3b563-137">Nový problém přijat</span><span class="sxs-lookup"><span data-stu-id="3b563-137">New issue received</span></span>
* <span data-ttu-id="3b563-138">Nové předplatné aktivované</span><span class="sxs-lookup"><span data-stu-id="3b563-138">New subscription activated</span></span>
* <span data-ttu-id="3b563-139">Potvrzení odběru obnovit</span><span class="sxs-lookup"><span data-stu-id="3b563-139">Subscription renewed confirmation</span></span>
* <span data-ttu-id="3b563-140">Odmítne požadavek na odběr</span><span class="sxs-lookup"><span data-stu-id="3b563-140">Subscription request declines</span></span>
* <span data-ttu-id="3b563-141">Přišel požadavek na předplatné</span><span class="sxs-lookup"><span data-stu-id="3b563-141">Subscription request received</span></span>

<span data-ttu-id="3b563-142">Tyto šablony se dá změnit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3b563-142">These templates can be modified as desired.</span></span>

<span data-ttu-id="3b563-143">tooview a nakonfigurovat hello e-mailových šablon pro vaše instance služby API Management, klikněte na tlačítko **oznámení** z hello **API Management** nabídce vlevo hello a vyberte hello **e-mailových šablon**  kartě.</span><span class="sxs-lookup"><span data-stu-id="3b563-143">tooview and configure hello email templates for your API Management instance, click **Notifications** from hello **API Management** menu on hello left, and select hello **Email Templates** tab.</span></span>

![Šablony e-mailů][api-management-email-templates]

<span data-ttu-id="3b563-145">tooview nebo konkrétní šablonu upravit, vyberte ji ze hello **šablony** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3b563-145">tooview or modify a specific template, select it from hello **Templates** drop-down list.</span></span>

![Seznam šablon e-mailů][api-management-email-templates-list]

<span data-ttu-id="3b563-147">Každý e-mailové šablony má předmět ve formátu prostého textu a definice text ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="3b563-147">Each email template has a subject in plain text, and a body definition in HTML format.</span></span> <span data-ttu-id="3b563-148">Každá položka se dají přizpůsobit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3b563-148">Each item can be customized as desired.</span></span>

![Editor šablony e-mailu][api-management-email-template]

<span data-ttu-id="3b563-150">Hello **parametry** seznam obsahuje seznam parametrů, které v případě vloženy do hello předmět ani text, bude nahrazené hello určené hodnotu, při odeslání e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="3b563-150">hello **Parameters** list contains a list of parameters, which when inserted into hello subject or body, will be replaced hello designated value when hello email is sent.</span></span> <span data-ttu-id="3b563-151">tooinsert parametr, umístěte kurzor hello, kde chcete toogo hello parametr a klikněte na tlačítko hello šipku toohello nalevo od názvu parametru hello.</span><span class="sxs-lookup"><span data-stu-id="3b563-151">tooinsert a parameter, place hello cursor where you wish hello parameter toogo, and click hello arrow toohello left of hello parameter name.</span></span>

<span data-ttu-id="3b563-152">Klikněte na tlačítko **Preview** nebo **odeslat testovací** toosee jak hello e-mailu bude vypadat nebo odeslat testovací e-mail.</span><span class="sxs-lookup"><span data-stu-id="3b563-152">Click **Preview** or **Send a test** toosee how hello email will look or send a test email.</span></span>

> [!NOTE] 
> <span data-ttu-id="3b563-153">Hello parametry nejsou nahrazené skutečnými hodnotami při zobrazení náhledu nebo odeslání testu.</span><span class="sxs-lookup"><span data-stu-id="3b563-153">hello parameters are not replaced with actual values when previewing or sending a test.</span></span>

<span data-ttu-id="3b563-154">toosave hello změny toohello e-mailové šablony, klikněte na tlačítko **Uložit**, nebo klikněte na tlačítko toocancel hello změny **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="3b563-154">toosave hello changes toohello email template, click **Save**, or toocancel hello changes click **Cancel**.</span></span>
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
