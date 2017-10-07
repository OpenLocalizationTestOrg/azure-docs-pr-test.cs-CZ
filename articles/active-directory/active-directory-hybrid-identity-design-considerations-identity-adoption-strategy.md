---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - Definovat strategii přijetí hybridní identity | Microsoft Docs"
description: "Pomocí podmíněného řízení přístupu Azure Active Directory kontroluje hello konkrétní podmínky, kterou vyberete při ověřování uživatele hello a před povolením přístupu toohello aplikace. Po splnění těchto podmínek hello uživatel ověří a povolená přístup toohello aplikace."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: b92fa5a9-c04c-4692-b495-ff64d023792c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 9ffca675d0c714392adfcbbc4dcfad12fccbac78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="define-a-hybrid-identity-adoption-strategy"></a>Definování strategie přijetí hybridní identity
V této úloze definujete hello hybridní identity přijetí strategie pro vaše hybridní identity řešení toomeet hello obchodní požadavky, které byly popsané v:

* [Určení obchodních potřeb](active-directory-hybrid-identity-design-considerations-business-needs.md)
* [Určení požadavků na synchronizaci adresáře](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
* [Stanovení požadavků na službu Multi-Factor authentication](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Definování strategie obchodních potřeb
první úlohou Hello řeší určení hello organizace obchodním potřebám.  To může být velmi široký a oboru nárůstu může dojít, pokud si nejste opatrní.  V hello začátku jednoduchost ale vždycky mějte na paměti, tooplan návrh, který bude přizpůsobení a usnadnění změn v budoucnu hello.  Bez ohledu na to, zda je jednoduchý návrhu nebo některého velmi složité Azure Active Directory je platforma Microsoft Identity hello, která podporuje Office 365, Microsoft Online Services a aplikace využívající cloudu.

## <a name="define-an-integration-strategy"></a>Definování strategie integraci
Microsoft má tři scénáře hlavní integrace, které jsou cloudové identity, synchronizované identity a federované identity.  Měli byste naplánovat na jednu z těchto integračních strategie přijetí.  Hello zvolíte, se může lišit a může zahrnovat hello rozhodnutí v zvolili jeden, jaký typ strategií činnost koncového uživatele, kterou chcete tooprovide, máte některé stávající infrastrukturu hello již na místě a co je nákladově nejefektivnější hello.  

![](./media/hybrid-id-design-considerations/integration-scenarios.png)

scénáře Hello definované v hello výše obrázku jsou:

* **Cloudové identity**: Jedná se o identity, které existují výhradně v cloudu hello.  V případě hello Azure AD by nacházet konkrétně v adresáři služby Azure AD.
* **Synchronizovat**: Jedná se o identity, které existují místně a v cloudu hello.  Pomocí služby Azure AD Connect, tito uživatelé vytvoření nebo propojit s existující účty Azure AD.  Hello hodnoty hash hesla uživatele se synchronizují z hello místní prostředí toohello cloudu v co se nazývá hodnoty hash hesla.  Při použití synchronizaci hello jeden přímý přístup paměti je, pokud uživatel je zakázána v hello místní prostředí, může to trvat až too3 hodin pro tento účet stav tooshow ve službě Azure AD.  Toto je z důvodu toohello synchronizace časový interval.
* **Federované**: tyto identity existují i místně a v cloudu hello.  Pomocí služby Azure AD Connect, tito uživatelé vytvoření nebo propojit s existující účty Azure AD.  

> [!NOTE]
> Další informace o možnostech synchronizace hello [integrace místních identit s Azure Active Directory](connect/active-directory-aadconnect.md).
> 
> 

Hello následující tabulka vám pomůže při určování hello výhody a nevýhody jednotlivých hello následujících strategií:

| Strategie | Výhody | Nevýhody |
| --- | --- | --- |
| **Cloudové identity** |Jednodušší toomanage pro menší organizace. <br> Nic potřeba další hardware tooinstall ne na místní<br>Snadno zakázat, pokud uživatel hello odejde hello společnosti |Uživatelé budou potřebovat toosign v při přístupu k úlohy v cloudu hello <br> Hesla mohou nebo nemusí být stejné pro cloudové a místní identity hello |
| **Synchronizovat** |Místní heslo bude ověřovat místní a cloudové adresáře <br>Jednodušší toomanage pro malé, střední a velké organizace <br>Uživatelé mohou mít jednotné přihlašování (SSO) pro některé zdroje <br> Metoda Microsoft upřednostňovaný pro synchronizaci <br> Jednodušší toomanage |Někteří zákazníci mohou být zdráhat toosynchronize jejich adresáře s hello cloudu z důvodu konkrétní společnosti dá |
| **Federované** |Uživatelé mohou mít jednotné přihlašování (SSO) <br>Pokud uživatel je ukončen nebo opustí, hello účet můžete okamžitě zakázáno a odvolat přístup,<br> Podporuje rozšířené scénáře, které nelze provést pomocí synchronizovat |Další kroky toosetup a konfigurace <br> Vyšší údržby <br> Může vyžadovat další hardware pro infrastrukturu služby tokenů zabezpečení hello <br> Může vyžadovat další hardware tooinstall hello federační server. Další software je vyžadován, pokud se používá služba AD FS <br> Vyžadovat rozsáhlé instalační program pro jednotné přihlašování <br> Kritický bod selhání, pokud je federační server hello dolů, uživatelé nebudou mít tooauthenticate |

### <a name="client-experience"></a>Možnosti klienta
Hello strategie, kterou použijete, bude určovat hello přihlášení činnost koncového uživatele.  Hello následující tabulky poskytují s informacemi o jaké hello uživatelé měli očekávají, že jejich přihlášení dojde toobe.  Všimněte si, že ne všechny federovaných identit zprostředkovatelé podporují jednotné přihlašování ve všech scénářích.

**Domain připojený a privátní síťových aplikací**:

|  | Synchronizované Identity | Federované Identity |
| --- | --- | --- |
| Webové prohlížeče |Ověřování pomocí formulářů |jednotné přihlašování na, někdy vyžaduje toosupply ID organizace |
| Outlook |Výzva k zadání přihlašovacích údajů |Výzva k zadání přihlašovacích údajů |
| Skype pro firmy (Lync) |Výzva k zadání přihlašovacích údajů |jednotného přihlašování pro aplikace Lync, výzva přihlašovací údaje pro Exchange |
| SkyDrive Pro |Výzva k zadání přihlašovacích údajů |jednotné přihlašování |
| Office Pro Plus předplatného |Výzva k zadání přihlašovacích údajů |jednotné přihlašování |

**Externí nebo nedůvěryhodných zdrojů**:

|  | Synchronizované Identity | Federované Identity |
| --- | --- | --- |
| Webové prohlížeče |Ověřování pomocí formulářů |Ověřování pomocí formulářů |
| Outlook, Skype pro firmy (Lync) Skydrive Pro předplatného systému Office |Výzva k zadání přihlašovacích údajů |Výzva k zadání přihlašovacích údajů |
| Exchange ActiveSync |Výzva k zadání přihlašovacích údajů |jednotného přihlašování pro aplikace Lync, výzva přihlašovací údaje pro Exchange |
| Mobilní aplikace |Výzva k zadání přihlašovacích údajů |Výzva k zadání přihlašovacích údajů |

Pokud zjistíte z úlohy 1, abyste měli 3. stran IdP nebo jsou toouse má jeden federační tooprovide s Azure AD, je třeba vědět hello následující toobe podporované funkce:

* Všechny poskytovatele SAML 2.0, který je kompatibilní s pro hello SP Lite profilu může podporovat tooAzure ověřování AD a přidružené aplikace
* Podporuje pasivní ověřování, což usnadňuje auth tooOWA, SPO, atd.
* Exchange Online klientů může být podporován pomocí hello SAML 2.0 rozšířeného klienta profilu (ECP)

Musí být také vědět, jaké funkce nebudete mít k dispozici:

* Bez podpory důvěryhodnosti/WS-Federation dojde k přerušení všech aktivních klientů
  * To znamená bez klienta, klient OneDrive, předplatného systému Office, Office Mobile předchozí tooOffice 2016
* Přechod ověřování toopassive Office mu umožní toosupport čistý IdPs SAML 2.0, ale podpora budou mít pořád na základě klienta klienta

> [!NOTE]
> Hello nejaktuálnější seznamu najdete v tématu http://aka.ms/ssoproviders článku hello.
> 
> 

## <a name="define-synchronization-strategy"></a>Definování strategie pro synchronizaci
V této úloze určíte hello nástroje, které budou použité toosynchronize hello organizace místní data toohello cloudu a co byste měli používat topologie.  Protože většina organizací použít služby Active Directory, informace o používání Azure AD Connect tooaddress hello otázky výše je součástí některých podrobností.  Pro prostředí, které nemají služby Active Directory jsou informace o použití FIM 2010 R2 nebo MIM 2016 toohelp plánování této strategie.  Ale budoucích verzích služby Azure AD Connect bude podporovat adresáře LDAP, tak v závislosti na časové ose, tyto informace nemusí být možné tooassist.

### <a name="synchronization-tools"></a>Nástroje pro synchronizaci
V průběhu let hello mít několik nástrojů pro synchronizaci existoval a použít pro různé scénáře.  Azure AD Connect je aktuálně hello přejděte tootool volbou pro všechny podporované scénáře.  AAD Sync a DirSync jsou pořád kolem a může i nacházet ve vašem prostředí teď. 

> [!NOTE]
> Hello nejnovější informace týkající se možnosti hello podporované každý nástroje, najdete v tématu [porovnání nástrojů integrace adresáře](active-directory-hybrid-identity-design-considerations-tools-comparison.md) článku.  
> 
> 

### <a name="supported-topologies"></a>Podporované topologie
Při definování strategie synchronizace, musíte určit hello topologie, která se používá. V závislosti na hello je informace, které byla určena v kroku 2, můžete určit, kterou topologii správné jeden toouse hello. Hello Jedna doménová struktura, jeden topologie Azure AD hello nejběžnější a skládá se z jedné doménové struktury služby Active Directory a jednu instanci Azure AD.  To se bude používat ve většině scénářů hello toobe a je topologie hello očekává při použití instalaci Azure AD Connect Express, jak je znázorněno na následujícím obrázku hello.

![](./media/hybrid-id-design-considerations/single-forest.png)Jedné doménové struktury scénář je velmi běžné toohave i malé i velké organizace více doménových strukturách, jak je znázorněno na obrázku 5.

> [!NOTE]
> Další informace o hello různých místních a topologie služby Azure AD s synchronizace Azure AD Connect, přečtěte si článek hello [topologie pro Azure AD Connect](connect/active-directory-aadconnect-topologies.md).
> 
> 

![](./media/hybrid-id-design-considerations/multi-forest.png) 

Scénář s více doménovými strukturami

Pokud tento případ hello pak hello více forest jedním topologie Azure AD považovat za Pokud hello splnění následujících podmínek:

* Uživatelé mají pouze 1 identity v rámci všech doménových struktur – to hello Jednoznačná identifikace uživatelů části popisuje podrobněji.
* ověření uživatele Hello toohello doménové struktury, ve kterém se nachází svou identitu
* Hlavní název uživatele a zdrojové ukotvení (neměnné id) bude pocházet z této doménové struktury
* Všech doménových strukturách jsou přístupné přes Azure AD Connect – to znamená, se nemusí toobe připojený k doméně a mohou být umístěny v hraniční síti, pokud to usnadňuje to.
* Uživatelé mají pouze jedna poštovní schránka
* Hello doménové struktury, který hostuje poštovní schránky uživatele má hello nejlepší kvalitu dat pro atributy, které jsou viditelné v hello Exchange globální seznam adres
* Pokud není žádná poštovní schránky na hello uživatele, pak všechny doménové struktury mohou být tyto hodnoty používané toocontribute
* Pokud máte propojená poštovní schránka, pak v jiné doménové struktuře použít toosign v je také jiný účet.

> [!NOTE]
> Objekty, které existují v jak místně a v cloudu hello "připojení" prostřednictvím jedinečný identifikátor. V kontextu hello synchronizace adresářů je tento jedinečný identifikátor odkazované tooas hello SourceAnchor. V kontextu hello z jednotné přihlašování je to odkazované tooas hello ImmutableId. [Koncepty návrhu Azure AD Connect](connect/active-directory-aadconnect-design-concepts.md#sourceanchor) další důležité informace týkající se použití hello SourceAnchor.
> 
> 

Pokud nejsou splněné hello výše a máte více než jeden aktivní účet nebo více než jedna poštovní schránka, Azure AD Connect bude vyberte jeden a ignorovat hello jiné.  Pokud jste propojili poštovních schránek, ale žádný jiný účet, tyto účty nebude exportovaný tooAzure AD a tento uživatel nebude členem žádné skupiny.  To se liší od jak tomu bylo v hello po s nástrojem DirSync a je záměrné toobetter podporu těchto scénářů s více doménovými strukturami. Scénář více doménovými strukturami je uvedený na následujícím obrázku hello.

![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 

**Více doménovými strukturami více scénáři se službou Azure AD**

Doporučuje se, že toohave, kterou právě jeden adresář ve službě Azure AD pro organizaci, ale je podporována, pokud je udržováno relace 1:1 mezi server synchronizace služby Azure AD Connect a adresář Azure AD.  Pro každou instanci Azure AD budete potřebovat instalace služby Azure AD Connect.  Navíc Azure AD, návrh je izolované a uživatelé jednu instanci Azure AD, nebudou uživatelé moct toosee v jiné instanci.

Je možné, a podporované tooconnect jeden místní instanci služby Active Directory toomultiple Azure AD adresáře jak je znázorněno na následujícím obrázku hello:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 

**Filtrování scénář jednou doménovou strukturou**

V pořadí toodo tento hello jsou splněny následující podmínky:

* Azure AD Connect synchronizačních serverů musí nakonfigurovat pro filtrování, každá má vzájemně se vylučuje sadu objektů.  To provést, například rozsahu každý server tooa konkrétní doménu nebo organizační jednotku.
* Doménu DNS se dají registrovat jenom v jedné adresář Azure AD, hello UPN uživatelů hello v hello místní AD musíte použít samostatné obory názvů
* Uživatelé v jednu instanci Azure AD bude pouze uživatelé moct toosee z jejich instance.  Ty není být schopný toosee uživatele v hello ostatní instance
* Pouze jeden z adresáře hello Azure AD můžete povolit Exchange hybridní s hello místní AD
* Vzájemné výhradní právo platí také toowrite zpět.  Díky tomu některé funkce zpětný zápis, není podporován s touto topologií, protože se tyto předpokládá jednomu místnímu konfigurace.  To zahrnuje následující:
  * Skupiny s výchozí konfigurací zpětný zápis
  * Zpětný zápis zařízení

Uvědomte si, že následující hello není podporována a nesmí být vybrána jako implementace:

* Není podporované toohave víc synchronizačních serverů Azure AD Connect připojení adresáře toohello stejnou službou Azure AD, i když jsou nakonfigurované toosynchronize vzájemně se vylučuje nastavit objektu
* Není podporován toosync hello stejných adresářích toomultiple Azure AD uživatele. 
* Je také nepodporované toomake konfiguraci změnit toomake uživatele v jednom tooappear Azure AD jako kontaktuje z jiného adresáře služby Azure AD. 
* Je také nepodporované toomodify Azure AD Connect sync tooconnect toomultiple Azure AD adresáře.
* Adresáře Azure AD jsou záměrné izolované. Toto je nepodporovaná toochange hello konfigurace Azure AD Connect sync tooread dat z jiného adresáře služby Azure AD v pokusu o toobuild běžné a jednotná GAL mezi hello adresáře. Je také uživatelé nepodporované tooexport tak kontaktuje tooanother místní AD pomocí Azure AD Connect sync.

> [!NOTE]
> Pokud vaše organizace brání počítači v síti toohello internetové připojení, v tomto článku jsou uvedené koncové body hello (plně kvalifikovaný název domény, IPv4 a IPv6 rozsahy adres), by měla obsahovat vaše odchozí povolit seznamy a zóně důvěryhodných lokalit aplikace Internet Explorer z klientské počítače tooensure vaše počítače můžou úspěšně pomocí Office 365. Další informace najdete v tématu [Office 365 adresy URL a rozsahy IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
> 
> 

## <a name="define-multi-factor-authentication-strategy"></a>Definování strategie pro službu Multi-Factor authentication
V této úloze určíte toouse strategie hello služby Multi-Factor authentication.  Azure Multi-Factor Authentication se dodává v dvě různé verze.  Jedna je cloudu a hello jiné místní pomocí hello Azure MFA serveru.  Podle hello hodnocení, které výše můžete můžete určit řešení, které je dobrý den správné jednu pro strategie.  Použití hello tabulce toodetermine, který návrh možnost nejlépe splnění požadavků zabezpečení vaší společnosti:

Možnosti služby Multi-Factor návrhu:

| Asset toosecure | MFA v cloudu hello | Místní MFA |
| --- | --- | --- |
| Aplikace Microsoft |Ano |Ano |
| Aplikace SaaS v galerii aplikací hello |Ano |Ano |
| Aplikace služby IIS publikované prostřednictvím proxy aplikace Azure AD |Ano |Ano |
| Aplikace služby IIS nepublikované prostřednictvím hello Proxy aplikace Azure AD |Ne |Ano |
| Vzdálený přístup jako síť VPN, RDG |Ne |Ano |

I když pro strategie, může mít vyrovnané na řešení, musíte stále toouse hello vyhodnocení z výše toho, kde se nachází vaši uživatelé.  To může způsobit toochange řešení hello.  Použijte následující tooassist hello tabulce můžete určování tohoto:

| Umístění uživatele | Upřednostňovanou možnost |
| --- | --- |
| Azure Active Directory |Více-FactorAuthentication v cloudu hello |
| Azure AD a místní AD využívající federaci se službou AD FS |Obě |
| Azure AD a místní AD pomocí služby Azure AD Connect bez synchronizace hesla |Obě |
| Azure AD a místní pomocí služby Azure AD Connect se synchronizací hesla |Obě |
| Místní AD |Server Multi-Factor Authentication |

> [!NOTE]
> Také se ujistěte, že podporuje možnost designu hello vícefaktorového ověřování, která jste vybrali hello funkce, které jsou požadovány pro váš návrh.  Další informace najdete v tématu [zvolte hello Multi-Factor Authentication řešení pro vás](../multi-factor-authentication/multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).
> 
> 

## <a name="multi-factor-auth-provider"></a>Zprostředkovatel vícefaktorového ověřování
Služba Multi-Factor authentication je k dispozici ve výchozím nastavení pro globální správce, kteří mají klienta Azure Active Directory. Ale pokud chcete tooextend tooall vícefaktorového ověřování uživatelů nebo má tooyour globální správci toobe možné tootake výhod funkce třeba hello portálu pro správu, vlastní přivítání a sestavy, pak je nutné zakoupit a nakonfigurovat Poskytovatele služby Multi-Factor Authentication.

> [!NOTE]
> Také se ujistěte, že podporuje možnost designu hello vícefaktorového ověřování, která jste vybrali hello funkce, které jsou požadovány pro váš návrh. 
> 
> 

## <a name="next-steps"></a>Další kroky
[Určení požadavků na ochranu dat](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

