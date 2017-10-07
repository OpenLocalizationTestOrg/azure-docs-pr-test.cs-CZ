---
title: "Azure Active Directory Domain Services: Povolení synchronizace hesel | Dokumentace Microsoftu"
description: "Začínáme se službou Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 8e073df9de2996f5ad159dda746881c014ea3d66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a>Povolit tooAzure synchronizace hesla Active Directory Domain Services
V předchozích úlohách jste povolili službu Azure Active Directory Domain Services pro tenanta služby Azure Active Directory (Azure AD). Další úlohou Hello je, že požadované tooenable synchronizace hodnoty hash přihlašovacích údajů pro ověřování tooAzure NT LAN Manager (NTLM) a protokolu Kerberos AD Domain Services. Po nastavení synchronizace přihlašovacích údajů, uživatelé můžou přihlásit toohello spravované doméně pomocí podnikových přihlašovacích.

potřebný postup Hello se liší pro uživatele jen cloudové účty vs uživatelské účty, které jsou synchronizované z vašeho místního adresáře pomocí služby Azure AD Connect.  Pokud se klientovi služby Azure AD má kombinaci cloudu pouze uživatele a uživatele z místní AD, je nutné tooperform oba kroky.

<br>

> [!div class="op_single_selector"]
> * **Jenom pro cloud uživatelské účty**: [synchronizovat hesla pro cloudové uživatelské účty, tooyour spravované doméně](active-directory-ds-getting-started-password-sync.md)
> * **Místní uživatelské účty**: [synchronizovat hesla uživatelských účtů, které jsou synchronizované z místní AD tooyour spravované domény](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-cloud-only-user-accounts"></a>Úloha 5: povolení tooyour synchronizace hesla pro cloudové uživatelské účty spravované domény.
Uživatelé tooauthenticate na hello spravované domény, Azure Active Directory Domain Services vyžaduje pověření hodnoty hash ve formátu, který je vhodný pro ověřování NTLM a Kerberos. Azure AD nebudou generovat a ukládat hodnoty hash přihlašovacích údajů hello formát, který má požadované pro ověřování protokolem NTLM nebo Kerberos, dokud nepovolíte Azure Active Directory Domain Services pro vašeho klienta. Ze zřejmých bezpečnostních důvodů Azure AD také neukládá přihlašovací hesla jako nešifrovaný text. Azure AD proto nemá tooautomatically způsob, jak vygenerovat tyto protokolu NTLM nebo Kerberos hodnoty hash přihlašovacích údajů podle existující přihlašovací údaje uživatelů.

> [!NOTE]
> Pokud má vaše organizace výhradně cloudový uživatelské účty, uživatele, kteří potřebují toouse Azure Active Directory Domain Services, musíte změnit své heslo. Jenom pro cloud uživatelský účet je účet, který byl vytvořen v adresáři služby Azure AD pomocí portálu Azure hello nebo rutin prostředí Azure AD PowerShell. Takové uživatelské účty se nesynchronizují z místního adresáře.
>
>

Tento proces změny hesla způsobí, že pověření hello hodnoty hash, které jsou vyžadované Azure Active Directory Domain Services pro Azure AD vygeneruje protokolu Kerberos a NTLM toobe ověřování. Můžete buď vyprší hello hesla pro všechny uživatele v hello klientů, kteří potřebují toouse Azure Active Directory Domain Services, nebo instruovat je toochange jejich hesla.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-user-account"></a>Povolení generování hodnot hash přihlašovacích údajů protokolů NTLM a Kerberos u uživatelského účtu jenom cloudu
Zde jsou hello pokyny, které že budete potřebovat tooprovide uživatelům, změnit jejich hesla:

1. Přejděte toohello [přístupový Panel Azure AD](http://myapps.microsoft.com) stránku vaší organizace.

    ![Spusťte přístupový panel Azure AD hello](./media/active-directory-domain-services-getting-started/access-panel.png)

2. V hello pravém horním rohu klikněte na název a vyberte **profil** nabídce hello.

    ![Výběr profilu](./media/active-directory-domain-services-getting-started/select-profile.png)

3. Na hello **profil** klikněte na **změnit heslo**.

    ![Kliknutí na Změnit heslo](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Pokud hello **změnit heslo** možnost není zobrazena v okně přístupový Panel hello, zajistěte, aby vaše organizace má nakonfigurovaný [Správa hesel ve službě Azure AD](../active-directory/active-directory-passwords-getting-started.md).
   >
   >
4. Na hello **změnit heslo** stránky, zadejte stávající (staré) heslo, zadejte nové heslo a potvrďte ho.

    ![Vytvoření virtuální sítě pro službu Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. Klikněte na **Odeslat**.

Několik minut poté, co jste změnili heslo, nové heslo hello je použitelná v Azure Active Directory Domain Services. Za několik minut (obvykle přibližně 20 minut), se můžete přihlásit toocomputers, které jsou připojené k toohello spravované doméně pomocí hello nově změnit heslo.

## <a name="related-content"></a>Související obsah
* [Jak tooupdate své vlastní heslo](../active-directory/active-directory-passwords-update-your-own-password.md)
* [Začínáme se správou hesel v Azure AD](../active-directory/active-directory-passwords-getting-started.md)
* [Povolení synchronizace hesel klienta tooAzure Active Directory Domain Services u synchronizovaného Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Správa spravované domény služby Azure Active Directory Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Připojení k Azure Active Directory Domain Services spravované domény tooan systému Windows virtuálního počítače](active-directory-ds-admin-guide-join-windows-vm.md)
* [Připojení k Red Hat Enterprise Linux virtuálního počítače tooan Azure Active Directory Domain Services spravované doméně](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
