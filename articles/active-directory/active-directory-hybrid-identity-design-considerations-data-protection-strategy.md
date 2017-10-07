---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - definování strategie pro ochranu dat | Microsoft Docs"
description: "Strategie ochrany dat hello definujete pro vaše hybridní identity řešení toomeet hello obchodní požadavky, které jste definovali."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: e76fd1f4-340a-492a-84d9-e05f3b7cc396
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 8fd7ab364a09de3b60293a4a1cbb6e0fa4a3295d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Definování strategie ochrany dat pro vaše řešení hybridní identity
V této úloze určíte hello strategie ochrany dat pro hybridní identity řešení toomeet hello podnikové požadavky definované v:

* [Určení požadavků na ochranu dat](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
* [Stanovení požadavků na správu obsahu](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
* [Určete požadavky řízení přístupu](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
* [Stanovení požadavků na reakce na incidenty](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Zadejte možnosti ochrany dat
Jak je popsáno v [určení požadavků na synchronizaci adresáře](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), umístěným na místních Microsoft Azure AD můžete synchronizovat s vaší Active Directory Domain Services (AD DS). Tato integrace umožňuje organizacím tooleverage Azure AD tooverify přihlašovacích údajů uživatele, když se pokoušíte tooaccess podnikovým prostředkům. To lze provést pro oba scénáře: data na rest místně a v cloudu hello.  Toodata přístupu ve službě Azure AD vyžaduje ověření uživatele pomocí služby tokenů zabezpečení (STS).

Po ověření hello hlavní název uživatele (UPN) je pro čtení z hello ověřovací token a hello replikovaný oddílu a odpovídající kontejneru je určen toohello uživatele domény. Informace o existence hello uživatele, povoleném stavu a role je používán hello autorizace systému toodetermine, zda hello požadovaný přístup toohello cíl klient není autorizován pro tohoto uživatele v této relaci. Některé akce oprávnění (konkrétně vytvořit uživateli, resetování hesla) vytvořit záznam pro audit, který mohou využívat klienta úsilí o dodržování předpisů toomanage správce nebo šetření.

Přesunutí dat z vašeho místního datového centra do úložiště Azure přes připojení k Internetu nemusí být vždy proveditelné kvůli toodata svazku, dostupnosti šířky pásma nebo další důležité informace. Hello [službu Import/Export úložiště Azure](../storage/common/storage-import-export-service.md) nabízí možnost založené na hardwaru pro umístění nebo načítání velkých objemů dat v úložišti objektů blob. Umožňuje vám toosend [šifrované nástrojem BitLocker](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) přímo tooan datové centrum Azure kde operátorům cloudu odešlete účet úložiště tooyour hello obsah, nebo si můžete stáhnout tooyour vaší služby Azure data jednotky tooreturn jednotky pevného disku tooyou. Pro tento proces (pomocí klíčem nástroje BitLocker vygenerované službou hello samotné během instalace úlohy hello), jsou přijaty pouze šifrovaná disky. Hello BitLocker klíč je k dispozici tooAzure samostatně, a zajišťuje tak mimo klíče sdílení vzdálené správy.

Vzhledem k tomu, že přenášených dat můžete provést v různých scénářích, je také důležité tooknow, který používá Microsoft Azure [virtuálních sítí](https://azure.microsoft.com/documentation/services/virtual-network/) tooisolate klienty provoz od sebe navzájem zaměstnávající míry například hostitele a hostů úroveň brány firewall, filtrování paketů IP, portu blokování a koncové body HTTPS. Většina interních komunikací Azure, včetně infrastruktury infrastruktury a infrastruktury zákazníka (místní), ale jsou šifrované. Další důležitou možností je hello komunikace v rámci datových centrech Azure; Microsoft spravuje tooassure sítě, aby žádný virtuální počítač, zosobnit nebo tajně poslouchat hello IP adresu jiného. Protokol TLS/SSL se používá při přístupu k Azure Storage nebo databází SQL, nebo když připojení tooCloud služby. V takovém případě hello zákazníka správce je zodpovědný za získání certifikátu TLS/SSL a jeho nasazení infrastruktury tootheir klienta. Přesouvání mezi virtuálními počítači přenos dat v jednom nasazení hello nebo mezi klienty v jednom nasazení prostřednictvím Microsoft Azure Virtual Network se dají chránit pomocí šifrovanou komunikaci protokoly, jako je například HTTPS, SSL/TLS nebo jiné.

V závislosti na tom, jak jste odpověděli na otázky hello v [určení požadavků na ochranu dat](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md), jak chcete tooprotect vaše data a jak hello hybridní řešení identit vám pomůže na které byste měli být schopný toodetermine. Hello tabulka ukazuje hello možnosti, které jsou dostupné pro jednotlivé scénáře ochrany dat nepodporuje v Azure.

| Možnosti ochrany dat | V klidovém stavu uložených v cloudu hello | Na rest na místě | Při přenosu |
| --- | --- | --- | --- |
| Nástroj BitLocker Drive Encryption |X |X | |
| Tooencrypt databáze systému SQL Server |X |X | |
| Šifrování virtuálních počítačů VM | | |X |
| SSL/TLS. | | |X |
| Síť VPN | | |X |

> [!NOTE]
> Čtení [dodržování předpisů podle funkce](https://azure.microsoft.com/support/trust-center/services/) v [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) tooknow více informací o hello certifikátů, které je kompatibilní s každou službu Azure.
> Vzhledem k tomu použít s vícevrstvých hello možnosti ochrany dat, se nedají použít pro tuto úlohu porovnání mezi těmito možnostmi. Ujistěte se, že se využívá pro každý stav, který hello data budou k dispozici všechny možnosti.
>
>

## <a name="define-content-management-options"></a>Zadejte možnosti správy obsahu
Jednou z výhod použití služby Azure AD toomanage hybridní infrastrukturu identit je z hlediska hello koncového uživatele zcela transparentní hello procesu. Hello uživatel se pokusí tooaccess sdíleného prostředku, hello prostředek vyžaduje ověření, hello uživatel bude mít toosend k ověření požadavku tooAzure AD v pořadí tooobtain hello token a přístup k prostředku hello. Tento celý proces probíhá pozadí bez zásahu uživatele. Je také možné toogrant oprávnění tooa [skupiny](active-directory-manage-groups.md#getting-started-with-access-management) uživatelů v pořadí tooallow je tooperform některé běžné akce.

Organizace, které jsou starat o ochrany osobních údajů obvykle vyžadují klasifikace dat pro své řešení. Pokud své aktuální místní infrastrukturu se už používá klasifikaci dat, je možné tooleverage Azure AD jako hello hlavní úložiště pro identitu uživatele. Nástroj běžné, že je použité místní pro klasifikaci dat se nazývá [sada nástrojů klasifikace dat](https://msdn.microsoft.com/library/Hh204743.aspx) pro Windows Server 2012 R2. Tento nástroj může pomoci tooidentify, klasifikovat a chránit data na souborových serverech v privátním cloudu. Je také možné tooleverage hello [automatickou klasifikaci souborů](https://technet.microsoft.com/library/hh831672.aspx) v systému Windows Server 2012 tooaccomplish to.

Pokud vaše organizace nemá klasifikace dat na místě, ale potřebuje tooprotect citlivé soubory bez přidání nové servery místně, můžou použít Microsoft [Azure Rights Management Service](https://technet.microsoft.com/library/JJ585026.aspx).  Azure RMS používá šifrování, identity a autorizace toohelp zásady zabezpečení, soubory a e-mailu a funguje napříč více zařízeními – telefony, tablety a počítače. Protože je Cloudová služba Azure RMS, není nutné tooexplicitly konfigurovat vztahy důvěryhodnosti s dalšími organizacemi, než se chráněný obsah můžete sdílet s nimi. Pokud už mají služby Office 365 nebo adresář služby Azure AD, podporuje se automaticky spolupráce mezi organizacemi. Můžete také synchronizovat jenom hello atributy adresáře, Azure RMS potřebuje toosupport společnou identitu pro vaše místní účty služby Active Directory, a to pomocí Azure Active Directory synchronizační služby (AAD Sync) nebo Azure AD Connect.

Toounderstand, kdo přistupuje k prostředku, který je sice podstatná součást správy obsahu, proto je důležité pro řešení správy identit hello možnosti bohaté protokolování. Azure AD poskytuje protokolu více než 30 dní, včetně:

* Změny v členství role (např: Přidání tooGlobal správce role uživatele)
* Aktualizace na přihlašovací údaje (např: změny hesla)
* Správa domény (např: ověření vlastní domény, odebrání domény)
* Přidání nebo odebrání aplikací
* Správa uživatelů (např: Přidání, odebrání, aktualizaci uživatele)
* Přidání nebo odebrání licencí

> [!NOTE]
> Čtení [zabezpečení Microsoft Azure a správa protokolu auditu](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) tooknow Další informace o možnosti protokolování v Azure.
> V závislosti na tom, jak jste odpověděli na otázky hello v [stanovení požadavků na správu obsahu](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md), byste měli mít toodetermine, jakým způsobem chcete hello obsahu toobe spravované v hybridní řešení identit. Všechny možnosti, které jsou zveřejněné v tabulce 6 jsou sice může integrovat s Azure AD, je důležité toodefine, což je vhodnější pro konkrétní obchodní potřeby.
>
>

| Možnosti správy obsahu | Výhody | Nevýhody |
| --- | --- | --- |
| Centralizovaný místně (server služby Active Directory Rights Management Server) |Plnou kontrolu nad infrastrukturou server hello zodpovědná za klasifikace dat hello <br> Integrované funkce v systému Windows Server, není nutné pro další licence nebo předplatné <br> Umožňuje integraci s Azure AD v hybridním scénáři <br> Podporuje informace rights management (IRM) funkce v služeb Microsoft Online services, jako je Exchange Online a SharePoint Online, jakož i Office 365 <br> Podporuje místní serverové produkty společnosti Microsoft, například Exchange Server, SharePoint Server a souborové servery se systémem Windows Server a infrastruktury klasifikace souborů (FCI). |Vyšší, údržby (zachovat až s aktualizacemi, konfigurace a potenciální upgrady), protože IT vlastní hello serveru <br> Vyžaduje serverovou infrastrukturu na místě<br> Doesn'tleverage možnosti Azure nativně |
| Centralizovaný v cloudu hello (Azure RMS) |Jednodušší toohello toomanage porovnání místní řešení <br> Umožňuje integraci se služba AD DS v hybridním scénáři <br>  Plně integrované se službou Azure AD <br> Nevyžaduje server místně v pořadí toodeploy hello služby <br> Podporuje místní serverové produkty společnosti Microsoft, například Exchange Server, SharePoint, Server a souborové servery se systémem Windows Server a souboru klasifikace infrastruktury (FCI) <br> Oddělení IT, může mít úplnou kontrolu nad klíčem příslušného tenanta na funkci BYOK. |Vaše organizace musí mít cloudové předplatné podporující službu RMS <br> Vaše organizace musí mít ověření uživatele toosupport adresář Azure AD RMS |
| Hybridní (Azure RMS integrovat, místní Active Directory Rights Management Server) |Tento scénář shromažďuje hello výhody obou, centralizovaný místně a v cloudu hello. |Vaše organizace musí mít cloudové předplatné podporující službu RMS <br> Vaše organizace musí mít ověření uživatele toosupport adresář Azure AD RMS, <br> Vyžaduje připojení mezi cloudové služby Azure a místní infrastruktury |

## <a name="define-access-control-options"></a>Zadejte možnosti řízení přístupu
S využitím hello ověřování, autorizace a řízení přístupu možnosti dostupné v Azure AD, nebudete moct tooenable vaší společnosti toouse úložiště centrální identit při povolení uživatelů a partnery toouse jednotné přihlašování (SSO), jak je znázorněno v hello Obrázek níže:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Centralizovaná správa a plně integraci s další adresáře

Poskytuje jeden toothousands přihlašování aplikací SaaS Azure Active Directory a místní webové aplikace. Přečtěte si prosím hello [seznam kompatibility federace Azure Active Directory: poskytovatelů identit třetích stran, které se dají použít tooimplement jednotného přihlašování](https://msdn.microsoft.com/library/azure/jj679342.aspx) článku Další podrobnosti o hello jednotné přihlašování třetích stran, které byly testovány podle Společnosti Microsoft. Tato funkce umožňuje organizaci tooimplement celou řadu scénářů B2B zachováním kontrolu nad hello správu identit a přístupu. Během hello B2B navrhování procesu je metoda ověřování hello důležité toounderstand, která se použije hello partnera a ověřit, jestli je tato metoda nepodporuje v Azure. Aktuálně jsou tyto metody podporovaný službou Azure AD:

* Security Assertion Markup Language (SAML)
* OAuth
* Pomocí protokolu Kerberos
* Tokeny
* Certifikáty

> [!NOTE]
> Přečtěte si [Azure Active Directory ověřovací protokoly](https://msdn.microsoft.com/library/azure/dn151124.aspx) tooknow více podrobností o jednotlivých protokolů a jeho funkce v Azure.
>
>

Prostřednictvím podpory hello Azure AD, mobilní obchodní aplikace, můžou používat hello stejné snadno Mobile Services ověřování prostředí tooallow zaměstnanci toosign do svých mobilních aplikací s svoje podnikové přihlašovací údaje služby Active Directory. Pomocí této funkce se podporuje Azure AD jako zprostředkovatele identity v Mobile Services společně s hello jiných poskytovatelů identit už podporujeme (mezi které patří Accounts Microsoft, Facebook ID, Google ID a Twitter ID). Pokud hello místní aplikace hello používá přihlašovací údaje uživatele, které jsou umístěné v hello společnosti AD DS, musejí být transparentní hello přístup z partnery a uživateli z cloudu hello. Můžete spravovat uživatele podmíněného přístupu řízení too(cloud-based) webových aplikací, webového rozhraní API, Microsoft cloudové služby, aplikace SaaS 3. stran a nativní (mobilní) klientské aplikace a má hello výhody zabezpečení, auditování, vytváření sestav v jednom místní. Je však doporučeno toovalidate, to v testovacím prostředí nebo s omezené množství uživatelů.

> [!TIP]
> je důležité toomention Azure AD má služby AD DS nemá zásad skupiny. V pořadí tooenforce zásady pro zařízení, budete potřebovat řešení správy mobilních zařízení, jako [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).
>
>

Jakmile hello uživatel ověřen pomocí služby Azure AD, je důležité, tooevaluate hello úroveň přístupu, který hello uživatele bude mít ji. Hello úroveň přístupu, který hello uživatele bude mít na prostředku se může lišit, zatímco Azure AD můžete přidat další vrstvu zabezpečení řízení přístup k prostředkům toosome, můžete musí také mějte na paměti, hello prostředků samotné může mít svůj vlastní seznam řízení přístupu samostatně, jako je například hello řízení přístupu pro soubory umístěné na souborovém serveru. Následující obrázek Hello shrnuje hello úrovně řízení přístupu, která může mít v hybridním scénáři:

![](./media/hybrid-id-design-considerations/accesscontrol.png)

Všechny interakce v diagramu hello vám obrázek X představuje jeden scénář řízení přístupu, která mohou být předmětem Azure AD. Zde máte popis jednotlivých scénářů:

1. Podmíněný přístup tooapplications, které jsou hostované na místě: registrovaná zařízení můžete použít se zásadami přístupu pro aplikace, které jsou nakonfigurované toouse služby AD FS v systému Windows Server 2012 R2. Další informace o nastavení podmíněného přístupu pro místní úložiště naleznete v tématu [Nastavení místního podmíněného přístupu pomocí nástroje Registrace zařízení služby Azure Active Directory](active-directory-conditional-access.md).

2. Řízení přístupu toohello portálu Azure: Azure také umožňuje portál toohello řízení přístupu pomocí řízení přístupu na základě role (RBAC)). Tato metoda umožňuje hello společnosti toorestrict hello množství operací, které konkrétního můžete udělat v hello portálu Azure. Pomocí RBAC toocontrol přístup toohello portálu, můžete správci IT delegovat přístup pomocí následujících postupů správy přístupu hello:

* Přiřazení role na základě skupiny: můžete přiřadit přístup tooAzure AD skupin, které mohou být synchronizovány z místní služby Active Directory. To vám umožní tooleverage hello investovali, které vaše organizace má provést v nástroji a procesů pro správu skupin. Můžete také použít funkce správy delegované skupiny hello Azure AD Premium.
* Využívání, které jsou součástí role v Azure: tři role je možné použít – tooensure vlastník, Přispěvatel a čtečky, aby uživatelé a skupiny mají oprávnění toodo pouze hello úlohy toodo potřebují svou práci.
* Tooresources granulární přístupu: můžete přiřadit toousers rolí a skupin pro určitý odběr, skupinu prostředků nebo jednotlivých prostředků Azure, jako je například na webu nebo v databázi. Tímto způsobem zajistíte, že uživatelé mají přístup k prostředkům hello tooall, které potřebují a žádné tooresources přístup, že toomanage není nutné.

> [!NOTE]
> Pokud vytváříte aplikace a chcete řízení přístupu hello toocustomize pro ně, je také možné toouse Azure AD aplikační role pro autorizaci. Zkontrolovat to [WebApp. RoleClaims DotNet příklad](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) o tom, toobuild vaší aplikace toouse tato funkce.
>
>

3. Podmíněný přístup pro aplikace Office 365 s Microsoft Intune: správci IT mohou poskytnout podmíněného přístupu zařízení zásady toosecure podnikovým prostředkům, zatímco v hello stejný čas povolení informační pracovníci na zařízení, která vyhovují tooaccess hello služby. Další informace najdete v tématu [Zásady podmíněného přístupu zařízení pro služby Office 365](active-directory-conditional-access-device-policies.md).

4. Podmíněný přístup pro aplikace Saas: [tuto funkci](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) umožňuje pravidla přístupu k jednotlivým aplikacím služby Multi-Factor authentication tooconfigure hello možnost tooblock přístup pro uživatele není v důvěryhodné síti. Můžete použít hello služby Multi-Factor authentication pravidla tooall uživatele, kteří jsou přiřazeny toohello aplikace, nebo pouze pro uživatele v rámci zadané skupiny zabezpečení. Uživatelé mohou být vyloučeny z požadavku vícefaktorového ověřování hello, pokud přistupují aplikace hello z IP adresy, že v uvnitř hello síti organizace.

Vzhledem k tomu použít s vícevrstvých hello možnosti pro řízení přístupu, se nedají použít pro tuto úlohu porovnání mezi těmito možnostmi. Ujistěte se, že se využívá všechny možnosti, které jsou dostupné pro každý scénář, který vyžaduje, abyste toocontrol přístup k tooyour prostředkům.

## <a name="define-incident-response-options"></a>Definování možností reakce na incidenty
Azure AD může být užitečné IT tooidentity potenciální rizika zabezpečení v prostředí hello sledováním činnost uživatele, může oddělení IT využívají Azure AD přístup a použití sestavy Přehled toogain schopností hello integrity a zabezpečení vaší organizace adresáře. Tyto informace a správce IT pomohou určit, kde může být bezpečnostním rizikům, tak, aby se adekvátní naplánovat toomitigate těchto rizik.  [Předplatné Azure AD Premium](active-directory-get-started-premium.md) obsahuje sadu sestavy zabezpečení, které můžete povolit IT tooobtain tyto informace. [Azure AD sestavy](active-directory-view-access-usage-reports.md) jsou klasifikovány, jak je uvedeno níže:

* **Sestavy anomálií**: obsahovat přihlášení, jsme našli toobe neobvyklé události. Naším cílem je toomake si vědom tyto aktivity a díky kterému budete mít toomake toobe rozhodnutí o tom, zda je událost podezřelé.
* **Integrované sestavy aplikace**: poskytuje přehled o tom, jak cloudové aplikace jsou používány ve vaší organizaci. Azure Active Directory umožňuje integraci s tisíci cloudových aplikací.
* **Zprávy o chybách**: označení chyb, které mohou nastat při zřizování účtů tooexternal aplikace.
* **Uživatelská sestavy**: Zobrazí zařízení nebo přihlášení v datech aktivity pro konkrétního uživatele.
* **Protokoly aktivity**: obsahovat záznam všech auditované události v rámci hello posledních 24 hodin, posledních 7 dní a posledních 30 dní, a také změny aktivity skupiny a aktivita resetování a registraci hesla.

> [!TIP]
> Další sestavy, který může také pomoct hello reakcí na incidenty team práce v případě je hello [uživatele s uniklé přihlašovací údaje](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) sestavy.  Tato sestava zobrazí všechny shody mezi tyto seznamu uniklé přihlašovací údaje a vašeho klienta.
>
>

Další důležité součástí sestavy ve službě Azure AD, které je možné během šetření reakcí na incidenty a jsou:

* **Aktivita resetování hesla**: Dobrý den, správce poskytnout přehled o tom, jak aktivně resetování hesla se používá v organizaci hello.
* **Registrace aktivita resetování hesla**: poskytuje přehledy, do kterých uživatelé zaregistrovali své metody pro resetování hesla, a metody, které jste vybrali.
* **Skupina aktivit**: poskytuje historii změn skupiny toohello (např: uživatelé přidat nebo odebrat) které byly zahájeny v hello přístupového panelu.

Kromě toho toohello základní generování sestav funkce dostupné v Azure AD Premium, které můžete využít během procesu šetření reakcí na incidenty, IT oddělení můžete také využít sestava auditu tooobtain informace, jako:

* Změny v členství role (např: Přidání tooGlobal správce role uživatele)
* Aktualizace na přihlašovací údaje (např: změny hesla)
* Správa domény (např: ověření vlastní domény, odebrání domény)
* Přidání nebo odebrání aplikací
* Správa uživatelů (např: Přidání, odebrání, aktualizaci uživatele)
* Přidání nebo odebrání licencí

Vzhledem k tomu použít s vícevrstvých hello možností reakce na incidenty, se nedají použít pro tuto úlohu porovnání mezi těmito možnostmi. Ujistěte se, že se využívá všechny možnosti, které jsou dostupné pro každý scénář, který vyžaduje, abyste toouse funkce generování sestav Azure AD jako součást procesu reakcí na incidenty vaší společnosti.

## <a name="next-steps"></a>Další kroky
[Určení úlohy správy hybridní identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
