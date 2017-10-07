---
title: "aaaConnect tooSQL databázi pomocí C a C++ | Microsoft Docs"
description: "Použití hello ukázkový kód v této úvodní toobuild moderních aplikací s jazykem C++ a výkonné relační databázi v hello cloudu s Azure SQL Database."
services: sql-database
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: 
ms.assetid: 07d9e0b1-3234-4f17-a252-a7559160a9db
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 03/06/2017
ms.author: edmacauley
ms.openlocfilehash: 9b581424c91bfdd93deb6914212629519a011d8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-database-using-c-and-c"></a>Připojit tooSQL databázi pomocí C a C++
Tento příspěvek je zaměřen na vývojáře C a C++ při tooconnect tooAzure databázi SQL. Ho je rozdělena na oddíly, můžete přeskočit toohello oddíl, který nejlépe zaznamená váš zájem. 

## <a name="prerequisites-for-hello-cc-tutorial"></a>Předpoklady pro kurz hello C/C++
Ujistěte se, že máte hello následující položky:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).
* Sadu [Visual Studio](https://www.visualstudio.com/downloads/). Musíte nainstalovat toobuild součásti jazyk C++ hello a tuto ukázku spustit.
* [Visual Studio Linux Development](https://visualstudiogallery.msdn.microsoft.com/725025cf-7067-45c2-8d01-1e0fd359ae6e). Pokud vyvíjíte v systému Linux, je nutné nainstalovat rozšíření pro Visual Studio Linux hello. 

## <a id="AzureSQL"></a>Azure SQL Database a SQL Server na virtuálních počítačích
Azure SQL je založený na systému Microsoft SQL Server a je navrženou tooprovide vysokou dostupnost, původce a škálovatelné služby. Existuje mnoho výhod toousing SQL Azure přes proprietární databáze spuštěn místně. SQL Azure nemáte mít tooinstall, nastavit, provádět údržbu nebo spravovat databáze, ale pouze hello obsah a struktura hello vaší databáze. Typické věcí, které jsme starosti s databázemi jako odolnost proti chybám a redundance jsou všech součástí. 

Azure má aktuálně dvě možnosti pro hostování úloh SQL serveru: Azure SQL database, databáze jako služba a systému SQL server na virtuální počítače (VM). Nám nebude získat podrobnosti o hello rozdíly mezi těmito dvěma s tím rozdílem, že databáze Azure SQL je nejlepším řešením pro nové cloudové aplikace tootake výhod hello úspora nákladů a optimalizace výkonu, které cloudové služby poskytovat. Pokud uvažujete o migraci nebo rozšíření místní toohello cloudové aplikace, SQL server na virtuální počítač Azure může fungovat lépe za vás. jednoduché věcí tookeep v tomto článku vytvoříme Azure SQL database. 

## <a id="ODBC"></a>Technologie pro přístup k datům: rozhraní ODBC a OLE DB
Připojování tooAzure databáze SQL se neliší a aktuálně existují dva způsoby tooconnect toodatabases: rozhraní ODBC (Open Database connectivity) a OLE DB (propojování a vkládání objektů databáze). V posledních letech má Microsoft v souladu s [rozhraní ODBC pro přístup k datům nativní relační](https://blogs.msdn.microsoft.com/sqlnativeclient/2011/08/29/microsoft-is-aligning-with-odbc-for-native-relational-data-access/). Rozhraní ODBC je poměrně jednoduché a také mnohem rychlejší než OLE DB. Hello pouze přímý přístup paměti v tomto poli je, že rozhraní ODBC používat rozhraní API starého stylu jazyka C. 

## <a id="Create"></a>Krok 1: Vytvoření databáze Azure SQL
V tématu hello [stránku Začínáme](sql-database-get-started-portal.md) toolearn jak toocreate ukázkovou databázi.  Alternativně můžete to provést [krátké video dvě minuty](https://azure.microsoft.com/documentation/videos/azure-sql-database-create-dbs-in-seconds/) hello toocreate databázi Azure SQL pomocí portálu Azure.

## <a id="ConnectionString"></a>Krok 2: Získání připojovacího řetězce
Po zřízená vaší databázi Azure SQL, můžete potřebovat toocarry out hello následující informace o připojení toodetermine kroky a přidat IP klienta pro přístup přes bránu firewall. 

V [portál Azure](https://portal.azure.com/), přejděte připojovací řetězec databáze ODBC tooyour Azure SQL pomocí hello **zobrazit databázové připojovací řetězce** uveden jako součást oddílu hello přehled pro vaši databázi: 

![ODBCConnectionString](./media/sql-database-develop-cplusplus-simple/azureportal.png)

![ODBCConnectionStringProps](./media/sql-database-develop-cplusplus-simple/dbconnection.png)

Kopírovat obsah hello hello **ODBC (zahrnuje Node.js) [ověřování SQL]** řetězec. Můžeme použít tento řetězec novější tooconnect z příkazového řádku překladač naše C++ ODBC. Tento řetězec obsahuje podrobné informace, jako je například hello ovladače, serveru a další parametry připojení databáze. 

## <a id="Firewall"></a>Krok 3: Přidání brány firewall toohello IP
Přejděte toohello části brány firewall na serveru databáze a přidat váš [IP brány firewall toohello klienta pomocí těchto kroků](sql-database-configure-firewall-settings.md) toomake zda Navážeme úspěšné připojení: 

![AddyourIPWindow](./media/sql-database-develop-cplusplus-simple/ip.png)

V tomto okamžiku jste nakonfigurovali vaší databázi SQL Azure a jsou připravené tooconnect z vašeho kódu C++. 

## <a id="Windows"></a>Krok 4: Připojení z aplikace Windows C/C++
Můžete snadno připojit tooyour [databázi SQL Azure pomocí rozhraní ODBC do systému Windows pomocí této ukázce](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29) který sestavení pomocí sady Visual Studio. Ukázka Hello implementuje příkazového řádku překladač ODBC, kterou lze použít tooconnect tooour databázi SQL Azure. Tato ukázka má buď soubor databáze název zdrojového souboru (DSN) jako argument příkazového řádku nebo hello podrobné připojovací řetězec, který jsme předtím zkopírovali z hello portálu Azure. Vyvolat stránku hello vlastnost pro tento projekt a vložte připojovací řetězec hello jako argument příkazu, jak je vidět tady: 

![DSN Propsfile](./media/sql-database-develop-cplusplus-simple/props.png)

Zkontrolujte, že zadáte hello správné ověřování podrobnosti pro vaši databázi jako součást tohoto připojovacího řetězce databáze. 

Spusťte aplikaci toobuild hello ho. Měli byste vidět hello následující okno ověřování úspěšné připojení. Můžete dokonce spouštět některé základní příkazy SQL jako **vytvořit tabulku** toovalidate připojení k databázi:

![SQL příkazy](./media/sql-database-develop-cplusplus-simple/sqlcommands.png)

Alternativně můžete vytvořit soubor DSN pomocí hello průvodce, který se spustí, když jsou k dispozici žádné argumenty příkazu. Doporučujeme tuto možnost a zkuste to. Tento soubor DSN můžete použít pro automatizaci a chrání vaše nastavení ověřování: 

![Vytvoření souboru DSN](./media/sql-database-develop-cplusplus-simple/datasource.png)

Blahopřejeme! Teď úspěšně připojíte tooAzure SQL pomocí C++ a rozhraní ODBC v systému Windows. Můžete pokračovat čtení toodo hello stejnou pro platformu Linux také. 

## <a id="Linux"></a>Krok 5: Připojení z aplikace Linux C/C++
V případě, že jste se ještě ještě dozvěděli hello příspěvky, Visual Studio teď umožňuje toodevelop také aplikace C++ Linux. Další informace o tento nový scénář v hello [Visual C++ pro Linux Development](https://blogs.msdn.microsoft.com/vcblog/2016/03/30/visual-c-for-linux-development/) blogu. toobuild pro Linux, je nutné vzdáleného počítače, kde běží vaše distro Linux. Pokud nemáte k dispozici, můžete nastavit jeden rychle díky [Linux Azure virtuálních počítačů](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

V tomto kurzu dejte nám předpokládají, že byl jako distribučního Ubuntu 16.04 Linux nastavit. Hello kroky v tomto poli by se měly používat také tooUbuntu 15.10, Red Hat 6 a Red Hat 7. 

Hello následovně instalovat hello knihovny potřebné pro SQL a rozhraní ODBC pro vaše distro:

    sudo su
    sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/mssql-ubuntu-test/ xenial main" > /etc/apt/sources.list.d/mssqlpreview.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    apt-get update
    apt-get install msodbcsql
    apt-get install unixodbc-dev-utf16 #this step is optional but recommended*

Spusťte sadu Visual Studio. V nabídce Nástroje -> Možnosti -> Cross platformy -> Správce připojení, přidejte pole připojení tooyour Linux: 

![Možnosti nástrojů](./media/sql-database-develop-cplusplus-simple/tools.png)

Po navázání připojení přes protokol SSH, vytvořte šablonu prázdný projekt (Linux): 

![Nové šablony projektu](./media/sql-database-develop-cplusplus-simple/template.png)

Poté můžete přidat [nové C zdroje souborů a nahraďte tento obsah](https://github.com/Microsoft/VCSamples/blob/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29/odbcconnector/odbcconnector.c). Pomocí hello Funkce SQLAllocHandle rozhraní API ODBC, SQLSetConnectAttr a SQLDriverConnect, by měl být schopný tooinitialize a vytvořit databázi tooyour připojení. Jako ukázkový Windows ODBC hello, je nutné tooreplace volání hello SQLDriverConnect s podrobnostmi hello z vaší dříve zkopírováno z hello portál Azure parametry připojovacího řetězce databáze. 

     retcode = SQLDriverConnect(
        hdbc, NULL, "Driver=ODBC Driver 13 for SQL"
                    "Server;Server=<yourserver>;Uid=<yourusername>;Pwd=<"
                    "yourpassword>;database=<yourdatabase>",
        SQL_NTS, outstr, sizeof(outstr), &outstrlen, SQL_DRIVER_NOPROMPT);

Hello poslední věcí toodo před kompilování tooadd **odbc** jako závislost knihovny: 

![Přidání rozhraní ODBC jako vstupní knihovny](./media/sql-database-develop-cplusplus-simple/lib.png)

toolaunch vaší aplikace, zprovoznit hello Linux konzoly z hello **ladění** nabídky: 

![Linux konzoly](./media/sql-database-develop-cplusplus-simple/linuxconsole.png)

Pokud vaše připojení bylo úspěšné, měli byste vidět aktuální název databáze hello vytištěn v hello konzoly Linux: 

![Okno výstup konzoly Linux](./media/sql-database-develop-cplusplus-simple/linuxconsolewindow.png)

Blahopřejeme! Hello kurz byl úspěšně dokončen a nyní můžete přihlásit tooyour databázi SQL Azure z jazyka C++ na platformách systému Windows a Linux.

## <a id="GetSolution"></a>Získání hello dokončení C/C++ řešení kurzu
Můžete najít řešení GetStarted hello, který obsahuje všechny ukázky hello v tomto článku na githubu:

* [Ukázka ODBC C++ Windows](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29), stáhněte si hello Windows C++ ODBC ukázka tooconnect tooAzure SQL
* [Ukázka ODBC C++ Linux](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29), stáhněte si hello Linux C++ ODBC ukázka tooconnect tooAzure SQL

## <a name="next-steps"></a>Další kroky
* Zkontrolujte hello [přehled vývoje SQL databáze](sql-database-develop-overview.md)
* Další informace o hello [referenční dokumentace rozhraní API ODBC](https://docs.microsoft.com/sql/odbc/reference/syntax/odbc-api-reference/)

## <a name="additional-resources"></a>Další zdroje
* [Vzory návrhu pro aplikace SaaS s více tenanty využívající Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Prozkoumejte všechny hello [možnosti databáze SQL](https://azure.microsoft.com/services/sql-database/)

