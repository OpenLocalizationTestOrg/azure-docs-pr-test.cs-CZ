---
title: "aaaDeploy toohello pružiny spouštění aplikací Azure App Service | Microsoft Docs"
description: "V tomto kurzu Průvodce vývojáře prostřednictvím hello kroky toodeploy hello pružiny spouštěcí Začínáme webové aplikace tooAzure služby App Service."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: 69f9c4903fd740125194402cdb4b4db46a1f2773
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-toohello-azure-app-service"></a>Nasazení aplikace spouštěcí pružiny toohello Azure App Service

Hello  **[pružiny Framework]**  open-source řešení, která pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java a jeden z hello oblíbených další projekty, které je postavená na této platformě je [Pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.

Tento kurz vás provede, když vytvoření hello ukázkovou pružiny spouštěcí Začínáme webovou aplikaci a jeho nasazení příliš[Azure App Service].

### <a name="prerequisites"></a>Požadavky

Pořadí toocomplete hello kroky v tomto kurzu je třeba toohave hello následující:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].
* Aktuální [Java Developer Kit (JDK)].
* Apache na [Maven] sestavení nástroj (verze 3).
* A [Git] klienta.

## <a name="create-hello-spring-boot-getting-started-web-app"></a>Vytvoření hello pružiny spouštěcí Začínáme webové aplikace

Hello následující postup vás provede kroky hello, které jsou požadované toocreate jednoduchou webovou aplikaci pružiny spouštěcí a otestovat ji místně.

1. Otevřete příkazový řádek a vytvářet toohold místního adresáře aplikace a změnit adresář toothat; například:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --nebo--
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klon hello [pružiny spouštěcí Začínáme] ukázkový projekt do adresáře hello jste právě vytvořili; například:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. Změnit adresář toohello dokončit projekt; například:
   ```
   cd gs-spring-boot
   cd complete
   ```

1. Vytvořit soubor JAR hello pomocí nástroje Maven; například:
   ```
   mvn package
   ```

1. Po vytvoření webové aplikace hello změnit soubor JAR toohello adresáře a spusťte hello webové aplikace; například:
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. Test webové aplikace hello procházení toohttp://localhost:8080 pomocí webového prohlížeče, nebo použijte syntaxi hello jako následující příklad, pokud máte k dispozici curl hello:
   ```
   curl http://localhost:8080
   ```

1. Měli byste vidět hello následující zpráva: **pozdrav z jara spouštěcí!**

   ![Procházet ukázkové aplikace][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a>Vytvoření webové aplikace Azure pro použití s Javou

Hello následující kroky vás provedou kroky toocreate hello webové aplikace Azure, nakonfigurujete hello požadované nastavení pro jazyk Java a konfigurovat přihlašovací údaje serveru FTP.

1. Procházet toohello [portál Azure] a přihlaste se.

1. Po přihlášení ke svému účtu na hello portálu Azure klikněte na ikonu hello nabídky pro **App Services**:
   
   ![portál Azure][AZ01]

1. Když hello **App Services** stránky se zobrazí, klikněte na tlačítko **+ přidat** toocreate nové služby App Service.

   ![Vytvoření služby App Service][AZ02]

1. Když se zobrazí seznam hello šablony webové aplikace, klikněte na odkaz hello hello základní webové aplikace Microsoft.

   ![Šablony webové aplikace][AZ03]

1. Když se zobrazí stránka informace o hello hello šablony webové aplikace, klikněte na tlačítko **vytvořit**.

   ![Vytvoření webové aplikace][AZ04]

1. Zadejte jedinečný název pro vaši webovou aplikaci a zadejte jakákoli další nastavení a potom **vytvořit**.

   ![Vytvoření nastavení webové aplikace][AZ05]

1. Po vytvoření webové aplikace, klikněte na ikonu nabídky hello **App Services**a potom klikněte na nově vytvořené webové aplikace:

   ![Seznam webových aplikací][AZ06]

1. Když webové aplikace se zobrazí, zadejte verzi Javy hello pomocí hello následující kroky:

   a. Klikněte na tlačítko hello **nastavení aplikace** položku nabídky.

   b. Zvolte **Java 8** pro verzi Javy hello.

   c. Zvolte **nejnovější** pro hello podverzi Javy.

   d. Zvolte **nejnovější 8.5 Tomcat** pro hello webový kontejner. (Tento kontejner ve skutečnosti se nepoužijí; Azure bude používat hello kontejneru z vaší aplikace pružiny spouštěcí.)

   e. Klikněte na **Uložit**.

   ![Nastavení aplikace][AZ07]

1. Zadejte přihlašovací údaje nasazení FTP pomocí hello následující kroky:

   a. Klikněte na tlačítko hello **přihlašovací údaje nasazení** položku nabídky.

   b. Zadejte uživatelské jméno a heslo.

   c. Klikněte na **Uložit**.

   ![Zadejte přihlašovací údaje nasazení.][AZ08]

1. Načíst informace o připojení FTP pomocí hello následující kroky:

   a. Klikněte na tlačítko hello **přihlašovací údaje nasazení** položku nabídky.

   b. Úplné uživatelské jméno FTP a adresu URL zkopírovat a uložit pro hello další části tohoto kurzu.

   ![Adresy URL FTP a přihlašovací údaje][AZ09]

## <a name="deploy-your-spring-boot-web-app-tooazure"></a>Nasazení vaší tooAzure pružiny spouštěcí webové aplikace

Hello následující kroky vás provede kroky toodeploy hello vaší tooAzure pružiny spouštěcí webové aplikace.

1. Otevřete textový editor, například Poznámkový blok systému Windows a vložte následující text do nového dokumentu hello a potom uložte soubor hello jako *web.config*:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. Po uložení hello *web.config* tooyour systému souborů, připojit tooyour webové aplikace pomocí FTP pomocí adresy URL hello, uživatelského jména a hesla z hello předcházející části tohoto kurzu. Například:
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. Změna hello vzdáleného adresáře toohello kořenové složky vaší webové aplikace (což je v */web/wwwroot*), pak zkopírujte soubor JAR hello z vaší aplikace pružiny spouštěcí a hello *web.config* z dříve. Například:
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. Po nasazení vaší JAR a *web.config* soubory tooyour webové aplikace, budete potřebovat toorestart vaší webové aplikace pomocí hello portálu Azure:

   ![][AZ10]

1. Test webové aplikace hello procházení URL tooyour webovou aplikaci pomocí webového prohlížeče, nebo použijte syntaxi hello jako následující příklad, pokud máte k dispozici curl hello:
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. Měli byste vidět hello následující zpráva: **pozdrav z jara spouštěcí!**

   ![Procházet ukázkové aplikace][SB02]

## <a name="next-steps"></a>Další kroky

Další informace o používání pružiny spuštění aplikace v Azure najdete v části hello následující články:

* [Nasazení aplikace spouštěcí Spring v systému Linux v hello Azure Container Service](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [Nasazení aplikace spouštěcí pružiny na Kubernetes Cluster hello Azure Container Service](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].

Další informace o depoying webové aplikace tooAzure pomocí protokolu FTP najdete v tématu [nasazení vaší aplikace tooAzure služby App Service pomocí FTP nebo S].

Další podrobnosti o hello pružiny spouštěcí ukázkový projekt najdete v tématu [pružiny spouštěcí Začínáme].

Pomoc s Začínáme s aplikací pružiny spouštěcí najdete v tématu hello **pružiny Initializr** v https://start.spring.io/.

Další informace o konfigurování dalších nastavení pro webové aplikace najdete v tématu [konfigurace webových aplikací ve službě Azure App Service].

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[konfigurace webových aplikací ve službě Azure App Service]: /azure/app-service-web/web-sites-configure
[nasazení vaší aplikace tooAzure služby App Service pomocí FTP nebo S]: https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí Začínáme]: https://github.com/spring-guides/gs-spring-boot
[pružiny Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png
