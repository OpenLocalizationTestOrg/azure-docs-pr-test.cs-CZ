---
title: "AAA \"cenových úrovní za Azure databáze pro databázi MySQL | Microsoft Docs\""
description: "Cenové úrovně v Azure databáze pro databázi MySQL"
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 2a05f00aff4f7166f04e035eb2a47e7662888ebc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-options-and-performance-understand-whats-available-in-each-pricing-tier"></a>Databáze Azure pro výkon a možnosti MySQL: co je k dispozici v jednotlivých cenových úrovní
Když vytvoříte databázi Azure pro server databáze MySQL, rozhodnete na tři hlavní možnosti tooconfigure hello prostředky přidělené pro tento server. Tyto možnosti vliv hello výkonu a možností škálování hello serveru.
- Cenová úroveň
- Výpočetní jednotky
- Storage (GB)

Každá cenová úroveň má rozsah toochoose (výpočetní jednotky) úrovně výkonu, v závislosti na požadavcích vaší úlohy. Vyšší úrovně výkonu poskytnout další prostředky pro váš server navrženou toodeliver vyšší propustnost. Můžete změnit úroveň výkonu hello serveru v rámci cenovou úroveň s téměř žádné výpadky aplikací.

> [!IMPORTANT]
> Zatímco hello služby je ve verzi public preview, není zaručené smlouvy úroveň služeb (SLA).

V rámci serveru Azure Database for MySQL můžete mít jednu nebo několik databází. Můžete vyjádřit výslovný toocreate jednu databázi na serveru tooutilize všechny prostředky hello nebo vytvořit více databází tooshare hello prostředky. 

## <a name="choose-a-pricing-tier"></a>Zvolte cenovou úroveň.
Zatímco ve verzi preview, Azure Database pro databázi MySQL nabízí dvě cenové úrovně: Basic a Standard. Úroveň Premium zatím není k dispozici, ale brzy. 

Hello následující tabulka obsahuje příklady hello cenové úrovně nejlepší vhodné pro různé zátěže a aplikace.

| Cenová úroveň | Cílová zátěž |
| :----------- | :----------------|
| Basic | Nejvhodnější pro malé úlohy, které vyžadují škálovatelných výpočetních prostředků a úložiště bez zaručit IOPS. Mezi příklady patří servery, které jsou používány pro vývoj nebo testování nebo méně rozsáhlá zřídka používané aplikace. |
| Standard | Hello přejděte toooption pro cloudové aplikace, které potřebují IOPS zaručit při vysoké propustnosti. Mezi příklady patří webové nebo analytických aplikací. |
| Premium | Nejvhodnější pro úlohy, které je třeba s nízkou latencí pro transakce a vstupně-výstupní operace. Poskytuje hello nejlepší podporu pro více souběžných uživatelů. Použít toodatabases podporující kritické podnikové procesy.<br />cenová úroveň Premium Hello není k dispozici ve verzi preview. |

toodecide o cenách vrstvy, první spuštění tak, že určíte, pokud vaše úlohy potřebuje záruku IOPS. Pokud ano, použijte cenová úroveň Standard.

| **Funkce cenové úrovně** | **Basic** | **Standard** |
| :------------------------ | :-------- | :----------- |
| Maximální výpočetní jednotky | 100 | 800 | 
| Maximální celková velikost úložiště | 1 TB | 1 TB | 
| Záruku IOPS úložiště | Není k dispozici | Ano | 
| Maximální velikost úložiště IOPS | Není k dispozici | 3000 | 
| Doba uchovávání záloh databáze | 7 dní | 35 dní | 

Během období preview hello nelze změnit cenovou úroveň, po vytvoření hello server. V budoucích hello bude možné tooupgrade nebo starší verzi serveru z jedné vrstvy tooanother cenovou úroveň.

## <a name="understand-hello-price"></a>Pochopení hello cena
Když vytvoříte novou databázi Azure pro databázi MySQL uvnitř hello [portálu Azure](https://portal.azure.com/#create/Microsoft.MySQLServer), klikněte na tlačítko hello **cenová úroveň** okno a hello měsíční náklady se zobrazí na základě na vybrané možnosti hello. Pokud nemáte předplatné Azure, použijte hello Azure cenové kalkulačky tooget odhadované ceny. Navštivte hello [Azure cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/) web, pak klikněte na tlačítko **přidat položky**, rozbalte hello **databáze** kategorie a zvolte **Azure Database pro databázi MySQL**  toocustomize hello možnosti.

## <a name="choose-a-performance-level-compute-units"></a>Zvolte si úroveň výkonu (výpočetní jednotky)
Jakmile zjistíte hello cenovou úroveň pro vaši databázi Azure pro server databáze MySQL, jste úroveň výkonu hello připravené toodetermine výběrem hello počet výpočetní jednotky potřebné. Vhodná výchozí hodnota je 200 nebo 400 výpočetní jednotky pro aplikace, které vyžadují vyšší souběžnosti uživatele pro jejich web nebo analytické úlohy a postupně podle potřeby upravit. 

Výpočetní jednotky jsou měřítkem propustnost zpracování procesoru, který zaručeně tooa toobe k dispozici jednotné Azure databáze pro server databáze MySQL. Výpočetní jednotka je kombinaci měření prostředků procesoru a paměti.  Další informace najdete v tématu [vysvětlením výpočetní jednotky](concepts-compute-unit-and-storage.md)

### <a name="basic-pricing-tier-performance-levels"></a>Základní cenovou úroveň úrovně výkonu:

| **Úroveň výkonu** | **50** | **100** |
| :-------------------- | :----- | :------ |
| Maximální počet výpočetní jednotky | 50 | 100 |
| Velikost zahrnuté úložiště | 50 GB | 50 GB |
| Maximální velikost úložiště serveru\* | 1 TB | 1 TB |

### <a name="standard-pricing-tier-performance-levels"></a>Standardní cenovou úroveň úrovně výkonu:

| **Úroveň výkonu** | **100** | **200** | **400** | **800** |
| :-------------------- | :------ | :------ | :------ | :------ |
| Maximální počet výpočetní jednotky | 100 | 200 | 400 | 800 |
| Velikost zahrnuté úložiště a zřízené IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS |
| Maximální velikost úložiště serveru\* | 1 TB | 1 TB | 1 TB | 1 TB |
| Zřízení serveru maximální IOPS | 3000 IOPS | 3000 IOPS | 3000 IOPS | 3000 IOPS |
| Zřízení serveru maximální IOPS za GB | Opravené 3 IOPS za GB | Opravené 3 IOPS za GB | Opravené 3 IOPS za GB | Opravené 3 IOPS za GB |

\*Maximální velikost úložiště serveru odkazuje toohello maximální zřízené velikost úložiště pro váš server.

## <a name="storage"></a>Úložiště 
Konfigurace úložiště Hello definuje hello množství úložiště kapacity k dispozici tooan Azure databáze pro server databáze MySQL. úložiště Hello používá služba hello zahrnuje hello databázové soubory, transakční protokoly a protokoly server hello MySQL. Zvažte hello velikost úložiště potřebné toohost své databáze a při výběru konfigurace úložiště hello hello požadavky na výkon (IOPS).

Některé kapacita úložiště je zahrnutý minimálně s každou cenovou úroveň, uvedené v předcházející tabulce jako "Velikost úložiště zahrnuté." hello Kapacita úložiště lze přidat při hello serveru, v přírůstcích po 125 GB toohello maximální povolené úložiště. kapacita úložiště další Hello se dá nakonfigurovat nezávisle na výpočetní jednotky konfigurace hello. podle toho hello množství úložiště vybrané změny ceny Hello.

Konfigurace IOPS Hello v každé úrovni výkonu má vztah toohello cenová úroveň a velikost úložiště hello vybrali. Základní úroveň neposkytuje záruku IOPS. V rámci hello standardní cenovou úroveň úměrně hello IOPS toomaximum velikost úložiště v pevný poměr 3:1. Hello zahrnuté úložiště záruky 125 GB pro 375 zřizuje IOPS, každý s vstupně-výstupní operace velikost až too256 KB. Můžete zvolit další úložiště až maximální too1 TB, tooguarantee 3 000 zřízený IOPS.

Monitorování hello metriky graf v hello Azure portal nebo zápisu rozhraní příkazového řádku Azure příkazy toomeasure hello spotřeba úložiště a IOPS. Toomonitor relevantní metriky jsou limit úložiště, procento úložiště, používá úložiště a vstupně-výstupní operace procent.

>[!IMPORTANT]
> Zatímco ve verzi preview, zvolte hello velikost úložiště v době hello vytvoření hello server. Změna velikosti úložiště hello na existující server se ještě nepodporuje. 

## <a name="scaling-a-server-up-or-down"></a>Škálování server nahoru nebo dolů
Původně zvolíte hello cenovou úroveň a úroveň výkonu, když vytvoříte databázi Azure pro databázi MySQL. Později, je možné škálovat hello výpočetní jednotky nahoru nebo dolů, dynamicky do rozsahu hello hello stejné cenová úroveň. V hello portálu Azure, posuňte hello výpočetní jednotky na serveru hello cenová úroveň okno nebo skript podle tento příklad: [sledování a škálování Azure Database pro MySQL server pomocí rozhraní příkazového řádku Azure](scripts/sample-scale-server.md)

Hello výpočetní jednotky škálování se provádí nezávisle na hello maximální velikost úložiště, které jste vybrali.

Pozadí hello změna hello úrovně výkonu databáze vytvoří repliku hello původní databáze na novou úroveň výkonu hello a pak přepne toohello repliky připojení. Během tohoto procesu bude ztracena žádná data. Během okamžiku stručný hello, když jsme přepínač přes toohello repliky jsou zakázané připojení toohello databáze, takže některé transakce na cestě může být vrácena zpět. Délka tohoto časového období je různá, ale v průměru nepřekračuje 4 sekundy a ve více než 99 % případů nepřekročí 30 sekund. Pokud máte velké množství transakcí pohybující se v okamžiku připojení hello jsou zakázané, toto okno může být delší.

Hello dobu trvání procesu hello celý škálování závisí na velikosti hello a cenovou úroveň hello serveru před a po změně hello. Například server, který mění výpočetní jednotky v rámci hello standardní cenovou úroveň, provést v rámci několik minut. Hello nové vlastnosti pro hello server se nepoužívají, dokud nebudou dokončeny změny hello.

## <a name="next-steps"></a>Další kroky
- Další informace o výpočetní jednotky, najdete v části [vysvětlením výpočetní jednotky](concepts-compute-unit-and-storage.md)
- Zjistěte, jak příliš[sledování a škálování Azure Database pro MySQL server pomocí rozhraní příkazového řádku Azure](scripts/sample-scale-server.md)
