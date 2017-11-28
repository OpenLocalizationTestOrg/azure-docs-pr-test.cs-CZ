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
# <a name="connect-a-mongodb-application-tooazure-cosmos-db"></a><span data-ttu-id="456fe-104">Připojit MongoDB aplikace tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="456fe-104">Connect a MongoDB application tooAzure Cosmos DB</span></span>
<span data-ttu-id="456fe-105">Zjistěte, jak tooconnect vaše MongoDB aplikace tooan Azure Cosmos DB účet pomocí připojovacího řetězce MongoDB.</span><span class="sxs-lookup"><span data-stu-id="456fe-105">Learn how tooconnect your MongoDB app tooan Azure Cosmos DB account by using a MongoDB connection string.</span></span> <span data-ttu-id="456fe-106">Potom můžete databázi Azure Cosmos DB jako hello data ukládat pro vaši aplikaci MongoDB.</span><span class="sxs-lookup"><span data-stu-id="456fe-106">You can then use an Azure Cosmos DB database as hello data store for your MongoDB app.</span></span> 

<span data-ttu-id="456fe-107">V tomto kurzu poskytuje informace o připojovacím řetězci tooretrieve dva způsoby:</span><span class="sxs-lookup"><span data-stu-id="456fe-107">This tutorial provides two ways tooretrieve connection string information:</span></span>

- <span data-ttu-id="456fe-108">[Hello rychlý start – metoda](#QuickstartConnection), pro použití s .NET, Node.js, prostředí MongoDB Shell, Java a Python ovladače</span><span class="sxs-lookup"><span data-stu-id="456fe-108">[hello quick start method](#QuickstartConnection), for use with .NET, Node.js, MongoDB Shell, Java, and Python drivers</span></span>
- <span data-ttu-id="456fe-109">[Hello vlastní připojovací řetězec metoda](#GetCustomConnection), pro použití s další ovladače</span><span class="sxs-lookup"><span data-stu-id="456fe-109">[hello custom connection string method](#GetCustomConnection), for use with other drivers</span></span>

## <a name="prerequisites"></a><span data-ttu-id="456fe-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="456fe-110">Prerequisites</span></span>

- <span data-ttu-id="456fe-111">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="456fe-111">An Azure account.</span></span> <span data-ttu-id="456fe-112">Pokud nemáte účet Azure, vytvořte [bezplatný účet Azure](https://azure.microsoft.com/free/) nyní.</span><span class="sxs-lookup"><span data-stu-id="456fe-112">If you don't have an Azure account, create a [free Azure account](https://azure.microsoft.com/free/) now.</span></span> 
- <span data-ttu-id="456fe-113">Účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="456fe-113">An Azure Cosmos DB account.</span></span> <span data-ttu-id="456fe-114">Pokyny najdete v tématu [sestavení webové aplikace MongoDB API pomocí rozhraní .NET a hello portál Azure](create-mongodb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="456fe-114">For instructions, see [Build a MongoDB API web app with .NET and hello Azure portal](create-mongodb-dotnet.md).</span></span>

## <span data-ttu-id="456fe-115"><a id="QuickstartConnection"></a>Získat hello MongoDB připojovací řetězec pomocí hello rychlý start</span><span class="sxs-lookup"><span data-stu-id="456fe-115"><a id="QuickstartConnection"></a>Get hello MongoDB connection string by using hello quick start</span></span>
1. <span data-ttu-id="456fe-116">V internetovém prohlížeči, přihlaste toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="456fe-116">In an Internet browser, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="456fe-117">V hello **Azure Cosmos DB** okně vyberte hello rozhraní API pro účet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="456fe-117">In hello **Azure Cosmos DB** blade, select hello API for MongoDB account.</span></span> 
3. <span data-ttu-id="456fe-118">V levém podokně hello hello účet okna klikněte na **úvodní**.</span><span class="sxs-lookup"><span data-stu-id="456fe-118">In hello left pane of hello account blade, click **Quick start**.</span></span> 
4. <span data-ttu-id="456fe-119">Vyberte svou platformu (**.NET**, **Node.js**, **MongoDB prostředí**, **Java**, **Python**).</span><span class="sxs-lookup"><span data-stu-id="456fe-119">Choose your platform (**.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**).</span></span> <span data-ttu-id="456fe-120">Pokud nevidíte ovladače nebo nástroj v seznamu uvedena, nemusíte si dělat starosti – jsme nepřetržitě dokumentů další fragmenty kódu připojení.</span><span class="sxs-lookup"><span data-stu-id="456fe-120">If you don't see your driver or tool listed, don't worry--we continuously document more connection code snippets.</span></span> <span data-ttu-id="456fe-121">Zadejte komentář níže na co chcete toosee.</span><span class="sxs-lookup"><span data-stu-id="456fe-121">Please comment below on what you'd like toosee.</span></span> <span data-ttu-id="456fe-122">toolearn jak toocraft vlastní připojení, přečtěte si [získat informace o připojovacím řetězci hello účet](#GetCustomConnection).</span><span class="sxs-lookup"><span data-stu-id="456fe-122">toolearn how toocraft your own connection, read [Get hello account's connection string information](#GetCustomConnection).</span></span>
5. <span data-ttu-id="456fe-123">Zkopírujte a vložte fragment kódu hello do vaší aplikace pro MongoDB.</span><span class="sxs-lookup"><span data-stu-id="456fe-123">Copy and paste hello code snippet into your MongoDB app.</span></span>

    ![Okna rychlý start](./media/connect-mongodb-account/QuickStartBlade.png)

## <span data-ttu-id="456fe-125"><a id="GetCustomConnection"></a>Získat hello MongoDB připojovací řetězec toocustomize</span><span class="sxs-lookup"><span data-stu-id="456fe-125"><a id="GetCustomConnection"></a> Get hello MongoDB connection string toocustomize</span></span>
1. <span data-ttu-id="456fe-126">V internetovém prohlížeči, přihlaste toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="456fe-126">In an Internet browser, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="456fe-127">V hello **Azure Cosmos DB** okně vyberte hello rozhraní API pro účet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="456fe-127">In hello **Azure Cosmos DB** blade, select hello API for MongoDB account.</span></span> 
3. <span data-ttu-id="456fe-128">V levém podokně hello hello účet okna klikněte na **připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="456fe-128">In hello left pane of hello account blade, click **Connection String**.</span></span> 
4. <span data-ttu-id="456fe-129">Hello **připojovací řetězec** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="456fe-129">hello **Connection String** blade opens.</span></span> <span data-ttu-id="456fe-130">Obsahuje všechny hello informace nezbytné tooconnect toohello účtu pomocí ovladač pro MongoDB, včetně preconstructed připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="456fe-130">It has all hello information necessary tooconnect toohello account by using a driver for MongoDB, including a preconstructed connection string.</span></span>

    ![Okno řetězec připojení](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a><span data-ttu-id="456fe-132">Požadavky na řetězec připojení</span><span class="sxs-lookup"><span data-stu-id="456fe-132">Connection string requirements</span></span>
> [!Important]
> <span data-ttu-id="456fe-133">Azure Cosmos DB má vysokými nároky na zabezpečení a standardy.</span><span class="sxs-lookup"><span data-stu-id="456fe-133">Azure Cosmos DB has strict security requirements and standards.</span></span> <span data-ttu-id="456fe-134">Účty Azure Cosmos DB vyžadují ověřování a zabezpečenou komunikaci prostřednictvím *SSL*.</span><span class="sxs-lookup"><span data-stu-id="456fe-134">Azure Cosmos DB accounts require authentication and secure communication via *SSL*.</span></span> 
>
>

<span data-ttu-id="456fe-135">Azure Cosmos DB podporuje hello standardní MongoDB formátu řetězce připojení identifikátor URI, pomocí několika specifické požadavky: účty Azure Cosmos DB vyžadují ověřování a zabezpečenou komunikaci prostřednictvím protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="456fe-135">Azure Cosmos DB supports hello standard MongoDB connection string URI format, with a couple of specific requirements: Azure Cosmos DB accounts require authentication and secure communication via SSL.</span></span> <span data-ttu-id="456fe-136">Ano je formátu řetězce připojení hello:</span><span class="sxs-lookup"><span data-stu-id="456fe-136">So, hello connection string format is:</span></span>

    mongodb://username:password@host:port/[database]?ssl=true

<span data-ttu-id="456fe-137">Hello hodnoty tohoto řetězce jsou k dispozici v hello **připojovací řetězec** okno uvedena výše:</span><span class="sxs-lookup"><span data-stu-id="456fe-137">hello values of this string are available in hello **Connection String** blade shown earlier:</span></span>

* <span data-ttu-id="456fe-138">Uživatelské jméno (povinné): název účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="456fe-138">Username (required): Azure Cosmos DB account name.</span></span>
* <span data-ttu-id="456fe-139">Heslo (povinné): heslo k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="456fe-139">Password (required): Azure Cosmos DB account password.</span></span>
* <span data-ttu-id="456fe-140">Hostitel (povinné): plně kvalifikovaný název domény hello účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="456fe-140">Host (required): FQDN of hello Azure Cosmos DB account.</span></span>
* <span data-ttu-id="456fe-141">Port (povinné): 10255.</span><span class="sxs-lookup"><span data-stu-id="456fe-141">Port (required): 10255.</span></span>
* <span data-ttu-id="456fe-142">Databáze (volitelné): hello databáze, která hello připojení používá.</span><span class="sxs-lookup"><span data-stu-id="456fe-142">Database (optional): hello database that hello connection uses.</span></span> <span data-ttu-id="456fe-143">Pokud je k dispozici žádná databáze, je výchozí databázi hello "test".</span><span class="sxs-lookup"><span data-stu-id="456fe-143">If no database is provided, hello default database is "test."</span></span>
* <span data-ttu-id="456fe-144">SSL = true (povinné)</span><span class="sxs-lookup"><span data-stu-id="456fe-144">ssl=true (required)</span></span>

<span data-ttu-id="456fe-145">Představte si třeba hello účet ukazuje hello **připojovací řetězec** okno.</span><span class="sxs-lookup"><span data-stu-id="456fe-145">For example, consider hello account shown in hello **Connection String** blade.</span></span> <span data-ttu-id="456fe-146">Je platný připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="456fe-146">A valid connection string is:</span></span>

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@anhohmongo.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a><span data-ttu-id="456fe-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="456fe-147">Next steps</span></span>
* <span data-ttu-id="456fe-148">Zjistěte, jak příliš[použít MongoChef](mongodb-mongochef.md) rozhraním API DB Cosmos Azure pro účet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="456fe-148">Learn how too[use MongoChef](mongodb-mongochef.md) with an Azure Cosmos DB API for MongoDB account.</span></span>
* <span data-ttu-id="456fe-149">Prozkoumejte hello rozhraní API služby Azure Cosmos DB pro MongoDB zobrazením [ukázky](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="456fe-149">Explore hello Azure Cosmos DB API for MongoDB by viewing [samples](mongodb-samples.md).</span></span>
