---
title: "Přihlášení pokyny pro Azure Toolkit IntelliJ | Microsoft Docs"
description: "Zjistěte, jak se přihlásit do služby Microsoft Azure pomocí sady nástrojů Azure pro IntelliJ."
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
ms.openlocfilehash: 4e2ed072bdaea0a71fef042c0c72b7656a42bbe8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>Přihlášení pokyny pro Azure Toolkit IntelliJ

Sada nástrojů Azure pro IntelliJ nabízí dvě metody pro přihlášení k účtu Azure:

  * **Interaktivní**: Zadejte vaše přihlašovací údaje Azure při každém přihlášení k účtu Azure.
  * **Automatizované**: Vytvořte soubor přihlašovacích údajů, který můžete použít automatické přihlášení k účtu Azure.

Následující části popisují způsob použití každé metody.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-interactively"></a>Interaktivní přihlášení k účtu Azure

Přihlaste se k Azure tak, že ručně zadáte vaše přihlašovací údaje Azure, takto:

1. Otevřete projekt s IntelliJ IDEA.

2. Klikněte na tlačítko **nástroje**, přejděte na příkaz **Azure**a potom klikněte na **přihlásit k Azure**.

   ![Příkaz IntelliJ Azure Sign In][I01]

3. V **přihlásit k Azure** vyberte **interaktivní**a potom klikněte na **přihlášení**.

   ![Okno Azure přihlásit s interaktivní vybrané][I02]

4. V **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlášení**.

   ![Okno dialogovém okně pro přihlášení k Azure][I03]

5. V **vyberte odběry** dialogové okno Vyberte předplatné, které chcete použít a pak klikněte na tlačítko **OK**.

   ![Dialogové okno Vybrat odběrů][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a>Po registraci interaktivním se odhlásit z účtu Azure

Po dokončení konfigurace účtu pomocí předchozího postupu automaticky odhlásíte se z účtu Azure pokaždé, když je restartovat IntelliJ IDEA. Ale pokud se chcete odhlásit z účtu Azure *bez* restartování IntelliJ IDEA, postupujte takto.

1. V IntelliJ IDEA na **nástroje** nabídky, přejděte na příkaz **Azure**a potom klikněte na **Azure Odhlásit**.

   ![Příkaz IntelliJ Azure odhlásit][L01]

2. V **Azure Odhlásit** okno pro potvrzení, klikněte na tlačítko **Ano**.

   ![Okno potvrzení Azure odhlásit][L02]

## <a name="sign-in-to-your-azure-account-automatically"></a>Přihlaste se k účtu Azure automaticky

Tato část vás provede procesem vytváření pověření souboru, který obsahuje hlavní data služby. Po dokončení tohoto procesu se používá Eclipse automaticky pro přihlášení do Azure při každém otevření projektu soubor s přihlašovacími údaji.

1. Otevřete projekt s IntelliJ IDEA.

2. Na **nástroje** nabídky, přejděte na příkaz **Azure**a potom klikněte na **přihlásit k Azure**.

   ![Příkaz IntelliJ Azure Sign In][A01]

3. V **přihlásit k Azure** vyberte **automatizovaná**a potom klikněte na **nový**.

   ![Okno Azure přihlásit s automatizovaná vybrané][A02]

4. V **dialogovém okně pro přihlášení k Azure** okno, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlášení**.

   ![Okno dialogovém okně pro přihlášení k Azure][A03]

5. V **vytvářet soubory ověřování** okně vyberte předplatné, které chcete použít, vyberte cílový adresář a pak klikněte na tlačítko **spustit**.

   ![Okno vytvořit soubory ověřování][A04]

6. V **stav vytváření objektu služby** dialogové okno, jakmile vaše soubory byly úspěšně vytvořeny, klikněte na tlačítko **OK**.

   ![Dialogové okno Stav vytvoření instančního objektu][A05]

7. V **přihlásit k Azure** okně klikněte na tlačítko **přihlášení**.

   ![Dialogové okno Přihlášení do Azure][A06]

8. V **vyberte odběry** dialogové okno Vyberte předplatné, které chcete použít a pak klikněte na tlačítko **OK**.

   ![Dialogové okno Vybrat odběrů][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a>Po přihlášení automaticky se odhlásit z účtu Azure

Po dokončení konfigurace účtu pomocí předchozího postupu nástrojů Azure automaticky přihlásí můžete ke svému účtu Azure pokaždé, když je restartovat IntelliJ IDEA. Ale pokud chcete odhlásit z účtu Azure a zabránit automatickému přihlašování nástrojů Azure, postupujte takto:

1. V IntelliJ IDEA na **nástroje** nabídky, přejděte na příkaz **Azure**a potom klikněte na **Azure Odhlásit**.

   ![Příkaz IntelliJ Azure odhlásit][L01]

2. V **Azure Odhlásit** okno pro potvrzení, klikněte na tlačítko **Ano**.

   ![Okno potvrzení Azure odhlásit][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a>Přihlásit k účtu Azure automaticky pomocí existující soubor přihlašovacích údajů

Pokud podepíšete mimo účtu Azure, když používáte IntelliJ IDEA, musíte použít existující soubor přihlašovacích údajů se automaticky znovu přihlásit k účtu. Konfigurace sady nástrojů Azure pro prostředí Eclipse, aby používal existující soubor přihlašovacích údajů, postupujte takto:

1. Otevřete projekt s IntelliJ IDEA.

2. Na **nástroje** nabídky, přejděte na příkaz **Azure**a potom klikněte na **přihlásit k Azure**.

   ![Příkaz IntelliJ Azure Sign In][A01]

3. V **přihlásit k Azure** vyberte **automatizovaná**a potom klikněte na **Procházet**.

   ![Okno Azure přihlásit s automatizovaná vybrané][A02]

4. V **vyberte soubor pro ověření** dialogové okno, vyberte soubor dříve vytvořenou přihlašovací údaje a pak klikněte na tlačítko **vyberte**.

   ![Vyberte soubor pro ověření dialogových oken][A08]

5. V **přihlásit k Azure** okně klikněte na tlačítko **přihlášení**.

   ![Okno Azure přihlásit s automatizovaná vybrané][A06]

6. V **vyberte odběry** dialogové okno Vyberte předplatné, které chcete použít a pak klikněte na tlačítko **OK**.

   ![Dialogové okno Vybrat odběrů][A07]

## <a name="next-steps"></a>Další kroky
Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:

* [Azure nástrojů pro Eclipse]
  * [Co je nového v sadě nástrojů Azure pro Eclipse]
  * [Instalace sady Azure Toolkit pro Eclipse]
  * [Pokyny přihlášení k Azure nástrojů pro Eclipse]
  * [Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Co je nového v sadě nástrojů Azure pro IntelliJ]
  * [Instalace sady Azure Toolkit pro IntelliJ]
  * *Přihlášení pokyny pro Azure Toolkit IntelliJ* (v tomto článku)
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Pokyny přihlášení k Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v sadě nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v sadě nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

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
