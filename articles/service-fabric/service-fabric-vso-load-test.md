---
title: "aaaLoad testování vaší aplikace pomocí sady Visual Studio Team Services | Microsoft Docs"
description: "Zjistěte, jak toostress testovací aplikace Azure Service Fabric pomocí Visual Studio Team Services."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: fc743585-0d1b-483f-981d-493f4552ac07
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 663cf8db5e8f0a4d0d7f27b585645d7f776392f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Zatížení testování vaší aplikace pomocí sady Visual Studio Team Services
Tento článek ukazuje, jak toouse Microsoft Visual Studio zatížení testovací funkce toostress testování aplikací. Používá Azure Service Fabric stavové služby back-end a bezstavové služby webového uživatelského rozhraní. Hello ukázková aplikace použít zde je simulátoru letadle umístění. Poskytnete ID letadle, čas odeslání a cíl. Hello back-end aplikace zpracovává hello požadavky a hello front-endu zobrazí v letadle hello mapy, odpovídající kritériím hello.

Hello následující diagram znázorňuje hello aplikace Service Fabric, která budete testování.

![Diagram hello příklad letadle umístění aplikace][0]

## <a name="prerequisites"></a>Požadavky
Než začnete, je třeba toodo hello následující:

* Získáte účet Visual Studio Team Services. Můžete jej získat zdarma v [Visual Studio Team Services](https://www.visualstudio.com).
* Získejte a nainstalujte Visual Studio 2013 nebo Visual Studio 2015. Tento článek používá edice Visual Studio 2015 Enterprise, ale Visual Studio 2013 a jiné edice by měly fungovat podobně.
* Nasazení vaší aplikace tooa pracovní prostředí. V tématu [jak toodeploy aplikace tooa vzdáleného clusteru pomocí sady Visual Studio](service-fabric-publish-app-remote-cluster.md) informace o této.
* Pochopení vzor používání vaší aplikace. Tyto informace jsou vzor zatížení používané toosimulate hello.
* Pochopení hello cílem pro zátěžové testování. To vám pomůže interpretovat a analýza výsledků zátěžových testů hello.

## <a name="create-and-run-hello-web-performance-and-load-test-project"></a>Vytvoření a spuštění projektu hello výkonu webu a zátěžového testu
### <a name="create-a-web-performance-and-load-test-project"></a>Vytvoření projektu výkonu webu a zátěžového testu
1. Otevřete sadu Visual Studio 2015. Zvolte **soubor** > **nový** > **projektu** v nabídce hello panelu tooopen hello **nový projekt** dialogové okno.
2. Rozbalte hello **Visual C#** uzel a zvolte **Test** > **výkonu webu a zátěžového testu projektu**. Pojmenujte projekt hello a potom zvolte hello **OK** tlačítko.

    ![Snímek obrazovky dialogového okna Nový projekt hello][1]

    Měli byste vidět nový projekt výkonu webu a zátěžového testu v Průzkumníku řešení.

    ![Snímek obrazovky Průzkumník řešení zobrazující hello nový projekt][2]

### <a name="record-a-web-performance-test"></a>Záznam testu výkonnosti webu
1. Otevřete hello .webtest projektu.
2. Zvolte hello **přidat záznam** ikonu toostart relaci záznamu v prohlížeči.

    ![Snímek obrazovky aplikace hello přidat záznam ikony v prohlížeči][3]

    ![Snímek obrazovky aplikace hello záznam tlačítka v prohlížeči][4]
3. Procházejte toohello aplikace Service Fabric. panel Hello záznam by měl zobrazit hello webových požadavků.

    ![Snímek obrazovky webových požadavků v záznamu panely hello][5]
4. Proveďte sekvenci akcí, které předpokládáte, aby uživatelé tooperform hello. Tyto akce slouží jako vzor toogenerate hello zatížení.
5. Až budete hotoví, zvolte hello **Zastavit** tlačítko toostop záznam.

    ![Snímek obrazovky tlačítka Zastavit hello][6]

    Hello .webtest projektu v sadě Visual Studio mají zaznamenány řadu požadavků. Dynamické parametry jsou automaticky nahradit. V tomto okamžiku můžete odstranit všechny závislosti navíc, opakované žádosti, které nejsou součástí vaší testovací scénáře.
6. Uložit hello projektu a pak zvolte hello **spustit Test** výkonu webu hello toorun příkaz lokálně otestovat a ujistěte se, zda vše funguje správně.

    ![Snímek obrazovky hello příkaz Spustit Test][7]

### <a name="parameterize-hello-web-performance-test"></a>Parametrizace hello testu výkonnosti webu
Převádění ho tooa programového testu výkonnosti webu a pak úpravy kódu hello můžete parametrizovat hello testu výkonnosti webu. Jako alternativu můžete vázat hello webové výkonu testovací tooa data seznamu tak, aby hello testovací iteruje hello data. V tématu [generování a spuštění testu výkonnosti webu programové](https://msdn.microsoft.com/library/ms182552.aspx) podrobnosti o tom, jak tooconvert hello webové výkonu testovací tooa programového testu. V tématu [přidat testu výkonnosti webu tooa zdroje dat](https://msdn.microsoft.com/library/ms243142.aspx) informace o tom, jak toobind data tooa výkonnosti webu otestovat.

V tomto příkladu jsme budete převést testu tooa programového testu výkonnosti webu hello tak můžete nahradit hello letadle ID vytvářenému identifikátoru GUID a přidat další požadavky toosend letů toodifferent umístění.

### <a name="create-a-load-test-project"></a>Vytvoření projektu testování zatížení
Projekt testu zatížení se skládá z jednoho nebo více scénářů testu výkonnosti webu hello a testování částí, společně s nastavením testu další zadaný zatížení. Hello následující kroky ukazují, jak toocreate zatížení testování projektu:

1. V místní nabídce hello projektu výkonu webu a zátěžového testu, zvolte **přidat** > **zátěžový Test**. V hello **testování zatížení** průvodce, zvolte hello **Další** tlačítko tooconfigure hello testovacích nastavení.
2. V hello **načíst vzor** zvolte, zda chcete zatížení konstantní uživatele nebo zatížení krok, který začíná na několik uživatelů a zvyšuje hello uživatelé v čase.

    Pokud máte funkční odhad hello množství zatížení uživatele a chcete toosee jak hello aktuální systém provede, zvolte **konstantní načíst**. Pokud je vaším cílem je toolearn, zda systém hello provede konzistentní v rámci různých zatížením, zvolte **krok zatížení**.
3. V hello **kombinace testů** zvolte hello **přidat** tlačítko a pak vyberte hello testů, které chcete tooinclude v hello zátěžový test. Můžete použít hello **distribuční** sloupec toospecify hello procento celkové testy spustit pro všechny testy.
4. V hello **spustit nastavení** , určete doba trvání testu zatížení hello.

   > [!NOTE]
   > Hello **testování iterací** možnost je dostupná pouze při spuštění zátěžového testu místně pomocí sady Visual Studio.
   >
   >
5. V hello **umístění** části **spustit nastavení**, zadejte hello umístění, kde se generují požadavky na testovací zatížení. Průvodce Hello může výzvu toolog v tooyour účtu Team Services. Přihlaste se a potom zvolte geografické umístění. Až budete hotoví, zvolte hello **Dokončit** tlačítko.
6. Po vytvoření hello zátěžový test, otevřete hello .loadtest projekt a zvolte hello aktuální spuštění nastavení, například **spustit nastavení** > **spustit Settings1 [Active]**. Otevře nastavení hello spustit v hello **vlastnosti** okno.
7. V hello **výsledky** části hello **spustit nastavení** vlastnosti – okno, hello **úložiště podrobností časování** by měl mít nastavení **žádné** jako Výchozí hodnota. Tuto hodnotu změnit příliš**všech podrobností o jednotlivých** tooget Další informace o výsledcích zátěžového testu hello. V tématu [načíst testování](https://www.visualstudio.com/load-testing.aspx) Další informace o jak tooconnect tooVisual Studio Team Services a spuštění zátěžového testu.

### <a name="run-hello-load-test-by-using-visual-studio-team-services"></a>Spusťte hello zátěžový test pomocí sady Visual Studio Team Services
Zvolte hello **spustit Test zatížení** příkaz toostart hello testu.

![Snímek obrazovky hello příkaz spuštění zátěžového testu][8]

## <a name="view-and-analyze-hello-load-test-results"></a>Zobrazení a analýza výsledků zátěžových testů hello
Jako hello načíst postupuje test, informace o výkonu hello jsou vynesena do grafu. Měli byste vidět něco podobné toohello následující grafu.

![Snímek obrazovky graf výkonu pro výsledků zátěžového testu][9]

1. Zvolte hello **stáhnout sestavu** odkaz v hello horní části stránky hello. Po stažení hello sestavy vyberte hello **zobrazit sestavu** tlačítko.

    Na hello **grafu** můžete zobrazit grafy pro různé čítače výkonu. Na hello **Souhrn** kartě hello celkové výsledky testů zobrazují. Hello **tabulky** kartě se zobrazují hello celkový počet předaný a k selhání zátěžové testy.
2. Zvolte hello počet odkazů na hello **Test** > **se nezdařilo** a hello **chyby** > **počet** sloupce Podrobnosti o chybě toosee.

    Hello **podrobností** kartě se zobrazují virtuální uživatele a testovací scénáře informace pro chybné žádosti. Tato data mohou být užitečné, pokud obsahuje více scénářů hello zátěžový test.

V tématu [analýza načíst výsledky testu v hello zobrazení grafů hello načíst testování analyzátor](https://www.visualstudio.com/load-testing.aspx) Další informace o zobrazení zatížení výsledky testu.

## <a name="automate-your-load-test"></a>Automatizovat zátěžového testu
Visual Studio Team Services načíst testovací poskytuje rozhraní API toohelp spravovat zátěžové testy a analyzovat výsledky v účtu Team Services. V tématu [rozhraní Rest API testování zatížení cloudu](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) Další informace.

## <a name="next-steps"></a>Další kroky
* [Monitorování a Diagnostika služby v instalačním programu pro vývoj místním počítači](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
