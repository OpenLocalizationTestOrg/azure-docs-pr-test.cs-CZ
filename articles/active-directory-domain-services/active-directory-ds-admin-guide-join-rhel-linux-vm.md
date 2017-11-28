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
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-tooa-managed-domain"></a><span data-ttu-id="22f2b-103">Připojení k spravované doméně tooa virtuální počítač Red Hat Enterprise Linux 7</span><span class="sxs-lookup"><span data-stu-id="22f2b-103">Join a Red Hat Enterprise Linux 7 virtual machine tooa managed domain</span></span>
<span data-ttu-id="22f2b-104">Tento článek ukazuje, jak toojoin tooan virtuální počítač Red Hat Enterprise Linux (RHEL) 7 Azure AD Domain Services spravované domény.</span><span class="sxs-lookup"><span data-stu-id="22f2b-104">This article shows you how toojoin a Red Hat Enterprise Linux (RHEL) 7 virtual machine tooan Azure AD Domain Services managed domain.</span></span>

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a><span data-ttu-id="22f2b-105">Zřídit virtuální počítač Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="22f2b-105">Provision a Red Hat Enterprise Linux virtual machine</span></span>
<span data-ttu-id="22f2b-106">Proveďte následující kroky tooprovision RHEL 7 virtuálního počítače pomocí portálu Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-106">Perform hello following steps tooprovision a RHEL 7 virtual machine using hello Azure portal.</span></span>

1. <span data-ttu-id="22f2b-107">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="22f2b-107">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

    ![Řídicí panel portálu Azure](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. <span data-ttu-id="22f2b-109">Klikněte na tlačítko **nový** na hello levé podokno a typ **Red Hat** do panelu Hledat text hello, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-109">Click **New** on hello left pane and type **Red Hat** into hello search bar as shown in hello following screenshot.</span></span> <span data-ttu-id="22f2b-110">Položky pro Red Hat Enterprise Linux se zobrazí ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-110">Entries for Red Hat Enterprise Linux appear in hello search results.</span></span> <span data-ttu-id="22f2b-111">Klikněte na tlačítko **Red Hat Enterprise Linux 7.2**.</span><span class="sxs-lookup"><span data-stu-id="22f2b-111">Click **Red Hat Enterprise Linux 7.2**.</span></span>

    ![Ve výsledcích vyberte RHEL](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. <span data-ttu-id="22f2b-113">výsledky hledání Hello v hello **všechno, co** podokně má seznam image hello Red Hat Enterprise Linux 7.2.</span><span class="sxs-lookup"><span data-stu-id="22f2b-113">hello search results in hello **Everything** pane should list hello Red Hat Enterprise Linux 7.2 image.</span></span> <span data-ttu-id="22f2b-114">Klikněte na tlačítko **Red Hat Enterprise Linux 7.2** tooview Další informace o bitové kopie virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-114">Click **Red Hat Enterprise Linux 7.2** tooview more information about hello virtual machine image.</span></span>

    ![Ve výsledcích vyberte RHEL](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. <span data-ttu-id="22f2b-116">V hello **Red Hat Enterprise Linux 7.2** podokně zobrazí další informace o bitové kopie virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-116">In hello **Red Hat Enterprise Linux 7.2** pane, you should see more information about hello virtual machine image.</span></span> <span data-ttu-id="22f2b-117">V hello **vybrat model nasazení** rozevíracího seznamu, vyberte **Classic**.</span><span class="sxs-lookup"><span data-stu-id="22f2b-117">In hello **Select a deployment model** dropdown, select **Classic**.</span></span> <span data-ttu-id="22f2b-118">Pak klikněte na tlačítko hello **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22f2b-118">Then click hello **Create** button.</span></span>

    ![Zobrazit podrobnosti image.](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. <span data-ttu-id="22f2b-120">V hello **Základy** stránku hello **vytvořit virtuální počítač** průvodce, zadejte hello **název hostitele** pro hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22f2b-120">In hello **Basics** page of hello **Create virtual machine** wizard, enter hello **Host Name** for hello new virtual machine.</span></span> <span data-ttu-id="22f2b-121">Také zadejte uživatelské jméno místního správce v hello **uživatelské jméno** pole a **heslo**.</span><span class="sxs-lookup"><span data-stu-id="22f2b-121">Also specify a local administrator user name in hello **User name** field and a **Password**.</span></span> <span data-ttu-id="22f2b-122">Můžete také zvolit toouse uživatel SSH klíče tooauthenticate hello místního správce.</span><span class="sxs-lookup"><span data-stu-id="22f2b-122">You may also choose toouse an SSH key tooauthenticate hello local administrator user.</span></span> <span data-ttu-id="22f2b-123">Také vybrat **cenová úroveň** hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22f2b-123">Also select a **Pricing Tier** for hello virtual machine.</span></span>

    ![Vytvoření virtuálního počítače – základy stránky](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. <span data-ttu-id="22f2b-125">V hello **velikost** stránku hello **vytvořit virtuální počítač** průvodce, vyberte hello velikost hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22f2b-125">In hello **Size** page of hello **Create virtual machine** wizard, select hello size for hello virtual machine.</span></span>

    ![Vytvoření virtuálního počítače – vyberte velikost](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. <span data-ttu-id="22f2b-127">V hello **nastavení** stránku hello **vytvořit virtuální počítač** hello průvodce, vyberte účet úložiště pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-127">In hello **Settings** page of hello **Create virtual machine** wizard, select hello storage account for hello virtual machine.</span></span> <span data-ttu-id="22f2b-128">Klikněte na tlačítko **virtuální sítě** tooselect hello virtuální sítě toowhich hello by měl být nasazen virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="22f2b-128">Click **Virtual Network** tooselect hello virtual network toowhich hello Linux VM should be deployed.</span></span> <span data-ttu-id="22f2b-129">V hello **virtuální sítě** okně, vyberte hello virtuální síť, ve kterém je dostupná služba Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="22f2b-129">In hello **Virtual Network** blade, select hello virtual network in which Azure AD Domain Services is available.</span></span> <span data-ttu-id="22f2b-130">V tomto příkladu vybereme hello 'MyPreviewVNet' virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="22f2b-130">In this example, we pick hello 'MyPreviewVNet' virtual network.</span></span>

    ![Vytvoření virtuálního počítače – výběr virtuální sítě](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. <span data-ttu-id="22f2b-132">Na hello **Souhrn** stránku hello **vytvořit virtuální počítač** průvodce zkontrolujte a klikněte na tlačítko hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22f2b-132">On hello **Summary** page of hello **Create virtual machine** wizard, review and click hello **OK** button.</span></span>

    ![Vytvoření virtuálního počítače - vybranou virtuální síť](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. <span data-ttu-id="22f2b-134">Nasazení hello nového virtuálního počítače na základě bitové kopie hello RHEL 7.2 by se měl spustit.</span><span class="sxs-lookup"><span data-stu-id="22f2b-134">Deployment of hello new virtual machine based on hello RHEL 7.2 image should start.</span></span>

    ![Vytvoření virtuálního počítače - nasazení bylo zahájeno](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. <span data-ttu-id="22f2b-136">Po několika minutách by měl být hello virtuální počítač úspěšně nasazen a připraven k použití.</span><span class="sxs-lookup"><span data-stu-id="22f2b-136">After a few minutes, hello virtual machine should be deployed successfully and ready for use.</span></span>

    ![Vytvoření virtuálního počítače – nasazení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-toohello-newly-provisioned-linux-virtual-machine"></a><span data-ttu-id="22f2b-138">Vzdálené připojení toohello nově zřízený Linux virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="22f2b-138">Connect remotely toohello newly provisioned Linux virtual machine</span></span>
<span data-ttu-id="22f2b-139">virtuální počítač Hello RHEL 7.2 zřízená v Azure.</span><span class="sxs-lookup"><span data-stu-id="22f2b-139">hello RHEL 7.2 virtual machine has been provisioned in Azure.</span></span> <span data-ttu-id="22f2b-140">Další úlohou Hello je tooconnect vzdáleně toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22f2b-140">hello next task is tooconnect remotely toohello virtual machine.</span></span>

<span data-ttu-id="22f2b-141">**Připojit virtuální počítač toohello RHEL 7.2** hello podle pokynů v článku hello [jak toolog na tooa virtuální počítač se systémem Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="22f2b-141">**Connect toohello RHEL 7.2 virtual machine** Follow hello instructions in hello article [How toolog on tooa virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="22f2b-142">Hello zbytek hello kroky předpokládají, že používáte hello PuTTY SSH tooconnect toohello RHEL virtuálního klienta.</span><span class="sxs-lookup"><span data-stu-id="22f2b-142">hello rest of hello steps assume you use hello PuTTY SSH client tooconnect toohello RHEL virtual machine.</span></span> <span data-ttu-id="22f2b-143">Další informace najdete v tématu hello [stránku položek ke stažení PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="22f2b-143">For more information, see hello [PuTTY Download page](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

1. <span data-ttu-id="22f2b-144">Otevřete hello PuTTY program.</span><span class="sxs-lookup"><span data-stu-id="22f2b-144">Open hello PuTTY program.</span></span>
2. <span data-ttu-id="22f2b-145">Zadejte hello **název hostitele** pro nově vytvořený virtuální počítač RHEL hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-145">Enter hello **Host Name** for hello newly created RHEL virtual machine.</span></span> <span data-ttu-id="22f2b-146">V tomto příkladu naše virtuální počítač má název hostitele hello 'contoso-rhel.cloudapp .net'.</span><span class="sxs-lookup"><span data-stu-id="22f2b-146">In this example, our virtual machine has hello host name 'contoso-rhel.cloudapp.net'.</span></span> <span data-ttu-id="22f2b-147">Pokud si nejste jisti hello název hostitele virtuálního počítače, naleznete v řídicí panel toohello virtuálních počítačů na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="22f2b-147">If you are not sure of hello host name of your VM, refer toohello VM dashboard on hello Azure portal.</span></span>

    ![Připojit puTTY](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. <span data-ttu-id="22f2b-149">Přihlaste se pomocí přihlašovacích údajů místního správce hello k, které jste zadali při vytváření virtuálního počítače hello toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22f2b-149">Log on toohello virtual machine using hello local administrator credentials you specified when hello virtual machine was created.</span></span> <span data-ttu-id="22f2b-150">V tomto příkladu jsme použili účet místního správce hello "mahesh".</span><span class="sxs-lookup"><span data-stu-id="22f2b-150">In this example, we used hello local administrator account "mahesh".</span></span>

    ![PuTTY přihlášení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-hello-linux-virtual-machine"></a><span data-ttu-id="22f2b-152">Nainstalujte požadované balíčky hello Linux virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="22f2b-152">Install required packages on hello Linux virtual machine</span></span>
<span data-ttu-id="22f2b-153">Po připojování toohello virtuálního počítače je dalším úkolem hello tooinstall balíčky požadované pro připojení k doméně hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22f2b-153">After connecting toohello virtual machine, hello next task is tooinstall packages required for domain join on hello virtual machine.</span></span> <span data-ttu-id="22f2b-154">Proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="22f2b-154">Perform hello following steps:</span></span>

1. <span data-ttu-id="22f2b-155">**Nainstalujte realmd:** hello realmd balíčku se používá pro připojení k doméně.</span><span class="sxs-lookup"><span data-stu-id="22f2b-155">**Install realmd:** hello realmd package is used for domain join.</span></span> <span data-ttu-id="22f2b-156">V terminálu PuTTY zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="22f2b-156">In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="22f2b-157">sudo yum instalace realmd</span><span class="sxs-lookup"><span data-stu-id="22f2b-157">sudo yum install realmd</span></span>

    ![Nainstalujte realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    <span data-ttu-id="22f2b-159">Po několika minutách by měl získat balíček realmd hello nainstalovaná hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22f2b-159">After a few minutes, hello realmd package should get installed on hello virtual machine.</span></span>

    ![realmd nainstalován](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. <span data-ttu-id="22f2b-161">**Nainstalujte sssd:** hello realmd balíček závisí na operace sssd tooperform domény spojení.</span><span class="sxs-lookup"><span data-stu-id="22f2b-161">**Install sssd:** hello realmd package depends on sssd tooperform domain join operations.</span></span> <span data-ttu-id="22f2b-162">V terminálu PuTTY zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="22f2b-162">In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="22f2b-163">sudo yum instalace sssd</span><span class="sxs-lookup"><span data-stu-id="22f2b-163">sudo yum install sssd</span></span>

    ![Nainstalujte sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    <span data-ttu-id="22f2b-165">Po několika minutách by měl získat balíček sssd hello nainstalovaná hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22f2b-165">After a few minutes, hello sssd package should get installed on hello virtual machine.</span></span>

    ![realmd nainstalován](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. <span data-ttu-id="22f2b-167">**Instalaci protokolu kerberos:** PuTTY terminálu, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="22f2b-167">**Install kerberos:** In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="22f2b-168">sudo yum instalace krb5 – pracovní stanice krb5-knihovny</span><span class="sxs-lookup"><span data-stu-id="22f2b-168">sudo yum install krb5-workstation krb5-libs</span></span>

    ![Instalaci protokolu kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    <span data-ttu-id="22f2b-170">Po několika minutách by měl získat balíček realmd hello nainstalovaná hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="22f2b-170">After a few minutes, hello realmd package should get installed on hello virtual machine.</span></span>

    ![Nainstalovat pomocí protokolu Kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-hello-linux-virtual-machine-toohello-managed-domain"></a><span data-ttu-id="22f2b-172">Připojení hello Linux virtuálního počítače toohello spravované doméně</span><span class="sxs-lookup"><span data-stu-id="22f2b-172">Join hello Linux virtual machine toohello managed domain</span></span>
<span data-ttu-id="22f2b-173">Teď, když hello požadované balíčky jsou nainstalovány na hello Linux virtuálního počítače, dalším úkolem hello je toojoin hello virtuálního počítače toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="22f2b-173">Now that hello required packages are installed on hello Linux virtual machine, hello next task is toojoin hello virtual machine toohello managed domain.</span></span>

1. <span data-ttu-id="22f2b-174">Zjistit hello služby AAD Domain Services spravované domény.</span><span class="sxs-lookup"><span data-stu-id="22f2b-174">Discover hello AAD Domain Services managed domain.</span></span> <span data-ttu-id="22f2b-175">V terminálu PuTTY zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="22f2b-175">In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="22f2b-176">sudo sféry zjistit CONTOSO100.COM</span><span class="sxs-lookup"><span data-stu-id="22f2b-176">sudo realm discover CONTOSO100.COM</span></span>

    ![Zjišťování sféry](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    <span data-ttu-id="22f2b-178">Pokud **zjišťování sféry** je nelze toofind vaší spravované domény, zkontrolujte tuto doménu hello je dostupný z hello virtuálního počítače (zkuste ping).</span><span class="sxs-lookup"><span data-stu-id="22f2b-178">If **realm discover** is unable toofind your managed domain, ensure that hello domain is reachable from hello virtual machine (try ping).</span></span> <span data-ttu-id="22f2b-179">Také zajistěte, aby hello virtuální počítač byl skutečně nasazené toohello stejnou virtuální síť, ve které hello je k dispozici spravované domény.</span><span class="sxs-lookup"><span data-stu-id="22f2b-179">Also ensure that hello virtual machine has indeed been deployed toohello same virtual network in which hello managed domain is available.</span></span>
2. <span data-ttu-id="22f2b-180">Inicializace protokolu kerberos.</span><span class="sxs-lookup"><span data-stu-id="22f2b-180">Initialize kerberos.</span></span> <span data-ttu-id="22f2b-181">V terminálu PuTTY zadejte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-181">In your PuTTY terminal, type hello following command.</span></span> <span data-ttu-id="22f2b-182">Ujistěte se, že zadáváte uživatel, který patří skupině toohello 'AAD řadič domény Administrators'.</span><span class="sxs-lookup"><span data-stu-id="22f2b-182">Ensure that you specify a user who belongs toohello 'AAD DC Administrators' group.</span></span> <span data-ttu-id="22f2b-183">Pouze tito uživatelé mohou připojit počítače toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="22f2b-183">Only these users can join computers toohello managed domain.</span></span>

    <span data-ttu-id="22f2b-184">kinitbob@CONTOSO100.COM</span><span class="sxs-lookup"><span data-stu-id="22f2b-184">kinit bob@CONTOSO100.COM</span></span>

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    <span data-ttu-id="22f2b-186">Zkontrolujte, že zadáte název domény hello velkými písmeny else kinit selže.</span><span class="sxs-lookup"><span data-stu-id="22f2b-186">Ensure that you specify hello domain name in capital letters, else kinit fails.</span></span>
3. <span data-ttu-id="22f2b-187">Připojit k doméně toohello počítač hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-187">Join hello machine toohello domain.</span></span> <span data-ttu-id="22f2b-188">V terminálu PuTTY zadejte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-188">In your PuTTY terminal, type hello following command.</span></span> <span data-ttu-id="22f2b-189">Zadejte hello stejného uživatele, které jste zadali v předchozím kroku (kinit) hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-189">Specify hello same user you specified in hello preceding step ('kinit').</span></span>

    <span data-ttu-id="22f2b-190">připojení k sféry sudo – podrobné CONTOSO100.COM -U 'bob@CONTOSO100.COM.</span><span class="sxs-lookup"><span data-stu-id="22f2b-190">sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'</span></span>

    ![Sféra spojení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

<span data-ttu-id="22f2b-192">Měli byste obdržet zprávu ("úspěšně zaregistrované počítače ve sféře") při hello počítač se úspěšně připojil toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="22f2b-192">You should get a message ("Successfully enrolled machine in realm") when hello machine is successfully joined toohello managed domain.</span></span>

## <a name="verify-domain-join"></a><span data-ttu-id="22f2b-193">Ověřte připojení k doméně</span><span class="sxs-lookup"><span data-stu-id="22f2b-193">Verify domain join</span></span>
<span data-ttu-id="22f2b-194">Můžete rychle ověřit, zda text hello počítače se převzaly služby úspěšně připojil toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="22f2b-194">You can quickly verify whether hello machine has been successfully joined toohello managed domain.</span></span> <span data-ttu-id="22f2b-195">Připojit toohello nově virtuálních počítačů systému RHEL pomocí SSH a uživatelský účet domény a pak zaškrtněte toosee, pokud je uživatelský účet hello analyzovat správně připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="22f2b-195">Connect toohello newly domain joined RHEL VM using SSH and a domain user account and then check toosee if hello user account is resolved correctly.</span></span>

1. <span data-ttu-id="22f2b-196">V terminálu, PuTTY hello zadejte následující příkaz tooconnect toohello nově připojené do domény RHEL virtuálního počítače pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="22f2b-196">In your PuTTY terminal, type hello following command tooconnect toohello newly domain joined RHEL virtual machine using SSH.</span></span> <span data-ttu-id="22f2b-197">Použít účet domény, který je součástí spravované domény toohello (například "bob@CONTOSO100.COM' v takovém případě.)</span><span class="sxs-lookup"><span data-stu-id="22f2b-197">Use a domain account that belongs toohello managed domain (for example, 'bob@CONTOSO100.COM' in this case.)</span></span>

    <span data-ttu-id="22f2b-198">SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="22f2b-198">ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net</span></span>
2. <span data-ttu-id="22f2b-199">PuTTY terminálu zadejte následující příkaz toosee-li hello domovský adresář byl inicializován správně hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-199">In your PuTTY terminal, type hello following command toosee if hello home directory was initialized correctly.</span></span>

    <span data-ttu-id="22f2b-200">PWD</span><span class="sxs-lookup"><span data-stu-id="22f2b-200">pwd</span></span>
3. <span data-ttu-id="22f2b-201">V terminálu PuTTY zadejte následující příkaz toosee-li hello členství ve skupinách jsou správně přeloženy hello.</span><span class="sxs-lookup"><span data-stu-id="22f2b-201">In your PuTTY terminal, type hello following command toosee if hello group memberships are being resolved correctly.</span></span>

    <span data-ttu-id="22f2b-202">id</span><span class="sxs-lookup"><span data-stu-id="22f2b-202">id</span></span>

<span data-ttu-id="22f2b-203">Následuje ukázkový výstup z těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="22f2b-203">A sample output of these commands follows:</span></span>

![Ověřte připojení k doméně](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a><span data-ttu-id="22f2b-205">Řešení potíží s připojení k doméně</span><span class="sxs-lookup"><span data-stu-id="22f2b-205">Troubleshooting domain join</span></span>
<span data-ttu-id="22f2b-206">Odkazovat toohello [připojení k doméně Poradce při potížích s](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) článku.</span><span class="sxs-lookup"><span data-stu-id="22f2b-206">Refer toohello [Troubleshooting domain join](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) article.</span></span>

## <a name="related-content"></a><span data-ttu-id="22f2b-207">Související obsah</span><span class="sxs-lookup"><span data-stu-id="22f2b-207">Related Content</span></span>
* [<span data-ttu-id="22f2b-208">Azure AD Domain Services – Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="22f2b-208">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="22f2b-209">Připojení k systému Windows Server virtuálního počítače tooan Azure AD Domain Services spravované doméně</span><span class="sxs-lookup"><span data-stu-id="22f2b-209">Join a Windows Server virtual machine tooan Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* <span data-ttu-id="22f2b-210">[Jak toolog na tooa virtuální počítač se systémem Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="22f2b-210">[How toolog on tooa virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* [<span data-ttu-id="22f2b-211">Instalaci protokolu Kerberos</span><span class="sxs-lookup"><span data-stu-id="22f2b-211">Installing Kerberos</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [<span data-ttu-id="22f2b-212">Red Hat Enterprise Linux 7 – Příručka pro integraci systému Windows</span><span class="sxs-lookup"><span data-stu-id="22f2b-212">Red Hat Enterprise Linux 7 - Windows Integration Guide</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
