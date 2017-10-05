---
title: "Jak obnovit na Server v Azure databáze pro databázi MySQL | Microsoft Docs"
description: "Tento článek popisuje, jak k obnovení serveru se ve službě Azure Database pro databázi MySQL pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 8c06dce534b628a602127fd02b152c8e04e02cc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-portal"></a><span data-ttu-id="5f626-103">Postup zálohování a obnovení serveru se ve službě Azure Database pro databázi MySQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5f626-103">How To Backup and Restore a server in Azure Database for MySQL using the Azure portal</span></span>

## <a name="backup-happens-automatically"></a><span data-ttu-id="5f626-104">Zálohování se automaticky stane</span><span class="sxs-lookup"><span data-stu-id="5f626-104">Backup happens Automatically</span></span>
<span data-ttu-id="5f626-105">Při použití Azure databáze pro databázi MySQL, služba databáze automaticky provede zálohování služby každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="5f626-105">When using Azure Database for MySQL, the database service automatically makes a backup of the service every 5 minutes.</span></span> 

<span data-ttu-id="5f626-106">Zálohy jsou k dispozici po dobu 7 dnů, při použití úroveň Basic a 35 dní při použití úrovně Standard.</span><span class="sxs-lookup"><span data-stu-id="5f626-106">The backups are available for 7 days when using Basic Tier, and 35 days when using Standard Tier.</span></span> <span data-ttu-id="5f626-107">Další informace najdete v tématu [Azure databáze MySQL úrovně služeb](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="5f626-107">For more information, see [Azure Database for MySQL service tiers](concepts-service-tiers.md)</span></span>

<span data-ttu-id="5f626-108">Pomocí této funkce automatického zálohování můžete obnovit na server a všechny jeho databáze do nového serveru starší v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="5f626-108">Using this automatic backup feature you may restore the server and all its databases into a new server to an earlier point-in-time.</span></span>

## <a name="restore-in-the-azure-portal"></a><span data-ttu-id="5f626-109">Obnovit na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5f626-109">Restore in the Azure portal</span></span>
<span data-ttu-id="5f626-110">Databáze pro databázi MySQL Azure umožňuje obnovit server zpět k určitému bodu v čase a do k o novou kopii tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="5f626-110">Azure Database for MySQL allows you to restore the server back to a point in time and into to a new copy of the server.</span></span> <span data-ttu-id="5f626-111">Pokud chcete obnovit svá data, můžete použít tento nový server.</span><span class="sxs-lookup"><span data-stu-id="5f626-111">You can use this new server to recover your data.</span></span> 

<span data-ttu-id="5f626-112">Například pokud tabulka byla omylem vyřadit v poledne v současné době můžete obnovit na čas těsně před poledne a načíst chybějící tabulku a data z této novou kopii tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="5f626-112">For example, if a table was accidentally dropped at noon today, you could restore to the time just before noon and retrieve the missing table and data from that new copy of the server.</span></span>

<span data-ttu-id="5f626-113">Následující kroky obnovit ukázkový server k určitému bodu v čase:</span><span class="sxs-lookup"><span data-stu-id="5f626-113">The following steps restore the sample server to a point in time:</span></span>

1. <span data-ttu-id="5f626-114">Přihlaste se k [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="5f626-114">Sign into the [Azure portal](https://portal.azure.com/)</span></span>

2. <span data-ttu-id="5f626-115">Vyhledejte vaší databázi Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="5f626-115">Locate your Azure Database for MySQL server.</span></span> <span data-ttu-id="5f626-116">V levém podokně vyberte **všechny prostředky**, pak vyberte svůj server ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="5f626-116">In the left pane, select **All resources**, then select your server from the list.</span></span>

3.  <span data-ttu-id="5f626-117">Nahoře v okně Přehled serveru, klikněte na **obnovení** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="5f626-117">On the top of the server overview blade, click **Restore** on the toolbar.</span></span> <span data-ttu-id="5f626-118">Otevře se okno obnovení.</span><span class="sxs-lookup"><span data-stu-id="5f626-118">The Restore blade opens.</span></span>
<span data-ttu-id="5f626-119">![Klikněte na tlačítko Obnovit](./media/howto-restore-server-portal/click-restore-button.png)</span><span class="sxs-lookup"><span data-stu-id="5f626-119">![click restore button](./media/howto-restore-server-portal/click-restore-button.png)</span></span>

4. <span data-ttu-id="5f626-120">Vyplňte formulář obnovení potřebné informace:</span><span class="sxs-lookup"><span data-stu-id="5f626-120">Fill out the Restore form with the required information:</span></span>

- <span data-ttu-id="5f626-121">**Obnovení bodu (UTC)**: pomocí výběr data a času pro výběr, vyberte v daném okamžiku k obnovení.</span><span class="sxs-lookup"><span data-stu-id="5f626-121">**Restore point (UTC)**: Using the Date picker and time picker, select a point-in-time to restore to.</span></span> <span data-ttu-id="5f626-122">Zadaný čas je ve formátu UTC, takže je pravděpodobně potřeba převést na místní čas UTC.</span><span class="sxs-lookup"><span data-stu-id="5f626-122">The time specified is in UTC format, so you likely need to convert the local time into UTC.</span></span>
- <span data-ttu-id="5f626-123">**Obnovit na nový server**: Zadejte nový název serveru a obnovte existující server do.</span><span class="sxs-lookup"><span data-stu-id="5f626-123">**Restore to new server**: Provide a new server name to restore the existing server into.</span></span>
- <span data-ttu-id="5f626-124">**Umístění**: volba oblasti automaticky naplní s oblasti zdrojového serveru a nedá se změnit.</span><span class="sxs-lookup"><span data-stu-id="5f626-124">**Location**: The region choice automatically populates with the source server region, and cannot be changed.</span></span>
- <span data-ttu-id="5f626-125">**Cenová úroveň**: cenová úroveň volba automaticky naplní s stejné cenovou úroveň, jako má zdrojový server a nelze je zde změnit.</span><span class="sxs-lookup"><span data-stu-id="5f626-125">**Pricing tier**: The pricing tier choice automatically populates with the same pricing tier as the source server, and cannot be changed here.</span></span> 
<span data-ttu-id="5f626-126">![Možnosti PITR obnovení](./media/howto-restore-server-portal/pitr-restore.png)</span><span class="sxs-lookup"><span data-stu-id="5f626-126">![PITR Restore](./media/howto-restore-server-portal/pitr-restore.png)</span></span>

5. <span data-ttu-id="5f626-127">Klikněte na tlačítko **OK** k obnovení serveru pro obnovení do bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="5f626-127">Click **OK** to restore the server to restore to a point in time.</span></span> 

6. <span data-ttu-id="5f626-128">Po dokončení obnovení vyhledejte nový server, který byl vytvořen k ověření, že databáze byly obnoveny podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="5f626-128">After the restore finishes, locate the new server that was created to verify the databases were restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f626-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f626-129">Next steps</span></span>
- [<span data-ttu-id="5f626-130">Knihovny připojení pro databázi Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="5f626-130">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)