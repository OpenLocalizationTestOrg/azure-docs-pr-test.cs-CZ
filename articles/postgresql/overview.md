---
title: "aaaOverview databáze Azure pro služba relační databáze PostgreSQL | Microsoft Docs"
description: "Poskytuje přehled databáze Azure pro služba relační databáze PostgreSQL."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 08/01/2017
ms.openlocfilehash: fd6821b56e5295b8b341d87b14d113f7a4b247c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-database-for-postgresql"></a>Co je Azure databáze PostgreSQL?

Azure databázi PostgreSQL je služba relační databáze v hello Microsoft Cloudová sestavená pro vývojáře na základě hello komunity verze softwaru open source [PostgreSQL](https://www.postgresql.org/) databázového stroje. Tato služba je ve verzi public preview. Azure databázi PostgreSQL přináší:
- Předvídatelný výkon na více úrovních služby
- Dynamické škálovatelnost žádné výpadky aplikací
- Integrovanou vysokou dostupnost
- Ochrana dat

Všechny tyto možnosti vyžadují téměř žádné správy a všechny jsou k dispozici bez dalších poplatků. Tyto funkce umožňují toofocus na rychlý vývoj aplikací a urychlení vaší toomarket času, než přidělování drahocenný čas a prostředky toomanaging virtuálních počítačů a infrastruktury. Kromě toho můžete dál toodevelop vaší aplikace pomocí hello otevřete source nástroje a platforma podle svého výběru a poskytovat s rychlostí hello a požadavky na vaše podnikání bez nutnosti nového dovednosti toolearn efektivitu. 

Tento článek je úvod tooAzure databáze PostgreSQL základní koncepty a funkce související tooperformance, škálovatelnost a možnosti správy. Zjistit že tyto rychlé spuštění tooget, kterou jste zahájili:

- [Vytvoření databáze Azure pro PostgreSQL pomocí portálu Azure](quickstart-create-server-database-portal.md)
- [Vytvoření databáze Azure pro PostgreSQL pomocí hello rozhraní příkazového řádku Azure](quickstart-create-server-database-azure-cli.md)

Řadu ukázek v Azure CLI a PowerShellu najdete tady:

- [Ukázek Azure CLI pro databázi Azure pro PostgreSQL](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Úprava výkonu a škálování bez výpadků

Databáze Azure pro službu PostgreSQL teď nabízí dvě úrovně služby: Basic a Standard. Každá úroveň služeb nabízí [různé úrovně výkonu, IOPS záruky a možnosti](concepts-service-tiers.md) toosupport lightweight tooheavyweight databázové úlohy. Pro pár peněz na měsíc můžete sestavit svoji první aplikaci na malé serveru a potom [změnit úroveň výkonu hello](scripts/sample-scale-server-up-or-down.md) v rámci služby vrstvy ručně nebo z programu na všechny čas toomeet hello potřebám vašeho řešení. Můžete provést bez výpadku tooyour aplikace nebo tooyour zákazníků. Umožňuje dynamické škálovatelnost vaší databáze tootransparently reagovat toorapidly měnící se požadavky na prostředky a umožňuje vám tooonly platit hello prostředky, je nutné, když je potřebujete.

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Jak můžete rozhodnout, kdy toodial nahoru či dolů? Použijete hello předdefinované výkonu monitorování a výstrah funkce v kombinaci s hello hodnocení výkonu na základě výpočetní jednotek. Používání těchto nástrojů, můžete rychle posoudit dopad hello výpočetní jednotky škálování nebo dolů na základě vaší aktuální nebo předpokládané výkonu potřeb. Podrobnosti najdete v tématu [databáze Azure pro výkon a možnosti PostgreSQL: co je dostupné na jednotlivých úrovních služby](./concepts-service-tiers.md).

## <a name="keep-your-app-and-business-running"></a>Udržujte své aplikace a podnikáni v chodu
Azure špičkové 99,99 % dostupnost (není k dispozici ve verzi preview) smlouvu o úrovni služeb (SLA) používá technologii globální sítě datových center spravovaných společností Microsoft, pomáhá udržet vaše aplikace s 24/7. S každou Azure databázi PostgreSQL serveru můžete využít výhod integrované zabezpečení, odolnost proti chybám a ochranu dat, který byste jinak museli toobuy nebo návrh, vytvářet a spravovat. Každá úroveň služeb s Azure Database pro PostgreSQL, nabízí komplexní sadu business kontinuitu podnikových procesů a možnosti, které můžete použít tooget nahoru a udržení tímto způsobem. Můžete použít [obnovení bodu v čase](howto-restore-server-portal.md) tooreturn tooan databáze dříve stavu až 35 dnů. Kromě toho pokud hello datového centra hostujícího vaše databáze dojde k výpadku, můžete obnovit databáze z geograficky redundantní kopie nejnovější zálohy.

## <a name="secure-your-data"></a>Zabezpečení dat
Služba Azure databáze mají tradici zabezpečení dat, databáze Azure pro PostgreSQL zachovává s funkcemi, které omezit přístup, ochrana dat na rest a v provozu a umožňují sledovat aktivitu. Navštivte hello [Centrum zabezpečení Azure](https://www.microsoft.com/TrustCenter/Security/default.aspx) informace o bezpečnosti samotné platformy Azure.

Hello databáze Azure pro službu PostgreSQL používá šifrování úložiště pro data na rest. Data, včetně zálohy jsou zašifrovaná na disku (s výjimkou hello dočasné soubory vytvořené modulem hello při spuštění dotazů). Služba Hello používá šifrování AES 256 bitů, který je součástí šifrování úložiště Azure a hello klíče jsou spravován systémem. Šifrování úložiště je vždycky zapnuté a nejde zakázat.

Ve výchozím nastavení, hello Azure databáze PostgreSQL služby je nakonfigurované toorequire [zabezpečení připojení SSL](./concepts-ssl-connection-security.md) pro data za provozu v síti hello. Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed hello" šifrování hello datový proud mezi hello serveru a aplikace.  Volitelně můžete zakázat, budete požadovat protokol SSL pro připojení služby tooyour databáze, pokud klientská aplikace nepodporuje připojení SSL.

## <a name="next-steps"></a>Další kroky
- V tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/postgresql/) porovnávání náklady a kalkulačky.
- Začínáme [vytvoření první databáze Azure pro PostgreSQL](./quickstart-create-server-database-portal.md).
- Vytvoření první aplikace Python, PHP, Ruby, C\#, Java, Node.js: [knihovny připojení](./concepts-connection-libraries.md)
