---
title: "Jak obnovit na Server v Azure databáze pro PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje, jak k obnovení serveru se v databázi Azure pro PostgreSQL pomocí portálu Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/20/2017
ms.openlocfilehash: 3fbdb7741481bd3620466c3489d3609f9ea6961f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a><span data-ttu-id="c1af4-103">Postup zálohování a obnovení serveru v databázi Azure pro PostgreSQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c1af4-103">How To Backup and Restore a server in Azure Database for PostgreSQL using the Azure portal</span></span>

## <a name="backup-happens-automatically"></a><span data-ttu-id="c1af4-104">Zálohování se automaticky stane</span><span class="sxs-lookup"><span data-stu-id="c1af4-104">Backup happens Automatically</span></span>
<span data-ttu-id="c1af4-105">Při použití Azure databáze PostgreSQL, služba databáze automaticky provede zálohování služby každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="c1af4-105">When using Azure Database for PostgreSQL, the database service automatically makes a backup of the service every 5 minutes.</span></span> 

<span data-ttu-id="c1af4-106">Zálohy jsou k dispozici po dobu 7 dnů, při použití úroveň Basic a 35 dní při použití úrovně Standard.</span><span class="sxs-lookup"><span data-stu-id="c1af4-106">The backups are available for 7 days when using Basic Tier, and 35 days when using Standard Tier.</span></span> <span data-ttu-id="c1af4-107">Další informace najdete v tématu [databáze Azure pro PostgreSQL úrovně služeb](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="c1af4-107">For more information, see [Azure Database for PostgreSQL service tiers](concepts-service-tiers.md)</span></span>

<span data-ttu-id="c1af4-108">Pomocí této funkce automatického zálohování můžete obnovit na server a všechny jeho databáze do nového serveru starší v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="c1af4-108">Using this automatic backup feature you may restore the server and all its databases into a new server to an earlier point-in-time.</span></span>

## <a name="restore-in-the-azure-portal"></a><span data-ttu-id="c1af4-109">Obnovit na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c1af4-109">Restore in the Azure portal</span></span>
<span data-ttu-id="c1af4-110">Azure databázi PostgreSQL umožňuje obnovit server zpět k určitému bodu v čase a do k o novou kopii tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="c1af4-110">Azure Database for PostgreSQL allows you to restore the server back to a point in time and into to a new copy of the server.</span></span> <span data-ttu-id="c1af4-111">Pokud chcete obnovit svá data, můžete použít tento nový server.</span><span class="sxs-lookup"><span data-stu-id="c1af4-111">You can use this new server to recover your data.</span></span> 

<span data-ttu-id="c1af4-112">Například pokud tabulka byla omylem vyřadit v poledne v současné době můžete obnovit na čas těsně před poledne a načíst chybějící tabulku a data z této novou kopii tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="c1af4-112">For example, if a table was accidentally dropped at noon today, you could restore to the time just before noon and retrieve the missing table and data from that new copy of the server.</span></span>

<span data-ttu-id="c1af4-113">Následující kroky obnovit ukázkový server k určitému bodu v čase:</span><span class="sxs-lookup"><span data-stu-id="c1af4-113">The following steps restore the sample server to a point in time:</span></span>
1. <span data-ttu-id="c1af4-114">Přihlaste se k [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="c1af4-114">Sign into the [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="c1af4-115">Vyhledejte vaší databázi Azure pro PostgreSQL server.</span><span class="sxs-lookup"><span data-stu-id="c1af4-115">Locate your Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="c1af4-116">Na portálu Azure klikněte na tlačítko **všechny prostředky** z levé nabídce a zadejte název, například **mypgserver 20170401**, k vyhledání existujícího serveru.</span><span class="sxs-lookup"><span data-stu-id="c1af4-116">In the Azure portal, click **All Resources** from the left-hand menu and type in the name, such as **mypgserver-20170401**, to search for your existing server.</span></span> <span data-ttu-id="c1af4-117">Klikněte na název serveru uvedený ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="c1af4-117">Click the server name listed in the search result.</span></span> <span data-ttu-id="c1af4-118">Otevře se stránka **Přehled** vašeho serveru a poskytne vám možnosti další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c1af4-118">The **Overview** page for your server opens and provides options for further configuration.</span></span>

   ![Portál Azure – vyhledávání umístit si server](media/postgresql-howto-restore-server-portal/1-locate.png)

3. <span data-ttu-id="c1af4-120">Nahoře v okně Přehled serveru, klikněte na **obnovení** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="c1af4-120">On the top of the server overview blade, click **Restore** on the toolbar.</span></span> <span data-ttu-id="c1af4-121">Otevře se okno obnovení.</span><span class="sxs-lookup"><span data-stu-id="c1af4-121">The Restore blade opens.</span></span>

   ![Azure databáze pro obnovení PostgreSQL – přehled – tlačítko](./media/postgresql-howto-restore-server-portal/2_server.png)

4. <span data-ttu-id="c1af4-123">Vyplňte formulář obnovení potřebné informace:</span><span class="sxs-lookup"><span data-stu-id="c1af4-123">Fill out the Restore form with the required information:</span></span>

   ![<span data-ttu-id="c1af4-124">Azure databázi PostgreSQL - informace o obnovení</span><span class="sxs-lookup"><span data-stu-id="c1af4-124">Azure Database for PostgreSQL - Restore information</span></span> ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - <span data-ttu-id="c1af4-125">**Bod obnovení**: Vyberte bodu v čase, k níž dojde před server byl změněn</span><span class="sxs-lookup"><span data-stu-id="c1af4-125">**Restore point**: Select a point-in-time that occurs before the server was changed</span></span>
  - <span data-ttu-id="c1af4-126">**Cílový server**: Zadejte nový název serveru, kterou chcete obnovit</span><span class="sxs-lookup"><span data-stu-id="c1af4-126">**Target server**: Provide a new server name you want to restore to</span></span>
  - <span data-ttu-id="c1af4-127">**Umístění**: nelze vyberte oblast, ve výchozím nastavení je stejné jako na zdrojovém serveru</span><span class="sxs-lookup"><span data-stu-id="c1af4-127">**Location**: You cannot select the region, by default it is same as the source server</span></span>
  - <span data-ttu-id="c1af4-128">**Cenová úroveň**: tuto hodnotu nelze změnit, při obnovení serveru.</span><span class="sxs-lookup"><span data-stu-id="c1af4-128">**Pricing tier**: You cannot change this value when restoring a server.</span></span> <span data-ttu-id="c1af4-129">Je stejný jako zdrojový server.</span><span class="sxs-lookup"><span data-stu-id="c1af4-129">It is same as the source server.</span></span> 

5. <span data-ttu-id="c1af4-130">Klikněte na tlačítko **OK** k obnovení serveru pro obnovení do bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="c1af4-130">Click **OK** to restore the server to restore to a point in time.</span></span> 

6. <span data-ttu-id="c1af4-131">Po dokončení obnovení vyhledejte nový server, který je vytvořen k ověření, že data byla obnovena podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="c1af4-131">Once the restore finishes, locate the new server that is created to verify the data was restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1af4-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1af4-132">Next steps</span></span>
- [<span data-ttu-id="c1af4-133">Knihovny připojení pro databázi Azure pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="c1af4-133">Connection libraries for Azure Database for PostgreSQL</span></span>](concepts-connection-libraries.md)
