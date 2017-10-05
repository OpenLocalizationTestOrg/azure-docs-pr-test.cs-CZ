---
title: "Nasazení vaší první aplikace do cloudu Foundry v Microsoft Azure | Microsoft Docs"
description: "Nasazení aplikace do cloudu Foundry v Azure"
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
ms.openlocfilehash: b617127fc0a3f8dcae293e356ea669edcfa5deff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-your-first-app-to-cloud-foundry-on-microsoft-azure"></a><span data-ttu-id="621e1-103">Nasazení vaší první aplikace do cloudu Foundry v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="621e1-103">Deploy your first app to Cloud Foundry on Microsoft Azure</span></span>

<span data-ttu-id="621e1-104">[Cloud Foundry](http://cloudfoundry.org) je platforma oblíbených open-source aplikace k dispozici na Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="621e1-104">[Cloud Foundry](http://cloudfoundry.org) is a popular open-source application platform available on Microsoft Azure.</span></span> <span data-ttu-id="621e1-105">V tomto článku jsme ukazují, jak nasadit a spravovat aplikaci v cloudu Foundry v prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="621e1-105">In this article, we show how to deploy and manage an application on Cloud Foundry in an Azure environment.</span></span>

## <a name="create-a-cloud-foundry-environment"></a><span data-ttu-id="621e1-106">Vytvořte prostředí cloudu Foundry</span><span class="sxs-lookup"><span data-stu-id="621e1-106">Create a Cloud Foundry environment</span></span>

<span data-ttu-id="621e1-107">Existuje několik možností pro vytvoření Foundry cloudové prostředí v Azure:</span><span class="sxs-lookup"><span data-stu-id="621e1-107">There are several options for creating a Cloud Foundry environment on Azure:</span></span>

- <span data-ttu-id="621e1-108">Použití [nabídka hrají cloudu Foundry] [ pcf-azuremarketplace] v Azure Marketplace vytvořit standardní prostředí, které obsahuje PCF Ops Manager a službu Azure Service Broker.</span><span class="sxs-lookup"><span data-stu-id="621e1-108">Use the [Pivotal Cloud Foundry offer][pcf-azuremarketplace] in the Azure Marketplace to create a standard environment that includes PCF Ops Manager and the Azure Service Broker.</span></span> <span data-ttu-id="621e1-109">Můžete najít [úplné pokyny] [ pcf-azuremarketplace-pivotaldocs] pro nasazení webu marketplace nabízí v hrají dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="621e1-109">You can find [complete instructions][pcf-azuremarketplace-pivotaldocs] for deploying the marketplace offer in the Pivotal documentation.</span></span>
- <span data-ttu-id="621e1-110">Vytvořit vlastní prostředí pomocí [ručního nasazení hrají cloudu Foundry][pcf-custom].</span><span class="sxs-lookup"><span data-stu-id="621e1-110">Create a customized environment by [deploying Pivotal Cloud Foundry manually][pcf-custom].</span></span>
- <span data-ttu-id="621e1-111">[Nasazení balíčků cloudu Foundry open-source přímo] [ oss-cf-bosh] nastavením [BOSH](http://bosh.io) ředitel, virtuální počítač, který koordinuje nasazení Foundry cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="621e1-111">[Deploy the open-source Cloud Foundry packages directly][oss-cf-bosh] by setting up a [BOSH](http://bosh.io) director, a VM that coordinates the deployment of the Cloud Foundry environment.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="621e1-112">Pokud nasazujete PCF z Azure Marketplace, poznamenejte si SYSTEMDOMAINURL a přihlašovací údaje správce potřebné pro přístup správce hrají aplikace, které jsou popsané v Průvodci nasazením aplikace marketplace.</span><span class="sxs-lookup"><span data-stu-id="621e1-112">If you are deploying PCF from the Azure Marketplace, make a note of the SYSTEMDOMAINURL and the admin credentials required to access the Pivotal Apps Manager, both of which are described in the marketplace deployment guide.</span></span> <span data-ttu-id="621e1-113">Že jsou nutné k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="621e1-113">They are needed to complete this tutorial.</span></span> <span data-ttu-id="621e1-114">Pro nasazení webu marketplace je SYSTEMDOMAINURL v https://system formuláře. *ip adresu*. cf.pcfazure.com.</span><span class="sxs-lookup"><span data-stu-id="621e1-114">For marketplace deployments, the SYSTEMDOMAINURL is in the form https://system.*ip-address*.cf.pcfazure.com.</span></span>

## <a name="connect-to-the-cloud-controller"></a><span data-ttu-id="621e1-115">Připojení k řadiči cloudu</span><span class="sxs-lookup"><span data-stu-id="621e1-115">Connect to the Cloud Controller</span></span>

<span data-ttu-id="621e1-116">Kontroleru cloudu je primárním vstupním bodem do cloudu Foundry prostředí pro nasazení a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="621e1-116">The Cloud Controller is the primary entry point to a Cloud Foundry environment for deploying and managing applications.</span></span> <span data-ttu-id="621e1-117">Základní rozhraní API řadiče v cloudu (CCAPI) je rozhraní REST API, ale je přístupný prostřednictvím různých nástrojů.</span><span class="sxs-lookup"><span data-stu-id="621e1-117">The core Cloud Controller API (CCAPI) is a REST API, but it is accessible through various tools.</span></span> <span data-ttu-id="621e1-118">V takovém případě budeme pracovat s ním prostřednictvím [cloudu Foundry rozhraní příkazového řádku][cf-cli].</span><span class="sxs-lookup"><span data-stu-id="621e1-118">In this case, we interact with it through the [Cloud Foundry CLI][cf-cli].</span></span> <span data-ttu-id="621e1-119">Rozhraní příkazového řádku můžete nainstalovat na systému Linux, systému MacOS nebo Windows, ale pokud chcete raději nechcete instalovat ho vůbec, je k dispozici předinstalován v [prostředí cloudu Azure][cloudshell-docs].</span><span class="sxs-lookup"><span data-stu-id="621e1-119">You can install the CLI on Linux, MacOS, or Windows, but if you'd prefer not to install it at all, it is available pre-installed in the [Azure Cloud Shell][cloudshell-docs].</span></span>

<span data-ttu-id="621e1-120">K přihlášení, předřadit `api` k SYSTEMDOMAINURL, který jste získali z marketplace nasazení.</span><span class="sxs-lookup"><span data-stu-id="621e1-120">To log in, prepend `api` to the SYSTEMDOMAINURL that you obtained from the marketplace deployment.</span></span> <span data-ttu-id="621e1-121">Vzhledem k tomu, že výchozí nasazení používá certifikát podepsaný svým držitelem, musí rovněž zahrnovat `skip-ssl-validation` přepínače.</span><span class="sxs-lookup"><span data-stu-id="621e1-121">Since the default deployment uses a self-signed certificate, you should also include the `skip-ssl-validation` switch.</span></span>

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

<span data-ttu-id="621e1-122">Zobrazí se výzva k přihlášení do Kontroleru cloudu.</span><span class="sxs-lookup"><span data-stu-id="621e1-122">You are prompted to log in to the Cloud Controller.</span></span> <span data-ttu-id="621e1-123">Použijte přihlašovací údaje účtu správce, které jste získali z kroků nasazení marketplace.</span><span class="sxs-lookup"><span data-stu-id="621e1-123">Use the admin account credentials that you acquired from the marketplace deployment steps.</span></span>

<span data-ttu-id="621e1-124">Poskytuje cloudu Foundry *orgs* a *prostory* jako obory názvů izolovat týmy a prostředí v rámci sdílené nasazení.</span><span class="sxs-lookup"><span data-stu-id="621e1-124">Cloud Foundry provides *orgs* and *spaces* as namespaces to isolate the teams and environments within a shared deployment.</span></span> <span data-ttu-id="621e1-125">Nasazení webu marketplace PCF zahrnuje výchozí *systému* organizace a sadu prostorů vytvořit tak, aby obsahovala základní součásti, jako třeba službu automatické škálování a zprostředkovatele služby Azure.</span><span class="sxs-lookup"><span data-stu-id="621e1-125">The PCF marketplace deployment includes the default *system* org and a set of spaces created to contain the base components, like the autoscaling service and the Azure service broker.</span></span> <span data-ttu-id="621e1-126">Nyní, vyberte *systému* místa.</span><span class="sxs-lookup"><span data-stu-id="621e1-126">For now, choose the *system* space.</span></span>


## <a name="create-an-org-and-space"></a><span data-ttu-id="621e1-127">Vytvoření organizace a místa</span><span class="sxs-lookup"><span data-stu-id="621e1-127">Create an org and space</span></span>

<span data-ttu-id="621e1-128">Pokud zadáte `cf apps`, najdete v části sadu systému aplikací, které jsou nasazené v prostoru systému v rámci org. systému</span><span class="sxs-lookup"><span data-stu-id="621e1-128">If you type `cf apps`, you see a set of system applications that have been deployed in the system space within the system org.</span></span> 

<span data-ttu-id="621e1-129">Byste měli mít *systému* org vyhrazena pro aplikace, systému, tak vytvoření organizace a místa pro uložení naše ukázková aplikace.</span><span class="sxs-lookup"><span data-stu-id="621e1-129">You should keep the *system* org reserved for system applications, so create an org and space to house our sample application.</span></span>

```bash
cf create-org myorg
cf create-space dev -o myorg
```

<span data-ttu-id="621e1-130">Použijte příkaz cíl přepnout do nové organizace a místa:</span><span class="sxs-lookup"><span data-stu-id="621e1-130">Use the target command to switch to the new org and space:</span></span>

```bash
cf target -o testorg -s dev
```

<span data-ttu-id="621e1-131">Teď když nasadíte aplikaci, je vytvořeno automaticky v nové organizace a místa.</span><span class="sxs-lookup"><span data-stu-id="621e1-131">Now, when you deploy an application, it is automatically created in the new org and space.</span></span> <span data-ttu-id="621e1-132">Chcete-li potvrdit, že aktuálně neexistují žádné aplikace v nové organizace/prostor, zadejte `cf apps` znovu.</span><span class="sxs-lookup"><span data-stu-id="621e1-132">To confirm that there are currently no apps in the new org/space, type `cf apps` again.</span></span>

> [!NOTE] 
> <span data-ttu-id="621e1-133">Další informace o orgs a prostory a jak mohou být použity pro řízení přístupu na základě role (RBAC) najdete v tématu [dokumentace cloudu Foundry][cf-orgs-spaces-docs].</span><span class="sxs-lookup"><span data-stu-id="621e1-133">For more information about orgs and spaces and how they can be used for role-based access control (RBAC), see the [Cloud Foundry documentation][cf-orgs-spaces-docs].</span></span>

## <a name="deploy-an-application"></a><span data-ttu-id="621e1-134">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="621e1-134">Deploy an application</span></span>

<span data-ttu-id="621e1-135">Můžeme použít ukázkové aplikace cloudu Foundry názvem Hello pružiny cloudu, což je napsanou v jazyce Java a na základě [pružiny Framework](http://spring.io) a [pružiny spouštěcí](http://projects.spring.io/spring-boot/).</span><span class="sxs-lookup"><span data-stu-id="621e1-135">Let's use a sample Cloud Foundry application called Hello Spring Cloud, which is written in Java and based on the [Spring Framework](http://spring.io) and [Spring Boot](http://projects.spring.io/spring-boot/).</span></span>

### <a name="clone-the-hello-spring-cloud-repository"></a><span data-ttu-id="621e1-136">Klonovat úložiště v cloudu pružiny Hello</span><span class="sxs-lookup"><span data-stu-id="621e1-136">Clone the Hello Spring Cloud repository</span></span>

<span data-ttu-id="621e1-137">Ukázková aplikace Hello pružiny cloudu je k dispozici na Githubu.</span><span class="sxs-lookup"><span data-stu-id="621e1-137">The Hello Spring Cloud sample application is available on GitHub.</span></span> <span data-ttu-id="621e1-138">Klonování pro vaše prostředí a změňte do nového adresáře:</span><span class="sxs-lookup"><span data-stu-id="621e1-138">Clone it to your environment and change into the new directory:</span></span>

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-the-application"></a><span data-ttu-id="621e1-139">Sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="621e1-139">Build the application</span></span>

<span data-ttu-id="621e1-140">Sestavení aplikace pomocí [Apache Maven](http://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="621e1-140">Build the app using [Apache Maven](http://maven.apache.org).</span></span>

```bash
mvn clean package
```

### <a name="deploy-the-application-with-cf-push"></a><span data-ttu-id="621e1-141">Nasazení aplikace pomocí nabízených CR</span><span class="sxs-lookup"><span data-stu-id="621e1-141">Deploy the application with cf push</span></span>

<span data-ttu-id="621e1-142">Většina aplikací pomocí Foundry cloudu můžete nasadit `push` příkaz:</span><span class="sxs-lookup"><span data-stu-id="621e1-142">You can deploy most applications to Cloud Foundry using the `push` command:</span></span>

```bash
cf push
```

<span data-ttu-id="621e1-143">Když jste *nabízená* aplikace cloudu Foundry zjistí typ aplikace (v tomto případě aplikace v jazyce Java) a identifikuje jeho závislosti (v tomto případě pružiny framework).</span><span class="sxs-lookup"><span data-stu-id="621e1-143">When you *push* an application, Cloud Foundry detects the type of application (in this case, a Java app) and identifies its dependencies (in this case, the Spring framework).</span></span> <span data-ttu-id="621e1-144">Potom balíčky vše potřebné ke spuštění kódu do kontejneru bitovou kopii samostatné říká *droplet*.</span><span class="sxs-lookup"><span data-stu-id="621e1-144">It then packages everything required to run your code into a standalone container image, known as a *droplet*.</span></span> <span data-ttu-id="621e1-145">Nakonec cloudu Foundry plány aplikace na jednu z dostupných počítačů ve vašem prostředí a vytvoří adresa URL, kde můžete dosáhnout, která je k dispozici ve výstupu příkazu.</span><span class="sxs-lookup"><span data-stu-id="621e1-145">Finally, Cloud Foundry schedules the application on one of the available machines in your environment and creates a URL where you can reach it, which is available in the output of the command.</span></span>

![Výstup z příkazu nabízené CR][cf-push-output]

<span data-ttu-id="621e1-147">Pokud chcete zobrazit aplikace hello pružiny cloud, otevřete zadané adresy URL v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="621e1-147">To see the hello-spring-cloud application, open the provided URL in your browser:</span></span>

![Výchozí nastavení uživatelského rozhraní pro cloudové pružiny Hello][hello-spring-cloud-basic]

> [!NOTE] 
> <span data-ttu-id="621e1-149">Další informace o tom co se stane při `cf push`, najdete v části [jak jsou připraveny aplikace] [ cf-push-docs] v dokumentaci k Foundry cloudu.</span><span class="sxs-lookup"><span data-stu-id="621e1-149">To learn more about what happens during `cf push`, see [How Applications Are Staged][cf-push-docs] in the Cloud Foundry documentation.</span></span>

## <a name="view-application-logs"></a><span data-ttu-id="621e1-150">Zobrazit protokoly aplikací</span><span class="sxs-lookup"><span data-stu-id="621e1-150">View application logs</span></span>

<span data-ttu-id="621e1-151">Pokud chcete zobrazit protokoly pro aplikaci, jeho název můžete použít rozhraní příkazového řádku Foundry cloudu:</span><span class="sxs-lookup"><span data-stu-id="621e1-151">You can use the Cloud Foundry CLI to view logs for an application by its name:</span></span>

```bash
cf logs hello-spring-cloud
```

<span data-ttu-id="621e1-152">Ve výchozím nastavení, v protokolech příkaz používá *tail*, který zobrazuje nové protokoly, jako jsou zapsané.</span><span class="sxs-lookup"><span data-stu-id="621e1-152">By default, the logs command uses *tail*, which shows new logs as they are written.</span></span> <span data-ttu-id="621e1-153">Pokud chcete zobrazit nové protokoly se zobrazí, aktualizujte hello pružiny cloudové aplikace v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="621e1-153">To see new logs appear, refresh the hello-spring-cloud app in the browser.</span></span>

<span data-ttu-id="621e1-154">Chcete-li zobrazit protokoly, které již byla zapsána, přidejte `recent` přepínače:</span><span class="sxs-lookup"><span data-stu-id="621e1-154">To view logs that have already been written, add the `recent` switch:</span></span>

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-the-application"></a><span data-ttu-id="621e1-155">Škálování aplikace</span><span class="sxs-lookup"><span data-stu-id="621e1-155">Scale the application</span></span>

<span data-ttu-id="621e1-156">Ve výchozím nastavení `cf push` pouze vytvoří jednu instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="621e1-156">By default, `cf push` only creates a single instance of your application.</span></span> <span data-ttu-id="621e1-157">K zajištění vysoké dostupnosti a povolit škálování pro vyšší propustnost, chcete obecně spustit více než jednu instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="621e1-157">To ensure high availability and enable scale out for higher throughput, you generally want to run more than one instance of your applications.</span></span> <span data-ttu-id="621e1-158">Můžete snadno škálovat již nasazené aplikace pomocí `scale` příkaz:</span><span class="sxs-lookup"><span data-stu-id="621e1-158">You can easily scale out already deployed applications using the `scale` command:</span></span>

```bash
cf scale -i 2 hello-spring-cloud
```

<span data-ttu-id="621e1-159">Spuštění `cf app` příkaz u aplikace zobrazí, že cloudové Foundry vytváří jiná instance aplikace.</span><span class="sxs-lookup"><span data-stu-id="621e1-159">Running the `cf app` command on the application shows that Cloud Foundry is creating another instance of the application.</span></span> <span data-ttu-id="621e1-160">Po spuštění aplikace Foundry cloudu se automaticky spustí přenosů do ní služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="621e1-160">Once the application has started, Cloud Foundry automatically starts load balancing traffic to it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="621e1-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="621e1-161">Next steps</span></span>

- <span data-ttu-id="621e1-162">[Přečtěte si dokumentaci cloudu Foundry][cloudfoundry-docs]</span><span class="sxs-lookup"><span data-stu-id="621e1-162">[Read the Cloud Foundry documentation][cloudfoundry-docs]</span></span>
- <span data-ttu-id="621e1-163">[Nastavení modulu plug-in Visual Studio Team Services pro Cloud Foundry][vsts-plugin]</span><span class="sxs-lookup"><span data-stu-id="621e1-163">[Set up the Visual Studio Team Services plugin for Cloud Foundry][vsts-plugin]</span></span>
- <span data-ttu-id="621e1-164">[Konfigurace trysek Analýza protokolů Microsoft pro Cloud Foundry][loganalytics-nozzle]</span><span class="sxs-lookup"><span data-stu-id="621e1-164">[Configure the Microsoft Log Analytics Nozzle for Cloud Foundry][loganalytics-nozzle]</span></span>

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