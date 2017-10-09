---
title: "aaaAzure služby Active Directory Automatická registrace zařízení – nejčastější dotazy | Microsoft Docs"
description: "Automatická registrace zařízení s Azure Active Directory, – nejčastější dotazy."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: ba7f113fd3bc310def001a1f44d938b0be71dba8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-automatic-device-registration-faq"></a>Nejčastější dotazy ohledně automatické registrace zařízení v Azure Active Directory

**Otázka: I nedávno zaregistrovaná zařízení hello. Proč nevidím hello zařízení v části Moje informace o uživateli v hello portál Azure?**

**Odpověď:** zařízení s Windows 10, které jsou připojené k doméně se automatická registrace zařízení se nezobrazí v části informace o uživateli hello.
Je nutné toouse prostředí PowerShell toosee všechna zařízení. 

Pouze hello následující zařízení jsou uvedené v části hello informace o uživateli:

- Všechny osobní zařízení, které nejsou připojené k enterprise 
- Všechny bez – Windows 10 nebo Windows Server 2016 
- Všechna zařízení jiný systém než Windows 

---

**Otázka: Proč nevidím všechna hello zařízení zaregistrované v Azure Active Directory v hello portál Azure?** 

**Odpověď:** v současné době neexistuje žádný způsob, jak toosee všech registrovaných zařízení v hello portálu Azure. Můžete vytvořit prostředí Azure PowerShell toofind všechna zařízení. Další podrobnosti najdete v tématu hello [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) rutiny.

--- 

**Otázka: jak lze zjistit, jaké stav hello zařízení registrace klienta hello je?**

**Odpověď:** stav registrace zařízení hello závisí na:

- Jaká zařízení hello
- Jak byl zaregistrován 
- Všechny podrobnosti související s tooit. 
 

---

**Otázka: Proč je zařízení I odstranili v hello Azure portálu nebo pomocí prostředí Windows PowerShell pořád objevuje as registrované?**

**Odpověď:** toto chování je záměrné. Hello zařízení nebude mít přístup tooresources v cloudu hello. Pokud chcete tooremove hello zařízení a znovu zaregistrovat, musí být manuální akce toobe pořízené hello zařízení. 

Pro Windows 10 a Windows Server 2016, které jsou místní AD připojené k doméně:

1.  Otevřete hello příkazový řádek jako správce.

2.  Typ`dsregcmd.exe /debug /leave`

3.  Odhlásit se a přihlaste se hello tootrigger naplánované se úloha, která znovu zaregistruje zařízení hello. 

Pro jiné platformy Windows, které jsou místní AD připojené k doméně:

1.  Otevřete hello příkazový řádek jako správce.
2.  Zadejte `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Zadejte `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---

**Otázka: Proč vidí položky duplicitní zařízení na portálu Azure?**

**ODPOVĚĎ:**

-   Pro Windows 10 a Windows Server 2016, pokud jsou opakovaných pokusech toounjoin a znova připojit hello stejného zařízení, může být duplicitní položky. 

-   Pokud jste použili přidat pracovní nebo školní účet, každý uživatel systému windows, který používá přidat pracovní nebo školní účet se vytvoří nový záznam zařízení s hello stejný název zařízení.

-   Jiné platformy Windows, které jsou místní AD připojené k doméně pomocí automatické registrace vytvoří nový záznam zařízení s hello stejný název zařízení pro každého uživatele domény, který se přihlásí do zařízení hello. 

-   AADJ počítač, který bylo vymazáno, přeinstalovat a znovu spojena s hello stejný název, se zobrazí jako jiný záznam se hello stejný název zařízení.

---

**Otázka: Proč může uživatel i nadále přístup k prostředkům ze zařízení, které I zakázali v hello portál Azure?**

**Odpověď:** může trvat až hodinu tooan odvolat toobe, použít.

>[!Note] 
>Pro zařízení, ke ztrátě doporučujeme vymazání hello zařízení tooensure, že uživatelé nemají přístup k zařízení hello. Další podrobnosti najdete v tématu [registrovat zařízení pro správu v Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**Otázka: Proč Moji uživatelé vidí "Tam nedostanete odsud"?**

**Odpověď:** Pokud jste nakonfigurovali určité toorequire pravidla podmíněného přístupu stavu konkrétní zařízení a zařízení hello nesplňuje kritéria hello, uživatelé se zablokuje a zobrazí tato zpráva. Prosím vyhodnocení hello pravidel a ujistěte se, že zařízení hello je možné toomeet hello kritéria tooavoid tuto zprávu.

---


**Otázka: I najdete v části hello zařízení záznam zařízení v části hello informace o uživateli v hello portál Azure a můžete zobrazit stav hello as registrované v klientovi hello. Se, že správně nastavit pro použití podmíněného přístupu?**

**Odpověď:** záznam zařízení hello (deviceID) a stav na portálu Azure hello musí odpovídat hello klienta a splnění libovolného kritéria hodnocení pro podmíněný přístup. Další podrobnosti najdete v tématu [Začínáme s Azure Active Directory Device Registration](active-directory-device-registration.md).

---

**Otázka: Proč se zobrazila zpráva "uživatelské jméno nebo heslo je chybné." pro zařízení I právě připojené tooAzure AD?**

**Odpověď:** běžných příčin pro tento scénář jsou:

- Pověření uživatele již není platný.

- Váš počítač je nelze toocommunicate s Azure Active Directory. Zkontrolujte všechny problémy se síťovým připojením.

- nebyly splněny Azure AD Join požadavky Hello. Zkontrolujte, že jste provedli kroky hello [rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-overview.md).  

- Federované přihlášení vyžaduje váš federační server toosupport WS-Trust aktivní koncový bod. 

---

**Otázka: Proč se zobrazuje hello "Oops... došlo k chybě!" dialogové okno při připojení k počítači?**

**Odpověď:** jedná o výsledek nastavení registrace Azure Active Directory s Intune. Další podrobnosti najdete v tématu [nastavení správy pro zařízení Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**Otázka: Proč můj pokus o toojoin počítači nezdaří, i když se mi nepřišel. veškeré informace o chybě?**

**Odpověď:** pravděpodobnou příčinou je, že hello uživatel je přihlášen toohello zařízení pomocí hello předdefinovaný účet správce. Před použitím Azure Active Directory Join instalace hello toocomplete vytvořte jiný místní účet. 

---

**Otázka: kde najdete pokyny k instalaci hello Automatická registrace zařízení?**

**Odpověď:** podrobné pokyny najdete v tématu [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**Otázka: kde můžete najít řešení potíží s informace o hello Automatická registrace zařízení?**

**Odpověď:** informace o odstraňování potíží, najdete v tématu:

- [Řešení potíží s automatickou registraci domény připojené počítače tooAzure AD – Windows 10 a Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md)

- [Řešení potíží s automatickou registraci domény připojené počítače tooAzure AD pro klienty nižší úrovně systému Windows](active-directory-device-registration-troubleshoot-windows-legacy.md)
 
---

