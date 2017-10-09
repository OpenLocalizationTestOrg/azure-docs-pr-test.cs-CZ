---
title: "Kurz: Konfigurace Workday pro automatické zřizování uživatelů místní služby Active Directory a Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak toouse Workday jako zdroj dat identity pro Active Directory a Azure Active Directory."
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/26/2017
ms.author: asmalser
ms.openlocfilehash: d4eb3237b8fe7614606c58b39fbefcb44f4060fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-with-on-premises-active-directory-and-azure-active-directory"></a>Kurz: Konfigurace Workday pro automatické zřizování uživatelů místní služby Active Directory a Azure Active Directory
cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform tooimport osoby z Workday do služby Active Directory a Azure Active Directory, s volitelné zpětný zápis tooWorkday některé atributy. 



## <a name="overview"></a>Přehled

Hello [zřizování služby Azure Active Directory uživatelů](active-directory-saas-app-provisioning.md) se integruje s hello [Workday lidských zdrojů API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) v pořadí tooprovision uživatelské účty. Azure AD používá tato připojení tooenable hello následující uživatele pracovní postupy zřizování:

* **Zřizování uživatelů tooActive Directory** -synchronizovat vybrané skupiny uživatelů z Workday do jednoho nebo více doménových struktur služby Active Directory. 

* **Zřizování tooAzure jenom pro cloud uživatelů služby Active Directory** -hybridní uživatelů, kteří se nacházejí ve službě Active Directory a Azure Active Directory může být zřízen do druhé pomocí hello [AAD Connect](connect/active-directory-aadconnect.md). Uživatelé, kteří jsou jenom pro cloud však může být zřízen přímo z Workday pomocí služby Active Directory tooAzure hello uživatele Azure AD zřizování služby.

* **Zpětný zápis e-mailové adresy tooWorkday** -zřizování služby uživatelů hello Azure AD může zapisovat vybrali Azure AD uživatelské atributy zpět tooWorkday, jako je například hello e-mailovou adresu.

### <a name="scenarios-covered"></a>Pokryté scénáře

zřizování pracovních nepodporuje uživatele Azure AD hello zřizování služby Hello Workday uživatelů umožňuje automatizaci hello následující scénáře správy životního cyklu lidské zdroje a identity:

* **Zařazení nového zaměstnanců** – při přidání nového zaměstnance tooWorkday, uživatelský účet se vytvoří automaticky v Active Directory, Azure Active Directory a volitelně Office 365 a [jiné aplikace SaaS podporovaný službou Azure AD ](active-directory-saas-app-provisioning.md), při zápisu zadní hello e-mailovou adresu tooWorkday.

* **Zaměstnanec atribut a profil aktualizace** – když záznam se aktualizuje v Workday (například jeho název, název nebo správce), jejich uživatelské účty se automaticky aktualizuje v Active Directory, Azure Active Directory a volitelně Office 365 a [jiné aplikace SaaS podporovaný službou Azure AD](active-directory-saas-app-provisioning.md).

* **Zaměstnanec ukončení** – Pokud zaměstnanec je ukončen v Workday, jejich uživatelský účet je automaticky zakázán v Active Directory, Azure Active Directory a volitelně Office 365 a [jiné aplikace SaaS podporovaný službou Azure AD](active-directory-saas-app-provisioning.md).

* **Zaměstnanec znovu najímá** – Pokud zaměstnanec je rehired ve Workday, jejich starý účet lze automaticky znovu aktivovat nebo znovu zřízené (v závislosti na vaši volbu) tooActive adresáře, Azure Active Directory a volitelně Office 365 a [jiné aplikace SaaS podporovaný službou Azure AD](active-directory-saas-app-provisioning.md).


## <a name="planning-your-solution"></a>Plánování řešení

Před zahájením svoji integraci s Workday Zkontrolujte předpoklady hello níže a čtení hello následující pokyny toomatch aktuální architektura služby Active Directory a požadavky s zřizování uživatelů hello solution(s) poskytuje Azure Active Adresář.

### <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure AD Premium P1 s přístupem globální správce
* Implementace klienta Workday pro účely testování a integrace
* Oprávnění správce v Workday toocreate uživatele systému integrace a zkontrolujte změny tootest zaměstnanec data pro účely testování
* Pro zřizování tooActive Directory uživatelů, je připojený k doméně serveru spuštěna služba Windows 2012 nebo vyšší vyžaduje toohost hello [místní agent synchronizace](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](connect/active-directory-aadconnect.md) pro synchronizaci mezi službou Active Directory a Azure AD

> [!NOTE]
> Pokud se klientovi služby Azure AD se nachází v Evropě, najdete v tématu hello [známé problémy](#known-issues) části níže.


### <a name="solution-architecture"></a>Architektura řešení

Azure AD poskytuje bohatou sadu zřizování toohelp konektory, které řešit zřizování identity správu životního cyklu a z Workday tooActive adresáře, Azure AD, aplikace SaaS a nad rámec. Které funkce bude používat a jak nastavit hello řešení se budou lišit v závislosti na prostředí a požadavky vaší organizace. Jako první krok udělejte si inventuru kolik hello následující jsou přítomna a nasazené ve vaší organizaci:

* Kolik doménových struktur Active Directory, jsou používány?
* Kolik domény služby Active Directory, jsou používány?
* Kolik Active Directory organizačních jednotek (OU), jsou používány?
* Kolik klientů služby Azure Active Directory, jsou používány?
* Existují uživatelé, kteří potřebují tooboth toobe zřízení služby Active Directory a Azure Active Directory (např. "hybridní" uživatelů)?
* Existují uživatelé, kteří potřebují tooAzure toobe zřízení služby Active Directory, ale není Active Directory (např. "jen cloudu" uživatelů)?
* Mají e-mailové adresy uživatele toobe zapsány zpět tooWorkday?

Až budete mít odpovědi toothese dotazy, můžete naplánovat vaší Workday zřizování nasazení podle hello pokynů níže.

#### <a name="using-provisioning-connector-apps"></a>Pomocí aplikací, zřizování konektoru

Azure Active Directory podporuje předem integrovaných zřizování konektory pro Workday a mnoho dalších aplikací SaaS. 

Jeden konektor zřizování rozhraní s hello rozhraní API systému jednoho zdroje a pomáhá zřídit data tooa jednoho cílového systému. Většina zřizování konektory, které podporuje Azure AD jsou pro jeden zdrojový a cílový systém (např. tooServiceNow Azure AD) a může být instalace stačí přidat aplikace hello dotyčném z Galerie aplikací Azure AD hello (např. ServiceNow). 

Ve službě Azure AD je relace 1: 1 mezi zřizování konektor instancí a instance aplikace:

| Zdrojového systému | Cílový systém |
| ---------- | ---------- | 
| Klientovi služby Azure AD | Aplikace SaaS |


Při práci s Workday a služby Active Directory, existuje však několik zdrojové a cílové toobe systémy, které jsou považovány za:

| Zdrojového systému | Cílový systém | Poznámky |
| ---------- | ---------- | ---------- |
| WORKDAY | Doménové struktury služby Active Directory | Každé doménové struktuře je považován za odlišné cílový systém |
| WORKDAY | Klientovi služby Azure AD | Podle potřeby pro uživatele jenom pro cloud |
| Doménové struktury služby Active Directory | Klientovi služby Azure AD | Tento tok se zpracovává souborem AAD Connect dnes |
| Klientovi služby Azure AD | WORKDAY | Pro zpětný zápis e-mailové adresy |

toofacilitate tyto více pracovních postupů toomultiple zdrojové a cílové systémy, Azure AD poskytuje více zřizování konektor aplikace, které můžete přidat z Galerie aplikace hello Azure AD:

![Galerie aplikací v AAD](./media/active-directory-saas-workday-inbound-tutorial/WD_Gallery.PNG)

* **WORKDAY tooActive Directory zřizování** -tuto aplikaci usnadňuje zřizování účtu uživatelů z Workday tooa jedné doménové struktury Active Directory. Pokud máte více doménových struktur, můžete přidat jednu instanci této aplikaci z Galerie aplikace hello Azure AD pro každou doménovou strukturu služby Active Directory, které potřebujete k tooprovision.

* **WORKDAY tooAzure AD zřizování** -při AAD Connect je hello nástroj, který by měl být tooAzure uživatelů služby Active Directory používané toosynchronize služby Active Directory, tato aplikace se nedá použít toofacilitate zřizování jenom pro cloud uživatelů z Workday tooa jeden Klienta Azure Active Directory.

* **Zpětný zápis WORKDAY** -tuto aplikaci usnadňuje zpětný zápis e-mailové adresy uživatele z tooWorkday Azure Active Directory.

> [!TIP]
> Hello regulární "Workday" aplikace se používá pro nastavení jednotného přihlašování mezi Workday a Azure Active Directory. 

Jak tooset až a nakonfigurovat speciální zřizování aplikací konektoru je hello předmět hello zbývající části tohoto kurzu. Které aplikace zvolíte tooconfigure bude záviset na systémy, které musíte tooprovision a kolik doménové struktury služby Active Directory a Azure AD klienti používají ve vašem prostředí.

![Přehled](./media/active-directory-saas-workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>Konfigurace uživatele systému integrace ve Workday
Běžné požadavek všechny konektory zřizování Workday hello je, že se že vyžadují přihlašovací údaje pro Workday systému integrace účet tooconnect toohello API Workday lidských zdrojů. Tato část popisuje, jak toocreate systémový integrátor účet v Workday.

> [!NOTE]
> Je možné toobypass tento postup a místo toho použít účet globálního správce Workday jako účet integrace systému hello. To může bez problémů fungují pro ukázky, ale nedoporučuje se používat pro nasazení v produkčním prostředí.

### <a name="create-an-integration-system-user"></a>Vytvořit uživatele s integrace systému

**toocreate o uživatele systému integrace:**

1. Přihlaste se pomocí účtu správce klienta Workday. V hello **Workday Workbench**, zadejte vytvoření uživatele hello vyhledávacího pole a pak klikněte na **vytvořit uživatele systému integrace**. 
   
    ![Vytvořit uživatele](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "vytvořit uživatele")
2. Dokončení hello **vytvořit uživatele systému integrace** úlohu zadáním uživatelské jméno a heslo pro nového uživatele systému integrace.  
 * Nechte hello **vyžadují nové heslo při dalším přihlášení** možnost není zaškrtnuto, protože tento uživatel bude protokolování prostřednictvím kódu programu. 
 * Nechte hello **minut časový limit relace** s jeho výchozí hodnotu 0, které zabrání hello uživatelských relací předčasně časového limitu. 
   
    ![Vytvořte uživatele systému integrace](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "vytvořte uživatele systému integrace")

### <a name="create-a-security-group"></a>Vytvořte skupinu zabezpečení
Potřebujete toocreate skupinu zabezpečení systému integrace bez omezení a přiřadit uživatele tooit hello.

**toocreate skupiny zabezpečení:**

1. Zadejte skupiny zabezpečení hello vyhledávacího pole a potom klikněte na **vytvořit skupinu zabezpečení**. 
   
    ![Skupiny CreateSecurity](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity skupiny")
2. Dokončení hello **vytvořit skupinu zabezpečení** úloh.  
3. Vyberte skupiny zabezpečení systému integrace – neomezeného z hello **typ z nevyužívá dělené tabulky skupiny zabezpečení** rozevíracího seznamu.
4. Vytvořte toowhich skupiny zabezpečení, které členy přidané. 
   
    ![Skupiny CreateSecurity](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity skupiny")

### <a name="assign-hello-integration-system-user-toohello-security-group"></a>Přiřazení skupiny zabezpečení hello integrace systému uživatele toohello

**uživatele systému integrace tooassign hello:**

1. Zadejte skupiny zabezpečení upravit hello vyhledávacího pole a pak klikněte na **upravit skupinu zabezpečení**. 
   
    ![Upravit skupinu zabezpečení](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "upravit skupinu zabezpečení")
2. Vyhledejte a vyberte novou skupinu zabezpečení integrace hello podle názvu. 
   
    ![Upravit skupinu zabezpečení](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "upravit skupinu zabezpečení")
3. Přidejte hello nové integrace systému uživatele toohello novou skupinu zabezpečení. 
   
    ![Skupina zabezpečení systému](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "skupina zabezpečení systému")  

### <a name="configure-security-group-options"></a>Konfigurovat možnosti pro skupiny zabezpečení
V tomto kroku udělíte nového zabezpečení toohello oprávnění skupiny pro **získat** a **Put** operací na objekty hello zabezpečené hello následující zásady zabezpečení domény:

* Zřizování externího účtu
* Worker dat: Sestavy veřejné pracovního procesu
* Dat pracovníků: Všechny pozice
* Pracovní Data: Personální informace aktuální
* Data pracovního procesu: Název firmy na profilu pracovní

**možnosti pro skupiny zabezpečení tooconfigure:**

1. Zadejte zásady zabezpečení domény hello vyhledávacího pole a poté klikněte na odkaz hello **zásady zabezpečení domény pro funkční oblast**.  
   
    ![Zásady zabezpečení domény](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "zásady zabezpečení domény")  
2. Vyhledávání systému a vyberte hello **systému** funkční oblast.  Klikněte na **OK**.  
   
    ![Zásady zabezpečení domény](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "zásady zabezpečení domény")  
3. V seznamu hello zásad zabezpečení pro hello funkční oblast systému rozbalte **zabezpečení správy** a vyberte zásady zabezpečení domény hello **zřizování externího účtu**.  
   
    ![Zásady zabezpečení domény](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "zásady zabezpečení domény")  
4. Klikněte na tlačítko **upravit oprávnění**a pak klikněte na hello **upravit oprávnění**dialogovém okně přidejte hello nového zabezpečení skupiny toohello seznamu skupin zabezpečení s **získat** a **Put** integrace oprávnění. 
   
    ![Upravit oprávnění](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "upravit oprávnění")  
5. Zopakujte krok 1 výše tooreturn toohello obrazovku pro výběr funkčním oblastem a tentokrát, vyhledejte pracovníky, vyberte hello **personální funkční oblast** a klikněte na tlačítko **OK**.
   
    ![Zásady zabezpečení domény](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "zásady zabezpečení domény")  
6. V seznamu hello zásad zabezpečení pro hello funkční oblast personální rozbalte **dat pracovníků: personální** a opakováním kroků 4 výše pro každou z nich zbývající zásady zabezpečení:

   * Worker dat: Sestavy veřejné pracovního procesu
   * Dat pracovníků: Všechny pozice
   * Pracovní Data: Personální informace aktuální
   * Data pracovního procesu: Název firmy na profilu pracovní
   
7. Opakujte kroky 1, výše tooreturn toohello obrazovku pro výběr funkčním oblastem a tentokrát, vyhledejte **kontaktní údaje**, vyberte hello personální funkční oblast a klikněte na tlačítko **OK**.

8.  V seznamu hello zásad zabezpečení pro hello funkční oblast personální rozbalte **dat pracovníků: pracovní kontaktní údaje**a opakováním kroků 4 výše pro zásady zabezpečení hello níže:

    * Dat pracovníků: Pracovní e-mailu

    ![Zásady zabezpečení domény](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "zásady zabezpečení domény")  
    
### <a name="activate-security-policy-changes"></a>Aktivovat změny zásad zabezpečení

**změny zásad zabezpečení tooactivate:**

1. Zadejte aktivovat hello vyhledávacího pole a potom klikněte na odkaz hello **aktivovat čekající změny zásad zabezpečení**. 
   
    ![Aktivovat](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "aktivovat") 
2. Začátek hello aktivovat čekající změny zásad zabezpečení úloh tak, že zadáte komentář pro účely auditování a pak klikněte na **OK**. 
   
    ![Aktivovat čekající na vyřízení zabezpečení](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "aktivovat čekající na vyřízení zabezpečení")   
3. Úloha dokončení hello na další obrazovce hello zaškrtnutím políčka hello **potvrdit**a potom klikněte na **OK**. 
   
    ![Aktivovat čekající na vyřízení zabezpečení](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "aktivovat čekající na vyřízení zabezpečení")  

## <a name="configuring-user-provisioning-from-workday-tooactive-directory"></a>Konfiguraci zřizování uživatelů z Workday tooActive adresáře
Postupujte podle těchto pokynů tooconfigure uživatelského účtu z Workday tooeach doménové struktury služby Active Directory, která vyžadují zřizování pro zřizování.

### <a name="part-1-adding-hello-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a>Část 1: Přidání hello zřizování konektor aplikace a vytvoření připojení tooWorkday hello

**tooconfigure Workday tooActive Directory zřizování:**

1.  Přejděte příliš<https://portal.azure.com>

2.  V levém navigačním panelu hello vyberte **Azure Active Directory**

3.  Vyberte **podnikové aplikace, které**, pak **všechny aplikace**.

4.  Vyberte **přidat aplikaci**a vyberte hello **všechny** kategorie.

5.  Vyhledejte **Workday zřizování tooActive Directory**a přidejte tuto aplikaci z Galerie hello.

6.  Po hello aplikace přidána, se zobrazí úvodní obrazovka podrobnosti o aplikaci, vyberte **zřizování**

7.  Změna hello **zřizování** **režimu** příliš**automatické**

8.  Dokončení hello **přihlašovací údaje správce** části následujícím způsobem:

   * **Uživatelské jméno správce** – zadejte uživatelské jméno hello hello Workday integrace systémový účet s připojeným názvem domény klienta hello. **By měl vypadat podobně jako:username@contoso4**

   * **Heslo správce –** heslo zadejte hello hello Workday integrace systémový účet

   * **URL – klienta** zadejte hello toohello Workday webové služby koncový bod adresy URL pro vašeho klienta. To by měl vypadat jako: https://wd3-impl-services1.workday.com/ccx/service/contoso4, kde contoso4 se nahradí název vašeho klienta správné a wd3 impl nahrazena hello prostředí správný řetězec.

   * **Doménové struktury služby Active Directory -** hello "Název" doménové struktury služby Active Directory, jak ho vrátila hello Get-ADForest prostředí PowerShell.. Toto je obvykle řetězec jako: *contoso.com*

   * **Kontejner služby Active Directory -** zadejte řetězec hello kontejneru, který obsahuje všechny uživatele v doménové struktuře AD. Příklad: *OU = standardní uživatelé, OU = Users, DC = contoso, DC = test*

   * **E-mailové oznámení –** zadejte e-mailovou adresu a zaškrtněte políčko "odeslání e-mailu, pokud dojde k selhání".

   * Klikněte na tlačítko hello **Test připojení** tlačítko. Pokud hello test připojení úspěšný, klikněte na tlačítko hello **Uložit** tlačítka v horní části hello. Pokud se nezdaří, zkontrolujte, zda jsou přihlašovací údaje do Workday hello platné ve Workday. 

![portál Azure](./media/active-directory-saas-workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>Část 2: Konfigurace mapování atributů 

V této části nakonfigurujete, jak jsou data uživatele z Workday do služby Active Directory.

1.  Na kartě zřizování hello pod **mapování**, klikněte na tlačítko **synchronizovat pracovníci Workday tooOnPremises**.

2.  V hello **zdrojového objektu oboru** pole, můžete vybrat, které skupiny uživatelů v Workday by měla být v oboru pro zřizování tooAD, definováním sadu filtrů založená na atributu. Výchozí rozsah Hello je "všichni uživatelé v Workday". Příklad filtrů:

   * Příklad: Obor toousers s ID pracovní mezi 1000000 a 2000000

      * Atribut: WorkerID

      * Operátor: Shoda REGEX

      * Hodnota: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Příklad: Pouze zaměstnanci a není contingent pracovníkům 

      * Atribut: EmployeeID

      * Operátor: Není NULL.

3.  V hello **cílový objekt akce** pole, můžete globálně filtrovat, jaké akce jsou povoleny toobe provést na službě Active Directory. **Vytvoření** a **aktualizace** jsou nejběžnější.

4.  V hello **mapování atributů** části, můžete definovat, jak jednotlivé Workday atributy mapování atributů adresáře tooActive.

5. Klikněte na existující mapování tooupdate atribut, nebo klikněte na tlačítko **přidat nové mapování** dolnímu hello hello obrazovky tooadd nové mapování. Mapování u jednotlivých atribut podporuje tyto vlastnosti:

      * **Typ mapování**

         * **Přímé** – zapíše hello hodnotu atributu hello Workday atribut toohello AD, beze změn

         * **Konstantní** -zapsat hodnotu statické, konstantní řetězec do atribut hello AD

         * **Výraz** – vám umožní toowrite vlastní hodnotu atributu hello AD, založené na jeden nebo více atributů Workday. [Další informace najdete v článku na výrazy](active-directory-saas-writing-expressions-for-attribute-mappings.md).

      * **Zdrojový atribut** -hello atribut uživatele z Workday.

      * **Výchozí hodnota** – volitelné. Pokud zdrojový atribut hello má prázdnou hodnotu, hello mapování zapíše tuto hodnotu.
            Nejběžnější konfigurace je tooleave tomto prázdné.

      * **Atribut target** – hello atribut uživatele ve službě Active Directory.

      * **Objekty používající tento atribut odpovídat** – zda by měl použít toto mapování toouniquely identifikaci uživatelů mezi Workday a služby Active Directory. To se obvykle nastavuje na pole ID pracovní pro Workday, který je obvykle namapována na jeden z atributů hello identifikační číslo zaměstnance ve službě Active Directory.

      * **Odpovídající prioritu** – více odpovídající atributy lze nastavit. Pokud existuje více vyhodnocují se v pořadí podle tohoto pole. Jakmile je nalezena shoda, žádné další odpovídající atributy vyhodnocují.

      * **Použít toto mapování**
       
         * **Vždy** – použít toto mapování na obou vytvoření uživatele a aktualizovat akce

         * **Pouze během vytváření** -použít toto mapování pouze na akcí vytvoření uživatele

6. Klikněte na tlačítko toosave mapování, **Uložit** hello horní části mapování atributů.

![portál Azure](./media/active-directory-saas-workday-inbound-tutorial/WD_2.PNG)

**Tady jsou některé příklad mapování atributů mezi Workday a služby Active Directory s některé běžné výrazy**

-   Hello výraz, který mapuje toohello parentDistinguishedName AD atributu může být použité tooprovision tooa uživatele, konkrétní organizační jednotku v závislosti na jeden nebo více atributů zdroj Workday. Tento příklad umístí uživatelům v jiné organizační jednotky, v závislosti na jejich data města Workday.

-   Hello výraz, který mapuje atribut userPrincipalName AD toohello vytvořit název UPN firstName.LastName@contoso.com. Nahrazuje také neplatné speciální znaky.

-   [Je dokumentaci na zapisují se výrazy sem](active-directory-saas-writing-expressions-for-attribute-mappings.md)

  
| WORKDAY ATRIBUT | ATRIBUT SLUŽBY ACTIVE DIRECTORY |  ODPOVÍDAJÍCÍ ID? | VYTVOŘIT / AKTUALIZOVAT |
| ---------- | ---------- | ---------- | ---------- |
|  **WorkerID**  |  Číslo zaměstnance | **Ano** | Zapisovat pouze na vytvoření | 
|  **Okres**   |   l   |     | Vytvoření + aktualizace |
|  **Společnosti**         | Společnosti   |     |  Vytvoření + aktualizace |
|  **CountryReferenceTwoLetter**      |   co |     |   Vytvoření + aktualizace |
| **CountryReferenceTwoLetter**    |  C  |     |         Vytvoření + aktualizace |
| **SupervisoryOrganization**  | Oddělení  |     |  Vytvoření + aktualizace |
|  **PreferredNameData**  |  displayName |     |   Vytvoření + aktualizace |
| **Číslo zaměstnance**    |  CN    |   |   Zapisovat pouze na vytvoření |
| **Fax**      | facsimileTelephoneNumber     |     |    Vytvoření + aktualizace |
| **FirstName**   | givenName       |     |    Vytvoření + aktualizace |
| **Přepínače (\[Active\],, "0", "True", "1")** |  AccountDisabled      |     | Vytvoření + aktualizace |
| **Mobile**  |    mobilní       |     |       Zapisovat pouze na vytvoření |
| **EmailAddress**    | E-mailu    |     |     Vytvoření + aktualizace |
| **ManagerReference**   | Správce  |     |  Vytvoření + aktualizace |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Vytvoření + aktualizace |
| **PSČ**  |   PSČ  |     | Vytvoření + aktualizace |
| **LocalReference** |  preferredLanguage  |     |  Vytvoření + aktualizace |
| ** Nahradit (Mid (Nahraďte (\[EmployeeID\],, "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\ \\ &lt; \\ \\ &gt; \]) "," ",), 1, 20)," ([\\\\.) \* \$] (file:///\\.) *$)", , "", , )**      |    Název účtu SAM            |     |         Zapisovat pouze na vytvoření |
| **Příjmení**   |   sn   |     |  Vytvoření + aktualizace |
| **CountryRegionReference** |  St     |     | Vytvoření + aktualizace |
| **AddressLineData**    |  StreetAddress  |     |   Vytvoření + aktualizace |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Zapisovat pouze na vytvoření |
| **BusinessTitle**   |  Název     |     |  Vytvoření + aktualizace |
| **Join("@",Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Join(".", [FirstName], [LastName]), , "([Øø])", , "oe", , ), , "[Ææ]", , "ae", , ), , "([äãàâãåáąÄÃÀÂÃÅÁĄA])", , "a", , ), , "([B])", , "b", , ), , "([CçčćÇČĆ])", , "c", , ), , "([ďĎD])", , "d", , ), , "([ëèéêęěËÈÉÊĘĚE])", , "e", , ), , "([F])", , "f", , ), , "([G])", , "g", , ), , "([H])", , "h", , ), , "([ïîìíÏÎÌÍI])", , "i", , ), , "([J])", , "j", , ), , "([K])", , "k", , ), , "([ľłŁĽL])", , "l", , ), , "([M])" ,, "m",), "([ñńňÑŃŇN])", "n",), "([öòőõôóÖÒŐÕÔÓO])", "o",), "([P])", "p",), "([Q])", "d:",), "([řŘR])", "r",), "([ßšśŠŚS])", "s",), "([TŤť])", "t",), "([üùûúůűÜÙÛÚŮŰU])", "u",), "([V])", "v",), "([W])", "w",), "([ýÿýŸÝY])", "y",), "([źžżŹŽŻZ])", "z",), "",,, "",), "contoso.com")**   | UserPrincipalName     |     | Vytvoření + aktualizace                                                   
| **Přepínače (\[okres\], "organizační jednotky standardní uživatelé, OU = Uživatelé, OU = výchozí, OU = umístění, DC = = contoso, DC = com", "Dallas", "organizační jednotky standardní uživatelé, OU = = Users, organizační jednotky Dallas, OU = umístění, DC = = contoso, DC = com", "Austinu", "organizační jednotky standardní uživatelé, OU = = oj uživatelé Austinu, OU = umístění, DC = = contoso, DC = com", "Seattle", "organizační jednotky standardní uživatelé, OU = = Users, organizační jednotky Seattle, OU = umístění, DC = = contoso, DC = com", "Praha", "organizační jednotky = standardní uživatelé Organizační jednotky Uživatelé, OU = Londýn, OU = = umístění, DC = contoso, DC = com ")**  | parentDistinguishedName     |     |  Vytvoření + aktualizace |
  
### <a name="part-3-configure-hello-on-premises-synchronization-agent"></a>Část 3: Konfigurace agenta synchronizace místní hello

V pořadí tooprovision tooActive Directory v místě musí být agent nainstalován na serveru připojeném k doméně v hello desire doménové struktury služby Active Directory. Správce domény (nebo Enterprise Admins) přihlašovací údaje jsou nutné k dokončení postupu hello.

**[Můžete si stáhnout hello místní agent synchronizace sem](https://go.microsoft.com/fwlink/?linkid=847801)**

Po instalaci agenta, spusťte příkazy prostředí Powershell hello níže tooconfigure hello agenta pro vaše prostředí.

**Příkaz #1**

> CD C:\\soubory programu\\Agent synchronizace služby Active Directory Microsoft Azure\\moduly\\AADSyncAgent

> Import-module AADSyncAgent.psd1

**Příkaz #2**

> Přidat ADSyncAgentActiveDirectoryConfiguration

* Vstup: "Název adresáře", zadejte název doménové struktury hello AD, zadané v části \#2
* Vstup: Jméno a heslo správce pro doménovou strukturu služby Active Directory

**Příkaz #3**

> Přidat ADSyncAgentAzureActiveDirectoryConfiguration

* Vstup: Globální jméno a heslo správce pro vašeho tenanta Azure AD

**Příkaz #4**

> Get-AdSyncAgentProvisioningTasks

* Akce: Zkontrolujte, že data jsou vrácena. Tento příkaz automaticky vyhledá Workday zřizování aplikací v klientovi služby Azure AD. Příklad výstupu:

> Název: AD doménové struktury
>
> Povoleno: True
>
> DirectoryName: mydomain.contoso.com
>
> Credentialed: False
>
> Identifikátor: WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

**Příkaz #5**

> Spuštění AdSyncAgentSynchronization-automatické

**Příkaz #6**

> net stop aadsyncagent

**Příkaz #7**

> net start aadsyncagent

### <a name="part-4-start-hello-service"></a>Část 4: Spuštění hello služby
Jakmile částí 1 – 3 byly dokončeny, můžete začít hello zřizování služby zpět v hello portálu pro správu Azure.

1.  V hello **zřizování** kartě, sada hello **Stav zřizování** k **na**.

2. Klikněte na **Uložit**.

3. Tato akce spustí počáteční synchronizaci hello, může trvat proměnný počet hodin v závislosti na tom, kolik uživatelé jsou v Workday.

4. Jednotlivé synchronizační událostmi, jako je například jaké uživatelé jsou při čtení z Workday a následně přidané nebo aktualizované tooActive adresáře, můžete následně zobrazit v hello **protokoly auditu** kartě. **[Najdete v části průvodce vytvářením sestav pro podrobné pokyny o tom, jak protokoly auditu hello tooread zřizování hello](active-directory-saas-provisioning-reporting.md)**

5.  Hello protokolu aplikací systému Windows v počítači agenta hello se zobrazí všechny operace, které se provádí prostřednictvím hello agenta.

6. Byla dokončena, bude zapisovat souhrnné zprávy o auditu **zřizování** kartě, jak je uvedeno níže.

![portál Azure](./media/active-directory-saas-workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-tooazure-active-directory"></a>Konfigurace služby Active Directory tooAzure zřizování uživatelů
Jak nakonfigurovat tooAzure zřizování služby Active Directory bude záviset na požadavků zřizování podle popisu v tabulce hello.

| Scénář | Řešení |
| -------- | -------- |
| **Uživatelé potřebují toobe zřízený tooActive Directory a Azure AD** | Použití  **[AAD Connect](connect/active-directory-aadconnect.md)** |
| **Uživatelé třeba toobe zřízený tooActive Directory jenom** | Použití  **[AAD Connect](connect/active-directory-aadconnect.md)** |
| **Uživatelé potřebují toobe zřízený tooAzure AD pouze (jenom cloud)** | Použití hello **zřizování služby Active Directory tooAzure Workday** aplikace v galerii aplikací hello |

Pokyny k nastavení služby Azure AD Connect, najdete v části hello [dokumentace Azure AD Connect](connect/active-directory-aadconnect.md).

Hello následující oddíly popisují nastavení připojení mezi Workday a Azure AD uživatele jenom pro cloud tooprovision.

> [!IMPORTANT]
> Pouze podle níže uvedeného postupu hello, pokud máte jenom pro cloud uživatelům, kteří potřebují tooAzure toobe zřízený AD a ne místní služby Active Directory.

### <a name="part-1-adding-hello-azure-ad-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a>Část 1: Přidání zřizování konektor aplikace hello Azure AD a vytvoření připojení tooWorkday hello

**tooconfigure Workday tooAzure služby Active Directory zřizování pro uživatele jenom pro cloud:**

1.  Přejděte příliš<https://portal.azure.com>.

2.  V levém navigačním panelu hello vyberte **Azure Active Directory**

3.  Vyberte **podnikové aplikace, které**, pak **všechny aplikace**.

4.  Vyberte **přidat aplikaci**a potom vyberte hello **všechny** kategorie.

5.  Vyhledejte **Workday tooAzure AD zřizování**a přidejte tuto aplikaci z Galerie hello.

6.  Po hello aplikace přidána, se zobrazí úvodní obrazovka podrobnosti o aplikaci, vyberte **zřizování**

7.  Změna hello **zřizování** **režimu** příliš**automatické**

8.  Dokončení hello **přihlašovací údaje správce** části následujícím způsobem:

   * **Uživatelské jméno správce** – zadejte uživatelské jméno hello hello Workday integrace systémový účet s připojeným názvem domény klienta hello. By měl vypadat podobně jako:username@contoso4

   * **Heslo správce –** heslo zadejte hello hello Workday integrace systémový účet

   * **URL – klienta** zadejte hello toohello Workday webové služby koncový bod adresy URL pro vašeho klienta. To by měl vypadat jako: https://wd3-impl-services1.workday.com/ccx/service/contoso4, kde contoso4 se nahradí název vašeho klienta správné a wd3 impl se nahradí hello prostředí správný řetězec (v případě potřeby).

   * **E-mailové oznámení –** zadejte e-mailovou adresu a zaškrtněte políčko "odeslání e-mailu, pokud dojde k selhání".

   * Klikněte na tlačítko hello **Test připojení** tlačítko.

   * Pokud hello test připojení úspěšný, klikněte na tlačítko hello **Uložit** tlačítka v horní části hello. Pokud se nezdaří, zkontrolujte že hello Workday URL a přihlašovací údaje jsou platné ve Workday.


### <a name="part-2-configure-attribute-mappings"></a>Část 2: Konfigurace mapování atributů 

V této části nakonfigurujete uživatelská data tok z Workday do Azure Active Directory pro uživatele jenom pro cloud.

1.  Na kartě zřizování hello pod **mapování**, klikněte na tlačítko **synchronizovat pracovníci tooAzure AD**.

2.   V hello **zdrojového objektu oboru** pole, můžete vybrat, které skupiny uživatelů v Workday by měla být v oboru pro zřizování tooAzure AD, definováním sadu filtrů založená na atributu. Výchozí rozsah Hello je "všichni uživatelé v Workday". Příklad filtrů:

   * Příklad: Obor toousers s ID pracovní mezi 1000000 a 2000000

      * Atribut: WorkerID

      * Operátor: Shoda REGEX

      * Hodnota: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Příklad: Pouze contingent pracovníků a ne regulární zaměstnanci

      * Atribut: ContingentID

      * Operátor: Není NULL.

3.  V hello **cílový objekt akce** pole, můžete globálně filtrovat, jaké akce jsou povoleny toobe provést na Azure AD. **Vytvoření** a **aktualizace** jsou nejběžnější.

4.  V hello **mapování atributů** části, můžete definovat, jak jednotlivé Workday atributy mapování atributů adresáře tooActive.

5. Klikněte na existující mapování tooupdate atribut, nebo klikněte na tlačítko **přidat nové mapování** dolnímu hello hello obrazovky tooadd nové mapování. Mapování u jednotlivých atribut podporuje tyto vlastnosti:

   * **Typ mapování**

      * **Přímé** – zapíše hello hodnotu atributu hello Workday atribut toohello AD, beze změn

      * **Konstantní** -zapsat hodnotu statické, konstantní řetězec do atribut hello AD

      * **Výraz** – vám umožní toowrite vlastní hodnotu atributu hello AD, založené na jeden nebo více atributů Workday. [Další informace najdete v článku na výrazy](active-directory-saas-writing-expressions-for-attribute-mappings.md).

   * **Zdrojový atribut** -hello atribut uživatele z Workday.

   * **Výchozí hodnota** – volitelné. Pokud zdrojový atribut hello má prázdnou hodnotu, hello mapování zapíše tuto hodnotu.
            Nejběžnější konfigurace je tooleave tomto prázdné.

   * **Atribut target** – hello atribut uživatele ve službě Azure AD.

   * **Objekty používající tento atribut odpovídat** – zda by měl použít toto mapování toouniquely identifikaci uživatelů mezi Workday a Azure AD. To se obvykle nastavuje na pole ID pracovní pro Workday, který je obvykle namapována do atribut rozšíření nebo atribut ID zaměstnance hello (Nový) ve službě Azure AD.

   * **Odpovídající prioritu** – více odpovídající atributy lze nastavit. Pokud existuje více vyhodnocují se v pořadí podle tohoto pole. Jakmile je nalezena shoda, žádné další odpovídající atributy vyhodnocují.

   * **Použít toto mapování**

     * **Vždy** – použít toto mapování na obou vytvoření uživatele a aktualizovat akce

     * **Pouze během vytváření** -použít toto mapování pouze na akcí vytvoření uživatele

6. Klikněte na tlačítko toosave mapování, **Uložit** hello horní části mapování atributů.

### <a name="part-3-start-hello-service"></a>Část 3: Spuštění hello služby
Jakmile částí 1 – 2 byly dokončeny, můžete začít hello zřizováním služby.

1.  V hello **zřizování** kartě, sada hello **Stav zřizování** k **na**.

2. Klikněte na **Uložit**.

3. Tato akce spustí počáteční synchronizaci hello, může trvat proměnný počet hodin v závislosti na tom, kolik uživatelé jsou v Workday.

4. Jednotlivé synchronizační události lze zobrazit v hello **protokoly auditu** kartě. **[Najdete v části průvodce vytvářením sestav pro podrobné pokyny o tom, jak protokoly auditu hello tooread zřizování hello](active-directory-saas-provisioning-reporting.md)**

5. Byla dokončena, bude zapisovat souhrnné zprávy o auditu **zřizování** kartě, jak je uvedeno níže.


## <a name="configuring-writeback-of-email-addresses-tooworkday"></a>Konfigurace zpětný zápis tooWorkday e-mailové adresy
Postupujte podle těchto pokynů tooconfigure zpětný zápis uživatele e-mailových adres z tooWorkday Azure Active Directory.

### <a name="part-1-adding-hello-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a>Část 1: Přidání hello zřizování konektor aplikace a vytvoření připojení tooWorkday hello

**tooconfigure Workday tooActive Directory zřizování:**

1.  Přejděte příliš<https://portal.azure.com>

2.  V levém navigačním panelu hello vyberte **Azure Active Directory**

3.  Vyberte **podnikové aplikace, které**, pak **všechny aplikace**.

4.  Vyberte **přidat aplikaci**, pak vyberte hello **všechny** kategorie.

5.  Vyhledejte **zpětný zápis Workday**a přidejte tuto aplikaci z Galerie hello.

6.  Po hello aplikace přidána, se zobrazí úvodní obrazovka podrobnosti o aplikaci, vyberte **zřizování**

7.  Změna hello **zřizování** **režimu** příliš**automatické**

8.  Dokončení hello **přihlašovací údaje správce** části následujícím způsobem:

   * **Uživatelské jméno správce** – zadejte uživatelské jméno hello hello Workday integrace systémový účet s připojeným názvem domény klienta hello. By měl vypadat podobně jako:username@contoso4

   * **Heslo správce –** heslo zadejte hello hello Workday integrace systémový účet

   * **URL – klienta** zadejte hello toohello Workday webové služby koncový bod adresy URL pro vašeho klienta. To by měl vypadat jako: https://wd3-impl-services1.workday.com/ccx/service/contoso4, kde contoso4 se nahradí název vašeho klienta správné a wd3 impl se nahradí hello prostředí správný řetězec (v případě potřeby).

   * **E-mailové oznámení –** zadejte e-mailovou adresu a zaškrtněte políčko "odeslání e-mailu, pokud dojde k selhání".

   * Klikněte na tlačítko hello **Test připojení** tlačítko. Pokud hello test připojení úspěšný, klikněte na tlačítko hello **Uložit** tlačítka v horní části hello. Pokud se nezdaří, zkontrolujte že hello Workday URL a přihlašovací údaje jsou platné ve Workday.


### <a name="part-2-configure-attribute-mappings"></a>Část 2: Konfigurace mapování atributů 


V této části nakonfigurujete, jak jsou data uživatele z Workday do služby Active Directory.

1.  Na kartě zřizování hello pod **mapování**, klikněte na tlačítko **synchronizaci Azure AD Uživatelé tooWorkday**.

2.  V hello **zdrojového objektu oboru** pole, můžete volitelně filtrovat, které skupiny uživatelů v Azure Active Directory by měly mít jejich e-mailové adresy zapsány zpět tooWorkday. Výchozí rozsah Hello je "všichni uživatelé ve službě Azure AD". 

3.  V hello **mapování atributů** části, můžete definovat, jak jednotlivé Workday atributy mapování atributů adresáře tooActive. Existuje mapování pro hello e-mailovou adresu ve výchozím nastavení. Hello odpovídající ID však musí být aktualizované toomatch uživatelů ve službě Azure Active Directory s jejich odpovídající položky Workday. Je Oblíbené odpovídající metoda toosynchronize hello Workday pracovní ID nebo zaměstnanec ID tooextensionAttribute1 15 ve službě Azure AD a pak použít tento atribut toomatch uživatele Azure AD zpátky v Workday.

4.  Klikněte na tlačítko toosave mapování, **Uložit** hello horní části hello mapování atributů části.

### <a name="part-3-start-hello-service"></a>Část 3: Spuštění hello služby
Jakmile částí 1 – 2 byly dokončeny, můžete začít hello zřizováním služby.

1.  V hello **zřizování** kartě, sada hello **Stav zřizování** k **na**.

2. Klikněte na **Uložit**.

3. Tato akce spustí počáteční synchronizaci hello, může trvat proměnný počet hodin v závislosti na tom, kolik uživatelé jsou v Workday.

4. Jednotlivé synchronizační události lze zobrazit v hello **protokoly auditu** kartě. **[Najdete v části průvodce vytvářením sestav pro podrobné pokyny o tom, jak protokoly auditu hello tooread zřizování hello](active-directory-saas-provisioning-reporting.md)**

5. Byla dokončena, bude zapisovat souhrnné zprávy o auditu **zřizování** kartě, jak je uvedeno níže.

## <a name="known-issues"></a>Známé problémy

* **Protokoly v Evropského národní prostředí auditu** – podle hello verze této verze technical Preview, je známý problém s hello [protokoly auditu](active-directory-saas-provisioning-reporting.md) pro hello Workday konektor aplikace nejsou uvedena v hello [portál Azure](https://portal.azure.com) Pokud klienta hello Azure AD se nachází v Evropského datového centra. Oprava pro tento problém je chystaný. Zkontrolujte, zda tento prostor v blízké budoucnosti aktualizací hello znovu. 

## <a name="additional-resources"></a>Další zdroje
* [Kurz: Konfigurace jednotného přihlašování mezi Workday a Azure Active Directory](active-directory-saas-workday-tutorial.md)
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Další kroky

* [Zjistěte, jak tooreview protokoly a získat sestavy o zřizování aktivity](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting)
