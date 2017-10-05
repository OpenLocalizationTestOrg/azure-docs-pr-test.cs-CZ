---
title: "Vytvoření rozhraní API v Azure API Management"
description: "Zjistěte, jak vytvořit a nakonfigurovat rozhraní API v Azure API Management."
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
ms.openlocfilehash: ab08256fbc3caca05bf23a12016ad2acf4fc7412
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-apis-in-azure-api-management"></a><span data-ttu-id="fa475-103">Vytvoření rozhraní API v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="fa475-103">How to create APIs in Azure API Management</span></span>
<span data-ttu-id="fa475-104">Rozhraní API ve službě API Management představuje sadu operací, které můžete vyvolat klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa475-104">An API in API Management represents a set of operations that can be invoked by client applications.</span></span> <span data-ttu-id="fa475-105">Vytvoří se nová rozhraní API na portálu vydavatele a pak požadované operace jsou přidány.</span><span class="sxs-lookup"><span data-stu-id="fa475-105">New APIs are created in the publisher portal, and then the desired operations are added.</span></span> <span data-ttu-id="fa475-106">Jakmile se operace, které jsou přidány, rozhraní API se přidá do produktu a může být publikována.</span><span class="sxs-lookup"><span data-stu-id="fa475-106">Once the operations are added, the API is added to a product and can be published.</span></span> <span data-ttu-id="fa475-107">Po publikování rozhraní API se můžete přihlásit k odběru a používají vývojáři.</span><span class="sxs-lookup"><span data-stu-id="fa475-107">Once an API is published, it can be subscribed to and used by developers.</span></span>

<span data-ttu-id="fa475-108">Tato příručka obsahuje prvním krokem v procesu: postup vytvoření a konfigurace nové rozhraní API ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="fa475-108">This guide shows the first step in the process: how to create and configure a new API in API Management.</span></span> <span data-ttu-id="fa475-109">Další informace o přidávání operací a publikování produktu najdete v tématu [přidání operací do rozhraní API] [ How to add operations to an API] a [postup vytvoření a publikování produktu][How to create and publish a product].</span><span class="sxs-lookup"><span data-stu-id="fa475-109">For more information on adding operations and publishing a product, see [How to add operations to an API][How to add operations to an API] and [How to create and publish a product][How to create and publish a product].</span></span>

## <span data-ttu-id="fa475-110"><a name="create-new-api"></a>Vytvořit nové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="fa475-110"><a name="create-new-api"> </a>Create a new API</span></span>
<span data-ttu-id="fa475-111">Rozhraní API vytvoříte a nakonfigurujete na portálu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="fa475-111">APIs are created and configured in the publisher portal.</span></span> <span data-ttu-id="fa475-112">Chcete-li získat přístup k portálu vydavatele, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fa475-112">To access the publisher portal, click **Publisher portal** in the Azure Portal for your API Management service.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="fa475-114">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="fa475-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="fa475-115">Klikněte na tlačítko **rozhraní API** z **API Management** nabídky na levé straně a pak klikněte na **přidání rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="fa475-115">Click **APIs** from the **API Management** menu on the left, and then click **add API**.</span></span>

![Vytvoření rozhraní API][api-management-create-api]

<span data-ttu-id="fa475-117">Použití **přidání nového rozhraní API** okno Konfigurace nového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa475-117">Use the **Add new API** window to configure the new API.</span></span>

![Přidání nového rozhraní API][api-management-add-new-api]

<span data-ttu-id="fa475-119">Následující pole se používají ke konfiguraci nového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa475-119">The following fields are used to configure the new API.</span></span>

* <span data-ttu-id="fa475-120">**Název webové rozhraní API** poskytuje jedinečný a popisný název pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa475-120">**Web API name** provides a unique and descriptive name for the API.</span></span> <span data-ttu-id="fa475-121">Zobrazí se na vývojáře a vydavatele portálech.</span><span class="sxs-lookup"><span data-stu-id="fa475-121">It is displayed in the developer and publisher portals.</span></span>
* <span data-ttu-id="fa475-122">**Adresu URL webové služby** odkazuje na službu HTTP implementace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa475-122">**Web service URL** references the HTTP service implementing the API.</span></span> <span data-ttu-id="fa475-123">Rozhraní API správy předá požadavky na tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="fa475-123">API management forwards requests to this address.</span></span>
* <span data-ttu-id="fa475-124">**Přípona adresy URL rozhraní API webové** se připojí k základní adresu URL pro službu správy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa475-124">**Web API URL suffix** is appended to the base URL for the API management service.</span></span> <span data-ttu-id="fa475-125">Základní adresa URL je společný pro všechny rozhraní API hostovaná v instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="fa475-125">The base URL is common for all APIs hosted by an API Management service instance.</span></span> <span data-ttu-id="fa475-126">API Management odlišuje rozhraní API podle jejich přípona a proto přípona musí být jedinečný pro každé rozhraní API pro daného vydavatele.</span><span class="sxs-lookup"><span data-stu-id="fa475-126">API Management distinguishes APIs by their suffix and therefore the suffix must be unique for every API for a given publisher.</span></span>
* <span data-ttu-id="fa475-127">**Schéma adresy URL rozhraní API webové** určuje protokoly, které lze použít pro přístup k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa475-127">**Web API URL scheme** determines which protocols can be used to access the API.</span></span> <span data-ttu-id="fa475-128">Ve výchozím nastavení je zadán HTTPs.</span><span class="sxs-lookup"><span data-stu-id="fa475-128">HTTPs is specified by default.</span></span>
* <span data-ttu-id="fa475-129">Chcete-li volitelně přidat toto nové rozhraní API pro určitý produkt, klikněte na tlačítko **produkty (volitelné)** rozevíracího seznamu a vyberte produkt.</span><span class="sxs-lookup"><span data-stu-id="fa475-129">To optionally add this new API to a product, click the **Products (optional)** drop-down and choose a product.</span></span> <span data-ttu-id="fa475-130">Tento krok můžete přidat rozhraní API k více produktům opakuje vícekrát.</span><span class="sxs-lookup"><span data-stu-id="fa475-130">This step can be repeated multiple times to add the API to multiple products.</span></span>

<span data-ttu-id="fa475-131">Po nakonfigurování požadované hodnoty, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fa475-131">Once the desired values are configured, click **Save**.</span></span> <span data-ttu-id="fa475-132">Po vytvoření nového rozhraní API na portálu vydavatele zobrazí souhrnná stránka rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa475-132">Once the new API is created, the summary page for the API is displayed in the publisher portal.</span></span>

![Souhrn rozhraní API][api-management-api-summary]

## <span data-ttu-id="fa475-134"><a name="configure-api-settings"></a>Nastavení konfigurace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="fa475-134"><a name="configure-api-settings"> </a>Configure API settings</span></span>
<span data-ttu-id="fa475-135">Můžete použít **nastavení** kartě ověřte a úpravě konfigurace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa475-135">You can use the **Settings** tab to verify and edit the configuration for an API.</span></span> <span data-ttu-id="fa475-136">**Název webové rozhraní API**, **adresu URL webové služby**, a **přípona adresy URL webového rozhraní API** původně nastaveny při vytvoření rozhraní API a nelze jej změnit zde.</span><span class="sxs-lookup"><span data-stu-id="fa475-136">**Web API name**, **Web service URL**, and **Web API URL suffix** are initially set when the API is created and can be modified here.</span></span> <span data-ttu-id="fa475-137">**Popis** poskytuje volitelný popis a **schéma adresy URL webového rozhraní API** určuje protokoly, které lze použít pro přístup k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa475-137">**Description** provides an optional description, and **Web API URL scheme** determines which protocols can be used to access the API.</span></span>

![Nastavení rozhraní API][api-management-api-settings]

<span data-ttu-id="fa475-139">Ke konfiguraci ověřování brány pro back-end službu implementace rozhraní API, vyberte **zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="fa475-139">To configure gateway authentication for the backend service implementing the API, select the **Security** tab.</span></span> <span data-ttu-id="fa475-140">**s přihlašovacími údaji** rozevíracího seznamu můžete použít ke konfiguraci **HTTP basic** nebo **klientské certifikáty** ověřování.</span><span class="sxs-lookup"><span data-stu-id="fa475-140">The **With credentials** drop-down can be used to configure **HTTP basic** or **Client certificates** authentication.</span></span> <span data-ttu-id="fa475-141">Pokud chcete používat základní ověřování protokolu HTTP, jednoduše zadejte požadované pověření.</span><span class="sxs-lookup"><span data-stu-id="fa475-141">To use HTTP basic authentication, simply enter the desired credentials.</span></span> <span data-ttu-id="fa475-142">Informace o používání ověřování certifikátu klienta najdete v tématu [jak zabezpečit back endové služby pomocí klienta pro ověřování pomocí certifikátu ve službě Azure API Management][How to secure back-end services using client certificate authentication in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="fa475-142">For information on using client certificate authentication, see [How to secure back-end services using client certificate authentication in Azure API Management][How to secure back-end services using client certificate authentication in Azure API Management].</span></span>

<span data-ttu-id="fa475-143">**Zabezpečení** můžete také použít ke konfiguraci **autorizace uživatelů** pomocí OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fa475-143">The **Security** tab can also be used to configure **User authorization** using OAuth 2.0.</span></span> <span data-ttu-id="fa475-144">Další informace najdete v tématu [autorizace vývojářským účtům pomocí OAuth 2.0 ve službě Azure API Management][How to authorize developer accounts using OAuth 2.0 in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="fa475-144">For more information, see [How to authorize developer accounts using OAuth 2.0 in Azure API Management][How to authorize developer accounts using OAuth 2.0 in Azure API Management].</span></span>

![Nastavení základního ověřování][api-management-api-settings-credentials]

<span data-ttu-id="fa475-146">Klikněte na tlačítko **Uložit** uložit změny provedené nastavení rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa475-146">Click **Save** to save any changes you make to the API settings.</span></span>

## <span data-ttu-id="fa475-147"><a name="next-steps"> </a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa475-147"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="fa475-148">Po vytvoření rozhraní API a nakonfigurované nastavení jsou další kroky pro přidání operací do rozhraní API, přidat rozhraní API do produktu a publikujete ji tak, aby se k dispozici pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="fa475-148">Once an API is created and the settings configured, the next steps are to add the operations to the API, add the API to a product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="fa475-149">Další informace naleznete v následujících článcích.</span><span class="sxs-lookup"><span data-stu-id="fa475-149">For more information, see the following articles.</span></span>

* <span data-ttu-id="fa475-150">[Přidání operací do rozhraní API][How to add operations to an API]</span><span class="sxs-lookup"><span data-stu-id="fa475-150">[How to add operations to an API][How to add operations to an API]</span></span>
* <span data-ttu-id="fa475-151">[Postup vytvoření a publikování produktu][How to create and publish a product]</span><span class="sxs-lookup"><span data-stu-id="fa475-151">[How to create and publish a product][How to create and publish a product]</span></span>

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

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How to secure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How to authorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
