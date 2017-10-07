---
title: aaaSign v pokyny pro hello Azure Toolkit IntelliJ | Microsoft Docs
description: "Zjistěte, jak hello toosign v tooMicrosoft Azure pomocí Azure Toolkit pro IntelliJ."
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
ms.openlocfilehash: 2de781fc19267cce133b1e6456481497e165fce4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-instructions-for-hello-azure-toolkit-for-intellij"></a>Přihlášení pokyny pro hello Azure Toolkit IntelliJ

Hello nástrojů Azure pro IntelliJ nabízí dvě metody pro přihlášení tooyour účet Azure:

  * **Interaktivní**: zadáte pokaždé, když jste přihlášení pro přihlašovací údaje Azure v tooyour účet Azure.
  * **Automatizované**: vytvoření souboru přihlašovací údaje používané přihlašovací tooautomatically v tooyour účet Azure.

Hello následující části popisují, jak toouse každá z metod.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-tooyour-azure-account-interactively"></a>Interaktivní přihlášení tooyour účet Azure

toosign v tooAzure ručně zadáním vaše přihlašovací údaje Azure hello následující:

1. Otevřete projekt s IntelliJ IDEA.

2. Klikněte na tlačítko **nástroje**, bod příliš**Azure**a potom klikněte na **přihlásit k Azure**.

   ![Hello příkaz IntelliJ Azure Sign In][I01]

3. V hello **přihlásit k Azure** vyberte **interaktivní**a potom klikněte na **přihlášení**.

   ![Hello přihlásit k Azure okno s interaktivní vybrané][I02]

4. V hello **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlášení**.

   ![Hello Azure přihlášení dialogového okna][I03]

5. V hello **vyberte odběry** dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.

   ![Dialogové okno Vybrat odběry Hello][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a>Po registraci interaktivním se odhlásit z účtu Azure

Po dokončení konfigurace účtu pomocí hello předchozích kroků automaticky odhlásíte se z účtu Azure pokaždé, když je restartovat IntelliJ IDEA. Ale pokud chcete toosign mimo účtu Azure *bez* restartování IntelliJ IDEA hello následující.

1. V IntelliJ IDEA na hello **nástroje** nabídce bodu příliš**Azure**a potom klikněte na **Azure Odhlásit**.

   ![Hello IntelliJ Azure přihlašovací Vystoupením – příkaz][L01]

2. V hello **Azure Odhlásit** okno pro potvrzení, klikněte na tlačítko **Ano**.

   ![Hello Azure odhlásit potvrzovacím okně][L02]

## <a name="sign-in-tooyour-azure-account-automatically"></a>Automaticky přihlásit tooyour účet Azure

Tato část vás provede procesem vytváření pověření souboru, který obsahuje hlavní data služby. Po dokončení tohoto procesu, otevřete prostředí Eclipse používá hello přihlašovací údaje souboru tooautomatically přihlášení, že v každém tooAzure času jste projekt.

1. Otevřete projekt s IntelliJ IDEA.

2. Na hello **nástroje** nabídce bodu příliš**Azure**a potom klikněte na **přihlásit k Azure**.

   ![Hello příkaz IntelliJ Azure Sign In][A01]

3. V hello **přihlásit k Azure** vyberte **automatizovaná**a potom klikněte na **nový**.

   ![Hello přihlásit k Azure okno s automatizovaná vybrané][A02]

4. V hello **dialogovém okně pro přihlášení k Azure** okno, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlášení**.

   ![Hello Azure přihlášení dialogového okna][A03]

5. V hello **vytvářet soubory ověřování** okno, vyberte hello odběry mají toouse, vyberte cílový adresář a pak klikněte na tlačítko **spustit**.

   ![okno vytvořit soubory ověřování Hello][A04]

6. V hello **stav vytváření objektu služby** dialogové okno, jakmile vaše soubory byly úspěšně vytvořeny, klikněte na tlačítko **OK**.

   ![Hello dialogové okno Stav vytvoření instančního objektu][A05]

7. V hello **přihlásit k Azure** okně klikněte na tlačítko **přihlášení**.

   ![Dialogové okno Přihlášení do Azure][A06]

8. V hello **vyberte odběry** dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.

   ![Dialogové okno Vybrat odběry Hello][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a>Po přihlášení automaticky se odhlásit z účtu Azure

Po dokončení konfigurace účtu pomocí hello předchozích kroků hello nástrojů Azure automaticky přihlašovat při tooyour pokaždé, když je restartovat IntelliJ IDEA účet Azure. Ale toosign mimo účtu Azure a zabránit hello nástrojů Azure z vás přihlásit automaticky, hello následující:

1. V IntelliJ IDEA na hello **nástroje** nabídce bodu příliš**Azure**a potom klikněte na **Azure Odhlásit**.

   ![Hello IntelliJ Azure přihlašovací Vystoupením – příkaz][L01]

2. V hello **Azure Odhlásit** okno pro potvrzení, klikněte na tlačítko **Ano**.

   ![Hello Azure odhlásit potvrzovacím okně][L03]

## <a name="sign-in-tooyour-azure-account-automatically-by-using-an-existing-credentials-file"></a>Přihlaste se tooyour účtu Azure automaticky pomocí existující soubor přihlašovacích údajů

Pokud podepíšete mimo účtu Azure, když používáte IntelliJ IDEA, je nutné použít symbolem tooautomatically existující soubor přihlašovacích údajů zpět v toohello účtu. tooconfigure hello Azure Toolkit pro Eclipse toouse existující soubor přihlašovacích údajů, hello následující:

1. Otevřete projekt s IntelliJ IDEA.

2. Na hello **nástroje** nabídce bodu příliš**Azure**a potom klikněte na **přihlásit k Azure**.

   ![Hello příkaz IntelliJ Azure Sign In][A01]

3. V hello **přihlásit k Azure** vyberte **automatizovaná**a potom klikněte na **Procházet**.

   ![Hello přihlásit k Azure okno s automatizovaná vybrané][A02]

4. V hello **vyberte soubor pro ověření** dialogové okno, vyberte soubor dříve vytvořenou přihlašovací údaje a pak klikněte na tlačítko **vyberte**.

   ![Dialogové okno Vybrat soubor pro ověření Hello][A08]

5. V hello **přihlásit k Azure** okně klikněte na tlačítko **přihlášení**.

   ![Hello přihlásit k Azure okno s automatizovaná vybrané][A06]

6. V hello **vyberte odběry** dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.

   ![Dialogové okno Vybrat odběry Hello][A07]

## <a name="next-steps"></a>Další kroky
Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující odkazy:

* [Azure nástrojů pro Eclipse]
  * [Co je nového v hello nástrojů Azure pro Eclipse]
  * [Instalace hello nástrojů Azure pro Eclipse]
  * [Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]
  * [Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Co je nového v hello nástrojů Azure pro IntelliJ]
  * [Instalace hello Azure Toolkit pro IntelliJ]
  * *Přihlášení pokyny pro hello Azure Toolkit IntelliJ* (v tomto článku)
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Sign-in instructions for hello Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure středisko pro vývojáře v jazyce Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
