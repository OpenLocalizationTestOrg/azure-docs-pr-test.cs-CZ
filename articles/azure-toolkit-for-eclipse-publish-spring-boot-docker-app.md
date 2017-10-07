---
title: "hello aaaPublish pružiny spouštěcí aplikace jako kontejner Docker pomocí nástrojů Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak hello toopublish tooMicrosoft webové aplikace Azure jako kontejner Docker pomocí nástrojů Azure pro prostředí Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/21/2017
ms.author: robmcm
ms.openlocfilehash: 29390c49c339a1ebb87cb3951b21cea01c0da15f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a>Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí hello Azure Toolkit pro Eclipse

Hello [pružiny Framework] open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java. Jeden z hello oblíbených další projekty, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.

[Docker] je do řešení open source, který pomáhá vývojářům automatizovat nasazení hello, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.

Tento kurz vás provede kroky toodeploy hello pružiny spouštěcí aplikace jako tooMicrosoft kontejner Docker Azure pomocí hello Azure Toolkit pro Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repository"></a>Klonování úložiště Docker spouštěcí pružiny výchozí hello

### <a name="import-hello-public-repository"></a>Import hello veřejného úložiště

Hello následující postup vás provede procesem klonování hello pružiny spouštěcí Docker úložiště tooyour místního počítače pomocí IntelliJ. Pokud chcete toouse příkazového řádku, najdete v části [nasazení pružiny spuštění aplikace v systému Linux v Azure Container Service][Deploy Spring Boot on Linux in ACS].

1. Otevřete prostředí Eclipse.

1. Klikněte na tlačítko **soubor** > **Import**.

   ![Nabídka Soubor importu][CL01]

1. Když hello **Import** otevře se dialogové okno:

   a. Rozbalte položku **Git**.

   b. Vyberte **projekty z Gitu**.
   
   c. Klikněte na **Další**.

   ![Dialog importovat][CL02]

1. Na hello **vybrat zdroj úložiště** stránky:

   a. Vyberte **klonovat URI**.
   
   b. Klikněte na **Další**.

   ![Vyberte zdrojové úložiště stránky][CL03]

1. Na hello **zdrojové úložiště Git** stránky:

   a. Pro **URI**, zadejte `https://github.com/spring-guides/gs-spring-boot-docker.git`. Tento krok by měl automaticky vyplnit hello **hostitele** a **úložiště cesta** pole s hello opravte hodnoty.
   
   b. Hello pružiny spouštěcí úložiště je veřejný, takže by neměla mít tooenter Git uživatelské jméno a heslo.
   
   c. Klikněte na **Další**.

   ![Zdrojové úložiště Git stránky][CL04]

1. Na hello **výběr větve** klikněte na tlačítko **Další**.

   ![Stránka Výběr větve][CL05]

1. Na hello **místní cílovou** stránky:

   a. Zadejte hello místní složku, kam chcete vaše místní úložiště.
   
   b. Klikněte na **Další**.

   ![Místní cílová stránka][CL06]

1. Na hello **vyberte toouse Průvodce pro import projektů** stránky:

   a. Vyberte **Import jako obecné projekt**.
   
   b. Klikněte na **Další**.

   !["Vyberte toouse Průvodce pro import projektů" stránky][CL07]

1. Na hello **Import projektů** stránky:

   a. Zadejte název projektu.
   
   b. Klikněte na **Dokončit**.

   ![Import projektů stránky][CL08]

1. Pokud hello úložiště se úspěšně klonovat, uvidíte všechny soubory hello uvedené v Eclipse.

   ![Místní úložiště][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a>Vytvořte projekt Maven z místního úložiště

úložiště Docker spouštěcí pružiny Hello obsahuje dokončený projekt Maven, které budete používat pro účely tohoto kurzu. 

1. Klikněte na tlačítko **soubor** > **Import**.

   ![Příkaz v nabídce Soubor hello import][CL01]

1. Když hello **Import** otevře se dialogové okno:

   a. Rozbalte položku **Maven**.
   
   b. Vyberte **existující projekty Maven**.
   
   c. Klikněte na **Další**.

   ![Dialog importovat][MV01]

1. Na hello **projekty Maven** stránky:

   a. Pro **kořenový adresář**, zadejte hello **dokončení** složky v místním úložišti.
   
   b. Rozbalte hello **Upřesnit** a zadejte vlastní název pro **šablony názvu**.
   
   c. Vyberte hello pole hello **pom.xml** soubor v projektu hello.
   
   d. Klikněte na **Dokončit**.

   ![Stránka projekty maven][MV02]

1. Pokud projekt Maven hello byl úspěšně otevřen, uvidíte druhý projekt uvedené v Eclipse.

   ![Místní projekt Maven][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a>Sestavení aplikace pružiny spouštěcí pomocí Maven

1. V prohlížeči projektu Eclipse hello vyberte projekt Maven hello.

1. Klikněte na tlačítko **spustit** > **spustit jako** > **sestavení Maven**.

   ![Příkazy toorun jako sestavení Maven][BU01]

1. Pokud vaše aplikace je úspěšně vytvořená, se hello okna konzoly zobrazuje stav hello.

   ![Úspěšný build Maven][BU02]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>Publikovat tooAzure vaší webové aplikace pomocí kontejner Docker

1. V prohlížeči projektu Eclipse hello vyberte projekt Maven hello.

1. Klikněte na tlačítko hello Azure **publikovat** nabídce a pak klikněte na tlačítko **publikovat jako kontejner Docker**.

   ![Publikovat jako příkaz kontejner Docker][PU01]

1. Když hello **nasazení kontejner Docker v Azure** zobrazí se dialogové okno:

   a. Zadejte název vlastního Docker bitové kopie.
   
   b. Pro **artefaktů toodeploy**, zadejte cestu toohello hello **gs pružiny spouštěcí docker-0.1.0.jar** souborů, které jste právě vytvořili.

   ![Zadejte možnosti Docker][PU02]

   Zobrazí se všechny existující hostitelů Docker. 

1. Pokud si zvolíte toodeploy tooan existující hostitele, můžete přeskočit toostep 5. Jinak použijte následující kroky toocreate hostitele hello:

   a. Klikněte na tlačítko **Přidat**.

      ![Přidat nový hostitel Docker][PU03]

   b. Když hello **vytvořit hostitelů Docker** zobrazí se dialogové okno, můžete zvolit výchozí hello tooaccept nebo můžete zadat vlastní nastavení pro nové Docker hostitele. (Podrobný popis hello různá nastavení, najdete v části [publikovat webovou aplikaci jako kontejner Docker pomocí hello Azure Toolkit pro IntelliJ][Publish Container with Azure Toolkit].) Klikněte na tlačítko **Další** jste zadali při které toouse nastavení.

      ![Zadejte možnosti hostitelů Docker][PU04]

   c. Můžete vybrat toouse existující přihlašovací údaje z trezoru služby Azure klíče nebo můžete tooenter nové Docker přihlašovací údaje. Klikněte na tlačítko **Dokončit** když nastavíte možnosti.

      ![Zadejte přihlašovací údaje hostitelů Docker][PU05]

1. Vyberte Docker hostiteli a pak klikněte na **Další**.

   ![Vyberte hostitele toouse Docker][PU06]

1. Na poslední stránce hello hello **nasazení kontejner Docker v Azure** dialogovém okně zadejte hello následující možnosti:

   a. Můžete toospecify vlastní název pro hello kontejneru, který bude hostovat vaše kontejner Docker nebo můžete přijmout výchozí hello.

   b. Zadejte porty TCP hello pro svého hostitele docker pomocí následující syntaxe hello: *[externí port]*:*[interní port]*. Například **80:8080** určuje externí port 80 a hello výchozí vnitřní pružiny spouštěcí port 8080.
   
      Pokud jste upravili interní port (například úpravou souboru application.yml hello), je třeba číslo portu hello toospecify pro správné směrování toooccur hello v Azure.

   c. Když nakonfigurujete tyto možnosti, klikněte na tlačítko **Dokončit**.

   ![Nasadit kontejner Docker v Azure][PU07]

1. Po dokončení publikování hello Azure Toolkit hello protokol činnosti Azure zobrazí **publikováno** hello stavu.

   ![Byla úspěšně nasazena hostitelů Docker][PU08]

## <a name="next-steps"></a>Další kroky

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png
