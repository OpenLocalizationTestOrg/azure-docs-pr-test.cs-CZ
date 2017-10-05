---
title: "Povolit Microsoft Windows Hello pro firmy ve vaší organizaci | Microsoft Docs"
description: "Pokyny k nasazení povolit Microsoft Passport ve vaší organizaci."
services: active-directory
documentationcenter: 
keywords: "Konfigurace Microsoft Passport, Microsoft Windows Hello pro firmy nasazení"
author: MarkusVi
manager: femila
tags: azure-classic-portal
ms.assetid: 7dbbe3c6-1cd7-429c-a9b2-115fcbc02416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: 58943e1e29755c983e55c675dd4fe7b75ac47b34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a><span data-ttu-id="f20b5-104">Povolit Microsoft Windows Hello pro firmy ve vaší organizaci</span><span class="sxs-lookup"><span data-stu-id="f20b5-104">Enable Microsoft Windows Hello for Business in your organization</span></span>
<span data-ttu-id="f20b5-105">Po [připojení zařízení s Windows 10 připojených k doméně se službou Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), proveďte postup pro povolení Microsoft Windows Hello pro firmy v organizaci:</span><span class="sxs-lookup"><span data-stu-id="f20b5-105">After [connecting Windows 10 domain-joined devices with Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), do the following to enable Microsoft Windows Hello for Business in your organization:</span></span>

1. <span data-ttu-id="f20b5-106">Nasazení nástroje System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="f20b5-106">Deploy System Center Configuration Manager</span></span>  
2. <span data-ttu-id="f20b5-107">Konfigurace nastavení zásad</span><span class="sxs-lookup"><span data-stu-id="f20b5-107">Configure policy settings</span></span>
3. <span data-ttu-id="f20b5-108">Konfigurace profilu certifikátu</span><span class="sxs-lookup"><span data-stu-id="f20b5-108">Configure the certificate profile</span></span>  

## <a name="deploy-system-center-configuration-manager"></a><span data-ttu-id="f20b5-109">Nasazení nástroje System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="f20b5-109">Deploy System Center Configuration Manager</span></span>
<span data-ttu-id="f20b5-110">Pokud chcete nasadit uživatelské certifikáty založené na Windows Hello pro firmy klíče, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="f20b5-110">To deploy user certificates based on Windows Hello for Business keys, you need the following:</span></span>

* <span data-ttu-id="f20b5-111">**System Center Configuration Manager aktuální větev** – je potřeba nainstalovat verzi 1606 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="f20b5-111">**System Center Configuration Manager Current Branch** - You need to install version 1606 or better.</span></span> <span data-ttu-id="f20b5-112">Další informace najdete v tématu [dokumentace pro System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) a [Blog týmu nástroje System Center Configuration Manager](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span><span class="sxs-lookup"><span data-stu-id="f20b5-112">For more information, see the [Documentation for System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) and [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span></span>
* <span data-ttu-id="f20b5-113">**Infrastruktury veřejných klíčů (PKI)** – Pokud chcete povolit Microsoft Windows Hello pro firmy pomocí uživatelských certifikátů, musíte mít infrastrukturu veřejných KLÍČŮ na místě.</span><span class="sxs-lookup"><span data-stu-id="f20b5-113">**Public key infrastructure (PKI)** - To enable Microsoft Windows Hello for Business by using user certificates, you must have a PKI in place.</span></span> <span data-ttu-id="f20b5-114">Pokud nemáte nebo nechcete používat pro certifikáty uživatelů, můžete nasadit nový řadič domény s Windows Server 2016 sestavení 10551 (nebo lepší) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="f20b5-114">If you don’t have one, or you don’t want to use it for user certificates, you can deploy a new domain controller that has Windows Server 2016 build 10551 (or better) installed.</span></span> <span data-ttu-id="f20b5-115">Postup [instalace repliky řadiče domény v existující doméně](https://technet.microsoft.com/library/jj574134.aspx) nebo [instalaci nové doménové struktury služby Active Directory, pokud vytváříte nové prostředí](https://technet.microsoft.com/library/jj574166).</span><span class="sxs-lookup"><span data-stu-id="f20b5-115">Follow the steps to [install a replica domain controller in an existing domain](https://technet.microsoft.com/library/jj574134.aspx) or to [install a new Active Directory forest, if you're creating a new environment](https://technet.microsoft.com/library/jj574166).</span></span> <span data-ttu-id="f20b5-116">(Soubory ISO jsou k dispozici ke stažení na [Signiant média Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span><span class="sxs-lookup"><span data-stu-id="f20b5-116">(The ISOs are available for download on [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span></span>

## <a name="configure-policy-settings"></a><span data-ttu-id="f20b5-117">Konfigurace nastavení zásad</span><span class="sxs-lookup"><span data-stu-id="f20b5-117">Configure policy settings</span></span>
<span data-ttu-id="f20b5-118">Pokud chcete nakonfigurovat Microsoft Windows Hello pro firmy nastavení zásad, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="f20b5-118">To configure the Microsoft Windows Hello for Business policy settings, you have two options:</span></span>

* <span data-ttu-id="f20b5-119">Zásady skupiny ve službě Active Directory</span><span class="sxs-lookup"><span data-stu-id="f20b5-119">Group Policy in Active Directory</span></span> 
* <span data-ttu-id="f20b5-120">System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="f20b5-120">The System Center Configuration Manager</span></span> 

<span data-ttu-id="f20b5-121">Pomocí zásad skupiny ve službě Active Directory je doporučená metoda konfigurace Microsoft Windows Hello pro firmy nastavení zásad.</span><span class="sxs-lookup"><span data-stu-id="f20b5-121">Using Group Policy in Active Directory is the recommended method to configure Microsoft Windows Hello for Business policy settings.</span></span> 

<span data-ttu-id="f20b5-122">Pomocí nástroje System Center Configuration Manager je preferovanou metodu, když ho také použít k nasazení certifikátů.</span><span class="sxs-lookup"><span data-stu-id="f20b5-122">Using System Center Configuration Manager is the preferred method when you also use it to deploy certificates.</span></span> <span data-ttu-id="f20b5-123">Tento scénář:</span><span class="sxs-lookup"><span data-stu-id="f20b5-123">This scenario:</span></span>

* <span data-ttu-id="f20b5-124">Zajišťuje kompatibilitu s novější scénáře nasazení</span><span class="sxs-lookup"><span data-stu-id="f20b5-124">Ensures compatibility with the newer deployment scenarios</span></span>
* <span data-ttu-id="f20b5-125">Vyžaduje na straně klienta Windows 10 verze 1607 nebo lepší.</span><span class="sxs-lookup"><span data-stu-id="f20b5-125">Requires on the client side Windows 10 Version 1607 or better.</span></span>

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a><span data-ttu-id="f20b5-126">Konfigurace Microsoft Windows Hello pro firmy prostřednictvím zásad skupiny ve službě Active Directory</span><span class="sxs-lookup"><span data-stu-id="f20b5-126">Configure Microsoft Windows Hello for Business via group policy in Active Directory</span></span>
<span data-ttu-id="f20b5-127">**Kroky**:</span><span class="sxs-lookup"><span data-stu-id="f20b5-127">**Steps**:</span></span>

1. <span data-ttu-id="f20b5-128">Otevřete správce serveru a přejděte do **nástroje** > **Správa zásad skupiny**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-128">Open Server Manager, and navigate to **Tools** > **Group Policy Management**.</span></span>
2. <span data-ttu-id="f20b5-129">Správa zásad skupiny přejděte k uzlu domény, který odpovídá k doméně, ve kterém chcete povolit připojení ke službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f20b5-129">From Group Policy Management, navigate to the domain node that corresponds to the domain in which you want to enable Azure AD Join.</span></span>
3. <span data-ttu-id="f20b5-130">Klikněte pravým tlačítkem na **objekty zásad skupiny**a vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-130">Right-click **Group Policy Objects**, and select **New**.</span></span> <span data-ttu-id="f20b5-131">Pojmenujte objektu zásad skupiny, například povolení Windows Hello pro firmy.</span><span class="sxs-lookup"><span data-stu-id="f20b5-131">Give your Group Policy Object a name, for example, Enable Windows Hello for Business.</span></span> <span data-ttu-id="f20b5-132">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-132">Click **OK**.</span></span>
4. <span data-ttu-id="f20b5-133">Klikněte pravým tlačítkem na nový objekt zásad skupiny a potom vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-133">Right-click your new Group Policy Object, and then select **Edit**.</span></span>
5. <span data-ttu-id="f20b5-134">Přejděte na **konfigurace počítače** > **zásady** > **šablony pro správu** > **Windows Součásti** > **Windows Hello pro firmy**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-134">Navigate to **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Windows Hello for Business**.</span></span>
6. <span data-ttu-id="f20b5-135">Klikněte pravým tlačítkem na **povolit Windows Hello pro firmy**a potom vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-135">Right-click **Enable Windows Hello for Business**, and then select **Edit**.</span></span>
7. <span data-ttu-id="f20b5-136">Vyberte **povoleno** a pak klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-136">Select the **Enabled** option button, and then click **Apply**.</span></span> <span data-ttu-id="f20b5-137">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-137">Click **OK**.</span></span>
8. <span data-ttu-id="f20b5-138">Teď můžete objekt zásad skupiny se propojit k umístění podle vaší volby.</span><span class="sxs-lookup"><span data-stu-id="f20b5-138">You can now link the Group Policy Object to a location of your choice.</span></span> <span data-ttu-id="f20b5-139">Chcete-li tato zásada u všech zařízení připojených k doméně Windows 10 ve vaší organizaci, propojte zásad skupiny do domény.</span><span class="sxs-lookup"><span data-stu-id="f20b5-139">To enable this policy for all of the domain-joined Windows 10 devices in your organization, link the Group Policy to the domain.</span></span> <span data-ttu-id="f20b5-140">Například:</span><span class="sxs-lookup"><span data-stu-id="f20b5-140">For example:</span></span>
   * <span data-ttu-id="f20b5-141">Konkrétní organizační jednotku (OU) ve službě Active Directory, kde budou umístěné počítače s Windows 10 připojených k doméně</span><span class="sxs-lookup"><span data-stu-id="f20b5-141">A specific organizational unit (OU) in Active Directory where Windows 10 domain-joined computers will be located</span></span>
   * <span data-ttu-id="f20b5-142">Skupinu zabezpečení, která obsahuje připojený k doméně počítače Windows 10, které se budou automaticky registrovat s Azure AD</span><span class="sxs-lookup"><span data-stu-id="f20b5-142">A specific security group that contains Windows 10 domain-joined computers that will be automatically registered with Azure AD</span></span>

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a><span data-ttu-id="f20b5-143">Konfigurace Windows Hello pro firmy pomocí System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="f20b5-143">Configure Windows Hello for Business using System Center Configuration Manager</span></span>
<span data-ttu-id="f20b5-144">**Kroky**:</span><span class="sxs-lookup"><span data-stu-id="f20b5-144">**Steps**:</span></span>

1. <span data-ttu-id="f20b5-145">Otevřete **System Center Configuration Manager**a potom přejděte na **prostředky a kompatibilita > Nastavení dodržování předpisů > přístup k prostředkům společnosti > Windows Hello pro firmy profily**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-145">Open the **System Center Configuration Manager**, and then navigate to **Assets & Compliance > Compliance Settings > Company Resource Access > Windows Hello for Business Profiles**.</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. <span data-ttu-id="f20b5-147">Na panelu nástrojů v horní části klikněte na tlačítko **vytvořit Windows Hello pro firmy profil**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-147">In the toolbar on the top, click **Create Windows Hello for business Profile**.</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. <span data-ttu-id="f20b5-149">Na **Obecné** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f20b5-149">On the **General** dialog, perform the following steps:</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    <span data-ttu-id="f20b5-151">a.</span><span class="sxs-lookup"><span data-stu-id="f20b5-151">a.</span></span> <span data-ttu-id="f20b5-152">V **název** textovému poli, zadejte název pro svůj profil, například **Můj profil WHfB**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-152">In the **Name** textbox, type a name for your profile, for example, **My WHfB Profile**.</span></span>
   
    <span data-ttu-id="f20b5-153">b.</span><span class="sxs-lookup"><span data-stu-id="f20b5-153">b.</span></span> <span data-ttu-id="f20b5-154">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-154">Click **Next**.</span></span>
4. <span data-ttu-id="f20b5-155">Na **podporované platformy** dialogovém okně, vyberte platformy, které se budou zřizovat s Tento profil Windows Hello pro firmy a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-155">On the **Supported Platforms** dialog, select the platforms that will be provisioned with this Windows Hello for business profile, and then click **Next**.</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. <span data-ttu-id="f20b5-157">Na **nastavení** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f20b5-157">On the **Settings** dialog, perform the following steps:</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    <span data-ttu-id="f20b5-159">a.</span><span class="sxs-lookup"><span data-stu-id="f20b5-159">a.</span></span> <span data-ttu-id="f20b5-160">Jako **konfigurace Windows Hello pro firmy**, vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-160">As **Configure Windows Hello for Business**, select **Enabled**.</span></span>
   
    <span data-ttu-id="f20b5-161">b.</span><span class="sxs-lookup"><span data-stu-id="f20b5-161">b.</span></span> <span data-ttu-id="f20b5-162">Jako **používat (Trusted Platform Module)**, vyberte **požadované**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-162">As **Use a Trusted Platform Module (TPM)**, select **Required**.</span></span> 
   
    <span data-ttu-id="f20b5-163">c.</span><span class="sxs-lookup"><span data-stu-id="f20b5-163">c.</span></span> <span data-ttu-id="f20b5-164">Jako **metodu ověřování**, vyberte **založené na certifikátech**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-164">As **Authentication method**, select **Certificate-based**.</span></span>
   
    <span data-ttu-id="f20b5-165">d.</span><span class="sxs-lookup"><span data-stu-id="f20b5-165">d.</span></span> <span data-ttu-id="f20b5-166">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-166">Click **Next**.</span></span>
6. <span data-ttu-id="f20b5-167">Na **Souhrn** dialogové okno, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-167">On the **Summary** dialog, click **Next**.</span></span>
7. <span data-ttu-id="f20b5-168">Na **dokončení** dialogové okno, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-168">On the **Completion** dialog, click **Close**.</span></span>
8. <span data-ttu-id="f20b5-169">Na panelu nástrojů v horní části klikněte na tlačítko **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-169">In the toolbar on the top, click **Deploy**.</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-the-certificate-profile"></a><span data-ttu-id="f20b5-171">Konfigurace profilu certifikátu</span><span class="sxs-lookup"><span data-stu-id="f20b5-171">Configure the certificate profile</span></span>
<span data-ttu-id="f20b5-172">Pokud používáte ověřování pomocí certifikátů pro místní ověřování, budete muset nakonfigurovat a nasadit profil certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f20b5-172">If you are using certificate-based authentication for on-premises authentication, you need to configure and deploy a certificate profile.</span></span> <span data-ttu-id="f20b5-173">Tato úloha vyžaduje, abyste nastavili serveru NDES a role lokality bodu registrace certifikátu v portálu System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="f20b5-173">This task requires you to set up an NDES server and Certificate Registration Point site role in the System Center Configuration Manager.</span></span> <span data-ttu-id="f20b5-174">Další podrobnosti najdete v tématu [požadavky na profily certifikátů v nástroji Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).</span><span class="sxs-lookup"><span data-stu-id="f20b5-174">For more details, see the [Prerequisites for Certificate Profiles in Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).</span></span>

1. <span data-ttu-id="f20b5-175">Otevřete **System Center Configuration Manager**a potom přejděte na **prostředky a kompatibilita > Nastavení dodržování předpisů > přístup k prostředkům společnosti > profily certifikátů**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-175">Open the **System Center Configuration Manager**, and then navigate to **Assets & Compliance > Compliance Settings > Company Resource Access > Certificate Profiles**.</span></span>
2. <span data-ttu-id="f20b5-176">Vyberte šablonu, která obsahuje čipové karty přihlášení rozšířené použití klíče (EKU).</span><span class="sxs-lookup"><span data-stu-id="f20b5-176">Select a template that has Smart Card sign-in extended key usage (EKU).</span></span>

<span data-ttu-id="f20b5-177">Na **zápis SCEP** stránky profilu certifikátu, je třeba vybrat **instalovat do služby Passport pro Work jinak dojde k selhání** jako **zprostředkovatele úložiště klíčů**.</span><span class="sxs-lookup"><span data-stu-id="f20b5-177">On the **SCEP Enrollment** page of the certificate profile, you need to choose **Install to Passport for Work otherwise fail** as the **Key Storage Provider**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f20b5-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f20b5-178">Next steps</span></span>
* [<span data-ttu-id="f20b5-179">Windows 10 pro firmy: Možnosti, jak používat zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="f20b5-179">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="f20b5-180">Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="f20b5-180">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="f20b5-181">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="f20b5-181">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="f20b5-182">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="f20b5-182">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="f20b5-183">Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe</span><span class="sxs-lookup"><span data-stu-id="f20b5-183">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="f20b5-184">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="f20b5-184">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

