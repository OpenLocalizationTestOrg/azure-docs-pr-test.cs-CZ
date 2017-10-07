---
title: "hello aaaManaging zařízení pomocí portálu Azure - preview | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure portálu toomanage zařízení."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: a39d14e4ce8bb79f0223a9de40d5f1259a869927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-devices-using-hello-azure-portal---preview"></a>Správa zařízení pomocí portálu Azure hello – náhled

>[!NOTE]
>Tato funkce je aktuálně ve verzi public preview. Připravit toorevert nebo odeberte všechny změny. Hello funkce je dostupná v libovolné předplatné služby Azure Active Directory (Azure AD) verzi public Preview. Když funkce hello je obecně dostupná, může vyžadovat některé aspekty funkcí hello však předplatné služby Azure Active Directory premium.


Se správou zařízení ve službě Azure Active Directory (Azure AD) můžete zajistit, že vaši uživatelé přistupují k prostředkům ze zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů. 

V tomto tématu:

- Předpokládá, že jste obeznámeni s hello [Úvod toodevice správy ve službě Azure Active Directory](device-management-introduction.md)

- Zajišťuje, že jste se informace o správě zařízení pomocí portálu Azure hello


toomanage zařízení v hello portálu Azure, budete potřebovat tooclick **zařízení** v hello **spravovat** části hello hello **Azure Active Directory** okno.

![Spravovat zařízení s Intune](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a>Konfigurace nastavení zařízení

toomanage zařízení pomocí hello portálu Azure, které potřebují toobe zaregistrovaný nebo připojený k tooAzure AD. Jako správce můžete upřesnit hello proces registrace a připojení zařízení tak, že nakonfigurujete nastavení zařízení hello.

![Spravovat zařízení s Intune](./media/device-management-azure-portal/22.png)


okno nastavení zařízení Hello umožňuje tooconfigure:

- **Uživatelé mohou připojit zařízení tooAzure AD** – toto nastavení vám umožní tooselect hello uživatelů, kteří mohou připojit zařízení tooAzure AD. Výchozí hodnota Hello je **všechny**.

- **Zařízení připojená k další místní správci v Azure AD** -vyberete hello uživatele, kteří jsou udělena práva místního správce v zařízení. Přidá toohello uživatelů tady přidat *Správci zařízení* role ve službě Azure AD. Globální správci ve službě Azure AD a vlastníci zařízení jsou udělena práva místního správce ve výchozím nastavení. Tato možnost je k dispozici prostřednictvím produkty, jako je například Azure AD Premium nebo Enterprise Mobility Suite (EMS) hello funkce edice premium. 

- **Uživatelé mohou registrovat svá zařízení s Azure AD** -potřebujete tooconfigure tato nastavení tooallow zařízení toobe registrované s Azure AD. Pokud vyberete **žádné**, zařízení nejsou povoleny tooregister, které nejsou připojené k Azure AD nebo hybridní připojený k Azure AD. Registrace s Microsoft Intune nebo správy mobilních zařízení (MDM) pro Office 365 vyžaduje registraci. Pokud jste nakonfigurovali některou z těchto služeb **všechny** je vybraná a **NONE** není k dispozici...

- **Vyžadovat, aby zařízení toojoin Multi-Factor Auth** – můžete zvolit, jestli uživatelé jsou požadované tooprovide druhé ověření zohlednit toojoin jejich zařízení tooAzure AD. Výchozí hodnota Hello je **ne**. Doporučujeme, abyste při registraci zařízení, které vyžadují služby Multi-Factor authentication. Než povolíte službu Multi-Factor authentication pro tuto službu, je nutné zajistit, že aplikace Multi-Factor authentication je nakonfigurován pro hello uživatele, kteří registrovat svá zařízení. Další informace o službám různých Azure Multi-Factor authentication, naleznete v části [Začínáme s Azure Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md). 

- **Maximální počet zařízení** – toto nastavení vám umožní tooselect hello maximální počet zařízení, které uživatel může mít ve službě Azure AD. Pokud uživatel dosáhne této kvóty, ale nejsou měly mít tooadd další zařízení až jeden nebo více hello stávajících zařízení se odeberou. pro všechna zařízení, které jsou připojené k Azure AD nebo dnes zaregistrovat Azure AD se počítá Hello zařízení uvozovky. Hello výchozí hodnota je **20**.

- **Uživatelé můžou synchronizovat nastavení a dat aplikací mezi zařízeními** – ve výchozím nastavení, toto nastavení je příliš**NONE**. Výběr konkrétní uživatele nebo skupiny nebo všechny umožňuje nastavení a data toosync aplikace hello uživatele na jejich zařízeních Windows 10. Další informace o tom, jak funguje synchronizace ve Windows 10.
Tato možnost je k dispozici prostřednictvím produkty, jako je například Azure AD Premium nebo Enterprise Mobility Suite (EMS) hello premium funkce.
 
    ![Spravovat zařízení s Intune](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a>Vyhledávání zařízení

Jako správce v hello portál Azure, máte dvě možnosti toolocate zaregistrován a připojené k zařízení:

- **Všechna zařízení** v hello **spravovat** části hello **zařízení** okno  

    ![Všechna zařízení](./media/device-management-azure-portal/41.png)


- **Zařízení** v hello **spravovat** části **uživatele** okno
 
    ![Všechna zařízení](./media/device-management-azure-portal/43.png)



Obě možnosti můžete získat zobrazení tooa který:


- Umožní vám toosearch pro zařízení pomocí hello zobrazovaný název jako filtr.

- Poskytuje podrobný přehled o zaregistrovaná a připojené k zařízení

- Umožní vám tooperform běžné úlohy správy zařízení
   

![Všechna zařízení](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a>Úlohy správy zařízení

Jako správce, můžete spravovat hello zaregistrovaný nebo zařízení připojená k. Tato část poskytuje informace o běžných úlohách správy zařízení.


**Spravovat zařízení s Intune** – Pokud jste správce služby Intune, můžete spravovat zařízení, které jsou označeny jako **Microsoft Intune**. Správce můžete zobrazit další zařízení 

![Spravovat zařízení s Intune](./media/device-management-azure-portal/31.png)


**Povolit / zakázat zařízení s Azure AD**

tooenable nebo zakázat zařízení, je třeba toobe globálního správce ve službě Azure AD. Zakázáním zařízení tomuto zařízení brání přístupu k prostředkům Azure AD.  toodisable hello zařízení, můžete buďto kliknout na *...* Klikněte na zařízení hello další podrobnosti.

 
![Spravovat zařízení s Intune](./media/device-management-azure-portal/33.png)

Zakázáním zařízení se změní stav hello v hello **povoleno** sloupec příliš**ne**.

![Zakázat zařízení](./media/device-management-azure-portal/32.png)


**Odstranění zařízení s Azure AD** -toodelete zařízení, je nutné toobe globálního správce ve službě Azure AD.  
Odstranění zařízení:
 
- Tomuto zařízení brání přístupu k prostředkům Azure AD 

- Odebere všechny podrobnosti, které jsou připojené toohello zařízení, například, klíče Bitlockeru pro zařízení s Windows  

- Reprezentuje neobnovitelná aktivity a se nedoporučuje, pokud je potřeba

Pokud se zařízení spravuje jiná autoritu pro správu (např. Microsoft Intune), Zkontrolujte prosím, že zařízení hello byl vymazání / vyřazení před odstraněním hello zařízení ve službě Azure AD.

Můžete buď vybrat "..." Klikněte na zařízení hello další podrobnosti nebo toodelete hello zařízení
 
![Odstranění zařízení](./media/device-management-azure-portal/34.png)


**Zobrazení nebo zkopírujte ID zařízení** -ID podrobnosti zařízení ID tooverify hello zařízení můžete použít na hello zařízení nebo pomocí prostředí PowerShell při řešení potíží. tooaccess hello kopie možnost, klikněte na tlačítko hello zařízení.

![Zobrazit ID zařízení](./media/device-management-azure-portal/35.png)
  

**Zobrazení nebo kopírování klíče Bitlockeru** – Pokud jste správce, můžete zobrazit kopie hello BitLocker klíče toohelp uživatelé toorecover jejich šifrované jednotce. Tyto klíče jsou dostupné pouze pro zařízení s Windows, které jsou zašifrované a jejich klíče uloženého ve službě Azure AD. Při přístupu k podrobnosti hello zařízení, můžete zkopírovat tyto klíče.
 
![Klíče Bitlockeru zobrazení](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a>Protokoly auditu


Hello zařízení aktivity jsou dostupné přes protokoly aktivity hello. To zahrnuje aktivity spustí služba hello registrace zařízení nebo uživatel hello:

- Vytváření zařízení a přidání vlastníků/uživatelů zařízení hello

- Změny nastavení toodevice

- Operace zařízení například odstranění nebo aktualizaci zařízení
 
Vaše vstupní bod toohello auditování dat je **protokoly auditu** v hello **aktivity** části hello **zařízení* okno.

![Protokoly auditu](./media/device-management-azure-portal/61.png)


Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:

- Hello datum a čas výskytu hello

- Hello cíle

- Hello iniciátor nebo objektu actor (který) aktivity

- Hello aktivity, (co)

![Protokoly auditu](./media/device-management-azure-portal/63.png)

Kliknutím můžete přizpůsobit zobrazení seznamu hello **sloupce** v panelu nástrojů hello.
 
![Protokoly auditu](./media/device-management-azure-portal/64.png)


toonarrow dolů hello nahlášené tooa dat na úrovni, funguje pro vás, můžete filtrovat data auditu hello pomocí hello následující pole:

- Catergory
- Typ prostředku aktivity
- Aktivita
- Rozsah dat
- cíl
- Zahájit (objektu Actor)

Kromě toho toohello filtrů, můžete vyhledat konkrétní položky.

![Protokoly auditu](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Další kroky

* [Úvod toodevice správy ve službě Azure Active Directory](device-management-introduction.md)



