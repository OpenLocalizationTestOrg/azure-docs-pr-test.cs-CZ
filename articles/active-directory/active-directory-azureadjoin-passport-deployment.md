---
title: "aaaEnable Microsoft Windows Hello pro firmy ve vaší organizaci | Microsoft Docs"
description: "Nasazení pokyny tooenable Microsoft Passport ve vaší organizaci."
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
ms.openlocfilehash: 6041f5916f606752bc55844b1b2d0a423b913cd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a><span data-ttu-id="24d00-104">Povolit Microsoft Windows Hello pro firmy ve vaší organizaci</span><span class="sxs-lookup"><span data-stu-id="24d00-104">Enable Microsoft Windows Hello for Business in your organization</span></span>
<span data-ttu-id="24d00-105">Po [připojení zařízení s Windows 10 připojených k doméně se službou Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), hello následující tooenable Microsoft Windows Hello pro firmy ve vaší organizaci:</span><span class="sxs-lookup"><span data-stu-id="24d00-105">After [connecting Windows 10 domain-joined devices with Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), do hello following tooenable Microsoft Windows Hello for Business in your organization:</span></span>

1. <span data-ttu-id="24d00-106">Nasazení nástroje System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="24d00-106">Deploy System Center Configuration Manager</span></span>  
2. <span data-ttu-id="24d00-107">Konfigurace nastavení zásad</span><span class="sxs-lookup"><span data-stu-id="24d00-107">Configure policy settings</span></span>
3. <span data-ttu-id="24d00-108">Konfigurace profilu certifikátu hello</span><span class="sxs-lookup"><span data-stu-id="24d00-108">Configure hello certificate profile</span></span>  

## <a name="deploy-system-center-configuration-manager"></a><span data-ttu-id="24d00-109">Nasazení nástroje System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="24d00-109">Deploy System Center Configuration Manager</span></span>
<span data-ttu-id="24d00-110">toodeploy uživatelské certifikáty založené na Windows Hello pro firmy klíče, je třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="24d00-110">toodeploy user certificates based on Windows Hello for Business keys, you need hello following:</span></span>

* <span data-ttu-id="24d00-111">**System Center Configuration Manager aktuální větve** -potřebovat tooinstall verze 1606 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="24d00-111">**System Center Configuration Manager Current Branch** - You need tooinstall version 1606 or better.</span></span> <span data-ttu-id="24d00-112">Další informace najdete v tématu hello [dokumentace pro System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) a [Blog týmu nástroje System Center Configuration Manager](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span><span class="sxs-lookup"><span data-stu-id="24d00-112">For more information, see hello [Documentation for System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) and [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span></span>
* <span data-ttu-id="24d00-113">**Infrastruktury veřejných klíčů (PKI)** -tooenable Microsoft Windows Hello pro firmy pomocí uživatelských certifikátů, musíte mít infrastrukturu veřejných KLÍČŮ na místě.</span><span class="sxs-lookup"><span data-stu-id="24d00-113">**Public key infrastructure (PKI)** - tooenable Microsoft Windows Hello for Business by using user certificates, you must have a PKI in place.</span></span> <span data-ttu-id="24d00-114">Pokud nemáte, nebo pokud nechcete, aby toouse ji u uživatelských certifikátů můžete nasadit nový řadič domény, který má sestavení 10551 (nebo lepší) nainstalován systém Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="24d00-114">If you don’t have one, or you don’t want toouse it for user certificates, you can deploy a new domain controller that has Windows Server 2016 build 10551 (or better) installed.</span></span> <span data-ttu-id="24d00-115">Postupujte podle kroků hello příliš[instalace repliky řadiče domény v existující doméně](https://technet.microsoft.com/library/jj574134.aspx) nebo příliš[instalaci nové doménové struktury služby Active Directory, pokud vytváříte nové prostředí](https://technet.microsoft.com/library/jj574166).</span><span class="sxs-lookup"><span data-stu-id="24d00-115">Follow hello steps too[install a replica domain controller in an existing domain](https://technet.microsoft.com/library/jj574134.aspx) or too[install a new Active Directory forest, if you're creating a new environment](https://technet.microsoft.com/library/jj574166).</span></span> <span data-ttu-id="24d00-116">(hello soubory ISO jsou k dispozici ke stažení na [Signiant média Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span><span class="sxs-lookup"><span data-stu-id="24d00-116">(hello ISOs are available for download on [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span></span>

## <a name="configure-policy-settings"></a><span data-ttu-id="24d00-117">Konfigurace nastavení zásad</span><span class="sxs-lookup"><span data-stu-id="24d00-117">Configure policy settings</span></span>
<span data-ttu-id="24d00-118">hello tooconfigure Microsoft Windows Hello pro firmy nastavení zásad, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="24d00-118">tooconfigure hello Microsoft Windows Hello for Business policy settings, you have two options:</span></span>

* <span data-ttu-id="24d00-119">Zásady skupiny ve službě Active Directory</span><span class="sxs-lookup"><span data-stu-id="24d00-119">Group Policy in Active Directory</span></span> 
* <span data-ttu-id="24d00-120">Hello System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="24d00-120">hello System Center Configuration Manager</span></span> 

<span data-ttu-id="24d00-121">Pomocí zásad skupiny ve službě Active Directory je doporučená metoda tooconfigure Microsoft Windows Hello pro firmy nastavení zásad hello.</span><span class="sxs-lookup"><span data-stu-id="24d00-121">Using Group Policy in Active Directory is hello recommended method tooconfigure Microsoft Windows Hello for Business policy settings.</span></span> 

<span data-ttu-id="24d00-122">Pomocí nástroje System Center Configuration Manager je metoda hello preferované, pokud jste ji použít i toodeploy certifikáty.</span><span class="sxs-lookup"><span data-stu-id="24d00-122">Using System Center Configuration Manager is hello preferred method when you also use it toodeploy certificates.</span></span> <span data-ttu-id="24d00-123">Tento scénář:</span><span class="sxs-lookup"><span data-stu-id="24d00-123">This scenario:</span></span>

* <span data-ttu-id="24d00-124">Zajišťuje kompatibilitu s hello novější scénáře nasazení</span><span class="sxs-lookup"><span data-stu-id="24d00-124">Ensures compatibility with hello newer deployment scenarios</span></span>
* <span data-ttu-id="24d00-125">Vyžaduje na straně klienta hello Windows 10 verze 1607 nebo lepší.</span><span class="sxs-lookup"><span data-stu-id="24d00-125">Requires on hello client side Windows 10 Version 1607 or better.</span></span>

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a><span data-ttu-id="24d00-126">Konfigurace Microsoft Windows Hello pro firmy prostřednictvím zásad skupiny ve službě Active Directory</span><span class="sxs-lookup"><span data-stu-id="24d00-126">Configure Microsoft Windows Hello for Business via group policy in Active Directory</span></span>
<span data-ttu-id="24d00-127">**Kroky**:</span><span class="sxs-lookup"><span data-stu-id="24d00-127">**Steps**:</span></span>

1. <span data-ttu-id="24d00-128">Otevřete správce serveru a přejděte příliš**nástroje** > **Správa zásad skupiny**.</span><span class="sxs-lookup"><span data-stu-id="24d00-128">Open Server Manager, and navigate too**Tools** > **Group Policy Management**.</span></span>
2. <span data-ttu-id="24d00-129">Správa zásad skupiny přejděte toohello domény uzlu, který odpovídá toohello domény, ve kterém chcete tooenable připojení ke službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="24d00-129">From Group Policy Management, navigate toohello domain node that corresponds toohello domain in which you want tooenable Azure AD Join.</span></span>
3. <span data-ttu-id="24d00-130">Klikněte pravým tlačítkem na **objekty zásad skupiny**a vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="24d00-130">Right-click **Group Policy Objects**, and select **New**.</span></span> <span data-ttu-id="24d00-131">Pojmenujte objektu zásad skupiny, například povolení Windows Hello pro firmy.</span><span class="sxs-lookup"><span data-stu-id="24d00-131">Give your Group Policy Object a name, for example, Enable Windows Hello for Business.</span></span> <span data-ttu-id="24d00-132">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="24d00-132">Click **OK**.</span></span>
4. <span data-ttu-id="24d00-133">Klikněte pravým tlačítkem na nový objekt zásad skupiny a potom vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="24d00-133">Right-click your new Group Policy Object, and then select **Edit**.</span></span>
5. <span data-ttu-id="24d00-134">Přejděte příliš**konfigurace počítače** > **zásady** > **šablony pro správu** > **Windows Součásti** > **Windows Hello pro firmy**.</span><span class="sxs-lookup"><span data-stu-id="24d00-134">Navigate too**Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Windows Hello for Business**.</span></span>
6. <span data-ttu-id="24d00-135">Klikněte pravým tlačítkem na **povolit Windows Hello pro firmy**a potom vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="24d00-135">Right-click **Enable Windows Hello for Business**, and then select **Edit**.</span></span>
7. <span data-ttu-id="24d00-136">Vyberte hello **povoleno** a pak klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="24d00-136">Select hello **Enabled** option button, and then click **Apply**.</span></span> <span data-ttu-id="24d00-137">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="24d00-137">Click **OK**.</span></span>
8. <span data-ttu-id="24d00-138">Nyní můžete propojit hello objektu zásad skupiny tooa umístění podle vaší volby.</span><span class="sxs-lookup"><span data-stu-id="24d00-138">You can now link hello Group Policy Object tooa location of your choice.</span></span> <span data-ttu-id="24d00-139">tooenable tato zásada u všech hello připojený k doméně zařízení s Windows 10 ve vaší organizaci, odkaz hello zásad skupiny toohello domény.</span><span class="sxs-lookup"><span data-stu-id="24d00-139">tooenable this policy for all of hello domain-joined Windows 10 devices in your organization, link hello Group Policy toohello domain.</span></span> <span data-ttu-id="24d00-140">Například:</span><span class="sxs-lookup"><span data-stu-id="24d00-140">For example:</span></span>
   * <span data-ttu-id="24d00-141">Konkrétní organizační jednotku (OU) ve službě Active Directory, kde budou umístěné počítače s Windows 10 připojených k doméně</span><span class="sxs-lookup"><span data-stu-id="24d00-141">A specific organizational unit (OU) in Active Directory where Windows 10 domain-joined computers will be located</span></span>
   * <span data-ttu-id="24d00-142">Skupinu zabezpečení, která obsahuje připojený k doméně počítače Windows 10, které se budou automaticky registrovat s Azure AD</span><span class="sxs-lookup"><span data-stu-id="24d00-142">A specific security group that contains Windows 10 domain-joined computers that will be automatically registered with Azure AD</span></span>

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a><span data-ttu-id="24d00-143">Konfigurace Windows Hello pro firmy pomocí System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="24d00-143">Configure Windows Hello for Business using System Center Configuration Manager</span></span>
<span data-ttu-id="24d00-144">**Kroky**:</span><span class="sxs-lookup"><span data-stu-id="24d00-144">**Steps**:</span></span>

1. <span data-ttu-id="24d00-145">Otevřete hello **System Center Configuration Manager**a potom přejděte příliš**prostředky a kompatibilita > Nastavení dodržování předpisů > přístup k prostředkům společnosti > Windows Hello pro firmy profily**.</span><span class="sxs-lookup"><span data-stu-id="24d00-145">Open hello **System Center Configuration Manager**, and then navigate too**Assets & Compliance > Compliance Settings > Company Resource Access > Windows Hello for Business Profiles**.</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. <span data-ttu-id="24d00-147">V panelu nástrojů hello hello nahoře, klikněte na **vytvořit Windows Hello pro firmy profil**.</span><span class="sxs-lookup"><span data-stu-id="24d00-147">In hello toolbar on hello top, click **Create Windows Hello for business Profile**.</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. <span data-ttu-id="24d00-149">Na hello **Obecné** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="24d00-149">On hello **General** dialog, perform hello following steps:</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    <span data-ttu-id="24d00-151">a.</span><span class="sxs-lookup"><span data-stu-id="24d00-151">a.</span></span> <span data-ttu-id="24d00-152">V hello **název** textovému poli, zadejte název pro svůj profil, například **Můj profil WHfB**.</span><span class="sxs-lookup"><span data-stu-id="24d00-152">In hello **Name** textbox, type a name for your profile, for example, **My WHfB Profile**.</span></span>
   
    <span data-ttu-id="24d00-153">b.</span><span class="sxs-lookup"><span data-stu-id="24d00-153">b.</span></span> <span data-ttu-id="24d00-154">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="24d00-154">Click **Next**.</span></span>
4. <span data-ttu-id="24d00-155">Na hello **podporované platformy** dialogové okno, vyberte hello platformy, které budou zřizovány s Tento Windows Hello pro firmy profil a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="24d00-155">On hello **Supported Platforms** dialog, select hello platforms that will be provisioned with this Windows Hello for business profile, and then click **Next**.</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. <span data-ttu-id="24d00-157">Na hello **nastavení** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="24d00-157">On hello **Settings** dialog, perform hello following steps:</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    <span data-ttu-id="24d00-159">a.</span><span class="sxs-lookup"><span data-stu-id="24d00-159">a.</span></span> <span data-ttu-id="24d00-160">Jako **konfigurace Windows Hello pro firmy**, vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="24d00-160">As **Configure Windows Hello for Business**, select **Enabled**.</span></span>
   
    <span data-ttu-id="24d00-161">b.</span><span class="sxs-lookup"><span data-stu-id="24d00-161">b.</span></span> <span data-ttu-id="24d00-162">Jako **používat (Trusted Platform Module)**, vyberte **požadované**.</span><span class="sxs-lookup"><span data-stu-id="24d00-162">As **Use a Trusted Platform Module (TPM)**, select **Required**.</span></span> 
   
    <span data-ttu-id="24d00-163">c.</span><span class="sxs-lookup"><span data-stu-id="24d00-163">c.</span></span> <span data-ttu-id="24d00-164">Jako **metodu ověřování**, vyberte **založené na certifikátech**.</span><span class="sxs-lookup"><span data-stu-id="24d00-164">As **Authentication method**, select **Certificate-based**.</span></span>
   
    <span data-ttu-id="24d00-165">d.</span><span class="sxs-lookup"><span data-stu-id="24d00-165">d.</span></span> <span data-ttu-id="24d00-166">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="24d00-166">Click **Next**.</span></span>
6. <span data-ttu-id="24d00-167">Na hello **Souhrn** dialogové okno, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="24d00-167">On hello **Summary** dialog, click **Next**.</span></span>
7. <span data-ttu-id="24d00-168">Na hello **dokončení** dialogové okno, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="24d00-168">On hello **Completion** dialog, click **Close**.</span></span>
8. <span data-ttu-id="24d00-169">V panelu nástrojů hello hello nahoře, klikněte na **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="24d00-169">In hello toolbar on hello top, click **Deploy**.</span></span>
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-hello-certificate-profile"></a><span data-ttu-id="24d00-171">Konfigurace profilu certifikátu hello</span><span class="sxs-lookup"><span data-stu-id="24d00-171">Configure hello certificate profile</span></span>
<span data-ttu-id="24d00-172">Pokud používáte ověřování pomocí certifikátů pro místní ověřování, budete potřebovat tooconfigure a nasazení profilu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="24d00-172">If you are using certificate-based authentication for on-premises authentication, you need tooconfigure and deploy a certificate profile.</span></span> <span data-ttu-id="24d00-173">Tato úloha vyžaduje tooset nastavujete NDES server a role lokality bodu registrace certifikátu v hello System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="24d00-173">This task requires you tooset up an NDES server and Certificate Registration Point site role in hello System Center Configuration Manager.</span></span> <span data-ttu-id="24d00-174">Další podrobnosti najdete v tématu hello [požadavky na profily certifikátů v nástroji Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).</span><span class="sxs-lookup"><span data-stu-id="24d00-174">For more details, see hello [Prerequisites for Certificate Profiles in Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).</span></span>

1. <span data-ttu-id="24d00-175">Otevřete hello **System Center Configuration Manager**a potom přejděte příliš**prostředky a kompatibilita > Nastavení dodržování předpisů > přístup k prostředkům společnosti > profily certifikátů**.</span><span class="sxs-lookup"><span data-stu-id="24d00-175">Open hello **System Center Configuration Manager**, and then navigate too**Assets & Compliance > Compliance Settings > Company Resource Access > Certificate Profiles**.</span></span>
2. <span data-ttu-id="24d00-176">Vyberte šablonu, která obsahuje čipové karty přihlášení rozšířené použití klíče (EKU).</span><span class="sxs-lookup"><span data-stu-id="24d00-176">Select a template that has Smart Card sign-in extended key usage (EKU).</span></span>

<span data-ttu-id="24d00-177">Na hello **zápis SCEP** stránku hello profilu certifikátu, je nutné toochoose **tooPassport pro pracovní jinak dojde k selhání instalace** jako hello **zprostředkovatele úložiště klíčů**.</span><span class="sxs-lookup"><span data-stu-id="24d00-177">On hello **SCEP Enrollment** page of hello certificate profile, you need toochoose **Install tooPassport for Work otherwise fail** as hello **Key Storage Provider**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24d00-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24d00-178">Next steps</span></span>
* [<span data-ttu-id="24d00-179">Windows 10 pro podnik hello: způsoby toouse zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="24d00-179">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="24d00-180">Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="24d00-180">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="24d00-181">Ověřování identit bez hesel pomocí Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="24d00-181">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="24d00-182">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="24d00-182">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="24d00-183">Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10</span><span class="sxs-lookup"><span data-stu-id="24d00-183">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="24d00-184">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="24d00-184">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

