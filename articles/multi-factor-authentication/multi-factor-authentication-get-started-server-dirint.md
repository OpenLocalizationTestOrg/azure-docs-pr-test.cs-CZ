---
title: aaaDirectory integrace mezi Azure Multi-Factor Authentication a Active Directory
description: "Toto je stránka Azure Multi-Factor authentication hello, který popisuje, jak toointegrate hello Azure Multi-Factor Authentication Server se službou Active Directory, aby bylo možné synchronizovat adresáře hello."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: def7a534-cfb2-492a-9124-87fb1148ab1f
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: fbff518b4641010d5f7745096e0ff658864d0805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Integrace adresáře mezi Azure MFA Serverem a službou Active Directory
Části integrace adresáře hello toointegrate hello Azure MFA serveru pomocí služby Active Directory nebo jiným adresářem LDAP. Můžete nakonfigurovat schématu adresáře hello toomatch atributy a nastavit synchronizaci automatické uživatele.

## <a name="settings"></a>Nastavení
Ve výchozím nastavení hello Azure Multi-Factor Authentication (MFA) Server je nakonfigurovaný tooimport nebo synchronizaci uživatelů ze služby Active Directory.  Karta integrace adresáře Hello umožňuje vám toooverride hello výchozí chování a toobind tooa jiný adresář LDAP, adresář ADAM nebo konkrétní řadič domény služby Active Directory.  Poskytuje také pro hello použití ověřování pomocí protokolu LDAP tooproxy LDAP nebo pro vázání protokolu LDAP jako cíl RADIUS, předběžné ověření pro ověřování IIS nebo primární ověřování pro portál User Portal.  Hello následující tabulka popisuje jednotlivé nastavení hello.

![Nastavení](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Funkce | Popis |
| --- | --- |
| Použít Active Directory |Vyberte toouse možnost používat službu Active Directory hello služby Active Directory pro importování a synchronizaci.  Toto je výchozí nastavení hello. <br>Poznámka: Pro službu Active Directory integration toowork správně, připojení hello počítače tooa doméně a přihlaste se pomocí účtu domény. |
| Zahrnout důvěryhodné domény |Zkontrolujte **zahrnout důvěryhodné domény** toohave hello agenta pokus o tooconnect toodomains důvěryhodný hello aktuální domény, jinou doménu v doménové struktuře hello nebo doménách, které se účastní vztahu důvěryhodnosti doménové struktury.  Pokud není import nebo synchronizaci uživatelů ze všech hello důvěryhodné domény, zrušte zaškrtnutí políčka hello políčko tooimprove výkonu.  Hello výchozím nastavení je zaškrtnuto. |
| Použít specifickou konfiguraci LDAP |Vyberte hello používat LDAP možnost toouse hello LDAP nastavení zadaných pro import a synchronizaci. Poznámka: Pokud je vybraná možnost použít LDAP, hello uživatelské rozhraní se změní odkazy z tooLDAP služby Active Directory. |
| Tlačítko Upravit |tlačítko Upravit Hello umožňuje toomodified nastavení konfigurace aktuální LDAP hello. |
| Použít dotazy na obor atributů |Určuje, jestli se mají použít dotazy na obor atributů.  Dotazy oboru atributů umožňují provádět efektivní prohledávání adresáře kvalifikaci záznamů podle položek hello v jiném atributu záznamu.  Hello Azure Multi-Factor Authentication Server používá atribut oboru dotazy tooefficiently dotazu hello uživatele, kteří jsou členy skupiny zabezpečení.   <br>Poznámka:  Existují případy, kdy se sice dotazy na obor atributů podporují, ale použít by se neměly.  Active Directory třeba může mít s dotazy na obor atributů problémy, pokud skupina zabezpečení obsahuje členy z více než jedné domény. V takovém případě zrušte výběr zaškrtávacího políčka hello. |

Hello následující tabulka popisuje hello nastavení konfigurace LDAP.

| Funkce | Popis |
| --- | --- |
| Server |Zadejte název hello hostitele nebo IP adresa serveru hello kterém běží adresář LDAP hello.  Můžete taky zadat záložní server oddělený středníkem. <br>Poznámka: Když je Typ vazby nastavený na SSL, je potřeba zadat plně kvalifikovaný název hostitele. |
| Základní rozlišující název |Zadejte hello rozlišující název objektu hello základního adresáře, ze kterého se spuštění všechny dotazy na adresář.  Příklad: dc=abc,dc=com. |
| Typ vazby - Dotazy |Vyberte hello vhodný typ vazby pro použití při vytváření vazby adresář LDAP toosearch hello.  Používá se pro importy, synchronizaci a překlad uživatelského jména. <br><br>  Anonymní – Vytvoří se anonymní vazba.  Hodnoty Rozlišující název vázání a Heslo vázání se nepoužijí.  To funguje jenom v případě hello adresář LDAP umožňuje anonymní vytváření vazby a oprávnění umožňují dotazování hello hello příslušné záznamy a atributy.  <br><br> Jednoduchá - hodnoty rozlišující název vázání a heslo vázání se předávají jako prostý text toobind toohello LDAP adresář.  Toto je pro účely testování, tooverify, který hello server dostupný a má tento účet hello vazby odpovídající přístup hello. Po instalaci odpovídajícího certifikátu hello použijte protokol SSL.  <br><br> SSL - hodnoty rozlišující název vázání a heslo vazby jsou šifrované pomocí adresář LDAP toohello toobind SSL.  Nainstalujte místně certifikátu, který hello vztahy důvěryhodnosti adresáře LDAP.  <br><br> Windows - uživatelské jméno vázání a heslo vázání se použité toosecurely připojení tooan řadič domény služby Active Directory nebo aplikačního režimu služby Active directory.  Pokud uživatelské jméno vazby zůstane prázdné, hello přihlášeného uživatelského účtu je použité toobind. |
| Typ vazby - Ověřování |Vyberte hello vhodný typ vazby pro použití při ověřování vazby LDAP.  Najdete v popisech typu hello vazby v části Typ vazby – dotazy.  Například to umožňuje toobe anonymní vázání pro dotazy použít protokol SSL při ověřování vazby LDAP použité toosecure. |
| Rozlišující název vázání nebo Uživatelské jméno vázání |Zadejte hello rozlišující název záznamu uživatele hello toouse hello účet při vytváření vazby toohello adresář LDAP.<br><br>Hello rozlišující název vazby se používá pouze v případě vazby typu jednoduché nebo SSL.  <br><br>Při vytváření vazby toohello adresář LDAP, když je typ vazby nastavený Windows, zadejte uživatelské jméno hello toouse účet Windows hello.  Pokud zůstane prázdné, hello přihlášeného uživatelského účtu je použité toobind. |
| Heslo vázání |Zadejte heslo vazby hello hello rozlišující název vázání nebo uživatelské jméno se adresář LDAP toohello použité toobind.  tooconfigure hello heslo pro hello Multi-Factor Auth Server AdSync Service, povolit synchronizaci a zkontrolujte, zda je spuštěna služba hello v místním počítači hello.  Hello heslo je uloženo v hello Windows uložených uživatelských jmen a hesel pod hello hello účet, kterým běží služba Multi-Factor Auth Server AdSync Service.  heslo Hello je také uloženo pod hello hello účet, kterým běží uživatelské rozhraní, jako a v části hello účet hello Multi-Factor Auth Server služby Multi-Factor Auth Server běží.  <br><br>Vzhledem k tomu, že hello heslo uloženo pouze v systému Windows v úložišti uživatelských jmen a hesel hello místního serveru, opakujte tento krok na každém Multi-Factor Auth serveru, který potřebuje přístup toohello heslo. |
| Omezení velikosti dotazu |Zadejte maximální velikost hello hello maximální počet uživatelů, které vrátí vyhledávání v adresáři.  Tento limit by měl odpovídat hello konfiguraci adresáře LDAP hello.  Pro velké vyhledávání, kde se nepodporuje stránkování import a synchronizaci pokusí tooretrieve uživatele v dávkách.  Pokud je zde větší než hello limit konfigurovaný pro adresář LDAP hello nastavený limit velikosti hello, nemusí být někteří uživatelé načteni. |
| Tlačítko Test |Klikněte na tlačítko **Test** server LDAP toohello tootest vazby.  <br><br>Nepotřebujete tooselect hello **používat LDAP** možnost tootest vazby. To umožňuje toobe vazby hello testována před použitím konfigurace LDAP hello. |

## <a name="filters"></a>Filtry
Filtry povolit tooset kritéria tooqualify záznamů při prohledávání adresáře.  Nastavení hello filtr, můžete určit obor hello objekty chcete toosynchronize.  

![Filtry](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure Multi-Factor Authentication má hello následující tři možnosti filtru:

* **Filtr kontejneru** -zadejte použít kritéria filtru pro hello tooqualify záznamů kontejneru při prohledávání adresáře.  Pro Active Directory a ADAM se obvykle používá (|(objectClass=organizationalUnit)(objectClass=container)).  Pro ostatní adresáře LDAP použijte kritéria filtru pro vyfiltrování jednotlivých typů objektů kontejneru, v závislosti na schématu adresáře hello.  <br>Poznámka: Pokud je toto pole prázdné, použije se výchozí hodnota ((objectClass=organizationalUnit)(objectClass=container)).
* **Filtr skupiny zabezpečení** -zadejte kritéria filtru hello používá tooqualify záznamů skupiny zabezpečení při prohledávání adresáře.  Pro Active Directory a ADAM se obvykle používá (&(objectCategory=group)(groupType:1.2.840.113556.1.4.804:=-2147483648)).  Pro ostatní adresáře LDAP použijte kritéria filtru pro vyfiltrování jednotlivých typů objektů skupiny zabezpečení, v závislosti na schématu adresáře hello.  <br>Poznámka: Pokud je toto pole prázdné, použije se výchozí hodnota (&(objectCategory=group)(groupType:1.2.840.113556.1.4.804:=-2147483648)).
* **Filtr uživatelů** -zadejte kritéria filtru hello používá tooqualify uživatelských záznamů při prohledávání adresáře.  Pro Active Directory a ADAM se obvykle používá (&(objectClass=user)(objectCategory=person)).  Pro ostatní adresáře LDAP použít (objectClass = inetOrgPerson) nebo něco podobné, v závislosti na schématu adresáře hello. <br>Poznámka: Pokud je toto pole prázdné, použije se výchozí hodnota (&(objectCategory=person)(objectClass=user)).

## <a name="attributes"></a>Atributy
Atributy můžete podle potřeby upravit pro konkrétní adresář.  To vám umožní tooadd vlastní atributy a systém doladit hello synchronizace tooonly hello atributy, které potřebujete. Použijte název hello hello attricute definovaným ve schématu adresáře hello hello hodnotu každé pole atributu. Hello následující tabulka obsahuje další informace o jednotlivých funkcích.

Atributy lze zadat ručně a nejsou požadované toomatch atribut v seznamu atributů hello.

![Atributy](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Funkce | Popis |
| --- | --- |
| Jedinečný identifikátor |Zadejte název atributu hello hello atributu, který slouží jako hello jedinečný identifikátor kontejneru, skupiny zabezpečení a uživatelských záznamů.  V Active Directory je to obvykle objectGUID. V jiných implementacích LDAP se může používat entryUUID nebo něco podobného.  Výchozí hodnota Hello je objectGUID. |
| Typ jedinečného identifikátoru |Vyberte typ hello hello atribut jedinečného identifikátoru.  Ve službě Active Directory je hello má atribut objectGUID typu GUID. V jiných implementacích LDAP se může používat typ Pole bajtů ASCII nebo Řetězec.  Hello výchozí hodnota je GUID. <br><br>Je důležité tooset tento typ správně od synchronizační položky je odkazováno dle jejich jedinečného identifikátoru. Hello typ jedinečného identifikátoru je použité toodirectly najít hello objektu v adresáři hello.  Tento typ tooString jsou při hello adresář ve skutečnosti ukládá hello hodnotu jako bajtové pole znaků ASCII nastavení brání synchronizace nebude fungovat správně. |
| Rozlišující název |Zadejte název atributu hello hello atributu, který obsahuje hello rozlišující název pro každý záznam.  V Active Directory je to obvykle distinguishedName. V jiných implementacích LDAP se může používat entryDN nebo něco podobného.  Hello výchozí hodnota je distinguishedName. <br><br>Pokud atribut obsahující pouze rozlišující název hello neexistuje, může použít atribut adspath hello.  Hello "LDAP: / /\<server\>/" část cesty hello se automaticky odstraní vypnutý, a právě hello rozlišující název objektu hello. |
| Název kontejneru |Zadejte název atributu hello hello atributu, který obsahuje název hello v záznamu kontejneru.  Hello hodnota tohoto atributu se zobrazí v hello hierarchii kontejneru při importu ze služby Active Directory nebo přidávání synchronizačních položek.  Hello výchozí hodnota je name. <br><br>Pokud různé kontejnery používají pro své názvy různé atributy, použijte středníky tooseparate více atributů názvu kontejneru.  Hello první atribut názvu kontejneru nalezený na objektu kontejneru je použité toodisplay její název. |
| Název skupiny zabezpečení |Zadejte název atributu hello hello atributu, který obsahuje název hello v záznamu skupiny zabezpečení.  Hello hodnota tohoto atributu se zobrazí v seznamu skupiny zabezpečení hello při importu ze služby Active Directory nebo přidávání synchronizačních položek.  Hello výchozí hodnota je name. |
| Uživatelské jméno |Zadejte název atributu hello hello atributu, který obsahuje hello uživatelské jméno v záznamu uživatele.  Hello hodnota tohoto atributu se používá jako uživatelské jméno Multi-Factor Auth Server hello.  Druhý atribut možné zadat jako zálohování toohello nejdřív.  druhý atribut Hello se používá pouze v případě hello první atribut neobsahuje hodnotu pro uživatele hello.  Hello výchozí hodnoty jsou userPrincipalName a sAMAccountName. |
| Jméno |Zadejte název atributu hello hello atributu, který obsahuje hello křestní jméno v záznamu uživatele.  Hello výchozí hodnota je givenName. |
| Příjmení |Zadejte název atributu hello hello atributu, který obsahuje hello příjmení v záznamu uživatele.  Hello výchozí hodnota je sn. |
| E-mailová adresa |Zadejte název atributu hello hello atributu, který obsahuje hello e-mailovou adresu v záznamu uživatele.  E-mailová adresa je použité toosend úvodní a aktualizaci uživatele toohello e-mailů.  Hello výchozí hodnota je mail. |
| Uživatelská skupina |Zadejte název atributu hello hello atributu, který obsahuje hello skupiny uživatelů v záznamu uživatele.  Skupiny uživatelů lze použít toofilter uživatelů v agentovi hello a v sestavách v portálu pro správu Multi-Factor Auth Server hello. |
| Popis |Zadejte název atributu hello hello atributu, který obsahuje popis hello v záznamu uživatele.  Popis se používá jen pro vyhledávání.  Hello výchozí hodnota je description. |
| Jazyk telefonního hovoru |Zadejte název atributu hello hello atributu, který obsahuje hello krátký název toouse hello jazyka pro hlasové hovory hello uživatele. |
| Jazyk textové zprávy |Zadejte název atributu hello hello atributu, který obsahuje hello krátký název toouse hello jazyka pro SMS zprávy pro uživatele hello. |
| Jazyk mobilní aplikace |Zadejte název atributu hello hello atributu, který obsahuje hello krátký název toouse hello jazyk pro telefonní aplikace textové zprávy pro uživatele hello. |
| Jazyk tokenu OATH |Zadejte název atributu hello hello atributu, který obsahuje hello krátký název toouse hello jazyka pro textové zprávy tokenu OATH pro uživatele hello. |
| Telefon do zaměstnání |Zadejte název atributu hello hello atributu, který obsahuje hello firmy telefonní číslo v záznamu uživatele.  Hello výchozí hodnota je telephoneNumber. |
| Telefon domů |Zadejte název atributu hello hello atributu, který obsahuje hello telefonní číslo domů v záznamu uživatele.  Hello výchozí hodnota je homePhone. |
| Pager |Zadejte název atributu hello hello atributu, který obsahuje číslo pageru hello v záznamu uživatele.  Hello výchozí hodnota je pager. |
| Mobilní telefon |Zadejte název atributu hello hello atributu, který obsahuje hello číslo mobilního telefonu v záznamu uživatele.  Výchozí hodnota Hello je mobile. |
| Fax |Zadejte název atributu hello hello atributu, který obsahuje hello číslo faxu v záznamu uživatele.  Hello výchozí hodnota je facsimileTelephoneNumber. |
| IP telefon |Zadejte název atributu hello hello atributu, který obsahuje hello IP telefonní číslo v záznamu uživatele.  Hello výchozí hodnota je ipPhone. |
| Vlastní |Zadejte název atributu hello hello atributu, který obsahuje vlastní telefonní číslo v záznamu uživatele.  Výchozí hodnota Hello je prázdná. |
| Linka |Zadejte název atributu hello hello atributu, který obsahuje hello linku telefonního čísla v záznamu uživatele.  jako hello rozšíření toohello primární telefonní číslo pouze se používá hodnota Hello hello rozšíření.  Výchozí hodnota Hello je prázdná. <br><br>Pokud není zadán atribut hello rozšíření, může být součástí atribut telefonního hello rozšíření. V takovém případě musí předcházet hello rozšíření s "x", aby získá správně analyzovat.  Například 555-123-4567 x890 výsledkem by 555-123-4567 jako hello telefonní číslo a 890 hello rozšíření. |
| Tlačítko Obnovit výchozí |Klikněte na tlačítko **obnovit výchozí nastavení** tooreturn všechny atributy zpět tootheir výchozí hodnotu.  Hello výchozí hodnoty by měly fungovat správně s běžným schématem služby Active Directory nebo ADAM na hello. |

Klikněte na tlačítko tooedit atributy **upravit** na kartě atributy hello.  Otevře okno, kde můžete upravit atributy hello. Vyberte hello **...**  další tooany atribut tooopen okno, kde si můžete vybrat, které toodisplay atributy.

![Upravit atributy](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Synchronizace
Synchronizace udržuje databázi uživatelů Azure MFA hello synchronizovat se službou hello uživatelů služby Active Directory nebo jiném adresáři přístup protokolu LDAP (Lightweight Directory). Hello proces je podobný tooimporting uživatelé ručně ze služby Active Directory, ale pravidelně se dotazuje na uživatele služby Active Directory a tooprocess změny ve skupině zabezpečení.  Zároveň zakazuje a odstraňuje uživatele, kteří byli odebráni z kontejneru, skupiny zabezpečení nebo služby Active Directory.

Hello služba Multi-Factor Auth ADSync je služba systému Windows, který provádí hello pravidelné cyklického dotazování služby Active Directory.  Toto není toobe zaměňovat s Azure AD Sync nebo Azure AD Connect.  Hello Multi-Factor Auth ADSync postavená na podobném základu kódu, sice konkrétní toohello Azure Multi-Factor Authentication Server.  Je nainstalována v zastaveném stavu a se spustila podle hello služba Multi-Factor Auth serveru při konfiguraci toorun.  Pokud máte víceserverovou konfiguraci služby Multi-Factor Auth Server, hello Multi-Factor Auth ADSync může spustit pouze na jednom serveru.

Hello služba Multi-Factor Auth ADSync používá rozšíření serveru DirSync LDAP poskytované Microsoft tooefficiently dotazování na změny hello.  Tento řídicí volající DirSync musí mít hello "directory get změny" vpravo a DS-Replication-Get-Changes rozšířené právo k řízení přístupu.  Ve výchozím nastavení jsou tato práva přiřazena toohello účty správce a LocalSystem na řadičích domény.  Hello služba Multi-Factor Auth AdSync ve výchozím nastavení je nakonfigurované toorun pod účtem LocalSystem.  Je proto nejjednodušší službu hello toorun na řadiči domény.  Pokud nakonfigurujete hello služby tooalways provést úplnou synchronizaci, může běžet pod účtem s nižší úrovní oprávnění.  Není to tak efektivní, ale nejsou k tomu potřeba úplná práva.

Pokud adresář LDAP hello podporuje a je nakonfigurován pro nástroj DirSync, pak dotazování pro změny skupiny uživatelů a bude fungovat hello stejné jako u služby Active Directory.  Pokud adresář LDAP hello nepodporuje hello ovládací prvek DirSync, je při každém cyklu provedena úplná synchronizace.

![Synchronizace](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Hello následující tabulka obsahuje další informace o každém z nastavení na kartě Synchronizace hello.

| Funkce | Popis |
| --- | --- |
| Povolit synchronizaci se službou Active Directory |Pokud je zaškrtnuto, služba Multi-Factor Auth Server hello pravidelně se dotazuje služby Active Directory na změny. <br><br>Poznámka: musí být přidán alespoň jednu položku synchronizace a synchronizovat je nutné provést před hello Multi-Factor Auth Server začne zpracovávat změny služba. |
| Synchronizovat každých |Zadejte hello čas interval hello Multi-Factor Auth Server service čekat mezi jednotlivými dotazy zpracováními změn. <br><br> Poznámka: hello intervalu zadaném je hello čas mezi začátky jednotlivých cyklů hello.  Pokud hello doba zpracování změny překračují hello interval, bude hello služba dotazovat okamžitě znovu. |
| Odebrat uživatele, kteří již nejsou ve službě Active Directory |Pokud je zaškrtnuto, hello služba Multi-Factor Auth Server zpracuje položky u odstraněných uživatelů odstraněných z Active Directory a odebrat hello související Multi-Factor Auth Server uživatele. |
| Vždy provádět úplnou synchronizaci |Pokud je zaškrtnuto, hello služba Multi-Factor Auth Server vždy provede úplnou synchronizaci.  Pokud není zaškrtnuto, hello služba Multi-Factor Auth Server provede přírůstkovou synchronizaci dotazováním pouze uživatelé, které se změnily.  výchozím Hello nastavení nezaškrtnuto. <br><br>Pokud není zaškrtnuto, Azure MFA Server provede přírůstkovou synchronizaci jenom v případě, že hello adresář podporuje ovládání DirSync hello a hello účet vazby toohello directory má přírůstkových dotazů DirSync tooperform oprávnění.  Pokud hello účet nemá příslušná oprávnění hello nebo hello synchronizaci podílí více domén, Azure MFA serveru provede úplnou synchronizaci. |
| Vyžadovat schválení správce, pokud bude zablokováno nebo odstraněno více než X uživatelů |Položky synchronizace může být nakonfigurované toodisable nebo odebrat uživatele, kteří již nejsou členem hello položky kontejneru nebo skupiny zabezpečení.  Pro jistotu schválení správcem, může být nutná Pokud počet hello toodisable uživatele nebo odeberte překročí prahovou hodnotu.  Pokud je toto políčko zaškrtnuté, bude při překročení určité hranice potřeba schválení.  Hello výchozí hodnota je 5 a rozsah hello je 1 too999. <br><br> Schválení je usnadněno tím, že první odesláním tooadministrators e-mailové oznámení. Hello e-mailová oznámení obsahují pokyny pro kontrolu a schvalování zakazování hello a odebírání uživatelů.  Při spuštění hello Multi-Factor Auth Server uživatelské rozhraní zobrazí výzva ke schválení. |

Hello **synchronizovat nyní** tlačítko vám umožní toorun provést úplnou synchronizaci pro hello synchronizační položky.  Při každém přidání, úpravě, odstranění nebo změně pořadí synchronizačních položek je potřeba provést úplnou synchronizaci.  Je také vyžaduje před hello služba Multi-Factor Auth AdSync je funkční, protože nastavuje počáteční bod, ze kterého hello služba bude dotazovat na přírůstkové změny hello.  Pokud byly provedeny změny položek toosynchronization, ale nebyla provedena úplná synchronizace, nebudete výzvami tooSynchronize nyní.

Hello **odebrat** tlačítko umožňuje hello správce toodelete jeden nebo víc synchronizačních položek ze seznamu položek synchronizace Multi-Factor Auth Server hello.

> [!WARNING]
> Jakmile se záznam synchronizační položky odstraní, už se nedá obnovit. Budete potřebovat tooadd hello synchronizace položky záznamů znovu Pokud jste ji odstranili omylem.

Hello synchronizační položka nebo položky synchronizace byly odebrány z Multi-Factor Auth Server.  Hello služba Multi-Factor Auth Server se již nebude moci zpracovat položky synchronizace hello.

Hello tlačítek Přesunout nahoru a přesunout dolů umožňují správci hello toochange hello pořadí položek synchronizace hello.  Hello pořadí je důležité, protože hello stejný uživatel může být členem více než jedné položky synchronizace (např. kontejneru a skupiny zabezpečení).  nastavení Hello použité toohello uživatele během synchronizace bude pocházet z první položky synchronizace hello v hello seznamu toowhich hello uživatele souvisí.  Proto potřeba seřadit hello synchronizačních položek v pořadí podle priority.

> [!TIP]
> Po odebrání synchronizačních položek by se měla provést úplná synchronizace.  Po změně pořadí synchronizačních položek by se měla provést úplná synchronizace.  Klikněte na tlačítko **synchronizovat nyní** tooperform úplnou synchronizaci.

## <a name="multi-factor-auth-servers"></a>Multi-Factor Auth Servery
Další servery Multi-Factor Auth server může být nastavit tooserve jako záložní proxy server RADIUS, proxy server LDAP, nebo pro ověřování služby IIS. Konfigurace synchronizace Hello je sdílena mezi všechny agenty hello. Pouze jeden z těchto agentů, ale může mít služba Multi-Factor Auth Server hello spuštěna. Tato karta vám umožní tooselect hello Multi-Factor Auth Server, který by měla být povolená synchronizace.

![Multi-Factor-Auth Servery](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
