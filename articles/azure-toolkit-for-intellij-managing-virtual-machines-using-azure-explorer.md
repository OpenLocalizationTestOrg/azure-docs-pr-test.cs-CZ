---
title: "Spravovat virtuální počítače pomocí Průzkumníku Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak spravovat virtuální počítače Azure pomocí Průzkumníka Azure pro IntelliJ."
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
ms.openlocfilehash: 9197580407b3509fbf9a842e1fee1e6348478c34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-intellij"></a>Spravovat virtuální počítače pomocí Průzkumníku Azure pro IntelliJ

Průzkumník Azure, který je součástí sady nástrojů Azure pro IntelliJ, poskytuje Java vývojářům snadno použitelné řešení pro správu virtuálních počítačů v jejich účtu Azure z uvnitř IntelliJ integrované vývojové prostředí (IDE).

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a>Vytvoření virtuálního počítače v IntelliJ

K vytvoření virtuálního počítače pomocí Průzkumníku Azure, postupujte takto: 

1. Přihlaste se k účtu Azure pomocí kroků v [přihlášení pokyny pro Azure Toolkit IntelliJ] článku.

2. V **Azure Explorer** , rozbalte **Azure** uzel, klikněte pravým tlačítkem na **virtuální počítače**a potom klikněte na **vytvoření virtuálního počítače**. 

   ![Příkaz vytvoření virtuálního počítače][CR01]  
    **Vytvořit nový virtuální počítač** otevře se průvodce.

3. V **zvolte předplatné** oken, vyberte své předplatné a pak klikněte na tlačítko **Další**. 

   ![Zvolte předplatné okna][CR02]

4. V **vyberte bitovou kopii virtuálního počítače** okno, zadejte následující informace:

   * **Umístění**: Určuje, kde bude vytvořen virtuální počítač (například *západní USA*). 

   * **Doporučená image**: Určuje, že zvolíte bitovou kopii z zkrácený seznam běžně používané bitové kopie.

   * **Vlastní image**: Určuje, že zvolíte vlastní image tím, že poskytuje následující informace:

      * **Vydavatel**: Určuje publisher, který vytvořil bitovou kopii, kterou budete používat pro virtuální počítač (například *Microsoft*).

      * **Nabízejí**: Určuje nabídky používat od vydavatele, vybraný virtuální počítač (například *JDK*).

      * **Skladová položka**: Určuje skladová jednotka (SKU) z vybrané nabídky (například *JDK_8*).

      * **Verze #**: Určuje, kterou verzi vybrané SKU používat.

   ![Vyberte okno bitovou kopii virtuálního počítače][CR03]

5. Klikněte na **Další**. 

6. V **základní nastavení virtuálního počítače** okno, zadejte následující informace:

   * **Název virtuálního počítače**: Určuje název pro nový virtuální počítač, který musí začínat písmenem a obsahovat pouze písmena, číslice a pomlčky.

   * **Velikost**: Určuje počet jader a paměti k přidělení pro virtuální počítač.

   * **Uživatelské jméno**: Určuje účet správce, který chcete vytvořit pro virtuální počítač pod správou.

   * **Heslo** a **potvrdit**: Určuje heslo pro účet správce.

   ![Okno základní nastavení virtuálního počítače][CR04]

7. Klikněte na **Další**. 

8. V **související prostředky** okno, zadejte následující informace:

   * **Skupina prostředků**: Určuje skupinu prostředků pro virtuální počítač. Vyberte jednu z následujících možností:
      * **Vytvořit nový**: Určuje, že chcete vytvořit novou skupinu prostředků.
      * **Použít existující**: Určuje, zda chcete vybrat ze seznamu skupin prostředků, které jsou spojeny s vaším účtem Azure.

       ![Okna přidružené prostředky][CR07]

   * **Účet úložiště**: Určuje účet úložiště, který chcete použít pro virtuální počítač uložen. Můžete vybrat existující účet úložiště nebo vytvořit nový účet. Pokud se rozhodnete **vytvořit nový**, zobrazí se dialogové okno následující:

      ![Dialogové okno Vytvořit účet úložiště][CR05]

   * **Virtuální síť** a **podsíť**: Určuje virtuální síť a podsíť, kterému se bude připojovat virtuálního počítače. Můžete použít existující síť a podsíť, nebo můžete vytvořit nové sítě a podsítě. Pokud vyberete **vytvořit nový**, zobrazí se dialogové okno následující:

      ![Dialogové okno vytvořit virtuální síť][CR06]

   * **Veřejná IP adresa**: Určuje externí IP adresu pro virtuální počítač. Můžete vytvořit novou IP adresu nebo, pokud virtuální počítač nebude mít veřejnou IP adresu, můžete si vybrat **(None)**. 

   * **Skupina zabezpečení sítě**: Určuje volitelné síťové brány firewall pro virtuální počítač. Můžete vybrat existující bránu firewall, nebo pokud virtuální počítač nebude používat síťovou bránu firewall, můžete vybrat **(None)**. 

   * **Skupina dostupnosti**: Určuje, že virtuální počítač může patřit do sadu volitelné dostupnosti. Můžete vybrat existující sadu dostupnosti, vytvořit novou skupinu dostupnosti nebo, pokud nebude virtuální počítač patří do skupiny dostupnosti, vyberte **(None)**.

9. Klikněte na **Dokončit**.  
    Nový virtuální počítač se zobrazí v okně nástroje Průzkumník Azure. 

   ![Nový virtuální počítač v zobrazení Průzkumník Azure][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a>Restartování virtuálního počítače v IntelliJ

K restartování virtuálního počítače pomocí Průzkumníku Azure v IntelliJ, postupujte takto:

1. V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na virtuální počítač a pak vyberte **restartujte**.

   ![Příkaz restartovat virtuální počítač][RE01]

2. V okně potvrzení klikněte na **Ano**. 

   ![Okno pro potvrzení restartování virtuálního počítače][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a>Vypnout virtuální počítač v IntelliJ

Vypnout spuštěného virtuálního počítače pomocí Průzkumníku Azure v IntelliJ, postupujte takto:

1. V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na virtuální počítač a pak vyberte **vypnutí**.

   ![Příkaz pro vypnutí virtuálního počítače][SH01]

2. V okně potvrzení klikněte na **Ano**. 

   ![Vypnutí virtuálního počítače potvrzovacím okně][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a>Odstranění virtuálního počítače v IntelliJ

Odstranění virtuálního počítače pomocí Průzkumníku Azure v IntelliJ, postupujte takto:

1. V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na virtuální počítač a pak vyberte **odstranit**.

   ![Příkaz Delete virtuálního počítače][DE01]

2. V okně potvrzení klikněte na **Ano**. 

   ![Okno pro potvrzení odstranění virtuálního počítače][DE02]

## <a name="next-steps"></a>Další kroky
Další informace o Azure velikostí virtuálních počítačů a cenách najdete v následujících zdrojích informací:

* Azure velikostí virtuálních počítačů
  * [Velikosti pro virtuální počítače s Windows v Azure]
  * [Velikosti pro virtuální počítače s Linuxem v Azure]
* Azure virtual Machines – ceny
  * [Ceny virtuálního počítače Windows]
  * [Virtuální počítače Linux – ceny]

Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v následujících zdrojích informací:

* [Azure nástrojů pro Eclipse]
  * [Co je nového v sadě nástrojů Azure pro Eclipse]
  * [Instalace sady Azure Toolkit pro Eclipse]
  * [Pokyny přihlášení k Azure nástrojů pro Eclipse]
  * [Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Co je nového v sadě nástrojů Azure pro IntelliJ]
  * [Instalace sady Azure Toolkit pro IntelliJ]
  * [přihlášení pokyny pro Azure Toolkit IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Pokyny přihlášení k Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[přihlášení pokyny pro Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v sadě nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v sadě nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

[Velikosti pro virtuální počítače s Windows v Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Velikosti pro virtuální počítače s Linuxem v Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Ceny virtuálního počítače Windows]: /pricing/details/virtual-machines/windows/
[Virtuální počítače Linux – ceny]: /pricing/details/virtual-machines/linux/


<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR08.png
