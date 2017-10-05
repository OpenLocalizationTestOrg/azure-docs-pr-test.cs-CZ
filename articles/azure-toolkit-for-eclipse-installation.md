---
title: Instalace Azure Toolkit pro Eclipse | Microsoft Docs
description: "Informace o instalaci sady nástrojů Azure pro prostředí Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 35cddba38c364dfb2f6a8646b0014d48ca4cb795
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a>Instalace Azure Toolkit pro Eclipse
Sada Azure pro Eclipse obsahuje šablony a funkce, které vám umožní snadno vytvářet, vývoj, testování a nasazení aplikace na platformě Azure pomocí vývojové prostředí Eclipse. Sada nástrojů Azure pro Eclipse je projekt Open Source. Zdrojový kód je k dispozici v části licencí MIT z <https://github.com/microsoft/azure-tools-for-java>.

Následující kroky ukazují, jak nainstalovat sadu Azure pro Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>K instalaci sady nástrojů Azure pro Eclipse
1. Spusťte Eclipse.
2. Klikněte na tlačítko **pomoci** nabídce a pak klikněte na tlačítko **instalace nového softwaru**, jak je znázorněno na následujícím obrázku.
   
    ![Instalace Azure Toolkit pro Eclipse][01]
3. V **dostupného softwaru** dialogové okno, v **pracovat s** textového pole, typ `http://dl.microsoft.com/eclipse` následuje **Enter** klíč.
4. V **název** podokně zkontrolujte **nástrojů Azure pro Eclipse**a zrušte zaškrtnutí políčka **obraťte se na všechny lokality aktualizace při instalaci najít požadovaný software**. Na obrazovce by měl vypadat přibližně takto:
   
    ![Instalace Azure Toolkit pro Eclipse][02]
5. Pokud rozbalíte **nástrojů Azure pro Eclipse**, zobrazí se následující položky:
   
   * **Modul plug-in přehled aplikace pro jazyk Java**: Tato součást umožňuje používat služby protokolování a analýza telemetrie Azure pro vaše aplikace a instance serveru.
   * **Azure filtru služeb řízení přístupu**: Tato součást poskytuje podporu pro ověřování uživatelů aplikace s Azure ACS, povolení scénářů přihlašování a externího logiku identity vně aplikace.
   * **Modul plug-in Azure běžné**: Tato součást poskytuje běžné funkce vyžaduje další součásti sady toolkit.
   * **Průzkumník Azure pro Eclipse**: Tato součást poskytuje běžné funkce vyžaduje další součásti sady toolkit.
   * **Azure modul plug-in pro Eclipse s Javou**: Tato součást poskytuje podporu pro vývoj projekty, které pomáhají vytváření, testování a nasazení aplikací v jazyce Java pro Microsoft Azure cloud v prostředí Eclipse a prostřednictvím příkazového řádku.
   * **Azure modulu plug-in webové aplikace s Javou**: Tato součást poskytuje podporu pro nasazení webových aplikací Java do kontejnerů webové aplikace Microsoft Azure.
   * **4.2 ovladač JDBC Microsoft pro systém SQL Server**: Tato součást poskytuje JDBC rozhraní API pro SQL Server a Microsoft Azure SQL Database pro Java platformy Enterprise Edition 8.
   * **Balíček pro Apache Qpid klientské knihovny pro JMS**: Tato součást poskytuje součást klienta nástroje JMS z projektu Apache Qpid povolit aplikace k používání protokolu AMQP zasílání zpráv ve službě Microsoft Azure.
   * **Balíček pro knihovny Microsoft Azure Libraries for Java**: Tato součást poskytuje rozhraní API pro přístup ke službám Microsoft Azure, jako je například úložiště, služby service bus, běh služby atd.
6. Klikněte na **Další**. (Pokud se setkáte neobvyklou zpoždění při instalaci sady nástrojů, ujistěte se, že **obraťte se na všechny lokality aktualizace při instalaci najít požadovaný software** není zaškrtnuta.)
7. V **nainstalovat podrobnosti** dialogové okno, klikněte na tlačítko **Další**.
   
    ![Přečtěte si podrobné informace o instalaci][03]
8. V **zkontrolujte licence** dialogové okno, přečtěte si podmínky licenční smlouvy. Pokud souhlasíte s podmínkami licenční smlouvy, klikněte na tlačítko **s podmínkami licenční smlouvy souhlasím** a pak klikněte na **Dokončit**. (Zbývající kroky předpokládají, že souhlasíte s podmínkami licenční smlouvy. Pokud nesouhlasíte s podmínkami licenční smlouvy, ukončete proces instalace.)
   
    ![Zkontrolujte licencí][04]
   
    Eclipse bude stáhněte a nainstalujte požadované balíčky.
   
    ![Průběh instalace][05]
9. Pokud se zobrazí výzva k restartování prostředí Eclipse pro dokončení instalace, klikněte na tlačítko **Ano**.
   
    ![Restartujte řádku][06]

## <a name="see-also"></a>Viz také
Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:

* [Azure nástrojů pro Eclipse]
  * [Novinky v sadě Azure Toolkit pro Eclipse]
  * *Instalace Azure Toolkit pro Eclipse (v tomto článku)*
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse]
  * [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Novinky v sadě Azure Toolkit pro IntelliJ]
  * [Instalace sady Azure Toolkit pro IntelliJ]
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Novinky v sadě Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Novinky v sadě Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->
