---
title: "Konfigurace oznámení a e-mailových šablon ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak konfigurace oznámení a e-mailových šablon ve službě Azure API Management."
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
ms.openlocfilehash: 3d8b74e32059cfc1a4c3a8fc7d3bd04676ee80c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a><span data-ttu-id="dd241-103">Konfigurace oznámení a e-mailových šablon ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="dd241-103">How to configure notifications and email templates in Azure API Management</span></span>
<span data-ttu-id="dd241-104">API Management nabízí možnost konfigurace oznámení pro konkrétní události a nakonfigurovat e-mailových šablon, které se používají ke komunikaci s správci a vývojáři instance služby API Management.</span><span class="sxs-lookup"><span data-stu-id="dd241-104">API Management provides the ability to configure notifications for specific events, and to configure the email templates that are used to communicate with the administrators and developers of an API Management instance.</span></span> <span data-ttu-id="dd241-105">Toto téma ukazuje, jak nakonfigurovat oznámení události, k dispozici a obsahuje základní informace o konfiguraci e-mailových šablon pro tyto události.</span><span class="sxs-lookup"><span data-stu-id="dd241-105">This topic shows how to configure notifications for the available events, and provides an overview of configuring the email templates used for these events.</span></span>

## <span data-ttu-id="dd241-106"><a name="publisher-notifications"></a>Konfigurace vydavatele oznámení</span><span class="sxs-lookup"><span data-stu-id="dd241-106"><a name="publisher-notifications"> </a>Configure publisher notifications</span></span>
<span data-ttu-id="dd241-107">Chcete-li konfigurovat oznámení, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dd241-107">To configure notifications, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="dd241-108">Tím přejdete na portál vydavatele služby API Management.</span><span class="sxs-lookup"><span data-stu-id="dd241-108">This takes you to the API Management publisher portal.</span></span>

![Portál vydavatele][api-management-management-console]

> [!NOTE] 
> <span data-ttu-id="dd241-110">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="dd241-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

<span data-ttu-id="dd241-111">Klikněte na tlačítko **oznámení** z **API Management** nabídky na levé straně, chcete-li zobrazit dostupné oznámení.</span><span class="sxs-lookup"><span data-stu-id="dd241-111">Click **Notifications** from the **API Management** menu on the left to view the available notifications.</span></span>

![Vydavatele oznámení][api-management-publisher-notifications]

<span data-ttu-id="dd241-113">Následující seznam událostí může být nakonfigurována pro oznámení.</span><span class="sxs-lookup"><span data-stu-id="dd241-113">The following list of events can be configured for notifications.</span></span>

* <span data-ttu-id="dd241-114">**Žádostí o odběr (které vyžadují schválení)** -příjemců zadané e-mailu a uživatelé dostanou e-mailová oznámení o odběru požadavky pro rozhraní API produkty, které vyžadují schválení.</span><span class="sxs-lookup"><span data-stu-id="dd241-114">**Subscription requests (requiring approval)** - The specified email recipients and users will receive email notifications about subscription requests for API products requiring approval.</span></span>
* <span data-ttu-id="dd241-115">**Nová předplatná** -příjemců zadané e-mailu a uživatelé dostanou e-mailová oznámení o nových předplatných rozhraní API produktu.</span><span class="sxs-lookup"><span data-stu-id="dd241-115">**New subscriptions** - The specified email recipients and users will receive email notifications about new API product subscriptions.</span></span>
* <span data-ttu-id="dd241-116">**Žádosti o aplikace Galerie** -příjemců zadané e-mailu a uživatelé obdrží e-mailová oznámení při nové aplikace se odešlou do Galerie aplikací.</span><span class="sxs-lookup"><span data-stu-id="dd241-116">**Application gallery requests** - The specified email recipients and users will receive email notifications when new applications are submitted to the application gallery.</span></span>
* <span data-ttu-id="dd241-117">**SKRYTÁ** -příjemců zadané e-mailu a uživatelé budou obdrží e-mailu skrytá kopii všechny e-maily odesílané pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="dd241-117">**BCC** - The specified email recipients and users will receive email blind carbon copies of all emails sent to developers.</span></span>
* <span data-ttu-id="dd241-118">**Nový problém nebo komentář** – příjemců zadané e-mailu a uživatelé dostanou e-mailová oznámení, když nový problém nebo komentář je odesíláno na portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="dd241-118">**New issue or comment** - The specified email recipients and users will receive email notifications when a new issue or comment is submitted on the developer portal.</span></span>
* <span data-ttu-id="dd241-119">**Zprávu zavřete účtu** -příjemců zadané e-mailu a uživatelé dostanou e-mailová oznámení po uzavření účtu.</span><span class="sxs-lookup"><span data-stu-id="dd241-119">**Close account message** - The specified email recipients and users will receive email notifications when an account is closed.</span></span>
* <span data-ttu-id="dd241-120">**Blížící limit kvóty předplatného** -následující příjemců e-mailu a uživatelé obdrží e-mailová oznámení při použití předplatné získá brzy bude dosaženo kvóty využití.</span><span class="sxs-lookup"><span data-stu-id="dd241-120">**Approaching subscription quota limit** - The following email recipients and users will receive email notifications when subscription usage gets close to usage quota.</span></span>

<span data-ttu-id="dd241-121">U každé události lze zadat příjemců e-mailu pomocí e-mailovou adresu textového pole nebo můžete vybrat uživatele ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="dd241-121">For each event, you can specify email recipients using the email address text box or you can select users from a list.</span></span>

<span data-ttu-id="dd241-122">Pokud chcete zadat e-mailové adresy, které mají být informování, zadejte je do textového pole e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="dd241-122">To specify the email addresses to be notified, enter them in the email address text box.</span></span> <span data-ttu-id="dd241-123">Pokud máte více e-mailové adresy, oddělte je čárkami.</span><span class="sxs-lookup"><span data-stu-id="dd241-123">If you have multiple email addresses, separate them using commas.</span></span>

![Příjemců oznámení][api-management-email-addresses]

<span data-ttu-id="dd241-125">K určení uživatelů, aby se zobrazilo oznámení, klikněte na tlačítko **přidat příjemce**, zaškrtněte políčko vedle položky uživatelé upozorněni, a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="dd241-125">To specify the users to be notified, click **add recipient**, check the box beside the users to be notified, and click **OK**.</span></span>

> [!NOTE] 
> <span data-ttu-id="dd241-126">V seznamu se zobrazí pouze správci.</span><span class="sxs-lookup"><span data-stu-id="dd241-126">Only administrators are displayed in the list.</span></span>


<span data-ttu-id="dd241-127">Po dokončení konfigurace příjemců oznámení, klikněte na tlačítko **Uložit** použít aktualizované oznámení příjemci.</span><span class="sxs-lookup"><span data-stu-id="dd241-127">After configuring the notification recipients, click **Save** to apply the updated notification recipients.</span></span>

> [!NOTE] 
> <span data-ttu-id="dd241-128">Pokud přejdete směrem od **vydavatele oznámení** kartě portálu vydavatele upozorní vás, pokud existují neuložené změny.</span><span class="sxs-lookup"><span data-stu-id="dd241-128">If you navigate away from the **Publisher Notifications** tab the publisher portal alerts you if there are unsaved changes.</span></span>


## <span data-ttu-id="dd241-129"><a name="email-templates"></a>Konfigurovat e-mailových šablon</span><span class="sxs-lookup"><span data-stu-id="dd241-129"><a name="email-templates"> </a>Configure email templates</span></span>
<span data-ttu-id="dd241-130">API Management poskytuje e-mailových šablon pro e-mailové zprávy, které se odesílají při správě a používání služby.</span><span class="sxs-lookup"><span data-stu-id="dd241-130">API Management provides email templates for the email messages that are sent in the course of administering and using the service.</span></span> <span data-ttu-id="dd241-131">Následující e-mailové šablony jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="dd241-131">The following email templates are provided.</span></span>

* <span data-ttu-id="dd241-132">Odeslání aplikace Galerie schválení</span><span class="sxs-lookup"><span data-stu-id="dd241-132">Application gallery submission approved</span></span>
* <span data-ttu-id="dd241-133">Písmeno farewell vývojáře</span><span class="sxs-lookup"><span data-stu-id="dd241-133">Developer farewell letter</span></span>
* <span data-ttu-id="dd241-134">Limit kvóty vývojáře blíží oznámení</span><span class="sxs-lookup"><span data-stu-id="dd241-134">Developer quota limit approaching notification</span></span>
* <span data-ttu-id="dd241-135">Pozvat uživatele</span><span class="sxs-lookup"><span data-stu-id="dd241-135">Invite user</span></span>
* <span data-ttu-id="dd241-136">Nový komentář přidat problému</span><span class="sxs-lookup"><span data-stu-id="dd241-136">New comment added to an issue</span></span>
* <span data-ttu-id="dd241-137">Nový problém přijat</span><span class="sxs-lookup"><span data-stu-id="dd241-137">New issue received</span></span>
* <span data-ttu-id="dd241-138">Nové předplatné aktivované</span><span class="sxs-lookup"><span data-stu-id="dd241-138">New subscription activated</span></span>
* <span data-ttu-id="dd241-139">Potvrzení odběru obnovit</span><span class="sxs-lookup"><span data-stu-id="dd241-139">Subscription renewed confirmation</span></span>
* <span data-ttu-id="dd241-140">Odmítne požadavek na odběr</span><span class="sxs-lookup"><span data-stu-id="dd241-140">Subscription request declines</span></span>
* <span data-ttu-id="dd241-141">Přišel požadavek na předplatné</span><span class="sxs-lookup"><span data-stu-id="dd241-141">Subscription request received</span></span>

<span data-ttu-id="dd241-142">Tyto šablony se dá změnit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="dd241-142">These templates can be modified as desired.</span></span>

<span data-ttu-id="dd241-143">Chcete-li zobrazit a konfigurovat e-mailových šablon pro vaše instance služby API Management, klikněte na tlačítko **oznámení** z **API Management** nabídky na levé straně a vyberte **e-mailových šablon**kartě.</span><span class="sxs-lookup"><span data-stu-id="dd241-143">To view and configure the email templates for your API Management instance, click **Notifications** from the **API Management** menu on the left, and select the **Email Templates** tab.</span></span>

![Šablony e-mailů][api-management-email-templates]

<span data-ttu-id="dd241-145">Chcete-li zobrazit nebo upravit šablonu, vyberte ho z **šablony** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="dd241-145">To view or modify a specific template, select it from the **Templates** drop-down list.</span></span>

![Seznam šablon e-mailů][api-management-email-templates-list]

<span data-ttu-id="dd241-147">Každý e-mailové šablony má předmět ve formátu prostého textu a definice text ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="dd241-147">Each email template has a subject in plain text, and a body definition in HTML format.</span></span> <span data-ttu-id="dd241-148">Každá položka se dají přizpůsobit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="dd241-148">Each item can be customized as desired.</span></span>

![Editor šablony e-mailu][api-management-email-template]

<span data-ttu-id="dd241-150">**Parametry** seznam obsahuje seznam parametrů, které v případě vložit do předmětu nebo textu, bude nahrazen určenou hodnotu, při odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="dd241-150">The **Parameters** list contains a list of parameters, which when inserted into the subject or body, will be replaced the designated value when the email is sent.</span></span> <span data-ttu-id="dd241-151">Pokud chcete vložit parametr, umístěte kurzor, kde chcete parametru přejděte a klikněte na šipku nalevo od názvu parametru.</span><span class="sxs-lookup"><span data-stu-id="dd241-151">To insert a parameter, place the cursor where you wish the parameter to go, and click the arrow to the left of the parameter name.</span></span>

<span data-ttu-id="dd241-152">Klikněte na tlačítko **Preview** nebo **odeslat testovací** zobrazíte jak e-mailu bude vypadat nebo odeslat testovací e-mail.</span><span class="sxs-lookup"><span data-stu-id="dd241-152">Click **Preview** or **Send a test** to see how the email will look or send a test email.</span></span>

> [!NOTE] 
> <span data-ttu-id="dd241-153">Parametry nejsou nahrazené skutečnými hodnotami při zobrazení náhledu nebo odeslání testu.</span><span class="sxs-lookup"><span data-stu-id="dd241-153">The parameters are not replaced with actual values when previewing or sending a test.</span></span>

<span data-ttu-id="dd241-154">Chcete-li uložit změny do šablony e-mailu, klikněte na tlačítko **Uložit**, nebo zrušit změny, klikněte na tlačítko **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="dd241-154">To save the changes to the email template, click **Save**, or to cancel the changes click **Cancel**.</span></span>
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
