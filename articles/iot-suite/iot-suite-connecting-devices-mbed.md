---
title: "aaaConnect zařízení pomocí jazyka C na mbed | Microsoft Docs"
description: "Popisuje, jak tooconnect toohello zařízení Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v jazyce C spuštění v zařízení s mbed."
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
ms.openlocfilehash: dcd1e74635e8dec678a59bff060a73f7cfabd124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-mbed"></a><span data-ttu-id="47a15-103">Připojit vaše zařízení toohello (mbed) předkonfigurovanému řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="47a15-103">Connect your device toohello remote monitoring preconfigured solution (mbed)</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="47a15-104">Přehled scénáře</span><span class="sxs-lookup"><span data-stu-id="47a15-104">Scenario overview</span></span>
<span data-ttu-id="47a15-105">V tomto scénáři vytvoříte zařízení, které odesílá hello následující telemetrie toohello vzdálené monitorování [předkonfigurované řešení][lnk-what-are-preconfig-solutions]:</span><span class="sxs-lookup"><span data-stu-id="47a15-105">In this scenario, you create a device that sends hello following telemetry toohello remote monitoring [preconfigured solution][lnk-what-are-preconfig-solutions]:</span></span>

* <span data-ttu-id="47a15-106">Venkovní teplota</span><span class="sxs-lookup"><span data-stu-id="47a15-106">External temperature</span></span>
* <span data-ttu-id="47a15-107">Vnitřní teplota</span><span class="sxs-lookup"><span data-stu-id="47a15-107">Internal temperature</span></span>
* <span data-ttu-id="47a15-108">Vlhkost</span><span class="sxs-lookup"><span data-stu-id="47a15-108">Humidity</span></span>

<span data-ttu-id="47a15-109">Pro jednoduchost hello kódu na zařízení hello generuje ukázkové hodnoty, ale doporučujeme vám tooextend hello ukázka připojením zařízení tooyour skutečné senzory a odesláním skutečné telemetrie.</span><span class="sxs-lookup"><span data-stu-id="47a15-109">For simplicity, hello code on hello device generates sample values, but we encourage you tooextend hello sample by connecting real sensors tooyour device and sending real telemetry.</span></span>

<span data-ttu-id="47a15-110">Hello zařízení je také možné toorespond toomethods volat z řídicí panel řešení hello a potřeby vlastnost hodnotami nastavenými v řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="47a15-110">hello device is also able toorespond toomethods invoked from hello solution dashboard and desired property values set in hello solution dashboard.</span></span>

<span data-ttu-id="47a15-111">toocomplete tohoto kurzu potřebujete aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="47a15-111">toocomplete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="47a15-112">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="47a15-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="47a15-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="47a15-113">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="47a15-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="47a15-114">Before you start</span></span>
<span data-ttu-id="47a15-115">Než začnete psát kód pro zařízení, je nutné zřídit předkonfigurované řešení vzdáleného monitorování a v něm zřídit nové vlastní zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a15-115">Before you write any code for your device, you must provision your remote monitoring preconfigured solution and provision a new custom device in that solution.</span></span>

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="47a15-116">Zřízení předkonfigurovaného řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="47a15-116">Provision your remote monitoring preconfigured solution</span></span>
<span data-ttu-id="47a15-117">Hello zařízení v tomto kurzu vytvoříte odesílá data tooan instanci hello [vzdálené monitorování] [ lnk-remote-monitoring] předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="47a15-117">hello device you create in this tutorial sends data tooan instance of hello [remote monitoring][lnk-remote-monitoring] preconfigured solution.</span></span> <span data-ttu-id="47a15-118">Pokud jste ještě nezřídili hello vzdálené monitorování předkonfigurované řešení v účtu Azure, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="47a15-118">If you haven't already provisioned hello remote monitoring preconfigured solution in your Azure account, use hello following steps:</span></span>

1. <span data-ttu-id="47a15-119">Na hello <https://www.azureiotsuite.com/> klikněte na tlačítko  **+**  toocreate řešení.</span><span class="sxs-lookup"><span data-stu-id="47a15-119">On hello <https://www.azureiotsuite.com/> page, click **+** toocreate a solution.</span></span>
2. <span data-ttu-id="47a15-120">Klikněte na tlačítko **vyberte** na hello **vzdálené monitorování** panelu toocreate řešení.</span><span class="sxs-lookup"><span data-stu-id="47a15-120">Click **Select** on hello **Remote monitoring** panel toocreate your solution.</span></span>
3. <span data-ttu-id="47a15-121">Na hello **vytvořit řešení vzdáleného sledování** stránky, zadejte **název řešení** podle vaší volby, vyberte hello **oblast** toodeploy do mají a vyberte hello Azure toouse toowant předplatné.</span><span class="sxs-lookup"><span data-stu-id="47a15-121">On hello **Create Remote monitoring solution** page, enter a **Solution name** of your choice, select hello **Region** you want toodeploy to, and select hello Azure subscription toowant toouse.</span></span> <span data-ttu-id="47a15-122">Potom klikněte na **Vytvořit řešení**.</span><span class="sxs-lookup"><span data-stu-id="47a15-122">Then click **Create solution**.</span></span>
4. <span data-ttu-id="47a15-123">Počkejte na dokončení procesu zřizování hello.</span><span class="sxs-lookup"><span data-stu-id="47a15-123">Wait until hello provisioning process completes.</span></span>

> [!WARNING]
> <span data-ttu-id="47a15-124">Hello předkonfigurované řešení využívají fakturovatelný služby Azure.</span><span class="sxs-lookup"><span data-stu-id="47a15-124">hello preconfigured solutions use billable Azure services.</span></span> <span data-ttu-id="47a15-125">Ujistěte se, tooremove hello předkonfigurované řešení ze svého předplatného, když jste hotovi s ním tooavoid všechny nepotřebné poplatky.</span><span class="sxs-lookup"><span data-stu-id="47a15-125">Be sure tooremove hello preconfigured solution from your subscription when you are done with it tooavoid any unnecessary charges.</span></span> <span data-ttu-id="47a15-126">Předkonfigurované řešení můžete úplně odebrat ze svého předplatného návštěvou hello <https://www.azureiotsuite.com/> stránky.</span><span class="sxs-lookup"><span data-stu-id="47a15-126">You can completely remove a preconfigured solution from your subscription by visiting hello <https://www.azureiotsuite.com/> page.</span></span>
> 
> 

<span data-ttu-id="47a15-127">Po dokončení hello zřizování pro hello řešení vzdáleného sledování, klikněte na tlačítko **spusťte** řídicí panel řešení hello tooopen v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="47a15-127">When hello provisioning process for hello remote monitoring solution finishes, click **Launch** tooopen hello solution dashboard in your browser.</span></span>

![Řídicí panel řešení][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a><span data-ttu-id="47a15-129">Zřízení zařízení v řešení vzdáleného sledování hello</span><span class="sxs-lookup"><span data-stu-id="47a15-129">Provision your device in hello remote monitoring solution</span></span>
> [!NOTE]
> <span data-ttu-id="47a15-130">Pokud jste už ve svém řešení zařízení zřídili, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="47a15-130">If you have already provisioned a device in your solution, you can skip this step.</span></span> <span data-ttu-id="47a15-131">Přihlašovací údaje tooknow hello zařízení musíte při vytváření hello klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="47a15-131">You need tooknow hello device credentials when you create hello client application.</span></span>
> 
> 

<span data-ttu-id="47a15-132">Pro zařízení tooconnect toohello předkonfigurované řešení, se musí identifikovat tooIoT centra pomocí platných přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="47a15-132">For a device tooconnect toohello preconfigured solution, it must identify itself tooIoT Hub using valid credentials.</span></span> <span data-ttu-id="47a15-133">Přihlašovací údaje hello zařízení můžete načíst z řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="47a15-133">You can retrieve hello device credentials from hello solution dashboard.</span></span> <span data-ttu-id="47a15-134">Přihlašovací údaje zařízení hello zahrnete do klientské aplikace později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="47a15-134">You include hello device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="47a15-135">tooadd zařízení tooyour řešení vzdáleného monitorování, dokončení hello následující kroky v řídicí panel řešení hello:</span><span class="sxs-lookup"><span data-stu-id="47a15-135">tooadd a device tooyour remote monitoring solution, complete hello following steps in hello solution dashboard:</span></span>

1. <span data-ttu-id="47a15-136">V hello levém dolním rohu hello řídicí panel, klikněte na **přidání zařízení**.</span><span class="sxs-lookup"><span data-stu-id="47a15-136">In hello lower left-hand corner of hello dashboard, click **Add a device**.</span></span>
   
   ![Přidání zařízení][1]
2. <span data-ttu-id="47a15-138">V hello **vlastní zařízení** panelu, klikněte na tlačítko **přidat nový**.</span><span class="sxs-lookup"><span data-stu-id="47a15-138">In hello **Custom Device** panel, click **Add new**.</span></span>
   
   ![Přidání vlastního zařízení][2]
3. <span data-ttu-id="47a15-140">Vyberte možnost **Definovat vlastní ID zařízení**.</span><span class="sxs-lookup"><span data-stu-id="47a15-140">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="47a15-141">Zadejte ID zařízení, jako **mydevice**, klikněte na tlačítko **Zkontrolujte ID** tooverify tento název již není používána a pak klikněte na tlačítko **vytvořit** tooprovision hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a15-141">Enter a Device ID such as **mydevice**, click **Check ID** tooverify that name isn't already in use, and then click **Create** tooprovision hello device.</span></span>
   
   ![Přidání ID zařízení][3]
4. <span data-ttu-id="47a15-143">Nastavit hello zařízení Poznámka: přihlašovací údaje (ID zařízení, název hostitele centra IoT a klíč zařízení).</span><span class="sxs-lookup"><span data-stu-id="47a15-143">Make a note hello device credentials (Device ID, IoT Hub Hostname, and Device Key).</span></span> <span data-ttu-id="47a15-144">Klientské aplikace, musí tyto hodnoty tooconnect toohello řešení vzdáleného sledování.</span><span class="sxs-lookup"><span data-stu-id="47a15-144">Your client application needs these values tooconnect toohello remote monitoring solution.</span></span> <span data-ttu-id="47a15-145">Potom klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="47a15-145">Then click **Done**.</span></span>
   
    ![Zobrazení přihlašovacích údajů zařízení][4]
5. <span data-ttu-id="47a15-147">Vyberte zařízení v seznamu zařízení hello v řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="47a15-147">Select your device in hello device list in hello solution dashboard.</span></span> <span data-ttu-id="47a15-148">Potom v hello **podrobnosti o zařízení** panelu, klikněte na tlačítko **povolit zařízení**.</span><span class="sxs-lookup"><span data-stu-id="47a15-148">Then, in hello **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="47a15-149">Hello stav zařízení je nyní **systémem**.</span><span class="sxs-lookup"><span data-stu-id="47a15-149">hello status of your device is now **Running**.</span></span> <span data-ttu-id="47a15-150">řešení vzdáleného monitorování Hello teď můžete přijímat telemetrická data ze zařízení a volat metody na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a15-150">hello remote monitoring solution can now receive telemetry from your device and invoke methods on hello device.</span></span>

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-hello-c-sample-solution"></a><span data-ttu-id="47a15-151">Sestavte a spusťte ukázkové řešení hello C</span><span class="sxs-lookup"><span data-stu-id="47a15-151">Build and run hello C sample solution</span></span>

<span data-ttu-id="47a15-152">Hello následující pokyny popisují postup hello připojení [povoleno mbed FRDM K64F Freescale] [ lnk-mbed-home] zařízení toohello řešení vzdáleného sledování.</span><span class="sxs-lookup"><span data-stu-id="47a15-152">hello following instructions describe hello steps for connecting an [mbed-enabled Freescale FRDM-K64F][lnk-mbed-home] device toohello remote monitoring solution.</span></span>

### <a name="connect-hello-mbed-device-tooyour-network-and-desktop-machine"></a><span data-ttu-id="47a15-153">Připojit hello mbed zařízení tooyour sítě a stolní počítače</span><span class="sxs-lookup"><span data-stu-id="47a15-153">Connect hello mbed device tooyour network and desktop machine</span></span>

1. <span data-ttu-id="47a15-154">Připojte hello mbed zařízení tooyour síti pomocí kabelu Ethernet.</span><span class="sxs-lookup"><span data-stu-id="47a15-154">Connect hello mbed device tooyour network using an Ethernet cable.</span></span> <span data-ttu-id="47a15-155">Tento krok je nezbytný, protože hello ukázkové aplikace vyžaduje přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="47a15-155">This step is necessary because hello sample application requires internet access.</span></span>

1. <span data-ttu-id="47a15-156">V tématu [Začínáme s mbed] [ lnk-mbed-getstarted] tooconnect mbed zařízení tooyour ploše počítače.</span><span class="sxs-lookup"><span data-stu-id="47a15-156">See [Getting Started with mbed][lnk-mbed-getstarted] tooconnect your mbed device tooyour desktop PC.</span></span>

1. <span data-ttu-id="47a15-157">Pokud počítače se systémem Windows, přečtěte si téma [konfigurace počítače] [ lnk-mbed-pcconnect] tooconfigure sériového portu přístup tooyour mbed zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a15-157">If your desktop PC is running Windows, see [PC Configuration][lnk-mbed-pcconnect] tooconfigure serial port access tooyour mbed device.</span></span>

### <a name="create-an-mbed-project-and-import-hello-sample-code"></a><span data-ttu-id="47a15-158">Vytvoření projektu mbed a importovat hello ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="47a15-158">Create an mbed project and import hello sample code</span></span>

<span data-ttu-id="47a15-159">Postupujte podle těchto kroků tooadd některé ukázkový kód tooan mbed projekt.</span><span class="sxs-lookup"><span data-stu-id="47a15-159">Follow these steps tooadd some sample code tooan mbed project.</span></span> <span data-ttu-id="47a15-160">Import projektu starter vzdálené monitorování hello a poté změňte hello projektu toouse hello MQTT protokol místo hello protokolu AMQP.</span><span class="sxs-lookup"><span data-stu-id="47a15-160">You import hello remote monitoring starter project and then change hello project toouse hello MQTT protocol instead of hello AMQP protocol.</span></span> <span data-ttu-id="47a15-161">V současné době je nutné toouse hello MQTT protokol toouse hello funkce správy zařízení služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="47a15-161">Currently, you need toouse hello MQTT protocol toouse hello device management features of IoT Hub.</span></span>

1. <span data-ttu-id="47a15-162">Ve webovém prohlížeči, přejděte toohello mbed.org [vývojáře lokality](https://developer.mbed.org/).</span><span class="sxs-lookup"><span data-stu-id="47a15-162">In your web browser, go toohello mbed.org [developer site](https://developer.mbed.org/).</span></span> <span data-ttu-id="47a15-163">Pokud nemáte registraci, zobrazí toocreate možnost účet (je to zdarma).</span><span class="sxs-lookup"><span data-stu-id="47a15-163">If you haven't signed up, you see an option toocreate an account (it's free).</span></span> <span data-ttu-id="47a15-164">Jinak Přihlaste se pomocí přihlašovacích údajů účtu.</span><span class="sxs-lookup"><span data-stu-id="47a15-164">Otherwise, log in with your account credentials.</span></span> <span data-ttu-id="47a15-165">Pak klikněte na tlačítko **kompilátoru** v horním pravém horním rohu stránky hello hello.</span><span class="sxs-lookup"><span data-stu-id="47a15-165">Then click **Compiler** in hello upper right-hand corner of hello page.</span></span> <span data-ttu-id="47a15-166">Tato akce přináší, toohello *prostoru* rozhraní.</span><span class="sxs-lookup"><span data-stu-id="47a15-166">This action brings you toohello *Workspace* interface.</span></span>

1. <span data-ttu-id="47a15-167">Zajistěte, aby se zobrazí v horním pravém horním rohu okna hello hello hello hardwarová platforma, kterou používáte, nebo klikněte na tlačítko hello ikonu v pravém rohu tooselect hello hardwarová platforma.</span><span class="sxs-lookup"><span data-stu-id="47a15-167">Make sure hello hardware platform you're using appears in hello upper right-hand corner of hello window, or click hello icon in hello right-hand corner tooselect your hardware platform.</span></span>

1. <span data-ttu-id="47a15-168">Klikněte na tlačítko **Import** v hlavní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="47a15-168">Click **Import** on hello main menu.</span></span> <span data-ttu-id="47a15-169">Pak klikněte na tlačítko **tooimport z adresy URL, klikněte sem**.</span><span class="sxs-lookup"><span data-stu-id="47a15-169">Then click **Click here tooimport from URL**.</span></span>
   
    ![Spusťte import toombed prostoru][6]

1. <span data-ttu-id="47a15-171">V místním okně hello, zadejte hello odkaz pro hello ukázkový kód https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ a klikněte na **Import**.</span><span class="sxs-lookup"><span data-stu-id="47a15-171">In hello pop-up window, enter hello link for hello sample code https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ then click **Import**.</span></span>
   
    ![Import ukázkový kód toombed prostoru][7]

1. <span data-ttu-id="47a15-173">Zobrazí se v okně kompilátoru mbed hello, import tento projekt taky importuje různých knihoven.</span><span class="sxs-lookup"><span data-stu-id="47a15-173">You can see in hello mbed compiler window that importing this project also imports various libraries.</span></span> <span data-ttu-id="47a15-174">Některé zadané a udržovat tým Azure IoT hello ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), zatímco jiné jsou dostupné v katalogu knihoven mbed hello knihovny třetích stran.</span><span class="sxs-lookup"><span data-stu-id="47a15-174">Some are provided and maintained by hello Azure IoT team ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), while others are third-party libraries available in hello mbed libraries catalog.</span></span>
   
    ![Zobrazení mbed projektu][8]

1. <span data-ttu-id="47a15-176">V hello **prostoru programu**, hello klikněte pravým tlačítkem na **iothub\_amqp\_přenosu** knihovny, klikněte na tlačítko **odstranit**a potom klikněte na **OK** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="47a15-176">In hello **Program Workspace**, right-click hello **iothub\_amqp\_transport** library, click **Delete**, and then click **OK** tooconfirm.</span></span>

1. <span data-ttu-id="47a15-177">V hello **prostoru programu**, klikněte pravým tlačítkem na hello **azure\_amqp\_c** knihovny, klikněte na tlačítko **odstranit**a potom klikněte na **OK**  tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="47a15-177">In hello **Program Workspace**, right-click hello **azure\_amqp\_c** library, click **Delete**, and then click **OK** tooconfirm.</span></span>

1. <span data-ttu-id="47a15-178">Klikněte pravým tlačítkem na hello **remote_monitoring** projekt v hello **prostoru programu**, vyberte **knihovny importu**, pak vyberte **z adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="47a15-178">Right-click hello **remote_monitoring** project in hello **Program Workspace**, select **Import Library**, then select **From URL**.</span></span>
   
    ![Spuštění knihovny importu toombed prostoru][6]

1. <span data-ttu-id="47a15-180">V místním okně hello, zadejte hello odkaz pro hello MQTT přenosu knihovny https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_přenosu / klikněte **Import**.</span><span class="sxs-lookup"><span data-stu-id="47a15-180">In hello pop-up window, enter hello link for hello MQTT transport library https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport/ then click **Import**.</span></span>
   
    ![Importovat knihovny toombed prostoru][12]

1. <span data-ttu-id="47a15-182">Hello zopakujte předchozí krok tooadd hello MQTT knihovny z https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.</span><span class="sxs-lookup"><span data-stu-id="47a15-182">Repeat hello previous step tooadd hello MQTT library from https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c/.</span></span>

1. <span data-ttu-id="47a15-183">Pracovní prostor se teď vypadá jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="47a15-183">Your workspace now looks like hello following:</span></span>

    ![Zobrazení mbed prostoru][13]

1. <span data-ttu-id="47a15-185">Otevřete hello vzdálené\_monitoring\remote_monitoring.c souboru a nahradit stávající hello `#include` příkazy s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="47a15-185">Open hello remote\_monitoring\remote_monitoring.c file and replace hello existing `#include` statements with hello following code:</span></span>

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
1. <span data-ttu-id="47a15-186">Odstranit všechny hello zbývající kód v hello vzdálené\_monitoring\remote\_monitoring.c souboru.</span><span class="sxs-lookup"><span data-stu-id="47a15-186">Delete all hello remaining code in hello remote\_monitoring\remote\_monitoring.c file.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a><span data-ttu-id="47a15-187">Sestavení a spuštění ukázkových hello</span><span class="sxs-lookup"><span data-stu-id="47a15-187">Build and run hello sample</span></span>

<span data-ttu-id="47a15-188">Přidat kód tooinvoke hello **vzdáleného\_monitorování\_spustit** funkce a potom sestavení a spuštění aplikace hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a15-188">Add code tooinvoke hello **remote\_monitoring\_run** function and then build and run hello device application.</span></span>

1. <span data-ttu-id="47a15-189">Přidat **hlavní** funkce s následujícím kódem na konci hello hello vzdálené\_monitoring.c souboru tooinvoke hello **vzdáleného\_monitorování\_spustit** funkce:</span><span class="sxs-lookup"><span data-stu-id="47a15-189">Add a **main** function with following code at hello end of hello remote\_monitoring.c file tooinvoke hello **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="47a15-190">Klikněte na tlačítko **zkompilovat** toobuild programu hello.</span><span class="sxs-lookup"><span data-stu-id="47a15-190">Click **Compile** toobuild hello program.</span></span> <span data-ttu-id="47a15-191">Můžete bezpečně ignorovat všechna upozornění, ale pokud hello sestavení generuje chyby, opravte je než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="47a15-191">You can safely ignore any warnings, but if hello build generates errors, fix them before proceeding.</span></span>

1. <span data-ttu-id="47a15-192">Pokud sestavení hello úspěšné, hello mbed kompilátoru web generuje soubor .bin s hello název projektu a ji stáhne tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="47a15-192">If hello build is successful, hello mbed compiler website generates a .bin file with hello name of your project and downloads it tooyour local machine.</span></span> <span data-ttu-id="47a15-193">Zkopírujte hello .bin souboru toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a15-193">Copy hello .bin file toohello device.</span></span> <span data-ttu-id="47a15-194">Ukládání zařízení toohello soubor .bin hello způsobí, že zařízení toorestart hello a spusťte program hello obsažené v souboru .bin hello.</span><span class="sxs-lookup"><span data-stu-id="47a15-194">Saving hello .bin file toohello device causes hello device toorestart and run hello program contained in hello .bin file.</span></span> <span data-ttu-id="47a15-195">Kdykoli můžete ručně restartovat programu hello stisknutím tlačítka resetování hello v hello mbed zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a15-195">You can manually restart hello program at any time by pressing hello reset button on hello mbed device.</span></span>

1. <span data-ttu-id="47a15-196">Připojte zařízení toohello pomocí SSH klienta aplikace, jako je například PuTTY.</span><span class="sxs-lookup"><span data-stu-id="47a15-196">Connect toohello device using an SSH client application, such as PuTTY.</span></span> <span data-ttu-id="47a15-197">Můžete určit hello sériového portu, který zařízení používá kontrolou Správci zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a15-197">You can determine hello serial port your device uses by checking Windows Device Manager.</span></span>
   
    ![][11]

1. <span data-ttu-id="47a15-198">V PuTTY, klikněte na tlačítko hello **sériové** typ připojení.</span><span class="sxs-lookup"><span data-stu-id="47a15-198">In PuTTY, click hello **Serial** connection type.</span></span> <span data-ttu-id="47a15-199">Hello zařízení obvykle připojí na 9600 přenosová, takže zadejte 9600 hello **rychlost** pole.</span><span class="sxs-lookup"><span data-stu-id="47a15-199">hello device typically connects at 9600 baud, so enter 9600 in hello **Speed** box.</span></span> <span data-ttu-id="47a15-200">Pak klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="47a15-200">Then click **Open**.</span></span>

1. <span data-ttu-id="47a15-201">spuštění programu Hello provádění.</span><span class="sxs-lookup"><span data-stu-id="47a15-201">hello program starts executing.</span></span> <span data-ttu-id="47a15-202">Pokud hello program nespustí automaticky při připojení, mohou mít tooreset hello Tabule (stiskněte kombinaci kláves CTRL + Break nebo Tabule hello stiskněte tlačítko pro obnovení).</span><span class="sxs-lookup"><span data-stu-id="47a15-202">You may have tooreset hello board (press CTRL+Break or press hello board's reset button) if hello program does not start automatically when you connect.</span></span>
   
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
