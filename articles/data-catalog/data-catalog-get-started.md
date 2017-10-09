---
title: "aaaGet spuštění s katalogem Data Catalog | Microsoft Docs"
description: "Začátku do konce kurz představující scénáře hello a možnosti služby Azure Data Catalog."
documentationcenter: 
services: data-catalog
author: steelanddata
manager: jhubbard
editor: 
tags: 
ms.assetid: 03332872-8d84-44a0-8a78-04fd30e14b18
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 7652918b5a8254f5cff9e32d77b1fd3e1bf59d59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-catalog"></a>Začínáme s Azure Data Catalog
Azure Data Catalog je plně spravovaná cloudová služba, která slouží jako systém pro registraci a zjišťování podnikových datových assetů. Podrobný přehled naleznete v části [Co je Azure Data Catalog?](data-catalog-what-is-data-catalog.md).

V tomto kurzu se naučíte pracovat se službou Azure Data Catalog. Můžete provést následující postupy v tomto kurzu hello:

| Postup | Popis |
|:--- |:--- |
| [Zřízení datového katalogu](#provision-data-catalog) |V tomto postupu zřídíte nebo nastavíte službu Azure Data Catalog. Pouze v případě, že hello katalogu nebyla nastavena před, proveďte tento krok. Je povolen pouze jeden katalog dat na organizaci (doména služby Microsoft Azure Active Directory), a to i tehdy, když je k vašemu účtu Azure přiřazeno více předplatných. |
| [Registrace datových prostředků](#register-data-assets) |V tomto postupu zaregistrovat datových prostředků z hello AdventureWorks2014 ukázkové databáze s hello data catalog. Registrace je proces extrahování klíčových strukturálních metadat například názvy, typy a umístění, ze zdroje dat hello a kopírování tohoto katalogu toohello metadata hello. Hello zdroj dat a datové prostředky zůstanou kde jsou, ale hello metadata jsou používána toomake katalogu hello je snadněji zjistitelné a srozumitelnější. |
| [Zjišťování datových prostředků](#discover-data-assets) |V tomto postupu použijete hello Azure Data Catalog portálu toodiscover datové prostředky, které byly zaregistrovány v předchozím kroku hello. Po registraci zdroje dat s Azure Data Catalog, jeho metadata je indexovaný službou hello tak, aby uživatelé mohou snadno prohledávat hello dat, které potřebují. |
| [Přidání poznámek k datovým prostředkům](#annotate-data-assets) |V tomto postupu zadáte poznámky (informace o například popisy, značky, dokumentace nebo odborníky) pro hello datových prostředků. Tyto informace doplňují hello metadat extrahovaných ze zdroje dat hello a zdroje dat hello toomake nerozumí toomore lidem. |
| [Připojit toodata prostředky](#connect-to-data-assets) |V tomto postupu otevřete datové assety v integrovaných klientských nástrojích (např. Excel a SQL Server Data Tools) i neintegrovaném nástroji (SQL Server Management Studio). |
| [Správa datových prostředků](#manage-data-assets) |V tomto postupu nastavíte zabezpečení svých datových assetů. Data Catalog není uživatelům přístup k datům toohello sám sebe. Vlastník Hello zdroje dat hello řídí přístup k datům. <br/><br/> Pomocí katalogu Data Catalog, můžete zjistit zdroje dat a zobrazení hello **metadata** související zdroje toohello registrované v katalogu hello. Mohou nastat situace, ale kde zdroje dat musí být viditelná jenom toospecific uživatele nebo toomembers určitých skupin. Pro tyto scénáře můžete Data Catalog tootake vlastnictví registrované datové prostředky v rámci hello katalogu a řízení viditelnost hello hello prostředků, které vlastníte. |
| [Odebrání datových prostředků](#remove-data-assets) |V tomto postupu zjistíte, jak hello tooremove datových prostředků z katalogu data catalog. |

## <a name="tutorial-prerequisites"></a>Požadavky kurzu
### <a name="azure-subscription"></a>Předplatné Azure
tooset si Azure Data Catalog, musí být vlastníkem hello nebo spoluvlastník předplatného Azure.

Předplatná Azure můžete uspořádat přístup k prostředkům služby toocloud jako Azure Data Catalog. Zároveň vám pomohou řídit způsob, jak je používání prostředků vykazováno, fakturováno a placeno. Každé předplatné může mít jiné nastavení fakturace a plateb. Můžete tedy mít jiná předplatná a jiné plány pro různá oddělení, projekty, regionální pobočky a podobně. Každé cloudové služby patří tooa předplatné, je nutné toohave předplatné před nastavením Azure Data Catalog. Další, najdete v části toolearn [spravovat účty, odběry a správu role](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Pokud nemáte předplatné, můžete si během několika minut bezplatně vytvořit zkušební účet. Podrobnosti viz [bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

### <a name="azure-active-directory"></a>Azure Active Directory
tooset si Azure Data Catalog, musí podepsané se pomocí účtu uživatele Azure Active Directory (Azure AD). Musíte být vlastníkem hello nebo spoluvlastník předplatného Azure.  

Azure AD poskytuje snadný způsob pro obchodní toomanage identity a přístup v hello cloudové i místní. Můžete použít pracovní nebo školní účet toosign v cloudu tooany nebo místní webové aplikace. Azure Data Catalog používá přihlášení tooauthenticate Azure AD. Další, najdete v části toolearn [co je Azure Active Directory](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Konfigurace zásad Azure Active Directory
Mohou nastat situace, kde se můžete přihlásit toohello portálu Azure Data Catalog, ale když se pokusíte toosign v toohello nástroj registrace zdroje dat, dojde k chybovou zprávu, která brání přihlášení. Této chybě může dojít, pokud jste na podnikové síti hello nebo když se připojují z mimo hello podnikové síti.

Nástroj pro registraci Hello používá *ověřování pomocí formulářů* toovalidate uživatelská přihlášení s Azure Active Directory. Pro úspěšné přihlášení, musí správce služby Azure Active Directory povolit ověřování pomocí formulářů v hello *globální zásady ověřování*.

S hello globální zásady ověřování můžete povolit ověřování samostatně pro intranetu a extranetu připojení, jak ukazuje následující obrázek hello. Pokud je ověřování pomocí formulářů není povoleno pro hello síť, ze kterého se připojujete, může dojít k chybám přihlášení.

 ![Zásady globálního ověřování Azure Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

Další informace naleznete v článku o [konfiguraci zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Zřízení datového katalogu
Můžete zřídit pouze jeden katalog dat na organizaci (doména Azure Active Directory). Proto pokud hello vlastníka nebo spoluvlastník předplatné Azure, který patří toothis domény služby Azure Active Directory již vytvořen katalog, nebudete moct toocreate katalog znovu i v případě, že máte více předplatných Azure. tootest, zda byl vytvořen katalog data catalog uživatele ve vaší doméně Azure Active Directory přejděte toohello [Domovská stránka Azure Data Catalog](http://azuredatacatalog.com) a ověřte, zda se zobrazí hello katalogu. Pokud katalog již byl pro vás vytvořen, přeskočte hello následující postup a přejděte toohello další části.    

1. Přejděte toohello [stránku služby Data Catalog](https://azure.microsoft.com/services/data-catalog) a klikněte na tlačítko **Začínáme**.
   
    ![Azure Data Catalog – marketingová cílová stránka](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Přihlaste se pomocí uživatelského účtu, který je vlastníkem hello nebo spoluvlastník předplatného Azure. Zobrazí následující stránky po přihlášení hello.
   
    ![Azure Data Catalog – zřízení katalogu dat](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Zadejte **název** katalogu data catalog hello hello **předplatné** chcete toouse a hello **umístění** hello katalogu.
4. Rozbalte položku **Ceny** a vyberte **edici** služby Azure Data Catalog (Free nebo Standard).
    ![Azure Data Catalog – výběr edice](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Rozbalte položku **uživatelé katalogu** a klikněte na tlačítko **přidat** tooadd uživatelů pro katalog data catalog hello. Se automaticky přidají toothis skupiny.
    ![Azure Data Catalog – uživatelé](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Rozbalte položku **katalogu správci** a klikněte na tlačítko **přidat** tooadd dalších správců pro katalog data catalog hello. Se automaticky přidají toothis skupiny.
    ![Azure Data Catalog – správci](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Klikněte na tlačítko **vytvořit katalog** toocreate hello data catalog pro vaši organizaci. Po vytvoření zobrazí hello domovskou stránku hello datového katalogu.
    ![Azure Data Catalog – vytvořeno](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-hello-azure-portal"></a>Najít katalog data catalog v hello portálu Azure
1. Na samostatné kartě ve webovém prohlížeči hello nebo v okně samostatný webový prohlížeč, přejít toohello [portál Azure](https://portal.azure.com) a přihlašování pomocí tohoto můžete použít toocreate hello katalogu data catalog v předchozím kroku hello hello stejný účet.
2. Vyberte možnost **Procházet** a klikněte na**Data Catalog**.
   
    ![Azure Data Catalog – procházet Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) se hello data katalogu, kterou jste vytvořili.
   
    ![Azure Data Catalog – zobrazení katalogu v seznamu](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
3. Klikněte na tlačítko hello katalogu, kterou jste vytvořili. Zobrazí hello **katalogu Data Catalog** okna portálu hello.
   
   ![Azure Data Catalog – okno na portálu ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
4. Můžete zobrazit vlastnosti hello katalogu data CATALOG a provede jejich aktualizaci. Klikněte například na **cenová úroveň** a změnit edici hello.
   
    ![Azure Data Catalog – cenová úroveň](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Ukázková databáze Adventure Works
V tomto kurzu zaregistrujete datové prostředky (tabulky) z hello AdventureWorks2014 ukázkové databáze pro hello databázového stroje SQL Server, ale můžete použít libovolný podporovaný zdroj dat, pokud si přejete toowork s daty, která je známá a relevantní tooyour role. Seznam podporovaných zdrojů dat naleznete v tématu [Podporované zdroje dat](data-catalog-dsr.md).

### <a name="install-hello-adventure-works-2014-oltp-database"></a>Instalace databáze Adventure Works 2014 OLTP hello
Hello databáze společnosti Adventure Works podporuje scénáře standardního online zpracování transakcí smyšleného výrobce jízdních kol (Adventure Works Cycles), což zahrnuje produkty, prodeje a nákupu. V tomto kurzu zaregistrujete informace o produktech do služby Azure Data Catalog.

ukázkové databáze společnosti Adventure Works hello tooinstall:

1. Stáhněte soubor [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) na webu CodePlex.
2. toorestore hello databáze na počítači, postupujte podle pokynů hello v [obnovení zálohy databáze pomocí SQL Server Management Studio](http://msdn.microsoft.com/library/ms177429.aspx), nebo pomocí následujících kroků:
   1. Otevřete aplikaci SQL Server Management Studio a připojte toohello databázového stroje SQL Server.
   2. Klikněte pravým tlačítkem myši na **Databáze** a vyberte možnost **Obnovit databázi**.
   3. V části **obnovit databázi**, klikněte na tlačítko hello **zařízení** možnost **zdroj** a klikněte na tlačítko **Procházet**.
   4. V části **Výběr zálohovacích zařízení** klikněte na **Přidat**.
   5. Přejděte toohello složku, kde máte hello **AdventureWorks2014.bak** soubor, vyberte hello souboru a klikněte na tlačítko **OK** tooclose hello **vyhledejte záložní soubor** dialogové okno.
   6. Klikněte na tlačítko **OK** tooclose hello **vyberte zálohovací zařízení** dialogové okno.    
   7. Klikněte na tlačítko **OK** tooclose hello **obnovit databázi** dialogové okno.

Nyní můžete zaregistrovat datových prostředků z ukázkové databáze společnosti Adventure Works hello pomocí Azure Data Catalog.

## <a name="register-data-assets"></a>Registrace datových assetů
V tomto cvičení použijete hello registrace nástroj tooregister datové prostředky z databáze společnosti Adventure Works hello hello katalogu. Registrace je hello proces extrahování klíčových strukturálních metadat, například názvy, typy a umístění ze zdroje dat hello a hello prostředků, které obsahuje a zkopírování tohoto katalogu toohello metadat. Hello zdroj dat a datové prostředky zůstanou kde jsou, ale hello metadata jsou používána toomake katalogu hello je snadněji zjistitelné a srozumitelnější.

### <a name="register-a-data-source"></a>Registrace zdroje dat
1. Přejděte toohello [Domovská stránka Azure Data Catalog](http://azuredatacatalog.com) a klikněte na tlačítko **publikovat Data**.
   
   ![Azure Data Catalog – tlačítko Publikovat data](media/data-catalog-get-started/data-catalog-publish-data.png)
2. Klikněte na tlačítko **spustit aplikaci** toodownload, instalaci a spuštění hello nástroj pro registraci ve vašem počítači.
   
   ![Azure Data Catalog – tlačítko Spustit](media/data-catalog-get-started/data-catalog-launch-application.png)
3. Na hello **úvodní** klikněte na tlačítko **přihlášení** a zadejte svá pověření.     
   
    ![Azure Data Catalog – Úvodní stránka](media/data-catalog-get-started/data-catalog-welcome-dialog.png)
4. Na hello **Microsoft Azure Data Catalog** klikněte na tlačítko **systému SQL Server** a **Další**.
   
    ![Azure Data Catalog – zdroje dat](media/data-catalog-get-started/data-catalog-data-sources.png)
5. Zadejte vlastnosti připojení serveru SQL Server hello pro **AdventureWorks2014** (viz následující ukázka hello) a klikněte na tlačítko **CONNECT**.
   
   ![Azure Data Catalog – nastavení připojení k SQL Serveru](media/data-catalog-get-started/data-catalog-sql-server-connection.png)
6. Zaregistrujte hello metadata datový prostředek. V tomto příkladu zaregistrujete **výroba/produkt** objekty z oboru názvů výroba společnosti AdventureWorks hello:
   
   1. V hello **hierarchii serverů** stromu, rozbalte položku **AdventureWorks2014** a klikněte na tlačítko **produkční**.
   2. Stisknutím klávesy Ctrl a kliknutím vyberte postupně **Product**, **ProductCategory**, **ProductDescription** a **ProductPhoto**.
   3. Klikněte na tlačítko hello **přesunout vybranou šipku** (**>**). Tato akce přesune všechny vybrané objekty do hello **toobe objekty zaregistrován** seznamu.
      
      ![Kurz Azure Data Catalog – procházení a výběr objektů](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
   4. Vyberte **zahrnout Náhled** tooinclude náhled snímku hello data. snímek Hello zahrnuje až too20 záznamy ze všech tabulek a se zkopíruje do katalogu hello.
   5. Vyberte **zahrnují Data profilu** tooinclude snímek statistiku objektu hello hello data profilu (například: minimální, maximální a průměrné hodnoty pro sloupec, počet řádků).
   6. V hello **přidat značky** zadejte **adventure funguje, cyklů**. Tím se přidají vyhledávací značky pro tyto datové assety. Značky jsou skvělý způsob, který toohelp uživatelům najít registrovaný zdroj dat..
   7. Zadejte název hello **odborné** tato data (volitelné).
      
      ![Kurz pro Azure Data Catalog – objekty toobe zaregistrován](media/data-catalog-get-started/data-catalog-objects-register.png)
   8. Klikněte na tlačítko **ZAREGISTROVAT**. Služba Azure Data Catalog zaregistruje vaše vybrané objekty. V tomto cvičení jsou registrované hello vybrané objekty z podniku Adventure Works. Nástroj pro registraci Hello extrahuje metadata z hello datový prostředek a zkopíruje data do služby Azure Data Catalog hello. Hello data zůstanou kde se nyní nachází, a zůstane pod dohledem hello hello správců a zásady hello aktuálním systému.
      
      ![Azure Data Catalog – registrované objekty](media/data-catalog-get-started/data-catalog-registered-objects.png)
   9. Klikněte na tlačítko toosee vaší registrované objekty zdrojů dat, **zobrazit portál**. Na portálu Azure Data Catalog hello zkontrolujte, jestli všechny čtyři tabulky a hello databáze v zobrazení mřížky hello.
      
      ![Objekty v portálu Azure Data Catalog hello ](media/data-catalog-get-started/data-catalog-view-portal.png)

V tomto cvičení jste zaregistrovali objekty z ukázkové databáze společnosti Adventure Works hello tak, aby bylo možné snadno rozpoznat uživatelé ve vaší organizaci. V dalším cvičení hello zjistíte, jak toodiscover registrovaných datových prostředků.

## <a name="discover-data-assets"></a>Zjišťování datových assetů
Funkce zjišťování ve službě Azure Data Catalog používá dva primární mechanismy: vyhledávání a filtrování.

Hledání je navrženou toobe intuitivní a efektivní. Ve výchozím nastavení jsou podmínky vyhledávání porovnání všechny vlastnosti v katalogu hello, včetně poznámek zadaný uživatelem.

Filtrování je určená toocomplement vyhledávání. Můžete vybrat konkrétní charakteristiky jako odborníky, typ zdroje dat, typ objektu a značky tooview odpovídající datových prostředků a hledání tooconstrain výsledků toomatching prostředky.

Pomocí kombinace vyhledávání a filtrování, můžete rychle procházet hello zdroje dat, které byly zapsány s Azure Data Catalog toodiscover hello datové prostředky, které potřebujete.

V tomto cvičení použijete hello Azure Data Catalog portálu toodiscover datové prostředky, které jste zaregistrovali v předchozím cvičení hello. Podrobnosti o syntaxi vyhledávání naleznete v článku [Referenční příručka syntaxe vyhledávání ve službě Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx).

Následuje několik příkladů pro zjišťování datových prostředků v katalogu hello.  

### <a name="discover-data-assets-with-basic-search"></a>Zjištění datových assetů pomocí základního vyhledávání
Základní vyhledávání vám pomůže prohledat katalog pomocí jednoho nebo více hledaných výrazů. Výsledky jsou všechny prostředky, které odpovídají na libovolné vlastnosti s jednou nebo více podmínek hello zadaný.

1. Klikněte na tlačítko **Domů** na portálu Azure Data Catalog hello. Pokud jste zavřeli hello webový prohlížeč, přejít toohello [Domovská stránka Azure Data Catalog](https://www.azuredatacatalog.com).
2. Hello vyhledávacího pole zadejte `cycles` a stiskněte klávesu **ENTER**.
   
    ![Azure Data Catalog – základní textové vyhledávání](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Zkontrolujte, jestli všechny čtyři tabulky a hello databáze (AdventureWorks2014) ve výsledcích hello. Můžete přepínat mezi **zobrazení mřížky** a **zobrazení seznamu** pomocí tlačítek na panelu nástrojů hello, jak ukazuje následující obrázek hello. Všimněte si, že hello hledaná klíčová slova je označený ve výsledcích hledání hello, protože hello **zvýrazněte** možnost je **ON**. Můžete také zadat hello počet **výsledky na stránce** ve výsledcích hledání.
   
    ![Azure Data Catalog – výsledky základního textového vyhledávání](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)
   
    Hello **hledání** panel je na hello left a hello **vlastnosti** panel je na správné hello. Na hello **hledání** panelu, můžete změnit kritéria vyhledávání a filtrování výsledků. Hello **vlastnosti** panelu zobrazí vlastnosti vybrané objektu v seznamu nebo hello mřížky.
4. Klikněte na tlačítko **produktu** ve výsledcích hledání hello. Klikněte na tlačítko hello **Preview**, **sloupce**, **Data profilu**, a **dokumentace** karty, nebo klikněte na tlačítko hello šipku tooexpand hello dolním podokně.  
   
    ![Azure Data Catalog – spodní podokno](media/data-catalog-get-started/data-catalog-data-asset-preview.png)
   
    Na hello **Preview** kartě Zobrazit náhled hello dat v hello **produktu** tabulky.  
5. Klikněte na tlačítko hello **sloupce** kartě toofind podrobnosti o sloupce (například **název** a **datový typ**) v hello datový prostředek.
6. Klikněte na tlačítko hello **Data profilu** kartě toosee hello profilace dat (například: počet řádků, velikost dat nebo minimální hodnotu ve sloupci) v hello datový prostředek.
7. Filtrování výsledků hello pomocí **filtry** na levé straně hello. Klikněte například na **tabulky** pro **typ objektu**, a zobrazit pouze tabulky hello čtyři, není hello databáze.
   
    ![Azure Data Catalog – filtrování výsledků vyhledávání](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Zjištění datových assetů pomocí zkoumání vlastností
Vlastnost rozsahu pomáhá zjišťovat prostředky dat, kde hello hledaný termín shoduje s hello zadaná vlastnost.

1. Vymazat hello **tabulky** filtrovat podle **typ objektu** v **filtry**.  
2. Hello vyhledávacího pole zadejte `tags:cycles` a stiskněte klávesu **ENTER**. V tématu [reference syntaxe vyhledávání v katalogu Data](https://msdn.microsoft.com/library/azure/mt267594.aspx) pro všechny vlastnosti, které můžete použít pro vyhledávání dat katalogu hello hello.
3. Zkontrolujte, jestli všechny čtyři tabulky a hello databáze (AdventureWorks2014) ve výsledcích hello.  
   
    ![Data Catalog – výsledky vyhledávání funkce zkoumání vlastnost](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-hello-search"></a>Uložte hledání hello
1. V hello **hledání** podokně hello **aktuální vyhledávání** , zadejte název pro vyhledávání hello a klikněte na **Uložit**.
   
    ![Azure Data Catalog – uložení vyhledávání](media/data-catalog-get-started/data-catalog-save-search.png)
2. Potvrďte, že hello uložené hledání se zobrazí v části **uložená hledání**.
   
    ![Azure Data Catalog – uložená vyhledávání](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Vyberte jednu z akcí hello na hello uložené hledání může trvat (**přejmenovat**, **odstranit**, **uložit jako výchozí** vyhledávání).
   
    ![Azure Data Catalog – možnosti uložených vyhledávání](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Logické operátory
Své vyhledávání můžete rozšířit nebo zúžit pomocí logických operátorů.

1. Hello vyhledávacího pole zadejte `tags:cycles AND objectType:table`a stiskněte klávesu **ENTER**.
2. Zkontrolujte, jestli pouze tabulky (ne databáze hello) ve výsledcích hello.  
   
    ![Azure Data Catalog – logické operátory ve vyhledávání](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Seskupování pomocí závorek
Pomocí seskupení v závorkách, můžete seskupit částí hello dotazu tooachieve logické izolace, zejména spolu s logickými operátory.

1. Hello vyhledávacího pole zadejte `name:product AND (tags:cycles AND objectType:table)` a stiskněte klávesu **ENTER**.
2. Zkontrolujte, jestli pouze hello **produktu** tabulky ve výsledcích hledání hello.
   
    ![Azure Data Catalog – vyhledávání seskupení](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Operátory porovnání
S pomocí operátorů porovnání lze použít porovnávání jiné než rovnost pro vlastnosti, které mají typ dat číslo nebo datum.

1. Hello vyhledávacího pole zadejte `lastRegisteredTime:>"06/09/2016"`.
2. Vymazat hello **tabulky** filtrovat podle **typ objektu**.
3. Stiskněte **ENTER**.
4. Zkontrolujte, jestli hello **produktu**, **ProductCategory**, **Popisvýrobku**, a **Fotografievýrobku** tabulky a hello AdventureWorks2014 databáze, kterou jste zaregistrovali ve výsledcích hledání.
   
    ![Azure Data Catalog – výsledky vyhledávání porovnání](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

V tématu [jak toodiscover datových prostředků](data-catalog-how-to-discover.md) podrobné informace o zjišťování datových prostředků a [reference syntaxe vyhledávání v katalogu Data](https://msdn.microsoft.com/library/azure/mt267594.aspx) pro syntaxe vyhledávání.

## <a name="annotate-data-assets"></a>Přidání poznámek k datovým assetům
V tomto cvičení použijete portál tooannotate Azure Data Catalog hello (přidat informace, jako jsou popisy, značky nebo odborníky) datové prostředky, které jste již dříve zaregistrovali v katalogu hello. Hello poznámky doplnit a vylepšení hello strukturální metadata extrahovaná ze zdroje dat hello během registrace a umožňuje mnohem snazší toodiscover hello datových prostředků a pochopit.

V tomto cvičení přidáte poznámky k jednomu datovému assetu (ProductPhoto). Přidat popisný název a popis toohello Fotografievýrobku datový prostředek.  

1. Přejděte toohello [Domovská stránka Azure Data Catalog](https://www.azuredatacatalog.com) a vyhledejte s `tags:cycles` toofind hello datových prostředků je zaregistrovaný.  
2. Ve výsledcích vyhledávání klikněte na **ProductPhoto**.  
3. Zadejte **obrázky produktů** pro **popisný název** a **fotografie výrobku pro marketingové materiály** pro hello **popis**.
   
    ![Azure Data Catalog – popis ProductPhoto](media/data-catalog-get-started/data-catalog-productphoto-description.png)
   
    Hello **popis** pomůže ostatním zjišťovat a pochopit, proč a jak toouse hello vybraný datový prostředek. Můžete také přidat další značky a zobrazit sloupce. Nyní můžete zkusit vyhledávání a filtrování toodiscover datové prostředky pomocí popisných metadat hello přidali jste toohello katalogu.

Můžete také provést hello následující na této stránce:

* Přidejte odborníky pro hello datový prostředek. Klikněte na tlačítko **přidat** v hello **odborníky** oblasti.
* Přidání značek na úrovni hello datovou sadu. Klikněte na tlačítko **přidat** v hello **značky** oblasti. Značka může být značka uživatele nebo značka glosáře. Hello standardní edice katalogu Data Catalog zahrnuje obchodní glosář, který pomáhá správcům katalogu definovat centrální obchodní taxonomii. Uživatelé katalogu mohou poté opatřit poznámkami datové assety pomocí termínů v glosáři. Další informace najdete v tématu [jak tooset až hello obchodní Glosář řízeným přidáváním značek](data-catalog-how-to-business-glossary.md)
* Přidání značek na úrovni sloupce hello. Klikněte na tlačítko **přidat** pod **značky** pro sloupec hello chcete tooannotate.
* Na úrovni sloupce hello přidáte popis. Zadejte **Popis** pro sloupec. Můžete také zobrazit hello popis metadat extrahovaných ze zdroje dat hello.
* Přidat **požádat o přístup** informace, které zobrazuje uživatele, jak toorequest přístup toohello datový prostředek.
  
    ![Azure Data Catalog – přidání značek, popisů](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)
* Zvolte hello **dokumentace** a zadejte dokumentaci hello datový prostředek. V dokumentaci k Azure Data Catalog můžete použít svůj katalog dat. jako obsahu úložiště toocreate dokončení popisů datových prostředků.
  
    ![Azure Data Catalog – karta Dokumentace](media/data-catalog-get-started/data-catalog-documentation.png)

Můžete také přidat poznámky toomultiple datových prostředků. Můžete například vybrat všechny hello datové prostředky, které jste zaregistrovali a zadejte odborník pro ně.

![Azure Data Catalog – přidání poznámky k více datovým assetům](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure Data Catalog podporuje velkého množství lidí sourcing tooannotations přístup. Každý uživatel katalogu Data Catalog můžete přidat značky (uživatele či glosář), popisy a další metadata, tak, aby každý uživatel s perspektivou na datový prostředek a jeho použití může mít tento perspektivy zachytit a uživatelům k dispozici tooother.

V tématu [jak tooannotate datových prostředků](data-catalog-how-to-annotate.md) podrobné informace o zadávání poznámek k datových prostředků.

## <a name="connect-toodata-assets"></a>Připojit toodata prostředky
V tomto cvičení otevřete datové assety v integrovaném klientském nástroji (Excel) i neintegrovaném nástroji (SQL Server Management Studio) s použitím informací o připojení.

> [!NOTE]
> Je důležité tooremember, který Azure Data Catalog není získáte přístup ke zdroji skutečná data toohello – jednoduše je jednodušší pro vás toodiscover a porozumět. Při připojení zdroje dat tooa hello klientská aplikace, kterou zvolíte používá pověření systému Windows nebo vás vyzve k zadání přihlašovacích údajů podle potřeby. Pokud jste dříve nebyla udělena zdroj dat toohello přístup, je třeba toobe přístup udělen, než se můžete připojit.
> 
> 

### <a name="connect-tooa-data-asset-from-excel"></a>Připojit tooa datový prostředek z Excelu
1. Ve výsledcích vyhledávání vyberte možnost **Product**. Klikněte na tlačítko **otevřít v** na panelu nástrojů hello a klikněte na **Excel**.
   
    ![Azure Data Catalog--toodata asset připojení](media/data-catalog-get-started/data-catalog-connect1.png)
2. Klikněte na tlačítko **otevřete** v místním okně stahování hello. Toto prostředí se může lišit v závislosti na prohlížeči hello.
   
    ![Azure Data Catalog – stažený soubor připojení pro Excel](media/data-catalog-get-started/data-catalog-download-open.png)
3. V hello **oznámení o zabezpečení aplikace Microsoft Excel** okně klikněte na tlačítko **povolit**.
   
    ![Azure Data Catalog – automaticky otevřené okno zabezpečení Excelu](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Ponechat výchozí nastavení hello v hello **importovat Data** dialogové okno a klikněte na tlačítko **OK**.
   
    ![Azure Data Catalog – import dat aplikace Excel](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Zobrazení hello zdroj dat v aplikaci Excel.
   
    ![Azure Data Catalog – tabulka produktů v Excelu](media/data-catalog-get-started/data-catalog-connect2.png)

V tomto cvičení jste se přihlásili toodata prostředky zjistí pomocí Azure Data Catalog. Pomocí portálu Azure Data Catalog hello, můžete připojit přímo pomocí hello klientských aplikací integrovaných do hello **otevřít v** nabídky. Můžete také připojit s libovolnou aplikací, které zvolíte pomocí připojení informace o umístění hello obsažené v metadatech prostředku hello. Například můžete SQL Server Management Studio tooconnect toohello AdventureWorks2014 databáze tooaccess hello dat v datových prostředků hello registrované v tomto kurzu.

1. Otevřete **SQL Server Management Studio**.
2. V hello **připojit tooServer** dialogovém okně zadejte název serveru hello z hello **vlastnosti** poddokně na portálu Azure Data Catalog hello.
3. Použijte příslušné ověřování a přihlašovací údaje tooaccess hello datový prostředek. Pokud nemáte přístup, použijte informace v hello **požádat o přístup** pole tooget ho.
   
    ![Azure Data Catalog – žádost o přístup](media/data-catalog-get-started/data-catalog-request-access.png)

Klikněte na tlačítko **zobrazuje připojovací řetězce** tooview a zkopírujte ADF.NET a rozhraní ODBC, OLEDB připojovací řetězce toohello schránky pro použití ve vaší aplikaci.

## <a name="manage-data-assets"></a>Správa datových assetů
V tomto kroku uvidíte jak tooset až zabezpečení pro vaše datové prostředky. Data Catalog není uživatelům přístup k datům toohello sám sebe. Vlastník Hello zdroje dat hello řídí přístup k datům.

Můžete používat katalogu Data Catalog tooview hello metadat a zdrojů dat toodiscover související zdroje toohello registrované v katalogu hello. Mohou nastat situace, ale kde zdroje dat mají jenom viditelné toospecific uživatele nebo toomembers určitých skupin. Pro tyto scénáře můžete registrované datové prostředky v rámci hello katalogu Data Catalog tootake vlastnictví a viditelnost hello ovládacího prvku toothen hello prostředků, které vlastníte.

> [!NOTE]
> Hello možnosti správy popsané v tomto cvičení jsou k dispozici pouze v hello Azure Data Catalog, Standard Edition, není v hello edice Free.
> V Azure Data Catalog může převzít vlastnictví datových prostředků, přidat spoluvlastníky toodata prostředky a nastavte hello viditelnost datových prostředků.
> 
> 

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Převzetí vlastnictví datových assetů a omezení viditelnosti
1. Přejděte toohello [Domovská stránka Azure Data Catalog](https://www.azuredatacatalog.com). V hello **vyhledávání** textové pole, zadejte `tags:cycles` a stiskněte klávesu **ENTER**.
2. Klikněte na položku v seznamu výsledků hello a klikněte na tlačítko **převzít vlastnictví** na panelu nástrojů hello.
3. V hello **správy** části hello **vlastnosti** panelu, klikněte na tlačítko **převzít vlastnictví**.
   
    ![Azure Data Catalog – převzetí vlastnictví](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. viditelnost toorestrict, zvolte **vlastníci a tito uživatelé** v hello **viditelnost** části a klikněte na tlačítko **přidat**. Zadejte e-mailové adresy uživatele v hello textového pole a stiskněte klávesu **ENTER**.
   
    ![Azure Data Catalog – omezení přístupu](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Odstranění datových assetů
V tomto cvičení použijete hello Azure Data Catalog portálu tooremove náhledu dat z registrovaných datových prostředků a odstranění datových prostředků z katalogu hello.

Ve službě Azure Data Catalog je možné odstranit jednotlivý asset nebo více assetů.

1. Přejděte toohello [Domovská stránka Azure Data Catalog](https://www.azuredatacatalog.com).
2. V hello **vyhledávání** textové pole, zadejte `tags:cycles` a klikněte na tlačítko **ENTER**.
3. Vyberte položku v seznamu výsledků hello a klikněte na tlačítko **odstranit** na hello nástrojů, jak ukazuje následující obrázek hello:
   
    ![Azure Data Catalog – odstranění položky mřížky](media/data-catalog-get-started/data-catalog-delete-grid-item.png)
   
    Pokud používáte zobrazení seznamu hello, hello zaškrtávací políčko je toohello nalevo od hello položky, jak ukazuje následující obrázek hello:
   
    ![Azure Data Catalog – odstranění položky seznamu](media/data-catalog-get-started/data-catalog-delete-list-item.png)
   
    Můžete také vybrat více datových prostředků a je odstranit, jak ukazuje následující obrázek hello:
   
    ![Azure Data Catalog – odstranění více datových assetů](media/data-catalog-get-started/data-catalog-delete-assets.png)

> [!NOTE]
> Hello výchozí chování katalogu hello je tooallow všechny uživatele tooregister všechny zdroje dat a tooallow všechny uživatele toodelete datový prostředek, který byl zaregistrován. možnosti správy Hello součástí hello Azure Data Catalog, Standard Edition poskytují další možnosti pro převzetí vlastnictví prostředků, omezení, kdo může zjišťovat prostředky a omezení, kdo může odstranit prostředky.
> 
> 

## <a name="summary"></a>Souhrn
V tomto kurzu jste prozkoumali základní možnosti služby Azure Data Catalog včetně registrace, přidávání poznámek, zjišťování a správy datových assetů organizace. Teď, když jste dokončili kurz hello, je čas spuštění tooget. Můžete začít ještě dnes registrací hello zdroje dat, které vám a vašemu týmu spoléhají na a pozvat kolegy toouse hello katalogu.

## <a name="references"></a>Odkazy
* [Jak tooregister datových prostředků](data-catalog-how-to-register.md)
* [Jak toodiscover datových prostředků](data-catalog-how-to-discover.md)
* [Jak tooannotate datových prostředků](data-catalog-how-to-annotate.md)
* [Jak toodocument datových prostředků](data-catalog-how-to-documentation.md)
* [Jak tooconnect toodata prostředky](data-catalog-how-to-connect.md)
* [Jak toomanage datových prostředků](data-catalog-how-to-manage.md)

