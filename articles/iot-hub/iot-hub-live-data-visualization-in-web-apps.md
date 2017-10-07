---
title: "vizualizace dat aaaReal čas dat snímačů z Azure IoT hub – Web Apps | Microsoft Docs"
description: "Pomocí funkce hello webové aplikace Microsoft Azure App Service toovisualize teploty a vlhkosti dat, která se shromažďují ze snímačů hello a odesílá tooyour Iot hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "vizualizace dat reálném čase, vizualizace dat za provozu, vizualizace dat snímačů"
ms.assetid: e42b07a8-ddd4-476e-9bfb-903d6b033e91
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 72f2dffee1c2f975948820eee9f2e287c3f77255
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-hello-web-apps-feature-of-azure-app-service"></a><span data-ttu-id="6c38a-104">Vizualizovat data snímačů v reálném čase ze služby Azure IoT hub pomocí hello funkce Web Apps služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6c38a-104">Visualize real-time sensor data from your Azure IoT hub by using hello Web Apps feature of Azure App Service</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="6c38a-106">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="6c38a-106">What you learn</span></span>

<span data-ttu-id="6c38a-107">V tomto kurzu zjistíte, jak je toovisualize data snímačů v reálném čase, která IoT hub přijímá spuštěním webové aplikace hostované na webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6c38a-107">In this tutorial, you learn how toovisualize real-time sensor data that your IoT hub receives by running a web application that is hosted on a web app.</span></span> <span data-ttu-id="6c38a-108">Pokud chcete tootry toovisualize hello data ve službě IoT hub pomocí Power BI, najdete v části [data snímačů v reálném čase pomocí Power BI toovisualize ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="6c38a-108">If you want tootry toovisualize hello data in your IoT hub by using Power BI, see [Use Power BI toovisualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="6c38a-109">Co dělat</span><span class="sxs-lookup"><span data-stu-id="6c38a-109">What you do</span></span>

- <span data-ttu-id="6c38a-110">Vytvoření webové aplikace v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6c38a-110">Create a web app in hello Azure portal.</span></span>
- <span data-ttu-id="6c38a-111">Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="6c38a-111">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="6c38a-112">Nakonfigurujte data snímačů hello webové aplikace tooread ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c38a-112">Configure hello web app tooread sensor data from your IoT hub.</span></span>
- <span data-ttu-id="6c38a-113">Nahrajte toobe webové aplikace hostované hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c38a-113">Upload a web application toobe hosted by hello web app.</span></span>
- <span data-ttu-id="6c38a-114">Otevřete hello webové aplikace toosee dat v reálném čase teploty a vlhkosti ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c38a-114">Open hello web app toosee real-time temperature and humidity data from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6c38a-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="6c38a-115">What you need</span></span>

- <span data-ttu-id="6c38a-116">[Nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md), která zahrnuje hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="6c38a-116">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md), which covers hello following requirements:</span></span>
  - <span data-ttu-id="6c38a-117">Aktivní předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="6c38a-117">An active Azure subscription</span></span>
  - <span data-ttu-id="6c38a-118">Služby Iot hub v rámci svého předplatného</span><span class="sxs-lookup"><span data-stu-id="6c38a-118">An Iot hub under your subscription</span></span>
  - <span data-ttu-id="6c38a-119">Klientská aplikace, která odesílá zprávy tooyour Iot hub</span><span class="sxs-lookup"><span data-stu-id="6c38a-119">A client application that sends messages tooyour Iot hub</span></span>
- [<span data-ttu-id="6c38a-120">Stáhněte si Git</span><span class="sxs-lookup"><span data-stu-id="6c38a-120">Download Git</span></span>](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a><span data-ttu-id="6c38a-121">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="6c38a-121">Create a web app</span></span>

1. <span data-ttu-id="6c38a-122">V hello [portál Azure](https://ms.portal.azure.com/), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6c38a-122">In hello [Azure portal](https://ms.portal.azure.com/), click **New** > **Web + Mobile** > **Web App**.</span></span>
2. <span data-ttu-id="6c38a-123">Zadejte název jedinečné úlohy ověřte hello předplatné, zadejte skupinu prostředků a umístění, vyberte **Pin toodashboard**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6c38a-123">Enter a unique job name, verify hello subscription, specify a resource group and a location, select **Pin toodashboard**, and then click **Create**.</span></span>

   <span data-ttu-id="6c38a-124">Doporučujeme vám, že jste vybrali hello stejné umístění jako u vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6c38a-124">We recommend that you select hello same location as that of your resource group.</span></span> <span data-ttu-id="6c38a-125">To pomáhá zajistit s rychlost zpracování a snižuje náklady na hello přenosu dat.</span><span class="sxs-lookup"><span data-stu-id="6c38a-125">Doing so assists with processing speed and reduces hello cost of data transfer.</span></span>

   ![Vytvoření webové aplikace](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-hello-web-app-tooread-data-from-your-iot-hub"></a><span data-ttu-id="6c38a-127">Nakonfigurovat hello webové aplikace tooread data ze služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="6c38a-127">Configure hello web app tooread data from your IoT hub</span></span>

1. <span data-ttu-id="6c38a-128">Otevřete hello webovou aplikaci, kterou jste právě zřídili.</span><span class="sxs-lookup"><span data-stu-id="6c38a-128">Open hello web app you’ve just provisioned.</span></span>
2. <span data-ttu-id="6c38a-129">Klikněte na tlačítko **nastavení aplikace**a pak v části **nastavení aplikace**, přidejte následující páry klíč – hodnota hello:</span><span class="sxs-lookup"><span data-stu-id="6c38a-129">Click **Application settings**, and then, under **App settings**, add hello following key/value pairs:</span></span>

   | <span data-ttu-id="6c38a-130">Klíč</span><span class="sxs-lookup"><span data-stu-id="6c38a-130">Key</span></span>                                   | <span data-ttu-id="6c38a-131">Hodnota</span><span class="sxs-lookup"><span data-stu-id="6c38a-131">Value</span></span>                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | <span data-ttu-id="6c38a-132">Azure.IoT.IoTHub.ConnectionString</span><span class="sxs-lookup"><span data-stu-id="6c38a-132">Azure.IoT.IoTHub.ConnectionString</span></span>     | <span data-ttu-id="6c38a-133">Získat z Průzkumníka iothub</span><span class="sxs-lookup"><span data-stu-id="6c38a-133">Obtained from iothub-explorer</span></span>                                |
   | <span data-ttu-id="6c38a-134">Azure.IoT.IoTHub.ConsumerGroup</span><span class="sxs-lookup"><span data-stu-id="6c38a-134">Azure.IoT.IoTHub.ConsumerGroup</span></span>        | <span data-ttu-id="6c38a-135">Hello název skupiny příjemců hello přidat tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="6c38a-135">hello name of hello consumer group that you add tooyour IoT hub</span></span>  |

   ![Přidat nastavení tooyour webové aplikace s páry klíč – hodnota](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. <span data-ttu-id="6c38a-137">Klikněte na tlačítko **nastavení aplikace**v části **obecné nastavení**, přepnutí hello **webové sokety** a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6c38a-137">Click **Application settings**, under **General settings**, toggle hello **Web sockets** option, and then click **Save**.</span></span>

   ![Přepněte možnost sockets webové hello](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-toobe-hosted-by-hello-web-app"></a><span data-ttu-id="6c38a-139">Nahrát toobe webové aplikace hostované hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="6c38a-139">Upload a web application toobe hosted by hello web app</span></span>

<span data-ttu-id="6c38a-140">Na Githubu jsme provedli jsme k dispozici webové aplikace, které se zobrazují data snímačů v reálném čase ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c38a-140">On GitHub, we've made available a web application that displays real-time sensor data from your IoT hub.</span></span> <span data-ttu-id="6c38a-141">Stačí toodo je hello webové aplikace toowork nakonfigurovat úložiště Git, stáhněte hello webové aplikace z webu GitHub a nahrajte ho tooAzure pro hello webové aplikace toohost.</span><span class="sxs-lookup"><span data-stu-id="6c38a-141">All you need toodo is configure hello web app toowork with a Git repository, download hello web application from GitHub, and then upload it tooAzure for hello web app toohost.</span></span>

1. <span data-ttu-id="6c38a-142">V hello webové aplikace, klikněte na **možnosti nasazení** > **zvolit zdroj** > **místní úložiště Git**a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c38a-142">In hello web app, click **Deployment Options** > **Choose Source** > **Local Git Repository**, and then click **OK**.</span></span>

   ![Konfigurace webové aplikace nasazení toouse hello místní úložiště Git](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. <span data-ttu-id="6c38a-144">Klikněte na tlačítko **přihlašovací údaje nasazení**, vytvořte uživatelské jméno a heslo toouse tooconnect toohello úložiště Git v Azure a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6c38a-144">Click **Deployment Credentials**, create a user name and password toouse tooconnect toohello Git repository in Azure, and then click **Save**.</span></span>

3. <span data-ttu-id="6c38a-145">Klikněte na tlačítko **přehled**a poznamenejte si hodnotu hello **adresu url pro Git clone**.</span><span class="sxs-lookup"><span data-stu-id="6c38a-145">Click **Overview**, and note hello value of **Git clone url**.</span></span>

   ![Získat adresu URL klonu Git hello vaší webové aplikace](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. <span data-ttu-id="6c38a-147">Otevřete okno terminálu v místním počítači nebo příkaz.</span><span class="sxs-lookup"><span data-stu-id="6c38a-147">Open a command or terminal window on your local computer.</span></span>

5. <span data-ttu-id="6c38a-148">Stažení hello webové aplikace z webu GitHub a nahrajte ho tooAzure pro hello webové aplikace toohost.</span><span class="sxs-lookup"><span data-stu-id="6c38a-148">Download hello web app from GitHub, and upload it tooAzure for hello web app toohost.</span></span> <span data-ttu-id="6c38a-149">toodo tedy spusťte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6c38a-149">toodo so, run hello following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > <span data-ttu-id="6c38a-150">\<Adresa URL klonu Git\> je hello URL úložiště Git hello najít na hello **přehled** stránku hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c38a-150">\<Git clone URL\> is hello URL of hello Git repository found on hello **Overview** page of hello web app.</span></span>

## <a name="open-hello-web-app-toosee-real-time-temperature-and-humidity-data-from-your-iot-hub"></a><span data-ttu-id="6c38a-151">Otevřete hello webové aplikace toosee dat v reálném čase teploty a vlhkosti ze služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="6c38a-151">Open hello web app toosee real-time temperature and humidity data from your IoT hub</span></span>

<span data-ttu-id="6c38a-152">Na hello **přehled** stránku vaší webové aplikace, klikněte na tlačítko hello URL tooopen hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c38a-152">On hello **Overview** page of your web app, click hello URL tooopen hello web app.</span></span>

![Získat adresu URL hello vaší webové aplikace](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

<span data-ttu-id="6c38a-154">Měli byste vidět hello v reálném čase teploty a vlhkosti data ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c38a-154">You should see hello real-time temperature and humidity data from your IoT hub.</span></span>

![Webové stránky aplikace zobrazuje v reálném čase teploty a vlhkosti](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> <span data-ttu-id="6c38a-156">Zkontrolujte, jestli hello ukázkové aplikace běží na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c38a-156">Ensure hello sample application is running on your device.</span></span> <span data-ttu-id="6c38a-157">Pokud není, zobrazí se prázdný graf, můžete se podívat toohello kurzy pod [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6c38a-157">If not, you will get a blank chart, you can refer toohello tutorials under [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c38a-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c38a-158">Next steps</span></span>
<span data-ttu-id="6c38a-159">Úspěšně jste použili vaší webové aplikace toovisualize snímačů v reálném čase data ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c38a-159">You've successfully used your web app toovisualize real-time sensor data from your IoT hub.</span></span>

<span data-ttu-id="6c38a-160">Toovisualize alternativní způsob data ze služby Azure IoT Hub, najdete v části [data snímačů v reálném čase pomocí Power BI toovisualize ze služby IoT hub](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="6c38a-160">For an alternative way toovisualize data from Azure IoT Hub, see [Use Power BI toovisualize real-time sensor data from your IoT hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
