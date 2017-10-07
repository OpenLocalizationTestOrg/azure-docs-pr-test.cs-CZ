---
title: "aaaHow tooremove uživatele pro přístup k aplikaci tooan | Microsoft Docs"
description: "Pochopit, jak tooremove uživatele pro přístup k aplikaci tooan"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 17017bddb73aad5a0ef3a411ac91bf0423f0b600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooremove-a-users-access-tooan-application"></a>Jak tooremove uživatele pro přístup k aplikaci tooan

Tento článek vám pomůže toounderstand jak tooremove uživatele pro přístup k aplikaci tooan.

## <a name="i-want-tooremove-a-specific-users-or-groups-assignment-tooan-application"></a>Chci tooremove aplikace tooan přiřazení konkrétního uživatele nebo skupiny

tooremove uživatele nebo skupinu přiřazení tooan aplikace, postupujte podle kroků hello uvedené v hello [odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) článku.

. ## chci toodisable všechny aplikace tooan přístupu pro každého uživatele

toodisable všechny tooan přihlášení uživatele-aplikací, postupujte podle kroků hello uvedených v hello [zakázat uživatelská přihlášení pro podnikové aplikace v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) článku.

## <a name="i-want-toodelete-an-application-entirely"></a>Chci, aby toodelete aplikaci zcela

příliš**odstranit aplikaci**, postupujte podle pokynů hello níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello chcete toodelete.

7.  Po načtení hello aplikace, klikněte na **odstranit** ikonu z hello hlavní aplikace **přehled** okno.

## <a name="i-want-toodisable-all-future-user-consent-operations-tooany-application"></a>Chci toodisable všechny budoucí uživatelská souhlasu operations tooany aplikace

Zakázání souhlas uživatele pro celý adresář zabránit koncovým uživatelům z souhlas tooany aplikace. Správci mohou stále souhlas na behalves uživatele. souhlas toolearn Další informace o aplikaci a proč může nebo nemusí nepřejete toodo, články [Principy uživatelů a správce souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

příliš**zakažte všechny operace souhlasu budoucí uživatele v adresáři celý**, postupujte podle pokynů hello níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **uživatelská nastavení**.

6.  Zakažte všechny operace souhlasu budoucí uživatele tak, že nastavení hello **uživatelé můžou aplikace tooaccess svá data** přepnutí příliš**ne** a klikněte na tlačítko hello **Uložit** tlačítko.


# <a name="next-steps"></a>Další kroky
[Správa přístupu tooapps](active-directory-managing-access-to-apps.md)
