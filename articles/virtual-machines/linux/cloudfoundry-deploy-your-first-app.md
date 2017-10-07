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
# <a name="deploy-your-first-app-toocloud-foundry-on-microsoft-azure"></a>Nasazení první aplikaci tooCloud Foundry v Microsoft Azure

[Cloud Foundry](http://cloudfoundry.org) je platforma oblíbených open-source aplikace k dispozici na Microsoft Azure. V tomto článku ukážeme, jak toodeploy a spravovat aplikaci v cloudu Foundry v prostředí Azure.

## <a name="create-a-cloud-foundry-environment"></a>Vytvořte prostředí cloudu Foundry

Existuje několik možností pro vytvoření Foundry cloudové prostředí v Azure:

- Použití hello [nabídka hrají cloudu Foundry] [ pcf-azuremarketplace] v Azure Marketplace toocreate hello standardní prostředí, které obsahuje PCF Ops Manager a hello Azure Service Broker. Můžete najít [úplné pokyny] [ pcf-azuremarketplace-pivotaldocs] pro nasazení hello marketplace nabízí v hello hrají dokumentaci.
- Vytvořit vlastní prostředí pomocí [ručního nasazení hrají cloudu Foundry][pcf-custom].
- [Nasazení balíčků cloudu Foundry open-source hello přímo] [ oss-cf-bosh] nastavením [BOSH](http://bosh.io) ředitel, virtuální počítač, který koordinuje hello nasazení hello Foundry cloudové prostředí.

> [!IMPORTANT] 
> Pokud nasazujete PCF z hello Azure Marketplace, poznamenejte si hello SYSTEMDOMAINURL a přihlašovací údaje správce hello požadované tooaccess hello hrají Správce aplikací, které jsou popsané v příručce pro nasazení webu marketplace hello. Že jsou potřebné toocomplete v tomto kurzu. Pro nasazení webu marketplace je hello SYSTEMDOMAINURL v https://system hello formuláře. *ip adresu*. cf.pcfazure.com.

## <a name="connect-toohello-cloud-controller"></a>Připojit toohello Kontroleru cloudu

Hello Kontroleru cloudu je hello primární vstupní bod tooa Foundry cloudové prostředí pro nasazení a Správa aplikací. základní Hello cloudu řadiče rozhraní API (CCAPI) je rozhraní REST API, ale je přístupný prostřednictvím různých nástrojů. V takovém případě budeme pracovat s ním prostřednictvím hello [cloudu Foundry rozhraní příkazového řádku][cf-cli]. Hello rozhraní příkazového řádku můžete nainstalovat na systému Linux, systému MacOS nebo Windows, ale pokud si přejete není tooinstall ho vůbec, je k dispozici předinstalován v hello [prostředí cloudu Azure][cloudshell-docs].

toolog, předřadit `api` toohello SYSTEMDOMAINURL, který jste získali z marketplace nasazení hello. Vzhledem k tomu, že nasazení výchozí hello používá certifikát podepsaný svým držitelem, musí rovněž zahrnovat hello `skip-ssl-validation` přepínače.

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

Jste výzvami toolog v toohello Kontroleru cloudu. Pomocí pověření účtu správce hello, které jste získali z kroků nasazení hello marketplace.

Poskytuje cloudu Foundry *orgs* a *prostory* jako týmy hello tooisolate obory názvů a prostředí v rámci sdílené nasazení. Hello PCF marketplace nasazení obsahuje výchozí hello *systému* organizace a sadu prostorů vytvořit toocontain hello základní součásti, jako je služba hello automatické škálování a zprostředkovatele služby Azure hello. Nyní, vyberte hello *systému* místa.


## <a name="create-an-org-and-space"></a>Vytvoření organizace a místa

Pokud zadáte `cf apps`, najdete v části sadu aplikací systému, které jsou nasazené v prostoru hello systému v rámci systému org. hello 

Byste měli mít hello *systému* org vyhrazena pro aplikace, systému, takže vytvořte organizace a místo toohouse naše ukázková aplikace.

```bash
cf create-org myorg
cf create-space dev -o myorg
```

Použijte hello cíl příkazu tooswitch toohello nové organizace a místa:

```bash
cf target -o testorg -s dev
```

Teď když nasadíte aplikaci, je vytvořeno automaticky v nové organizace hello a místo. tooconfirm, která aktuálně nejsou k dispozici žádné aplikace. v nové organizace hello/prostor, zadejte `cf apps` znovu.

> [!NOTE] 
> Další informace o orgs a prostory a jak mohou být použity pro řízení přístupu na základě role (RBAC) najdete v tématu hello [dokumentace cloudu Foundry][cf-orgs-spaces-docs].

## <a name="deploy-an-application"></a>Nasazení aplikace

Můžeme použít ukázkové aplikace cloudu Foundry názvem Hello pružiny cloudu, což je napsanou v jazyce Java a podle hello [pružiny Framework](http://spring.io) a [pružiny spouštěcí](http://projects.spring.io/spring-boot/).

### <a name="clone-hello-hello-spring-cloud-repository"></a>Klonování hello Hello pružiny cloudové úložiště

Hello Hello pružiny cloudu ukázkové aplikace je k dispozici na Githubu. Naklonujte tooyour prostředí a změňte do nového adresáře hello:

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-hello-application"></a>Vytvoření aplikace hello

Sestavení hello aplikace pomocí [Apache Maven](http://maven.apache.org).

```bash
mvn clean package
```

### <a name="deploy-hello-application-with-cf-push"></a>Nasazení aplikace hello s nabízené CR

Většina aplikací tooCloud Foundry můžete nasadit pomocí hello `push` příkaz:

```bash
cf push
```

Pokud jste *nabízené* aplikace, cloudu Foundry zjistí hello typu aplikace (v tomto případě aplikace v jazyce Java) a identifikuje jeho závislosti (v tomto případě rámci pružiny hello). Pak balíčky všechno požadované toorun kódu do kontejneru bitovou kopii samostatné říká *droplet*. Nakonec cloudu Foundry plány hello aplikaci na některém hello dostupných počítačů ve vašem prostředí a vytvoří adresa URL, kde můžete dosáhnout, která je dostupná v hello výstup hello příkazu.

![Výstup z příkazu nabízené CR][cf-push-output]

toosee hello hello. pružiny cloudových aplikací, otevřete hello zadat adresu URL v prohlížeči:

![Výchozí nastavení uživatelského rozhraní pro cloudové pružiny Hello][hello-spring-cloud-basic]

> [!NOTE] 
> toolearn více informací o co se stane, že během `cf push`, najdete v části [jak jsou připraveny aplikace] [ cf-push-docs] v hello cloudu Foundry dokumentaci.

## <a name="view-application-logs"></a>Zobrazit protokoly aplikací

Jeho název můžete použít protokoly tooview hello cloudu Foundry rozhraní příkazového řádku pro aplikaci:

```bash
cf logs hello-spring-cloud
```

Ve výchozím nastavení, protokoly hello používá příkaz *tail*, který zobrazuje nové protokoly, jako jsou zapsané. toosee nové protokoly se zobrazí, aktualizujte hello hello pružiny cloud aplikaci v prohlížeči hello.

tooview protokoly, které již byla zapsána, přidejte hello `recent` přepínače:

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-hello-application"></a>Škálování aplikace hello

Ve výchozím nastavení `cf push` pouze vytvoří jednu instanci aplikace. tooensure vysokou dostupnost a povolit škálování pro vyšší propustnost, chcete obecně toorun více než jednu instanci aplikace. Můžete snadno škálovat již nasazené aplikace pomocí hello `scale` příkaz:

```bash
cf scale -i 2 hello-spring-cloud
```

Spuštěné hello `cf app` příkaz na hello aplikace zobrazí, že cloudové Foundry vytváří jiná instance aplikace hello. Po zahájení aplikace hello, Foundry cloudu se automaticky spustí provoz tooit Vyrovnávání zatížení.


## <a name="next-steps"></a>Další kroky

- [Čtení hello dokumentace cloudu Foundry][cloudfoundry-docs]
- [Nastavení modulu plug-in hello Visual Studio Team Services pro Cloud Foundry][vsts-plugin]
- [Konfigurace hello trysek Analýza protokolů Microsoft pro Cloud Foundry][loganalytics-nozzle]

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