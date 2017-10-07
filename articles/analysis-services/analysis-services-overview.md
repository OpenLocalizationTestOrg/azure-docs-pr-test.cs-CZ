---
title: aaaWhat je Azure Analysis Services | Microsoft Docs
description: "Získáte hello velký obrázek služby Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 83d7a29c-57ae-4aa0-8327-72dd8f00247d
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2017
ms.author: owend
ms.openlocfilehash: 48830a86f47a8ddc7770e6c44dd56c29927fe582
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-analysis-services"></a>Co je služba Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure Analysis Services poskytuje data podnikové úrovni modelování v cloudu hello. Je to plně spravovaná platforma jako služba (PaaS) integrovaná se službami datové platformy Azure. 

Se službou Analysis Services můžete provádět mashup a kombinování dat z více zdrojů, definovat metriky a zabezpečit svá data v jediném důvěryhodném sémantickém datovém modelu. datový model Hello poskytuje způsob snadnější a rychlejší pro vaše uživatele toobrowse masivní objemy dat pomocí klientské aplikace jako Power BI, Excel, služby Reporting Services, aplikace třetích stran a vlastní.

![Zdroje dat](./media/analysis-services-overview/aas-overview-data-sources.png)

Podívejte se na [toto video](https://sec.ch9.ms/ch9/d6dd/a1cda46b-ef03-4cea-8f11-68da23c5d6dd/AzureASoverview_high.mp4) toolearn jak Azure Analysis Services zapadá do společnosti Microsoft je celkové BI schopnosti a jak můžete využívat výhod získávání datové modely do cloudu hello.

## <a name="built-on-sql-server-analysis-services"></a>Vytvořeno na základě SQL Server Analysis Services
Služba Azure Analysis Services je kompatibilní s mnoha skvělými funkcemi, které už jsou ve službě SQL Server Analysis Services Enterprise Edition. Azure Analysis Services podporuje tabulkové modely na hello 1200 a 1400 [úrovně kompatibility](analysis-services-compat-level.md). Podporují se oddíly, zabezpečení na úrovni řádku, obousměrné relace i překlady. Režimy In-Memory a DirectQuery znamenají bleskově rychlé dotazy nad obrovskými a komplexními datovými sadami.

Tabulkové modely nabízejí rychlý vývoj a jsou vysoce přizpůsobitelné. Pro vývojáře zahrnují tabulkové modely objekty modelu toodescribe hello tabulkové objektu modelu (TNÍ). TNÍ je k dispozici ve formátu JSON prostřednictvím hello [tabulkový Model skriptovací jazyk (TMSL)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) a hello jazyk definice AMO data prostřednictvím hello [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx) oboru názvů.

## <a name="better-with-azure"></a>Lepší s Azure
Azure Analysis Services spolupracuje s řadou služeb Azure umožňuje toobuild pokročilé řešení pro analýzu. Integrace s [Azure Active Directory](../active-directory/active-directory-whatis.md) poskytuje zabezpečený přístup na základě rolí tooyour důležitá data. Integrovat [Azure Data Factory](../data-factory/data-factory-introduction.md) kanály zahrnutím aktivitu, která načte data do modelu hello. Můžete použít služby [Azure Automation](../automation/automation-intro.md) a [Azure Functions](../azure-functions/functions-overview.md) k prosté orchestraci modelů pomocí vlastního kódu.

## <a name="get-up-and-running-quickly"></a>Rychlé zprovoznění
Na webu Azure Portal můžete [vytvořit server](analysis-services-create-server.md) během několika minut. A pomocí PowerShellu a [šablon](../azure-resource-manager/resource-manager-create-first-template.md) Azure Resource Manageru můžete zřizovat servery s využitím deklarativní šablony. S jedinou šablonou můžete nasadit několik služeb společně s dalšími komponentami Azure, jako jsou účty úložiště nebo služba Azure Functions. 

Jakmile máte vytvořený server, můžete vytvořit tabulkový model přímo na webu Azure Portal. S novou hello (preview) [návrháře funkce webu](analysis-services-create-model-portal.md), můžete se připojit tooan databáze SQL Azure, Azure SQL Data Warehouse zdroje dat, nebo importovat soubor .pbix Power BI Desktop. Relace mezi tabulkami se vytvoří automaticky a můžete vytvořit míry nebo upravte hello model.bim soubor ve formátu json v pravém z prohlížeče.

## <a name="scale-tooyour-needs"></a>Škálování tooyour potřeb
Služba Azure Analysis Services je dostupná v úrovních Developer, Basic a Standard. V jednotlivých vrstvách náklady na plán lišit podle velikosti tooprocessing napájení, QPUs a paměti. Při vytváření serveru si vyberete plán na nějaké úrovni. Můžete změnit plány nebo dolů v rámci hello stejnou úroveň nebo upgradu tooa vyšší úroveň, ale nepovedlo se downgradovat z vyšší úrovně tooa nižší úroveň.

Vertikálně navyšujte nebo snižujte kapacitu vašeho serveru nebo ho pozastavte. Použít hello portál Azure nebo mít úplnou kontrolu na průběžně pomocí prostředí PowerShell. Platíte jenom za to, co používáte. toolearn Další informace o různých plánech hello a úrovně a použití hello cenové kalkulačky toodetermine hello správného plánu pro vás, najdete v části [ceník služby Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="keep-your-data-close"></a>Mějte data blízko sebe
Servery Azure Analysis Services mohou být vytvořeny v hello následující [oblastí Azure](https://azure.microsoft.com/regions/):

| Amerika | Evropa | Asie a Tichomoří |
|----------|--------|--------------|
|  Brazílie – jih<br> Střední Kanada<br> Východní USA 2<br> Střed USA – sever<br> Střed USA – jih<br> Západní střed USA<br> Západní USA | Severní Evropa<br> Spojené království – jih<br> Západní Evropa |   Austrálie – jihovýchod<br> Japonsko – východ<br> Jihovýchodní Asie<br> Indie – západ  |

Nové oblasti přidávané celou dobu hello, takže tento seznam mohou být neúplné. Umístění volíte při vytváření serveru na webu Azure Portal nebo pomocí šablon Azure Resource Manageru. tooget hello co nejlepší výkon, vyberte umístění nejbližší největší uživatelskou základnu. Zajistěte [vysokou dostupnost](analysis-services-bcdr.md) nasazením modelů na redundantních serverech ve více oblastech.

## <a name="migrate-your-existing-tabular-models"></a>Migrace existujících tabulkových modelů
Pokud již máte existující řešení SQL Server Analysis Services modelu místně, můžete migrovat tooAzure Analysis Services bez významné změny. toomigrate, můžete použít rozšíření SSDT toodeploy serveru tooyour modelu. Nebo můžete v aplikaci SSMS použít zálohování a obnovení nebo jazyk TMSL.

Pokud máte místní zdroje dat, je nutné tooinstall a nakonfigurovat [místní brána dat](analysis-services-gateway.md). Pokud máte role a členy role, které jsou již nakonfigurována, migrovat role, ale máte členy role tooreadd pomocí aplikace SSMS nebo prostředí PowerShell.

## <a name="connect-toopopular-data-sources"></a>Připojení zdroje dat toopopular
Podporuje Azure Analysis Services [připojení zdroje toodata](analysis-services-datasource.md) místní ve vaší organizaci a v cloudu hello. Kombinací dat z místních a cloudových zdrojů dat získáte hybridní řešení. 

Nové tabulkové modely 1400 pomocí funkce načíst Data moderní hello v sadě SSDT, na základě hello M vzorce dotazu jazyka. S načíst Data máte další transformaci dat a funkce hybridní webové aplikace a hello možnost toocreate a upravit vlastní rozšířené milion vzorce jazyka dotazů. Například u tabulkových modelů 1400 můžete modelovat na základě datových souborů ve službě Azure Blob Storage.

## <a name="use-hello-tools-you-already-know"></a>Použít hello nástroje, které už znáte

![Vývojářské nástroje BI](./media/analysis-services-overview/aas-overview-dev-tools.png)

#### <a name="sql-server-data-tools-ssdt-for-visual-studio"></a>SQL Server Data Tools (SSDT) pro Visual Studio
Vývoj a nasazení modelů pomocí hello volné [SQL Server Data Tools (SSDT) pro Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). Rozšíření SSDT zahrnuje šablony projektů Analysis Services, které vám pomůžou s rychlým zprovozněním. Rozšíření SSDT nyní zahrnuje hello moderní načíst Data zdroje dat dotazu a hybridní webové aplikace funkce pro tabulkové modely 1400. Pokud jste obeznámeni s načíst Data v Power BI Desktop a Excel 2016, již víte, jak je snadné toocreate vysoce uzpůsobeny dotazy na zdroj dat.

#### <a name="sql-server-management-studio"></a>SQL Server Management Studio
Spravujte servery a modelové databáze pomocí aplikace [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Připojte tooyour servery v cloudu hello. Spouštění skriptů TMSL přímo z okna dotazu hello XMLA a automatizaci úloh pomocí TMSL skriptů. Nové funkce se přidávají rychle – aplikace SSMS se aktualizuje každý měsíc.

#### <a name="powershell"></a>PowerShell
Úlohy správy serveru prostředků jako je vytváření servery, pozastavení nebo obnovení operací serveru nebo změna úrovně služby hello (vrstvy) použít rutiny Azure Resource Manager (AzureRM). Další úlohy správy databáze například přidáním nebo odebráním členy role, zpracování, nebo spouštění skriptů TMSL použít rutiny v modulu SqlServer hello. Moduly AzureRM a SQL Server jsou k dispozici v hello [Galerie prostředí PowerShell](https://www.powershellgallery.com/).


## <a name="your-data-is-secure"></a>Vaše data jsou v bezpečí
![Vizualizace dat](./media/analysis-services-overview/aas-overview-secure.png)

#### <a name="authentication"></a>Ověřování
Ověřování uživatelů pro Azure Analysis Services zařizuje služba [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Při pokusu o toolog v databázi tooan Azure Analysis Services, použijte uživatelé identity účet organizace s databází access toohello se pokoušíte tooaccess. Tyto identit uživatelů musí být členy hello výchozí Azure Active Directory pro předplatné hello, kde je umístěn server hello Azure Analysis Services. Další, najdete v části toolearn [ověřování a uživatel oprávnění](analysis-services-manage-users.md).

#### <a name="data-security"></a>Zabezpečení dat
Azure Analysis Services využívá úložiště toopersist úložiště objektů Azure Blob a metadat pro databáze služby Analysis Services. Datové soubory jsou v rámci objektu Blob šifrované pomocí šifrování na straně serveru Azure Blob. Při použití režimu přímých dotazů se ukládají jenom metada. skutečná data Hello přistupuje ze zdroje dat hello v době dotazu.

#### <a name="on-premises-data-sources"></a>Místní zdroje dat
Zabezpečený přístup toodata zdržují na místě ve vaší organizaci se dosahuje instalování a konfigurování [místní brána dat](analysis-services-gateway.md). Brány zadejte toodata přístup pro přímý dotaz a režimy v paměti. Při model Azure Analysis Services připojí tooan zdroj dat na místě, se společně s hello šifrovat přihlašovací údaje pro zdroj dat pro místní hello vytvoří dotazu. cloudové službě Brána pro Hello analyzuje hello dotazu a nabízených oznámení hello požadavek tooan Azure Service Bus. místní brána Hello dotazuje hello Azure Service Bus pro žádosti čekající na vyřízení. Brána Hello pak získá hello dotazu, dešifruje hello přihlašovací údaje a připojí toohello zdroj dat pro spuštění. Hello výsledky jsou pak odeslané ze zdroje dat hello, zpět toohello brány a potom na toohello Azure Analysis Services databáze.

Azure Analysis Services se řídí hello [Microsoft Online Services podmínky](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) a hello [prohlášení o ochraně osobních údajů služeb Microsoft Online](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
toolearn Další informace o zabezpečení Azure, najdete v části hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="supports-hello-latest-client-tools"></a>Podporuje hello nejnovější klienta nástroje
![Vizualizace dat](./media/analysis-services-overview/aas-overview-clients.png)

Moderní nástroje pro zkoumání a vizualizaci dat, jako jsou Power BI, Excel a nástroje třetích stran, poskytují uživatelům vysoce interaktivní a vizuálně bohaté přehledy o datech modelu.

Klienti používají MSOLAP, AMO nebo ADOMD [klientské knihovny](analysis-services-data-providers.md) tooconnect tooAnalysis služby servery. Klientské aplikace Microsoftu jako Power BI Desktop a Excel instalují všechny tři klientské knihovny. Ale mějte na paměti, v závislosti na verzi hello nebo četnost aktualizací, knihovny klienta nemusí být hello nejnovější verze vyžadované službou Azure Analysis Services. Hello totéž platí i toocustom aplikace nebo dalších rozhraní, jako je například AsCmd, TNÍ, ADOMD.NET. Tyto aplikace obvykle vyžadují ruční instalaci hello knihovny jako součást balíčku.


## <a name="get-help"></a>Podpora

#### <a name="documentation"></a>Dokumentace
Azure Analysis Services je jednoduchý tooset nahoru a toomanage. Můžete najít všechny informace o hello potřebujete toocreate a spravovat váš server services. Při vytváření datového modelu toodeploy tooyour serveru, má se stejné mnohem hello, protože je pro vytvoření datového modelu nasazení tooan na místním serveru. [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services) obsahuje rozsáhlou knihovnu článků s koncepty, postupy, kurzy a odkazy.

#### <a name="videos"></a>Videa
Užitečná videa najdete v části [Azure Analysis Services na webu Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

#### <a name="blogs"></a>Blogy
Všechno se rychle mění. Vždy získáte hello nejnovější informace o hello [blogu týmu služby Analysis Services](https://blogs.msdn.microsoft.com/analysisservices/) a [Azure blog](https://azure.microsoft.com/blog/).

#### <a name="community"></a>Komunita
Služba Analysis Services má velmi aktivní komunitu uživatelů. Připojení hello konverzace na [fórum Azure Analysis Services](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Váš názor
Máte návrhy nebo požadavky na funkce? Nacházet zda tooleave komentáře na [Azure Analysis Services zpětné vazby](https://aka.ms/azureanalysisservicesfeedback).

Máte návrhy o dokumentaci hello? Můžete přidat komentář pomocí Livefyre dole hello jednotlivých článků.

## <a name="next-steps"></a>Další kroky
Teď, když víte, informace o Azure Analysis Services, je čas spuštění tooget. Zjistěte, jak příliš[vytvořit server](analysis-services-create-server.md) v Azure. Pokud váš server je připraven, krok prostřednictvím hello [společnosti Adventure Works kurzu](tutorials/aas-adventure-works-tutorial.md) toolearn jak toocreate plně funkční tabulkový model a nasaďte ho tooyour serveru.
