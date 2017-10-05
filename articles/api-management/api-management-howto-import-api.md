---
title: "Import rozhraní API do Azure API Management | Microsoft Docs"
description: "Zjistěte, jak importovat rozhraní API a jeho operací do Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 40398b0a-ac2c-43f0-89e1-07e4abbf502f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: c851b88fc1067e65044266d07775717c028e75d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a><span data-ttu-id="1d04f-103">Postup importu definice rozhraní API s operacemi v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="1d04f-103">How to import the definition of an API with operations in Azure API Management</span></span>
<span data-ttu-id="1d04f-104">Ve službě API Management můžete vytvořit nová rozhraní API a operace přidat ručně, nebo rozhraní API může být importován společně s operací v jednom kroku.</span><span class="sxs-lookup"><span data-stu-id="1d04f-104">In API Management, new APIs can be created and the operations added manually, or the API can be imported along with the operations in one step.</span></span>

<span data-ttu-id="1d04f-105">Rozhraní API a jejich operace lze importovat z následujících formátů.</span><span class="sxs-lookup"><span data-stu-id="1d04f-105">APIs and their operations can be imported using the following formats.</span></span>

* <span data-ttu-id="1d04f-106">WADL</span><span class="sxs-lookup"><span data-stu-id="1d04f-106">WADL</span></span>
* <span data-ttu-id="1d04f-107">Swagger</span><span class="sxs-lookup"><span data-stu-id="1d04f-107">Swagger</span></span>

<span data-ttu-id="1d04f-108">Tato příručka ukazuje, jak vytvořit nové rozhraní API a importovat jeho operací v jednom kroku.</span><span class="sxs-lookup"><span data-stu-id="1d04f-108">This guide shows how create a new API and import its operations in one step.</span></span> <span data-ttu-id="1d04f-109">Informace o ruční vytvoření rozhraní API a přidávání operací najdete v tématu [vytvoření rozhraní API] [ How to create APIs] a [přidání operací do rozhraní API][How to add operations to an API].</span><span class="sxs-lookup"><span data-stu-id="1d04f-109">For information on manually creating an API and adding operations, see [How to create APIs][How to create APIs] and [How to add operations to an API][How to add operations to an API].</span></span>

## <span data-ttu-id="1d04f-110"><a name="import-api"> </a>Import rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1d04f-110"><a name="import-api"> </a>Import an API</span></span>
<span data-ttu-id="1d04f-111">Rozhraní API vytvoříte a nakonfigurujete na portálu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="1d04f-111">APIs are created and configured in the publisher portal.</span></span> <span data-ttu-id="1d04f-112">Chcete-li získat přístup k portálu vydavatele, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d04f-112">To access the publisher portal, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="1d04f-113">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="1d04f-113">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Portál vydavatele][api-management-management-console]

<span data-ttu-id="1d04f-115">Klikněte na tlačítko **rozhraní API** z **API Management** nabídky na levé straně a pak klikněte na **importovat rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="1d04f-115">Click **APIs** from the **API Management** menu on the left, and then click **import API**.</span></span>

![Rozhraní API pro import][api-management-import-apis]

<span data-ttu-id="1d04f-117">**Rozhraní API pro Import** okno má tři karty, které odpovídají tři způsoby, jak poskytnout specifikace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1d04f-117">The **Import API** window has three tabs that correspond to the three ways to provide the API specification.</span></span>

* <span data-ttu-id="1d04f-118">**Ze schránky** umožňuje specifikace rozhraní API vložit do určené textového pole.</span><span class="sxs-lookup"><span data-stu-id="1d04f-118">**From clipboard** allows you to paste the API specification into the designated text box.</span></span>
* <span data-ttu-id="1d04f-119">**Ze souboru** můžete procházet a vyberte soubor, který obsahuje specifikace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1d04f-119">**From file** allows you to browse to and select the file that contains the API specification.</span></span>
* <span data-ttu-id="1d04f-120">**Z adresy URL** umožňuje zadat adresu URL do specifikace rozhraní pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1d04f-120">**From URL** allows you to supply the URL to the specification for the API.</span></span>

![Formát importovat rozhraní API][api-management-import-api-clipboard]

<span data-ttu-id="1d04f-122">Po zadání specifikace rozhraní API, pomocí přepínačů na pravé straně, k označení specifikace formátu.</span><span class="sxs-lookup"><span data-stu-id="1d04f-122">After providing the API specification, use the radio buttons on the right to indicate the specification format.</span></span> <span data-ttu-id="1d04f-123">Jsou podporovány následující formáty.</span><span class="sxs-lookup"><span data-stu-id="1d04f-123">The following formats are supported.</span></span>

* <span data-ttu-id="1d04f-124">WADL</span><span class="sxs-lookup"><span data-stu-id="1d04f-124">WADL</span></span>
* <span data-ttu-id="1d04f-125">Swagger</span><span class="sxs-lookup"><span data-stu-id="1d04f-125">Swagger</span></span>

<span data-ttu-id="1d04f-126">Potom zadejte **přípona adresy URL webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="1d04f-126">Next, enter a **Web API URL suffix**.</span></span> <span data-ttu-id="1d04f-127">Připojí se k základní adresu URL služby API management.</span><span class="sxs-lookup"><span data-stu-id="1d04f-127">This is appended to the base URL for your API management service.</span></span> <span data-ttu-id="1d04f-128">Základní adresa URL je běžné pro všechna rozhraní API hostované na každou instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="1d04f-128">The base URL is common for all APIs hosted on each instance of an API Management service.</span></span> <span data-ttu-id="1d04f-129">API Management odlišuje rozhraní API podle jejich přípona a proto přípona musí být jedinečný pro každé rozhraní API na konkrétní instance služby API management.</span><span class="sxs-lookup"><span data-stu-id="1d04f-129">API Management distinguishes APIs by their suffix and therefore the suffix must be unique for every API in a specific API management service instance.</span></span>

<span data-ttu-id="1d04f-130">Po zadání všech hodnot, klikněte na **Uložit** k vytvoření rozhraní API a související operace.</span><span class="sxs-lookup"><span data-stu-id="1d04f-130">Once all values are entered, click **Save** to create the API and the associated operations.</span></span> 

> [!NOTE]
> <span data-ttu-id="1d04f-131">Kurz import základní kalkulačky rozhraní API ve formátu Swagger, najdete v části [Správa vašeho prvního rozhraní API v Azure API Management](api-management-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1d04f-131">For a tutorial of importing a basic calculator API in Swagger format, see [Manage your first API in Azure API Management](api-management-get-started.md).</span></span>
> 
> 

## <span data-ttu-id="1d04f-132"><a name="export-api"></a> Export rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1d04f-132"><a name="export-api"> </a> Export an API</span></span>
<span data-ttu-id="1d04f-133">Kromě import nových rozhraní API, můžete exportovat definice vaše rozhraní API na portálu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="1d04f-133">In addition to importing new APIs, you can export the definitions of your APIs from the publisher portal.</span></span> <span data-ttu-id="1d04f-134">Chcete-li to provést, klikněte na tlačítko **Export rozhraní API** z **kartu Souhrn** z vaší **rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="1d04f-134">To do so, click **Export API** from the **Summary tab** of your **API**.</span></span>

![Export rozhraní API][api-management-export-api]

<span data-ttu-id="1d04f-136">Rozhraní API je možné exportovat pomocí WADL nebo Swagger.</span><span class="sxs-lookup"><span data-stu-id="1d04f-136">APIs can be exported using WADL or Swagger.</span></span> <span data-ttu-id="1d04f-137">Vyberte požadovaný formát, klikněte na tlačítko **Uložit**a vyberte umístění, do kterého chcete soubor uložit.</span><span class="sxs-lookup"><span data-stu-id="1d04f-137">Select the desired format, click **Save**, and choose the location in which to save the file.</span></span>

![Formát exportu rozhraní API][api-management-export-api-format]

## <span data-ttu-id="1d04f-139"><a name="next-steps"> </a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d04f-139"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="1d04f-140">Po vytvoření rozhraní API a operace importu, můžete zkontrolovat a nakonfigurujte jakákoli další nastavení, přidat rozhraní API do produktu a publikovat, aby bylo k dispozici pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="1d04f-140">Once an API is created and the operations imported, you can review and configure any additional settings, add the API to a Product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="1d04f-141">Další informace najdete v následujících pokynech.</span><span class="sxs-lookup"><span data-stu-id="1d04f-141">For more information, see the following guides.</span></span>

* <span data-ttu-id="1d04f-142">[Postup konfigurace nastavení rozhraní API][How to configure API settings]</span><span class="sxs-lookup"><span data-stu-id="1d04f-142">[How to configure API settings][How to configure API settings]</span></span>
* <span data-ttu-id="1d04f-143">[Postup vytvoření a publikování produktu][How to create and publish a product]</span><span class="sxs-lookup"><span data-stu-id="1d04f-143">[How to create and publish a product][How to create and publish a product]</span></span>

[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create APIs]: api-management-howto-create-apis.md
[How to configure API settings]: api-management-howto-create-apis.md#configure-api-settings
