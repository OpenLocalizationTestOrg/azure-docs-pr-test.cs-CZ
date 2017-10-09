---
title: "aaaSign v pokyny pro hello nástrojů Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak hello toosign do Microsoft Azure pomocí nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 95be64750ca0147f76dae8f364fad80cb9ccc969
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sign-in-instructions-for-hello-azure-toolkit-for-eclipse"></a>Azure přihlášení v pokyny pro hello nástrojů Azure pro Eclipse

Hello nástrojů Azure pro prostředí Eclipse nabízí dvě metody pro přihlášení k účtu Azure:

  * **Interaktivní** – při použití této metody bude zadejte vaše přihlašovací údaje Azure pokaždé, když jste přihlášení ke svému účtu Azure.
  * **Automatizované** – při použití této metody vytvoříte soubor přihlašovacích údajů, který obsahuje hlavní data vaší služby, po které můžete použít hello přihlašovací údaje souboru tooautomatically přihlášení ke svému účtu Azure.

Hello kroky v hello následující části se popisují, jak toouse každá z metod.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a>Interaktivní přihlášení k účtu Azure

Hello následující kroky ilustruje způsob toosign do Azure tak, že ručně zadáte vaše přihlašovací údaje Azure.

1. Otevřete projekt s Eclipse.

1. Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.

   ![Eclipse nabídky pro Azure přihlášení][I01]

1. Když hello **přihlásit k Azure** se zobrazí dialogové okno, vyberte **interaktivní**a potom klikněte na **přihlásit**.

   ![Přihlaste se dialogové okno][I02]

1. Když hello **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlásit**.

   ![Dialogové okno Přihlášení do Azure][I03]

1. Když hello **vyberte odběry** zobrazí se dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.

   ![Vyberte předplatné, dialogové okno][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a>Podepisování mimo účtu Azure, když jste přihlášení interaktivně

Po nakonfigurování hello kroky v předchozí části hello budete automaticky odhlášení ze účtu Azure při každém restartování prostředí Eclipse. Pokud chcete toosign mimo účtu Azure bez restartování prostředí Eclipse, ale použijte hello následující kroky.

1. V prostředí Eclipse, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **Odhlásit**.

   ![Eclipse nabídky pro Azure odhlášení][L01]

1. Když hello **Azure Odhlásit** se zobrazí dialogové okno, klikněte na tlačítko **Ano**.

   ![Odhlásit se dialogové okno][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-toouse-in-hello-future"></a>Přihlášení k účtu Azure automaticky a vytváření přihlašovacích údajů souboru toouse v hello budoucí

Hello následující postup vás provede vytváření pověření souboru, který obsahuje hlavní data služby. Po dokončení těchto kroků, Eclipse bude automaticky použití hello přihlašovací údaje souboru tooautomatically přihlášení, že jste do Azure každý času jste otevřete projekt.

1. Otevřete projekt s Eclipse.

1. Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.

   ![Eclipse nabídky pro Azure přihlášení][A01]

1. Když hello **přihlásit k Azure** se zobrazí dialogové okno, vyberte **automatizovaná**a potom klikněte na **nový**.

   ![Přihlaste se dialogové okno][A02]

1. Když hello **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlásit**.

   ![Dialogové okno Přihlášení do Azure][A03]

1. Když hello **vytvoření souborů ověřování** zobrazí se dialogové okno, vyberte hello odběry mají toouse, vyberte cílový adresář a pak klikněte na tlačítko **spustit**.

   ![Dialogové okno Přihlášení do Azure][A04]

1. Hello **instanční objekt Creatation stav** zobrazí se dialogové okno a poté, co vaše soubory byly úspěšně vytvořeny, klikněte na tlačítko **OK**.

   ![Dialogové okno Stav Creatation instančního objektu][A05]

1. Když hello **přihlásit k Azure** se zobrazí dialogové okno, klikněte na tlačítko **přihlásit**.

   ![Dialogové okno Přihlášení do Azure][A06]

1. Když hello **vyberte odběry** zobrazí se dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.

   ![Vyberte předplatné, dialogové okno][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>Podepisování mimo účtu Azure, když jste přihlášení automaticky

Po nakonfigurování hello kroky v předchozí části hello hello nástrojů Azure automaticky podepíše můžete ke svému účtu Azure při každém restartování prostředí Eclipse. Ale toosign mimo účtu Azure a zabránit hello nástrojů Azure automaticky hello použijte následující kroky vás přihlásit.

1. V prostředí Eclipse, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **Odhlásit**.

   ![Eclipse nabídky pro Azure odhlášení][L01]

1. Když hello **Azure Odhlásit** se zobrazí dialogové okno, klikněte na tlačítko **Ano**.

   ![Odhlásit se dialogové okno][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>Přihlášení k účtu Azure automaticky pomocí pověření souboru, který jste již vytvořili

Pokud podepíšete mimo Azure, pokud používáte Eclipse, je nutné tooreconfigure hello Azure Toolkit pro Eclipse toouse soubor přihlašovacích údajů, který jste vytvořili předtím, než můžete automaticky přihlásit do váš účet Azure. Hello následující postup vás provede konfiguraci hello Azure Toolkit toouse existující soubor přihlašovacích údajů.

1. Otevřete projekt s Eclipse.

1. Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.

   ![Eclipse nabídky pro Azure přihlášení][A01]

1. Když hello **přihlásit k Azure** se zobrazí dialogové okno, vyberte **automatizovaná**a potom klikněte na **Procházet**.

   ![Přihlaste se dialogové okno][A02]

1. Když hello **vyberte ověřit soubor** dialogové okno se zobrazí, vyberte soubor přihlašovacích údajů, který jste předtím vytvořili a pak klikněte na tlačítko **vyberte**.

   ![Přihlaste se dialogové okno][A08]

1. Když hello **přihlásit k Azure** se zobrazí dialogové okno, klikněte na tlačítko **přihlásit**.

   ![Dialogové okno Přihlášení do Azure][A06]

1. Když hello **vyberte odběry** zobrazí se dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.

   ![Vyberte předplatné, dialogové okno][A07]

## <a name="see-also"></a>Viz také
Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující odkazy:

* [Azure nástrojů pro Eclipse]
  * [Co je nového v hello nástrojů Azure pro Eclipse]
  * [Instalace hello nástrojů Azure pro Eclipse]
  * *Přihlášení v pokyny pro hello nástrojů Azure pro Eclipse (v tomto článku)*
  * [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Co je nového v hello nástrojů Azure pro IntelliJ]
  * [Instalace hello Azure Toolkit pro IntelliJ]
  * [Přihlášení v pokyny pro hello nástrojů Azure pro IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Sign In Instructions for hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Přihlášení v pokyny pro hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
