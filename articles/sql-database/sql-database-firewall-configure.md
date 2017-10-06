---
title: "pravidla brány firewall aaaAzure databáze SQL | Microsoft Docs"
description: "Zjistěte, jak tooconfigure SQL databáze brány firewall s přístupem toomanage pravidla brány firewall úrovni serveru a na úrovni databáze."
keywords: "brána firewall databáze"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: ac57f84c-35c3-4975-9903-241c8059011e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 04/10/2017
ms.author: rickbyh
ms.openlocfilehash: 6a8cdf629d0d0e55421a5e9f9b894a21371be568
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-server-level-and-database-level-firewall-rules"></a>Azure pravidla brány firewall serveru úroveň a databáze SQL Database 

Microsoft Azure SQL Database poskytuje relační databázovou službu pro aplikace Azure a další internetové aplikace. toohelp chránit vaše data, brány firewall zabránit všechny přístup tooyour databázový server, dokud nezadáte, které počítače mají oprávnění. brány firewall Hello uděluje přístup toodatabases podle hello pocházející IP adresu každého požadavku.

## <a name="overview"></a>Přehled

Na začátku všechny Transact-SQL přístup tooyour Azure SQL server je blokována bránou hello firewall. toobegin pomocí serveru Azure SQL je nutné zadat jedno nebo více pravidel brány firewall na úrovni serveru, které umožňují přístup tooyour Azure SQL server. Použijte toospecify pravidla brány firewall hello rozsahy IP adresu, která z hello Internetu jsou povolené a zda aplikací Azure se mohou pokusit tooconnect tooyour Azure SQL server.

toojust přístup udělit tooselectively mezi hello databází na vašem serveru Azure SQL, musíte vytvořit pravidlo úroveň databáze pro hello požadovaná databáze. Zadejte rozsah IP adres pro pravidlo firewallu hello databáze, které je nad rámec hello zadaná v pravidle brány firewall na úrovni serveru hello rozsah adres IP a ujistěte se, že hello IP adresa klienta hello spadá v rozsahu hello zadaná v pravidle hello úroveň databáze.

Hello pokusy o připojení z Internetu a Azure musí nejdřív projít přes bránu firewall hello než dosáhnou Azure SQL serveru nebo v databázi SQL, jak je znázorněno v následujícím diagramu hello:

   ![Diagram popisující konfiguraci brány firewall.][1]

* **Pravidla brány firewall na úrovni serveru:** tato pravidla povolit klienty tooaccess celý server Azure SQL, který je hello všechny hello databází v rámci stejného logického serveru. Tato pravidla jsou uložené v hello **hlavní** databáze. Pravidla brány firewall na úrovni serveru se dá nakonfigurovat pomocí hello portálu nebo pomocí příkazů Transact-SQL. pravidla brány firewall na úrovni serveru toocreate pomocí hello portál Azure nebo prostředí PowerShell, musí být Přispěvatel předplatné nebo vlastníka předplatného hello. toocreate pravidlo brány firewall na úrovni serveru pomocí jazyka Transact-SQL, je nutné připojit toohello instanci služby SQL Database jako hlavní přihlášení na úrovni serveru hello nebo hello správce Azure Active Directory, (což znamená, že pravidlo brány firewall na úrovni serveru musí být nejprve vytvořen uživatel s oprávnění na úrovni Azure).
* **Pravidla brány firewall na úrovni databáze:** tato pravidla povolit klienty tooaccess určité (zabezpečení) databáze v rámci hello stejný logický server. Můžete vytvořit tato pravidla pro každou databázi (včetně hello **hlavní** database0) a jsou uložené v jednotlivých databázích hello. Pravidla brány firewall na úrovni databáze se dá nakonfigurovat jenom pomocí příkazů Transact-SQL a až po nakonfigurování hello první brány firewall na úrovni serveru. Pokud zadáte rozsah IP adres v pravidle brány firewall na úrovni databáze hello, který je zadaný v pravidle brány firewall na úrovni serveru hello hello mimo rozsah, můžete přístup pouze klienti, které mají IP adresy v rozsahu úroveň databáze hello hello databázi. Pro jednu databázi můžete mít maximálně 128 pravidel brány firewall na úrovni databáze. Pravidla brány firewall na úrovni databáze pro hlavní a uživatele databáze lze pouze vytvořit a spravovat prostřednictvím jazyka Transact-SQL. Další informace o konfiguraci pravidla brány firewall na úrovni databáze najdete v příkladu hello později v tomto článku a v tématu [sp_set_database_firewall_rule (databáze SQL Azure)](https://msdn.microsoft.com/library/dn270010.aspx).

**Doporučení:** společnost Microsoft doporučuje používat pravidla brány firewall na úrovni databáze vždy, když možné tooenhance zabezpečení a toomake víc přenosného databáze. Pomocí pravidel brány firewall na úrovni serveru pro správce a až budete mít mnoho databází, které mají hello stejné požadavky na přístup a nechcete toospend čas konfigurace jednotlivě každou databázi.

> [!Note]
> Informace o přenosné databáze v kontextu hello kontinuity podnikových procesů najdete v tématu [požadavky na ověřování pro zotavení po havárii](sql-database-geo-replication-security-config.md).
>

### <a name="connecting-from-hello-internet"></a>Připojení z Internetu hello

Když se počítač pokusí tooconnect tooyour databázového serveru z hello Internetu, brána firewall hello nejprve zkontroluje hello pocházející IP adresu požadavku hello vůči pravidla brány firewall na úrovni databáze hello hello databáze, který vyžaduje připojení hello:

* Pokud IP adresa hello hello požadavku je do jedné z hello rozsahů určených v pravidlech brány firewall na úrovni databáze hello, je hello připojení udělen toohello SQL Database, která obsahuje pravidlo hello.
* Pokud IP adresa hello hello požadavku není do jedné z hello rozsahů určených v hello pravidlo brány firewall na úrovni databáze, se kontroluje hello pravidla brány firewall na úrovni serveru. Pokud IP adresa hello hello požadavku je do jedné z hello rozsahů určených v hello pravidla brány firewall na úrovni serveru, jsou udělena hello připojení. Pravidla brány firewall na úrovni serveru použít tooall SQL databáze na serveru Azure SQL hello.  
* Pokud IP adresa hello hello požadavku není v rámci hello rozsahy zadanému v některém z hello úroveň databáze nebo pravidla brány firewall na úrovni serveru, hello žádost o připojení selže.

> [!NOTE]
> tooaccess databáze SQL Azure z místního počítače, ujistěte se, že brána firewall hello na vaší sítí a místním počítači umožňuje odchozí komunikaci na portu TCP 1433.
> 

### <a name="connecting-from-azure"></a>Připojení z Azure
tooallow aplikace z Azure tooconnect tooyour serveru Azure SQL, Azure připojení musí být povolena. Když se aplikace z Azure pokusí tooconnect tooyour databázový server, brány firewall hello ověřuje, zda je povolen Azure připojení. Brána firewall nastavení s počáteční a koncovou adresu rovnající too0.0.0.0 označuje, že tato připojení jsou povoleny. Pokud pokus o připojení hello není povolena, nedorazily hello požadavku server Azure SQL Database hello.

> [!IMPORTANT]
> Tato možnost nakonfiguruje hello brány firewall tooallow všechna připojení z Azure včetně připojení z hello odběry jiných zákazníků. Když vyberete tuto možnost, zajistěte, aby vaše přihlášení a oprávnění uživatele omezit tooonly přístup oprávněným uživatelům.
> 

## <a name="creating-and-managing-firewall-rules"></a>Vytváření a správu pravidel brány firewall
Hello první nastavení brány firewall na úrovni serveru můžete vytvořit pomocí hello [portál Azure](https://portal.azure.com/) nebo programově pomocí [prostředí Azure PowerShell](https://msdn.microsoft.com/library/azure/dn546724.aspx), [rozhraní příkazového řádku Azure](/cli/azure/sql/server/firewall-rule#create), nebo hello [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn505712.aspx). Další pravidla brány firewall na úrovni serveru můžete vytvářet a spravovat těmito způsoby nebo prostřednictvím jazyka Transact-SQL. 

> [!IMPORTANT]
> Pravidla brány firewall na úrovni databáze mohou být vytvořeny pouze a spravovat pomocí jazyka Transact-SQL. 
>

tooimprove výkonu, brány firewall na úrovni serveru, který pravidla jsou dočasně uloženy v mezipaměti na úrovni databáze hello. mezipaměť hello toorefresh, najdete v části [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). 

> [!TIP]
> Můžete použít [auditování databáze SQL](sql-database-auditing.md) změny tooaudit úrovni serveru a na úrovni databáze brány firewall.
>

### <a name="azure-portal"></a>portál Azure

tooset pravidla brány firewall na úrovni serveru v hello portálu Azure, můžete buď přejít toohello Přehled stránky pro stránku Azure SQL database nebo hello přehled logického serveru Azure databáze.

> [!TIP]
> Podívejte se kurz [vytvořit databáze pomocí portálu Azure hello](sql-database-get-started-portal.md).
>

**Na stránce Přehled databáze**

1. Klikněte na tlačítko tooset pravidlo brány firewall na úrovni serveru na stránce Přehled hello databáze, **nastavení brány firewall serveru** na hello nástrojů, jak ukazuje následující obrázek hello: hello **nastavení brány Firewall** stránku hello SQL Otevře se databázový server.

      ![pravidlo brány firewall serveru](./media/sql-database-get-started-portal/server-firewall-rule.png) 

2. Klikněte na tlačítko **přidat IP adresu klienta** na hello nástrojů tooadd hello IP adresu počítače hello aktuálně používáte a potom klikněte na **Uložit**. Vytvoří se pravidlo brány firewall na úrovni serveru pro vaši aktuální IP adresu.

      ![nastavení pravidla brány firewall serveru](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

**Na stránce Přehled serveru**

Hello stránku přehled pro váš server otevře zobrazující text hello plně kvalifikovaný název serveru (například **mynewserver20170403.database.windows.net**) a poskytuje možnosti pro další konfiguraci.

1. tooset pravidlo úroveň serveru na stránce Přehled serveru, klikněte na tlačítko **brány Firewall** v levé nabídce hello v části nastavení, jak je zřejmé hello následující bitové kopie: 

     ![Přehled logický server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Klikněte na tlačítko **přidat IP adresu klienta** na hello nástrojů tooadd hello IP adresu počítače hello aktuálně používáte a potom klikněte na **Uložit**. Vytvoří se pravidlo brány firewall na úrovni serveru pro vaši aktuální IP adresu.

     ![nastavení pravidla brány firewall serveru](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

### <a name="transact-sql"></a>Transact-SQL
| Zobrazení katalogu nebo uložená procedura | Úroveň | Popis |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Server |Zobrazí aktuální pravidla brány firewall na úrovni serveru hello |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Server |Vytvoří nebo aktualizuje pravidla brány firewall na úrovni serveru. |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Server |Odebere pravidla brány firewall na úrovni serveru. |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Databáze |Zobrazí aktuální pravidla brány firewall na úrovni databáze hello |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Databáze |Vytvoří nebo aktualizuje pravidla brány firewall na úrovni databáze hello |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Databáze |Odebere pravidla brány firewall na úrovni databáze. |


Hello následující příklady zkontrolujte hello existující pravidla, povolit rozsah IP adres na serveru hello Contoso a odstraní pravidlo brány firewall:
   
```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```
  
Potom přidejte pravidlo brány firewall.
   
```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

toodelete pravidlo brány firewall na úrovni serveru, provedení hello sp_delete_firewall_rule uložené procedury. Hello následující příklad odstraní hello pravidlo s názvem ContosoFirewallRule:
   
```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```   

### <a name="azure-powershell"></a>Azure PowerShell
| Rutina | Úroveň | Popis |
| --- | --- | --- |
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) |Server |Vrátí aktuální pravidla brány firewall na úrovni serveru hello |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) |Server |Vytvoří nové pravidlo brány firewall na úrovni serveru |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) |Server |Aktualizace vlastnosti hello existující pravidlo brány firewall na úrovni serveru |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) |Server |Odebere pravidla brány firewall na úrovni serveru. |


Následující ukázka Hello Nastaví pravidlo brány firewall na úrovni serveru pomocí prostředí PowerShell:

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> Prostředí PowerShell příklady v kontextu hello rychle začít, najdete v části [vytvořit DB - PowerShell](sql-database-get-started-powershell.md) a [vytvářet izolované databáze a nakonfigurujte pravidlo brány firewall pomocí prostředí PowerShell](scripts/sql-database-create-and-configure-database-powershell.md)
>

### <a name="azure-cli"></a>Azure CLI
| Rutina | Úroveň | Popis |
| --- | --- | --- |
| [Vytvoření brány firewall az sql serveru](/cli/azure/sql/server/firewall-rule#create) | Vytvoří brány firewall pravidla tooallow přístup tooall databází SQL na serveru hello z hello zadaný rozsah IP adres.|
| [odstranění brány firewall serveru sql az](/cli/azure/sql/server/firewall-rule#delete)| Odstraní pravidlo brány firewall.|
| [seznam az sql serverů brány firewall](/cli/azure/sql/server/firewall-rule#list)| Obsahuje seznam pravidel brány firewall hello.|
| [pravidlo brány firewall serveru sql az zobrazit](/cli/azure/sql/server/firewall-rule#show)| Zobrazí podrobnosti o hello pravidla brány firewall.|
| [AX aktualizace pravidla brány firewall serveru sql](/cli/azure/sql/server/firewall-rule#update)| Aktualizuje pravidlo brány firewall.

Následující ukázka Hello Nastaví pravidlo brány firewall na úrovni serveru pomocí rozhraní příkazového řádku Azure hello: 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Příklad rozhraní příkazového řádku Azure v kontextu hello rychle začít, najdete v části [vytvořit odpis - rozhraní příkazového řádku Azure](sql-database-get-started-cli.md) a [vytvářet izolované databáze a nakonfigurujte pravidlo brány firewall pomocí hello rozhraní příkazového řádku Azure](scripts/sql-database-create-and-configure-database-cli.md)
>

### <a name="rest-api"></a>REST API
| Rozhraní API | Úroveň | Popis |
| --- | --- | --- |
| [Výpis pravidel brány firewall](https://msdn.microsoft.com/library/azure/dn505715.aspx) |Server |Zobrazí aktuální pravidla brány firewall na úrovni serveru hello |
| [Vytvoření pravidla brány firewall](https://msdn.microsoft.com/library/azure/dn505712.aspx) |Server |Vytvoří nebo aktualizuje pravidla brány firewall na úrovni serveru. |
| [Nastavení pravidla brány firewall](https://msdn.microsoft.com/library/azure/dn505707.aspx) |Server |Aktualizace vlastnosti hello existující pravidlo brány firewall na úrovni serveru |
| [Odstranění pravidla brány firewall](https://msdn.microsoft.com/library/azure/dn505706.aspx) |Server |Odebere pravidla brány firewall na úrovni serveru. |

## <a name="server-level-firewall-rule-versus-a-database-level-firewall-rule"></a>Pravidlo brány firewall na úrovni serveru a pravidla brány firewall na úrovni databáze
OTÁZKY. Musí být uživatelé jednu databázi plně izolovaném z jiné databáze?   
  Pokud ano, udělte přístup pomocí pravidel brány firewall na úrovni databáze. Tím je zabráněno pomocí pravidla brány firewall na úrovni serveru, která umožňují přístup přes bránu firewall hello tooall databáze, snižuje hello hloubku svoji ochranu.   
 
OTÁZKY. Uživatelů na adrese IP hello potřebujete přístup tooall databáze?   
  Pomocí brány firewall na úrovni serveru pravidla tooreduce hello počet, kolikrát je nutné nakonfigurovat pravidla brány firewall.   

OTÁZKY. Hello osobě nebo týmu konfigurace pravidel brány firewall hello pouze má přístup prostřednictvím hello portál Azure, prostředí PowerShell nebo rozhraní REST API hello?   
  Je nutné použít pravidla brány firewall na úrovni serveru. Pravidla brány firewall na úrovni databáze se dá nakonfigurovat jenom pomocí jazyka Transact-SQL.  

OTÁZKY. Je hello osobě nebo týmu konfigurace pravidel brány firewall hello zakázáno s vysoké úrovně oprávnění na úrovni databáze hello?   
  Pomocí pravidel brány firewall na úrovni serveru. Konfigurace pravidel brány firewall na úrovni databáze pomocí jazyka Transact-SQL, vyžaduje alespoň `CONTROL DATABASE` oprávnění na úrovni databáze hello.  

OTÁZKY. Je hello osobě nebo týmu konfigurace nebo auditování hello pravidla brány firewall, centrálně spravovat pravidla brány firewall pro mnoho (možná 100s) z databáze?   
  Tento výběr závisí na konkrétních potřeb a prostředí. Pravidla brány firewall na úrovni serveru může být snazší tooconfigure, ale skriptování můžete nakonfigurovat pravidla, na úrovni databáze hello. I když používáte pravidla brány firewall na úrovni serveru, pokud může být nutné pravidel firewallu databáze hello, tooaudit, toosee uživatelé s `CONTROL` oprávnění v databázi hello vytvořili pravidla brány firewall na úrovni databáze.   

OTÁZKY. Můžete použít kombinaci obou pravidla brány firewall úrovni serveru a na úrovni databáze?   
  Ano. Někteří uživatelé, třeba správci, může být nutné pravidla brány firewall na úrovni serveru. Jiných uživatelů, jako jsou uživatelé databázovou aplikaci, může být nutné pravidla brány firewall na úrovni databáze.   

## <a name="troubleshooting-hello-database-firewall"></a>Řešení potíží s hello databáze brány firewall
Mějte na paměti následující body při přístupu k toohello služby Microsoft Azure SQL Database tak, jak očekáváte, že hello:

* **Konfigurace místní brány firewall:** počítače mohli dostat k Azure SQL Database, musíte toocreate výjimku brány firewall v počítači pro TCP port 1433. Pokud provádíte připojení uvnitř hranic hello cloudu Azure, můžete mít tooopen další porty. Další informace najdete v tématu hello **SQL Database: mimo vs uvnitř** části [porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
* **Překlad síťových adres (NAT):** splatnosti tooNAT, hello IP adresu používanou službou tooAzure tooconnect vašeho počítače SQL Database může být jiný než hello IP adresy uvedené v nastavení konfigurace IP počítače. tooview hello IP adres je počítač pomocí tooconnect tooAzure, přihlaste se toohello portál a přejděte toohello **konfigurace** karta na hello serveru, který je hostitelem databáze. V části hello **povolené IP adresy** části hello **aktuální IP adresa klienta** se zobrazí. Klikněte na tlačítko **přidat** toohello **povolené IP adresy** tooallow tento server hello tooaccess počítače.
* **Seznam povolených změny toohello ještě nevstoupilo v platnost:** může být co nejvíce pět minut zpoždění pro změny toohello Azure SQL Database brány firewall konfigurace tootake vliv.
* **Hello přihlášení nemá oprávnění nebo nesprávné heslo byl použit:** Pokud přihlášení nemá oprávnění na serveru Azure SQL Database hello nebo je chybné heslo hello používá, hello připojení toohello Azure SQL Database serveru byl odepřen. Vytvoření nastavení brány firewall pouze poskytuje klientům tooattempt možnost připojení serveru tooyour; Každý klient musí poskytnout hello potřebná zabezpečovací pověření. Další informace o přípravě přihlášení najdete v tématu Správa databází, přihlášení a uživatelů ve službě Azure SQL Database.
* **Dynamickou IP adresu:** Pokud máte připojení k Internetu pomocí dynamické IP adresy a máte potíže získávání přes bránu firewall hello, mohou zkuste jednu z následujících řešení hello:
  
  * Zeptejte se, že poskytovatele služeb Internetu (ISP) pro rozsah IP adres hello přiřadili tooyour klientské počítače tohoto serveru Azure SQL Database hello přístup a poté přidejte hello rozsah IP adres jako pravidlo brány firewall.
  * Získat statické IP adresy místo pro klientské počítače a poté přidejte hello IP adresy jako pravidla brány firewall.

## <a name="next-steps"></a>Další kroky

- Rychlý start na vytváření databáze a pravidlo brány firewall na úrovni serveru, najdete v části [vytvoření Azure SQL database](sql-database-get-started-portal.md).
- Nápovědu v databázi Azure SQL připojování tooan z s otevřeným zdrojem nebo aplikacích třetích stran najdete v tématu [úvodní kód klienta ukázky tooSQL databáze](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Informace o další porty, když musíte tooopen, najdete v části hello **SQL Database: mimo vs uvnitř** části [porty nad rámec 1433 pro technologii ADO.NET 4.5 a databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md)
- Přehled zabezpečení Azure SQL Database, najdete v části [zabezpečení databáze](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
