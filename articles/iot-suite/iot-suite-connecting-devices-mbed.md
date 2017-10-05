---
title: "Připojení zařízení pomocí jazyka C na mbed | Microsoft Docs"
description: "Popisuje, jak se připojit k Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v jazyce C spuštění v zařízení s mbed zařízení."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 9551075e-dcf9-488f-943e-d0eb0e6260be
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: dobett
ROBOTS: NOINDEX
ms.openlocfilehash: ef7b78f85a787f8fbe22c0e26aa34f0cd1685d58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a><span data-ttu-id="ab628-103">Připojte zařízení k monitorování předkonfigurované řešení vzdáleného (mbed)</span><span class="sxs-lookup"><span data-stu-id="ab628-103">Connect your device to the remote monitoring preconfigured solution (mbed)</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="ab628-104">Přehled scénáře</span><span class="sxs-lookup"><span data-stu-id="ab628-104">Scenario overview</span></span>
<span data-ttu-id="ab628-105">V tomto scénáři vytvoříte zařízení, které odesílá následující telemetrii do [předkonfigurovaného řešení][lnk-what-are-preconfig-solutions] vzdáleného monitorování:</span><span class="sxs-lookup"><span data-stu-id="ab628-105">In this scenario, you create a device that sends the following telemetry to the remote monitoring [preconfigured solution][lnk-what-are-preconfig-solutions]:</span></span>

* <span data-ttu-id="ab628-106">Venkovní teplota</span><span class="sxs-lookup"><span data-stu-id="ab628-106">External temperature</span></span>
* <span data-ttu-id="ab628-107">Vnitřní teplota</span><span class="sxs-lookup"><span data-stu-id="ab628-107">Internal temperature</span></span>
* <span data-ttu-id="ab628-108">Vlhkost</span><span class="sxs-lookup"><span data-stu-id="ab628-108">Humidity</span></span>

<span data-ttu-id="ab628-109">Kód v zařízení pro zjednodušení generuje ukázkové hodnoty, ale doporučujeme vám ukázku rozšířit připojením skutečných senzorů k zařízení a odesíláním skutečné telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ab628-109">For simplicity, the code on the device generates sample values, but we encourage you to extend the sample by connecting real sensors to your device and sending real telemetry.</span></span>

<span data-ttu-id="ab628-110">Zařízení je také schopné odpovídat na metody vyvolané z řídicího panelu řešení a na požadované hodnoty vlastností nastavené na řídicím panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="ab628-110">The device is also able to respond to methods invoked from the solution dashboard and desired property values set in the solution dashboard.</span></span>

<span data-ttu-id="ab628-111">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ab628-111">To complete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="ab628-112">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="ab628-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ab628-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="ab628-113">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="ab628-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ab628-114">Before you start</span></span>
<span data-ttu-id="ab628-115">Než začnete psát kód pro zařízení, je nutné zřídit předkonfigurované řešení vzdáleného monitorování a v něm zřídit nové vlastní zařízení.</span><span class="sxs-lookup"><span data-stu-id="ab628-115">Before you write any code for your device, you must provision your remote monitoring preconfigured solution and provision a new custom device in that solution.</span></span>

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="ab628-116">Zřízení předkonfigurovaného řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="ab628-116">Provision your remote monitoring preconfigured solution</span></span>
<span data-ttu-id="ab628-117">Zařízení, které v tomto kurzu vytvoříte, odesílá data do instance předkonfigurovaného řešení [vzdáleného monitorování][lnk-remote-monitoring].</span><span class="sxs-lookup"><span data-stu-id="ab628-117">The device you create in this tutorial sends data to an instance of the [remote monitoring][lnk-remote-monitoring] preconfigured solution.</span></span> <span data-ttu-id="ab628-118">Pokud jste ve svém účtu Azure ještě nezřídili předkonfigurované řešení vzdáleného monitorování, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="ab628-118">If you haven't already provisioned the remote monitoring preconfigured solution in your Azure account, use the following steps:</span></span>

1. <span data-ttu-id="ab628-119">Na stránce <https://www.azureiotsuite.com/> můžete vytvořit řešení kliknutím na **+**.</span><span class="sxs-lookup"><span data-stu-id="ab628-119">On the <https://www.azureiotsuite.com/> page, click **+** to create a solution.</span></span>
2. <span data-ttu-id="ab628-120">Kliknutím na **Vybrat** na panelu **Vzdálené monitorování** vytvořte řešení.</span><span class="sxs-lookup"><span data-stu-id="ab628-120">Click **Select** on the **Remote monitoring** panel to create your solution.</span></span>
3. <span data-ttu-id="ab628-121">Na stránce **Vytvořit řešení vzdáleného monitorování** zadejte **Název řešení** podle vašeho výběru, vyberte **Oblast**, do které chcete řešení nasadit, a vyberte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="ab628-121">On the **Create Remote monitoring solution** page, enter a **Solution name** of your choice, select the **Region** you want to deploy to, and select the Azure subscription to want to use.</span></span> <span data-ttu-id="ab628-122">Potom klikněte na **Vytvořit řešení**.</span><span class="sxs-lookup"><span data-stu-id="ab628-122">Then click **Create solution**.</span></span>
4. <span data-ttu-id="ab628-123">Počkejte, dokud proces zřizování neskončí.</span><span class="sxs-lookup"><span data-stu-id="ab628-123">Wait until the provisioning process completes.</span></span>

> [!WARNING]
> <span data-ttu-id="ab628-124">Předkonfigurovaná řešení využívají fakturovatelné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ab628-124">The preconfigured solutions use billable Azure services.</span></span> <span data-ttu-id="ab628-125">Abyste se vyhnuli zbytečným poplatkům, nezapomeňte předkonfigurované řešení odebrat ze svého předplatného, jakmile s ním budete hotovi.</span><span class="sxs-lookup"><span data-stu-id="ab628-125">Be sure to remove the preconfigured solution from your subscription when you are done with it to avoid any unnecessary charges.</span></span> <span data-ttu-id="ab628-126">Předkonfigurované řešení můžete ze svého předplatného úplně odebrat na stránce <https://www.azureiotsuite.com/>.</span><span class="sxs-lookup"><span data-stu-id="ab628-126">You can completely remove a preconfigured solution from your subscription by visiting the <https://www.azureiotsuite.com/> page.</span></span>
> 
> 

<span data-ttu-id="ab628-127">Po skončení procesu zřizování řešení vzdáleného monitorování klikněte na **Spustit**. Ve vašem prohlížeči se otevře řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="ab628-127">When the provisioning process for the remote monitoring solution finishes, click **Launch** to open the solution dashboard in your browser.</span></span>

![Řídicí panel řešení][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a><span data-ttu-id="ab628-129">Zřízení zařízení v řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="ab628-129">Provision your device in the remote monitoring solution</span></span>
> [!NOTE]
> <span data-ttu-id="ab628-130">Pokud jste už ve svém řešení zařízení zřídili, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="ab628-130">If you have already provisioned a device in your solution, you can skip this step.</span></span> <span data-ttu-id="ab628-131">Při vytváření klientské aplikace potřebujete znát přihlašovací údaje zařízení.</span><span class="sxs-lookup"><span data-stu-id="ab628-131">You need to know the device credentials when you create the client application.</span></span>
> 
> 

<span data-ttu-id="ab628-132">Aby se zařízení mohlo připojit k předkonfigurovanému řešení, musí se identifikovat ve službě IoT Hub pomocí platných přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="ab628-132">For a device to connect to the preconfigured solution, it must identify itself to IoT Hub using valid credentials.</span></span> <span data-ttu-id="ab628-133">Přihlašovací údaje zařízení můžete zjistit z řídicího panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="ab628-133">You can retrieve the device credentials from the solution dashboard.</span></span> <span data-ttu-id="ab628-134">Přihlašovací údaje zařízení vložíte do klientské aplikace později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ab628-134">You include the device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="ab628-135">Pokud chcete přidat zařízení do řešení vzdáleného monitorování, proveďte na řídicím panelu řešení následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ab628-135">To add a device to your remote monitoring solution, complete the following steps in the solution dashboard:</span></span>

1. <span data-ttu-id="ab628-136">V levém dolním rohu řídicího panelu klikněte na **Přidat zařízení**.</span><span class="sxs-lookup"><span data-stu-id="ab628-136">In the lower left-hand corner of the dashboard, click **Add a device**.</span></span>
   
   ![Přidání zařízení][1]
2. <span data-ttu-id="ab628-138">Na panelu **Vlastní zařízení** klikněte na **Přidat nové**.</span><span class="sxs-lookup"><span data-stu-id="ab628-138">In the **Custom Device** panel, click **Add new**.</span></span>
   
   ![Přidání vlastního zařízení][2]
3. <span data-ttu-id="ab628-140">Vyberte možnost **Definovat vlastní ID zařízení**.</span><span class="sxs-lookup"><span data-stu-id="ab628-140">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="ab628-141">Zadejte ID zařízení, třeba **mydevice**, klikněte na **Zkontrolovat ID** pro ověření, že se tento název ještě nepoužívá, a potom zřiďte zařízení kliknutím na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ab628-141">Enter a Device ID such as **mydevice**, click **Check ID** to verify that name isn't already in use, and then click **Create** to provision the device.</span></span>
   
   ![Přidání ID zařízení][3]
4. <span data-ttu-id="ab628-143">Poznamenejte si přihlašovací údaje zařízení (ID zařízení, název hostitele služby IoT Hub a Klíč zařízení).</span><span class="sxs-lookup"><span data-stu-id="ab628-143">Make a note the device credentials (Device ID, IoT Hub Hostname, and Device Key).</span></span> <span data-ttu-id="ab628-144">Klientská aplikace potřebuje tyto hodnoty pro připojení k řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="ab628-144">Your client application needs these values to connect to the remote monitoring solution.</span></span> <span data-ttu-id="ab628-145">Potom klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="ab628-145">Then click **Done**.</span></span>
   
    ![Zobrazení přihlašovacích údajů zařízení][4]
5. <span data-ttu-id="ab628-147">V seznamu zařízení na řídicím panelu řešení vyberte své zařízení.</span><span class="sxs-lookup"><span data-stu-id="ab628-147">Select your device in the device list in the solution dashboard.</span></span> <span data-ttu-id="ab628-148">Pak na panelu **Podrobnosti o zařízení** klikněte na **Povolit zařízení**.</span><span class="sxs-lookup"><span data-stu-id="ab628-148">Then, in the **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="ab628-149">Stav vašeho zařízení je teď **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="ab628-149">The status of your device is now **Running**.</span></span> <span data-ttu-id="ab628-150">Řešení vzdáleného monitorování teď může z vašeho zařízení přijímat telemetrii a vyvolávat v něm metody.</span><span class="sxs-lookup"><span data-stu-id="ab628-150">The remote monitoring solution can now receive telemetry from your device and invoke methods on the device.</span></span>

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-the-c-sample-solution"></a><span data-ttu-id="ab628-151">Sestavte a spusťte ukázkové řešení C</span><span class="sxs-lookup"><span data-stu-id="ab628-151">Build and run the C sample solution</span></span>

<span data-ttu-id="ab628-152">Následující pokyny popisují kroky pro připojení [povoleno mbed FRDM K64F Freescale] [ lnk-mbed-home] zařízení do řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="ab628-152">The following instructions describe the steps for connecting an [mbed-enabled Freescale FRDM-K64F][lnk-mbed-home] device to the remote monitoring solution.</span></span>

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a><span data-ttu-id="ab628-153">Zařízení s mbed připojte k počítači sítě a vzdálené ploše</span><span class="sxs-lookup"><span data-stu-id="ab628-153">Connect the mbed device to your network and desktop machine</span></span>

1. <span data-ttu-id="ab628-154">Připojte zařízení mbed k síti pomocí kabelu Ethernet.</span><span class="sxs-lookup"><span data-stu-id="ab628-154">Connect the mbed device to your network using an Ethernet cable.</span></span> <span data-ttu-id="ab628-155">Tento krok je nezbytný, protože ukázkové aplikace vyžaduje přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="ab628-155">This step is necessary because the sample application requires internet access.</span></span>

1. <span data-ttu-id="ab628-156">V tématu [Začínáme s mbed] [ lnk-mbed-getstarted] k připojení zařízení mbed pro stolní počítač.</span><span class="sxs-lookup"><span data-stu-id="ab628-156">See [Getting Started with mbed][lnk-mbed-getstarted] to connect your mbed device to your desktop PC.</span></span>

1. <span data-ttu-id="ab628-157">Pokud počítače se systémem Windows, přečtěte si téma [konfigurace počítače] [ lnk-mbed-pcconnect] nakonfigurujte sériového portu zařízení mbed.</span><span class="sxs-lookup"><span data-stu-id="ab628-157">If your desktop PC is running Windows, see [PC Configuration][lnk-mbed-pcconnect] to configure serial port access to your mbed device.</span></span>

### <a name="create-an-mbed-project-and-import-the-sample-code"></a><span data-ttu-id="ab628-158">Vytvoření projektu mbed a importovat ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="ab628-158">Create an mbed project and import the sample code</span></span>

<span data-ttu-id="ab628-159">Postupujte podle těchto kroků pro přidání do projektu mbed některé ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="ab628-159">Follow these steps to add some sample code to an mbed project.</span></span> <span data-ttu-id="ab628-160">Import vzdálené monitorování starter projekt a poté změňte projekt, který používá protokol MQTT místo protokolu AMQP.</span><span class="sxs-lookup"><span data-stu-id="ab628-160">You import the remote monitoring starter project and then change the project to use the MQTT protocol instead of the AMQP protocol.</span></span> <span data-ttu-id="ab628-161">Nyní budete muset použít protokol MQTT používat funkce správy zařízení služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="ab628-161">Currently, you need to use the MQTT protocol to use the device management features of IoT Hub.</span></span>

1. <span data-ttu-id="ab628-162">Ve webovém prohlížeči, přejděte mbed.org [vývojáře lokality](https://developer.mbed.org/).</span><span class="sxs-lookup"><span data-stu-id="ab628-162">In your web browser, go to the mbed.org [developer site](https://developer.mbed.org/).</span></span> <span data-ttu-id="ab628-163">Pokud nemáte registraci, zobrazí se možnost vytvoření účtu (je to zdarma).</span><span class="sxs-lookup"><span data-stu-id="ab628-163">If you haven't signed up, you see an option to create an account (it's free).</span></span> <span data-ttu-id="ab628-164">Jinak Přihlaste se pomocí přihlašovacích údajů účtu.</span><span class="sxs-lookup"><span data-stu-id="ab628-164">Otherwise, log in with your account credentials.</span></span> <span data-ttu-id="ab628-165">Pak klikněte na tlačítko **kompilátoru** v pravém horním rohu stránky.</span><span class="sxs-lookup"><span data-stu-id="ab628-165">Then click **Compiler** in the upper right-hand corner of the page.</span></span> <span data-ttu-id="ab628-166">Tato akce integruje do *prostoru* rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ab628-166">This action brings you to the *Workspace* interface.</span></span>

1. <span data-ttu-id="ab628-167">Ujistěte se, že se zobrazí v pravém horním rohu okna hardwaru platformu, kterou používáte, nebo klikněte na ikonu v pravém rohu vyberte platformu hardwaru.</span><span class="sxs-lookup"><span data-stu-id="ab628-167">Make sure the hardware platform you're using appears in the upper right-hand corner of the window, or click the icon in the right-hand corner to select your hardware platform.</span></span>

1. <span data-ttu-id="ab628-168">Klikněte na tlačítko **Import** v hlavní nabídce.</span><span class="sxs-lookup"><span data-stu-id="ab628-168">Click **Import** on the main menu.</span></span> <span data-ttu-id="ab628-169">Pak klikněte na tlačítko **kliknutím sem importovat z adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="ab628-169">Then click **Click here to import from URL**.</span></span>
   
    ![Spusťte import do pracovního prostoru mbed][6]

1. <span data-ttu-id="ab628-171">Zadejte odkaz pro https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ kód ukázka v místním okně, pak klikněte na **Import**.</span><span class="sxs-lookup"><span data-stu-id="ab628-171">In the pop-up window, enter the link for the sample code https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ then click **Import**.</span></span>
   
    ![Import ukázkový kód do pracovního prostoru mbed][7]

1. <span data-ttu-id="ab628-173">Zobrazí se v okně kompilátoru mbed, import tento projekt taky importuje různých knihoven.</span><span class="sxs-lookup"><span data-stu-id="ab628-173">You can see in the mbed compiler window that importing this project also imports various libraries.</span></span> <span data-ttu-id="ab628-174">Některé zadané a udržovat tým Azure IoT ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), zatímco jiné jsou dostupné v katalogu knihoven mbed knihovny třetích stran.</span><span class="sxs-lookup"><span data-stu-id="ab628-174">Some are provided and maintained by the Azure IoT team ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), while others are third-party libraries available in the mbed libraries catalog.</span></span>
   
    ![Zobrazení mbed projektu][8]

1. <span data-ttu-id="ab628-176">V **prostoru programu**, klikněte pravým tlačítkem myši **iothub\_amqp\_přenosu** knihovny, klikněte na tlačítko **odstranit**a potom klikněte na  **OK** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="ab628-176">In the **Program Workspace**, right-click the **iothub\_amqp\_transport** library, click **Delete**, and then click **OK** to confirm.</span></span>

1. <span data-ttu-id="ab628-177">V **prostoru programu**, klikněte pravým tlačítkem myši **azure\_amqp\_c** knihovny, klikněte na tlačítko **odstranit**a potom klikněte na **OK** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="ab628-177">In the **Program Workspace**, right-click the **azure\_amqp\_c** library, click **Delete**, and then click **OK** to confirm.</span></span>

1. <span data-ttu-id="ab628-178">Klikněte pravým tlačítkem myši **remote_monitoring** projektu v **prostoru programu**, vyberte **knihovny importu**, pak vyberte **z adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="ab628-178">Right-click the **remote_monitoring** project in the **Program Workspace**, select **Import Library**, then select **From URL**.</span></span>
   
    ![Spuštění knihovny importu do pracovního prostoru mbed][6]

1. <span data-ttu-id="ab628-180">V místním okně, zadejte odkaz pro https://developer.mbed.org/users/AzureIoTClient/code/iothub knihovny přenosu MQTT\_mqtt\_přenosu / klikněte **Import**.</span><span class="sxs-lookup"><span data-stu-id="ab628-180">In the pop-up window, enter the link for the MQTT transport library https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport/ then click **Import**.</span></span>
   
    ![Import knihovny do pracovního prostoru mbed][12]

1. <span data-ttu-id="ab628-182">Opakujte předchozí krok pro přidání knihovně MQTT z https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.</span><span class="sxs-lookup"><span data-stu-id="ab628-182">Repeat the previous step to add the MQTT library from https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c/.</span></span>

1. <span data-ttu-id="ab628-183">Pracovní prostor teď vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="ab628-183">Your workspace now looks like the following:</span></span>

    ![Zobrazení mbed prostoru][13]

1. <span data-ttu-id="ab628-185">Otevřete vzdálených\_monitoring\remote_monitoring.c souboru a nahradit stávající `#include` příkazy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ab628-185">Open the remote\_monitoring\remote_monitoring.c file and replace the existing `#include` statements with the following code:</span></span>

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"

    #ifdef MBED_BUILD_TIMESTAMP
    #include "certs.h"
    #endif // MBED_BUILD_TIMESTAMP
    ```
1. <span data-ttu-id="ab628-186">Odstranit všechny zbývající kód ve vzdáleném\_monitoring\remote\_monitoring.c souboru.</span><span class="sxs-lookup"><span data-stu-id="ab628-186">Delete all the remaining code in the remote\_monitoring\remote\_monitoring.c file.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a><span data-ttu-id="ab628-187">Sestavit a spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="ab628-187">Build and run the sample</span></span>

<span data-ttu-id="ab628-188">Přidejte kód, který má být vyvolán **vzdáleného\_monitorování\_spustit** funkce a potom sestavit a spustit aplikaci zařízení.</span><span class="sxs-lookup"><span data-stu-id="ab628-188">Add code to invoke the **remote\_monitoring\_run** function and then build and run the device application.</span></span>

1. <span data-ttu-id="ab628-189">Přidat **hlavní** funkce s následujícím kódem na konci vzdálených\_monitoring.c soubor má být vyvolán **vzdáleného\_monitorování\_spustit** funkce:</span><span class="sxs-lookup"><span data-stu-id="ab628-189">Add a **main** function with following code at the end of the remote\_monitoring.c file to invoke the **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="ab628-190">Klikněte na tlačítko **zkompilovat** k vytvoření programu.</span><span class="sxs-lookup"><span data-stu-id="ab628-190">Click **Compile** to build the program.</span></span> <span data-ttu-id="ab628-191">Můžete bezpečně ignorovat všechna upozornění, ale pokud sestavení generuje chyby, opravte je než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ab628-191">You can safely ignore any warnings, but if the build generates errors, fix them before proceeding.</span></span>

1. <span data-ttu-id="ab628-192">Pokud sestavení úspěšné, web kompilátoru mbed generuje soubor .bin s názvem vašeho projektu a soubory ke stažení do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="ab628-192">If the build is successful, the mbed compiler website generates a .bin file with the name of your project and downloads it to your local machine.</span></span> <span data-ttu-id="ab628-193">Zkopírujte soubor .bin do zařízení.</span><span class="sxs-lookup"><span data-stu-id="ab628-193">Copy the .bin file to the device.</span></span> <span data-ttu-id="ab628-194">Ukládání souboru .bin do zařízení způsobí, že zařízení restartovat a poté spusťte program obsažené v souboru .bin.</span><span class="sxs-lookup"><span data-stu-id="ab628-194">Saving the .bin file to the device causes the device to restart and run the program contained in the .bin file.</span></span> <span data-ttu-id="ab628-195">Stisknutím tlačítka Obnovit v zařízení mbed můžete ručně restartovat program kdykoli.</span><span class="sxs-lookup"><span data-stu-id="ab628-195">You can manually restart the program at any time by pressing the reset button on the mbed device.</span></span>

1. <span data-ttu-id="ab628-196">Připojte k zařízení pomocí protokolu SSH klienta aplikace, jako je například PuTTY.</span><span class="sxs-lookup"><span data-stu-id="ab628-196">Connect to the device using an SSH client application, such as PuTTY.</span></span> <span data-ttu-id="ab628-197">Můžete určit sériového portu, který zařízení používá kontrolou Správci zařízení.</span><span class="sxs-lookup"><span data-stu-id="ab628-197">You can determine the serial port your device uses by checking Windows Device Manager.</span></span>
   
    ![][11]

1. <span data-ttu-id="ab628-198">V PuTTY, klikněte na **sériové** typ připojení.</span><span class="sxs-lookup"><span data-stu-id="ab628-198">In PuTTY, click the **Serial** connection type.</span></span> <span data-ttu-id="ab628-199">V 9600 přenosová obvykle připojení zařízení, takže zadejte 9600 v **rychlost** pole.</span><span class="sxs-lookup"><span data-stu-id="ab628-199">The device typically connects at 9600 baud, so enter 9600 in the **Speed** box.</span></span> <span data-ttu-id="ab628-200">Pak klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="ab628-200">Then click **Open**.</span></span>

1. <span data-ttu-id="ab628-201">Program spustí provádění.</span><span class="sxs-lookup"><span data-stu-id="ab628-201">The program starts executing.</span></span> <span data-ttu-id="ab628-202">Možná budete muset resetovat panel (stiskněte kombinaci kláves CTRL + Break nebo stiskněte tlačítko Obnovit panelu) Pokud program nespustí automaticky při připojení.</span><span class="sxs-lookup"><span data-stu-id="ab628-202">You may have to reset the board (press CTRL+Break or press the board's reset button) if the program does not start automatically when you connect.</span></span>
   
    ![][10]

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png
[12]: ./media/iot-suite-connecting-devices-mbed/mbed7.png
[13]: ./media/iot-suite-connecting-devices-mbed/mbed8.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
