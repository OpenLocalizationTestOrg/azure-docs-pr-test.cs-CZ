---
title: "Publikovat kontejner Docker pomocí nástrojů Azure pro Eclipse | Microsoft Docs"
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
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 746bb0a073396b84fa8592adda6748a2a5692ee8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a>Publikovat webovou aplikaci jako kontejner Docker pomocí nástrojů Azure pro Eclipse

Kontejnery docker jsou často používaný metodu pro nasazení webových aplikací. Pomocí Docker kontejnery vývojáři zvládá všechny své soubory projektu a závislosti do jednoho balíčku pro nasazení na server. Sada nástrojů Azure pro Eclipse usnadňuje tento proces pro vývojáře v jazyce Java přidáním *publikovat jako kontejner Docker* funkcí pro nasazení do služby Microsoft Azure. Tento článek vás provede kroky potřebné k publikování aplikací do Azure jako kontejnery Docker.

> [!NOTE]
> Další informace o Docker je k dispozici na [Docker webu].
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>Publikování webové aplikace do Azure pomocí kontejner Docker

1. Otevření projektu webové aplikace v prostředí Eclipse.

2. Spuštění **publikovat jako kontejner Docker** průvodce, proveďte jednu z následujících akcí:

   * V **Navigátor** zobrazení, klikněte pravým tlačítkem na projekt, klikněte na tlačítko **Azure**a potom klikněte na **publikovat jako kontejner Docker**.

      ![Navigátor zobrazení publikovat jako příkaz kontejner Docker][PUB01]

   * Na panelu nástrojů Eclipse klikněte na **publikovat** tlačítko a potom klikněte na **publikovat jako kontejner Docker**.

      ![Panel nástrojů Eclipse publikovat jako příkaz kontejner Docker][PUB02]
      
    **Nasadit kontejner Docker v Azure** otevře se průvodce.

    ![Kontejner Docker nasadit na Průvodce Azure][PUB03]

3. V **zadejte název bitové kopie, vyberte artefaktů cestu a zkontrolujte hostitelů Docker má být použit** okno, postupujte takto:

    a. V **název bitové kopie Docker** zadejte jedinečný název pro Docker hostitele. (Průvodce automaticky vytvoří název, ale můžete ho upravit.)

    b. **Hostitele** oblasti jsou zobrazeny všechny hostitelů Docker, které jste už vytvořili. Proveďte jednu z následujících akcí:

    * Pokud máte u existujícího hostitelů Docker, můžete nasadit webové aplikace k němu.
    * Chcete-li vytvořit nového hostitele Docker, klikněte na tlačítko **přidat**.  
      
    **Vytvořit hostitelů Docker** otevře se dialogové okno.

    ![Nasadit kontejner Docker na Průvodce Azure][PUB04a]

4. V **nakonfigurovat nový virtuální počítač** okně určete následující možnosti pro Docker hostitele. (Průvodce automaticky vygeneruje většina možností pro vás, ale můžete upravit některé z nich.)

   a. **Název**: Zadejte jedinečný název pro Docker hostitele. (Není stejný jako název Docker bitové kopie, který jste dřív zadali.)

   b. **Předplatné**: Zadejte předplatné Azure, který používáte pro svého hostitele.

   c. **Oblast**: Zadejte geografické oblasti, kde se nachází váš hostitel.

   d. Na **hostitelským operačním systémem a velikost** karty:
     * **Hostitelem OS**: Zadejte operační systém pro virtuální počítač, který obsahuje váš hostitel.
     * **Velikost**: Zadejte velikost virtuálního počítače pro hostitele.

   e. Na **skupiny prostředků** karty:
     * **Novou skupinu prostředků**: Vytvořte novou skupinu prostředků pro svého hostitele.
     * **Existující skupinu prostředků**: Zadejte existující skupinu prostředků z účtu Azure.

   f. Na **sítě** karty:
     * **Nové virtuální sítě**: vytvoření nové virtuální sítě pro hostitele.
     * **Existující virtuální síť**: Zadejte existující virtuální síť od účtu Azure.

   g. Na **úložiště** karty:
     * **Nový účet úložiště**: Vytvořte nový účet úložiště pro hostitele.
     * **Stávající účet úložiště**: Zadejte existující účet úložiště z účtu Azure.

5. Klikněte na **Další**.

6. V **konfiguraci protokolu v přihlašovací údaje a nastavení portu** okno, vyberte jednu z následujících možností:

    * **Importovat pověření z Azure Key Vault**: Určuje dříve uloženou sadu přihlašovacích údajů, které jsou uložené ve vašem předplatném Azure.

      >[!NOTE]
      >Azure Key Vault, vytvořený pomocí určitého účtu nebo instanční objekt není automaticky přístupný pro jiný účet nebo instančního objektu, který sdílí předplatné. Povolit jiný účet nebo služby hlavní používat Key Vault, vyžaduje použití portálu Azure přidat účet nebo instanční objekt.

    * **Nový protokol v přihlašovacích údajích**: vytvoří novou sadu přihlašovacích údajů. Pokud vyberete tuto možnost, postupujte takto:
    
      * Na **pověření virtuálních počítačů** , zvolte jednu z následujících možností pro virtuální počítač přihlašovací údaje vaší Docker hostitele:

          * **Uživatelské jméno**: Zadejte uživatelské jméno pro své přihlašovací údaje virtuálního počítače.
          * **Heslo** a **potvrdit**: Zadejte heslo pro své přihlašovací údaje virtuálního počítače.
          * **SSH**: Zadejte nastavení Secure Shell (SSH) pro Docker hostitele. Můžete zvolit z následujících možností:
            * **Žádný**: Určuje, že virtuální počítač nebude povolovat připojení SSH.
            * **Automaticky generovat**: automaticky vytvoří požadavků nastavení pro připojení pomocí protokolu SSH.
            * **Import z adresáře**: Určuje adresář, který obsahuje sadu dříve uloženou nastavení SSH. Adresář musí obsahovat následující dva soubory:
                * *id_rsa*: obsahuje identifikační RSA pro uživatele.
                * *id_rsa.pub*: obsahuje veřejný klíč RSA, který se používá k ověřování.
        
        ![Vytvořit hostitele Docker][PUB05]

      * Na **Docker démon pověření** určete následující možnosti:

          * **Port démon docker**: Zadejte jedinečný port TCP pro Docker hostitele.
          * **Zabezpečení TLS**: Zadejte Transport Layer Security nastavení pro Docker hostitele. Můžete zvolit z následujících možností:
            * **Žádný**: Určuje, že virtuální počítač nebude povolovat připojení protokol TLS.
            * **Automaticky generovat**: automaticky vytvoří požadavků nastavení pro připojení prostřednictvím protokolu TLS.
            * **Import z adresáře**: Určuje adresář, který obsahuje sadu dříve uloženou nastavení TLS. Přesněji řečeno adresář musí obsahovat následující šesti soubory:
                * *CA.pem* a *certifikační autority key.pem*: obsahovat certifikát a veřejný klíč pro certifikační autority TLS.
                * *cert.pem* a *key.pem*: obsahovat klientský certifikát a veřejný klíč, který se používá k ověřování TLS.
                * *Server.pem* a *server key.pem*: obsahovat serverový certifikát a veřejný klíč pro hostitele.

        ![Vytvořit hostitele Docker][PUB06]

7. Po zadání všechny předchozí informace, klikněte na tlačítko **Dokončit**.

8. V **nasadit kontejner Docker v Azure** průvodce, klikněte na tlačítko **Další**.

   ![Kontejner Docker nasadit na Průvodce Azure][PUB07]

9. V **konfigurace kontejner Docker vytvořit** okno, postupujte takto:

   a. V **název kontejneru Docker** zadejte jedinečný název pro váš kontejner Docker.

   b. Vyberte jednu z následujících imagí Dockeru:
     * **Předdefinované image Docker**: Určuje existující bitovou kopii Docker z Azure.

       >[!NOTE]
       >Seznam imagí Dockeru v tomto poli se skládá z několika bitové kopie, které Azure Toolkit nakonfigurován, aby opravit tak, aby vaše artefaktů je nasazená automaticky.

     * **Vlastní soubor Docker**: Určuje předtím uložili soubor Docker ze svého místního počítače.

       >[!NOTE]
       >Toto je pokročilejší funkce pro vývojáře, kteří chtějí nasadit vlastní soubor Docker. Je však až vývojáře, kteří tuto možnost použijte k zajištění, že jejich soubor Docker správně vytvořen. Sada nástrojů Azure neověřuje obsah vlastní soubor Docker, tak nasazení může selhat, pokud soubor Docker má problémy. Kromě toho sada nástrojů Azure očekává vlastní soubor Docker tak, aby obsahovala artefaktem webové aplikace a pokusí se otevřít připojení HTTP. Pokud vývojáři publikovat jiného typu artefaktů, můžou získat neškodné chyby po nasazení.

   c. **Nastavení portu**: Zadejte jedinečnou vazbu port TCP pro váš kontejner Docker.

     ![Konfigurovat kontejner Docker vytvořit okno][PUB08]

10. Po dokončení všech předchozích kroků, klikněte na tlačítko **Dokončit**.

Sada nástrojů Azure zahájí nasazení webové aplikace do Azure na kontejner Docker. 

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

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].

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