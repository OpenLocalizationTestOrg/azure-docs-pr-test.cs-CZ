---
title: "Postup přidání zdroje událostí služby IoT Hub do prostředí Azure časové řady Insights | Microsoft Docs"
description: "Tento kurz vysvětluje postup přidání připojeném do služby IoT Hub pro vaše prostředí časové řady Statistika zdroje událostí"
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
ms.openlocfilehash: 72037677fac528ff8174d25b474ca7e70826a7b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-an-iot-hub-event-source"></a><span data-ttu-id="79448-103">Postup přidání zdroje událostí služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="79448-103">How to add an IoT Hub event source</span></span>

<span data-ttu-id="79448-104">Tento kurz se zaměřuje na tom, jak pomocí portálu Azure přidejte zdroje událostí, který čte ze služby IoT Hub pro vaše prostředí Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="79448-104">This tutorial covers how to use the Azure portal to add an event source that reads from an IoT Hub to your Time Series Insights environment.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79448-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="79448-105">Prerequisites</span></span>

<span data-ttu-id="79448-106">Vytvoření služby IoT Hub a jsou k němu zápisu událostí.</span><span class="sxs-lookup"><span data-stu-id="79448-106">You have created an IoT Hub and are writing events to it.</span></span> <span data-ttu-id="79448-107">Další informace o centra IoT najdete v tématu <https://azure.microsoft.com/services/iot-hub/></span><span class="sxs-lookup"><span data-stu-id="79448-107">For more information on IoT Hubs, see <https://azure.microsoft.com/services/iot-hub/></span></span>

> <span data-ttu-id="79448-108">[Skupiny příjemců] Každý zdroj události Statistika časové řady musí mít svůj vlastní vyhrazenou skupinu spotřebitelů která není sdílena s dalšími uživateli.</span><span class="sxs-lookup"><span data-stu-id="79448-108">[Consumer Groups] Each Time Series Insights event source needs to have its own dedicated consumer group that is not shared with any other consumers.</span></span> <span data-ttu-id="79448-109">Pokud více čtečky přijímat události z stejnou skupinu uživatelů, budou pravděpodobně najdete v části selhání všechny nástroje pro čtení.</span><span class="sxs-lookup"><span data-stu-id="79448-109">If multiple readers consume events from the same consumer group, all readers are likely to see failures.</span></span> <span data-ttu-id="79448-110">Podrobnosti najdete v tématu [Příručka vývojáře pro službu IoT Hub](../iot-hub/iot-hub-devguide.md).</span><span class="sxs-lookup"><span data-stu-id="79448-110">For details, see the [IoT Hub developer guide](../iot-hub/iot-hub-devguide.md).</span></span>

## <a name="choose-an-import-option"></a><span data-ttu-id="79448-111">Zvolte možnost importovat</span><span class="sxs-lookup"><span data-stu-id="79448-111">Choose an Import option</span></span>

<span data-ttu-id="79448-112">Nastavení pro zdroj události lze zadat ručně nebo centrum IoT je možné vybrat z centra IoT, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="79448-112">The settings for the event source can be entered manually or an IoT hub can be selected from the IoT hubs that are available to you.</span></span>
<span data-ttu-id="79448-113">V **možnost importu** selektor, vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="79448-113">In the **Import Option** selector, choose one of the following options:</span></span>

* <span data-ttu-id="79448-114">Zadejte nastavení centra IoT ručně</span><span class="sxs-lookup"><span data-stu-id="79448-114">Provide IoT Hub settings manually</span></span>
* <span data-ttu-id="79448-115">Pomocí služby IoT Hub z dostupných předplatných</span><span class="sxs-lookup"><span data-stu-id="79448-115">Use IoT Hub from available subscriptions</span></span>

### <a name="select-an-available-iot-hub"></a><span data-ttu-id="79448-116">Vyberte dostupné služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="79448-116">Select an available IoT Hub</span></span>

<span data-ttu-id="79448-117">Následující tabulka popisuje jednotlivé možnosti na kartě nový zdroj událostí s jeho popis při výběru Centrum IoT k dispozici jako zdroje událostí:</span><span class="sxs-lookup"><span data-stu-id="79448-117">The following table explains each option in the New Event Source tab with its description when selecting an available IoT Hub as an event source:</span></span>

| <span data-ttu-id="79448-118">NÁZEV VLASTNOSTI</span><span class="sxs-lookup"><span data-stu-id="79448-118">PROPERTY NAME</span></span> | <span data-ttu-id="79448-119">POPIS</span><span class="sxs-lookup"><span data-stu-id="79448-119">DESCRIPTION</span></span> |
| --- | --- |
| <span data-ttu-id="79448-120">Název zdroje událostí</span><span class="sxs-lookup"><span data-stu-id="79448-120">Event source name</span></span> | <span data-ttu-id="79448-121">Název zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="79448-121">The name of your event source.</span></span> <span data-ttu-id="79448-122">Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="79448-122">This name must be unique within your Time Series Insights environment.</span></span>
| <span data-ttu-id="79448-123">Zdroj</span><span class="sxs-lookup"><span data-stu-id="79448-123">Source</span></span> | <span data-ttu-id="79448-124">Zvolte **IoT Hub** k vytvoření zdroje událostí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="79448-124">Choose **IoT Hub** to create an IoT Hub event source.</span></span>
| <span data-ttu-id="79448-125">Id předplatného</span><span class="sxs-lookup"><span data-stu-id="79448-125">Subscription Id</span></span> | <span data-ttu-id="79448-126">Vyberte předplatné, ve kterém byla vytvořena toto centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="79448-126">Select the subscription in which this IoT hub was created.</span></span>
| <span data-ttu-id="79448-127">Název centra IoT</span><span class="sxs-lookup"><span data-stu-id="79448-127">IoT hub name</span></span> | <span data-ttu-id="79448-128">Vyberte název služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="79448-128">Select the name of the IoT Hub.</span></span>
| <span data-ttu-id="79448-129">Název zásady centra IoT</span><span class="sxs-lookup"><span data-stu-id="79448-129">IoT hub policy name</span></span> | <span data-ttu-id="79448-130">Vyberte zásady sdíleného přístupu, které lze najít na kartě nastavení služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="79448-130">Select the shared access policy, which can be found on the IoT Hub settings tab.</span></span> <span data-ttu-id="79448-131">Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="79448-131">Each shared access policy has a name, permissions that you set, and access keys.</span></span> <span data-ttu-id="79448-132">Zásada sdíleného přístupu ke zdroji událostí *musí* mít **služba připojit** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="79448-132">The shared access policy for your event source *must* have **service connect** permissions.</span></span>
| <span data-ttu-id="79448-133">Skupina uživatelů centra IoT</span><span class="sxs-lookup"><span data-stu-id="79448-133">IoT hub consumer group</span></span> | <span data-ttu-id="79448-134">Skupiny příjemců číst události ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="79448-134">The Consumer Group to read events from the IoT Hub.</span></span> <span data-ttu-id="79448-135">Důrazně doporučujeme použít vyhrazenou skupinu spotřebitelů zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="79448-135">It is highly recommended to use a dedicated consumer group for your event source.</span></span>

### <a name="provide-iot-hub-settings-manually"></a><span data-ttu-id="79448-136">Zadejte nastavení centra IoT ručně</span><span class="sxs-lookup"><span data-stu-id="79448-136">Provide IoT Hub settings manually</span></span>

<span data-ttu-id="79448-137">Následující tabulka popisuje každou vlastnost v kartě nový zdroj událostí s jeho popis při zadávání nastavení ručně:</span><span class="sxs-lookup"><span data-stu-id="79448-137">The following table explains each property in the New Event Source tab with its description when entering settings manually:</span></span>

| <span data-ttu-id="79448-138">NÁZEV VLASTNOSTI</span><span class="sxs-lookup"><span data-stu-id="79448-138">PROPERTY NAME</span></span> | <span data-ttu-id="79448-139">POPIS</span><span class="sxs-lookup"><span data-stu-id="79448-139">DESCRIPTION</span></span> |
| --- | --- |
| <span data-ttu-id="79448-140">Název zdroje událostí</span><span class="sxs-lookup"><span data-stu-id="79448-140">Event source name</span></span> | <span data-ttu-id="79448-141">Název zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="79448-141">The name of your event source.</span></span> <span data-ttu-id="79448-142">Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="79448-142">This name must be unique within your Time Series Insights environment.</span></span>
| <span data-ttu-id="79448-143">Zdroj</span><span class="sxs-lookup"><span data-stu-id="79448-143">Source</span></span> | <span data-ttu-id="79448-144">Zvolte **IoT Hub** k vytvoření zdroje událostí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="79448-144">Choose **IoT Hub** to create an IoT Hub event source.</span></span>
| <span data-ttu-id="79448-145">Id předplatného</span><span class="sxs-lookup"><span data-stu-id="79448-145">Subscription Id</span></span> | <span data-ttu-id="79448-146">Předplatné, ve kterém byla vytvořena toto centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="79448-146">The subscription in which this IoT hub was created.</span></span>
| <span data-ttu-id="79448-147">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="79448-147">Resource group</span></span> | <span data-ttu-id="79448-148">Předplatné, ve kterém byla vytvořena toto centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="79448-148">The subscription in which this IoT hub was created.</span></span>
| <span data-ttu-id="79448-149">Název centra IoT</span><span class="sxs-lookup"><span data-stu-id="79448-149">IoT hub name</span></span> | <span data-ttu-id="79448-150">Název služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="79448-150">The name of your IoT Hub.</span></span> <span data-ttu-id="79448-151">Když vytvoříte Centrum IoT, dáte mu taky určitý název</span><span class="sxs-lookup"><span data-stu-id="79448-151">When you created your IoT hub, you also gave it a specific name</span></span>
| <span data-ttu-id="79448-152">Název zásady centra IoT</span><span class="sxs-lookup"><span data-stu-id="79448-152">IoT hub policy name</span></span> | <span data-ttu-id="79448-153">Zásady sdíleného přístupu, které se dají vytvořit na kartě nastavení služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="79448-153">The shared access policy, which can be created on the IoT Hub settings tab.</span></span> <span data-ttu-id="79448-154">Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="79448-154">Each shared access policy has a name, permissions that you set, and access keys.</span></span> <span data-ttu-id="79448-155">Zásada sdíleného přístupu ke zdroji událostí *musí* mít **služba připojit** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="79448-155">The shared access policy for your event source *must* have **service connect** permissions.</span></span>
| <span data-ttu-id="79448-156">Klíč zásad centra IoT</span><span class="sxs-lookup"><span data-stu-id="79448-156">IoT hub policy key</span></span> | <span data-ttu-id="79448-157">Sdílený přístupový klíč použitý k ověření přístupu k oboru názvů Service Bus.</span><span class="sxs-lookup"><span data-stu-id="79448-157">The Shared Access key used to authenticate access to the Service Bus namespace.</span></span> <span data-ttu-id="79448-158">Primární nebo sekundární klíč zadejte sem.</span><span class="sxs-lookup"><span data-stu-id="79448-158">Type the primary or secondary key here.</span></span>
| <span data-ttu-id="79448-159">Skupina uživatelů centra IoT</span><span class="sxs-lookup"><span data-stu-id="79448-159">IoT hub consumer group</span></span> | <span data-ttu-id="79448-160">Skupiny příjemců číst události ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="79448-160">The Consumer Group to read events from the IoT Hub.</span></span> <span data-ttu-id="79448-161">Důrazně doporučujeme použít vyhrazenou skupinu spotřebitelů zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="79448-161">It is highly recommended to use a dedicated consumer group for your event source.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79448-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79448-162">Next steps</span></span>

1. <span data-ttu-id="79448-163">Přidat zásadu přístupu dat do vašeho prostředí [data definovat zásady přístupu](time-series-insights-data-access.md)</span><span class="sxs-lookup"><span data-stu-id="79448-163">Add a data access policy to your environment [Define data access policies](time-series-insights-data-access.md)</span></span>
1. <span data-ttu-id="79448-164">Přístup k prostředí v [portálu Statistika časové řady](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="79448-164">Access your environment in the [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>