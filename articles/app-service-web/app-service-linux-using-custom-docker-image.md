---
title: "aaaHow toouse vlastní image Docker pro webové aplikace Azure v systému Linux | Microsoft Docs"
description: "Jak toouse vlastní Docker obrázků pro webové aplikace Azure v systému Linux."
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
ms.openlocfilehash: 8853095d0e1067cfea4297bbd23b622fe4a0d4db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a><span data-ttu-id="32f90-104">Použití vlastní image Docker pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="32f90-104">Using a custom Docker image for Azure Web App on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="32f90-105">Služba App Service poskytuje zásobníky předem definované aplikací v systému Linux s podporou pro konkrétní verze, jako je například PHP 7.0 a Node.js 4.5.</span><span class="sxs-lookup"><span data-stu-id="32f90-105">App Service provides pre-defined application stacks on Linux with support for specific versions, such as PHP 7.0 and Node.js 4.5.</span></span> <span data-ttu-id="32f90-106">Aplikační služby v systému Linux používá Docker kontejnery toohost, tyto předem integrované zásobníky aplikace.</span><span class="sxs-lookup"><span data-stu-id="32f90-106">App Service on Linux uses Docker containers toohost these pre-built application stacks.</span></span> <span data-ttu-id="32f90-107">Můžete také použít vlastní toodeploy image Docker vaší webové aplikace tooan zásobník aplikací v Azure, již není definovaná.</span><span class="sxs-lookup"><span data-stu-id="32f90-107">You can also use a custom Docker image toodeploy your web app tooan application stack that is not already defined in Azure.</span></span> <span data-ttu-id="32f90-108">Vlastní imagí Dockeru může být hostitelem buď veřejných nebo privátních Docker úložiště.</span><span class="sxs-lookup"><span data-stu-id="32f90-108">Custom Docker images can be hosted on either a public or private Docker repository.</span></span>


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a><span data-ttu-id="32f90-109">Postupy: nastavení vlastní image Docker pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="32f90-109">How to: set a custom Docker image for a web app</span></span>
<span data-ttu-id="32f90-110">Můžete nastavit hello vlastní image Docker pro obě nové a stávající aplikace a weby.</span><span class="sxs-lookup"><span data-stu-id="32f90-110">You can set hello custom Docker image for both new and existing webs apps.</span></span> <span data-ttu-id="32f90-111">Při vytváření webové aplikace v systému Linux v hello [portál Azure](https://portal.azure.com/#create/Microsoft.AppSvcLinux), klikněte na tlačítko **kontejneru konfigurace** tooset vlastní image Docker:</span><span class="sxs-lookup"><span data-stu-id="32f90-111">When you create a web app on Linux in hello [Azure portal](https://portal.azure.com/#create/Microsoft.AppSvcLinux), click **Configure container** tooset a custom Docker image:</span></span>

![Vlastní Image Docker pro novou webovou aplikaci v systému Linux][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a><span data-ttu-id="32f90-113">Postupy: použití vlastní image Docker z úložiště Docker Hub</span><span class="sxs-lookup"><span data-stu-id="32f90-113">How to: use a custom Docker image from Docker Hub</span></span> ##
<span data-ttu-id="32f90-114">toouse vlastní image Docker z úložiště Docker Hub:</span><span class="sxs-lookup"><span data-stu-id="32f90-114">toouse a custom Docker image from Docker Hub:</span></span>

1. <span data-ttu-id="32f90-115">V hello [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="32f90-115">In hello [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="32f90-116">Vyberte **úložiště Docker Hub** jako hello **zdroj bitové kopie**, pak klikněte buď na **veřejné** nebo **privátní** a typ hello **bitové kopie a název značky volitelné**, jako například `node:4.5`.</span><span class="sxs-lookup"><span data-stu-id="32f90-116">Select **Docker Hub** as hello **Image source**, then click either **Public** or **Private** and type hello **Image and optional tag name**, such as `node:4.5`.</span></span> <span data-ttu-id="32f90-117">Hello **spuštění příkazu** je sada automaticky na základě co je definována v souboru bitové kopie Docker hello, ale můžete nastavit vlastní příkazy.</span><span class="sxs-lookup"><span data-stu-id="32f90-117">hello **Startup command** is set automatically based on what is defined in hello Docker image file, but you can set your own commands.</span></span>  

    ![Obrázek veřejného úložiště Docker Hub konfigurace][2]

    <span data-ttu-id="32f90-119">Když bitové kopie z privátní úložiště, musíte taky přihlašovací údaje úložiště Docker Hub hello tooenter jako (**uživatelské jméno přihlášení** a **heslo**) pro privátní úložiště Docker Hub hello.</span><span class="sxs-lookup"><span data-stu-id="32f90-119">When your image is from a private repository, you also need tooenter hello Docker Hub credentials as (**Login username** and **Password**) for hello private Docker Hub repository.</span></span>

    ![Obrázek privátní úložiště Docker Hub konfigurace][3]

3. <span data-ttu-id="32f90-121">Po nakonfigurování hello kontejner, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="32f90-121">After you have configured hello container, click **Save**.</span></span>

## <a name="how-toouse-a-docker-image-from-a-private-image-registry"></a><span data-ttu-id="32f90-122">Jak toouse Docker obrázek z registru privátní bitové kopie</span><span class="sxs-lookup"><span data-stu-id="32f90-122">How toouse a Docker image from a private image registry</span></span> ##
<span data-ttu-id="32f90-123">toouse vlastní image Docker z registru privátní bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="32f90-123">toouse a custom Docker image from a private image registry:</span></span>

1. <span data-ttu-id="32f90-124">V hello [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="32f90-124">In hello [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="32f90-125">Klikněte na tlačítko **privátní registru** jako hello **zdroj bitové kopie**.</span><span class="sxs-lookup"><span data-stu-id="32f90-125">Click **Private registry** as hello **Image source**.</span></span> <span data-ttu-id="32f90-126">Zadejte hello **bitové kopie a název značky volitelné**, **adresa URL serveru** pro privátní registru hello spolu s přihlašovací údaje hello (**uživatelské jméno přihlášení** a **heslo** ).</span><span class="sxs-lookup"><span data-stu-id="32f90-126">Enter hello **Image and optional tag name**, **Server URL** for hello private registry, along with hello credentials (**Login username** and **Password**).</span></span> <span data-ttu-id="32f90-127">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="32f90-127">Click **Save**.</span></span>

    ![Obrázek Docker z registru privátní konfigurace][4]


## <a name="how-to-set-hello-port-used-by-your-docker-image"></a><span data-ttu-id="32f90-129">Postupy: nastavení portu hello používá Docker image</span><span class="sxs-lookup"><span data-stu-id="32f90-129">How to: set hello port used by your Docker image</span></span> ##

<span data-ttu-id="32f90-130">Pokud používáte vlastní image Docker pro vaši webovou aplikaci, můžete použít hello `WEBSITES_PORT` proměnné prostředí v váš soubor Docker, který získá přidat toohello generované kontejneru.</span><span class="sxs-lookup"><span data-stu-id="32f90-130">When you use a custom Docker image for your web app, you can use hello `WEBSITES_PORT` environment variable in your Dockerfile, which gets added toohello generated container.</span></span> <span data-ttu-id="32f90-131">Vezměte v úvahu následující ukázka soubor docker pro poznámky Ruby aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="32f90-131">Consider hello following example of a docker file for a Ruby application:</span></span>

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

<span data-ttu-id="32f90-132">Na poslední řádek hello příkazu uvidíte, že tuto proměnnou prostředí WEBSITES_PORT hello předaný za běhu.</span><span class="sxs-lookup"><span data-stu-id="32f90-132">On last line of hello command, you can see that hello WEBSITES_PORT environment variable is passed at runtime.</span></span> <span data-ttu-id="32f90-133">Mějte na paměti, že v příkazech záleží velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="32f90-133">Remember that casing matters in commands.</span></span>

<span data-ttu-id="32f90-134">Dřív byla hello platformě pomocí `PORT` aplikace nastavení, jsme plánování toodeprecate hello použití této aplikace, nastavení a přesunout toousing `WEBSITES_PORT` výhradně.</span><span class="sxs-lookup"><span data-stu-id="32f90-134">Previously hello platform was using `PORT` app setting, we are planning toodeprecate hello use this app setting and move toousing `WEBSITES_PORT` exclusively.</span></span>

<span data-ttu-id="32f90-135">Pokud používáte stávající image Docker vytvořené někdo jiný, musíte toospecify jiný port než port 80 pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="32f90-135">When you use an existing Docker image built by someone else, you may need toospecify a port other than port 80 for hello application.</span></span> <span data-ttu-id="32f90-136">tooconfigure hello port, přidání aplikace nastavení s názvem `WEBSITES_PORT` hodnotou hello, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="32f90-136">tooconfigure hello port, add an application setting named `WEBSITES_PORT` with hello value as shown below:</span></span>

![Konfigurace nastavení portu aplikace pro vlastní obrázek Docker][6]

## <a name="how-to-set-hello-startup-time-for-your-docker-image"></a><span data-ttu-id="32f90-138">Postupy: nastavení času spuštění hello pro bitové kopie Docker</span><span class="sxs-lookup"><span data-stu-id="32f90-138">How to: set hello startup time for your Docker image</span></span> ##

<span data-ttu-id="32f90-139">Ve výchozím nastavení Pokud vaše kontejneru nespustí před 230 sekund, restartuje hello platforma vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="32f90-139">By default, if your container does not start before 230 seconds, hello platform will restart your container.</span></span> <span data-ttu-id="32f90-140">Pokud vaše vlastní image Docker spustí ve více než 230 sekund, můžete použít hello `WEBSITES_CONTAINER_START_TIME_LIMIT` nastavení aplikace hello hodnota tohoto nastavení je v sekundách, to vám umožní zachovat platformy hello vašeho kontejneru spuštěné před jej restartovat.</span><span class="sxs-lookup"><span data-stu-id="32f90-140">If your custom Docker image starts in more than 230 seconds, you can use hello `WEBSITES_CONTAINER_START_TIME_LIMIT` app setting, hello value for this setting is in seconds, this will allow hello platform keep your container running before restarting it.</span></span> <span data-ttu-id="32f90-141">Hello výchozí hodnota je 230 sekund a hello maximální povolená hodnota je 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="32f90-141">hello default value is 230 seconds, and hello max allowed value is 600 seconds.</span></span>

## <a name="how-to-unmount-hello-platform-provided-storage"></a><span data-ttu-id="32f90-142">Postupy: odpojení hello platforma poskytovaná úložiště</span><span class="sxs-lookup"><span data-stu-id="32f90-142">How to: unmount hello platform provided storage</span></span> ##

<span data-ttu-id="32f90-143">Ve výchozím nastavení, bude hello platformy připojit trvalého úložiště sdílené složky toohello `\home\` adresáře.</span><span class="sxs-lookup"><span data-stu-id="32f90-143">By default, hello platform will mount a persistent storage share toohello `\home\` directory.</span></span> <span data-ttu-id="32f90-144">Pokud je image kontejneru nemusí trvalé sdílené složky, můžete zakázat připojení tohoto úložiště pomocí nastavení hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` aplikace nastavení příliš`false`.</span><span class="sxs-lookup"><span data-stu-id="32f90-144">If your container image does not need a persistent share, you can disable mounting that storage by setting hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app setting too`false`.</span></span> <span data-ttu-id="32f90-145">Z lokality scm hello budete mít stále přístup k úložišti toothat a všechny protokoly Docker (Pokud je povoleno) se zapíšou toohello souborů protokolu vygenerovaných hello platformy.</span><span class="sxs-lookup"><span data-stu-id="32f90-145">You will still have access toothat storage from hello scm site, and all Docker logs (if enabled) will be written toohello log files generated by hello platform.</span></span>

## <a name="how-to-switch-back-toousing-a-built-in-image"></a><span data-ttu-id="32f90-146">Postupy: přepínač zpět toousing integrované bitové kopie</span><span class="sxs-lookup"><span data-stu-id="32f90-146">How to: Switch back toousing a built-in image</span></span> ##

<span data-ttu-id="32f90-147">tooswitch pomocí vlastní image toousing integrované bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="32f90-147">tooswitch from using a custom image toousing a built-in image:</span></span>

1. <span data-ttu-id="32f90-148">V hello [portál Azure](https://portal.azure.com), vyhledejte vaší webové aplikace v systému Linux, pak v **nastavení** klikněte na tlačítko **služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="32f90-148">In hello [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **App Service**.</span></span>

2. <span data-ttu-id="32f90-149">Vyberte vaše **Runtime zásobníku** toouse hello integrované bitové kopie, pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="32f90-149">Select your **Runtime Stack** toouse for hello built-in image, then click **Save**.</span></span> 

![Konfigurace předdefinovaných Docker image][5]


## <a name="troubleshooting"></a><span data-ttu-id="32f90-151">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="32f90-151">Troubleshooting</span></span> ##

<span data-ttu-id="32f90-152">Pokud vaše aplikace nezdaří toostart s vlastní bitovou kopii Docker, zkontrolujte hello Docker protokolů v adresáři LogFiles hello.</span><span class="sxs-lookup"><span data-stu-id="32f90-152">When your application fails toostart with your custom Docker image, check hello Docker logs in hello LogFiles directory.</span></span> <span data-ttu-id="32f90-153">Buď prostřednictvím webu SCM nebo FTP, můžete přístup k tomuto adresáři.</span><span class="sxs-lookup"><span data-stu-id="32f90-153">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="32f90-154">toolog hello `stdout` a `stderr` z kontejneru, je nutné tooenable **kontejner Docker protokolování** pod **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="32f90-154">toolog hello `stdout` and `stderr` from your container, you need tooenable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Povolení protokolování][8]

![Pomocí modulu Kudu tooview Docker protokolů][7]

<span data-ttu-id="32f90-157">Dostanete hello SCM lokality z **Rozšířené nástroje** v hello **nástroje pro vývoj** nabídky.</span><span class="sxs-lookup"><span data-stu-id="32f90-157">You can access hello SCM site from **Advanced Tools** in hello **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32f90-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32f90-158">Next Steps</span></span> ##

<span data-ttu-id="32f90-159">Postupujte podle hello následující odkazy tooget začít s webovou aplikaci v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="32f90-159">Follow hello following links tooget started with Web App on Linux.</span></span>   

* [<span data-ttu-id="32f90-160">Úvod tooAzure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="32f90-160">Introduction tooAzure Web App on Linux</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="32f90-161">Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="32f90-161">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](./app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="32f90-162">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="32f90-162">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<span data-ttu-id="32f90-163">Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="32f90-163">Post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>


<!--Image references-->
[1]: ./media/app-service-linux-using-custom-docker-image/new-configure-container.png
[2]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-public.png
[3]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-private.png
[4]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-privateregistry.png
[5]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-builtin.png
[6]: ./media/app-service-linux-using-custom-docker-image/setting-port.png
[7]: ./media/app-service-linux-using-custom-docker-image/kudu-docker-logs.png
[8]: ./media/app-service-linux-using-custom-docker-image/logging.png
