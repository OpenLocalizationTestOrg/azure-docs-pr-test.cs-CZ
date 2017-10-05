---
title: "Přehled služby Azure Media Services | Dokumentace Microsoftu"
description: "Toto téma nabízí přehled Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/04/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 2a175aed40b9775d9a4de6877eb3467b43819568
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-media-services-overview"></a><span data-ttu-id="afddf-103">Přehled služby Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="afddf-103">Azure Media Services overview</span></span> 

<span data-ttu-id="afddf-104">Microsoft Azure Media Services je rozšiřitelná cloudová platforma, která vývojářům umožňuje vytvářet škálovatelné aplikace pro správu a doručování médií.</span><span class="sxs-lookup"><span data-stu-id="afddf-104">Microsoft Azure Media Services is an extensible cloud-based platform that enables developers to build scalable media management and delivery applications.</span></span> <span data-ttu-id="afddf-105">Služba Media Services využívá rozhraní REST API, které vám umožní bezpečně nahrávat, ukládat, kódovat a balit obsah (video nebo zvuk) doručovaný na vyžádání i v živě streamovaný různým klientům (například do televizí, počítačů a mobilních zařízení).</span><span class="sxs-lookup"><span data-stu-id="afddf-105">Media Services is based on REST APIs that enable you to securely upload, store, encode, and package video or audio content for both on-demand and live streaming delivery to various clients (for example, TV, PC, and mobile devices).</span></span>

<span data-ttu-id="afddf-106">Pomocí Media Services můžete vytvářet pracovní postupy od začátku až do konce.</span><span class="sxs-lookup"><span data-stu-id="afddf-106">You can build end-to-end workflows using entirely Media Services.</span></span> <span data-ttu-id="afddf-107">V některých částech pracovního postupu můžete použít komponenty třetích stran.</span><span class="sxs-lookup"><span data-stu-id="afddf-107">You can also choose to use third-party components for some parts of your workflow.</span></span> <span data-ttu-id="afddf-108">Můžete například kódovat pomocí kodéru třetí strany.</span><span class="sxs-lookup"><span data-stu-id="afddf-108">For example, encode using a third-party encoder.</span></span> <span data-ttu-id="afddf-109">Potom obsah nahrajete, zabezpečíte, zabalíte a doručíte pomocí služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="afddf-109">Then, upload, protect, package, deliver using Media Services.</span></span>

<span data-ttu-id="afddf-110">Svůj obsah můžete streamovat živě nebo doručovat na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="afddf-110">You can choose to stream your content live or deliver content on-demand.</span></span> <span data-ttu-id="afddf-111">Téma obsahuje i odkazy na další související témata.</span><span class="sxs-lookup"><span data-stu-id="afddf-111">The topic also links to other relevant topics.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="afddf-112">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="afddf-112">Media Services learning paths</span></span>
<span data-ttu-id="afddf-113">Mapu kurzů k AMS můžete zobrazit tady:</span><span class="sxs-lookup"><span data-stu-id="afddf-113">You can view AMS learning paths here:</span></span>

* [<span data-ttu-id="afddf-114">Pracovní postup živého streamování AMS</span><span class="sxs-lookup"><span data-stu-id="afddf-114">AMS Live Streaming Workflow</span></span>](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [<span data-ttu-id="afddf-115">Pracovní postup streamování AMS na vyžádání</span><span class="sxs-lookup"><span data-stu-id="afddf-115">AMS on-Demand Streaming Workflow</span></span>](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a><span data-ttu-id="afddf-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="afddf-116">Prerequisites</span></span>

<span data-ttu-id="afddf-117">Pokud chcete začít používat Azure Media Services, potřebujete následující:</span><span class="sxs-lookup"><span data-stu-id="afddf-117">To start using Azure Media Services, you should have the following:</span></span>

* <span data-ttu-id="afddf-118">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="afddf-118">An Azure account.</span></span> <span data-ttu-id="afddf-119">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="afddf-119">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="afddf-120">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="afddf-120">For details, see [Azure Free Trial](https://azure.microsoft.com).</span></span>
* <span data-ttu-id="afddf-121">Účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="afddf-121">An Azure Media Services account.</span></span> <span data-ttu-id="afddf-122">Další informace najdete v článku o [vytvoření účtu](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="afddf-122">For more information, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="afddf-123">(Volitelné) Nastavte vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="afddf-123">(Optional) Set up development environment.</span></span> <span data-ttu-id="afddf-124">Jako vývojové prostředí si zvolte .NET nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="afddf-124">Choose .NET or REST API for your development environment.</span></span> <span data-ttu-id="afddf-125">Další informace najdete v článku o [nastavení prostředí](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="afddf-125">For more information, see [Set up environment](media-services-dotnet-how-to-use.md).</span></span>

    <span data-ttu-id="afddf-126">Seznamte se také s postupem [připojení prostřednictvím kódu programu k rozhraní API pro AMS](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="afddf-126">Also, learn how to [connect  programmatically to AMS API](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
* <span data-ttu-id="afddf-127">Koncový bod streamování Standard nebo Premium ve spuštěném stavu.</span><span class="sxs-lookup"><span data-stu-id="afddf-127">A standard or premium streaming endpoint in started state.</span></span>  <span data-ttu-id="afddf-128">Další informace najdete v článku o [správě koncových bodů streamování](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="afddf-128">For more information, see [Managing streaming endpoints](media-services-portal-manage-streaming-endpoints.md)</span></span>

## <a name="sdks-and-tools"></a><span data-ttu-id="afddf-129">Sady SDK a nástroje</span><span class="sxs-lookup"><span data-stu-id="afddf-129">SDKs and tools</span></span>

<span data-ttu-id="afddf-130">Pokud chcete vytvořit řešení Media Services, můžete použít následující pomůcky:</span><span class="sxs-lookup"><span data-stu-id="afddf-130">To build Media Services solutions, you can use:</span></span>

* [<span data-ttu-id="afddf-131">Rozhraní REST API služby Media Services</span><span class="sxs-lookup"><span data-stu-id="afddf-131">Media Services REST API</span></span>](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* <span data-ttu-id="afddf-132">Jednu z dostupných klientských sad SDK:</span><span class="sxs-lookup"><span data-stu-id="afddf-132">One of the available client SDKs:</span></span>
    * <span data-ttu-id="afddf-133">[Azure Media Services SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services),</span><span class="sxs-lookup"><span data-stu-id="afddf-133">[Azure Media Services SDK for .NET](https://github.com/Azure/azure-sdk-for-media-services),</span></span>
    * <span data-ttu-id="afddf-134">[Azure SDK pro Javu](https://github.com/Azure/azure-sdk-for-java),</span><span class="sxs-lookup"><span data-stu-id="afddf-134">[Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java),</span></span>
    * <span data-ttu-id="afddf-135">[Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),</span><span class="sxs-lookup"><span data-stu-id="afddf-135">[Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),</span></span>
    * <span data-ttu-id="afddf-136">[Azure Media Services pro Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Jedná se o verzi sady SDK, kterou nevytvořil Microsoft.</span><span class="sxs-lookup"><span data-stu-id="afddf-136">[Azure Media Services for Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (This is a non-Microsoft version of a Node.js SDK.</span></span> <span data-ttu-id="afddf-137">Spravuje ji komunita a aktuálně nemá 100% pokrytí rozhraní API pro AMS.)</span><span class="sxs-lookup"><span data-stu-id="afddf-137">It is maintained by a community and currently does not have a 100% coverage of the AMS APIs).</span></span>
* <span data-ttu-id="afddf-138">Existující nástroje:</span><span class="sxs-lookup"><span data-stu-id="afddf-138">Existing tools:</span></span>
    * [<span data-ttu-id="afddf-139">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="afddf-139">Azure portal</span></span>](https://portal.azure.com/)
    * <span data-ttu-id="afddf-140">[Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) je aplikace napsaná v jazyce Winforms/C# pro Windows.)</span><span class="sxs-lookup"><span data-stu-id="afddf-140">[Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) is a Winforms/C# application for Windows)</span></span>

## <a name="concepts-and-overview"></a><span data-ttu-id="afddf-141">Koncepty a přehled</span><span class="sxs-lookup"><span data-stu-id="afddf-141">Concepts and overview</span></span>
<span data-ttu-id="afddf-142">Informace o konceptech Azure Media Services najdete v článku [Koncepty](media-services-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="afddf-142">For Azure Media Services concepts, see [Concepts](media-services-concepts.md).</span></span>

<span data-ttu-id="afddf-143">Řada návodů, které vás seznámí se všemi hlavními součástmi Azure Media Services, najdete v článku o [podrobných kurzech pro Azure Media Services](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series).</span><span class="sxs-lookup"><span data-stu-id="afddf-143">For a how-to series that introduces you to all the main components of Azure Media Services, see [Azure Media Services Step-by-Step tutorials](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series).</span></span> <span data-ttu-id="afddf-144">Tato řada návodů obsahuje přehled konceptů a využívá nástroj AMSE k předvádění úloh AMS.</span><span class="sxs-lookup"><span data-stu-id="afddf-144">This series has a great overview of concepts and it uses the AMSE tool to demonstrate AMS tasks.</span></span> <span data-ttu-id="afddf-145">Nástroj AMSE je nástrojem systému Windows.</span><span class="sxs-lookup"><span data-stu-id="afddf-145">AMSE tool is a Windows tool.</span></span> <span data-ttu-id="afddf-146">Tento nástroj podporuje většinu úloh, které můžete provádět prostřednictvím kódu programu se sadami [AMS SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK pro Javu](https://github.com/Azure/azure-sdk-for-java), nebo  [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).</span><span class="sxs-lookup"><span data-stu-id="afddf-146">This tool supports most of the tasks you can achieve programmatically with [AMS SDK for .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java), or  [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).</span></span>

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a><span data-ttu-id="afddf-147">Podporované scénáře a dostupnost služby Media Services v datových centrech</span><span class="sxs-lookup"><span data-stu-id="afddf-147">Supported scenarios and availability of Media Services across data centers</span></span>

<span data-ttu-id="afddf-148">Podrobné informace najdete v tématu [Scénáře a dostupnost funkcí a služeb AMS v datových centrech](scenarios-and-availability.md).</span><span class="sxs-lookup"><span data-stu-id="afddf-148">For detailed information, see [AMS scenarios and availability of features and services across data centers](scenarios-and-availability.md).</span></span>

## <a name="service-level-agreement-sla"></a><span data-ttu-id="afddf-149">Smlouva SLA</span><span class="sxs-lookup"><span data-stu-id="afddf-149">Service Level Agreement (SLA)</span></span>

* <span data-ttu-id="afddf-150">V případě služby Media Services Encoding zaručujeme 99,9% dostupnosti transakcí REST API.</span><span class="sxs-lookup"><span data-stu-id="afddf-150">For Media Services Encoding, we guarantee 99.9% availability of REST API transactions.</span></span>
* <span data-ttu-id="afddf-151">V případě streamování budeme úspěšně obsluhovat požadavky se zárukou 99,9% dostupnosti pro existující mediální obsah, pokud jste zakoupili koncový bod streamování Standard nebo Premium.</span><span class="sxs-lookup"><span data-stu-id="afddf-151">For Streaming, we will successfully service requests with a 99.9% availability guarantee for existing media content when a standard or premium streaming endpoint is purchased.</span></span>
* <span data-ttu-id="afddf-152">V případě živých kanálů zaručujeme externí konektivitu minimálně po 99,9 % času.</span><span class="sxs-lookup"><span data-stu-id="afddf-152">For Live Channels, we guarantee that running Channels will have external connectivity at least 99.9% of the time.</span></span>
* <span data-ttu-id="afddf-153">V případě Content Protection zaručujeme úspěšné plnění klíčových požadavků minimálně po 99,9 % času.</span><span class="sxs-lookup"><span data-stu-id="afddf-153">For Content Protection, we guarantee that we will successfully fulfill key requests at least 99.9% of the time.</span></span>
* <span data-ttu-id="afddf-154">V případě indexeru po 99,9 % času úspěšně obsluhujeme požadavky úloh indexeru, které zpracovává jednotka rezervovaná pro kódování.</span><span class="sxs-lookup"><span data-stu-id="afddf-154">For Indexer, we will successfully service Indexer Task requests processed with an Encoding Reserved Unit 99.9% of the time.</span></span>

<span data-ttu-id="afddf-155">Další informace najdete v článku [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="afddf-155">For more information, see [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).</span></span>

<span data-ttu-id="afddf-156">Informace o dostupnosti v datových centrech najdete v části [Dostupnost](scenarios-and-availability.md#availability).</span><span class="sxs-lookup"><span data-stu-id="afddf-156">For information about availability in datacenters, see the [Avaiability](scenarios-and-availability.md#availability) section.</span></span>

## <a name="support"></a><span data-ttu-id="afddf-157">Podpora</span><span class="sxs-lookup"><span data-stu-id="afddf-157">Support</span></span>

<span data-ttu-id="afddf-158">[Podpora Azure](https://azure.microsoft.com/support/options/) nabízí možnosti odborné pomoci pro Azure včetně služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="afddf-158">[Azure Support](https://azure.microsoft.com/support/options/) provides support options for Azure, including Media Services.</span></span>

## <a name="provide-feedback"></a><span data-ttu-id="afddf-159">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="afddf-159">Provide feedback</span></span>

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
