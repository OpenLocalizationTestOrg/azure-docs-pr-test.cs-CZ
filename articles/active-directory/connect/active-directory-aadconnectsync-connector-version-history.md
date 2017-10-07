---
title: "aaaConnector historie verzí | Microsoft Docs"
description: "Toto téma obsahuje seznam všech verzích hello konektory pro Forefront Identity Manager (FIM) a Microsoft Identity Manager (MIM)"
services: active-directory
documentationcenter: 
author: fimguy
manager: femila
editor: 
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: fimguy
ms.openlocfilehash: 3522f17c30e46542eaa367ecdefdfd2fc47f71a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connector-version-release-history"></a>Historie vydaných verzí konektoru
Hello konektory pro Forefront Identity Manager (FIM) a Microsoft Identity Manager (MIM) jsou často aktualizuje.

> [!NOTE]
> Toto téma je k dispozici pouze v produktu FIM a MIM. Tyto konektory nejsou podporovány pro instalaci na Azure AD Connect. Vydaná konektory jsou předinstalovat službu AADConnect. při upgradu toospecified sestavení.

Toto téma uvádí všechny verze hello konektory, které byly vydány.

Související odkazy:

* [Stáhněte si nejnovější konektory](http://go.microsoft.com/fwlink/?LinkId=717495)
* [Obecné konektor LDAP](active-directory-aadconnectsync-connector-genericldap.md) referenční dokumentace
* [Obecné konektor SQL](active-directory-aadconnectsync-connector-genericsql.md) referenční dokumentace
* [Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) referenční dokumentace
* [PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) referenční dokumentace
* [Konektoru Lotus Domino](active-directory-aadconnectsync-connector-domino.md) referenční dokumentace


## <a name="116040-aadconnect-pending-release"></a>1.1.604.0 (AADConnect čeká na uvolnění)


### <a name="fixed-issues"></a>Opravené problémy:

* Obecné webové služby:
  * Byl opraven problém brání vytváří při nebyly k dispozici dva nebo víc koncových bodů protokolu SOAP projektu.
* Obecné SQL:
  * V operaci hello importu hello GSQL nebyl konvertování času správně, při uložení tooconnector místa. Hello výchozí formát data a času pro konektor místo hello GSQL byl změněn z 'rrrr MM-dd: ssZ' too'yyyy-MM-dd: ssZ '.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Opravené problémy:

* Obecné webové služby:
  * Nástroj Wsconfig Hello nepřevádět správně pole Json hello z "Ukázková žádost" pro metodu služby REST hello. Toto pole Json pro požadavek REST hello příčinou problémů s serializace.
  * Webové služby konektoru konfigurační nástroj nepodporuje názvy atributů JSON využití místa symbolů 
    * Vzor nahrazení lze přidat ručně toohello WSConfigTool.exe.config souboru, například```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```

* Lotus Notes:
  * Když hello možnost **povolit vlastní udělení licence certifikátorům pro organizace nebo organizační jednotky** je zakázána, pak hello konektoru selže při exportu (Update) po exportu hello toku všechny atributy jsou exportovaný tooDomino, ale v době hello Export KeyNotFoundException se vrátí tooSync. 
    * To je způsobeno hello přejmenovat operace selže, když se ho pokusí toochange rozlišující název (uživatelské jméno atribut) tak, že změníte jeden z atributů hello níže:  
      - Příjmení
      - FirstName
      - Indexována
      - AltFullName
      - AltFullNameLanguage
      - organizační jednotky
      - altcommonname

  * Když **povolit vlastní udělení licence certifikátorům pro organizace nebo organizační jednotky** možnost povolena, ale vyžaduje udělení licence certifikátorům jsou stále prázdné, dojde k KeyNotFoundException.

### <a name="enhancements"></a>Vylepšení:

* Obecné SQL:
  * **Scénář: přepracovali Neimplementováno:** "*" funkce
  * **Popis řešení:** změnit metodu pro [odkaz na více hodnot atributů zpracování](active-directory-aadconnectsync-connector-genericsql.md).


### <a name="fixed-issues"></a>Opravené problémy:

* Obecné webové služby:
  * Nelze importovat konfiguraci serveru, pokud je k dispozici webovou službu konektoru
  * Webová služba konektoru nepracuje s více webových služeb

* Obecné SQL:
  * Žádné typy objektů jsou uvedené pro jednu hodnotu Odkazovaný atribut
  * Rozdílový import na sledování změn strategie odstraní objekt při hodnota se odebere z tabulky s více hodnotami
  * OverflowException v konektoru GSQL s DB2 na AS / 400

Lotus:
  * Přidání možnost tooenable\disable hledání organizační jednotky před otevřením GlobalParameters stránky

## <a name="114430"></a>1.1.443.0

Vydáno: 2017 března

### <a name="enhancements"></a>Vylepšení

* Obecné SQL:</br>
  **Scénář příznaky:** je dobře známé omezení s hello konektor SQL, kde jsme pouze povolit typu odkaz tooone objektu a vyžadují křížových odkazů se členy. </br>
  **Popis řešení:** v krok zpracování hello odkazů na byly "*" zvolíte možnost, všechny kombinace typů objektů bude vrácen zpět toohello synchronizační modul.

>[!Important]
- Tím se vytvoří mnoho zástupné symboly
- Je požadováno toomake se, že názvy hello je jedinečný pro různé typy objektů.


* Obecné LDAP:</br>
 **Scénář:** na konkrétní oddíl je vybrána pouze několik kontejnerů, pak hello vyhledávání stále bude provedeno v celý oddíl. Konkrétní bude filtrováno podle synchronizační služba, ale ne podle MA, což by mohlo způsobit snížení výkonu. </br>

 **Popis řešení:** toomake kód změnit GLDAP konektor možné projít všechny kontejnery a hledání objektů v každé z nich, namísto hledání v celé oddílu hello.


* Lotus Domino:

  **Scénář:** podporu odstranění Domino pošty pro odebrání osoba během exportu. </br>
  **Řešení:** podporu odstranění konfigurovat e-mailu pro odebrání osoba během exportu.

### <a name="fixed-issues"></a>Opravené problémy:
* Obecné webové služby:
 * Při změně adresy URL služby hello ve výchozím SAP wsconfig projekty prostřednictvím webové služby konfigurační nástroj pak se stane hello následující chyba: Nelze najít část cesty hello

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Obecné LDAP:
 * Konektor GLDAP nejsou vidět všechny atributy ve službě AD LDS
 * Průvodce zalomení při zjištění žádné atributy UPN ze schématu adresáře LDAP hello
 * Rozdílová importy selhání s chybami zjišťování nejsou k dispozici během úplný import, pokud není vybraná atribut "objectclass"
 * Na stránce konfigurace "Konfigurace oddílů a hierarchií", nezobrazí všechny objekty, který typ je rovna toohello oddílu pro nové servery v hello obecné  
LDAP MA. Ukázalo se pouze objekty z oddílu RootDSE.


* Obecné SQL:
 * Opravte pro obecné SQL vodoznak chyb vícehodnotový atribut není importován rozdílový Import
 * Při exportu deleted\added hodnoty atributu s více hodnotami, nejsou deleted\added ve zdroji dat.  


* Lotus Notes:
 * Konkrétní pole, které "Úplný název" se zobrazí v úložišti metaverse hello správně ale při exportu tooNotes hello hodnotu pro atribut hello má hodnotu Null nebo prázdný.
 * Opravit duplicitní Certifier došlo k chybě
 * Pokud je vybrána hello objektu bez všechna data na hello konektoru Lotus Domino s jinými objekty potom jsme chyba se zobrazí hello zjišťování při provádění úplné importu.
 * Pokud se rozdílový Import se službou na hello konektoru Lotus Domino na konci hello této spuštění, hello Microsoft.IdentityManagement.MA.LotusDomino.Service.exe někdy vrátí chybu aplikace.
 * Skupiny členství celkové funguje bez problémů a zůstane zachované, s výjimkou při spuštění hello export tootry tooremove uživatele z členství se zobrazí jako úspěšně dokončený s aktualizací, ale není získat z členství v Lotus Notes ve skutečnosti odebrán uživatel hello.
 * Režim toochoose příležitost exportu jako "Připojení položka v dolní části" byl přidán v konfiguraci grafického uživatelského rozhraní Lotus MA tooappend nové položky v dolní části během exportu hello více hodnot atributů.
 * Konektor přidá hello potřebné logiky toodelete hello soubor z hello složku pošty a ID trezoru.
 * Odstraňte členství nefunguje mezi NAB člen.
 * Hodnoty by měla být úspěšně odstraněna z vícehodnotového atributu

## <a name="111170"></a>1.1.117.0
Vydáno: března 2016

**Nový konektor**  
Počáteční verze hello [obecné konektor SQL](active-directory-aadconnectsync-connector-genericsql.md).

**Nové funkce:**

* Generický konektor LDAP:
  * Přidaná podpora pro Rozdílový import s Isode.
* Konektor webových služeb:
  * Aktualizované hello csEntryChangeResult aktivity a setImportErrorCode aktivity tooallow objekt úrovně chyby toobe vrácený back toohello synchronizační modul.
  * Aktualizované hello SAP6 a SAP6User šablony toouse hello nový objekt úrovně Chyba funkce.
* Konektoru Lotus Domino:
  * Pro export budete potřebovat jeden certifier na adresář. Můžete teď hello použijte stejné heslo pro všechny udělení licence certifikátorům toomake hello správu jednodušší.

**Opravené problémy:**

* Generický konektor LDAP:
  * Pro IBM Tivoli DS nebyla zjištěna správně některé atributy typu odkaz.
  * Otevřete protokol během Rozdílový import bylo zkráceno prázdné znaky na hello začátku a konci řetězce.
  * Novell a NetIQ export, přesunout objekt mezi organizační jednotky nebo kontejnery a na hello stejný čas přejmenovat hello objektu se nezdařilo.
* Konektor webových služeb:
  * Pokud webová služba hello měli víc koncových bodů pro stejnou vazbu, pak hello konektor není zjistit správně těchto koncových bodů.
* Konektoru Lotus Domino:
  * Exportu hello fullName atribut tooa e-mailu v databázi nebyla úspěšná.
  * Export, který jak přidat nebo odebrat člena ze skupiny jen exportovaný hello přidat členy.
  * Pokud poznámky k dokumentu je neplatný (hello atribut isValid nastavit toofalse), pak hello konektoru selže.

## <a name="older-releases"></a>Starší verze
Před. března 2016 byly vydané hello konektory jako témata týkající se podpory.

**Obecné LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, září 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, března 2015
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, leden 2015
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, září 2014
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, března 2014

**Webovým službám**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, září 2014

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, září 2014

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, září 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, března 2015
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, srpen 2014
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003 února 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, říjen 2013
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, srpen 2013

## <a name="next-steps"></a>Další kroky
Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
