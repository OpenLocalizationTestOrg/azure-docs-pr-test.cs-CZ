---
title: "aaaHow toocreate rozhraní API v Azure API Management"
description: "Zjistěte, jak toocreate a konfigurace rozhraní API v Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 48ed8d93947253aa1e67ad995927ed6101cac072
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-apis-in-azure-api-management"></a><span data-ttu-id="d2fa1-103">Jak toocreate rozhraní API v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d2fa1-103">How toocreate APIs in Azure API Management</span></span>
<span data-ttu-id="d2fa1-104">Rozhraní API ve službě API Management představuje sadu operací, které můžete vyvolat klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-104">An API in API Management represents a set of operations that can be invoked by client applications.</span></span> <span data-ttu-id="d2fa1-105">Vytvoří se nová rozhraní API portálu vydavatele hello a pak hello potřeby, že operace jsou přidány.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-105">New APIs are created in hello publisher portal, and then hello desired operations are added.</span></span> <span data-ttu-id="d2fa1-106">Jakmile hello operace jsou přidány, hello rozhraní API je přidána tooa produktu a může být publikována.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-106">Once hello operations are added, hello API is added tooa product and can be published.</span></span> <span data-ttu-id="d2fa1-107">Po publikování rozhraní API může být odebírané tooand používají vývojáři.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-107">Once an API is published, it can be subscribed tooand used by developers.</span></span>

<span data-ttu-id="d2fa1-108">Tato příručka obsahuje hello prvním krokem v procesu hello: jak toocreate a nakonfigurujte nové rozhraní API ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-108">This guide shows hello first step in hello process: how toocreate and configure a new API in API Management.</span></span> <span data-ttu-id="d2fa1-109">Další informace o přidávání operací a publikování produktu najdete v tématu [jak tooan tooadd operace rozhraní API] [ How tooadd operations tooan API] a [jak toocreate a publikování produktu] [ How toocreate and publish a product].</span><span class="sxs-lookup"><span data-stu-id="d2fa1-109">For more information on adding operations and publishing a product, see [How tooadd operations tooan API][How tooadd operations tooan API] and [How toocreate and publish a product][How toocreate and publish a product].</span></span>

## <span data-ttu-id="d2fa1-110"><a name="create-new-api"></a>Vytvořit nové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d2fa1-110"><a name="create-new-api"> </a>Create a new API</span></span>
<span data-ttu-id="d2fa1-111">Rozhraní API vytvoříte a nakonfigurujete na portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-111">APIs are created and configured in hello publisher portal.</span></span> <span data-ttu-id="d2fa1-112">Klikněte na portálu vydavatele hello tooaccess **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-112">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="d2fa1-114">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="d2fa1-115">Klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **přidání rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-115">Click **APIs** from hello **API Management** menu on hello left, and then click **add API**.</span></span>

![Vytvoření rozhraní API][api-management-create-api]

<span data-ttu-id="d2fa1-117">Použití hello **přidání nového rozhraní API** okno tooconfigure hello nové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-117">Use hello **Add new API** window tooconfigure hello new API.</span></span>

![Přidání nového rozhraní API][api-management-add-new-api]

<span data-ttu-id="d2fa1-119">Hello následující pole jsou použité tooconfigure hello nové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-119">hello following fields are used tooconfigure hello new API.</span></span>

* <span data-ttu-id="d2fa1-120">**Název webové rozhraní API** poskytuje jedinečný a popisný název pro rozhraní API hello.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-120">**Web API name** provides a unique and descriptive name for hello API.</span></span> <span data-ttu-id="d2fa1-121">Zobrazí se na vývojáře a vydavatele portálech hello.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-121">It is displayed in hello developer and publisher portals.</span></span>
* <span data-ttu-id="d2fa1-122">**Adresu URL webové služby** odkazy hello služba HTTP implementace rozhraní API hello.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-122">**Web service URL** references hello HTTP service implementing hello API.</span></span> <span data-ttu-id="d2fa1-123">Rozhraní API správy předává požadavky toothis adresu.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-123">API management forwards requests toothis address.</span></span>
* <span data-ttu-id="d2fa1-124">**Přípona adresy URL rozhraní API webové** je připojením toohello základní adresu URL pro službu správy hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-124">**Web API URL suffix** is appended toohello base URL for hello API management service.</span></span> <span data-ttu-id="d2fa1-125">základní adresu URL Hello je společná pro všechna rozhraní API hostované instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-125">hello base URL is common for all APIs hosted by an API Management service instance.</span></span> <span data-ttu-id="d2fa1-126">API Management odlišuje rozhraní API podle jejich přípona a proto hello přípona musí být jedinečný pro každé rozhraní API pro daného vydavatele.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-126">API Management distinguishes APIs by their suffix and therefore hello suffix must be unique for every API for a given publisher.</span></span>
* <span data-ttu-id="d2fa1-127">**Schéma adresy URL rozhraní API webové** určuje protokoly, které lze použít tooaccess hello API.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-127">**Web API URL scheme** determines which protocols can be used tooaccess hello API.</span></span> <span data-ttu-id="d2fa1-128">Ve výchozím nastavení je zadán HTTPs.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-128">HTTPs is specified by default.</span></span>
* <span data-ttu-id="d2fa1-129">toooptionally přidat tento nový tooa produkt rozhraní API, klikněte na tlačítko hello **produkty (volitelné)** rozevíracího seznamu a vyberte produkt.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-129">toooptionally add this new API tooa product, click hello **Products (optional)** drop-down and choose a product.</span></span> <span data-ttu-id="d2fa1-130">Tento krok může být opakovaný vícekrát tooadd hello rozhraní API toomultiple produkty.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-130">This step can be repeated multiple times tooadd hello API toomultiple products.</span></span>

<span data-ttu-id="d2fa1-131">Jakmile hello potřeby hodnoty jsou nakonfigurované, klikněte na možnost **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-131">Once hello desired values are configured, click **Save**.</span></span> <span data-ttu-id="d2fa1-132">Po vytvoření nového rozhraní API hello hello souhrnná stránka rozhraní API hello se zobrazí na portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-132">Once hello new API is created, hello summary page for hello API is displayed in hello publisher portal.</span></span>

![Souhrn rozhraní API][api-management-api-summary]

## <span data-ttu-id="d2fa1-134"><a name="configure-api-settings"></a>Nastavení konfigurace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d2fa1-134"><a name="configure-api-settings"> </a>Configure API settings</span></span>
<span data-ttu-id="d2fa1-135">Můžete použít hello **nastavení** kartě tooverify a upravit hello konfigurace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-135">You can use hello **Settings** tab tooverify and edit hello configuration for an API.</span></span> <span data-ttu-id="d2fa1-136">**Název webové rozhraní API**, **adresu URL webové služby**, a **přípona adresy URL webového rozhraní API** původně nastaveny při hello rozhraní API je vytvořen a je možné upravit zde.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-136">**Web API name**, **Web service URL**, and **Web API URL suffix** are initially set when hello API is created and can be modified here.</span></span> <span data-ttu-id="d2fa1-137">**Popis** poskytuje volitelný popis a **schéma adresy URL webového rozhraní API** určuje protokoly, které lze použít tooaccess hello API.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-137">**Description** provides an optional description, and **Web API URL scheme** determines which protocols can be used tooaccess hello API.</span></span>

![Nastavení rozhraní API][api-management-api-settings]

<span data-ttu-id="d2fa1-139">ověřování brány tooconfigure hello back-end služby implementující rozhraní API hello, vyberte hello **zabezpečení** kartě hello **s přihlašovacími údaji** rozevíracího seznamu lze použít tooconfigure **HTTP základní** nebo **klientské certifikáty** ověřování.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-139">tooconfigure gateway authentication for hello backend service implementing hello API, select hello **Security** tab. hello **With credentials** drop-down can be used tooconfigure **HTTP basic** or **Client certificates** authentication.</span></span> <span data-ttu-id="d2fa1-140">Základní ověřování toouse HTTP, jednoduše zadejte přihlašovací údaje hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-140">toouse HTTP basic authentication, simply enter hello desired credentials.</span></span> <span data-ttu-id="d2fa1-141">Informace o používání ověřování certifikátu klienta najdete v tématu [jak toosecure back endové služby pomocí klientského certifikátu ověřování ve službě Azure API Management][How toosecure back-end services using client certificate authentication in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="d2fa1-141">For information on using client certificate authentication, see [How toosecure back-end services using client certificate authentication in Azure API Management][How toosecure back-end services using client certificate authentication in Azure API Management].</span></span>

<span data-ttu-id="d2fa1-142">Hello **zabezpečení** karta může být také použít tooconfigure **autorizace uživatelů** pomocí OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-142">hello **Security** tab can also be used tooconfigure **User authorization** using OAuth 2.0.</span></span> <span data-ttu-id="d2fa1-143">Další informace najdete v tématu [jak účtů tooauthorize vývojáře pomocí OAuth 2.0 ve službě Azure API Management][How tooauthorize developer accounts using OAuth 2.0 in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="d2fa1-143">For more information, see [How tooauthorize developer accounts using OAuth 2.0 in Azure API Management][How tooauthorize developer accounts using OAuth 2.0 in Azure API Management].</span></span>

![Nastavení základního ověřování][api-management-api-settings-credentials]

<span data-ttu-id="d2fa1-145">Klikněte na tlačítko **Uložit** toosave provedené změny toohello nastavení rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-145">Click **Save** toosave any changes you make toohello API settings.</span></span>

## <span data-ttu-id="d2fa1-146"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="d2fa1-146"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="d2fa1-147">Po vytvoření rozhraní API a hello nastavení, hello další kroky jsou tooadd hello operations toohello rozhraní API, přidejte hello rozhraní API tooa produktu a publikovat, aby bylo k dispozici pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-147">Once an API is created and hello settings configured, hello next steps are tooadd hello operations toohello API, add hello API tooa product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="d2fa1-148">Další informace najdete v tématu hello následující články.</span><span class="sxs-lookup"><span data-stu-id="d2fa1-148">For more information, see hello following articles.</span></span>

* <span data-ttu-id="d2fa1-149">[Jak tooan tooadd operace rozhraní API][How tooadd operations tooan API]</span><span class="sxs-lookup"><span data-stu-id="d2fa1-149">[How tooadd operations tooan API][How tooadd operations tooan API]</span></span>
* <span data-ttu-id="d2fa1-150">[Jak toocreate a publikování produktu][How toocreate and publish a product]</span><span class="sxs-lookup"><span data-stu-id="d2fa1-150">[How toocreate and publish a product][How toocreate and publish a product]</span></span>

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How toosecure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How tooauthorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
