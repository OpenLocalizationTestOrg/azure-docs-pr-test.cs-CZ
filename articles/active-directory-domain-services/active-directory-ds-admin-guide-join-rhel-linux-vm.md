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
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a><span data-ttu-id="e5b5c-103">Připojte virtuální počítač Red Hat Enterprise Linux 7 ke spravované doméně</span><span class="sxs-lookup"><span data-stu-id="e5b5c-103">Join a Red Hat Enterprise Linux 7 virtual machine to a managed domain</span></span>
<span data-ttu-id="e5b5c-104">Tento článek ukazuje, jak připojit virtuální počítač Red Hat Enterprise Linux (RHEL) 7 k spravované doméně služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-104">This article shows you how to join a Red Hat Enterprise Linux (RHEL) 7 virtual machine to an Azure AD Domain Services managed domain.</span></span>

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a><span data-ttu-id="e5b5c-105">Zřídit virtuální počítač Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="e5b5c-105">Provision a Red Hat Enterprise Linux virtual machine</span></span>
<span data-ttu-id="e5b5c-106">Proveďte následující kroky pro zřízení virtuálního počítače RHEL 7 pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-106">Perform the following steps to provision a RHEL 7 virtual machine using the Azure portal.</span></span>

1. <span data-ttu-id="e5b5c-107">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e5b5c-107">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

    ![Řídicí panel portálu Azure](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. <span data-ttu-id="e5b5c-109">Klikněte na tlačítko **nový** v levém podokně a typ **Red Hat** do panelu Hledat, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-109">Click **New** on the left pane and type **Red Hat** into the search bar as shown in the following screenshot.</span></span> <span data-ttu-id="e5b5c-110">Položky pro Red Hat Enterprise Linux se zobrazí ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-110">Entries for Red Hat Enterprise Linux appear in the search results.</span></span> <span data-ttu-id="e5b5c-111">Klikněte na tlačítko **Red Hat Enterprise Linux 7.2**.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-111">Click **Red Hat Enterprise Linux 7.2**.</span></span>

    ![Ve výsledcích vyberte RHEL](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. <span data-ttu-id="e5b5c-113">Výsledkem hledání **všechno, co** podokně by měl seznamu bitovou kopii Red Hat Enterprise Linux 7.2.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-113">The search results in the **Everything** pane should list the Red Hat Enterprise Linux 7.2 image.</span></span> <span data-ttu-id="e5b5c-114">Klikněte na tlačítko **Red Hat Enterprise Linux 7.2** zobrazíte další informace o bitové kopie virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-114">Click **Red Hat Enterprise Linux 7.2** to view more information about the virtual machine image.</span></span>

    ![Ve výsledcích vyberte RHEL](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. <span data-ttu-id="e5b5c-116">V **Red Hat Enterprise Linux 7.2** podokně zobrazí další informace o bitové kopie virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-116">In the **Red Hat Enterprise Linux 7.2** pane, you should see more information about the virtual machine image.</span></span> <span data-ttu-id="e5b5c-117">V **vybrat model nasazení** rozevíracího seznamu, vyberte **Classic**.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-117">In the **Select a deployment model** dropdown, select **Classic**.</span></span> <span data-ttu-id="e5b5c-118">Klikněte **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-118">Then click the **Create** button.</span></span>

    ![Zobrazit podrobnosti image.](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. <span data-ttu-id="e5b5c-120">V **Základy** stránky **vytvořit virtuální počítač** průvodce, zadejte **název hostitele** pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-120">In the **Basics** page of the **Create virtual machine** wizard, enter the **Host Name** for the new virtual machine.</span></span> <span data-ttu-id="e5b5c-121">Také zadat uživatelské jméno místního správce ve **uživatelské jméno** pole a **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-121">Also specify a local administrator user name in the **User name** field and a **Password**.</span></span> <span data-ttu-id="e5b5c-122">Můžete také použít klíč SSH k ověření uživatele místního správce.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-122">You may also choose to use an SSH key to authenticate the local administrator user.</span></span> <span data-ttu-id="e5b5c-123">Také vybrat **cenová úroveň** pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-123">Also select a **Pricing Tier** for the virtual machine.</span></span>

    ![Vytvoření virtuálního počítače – základy stránky](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. <span data-ttu-id="e5b5c-125">V **velikost** stránky **vytvořit virtuální počítač** průvodce, vyberte velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-125">In the **Size** page of the **Create virtual machine** wizard, select the size for the virtual machine.</span></span>

    ![Vytvoření virtuálního počítače – vyberte velikost](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. <span data-ttu-id="e5b5c-127">V **nastavení** stránky **vytvořit virtuální počítač** průvodce, vyberte účet úložiště pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-127">In the **Settings** page of the **Create virtual machine** wizard, select the storage account for the virtual machine.</span></span> <span data-ttu-id="e5b5c-128">Klikněte na tlačítko **virtuální sítě** k vyberte virtuální síť, ke které by měly být nasazeny virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-128">Click **Virtual Network** to select the virtual network to which the Linux VM should be deployed.</span></span> <span data-ttu-id="e5b5c-129">V **virtuální sítě** okně, vyberte virtuální síť, ve které Azure AD Domain Services je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-129">In the **Virtual Network** blade, select the virtual network in which Azure AD Domain Services is available.</span></span> <span data-ttu-id="e5b5c-130">V tomto příkladu vybereme 'MyPreviewVNet' virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-130">In this example, we pick the 'MyPreviewVNet' virtual network.</span></span>

    ![Vytvoření virtuálního počítače – výběr virtuální sítě](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. <span data-ttu-id="e5b5c-132">Na **Souhrn** stránky **vytvořit virtuální počítač** průvodce zkontrolujte a klikněte na tlačítko **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-132">On the **Summary** page of the **Create virtual machine** wizard, review and click the **OK** button.</span></span>

    ![Vytvoření virtuálního počítače - vybranou virtuální síť](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. <span data-ttu-id="e5b5c-134">Nasazení nového virtuálního počítače na základě bitové kopie systému RHEL 7.2 by se měl spustit.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-134">Deployment of the new virtual machine based on the RHEL 7.2 image should start.</span></span>

    ![Vytvoření virtuálního počítače - nasazení bylo zahájeno](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. <span data-ttu-id="e5b5c-136">Po několika minutách musí být virtuální počítač nasazené úspěšně a připravené k použití.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-136">After a few minutes, the virtual machine should be deployed successfully and ready for use.</span></span>

    ![Vytvoření virtuálního počítače – nasazení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a><span data-ttu-id="e5b5c-138">Vzdáleně připojit k nově zřízeného virtuálního počítače systému Linux</span><span class="sxs-lookup"><span data-stu-id="e5b5c-138">Connect remotely to the newly provisioned Linux virtual machine</span></span>
<span data-ttu-id="e5b5c-139">Virtuální počítač RHEL 7.2 zřízená v Azure.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-139">The RHEL 7.2 virtual machine has been provisioned in Azure.</span></span> <span data-ttu-id="e5b5c-140">Dalším krokem je vzdáleně připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-140">The next task is to connect remotely to the virtual machine.</span></span>

<span data-ttu-id="e5b5c-141">**Připojit k virtuálnímu počítači systému RHEL 7.2** postupujte podle pokynů v článku [přihlášení do virtuálního počítače se systémem Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e5b5c-141">**Connect to the RHEL 7.2 virtual machine** Follow the instructions in the article [How to log on to a virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="e5b5c-142">Další kroky předpokládají, že používáte pro připojení k virtuálnímu počítači systému RHEL klienta PuTTY SSH.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-142">The rest of the steps assume you use the PuTTY SSH client to connect to the RHEL virtual machine.</span></span> <span data-ttu-id="e5b5c-143">Další informace najdete v tématu [stránku položek ke stažení PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="e5b5c-143">For more information, see the [PuTTY Download page](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

1. <span data-ttu-id="e5b5c-144">Otevřete PuTTY program.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-144">Open the PuTTY program.</span></span>
2. <span data-ttu-id="e5b5c-145">Zadejte **název hostitele** pro nově vytvořený virtuální počítač RHEL.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-145">Enter the **Host Name** for the newly created RHEL virtual machine.</span></span> <span data-ttu-id="e5b5c-146">V tomto příkladu naše virtuální počítač má název hostitele 'contoso-rhel.cloudapp .net'.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-146">In this example, our virtual machine has the host name 'contoso-rhel.cloudapp.net'.</span></span> <span data-ttu-id="e5b5c-147">Pokud si nejste jisti název hostitele virtuálního počítače, podívejte se na řídicím panelu virtuálního počítače na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-147">If you are not sure of the host name of your VM, refer to the VM dashboard on the Azure portal.</span></span>

    ![Připojit puTTY](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. <span data-ttu-id="e5b5c-149">Přihlaste se k virtuálnímu počítači pomocí přihlašovacích údajů místního správce, které jste zadali, vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-149">Log on to the virtual machine using the local administrator credentials you specified when the virtual machine was created.</span></span> <span data-ttu-id="e5b5c-150">V tomto příkladu jsme použili účet místního správce "mahesh".</span><span class="sxs-lookup"><span data-stu-id="e5b5c-150">In this example, we used the local administrator account "mahesh".</span></span>

    ![PuTTY přihlášení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-the-linux-virtual-machine"></a><span data-ttu-id="e5b5c-152">Nainstalujte požadované balíčky Linux virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e5b5c-152">Install required packages on the Linux virtual machine</span></span>
<span data-ttu-id="e5b5c-153">Po připojení k virtuálnímu počítači, je dalším krokem instalace balíčky požadované pro připojení k doméně na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-153">After connecting to the virtual machine, the next task is to install packages required for domain join on the virtual machine.</span></span> <span data-ttu-id="e5b5c-154">Proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e5b5c-154">Perform the following steps:</span></span>

1. <span data-ttu-id="e5b5c-155">**Nainstalujte realmd:** realmd balíčku se používá pro připojení k doméně.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-155">**Install realmd:** The realmd package is used for domain join.</span></span> <span data-ttu-id="e5b5c-156">V terminálu PuTTY zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e5b5c-156">In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="e5b5c-157">sudo yum instalace realmd</span><span class="sxs-lookup"><span data-stu-id="e5b5c-157">sudo yum install realmd</span></span>

    ![Nainstalujte realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    <span data-ttu-id="e5b5c-159">Po několika minutách by měly získat balíček realmd nainstalovány na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-159">After a few minutes, the realmd package should get installed on the virtual machine.</span></span>

    ![realmd nainstalován](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. <span data-ttu-id="e5b5c-161">**Nainstalujte sssd:** závislý balíček realmd sssd k provádění operací připojení k doméně.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-161">**Install sssd:** The realmd package depends on sssd to perform domain join operations.</span></span> <span data-ttu-id="e5b5c-162">V terminálu PuTTY zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e5b5c-162">In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="e5b5c-163">sudo yum instalace sssd</span><span class="sxs-lookup"><span data-stu-id="e5b5c-163">sudo yum install sssd</span></span>

    ![Nainstalujte sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    <span data-ttu-id="e5b5c-165">Po několika minutách by měly získat balíček sssd nainstalovány na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-165">After a few minutes, the sssd package should get installed on the virtual machine.</span></span>

    ![realmd nainstalován](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. <span data-ttu-id="e5b5c-167">**Instalaci protokolu kerberos:** v PuTTY terminálu, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e5b5c-167">**Install kerberos:** In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="e5b5c-168">sudo yum instalace krb5 – pracovní stanice krb5-knihovny</span><span class="sxs-lookup"><span data-stu-id="e5b5c-168">sudo yum install krb5-workstation krb5-libs</span></span>

    ![Instalaci protokolu kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    <span data-ttu-id="e5b5c-170">Po několika minutách by měly získat balíček realmd nainstalovány na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-170">After a few minutes, the realmd package should get installed on the virtual machine.</span></span>

    ![Nainstalovat pomocí protokolu Kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a><span data-ttu-id="e5b5c-172">Připojit virtuální počítač Linux k spravované doméně</span><span class="sxs-lookup"><span data-stu-id="e5b5c-172">Join the Linux virtual machine to the managed domain</span></span>
<span data-ttu-id="e5b5c-173">Teď, když požadované balíčky jsou nainstalovány na virtuální počítač Linux, dalším úkolem je připojení virtuálního počítače k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-173">Now that the required packages are installed on the Linux virtual machine, the next task is to join the virtual machine to the managed domain.</span></span>

1. <span data-ttu-id="e5b5c-174">Zjistit spravované doméně služby AAD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-174">Discover the AAD Domain Services managed domain.</span></span> <span data-ttu-id="e5b5c-175">V terminálu PuTTY zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e5b5c-175">In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="e5b5c-176">sudo sféry zjistit CONTOSO100.COM</span><span class="sxs-lookup"><span data-stu-id="e5b5c-176">sudo realm discover CONTOSO100.COM</span></span>

    ![Zjišťování sféry](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    <span data-ttu-id="e5b5c-178">Pokud **zjišťování sféry** se nepodařilo najít vaší spravované domény, ujistěte se, že doména je dostupný z virtuálního počítače (zkuste ping).</span><span class="sxs-lookup"><span data-stu-id="e5b5c-178">If **realm discover** is unable to find your managed domain, ensure that the domain is reachable from the virtual machine (try ping).</span></span> <span data-ttu-id="e5b5c-179">Ujistěte se také, že virtuální počítač skutečně byla nasazena do stejné virtuální síti, ve kterém je k dispozici spravované domény.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-179">Also ensure that the virtual machine has indeed been deployed to the same virtual network in which the managed domain is available.</span></span>
2. <span data-ttu-id="e5b5c-180">Inicializace protokolu kerberos.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-180">Initialize kerberos.</span></span> <span data-ttu-id="e5b5c-181">V terminálu PuTTY zadejte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-181">In your PuTTY terminal, type the following command.</span></span> <span data-ttu-id="e5b5c-182">Ujistěte se, že zadáváte uživatel, který patří do skupiny "Administrators AAD řadič domény.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-182">Ensure that you specify a user who belongs to the 'AAD DC Administrators' group.</span></span> <span data-ttu-id="e5b5c-183">Pouze tito uživatelé se může připojit počítače k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-183">Only these users can join computers to the managed domain.</span></span>

    <span data-ttu-id="e5b5c-184">kinitbob@CONTOSO100.COM</span><span class="sxs-lookup"><span data-stu-id="e5b5c-184">kinit bob@CONTOSO100.COM</span></span>

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    <span data-ttu-id="e5b5c-186">Zkontrolujte, že zadáte název domény velkými písmeny else kinit selže.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-186">Ensure that you specify the domain name in capital letters, else kinit fails.</span></span>
3. <span data-ttu-id="e5b5c-187">Připojení počítače k doméně.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-187">Join the machine to the domain.</span></span> <span data-ttu-id="e5b5c-188">V terminálu PuTTY zadejte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-188">In your PuTTY terminal, type the following command.</span></span> <span data-ttu-id="e5b5c-189">Zadejte stejný uživatel, který jste zadali v předchozím kroku (kinit).</span><span class="sxs-lookup"><span data-stu-id="e5b5c-189">Specify the same user you specified in the preceding step ('kinit').</span></span>

    <span data-ttu-id="e5b5c-190">připojení k sféry sudo – podrobné CONTOSO100.COM -U 'bob@CONTOSO100.COM.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-190">sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'</span></span>

    ![Sféra spojení](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

<span data-ttu-id="e5b5c-192">Měli byste obdržet zprávu ("úspěšně zaregistrované počítače ve sféře") Pokud je počítač úspěšně připojený k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-192">You should get a message ("Successfully enrolled machine in realm") when the machine is successfully joined to the managed domain.</span></span>

## <a name="verify-domain-join"></a><span data-ttu-id="e5b5c-193">Ověřte připojení k doméně</span><span class="sxs-lookup"><span data-stu-id="e5b5c-193">Verify domain join</span></span>
<span data-ttu-id="e5b5c-194">Můžete rychle ověřit, zda je počítač byl úspěšně připojen k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-194">You can quickly verify whether the machine has been successfully joined to the managed domain.</span></span> <span data-ttu-id="e5b5c-195">Připojení k nově virtuálních počítačů systému RHEL pomocí SSH a uživatelský účet domény a zkontrolujte Pokud uživatelský účet vyřešen správně připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-195">Connect to the newly domain joined RHEL VM using SSH and a domain user account and then check to see if the user account is resolved correctly.</span></span>

1. <span data-ttu-id="e5b5c-196">V terminálu PuTTY zadejte následující příkaz pro připojení k nově RHEL virtuálního počítače pomocí protokolu SSH připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-196">In your PuTTY terminal, type the following command to connect to the newly domain joined RHEL virtual machine using SSH.</span></span> <span data-ttu-id="e5b5c-197">Použít účet domény, který patří k spravované doméně (například "bob@CONTOSO100.COM' v takovém případě.)</span><span class="sxs-lookup"><span data-stu-id="e5b5c-197">Use a domain account that belongs to the managed domain (for example, 'bob@CONTOSO100.COM' in this case.)</span></span>

    <span data-ttu-id="e5b5c-198">SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="e5b5c-198">ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net</span></span>
2. <span data-ttu-id="e5b5c-199">V terminálu PuTTY zadejte následující příkaz zobrazit, pokud byl domovský adresář správně inicializován.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-199">In your PuTTY terminal, type the following command to see if the home directory was initialized correctly.</span></span>

    <span data-ttu-id="e5b5c-200">PWD</span><span class="sxs-lookup"><span data-stu-id="e5b5c-200">pwd</span></span>
3. <span data-ttu-id="e5b5c-201">V terminálu PuTTY zadejte následující příkaz zobrazit, pokud členství ve skupinách jsou řešeny správně.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-201">In your PuTTY terminal, type the following command to see if the group memberships are being resolved correctly.</span></span>

    <span data-ttu-id="e5b5c-202">id</span><span class="sxs-lookup"><span data-stu-id="e5b5c-202">id</span></span>

<span data-ttu-id="e5b5c-203">Následuje ukázkový výstup z těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="e5b5c-203">A sample output of these commands follows:</span></span>

![Ověřte připojení k doméně](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a><span data-ttu-id="e5b5c-205">Řešení potíží s připojení k doméně</span><span class="sxs-lookup"><span data-stu-id="e5b5c-205">Troubleshooting domain join</span></span>
<span data-ttu-id="e5b5c-206">Odkazovat [připojení k doméně Poradce při potížích s](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) článku.</span><span class="sxs-lookup"><span data-stu-id="e5b5c-206">Refer to the [Troubleshooting domain join](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) article.</span></span>

## <a name="related-content"></a><span data-ttu-id="e5b5c-207">Související obsah</span><span class="sxs-lookup"><span data-stu-id="e5b5c-207">Related Content</span></span>
* [<span data-ttu-id="e5b5c-208">Azure AD Domain Services – Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="e5b5c-208">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="e5b5c-209">Připojení virtuálního počítače s Windows serverem k spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="e5b5c-209">Join a Windows Server virtual machine to an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* <span data-ttu-id="e5b5c-210">[Jak se přihlásit do virtuálního počítače se systémem Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e5b5c-210">[How to log on to a virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* [<span data-ttu-id="e5b5c-211">Instalaci protokolu Kerberos</span><span class="sxs-lookup"><span data-stu-id="e5b5c-211">Installing Kerberos</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [<span data-ttu-id="e5b5c-212">Red Hat Enterprise Linux 7 – Příručka pro integraci systému Windows</span><span class="sxs-lookup"><span data-stu-id="e5b5c-212">Red Hat Enterprise Linux 7 - Windows Integration Guide</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
