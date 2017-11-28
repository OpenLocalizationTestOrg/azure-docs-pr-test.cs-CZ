---
title: "aaaImport rozhraní API do Azure API Management | Microsoft Docs"
description: "Zjistěte, jak tooimport rozhraní API a jeho operací do Azure API Management."
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
ms.openlocfilehash: 20fbbb53243aecc24d72833ec0904ae8fab97863
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-hello-definition-of-an-api-with-operations-in-azure-api-management"></a><span data-ttu-id="46e8a-103">Jak tooimport hello definice rozhraní API s operacemi v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="46e8a-103">How tooimport hello definition of an API with operations in Azure API Management</span></span>
<span data-ttu-id="46e8a-104">Ve službě API Management vytvořením nových rozhraní API a operace hello přidat ručně nebo hello rozhraní API může být importován společně s hello operace v jednom kroku.</span><span class="sxs-lookup"><span data-stu-id="46e8a-104">In API Management, new APIs can be created and hello operations added manually, or hello API can be imported along with hello operations in one step.</span></span>

<span data-ttu-id="46e8a-105">Rozhraní API a jejich operace lze importovat pomocí hello následující formáty.</span><span class="sxs-lookup"><span data-stu-id="46e8a-105">APIs and their operations can be imported using hello following formats.</span></span>

* <span data-ttu-id="46e8a-106">WADL</span><span class="sxs-lookup"><span data-stu-id="46e8a-106">WADL</span></span>
* <span data-ttu-id="46e8a-107">Swagger</span><span class="sxs-lookup"><span data-stu-id="46e8a-107">Swagger</span></span>

<span data-ttu-id="46e8a-108">Tato příručka ukazuje, jak vytvořit nové rozhraní API a importovat jeho operací v jednom kroku.</span><span class="sxs-lookup"><span data-stu-id="46e8a-108">This guide shows how create a new API and import its operations in one step.</span></span> <span data-ttu-id="46e8a-109">Informace o ruční vytvoření rozhraní API a přidávání operací najdete v tématu [jak toocreate rozhraní API] [ How toocreate APIs] a [jak tooan tooadd operace rozhraní API] [ How tooadd operations tooan API].</span><span class="sxs-lookup"><span data-stu-id="46e8a-109">For information on manually creating an API and adding operations, see [How toocreate APIs][How toocreate APIs] and [How tooadd operations tooan API][How tooadd operations tooan API].</span></span>

## <span data-ttu-id="46e8a-110"><a name="import-api"></a>Import rozhraní API</span><span class="sxs-lookup"><span data-stu-id="46e8a-110"><a name="import-api"> </a>Import an API</span></span>
<span data-ttu-id="46e8a-111">Rozhraní API vytvoříte a nakonfigurujete na portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="46e8a-111">APIs are created and configured in hello publisher portal.</span></span> <span data-ttu-id="46e8a-112">Klikněte na portálu vydavatele hello tooaccess **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="46e8a-112">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="46e8a-113">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="46e8a-113">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Portál vydavatele][api-management-management-console]

<span data-ttu-id="46e8a-115">Klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **importovat rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="46e8a-115">Click **APIs** from hello **API Management** menu on hello left, and then click **import API**.</span></span>

![Rozhraní API pro import][api-management-import-apis]

<span data-ttu-id="46e8a-117">Hello **rozhraní API pro Import** okno má tři karty, které odpovídají toohello tři způsoby tooprovide hello API specifikace.</span><span class="sxs-lookup"><span data-stu-id="46e8a-117">hello **Import API** window has three tabs that correspond toohello three ways tooprovide hello API specification.</span></span>

* <span data-ttu-id="46e8a-118">**Ze schránky** umožňuje specifikaci toopaste hello rozhraní API do hello určené textového pole.</span><span class="sxs-lookup"><span data-stu-id="46e8a-118">**From clipboard** allows you toopaste hello API specification into hello designated text box.</span></span>
* <span data-ttu-id="46e8a-119">**Ze souboru** vám umožní toobrowse tooand hello vyberte soubor, který obsahuje specifikaci hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="46e8a-119">**From file** allows you toobrowse tooand select hello file that contains hello API specification.</span></span>
* <span data-ttu-id="46e8a-120">**Z adresy URL** vám umožní toosupply hello URL toohello specifikace hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="46e8a-120">**From URL** allows you toosupply hello URL toohello specification for hello API.</span></span>

![Formát importovat rozhraní API][api-management-import-api-clipboard]

<span data-ttu-id="46e8a-122">Po zadání specifikace hello rozhraní API, použijte hello rádiová tlačítka na hello správné tooindicate hello specifikace formátu.</span><span class="sxs-lookup"><span data-stu-id="46e8a-122">After providing hello API specification, use hello radio buttons on hello right tooindicate hello specification format.</span></span> <span data-ttu-id="46e8a-123">jsou podporovány následující formáty Hello.</span><span class="sxs-lookup"><span data-stu-id="46e8a-123">hello following formats are supported.</span></span>

* <span data-ttu-id="46e8a-124">WADL</span><span class="sxs-lookup"><span data-stu-id="46e8a-124">WADL</span></span>
* <span data-ttu-id="46e8a-125">Swagger</span><span class="sxs-lookup"><span data-stu-id="46e8a-125">Swagger</span></span>

<span data-ttu-id="46e8a-126">Potom zadejte **přípona adresy URL webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="46e8a-126">Next, enter a **Web API URL suffix**.</span></span> <span data-ttu-id="46e8a-127">Toto je základní adresu URL připojení toohello služby API management.</span><span class="sxs-lookup"><span data-stu-id="46e8a-127">This is appended toohello base URL for your API management service.</span></span> <span data-ttu-id="46e8a-128">základní adresu URL Hello je společná pro všechna rozhraní API hostované na každou instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="46e8a-128">hello base URL is common for all APIs hosted on each instance of an API Management service.</span></span> <span data-ttu-id="46e8a-129">API Management odlišuje rozhraní API podle jejich přípona a proto hello přípona musí být jedinečný pro každé rozhraní API na konkrétní instance služby API management.</span><span class="sxs-lookup"><span data-stu-id="46e8a-129">API Management distinguishes APIs by their suffix and therefore hello suffix must be unique for every API in a specific API management service instance.</span></span>

<span data-ttu-id="46e8a-130">Po zadání všech hodnot, klikněte na **Uložit** toocreate hello rozhraní API a hello související operace.</span><span class="sxs-lookup"><span data-stu-id="46e8a-130">Once all values are entered, click **Save** toocreate hello API and hello associated operations.</span></span> 

> [!NOTE]
> <span data-ttu-id="46e8a-131">Kurz import základní kalkulačky rozhraní API ve formátu Swagger, najdete v části [Správa vašeho prvního rozhraní API v Azure API Management](api-management-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="46e8a-131">For a tutorial of importing a basic calculator API in Swagger format, see [Manage your first API in Azure API Management](api-management-get-started.md).</span></span>
> 
> 

## <span data-ttu-id="46e8a-132"><a name="export-api"></a> Export rozhraní API</span><span class="sxs-lookup"><span data-stu-id="46e8a-132"><a name="export-api"> </a> Export an API</span></span>
<span data-ttu-id="46e8a-133">V tooimporting přidání nových rozhraní API, můžete exportovat hello Definice vaše rozhraní API z portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="46e8a-133">In addition tooimporting new APIs, you can export hello definitions of your APIs from hello publisher portal.</span></span> <span data-ttu-id="46e8a-134">toodo proto, klikněte na **Export rozhraní API** z hello **kartu Souhrn** z vaší **rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="46e8a-134">toodo so, click **Export API** from hello **Summary tab** of your **API**.</span></span>

![Export rozhraní API][api-management-export-api]

<span data-ttu-id="46e8a-136">Rozhraní API je možné exportovat pomocí WADL nebo Swagger.</span><span class="sxs-lookup"><span data-stu-id="46e8a-136">APIs can be exported using WADL or Swagger.</span></span> <span data-ttu-id="46e8a-137">Vyberte požadovaný formát hello, klikněte na tlačítko **Uložit**a vyberte umístění hello v souboru které toosave hello.</span><span class="sxs-lookup"><span data-stu-id="46e8a-137">Select hello desired format, click **Save**, and choose hello location in which toosave hello file.</span></span>

![Formát exportu rozhraní API][api-management-export-api-format]

## <span data-ttu-id="46e8a-139"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="46e8a-139"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="46e8a-140">Po vytvoření rozhraní API a operace hello importovat, můžete zkontrolovat a nakonfigurujte jakákoli další nastavení, přidejte hello rozhraní API tooa produktu a publikovat, aby bylo k dispozici pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="46e8a-140">Once an API is created and hello operations imported, you can review and configure any additional settings, add hello API tooa Product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="46e8a-141">Další informace najdete v tématu hello následující příručky.</span><span class="sxs-lookup"><span data-stu-id="46e8a-141">For more information, see hello following guides.</span></span>

* <span data-ttu-id="46e8a-142">[Jak nastavení tooconfigure rozhraní API][How tooconfigure API settings]</span><span class="sxs-lookup"><span data-stu-id="46e8a-142">[How tooconfigure API settings][How tooconfigure API settings]</span></span>
* <span data-ttu-id="46e8a-143">[Jak toocreate a publikování produktu][How toocreate and publish a product]</span><span class="sxs-lookup"><span data-stu-id="46e8a-143">[How toocreate and publish a product][How toocreate and publish a product]</span></span>

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

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocreate APIs]: api-management-howto-create-apis.md
[How tooconfigure API settings]: api-management-howto-create-apis.md#configure-api-settings
