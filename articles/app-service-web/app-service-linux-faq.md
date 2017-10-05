---
title: "Webové aplikace Azure App Service v systému Linux – nejčastější dotazy | Microsoft Docs"
description: "Webové aplikace Azure App Service v systému Linux – nejčastější dotazy."
keywords: "služby Azure app service, webové aplikace, – nejčastější dotazy, linux, operačních systémů"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: 6122f28b35d143ec26a379ae9aa8aee9bdaaff9e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-app-service-web-app-on-linux-faq"></a><span data-ttu-id="c12fd-104">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="c12fd-104">Azure App Service Web App on Linux FAQ</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="c12fd-105">S vydáním webové aplikace v systému Linux pracujeme na přidání funkcí a vylepšení naše platforma provádění.</span><span class="sxs-lookup"><span data-stu-id="c12fd-105">With the release of Web App on Linux, we're working on adding features and making improvements to our platform.</span></span> <span data-ttu-id="c12fd-106">Tady jsou některé nejčastější dotazy (FAQ), které zákazníkům mít byla zprávu s požadavkem za posledních měsíců.</span><span class="sxs-lookup"><span data-stu-id="c12fd-106">Here are some frequently asked questions (FAQ) that our customers have been asking us over the last months.</span></span>
<span data-ttu-id="c12fd-107">Pokud máte dotazy, komentáře na článek a jsme budete nejdříve hovor.</span><span class="sxs-lookup"><span data-stu-id="c12fd-107">If you have a question, comment on the article and we'll answer it as soon as possible.</span></span>

## <a name="built-in-images"></a><span data-ttu-id="c12fd-108">Předdefinované bitové kopie</span><span class="sxs-lookup"><span data-stu-id="c12fd-108">Built-in images</span></span>

<span data-ttu-id="c12fd-109">**Otázka:** chcete rozvětvit předdefinované Docker kontejnerů, které poskytuje platformu.</span><span class="sxs-lookup"><span data-stu-id="c12fd-109">**Q:** I want to fork the built-in Docker containers that the platform provides.</span></span> <span data-ttu-id="c12fd-110">Kde najdu tyto soubory?</span><span class="sxs-lookup"><span data-stu-id="c12fd-110">Where can I find those files?</span></span>

<span data-ttu-id="c12fd-111">**Odpověď:** můžete najít všechny soubory Docker na [Githubu](https://github.com/azure-app-service).</span><span class="sxs-lookup"><span data-stu-id="c12fd-111">**A:** You can find all Docker files on [GitHub](https://github.com/azure-app-service).</span></span> <span data-ttu-id="c12fd-112">Můžete najít všechny kontejnery Docker na [úložiště Docker Hub](https://hub.docker.com/u/appsvc/).</span><span class="sxs-lookup"><span data-stu-id="c12fd-112">You can find all Docker containers on [Docker Hub](https://hub.docker.com/u/appsvc/).</span></span>

<span data-ttu-id="c12fd-113">**Otázka:** jaké jsou očekávané hodnoty pro spuštění souboru část, když je možné nakonfigurovat zásobník runtime?</span><span class="sxs-lookup"><span data-stu-id="c12fd-113">**Q:** What are the expected values for the Startup File section when I configure the runtime stack?</span></span>

<span data-ttu-id="c12fd-114">**Odpověď:** pro Node.Js, určíte PM2 konfiguračního souboru nebo souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="c12fd-114">**A:** For Node.Js, you specify the PM2 configuration file or your script file.</span></span> <span data-ttu-id="c12fd-115">.NET Core zadejte název vaší zkompilované knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="c12fd-115">For .NET Core, specify your compiled DLL name.</span></span> <span data-ttu-id="c12fd-116">Pro Ruby můžete zadat Ruby skript, který chcete inicializovat vaší aplikace pomocí.</span><span class="sxs-lookup"><span data-stu-id="c12fd-116">For Ruby, you can specify the Ruby script that you want to initialize your app with.</span></span>

## <a name="management"></a><span data-ttu-id="c12fd-117">Správa</span><span class="sxs-lookup"><span data-stu-id="c12fd-117">Management</span></span>

<span data-ttu-id="c12fd-118">**Otázka:** co se stane při stisknutí tlačítka restartování na portálu Azure?</span><span class="sxs-lookup"><span data-stu-id="c12fd-118">**Q:** What happens when I press the restart button in the Azure portal?</span></span>

<span data-ttu-id="c12fd-119">**Odpověď:** jde o ekvivalent Docker restartování.</span><span class="sxs-lookup"><span data-stu-id="c12fd-119">**A:** This is the equivalent of Docker restart.</span></span>

<span data-ttu-id="c12fd-120">**Otázka:** můžete použít Secure Shell (SSH) pro připojení k aplikaci kontejneru virtuální počítač (VM)?</span><span class="sxs-lookup"><span data-stu-id="c12fd-120">**Q:** Can I use Secure Shell (SSH) to connect to the app container virtual machine (VM)?</span></span>

<span data-ttu-id="c12fd-121">**Odpověď:** Ano, můžete to udělat přes lokalitu SCM, zkontrolujte v následujícím článku na další informace [podpora SSH pro webové aplikace v systému Linux](./app-service-linux-ssh-support.md)</span><span class="sxs-lookup"><span data-stu-id="c12fd-121">**A:** Yes, you can do that through the SCM site, check the following article for more information [SSH support for Web App on Linux](./app-service-linux-ssh-support.md)</span></span>

<span data-ttu-id="c12fd-122">**Otázka:** vytvořit roviny Linux App Service pomocí sady SDK nebo šablonu ARM, jak to můžete dosáhnout?</span><span class="sxs-lookup"><span data-stu-id="c12fd-122">**Q:** I want to create a Linux App Service plane through SDK or an ARM template, how can I achieve this?</span></span>

<span data-ttu-id="c12fd-123">**Odpověď:** je nutné nastavit `reserved` pole aplikace služby k `true`.</span><span class="sxs-lookup"><span data-stu-id="c12fd-123">**A:** You need to set the `reserved` field of the app service to `true`.</span></span>

## <a name="continuous-integrationdeployment"></a><span data-ttu-id="c12fd-124">Průběžnou integraci a nasazení</span><span class="sxs-lookup"><span data-stu-id="c12fd-124">Continuous integration/deployment</span></span>

<span data-ttu-id="c12fd-125">**Otázka:** webová aplikace dál používá image staré kontejner Docker po aktualizovali bitovou kopii na úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="c12fd-125">**Q:** My web app still uses an old Docker container image after I've updated the image on Docker Hub.</span></span> <span data-ttu-id="c12fd-126">Podporujete průběžnou integraci a nasazení vlastní kontejnerů?</span><span class="sxs-lookup"><span data-stu-id="c12fd-126">Do you support continuous integration/deployment of custom containers?</span></span>

<span data-ttu-id="c12fd-127">**Odpověď:** nastavit průběžnou integraci a nasazení pro Azure kontejneru registru nebo DockerHub Image kontrolou v následujícím článku [průběžné nasazování pomocí webové aplikace Azure v systému Linux](./app-service-linux-ci-cd.md).</span><span class="sxs-lookup"><span data-stu-id="c12fd-127">**A:** To set up continuous integration/deployment for Azure Container Registry or DockerHub images by check the following article [Continuous Deployment with Azure Web App on Linux](./app-service-linux-ci-cd.md).</span></span> <span data-ttu-id="c12fd-128">Pro privátní registrech můžete aktualizovat kontejneru zastavení a spuštění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c12fd-128">For private registries, you can refresh the container by stopping and then starting your web app.</span></span> <span data-ttu-id="c12fd-129">Nebo můžete změnit nebo přidat nastavení fiktivní aplikace můžete vynutit aktualizaci vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c12fd-129">Or you can change or add a dummy application setting to force a refresh of your container.</span></span>

<span data-ttu-id="c12fd-130">**Otázka:** podporují pracovní prostředí?</span><span class="sxs-lookup"><span data-stu-id="c12fd-130">**Q:** Do you support staging environments?</span></span>

<span data-ttu-id="c12fd-131">**Odpověď:** Ano.</span><span class="sxs-lookup"><span data-stu-id="c12fd-131">**A:** Yes.</span></span>

<span data-ttu-id="c12fd-132">**Otázka:** je možné používat **nasazení webu** nasazení webová aplikace?</span><span class="sxs-lookup"><span data-stu-id="c12fd-132">**Q:** Can I use **web deploy** to deploy my web app?</span></span>

<span data-ttu-id="c12fd-133">**Odpověď:** Ano, je nutné nastavit aplikaci názvem `WEBSITE_WEBDEPLOY_USE_SCM` k `false`.</span><span class="sxs-lookup"><span data-stu-id="c12fd-133">**A:** Yes, you need to set an app setting called `WEBSITE_WEBDEPLOY_USE_SCM` to `false`.</span></span>

## <a name="language-support"></a><span data-ttu-id="c12fd-134">Podpora jazyků</span><span class="sxs-lookup"><span data-stu-id="c12fd-134">Language support</span></span>

<span data-ttu-id="c12fd-135">**Otázka:** podporují nezkompilované aplikace .NET Core?</span><span class="sxs-lookup"><span data-stu-id="c12fd-135">**Q:** Do you support uncompiled .NET Core apps?</span></span>

<span data-ttu-id="c12fd-136">**Odpověď:** Ano.</span><span class="sxs-lookup"><span data-stu-id="c12fd-136">**A:** Yes.</span></span>

<span data-ttu-id="c12fd-137">**Otázka:** podporujete autora jako správce závislostí pro aplikace PHP?</span><span class="sxs-lookup"><span data-stu-id="c12fd-137">**Q:** Do you support Composer as a dependency manager for PHP apps?</span></span>

<span data-ttu-id="c12fd-138">**Odpověď:** Ano.</span><span class="sxs-lookup"><span data-stu-id="c12fd-138">**A:** Yes.</span></span> <span data-ttu-id="c12fd-139">Během nasazení Git by měl zjistit Kudu nasazujete aplikace PHP (díky přítomnost souboru composer.json) a aktivuje autora instalace pro vás.</span><span class="sxs-lookup"><span data-stu-id="c12fd-139">During a Git deployment, Kudu should detect that you are deploying a PHP application (thanks to the presence of a composer.json file) and will trigger a composer install for you.</span></span>

## <a name="custom-containers"></a><span data-ttu-id="c12fd-140">Vlastní kontejnery</span><span class="sxs-lookup"><span data-stu-id="c12fd-140">Custom containers</span></span>

<span data-ttu-id="c12fd-141">**Otázka:** používám vlastní vlastní kontejner.</span><span class="sxs-lookup"><span data-stu-id="c12fd-141">**Q:** I'm using my own custom container.</span></span> <span data-ttu-id="c12fd-142">Moje aplikace se nachází v `\home\` adresáře, ale I nemůže najít Moje soubory při procházení obsahu pomocí [SCM lokality](https://github.com/projectkudu/kudu) nebo klient FTP.</span><span class="sxs-lookup"><span data-stu-id="c12fd-142">My app resides in the `\home\` directory, but I can't find my files when I browse the content by using the [SCM site](https://github.com/projectkudu/kudu) or an FTP client.</span></span> <span data-ttu-id="c12fd-143">Kde jsou moje soubory?</span><span class="sxs-lookup"><span data-stu-id="c12fd-143">Where are my files?</span></span>

<span data-ttu-id="c12fd-144">**Odpověď:** nemůžeme připojit k serveru SMB pro sdílení `\home\` adresáře.</span><span class="sxs-lookup"><span data-stu-id="c12fd-144">**A:** We mount an SMB share to the `\home\` directory.</span></span> <span data-ttu-id="c12fd-145">Tím se přepíše veškerý obsah, který je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c12fd-145">This will override any content that's there.</span></span>

<span data-ttu-id="c12fd-146">**Otázka:** používám vlastní vlastní kontejner.</span><span class="sxs-lookup"><span data-stu-id="c12fd-146">**Q:** I'm using my own custom container.</span></span> <span data-ttu-id="c12fd-147">Nechci platformou připojit k serveru SMB pro sdílení `\home\`.</span><span class="sxs-lookup"><span data-stu-id="c12fd-147">I don't want the platform to mount an SMB share to the `\home\`.</span></span>

<span data-ttu-id="c12fd-148">**Odpověď:** můžete to udělat nastavením `WEBSITES_ENABLE_APP_SERVICE_STORAGE` nastavení aplikace nastavte na `false`.</span><span class="sxs-lookup"><span data-stu-id="c12fd-148">**A:** You can do that by setting the `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app setting to `false`.</span></span>

<span data-ttu-id="c12fd-149">**Otázka:** Mé vlastní kontejner trvá dlouhou dobu spuštění a platformou restartuje kontejner před dokončením spuštění.</span><span class="sxs-lookup"><span data-stu-id="c12fd-149">**Q:** My custom container takes a long time to start, and the platform restart the container before it finishes starting up.</span></span>

<span data-ttu-id="c12fd-150">**Odpověď:** můžete nakonfigurovat čas platformou bude čekat před restartováním vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c12fd-150">**A:** You can configure the time the platform will wait before restarting your container.</span></span> <span data-ttu-id="c12fd-151">To můžete provést nastavením `WEBSITES_CONTAINER_START_TIME_LIMIT` nastavení aplikace nastavte na požadovanou hodnotu v sekundách.</span><span class="sxs-lookup"><span data-stu-id="c12fd-151">This can be done by setting the `WEBSITES_CONTAINER_START_TIME_LIMIT` app setting to the desired value in seconds.</span></span> <span data-ttu-id="c12fd-152">Výchozí hodnota je 230 sekund a maximální je 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="c12fd-152">The default is 230 seconds, and the max is 600 seconds.</span></span>

<span data-ttu-id="c12fd-153">**Otázka:** co je formát adresy url serveru privátní registru?</span><span class="sxs-lookup"><span data-stu-id="c12fd-153">**Q:** What is the format for private registry server url?</span></span>

<span data-ttu-id="c12fd-154">**Odpověď:** budete muset zadat registru úplnou adresu url včetně `http://` nebo `https://`.</span><span class="sxs-lookup"><span data-stu-id="c12fd-154">**A:** You need to provide the full registry url including `http://` or `https://`.</span></span>

<span data-ttu-id="c12fd-155">**Otázka:** formát pro název bitové kopie v privátní registru možnosti?</span><span class="sxs-lookup"><span data-stu-id="c12fd-155">**Q:** What is the format for the image name in private registry option?</span></span>

<span data-ttu-id="c12fd-156">**Odpověď:** budete muset přidat název úplnou bitovou kopii, včetně adresu url privátní registru (např.</span><span class="sxs-lookup"><span data-stu-id="c12fd-156">**A:** You need to add the full image name including the private registry url (eg.</span></span> <span data-ttu-id="c12fd-157">myacr.azurecr.IO/DotNet:Latest)</span><span class="sxs-lookup"><span data-stu-id="c12fd-157">myacr.azurecr.io/dotnet:latest)</span></span>

<span data-ttu-id="c12fd-158">**Otázka:** chcete vystavit více než jeden port na mé vlastní kontejner bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="c12fd-158">**Q:** I want to expose more than one port on my custom container image.</span></span> <span data-ttu-id="c12fd-159">Je to možné?</span><span class="sxs-lookup"><span data-stu-id="c12fd-159">Is that possible?</span></span>

<span data-ttu-id="c12fd-160">**Odpověď:** aktuálně, která není podporována.</span><span class="sxs-lookup"><span data-stu-id="c12fd-160">**A:** Currently, that isn't supported.</span></span>

<span data-ttu-id="c12fd-161">**Otázka:** můžete zahrnout vlastní úložiště?</span><span class="sxs-lookup"><span data-stu-id="c12fd-161">**Q:** Can I bring my own storage?</span></span>

<span data-ttu-id="c12fd-162">**Odpověď:** aktuálně, která není podporována.</span><span class="sxs-lookup"><span data-stu-id="c12fd-162">**A:** Currently that isn't supported.</span></span>

<span data-ttu-id="c12fd-163">**Otázka:** není možné prohlížet procesy systému nebo spuštění souboru Mé vlastní kontejner z webu Správce služeb.</span><span class="sxs-lookup"><span data-stu-id="c12fd-163">**Q:** I can't browse my custom container's file system or running processes from the SCM site.</span></span> <span data-ttu-id="c12fd-164">Co to znamená?</span><span class="sxs-lookup"><span data-stu-id="c12fd-164">Why is that?</span></span>

<span data-ttu-id="c12fd-165">**Odpověď:** SCM lokality běží ve zvláštním kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c12fd-165">**A:** The SCM site runs in a separate container.</span></span> <span data-ttu-id="c12fd-166">Nelze zkontrolovat soubor systému nebo spuštění procesů kontejneru aplikace.</span><span class="sxs-lookup"><span data-stu-id="c12fd-166">You can't check the file system or running processes of the app container.</span></span>

<span data-ttu-id="c12fd-167">**Otázka:** Mé vlastní kontejner naslouchá na jiný port než port 80.</span><span class="sxs-lookup"><span data-stu-id="c12fd-167">**Q:** My custom container listens to a port other than port 80.</span></span> <span data-ttu-id="c12fd-168">Konfigurování aplikace my směrovat požadavky k tomuto portu</span><span class="sxs-lookup"><span data-stu-id="c12fd-168">How can I configure my app to route the requests to that port?</span></span>

<span data-ttu-id="c12fd-169">**Odpověď:** máme automatické zjišťování port, můžete také zadat aplikaci s názvem **WEBSITES_PORT**a jako hodnotu číslo portu očekávané.</span><span class="sxs-lookup"><span data-stu-id="c12fd-169">**A:** We have auto port detection, also you can specify an application setting called **WEBSITES_PORT**, and give it the value of the expected port number.</span></span> <span data-ttu-id="c12fd-170">Dřív používal platformou `PORT` aplikace nastavení, jsme plánování přestat používat použití této aplikace, nastavení a přesouvat pomocí `WEBSITES_PORT` výhradně.</span><span class="sxs-lookup"><span data-stu-id="c12fd-170">Previously the platform was using `PORT` app setting, we are planning to deprecate the use this app setting and move to using `WEBSITES_PORT` exclusively.</span></span>

<span data-ttu-id="c12fd-171">**Otázka:** je nutné implementovat HTTPS v mé vlastní kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c12fd-171">**Q:** Do I need to implement HTTPS in my custom container.</span></span>

<span data-ttu-id="c12fd-172">**Odpověď:** Ne, platformu zpracovává ukončení protokolu HTTPS na sdílené frontends.</span><span class="sxs-lookup"><span data-stu-id="c12fd-172">**A:** No, the platform handles HTTPS termination at the shared frontends.</span></span>

## <a name="pricing-and-sla"></a><span data-ttu-id="c12fd-173">Ceny a smlouva SLA</span><span class="sxs-lookup"><span data-stu-id="c12fd-173">Pricing and SLA</span></span>

<span data-ttu-id="c12fd-174">**Otázka:** novinky ceny při používání verzi public preview?</span><span class="sxs-lookup"><span data-stu-id="c12fd-174">**Q:** What's the pricing while you're using the public preview?</span></span>

<span data-ttu-id="c12fd-175">**Odpověď:** budou se vám účtovat poloviční počet hodin, které běží vaše aplikace s normální ceny služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c12fd-175">**A:** You are charged half the number of hours that your app runs, with the normal Azure App Service pricing.</span></span> <span data-ttu-id="c12fd-176">To znamená, že můžete získat tak slevu 50 procent na normální Azure App Service – ceny.</span><span class="sxs-lookup"><span data-stu-id="c12fd-176">This means that you get a 50 percent discount on normal Azure App Service pricing.</span></span>

## <a name="other"></a><span data-ttu-id="c12fd-177">Ostatní</span><span class="sxs-lookup"><span data-stu-id="c12fd-177">Other</span></span>

<span data-ttu-id="c12fd-178">**Otázka:** co jsou podporovanými znaky v názvech nastavení aplikace?</span><span class="sxs-lookup"><span data-stu-id="c12fd-178">**Q:** What are the supported characters in application settings names?</span></span>

<span data-ttu-id="c12fd-179">**Odpověď:** pro nastavení aplikace lze použít pouze A-Z,-z, 0 – 9 a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="c12fd-179">**A:** You can only use A-Z, a-z, 0-9, and the underscore for application settings.</span></span>

<span data-ttu-id="c12fd-180">**Otázka:** kde mohou požadovat nové funkce?</span><span class="sxs-lookup"><span data-stu-id="c12fd-180">**Q:** Where can I request new features?</span></span>

<span data-ttu-id="c12fd-181">**Odpověď:** můžete odeslat vaše nápad na [fóru pro zpětnou vazbu webové aplikace](https://aka.ms/webapps-uservoice).</span><span class="sxs-lookup"><span data-stu-id="c12fd-181">**A:** You can submit your idea at the [Web Apps feedback forum](https://aka.ms/webapps-uservoice).</span></span> <span data-ttu-id="c12fd-182">Přidejte "[Linux]" na název vaší představu.</span><span class="sxs-lookup"><span data-stu-id="c12fd-182">Add "[Linux]" to the title of your idea.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c12fd-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c12fd-183">Next steps</span></span>
* [<span data-ttu-id="c12fd-184">Co je Azure webové aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="c12fd-184">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="c12fd-185">Podpora SSH pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c12fd-185">SSH support for Azure Web App on Linux</span></span>](./app-service-linux-ssh-support.md)
* [<span data-ttu-id="c12fd-186">Nastavení přípravných prostředí v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c12fd-186">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="c12fd-187">Průběžné nasazování pomocí Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c12fd-187">Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)
