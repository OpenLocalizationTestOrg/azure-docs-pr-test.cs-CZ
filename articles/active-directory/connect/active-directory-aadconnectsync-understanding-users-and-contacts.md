---
title: "Synchronizace Azure AD Connect: Principy uživatelů a kontaktů | Microsoft Docs"
description: "Vysvětluje, uživatelů a kontaktů v synchronizaci Azure AD Connect."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d204647-213a-4519-bd62-49563c421602
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi;andkjell
ms.openlocfilehash: 4d80648c53a2981eb2626338913ed2282423f183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Synchronizace služby Azure AD Connect: Principy uživatelů a kontaktů
Existuje několik různých důvodů, proč by měla mít několik doménových struktur služby Active Directory a existuje několik různých nasazení topologie. Po fúze a akvizice zahrnovat běžné modely k nasazení účtu prostředků a GAL sync'ed doménových struktur. Ale i v případě, že jsou čistá modely, hybridní modely jsou společné také. Hello výchozí konfigurace v synchronizaci Azure AD Connect nepředpokládá žádné konkrétní modelu, ale v závislosti na tom, jak hledání shody uživatelů byla vybrána v instalační příručce hello, může být dodržen různé chování.

V tomto tématu provedeme chování hello výchozí konfigurace v určitých topologiích. Projdeme hello konfigurace a hello editoru pravidel synchronizace lze použít toolook v konfiguraci hello.

Existuje několik obecná pravidla, která předpokládá hello konfigurace:

* Bez ohledu na to, které pořadí jsme importovat ze zdroje hello Active Directory hello konečný výsledek by měl vždycky být hello stejné.
* Aktivní účet vždy přispějí přihlašovací údaje, včetně **userPrincipalName** a **sourceAnchor**.
* Deaktivovaného účtu přispějí userPrincipalName a sourceAnchor, pokud je propojená poštovní schránka, pokud neexistuje žádné aktivní účet toobe nalezen.
* Účet s poštovní schránku propojenou se nikdy použije pro userPrincipalName a sourceAnchor. Předpokládá se, že bude aktivní účet později nalezeno.
* Objekt kontaktu může být zřízené tooAzure AD jako kontakt nebo jako uživatel. Nevíte skutečně dokud všech doménových struktur služby Active Directory zdroj byly zpracovány.

## <a name="contacts"></a>Kontakty
S kontakty představující uživatele v jiné doménové struktuře je běžné po fúze a akvizice kde je řešení GALSync přemostění dvou nebo více doménovými strukturami systému Exchange. Objekt kontaktu Hello vždycky připojuje z hello konektoru místo toohello metaverse pomocí atribut mail hello. Pokud již existuje objekt kontaktu nebo objektu uživatele s hello stejné poštovní adresa, hello objekty jsou propojeny. To je nakonfigurovaný v pravidle hello **v ze služby Active Directory – obraťte se na připojení**. Je také na pravidlo s názvem **v ze služby Active Directory – běžné kontaktujte** s atributem atributu toku toohello úložiště metaverse **sourceObjectType** s hello konstanta **kontaktujte**. Toto pravidlo má velmi nízkou prioritu, pokud je připojený k toohello uživatelských objektů stejné objektu úložiště metaverse a pak hello pravidlo **v ze služby Active Directory – běžné uživatele** přispějí atribut toothis uživatele hodnota hello. S tímto pravidlem bude mít tento atribut hello hodnotu, pokud byl připojen žádný uživatel, kontaktujte a hello hodnota uživatele, pokud byl nalezen nejméně jeden uživatel.

Pro zřizování tooAzure k objektu AD, hello odchozí pravidlo **Out tooAAD – obraťte se na připojení** vytvoří objekt kontaktu, pokud hello atribut úložiště metaverse **sourceObjectType** je nastaven příliš**obraťte se na** . Pokud tento atribut je nastaven příliš**uživatele**, pak hello pravidlo **Out tooAAD – připojení uživatele k** vytvoří objekt uživatele.
Je možné, že objekt povýší z kontaktujte tooUser při další zdroje Active Directory byly naimportovány a synchronizovány.

Například v topologii GALSync jsme najdete kontaktní objekty pro všechny uživatele v druhé doménové struktury hello při jsme importu hello první doménovou strukturu. To bude dvoufázové instalace nové kontaktní objekty v hello konektoru AAD. Když jsme později importovat a synchronizovat hello druhé doménové struktury, jsme vyhledá hello skutečné uživatelů a připojovat je stávající úložiště metaverse objekty toohello. Potom jsme se odstranit objekt kontaktu hello v AAD a místo toho vytvořte nový objekt uživatele.

Pokud máte topologii, kde jsou uživatelé vyjádřené kontakty, zkontrolujte, zda že v Průvodce instalací hello vyberete toomatch uživatelů na atribut mail hello. Pokud vyberete jinou možnost, bude mít závislé konfigurace aplikace pořadí. Kontaktujte objekty se vždy připojí na atribut mail hello, ale uživatelské objekty se připojí pouze na atribut mail hello, pokud byla tato možnost vybrána v hello Průvodce instalací. Může pak skončili s dva různé objekty v úložišti metaverse hello s hello stejný atribut mail, pokud objekt kontaktu hello naimportované před hello objektu user. Během exportu tooAzure AD bude vyvolána k chybě. Toto chování je záměrné a může naznačovat chybná data nebo tuto topologii hello nebyl správně identifikovány během instalace hello.

## <a name="disabled-accounts"></a>Zakázané účty
Zakázané účty jsou synchronizovány také tooAzure AD. Zakázané účty jsou běžné toorepresent prostředky v systému Exchange, například konferenční místnosti. Výjimka Hello je uživatelé s poštovní schránku propojenou; Jak jsme uvedli tyto se nikdy zřídit účtu tooAzure AD.

předpokládá Hello je, že pokud je účet zakázaný uživatel nalezen, pak jsme nebude najít jiný účet služby active později a objekt hello je zřízená tooAzure AD s hello userPrincipalName a sourceAnchor nalezen. V případě jiný aktivní účet se připojí toohello, který se použije stejný objekt úložiště metaverse, poté její userPrincipalName a sourceAnchor.

## <a name="changing-sourceanchor"></a>Změna sourceAnchor
Pokud objekt, který se exportoval tooAzure AD mělo není povolené toochange hello sourceAnchor už. Když byl objekt hello atribut úložiště metaverse exportovaný hello **cloudSourceAnchor** nastavena hello **sourceAnchor** hodnota přijata službou Azure AD. Pokud **sourceAnchor** mění a neshodují **cloudSourceAnchor**, pravidlo hello **Out tooAAD – připojení uživatele k** vyvolá výjimku hello chyba **atribut sourceAnchor došlo ke změně**. V takovém případě musí být hello konfigurace nebo data opravit tak hello stejné sourceAnchor je k dispozici v úložišti metaverse hello znovu před hello objekt lze znovu synchronizovat.

## <a name="additional-resources"></a>Další zdroje
* [Azure AD Connect Sync: Možnosti přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)

