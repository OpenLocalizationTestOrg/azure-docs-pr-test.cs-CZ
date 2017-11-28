---
title: "aaaHow tooadd prostředí Statistika řady čas Azure IoT Hub událostí zdroj tooyour | Microsoft Docs"
description: "Tento kurz popisuje, jak tooadd událost zdroj, který je připojen tooan IoT Hub tooyour časové řady Přehled prostředí"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: c626f9653d1c012360120fa9fc3d211d7d5beb5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-an-iot-hub-event-source"></a><span data-ttu-id="71f6b-103">Jak tooadd zdroje událostí služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="71f6b-103">How tooadd an IoT Hub event source</span></span>

<span data-ttu-id="71f6b-104">Tento kurz popisuje, jak toouse hello Azure portálu tooadd zdroje událostí, který čte z prostředí IoT Hub tooyour Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="71f6b-104">This tutorial covers how toouse hello Azure portal tooadd an event source that reads from an IoT Hub tooyour Time Series Insights environment.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71f6b-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="71f6b-105">Prerequisites</span></span>

<span data-ttu-id="71f6b-106">Vytvoření služby IoT Hub a jsou zápis tooit události.</span><span class="sxs-lookup"><span data-stu-id="71f6b-106">You have created an IoT Hub and are writing events tooit.</span></span> <span data-ttu-id="71f6b-107">Další informace o centra IoT najdete v tématu <https://azure.microsoft.com/services/iot-hub/></span><span class="sxs-lookup"><span data-stu-id="71f6b-107">For more information on IoT Hubs, see <https://azure.microsoft.com/services/iot-hub/></span></span>

> <span data-ttu-id="71f6b-108">[Skupiny příjemců] Každý zdroj události Statistika časové řady musí toohave vlastní vyhrazenou skupinu spotřebitelů která není sdílena s dalšími uživateli.</span><span class="sxs-lookup"><span data-stu-id="71f6b-108">[Consumer Groups] Each Time Series Insights event source needs toohave its own dedicated consumer group that is not shared with any other consumers.</span></span> <span data-ttu-id="71f6b-109">Pokud využívat více čtenářů událostí z hello stejnou skupinu uživatelů, pravděpodobně toosee selhání jsou všechny nástroje pro čtení.</span><span class="sxs-lookup"><span data-stu-id="71f6b-109">If multiple readers consume events from hello same consumer group, all readers are likely toosee failures.</span></span> <span data-ttu-id="71f6b-110">Podrobnosti najdete v tématu hello [Příručka vývojáře pro službu IoT Hub](../iot-hub/iot-hub-devguide.md).</span><span class="sxs-lookup"><span data-stu-id="71f6b-110">For details, see hello [IoT Hub developer guide](../iot-hub/iot-hub-devguide.md).</span></span>

## <a name="choose-an-import-option"></a><span data-ttu-id="71f6b-111">Zvolte možnost importovat</span><span class="sxs-lookup"><span data-stu-id="71f6b-111">Choose an Import option</span></span>

<span data-ttu-id="71f6b-112">Hello nastavení pro zdroj události hello lze zadat ručně nebo centrum IoT je možné vybrat z centra IoT hello, které jsou k dispozici tooyou.</span><span class="sxs-lookup"><span data-stu-id="71f6b-112">hello settings for hello event source can be entered manually or an IoT hub can be selected from hello IoT hubs that are available tooyou.</span></span>
<span data-ttu-id="71f6b-113">V hello **možnost importu** selektor, vyberte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="71f6b-113">In hello **Import Option** selector, choose one of hello following options:</span></span>

* <span data-ttu-id="71f6b-114">Zadejte nastavení centra IoT ručně</span><span class="sxs-lookup"><span data-stu-id="71f6b-114">Provide IoT Hub settings manually</span></span>
* <span data-ttu-id="71f6b-115">Pomocí služby IoT Hub z dostupných předplatných</span><span class="sxs-lookup"><span data-stu-id="71f6b-115">Use IoT Hub from available subscriptions</span></span>

### <a name="select-an-available-iot-hub"></a><span data-ttu-id="71f6b-116">Vyberte dostupné služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="71f6b-116">Select an available IoT Hub</span></span>

<span data-ttu-id="71f6b-117">Hello následující tabulka popisuje jednotlivé možnosti na kartě hello nový zdroj událostí s jeho popis při výběru Centrum IoT k dispozici jako zdroje událostí:</span><span class="sxs-lookup"><span data-stu-id="71f6b-117">hello following table explains each option in hello New Event Source tab with its description when selecting an available IoT Hub as an event source:</span></span>

| <span data-ttu-id="71f6b-118">NÁZEV VLASTNOSTI</span><span class="sxs-lookup"><span data-stu-id="71f6b-118">PROPERTY NAME</span></span> | <span data-ttu-id="71f6b-119">POPIS</span><span class="sxs-lookup"><span data-stu-id="71f6b-119">DESCRIPTION</span></span> |
| --- | --- |
| <span data-ttu-id="71f6b-120">Název zdroje událostí</span><span class="sxs-lookup"><span data-stu-id="71f6b-120">Event source name</span></span> | <span data-ttu-id="71f6b-121">Hello název zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="71f6b-121">hello name of your event source.</span></span> <span data-ttu-id="71f6b-122">Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="71f6b-122">This name must be unique within your Time Series Insights environment.</span></span>
| <span data-ttu-id="71f6b-123">Zdroj</span><span class="sxs-lookup"><span data-stu-id="71f6b-123">Source</span></span> | <span data-ttu-id="71f6b-124">Zvolte **IoT Hub** toocreate zdroje událostí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="71f6b-124">Choose **IoT Hub** toocreate an IoT Hub event source.</span></span>
| <span data-ttu-id="71f6b-125">Id předplatného</span><span class="sxs-lookup"><span data-stu-id="71f6b-125">Subscription Id</span></span> | <span data-ttu-id="71f6b-126">Vyberte hello předplatné, ve kterém byla vytvořena toto centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="71f6b-126">Select hello subscription in which this IoT hub was created.</span></span>
| <span data-ttu-id="71f6b-127">Název centra IoT</span><span class="sxs-lookup"><span data-stu-id="71f6b-127">IoT hub name</span></span> | <span data-ttu-id="71f6b-128">Vyberte název hello hello IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="71f6b-128">Select hello name of hello IoT Hub.</span></span>
| <span data-ttu-id="71f6b-129">Název zásady centra IoT</span><span class="sxs-lookup"><span data-stu-id="71f6b-129">IoT hub policy name</span></span> | <span data-ttu-id="71f6b-130">Vyberte hello sdíleného přístupu zásady, které lze najít v hello karta nastavení služby IoT Hub. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="71f6b-130">Select hello shared access policy, which can be found on hello IoT Hub settings tab. Each shared access policy has a name, permissions that you set, and access keys.</span></span> <span data-ttu-id="71f6b-131">Hello sdílené zásady přístupu pro váš zdroj událostí *musí* mít **služba připojit** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="71f6b-131">hello shared access policy for your event source *must* have **service connect** permissions.</span></span>
| <span data-ttu-id="71f6b-132">Skupina uživatelů centra IoT</span><span class="sxs-lookup"><span data-stu-id="71f6b-132">IoT hub consumer group</span></span> | <span data-ttu-id="71f6b-133">Hello skupiny příjemců tooread události z hello IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="71f6b-133">hello Consumer Group tooread events from hello IoT Hub.</span></span> <span data-ttu-id="71f6b-134">Vysoce je doporučeno toouse vyhrazenou skupinu spotřebitelů zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="71f6b-134">It is highly recommended toouse a dedicated consumer group for your event source.</span></span>

### <a name="provide-iot-hub-settings-manually"></a><span data-ttu-id="71f6b-135">Zadejte nastavení centra IoT ručně</span><span class="sxs-lookup"><span data-stu-id="71f6b-135">Provide IoT Hub settings manually</span></span>

<span data-ttu-id="71f6b-136">Hello následující tabulka popisuje každou vlastnost v kartě hello nový zdroj událostí s jeho popis při zadávání nastavení ručně:</span><span class="sxs-lookup"><span data-stu-id="71f6b-136">hello following table explains each property in hello New Event Source tab with its description when entering settings manually:</span></span>

| <span data-ttu-id="71f6b-137">NÁZEV VLASTNOSTI</span><span class="sxs-lookup"><span data-stu-id="71f6b-137">PROPERTY NAME</span></span> | <span data-ttu-id="71f6b-138">POPIS</span><span class="sxs-lookup"><span data-stu-id="71f6b-138">DESCRIPTION</span></span> |
| --- | --- |
| <span data-ttu-id="71f6b-139">Název zdroje událostí</span><span class="sxs-lookup"><span data-stu-id="71f6b-139">Event source name</span></span> | <span data-ttu-id="71f6b-140">Hello název zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="71f6b-140">hello name of your event source.</span></span> <span data-ttu-id="71f6b-141">Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="71f6b-141">This name must be unique within your Time Series Insights environment.</span></span>
| <span data-ttu-id="71f6b-142">Zdroj</span><span class="sxs-lookup"><span data-stu-id="71f6b-142">Source</span></span> | <span data-ttu-id="71f6b-143">Zvolte **IoT Hub** toocreate zdroje událostí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="71f6b-143">Choose **IoT Hub** toocreate an IoT Hub event source.</span></span>
| <span data-ttu-id="71f6b-144">Id předplatného</span><span class="sxs-lookup"><span data-stu-id="71f6b-144">Subscription Id</span></span> | <span data-ttu-id="71f6b-145">Hello předplatné, ve kterém byla vytvořena toto centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="71f6b-145">hello subscription in which this IoT hub was created.</span></span>
| <span data-ttu-id="71f6b-146">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="71f6b-146">Resource group</span></span> | <span data-ttu-id="71f6b-147">Hello předplatné, ve kterém byla vytvořena toto centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="71f6b-147">hello subscription in which this IoT hub was created.</span></span>
| <span data-ttu-id="71f6b-148">Název centra IoT</span><span class="sxs-lookup"><span data-stu-id="71f6b-148">IoT hub name</span></span> | <span data-ttu-id="71f6b-149">Hello název služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="71f6b-149">hello name of your IoT Hub.</span></span> <span data-ttu-id="71f6b-150">Když vytvoříte Centrum IoT, dáte mu taky určitý název</span><span class="sxs-lookup"><span data-stu-id="71f6b-150">When you created your IoT hub, you also gave it a specific name</span></span>
| <span data-ttu-id="71f6b-151">Název zásady centra IoT</span><span class="sxs-lookup"><span data-stu-id="71f6b-151">IoT hub policy name</span></span> | <span data-ttu-id="71f6b-152">zásady přístupu Hello sdílené, které se dají vytvořit na hello karta nastavení služby IoT Hub. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="71f6b-152">hello shared access policy, which can be created on hello IoT Hub settings tab. Each shared access policy has a name, permissions that you set, and access keys.</span></span> <span data-ttu-id="71f6b-153">Hello sdílené zásady přístupu pro váš zdroj událostí *musí* mít **služba připojit** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="71f6b-153">hello shared access policy for your event source *must* have **service connect** permissions.</span></span>
| <span data-ttu-id="71f6b-154">Klíč zásad centra IoT</span><span class="sxs-lookup"><span data-stu-id="71f6b-154">IoT hub policy key</span></span> | <span data-ttu-id="71f6b-155">Sdílený přístupový klíč Hello používá tooauthenticate oboru názvů Service Bus toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="71f6b-155">hello Shared Access key used tooauthenticate access toohello Service Bus namespace.</span></span> <span data-ttu-id="71f6b-156">Hello primární nebo sekundární klíč zadejte sem.</span><span class="sxs-lookup"><span data-stu-id="71f6b-156">Type hello primary or secondary key here.</span></span>
| <span data-ttu-id="71f6b-157">Skupina uživatelů centra IoT</span><span class="sxs-lookup"><span data-stu-id="71f6b-157">IoT hub consumer group</span></span> | <span data-ttu-id="71f6b-158">Hello skupiny příjemců tooread události z hello IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="71f6b-158">hello Consumer Group tooread events from hello IoT Hub.</span></span> <span data-ttu-id="71f6b-159">Vysoce je doporučeno toouse vyhrazenou skupinu spotřebitelů zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="71f6b-159">It is highly recommended toouse a dedicated consumer group for your event source.</span></span>

## <a name="next-steps"></a><span data-ttu-id="71f6b-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71f6b-160">Next steps</span></span>

1. <span data-ttu-id="71f6b-161">Přidání prostředí dat zásad přístupu tooyour [data definovat zásady přístupu](time-series-insights-data-access.md)</span><span class="sxs-lookup"><span data-stu-id="71f6b-161">Add a data access policy tooyour environment [Define data access policies](time-series-insights-data-access.md)</span></span>
1. <span data-ttu-id="71f6b-162">Přístup k prostředí v hello [portálu Statistika časové řady](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="71f6b-162">Access your environment in hello [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
