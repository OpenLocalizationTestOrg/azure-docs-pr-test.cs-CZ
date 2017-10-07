---
title: "aaaOverview databáze Azure pro služba relační databáze MySQL | Microsoft Docs"
description: "Přehled hello databáze Azure pro služba relační databáze MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 08/02/2017
ms.custom: mvc
ms.openlocfilehash: f02493e2c3c38ccab408a718f98861600481812d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-database-for-mysql-service-introduction"></a>Co je Azure Database pro databázi MySQL? Služba Úvod
Databáze pro databázi MySQL Azure je služba relační databáze v hello cloudu Microsoftu na základě [MySQL Community Edition](https://www.mysql.com/products/community/) databázového stroje.  Azure Database pro databázi MySQL nabízí:

- Předvídatelný výkon na více úrovních služby
- Dynamické škálovatelnost žádné výpadky aplikací
- Integrovanou vysokou dostupnost
- Ochrana dat

Tyto možnosti vyžadují téměř žádné správy a všechny jsou k dispozici bez dalších poplatků. Umožňují vám toofocus na rychlý vývoj aplikací a urychlení vaší toomarket času, než přidělování drahocenný čas a prostředky toomanaging virtuálních počítačů a infrastruktury. Kromě toho můžete dál toodevelop vaší aplikace pomocí hello otevřete source nástroje a platforma podle svého výběru a poskytovat s rychlostí hello a požadavky na vaše podnikání bez nutnosti nového dovednosti toolearn efektivitu.

![Databáze Azure pro koncepční diagram MySQL](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Tento článek je úvod tooAzure databáze MySQL základní koncepty a funkce související tooperformance, škálovatelnost a možnosti správy, s podrobnostmi tooexplore odkazy. Zjistit že tyto rychlé spuštění tooget, kterou jste zahájili:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md)

Sada ukázek Azure CLI najdete v části:
- [Ukázek Azure CLI pro databázi Azure pro databázi MySQL](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Úprava výkonu a škálování bez výpadků
Databáze Azure pro službu MySQL nabízí dvě úrovně služby: Basic a Standard. Každá úroveň nabízí různé výkon a možnosti toosupport lightweight tooheavyweight databázové úlohy. První aplikace s malou databází můžete vytvořit pro několik dolar měsíc a potom změnu vaší tooscale vrstvy služby s musí vaše řešení bez výpadků. Dynamické škálovatelnost umožňuje vaší databáze toorapidly reakce tootransparently změna požadavky na prostředky. Platíte jenom pro hello zdrojů, které potřebujete, když je potřebujete.

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Jak poznáme hello klikněte pravým tlačítkem na zastavení při přidávání nebo ubírání? Použijte hello předdefinované výkonu monitorování a výstrah funkce v kombinaci s hello hodnocení výkonu na základě výpočetní jednotky. Používání těchto funkcí, můžete rychle posoudit dopad hello škálování nahoru nebo dolů v závislosti na vaší stávající nebo projektu požadavkům na výkon. V tématu [koncepty: úrovních služeb](concepts-service-tiers.md) podrobnosti.

## <a name="keep-your-app-and-business-running"></a>Udržujte své aplikace a podnikáni v chodu
Azure špičkové 99,99 % dostupnost smlouvu o úrovni služeb (SLA) používá technologii globální sítě datových center spravovaných společností Microsoft, pomáhá udržet vaše aplikace s 24/7. S každou databází Azure pro server databáze MySQL můžete využít výhod integrované zabezpečení, odolnost proti chybám a ochranu dat, který byste jinak museli toobuy nebo návrh, vytvářet a spravovat. S Azure Database pro databázi MySQL, můžete použít v okamžiku obnovení toorecover server tooan dříve stavu až 35 dnů.

## <a name="secure-your-data"></a>Zabezpečení dat
Služba Azure databáze mají tradici zabezpečení dat, že Azure Database pro databázi MySQL zachovává s funkcemi, které omezit přístup, ochrana dat na rest a v provozu a umožňují sledovat aktivitu. Navštivte hello [Centrum zabezpečení Azure](https://www.microsoft.com/en-us/TrustCenter/Security/default.aspx) informace o bezpečnosti samotné platformy Azure.

Hello databáze Azure pro službu MySQL používá šifrování úložiště pro data na rest. Data, včetně zálohování, jsou zašifrovaná na disku (s výjimkou hello dočasné soubory vytvořené modulem hello při spuštění dotazů). Služba Hello používá šifrování AES 256 bitů, který je součástí šifrování úložiště Azure a hello klíče jsou spravován systémem. Šifrování úložiště je vždycky zapnuté a nejde zakázat.

Ve výchozím nastavení, hello databáze Azure pro službu MySQL je nakonfigurované toorequire [zabezpečení připojení SSL](./concepts-ssl-connection-security.md) pro data za provozu v síti hello. Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed hello" šifrování hello datový proud mezi hello serveru a aplikace.  Volitelně můžete zakázat, budete požadovat protokol SSL pro připojení služby tooyour databáze, pokud klientská aplikace nepodporuje připojení SSL.

## <a name="next-steps"></a>Další kroky
Teď, když jste pro čtení Úvod tooAzure databáze pro databázi MySQL a odpovědi hello otázku "Co je Azure databáze pro databázi MySQL?", jste připraveni:
- V tématu hello porovnávání náklady a kalkulačky stránce s cenami. [Ceny](https://azure.microsoft.com/pricing/details/mysql/)
- Začněte vytvořením první server. [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- Vytvoření první aplikace Python, PHP, Ruby, C\#, Java, Node.js: [používat knihovny připojení tooconnect tooAzure databáze pro databázi MySQL](concepts-connection-libraries.md)
