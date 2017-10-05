---
title: "Spravovat účty úložiště pomocí Průzkumníka Azure pro IntelliJ | Microsoft Docs"
description: "Naučte se spravovat účtům Azure storage pomocí Průzkumníka Azure pro IntelliJ."
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
ms.openlocfilehash: a1b56cc2751fc43a1ad6917eca77eec460f26694
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>Spravovat účty úložiště pomocí Průzkumníka Azure pro IntelliJ

Průzkumník Azure, který je součástí sady nástrojů Azure pro IntelliJ, poskytuje vývojáře v jazyce Java s řešením snadno použitelné pro správu účtů úložiště v Azure účtu z uvnitř IntelliJ integrované vývojové prostředí (IDE).

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a>Vytvořit účet úložiště v IntelliJ

Pokud chcete vytvořit účet úložiště pomocí Průzkumníka Azure, postupujte takto:

1. Přihlaste se k účtu Azure pomocí [pokyny přihlášení k Azure nástrojů pro Eclipse]. 

2. V **Azure Explorer** , rozbalte **Azure** uzel, klikněte pravým tlačítkem na **účty úložiště**a potom klikněte na **vytvořit účet úložiště**.

   ![Vytvořit účet úložiště příkaz][CS01]

3. V **vytvořit účet úložiště** dialogové okno pole, určete následující možnosti:

   ![Vytvořit nový účet úložiště, dialogové okno][CS02]

   * **Název**: Určuje název pro nový účet úložiště.

   * **Účet druh**: Určuje typ účtu úložiště k vytvoření (například "Úložiště objektů Blob"). Další informace najdete v tématu [účty Azure storage]. 

   * **Výkon**: Určuje, který účet úložiště nabídky používat od vydavatele, vybrané (například "Premium"). Další informace najdete v tématu [úložiště Azure škálovatelnosti a cílech výkonnosti]. 

   * **Replikace**: Určuje replikace pro účet úložiště (například "Zónově redundantní"). Další informace najdete v tématu [replikace Azure storage]. 

   * **Předplatné**: Určuje předplatné Azure, kterou chcete použít pro nový účet úložiště.

   * **Umístění**: Určuje umístění, které se vytvoří účet úložiště (například "Západní USA").

   * **Skupina prostředků**: Určuje skupinu prostředků pro virtuální počítač. Vyberte jednu z následujících možností:
      * **Vytvořit nový**: Určuje, že chcete vytvořit novou skupinu prostředků.
      * **Použít existující**: Určuje, že vyberete ze seznamu skupin prostředků, které jsou spojeny s vaším účtem Azure.

4. Když zadáte všechny z předchozích možností klikněte na tlačítko **OK**.

## <a name="create-a-storage-container-in-intellij"></a>Vytvoření kontejneru úložiště v IntelliJ

Pokud chcete vytvořit kontejner úložiště pomocí Průzkumníka Azure, postupujte takto:

1. V zobrazení Průzkumník Azure, klikněte pravým tlačítkem na účet úložiště, kde chcete vytvořit kontejner a potom klikněte na **vytvořit kontejner objektů blob**.

   ![Vytvoření příkazu kontejner objektů blob][CC01]

2. V **vytvořit kontejner objektů blob** dialogové okno, zadejte název pro váš kontejner a pak klikněte na tlačítko **OK**. Další informace o pojmenovávání kontejnerů úložiště najdete v tématu [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat].

   ![Vytvoření dialogového okna kontejneru úložiště][CC02]

## <a name="delete-a-storage-container-in-intellij"></a>Odstranit kontejner úložiště v IntelliJ

Pokud chcete odstranit kontejner úložiště pomocí Průzkumníka Azure, postupujte takto:

1. V zobrazení Průzkumník Azure, klikněte pravým tlačítkem na kontejner úložiště a pak klikněte na tlačítko **odstranit**.

   ![Odstranit příkaz kontejneru úložiště][DC01]

2. V okně potvrzení klikněte na **Ano**.

   ![Odstranit okno pro potvrzení kontejneru úložiště][DC02]

## <a name="delete-a-storage-account-in-intellij"></a>Odstranit účet úložiště v IntelliJ

Pokud chcete odstranit účet úložiště pomocí Průzkumníka Azure, postupujte takto:

1. V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na účet úložiště a pak vyberte **odstranit**.

   ![Odstraňte nabídce účtu úložiště][DS01]

2. V okně potvrzení klikněte na **Ano**.

   ![Odstranit okno pro potvrzení účtu úložiště][DS02]

## <a name="next-steps"></a>Další kroky
Další informace o účtech úložiště Azure velikosti a ceny, najdete v následujících zdrojích informací:

* [Úvod do Microsoft Azure Storage]
* [účty Azure storage]
* Účet služby Azure storage velikosti
  * [Velikosti pro účty úložiště v systému Windows v Azure]
  * [Velikosti pro Linux účty úložiště v Azure]
* Účet služby Azure storage – ceny
  * [Ceny účtu úložiště systému Windows]
  * [Ceny Linux účet úložiště]

Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v následujících zdrojích informací:

* [Azure nástrojů pro Eclipse]
  * [Co je nového v sadě nástrojů Azure pro Eclipse]
  * [Instalace sady Azure Toolkit pro Eclipse]
  * [pokyny přihlášení k Azure nástrojů pro Eclipse]
  * [Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Co je nového v sadě nástrojů Azure pro IntelliJ]
  * [Instalace sady Azure Toolkit pro IntelliJ]
  * [Přihlášení pokyny pro Azure Toolkit IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[pokyny přihlášení k Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Přihlášení pokyny pro Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v sadě nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v sadě nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

[Úvod do Microsoft Azure Storage]: /azure/storage/storage-introduction
[účty Azure storage]: /azure/storage/storage-create-storage-account
[replikace Azure storage]: /azure/storage/storage-redundancy
[Škálovatelnost a cíle výkonnosti Azure storage]: /azure/storage/storage-scalability-targets
[pojmenování a odkazování na kontejnerů, objektů BLOB a metadat]: http://go.microsoft.com/fwlink/?LinkId=255555

[Velikosti pro účty úložiště v systému Windows v Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Velikosti pro Linux účty úložiště v Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Ceny účtu úložiště systému Windows]: /pricing/details/virtual-machines/windows/
[Ceny Linux účet úložiště]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
