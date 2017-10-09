---
title: "hello aaaPublish pružiny spouštěcí aplikace jako kontejner Docker pomocí nástrojů Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak hello toopublish tooMicrosoft webové aplikace Azure jako kontejner Docker pomocí nástrojů Azure pro IntelliJ."
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
ms.openlocfilehash: 8964cb33fd8f61a39f091633ae9074d9658232fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a>Publikování aplikace pružiny spouštěcí jako kontejner Docker pomocí hello Azure Toolkit pro IntelliJ

Hello [pružiny Framework] open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java. Jeden z hello oblíbených další projekty, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.

[Docker] je do řešení open source, který pomáhá vývojářům automatizovat nasazení hello, škálování a správu svých aplikací, které jsou spuštěny v kontejnerech.

Tento kurz vás provede kroky toodeploy hello pružiny spouštěcí aplikace jako tooMicrosoft kontejner Docker Azure pomocí hello Azure Toolkit pro IntelliJ.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repo"></a>Klonování úložiště Docker spouštěcí pružiny výchozí hello

Hello následující postup vás provede procesem klonování úložiště Docker spouštěcí pružiny hello pomocí IntelliJ. Pokud chcete toouse příkazového řádku, najdete v části [nasazení pružiny spuštění aplikace v systému Linux v Azure Container Service][Deploy Spring Boot on Linux in ACS].

1. Otevřete IntelliJ.

1. Na úvodní obrazovce hello vyberte hello **Githubu** možnost v hello **rezervovat z verzí** seznamu.

   ![Možnost GitHub pro správu verzí][CL01]

1. Zadejte přihlašovací údaje, pokud jste výzvami toolog v.

   * Pokud používáte toolog uživatelského jména a hesla v tooGitHub:

      ![Dialogové okno pro zadání Githubu uživatelské jméno a heslo][CL02a]

   * Pokud používáte tokenu toolog v tooGitHub:

      ![Dialogové okno pro zadání token Githubu][CL02b]

1. Zadejte **https://github.com/spring-guides/gs-spring-boot-docker.git** hello úložišti adresy URL, zadejte místní cestu a informace o složce a pak klikněte na tlačítko **klon**.

   ![Dialogové okno klonování úložiště][CL03]

1. Když se zobrazí výzva toocreate IntelliJ projekt, vyberte **ne**.

   ![Odmítnutí toocreate projektu IntelliJ][CL04]

1. Na úvodní stránku hello, klikněte na tlačítko **Importovat projekt**.

   ![Import výběru projektů][CL05]

1. Vyhledejte cestu hello, které jste naklonovali úložiště pružiny spouštěcí hello, vyberte hello **dokončení** ve složce hello kořenové a pak klikněte na tlačítko **OK**.

   ![Vyberte složku pro import][CL06]

1. Když se zobrazí výzva, vyberte **vytvoření projektu z existujících zdrojů**.

   ![Možnost toocreate projektu z existujících zdrojů][CL07]

1. Zadejte název projektu nebo přijměte výchozí hello, ověřte správnou cestu toohello hello **dokončení** složku a pak klikněte na tlačítko **Další**.

   ![Zadejte název projektu hello][CL08]

1. Přizpůsobit všechny adresáře pro import a pak klikněte na tlačítko **Další**.

   ![Zvolte adresáře][CL09]

1. Zkontrolujte tooimport hello knihovny a pak klikněte na tlačítko **Další**.

   ![Zkontrolujte projektu knihovny][CL10]

1. Zkontrolujte struktura hello modulu a pak klikněte na tlačítko **Další**.

   ![Zkontrolujte struktura modulu][CL11]

1. Zadejte vaše JDK a pak klikněte na tlačítko **Další**.

   ![Zadejte JDK][CL12]

1. Klikněte na **Dokončit**.

   ![Tlačítko Dokončit][CL13]

IntelliJ importuje hello pružiny spouštěcí aplikace jako projekt a zobrazí hello struktura po dokončení importu hello.

![Spuštění aplikace Spring v IntelliJ][CL14]

## <a name="build-your-spring-boot-app"></a>Sestavení vaší pružiny spouštěcí aplikace

### <a name="build-hello-app-by-using-hello-maven-pom"></a>Sestavení aplikace hello pomocí hello Maven POM

1. Pokud už není otevřený, otevřete okno nástroje Maven hello. Klikněte na tlačítko **zobrazení** > **nástroj Windows** > **projekty Maven**.

   ![Nástroje systému Windows a projekty Maven příkazy][BU01]

1. V okně nástroje Maven hello, klikněte pravým tlačítkem na **balíček** a vyberte **spustit Build Maven**. (Pokud projekt Maven nezobrazí automaticky, klikněte na tlačítko hello **znovu naimportovat** ikonu na panelu nástrojů hello Maven.)

   ![Spusťte příkaz Maven Build][BU02]

1. IntelliJ by se měl zobrazit **ÚSPĚŠNOSTI sestavení** zprávy při pružiny spouštěcí aplikace je úspěšně vytvořen.

   ![Zpráva ÚSPĚŠNOSTI sestavení][BU03]

### <a name="create-a-deployment-ready-artifact"></a>Vytvoření artefaktu připravené pro nasazení

toopublish pružiny spouštěcí aplikace, budete potřebovat toocreate artefaktů připravené pro nasazení. Použijte hello následující kroky:

1. Otevřete projekt webové aplikace v IntelliJ.

1. Klikněte na tlačítko **soubor**a potom klikněte na **strukturu projektu**.

   ![Struktura projekt – příkaz][ART01]

1. Klikněte na zelenou plus hello (**+**) symbolů tooadd artefakt, klikněte na tlačítko **JAR**a potom klikněte na **prázdný**.

   ![Přidat artefakt][ART02]

1. Název vašeho artefaktů přičemž zajistěte, aby není tooadd hello rozšíření ".jar" a pak zadejte hello cílovou složku pro výstupní Maven hello.

   ![Zadejte vlastnosti artefaktů][ART03]

1. Vytvoření manifestu pro vaše artefaktů (volitelné):

   a. Klikněte na tlačítko **vytvoření manifestu**.

      ![Klikněte na tlačítko Vytvořit Manifest hello][ART04a]

   b. Zvolte hello výchozí cesta pro hello artefaktů a pak klikněte na tlačítko **OK**.

      ![Zadejte cestu artefaktů][ART04b]

   c. Klikněte na tlačítko se třemi tečkami hello (**...** ) toolocate hello hlavní třídy.

      ![Vyhledejte hlavní – třída][ART04c]

   d. Vyberte hlavní třídy a pak klikněte na tlačítko **OK**.

      ![Zadejte hlavní – třída][ART04d]

1. Klikněte na **OK**.

   ![Zavřete dialogové okno projektu struktura hello][ART05]

> [!NOTE]
> Další informace o vytváření artefakty v IntelliJ najdete v tématu [konfigurace artefakty] na webu JetBrains hello.
>

### <a name="build-hello-artifact-for-deployment"></a>Sestavení hello artefaktů pro nasazení

1. Klikněte na tlačítko **sestavení**a potom klikněte na **artefakty**.

   ![Příkaz artefaktů sestavení][BU04]

1. Když hello **sestavení artefaktů** místní nabídce klikněte na tlačítko **sestavení**.

   ![Sestavení artefaktů kontextové nabídky][BU05]

IntelliJ by měl zobrazovat hello dokončit artefaktů pro vaši aplikaci pružiny spouštěcí v okně nástroje projektu hello.

   ![Vytvořený artefaktů][BU06]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>Publikovat tooAzure vaší webové aplikace pomocí kontejner Docker

1. Pokud nejste přihlášeni tooyour účet Azure, postupujte podle kroků hello v [přihlášení pokyny pro hello Azure Toolkit IntelliJ][Azure Sign In for IntelliJ].

1. V okně nástroje hello prohlížeči projektu klikněte pravým tlačítkem na projekt hello a vyberte **Azure** > **publikovat jako kontejner Docker**.

   ![Publikovat jako příkaz kontejner Docker][PU01]

1. Když hello **nasadit kontejner Docker v Azure** se zobrazí dialogové okno, jsou zobrazeny všechny existující hostitelů Docker. Pokud si zvolíte toodeploy tooan existující hostitele, můžete přeskočit toostep 4. Jinak použijte následující kroky toocreate hostitele hello:

   a. Klikněte na zelenou plus hello (**+**) symbol.

      ![Přidat nový hostitel Docker][PU02]

   b. Když hello **vytvořit hostitelů Docker** zobrazí se dialogové okno, můžete zvolit výchozí hello tooaccept nebo můžete zadat vlastní nastavení pro nové Docker hostitele. (Podrobný popis hello různá nastavení, najdete v části [publikovat webovou aplikaci jako kontejner Docker pomocí hello Azure Toolkit pro IntelliJ][Publish Container with Azure Toolkit].) Klikněte na tlačítko **Další** jste zadali při které toouse nastavení.

      ![Zadejte možnosti hostitelů Docker][PU03a]

   c. Můžete vybrat toouse existující přihlašovací údaje z trezoru služby Azure klíče nebo můžete tooenter nové Docker přihlašovací údaje. Klikněte na tlačítko **Dokončit** když nastavíte možnosti.

      ![Zadejte přihlašovací údaje hostitelů Docker][PU03b]

1. Vyberte Docker hostiteli a pak klikněte na **Další**.

   ![Vyberte toouse hostitelů Docker hello][PU04]

1. Na poslední stránce hello hello **nasadit kontejner Docker v Azure** dialogovém okně zadejte hello následující možnosti:

   a. Můžete toospecify vlastní název pro hello kontejneru, který bude hostovat vaše kontejner Docker nebo můžete přijmout výchozí hello.

   b. Zadejte porty TCP hello pro svého hostitele docker pomocí následující syntaxe hello: *[externí port]*:*[interní port]*. Například **80:8080** určuje externí port 80 a hello výchozí vnitřní pružiny spouštěcí port 8080.
   
      Pokud jste upravili interní port (například úpravou souboru application.yml hello), je třeba číslo portu hello toospecify pro správné směrování toooccur hello v Azure.

   c. Po nakonfigurování těchto možností, klikněte na tlačítko **Dokončit**.

   ![Nasadit kontejner Docker v Azure][PU05]

1. Po dokončení publikování hello Azure Toolkit hello protokol činnosti Azure zobrazí **publikováno** hello stavu.

   ![Byla úspěšně nasazena hostitelů Docker][PU06]

## <a name="next-steps"></a>Další kroky

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

toolearn o další metody pro vytváření aplikací pružiny spouštěcí pomocí IntelliJ, najdete v části [vytváření projektů spouštěcí pružiny](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) na webu JetBrains hello.

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[konfigurace artefakty]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny Framework]: https://spring.io/

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
