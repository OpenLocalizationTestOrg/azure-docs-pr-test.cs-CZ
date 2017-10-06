---
title: "Azure Active Directory Domain Services: Správa spravované domény | Microsoft Docs"
description: "Správa spravované domény Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4fdbc75-3e6b-4e20-8494-5dcc3bf2220a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 11acc79e06163e3193b1aa981f2cd28af812789d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Správa spravované domény služby Azure Active Directory Domain Services
Tento článek ukazuje, jak tooadminister doméně služby Azure Active Directory (AD) spravované domény.

## <a name="before-you-begin"></a>Než začnete
úlohy hello tooperform uvedené v tomto článku, budete potřebovat:

1. Platná **předplatné**.
2. **Adresář Azure AD** – buď synchronizovány s místní adresář nebo výhradně cloudový adresář.
3. **Azure AD Domain Services** musí být povolen pro adresář hello Azure AD. Pokud jste tak dosud neučinili, postupujte podle kroků uvedených v hello všechny hello úlohy [příručce Začínáme](active-directory-ds-getting-started.md).
4. A **virtuální počítač připojený k doméně** z které můžete spravovat hello spravované doméně služby Azure AD Domain Services. Pokud nemáte virtuálního počítače, postupujte podle všechny hello úkoly popsané v článku hello s názvem [připojit k spravované doméně systému Windows virtuálního počítače tooa](active-directory-ds-admin-guide-join-windows-vm.md).
5. Potřebujete přihlašovací údaje hello **uživatele účtu patřící toohello 'správci AAD řadič domény, skupiny** ve vašem adresáři tooadminister vaší spravované domény.

<br>

## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Úlohy správy, které můžete provádět na spravované doméně
Členům skupiny hello 'Administrators AAD řadič domény, jsou udělena oprávnění na hello spravované domény, které umožňují je například tooperform úlohy:

* Připojení počítače toohello spravované domény.
* Nakonfigurujte hello předdefinovaných objektů zásad skupiny pro kontejnery 'AADDC počítačů' a 'AADDC uživatele' hello v hello spravované domény.
* Spravovat DNS na hello spravované domény.
* Vytvořit a spravovat vlastní organizační jednotky (OU) hello spravované domény.
* Získat přístup pro správu toocomputers připojený toohello spravované domény.

## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Oprávnění správce nemáte na spravované doméně
Hello domény spravuje Microsoft, včetně aktivit, jako je například opravy, monitorování a provádění zálohování. Proto je uzamčené hello domény a nemáte oprávnění tooperform určitých úloh správy v doméně hello. Níže jsou příklady úloh, které nelze provést.

* Nejsou udělena oprávnění správce domény nebo správce podnikové sítě pro hello spravované domény.
* Nelze rozšířit schéma hello hello spravované domény.
* Nemůžete se připojit toodomain řadiče pro hello spravované doméně pomocí vzdálené plochy.
* Nelze přidat toohello řadiče domény, spravované domény.

## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-tooremotely-administer-hello-managed-domain"></a>Úloha 1 - zřídit tooremotely virtuální počítač připojený k doméně systému Windows Server spravovat hello spravované doméně
Spravované domény služby Azure AD Domain Services můžete spravovat pomocí známých nástrojů pro správu služby Active Directory jako hello správy Center Active Directory (ADAC) nebo AD PowerShell. Správci klientů nemají oprávnění tooconnect toodomain řadiče na spravované doméně hello přes vzdálenou plochu. Proto členy hello, které můžete spravovat skupiny, Administrators AAD řadič domény, spravované domény vzdáleně pomocí nástroje pro správu AD z počítače systému Windows Server nebo klienta, který je připojený k toohello spravované domény. Nástroje pro správu služby AD může být nainstalována jako součást hello vzdálenou správu serveru (RSAT) volitelná funkce v systému Windows Server a klientských počítačů připojených k toohello spravované domény.

prvním krokem Hello je tooset virtuálního počítače Windows serveru, který je připojený k toohello spravované domény. Pokyny naleznete v článku toohello s názvem [k doméně systému Windows Server virtuálního počítače tooan Azure AD Domain Services spravované](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-hello-managed-domain-from-a-client-computer-for-example-windows-10"></a>Vzdálená správa spravované domény hello z klientského počítače (například Windows 10)
Hello pokyny v tomto článku hello tooadminister virtuálního počítače Windows serveru AAD DS spravované domény. Ale můžete také toouse toodo virtuálního počítače klienta (například Windows 10) Windows proto.

Můžete [nainstalovat vzdálenou správu serveru (RSAT)](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) na virtuálním počítači klienta Windows podle pokynů hello na webu TechNet.

## <a name="task-2---install-active-directory-administration-tools-on-hello-virtual-machine"></a>Úloha 2 – nástroje pro správu instalace služby Active Directory na virtuálním počítači hello
Proveďte následující kroky nástroje pro správu služby Active Directory hello tooinstall na virtuální počítač připojený k doméně hello hello. Další informace najdete v článku Technet [informace o instalaci a použití nástrojů pro vzdálenou správu serveru](https://technet.microsoft.com/library/hh831501.aspx).

1. Přejděte příliš**virtuální počítače** uzlu v hello portál Azure classic. Vyberte virtuální počítač hello jste vytvořili v úloze 1 a klikněte na tlačítko **Connect** na panelu příkazů hello v hello dolní části okna hello.

    ![Připojit tooWindows virtuálního počítače](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. portál classic Hello vás vyzve k tooopen nebo uložit soubor s příponou '.rdp', což je použité tooconnect toohello virtuálního počítače. Po dokončení stahování, klikněte na tlačítko tooopen hello souboru.
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
9. Na hello **funkce** klikněte na tlačítko tooexpand hello **nástrojů pro vzdálenou správu serveru** uzel a potom klikněte na tlačítko tooexpand hello **nástroje pro správu rolí** uzlu. Vyberte **služby AD DS a AD LDS nástroje** funkci ze seznamu hello nástroje pro správu rolí.

    ![Stránka funkce](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)
10. Na hello **potvrzení** klikněte na tlačítko **nainstalovat** tooinstall hello AD a nástroje služby AD LDS funkce na hello virtuálního počítače. Po úspěšném dokončení instalace funkce klikněte na tlačítko **Zavřít** tooexit hello **přidat role a funkce** průvodce.

    ![Stránka potvrzení](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)

## <a name="task-3---connect-tooand-explore-hello-managed-domain"></a>Úloha 3 – připojit tooand prozkoumat hello spravované doméně
Teď, když jsou nainstalovány nástroje pro správu hello AD na hello připojený k doméně virtuálního počítače, můžeme používat tyto nástroje tooexplore a spravovat hello spravované domény.

> [!NOTE]
> Je třeba toobe členem skupiny "Administrators AAD řadiče domény" hello, tooadminister hello spravované domény.
>
>

1. Hello úvodní obrazovce klikněte na tlačítko **nástroje pro správu**. Měli byste vidět hello AD nástroje pro správu nainstalovaná hello virtuálního počítače.

    ![Nástroje pro správu nainstalován na serveru](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Klikněte na tlačítko **Centrum správy služby Active Directory**.

    ![Centrum správy služby Active Directory](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. tooexplore hello domény, klikněte na název domény hello v levém podokně hello (například "contoso100.com"). Všimněte si dvě kontejnery označované jako 'AADDC počítače' a 'AADDC uživatele'.

    ![Centrum ADAC - zobrazení domény](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)
4. Klikněte na tlačítko hello kontejneru s názvem **AADDC uživatelé** toosee všichni uživatelé a skupiny, které patří toohello spravované domény. Měli byste vidět uživatelských účtů a skupin ze služby Azure AD klienta zobrazit v tomto kontejneru. Všimněte si v tomto příkladu, uživatelský účet pro uživatele hello názvem "bob" a skupinu s názvem "Správci AAD řadič domény, jsou k dispozici v tomto kontejneru.

    ![Centrum ADAC - uživatelé domény](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)
5. Klikněte na tlačítko hello kontejneru s názvem **AADDC počítače** toosee hello počítače připojené k spravované doméně toothis. Měli byste vidět položku pro hello aktuální virtuální počítač, který je připojený k toohello domény. Účty počítačů pro všechny počítače, které jsou připojené k toohello spravované doméně jsou uloženy v tomto kontejneru 'AADDC počítače, Azure AD Domain Services.

    ![Centrum ADAC - počítačů připojených k doméně](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Připojení k systému Windows Server virtuálního počítače tooan Azure AD Domain Services spravované doméně](active-directory-ds-admin-guide-join-windows-vm.md)
* [Nasazení nástroje pro správu vzdáleného serveru](https://technet.microsoft.com/library/hh831501.aspx)
