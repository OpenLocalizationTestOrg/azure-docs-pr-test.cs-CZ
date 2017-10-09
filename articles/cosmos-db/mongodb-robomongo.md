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
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="dc6a3-104">Pomocí Azure DB Cosmos Robomongo: rozhraní API pro MongoDB účet</span><span class="sxs-lookup"><span data-stu-id="dc6a3-104">Use Robomongo with an Azure Cosmos DB: API for MongoDB account</span></span>
<span data-ttu-id="dc6a3-105">tooconnect tooan Cosmos databázi Azure: rozhraní API pro MongoDB účtu pomocí Robomongo, musíte:</span><span class="sxs-lookup"><span data-stu-id="dc6a3-105">tooconnect tooan Azure Cosmos DB: API for MongoDB account using Robomongo, you must:</span></span>

* <span data-ttu-id="dc6a3-106">Stáhněte a nainstalujte [Robomongo](https://robomongo.org/)</span><span class="sxs-lookup"><span data-stu-id="dc6a3-106">Download and install [Robomongo](https://robomongo.org/)</span></span>
* <span data-ttu-id="dc6a3-107">Mít vaše Azure DB Cosmos: rozhraní API pro MongoDB účet [připojovací řetězec](connect-mongodb-account.md) informace</span><span class="sxs-lookup"><span data-stu-id="dc6a3-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="connect-using-robomongo"></a><span data-ttu-id="dc6a3-108">Připojte se pomocí Robomongo</span><span class="sxs-lookup"><span data-stu-id="dc6a3-108">Connect using Robomongo</span></span>
<span data-ttu-id="dc6a3-109">tooadd vaše Azure DB Cosmos: rozhraní API pro MongoDB účet toohello Robomongo MongoDB připojení, proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="dc6a3-109">tooadd your Azure Cosmos DB: API for MongoDB account toohello Robomongo MongoDB Connections, perform hello following steps.</span></span>

1. <span data-ttu-id="dc6a3-110">Načíst vaše Azure DB Cosmos: rozhraní API pro účet informace o připojení MongoDB pomocí pokynů hello [zde](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="dc6a3-110">Retrieve your Azure Cosmos DB: API for MongoDB account connection information using hello instructions [here](connect-mongodb-account.md).</span></span>

    ![Snímek obrazovky okna řetězec připojení hello](./media/mongodb-robomongo/connectionstringblade.png)
2. <span data-ttu-id="dc6a3-112">Spustit *Robomongo.exe*</span><span class="sxs-lookup"><span data-stu-id="dc6a3-112">Run *Robomongo.exe*</span></span>

3. <span data-ttu-id="dc6a3-113">Klikněte na tlačítko připojení hello pod **soubor** toomanage připojení.</span><span class="sxs-lookup"><span data-stu-id="dc6a3-113">Click hello connection button under **File** toomanage your connections.</span></span> <span data-ttu-id="dc6a3-114">Potom klikněte na **vytvořit** v hello **MongoDB připojení** okno, které se otevře hello **nastavení připojení** okno.</span><span class="sxs-lookup"><span data-stu-id="dc6a3-114">Then, click **Create** in hello **MongoDB Connections** window, which will open up hello **Connection Settings** window.</span></span>

4. <span data-ttu-id="dc6a3-115">V hello **nastavení připojení** okno, zvolte název.</span><span class="sxs-lookup"><span data-stu-id="dc6a3-115">In hello **Connection Settings** window, choose a name.</span></span> <span data-ttu-id="dc6a3-116">Vyhledejte hello **hostitele** a **Port** z vašich informací připojení v kroku 1 a zadejte je do **adresu** a **Port**, v tomto pořadí .</span><span class="sxs-lookup"><span data-stu-id="dc6a3-116">Then, find hello **Host** and **Port** from your connection information in Step 1 and enter them into **Address** and **Port**, respectively.</span></span>

    ![Snímek obrazovky hello Robomongo spravovat připojení](./media/mongodb-robomongo/manageconnections.png)
5. <span data-ttu-id="dc6a3-118">Na hello **ověřování** , klikněte na **provést ověřování**.</span><span class="sxs-lookup"><span data-stu-id="dc6a3-118">On hello **Authentication** tab, click **Perform authentication**.</span></span> <span data-ttu-id="dc6a3-119">Potom zadejte vaše databáze (výchozí hodnota je *správce*), **uživatelské jméno** a **heslo**.</span><span class="sxs-lookup"><span data-stu-id="dc6a3-119">Then, enter your Database (default is *Admin*), **User Name** and **Password**.</span></span>
<span data-ttu-id="dc6a3-120">Obě **uživatelské jméno** a **heslo** lze nalézt v informace o připojení v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="dc6a3-120">Both **User Name** and **Password** can be found in your connection information in Step 1.</span></span>

    ![Snímek obrazovky hello Robomongo kartu ověřování](./media/mongodb-robomongo/authentication.png)
6. <span data-ttu-id="dc6a3-122">Na hello **SSL** zkontrolujte **protokol SSL pomocí**, pak změňte hello **metodu ověřování** příliš**certifikát podepsaný svým držitelem**.</span><span class="sxs-lookup"><span data-stu-id="dc6a3-122">On hello **SSL** tab, check **Use SSL protocol**, then change hello **Authentication Method** too**Self-signed Certificate**.</span></span>

    ![Snímek obrazovky hello Robomongo SSL karta](./media/mongodb-robomongo/SSL.png)
7. <span data-ttu-id="dc6a3-124">Nakonec klikněte na **Test** tooverify se pak může tooconnect **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="dc6a3-124">Finally, click **Test** tooverify that you are able tooconnect, then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc6a3-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dc6a3-125">Next steps</span></span>
* <span data-ttu-id="dc6a3-126">Prozkoumejte Azure Cosmos DB: Rozhraní API pro MongoDB [ukázky](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="dc6a3-126">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
