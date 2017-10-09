---
title: aaaUse Robomongo pro Azure Cosmos DB | Microsoft Docs
description: "Zjistěte, jak toouse Robomongo se Azure DB Cosmos: rozhraní API pro MongoDB účet"
keywords: robomongo
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 3018243e904015426dc69a69b26fb53421e1fe4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Pomocí Azure DB Cosmos Robomongo: rozhraní API pro MongoDB účet
tooconnect tooan Cosmos databázi Azure: rozhraní API pro MongoDB účtu pomocí Robomongo, musíte:

* Stáhněte a nainstalujte [Robomongo](https://robomongo.org/)
* Mít vaše Azure DB Cosmos: rozhraní API pro MongoDB účet [připojovací řetězec](connect-mongodb-account.md) informace

## <a name="connect-using-robomongo"></a>Připojte se pomocí Robomongo
tooadd vaše Azure DB Cosmos: rozhraní API pro MongoDB účet toohello Robomongo MongoDB připojení, proveďte následující kroky hello.

1. Načíst vaše Azure DB Cosmos: rozhraní API pro účet informace o připojení MongoDB pomocí pokynů hello [zde](connect-mongodb-account.md).

    ![Snímek obrazovky okna řetězec připojení hello](./media/mongodb-robomongo/connectionstringblade.png)
2. Spustit *Robomongo.exe*

3. Klikněte na tlačítko připojení hello pod **soubor** toomanage připojení. Potom klikněte na **vytvořit** v hello **MongoDB připojení** okno, které se otevře hello **nastavení připojení** okno.

4. V hello **nastavení připojení** okno, zvolte název. Vyhledejte hello **hostitele** a **Port** z vašich informací připojení v kroku 1 a zadejte je do **adresu** a **Port**, v tomto pořadí .

    ![Snímek obrazovky hello Robomongo spravovat připojení](./media/mongodb-robomongo/manageconnections.png)
5. Na hello **ověřování** , klikněte na **provést ověřování**. Potom zadejte vaše databáze (výchozí hodnota je *správce*), **uživatelské jméno** a **heslo**.
Obě **uživatelské jméno** a **heslo** lze nalézt v informace o připojení v kroku 1.

    ![Snímek obrazovky hello Robomongo kartu ověřování](./media/mongodb-robomongo/authentication.png)
6. Na hello **SSL** zkontrolujte **protokol SSL pomocí**, pak změňte hello **metodu ověřování** příliš**certifikát podepsaný svým držitelem**.

    ![Snímek obrazovky hello Robomongo SSL karta](./media/mongodb-robomongo/SSL.png)
7. Nakonec klikněte na **Test** tooverify se pak může tooconnect **Uložit**.

## <a name="next-steps"></a>Další kroky
* Prozkoumejte Azure Cosmos DB: Rozhraní API pro MongoDB [ukázky](mongodb-samples.md).
