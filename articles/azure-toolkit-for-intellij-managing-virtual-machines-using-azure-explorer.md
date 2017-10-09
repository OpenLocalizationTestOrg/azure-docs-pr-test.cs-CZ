---
title: "hello aaaManage virtuální počítače pomocí Průzkumníku Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak toomanage virtuální počítače Azure pomocí hello Průzkumníka Azure pro IntelliJ."
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
ms.openlocfilehash: a73dd4f73b311dd3413f6712e3b76c36ee464de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-by-using-hello-azure-explorer-for-intellij"></a>Správa virtuálních počítačů pomocí hello Průzkumník Azure pro IntelliJ

Hello Průzkumník Azure, který je součástí hello nástrojů Azure pro IntelliJ, poskytuje vývojáře v jazyce Java s řešením snadno použitelné pro správu virtuálních počítačů v jejich účtu Azure z uvnitř hello IntelliJ integrované vývojové prostředí (IDE).

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a>Vytvoření virtuálního počítače v IntelliJ

toocreate virtuálního počítače pomocí hello Azure Explorer hello následující: 

1. Přihlaste se pomocí kroků hello v hello tooyour účet Azure [přihlášení pokyny pro hello Azure Toolkit IntelliJ] článku.

2. V hello **Azure Explorer** , rozbalte hello **Azure** uzel, klikněte pravým tlačítkem na **virtuální počítače**a potom klikněte na **vytvoření virtuálního počítače**. 

   ![Hello příkaz vytvoření virtuálního počítače][CR01]  
    Hello **vytvořit nový virtuální počítač** otevře se průvodce.

3. V hello **zvolte předplatné** oken, vyberte své předplatné a pak klikněte na tlačítko **Další**. 

   ![Hello zvolte předplatné okna][CR02]

4. V hello **vyberte bitovou kopii virtuálního počítače** okno, zadejte hello následující informace:

   * **Umístění**: Určuje, kde bude vytvořen virtuální počítač (například *západní USA*). 

   * **Doporučená image**: Určuje, že zvolíte bitovou kopii z zkrácený seznam běžně používané bitové kopie.

   * **Vlastní image**: Určuje, že zvolíte vlastní image tím, že poskytuje hello následující informace:

      * **Vydavatel**: Určuje hello vydavatele, který vytvořili hello obrázek, který budete používat pro virtuální počítač (například *Microsoft*).

      * **Nabízejí**: Určuje nabídky toouse od vydavatele vybrané hello hello virtuálního počítače (například *JDK*).

      * **Skladová položka**: Určuje hello skladové jednotky (SKU) toouse z vybrané nabídky hello (například *JDK_8*).

      * **Verze #**: Určuje, která verze hello vybrané SKU toouse.

   ![Hello vyberte časové období bitovou kopii virtuálního počítače][CR03]

5. Klikněte na **Další**. 

6. V hello **základní nastavení virtuálního počítače** okno, zadejte hello následující informace:

   * **Název virtuálního počítače**: Určuje hello název pro nový virtuální počítač, který musí začínat písmenem a obsahovat pouze písmena, číslice a pomlčky.

   * **Velikost**: Určuje hello počet jader a paměti tooallocate pro virtuální počítač.

   * **Uživatelské jméno**: Určuje hello správce účtu toocreate pro virtuální počítač pod správou.

   * **Heslo** a **potvrdit**: Určuje hello heslo pro účet správce.

   ![okno Hello základní nastavení virtuálního počítače][CR04]

7. Klikněte na **Další**. 

8. V hello **související prostředky** okno, zadejte hello následující informace:

   * **Skupina prostředků**: Určuje hello skupinu prostředků pro virtuální počítač. Vyberte jednu z hello následující možnosti:
      * **Vytvořit nový**: Určuje, že chcete toocreate novou skupinu prostředků.
      * **Použít existující**: Určuje, že chcete tooselect ze seznamu skupin prostředků, které jsou spojeny s vaším účtem Azure.

       ![okna přidružené prostředky Hello][CR07]

   * **Účet úložiště**: Určuje toouse účet hello úložiště pro uložení virtuálního počítače. Můžete vybrat existující účet úložiště nebo vytvořit nový účet. Pokud se rozhodnete **vytvořit nový**, zobrazí se následující dialogové okno hello:

      ![Dialogové okno Vytvořit účet úložiště Hello][CR05]

   * **Virtuální síť** a **podsíť**: Určuje hello virtuální síť a podsíť, kterému se bude připojovat virtuálního počítače. Můžete použít existující síť a podsíť, nebo můžete vytvořit nové sítě a podsítě. Pokud vyberete **vytvořit nový**, zobrazí se následující dialogové okno hello:

      ![Dialogové okno vytvořit virtuální síť Hello][CR06]

   * **Veřejná IP adresa**: Určuje externí IP adresu pro virtuální počítač. Můžete toocreate novou IP adresu nebo, pokud virtuální počítač nebude mít veřejnou IP adresu, můžete si vybrat **(None)**. 

   * **Skupina zabezpečení sítě**: Určuje volitelné síťové brány firewall pro virtuální počítač. Můžete vybrat existující bránu firewall, nebo pokud virtuální počítač nebude používat síťovou bránu firewall, můžete vybrat **(None)**. 

   * **Skupina dostupnosti**: Určuje, že virtuální počítač může patřit do sadu volitelné dostupnosti. Můžete vybrat existující sadu dostupnosti, vytvořte novou skupinu dostupnosti nebo, pokud virtuální počítač nebude patří tooan nastavení dostupnosti, vyberte **(None)**.

9. Klikněte na **Dokončit**.  
    Nový virtuální počítač se zobrazí v okně nástroje Azure Exploreru hello. 

   ![Nový virtuální počítač v hello Průzkumník Azure][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a>Restartování virtuálního počítače v IntelliJ

virtuální počítač pomocí hello Průzkumník Azure v IntelliJ, toorestart hello následující:

1. V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na hello virtuálního počítače a pak vyberte **restartujte**.

   ![příkaz restartovat virtuální počítač Hello][RE01]

2. V okně potvrzení hello klikněte **Ano**. 

   ![Hello restartovat virtuální počítač potvrzovacím okně][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a>Vypnout virtuální počítač v IntelliJ

tooshut dolů spuštěného virtuálního počítače pomocí hello Průzkumník Azure v IntelliJ, hello následující:

1. V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na hello virtuálního počítače a pak vyberte **vypnutí**.

   ![příkaz pro vypnutí virtuálního počítače Hello][SH01]

2. V okně potvrzení hello klikněte **Ano**. 

   ![Vypněte virtuální počítač potvrzovacím okně Hello][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a>Odstranění virtuálního počítače v IntelliJ

virtuální počítač pomocí hello Průzkumník Azure v IntelliJ, toodelete hello následující:

1. V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na hello virtuálního počítače a pak vyberte **odstranit**.

   ![příkaz Delete Hello virtuálního počítače][DE01]

2. V okně potvrzení hello klikněte **Ano**. 

   ![Hello odstranit okno pro potvrzení virtuálního počítače][DE02]

## <a name="next-steps"></a>Další kroky
Další informace o Azure velikosti virtuálního počítače a cenách najdete v tématu hello následující prostředky:

* Azure velikostí virtuálních počítačů
  * [Velikosti pro virtuální počítače s Windows v Azure]
  * [Velikosti pro virtuální počítače s Linuxem v Azure]
* Azure virtual Machines – ceny
  * [Ceny virtuálního počítače Windows]
  * [Virtuální počítače Linux – ceny]

Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující prostředky:

* [Azure nástrojů pro Eclipse]
  * [Co je nového v hello nástrojů Azure pro Eclipse]
  * [Instalace hello nástrojů Azure pro Eclipse]
  * [Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]
  * [Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Co je nového v hello nástrojů Azure pro IntelliJ]
  * [Instalace hello Azure Toolkit pro IntelliJ]
  * [přihlášení pokyny pro hello Azure Toolkit IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[přihlášení pokyny pro hello Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

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
