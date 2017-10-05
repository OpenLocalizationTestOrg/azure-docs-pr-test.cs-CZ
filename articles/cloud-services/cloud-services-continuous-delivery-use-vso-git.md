---
title: "Nastavené průběžné doručování s Git a Visual Studio Team Services v Azure | Microsoft Docs"
description: "Naučte se konfigurovat týmových projektech Visual Studio Team Services používat Git automaticky vytvářet a nasazovat do funkci webové aplikace v Azure App Service nebo cloudové služby."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 4b3297ef-0de6-4d5f-925c-fcdacc3085ac
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: f4f5f231536bc381d17898ff2c592be821168a65
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Průběžné doručování do Azure pomocí služby Visual Studio Team Services a Gitu
Týmové projekty Visual Studio Team Services můžete použít k hostování úložiště Git pro váš zdrojový kód a automaticky sestavení a nasazení do webové aplikace Azure nebo cloudových služeb, vždy, když push potvrzení změn do úložiště.

Budete potřebovat Visual Studio 2013 a Azure SDK nainstalována. Pokud ještě nemáte Visual Studio 2013, stáhněte ho tak, že zvolíte **začněte zadarmo** odkaz na [www.visualstudio.com](http://www.visualstudio.com). Nainstalovat sadu Azure SDK z [zde](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Účet Visual Studio Team Services k dokončení tohoto kurzu potřebujete: můžete [zdarma otevřít účet Visual Studio Team Services](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

Pokud chcete nastavit Cloudová služba pro automatické vytvoření a nasazení do Azure pomocí sady Visual Studio Team Services, postupujte podle těchto kroků.

## <a name="1-create-a-git-repository"></a>1: Vytvoření úložiště Git
1. Pokud nemáte účet Visual Studio Team Services, můžete jej získat [zde](http://go.microsoft.com/fwlink/?LinkId=397665). Když vytvoříte týmový projekt, vyberte Git jako systém správce zdrojů. Postupujte podle pokynů pro připojení k vašemu týmovému projektu sady Visual Studio.
2. V **Team Explorer**, vyberte **klonovat toto úložiště** odkaz.
   
    ![][3]
3. Zadejte umístění místní kopie a potom zvolte **klon** tlačítko.

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: vytvoření projektu a potvrďte ho do úložiště
1. V **Team Explorer**v **řešení** zvolte **nový** odkaz pro vytvoření nového projektu v místním úložišti.
   
    ![][4]
2. Podle kroků v tomto názorném postupu můžete nasadit webovou aplikaci nebo cloudové služby (aplikace Azure). Vytvořte nový projekt Azure Cloud Service, nebo nový projekt ASP.NET MVC. Přesvědčte se, že projekt cílí na rozhraní .NET Framework 4 nebo novější. Pokud vytváříte projekt cloudové služby, přidejte webová role ASP.NET MVC a roli pracovního procesu.
   Pokud chcete vytvořit webovou aplikaci, vyberte **webové aplikace ASP.NET** projektu šablony a potom vyberte **MVC**. V tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) Další informace.
3. Otevřete místní nabídku pro řešení a zvolte **potvrdit**.
   
    ![][7]
4. Pokud jste použili Git ve Visual Studio Team Services poprvé, budete muset zadat nějaké informace, které Identifikujte se v úložišti Git. V **čekající změny** oblasti **Team Explorer**, zadejte své uživatelské jméno a e-mailovou adresu. Pro potvrzení zadejte komentář a potom vyberte **potvrzení** tlačítko.
   
    ![][8]
5. Všimněte si možnosti, které chcete zahrnout nebo vyloučit konkrétní změny při se změnami. Pokud chcete změny jsou vyloučeny, zvolte **zahrnují všechny**.
6. Jste nyní potvrzena změny v místní kopii úložiště. V dalším kroku provedené změny se serverem synchronizovat tak, že zvolíte **synchronizace** odkaz.

## <a name="3-connect-the-project-to-azure"></a>3: připojení projekt do Azure
1. Teď, když máte úložiště Git v sadě Visual Studio Team Services s některé zdrojového kódu v ní, jste připraveni se připojit k Azure úložiště git.  V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), vyberte svou cloudové služby nebo webovou aplikaci nebo vytvořte novou výběrem + ikona ve spodní levé a výběr **Cloudová služba** nebo **webové aplikace** a potom **rychle vytvořit**.
   
    ![][9]
2. Cloudové služby, zvolte **nastavit publikování pomocí Visual Studio Team Services** odkaz. Pro webové aplikace, vyberte **nastavení nasazení od správy zdrojového kódu** odkaz.
   
    ![][10]
3. V průvodci, do textového pole zadejte název účtu Visual Studio Team Services a zvolte **autorizovat teď** odkaz. Můžete být požádáni o přihlášení.
   
    ![][11]
4. V **požadavku na připojení** automaticky otevíraný dialog, zvolte **přijmout** autorizace Azure ke konfiguraci týmového projektu v sadě Visual Studio Team Services.
   
    ![][12]
5. Po úspěšné ověřování, zobrazí rozevírací seznam obsahující týmových projektech Visual Studio Team Services.  Vyberte název týmového projektu, který jste vytvořili v předchozím kroku a vyberte značku zaškrtnutí v průvodci.
   
    ![][13]
   
    Při příštím push potvrzení změn do úložiště, Visual Studio Team Services bude sestavovat a nasazení projektu do Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: aktivovat opětovném sestavení a znovu nasaďte projekt
1. V sadě Visual Studio otevřete soubor a ho změnit. Můžete například změnit soubor `_Layout.cshtml` v zobrazení\\sdílené složky MVC webové role.
   
    ![][17]
2. Upravit text zápatí pro lokalitu a soubor uložte.
   
    ![][18]
3. V **Průzkumníku řešení**, otevřete místní nabídku pro řešení uzel, uzel projektu nebo změnit a potom zvolte soubor **potvrdit**.
4. Zadejte komentář a zvolte **potvrdit**.
   
    ![][20]
5. Vyberte **synchronizace** odkaz.
   
    ![][38]
6. Vyberte **nabízené** odkaz tak, aby nabízel vaše potvrzení do úložiště v sadě Visual Studio Team Services. (Můžete také **synchronizace** tlačítko chcete zkopírovat do úložiště vaše potvrzení. Rozdíl je, že **synchronizace** také vrátí nejnovějších změn z úložiště.)
   
    ![][39]
7. Zvolte **domácí** tlačítko se vrátíte do **Team Explorer** domovskou stránku.
   
    ![][21]
8. Zvolte **sestavení** Chcete-li zobrazit nastavení v průběhu.
   
    ![][22]
   
    **Team Explorer** ukazuje, že byla spuštěna sestavení pro vaše vrácení se změnami.
   
    ![][23]
9. Chcete-li zobrazit podrobné protokolu v průběhu sestavení, dvakrát klikněte na název sestavení v průběhu.
10. Při sestavení je v průběhu, podívejte se na definici sestavení, která byla vytvořena, když jste použili Průvodce propojení s Azure.  Otevřete místní nabídku pro definici sestavení a zvolte **upravit definici sestavení**.
    
    ![][25]
11. V **aktivační událost** karty, zobrazí se, že je ve výchozím nastavení na každé vrácení se změnami, definici sestavení. (Pro cloudové služby Visual Studio Team Services vytvoří a nasadí hlavní větve do fázovacího prostředí automaticky. Stále je nutné provést ruční krok k nasazení na živý web. Pro webovou aplikaci, která nemá pracovní prostředí nasadí hlavní větve přímo na živý web.
    
    ![][26]
12. V **proces** kartě uvidíte prostředí nasazení je nastavena na název cloudové služby nebo webovou aplikaci.
    
     ![][27]
13. Zadejte hodnoty vlastností, pokud chcete jiné hodnoty než výchozí hodnoty. Vlastnosti pro publikování v Azure jsou ve **nasazení** části a může být také nutné nastavit MSBuild parametry. V cloudu projekt služby, můžete zadat konfiguraci služby než "Cloud", nastavte parametry nástroje MSbuild na `/p:TargetProfile=[YourProfile]` kde *[YourProfile]* odpovídá konfigurační soubor služby s názvem, jako je objekt ServiceConfiguration. *YourProfile*.cscfg.
    
     V následující tabulce jsou uvedeny dostupné vlastnosti v **nasazení** části:
    
    | Vlastnost | Výchozí hodnota |
    | --- | --- |
    | Povolit nedůvěryhodné certifikáty |Pokud je hodnota false, musí být podepsané certifikáty SSL kořenovou autoritou. |
    | Povolit upgradu |Umožňuje nasazení k aktualizaci stávajícího nasazení místo vytvoření nové. Zachovává IP adresu. |
    | Neodstraňujte |V případě hodnoty true Nepřepisovat existující nesouvisejícími nasazení (upgrade je povolena.). |
    | Cesta k nastavení nasazení |Cesta k souboru .pubxml pro webovou aplikaci, relativně ke kořenové složce úložiště. Ignorovat pro cloudové služby. |
    | Prostředí nasazení služby SharePoint |Stejný jako název služby. |
    | Prostředí nasazení Azure |Webové aplikace nebo cloudové název služby. |
14. Buildu té doby by se měly dokončit úspěšně.
    
     ![][28]
15. Pokud dvakrát kliknete na název sestavení, Visual Studio se zobrazí **souhrnu sestavení**, včetně všech výsledků testu z přidružené projektů testování částí.
    
     ![][29]
16. V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), přidružené nasazení můžete zobrazit na **nasazení** kartě, pokud je vybrána pracovní prostředí.
    
     ![][30]
17. Přejděte na adresu URL vašeho webu. Pro webovou aplikaci, zvolte pouze **Procházet** tlačítko na portálu. Cloudové služby, vyberte adresu URL v **rychlý přehled** části **řídicí panel** stránky, která obsahuje pracovní prostředí.
    
    Nasazení z průběžnou integraci pro cloudové služby jsou publikovány do prostředí pracovní ve výchozím nastavení. Toto můžete změnit nastavením **alternativní prostředí cloudové služby** vlastnost **produkční**. Zde je, kde je adresa URL webu na stránce řídicího panelu cloudové služby.
    
    ![][31]
    
    Na nich spuštěné webu se otevře novou kartu prohlížeče.
    
    ![][32]
18. Pokud provedete další změny do projektu, aktivační události je více sestavení a budou se hromadit více nasazení. Nejnovější je označena jako aktivní.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: znovu nasaďte starší sestavení
Tento krok je volitelný. Na portálu Azure classic, zvolte k předchozímu nasazení a **znovu nasaďte** převíjení váš web, dřívější vrácení se změnami. Všimněte si, že to aktivují nového sestavení v sadě TFS a vytvořit nový záznam v historii nasazení.

![][34]

## <a name="6-change-the-production-deployment"></a>6: Změňte produkční nasazení
Jakmile budete připraveni, můžete zvýšit úroveň pracovního prostředí do produkčního prostředí, tak, že zvolíte **odkládacího souboru** na portálu Azure classic. Nově nasazené pracovní prostředí se zvýší úroveň na produkční a předchozí provozním prostředí, pokud existuje, bude pracovní prostředí. Aktivní nasazení může být různé pro produkční a pracovní prostředí, ale historii nasazení poslední sestavení je stejný bez ohledu na prostředí.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: nasazení z pracovní větev.
Při použití Git, obvykle provedete změny v pracovní větev a integrovat do hlavní větve, když váš vývojový dosáhne konečného stavu. V průběhu fáze vývoje projektu budete chtít vytvořit a nasadit pracovní větev do Azure.

1. V **Team Explorer**, vyberte **Domů** tlačítko a potom zvolte **větví** tlačítko.
   
    ![][40]
2. Vyberte **nové větve** odkaz.
   
    ![][41]
3. Zadejte název větve, jako je například "fungoval," a zvolte **vytvoření větve**. Tím se vytvoří nové místní větve.
   
    ![][42]
4. Publikujte větev. Zvolte název větve v **Nepublikováno větví**a zvolte **publikovat**.
   
    ![][44]
5. Ve výchozím nastavení pouze změny do hlavní větve aktivovat průběžné build. Chcete-li nastavit průběžné sestavení pro pracovní větev, zvolte **sestavení** stránky v **Team Explorer**a zvolte **upravit definici sestavení**.
6. Otevřete **nastavení zdroje** kartě. V části **monitorovat větve pro průběžnou integraci a sestavení**, zvolte **kliknutím sem přidejte nový řádek**.
   
    ![][47]
7. Zadejte, které jste vytvořili, například odolný systém souborů nebo oznámení nebo pracovní větev.
   
    ![][48]
8. V kódu změňte, otevřete místní nabídku pro změněný soubor a potom zvolte **potvrdit**.
   
    ![][43]
9. Vyberte **nesynchronizované potvrzení** propojit a zvolte **synchronizace** tlačítko nebo **Push** odkaz zkopírovat změny do kopie pracovní větev ve Visual Studio Team Services.
   
   ![][45]
10. Přejděte na **sestavení** zobrazení a najít sestavení, který právě získali aktivoval pro pracovní větev.

## <a name="next-steps"></a>Další kroky
Další tipy k používání Git s Visual Studio Team Services naleznete v tématu [vývoj a sdílet kódu v úložišti Git pomocí sady Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) a informace o použití úložiště Git, který není spravován nástrojem Visual Studio Team Services k publikování do Azure najdete v tématu [průběžné nasazování do služby Azure App Service](../app-service-web/app-service-continuous-deployment.md). Další informace o sadě Visual Studio Team Services najdete v tématu [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
