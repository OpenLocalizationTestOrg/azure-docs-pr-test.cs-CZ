---
title: "Azure Active Directory Domain Services: Příručka pro správu | Microsoft Docs"
description: "Vytvoření organizační jednotce (OU) v doménách spravovaných Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: ce7539e5d5c7c1bf9505ef229f2d31d84c00da05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Vytvoření organizační jednotce (OU) na spravované doméně služby Azure AD Domain Services
Azure AD Domain Services spravovaných domén obsahovat dvě předdefinované kontejnery označované jako 'AADDC počítače' a 'AADDC uživatele'. Hello AADDC počítače, který má kontejneru objektů počítače pro všechny počítače, které jsou připojené k spravované doméně toohello. kontejner 'AADDC uživatele' Hello zahrnuje uživatelů a skupin v klientovi Azure AD hello. V některých případech může být nutné toocreate účty služby na hello spravované domény toodeploy úlohy. Pro tento účel můžete vytvořit vlastní organizační jednotce (OU) ve spravované doméně hello a vytvořte účty služby v rámci dané organizační jednotce. Tento článek ukazuje, jak toocreate organizační jednotky v vaší spravované domény.

## <a name="before-you-begin"></a>Než začnete
úlohy hello tooperform uvedené v tomto článku, budete potřebovat:

1. Platná **předplatné**.
2. **Adresář Azure AD** – buď synchronizovány s místní adresář nebo výhradně cloudový adresář.
3. **Azure AD Domain Services** musí být povolen pro adresář hello Azure AD. Pokud jste tak dosud neučinili, postupujte podle kroků uvedených v hello všechny hello úlohy [příručce Začínáme](active-directory-ds-getting-started.md).
4. Virtuální počítač připojený k doméně z níž spravujete hello Azure AD Domain Services spravované domény. Pokud nemáte virtuálního počítače, postupujte podle všechny hello úkoly popsané v článku hello s názvem [připojit k spravované doméně systému Windows virtuálního počítače tooa](active-directory-ds-admin-guide-join-windows-vm.md).
5. Potřebujete přihlašovací údaje hello **uživatele účtu patřící toohello 'správci AAD řadič domény, skupiny** ve vašem adresáři toocreate vlastní organizační jednotku na vaší spravované domény.

## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Nainstalujte nástroje pro správu AD na virtuální počítač připojený k doméně pro vzdálenou správu
Spravované domény služby Azure AD Domain Services můžete spravovat vzdáleně pomocí známých nástrojů pro správu služby Active Directory například hello správy Center Active Directory (ADAC) nebo AD PowerShell. Správci klientů nemají oprávnění tooconnect toodomain řadiče na spravované doméně hello přes vzdálenou plochu. tooadminister hello spravované domény, nainstalujte funkci nástroje pro správu hello AD spravované doméně připojené k toohello virtuálního počítače. Najdete v článku toohello s názvem [spravovat spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md) pokyny.

## <a name="create-an-organizational-unit-on-hello-managed-domain"></a>Vytvořit organizační jednotku v hello spravované doméně
Teď, když jsou nainstalovány nástroje pro správu hello AD na hello připojený k doméně virtuálního počítače, můžeme použít tyto nástroje toocreate s organizační jednotkou na hello spravované domény. Proveďte hello následující kroky:

> [!NOTE]
> Pouze členové skupiny hello 'AAD řadič domény správci, mají hello toocreate požadovaná oprávnění vlastní organizační jednotce. Ujistěte se, že provedete následující kroky jako uživatel, který patří skupině toothis hello.
>
>

1. Hello úvodní obrazovce klikněte na tlačítko **nástroje pro správu**. Měli byste vidět hello AD nástroje pro správu nainstalovaná hello virtuálního počítače.

    ![Nástroje pro správu nainstalován na serveru](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Klikněte na tlačítko **Centrum správy služby Active Directory**.

    ![Centrum správy služby Active Directory](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. tooview hello domény, klikněte na název domény hello v levém podokně hello (například "contoso100.com").

    ![Centrum ADAC - zobrazení domény](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)
4. Na pravé straně hello **úlohy** podokně klikněte na tlačítko **nový** uzlu hello název domény. V tomto příkladu jsme klikněte na **nový** pod uzlem "contoso100(local)" hello na pravé straně hello **úlohy** podokně.

    ![Centrum ADAC - nové organizační jednotky](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)
5. Měli byste vidět hello možnost toocreate organizační jednotku. Klikněte na tlačítko **organizační jednotky** toolaunch hello **vytvořit organizační jednotku** dialogové okno.
6. V hello **vytvořit organizační jednotku** dialogové okno, zadejte **název** pro hello nové organizační jednotky. Zadejte krátký popis hello organizační jednotky. Můžete také nastavit hello **správce** pole pro hello organizační jednotky. toocreate hello vlastní organizační jednotku, klikněte na tlačítko **OK**.

    ![Centrum ADAC - vytvořit organizační jednotku dialogové okno](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)
7. nově vytvořený organizační jednotky Hello by se měla zobrazit v hello AD Centra správy (ADAC).

    ![Centrum ADAC - vytvořit organizační jednotku](./media/active-directory-domain-services-admin-guide/create-ou-done.png)

## <a name="permissionssecurity-for-newly-created-ous"></a>Oprávnění zabezpečení pro nově vytvořený organizační jednotky
Ve výchozím nastavení hello hello uživatele (člen skupiny hello 'AAD řadič domény Administrators'), který vytvořil hello vlastní organizační jednotky jsou udělena oprávnění pro správu (Úplné řízení) přes organizační jednotky. Hello uživatele můžete pokračovat a udělit oprávnění tooother uživatele nebo skupinu toohello 'správci AAD řadič domény, podle potřeby. Jak je vidět v hello následující snímek obrazovky, hello uživatele 'bob@domainservicespreview.onmicrosoft.com, kdo vytvořený hello nové 'MyCustomOU' organizační jednotka je oprávnění k úplnému řízení nad ním.

 ![Centrum ADAC - nového zabezpečení organizační jednotky](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)

## <a name="notes-on-administering-custom-ous"></a>Poznámky k správě vlastní organizační jednotky
Teď, když jste vytvořili vlastní organizační jednotky, můžete ihned začít a vytvořit uživatele, skupiny, počítače a účty služby v této organizační jednotce. Nelze přesunout uživatele nebo skupiny z hello 'AADDC uživatele' organizační jednotky toocustom organizační jednotky.

> [!WARNING]
> Uživatelské účty, skupiny, účty služeb a objekty počítače, které vytvoříte, vlastní organizačních jednotek nejsou k dispozici v klientovi služby Azure AD. Jinými slovy tyto objekty nezobrazovat díky hello Azure AD Graph API nebo v hello uživatelského rozhraní Azure AD. Tyto objekty jsou k dispozici pouze ve vaší spravované doméně služby Azure AD Domain Services.
>
>

## <a name="related-content"></a>Související obsah
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Konfigurace zásad skupiny na spravované doméně](active-directory-ds-admin-guide-administer-group-policy.md)
* [Centrum správy služby Active Directory: Začínáme](https://technet.microsoft.com/library/dd560651.aspx)
* [Podrobný průvodce účty služby](https://technet.microsoft.com/library/dd548356.aspx)
