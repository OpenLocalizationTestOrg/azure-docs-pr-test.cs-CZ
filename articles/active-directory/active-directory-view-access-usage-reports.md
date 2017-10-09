---
title: "aaaView přístupu a použití sestav | Microsoft Docs"
description: "Vysvětluje, jak tooview přístupu a použití sestav toogain aspekty hello integrity a zabezpečení adresáře vaší organizace."
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: a074bc4e-cf3f-4ad1-8cc6-4199d2e09ce4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 1c18fd2a327ae8b67f62ce2754f643bdb03514a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-your-access-and-usage-reports"></a>Zobrazení sestav přístupů a používání
*Tato dokumentace je součástí hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).*

Můžete použít Azure Active Directory přístup a použití sestav toogain přehled hello integrity a zabezpečení adresáře vaší organizace. Tyto informace a správce directory pomohou určit, kde může být bezpečnostním rizikům, tak, aby se adekvátní naplánovat toomitigate těchto rizik.

V hello portálu pro správu Azure sestavy jsou rozdělené do hello následující způsoby:

* Sestavy anomálií – Contain přihlašovací události, že jsme našli toobe neobvyklé. Naším cílem je toomake si vědom tyto aktivity a díky kterému budete mít toomake toobe rozhodnutí o tom, zda je událost podezřelé.
* Integrované sestavy aplikací – poskytuje přehled o tom, jak cloudové aplikace jsou používány ve vaší organizaci. Azure Active Directory umožňuje integraci s tisíci cloudových aplikací.
* Zprávy o chybách – Označit chyby, které mohou nastat při zřizování účtů tooexternal aplikace.
* Uživatelská sestavy – zobrazení zařízení nebo přihlašovací údaje aktivity pro konkrétního uživatele.
* Protokoly aktivity – Contain záznam všech auditované události v rámci hello posledních 24 hodin, posledních 7 dnů, nebo posledních 30 dní, a také změny aktivity skupiny a aktivita resetování a registraci hesla.

> [!NOTE]
> * Některé rozšířené sestavy o využití anomálií a prostředky jsou k dispozici, pouze pokud povolíte [Azure Active Directory Premium](active-directory-get-started-premium.md). Rozšířené sestavy pomohou vylepšit zabezpečení přístupu odpovídají toopotential hrozeb a získat přístup k tooanalytics na využití zařízení přístup a aplikace.
> * Edice Azure Active Directory Premium a Basic jsou k dispozici zákazníkům v Číně pomocí hello celosvětové instance služby Azure Active Directory. Edice Azure Active Directory Premium a Basic nejsou aktuálně podporované ve službě Microsoft Azure hello provozované v Číně společností 21Vianet. Další informace, kontaktujte nás na hello [fóru služby Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).
> 
> 

## <a name="reports"></a>Reports
| Sestava | Popis |
| --- | --- |
| **Sestavy neobvyklé aktivity** | |
| [Přihlášení z neznámých zdrojů](active-directory-reporting-sign-ins-from-unknown-sources.md) |Může znamenat pokusu o toosign v bez trasovány. |
| [Přihlášení po několika selháních](active-directory-reporting-sign-ins-after-multiple-failures.md) |Může znamenat útok hrubou silou úspěšné. |
| [Přihlášení z několika zeměpisných oblastí](active-directory-reporting-sign-ins-from-multiple-geographies.md) |Může znamenat, že více uživatelů se přihlašujete hello stejný účet. |
| [Přihlášení z IP adres s podezřelou aktivitou](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md) |Může znamenat úspěšného přihlášení po pokusu o dlouhodobě neoprávněných vniknutí. |
| [Přihlášení z pravděpodobně nakažených zařízení](active-directory-reporting-sign-ins-from-possibly-infected-devices.md) |Může znamenat toosign pokus v z pravděpodobně nakažených zařízení. |
| [Nestandardní přihlašovací aktivita](active-directory-reporting-irregular-sign-in-activity.md) |Může znamenat události neobvyklé toousers se přihlášení vzory. |
| [Uživatelé s neobvyklou přihlašovací aktivitou](active-directory-reporting-users-with-anomalous-sign-in-activity.md) |Označuje uživatelů, jejíž účty ohrožení. |
| Uživatelé s uniklými přihlašovacími údaji |Uživatelé s uniklými přihlašovacími údaji |
| **Protokoly aktivity** | |
| Sestava auditu |Auditované události ve vašem adresáři |
| Aktivity resetování hesla |Poskytuje podrobný přehled o resetování hesla, které nastat ve vaší organizaci. |
| Registrace aktivita resetování hesla |Poskytuje že podrobný přehled o heslo resetovat registrace, ke kterým dochází ve vaší organizaci. |
| Aktivita samoobslužných skupin |Poskytuje aktivity protokolu tooall skupiny samoobslužné aktivity ve vašem adresáři |
| **Integrované aplikace** | |
| Využití aplikací |Poskytuje souhrnné informace o využití pro všechny aplikace SaaS integrované s adresářem. |
| Účet zřizování aktivity |Poskytuje historii pokusů tooprovision účty tooexternal aplikacím. |
| Stav Změna hesla |Poskytuje podrobný přehled o stavu výměny automatické hesla aplikací SaaS. |
| Chyby zřizování účtů |Označuje aplikace tooexternal přístup toousers dopad. |
| **Rights management** | |
| Použití služby RMS |Poskytuje souhrn pro využití Rights Management |
| Většina aktivní uživatele RMS |Uvádí nejvyšší 1000 active uživatelé získat přístup k chráněnému službou RMS soubory |
| Použití zařízení RMS |Zobrazí seznam zařízení použít pro soubory přístupu k chráněnému službou RMS |
| Využívání aplikací RMS povoleno |Poskytuje možnosti pro aplikace s podporou využití služby RMS |

## <a name="report-editions"></a>Sestava edice
| Sestava | Free | Basic | Premium |
| --- | --- | --- | --- |
| **Sestavy neobvyklé aktivity** | | | |
| Přihlášení z neznámých zdrojů |✓ |✓ |✓ |
| Přihlášení po několika selháních |✓ |✓ |✓ |
| Přihlášení z několika zeměpisných oblastí |✓ |✓ |✓ |
| Přihlášení z IP adres s podezřelou aktivitou | | |✓ |
| Přihlášení z pravděpodobně nakažených zařízení | | |✓ |
| Nestandardní přihlašovací aktivita | | |✓ |
| Uživatelé s neobvyklou přihlašovací aktivitou | | |✓ |
| Uživatelé s uniklými přihlašovacími údaji | | |✓ |
| **Protokoly aktivity** | | | |
| Sestava auditu |✓ |✓ |✓ |
| Aktivity resetování hesla | | |✓ |
| Registrace aktivita resetování hesla | | |✓ |
| Aktivita samoobslužných skupin | | |✓ |
| **Integrované aplikace** | | | |
| Využití aplikací | | |✓ |
| Účet zřizování aktivity |✓ |✓ |✓ |
| Stav Změna hesla | | |✓ |
| Chyby zřizování účtů |✓ |✓ |✓ |
| **Rights management** | | | |
| Použití služby RMS | | |Pouze služba RMS |
| Většina aktivní uživatele RMS | | |Pouze služba RMS |
| Použití zařízení RMS | | |Pouze služba RMS |
| Využívání aplikací RMS povoleno | | |Pouze služba RMS |

## <a name="anomalous-activity-reports"></a>Sestavy neobvyklé aktivity
<p>Hello neobvyklá přihlášení sestavy aktivit příznak podezřelé přihlašovací aktivity tooOffice365, portálu pro správu Azure, přístupový Panel Azure AD, Sharepoint Online, Dynamics CRM Online a dalších služeb Microsoft online services.</p>

<p>Všechny tyto sestavy, s výjimkou hello "Přihlášení po několika selháních" sestava také příznak podezřelé <i>federovaný</i> přihlášení služby, zmíněnými in toohello bez ohledu na to poskytovatel federace hello. </p>

<p>k dispozici jsou Hello následující sestavy: </p><ul>

<li>[Přihlášení z neznámých zdrojů](active-directory-reporting-sign-ins-from-unknown-sources.md).</li>

<li>[Přihlášení po několika selháních](active-directory-reporting-sign-ins-after-multiple-failures.md).</li>

<li>[Přihlášení z několika zeměpisných oblastí](active-directory-reporting-sign-ins-from-multiple-geographies.md).</li>

<li>[Přihlášení z IP adres s podezřelou aktivitou](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md).</li>

<li>[Nestandardní přihlašovací aktivita](active-directory-reporting-irregular-sign-in-activity.md).</li>

<li>[Přihlášení z pravděpodobně nakažených zařízení](active-directory-reporting-sign-ins-from-possibly-infected-devices.md).</li>

<li>[Uživatelé s neobvyklou přihlašovací aktivitou](active-directory-reporting-users-with-anomalous-sign-in-activity.md).</li>

<li>Uživatelé s uniklými přihlašovacími údaji</li></ul>

## <a name="activity-logs"></a>Protokoly aktivit
### <a name="audit-report"></a>Sestava auditu
| Popis | Umístění sestavy |
|:--- |:--- |
| Zobrazí záznam všech auditované události v rámci hello posledních 24 hodin, posledních 7 dní a posledních 30 dnů. <br /> Další informace najdete v tématu [události sestavy auditování Azure ve službě Active Directory](active-directory-reporting-audit-events.md) |Adresář > kartě sestavy |

![Sestava auditu](./media/active-directory-view-access-usage-reports/auditReport.PNG)

### <a name="password-reset-activity"></a>Aktivity resetování hesla
| Popis | Umístění sestavy |
|:--- |:--- |
| Ukazuje, že všechny heslo resetovat pokusy, k nimž došlo ve vaší organizaci. |Adresář > kartě sestavy |

![Aktivity resetování hesla](./media/active-directory-view-access-usage-reports/passwordResetActivity.PNG)

### <a name="password-reset-registration-activity"></a>Registrace aktivita resetování hesla
| Popis | Umístění sestavy |
|:--- |:--- |
| Ukazuje, že všechny heslo resetovat registrace, které mají ve vaší organizaci došlo k chybě |Adresář > kartě sestavy |

![Registrace aktivita resetování hesla](./media/active-directory-view-access-usage-reports/passwordResetRegistrationActivity.PNG)

### <a name="self-service-groups-activity"></a>Aktivita samoobslužných skupin
| Popis | Umístění sestavy |
|:--- |:--- |
| Zobrazuje všechny činnost hello samoobslužných spravované skupin ve vašem adresáři. |Adresář > Uživatelé > <i>uživatele</i> > karta zařízení |

![Aktivita samoobslužných skupin](./media/active-directory-view-access-usage-reports/selfServiceGroupsActivity.PNG)

## <a name="integrated-applications-reports"></a>Sestavy integrované aplikace
### <a name="application-usage-summary"></a>Využití aplikací: souhrn
| Popis | Umístění sestavy |
|:--- |:--- |
| Pomocí této sestavy, pokud chcete toosee využití pro všechny aplikace SaaS hello ve vašem adresáři. Tato sestava vychází hello počet pokusů, které uživatelé jste klikli na hello aplikace hello přístupového panelu. |Adresář > kartě sestavy |

Tato sestava zahrnuje přihlášení příliš*všechny* aplikace, které má adresáře přístup, včetně předem integrovaných aplikací společnosti Microsoft.

Předem integrovaných aplikací společnosti Microsoft zahrnují Office 365, Sharepoint, hello portálu pro správu Azure a dalších.

![Souhrn využití aplikací](./media/active-directory-view-access-usage-reports/applicationUsage.PNG)

### <a name="application-usage-detailed"></a>Využití aplikací: podrobnosti
| Popis | Umístění sestavy |
|:--- |:--- |
| Pomocí této sestavy, pokud chcete toosee používá kolik konkrétní aplikaci SaaS. Tato sestava vychází hello počet pokusů, které uživatelé jste klikli na hello aplikace hello přístupového panelu. |Adresář > kartě sestavy |

### <a name="application-dashboard"></a>Řídicí panel aplikací
| Popis | Umístění sestavy |
|:--- |:--- |
| Tato sestava označuje aplikace toohello kumulativní sign in Uživatelé ve vaší organizaci v vybrané časovém intervalu. Graf Hello na stránce řídicího panelu hello vám pomůže identifikovat trendy pro všechny použití této aplikace. |Adresář > aplikace > řídicí panel |

## <a name="error-reports"></a>Zprávy o chybách
### <a name="account-provisioning-errors"></a>Chyby zřizování účtů
| Popis | Umístění sestavy |
|:--- |:--- |
| Pomocí této toomonitor chyby, ke kterým došlo během synchronizace hello účtů z tooAzure aplikace SaaS služby Active Directory. |Adresář > kartě sestavy |

![Chyby zřizování účtů](./media/active-directory-view-access-usage-reports/accountProvisioningErrors.PNG)

## <a name="user-specific-reports"></a>Sestavy specifické pro uživatele
### <a name="devices"></a>Zařízení
| Popis | Umístění sestavy |
|:--- |:--- |
| Pomocí této sestavy, pokud chcete toosee hello IP adresu a zeměpisné umístění zařízení se tooaccess Azure Active Directory má použít pro konkrétního uživatele. |Adresář > Uživatelé > <i>uživatele</i> > karta zařízení |

### <a name="activity"></a>Aktivita
| Popis | Umístění sestavy |
|:--- |:--- |
| Zobrazuje hello přihlašovací aktivity pro uživatele. Hello sestava obsahuje informace, jako jsou aplikace hello přihlášeni, zařízení používá, IP adresa a umístění. Neshromažďujeme hello historie pro uživatele, kteří se přihlaste pomocí účtu Microsoft. |Adresář > Uživatelé > <i>uživatele</i> > karta aktivity |

#### <a name="sign-in-events-included-in-hello-user-activity-report"></a>Přihlaste se události, které jsou součástí hello aktivity uživatelů sestavy
Pouze určité typy přihlášení události se zobrazí v hello sestavy aktivity uživatelů.

| Typ události | Zahrnuté? |
| --- | --- |
| Podepsat in toohello [přístupového panelu](http://myapps.microsoft.com/) |Ano |
| Podepsat in toohello [portálu pro správu Azure](https://manage.windowsazure.com/) |Ano |
| Podepsat in toohello [portálu Microsoft Azure](https://portal.azure.com/) |Ano |
| Podepsat in toohello [portál Office 365](http://portal.office.com/) |Ano |
| Podepsat in tooa nativní aplikace, jako je Outlook (viz výjimky uvedené níže) |Ano |
| Podepsat aplikaci federovaný zřízený in tooa prostřednictvím hello přístupového panelu, jako je Salesforce |Ano |
| Podepsání aplikace založené na heslech in tooa prostřednictvím hello přístupového panelu, jako jsou služby Twitter. |Ano |
| Přihlašovací in tooa vlastní obchodní aplikace, který byl přidán toohello adresáře |Žádný (už brzy) |
| Podepsat aplikaci Azure AD Application Proxy tooan in, který byl přidán toohello adresáře |Žádný (už brzy) |

> Poznámka: tooreduce hello množství šumu v této sestavě, přihlášení pomocí hello [Microsoft Online Services Sign-In Assistant](http://community.office365.com/en-us/w/sso/534.aspx) nejsou zobrazeny.
> 
> 

## <a name="things-tooconsider-if-you-suspect-security-breach"></a>Tooconsider věcí, pokud máte podezření na narušení zabezpečení
Pokud máte podezření, že uživatelský účet může být ohrožena nebo jakýkoli druh uživatele podezřelé aktivity, která může způsobit, že tooa porušení zabezpečení vašich dat adresáře v cloudu hello, může být vhodné tooconsider jeden nebo více hello následující akce:

* Obraťte se na aktivity hello tooverify hello uživatelů
* Resetování hesla hello uživatele
* [Povolit službu Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md) pro lepší zabezpečení.

## <a name="view-or-download-a-report"></a>Zobrazit nebo stáhnout sestavu
1. V hello portál Azure classic, klikněte na **služby Active Directory**, klikněte na název hello adresáře vaší organizace a pak klikněte na tlačítko **sestavy**.
2. Na stránce hello sestavy, klikněte na sestavu hello chcete tooview nebo stažení.
   
   > [!NOTE]
   > Pokud je to hello poprvé použijete hello reporting funkce služby Azure Active Directory, zobrazí se zpráva tooOpt v. Pokud souhlasíte, klikněte na tlačítko toocontinue ikonu značky zaškrtnutí hello.
   > 
   > 
3. Klikněte na tlačítko Další tooInterval hello rozevírací nabídky a pak vyberte jeden z následujících časových rozsahů, které se mají použít při generování této sestavy hello:
   
   * Posledních 24 hodin
   * Posledních 7 dnů
   * Posledních 30 dnů
4. Klikněte na hello zaškrtnutí ikonu toorun hello sestavu.
   * Až too1000 v hello portál Azure classic zobrazí události.
5. Pokud je k dispozici, klikněte na tlačítko **Stáhnout** toodownload hello tooa komprimovaný soubor sestavy ve formátu hodnot oddělených čárkami (CSV) pro offline zobrazení nebo archivačním účelům.
   * Až too75 bude 000 událostí součástí hello stáhnout soubor.
   * Pro další data, podívejte se na hello [rozhraní API pro vytváření sestav Azure AD](active-directory-reporting-api-getting-started.md).

## <a name="ignore-an-event"></a>Ignorovat událost
Při zobrazení všech sestav anomálií, můžete si všimnout, že můžete ignorovat různé události, které se zobrazí v související sestavy. jednoduše zvýrazněte událost hello v sestavě hello tooignore na události a potom klikněte na **Ignorovat**. Hello **Ignorovat** tlačítko hello zvýrazněná událost se trvale odebrat ze sestavy hello a lze použít pouze licencovanou globální správce.

## <a name="automatic-email-notifications"></a>Automatické e-mailových oznámení
Pro další informace o Azure AD je reporting oznámení, podívejte se na [Azure Active Directory Reporting oznámení](active-directory-reporting-notifications.md).

## <a name="whats-next"></a>Kam dál
* [Začínáme se službou Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Přidání firemního brandingu tooyour stránky přihlásit a přístupového panelu](active-directory-add-company-branding.md)

