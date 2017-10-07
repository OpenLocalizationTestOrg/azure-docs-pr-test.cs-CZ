---
title: "zařízení Azure IoT aaaThe SDK pro jazyk C | Microsoft Docs"
description: "Začínáme s Azure IoT zařízení hello SDK pro jazyk C a zjistěte, jak toocreate aplikací pro zařízení, komunikují s služby IoT hub."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: e448b061-6bdd-470a-a527-15ec03cca7b9
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: obloch
ms.openlocfilehash: 9e20742e6ea513c124bfaf28f02f6fba86170daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c"></a><span data-ttu-id="bacd0-103">Pro zařízení Azure IoT SDK pro jazyk C</span><span class="sxs-lookup"><span data-stu-id="bacd0-103">Azure IoT device SDK for C</span></span>

<span data-ttu-id="bacd0-104">Hello **zařízení Azure IoT SDK** je sada knihoven určené toosimplify hello proces odesílání zpráv tooand příjmu zprávy z hello **Azure IoT Hub** služby.</span><span class="sxs-lookup"><span data-stu-id="bacd0-104">hello **Azure IoT device SDK** is a set of libraries designed toosimplify hello process of sending messages tooand receiving messages from hello **Azure IoT Hub** service.</span></span> <span data-ttu-id="bacd0-105">Existují různé varianty hello SDK, každý cílení na konkrétní platformu, ale tento článek popisuje hello **zařízení Azure IoT SDK pro jazyk C**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-105">There are different variations of hello SDK, each targeting a specific platform, but this article describes hello **Azure IoT device SDK for C**.</span></span>

<span data-ttu-id="bacd0-106">zařízení Azure IoT Hello SDK pro jazyk C je napsán v přenositelnost toomaximize ANSI C (C99).</span><span class="sxs-lookup"><span data-stu-id="bacd0-106">hello Azure IoT device SDK for C is written in ANSI C (C99) toomaximize portability.</span></span> <span data-ttu-id="bacd0-107">Díky této funkci hello knihovny vhodným toooperate na několika platformách a zařízeních, zejména v případě, že minimalizovat disku a spotřeba paměti je prioritu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-107">This feature makes hello libraries well-suited toooperate on multiple platforms and devices, especially where minimizing disk and memory footprint is a priority.</span></span>

<span data-ttu-id="bacd0-108">Existují širokou škálu platformy, na které hello SDK je testovaná (viz hello [Azure certifikované pro katalog zařízení IoT](https://catalog.azureiotsuite.com/) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="bacd0-108">There are a broad range of platforms on which hello SDK has been tested (see hello [Azure Certified for IoT device catalog](https://catalog.azureiotsuite.com/) for details).</span></span> <span data-ttu-id="bacd0-109">I když tento článek obsahuje návody ukázkový kód spuštěný na platformě Windows hello, napříč hello rozsah podporované platformy je stejný jako kód hello popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="bacd0-109">Although this article includes walkthroughs of sample code running on hello Windows platform, hello code described in this article is identical across hello range of supported platforms.</span></span>

<span data-ttu-id="bacd0-110">Tento článek vás seznámí toohello architektura zařízení Azure IoT hello SDK pro C. Ukazuje, jak knihovny zařízení hello tooinitialize, odesílat data tooIoT rozbočovače a přijímat zprávy z něj.</span><span class="sxs-lookup"><span data-stu-id="bacd0-110">This article introduces you toohello architecture of hello Azure IoT device SDK for C. It demonstrates how tooinitialize hello device library, send data tooIoT Hub, and receive messages from it.</span></span> <span data-ttu-id="bacd0-111">Hello informace v tomto článku by měla být dostatek tooget pomocí hello SDK spuštěna, ale také poskytuje ukazatele tooadditional informace o knihovnách hello.</span><span class="sxs-lookup"><span data-stu-id="bacd0-111">hello information in this article should be enough tooget started using hello SDK, but also provides pointers tooadditional information about hello libraries.</span></span>

## <a name="sdk-architecture"></a><span data-ttu-id="bacd0-112">Architektura sady SDK</span><span class="sxs-lookup"><span data-stu-id="bacd0-112">SDK architecture</span></span>

<span data-ttu-id="bacd0-113">Můžete najít hello [ **zařízení Azure IoT SDK pro jazyk C** ](https://github.com/Azure/azure-iot-sdk-c) Githubu úložiště a zobrazte podrobnosti o hello rozhraní API v hello [referenční dokumentace rozhraní API jazyka C](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="bacd0-113">You can find hello [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of hello API in hello [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

<span data-ttu-id="bacd0-114">nejnovější verzi knihovny hello Hello lze nalézt v hello **hlavní** větve hello úložiště:</span><span class="sxs-lookup"><span data-stu-id="bacd0-114">hello latest version of hello libraries can be found in hello **master** branch of hello repository:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

* <span data-ttu-id="bacd0-115">Hello základní implementaci hello SDK je v hello **iothub\_klienta** složku, která obsahuje implementaci hello hello nejnižší vrstvu rozhraní API v hello SDK: hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="bacd0-115">hello core implementation of hello SDK is in hello **iothub\_client** folder that contains hello implementation of hello lowest API layer in hello SDK: hello **IoTHubClient** library.</span></span> <span data-ttu-id="bacd0-116">Hello **IoTHubClient** knihovna obsahuje rozhraní API pro implementace nezpracovaná zasílání zpráv pro odesílání zpráv tooIoT rozbočovače a přijímání zpráv ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bacd0-116">hello **IoTHubClient** library contains APIs implementing raw messaging for sending messages tooIoT Hub and receiving messages from IoT Hub.</span></span> <span data-ttu-id="bacd0-117">Při použití této knihovny, je zodpovědná za implementace zpráva serializaci, ale další podrobnosti o komunikaci se službou IoT Hub jsou zpracovávány pro vás.</span><span class="sxs-lookup"><span data-stu-id="bacd0-117">When using this library, you are responsible for implementing message serialization, but other details of communicating with IoT Hub are handled for you.</span></span>
* <span data-ttu-id="bacd0-118">Hello **serializátor** složka obsahuje podpůrné funkce a ukázky, které ukazují, jak hello tooserialize dat před odesláním tooAzure IoT Hub pomocí klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="bacd0-118">hello **serializer** folder contains helper functions and samples that show you how tooserialize data before sending tooAzure IoT Hub using hello client library.</span></span> <span data-ttu-id="bacd0-119">použití Hello hello serializátor není povinné a zajišťuje pro potřeby.</span><span class="sxs-lookup"><span data-stu-id="bacd0-119">hello use of hello serializer is not mandatory and is provided as a convenience.</span></span> <span data-ttu-id="bacd0-120">toouse hello **serializátor** knihovny, můžete definovat model, který určuje hello data toosend tooIoT rozbočovače hello zprávy a očekáváte, že tooreceive z něj.</span><span class="sxs-lookup"><span data-stu-id="bacd0-120">toouse hello **serializer** library, you define a model that specifies hello data toosend tooIoT Hub and hello messages you expect tooreceive from it.</span></span> <span data-ttu-id="bacd0-121">Po definování hello modelu hello SDK poskytuje s plochy rozhraní API, která vám umožní tooeasily pracovní zařízení cloud a zprávy typu cloud zařízení bez obav o hello podrobnosti serializace.</span><span class="sxs-lookup"><span data-stu-id="bacd0-121">Once hello model is defined, hello SDK provides you with an API surface that enables you tooeasily work with device-to-cloud and cloud-to-device messages without worrying about hello serialization details.</span></span> <span data-ttu-id="bacd0-122">Hello knihovně závisí na jiné opensourcové knihovny, které implementují přenosu pomocí protokolů, například MQTT a AMQP.</span><span class="sxs-lookup"><span data-stu-id="bacd0-122">hello library depends on other open source libraries that implement transport using protocols such as MQTT and AMQP.</span></span>
* <span data-ttu-id="bacd0-123">Hello **IoTHubClient** knihovny závisí na jiné opensourcové knihovny:</span><span class="sxs-lookup"><span data-stu-id="bacd0-123">hello **IoTHubClient** library depends on other open source libraries:</span></span>
  * <span data-ttu-id="bacd0-124">Hello [Azure C sdílené nástroj](https://github.com/Azure/azure-c-shared-utility) knihovny, která poskytuje běžné funkce pro základní úlohy (například řetězce, manipulace se seznamem a vstupně-výstupní operace) potřeby napříč několika souvisejících s Azure C sady SDK.</span><span class="sxs-lookup"><span data-stu-id="bacd0-124">hello [Azure C shared utility](https://github.com/Azure/azure-c-shared-utility) library, which provides common functionality for basic tasks (such as strings, list manipulation, and IO) needed across several Azure-related C SDKs.</span></span>
  * <span data-ttu-id="bacd0-125">Hello [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) knihovny, která je na straně klienta implementace AMQP optimalizované pro zařízení prostředků omezené.</span><span class="sxs-lookup"><span data-stu-id="bacd0-125">hello [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) library, which is a client-side implementation of AMQP optimized for resource constrained devices.</span></span>
  * <span data-ttu-id="bacd0-126">Hello [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) knihovny, která je pro obecné účely knihovna implementaci protokolu MQTT hello a optimalizovaný pro zařízení prostředků omezené.</span><span class="sxs-lookup"><span data-stu-id="bacd0-126">hello [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) library, which is a general-purpose library implementing hello MQTT protocol and optimized for resource constrained devices.</span></span>

<span data-ttu-id="bacd0-127">Použijte tyto knihovny je snazší toounderstand prohlížením ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="bacd0-127">Use of these libraries is easier toounderstand by looking at example code.</span></span> <span data-ttu-id="bacd0-128">Hello následující části vás provede procesem řadu hello ukázkové aplikace, které jsou součástí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="bacd0-128">hello following sections walk you through several of hello sample applications that are included in hello SDK.</span></span> <span data-ttu-id="bacd0-129">Tento názorný postup měl dát dobrou působí pro hello různé možnosti hello architektury vrstev hello SDK a hello toohow Úvod rozhraní API fungovat.</span><span class="sxs-lookup"><span data-stu-id="bacd0-129">This walkthrough should give you a good feel for hello various capabilities of hello architectural layers of hello SDK and an introduction toohow hello APIs work.</span></span>

## <a name="before-you-run-hello-samples"></a><span data-ttu-id="bacd0-130">Před spuštěním ukázky hello</span><span class="sxs-lookup"><span data-stu-id="bacd0-130">Before you run hello samples</span></span>

<span data-ttu-id="bacd0-131">Před spuštěním ukázky hello v zařízení Azure IoT hello SDK pro jazyk C, je nutné [vytvořit instanci hello služby IoT Hub](iot-hub-create-through-portal.md) ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="bacd0-131">Before you can run hello samples in hello Azure IoT device SDK for C, you must [create an instance of hello IoT Hub service](iot-hub-create-through-portal.md) in your Azure subscription.</span></span> <span data-ttu-id="bacd0-132">Dokončete hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="bacd0-132">Then complete hello following tasks:</span></span>

* <span data-ttu-id="bacd0-133">Příprava vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="bacd0-133">Prepare your development environment</span></span>
* <span data-ttu-id="bacd0-134">Získejte přihlašovací údaje zařízení.</span><span class="sxs-lookup"><span data-stu-id="bacd0-134">Obtain device credentials.</span></span>

### <a name="prepare-your-development-environment"></a><span data-ttu-id="bacd0-135">Příprava vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="bacd0-135">Prepare your development environment</span></span>

<span data-ttu-id="bacd0-136">Balíčky jsou k dispozici pro běžné platformy (například NuGet pro systém Windows nebo apt_get Debian a Ubuntu) a ukázky hello použít tyto balíčky, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bacd0-136">Packages are provided for common platforms (such as NuGet for Windows or apt_get for Debian and Ubuntu) and hello samples use these packages when available.</span></span> <span data-ttu-id="bacd0-137">V některých případech musíte toocompile hello SDK pro nebo na zařízení.</span><span class="sxs-lookup"><span data-stu-id="bacd0-137">In some cases, you need toocompile hello SDK for or on your device.</span></span> <span data-ttu-id="bacd0-138">Pokud potřebujete toocompile hello SDK, přečtěte si [Příprava vývojového prostředí](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) v úložišti GitHub hello.</span><span class="sxs-lookup"><span data-stu-id="bacd0-138">If you need toocompile hello SDK, see [Prepare your development environment](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) in hello GitHub repository.</span></span>

<span data-ttu-id="bacd0-139">tooobtain hello ukázkový kód aplikace, stáhnout kopii hello SDK z Githubu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-139">tooobtain hello sample application code, download a copy of hello SDK from GitHub.</span></span> <span data-ttu-id="bacd0-140">Získání vaší kopie hello zdroje z hello **hlavní** větve hello [úložiště GitHub](https://github.com/Azure/azure-iot-sdk-c).</span><span class="sxs-lookup"><span data-stu-id="bacd0-140">Get your copy of hello source from hello **master** branch of hello [GitHub repository](https://github.com/Azure/azure-iot-sdk-c).</span></span>


### <a name="obtain-hello-device-credentials"></a><span data-ttu-id="bacd0-141">Získat přihlašovací údaje hello zařízení</span><span class="sxs-lookup"><span data-stu-id="bacd0-141">Obtain hello device credentials</span></span>

<span data-ttu-id="bacd0-142">Teď, když máte hello ukázka zdrojového kódu, je další toodo věc hello tooget sadu přihlašovacích údajů zařízení.</span><span class="sxs-lookup"><span data-stu-id="bacd0-142">Now that you have hello sample source code, hello next thing toodo is tooget a set of device credentials.</span></span> <span data-ttu-id="bacd0-143">Pro zařízení toobe možné tooaccess služby IoT hub je nejprve nutno přidat hello zařízení toohello registru identit služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bacd0-143">For a device toobe able tooaccess an IoT hub, you must first add hello device toohello IoT Hub identity registry.</span></span> <span data-ttu-id="bacd0-144">Když přidáte zařízení, můžete získat sadu pověření zařízení, které potřebujete pro hello zařízení toobe možné tooconnect toohello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="bacd0-144">When you add your device, you get a set of device credentials that you need for hello device toobe able tooconnect toohello IoT hub.</span></span> <span data-ttu-id="bacd0-145">ukázkové aplikace Hello popsané v další části hello předpokládají, že tyto přihlašovací údaje ve formě hello **zařízení připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-145">hello sample applications discussed in hello next section expect these credentials in hello form of a **device connection string**.</span></span>

<span data-ttu-id="bacd0-146">Existuje několik nástrojů toohelp s otevřeným zdrojem můžete spravovat své služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="bacd0-146">There are several open source tools toohelp you manage your IoT hub.</span></span>

* <span data-ttu-id="bacd0-147">Aplikace systému Windows s názvem [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span><span class="sxs-lookup"><span data-stu-id="bacd0-147">A Windows application called [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span></span>
* <span data-ttu-id="bacd0-148">Nástroj příkazového řádku node.js napříč platformami názvem [iothub-explorer](https://github.com/azure/iothub-explorer).</span><span class="sxs-lookup"><span data-stu-id="bacd0-148">A cross-platform node.js CLI tool called [iothub-explorer](https://github.com/azure/iothub-explorer).</span></span>

<span data-ttu-id="bacd0-149">Tento kurz používá hello grafické *explorer zařízení* nástroj.</span><span class="sxs-lookup"><span data-stu-id="bacd0-149">This tutorial uses hello graphical *device explorer* tool.</span></span> <span data-ttu-id="bacd0-150">Můžete taky hello *iothub-explorer* Pokud dáváte přednost toouse nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="bacd0-150">You can also use hello *iothub-explorer* tool if you prefer toouse a CLI tool.</span></span>

<span data-ttu-id="bacd0-151">Nástroj Průzkumník zařízení Hello používá tooperform knihovny služby Azure IoT hello různé funkce na IoT Hub, včetně přidávání zařízení.</span><span class="sxs-lookup"><span data-stu-id="bacd0-151">hello device explorer tool uses hello Azure IoT service libraries tooperform various functions on IoT Hub, including adding devices.</span></span> <span data-ttu-id="bacd0-152">Jestliže používáte hello zařízení explorer nástroj tooadd zařízení, můžete získat připojovací řetězec pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="bacd0-152">If you use hello device explorer tool tooadd a device, you get a connection string for your device.</span></span> <span data-ttu-id="bacd0-153">Je nutné tento připojovací řetězec toorun hello ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bacd0-153">You need this connection string toorun hello sample applications.</span></span>

<span data-ttu-id="bacd0-154">Pokud si nejste obeznámeni s nástroji Průzkumník hello zařízení, hello následující postup popisuje, jak toouse ho tooadd zařízení a získat připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="bacd0-154">If you're not familiar with hello device explorer tool, hello following procedure describes how toouse it tooadd a device and obtain a device connection string.</span></span>

<span data-ttu-id="bacd0-155">Nástroj pro zařízení explorer tooinstall hello, najdete v části [jak toouse hello Explorer zařízení pro zařízení IoT Hub](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span><span class="sxs-lookup"><span data-stu-id="bacd0-155">tooinstall hello device explorer tool, see [How toouse hello Device Explorer for IoT Hub devices](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span></span>

<span data-ttu-id="bacd0-156">Při spuštění programu hello, zobrazí se toto rozhraní:</span><span class="sxs-lookup"><span data-stu-id="bacd0-156">When you run hello program, you see this interface:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

<span data-ttu-id="bacd0-157">Zadejte vaše **IoT Hub připojovací řetězec** v hello nejprve pole a klikněte na **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-157">Enter your **IoT Hub Connection String** in hello first field and click **Update**.</span></span> <span data-ttu-id="bacd0-158">Tento krok nakonfiguruje nástroj hello tak, aby může komunikovat se službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bacd0-158">This step configures hello tool so that it can communicate with IoT Hub.</span></span>

<span data-ttu-id="bacd0-159">Pokud je nakonfigurovaná hello připojovací řetězec služby IoT Hub, klikněte na tlačítko hello **správy** karty:</span><span class="sxs-lookup"><span data-stu-id="bacd0-159">When hello IoT Hub connection string is configured, click hello **Management** tab:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

<span data-ttu-id="bacd0-160">Tato karta je, kde budete spravovat hello zařízení zaregistrovaná ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="bacd0-160">This tab is where you manage hello devices registered in your IoT hub.</span></span>

<span data-ttu-id="bacd0-161">Vytvoření zařízení kliknutím hello **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bacd0-161">You create a device by clicking hello **Create** button.</span></span> <span data-ttu-id="bacd0-162">Zobrazí se dialogové okno zobrazí sadu předem vyplněná klíče (primární i sekundární).</span><span class="sxs-lookup"><span data-stu-id="bacd0-162">A dialog displays with a set of pre-populated keys (primary and secondary).</span></span> <span data-ttu-id="bacd0-163">Zadejte **ID zařízení** a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-163">Enter a **Device ID** and then click **Create**.</span></span>

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

<span data-ttu-id="bacd0-164">Při vytváření hello zařízení hello zařízení seznam aktualizací s všechny hello zaregistrované zařízení, včetně hello ten, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="bacd0-164">When hello device is created, hello Devices list updates with all hello registered devices, including hello one you just created.</span></span> <span data-ttu-id="bacd0-165">Pokud kliknete pravým tlačítkem na nové zařízení, zobrazí se v této nabídce:</span><span class="sxs-lookup"><span data-stu-id="bacd0-165">If you right-click your new device, you see this menu:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

<span data-ttu-id="bacd0-166">Pokud se rozhodnete **zkopírujte připojovací řetězec pro vybrané zařízení**, hello zařízení připojovací řetězec je zkopírovaný toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="bacd0-166">If you choose **Copy connection string for selected device**, hello device connection string is copied toohello clipboard.</span></span> <span data-ttu-id="bacd0-167">Ponechat si kopii hello zařízení připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="bacd0-167">Keep a copy of hello device connection string.</span></span> <span data-ttu-id="bacd0-168">Budete ho potřebovat při spuštění ukázkové aplikace hello popsané v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="bacd0-168">You need it when running hello sample applications described in hello following sections.</span></span>

<span data-ttu-id="bacd0-169">Po dokončení kroků hello výše, jste připravené toostart systémem nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="bacd0-169">When you've completed hello steps above, you're ready toostart running some code.</span></span> <span data-ttu-id="bacd0-170">Obě ukázky mít konstanta hello horní části hello hlavní zdrojový soubor, který umožňuje tooenter připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="bacd0-170">Both samples have a constant at hello top of hello main source file that enables you tooenter a connection string.</span></span> <span data-ttu-id="bacd0-171">Například hello odpovídající řádek z hello **iothub\_klienta\_ukázka\_mqtt** aplikace se zobrazí následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="bacd0-171">For example, hello corresponding line from hello **iothub\_client\_sample\_mqtt** application appears as follows.</span></span>

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-hello-iothubclient-library"></a><span data-ttu-id="bacd0-172">Použití knihovny IoTHubClient hello</span><span class="sxs-lookup"><span data-stu-id="bacd0-172">Use hello IoTHubClient library</span></span>

<span data-ttu-id="bacd0-173">V rámci hello **iothub\_klienta** složky v hello [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) úložiště, je **ukázky** složky, která obsahuje aplikaci s názvem **iothub\_klienta\_ukázka\_mqtt**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-173">Within hello **iothub\_client** folder in hello [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) repository, there is a **samples** folder that contains an application called **iothub\_client\_sample\_mqtt**.</span></span>

<span data-ttu-id="bacd0-174">verze Windows Hello hello **iothub\_klienta\_ukázka\_mqtt** aplikace obsahuje hello následující řešení sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="bacd0-174">hello Windows version of hello **iothub\_client\_sample\_mqtt** application includes hello following Visual Studio solution:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-mqtt.PNG)

> [!NOTE]
> <span data-ttu-id="bacd0-175">Pokud tento projekt otevřít v aplikaci Visual Studio 2017, přijměte hello výzvy tooretarget hello projektu toohello nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="bacd0-175">If you open this project in Visual Studio 2017, accept hello prompts tooretarget hello project toohello latest version.</span></span>

<span data-ttu-id="bacd0-176">Toto řešení obsahuje jedné aplikace.</span><span class="sxs-lookup"><span data-stu-id="bacd0-176">This solution contains a single project.</span></span> <span data-ttu-id="bacd0-177">Existují čtyři balíčky NuGet, které jsou nainstalované v tomto řešení:</span><span class="sxs-lookup"><span data-stu-id="bacd0-177">There are four NuGet packages installed in this solution:</span></span>

* <span data-ttu-id="bacd0-178">Microsoft.Azure.C.SharedUtility</span><span class="sxs-lookup"><span data-stu-id="bacd0-178">Microsoft.Azure.C.SharedUtility</span></span>
* <span data-ttu-id="bacd0-179">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="bacd0-179">Microsoft.Azure.IoTHub.MqttTransport</span></span>
* <span data-ttu-id="bacd0-180">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="bacd0-180">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
* <span data-ttu-id="bacd0-181">Microsoft.Azure.umqtt</span><span class="sxs-lookup"><span data-stu-id="bacd0-181">Microsoft.Azure.umqtt</span></span>

<span data-ttu-id="bacd0-182">Budete vždy potřebovat hello **Microsoft.Azure.C.SharedUtility** balíček při práci s hello SDK.</span><span class="sxs-lookup"><span data-stu-id="bacd0-182">You always need hello **Microsoft.Azure.C.SharedUtility** package when you are working with hello SDK.</span></span> <span data-ttu-id="bacd0-183">Tato ukázka používá protokol MQTT hello, proto je nutné zahrnout hello **Microsoft.Azure.umqtt** a **Microsoft.Azure.IoTHub.MqttTransport** balíčků (existují zde ekvivalentní balíčky pro AMQP a HTTP ).</span><span class="sxs-lookup"><span data-stu-id="bacd0-183">This sample uses hello MQTT protocol, therefore you must include hello **Microsoft.Azure.umqtt** and **Microsoft.Azure.IoTHub.MqttTransport** packages (there are equivalent packages for AMQP and HTTP).</span></span> <span data-ttu-id="bacd0-184">Protože ukázka hello používá hello **IoTHubClient** knihovny, musíte taky zahrnout hello **Microsoft.Azure.IoTHub.IoTHubClient** balíček ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="bacd0-184">Because hello sample uses hello **IoTHubClient** library, you must also include hello **Microsoft.Azure.IoTHub.IoTHubClient** package in your solution.</span></span>

<span data-ttu-id="bacd0-185">Hello implementace pro ukázkovou aplikaci hello můžete najít v hello **iothub\_klienta\_ukázka\_mqtt.c** zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="bacd0-185">You can find hello implementation for hello sample application in hello **iothub\_client\_sample\_mqtt.c** source file.</span></span>

<span data-ttu-id="bacd0-186">Hello následující kroky pomocí této ukázkové aplikaci toowalk vás jaké jsou požadavky toouse hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="bacd0-186">hello following steps use this sample application toowalk you through what's required toouse hello **IoTHubClient** library.</span></span>

### <a name="initialize-hello-library"></a><span data-ttu-id="bacd0-187">Inicializace knihovny hello</span><span class="sxs-lookup"><span data-stu-id="bacd0-187">Initialize hello library</span></span>

> [!NOTE]
> <span data-ttu-id="bacd0-188">Před zahájením práce s knihovnami hello, může být nutné tooperform inicializace některé specifické pro platformu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-188">Before you start working with hello libraries, you may need tooperform some platform-specific initialization.</span></span> <span data-ttu-id="bacd0-189">Například pokud máte v plánu toouse AMQP v systému Linux je nutné inicializovat knihovny OpenSSL hello.</span><span class="sxs-lookup"><span data-stu-id="bacd0-189">For example, if you plan toouse AMQP on Linux you must initialize hello OpenSSL library.</span></span> <span data-ttu-id="bacd0-190">Hello ukázky v hello [úložiště GitHub](https://github.com/Azure/azure-iot-sdk-c) volání funkce nástroj hello **platformy\_init** při hello spustí klienta a volejte hello **platformy\_deinit**  funkce před ukončením.</span><span class="sxs-lookup"><span data-stu-id="bacd0-190">hello samples in hello [GitHub repository](https://github.com/Azure/azure-iot-sdk-c) call hello utility function **platform\_init** when hello client starts and call hello **platform\_deinit** function before exiting.</span></span> <span data-ttu-id="bacd0-191">Tyto funkce jsou deklarované v hello platform.h hlavičkový soubor.</span><span class="sxs-lookup"><span data-stu-id="bacd0-191">These functions are declared in hello platform.h header file.</span></span> <span data-ttu-id="bacd0-192">Zkontrolujte hello Definice tyto funkce pro svou cílovou platformu v hello [úložiště](https://github.com/Azure/azure-iot-sdk-c) toodetermine jestli potřebujete tooinclude žádné specifické pro platformu inicializace kódu v vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="bacd0-192">Examine hello definitions of these functions for your target platform in hello [repository](https://github.com/Azure/azure-iot-sdk-c) toodetermine whether you need tooinclude any platform-specific initialization code in your client.</span></span>

<span data-ttu-id="bacd0-193">toostart práci s knihovnami hello nejprve přidělit popisovač klienta služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="bacd0-193">toostart working with hello libraries, first allocate an IoT Hub client handle:</span></span>

```c
if ((iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

<span data-ttu-id="bacd0-194">Předáte kopii hello zařízení připojovací řetězec, který jste získali z hello zařízení explorer nástroj toothis funkce.</span><span class="sxs-lookup"><span data-stu-id="bacd0-194">You pass a copy of hello device connection string you obtained from hello device explorer tool toothis function.</span></span> <span data-ttu-id="bacd0-195">Můžete také určit toouse protokol hello komunikace.</span><span class="sxs-lookup"><span data-stu-id="bacd0-195">You also designate hello communications protocol toouse.</span></span> <span data-ttu-id="bacd0-196">Tento příklad používá MQTT, ale AMQP a HTTP jsou také možnosti.</span><span class="sxs-lookup"><span data-stu-id="bacd0-196">This example uses MQTT, but AMQP and HTTP are also options.</span></span>

<span data-ttu-id="bacd0-197">Pokud máte platný **IOTHUB\_klienta\_zpracování**, je možné spustit volání rozhraní API toosend hello a přijímat zprávy tooand ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bacd0-197">When you have a valid **IOTHUB\_CLIENT\_HANDLE**, you can start calling hello APIs toosend and receive messages tooand from IoT Hub.</span></span>

### <a name="send-messages"></a><span data-ttu-id="bacd0-198">Odesílání zpráv</span><span class="sxs-lookup"><span data-stu-id="bacd0-198">Send messages</span></span>

<span data-ttu-id="bacd0-199">Ukázková aplikace Hello nastaví centrum IoT k tooyour smyčky toosend zprávy.</span><span class="sxs-lookup"><span data-stu-id="bacd0-199">hello sample application sets up a loop toosend messages tooyour IoT hub.</span></span> <span data-ttu-id="bacd0-200">Hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="bacd0-200">hello following snippet:</span></span>

- <span data-ttu-id="bacd0-201">Vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-201">Creates a message.</span></span>
- <span data-ttu-id="bacd0-202">Přidá zprávu toohello vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bacd0-202">Adds a property toohello message.</span></span>
- <span data-ttu-id="bacd0-203">Odešle zprávu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-203">Sends a message.</span></span>

<span data-ttu-id="bacd0-204">Nejprve vytvořte zprávu:</span><span class="sxs-lookup"><span data-stu-id="bacd0-204">First, create a message:</span></span>

```c
size_t iterator = 0;
do
{
    if (iterator < MESSAGE_COUNT)
    {
        sprintf_s(msgText, sizeof(msgText), "{\"deviceId\":\"myFirstDevice\",\"windSpeed\":%.2f}", avgWindSpeed + (rand() % 4 + 2));
        if ((messages[iterator].messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText))) == NULL)
        {
            (void)printf("ERROR: iotHubMessageHandle is NULL!\r\n");
        }
        else
        {
            messages[iterator].messageTrackingId = iterator;
            MAP_HANDLE propMap = IoTHubMessage_Properties(messages[iterator].messageHandle);
            (void)sprintf_s(propText, sizeof(propText), "PropMsg_%zu", iterator);
            if (Map_AddOrUpdate(propMap, "PropName", propText) != MAP_OK)
            {
                (void)printf("ERROR: Map_AddOrUpdate Failed!\r\n");
            }

            if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messages[iterator].messageHandle, SendConfirmationCallback, &messages[iterator]) != IOTHUB_CLIENT_OK)
            {
                (void)printf("ERROR: IoTHubClient_LL_SendEventAsync..........FAILED!\r\n");
            }
            else
            {
                (void)printf("IoTHubClient_LL_SendEventAsync accepted message [%d] for transmission tooIoT Hub.\r\n", (int)iterator);
            }
        }
    }
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1);

    iterator++;
} while (g_continueRunning);
```

<span data-ttu-id="bacd0-205">Pokaždé, když odešlete zprávu, zadáte odkaz funkce zpětného volání tooa, která je volána při odesílání dat hello.</span><span class="sxs-lookup"><span data-stu-id="bacd0-205">Every time you send a message, you specify a reference tooa callback function that's invoked when hello data is sent.</span></span> <span data-ttu-id="bacd0-206">V tomto příkladu je volána funkce zpětného volání hello **SendConfirmationCallback**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-206">In this example, hello callback function is called **SendConfirmationCallback**.</span></span> <span data-ttu-id="bacd0-207">Hello následující fragment kódu ukazuje tuto funkci zpětného volání:</span><span class="sxs-lookup"><span data-stu-id="bacd0-207">hello following snippet shows this callback function:</span></span>

```c
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %zu with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

<span data-ttu-id="bacd0-208">Všimněte si volání toohello hello **IoTHubMessage\_Destroy** fungovat, když jste hotovi s uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-208">Note hello call toohello **IoTHubMessage\_Destroy** function when you're done with hello message.</span></span> <span data-ttu-id="bacd0-209">Tato funkce uvolní prostředky hello přidělené při vytváření uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-209">This function frees hello resources allocated when you created hello message.</span></span>

### <a name="receive-messages"></a><span data-ttu-id="bacd0-210">Příjem zpráv</span><span class="sxs-lookup"><span data-stu-id="bacd0-210">Receive messages</span></span>

<span data-ttu-id="bacd0-211">Přijímání zprávy je asynchronní operace.</span><span class="sxs-lookup"><span data-stu-id="bacd0-211">Receiving a message is an asynchronous operation.</span></span> <span data-ttu-id="bacd0-212">Když hello zařízení přijme zprávu o se nejprve zaregistrovat tooinvoke hello zpětného volání:</span><span class="sxs-lookup"><span data-stu-id="bacd0-212">First, you register hello callback tooinvoke when hello device receives a message:</span></span>

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext) != IOTHUB_CLIENT_OK)
{
    (void)printf("ERROR: IoTHubClient_LL_SetMessageCallback..........FAILED!\r\n");
}
else
{
    (void)printf("IoTHubClient_LL_SetMessageCallback...successful.\r\n");
...
```

<span data-ttu-id="bacd0-213">Parametr last Hello je neplatný ukazatel toowhatever, které chcete.</span><span class="sxs-lookup"><span data-stu-id="bacd0-213">hello last parameter is a void pointer toowhatever you want.</span></span> <span data-ttu-id="bacd0-214">V ukázce hello je celé číslo tooan ukazatele, ale může to být tooa ukazatel složitější datové struktury.</span><span class="sxs-lookup"><span data-stu-id="bacd0-214">In hello sample, it's a pointer tooan integer but it could be a pointer tooa more complex data structure.</span></span> <span data-ttu-id="bacd0-215">Tento parametr umožňuje hello zpětného volání funkce toooperate na sdílení stavu s hello volající tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="bacd0-215">This parameter enables hello callback function toooperate on shared state with hello caller of this function.</span></span>

<span data-ttu-id="bacd0-216">Když hello zařízení obdrží zprávu, hello registrovaná funkce zpětného volání je volána.</span><span class="sxs-lookup"><span data-stu-id="bacd0-216">When hello device receives a message, hello registered callback function is invoked.</span></span> <span data-ttu-id="bacd0-217">Tato funkce zpětného volání načte:</span><span class="sxs-lookup"><span data-stu-id="bacd0-217">This callback function retrieves:</span></span>

* <span data-ttu-id="bacd0-218">id zprávy Hello a id korelace z uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-218">hello message id and correlation id from hello message.</span></span>
* <span data-ttu-id="bacd0-219">obsah zprávy Hello.</span><span class="sxs-lookup"><span data-stu-id="bacd0-219">hello message content.</span></span>
* <span data-ttu-id="bacd0-220">Vlastní vlastnosti z uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-220">Any custom properties from hello message.</span></span>

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    MAP_HANDLE mapProperties;
    const char* messageId;
    const char* correlationId;

    // Message properties
    if ((messageId = IoTHubMessage_GetMessageId(message)) == NULL)
    {
        messageId = "<null>";
    }

    if ((correlationId = IoTHubMessage_GetCorrelationId(message)) == NULL)
    {
        correlationId = "<null>";
    }

    // Message content
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        (void)printf("unable tooretrieve hello message data\r\n");
    }
    else
    {
        (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);
        // If we receive hello work 'quit' then we stop running
        if (size == (strlen("quit") * sizeof(char)) && memcmp(buffer, "quit", size) == 0)
        {
            g_continueRunning = false;
        }
    }

    // Retrieve properties from hello message
    mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                size_t index;

                printf(" Message Properties:\r\n");
                for (index = 0; index < propertyCount; index++)
                {
                    (void)printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                (void)printf("\r\n");
            }
        }
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

<span data-ttu-id="bacd0-221">Použití hello **IoTHubMessage\_GetByteArray** funkce tooretrieve uvítací zprávu, která v tomto příkladu je řetězec.</span><span class="sxs-lookup"><span data-stu-id="bacd0-221">Use hello **IoTHubMessage\_GetByteArray** function tooretrieve hello message, which in this example is a string.</span></span>

### <a name="uninitialize-hello-library"></a><span data-ttu-id="bacd0-222">Uninitialize – hello knihovně</span><span class="sxs-lookup"><span data-stu-id="bacd0-222">Uninitialize hello library</span></span>

<span data-ttu-id="bacd0-223">Po dokončení odesílání událostí a přijímání zpráv, můžete inicializaci hello knihovně IoT.</span><span class="sxs-lookup"><span data-stu-id="bacd0-223">When you're done sending events and receiving messages, you can uninitialize hello IoT library.</span></span> <span data-ttu-id="bacd0-224">toodo tedy vydejte hello následující volání funkce:</span><span class="sxs-lookup"><span data-stu-id="bacd0-224">toodo so, issue hello following function call:</span></span>

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

<span data-ttu-id="bacd0-225">Toto volání uvolní prostředky hello dříve přidělené hello **IoTHubClient\_CreateFromConnectionString** funkce.</span><span class="sxs-lookup"><span data-stu-id="bacd0-225">This call frees up hello resources previously allocated by hello **IoTHubClient\_CreateFromConnectionString** function.</span></span>

<span data-ttu-id="bacd0-226">Jak můžete vidět, je snadné toosend a přijímat zprávy pomocí hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="bacd0-226">As you can see, it's easy toosend and receive messages with hello **IoTHubClient** library.</span></span> <span data-ttu-id="bacd0-227">Hello knihovně zpracovává hello podrobnosti o komunikaci se službou IoT Hub, včetně které toouse protocol (z hlediska hello hello vývojáře, je tato možnost jednoduché konfigurace).</span><span class="sxs-lookup"><span data-stu-id="bacd0-227">hello library handles hello details of communicating with IoT Hub, including which protocol toouse (from hello perspective of hello developer, this is a simple configuration option).</span></span>

<span data-ttu-id="bacd0-228">Hello **IoTHubClient** knihovna také poskytuje přesnou kontrolu nad jak tooserialize hello data vaše zařízení odesílá tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="bacd0-228">hello **IoTHubClient** library also provides precise control over how tooserialize hello data your device sends tooIoT Hub.</span></span> <span data-ttu-id="bacd0-229">V některých případech tato úroveň řízení je několik výhod, ale v jiné je podrobností implementace, které nemají být, že toobe problémem.</span><span class="sxs-lookup"><span data-stu-id="bacd0-229">In some cases this level of control is an advantage, but in others it is an implementation detail that you don't want toobe concerned with.</span></span> <span data-ttu-id="bacd0-230">Pokud tento způsob hello případ, můžete zvážit použití hello **serializátor** knihovny, která je popsaná v další části hello.</span><span class="sxs-lookup"><span data-stu-id="bacd0-230">If that's hello case, you might consider using hello **serializer** library, which is described in hello next section.</span></span>

## <a name="use-hello-serializer-library"></a><span data-ttu-id="bacd0-231">Použití knihovny serializátor hello</span><span class="sxs-lookup"><span data-stu-id="bacd0-231">Use hello serializer library</span></span>

<span data-ttu-id="bacd0-232">Koncepčně hello **serializátor** knihovna je umístěna nad hello **IoTHubClient** knihovny v hello SDK.</span><span class="sxs-lookup"><span data-stu-id="bacd0-232">Conceptually hello **serializer** library sits on top of hello **IoTHubClient** library in hello SDK.</span></span> <span data-ttu-id="bacd0-233">Používá hello **IoTHubClient** knihovny pro hello základní komunikace se službou IoT Hub, ale přidá modelování možnosti, které musejí hello práci s zpráva serializace odebrání vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="bacd0-233">It uses hello **IoTHubClient** library for hello underlying communication with IoT Hub, but it adds modeling capabilities that remove hello burden of dealing with message serialization from hello developer.</span></span> <span data-ttu-id="bacd0-234">Jak funguje tato knihovna je nejvhodnější ukázán na příkladu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-234">How this library works is best demonstrated by an example.</span></span>

<span data-ttu-id="bacd0-235">Uvnitř hello **serializátor** složky v hello [úložiště azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c), je **ukázky** složky, která obsahuje aplikaci s názvem **simplesample \_mqtt**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-235">Inside hello **serializer** folder in hello [azure-iot-sdk-c repository](https://github.com/Azure/azure-iot-sdk-c), is a **samples** folder that contains an application called **simplesample\_mqtt**.</span></span> <span data-ttu-id="bacd0-236">verze systému Windows Hello této ukázky obsahuje hello následující řešení sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="bacd0-236">hello Windows version of this sample includes hello following Visual Studio solution:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_mqtt.PNG)

> [!NOTE]
> <span data-ttu-id="bacd0-237">Pokud tento projekt otevřít v aplikaci Visual Studio 2017, přijměte hello výzvy tooretarget hello projektu toohello nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="bacd0-237">If you open this project in Visual Studio 2017, accept hello prompts tooretarget hello project toohello latest version.</span></span>

<span data-ttu-id="bacd0-238">Stejně jako u předchozí ukázce hello tato zahrnuje několik balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="bacd0-238">As with hello previous sample, this one includes several NuGet packages:</span></span>

* <span data-ttu-id="bacd0-239">Microsoft.Azure.C.SharedUtility</span><span class="sxs-lookup"><span data-stu-id="bacd0-239">Microsoft.Azure.C.SharedUtility</span></span>
* <span data-ttu-id="bacd0-240">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="bacd0-240">Microsoft.Azure.IoTHub.MqttTransport</span></span>
* <span data-ttu-id="bacd0-241">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="bacd0-241">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
* <span data-ttu-id="bacd0-242">Microsoft.Azure.IoTHub.Serializer</span><span class="sxs-lookup"><span data-stu-id="bacd0-242">Microsoft.Azure.IoTHub.Serializer</span></span>
* <span data-ttu-id="bacd0-243">Microsoft.Azure.umqtt</span><span class="sxs-lookup"><span data-stu-id="bacd0-243">Microsoft.Azure.umqtt</span></span>

<span data-ttu-id="bacd0-244">Seznámili jste se většina těchto balíčků v předchozí ukázce hello, ale **Microsoft.Azure.IoTHub.Serializer** je nová.</span><span class="sxs-lookup"><span data-stu-id="bacd0-244">You've seen most of these packages in hello previous sample, but **Microsoft.Azure.IoTHub.Serializer** is new.</span></span> <span data-ttu-id="bacd0-245">Tento balíček je požadováno, když používáte hello **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="bacd0-245">This package is required when you use hello **serializer** library.</span></span>

<span data-ttu-id="bacd0-246">Implementace hello vzorové aplikace hello můžete najít v hello **simplesample\_mqtt.c** souboru.</span><span class="sxs-lookup"><span data-stu-id="bacd0-246">You can find hello implementation of hello sample application in hello **simplesample\_mqtt.c** file.</span></span>

<span data-ttu-id="bacd0-247">Hello následující části vás provede procesem hello klíčových částí této ukázce.</span><span class="sxs-lookup"><span data-stu-id="bacd0-247">hello following sections walk you through hello key parts of this sample.</span></span>

### <a name="initialize-hello-library"></a><span data-ttu-id="bacd0-248">Inicializace knihovny hello</span><span class="sxs-lookup"><span data-stu-id="bacd0-248">Initialize hello library</span></span>

<span data-ttu-id="bacd0-249">Práce s hello toostart **serializátor** knihovny, inicializace hello volání rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="bacd0-249">toostart working with hello **serializer** library, call hello initialization APIs:</span></span>

```c
if (serializer_init(NULL) != SERIALIZER_OK)
{
    (void)printf("Failed on serializer_init\r\n");
}
else
{
    IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol);
    srand((unsigned int)time(NULL));
    int avgWindSpeed = 10;

    if (iotHubClientHandle == NULL)
    {
        (void)printf("Failed on IoTHubClient_LL_Create\r\n");
    }
    else
    {
        ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
        if (myWeather == NULL)
        {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
        }
        else
        {
...
```

<span data-ttu-id="bacd0-250">Hello volání toohello **serializátor\_init** funkce je jednorázové volání a inicializuje hello základní knihovny.</span><span class="sxs-lookup"><span data-stu-id="bacd0-250">hello call toohello **serializer\_init** function is a one-time call and initializes hello underlying library.</span></span> <span data-ttu-id="bacd0-251">Potom zavolejte hello **IoTHubClient\_UDOU\_CreateFromConnectionString** funkci, která je hello stejné rozhraní API jako hello **IoTHubClient** ukázka.</span><span class="sxs-lookup"><span data-stu-id="bacd0-251">Then, you call hello **IoTHubClient\_LL\_CreateFromConnectionString** function, which is hello same API as in hello **IoTHubClient** sample.</span></span> <span data-ttu-id="bacd0-252">Toto volání nastaví připojovací řetězec zařízení (Toto volání se také kterémkoliv hello protokolu chcete toouse).</span><span class="sxs-lookup"><span data-stu-id="bacd0-252">This call sets your device connection string (this call is also where you choose hello protocol you want toouse).</span></span> <span data-ttu-id="bacd0-253">Tato ukázka používá MQTT jako hello přenosu, ale může použije protokol AMQP nebo HTTP.</span><span class="sxs-lookup"><span data-stu-id="bacd0-253">This sample uses MQTT as hello transport, but could use AMQP or HTTP.</span></span>

<span data-ttu-id="bacd0-254">Nakonec zavolejte hello **vytvořit\_modelu\_INSTANCE** funkce.</span><span class="sxs-lookup"><span data-stu-id="bacd0-254">Finally, call hello **CREATE\_MODEL\_INSTANCE** function.</span></span> <span data-ttu-id="bacd0-255">**WeatherStation** je obor názvů hello hello modelu a **ContosoAnemometer** je název hello hello modelu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-255">**WeatherStation** is hello namespace of hello model and **ContosoAnemometer** is hello name of hello model.</span></span> <span data-ttu-id="bacd0-256">Po vytvoření instance hello modelu, můžete ho toostart odesílání a přijímání zpráv.</span><span class="sxs-lookup"><span data-stu-id="bacd0-256">Once hello model instance is created, you can use it toostart sending and receiving messages.</span></span> <span data-ttu-id="bacd0-257">Je však důležité toounderstand, jaké je model.</span><span class="sxs-lookup"><span data-stu-id="bacd0-257">However, it's important toounderstand what a model is.</span></span>

### <a name="define-hello-model"></a><span data-ttu-id="bacd0-258">Definování hello modelu</span><span class="sxs-lookup"><span data-stu-id="bacd0-258">Define hello model</span></span>

<span data-ttu-id="bacd0-259">Model v hello **serializátor** knihovny definuje hello zprávy, které zařízení může odesílat tooIoT rozbočovače a hello zprávy, označované jako *akce* v hello Modelovací jazyk, který může přijímat.</span><span class="sxs-lookup"><span data-stu-id="bacd0-259">A model in hello **serializer** library defines hello messages that your device can send tooIoT Hub and hello messages, called *actions* in hello modeling language, which it can receive.</span></span> <span data-ttu-id="bacd0-260">Definovat model pomocí sady maker v jazyce C jako hello **simplesample\_mqtt** ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="bacd0-260">You define a model using a set of C macros as in hello **simplesample\_mqtt** sample application:</span></span>

```c
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(int, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

<span data-ttu-id="bacd0-261">Hello **začít\_obor názvů** a **END\_obor názvů** makra obou trvat hello obor názvů modelu hello jako argument.</span><span class="sxs-lookup"><span data-stu-id="bacd0-261">hello **BEGIN\_NAMESPACE** and **END\_NAMESPACE** macros both take hello namespace of hello model as an argument.</span></span> <span data-ttu-id="bacd0-262">Očekává se, že nic mezi tyto makra je hello definice modelu nebo modely a hello datové struktury, které používají modely hello.</span><span class="sxs-lookup"><span data-stu-id="bacd0-262">It's expected that anything between these macros is hello definition of your model or models, and hello data structures that hello models use.</span></span>

<span data-ttu-id="bacd0-263">V tomto příkladu je jednotný model názvem **ContosoAnemometer**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-263">In this example, there is a single model called **ContosoAnemometer**.</span></span> <span data-ttu-id="bacd0-264">Tento model definuje dva kusy dat, že zařízení může odesílat tooIoT rozbočovače: **DeviceId** a **větru**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-264">This model defines two pieces of data that your device can send tooIoT Hub: **DeviceId** and **WindSpeed**.</span></span> <span data-ttu-id="bacd0-265">Definuje také tři akce (zpráv), které vaše zařízení může získat: **TurnFanOn**, **TurnFanOff**, a **SetAirResistance**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-265">It also defines three actions (messages) that your device can receive: **TurnFanOn**, **TurnFanOff**, and **SetAirResistance**.</span></span> <span data-ttu-id="bacd0-266">Každý datový prvek má typ, přičemž každá akce má název (a volitelně sadu parametrů).</span><span class="sxs-lookup"><span data-stu-id="bacd0-266">Each data element has a type, and each action has a name (and optionally a set of parameters).</span></span>

<span data-ttu-id="bacd0-267">Hello dat a akce definované v modelu hello definovat plochy rozhraní API, můžete použít toosend zprávy tooIoT rozbočovače a reagovat toomessages odeslané toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="bacd0-267">hello data and actions defined in hello model define an API surface that you can use toosend messages tooIoT Hub, and respond toomessages sent toohello device.</span></span> <span data-ttu-id="bacd0-268">Použití tohoto modelu odhalíte nejlépe v příkladu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-268">Use of this model is best understood through an example.</span></span>

### <a name="send-messages"></a><span data-ttu-id="bacd0-269">Odesílání zpráv</span><span class="sxs-lookup"><span data-stu-id="bacd0-269">Send messages</span></span>

<span data-ttu-id="bacd0-270">Hello model definuje hello data, můžete odeslat tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="bacd0-270">hello model defines hello data you can send tooIoT Hub.</span></span> <span data-ttu-id="bacd0-271">V tomto příkladu to znamená, jeden z hello dvě položky dat definované pomocí hello **WITH_DATA** makro.</span><span class="sxs-lookup"><span data-stu-id="bacd0-271">In this example, that means one of hello two data items defined using hello **WITH_DATA** macro.</span></span> <span data-ttu-id="bacd0-272">Existuje několik kroků požadované toosend **DeviceId** a **větru** hodnoty tooan IoT hub.</span><span class="sxs-lookup"><span data-stu-id="bacd0-272">There are several steps required toosend **DeviceId** and **WindSpeed** values tooan IoT hub.</span></span> <span data-ttu-id="bacd0-273">Hello je nejdřív tooset hello data, která chcete toosend:</span><span class="sxs-lookup"><span data-stu-id="bacd0-273">hello first is tooset hello data you want toosend:</span></span>

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

<span data-ttu-id="bacd0-274">Hello modelu definovaného dříve vám umožní tooset hello hodnoty nastavením členy **struktura**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-274">hello model you defined earlier enables you tooset hello values by setting members of a **struct**.</span></span> <span data-ttu-id="bacd0-275">V dalším kroku serializovat uvítací zprávu, že chcete toosend:</span><span class="sxs-lookup"><span data-stu-id="bacd0-275">Next, serialize hello message you want toosend:</span></span>

```c
unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, myWeather->DeviceId, myWeather->WindSpeed) != CODEFIRST_OK)
{
    (void)printf("Failed tooserialize\r\n");
}
else
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
    free(destination);
}
```

<span data-ttu-id="bacd0-276">Tento kód serializuje hello zařízení cloud tooa vyrovnávací paměti (odkazuje **cílové**).</span><span class="sxs-lookup"><span data-stu-id="bacd0-276">This code serializes hello device-to-cloud tooa buffer (referenced by **destination**).</span></span> <span data-ttu-id="bacd0-277">Kód Hello poté vyvolá hello **sendMessage** funkce toosend hello zpráva tooIoT rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="bacd0-277">hello code then invokes hello **sendMessage** function toosend hello message tooIoT Hub:</span></span>

```c
static void sendMessage(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle == NULL)
    {
        printf("unable toocreate a new IoTHubMessage\r\n");
    }
    else
    {
        if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed toohand over hello message tooIoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted hello message for delivery\r\n");
        }
        IoTHubMessage_Destroy(messageHandle);
    }
    messageTrackingId++;
}
```


<span data-ttu-id="bacd0-278">druhý parametr toolast Hello **IoTHubClient\_UDOU\_SendEventAsync** je funkce zpětného volání tooa referenční dokumentace, která je volána, když hello data je úspěšně odeslána.</span><span class="sxs-lookup"><span data-stu-id="bacd0-278">hello second toolast parameter of **IoTHubClient\_LL\_SendEventAsync** is a reference tooa callback function that's called when hello data is successfully sent.</span></span> <span data-ttu-id="bacd0-279">Tady je funkce zpětného volání hello v ukázce hello:</span><span class="sxs-lookup"><span data-stu-id="bacd0-279">Here's hello callback function in hello sample:</span></span>

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

<span data-ttu-id="bacd0-280">druhý parametr Hello je ukazatel toouser kontextu; Hello předán stejné ukazatel příliš**IoTHubClient\_UDOU\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-280">hello second parameter is a pointer toouser context; hello same pointer passed too**IoTHubClient\_LL\_SendEventAsync**.</span></span> <span data-ttu-id="bacd0-281">V takovém případě hello kontext je jednoduchý čítač, ale může být, vše, co chcete.</span><span class="sxs-lookup"><span data-stu-id="bacd0-281">In this case, hello context is a simple counter, but it can be anything you want.</span></span>

<span data-ttu-id="bacd0-282">To je všechno je toosending zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="bacd0-282">That's all there is toosending device-to-cloud messages.</span></span> <span data-ttu-id="bacd0-283">je technologie Hello pouze zopakuji toocover jak tooreceive zprávy.</span><span class="sxs-lookup"><span data-stu-id="bacd0-283">hello only thing left toocover is how tooreceive messages.</span></span>

### <a name="receive-messages"></a><span data-ttu-id="bacd0-284">Příjem zpráv</span><span class="sxs-lookup"><span data-stu-id="bacd0-284">Receive messages</span></span>

<span data-ttu-id="bacd0-285">Přijímání zprávy funguje podobně toohello způsobem zprávy fungovat v hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="bacd0-285">Receiving a message works similarly toohello way messages work in hello **IoTHubClient** library.</span></span> <span data-ttu-id="bacd0-286">Nejprve zaregistrovat funkci zpětného volání zpráva:</span><span class="sxs-lookup"><span data-stu-id="bacd0-286">First, you register a message callback function:</span></span>

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable tooIoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

<span data-ttu-id="bacd0-287">Potom napíšete hello funkce zpětného volání, která je volána, když je obdržena zprávu:</span><span class="sxs-lookup"><span data-stu-id="bacd0-287">Then, you write hello callback function that's invoked when a message is received:</span></span>

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable tooIoTHubMessage_GetByteArray\r\n");
        result = IOTHUBMESSAGE_ABANDONED;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed toomalloc\r\n");
            result = IOTHUBMESSAGE_ABANDONED;
        }
        else
        {
            (void)memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

<span data-ttu-id="bacd0-288">Tento kód je často používaný – ho má stejné hello pro řešení.</span><span class="sxs-lookup"><span data-stu-id="bacd0-288">This code is boilerplate -- it's hello same for any solution.</span></span> <span data-ttu-id="bacd0-289">Tato funkce přijímá uvítací zprávu a má na starosti směrování je příliš toohello příslušnou funkci prostřednictvím volání hello**EXECUTE\_příkaz**.</span><span class="sxs-lookup"><span data-stu-id="bacd0-289">This function receives hello message and takes care of routing it toohello appropriate function through hello call too**EXECUTE\_COMMAND**.</span></span> <span data-ttu-id="bacd0-290">Volaná funkce Hello v tomto okamžiku závisí na definici hello hello akcí v daném modelu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-290">hello function called at this point depends on hello definition of hello actions in your model.</span></span>

<span data-ttu-id="bacd0-291">Když definujete akce v modelu, jste požadované tooimplement funkci, která je volána, když zařízení obdrží hello odpovídající zprávu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-291">When you define an action in your model, you're required tooimplement a function that's called when your device receives hello corresponding message.</span></span> <span data-ttu-id="bacd0-292">Například pokud váš model definuje tato akce:</span><span class="sxs-lookup"><span data-stu-id="bacd0-292">For example, if your model defines this action:</span></span>

```c
WITH_ACTION(SetAirResistance, int, Position)
```

<span data-ttu-id="bacd0-293">Definice funkce s Tento podpis:</span><span class="sxs-lookup"><span data-stu-id="bacd0-293">Define a function with this signature:</span></span>

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

<span data-ttu-id="bacd0-294">Všimněte si, jak hello název funkce hello odpovídá názvu hello hello akce v modelu hello a aby hello parametry funkce hello odpovídaly hello parametry zadané pro akce hello.</span><span class="sxs-lookup"><span data-stu-id="bacd0-294">Note how hello name of hello function matches hello name of hello action in hello model and that hello parameters of hello function match hello parameters specified for hello action.</span></span> <span data-ttu-id="bacd0-295">první parametr Hello vždy je nutné a obsahuje instanci toohello ukazatel modelu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-295">hello first parameter is always required and contains a pointer toohello instance of your model.</span></span>

<span data-ttu-id="bacd0-296">Když hello zařízení obdrží zprávu, která odpovídá tento podpis, hello odpovídající funkce je volána.</span><span class="sxs-lookup"><span data-stu-id="bacd0-296">When hello device receives a message that matches this signature, hello corresponding function is called.</span></span> <span data-ttu-id="bacd0-297">Proto kromě zajištění dostatečného s tooinclude hello často používaný kód z **IoTHubMessage**, přijímání zpráv stačí definice jednoduché funkce pro jednotlivé akce definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="bacd0-297">Therefore, aside from having tooinclude hello boilerplate code from **IoTHubMessage**, receiving messages is just a matter of defining a simple function for each action defined in your model.</span></span>

### <a name="uninitialize-hello-library"></a><span data-ttu-id="bacd0-298">Uninitialize – hello knihovně</span><span class="sxs-lookup"><span data-stu-id="bacd0-298">Uninitialize hello library</span></span>

<span data-ttu-id="bacd0-299">Když jste hotovi odesílání dat a přijímání zpráv, můžete inicializaci hello knihovně IoT:</span><span class="sxs-lookup"><span data-stu-id="bacd0-299">When you're done sending data and receiving messages, you can uninitialize hello IoT library:</span></span>

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

<span data-ttu-id="bacd0-300">Každá z těchto tří funkcí zarovnaná s hello tři Inicializace funkce popsané.</span><span class="sxs-lookup"><span data-stu-id="bacd0-300">Each of these three functions aligns with hello three initialization functions described previously.</span></span> <span data-ttu-id="bacd0-301">Volání tato rozhraní API zajistí, že můžete uvolnit dříve přidělené prostředky.</span><span class="sxs-lookup"><span data-stu-id="bacd0-301">Calling these APIs ensures that you free previously allocated resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bacd0-302">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bacd0-302">Next Steps</span></span>

<span data-ttu-id="bacd0-303">Tento článek zahrnutých hello základy používání knihovny hello v hello **zařízení Azure IoT SDK pro jazyk C**. Je k dispozici dostatek toounderstand informace, co je součástí hello SDK, jeho architektura a způsob spuštění tooget práce s hello ukázky Windows.</span><span class="sxs-lookup"><span data-stu-id="bacd0-303">This article covered hello basics of using hello libraries in hello **Azure IoT device SDK for C**. It provided you with enough information toounderstand what's included in hello SDK, its architecture, and how tooget started working with hello Windows samples.</span></span> <span data-ttu-id="bacd0-304">Další článek Hello pokračuje hello popis hello SDK tím, že vysvětlí [více o hello knihovně IoTHubClient](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="bacd0-304">hello next article continues hello description of hello SDK by explaining [more about hello IoTHubClient library](iot-hub-device-sdk-c-iothubclient.md).</span></span>

<span data-ttu-id="bacd0-305">toolearn Další informace o vývoji pro IoT Hub, najdete v části hello [SDK služby Azure IoT][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="bacd0-305">toolearn more about developing for IoT Hub, see hello [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="bacd0-306">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="bacd0-306">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="bacd0-307">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="bacd0-307">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
