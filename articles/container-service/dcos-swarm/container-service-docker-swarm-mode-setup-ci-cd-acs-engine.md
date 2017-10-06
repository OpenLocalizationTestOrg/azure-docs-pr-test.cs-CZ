---
title: "aaaCI/CD s Azure Container Service modul a režim Swarm | Microsoft Docs"
description: "Pomocí Docker Swarm režimu, registru kontejner Azure a Visual Studio Team Services toodeliver nepřetržitě aplikace .NET Core více kontejnerů Azure Container Service modul"
services: container-service
documentationcenter: " "
author: diegomrtnzg
manager: esterdnb
tags: acs, azure-container-service, acs-engine
keywords: "Docker, kontejnery, Micro-services, Swarm, Azure, Visual Studio Team služby, DevOps, modul služby ACS"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2017
ms.author: diegomrtnzg
ms.custom: mvc
ms.openlocfilehash: 040522c452f7ea0ce3c92f2fe57b1c141b97e380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a>Úplné CI/CD kanálu toodeploy aplikace s více kontejnerů v Azure Container Service pomocí modulu služby ACS a Docker Swarm režimu pomocí Visual Studio Team Services

*Tento článek vychází z [úplné CI/CD kanálu toodeploy aplikace s více kontejnerů v Azure Container Service pomocí nástroje Docker Swarm, pomocí sady Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) dokumentace*

V současné době jedna z hello nejnáročnějších úkolů při vývoj moderních aplikací pro hello cloud se může toodeliver tyto aplikace nepřetržitě. V tomto článku se dozvíte, jak se tooimplement úplné průběžnou integraci a nasazení (CI/CD) kanálu, pomocí: 
* Stroj služby Azure kontejner s režimem Docker Swarm
* Azure Container Registry
* Visual Studio Team Services

Tento článek vychází jednoduchou aplikaci, k dispozici na [Githubu](https://github.com/jcorioland/MyShop/tree/docker-linux), vyvinuté pomocí ASP.NET Core. Hello aplikace se skládá ze čtyř různých služeb: tři webového rozhraní API a jeden web front-endu:

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

cíl Hello je toodeliver tuto aplikaci nepřetržitě v clusteru Docker Swarm režimu, pomocí sady Visual Studio Team Services. Hello následující obrázek podrobnosti tohoto kanálu nastavené průběžné doručování:

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

Následuje stručné vysvětlení hello kroky:

1. Změny kódu jsou potvrzené toohello úložiště zdrojového kódu (tady Githubu) 
2. GitHub aktivuje build ve Visual Studio Team Services 
3. Visual Studio Team Services získá hello nejnovější verzi hello zdroje a vytvoří všechny hello bitové kopie, které tvoří aplikace hello 
4. Visual Studio Team Services doručí registru Docker tooa každé bitové kopie vytvořené pomocí služby Azure kontejneru registru hello 
5. Visual Studio Team Services aktivuje novou verzí 
6. verze Hello spouští některé příkazy hlavního uzlu clusteru serveru hello Azure container service pomocí protokolu SSH 
7. Docker Swarm režimu v clusteru hello vrátí hello nejnovější verzi bitové kopie hello 
8. Hello nové verze aplikace hello se nasazuje pomocí Docker zásobníku 

## <a name="prerequisites"></a>Požadavky

Před zahájením tohoto kurzu potřebujete toocomplete hello následující úlohy:

- [Vytvoření clusteru Swarm režimu v Azure Container Service s modulem služby ACS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [Propojení s clusterem Swarm hello v Azure Container Service](../container-service-connect.md)
- [Vytvoření služby Azure kontejneru registru](../../container-registry/container-registry-get-started-portal.md)
- [Máte účet a tým projekt Visual Studio Team Services vytvořen](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [Rozvětvení hello Githubu úložiště tooyour účet GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> Hello orchestrator Docker Swarm v Azure Container Service používá starší verzi samostatné Swarm. V současné době hello integrované [Swarm režimu](https://docs.docker.com/engine/swarm/) (v Docker 1.12 a vyšší) není podporovaný orchestrator v Azure Container Service. Z tohoto důvodu používáme [ACS modul](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), komunity podílí [šablony rychlý Start](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), nebo řešení Docker v hello [Azure Marketplace](https://azuremarketplace.microsoft.com).
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>Krok 1: Konfigurace účtu Visual Studio Team Services 

V této části můžete nakonfigurovat účet Visual Studio Team Services. Koncové tooconfigure služby VSTS služby body, ve vašem projektu Visual Studio Team Services, klikněte na tlačítko hello **nastavení** v hello nástrojů a vyberte ikonu **služby**.

![Koncový bod služby otevřete](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a>Připojení účtu Visual Studio Team Services a Azure

Nastavte připojení mezi projektu služby VSTS a účtu Azure.

1. Na levé straně hello, klikněte na tlačítko **nový koncový bod služby** > **Azure Resource Manager**.
2. služby VSTS toowork tooauthorize s vaším účtem Azure, vyberte vaše **předplatné** a klikněte na tlačítko **OK**.

    ![Visual Studio Team Services - autorizovat Azure](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a>Připojit Visual Studio Team Services a GitHub

Nastavte připojení mezi projektu služby VSTS a účtu Githubu.

1. Na levé straně hello, klikněte na tlačítko **nový koncový bod služby** > **Githubu**.
2. služby VSTS toowork tooauthorize s vaším účtem Githubu, klikněte na tlačítko **Authorize** a použijte postup hello v otevřeném okně hello.

    ![Visual Studio Team Services - autorizovat Githubu](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-tooyour-azure-container-service-cluster"></a>Připojení clusteru Azure Container Service tooyour služby VSTS

Hello poslední kroky před získáním do kanálu hello CI/CD jsou tooconfigure externí připojení tooyour Docker Swarm clusteru v Azure. 

1. Pro cluster hello Docker Swarm, přidání koncového bodu typu **SSH**. Zadejte informace o připojení SSH hello clusteru Swarm (hlavní uzel).

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

Všechny konfigurace hello je nyní provádí. V dalších krocích hello můžete vytvořit hello CI/CD kanálu, který vytvoří a nasadí hello aplikace toohello Docker Swarm clusteru. 

## <a name="step-2-create-hello-build-definition"></a>Krok 2: Vytvoření definici sestavení hello

V tomto kroku můžete nastavit definice sestavení pro svůj projekt služby VSTS a definovat hello sestavení pracovního postupu pro kontejner obrázků

### <a name="initial-definition-setup"></a>Definice počáteční instalace

1. toocreate definice sestavení, připojit tooyour Visual Studio Team Services projektu a klikněte na tlačítko **sestavení a verze**. V hello **sestavení definice** klikněte na tlačítko **+ nový**. 

    ![Visual Studio Team Services – nové sestavení definice](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. Vyberte hello **prázdný proces**.

    ![Visual Studio Team Services – nové definice buildu prázdný](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. Potom klikněte na hello **proměnné** kartě a vytvořte dva nové proměnné: **RegistryURL** a **AgentURL**. Vložte hello hodnoty registru a DNS agenty clusteru.

    ![Visual Studio Team Services - proměnné konfigurace sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. Na hello **definice sestavení** stránky, otevřete hello **aktivační události** a nakonfigurujte hello sestavení toouse průběžnou integraci s hello rozvětvení hello MyShop projektu, který jste vytvořili v hello požadavky. Pak vyberte **dávky změny**. Ujistěte se, že jste vybrali *docker linux* jako hello **větev specifikace**.

    ![Visual Studio Team Services - úložiště konfigurace sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. Nakonec klikněte na hello **možnosti** a nakonfigurujte hello výchozí agenta fronty příliš**Preview Linux hostované**.

    ![Visual Studio Team Services - Host Agent Configuration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-hello-build-workflow"></a>Definování hello sestavení pracovního postupu
Další kroky Hello definovat hello sestavení pracovního postupu. Nejprve je třeba zdroj hello tooconfigure hello kódu. toodo ho vyberte **Githubu** a **úložiště** a **větve** (docker-linux).

![Visual Studio Team Services - konfigurace zdrojového kódu](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

Existují pět toobuild bitové kopie kontejner pro hello *MyShop* aplikace. Každé bitové kopie je sestaven pomocí hello soubor Docker umístěný v hello složky projektu:

* ProductsApi
* Proxy server
* RatingsApi
* RecommandationsApi
* ShopFront

Budete potřebovat dva kroky Docker každé bitové kopie, jednu toobuild hello image a jednu image hello toopush v registru kontejner Azure hello. 

1. Klikněte na tlačítko tooadd krok v postupu sestavení hello **+ přidat krok sestavení** a vyberte **Docker**.

    ![Visual Studio Team Services - přidat kroky sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. Pro každé bitové kopie, nakonfigurovat jeden krok, který používá hello `docker build` příkaz.

    ![Visual Studio Team Services - Docker sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    Pro hello sestavení operaci, vyberte kontejner registr Azure, hello **vytvořit bitovou kopii** akce a hello soubor Docker, která definuje každé bitové kopie. Sada hello **pracovní adresář** jako hello soubor Docker kořenový adresář, definovat hello **název bitové kopie**a vyberte **zahrnují nejnovější značky**.
    
    Hello název bitové kopie má toobe v tomto formátu: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```. Nahraďte **[NAME]** s hello název bitové kopie:
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. Pro každé bitové kopie, nakonfigurujte druhý krok, který používá hello `docker push` příkaz.

    ![Visual Studio Team Services - nabízené Docker](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    Pro hello pomocí operace push, vyberte kontejner Azure registr, hello **Push bitovou kopii** akce, zadejte hello **název bitové kopie** který je vytvořen v předchozím kroku hello a vyberte **zahrnout nejnovější značky** .

4. Po dokončení můžete konfigurovat hello sestavení a push kroky pro každý hello pět bitových kopií, přidejte že tři další kroky v hello sestavení pracovního postupu.

   ![Visual Studio Team Services - přidat úlohu příkazového řádku](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. Úlohu příkazového řádku, která se používá bash skriptu tooreplace hello *RegistryURL* výskyt v soubor docker-compose.yml hello s proměnnou RegistryURL hello. 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio Team Services - vytvářené aktualizace souboru s adresou URL registru](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. Úlohu příkazového řádku, která se používá bash skriptu tooreplace hello *AgentURL* výskyt v soubor docker-compose.yml hello s proměnnou AgentURL hello.
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. Úloha, která zahodí hello aktualizovat vytvářené souboru, protože sestavení artefaktů, je možné v hello verzi. V tématu hello následující obrazovka podrobnosti.

         ![Visual Studio Team Services - publikování artefaktů](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![Visual Studio Team Services - publikování vytvářené souboru](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. Klikněte na tlačítko **Uložit & fronty** tootest vaší definice sestavení.

   ![Visual Studio Team Services - uložit a fronty](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Visual Studio Team Services – nové fronty](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. Pokud hello **sestavení** je správný, máte toosee Tato obrazovka:

  ![Visual Studio Team Services - sestavení bylo úspěšně dokončeno](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-hello-release-definition"></a>Krok 3: Vytvoření definice verze hello

Visual Studio Team Services umožňuje příliš[Správa verzí v různých prostředích](https://www.visualstudio.com/team-services/release-management/). Průběžné nasazování toomake nasazení aplikace na vašich různých prostředích (například vývojářů, testovací, předprodukční a produkční) smooth způsobem můžete povolit. Můžete vytvořit prostředí, které představuje váš cluster Azure Container Service Docker Swarm režimu.

![Visual Studio Team Services - tooACS verze](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Instalační program původní verze

1. Klikněte na tlačítko toocreate definici verze **verze** > **+ verze**

2. tooconfigure hello artefaktů zdroje, klikněte na tlačítko **artefakty** > **odkaz na zdroj artefaktů**. Zde propojte tento nový definice toohello sestavení pro vydání který jste zadali v předchozím kroku hello. Potom je k dispozici v procesu verze hello soubor docker-compose.yml hello.

    ![Visual Studio Team Services - artefakty verze](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. tooconfigure hello verze aktivační událost, klikněte na tlačítko **aktivační události** a vyberte **průběžné nasazování**. Aktivační událost hello sadu pro hello stejný zdroj artefaktů. Toto nastavení zajistí, že nová verze začne po úspěšném dokončení sestavení hello.

    ![Visual Studio Team Services - verze aktivační události](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. Klikněte na tlačítko tooconfigure hello verze proměnné, **proměnné** a vyberte **+ proměnné** toocreate tři nové proměnné s informacemi hello hello registru: **docker.username**, **docker.password**, a **docker.registry**. Vložte hello hodnoty registru a DNS agenty clusteru.

    ![Visual Studio Team Services - úložiště konfigurace sestavení](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > Jak je znázorněno na předchozí úvodní obrazovka, klikněte na tlačítko hello **zámku** zaškrtnout políčko docker.password. Toto nastavení je důležité toorestrict hello heslo.
    >

### <a name="define-hello-release-workflow"></a>Definice pracovního postupu verze hello

verze Hello pracovní postup se skládá z dvě úlohy, které přidáte.

1. Konfigurace úloh toosecurely kopie hello tvoří soubor tooa *nasazení* složky na hello Docker Swarm hlavního uzlu, pomocí připojení SSH hello jste nakonfigurovali dříve. V tématu hello následující obrazovka podrobnosti.
    
    Zdrojová složka:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```

    ![Visual Studio Team Services - verze spojovací bod služby](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. Nakonfigurujte druhý tooexecute úloh příkaz toorun bash `docker` a `docker stack deploy` příkazů na hlavní uzel hello. V tématu hello následující obrazovka podrobnosti.

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![Visual Studio Team Services - Bash verze](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    příkaz Hello proveden na hlavní server hello používá hello příkazového řádku Dockeru a hello Docker Compose CLI toodo hello následující úlohy:

    - Protokolu v registru toohello kontejner Azure (používá tři proměnné sestavení, které jsou definovány v hello **proměnné** karty)
    - Definování hello **DOCKER_HOST** proměnné toowork koncovému bodu Swarm hello (: 2375)
    - Přejděte toohello *nasazení* složky, který byl vytvořen hello předcházející úlohy zabezpečené kopie a který obsahuje soubor docker-compose.yml hello 
    - Spuštění `docker stack deploy` příkazy, které hello nových bitových kopií pro vyžádání obsahu a vytvoření kontejnerů hello.

    >[!IMPORTANT]
    > Jak je znázorněno na předchozí obrazovce hello, nechte hello **nezdaří do datového proudu STDERR** nezaškrtnuté políčko. Toto nastavení umožňuje nám toocomplete hello verzi z důvodu příliš`docker-compose` vytiskne několik diagnostické zprávy, jako jsou kontejnery se zastavit nebo se na výstup hello standardní chyba odstraněn. Pokud zaškrtnete políčko hello, hlásí Visual Studio Team Services nastaly chyby během hello verzi, i pokud všechno proběhne správně.
    >
3. Uložte tuto novou verzi definici.

## <a name="step-4-test-hello-cicd-pipeline"></a>Krok 4: Test hello CI/CD kanálu

Teď, když jste hotovi s konfigurací hello, je čas tootest tento nový kanál CI/CD. Hello nejjednodušší způsob, jak tootest je hello tooupdate hello zdrojového kódu a provést změny do úložiště GitHub. Několik sekund poté, co push hello kód, zobrazí se nové sestavení spuštěná ve Visual Studio Team Services. Po úspěšném dokončení novou verzi je aktivována a nasazena hello nové verze aplikace hello na clusteru Azure Container Service hello.

## <a name="next-steps"></a>Další kroky

* Další informace o CI/CD s Visual Studio Team Services najdete v tématu hello [služby VSTS sestavení přehled](https://www.visualstudio.com/docs/build/overview).
* Další informace o modulu služby ACS, najdete v části hello [úložiště GitHub modul ACS](https://github.com/Azure/acs-engine).
* Další informace o Docker Swarm režimu najdete v tématu hello [Docker Swarm přehled režimu](https://docs.docker.com/engine/swarm/).
