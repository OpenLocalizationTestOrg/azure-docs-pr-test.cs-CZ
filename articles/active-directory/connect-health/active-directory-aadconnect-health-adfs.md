---
title: "aaaUsing Azure AD Connect Health se službou AD FS | Microsoft Docs"
description: "Toto je stránka hello Azure AD Connect Health jak toomonitor místní infrastrukturu AD FS."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0cd26e8762be65e09d22e1f113e5165c4f131715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-ad-fs-using-azure-ad-connect-health"></a>Sledování služby AD FS pomocí služby Azure AD Connect Health
Hello následující dokumentace je konkrétní toomonitoring infrastrukturu služby AD FS pomocí Azure AD Connect Health. Informace o sledování služby Azure AD Connect (Sync) pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md). Informace o sledování služby Active Directory Domain Services pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Upozornění služby AD FS
Hello části výstrahy Azure AD Connect Health poskytuje že Hello seznam aktivních výstrah. Každé upozornění obsahuje důležité informace, postup řešení a odkazy toorelated dokumentaci.

Dvojitým kliknutím aktivní nebo vyřešený výstrah, tooopen nové okno s doplňujícími informacemi, kroky, které můžete provést tooresolve hello výstrahy a odkazy toorelevant dokumentaci. Můžete také zobrazit historická data na výstrahy, které byly vyřešeny hello posledních.

![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>Funkce analýzy využití služby AD FS
Azure AD Connect Health analýzy využití analyzuje hello ověřovací provoz na federačních serverech. Poklepáním na hello políčka funkce analýzy využití, tooopen hello využití analytics okně se zobrazí několik metriky a seskupení.

> [!NOTE]
> toouse analýzy využití se službou AD FS, musíte zajistit, že je povoleno auditování služby AD FS. Další informace najdete v článku o [povolení auditování služby AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health/report1.png)

tooselect další metriky, určit časový rozsah, nebo seskupení hello toochange, klikněte pravým tlačítkem na graf analýzy využití hello a vyberte upravit graf. Potom můžete zadat hello časový rozsah, vyberte jiné metriky a změnit seskupení hello. Můžete zobrazit hello distribuci ověřovacího provozu hello podle různých "metrik" a jednotlivé metriky pomocí parametrů relevantní "Seskupit podle", které jsou popsané v následující části hello skupiny:

**Metrika: Celkový počet požadavků** – Celkový počet požadavků zpracovaných servery AD FS.

|Seskupit podle | Co hello seskupení znamená a proč je užitečné? |
| --- | --- |
| Všechny | Zobrazuje počet hello celkový počet požadavků zpracovaných ve všech serverech služby AD FS.|
| Aplikace | Skupiny hello celkový počet požadavků podle cílové hello předávající strany. Toto seskupení je užitečné toounderstand, které jednotlivé aplikace přijímají procentem celkového provozu hello. |
|  Server |Skupiny hello celkový počet požadavků podle hello serveru, který zpracovává požadavek hello. Toto seskupení je užitečné toounderstand hello distribucí zatížení celkového provozu hello.
| Připojení k pracovišti |Skupiny hello celkový počet požadavků podle na tom, jestli jsou přicházející ze zařízení, které jsou připojené k síti na pracovišti (známá). Toto seskupení je užitečné toounderstand, pokud vaše prostředky ke kterým se přistupuje pomocí zařízení, které jsou neznámé toohello identity infrastruktury. |
|  Metoda ověřování | Skupiny hello celkový počet požadavků podle hello metodu ověřování ověřování. Toto seskupení je užitečné toounderstand hello běžné metoda ověřování, která se k ověřování používá. Následují hello možné metody ověřování. <ol> <li>integrované ověřování systému Windows (Windows)</li> <li>ověřování pomocí formulářů (formuláře)</li> <li>jednotné přihlašování (SSO)</li> <li>ověření certifikátem X509 (certifikát)</li> <br>Pokud hello federační servery přijmout žádost o hello pomocí souboru Cookie jednotného přihlašování, tento požadavek se počítá jako jednotné přihlašování (jednotné přihlašování). V takových případech Pokud je platný, soubor cookie hello hello uživatele není vyzváni tooprovide přihlašovací údaje a získá bezproblémový přístup toohello aplikaci. Toto chování je běžné v případě, že máte několik přijímajících stran, které jsou chráněné federačními servery hello. |
| Umístění v síti | Skupiny hello celkový počet požadavků podle umístění v síti hello hello uživatele. Může to být intranet nebo extranet. Toto seskupení je užitečné tooknow, jaké procento hello provoz pochází z hello intranet a vnější síť. |


**Metriky: Vyžádat celkový počet se nezdařilo** -hello celkový počet neúspěšných požadavků zpracovaných službou hello federation service. (Tato metrika je dostupná pouze ve službě AD FS pro Windows Server 2012 R2.)

|Seskupit podle | Co hello seskupení znamená a proč je užitečné? |
| --- | --- |
| Typ chyby | Znázorňuje hello počet chyb podle předdefinovaných typů chyb. Toto seskupení je užitečné toounderstand hello běžnými typy chyb. <ul><li>Nesprávné uživatelské jméno nebo heslo: chyby kvůli tooincorrect uživatelské jméno nebo heslo.</li> <li>"Uzamčení extranetu": selhání kvůli toohello požadavky přijatými od uživatele, který byl uzamčen z extranetu </li><li> "Prošlé heslo": selhání kvůli toousers přihlásíte platnost jeho hesla.</li><li>"Deaktivovaný účet": selhání kvůli toousers protokolování pomocí deaktivovaného účtu.</li><li>"Ověřování zařízení": chyby z důvodu selhání toousers tooauthenticate pomocí ověření zařízení.</li><li>"Ověřování certifikátu uživatele": chyby z důvodu selhání toousers tooauthenticate kvůli neplatnému certifikátu.</li><li>"MFA": chyby z důvodu selhání toouser tooauthenticate pomocí služby Multi-Factor Authentication.</li><li>"Jiné přihlašovací údaje": "Autorizace vystavení": chyby z důvodu selhání tooauthorization.</li><li>"Delegování vystavení": chyby z důvodu chyby tooissuance delegování.</li><li>"Přijetí tokenu": selhání kvůli tooADFS odmítat hello tokenu od poskytovatele Identity jiných výrobců.</li><li>(Protocol): Chyba z důvodu chyby tooprotocol.</li><li>„Neznámá“: Zachytit vše. Jakékoli jiné chyby, které se nehodí do hello definované kategorie.</li> |
| Server | Skupiny hello chyby podle serveru hello. Toto seskupení je užitečné toounderstand hello Chyba distribuce mezi servery. Nerovnoměrná distribuce může naznačovat vadný stav serveru. |
| Umístění v síti | Skupiny hello chyby podle umístění v síti hello hello požadavků (intranet vs. extranet). Toto seskupení je užitečné toounderstand hello typ neúspěšných požadavků. |
|  Aplikace | Skupiny hello chyby podle hello cílové aplikace (přijímající strany). Toto seskupení je užitečné toounderstand, která cílová aplikace zaznamenává největší počet chyb. |

**Metrika: Počet uživatelů** – Průměrný počet jedinečných uživatelů aktivně ověřujících pomocí AD FS

|Seskupit podle | Co hello seskupení znamená a proč je užitečné? |
| --- | --- |
|Všechny |Tato metrika poskytuje počet průměrný počet uživatelů pomocí služby FS hello v hello vybraném časovém intervalu. Hello uživatelé nejsou seskupení. <br>průměr Hello závisí na vybraném časovém intervalu hello. |
| Aplikace |Skupiny hello průměrný počet uživatelů podle hello cílové aplikace (přijímající strany). Toto seskupení je užitečné toounderstand počet uživatelů, kteří používají aplikaci, pro kterou. |

## <a name="performance-monitoring-for-ad-fs"></a>Sledování výkonu služby AD FS
Sledování výkonu služby Azure AD Connect Health poskytuje sledovací informace o metrikách. Zaškrtnutím políčka sledování hello, otevře se nové okno s podrobné informace o metrikách hello.

![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health/perf1.png)

Pokud vyberete možnost Filter hello hello horní části okna hello, můžete filtrovat podle serveru toosee metriky na jednotlivých serverech. Metrika toochange, klikněte pravým tlačítkem na hello monitorování grafu v části monitorování okno hello a vyberte upravit graf (nebo tlačítko Upravit graf vyberte hello). Z hello nové okno, které se otevře můžete vybrat další metriky pomocí rozevíracího seznamu hello a zadejte časový interval pro zobrazení dat výkonu hello.

## <a name="reports-for-ad-fs"></a>Sestavy služby AD FS
Azure AD Connect Health poskytuje sestavy s informacemi o činnosti a výkonu služby AD FS. Tyto sestavy pomáhají správcům získat přehled o aktivitách na jejich serverech AD FS.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>Nejčastějších 50 uživatelů s neúspěšným přihlášením kvůli uživatelskému jména nebo heslu
Jednou z běžných příčin neúspěšného požadavku na ověření na serveru služby AD FS hello je žádost s neplatnými přihlašovacími údaji, tedy nesprávným uživatelským jménem nebo heslo. Obvykle se stane toousers kvůli toocomplex hesla, zapomenutým heslům nebo překlepům.

Existují i jiné důvody, které může mít za následek neočekávaný počet požadavků zpracovanou server služby AD FS, jako například: aplikace, která vyprší platnost mezipaměti přihlašovací údaje uživatele a přihlašovací údaje hello nebo uživatel se zlými úmysly pokus toosign k účtu pomocí řady známých hesel. Tyto dva příklady jsou platné důvodům, které by mohlo vést tooa nárůst v žádosti.

Azure AD Connect Health pro ADFS poskytuje sestavy o prvních 50 uživatelů s neúspěšných pokusů o přihlášení z důvodu tooinvalid uživatelské jméno nebo heslo. Tuto sestavu můžete dosáhnout zpracováním událostí auditu hello generované všechny servery hello služby AD FS ve farmách hello

![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report1a.png)

V rámci této sestavy máte snadný přístup toohello následující informace:

* Celkový počet neúspěšných požadavků s nesprávné uživatelské jméno a heslo v hello posledních 30 dnů
* Průměrný počet uživatelů, kteří mají problém s přihlašováním kvůli chybnému uživatelskému jménu nebo heslu, za jeden den.

Kliknutím na tuto část přejdete okna toohello hlavní sestavy, které poskytuje další podrobnosti. Toto okno obsahuje graf s trendů informace toohelp stanovení základní úrovně o požadavků s nesprávným uživatelským jménem nebo heslem. Kromě toho poskytuje hello seznam prvních 50 uživatelů s hello počet neúspěšných pokusů o přihlášení.

Hello graf obsahuje hello následující informace:

* Hello celkový počet neúspěšných přihlášení z důvodu tooa chybný uživatelského jména a hesla na denní bázi.
* Hello celkový počet jedinečných uživatelů s neúspěšným přihlášení na denní bázi.
* IP adresa klienta posledního požadavku

![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report3a.png)

Sestava Hello obsahuje hello následující informace:

| Položky sestavy | Popis |
| --- | --- |
| ID uživatele |Zobrazuje hello ID uživatele, který byl použit. Tato hodnota je co hello uživatele zadali, což v některých případech je hello chybné uživatelské ID, které používá. |
| Neúspěšné pokusy |Ukazuje hello celkový počet neúspěšných pokusů o přihlášení pro ID tohoto konkrétního uživatele. Hello tabulka je řazená hello nejvíce počet neúspěšných pokusů v sestupném pořadí. |
| Poslední chyba |Zobrazuje časové razítko hello když hello poslední došlo k chybě. |
| IP adresa poslední chyby |Zobrazuje hello IP adresa klienta z hello nejnovější chybný požadavek. |

> [!NOTE]
> Tato sestava se automaticky aktualizuje po každé dvě hodiny s hello nové informace shromážděné během této doby. V důsledku toho nemusí být pokusů o přihlášení v rámci hello poslední dvě hodiny zahrnuty v sestavě hello.
>
>

## <a name="related-links"></a>Související odkazy
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalace agenta služby Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)