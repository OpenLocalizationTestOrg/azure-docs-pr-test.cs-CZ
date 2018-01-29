---
title: "Nasazení aplikace Azure Service Fabric pomocí průběžnou integraci (Team Services) | Microsoft Docs"
description: "Zjistěte, jak nastavit průběžnou integraci a nasazení pro aplikace Service Fabric pomocí Visual Studio Team Services.  Nasazení aplikace do clusteru Service Fabric v Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/13/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 2fb7ab906208a58c0b5cd3af8b53188fbab94029
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Nasazení aplikace s CI nebo CD pro cluster Service Fabric
V tomto kurzu je součástí tři řady a popisuje, jak nastavit průběžnou integraci a nasazení pro aplikaci Azure Service Fabric pomocí Visual Studio Team Services.  Je potřeba stávající aplikace Service Fabric, aplikace vytvořené v [sestavení aplikace .NET](service-fabric-tutorial-create-dotnet-app.md) slouží jako příklad.

V rámci tři řady, zjistíte, jak:

> [!div class="checklist"]
> * Do projektu přidejte zdrojového kódu
> * Vytvořit definici sestavení v Team Services
> * Vytvoření definice verze v Team Services
> * Automatické nasazení a upgrade aplikace

V této série kurzu zjistíte, jak:
> [!div class="checklist"]
> * [Sestavení aplikace .NET Service Fabric](service-fabric-tutorial-create-dotnet-app.md)
> * [Nasazení aplikace na vzdálený cluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * Konfigurace CI/CD pomocí Visual Studio Team Services
> * [Nastavení monitorování a diagnostiky pro aplikaci](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte **Azure development** a **ASP.NET a webové vývoj** úlohy.
- [Instalace Service Fabric SDK](service-fabric-get-started.md)
- Vytvoření clusteru s podporou Windows Service Fabric v Azure, například pomocí [projdete tímto kurzem](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- Vytvoření [účtu Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-the-voting-sample-application"></a>Stažení ukázkové aplikace Voting
Pokud není sestavení Voting ukázkovou aplikaci [součástí jeden z této série kurz](service-fabric-tutorial-create-dotnet-app.md), můžete ho stáhnout. V příkazovém okně spusťte následující příkaz, který klonovat úložiště ukázkové aplikace do místního počítače.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Příprava profilu publikování
Teď, když jste [vytvořili aplikaci](service-fabric-tutorial-create-dotnet-app.md) a mít [nasadit aplikaci do Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md), jste připraveni nastavte průběžnou integraci.  Nejdřív připravte profil publikování v rámci vaší aplikace pro použití procesu nasazení, který spouští v rámci Team Services.  Profil publikování, musí být nakonfigurovaný pro cluster, který jste předtím vytvořili.  Spuštění sady Visual Studio a otevřít existující projekt aplikace Service Fabric.  V **Průzkumníku řešení**, klikněte pravým tlačítkem na aplikaci a vyberte **publikování...** .

Zvolte profil cíl v rámci projektu aplikace použít pro pracovní postup průběžnou integraci, například v cloudu.  Zadejte koncový bod připojení clusteru.  Zkontrolujte **upgradu aplikace** políčko tak, aby vaše aplikace upgrady pro každé nasazení v Team Services.  Klikněte **Uložit** hypertextový odkaz na Uložit nastavení do profil publikování a pak klikněte na **zrušit** zavřete dialogové okno.  

![Push profilu][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>Sdílet řešení sady Visual Studio do nového úložiště Team Services Git
Sdílejte vaše zdrojové soubory aplikací do týmového projektu v Team Services, můžete vygenerovat sestavení.  

Vytvoření nové místní úložiště Git pro svůj projekt tak, že vyberete **přidat do správy zdrojového kódu** -> **Git** na stavovém řádku v pravém dolním rohu Visual Studio. 

V **Push** zobrazit v **Team Explorer**, vyberte **publikování úložiště Git** tlačítko pod **nabízená Visual Studio Team Services**.

![Úložiště Git push][push-git-repo]

Ověřit e-mailu a vyberte svůj účet za **Team Services domény** rozevíracího seznamu. Zadejte název úložiště a vyberte **publikovat úložiště**.

![Úložiště Git push][publish-code]

Publikování úložišti vytvoří nového týmového projektu v účtu se stejným názvem jako místní úložiště. Chcete-li vytvořit úložišti v existující týmového projektu, klikněte na tlačítko **Upřesnit** vedle **úložiště** název a vyberte týmového projektu. Váš kód na webu můžete zobrazit výběrem **najdete v článku na webu**.

## <a name="configure-continuous-delivery-with-vsts"></a>Služby VSTS nakonfigurovat nastavené průběžné doručování
Definice sestavení Team Services popisuje pracovní postup, který se skládá ze sady kroky sestavení, které jsou prováděny postupně. Vytvoření definice sestavení, která vytvoří balíček aplikace Service Fabric a artefaktů, k nasazení na cluster Service Fabric. Další informace o [definice sestavení Team Services](https://www.visualstudio.com/docs/build/define/create). 

Verze definice Team Services popisuje pracovní postup, který nasadí balíček aplikace do clusteru. Při použití společně, definici sestavení a verze definice spustit celý pracovní postup od zdrojové soubory k konče spuštěné aplikace v clusteru. Další informace o Team Services [verze definice](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-build-definition"></a>Vytvořit definici sestavení
Otevřete webový prohlížeč a přejděte do nového týmového projektu v: [https://&lt;stránku Můj účet&gt;.visualstudio.com/Voting/Voting%20Team/_git/Voting](https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting). 

Vyberte **sestavení a verze** kartě pak **sestavení**, pak **+ novou definici**.  V **vyberte šablonu**, vyberte **aplikace Azure Service Fabric** šablonu a klikněte na tlačítko **použít**. 

![Výběr šablony sestavení][select-build-template] 

V **úlohy**, zadejte "Hostované VS2017" jako **fronty agenta**. 

![Vyberte úlohy][save-and-queue]

V části **aktivační události**, povolit průběžnou integraci nastavením **aktivovat stav**.  Vyberte **uložte a fronty** ruční spuštění sestavení.  

![Vyberte aktivační události][save-and-queue2]

Také vytvoří aktivační událost nabízené nebo vrácení se změnami. Chcete-li zkontrolovat průběh sestavení, přepněte na **sestavení** kartě.  Jakmile ověříte úspěšně spustí sestavení, definujte definici verze, která nasadí aplikaci do clusteru. 

### <a name="create-a-release-definition"></a>Vytvoření definice verze  

Vyberte **sestavení a verze** kartě pak **verze**, pak **+ novou definici**.  V **vyberte šablonu**, vyberte **Azure Service Fabric nasazení** šablonu ze seznamu a potom **použít**.  

![Výběr šablony verze][select-release-template]

Vyberte **úlohy**->**prostředí 1** a potom **+ nový** přidat nové připojení clusteru.

![Přidat připojení clusteru][add-cluster-connection]

V **přidat nové připojení prostředků infrastruktury služby** zobrazení vyberte **na základě certifikátů** nebo **Azure Active Directory** ověřování.  Zadejte název připojení "mysftestcluster" a koncový bod clusteru z "tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000" (nebo koncový bod clusteru, který nasazujete). 

Pro ověřování na základě certifikátů, přidejte **kryptografický otisk certifikátu serveru** certifikátu serveru se používá k vytvoření clusteru.  V **klientský certifikát**, přidejte kódování base-64 souboru certifikátu klienta. Na toto pole informace o tom, jak získat této kódování base-64 reprezentace kódovaného certifikátu zobrazit automaticky otevírané okno nápovědy. Také přidat **heslo** pro certifikát.  Certifikát serveru nebo clusteru můžete použít, pokud nemáte samostatný klientský certifikát. 

Pro přihlašovací údaje služby Azure Active Directory, přidejte **kryptografický otisk certifikátu serveru** certifikátu serveru použít k vytvoření clusteru a přihlašovací údaje chcete použít pro připojení k clusteru, **uživatelské jméno** a **heslo** pole. 

Klikněte na tlačítko **přidat** pro uložení připojení clusteru.

V dalším kroku přidáte artefaktů sestavení do kanálu, aby definice vydání, můžete najít výstup ze sestavení. Vyberte **kanálu** a **artefakty**->**+ přidat**.  V **zdroj (definice sestavení)**, vyberte definici sestavení, který jste předtím vytvořili.  Klikněte na tlačítko **přidat** pro uložení artefaktů sestavení.

![Přidat artefaktů][add-artifact]

Povolte aktivační událost průběžné nasazování tak, aby verze se automaticky vytvoří při sestavení se dokončí. Klikněte na ikonu v artefaktu, povolit aktivační událost a klikněte na **Uložit** se uložit definici verze.

![Povolit aktivační události][enable-trigger]

Vyberte **+ verze** -> **vytvořit verzi** -> **vytvořit** ručně vytvořit verze.  Ověřte, zda nasazení bylo úspěšné a aplikace běží v clusteru.  Otevřete webový prohlížeč a přejděte do [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Poznámka: verze aplikace, v tomto příkladu je "1.0.0.20170616.3". 

## <a name="commit-and-push-changes-trigger-a-release"></a>Potvrzení a doručte změny, aktivace vydání verze
K ověření, že kanál průběžnou integraci funguje kontrolou v některé změny kódu Team Services.    

Při psaní kódu, změny jsou sledovány automaticky Visual Studio. Potvrzení změn místní úložiště Git tak, že vyberete ikonu (čekajících změn![Čekající na vyřízení][pending]) ze stavového řádku v vpravo dole.

Na **změny** zobrazení v Team Exploreru, přidat zprávu s popisem aktualizace a provést změny.

![Potvrdit všechny][changes]

Vyberte ikonu nepublikované změny stavu panelu (![Nepublikováno změny][unpublished-changes]) nebo synchronizace zobrazení v Team Exploreru. Vyberte **Push** aktualizovat kódu v produktu Team Services nebo TFS.

![Doručte změny][push]

Když zavedete změny Team Services automaticky aktivuje build.  Pokud definici sestavení úspěšně dokončí, verze se automaticky vytvoří a spustí upgrade aplikace v clusteru.

Chcete-li zkontrolovat průběh sestavení, přepněte na **sestavení** kartě v **Team Explorer** v sadě Visual Studio.  Jakmile ověříte úspěšně spustí sestavení, definujte definici verze, která nasadí aplikaci do clusteru.

Ověřte, zda nasazení bylo úspěšné a aplikace běží v clusteru.  Otevřete webový prohlížeč a přejděte do [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Poznámka: verze aplikace, v tomto příkladu je "1.0.0.20170815.3".

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>Aktualizace aplikace
Proveďte změny kódu v aplikaci.  Uložte a provedení změn, podle předchozích kroků.

Po upgradu aplikace, můžete sledovat průběh upgradu v Service Fabric Exploreru:

![Service Fabric Explorer][sfx2]

Upgrade aplikace může trvat několik minut. Po dokončení upgradu bude aplikace spuštěna další verze.  V tomto příkladu "1.0.0.20170815.4".

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Do projektu přidejte zdrojového kódu
> * Vytvořit definici sestavení
> * Vytvoření definice verze
> * Automatické nasazení a upgrade aplikace

Přechodu na další kurz:
> [!div class="nextstepaction"]
> [Nastavení monitorování a diagnostiky pro aplikaci](service-fabric-tutorial-monitoring-aspnet.md) 


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue.png
[save-and-queue2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue2.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/EnableTrigger.png
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
