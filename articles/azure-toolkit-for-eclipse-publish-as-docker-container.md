---
title: "hello aaaPublish kontejner Docker pomocí nástrojů Azure pro Eclipse | Microsoft Docs"
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
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 53ec3a7f7a171691024e03622fd48d6f1e257b50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a>Publikovat webovou aplikaci jako kontejner Docker pomocí hello Azure Toolkit pro Eclipse

Kontejnery docker jsou často používaný metodu pro nasazení webových aplikací. Pomocí Docker kontejnery vývojáři zvládá všechny své soubory projektu a závislosti do jednoho balíčku pro nasazení tooa server. Hello nástrojů Azure pro prostředí Eclipse usnadňuje tento proces pro vývojáře v jazyce Java přidáním *publikovat jako kontejner Docker* funkce pro nasazení tooMicrosoft Azure. Tento článek vás provede hello kroky požadované toopublish tooAzure vaší aplikace jako kontejnery Docker.

> [!NOTE]
> Další informace o Docker je k dispozici na hello [Docker webu].
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>Publikovat tooAzure vaší webové aplikace pomocí kontejner Docker

1. Otevření projektu webové aplikace v prostředí Eclipse.

2. toostart hello **publikovat jako kontejner Docker** průvodce, proveďte jednu z následujících hello:

   * V hello **Navigátor** zobrazení, klikněte pravým tlačítkem na projekt, klikněte na tlačítko **Azure**a potom klikněte na **publikovat jako kontejner Docker**.

      ![Navigátor zobrazení publikovat jako příkaz kontejner Docker][PUB01]

   * Na panelu nástrojů Eclipse hello, klikněte na tlačítko hello **publikovat** tlačítko a potom klikněte na **publikovat jako kontejner Docker**.

      ![Panel nástrojů Eclipse publikovat jako příkaz kontejner Docker][PUB02]
      
    Hello **nasadit kontejner Docker v Azure** otevře se průvodce.

    ![Hello kontejner Docker nasadit na Průvodce Azure][PUB03]

3. V hello **zadejte název bitové kopie, vyberte cestu hello artefaktů a zkontrolujte toobe hostitelů Docker používá** okně hello následující:

    a. V hello **název bitové kopie Docker** zadejte jedinečný název pro Docker hostitele. (hello průvodce automaticky vytvoří název, ale můžete ho upravit.)

    b. Hello **hostitele** oblasti jsou zobrazeny všechny hostitelů Docker, které jste už vytvořili. Proveďte jednu z následujících hello:

    * Pokud máte u existujícího hostitelů Docker, můžete nasadit tooit vaší webové aplikace.
    * Klikněte na tlačítko toocreate na nového hostitele Docker **přidat**.  
      
    Hello **vytvořit hostitelů Docker** otevře se dialogové okno.

    ![Nasadit kontejner Docker na Průvodce Azure][PUB04a]

4. V hello **konfigurace hello nový virtuální počítač** okno, zadejte následující možnosti pro svého hostitele Docker hello. (hello průvodce automaticky generuje většina možností hello za vás, ale můžete upravit některé z nich.)

   a. **Název**: Zadejte jedinečný název pro hello Docker hostitele. (Není stejný jako název Docker bitové kopie, který jste dřív zadali hello hello.)

   b. **Předplatné**: Zadejte hello předplatné Azure, který používáte pro svého hostitele.

   c. **Oblast**: Zadejte hello geografické oblasti, kde se nachází váš hostitel.

   d. Na hello **hostitelským operačním systémem a velikost** karty:
     * **Hostitelem OS**: Zadejte hello operačního systému pro hello virtuální počítač, který obsahuje váš hostitel.
     * **Velikost**: Zadejte hello velikost virtuálního počítače pro hostitele.

   e. Na hello **skupiny prostředků** karty:
     * **Novou skupinu prostředků**: Vytvořte novou skupinu prostředků pro svého hostitele.
     * **Existující skupinu prostředků**: Zadejte existující skupinu prostředků z účtu Azure.

   f. Na hello **sítě** karty:
     * **Nové virtuální sítě**: vytvoření nové virtuální sítě pro hostitele.
     * **Existující virtuální síť**: Zadejte existující virtuální síť od účtu Azure.

   g. Na hello **úložiště** karty:
     * **Nový účet úložiště**: Vytvořte nový účet úložiště pro hostitele.
     * **Stávající účet úložiště**: Zadejte existující účet úložiště z účtu Azure.

5. Klikněte na **Další**.

6. V hello **konfiguraci protokolu v přihlašovací údaje a nastavení portu** okno, vyberte jednu z hello následující možnosti:

    * **Importovat pověření z Azure Key Vault**: Určuje dříve uloženou sadu přihlašovacích údajů, které jsou uložené ve vašem předplatném Azure.

      >[!NOTE]
      >Azure Key Vault, vytvořený pomocí určitého účtu nebo instanční objekt není automaticky přístupný pro jiný účet nebo instančního objektu, který sdílí hello předplatného. tooallow jiný účet nebo služby hlavní toouse hello Key Vault, je nutné použít účet Azure portálu tooadd hello hello nebo instanční objekt.

    * **Nový protokol v přihlašovacích údajích**: vytvoří novou sadu přihlašovacích údajů. Pokud vyberete tuto možnost, hello následující:
    
      * Na hello **virtuálních počítačů pověření** , zvolte jednu z následujících hello možností hello virtuálnímu počítači přihlašovací údaje vaší Docker hostitele:

          * **Uživatelské jméno**: Zadejte uživatelské jméno hello pro své přihlašovací údaje virtuálního počítače.
          * **Heslo** a **potvrdit**: Zadejte hello heslo pro své přihlašovací údaje virtuálního počítače.
          * **SSH**: Zadejte nastavení hello Secure Shell (SSH) pro Docker hostitele. Můžete vybrat z hello následující možnosti:
            * **Žádný**: Určuje, že virtuální počítač nebude povolovat připojení SSH.
            * **Automaticky generovat**: automaticky vytvoří hello požadavků nastavení pro připojení pomocí protokolu SSH.
            * **Import z adresáře**: Určuje adresář, který obsahuje sadu dříve uloženou nastavení SSH. Hello directory musí obsahovat hello následující dva soubory:
                * *id_rsa*: obsahuje identifikační hello RSA pro uživatele.
                * *id_rsa.pub*: obsahuje hello veřejným klíčem RSA používaný pro ověřování.
        
        ![Vytvořit hostitele Docker][PUB05]

      * Na hello **Docker démon pověření** zadejte hello následující možnosti:

          * **Port démon docker**: Zadejte hello jedinečný port TCP pro Docker hostitele.
          * **Zabezpečení TLS**: Zadejte hello Transport Layer Security nastavení pro Docker hostitele. Můžete vybrat z hello následující možnosti:
            * **Žádný**: Určuje, že virtuální počítač nebude povolovat připojení protokol TLS.
            * **Automaticky generovat**: automaticky vytvoří hello požadavků nastavení pro připojení prostřednictvím protokolu TLS.
            * **Import z adresáře**: Určuje adresář, který obsahuje sadu dříve uloženou nastavení TLS. Přesněji řečeno hello directory musí obsahovat následující soubory šesti hello:
                * *CA.pem* a *certifikační autority key.pem*: obsahovat hello certifikát a veřejný klíč pro hello TLS certifikační autority.
                * *cert.pem* a *key.pem*: obsahovat hello klientský certifikát a veřejný klíč, který se používá k ověřování TLS.
                * *Server.pem* a *server key.pem*: obsahovat hello serveru certifikát a veřejný klíč pro hostitele hello.

        ![Vytvořit hostitele Docker][PUB06]

7. Po zadání všech hello předcházející informace, klikněte na tlačítko **Dokončit**.

8. V hello **nasadit kontejner Docker v Azure** průvodce, klikněte na tlačítko **Další**.

   ![Hello kontejner Docker nasadit na Průvodce Azure][PUB07]

9. V hello **konfigurace toobe kontejner Docker hello vytvořit** okně hello následující:

   a. V hello **název kontejneru Docker** zadejte jedinečný název pro váš kontejner Docker.

   b. Vyberte jednu z následujících imagí Dockeru hello:
     * **Předdefinované image Docker**: Určuje existující bitovou kopii Docker z Azure.

       >[!NOTE]
       >Hello seznam imagí Dockeru v tomto poli se skládá z více bitových kopií této hello nástrojů Azure byla nakonfigurovaná toopatch tak, aby vaše artefaktů je nasazená automaticky.

     * **Vlastní soubor Docker**: Určuje předtím uložili soubor Docker ze svého místního počítače.

       >[!NOTE]
       >Toto je pokročilejší funkce pro vývojáře, kteří chtějí toodeploy vlastní soubor Docker. Je však až toodevelopers, kteří používat tuto možnost tooensure vytvořené jejich soubor Docker správně. Hello nástrojů Azure neověřuje hello obsah vlastní soubor Docker, takže hello nasazení může selhat, pokud hello soubor Docker má problémy. Kromě toho hello nástrojů Azure očekává hello vlastní soubor Docker toocontain artefaktem webové aplikace, a pokusí tooopen připojení HTTP. Pokud vývojáři publikovat jiného typu artefaktů, můžou získat neškodné chyby po nasazení.

   c. **Nastavení portu**: Zadejte hello jedinečnou TCP port vazbu pro váš kontejner Docker.

     ![Hello konfigurovat hello Docker kontejneru toobe vytvořit okno][PUB08]

10. Po dokončení všech předchozích kroků hello, klikněte na tlačítko **Dokončit**.

Hello Azure Toolkit zahájí nasazení vaší webové aplikace tooAzure v kontejner Docker. 

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

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].

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

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png