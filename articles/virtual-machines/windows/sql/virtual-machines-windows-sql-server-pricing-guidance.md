---
title: "náklady na aaaManage efektivně pro SQL Server na virtuálních počítačích Azure | Microsoft Docs"
description: "Obsahuje doporučené postupy pro výběr hello správné virtuálního počítače SQL serverem cenový model."
services: virtual-machines-windows
documentationcenter: na
author: luisherring
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/18/2017
ms.author: jroth
ms.openlocfilehash: 6c6a4394e95b5a915ea3e7d5965730000d331036
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>Pokyny pro virtuální počítače SQL serveru Azure – ceny

Toto téma obsahuje cenovou pokyny pro virtuální počítače systému SQL Server v Azure. Existuje několik možností, které ovlivňují náklady a je důležité toopick hello správné bitovou kopii, která vyrovnává náklady obchodním požadavkům.

## <a name="free-licensed-sql-server-editions"></a>Uvolněte licenci edicích systému SQL Server

Pokud chcete toodevelop, otestovat, nebo sestavení testování konceptu, potom použijte hello volné licenci **SQL Server Developer edition**. Tato edice obsahuje všechno, co v systému SQL Server Enterprise edition, proto můžete ji použít toobuild jakoukoli aplikaci. Toorun je právě není povoleno v produkčním prostředí. Virtuální počítač systému SQL Server Developer vám bude účtovat pouze hello náklady hello virtuálního počítače, ne pro licencování SQL serveru.

Pokud chcete, aby toorun prosté úlohy v produkčním prostředí (< 4 jádra, < 1 GB paměti, < 10 GB/databáze), pak použít hello volné licenci **edice systému SQL Server Express**. Express virtuálního počítače s SQL se pouze účtují hello náklady na hello virtuální počítač, není licencování SQL.

Pro tyto vývoj/testování nebo lightweight produkčním prostředí můžete také šetřit peníze a vybrat menší velikost virtuálního počítače, který odpovídá těchto úloh. Hello DS1v2 může být vhodný pro tyto úlohy.

toocreate SQL serveru 2016 virtuální počítač Azure s jedním z těchto bitových kopií, najdete v části hello následující odkazy:

- [SQL Server 2016 vývojáře Azure virtuálních počítačů](https://ms.portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP1DeveloperWindowsServer2016-ARM)
- [SQL Server 2016 Express virtuálních počítačů Azure](https://ms.portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP1ExpressWindowsServer2016-ARM)

## <a name="paid-sql-server-editions"></a>Placených edicích systému SQL Server

Pokud máte jiný lightweight produkční zatížení, použijte jednu z následujících edicích systému SQL Server hello:

| Edice systému SQL Server | Úloha |
|-----|-----|
| Web | Malé weby |
| Standard | Malé toomedium úlohy |
| Enterprise | Velké soubory nebo důležité úlohy|

Máte dvě možnosti toopay za licencování SQL serveru pro tyto edice: *platíte za použití* nebo *přineste si vlastní licenci*.

### <a name="pay-per-usage"></a>Platba za použití

**Platící licenci na SQL Server hello za použití** znamená, že hello za minutu náklady na provozování hello virtuálního počítače Azure obsahuje hello náklady hello licenci na SQL Server. Zobrazí hello ceny hello různých edicích systému SQL Server (Web, Standard, Enterprise) v hello [virtuálního počítače Azure stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard). Hello náklady je hello stejné pro všechny verze systému SQL Server (too2016 2008 R2). Jak se systémem SQL Server licencování obecně hello náklady na licencování za minutu, závisí na hello počet jader virtuálního počítače.

Platícího hello systému SQL Server licencování za použití se doporučuje pro:

- Dočasné nebo pravidelné úlohy. Například aplikace, která vyžaduje toosupport událost pro několik měsíců každý rok, nebo obchodní analýzy v pondělí.
- Úlohy s neznámé životnost nebo určený počet číslic. Například aplikace, nemusí být nutné několik měsíců, nebo které může být více nebo méně výpočetního výkonu, v závislosti na vyžádání.

toocreate SQL serveru 2016 virtuální počítač Azure s jedním z těchto bitových kopií platím za využití najdete v části hello následující odkazy:

- [SQL Server virtuálního počítače Azure webové 2016](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016)
- [SQL Server 2016 standardní virtuální počítač Azure](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016)
- [SQL Server 2016 Enterprise Azure virtuálních počítačů](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016)

> [!IMPORTANT]
> Při vytváření virtuálního počítače s SQL serverem v hello portálu Azure, hello odhadované měsíční náklady na zobrazené na hello **zvolte velikost** okno nezahrnuje náklady na licencování SQL serveru. Toto je hello náklady hello virtuální počítač samostatně.
>
> ![Vyberte virtuální počítač velikost okna](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>Hello volné Express a vývojáře edic systému SQL Server Toto je celkový počet odhadované náklady hello. Ale pro webové, Standard a Enterprise, najít hello další náklady na licencování SQL na hello [virtuální počítače s Windows stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Na stránce s cenami hello vyberte váš cílový edici systému SQL Server.

### <a name="bring-your-own-license-byol"></a>Používání vlastní licence (BYOL)

**Přináší vlastní licenci na SQL Server prostřednictvím mobilita licencí**, nazývaná také jen tooas **BYOL**, znamená existující Volume License serveru SQL pomocí programu Software Assurance virtuální počítač Azure. Vzhledem k tomu, že jste už získali licence a programu Software Assurance prostřednictvím multilicenčního programu vám bude jenom virtuálního počítače s SQL serverem pomocí BYOL účtovat hello náklady na provozování hello virtuálního počítače, ne pro licencování SQL serveru.

Přináší vlastní SQL licencování prostřednictvím mobilita licencí se doporučuje pro:

- Nepřetržité úlohy. Například aplikace, která potřebuje toosupport podnikové operace 24 x 7.
- Úlohy s známé životnost a škálování. Například aplikace, která se bude vyžadovat celý rok hello a které vyžádání má byla naplánované.

toouse BYOL s virtuálního počítače s SQL Server, musí mít licenci pro SQL Server Standard nebo Enterprise a [programu Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1), což je požadovaná možnost prostřednictvím některé [Volume Licensing](https://www.microsoft.com/en-us/download/details.aspx?id=10585) programy a volitelné zakoupit s ostatními.  se liší Hello cenové úrovně poskytované prostřednictvím multilicenčních programů na základě typu hello smlouvy a hello a množství nebo závazků tooSQL serveru. Ale jako existuje pravidlo, přináší vlastní licenci pro produkční nepřetržité úlohy má hello následující výhody:

| BYOL výhody | Popis |
|-----|-----|
| **Úspora nákladů** | Přináší vlastní licenci na SQL Server je nákladově efektivnější než platícího za použití, pokud zatížení se budou spouštět nepřetržitě SQL Server Standard nebo Enterprise pro *více než 10 měsíců*. |
| **Dlouhodobé úspory** | V průměru je *30 % levnější za jeden rok.* toobuy nebo obnovení systému SQL Server licenci pro hello první 3 roky. Kromě toho po 3 roky, nepotřebujete toorenew hello licence už jenom platím pro Software Assurance. V tomto okamžiku je *200 % levnější*. |
| **Volné pasivní sekundární repliky** | Další výhodou přinesou vlastní licence je hello [volné licencování pro jeden pasivní sekundární repliky](https://azure.microsoft.com/pricing/licensing-faq/) na Server SQL pro vysokou dostupnost pro účely. To snižuje v poloviční hello licenční náklady na vysoce dostupné nasazení systému SQL Server (například použití skupin dostupnosti Always On). Hello práva toorun hello pasivní sekundární je zajišťována prostřednictvím hello výhody Software Assurance servery převzetí služeb při selhání. |

toocreate SQL serveru 2016 virtuální počítač Azure s jedním z těchto bitových kopií přineste si vlastníte license najdete v části virtuální počítače hello předponu "{BYOL}":

- [SQL Server 2016 Enterprise Azure virtuálních počítačů](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1EnterpriseWindowsServer2016)
- [SQL Server 2016 standardní virtuální počítač Azure](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)

> [!NOTE]
> Dejte nám vědět, do 10 dnů kolik licencí systému SQL Server, budete používat v Azure. Hello odkazy toohello předchozí obrázků je pokyny o tom, toodo to.

## <a name="avoid-unecessary-costs"></a>Vyhněte se unecessary náklady

Pokud používáte jakékoli úlohy, které nepoužívají nepřetržitě, vezměte v úvahu vypíná hello virtuálního počítače během období neaktivní hello. Platíte jenom za to, co používáte.

Například pokud jednoduše se na SQL Server na virtuální počítač Azure, nebude chcete tooincur poplatky omylem ponecháte systémem týdny. Jedno řešení je toouse hello [funkce automatického ukončení](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Virtuální počítač SQL autoshutdown](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Automatické ukončení je součástí větší sady podobné funkce poskytované službou [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab).

Pro jiné pracovní postupy, zvažte automaticky vypínání a restartování virtuálních počítačů Azure v rámci řešení pro skriptování, například [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!IMPORTANT]
> Vypínání a rušení přidělení virtuálního počítače je hello pouze způsob tooavoid poplatky. Jednoduše zastavení nebo pomocí tooshut možnosti napájení dolů hello virtuální počítač stále způsobuje poplatky za používání.

## <a name="next-steps"></a>Další kroky

Obecné Azure ceny pokyny najdete v tématu [zabránit neočekávané náklady s Azure fakturace a náklady na správu](../../../billing/billing-getting-started.md).

Hello nejnovější virtuálních počítačů, ceny, včetně systému SQL Server, najdete v části hello [virtuálního počítače Azure stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard).

Přečtěte si další témata virtuálního počítače systému SQL Server na [SQL Server na Přehled virtuálních počítačů Azure](virtual-machines-windows-sql-server-iaas-overview.md).
