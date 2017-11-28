---
title: "aaaAzure webové aplikace App Service na nejčastější dotazy týkající se systému Linux | Microsoft Docs"
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
ms.openlocfilehash: c7798d9144d936eecdc0e191fc870b0ee0b220c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-web-app-on-linux-faq"></a><span data-ttu-id="7d44e-104">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="7d44e-104">Azure App Service Web App on Linux FAQ</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="7d44e-105">Verze hello webové aplikace v systému Linux pracujeme na přidání funkcí a vylepšení tooour platformy provádění.</span><span class="sxs-lookup"><span data-stu-id="7d44e-105">With hello release of Web App on Linux, we're working on adding features and making improvements tooour platform.</span></span> <span data-ttu-id="7d44e-106">Zde jsou uvedeny některé časté otázky (FAQ), které zákazníkům mít byla zprávu s požadavkem přes hello posledních měsíců.</span><span class="sxs-lookup"><span data-stu-id="7d44e-106">Here are some frequently asked questions (FAQ) that our customers have been asking us over hello last months.</span></span>
<span data-ttu-id="7d44e-107">Pokud máte dotazy, komentáře k článku hello a jsme budete nejdříve hovor.</span><span class="sxs-lookup"><span data-stu-id="7d44e-107">If you have a question, comment on hello article and we'll answer it as soon as possible.</span></span>

## <a name="built-in-images"></a><span data-ttu-id="7d44e-108">Předdefinované bitové kopie</span><span class="sxs-lookup"><span data-stu-id="7d44e-108">Built-in images</span></span>

<span data-ttu-id="7d44e-109">**Otázka:** chci poskytuje toofork hello předdefinované Docker kontejnery, které hello platformy.</span><span class="sxs-lookup"><span data-stu-id="7d44e-109">**Q:** I want toofork hello built-in Docker containers that hello platform provides.</span></span> <span data-ttu-id="7d44e-110">Kde najdu tyto soubory?</span><span class="sxs-lookup"><span data-stu-id="7d44e-110">Where can I find those files?</span></span>

<span data-ttu-id="7d44e-111">**Odpověď:** můžete najít všechny soubory Docker na [Githubu](https://github.com/azure-app-service).</span><span class="sxs-lookup"><span data-stu-id="7d44e-111">**A:** You can find all Docker files on [GitHub](https://github.com/azure-app-service).</span></span> <span data-ttu-id="7d44e-112">Můžete najít všechny kontejnery Docker na [úložiště Docker Hub](https://hub.docker.com/u/appsvc/).</span><span class="sxs-lookup"><span data-stu-id="7d44e-112">You can find all Docker containers on [Docker Hub](https://hub.docker.com/u/appsvc/).</span></span>

<span data-ttu-id="7d44e-113">**Otázka:** co jsou hello očekávaných hodnot pro hello oddílu spuštění souboru, když je možné nakonfigurovat hello runtime zásobníku?</span><span class="sxs-lookup"><span data-stu-id="7d44e-113">**Q:** What are hello expected values for hello Startup File section when I configure hello runtime stack?</span></span>

<span data-ttu-id="7d44e-114">**Odpověď:** pro Node.Js, určíte hello PM2 konfiguračního souboru nebo souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="7d44e-114">**A:** For Node.Js, you specify hello PM2 configuration file or your script file.</span></span> <span data-ttu-id="7d44e-115">.NET Core zadejte název vaší zkompilované knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="7d44e-115">For .NET Core, specify your compiled DLL name.</span></span> <span data-ttu-id="7d44e-116">Pro Ruby, můžete zadat hello Ruby skriptu, které chcete tooinitialize vaší aplikace pomocí.</span><span class="sxs-lookup"><span data-stu-id="7d44e-116">For Ruby, you can specify hello Ruby script that you want tooinitialize your app with.</span></span>

## <a name="management"></a><span data-ttu-id="7d44e-117">Správa</span><span class="sxs-lookup"><span data-stu-id="7d44e-117">Management</span></span>

<span data-ttu-id="7d44e-118">**Otázka:** co se stane při stisknutí tlačítka restartování hello v hello portál Azure?</span><span class="sxs-lookup"><span data-stu-id="7d44e-118">**Q:** What happens when I press hello restart button in hello Azure portal?</span></span>

<span data-ttu-id="7d44e-119">**Odpověď:** to je hello ekvivalentní Docker restartování.</span><span class="sxs-lookup"><span data-stu-id="7d44e-119">**A:** This is hello equivalent of Docker restart.</span></span>

<span data-ttu-id="7d44e-120">**Otázka:** můžete použít Secure Shell (SSH) tooconnect toohello aplikace kontejneru virtuální počítač (VM)?</span><span class="sxs-lookup"><span data-stu-id="7d44e-120">**Q:** Can I use Secure Shell (SSH) tooconnect toohello app container virtual machine (VM)?</span></span>

<span data-ttu-id="7d44e-121">**Odpověď:** Ano, můžete provést, prostřednictvím webu SCM hello, zkontrolujte hello následující článek informace [podpora SSH pro webové aplikace v systému Linux](./app-service-linux-ssh-support.md)</span><span class="sxs-lookup"><span data-stu-id="7d44e-121">**A:** Yes, you can do that through hello SCM site, check hello following article for more information [SSH support for Web App on Linux](./app-service-linux-ssh-support.md)</span></span>

<span data-ttu-id="7d44e-122">**Otázka:** chci toocreate roviny Linux App Service pomocí sady SDK nebo šablonu ARM, jak to můžete dosáhnout?</span><span class="sxs-lookup"><span data-stu-id="7d44e-122">**Q:** I want toocreate a Linux App Service plane through SDK or an ARM template, how can I achieve this?</span></span>

<span data-ttu-id="7d44e-123">**Odpověď:** potřebujete tooset hello `reserved` pole aplikace hello služby příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="7d44e-123">**A:** You need tooset hello `reserved` field of hello app service too`true`.</span></span>

## <a name="continuous-integrationdeployment"></a><span data-ttu-id="7d44e-124">Průběžnou integraci a nasazení</span><span class="sxs-lookup"><span data-stu-id="7d44e-124">Continuous integration/deployment</span></span>

<span data-ttu-id="7d44e-125">**Otázka:** webová aplikace dál používá image staré kontejner Docker po aktualizovali hello bitové kopie na úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="7d44e-125">**Q:** My web app still uses an old Docker container image after I've updated hello image on Docker Hub.</span></span> <span data-ttu-id="7d44e-126">Podporujete průběžnou integraci a nasazení vlastní kontejnerů?</span><span class="sxs-lookup"><span data-stu-id="7d44e-126">Do you support continuous integration/deployment of custom containers?</span></span>

<span data-ttu-id="7d44e-127">**Odpověď:** tooset až průběžné integraci a nasazení pro Azure kontejneru registru nebo DockerHub Image podle následujícího článku hello kontrola [průběžné nasazování pomocí webové aplikace Azure v systému Linux](./app-service-linux-ci-cd.md).</span><span class="sxs-lookup"><span data-stu-id="7d44e-127">**A:** tooset up continuous integration/deployment for Azure Container Registry or DockerHub images by check hello following article [Continuous Deployment with Azure Web App on Linux](./app-service-linux-ci-cd.md).</span></span> <span data-ttu-id="7d44e-128">Pro privátní registrech můžete aktualizovat hello kontejneru zastavení a spuštění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d44e-128">For private registries, you can refresh hello container by stopping and then starting your web app.</span></span> <span data-ttu-id="7d44e-129">Nebo můžete změnit nebo přidat fiktivní aplikace nastavení tooforce aktualizaci vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7d44e-129">Or you can change or add a dummy application setting tooforce a refresh of your container.</span></span>

<span data-ttu-id="7d44e-130">**Otázka:** podporují pracovní prostředí?</span><span class="sxs-lookup"><span data-stu-id="7d44e-130">**Q:** Do you support staging environments?</span></span>

<span data-ttu-id="7d44e-131">**Odpověď:** Ano.</span><span class="sxs-lookup"><span data-stu-id="7d44e-131">**A:** Yes.</span></span>

<span data-ttu-id="7d44e-132">**Otázka:** je možné používat **nasazení webu** toodeploy webová aplikace?</span><span class="sxs-lookup"><span data-stu-id="7d44e-132">**Q:** Can I use **web deploy** toodeploy my web app?</span></span>

<span data-ttu-id="7d44e-133">**Odpověď:** Ano, je nutné tooset aplikace názvem `WEBSITE_WEBDEPLOY_USE_SCM` příliš`false`.</span><span class="sxs-lookup"><span data-stu-id="7d44e-133">**A:** Yes, you need tooset an app setting called `WEBSITE_WEBDEPLOY_USE_SCM` too`false`.</span></span>

## <a name="language-support"></a><span data-ttu-id="7d44e-134">Podpora jazyků</span><span class="sxs-lookup"><span data-stu-id="7d44e-134">Language support</span></span>

<span data-ttu-id="7d44e-135">**Otázka:** podporují nezkompilované aplikace .NET Core?</span><span class="sxs-lookup"><span data-stu-id="7d44e-135">**Q:** Do you support uncompiled .NET Core apps?</span></span>

<span data-ttu-id="7d44e-136">**Odpověď:** Ano.</span><span class="sxs-lookup"><span data-stu-id="7d44e-136">**A:** Yes.</span></span>

<span data-ttu-id="7d44e-137">**Otázka:** podporujete autora jako správce závislostí pro aplikace PHP?</span><span class="sxs-lookup"><span data-stu-id="7d44e-137">**Q:** Do you support Composer as a dependency manager for PHP apps?</span></span>

<span data-ttu-id="7d44e-138">**Odpověď:** Ano.</span><span class="sxs-lookup"><span data-stu-id="7d44e-138">**A:** Yes.</span></span> <span data-ttu-id="7d44e-139">Během nasazení Git by měl zjistit Kudu nasazujete aplikace PHP (Děkujeme toohello přítomnost composer.json souboru) a aktivuje autora instalace pro vás.</span><span class="sxs-lookup"><span data-stu-id="7d44e-139">During a Git deployment, Kudu should detect that you are deploying a PHP application (thanks toohello presence of a composer.json file) and will trigger a composer install for you.</span></span>

## <a name="custom-containers"></a><span data-ttu-id="7d44e-140">Vlastní kontejnery</span><span class="sxs-lookup"><span data-stu-id="7d44e-140">Custom containers</span></span>

<span data-ttu-id="7d44e-141">**Otázka:** používám vlastní vlastní kontejner.</span><span class="sxs-lookup"><span data-stu-id="7d44e-141">**Q:** I'm using my own custom container.</span></span> <span data-ttu-id="7d44e-142">Moje aplikace se nachází v hello `\home\` adresáře, ale I nemůže najít Moje soubory při procházení obsahu hello pomocí hello [SCM lokality](https://github.com/projectkudu/kudu) nebo klient FTP.</span><span class="sxs-lookup"><span data-stu-id="7d44e-142">My app resides in hello `\home\` directory, but I can't find my files when I browse hello content by using hello [SCM site](https://github.com/projectkudu/kudu) or an FTP client.</span></span> <span data-ttu-id="7d44e-143">Kde jsou moje soubory?</span><span class="sxs-lookup"><span data-stu-id="7d44e-143">Where are my files?</span></span>

<span data-ttu-id="7d44e-144">**Odpověď:** nemůžeme připojit toohello sdílené složky SMB `\home\` adresáře.</span><span class="sxs-lookup"><span data-stu-id="7d44e-144">**A:** We mount an SMB share toohello `\home\` directory.</span></span> <span data-ttu-id="7d44e-145">Tím se přepíše veškerý obsah, který je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7d44e-145">This will override any content that's there.</span></span>

<span data-ttu-id="7d44e-146">**Otázka:** používám vlastní vlastní kontejner.</span><span class="sxs-lookup"><span data-stu-id="7d44e-146">**Q:** I'm using my own custom container.</span></span> <span data-ttu-id="7d44e-147">Nechci hello platformy toomount toohello sdílené složky SMB `\home\`.</span><span class="sxs-lookup"><span data-stu-id="7d44e-147">I don't want hello platform toomount an SMB share toohello `\home\`.</span></span>

<span data-ttu-id="7d44e-148">**Odpověď:** můžete to udělat pomocí nastavení hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` aplikace nastavení příliš`false`.</span><span class="sxs-lookup"><span data-stu-id="7d44e-148">**A:** You can do that by setting hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app setting too`false`.</span></span>

<span data-ttu-id="7d44e-149">**Otázka:** Mé vlastní kontejner trvá dlouho toostart a hello platformy restartování hello kontejneru dříve, než se dokončí spuštění.</span><span class="sxs-lookup"><span data-stu-id="7d44e-149">**Q:** My custom container takes a long time toostart, and hello platform restart hello container before it finishes starting up.</span></span>

<span data-ttu-id="7d44e-150">**Odpověď:** můžete nakonfigurovat čas hello hello platformy bude čekat před restartováním vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7d44e-150">**A:** You can configure hello time hello platform will wait before restarting your container.</span></span> <span data-ttu-id="7d44e-151">To můžete provést nastavení hello `WEBSITES_CONTAINER_START_TIME_LIMIT` hodnota požadovaného toohello nastavení aplikace v sekundách.</span><span class="sxs-lookup"><span data-stu-id="7d44e-151">This can be done by setting hello `WEBSITES_CONTAINER_START_TIME_LIMIT` app setting toohello desired value in seconds.</span></span> <span data-ttu-id="7d44e-152">Výchozí hodnota Hello je 230 sekund a maximální hello je 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="7d44e-152">hello default is 230 seconds, and hello max is 600 seconds.</span></span>

<span data-ttu-id="7d44e-153">**Otázka:** co je hello formát adresy url serveru privátní registru?</span><span class="sxs-lookup"><span data-stu-id="7d44e-153">**Q:** What is hello format for private registry server url?</span></span>

<span data-ttu-id="7d44e-154">**Odpověď:** potřebujete tooprovide hello registru úplnou adresu url včetně `http://` nebo `https://`.</span><span class="sxs-lookup"><span data-stu-id="7d44e-154">**A:** You need tooprovide hello full registry url including `http://` or `https://`.</span></span>

<span data-ttu-id="7d44e-155">**Otázka:** co je hello formát pro název bitové kopie hello v privátní registru možnosti?</span><span class="sxs-lookup"><span data-stu-id="7d44e-155">**Q:** What is hello format for hello image name in private registry option?</span></span>

<span data-ttu-id="7d44e-156">**Odpověď:** potřebujete tooadd hello úplnou bitovou kopii název včetně hello privátní registru url (např.</span><span class="sxs-lookup"><span data-stu-id="7d44e-156">**A:** You need tooadd hello full image name including hello private registry url (eg.</span></span> <span data-ttu-id="7d44e-157">myacr.azurecr.IO/DotNet:Latest)</span><span class="sxs-lookup"><span data-stu-id="7d44e-157">myacr.azurecr.io/dotnet:latest)</span></span>

<span data-ttu-id="7d44e-158">**Otázka:** chci více než jeden port pro tooexpose na mé vlastní kontejner bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="7d44e-158">**Q:** I want tooexpose more than one port on my custom container image.</span></span> <span data-ttu-id="7d44e-159">Je to možné?</span><span class="sxs-lookup"><span data-stu-id="7d44e-159">Is that possible?</span></span>

<span data-ttu-id="7d44e-160">**Odpověď:** aktuálně, která není podporována.</span><span class="sxs-lookup"><span data-stu-id="7d44e-160">**A:** Currently, that isn't supported.</span></span>

<span data-ttu-id="7d44e-161">**Otázka:** můžete zahrnout vlastní úložiště?</span><span class="sxs-lookup"><span data-stu-id="7d44e-161">**Q:** Can I bring my own storage?</span></span>

<span data-ttu-id="7d44e-162">**Odpověď:** aktuálně, která není podporována.</span><span class="sxs-lookup"><span data-stu-id="7d44e-162">**A:** Currently that isn't supported.</span></span>

<span data-ttu-id="7d44e-163">**Otázka:** není možné prohlížet procesy systému nebo spuštění souboru Moje vlastní kontejner z lokality SCM hello.</span><span class="sxs-lookup"><span data-stu-id="7d44e-163">**Q:** I can't browse my custom container's file system or running processes from hello SCM site.</span></span> <span data-ttu-id="7d44e-164">Co to znamená?</span><span class="sxs-lookup"><span data-stu-id="7d44e-164">Why is that?</span></span>

<span data-ttu-id="7d44e-165">**Odpověď:** hello SCM lokality běží ve zvláštním kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7d44e-165">**A:** hello SCM site runs in a separate container.</span></span> <span data-ttu-id="7d44e-166">Nelze zkontrolovat hello systému souborů nebo spuštěných procesů kontejneru aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="7d44e-166">You can't check hello file system or running processes of hello app container.</span></span>

<span data-ttu-id="7d44e-167">**Otázka:** Mé vlastní kontejner naslouchá tooa port než 80.</span><span class="sxs-lookup"><span data-stu-id="7d44e-167">**Q:** My custom container listens tooa port other than port 80.</span></span> <span data-ttu-id="7d44e-168">Jak lze nastavit Moje aplikace tooroute hello požadavky toothat port?</span><span class="sxs-lookup"><span data-stu-id="7d44e-168">How can I configure my app tooroute hello requests toothat port?</span></span>

<span data-ttu-id="7d44e-169">**Odpověď:** máme automatické zjišťování port, můžete také zadat aplikaci s názvem **WEBSITES_PORT**a dejte mu hello hodnotu hello očekáváno číslo portu.</span><span class="sxs-lookup"><span data-stu-id="7d44e-169">**A:** We have auto port detection, also you can specify an application setting called **WEBSITES_PORT**, and give it hello value of hello expected port number.</span></span> <span data-ttu-id="7d44e-170">Dřív byla hello platformě pomocí `PORT` aplikace nastavení, jsme plánování toodeprecate hello použití této aplikace, nastavení a přesunout toousing `WEBSITES_PORT` výhradně.</span><span class="sxs-lookup"><span data-stu-id="7d44e-170">Previously hello platform was using `PORT` app setting, we are planning toodeprecate hello use this app setting and move toousing `WEBSITES_PORT` exclusively.</span></span>

<span data-ttu-id="7d44e-171">**Otázka:** potřebuji tooimplement HTTPS v mé vlastní kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7d44e-171">**Q:** Do I need tooimplement HTTPS in my custom container.</span></span>

<span data-ttu-id="7d44e-172">**Odpověď:** Ne, hello platformy zpracovává ukončení protokolu HTTPS v frontends hello sdílet.</span><span class="sxs-lookup"><span data-stu-id="7d44e-172">**A:** No, hello platform handles HTTPS termination at hello shared frontends.</span></span>

## <a name="pricing-and-sla"></a><span data-ttu-id="7d44e-173">Ceny a smlouva SLA</span><span class="sxs-lookup"><span data-stu-id="7d44e-173">Pricing and SLA</span></span>

<span data-ttu-id="7d44e-174">**Otázka:** co je hello ceny při používání hello verzi public preview?</span><span class="sxs-lookup"><span data-stu-id="7d44e-174">**Q:** What's hello pricing while you're using hello public preview?</span></span>

<span data-ttu-id="7d44e-175">**Odpověď:** budou se vám účtovat polovinu hello počet hodin, které běží vaše aplikace s hello normální Azure App Service – ceny.</span><span class="sxs-lookup"><span data-stu-id="7d44e-175">**A:** You are charged half hello number of hours that your app runs, with hello normal Azure App Service pricing.</span></span> <span data-ttu-id="7d44e-176">To znamená, že můžete získat tak slevu 50 procent na normální Azure App Service – ceny.</span><span class="sxs-lookup"><span data-stu-id="7d44e-176">This means that you get a 50 percent discount on normal Azure App Service pricing.</span></span>

## <a name="other"></a><span data-ttu-id="7d44e-177">Ostatní</span><span class="sxs-lookup"><span data-stu-id="7d44e-177">Other</span></span>

<span data-ttu-id="7d44e-178">**Otázka:** co jsou podporovány hello písmena v názvech nastavení aplikace?</span><span class="sxs-lookup"><span data-stu-id="7d44e-178">**Q:** What are hello supported characters in application settings names?</span></span>

<span data-ttu-id="7d44e-179">**Odpověď:** můžete použít A-Z,-z, 0-9 a hello podtržítka pro nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d44e-179">**A:** You can only use A-Z, a-z, 0-9, and hello underscore for application settings.</span></span>

<span data-ttu-id="7d44e-180">**Otázka:** kde mohou požadovat nové funkce?</span><span class="sxs-lookup"><span data-stu-id="7d44e-180">**Q:** Where can I request new features?</span></span>

<span data-ttu-id="7d44e-181">**Odpověď:** můžete odeslat vaše nápad na hello [fóru pro zpětnou vazbu webové aplikace](https://aka.ms/webapps-uservoice).</span><span class="sxs-lookup"><span data-stu-id="7d44e-181">**A:** You can submit your idea at hello [Web Apps feedback forum](https://aka.ms/webapps-uservoice).</span></span> <span data-ttu-id="7d44e-182">Přidejte název "[Linux]" toohello vaše představu.</span><span class="sxs-lookup"><span data-stu-id="7d44e-182">Add "[Linux]" toohello title of your idea.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d44e-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d44e-183">Next steps</span></span>
* [<span data-ttu-id="7d44e-184">Co je Azure webové aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="7d44e-184">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="7d44e-185">Podpora SSH pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="7d44e-185">SSH support for Azure Web App on Linux</span></span>](./app-service-linux-ssh-support.md)
* [<span data-ttu-id="7d44e-186">Nastavení přípravných prostředí v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7d44e-186">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="7d44e-187">Průběžné nasazování pomocí Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="7d44e-187">Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)
