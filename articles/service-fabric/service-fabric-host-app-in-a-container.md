---
title: aaaDeploy aplikace .NET v kontejneru tooAzure Service Fabric | Microsoft Docs
description: "Se naučíte, jak toopackage aplikace .NET v sadě Visual Studio v kontejner Docker. Tuto novou aplikaci \"kontejner\" se pak nasadí cluster Service Fabric tooa."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.openlocfilehash: 094b0e71d76b2e56ffb9b23638dd8154b3aff5fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-net-application-in-a-windows-container-tooazure-service-fabric"></a>Nasazení aplikace .NET v kontejneru tooAzure Windows Service Fabric

Tento kurz ukazuje, jak toodeploy stávající aplikaci ASP.NET v kontejneru systému Windows v Azure.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření projektu Docker v sadě Visual Studio
> * Containerize stávající aplikaci
> * Instalační program průběžnou integraci s Visual Studio a služby VSTS

## <a name="prerequisites"></a>Požadavky

1. Nainstalujte [CE Docker pro systém Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) tak, aby kontejnery můžete spustit na Windows 10.
2. Seznamte se s hello [rychlý start pro Windows 10 kontejnery][link-container-quickstart].
3. Stáhnout hello [Fabrikam Fiber CallCenter] [ link-fabrikam-github] ukázkové aplikace.
4. Nainstalujte [prostředí Azure PowerShell][link-azure-powershell-install]
5. Nainstalujte hello [rozšíření průběžné doručování nástrojů pro Visual Studio 2017][link-visualstudio-cd-extension]
6. Vytvoření [předplatného Azure] [ link-azure-subscription] a [účet Visual Studio Team Services][link-vsts-account]. 
7. [Vytvoření clusteru s podporou v Azure](service-fabric-tutorial-create-cluster-azure-ps.md)

## <a name="containerize-hello-application"></a>Containerize aplikace hello

Teď, když máte [cluster Service Fabric běží v Azure](service-fabric-tutorial-create-cluster-azure-ps.md) jsou připravené toocreate a nasadit kontejnerizované aplikaci. toostart spuštění naše aplikace v kontejneru, potřebujeme tooadd **Docker podporu** toohello projektu v sadě Visual Studio. Když přidáte **Docker podporu** dojít dvě věci toohello aplikace. První, _soubor Docker_ je přidána toohello projektu. Tohoto nového souboru popisuje, jak hello kontejneru image je toobe vytvořené. Potom druhé, nový _docker compose_ projekt je přidán toohello řešení. Hello nový projekt obsahuje několik docker compose soubory. Docker compose soubory mohou být použité toodescribe jak spusťte kontejner hello.

Další informace o práci s [kontejner nástroje sady Visual Studio][link-visualstudio-container-tools].

>[!NOTE]
>Pokud je hello prvním používáte bitových kopií kontejneru systému Windows v počítači, musí Docker CE stahují hello základní Image pro kontejnerů. Hello obrázků použitých v tomto kurzu jsou 14 GB. Pokračujte a spusťte následující příkaz terminálu toopull hello základní Image hello:
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a>Přidání podpory Docker

Otevřete hello [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] souborů v sadě Visual Studio.

Klikněte pravým tlačítkem na hello **FabrikamFiber.Web** Projekt > **přidat** > **Docker podporu**.

### <a name="add-support-for-sql"></a>Přidání podpory pro SQL

Tato aplikace používá jako hello zprostředkovatele dat, SQL, tak, aby SQL server vyžaduje toorun hello aplikace. Referenční bitovou kopii systému SQL Server kontejneru v našem docker compose.override.yml souboru.

V sadě Visual Studio otevřete **Průzkumníku řešení**, Najít **docker compose**a soubor otevřít hello **docker compose.override.yml**.

Přejděte toohello `services:` uzlu přidat uzel s názvem `db:` položka systému SQL Server hello hello kontejneru, který definuje.

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
>Jakýkoli Server SQL dáváte přednost pro místní ladění, můžete použít, dokud je dostupný z vašeho hostitele. Ale **localdb** nepodporuje `container -> host` komunikace.

>[!WARNING]
>Zachování dat není podporován systémem SQL Server v kontejneru. Když se zastaví kontejner hello, data budou vymazána. Nepoužívejte tuto konfiguraci pro produkční prostředí.

Přejděte toohello `fabrikamfiber.web:` uzel a přidat podřízený uzel s názvem `depends_on:`. To zajistí, že hello `db` (kontejner systému SQL Server hello) bude spuštěn před naše webové aplikace (fabrikamfiber.web).

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-hello-web-config"></a>Aktualizovat hello webové konfigurace

Zpět v hello **FabrikamFiber.Web** projektu, aktualizace hello připojovací řetězec v hello **web.config** souboru toopoint toohello systému SQL Server v kontejneru hello.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Pokud chcete, aby toouse jiný Server SQL při sestavování verze sestavení webové aplikace, přidejte jiný soubor web.release.config tooyour připojovacího řetězce.

### <a name="test-your-container"></a>Vaše kontejner testů

Stiskněte klávesu **F5** toorun a ladění aplikace hello v vašeho kontejneru.

Hraniční otevře se stránka definované spuštění vaší aplikace pomocí IP adresy hello hello kontejneru na hello interní NAT síti (obvykle 172.x.x.x). toolearn Další informace o ladění aplikací v kontejnerech pomocí Visual Studio 2017, najdete v části [v tomto článku][link-debug-container].

![třeba společnost fabrikam v kontejneru][image-web-preview]

kontejner Hello je nyní připraven toobe vytvořené a zabalené aplikace Service Fabric. Jakmile máte hello kontejneru bitové kopie vytvořené v počítači, můžete poslat ho tooany kontejneru registru a stahují toorun tooany hostitele.

## <a name="get-hello-application-ready-for-hello-cloud"></a>Příprava aplikace hello hello cloudu

aplikace hello tooget připraven ke spuštění v Service Fabric v Azure, potřebujeme toocomplete dva kroky:

1. Vystavení hello portu, kde Chceme mít tooreach toobe, naše webové aplikace v clusteru Service Fabric hello.
2. Zadejte databázi SQL připravené pro produkční pro naši aplikaci.

### <a name="expose-hello-port-for-hello-app"></a>Vystavení hello port pro aplikace hello
cluster Service Fabric Hello jsme nakonfigurovali, má port *80* otevřete ve výchozím nastavení v hello nástroj pro vyrovnávání zatížení Azure, který vyrovnává příchozí provoz toohello clusteru. Naše kontejneru na port jsme můžou zpřístupnit prostřednictvím našich soubor docker-compose.yml.

V sadě Visual Studio otevřete **Průzkumníku řešení**, Najít **docker compose**a soubor otevřít hello **docker compose.override.yml**.

Upravit hello `fabrikamfiber.web:` uzlu přidat podřízený uzel s názvem `ports:`.

Přidat položku řetězec `- "80:80"`.

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a>Použít provozní databáze SQL
Při spuštění v produkčním prostředí, potřebujeme naše data v našem databáze. Aktuálně neexistují žádná data trvalé tooguarantee způsob, jak v kontejneru, proto nelze uložit provozních dat v systému SQL Server v kontejneru.

Doporučujeme, abyste že využívat Azure SQL Database. tooset nahoru a spusťte na spravovaný Server SQL Azure, navštivte hello [Azure SQL Database – elementy QuickStart] [ link-azure-sql] článku.

>[!NOTE]
>Mějte na paměti, toochange hello připojovací řetězce toohello SQL server v hello **web.release.config** souboru v hello **FabrikamFiber.Web** projektu.
>
>Tato aplikace se elegantně selže, pokud žádné databáze SQL je dostupný. Můžete zvolit toogo dopředu a nasadit aplikace hello s žádné systému SQL server.

## <a name="deploy-with-visual-studio-team-services"></a>Nasazení s Visual Studio Team Services

tooset až nasazení pomocí sady Visual Studio Team Services, budete potřebovat tooinstall hello [rozšíření průběžné doručování nástrojů pro Visual Studio 2017][link-visualstudio-cd-extension]. Toto rozšíření umožňuje snadno toodeploy tooAzure nakonfigurováním Visual Studio Team Services a získat cluster Service Fabric tooyour nasazené aplikace.

tooget spuštění kódu musí být hostované ve správě zdrojového kódu. Hello zbývající část tohoto oddílu předpokládá **git** je používán.

### <a name="set-up-a-vsts-repo"></a>Nastavení služby VSTS úložišti
V pravém dolním rohu hello sady Visual Studio, klikněte na položku **přidat tooSource řízení** > **Git** (nebo obě tyto možnosti dáváte přednost).

![tlačítko hello zdroj ovládacího prvku][image-source-control]

V hello _Team Explorer_ podokně, stiskněte klávesu **publikování úložiště Git**.

Vyberte název služby VSTS úložiště a stiskněte klávesu **úložiště**.

![publikování tooVSTS úložišti][image-publish-repo]

Teď, když kód synchronizována s zdrojové úložiště služby VSTS, můžete nakonfigurovat průběžnou integraci a nastavené průběžné doručování.

### <a name="setup-continuous-delivery"></a>Instalační program nastavené průběžné doručování

V _Průzkumníku řešení_, klikněte pravým tlačítkem na hello **řešení** > **konfigurace nastavené průběžné doručování**.

Vyberte hello předplatné Azure.

Nastavit **typ hostitele** příliš**Service Fabric Cluster**.

Nastavit **cílového hostitele** cluster service fabric toohello jste vytvořili v předchozí části hello.

Vyberte **kontejneru registru** toopublish kontejner tak, aby.

>[!TIP]
>Použití hello **upravit** tlačítko toocreate registru kontejneru.

Stiskněte **OK**.

![instalace služby fabric průběžnou integraci][image-setup-ci]
   
   Po dokončení konfigurace hello vaší kontejner je nasazené tooService prostředků infrastruktury. Vždy, když nabízená aktualizace toohello úložiště je provést nové sestavení a verze.
   
   >[!NOTE]
   >Vytváření hello kontejneru Image trvat přibližně 15 minut.
   >cluster Service Fabric toohello první nasazení Hello způsobí, že hello základní jádro systému Windows Server kontejneru bitové kopie toobe stáhli. stažení Hello trvá toocomplete dalších 5 až 10 minut.

Procházet toohello Fabrikam Call Center aplikace pomocí adresy url hello clusteru: například *http://mycluster.westeurope.cloudapp.azure.com*

Teď, když máte kontejnerizované a nasadit řešení hello Fabrikam Call Center, můžete otevřít hello [portál Azure] [ link-azure-portal] a zobrazit hello aplikace běžící v Service Fabric. aplikace hello tootry, otevřete webový prohlížeč a přejděte toohello adresu URL clusteru Service Fabric.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření projektu Docker v sadě Visual Studio
> * Containerize stávající aplikaci
> * Instalační program průběžnou integraci s Visual Studio a služby VSTS

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance toohello next tutorial toolearn how toobind a custom SSL certificate tooit.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate tooAzure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
