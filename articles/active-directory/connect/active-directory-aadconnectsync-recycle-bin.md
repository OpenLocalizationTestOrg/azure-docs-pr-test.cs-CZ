---
title: "Synchronizace Azure AD Connect: zapnutí koše AD | Microsoft Docs"
description: "Toto téma doporučuje použití hello funkce Koš služby AD s Azure AD Connect."
services: active-directory
keywords: "Koš služby AD, náhodným odstraněním, zdrojové ukotvení"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 2bb4827d677ccecfd8d2861f2a2fcf73b8cc2d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Synchronizace Azure AD Connect: zapnutí koše AD
Doporučujeme, abyste povolili funkce Koš hello AD pro vaše Active Directory v místě, které jsou synchronizované tooAzure AD. 

Pokud jste omylem odstranili místní objekt uživatele AD a obnovení pomocí funkce hello, Azure AD obnoví hello odpovídající objekt uživatele Azure AD.  Informace o funkci Koš hello AD, najdete v části tooarticle [přehled scénářů pro obnovení odstranit objekty služby Active Directory](https://technet.microsoft.com/library/dd379542.aspx).

## <a name="benefits-of-enabling-hello-ad-recycle-bin"></a>Výhody povolení hello AD Koš.
Tato funkce vám pomůže s obnovováním objekty uživatele Azure AD díky hello následující:

* Pokud jste omylem odstranili místní uživatele AD objektu hello odpovídající objekt uživatele Azure AD budou odstraněny v hello příštím synchronizačním cyklu. Ve výchozím nastavení zajišťuje Azure AD objekt uživatele Azure AD hello odstranit dočasně odstraněné stavu po dobu 30 dnů.

* Pokud máte místní povolena funkce Koš služby AD, můžete obnovit hello odstranit místní objekt uživatele AD beze změny jeho hodnota zdrojové ukotvení. Když hello obnovit místní objekt uživatele AD se synchronizují tooAzure AD, Azure AD bude obnovení hello odpovídající obnovitelné odstranění objektu uživatele Azure AD. Informace o atributu zdrojové ukotvení najdete v části tooarticle [Azure AD Connect: konceptech návrhu](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Pokud nemáte místní recyklovat povolena funkce Koš služby AD, můžete být požadované toocreate AD uživatele objektu tooreplace hello odstraněného objektu. Pokud službu synchronizace Azure AD Connect je nakonfigurované toouse generována AD atribut (například identifikátoru GUID) pro atribut zdrojové ukotvení hello, hello nově vytvořený objekt uživatele AD nebude mít hello stejnou hodnotu zdrojové ukotvení jako hello odstraněný objekt uživatele AD. Když hello nově vytvořený objekt uživatele AD synchronizované tooAzure AD, Azure AD vytvoří nový objekt uživatele Azure AD, místo obnovení hello obnovitelné odstranění objektu uživatele Azure AD.

> [!NOTE]
> Ve výchozím nastavení Azure AD udržuje odstraněných objektů uživatele Azure AD ve stavu odstraněno 30 dní, než se trvale odstraní. Správci mohou však urychlit hello odstranění těchto objektů. Jakmile hello objekty jsou trvale odstraněny, se již nebude možné obnovit, i když místní, že je povolena funkce Koš služby AD.



## <a name="next-steps"></a>Další kroky
**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)

* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
