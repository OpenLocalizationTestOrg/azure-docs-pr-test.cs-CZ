---
title: Azure IoT Hub postup | Microsoft Docs
description: "Jak se jako vývojář použít k různým funkcím služby IoT Hub?"
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 786121ae249d69376b4be4c74000868cbb208989
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-azure-iot-hub"></a><span data-ttu-id="1c95b-103">Jak používat Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="1c95b-103">How to use Azure IoT Hub</span></span>

<span data-ttu-id="1c95b-104">Máte různé možnosti, jak se naučit vyvíjet pro službu IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="1c95b-104">You have various options to learn how to develop for the IoT Hub service:</span></span>

* <span data-ttu-id="1c95b-105">Přečtěte si konceptuální články, které funkce IoT Hub podrobně popsány.</span><span class="sxs-lookup"><span data-stu-id="1c95b-105">Read the conceptual articles that describe the features of IoT Hub in detail.</span></span>
* <span data-ttu-id="1c95b-106">Proveďte jeden z kurzů, které se týkají k různým funkcím služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c95b-106">Follow one of the tutorials that cover the various features of IoT Hub.</span></span>

## <a name="developer-guide"></a><span data-ttu-id="1c95b-107">Příručka pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="1c95b-107">Developer guide</span></span>

<span data-ttu-id="1c95b-108">Jako vývojář, můžete si přečíst podrobný koncepční informace o IoT Hub v [Příručka vývojáře][lnk-devguide].</span><span class="sxs-lookup"><span data-stu-id="1c95b-108">As a developer, you can read detailed conceptual guidance about IoT Hub in the [Developer Guide][lnk-devguide].</span></span> <span data-ttu-id="1c95b-109">Tato příručka obsahuje:</span><span class="sxs-lookup"><span data-stu-id="1c95b-109">This guide includes:</span></span>

* <span data-ttu-id="1c95b-110">Podrobný popis všech funkcí služby IoT Hub, které vám pomůže dozvědět, jak je používat.</span><span class="sxs-lookup"><span data-stu-id="1c95b-110">Detailed descriptions of all IoT Hub features that help you to learn how to use them.</span></span>
* <span data-ttu-id="1c95b-111">Pokyny o tom, jak zvolit, pokud jsou k dispozici více možností.</span><span class="sxs-lookup"><span data-stu-id="1c95b-111">Guidance on how to choose when multiple options are available.</span></span>

## <a name="tutorials"></a><span data-ttu-id="1c95b-112">Kurzy</span><span class="sxs-lookup"><span data-stu-id="1c95b-112">Tutorials</span></span>

<span data-ttu-id="1c95b-113">Pokud dáváte přednost Další informace týkající se konkrétních funkcí služby IoT Hub projdete praktických příkladech, existuje několik kurzy lze vybírat.</span><span class="sxs-lookup"><span data-stu-id="1c95b-113">If you prefer to learn about specific IoT Hub features by working through hands-on exercises, there are several tutorials to choose from.</span></span> <span data-ttu-id="1c95b-114">Řadu tyto kurzy jsou k dispozici v více programovacích jazyků.</span><span class="sxs-lookup"><span data-stu-id="1c95b-114">Many of these tutorials are available in multiple programming languages.</span></span> <span data-ttu-id="1c95b-115">Tyto kurzy patří:</span><span class="sxs-lookup"><span data-stu-id="1c95b-115">These tutorials include:</span></span>

- <span data-ttu-id="1c95b-116">[Zpracování zpráv typu zařízení cloud IoT Hub pomocí trasy][lnk-routes-tutorial].</span><span class="sxs-lookup"><span data-stu-id="1c95b-116">[Process IoT Hub device-to-cloud messages using routes][lnk-routes-tutorial].</span></span> <span data-ttu-id="1c95b-117">V tomto kurzu se dozvíte, jak používat služby IoT Hub směrování pravidla k odesílání zpráv typu zařízení cloud způsobem snadno, založené na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="1c95b-117">This tutorial shows you how to use IoT Hub routing rules to dispatch device-to-cloud messages in an easy, configuration-based way.</span></span>

- <span data-ttu-id="1c95b-118">[Odesílání zpráv typu cloud zařízení s centrem IoT][lnk-c2d-tutorial].</span><span class="sxs-lookup"><span data-stu-id="1c95b-118">[Send cloud-to-device messages with IoT Hub][lnk-c2d-tutorial].</span></span> <span data-ttu-id="1c95b-119">V tomto kurzu se dozvíte, jak k odesílání zpráv typu cloud zařízení prostřednictvím služby IoT Hub a příjem zpráv typu cloud zařízení na zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c95b-119">This tutorial shows you how to send cloud-to-device messages through IoT Hub and receive cloud-to-device messages on a device.</span></span>

- <span data-ttu-id="1c95b-120">[Odeslání souborů ze zařízení do cloudu pomocí služby IoT Hub][lnk-upload-tutorial].</span><span class="sxs-lookup"><span data-stu-id="1c95b-120">[Upload files from devices to the cloud with IoT Hub][lnk-upload-tutorial].</span></span> <span data-ttu-id="1c95b-121">V tomto kurzu se dozvíte, jak používat funkce nahrávání souboru služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c95b-121">This tutorial shows you how to use the file upload capabilities of IoT Hub.</span></span>

- <span data-ttu-id="1c95b-122">[Začínáme s dvojčata zařízení][lnk-twin-tutorial].</span><span class="sxs-lookup"><span data-stu-id="1c95b-122">[Get started with device twins][lnk-twin-tutorial].</span></span> <span data-ttu-id="1c95b-123">Tento kurz vás seznámí s dvojčata zařízení, hlášené vlastnosti, požadované vlastnosti a značky.</span><span class="sxs-lookup"><span data-stu-id="1c95b-123">This tutorial introduces you to device twins, reported properties, desired properties, and tags.</span></span> <span data-ttu-id="1c95b-124">Pomocí dvojčata zařízení synchronizovat hodnoty ve vašich zařízeních.</span><span class="sxs-lookup"><span data-stu-id="1c95b-124">You use device twins to synchronize values with your devices.</span></span>

- <span data-ttu-id="1c95b-125">[Používat přímé metody][lnk-methods-tutorial].</span><span class="sxs-lookup"><span data-stu-id="1c95b-125">[Use direct methods][lnk-methods-tutorial].</span></span> <span data-ttu-id="1c95b-126">V tomto kurzu se dozvíte, jak používat přímé metody.</span><span class="sxs-lookup"><span data-stu-id="1c95b-126">This tutorial shows you how to use direct methods.</span></span> <span data-ttu-id="1c95b-127">Přidejte obslužnou rutinu pro přímé metoda simulovaného zařízení a vyvolání metody přímé ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c95b-127">You add a handler for a direct method in your simulated device and invoke the direct method from IoT Hub.</span></span>

- <span data-ttu-id="1c95b-128">[Začínáme se správou zařízení][lnk-dm-tutorial].</span><span class="sxs-lookup"><span data-stu-id="1c95b-128">[Get started with device management][lnk-dm-tutorial].</span></span> <span data-ttu-id="1c95b-129">V tomto kurzu se dozvíte, jak používat funkce správy klíčů zařízení jako dvojčata a přímé metody.</span><span class="sxs-lookup"><span data-stu-id="1c95b-129">This tutorial shows you how to use key device management features such as twins and direct methods.</span></span> <span data-ttu-id="1c95b-130">Pomocí těchto funkcí vzdáleně restartování simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c95b-130">You use these features to remotely reboot your simulated device.</span></span>

- <span data-ttu-id="1c95b-131">[Použijte požadované vlastnosti pro konfiguraci zařízení][lnk-properties-tutorial].</span><span class="sxs-lookup"><span data-stu-id="1c95b-131">[Use desired properties to configure devices][lnk-properties-tutorial].</span></span> <span data-ttu-id="1c95b-132">Tento kurz ukazuje, že jste způsobu pro použití zařízení pro dvojici potřeby a hlášené vlastnosti, na vzdáleně konfigurace zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c95b-132">This tutorial shows you how to use the device twin's desired and reported properties, to remotely configure your device.</span></span>

- <span data-ttu-id="1c95b-133">[Používat úlohy zařízení k zahájení aktualizace firmwaru zařízení][lnk-jobs-tutorial].</span><span class="sxs-lookup"><span data-stu-id="1c95b-133">[Use device jobs to initiate a device firmware update][lnk-jobs-tutorial].</span></span> <span data-ttu-id="1c95b-134">V tomto kurzu se dozvíte, jak používat funkce správy klíčů zařízení jako dvojčata a přímé metody.</span><span class="sxs-lookup"><span data-stu-id="1c95b-134">This tutorial shows you how to use key device management features such as twins and direct methods.</span></span> <span data-ttu-id="1c95b-135">Zjistíte, jak používat tyto funkce vzdálené aktualizace firmwaru zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c95b-135">You learn how to use these features to remotely update your device's firmware.</span></span>

- <span data-ttu-id="1c95b-136">[Plán a všesměrovým úlohy][lnk-schedule-tutorial].</span><span class="sxs-lookup"><span data-stu-id="1c95b-136">[Schedule and broadcast jobs][lnk-schedule-tutorial].</span></span> <span data-ttu-id="1c95b-137">V tomto kurzu se dozvíte, jak používat požadované vlastnosti a metody pro přímý pro interakci s více zařízení v naplánovaném čase.</span><span class="sxs-lookup"><span data-stu-id="1c95b-137">This tutorial shows you how to use desired properties and direct methods to interact with multiple devices at a scheduled time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c95b-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1c95b-138">Next steps</span></span>

<span data-ttu-id="1c95b-139">Další informace o službě IoT Hub, najdete v článku [Příručka vývojáře][lnk-devguide].</span><span class="sxs-lookup"><span data-stu-id="1c95b-139">To learn more about the IoT Hub service, see the [Developer Guide][lnk-devguide].</span></span>

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-routes-tutorial]: ./iot-hub-csharp-csharp-process-d2c.md
[lnk-c2d-tutorial]: ./iot-hub-csharp-csharp-c2d.md
[lnk-upload-tutorial]: ./iot-hub-csharp-csharp-file-upload.md
[lnk-twin-tutorial]: ./iot-hub-node-node-twin-getstarted.md
[lnk-methods-tutorial]: ./iot-hub-node-node-direct-methods.md
[lnk-dm-tutorial]: ./iot-hub-node-node-device-management-get-started.md
[lnk-properties-tutorial]: ./iot-hub-node-node-twin-how-to-configure.md
[lnk-jobs-tutorial]: ./iot-hub-node-node-firmware-update.md
[lnk-schedule-tutorial]: ./iot-hub-node-node-schedule-jobs.md