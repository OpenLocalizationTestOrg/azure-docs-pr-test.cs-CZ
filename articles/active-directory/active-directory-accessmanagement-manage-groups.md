---
title: aaaManaging skupin v Azure Active Directory | Microsoft Docs
description: "Jak toocreate a spravovat skupiny toomanage Azure uživatelů pomocí služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d1f5451c-3807-423c-8bac-2822d27b893f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 9bee224655639740b3dd99983892b30c3c537aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-groups-in-azure-active-directory"></a>Správa skupin ve službě Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [Portál Azure Classic](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Jedna z funkcí hello správy uživatele Azure Active Directory (Azure AD) je hello možnost toocreate skupiny uživatelů. Používáte úlohy správy skupiny tooperform, jako je například přiřazení licence nebo oprávnění tooa počet uživatelů najednou. Můžete také skupiny tooassign přístupová oprávnění k

* Prostředky, například objekty v adresáři hello
* Adresář externí toohello prostředky jako aplikace SaaS, služby Azure, weby služby SharePoint nebo místní prostředky

Vlastník prostředku kromě toho můžete přiřadit přístup tooa tooan Azure AD skupinu prostředků vlastníkem někdo jiný. Toto přiřazení uděluje hello členové skupiny přístup toohello prostředku. Vlastník hello hello skupiny potom spravuje členství ve skupině hello. Efektivně hello vlastníka delegáti toohello vlastník prostředku hello skupiny hello oprávnění tooassign uživatelé tootheir prostředku.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. Jak toomanage skupin v Centru pro správu hello Azure AD, najdete v části [vytvořte skupinu a přidejte členy v Azure Active Directory](active-directory-groups-create-azure-portal.md).

## <a name="how-do-i-create-a-group"></a>Vytvoření skupiny
V závislosti na toowhich hello služby, které má vaše organizace předplatné můžete vytvořit skupinu pomocí jedné z následujících hello:

* Hello portál Azure classic
* portál účtů Hello Office 365
* portál účtů Windows Intune Hello

Úlohy popíšeme, jak provést v hello portál Azure classic. Další informace o používání portálů mimo platformu Azure toomanage adresáře Azure AD najdete v tématu [Správa adresáře služby Azure AD](active-directory-administer.md).

1. V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak vyberte název hello hello adresáře pro vaši organizaci.
2. Vyberte hello **skupiny** kartě.
3. Vyberte **Přidat skupinu**.
4. V hello **přidat skupinu** okno, zadejte hello název a popis skupiny hello.

## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Přidání nebo odebrání jednotlivých uživatelů ve skupině zabezpečení
**tooadd skupinu tooa jednotlivé uživatele**

1. V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak vyberte název hello hello adresáře pro vaši organizaci.
2. Vyberte hello **skupiny** kartě.
3. Otevřete toowhich skupiny hello chcete tooadd členy. Otevřete hello **členy** kartě hello vybrané skupiny, pokud ho ještě není zobrazení.
4. Vyberte **Přidat členy**.
5. Na hello **přidat členy** stránky, vyberte hello název hello uživatele nebo skupinu, že chcete tooadd jako člena této skupiny. Ujistěte se, zda tento název je přidána toohello **vybrané** podokně.

**tooremove jednotlivého uživatele ze skupiny**

1. V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak vyberte název hello hello adresáře pro vaši organizaci.
2. Vyberte hello **skupiny** kartě.
3. Otevřete skupinu hello, ze kterého mají být členy tooremove.
4. Vyberte hello **členy** karty, vyberte hello název člena hello má tooremove z této skupiny a pak klikněte na tlačítko **odebrat**.
5. Příkazového řádku hello potvrďte, že chcete tooremove tohoto člena ze skupiny hello.

## <a name="how-can-i-manage-hello-membership-of-a-group-dynamically"></a>Jak lze spravovat hello členství ve skupině dynamicky?
Ve službě Azure AD můžete velmi snadno nastavit jednoduché pravidlo toodetermine, uživatelů, kteří jsou členy toobe skupiny hello. Jednoduché pravidlo je takové pravidlo, které provádí jenom jedno porovnání. Například pokud je skupina přiřazena tooa aplikace SaaS, můžete nastavit uživatele tooadd pravidlo, kteří mají na pozici "Obchodního zástupce". Toto pravidlo pak udělují přístup toothis SaaS aplikace tooall uživatelů s touto funkcí ve vašem adresáři.

Pokud se vyhodnotí jako hello systému atributy změnu uživatele, všechna pravidla dynamické skupiny v adresáři toosee Pokud změnu atributu hello hello uživatele by spustit žádnou skupinu přidá nebo odebere. Pokud uživatel splňuje pravidlo ve skupině, přidají se jako člen skupiny toothat. Pokud už vyhovují hello pravidlo, které jsou členy skupiny, budou odstraněny jako členy z této skupiny.

> [!NOTE]
> Pravidlo pro dynamické členství můžete nastavit pro skupiny zabezpečení nebo pro skupiny Office 365. Vnořené členství ve skupinách nejsou aktuálně podporovány pro přiřazení na základě skupiny tooapplications.
>
> Dynamické členství ve skupinách vyžaduje toobe licence Azure AD Premium přiřazené
>
> * Hello správce, který spravuje hello pravidlo pro skupinu
> * Všichni členové skupiny hello
>
>

**tooenable dynamické členství ve skupině**

1. V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak vyberte název hello hello adresáře pro vaši organizaci.
2. Vyberte hello **skupiny** kartě a chcete tooedit skupiny otevřete hello.
3. Vyberte hello **konfigurace** kartě a poté nastavte **povolit dynamické členství** příliš**Ano**.
4. Nastavte jedno jednoduché pravidlo pro skupinu toocontrol hello dynamické členství pro funkce v této skupině. Ujistěte se, zda text hello **přidat uživatele kde** možnost je vybrána a potom vyberte vlastnost uživatele ze seznamu hello (například. oddělení, pracovní funkce atd.),
5. V dalším kroku vyberte podmínku (nerovná se, rovná se, nezačíná, začíná, neobsahuje, obsahuje, neodpovídá, odpovídá).
6. Zadejte hodnotu porovnání pro vlastnost hello vybrané uživatele.

toolearn o tom, toocreate *rozšířené* pravidel (která může obsahovat několik porovnání) pro dynamické členství ve skupině, najdete v části [pravidel pomocí atributů toocreate rozšířené](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Další informace
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu tooresources pomocí skupin Azure Active Directory](active-directory-manage-groups.md)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
