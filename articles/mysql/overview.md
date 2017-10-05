---
title: "Přehled Azure databáze pro služba relační databáze MySQL | Microsoft Docs"
description: "Přehled databáze Azure pro služba relační databáze MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 08/02/2017
ms.custom: mvc
ms.openlocfilehash: a1becaf8465f68ecac768c5c6b2dbc95e8ff7278
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="what-is-azure-database-for-mysql-service-introduction"></a>Co je Azure Database pro databázi MySQL? Služba Úvod
Databáze pro databázi MySQL Azure je služba relační databáze v cloudu Microsoftu na základě [MySQL Community Edition](https://www.mysql.com/products/community/) databázového stroje.  Azure Database pro databázi MySQL nabízí:

- Předvídatelný výkon na více úrovních služby
- Dynamické škálovatelnost žádné výpadky aplikací
- Integrovanou vysokou dostupnost
- Ochrana dat

Tyto možnosti vyžadují téměř žádné správy a všechny jsou k dispozici bez dalších poplatků. Umožňují zaměřit na rychlý vývoj aplikací a urychlení doby uvedení na trh, namísto přidělování drahocenný čas a prostředky se správou virtuálních počítačů a infrastruktury. Kromě toho můžete nadále vývoj aplikace s otevřeným zdrojem nástroje a platformy podle svého výběru a poskytovat s rychlostí a požadavky na vaše podnikání bez nutnosti další nové dovednosti efektivitu.

![Databáze Azure pro koncepční diagram MySQL](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Tento článek je úvodem do databáze Azure pro MySQL základní koncepty a funkcí týkajících se výkonu, škálovatelnosti a spravovatelnosti, s odkazy na podrobnější informace. Tyto rychlé starty vám pomůžou začít:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md)

Sada ukázek Azure CLI najdete v části:
- [Ukázek Azure CLI pro databázi Azure pro databázi MySQL](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Úprava výkonu a škálování bez výpadků
Databáze Azure pro službu MySQL nabízí dvě úrovně služby: Basic a Standard. Každá úroveň nabízí různé výkon a možnosti pro podporu – od nejlehčích k těm nejnáročnějším. První aplikace s malou databází můžete vytvořit pro několik dolar měsíc a potom změnit úroveň služby škálování s potřebami vaše řešení bez výpadků. Dynamické škálovatelnost umožňuje databázi transparentně reagovat na měnící se rychle požadavky na prostředky. Platí pouze pro prostředky, které budete potřebovat, pokud je potřebujete.

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Jak poznáme správnou hodnotu nastavení při přidávání nebo ubírání výkonu? Pomocí předdefinovaných výkonu monitorování a výstrah funkce v kombinaci s hodnocení výkonu na základě výpočetní jednotky. Používání těchto funkcí, můžete rychle posoudit dopad škálování nahoru nebo dolů v závislosti na vaší stávající nebo projektu požadavkům na výkon. V tématu [koncepty: úrovních služeb](concepts-service-tiers.md) podrobnosti.

## <a name="keep-your-app-and-business-running"></a>Udržujte své aplikace a podnikáni v chodu
Azure špičkové 99,99 % dostupnost smlouvu o úrovni služeb (SLA) používá technologii globální sítě datových center spravovaných společností Microsoft, pomáhá udržet vaše aplikace s 24/7. S každou databází Azure pro server databáze MySQL můžete využít výhod integrované zabezpečení, odolnost proti chybám a ochrany dat, která by jinak muset koupit nebo návrh, vytvářet a spravovat. S Azure Database pro databázi MySQL můžete v okamžiku obnovení obnovit server do předchozího stavu, až 35 dnů.

## <a name="secure-your-data"></a>Zabezpečení dat
Služba Azure databáze mají tradici zabezpečení dat, že Azure Database pro databázi MySQL zachovává s funkcemi, které omezit přístup, ochrana dat na rest a v provozu a umožňují sledovat aktivitu. Přejděte [Centrum zabezpečení Azure](https://www.microsoft.com/en-us/TrustCenter/Security/default.aspx) informace o bezpečnosti samotné platformy Azure.

Databáze Azure pro službu MySQL používá šifrování úložiště pro data na rest. Data, včetně zálohování, jsou zašifrovaná na disku (s výjimkou dočasné soubory vytvořené modulem při spuštění dotazů). Služba používá šifrování AES 256 bitů, který je součástí šifrování úložiště Azure a klíče je spravován systémem. Šifrování úložiště je vždycky zapnuté a nejde zakázat.

Ve výchozím nastavení je databáze Azure pro službu MySQL nakonfigurovaná tak, aby vyžadovala [zabezpečení připojení SSL](./concepts-ssl-connection-security.md) pro data za provozu v síti. Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.  Volitelně můžete zakázat, budete požadovat protokol SSL pro připojení k databázi služby, pokud klientská aplikace nepodporuje připojení SSL.

## <a name="next-steps"></a>Další kroky
Teď, když jste si přečetli Úvod do Azure Database pro databázi MySQL a znáte odpověď na otázku "Co je Azure databáze pro databázi MySQL?", jste připraveni:
- Najdete na stránce s cenami pro porovnávání náklady a kalkulačky. [Ceny](https://azure.microsoft.com/pricing/details/mysql/)
- Začněte vytvořením první server. [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- Vytvoření první aplikace Python, PHP, Ruby, C\#, Java, Node.js: [připojení knihovny používané pro připojení k databázi Azure pro databázi MySQL](concepts-connection-libraries.md)
