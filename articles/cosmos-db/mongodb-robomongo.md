---
title: "Použít Robomongo pro Azure Cosmos DB | Microsoft Docs"
description: "Další informace o použití Robomongo se Azure DB Cosmos: rozhraní API pro MongoDB účet"
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
ms.openlocfilehash: 8983594776a1bbe413a6d7cf2cd518f0e327648a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="5ecce-104">Pomocí Azure DB Cosmos Robomongo: rozhraní API pro MongoDB účet</span><span class="sxs-lookup"><span data-stu-id="5ecce-104">Use Robomongo with an Azure Cosmos DB: API for MongoDB account</span></span>
<span data-ttu-id="5ecce-105">Pro připojení k databázi Azure Cosmos: rozhraní API pro MongoDB účtu pomocí Robomongo, musíte:</span><span class="sxs-lookup"><span data-stu-id="5ecce-105">To connect to an Azure Cosmos DB: API for MongoDB account using Robomongo, you must:</span></span>

* <span data-ttu-id="5ecce-106">Stáhněte a nainstalujte [Robomongo](https://robomongo.org/)</span><span class="sxs-lookup"><span data-stu-id="5ecce-106">Download and install [Robomongo](https://robomongo.org/)</span></span>
* <span data-ttu-id="5ecce-107">Mít vaše Azure DB Cosmos: rozhraní API pro MongoDB účet [připojovací řetězec](connect-mongodb-account.md) informace</span><span class="sxs-lookup"><span data-stu-id="5ecce-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="connect-using-robomongo"></a><span data-ttu-id="5ecce-108">Připojte se pomocí Robomongo</span><span class="sxs-lookup"><span data-stu-id="5ecce-108">Connect using Robomongo</span></span>
<span data-ttu-id="5ecce-109">Přidání vaší Azure DB Cosmos: rozhraní API pro MongoDB účet pro připojení MongoDB Robomongo, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="5ecce-109">To add your Azure Cosmos DB: API for MongoDB account to the Robomongo MongoDB Connections, perform the following steps.</span></span>

1. <span data-ttu-id="5ecce-110">Načíst vaše Azure DB Cosmos: rozhraní API pro účet informace o připojení MongoDB pomocí pokynů [zde](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="5ecce-110">Retrieve your Azure Cosmos DB: API for MongoDB account connection information using the instructions [here](connect-mongodb-account.md).</span></span>

    ![Snímek obrazovky okna řetězec připojení](./media/mongodb-robomongo/connectionstringblade.png)
2. <span data-ttu-id="5ecce-112">Spustit *Robomongo.exe*</span><span class="sxs-lookup"><span data-stu-id="5ecce-112">Run *Robomongo.exe*</span></span>

3. <span data-ttu-id="5ecce-113">Klikněte na tlačítko připojení v části **souboru** ke správě připojení.</span><span class="sxs-lookup"><span data-stu-id="5ecce-113">Click the connection button under **File** to manage your connections.</span></span> <span data-ttu-id="5ecce-114">Potom klikněte na **vytvořit** v **MongoDB připojení** okno, které se otevře **nastavení připojení** okno.</span><span class="sxs-lookup"><span data-stu-id="5ecce-114">Then, click **Create** in the **MongoDB Connections** window, which will open up the **Connection Settings** window.</span></span>

4. <span data-ttu-id="5ecce-115">V **nastavení připojení** okno, zvolte název.</span><span class="sxs-lookup"><span data-stu-id="5ecce-115">In the **Connection Settings** window, choose a name.</span></span> <span data-ttu-id="5ecce-116">Najdete **hostitele** a **Port** z vašich informací připojení v kroku 1 a zadejte je do **adresu** a **Port**, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5ecce-116">Then, find the **Host** and **Port** from your connection information in Step 1 and enter them into **Address** and **Port**, respectively.</span></span>

    ![Snímek obrazovky Robomongo spravovat připojení](./media/mongodb-robomongo/manageconnections.png)
5. <span data-ttu-id="5ecce-118">Na **ověřování** , klikněte na **provést ověřování**.</span><span class="sxs-lookup"><span data-stu-id="5ecce-118">On the **Authentication** tab, click **Perform authentication**.</span></span> <span data-ttu-id="5ecce-119">Potom zadejte vaše databáze (výchozí hodnota je *správce*), **uživatelské jméno** a **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5ecce-119">Then, enter your Database (default is *Admin*), **User Name** and **Password**.</span></span>
<span data-ttu-id="5ecce-120">Obě **uživatelské jméno** a **heslo** lze nalézt v informace o připojení v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="5ecce-120">Both **User Name** and **Password** can be found in your connection information in Step 1.</span></span>

    ![Snímek obrazovky s kartou ověřování Robomongo](./media/mongodb-robomongo/authentication.png)
6. <span data-ttu-id="5ecce-122">Na **SSL** zkontrolujte **protokol SSL použití**, změňte **metodu ověřování** k **certifikát podepsaný svým držitelem**.</span><span class="sxs-lookup"><span data-stu-id="5ecce-122">On the **SSL** tab, check **Use SSL protocol**, then change the **Authentication Method** to **Self-signed Certificate**.</span></span>

    ![Snímek obrazovky s kartou SSL Robomongo](./media/mongodb-robomongo/SSL.png)
7. <span data-ttu-id="5ecce-124">Nakonec klikněte na **Test** a ověřte, zda lze pro připojení, potom **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5ecce-124">Finally, click **Test** to verify that you are able to connect, then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5ecce-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ecce-125">Next steps</span></span>
* <span data-ttu-id="5ecce-126">Prozkoumejte Azure Cosmos DB: Rozhraní API pro MongoDB [ukázky](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5ecce-126">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
