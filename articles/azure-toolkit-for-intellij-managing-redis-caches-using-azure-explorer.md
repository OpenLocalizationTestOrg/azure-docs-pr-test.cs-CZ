---
title: "Správa mezipaměti Redis Cache pomocí Průzkumníka Azure pro IntelliJ | Microsoft Docs"
description: "Naučte se spravovat své mezipaměti redis systému Azure pomocí Průzkumníka Azure pro IntelliJ."
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
ms.date: 06/14/2017
ms.author: robmcm
ms.openlocfilehash: 9ab8ae17ee2a92b5b16d2210366c00b5b8023fa8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a>Správa mezipaměti Redis Cache pomocí Průzkumníka Azure pro IntelliJ

Průzkumník Azure, který je součástí sady nástrojů Azure pro IntelliJ, poskytuje možnosti pro vývojáře v jazyce Java s řešením pro správu pro snadné použití redis mezipaměti v účtu Azure z uvnitř IntelliJ IDE.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a>Vytvoření mezipaměti Redis pomocí IntelliJ

Následující postup vás provede kroky k vytvoření mezipaměti redis pomocí Průzkumníka Azure.

1. Přihlaste se k účtu Azure, pomocí kroků v [přihlášení v pokyny pro Azure nástrojů pro IntelliJ] článku.

1. V **Průzkumník Azure** nástroj okno, rozbalte **Azure** uzel, klikněte pravým tlačítkem na **mezipaměti Redis cache**a potom klikněte na **vytvořit Redis Cache**.

   ![Vytvořit nabídku Redis Cache][CR01]

1. Když **nová mezipaměť Redis** se zobrazí dialogové okno, určete následující možnosti:

   ![Nová mezipaměť Redis dialogové okno vytvořit][CR02]

   a. **Název DNS**: Určuje subdoméně DNS pro novou mezipaměť redis systému, který se přidá jako předpona k ". redis.cache.windows.net"; například: *wingtiptoys.redis.cache.windows.net*.

   b. **Předplatné**: Určuje předplatné Azure, kterou chcete použít pro nové mezipaměti redis.

   c. **Skupina prostředků**: Určuje skupinu prostředků pro vaše mezipaměť redis, je potřeba vyberte jednu z následujících možností:
      * **Vytvořit nový**: Určuje, že chcete vytvořit novou skupinu prostředků.
      * **Použít existující**: Určuje, že zvolíte ze seznamu skupin prostředků, které jsou přidružené k účtu Azure.

   d. **Umístění**: Určuje umístění, kde se má vytvořit vaše mezipaměť redis; například *západní USA*.

   e. **Cenová úroveň**: Určuje, které cenovou úroveň, které používá vaše mezipaměť redis; toto nastavení určuje počet připojení klientů. (Další informace najdete v tématu [Redis Cache ceny].)

   f. **Port bez SSL**: Určuje, jestli vaše mezipaměť redis umožňuje připojení bez SSL; ve výchozím nastavení, jsou povoleny pouze připojení SSL.

1. Pokud jste zadali všechna nastavení mezipaměti redis, klikněte na tlačítko **OK**.

Po vytvoření mezipaměti redis, zobrazí se v Průzkumníkovi Azure.

   ![Redis Cache v Azure Průzkumníku][CR03]

> [!NOTE]
>
> Další informace o konfiguraci vašeho Azure redis cache nastavení najdete v tématu [postup konfigurace Azure Redis Cache].
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a>Zobrazí vlastnosti pro mezipaměť Redis v IntelliJ

1. V Průzkumníku Azure, klikněte pravým tlačítkem na vaše mezipaměť redis a klikněte na tlačítko **zobrazit vlastnosti**.

   ![Místní nabídku Azure Průzkumníka zobrazíte vlastnosti mezipaměti redis][SP01]

1. Průzkumník Azure zobrazí vlastnosti pro vaše mezipaměť redis.

   ![Vlastnosti mezipaměti Redis][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a>Odstranění mezipaměti Redis pomocí IntelliJ

1. V Průzkumníku Azure, klikněte pravým tlačítkem na vaše mezipaměť redis a klikněte na tlačítko **odstranit**.

   ![Místní nabídku Azure Průzkumníka odstranit mezipaměti redis][DE01]

1. Klikněte na tlačítko **Ano** při zobrazení výzvy k odstranění vaše mezipaměť redis.

   ![Odstranění řádku mezipaměti redis][DE02]

## <a name="next-steps"></a>Další kroky

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

Další informace o mezipaměti redis systému Azure, nastavení konfigurace a cenách naleznete v následujících tématech:

* [Azure Redis Cache]
* [Dokumentaci k mezipaměti redis]
* [Redis Cache ceny]
* [postup konfigurace Azure Redis Cache]

<!-- URL List -->

[Redis Cache ceny]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Dokumentaci k mezipaměti redis]: ./redis-cache/index.md
[postup konfigurace Azure Redis Cache]: ./redis-cache/cache-configure.md
[přihlášení v pokyny pro Azure nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
