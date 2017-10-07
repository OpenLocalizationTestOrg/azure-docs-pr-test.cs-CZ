---
title: "účty úložiště aaaManage pomocí hello Průzkumník Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak toomanage úložiště Azure účtů pomocí hello Průzkumník Azure pro Eclipse."
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
ms.openlocfilehash: b7ec53e77e3c96d87754b9a658d31e6182121b22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-storage-accounts-by-using-hello-azure-explorer-for-eclipse"></a>Správa účtů úložiště pomocí hello Průzkumník Azure pro Eclipse

Hello Průzkumník Azure, který je součástí hello nástrojů Azure pro prostředí Eclipse, poskytuje vývojáře v jazyce Java s řešením pro snadné použití ke správě účtů úložiště v účtu Azure z uvnitř hello Eclipse integrované vývojové prostředí (IDE).

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-eclipse"></a>Vytvořit účet úložiště v prostředí Eclipse

toocreate účet úložiště pomocí hello Azure Explorer hello následující:

1. Přihlaste se pomocí hello tooyour účet Azure [přihlášení pokyny pro hello nástrojů Azure pro Eclipse].

2. V hello **Azure Explorer** , rozbalte hello **Azure** uzel, klikněte pravým tlačítkem na **účty úložiště**a pak klikněte na tlačítko **vytvořit účet úložiště**.

   ![Vytvořit účet úložiště příkaz][CS01]

3. V hello **vytvořit účet úložiště** dialogovém okně zadejte hello následující možnosti:

   ![Vytvořit nový účet úložiště, dialogové okno][CS02]

   * **Název**: Určuje název hello hello nový účet úložiště.

   * **Předplatné**: Určuje hello předplatné Azure, který má toouse pro nový účet úložiště hello.

   * **Skupina prostředků**: Určuje hello skupinu prostředků pro virtuální počítač. Vyberte jednu z hello následující možnosti:
      * **Vytvořit nový**: Určuje, že chcete toocreate novou skupinu prostředků.
      * **Použít existující**: Určuje, že vyberete ze seznamu skupin prostředků, které jsou spojeny s vaším účtem Azure.

   * **Oblast**: Určuje hello umístění, které se vytvoří účet úložiště (například "Západní USA").

   * **Účet druh**: Určuje typ hello toocreate účtu úložiště (například "úložiště objektů Blob"). Další informace najdete v tématu [účty Azure storage].

   * **Výkon**: Určuje, který účet úložiště nabídky toouse od vydavatele vybrané hello (například "Premium"). Další informace najdete v tématu [úložiště Azure škálovatelnosti a cílech výkonnosti].

   * **Replikace**: Určuje hello replikace pro účet úložiště hello (například "Zónově redundantní"). Další informace najdete v tématu [replikace Azure storage].

4. Pokud jste zadali všechny hello předcházející možnosti, klikněte na tlačítko **vytvořit**.

## <a name="create-a-storage-container-in-eclipse"></a>Vytvoření kontejneru úložiště v prostředí Eclipse

toocreate kontejner úložiště pomocí hello Azure Explorer hello následující:

1. V hello **Azure Explorer** zobrazit, klikněte pravým tlačítkem na účet úložiště hello, kam chcete toocreate kontejner a pak klikněte na tlačítko **vytvořit kontejner objektů blob**.

   ![Vytvoření příkazu kontejner objektů blob][CC01]

2. V hello **vytvořit kontejner objektů blob** dialogové okno, zadejte název hello pro váš kontejner a potom klikněte na **OK**. Další informace o pojmenovávání kontejnerů úložiště najdete v tématu [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat].

   ![Vytvoření dialogového okna kontejner objektů blob][CC02]

## <a name="delete-a-storage-container-in-eclipse"></a>Odstranit kontejner úložiště v prostředí Eclipse

toodelete kontejner úložiště pomocí hello Azure Explorer hello následující:

1. V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na kontejner úložiště hello a pak klikněte na tlačítko **odstranit**.

   ![Odstranit příkaz kontejneru úložiště][DC01]

2. V okně potvrzení hello klikněte **OK**.

   ![Odstranit okno pro potvrzení kontejneru úložiště][DC02]

## <a name="delete-a-storage-account-in-eclipse"></a>Odstranit účet úložiště v prostředí Eclipse

toodelete účet úložiště pomocí hello Azure Explorer hello následující:

1. V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na účet úložiště hello a pak klikněte na tlačítko **odstranit**.

   ![Příkaz účet úložiště odstranit][DS01]

2. V okně potvrzení hello klikněte **OK**.

   ![Odstranit okno pro potvrzení účtu úložiště][DS02]

## <a name="next-steps"></a>Další kroky
Další informace o účtech Azure storage, velikosti a cenách najdete v části hello následující prostředky:

* [Úvod tooMicrosoft Azure Storage]
* [účty Azure storage]
* Účet služby Azure storage velikosti
  * [Velikosti pro účty úložiště v systému Windows v Azure]
  * [Velikosti pro Linux účty úložiště v Azure]
* Účet služby Azure storage – ceny
  * [Ceny účtu úložiště systému Windows]
  * [Ceny Linux účet úložiště]

Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující prostředky:

* [Azure nástrojů pro Eclipse]
  * [Co je nového v hello nástrojů Azure pro Eclipse]
  * [Instalace hello nástrojů Azure pro Eclipse]
  * [přihlášení pokyny pro hello nástrojů Azure pro Eclipse]
  * [Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Co je nového v hello nástrojů Azure pro IntelliJ]
  * [Instalace hello Azure Toolkit pro IntelliJ]
  * [Přihlášení pokyny pro hello Azure Toolkit IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[přihlášení pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Přihlášení pokyny pro hello Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

[Úvod tooMicrosoft Azure Storage]: /azure/storage/storage-introduction
[účty Azure storage]: /azure/storage/storage-create-storage-account
[replikace Azure storage]: /azure/storage/storage-redundancy
[Škálovatelnost a cíle výkonnosti Azure storage]: /azure/storage/storage-scalability-targets
[pojmenování a odkazování na kontejnerů, objektů BLOB a metadat]: http://go.microsoft.com/fwlink/?LinkId=255555

[Velikosti pro účty úložiště v systému Windows v Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Velikosti pro Linux účty úložiště v Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Ceny účtu úložiště systému Windows]: /pricing/details/virtual-machines/windows/
[Ceny Linux účet úložiště]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC02.png
