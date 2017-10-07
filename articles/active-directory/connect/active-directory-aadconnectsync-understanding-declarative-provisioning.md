---
title: "Azure AD Connect: Principy deklarativní zřizování | Microsoft Docs"
description: "Vysvětluje hello deklarativní zřizování konfigurační model v Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: cfbb870d-be7d-47b3-ba01-9e78121f0067
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: f11e078f0aafacf94d69f0726ae41629a8470336
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Synchronizace Azure AD Connect: Principy deklarativní zřizování
Toto téma vysvětluje hello konfigurační model v Azure AD Connect. Hello model se nazývá deklarativní zřizování a umožňuje toomake snadno změně konfigurace. Celou řadu věcí, které jsou popsané v tomto tématu jsou rozšířené a není potřeba pro většinu scénářů zákazníka.

## <a name="overview"></a>Přehled
Deklarativní zřizování je zpracování objektů brzo ze zdrojového připojení adresáře a určuje, jak hello objektů a atributů by měl být transformovat z cíl tooa zdroje. Objekt je zpracovat v kanálu synchronizace a hello kanálu je hello stejné pro příchozí a odchozí pravidla. Příchozí pravidlo je úložiště metaverse konektoru místo toohello a odchozí pravidlo je z prostoru konektoru tooa hello úložiště metaverse.

![Synchronizace kanálu](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

Hello kanálu má několik různých modulů. Každé z nich je zodpovědná za jeden koncept objekt synchronizace.

![Synchronizace kanálu](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

* Zdroje, hello zdrojového objektu
* [Obor](#scope), vyhledá všechna pravidla synchronizace, které jsou v oboru
* [Připojení k](#join), určuje vztah mezi prostoru konektoru a úložiště metaverse
* [Transformace](#transform), jak by měl být atributy transformovat vypočte a toku
* [Priorita](#precedence), vyřeší konfliktní atribut příspěvky
* Cíl, hello cílový objekt

## <a name="scope"></a>Rozsah
modul oboru Hello je vyhodnocení objekt a určuje hello pravidla, která jsou v oboru a by měl být součástí hello zpracování. V závislosti na hello hodnot atributů hello objekt jsou různé synchronizační pravidla vyhodnotí toobe v oboru. Zakázaný uživatel se žádné poštovní schránky Exchange například mít různá pravidla než v případě povoleného uživatele s poštovní schránku.  
![Rozsah](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

obor Hello je definován jako skupiny a klauzule. klauzule Hello jsou uvnitř skupiny. Logické a se používá mezi všechny klauzule ve skupině. Například (oddělení IT a země = = Dánsko). Logické nebo se používá mezi skupinami.

![Rozsah](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Hello oboru na tomto obrázku byste si měli přečíst jako (oddělení IT a země = = Dánsko) nebo (země = Švédsko). Pokud skupina 1 nebo skupinu 2 vyhodnotí tootrue, hello pravidlo je v oboru.

Hello oboru modul podporuje hello následující operace.

| Operace | Popis |
| --- | --- |
| ROVNÁ NOTEQUAL |Porovnat řetězec, která vyhodnotí, pokud je hodnota rovna toohello hodnotu v atributu hello. Více hodnot atributů najdete v části ISIN a ISNOTIN. |
| LESSTHAN LESSTHAN_OR_EQUAL |Porovnat řetězec, který vyhodnotí, jestli je hodnota menší než hodnota hello hodnoty v atributu hello. |
| OBSAHUJE, NOTCONTAINS |Porovnat řetězec, která vyhodnotí, pokud hodnota může být uvnitř hodnotu v atributu hello nalezena někde. |
| STARTSWITH, NOTSTARTSWITH |Porovnat řetězec, který se vyhodnotí, jestli je hodnota v hello začátku hello hodnotu v atributu hello. |
| ENDSWITH, NOTENDSWITH |Porovnat řetězec, který vyhodnotí, jestli je hodnota v hello konec hello hodnoty v atributu hello. |
| GREATERTHAN GREATERTHAN_OR_EQUAL |Porovnat řetězec, která vyhodnotí, pokud je hodnota vyšší než hodnota hello v atributu hello. |
| ISNULL ISNOTNULL |Vyhodnotí chybí-li hello atribut z objektu hello. Pokud atribut hello není k dispozici a proto hodnotu null, hello pravidlo je v oboru. |
| ISIN ISNOTIN |Vyhodnotí, pokud hodnota hello nachází v hello definován atribut. Tato operace je více hodnot varianta hello je ROVNO a NOTEQUAL. Hello atribut by měl toobe více hodnot atributů a pokud hodnota hello naleznete v některé z hodnot atributů hello, pak hello pravidlo je v oboru. |
| ISBITSET ISNOTBITSET |Vyhodnotí, pokud je konkrétní bit nastavený. Například může být použité tooevaluate hello bitů userAccountControl toosee Pokud uživatel povolený nebo zakázaný. |
| ISMEMBEROF ISNOTMEMBEROF |Hello hodnota by měla obsahovat skupinu rozlišující název tooa v prostoru konektoru hello. Pokud se objekt hello členem zadané skupiny hello, hello pravidlo je v oboru. |

## <a name="join"></a>Spojit
Hello připojení k modulu v kanálu synchronizace hello je zodpovědná za vyhledávání hello vztah mezi objektem hello ve zdroji hello a objekt v cílové hello. Tento vztah na příchozí pravidlo, bude objekt v prostoru konektoru, hledání tooan objekt relace v úložišti metaverse hello.  
![Připojení mezi cs a mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
cílem Hello je toosee, pokud objekt již existuje v úložišti metaverse hello, vytvořené jiný konektor, by měl přidružen. Například prostředek účet doménové struktury hello uživatele z doménové struktury hello účet by měl být připojený hello uživatele z doménové struktury prostředku hello.

Spojení se používají převážně na příchozích pravidel toojoin konektoru místo objekty společně toohello stejný objekt úložiště metaverse.

Hello spojení jsou definovány jako jednu nebo více skupin. Uvnitř skupiny máte klauzule. Logické a se používá mezi všechny klauzule ve skupině. Logické nebo se používá mezi skupinami. skupiny Hello se zpracovávají v pořadí od nejvyšší toobottom. Když jedna skupina má v cílové hello najde přesně jednu shodu s objektem, se vyhodnocují žádná další pravidla spojení. Pokud v počtu nula či více, než jeden objekt, pokračuje zpracování toohello další skupiny pravidel. Z tohoto důvodu by se vytvořit pravidla hello v hello pořadí nejvíce explicitní první a více přibližné na konci hello.  
![Připojení k definici](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
spojení Hello na tomto obrázku jsou zpracovávány z horní toobottom. První kanál synchronizace hello se zobrazí, pokud je shoda s employeeID. V opačném případě druhého pravidla text hello se zobrazí, pokud název účtu hello lze použít toojoin hello objekty společně. Pokud není nalezena shoda buď, je pravidlo třetí a finální hello více přibližné shody pomocí hello jméno uživatele.

Pokud byly vyhodnoceny všechna pravidla spojení a je přesně jednu shodu, hello **typu propojení** na hello **popis** stránky se používá. Pokud je tato možnost nastavená příliš**zřídit**, pak se vytvoří nový objekt v cílové hello.  
![Zřízení nebo připojení k](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Objekt by měl mít pouze jedno pravidlo jedním synchronizačním s pravidly spojení v oboru. Pokud existují více pravidel synchronizace kterých byla definována spojení, dojde k chybě. Priorita není použité tooresolve spojení konflikty. Objekt musí mít připojení k pravidlo v oboru pro atributy tooflow s hello stejné příchozí nebo odchozí směr. Pokud potřebujete tooflow atributy příchozí a odchozí toohello stejný objekt musí mít příchozí i pravidlo odchozí synchronizace s spojení.

Odchozí spojení je speciální chování, když se ho pokusí tooprovision prostoru konektoru cíl tooa k objektu. Hello rozlišující název atributu je zkuste použít toofirst zpětného spojení. Pokud již existuje objekt v prostoru konektoru hello cíl s hello stejné rozlišující název, hello, které jsou připojené objekty.

připojení k modulu Hello je Vyhodnocená jenom po při přechodu do oboru dojde nové pravidlo synchronizace. Pokud objekt má připojený, není to odpojování i v případě, že již nesplňuje kritéria spojení hello. Pokud chcete toodisjoin objektu, se musí hello synchronizační pravidlo, které připojený hello objekty dostala mimo rozsah.

### <a name="metaverse-delete"></a>Odstranění úložiště Metaverse
Zůstane, pokud je v oboru s jedno pravidlo synchronizace objektu úložiště metaverse **typu propojení** nastavit příliš**zřídit** nebo **StickyJoin**. StickyJoin se používá, pokud konektor není povoleno tooprovision nový objekt úložiště metaverse toohello, ale pokud se má propojit, je třeba jej odstranit v hello zdroji předtím, než je odstraněn objekt úložiště metaverse hello.

Při odstranění objektu úložiště metaverse všechny objekty přidružené pravidlo odchozí synchronizace označen pro **zřídit** jsou označena pro odstranění.

## <a name="transformations"></a>Transformace
Transformace Hello jsou použité toodefine, jak by měla toku atributů z hello zdroj toohello cíle. Hello toky může mít jednu z následujících hello **toku typy**: přímo, konstanta nebo výraz. Hodnota atributu jako tok přímé tok,-je bez další transformací. Nastaví hello konstantní hodnotu zadané hodnotě. Výraz používá hello deklarativní zřizování výrazu jazyka tooexpress jak by měla být hello transformace. Hello podrobnosti pro jazyk výrazů hello lze nalézt v hello [pochopení deklarativní zřizování jazyk výrazů](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) tématu.

![Zřízení nebo připojení k](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

Hello **použít jednou** políčko definuje tento hello atribut by měl lze nastavit pouze při vytvoření objektu hello. Tato konfigurace může být například použít tooset počátečního hesla pro nový objekt uživatele.

### <a name="merging-attribute-values"></a>Slučování hodnoty atributu
V toky atributů hello je nastavení toodetermine Pokud více hodnot atributů by měly být sloučeny z několika různých konektorů. Hello výchozí hodnota je **aktualizace**, což znamená, že hello synchronizační pravidlo s nejvyšší prioritou by měla win.

![Merge – typy](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

K dispozici je také **sloučení** a **MergeCaseInsensitive**. Tyto možnosti Povolit toomerge hodnoty z různých zdrojů. Například lze atribut člen nebo proxyAddresses hello použité toomerge z několika různých doménových struktur. Pokud použijete tuto možnost, všechny synchronizace pravidla v oboru pro objekt musí používat hello stejný typ sloučení. Nelze definovat **aktualizace** z jeden konektor a **sloučení** z jiné. Pokud se pokusíte, obdržíte chybu.

Hello rozdíl mezi **sloučení** a **MergeCaseInsensitive** je, jak tooprocess duplicitní hodnoty atributu. synchronizační modul Hello zajistí, že duplicitní hodnoty nejsou vloženy do atribut target hello. S **MergeCaseInsensitive**, duplicitní hodnoty s pouze rozdíly v případě, že nebudete toobe přítomen. Například byste neměli vidět obě "SMTP:bob@contoso.com"a"smtp:bob@contoso.com" v atribut target hello. **Sloučení** je jenom prohlížení hello přesné hodnoty a více hodnot tam, kde existuje jenom rozdíl v případě může být k dispozici.

Hello možnost **nahradit** je stejný jako hello **aktualizace**, ale se nepoužívá.

### <a name="control-hello-attribute-flow-process"></a>Proces řízení hello atribut toku
Když více pravidel příchozí synchronizace jsou nakonfigurované toocontribute toohello stejný atribut úložiště metaverse a pak přednost je použité toodetermine Vítěz hello. Pravidlo synchronizace Hello s nejvyšší prioritou (nejnižší číselnou hodnotu) bude toocontribute hello hodnotu. Hello stejné se stane, že pro odchozí pravidla. Pravidlo synchronizace Hello s nejvyšší prioritou wins a přispívat hello hodnota toohello připojený adresář.

V některých případech místo přispívat hodnotu, pravidlo synchronizace hello měli určit chování ostatní pravidla. Existují některé speciální literály použít pro tento případ.

Pro příchozí pravidla synchronizace hello literálu **NULL** lze použít tooindicate, že tok hello má toocontribute žádná hodnota. Jiné pravidlo s nižší prioritou můžete přispívat hodnotu. Pokud žádné pravidlo podílí hodnotu, pak se odebere atribut úložiště metaverse hello. Odchozí pravidla Pokud **NULL** je konečná hodnota hello po zpracování všech pravidel synchronizace, pak hodnota hello odebere v hello připojený adresář.

Hello literálu **AuthoritativeNull** je příliš podobné**NULL** s hello rozdílem, že žádná nižší prioritu pravidla můžete přispívat hodnotu.

Tok atributů použít také **IgnoreThisFlow**. Je podobné tooNULL v hello smyslu, že ukazuje nic toocontribute. Hello rozdílem je, se neodebere již existující hodnoty v cílové hello. Je jako toku atributů hello nebyla nikdy existuje.

Zde naleznete příklad:

V *Out tooAD - Exchange uživatele hybridní* naleznete hello následující postup:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Tento výraz byste si měli přečíst jako: Pokud je poštovní schránka hello uživatele ve službě Azure AD, toku atributů hello z tooAD Azure AD. Pokud ne, není toku cokoli back tooActive adresáře. V takovém případě ponechá hello existující hodnotu v AD.

### <a name="importedvalue"></a>ImportedValue
vzhledem k tomu, že název atributu hello musí být uzavřena v uvozovkách, nikoli hranaté závorky se liší od dalších funkcí Funkce Hello ImportedValue:  
`ImportedValue("proxyAddresses")`.

Obvykle během synchronizace používá atribut hello očekávaná hodnota, i když nebyla dosud exportovat nebo došlo k chybě při exportu ("top věž hello"). Příchozí synchronizace se předpokládá, že atribut, který ještě nedosáhla připojený adresář nakonec nedosáhne. V některých případech je důležité tooonly synchronizovat hodnotu, která byla potvrzena hello připojený adresář ("hologram a rozdílový import věž").

Příklad této funkce lze nalézt v hello out-of-box synchronizační pravidlo *v ze služby Active Directory – běžné uživatele ze serveru Exchange*. V hybridní Exchange hello přidané systémem Exchange online pouze synchronizovat při bylo potvrzeno, že byl úspěšně exportován hello hodnotu:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Priorita
Při pokusu o několik pravidel synchronizace toocontribute hello stejný cíl toohello hodnotu atributu, hello přednost hodnota použitých toodetermine hello Vítěz. pravidlo Hello s nejvyšší prioritou, nejnižší číselnou hodnotu, bude atribut hello toocontribute v konfliktu.

![Merge – typy](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Toto uspořádání může být použité toodefine přesnější atribut toky pro malou podmnožinu objektů. Například, ujistěte se, který atributy z aktivovaný účet hello out--pole pravidel (**uživatele AccountEnabled**) mají přednost před z jiné účty.

Mezi konektory lze definovat prioritu. Nejprve umožňující konektory s lepší toocontribute hodnotami data.

### <a name="multiple-objects-from-hello-same-connector-space"></a>Více objektů z hello stejné prostoru konektoru
Pokud máte několik objektů v hello stejné konektoru místo toohello připojený k objektu stejné úložiště metaverse, musí být upravena prioritu. Pokud několik objekty jsou v oboru z hello stejné pravidlo synchronizace a potom hello synchronizační modul není možné toodetermine přednost. Ho je nejednoznačný, které zdrojový objekt musí přispívat hello hodnota toohello metaverse. Tato konfigurace je hlášené jako nejednoznačný, i když hello atributy ve zdroji hello hello stejnou hodnotu.  
![Více objektů připojený toohello stejného objektu mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

V tomto scénáři musíte toochange hello obor pravidel synchronizace hello tak hello zdroj objekty mají různé synchronizační pravidla v oboru. Což vám umožní toodefine různých priorit.  
![Více objektů připojený toohello stejného objektu mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Další kroky
* Další informace o hello výrazu jazyka v [Principy deklarativní zřizování výrazy](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
* Najdete v tématu Jak deklarativní zřizování je použité out-of-box v [Principy hello výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md).
* V tématu Jak toomake praktická změnit pomocí deklarativní zřizování v [jak toomake toohello změnu výchozí konfigurace](active-directory-aadconnectsync-change-the-configuration.md).
* Pokračovat tooread jak uživatelů a kontaktů spolupracují v [Principy uživatelů a kontaktů](active-directory-aadconnectsync-understanding-users-and-contacts.md).

**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)

**Témata odkazů**

* [Synchronizace Azure AD Connect: odkaz na funkce](active-directory-aadconnectsync-functions-reference.md)
