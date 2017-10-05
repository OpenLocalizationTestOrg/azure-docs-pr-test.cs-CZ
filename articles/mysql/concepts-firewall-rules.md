---
title: "Databáze Azure pro pravidla brány firewall serveru MySQL | Microsoft Docs"
description: "Popisuje pravidla brány firewall pro vaši databázi Azure pro server databáze MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 7456cef7a5da665ee3d70df64265b8186a79f9b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a><span data-ttu-id="d9cba-103">Databáze Azure pro pravidla brány firewall serveru MySQL</span><span class="sxs-lookup"><span data-stu-id="d9cba-103">Azure Database for MySQL server firewall rules</span></span>
<span data-ttu-id="d9cba-104">Brány firewall zabránit veškerý přístup k databázovému serveru, dokud nezadáte, které počítače mají oprávnění.</span><span class="sxs-lookup"><span data-stu-id="d9cba-104">Firewalls prevent all access to your database server until you specify which computers have permission.</span></span> <span data-ttu-id="d9cba-105">Brána firewall uděluje přístup k serveru na základě původní IP adresy každého požadavku.</span><span class="sxs-lookup"><span data-stu-id="d9cba-105">The firewall grants access to the server based on the originating IP address of each request.</span></span>

<span data-ttu-id="d9cba-106">Bránu firewall nakonfigurujete tak, že vytvoříte pravidla brány firewall určující rozsahy přípustných IP adres.</span><span class="sxs-lookup"><span data-stu-id="d9cba-106">To configure your firewall, you create firewall rules that specify ranges of acceptable IP addresses.</span></span> <span data-ttu-id="d9cba-107">Můžete vytvořit pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cba-107">You can create firewall rules at the server level.</span></span>

<span data-ttu-id="d9cba-108">**Pravidla brány firewall:** tato pravidla povolit klientům přístup k vaší celou databázi Azure pro server databáze MySQL, to znamená, všechny databáze v rámci stejného logického serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cba-108">**Firewall rules:** These rules enable clients to access your entire Azure Database for MySQL server, that is, all the databases within the same logical server.</span></span> <span data-ttu-id="d9cba-109">Pravidla brány firewall na úrovni serveru se dá nakonfigurovat pomocí portálu Azure nebo pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="d9cba-109">Server-level firewall rules can be configured by using the Azure portal or using Azure CLI commands.</span></span> <span data-ttu-id="d9cba-110">Pokud chcete vytvořit pravidla brány firewall na úrovni serveru, musí být vlastník předplatného nebo jeho přispěvatelem předplatného.</span><span class="sxs-lookup"><span data-stu-id="d9cba-110">To create server-level firewall rules, you must be the subscription owner or a subscription contributor.</span></span>

## <a name="firewall-overview"></a><span data-ttu-id="d9cba-111">Přehled brány firewall</span><span class="sxs-lookup"><span data-stu-id="d9cba-111">Firewall overview</span></span>
<span data-ttu-id="d9cba-112">Všechny databáze přístup k vaší databázi Azure pro server databáze MySQL je blokován bránou firewall ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d9cba-112">All database access to your Azure Database for MySQL server is blocked by the firewall by default.</span></span> <span data-ttu-id="d9cba-113">Pokud chcete začít používat server z jiného počítače, je třeba zadat jeden nebo více pravidel brány firewall na úrovni serveru pro povolení přístupu k serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cba-113">To begin using your server from another computer, you need to specify one or more server-level firewall rules to enable access to your server.</span></span> <span data-ttu-id="d9cba-114">Použití pravidel brány firewall můžete určit IP rozsahy adres z Internetu, a povolit.</span><span class="sxs-lookup"><span data-stu-id="d9cba-114">Use the firewall rules to specify which IP address ranges from the Internet to allow.</span></span> <span data-ttu-id="d9cba-115">Přístup k webu portálu Azure, samotné nebude ovlivněné pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="d9cba-115">Access to the Azure portal website itself is not impacted by the firewall rules.</span></span>

<span data-ttu-id="d9cba-116">Pokusy o připojení z Internetu a Azure musí nejdřív projít přes bránu firewall než dosáhnou Azure databáze pro databázi MySQL, jak je znázorněno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="d9cba-116">Connection attempts from the Internet and Azure must first pass through the firewall before they can reach your Azure Database for MySQL database, as shown in the following diagram:</span></span>

![Příklad toku fungování brány firewall](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a><span data-ttu-id="d9cba-118">Připojení z Internetu</span><span class="sxs-lookup"><span data-stu-id="d9cba-118">Connecting from the Internet</span></span>
<span data-ttu-id="d9cba-119">Pravidla brány firewall na úrovni serveru platí pro všechny databáze na databázi Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="d9cba-119">Server-level firewall rules apply to all databases on the Azure Database for MySQL server.</span></span>

<span data-ttu-id="d9cba-120">Pokud je IP adresa požadavku v jednom z rozsahů určených v pravidlech brány firewall na úrovni serveru, připojení je povoleno.</span><span class="sxs-lookup"><span data-stu-id="d9cba-120">If the IP address of the request is within one of the ranges specified in the server-level firewall rules, the connection is granted.</span></span>

<span data-ttu-id="d9cba-121">Pokud IP adresa požadavku není v rámci rozsahů určených v žádné z pravidel brány firewall na úrovni databáze nebo úrovni serveru, požadavek na připojení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="d9cba-121">If the IP address of the request is not within the ranges specified in any of the database-level or server-level firewall rules, the connection request fails.</span></span>

## <a name="programmatically-managing-firewall-rules"></a><span data-ttu-id="d9cba-122">Programová správa pravidel brány firewall</span><span class="sxs-lookup"><span data-stu-id="d9cba-122">Programmatically managing firewall rules</span></span>
<span data-ttu-id="d9cba-123">Kromě portálu Azure lze spravovat pravidla brány firewall programově pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="d9cba-123">In addition to the Azure portal, firewall rules can be managed programmatically using Azure CLI.</span></span> <span data-ttu-id="d9cba-124">Viz také [vytvořit a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure](./howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="d9cba-124">See also [Create and manage Azure Database for MySQL firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)</span></span>

## <a name="troubleshooting-the-database-firewall"></a><span data-ttu-id="d9cba-125">Řešení potíží s branou firewall databáze</span><span class="sxs-lookup"><span data-stu-id="d9cba-125">Troubleshooting the database firewall</span></span>
<span data-ttu-id="d9cba-126">Při přístupu k databázi Microsoft Azure pro službu MySQL server tak, jak očekáváte, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="d9cba-126">Consider the following points when access to the Microsoft Azure Database for MySQL server service does not behave as you expect:</span></span>

* <span data-ttu-id="d9cba-127">**Změny v seznamu povolených ještě nevstoupilo v platnost:** může být co nejvíce pět minut zpoždění pro změny databáze Azure pro konfiguraci brány firewall serveru MySQL vstoupily v platnost.</span><span class="sxs-lookup"><span data-stu-id="d9cba-127">**Changes to the allow list have not taken effect yet:** There may be as much as a five-minute delay for changes to the Azure Database for MySQL Server firewall configuration to take effect.</span></span>

* <span data-ttu-id="d9cba-128">**Přihlášení nemá oprávnění nebo nesprávné heslo byl použit:** Pokud přihlášení nemá oprávnění v databázi Azure pro server databáze MySQL nebo je chybné heslo použít, připojení k databázi Azure MySQL serveru byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="d9cba-128">**The login is not authorized or an incorrect password was used:** If a login does not have permissions on the Azure Database for MySQL server or the password used is incorrect, the connection to the Azure Database for MySQL server is denied.</span></span> <span data-ttu-id="d9cba-129">Vytvoření nastavení brány firewall klientům pouze poskytuje možnost pokusit se o připojení k vašemu serveru – každý klient musí dodat potřebné zabezpečené přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="d9cba-129">Creating a firewall setting only provides clients with an opportunity to attempt connecting to your server; each client must provide the necessary security credentials.</span></span>

* <span data-ttu-id="d9cba-130">**Dynamická IP adresa:** Pokud vaše internetové připojení používá dynamické přidělování IP adres a máte problémy dostat se přes bránu firewall, můžete zkusit jedno z následujících řešení:</span><span class="sxs-lookup"><span data-stu-id="d9cba-130">**Dynamic IP address:** If you have an Internet connection with dynamic IP addressing and you are having trouble getting through the firewall, you could try one of the following solutions:</span></span>

* <span data-ttu-id="d9cba-131">Požádejte poskytovatele služeb Internetu (ISP) pro rozsah IP adres přiřazené v klientských počítačích, které přístup k databázi Azure pro server databáze MySQL a pak přidat rozsah IP adres jako pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="d9cba-131">Ask your Internet Service Provider (ISP) for the IP address range assigned to your client computers that access the Azure Database for MySQL server, and then add the IP address range as a firewall rule.</span></span>

* <span data-ttu-id="d9cba-132">Získejte pro své klientské počítače statické přidělování IP adres a následně přidejte tyto IP adresy jako pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="d9cba-132">Get static IP addressing instead for your client computers, and then add the IP addresses as firewall rules.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9cba-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9cba-133">Next steps</span></span>

<span data-ttu-id="d9cba-134">[Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure](./howto-manage-firewall-using-portal.md)
[vytvořit a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure](./howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="d9cba-134">[Create and manage Azure Database for MySQL firewall rules using the Azure portal](./howto-manage-firewall-using-portal.md)
[Create and manage Azure Database for MySQL firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)</span></span>
