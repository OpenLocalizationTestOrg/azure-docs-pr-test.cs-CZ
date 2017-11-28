---
title: "Přehled služby Media Services aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: 81f9f4d9ff75effea30c10fd09449e9d2025f377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-overview"></a><span data-ttu-id="a45ba-103">Přehled služby Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="a45ba-103">Azure Media Services overview</span></span> 

<span data-ttu-id="a45ba-104">Microsoft Azure Media Services je rozšiřitelná platforma založená na cloudu, která umožňuje vývojářům toobuild škálovatelné média správu a doručování aplikacím.</span><span class="sxs-lookup"><span data-stu-id="a45ba-104">Microsoft Azure Media Services is an extensible cloud-based platform that enables developers toobuild scalable media management and delivery applications.</span></span> <span data-ttu-id="a45ba-105">Služba Media Services je založena na rozhraní REST API, která umožňují toosecurely nahrávání, ukládání, kódování a video nebo zvuk obsah balíčku pro vysílání na vyžádání i živé streamování doručení toovarious klienty (například TV, počítače a mobilní zařízení).</span><span class="sxs-lookup"><span data-stu-id="a45ba-105">Media Services is based on REST APIs that enable you toosecurely upload, store, encode, and package video or audio content for both on-demand and live streaming delivery toovarious clients (for example, TV, PC, and mobile devices).</span></span>

<span data-ttu-id="a45ba-106">Pomocí Media Services můžete vytvářet pracovní postupy od začátku až do konce.</span><span class="sxs-lookup"><span data-stu-id="a45ba-106">You can build end-to-end workflows using entirely Media Services.</span></span> <span data-ttu-id="a45ba-107">Můžete také toouse komponenty třetích stran pro některé části pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="a45ba-107">You can also choose toouse third-party components for some parts of your workflow.</span></span> <span data-ttu-id="a45ba-108">Můžete například kódovat pomocí kodéru třetí strany.</span><span class="sxs-lookup"><span data-stu-id="a45ba-108">For example, encode using a third-party encoder.</span></span> <span data-ttu-id="a45ba-109">Potom obsah nahrajete, zabezpečíte, zabalíte a doručíte pomocí služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="a45ba-109">Then, upload, protect, package, deliver using Media Services.</span></span>

<span data-ttu-id="a45ba-110">Můžete vybrat toostream obsah za provozu nebo doručování obsahu na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="a45ba-110">You can choose toostream your content live or deliver content on-demand.</span></span> <span data-ttu-id="a45ba-111">Hello téma obsahuje i odkazy tooother související témata.</span><span class="sxs-lookup"><span data-stu-id="a45ba-111">hello topic also links tooother relevant topics.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="a45ba-112">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="a45ba-112">Media Services learning paths</span></span>
<span data-ttu-id="a45ba-113">Mapu kurzů k AMS můžete zobrazit tady:</span><span class="sxs-lookup"><span data-stu-id="a45ba-113">You can view AMS learning paths here:</span></span>

* [<span data-ttu-id="a45ba-114">Pracovní postup živého streamování AMS</span><span class="sxs-lookup"><span data-stu-id="a45ba-114">AMS Live Streaming Workflow</span></span>](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [<span data-ttu-id="a45ba-115">Pracovní postup streamování AMS na vyžádání</span><span class="sxs-lookup"><span data-stu-id="a45ba-115">AMS on-Demand Streaming Workflow</span></span>](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a><span data-ttu-id="a45ba-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a45ba-116">Prerequisites</span></span>

<span data-ttu-id="a45ba-117">toostart využívající Azure Media Services, měli byste mít hello následující:</span><span class="sxs-lookup"><span data-stu-id="a45ba-117">toostart using Azure Media Services, you should have hello following:</span></span>

* <span data-ttu-id="a45ba-118">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a45ba-118">An Azure account.</span></span> <span data-ttu-id="a45ba-119">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="a45ba-119">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="a45ba-120">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="a45ba-120">For details, see [Azure Free Trial](https://azure.microsoft.com).</span></span>
* <span data-ttu-id="a45ba-121">Účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="a45ba-121">An Azure Media Services account.</span></span> <span data-ttu-id="a45ba-122">Další informace najdete v článku o [vytvoření účtu](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="a45ba-122">For more information, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="a45ba-123">(Volitelné) Nastavte vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="a45ba-123">(Optional) Set up development environment.</span></span> <span data-ttu-id="a45ba-124">Jako vývojové prostředí si zvolte .NET nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="a45ba-124">Choose .NET or REST API for your development environment.</span></span> <span data-ttu-id="a45ba-125">Další informace najdete v článku o [nastavení prostředí](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="a45ba-125">For more information, see [Set up environment](media-services-dotnet-how-to-use.md).</span></span>

    <span data-ttu-id="a45ba-126">Také zjistěte, jak příliš[připojení prostřednictvím kódu programu tooAMS rozhraní API](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="a45ba-126">Also, learn how too[connect  programmatically tooAMS API](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
* <span data-ttu-id="a45ba-127">Koncový bod streamování Standard nebo Premium ve spuštěném stavu.</span><span class="sxs-lookup"><span data-stu-id="a45ba-127">A standard or premium streaming endpoint in started state.</span></span>  <span data-ttu-id="a45ba-128">Další informace najdete v článku o [správě koncových bodů streamování](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="a45ba-128">For more information, see [Managing streaming endpoints](media-services-portal-manage-streaming-endpoints.md)</span></span>

## <a name="sdks-and-tools"></a><span data-ttu-id="a45ba-129">Sady SDK a nástroje</span><span class="sxs-lookup"><span data-stu-id="a45ba-129">SDKs and tools</span></span>

<span data-ttu-id="a45ba-130">toobuild řešení Media Services, můžete:</span><span class="sxs-lookup"><span data-stu-id="a45ba-130">toobuild Media Services solutions, you can use:</span></span>

* [<span data-ttu-id="a45ba-131">Rozhraní REST API služby Media Services</span><span class="sxs-lookup"><span data-stu-id="a45ba-131">Media Services REST API</span></span>](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* <span data-ttu-id="a45ba-132">Jeden z dostupných klientů hello sady SDK:</span><span class="sxs-lookup"><span data-stu-id="a45ba-132">One of hello available client SDKs:</span></span>
    * <span data-ttu-id="a45ba-133">[Azure Media Services SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services),</span><span class="sxs-lookup"><span data-stu-id="a45ba-133">[Azure Media Services SDK for .NET](https://github.com/Azure/azure-sdk-for-media-services),</span></span>
    * <span data-ttu-id="a45ba-134">[Azure SDK pro Javu](https://github.com/Azure/azure-sdk-for-java),</span><span class="sxs-lookup"><span data-stu-id="a45ba-134">[Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java),</span></span>
    * <span data-ttu-id="a45ba-135">[Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),</span><span class="sxs-lookup"><span data-stu-id="a45ba-135">[Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),</span></span>
    * <span data-ttu-id="a45ba-136">[Azure Media Services pro Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Jedná se o verzi sady SDK, kterou nevytvořil Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a45ba-136">[Azure Media Services for Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (This is a non-Microsoft version of a Node.js SDK.</span></span> <span data-ttu-id="a45ba-137">Ho je možný díky komunita a aktuálně nemá 100 % pokrytí rozhraní API pro AMS hello).</span><span class="sxs-lookup"><span data-stu-id="a45ba-137">It is maintained by a community and currently does not have a 100% coverage of hello AMS APIs).</span></span>
* <span data-ttu-id="a45ba-138">Existující nástroje:</span><span class="sxs-lookup"><span data-stu-id="a45ba-138">Existing tools:</span></span>
    * [<span data-ttu-id="a45ba-139">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a45ba-139">Azure portal</span></span>](https://portal.azure.com/)
    * <span data-ttu-id="a45ba-140">[Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) je aplikace napsaná v jazyce Winforms/C# pro Windows.)</span><span class="sxs-lookup"><span data-stu-id="a45ba-140">[Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) is a Winforms/C# application for Windows)</span></span>

## <a name="concepts-and-overview"></a><span data-ttu-id="a45ba-141">Koncepty a přehled</span><span class="sxs-lookup"><span data-stu-id="a45ba-141">Concepts and overview</span></span>
<span data-ttu-id="a45ba-142">Informace o konceptech Azure Media Services najdete v článku [Koncepty](media-services-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="a45ba-142">For Azure Media Services concepts, see [Concepts](media-services-concepts.md).</span></span>

<span data-ttu-id="a45ba-143">Jak – tooseries vás seznámí s tooall hello hlavními součástmi Azure Media Services, najdete v části [Azure Media Services podrobných kurzech](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series).</span><span class="sxs-lookup"><span data-stu-id="a45ba-143">For a how-tooseries that introduces you tooall hello main components of Azure Media Services, see [Azure Media Services Step-by-Step tutorials](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series).</span></span> <span data-ttu-id="a45ba-144">Tato návody obsahují skvělý Přehled konceptů a používá úlohy toodemonstrate AMS nástroj AMSE hello.</span><span class="sxs-lookup"><span data-stu-id="a45ba-144">This series has a great overview of concepts and it uses hello AMSE tool toodemonstrate AMS tasks.</span></span> <span data-ttu-id="a45ba-145">Nástroj AMSE je nástrojem systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a45ba-145">AMSE tool is a Windows tool.</span></span> <span data-ttu-id="a45ba-146">Tento nástroj podporuje většinu úloh hello můžete dosáhnout prostřednictvím kódu programu s [AMS SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK pro jazyk Java](https://github.com/Azure/azure-sdk-for-java), nebo [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).</span><span class="sxs-lookup"><span data-stu-id="a45ba-146">This tool supports most of hello tasks you can achieve programmatically with [AMS SDK for .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java), or  [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).</span></span>

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a><span data-ttu-id="a45ba-147">Podporované scénáře a dostupnost služby Media Services v datových centrech</span><span class="sxs-lookup"><span data-stu-id="a45ba-147">Supported scenarios and availability of Media Services across data centers</span></span>

<span data-ttu-id="a45ba-148">Podrobné informace najdete v tématu [Scénáře a dostupnost funkcí a služeb AMS v datových centrech](scenarios-and-availability.md).</span><span class="sxs-lookup"><span data-stu-id="a45ba-148">For detailed information, see [AMS scenarios and availability of features and services across data centers](scenarios-and-availability.md).</span></span>

## <a name="service-level-agreement-sla"></a><span data-ttu-id="a45ba-149">Smlouva SLA</span><span class="sxs-lookup"><span data-stu-id="a45ba-149">Service Level Agreement (SLA)</span></span>

* <span data-ttu-id="a45ba-150">V případě služby Media Services Encoding zaručujeme 99,9% dostupnosti transakcí REST API.</span><span class="sxs-lookup"><span data-stu-id="a45ba-150">For Media Services Encoding, we guarantee 99.9% availability of REST API transactions.</span></span>
* <span data-ttu-id="a45ba-151">V případě streamování budeme úspěšně obsluhovat požadavky se zárukou 99,9% dostupnosti pro existující mediální obsah, pokud jste zakoupili koncový bod streamování Standard nebo Premium.</span><span class="sxs-lookup"><span data-stu-id="a45ba-151">For Streaming, we will successfully service requests with a 99.9% availability guarantee for existing media content when a standard or premium streaming endpoint is purchased.</span></span>
* <span data-ttu-id="a45ba-152">Živých kanálů Zaručujeme, že spuštění kanály bude mít externí konektivitu minimálně 99,9 % času hello.</span><span class="sxs-lookup"><span data-stu-id="a45ba-152">For Live Channels, we guarantee that running Channels will have external connectivity at least 99.9% of hello time.</span></span>
* <span data-ttu-id="a45ba-153">V případě Content Protection Zaručujeme, že jsme bude úspěšné plnění klíčových požadavků minimálně 99,9 % času hello.</span><span class="sxs-lookup"><span data-stu-id="a45ba-153">For Content Protection, we guarantee that we will successfully fulfill key requests at least 99.9% of hello time.</span></span>
* <span data-ttu-id="a45ba-154">Pro Indexer, úspěšně obsluhujeme požadavky úloh indexeru zpracovat rezervovaná pro kódování jednotky 99,9 % času hello.</span><span class="sxs-lookup"><span data-stu-id="a45ba-154">For Indexer, we will successfully service Indexer Task requests processed with an Encoding Reserved Unit 99.9% of hello time.</span></span>

<span data-ttu-id="a45ba-155">Další informace najdete v článku [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="a45ba-155">For more information, see [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).</span></span>

<span data-ttu-id="a45ba-156">Informace o dostupnosti v datových centrech najdete v tématu hello [Avaiability](scenarios-and-availability.md#availability) části.</span><span class="sxs-lookup"><span data-stu-id="a45ba-156">For information about availability in datacenters, see hello [Avaiability](scenarios-and-availability.md#availability) section.</span></span>

## <a name="support"></a><span data-ttu-id="a45ba-157">Podpora</span><span class="sxs-lookup"><span data-stu-id="a45ba-157">Support</span></span>

<span data-ttu-id="a45ba-158">[Podpora Azure](https://azure.microsoft.com/support/options/) nabízí možnosti odborné pomoci pro Azure včetně služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="a45ba-158">[Azure Support](https://azure.microsoft.com/support/options/) provides support options for Azure, including Media Services.</span></span>

## <a name="provide-feedback"></a><span data-ttu-id="a45ba-159">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="a45ba-159">Provide feedback</span></span>

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
