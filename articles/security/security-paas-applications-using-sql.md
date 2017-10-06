---
title: "aaaSecuring databáze PaaS v Azure | Microsoft Docs"
description: " Další informace o zabezpečení Azure SQL Database a SQL Data Warehouse osvědčené postupy pro zabezpečení vašich PaaS webové a mobilní aplikace. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: terrylan
ms.openlocfilehash: a7f9a2846b59dcb05e82f2ad88b41c5213295603
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-databases-in-azure"></a>Zabezpečení databáze PaaS v Azure

V tomto článku probereme kolekce [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) a [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/) osvědčené postupy zabezpečení pro zabezpečení vašich PaaS webové a mobilní aplikace. Tyto doporučené postupy jsou odvozeny od našich zkušeností s Azure a hello prostředí zákazníků, jako sami.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Azure SQL Database a SQL Data Warehouse
[Azure SQL Database](../sql-database/sql-database-technical-overview.md) a [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) zadejte služba relační databáze pro aplikace založené na Internetu. Podívejme se na služby, které pomáhají chránit vaše aplikace a data při použití Azure SQL Database a SQL Data Warehouse v nasazení PaaS:

- Ověřování Azure Active Directory (namísto ověřování serveru SQL Server)
- Brány firewall pro Azure SQL
- Transparentní šifrování dat (šifrování TDE)

## <a name="best-practices"></a>Osvědčené postupy

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Používání úložiště v centrální identity pro ověřování a autorizaci

Azure SQL databáze může být nakonfigurované toouse jeden ze dvou typů ověřování:

- **Ověřování SQL** používá uživatelské jméno a heslo. Když vytvoříte hello logický server pro databázi, jste zadali "serveru admin" přihlášení s uživatelské jméno a heslo. Pomocí těchto přihlašovacích údajů, můžete ověřovat tooany databáze na tomto serveru jako vlastník databáze hello.

- **Ověřování služby Azure Active Directory** používá identity spravované službou Azure Active Directory a je podporována pro spravované a integrované domén. toouse ověřování Azure Active Directory, musíte vytvořit jiný správce serveru názvem hello "Azure AD admin", který je povolen tooadminister Azure AD Uživatelé a skupiny. Tento správce také smí provádět všechny operace jako běžný správce serveru.

[Ověřování Azure Active Directory](../active-directory/develop/active-directory-authentication-scenarios.md) mechanismus připojující tooAzure SQL Database a SQL Data Warehouse pomocí identit v Azure Active Directory (AD). Azure AD poskytuje alternativní tooSQL ověření serveru, můžete zastavit hello, jak narůstá počet uživatelských identit mezi servery databáze. Azure AD authentication umožňuje toocentrally můžete spravovat identity hello databáze uživatelů a další služby Microsoftu v jednom centrálním místě. Centrální správa ID umožňuje uživatelům toomanage databáze na jednom místě a zjednodušuje správu oprávnění.  

Výhody používání ověřování Azure AD místo ověřování SQL patří:

- Umožňuje otočení heslo na jednom místě.
- Spravuje oprávnění databáze pomocí externí Azure skupiny AD.
- Eliminuje ukládání hesel povolením integrované ověřování systému Windows a jiných forem ověřování podporovaných službou Azure AD.
- Používá obsažené databáze uživatelé tooauthenticate identity na úrovni databáze hello.
- Podporuje ověřování na základě tokenu pro připojování tooSQL databáze aplikace.
- Podporuje služby AD FS (federation domény) nebo ověřování nativní uživatele a heslo pro místní služby Azure AD bez synchronizace domény.
- Podporuje připojení z SQL Server Management Studio, které používají Universal ověřování služby Active Directory, která zahrnuje [Multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md). Vícefaktorové ověřování zahrnuje silné ověřování s celou řadu možností snadno ověření – telefonní hovor, textová zpráva, čipové karty s PIN kód nebo oznámení mobilní aplikace. Další informace najdete v tématu [SSMS podpora pro Azure AD MFA s SQL Database a SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

toolearn Další informace o ověřování Azure AD, najdete v části:

- [Připojení tooSQL databáze nebo SQL Data Warehouse pomocí pomocí Azure ověřování Active Directory](../sql-database/sql-database-aad-authentication.md)
- [TooAzure ověřování SQL datového skladu](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Podpora ověřování na základě tokenu pro databázi Azure SQL pomocí ověřování Azure AD](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> tooensure, že Azure Active Directory je vhodné pro vaše prostředí, najdete v části [funkce Azure AD a omezení](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), konkrétně hello další aspekty.
>
>

### <a name="restrict-access-based-on-ip-address"></a>Omezení přístupu na základě IP adresy
Můžete vytvořit pravidla brány firewall, které zadejte rozsahy IP adres, přijatelný. Tato pravidla je možné cílit na hello server a úrovně databáze. Doporučujeme používat pravidla brány firewall na úrovni databáze vždy, když možné tooenhance zabezpečení a toomake víc přenosného databáze. Pravidla brány firewall na úrovni serveru jsou nejvhodnější pro správce a až budete mít mnoho databází, které mají hello stejné požadavky na přístup, ale nechcete, aby čas toospend konfigurace jednotlivě každou databázi.

Omezení podle IP adresy SQL Database výchozí zdroj povolit přístup z libovolné Azure adresy (včetně jiných předplatných a klienti). Můžete omezit tento tooonly povolit IP adresy tooaccess hello instanci. I přes brány firewall pro SQL a omezení podle IP adresy je stále nutné silné ověřování. V tématu hello doporučení, která dříve v tomto článku.

toolearn Další informace o omezení IP adres a brány Firewall SQL Azure najdete v části:

- [Řízení přístupu Azure SQL Database](../sql-database/sql-database-control-access.md)
- [Konfigurace pravidel brány firewall databáze Azure SQL Database – přehled](../sql-database/sql-database-firewall-configure.md)
- [Konfigurace pravidla brány firewall na úrovni serveru Azure SQL Database pomocí portálu Azure hello](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>Šifrování neaktivních uložených dat
[Transparentní šifrování šifrování dat (TDE)](https://msdn.microsoft.com/library/azure/bb934049) je ve výchozím nastavení povolené. Šifrování TDE transparentně šifruje SQL Server, databáze SQL Azure a Azure SQL Data Warehouse data a soubory protokolu. Šifrování TDE chrání před útoky přímý přístup toohello souborů nebo jejich zálohování. To umožňuje tooencrypt dat v klidovém stavu beze změny stávajících aplikací. Šifrování TDE by měla zůstat vždy povoleno; To však neukončí útočník pomocí hello normální přístup k cestě. Šifrování TDE poskytuje možnost toocomply hello s mnoha právní a pokyny uvedenými v různých odvětví.

Azure SQL spravuje o problémech souvisejících s klíče pro šifrování TDE. Jak se šifrování TDE, místní musí dát zvláštní pozor tooensure obnovitelnost a při přesunu databází. Ve složitějších scénářích hello klíče se dají explicitně spravovat Azure Key Vault prostřednictvím ekm (viz [povolit šifrování TDE na SQL serveru pomocí EKM](/security/encryption/enable-tde-on-sql-server-using-ekm)). To také umožňuje prostřednictvím funkce Azure klíč trezory BYOK pro přineste si vlastní klíč (BYOK).

Azure SQL zajišťuje šifrování pro sloupce prostřednictvím [vždy šifrována](/sql/relational-databases/security/encryption/always-encrypted-database-engine). To umožňuje přístup pouze autorizovaných aplikací toosensitive sloupce. Tento druh šifrování pomocí omezuje dotazy SQL pro hodnoty na základě tooequality šifrované sloupce.

Šifrování na úrovni aplikace by měla být používána pro vybrané údaje. Otázky suverenity dat. můžete se někdy zmírnit šifrování dat s klíčem, který je uložen v hello správnou zemi. Přenos dat i náhodných zabrání způsobující problém, protože je to možné toodecrypt hello, data bez hello klíče, za předpokladu, že silné algoritmus se používá (například AES 256).

Můžete vytvořit další bezpečnostní opatření toohelp zabezpečené hello databáze například návrhu zabezpečení systému, šifrování důvěrné prostředky a vytváření brány firewall kolem hello databázové servery.

## <a name="next-steps"></a>Další kroky
Tento článek se zavedl jste tooa kolekce SQL Database a SQL Data Warehouse osvědčené postupy zabezpečení pro zabezpečení vašich PaaS webové a mobilní aplikace. Další informace o zabezpečení vašeho nasazení PaaS toolearn najdete v části:

- [Zabezpečení nasazení PaaS](security-paas-deployments.md)
- [Zabezpečení PaaS webové a mobilní aplikace pomocí Azure App Services](security-paas-applications-using-app-services.md)
