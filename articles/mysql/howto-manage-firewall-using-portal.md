---
title: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure | Microsoft Docs"
description: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 33198e5a6e11df2db3a17fc96a0b3cd4b1a284e8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-the-azure-portal"></a><span data-ttu-id="f270f-103">Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f270f-103">Create and manage Azure Database for MySQL firewall rules using the Azure portal</span></span>
<span data-ttu-id="f270f-104">Pravidla brány firewall na úrovni serveru umožňují správcům přístup k databázi Azure pro Server databáze MySQL z zadaná IP adresa nebo rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="f270f-104">Server-level firewall rules enable administrators to access an Azure Database for MySQL Server from a specified IP address or range of IP addresses.</span></span> 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a><span data-ttu-id="f270f-105">Vytvoření pravidla brány firewall na úrovni serveru na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f270f-105">Create a server-level firewall rule in the Azure portal</span></span>

1. <span data-ttu-id="f270f-106">V okně serveru MySQL, v části nastavení klikněte na **zabezpečení připojení** otevřete okno zabezpečení připojení pro databázi Azure pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="f270f-106">On the MySQL server blade, under Settings heading, click **Connection Security** to open the Connection Security blade for the Azure Database for MySQL.</span></span>

   ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="f270f-108">Klikněte na tlačítko **přidat Moje IP** na panelu nástrojů k vytvoření pravidla s IP adresou tohoto počítače, kterou posuzuje systému Azure.</span><span class="sxs-lookup"><span data-stu-id="f270f-108">Click **Add My IP** on the toolbar to create a rule with the IP address of your computer, as perceived by the Azure system.</span></span>

   ![Portál Azure – klikněte na tlačítko Přidat Moje IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. <span data-ttu-id="f270f-110">Před uložením konfigurace ověřte IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f270f-110">Verify your IP address before saving the configuration.</span></span> <span data-ttu-id="f270f-111">V některých situacích IP adresu zjištěnými pomocí portálu Azure se liší od adresa IP použitá při přístupu k Internetu a servery Azure.</span><span class="sxs-lookup"><span data-stu-id="f270f-111">In some situations, the IP address observed by Azure portal differs from the IP address used when accessing the internet and Azure servers.</span></span> <span data-ttu-id="f270f-112">Proto musíte změnit počáteční IP a koncová IP adresa, aby funkce pravidla podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="f270f-112">Therefore, you may need to change the Start IP and End IP to make the rule function as expected.</span></span>

   <span data-ttu-id="f270f-113">Pomocí vyhledávacího webu nebo jiný nástroj, online zkontrolujte vlastní IP adresu (například hledání "co je adresa IP").</span><span class="sxs-lookup"><span data-stu-id="f270f-113">Use a search engine or other online tool to check your own IP address (for example, search "what is my IP address").</span></span>

   ![Bing co je můj IP](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. <span data-ttu-id="f270f-115">Přidáte další adresní rozsahy.</span><span class="sxs-lookup"><span data-stu-id="f270f-115">Add additional address ranges.</span></span> <span data-ttu-id="f270f-116">V pravidlech pro databázi Azure pro bránu firewall MySQL můžete zadat jednu IP adresu nebo rozsah adres.</span><span class="sxs-lookup"><span data-stu-id="f270f-116">In the rules for the Azure Database for MySQL firewall, you can specify a single IP address, or a range of addresses.</span></span> <span data-ttu-id="f270f-117">Pokud chcete omezit platnost pravidla pro jednu IP adresu, zadejte stejnou adresu v poli pro počáteční IP a koncová IP adresa.</span><span class="sxs-lookup"><span data-stu-id="f270f-117">If you want to limit the rule to one single IP address, type the same address in the field for Start IP and End IP.</span></span> <span data-ttu-id="f270f-118">Otevření brány firewall umožňuje správcům a uživatelům umožňuje přístup k jakékoli databázi na serveru databáze MySQL, ke kterým mají platné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f270f-118">Opening the firewall enables administrators and users to access any database on the MySQL server to which they have valid credentials.</span></span>

   ![<span data-ttu-id="f270f-119">Portál Azure – pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="f270f-119">Azure portal - firewall rules</span></span> ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. <span data-ttu-id="f270f-120">Klikněte na tlačítko **Uložit** na panelu nástrojů Uložit toto pravidlo brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="f270f-120">Click **Save** on the toolbar to save this server-level firewall rule.</span></span> <span data-ttu-id="f270f-121">Počkejte, než pro potvrzení, že aktualizace, která se pravidla brány firewall bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="f270f-121">Wait for the confirmation that the update to the firewall rules was successful.</span></span>

   ![Portál Azure – klikněte na tlačítko Uložit](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a><span data-ttu-id="f270f-123">Správa stávajících pravidel brány firewall na úrovni serveru na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f270f-123">Manage existing server-level firewall rules through the Azure portal</span></span>
<span data-ttu-id="f270f-124">Opakujte kroky ke správě pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="f270f-124">Repeat the steps to manage the firewall rules.</span></span>
* <span data-ttu-id="f270f-125">Chcete-li přidat počítač, klikněte na tlačítko **+ přidat Moje IP**.</span><span class="sxs-lookup"><span data-stu-id="f270f-125">To add the current computer, click **+ Add My IP**.</span></span>
* <span data-ttu-id="f270f-126">Pokud chcete přidat další IP adresy, zadejte **název pravidla**, **počáteční IP**, a **KONCOVÁ IP adresa**.</span><span class="sxs-lookup"><span data-stu-id="f270f-126">To add additional IP addresses, type in the **RULE NAME**, **START IP**, and **END IP**.</span></span>
* <span data-ttu-id="f270f-127">Pokud chcete upravit stávající pravidlo, klikněte na libovolné pole pravidla a upravte ho.</span><span class="sxs-lookup"><span data-stu-id="f270f-127">To modify an existing rule, click any of the fields in the rule and modify.</span></span>
* <span data-ttu-id="f270f-128">Pokud chcete odstranit existující pravidlo, klikněte na tlačítko se třemi tečkami [...] a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="f270f-128">To delete an existing rule, click the ellipsis […] and click **Delete**.</span></span>
* <span data-ttu-id="f270f-129">Kliknutím na **Uložit** uložte změny.</span><span class="sxs-lookup"><span data-stu-id="f270f-129">Click **Save** to save the changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f270f-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f270f-130">Next steps</span></span>
- <span data-ttu-id="f270f-131">Pomoc při připojování k databázi Azure pro server databáze MySQL, najdete v tématu [knihovny připojení pro databázi Azure pro databázi MySQL](./concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="f270f-131">For help in connecting to an Azure Database for MySQL server, see [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md)</span></span>
