---
title: aaaCI/CD s Azure Container Service a Swarm | Microsoft Docs
description: "Pomocí Docker Swarm, registru kontejner Azure a Visual Studio Team Services toodeliver nepřetržitě aplikace .NET Core více kontejnerů Azure Container Service"
services: container-service
documentationcenter: " "
author: jcorioland
manager: pierlag
tags: acs, azure-container-service
keywords: Docker, kontejnery, Micro-services, Swarm, Azure, Visual Studio Team Services, DevOps
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: 35348800aa620469fb62ab3e5a02b33834359446
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a>Úplné CI/CD kanálu toodeploy aplikace s více kontejnerů v Azure Container Service pomocí nástroje Docker Swarm, pomocí sady Visual Studio Team Services

Jeden z hello nejnáročnějších úkolů při vývoj moderních aplikací pro hello cloud se může toodeliver tyto aplikace průběžně. V tomto článku se dozvíte, jak vytvářet tooimplement úplné průběžnou integraci a nasazení (CI/CD) kanálu Azure Container Service pomocí Docker Swarm, registru kontejner Azure a Visual Studio Team Services a správa verzí.

Tento článek vychází jednoduchou aplikaci, k dispozici na [Githubu](https://github.com/jcorioland/MyShop/tree/acs-docs), vyvinuté pomocí ASP.NET Core. Hello aplikace se skládá ze čtyř různých služeb: tři webového rozhraní API a jeden web front-endu:

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

cíl Hello je toodeliver tuto aplikaci nepřetržitě v clusteru Docker Swarm, pomocí sady Visual Studio Team Services. Hello následující obrázek podrobnosti tohoto kanálu nastavené průběžné doručování:

![MyShop ukázkové aplikace](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Následuje stručné vysvětlení hello kroky:

1. Změny kódu jsou potvrzené toohello úložiště zdrojového kódu (tady Githubu) 
2. GitHub aktivuje build ve Visual Studio Team Services 
3. Visual Studio Team Services získá hello nejnovější verzi hello zdroje a vytvoří všechny hello bitové kopie, které tvoří aplikace hello 
4. Visual Studio Team Services doručí registru Docker tooa každé bitové kopie vytvořené pomocí služby Azure kontejneru registru hello 
5. Visual Studio Team Services aktivuje novou verzí 
6. verze Hello spouští některé příkazy hlavního uzlu clusteru serveru hello Azure container service pomocí protokolu SSH 
7. Docker Swarm hello clusteru si hello nejnovější verzi hello obrázků 
8. Hello nové verze aplikace hello se nasazuje pomocí Docker Compose 

## <a name="prerequisites"></a>Požadavky

Před zahájením tohoto kurzu potřebujete toocomplete hello následující úlohy:

- [Vytvoření clusteru Swarm v Azure Container Service](container-service-deployment.md)
- [Propojení s clusterem Swarm hello v Azure Container Service](../container-service-connect.md)
- [Vytvoření služby Azure kontejneru registru](../../container-registry/container-registry-get-started-portal.md)
- [Máte účet a tým projekt Visual Studio Team Services vytvořen](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [Rozvětvení hello Githubu úložiště tooyour účet GitHub](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Budete také potřebovat počítače Ubuntu (14.04 a 16.04) s Docker nainstalována. Tento počítač se používá ve Visual Studio Team Services během hello sestavení a verze procesy. Jedním ze způsobů toocreate tento počítač je k dispozici v hello image hello toouse [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/). 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>Krok 1: Konfigurace účtu Visual Studio Team Services 

V této části můžete nakonfigurovat účet Visual Studio Team Services.

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a>Konfigurace agenta pro Visual Studio Team Services Linux sestavení

toocreate imagí Dockeru a nabízených sestavení těchto bitových kopií do služby Azure kontejneru registru z Visual Studio Team Services, musíte tooregister agenta systému Linux. Máte tyto možnosti instalace:

* [Nasazení agenta v systému Linux](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [Pomocí Docker toorun hello služby VSTS agenta](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-hello-docker-integration-vsts-extension"></a>Instalace rozšíření služby VSTS integrace Dockeru hello

Společnost Microsoft poskytuje toowork služby VSTS rozšíření s Docker v sestavení a verze procesy. Toto rozšíření je k dispozici v hello [služby VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker). Klikněte na tlačítko **nainstalovat** tooadd tento účet služby VSTS tooyour rozšíření:

![Nainstalujte hello integrace Dockeru](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

Zobrazí se výzva tooconnect tooyour služby VSTS účtu pomocí svých přihlašovacích údajů. 

### <a name="connect-visual-studio-team-services-and-github"></a>Připojit Visual Studio Team Services a GitHub

Nastavte připojení mezi projektu služby VSTS a účtu Githubu.

1. V projektu Visual Studio Team Services, klikněte na tlačítko hello **nastavení** v hello nástrojů a vyberte ikonu **služby**.

    ![Visual Studio Team Services - externí připojení](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. Na levé straně hello, klikněte na tlačítko **nový koncový bod služby** > **Githubu**.

    ![Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. služby VSTS toowork tooauthorize s vaším účtem Githubu, klikněte na tlačítko **Authorize** a použijte postup hello v otevřeném okně hello.

    ![Visual Studio Team Services - autorizovat Githubu](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-tooyour-azure-container-registry-and-azure-container-service-cluster"></a>Připojení služby VSTS tooyour kontejner Azure registru a cluster Azure Container Service

Hello poslední kroky před získáním do kanálu hello CI/CD jsou tooconfigure externí připojení tooyour kontejneru registru a cluster Docker Swarm v Azure. 

1. V hello **služby** nastavení projektu Visual Studio Team Services přidat koncový bod služby typu **Docker registru**. 

2. V překryvném hello, které se otevře zadejte hello URL a pověření hello registru systému Windows Azure kontejneru.

    ![Visual Studio Team Services - Docker registru](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. Pro cluster hello Docker Swarm, přidání koncového bodu typu **SSH**. Zadejte informace o připojení SSH hello clusteru Swarm.

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Všechny konfigurace hello je nyní provádí. V dalších krocích hello můžete vytvořit hello CI/CD kanálu, který vytvoří a nasadí hello aplikace toohello Docker Swarm clusteru. 

## <a name="step-2-create-hello-build-definition"></a>Krok 2: Vytvoření definici sestavení hello

V tomto kroku můžete nastavit definitionfor sestavení projektu služby VSTS a definovat hello sestavení pracovního postupu pro kontejner obrázků

### <a name="initial-definition-setup"></a>Definice počáteční instalace

1. toocreate definice sestavení, připojit tooyour Visual Studio Team Services projektu a klikněte na tlačítko **sestavení a verze**. 

2. V hello **sestavení definice** klikněte na tlačítko **+ nový**. Vyberte hello **prázdný** šablony.

    ![Visual Studio Team Services – nové sestavení definice](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. Konfigurace nového sestavení hello se zdrojem úložiště Githubu, zkontrolujte **průběžnou integraci**a vyberte hello agenta fronty, kde jste zaregistrovali Linux agent. Klikněte na tlačítko **vytvořit** toocreate hello sestavení definice.

    ![Visual Studio Team Services – vytvoření definice sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. Na hello **definice sestavení** stránky, poprvé otevřete hello **úložiště** a nakonfigurujte hello sestavení toouse hello rozvětvení hello MyShop projektu, který jste vytvořili v hello požadavky. Ujistěte se, že jste vybrali *acs dokumentace* jako hello **výchozí větev**.

    ![Visual Studio Team Services - úložiště konfigurace sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. Na hello **aktivační události** nakonfigurujte hello sestavení toobe spustí po každém potvrzení. Vyberte **průběžnou integraci** a **dávky změny**.

    ![Visual Studio Team Services - konfigurace aktivační události sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-hello-build-workflow"></a>Definování hello sestavení pracovního postupu
Další kroky Hello definovat hello sestavení pracovního postupu. Existují pět toobuild bitové kopie kontejner pro hello *MyShop* aplikace. Každé bitové kopie je sestaven pomocí hello soubor Docker umístěný v hello složky projektu:

* ProductsApi
* Proxy server
* RatingsApi
* RecommandationsApi
* ShopFront

Budete potřebovat dva kroky Docker tooadd každé bitové kopie, jednu toobuild hello image a jednu image hello toopush v registru kontejner Azure hello. 

1. Klikněte na tlačítko tooadd krok v postupu sestavení hello **+ přidat krok sestavení** a vyberte **Docker**.

    ![Visual Studio Team Services - přidat kroky sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. Pro každé bitové kopie, nakonfigurovat jeden krok, který používá hello `docker build` příkaz.

    ![Visual Studio Team Services - Docker sestavení](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Pro hello sestavení operaci, vyberte kontejner Azure registr, hello **vytvořit bitovou kopii** akce a hello soubor Docker, která definuje každé bitové kopie. Sada hello **sestavení kontextu** jako hello soubor Docker kořenový adresář a definujte hello **název bitové kopie**. 
    
    Jak je vidět na předchozím obrazovky hello, začněte název bitové kopie hello hello URI registru systému Windows Azure kontejneru. (Můžete také použít značku sestavení proměnné tooparameterize hello hello Image, jako je například identifikátor hello sestavení v tomto příkladu.)

3. Pro každé bitové kopie, nakonfigurujte druhý krok, který používá hello `docker push` příkaz.

    ![Visual Studio Team Services - nabízené Docker](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Pro hello pomocí operace push, vyberte kontejner Azure registr, hello **Push bitovou kopii** akce a zadejte hello **název bitové kopie** který je vytvořen v předchozím kroku hello.

4. Po dokončení můžete konfigurovat hello sestavení a push kroky pro každý hello pět bitových kopií, přidejte že dva další kroky v hello sestavení pracovního postupu.

    a. Úlohu příkazového řádku, která se používá bash skriptu tooreplace hello *BuildNumber* výskyt v soubor docker-compose.yml hello s hello aktuální sestavení ID. V tématu hello následující obrazovka podrobnosti.

    ![Visual Studio Team Services - vytvářené aktualizace souboru](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Úloha, která zahodí hello aktualizovat vytvářené souboru, protože sestavení artefaktů, je možné v hello verzi. V tématu hello následující obrazovka podrobnosti.

    ![Visual Studio Team Services - publikování vytvářené souboru](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. Klikněte na tlačítko **Uložit** a název vaší definice sestavení.

## <a name="step-3-create-hello-release-definition"></a>Krok 3: Vytvoření definice verze hello

Visual Studio Team Services umožňuje příliš[Správa verzí v různých prostředích](https://www.visualstudio.com/team-services/release-management/). Průběžné nasazování toomake nasazení aplikace na vašich různých prostředích (například vývojářů, testovací, předprodukční a produkční) smooth způsobem můžete povolit. Můžete vytvořit nové prostředí, které představuje váš cluster Azure Container Service Docker Swarm.

![Visual Studio Team Services - tooACS verze](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Instalační program původní verze

1. Klikněte na tlačítko toocreate definici verze **verze** > **+ verze**

2. tooconfigure hello artefaktů zdroje, klikněte na tlačítko **artefakty** > **odkaz na zdroj artefaktů**. Zde propojte tento nový definice toohello sestavení pro vydání který jste zadali v předchozím kroku hello. Díky tomu je k dispozici v procesu verze hello soubor docker-compose.yml hello.

    ![Visual Studio Team Services - artefakty verze](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. tooconfigure hello verze aktivační událost, klikněte na tlačítko **aktivační události** a vyberte **průběžné nasazování**. Aktivační událost hello sadu pro hello stejný zdroj artefaktů. Toto nastavení zajistí, že začne novou verzi také hello sestavení se dokončí úspěšně.

    ![Visual Studio Team Services - verze aktivační události](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-hello-release-workflow"></a>Definice pracovního postupu verze hello

verze Hello pracovní postup se skládá z dvě úlohy, které přidáte.

1. Konfigurace úloh toosecurely kopie hello tvoří soubor tooa *nasazení* složky na hello Docker Swarm hlavního uzlu, pomocí připojení SSH hello jste nakonfigurovali dříve. V tématu hello následující obrazovka podrobnosti.

    ![Visual Studio Team Services - verze spojovací bod služby](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. Nakonfigurujte druhý tooexecute úloh příkaz toorun bash `docker` a `docker-compose` příkazů na hlavní uzel hello. V tématu hello následující obrazovka podrobnosti.

    ![Visual Studio Team Services - Bash verze](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    příkaz Hello proveden na hello hello hlavní pomocí příkazového řádku Dockeru a hello Docker Compose CLI toodo hello následující úlohy:

    - Přihlášení toohello kontejner Azure registru (používá tři variab'les sestavení, která jsou definována v hello **proměnné** karty)
    - Definování hello **DOCKER_HOST** proměnné toowork koncovému bodu Swarm hello (: 2375)
    - Přejděte toohello *nasazení* složky, který byl vytvořen hello předcházející úlohy zabezpečené kopie a který obsahuje soubor docker-compose.yml hello 
    - Spuštění `docker-compose` příkazy, které hello nové bitové kopie, pro vyžádání obsahu zastavení služeb hello, odeberte hello služby a vytvoření kontejnerů hello.

    >[!IMPORTANT]
    > Jak je vidět na předchozím obrazovky hello, nechte hello **nezdaří do datového proudu STDERR** nezaškrtnuté políčko. To je důležité nastavit, protože `docker-compose` vytiskne několik diagnostické zprávy, jako jsou kontejnery se zastavit nebo se na výstup hello standardní chyba odstraněn. Pokud zaškrtnete políčko hello, hlásí Visual Studio Team Services nastaly chyby během hello verzi, i pokud všechno proběhne správně.
    >
3. Uložte tuto novou verzi definici.


>[!NOTE]
>Toto nasazení obsahuje výpadky, protože jsme zastavení služeb staré hello a systémem hello nový. Ho je možné tooavoid to pomocí tohoto postupu nasazení blue zeleně.
>

## <a name="step-4-test-hello-cicd-pipeline"></a>Krok 4. Test hello CI/CD kanálu

Teď, když jste hotovi s konfigurací hello, je čas tootest tento nový kanál CI/CD. Hello nejjednodušší způsob, jak tootest je hello tooupdate hello zdrojového kódu a provést změny do úložiště GitHub. Několik sekund poté, co push hello kód, zobrazí se nové sestavení spuštěná ve Visual Studio Team Services. Po úspěšném dokončení novou verzi se spustí a provede nasazení nové verze aplikace hello na clusteru Azure Container Service hello hello.

## <a name="next-steps"></a>Další kroky

* Další informace o CI/CD s Visual Studio Team Services najdete v tématu hello [služby VSTS sestavení přehled](https://www.visualstudio.com/docs/build/overview).
