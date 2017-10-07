---
title: "aaaAzure SQL databáze Azure Případová studie - Umbraco | Microsoft Docs"
description: "Další informace o tom, jak Umbraco používá SQL Database tooquickly zřizování a škálování služby pro tisíce klienty v cloudu hello"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5243d31e-3241-4cb0-9470-ad488ff28572
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 93e39e509831a5ff90f129d9537ece0b0dafef0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="umbraco-uses-azure-sql-database-tooquickly-provision-and-scale-services-for-thousands-of-tenants-in-hello-cloud"></a>Umbraco používá Azure SQL Database tooquickly zřizování a škálování služby pro tisíce klienty v cloudu hello
![Umbraco Logo](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco je oblíbených open-source obsahu – systém správy (CMS), můžete spustit nic z malých kampaň či – příručka lokalit aplikace toocomplex pro žádnou 500 společnosti a globální média weby. 

> "Máme poměrně velké komunita vývojáře, kteří používají hello systému s více než 100 000 vývojáři na našich fórech a více než 350,000 lokalit, které jsou nasazeny, systémem Umbraco."
> 
> – Mortena Christensen, technické realizace, Umbraco
> 
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-Umbraco/player]
> 
> 

toosimplify zákaznických nasazení, Umbraco přidat Umbraco jako-Service (UaaS): nabídku softwaru jako služba (SaaS), která eliminuje potřebu hello místní nasazení poskytuje integrované škálování a odebere správu režie povolením Vývojáři toofocus na inovace spíše než řešení pro správu produktu. Umbraco je možné tooprovide tyto výhody podle hello flexibilní modelu platforma jako služba (PaaS), které nabízí Microsoft Azure.

UaaS umožňuje zákazníkům toouse SaaS Umbraco CMS možnosti, které byly dříve mimo jejich reach. Těchto zákazníků se zřídí pomocí pracovního prostředí CMS, která obsahuje provozní databáze. Zákazníci můžete přidat až tootwo další databáze pro vývoj a přípravných prostředí, v závislosti na jejich požadavky. Pokud se požaduje nového prostředí, automatizovaného procesu přiřadí tohoto zákazníka databáze automaticky. novou databázi Hello je připraven v sekundách, protože databáze hello již předem zřízená podle Umbraco z Azure elastickém fondu k dispozici databází (viz obrázek 1).

![Zřizování životního cyklu Umbraco](./media/sql-database-implementation-umbraco/figure1.png)

Obrázek 1. Zřizování životní cyklus Umbraco jako služba (UaaS)

## <a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure elastické fondy a automatizace zjednodušit nasazení
S Azure SQL Database a jinými službami Azure Umbraco zákazníci mohou samoobslužně zřizovat jejich prostředí a Umbraco můžete snadno sledovat a spravovat databáze v rámci intuitivní pracovního postupu:

1. Zřídit
   
   Umbraco udržuje kapacitou 200 dostupné předem zřízená databáze z elastické fondy. Pokud nový zákazník zaregistruje UaaS, Umbraco poskytuje hello zákazníka pomocí nového prostředí CMS skoro v reálném čase přiřazením databázi z hello dostupnosti fondu.
   
   Když fond dostupnosti dosáhne prahové hodnoty, se vytvoří nový elastický fond a nové databáze jsou předem zřízená toobe přiřazené toocustomers podle potřeby.
   
   Implementace je plně automatizovat pomocí správy knihovny jazyka C# a fronty Azure Service Bus.
2. Využívat
   
   Zákazníci použijte jeden toothree prostředí (pro produkční, pracovní nebo vývoj), každý s svou vlastní databázi. V elastické fondy, které umožňuje efektivní škálování bez nutnosti tooover-provision Umbraco tooprovide jsou databáze zákazníka.
   
   ![Přehled Umbraco projektu](./media/sql-database-implementation-umbraco/figure2.png)
   
   ![Umbraco podrobností projektu](./media/sql-database-implementation-umbraco/figure3.png)
   
   Obrázek 2. Web zákazníka Umbraco jako-Service (UaaS), zobrazující projektu přehled a podrobnosti
   
   Databáze SQL Azure používá jednotky transakcí databáze (Dtu) toorepresent hello relativní výkon požadované pro skutečné databázové transakce. Pro zákazníky UaaS databáze obvykle fungují v asi 10 Dtu, ale každá z nich má hello pružnost tooscale na vyžádání. To znamená, že UaaS můžete zajistit, že zákazníci potřebné prostředky, vždy mají i během špiček. Například během poslední události sportu nedělní došlo jednoho zákazníka UaaS databáze vrcholů až too100 Dtu pro dobu trvání hello ve hře hello. Azure elastické fondy ještě umožňují Umbraco toosupport této vysokého zatížení bez snížení výkonu.
3. Monitorování
   
   Umbraco monitorování databáze aktivity používat řídicí panely v rámci hello portál Azure, spolu s vlastní e-mailové výstrahy.
4. Zotavení po havárii
   
   Azure nabízí dvě možnosti zotavení po havárii (DR): aktivní geografickou replikaci a geografické obnovení. Hello možnost zotavení po Havárii, která společnost měli vybrat závisí na jeho [kontinuity podnikových procesů cíle](sql-database-business-continuity.md).
   
   aktivní geografickou replikací poskytuje nejrychlejší úroveň hello odpovědi v události hello výpadku. Používá aktivní geografickou replikaci, můžete vytvořit až toofour čitelný sekundární repliky na serverech v různých oblastech, a potom můžete spustit převzetí služeb při selhání tooany hello sekundárních databází v hello událostí selhání.
   
   Umbraco nevyžaduje geografická replikace, ale jeho využívat Azure geografické obnovení toohelp ověřte minimální výpadku v případě hello výpadku. geografické obnovení spoléhá na zálohování databáze v geograficky redundantní úložiště Azure. Když dojde k výpadku v hello primární oblasti, který umožňuje uživatelům toorestore ze záložní kopie.
5. Deaktivace přidělení
   
   Při odstranění prostředí projektu, se odeberou všechny přidružené databáze (vývoj, přípravné nebo za provozu) při čištění fronty Azure Service Bus. To automatizované procesu obnovení hello tooUmbraco nepoužívané databáze elastické databáze dostupnosti fondu, aby byly k dispozici pro budoucí zřizování při zachování maximální využití.

## <a name="elastic-pools-allow-uaas-tooscale-with-ease"></a>Elastické fondy povolit UaaS tooscale snadno
Využitím Azure elastické fondy, Umbraco, můžete optimalizovat výkon pro své zákazníky bez nutnosti tooover nebo snížení zřizování. Umbraco aktuálně má téměř 3000 databází mezi 19 elastické fondy, s tooeasily hello možnost škálování jako potřebné tooaccommodate jejich stávající zákazníky služby 325,000 nebo nové zákazníky, kteří jsou připravené toodeploy CMS v cloudu hello.

Ve skutečnosti podle tooMorten Christensen, technické vést v Umbraco, "UaaS nyní dochází k růstu asi 30 nové zákazníky za den. Naše zákazníky jsou ctí s výhodou hello je možné tooprovision nové projekty v sekundách, okamžitě publikování aktualizace tootheir za provozu lokalit z prostředí pro vývoj pomocí 'nasazení jedním kliknutím a změnit tak, jak rychle, pokud najdou chyby . "

Pokud zákazník nevyžaduje druhý a třetí prostředí už, ho můžete jednoduše odebrat tato prostředí. Který uvolní prostředky, které můžete využít jako součást hello fondu elastické databáze dostupnosti Umbraco ostatních zákazníků.

![Architektura nasazení služby Umbraco](./media/sql-database-implementation-umbraco/figure4.png)

Obrázek 3. Architektura UaaS nasazení v Microsoft Azure

## <a name="hello-path-from-datacenter-toocloud"></a>Cesta Hello z datacenter toocloud
Když vývojáři Umbraco hello původně hello rozhodnutí toomove tooa SaaS model, znal by, že potřebují toobuild nákladově efektivní a škálovatelné způsob, jak se služba hello.

> "elastické fondy jsou vzájemně přizpůsobit pro naše SaaS nabídky vzhledem k tomu, že jsme vytočit kapacity nahoru a dolů podle potřeby. Zřizování je snadné a naše instalačního programu, abychom využití na maximální."
> 
> – Mortena Christensen, technické realizace, Umbraco
> 
> 

"Jsme chtěli toospend naše času na řešení problémů s našich zákazníků, není správu infrastruktury. Jsme chtěli toomake snadno naše zákazníky tooget hello maximální hodnotu "Niels Hartvig, zakladatele Umbraco je uveden. "Původně za hostitelské servery hello, sebe, ale plánování kapacity by byl při důvodů." Shodou okolností Umbraco není využívat všechny správce databáze, které podtržítka nabídky hodnota klíče pro použití UaaS.

Jeden cíl důležité pro vývojáře Umbraco hello se tooprovide UaaS zákazníci tooprovision prostředí způsob, jak rychle a bez omezení kapacity. Ale za předpokladu, že vyhrazené hostovanou službu v datových centrech Umbraco by mít požadované spoustu přebytečnou kapacitou toohandle shluky ve zpracování. Který by chtěl přidání významnou výpočetní infrastrukturu, která by mít byla pravidelně nedostatečně využité.

Kromě toho hello Umbraco vývojový tým chtěli řešení, které by mohly tooreuse co nejvíc svůj existující kód míře. Jako vývojář Umbraco stavy Mikkel Madsen, "nám radostí s hello vývojové nástroje společnosti Microsoft, které nám již obeznámeni s, jako je Microsoft SQL Server, Microsoft Azure SQL Database, ASP.net a Internetová informační služba (IIS). Než začne investovat IaaS nebo PaaS cloudové řešení, jsme chtěli toomake se, že ho by podporují naše nástroje společnosti Microsoft a platformy, takže jsme nebude mít toomake velkých změn tooour kód základní. "

toomeet všechna její kritéria Umbraco hledán partnera cloudu s hello kvalifikaci následující:

* Dostatečnou kapacitu a spolehlivost
* Podpora pro vývojové nástroje společnosti Microsoft, tak, že Umbraco technici nebude vynutit toocompletely reinvent jejich vývojového prostředí
* Přítomnosti ve všech hello zeměpisné trhy, ve kterých UaaS bojuje (není-, jejich přístup k datům rychle a že jsou jejich data uložena v umístění, které splňuje jejich místní zákonné požadavky, bude podnikům nutné tooensure)

## <a name="why-umbraco-chose-azure-for-uaas"></a>Proč Umbraco zvolili Azure pro UaaS
Podle tooMorten Christensen "po zvážení všech našich možností, jsme vybrali Azure vzhledem k tomu, že se splní všechny naše kritéria, možnosti správy a toofamiliarity škálovatelnost a nákladová efektivnost. Nastavení prostředí hello na virtuálních počítačích Azure a každé prostředí má svou vlastní instanci databáze SQL Azure s všechny instance hello v elastické fondy. Oddělením databází mezi vývoj, pracovní a za provozu prostředí můžete nabízí svým zákazníkům robustní výkonu izolace shodná tooscale – velký win. "

Pokračuje Mortena, "před, měli jsme tooprovision servery pro webové databáze ručně. Teď nemáme toothink o něm. Všechno, co je automatizováno – od zřízení toocleanup. "

Mortena je také radostí s hello škálování poskytovaný platformou Azure. "elastické fondy jsou vzájemně přizpůsobit pro naše SaaS nabídky vzhledem k tomu, že jsme vytočit kapacity nahoru a dolů podle potřeby. Zřizování je snadné a naše instalačního programu, abychom využití na maximální." Stavy Mortena, "hello jednoduchost elastické fondy, společně s hello záruky pro na základě služeb vrstvy Dtu nám dává hello power tooprovision nové fondy zdrojů na vyžádání. Jeden z našich zákazníků větší nedávno Špičatá too100 Dtu ve svém prostředí za provozu. Použití Azure, naše elastické fondy zadat hello zákazníka databáze s hello prostředky, které budou potřebné, bez nutnosti toopredict DTU požadavky v reálném čase. Jednoduše řečeno, naše zákazníky získání hello čas, očekávané a jsme naplňují našich smluv o úrovni služeb výkonu."

Mikkel Madsen shrnuje ho: "jsme jste kterých je založena hello výkonné Azure algoritmus, který se připojuje běžné SaaS scénář (nové zákazníky pro registrace v reálném čase ve velkém měřítku) tooour vzor aplikací (předem zřizování databáze, i vývoj a za provozu) nad hello základní technologii (pomocí fronty Azure Service Bus ve spojení s Azure SQL Database)."

## <a name="with-azure-uaas-is-exceeding-customer-expectations"></a>S Azure UaaS přesahuje očekávání zákazníka
Od vyberete Azure jako jeho partnerský server pro cloud, byl Umbraco možné tooprovide UaaS zákazníkům optimálního výkonu správy obsahu, bez potřeby z vlastním hostováním řešení investice hello IT prostředků. Jako Mortena informacemi o tom, "jsme rádi hello vývojáře pohodlí a škálovatelnost, která nám dává Azure a našich zákazníků jsou thrilled s funkcemi hello a spolehlivosti. Celkově platí byla skvělé win pro nás!"

## <a name="more-information"></a>Další informace
* toolearn Další informace o Azure elastické fondy, najdete v části [elastické fondy](sql-database-elastic-pool.md).
* toolearn Další informace o Azure Service Bus, najdete v části [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* toolearn Další informace o webových rolí a rolí pracovního procesu, najdete v části [rolí pracovního procesu](../fundamentals-introduction-to-azure.md#compute).    
* toolearn Další informace o virtuální sítě, najdete v části [virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/).    
* toolearn Další informace o zálohování a obnovení, najdete v části [kontinuity podnikových procesů](sql-database-business-continuity.md).    
* toolearn Další informace o monitorování ppols, najdete v části [monitorování fondy](sql-database-elastic-pool-manage-portal.md).    
* toolearn Další informace o Umbraco jako služba, najdete v části [Umbraco](https://umbraco.com/cloud).

