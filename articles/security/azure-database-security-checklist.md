---
title: "kontrolní seznam zabezpečení databáze aaaAzure | Microsoft Docs"
description: "Tento článek obsahuje sadu kontrolní seznam pro zabezpečení databáze Azure."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: tomsh
ms.openlocfilehash: 5e3a69591df3c8508a8707a2d47068342863ce93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-security-checklist"></a>Kontrolní seznam zabezpečení Azure databáze

toohelp zvýšit zabezpečení, databáze Azure zahrnují řadu ovládacích prvků integrované zabezpečení, můžete použít toolimit a řízení přístupu.

Mezi ně patří:

-   Brány firewall, která vám umožní toocreate [pravidla brány firewall](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) omezení připojení pomocí IP adresy
-   Server brány firewall na úrovni přístupné z hello portálu Azure
-   Pravidla brány firewall na úrovni databáze přístupné z aplikace SSMS
-   Databáze tooyour zabezpečené připojení pomocí zabezpečené připojovací řetězce
-   Použijte správu přístupu
-   Šifrování dat
-   Auditování databáze SQL
-   Detekce hrozeb databáze SQL

## <a name="introduction"></a>Úvod
Cloud computing vyžaduje nové vzorů zabezpečení, které jsou obeznámeni toomany aplikace uživatelé, správci databází a programátory v jazyce. Některé organizace v důsledku toho jsou odhodlání tooimplement cloudové infrastruktury pro správu dat z důvodu tooperceived bezpečnostní rizika. Velká část tuto situaci však můžete zmírnit prostřednictvím lépe porozumět hello zabezpečení funkcí integrovaných do Microsoft Azure a Microsoft Azure SQL Database.

## <a name="checklist"></a>Kontrolní seznam
Doporučujeme, abyste si přečetli hello [osvědčené postupy zabezpečení databáze Azure](https://docs.microsoft.com/en-us/azure/security/azure-database-security-best-practices) článek předchozí tooreviewing Tento kontrolní seznam. Až porozumíte hello osvědčené postupy, bude se mít tooget hello naplno Tento kontrolní seznam. Pak můžete použít tento kontrolní seznam toomake se, že jsme vyřešili hello důležité problémy v zabezpečení databáze Azure.


|Kontrolní seznam kategorie| Popis|
| ------------ | -------- |
|**Ochrana dat**||
| <br> Šifrování během pohybu nebo přenosu| <ul><li>[Transport Layer Security](https://docs.microsoft.com/en-us/windows-server/security/tls/transport-layer-security-protocol), pro šifrování dat, když se data přenášejí toohello sítě.</li><li>Databáze vyžaduje zabezpečenou komunikaci od klientů podle hello [TDS (Tabular Data Stream)](https://msdn.microsoft.com/en-in/library/dd357628.aspx) protokol přes protokol TLS (Transport Layer Security).</li></ul> |
|<br>Šifrování v klidovém stavu| <ul><li>[Transparentní šifrování dat](http://go.microsoft.com/fwlink/?LinkId=526242), je-li neaktivní data fyzicky uložena v jakékoli digitální podobě.</li></ul>|
|**Řízení přístupu**||  
|<br> Přístup k databázi | <ul><li>[Ověřování](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-control-access) (Azure Active Directory Authentication) AD ověřování pomocí identity spravované službou Azure Active Directory.</li><li>[Autorizace](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-control-access) udělit uživatelům hello nejnižší oprávnění potřebná.</li></ul> |
|<br>Přístup k aplikaci| <ul><li>[Řádek úroveň zabezpečení](https://msdn.microsoft.com/library/dn765131) (pomocí zásad zabezpečení, na stejnou dobu omezení přístupu na úrovni řádků na základě uživatele identit, role nebo provádění kontextu hello).</li><li>[Dynamické maskování dat](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-dynamic-data-masking-get-started) (pomocí oprávnění & zásady, omezuje zranitelnost citlivá data pomocí maskování ho toonon privilegovaných uživatelů)</li></ul>|
|**Proaktivní monitorování**||  
| <br>Sledování & zjišťování| <ul><li>[Auditování](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing) sleduje události databáze a zapisuje je protokol auditování tooan / log aktivity vaší [účet úložiště Azure](https://docs.microsoft.com/en-us/azure/storage/storage-create-storage-account).</li><li>Sledování databázi Azure stavu pomocí [protokoly aktivity monitorování Azure](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</li><li>[Detekce hrozby](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-threat-detection) zjistí nezvyklé databázové aktivity, které indikují potenciální bezpečnostní hrozby toohello databáze. </li></ul> |
|<br>Azure Security Center| <ul><li>[Data monitorování](https://docs.microsoft.com/en-us/azure/security-center/security-center-enable-auditing-on-sql-databases) použití služby Azure Security Center jako centralizované zabezpečení řešení monitorování pro SQL a jinými službami Azure.</li></ul>|     

## <a name="conclusion"></a>Závěr
Databáze Azure je platforma robustní databáze, s celou řadu funkcí zabezpečení, které splňují mnoho organizace i regulačních požadavků. Můžete snadno chránit data pomocí řízení dat tooyour hello fyzický přístup a použití různých možností pro zabezpečení dat na hello souboru-, sloupec- nebo nízkoúrovňové transparentní šifrování dat, šifrování na úrovni buněk nebo zabezpečení na úrovni řádků. Vždy šifrovaný také umožní operace proti šifrovaná data, ke zjednodušení procesu hello aktualizací aplikace. Přístup k protokolům tooauditing aktivity databáze SQL se pak poskytuje hello informace, které potřebujete, abyste tooknow jak a kdy je přístup k datům.

## <a name="next-steps"></a>Další kroky
Hello ochranu databáze před uživateli se zlými úmysly a neoprávněným přístupem pomocí několika jednoduchých kroků můžete zvýšit. V tomto kurzu jste postup:

- Nastavit [pravidla brány firewall](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) vašeho serveru nebo databáze.
- Ochrana dat s [šifrování](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/sql-server-encryption).
- Povolit [auditování databáze SQL](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing).

