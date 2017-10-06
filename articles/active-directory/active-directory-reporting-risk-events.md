---
title: "události riziko aaaAzure Active Directory | Microsoft Docs"
description: "Toto téma nabízí podrobný přehled o jaké jsou rizikových událostech."
services: active-directory
keywords: "ochrany identit Azure active directory, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
author: MarkusVi
manager: femila
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d771c1f43707744aac728c4f72000d855cbd6e1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-risk-events"></a>Azure Active Directory rizikových událostí

velká většina Hello zabezpečení narušení trvat umístit pokud útočník přístup tooan prostředí tak, že krádež identity uživatele. Zjišťování ohrožení zabezpečení identity je bez snadno úlohy. Azure Active Directory používá adaptivní strojového učení algoritmů a heuristiky podezřelé akce toodetect, které jsou související tooyour uživatelské účty. Každý zjistil podezřelé akce je uložený v záznamu názvem *riziko událostí*.

V současné době Azure Active Directory zjistí šesti typy rizikových událostí:

- [Uživatelé s uniklé přihlašovací údaje](#leaked-credentials) 
- [Přihlášení z anonymních IP adres](#sign-ins-from-anonymous-ip-addresses) 
- [Neuskutečnitelná cesta tooatypical umístění](#impossible-travel-to-atypical-locations) 
- [Přihlášení z neznámých míst](#sign-in-from-unfamiliar-locations)
- [Přihlášení z nakažených zařízení](#sign-ins-from-infected-devices) 
- [Přihlášení z IP adres s podezřelou aktivitou](#sign-ins-from-ip-addresses-with-suspicious-activity) 


![Riziko událostí](./media/active-directory-reporting-risk-events/91.png)

Toto téma poskytuje můžete jsou podrobný přehled o událostech, které riziko a jak lze využít tooprotect identitami Azure AD.


## <a name="risk-event-types"></a>Typy rizikových událostí

vlastnost typu události riziko Hello je že identifikátor pro hello podezřelé akce záznam riziko událostí byl vytvořen pro.  
Průběžné investice do společnosti Microsoft do procesu zjišťování hello vést k:

- Vylepšení toohello detekce přesnost existující rizikových událostí 
- Nové typy událostí rizik, které bude přidána v budoucí hello

### <a name="leaked-credentials"></a>Uniklé přihlašovací údaje

Když internetovým zločincům ohrožení platný hesla oprávněných uživatelů, hello podvodníci často sdílet tyto přihlašovací údaje. To se obvykle provádí zveřejněním je veřejně na serverech tmavý webové vložení hello nebo obchodování nebo prodávané hello pověření na černém trhu hello. Hello Microsoft úniku přihlašovacích údajů služby získá uživatelské jméno / heslo páry monitorování veřejné a tmavým webů a práce s:

- Výzkumní pracovníci
- Policie a bezpečnostních složek
- Zabezpečení týmy v Microsoftu
- Další důvěryhodných zdrojů 

Když služba hello zakoupí uživatelské jméno / heslo páry je provedena kontrola proti aktuální platné přihlašovací údaje uživatele AAD. Když je nalezena shoda, znamená to, že byl napaden heslo uživatele a *úniku přihlašovacích údajů riziko událostí* je vytvořena.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Přihlášení z anonymních IP adres

Tento typ události riziko identifikuje uživatele, kteří mají úspěšném přihlášení z IP adresy, která byla zjištěna jako IP adresa anonymního proxy serveru. Tyto servery proxy jsou uživatelé, kteří mají toohide používá IP adresu své zařízení a mohou být použity pro zlými úmysly.


### <a name="impossible-travel-tooatypical-locations"></a>Neuskutečnitelná cesta tooatypical umístění

Tento typ události riziko identifikuje dvě přihlášení pocházející z geograficky vzdáleným umístění, kde alespoň jeden z hello umístění může být také pro hello uživatele netypické, dřívějšímu chování. Kromě toho je kratší než by trvaly hello tootravel uživatele z první toohello umístění hello druhý čas hello hello čas mezi hello dvě přihlášení, indikující, že jiný uživatel používá hello stejné přihlašovací údaje. 

Tato algoritmu strojového učení, který ignoruje zřejmé "*falešně pozitivních*" Přidání toohello neuskutečnitelná cesta podmínky, například sítě VPN a umístění pravidelně používat další uživatelé v organizaci hello.  Hello systém má po počáteční learning dobu 14 dnů, za které se zjišťuje chování přihlášení nového uživatele.

### <a name="sign-in-from-unfamiliar-locations"></a>Přihlášení z neznámých míst

Tento typ události riziko považovat za přihlášení umístění (IP, zeměpisné šířky nebo zeměpisnou délku a ASN) toodetermine nové / neznámého umístění. systém Hello ukládá informace o předchozích umístění používaných uživatelem a zvažuje těchto "známých" umístění. Hello riziko událost se aktivuje při přihlášení hello z umístění, které již nejsou v hello seznam známých umístění. Hello systému má počáteční learning období 30 dní, za které není příznak jiných umístění jako neznámých míst. systém Hello také ignoruje přihlášení ze známé zařízení a umístění, které jsou geograficky zavřete tooa známé umístění. 

### <a name="sign-ins-from-infected-devices"></a>Přihlášení z nakažených zařízení

Tento typ události riziko identifikuje přihlášení ze zařízení nakažená malwarem, které jsou známé tooactively komunikovat se serverem robota. To určuje se pomocí souvztažnosti IP adresy zařízení hello uživatele vůči IP adresy, které byly v kontaktu se serverem robota. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Přihlášení z IP adres s podezřelou aktivitou
Tento typ události riziko identifikuje IP adresy, ze kterých vysoký počet neúspěšných pokusů o přihlášení se zobrazily, několika uživatelským účtům během krátké doby času. To odpovídá vzorem provozu IP adres používaných útočníky a silné ukazatel, které účty jsou buď již nebo o toobe ohrožení zabezpečení. Toto je algoritmu strojového učení, který ignoruje zřejmé "*false pozitivních*", jako jsou například IP adresy, které se pravidelně používají jinými uživateli v organizaci hello.  Hello systém má po počáteční learning dobu 14 dnů, kde se zjišťuje hello přihlášení chování nového uživatele a nového klienta.


## <a name="detection-type"></a>Detekce typu

vlastnost typu detekce Hello je slouží jako ukazatel (v reálném čase nebo Offline) pro časový rámec detekce hello riziko události.  
V současné době většina riziko jsou detekovány události do režimu offline v operaci následného zpracování po události riziko hello.

Hello následující tabulka uvádí hello dobu potřebnou pro detekce typu tooshow v související sestavy:

| Detekce typu | Latence sestav |
| --- | --- |
| V reálném čase | 5 too10 minut |
| V režimu offline | 2 too4 hodin |


Pro hello riziko typy událostí, které zjistí Azure Active Directory hello detekce typy jsou:

| Typ události rizik | Detekce typu |
| :-- | --- | 
| [Uživatelé s uniklé přihlašovací údaje](#leaked-credentials) | V režimu offline |
| [Přihlášení z anonymních IP adres](#sign-ins-from-anonymous-ip-addresses) | V reálném čase |
| [Neuskutečnitelná cesta tooatypical umístění](#impossible-travel-to-atypical-locations) | V režimu offline |
| [Přihlášení z neznámých míst](#sign-in-from-unfamiliar-locations) | V reálném čase |
| [Přihlášení z nakažených zařízení](#sign-ins-from-infected-devices) | V režimu offline |
| [Přihlášení z IP adres s podezřelou aktivitou](#sign-ins-from-ip-addresses-with-suspicious-activity) | V režimu offline|


## <a name="risk-level"></a>Úroveň rizika

Hello riziko úrovně vlastnost události riziko je indikátor (vysoká, střední nebo nízká) pro hello závažnost a spolehlivosti hello riziko události. Tato vlastnost vám pomůže tooprioritize hello akce, které je nutné provést. 

Hello závažnosti události hello riziko představuje hello sílu hello signál jako předpověď identity ohrožení.  
Hello spolehlivosti je pro možnost hello falešně pozitivních slouží jako ukazatel. 

Například: 

* **Vysoká**: vysokou spolehlivostí a riziko událostí vysokou závažností. Tyto události jsou silné indikátory identita hello uživatele došlo k ohrožení, a všechny uživatelské účty dopad by měl být opraven okamžitě.

* **Střední**: vysokou závažností, ale nižší událostí spolehlivosti riziko, a naopak. Tyto události jsou potenciálně rizikové a všechny uživatelské účty dopad by bylo možno opravit.

* **Nízká**: nízkou spolehlivosti a nízkou závažností riziko událostí. Tato událost nevyžadují okamžitý zásah, ale poskytnout při kombinaci s jinými událostmi riziko, že dojde k ohrožení bezpečnosti silné indikace toho, zda text hello identity.

![Úroveň rizika](./media/active-directory-reporting-risk-events/01.png)

### <a name="leaked-credentials"></a>Uniklé přihlašovací údaje

Úniku rizikových událostech, které jsou klasifikovány jako přihlašovací údaje **vysokou**, protože poskytují jasný náznak, že hello uživatelské jméno a heslo jsou k dispozici tooan útočníka.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Přihlášení z anonymních IP adres

Hello úroveň rizika pro tento typ události riziko je **střední** protože anonymní IP adresa není silné údaj o ohrožení účtu.  
Doporučujeme, pokud používají anonymních IP adres okamžitě kontaktujte uživatele tooverify hello.


### <a name="impossible-travel-tooatypical-locations"></a>Neuskutečnitelná cesta tooatypical umístění

Neuskutečnitelná cesta je obvykle dobrou indikátoru, že se hacker bylo možné toosuccessfully přihlásit. False pozitivních však může dojít, když se uživatel při cestě pomocí nového zařízení nebo pomocí sítě VPN, který se obvykle nepoužívá jinými uživateli v organizaci hello. Jiný zdroj pozitivních false je aplikace, které nesprávně jako klient IP adresy, která by mohla hello vzhled předat serveru IP adresy z přihlášení je hostována dochází z hello datové centrum, kde aplikace je back-end (často jsou to datových centrech společnosti Microsoft, dávají hello vzhled sign-in probíhající od Microsoftu ve vlastnictví IP adresy). V důsledku těchto false pozitivních hello úroveň rizika pro tuto událost riziko je **střední**.

> [!TIP]
> Můžete snížit množství hello hlášené positves false pro tento typ události riziko nakonfigurováním [s názvem umístění](active-directory-named-locations.md). 

### <a name="sign-in-from-unfamiliar-locations"></a>Přihlášení z neznámých míst

Neznámých míst nabízejí silné označení, že útočník je možné toouse odcizené identity. False pozitivních může dojít, když uživatel při cestě, je vyzkoušení nové zařízení nebo pomocí nové připojení VPN. V důsledku těchto falešně pozitivních zjištění hello úroveň rizika pro tento typ události je **střední**.

### <a name="sign-ins-from-infected-devices"></a>Přihlášení z nakažených zařízení

Tato událost riziko identifikuje IP adresy, nikoli zařízením uživatele. Pokud několika zařízeními jsou za jednu IP adresu a jenom některé jsou řízené robota síť, přihlášení z jiných zařízení Moje aktivační událost Tato událost zbytečně, což je hello důvod pro klasifikaci této události riziko jako **nízká**.  

Doporučujeme kontaktovat uživatele hello a kontrolovat všechny hello uživatelské zařízení. Je také možné, že je nakažená osobní zařízení uživatele, nebo jak už bylo zmíněno dříve, aby používal někdo zařízení s nakažené v hello stejné IP adres jako uživatel hello. Nakažených zařízení jsou často napadené malwarem, který dosud nebyly určeny antivirový software a může taky znamenat jako chybných uživatelských návyky, které mohly způsobit toobecome zařízení hello nakažená.

Další informace o tom, tooaddress napadení malwarem, najdete v části hello [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Přihlášení z IP adres s podezřelou aktivitou

Doporučujeme kontaktovat uživatele tooverify hello, pokud se ve skutečnosti přihlášení z IP adresy, která byla označena jako podezřelá. je Hello úroveň rizika pro tento typ události "**střední**" vzhledem k tomu může být několik zařízení za hello stejnou IP adresu, zatímco jenom některé může být zodpovědná za hello podezřelou aktivitu. 


 
## <a name="next-steps"></a>Další kroky

Riziko události jsou hello foundation ochrany identit Azure AD. Azure AD můžete zjistit aktuálně šesti rizikových událostí: 


| Typ události rizik | Úroveň rizika | Detekce typu |
| :-- | --- | --- |
| [Uživatelé s uniklé přihlašovací údaje](#leaked-credentials) | Vysoký | V režimu offline |
| [Přihlášení z anonymních IP adres](#sign-ins-from-anonymous-ip-addresses) | Střednědobé používání | V reálném čase |
| [Neuskutečnitelná cesta tooatypical umístění](#impossible-travel-to-atypical-locations) | Střednědobé používání | V režimu offline |
| [Přihlášení z neznámých míst](#sign-in-from-unfamiliar-locations) | Střednědobé používání | V reálném čase |
| [Přihlášení z nakažených zařízení](#sign-ins-from-infected-devices) | Nízký | V režimu offline |
| [Přihlášení z IP adres s podezřelou aktivitou](#sign-ins-from-ip-addresses-with-suspicious-activity) | Střednědobé používání | V režimu offline|

Kde lze najít hello rizikových událostech, které byly nalezeny ve vašem prostředí?
Existují dvě místa, kde můžete zkontrolovat hlášené rizikových událostí:

 - **Generování sestav služby Azure AD** -rizikových událostí jsou součástí zabezpečení Azure AD sestavy. Další podrobnosti najdete v tématu hello [uživatele na riziko zabezpečení sestavy](active-directory-reporting-security-user-at-risk.md) a hello [sestavy zabezpečení rizikové přihlášení](active-directory-reporting-security-risky-sign-ins.md).

 - **Azure AD Identity Protection** -rizikových událostí je také součástí [Azure Active Directory Identity Protection](active-directory-identityprotection.md) funkce vytváření sestav.
    

Při hello zjišťování rizikových událostí již představuje důležitou součást ochraně vaší identity, máte také možnost tooeither hello ručně adresa je nebo i implementovat automatické odpovědi konfigurací zásady podmíněného přístupu. Další podrobnosti najdete v tématu o [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
 
