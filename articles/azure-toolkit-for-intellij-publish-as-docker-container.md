---
title: "hello aaaPublish kontejner Docker pomocí nástrojů Azure pro IntelliJ | Microsoft Docs"
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
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: bee94cb269ea707ae7ad55232e23e915aec48c34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a>Publikovat webovou aplikaci pomocí hello Azure Toolkit pro IntelliJ jako kontejner Docker

Kontejnery docker jsou často používaný metodu pro nasazení webových aplikací. Pomocí Docker kontejnery vývojáři zvládá všechny své soubory projektu a závislosti do jednoho balíčku pro nasazení tooa server. Hello nástrojů Azure pro IntelliJ usnadňuje tento proces pro vývojáře v jazyce Java přidáním *publikovat jako kontejner Docker* funkce pro nasazení tooMicrosoft Azure. Tento článek vás provede hello kroky požadované toopublish tooAzure vaší aplikace jako kontejnery Docker.

> [!NOTE]
>
> Další informace o Docker je k dispozici na hello [Docker webu].
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>Publikovat tooAzure vaší webové aplikace pomocí kontejner Docker

> [!NOTE]
> * toopublish vaší webové aplikaci, musíte vytvořit artefakt připravené pro nasazení. toolearn více, viz hello [Další informace o vytváření artefakty](#artifacts) části.
>
> * Po dokončení Průvodce nasazením hello alespoň jednou, většinu nastavení jsou použity jako výchozí, když znovu spustíte Průvodce hello.
>

1. Otevřete projekt webové aplikace v IntelliJ.

2. toostart hello **publikovat jako kontejner Docker** průvodce, proveďte jednu z následujících hello:

   * V hello **projektu** nástroj okna, klikněte pravým tlačítkem na projekt, klikněte na **Azure**a potom klikněte na **publikovat jako kontejner Docker**:

      ![Hello publikovat jako příkaz kontejner Docker][PUB01]

   * Na panelu nástrojů IntelliJ hello, klikněte na tlačítko hello **publikovat skupiny** tlačítko a potom klikněte na **publikovat jako kontejner Docker**:

      ![Hello publikovat jako příkaz kontejner Docker][PUB02]  
    Hello **nasadit kontejner Docker v Azure** otevře se průvodce.

   ![Hello kontejner Docker nasadit na Průvodce Azure][PUB03]

3. V hello **zadejte název bitové kopie, vyberte cestu hello artefaktů a zkontrolujte toobe hostitelů Docker používá** okně hello následující: 

   a. V hello **název bitové kopie Docker** zadejte jedinečný název pro Docker hostitele. (hello průvodce automaticky vytvoří název, ale můžete ho upravit.) 

   b. Hello **hostitele** oblasti jsou zobrazeny všechny hostitelů Docker, které jste už vytvořili. Proveďte jednu z následujících hello: 
      * Pokud máte u existujícího hostitelů Docker, můžete nasadit tooit vaší webové aplikace.
      * toocreate Docker hostitele, klikněte na tlačítko hello zelená – znaménko plus (**+**).  
       Hello **vytvořit hostitelů Docker** otevře se dialogové okno. 

      ![Nasadit kontejner Docker na Průvodce Azure][PUB04a]

4. V hello **konfigurace hello nový virtuální počítač** okno, zadejte následující informace o hostiteli Docker hello. (hello průvodce automaticky generuje většinu hello informace pro vás, ale můžete upravit některé z nich.) 

   a. V hello **název** zadejte jedinečný název hostitele Docker hello. (Není stejný jako název Docker bitové kopie, který jste dřív zadali hello hello.) 
    
   b. V hello **předplatné** zadejte hello předplatné Azure, který používáte pro svého hostitele. 
      
   c. V hello **oblast** zadejte hello geografické oblasti, kde se nachází váš hostitel.
      
   d. Na hello **operačního systému a velikost** kartě, hello následující:      
      * **Hostitelem OS**: Zadejte hello operačního systému pro hello virtuální počítač, který obsahuje váš hostitel. 
      * **Velikost**: Zadejte hello velikost virtuálního počítače pro hostitele.   
       
   e. Na hello **skupiny prostředků** , vyberte jednu z následujících hello:      
      * **Novou skupinu prostředků**: vytvoření skupiny prostředků pro svého hostitele.
      * **Existující skupinu prostředků**: Zadejte existující skupinu prostředků z účtu Azure. 
       
   f. Na hello **sítě** , vyberte jednu z následujících hello:      
      * **Nové virtuální sítě**: vytvoření virtuální sítě pro hostitele.
      * **Existující virtuální síť**: Zadejte existující virtuální sítě z účtu Azure. 
       
   g. Na hello **úložiště** , vyberte jednu z následujících hello:      
      * **Nový účet úložiště**: vytvoření účtu úložiště pro hostitele.
      * **Stávající účet úložiště**: Zadejte existující účet úložiště z účtu Azure.
       
5. Klikněte na **Další**.  
     Hello **konfiguraci protokolu v přihlašovací údaje a nastavení portu** otevře se okno.

      ![Konfigurace protokolu Hello v okně nastavení port a přihlašovací údaje][PUB05]

6. Vyberte jednu z hello následující možnosti:

      * **Importovat pověření z Azure Key Vault**: Zadejte dříve uloženou sadu přihlašovacích údajů, které jsou uložené ve vašem předplatném Azure.

          > [!NOTE]
          > Azure trezoru klíčů, který je vytvořen pomocí určitého účtu nebo instanční objekt není automaticky přístupný pro jiný účet nebo instančního objektu, který sdílí hello předplatného. tooallow jiný účet nebo služby toouse hlavní klíč hello trezoru, je nutné použít hello Azure portálu tooadd hello účet nebo instanční objekt.

      * **Nový protokol v přihlašovacích údajích**: Vytvořte novou sadu přihlašovacích údajů. Pokud vyberete tuto možnost, hello následující:

        a. Na hello **virtuálních počítačů pověření** kartě, zadejte následující informace o virtuálním počítači přihlašovací údaje hello váš hostitel Docker hello: * **uživatelské jméno**: Zadejte hello uživatelské jméno pro přihlášení vaše virtuální počítače přihlašovací údaje.
             * **Heslo** a **potvrdit**: Zadejte hello heslo pro své virtuální počítače přihlašovací údaje.
             * **SSH**: Zadejte nastavení hello Secure Shell (SSH) pro Docker hostitele. Můžete vybrat jednu z hello následující možnosti: * **žádné**: Určuje, že virtuální počítač neumožňuje připojení SSH.
                * **Automaticky generovat**: automaticky vytvoří hello požadavků nastavení pro připojení pomocí protokolu SSH.
                * **Import z adresáře**: vám umožní toospecify adresář, který obsahuje sadu dříve uloženou nastavení SSH. Hello directory musí obsahovat hello následující dva soubory:
                
                  * *id_rsa*: Contains hello RSA identification for a user.
                  * *id_rsa.pub*: Contains hello RSA public key that is used for authentication.
            
        b. Na hello **Docker démon přístup** kartě, zadejte hello následující informace:

          ![Vytvořit hostitele Docker][PUB06]
    
             * **Docker Daemon port**: Enter hello unique TCP port for your Docker host.
             * **TLS Security**: Enter hello Transport Layer Security settings for your Docker host. You can choose from hello following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates hello requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. hello directory must contain hello following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain hello certificate and public key for hello TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain hello client certificate and public key that is used for TLS authentication.

7. Po zadání hello požadované informace, klikněte na tlačítko **Dokončit**.  
    Hello **nasadit kontejner Docker v Azure** se znovu zobrazí průvodce.

   ![Nasadit kontejner Docker na Průvodce Azure][PUB07]

8. Klikněte na **Další**.  
    Hello **konfigurace toobe kontejner Docker hello vytvořit** otevře se okno.

   ![Hello konfigurovat hello Docker kontejneru toobe vytvořit okno][PUB08]

9. V hello **konfigurace toobe kontejner Docker hello vytvořit** okno, zadejte hello následující informace: 

   a. V hello **název kontejneru Docker** zadejte jedinečný název pro váš kontejner Docker.

   b. Vyberte jednu z následujících imagí Dockeru hello: 

      * **Předdefinované image Docker**: Určete existující bitovou kopii Docker z Azure. 

        > [!NOTE]
        > Hello seznam imagí Dockeru v tomto poli se skládá z více bitových kopií této hello nástrojů Azure byla nakonfigurovaná toopatch tak, aby vaše artefaktů je nasazená automaticky. 

      * **Vlastní soubor Docker**: Zadejte předtím uložili soubor Docker ze svého místního počítače.

        > [!NOTE]
        > Toto je pokročilejší funkce pro vývojáře, kteří chtějí toodeploy vlastní soubor Docker. Je však až toodevelopers, kteří používat tuto možnost tooensure vytvořené jejich soubor Docker správně. Protože hello Azure Toolkit neověřuje hello obsah vlastní soubor Docker, hello nasazení může selhat, pokud má hello soubor Docker problémy. Kromě toho protože hello nástrojů Azure očekává hello vlastní soubor Docker toocontain artefaktem webové aplikace, pokusí tooopen připojení HTTP. Pokud vývojáři publikovat jiného typu artefaktů, obdrží může neškodné chyby po nasazení.

   c. V hello **nastavení portu** zadejte hello jedinečnou TCP port vazbu pro váš kontejner Docker. 

10. Po dokončení předchozích kroků hello, klikněte na tlačítko **Dokončit**. 

Hello Azure Toolkit zahájí nasazení vaší webové aplikace tooAzure v kontejner Docker. Pokud jste nakonfigurovali IntelliJ toobe nasazené v pozadí hello **nasazení tooAzure** se zobrazí indikátor průběhu. 

![indikátor průběhu nasazení Hello][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a>Další informace o vytváření artefaktů

toocreate artefaktem připravené pro nasazení hello následující:

1. Otevřete projekt webové aplikace v IntelliJ.

2. Klikněte na tlačítko **soubor**a potom klikněte na **strukturu projektu**.

   ![Hello příkaz struktura projektu][ART01]

3. tooadd na artefaktů, klikněte na tlačítko hello zelená – znaménko plus (**+**) a potom klikněte na **webové aplikace: archivu**.

   ![Hello příkaz "Archivu webové aplikace:"][ART02]

4. V hello **název** pole, zadejte název vaší artefaktů (nezahrnují hello *.war* rozšíření) a potom klikněte na **OK**.

   ![pole Název artefaktů Hello][ART03]

Další informace o vytváření artefakty v IntelliJ najdete v tématu [konfigurace artefakty] na webu JetBrains hello.

## <a name="next-steps"></a>Další kroky
Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující prostředky:

* [Azure nástrojů pro Eclipse]
  * [Co je nového v hello nástrojů Azure pro Eclipse]
  * [Instalace hello nástrojů Azure pro Eclipse]
  * [Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]
  * [Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Co je nového v hello nástrojů Azure pro IntelliJ]
  * [Instalace hello Azure Toolkit pro IntelliJ]
  * [Přihlášení pokyny pro hello Azure Toolkit IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].

Další zdroje pro Docker najdete v tématu hello oficiální [Docker webu].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Přihlášení pokyny pro hello Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

[Docker webu]: https://www.docker.com/
[konfigurace artefakty]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
