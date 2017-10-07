---
title: "aaaManaging mezipaměti Redis Cache pomocí hello Průzkumník Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak toomanage Azure redis ukládá do mezipaměti pomocí hello Průzkumník Azure pro IntelliJ."
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
ms.openlocfilehash: 76ba37a2a35c26d0045e17003181108992eb957d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-redis-caches-using-hello-azure-explorer-for-intellij"></a>Správa mezipaměti Redis Cache pomocí hello Průzkumník Azure pro IntelliJ

Průzkumník Azure, který je součástí hello nástrojů Azure pro IntelliJ, poskytuje možnosti pro vývojáře v jazyce Java s řešením snadno použitelné pro správu redis mezipaměti v účtu Azure z uvnitř hello IntelliJ IDE Hello.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a>Vytvoření mezipaměti Redis pomocí IntelliJ

Hello následující kroky vás provedou kroky toocreate hello mezipaměti redis pomocí hello Průzkumník Azure.

1. Přihlaste se tooyour účet Azure postupem hello hello [přihlášení v pokyny pro hello nástrojů Azure pro IntelliJ] článku.

1. V hello **Azure Explorer** nástroj okno, rozbalte položku hello **Azure** uzel, klikněte pravým tlačítkem na **mezipaměti Redis cache**a potom klikněte na **vytvořit Redis Cache**.

   ![Vytvořit nabídku Redis Cache][CR01]

1. Když hello **nová mezipaměť Redis** se zobrazí dialogové okno, zadejte hello následující možnosti:

   ![Nová mezipaměť Redis dialogové okno vytvořit][CR02]

   a. **Název DNS**: Určuje hello DNS subdomény pro hello novou mezipaměť redis systému, který se přidá jako předpona příliš ". redis.cache.windows .net"; například: *wingtiptoys.redis.cache.windows.net*.

   b. **Předplatné**: Určuje hello předplatné Azure, které chcete použít pro nová mezipaměť redis hello toouse.

   c. **Skupina prostředků**: Určuje hello skupinu prostředků pro vaše mezipaměť redis, je třeba toochoose jeden hello následující možnosti:
      * **Vytvořit nový**: Určuje, že chcete toocreate novou skupinu prostředků.
      * **Použít existující**: Určuje, že zvolíte ze seznamu skupin prostředků, které jsou přidružené k účtu Azure.

   d. **Umístění**: Určuje hello umístění, kde se má vytvořit vaše mezipaměť redis; například *západní USA*.

   e. **Cenová úroveň**: Určuje, které cenovou úroveň, které používá vaše mezipaměť redis; toto nastavení určuje hello počtu připojení klientů. (Další informace najdete v tématu [Redis Cache ceny].)

   f. **Port bez SSL**: Určuje, jestli vaše mezipaměť redis umožňuje připojení bez SSL; ve výchozím nastavení, jsou povoleny pouze připojení SSL.

1. Pokud jste zadali všechna nastavení mezipaměti redis, klikněte na tlačítko **OK**.

Po vytvoření mezipaměti redis, zobrazí se v hello Průzkumník Azure.

   ![Redis Cache v Azure Průzkumníku][CR03]

> [!NOTE]
>
> Další informace o konfiguraci vašeho Azure redis cache nastavení najdete v tématu [jak tooconfigure Azure Redis Cache].
>

## <a name="display-hello-properties-for-your-redis-cache-in-intellij"></a>Zobrazí vlastnosti hello ke svojí mezipaměti Redis v IntelliJ

1. V hello Azure Explorer, klikněte pravým tlačítkem na vaše mezipaměť redis a klikněte na **zobrazit vlastnosti**.

   ![Azure Explorer kontextové nabídky toodisplay vlastnosti mezipaměti redis][SP01]

1. Hello Azure Explorer zobrazí hello vlastnosti mezipaměti redis.

   ![Vlastnosti mezipaměti Redis][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a>Odstranění mezipaměti Redis pomocí IntelliJ

1. V hello Azure Explorer, klikněte pravým tlačítkem na vaše mezipaměť redis a klikněte na **odstranit**.

   ![Azure Explorer kontextové nabídky toodelete mezipaměti redis][DE01]

1. Klikněte na tlačítko **Ano** při zobrazení výzvy toodelete vaše mezipaměť redis.

   ![Odstranění řádku mezipaměti redis][DE02]

## <a name="next-steps"></a>Další kroky

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

Další informace o mezipamětí Azure redis, nastavení konfigurace a cenách najdete v tématu hello následující odkazy:

* [Azure Redis Cache]
* [Dokumentaci k mezipaměti redis]
* [Redis Cache ceny]
* [jak tooconfigure Azure Redis Cache]

<!-- URL List -->

[Redis Cache ceny]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Dokumentaci k mezipaměti redis]: ./redis-cache/index.md
[jak tooconfigure Azure Redis Cache]: ./redis-cache/cache-configure.md
[přihlášení v pokyny pro hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
