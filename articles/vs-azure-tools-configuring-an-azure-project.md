---
title: "Konfigurace projektu Azure cloud service pomocí sady Visual Studio | Microsoft Docs"
description: "Informace o konfiguraci projektu Azure cloud service v sadě Visual Studio, v závislosti na požadavcích pro tento projekt."
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
ms.openlocfilehash: 799715093bd1a8c8e77e6cdb7168670dc263dfa5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Konfigurace projektu Azure cloud service pomocí sady Visual Studio
Projekt Azure cloud service můžete nakonfigurovat v závislosti na požadavcích pro tento projekt. Můžete nastavit vlastnosti projektu v těchto kategoriích:

- **Publikování Cloudová služba Azure** – můžete nastavit vlastnost a ujistěte se, že není omylem odstranit stávající cloudovou službu nasadit do Azure.
- **Spustit nebo ladění cloudové služby na místním počítači** – můžete vybrat konfiguraci služby, kterou chcete použít a uvést, zda chcete spustit emulátoru úložiště Azure.
- **Ověřit balíček cloudové služby, když je vytvořeno** -rozhodnete zacházet s všechna upozornění jako chyby, tak, aby bylo možné zajistit, že balíček služby cloud nasadí bez problémů. 

## <a name="steps-to-configure-an-azure-cloud-service-project"></a>Postup konfigurace projektu Azure cloud service
1. Otevřít nebo vytvořit projekt cloudové služby v sadě Visual Studio

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a v místní nabídce vyberte **vlastnosti**.
   
1. Na stránce vlastností projektu, vyberte **vývoj** kartě.

    ![Nabídka Vlastnosti projektu](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. Nastavit **výzva před odstraněním stávajícího nasazení** k **True**. Toto nastavení zásady pomáhají zajistit, že nemáte omylem odstranit stávajícího nasazení v Azure

1. Vyberte požadovanou **konfigurace služby** udávajících konfiguraci služby, kterou chcete použít při spuštění nebo ladění cloudové služby místně. Další informace o tom, jak změnit konfiguraci služby pro role, naleznete v části [postup konfigurace role pro cloudové služby Azure pomocí sady Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).

1. Nastavit **emulátoru úložiště Azure spustit** k **True** při spouštění nebo ladění cloudové služby místně spusťte emulátor úložiště Azure.

1. Nastavit **považovat upozornění jako chyby** k **True** a ujistěte se, pokud nejsou chyby ověření balíček nelze publikovat.

1. Nastavit **použít webový projekt porty** k **True** a ujistěte se, že vaši webovou roli používá stejný port pokaždé, když ji spustí místně v IIS Express.

1. Na panelu nástrojů Visual Studio vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky
- [Konfigurace projektu Azure pomocí více konfigurace služby](vs-azure-tools-multiple-services-project-configurations.md)

