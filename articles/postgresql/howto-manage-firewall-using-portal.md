---
title: "aaaCreate a správě Azure databáze pro pravidla brány firewall PostgreSQL pomocí hello portálu Azure | Microsoft Docs"
description: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí hello portálu Azure"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 6a41a077168657769e442401e9df9931aa809240
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-hello-azure-portal"></a><span data-ttu-id="0f830-103">Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0f830-103">Create and manage Azure Database for PostgreSQL firewall rules using hello Azure portal</span></span>
<span data-ttu-id="0f830-104">Pravidla brány firewall na úrovni serveru povolit správci tooaccess databázi Azure pro PostgreSQL Server ze zadané IP adresy nebo rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="0f830-104">Server-level firewall rules enable administrators tooaccess an Azure Database for PostgreSQL Server from a specified IP address or range of IP addresses.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="0f830-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0f830-105">Prerequisites</span></span>
<span data-ttu-id="0f830-106">toostep prostřednictvím této jak tooguide, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="0f830-106">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="0f830-107">Server [vytvoření databáze Azure pro PostgreSQL](quickstart-create-server-database-portal.md)</span><span class="sxs-lookup"><span data-stu-id="0f830-107">A server [Create an Azure Database for PostgreSQL](quickstart-create-server-database-portal.md)</span></span>

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="0f830-108">Vytvoření pravidla brány firewall na úrovni serveru v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0f830-108">Create a server-level firewall rule in hello Azure portal</span></span>
1. <span data-ttu-id="0f830-109">V okně serveru PostgreSQL hello, v části nastavení klikněte na **zabezpečení připojení** tooopen hello zabezpečení připojení okně hello databáze Azure pro PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="0f830-109">On hello PostgreSQL server blade, under Settings heading, click **Connection Security** tooopen hello Connection Security blade for hello Azure Database for PostgreSQL.</span></span>

  ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="0f830-111">Klikněte na tlačítko **přidat Moje IP** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="0f830-111">Click **Add My IP** on hello toolbar.</span></span> <span data-ttu-id="0f830-112">Pravidlo tím se vytvoří automaticky s hello IP adresa vašeho počítače, který je dosahovaný podle hello systému Azure.</span><span class="sxs-lookup"><span data-stu-id="0f830-112">This will create a rule automatically with hello IP address of your computer, as perceived by hello Azure system.</span></span>

  ![Portál Azure – klikněte na tlačítko Přidat Moje IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. <span data-ttu-id="0f830-114">Před uložením hello konfigurace ověřte IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0f830-114">Verify your IP address before saving hello configuration.</span></span> <span data-ttu-id="0f830-115">V některých situacích hello IP adresu zjištěnými pomocí portálu Azure se liší od hello IP adresu použitou při přístupu k hello Internetu a serverech Azure.</span><span class="sxs-lookup"><span data-stu-id="0f830-115">In some situations, hello IP address observed by Azure portal differs from hello IP address used when accessing hello internet and Azure servers.</span></span> <span data-ttu-id="0f830-116">Proto může být nutné toochange hello počáteční IP a koncová IP adresa toomake hello funkce pravidla podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="0f830-116">Therefore, you may need toochange hello Start IP and End IP toomake hello rule function as expected.</span></span>
<span data-ttu-id="0f830-117">Pomocí vyhledávacího webu nebo jiných online nástroj toocheck vlastní IP adresu (například Bing vyhledávání se "Jaký je můj IP").</span><span class="sxs-lookup"><span data-stu-id="0f830-117">Use a search engine or other online tool toocheck your own IP address (for example, Bing search "what is my IP").</span></span>

  ![Co je můj IP Bing hledání](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. <span data-ttu-id="0f830-119">Přidáte další adresní rozsahy.</span><span class="sxs-lookup"><span data-stu-id="0f830-119">Add additional address ranges.</span></span> <span data-ttu-id="0f830-120">V hello pravidla pro hello Azure databáze PostgreSQL brány firewall můžete zadat jednu IP adresu nebo rozsah adres.</span><span class="sxs-lookup"><span data-stu-id="0f830-120">In hello rules for hello Azure Database for PostgreSQL firewall, you can specify a single IP address, or a range of addresses.</span></span> <span data-ttu-id="0f830-121">Pokud chcete, aby toolimit hello pravidlo tooone jednu IP adresu, typ hello stejné počáteční IP a koncová IP adresa v poli hello.</span><span class="sxs-lookup"><span data-stu-id="0f830-121">If you want toolimit hello rule tooone single IP address, type hello same address in hello field for Start IP and End IP.</span></span> <span data-ttu-id="0f830-122">Otevření brány firewall hello umožňuje správcům a uživatelům databáze tooany toologin na hello toowhich PostgreSQL serveru mají platné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0f830-122">Opening hello firewall enables administrators and users toologin tooany database on hello PostgreSQL server toowhich they have valid credentials.</span></span>

  ![<span data-ttu-id="0f830-123">Portál Azure – pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="0f830-123">Azure portal - firewall rules</span></span> ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. <span data-ttu-id="0f830-124">Klikněte na tlačítko **Uložit** na hello nástrojů toosave toto pravidlo brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="0f830-124">Click **Save** on hello toolbar toosave this server-level firewall rule.</span></span> <span data-ttu-id="0f830-125">Počkejte hello potvrzení, zda text hello pravidla brány firewall toohello aktualizace proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="0f830-125">Wait for hello confirmation that hello update toohello firewall rules was successful.</span></span>

  ![Portál Azure – klikněte na tlačítko Uložit](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a><span data-ttu-id="0f830-127">Spravovat existující pravidla brány firewall na úrovni serveru prostřednictvím hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0f830-127">Manage existing server-level firewall rules through hello Azure portal</span></span>
<span data-ttu-id="0f830-128">Opakujte kroky hello, toomanage pravidla brány firewall hello.</span><span class="sxs-lookup"><span data-stu-id="0f830-128">Repeat hello steps toomanage hello firewall rules.</span></span>
* <span data-ttu-id="0f830-129">tooadd hello aktuální počítač, klikněte na tlačítko hello příliš + **přidat Moje IP**.</span><span class="sxs-lookup"><span data-stu-id="0f830-129">tooadd hello current computer, click hello button too+ **Add My IP**.</span></span> <span data-ttu-id="0f830-130">Klikněte na tlačítko **Uložit** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="0f830-130">Click **Save** toosave hello changes.</span></span>
* <span data-ttu-id="0f830-131">tooadd další IP adresy, zadejte název pravidla a počáteční IP adresa, koncová IP adresa hello.</span><span class="sxs-lookup"><span data-stu-id="0f830-131">tooadd additional IP addresses, type in hello Rule Name, Start IP Address, and End IP Address.</span></span> <span data-ttu-id="0f830-132">Klikněte na tlačítko **Uložit** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="0f830-132">Click **Save** toosave hello changes.</span></span>
* <span data-ttu-id="0f830-133">toomodify existující pravidlo, klikněte na libovolné hello pole v pravidle hello a upravit.</span><span class="sxs-lookup"><span data-stu-id="0f830-133">toomodify an existing rule, click any of hello fields in hello rule and modify.</span></span> <span data-ttu-id="0f830-134">Klikněte na tlačítko **Uložit** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="0f830-134">Click **Save** toosave hello changes.</span></span>
* <span data-ttu-id="0f830-135">toodelete existující pravidlo, klikněte na tlačítko se třemi tečkami hello [...] a klikněte na tlačítko Odstranit odebrat hello pravidlo.</span><span class="sxs-lookup"><span data-stu-id="0f830-135">toodelete an existing rule, click hello ellipsis […] and click Delete remove hello rule.</span></span> <span data-ttu-id="0f830-136">Klikněte na tlačítko **Uložit** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="0f830-136">Click **Save** toosave hello changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f830-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f830-137">Next steps</span></span>
- <span data-ttu-id="0f830-138">Podobně můžete skriptovat příliš[vytvořit a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure](howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="0f830-138">Similarly, you can script too[Create and manage Azure Database for PostgreSQL firewall rules using Azure CLI](howto-manage-firewall-using-cli.md)</span></span>
- <span data-ttu-id="0f830-139">Pomoc při připojování tooan Azure databáze PostgreSQL serveru najdete v tématu [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="0f830-139">For help in connecting tooan Azure Database for PostgreSQL server, see [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
