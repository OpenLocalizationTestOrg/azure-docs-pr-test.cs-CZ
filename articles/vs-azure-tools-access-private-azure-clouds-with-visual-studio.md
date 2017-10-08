---
title: "aaaAccessing privátních cloudů Azure pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak tooaccess soukromé cloudové prostředky pomocí sady Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.openlocfilehash: 5cfd6439afdcf98c6f7d7f29ab6c4256ed02533a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Přístup k privátní cloud Azure pomocí sady Visual Studio
Ve výchozím nastavení Visual Studio podporuje koncové body REST veřejného cloudu Azure. V tomto tématu se dozvíte, jak toouse vaší privátní cloud je certifikát tooaccess - a interakci s – hello privátního cloudu ze sady Visual Studio.

## <a name="tooaccess-a-private-azure-cloud-in-visual-studio"></a>tooaccess privátní Azure cloud v sadě Visual Studio
1. V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885) pro privátní cloud hello, stáhněte soubor nastavení publikování, nebo se obraťte na svého správce pro soubor nastavení publikování. Ve veřejné verzi hello Azure, hello odkaz toodownload jde [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (hello stažený soubor by měl mít příponu `.publishsettings`)

1. Otevřete Visual Studio

1. V **Průzkumníka serveru**, klikněte pravým tlačítkem na hello **Azure** uzlu a hello místní nabídce vyberte **spravovat a odběry filtru**.
   
    ![Spravovat odběry příkaz](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. V hello **spravovat předplatná Microsoft Azure** dialogové okno, vyberte hello **certifikáty** a pak vyberte **Import**.
   
    ![Import certifikátů Azure](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. V hello **předplatná Microsoft Azure Import** dialogovém okně, vyberte **Procházet**.

    ![V dialogovém okně předplatná Microsoft Azure Import hello tlačítko Procházet](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. V hello **otevřete** dialogové okno, toohello Procházet adresář, kde je uložený hello – soubor s nastavením publikování, vyberte hello souboru a pak vyberte **otevřete**.

    ![Vyberte soubor s nastavením publikování hello](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. Když vrátil toohello **předplatná Microsoft Azure Import** dialogovém okně, vyberte **Import**.

    ![Importovat soubor s nastavením publikování hello](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    Hello certifikáty jsou importovány ze souboru nastavení publikování hello do sady Visual Studio a nyní můžete pracovat s vaší privátní cloudové prostředky.
   
## <a name="next-steps"></a>Další kroky
- [Publikování tooan Cloudová služba Azure ze sady Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- [Postupy: stahování a Import publikovat nastavení a informace o předplatném](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)
