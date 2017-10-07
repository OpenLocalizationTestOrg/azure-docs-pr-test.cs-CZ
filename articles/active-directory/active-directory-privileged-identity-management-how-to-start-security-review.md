---
title: "aaaHow toostart kontrola přístupu | Microsoft Docs"
description: "Zjistěte, jak toocreate přístupu zkontrolujte pro privilegované identity pomocí hello aplikací Azure Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 3e52b731-55f4-4c8a-ba87-9fd34033f52f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 24feac307f77c69b5d68d6ae0623dbcb52416b01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostart-an-access-review-in-azure-ad-privileged-identity-management"></a>Jak toostart přístupu zkontrolujte v Azure AD Privileged Identity Management
Přiřazení rolí stát "zastaralé", když uživatelé mají privilegovaný přístup, které už nepotřebují. V pořadí tooreduce hello riziko spojené s přiřazení těchto zastaralých rolí Správci privilegované role by pravidelně zkontrolovat hello rolí, které mají uživatelé. Tento dokument popisuje hello kroky pro spuštění kontrola přístupu v Azure AD Privileged Identity Management (PIM).

## <a name="start-an-access-review"></a>Spuštění kontrola přístupu
> [!NOTE]
> Pokud jste nepřidali řídicí panel tooyour hello PIM aplikací v hello portálu Azure, najdete v části hello kroky v [Začínáme s Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)
> 
> 

Z hello PIM aplikace hlavní stránky existují tři způsoby toostart kontrola přístupu:

* **Přístup k recenze** > **přidat**
* **Role** > **zkontrolujte** tlačítko
* Vyberte hello určité role toobe zkontrolováni z seznam rolí hello > **zkontrolujte** tlačítko

Po kliknutí na hello **zkontrolujte** tlačítko hello **spustit kontrola přístupu** otevře se okno. V tomto okně jste probíhající tooconfigure hello zkontrolujte s názvem a čas omezení, zvolte roli tooreview a rozhodnout, který provede zkontrolujte hello.

![Kontrola přístupu – snímek obrazovky Start][1]

### <a name="configure-hello-review"></a>Konfigurace zkontrolujte hello
Zkontrolujte toocreate přístup, budete potřebovat tooname ho a nastaví počáteční a koncové datum.

![Nakonfigurujte kontrolní – snímek obrazovky][2]

Ujistěte se, hello délka hello zkontrolujte dostatečně dlouhou dobu toocomplete uživatele. Pokud dokončíte před hello koncové datum, můžete vždy zastavit hello zkontrolujte již v rané fázi.

### <a name="choose-a-role-tooreview"></a>Vyberte role tooreview
Každý revize se zaměřuje na jen jednu roli. Pokud jste začali kontrola přístupu hello v okně konkrétní roli, budete potřebovat toochoose roli teď.

1. Přejděte příliš**Zkontrolujte členství v roli**
   
    ![Zkontrolujte členství v rolích – snímek obrazovky][3]
2. Vyberte jednu roli ze seznamu hello.

### <a name="decide-who-will-perform-hello-review"></a>Rozhodněte, který provede zkontrolujte hello
Existují tři možnosti pro provádění kontrolu. Můžete přiřadit hello zkontrolujte toosomeone else toocomplete, můžete provést sami, nebo může mít každý uživatel, zkontrolujte své vlastní přístup.

1. Přejděte příliš**vyberte**
   
    ![Vyberte – snímek obrazovky][4]
2. Vyberte jednu z možností hello:
   
   * **Vyberte kontrolora**: tuto možnost použijte, pokud si nejste jisti, který potřebuje přístup. Pomocí této možnosti můžete přiřadit vlastníka prostředku tooa zkontrolujte hello nebo toocomplete správce skupiny.
   * **Poslat mi**: užitečné, pokud chcete, aby toopreview jak pracovní zkontroluje přístup, nebo chcete tooreview jménem osoby, které nelze.
   * **Členy zkontrolujte sami**: použití této možnosti toohave hello uživatelé kontrole vlastní přiřazení rolí.

### <a name="start-hello-review"></a>Spustit kontrolní hello
Nakonec máte toorequire hello možnost, že uživatelé zadat příslušný důvod. Pokud schválí jejich přístup. Pokud chcete přidat popis hello zkontrolujte a vyberte **spustit**.

Zajistěte, aby dát uživatelům vědět, že je kontrola přístupu jim a zobrazit je [jak tooperform přístupu zkontrolujte](active-directory-privileged-identity-management-how-to-perform-security-review.md).

## <a name="manage-hello-access-review"></a>Spravovat kontrola přístupu hello
Můžete sledovat průběh hello hello kontroloři dokončení jejich recenze v hello řídicí panel Azure AD PIM, v hello zkontroluje přístup k oddílu. Žádné oprávnění se změní v adresáři hello dokud [dokončení zkontrolujte hello](active-directory-privileged-identity-management-how-to-complete-review.md).

Dokud hello zkontrolujte doba je u konce, uživatelé toocomplete můžete připomenout jejich kontrola nebo zastavení hello kontrolní časná z hello přístupu zkontroluje části.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="pim-table-of-contents"></a>Tabulka PIM obsahu
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
