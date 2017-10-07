---
title: "Řešení potíží: 'Služby Active Directory, je položka chybí nebo není k dispozici | Microsoft Docs"
description: "Jaké toodo při položku nabídky služby Active Directory se nezobrazí v hello portálu pro správu Azure."
services: active-directory
documentationcenter: na
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 3383020d-6397-43ea-b7aa-c6a9d6a1e3df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.openlocfilehash: d7355a4e39141f0b09272dc5615c309b23c8c70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Řešení potíží: 'Služby Active Directory, je položka chybí nebo není k dispozici
Řadu hello pokyny k používání funkce služby Azure Active Directory a služby začínat řetězcem "přejděte toohello Azure Management Portal a klikněte na tlačítko **služby Active Directory**." Ale co můžete udělat, pokud není uvedené hello služby Active Directory rozšíření nebo položku nabídky, nebo pokud je označen **není k dispozici**? Toto téma je navrženou toohelp. Popisuje hello podmínky, za kterých **služby Active Directory** se nezobrazí nebo není k dispozici a vysvětluje, jak tooproceed.

## <a name="active-directory-is-missing"></a>Chybí služby Active Directory
Obvykle **služby Active Directory** položka je zobrazena v nabídce levém navigačním hello. Hello pokyny v Azure Active Directory postupech předpokládají, že tato položka je v zobrazení.

![Snímek obrazovky: služby Active Directory v Azure](./media/active-directory-troubleshooting/typical-view.png)

Hello služby Active Directory položka je zobrazena v levé navigační nabídce hello, pokud žádné z následujících podmínek hello hodnotu true. V opačném hello položky nezobrazí.

* aktuální uživatel Hello přihlášeni pomocí účtu Microsoft (dříve označované jako účet Windows Live ID).
  
    NEBO
* Hello klientovi Azure má adresáře a hello aktuálního účtu je správce adresáře.
  
    NEBO
* Hello klientovi Azure má alespoň jeden obor názvů řízení přístupu Azure AD (ACS). Další informace najdete v tématu [Namespace řízení přístupu](https://msdn.microsoft.com/library/azure/gg185908.aspx).
  
    NEBO
* Hello klientovi Azure má aspoň jednoho poskytovatele ověřování Azure Multi-Factor Authentication. Další informace najdete v tématu [Správa poskytovatelé Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

toocreate na obor názvů řízení přístupu nebo poskytovatele služby Multi-Factor Authentication, klikněte na tlačítko **+ nový** > **App Services** > **služby Active Directory**.

adresář tooa tooget práva pro správu, mají správce přiřadit tooyour účtu s rolí. Podrobnosti najdete v tématu [přiřazení rolí správce](active-directory-assign-admin-roles.md).

## <a name="active-directory-is-not-available"></a>Služba Active Directory není k dispozici
Když kliknete na tlačítko **+ nový** > **App Services**, **služby Active Directory** položka je zobrazena. Konkrétně hello položky služby Active Directory se zobrazí, když některé z funkcí služby Active Directory hello, jako je například adresář, řízení přístupu nebo zprostředkovatel vícefaktorového ověřování, jsou k dispozici toohello aktuálního uživatele.

Ale při načítání stránky hello hello položka není k dispozici a je označen jako **není k dispozici**. Toto je dočasný stav. Pokud jste Počkejte několik sekund, hello položky k dispozici. Pokud se prodlužuje hello zpoždění, aktualizace webovou stránku hello často řeší problém hello.

![Snímek obrazovky: není k dispozici služba Active Directory](./media/active-directory-troubleshooting/not-available.png)

