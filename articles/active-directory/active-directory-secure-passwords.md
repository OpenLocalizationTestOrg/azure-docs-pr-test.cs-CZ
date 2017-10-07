---
title: "aaaAzure AD vrstvené heslo zabezpečení | Microsoft Docs"
description: "Vysvětluje, jak Azure AD vynucuje silná hesla a chrání hesla uživatelů z zločinci,"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: joflore
ms.openlocfilehash: 10d8b600d9f4c02355b2cd8c5dccf8505aaf210d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="a-multi-tiered-approach-tooazure-ad-password-security"></a>Zabezpečení přístupu víceúrovňových tooAzure AD heslo

Tento článek popisuje některé z osvědčených postupů můžete postupovat podle uživatele nebo tooprotect správce Azure Active Directory (Azure AD) nebo Account Microsoft.

 > [!NOTE]
 > Správci služby Azure AD můžete resetovat hesla uživatelů podle pokynů hello v článku hello [resetovat hello heslo pro uživatele v Azure Active Directory](active-directory-users-reset-password-azure-portal.md).
 >
 > Uživatelům můžete resetovat vlastní heslo pomocí hello pokyny v článku hello [nápovědy zapomenuté heslo Azure AD](active-directory-passwords-update-your-own-password.md).
 >

## <a name="password-requirements"></a>Požadavky na heslo

Azure AD zahrnuje hello následující běžné přístupy toosecuring hesla:

* Požadavky na délku hesla
* Požadavky na složitost hesla
* Pravidelné a opakované vypršení platnosti hesla

Informace o vytvoření nového ve službě Azure Active Directory najdete v tématu hello [Azure AD samoobslužného resetování hesla pro odborníky v oblasti IT hello](active-directory-passwords.md).

## <a name="azure-ad-password-protections"></a>Ochrana hesla služby Azure AD

Azure AD a hello systémem účtů Microsoft použít odvětví ověřené blíží tooensure zabezpečení ochranu uživatele a správce hesel, včetně:

* Dynamicky zakázaná hesla
* Inteligentní uzamčení hesla

Informace o správě hesel podle aktuální research, najdete v dokumentu White Paper hello [heslo pokyny](http://aka.ms/passwordguidance).

### <a name="dynamically-banned-passwords"></a>Dynamicky zakázaná hesla

Azure AD a Accounts Microsoft ochrana heslem zabezpečit pomocí dynamicky zakazování běžně používané hesla. tým ochrany identit Azure ID Hello pravidelně analyzuje seznamů zakázaných heslo, brání uživatelům v výběr běžně používané hesla. Tato služba je k dispozici tooAzure AD a Zákazníci využívající službu účet Microsoft hello.

Při vytváření hesel, je důležité správci tooencourage uživatelé toochoose heslo vět, které zahrnují jedinečnou kombinaci písmena, čísla, znaky nebo slova. Tento přístup pomáhá téměř nemožné toobe ohrožení zabezpečení, ale usnadňuje uživatelům tooremember toomake uživatelská hesla.

#### <a name="password-breaches"></a>Narušení heslo

Společnost Microsoft pracuje vždy toostay jeden krok před zločinci.

tým Azure AD Identity Protection Hello průběžně analyzuje hesla, které se běžně používají. Zločinci také použít podobné tooinform strategie jejich útoky, jako je například vytváření [duhové tabulky](https://en.wikipedia.org/wiki/Rainbow_table) pro krakování hodnot hash hesel.

Společnost Microsoft průběžně analyzuje [úniky dat](https://www.privacyrights.org/data-breaches) toomaintain seznam dynamicky aktualizované zakázaného heslo, které zajišťuje, že citlivé hesla jsou zakázané dřív, než narostou reálné hrozby tooAzure AD zákazníků. Další informace o našich aktuální úsilí zabezpečení najdete v tématu hello [sestavy Intelligence zabezpečení Microsoft](https://www.microsoft.com/security/sir/default.aspx).

### <a name="smart-password-lockout"></a>Inteligentní uzamčení hesla

Pokud Azure AD zjistí potenciální snažíme toohack internetový trestního do heslo uživatele, jsme uzamčení hello uživatelský účet s inteligentní uzamčení heslo. Azure AD je navržený tak, toodetermine hello riziko spojené s konkrétní přihlašovací relace. Potom pomocí hello nejaktuálnější data zabezpečení, použijeme uzamčení sémantiku toostop internetových hrozeb.

Pokud se uživatel uzamkne mimo Azure AD, jejich obrazovky vypadá podobně jako toohello, ten, který následuje:

  ![Uzamčení a vyloučení z Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)

Pro ostatní účty Microsoft jejich obrazovky vypadá podobně jako toohello, ten, který následuje:

  ![Uzamčení a vyloučení z účtu Microsoft](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Informace o vytvoření nového ve službě Azure Active Directory najdete v tématu hello [Azure AD samoobslužného resetování hesla pro odborníky v oblasti IT hello](active-directory-passwords.md).

  >[!NOTE]
  >Pokud jste správce Azure AD, může být vhodné toouse [Windows Hello](https://www.microsoft.com/windows/windows-hello) tooavoid s uživatelům vytvářet zcela tradiční hesla.
  >

## <a name="next-steps"></a>Další kroky

* [Jak tooupdate své vlastní heslo](active-directory-passwords-update-your-own-password.md)
* [Hello základy Azure identity management.](fundamentals-identity.md)
* [Aktivita resetování zprávu o heslo](active-directory-passwords-reporting.md)


