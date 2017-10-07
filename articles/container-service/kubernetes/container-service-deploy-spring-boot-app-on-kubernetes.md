---
title: "aaaDeploy pružiny spouštění aplikace na Kubernetes v Azure Container Service | Microsoft Docs"
description: "Tento kurz vás provede, když hello kroky toodeploy pružiny spuštění aplikace v clusteru s podporou Kubernetes v Microsoft Azure."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 2bf9df459f874a1f478f43cdd29992d86c370837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-hello-azure-container-service"></a>Nasazení aplikace spouštěcí pružiny na Kubernetes Cluster hello Azure Container Service

Hello  **[pružiny Framework]**  oblíbených rozhraní open source, které pomáhá vytvářet webové, mobilní a aplikacích API vývojáře v jazyce Java. Tento kurz používá ukázkovou aplikaci vytvořený [pružiny spouštěcí], konvence přístupu při použití pružiny tooget rychle začít.

**[Kubernetes]**  a  **[Docker]**  jsou open-source řešení, která pomáhají vývojáři automatizovat, hello nasazení, škálování a správu svých aplikací běžících v kontejnerech.

Tento kurz vás provede, když kombinace těchto dvou oblíbených, open-source technologie toodevelop a nasaďte tooMicrosoft pružiny spouštěcí aplikace Azure. Přesněji řečeno, použijete  *[pružiny spouštěcí]*  pro vývoj aplikací  *[Kubernetes]*  pro nasazení kontejneru a hello [Azure Container Service (ACS)] toohost vaší aplikace.

### <a name="prerequisites"></a>Požadavky

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].
* Hello [rozhraní příkazového řádku Azure (CLI)].
* Aktuální [Java Developer Kit (JDK)].
* Apache na [Maven] sestavení nástroj (verze 3).
* A [Git] klienta.
* A [Docker] klienta.

> [!NOTE]
>
> Z důvodu toohello virtualizace požadavky tohoto kurzu nelze sledovat hello kroky v tomto článku na virtuálním počítači; fyzický počítač musí používat s funkcemi virtualizace.
>

## <a name="create-hello-spring-boot-on-docker-getting-started-web-app"></a>Vytvoření hello pružiny spouštěcí na Docker Začínáme webové aplikace

Hello následující kroky vás provede procesem vytváření webové aplikace pružiny spouštěcí a místní testování.

1. Otevřete příkazový řádek a vytvářet toohold místního adresáře aplikace a změnit adresář toothat; například:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --nebo--
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klon hello [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře hello.
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Změňte adresář toohello dokončení projektu.
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. Používejte Maven toobuild a spuštění hello ukázkovou aplikaci.
   ```
   mvn package spring-boot:run
   ```

1. Testování hello webové aplikace procházením toohttp://localhost:8080 nebo s následující hello `curl` příkaz:
   ```
   curl http://localhost:8080
   ```

1. Měli byste vidět hello následující zpráva: **Hello, World Docker**

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a>Vytvoření registru kontejneru služby Azure pomocí hello rozhraní příkazového řádku Azure

1. Otevřete příkazový řádek.

1. Přihlaste se tooyour účet Azure:
   ```azurecli
   az login
   ```

1. Vytvořte skupinu prostředků pro hello použité v tomto kurzu prostředky Azure.
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. Vytvořte kontejner privátní Azure registru ve skupině prostředků hello. kurz Hello doručí hello ukázkové aplikace jako soubor Docker image toothis registru v dalších krocích. Nahraďte `wingtiptoysregistry` s jedinečným názvem pro vaše registru.
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-toohello-container-registry"></a>Push vaší aplikace toohello kontejneru registru

1. Přejděte toohello konfiguračního adresáře pro instalaci Maven (výchozí ~/.m2/ nebo C:\Users\username\.m2) a otevřené hello *souborech settings.xml* soubor v textovém editoru.

1. Načtení hello hesla pro vaše kontejneru registru z hello rozhraní příkazového řádku Azure.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Přidání vaší registru kontejner Azure id a heslo tooa nové `<server>` kolekce v hello *souborech settings.xml* souboru.
Hello `id` a `username` jsou hello název registru hello. Použití hello `password` hodnotu z předchozí příkaz hello (bez uvozovek).

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí (například "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker / dokončení*") a otevřete hello *pom.xml* soubor v textovém editoru.

1. Aktualizace hello `<properties>` kolekce v hello *pom.xml* soubor s hodnotou server hello přihlášení pro vaše registru kontejner Azure.

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Aktualizace hello `<plugins>` kolekce v hello *pom.xml* souboru, který hello `<plugin>` obsahuje hello přihlašovací adresu a registru název serveru pro váš registru kontejner Azure.

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí a spusťte následující příkaz toobuild hello Docker kontejneru a nabízených hello image toohello registru hello:

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  Zobrazí chybovou zprávu, která je podobné tooone hello následující při Maven nabízených oznámení tooAzure hello bitové kopie:
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Pokud se tato chyba, přihlaste se tooAzure z příkazového řádku Dockeru hello.
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Potom push vaší kontejneru:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-hello-azure-cli"></a>Vytvoření clusteru s podporou Kubernetes na ACS pomocí hello rozhraní příkazového řádku Azure

1. Vytvoření clusteru Kubernetes v Azure Container Service. Hello následující příkaz vytvoří *kubernetes* clusteru v hello *Northwind kubernetes* prostředků skupiny s *Northwind containerservice* jako hello cluster název, a *Northwind kubernetes* jako předpona DNS hello:
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   Tento příkaz může chvíli trvat toocomplete.

1. Nainstalujte `kubectl` pomocí hello rozhraní příkazového řádku Azure. Linux uživatelé mohou mít tooprefix tento příkaz s `sudo` vzhledem k tomu, že nasadí hello Kubernetes CLI příliš`/usr/local/bin`.
   ```azurecli
   az acs kubernetes install-cli
   ```

1. Stáhněte si informace o konfiguraci clusteru hello tak můžete spravovat cluster z hello Kubernetes webové rozhraní a `kubectl`. 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-hello-image-tooyour-kubernetes-cluster"></a>Nasazení clusteru Kubernetes tooyour image hello

V tomto kurzu nasadí hello aplikace pomocí `kubectl`, pak umožňují tooexplore hello nasazení prostřednictvím hello Kubernetes webového rozhraní.

### <a name="deploy-with-hello-kubernetes-web-interface"></a>Nasazení s hello Kubernetes webové rozhraní

1. Otevřete příkazový řádek.

1. Otevřít web hello konfigurace pro váš cluster Kubernetes ve výchozím prohlížeči:
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. Když hello Kubernetes konfigurace webu se otevře v prohlížeči, klikněte na odkaz hello příliš**nasadit kontejnerizované aplikaci**:

   ![Kubernetes konfigurace webu][KB01]

1. Když hello **nasadit kontejnerizované aplikaci** se zobrazí stránka, zadejte hello následující možnosti:

   a. Vyberte **zadejte níže uvedené podrobnosti o aplikaci**.

   b. Zadejte název aplikace pružiny spouštěcí pro hello **název aplikace**; například: "*gs pružiny spouštěcí docker*".

   c. Zadejte přihlašovací serveru a kontejneru bitové kopie z dříve hello **kontejneru image**; například: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".

   d. Zvolte **externí** pro hello **služby**.

   e. Zadejte vaše externí i interní portů v hello **Port** a **cíle port** textových polí.

   ![Kubernetes konfigurace webu][KB02]


1. Klikněte na tlačítko **nasadit** toodeploy hello kontejneru.

   ![Nasazení kontejneru][KB05]

1. Po nasazení vaší aplikace, zobrazí se aplikace pružiny spouštěcí uvedené v části **služby**.

   ![Kubernetes služby][KB06]

1. Pokud kliknete na odkaz hello **externí koncové body**, uvidíte pružiny spouštěcí aplikace spuštěné v Azure.

   ![Kubernetes služby][KB07]

   ![Procházet ukázkovou aplikaci v Azure][SB02]


### <a name="deploy-with-kubectl"></a>Nasazení s kubectl

1. Otevřete příkazový řádek.

1. Spusťte vašeho kontejneru v clusteru Kubernetes hello pomocí hello `kubectl run` příkaz. Zadejte název služby pro aplikaci v Kubernetes a název hello úplnou bitovou kopii. Například:
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   V tomto příkazu:

   * název kontejneru Hello `gs-spring-boot-docker` je zadán ihned po hello `run` příkaz

   * Hello `--image` parametr určuje hello kombinaci přihlášení na server a název bitové kopie jako`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. Vystavení clusteru Kubernetes externě pomocí hello `kubectl expose` příkaz. Zadejte název vaší služby, hello veřejné TCP port používaný tooaccess hello aplikace a hello interní cílový port, který aplikace naslouchá na. Například:
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   V tomto příkazu:

   * název kontejneru Hello `gs-spring-boot-docker` je zadán ihned po hello `expose deployment` příkaz

   * Hello `--type` parametr určuje, že cluster hello používá nástroj pro vyrovnávání zatížení

   * Hello `--port` parametr určuje hello veřejné TCP port 80. Máte přístup k aplikaci hello na tento port.

   * Hello `--target-port` parametr určuje hello interní TCP port 8080. Nástroj pro vyrovnávání zatížení Hello předává požadavky tooyour aplikace na tomto portu.

1. Po nasazení aplikace hello toohello clusteru dotaz hello externí IP adresu a otevřete ve webovém prohlížeči:

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Procházet ukázkovou aplikaci v Azure][SB02]


## <a name="next-steps"></a>Další kroky

Další informace o používání spouštěcí Spring v Azure najdete v části hello následující články:

* [Nasazení aplikace spouštěcí pružiny toohello Azure App Service](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Nasazení aplikace pružiny spouštění v systému Linux v hello Azure Container Service](container-service-deploy-spring-boot-app-on-linux.md)

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].

Další informace o hello pružiny spouštěcí na Docker ukázkový projekt najdete v tématu [pružiny spouštěcí na Docker Začínáme].

Hello následující odkazy obsahují další informace o vytváření aplikací pro spouštěcí pružiny:

* Další informace o vytvoření jednoduché aplikace pružiny spouštěcí najdete v části hello Spring Initializr v https://start.spring.io/.

Hello následující odkazy obsahují další informace o používání Kubernetes s Azure:

* [Začínáme s Kubernetes cluster Container Service](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [Pomocí hello Kubernetes webové uživatelské rozhraní s Azure Container Service](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

Další informace o používání rozhraní příkazového řádku Kubernetes je k dispozici v hello **kubectl** v uživatelské příručce <https://kubernetes.io/docs/user-guide/kubectl/>.

Hello Kubernetes web má několik články, které popisují pomocí bitové kopie v privátní registrech:

* [Konfigurace služby pro pracovními stanicemi soustředěnými kolem účtů]
* [Obory názvů]
* [Stahování bitovou kopii z privátní registru]

Další příklady jak toouse vlastní Docker obrázků s Azure, najdete v části [pomocí vlastní image Docker pro webové aplikace Azure v systému Linux].

<!-- URL List -->

[rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[pomocí vlastní image Docker pro webové aplikace Azure v systému Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker
[pružiny Framework]: https://spring.io/
[Konfigurace služby pro pracovními stanicemi soustředěnými kolem účtů]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Obory názvů]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Stahování bitovou kopii z privátní registru]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR04.png

[KB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB01.png
[KB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB02.png
[KB03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB03.png
[KB04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB04.png
[KB05]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB05.png
[KB06]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB06.png
[KB07]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB07.png
