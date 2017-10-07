---
title: "aaaOverview zabezpečení v Data Lake Store | Microsoft Docs"
description: "Pochopit, jak Azure Data Lake Store je bezpečnější úložiště velkých objemů dat"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 69e20bd046a9427202074baf59bff13f5b6a83ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-in-azure-data-lake-store"></a>Zabezpečení v Azure Data Lake Store
Mnoho podniků jsou využívat výhod analýzy velkých objemů dat pro obchodní Statistika toohelp, které je provést inteligentní rozhodnutí. Organizace může mít složitý a regulovaná prostředí s roste počet různých uživatelů. Je důležité pro enterprise toomake zda bezpečněji, uložení kritickými podnikovými daty s hello správné úrovně tooindividual uživatelům udělen přístup. Azure Data Lake Store je navržený tak toohelp splňovat tyto požadavky na zabezpečení. V tomto článku se dozvíte o funkcích zabezpečení hello Data Lake Store, včetně:

* Authentication
* Autorizace
* Izolace sítě
* Ochrana dat
* Auditování

## <a name="authentication-and-identity-management"></a>Ověřování a identita správy
Ověřování je proces hello, podle kterého je ověřit identitu uživatele, když hello uživatel pracuje s Data Lake Store nebo s jakoukoli službu, která se připojuje tooData Lake Store. Pro správu identit a ověření Data Lake Store využívá [Azure Active Directory](../active-directory/active-directory-whatis.md), komplexní přístupu a identit a správy cloudové řešení, které usnadňuje správu hello uživatelů a skupin.

Každé předplatné Azure může být přidružen k instanci služby Azure Active Directory. Pouze uživatelé a identita služby, které jsou definované ve službě Azure Active Directory můžete přístup k účtu Data Lake Store pomocí hello portál Azure, nástroje příkazového řádku, nebo prostřednictvím klientské aplikace, které vaše organizace sestavení pomocí hello Azure Data Lake Store SDK. Klíčové výhody použití služby Azure Active Directory jako mechanismus řízení centralizované přístupu jsou:

* Zjednodušená správa životního cyklu identity. Hello identity uživatele nebo služby (hlavní identity služby) můžete rychle vytvořit a rychle odvolán jednoduše odstranit nebo zakázat účet hello v adresáři hello.
* Služba Multi-Factor authentication. [Služba Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication.md) poskytuje další úroveň zabezpečení pro uživatelská přihlášení a transakce.
* Ověřování z libovolného klienta přes standardní otevřete protokol, jako je například účtu OAuth nebo OpenID.
* Federaci se enterprise adresářových služeb a zprostředkovatelů identity cloudu.

## <a name="authorization-and-access-control"></a>Autorizace a řízení přístupu
Po Azure Active Directory ověřuje uživatele, aby hello uživatel získat přístup k Azure Data Lake Store, ovládací prvky autorizace přístupová oprávnění pro Data Lake Store. Data Lake Store odděluje autorizace aktivity souvisejícím s účtem a souvisejících s daty v hello následujícím způsobem:

* [Řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) (RBAC) poskytovaný platformou Azure pro správu účtu
* POSIX seznamu ACL pro přístup k datům v úložišti hello

### <a name="rbac-for-account-management"></a>RBAC pro správu účtu
Čtyři základní role jsou definovány pro Data Lake Store ve výchozím nastavení. role Hello povolit různé operace v účtu Data Lake Store prostřednictvím hello portálu Azure, PowerShell a rozhraní REST API. Hello vlastníka a role Přispěvatel provádět celou řadu funkcí správy na účet hello. Můžete přiřadit role toousers čtečky hello, kdo pouze interagovat s daty.

![Role RBAC](./media/data-lake-store-security-overview/rbac-roles.png "role RBAC")

Všimněte si, i když role přiřazené pro správu účtu, některé role ovlivnit toodata přístup. Je nutné toouse toooperations přístup toocontrol seznamy ACL, který může uživatel provádět v systému souborů hello. Hello následující tabulka obsahuje souhrn práva pro správu a data přístupová práva pro hello výchozí role.

| Role | Práva pro správu | Data přístupová práva | Vysvětlení |
| --- | --- | --- | --- |
| Žádné přiřazenou roli |Žádný |Řídí seznamu ACL |Hello uživatel nemůže používat hello Azure portal nebo Azure PowerShell rutiny toobrowse Data Lake Store. Hello uživatel může použít jenom nástroje příkazového řádku. |
| Vlastník |Všechny |Všechny |role vlastník Hello je superuživatele. Tato role můžou spravovat všechno a má úplný přístup toodata. |
| Čtenář |jen pro čtení |Řídí seznamu ACL |role čtenáře Hello můžou zobrazit všechno týkající se správy účtů, například u kterých je uživatel přiřazenou roli toowhich. role čtenáře Hello nelze provést žádné změny. |
| Přispěvatel |Všechny kromě přidání a odebrání rolí |Řídí seznamu ACL |role Přispěvatel Hello můžete spravovat některé aspekty účtu, jako je například nasazení a vytváření a Správa výstrah. role Přispěvatel Hello nelze přidat nebo odebrat role. |
| Správce přístupu uživatelů |Přidání a odebrání rolí |Řídí seznamu ACL |role správce přístupu uživatelů Hello můžete spravovat přístup tooaccounts uživatele. |

Pokyny najdete v tématu [přiřadit uživatele nebo skupiny zabezpečení účtů tooData Lake Store](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Pomocí seznamů řízení přístupu pro operace v systémech souborů.
Data Lake Store je systém souborů hierarchické jako Hadoop Distributed File System (HDFS) a podporuje [seznamy ACL POSIX](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Ovládá pro čtení (r), zápis (w) a spouštět (tooresources oprávnění pro roli vlastníka hello hello vlastníků skupiny a pro ostatní uživatele a skupiny x). Seznamy ACL v hello Data Lake Store Public Preview (hello aktuální verzi), může být povolena na hello kořenová složka, podsložky a jednotlivé soubory. Další informace o fungování seznamů řízení přístupu v souvislosti s Data Lake Storem najdete v tématu [Řízení přístupu v Data Lake Storu](data-lake-store-access-control.md).

Doporučujeme, abyste definovat seznamy ACL pro více uživatelů pomocí [skupiny zabezpečení](../active-directory/active-directory-accessmanagement-manage-groups.md). Přidat skupinu zabezpečení tooa uživatelů a potom přiřadit hello seznamy ACL pro skupinu zabezpečení toothat souboru nebo složky. To je užitečné při chcete tooprovide vlastní přístup, protože jsou omezené tooadding maximálně devět položek pro vlastní přístup. Další informace o tom, jak toobetter zabezpečení dat uložených v Data Lake Store pomocí skupin zabezpečení služby Azure Active Directory najdete v tématu [přiřadit uživatele nebo skupiny zabezpečení jako seznamy ACL toohello systém souborů Azure Data Lake Store](data-lake-store-secure-data.md#filepermissions).

![Standardní a vlastní přístup](./media/data-lake-store-security-overview/adl.acl.2.png "standardní a vlastní přístup")

## <a name="network-isolation"></a>Izolace sítě
Použití Data Lake Store toohelp řízení přístupu tooyour datové úložiště na úrovni sítě hello. Můžete určit brány firewall a definovat rozsah IP adres pro klienty důvěryhodné. Rozsah IP adres umožňuje připojení pouze klienti, kteří mají IP adresu v rozsahu hello definované tooData Lake Store.

![Nastavení brány firewall a IP přístup](./media/data-lake-store-security-overview/firewall-ip-access.png "nastavení a IP adresu brány Firewall")

## <a name="data-protection"></a>Ochrana dat
Azure Data Lake Store chrání vaše data v průběhu jejich životního cyklu. Pro data během přenosu Data Lake Store využívá hello standardní zabezpečení TLS (Transport Layer) protokol toosecure data přes síť hello.

![Šifrování v Data Lake Store](./media/data-lake-store-security-overview/adls-encryption.png "šifrování v Data Lake Store")

Data Lake Store taky zajišťuje šifrování dat, který je uložený v účtu hello. Můžete pokusit toohave vaše data zašifrovaná nebo zvolit žádné šifrování. Pokud můžete vyjádřit výslovný souhlas pro šifrování, data uložená v Data Lake Store je šifrovaný předchozí toostoring na trvalé médiu. V takovém případě Data Lake Store automaticky předchozí toopersisting data šifruje a dešifruje předchozí tooretrieval data, takže je zcela transparentní toohello klientský přístup k datům hello. Neexistuje žádná změna kódu vyžaduje na data tooencrypt/dešifrování hello na straně klienta.

Pro správu klíčů Data Lake Store poskytuje dva režimy pro správu hlavní šifrovacích klíčů (MEKs), které jsou požadovány pro dešifrování žádná data, která je uložená v hello Data Lake Store. Můžete je nechat buď Data Lake Store můžete spravovat hello MEKs, nebo zvolte tooretain vlastnictví hello MEKs pomocí účtu Azure Key Vault. Když při vytvoření účtu Data Lake Store zadáte hello režimu správy klíčů. Další informace o tom, tooprovide konfigurace související se šifrování, najdete v části [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md).

## <a name="auditing-and-diagnostic-logs"></a>Protokoly auditování a diagnostiky
Můžete použít protokoly auditování a diagnostiky, v závislosti na tom, jestli hledáte protokoly pro týkajících se správy aktivit nebo aktivit souvisejících s daty.

* Aktivity související s správy pomocí rozhraní API Správce Azure Resource Manager a jsou prezentované v hello portál Azure přes protokoly auditu.
* Aktivity související s data pomocí rozhraní REST API WebHDFS a jsou prezentované v hello portál Azure prostřednictvím diagnostické protokoly.

### <a name="auditing-logs"></a>Protokoly auditování
toocomply s předpisy, organizace může vyžadovat záznamy odpovídající auditu, pokud potřebuje toodig na konkrétní incidenty. Data Lake Store má integrované sledování a auditování a protokoluje všechny aktivity správy účtu.

Účet správy pro záznamy pro audit, zobrazení a zvolte hello sloupce, které chcete toolog. Můžete také exportovat tooAzure protokoly auditu úložiště.

![Protokoly auditu](./media/data-lake-store-security-overview/audit-logs.png "Protokoly auditu")

### <a name="diagnostic-logs"></a>Diagnostické protokoly
Můžete nastavit záznamy auditu přístupu k datům v hello portálu Azure (v nastavení pro diagnostiku) a vytvoření účtu úložiště objektů Blob v Azure, kde jsou uloženy protokoly hello.

![Diagnostické protokoly](./media/data-lake-store-security-overview/diagnostic-logs.png "diagnostické protokoly")

Po konfiguraci nastavení pro diagnostiku hello protokoly můžete zobrazit na hello **diagnostické protokoly** kartě.

Další informace o práci s diagnostické protokoly s Azure Data Lake Store najdete v tématu [přístupu k diagnostickým protokolům pro Data Lake Store](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Souhrn
Podnikoví zákazníci potřebují cloudové platformy analýzy dat, která je toouse zabezpečení a snadné. Azure Data Lake Store je navržený tak toohelp adres tyto požadavky přes správu identit a ověření pomocí integrace služby Azure Active Directory, ověření na základě seznamu ACL, izolace sítě, šifrování dat při přenosu i v klidu (k dispozici v hello budoucí) a auditování.

Pokud chcete toosee nové funkce v Data Lake Store, pošlete nám svůj názor v hello [Data Lake Store UserVoice fórum](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
* [Začínáme s Data Lake Store](data-lake-store-get-started-portal.md)
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)

