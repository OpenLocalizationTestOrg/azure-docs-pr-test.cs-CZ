---
title: "aaaContinuous tooAzure nasazení služby App Service | Microsoft Docs"
description: "Zjistěte, jak tooenable průběžné nasazování tooAzure služby App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 62a22cbda354fd5b0a1b9729c8c375408e75049f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-tooazure-app-service"></a>Průběžné tooAzure nasazení služby App Service
Tento kurz ukazuje, jak tooconfigure pracovního postupu průběžné nasazování pro vaše [Azure App Service] aplikace. Služby App Service integrace s BitBucket, Githubu, a [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) umožňuje průběžně pracovní postup nasazení, kde Azure vyžaduje nejnovější aktualizace hello z projektu publikované tooone z těchto služeb. Průběžné nasazování je skvělou možností pro projekty, u kterých se integruje více příspěvků nebo u kterých integrace probíhá často.

toofind out jak hello tooconfigure průběžné nasazování ručně z úložiště v cloudu nejsou uvedené pomocí portálu Azure (například [GitLab](https://gitlab.com/)), najdete v části [nastavení průběžné nasazování pomocí ruční kroky](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="overview"></a>Povolení průběžného nasazování
průběžné nasazování tooenable,

1. Publikujte úložiště obsahu toohello vaší aplikace, který se použije pro průběžné nasazování.  
    Další informace o publikování vašeho projektu toothese služby najdete v tématu [vytvořit úložiště (Githubu)], [vytvořit úložiště (BitBucket)], a [začít pracovat s služby VSTS].
2. V okně vaší aplikace nabídky v hello [portál Azure], klikněte na tlačítko **nasazení aplikace > Možnosti nasazení**. Klikněte na tlačítko **zvolit zdroj**, zvolte položku zdroj nasazení hello.  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > tooconfigure účet služby VSTS pro nasazení služby App Service najdete v tématu to [kurzu](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
   > 
   > 
3. Dokončení pracovního postupu hello autorizace.
4. V hello **zdroj nasazení** okně Zvolit hello projekt a rozvětvují toodeploy z. Až to budete mít, klikněte na **OK**.
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > Při povolování průběžného nasazování s použitím GitHubu nebo BitBucketu se zobrazí veřejné i soukromé projekty.
   > 
   > 
   
    Služby App Service vytvoří přidružení se hello vybrané úložiště, který vrátí v souborech hello z hello zadaný větve a udržuje klon úložiště pro aplikaci služby App Service. Při konfiguraci služby VSTS průběžné nasazování z hello portálu Azure, integrace hello používá hello služby App Service [modul nasazení Kudu](https://github.com/projectkudu/kudu/wiki), které již automatizuje úlohy sestavení a nasazení s každou `git push`. Není nutné tooseparately nastavit průběžné nasazování v služby VSTS. Po dokončení tohoto procesu se hello **možnosti nasazení** okně aplikace se zobrazí aktivní nasazení, která určuje nasazení proběhla úspěšně.
5. tooverify hello aplikace je úspěšně nasazená, klikněte na tlačítko hello **URL** hello horní části okna hello aplikací v hello portálu Azure.
6. tooverify, který průběžné nasazování dochází z hello úložiště podle vašeho výběru push úložiště toohello změnu. Aplikace by měl aktualizovat změny hello tooreflect krátce po dokončení hello nabízené toohello úložiště. Můžete ověřit, že má vyžádat v aktualizaci hello v hello **možnosti nasazení** okno vaší aplikace.

## <a name="VSsolution"></a>Průběžné nasazování řešení sady Visual Studio
Vkládání tooAzure řešení sady Visual Studio služby App Service je stejně jednoduché jako vkládání jednoduché index.html souboru. Hello procesu nasazení služby App Service zjednodušuje všechny hello podrobnosti, včetně závislostí NuGet obnovení a vytváření binární soubory aplikace hello. Můžete dodržujte doporučené postupy řízení hello zdroj Správa kód pouze do úložiště Git a umožní nasazení služby App Service se postará o hello rest.

Hello kroky pro vkládání vaší tooApp řešení sady Visual Studio jsou služby hello stejné jako hello [předchozí části](#overview), za předpokladu, že můžete nakonfigurovat následujícím způsobem řešení a úložiště:

* Použít hello Visual Studio zdroj ovládacího prvku možnost toogenerate `.gitignore` soubor například hello bitovou kopii níže nebo přidejte ručně `.gitignore` soubor v kořenovém úložišti s obsahu podobné toothis [.gitignore ukázka](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* Přidejte hello celé řešení directory stromu tooyour úložiště, se soubor .sln hello v kořenovém úložišti hello.

Po nastavení úložiště bez vyžádání, jak je popsáno a aplikace v Azure nakonfigurovaný pro nepřetržitou publikování z jednoho z hello online úložiště Git, můžete vyvíjet aplikace ASP.NET místně v sadě Visual Studio a průběžně nasadíte tak svůj kód jednoduše načte Když zavedete vaše změny tooyour online úložiště Git.

## <a name="disableCD"></a>Zakázání průběžného nasazování
průběžné nasazování toodisable,

1. V okně vaší aplikace nabídky v hello [portál Azure], klikněte na tlačítko **nasazení aplikace > Možnosti nasazení**. Pak klikněte na tlačítko **odpojení** v hello **možnosti nasazení** okno.
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. Po odpovídání **Ano** toohello potvrzující zpráva, se můžete vrátit okně tooyour aplikace a klikněte na tlačítko **nasazení aplikace > Možnosti nasazení** Pokud byste chtěli tooset až publikování z jiného zdroje.

## <a name="additional-resources"></a>Další zdroje
* [Jak tooinvestigate běžné problémy se průběžné nasazování.](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Jak toouse prostředí PowerShell pro Azure]
* [Jak toouse hello nástroje příkazového řádku Azure pro Mac a Linux]
* [Dokumentace pro Git]
* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)
* [Použití Azure tooautomatically generovat CI/CD kanálu toodeploy ASP.NET 4 aplikace](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/
[portál Azure]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Jak toouse prostředí PowerShell pro Azure]: /powershell/azureps-cmdlets-docs
[Jak toouse hello nástroje příkazového řádku Azure pro Mac a Linux]:../cli-install-nodejs.md
[Dokumentace pro Git]: http://git-scm.com/documentation

[vytvořit úložiště (Githubu)]: https://help.github.com/articles/create-a-repo
[vytvořit úložiště (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[začít pracovat s služby VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
