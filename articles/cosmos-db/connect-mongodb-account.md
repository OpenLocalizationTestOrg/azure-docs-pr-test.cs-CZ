---
title: "MongoDB připojovací řetězec pro účet Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak připojit aplikace MongoDB k účtu Azure Cosmos DB pomocí připojovacího řetězce MongoDB."
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
ms.openlocfilehash: 5a47001705531d971d3181df9c0aa8f957168845
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a><span data-ttu-id="179ea-104">Připojení aplikace MongoDB pro Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="179ea-104">Connect a MongoDB application to Azure Cosmos DB</span></span>
<span data-ttu-id="179ea-105">Zjistěte, jak připojit aplikace MongoDB k účtu Azure Cosmos DB pomocí připojovacího řetězce MongoDB.</span><span class="sxs-lookup"><span data-stu-id="179ea-105">Learn how to connect your MongoDB app to an Azure Cosmos DB account by using a MongoDB connection string.</span></span> <span data-ttu-id="179ea-106">Potom můžete databázi Azure Cosmos DB jako data store pro vaši aplikaci MongoDB.</span><span class="sxs-lookup"><span data-stu-id="179ea-106">You can then use an Azure Cosmos DB database as the data store for your MongoDB app.</span></span> 

<span data-ttu-id="179ea-107">V tomto kurzu nabízí dva způsoby, jak načíst informace o připojovacím řetězci:</span><span class="sxs-lookup"><span data-stu-id="179ea-107">This tutorial provides two ways to retrieve connection string information:</span></span>

- <span data-ttu-id="179ea-108">[Rychlý start – metoda](#QuickstartConnection), pro použití s .NET, Node.js, prostředí MongoDB Shell, Java a Python ovladače</span><span class="sxs-lookup"><span data-stu-id="179ea-108">[The quick start method](#QuickstartConnection), for use with .NET, Node.js, MongoDB Shell, Java, and Python drivers</span></span>
- <span data-ttu-id="179ea-109">[Metodu vlastní připojovací řetězec](#GetCustomConnection), pro použití s další ovladače</span><span class="sxs-lookup"><span data-stu-id="179ea-109">[The custom connection string method](#GetCustomConnection), for use with other drivers</span></span>

## <a name="prerequisites"></a><span data-ttu-id="179ea-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="179ea-110">Prerequisites</span></span>

- <span data-ttu-id="179ea-111">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="179ea-111">An Azure account.</span></span> <span data-ttu-id="179ea-112">Pokud nemáte účet Azure, vytvořte [bezplatný účet Azure](https://azure.microsoft.com/free/) nyní.</span><span class="sxs-lookup"><span data-stu-id="179ea-112">If you don't have an Azure account, create a [free Azure account](https://azure.microsoft.com/free/) now.</span></span> 
- <span data-ttu-id="179ea-113">Účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="179ea-113">An Azure Cosmos DB account.</span></span> <span data-ttu-id="179ea-114">Pokyny najdete v tématu [sestavení webové aplikace MongoDB API pomocí rozhraní .NET a portálu Azure](create-mongodb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="179ea-114">For instructions, see [Build a MongoDB API web app with .NET and the Azure portal](create-mongodb-dotnet.md).</span></span>

## <span data-ttu-id="179ea-115"><a id="QuickstartConnection"></a>Získat připojovací řetězec MongoDB pomocí rychlý start</span><span class="sxs-lookup"><span data-stu-id="179ea-115"><a id="QuickstartConnection"></a>Get the MongoDB connection string by using the quick start</span></span>
1. <span data-ttu-id="179ea-116">V internetovém prohlížeči, přihlaste se k [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="179ea-116">In an Internet browser, sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="179ea-117">V **Azure Cosmos DB** okně vybrat rozhraní API pro účet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="179ea-117">In the **Azure Cosmos DB** blade, select the API for MongoDB account.</span></span> 
3. <span data-ttu-id="179ea-118">V levém podokně okna účet, klikněte na tlačítko **úvodní**.</span><span class="sxs-lookup"><span data-stu-id="179ea-118">In the left pane of the account blade, click **Quick start**.</span></span> 
4. <span data-ttu-id="179ea-119">Vyberte svou platformu (**.NET**, **Node.js**, **MongoDB prostředí**, **Java**, **Python**).</span><span class="sxs-lookup"><span data-stu-id="179ea-119">Choose your platform (**.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**).</span></span> <span data-ttu-id="179ea-120">Pokud nevidíte ovladače nebo nástroj v seznamu uvedena, nemusíte si dělat starosti – jsme nepřetržitě dokumentů další fragmenty kódu připojení.</span><span class="sxs-lookup"><span data-stu-id="179ea-120">If you don't see your driver or tool listed, don't worry--we continuously document more connection code snippets.</span></span> <span data-ttu-id="179ea-121">Zadejte komentář níže na co se má zobrazit.</span><span class="sxs-lookup"><span data-stu-id="179ea-121">Please comment below on what you'd like to see.</span></span> <span data-ttu-id="179ea-122">Chcete-li zjistit, jak vytvořit vlastní připojení, přečtěte si [získat informace o připojovacím řetězci účtu](#GetCustomConnection).</span><span class="sxs-lookup"><span data-stu-id="179ea-122">To learn how to craft your own connection, read [Get the account's connection string information](#GetCustomConnection).</span></span>
5. <span data-ttu-id="179ea-123">Zkopírujte a vložte fragment kódu do vaší aplikace pro MongoDB.</span><span class="sxs-lookup"><span data-stu-id="179ea-123">Copy and paste the code snippet into your MongoDB app.</span></span>

    ![Okna rychlý start](./media/connect-mongodb-account/QuickStartBlade.png)

## <span data-ttu-id="179ea-125"><a id="GetCustomConnection"></a>Získat MongoDB připojovací řetězec pro přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="179ea-125"><a id="GetCustomConnection"></a> Get the MongoDB connection string to customize</span></span>
1. <span data-ttu-id="179ea-126">V internetovém prohlížeči, přihlaste se k [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="179ea-126">In an Internet browser, sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="179ea-127">V **Azure Cosmos DB** okně vybrat rozhraní API pro účet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="179ea-127">In the **Azure Cosmos DB** blade, select the API for MongoDB account.</span></span> 
3. <span data-ttu-id="179ea-128">V levém podokně okna účet, klikněte na tlačítko **připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="179ea-128">In the left pane of the account blade, click **Connection String**.</span></span> 
4. <span data-ttu-id="179ea-129">**Připojovací řetězec** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="179ea-129">The **Connection String** blade opens.</span></span> <span data-ttu-id="179ea-130">Obsahuje všechny informace potřebné k připojení k účtu pomocí ovladač pro MongoDB, včetně preconstructed připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="179ea-130">It has all the information necessary to connect to the account by using a driver for MongoDB, including a preconstructed connection string.</span></span>

    ![Okno řetězec připojení](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a><span data-ttu-id="179ea-132">Požadavky na řetězec připojení</span><span class="sxs-lookup"><span data-stu-id="179ea-132">Connection string requirements</span></span>
> [!Important]
> <span data-ttu-id="179ea-133">Azure Cosmos DB má vysokými nároky na zabezpečení a standardy.</span><span class="sxs-lookup"><span data-stu-id="179ea-133">Azure Cosmos DB has strict security requirements and standards.</span></span> <span data-ttu-id="179ea-134">Účty Azure Cosmos DB vyžadují ověřování a zabezpečenou komunikaci prostřednictvím *SSL*.</span><span class="sxs-lookup"><span data-stu-id="179ea-134">Azure Cosmos DB accounts require authentication and secure communication via *SSL*.</span></span> 
>
>

<span data-ttu-id="179ea-135">Azure Cosmos DB podporuje standardní MongoDB připojovací řetězec formátu URI, pomocí několika specifické požadavky: účty Azure Cosmos DB vyžadují ověřování a zabezpečenou komunikaci prostřednictvím protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="179ea-135">Azure Cosmos DB supports the standard MongoDB connection string URI format, with a couple of specific requirements: Azure Cosmos DB accounts require authentication and secure communication via SSL.</span></span> <span data-ttu-id="179ea-136">Ano je formátu řetězce připojení:</span><span class="sxs-lookup"><span data-stu-id="179ea-136">So, the connection string format is:</span></span>

    mongodb://username:password@host:port/[database]?ssl=true

<span data-ttu-id="179ea-137">Hodnoty tohoto řetězce jsou k dispozici v **připojovací řetězec** okno uvedena výše:</span><span class="sxs-lookup"><span data-stu-id="179ea-137">The values of this string are available in the **Connection String** blade shown earlier:</span></span>

* <span data-ttu-id="179ea-138">Uživatelské jméno (povinné): název účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="179ea-138">Username (required): Azure Cosmos DB account name.</span></span>
* <span data-ttu-id="179ea-139">Heslo (povinné): heslo k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="179ea-139">Password (required): Azure Cosmos DB account password.</span></span>
* <span data-ttu-id="179ea-140">Hostitel (povinné): plně kvalifikovaný název domény účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="179ea-140">Host (required): FQDN of the Azure Cosmos DB account.</span></span>
* <span data-ttu-id="179ea-141">Port (povinné): 10255.</span><span class="sxs-lookup"><span data-stu-id="179ea-141">Port (required): 10255.</span></span>
* <span data-ttu-id="179ea-142">Databáze (volitelné): databáze, který používá připojení.</span><span class="sxs-lookup"><span data-stu-id="179ea-142">Database (optional): The database that the connection uses.</span></span> <span data-ttu-id="179ea-143">Pokud je k dispozici žádná databáze, výchozí databáze je "test".</span><span class="sxs-lookup"><span data-stu-id="179ea-143">If no database is provided, the default database is "test."</span></span>
* <span data-ttu-id="179ea-144">SSL = true (povinné)</span><span class="sxs-lookup"><span data-stu-id="179ea-144">ssl=true (required)</span></span>

<span data-ttu-id="179ea-145">Představte si třeba účet, který ukazuje **připojovací řetězec** okno.</span><span class="sxs-lookup"><span data-stu-id="179ea-145">For example, consider the account shown in the **Connection String** blade.</span></span> <span data-ttu-id="179ea-146">Je platný připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="179ea-146">A valid connection string is:</span></span>

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@anhohmongo.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a><span data-ttu-id="179ea-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="179ea-147">Next steps</span></span>
* <span data-ttu-id="179ea-148">Zjistěte, jak [použít MongoChef](mongodb-mongochef.md) rozhraním API DB Cosmos Azure pro účet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="179ea-148">Learn how to [use MongoChef](mongodb-mongochef.md) with an Azure Cosmos DB API for MongoDB account.</span></span>
* <span data-ttu-id="179ea-149">Prozkoumejte rozhraní API služby Azure Cosmos DB pro MongoDB zobrazením [ukázky](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="179ea-149">Explore the Azure Cosmos DB API for MongoDB by viewing [samples](mongodb-samples.md).</span></span>
