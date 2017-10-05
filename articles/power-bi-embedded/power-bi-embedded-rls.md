---
title: "Zabezpečení na úrovni řádků s Power BI Embedded"
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
ms.openlocfilehash: 1cde5b9ee4c716af07d427d4d0eb3f0775d456ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="row-level-security-with-power-bi-embedded"></a>Zabezpečení na úrovni řádků v Power BI Embedded

Zabezpečení na úrovni řádků (RLS) slouží k omezení přístupu uživatelů k konkrétní dat v rámci sestavu nebo datovou sadu, povolení pro několik různých uživatelům používat stejné sestavě při všech dat vidět jiné. Power BI Embedded teď podporuje datové sady, které jsou nakonfigurované s RLS.

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

Aby bylo možné využít výhod RLS, je důležité, že rozumíte tři hlavní koncepty; Uživatelé, role a pravidla. Podívejme bližší pohled na každý:

**Uživatelé** – tyto jsou skutečné koncovým uživatelům prohlížení sestav. V Power BI Embedded uživatelé se identifikují podle vlastnosti uživatelské jméno v tokenu aplikace.

**Role** – uživatelé patří do role. Role je kontejner pro pravidla a může mít název něco jako "Vedoucí prodeje" nebo "Obchodního zástupce". V Power BI Embedded uživatelé se identifikují podle vlastnosti rolí v tokenu aplikace.

**Pravidla** – role mají pravidla a tato pravidla jsou skutečné filtry, které se chystáte použít k datům. To může být stejně jednoduché jako "země = USA" nebo něco víc dynamické.

### <a name="example"></a>Příklad

Pro zbývající část tohoto článku poskytujeme příklad vytváření RLS a využívání, v rámci aplikace embedded. Naše Ukázka používá [prodejní analýzy ukázka](http://go.microsoft.com/fwlink/?LinkID=780547) soubor PBIX.

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

Naše ukázka prodejní analýzy prodejů pro všechny úložiště v určité maloobchodní řetězec. Bez RLS, bez ohledu na to, které oblastní správce přihlásí a zobrazení sestavy, uvidí stejná data. Vedoucí pracovníci určil každý oblastní manager byste měli vidět jenom prodeje pro úložiště, které spravují a k tomu můžeme použít RLS.

RLS je vytvořené v Power BI Desktop. Po otevření datovou sadu a sestavu jsme můžete přepnout do zobrazení diagramu zobrazíte schéma:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

Zde je několik věcí Všimněte s toto schéma:

* Všechny míry, jako například **celkový prodej**, jsou uloženy v **prodej** tabulky faktů.
* Existují čtyři další dimenze související tabulky: **položky**, **čas**, **úložiště**, a **oblastní**.
* Šipky v řádcích vztah znamenat toho, jak filtry můžete toku z jedné tabulky do jiné. Například, pokud filtr je umístěn na **čas [datum]**, v aktuálním schématu bude pouze filtrovat dolů hodnoty **prodej** tabulky. Žádné jiné tabulky by ovlivnila tento filtr, protože všechny šipek na řádcích vztah bod registrace k tabulce prodeje a není rychle.
* **Oblastní** tabulka udává, který správce je pro každou oblast:
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

Založené na tomto schématu v případě, že jsme použít filtr pro **oblastní Manager** sloupce v tabulce oblastní, a pokud tento filtr odpovídá uživatele zobrazení sestavy, bude tento filtr také filtrovat dolů **úložiště** a **prodej** tabulky jenom zobrazit data pro tento konkrétní oblastní správce.

Tady je jak:

1. Na kartě modelování klikněte na **spravovat role**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. Vytvoření nové role s názvem **Manager**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. V **oblastní** tabulky zadejte následující výraz DAX: **[oblastní Manager] = USERNAME()**  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. Abyste měli jistotu, pravidla pracují na **modelování** , klikněte na **zobrazení jako role**a potom zadejte následující:  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   Sestavy se nyní zobrazí data, jako kdyby byly přihlášení jako **Andrew Ma**.

Použití filtru způsob, jakým jsme se zde bude filtrovat dolů všechny záznamy v **oblastní**, **úložiště**, a **prodej** tabulky. Ale kvůli směr filtru na vztahy mezi **prodej** a **čas**, **prodej** a **položky**, a **položky** a **čas** tabulky se nebudou filtrovat dolů.

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

Které mohou být ok pro tento požadavek, ale pokud jsme nechcete, aby správce, kteří mají zobrazovat položky, pro které nemají žádné prodeje, jsme může zapněte obousměrné křížové filtrování pro relaci a toku zabezpečení filtr v obou směrech. To lze provést tak, že upravíte vztah mezi **prodej** a **položky**, podobné výjimky:

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

Teď, může také toku filtry z tabulky prodeje pro **položky** tabulky:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

> [!NOTE]
> Pokud používáte režim DirectQuery pro data, musíte povolit obousměrné křížové filtrování výběrem tyto dvě možnosti:

1. **Soubor** -> **možnosti a nastavení** -> **funkce verze Preview** -> **povolit křížové filtrování v obou směrech DirectQuery**.
2. **Soubor** -> **možnosti a nastavení** -> **DirectQuery** -> **Povolit neomezené míry v režimu DirectQuery**.

Další informace o obousměrné křížové filtrování, stáhněte si [obousměrné křížové filtrování v SQL Server Analysis Services 2016 a Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) dokument White Paper.

Tím se zabalí do všech práci, kterou je třeba provést v Power BI Desktop, ale je, že jeden další část práce, která je potřeba zajistit, aby RLS pravidel, že jsme definovali pracovní v Power BI Embedded. Uživatelé se ověří a autorizuje vaše aplikace a tokeny aplikací slouží k udělení tohoto přístupu uživatelů k konkrétní sestavy Power BI Embedded. Power BI Embedded nemá žádné konkrétní informace, na který je vaše uživatele. Pro RLS pro práci budete muset předat některé další kontext jako součást tokenu vaší aplikace:

* **uživatelské jméno** (volitelné) – používá se s RLS Toto je řetězec, který slouží k identifikaci uživatele při použití pravidla RLS. V části používání řádek zabezpečení na úrovni s Power BI Embedded
* **role** – řetězec obsahující rolí vyberte při použití pravidla zabezpečení na úrovni řádků. Pokud předávání víc než jedné role, musí být předán jako pole řetězců.

Vytvoření tohoto tokenu pomocí [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) metoda. Pokud se vlastnost username nachází, je třeba alespoň jednu hodnotu předat také v rolích.

Například můžete změnit EmbedSample. Mohlo dojít k aktualizaci řádku DashboardController 55 z

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

na

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

Tokenu úplné aplikace bude vypadat přibližně takto:

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

Nyní s všechny části společně, při přihlášení do aplikace pro zobrazení této sestavy pouze budou moci zobrazit data, která mohou zobrazit, jak jsou definovány pomocí našich zabezpečení na úrovni řádků.

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Viz také

[Zabezpečení na úrovni řádků (RLS) s výkonem](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Chcete se ještě na něco zeptat? [Vyzkoušejte komunitu Power BI](http://community.powerbi.com/)

