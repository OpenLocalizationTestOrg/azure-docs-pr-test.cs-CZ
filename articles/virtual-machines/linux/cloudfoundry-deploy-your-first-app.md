---
title: "aaaDeploy první aplikaci tooCloud Foundry v Microsoft Azure | Microsoft Docs"
description: "Nasazení aplikace tooCloud Foundry v Azure"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 8fa04a58-56ad-4e6c-bef4-d02c80d4b60f
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: seanmck
ms.openlocfilehash: 878da38f6eabe32a339f02aa0ead811d6e5af9a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-first-app-toocloud-foundry-on-microsoft-azure"></a><span data-ttu-id="6b5e2-103">Nasazení první aplikaci tooCloud Foundry v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="6b5e2-103">Deploy your first app tooCloud Foundry on Microsoft Azure</span></span>

<span data-ttu-id="6b5e2-104">[Cloud Foundry](http://cloudfoundry.org) je platforma oblíbených open-source aplikace k dispozici na Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-104">[Cloud Foundry](http://cloudfoundry.org) is a popular open-source application platform available on Microsoft Azure.</span></span> <span data-ttu-id="6b5e2-105">V tomto článku ukážeme, jak toodeploy a spravovat aplikaci v cloudu Foundry v prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-105">In this article, we show how toodeploy and manage an application on Cloud Foundry in an Azure environment.</span></span>

## <a name="create-a-cloud-foundry-environment"></a><span data-ttu-id="6b5e2-106">Vytvořte prostředí cloudu Foundry</span><span class="sxs-lookup"><span data-stu-id="6b5e2-106">Create a Cloud Foundry environment</span></span>

<span data-ttu-id="6b5e2-107">Existuje několik možností pro vytvoření Foundry cloudové prostředí v Azure:</span><span class="sxs-lookup"><span data-stu-id="6b5e2-107">There are several options for creating a Cloud Foundry environment on Azure:</span></span>

- <span data-ttu-id="6b5e2-108">Použití hello [nabídka hrají cloudu Foundry] [ pcf-azuremarketplace] v Azure Marketplace toocreate hello standardní prostředí, které obsahuje PCF Ops Manager a hello Azure Service Broker.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-108">Use hello [Pivotal Cloud Foundry offer][pcf-azuremarketplace] in hello Azure Marketplace toocreate a standard environment that includes PCF Ops Manager and hello Azure Service Broker.</span></span> <span data-ttu-id="6b5e2-109">Můžete najít [úplné pokyny] [ pcf-azuremarketplace-pivotaldocs] pro nasazení hello marketplace nabízí v hello hrají dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-109">You can find [complete instructions][pcf-azuremarketplace-pivotaldocs] for deploying hello marketplace offer in hello Pivotal documentation.</span></span>
- <span data-ttu-id="6b5e2-110">Vytvořit vlastní prostředí pomocí [ručního nasazení hrají cloudu Foundry][pcf-custom].</span><span class="sxs-lookup"><span data-stu-id="6b5e2-110">Create a customized environment by [deploying Pivotal Cloud Foundry manually][pcf-custom].</span></span>
- <span data-ttu-id="6b5e2-111">[Nasazení balíčků cloudu Foundry open-source hello přímo] [ oss-cf-bosh] nastavením [BOSH](http://bosh.io) ředitel, virtuální počítač, který koordinuje hello nasazení hello Foundry cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-111">[Deploy hello open-source Cloud Foundry packages directly][oss-cf-bosh] by setting up a [BOSH](http://bosh.io) director, a VM that coordinates hello deployment of hello Cloud Foundry environment.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="6b5e2-112">Pokud nasazujete PCF z hello Azure Marketplace, poznamenejte si hello SYSTEMDOMAINURL a přihlašovací údaje správce hello požadované tooaccess hello hrají Správce aplikací, které jsou popsané v příručce pro nasazení webu marketplace hello.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-112">If you are deploying PCF from hello Azure Marketplace, make a note of hello SYSTEMDOMAINURL and hello admin credentials required tooaccess hello Pivotal Apps Manager, both of which are described in hello marketplace deployment guide.</span></span> <span data-ttu-id="6b5e2-113">Že jsou potřebné toocomplete v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-113">They are needed toocomplete this tutorial.</span></span> <span data-ttu-id="6b5e2-114">Pro nasazení webu marketplace je hello SYSTEMDOMAINURL v https://system hello formuláře. *ip adresu*. cf.pcfazure.com.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-114">For marketplace deployments, hello SYSTEMDOMAINURL is in hello form https://system.*ip-address*.cf.pcfazure.com.</span></span>

## <a name="connect-toohello-cloud-controller"></a><span data-ttu-id="6b5e2-115">Připojit toohello Kontroleru cloudu</span><span class="sxs-lookup"><span data-stu-id="6b5e2-115">Connect toohello Cloud Controller</span></span>

<span data-ttu-id="6b5e2-116">Hello Kontroleru cloudu je hello primární vstupní bod tooa Foundry cloudové prostředí pro nasazení a Správa aplikací.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-116">hello Cloud Controller is hello primary entry point tooa Cloud Foundry environment for deploying and managing applications.</span></span> <span data-ttu-id="6b5e2-117">základní Hello cloudu řadiče rozhraní API (CCAPI) je rozhraní REST API, ale je přístupný prostřednictvím různých nástrojů.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-117">hello core Cloud Controller API (CCAPI) is a REST API, but it is accessible through various tools.</span></span> <span data-ttu-id="6b5e2-118">V takovém případě budeme pracovat s ním prostřednictvím hello [cloudu Foundry rozhraní příkazového řádku][cf-cli].</span><span class="sxs-lookup"><span data-stu-id="6b5e2-118">In this case, we interact with it through hello [Cloud Foundry CLI][cf-cli].</span></span> <span data-ttu-id="6b5e2-119">Hello rozhraní příkazového řádku můžete nainstalovat na systému Linux, systému MacOS nebo Windows, ale pokud si přejete není tooinstall ho vůbec, je k dispozici předinstalován v hello [prostředí cloudu Azure][cloudshell-docs].</span><span class="sxs-lookup"><span data-stu-id="6b5e2-119">You can install hello CLI on Linux, MacOS, or Windows, but if you'd prefer not tooinstall it at all, it is available pre-installed in hello [Azure Cloud Shell][cloudshell-docs].</span></span>

<span data-ttu-id="6b5e2-120">toolog, předřadit `api` toohello SYSTEMDOMAINURL, který jste získali z marketplace nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-120">toolog in, prepend `api` toohello SYSTEMDOMAINURL that you obtained from hello marketplace deployment.</span></span> <span data-ttu-id="6b5e2-121">Vzhledem k tomu, že nasazení výchozí hello používá certifikát podepsaný svým držitelem, musí rovněž zahrnovat hello `skip-ssl-validation` přepínače.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-121">Since hello default deployment uses a self-signed certificate, you should also include hello `skip-ssl-validation` switch.</span></span>

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

<span data-ttu-id="6b5e2-122">Jste výzvami toolog v toohello Kontroleru cloudu.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-122">You are prompted toolog in toohello Cloud Controller.</span></span> <span data-ttu-id="6b5e2-123">Pomocí pověření účtu správce hello, které jste získali z kroků nasazení hello marketplace.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-123">Use hello admin account credentials that you acquired from hello marketplace deployment steps.</span></span>

<span data-ttu-id="6b5e2-124">Poskytuje cloudu Foundry *orgs* a *prostory* jako týmy hello tooisolate obory názvů a prostředí v rámci sdílené nasazení.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-124">Cloud Foundry provides *orgs* and *spaces* as namespaces tooisolate hello teams and environments within a shared deployment.</span></span> <span data-ttu-id="6b5e2-125">Hello PCF marketplace nasazení obsahuje výchozí hello *systému* organizace a sadu prostorů vytvořit toocontain hello základní součásti, jako je služba hello automatické škálování a zprostředkovatele služby Azure hello.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-125">hello PCF marketplace deployment includes hello default *system* org and a set of spaces created toocontain hello base components, like hello autoscaling service and hello Azure service broker.</span></span> <span data-ttu-id="6b5e2-126">Nyní, vyberte hello *systému* místa.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-126">For now, choose hello *system* space.</span></span>


## <a name="create-an-org-and-space"></a><span data-ttu-id="6b5e2-127">Vytvoření organizace a místa</span><span class="sxs-lookup"><span data-stu-id="6b5e2-127">Create an org and space</span></span>

<span data-ttu-id="6b5e2-128">Pokud zadáte `cf apps`, najdete v části sadu aplikací systému, které jsou nasazené v prostoru hello systému v rámci systému org. hello</span><span class="sxs-lookup"><span data-stu-id="6b5e2-128">If you type `cf apps`, you see a set of system applications that have been deployed in hello system space within hello system org.</span></span> 

<span data-ttu-id="6b5e2-129">Byste měli mít hello *systému* org vyhrazena pro aplikace, systému, takže vytvořte organizace a místo toohouse naše ukázková aplikace.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-129">You should keep hello *system* org reserved for system applications, so create an org and space toohouse our sample application.</span></span>

```bash
cf create-org myorg
cf create-space dev -o myorg
```

<span data-ttu-id="6b5e2-130">Použijte hello cíl příkazu tooswitch toohello nové organizace a místa:</span><span class="sxs-lookup"><span data-stu-id="6b5e2-130">Use hello target command tooswitch toohello new org and space:</span></span>

```bash
cf target -o testorg -s dev
```

<span data-ttu-id="6b5e2-131">Teď když nasadíte aplikaci, je vytvořeno automaticky v nové organizace hello a místo.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-131">Now, when you deploy an application, it is automatically created in hello new org and space.</span></span> <span data-ttu-id="6b5e2-132">tooconfirm, která aktuálně nejsou k dispozici žádné aplikace. v nové organizace hello/prostor, zadejte `cf apps` znovu.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-132">tooconfirm that there are currently no apps in hello new org/space, type `cf apps` again.</span></span>

> [!NOTE] 
> <span data-ttu-id="6b5e2-133">Další informace o orgs a prostory a jak mohou být použity pro řízení přístupu na základě role (RBAC) najdete v tématu hello [dokumentace cloudu Foundry][cf-orgs-spaces-docs].</span><span class="sxs-lookup"><span data-stu-id="6b5e2-133">For more information about orgs and spaces and how they can be used for role-based access control (RBAC), see hello [Cloud Foundry documentation][cf-orgs-spaces-docs].</span></span>

## <a name="deploy-an-application"></a><span data-ttu-id="6b5e2-134">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="6b5e2-134">Deploy an application</span></span>

<span data-ttu-id="6b5e2-135">Můžeme použít ukázkové aplikace cloudu Foundry názvem Hello pružiny cloudu, což je napsanou v jazyce Java a podle hello [pružiny Framework](http://spring.io) a [pružiny spouštěcí](http://projects.spring.io/spring-boot/).</span><span class="sxs-lookup"><span data-stu-id="6b5e2-135">Let's use a sample Cloud Foundry application called Hello Spring Cloud, which is written in Java and based on hello [Spring Framework](http://spring.io) and [Spring Boot](http://projects.spring.io/spring-boot/).</span></span>

### <a name="clone-hello-hello-spring-cloud-repository"></a><span data-ttu-id="6b5e2-136">Klonování hello Hello pružiny cloudové úložiště</span><span class="sxs-lookup"><span data-stu-id="6b5e2-136">Clone hello Hello Spring Cloud repository</span></span>

<span data-ttu-id="6b5e2-137">Hello Hello pružiny cloudu ukázkové aplikace je k dispozici na Githubu.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-137">hello Hello Spring Cloud sample application is available on GitHub.</span></span> <span data-ttu-id="6b5e2-138">Naklonujte tooyour prostředí a změňte do nového adresáře hello:</span><span class="sxs-lookup"><span data-stu-id="6b5e2-138">Clone it tooyour environment and change into hello new directory:</span></span>

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-hello-application"></a><span data-ttu-id="6b5e2-139">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6b5e2-139">Build hello application</span></span>

<span data-ttu-id="6b5e2-140">Sestavení hello aplikace pomocí [Apache Maven](http://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="6b5e2-140">Build hello app using [Apache Maven](http://maven.apache.org).</span></span>

```bash
mvn clean package
```

### <a name="deploy-hello-application-with-cf-push"></a><span data-ttu-id="6b5e2-141">Nasazení aplikace hello s nabízené CR</span><span class="sxs-lookup"><span data-stu-id="6b5e2-141">Deploy hello application with cf push</span></span>

<span data-ttu-id="6b5e2-142">Většina aplikací tooCloud Foundry můžete nasadit pomocí hello `push` příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b5e2-142">You can deploy most applications tooCloud Foundry using hello `push` command:</span></span>

```bash
cf push
```

<span data-ttu-id="6b5e2-143">Pokud jste *nabízené* aplikace, cloudu Foundry zjistí hello typu aplikace (v tomto případě aplikace v jazyce Java) a identifikuje jeho závislosti (v tomto případě rámci pružiny hello).</span><span class="sxs-lookup"><span data-stu-id="6b5e2-143">When you *push* an application, Cloud Foundry detects hello type of application (in this case, a Java app) and identifies its dependencies (in this case, hello Spring framework).</span></span> <span data-ttu-id="6b5e2-144">Pak balíčky všechno požadované toorun kódu do kontejneru bitovou kopii samostatné říká *droplet*.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-144">It then packages everything required toorun your code into a standalone container image, known as a *droplet*.</span></span> <span data-ttu-id="6b5e2-145">Nakonec cloudu Foundry plány hello aplikaci na některém hello dostupných počítačů ve vašem prostředí a vytvoří adresa URL, kde můžete dosáhnout, která je dostupná v hello výstup hello příkazu.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-145">Finally, Cloud Foundry schedules hello application on one of hello available machines in your environment and creates a URL where you can reach it, which is available in hello output of hello command.</span></span>

![Výstup z příkazu nabízené CR][cf-push-output]

<span data-ttu-id="6b5e2-147">toosee hello hello. pružiny cloudových aplikací, otevřete hello zadat adresu URL v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="6b5e2-147">toosee hello hello-spring-cloud application, open hello provided URL in your browser:</span></span>

![Výchozí nastavení uživatelského rozhraní pro cloudové pružiny Hello][hello-spring-cloud-basic]

> [!NOTE] 
> <span data-ttu-id="6b5e2-149">toolearn více informací o co se stane, že během `cf push`, najdete v části [jak jsou připraveny aplikace] [ cf-push-docs] v hello cloudu Foundry dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-149">toolearn more about what happens during `cf push`, see [How Applications Are Staged][cf-push-docs] in hello Cloud Foundry documentation.</span></span>

## <a name="view-application-logs"></a><span data-ttu-id="6b5e2-150">Zobrazit protokoly aplikací</span><span class="sxs-lookup"><span data-stu-id="6b5e2-150">View application logs</span></span>

<span data-ttu-id="6b5e2-151">Jeho název můžete použít protokoly tooview hello cloudu Foundry rozhraní příkazového řádku pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="6b5e2-151">You can use hello Cloud Foundry CLI tooview logs for an application by its name:</span></span>

```bash
cf logs hello-spring-cloud
```

<span data-ttu-id="6b5e2-152">Ve výchozím nastavení, protokoly hello používá příkaz *tail*, který zobrazuje nové protokoly, jako jsou zapsané.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-152">By default, hello logs command uses *tail*, which shows new logs as they are written.</span></span> <span data-ttu-id="6b5e2-153">toosee nové protokoly se zobrazí, aktualizujte hello hello pružiny cloud aplikaci v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-153">toosee new logs appear, refresh hello hello-spring-cloud app in hello browser.</span></span>

<span data-ttu-id="6b5e2-154">tooview protokoly, které již byla zapsána, přidejte hello `recent` přepínače:</span><span class="sxs-lookup"><span data-stu-id="6b5e2-154">tooview logs that have already been written, add hello `recent` switch:</span></span>

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-hello-application"></a><span data-ttu-id="6b5e2-155">Škálování aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6b5e2-155">Scale hello application</span></span>

<span data-ttu-id="6b5e2-156">Ve výchozím nastavení `cf push` pouze vytvoří jednu instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-156">By default, `cf push` only creates a single instance of your application.</span></span> <span data-ttu-id="6b5e2-157">tooensure vysokou dostupnost a povolit škálování pro vyšší propustnost, chcete obecně toorun více než jednu instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-157">tooensure high availability and enable scale out for higher throughput, you generally want toorun more than one instance of your applications.</span></span> <span data-ttu-id="6b5e2-158">Můžete snadno škálovat již nasazené aplikace pomocí hello `scale` příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b5e2-158">You can easily scale out already deployed applications using hello `scale` command:</span></span>

```bash
cf scale -i 2 hello-spring-cloud
```

<span data-ttu-id="6b5e2-159">Spuštěné hello `cf app` příkaz na hello aplikace zobrazí, že cloudové Foundry vytváří jiná instance aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-159">Running hello `cf app` command on hello application shows that Cloud Foundry is creating another instance of hello application.</span></span> <span data-ttu-id="6b5e2-160">Po zahájení aplikace hello, Foundry cloudu se automaticky spustí provoz tooit Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6b5e2-160">Once hello application has started, Cloud Foundry automatically starts load balancing traffic tooit.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6b5e2-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b5e2-161">Next steps</span></span>

- <span data-ttu-id="6b5e2-162">[Čtení hello dokumentace cloudu Foundry][cloudfoundry-docs]</span><span class="sxs-lookup"><span data-stu-id="6b5e2-162">[Read hello Cloud Foundry documentation][cloudfoundry-docs]</span></span>
- <span data-ttu-id="6b5e2-163">[Nastavení modulu plug-in hello Visual Studio Team Services pro Cloud Foundry][vsts-plugin]</span><span class="sxs-lookup"><span data-stu-id="6b5e2-163">[Set up hello Visual Studio Team Services plugin for Cloud Foundry][vsts-plugin]</span></span>
- <span data-ttu-id="6b5e2-164">[Konfigurace hello trysek Analýza protokolů Microsoft pro Cloud Foundry][loganalytics-nozzle]</span><span class="sxs-lookup"><span data-stu-id="6b5e2-164">[Configure hello Microsoft Log Analytics Nozzle for Cloud Foundry][loganalytics-nozzle]</span></span>

<!-- LINKS -->

[pcf-azuremarketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry
[pcf-custom]: https://docs.pivotal.io/pivotalcf/1-10/customizing/azure.html
[oss-cf-bosh]: https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs
[pcf-azuremarketplace-pivotaldocs]: https://docs.pivotal.io/pivotalcf/customizing/pcf_azure.html
[cf-cli]: https://github.com/cloudfoundry/cli
[cloudshell-docs]: https://docs.microsoft.com/azure/cloud-shell/overview
[cf-orgs-spaces-docs]: https://docs.cloudfoundry.org/concepts/roles.html
[spring-boot]: https://projects.spring.io/spring-boot/
[spring-framework]: http://spring.io
[cf-push-docs]: https://docs.cloudfoundry.org/concepts/how-applications-are-staged.html
[cloudfoundry-docs]: https://docs.cloudfoundry.org
[vsts-plugin]: https://github.com/Microsoft/vsts-cloudfoundry
[loganalytics-nozzle]: https://github.com/Azure/oms-log-analytics-firehose-nozzle

<!-- IMAGES -->
[cf-push-output]: ./media/cloudfoundry-deploy-your-first-app/cf-push-output.png
[hello-spring-cloud-basic]: ./media/cloudfoundry-deploy-your-first-app/hello-spring-cloud-basic.png