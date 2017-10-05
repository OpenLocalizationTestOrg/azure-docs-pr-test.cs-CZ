---
title: "Nastavené průběžné doručování s Visual Studio Team Services v Azure | Microsoft Docs"
description: "Naučte se konfigurovat týmových projektech Visual Studio Team Services automaticky vytvářet a nasazovat do funkci webové aplikace v Azure App Service nebo cloudové služby."
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
ms.openlocfilehash: d80ce63eb7ddfd7c45726be887a772f9a7594b28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Průběžné doručování do Azure pomocí služby Visual Studio Team Services
Můžete nakonfigurovat týmových projektech Visual Studio Team Services k automatickému vytvoření a nasazení do webové aplikace Azure nebo cloudové služby.  (Informace o tom, jak nastavit průběžné sestavení a nasadit pomocí systému *místní* Team Foundation Server najdete v části [nastavené průběžné doručování pro cloudové služby v Azure](cloud-services-dotnet-continuous-delivery.md).)

Tento kurz předpokládá, že máte Visual Studio 2013 a Azure SDK nainstalována. Pokud ještě nemáte Visual Studio 2013, stáhněte ho tak, že zvolíte **začněte zadarmo** odkaz na [www.visualstudio.com](http://www.visualstudio.com). Nainstalovat sadu Azure SDK z [zde](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Účet Visual Studio Team Services k dokončení tohoto kurzu potřebujete: můžete [zdarma otevřít účet Visual Studio Team Services](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

Pokud chcete nastavit Cloudová služba pro automatické vytvoření a nasazení do Azure pomocí sady Visual Studio Team Services, postupujte podle těchto kroků.

## <a name="1-create-a-team-project"></a>1: Vytvoření týmového projektu
Postupujte podle pokynů [sem](http://go.microsoft.com/fwlink/?LinkId=512980) k vytvoření týmového projektu a tu propojit na Visual Studio. Tento návod předpokládá, že používáte jako řešení pro řízení zdrojového serveru Team Foundation verze ovládacího prvku (TFVC). Pokud chcete použít pro správu verzí Git, přečtěte si téma [Git verze tohoto návodu](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: projektu do správy zdrojového kódu se změnami
1. V sadě Visual Studio otevřete řešení, které chcete nasadit, nebo vytvořte novou.
   Podle kroků v tomto názorném postupu můžete nasadit webovou aplikaci nebo cloudové služby (aplikace Azure).
   Pokud chcete vytvořit nové řešení, vytvořte nový projekt Azure Cloud Service, nebo nový projekt ASP.NET MVC. Ujistěte se, že cílem projektu rozhraní .NET Framework 4 nebo 4.5 a pokud vytvoříte projekt cloudové služby, přidat webová role ASP.NET MVC a roli pracovního procesu a zvolte Internetové aplikace pro webovou roli. Po zobrazení výzvy vyberte **Internetové aplikace**.
   Pokud chcete vytvořit webovou aplikaci, zvolte šablonu projektu webové aplikace ASP.NET a potom zvolte MVC. V tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).
   
   > [!NOTE]
   > Visual Studio Team Services nyní podporují pouze CI nasazení webových aplikací, Visual Studio. Webové projekty jsou mimo rozsah.
   > 
   > 
2. Otevřete kontextu nabídku pro řešení a zvolte **přidat řešení správy zdrojového kódu**.
   
    ![][5]
3. Potvrďte nebo změňte výchozí hodnoty a zvolte **OK** tlačítko. Po dokončení procesu zdroj ovládacího prvku ikony zobrazovat v **Průzkumníku řešení**.
   
    ![][6]
4. Otevřete místní nabídku pro řešení a zvolte **změnami**.
   
    ![][7]
5. V **čekající změny** oblasti **Team Explorer**zadejte poznámku k vrácení se změnami a zvolte **změnami** tlačítko.
   
    ![][8]
   
    Všimněte si možnosti, které chcete zahrnout nebo vyloučit konkrétní změny při se změnami. Pokud chcete, jsou vyloučeny změny, zvolte **zahrnují všechny** odkaz.
   
    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: připojení projekt do Azure
1. Nyní když máte Visual Studio Team Services týmového projektu s některé zdrojového kódu v ní, jste připraveni se připojit k Azure týmového projektu.  V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), vyberte svou cloudové služby nebo webovou aplikaci nebo vytvořte novou výběrem  **+**  ikona ve spodní levé a výběr **Cloudová služba**nebo **webová aplikace** a potom **rychle vytvořit**. Vyberte **nastavit publikování pomocí Visual Studio Team Services** odkaz.
   
    ![][10]
2. V průvodci zadejte název účtu Visual Studio Team Services textové pole a klikněte na **autorizovat teď** odkaz. Můžete být požádáni o přihlášení.
   
    ![][11]
3. V **požadavku na připojení** automaticky otevíraný dialog, vyberte **přijmout** tlačítko Autorizovat Azure nakonfigurovat týmového projektu v Visual Studio Team Services.
   
    ![][12]
4. Při ověřování úspěšné, uvidíte rozevírací seznam obsahující seznam týmových projektech Visual Studio Team Services. Vyberte název týmového projektu, který jste vytvořili v předchozím kroku a pak zvolte tlačítko zaškrtnutí v průvodci.
   
    ![][13]
5. Po projektu je propojená, zobrazí se některé pokyny k vrácení se změnami změny k vašemu týmovému projektu Visual Studio Team Services.  Na vaše další vrácení se změnami Visual Studio Team Services bude sestavovat a nasazení projektu do Azure.  Zkuste tento teď pomocí klepnutím na tlačítko **změnami ze sady Visual Studio** odkaz a potom **spusťte Visual Studio** odkaz (nebo ekvivalentní **Visual Studio** tlačítko v dolní části portál obrazovky).
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: aktivovat opětovném sestavení a znovu nasaďte projekt
1. V sadě Visual Studio **Team Explorer**, vyberte **Průzkumník správy zdrojového kódu** odkaz.
   
    ![][15]
2. Přejděte k souboru řešení a otevřete jej.
   
    ![][16]
3. V **Průzkumníku**, otevřete soubor a ho změnit. Můžete například změnit soubor `_Layout.cshtml` v zobrazení\\sdílené složky MVC webové role.
   
    ![][17]
4. Upravit logo pro lokalitu a zvolte **Ctrl + S** k uložení souboru.
   
    ![][18]
5. V **Team Explorer**, vyberte **čekající změny** odkaz.
   
    ![][19]
6. Zadejte komentář a potom zvolte **změnami** tlačítko.
   
    ![][20]
7. Zvolte **domácí** tlačítko se vrátíte do **Team Explorer** domovskou stránku.
   
    ![][21]
8. Vyberte **sestavení** odkaz zobrazíte nastavení v průběhu.
   
    ![][22]
   
    **Team Explorer** ukazuje, že byla spuštěna sestavení pro vaše vrácení se změnami.
   
    ![][23]
9. Dvakrát klikněte na název sestavení v průběhu zobrazíte podrobné protokolu v průběhu sestavení.
   
    ![][24]
10. Při sestavení je v průběhu, podívejte se na definici sestavení, která byla vytvořena, když jste propojili TFS do Azure pomocí průvodce.  Otevřete místní nabídku pro definici sestavení a zvolte **upravit definici sestavení**.
    
     ![][25]
    
     V **aktivační událost** karty, zobrazí se, že je ve výchozím nastavení na každé vrácení se změnami definici sestavení.
    
     ![][26]
    
     V **proces** kartě uvidíte prostředí nasazení je nastavena na název cloudové služby nebo webovou aplikaci. Pokud pracujete s webovými aplikacemi, bude vlastnosti, které vidíte liší od těch, které tady uvedené.
    
     ![][27]
11. Zadejte hodnoty vlastností, pokud chcete jiné hodnoty než výchozí hodnoty. Vlastnosti pro publikování v Azure jsou ve **nasazení** části.
    
     V následující tabulce jsou uvedeny dostupné vlastnosti v **nasazení** části:
    
    | Vlastnost | Výchozí hodnota |
    | --- | --- |
    | Povolit nedůvěryhodné certifikáty |Pokud je hodnota false, musí být podepsané certifikáty SSL kořenovou autoritou. |
    | Povolit upgradu |Umožňuje nasazení k aktualizaci stávajícího nasazení místo vytvoření nové. Zachovává IP adresu. |
    | Neodstraňujte |V případě hodnoty true Nepřepisovat existující nesouvisejícími nasazení (upgrade je povolena.). |
    | Cesta k nastavení nasazení |Cesta k souboru .pubxml pro webovou aplikaci, relativně ke kořenové složce úložiště. Ignorovat pro cloudové služby. |
    | Prostředí nasazení služby SharePoint |Stejný jako název služby. |
    | Prostředí nasazení Azure |Webové aplikace nebo cloudové název služby. |
12. Pokud používáte více konfigurace služby (soubory .cscfg), abyste mohli zadat konfiguraci požadované služby v **sestavení, Upřesnit, argumenty MSBuild** nastavení. Například pokud chcete používat ServiceConfiguration.Test.cscfg, nastavit argumenty MSBuild řádku `/p:TargetProfile=Test`.
    
     ![][38]
    
     Buildu té doby by se měly dokončit úspěšně.
    
     ![][28]
13. Pokud dvakrát kliknete na název sestavení, Visual Studio se zobrazí **souhrnu sestavení**, včetně všech výsledků testu z přidružené projektů testování částí.
    
     ![][29]
14. V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), přidružené nasazení můžete zobrazit na **nasazení** kartě, pokud je vybrána pracovní prostředí.
    
     ![][30]
15. Přejděte na adresu URL vašeho webu. Pro webovou aplikaci, klepněte **Procházet** tlačítka na panelu příkazů. Cloudové služby, vyberte adresu URL v **rychlý přehled** části **řídicí panel** stránky, která obsahuje pracovní prostředí pro cloudové služby. Nasazení z průběžnou integraci pro cloudové služby jsou publikovány do prostředí pracovní ve výchozím nastavení. Toto můžete změnit nastavením **alternativní prostředí cloudové služby** vlastnost **produkční**. Tento snímek obrazovky ukazuje, kde je adresa URL webu na stránce řídicího panelu cloudové služby.
    
    ![][31]
    
    Na nich spuštěné webu se otevře novou kartu prohlížeče.
    
    ![][32]
    
    Pro cloudové služby, pokud udělat jiné změny do projektu, aktivační události je více sestavení a budou se hromadit více nasazení. Nejnovější označen jako aktivní.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: znovu nasaďte starší sestavení
Tento krok platí pro cloudové služby a je volitelný. Na portálu Azure classic, zvolte k předchozímu nasazení a pak **znovu nasaďte** tlačítko rewind váš web, dřívější vrácení se změnami.  Všimněte si, že to aktivují nového sestavení v sadě TFS a vytvořit nový záznam v historii nasazení.

![][34]

## <a name="6-change-the-production-deployment"></a>6: Změňte produkční nasazení
Tento krok platí jenom pro cloudové služby, není webové aplikace. Jakmile budete připraveni, můžete zvýšit úroveň pracovního prostředí do produkčního prostředí, tak, že zvolíte **Prohodit** tlačítka na portálu Azure classic. Nově nasazené pracovní prostředí se zvýší úroveň na produkční a předchozí provozním prostředí, pokud existuje, bude pracovní prostředí. Aktivní nasazení může být různé pro produkční a pracovní prostředí, ale historii nasazení poslední sestavení je stejný bez ohledu na prostředí.

![][35]

## <a name="7-run-unit-tests"></a>7: spouštění testů jednotek
Tento krok platí pouze pro webové aplikace, není cloudových služeb. Brány kvality uvést na vaše nasazení, můžete spustit testy jednotek a jejich případné selhání, můžete zastavit nasazení.

1. V sadě Visual Studio přidejte projektu testování částí.
   
   ![][39]
2. Přidejte odkazy na projekt na projekt, který chcete testovat.
   
   ![][40]
3. Přidejte některé testy jednotek. Chcete-li začít, zkuste fiktivní test, který bude vždycky předávat.
   
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
4. Upravit definici sestavení, vyberte **proces** a rozbalte **Test** uzlu.
5. Nastavte **sestavení selhání při testu selhání** na hodnotu True. To znamená, že nedojde k nasazení, pokud jsou testy úspěšné.
   
   ![][41]
6. Fronta nové sestavení.
   
   ![][42]
   
   ![][43]
7. Při sestavení je budete pokračovat, zkontrolujte v jeho průběhu.
   
    ![][44]
   
    ![][45]
8. Pokud se provádí sestavení, zkontrolujte výsledky.
   
    ![][46]
   
    ![][47]
9. Zkuste vytvořit test, který se nezdaří. Přidat nový test zkopírováním první z nich, přejmenujte ji a Zakomentovat řádek kódu, který s oznámením, že NotImplementedException – jde o očekávané výjimku.
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. Zkontrolujte v změn do fronty nové sestavení.
    
     ![][48]
11. Zobrazení výsledků testu zobrazíte podrobnosti o selhání.
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a>Další kroky
Další informace o jednotce testování v sadě Visual Studio Team Services najdete v tématu [spouštění testování částí v buildu](http://go.microsoft.com/fwlink/p/?LinkId=510474). Pokud používáte Git, přečtěte si téma [sdílet kódu v úložišti Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) a [průběžné nasazování do služby Azure App Service](../app-service-web/app-service-continuous-deployment.md).  Další informace o sadě Visual Studio Team Services najdete v tématu [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

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
