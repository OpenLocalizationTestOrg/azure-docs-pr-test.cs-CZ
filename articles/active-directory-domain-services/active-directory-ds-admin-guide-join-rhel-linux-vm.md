---
title: "Azure Active Directory Domain Services: Připojení k spravované doméně virtuálních počítačů systému RHEL tooa | Microsoft Docs"
description: "Připojit virtuální počítač Red Hat Enterprise Linux tooAzure AD Domain Services"
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
ms.openlocfilehash: 41ca2aaf2eefbf9c403d2b834d61a1aa0943d950
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-tooa-managed-domain"></a>Připojení k spravované doméně tooa virtuální počítač Red Hat Enterprise Linux 7
Tento článek ukazuje, jak toojoin tooan virtuální počítač Red Hat Enterprise Linux (RHEL) 7 Azure AD Domain Services spravované domény.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Zřídit virtuální počítač Red Hat Enterprise Linux
Proveďte následující kroky tooprovision RHEL 7 virtuálního počítače pomocí portálu Azure hello hello.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

    ![Řídicí panel portálu Azure](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. Klikněte na tlačítko **nový** na hello levé podokno a typ **Red Hat** do panelu Hledat text hello, jak ukazuje následující snímek obrazovky hello. Položky pro Red Hat Enterprise Linux se zobrazí ve výsledcích hledání hello. Klikněte na tlačítko **Red Hat Enterprise Linux 7.2**.

    ![Ve výsledcích vyberte RHEL](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. výsledky hledání Hello v hello **všechno, co** podokně má seznam image hello Red Hat Enterprise Linux 7.2. Klikněte na tlačítko **Red Hat Enterprise Linux 7.2** tooview Další informace o bitové kopie virtuálního počítače hello.

    ![Ve výsledcích vyberte RHEL](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. V hello **Red Hat Enterprise Linux 7.2** podokně zobrazí další informace o bitové kopie virtuálního počítače hello. V hello **vybrat model nasazení** rozevíracího seznamu, vyberte **Classic**. Pak klikněte na tlačítko hello **vytvořit** tlačítko.

    ![Zobrazit podrobnosti image.](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. V hello **Základy** stránku hello **vytvořit virtuální počítač** průvodce, zadejte hello **název hostitele** pro hello nového virtuálního počítače. Také zadejte uživatelské jméno místního správce v hello **uživatelské jméno** pole a **heslo**. Můžete také zvolit toouse uživatel SSH klíče tooauthenticate hello místního správce. Také vybrat **cenová úroveň** hello virtuálního počítače.

    ![Vytvoření virtuálního počítače – základy stránky](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. V hello **velikost** stránku hello **vytvořit virtuální počítač** průvodce, vyberte hello velikost hello virtuálního počítače.

    ![Vytvoření virtuálního počítače – vyberte velikost](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. V hello **nastavení** stránku hello **vytvořit virtuální počítač** hello průvodce, vyberte účet úložiště pro virtuální počítač hello. Klikněte na tlačítko **virtuální sítě** tooselect hello virtuální sítě toowhich hello by měl být nasazen virtuální počítač s Linuxem. V hello **virtuální sítě** okně, vyberte hello virtuální síť, ve kterém je dostupná služba Azure AD Domain Services. V tomto příkladu vybereme hello 'MyPreviewVNet' virtuální sítě.

    ![Vytvoření virtuálního počítače – výběr virtuální sítě](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. Na hello **Souhrn** stránku hello **vytvořit virtuální počítač** průvodce zkontrolujte a klikněte na tlačítko hello **OK** tlačítko.

    ![Vytvoření virtuálního počítače - vybranou virtuální síť](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. Nasazení hello nového virtuálního počítače na základě bitové kopie hello RHEL 7.2 by se měl spustit.

    ![Vytvoření virtuálního počítače - nasazení bylo zahájeno](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. Po několika minutách by měl být hello virtuální počítač úspěšně nasazen a připraven k použití.

    ![Vytvoření virtuálního počítače – nasazení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-toohello-newly-provisioned-linux-virtual-machine"></a>Vzdálené připojení toohello nově zřízený Linux virtuálního počítače
virtuální počítač Hello RHEL 7.2 zřízená v Azure. Další úlohou Hello je tooconnect vzdáleně toohello virtuálního počítače.

**Připojit virtuální počítač toohello RHEL 7.2** hello podle pokynů v článku hello [jak toolog na tooa virtuální počítač se systémem Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Hello zbytek hello kroky předpokládají, že používáte hello PuTTY SSH tooconnect toohello RHEL virtuálního klienta. Další informace najdete v tématu hello [stránku položek ke stažení PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Otevřete hello PuTTY program.
2. Zadejte hello **název hostitele** pro nově vytvořený virtuální počítač RHEL hello. V tomto příkladu naše virtuální počítač má název hostitele hello 'contoso-rhel.cloudapp .net'. Pokud si nejste jisti hello název hostitele virtuálního počítače, naleznete v řídicí panel toohello virtuálních počítačů na hello portálu Azure.

    ![Připojit puTTY](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. Přihlaste se pomocí přihlašovacích údajů místního správce hello k, které jste zadali při vytváření virtuálního počítače hello toohello virtuálního počítače. V tomto příkladu jsme použili účet místního správce hello "mahesh".

    ![PuTTY přihlášení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-hello-linux-virtual-machine"></a>Nainstalujte požadované balíčky hello Linux virtuálního počítače
Po připojování toohello virtuálního počítače je dalším úkolem hello tooinstall balíčky požadované pro připojení k doméně hello virtuálního počítače. Proveďte hello následující kroky:

1. **Nainstalujte realmd:** hello realmd balíčku se používá pro připojení k doméně. V terminálu PuTTY zadejte hello následující příkaz:

    sudo yum instalace realmd

    ![Nainstalujte realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Po několika minutách by měl získat balíček realmd hello nainstalovaná hello virtuálního počítače.

    ![realmd nainstalován](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. **Nainstalujte sssd:** hello realmd balíček závisí na operace sssd tooperform domény spojení. V terminálu PuTTY zadejte hello následující příkaz:

    sudo yum instalace sssd

    ![Nainstalujte sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Po několika minutách by měl získat balíček sssd hello nainstalovaná hello virtuálního počítače.

    ![realmd nainstalován](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. **Instalaci protokolu kerberos:** PuTTY terminálu, zadejte následující příkaz hello:

    sudo yum instalace krb5 – pracovní stanice krb5-knihovny

    ![Instalaci protokolu kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Po několika minutách by měl získat balíček realmd hello nainstalovaná hello virtuálního počítače.

    ![Nainstalovat pomocí protokolu Kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-hello-linux-virtual-machine-toohello-managed-domain"></a>Připojení hello Linux virtuálního počítače toohello spravované doméně
Teď, když hello požadované balíčky jsou nainstalovány na hello Linux virtuálního počítače, dalším úkolem hello je toojoin hello virtuálního počítače toohello spravované domény.

1. Zjistit hello služby AAD Domain Services spravované domény. V terminálu PuTTY zadejte hello následující příkaz:

    sudo sféry zjistit CONTOSO100.COM

    ![Zjišťování sféry](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Pokud **zjišťování sféry** je nelze toofind vaší spravované domény, zkontrolujte tuto doménu hello je dostupný z hello virtuálního počítače (zkuste ping). Také zajistěte, aby hello virtuální počítač byl skutečně nasazené toohello stejnou virtuální síť, ve které hello je k dispozici spravované domény.
2. Inicializace protokolu kerberos. V terminálu PuTTY zadejte následující příkaz hello. Ujistěte se, že zadáváte uživatel, který patří skupině toohello 'AAD řadič domény Administrators'. Pouze tito uživatelé mohou připojit počítače toohello spravované domény.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Zkontrolujte, že zadáte název domény hello velkými písmeny else kinit selže.
3. Připojit k doméně toohello počítač hello. V terminálu PuTTY zadejte následující příkaz hello. Zadejte hello stejného uživatele, které jste zadali v předchozím kroku (kinit) hello.

    připojení k sféry sudo – podrobné CONTOSO100.COM -U 'bob@CONTOSO100.COM.

    ![Sféra spojení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Měli byste obdržet zprávu ("úspěšně zaregistrované počítače ve sféře") při hello počítač se úspěšně připojil toohello spravované domény.

## <a name="verify-domain-join"></a>Ověřte připojení k doméně
Můžete rychle ověřit, zda text hello počítače se převzaly služby úspěšně připojil toohello spravované domény. Připojit toohello nově virtuálních počítačů systému RHEL pomocí SSH a uživatelský účet domény a pak zaškrtněte toosee, pokud je uživatelský účet hello analyzovat správně připojený k doméně.

1. V terminálu, PuTTY hello zadejte následující příkaz tooconnect toohello nově připojené do domény RHEL virtuálního počítače pomocí protokolu SSH. Použít účet domény, který je součástí spravované domény toohello (například "bob@CONTOSO100.COM' v takovém případě.)

    SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net
2. PuTTY terminálu zadejte následující příkaz toosee-li hello domovský adresář byl inicializován správně hello.

    PWD
3. V terminálu PuTTY zadejte následující příkaz toosee-li hello členství ve skupinách jsou správně přeloženy hello.

    id

Následuje ukázkový výstup z těchto příkazů:

![Ověřte připojení k doméně](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a>Řešení potíží s připojení k doméně
Odkazovat toohello [připojení k doméně Poradce při potížích s](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) článku.

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Připojení k systému Windows Server virtuálního počítače tooan Azure AD Domain Services spravované doméně](active-directory-ds-admin-guide-join-windows-vm.md)
* [Jak toolog na tooa virtuální počítač se systémem Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Instalaci protokolu Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 – Příručka pro integraci systému Windows](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
