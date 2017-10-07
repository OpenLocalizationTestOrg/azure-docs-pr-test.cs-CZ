---
title: "aaaConfigure projektu Azure cloud service pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure cloudové služby projektu v sadě Visual Studio, v závislosti na požadavcích pro tento projekt."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: 40eb5eedd6ea23bf03c8707431799be28beae701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Konfigurace projektu Azure cloud service pomocí sady Visual Studio
Projekt Azure cloud service můžete nakonfigurovat v závislosti na požadavcích pro tento projekt. Můžete nastavit vlastnosti pro hello projekt pro hello následujících kategorií:

- **Publikování cloudové služby tooAzure** – můžete nastavit vlastnost toomake jistotu, že není existující služby nasadit tooAzure cloudu odstraněn omylem.
- **Spustit nebo ladění cloudové služby na místním počítači hello** – můžete vybrat toouse konfigurace služby a určení toho, zda emulátor úložiště Azure toostart hello.
- **Ověřit balíček cloudové služby, když je vytvořeno** -rozhodnete tootreat žádné upozornění jako chyby tak, aby můžete zajistit, že balíček hello cloudové služby nasadí bez problémů. 

## <a name="steps-tooconfigure-an-azure-cloud-service-project"></a>Kroky tooconfigure projektu Azure cloud service
1. Otevřít nebo vytvořit projekt cloudové služby v sadě Visual Studio

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a hello místní nabídce vyberte **vlastnosti**.
   
1. Na stránce vlastností projektu hello vyberte hello **vývoj** kartě.

    ![Nabídka Vlastnosti projektu](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. Nastavit **výzva před odstraněním stávajícího nasazení** příliš**True**. Toto nastavení pomáhá tooensure nejsou omylem odstranit stávajícího nasazení v Azure

1. Vyberte hello potřeby **konfigurace služby** tooindicate konfiguraci služby, kterou chcete toouse při spouštění nebo ladění cloudové služby místně. Další informace o tom, v tématu Konfigurace služby pro roli, toomodify [jak tooconfigure hello role Azure cloud service pomocí sady Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).

1. Nastavit **emulátoru úložiště Azure spustit** příliš**True** toostart hello emulátoru úložiště Azure při spouštění nebo ladění cloudové služby místně.

1. Nastavit **považovat upozornění jako chyby** příliš**True** toomake se, pokud nejsou chyby ověření balíček nelze publikovat.

1. Nastavit **použít webový projekt porty** příliš**True** toomake jistotu, že vaši webovou roli používá stejný port se každý hello spuštění ho místně v IIS Express.

1. Hello nástrojů Visual Studio, vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky
- [Konfigurace projektu Azure pomocí více konfigurace služby](vs-azure-tools-multiple-services-project-configurations.md)

