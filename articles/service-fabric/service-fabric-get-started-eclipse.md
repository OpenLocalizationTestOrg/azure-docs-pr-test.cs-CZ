---
title: aaaAzure Service Fabric modulu plug-in pro Eclipse | Microsoft Docs
description: "Začínáme s hello Service Fabric modulu plug-in pro Eclipse."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2016
ms.author: saysa
ms.openlocfilehash: 4ba5a28a6282387249a2bd4e62314e891ff04162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a>Modul plug-in Service Fabric pro vývoj aplikací v Eclipse Javě
Eclipse je jednou z nejčastěji používá hello integrované vývojové prostředí (integrovaného vývojového prostředí) pro vývojáře v jazyce Java. V tomto článku jsme popisují, jak tooset do vašeho prostředí Eclipse vývoj prostředí toowork s Azure Service Fabric. Zjistěte, jak vytvořit aplikace Service Fabric tooinstall hello Service Fabric modul plug-in a nasazení vaší Service Fabric aplikace tooa místního nebo vzdáleného clusteru Service Fabric v prostředí Eclipse Neónová.

## <a name="install-or-update-hello-service-fabric-plug-in-in-eclipse-neon"></a>Nainstalujte nebo aktualizujte hello Service Fabric v prostředí Eclipse Neónová modulu plug-in
Modul plug-in Service Fabric můžete nainstalovat do Eclipse. Hello modul plug-in může zjednodušit proces vytváření a nasazení služeb Java hello.

1.  Ujistěte se, že máte nejnovější verzi Eclipse Neónová hello a hello nejnovější verzi Buildship nainstalován (1.0.17 nebo novější):
    -   toocheck hello verze nainstalované součásti v prostředí Eclipse Neónová přejděte příliš**pomoci** > **podrobné informace o instalaci**.
    -   tooupdate Buildship, najdete v části [Eclipse Buildship: Eclipse moduly plug-in pro Gradle][buildship-update].
    -   toocheck pro a nainstalovat aktualizace pro Eclipse Neónová přejděte příliš**pomoci** > **vyhledat aktualizace**.

2.  tooinstall hello Service Fabric modul plug-in, v prostředí Eclipse Neónová přejděte příliš**pomoci** > **instalace nového softwaru**.
  1.    V hello **pracovat s** zadejte **http://dl.microsoft.com/eclipse**.
  2.    Klikněte na tlačítko **Přidat**.

         ![Modul plug-in Service Fabric pro Eclipse Neon][sf-eclipse-plugin-install]
  3.    Vyberte hello Service Fabric modul plug-in a pak klikněte na tlačítko **Další**.
  4.    Dokončete kroky instalace hello a přijměte licenční podmínky softwaru společnosti Microsoft hello.

Pokud již máte hello Service Fabric modulu plug-in nainstalována, ujistěte se, že máte nejnovější verzi hello. toocheck dostupných aktualizací, přejděte příliš**pomoci** > **podrobné informace o instalaci**. V seznamu nainstalovaných modulů plug-in hello vyberte Service Fabric a pak klikněte na tlačítko **aktualizace**. Nainstalují se dostupné aktualizace.

> [!NOTE]
> Pokud instalace nebo aktualizace hello Service Fabric modul plug-in je pomalé, může to být kvůli tooan Eclipse nastavení. Eclipse shromažďuje metadata na všechny lokality tooupdate změny, které jsou registrovány s vaší instancí Eclipse. toospeed až hello proces vyhledávání a instalace modulu plug-in aktualizace Service Fabric, přejděte příliš**dostupných stránek softwaru**. Zrušte zaškrtnutí políček hello pro všechny weby s výjimkou hello ten, který ukazuje toohello Service Fabric modulu plug-in umístění (http://dl.microsoft.com/eclipse/azure/servicefabric).

## <a name="create-a-service-fabric-application-in-eclipse"></a>Vytvoření aplikace Service Fabric pomocí Eclipse

1.  V Eclipse Neónová přejděte příliš**soubor** > **nový** > **jiných**. Vyberte **Service Fabric Project** (Projekt Service Fabric) a potom klikněte na **Next** (Další).

    ![Nový projekt Service Fabric – stránka 1][create-application/p1]

2.  Zadejte název pro svůj projekt a potom klikněte na **Next** (Další).

    ![Nový projekt Service Fabric – stránka 2][create-application/p2]

3.  V seznamu hello šablon vyberte **šablony služby**. Vyberte typ šablony služby (Actor, Stateless, Container nebo Guest Binary) a potom klikněte na **Next** (Další).

    ![Nový projekt Service Fabric – stránka 3][create-application/p3]

4.  Zadejte název služby hello podrobnosti služby a pak klikněte na tlačítko **Dokončit**.

    ![Nový projekt Service Fabric – stránka 4][create-application/p4]

5. Když vytvoříte svůj první projekt Service Fabric v hello **otevřete přidružené hlediska** dialogové okno, klikněte na tlačítko **Ano**.

    ![Nový projekt Service Fabric – stránka 5][create-application/p5]

6.  Nový projekt vypadá takto:

    ![Nový projekt Service Fabric – stránka 6][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a>Vytvoření a nasazení aplikace Service Fabric v Eclipse

1.  Klikněte na novou aplikaci Service Fabric pravým tlačítkem a potom vyberte **Service Fabric**.

    ![Nabídka Service Fabric zobrazená po kliknutí pravým tlačítkem][publish/RightClick]

2. V podnabídce hello vyberte požadovanou možnost hello:
    -   Klikněte na tlačítko aplikace hello toobuild bez čištění, **sestavení aplikace**.
    -   Klikněte na tlačítko toodo čistá sestavení aplikace hello **znovu sestavte aplikaci**.
    -   Klikněte na tlačítko aplikace hello tooclean integrovaný artefaktů, **vyčištění aplikace**.

3.  V této nabídce můžete zvolit také nasazení, zrušení nasazení nebo publikování aplikace:
    -   toodeploy tooyour místní cluster, klikněte na tlačítko **nasazení aplikace**.
    -   V hello **publikovat aplikace** dialogové okno, vyberte profil publikování:
        -  **Local.json**
        -  **Cloud.json**

     Tyto soubory JavaScript Object Notation (JSON) ukládat informace (například koncové body připojení a informace o zabezpečení), které jsou požadované tooconnect tooyour místní nebo v cloudu (Azure) clusteru.

  ![Nabídka Publish (Publikovat) v Service Fabric][publish/Publish]

Toodeploy jiný způsob, jak vaše aplikace Service Fabric pomocí Eclipse spuštění konfigurace.

  1.    Přejděte příliš**spustit** > **konfigurace spuštění**.
  2.    V části **Gradle projektu**, vyberte hello **ServiceFabricDeployer** spustit konfigurace.
  3.    V pravém podokně hello na hello **argumenty** kartě pro **publishProfile**, vyberte **místní** nebo **cloudu**.  Výchozí hodnota Hello je **místní**. toodeploy tooa vzdálené nebo cloudu cluster, vyberte **cloudu**.
  4.    tooensure, správné informace hello se importují v hello publikační profily, upravit **Local.json** nebo **Cloud.json** podle potřeby. Můžete přidat nebo aktualizovat podrobnosti o koncových bodech a zabezpečovací přihlašovací údaje.
  5.    Ujistěte se, že **pracovní adresář** body toohello aplikace, které chcete toodeploy. toochange hello aplikace, klikněte na tlačítko hello **prostoru** tlačítko a pak vyberte aplikaci hello.
  6.    Klikněte na **Apply** (Použít) a potom na **Run** (Spustit).

Vaše aplikace se během chvilky sestaví a nasadí. Můžete sledovat stav nasazení hello v Service Fabric Exploreru.  

## <a name="add-a-service-fabric-service-tooyour-service-fabric-application"></a>Přidat Service Fabric service tooyour aplikace Service Fabric

tooadd Service Fabric service tooan existující aplikace Service Fabric, hello následující kroky:

1.  Klikněte pravým tlačítkem na hello projektu tooadd služby a pak klikněte na **Service Fabric**.

    ![Přidat službu Service Fabric – stránka 1][add-service/p1]

2.  Klikněte na tlačítko **přidat Service Fabric Service**, a dokončení hello sada kroků tooadd toohello projektu služby.
3.  Šablona služby vyberte hello tooadd tooyour projekt a pak klikněte na **Další**.

    ![Přidat službu Service Fabric – stránka 2][add-service/p2]

4.  Zadejte název služby hello (a další podrobnosti, podle potřeby) a pak klikněte na tlačítko hello **přidat službu** tlačítko.  

    ![Přidat službu Service Fabric – stránka 3][add-service/p3]

5.  Po přidání služby hello strukturu celkové projektu vypadá podobně jako toohello následující projektu:

    ![Přidat službu Service Fabric – stránka 4][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a>Úprava verzí manifestu aplikace Service Fabric v jazyce Java

verze manifestu tooedit, klikněte pravým tlačítkem na projekt hello, přejděte příliš**Service Fabric** a vyberte **upravit verze manifestu...**  z rozevírací nabídky hello. V Průvodci hello, můžete aktualizovat hello manifestu verze pro service manifest manifestu, aplikace a hello verze pro **kód**, **konfigurace** a **Data** balíčky.

Pokud zaškrtnete možnost hello **automatické aktualizaci aplikace a verze aktualizace service** a aktualizujte na verzi, pak se automaticky aktualizuje verze manifestu hello. toogive příklad, je nejprve vybrat hello políčko a potom aktualizaci hello verze **kód** verzi z 0.0.0 too0.0.1 a klikněte na **Dokončit**, pak službu verze manifestu a manifest aplikace verze bude automaticky aktualizovány too0.0.1.

## <a name="upgrade-your-service-fabric-java-application"></a>Upgrade aplikace Service Fabric v Javě

Scénář upgradu, můžete vytvořit hello **App1** projektu pomocí hello Service Fabric v prostředí Eclipse modulu plug-in. Provedli jste nasazení pomocí modulu plug-in toocreate hello aplikaci s názvem **fabric: / App1Application**. je typem aplikace Hello **App1AppicationType**, a verze aplikace hello je 1.0. Teď chcete tooupgrade aplikace bez přerušení dostupnosti.

Nejprve proveďte požadované změny tooyour aplikace a pak znovu vytvořte hello upravit služby. Aktualizace hello změnit služby soubor manifestu (ServiceManifest.xml) hello aktualizované verze pro hello služby (a kód, konfigurace nebo Data, jako je relevantní). Navíc upravte manifest aplikace hello (ApplicationManifest.xml) s hello aktualizovat číslo verze aplikace hello a hello upravené služby.  

tooupgrade vaší aplikace pomocí Neónová Eclipse, můžete vytvořit duplicitní spuštění konfiguračního profilu. Potom ho použít tooupgrade aplikace podle potřeby.

1.  Přejděte příliš**spustit** > **konfigurace spuštění**. V levém podokně hello, klikněte na tlačítko hello malou šipku toohello nalevo od **Gradle projektu**.
2.  Klikněte pravým tlačítkem na **ServiceFabricDeployer** a potom vyberte **Duplicate** (Duplikovat). Zadejte nový název pro tuto konfiguraci, třeba **ServiceFabricUpgrader**.
3.  V pravém panelu hello na hello **argumenty** změňte **- Pconfig = 'nasazení'** příliš**- Pconfig = 'upgradu,**a potom klikněte na **použít**.

Tento proces vytvoří a uloží spuštění konfigurační profil můžete použít na všechny čas tooupgrade vaší aplikace. Získá také hello nejnovější verze typu aktualizovanou aplikaci ze souboru manifestu aplikace hello.

upgrade aplikace Hello trvá několik minut. Můžete monitorovat hello upgradu aplikace v Service Fabric Exploreru.

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a>Migrace starého toobe aplikace Service Fabric Java použít s Maven
Nedávno jsme přesunuli knihovny Service Fabric Java z úložiště tooMaven Service Fabric Java SDK. Pokud je hello nové aplikace, který generovat pomocí prostředí Eclipse, vygeneruje nejnovější aktualizované projekty (které bude možné toowork s Maven), můžete aktualizovat existující Service Fabric bezstavové nebo objektu actor aplikací Java, které byly pomocí hello Service Fabric Java SDK starší, toouse hello Service Fabric Java závislostí z Maven. Postupujte podle kroků hello [sem](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure starší aplikace funguje s Maven.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
