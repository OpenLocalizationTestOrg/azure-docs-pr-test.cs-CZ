---
title: "Vytváření sestav v Azure Active Directory automatické uživatelský účet zřizování aplikace tooSaaS | Microsoft Docs"
description: "Zjistěte, jak toocheck hello Stav zřizování úlohy automatické uživatelský účet a jak tootroubleshoot hello zřizování jednotlivé uživatele."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: asmalser-msft
ms.openlocfilehash: 5dcf9e5dbaacf3a2c81183c5d81e331858671b86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-reporting-on-automatic-user-account-provisioning"></a>Kurz: Generování sestav na uživatele automatické zřizování účtu


Azure Active Directory zahrnuje [uživatelský účet zřizování služby](active-directory-saas-app-provisioning.md) která pomáhá automatizovat hello zřizování zrušte zřizování uživatelských účtů v aplikace SaaS a jiných systémů, za účelem hello identity začátku do konce cyklu Správa. Azure AD podporuje zřizování konektory pro všechny aplikace hello a systémy v části "Doporučený" hello hello předem integrovaných uživatelů [galerii aplikací Azure AD](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured).

Tento článek popisuje, jak toocheck hello Stav zřizování úlohy po byl nastaven a jak tootroubleshoot hello zřizování jednotlivých uživatelů a skupin.

## <a name="overview"></a>Přehled

Zřizování konektory jsou primárně nastavit a konfigurovat pomocí hello [portál pro správu Azure](https://portal.azure.com), pomocí následujících hello [poskytuje dokumentaci](active-directory-saas-tutorial-list.md) aplikace hello kde uživatelský účet zřizování se požaduje. Jakmile nakonfigurovaná a spuštěná, mohou být zřizování úlohy pro aplikaci oznámeny na pomocí jedné ze dvou způsobů:

* **Portál pro správu Azure** – Tento článek popisuje především načítání informací sestavy z hello [portál pro správu Azure](https://portal.azure.com), která nabízí zřizování souhrnnou sestavu jak podrobné zřizování protokoly pro danou aplikaci auditu.

* **Audit rozhraní API** – Azure Active Directory také poskytuje auditu rozhraní API, že umožňuje programový načtení hello podrobné zřizování protokoly auditu. V tématu [auditování Azure Active Directory referenční dokumentace rozhraní API](active-directory-reporting-api-audit-reference.md) pro konkrétní toousing dokumentace toto rozhraní API. Když tento článek nepopisuje konkrétně jak toouse hello rozhraní API, podrobnosti hello typy zřizování události, které se zaznamenávají do protokolu auditování hello.

### <a name="definitions"></a>Definice

Tento článek používá hello následující podmínky, definovaná níže:

* **Zdroj systému** -hello úložiště uživatelů, kteří hello zřizování služby Azure AD se synchronizuje z. Azure Active Directory je hello zdrojovém systému pro většinu hello předem integrovaných zřizování konektory, ale existují některé výjimky (Příklad: Workday příchozí synchronizace).

* **Cíl systému** -hello úložiště uživatelů, kteří hello zřizování služby Azure AD synchronizuje k. To je obvykle SaaS aplikace (příklady: Salesforce, ServiceNow, Google Apps, Dropbox pro firmy), ale v některých případech může být v místním systému, jako je Active Directory (Příklad: tooActive Workday příchozí synchronizace adresáře).


## <a name="getting-provisioning-reports-from-hello-azure-management-portal"></a>Získávání zřizování sestavy z portálu správy Azure hello

tooget zřizování informací sestavy pro danou aplikaci, počáteční spuštěním hello [portál pro správu Azure](https://portal.azure.com) a procházení toohello podniková aplikace, pro který je nakonfigurovaný zřizování. Například pokud zřizujete uživatelé tooLinkedIn zvýšení, je hello navigační cesta toohello podrobností o aplikaci:

**Azure Active Directory > podnikové aplikace, které > všechny aplikace > LinkedIn zvýšení oprávnění**

Tady hello zřizování souhrnnou sestavu a zřizování protokoly auditu hello k dispozici, i popsané dole.


### <a name="provisioning-summary-report"></a>Zřizování souhrnnou sestavu

Hello zřizování souhrnné sestavy se zobrazí na hello **zřizování** kartě pro danou aplikaci. Nachází se v části Podrobnosti o synchronizaci hello pod **nastavení**a poskytuje hello následující informace:

* Hello celkový počet uživatelů a / skupiny, byly synchronizované a jsou momentálně v oboru pro zřizování mezi systémem hello zdrojový a cílový systém hello.

* Hello poslední čas hello synchronizace byla spuštěna. Synchronizace většinou dochází každých 20 40 minut, po dokončení úplné synchronizace.

* Zda byla dokončena počáteční úplná synchronizace.

* Hello procesu zřizování, jestli má byly umístěny do karantény a jaké hello důvod stavu karantény hello je například (selhání toocommunicate s cílovým systémem kvůli tooinvalid přihlašovací údaje správce)

Hello zřizování souhrnnou sestavu by měl být hello první místní správci vzhled toocheck na hello provozní stav úlohy zřizování hello.

 ![Souhrnná sestava](./media/active-directory-saas-provisioning-reporting/summary_report.PNG)

### <a name="provisioning-audit-logs"></a>Protokoly auditu a zřizování
Všechny aktivity provedené hello zřizování služby se zaznamenávají do protokolů auditu hello Azure AD, které lze zobrazit v hello **protokoly auditu** kartě pod hello **zřizování účtu** kategorie. Typy událostí protokolu aktivit patří:

* **Import události** – událost "import" se zaznamenává pokaždé, když služba zřizování hello Azure AD načte informace o jednotlivé uživatele nebo skupiny ze zdrojového systému nebo cílového systému. Při synchronizaci uživatelů se načítají z hello zdrojovém systému nejprve s hello výsledky. zaznamenává jako "import" události. Hello odpovídající ID hello načíst uživatelů jsou potom dotaz proti hello cílový systém toocheck, pokud existují, s hello výsledky. zaznamenává taky jako "import" události. Tyto události zaznamenejte všechny atributy mapovat uživatele a jejich hodnoty, které se zobrazily zřizování službou hello Azure AD během hello hello události. 

* **Události pravidlo synchronizace** – tyto události sestav o výsledcích hello hello atribut mapování pravidel a žádné nakonfigurované oboru filtrů, po importu a vyhodnocují na základě zdrojové a cílové systémy hello dat uživatele. Například pokud uživatel ve zdrojovém systému se považují za toobe v oboru pro zřizování a domnělého toonot existovat v cílovém systému hello, pak tato událost zaznamenává, hello uživatele se zřídí v cílovém systému hello. 

* **Export události** -pokaždé, když uživatel účet nebo skupinu objekt tooa cílový systém zapíše zřizování služby hello Azure AD se zaznamená událost "export". Tyto události zaznamenejte všechny atributy uživatelů a jejich hodnoty, které byly sepsány podle hello zřizování služby Azure AD během hello hello události. Pokud došlo k chybě při zápisu hello uživatelského účtu nebo skupiny objekt toohello cílového systému, zobrazí se zde.

* **Zpracování událostí úschově** -proces escrows dojít, když hello zřizování služby dojde k chybě při pokusu o operaci a zahájí operaci hello tooretry na intervalu back mimo dobu. Operace zřizování vyřazenou pokaždé, když se zaznamená událost "úschově".

Při prohlížení zřizování události pro jednotlivé uživatele, dojde k událostem hello normálně v tomto pořadí:

1. Import událostí: uživatel se načítají ze zdrojového systému hello.

2. Import událostí: cílovém systému je předmětem dotazu toocheck hello existence hello načíst uživatele.

3. Událost pravidlo synchronizace: uživatelská data ze zdrojové a cílové systémy jsou porovnán s atribut hello nakonfigurovaná pravidla mapování a rozsahu toodetermine filtry, jaké akce, pokud existuje, je třeba provést.

4. Export událostí: Pokud hello synchronizační pravidlo událostí závisí, že má být akce provést (např. přidat, Update, Delete), pak výsledky hello hello akce se zaznamenávají v události Export.

![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-provisioning-reporting/audit_logs.PNG)


### <a name="looking-up-provisioning-events-for-a-specific-user"></a>Vyhledávání zřizování události pro konkrétního uživatele

Hello nejběžnější případ použití pro hello zřizování protokoly auditu je toocheck hello zřizování stav jednotlivých uživatelských účtů. toolook hello poslední zřizování událostí pro konkrétního uživatele:

1. Přejděte toohello **protokoly auditu** části.

2. Z hello **kategorie** nabídce vyberte možnost **zřizování účtu**.

3. V hello **rozsah** nabídky, vyberte hello rozsah chcete toosearch,

4. V hello **vyhledávání** panel, zadejte ID uživatele hello hello uživatele chcete toosearch pro. Hodnota ID Hello formát by měl odpovídat ať jste vybrali jako hello primární odpovídající ID v mapování atributů hello (například userPrincipalName nebo ID číslo zaměstnance). požadovanou hodnotu ID Hello budou viditelné ve sloupci hello cíle (cílů).

5. Toosearch stiskněte klávesu Enter. Hello nejnovější zřizování události bude vrácena jako první.

6. Pokud jsou vráceny události, poznamenejte si typy aktivit hello a jestli byla úspěšná nebo neúspěšná. Pokud se žádné výsledky, pak znamená hello uživatel buď neexistuje, nebo nebylo dosud zjištěno hello procesu zřizování, pokud zatím není dokončený úplnou synchronizaci.

7. Kliknutím na jednotlivé události tooview Rozšířené podrobnosti, včetně všech vlastnosti uživatele, které byly načteny, vyhodnotí nebo zapsat jako součást hello událostí.


### <a name="tips-for-viewing-hello-provisioning-audit-logs"></a>Tipy pro zobrazení hello zřizování protokoly auditu

Nejlepší čitelnější v hello portálu Azure vyberte hello **sloupce** tlačítko a vyberte tyto sloupce:

* **Datum** -ukazuje hello datum hello událostí došlo k chybě.
* **Cíle (cílů)** -ukazuje hello aplikaci název a uživatelské ID, které jsou témata hello hello události.
* **Aktivita** -hello typ aktivity, jak je popsáno výše.
* **Stav** – ať hello událostí bylo úspěšné, nebo ne.
* **Důvod stavu** – souhrn co se stalo v hello zřizování událostí.


## <a name="troubleshooting"></a>Řešení potíží

Hello zřizování souhrnné sestavy a auditu protokoly hrát klíčovou roli pomáhá správci řešení potíží s různými uživatelský účet zřizování problémy.

Na základě scénáře návod tootroubleshoot automatické zřizování uživatelů, najdete v části [problémy konfigurace a zřizování uživatelů tooan aplikace](active-directory-application-provisioning-content-map.md).


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
