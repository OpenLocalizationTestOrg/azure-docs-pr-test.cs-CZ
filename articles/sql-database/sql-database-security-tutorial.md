---
title: "Zabezpečení databáze Azure SQL | Microsoft Docs"
description: "Další informace o techniky a funkce zabezpečení databáze Azure SQL."
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/28/2017
ms.author: daredis
ms.openlocfilehash: 4bc09ad13ed0c9dc9257e9c75ec6f9ff3d689a0b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="secure-your-azure-sql-database"></a><span data-ttu-id="8bd2e-103">Zabezpečení databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="8bd2e-103">Secure your Azure SQL Database</span></span>

<span data-ttu-id="8bd2e-104">Služba SQL Database chrání vaše data omezením přístupu k databázi pomocí pravidel brány firewall, ověřovacími mechanismy vyžadujícími po uživatelích prokázání identity a autorizací přístupu k datům prostřednictvím členství a oprávnění na základě rolí, stejně jako prostřednictvím zabezpečení na úrovni řádku a dynamického maskování dat.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-104">SQL Database secures your data by limiting access to your database using firewall rules, authentication mechanisms requiring users to prove their identity, and authorization to data through role-based memberships and permissions, as well as through row-level security and dynamic data masking.</span></span>

<span data-ttu-id="8bd2e-105">Můžete zvýšit ochranu databáze před uživateli se zlými úmysly a neoprávněným přístupem pomocí několika jednoduchých kroků.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-105">You can improve the protection of your database against malicious users or unauthorized access with just a few simple steps.</span></span> <span data-ttu-id="8bd2e-106">V tomto kurzu jste postup:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-106">In this tutorial you learn to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="8bd2e-107">Nastavit pravidla brány firewall na úrovni serveru pro váš server na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8bd2e-107">Set up server-level firewall rules for your server in the Azure portal</span></span>
> * <span data-ttu-id="8bd2e-108">Nastavit pravidla brány firewall na úrovni databáze pro vaši databázi pomocí aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="8bd2e-108">Set up database-level firewall rules for your database using SSMS</span></span>
> * <span data-ttu-id="8bd2e-109">Připojení k vaší databázi pomocí zabezpečené připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="8bd2e-109">Connect to your database using a secure connection string</span></span>
> * <span data-ttu-id="8bd2e-110">Spravovat přístup uživatelů</span><span class="sxs-lookup"><span data-stu-id="8bd2e-110">Manage user access</span></span>
> * <span data-ttu-id="8bd2e-111">Chraňte svá data pomocí šifrování</span><span class="sxs-lookup"><span data-stu-id="8bd2e-111">Protect your data with encryption</span></span>
> * <span data-ttu-id="8bd2e-112">Povolit auditování databáze SQL</span><span class="sxs-lookup"><span data-stu-id="8bd2e-112">Enable SQL Database auditing</span></span>
> * <span data-ttu-id="8bd2e-113">Povolení detekce hrozeb databáze SQL</span><span class="sxs-lookup"><span data-stu-id="8bd2e-113">Enable SQL Database threat detection</span></span>

<span data-ttu-id="8bd2e-114">Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-114">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8bd2e-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8bd2e-115">Prerequisites</span></span>

<span data-ttu-id="8bd2e-116">K dokončení tohoto kurzu, ujistěte se, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-116">To complete this tutorial, make sure you have the following:</span></span>

- <span data-ttu-id="8bd2e-117">Nainstalovanou nejnovější verzi [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="8bd2e-117">Installed the newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span></span> 
- <span data-ttu-id="8bd2e-118">Nainstalovaný Microsoft Excel</span><span class="sxs-lookup"><span data-stu-id="8bd2e-118">Installed Microsoft Excel</span></span>
- <span data-ttu-id="8bd2e-119">Vytvoření služby Azure SQL server a databáze - najdete v části [vytvoření Azure SQL database na portálu Azure](sql-database-get-started-portal.md), [vytvářet izolované databáze Azure SQL pomocí rozhraní příkazového řádku Azure](sql-database-get-started-cli.md), a [vytvořit jeden SQL Azure databáze pomocí prostředí PowerShell](sql-database-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8bd2e-119">Created an Azure SQL server and database - See [Create an Azure SQL database in the Azure portal](sql-database-get-started-portal.md), [Create a single Azure SQL database using the Azure CLI](sql-database-get-started-cli.md), and [Create a single Azure SQL database using PowerShell](sql-database-get-started-powershell.md).</span></span> 

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="8bd2e-120">Přihlášení k portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8bd2e-120">Log in to the Azure portal</span></span>

<span data-ttu-id="8bd2e-121">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8bd2e-121">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a><span data-ttu-id="8bd2e-122">Vytvoření pravidla brány firewall na úrovni serveru na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8bd2e-122">Create a server-level firewall rule in the Azure portal</span></span>

<span data-ttu-id="8bd2e-123">Databáze SQL jsou chráněné bránou firewall v Azure.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-123">SQL databases are protected by a firewall in Azure.</span></span> <span data-ttu-id="8bd2e-124">Ve výchozím nastavení všechna připojení k serveru a databází uvnitř serveru odmítají s výjimkou připojení z jiných služeb systému Azure.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-124">By default, all connections to the server and the databases inside the server are rejected except for connections from other Azure services.</span></span> <span data-ttu-id="8bd2e-125">Další informace najdete v tématu [pravidla brány firewall úrovni serveru a na úrovni databáze Azure SQL Database](sql-database-firewall-configure.md).</span><span class="sxs-lookup"><span data-stu-id="8bd2e-125">For more information, see [Azure SQL Database server-level and database-level firewall rules](sql-database-firewall-configure.md).</span></span>

<span data-ttu-id="8bd2e-126">Nejbezpečnější konfiguraci je nastavení "Povolit přístup ke službám Azure" na hodnotu OFF.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-126">The most secure configuration is to set 'Allow access to Azure services' to OFF.</span></span> <span data-ttu-id="8bd2e-127">Pokud potřebujete připojení k databázi ze služby virtuální počítač Azure nebo cloudu, měli byste vytvořit [vyhrazenou IP adresu](../virtual-network/virtual-networks-reserved-public-ip.md) a povolit pouze vyhrazené IP adresy přístup přes bránu firewall.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-127">If you need to connect to the database from an Azure VM or cloud service, you should create a [Reserved IP](../virtual-network/virtual-networks-reserved-public-ip.md) and allow only the reserved IP address access through the firewall.</span></span> 

<span data-ttu-id="8bd2e-128">Postupujte podle těchto kroků můžete vytvořit [pravidlo brány firewall na úrovni serveru SQL Database](sql-database-firewall-configure.md) pro váš server umožňuje připojení z konkrétní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-128">Follow these steps to create a [SQL Database server-level firewall rule](sql-database-firewall-configure.md) for your server to allow connections from a specific IP address.</span></span> 

> [!NOTE]
> <span data-ttu-id="8bd2e-129">Pokud jste vytvořili ukázkové databáze v Azure pomocí jedné z předchozích kurzy nebo – elementy QuickStart a fungují v tomto kurzu v počítači se stejnou adresou IP, který měl při si projít tyto kurzy, můžete tento krok přeskočit, protože už je nutné vytvořit ated pravidlo brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-129">If you have created a sample database in Azure using one of the previous tutorials or quickstarts and are performing this tutorial on a computer with the same IP address that it had when you walked through those tutorials, you can skip this step as you will have already created a server-level firewall rule.</span></span>
>

1. <span data-ttu-id="8bd2e-130">Klikněte na tlačítko **databází SQL** z nabídky levé straně a klikněte na databázi, kterou chcete nakonfigurovat bránu firewall pravidla pro u **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-130">Click **SQL databases** from the left-hand menu and click the database you would like to configure the firewall rule for on the **SQL databases** page.</span></span> <span data-ttu-id="8bd2e-131">Otevře se stránka Přehled pro vaši databázi, ukazuje název plně kvalifikovaný serveru (například **mynewserver 20170313.database.windows.net**) a poskytuje možnosti pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-131">The overview page for your database opens, showing you the fully qualified server name (such as **mynewserver-20170313.database.windows.net**) and provides options for further configuration.</span></span>

      ![pravidlo brány firewall serveru](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. <span data-ttu-id="8bd2e-133">Klikněte na **Nastavit bránu firewall serveru** na panelu nástrojů, jak je vidět na předchozím obrázku.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-133">Click **Set server firewall** on the toolbar as shown in the previous image.</span></span> <span data-ttu-id="8bd2e-134">Otevře se stránka **Nastavení brány firewall** pro server služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-134">The **Firewall settings** page for the SQL Database server opens.</span></span> 

3. <span data-ttu-id="8bd2e-135">Klikněte na tlačítko **přidat IP adresu klienta** na panelu nástrojů přidejte veřejnou IP adresu počítače, připojení se k portálu a zadejte pravidlo brány firewall ručně a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-135">Click **Add client IP** on the toolbar to add the public IP address of the computer connected to the portal with or enter the firewall rule manually and then click **Save**.</span></span>

      ![nastavení pravidla brány firewall serveru](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. <span data-ttu-id="8bd2e-137">Kliknutím na **OK** a pak na **X** zavřete stránku **Nastavení brány firewall**.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-137">Click **OK** and then click the **X** to close the **Firewall settings** page.</span></span>

<span data-ttu-id="8bd2e-138">Nyní můžete připojit k jakékoli databázi na serveru se zadanou IP adresu nebo rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-138">You can now connect to any database in the server with the specified IP address or IP address range.</span></span>

> [!NOTE]
> <span data-ttu-id="8bd2e-139">SQL Database komunikuje přes port 1433.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-139">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="8bd2e-140">Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 1433 bránou firewall vaší sítě povolený.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-140">If you are trying to connect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="8bd2e-141">Pokud je to tak, nebudete se moct připojit k serveru Azure SQL Database, dokud vaše IT oddělení neotevře port 1433.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-141">If so, you will not be able to connect to your Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a><span data-ttu-id="8bd2e-142">Vytvoření pravidla brány firewall na úrovni databáze pomocí aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="8bd2e-142">Create a database-level firewall rule using SSMS</span></span>

<span data-ttu-id="8bd2e-143">Pravidla brány firewall na úrovni databáze umožňují vytvořit jinou bránu firewall nastavení pro jiné databáze v rámci stejného logického serveru a k vytvoření pravidla brány firewall, které jsou přenosné - znamená, že následují databáze během [převzetíslužebpřiselhání](sql-database-geo-replication-overview.md) místo uložené na serveru SQL server.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-143">Database-level firewall rules enable you to create different firewall settings for different databases within the same logical server and to create firewall rules that are portable - meaning that they follow the database during a [failover](sql-database-geo-replication-overview.md) rather than being stored on the SQL server.</span></span> <span data-ttu-id="8bd2e-144">Pravidla brány firewall na úrovni databáze může být pouze nakonfigurovaná pomocí příkazů Transact-SQL a pouze po nakonfigurování prvního pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-144">Database-level firewall rules can only be configured by using Transact-SQL statements and only after you have configured the first server-level firewall rule.</span></span> <span data-ttu-id="8bd2e-145">Další informace najdete v tématu [pravidla brány firewall úrovni serveru a na úrovni databáze Azure SQL Database](sql-database-firewall-configure.md).</span><span class="sxs-lookup"><span data-stu-id="8bd2e-145">For more information, see [Azure SQL Database server-level and database-level firewall rules](sql-database-firewall-configure.md).</span></span>

<span data-ttu-id="8bd2e-146">Postupujte podle následujících kroků k vytvoření pravidla brány firewall pro specifické pro databázi.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-146">Follows these steps to create a database-specific firewall rule.</span></span>

1. <span data-ttu-id="8bd2e-147">Připojení ke své databázi, například pomocí [SQL Server Management Studio](./sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="8bd2e-147">Connect to your database, for example using [SQL Server Management Studio](./sql-database-connect-query-ssms.md).</span></span>

2. <span data-ttu-id="8bd2e-148">V Průzkumníku objektů, klikněte pravým tlačítkem na databázi, kterou chcete přidat pravidlo brány firewall pro a klikněte na **nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-148">In Object Explorer, right-click on the database you want to add a firewall rule for and click **New Query**.</span></span> <span data-ttu-id="8bd2e-149">Otevře se prázdné okno dotazu připojené k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-149">A blank query window opens that is connected to your database.</span></span>

3. <span data-ttu-id="8bd2e-150">V okně dotazu změnit IP adresu na veřejnou IP adresu a potom spusťte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-150">In the query window, modify the IP address to your public IP address and then execute the following query:</span></span>

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. <span data-ttu-id="8bd2e-151">Na panelu nástrojů klikněte na tlačítko **Execute** a vytvořte tak pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-151">On the toolbar, click **Execute** to create the firewall rule.</span></span>

## <a name="view-how-to-connect-an-application-to-your-database-using-a-secure-connection-string"></a><span data-ttu-id="8bd2e-152">Zobrazit postup připojení aplikace k vaší databázi pomocí zabezpečené připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="8bd2e-152">View how to connect an application to your database using a secure connection string</span></span>

<span data-ttu-id="8bd2e-153">Aby zabezpečené, šifrované připojení mezi klientská aplikace a SQL Database, připojovací řetězec má nakonfigurovat tak, aby:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-153">To ensure a secure, encrypted connection between a client application and SQL Database, the connection string has to be configured to:</span></span>

- <span data-ttu-id="8bd2e-154">Žádosti o šifrované připojení, a</span><span class="sxs-lookup"><span data-stu-id="8bd2e-154">Request an encrypted connection, and</span></span>
- <span data-ttu-id="8bd2e-155">Není důvěřovat certifikátu serveru.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-155">To not trust the server certificate.</span></span> 

<span data-ttu-id="8bd2e-156">Tím se naváže připojení pomocí zabezpečení TLS (Transport Layer) a snižuje riziko útoků man-in-the-middle.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-156">This establishes a connection using Transport Layer Security (TLS) and reduces the risk of man-in-the-middle attacks.</span></span> <span data-ttu-id="8bd2e-157">Můžete získat správně nakonfigurované připojovací řetězce pro vaši databázi SQL pro podporované klientské ovladače z portálu Azure jak je vidět na tomto snímku obrazovky pro technologii ADO.net.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-157">You can obtain correctly configured connection strings for your SQL Database for supported client drivers from the Azure portal as shown for ADO.net in this screenshot.</span></span>

1. <span data-ttu-id="8bd2e-158">Vyberte **databází SQL** z nabídky na levé straně a klikněte na databázi **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-158">Select **SQL databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span>

2. <span data-ttu-id="8bd2e-159">Na **přehled** stránky pro vaši databázi, klikněte na tlačítko **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-159">On the **Overview** page for your database, click **Show database connection strings**.</span></span>

3. <span data-ttu-id="8bd2e-160">Zkontrolujte úplný připojovací řetězec **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-160">Review the complete **ADO.NET** connection string.</span></span>

    ![Připojovací řetězec pro ADO.NET](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a><span data-ttu-id="8bd2e-162">Vytváření uživatelů databáze</span><span class="sxs-lookup"><span data-stu-id="8bd2e-162">Creating database users</span></span>

<span data-ttu-id="8bd2e-163">Před vytvořením všechny uživatele, musíte nejprve zvolit jednu z Azure SQL Database podporuje dva typy ověřování:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-163">Before creating any users, you must first choose from one of two authentication types supported by Azure SQL Database:</span></span> 

<span data-ttu-id="8bd2e-164">**Ověřování SQL**, který používá uživatelské jméno a heslo pro přihlášení a uživatele, které jsou platné pouze v kontextu konkrétní databáze v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-164">**SQL Authentication**, which uses username and password for logins and users that are valid only in the context of a specific database within a logical server.</span></span> 

<span data-ttu-id="8bd2e-165">**Ověřování služby Azure Active Directory**, identity, které jsou spravované službou Azure Active Directory, který používá.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-165">**Azure Active Directory Authentication**, which uses identities managed by Azure Active Directory.</span></span> 

<span data-ttu-id="8bd2e-166">Pokud chcete použít [Azure Active Directory](./sql-database-aad-authentication.md) k ověření proti databázi SQL, musí existovat vyplněná Azure Active Directory, abyste mohli pokračovat.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-166">If you want to use [Azure Active Directory](./sql-database-aad-authentication.md) to authenticate against SQL Database, a populated Azure Active Directory must exist before you can proceed.</span></span>

<span data-ttu-id="8bd2e-167">Postupujte podle těchto kroků můžete vytvořit uživatele pomocí ověřování SQL:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-167">Follow these steps to create a user using SQL Authentication:</span></span>

1. <span data-ttu-id="8bd2e-168">Připojení ke své databázi, například pomocí [SQL Server Management Studio](./sql-database-connect-query-ssms.md) pomocí svých přihlašovacích údajů správce serveru.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-168">Connect to your database, for example using [SQL Server Management Studio](./sql-database-connect-query-ssms.md) using your server admin credentials.</span></span>

2. <span data-ttu-id="8bd2e-169">V Průzkumníku objektů, klikněte pravým tlačítkem na databázi, kterou chcete přidat nového uživatele na a klikněte na tlačítko **nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-169">In Object Explorer, right-click on the database you want to add a new user on and click **New Query**.</span></span> <span data-ttu-id="8bd2e-170">Otevře okno prázdné dotazu, který je připojen k vybrané databázi.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-170">A blank query window opens that is connected to the selected database.</span></span>

3. <span data-ttu-id="8bd2e-171">Do okna dotazu zadejte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-171">In the query window, enter the following query:</span></span>

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. <span data-ttu-id="8bd2e-172">Na panelu nástrojů klikněte na tlačítko **Execute** vytvořit uživateli.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-172">On the toolbar, click **Execute** to create the user.</span></span>

5. <span data-ttu-id="8bd2e-173">Ve výchozím nastavení uživatel může připojit k databázi, ale nemá oprávnění číst nebo zapisovat data.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-173">By default, the user can connect to the database, but has no permissions to read or write data.</span></span> <span data-ttu-id="8bd2e-174">Udělení těchto oprávnění pro nově vytvořeného uživatele, spusťte následující dva příkazy v novém okně dotazu</span><span class="sxs-lookup"><span data-stu-id="8bd2e-174">To grant these permissions to the newly created user, execute the following two commands in a new query window</span></span>

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

<span data-ttu-id="8bd2e-175">Je vhodné vytvořit tyto účty bez oprávnění správce na úrovni databáze pro připojení k databázi, pokud potřebujete provést úlohy správce, jako je vytváření nových uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-175">It is best practice to create these non-administrator accounts at the database level to connect to your database unless you need to execute administrator tasks like creating new users.</span></span> <span data-ttu-id="8bd2e-176">Přečtěte si [kurz služby Azure Active Directory](./sql-database-aad-authentication-configure.md) o tom, jak ověřit pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-176">Please review the [Azure Active Directory tutorial](./sql-database-aad-authentication-configure.md) on how to authenticate using Azure Active Directory.</span></span>


## <a name="protect-your-data-with-encryption"></a><span data-ttu-id="8bd2e-177">Chraňte svá data pomocí šifrování</span><span class="sxs-lookup"><span data-stu-id="8bd2e-177">Protect your data with encryption</span></span>

<span data-ttu-id="8bd2e-178">Azure SQL Database transparentní šifrování dat (šifrování TDE) automaticky šifruje vaše data v klidu, bez nutnosti změny k aplikaci přístup k databázi šifrované.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-178">Azure SQL Database transparent data encryption (TDE) automatically encrypts your data at rest, without requiring any changes to the application accessing the encrypted database.</span></span> <span data-ttu-id="8bd2e-179">Pro nově vytvořené databáze je ve výchozím nastavení šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-179">For newly created databases, TDE is on by default.</span></span> <span data-ttu-id="8bd2e-180">Chcete-li povolit šifrování TDE pro vaši databázi nebo ověřte, zda je šifrování TDE na, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-180">To enable TDE for your database or to verify that TDE is on, follow these steps:</span></span>

1. <span data-ttu-id="8bd2e-181">Vyberte **databází SQL** z nabídky na levé straně a klikněte na databázi **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-181">Select **SQL databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 

2. <span data-ttu-id="8bd2e-182">Klikněte na **transparentní šifrování dat** otevřete stránku konfigurace pro šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-182">Click on **Transparent data encryption** to open the configuration page for TDE.</span></span>

    ![Transparentní šifrování dat](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. <span data-ttu-id="8bd2e-184">V případě potřeby nastavte **šifrování dat** na hodnotu ON a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-184">If necessary, set **Data encryption** to ON and click **Save**.</span></span>

<span data-ttu-id="8bd2e-185">Spustí se proces šifrování na pozadí.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-185">The encryption process starts in the background.</span></span> <span data-ttu-id="8bd2e-186">Průběh můžete sledovat pomocí připojení k databázi SQL pomocí [SQL Server Management Studio](./sql-database-connect-query-ssms.md) dotazováním encryption_state sloupec `sys.dm_database_encryption_keys` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-186">You can monitor the progress by connecting to SQL Database using [SQL Server Management Studio](./sql-database-connect-query-ssms.md) by querying the encryption_state column of the `sys.dm_database_encryption_keys` view.</span></span>

## <a name="enable-sql-database-auditing-if-necessary"></a><span data-ttu-id="8bd2e-187">Povolit auditování databáze SQL, v případě potřeby</span><span class="sxs-lookup"><span data-stu-id="8bd2e-187">Enable SQL Database auditing, if necessary</span></span>

<span data-ttu-id="8bd2e-188">Auditování databáze SQL Azure sleduje události databáze a zápisu, které mají auditu protokolu ve vašem účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-188">Azure SQL Database Auditing tracks database events and writes them to an audit log in your Azure Storage account.</span></span> <span data-ttu-id="8bd2e-189">Auditování vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou případné porušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-189">Auditing can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate potential security violations.</span></span> <span data-ttu-id="8bd2e-190">Postupujte podle těchto kroků můžete vytvořit výchozí zásady pro vaši databázi SQL auditování:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-190">Follow these steps to create a default auditing policy for your SQL database:</span></span>

1. <span data-ttu-id="8bd2e-191">Vyberte **databází SQL** z nabídky na levé straně a klikněte na databázi **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-191">Select **SQL databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 

2. <span data-ttu-id="8bd2e-192">V okně Nastavení vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-192">In the Settings blade, select **Auditing & Threat Detection**.</span></span> <span data-ttu-id="8bd2e-193">Všimněte si, že auditování na úrovni serveru je diabled a zda je **zobrazit nastavení serveru** odkaz, který umožňuje zobrazit nebo upravit nastavení auditování ze tento kontext serveru.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-193">Notice that sever-level auditing is diabled and that there is a **View server settings** link that allows you to view or modify the server auditing settings from this context.</span></span>

    ![Auditování okno](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. <span data-ttu-id="8bd2e-195">Pokud dáváte přednost umožňuje typ auditu (nebo umístění?), než je zadaná na úrovni serveru, zapněte **ON** auditování a vyberte **Blob** typu auditování.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-195">If you prefer to enable an Audit type (or location?) different from the one specified at the server level, turn **ON** Auditing, and choose the **Blob** Auditing Type.</span></span> <span data-ttu-id="8bd2e-196">Pokud je auditování objektů Blob serveru je povoleno, audit databáze nakonfigurována, budou existovat node souběžně s auditování objektů Blob serveru.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-196">If server Blob auditing is enabled, the database configured audit will exist side by side with the server Blob audit.</span></span>

    ![Zapnout auditování](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. <span data-ttu-id="8bd2e-198">Vyberte **podrobnosti úložiště** otevřete okno úložiště protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-198">Select **Storage Details** to open the Audit Logs Storage Blade.</span></span> <span data-ttu-id="8bd2e-199">Vyberte účet úložiště Azure, kde budou uloženy protokoly, a pak klikněte na dobu uchování, po jejímž uplynutí se odstraní starých protokolů, **OK** dole.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-199">Select the Azure storage account where logs will be saved, and the retention period, after which the old logs will be deleted, then click **OK** at the bottom.</span></span> 

   > [!TIP]
   > <span data-ttu-id="8bd2e-200">Použijte stejný účet úložiště pro všechny databáze, auditované k plnému využití mimo auditování šablony sestavy.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-200">Use the same storage account for all audited databases to get the most out of the auditing reports templates.</span></span>
   > 

5. <span data-ttu-id="8bd2e-201">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-201">Click **Save**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8bd2e-202">Pokud chcete přizpůsobit auditované události, můžete k tomu pomocí prostředí PowerShell nebo rozhraní REST API - najdete v článku [automatizace (PowerShell / REST API)](sql-database-auditing.md#subheading-7) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-202">If you want to customize the audited events, you can do this via PowerShell or REST API - see the [Automation (PowerShell / REST API)](sql-database-auditing.md#subheading-7) section for more details.</span></span>
>

## <a name="enable-sql-database-threat-detection"></a><span data-ttu-id="8bd2e-203">Povolení detekce hrozeb databáze SQL</span><span class="sxs-lookup"><span data-stu-id="8bd2e-203">Enable SQL Database threat detection</span></span>

<span data-ttu-id="8bd2e-204">Detekce hrozeb poskytuje novou vrstvu zabezpečení, která uživatelům umožňuje zjistit a reagovat na potenciální hrozby, kdy k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-204">Threat Detection provides a new layer of security, which enables customers to detect and respond to potential threats as they occur by providing security alerts on anomalous activities.</span></span> <span data-ttu-id="8bd2e-205">Uživatele můžete prozkoumat podezřelé události pomocí auditování databáze SQL k určení, pokud vyplývají z pokus o přístup, porušení nebo využívat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-205">Users can explore the suspicious events using SQL Database Auditing to determine if they result from an attempt to access, breach or exploit data in the database.</span></span> <span data-ttu-id="8bd2e-206">Detekce hrozeb zjednodušuje na potenciální hrozby adres do databáze bez nutnosti odborné zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-206">Threat Detection makes it simple to address potential threats to the database without the need to be a security expert or manage advanced security monitoring systems.</span></span>
<span data-ttu-id="8bd2e-207">Například detekce hrozeb zjistila určité nezvyklé databázové aktivity, které indikují potenciální pokusu o Injektáž SQL.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-207">For example, Threat Detection detects certain anomalous database activities indicating potential SQL injection attempts.</span></span> <span data-ttu-id="8bd2e-208">Injektáž SQL je jedním z běžné problémy zabezpečení webových aplikací na Internetu, slouží k útokům na základě dat aplikace.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-208">SQL injection is one of the common Web application security issues on the Internet, used to attack data-driven applications.</span></span> <span data-ttu-id="8bd2e-209">Útočníci využít výhod vložit škodlivý příkazy SQL, do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi aplikace ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-209">Attackers take advantage of application vulnerabilities to inject malicious SQL statements into application entry fields, for breaching or modifying data in the database.</span></span>

1. <span data-ttu-id="8bd2e-210">Přejděte do okna konfigurace SQL databáze, kterou chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-210">Navigate to the configuration blade of the SQL database you want to monitor.</span></span> <span data-ttu-id="8bd2e-211">V okně Nastavení vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-211">In the Settings blade, select **Auditing & Threat Detection**.</span></span>

    ![Navigační podokno](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. <span data-ttu-id="8bd2e-213">V **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-213">In the **Auditing & Threat Detection** configuration blade turn **ON** auditing, which will display the threat detection settings.</span></span>

3. <span data-ttu-id="8bd2e-214">Zapnout **ON** detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-214">Turn **ON** threat detection.</span></span>

4. <span data-ttu-id="8bd2e-215">Konfigurace seznamu e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklé databázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-215">Configure the list of emails that will receive security alerts upon detection of anomalous database activities.</span></span>

5. <span data-ttu-id="8bd2e-216">Klikněte na tlačítko **Uložit** v **auditování a detekce hrozeb** uložíte nové nebo aktualizované auditování a zásady detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-216">Click **Save** in the **Auditing & Threat detection** blade to save the new or updated auditing and threat detection policy.</span></span>

    ![Navigační podokno](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    <span data-ttu-id="8bd2e-218">Pokud se zjistila nezvyklé databázové aktivity, obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-218">If anomalous database activities are detected, you will receive an email notification upon detection of anomalous database activities.</span></span> <span data-ttu-id="8bd2e-219">E-mailu bude poskytují informace o události podezřelé zabezpečení, včetně povahu neobvyklé aktivity, název databáze, název serveru a čas události.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-219">The email will provide information on the suspicious security event including the nature of the anomalous activities, database name, server name and the event time.</span></span> <span data-ttu-id="8bd2e-220">Kromě toho bude poskytovat informace na možné příčiny a doporučené akce ke zkoumání a zmírnit potenciální hrozbu do databáze.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-220">In addition, it will provide information on possible causes and recommended actions to investigate and mitigate the potential threat to the database.</span></span> <span data-ttu-id="8bd2e-221">V dalších krocích vás prostřednictvím co dělat, by měl obdržíte e-mailu:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-221">The next steps walk you through what to do should you receive such an email:</span></span>

    ![E-mailu detekce hrozeb](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. <span data-ttu-id="8bd2e-223">V e-mailu, klikněte na **protokolu auditování databáze SQL Azure** odkaz, který bude spuštění portálu Azure a zobrazit relevantní auditování záznamy v době podezřelé události.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-223">In the email, click on the **Azure SQL Auditing Log** link, which will launch the Azure portal and show the relevant auditing records around the time of the suspicious event.</span></span>

    ![Záznamy auditu](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. <span data-ttu-id="8bd2e-225">Klikněte na záznamy auditu zobrazíte další podrobnosti v databázi podezřelé aktivity, například příkazy SQL, selhání důvod a klient IP.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-225">Click on the audit records to view more details on the suspicious database activities such as SQL statement, failure reason and client IP.</span></span>

    ![Podrobnosti záznamu](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. <span data-ttu-id="8bd2e-227">V okně auditování záznamy, klikněte na tlačítko **otevřete v aplikaci Excel** otevřete předem nakonfigurovaná v aplikaci excel šablony k importu a spuštění hlubší analýzy protokol auditu v době podezřelé události.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-227">In the Auditing Records blade, click  **Open in Excel** to open a pre-configured excel template to import and run deeper analysis of the audit log around the time of the suspicious event.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bd2e-228">V aplikaci Excel 2010 nebo novější, Power Query a **rychle zkombinovat** nastavení je povinné.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-228">In Excel 2010 or later, Power Query and the **Fast Combine** setting is required.</span></span>

    ![Otevřené záznamy v aplikaci Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. <span data-ttu-id="8bd2e-230">Ke konfiguraci **rychle zkombinovat** nastavení - v **POWER QUERY** pásu karet vyberte **možnosti** zobrazíte dialogové okno Možnosti.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-230">To configure the **Fast Combine** setting - In the **POWER QUERY** ribbon tab, select **Options** to display the Options dialog.</span></span> <span data-ttu-id="8bd2e-231">Vyberte v části o ochraně osobních údajů a vyberte druhou možnost - 'Ignorovat úrovně soukromí a potenciálně tak vylepšit výkon':</span><span class="sxs-lookup"><span data-stu-id="8bd2e-231">Select the Privacy section and choose the second option - 'Ignore the Privacy Levels and potentially improve performance':</span></span>

    ![Kombinování rychlé aplikace Excel](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. <span data-ttu-id="8bd2e-233">Načíst protokoly auditu SQL, zajistěte, aby parametrů v nastavení kartě jsou správně nastavena a poté vyberte pásu karet, Data a klikněte na tlačítko 'aktualizovat vše..</span><span class="sxs-lookup"><span data-stu-id="8bd2e-233">To load SQL audit logs, ensure that the parameters in the settings tab are set correctly and then select the 'Data' ribbon and click the 'Refresh All' button.</span></span>

    ![Parametry aplikace Excel](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. <span data-ttu-id="8bd2e-235">Výsledky se zobrazí v **protokoly auditu SQL** listu, která umožňuje spustit hlubší analýzu neobvyklé aktivity, které byly zjištěny a zmírnit dopad událostí zabezpečení ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-235">The results appear in the **SQL Audit Logs** sheet which enables you to run deeper analysis of the anomalous activities that were detected, and mitigate the impact of the security event in your application.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8bd2e-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8bd2e-236">Next steps</span></span>
<span data-ttu-id="8bd2e-237">Můžete zvýšit ochranu databáze před uživateli se zlými úmysly a neoprávněným přístupem pomocí několika jednoduchých kroků.</span><span class="sxs-lookup"><span data-stu-id="8bd2e-237">You can improve the protection of your database against malicious users or unauthorized access with just a few simple steps.</span></span> <span data-ttu-id="8bd2e-238">V tomto kurzu jste postup:</span><span class="sxs-lookup"><span data-stu-id="8bd2e-238">In this tutorial you learn to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="8bd2e-239">Nastavte pravidla firewallu pro databázi serveru a nebo</span><span class="sxs-lookup"><span data-stu-id="8bd2e-239">Set up firewall rules for your sever and or database</span></span>
> * <span data-ttu-id="8bd2e-240">Připojení k vaší databázi pomocí zabezpečené připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="8bd2e-240">Connect to your database using a secure connection string</span></span>
> * <span data-ttu-id="8bd2e-241">Spravovat přístup uživatelů</span><span class="sxs-lookup"><span data-stu-id="8bd2e-241">Manage user access</span></span>
> * <span data-ttu-id="8bd2e-242">Chraňte svá data pomocí šifrování</span><span class="sxs-lookup"><span data-stu-id="8bd2e-242">Protect your data with encryption</span></span>
> * <span data-ttu-id="8bd2e-243">Povolit auditování databáze SQL</span><span class="sxs-lookup"><span data-stu-id="8bd2e-243">Enable SQL Database auditing</span></span>
> * <span data-ttu-id="8bd2e-244">Povolení detekce hrozeb databáze SQL</span><span class="sxs-lookup"><span data-stu-id="8bd2e-244">Enable SQL Database threat detection</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="8bd2e-245">Vylepšení výkonu SQL Database</span><span class="sxs-lookup"><span data-stu-id="8bd2e-245">Improve SQL Database performance</span></span>](sql-database-performance-tutorial.md)

