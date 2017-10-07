---
title: "aaaAzure ochrany identit služby Active Directory | Microsoft Docs"
description: "Zjistěte, jak Azure AD Identity Protection vám umožní toolimit hello schopnost útočník tooexploit ohroženými identity nebo zařízení a toosecure identity nebo zařízení, která byla dříve toobe podezření nebo známých ohrožení zabezpečení."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ecca4f3cdb65585687cf44a80024f26c7cab22ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

Azure Active Directory ochrany identit je funkce hello Azure AD Premium P2 edici, která vám umožní:

- Zjistit potenciální ohrožení zabezpečení, které ovlivňují identity ve vaší organizaci

- Konfigurace toodetected automatické odpovědi podezřelé se akce, které jsou identity související tooyour organizace  

- Prozkoumat podezřelé incidenty a proveďte příslušnou akci tooresolve je   


## <a name="getting-started"></a>Začínáme

Microsoft zabezpečuje cloudové identity pro více než deset. V Azure Active Directory Identity Protection, ve vašem prostředí, můžete použít hello stejné ochrany systémy Microsoft používá toosecure identity.

velká většina Hello zabezpečení narušení trvat umístit pokud útočník přístup tooan prostředí tak, že krádež identity uživatele. V průběhu let hello se staly útočníci stále efektivní využívání narušení třetích stran a za použití útoky phishing sofistikované. Jakmile útočník získá přístup tooeven nízkou privilegované uživatelských účtů, je pro ně toogain přístup tooimportant firemním prostředkům prostřednictvím laterální pohyb je poměrně snadné.

V důsledku toho je potřeba:

- Chránit všechny identity, bez ohledu na jejich úroveň oprávnění

- Proaktivní ohroženými identity zabránit se překročen

Zjišťování ohrožení zabezpečení identity je bez snadno úlohy. Azure Active Directory používá algoritmy adaptivní strojového učení a heuristiky toodetect anomálií a podezřelé incidenty, které indikují potenciálně ohrožených identity. Na základě těchto dat Identity Protection generuje sestavy a výstrahy, které umožňují tooevaluate hello zjištěné problémy a proveďte příslušné zmírnění nebo nápravné akce.

Azure Active Directory Identity Protection je větší než monitorování a vytváření sestav nástroje. tooprotect identity ve vaší organizaci, můžete nakonfigurovat zásady na základě rizik, které automaticky odpovídat toodetected problémy po dosažení zadané riziko úroveň. Tyto zásady, kromě tooother podmíněný přístup k ovládací prvky, které poskytuje Azure Active Directory a EMS, můžete buď automaticky blokovat nebo zahájit adaptivní nápravné akce, včetně resetování hesel a vynucení služby Multi-Factor authentication.


#### <a name="identity-protection-capabilities"></a>Funkce ochrany identit

**Zjišťování ohrožení zabezpečení a rizikové účty:**  

* Poskytování vlastních doporučení tooimprove celkové postavení zabezpečení podle zvýraznění ohrožení zabezpečení
* Výpočet úrovní rizika přihlášení
* Výpočet úrovní rizika uživatele


**Zkoumání rizikových událostí:**

* Odesílání oznámení o rizikových událostech
* Zkoumání rizikových událostí pomocí relevantní a kontextové informace
* Poskytuje základní pracovních tootrack šetření
* Poskytuje snadný přístup tooremediation akcí, jako je resetování hesla

**Zásady podmíněného přístupu na základě rizika:**

* Zásady toomitigate rizikové přihlášení blokování přihlášení nebo že vyřeší problémy spojené služby Multi-Factor authentication.
* Zásady tooblock nebo zabezpečený rizikové uživatelské účty
* Tooregister uživatelé toorequire zásad pro službu Multi-Factor authentication



## <a name="identity-protection-roles"></a>Role ochranu identity

tooload vyrovnávání hello správu činnosti týkající se vaší implementace ochrany identit, můžete přiřadit několik rolí. Azure AD Identity Protection podporuje 3 directory role:

| Role                         | Můžete provést                          | Nelze provést
| :--                          | ---                                |  ---   |
| Globální správce         | Úplný přístup tooIdentity ochranu zařadit Identity Protection| |
| Správce zabezpečení       | Úplný přístup tooIdentity ochrany | Zařadit Identity Protection resetovat hesla pro uživatele |
| Čtenář zabezpečení              | Přístup jen připravené tooIdentity ochrany | Zařadit Identity Protection, uživatelé remidiate, nakonfigurovat zásady, resetování hesla |




Další podrobnosti najdete v tématu [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)



## <a name="detection"></a>Detection (Detekce)

### <a name="vulnerabilities"></a>Ohrožení zabezpečení

Azure Active Directory Identity Protection analýz konfiguraci a zjistí chyby zabezpečení, které můžou mít vliv na identit uživatelů. Další podrobnosti najdete v tématu [chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).

### <a name="risk-events"></a>Riziko události

Azure Active Directory používá adaptivní strojového učení algoritmů a heuristiky podezřelé akce toodetect, které jsou související tooyour uživatelských identit. Hello systém vytvoří záznam pro každé zjištěné podezřelé akce. Tyto záznamy se také označují jako rizikových událostech.  
Další podrobnosti najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).


## <a name="investigation"></a>Šetření
Vaše cesty přes Identity Protection obvykle začíná řídicího panelu ochrany identit hello.

![Náprava](./media/active-directory-identityprotection/1000.png "nápravy")

řídicí panel Hello poskytuje přístup k:

* Sestavy, jako **uživatelé označení příznakem rizik**, **rizik události** a **ohrožení zabezpečení**
* Nastavení, jako například hello konfiguraci vašeho **zásady zabezpečení**, **oznámení** a **registrace služby Multi-Factor authentication**

Je obvykle počáteční bod pro šetření, což je proces hello kontrole hello aktivity, protokoly a další důležité informace související tooa rizik toodecide událostí, zda jsou potřebné kroky ke zmírnění nebo nápravy, a jak byl hello identity dojde k ohrožení a pochopit, jak hello ohrožené identity byl použit.

Dokáže spojit vaše aktivity toohello šetření [oznámení](active-directory-identityprotection-notifications.md) Azure Active Directory Protection odešle na e-mailu.

Hello následující části poskytují další podrobnosti a hello kroky, které jsou související tooan šetření.  


## <a name="risky-sign-ins"></a>Rizikové přihlášení

Azure Active Directory zjistí [rizik typů událostí](active-directory-reporting-risk-events.md#risk-event-types) v reálném čase a offline. Každý riziko událost, která byla zjištěna u přihlášení uživatele přispívá tooa logický pojem názvem rizikové přihlášení. Rizikové přihlášení je indikátorem pro pokusu přihlášení, který nemusí provedly legitimní vlastníkem hello uživatelského účtu.


### <a name="sign-in-risk-level"></a>Úroveň rizika přihlášení

Úroveň rizika přihlášení je označením (vysoká, střední nebo nízká) hello pravděpodobnost, že pokus o přihlášení nebyla provedena legitimní vlastníkem hello uživatelského účtu.

### <a name="mitigating-sign-in-risk-events"></a>Zmírnění rizika přihlašovací události

Omezení rizik je funkce hello toolimit akce útočník tooexploit ohroženými identity nebo zařízení, aniž byste obnovili hello identity nebo zařízení tooa bezpečné stavu. Zmírnění nevyřeší předchozí události přihlášení riziko spojené s hello identity nebo zařízení.

toomitigate rizikové přihlášení automaticky, můžete nakonfigurovat policicies přihlášení riziko zabezpečení. Pomocí těchto zásad, zvažte úroveň rizika hello hello uživatele nebo hello přihlášení tooblock rizikové přihlášení nebo vyžadovat hello uživatele tooperform vícefaktorové ověřování. Tyto akce může zabránit útočníkům ve využívání odcizené identity toocause poškození a může poskytnout některé čas toosecure hello identity.

### <a name="sign-in-risk-security-policy"></a>Zásady zabezpečení riziko přihlášení
Zásady přihlášení riziko je zásadu podmíněného přístupu, která vyhodnotí hello riziko tooa konkrétní přihlášení a použije způsoby zmírnění rizik na základě předem definované podmínky a pravidla.

![Zásady přihlášení riziko](./media/active-directory-identityprotection/1014.png "zásad riziko přihlašování")

Azure AD Identity Protection pomáhá spravovat hello zmírnění rizikové přihlášení tím, že vám umožňuje:

* Sada hello uživatelů a skupin hello zásad platí pro:

    ![Zásady přihlášení riziko](./media/active-directory-identityprotection/1015.png "zásad riziko přihlašování")
* Nastavení hello přihlášení riziko úrovně prahové hodnoty (nízká, střední nebo vysokou), která spustí hello zásad:

    ![Zásady přihlášení riziko](./media/active-directory-identityprotection/1016.png "zásad riziko přihlašování")
* Sada hello ovládací prvky toobe vynutí při aktivuje hello zásad:  

    ![Zásady přihlášení riziko](./media/active-directory-identityprotection/1017.png "zásad riziko přihlašování")
* Stav přepínače hello zásad:

    ![Registrace MFA](./media/active-directory-identityprotection/403.png "registrace MFA")
* Kontrola a vyhodnocení hello dopad změny před aktivací ho:

    ![Zásady přihlášení riziko](./media/active-directory-identityprotection/1018.png "zásad riziko přihlašování")

#### <a name="what-you-need-tooknow"></a>Co je třeba tooknow
Můžete nakonfigurovat přihlášení riziko zabezpečení zásady toorequire službou Multi-Factor authentication:

![Zásady přihlášení riziko](./media/active-directory-identityprotection/1017.png "zásad riziko přihlašování")

Ale z bezpečnostních důvodů se toto nastavení funguje pouze pro uživatele, kteří již byl registrován pro službu Multi-Factor authentication. Pokud se pro uživatele, který dosud není registrován u služby Multi-Factor authentication je splnit hello podmínku toorequire služby Multi-Factor authentication, uživatel hello je blokován.

Jako osvědčený postup Pokud chcete, aby toorequire vícefaktorového ověřování pro rizikové přihlášení, proveďte následující kroky:

1. Povolit hello [zásady registrace služby Multi-Factor authentication](#multi-factor-authentication-registration-policy) hello vliv na uživatele.
2. Vyžadovat hello ovlivněn toologin uživatelé v tooperform-rizikové relace registrace MFA

Dokončení těchto kroků zajistí, že je vyžadované pro rizikové přihlášení vícefaktorové ověřování.

#### <a name="best-practices"></a>Osvědčené postupy
Výběr **vysokou** prahová hodnota snižuje hello stanovený počet zásady se aktivuje a minimalizuje dopad toousers hello.  

Ale vyloučí **nízká** a **střední** přihlášení příznakem rizik z hello zásady, které nemusí blokovat útočník z zneužitím ohrožení zabezpečení identity.

Při nastavení hello zásad

* Vyloučit uživatele, kteří nepodporují / nemůže mít vícefaktorového ověřování
* Vyloučit uživatele v národní prostředí, kde není praktické povolit zásady hello (například žádný přístup toohelpdesk)
* Vyloučení uživatelé, kteří jsou pravděpodobně toogenerate spoustu false pozitivních (vývojáři, analytikům zabezpečení)
* Použití **vysokou** prahová hodnota během počáteční zásadách, nebo pokud minimalizujete musí výzvy pohledu koncové uživatele.
* Použití **nízká** prahovou hodnotu, pokud vaše organizace vyžaduje vyšší úroveň zabezpečení. Výběr **nízká** prahová hodnota zavádí další uživatelské přihlašovací výzvy, ale zvýšení zabezpečení.

doporučené výchozí pro většinu organizací je tooconfigure pravidlo pro Hello **střední** prahová hodnota toostrike rovnováhu mezi využitelností a zabezpečení.

zásady přihlášení riziko Hello je:

* Použité tooall prohlížeče provoz a přihlášení pomocí moderní ověřování.
* Není použité tooapplications pomocí starší protokolů zabezpečení zakázáním hello WS-Trust koncovému bodu IDP hello federovaný, jako je například služba AD FS.

Hello **rizikových událostech** stránky v konzole Identity Protection hello zobrazuje všechny události:

* Použití této zásady
* Můžete zkontrolovat hello aktivity a zjistit, zda byla akce hello odpovídající nebo ne

Přehled hello související uživatelské prostředí, najdete v části:

* [Obnovení rizikové přihlášení](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [Rizikové přihlášení blokován](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [Možnosti přihlášení s Azure AD Identity Protection](active-directory-identityprotection-flows.md)  

**Dialogové okno související konfigurace hello tooopen**:

- Na hello **Azure AD Identity Protection** okno, v hello **konfigurace** klikněte na tlačítko **zásad přihlašování riziko**.

    ![Zásady uživatele ridk](./media/active-directory-identityprotection/1014.png "ridk zásady uživatele")



## <a name="users-flagged-for-risk"></a>Uživatelé označení příznakem rizika

Všechny aktivní [rizik události](active-directory-identity-protection-risk-events.md) , byly zjištěny službou Azure Active Directory pro uživatele přispívat tooa logický pojem názvem riziko pro uživatele. Uživatele s příznakem pro riziko je indikátorem pro uživatelský účet, který byl napaden.

![Uživatelé označení příznakem rizika](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a>Úroveň rizika uživatele

Úroveň rizika uživatel je to znamenat (vysoká, střední nebo nízká) hello pravděpodobnost, že identita hello uživatele došlo k ohrožení zabezpečení. Se počítá na základě na hello uživatelem riziko události, které jsou přidružené k identitě uživatele.

Hello stav události riziko je buď **Active** nebo **uzavřeno**. Pouze riziko události, které jsou **Active** přispívat toohello uživatele riziko úrovně výpočtu.

úroveň rizika uživatele Hello se počítá pomocí hello následující zadání:

* Aktivní rizikových událostí, které mají vliv hello uživatele
* Úroveň rizika tyto události
* Jestli nebyly provedeny žádné nápravné akce

![Uživatel rizika](./media/active-directory-identityprotection/1031.png "rizika uživatele")

Můžete použít hello uživatele riziko úrovně toocreate zásady podmíněného přístupu které zablokovat rizikové uživatelům přihlášení nebo vynutit je toosecurely změnit své heslo.

### <a name="closing-risk-events-manually"></a>Zavřením rizikových událostí ručně

Ve většině případů bude trvat nápravné akce, jako je zabezpečené heslo resetovat tooautomatically zavřít rizikových událostech. Ale to nemusí být vždy možné.  
Toto je, například hello případu, kdy:

* Uživatel s Active rizikových událostí byl odstraněn.
* Šetření zjistí, že hlášené riziko událostí byl provést legitimní uživatel hello

Protože rizikových událostech, které jsou **Active** přispívat výpočtu riziko toohello uživatele, může mít nižší úroveň rizika ukončením rizikových událostí ručně toomanually.  
Během hello během šetření můžete zvolit tootake některé z těchto akcí toochange hello stav události rizika:

![Akce](./media/active-directory-identityprotection/34.png "akce")

* **Vyřešte** – Pokud po prozkoumání riziko událostí, trvalo akce odpovídající nápravu mimo ochrany identit a budete mít dojem, že událost riziko hello by se měly zvažovat zavřená, události hello označit jako Vyřešeno. Vyřešit události nastaví tooClosed stav hello riziko události a události hello riziko už přispějí toouser riziko.
* **Označit jako falešně pozitivní** – v některých případech můžete prozkoumat události riziko a zjistit, že byla nesprávně označena jako rizikové. Můžete snížit počet hello takové výskytů označením hello riziko událostí jako falešně pozitivní. To vám pomůže hello strojového učení algoritmy tooimprove hello klasifikace podobné události v budoucnu hello. Stav Hello falešně pozitivní událostí je příliš**uzavřeno** a už přispějí toouser riziko.
* **Ignorovat** – Pokud nebyly provedeny žádné akci automatické nápravy, ale aby hello riziko událostí toobe odebral ze seznamu active hello, můžete označit riziko událostí ignorovat a bude uzavřen hello stav události. Ignoruje události nepřispívají toouser riziko. Tato možnost by měla být použita pouze za neobvyklé okolnosti.
* **Znovu aktivovat** -riziko události, které byly ručně (výběrem **vyřešit**, **falešně pozitivní**, nebo **Ignorovat**) lze znovu aktivovat, nastavení hello Stav události zpět příliš**Active**. Opětovně aktivovaných rizikových událostech přispívat toohello uživatele riziko úrovně výpočtu. Nelze znovu aktivovat, rizikových událostech (například zabezpečené heslo resetovat) uzavřeny prostřednictvím nápravy.

**Dialogové okno související konfigurace hello tooopen**:

1. Na hello **Azure AD Identity Protection** okno, v části **prošetření**, klikněte na tlačítko **rizik události**.

    ![Resetování hesla ruční](./media/active-directory-identityprotection/1002.png "resetování hesla ruční")
2. V hello **rizik události** klikněte na riziko.

    ![Resetování hesla ruční](./media/active-directory-identityprotection/1003.png "resetování hesla ruční")
3. V okně hello rizika klikněte pravým tlačítkem na uživatele.

    ![Resetování hesla ruční](./media/active-directory-identityprotection/1004.png "resetování hesla ruční")

### <a name="closing-all-risk-events-for-a-user-manually"></a>Zavřít všechny události riziko pro uživatele ručně
Místo ručně zavřete riziko události pro uživatele jednotlivě, Azure Active Directory Identity Protection také poskytuje metoda tooclose všechny události pro uživatele s jedním kliknutím.

![Akce](./media/active-directory-identityprotection/2222.png "akce")

Když kliknete na tlačítko **zavřít všechny události**, jsou zavřeny všechny události a hello vliv na uživatele již není v ohrožení.

### <a name="remediating-user-risk-events"></a>Nekompatibilních rizikových událostí uživatele

Nápravy je akci toosecure identity nebo zařízení, která byla dříve by mohly vzbuzovat podezření nebo známé toobe ohrožení zabezpečení. Akce nápravy obnoví hello identity nebo zařízení tooa bezpečné stav a odstraňuje předchozí rizikových událostech spojených s hello identity nebo zařízení.

tooremediate uživatele rizikových událostí, můžete postupovat následovně:

* Proveďte ručně riziko událostí zabezpečené heslo resetovat tooremediate uživatele
* Konfigurace uživatel riziko zabezpečení zásady toomitigate nebo automaticky napravit uživatele rizikových událostí
* Znovu přeinstalovat image hello nakažených zařízení  

#### <a name="manual-secure-password-reset"></a>Resetování ruční zabezpečeného hesla
Resetování zabezpečeného hesla je efektivní nápravy pro mnoho událostí riziko a při provádění automaticky zavře tyto události riziko přepočítá úroveň rizika hello uživatele. Můžete použít hello Identity Protection řídicí panel tooinitiate obnovení hesla pro rizikové uživatele.

Hello příslušné dialogové okno obsahuje dvě různé metody tooreset heslo:

**Resetovat heslo** – vyberte **vyžadují hello uživatele tooreset své heslo** tooallow hello uživatele tooself obnovit, pokud má uživatel hello zaregistrován u služby Multi-Factor authentication. Během hello uživatele příštím přihlášení bude uživatel hello požadované toosolve vícefaktorového ověřování challenge úspěšně a pak, vynucené toochange hello heslo. Tato možnost není dostupná, pokud hello uživatelský účet ještě není registrované služby Multi-Factor authentication.

**Dočasné heslo** – vyberte **generovat dočasné heslo** tooimmediately zneplatnit hello stávající heslo a vytvořit nové dočasné heslo pro uživatele hello. Odešlete hello nové dočasné heslo tooan alternativní e-mailovou adresu pro hello uživatel nebo správce toohello uživatelů. Protože hello heslo je dočasný, bude uživatel hello výzvami toochange hello hesla při přihlášení.

![Zásady](./media/active-directory-identityprotection/1005.png "zásad")

**Dialogové okno související konfigurace hello tooopen**:

1. Na hello **Azure AD Identity Protection** okně klikněte na tlačítko **uživatelé označení příznakem rizik**.

    ![Resetování hesla ruční](./media/active-directory-identityprotection/1006.png "resetování hesla ruční")
2. Hello seznam uživatelů vyberte uživatele, který má alespoň jeden rizikových událostech.

    ![Resetování hesla ruční](./media/active-directory-identityprotection/1007.png "resetování hesla ruční")
3. V okně hello uživatele, klikněte na tlačítko **resetovat heslo**.

    ![Resetování hesla ruční](./media/active-directory-identityprotection/1008.png "resetování hesla ruční")

### <a name="user-risk-security-policy"></a>Zásada zabezpečení riziko uživatelů
Zásady uživatele rizik zabezpečení je zásadu podmíněného přístupu, která vyhodnotí hello riziko úrovně tooa konkrétního uživatele a použije nápravy a zmírnění akce na základě předem definované podmínky a pravidla.

![Zásady uživatele ridk](./media/active-directory-identityprotection/1009.png "ridk zásady uživatele")

Azure AD Identity Protection pomáhá spravovat hello zmírnění a náprava uživatelé označení příznakem rizik a to:

* Sada hello uživatelů a skupin hello zásad platí pro:

    ![Zásady uživatele ridk](./media/active-directory-identityprotection/1010.png "ridk zásady uživatele")
* Nastavení hello uživatele riziko úrovně prahové hodnoty (nízká, střední nebo vysokou), která spustí hello zásad:

    ![Zásady uživatele ridk](./media/active-directory-identityprotection/1011.png "ridk zásady uživatele")
* Sada hello ovládací prvky toobe vynutí při aktivuje hello zásad:

    ![Zásady uživatele ridk](./media/active-directory-identityprotection/1012.png "ridk zásady uživatele")
* Stav přepínače hello zásad:

    ![Zásady uživatele ridk](./media/active-directory-identityprotection/403.png "registrace MFA")
* Kontrola a vyhodnocení hello dopad změny před aktivací ho:

    ![Zásady uživatele ridk](./media/active-directory-identityprotection/1013.png "ridk zásady uživatele")

Výběr **vysokou** prahová hodnota snižuje hello stanovený počet zásady se aktivuje a minimalizuje dopad toousers hello.
Ale vyloučí **nízká** a **střední** uživatelé označení příznakem rizik z hello zásady, které nemusí zabezpečit identity nebo zařízení, měla by mohly vzbuzovat podezření nebo známé toobe ohrožení zabezpečení.

Při nastavení hello zásad

* Vyloučení uživatelé, kteří jsou pravděpodobně toogenerate spoustu false pozitivních (vývojáři, analytikům zabezpečení)
* Vyloučit uživatele v národní prostředí, kde není praktické povolit zásady hello (například žádný přístup toohelpdesk)
* Použití **vysokou** prahová hodnota během počáteční zásadách, nebo pokud minimalizujete musí výzvy pohledu koncové uživatele.
* Použití **nízká** prahovou hodnotu, pokud vaše organizace vyžaduje vyšší úroveň zabezpečení. Výběr **nízká** prahová hodnota zavádí další uživatelské přihlašovací výzvy, ale zvýšení zabezpečení.

doporučené výchozí pro většinu organizací je tooconfigure pravidlo pro Hello **střední** prahová hodnota toostrike rovnováhu mezi využitelností a zabezpečení.

Přehled hello související uživatelské prostředí, najdete v části:

* [Dojde k ohrožení tok obnovení účtu](active-directory-identityprotection-flows.md#compromised-account-recovery).  
* [Dojde k ohrožení účet byl uzamčen toku](active-directory-identityprotection-flows.md#compromised-account-blocked).  

**Dialogové okno související konfigurace hello tooopen**:

- Na hello **Azure AD Identity Protection** okno, v hello **konfigurace** klikněte na tlačítko **zásady uživatele riziko**.

    ![Zásady uživatele ridk](./media/active-directory-identityprotection/1009.png "ridk zásady uživatele")

### <a name="mitigating-user-risk-events"></a>Zmírnění rizik událostí uživatele
Správci mohou nastavit uživatele riziko zabezpečení zásady tooblock uživatelů při přihlašování v závislosti na úroveň rizika hello.

Blokování přihlášení:

* Zabraňuje hello generování nových událostí riziko uživatele pro hello vliv na uživatele
* Umožňuje správci toomanually napravit hello riziko událostech, které mají vliv identita uživatele hello a obnovte ji tooa zabezpečen.



## <a name="multi-factor-authentication-registration-policy"></a>Zásady registrace služby Multi-Factor authentication
Ověřování Azure Multi-Factor authentication je metoda ověřování, který jste vyžadující hello použití více než jen uživatelské jméno a heslo. Poskytuje druhou vrstvu zabezpečení toouser přihlášení a transakce.  
Doporučujeme vyžadovat ověřování Azure Multi-Factor authentication pro přihlášení uživatele, protože ho:

* Poskytuje silné ověřování s celou řadu možností snadno ověření
* Hraje důležitou roli při přípravě tooprotect vaší organizace a obnovit z účtu ohrožení

![Zásady uživatele ridk](./media/active-directory-identityprotection/1019.png "ridk zásady uživatele")

Další podrobnosti najdete v tématu [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)

Azure AD Identity Protection pomáhá spravovat hello zavádění registrace služby Multi-Factor authentication tím, že nakonfigurujete zásadu, která umožňuje:

* Sada hello uživatelů a skupin hello zásad platí pro:

    ![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1020.png "zásad vícefaktorového ověřování")
* Sada hello ovládací prvky toobe vynutí při hello zásad aktivuje::  

    ![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1021.png "zásad vícefaktorového ověřování")
* Stav přepínače hello zásad:

    ![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/403.png "zásad vícefaktorového ověřování")
* Zobrazit aktuální stav registrace hello:

    ![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1022.png "zásad vícefaktorového ověřování")

Přehled hello související uživatelské prostředí, najdete v části:

* [Postup registrace služby Multi-Factor authentication](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  
* [Přihlášení vyskytne s Azure AD Identity Protection](active-directory-identityprotection-flows.md).  

**Dialogové okno související konfigurace hello tooopen**:

- Na hello **Azure AD Identity Protection** okno, v hello **konfigurace** klikněte na tlačítko **registrace služby Multi-Factor authentication**.

    ![Zásady vícefaktorového ověřování](./media/active-directory-identityprotection/1019.png "zásad vícefaktorového ověřování")

## <a name="next-steps"></a>Další kroky
* [Kanál 9: Azure AD a Identity zobrazení: Identity Protection verze Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [Povolení ochrany identit Azure Active Directory](active-directory-identityprotection-enable.md)

* [Chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md)

* [Azure Active Directory rizikových událostí](active-directory-identity-protection-risk-events.md)

* [Azure oznámení ochrany identit služby Active Directory](active-directory-identityprotection-notifications.md)

* [Azure seznam strategií ochrany identit Active Directory](active-directory-identityprotection-playbook.md)

* [Azure slovník ochrany identit služby Active Directory](active-directory-identityprotection-glossary.md)

* [Možnosti přihlášení s Azure AD Identity Protection](active-directory-identityprotection-flows.md)

* [Azure Active Directory identitu ochrana – jak toounblock uživatelů](active-directory-identityprotection-unblock-howto.md)

* [Začínáme s Azure Active Directory Identity Protection a Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)
