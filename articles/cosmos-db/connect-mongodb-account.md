---
title: "aaaMongoDB připojovací řetězec pro účet Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak tooconnect vaše MongoDB aplikace tooan Azure Cosmos DB účet pomocí připojovacího řetězce MongoDB."
keywords: "mongodb připojovací řetězec"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: e36f7375-9329-403b-afd1-4ab49894f75e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.openlocfilehash: c0b81cb49a10e09e3f02411b91731c7f980ec47d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-mongodb-application-tooazure-cosmos-db"></a>Připojit MongoDB aplikace tooAzure Cosmos DB
Zjistěte, jak tooconnect vaše MongoDB aplikace tooan Azure Cosmos DB účet pomocí připojovacího řetězce MongoDB. Potom můžete databázi Azure Cosmos DB jako hello data ukládat pro vaši aplikaci MongoDB. 

V tomto kurzu poskytuje informace o připojovacím řetězci tooretrieve dva způsoby:

- [Hello rychlý start – metoda](#QuickstartConnection), pro použití s .NET, Node.js, prostředí MongoDB Shell, Java a Python ovladače
- [Hello vlastní připojovací řetězec metoda](#GetCustomConnection), pro použití s další ovladače

## <a name="prerequisites"></a>Požadavky

- Účet Azure. Pokud nemáte účet Azure, vytvořte [bezplatný účet Azure](https://azure.microsoft.com/free/) nyní. 
- Účet služby Azure Cosmos DB. Pokyny najdete v tématu [sestavení webové aplikace MongoDB API pomocí rozhraní .NET a hello portál Azure](create-mongodb-dotnet.md).

## <a id="QuickstartConnection"></a>Získat hello MongoDB připojovací řetězec pomocí hello rychlý start
1. V internetovém prohlížeči, přihlaste toohello [portál Azure](https://portal.azure.com).
2. V hello **Azure Cosmos DB** okně vyberte hello rozhraní API pro účet MongoDB. 
3. V levém podokně hello hello účet okna klikněte na **úvodní**. 
4. Vyberte svou platformu (**.NET**, **Node.js**, **MongoDB prostředí**, **Java**, **Python**). Pokud nevidíte ovladače nebo nástroj v seznamu uvedena, nemusíte si dělat starosti – jsme nepřetržitě dokumentů další fragmenty kódu připojení. Zadejte komentář níže na co chcete toosee. toolearn jak toocraft vlastní připojení, přečtěte si [získat informace o připojovacím řetězci hello účet](#GetCustomConnection).
5. Zkopírujte a vložte fragment kódu hello do vaší aplikace pro MongoDB.

    ![Okna rychlý start](./media/connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a>Získat hello MongoDB připojovací řetězec toocustomize
1. V internetovém prohlížeči, přihlaste toohello [portál Azure](https://portal.azure.com).
2. V hello **Azure Cosmos DB** okně vyberte hello rozhraní API pro účet MongoDB. 
3. V levém podokně hello hello účet okna klikněte na **připojovací řetězec**. 
4. Hello **připojovací řetězec** otevře se okno. Obsahuje všechny hello informace nezbytné tooconnect toohello účtu pomocí ovladač pro MongoDB, včetně preconstructed připojovací řetězec.

    ![Okno řetězec připojení](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Požadavky na řetězec připojení
> [!Important]
> Azure Cosmos DB má vysokými nároky na zabezpečení a standardy. Účty Azure Cosmos DB vyžadují ověřování a zabezpečenou komunikaci prostřednictvím *SSL*. 
>
>

Azure Cosmos DB podporuje hello standardní MongoDB formátu řetězce připojení identifikátor URI, pomocí několika specifické požadavky: účty Azure Cosmos DB vyžadují ověřování a zabezpečenou komunikaci prostřednictvím protokolu SSL. Ano je formátu řetězce připojení hello:

    mongodb://username:password@host:port/[database]?ssl=true

Hello hodnoty tohoto řetězce jsou k dispozici v hello **připojovací řetězec** okno uvedena výše:

* Uživatelské jméno (povinné): název účtu Azure Cosmos DB.
* Heslo (povinné): heslo k účtu Azure Cosmos DB.
* Hostitel (povinné): plně kvalifikovaný název domény hello účet Azure Cosmos DB.
* Port (povinné): 10255.
* Databáze (volitelné): hello databáze, která hello připojení používá. Pokud je k dispozici žádná databáze, je výchozí databázi hello "test".
* SSL = true (povinné)

Představte si třeba hello účet ukazuje hello **připojovací řetězec** okno. Je platný připojovací řetězec:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@anhohmongo.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[použít MongoChef](mongodb-mongochef.md) rozhraním API DB Cosmos Azure pro účet MongoDB.
* Prozkoumejte hello rozhraní API služby Azure Cosmos DB pro MongoDB zobrazením [ukázky](mongodb-samples.md).
