---
title: "aaaCreating projektu Azure cloud service pomocí sady Visual Studio | Microsoft Docs"
description: "Další informace nyní toocreate projektu Azure cloud service pomocí sady Visual Studio"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 3c357016aa423688199a7ab3a670115e33a98fe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a>Vytvoření projektu Azure cloud service pomocí sady Visual Studio
Hello nástroje Azure pro sadu Visual Studio poskytuje šablony projektu, který umožňuje vytvářet cloudové služby Azure. Po vytvoření hello projektu sady Visual Studio umožňuje tooconfigure, ladění a nasazení hello cloudové služby tooAzure.

## <a name="steps-toocreate-an-azure-cloud-service-project-in-visual-studio"></a>Kroky toocreate projektu Azure cloud service v sadě Visual Studio
Tato část vás provede procesem vytvoření projektu Azure cloud service v sadě Visual Studio pomocí jedné nebo více webových rolí.  

1. Spuštění sady Visual Studio jako správce.

1. V hlavní nabídce hello vyberte **soubor** > **nový** > **projektu**.

1. Vyberte **cloudu** z hello Visual C# nebo Visual Basic projektu šablony uzly a vyberte **Azure Cloud Service** hello seznamu šablon.

    ![Nové cloudové služby Azure](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. Určete, která verze rozhraní .NET Framework, pokud chcete toouse toodevelop hello.

1. Zadejte název a umístění projektu a název řešení hello. 

1. Vyberte **OK**.

1. V hello **nová Cloudová služba Microsoft Azure** dialogové okno, vyberte hello role má tooadd a zvolte hello šipku vpravo tlačítko tooadd je tooyour řešení.

    ![Vyberte nové role Azure cloudové služby](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. toorename role, kterou jste přidali, při přechodu na hello role v hello **nová Cloudová služba Microsoft Azure** dialogové okno a v místní nabídce hello, vyberte **přejmenovat**. Můžete také přejmenovat roli v rámci vašeho řešení (v hello **Průzkumníku řešení**) po byla přidána.

    ![Přejmenujte roli služby Azure cloud](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

projekt Visual Studio Azure Hello má přidružení toohello role projekty v řešení hello. projekt Hello také zahrnuje hello *souboru definice služby* a *konfigurační soubor služby*:

- **Soubor definice služby** -definuje hello nastavení běhového prostředí pro aplikaci, včetně toho, jaké role jsou požadovány, koncových bodů a velikost virtuálního počítače. 
- **Konfigurační soubor služby** -nakonfiguruje jak velký počet instancí role jsou spouštěny a hello hodnoty hello nastavení definované pro roli. 

Další informace o těchto souborech najdete v tématu [konfigurovat hello role pro cloudové služby Azure pomocí sady Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

## <a name="next-steps"></a>Další kroky
- [Správa rolí v projektech Azure cloud service pomocí sady Visual Studio](./vs-azure-tools-cloud-service-project-managing-roles.md)
