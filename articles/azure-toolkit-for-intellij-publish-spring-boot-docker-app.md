---
title: "Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí nástrojů Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak publikovat webovou aplikaci do služby Microsoft Azure jako kontejner Docker pomocí nástrojů Azure pro IntelliJ."
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
ms.openlocfilehash: b771238934183c953615ac33c42a275d80657556
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a>Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí nástrojů Azure pro IntelliJ

[Pružiny Framework] open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java. Jedním z dalších oblíbených projektů, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.

[Docker] je do řešení open source, který pomáhá vývojářům automatizovat nasazení, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.

Tento kurz vás provede kroky nasazení spouštěcí pružiny aplikace jako kontejner Docker do služby Microsoft Azure pomocí sady nástrojů Azure pro IntelliJ.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repo"></a>Naklonujte úložiště Docker spouštěcí pružiny výchozí

Následující postup vás provede procesem klonování úložiště Docker spouštěcí pružiny pomocí IntelliJ. Pokud chcete použít příkazový řádek, přečtěte si téma [nasazení pružiny spuštění aplikace v systému Linux v Azure Container Service][Deploy Spring Boot on Linux in ACS].

1. Otevřete IntelliJ.

1. Na úvodní obrazovce, vyberte **Githubu** možnost **rezervovat z verzí** seznamu.

   ![Možnost GitHub pro správu verzí][CL01]

1. Zadejte přihlašovací údaje, pokud se zobrazí výzva k přihlášení.

   * Pokud používáte uživatelské jméno a heslo pro přihlášení ke Githubu:

      ![Dialogové okno pro zadání Githubu uživatelské jméno a heslo][CL02a]

   * Pokud používáte token do protokolu Githubu:

      ![Dialogové okno pro zadání token Githubu][CL02b]

1. Zadejte **https://github.com/spring-guides/gs-spring-boot-docker.git** úložišti adresy URL, zadejte místní cestu a informace o složce a pak klikněte na tlačítko **klon**.

   ![Dialogové okno klonování úložiště][CL03]

1. Když se zobrazí výzva k vytvoření projektu IntelliJ, vyberte **ne**.

   ![Odmítnutí k vytvoření projektu IntelliJ][CL04]

1. Na úvodní stránce klikněte na tlačítko **Importovat projekt**.

   ![Import výběru projektů][CL05]

1. Vyhledejte cestu, do které jste naklonovali úložiště pružiny spouštěcí, vyberte **dokončení** složku kořenové a pak klikněte na tlačítko **OK**.

   ![Vyberte složku pro import][CL06]

1. Když se zobrazí výzva, vyberte **vytvoření projektu z existujících zdrojů**.

   ![Možnost vytváření projektů z existujícího zdroje][CL07]

1. Zadejte název projektu nebo přijměte výchozí nastavení, ověřte správnou cestu **dokončení** složku a pak klikněte na tlačítko **Další**.

   ![Zadejte název projektu][CL08]

1. Přizpůsobit všechny adresáře pro import a pak klikněte na tlačítko **Další**.

   ![Zvolte adresáře][CL09]

1. Zkontrolujte knihoven importovat, a pak klikněte na **Další**.

   ![Zkontrolujte projektu knihovny][CL10]

1. Zkontrolujte struktura modulu a pak klikněte na tlačítko **Další**.

   ![Zkontrolujte struktura modulu][CL11]

1. Zadejte vaše JDK a pak klikněte na tlačítko **Další**.

   ![Zadejte JDK][CL12]

1. Klikněte na **Dokončit**.

   ![Tlačítko Dokončit][CL13]

IntelliJ importuje pružiny spouštěcí aplikace jako projekt a zobrazí strukturu po dokončení importu.

![Spuštění aplikace Spring v IntelliJ][CL14]

## <a name="build-your-spring-boot-app"></a>Sestavení vaší pružiny spouštěcí aplikace

### <a name="build-the-app-by-using-the-maven-pom"></a>Sestavení aplikace pomocí Maven POM

1. Pokud už není otevřený, otevřete okno nástroje Maven. Klikněte na tlačítko **zobrazení** > **nástroj Windows** > **projekty Maven**.

   ![Nástroje systému Windows a projekty Maven příkazy][BU01]

1. V okně nástroje Maven, klikněte pravým tlačítkem na **balíček** a vyberte **spustit Build Maven**. (Pokud projekt Maven nezobrazí automaticky, klikněte **znovu naimportovat** ikonu na panelu nástrojů Maven.)

   ![Spusťte příkaz Maven Build][BU02]

1. IntelliJ by se měl zobrazit **ÚSPĚŠNOSTI sestavení** zprávy při pružiny spouštěcí aplikace je úspěšně vytvořen.

   ![Zpráva ÚSPĚŠNOSTI sestavení][BU03]

### <a name="create-a-deployment-ready-artifact"></a>Vytvoření artefaktu připravené pro nasazení

K publikování aplikace pružiny spouštěcí, musíte vytvořit artefakt připravené pro nasazení. Použijte k tomu následující postup:

1. Otevřete projekt webové aplikace v IntelliJ.

1. Klikněte na tlačítko **soubor**a potom klikněte na **strukturu projektu**.

   ![Struktura projekt – příkaz][ART01]

1. Klikněte na zelenou plus (**+**) symbol, který chcete přidat artefakt, klikněte na tlačítko **JAR**a potom klikněte na **prázdný**.

   ![Přidat artefakt][ART02]

1. Zadejte název vaší artefaktů při vytváření nezapomeňte přidat příponu ".jar" a pak zadejte cílovou složku pro výstupní Maven.

   ![Zadejte vlastnosti artefaktů][ART03]

1. Vytvoření manifestu pro vaše artefaktů (volitelné):

   a. Klikněte na tlačítko **vytvoření manifestu**.

      ![Klikněte na tlačítko Vytvořit Manifest][ART04a]

   b. Výchozí cesta pro artefaktu a potom na tlačítko **OK**.

      ![Zadejte cestu artefaktů][ART04b]

   c. Klikněte na tlačítko se třemi tečkami (**...** ) najít hlavní třídy.

      ![Vyhledejte hlavní – třída][ART04c]

   d. Vyberte hlavní třídy a pak klikněte na tlačítko **OK**.

      ![Zadejte hlavní – třída][ART04d]

1. Klikněte na **OK**.

   ![Zavřete dialogové okno struktura projektu][ART05]

> [!NOTE]
> Další informace o vytváření artefakty v IntelliJ najdete v tématu [konfigurace artefakty] na webu JetBrains.
>

### <a name="build-the-artifact-for-deployment"></a>Sestavení artefaktů pro nasazení

1. Klikněte na tlačítko **sestavení**a potom klikněte na **artefakty**.

   ![Příkaz artefaktů sestavení][BU04]

1. Když **sestavení artefaktů** místní nabídce klikněte na tlačítko **sestavení**.

   ![Sestavení artefaktů kontextové nabídky][BU05]

IntelliJ by měl zobrazit dokončené artefakt Spring spuštění aplikace v okně nástroje projektu.

   ![Vytvořený artefaktů][BU06]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>Publikování webové aplikace do Azure pomocí kontejner Docker

1. Pokud nejste přihlášeni k účtu Azure, postupujte podle kroků v [přihlášení pokyny pro Azure Toolkit IntelliJ][Azure Sign In for IntelliJ].

1. V okně nástroje prohlížeči projektu klikněte pravým tlačítkem na projekt a potom vyberte **Azure** > **publikovat jako kontejner Docker**.

   ![Publikovat jako příkaz kontejner Docker][PU01]

1. Když **nasadit kontejner Docker v Azure** se zobrazí dialogové okno, jsou zobrazeny všechny existující hostitelů Docker. Pokud se rozhodnete nasadit do existující hostitele, můžete přeskočit ke kroku 4. Jinak použijte následující kroky pro vytvoření hostitele:

   a. Klikněte na zelenou plus (**+**) symbol.

      ![Přidat nový hostitel Docker][PU02]

   b. Když **vytvořit hostitelů Docker** zobrazí se dialogové okno, můžete přijmout výchozí hodnoty nebo můžete zadat vlastní nastavení pro nové Docker hostitele. (Podrobný popis různá nastavení, najdete v části [publikovat webovou aplikaci jako kontejner Docker pomocí nástrojů Azure pro IntelliJ][Publish Container with Azure Toolkit].) Klikněte na tlačítko **Další** jste zadali při nastavení, které chcete použít.

      ![Zadejte možnosti hostitelů Docker][PU03a]

   c. Můžete použít existující přihlašovací údaje z trezoru služby Azure klíče, nebo můžete zadat nový Docker přihlašovací údaje. Klikněte na tlačítko **Dokončit** když nastavíte možnosti.

      ![Zadejte přihlašovací údaje hostitelů Docker][PU03b]

1. Vyberte Docker hostiteli a pak klikněte na **Další**.

   ![Vyberte hostitel Docker používat][PU04]

1. Na poslední stránce **nasadit kontejner Docker v Azure** dialogové okno pole, určete následující možnosti:

   a. Můžete zadat vlastní název kontejneru, který bude hostovat vaše kontejner Docker nebo můžete přijmout výchozí nastavení.

   b. Zadejte porty protokolu TCP pro svého hostitele docker pomocí následující syntaxe: *[externí port]*:*[interní port]*. Například **80:8080** určuje externí port 80 a výchozí vnitřní pružiny spouštěcí port 8080.
   
      Pokud jste upravili interní port (například úpravou souboru application.yml), je třeba zadat číslo portu pro správné směrování v Azure.

   c. Po nakonfigurování těchto možností, klikněte na tlačítko **Dokončit**.

   ![Nasadit kontejner Docker v Azure][PU05]

1. Po dokončení publikování sady nástrojů Azure protokol činnosti Azure zobrazí **publikováno** stavu.

   ![Byla úspěšně nasazena hostitelů Docker][PU06]

## <a name="next-steps"></a>Další kroky

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

Další informace o další metody pro vytváření aplikací pružiny spouštěcí pomocí IntelliJ najdete v tématu [vytváření projektů spouštěcí pružiny](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) na webu JetBrains.

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[konfigurace artefakty]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[Pružiny Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png
