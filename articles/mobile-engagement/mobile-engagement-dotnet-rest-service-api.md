---
title: "hello aaaUsing tooaccess REST API rozhraní API služby Azure Mobile Engagement"
description: "Popisuje, jak toouse hello tooaccess Mobile Engagement REST API rozhraní API služby Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: 
ms.assetid: e8df4897-55ee-45df-b41e-ff187e3d9d12
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 315761299a42df6f65e81df20e632f9713844b0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-rest-tooaccess-azure-mobile-engagement-service-apis"></a><span data-ttu-id="936c8-103">Pomocí REST tooaccess rozhraní API služby Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="936c8-103">Using REST tooaccess Azure Mobile Engagement Service APIs</span></span>
<span data-ttu-id="936c8-104">Azure Mobile Engagement poskytuje hello [REST API služby Azure Mobile Engagement](https://msdn.microsoft.com/library/azure/mt683754.aspx) pro vás toomanage zařízení kampaní Reach/nabízených atd.</span><span class="sxs-lookup"><span data-stu-id="936c8-104">Azure Mobile Engagement provides hello [Azure Mobile Engagement REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) for you toomanage Devices, Reach/Push campaigns etc.</span></span>

> [!NOTE]
> <span data-ttu-id="936c8-105">Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků.</span><span class="sxs-lookup"><span data-stu-id="936c8-105">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="936c8-106">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="936c8-106">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="936c8-107">Pokud nechcete, aby toouse hello rozhraní REST API přímo, poskytujeme také [soubor Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , které můžete použít s nástroje toogenerate SDK pro preferovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="936c8-107">If you do not want toouse hello REST APIs directly, we also provide a [Swagger file](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) that you can use with tools toogenerate SDKs for your preferred language.</span></span> <span data-ttu-id="936c8-108">Doporučujeme používat hello [AutoRest](https://github.com/Azure/AutoRest) nástroj toogenerate vaší sady SDK z našich souboru Swagger.</span><span class="sxs-lookup"><span data-stu-id="936c8-108">We recommend using hello [AutoRest](https://github.com/Azure/AutoRest) tool toogenerate your SDK from our Swagger file.</span></span> <span data-ttu-id="936c8-109">Vytvořili jsme .NET SDK podobným způsobem, což vám umožní toointeract s Tato rozhraní API pomocí obálku C# a nemáte toodo hello ověřování tokenu vyjednávání a aktualizujte sami.</span><span class="sxs-lookup"><span data-stu-id="936c8-109">We have created a .NET SDK in a similar manner which allows you toointeract with these APIs using a C# wrapper and you don't have toodo hello authentication token negotiation and refresh yourself.</span></span> <span data-ttu-id="936c8-110">V tématu [služby rozhraní API .NET SDK – ukázka](mobile-engagement-dotnet-sdk-service-api.md) toolearn jak toouse hello .net SDK pro rozhraní API</span><span class="sxs-lookup"><span data-stu-id="936c8-110">See [Service API .NET SDK Sample](mobile-engagement-dotnet-sdk-service-api.md) toolearn how toouse hello .net SDK for API</span></span>
