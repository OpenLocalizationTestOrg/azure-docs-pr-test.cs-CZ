---
title: Instalace Azure Toolkit pro IntelliJ | Microsoft Docs
description: "Naučte se nainstalovat sadu Azure pro IntelliJ IDEA."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: bf11a8580500f78c4a96a02953f221501eeffe6c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="installing-the-azure-toolkit-for-intellij"></a>Instalace Azure Toolkit pro IntelliJ
Sada Azure pro IntelliJ obsahuje šablony a funkce, které vám umožní snadno vytvářet, vývoj, testování a nasazení aplikace na platformě Azure pomocí IntelliJ IDEA vývojové prostředí. Sada nástrojů Azure pro IntelliJ je projekt Open Source, jejichž zdrojový kód je k dispozici v části licencí MIT z lokality projektu na Githubu na následující adrese URL:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Existují dvě metody instalace nástrojů Azure pro IntelliJ z dialogového okna nastavení a v nabídce konfigurovat na obrazovce start; Obě metody instalace je popsána v následujících krocích.

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a>K instalaci sady nástrojů Azure pro IntelliJ z dialogové okno nastavení
1. Spusťte IntelliJ IDEA.
2. Když IntelliJ IDEA otevře, klikněte na tlačítko **soubor**, pak klikněte na tlačítko **nastavení**.
   
    ![Otevřete dialogové okno IntelliJ IDEA nastavení][01a]
3. V dialogovém okně nastavení klikněte na **modulů plug-in**a potom klikněte na **procházet úložiště**.
   
    ![Dialogové okno nastavení IntelliJ IDEA][02a]
4. V **procházet úložiště** dialogové okno vyhledávacího pole zadejte "Azure". Zvýrazněte **nástrojů Azure pro IntelliJ**a potom klikněte na **nainstalovat**.
   
    ![Vyhledejte IntelliJ Azure sady Toolkit][03]
   
    IntelliJ IDEA zobrazí průběh instalace v dialogovém okně.
   
    ![Průběh instalace][04]
5. Po dokončení instalace, klikněte na tlačítko **restartujte IntelliJ IDEA**.
   
    ![Restartujte IntelliJ IDEA][05]
6. Klikněte na tlačítko **OK** zavřete dialogové okno nastavení.
   
    ![Zavřete dialogové okno nastavení IntelliJ IDEA][06]
7. Po zobrazení výzvy k restartování IntelliJ IDEA nebo odložit, klikněte na **restartujte**.
   
    ![Restartujte IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a>K instalaci sady nástrojů Azure pro IntelliJ z obrazovky start
1. Spusťte IntelliJ IDEA.
2. Jakmile se zobrazí na úvodní obrazovce IntelliJ IDEA, klikněte na tlačítko **konfigurace**, pak klikněte na tlačítko **modulů plug-in**.
   
    ![Instalace modulů plug-in IntelliJ IDEA][01b]
3. V **modulů plug-in** dialogové okno, klikněte na tlačítko **procházet úložiště**.
   
    ![Procházet IntelliJ IDEA modulu plug-in úložiště][02b]
4. V **procházet úložiště** dialogové okno vyhledávacího pole zadejte "Azure". Zvýrazněte **nástrojů Azure pro IntelliJ**a potom klikněte na **nainstalovat**.
   
    ![Vyhledejte IntelliJ Azure sady Toolkit][03]
   
    IntelliJ IDEA zobrazí průběh instalace v dialogovém okně.
   
    ![Průběh instalace][04]
5. Po dokončení instalace, klikněte na tlačítko **restartujte IntelliJ IDEA**.
   
    ![Restartujte IntelliJ IDEA][05]
6. Po zobrazení výzvy k restartování IntelliJ IDEA nebo odložit, klikněte na **restartujte**.
   
    ![Restartujte IntelliJ IDEA][07]

## <a name="see-also"></a>Viz také
Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:

* [Azure nástrojů pro Eclipse]
  * [Novinky v sadě Azure Toolkit pro Eclipse]
  * [Instalace sady Azure Toolkit pro Eclipse]
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse]
  * [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Novinky v sadě Azure Toolkit pro IntelliJ]
  * *Instalace Azure Toolkit pro IntelliJ (v tomto článku)*
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Novinky v sadě Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Novinky v sadě Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01a]: ./media/azure-toolkit-for-intellij-installation/01-intellij-file-settings.png
[01b]: ./media/azure-toolkit-for-intellij-installation/01-intellij-configure-dropdown.png
[02a]: ./media/azure-toolkit-for-intellij-installation/02-intellij-settings-dialog.png
[02b]: ./media/azure-toolkit-for-intellij-installation/02-intellij-plugins-dialog.png
[03]: ./media/azure-toolkit-for-intellij-installation/03-intellij-browse-repositories.png
[04]: ./media/azure-toolkit-for-intellij-installation/04-install-progress.png
[05]: ./media/azure-toolkit-for-intellij-installation/05-restart-intellij.png
[06]: ./media/azure-toolkit-for-intellij-installation/06-intellij-settings-dialog.png
[07]: ./media/azure-toolkit-for-intellij-installation/07-restart-intellij.png
