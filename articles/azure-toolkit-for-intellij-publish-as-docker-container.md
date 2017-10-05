---
title: "Publikovat kontejner Docker pomocí nástrojů Azure pro IntelliJ | Microsoft Docs"
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
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 96680319a6c4c0f0a4673cd6303a5b172f428797
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a>Publikování webové aplikace pomocí nástrojů Azure pro IntelliJ jako kontejner Docker

Kontejnery docker jsou často používaný metodu pro nasazení webových aplikací. Pomocí Docker kontejnery vývojáři zvládá všechny své soubory projektu a závislosti do jednoho balíčku pro nasazení na server. Sada nástrojů Azure pro IntelliJ usnadňuje tento proces pro vývojáře v jazyce Java přidáním *publikovat jako kontejner Docker* funkcí pro nasazení do služby Microsoft Azure. Tento článek vás provede kroky potřebné k publikování aplikací do Azure jako kontejnery Docker.

> [!NOTE]
>
> Další informace o Docker je k dispozici na [Docker webu].
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>Publikování webové aplikace do Azure pomocí kontejner Docker

> [!NOTE]
> * K publikování webové aplikace, musíte vytvořit artefakt připravené pro nasazení. Další informace najdete v tématu [Další informace o vytváření artefakty](#artifacts) části.
>
> * Po dokončení Průvodce nasazením alespoň jednou, většinu nastavení jsou použity jako výchozí, když spustíte průvodce znovu.
>

1. Otevřete projekt webové aplikace v IntelliJ.

2. Spuštění **publikovat jako kontejner Docker** průvodce, proveďte jednu z následujících akcí:

   * V **projektu** nástroj okna, klikněte pravým tlačítkem na projekt, klikněte na **Azure**a potom klikněte na **publikovat jako kontejner Docker**:

      ![Publikovat jako příkaz kontejner Docker][PUB01]

   * Na panelu nástrojů IntelliJ, klikněte na tlačítko **publikovat skupiny** tlačítko a potom klikněte na **publikovat jako kontejner Docker**:

      ![Publikovat jako příkaz kontejner Docker][PUB02]  
    **Nasadit kontejner Docker v Azure** otevře se průvodce.

   ![Kontejner Docker nasadit na Průvodce Azure][PUB03]

3. V **zadejte název bitové kopie, vyberte artefaktů cestu a zkontrolujte hostitelů Docker má být použit** okno, postupujte takto: 

   a. V **název bitové kopie Docker** zadejte jedinečný název pro Docker hostitele. (Průvodce automaticky vytvoří název, ale můžete ho upravit.) 

   b. **Hostitele** oblasti jsou zobrazeny všechny hostitelů Docker, které jste už vytvořili. Proveďte jednu z následujících akcí: 
      * Pokud máte u existujícího hostitelů Docker, můžete nasadit webové aplikace k němu.
      * Chcete-li vytvořit Docker hostitele, klikněte na tlačítko zeleného znaménka (**+**).  
       **Vytvořit hostitelů Docker** otevře se dialogové okno. 

      ![Nasadit kontejner Docker na Průvodce Azure][PUB04a]

4. V **nakonfigurovat nový virtuální počítač** okno, zadejte následující informace o Docker hostiteli. (Průvodce automaticky generuje většinu informací pro vás, ale můžete upravit některé z nich.) 

   a. V **název** zadejte jedinečný název pro Docker hostitele. (Není stejný jako název Docker bitové kopie, který jste dřív zadali.) 
    
   b. V **předplatné** zadejte předplatné Azure, který používáte pro svého hostitele. 
      
   c. V **oblast** zadejte geografické oblasti, kde se nachází váš hostitel.
      
   d. Na **operačního systému a velikost** kartě, postupujte takto:      
      * **Hostitelem OS**: Zadejte operační systém pro virtuální počítač, který obsahuje váš hostitel. 
      * **Velikost**: Zadejte velikost virtuálního počítače pro hostitele.   
       
   e. Na **skupiny prostředků** , vyberte z následujících:      
      * **Novou skupinu prostředků**: vytvoření skupiny prostředků pro svého hostitele.
      * **Existující skupinu prostředků**: Zadejte existující skupinu prostředků z účtu Azure. 
       
   f. Na **sítě** , vyberte z následujících:      
      * **Nové virtuální sítě**: vytvoření virtuální sítě pro hostitele.
      * **Existující virtuální síť**: Zadejte existující virtuální sítě z účtu Azure. 
       
   g. Na **úložiště** , vyberte z následujících:      
      * **Nový účet úložiště**: vytvoření účtu úložiště pro hostitele.
      * **Stávající účet úložiště**: Zadejte existující účet úložiště z účtu Azure.
       
5. Klikněte na **Další**.  
     **Konfiguraci protokolu v přihlašovací údaje a nastavení portu** otevře se okno.

      ![Konfigurace protokolů v okně nastavení port a přihlašovací údaje][PUB05]

6. Vyberte jednu z následujících možností:

      * **Importovat pověření z Azure Key Vault**: Zadejte dříve uloženou sadu přihlašovacích údajů, které jsou uložené ve vašem předplatném Azure.

          > [!NOTE]
          > Azure trezoru klíčů, který je vytvořen pomocí určitého účtu nebo instanční objekt není automaticky přístupný pro jiný účet nebo instančního objektu, který sdílí předplatné. Povolit hlavní trezoru klíčů použijte jiný účet nebo služby, vyžaduje použití portálu Azure přidat účet nebo instanční objekt.

      * **Nový protokol v přihlašovacích údajích**: Vytvořte novou sadu přihlašovacích údajů. Pokud vyberete tuto možnost, postupujte takto:

        a. Na **virtuálních počítačů pověření** kartě, zadejte následující informace pro virtuální počítač přihlašovací údaje pro váš hostitel Docker: * **uživatelské jméno**: Zadejte uživatelské jméno pro své virtuální počítače přihlašovací údaje.
             * **Heslo** a **potvrdit**: Zadejte heslo pro své virtuální počítače přihlašovací údaje.
             * **SSH**: Zadejte nastavení Secure Shell (SSH) pro Docker hostitele. Můžete vybrat jednu z následujících možností: * **žádné**: Určuje, že virtuální počítač neumožňuje připojení SSH.
                * **Automaticky generovat**: automaticky vytvoří požadavků nastavení pro připojení pomocí protokolu SSH.
                * **Import z adresáře**: můžete zadat adresář, který obsahuje sadu dříve uloženou nastavení SSH. Adresář musí obsahovat následující dva soubory:
                
                  * *id_rsa*: Contains the RSA identification for a user.
                  * *id_rsa.pub*: Contains the RSA public key that is used for authentication.
            
        b. Na **Docker démon přístup** kartě, zadejte následující informace:

          ![Vytvořit hostitele Docker][PUB06]
    
             * **Docker Daemon port**: Enter the unique TCP port for your Docker host.
             * **TLS Security**: Enter the Transport Layer Security settings for your Docker host. You can choose from the following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates the requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. The directory must contain the following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.

7. Po zadání požadovaných informací klikněte na tlačítko **Dokončit**.  
    **Nasadit kontejner Docker v Azure** se znovu zobrazí průvodce.

   ![Nasadit kontejner Docker na Průvodce Azure][PUB07]

8. Klikněte na **Další**.  
    **Konfigurace kontejner Docker vytvořit** otevře se okno.

   ![Konfigurovat kontejner Docker vytvořit okno][PUB08]

9. V **konfigurace kontejner Docker vytvořit** okno, zadejte následující informace: 

   a. V **název kontejneru Docker** zadejte jedinečný název pro váš kontejner Docker.

   b. Vyberte jednu z následujících imagí Dockeru: 

      * **Předdefinované image Docker**: Určete existující bitovou kopii Docker z Azure. 

        > [!NOTE]
        > Seznam imagí Dockeru v tomto poli se skládá z několika bitové kopie, které Azure Toolkit nakonfigurován, aby opravit tak, aby vaše artefaktů je nasazená automaticky. 

      * **Vlastní soubor Docker**: Zadejte předtím uložili soubor Docker ze svého místního počítače.

        > [!NOTE]
        > Toto je pokročilejší funkce pro vývojáře, kteří chtějí nasadit vlastní soubor Docker. Je však až vývojáře, kteří tuto možnost použijte k zajištění, že jejich soubor Docker správně vytvořen. Protože Azure Toolkit neověřuje obsah vlastní soubor Docker, nasazení může selhat, pokud má soubor Docker problémy. Kromě toho protože sada nástrojů Azure očekává vlastní soubor Docker tak, aby obsahovala artefaktem webové aplikace, se pokusí otevřít připojení HTTP. Pokud vývojáři publikovat jiného typu artefaktů, obdrží může neškodné chyby po nasazení.

   c. V **nastavení portu** zadejte jedinečnou vazbu port TCP pro váš kontejner Docker. 

10. Po dokončení předchozího postupu, klikněte na tlačítko **Dokončit**. 

Sada nástrojů Azure zahájí nasazení webové aplikace do Azure na kontejner Docker. Pokud jste nakonfigurovali IntelliJ k nasazení na pozadí **nasazení do Azure** se zobrazí indikátor průběhu. 

![Indikátor průběhu nasazení][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a>Další informace o vytváření artefaktů

Pokud chcete vytvořit artefakt připravené pro nasazení, postupujte takto:

1. Otevřete projekt webové aplikace v IntelliJ.

2. Klikněte na tlačítko **soubor**a potom klikněte na **strukturu projektu**.

   ![Příkaz struktura projektu][ART01]

3. Chcete-li přidat artefakt, klikněte na tlačítko zeleného znaménka (**+**) a potom klikněte na **webové aplikace: archivu**.

   ![Příkaz "Archivu webové aplikace:"][ART02]

4. V **název** pole, zadejte název vaší artefaktů (nezahrnují *.war* rozšíření) a potom klikněte na **OK**.

   ![Pole Název artefaktů][ART03]

Další informace o vytváření artefakty v IntelliJ najdete v tématu [konfigurace artefakty] na webu JetBrains.

## <a name="next-steps"></a>Další kroky
Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v následujících zdrojích informací:

* [Azure nástrojů pro Eclipse]
  * [Co je nového v sadě nástrojů Azure pro Eclipse]
  * [Instalace sady Azure Toolkit pro Eclipse]
  * [Pokyny přihlášení k Azure nástrojů pro Eclipse]
  * [Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Co je nového v sadě nástrojů Azure pro IntelliJ]
  * [Instalace sady Azure Toolkit pro IntelliJ]
  * [Přihlášení pokyny pro Azure Toolkit IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).

Další zdroje pro Docker, najdete v oficiální [Docker webu].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Pokyny přihlášení k Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Přihlášení pokyny pro Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v sadě nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v sadě nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

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
