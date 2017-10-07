---
title: "Azure Active Directory Domain Services: Správa zásad skupiny na spravovaných domén | Microsoft Docs"
description: "Zásady skupiny Správa v Azure Active Directory Domain Services spravované domény"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: d56ebf27e3015a7f3385ac5a4ddd77ea2c88387f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="administer-group-policy-on-an-azure-ad-domain-services-managed-domain"></a>Správa zásad skupiny na spravované doméně služby Azure AD Domain Services
Azure Active Directory Domain Services zahrnuje předdefinované objekty zásad skupiny (GPO) pro kontejnery hello 'AADDC uživatele' a "AADDC počítače". Tyto integrované tooconfigure objekty zásad skupiny Zásady skupiny ve spravované doméně hello můžete přizpůsobit. Kromě toho členy skupiny "AAD řadič domény Administrators" hello, můžete vytvořit své vlastní vlastní organizační jednotky ve spravované doméně hello. Můžete také vytvořit vlastní objekty zásad skupiny a propojit je toothese vlastní organizační jednotky. Uživatelé, kteří patří skupiny toohello 'AAD řadič domény Administrators, jsou udělena oprávnění správy zásad skupiny na hello spravované domény.

## <a name="before-you-begin"></a>Než začnete
úlohy hello tooperform uvedené v tomto článku, budete potřebovat:

1. Platná **předplatné**.
2. **Adresář Azure AD** – buď synchronizovány s místní adresář nebo výhradně cloudový adresář.
3. **Azure AD Domain Services** musí být povolen pro adresář hello Azure AD. Pokud jste tak dosud neučinili, postupujte podle kroků uvedených v hello všechny hello úlohy [příručce Začínáme](active-directory-ds-getting-started.md).
4. A **virtuální počítač připojený k doméně** z které můžete spravovat hello spravované doméně služby Azure AD Domain Services. Pokud nemáte virtuálního počítače, postupujte podle všechny hello úkoly popsané v článku hello s názvem [připojit k spravované doméně systému Windows virtuálního počítače tooa](active-directory-ds-admin-guide-join-windows-vm.md).
5. Potřebujete přihlašovací údaje hello **uživatele účtu patřící toohello 'správci AAD řadič domény, skupiny** ve vašem adresáři tooadminister zásad skupiny pro vaší spravované domény.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-tooremotely-administer-group-policy-for-hello-managed-domain"></a>Úloha 1 - zřídit virtuální počítač připojený k doméně tooremotely Správa zásad skupiny pro spravované doméně hello
Spravované domény služby Azure AD Domain Services můžete spravovat vzdáleně pomocí známých nástrojů pro správu služby Active Directory například hello správy Center Active Directory (ADAC) nebo AD PowerShell. Podobně zásad skupiny pro spravované doméně hello lze spravovat vzdáleně pomocí nástrojů pro správu zásad skupiny hello.

Správci v adresáři služby Azure AD nemají oprávnění tooconnect toodomain řadiče na spravované doméně hello přes vzdálenou plochu. Členové skupiny hello 'Administrators AAD řadič domény, mohou spravovat zásad skupiny pro spravované domény vzdáleně. Používání nástrojů zásad skupiny na spravované doméně počítač připojený k toohello Windows Server nebo klienta. Jako součást hello volitelnou funkci Správa zásad skupiny na Windows Server a klientských počítačů připojených k spravované doméně toohello se dá nainstalovat nástroje zásad skupiny.

první úlohou Hello je tooprovision virtuálního počítače Windows serveru, který je připojený k toohello spravované domény. Pokyny naleznete v článku toohello s názvem [k doméně systému Windows Server virtuálního počítače tooan Azure AD Domain Services spravované](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-group-policy-tools-on-hello-virtual-machine"></a>Úloha 2 – zásad skupiny instalace nástroje na hello virtuálního počítače
Proveďte následující kroky nástroje pro správu zásad skupiny hello tooinstall na virtuální počítač připojený k doméně hello hello.

1. Přejděte příliš**virtuální počítače** uzlu v hello portál Azure classic. Vyberte virtuální počítač hello jste vytvořili v úloze 1 a klikněte na tlačítko **Connect** na panelu příkazů hello v hello dolní části okna hello.

    ![Připojit tooWindows virtuálního počítače](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. portál classic Hello vás vyzve k tooopen nebo uložit soubor s příponou '.rdp', což je použité tooconnect toohello virtuálního počítače. Po dokončení stahování, klikněte na soubor hello.
3. Hello řádku přihlášení použijte hello přihlašovací údaje uživatele, skupiny toohello 'AAD řadič domény Administrators, které patří. Například používáme 'bob@domainservicespreview.onmicrosoft.com' v našem případě.
4. Hello úvodní obrazovce otevřete **správce serveru**. Klikněte na tlačítko **přidat role a funkce** v hello centrální podokně okna Správce serveru hello.

    ![Spusťte správce serveru na virtuálním počítači](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Na hello **než začnete** stránku hello **Průvodce přidáním rolí a funkcí**, klikněte na tlačítko **Další**.

    ![Před zahájením stránku](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Na hello **typ instalace** ponechte hello **instalace na základě rolí nebo na základě funkcí** zaškrtnuto políčko a klikněte na tlačítko **Další**.

    ![Stránka Typ instalace](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Na hello **výběr serveru** vyberte hello aktuální virtuální počítač z fondu serverů hello a klikněte na tlačítko **Další**.

    ![Stránka Výběr serveru](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Na hello **role serveru** klikněte na tlačítko **Další**. Tuto stránku jsme vynechat, protože jsme nejsou instalaci rolí na hello server.
9. Na hello **funkce** stránky, vyberte hello **Správa zásad skupiny** funkce.

    ![Stránka funkce](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management.png)
10. Na hello **potvrzení** klikněte na tlačítko **nainstalovat** tooinstall hello funkci Správa zásad skupiny na virtuálním počítači hello. Po úspěšném dokončení instalace funkce klikněte na tlačítko **Zavřít** tooexit hello **přidat role a funkce** průvodce.

    ![Stránka potvrzení](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management-confirmation.png)

## <a name="task-3---launch-hello-group-policy-management-console-tooadminister-group-policy"></a>Úloha 3 – spuštění hello zásad skupiny správy konzoly tooadminister zásad skupiny
Můžete použít konzolu pro správu zásad skupiny hello na virtuální počítač připojený k doméně tooadminister hello zásady skupiny ve spravované doméně hello.

> [!NOTE]
> Je nutné toobe členem skupiny hello 'AAD řadič domény správci', tooadminister zásady skupiny ve spravované doméně hello.
>
>

1. Hello úvodní obrazovce klikněte na tlačítko **nástroje pro správu**. Měli byste vidět hello **Správa zásad skupiny** konzola nainstalovaná na hello virtuálního počítače.

    ![Správa zásad skupiny spuštění](./media/active-directory-domain-services-admin-guide/gp-management-installed.png)
2. Klikněte na tlačítko **Správa zásad skupiny** konzoly pro správu zásad skupiny toolaunch hello.

    ![Konzola zásad skupiny](./media/active-directory-domain-services-admin-guide/gp-management-console.png)

## <a name="task-4---customize-built-in-group-policy-objects"></a>Úloha 4: Přizpůsobení předdefinované objekty zásad skupiny
Existují dvě předdefinované objekty zásad skupiny (GPO) – jeden pro hello 'AADDC počítačů' a 'AADDC uživatele' kontejnery v vaší spravované domény. Můžete upravit zásady skupiny pro tooconfigure tyto objekty zásad skupiny ve spravované doméně hello.

1. V hello **Správa zásad skupiny** konzoly, klikněte na tlačítko tooexpand hello **doménové struktury: contoso100.com** a **domén** uzly toosee hello zásady skupiny vaší spravované domény.

    ![Předdefinované objekty zásad skupiny](./media/active-directory-domain-services-admin-guide/builtin-gpos.png)
2. Tyto integrované zásady skupiny tooconfigure objekty zásad skupiny můžete přizpůsobit na vaší spravované domény. Klikněte pravým tlačítkem na objekt zásad skupiny hello a klikněte na tlačítko **upravit...**  toocustomize hello předdefinovaných objektů zásad skupiny. Nástroj Configuration Editor zásad skupiny Hello umožňuje vám toocustomize hello objektu zásad skupiny.

    ![Upravit integrované objekt zásad skupiny](./media/active-directory-domain-services-admin-guide/edit-builtin-gpo.png)
3. Teď můžete použít hello **Editor správy zásad skupiny** konzole tooedit hello předdefinovaných objektů zásad skupiny. Například hello následující snímek obrazovky ukazuje, jak toocustomize hello předdefinované 'AADDC počítače, objekt zásad skupiny.

    ![Přizpůsobení objektu zásad skupiny](./media/active-directory-domain-services-admin-guide/gp-editor.png)

## <a name="task-5---create-a-custom-group-policy-object-gpo"></a>Úloha 5 – vytvoření vlastní objekt zásad skupiny (GPO)
Můžete vytvořit nebo importovat vlastní objekty zásad skupiny vlastní. Můžete také propojit vlastní objekty zásad skupiny tooa vlastní organizační jednotky, které jste vytvořili v vaší spravované domény. Další informace o vytvoření vlastní organizační jednotky, najdete v části [spravované domény vytvořit vlastní](active-directory-ds-admin-guide-create-ou.md).

> [!NOTE]
> Je nutné toobe členem skupiny hello 'AAD řadič domény správci', tooadminister zásady skupiny ve spravované doméně hello.
>
>

1. V hello **Správa zásad skupiny** konzoly, klikněte na tlačítko tooselect vlastní organizační jednotky (OU). Klikněte pravým tlačítkem na hello organizační jednotku a klikněte na tlačítko **vytvořit objekt zásad skupiny v této doméně a propojit jej sem...** .

    ![Vytvořit vlastní objekt zásad skupiny](./media/active-directory-domain-services-admin-guide/gp-create-gpo.png)
2. Zadejte název pro nový objekt zásad skupiny hello a klikněte na tlačítko **OK**.

    ![Zadejte název pro objekt zásad skupiny](./media/active-directory-domain-services-admin-guide/gp-specify-gpo-name.png)
3. Nový objekt zásad skupiny se vytvoří a propojené tooyour vlastní organizační jednotce. Klikněte pravým tlačítkem na objekt zásad skupiny hello a klikněte na tlačítko **upravit...**  nabídce hello.

    ![Nově vytvořeného objektu zásad skupiny](./media/active-directory-domain-services-admin-guide/gp-gpo-created.png)
4. Můžete přizpůsobit hello nově vytvořený objekt zásad skupiny pomocí hello **Editor správy zásad skupiny**.

    ![Přizpůsobení nový objekt zásad skupiny](./media/active-directory-domain-services-admin-guide/gp-customize-gpo.png)


Další informace o používání [konzoly pro správu zásad skupiny](https://technet.microsoft.com/library/cc753298.aspx) je k dispozici na webu Technet.

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Připojení k systému Windows Server virtuálního počítače tooan Azure AD Domain Services spravované doméně](active-directory-ds-admin-guide-join-windows-vm.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Konzola pro správu zásad skupiny](https://technet.microsoft.com/library/cc753298.aspx)
