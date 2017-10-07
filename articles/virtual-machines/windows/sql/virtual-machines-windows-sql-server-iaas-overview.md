---
title: "aaaOverview systému SQL Server na virtuálních počítačích Azure | Microsoft Docs"
description: "Informace o tom, jak toorun úplné edicích systému SQL Server v Azure virtuální počítače. Získejte přímé odkazy tooall virtuální počítač s SQL serverem Image a související obsah."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.openlocfilehash: 07be567c76f4435961592fc0872fe41cd45bd79d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Přehled SQL Serveru v Azure Virtual Machines
Toto téma popisuje možnosti provozování SQL serveru na virtuálních počítačích Azure (VM), spolu s [odkazy Image tooportal](#option-1-create-a-sql-vm-with-per-minute-licensing) a přehled [běžné úlohy](#manage-your-sql-vm).

> [!NOTE]
> Pokud jste již obeznámeni s SQL serverem a právě má toosee jak toodeploy virtuální počítač SQL Server najdete v části [zřízení virtuálního počítače s SQL Server v hello portál Azure](virtual-machines-windows-portal-sql-server-provision.md).
> 
> 

## <a name="overview"></a>Přehled
Pokud jste správce databáze nebo vývojář, poskytují virtuální počítače Azure toomove způsob, jak vaše místní systém SQL Server úlohy a aplikace toohello cloudu. Hello následující video obsahuje technické informace o virtuálních počítačích Azure SQL Server.

> [!VIDEO https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016/player]
> 
> 

Hello video obsahuje hello následující oblasti:

| Čas | Oblast |
| --- | --- |
| 00:21 |Co jsou virtuální počítače Azure? |
| 01:45 |Zabezpečení |
| 02:50 |Připojení |
| 03:30 |Spolehlivost a výkon úložiště |
| 05:20 |Velikost virtuálních počítačů |
| 05:54 |Vysoká dostupnost a smlouva SLA |
| 07:30 |Podpora konfigurace |
| 08:00 |Monitorování |
| 08:32 |Ukázka: Vytvoření virtuálního počítače s SQL Serverem 2016 |

> [!NOTE]
> Hello video se zaměřuje na SQL Server 2016, ale Azure poskytuje Image virtuálních počítačů pro mnoho verze systému SQL Server, včetně 2012, 2014 a 2016. 
> 
> 

## <a name="scenarios"></a>Scénáře
Existuje mnoho důvodů, které můžete vybrat toohost vaše data v Azure. Pokud vaše aplikace je přesunutí tooAzure, zlepšuje výkon tooalso přesunutí hello data. Ale jsou tu i další výhody. Máte automaticky přístup toomultiple datových centrech globální přítomnosti a zotavení po havárii. Hello dat je také vysoce zabezpečených a trvalost.

SQL Server běžící na virtuálních počítačích Azure je jednou z možností, jak ukládat relační data v Azure. Je to dobrá volba pro několik scénářů. Například podobně jako místní počítač systému SQL Server, možná tooan můžete tooconfigure hello virtuálního počítače Azure. Nebo můžete chtít toorun další aplikace a služby na hello stejnému databázovému serveru. Existují dva hlavní prostředky, které vám pomůžou promyslet si ještě další scénáře a předpoklady:

* [SQL Server na virtuálních počítačích Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/) poskytuje přehled hello Doporučené scénáře použití systému SQL Server na virtuálních počítačích Azure. 
* [Volba cloudového řešení systému SQL Server: Azure SQL (PaaS) Database nebo SQL Server na virtuálních počítačích Azure (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md) nabízí podrobné porovnání SQL Database a SQL Serveru běžícího na virtuálním počítači.

## <a name="create-a-new-sql-vm"></a>Vytvoření nového virtuálního počítače s SQL Serverem
Hello následující oddíly poskytují přímé odkazy toohello portál Azure pro hello bitové kopie Galerie virtuálních počítačů systému SQL Server. V závislosti na hello bitovou kopii, kterou vyberete můžete buď platím pro náklady na licencování SQL serveru na základě za minutu, nebo můžete zahrnout vlastní licenci (BYOL).

Najdete podrobné pokyny pro vytvoření nového virtuálního počítače SQL v kurzu hello [zřízení virtuálního počítače s SQL Server v hello portál Azure](virtual-machines-windows-portal-sql-server-provision.md). Projděte si také téma hello [nejlepší postupy z hlediska výkonu pro virtuální počítače serveru SQL](virtual-machines-windows-sql-performance.md), která vysvětluje, jak tooselect hello příslušný počítač velikost a další funkce dostupné při zřizování.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Možnost 1: Vytvoření virtuálního počítače s SQL Serverem a licencováním po minutách
Hello následující tabulka obsahuje přehled hello nejnovější obrázků systému SQL Server v galerii virtuálních počítačů hello. Klikněte na všechny odkaz toobegin vytváření nového virtuálního počítače s SQL se zadanou verzí, edice a operačního systému. 

> [!TIP]
> toounderstand hello virtuálních počítačů a SQL ceny pro tyto Image, najdete v části [ceny pokyny pro virtuální počítače Azure SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md).

| Verze | Operační systém | Edice |
| --- | --- | --- |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1ExpressWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1DeveloperWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2) |
| **SQL Server 2012 SP3** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2) |

Kromě seznamu toothis jiné kombinace verzí systému SQL Server a operační systémy jsou k dispozici. Najít další Image přes marketplace vyhledávání v hello portálu Azure. 

## <a id="BYOL"></a> Možnost 2: Vytvoření virtuálního počítače s SQL Serverem a stávající licencí
Můžete také používat vlastní licenci (BYOL). V tomto scénáři zaplatíte jenom za hello virtuálních počítačů bez jakýchkoli dalších poplatků za licencování SQL serveru. toouse vlastní licenci, použijte hello matice verzí systému SQL Server, edicí a operačních systémů níže. Hello portálu mají názvy těchto obrázků předponu **{BYOL}**.

> [!TIP]
> Používáním vlastní licence můžete časem ušetřit peníze za nepřetržité produkční úlohy. Další informace najdete v tématu [Doprovodné materiály k cenám pro virtuální počítače Azure s SQL Serverem](virtual-machines-windows-sql-server-pricing-guidance.md).

| Version | Operační systém | Edice |
| --- | --- | --- |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2) |
| **SQL Server 2012 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2) |

Kromě seznamu toothis jiné kombinace verzí systému SQL Server a operační systémy jsou k dispozici. Najít další Image přes marketplace vyhledávání v hello portálu Azure (vyhledejte "{BYOL} SQL Server").

> [!IMPORTANT]
> toouse Image virtuálních počítačů BYOL, musíte mít smlouvu Enterprise Agreement s [mobilitou licencí v rámci programu Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/). Budete také potřebovat platnou licenci pro hello verzi nebo edici systému SQL Server chcete toouse. Je nutné [zadejte hello nezbytné BYOL informace tooMicrosoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) v rámci **10** dnů od zřízení virtuálního počítače. 

> [!NOTE]
> Není možné toochange hello licencování vlastní licenci modelu virtuální počítač s SQL serverem toouse platím za minutu. V takovém případě musíte vytvořit nový virtuální počítač BYOL a migrace vaší databáze toohello nového virtuálního počítače. 

## <a name="manage-your-sql-vm"></a>Správa vašeho virtuálního počítače s SQL Serverem
Po zřízení virtuálního počítače s SQL Serverem je možné volitelně provést některé úlohy správy. V mnoha aspektech se SQL Server konfiguruje a spravuje úplně stejně jako místní instance SQL Serveru. Některé úlohy jsou však konkrétní tooAzure. Hello následující části zvýrazněte některé z těchto oblastí s informacemi o toomore odkazy.

### <a name="connect-toohello-vm"></a>Připojit toohello virtuálních počítačů
Jeden z kroků správu nejzákladnější hello je tooconnect tooyour virtuální počítač s SQL serverem prostřednictvím nástrojů, jako je například SQL Server Management Studio (SSMS). Pokyny, jak tooconnect tooyour nový virtuální počítač, Server SQL najdete v části [připojit tooa virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migrace dat
Pokud máte existující databázi, budete muset toomove této toohello nově zřízený virtuální počítač SQL. Seznam možností migrace a pokyny najdete v tématu [migrace databáze tooSQL Server na virtuálním počítači Azure](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Nakonfigurování vysoké dostupnosti
Pokud požadujete vysokou dostupnost, doporučujeme nakonfigurovat skupiny dostupnosti SQL Serveru. Znamená to využívat více virtuálních počítačů Azure ve virtuální síti. Hello portál Azure obsahuje šablonu, která nastaví tuto konfiguraci. Další informace najdete v tématu [Konfigurace skupiny dostupnosti AlwaysOn ve virtuálních počítačích Azure Resource Manageru](virtual-machines-windows-portal-sql-alwayson-availability-groups.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Pokud chcete nakonfigurovat toomanually skupiny dostupnosti a přidružený naslouchací proces, najdete v části [konfigurace skupin dostupnosti AlwaysOn ve virtuálním počítači Azure](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Další důležité informace o tom, co je potřeba zvážit z hlediska vysoké dostupnosti, najdete v tématu [Vysoká dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Zálohování dat
Virtuální počítače Azure můžete využít výhod [automatizovaného zálohování](virtual-machines-windows-sql-automated-backup.md), které pravidelně vytvoří zálohy databáze tooblob úložiště. Tento postup můžete použít také ručně. Další informace najdete v tématu [Použití služby Azure Storage pro zálohování a obnovování SQL Serveru](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Přehled všech možností zálohování a obnovení najdete v tématu [Zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Automatické aktualizace
Virtuální počítače Azure můžete použít [automatizovaných oprav](virtual-machines-windows-sql-automated-patching.md) tooschedule údržby pro instalaci důležité systém windows a SQL Server se automaticky aktualizuje.

### <a name="customer-experience-improvement-program-ceip"></a>Program Zlepšování softwaru a služeb na základě zkušeností uživatelů (CEIP)
Hello programu zlepšování zkušeností zákazníků (CEIP) je ve výchozím nastavení povolené. Tento pravidelně zasílá sestavy tooMicrosoft toohelp zlepšení systému SQL Server. Neexistuje žádné úlohy správy, vyžaduje se programu CEIP, pokud chcete, aby toodisable ho po zřízení. Můžete upravit nebo zakázat hello programu CEIP připojením toohello virtuálního počítače pomocí vzdálené plochy. Spusťte hello **chyba systému SQL Server a vytváření sestav využití** nástroj. Postupujte podle hello pokyny toodisable reporting. 

Další informace najdete v tématu hello programu CEIP části hello [přijmout licenční podmínky](https://msdn.microsoft.com/library/ms143343.aspx) tématu. 

## <a name="next-steps"></a>Další kroky

Dotazy o cenách najdete v části [ceny pokyny pro virtuální počítače Azure SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md) a hello [Azure stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Vyberte váš cílový edici systému SQL Server v hello **operační systém a Software** seznamu. Zobrazte hello ceny jinak velikostí virtuálních počítačů.

Máte nějaký další dotaz? Nejdříve si projděte hello [SQL Server na virtuálních počítačích Azure – nejčastější dotazy](virtual-machines-windows-sql-server-iaas-faq.md). Ale také přidat toohello dolní části všech témat toointeract virtuální počítač SQL vaše dotazy nebo připomínkami se společnosti Microsoft a hello komunity.
