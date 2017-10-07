---
title: "aaaInstalling hello nástrojů Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak tooinstall hello nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 6c195fab2b47fb5c42541a8cf52be4ec88d27b5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="installing-hello-azure-toolkit-for-eclipse"></a>Instalace hello nástrojů Azure pro Eclipse
Hello nástrojů Azure pro prostředí Eclipse obsahuje šablony a funkce, které umožňují tooeasily vytvoření, vývoj, testování a nasazení aplikace na platformě Azure pomocí vývojové prostředí Eclipse hello. Hello nástrojů Azure pro Eclipse je projekt Open Source. Hello zdrojový kód je k dispozici v části hello licencí MIT z <https://github.com/microsoft/azure-tools-for-java>.

Hello následující kroky vám ukážou, jak tooinstall hello nástrojů Azure pro prostředí Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="tooinstall-hello-azure-toolkit-for-eclipse"></a>tooinstall hello nástrojů Azure pro Eclipse
1. Spusťte Eclipse.
2. Klikněte na tlačítko hello **pomoci** nabídce a pak klikněte na tlačítko **instalace nového softwaru**, jak ukazuje následující obrázek hello.
   
    ![Instalace hello nástrojů Azure pro Eclipse][01]
3. V hello **dostupného softwaru** dialogové okno, v rámci hello **pracovat s** textového pole, typ `http://dl.microsoft.com/eclipse` následuje hello **Enter** klíč.
4. V hello **název** podokně zkontrolujte **nástrojů Azure pro Eclipse**a zrušte zaškrtnutí políčka **obraťte se na všech lokalitách během aktualizace instalace softwaru potřeba toofind**. Na obrazovce by měla vypadat podobně jako toohello následující:
   
    ![Instalace hello nástrojů Azure pro Eclipse][02]
5. Pokud rozbalíte hello **nástrojů Azure pro Eclipse**, zobrazí se hello následující položky:
   
   * **Modul plug-in přehled aplikace pro jazyk Java**: Tato součást vám umožní služby protokolování a analýza telemetrie toouse Azure pro vaše aplikace a instance serveru.
   * **Azure filtru služeb řízení přístupu**: Tato součást poskytuje podporu pro ověřování uživatelů aplikace s Azure ACS, povolení scénářů přihlašování a externího logiku identity vně aplikace hello.
   * **Modul plug-in Azure běžné**: Tato součást poskytuje běžné funkce hello vyžaduje další součásti sady toolkit.
   * **Průzkumník Azure pro Eclipse**: Tato součást poskytuje běžné funkce hello vyžaduje další součásti sady toolkit.
   * **Azure modul plug-in pro Eclipse s Javou**: Tato součást poskytuje podporu pro vývoj projekty, které pomáhají vytváření, testování a nasazení aplikací v jazyce Java pro Microsoft Azure cloud hello v prostředí Eclipse a prostřednictvím příkazového řádku.
   * **Azure modulu plug-in webové aplikace s Javou**: Tato součást poskytuje podporu pro nasazení Java webové aplikace tooMicrosoft webové aplikace Azure kontejnery.
   * **4.2 ovladač JDBC Microsoft pro systém SQL Server**: Tato součást poskytuje JDBC rozhraní API pro SQL Server a Microsoft Azure SQL Database pro Java platformy Enterprise Edition 8.
   * **Balíček pro Apache Qpid klientské knihovny pro JMS**: Tato součást poskytuje hello JMS klientskou součást z hello Apache Qpid projektu tooenable vaší aplikace toouse AMQP zasílání zpráv ve službě Microsoft Azure.
   * **Balíček pro knihovny Microsoft Azure Libraries for Java**: Tato součást poskytuje rozhraní API pro přístup ke službám Microsoft Azure, jako je například úložiště, služby service bus, běh služby atd.
6. Klikněte na **Další**. (Pokud při instalaci nástrojů hello dochází neobvyklou zpoždění, ověřte, že **obraťte se na všech lokalitách během aktualizace instalovat software toofind požadované** není zaškrtnuta.)
7. V hello **nainstalovat podrobnosti** dialogové okno, klikněte na tlačítko **Další**.
   
    ![Přečtěte si podrobné informace o instalaci][03]
8. V hello **zkontrolujte licence** dialogové okno, přečtěte si podmínky licenční smlouvy hello hello. Pokud podmínky hello hello licenční smlouvy souhlasíte, klikněte na tlačítko **přijímám podmínky licenční smlouvy hello hello** a pak klikněte na **Dokončit**. (hello zbývající kroky předpokládají, že vyjadřujete souhlas s podmínkami hello hello licenčních smluv. Pokud nesouhlasíte hello podmínky hello licenčních smluv, ukončete proces instalace hello.)
   
    ![Zkontrolujte licencí][04]
   
    Eclipse bude stáhněte a nainstalujte požadované balíčky hello.
   
    ![Průběh instalace][05]
9. Pokud se zobrazí výzva toorestart Eclipse toocomplete instalace, klikněte na tlačítko **Ano**.
   
    ![Restartujte řádku][06]

## <a name="see-also"></a>Viz také
Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující odkazy:

* [Azure nástrojů pro Eclipse]
  * [Co je nového v hello nástrojů Azure pro Eclipse]
  * *Instalace hello nástrojů Azure pro Eclipse (v tomto článku)*
  * [Přihlášení v pokyny pro hello nástrojů Azure pro Eclipse]
  * [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Co je nového v hello nástrojů Azure pro IntelliJ]
  * [Instalace hello Azure Toolkit pro IntelliJ]
  * [Přihlášení v pokyny pro hello nástrojů Azure pro IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Přihlášení v pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Přihlášení v pokyny pro hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->
