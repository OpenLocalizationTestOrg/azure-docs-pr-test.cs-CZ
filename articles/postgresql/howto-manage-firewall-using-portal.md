---
title: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí portálu Azure | Microsoft Docs"
description: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí portálu Azure"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 20ac1392949a6f604e68d984cb50273b61051037
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-the-azure-portal"></a><span data-ttu-id="00c6e-103">Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="00c6e-103">Create and manage Azure Database for PostgreSQL firewall rules using the Azure portal</span></span>
<span data-ttu-id="00c6e-104">Pravidla brány firewall na úrovni serveru umožňují správcům přístup k databázi Azure pro PostgreSQL Server z zadaná IP adresa nebo rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="00c6e-104">Server-level firewall rules enable administrators to access an Azure Database for PostgreSQL Server from a specified IP address or range of IP addresses.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="00c6e-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="00c6e-105">Prerequisites</span></span>
<span data-ttu-id="00c6e-106">Chcete-li krok tímto průvodcem postupy, je třeba:</span><span class="sxs-lookup"><span data-stu-id="00c6e-106">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="00c6e-107">Server [vytvoření databáze Azure pro PostgreSQL](quickstart-create-server-database-portal.md)</span><span class="sxs-lookup"><span data-stu-id="00c6e-107">A server [Create an Azure Database for PostgreSQL](quickstart-create-server-database-portal.md)</span></span>

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a><span data-ttu-id="00c6e-108">Vytvoření pravidla brány firewall na úrovni serveru na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="00c6e-108">Create a server-level firewall rule in the Azure portal</span></span>
1. <span data-ttu-id="00c6e-109">V okně PostgreSQL server v části nastavení klikněte na **zabezpečení připojení** otevřete okno zabezpečení připojení pro databázi Azure pro PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="00c6e-109">On the PostgreSQL server blade, under Settings heading, click **Connection Security** to open the Connection Security blade for the Azure Database for PostgreSQL.</span></span>

  ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="00c6e-111">Klikněte na tlačítko **přidat Moje IP** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="00c6e-111">Click **Add My IP** on the toolbar.</span></span> <span data-ttu-id="00c6e-112">Tím se vytvoří pravidlo automaticky s IP adresou tohoto počítače, kterou posuzuje systému Azure.</span><span class="sxs-lookup"><span data-stu-id="00c6e-112">This will create a rule automatically with the IP address of your computer, as perceived by the Azure system.</span></span>

  ![Portál Azure – klikněte na tlačítko Přidat Moje IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. <span data-ttu-id="00c6e-114">Před uložením konfigurace ověřte IP adresu.</span><span class="sxs-lookup"><span data-stu-id="00c6e-114">Verify your IP address before saving the configuration.</span></span> <span data-ttu-id="00c6e-115">V některých situacích IP adresu zjištěnými pomocí portálu Azure se liší od adresa IP použitá při přístupu k Internetu a servery Azure.</span><span class="sxs-lookup"><span data-stu-id="00c6e-115">In some situations, the IP address observed by Azure portal differs from the IP address used when accessing the internet and Azure servers.</span></span> <span data-ttu-id="00c6e-116">Proto musíte změnit počáteční IP a koncová IP adresa, aby funkce pravidla podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="00c6e-116">Therefore, you may need to change the Start IP and End IP to make the rule function as expected.</span></span>
<span data-ttu-id="00c6e-117">Pomocí vyhledávacího webu nebo jiný nástroj, online zkontrolujte vlastní IP adresu (například Bing vyhledávání se "Jaký je můj IP").</span><span class="sxs-lookup"><span data-stu-id="00c6e-117">Use a search engine or other online tool to check your own IP address (for example, Bing search "what is my IP").</span></span>

  ![Co je můj IP Bing hledání](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. <span data-ttu-id="00c6e-119">Přidáte další adresní rozsahy.</span><span class="sxs-lookup"><span data-stu-id="00c6e-119">Add additional address ranges.</span></span> <span data-ttu-id="00c6e-120">V pravidlech pro databázi Azure pro bránu firewall PostgreSQL můžete zadat jednu IP adresu nebo rozsah adres.</span><span class="sxs-lookup"><span data-stu-id="00c6e-120">In the rules for the Azure Database for PostgreSQL firewall, you can specify a single IP address, or a range of addresses.</span></span> <span data-ttu-id="00c6e-121">Pokud chcete omezit platnost pravidla pro jednu IP adresu, zadejte stejnou adresu v poli pro počáteční IP a koncová IP adresa.</span><span class="sxs-lookup"><span data-stu-id="00c6e-121">If you want to limit the rule to one single IP address, type the same address in the field for Start IP and End IP.</span></span> <span data-ttu-id="00c6e-122">Otevření brány firewall umožňuje správcům a uživatelům k přihlášení k jakékoli databázi na serveru PostgreSQL, k němuž mají platné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="00c6e-122">Opening the firewall enables administrators and users to login to any database on the PostgreSQL server to which they have valid credentials.</span></span>

  ![<span data-ttu-id="00c6e-123">Portál Azure – pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="00c6e-123">Azure portal - firewall rules</span></span> ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. <span data-ttu-id="00c6e-124">Klikněte na tlačítko **Uložit** na panelu nástrojů Uložit toto pravidlo brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="00c6e-124">Click **Save** on the toolbar to save this server-level firewall rule.</span></span> <span data-ttu-id="00c6e-125">Počkejte, než pro potvrzení, že aktualizace, která se pravidla brány firewall bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="00c6e-125">Wait for the confirmation that the update to the firewall rules was successful.</span></span>

  ![Portál Azure – klikněte na tlačítko Uložit](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a><span data-ttu-id="00c6e-127">Správa stávajících pravidel brány firewall na úrovni serveru na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="00c6e-127">Manage existing server-level firewall rules through the Azure portal</span></span>
<span data-ttu-id="00c6e-128">Opakujte kroky ke správě pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="00c6e-128">Repeat the steps to manage the firewall rules.</span></span>
* <span data-ttu-id="00c6e-129">Pokud chcete přidat počítač, kliknutím na tlačítko + **přidat Moje IP**.</span><span class="sxs-lookup"><span data-stu-id="00c6e-129">To add the current computer, click the button to + **Add My IP**.</span></span> <span data-ttu-id="00c6e-130">Kliknutím na **Uložit** uložte změny.</span><span class="sxs-lookup"><span data-stu-id="00c6e-130">Click **Save** to save the changes.</span></span>
* <span data-ttu-id="00c6e-131">Pro přidání dalších IP adres zadejte Název pravidla, Počáteční IP adresu a Koncovou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="00c6e-131">To add additional IP addresses, type in the Rule Name, Start IP Address, and End IP Address.</span></span> <span data-ttu-id="00c6e-132">Kliknutím na **Uložit** uložte změny.</span><span class="sxs-lookup"><span data-stu-id="00c6e-132">Click **Save** to save the changes.</span></span>
* <span data-ttu-id="00c6e-133">Pokud chcete upravit stávající pravidlo, klikněte na libovolné pole pravidla a upravte ho.</span><span class="sxs-lookup"><span data-stu-id="00c6e-133">To modify an existing rule, click any of the fields in the rule and modify.</span></span> <span data-ttu-id="00c6e-134">Kliknutím na **Uložit** uložte změny.</span><span class="sxs-lookup"><span data-stu-id="00c6e-134">Click **Save** to save the changes.</span></span>
* <span data-ttu-id="00c6e-135">Pokud chcete odstranit existující pravidlo, klikněte na tlačítko se třemi tečkami [...] a klikněte na tlačítko odebrat odstranit pravidlo.</span><span class="sxs-lookup"><span data-stu-id="00c6e-135">To delete an existing rule, click the ellipsis […] and click Delete remove the rule.</span></span> <span data-ttu-id="00c6e-136">Kliknutím na **Uložit** uložte změny.</span><span class="sxs-lookup"><span data-stu-id="00c6e-136">Click **Save** to save the changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="00c6e-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="00c6e-137">Next steps</span></span>
- <span data-ttu-id="00c6e-138">Podobně můžete skript pro [vytvořit a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure](howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="00c6e-138">Similarly, you can script to [Create and manage Azure Database for PostgreSQL firewall rules using Azure CLI](howto-manage-firewall-using-cli.md)</span></span>
- <span data-ttu-id="00c6e-139">Pomoc při připojování k databázi Azure pro PostgreSQL server najdete v tématu [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="00c6e-139">For help in connecting to an Azure Database for PostgreSQL server, see [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
