---
title: "Jak tooRestore na Server v Azure databáze PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje, jak hello toorestore na serveru v Azure databázi PostgreSQL pomocí portálu Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/20/2017
ms.openlocfilehash: bc7351f384607397806d837afd3e1d7a26575e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-postgresql-using-hello-azure-portal"></a><span data-ttu-id="72dfc-103">Jak hello tooBackup a obnovení na serveru v Azure databázi PostgreSQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="72dfc-103">How tooBackup and Restore a server in Azure Database for PostgreSQL using hello Azure portal</span></span>

## <a name="backup-happens-automatically"></a><span data-ttu-id="72dfc-104">Zálohování se automaticky stane</span><span class="sxs-lookup"><span data-stu-id="72dfc-104">Backup happens Automatically</span></span>
<span data-ttu-id="72dfc-105">Při použití Azure databáze PostgreSQL, služba hello databáze automaticky provede zálohování služby hello každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="72dfc-105">When using Azure Database for PostgreSQL, hello database service automatically makes a backup of hello service every 5 minutes.</span></span> 

<span data-ttu-id="72dfc-106">Hello zálohy jsou k dispozici po dobu 7 dnů, při použití úroveň Basic a 35 dní při použití úrovně Standard.</span><span class="sxs-lookup"><span data-stu-id="72dfc-106">hello backups are available for 7 days when using Basic Tier, and 35 days when using Standard Tier.</span></span> <span data-ttu-id="72dfc-107">Další informace najdete v tématu [databáze Azure pro PostgreSQL úrovně služeb](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="72dfc-107">For more information, see [Azure Database for PostgreSQL service tiers](concepts-service-tiers.md)</span></span>

<span data-ttu-id="72dfc-108">Pomocí této funkce automatického zálohování můžete obnovit hello server a všechny jeho databáze do nový server tooan dříve v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="72dfc-108">Using this automatic backup feature you may restore hello server and all its databases into a new server tooan earlier point-in-time.</span></span>

## <a name="restore-in-hello-azure-portal"></a><span data-ttu-id="72dfc-109">Obnovit v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="72dfc-109">Restore in hello Azure portal</span></span>
<span data-ttu-id="72dfc-110">Azure databázi PostgreSQL umožňuje toorestore hello server back tooa bod v čase a do tooa novou kopii hello server.</span><span class="sxs-lookup"><span data-stu-id="72dfc-110">Azure Database for PostgreSQL allows you toorestore hello server back tooa point in time and into tooa new copy of hello server.</span></span> <span data-ttu-id="72dfc-111">Můžete použít tento nový server toorecover vaše data.</span><span class="sxs-lookup"><span data-stu-id="72dfc-111">You can use this new server toorecover your data.</span></span> 

<span data-ttu-id="72dfc-112">Například pokud tabulka byla omylem vyřadit v poledne v současné době můžete obnovit toohello čas těsně před poledne a načíst hello chybějící tabulky a data z této novou kopii hello server.</span><span class="sxs-lookup"><span data-stu-id="72dfc-112">For example, if a table was accidentally dropped at noon today, you could restore toohello time just before noon and retrieve hello missing table and data from that new copy of hello server.</span></span>

<span data-ttu-id="72dfc-113">Následující kroky obnovení hello ukázkový server tooa bod v čase Hello:</span><span class="sxs-lookup"><span data-stu-id="72dfc-113">hello following steps restore hello sample server tooa point in time:</span></span>
1. <span data-ttu-id="72dfc-114">Přihlaste se k hello [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="72dfc-114">Sign into hello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="72dfc-115">Vyhledejte vaší databázi Azure pro PostgreSQL server.</span><span class="sxs-lookup"><span data-stu-id="72dfc-115">Locate your Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="72dfc-116">V hello portálu Azure, klikněte na **všechny prostředky** z nabídky na levé straně hello a zadejte název hello, jako například **mypgserver 20170401**, toosearch pro existující server.</span><span class="sxs-lookup"><span data-stu-id="72dfc-116">In hello Azure portal, click **All Resources** from hello left-hand menu and type in hello name, such as **mypgserver-20170401**, toosearch for your existing server.</span></span> <span data-ttu-id="72dfc-117">Klikněte na název serveru hello uvedené v výsledek hledání hello.</span><span class="sxs-lookup"><span data-stu-id="72dfc-117">Click hello server name listed in hello search result.</span></span> <span data-ttu-id="72dfc-118">Hello **přehled** stránky pro váš server otevře a poskytuje možnosti pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="72dfc-118">hello **Overview** page for your server opens and provides options for further configuration.</span></span>

   ![Portál Azure – prohledávání toolocate vašeho serveru](media/postgresql-howto-restore-server-portal/1-locate.png)

3. <span data-ttu-id="72dfc-120">Na hello horní části okna Přehled hello serveru, klikněte na tlačítko **obnovení** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="72dfc-120">On hello top of hello server overview blade, click **Restore** on hello toolbar.</span></span> <span data-ttu-id="72dfc-121">Otevře se okno obnovení Hello.</span><span class="sxs-lookup"><span data-stu-id="72dfc-121">hello Restore blade opens.</span></span>

   ![Azure databáze pro obnovení PostgreSQL – přehled – tlačítko](./media/postgresql-howto-restore-server-portal/2_server.png)

4. <span data-ttu-id="72dfc-123">Vyplňte formulář obnovení hello hello požadované informace:</span><span class="sxs-lookup"><span data-stu-id="72dfc-123">Fill out hello Restore form with hello required information:</span></span>

   ![<span data-ttu-id="72dfc-124">Azure databázi PostgreSQL - informace o obnovení</span><span class="sxs-lookup"><span data-stu-id="72dfc-124">Azure Database for PostgreSQL - Restore information</span></span> ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - <span data-ttu-id="72dfc-125">**Bod obnovení**: Vyberte bodu v čase, k níž dojde před hello server byl změněn</span><span class="sxs-lookup"><span data-stu-id="72dfc-125">**Restore point**: Select a point-in-time that occurs before hello server was changed</span></span>
  - <span data-ttu-id="72dfc-126">**Cílový server**: Zadejte nový název serveru, který chcete toorestore k</span><span class="sxs-lookup"><span data-stu-id="72dfc-126">**Target server**: Provide a new server name you want toorestore to</span></span>
  - <span data-ttu-id="72dfc-127">**Umístění**: nelze vybrat hello oblast, ve výchozím nastavení je stejný jako zdrojový server hello</span><span class="sxs-lookup"><span data-stu-id="72dfc-127">**Location**: You cannot select hello region, by default it is same as hello source server</span></span>
  - <span data-ttu-id="72dfc-128">**Cenová úroveň**: tuto hodnotu nelze změnit, při obnovení serveru.</span><span class="sxs-lookup"><span data-stu-id="72dfc-128">**Pricing tier**: You cannot change this value when restoring a server.</span></span> <span data-ttu-id="72dfc-129">Je stejný jako zdrojový server hello.</span><span class="sxs-lookup"><span data-stu-id="72dfc-129">It is same as hello source server.</span></span> 

5. <span data-ttu-id="72dfc-130">Klikněte na tlačítko **OK** toorestore hello server toorestore tooa bod v čase.</span><span class="sxs-lookup"><span data-stu-id="72dfc-130">Click **OK** toorestore hello server toorestore tooa point in time.</span></span> 

6. <span data-ttu-id="72dfc-131">Po dokončení obnovení hello vyhledejte hello nový server, který se vytvoří hello tooverify data byla obnovena podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="72dfc-131">Once hello restore finishes, locate hello new server that is created tooverify hello data was restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72dfc-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72dfc-132">Next steps</span></span>
- [<span data-ttu-id="72dfc-133">Knihovny připojení pro databázi Azure pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="72dfc-133">Connection libraries for Azure Database for PostgreSQL</span></span>](concepts-connection-libraries.md)
