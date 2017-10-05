---
title: "Jak používat vlastní image Docker pro webové aplikace Azure v systému Linux | Microsoft Docs"
description: "Jak používat vlastní image Docker pro webové aplikace Azure v systému Linux."
keywords: "služby Azure app service, webové aplikace, linux, docker, kontejneru"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 1458217a31c4781b28877c030a665f5b22819e13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a><span data-ttu-id="5e1b6-104">Použití vlastní image Docker pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5e1b6-104">Using a custom Docker image for Azure Web App on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="5e1b6-105">Služba App Service poskytuje zásobníky předem definované aplikací v systému Linux s podporou pro konkrétní verze, jako je například PHP 7.0 a Node.js 4.5.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-105">App Service provides pre-defined application stacks on Linux with support for specific versions, such as PHP 7.0 and Node.js 4.5.</span></span> <span data-ttu-id="5e1b6-106">Aplikační služby v systému Linux využívá Docker kontejnery k hostování těchto zásobníky předem sestavených aplikací.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-106">App Service on Linux uses Docker containers to host these pre-built application stacks.</span></span> <span data-ttu-id="5e1b6-107">Vlastní image Docker můžete také použít k nasazení vaší webové aplikace do zásobník aplikací, které již nejsou definované v Azure.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-107">You can also use a custom Docker image to deploy your web app to an application stack that is not already defined in Azure.</span></span> <span data-ttu-id="5e1b6-108">Vlastní imagí Dockeru může být hostitelem buď veřejných nebo privátních Docker úložiště.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-108">Custom Docker images can be hosted on either a public or private Docker repository.</span></span>


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a><span data-ttu-id="5e1b6-109">Postupy: nastavení vlastní image Docker pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="5e1b6-109">How to: set a custom Docker image for a web app</span></span>
<span data-ttu-id="5e1b6-110">Můžete nastavit vlastní obrázek Docker pro obě aplikace a weby nových nebo stávajících.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-110">You can set the custom Docker image for both new and existing webs apps.</span></span> <span data-ttu-id="5e1b6-111">Při vytváření webové aplikace v systému Linux v [portál Azure](https://portal.azure.com/#create/Microsoft.AppSvcLinux), klikněte na tlačítko **kontejneru konfigurace** nastavit vlastní image Docker:</span><span class="sxs-lookup"><span data-stu-id="5e1b6-111">When you create a web app on Linux in the [Azure portal](https://portal.azure.com/#create/Microsoft.AppSvcLinux), click **Configure container** to set a custom Docker image:</span></span>

![Vlastní Image Docker pro novou webovou aplikaci v systému Linux][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a><span data-ttu-id="5e1b6-113">Postupy: použití vlastní image Docker z úložiště Docker Hub</span><span class="sxs-lookup"><span data-stu-id="5e1b6-113">How to: use a custom Docker image from Docker Hub</span></span> ##
<span data-ttu-id="5e1b6-114">Použití vlastní image Docker z úložiště Docker Hub:</span><span class="sxs-lookup"><span data-stu-id="5e1b6-114">To use a custom Docker image from Docker Hub:</span></span>

1. <span data-ttu-id="5e1b6-115">V [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-115">In the [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="5e1b6-116">Vyberte **úložiště Docker Hub** jako **zdroj bitové kopie**, pak klikněte buď na **veřejné** nebo **privátní** a zadejte **bitové kopie a název značky volitelné**, jako například `node:4.5`.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-116">Select **Docker Hub** as the **Image source**, then click either **Public** or **Private** and type the **Image and optional tag name**, such as `node:4.5`.</span></span> <span data-ttu-id="5e1b6-117">**Spuštění příkazu** je sada automaticky na základě co je definována v souboru bitové kopie Docker, ale můžete nastavit vlastní příkazy.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-117">The **Startup command** is set automatically based on what is defined in the Docker image file, but you can set your own commands.</span></span>  

    ![Obrázek veřejného úložiště Docker Hub konfigurace][2]

    <span data-ttu-id="5e1b6-119">Když bitové kopie z privátní úložiště, musíte taky zadat přihlašovací údaje úložiště Docker Hub jako (**uživatelské jméno přihlášení** a **heslo**) pro privátní úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-119">When your image is from a private repository, you also need to enter the Docker Hub credentials as (**Login username** and **Password**) for the private Docker Hub repository.</span></span>

    ![Obrázek privátní úložiště Docker Hub konfigurace][3]

3. <span data-ttu-id="5e1b6-121">Po konfiguraci kontejneru, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-121">After you have configured the container, click **Save**.</span></span>

## <a name="how-to-use-a-docker-image-from-a-private-image-registry"></a><span data-ttu-id="5e1b6-122">Jak použít bitovou kopii Docker z registru privátní bitové kopie</span><span class="sxs-lookup"><span data-stu-id="5e1b6-122">How to use a Docker image from a private image registry</span></span> ##
<span data-ttu-id="5e1b6-123">Použití vlastní image Docker z registru privátní bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="5e1b6-123">To use a custom Docker image from a private image registry:</span></span>

1. <span data-ttu-id="5e1b6-124">V [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-124">In the [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="5e1b6-125">Klikněte na tlačítko **privátní registru** jako **zdroj bitové kopie**.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-125">Click **Private registry** as the **Image source**.</span></span> <span data-ttu-id="5e1b6-126">Zadejte **bitové kopie a název značky volitelné**, **adresa URL serveru** pro privátní registru, spolu s přihlašovací údaje (**uživatelské jméno přihlášení** a **heslo**).</span><span class="sxs-lookup"><span data-stu-id="5e1b6-126">Enter the **Image and optional tag name**, **Server URL** for the private registry, along with the credentials (**Login username** and **Password**).</span></span> <span data-ttu-id="5e1b6-127">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-127">Click **Save**.</span></span>

    ![Obrázek Docker z registru privátní konfigurace][4]


## <a name="how-to-set-the-port-used-by-your-docker-image"></a><span data-ttu-id="5e1b6-129">Postupy: nastavení port je používán bitové kopie Docker</span><span class="sxs-lookup"><span data-stu-id="5e1b6-129">How to: set the port used by your Docker image</span></span> ##

<span data-ttu-id="5e1b6-130">Pokud používáte vlastní image Docker pro vaši webovou aplikaci, můžete použít `WEBSITES_PORT` proměnné prostředí v váš soubor Docker, který získá přidat do kontejneru vygenerovaný.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-130">When you use a custom Docker image for your web app, you can use the `WEBSITES_PORT` environment variable in your Dockerfile, which gets added to the generated container.</span></span> <span data-ttu-id="5e1b6-131">Podívejte se na následující příklad souboru docker pro poznámky Ruby aplikaci:</span><span class="sxs-lookup"><span data-stu-id="5e1b6-131">Consider the following example of a docker file for a Ruby application:</span></span>

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

<span data-ttu-id="5e1b6-132">Na poslední řádek příkazu se zobrazí, že proměnná prostředí WEBSITES_PORT předaný za běhu.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-132">On last line of the command, you can see that the WEBSITES_PORT environment variable is passed at runtime.</span></span> <span data-ttu-id="5e1b6-133">Mějte na paměti, že v příkazech záleží velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-133">Remember that casing matters in commands.</span></span>

<span data-ttu-id="5e1b6-134">Dřív používal platformou `PORT` aplikace nastavení, jsme plánování přestat používat použití této aplikace, nastavení a přesouvat pomocí `WEBSITES_PORT` výhradně.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-134">Previously the platform was using `PORT` app setting, we are planning to deprecate the use this app setting and move to using `WEBSITES_PORT` exclusively.</span></span>

<span data-ttu-id="5e1b6-135">Při použití stávající image Docker vytvořené někdo jiný, budete možná muset zadat jiný port než port 80 pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-135">When you use an existing Docker image built by someone else, you may need to specify a port other than port 80 for the application.</span></span> <span data-ttu-id="5e1b6-136">Pokud chcete konfigurovat port, přidání aplikace nastavení s názvem `WEBSITES_PORT` s hodnotou, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="5e1b6-136">To configure the port, add an application setting named `WEBSITES_PORT` with the value as shown below:</span></span>

![Konfigurace nastavení portu aplikace pro vlastní obrázek Docker][6]

## <a name="how-to-set-the-startup-time-for-your-docker-image"></a><span data-ttu-id="5e1b6-138">Postupy: nastavení času spuštění pro bitové kopie Docker</span><span class="sxs-lookup"><span data-stu-id="5e1b6-138">How to: set the startup time for your Docker image</span></span> ##

<span data-ttu-id="5e1b6-139">Ve výchozím nastavení Pokud vaše kontejneru nespustí před 230 sekund, restartuje platforma vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-139">By default, if your container does not start before 230 seconds, the platform will restart your container.</span></span> <span data-ttu-id="5e1b6-140">Pokud vaše vlastní image Docker spustí ve více než 230 sekund, můžete použít `WEBSITES_CONTAINER_START_TIME_LIMIT` aplikace nastavení, hodnota tohoto nastavení je v sekundách, to umožní zachovat platforma vašeho kontejneru spuštěné před jeho restartování.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-140">If your custom Docker image starts in more than 230 seconds, you can use the `WEBSITES_CONTAINER_START_TIME_LIMIT` app setting, the value for this setting is in seconds, this will allow the platform keep your container running before restarting it.</span></span> <span data-ttu-id="5e1b6-141">Výchozí hodnota je 230 sekund a maximální povolená hodnota je 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-141">The default value is 230 seconds, and the max allowed value is 600 seconds.</span></span>

## <a name="how-to-unmount-the-platform-provided-storage"></a><span data-ttu-id="5e1b6-142">Postupy: odpojení platforma poskytovaná úložiště</span><span class="sxs-lookup"><span data-stu-id="5e1b6-142">How to: unmount the platform provided storage</span></span> ##

<span data-ttu-id="5e1b6-143">Ve výchozím nastavení, bude platformou připojit sdílenou složku trvalé úložiště k `\home\` adresáře.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-143">By default, the platform will mount a persistent storage share to the `\home\` directory.</span></span> <span data-ttu-id="5e1b6-144">Pokud je image kontejneru nemusí trvalé sdílené složky, můžete zakázat připojení tohoto úložiště nastavením `WEBSITES_ENABLE_APP_SERVICE_STORAGE` nastavení aplikace nastavte na `false`.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-144">If your container image does not need a persistent share, you can disable mounting that storage by setting the `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app setting to `false`.</span></span> <span data-ttu-id="5e1b6-145">Budete mít stále přístup do tohoto úložiště z webu scm a všechny protokoly Docker (Pokud je povoleno) se zapíšou do protokolu soubory generované platformu.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-145">You will still have access to that storage from the scm site, and all Docker logs (if enabled) will be written to the log files generated by the platform.</span></span>

## <a name="how-to-switch-back-to-using-a-built-in-image"></a><span data-ttu-id="5e1b6-146">Postupy: Přejít zpět k používání předdefinovaných bitové kopie</span><span class="sxs-lookup"><span data-stu-id="5e1b6-146">How to: Switch back to using a built-in image</span></span> ##

<span data-ttu-id="5e1b6-147">Chcete-li přepnout pomocí vlastní image pomocí předdefinovaných bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="5e1b6-147">To switch from using a custom image to using a built-in image:</span></span>

1. <span data-ttu-id="5e1b6-148">V [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-148">In the [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **App Service**.</span></span>

2. <span data-ttu-id="5e1b6-149">Vyberte vaše **Runtime zásobníku** Pokud chcete použít integrované bitové kopie, pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-149">Select your **Runtime Stack** to use for the built-in image, then click **Save**.</span></span> 

![Konfigurace předdefinovaných Docker image][5]


## <a name="troubleshooting"></a><span data-ttu-id="5e1b6-151">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="5e1b6-151">Troubleshooting</span></span> ##

<span data-ttu-id="5e1b6-152">Pokud vaše aplikace se nepodaří spustit s vlastní Docker bitovou kopii, zkontrolujte, že že docker protokolů v adresáři LogFiles.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-152">When your application fails to start with your custom Docker image, check the Docker logs in the LogFiles directory.</span></span> <span data-ttu-id="5e1b6-153">Buď prostřednictvím webu SCM nebo FTP, můžete přístup k tomuto adresáři.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-153">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="5e1b6-154">Do protokolu `stdout` a `stderr` z kontejneru, je nutné povolit **kontejner Docker protokolování** pod **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-154">To log the `stdout` and `stderr` from your container, you need to enable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Povolení protokolování][8]

![Pokud chcete zobrazit protokoly Docker pomocí modulu Kudu][7]

<span data-ttu-id="5e1b6-157">Můžete získat přístup k webu SCM z **Rozšířené nástroje** v **nástroje pro vývoj** nabídky.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-157">You can access the SCM site from **Advanced Tools** in the **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e1b6-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e1b6-158">Next Steps</span></span> ##

<span data-ttu-id="5e1b6-159">Postupujte podle následujících odkazů můžete začít pracovat s webové aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="5e1b6-159">Follow the following links to get started with Web App on Linux.</span></span>   

* [<span data-ttu-id="5e1b6-160">Úvod do Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5e1b6-160">Introduction to Azure Web App on Linux</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="5e1b6-161">Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5e1b6-161">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](./app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="5e1b6-162">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="5e1b6-162">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<span data-ttu-id="5e1b6-163">Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="5e1b6-163">Post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>


<!--Image references-->
[1]: ./media/app-service-linux-using-custom-docker-image/new-configure-container.png
[2]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-public.png
[3]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-private.png
[4]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-privateregistry.png
[5]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-builtin.png
[6]: ./media/app-service-linux-using-custom-docker-image/setting-port.png
[7]: ./media/app-service-linux-using-custom-docker-image/kudu-docker-logs.png
[8]: ./media/app-service-linux-using-custom-docker-image/logging.png
