---
title: "SQL Server na virtuálních počítačích Azure – nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje odpovědi na nejčastější dotazy týkající se systémem SQL Server na virtuálních počítačích Azure."
services: virtual-machines-windows
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/23/2017
ms.author: v-shysun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 447ece9653a4cf69153f9c85e222e3ea4e8bae16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-azure-virtual-machines"></a>Nejčastější dotazy týkající se SQL Serveru na virtuálních počítačích Azure
Toto téma obsahuje odpovědi na některé nejčastější dotazy o spuštění [systému SQL Server na virtuálních počítačích Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

1. **Jak vytvořit virtuální počítač Azure se systémem SQL Server?**

    Nejsnazším řešením je vytvoření virtuálního počítače, který zahrnuje SQL Server. Kurz týkající se registrace do služby Azure a vytvoření virtuálního počítače s SQL z portálu, najdete v části [zřízení virtuálního počítače s SQL serverem na portálu Azure](virtual-machines-windows-portal-sql-server-provision.md). Můžete vybrat bitovou kopii virtuálního počítače, který používá na licencování SQL serveru platím za minutu, nebo můžete použít image, která umožňuje přineste si vlastní licenci na SQL Server. Máte také možnost ručně instalaci systému SQL Server na virtuálním počítači a opakovaného použití licenci na místě. Pokud jste přineste si vlastní licenci, musíte mít [mobilitou licencí v rámci programu Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/). Další informace najdete v tématu [Doprovodné materiály k cenám pro virtuální počítače Azure s SQL Serverem](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Jaký je rozdíl mezi virtuálním počítačům systému SQL a služba SQL Database?**

    Koncepčně systémem SQL Server na virtuální počítač Azure, není, se liší od systémem SQL Server ve vzdálené datovém centru. Naproti tomu [SQL Database](../../../sql-database/sql-database-technical-overview.md) nabízí databáze jako služba. S databází SQL nemáte přístup k počítačům, které hostují vaše databáze. Úplné porovnání najdete v tématu [volba cloudového řešení SQL serveru: Azure SQL (PaaS) Database nebo SQL Server na virtuálních počítačích Azure (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Jak můžete migrovat místní databáze systému SQL Server do cloudu?**

    Nejprve vytvořte virtuální počítač Azure s instancí serveru SQL. Poté migrujte místní databáze do této instance. Strategie migrace dat, najdete v části [migrovat databázi systému SQL Server do systému SQL Server ve virtuálním počítači Azure](virtual-machines-windows-migrate-sql.md).

1. **Můžete instalaci druhé instance systému SQL Server na stejného virtuálního počítače? Můžete změnit nainstalované součásti výchozí instance?**

    Ano. Instalační médium systému SQL Server nachází ve složce na **C** jednotky. Spustit **Setup.exe** z tohoto umístění pro přidání nové instance systému SQL Server nebo změna jiné instalace funkce serveru SQL Server na počítači. Všimněte si, že některé funkce, například automatizovaného zálohování, automatizovaných oprav a Azure Key Vault integraci, fungovat jenom na výchozí instanci.

1. **Můžete odinstalovat výchozí instance systému SQL Server?**

    Ano. Ale existují některé aspekty. Jak jsme uvedli v předchozím odpověď, funkce, které spoléhají na [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md) pracovat pouze u výchozí instance. Pokud odinstalujete výchozí instanci, podívejte se i nadále rozšíření a může způsobit chyby v protokolu událostí. Tyto chyby jsou z následujících dvou zdrojů: **správu přihlašovacích údajů systému Microsoft SQL Server** a **IaaS agenta systému Microsoft SQL Server**. Jedním z chyby může být podobné následujícímu:
    
        A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. 
        
    Pokud se rozhodnete odinstalovat výchozí instanci, odinstalovat také [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md) také.

1. **Jak upgradovat na novou verzi nebo edici systému SQL Server ve virtuálním počítači Azure?**

    V současné době není k dispozici žádné místní upgrade pro SQL Server běžící ve virtuálním počítači Azure. Vytvořit nový virtuální počítač Azure s požadovanou verzi nebo edici systému SQL Server, a potom migrovat své databáze na nový server pomocí standardní [techniky migrace dat](virtual-machines-windows-migrate-sql.md).

1. **Jak můžete instalovat Moje licencovanou kopii systému SQL Server na virtuální počítač Azure?**

    Chcete-li to provést dvěma způsoby. Můžete zřídit mezi [bitové kopie virtuálních počítačů, které podporuje licence](virtual-machines-windows-sql-server-iaas-overview.md#BYOL). Další možností je ke zkopírování instalačního média systému SQL Server do virtuálního počítače Windows serveru a SQL Server nainstalovat na virtuální počítač. Pro licencování z důvodů, musíte mít [mobilitou licencí v rámci programu Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/). Další informace najdete v tématu [Doprovodné materiály k cenám pro virtuální počítače Azure s SQL Serverem](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Můžete změnit virtuálního počítače použít vlastní licenci na SQL Server, pokud byl vytvořen z jedné z bitových kopií průběžnými platbami Galerie?**

    Ne. Nelze přepnout z platím za minutu licencí k používání vlastní licence. Vytvořit nový virtuální počítač Azure pomocí jedné z [BYOL image](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), a potom migrovat své databáze na nový server pomocí standardní [techniky migrace dat](virtual-machines-windows-migrate-sql.md).

1. **SQL Server převzetí služeb při selhání clusteru instance (FCI) podporuje na virtuálních počítačích Azure?**

   Ano. Můžete [vytvoření clusteru s podporou převzetí služeb při selhání systému Windows v systému Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) a použít prostory úložiště – přímé (S2D) pro úložiště clusteru. Alternativně můžete řešení třetí strany, clustering nebo úložiště, jak je popsáno v [vysoké dostupnosti a zotavení po havárii pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

1. **Musím platit na licencí systému SQL Server na virtuální počítač Azure, pokud je pouze používá pro pohotovostní režim nebo převzetí služeb při selhání?**

    Nemusíte zaplatit licence systém SQL Server účastní jako pasivní sekundární replika v nasazení s vysokou DOSTUPNOSTÍ, pokud máte Software Assurance a použijete mobilita licencí, jak je popsáno v [virtuální počítač nejčastější dotazy ohledně licencování](http://azure.microsoft.com/pricing/licensing-faq/).

1. **Jak jsou aktualizace a aktualizace service Pack použity na virtuální počítač SQL Server?**

    Virtuální počítače udělení ovládat hostitelský počítač, včetně, kdy a jak můžete použít aktualizace. Pro operační systém, můžete provést ručně aktualizace systému windows, nebo můžete povolit plánování služba s názvem [automatizovaných oprav](virtual-machines-windows-sql-automated-patching.md). Automatizované opravy nainstalují jakékoli aktualizace, které jsou označené jako důležité, včetně aktualizací SQL Serveru v této kategorii. Ostatní volitelné aktualizace SQL Server se musí instalovat ručně.

1. **Je možné nastavit konfigurace není vidět v galerii virtuálních počítačů (pro příklad Windows 2008 R2 a SQL Server 2012)?**

    Ne. Galerie Image virtuálních počítačů, které zahrnují systému SQL Server musíte vybrat jednu z zadané bitové kopie.

1. **Instalace nástrojů SQL Data na virtuálním počítači Azure**

     Stáhněte a nainstalujte nástroje SQL Data z [Microsoft SQL Server Data Tools – Business Intelligence pro Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Zdroje

Přehled systému SQL Server na virtuálních počítačích Azure, podívejte se video [virtuální počítač Azure je nejlepší platformou pro SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). V tomto tématu můžete získat také funkční ÚVOD [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

Další zdroje informací:

* [Zřízení virtuálního počítače s SQL Serverem na webu Azure Portal](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrace databáze do systému SQL Server ve virtuálním počítači Azure](virtual-machines-windows-migrate-sql.md)
* [Vysoká dostupnost a zotavení po havárii pro SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-high-availability-dr.md)
* [Osvědčené postupy z hlediska výkonu pro SQL Server na Azure Virtual Machines](virtual-machines-windows-sql-performance.md)
* [Modely aplikací a vývojové strategie pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md) 