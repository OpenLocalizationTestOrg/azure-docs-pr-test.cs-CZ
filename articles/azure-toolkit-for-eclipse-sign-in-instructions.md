---
title: "Přihlaste se pokyny pro Azure nástrojů Eclipse | Microsoft Docs"
description: "Zjistěte, jak se přihlásit ke službě Microsoft Azure pomocí sady nástrojů Azure pro Eclipse."
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
ms.openlocfilehash: 02dd9935086c4c40d9ed54cc9ff2412ca96889f5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>Azure přihlášení pokyny pro Azure Toolkit pro Eclipse

Sada nástrojů Azure pro Eclipse nabízí dvě metody pro přihlášení k účtu Azure:

  * **Interaktivní** – při použití této metody bude zadejte vaše přihlašovací údaje Azure pokaždé, když jste přihlášení ke svému účtu Azure.
  * **Automatizované** – při použití této metody vytvoříte soubor přihlašovacích údajů, který obsahuje hlavní data vaší služby, po které můžete použít soubor s přihlašovacími údaji se automaticky přihlásit ke svému účtu Azure.

Kroky v následujících částech se popisují způsob použití každé metody.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a>Interaktivní přihlášení k účtu Azure

Následující postup se ukazují, jak pro přihlášení do Azure tak, že ručně zadáte vaše přihlašovací údaje Azure.

1. Otevřete projekt s Eclipse.

1. Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.

   ![Eclipse nabídky pro Azure přihlášení][I01]

1. Když **přihlásit k Azure** se zobrazí dialogové okno, vyberte **interaktivní**a potom klikněte na **přihlásit**.

   ![Přihlaste se dialogové okno][I02]

1. Když **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlásit**.

   ![Dialogové okno Přihlášení do Azure][I03]

1. Když **vyberte odběry** se zobrazí dialogové okno, vyberte předplatné, které chcete použít a pak klikněte na **OK**.

   ![Vyberte předplatné, dialogové okno][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a>Podepisování mimo účtu Azure, když jste přihlášení interaktivně

Po konfiguraci podle kroků v předchozí části, budete automaticky odhlášení ze účtu Azure při každém restartování prostředí Eclipse. Pokud chcete odhlásit z účtu Azure bez restartování prostředí Eclipse, ale použijte následující kroky.

1. V prostředí Eclipse, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **Odhlásit**.

   ![Eclipse nabídky pro Azure odhlášení][L01]

1. Když **Azure Odhlásit** se zobrazí dialogové okno, klikněte na tlačítko **Ano**.

   ![Odhlásit se dialogové okno][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a>Přihlášení k účtu Azure automaticky a vytvoření souboru přihlašovací údaje používat v budoucnosti

Následující kroky vás procesem vytvoření pověření souboru, který obsahuje hlavní data služby. Po dokončení těchto kroků Eclipse automaticky používat soubor s přihlašovacími údaji automaticky pro přihlášení do Azure při každém otevření projektu.

1. Otevřete projekt s Eclipse.

1. Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.

   ![Eclipse nabídky pro Azure přihlášení][A01]

1. Když **přihlásit k Azure** se zobrazí dialogové okno, vyberte **automatizovaná**a potom klikněte na **nový**.

   ![Přihlaste se dialogové okno][A02]

1. Když **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlásit**.

   ![Dialogové okno Přihlášení do Azure][A03]

1. Když **vytvoření ověřování souborů** se zobrazí dialogové okno, vyberte předplatné, které chcete použít, vyberte cílový adresář a pak klikněte na tlačítko **spustit**.

   ![Dialogové okno Přihlášení do Azure][A04]

1. **Instanční objekt Creatation stav** zobrazí se dialogové okno a poté, co vaše soubory byly úspěšně vytvořeny, klikněte na tlačítko **OK**.

   ![Dialogové okno Stav Creatation instančního objektu][A05]

1. Když **přihlásit k Azure** se zobrazí dialogové okno, klikněte na tlačítko **přihlásit**.

   ![Dialogové okno Přihlášení do Azure][A06]

1. Když **vyberte odběry** se zobrazí dialogové okno, vyberte předplatné, které chcete použít a pak klikněte na **OK**.

   ![Vyberte předplatné, dialogové okno][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>Podepisování mimo účtu Azure, když jste přihlášení automaticky

Po konfiguraci podle kroků v předchozí části, se můžete ke svému účtu Azure při každém restartování prostředí Eclipse přihlásit automaticky nástrojů Azure. Pokud chcete odhlásit z účtu Azure a zabránit automatickému přihlašování nástrojů Azure, použijte následující postup.

1. V prostředí Eclipse, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **Odhlásit**.

   ![Eclipse nabídky pro Azure odhlášení][L01]

1. Když **Azure Odhlásit** se zobrazí dialogové okno, klikněte na tlačítko **Ano**.

   ![Odhlásit se dialogové okno][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>Přihlášení k účtu Azure automaticky pomocí pověření souboru, který jste již vytvořili

Pokud podepíšete mimo Azure, pokud používáte Eclipse, musíte znovu nakonfigurovat sady nástrojů Azure pro prostředí Eclipse, aby použít soubor přihlašovacích údajů, který jste vytvořili předtím, než můžete automaticky přihlásit do váš účet Azure. Následující postup vás provede konfigurace sady nástrojů Azure používat existující soubor přihlašovacích údajů.

1. Otevřete projekt s Eclipse.

1. Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.

   ![Eclipse nabídky pro Azure přihlášení][A01]

1. Když **přihlásit k Azure** se zobrazí dialogové okno, vyberte **automatizovaná**a potom klikněte na **Procházet**.

   ![Přihlaste se dialogové okno][A02]

1. Když **vyberte ověřit soubor** dialogové okno se zobrazí, vyberte soubor přihlašovacích údajů, který jste předtím vytvořili a pak klikněte na tlačítko **vyberte**.

   ![Přihlaste se dialogové okno][A08]

1. Když **přihlásit k Azure** se zobrazí dialogové okno, klikněte na tlačítko **přihlásit**.

   ![Dialogové okno Přihlášení do Azure][A06]

1. Když **vyberte odběry** se zobrazí dialogové okno, vyberte předplatné, které chcete použít a pak klikněte na **OK**.

   ![Vyberte předplatné, dialogové okno][A07]

## <a name="see-also"></a>Viz také
Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:

* [Azure nástrojů pro Eclipse]
  * [Novinky v sadě Azure Toolkit pro Eclipse]
  * [Instalace sady Azure Toolkit pro Eclipse]
  * *Přihlaste se pokyny pro Azure nástrojů Eclipse (v tomto článku)*
  * [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Novinky v sadě Azure Toolkit pro IntelliJ]
  * [Instalace sady Azure Toolkit pro IntelliJ]
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Novinky v sadě Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Novinky v sadě Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

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
