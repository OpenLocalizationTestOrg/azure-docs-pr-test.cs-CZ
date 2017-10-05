---
title: "Pomocí rozhraní REST API pro přístup k rozhraní API služby Azure Mobile Engagement"
description: "Popisuje, jak používat Mobile Engagement REST API pro přístup k rozhraní API služby Azure Mobile Engagement"
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
ms.openlocfilehash: 4b21bca6fee7012ce1dba96950ae075eded414f8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a><span data-ttu-id="c4f97-103">Pomocí REST pro přístup k rozhraní API služby Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="c4f97-103">Using REST to access Azure Mobile Engagement Service APIs</span></span>
<span data-ttu-id="c4f97-104">Azure Mobile Engagement poskytuje [REST API služby Azure Mobile Engagement](https://msdn.microsoft.com/library/azure/mt683754.aspx) pro správu zařízení, kampaní Reach/nabízených atd.</span><span class="sxs-lookup"><span data-stu-id="c4f97-104">Azure Mobile Engagement provides the [Azure Mobile Engagement REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) for you to manage Devices, Reach/Push campaigns etc.</span></span>

> [!NOTE]
> <span data-ttu-id="c4f97-105">Službu Azure Mobile Engagement vyřadíme z provozu v březnu 2018. V současnosti je dostupná jenom pro stávající zákazníky.</span><span class="sxs-lookup"><span data-stu-id="c4f97-105">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="c4f97-106">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="c4f97-106">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="c4f97-107">Pokud nechcete používat rozhraní REST API přímo, poskytujeme také [soubor Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , můžete pomocí nástrojů můžete vygenerovat pomocí sady SDK pro preferovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="c4f97-107">If you do not want to use the REST APIs directly, we also provide a [Swagger file](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) that you can use with tools to generate SDKs for your preferred language.</span></span> <span data-ttu-id="c4f97-108">Doporučujeme používat [AutoRest](https://github.com/Azure/AutoRest) nástroj pro generování vaší sady SDK z našich souboru Swagger.</span><span class="sxs-lookup"><span data-stu-id="c4f97-108">We recommend using the [AutoRest](https://github.com/Azure/AutoRest) tool to generate your SDK from our Swagger file.</span></span> <span data-ttu-id="c4f97-109">Vytvořili jsme .NET SDK podobným způsobem, který umožňuje pracovat s Tato rozhraní API pomocí obálku C# a nemusíte dělat vyjednávání tokenu ověřování a aktualizujte sami.</span><span class="sxs-lookup"><span data-stu-id="c4f97-109">We have created a .NET SDK in a similar manner which allows you to interact with these APIs using a C# wrapper and you don't have to do the authentication token negotiation and refresh yourself.</span></span> <span data-ttu-id="c4f97-110">V tématu [služby rozhraní API .NET SDK – ukázka](mobile-engagement-dotnet-sdk-service-api.md) se dozvíte, jak používat .net SDK pro rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c4f97-110">See [Service API .NET SDK Sample](mobile-engagement-dotnet-sdk-service-api.md) to learn how to use the .net SDK for API</span></span>
