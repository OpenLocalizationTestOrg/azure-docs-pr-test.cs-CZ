---
title: "doručení aaaContinuous s Visual Studio Team Services v Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Visual Studio Team Services týmové projekty tooautomatically sestavení a nasazení funkce toohello webové aplikace v Azure App Service nebo cloudové služby."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 797f67ad-e4d4-4063-ae91-41cbdf154191
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: eae75729e1c1a55f9bc3375604a8192f329d0042
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services"></a>Pomocí Visual Studio Team Services tooAzure nastavené průběžné doručování
Konfigurace buildu tooautomatically týmových projektů Visual Studio Team Services a nasazením tooAzure webové aplikace nebo cloudové služby.  (Informace o tom, jak tooset až průběžné sestavení a nasadit pomocí systému *místní* Team Foundation Server najdete v části [nastavené průběžné doručování pro cloudové služby v Azure](cloud-services-dotnet-continuous-delivery.md).)

Tento kurz předpokládá, že máte Visual Studio 2013 a Azure SDK nainstalovat hello. Pokud ještě nemáte Visual Studio 2013, stáhněte ho tak, že zvolíte hello **začněte zadarmo** odkaz na [www.visualstudio.com](http://www.visualstudio.com). Instalace hello Azure SDK z [zde](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Budete potřebovat Visual Studio Team Services toocomplete účet v tomto kurzu: můžete [zdarma otevřít účet Visual Studio Team Services](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

tooset nahoru cloudové služby tooautomatically sestavení a nasazení tooAzure pomocí Visual Studio Team Services, postupujte podle těchto kroků.

## <a name="1-create-a-team-project"></a>1: Vytvoření týmového projektu
Postupujte podle pokynů hello [sem](http://go.microsoft.com/fwlink/?LinkId=512980) toocreate váš tým projektu a propojit jej tooVisual Studio. Tento návod předpokládá, že používáte jako řešení pro řízení zdrojového serveru Team Foundation verze ovládacího prvku (TFVC). Pokud chcete toouse Git pro správu verzí, najdete v části [hello Git verze tohoto návodu](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-toosource-control"></a>2: Zkontrolujte v ovládacím prvku toosource projektu
1. V sadě Visual Studio otevřete řešení hello sami toodeploy, nebo vytvořte novou.
   Můžete nasadit webovou aplikaci nebo cloudové služby (aplikace Azure) podle následujících hello kroky v tomto návodu.
   Pokud chcete toocreate nové řešení, vytvořte nový projekt Azure Cloud Service, nebo nový projekt ASP.NET MVC. Ujistěte se, který hello cíle projektu rozhraní .NET Framework 4 nebo 4.5 a při vytváření projektu cloudové služby, přidat webová role ASP.NET MVC a roli pracovního procesu a zvolte Internetové aplikace hello webové role. Po zobrazení výzvy vyberte **Internetové aplikace**.
   Pokud chcete toocreate webovou aplikaci, zvolte šablona projektu webové aplikace ASP.NET hello a potom zvolte MVC. V tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).
   
   > [!NOTE]
   > Visual Studio Team Services nyní podporují pouze CI nasazení webových aplikací, Visual Studio. Webové projekty jsou mimo rozsah.
   > 
   > 
2. Otevřete řešení hello hello kontextovou nabídku a vyberte **tooSource přidat řešení řízení**.
   
    ![][5]
3. Potvrďte nebo změňte výchozí nastavení hello a zvolte hello **OK** tlačítko. Po dokončení procesu hello zdroj ovládacího prvku ikony zobrazovat v **Průzkumníku řešení**.
   
    ![][6]
4. Otevřete hello místní nabídku pro hello řešení a zvolte **změnami**.
   
    ![][7]
5. V hello **čekající změny** oblasti **Team Explorer**, zadejte komentář pro hello vrácení se změnami a zvolte hello **změnami** tlačítko.
   
    ![][8]
   
    Poznámka: možnosti tooinclude hello nebo vyloučit konkrétní změny při se změnami. Pokud chcete, jsou vyloučeny změny, zvolte hello **zahrnují všechny** odkaz.
   
    ![][9]

## <a name="3-connect-hello-project-tooazure"></a>3: připojení tooAzure projektu hello
1. Nyní když máte Visual Studio Team Services týmového projektu s některé zdrojového kódu v ní, se připravené tooconnect váš tým projektu tooAzure.  V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), vyberte svou cloudové služby nebo webovou aplikaci nebo vytvořte novou výběrem hello  **+**  ikonu v dolní hello vlevo a výběr **cloudovéslužby** nebo **webová aplikace** a potom **rychle vytvořit**. Zvolte hello **nastavit publikování pomocí Visual Studio Team Services** odkaz.
   
    ![][10]
2. V Průvodci hello hello název svého účtu Visual Studio Team Services v textovém poli hello a klikněte na hello **autorizovat teď** odkaz. Můžete být požádáni toosign v.
   
    ![][11]
3. V hello **požadavku na připojení** automaticky otevíraný dialog, zvolte hello **přijmout** tooauthorize tlačítko Azure tooconfigure váš tým projektu ve Visual Studio Team Services.
   
    ![][12]
4. Při ověřování úspěšné, uvidíte rozevírací seznam obsahující seznam týmových projektech Visual Studio Team Services. Zvolte název hello týmový projekt, který jste vytvořili v předchozích krocích hello a potom zvolte tlačítko zaškrtnutí hello průvodce.
   
    ![][13]
5. Po projektu je propojená, zobrazí se některé pokyny k vrácení se změnami změny tooyour Visual Studio Team Services týmového projektu.  Na vaše další vrácení se změnami Visual Studio Team Services bude sestavovat a nasadit tooAzure vašeho projektu.  Zkuste tuto chybu kliknutím hello **změnami ze sady Visual Studio** propojit a pak hello **spusťte Visual Studio** propojení (nebo ekvivalentní hello **Visual Studio** tlačítko dole hello úvodní portálu obrazovka).
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: aktivovat opětovném sestavení a znovu nasaďte projekt
1. V sadě Visual Studio **Team Explorer**, zvolte hello **Průzkumník správy zdrojového kódu** odkaz.
   
    ![][15]
2. Přejděte na soubor řešení tooyour a otevřete jej.
   
    ![][16]
3. V **Průzkumníku**, otevřete soubor a ho změnit. Můžete například změnit soubor hello `_Layout.cshtml` pod zobrazení hello\\sdílené složky MVC webové role.
   
    ![][17]
4. Upravit hello logo hello lokality a zvolte **Ctrl + S** toosave hello souboru.
   
    ![][18]
5. V **Team Explorer**, zvolte hello **čekající změny** odkaz.
   
    ![][19]
6. Zadejte komentář a potom zvolte hello **změnami** tlačítko.
   
    ![][20]
7. Zvolte hello **domácí** tlačítko tooreturn toohello **Team Explorer** domovské stránky.
   
    ![][21]
8. Zvolte hello **sestavení** hello tooview odkaz sestavení v průběhu.
   
    ![][22]
   
    **Team Explorer** ukazuje, že byla spuštěna sestavení pro vaše vrácení se změnami.
   
    ![][23]
9. Dvakrát klikněte na název hello hello sestavení v průběhu tooview podrobné protokolu sestavení průběhu hello.
   
    ![][24]
10. Probíhá při sestavení hello prohlédněte hello definice sestavení, která byla vytvořena, když jste propojili TFS tooAzure pomocí Průvodce hello.  Otevřete hello místní nabídku pro definici sestavení hello a zvolte **upravit definici sestavení**.
    
     ![][25]
    
     V hello **aktivační událost** karty, zobrazí se, že definici sestavení hello toobuild na každý vrácení se změnami ve výchozím nastavení.
    
     ![][26]
    
     V hello **proces** kartě uvidíte prostředí nasazení hello nastavena toohello název cloudové služby nebo webovou aplikaci. Pokud pracujete s webovými aplikacemi, bude hello vlastnosti, které vidíte liší od těch, které tady uvedené.
    
     ![][27]
11. Zadejte hodnoty pro vlastnosti hello, pokud chcete jiné hodnoty než výchozí hello. Hello vlastnosti pro publikování v Azure jsou ve hello **nasazení** části.
    
     Hello následující tabulce jsou uvedeny dostupné vlastnosti hello v hello **nasazení** části:
    
    | Vlastnost | Výchozí hodnota |
    | --- | --- |
    | Povolit nedůvěryhodné certifikáty |Pokud je hodnota false, musí být podepsané certifikáty SSL kořenovou autoritou. |
    | Povolit upgradu |Umožňuje nasazení tooupdate hello stávajícího nasazení místo vytvoření nové. Zachovává hello IP adresu. |
    | Neodstraňujte |V případě hodnoty true Nepřepisovat existující nesouvisejícími nasazení (upgrade je povolena.). |
    | Cesta tooDeployment nastavení |Hello cesta tooyour .pubxml soubor pro webovou aplikaci, relativní toohello kořenové složky hello úložišti. Ignorovat pro cloudové služby. |
    | Prostředí nasazení služby SharePoint |Hello stejný jako název služby hello. |
    | Prostředí nasazení Azure |Hello aplikace nebo cloudové název webové služby. |
12. Pokud používáte více konfigurace služby (soubory .cscfg), abyste mohli zadat konfiguraci hello požadované služby v hello **sestavení, Upřesnit, argumenty MSBuild** nastavení. Například toouse ServiceConfiguration.Test.cscfg, nastavte argumenty MSBuild řádku `/p:TargetProfile=Test`.
    
     ![][38]
    
     Buildu té doby by se měly dokončit úspěšně.
    
     ![][28]
13. Pokud dvakrát kliknete na název sestavení hello Visual Studio se zobrazí **souhrnu sestavení**, včetně všech výsledků testu z přidružené projektů testování částí.
    
     ![][29]
14. V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), hello přidružené nasazení můžete zobrazit na hello **nasazení** kartě, pokud je vybrána hello pracovní prostředí.
    
     ![][30]
15. Procházejte tooyour adresy URL. Pro webovou aplikaci, stačí kliknout na hello **Procházet** tlačítka na panelu příkazů hello. Pro cloudovou službu, zvolte hello URL hello **rychlý přehled** části hello **řídicí panel** stránky, která obsahuje hello pracovní prostředí pro cloudové služby. Nasazení z průběžnou integraci pro cloudové služby jsou publikované toohello pracovní prostředí ve výchozím nastavení. Můžete to změnit nastavení hello **alternativní prostředí cloudové služby** vlastnost příliš**produkční**. Tento snímek obrazovky ukazuje, kde hello že adresa URL webu je na stránce řídicího panelu hello cloudové služby.
    
    ![][31]
    
    Na nové záložce prohlížeče otevřete tooreveal spuštěné webu.
    
    ![][32]
    
    Pro cloudové služby Pokud provedete projektu tooyour další změny, aktivační události je více sestavení a budou se hromadit více nasazení. Hello nejnovější jeden označen jako aktivní.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: znovu nasaďte starší sestavení
Tento krok platí toocloud služby a je volitelný. V hello portál Azure classic, vyberte k předchozímu nasazení a potom zvolte hello **znovu nasaďte** tlačítko toorewind vaší lokality tooan dříve vrácení se změnami.  Všimněte si, že to aktivují nového sestavení v sadě TFS a vytvořit nový záznam v historii nasazení.

![][34]

## <a name="6-change-hello-production-deployment"></a>6: změnit hello produkčního nasazení
Tento krok platí jenom toocloud služby, není webové aplikace. Jakmile budete připraveni, můžete zvýšit úroveň hello pracovní prostředí toohello produkčního prostředí, tak, že zvolíte hello **Prohodit** tlačítka na hello portál Azure classic. Hello nově nasazené pracovní prostředí se zvýšenou úrovní tooProduction a hello předchozí provozním prostředí, pokud existuje, bude pracovní prostředí. Hello aktivní nasazení může být různé pro hello produkční a pracovní prostředí, ale historii nasazení hello poslední sestavení je hello stejný bez ohledu na to prostředí.

![][35]

## <a name="7-run-unit-tests"></a>7: spouštění testů jednotek
Tento krok platí jenom tooweb aplikace, není cloudové služby. tooput brány kvality na vaše nasazení, můžete spustit testy jednotek a jejich případné selhání, můžete zastavit nasazení hello.

1. V sadě Visual Studio přidejte projektu testování částí.
   
   ![][39]
2. Přidáte projekt toohello odkazy na projekt chcete tootest.
   
   ![][40]
3. Přidejte některé testy jednotek. tooget spustili, zkuste fiktivní test, který bude vždycky předávat.
   
       ```
       using System;
       using Microsoft.VisualStudio.TestTools.UnitTesting;
   
       namespace UnitTestProject1
       {
           [TestClass]
           public class UnitTest1
           {
               [TestMethod]
               [ExpectedException(typeof(NotImplementedException))]
               public void TestMethod1()
               {
                   throw new NotImplementedException();
               }
           }
       }
       ```
4. Upravit definici sestavení hello, zvolte hello **proces** a rozbalte hello **Test** uzlu.
5. Sada hello **sestavení selhání při testu selhání** tooTrue. To znamená, že nasazení hello nedojde, pokud hello testy průchodu.
   
   ![][41]
6. Fronta nové sestavení.
   
   ![][42]
   
   ![][43]
7. Při sestavení hello je budete pokračovat, zkontrolujte v jeho průběhu.
   
    ![][44]
   
    ![][45]
8. Pokud se provádí hello sestavení, zkontrolujte výsledky testů hello.
   
    ![][46]
   
    ![][47]
9. Zkuste vytvořit test, který se nezdaří. Přidat nový test zkopírováním hello první z nich, přejmenujte ji a komentář hello řádek kódu, který s oznámením, že NotImplementedException – jde o očekávané výjimku.
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. Vrácení se změnami hello změnit tooqueue nové sestavení.
    
     ![][48]
11. Zobrazit hello testovací výsledky toosee podrobnosti o selhání hello.
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a>Další kroky
Další informace o jednotce testování v sadě Visual Studio Team Services najdete v tématu [spouštění testování částí v buildu](http://go.microsoft.com/fwlink/p/?LinkId=510474). Pokud používáte Git, přečtěte si téma [sdílet kódu v úložišti Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) a [tooAzure průběžné nasazování služby App Service](../app-service-web/app-service-continuous-deployment.md).  Další informace o sadě Visual Studio Team Services najdete v tématu [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
