---
title: "aaaAzure Přehled zabezpečení databáze SQL | Microsoft Docs"
description: "Další informace o zabezpečení Azure SQL Database a SQL Server, včetně hello rozdíly mezi hello cloudové a místní SQL Server, pokud jde tooauthentication, autorizace, zabezpečení připojení, šifrování a dodržování předpisů."
services: sql-database
documentationcenter: 
author: tmullaney
manager: jhubbard
editor: 
ms.assetid: a012bb85-7fb4-4fde-a2fc-cf426c0a56bb
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/05/2017
ms.author: thmullan;jackr
ms.openlocfilehash: 7b0b0d1b59ec4018d9fb668bc8b6b56c9907e982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-your-sql-database"></a>Zabezpečení SQL Database

Tento článek vás provede základy hello zabezpečení datové vrstvy hello aplikace pomocí Azure SQL Database. Tento článek vám především pomůže začít s prostředky pro ochranu dat, řízením přístupu a proaktivním monitorováním. 

Úplný přehled funkcí zabezpečení, které jsou k dispozici na všech typů SQL, najdete v části hello [pro databázový stroj systému SQL Server a databázi SQL Azure Security Center](https://msdn.microsoft.com/library/bb510589). Další informace jsou také k dispozici v hello [zabezpečení a Azure SQL Database technické dokumentu white paper](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="protect-data"></a>Ochrana dat
Služba SQL Database chrání vaše data zajištěním šifrování přenášených dat pomocí [protokolu TLS (Transport Layer Security)](https://support.microsoft.com/kb/3135244), neaktivních uložených dat pomocí [transparentního šifrování dat](http://go.microsoft.com/fwlink/?LinkId=526242) a používaných dat pomocí funkce [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx). 

> [!IMPORTANT]
>Všechny tooAzure připojení databáze SQL vyžadovat šifrování (SSL/TLS) na všechny časy dat při tooand "při přenosu" z databáze hello. V připojovacím řetězci vaší aplikace, musíte zadat parametry tooencrypt hello připojení a *není* tootrust hello certifikát serveru (to se provádí pro můžete při kopírování připojovacího řetězce z hello portálu Azure Classic), v opačném případě hello připojení nebude ověřit identitu hello hello serveru a bude náchylný příliš "man-in-the-middle" útoky. Pro ovladač technologie ADO.NET hello, například tyto řetězce jsou parametry připojení **šifrovat = True** a **TrustServerCertificate = False**. 

Pro jiné způsoby tooencrypt data, vezměte v úvahu:

* [Šifrování na úrovni buněk](https://msdn.microsoft.com/library/ms179331.aspx) tooencrypt určité sloupce nebo i buňky dat s různými šifrovacími klíči.
* Pokud potřebujete modul hardwarového zabezpečení nebo chcete centrálně spravovat hierarchii šifrovacích klíčů, měli byste uvažovat o [řešení Azure Key Vault s SQL Serverem na virtuálním počítači v Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).

## <a name="control-access"></a>Řízení přístupu
Databáze SQL slouží k zabezpečení dat omezením přístupu k databázi tooyour pomocí pravidel brány firewall, ověřovací mechanismy, které by uživatelé tooprove své identity a autorizace toodata prostřednictvím členství na základě rolí a oprávnění, a také prostřednictvím zabezpečení na úrovni řádků a maskování dynamická data. Informace o použití hello funkce řízení přístupu v databázi SQL, najdete v části [řízení přístupu](sql-database-control-access.md).

> [!IMPORTANT]
> Správa databází a logických serverů v Azure se řídí tím, jaké role má uživatelský účet na portálu přiřazené. Další informace k tomuto tématu najdete v článku o [řízení přístupu podle rolí na portálu Azure Portal](../active-directory/role-based-access-control-what-is.md).
>

### <a name="firewall-and-firewall-rules"></a>Brána firewall a pravidla brány firewall
toohelp chránit vaše data, brány firewall zabránit všechny přístup tooyour databázový server, dokud nezadáte, které počítače mají oprávnění prostřednictvím [pravidla brány firewall](sql-database-firewall-configure.md). brány firewall Hello uděluje přístup toodatabases podle hello pocházející IP adresu každého požadavku.

### <a name="authentication"></a>Authentication
Ověřování databáze SQL odkazuje toohow prokázání své identity při připojování toohello databáze. SQL Database podporuje dva typy ověřování:

* **Ověřování SQL,** které používá uživatelské jméno a heslo. Když vytvoříte hello logický server pro databázi, jste zadali "serveru admin" přihlášení s uživatelské jméno a heslo. Pomocí těchto přihlašovacích údajů, můžete ověřovat tooany databáze na tomto serveru jako vlastník databáze hello nebo "dbo." 
* **Ověřování pomocí Azure Active Directory,** které používá identity spravované v Azure Active Directory a je podporované u spravovaných a integrovaných domén. [Kdykoliv to půjde](https://msdn.microsoft.com/library/ms144284.aspx), použijte ověřování pomocí Active Directory (integrované zabezpečení). Pokud chcete toouse ověřování Azure Active Directory, musíte vytvořit jiný správce serveru názvem hello "Azure AD admin", který je povolen tooadminister Azure AD Uživatelé a skupiny. Tento správce také smí provádět všechny operace jako běžný správce serveru. V tématu [připojení tooSQL databáze pomocí pomocí Azure Active Directory Authentication](sql-database-aad-authentication.md) pro návod, jak toocreate tooenable Správce služby Azure AD ověřování Azure Active Directory.

### <a name="authorization"></a>Autorizace
Autorizace odkazuje toowhat může uživatel provádět v rámci Azure SQL Database, a to je řízené podle členství role databáze váš uživatelský účet a objekt úroveň oprávnění. Jako osvědčený postup byste měli udělit uživatelům hello nejnižší oprávnění potřebná. Hello účet správce serveru, ke kterému se připojujete se členem role db_owner, který má autority toodo nic v rámci hello databáze. Tento účet uložte kvůli nasazení upgradovaných schémat a dalším možnostem správy. Použít účet "ApplicationUser" hello s omezenější tooconnect oprávnění z vaší aplikace toohello databáze pomocí hello nejnižších oprávnění vyžadované vaší aplikace.

### <a name="row-level-security"></a>Zabezpečení na úrovni řádku
Zabezpečení na úrovni řádků umožňuje zákazníkům toocontrol přístup toorows v tabulce databáze na základě charakteristik hello hello uživatele provádění dotazu (například skupinu členství nebo provádění kontextu). Další informace najdete v tématu [Zabezpečení na úrovni řádku](https://msdn.microsoft.com/library/dn765131).

### <a name="data-masking"></a>Maskování dat 
Databáze SQL dynamická data maskování omezuje ohrožení citlivých dat pomocí maskování ho toonon privilegovaných uživatelů. Dynamické maskování automaticky dat zjistí potenciálně citlivá data v databázi SQL Azure a poskytuje řešitelné doporučení toomask těchto polí s minimálním dopadem na aplikační vrstvu hello. Funguje díky zmatení přes pole určené databáze, zatímco hello data v databázi hello se nezmění data hello citlivá data v hello sadu výsledků dotazu. Další informace najdete v tématu [Začínáme s SQL Database dynamická data maskování](sql-database-dynamic-data-masking-get-started.md) lze použít toolimit ohrožení citlivých dat.

## <a name="proactive-monitoring"></a>Proaktivní monitorování
Služba SQL Database chrání vaše data poskytováním možností auditování a detekce hrozeb. 

### <a name="auditing"></a>Auditování
Auditování databáze SQL sleduje databázové aktivity a pomůže vám toomaintain dodržování předpisů, zaznamenáním protokol auditu tooan databáze události ve vašem účtu úložiště Azure. Auditování vám umožní toounderstand probíhající databázové aktivity, jakož i analyzovat a historickou činnost tooidentify potenciální hrozby nebo možného zneužití a zabezpečení porušení. Další informace najdete v tématu [Začínáme s auditováním služby SQL Database](sql-database-auditing.md).  

### <a name="threat-detection"></a>Detekce hrozeb
Detekce hrozeb doplňuje auditování, tím, že poskytuje další vrstvu zabezpečení intelligence integrovaný do služby Azure SQL Database hello, který zjistí pokusy o neobvyklou a potenciálně škodlivé tooaccess nebo zneužití databází. O podezřelých aktivit, potenciální ohrožení zabezpečení a prostřednictvím injektáže SQL, a vzory přístupu k nezvyklé databázové, zobrazí se výstraha. Můžete prohlížet výstrah o zjištěných hrozbách [Azure Security Center](https://azure.microsoft.com/services/security-center/) a zadejte podrobnosti podezřelých aktivit a doporučujeme akce jak tooinvestigate a zmírnit hrozby hello. Detekce hrozeb stojí $15, počet serverů za měsíc. Bude zdarma hello prvních 60 dní. Další informace najdete v článku [Začínáme zjišťovat hrozby ve službě SQL Database](sql-database-threat-detection.md).
 
### <a name="data-masking"></a>Maskování dat 
Databáze SQL dynamická data maskování omezuje ohrožení citlivých dat pomocí maskování ho toonon privilegovaných uživatelů. Dynamické maskování automaticky dat zjistí potenciálně citlivá data v databázi SQL Azure a poskytuje řešitelné doporučení toomask těchto polí s minimálním dopadem na aplikační vrstvu hello. Funguje díky zmatení přes pole určené databáze, zatímco hello data v databázi hello se nezmění data hello citlivá data v hello sadu výsledků dotazu. Další informace najdete v tématu Začínáme s [maskování dynamická data databáze SQL](sql-database-dynamic-data-masking-get-started.md)
 
## <a name="compliance"></a>Dodržování předpisů
Kromě toohello výše funkcí, které může pomoci aplikace splňovat různé požadavky na zabezpečení, Azure SQL Database také účastní pravidelné audity a certifikovala proti počet standardů dodržování předpisů. Další informace najdete v tématu hello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/), kde můžete najít hello aktuální seznam [dodržování předpisů certifikátů databáze SQL](https://azure.microsoft.com/support/trust-center/services/).

## <a name="next-steps"></a>Další kroky

- Informace o použití hello funkce řízení přístupu v databázi SQL, najdete v části [řízení přístupu](sql-database-control-access.md).
- Informace o auditování databáze, najdete v části [auditování databáze SQL](sql-database-auditing.md).
- Informace o detekce hrozeb, najdete v části [detekce hrozeb SQL Database](sql-database-threat-detection.md).
