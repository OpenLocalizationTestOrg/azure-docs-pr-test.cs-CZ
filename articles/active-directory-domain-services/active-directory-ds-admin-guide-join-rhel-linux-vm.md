---
title: "Azure Active Directory Domain Services: Připojení virtuálních počítačů systému RHEL ke spravované doméně | Microsoft Docs"
description: "Připojit virtuální počítač Red Hat Enterprise Linux k Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 87291c47-1280-43f8-8fb2-da1bd61a4942
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 69f1850bfed90392e9a4695e2443ffaa6bfc746d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Připojte virtuální počítač Red Hat Enterprise Linux 7 ke spravované doméně
Tento článek ukazuje, jak připojit virtuální počítač Red Hat Enterprise Linux (RHEL) 7 k spravované doméně služby Azure AD Domain Services.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Zřídit virtuální počítač Red Hat Enterprise Linux
Proveďte následující kroky pro zřízení virtuálního počítače RHEL 7 pomocí portálu Azure.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

    ![Řídicí panel portálu Azure](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. Klikněte na tlačítko **nový** v levém podokně a typ **Red Hat** do panelu Hledat, jak je znázorněno na následujícím snímku obrazovky. Položky pro Red Hat Enterprise Linux se zobrazí ve výsledcích hledání. Klikněte na tlačítko **Red Hat Enterprise Linux 7.2**.

    ![Ve výsledcích vyberte RHEL](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. Výsledkem hledání **všechno, co** podokně by měl seznamu bitovou kopii Red Hat Enterprise Linux 7.2. Klikněte na tlačítko **Red Hat Enterprise Linux 7.2** zobrazíte další informace o bitové kopie virtuálního počítače.

    ![Ve výsledcích vyberte RHEL](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. V **Red Hat Enterprise Linux 7.2** podokně zobrazí další informace o bitové kopie virtuálního počítače. V **vybrat model nasazení** rozevíracího seznamu, vyberte **Classic**. Klikněte **vytvořit** tlačítko.

    ![Zobrazit podrobnosti image.](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. V **Základy** stránky **vytvořit virtuální počítač** průvodce, zadejte **název hostitele** pro nový virtuální počítač. Také zadat uživatelské jméno místního správce ve **uživatelské jméno** pole a **heslo**. Můžete také použít klíč SSH k ověření uživatele místního správce. Také vybrat **cenová úroveň** pro virtuální počítač.

    ![Vytvoření virtuálního počítače – základy stránky](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. V **velikost** stránky **vytvořit virtuální počítač** průvodce, vyberte velikost virtuálního počítače.

    ![Vytvoření virtuálního počítače – vyberte velikost](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. V **nastavení** stránky **vytvořit virtuální počítač** průvodce, vyberte účet úložiště pro virtuální počítač. Klikněte na tlačítko **virtuální sítě** k vyberte virtuální síť, ke které by měly být nasazeny virtuálního počítače s Linuxem. V **virtuální sítě** okně, vyberte virtuální síť, ve které Azure AD Domain Services je k dispozici. V tomto příkladu vybereme 'MyPreviewVNet' virtuální sítě.

    ![Vytvoření virtuálního počítače – výběr virtuální sítě](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. Na **Souhrn** stránky **vytvořit virtuální počítač** průvodce zkontrolujte a klikněte na tlačítko **OK** tlačítko.

    ![Vytvoření virtuálního počítače - vybranou virtuální síť](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. Nasazení nového virtuálního počítače na základě bitové kopie systému RHEL 7.2 by se měl spustit.

    ![Vytvoření virtuálního počítače - nasazení bylo zahájeno](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. Po několika minutách musí být virtuální počítač nasazené úspěšně a připravené k použití.

    ![Vytvoření virtuálního počítače – nasazení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Vzdáleně připojit k nově zřízeného virtuálního počítače systému Linux
Virtuální počítač RHEL 7.2 zřízená v Azure. Dalším krokem je vzdáleně připojit k virtuálnímu počítači.

**Připojit k virtuálnímu počítači systému RHEL 7.2** postupujte podle pokynů v článku [přihlášení do virtuálního počítače se systémem Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Další kroky předpokládají, že používáte pro připojení k virtuálnímu počítači systému RHEL klienta PuTTY SSH. Další informace najdete v tématu [stránku položek ke stažení PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Otevřete PuTTY program.
2. Zadejte **název hostitele** pro nově vytvořený virtuální počítač RHEL. V tomto příkladu naše virtuální počítač má název hostitele 'contoso-rhel.cloudapp .net'. Pokud si nejste jisti název hostitele virtuálního počítače, podívejte se na řídicím panelu virtuálního počítače na portálu Azure.

    ![Připojit puTTY](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. Přihlaste se k virtuálnímu počítači pomocí přihlašovacích údajů místního správce, které jste zadali, vytvoření virtuálního počítače. V tomto příkladu jsme použili účet místního správce "mahesh".

    ![PuTTY přihlášení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Nainstalujte požadované balíčky Linux virtuálního počítače
Po připojení k virtuálnímu počítači, je dalším krokem instalace balíčky požadované pro připojení k doméně na virtuálním počítači. Proveďte následující kroky:

1. **Nainstalujte realmd:** realmd balíčku se používá pro připojení k doméně. V terminálu PuTTY zadejte následující příkaz:

    sudo yum instalace realmd

    ![Nainstalujte realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Po několika minutách by měly získat balíček realmd nainstalovány na virtuálním počítači.

    ![realmd nainstalován](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. **Nainstalujte sssd:** závislý balíček realmd sssd k provádění operací připojení k doméně. V terminálu PuTTY zadejte následující příkaz:

    sudo yum instalace sssd

    ![Nainstalujte sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Po několika minutách by měly získat balíček sssd nainstalovány na virtuálním počítači.

    ![realmd nainstalován](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. **Instalaci protokolu kerberos:** v PuTTY terminálu, zadejte následující příkaz:

    sudo yum instalace krb5 – pracovní stanice krb5-knihovny

    ![Instalaci protokolu kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Po několika minutách by měly získat balíček realmd nainstalovány na virtuálním počítači.

    ![Nainstalovat pomocí protokolu Kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Připojit virtuální počítač Linux k spravované doméně
Teď, když požadované balíčky jsou nainstalovány na virtuální počítač Linux, dalším úkolem je připojení virtuálního počítače k spravované doméně.

1. Zjistit spravované doméně služby AAD Domain Services. V terminálu PuTTY zadejte následující příkaz:

    sudo sféry zjistit CONTOSO100.COM

    ![Zjišťování sféry](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Pokud **zjišťování sféry** se nepodařilo najít vaší spravované domény, ujistěte se, že doména je dostupný z virtuálního počítače (zkuste ping). Ujistěte se také, že virtuální počítač skutečně byla nasazena do stejné virtuální síti, ve kterém je k dispozici spravované domény.
2. Inicializace protokolu kerberos. V terminálu PuTTY zadejte následující příkaz. Ujistěte se, že zadáváte uživatel, který patří do skupiny "Administrators AAD řadič domény. Pouze tito uživatelé se může připojit počítače k spravované doméně.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Zkontrolujte, že zadáte název domény velkými písmeny else kinit selže.
3. Připojení počítače k doméně. V terminálu PuTTY zadejte následující příkaz. Zadejte stejný uživatel, který jste zadali v předchozím kroku (kinit).

    připojení k sféry sudo – podrobné CONTOSO100.COM -U 'bob@CONTOSO100.COM.

    ![Sféra spojení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Měli byste obdržet zprávu ("úspěšně zaregistrované počítače ve sféře") Pokud je počítač úspěšně připojený k spravované doméně.

## <a name="verify-domain-join"></a>Ověřte připojení k doméně
Můžete rychle ověřit, zda je počítač byl úspěšně připojen k spravované doméně. Připojení k nově virtuálních počítačů systému RHEL pomocí SSH a uživatelský účet domény a zkontrolujte Pokud uživatelský účet vyřešen správně připojený k doméně.

1. V terminálu PuTTY zadejte následující příkaz pro připojení k nově RHEL virtuálního počítače pomocí protokolu SSH připojený k doméně. Použít účet domény, který patří k spravované doméně (například "bob@CONTOSO100.COM' v takovém případě.)

    SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net
2. V terminálu PuTTY zadejte následující příkaz zobrazit, pokud byl domovský adresář správně inicializován.

    PWD
3. V terminálu PuTTY zadejte následující příkaz zobrazit, pokud členství ve skupinách jsou řešeny správně.

    id

Následuje ukázkový výstup z těchto příkazů:

![Ověřte připojení k doméně](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a>Řešení potíží s připojení k doméně
Odkazovat [připojení k doméně Poradce při potížích s](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) článku.

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Připojení virtuálního počítače s Windows serverem k spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Jak se přihlásit do virtuálního počítače se systémem Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Instalaci protokolu Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 – Příručka pro integraci systému Windows](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
