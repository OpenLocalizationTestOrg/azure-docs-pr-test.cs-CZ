---
title: "Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí nástrojů Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak publikovat webovou aplikaci do služby Microsoft Azure jako kontejner Docker pomocí nástrojů Azure pro Eclipse."
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
ms.openlocfilehash: fcb60fcfbda26f5f37bfb0edcb01f8737188b6bc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a>Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí nástrojů Azure pro Eclipse

[Pružiny Framework] open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java. Jedním z dalších oblíbených projektů, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.

[Docker] je do řešení open source, který pomáhá vývojářům automatizovat nasazení, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.

Tento kurz vás provede kroky nasazení spouštěcí pružiny aplikace jako kontejner Docker do služby Microsoft Azure pomocí sady nástrojů Azure pro Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a>Naklonujte úložiště Docker spouštěcí pružiny výchozí

### <a name="import-the-public-repository"></a>Import veřejného úložiště

Následující postup vás provede procesem klonování úložiště Docker spouštěcí pružiny do místního počítače pomocí IntelliJ. Pokud chcete použít příkazový řádek, přečtěte si téma [nasazení pružiny spuštění aplikace v systému Linux v Azure Container Service][Deploy Spring Boot on Linux in ACS].

1. Otevřete prostředí Eclipse.

1. Klikněte na tlačítko **soubor** > **Import**.

   ![Nabídka Soubor importu][CL01]

1. Když **Import** otevře se dialogové okno:

   a. Rozbalte položku **Git**.

   b. Vyberte **projekty z Gitu**.
   
   c. Klikněte na **Další**.

   ![Dialog importovat][CL02]

1. Na **vybrat zdroj úložiště** stránky:

   a. Vyberte **klonovat URI**.
   
   b. Klikněte na **Další**.

   ![Vyberte zdrojové úložiště stránky][CL03]

1. Na **zdrojové úložiště Git** stránky:

   a. Pro **URI**, zadejte `https://github.com/spring-guides/gs-spring-boot-docker.git`. Tento krok by měl automaticky vyplnit **hostitele** a **úložiště cesta** pole s správné hodnoty.
   
   b. Spouštěcí pružiny úložiště je veřejný, proto by nemělo být zadejte Git uživatelské jméno a heslo.
   
   c. Klikněte na **Další**.

   ![Zdrojové úložiště Git stránky][CL04]

1. Na **výběr větve** klikněte na tlačítko **Další**.

   ![Stránka Výběr větve][CL05]

1. Na **místní cílovou** stránky:

   a. Zadejte místní složku, kam chcete vaše místní úložiště.
   
   b. Klikněte na **Další**.

   ![Místní cílová stránka][CL06]

1. Na **vyberte Průvodce pro import projektů** stránky:

   a. Vyberte **Import jako obecné projekt**.
   
   b. Klikněte na **Další**.

   !["Vyberte průvodce pro import projektů" stránky][CL07]

1. Na **Import projektů** stránky:

   a. Zadejte název projektu.
   
   b. Klikněte na **Dokončit**.

   ![Import projektů stránky][CL08]

1. Pokud úložiště je úspěšně klonovat, zobrazí se všechny soubory, které jsou uvedené v Eclipse.

   ![Místní úložiště][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a>Vytvořte projekt Maven z místního úložiště

Úložiště Docker spouštěcí pružiny obsahuje dokončený projekt Maven, které budete používat pro účely tohoto kurzu. 

1. Klikněte na tlačítko **soubor** > **Import**.

   ![Příkaz Import v nabídce Soubor][CL01]

1. Když **Import** otevře se dialogové okno:

   a. Rozbalte položku **Maven**.
   
   b. Vyberte **existující projekty Maven**.
   
   c. Klikněte na **Další**.

   ![Dialog importovat][MV01]

1. Na **projekty Maven** stránky:

   a. Pro **kořenový adresář**, zadejte **dokončení** složky v místním úložišti.
   
   b. Rozbalte **Upřesnit** a zadejte vlastní název pro **šablony názvu**.
   
   c. Vyberte pole **pom.xml** v projektu.
   
   d. Klikněte na **Dokončit**.

   ![Stránka projekty maven][MV02]

1. Pokud projekt Maven byl úspěšně otevřen, uvidíte druhý projekt uvedené v Eclipse.

   ![Místní projekt Maven][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a>Sestavení aplikace pružiny spouštěcí pomocí Maven

1. V prohlížeči projektu Eclipse vyberte projekt Maven.

1. Klikněte na tlačítko **spustit** > **spustit jako** > **sestavení Maven**.

   ![Příkazy ke spuštění jako Maven sestavení][BU01]

1. Pokud vaše aplikace je úspěšně vytvořen, v okně konzoly zobrazuje stav.

   ![Úspěšný build Maven][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>Publikování webové aplikace do Azure pomocí kontejner Docker

1. V prohlížeči projektu Eclipse vyberte projekt Maven.

1. Klikněte na tlačítko Azure **publikovat** nabídce a pak klikněte na tlačítko **publikovat jako kontejner Docker**.

   ![Publikovat jako příkaz kontejner Docker][PU01]

1. Když **nasazení kontejner Docker v Azure** zobrazí se dialogové okno:

   a. Zadejte název vlastního Docker bitové kopie.
   
   b. Pro **artefaktů nasazení**, zadejte cestu k **gs pružiny spouštěcí docker-0.1.0.jar** souborů, které jste právě vytvořili.

   ![Zadejte možnosti Docker][PU02]

   Zobrazí se všechny existující hostitelů Docker. 

1. Pokud se rozhodnete nasadit do existující hostitele, můžete přeskočit ke kroku 5. Jinak použijte následující kroky pro vytvoření hostitele:

   a. Klikněte na tlačítko **Přidat**.

      ![Přidat nový hostitel Docker][PU03]

   b. Když **vytvořit hostitelů Docker** zobrazí se dialogové okno, můžete přijmout výchozí hodnoty nebo můžete zadat vlastní nastavení pro nové Docker hostitele. (Podrobný popis různá nastavení, najdete v části [publikovat webovou aplikaci jako kontejner Docker pomocí nástrojů Azure pro IntelliJ][Publish Container with Azure Toolkit].) Klikněte na tlačítko **Další** jste zadali při nastavení, které chcete použít.

      ![Zadejte možnosti hostitelů Docker][PU04]

   c. Můžete použít existující přihlašovací údaje z trezoru služby Azure klíče, nebo můžete zadat nový Docker přihlašovací údaje. Klikněte na tlačítko **Dokončit** když nastavíte možnosti.

      ![Zadejte přihlašovací údaje hostitelů Docker][PU05]

1. Vyberte Docker hostiteli a pak klikněte na **Další**.

   ![Vyberte hostitele Docker používat][PU06]

1. Na poslední stránce **nasazení kontejner Docker v Azure** dialogové okno pole, určete následující možnosti:

   a. Můžete zadat vlastní název kontejneru, který bude hostovat vaše kontejner Docker nebo můžete přijmout výchozí nastavení.

   b. Zadejte porty protokolu TCP pro svého hostitele docker pomocí následující syntaxe: *[externí port]*:*[interní port]*. Například **80:8080** určuje externí port 80 a výchozí vnitřní pružiny spouštěcí port 8080.
   
      Pokud jste upravili interní port (například úpravou souboru application.yml), je třeba zadat číslo portu pro správné směrování v Azure.

   c. Když nakonfigurujete tyto možnosti, klikněte na tlačítko **Dokončit**.

   ![Nasadit kontejner Docker v Azure][PU07]

1. Po dokončení publikování sady nástrojů Azure protokol činnosti Azure zobrazí **publikováno** stavu.

   ![Byla úspěšně nasazena hostitelů Docker][PU08]

## <a name="next-steps"></a>Další kroky

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[Pružiny Framework]: https://spring.io/

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
