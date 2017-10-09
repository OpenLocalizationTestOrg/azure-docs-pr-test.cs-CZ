---
title: aaaGeneric konektor LDAP | Microsoft Docs
description: "Tento článek popisuje, jak tooconfigure Microsoft obecné konektor LDAP."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 25031b4da196bd073902b04b0705762bfa0118b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="generic-ldap-connector-technical-reference"></a>Technické informace o obecné konektor LDAP
Tento článek popisuje hello obecné konektor LDAP. Hello článek se týká toohello následující produkty:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manageru 2010 R2 (FIM2010R2)
  * Musíte použít opravu hotfix 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

Pro MIM2016 a FIM2010R2, je k dispozici ke stažení z hello hello konektor [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Při odkazování dokumenty RFC tooIETF, tento dokument je formátu hello (RFC [RFC číslo] nebo [oddílu v dokumentu RFC]), například (RFC 4512/4.3).
Další informace najdete v http://tools.ietf.org/html/rfc4500 (je třeba tooreplace 4500 RFC číslem správné hello).

## <a name="overview-of-hello-generic-ldap-connector"></a>Přehled hello obecné konektor LDAP
Hello obecné konektor LDAP umožňuje vám toointegrate hello synchronizační služby se serverem LDAP v3.

Některé operace a prvky schématu, například těch, které potřeby tooperform Rozdílový import, nebyly zadány v dokumentech hello IETF RFC. Pro tyto operace jsou podporovány pouze adresáře LDAP explicitně určena.

Z hlediska vysoké úrovně podporuje hello aktuální verzi konektoru hello hello následující funkce:

| Funkce | Podpora |
| --- | --- |
| Připojeného zdroje dat |Hello konektor je podporována u všech serverů LDAP v3 (specifikaci RFC 4510). Byl testován s hello následující: <li>Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Microsoft Active Directory globální katalog (AD GC)</li><li>389 adresářový Server</li><li>Apache adresářový Server</li><li>IBM Tivoli DS</li><li>Isode adresáře</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Otevřete DJ</li><li>Otevřete DS</li><li>Otevřete LDAP (openldap.org)</li><li>Oracle (dříve Sun) Directory Server Enterprise Edition</li><li>Virtuální adresář serveru RadiantOne (VDS)</li><li>Jeden Sun Directory Server</li>**Významné adresářů není podporována:** <li>Microsoft Active Directory Domain Services (AD DS) [použití hello integrovaného konektoru služby Active Directory místo]</li><li>Oracle Internet adresáře (OID)</li> |
| Scénáře |<li>Správa životního cyklu objektu</li><li>Správa skupin</li><li>Správa hesel</li> |
| Operace |Hello následující operace jsou podporovány na všechny adresáře LDAP: <li>Úplný Import</li><li>Export</li>Hello následující operace jsou podporovány pouze na zadaných adresářích:<li>Rozdílový import</li><li>Nastavení hesla, změnit heslo</li> |
| Schéma |<li>Schéma je zjištěna ze schématu LDAP hello (RFC3673 a RFC4512 nebo 4.2)</li><li>Podporuje strukturální třídy, aux třídy a třída objektu extensibleObject (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Podpora správy Rozdílový import a heslo
Rozdílový import a Správa hesel podporováno adresáře:

* Microsoft Active Directory Lightweight Directory Services (AD LDS)
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavení hesla
* Microsoft Active Directory globální katalog (AD GC)
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavení hesla
* 389 adresářový Server
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Apache adresářový Server
  * Vzhledem k tomu, že tento adresář nemá trvalé změny protokolu nepodporuje Rozdílový import
  * Podporuje nastavení hesla
* IBM Tivoli DS
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Isode adresáře
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Novell eDirectory a NetIQ eDirectory
  * Podporuje operace přidat, aktualizace a přejmenování pro Rozdílový import
  * Nepodporuje operace Delete pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Otevřete DJ
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Otevřete DS
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Otevřete LDAP (openldap.org)
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavení hesla
  * Nepodporuje změnu hesla
* Oracle (dříve Sun) Directory Server Enterprise Edition
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Virtuální adresář serveru RadiantOne (VDS)
  * Musí používat verzi 7.1.1 nebo vyšší
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Jeden Sun Directory Server
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo

### <a name="prerequisites"></a>Požadavky
Než použijete hello konektor, zkontrolujte, zda že máte na serveru pro synchronizaci hello hello následující:

* 4.5.2 rozhraní Microsoft .NET Framework nebo novější

### <a name="detecting-hello-ldap-server"></a>Zjišťování serveru LDAP hello
Hello konektor závisí na různých technik toodetect a identifikovat hello LDAP server. Konektor používá Hello hello DSE kořenové, název dodavatele a verzi, a jeho kontroluje hello toofind jedinečný objekty a atributy schématu známé tooexist v určitých serverů LDAP. Tato data, pokud nalezen, je použít toopre-naplnit hello možnosti konfigurace v hello konektor.

### <a name="connected-data-source-permissions"></a>Oprávnění připojených zdrojů dat
tooperform import a export operací na hello objekty v hello připojený adresář, hello konektor účet musí mít dostatečná oprávnění. konektor Hello potřebuje zápisu možné tooexport toobe oprávnění a možnost tooimport toobe oprávnění číst. Konfigurace oprávnění se provádí v rámci hello správu zkušenosti hello cílový adresář sám sebe.

### <a name="ports-and-protocols"></a>Porty a protokoly
konektor Hello používá číslo portu hello zadaný v konfiguraci hello, který ve výchozím nastavení je 389 pro LDAP a 636 pro LDAPS.

Pro LDAPS musí používat protokol SSL 3.0 nebo TLS. Protokol SSL 2.0 není podporována a nelze aktivovat.

### <a name="required-controls-and-features"></a>Požadovaný ovládací prvky a funkce
Hello následující LDAP ovládací prvky na funkce musí být k dispozici na serveru LDAP hello hello konektor toowork správně:  
`1.3.6.1.4.1.4203.1.5.3`Filtry true nebo False

Filtr hodnotu True nebo False Hello není hlášena často podporuje adresáře LDAP a může zobrazovat ve hello **globální stránky** pod **povinné funkce nebyla nalezena**. Je použité toocreate **nebo** filtry v dotazech LDAP, například při importu více typů objektu. Pokud importujete více než jeden typ objektu, serveru LDAP podporuje tuto funkci.

Pokud používáte adresář, kde je jedinečný identifikátor hello ukotvení hello následující musí být také k dispozici (Další informace najdete v tématu hello [konfigurace kotvy](#configure-anchors) část):  
`1.3.6.1.4.1.4203.1.5.1`Všechny provozní atributy

Pokud má hello adresář více objektů, než co můžete začlenit v jednom adresáři toohello volání, doporučujeme toouse stránkování. Pro toowork stránkování budete potřebovat hello následující možnosti:

**Možnost 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Možnost 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Pokud obě možnosti jsou povolené v konfiguraci konektoru hello, použije se pagedResultsControl.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl používá jenom s hello USNChanged Rozdílový import metoda toobe možné toosee odstranit objekty.

konektor Hello pokusí toodetect hello možnosti nachází na serveru hello. Když hello možnosti nelze rozpoznat, upozornění se nachází na stránce globální hello ve vlastnostech konektoru hello. Ne všechny servery LDAP nachází všechny ovládací prvky nebo funkce se podporují a i v případě, že toto upozornění je k dispozici, hello konektoru může fungovat bez problémů.

### <a name="delta-import"></a>Rozdílový import
Rozdílový import je k dispozici, pouze pokud byla zjištěna podpora adresáře. Hello následující metody se aktuálně používají:

* LDAP Accesslog. V tématu [http://www.openldap.org/doc/admin24/overlays.html#Access protokolování](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
* Protokol LDAP změn. V tématu [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Časové razítko. Novell/NetIQ eDirectory hello konektor používá poslední datum a čas tooget vytvořit a aktualizovat objekty. Novell/NetIQ eDirectory neposkytuje ekvivalentní znamená tooretrieve odstranit objekty. Tuto možnost lze také Pokud žádnou jinou metodu Rozdílový import je aktivní na serveru LDAP hello. Tato možnost není možné tooimport odstranit objekty.
* USNChanged. Přejděte na téma: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Nepodporuje se
Následující funkce LDAP Hello nejsou podporovány:

* Referenční seznamy LDAP mezi servery (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Vytvořit nový konektor
tooCreate generický konektor LDAP v **synchronizační služba** vyberte **agenta pro správu** a **vytvořit**. Vyberte hello **obecné LDAP (Microsoft)** konektor.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Připojení
Na stránce hello připojení je nutné zadat hello hostitelů, Port a vazba informace. V závislosti na tom, které je vazba vybrané, další informace může být potřeba zadat ve hello následující části.

![Připojení](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

* Hello časový limit připojení nastavení slouží pouze pro hello prvního připojení toohello serveru při zjišťování hello schématu.
* Pokud vazba je anonymní, pak ani uživatelského jména a hesla ani certifikát se používají.
* Pro další vazby, zadejte informace o buď v uživatelské jméno / heslo nebo vyberte certifikát.
* Pokud používáte tooauthenticate protokolu Kerberos, zadejte také hello sféry nebo domény hello uživatele.

Hello **atribut aliasy** textového pole se používá pro atributy definované ve schématu hello se syntaxí RFC4522. Tyto atributy nebyl nalezen při rozpoznávání schématu a hello konektor musí pomoct tooidentify těmito atributy. Například následující musí být zadaný v hello atribut aliasy pole toocorrectly hello Identifikujte hello userCertificate atribut jako binární atribut:

`userCertificate;binary`

Hello následuje příklad, jak může vypadat tuto konfiguraci:

![Připojení](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Vyberte hello **zahrnout provozní atributy do schématu** tooalso políčko zahrnout atributy vytvořená serverem hello. Mezi ně patří atributy, jako když hello objekt byl vytvořen a čas poslední aktualizace.

Vyberte **zahrnout extensible atributy do schématu** budou použita rozšiřitelné objekty (RFC4512/4.3) a povolení této možnosti umožňuje každý atribut toobe použít na všechny objektu. Tato možnost vám hello schématu velké, pokud připojený adresář hello používá toto doporučení hello funkce je tookeep hello možnost zrušit.

### <a name="global-parameters"></a>Globální parametry
Na stránce globální parametry hello nakonfigurujete protokol změn delta toohello hello rozlišující název a další funkce LDAP. stránku Hello je již nastavena hello informace poskytované serverem LDAP hello.

![Připojení](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

horní části Hello zobrazí informace poskytované hello samotného serveru, jako je například název hello serveru hello. Hello konektor také ověří, zda hello povinné ovládací prvky jsou k dispozici v hello DSE kořenové. Pokud tyto ovládací prvky nejsou na seznamu, se zobrazí upozornění. Některé adresáře LDAP, neuvádějte všem funkcím v hello DSE kořenové a je možné, že konektor funguje bez problémů text hello, i když je k dispozici upozornění.

Hello **podporované ovládací prvky** zaškrtávací políčka kontrolovat hello chování pro určité operace:

* Pomocí stromové struktury odstranit vybrané, se odstraní hierarchie s jedno volání LDAP. S stromu odstranit zrušit nemá hello konektor odstranit rekurzivní v případě potřeby.
* S stránkových výsledků vybraného nemá hello konektor stránkové import s hello velikost zadaná v hello spustit kroky.
* Hello VLVControl a SortControl je model alternativní toohello pagedResultsControl tooread data z adresáře LDAP hello.
* Pokud jsou všechny tři možnosti (pagedResultsControl, VLVControl a SortControl) nezaškrtnuté hello pak konektor importuje všech objektů v rámci jedné operace, které může selhat, pokud je to velké adresář.
* ShowDeletedControl se používá pouze v případě metody import Delta hello USNChanged.

Protokol změn Hello rozlišující název je názvový kontext hello používá protokol změn hello rozdílů, například **cn = protokol změn**. Tato hodnota musí být zadán toobe možné toodo Rozdílový import.

Hello následuje seznam protokol změn výchozí DNs:

| Adresář | Protokol změn rozdílů |
| --- | --- |
| Uvolňování paměti Microsoft AD LDS a AD |Automaticky zjištěna. USNChanged. |
| Apache adresářový Server |Není k dispozici. |
| Directory 389 |Protokol změn. Výchozí hodnota toouse: **cn = protokol změn** |
| IBM Tivoli DS |Protokol změn. Výchozí hodnota toouse: **cn = protokol změn** |
| Isode adresáře |Protokol změn. Výchozí hodnota toouse: **cn = protokol změn** |
| Novell/NetIQ eDirectory |Není k dispozici. Časové razítko. Hello konektor používá poslední aktualizované datum a čas tooget přidají a záznamy. |
| Otevřete DJ/DS |Protokol změn.  Výchozí hodnota toouse: **cn = protokol změn** |
| Otevřete LDAP |Přístup k protokolu. Výchozí hodnota toouse: **cn = accesslog** |
| Oracle DSEE |Protokol změn. Výchozí hodnota toouse: **cn = protokol změn** |
| RadiantOne VDS |Virtuální adresář. Závisí na tooVDS directory připojené hello. |
| Jeden Sun Directory Server |Protokol změn. Výchozí hodnota toouse: **cn = protokol změn** |

atribut password Hello je název hello hello atribut hello konektor by měl používat heslo hello tooset v změny hesla a heslo množinové operace.
Tato hodnota je ve výchozím nastavení obsahuje příliš**userPassword** však lze změnit v případě potřeby pro konkrétní systém LDAP.

V seznamu hello další oddíly je možné tooadd další obory názvů, automaticky zjištěna. Například můžete toto nastavení použít, pokud několik serverů, které tvoří logickou cluster, který by měl všechny importovat v hello stejnou dobu. Stejně jako služby Active Directory může mít několik domén v jedné doménové struktuře, ale všechny domény sdílet jedno schéma, hello stejné můžete simulated zadáním hello další obory názvů v tomto poli. Každý obor názvů můžete importovat z různých serverů a je nakonfigurované na stránce konfigurace oddílů a hierarchií hello. Pomocí kombinace kláves Ctrl + Enter tooget nový řádek.

### <a name="configure-provisioning-hierarchy"></a>Konfigurace hierarchie zřizování
Tato stránka je použité toomap hello rozlišující název součásti, například organizační jednotku, toohello typ objektu, který by měl být zřízený, například organizationalUnit.

![Zřizování hierarchie](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Konfigurace hierarchie zřizování, můžete konfigurovat konektor tooautomatically hello vytvořit strukturu v případě potřeby. Například, pokud je obor názvů dc = contoso, řadič domény com a nový objekt cn = Jan, ou = = Seattle, c = US, řadič domény = contoso, dc = com je zřízený a pak hello konektor můžete vytvořit objekt typu země pro USA a organizationalUnit pro Seattle, pokud těch, které ještě nejsou existuje v adresáři hello.

### <a name="configure-partitions-and-hierarchies"></a>Konfigurace oddílů a hierarchií
Na stránce hello oddílů a hierarchií, vyberte všechny obory názvů s objekty plánujete tooimport a export.

![Oddíly](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Pro každý obor názvů je také možné tooconfigure nastavení připojení, které by se mělo přepsat hello hodnoty zadané na obrazovce připojení hello. Pokud tyto hodnoty jsou ponechána tootheir výchozí prázdnou hodnotu, se používá hello informace z úvodní obrazovka připojení.

Je také možné tooselect které kontejnery a organizační jednotky hello by měl import a export na konektoru.

Při prohledávání probíhá přes všechny kontejnery v oddílu hello. V případech, kde je velké množství kontejnerů toto chování způsobí snížení tooperformance.

>[!NOTE]
Počínaje hello března 2017 aktualizace toohello obecné LDAP konektor hledání omezené v kontejnerech hello vybrané tooonly oboru. To lze provést výběrem hello políčko "vyhledávání pouze ve vybrané kontejnery, jak ukazuje následující obrázek hello.

![Vyhledávat pouze vybrané kontejnerů](./media/active-directory-aadconnectsync-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Konfigurace ukotvení
Tato stránka nemá vždy hodnotu předkonfigurované a nedá se změnit. Pokud byla zjištěna hello serveru dodavatele, může hello ukotvení naplněno s atributem neměnné pro příklad hello identifikátor GUID pro objekt. Pokud nebyl nalezen nebo je známý toonot neměnné atribut a potom hello konektor jako kotvu hello používá rozlišující název (rozlišující název).

![ukotvení](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)


Hello následuje seznam serverů LDAP a ukotvení hello používá:

| Adresář | Atribut kotvy |
| --- | --- |
| Uvolňování paměti Microsoft AD LDS a AD |objectGUID |
| 389 adresářový Server |rozlišující název |
| Apache adresáře |rozlišující název |
| IBM Tivoli DS |rozlišující název |
| Isode adresáře |rozlišující název |
| Novell/NetIQ eDirectory |IDENTIFIKÁTOR GUID |
| Otevřete DJ/DS |rozlišující název |
| Otevřete LDAP |rozlišující název |
| Oracle ODSEE |rozlišující název |
| RadiantOne VDS |rozlišující název |
| Jeden Sun Directory Server |rozlišující název |

## <a name="other-notes"></a>Další poznámky
Tato část obsahuje informace o aspektů, které jsou specifické toothis konektoru nebo z jiných důvodů jsou důležité tooknow.

### <a name="delta-import"></a>Rozdílový import
vodoznak Hello rozdílů v otevřené LDAP je datum a čas UTC. Z tohoto důvodu musí být synchronizovány hello hodiny mezi synchronizační služba FIM a hello otevřete LDAP. Pokud ne, může být některé položky v hello rozdílové změny protokolu vynechán.

Pro Novell eDirectory není rozdílový import hello zjišťování žádné odstranění objektu. Z tohoto důvodu je nutné toorun úplnou pravidelný import toofind odstranit objekty.

Adresáře s rozdílový změn protokolu, který je založen na datum a čas, se důrazně doporučuje toorun úplný import v pravidelných dobu. Tento proces umožňuje hello synchronizační modul toofind a rozdíly mezi hello LDAP server a co je aktuálně v prostoru konektoru hello.

## <a name="troubleshooting"></a>Řešení potíží
* Informace o tom, jak protokolování tooenable tootroubleshoot hello konektoru najdete v tématu hello [jak tooEnable trasování ETW pro konektory](http://go.microsoft.com/fwlink/?LinkId=335731).
