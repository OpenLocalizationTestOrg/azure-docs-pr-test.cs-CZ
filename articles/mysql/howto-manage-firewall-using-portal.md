---
title: "aaaCreate a správě Azure databáze pro pravidla brány firewall MySQL pomocí hello portálu Azure | Microsoft Docs"
description: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí hello portálu Azure"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 30dbdde4299df564fbf6581419e908186fe3b823
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-hello-azure-portal"></a><span data-ttu-id="750db-103">Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="750db-103">Create and manage Azure Database for MySQL firewall rules using hello Azure portal</span></span>
<span data-ttu-id="750db-104">Pravidla brány firewall na úrovni serveru povolit správci tooaccess databázi Azure pro Server databáze MySQL ze zadané IP adresy nebo rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="750db-104">Server-level firewall rules enable administrators tooaccess an Azure Database for MySQL Server from a specified IP address or range of IP addresses.</span></span> 

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="750db-105">Vytvoření pravidla brány firewall na úrovni serveru v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="750db-105">Create a server-level firewall rule in hello Azure portal</span></span>

1. <span data-ttu-id="750db-106">V okně serveru MySQL hello, v části nastavení klikněte na **zabezpečení připojení** tooopen hello zabezpečení připojení okně hello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="750db-106">On hello MySQL server blade, under Settings heading, click **Connection Security** tooopen hello Connection Security blade for hello Azure Database for MySQL.</span></span>

   ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="750db-108">Klikněte na tlačítko **přidat Moje IP** na panelu nástrojů toocreate hello pravidlo s hello IP adresa počítače, kterou posuzuje hello systému Azure.</span><span class="sxs-lookup"><span data-stu-id="750db-108">Click **Add My IP** on hello toolbar toocreate a rule with hello IP address of your computer, as perceived by hello Azure system.</span></span>

   ![Portál Azure – klikněte na tlačítko Přidat Moje IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. <span data-ttu-id="750db-110">Před uložením hello konfigurace ověřte IP adresu.</span><span class="sxs-lookup"><span data-stu-id="750db-110">Verify your IP address before saving hello configuration.</span></span> <span data-ttu-id="750db-111">V některých situacích hello IP adresu zjištěnými pomocí portálu Azure se liší od hello IP adresu použitou při přístupu k hello Internetu a serverech Azure.</span><span class="sxs-lookup"><span data-stu-id="750db-111">In some situations, hello IP address observed by Azure portal differs from hello IP address used when accessing hello internet and Azure servers.</span></span> <span data-ttu-id="750db-112">Proto může být nutné toochange hello počáteční IP a koncová IP adresa toomake hello funkce pravidla podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="750db-112">Therefore, you may need toochange hello Start IP and End IP toomake hello rule function as expected.</span></span>

   <span data-ttu-id="750db-113">Pomocí vyhledávacího webu nebo jiných online nástroj toocheck vlastní IP adresu (například hledání "co je adresa IP").</span><span class="sxs-lookup"><span data-stu-id="750db-113">Use a search engine or other online tool toocheck your own IP address (for example, search "what is my IP address").</span></span>

   ![Bing co je můj IP](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. <span data-ttu-id="750db-115">Přidáte další adresní rozsahy.</span><span class="sxs-lookup"><span data-stu-id="750db-115">Add additional address ranges.</span></span> <span data-ttu-id="750db-116">V hello pravidla pro hello Azure databáze MySQL brány firewall můžete zadat jednu IP adresu nebo rozsah adres.</span><span class="sxs-lookup"><span data-stu-id="750db-116">In hello rules for hello Azure Database for MySQL firewall, you can specify a single IP address, or a range of addresses.</span></span> <span data-ttu-id="750db-117">Pokud chcete, aby toolimit hello pravidlo tooone jednu IP adresu, typ hello stejné počáteční IP a koncová IP adresa v poli hello.</span><span class="sxs-lookup"><span data-stu-id="750db-117">If you want toolimit hello rule tooone single IP address, type hello same address in hello field for Start IP and End IP.</span></span> <span data-ttu-id="750db-118">Otevření brány firewall hello umožňuje správcům a uživatelům tooaccess všechny databáze na hello toowhich MySQL serveru mají platné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="750db-118">Opening hello firewall enables administrators and users tooaccess any database on hello MySQL server toowhich they have valid credentials.</span></span>

   ![<span data-ttu-id="750db-119">Portál Azure – pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="750db-119">Azure portal - firewall rules</span></span> ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. <span data-ttu-id="750db-120">Klikněte na tlačítko **Uložit** na hello nástrojů toosave toto pravidlo brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="750db-120">Click **Save** on hello toolbar toosave this server-level firewall rule.</span></span> <span data-ttu-id="750db-121">Počkejte hello potvrzení, zda text hello pravidla brány firewall toohello aktualizace proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="750db-121">Wait for hello confirmation that hello update toohello firewall rules was successful.</span></span>

   ![Portál Azure – klikněte na tlačítko Uložit](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a><span data-ttu-id="750db-123">Spravovat existující pravidla brány firewall na úrovni serveru prostřednictvím hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="750db-123">Manage existing server-level firewall rules through hello Azure portal</span></span>
<span data-ttu-id="750db-124">Opakujte kroky hello, toomanage pravidla brány firewall hello.</span><span class="sxs-lookup"><span data-stu-id="750db-124">Repeat hello steps toomanage hello firewall rules.</span></span>
* <span data-ttu-id="750db-125">tooadd hello aktuální počítač, klikněte na tlačítko **+ přidat Moje IP**.</span><span class="sxs-lookup"><span data-stu-id="750db-125">tooadd hello current computer, click **+ Add My IP**.</span></span>
* <span data-ttu-id="750db-126">tooadd další IP adresy, zadejte v hello **název pravidla**, **počáteční IP**, a **KONCOVÁ IP adresa**.</span><span class="sxs-lookup"><span data-stu-id="750db-126">tooadd additional IP addresses, type in hello **RULE NAME**, **START IP**, and **END IP**.</span></span>
* <span data-ttu-id="750db-127">toomodify existující pravidlo, klikněte na libovolné hello pole v pravidle hello a upravit.</span><span class="sxs-lookup"><span data-stu-id="750db-127">toomodify an existing rule, click any of hello fields in hello rule and modify.</span></span>
* <span data-ttu-id="750db-128">toodelete existující pravidlo, klikněte na tlačítko se třemi tečkami hello [...] a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="750db-128">toodelete an existing rule, click hello ellipsis […] and click **Delete**.</span></span>
* <span data-ttu-id="750db-129">Klikněte na tlačítko **Uložit** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="750db-129">Click **Save** toosave hello changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="750db-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="750db-130">Next steps</span></span>
- <span data-ttu-id="750db-131">Pomoc při připojování tooan Azure databáze MySQL serveru najdete v tématu [knihovny připojení pro databázi Azure pro databázi MySQL](./concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="750db-131">For help in connecting tooan Azure Database for MySQL server, see [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md)</span></span>
