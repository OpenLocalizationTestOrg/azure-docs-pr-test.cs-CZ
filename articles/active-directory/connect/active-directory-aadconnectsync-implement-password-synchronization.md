---
title: Synchronizace hesel aaaImplement s Azure AD Connect sync | Microsoft Docs
description: Jak funguje synchronizace hesel a poskytuje informace o tooset nahoru.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: a0401640f2a4d914419ee4446f923bb3a972389d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-password-synchronization-with-azure-ad-connect-sync"></a>Implementace synchronizace hesel s synchronizace Azure AD Connect
Tento článek obsahuje informace, je nutné toosynchronize vaše hesel uživatelů mezi místní instanci Active Directory instance tooa cloudové služby Azure Active Directory (Azure AD).

## <a name="what-is-password-synchronization"></a>Co je synchronizace hesel
Hello pravděpodobnost, že jste se blokovat práce s aplikací z důvodu tooa zapomenete heslo související číslo toohello různá hesla musíte tooremember. Hello více hesel, budete potřebovat tooremember hello vyšší hello pravděpodobnosti tooforget jeden. Dotazy a volání o resetování hesla a další problémy související s hesly vyžádání hello nejvíce zdrojů technickou podporu.

Synchronizace hesel je že funkce používá toosynchronize hesel uživatelů z místní služby Active Directory instance tooa cloudové služby Azure AD instance.
Pomocí této funkce toosign v tooAzure AD služby, jako je Office 365, Microsoft Intune, CRM Online a Azure Active Directory Domain Services (Azure AD DS). Přihlášení služby toohello pomocí hello stejné heslo, které použijete v tooyour toosign místní instanci Active Directory.

![Co je služba Azure AD Connect](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Protože se sníží hello počet hesel, uživatelé potřebovat toomaintain toojust jeden. Synchronizace hesel vám pomůže:

* Zvýšení produktivity hello uživatelů.
* Snižte náklady na technickou podporu.  

Navíc pokud se rozhodnete toouse [federační službou Active Directory Federation Services (AD FS)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), můžete volitelně můžete nastavit synchronizaci hesel jako záložní případ selhání infrastruktury služby AD FS.

Synchronizace hesel je funkce synchronizace adresáře toohello rozšíření implementované synchronizace Azure AD Connect. Synchronizace hesel toouse ve vašem prostředí, budete muset:

* Instalovat Azure AD Connect.  
* Konfigurace synchronizace adresářů mezi vaší místní instancí Active Directory a vaše instance služby Azure Active Directory.
* Povolení synchronizace hesel.

Podrobnější informace najdete v článku [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

> [!NOTE]
> Další podrobnosti o Azure Active Directory Domain Services nakonfigurovaná pro synchronizaci FIPS a heslo najdete v části "synchronizace hesel a FIPS" později v tomto článku.
>
>

## <a name="how-password-synchronization-works"></a>Princip funkce Synchronizace hesla
Hello služba Active Directory domain Services ukládá hesla v podobě hello reprezentace hodnotu hash hello skutečného uživatelského hesla. Hodnota hash je výsledek jednosměrné matematické funkce (hello *algoritmus hash*). Neexistuje žádný metoda toorevert hello výsledek jednosměrné funkce toohello prostý text verze heslo. V místní síti tooyour nelze použít toosign hash a heslo.

toosynchronize, které vaše heslo, synchronizace Azure AD Connect extrahuje vaše hodnoty hash hesla z hello místní instanci Active Directory. Další bezpečnostní zpracování je použita hodnota hash hesla toohello předtím, než je synchronizován toohello Služba ověřování v Azure Active Directory. Hesla se synchronizují na jednotlivé uživatele a v chronologickém pořadí.

Hello samotný datový tok procesu synchronizace hesla hello je podobné toohello synchronizace uživatelských dat, jako je například DisplayName nebo e-mailové adresy. Ale hesla se synchronizují častěji než standardní directory hello časový interval synchronizace pro ostatní atributy. procesu synchronizace hesla Hello spouští každých 2 minut. Nelze změnit frekvenci hello tohoto procesu. Při synchronizaci hesla, přepíše stávající heslo cloudu hello.

Hello poprvé povolíte hello funkce Synchronizace hesla, provede počáteční synchronizaci hesel hello všech uživatelů v oboru. Podmnožinu uživatelská hesla, které chcete toosynchronize nemůže explicitně definovat.

Pokud změníte heslo k místní, hello aktualizovat heslo je synchronizován, nejčastěji během několika minut.
funkce Synchronizace hesla Hello automaticky opakovat pokusy o synchronizaci se nezdařilo. Pokud dojde k chybě během toosynchronize pokusu o zadání hesla, je váš prohlížeč událostí přihlášení k chybě.

Hello synchronizace hesla nemá žádný vliv na hello uživatele, který je aktuálně přihlášený.
Aktuální relace cloudové služby nemá vliv okamžitě změnu hesla synchronizované, ke kterému dochází, když jste přihlášení tooa cloudové služby. Ale pokud hello Cloudová služba vyžaduje, abyste tooauthenticate znovu, je potřeba tooprovide nové heslo.

Musí uživatel zadat svoje podnikové přihlašovací údaje druhý čas tooauthenticate tooAzure AD, bez ohledu na to, jestli jste přihlášení tootheir podnikové síti. Tyto vzor můžete minimalizovat, ale pokud hello uživatel vybere hello zachovat mi podepsané v políčko (KMSI) při přihlašování. Tento výběr nastaví soubor cookie relace, který obchází ověřování na krátkou dobu. Chování KMSI můžete povolit nebo zakázat pomocí Správce Azure AD hello.

> [!NOTE]
> Synchronizace hesla je podporována pouze pro uživatele typu hello objektu ve službě Active Directory. Nepodporuje pro typ objektu iNetOrgPerson hello.

### <a name="detailed-description-of-how-password-synchronization-works"></a>Podrobný popis toho, jak funguje synchronizace hesel
Hello následující text popisuje podrobný Princip funkce Synchronizace hesla mezi služby Active Directory a Azure AD.

![Tok podrobné heslo](./media/active-directory-aadconnectsync-implement-password-synchronization/arch3.png)


1. Každé dvě minuty, agent synchronizace hesel hello na server hello AD Connect požadavky hodnot hash hesel uložených (atribut unicodePwd hello) z řadiče domény přes standardní hello [MS-DRSR](https://msdn.microsoft.com/library/cc228086.aspx) toosynchronize dat používá replikace protokol mezi řadiče domény. Replikovat změny adresáře a replikovat Directory změny všechny AD oprávnění (udělená na instalaci ve výchozím nastavení) tooobtain hello hodnot hash hesel, musí mít účet služby Hello.
2. Před odesláním, hello řadič domény šifruje hello MD4 hodnoty hash hesla pomocí klíče, který je [MD5](http://www.rfc-editor.org/rfc/rfc1321.txt) hash klíče relace hello RPC a salt. Pak odešle agent synchronizace hesel toohello výsledek hello prostřednictvím protokolu RPC. Hello řadič domény také předá hello řetězce salt toohello synchronizace agenta pomocí protokolu replikace hello řadič domény, takže hello agenta bude možné toodecrypt hello obálky.
3.  Po agent synchronizace hesel hello má hello šifrované obálky, používá [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) a hello řetězce salt toogenerate MD4 původním formátu back tooits služby klíče toodecrypt hello přijal data. Žádné okamžiku má agent synchronizace hesel hello hesla v nešifrovaném textu toohello přístup. Hello agent synchronizace hesel použití algoritmu MD5 je určené výhradně pro kompatibilitu s protokolem replikace s hello řadiče domény a používá se pouze místní mezi hello řadiče domény a agent synchronizace hesel hello.
4.  agent synchronizace hesel Hello rozšíří hello binární heslo 16 bajtů hash too64 bajtů první převodu hello hash tooa 32 bajtů hexadecimální řetězce, pak tento řetězec převod zpět do binární kódování UTF-16.
5.  agent synchronizace hesel Hello přidá salt, skládající se z délka v bajtech 10 salt, toohello 64 bajtů binární toofurther chránit hello původní hodnoty hash.
6.  agent synchronizace hesel Hello pak kombinuje hash hello MD4 plus salt a vstup do hello [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt) funkce. 1000 iterací hello [HMAC SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) klíčovaný algoritmus hash se používá. 
7.  agent synchronizace hesel Hello trvá hello výsledné 32 bajtů hash, zřetězí obou hello salt a hello počet iterací tooit SHA256 (pro použití služby Azure AD) a pak přenáší hello řetězec z Azure AD Connect tooAzure AD přes protokol SSL.</br> 
8.  Když uživatel pokusí toosign v tooAzure AD a zadá své heslo, hello heslo je spuštění prostřednictvím hello stejné MD4 + řetězce salt + PBKDF2 + HMAC SHA256 procesu. Pokud výsledný hash hello odpovídající hodnotě hash hello uložené ve službě Azure AD, hello uživatel zadá hello správné heslo a je ověřen. 

>[!Note] 
>Hodnota hash MD4 původní Hello není přenášená tooAzure AD. Místo toho se přenášejí hello hello původní hodnota hash MD4 algoritmus hash SHA256. Výsledkem je pokud je získali hello hash uložené ve službě Azure AD, ho nelze použít při útoku pass-the-hash místně.

### <a name="how-password-synchronization-works-with-azure-active-directory-domain-services"></a>Princip funkce Synchronizace hesla s Azure Active Directory Domain Services
Můžete také použít toosynchronize funkce Synchronizace hesla hello místních hesel příliš[Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). V tomto scénáři instanci Azure Active Directory Domain Services hello ověří vaši uživatelé v cloudu hello všechny metody hello dostupné v místní instanci služby Active Directory. činnost Hello tohoto scénáře se toousing hello podobné migrace nástroj ADMT (Active Directory) v místním prostředí.

### <a name="security-considerations"></a>Aspekty zabezpečení
Při synchronizaci hesel, není ve formátu prostého textu hello vašeho hesla, funkce Synchronizace hesla zveřejněné toohello, tooAzure AD nebo všechny služby související hello.

Ověření uživatele probíhá služby Azure AD a nikoli proti hello organizace služby Active Directory. Pokud má vaše organizace pochybnostmi data hesel v žádný formulář ponechat hello místní, zvažte hello fakt, že hello SHA256 heslo data uložená ve službě Azure AD – hodnota hash hello původní hodnota hash MD4 – je výrazně bezpečnější než co je uložený ve službě Active Directory. Dál platí protože tato hodnota hash SHA256 nelze dešifrovat, nemohou být brzy vrátit prostředí služby Active Directory toohello organizace a zobrazí jako platné uživatelské heslo v rámci útoku pass-the-hash.





### <a name="password-policy-considerations"></a>Aspekty zásad hesel
Existují dva typy zásad hesel, které jsou ovlivněné povolení synchronizace hesel:

* Zásady složitost hesel
* Zásad vypršení platnosti hesla

#### <a name="password-complexity-policy"></a>Zásady složitost hesel  
Pokud je povolená synchronizace hesel, zásady pro složitost hesla hello v místní instanci služby Active Directory přepsat složitost zásady v cloudu hello pro synchronizované uživatele. Můžete použít všechny platné hesel hello z vaší místní služby Active Directory instance tooaccess služby Azure AD.

> [!NOTE]
> Hesla pro uživatele, které jsou vytvořené přímo v cloudu hello jsou stále subjektu toopassword zásady, jak jsou definovány v cloudu hello.

#### <a name="password-expiration-policy"></a>Zásad vypršení platnosti hesla  
Pokud je uživatel v oboru hello synchronizace hesel, je příliš nastavit heslo pro účet cloudu hello*nikdy nevyprší*.

Toosign může pokračovat v tooyour cloudové služby s použitím synchronizované heslo, které je vypršení její platnosti v místním prostředí. Heslo cloudu se aktualizuje hello příští změně hesla hello v místním prostředí hello.

#### <a name="account-expiration"></a>Vypršení platnosti účtu
Pokud vaše organizace používá atribut hello AccountExpires zadává jako součást Správa uživatelských účtů, mějte na paměti, že tento atribut není synchronizovaná tooAzure AD. V důsledku toho vypršenou platností účet služby Active Directory v prostředí nakonfigurovaná pro synchronizaci hesla bude nadále aktivní ve službě Azure AD. Doporučujeme vám, že pokud vypršela platnost účtu hello, by měly aktivovat akce pracovního postupu, skript prostředí PowerShell, která zakáže účet hello uživatele Azure AD. Naopak když účet hello je zapnuté, instance služby Azure AD hello měl by být zapnut.

### <a name="overwrite-synchronized-passwords"></a>Přepsat synchronizované hesla
Správce můžete ručně obnovit heslo pomocí prostředí Windows PowerShell.

V takovém případě nové heslo hello přepíše synchronizované heslo a nové heslo použité toohello jsou definované v cloudu hello všechny zásady pro hesla.

Pokud změníte místní heslo, nové heslo hello se synchronizují toohello cloudu a přepíše hello ručně aktualizovat hesla.

Hello synchronizace hesla nemá žádný vliv na hello Azure uživatele, který je přihlášený. Aktuální relace cloudové služby nemá vliv okamžitě změnu hesla synchronizované, ke kterému dochází, když jste přihlášení tooa cloudové služby. KMSI rozšiřuje hello trvání tento rozdíl. Když hello Cloudová služba vyžaduje, abyste tooauthenticate znovu, musíte tooprovide nové heslo.

### <a name="additional-advantages"></a>Další výhody

- Obecně platí synchronizace hesel je jednodušší tooimplement než federační služby. Nevyžaduje žádné další servery a eliminuje závislost na vysoce dostupný federační služby tooauthenticate uživatelů. 
- Synchronizace hesel se dá taky povolit v přidání toofederation. Mohou být použity jako zálohu Pokud své služby FS dojde k výpadku.












## <a name="enable-password-synchronization"></a>Povolení synchronizace hesel
Při instalaci Azure AD Connect s použitím hello **Expresní nastavení** možnost synchronizace hesel je automaticky povolené. Další podrobnosti najdete v tématu [Začínáme s Azure AD Connect pomocí expresního nastavení](active-directory-aadconnect-get-started-express.md).

Pokud použijete vlastní nastavení při instalaci Azure AD Connect, synchronizace hesel je k dispozici na stránce přihlášení uživatele hello. Další podrobnosti najdete v tématu [vlastní instalace Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

![Povolení synchronizace hesel](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Synchronizace hesel a FIPS
Pokud váš server byl uzamčen podle tooFederal Information Processing Standard (FIPS), je zakázané MD5.

**tooenable MD5 pro synchronizace hesel, proveďte následující kroky hello:**

1. Přejděte Sync\Bin too%programfiles%\Azure AD.
2. Otevřete miiserver.exe.config.
3. Přejděte toohello konfigurace/runtime uzlu na konci hello hello souboru.
4. Přidejte hello následující uzly:`<enforceFIPSPolicy enabled="false"/>`
5. Uložte provedené změny.

Pro srovnání je tento fragment kódu, co by měl vypadat jako:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Informace o zabezpečení a FIPS najdete v tématu [AAD Sync heslo, šifrování a FIPS dodržování předpisů](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/).

## <a name="troubleshoot-password-synchronization"></a>Řešení potíží s synchronizace hesel
Pokud máte problémy s synchronizace hesel, najdete v části [řešení synchronizace hesel](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="next-steps"></a>Další kroky
* [Synchronizace Azure AD Connect: přizpůsobení možnosti synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
