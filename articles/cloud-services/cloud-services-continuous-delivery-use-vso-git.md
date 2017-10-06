---
title: "doručení aaaContinuous s Git a Visual Studio Team Services v Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Visual Studio Team Services týmové projekty toouse Git tooautomatically sestavení a nasazení funkce toohello webové aplikace v Azure App Service nebo cloudové služby."
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
ms.openlocfilehash: 936c42194f45be55597a77f9a3a6deb4480ed94b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services-and-git"></a>Nastavené průběžné doručování tooAzure pomocí Visual Studio Team Services a Git
Můžete použít Visual Studio Team Services týmové projekty toohost úložiště Git pro zdrojový kód a automaticky sestavení a nasadit tooAzure webové aplikace nebo cloudové služby vždy, když push úložiště toohello potvrzení.

Budete potřebovat Visual Studio 2013 a Azure SDK nainstalovat hello. Pokud ještě nemáte Visual Studio 2013, stáhněte ho tak, že zvolíte hello **začněte zadarmo** odkaz na [www.visualstudio.com](http://www.visualstudio.com). Instalace hello Azure SDK z [zde](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Budete potřebovat Visual Studio Team Services toocomplete účet v tomto kurzu: můžete [zdarma otevřít účet Visual Studio Team Services](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

tooset nahoru cloudové služby tooautomatically sestavení a nasazení tooAzure pomocí Visual Studio Team Services, postupujte podle těchto kroků.

## <a name="1-create-a-git-repository"></a>1: Vytvoření úložiště Git
1. Pokud nemáte účet Visual Studio Team Services, můžete jej získat [zde](http://go.microsoft.com/fwlink/?LinkId=397665). Když vytvoříte týmový projekt, vyberte Git jako systém správce zdrojů. Postupujte podle hello pokyny tooconnect Visual Studio tooyour týmového projektu.
2. V **Team Explorer**, zvolte hello **klonovat toto úložiště** odkaz.
   
    ![][3]
3. Zadejte umístění hello hello místní kopie a potom zvolte hello **klon** tlačítko.

## <a name="2-create-a-project-and-commit-it-toohello-repository"></a>2: Vytvořte projekt a potvrdit ho toohello úložiště
1. V **Team Explorer**, v hello **řešení** zvolte hello **nový** odkaz toocreate nový projekt v místním úložišti hello.
   
    ![][4]
2. Můžete nasadit webovou aplikaci nebo cloudové služby (aplikace Azure) podle následujících hello kroky v tomto návodu. Vytvořte nový projekt Azure Cloud Service, nebo nový projekt ASP.NET MVC. Přesvědčte se, že cíle projektu hello hello rozhraní .NET Framework 4 nebo novější. Pokud vytváříte projekt cloudové služby, přidejte webová role ASP.NET MVC a roli pracovního procesu.
   Pokud chcete, aby toocreate webovou aplikaci, vyberte hello **webové aplikace ASP.NET** projektu šablony a potom vyberte **MVC**. V tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) Další informace.
3. Otevřete hello místní nabídku pro hello řešení a zvolte **potvrdit**.
   
    ![][7]
4. Pokud je to hello poprvé Git jste používali ve Visual Studio Team Services, budete potřebovat tooprovide některé informace tooidentify sami v úložišti Git. V hello **čekající změny** oblasti **Team Explorer**, zadejte své uživatelské jméno a e-mailovou adresu. Potvrzení hello zadejte komentář a potom vyberte hello **potvrzení** tlačítko.
   
    ![][8]
5. Poznámka: možnosti tooinclude hello nebo vyloučit konkrétní změny při se změnami. Pokud hello změny, které chcete jsou vyloučeny, zvolte **zahrnují všechny**.
6. Jste nyní hello potvrdit změny v místní kopii hello úložiště. V dalším kroku provedené změny synchronizovat s hello server tak, že zvolíte hello **synchronizace** odkaz.

## <a name="3-connect-hello-project-tooazure"></a>3: připojení tooAzure projektu hello
1. Teď, když máte úložiště Git v sadě Visual Studio Team Services s některé zdrojového kódu v ní, se připravené tooconnect vaše tooAzure úložiště git.  V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), vyberte svou cloudové služby nebo webovou aplikaci nebo vytvořte novou výběrem hello + ikonu v dolní hello vlevo a výběr **Cloudová služba** nebo **webové aplikace**a potom **rychle vytvořit**.
   
    ![][9]
2. Pro cloudové služby, zvolte hello **nastavit publikování pomocí Visual Studio Team Services** odkaz. Pro webové aplikace, vyberte hello **nastavení nasazení od správy zdrojového kódu** odkaz.
   
    ![][10]
3. V Průvodci hello, zadejte název účtu Visual Studio Team Services hello v textovém poli hello a zvolte hello **autorizovat teď** odkaz. Můžete být požádáni toosign v.
   
    ![][11]
4. V hello **požadavku na připojení** automaticky otevíraný dialog, zvolte **přijmout** tooauthorize Azure tooconfigure váš tým projektu ve Visual Studio Team Services.
   
    ![][12]
5. Po úspěšné ověřování, zobrazí rozevírací seznam obsahující týmových projektech Visual Studio Team Services.  Vyberte název týmového projektu, který jste vytvořili v předchozích krocích hello hello a zvolte tlačítko zaškrtnutí hello průvodce.
   
    ![][13]
   
    Hello příštím push potvrzení tooyour úložiště, Visual Studio Team Services bude sestavovat a nasadit tooAzure vašeho projektu.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: aktivovat opětovném sestavení a znovu nasaďte projekt
1. V sadě Visual Studio otevřete soubor a ho změnit. Můžete například změnit soubor hello `_Layout.cshtml` pod zobrazení hello\\sdílené složky MVC webové role.
   
    ![][17]
2. Upravit text zápatí hello hello lokality a uložte soubor hello.
   
    ![][18]
3. V **Průzkumníku řešení**, otevřete hello místní nabídky pro hello řešení uzel, uzel projektu nebo hello souborů můžete změnit a potom zvolte **potvrdit**.
4. Zadejte komentář a zvolte **potvrdit**.
   
    ![][20]
5. Zvolte hello **synchronizace** odkaz.
   
    ![][38]
6. Zvolte hello **Push** odkaz toopush úložiště toohello potvrzení ve Visual Studio Team Services. (Můžete použít také hello **synchronizace** tlačítko toocopy úložiště toohello potvrzení. Hello rozdílem je, že **synchronizace** také si hello nejnovějších změn z hello úložiště.)
   
    ![][39]
7. Zvolte hello **domácí** tlačítko tooreturn toohello **Team Explorer** domovské stránky.
   
    ![][21]
8. Zvolte **sestavení** tooview hello sestavení v průběhu.
   
    ![][22]
   
    **Team Explorer** ukazuje, že byla spuštěna sestavení pro vaše vrácení se změnami.
   
    ![][23]
9. tooview podrobný protokol jako hello sestavení postupuje, dvakrát klikněte na název hello hello sestavení v průběhu.
10. Probíhá při sestavení hello prohlédněte hello definice sestavení, která byla vytvořena, když jste použili tooAzure toolink Průvodce hello.  Otevřete hello místní nabídku pro definici sestavení hello a zvolte **upravit definici sestavení**.
    
    ![][25]
11. V hello **aktivační událost** karty, zobrazí se, že definici sestavení hello toobuild na každý vrácení se změnami, ve výchozím nastavení. (Pro cloudové služby Visual Studio Team Services vytvoří a nasadí hello hlavní větve toohello pracovní prostředí automaticky. Musíte ještě toodo živý web manuální krok toodeploy toohello. Pro webovou aplikaci, která nemá pracovní prostředí nasazení hlavní větve hello přímo toohello live lokality.
    
    ![][26]
12. V hello **proces** kartě uvidíte prostředí nasazení hello nastavena toohello název cloudové služby nebo webovou aplikaci.
    
     ![][27]
13. Zadejte hodnoty pro vlastnosti hello, pokud chcete jiné hodnoty než výchozí hello. Hello vlastnosti pro publikování v Azure jsou ve hello **nasazení** části a může být také nutné tooset MSBuild parametry. Například v projektu cloudové služby, toospecify konfigurace služby než "Cloud", nastavte parametry nástroje MSbuild hello příliš`/p:TargetProfile=[YourProfile]` kde *[YourProfile]* odpovídá konfigurační soubor služby s názvem, jako je Objekt ServiceConfiguration. *YourProfile*.cscfg.
    
     Hello následující tabulce jsou uvedeny dostupné vlastnosti hello v hello **nasazení** části:
    
    | Vlastnost | Výchozí hodnota |
    | --- | --- |
    | Povolit nedůvěryhodné certifikáty |Pokud je hodnota false, musí být podepsané certifikáty SSL kořenovou autoritou. |
    | Povolit upgradu |Umožňuje nasazení tooupdate hello stávajícího nasazení místo vytvoření nové. Zachovává hello IP adresu. |
    | Neodstraňujte |V případě hodnoty true Nepřepisovat existující nesouvisejícími nasazení (upgrade je povolena.). |
    | Cesta tooDeployment nastavení |Hello cesta tooyour .pubxml soubor pro webovou aplikaci, relativní toohello kořenové složky hello úložišti. Ignorovat pro cloudové služby. |
    | Prostředí nasazení služby SharePoint |Hello stejný jako název služby hello. |
    | Prostředí nasazení Azure |Hello aplikace nebo cloudové název webové služby. |
14. Buildu té doby by se měly dokončit úspěšně.
    
     ![][28]
15. Pokud dvakrát kliknete na název sestavení hello Visual Studio se zobrazí **souhrnu sestavení**, včetně všech výsledků testu z přidružené projektů testování částí.
    
     ![][29]
16. V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), hello přidružené nasazení můžete zobrazit na hello **nasazení** kartě, pokud je vybrána hello pracovní prostředí.
    
     ![][30]
17. Procházejte tooyour adresy URL. Pro webovou aplikaci, vyberte právě hello **Procházet** tlačítko hello portálu. Pro cloudovou službu, zvolte hello URL hello **rychlý přehled** části hello **řídicí panel** stránky, která obsahuje hello pracovní prostředí.
    
    Nasazení z průběžnou integraci pro cloudové služby jsou publikované toohello pracovní prostředí ve výchozím nastavení. Můžete to změnit nastavení hello **alternativní prostředí cloudové služby** vlastnost příliš**produkční**. Zde je, kde je adresa URL webu hello na stránce řídicího panelu hello cloudové služby.
    
    ![][31]
    
    Na nové záložce prohlížeče otevřete tooreveal spuštěné webu.
    
    ![][32]
18. Provedete-li jiné změny tooyour projektu, aktivační události je více sestavení a budou se hromadit více nasazení. Hello nejnovější jeden je označen jako aktivní.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: znovu nasaďte starší sestavení
Tento krok je volitelný. V hello portál Azure classic, zvolte k předchozímu nasazení a **znovu nasaďte** toorewind vaší lokality tooan dříve vrácení se změnami. Všimněte si, že to aktivují nového sestavení v sadě TFS a vytvořit nový záznam v historii nasazení.

![][34]

## <a name="6-change-hello-production-deployment"></a>6: změnit hello produkčního nasazení
Jakmile budete připraveni, můžete zvýšit úroveň hello pracovní prostředí toohello produkčního prostředí, tak, že zvolíte **odkládacího souboru** v hello portál Azure classic. Hello nově nasazené pracovní prostředí se zvýšenou úrovní tooProduction a hello předchozí provozním prostředí, pokud existuje, bude pracovní prostředí. Hello aktivní nasazení může být různé pro hello produkční a pracovní prostředí, ale historii nasazení hello poslední sestavení je hello stejný bez ohledu na to prostředí.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: nasazení z pracovní větev.
Při použití Git, obvykle provedete změny v pracovní větev a integrovat do hlavní větve hello, pokud váš vývojový dosáhne konečného stavu. V průběhu fáze vývoje hello projektu budete má toobuild a nasaďte hello pracovní větev tooAzure.

1. V **Team Explorer**, zvolte hello **Domů** tlačítko a potom zvolte hello **větví** tlačítko.
   
    ![][40]
2. Zvolte hello **nové větve** odkaz.
   
    ![][41]
3. Zadejte název hello hello větve, jako je například "fungoval," a zvolte **vytvoření větve**. Tím se vytvoří nové místní větve.
   
    ![][42]
4. Publikujte hello větev. Zvolte název větve hello v **Nepublikováno větví**a zvolte **publikovat**.
   
    ![][44]
5. Ve výchozím nastavení pouze změní aktivační událost hlavní větve toohello průběžné sestavení. tooset až průběžné sestavení pro pracovní větve, zvolte hello **sestavení** stránky v **Team Explorer**a zvolte **upravit definici sestavení**.
6. Otevřete hello **nastavení zdroje** kartě. V části **monitorovat větve pro průběžnou integraci a sestavení**, zvolte **tooadd nový řádek, klikněte sem**.
   
    ![][47]
7. Zadejte hello větev, kterou jste vytvořili, například odolný systém souborů nebo oznámení nebo pracovní.
   
    ![][48]
8. Provést změny v kódu hello, otevřete hello místní nabídky souboru hello změnit a potom zvolte **potvrdit**.
   
    ![][43]
9. Zvolte hello **nesynchronizované potvrzení** propojit a zvolte hello **synchronizace** tlačítko nebo hello **Push** odkaz toocopy hello změny toohello kopii hello pracovní větev v sadě Visual Studio Team Services.
   
   ![][45]
10. Přejděte toohello **sestavení** zobrazení a najít hello sestavení, který právě získali aktivoval pro hello pracovní větev.

## <a name="next-steps"></a>Další kroky
najdete v části Další tipy pro Visual Studio Team Services, pomocí Git toolearn [vývoj a sdílet kódu v úložišti Git pomocí sady Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) a informace o použití úložiště Git, který není spravován nástrojem Visual Studio Team Services toopublish tooAzure, najdete v části [tooAzure průběžné nasazování služby App Service](../app-service-web/app-service-continuous-deployment.md). Další informace o sadě Visual Studio Team Services najdete v tématu [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

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
