---
title: "aaaService prostředků infrastruktury aplikace upgradu kurzu | Microsoft Docs"
description: "Tento článek vás provede hello činnost nasazení aplikace Service Fabric, změna hello kódu a zavádění upgrade pomocí sady Visual Studio."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a3181a7a-9ab1-4216-b07a-05b79bd826a4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: d069ff0b291018dbac846e65cddff1e9d73d156c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-tutorial-using-visual-studio"></a>Kurz upgradu Service Fabric aplikace pomocí sady Visual Studio
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

Azure Service Fabric zjednodušuje proces hello upgrade cloudovým aplikacím tím, že zajistí, že budou upgradovány pouze změněné služby a zda je monitorovaný stavu aplikací v rámci procesu upgradu hello. Je také automaticky vrátí zpět předchozí verze toohello hello aplikace při zjištění problémy. Upgrade aplikace Service Fabric je *nula. výpadků*, protože můžete provést upgrade aplikace hello bez výpadků. Tento kurz se zaměřuje na tom, jak toocomplete postupného upgradu ze sady Visual Studio.

## <a name="step-1-build-and-publish-hello-visual-objects-sample"></a>Krok 1: Vytvoření a publikování ukázkové vizuální objekty hello
Nejprve stáhnout hello [vizuální objekty](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Actors/VisualObjects) aplikace z webu GitHub. Potom sestavení a publikování aplikace hello kliknutím pravým tlačítkem na projekt aplikace hello, **VisualObjects**a výběr hello **publikovat** v položku nabídky hello Service Fabric.

![Kontextové nabídky pro aplikace Service Fabric][image1]

Výběr **publikovat** otevře místní okno, a můžete nastavit hello **cíle profil** příliš**PublishProfiles\Local.xml**. Hello okno by měl vypadat jako následující hello před kliknutím na **publikovat**.

![Publikování aplikace Service Fabric][image2]

Teď můžete kliknout na **publikovat** v dialogovém okně hello. Můžete použít [aplikace Service Fabric Explorer tooview hello clusteru a hello](service-fabric-visualizing-your-cluster.md). Hello vizuální objekty aplikace má webové služby, můžete přejít zadáním tooby [http://localhost:8081/visualobjects/](http://localhost:8081/visualobjects/) hello adresního řádku prohlížeče.  Měli byste vidět 10 plovoucí vizuální objekty pohyb na úvodní obrazovka.

**Poznámka:** Pokud nasazení příliš`Cloud.xml` profilu (Azure Service Fabric), aplikace hello potom by mělo být k dispozici na **http://{ServiceFabricName}. { Region}.cloudapp.Azure.com:8081/visualobjects/**. Zajistěte, aby byla `8081/TCP` nakonfigurované v hello nástroj pro vyrovnávání zatížení (najde hello nástroj pro vyrovnávání zatížení v hello stejné skupině prostředků jako hello Service Fabric instance).

## <a name="step-2-update-hello-visual-objects-sample"></a>Krok 2: Aktualizace Ukázka vizuální objekty hello
Můžete si všimnout, že s verzí hello, který byl nasazen v kroku 1, není otočit hello vizuální objekty. Umožňuje upgradovat tento tooone aplikace, kde také otočit hello vizuální objekty.

Vyberte projekt VisualObjects.ActorService hello v rámci hello VisualObjects řešení a otevřete hello **VisualObjectActor.cs** souboru. V rámci tohoto souboru, přejděte metoda toohello `MoveObject`, komentář `visualObject.Move(false)`a zrušte komentář u `visualObject.Move(true)`. Tato změna kódu otočí hello objekty po upgradu služby hello.  **Nyní můžete vytvořit (ne opětovné sestavení) hello řešení**, která sestavení hello upravit projekty. Pokud vyberete *znovu vytvořit všechny*, máte tooupdate hello verze pro všechny hello projekty.

Také potřebujeme tooversion naší aplikace. verze hello toomake změny po kliknutí pravým tlačítkem na hello **VisualObjects** projektu, můžete použít hello Visual Studio **upravit verze manifestu** možnost. Tato možnost zobrazí hello dialogové okno pro edition verze následujícím způsobem:

![Dialogové okno Správa verzí][image3]

Projekty a jejich kód balíčky, společně s tooversion aplikace hello 2.0.0 upravil hello verze aktualizací pro hello Po provedení změn hello, hello manifest by měl vypadat jako následující hello (tučné části zobrazit změny hello):

![Aktualizace verze][image4]

Nástroje sady Visual Studio Hello můžete provést automatické kumulativní verzí při výběru **automatické aktualizaci aplikace a verze aktualizace service**. Pokud používáte [SemVer](http://www.semver.org), je nutné tooupdate hello kódu a/nebo konfigurace balíčku verze samostatně, pokud tuto možnost.

Uložte změny hello a nyní zkontrolujte hello **upgradu hello aplikace** pole.

## <a name="step-3--upgrade-your-application"></a>Krok 3: Upgradu vaší aplikace
Seznamte se s hello [parametry upgradu aplikace](service-fabric-application-upgrade-parameters.md) a hello [procesu upgradu](service-fabric-application-upgrade.md) tooget dobrou znalost jazyka hello různé parametry, vypršení časových limitů a stavu kritérium, které můžete upgradovat použít. V tomto návodu je nastavit kritéria hodnocení stavu služby hello toohello výchozí (sledována režim). Tato nastavení můžete nakonfigurovat tak, že vyberete **nakonfigurovat nastavení upgradu** a poté upravíte parametry hello podle potřeby.

Teď nemůžeme se všechny aplikace hello toostart sadu upgradu výběrem **publikovat**. Tato možnost provede upgrade vaší aplikace tooversion 2.0.0, ve kterém otočit hello objekty. Service Fabric upgraduje jednu aktualizační doménu najednou (některé objekty se aktualizují jako první, za nímž následuje jiné) a služby hello zůstávají dostupné během upgradu hello. Služba toohello Access lze zkontrolovat prostřednictvím vašeho klienta (prohlížeč).  

Nyní, jako hello aplikace upgradu bude pokračovat, je možné jej monitorovat pomocí Service Fabric Exploreru pomocí hello **upgrady v průběhu** na kartě aplikace hello.

Za několik minut všechny domény aktualizace je potřeba upgradovat (Dokončit), a okno výstup sady Visual Studio hello by také stavu že dokončení upgradu této hello. A by měl zjistit, který *všechny* hello vizuální objekty v okně prohlížeče jsou nyní otáčení!

Můžete má tootry změna hello verze a přesouvání z verze 2.0.0 tooversion 3.0.0 jako cvičení, nebo dokonce z verze 2.0.0 zpět tooversion 1.0.0. Přehrání s vypršení časových limitů a toomake zásady stavu sami obeznámeni s nimi. Po nasazení clusteru Azure jako tooan byl proti tooa místní cluster, může být použité parametry hello toodiffer. Doporučujeme vám, můžete nastavit hello vypršení časových limitů.

## <a name="next-steps"></a>Další kroky
[Upgrade vaší aplikace pomocí prostředí PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.

Řídí, jak je upgrade vaší aplikace pomocí [upgrade parametry](service-fabric-application-upgrade-parameters.md).

Aby aplikace upgradů kompatibilní metodou učení jak toouse [serializace dat](service-fabric-application-upgrade-data-serialization.md).

Zjistěte, jak toouse pokročilé funkce při upgradu vaší aplikace tím, že odkazuje příliš[Advanced témata](service-fabric-application-upgrade-advanced.md).

Řešení běžných potíží v upgradů aplikací tím, že odkazuje toohello kroky v [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).

[image1]: media/service-fabric-application-upgrade-tutorial/upgrade7.png
[image2]: media/service-fabric-application-upgrade-tutorial/upgrade1.png
[image3]: media/service-fabric-application-upgrade-tutorial/upgrade5.png
[image4]: media/service-fabric-application-upgrade-tutorial/upgrade6.png
