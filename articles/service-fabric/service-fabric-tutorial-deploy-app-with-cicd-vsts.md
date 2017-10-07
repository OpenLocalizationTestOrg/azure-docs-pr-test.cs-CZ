---
title: "aaaDeploy aplikace Azure Service Fabric pomocí průběžnou integraci (Team Services) | Microsoft Docs"
description: "Zjistěte, jak tooset průběžnou integraci a nasazení pro aplikace Service Fabric pomocí Visual Studio Team Services.  Nasazení clusteru Service Fabric tooa aplikace v Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi
ms.openlocfilehash: ba9a632b247b0f467e7b66fbe77b4ad54fb3d9ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-with-cicd-tooa-service-fabric-cluster"></a>Nasazení aplikace se cluster Service Fabric tooa CI/CD
V tomto kurzu je součástí tři řady a popisuje, jak tooset průběžnou integraci a nasazení pro aplikaci Azure Service Fabric pomocí Visual Studio Team Services.  Je potřeba stávající aplikace Service Fabric, hello aplikace vytvořené v [sestavení aplikace .NET](service-fabric-tutorial-create-dotnet-app.md) slouží jako příklad.

V rámci tři řady hello, zjistíte, jak:

> [!div class="checklist"]
> * Přidat projekt tooyour správy zdrojů
> * Vytvořit definici sestavení v Team Services
> * Vytvoření definice verze v Team Services
> * Automatické nasazení a upgrade aplikace

V této série kurzu zjistíte, jak:
> [!div class="checklist"]
> * [Sestavení aplikace .NET Service Fabric](service-fabric-tutorial-create-dotnet-app.md)
> * [Nasazení vzdáleného clusteru tooa hello aplikace](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * Konfigurace CI/CD pomocí Visual Studio Team Services

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte hello **Azure development** a **ASP.NET a webové vývoj** úlohy.
- [Nainstalujte hello Service Fabric SDK](service-fabric-get-started.md)
- Vytvoření aplikace Service Fabric, například pomocí [projdete tímto kurzem](service-fabric-tutorial-create-dotnet-app.md). 
- Vytvoření clusteru s podporou Windows Service Fabric v Azure, například pomocí [projdete tímto kurzem](service-fabric-tutorial-create-cluster-azure-ps.md)
- Vytvoření [účtu Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-hello-voting-sample-application"></a>Stáhněte si ukázkovou aplikaci Voting hello
Pokud není sestavení hello Voting ukázkovou aplikaci [součástí jeden z této série kurz](service-fabric-tutorial-create-dotnet-app.md), můžete ho stáhnout. V příkazovém okně spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Příprava profilu publikování
Teď, když jste [vytvořili aplikaci](service-fabric-tutorial-create-dotnet-app.md) a mít [nasazené aplikace tooAzure hello](service-fabric-tutorial-deploy-app-to-party-cluster.md), jste připravené tooset až průběžnou integraci.  Nejdřív připravte profil publikování v rámci vaší aplikace pro použití hello procesu nasazení, který spouští v rámci Team Services.  profil publikování se Hello by měla být nakonfigurované tootarget hello clusteru, který jste předtím vytvořili.  Spuštění sady Visual Studio a otevřít existující projekt aplikace Service Fabric.  V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello aplikace a vyberte **publikování...** .

Zvolte profil cílového v rámci vaší toouse projekt aplikace pro pracovní postup průběžnou integraci, například v cloudu.  Zadejte hello koncového bodu připojení clusteru.  Zkontrolujte hello **upgradu hello aplikace** políčko tak, aby vaše aplikace upgrady pro každé nasazení v Team Services.  Klikněte na tlačítko hello **Uložit** hypertextový odkaz toosave hello nastavení toohello profil publikování a pak klikněte na **zrušit** dialogové okno tooclose hello.  

![Push profilu][publish-app-profile]

## <a name="share-your-visual-studio-solution-tooa-new-team-services-git-repo"></a>Sdílet vaše sady Visual Studio řešení tooa nové Team Services úložiště Git
Sdílejte zdrojových souborech aplikace, které tooa týmového projektu v Team Services, můžete vygenerovat sestavení.  

Vytvoření nové místní úložiště Git pro svůj projekt tak, že vyberete **přidat tooSource řízení** -> **Git** na stavovém řádku hello v hello pravém dolním rohu Visual Studio. 

V hello **Push** zobrazit v **Team Explorer**, vyberte hello **publikování úložiště Git** tlačítko pod **Push tooVisual Studio Team Services**.

![Úložiště Git push][push-git-repo]

Ověřit e-mailu a vyberte svůj účet v hello **Team Services domény** rozevíracího seznamu. Zadejte název úložiště a vyberte **publikovat úložiště**.

![Úložiště Git push][publish-code]

Ve vašem účtu s hello stejný název jako místní úložiště hello publikování hello úložišti vytvoří nového týmového projektu. toocreate hello úložišti v existující týmového projektu, klikněte na tlačítko **Upřesnit** další příliš**úložiště** název a vyberte týmového projektu. Váš kód můžete zobrazit na webu hello výběrem **najdete v článku na webu hello**.

## <a name="configure-continuous-delivery-with-vsts"></a>Služby VSTS nakonfigurovat nastavené průběžné doručování
Definice sestavení Team Services popisuje pracovní postup, který se skládá ze sady kroky sestavení, které jsou prováděny postupně. Vytvoření definice sestavení, která vytvoří balíček aplikace Service Fabric a další artefakty, cluster Service Fabric tooa toodeploy. Další informace o [definice sestavení Team Services](https://www.visualstudio.com/docs/build/define/create). 

Verze definice Team Services popisuje pracovní postup, který nasadí cluster tooa balíček služby aplikací. Při použití společně, hello sestavení definice a verze definice provést hello celý pracovní postup, počínaje tooending zdrojové soubory s běžící aplikací v clusteru. Další informace o Team Services [verze definice](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-build-definition"></a>Vytvořit definici sestavení
Otevřete webový prohlížeč a přejděte tooyour nového týmového projektu v: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting. 

Vyberte hello **sestavení a verze** kartě pak **sestavení**, pak **+ novou definici**.  V **vyberte šablonu**, vyberte hello **aplikace Azure Service Fabric** šablonu a klikněte na tlačítko **použít**. 

![Výběr šablony sestavení][select-build-template] 

Hello hlasování aplikace obsahuje aplikace .NET Core, proto přidat úloha, která obnoví hello závislosti. V hello **úlohy** zobrazit, vyberte možnost **+ přidat úloha** hello dole vlevo. Hledání "Příkazový řádek" toofind hello úlohu příkazového řádku a potom klikněte na **přidat**. 

![Přidat úloha][add-task] 

V hello novou úlohu, zadejte "Spustit dotnet.exe" v **zobrazovaný název**, "dotnet.exe" v **nástroj**a "obnovení" v **argumenty**. 

![Nová úloha][new-task] 

V hello **aktivační události** zobrazit, klikněte na tlačítko hello **povolení této aktivační události** přepínače v rámci **průběžnou integraci**. 

Vyberte **Uložit & fronty** a zadejte "Hostované VS2017" jako hello **fronty agenta**. Vyberte **fronty** toomanually spuštění sestavení.  Vytvoří také aktivační události nabízené nebo vrácení se změnami.

toocheck sestavení průběh, přepínač toohello **sestavení** kartě.  Po ověření úspěšně spustí hello sestavení, zadejte definici verze, která nasadí cluster tooa aplikace. 

### <a name="create-a-release-definition"></a>Vytvoření definice verze  

Vyberte hello **sestavení a verze** kartě pak **verze**, pak **+ novou definici**.  V **vytvořit verze definice**, vyberte hello **Azure Service Fabric nasazení** šablonu ze seznamu hello a klikněte na tlačítko **Další**.  Vyberte hello **sestavení** zdroje, zkontrolujte hello **průběžné nasazování** pole a klikněte na tlačítko **vytvořit**. 

V hello **prostředí** zobrazit, klikněte na tlačítko **přidat** toohello napravo od **připojení clusteru**.  Zadejte název připojení "mysftestcluster", "tcp://mysftestcluster.westus.cloudapp.azure.com:19000" a hello Azure Active Directory nebo přihlašovací údaje certifikátu koncový bod clusteru pro hello cluster. U přihlašovacích údajů Azure Active Directory, definovat hello pověření, které chcete cluster toohello tooconnect toouse v hello **uživatelské jméno** a **heslo** pole. Pro ověřování pomocí certifikátů, zadejte hello Base64 kódování souboru certifikátu klienta hello v hello **klientský certifikát** pole.  Zobrazit automaticky otevírané okno nápovědy hello na informace o tom, že pole tooget, která hodnota.  Pokud váš certifikát je chráněný heslem, zadejte heslo hello v hello **heslo** pole.  Klikněte na tlačítko **Uložit** toosave hello verze definice.

![Přidat připojení clusteru][add-cluster-connection] 

Klikněte na tlačítko **spustit na agentovi**, pak vyberte **hostované VS2017** pro **nasazení fronty**. Klikněte na tlačítko **Uložit** toosave hello verze definice.

![Spustit u agenta][run-on-agent]

Vyberte **+ verze** -> **vytvořit verzi** -> **vytvořit** toomanually vytvořit verzi.  Ověřte, zda text hello nasazení bylo úspěšné a hello aplikace běží v clusteru hello.  Otevřete webový prohlížeč a přejděte příliš[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Poznámka: verze aplikace hello, v tomto příkladu je "1.0.0.20170616.3". 

## <a name="commit-and-push-changes-trigger-a-release"></a>Potvrzení a doručte změny, aktivace vydání verze
v některých změn kódu kontrolou tooTeam služby funguje tooverify, který hello průběžnou integraci kanálu.    

Při psaní kódu, změny jsou sledovány automaticky Visual Studio. Potvrdit změny tooyour místní úložiště Git tak, že vyberete hello čekající změny ikonu (![Čekající na vyřízení][pending]) z hello stavového řádku v hello vpravo dole.

Na hello **změny** zobrazení v Team Exploreru, přidat zprávu s popisem aktualizace a provést změny.

![Potvrdit všechny][changes]

Ikona vyberte hello nepublikované změny stavového řádku (![Nepublikováno změny][unpublished-changes]) nebo hello synchronizace zobrazení v Team Exploreru. Vyberte **Push** tooupdate kódu v produktu Team Services nebo TFS.

![Doručte změny][push]

Když zavedete hello změny tooTeam služby automaticky aktivační události sestavení.  Pokud definici sestavení hello úspěšně dokončí, verze se automaticky vytvoří a spustí upgrade hello aplikace v clusteru hello.

toocheck sestavení průběh, přepínač toohello **sestavení** kartě v **Team Explorer** v sadě Visual Studio.  Po ověření úspěšně spustí hello sestavení, zadejte definici verze, která nasadí cluster tooa aplikace.

Ověřte, zda text hello nasazení bylo úspěšné a hello aplikace běží v clusteru hello.  Otevřete webový prohlížeč a přejděte příliš[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Poznámka: verze aplikace hello, v tomto příkladu je "1.0.0.20170815.3".

![Service Fabric Explorer][sfx1]

## <a name="update-hello-application"></a>Aktualizovat aplikaci hello
Proveďte změny kódu v aplikaci hello.  Uložte a provedení změn hello, hello předchozích kroků.

Jakmile hello upgrade aplikace hello začne, můžete sledovat průběh upgradu hello v Service Fabric Exploreru:

![Service Fabric Explorer][sfx2]

upgrade aplikace Hello může trvat několik minut. Po dokončení upgradu hello hello bude spuštěna aplikace hello další verze.  V tomto příkladu "1.0.0.20170815.4".

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Přidat projekt tooyour správy zdrojů
> * Vytvořit definici sestavení
> * Vytvoření definice verze
> * Automatické nasazení a upgrade aplikace

Teď, když máte nasazené aplikace a nakonfigurovali průběžnou integraci, zkuste následující hello:
- [Upgrade aplikace](service-fabric-application-upgrade.md)
- [Testování aplikace](service-fabric-testability-overview.md) 
- [Monitorování a Diagnostika](service-fabric-diagnostics-overview.md)


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[add-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddTask.png
[new-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewTask.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
[run-on-agent]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/RunOnAgent.png