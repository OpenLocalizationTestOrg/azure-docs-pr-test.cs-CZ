---
title: aaaCreate aplikace .NET Service Fabric v Azure | Microsoft Docs
description: "Vytvořte aplikaci .NET pro Azure pomocí hello Service Fabric úvodní ukázka."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.openlocfilehash: 20ef88dbf9fb0def31234ef05679a19009b9b529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-net-service-fabric-application-in-azure"></a>Vytvoření aplikace .NET Service Fabric v Azure
Azure Service Fabric je platforma distribuovaných systémů pro nasazování a správu škálovatelných a spolehlivých mikroslužeb a kontejnerů. 

Tento rychlý start ukazuje, jak toodeploy vaší první aplikace tooService .NET prostředků infrastruktury. Jakmile budete hotovi, máte hlasovací aplikaci s ASP.NET Core web, který je front-end, který uloží výsledků hlasování ve stavové služby back-end v clusteru hello.

![Snímek obrazovky aplikace](./media/service-fabric-quickstart-dotnet/application-screenshot.png)

Pomocí této aplikace se dozvíte, jak:
> [!div class="checklist"]
> * Vytvoření aplikace pomocí rozhraní .NET a Service Fabric
> * Pomocí ASP.NET core jako webového front-endu
> * Ukládání dat aplikací ve stavové služby
> * Ladění aplikace místně
> * Nasazení clusteru tooa hello aplikace v Azure
> * Aplikace hello škálování mezi několika uzly
> * Provedení postupného upgradu aplikace

## <a name="prerequisites"></a>Požadavky
toocomplete tento rychlý start:
1. [Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) s hello **Azure development** a **ASP.NET a webové vývoj** úlohy.
2. [Nainstalovat Git](https://git-scm.com/).
3. [Nainstalujte hello Microsoft Azure Service Fabric SDK](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK)
4. Spusťte následující příkaz tooenable Visual Studio toodeploy toohello místní cluster Service Fabric hello:
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
    ```

## <a name="download-hello-sample"></a>Stažení ukázky hello
V příkazovém okně spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello.
```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="run-hello-application-locally"></a>Místní spuštění aplikace hello
Klikněte pravým tlačítkem na ikonu hello Visual Studio v nabídce Start hello a zvolte **spustit jako správce**. Pořadí tooattach hello ladicí program tooyour služby je nutné toorun Visual Studio jako správce.

Otevřete hello **Voting.sln** řešení sady Visual Studio z hello úložiště, které jste naklonovali.

aplikace hello toodeploy, stiskněte klávesu **F5**.

> [!NOTE]
> Hello poprvé spustíte a nasazení aplikace hello, Visual Studio vytvoří místní cluster pro ladění. Tato operace může chvíli trvat. Stav vytváření clusteru Hello se zobrazí v okně výstupu sady Visual Studio hello.

Po dokončení nasazení hello spustit prohlížeč a otevřete tuto stránku: `http://localhost:8080` -hello webového front-endu aplikace hello.

![Aplikace front-endu](./media/service-fabric-quickstart-dotnet/application-screenshot-new.png)

Teď můžete přidat sadu hlasovací tlačítka a spuštění trvá hlasy. Hello spouštět aplikace a ukládá všechna data v clusteru Service Fabric, bez nutnosti hello samostatné databáze.

## <a name="walk-through-hello-voting-sample-application"></a>Provede hello hlasování ukázkové aplikace
Hello hlasování aplikace se skládá ze dvou služeb:
- Webová služba front-endu (VotingWeb) – ASP.NET Core webových front-endové služby, která obsluhuje hello webové stránky a zpřístupňuje rozhraní API toocommunicate s hello back-end službu.
- Služba back endu (VotingData)-ASP.NET Core webová služba, která zpřístupňuje rozhraní API toostore hello hlas výsledkem slovník spolehlivé trvalé na disku.

![Diagram aplikace](./media/service-fabric-quickstart-dotnet/application-diagram.png)

Když hlasovat v hello aplikace hello následující dojít k události:
1. JavaScript odešle hello hlas požadavek toohello webové rozhraní API v hello webové front-endové služby jako požadavek HTTP PUT.

2. front-end webové Hello používá proxy toolocate a předávat k službě back-end toohello požadavek HTTP PUT.

3. Hello back endové službě trvá hello příchozího požadavku, a ukládá hello aktualizovat výsledek v spolehlivé slovník, který získá replikované toomultiple uzlů v clusteru hello a trvalé na disku. Všechny aplikace hello data se ukládají v clusteru hello, takže se žádná databáze.

## <a name="debug-in-visual-studio"></a>Ladění v sadě Visual Studio
Při ladění aplikace v sadě Visual Studio, kterou používáte místní cluster Service Fabric vývoj. Máte možnost tooadjust hello váš ladění tooyour scénář prostředí. V této aplikaci uložíme data v naší službě back-end pomocí slovník spolehlivé. Visual Studio odebere aplikaci hello za výchozí při zastavení ladicího programu hello. Odebrání aplikace hello způsobí hello data hello back-end služby tooalso odeberou. toopersist hello data mezi relace ladění, můžete změnit hello **režim ladění aplikací** jako vlastnost na hello **Voting** projektu v sadě Visual Studio.

toolook na co se stane, že v kódu hello dokončení hello následující kroky:
1. Otevřete hello **VotesController.cs** souboru a nastavit zarážky v hello webové rozhraní API **Put** – metoda (řádku 47) – můžete vyhledat hello soubor v Průzkumníku řešení v sadě Visual Studio hello.

2. Otevřete hello **VoteDataController.cs** souboru a nastavit zarážky v tomto rozhraní web API **Put** – metoda (řádku 50).

3. Vrátit toohello prohlížeče a klikněte na hlasování možnost nebo přidat novou možnost hlasování. Kliknutí na první zarážky hello do kontroleru hello webové front endové rozhraní api.
    - Toto je, kde hello JavaScript v prohlížeči hello odešle řadič požadavek toohello webové rozhraní API v hello front-endové služby.
    
    ![Přidejte Front-End služby hlas](./media/service-fabric-quickstart-dotnet/addvote-frontend.png)

    - Nejprve jsme vytvořit adresu URL toohello hello ReverseProxy pro naši službu back-end **(1)**.
    - Potom pošleme hello požadavek HTTP PUT toohello ReverseProxy **(2)**.
    - Nakonec hello vrátíme hello odpověď z klienta toohello back-end služby hello **(3)**.

4. Stiskněte klávesu **F5** toocontinue
    - Nyní jste na hello zarážce v hello back endové službě.
    
    ![Přidat hlas Back-End službu](./media/service-fabric-quickstart-dotnet/addvote-backend.png)

    - V prvním řádku hello hello metoda **(1)** používáme hello `StateManager` tooget nebo přidejte spolehlivé slovník nazývaný `counts`.
    - Všechny interakce s hodnotami ve slovníku spolehlivé vyžadují transakci, této konfigurace pomocí příkazu **(2)** vytvoří této transakce.
    - V transakci hello jsme potom aktualizujte hodnotu hello hello relevantní klíče pro hello volíte možnost a potvrzení hello operaci **(3)**. Po potvrzení hello metoda vrátí, hello dat ve slovníku hello k aktualizaci a replikované tooother uzly v clusteru hello. Hello data jsou nyní bezpečně uložena v clusteru hello a hello back-end služby při selhání tooother uzly, i nadále s hello data k dispozici.
5. Stiskněte klávesu **F5** toocontinue

hello toostop ladicí relace, stiskněte klávesu **Shift + F5**.

## <a name="deploy-hello-application-tooazure"></a>Nasazení aplikace tooAzure hello
toodeploy hello aplikace tooa clusteru v Azure, můžete buď zvolíte toocreate vlastní clusteru, nebo použijte Cluster strany.

Strany clustery jsou zdarma, časově omezené clusterů Service Fabric hostované v Azure a spusťte Service Fabric týmem hello kde každý, kdo můžete nasadit aplikace a další informace o platformě hello. tooget přístup tooa strany clusteru [postupujte podle pokynů hello](http://aka.ms/tryservicefabric). 

Informace o vytvoření vlastního clusteru najdete v tématu [Vytvoření vašeho prvního clusteru Service Fabric v Azure](service-fabric-get-started-azure-cluster.md).

> [!Note]
> Služba front-endu webové Hello je nakonfigurované toolisten na portu 8080 pro příchozí provoz. Ujistěte se, že je ve vašem clusteru tento port otevřený. Pokud používáte hello strany clusteru, tento port je otevřený.
>

### <a name="deploy-hello-application-using-visual-studio"></a>Nasazení aplikace hello pomocí sady Visual Studio
Teď, když aplikace hello je připraveno, můžete ho nasadit cluster tooa přímo ze sady Visual Studio.

1. Klikněte pravým tlačítkem na **Voting** v hello Průzkumníku řešení a zvolte **publikovat**. Otevře se dialogové okno publikování Hello.

    ![Dialogové okno Publikovat](./media/service-fabric-quickstart-dotnet/publish-app.png)

2. Typ v hello koncového bodu připojení clusteru hello v hello **koncového bodu připojení** pole a klikněte na tlačítko **publikovat**. Když zaregistrujete hello strany clusteru, je k dispozici hello koncového bodu připojení v prohlížeči hello. -například `winh1x87d1d.westus.cloudapp.azure.com:19000`.

3. Otevřete prohlížeč a zadejte adresu clusteru hello – například `http://winh1x87d1d.westus.cloudapp.azure.com`. Teď byste měli vidět hello aplikace běžící v clusteru hello v Azure.

![Aplikace front-endu](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)

## <a name="scale-applications-and-services-in-a-cluster"></a>Škálování aplikací a služeb v clusteru
Service Fabric služby je možné snadno rozšířit mezi tooaccommodate clusteru pro změnu hello zatížení služby hello. Škálování služby změnou hello počet instancí spuštěných v clusteru hello. Máte více způsobů škálování vašim službám, můžete použít skripty nebo příkazy z prostředí PowerShell nebo Service Fabric rozhraní příkazového řádku (sfctl). V tomto příkladu používáme Service Fabric Exploreru.

Service Fabric Explorer běží v na všech clusterech Service Fabric a je přístupný z prohlížeče, procházením toohello port pro správu clusterů HTTP (19080), například `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.

tooscale hello webové front-endové služby, hello následující kroky:

1. Otevřete ve vašem clusteru Service Fabric Explorer – například `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Klikněte na další toohello hello třemi tečkami (tři tečky) **fabric: / Voting/VotingWeb** uzlu v hello treeview a zvolte **škálování služby**.

    ![Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scale.png)

    Teď můžete zvolit tooscale hello počet instancí front-endu webové hello.

3. Změňte číslo hello příliš**2** a klikněte na tlačítko **škálování služby**.
4. Klikněte na hello **fabric: / Voting/VotingWeb** uzel v hello stromové zobrazení a rozbalte uzel oddílu hello (představované identifikátor GUID).

    ![Service Fabric Explorer škálování Service](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scaled-service.png)

    Nyní můžete vidět, že služba hello má dvě instance, a v zobrazení stromu hello uzlů, které instance hello spustili.

Pomocí této úlohy jednoduchou správu jsme dvojitá hello prostředky, které jsou k dispozici pro naše zatížení front-endovou službu tooprocess uživatele. Je důležité toounderstand, který není nutné více instancí služby toohave, ho spustit spolehlivě. Pokud se služby nezdaří, Service Fabric zajišťuje, že nové instance služby běží v clusteru hello.

## <a name="perform-a-rolling-application-upgrade"></a>Provedení postupného upgradu aplikace
Pokud nasazujete aplikaci tooyour nové aktualizace, Service Fabric zavede hello aktualizace bezpečným způsobem. Vrácení upgradu vám dává žádné výpadky při upgradu a také automatické vrácení zpět, musí dojít k chybám.

tooupgrade hello aplikace, hello následující:

1. Otevřete hello **Index.cshtml** souborů v sadě Visual Studio – můžete vyhledat hello soubor v Průzkumníku řešení v sadě Visual Studio hello.
2. Přidáním některé text – například změnit hello nadpis na stránce hello.
    ```html
        <div class="col-xs-8 col-xs-offset-2 text-center">
            <h2>Service Fabric Voting Sample v2</h2>
        </div>
    ```
3. Uložte soubor hello.
4. Klikněte pravým tlačítkem na **Voting** v hello Průzkumníku řešení a zvolte **publikovat**. Otevře se dialogové okno publikování Hello.
5. Klikněte na tlačítko hello **Manifest verze** tlačítko toochange hello verze hello služby a aplikace.
6. Změna hello verzi hello **kód** prvek v rámci **VotingWebPkg** příliš "2.0.0", například a klikněte na tlačítko **Uložit**.

    ![Dialogové okno Změnit verzi](./media/service-fabric-quickstart-dotnet/change-version.png)
7. V hello **publikovat aplikace Service Fabric** dialogové okno, zkontrolujte hello upgradu hello aplikace políčko a klikněte na **publikovat**.

    ![Dialogové okno publikování upgradovat nastavení](./media/service-fabric-quickstart-dotnet/upgrade-app.png)
8. Otevřete prohlížeč a vyhledejte adresu clusteru toohello na portu 19080 – například `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.
9. Klikněte na hello **aplikace** uzlu hello stromové zobrazení a potom **upgrady v průběhu** v pravém podokně hello. Zobrazí jak hello upgradu vrátí prostřednictvím hello upgradovacích domén v clusteru, ujistěte se, že každá doména, je v pořádku před pokračováním toohello Další.
    ![Upgrade zobrazení v Service Fabric Exploreru](./media/service-fabric-quickstart-dotnet/upgrading.png)

    Service Fabric lze upgrady bezpečné tím, že po upgradu hello služby v každém uzlu v clusteru hello dvě minuty. Očekávejte tootake celý aktualizace hello přibližně osm minut.

10. Při upgradu hello běží, můžete dál používat aplikace hello. Vzhledem k tomu, že máte dvě instance služby hello spuštěna v clusteru hello, některé z vašich žádosti o může získat upgradovanou verzi produktu aplikace hello, zatímco jiné může stále dojít k předchozí verzi aplikace hello.

## <a name="next-steps"></a>Další kroky
V tomto rychlém startu jste se naučili:

> [!div class="checklist"]
> * Vytvoření aplikace pomocí rozhraní .NET a Service Fabric
> * Pomocí ASP.NET core jako webového front-endu
> * Ukládání dat aplikací ve stavové služby
> * Ladění aplikace místně
> * Nasazení clusteru tooa hello aplikace v Azure
> * Aplikace hello škálování mezi několika uzly
> * Provedení postupného upgradu aplikace

toolearn informace o Service Fabric a rozhraní .NET, podívejte se na tomto kurzu:
> [!div class="nextstepaction"]
> [Aplikace .NET v Service Fabric](service-fabric-tutorial-create-dotnet-app.md)