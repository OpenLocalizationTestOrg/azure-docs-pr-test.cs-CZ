---
title: aaaSecure Azure SQL database | Microsoft Docs
description: "Další informace o toosecure techniky a funkce Azure SQL database."
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
ms.openlocfilehash: 1450d633d6f65faf1b8a2dc0dc7dfe996fb0719d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-azure-sql-database"></a><span data-ttu-id="e9379-103">Zabezpečení databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="e9379-103">Secure your Azure SQL Database</span></span>

<span data-ttu-id="e9379-104">Databáze SQL slouží k zabezpečení dat omezením přístupu k databázi tooyour pomocí pravidel brány firewall, ověřovací mechanismy, které by uživatelé tooprove své identity a autorizace toodata prostřednictvím členství na základě rolí a oprávnění, a také prostřednictvím zabezpečení na úrovni řádků a maskování dynamická data.</span><span class="sxs-lookup"><span data-stu-id="e9379-104">SQL Database secures your data by limiting access tooyour database using firewall rules, authentication mechanisms requiring users tooprove their identity, and authorization toodata through role-based memberships and permissions, as well as through row-level security and dynamic data masking.</span></span>

<span data-ttu-id="e9379-105">Hello ochranu databáze před uživateli se zlými úmysly a neoprávněným přístupem pomocí několika jednoduchých kroků můžete zvýšit.</span><span class="sxs-lookup"><span data-stu-id="e9379-105">You can improve hello protection of your database against malicious users or unauthorized access with just a few simple steps.</span></span> <span data-ttu-id="e9379-106">V tomto kurzu jste postup:</span><span class="sxs-lookup"><span data-stu-id="e9379-106">In this tutorial you learn to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="e9379-107">Nastavit pravidla brány firewall na úrovni serveru pro váš server v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e9379-107">Set up server-level firewall rules for your server in hello Azure portal</span></span>
> * <span data-ttu-id="e9379-108">Nastavit pravidla brány firewall na úrovni databáze pro vaši databázi pomocí aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="e9379-108">Set up database-level firewall rules for your database using SSMS</span></span>
> * <span data-ttu-id="e9379-109">Připojit databáze tooyour pomocí zabezpečené připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="e9379-109">Connect tooyour database using a secure connection string</span></span>
> * <span data-ttu-id="e9379-110">Spravovat přístup uživatelů</span><span class="sxs-lookup"><span data-stu-id="e9379-110">Manage user access</span></span>
> * <span data-ttu-id="e9379-111">Chraňte svá data pomocí šifrování</span><span class="sxs-lookup"><span data-stu-id="e9379-111">Protect your data with encryption</span></span>
> * <span data-ttu-id="e9379-112">Povolit auditování databáze SQL</span><span class="sxs-lookup"><span data-stu-id="e9379-112">Enable SQL Database auditing</span></span>
> * <span data-ttu-id="e9379-113">Povolení detekce hrozeb databáze SQL</span><span class="sxs-lookup"><span data-stu-id="e9379-113">Enable SQL Database threat detection</span></span>

<span data-ttu-id="e9379-114">Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="e9379-114">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9379-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e9379-115">Prerequisites</span></span>

<span data-ttu-id="e9379-116">toocomplete tento kurz, ověřte zda máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="e9379-116">toocomplete this tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="e9379-117">Nejnovější verze nainstalované hello [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="e9379-117">Installed hello newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span></span> 
- <span data-ttu-id="e9379-118">Nainstalovaný Microsoft Excel</span><span class="sxs-lookup"><span data-stu-id="e9379-118">Installed Microsoft Excel</span></span>
- <span data-ttu-id="e9379-119">Vytvoření služby Azure SQL server a databáze - najdete v části [vytvoření Azure SQL database v hello portál Azure](sql-database-get-started-portal.md), [vytvářet izolované databáze Azure SQL pomocí rozhraní příkazového řádku Azure hello](sql-database-get-started-cli.md), a [vytvořit jeden SQL Azure databáze pomocí prostředí PowerShell](sql-database-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e9379-119">Created an Azure SQL server and database - See [Create an Azure SQL database in hello Azure portal](sql-database-get-started-portal.md), [Create a single Azure SQL database using hello Azure CLI](sql-database-get-started-cli.md), and [Create a single Azure SQL database using PowerShell](sql-database-get-started-powershell.md).</span></span> 

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="e9379-120">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e9379-120">Log in toohello Azure portal</span></span>

<span data-ttu-id="e9379-121">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e9379-121">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="e9379-122">Vytvoření pravidla brány firewall na úrovni serveru v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e9379-122">Create a server-level firewall rule in hello Azure portal</span></span>

<span data-ttu-id="e9379-123">Databáze SQL jsou chráněné bránou firewall v Azure.</span><span class="sxs-lookup"><span data-stu-id="e9379-123">SQL databases are protected by a firewall in Azure.</span></span> <span data-ttu-id="e9379-124">Ve výchozím nastavení všechna připojení toohello server hello databáze a uvnitř hello server odmítají s výjimkou připojení z jiných služeb systému Azure.</span><span class="sxs-lookup"><span data-stu-id="e9379-124">By default, all connections toohello server and hello databases inside hello server are rejected except for connections from other Azure services.</span></span> <span data-ttu-id="e9379-125">Další informace najdete v tématu [pravidla brány firewall úrovni serveru a na úrovni databáze Azure SQL Database](sql-database-firewall-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e9379-125">For more information, see [Azure SQL Database server-level and database-level firewall rules](sql-database-firewall-configure.md).</span></span>

<span data-ttu-id="e9379-126">nejbezpečnější konfigurace Hello je tooOFF tooset 'Povolit přístup k službám tooAzure'.</span><span class="sxs-lookup"><span data-stu-id="e9379-126">hello most secure configuration is tooset 'Allow access tooAzure services' tooOFF.</span></span> <span data-ttu-id="e9379-127">Pokud potřebujete tooconnect toohello databáze ze služby virtuální počítač Azure nebo cloudu, měli byste vytvořit [vyhrazenou IP adresu](../virtual-network/virtual-networks-reserved-public-ip.md) a povolit pouze hello vyhrazené IP adresy přístup přes bránu firewall hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-127">If you need tooconnect toohello database from an Azure VM or cloud service, you should create a [Reserved IP](../virtual-network/virtual-networks-reserved-public-ip.md) and allow only hello reserved IP address access through hello firewall.</span></span> 

<span data-ttu-id="e9379-128">Postupujte podle těchto kroků toocreate [pravidlo brány firewall na úrovni serveru SQL Database](sql-database-firewall-configure.md) pro váš server tooallow připojení z konkrétní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="e9379-128">Follow these steps toocreate a [SQL Database server-level firewall rule](sql-database-firewall-configure.md) for your server tooallow connections from a specific IP address.</span></span> 

> [!NOTE]
> <span data-ttu-id="e9379-129">Pokud jste vytvořili ukázkové databáze v Azure pomocí jedné z předchozích kurzy hello nebo – elementy QuickStart a jsou pustíte do tohoto kurzu na počítači s hello stejnou IP adresu to, že když si projít tyto kurzy, můžete tento krok přeskočíte, bude mít již vytvořil pravidlo brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="e9379-129">If you have created a sample database in Azure using one of hello previous tutorials or quickstarts and are performing this tutorial on a computer with hello same IP address that it had when you walked through those tutorials, you can skip this step as you will have already created a server-level firewall rule.</span></span>
>

1. <span data-ttu-id="e9379-130">Klikněte na tlačítko **databází SQL** z hello levé nabídce databáze hello byste chtěli tooconfigure hello pravidlo brány firewall pro hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="e9379-130">Click **SQL databases** from hello left-hand menu and click hello database you would like tooconfigure hello firewall rule for on hello **SQL databases** page.</span></span> <span data-ttu-id="e9379-131">Hello přehledová stránka otevře vaší databáze, zobrazující text hello plně kvalifikovaný název serveru (například **mynewserver 20170313.database.windows.net**) a poskytuje možnosti pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e9379-131">hello overview page for your database opens, showing you hello fully qualified server name (such as **mynewserver-20170313.database.windows.net**) and provides options for further configuration.</span></span>

      ![pravidlo brány firewall serveru](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. <span data-ttu-id="e9379-133">Klikněte na tlačítko **nastavení brány firewall serveru** na panelu nástrojů hello viz předchozí obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-133">Click **Set server firewall** on hello toolbar as shown in hello previous image.</span></span> <span data-ttu-id="e9379-134">Hello **nastavení brány Firewall** otevře se stránka pro hello databáze SQL server.</span><span class="sxs-lookup"><span data-stu-id="e9379-134">hello **Firewall settings** page for hello SQL Database server opens.</span></span> 

3. <span data-ttu-id="e9379-135">Klikněte na tlačítko **přidat IP adresu klienta** na hello tooadd nástrojů hello veřejnou IP adresu hello počítače připojené toohello portál nebo ručně zadejte hello pravidlo brány firewall a poté klikněte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e9379-135">Click **Add client IP** on hello toolbar tooadd hello public IP address of hello computer connected toohello portal with or enter hello firewall rule manually and then click **Save**.</span></span>

      ![nastavení pravidla brány firewall serveru](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. <span data-ttu-id="e9379-137">Klikněte na tlačítko **OK** a pak klikněte na tlačítko hello **X** tooclose hello **nastavení brány Firewall** stránky.</span><span class="sxs-lookup"><span data-stu-id="e9379-137">Click **OK** and then click hello **X** tooclose hello **Firewall settings** page.</span></span>

<span data-ttu-id="e9379-138">Nyní můžete přihlásit tooany databázi serveru hello s hello zadaná IP adresa nebo rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="e9379-138">You can now connect tooany database in hello server with hello specified IP address or IP address range.</span></span>

> [!NOTE]
> <span data-ttu-id="e9379-139">SQL Database komunikuje přes port 1433.</span><span class="sxs-lookup"><span data-stu-id="e9379-139">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="e9379-140">Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 1433 nemusí mít povolený bránou firewall vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="e9379-140">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="e9379-141">Pokud ano, nebudou se moct tooconnect serveru Azure SQL Database tooyour, dokud vaše IT oddělení otevře port 1433.</span><span class="sxs-lookup"><span data-stu-id="e9379-141">If so, you will not be able tooconnect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a><span data-ttu-id="e9379-142">Vytvoření pravidla brány firewall na úrovni databáze pomocí aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="e9379-142">Create a database-level firewall rule using SSMS</span></span>

<span data-ttu-id="e9379-143">Pravidla brány firewall na úrovni databáze umožňují toocreate jinou bránu firewall nastavení pro jiné databáze v rámci stejné logické serveru a toocreate pravidel brány firewall, které jsou přenosné - znamená, že následují hello databáze během hello [převzetí služeb při selhání ](sql-database-geo-replication-overview.md) místo uložené na serveru SQL server hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-143">Database-level firewall rules enable you toocreate different firewall settings for different databases within hello same logical server and toocreate firewall rules that are portable - meaning that they follow hello database during a [failover](sql-database-geo-replication-overview.md) rather than being stored on hello SQL server.</span></span> <span data-ttu-id="e9379-144">Pravidla brány firewall na úrovni databáze může být pouze nakonfigurovaná pomocí příkazů Transact-SQL a pouze po nakonfigurování hello první pravidlo brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="e9379-144">Database-level firewall rules can only be configured by using Transact-SQL statements and only after you have configured hello first server-level firewall rule.</span></span> <span data-ttu-id="e9379-145">Další informace najdete v tématu [pravidla brány firewall úrovni serveru a na úrovni databáze Azure SQL Database](sql-database-firewall-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e9379-145">For more information, see [Azure SQL Database server-level and database-level firewall rules](sql-database-firewall-configure.md).</span></span>

<span data-ttu-id="e9379-146">Následuje tyto kroky toocreate pravidlo brány firewall pro specifické pro databázi.</span><span class="sxs-lookup"><span data-stu-id="e9379-146">Follows these steps toocreate a database-specific firewall rule.</span></span>

1. <span data-ttu-id="e9379-147">Připojit databáze tooyour, například pomocí [SQL Server Management Studio](./sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="e9379-147">Connect tooyour database, for example using [SQL Server Management Studio](./sql-database-connect-query-ssms.md).</span></span>

2. <span data-ttu-id="e9379-148">V Průzkumníku objektů, klikněte pravým tlačítkem na databázi hello chcete tooadd brány firewall pro pravidlo a klikněte na tlačítko **nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="e9379-148">In Object Explorer, right-click on hello database you want tooadd a firewall rule for and click **New Query**.</span></span> <span data-ttu-id="e9379-149">Otevře okno prázdné dotazu který je připojený tooyour databáze.</span><span class="sxs-lookup"><span data-stu-id="e9379-149">A blank query window opens that is connected tooyour database.</span></span>

3. <span data-ttu-id="e9379-150">V okně dotazu hello upravit hello IP adresu tooyour veřejnou IP adresu a potom spusťte následující dotaz hello:</span><span class="sxs-lookup"><span data-stu-id="e9379-150">In hello query window, modify hello IP address tooyour public IP address and then execute hello following query:</span></span>

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. <span data-ttu-id="e9379-151">Na panelu nástrojů hello, klikněte na tlačítko **Execute** pravidlo brány firewall toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-151">On hello toolbar, click **Execute** toocreate hello firewall rule.</span></span>

## <a name="view-how-tooconnect-an-application-tooyour-database-using-a-secure-connection-string"></a><span data-ttu-id="e9379-152">Zobrazit, jak tooconnect aplikaci tooyour databáze pomocí zabezpečené připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="e9379-152">View how tooconnect an application tooyour database using a secure connection string</span></span>

<span data-ttu-id="e9379-153">tooensure zabezpečené, šifrované připojení mezi klientská aplikace a SQL Database, hello připojovací řetězec má toobe nakonfigurovaný tak, aby:</span><span class="sxs-lookup"><span data-stu-id="e9379-153">tooensure a secure, encrypted connection between a client application and SQL Database, hello connection string has toobe configured to:</span></span>

- <span data-ttu-id="e9379-154">Žádosti o šifrované připojení, a</span><span class="sxs-lookup"><span data-stu-id="e9379-154">Request an encrypted connection, and</span></span>
- <span data-ttu-id="e9379-155">certifikát serveru hello toonot vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="e9379-155">toonot trust hello server certificate.</span></span> 

<span data-ttu-id="e9379-156">Tím se naváže připojení pomocí zabezpečení TLS (Transport Layer) a snižuje riziko hello útokům man-in-the-middle.</span><span class="sxs-lookup"><span data-stu-id="e9379-156">This establishes a connection using Transport Layer Security (TLS) and reduces hello risk of man-in-the-middle attacks.</span></span> <span data-ttu-id="e9379-157">Můžete získat správně nakonfigurované připojovací řetězce pro vaši databázi SQL pro podporované klientské ovladače z portálu Azure hello jak je vidět na tomto snímku obrazovky pro technologii ADO.net.</span><span class="sxs-lookup"><span data-stu-id="e9379-157">You can obtain correctly configured connection strings for your SQL Database for supported client drivers from hello Azure portal as shown for ADO.net in this screenshot.</span></span>

1. <span data-ttu-id="e9379-158">Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="e9379-158">Select **SQL databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span>

2. <span data-ttu-id="e9379-159">Na hello **přehled** stránky pro vaši databázi, klikněte na tlačítko **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="e9379-159">On hello **Overview** page for your database, click **Show database connection strings**.</span></span>

3. <span data-ttu-id="e9379-160">Zkontrolujte hello dokončení **ADO.NET** připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="e9379-160">Review hello complete **ADO.NET** connection string.</span></span>

    ![Připojovací řetězec pro ADO.NET](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a><span data-ttu-id="e9379-162">Vytváření uživatelů databáze</span><span class="sxs-lookup"><span data-stu-id="e9379-162">Creating database users</span></span>

<span data-ttu-id="e9379-163">Před vytvořením všechny uživatele, musíte nejprve zvolit jednu z Azure SQL Database podporuje dva typy ověřování:</span><span class="sxs-lookup"><span data-stu-id="e9379-163">Before creating any users, you must first choose from one of two authentication types supported by Azure SQL Database:</span></span> 

<span data-ttu-id="e9379-164">**Ověřování SQL**, který používá uživatelské jméno a heslo pro přihlášení a uživatele, kteří jsou platné pouze v hello kontextu konkrétní databáze v rámci logického serveru.</span><span class="sxs-lookup"><span data-stu-id="e9379-164">**SQL Authentication**, which uses username and password for logins and users that are valid only in hello context of a specific database within a logical server.</span></span> 

<span data-ttu-id="e9379-165">**Ověřování služby Azure Active Directory**, identity, které jsou spravované službou Azure Active Directory, který používá.</span><span class="sxs-lookup"><span data-stu-id="e9379-165">**Azure Active Directory Authentication**, which uses identities managed by Azure Active Directory.</span></span> 

<span data-ttu-id="e9379-166">Pokud chcete, aby toouse [Azure Active Directory](./sql-database-aad-authentication.md) tooauthenticate proti databázi SQL, vyplněná Azure Active Directory musí existovat, abyste mohli pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e9379-166">If you want toouse [Azure Active Directory](./sql-database-aad-authentication.md) tooauthenticate against SQL Database, a populated Azure Active Directory must exist before you can proceed.</span></span>

<span data-ttu-id="e9379-167">Postupujte podle těchto kroků toocreate uživatele pomocí ověřování SQL:</span><span class="sxs-lookup"><span data-stu-id="e9379-167">Follow these steps toocreate a user using SQL Authentication:</span></span>

1. <span data-ttu-id="e9379-168">Připojit databáze tooyour, například pomocí [SQL Server Management Studio](./sql-database-connect-query-ssms.md) pomocí svých přihlašovacích údajů správce serveru.</span><span class="sxs-lookup"><span data-stu-id="e9379-168">Connect tooyour database, for example using [SQL Server Management Studio](./sql-database-connect-query-ssms.md) using your server admin credentials.</span></span>

2. <span data-ttu-id="e9379-169">V Průzkumníku objektů, klikněte pravým tlačítkem na databázi hello tooadd nového uživatele na a klikněte na tlačítko **nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="e9379-169">In Object Explorer, right-click on hello database you want tooadd a new user on and click **New Query**.</span></span> <span data-ttu-id="e9379-170">Otevře okno prázdné dotazu který je připojený toohello vybrané databáze.</span><span class="sxs-lookup"><span data-stu-id="e9379-170">A blank query window opens that is connected toohello selected database.</span></span>

3. <span data-ttu-id="e9379-171">V okně dotazu hello zadejte hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="e9379-171">In hello query window, enter hello following query:</span></span>

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. <span data-ttu-id="e9379-172">Na panelu nástrojů hello, klikněte na tlačítko **Execute** toocreate hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="e9379-172">On hello toolbar, click **Execute** toocreate hello user.</span></span>

5. <span data-ttu-id="e9379-173">Ve výchozím nastavení hello uživatel může připojit toohello databáze, ale neobsahuje žádná oprávnění tooread nebo zapisovat data.</span><span class="sxs-lookup"><span data-stu-id="e9379-173">By default, hello user can connect toohello database, but has no permissions tooread or write data.</span></span> <span data-ttu-id="e9379-174">toogrant těchto oprávnění toohello nově vytvořený uživatel, spustit hello následující dva příkazy v novém okně dotazu</span><span class="sxs-lookup"><span data-stu-id="e9379-174">toogrant these permissions toohello newly created user, execute hello following two commands in a new query window</span></span>

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

<span data-ttu-id="e9379-175">Je nejlepší postup toocreate těchto účtů bez oprávnění správce v hello databáze úrovně tooconnect tooyour databáze, pokud potřebujete tooexecute správce úkoly, jako je vytváření nových uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e9379-175">It is best practice toocreate these non-administrator accounts at hello database level tooconnect tooyour database unless you need tooexecute administrator tasks like creating new users.</span></span> <span data-ttu-id="e9379-176">Zkontrolujte hello [kurz služby Azure Active Directory](./sql-database-aad-authentication-configure.md) o tooauthenticate pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e9379-176">Please review hello [Azure Active Directory tutorial](./sql-database-aad-authentication-configure.md) on how tooauthenticate using Azure Active Directory.</span></span>


## <a name="protect-your-data-with-encryption"></a><span data-ttu-id="e9379-177">Chraňte svá data pomocí šifrování</span><span class="sxs-lookup"><span data-stu-id="e9379-177">Protect your data with encryption</span></span>

<span data-ttu-id="e9379-178">Azure SQL Database transparentní šifrování dat (šifrování TDE) automaticky šifruje vaše data v klidu, bez nutnosti jakékoli změny toohello aplikace přístup k hello šifrované databázi.</span><span class="sxs-lookup"><span data-stu-id="e9379-178">Azure SQL Database transparent data encryption (TDE) automatically encrypts your data at rest, without requiring any changes toohello application accessing hello encrypted database.</span></span> <span data-ttu-id="e9379-179">Pro nově vytvořené databáze je ve výchozím nastavení šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="e9379-179">For newly created databases, TDE is on by default.</span></span> <span data-ttu-id="e9379-180">tooenable TDE pro databáze nebo tooverify, kterých je na serveru, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e9379-180">tooenable TDE for your database or tooverify that TDE is on, follow these steps:</span></span>

1. <span data-ttu-id="e9379-181">Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="e9379-181">Select **SQL databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 

2. <span data-ttu-id="e9379-182">Klikněte na **transparentní šifrování dat** tooopen hello konfigurační stránku pro šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="e9379-182">Click on **Transparent data encryption** tooopen hello configuration page for TDE.</span></span>

    ![Transparentní šifrování dat](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. <span data-ttu-id="e9379-184">V případě potřeby nastavte **šifrování dat** tooON a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e9379-184">If necessary, set **Data encryption** tooON and click **Save**.</span></span>

<span data-ttu-id="e9379-185">Spustí se proces šifrování Hello hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="e9379-185">hello encryption process starts in hello background.</span></span> <span data-ttu-id="e9379-186">Můžete sledovat průběh hello pomocí připojování tooSQL databáze [SQL Server Management Studio](./sql-database-connect-query-ssms.md) dotazováním hello encryption_state sloupec hello `sys.dm_database_encryption_keys` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e9379-186">You can monitor hello progress by connecting tooSQL Database using [SQL Server Management Studio](./sql-database-connect-query-ssms.md) by querying hello encryption_state column of hello `sys.dm_database_encryption_keys` view.</span></span>

## <a name="enable-sql-database-auditing-if-necessary"></a><span data-ttu-id="e9379-187">Povolit auditování databáze SQL, v případě potřeby</span><span class="sxs-lookup"><span data-stu-id="e9379-187">Enable SQL Database auditing, if necessary</span></span>

<span data-ttu-id="e9379-188">Auditování databáze SQL Azure sleduje události databáze a zapisuje je protokol auditování tooan ve vašem účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e9379-188">Azure SQL Database Auditing tracks database events and writes them tooan audit log in your Azure Storage account.</span></span> <span data-ttu-id="e9379-189">Auditování vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou případné porušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e9379-189">Auditing can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate potential security violations.</span></span> <span data-ttu-id="e9379-190">Postupujte podle těchto kroků toocreate výchozí zásady auditování pro databázi SQL:</span><span class="sxs-lookup"><span data-stu-id="e9379-190">Follow these steps toocreate a default auditing policy for your SQL database:</span></span>

1. <span data-ttu-id="e9379-191">Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="e9379-191">Select **SQL databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 

2. <span data-ttu-id="e9379-192">V okně Nastavení hello, vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="e9379-192">In hello Settings blade, select **Auditing & Threat Detection**.</span></span> <span data-ttu-id="e9379-193">Všimněte si, že auditování na úrovni serveru je diabled a zda je **zobrazit nastavení serveru** odkaz, který vám umožní tooview nebo změna nastavení auditování serveru hello z tohoto kontextu.</span><span class="sxs-lookup"><span data-stu-id="e9379-193">Notice that sever-level auditing is diabled and that there is a **View server settings** link that allows you tooview or modify hello server auditing settings from this context.</span></span>

    ![Auditování okno](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. <span data-ttu-id="e9379-195">Pokud dáváte přednost tooenable auditu typu (nebo umístění?) liší od hello uvedené na úrovni serveru hello, zapněte **ON** auditování a zvolte hello **Blob** typu auditování.</span><span class="sxs-lookup"><span data-stu-id="e9379-195">If you prefer tooenable an Audit type (or location?) different from hello one specified at hello server level, turn **ON** Auditing, and choose hello **Blob** Auditing Type.</span></span> <span data-ttu-id="e9379-196">Pokud je auditování objektů Blob serveru je povoleno, budou existovat audit databáze nakonfigurované hello node souběžně s auditování objektů Blob server hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-196">If server Blob auditing is enabled, hello database configured audit will exist side by side with hello server Blob audit.</span></span>

    ![Zapnout auditování](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. <span data-ttu-id="e9379-198">Vyberte **podrobnosti úložiště** tooopen hello okno úložiště protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="e9379-198">Select **Storage Details** tooopen hello Audit Logs Storage Blade.</span></span> <span data-ttu-id="e9379-199">Účet úložiště Azure vyberte hello uložení protokoly a dobu uchování hello, po které hello se odstraní staré protokoly, pak klikněte na **OK** dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-199">Select hello Azure storage account where logs will be saved, and hello retention period, after which hello old logs will be deleted, then click **OK** at hello bottom.</span></span> 

   > [!TIP]
   > <span data-ttu-id="e9379-200">Použití hello stejný účet úložiště pro všechny tooget hello auditování databází naplno hello auditování sestavy šablony.</span><span class="sxs-lookup"><span data-stu-id="e9379-200">Use hello same storage account for all audited databases tooget hello most out of hello auditing reports templates.</span></span>
   > 

5. <span data-ttu-id="e9379-201">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e9379-201">Click **Save**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e9379-202">Pokud chcete toocustomize hello auditovat události, můžete k tomu pomocí prostředí PowerShell nebo rozhraní REST API - najdete v části hello [automatizace (PowerShell / REST API)](sql-database-auditing.md#subheading-7) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e9379-202">If you want toocustomize hello audited events, you can do this via PowerShell or REST API - see hello [Automation (PowerShell / REST API)](sql-database-auditing.md#subheading-7) section for more details.</span></span>
>

## <a name="enable-sql-database-threat-detection"></a><span data-ttu-id="e9379-203">Povolení detekce hrozeb databáze SQL</span><span class="sxs-lookup"><span data-stu-id="e9379-203">Enable SQL Database threat detection</span></span>

<span data-ttu-id="e9379-204">Detekce hrozeb poskytuje novou vrstvu zabezpečení, což umožňuje zákazníkům toodetect a toopotential hrozeb reagovat, když k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity.</span><span class="sxs-lookup"><span data-stu-id="e9379-204">Threat Detection provides a new layer of security, which enables customers toodetect and respond toopotential threats as they occur by providing security alerts on anomalous activities.</span></span> <span data-ttu-id="e9379-205">Uživatele můžete prozkoumat hello podezřelé události pomocí toodetermine auditování databáze SQL, pokud budou výsledkem pokus o tooaccess, porušení nebo využívat data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-205">Users can explore hello suspicious events using SQL Database Auditing toodetermine if they result from an attempt tooaccess, breach or exploit data in hello database.</span></span> <span data-ttu-id="e9379-206">Detekce hrozeb je jednoduchý tooaddress potenciální hrozby toohello databáze bez hello nutné toobe expert zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.</span><span class="sxs-lookup"><span data-stu-id="e9379-206">Threat Detection makes it simple tooaddress potential threats toohello database without hello need toobe a security expert or manage advanced security monitoring systems.</span></span>
<span data-ttu-id="e9379-207">Například detekce hrozeb zjistila určité nezvyklé databázové aktivity, které indikují potenciální pokusu o Injektáž SQL.</span><span class="sxs-lookup"><span data-stu-id="e9379-207">For example, Threat Detection detects certain anomalous database activities indicating potential SQL injection attempts.</span></span> <span data-ttu-id="e9379-208">Injektáž SQL je jedním z hello běžné webové aplikace problémy se zabezpečením na Internetu, použít tooattack datové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-208">SQL injection is one of hello common Web application security issues on hello Internet, used tooattack data-driven applications.</span></span> <span data-ttu-id="e9379-209">Útočníci využít výhod aplikace ohrožení zabezpečení tooinject škodlivý příkazů SQL do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-209">Attackers take advantage of application vulnerabilities tooinject malicious SQL statements into application entry fields, for breaching or modifying data in hello database.</span></span>

1. <span data-ttu-id="e9379-210">Přejděte okno Konfigurace toohello hello chcete toomonitor databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="e9379-210">Navigate toohello configuration blade of hello SQL database you want toomonitor.</span></span> <span data-ttu-id="e9379-211">V okně Nastavení hello, vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="e9379-211">In hello Settings blade, select **Auditing & Threat Detection**.</span></span>

    ![Navigační podokno](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. <span data-ttu-id="e9379-213">V hello **auditování a detekce hrozeb** zapnout okno Konfigurace **ON** auditování, které se zobrazí nastavení detekce hrozby hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-213">In hello **Auditing & Threat Detection** configuration blade turn **ON** auditing, which will display hello threat detection settings.</span></span>

3. <span data-ttu-id="e9379-214">Zapnout **ON** detekce hrozby.</span><span class="sxs-lookup"><span data-stu-id="e9379-214">Turn **ON** threat detection.</span></span>

4. <span data-ttu-id="e9379-215">Konfigurace seznamu hello e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklé databázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="e9379-215">Configure hello list of emails that will receive security alerts upon detection of anomalous database activities.</span></span>

5. <span data-ttu-id="e9379-216">Klikněte na tlačítko **Uložit** v hello **auditování a detekce hrozeb** okno toosave hello nové nebo aktualizované auditování a hrozeb zásady detekce.</span><span class="sxs-lookup"><span data-stu-id="e9379-216">Click **Save** in hello **Auditing & Threat detection** blade toosave hello new or updated auditing and threat detection policy.</span></span>

    ![Navigační podokno](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    <span data-ttu-id="e9379-218">Pokud se zjistila nezvyklé databázové aktivity, obdržíte e-mail s oznámením při zjištění nezvyklé databázové aktivity.</span><span class="sxs-lookup"><span data-stu-id="e9379-218">If anomalous database activities are detected, you will receive an email notification upon detection of anomalous database activities.</span></span> <span data-ttu-id="e9379-219">e-mailu Hello se poskytují informace o událostí hello podezřelé zabezpečení včetně hello povaha hello neobvyklé aktivity, název databáze, čas serveru název a hello události.</span><span class="sxs-lookup"><span data-stu-id="e9379-219">hello email will provide information on hello suspicious security event including hello nature of hello anomalous activities, database name, server name and hello event time.</span></span> <span data-ttu-id="e9379-220">Kromě toho se budou poskytují informace o možných příčin a doporučená akce tooinvestigate a zmírnit hello potenciální hrozby toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="e9379-220">In addition, it will provide information on possible causes and recommended actions tooinvestigate and mitigate hello potential threat toohello database.</span></span> <span data-ttu-id="e9379-221">Hello další kroky procházení měli byste prostřednictvím co toodo budete dostávat e-mailu:</span><span class="sxs-lookup"><span data-stu-id="e9379-221">hello next steps walk you through what toodo should you receive such an email:</span></span>

    ![E-mailu detekce hrozeb](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. <span data-ttu-id="e9379-223">V e-mailu hello, klikněte na hello **protokolu auditování databáze SQL Azure** odkaz, který bude spusťte hello portál Azure a zobrazit relevantní auditování záznamy hello době hello hello podezřelé události.</span><span class="sxs-lookup"><span data-stu-id="e9379-223">In hello email, click on hello **Azure SQL Auditing Log** link, which will launch hello Azure portal and show hello relevant auditing records around hello time of hello suspicious event.</span></span>

    ![Záznamy auditu](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. <span data-ttu-id="e9379-225">Klikněte na tooview záznamy auditu hello další podrobnosti o hello databáze podezřelé aktivity, například příkazy SQL, selhání důvod a klient IP.</span><span class="sxs-lookup"><span data-stu-id="e9379-225">Click on hello audit records tooview more details on hello suspicious database activities such as SQL statement, failure reason and client IP.</span></span>

    ![Podrobnosti záznamu](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. <span data-ttu-id="e9379-227">V okně hello auditování záznamy, klikněte na tlačítko **otevřít v aplikaci Excel** tooimport šablony a spuštění hlubší analýzu protokolů auditu hello době hello hello podezřelé události v aplikaci excel tooopen předem nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="e9379-227">In hello Auditing Records blade, click  **Open in Excel** tooopen a pre-configured excel template tooimport and run deeper analysis of hello audit log around hello time of hello suspicious event.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e9379-228">V aplikaci Excel 2010 nebo novější, Power Query a hello **rychle zkombinovat** nastavení je povinné.</span><span class="sxs-lookup"><span data-stu-id="e9379-228">In Excel 2010 or later, Power Query and hello **Fast Combine** setting is required.</span></span>

    ![Otevřené záznamy v aplikaci Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. <span data-ttu-id="e9379-230">tooconfigure hello **rychle zkombinovat** nastavení - hello **POWER QUERY** pásu karet vyberte **možnosti** dialogovém okně Možnosti toodisplay hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-230">tooconfigure hello **Fast Combine** setting - In hello **POWER QUERY** ribbon tab, select **Options** toodisplay hello Options dialog.</span></span> <span data-ttu-id="e9379-231">Vyberte část hello ochrany osobních údajů a zvolte hello druhá možnost - 'Ignorovat hello úrovně ochrany osobních údajů a potenciálně tak vylepšit výkon':</span><span class="sxs-lookup"><span data-stu-id="e9379-231">Select hello Privacy section and choose hello second option - 'Ignore hello Privacy Levels and potentially improve performance':</span></span>

    ![Kombinování rychlé aplikace Excel](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. <span data-ttu-id="e9379-233">protokoly auditu tooload SQL, zajistěte, aby hello parametry v kartě nastavení hello jsou správně nastavena a poté vyberte pásu karet "Data" hello a klikněte na tlačítko "Aktualizovat všechny" hello.</span><span class="sxs-lookup"><span data-stu-id="e9379-233">tooload SQL audit logs, ensure that hello parameters in hello settings tab are set correctly and then select hello 'Data' ribbon and click hello 'Refresh All' button.</span></span>

    ![Parametry aplikace Excel](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. <span data-ttu-id="e9379-235">Hello výsledky se zobrazí v hello **protokoly auditu SQL** listu, která vám umožní toorun hlubší analýzu hello neobvyklé aktivity, které byly zjištěny a zmírnit dopad hello hello událostí zabezpečení ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e9379-235">hello results appear in hello **SQL Audit Logs** sheet which enables you toorun deeper analysis of hello anomalous activities that were detected, and mitigate hello impact of hello security event in your application.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e9379-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9379-236">Next steps</span></span>
<span data-ttu-id="e9379-237">Hello ochranu databáze před uživateli se zlými úmysly a neoprávněným přístupem pomocí několika jednoduchých kroků můžete zvýšit.</span><span class="sxs-lookup"><span data-stu-id="e9379-237">You can improve hello protection of your database against malicious users or unauthorized access with just a few simple steps.</span></span> <span data-ttu-id="e9379-238">V tomto kurzu jste postup:</span><span class="sxs-lookup"><span data-stu-id="e9379-238">In this tutorial you learn to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="e9379-239">Nastavte pravidla firewallu pro databázi serveru a nebo</span><span class="sxs-lookup"><span data-stu-id="e9379-239">Set up firewall rules for your sever and or database</span></span>
> * <span data-ttu-id="e9379-240">Připojit databáze tooyour pomocí zabezpečené připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="e9379-240">Connect tooyour database using a secure connection string</span></span>
> * <span data-ttu-id="e9379-241">Spravovat přístup uživatelů</span><span class="sxs-lookup"><span data-stu-id="e9379-241">Manage user access</span></span>
> * <span data-ttu-id="e9379-242">Chraňte svá data pomocí šifrování</span><span class="sxs-lookup"><span data-stu-id="e9379-242">Protect your data with encryption</span></span>
> * <span data-ttu-id="e9379-243">Povolit auditování databáze SQL</span><span class="sxs-lookup"><span data-stu-id="e9379-243">Enable SQL Database auditing</span></span>
> * <span data-ttu-id="e9379-244">Povolení detekce hrozeb databáze SQL</span><span class="sxs-lookup"><span data-stu-id="e9379-244">Enable SQL Database threat detection</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="e9379-245">Vylepšení výkonu SQL Database</span><span class="sxs-lookup"><span data-stu-id="e9379-245">Improve SQL Database performance</span></span>](sql-database-performance-tutorial.md)

