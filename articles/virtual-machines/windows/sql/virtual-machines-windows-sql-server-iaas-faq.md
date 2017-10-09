---
title: "aaaSQL Server na virtuálních počítačích Azure – nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje odpovědi toofrequently kladené dotazy týkající se systémem SQL Server na virtuálních počítačích Azure."
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
ms.openlocfilehash: 70a8777bf765dcc69f433aa1fb59eb94929caab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-azure-virtual-machines"></a>Nejčastější dotazy týkající se SQL Serveru na virtuálních počítačích Azure
Toto téma obsahuje odpovědi toosome Dobrý den nejčastější dotazy týkající se spouštění [systému SQL Server na virtuálních počítačích Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

1. **Jak vytvořit virtuální počítač Azure se systémem SQL Server?**

    Hello Nejsnazším řešením je toocreate virtuální počítač, který zahrnuje SQL Server. Kurz týkající se registrace do služby Azure a vytvoření virtuálního počítače s SQL z hello portálu, najdete v části [zřízení virtuálního počítače s SQL Server v hello portálu Azure](virtual-machines-windows-portal-sql-server-provision.md). Můžete vybrat bitovou kopii virtuálního počítače, který používá na licencování SQL serveru platím za minutu, nebo můžete použít image, která vám umožní toobring vlastní licenci na SQL Server. Máte také možnost hello ručně instalaci systému SQL Server na virtuálním počítači a opakovaného použití licenci na místě. Pokud jste přineste si vlastní licenci, musíte mít [mobilitou licencí v rámci programu Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/). Další informace najdete v tématu [Doprovodné materiály k cenám pro virtuální počítače Azure s SQL Serverem](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Co je hello rozdíl mezi virtuálním počítačům systému SQL a hello služba SQL Database?**

    Koncepčně systémem SQL Server na virtuální počítač Azure, není, se liší od systémem SQL Server ve vzdálené datovém centru. Naproti tomu [SQL Database](../../../sql-database/sql-database-technical-overview.md) nabízí databáze jako služba. S databází SQL nemáte přístup toohello počítače, které hostují vaše databáze. Úplné porovnání najdete v tématu [volba cloudového řešení SQL serveru: Azure SQL (PaaS) Database nebo SQL Server na virtuálních počítačích Azure (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Jak můžete migrovat toohello databáze systému SQL Server Moje místní cloudu?**

    Nejprve vytvořte virtuální počítač Azure s instancí serveru SQL. Potom migrujte instance toothat místní databáze. Strategie migrace dat, najdete v části [migrovat tooSQL databáze serveru SQL Server ve virtuálním počítači Azure](virtual-machines-windows-migrate-sql.md).

1. **Můžete instalaci druhé instance systému SQL Server na hello stejného virtuálního počítače? Můžete změnit nainstalované součásti hello výchozí instanci?**

    Ano. Hello instalačního média systému SQL Server je umístěn ve složce v hello **C** jednotky. Spustit **Setup.exe** z tohoto umístění tooadd nové instance systému SQL Server nebo toochange jiné nainstalované funkce systému SQL Server na počítači hello. Všimněte si, že některé funkce, například automatizovaného zálohování, automatizovaných oprav a Azure Key Vault integraci, pracovat pouze proti hello výchozí instanci.

1. **Můžete odinstalovat hello výchozí instance systému SQL Server?**

    Ano. Ale existují některé aspekty. Jak je uvedeno v předchozí odpovědi hello, funkce, které jsou závislé na hello [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md) pracovat pouze v hello výchozí instanci. Pokud odinstalujete hello výchozí instance, pokračuje toolook pro něj hello rozšíření a může způsobit chyby v protokolu událostí. Tyto chyby jsou z následujících dvou zdrojů hello: **správu přihlašovacích údajů systému Microsoft SQL Server** a **IaaS agenta systému Microsoft SQL Server**. Jedním z hello chyby může být podobné toohello následující:
    
        A network-related or instance-specific error occurred while establishing a connection tooSQL Server. hello server was not found or was not accessible. 
        
    Pokud se rozhodnete toouninstall hello výchozí instance, také odinstalovat hello [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md) také.

1. **Jak mohu provést upgrade tooa novou verzi nebo edici hello systému SQL Server ve virtuálním počítači Azure?**

    V současné době není k dispozici žádné místní upgrade pro SQL Server běžící ve virtuálním počítači Azure. Vytvořit nový virtuální počítač Azure s hello potřeby verzi nebo edici systému SQL Server, a potom migrovat nový server databáze toohello pomocí standardní [techniky migrace dat](virtual-machines-windows-migrate-sql.md).

1. **Jak můžete instalovat Moje licencovanou kopii systému SQL Server na virtuální počítač Azure?**

    Existují dva způsoby toodo to. Můžete zřídit jeden hello [bitové kopie virtuálních počítačů, které podporuje licence](virtual-machines-windows-sql-server-iaas-overview.md#BYOL). Další možností je toocopy hello systému SQL Server instalační média tooa virtuálního počítače Windows serveru a SQL Server nainstalovat na hello virtuálních počítačů. Pro licencování z důvodů, musíte mít [mobilitou licencí v rámci programu Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/). Další informace najdete v tématu [Doprovodné materiály k cenám pro virtuální počítače Azure s SQL Serverem](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Je možné změnit počítač toouse vlastní licenci na SQL Server Pokud byl vytvořen z jednoho hello průběžnými platbami Galerie obrázků?**

    Ne. Vlastní licenci, nelze přepnout z licencování toousing platím za minutu. Vytvořit nový virtuální počítač Azure pomocí jedné z hello [BYOL image](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), a potom migrovat nový server databáze toohello pomocí standardní [techniky migrace dat](virtual-machines-windows-migrate-sql.md).

1. **SQL Server převzetí služeb při selhání clusteru instance (FCI) podporuje na virtuálních počítačích Azure?**

   Ano. Můžete [vytvoření clusteru s podporou převzetí služeb při selhání systému Windows v systému Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) a použít prostory úložiště – přímé (S2D) pro úložiště clusteru hello. Alternativně můžete řešení třetí strany, clustering nebo úložiště, jak je popsáno v [vysoké dostupnosti a zotavení po havárii pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

1. **Musím toopay toolicense SQL Server na virtuální počítač Azure, pokud je pouze používá pro pohotovostní režim nebo převzetí služeb při selhání?**

    Nemáte toopay toolicense jeden systému SQL Server účastní jako pasivní sekundární replika v nasazení s vysokou DOSTUPNOSTÍ, pokud máte Software Assurance a použijete mobilita licencí, jak je popsáno v [virtuální počítač nejčastější dotazy ohledně licencování](http://azure.microsoft.com/pricing/licensing-faq/).

1. **Jak jsou aktualizace a aktualizace service Pack použity na virtuální počítač SQL Server?**

    Virtuální počítače udělení ovládat hello hostitelský počítač, včetně, kdy a jak můžete použít aktualizace. Pro hello operační systém, můžete provést ručně aktualizace systému windows, nebo můžete povolit plánování služba s názvem [automatizovaných oprav](virtual-machines-windows-sql-automated-patching.md). Automatizované opravy nainstalují jakékoli aktualizace, které jsou označené jako důležité, včetně aktualizací SQL Serveru v této kategorii. Další volitelné aktualizace tooSQL, Server musí být nainstalován ručně.

1. **Je možné tooset až konfigurace není vidět v galerii virtuálních počítačů hello (pro příklad Windows 2008 R2 a SQL Server 2012)?**

    Ne. Galerie Image virtuálních počítačů, které zahrnují systému SQL Server musíte vybrat jednu z Image poskytované hello.

1. **Instalace nástrojů SQL Data na virtuálním počítači Azure**

     Stáhnout a nainstalovat nástroje SQL Data hello z [Microsoft SQL Server Data Tools – Business Intelligence pro Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Zdroje

Přehled systému SQL Server na virtuálních počítačích Azure, podívejte se na hello video [virtuální počítač Azure je hello nejlepší platformou pro SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). Dobrý Úvod můžete získat také v tématu hello [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

Další zdroje informací:

* [Zřízení virtuálního počítače s SQL Server v hello portálu Azure](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrace databáze tooSQL Server na virtuálním počítači Azure](virtual-machines-windows-migrate-sql.md)
* [Vysoká dostupnost a zotavení po havárii pro SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-high-availability-dr.md)
* [Osvědčené postupy z hlediska výkonu pro SQL Server na Azure Virtual Machines](virtual-machines-windows-sql-performance.md)
* [Modely aplikací a vývojové strategie pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md) 