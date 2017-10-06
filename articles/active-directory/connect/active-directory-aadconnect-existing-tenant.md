---
title: "Azure AD Connect: Pokud už máte Azure AD | Microsoft Docs"
description: "Toto téma popisuje, jak toouse připojovat, když máte existujícího klienta Azure AD."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: f7b166e9e85c1f03330e8e0074d4afa4036738c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-when-you-have-an-existent-tenant"></a>Azure AD Connect: Pokud máte existující klienta
Většina hello tématech jak toouse Azure AD Connect předpokládá můžete začít s novou Azure AD klienta a že nejsou žádní uživatelé nebo existuje jiné objekty. Pokud jste spustili s klient služby Azure AD, ale naplněný uživatelů a dalších objektů a teď chcete toouse připojit, pak toto téma je pro vás.

## <a name="hello-basics"></a>Základy Hello
Řídí objektu ve službě Azure AD se buď hlavním v hello cloudu (Azure AD) nebo místní. Pro jeden jednoho objektu se nedají spravovat některé atributy místní a některé další atributy ve službě Azure AD. Každý objekt má příznak, který udává, kdy je objekt hello spravován.

Některé uživatele na místě a dalších můžete spravovat v cloudu hello. Běžný scénář pro tuto konfiguraci je organizace se smíšenými účetní pracovníků a prodeje pracovních procesů. Hello účetní pracovníci mít místní účet AD, ale prodeje pracovníci hello nepodporují, budou mít účet ve službě Azure AD. By spravovat některé uživatele místní a některé ve službě Azure AD.

Pokud jste začali toomanage uživatelů ve službě Azure AD, které jsou k dispozici také v místní službě AD a později chcete toouse připojit, pak jsou některé další otázky, které že budete potřebovat tooconsider.

## <a name="sync-with-existing-users-in-azure-ad"></a>Synchronizovat s stávajících uživatelů ve službě Azure AD
Při instalaci Azure AD Connect a spustíte synchronizaci, hello synchronizační služba Azure AD (ve službě Azure AD) neobsahuje kontrolu pro každý nový objekt a akci toofind existující toomatch objektu. Pro tento proces se používají tři atributy: **userPrincipalName**, **proxyAddresses**, a **sourceAnchor**/**immutableID** . Shoda s **userPrincipalName** a **proxyAddresses** se označuje jako **logicky shodu**. Shoda s **sourceAnchor** se označuje jako **pevný shodu**. Pro hello **proxyAddresses** atribut pouze hello hodnotu s **SMTP:**, který je hello primární e-mailovou adresu, slouží k vyhodnocení hello.

Shoda Hello je Vyhodnocená jenom pro všechny nové objekty pocházejících z připojení. Pokud změníte stávající objekt, je odpovídající, žádný z těchto atributů, pak se zobrazí chybová místo.

Pokud Azure AD nalezne objekt, kde jsou hodnoty atributu hello hello stejné pro objekt přicházející z Connect a který se již nachází ve službě Azure AD a potom hello objektu ve službě Azure AD převzat připojit. Hello dříve objekt spravovanými přes cloud je označení místního spravované. Všechny atributy ve službě Azure AD s hodnotou v místní AD přepsána hello místní hodnota. Hello výjimky je v případě má atribut **NULL** hodnotu místně. V takovém případě hello hodnotu v Azure AD zůstane, ale stále ji změnit jedině tak místní toosomething else.

> [!WARNING]
> Vzhledem k tomu, že všechny atributy ve službě Azure AD budou přepsány hello místní hodnota toobe, ujistěte se, že máte funkční data místně. Například pokud pouze jste nahráli e-mailovou adresu v Office 365 a není zachovány aktualizovat v místní službě AD DS a ztratit všechny hodnoty v Azure AD nebo Office 365 nejsou k dispozici ve službě AD DS.

> [!IMPORTANT]
> Pokud používat synchronizaci hesla, která je vždy používá Expresní nastavení, pak dojde k přepsání hello hesla ve službě Azure AD hello heslo v místní AD. Pokud jsou vaši uživatelé použít toomanage různá hesla, je třeba v tooinform je, které by měli používat hello místní heslo, pokud jste nainstalovali připojit.

Hello předchozí části a upozornění je třeba zvážit při plánování. Pokud jste provedli mnoho změn ve službě Azure AD není promítnuta místní služby AD DS, pak je nutné tooplan pro jak toopopulate služby AD DS s hello před synchronizaci objektů vašeho službou Azure AD Connect aktualizovat hodnoty.

Pokud kritériím odpovídá objektů s konfigurace soft-match pak hello **sourceAnchor** přidání toohello objektu ve službě Azure AD, pevné shoda lze později.

### <a name="hard-match-vs-soft-match"></a>Pevný match vs konfigurace Soft-match
Pro novou instalaci připojení není žádný praktické rozdíl mezi soft - a pevné nalezena shoda. Hello rozdíl je v situaci, obnovení po havárii. Pokud jste ztratili serveru s Azure AD Connect, bez ztráty dat, můžete přeinstalovat novou instanci. Objekt s sourceAnchor je odeslána tooConnect během počáteční instalace. Hello shodu lze poté vyhodnotit klientem hello (Azure AD Connect), což je mnohem rychlejší než to stejné hello ve službě Azure AD. Pevné shoda bude vyhodnocena Connect a službou Azure AD. Logicky shoda pouze vyhodnotí se službou Azure AD.

### <a name="other-objects-than-users"></a>Jiné objekty než uživatelé
Uživatelé obvykle mají userPrincipalName a proxyAddresses, tím snadno hello shodu. Ale jiné objekty, třeba skupiny zabezpečení, těch, které nemají. V takovém případě je možné porovnávat jenom na pevné odpovídající pomocí hello sourceAnchor. Hello sourceAnchor je vždy hello Base64 převést **objectGUID** místně, tak v Azure AD, když potřebujete toomatch dva objekty, je třeba aktualizovat hodnotu hello. Hello sourceAnchor/immutableID lze aktualizovat pouze pomocí prostředí PowerShell a ne prostřednictvím hello portálů.

## <a name="create-a-new-on-premises-active-directory-from-data-in-azure-ad"></a>Vytvoření nové místní Active Directory z dat ve službě Azure AD
Někteří zákazníci začínat čistě cloudové řešení s Azure AD a nemají místní AD. Později má tooconsume místních prostředků a chcete toobuild místní AD podle data služby Azure AD. Azure AD Connect nemůže pomoct pro tento scénář. Nedojde k vytvoření uživatele na místě a nemá žádné možnost tooset hello heslo místní toohello stejný jako v Azure AD.

Pokud hello pouze důvod, proč máte v plánu tooadd místní AD se objekty LOBs toosupport (-obchodní aplikace), pak je možná byste měli zvážit toouse [služby Azure AD domain services](../../active-directory-domain-services/index.md) místo.

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
