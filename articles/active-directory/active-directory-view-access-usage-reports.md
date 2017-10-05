---
title: "Zobrazení sestav přístupu a použití | Microsoft Docs"
description: "Vysvětluje, jak k zobrazení sestav využití a přístupu k proniknout do integrity a zabezpečení adresáři vaší organizace."
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
ms.openlocfilehash: 038ac79ebf61c6429fbf7ca21eefe9414bcfc03a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-your-access-and-usage-reports"></a>Zobrazení sestav přístupů a používání
*Tato dokumentace je součástí [Příručky generování sestav v Azure Active Directory](active-directory-reporting-guide.md).*

Přístup k Azure Active Directory a sestavy využití, které slouží k získat přehled o integrity a zabezpečení adresáři vaší organizace. Tyto informace a správce directory pomohou určit, kde může být bezpečnostním rizikům, tak, aby adekvátní můžete naplánovat zmírnění.

Na portálu správy Azure sestavy jsou rozdělené do následujících způsobů:

* Sestavy anomálií – Contain přihlášení události, které je neobvyklé. Naším cílem je mít budete vědět, tyto aktivity a díky kterému budete mít možnost provádět rozhodnutí o tom, zda je událost podezřelé.
* Integrované sestavy aplikací – poskytuje přehled o tom, jak cloudové aplikace jsou používány ve vaší organizaci. Azure Active Directory umožňuje integraci s tisíci cloudových aplikací.
* Zprávy o chybách – Označit chyby, které mohou nastat při zřizování účtů k externím aplikacím.
* Uživatelská sestavy – zobrazení zařízení nebo přihlašovací údaje aktivity pro konkrétního uživatele.
* Protokoly aktivity – Contain záznam všech auditované události v posledních 24 hodin, posledních 7 dnů, nebo posledních 30 dní, a také změny aktivity skupiny a aktivita resetování a registraci hesla.

> [!NOTE]
> * Některé rozšířené sestavy o využití anomálií a prostředky jsou k dispozici, pouze pokud povolíte [Azure Active Directory Premium](active-directory-get-started-premium.md). Rozšířené sestavy můžete vylepšit zabezpečení přístupu, reagovat na případné hrozby a získat přístup k analýze na využití zařízení přístup a aplikace.
> * Edice Premium a Basic služby Azure Active Directory jsou zákazníkům v Číně dostupné prostřednictvím celosvětové instance služby Azure Active Directory. Edice Premium a Basic služby Azure Active Directory nejsou aktuálně podporované ve službě Microsoft Azure provozované v Číně společností 21Vianet. Další informace si vyžádejte na [fóru služby Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).
> 
> 

## <a name="reports"></a>Reports
| Sestava | Popis |
| --- | --- |
| **Sestavy neobvyklé aktivity** | |
| [Přihlášení z neznámých zdrojů](active-directory-reporting-sign-ins-from-unknown-sources.md) |Může znamenat pokus o přihlášení bez trasovány. |
| [Přihlášení po několika selháních](active-directory-reporting-sign-ins-after-multiple-failures.md) |Může znamenat útok hrubou silou úspěšné. |
| [Přihlášení z několika zeměpisných oblastí](active-directory-reporting-sign-ins-from-multiple-geographies.md) |Může znamenat, že více uživatelů se přihlašují pomocí stejného účtu. |
| [Přihlášení z IP adres s podezřelou aktivitou](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md) |Může znamenat úspěšného přihlášení po pokusu o dlouhodobě neoprávněných vniknutí. |
| [Přihlášení z pravděpodobně nakažených zařízení](active-directory-reporting-sign-ins-from-possibly-infected-devices.md) |Může znamenat pokus o přihlášení z pravděpodobně nakažených zařízení. |
| [Nestandardní přihlašovací aktivita](active-directory-reporting-irregular-sign-in-activity.md) |Může znamenat do uživatelova přihlášení vzory neobvyklé události. |
| [Uživatelé s neobvyklou přihlašovací aktivitou](active-directory-reporting-users-with-anomalous-sign-in-activity.md) |Označuje uživatelů, jejíž účty ohrožení. |
| Uživatelé s uniklými přihlašovacími údaji |Uživatelé s uniklými přihlašovacími údaji |
| **Protokoly aktivity** | |
| Sestava auditu |Auditované události ve vašem adresáři |
| Aktivity resetování hesla |Poskytuje podrobný přehled o resetování hesla, které nastat ve vaší organizaci. |
| Registrace aktivita resetování hesla |Poskytuje že podrobný přehled o heslo resetovat registrace, ke kterým dochází ve vaší organizaci. |
| Aktivita samoobslužných skupin |Poskytuje protokol činnosti pro všechny skupiny samoobslužné aktivity ve vašem adresáři |
| **Integrované aplikace** | |
| Využití aplikací |Poskytuje souhrnné informace o využití pro všechny aplikace SaaS integrované s adresářem. |
| Účet zřizování aktivity |Poskytuje historii pokusy o zřízení účty k externím aplikacím. |
| Stav Změna hesla |Poskytuje podrobný přehled o stavu výměny automatické hesla aplikací SaaS. |
| Chyby zřizování účtů |Označuje vliv na přístup uživatelů k externím aplikacím. |
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
<p>Neobvyklé přihlašovací aktivity sestavy příznak přihlašovací podezřelé aktivity pro Office 365, portálu pro správu Azure, přístupový Panel Azure AD, Sharepoint Online, Dynamics CRM Online a dalších služeb Microsoft online services.</p>

<p>Všechny tyto sestavy, s výjimkou sestavy "Přihlášení po několika selháních" také příznak podezřelé <i>federovaný</i> přihlášení zmíněnými služby, bez ohledu na to poskytovatel federace. </p>

<p>Jsou k dispozici následující sestavy: </p><ul>

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
| Zobrazí záznam všech auditované události v posledních 24 hodin, posledních 7 dní a posledních 30 dnů. <br /> Další informace najdete v tématu [události sestavy auditování Azure ve službě Active Directory](active-directory-reporting-audit-events.md) |Adresář > kartě sestavy |

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
| Zobrazí všechny aktivity pro samoobslužné služby spravované skupiny ve vašem adresáři. |Adresář > Uživatelé > <i>uživatele</i> > karta zařízení |

![Aktivita samoobslužných skupin](./media/active-directory-view-access-usage-reports/selfServiceGroupsActivity.PNG)

## <a name="integrated-applications-reports"></a>Sestavy integrované aplikace
### <a name="application-usage-summary"></a>Využití aplikací: souhrn
| Popis | Umístění sestavy |
|:--- |:--- |
| Pomocí této sestavy, pokud chcete zobrazit využití pro všechny aplikace SaaS ve vašem adresáři. Tato sestava je založena na počtu časy, kdy uživatelé klikli na aplikaci na přístupovém panelu. |Adresář > kartě sestavy |

Tato sestava obsahuje sign in se *všechny* aplikace, které má adresáře přístup, včetně předem integrovaných aplikací společnosti Microsoft.

Předem integrovaných aplikací společnosti Microsoft zahrnují Office 365, Sharepoint, portálu pro správu Azure a dalších.

![Souhrn využití aplikací](./media/active-directory-view-access-usage-reports/applicationUsage.PNG)

### <a name="application-usage-detailed"></a>Využití aplikací: podrobnosti
| Popis | Umístění sestavy |
|:--- |:--- |
| Pomocí této sestavy, pokud chcete zobrazit, kolik konkrétní aplikace SaaS je používán. Tato sestava je založena na počtu časy, kdy uživatelé klikli na aplikaci na přístupovém panelu. |Adresář > kartě sestavy |

### <a name="application-dashboard"></a>Řídicí panel aplikací
| Popis | Umístění sestavy |
|:--- |:--- |
| Tato sestava označuje kumulativní sign in na aplikaci uživatelé ve vaší organizaci v vybrané časovém intervalu. Graf na stránce řídicího panelu vám pomůže identifikovat trendy pro všechny použití této aplikace. |Adresář > aplikace > řídicí panel |

## <a name="error-reports"></a>Zprávy o chybách
### <a name="account-provisioning-errors"></a>Chyby zřizování účtů
| Popis | Umístění sestavy |
|:--- |:--- |
| Použijte ke sledování chyb, ke kterým došlo během synchronizace účtů z aplikací SaaS do Azure Active Directory. |Adresář > kartě sestavy |

![Chyby zřizování účtů](./media/active-directory-view-access-usage-reports/accountProvisioningErrors.PNG)

## <a name="user-specific-reports"></a>Sestavy specifické pro uživatele
### <a name="devices"></a>Zařízení
| Popis | Umístění sestavy |
|:--- |:--- |
| Pomocí této sestavy, pokud chcete zobrazit IP adresu a zeměpisné umístění zařízení, která konkrétní uživatel byl použit pro přístup k Azure Active Directory. |Adresář > Uživatelé > <i>uživatele</i> > karta zařízení |

### <a name="activity"></a>Aktivita
| Popis | Umístění sestavy |
|:--- |:--- |
| Zobrazuje přihlašovací aktivity pro uživatele. Sestava obsahuje informace, jako jsou aplikace přihlášeni, zařízení používá, IP adresa a umístění. Neshromažďujeme historie pro uživatele, kteří se přihlaste pomocí účtu Microsoft. |Adresář > Uživatelé > <i>uživatele</i> > karta aktivity |

#### <a name="sign-in-events-included-in-the-user-activity-report"></a>Přihlaste se události, které jsou zahrnuty do sestavy aktivity uživatelů
Pouze určité typy přihlášení události se zobrazí v sestavě aktivity uživatelů.

| Typ události | Zahrnuté? |
| --- | --- |
| Přihlášení k [přístup k panelu](http://myapps.microsoft.com/) |Ano |
| Přihlášení k [portál pro správu Azure](https://manage.windowsazure.com/) |Ano |
| Přihlášení k [portálu Microsoft Azure](https://portal.azure.com/) |Ano |
| Přihlášení k [portál Office 365](http://portal.office.com/) |Ano |
| Přihlášení k nativní aplikaci, jako je Outlook (viz výjimky uvedené níže) |Ano |
| Přihlášení do aplikace federovaný zřízené prostřednictvím přístupového panelu, jako je Salesforce |Ano |
| Přihlášení do aplikace založené na heslech prostřednictvím přístupového panelu, jako jsou služby Twitter. |Ano |
| Přihlášení do vlastní obchodní aplikace, který byl přidán do adresáře |Žádný (už brzy) |
| Přihlášení do aplikace Azure AD Application Proxy, který byl přidán do adresáře |Žádný (už brzy) |

> Poznámka: Chcete-li omezit množství šumu v této sestavě, přihlášení pomocí [Microsoft Online Services Sign-In Assistant](http://community.office365.com/en-us/w/sso/534.aspx) nejsou zobrazeny.
> 
> 

## <a name="things-to-consider-if-you-suspect-security-breach"></a>Co je třeba zvážit, pokud máte podezření porušení zabezpečení
Pokud máte podezření, že uživatelský účet může dojít k ohrožení a jakýkoli druh uživatele podezřelé aktivity, která může vést k porušení zabezpečení vašich dat adresáře v cloudu, je vhodné vzít v úvahu jeden nebo více z následujících akcí:

* Kontaktovat uživatele a ověřte aktivitu
* Resetovat heslo uživatele
* [Povolit službu Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md) pro lepší zabezpečení.

## <a name="view-or-download-a-report"></a>Zobrazit nebo stáhnout sestavu
1. Na portálu Azure classic klikněte na tlačítko **služby Active Directory**, klikněte na název adresáře vaší organizace a pak klikněte na tlačítko **sestavy**.
2. Na stránce sestavy klikněte na sestavu, kterou chcete zobrazit nebo stáhnout.
   
   > [!NOTE]
   > Pokud je to první, kdy jste použili funkci vytváření sestav služby Azure Active Directory, zobrazí se zprávy, která se vyjádřit výslovný v. Pokud souhlasíte, klikněte na ikonu značky zaškrtnutí pokračujte.
   > 
   > 
3. Klikněte na rozevírací nabídku vedle intervalu a potom vyberte jednu z následujících časových rozsahů, které se mají použít při generování této sestavy:
   
   * Posledních 24 hodin
   * Posledních 7 dnů
   * Posledních 30 dnů
4. Klikněte na ikonu značky zaškrtnutí pro spuštění sestavy.
   * Až 1 000 událostí se zobrazí na portálu Azure classic.
5. Pokud je k dispozici, klikněte na tlačítko **Stáhnout** stahování sestavu komprimovaný soubor ve formátu hodnot oddělených čárkami (CSV) pro offline zobrazení nebo archivačním účelům.
   * Až než 75 000 událostí bude součástí staženého souboru.
   * Pro další data, podívejte se [rozhraní API pro vytváření sestav Azure AD](active-directory-reporting-api-getting-started.md).

## <a name="ignore-an-event"></a>Ignorovat událost
Při zobrazení všech sestav anomálií, můžete si všimnout, že můžete ignorovat různé události, které se zobrazí v související sestavy. Ignorovat událost, jednoduše zvýrazněte událost v sestavě a potom klikněte na **Ignorovat**. **Ignorovat** tlačítko zvýrazněná událost se trvale odebrat ze sestavy a lze použít pouze licencovanou globální správce.

## <a name="automatic-email-notifications"></a>Automatické e-mailových oznámení
Pro další informace o Azure AD je reporting oznámení, podívejte se na [Azure Active Directory Reporting oznámení](active-directory-reporting-notifications.md).

## <a name="whats-next"></a>Kam dál
* [Začínáme se službou Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Přidání firemního brandingu na přihlašovací stránku a na stránku přístupového panelu](active-directory-add-company-branding.md)

