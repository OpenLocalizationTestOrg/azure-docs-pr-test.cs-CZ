---
title: "aaaHow toocomplete kontrola přístupu | Microsoft Docs"
description: "Po spuštění kontrola přístupu v Azure AD Privileged Identity Management zjistěte, jak toocomplete ho a zobrazení výsledků hello"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: abc2d3dd-afd5-42cf-8a17-6c11f5674c35
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: f99ddf3ebcf80b51110326064d584f33d8e1679a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocomplete-an-access-review-in-azure-ad-privileged-identity-management"></a>Jak toocomplete přístupu zkontrolujte v Azure AD Privileged Identity Management
Správce privilegovaných rolí můžete zkontrolovat jednou privilegovaného přístupu [revize zabezpečení byla spuštěna](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure AD Privileged Identity Management (PIM), bude automaticky posílat výzvy uživatelé tooreview jejich přístup k e-mailu. Pokud uživatel neobdrželi e-mailu, můžete odeslat je hello pokyny [jak tooperform zabezpečení zkontrolujte](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Po dobu kontrola zabezpečení hello je nad nebo všichni uživatelé hello dokončení jejich samoobslužné zkontrolujte, postupujte podle kroků hello v této kontrole hello toomanage článek a zobrazit výsledky hello.

## <a name="manage-security-reviews"></a>Správa zabezpečení recenze
1. Přejděte toohello [portál Azure](https://portal.azure.com/) a vyberte hello **Azure AD Privileged Identity Management** aplikace na řídicím panelu.
2. Vyberte hello **přístup recenze** část řídicího panelu hello.
3. Vyberte, které chcete toomanage kontrola přístupu hello.

V okně podrobností kontrola přístupu hello jsou číslo možností pro správu této revize.

![PIM přístupu zkontrolujte tlačítka – snímek obrazovky][1]

### <a name="remind"></a>Připomenout
Pokud kontrola přístupu je nastaven tak, aby uživatelé hello zkontrolujte sami, hello **připomenutí** tlačítko odesílá oznámení. 

### <a name="stop"></a>Zastavit
Všechny recenze přístup mít koncové datum, ale můžete použít hello **Zastavit** tlačítko toofinish je již v rané fázi. Pokud žádné uživatele nebyly revidován té doby, nebudou moct tooafter zastavíte zkontrolujte hello. Kontrolu nelze restartovat po byla zastavena.

### <a name="apply"></a>Použít
Po dokončení kontrola přístupu buď protože bylo dosaženo maximálního hello koncové datum nebo byla ručně zastavena hello **použít** tlačítko implementuje hello výsledek zkontrolujte hello. Pokud v hello zkontrolujte byl odepřen přístup uživatele, je to hello krok, který odstraní přiřazení role.  

### <a name="export"></a>Export
Pokud chcete výsledky hello tooapply revize zabezpečení hello ručně, můžete exportovat zkontrolujte hello. Hello **exportovat** tlačítko se spustí stahování souboru CSV. Můžete spravovat hello výsledky v aplikaci Excel nebo programy, které soubory sdíleného svazku clusteru.

### <a name="delete"></a>Odstranění
Pokud si nejste zájem o hello žádné další zkontrolovat, odstraňte jej. Hello **odstranit** tlačítko odebere kontrolní hello hello PIM aplikace.

> [!IMPORTANT]
> Nebude zobrazí upozornění, než dojde k odstranění, proto se ujistěte, že chcete toodelete, zkontrolovat. 

## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
