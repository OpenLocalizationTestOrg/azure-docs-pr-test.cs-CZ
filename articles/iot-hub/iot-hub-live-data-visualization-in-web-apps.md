---
title: "Vizualizace dat v reálném čase s daty ze snímačů z Azure IoT hub – Web Apps | Microsoft Docs"
description: "Použijte funkci webové aplikace Microsoft Azure App Service k vizualizaci dat teploty a vlhkosti, který se shromažďují ze senzoru a odesílá do služby Iot hub."
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
ms.openlocfilehash: e037f5c29cabf8e5d0d3e7ded187280a0652d5c3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-the-web-apps-feature-of-azure-app-service"></a><span data-ttu-id="6c2ce-104">Vizualizovat data snímačů v reálném čase ze služby Azure IoT hub pomocí funkce Web Apps služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6c2ce-104">Visualize real-time sensor data from your Azure IoT hub by using the Web Apps feature of Azure App Service</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="6c2ce-106">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="6c2ce-106">What you learn</span></span>

<span data-ttu-id="6c2ce-107">V tomto kurzu zjistěte, jak k vizualizaci dat snímačů v reálném čase, který IoT hub přijímá spuštěním webové aplikace, která je hostovaná ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-107">In this tutorial, you learn how to visualize real-time sensor data that your IoT hub receives by running a web application that is hosted on a web app.</span></span> <span data-ttu-id="6c2ce-108">Pokud chcete, zkuste k vizualizaci dat ve službě IoT hub pomocí Power BI, najdete v článku [pomocí Power BI k vizualizaci dat snímačů v reálném čase ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="6c2ce-108">If you want to try to visualize the data in your IoT hub by using Power BI, see [Use Power BI to visualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="6c2ce-109">Co dělat</span><span class="sxs-lookup"><span data-stu-id="6c2ce-109">What you do</span></span>

- <span data-ttu-id="6c2ce-110">Vytvořte webovou aplikaci na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-110">Create a web app in the Azure portal.</span></span>
- <span data-ttu-id="6c2ce-111">Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-111">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="6c2ce-112">Konfigurace webové aplikace načíst data snímačů ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-112">Configure the web app to read sensor data from your IoT hub.</span></span>
- <span data-ttu-id="6c2ce-113">Nahrajte webovou aplikaci pro hostování webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-113">Upload a web application to be hosted by the web app.</span></span>
- <span data-ttu-id="6c2ce-114">Otevřete webovou aplikaci se zobrazí v reálném čase teploty a vlhkosti data ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-114">Open the web app to see real-time temperature and humidity data from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6c2ce-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="6c2ce-115">What you need</span></span>

- <span data-ttu-id="6c2ce-116">[Nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md), která zahrnuje následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="6c2ce-116">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md), which covers the following requirements:</span></span>
  - <span data-ttu-id="6c2ce-117">Aktivní předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="6c2ce-117">An active Azure subscription</span></span>
  - <span data-ttu-id="6c2ce-118">Služby Iot hub v rámci svého předplatného</span><span class="sxs-lookup"><span data-stu-id="6c2ce-118">An Iot hub under your subscription</span></span>
  - <span data-ttu-id="6c2ce-119">Klientská aplikace, která odesílá zprávy do služby Iot hub</span><span class="sxs-lookup"><span data-stu-id="6c2ce-119">A client application that sends messages to your Iot hub</span></span>
- [<span data-ttu-id="6c2ce-120">Stáhněte si Git</span><span class="sxs-lookup"><span data-stu-id="6c2ce-120">Download Git</span></span>](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a><span data-ttu-id="6c2ce-121">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="6c2ce-121">Create a web app</span></span>

1. <span data-ttu-id="6c2ce-122">V [portál Azure](https://ms.portal.azure.com/), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-122">In the [Azure portal](https://ms.portal.azure.com/), click **New** > **Web + Mobile** > **Web App**.</span></span>
2. <span data-ttu-id="6c2ce-123">Zadejte název jedinečné úlohy ověřte předplatné, zadejte skupinu prostředků a umístění, vyberte **připnout na řídicí panel**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-123">Enter a unique job name, verify the subscription, specify a resource group and a location, select **Pin to dashboard**, and then click **Create**.</span></span>

   <span data-ttu-id="6c2ce-124">Doporučujeme vám, vyberte stejné umístění, jako vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-124">We recommend that you select the same location as that of your resource group.</span></span> <span data-ttu-id="6c2ce-125">To pomáhá zajistit s rychlost zpracování a snižuje náklady na přenos dat.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-125">Doing so assists with processing speed and reduces the cost of data transfer.</span></span>

   ![Vytvoření webové aplikace](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a><span data-ttu-id="6c2ce-127">Konfigurace webové aplikace načíst data ze služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="6c2ce-127">Configure the web app to read data from your IoT hub</span></span>

1. <span data-ttu-id="6c2ce-128">Otevřete webovou aplikaci, kterou jste právě zřídili.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-128">Open the web app you’ve just provisioned.</span></span>
2. <span data-ttu-id="6c2ce-129">Klikněte na tlačítko **nastavení aplikace**a pak v části **nastavení aplikace**, přidejte následující páry klíč – hodnota:</span><span class="sxs-lookup"><span data-stu-id="6c2ce-129">Click **Application settings**, and then, under **App settings**, add the following key/value pairs:</span></span>

   | <span data-ttu-id="6c2ce-130">Klíč</span><span class="sxs-lookup"><span data-stu-id="6c2ce-130">Key</span></span>                                   | <span data-ttu-id="6c2ce-131">Hodnota</span><span class="sxs-lookup"><span data-stu-id="6c2ce-131">Value</span></span>                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | <span data-ttu-id="6c2ce-132">Azure.IoT.IoTHub.ConnectionString</span><span class="sxs-lookup"><span data-stu-id="6c2ce-132">Azure.IoT.IoTHub.ConnectionString</span></span>     | <span data-ttu-id="6c2ce-133">Získat z Průzkumníka iothub</span><span class="sxs-lookup"><span data-stu-id="6c2ce-133">Obtained from iothub-explorer</span></span>                                |
   | <span data-ttu-id="6c2ce-134">Azure.IoT.IoTHub.ConsumerGroup</span><span class="sxs-lookup"><span data-stu-id="6c2ce-134">Azure.IoT.IoTHub.ConsumerGroup</span></span>        | <span data-ttu-id="6c2ce-135">Název skupiny uživatelů, která přidáte do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="6c2ce-135">The name of the consumer group that you add to your IoT hub</span></span>  |

   ![Přidat nastavení do vaší webové aplikace s páry klíč – hodnota](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. <span data-ttu-id="6c2ce-137">Klikněte na tlačítko **nastavení aplikace**v části **obecné nastavení**, přepnutí **webové sokety** a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-137">Click **Application settings**, under **General settings**, toggle the **Web sockets** option, and then click **Save**.</span></span>

   ![Přepněte možnost webové sokety](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a><span data-ttu-id="6c2ce-139">Nahrát webovou aplikaci pro hostování webových aplikací</span><span class="sxs-lookup"><span data-stu-id="6c2ce-139">Upload a web application to be hosted by the web app</span></span>

<span data-ttu-id="6c2ce-140">Na Githubu jsme provedli jsme k dispozici webové aplikace, které se zobrazují data snímačů v reálném čase ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-140">On GitHub, we've made available a web application that displays real-time sensor data from your IoT hub.</span></span> <span data-ttu-id="6c2ce-141">Všechny, které musíte udělat, je konfigurace webové aplikace pro práci s úložiště Git, stahování webové aplikace z webu GitHub a nahrajte ho do Azure pro webovou aplikaci na hostitele.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-141">All you need to do is configure the web app to work with a Git repository, download the web application from GitHub, and then upload it to Azure for the web app to host.</span></span>

1. <span data-ttu-id="6c2ce-142">Ve webové aplikaci, klikněte na tlačítko **možnosti nasazení** > **zvolit zdroj** > **místní úložiště Git**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-142">In the web app, click **Deployment Options** > **Choose Source** > **Local Git Repository**, and then click **OK**.</span></span>

   ![Konfigurace nasazení webové aplikace použít místní úložiště Git](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. <span data-ttu-id="6c2ce-144">Klikněte na tlačítko **přihlašovací údaje nasazení**, vytvořte uživatelské jméno a heslo použít k připojení do úložiště Git v Azure a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-144">Click **Deployment Credentials**, create a user name and password to use to connect to the Git repository in Azure, and then click **Save**.</span></span>

3. <span data-ttu-id="6c2ce-145">Klikněte na tlačítko **přehled**a poznamenejte si hodnotu **adresu url pro Git clone**.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-145">Click **Overview**, and note the value of **Git clone url**.</span></span>

   ![Získat adresu URL klonu Git webové aplikace](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. <span data-ttu-id="6c2ce-147">Otevřete okno terminálu v místním počítači nebo příkaz.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-147">Open a command or terminal window on your local computer.</span></span>

5. <span data-ttu-id="6c2ce-148">Stažení webové aplikace z webu GitHub a nahrajte ho do Azure pro webovou aplikaci na hostitele.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-148">Download the web app from GitHub, and upload it to Azure for the web app to host.</span></span> <span data-ttu-id="6c2ce-149">Uděláte to tak, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6c2ce-149">To do so, run the following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > <span data-ttu-id="6c2ce-150">\<Adresa URL klonu Git\> je adresa URL úložiště Git v nalezen **přehled** stránku webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-150">\<Git clone URL\> is the URL of the Git repository found on the **Overview** page of the web app.</span></span>

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a><span data-ttu-id="6c2ce-151">Otevřete webovou aplikaci se zobrazí v reálném čase teploty a vlhkosti data ze služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="6c2ce-151">Open the web app to see real-time temperature and humidity data from your IoT hub</span></span>

<span data-ttu-id="6c2ce-152">Na **přehled** stránku vaší webové aplikace, klikněte na adresu URL otevřete webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-152">On the **Overview** page of your web app, click the URL to open the web app.</span></span>

![Získat adresu URL webové aplikace](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

<span data-ttu-id="6c2ce-154">Měli byste vidět data v reálném čase teploty a vlhkosti ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-154">You should see the real-time temperature and humidity data from your IoT hub.</span></span>

![Webové stránky aplikace zobrazuje v reálném čase teploty a vlhkosti](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> <span data-ttu-id="6c2ce-156">Zkontrolujte, jestli že ukázkové aplikace běží na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-156">Ensure the sample application is running on your device.</span></span> <span data-ttu-id="6c2ce-157">Pokud není, zobrazí se prázdný graf, najdete podrobné pokyny v části [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6c2ce-157">If not, you will get a blank chart, you can refer to the tutorials under [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c2ce-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c2ce-158">Next steps</span></span>
<span data-ttu-id="6c2ce-159">Webové aplikace jste úspěšně použili k vizualizaci dat snímačů v reálném čase ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6c2ce-159">You've successfully used your web app to visualize real-time sensor data from your IoT hub.</span></span>

<span data-ttu-id="6c2ce-160">Alternativní způsob k vizualizaci dat ze služby Azure IoT Hub, najdete v části [pomocí Power BI k vizualizaci dat snímačů v reálném čase ze služby IoT hub](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="6c2ce-160">For an alternative way to visualize data from Azure IoT Hub, see [Use Power BI to visualize real-time sensor data from your IoT hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
