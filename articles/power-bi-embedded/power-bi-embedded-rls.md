---
title: "zabezpečení na úrovni aaaRow s Power BI Embedded"
description: "Podrobnosti o zabezpečení na úrovni řádků s Power BI Embedded"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 7936ade5-2c75-435b-8314-ea7ca815867a
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 384f78826ecc710cf8f101b251ae68b074f3e98b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="row-level-security-with-power-bi-embedded"></a>Zabezpečení na úrovni řádků v Power BI Embedded

Zabezpečení na úrovni řádků (RLS) lze použít toorestrict uživatel přístup tooparticular data v rámci sestavu nebo datovou sadu, které umožní více různým uživatelům toouse hello stejné sestavě při všechny vidět různá data. Power BI Embedded teď podporuje datové sady, které jsou nakonfigurované s RLS.

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

V pořadí tootake výhod RLS je důležité, že rozumíte tři hlavní koncepty; Uživatelé, role a pravidla. Podívejme bližší pohled na každý:

**Uživatelé** – tyto prohlížíte skutečné koncoví uživatelé hello sestavy. V Power BI Embedded uživatelé se identifikují podle vlastnosti hello uživatelské jméno v tokenu aplikace.

**Role** – uživatelé patří tooroles. Role je kontejner pro pravidla a může mít název něco jako "Vedoucí prodeje" nebo "Obchodního zástupce". V Power BI Embedded uživatelé se identifikují podle vlastnosti rolí hello v tokenu aplikace.

**Pravidla** – role mají pravidla a tato pravidla jsou hello skutečné filtry, které budou data toohello toobe použít. To může být stejně jednoduché jako "země = USA" nebo něco víc dynamické.

### <a name="example"></a>Příklad

Pro hello zbývající části tohoto článku poskytujeme příklad vytváření RLS a využívání, v rámci aplikace embedded. Naše Ukázka používá hello [prodejní analýzy ukázka](http://go.microsoft.com/fwlink/?LinkID=780547) soubor PBIX.

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

Naše ukázka prodejní analýzy prodejů pro všechny hello úložiště v určité maloobchodní řetězec. Bez RLS bez ohledu na to, které oblastní správce přihlásí a zobrazení hello sestavy, se zobrazí hello stejná data. Vedoucí pracovníci bylo zjištěno, že každý správce oblastní byste měli vidět jenom hello prodeje pro hello úložiště, které spravují a toodo to, můžeme použít RLS.

RLS je vytvořené v Power BI Desktop. Po otevření hello datovou sadu a sestavu jsme přepínači toodiagram zobrazení toosee hello schématu:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

Tady je několik věcí toonotice s toto schéma:

* Všechny míry, jako například **celkový prodej**, jsou uloženy v hello **prodej** tabulky faktů.
* Existují čtyři další dimenze související tabulky: **položky**, **čas**, **úložiště**, a **oblastní**.
* šipky Hello na řádky vztah hello označují toho, jak filtry můžete toku z jedné tabulky tooanother. Například, pokud filtr je umístěn na **čas [datum]**, v aktuálním schématu hello bude pouze filtrovat dolů hodnoty v hello **prodej** tabulky. Žádné jiné tabulky by ovlivnila tento filtr od všech hello šipek hello vztah řádky bodu toohello prodeje tabulky a není rychle.
* Hello **oblastní** tabulka udává, který správce hello je pro každou oblast:
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

Založené na tomto schématu v případě, že jsme použít filtr toohello **Manager oblastní** sloupec v hello oblastní tabulky, a pokud tento filtr odpovídá hello uživatele zobrazení hello sestavy, bude tento filtr také filtrovat dolů hello **úložiště**a **prodej** tabulky tooonly zobrazit data pro tento konkrétní oblastní správce.

Tady je jak:

1. Na kartě hello modelování, klikněte na tlačítko **spravovat role**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. Vytvoření nové role s názvem **Manager**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. V hello **oblastní** tabulky zadejte následující výraz jazyka DAX hello: **[oblastní Manager] = USERNAME()**  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. zda pravidla hello toomake pracují na hello **modelování** , klikněte na **zobrazení jako role**a potom zadejte následující hello:  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   Hello sestavy se nyní zobrazí data jako kdyby byly přihlášení jako **Andrew Ma**.

Použití filtru hello, způsob hello jsme se zde bude filtrovat dolů všechny záznamy v hello **oblastní**, **úložiště**, a **prodej** tabulky. Ale kvůli hello směr filtru na hello vztahy mezi **prodej** a **čas**, **prodej** a **položky**a **Položky** a **čas** tabulky se nebudou filtrovat dolů.

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

Které mohou být ok pro tento požadavek, ale pokud jsme nechcete, aby správci toosee položky, pro které nemají žádné prodeje, jsme může zapněte obousměrné křížové filtrování hello vztah a toku hello zabezpečení filtr v obou směrech. To lze provést tak, že upravíte hello vztah mezi **prodej** a **položky**, podobné výjimky:

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

Teď, může také toku filtry z hello prodej tabulky toohello **položky** tabulky:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

> [!NOTE]
> Pokud používáte režim DirectQuery pro data, budete potřebovat tooenable obousměrného křížové filtrování výběrem tyto dvě možnosti:

1. **Soubor** -> **možnosti a nastavení** -> **funkce verze Preview** -> **povolit křížové filtrování v obou směrech DirectQuery**.
2. **Soubor** -> **možnosti a nastavení** -> **DirectQuery** -> **Povolit neomezené míry v režimu DirectQuery**.

Další informace o obousměrné křížové filtrování, stažení hello toolearn [obousměrné křížové filtrování v SQL Server Analysis Services 2016 a Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) dokument White Paper.

Tím se zabalí do všech hello práci, kterou toobe v Power BI Desktop, ale existuje jeden další část práce, která potřebuje provést toobe toomake hello RLS pravidla jsme definovali pracovní v Power BI Embedded. Uživatelé se ověří a autorizuje vaše aplikace a tokeny aplikací jsou použité toogrant tohoto uživatele tooa konkrétní Power BI Embedded sestavu přístupu. Power BI Embedded nemá žádné konkrétní informace, na který je vaše uživatele. Pro RLS toowork budete potřebovat toopass některé další kontext jako součást tokenu vaší aplikace:

* **uživatelské jméno** (volitelné) – používá se s RLS Toto je řetězec, který lze použít toohelp identifikovat hello uživatele při použití pravidla RLS. V části používání řádek zabezpečení na úrovni s Power BI Embedded
* **role** – řetězec obsahující hello role tooselect při použití pravidla zabezpečení na úrovni řádků. Pokud předávání víc než jedné role, musí být předán jako pole řetězců.

Vytvoření tokenu hello pomocí hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) metoda. Pokud vlastnost username hello je k dispozici, je třeba alespoň jednu hodnotu předat také v rolích.

Můžete například změnit hello EmbedSample. Mohlo dojít k aktualizaci řádku DashboardController 55 z

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

na

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

tokenu úplné aplikace Hello bude vypadat přibližně takto:

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

Nyní se všechny kusy hello společně, při přihlášení do našich tooview aplikace této sestavy pouze budou mít toosee hello data, mohou toosee, podle definice naše zabezpečení na úrovni řádků.

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Viz také

[Zabezpečení na úrovni řádků (RLS) s výkonem](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Chcete se ještě na něco zeptat? [Zkuste hello komunitě Power BI](http://community.powerbi.com/)

