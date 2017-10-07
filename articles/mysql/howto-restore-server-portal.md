---
title: "aaaHow tooRestore serveru ve službě Azure Database pro databázi MySQL | Microsoft Docs"
description: "Tento článek popisuje, jak hello toorestore na serveru v Azure databázi MySQL pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b990d5b37c5d4924de9571192b923e3c81094ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-mysql-using-hello-azure-portal"></a><span data-ttu-id="1ce7a-103">Jak hello tooBackup a obnovení na serveru v Azure databázi MySQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1ce7a-103">How tooBackup and Restore a server in Azure Database for MySQL using hello Azure portal</span></span>

## <a name="backup-happens-automatically"></a><span data-ttu-id="1ce7a-104">Zálohování se automaticky stane</span><span class="sxs-lookup"><span data-stu-id="1ce7a-104">Backup happens Automatically</span></span>
<span data-ttu-id="1ce7a-105">Při použití Azure databáze pro databázi MySQL, služba hello databáze automaticky provede zálohování služby hello každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-105">When using Azure Database for MySQL, hello database service automatically makes a backup of hello service every 5 minutes.</span></span> 

<span data-ttu-id="1ce7a-106">Hello zálohy jsou k dispozici po dobu 7 dnů, při použití úroveň Basic a 35 dní při použití úrovně Standard.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-106">hello backups are available for 7 days when using Basic Tier, and 35 days when using Standard Tier.</span></span> <span data-ttu-id="1ce7a-107">Další informace najdete v tématu [Azure databáze MySQL úrovně služeb](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="1ce7a-107">For more information, see [Azure Database for MySQL service tiers](concepts-service-tiers.md)</span></span>

<span data-ttu-id="1ce7a-108">Pomocí této funkce automatického zálohování můžete obnovit hello server a všechny jeho databáze do nový server tooan dříve v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-108">Using this automatic backup feature you may restore hello server and all its databases into a new server tooan earlier point-in-time.</span></span>

## <a name="restore-in-hello-azure-portal"></a><span data-ttu-id="1ce7a-109">Obnovit v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1ce7a-109">Restore in hello Azure portal</span></span>
<span data-ttu-id="1ce7a-110">Databáze pro databázi MySQL Azure umožňuje toorestore hello server back tooa bod v čase a do tooa novou kopii hello server.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-110">Azure Database for MySQL allows you toorestore hello server back tooa point in time and into tooa new copy of hello server.</span></span> <span data-ttu-id="1ce7a-111">Můžete použít tento nový server toorecover vaše data.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-111">You can use this new server toorecover your data.</span></span> 

<span data-ttu-id="1ce7a-112">Například pokud tabulka byla omylem vyřadit v poledne v současné době můžete obnovit toohello čas těsně před poledne a načíst hello chybějící tabulky a data z této novou kopii hello server.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-112">For example, if a table was accidentally dropped at noon today, you could restore toohello time just before noon and retrieve hello missing table and data from that new copy of hello server.</span></span>

<span data-ttu-id="1ce7a-113">Následující kroky obnovení hello ukázkový server tooa bod v čase Hello:</span><span class="sxs-lookup"><span data-stu-id="1ce7a-113">hello following steps restore hello sample server tooa point in time:</span></span>

1. <span data-ttu-id="1ce7a-114">Přihlaste se k hello [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="1ce7a-114">Sign into hello [Azure portal](https://portal.azure.com/)</span></span>

2. <span data-ttu-id="1ce7a-115">Vyhledejte vaší databázi Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-115">Locate your Azure Database for MySQL server.</span></span> <span data-ttu-id="1ce7a-116">V levém podokně hello vyberte **všechny prostředky**, pak vyberte svůj server ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-116">In hello left pane, select **All resources**, then select your server from hello list.</span></span>

3.  <span data-ttu-id="1ce7a-117">Na hello horní části okna Přehled hello serveru, klikněte na tlačítko **obnovení** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-117">On hello top of hello server overview blade, click **Restore** on hello toolbar.</span></span> <span data-ttu-id="1ce7a-118">Otevře se okno obnovení Hello.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-118">hello Restore blade opens.</span></span>
<span data-ttu-id="1ce7a-119">![Klikněte na tlačítko Obnovit](./media/howto-restore-server-portal/click-restore-button.png)</span><span class="sxs-lookup"><span data-stu-id="1ce7a-119">![click restore button](./media/howto-restore-server-portal/click-restore-button.png)</span></span>

4. <span data-ttu-id="1ce7a-120">Vyplňte formulář obnovení hello hello požadované informace:</span><span class="sxs-lookup"><span data-stu-id="1ce7a-120">Fill out hello Restore form with hello required information:</span></span>

- <span data-ttu-id="1ce7a-121">**Obnovení bodu (UTC)**: pomocí hello výběr data a času výběr, vyberte v okamžiku toorestore k.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-121">**Restore point (UTC)**: Using hello Date picker and time picker, select a point-in-time toorestore to.</span></span> <span data-ttu-id="1ce7a-122">Zadaný čas Hello je ve formátu UTC, proto musíte pravděpodobně tooconvert hello místního času na čas UTC.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-122">hello time specified is in UTC format, so you likely need tooconvert hello local time into UTC.</span></span>
- <span data-ttu-id="1ce7a-123">**Obnovení serveru toonew**: Zadejte název toorestore hello existující server do nového serveru.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-123">**Restore toonew server**: Provide a new server name toorestore hello existing server into.</span></span>
- <span data-ttu-id="1ce7a-124">**Umístění**: volba oblasti hello automaticky naplní s hello zdrojový server oblasti a nedá se změnit.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-124">**Location**: hello region choice automatically populates with hello source server region, and cannot be changed.</span></span>
- <span data-ttu-id="1ce7a-125">**Cenová úroveň**: hello cenová úroveň volba automaticky naplní s hello stejné cenovou úroveň jako zdrojový server hello a nelze je zde změnit.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-125">**Pricing tier**: hello pricing tier choice automatically populates with hello same pricing tier as hello source server, and cannot be changed here.</span></span> 
<span data-ttu-id="1ce7a-126">![Možnosti PITR obnovení](./media/howto-restore-server-portal/pitr-restore.png)</span><span class="sxs-lookup"><span data-stu-id="1ce7a-126">![PITR Restore](./media/howto-restore-server-portal/pitr-restore.png)</span></span>

5. <span data-ttu-id="1ce7a-127">Klikněte na tlačítko **OK** toorestore hello server toorestore tooa bod v čase.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-127">Click **OK** toorestore hello server toorestore tooa point in time.</span></span> 

6. <span data-ttu-id="1ce7a-128">Po dokončení obnovení hello vyhledejte hello nový server, který byl vytvořen hello tooverify databáze byly obnoveny podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="1ce7a-128">After hello restore finishes, locate hello new server that was created tooverify hello databases were restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ce7a-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ce7a-129">Next steps</span></span>
- [<span data-ttu-id="1ce7a-130">Knihovny připojení pro databázi Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="1ce7a-130">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)