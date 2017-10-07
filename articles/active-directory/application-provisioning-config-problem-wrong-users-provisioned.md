---
title: "aaaWrong sadu uživatelů, bude aplikace Galerie zřízené tooan Azure AD | Microsoft Docs"
description: "Zjistěte, jak toofind out proč probíhá jiné sady uživatelů zřídit tooan aplikace než ty, které jste očekávali"
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
ms.openlocfilehash: adb90b12a53fb3160ce2b73b2559df92b283438e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="wrong-set-of-users-are-being-provisioned-tooan-azure-ad-gallery-application"></a>Nesprávný sadu uživatelů, bude aplikace Galerie zřízené tooan Azure AD

Kteří uživatelé mají aplikace zřízené toohello primárně vycházejí z kteří uživatelé a skupiny byly **přiřazené** toohello aplikace.

Jak používat prostředky hello níže toolearn toocheck, kteří uživatelé a skupiny mají přiřazený tooan aplikací v rámci Azure Active Directory.

## <a name="assign-a-user-directly-as-an-administrator"></a>Přiřazení uživatele přímo jako správce

tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.

7.  Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.

8.  Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.

9.  Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.

10. Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**. Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.

13. Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.

14. **Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.

15. Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.

Pokud je nakonfigurován zřizování a pro aplikace je již spuštěn, noví uživatelé musí být zřízená tooan aplikace v přibližně 10 minut. Zkontrolujte hello **protokoly auditu** podrobnosti.

## <a name="assign-a-group-directly-tooan-application-as-an-administrator"></a>Přiřadit ke skupině přímo tooan aplikace jako správce

tooassign jeden nebo více skupin tooan aplikace přímo, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.

7.  Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.

8.  Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.

9.  Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.

10. Typ v hello **název úplné skupiny** hello skupiny, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Pozastavte ukazatel myši nad hello **skupiny** v seznamu tooreveal hello **políčko**. Klikněte na tlačítko hello políčko další toohello skupiny profilu fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli příliš**přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole a klikněte na zaškrtávací políčko tooadd hello této skupiny toohello **vybrané** seznamu.

13. Po dokončení výběru skupiny klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.

14. **Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect toohello tooassign role skupinách vybrali.

15. Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybrané skupiny.

Pokud je nakonfigurován zřizování a pro aplikace je již spuštěn, noví uživatelé, obsažené v rámci skupiny hello musí být zřízená tooan aplikace v přibližně 10 minut. Zkontrolujte hello **protokoly auditu** podrobnosti.

>[!IMPORTANT]
>Zřizování hello název skupiny a skupina podrobnosti v přidání toohello členy, pokud pro některé aplikace podporován. Můžete povolit nebo zakázat tuto funkci povolením nebo zakázáním hello **mapování** pro objekty skupiny uvedené v hello **zřizování** kartě. 
>
>

Pokud zřizování skupiny je povolena, ujistěte se, že tooreview hello atribut mapování tooensure na odpovídající pole se používá pro hello "odpovídající ID". To může být hello zobrazované jméno nebo e-mailu alias, protože hello skupiny a její členy nelze zřídit Pokud hello odpovídající vlastnost není prázdný, nebo není vyplněný pro skupinu ve službě Azure AD.

## <a name="next-steps"></a>Další kroky
[Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory](active-directory-saas-app-provisioning.md)
