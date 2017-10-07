---
title: "Azure Active Directory Domain Services: Správa DNS v doménách spravovaných | Microsoft Docs"
description: "Spravovat DNS v doménách spravovaných Azure Active Directory Domain Services"
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
ms.openlocfilehash: f2085283649eadd3c9e89f708b0eecf10b2d7d70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Spravovat DNS na spravované doméně služby Azure AD Domain Services
Azure Active Directory Domain Services zahrnuje server DNS (překlad názvu domény), který poskytuje překlad názvů DNS pro hello spravované domény. V některých případech může být nutné tooconfigure DNS na hello spravované domény. Může být nutné toocreate záznamy DNS pro počítače, které nejsou připojené k toohello domény nakonfigurovat virtuální IP adresy pro vyrovnávání zatížení nebo nastavit externí servery DNS pro předávání. Z tohoto důvodu jsou uživatelé, kteří patří skupiny 'AAD řadič domény Administrators' toohello udělena oprávnění správy DNS na hello spravované domény.

## <a name="before-you-begin"></a>Než začnete
úlohy hello tooperform uvedené v tomto článku, budete potřebovat:

1. Platná **předplatné**.
2. **Adresář Azure AD** – buď synchronizovány s místní adresář nebo výhradně cloudový adresář.
3. **Azure AD Domain Services** musí být povolen pro adresář hello Azure AD. Pokud jste tak dosud neučinili, postupujte podle kroků uvedených v hello všechny hello úlohy [příručce Začínáme](active-directory-ds-getting-started.md).
4. A **virtuální počítač připojený k doméně** z které můžete spravovat hello spravované doméně služby Azure AD Domain Services. Pokud nemáte virtuálního počítače, postupujte podle všechny hello úkoly popsané v článku hello s názvem [připojit k spravované doméně systému Windows virtuálního počítače tooa](active-directory-ds-admin-guide-join-windows-vm.md).
5. Potřebujete přihlašovací údaje hello **uživatele účtu patřící toohello 'správci AAD řadič domény, skupiny** ve vašem adresáři tooadminister DNS vaší spravované domény.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-tooremotely-administer-dns-for-hello-managed-domain"></a>Úloha 1 - zřídit virtuální počítač připojený k doméně tooremotely spravovat DNS pro spravované doméně hello
Spravované domény služby Azure AD Domain Services můžete spravovat vzdáleně pomocí známých nástrojů pro správu služby Active Directory například hello správy Center Active Directory (ADAC) nebo AD PowerShell. Podobně DNS pro spravované doméně hello lze spravovat vzdáleně pomocí nástrojů pro správu serveru DNS hello.

Správci v adresáři služby Azure AD nemají oprávnění tooconnect toodomain řadiče na spravované doméně hello přes vzdálenou plochu. Členové skupiny hello 'Administrators AAD řadič domény, mohou spravovat DNS pro spravované domény vzdáleně pomocí nástrojů DNS serveru z počítače systému Windows Server nebo klienta, který je připojený k toohello spravované domény. Nástroje pro DNS Server můžete nainstalovat jako součást hello vzdálenou správu serveru (RSAT) volitelná funkce v systému Windows Server a klientských počítačů připojených k toohello spravované domény.

první úlohou Hello je tooprovision virtuálního počítače Windows serveru, který je připojený k toohello spravované domény. Pokyny naleznete v článku toohello s názvem [k doméně systému Windows Server virtuálního počítače tooan Azure AD Domain Services spravované](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-dns-server-tools-on-hello-virtual-machine"></a>Úloha 2 – nástroje pro instalaci serveru DNS na virtuálním počítači hello
Proveďte následující kroky nástroje pro správu DNS hello tooinstall na virtuální počítač připojený k doméně hello hello. Další informace o [instalaci a použití nástrojů pro vzdálenou správu serveru](https://technet.microsoft.com/library/hh831501.aspx), najdete v článku webu Technet.

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
9. Na hello **funkce** klikněte na tlačítko tooexpand hello **nástrojů pro vzdálenou správu serveru** uzel a potom klikněte na tlačítko tooexpand hello **nástroje pro správu rolí** uzlu. Vyberte **nástroje serveru DNS** funkci ze seznamu hello nástroje pro správu rolí.

    ![Stránka funkce](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)
10. Na hello **potvrzení** klikněte na tlačítko **nainstalovat** nástroje tooinstall hello DNS Server pro funkce na hello virtuálního počítače. Po úspěšném dokončení instalace funkce klikněte na tlačítko **Zavřít** tooexit hello **přidat role a funkce** průvodce.

    ![Stránka potvrzení](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)

## <a name="task-3---launch-hello-dns-management-console-tooadminister-dns"></a>Úloha 3 – spuštění hello DNS správy konzoly tooadminister DNS
Nyní, že nástroje serveru DNS hello funkce se instaluje na hello virtuální počítač připojený k doméně, budeme moci použít hello DNS nástroje tooadminister DNS na hello spravované domény.

> [!NOTE]
> Je nutné toobe člen skupiny hello 'AAD řadič domény Administrators', tooadminister DNS na hello spravované domény.
>
>

1. Hello úvodní obrazovce klikněte na tlačítko **nástroje pro správu**. Měli byste vidět hello **DNS** konzola nainstalovaná na hello virtuálního počítače.

    ![Nástroje pro správu - konzolu DNS](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)
2. Klikněte na tlačítko **DNS** konzoly pro správu DNS toolaunch hello.
3. V hello **připojit tooDNS Server** dialogové okno, klikněte na možnost hello s názvem **hello následující počítače**a zadejte název domény DNS hello hello spravované domény (například "contoso100.com").

    ![Konzolu DNS - připojení toodomain](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)
4. Hello konzolu DNS připojí toohello spravované domény.

    ![Konzolu DNS - spravovat domény](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)
5. Nyní můžete vytvořit záznamy DNS hello DNS konzoly tooadd pro počítače v rámci hello virtuální sítě, ve kterém jste povolili služby AAD Domain Services.

> [!WARNING]
> Buďte opatrní při správě DNS pro hello spravované doméně pomocí nástroje pro správu DNS. Ujistěte se, že jste **odstranit nebo upravit hello předdefinované záznamy DNS, které jsou používané službami domény v doméně hello**. Předdefinované záznamy DNS zahrnují záznamy DNS domény, záznamy názvového serveru a další záznamy použít pro umístění řadiče domény. Pokud upravíte tyto záznamy, dojde k narušení služby domain services ve virtuální síti hello.
>
>

V tématu hello [nástrojů DNS článku na webu Technet](https://technet.microsoft.com/library/cc753579.aspx) pro další informace o správě DNS.

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Připojení k systému Windows Server virtuálního počítače tooan Azure AD Domain Services spravované doméně](active-directory-ds-admin-guide-join-windows-vm.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Nástroje pro správu DNS](https://technet.microsoft.com/library/cc753579.aspx)
