---
title: "Azure Active Directory Domain Services: Nasazení Proxy aplikace služby Azure Active Directory | Microsoft Docs"
description: "Použít proxy aplikace služby Azure AD na spravované domény Azure Active Directory Domain Services"
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
ms.openlocfilehash: 4142111231d0256960d0c02d686d51533ba2171c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="c832e-103">Nasazení Azure AD Application Proxy na spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="c832e-103">Deploy Azure AD Application Proxy on an Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="c832e-104">Proxy aplikace služby Azure Active Directory (AD) umožňuje podporu zaměstnanci na vzdálených pracovištích tím, že publikujete místní aplikace toobe přístupné přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="c832e-104">Azure Active Directory (AD) Application Proxy helps you support remote workers by publishing on-premises applications toobe accessed over hello internet.</span></span> <span data-ttu-id="c832e-105">S Azure AD Domain Services můžete nyní navýšení a shift starší verze aplikací běžících místní tooAzure infrastruktury služby.</span><span class="sxs-lookup"><span data-stu-id="c832e-105">With Azure AD Domain Services, you can now lift-and-shift legacy applications running on-premises tooAzure Infrastructure Services.</span></span> <span data-ttu-id="c832e-106">Potom můžete publikovat tyto aplikace pomocí hello Azure AD Application Proxy, tooprovide toousers zabezpečený vzdálený přístup ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="c832e-106">You can then publish these applications using hello Azure AD Application Proxy, tooprovide secure remote access toousers in your organization.</span></span>

<span data-ttu-id="c832e-107">Pokud jste nový toohello proxy aplikace služby Azure AD, další informace o této funkci s hello následujícího článku: [jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace](../active-directory/active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c832e-107">If you're new toohello Azure AD Application Proxy, learn more about this feature with hello following article: [How tooprovide secure remote access tooon-premises applications](../active-directory/active-directory-application-proxy-get-started.md).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="c832e-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c832e-108">Before you begin</span></span>
<span data-ttu-id="c832e-109">úlohy hello tooperform uvedené v tomto článku, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="c832e-109">tooperform hello tasks listed in this article, you need:</span></span>

1. <span data-ttu-id="c832e-110">Platná **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="c832e-110">A valid **Azure subscription**.</span></span>
2. <span data-ttu-id="c832e-111">**Adresář Azure AD** – buď synchronizovány s místní adresář nebo výhradně cloudový adresář.</span><span class="sxs-lookup"><span data-stu-id="c832e-111">An **Azure AD directory** - either synchronized with an on-premises directory or a cloud-only directory.</span></span>
3. <span data-ttu-id="c832e-112">**Licence Azure AD Basic nebo Premium** je požadovaná toouse hello proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c832e-112">An **Azure AD Basic or Premium license** is required toouse hello Azure AD Application Proxy.</span></span>
4. <span data-ttu-id="c832e-113">**Azure AD Domain Services** musí být povolen pro adresář hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c832e-113">**Azure AD Domain Services** must be enabled for hello Azure AD directory.</span></span> <span data-ttu-id="c832e-114">Pokud jste tak dosud neučinili, postupujte podle kroků uvedených v hello všechny hello úlohy [příručce Začínáme](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="c832e-114">If you haven't done so, follow all hello tasks outlined in hello [Getting Started guide](active-directory-ds-getting-started.md).</span></span>

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a><span data-ttu-id="c832e-115">Úloha 1 – povolení služby Azure AD Application Proxy pro váš adresář Azure AD</span><span class="sxs-lookup"><span data-stu-id="c832e-115">Task 1 - Enable Azure AD Application Proxy for your Azure AD directory</span></span>
<span data-ttu-id="c832e-116">Proveďte následující kroky tooenable hello Azure AD Application Proxy pro váš adresář Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="c832e-116">Perform hello following steps tooenable hello Azure AD Application Proxy for your Azure AD directory.</span></span>

1. <span data-ttu-id="c832e-117">Přihlaste se jako správce v hello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c832e-117">Sign in as an administrator in hello [Azure portal](http://portal.azure.com).</span></span>

2. <span data-ttu-id="c832e-118">Klikněte na tlačítko **Azure Active Directory** toobring si přehled directory hello.</span><span class="sxs-lookup"><span data-stu-id="c832e-118">Click **Azure Active Directory** toobring up hello directory overview.</span></span> <span data-ttu-id="c832e-119">Klikněte na tlačítko **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c832e-119">Click **Enterprise applications**.</span></span>

    ![Vyberte adresář Azure AD](./media/app-proxy/app-proxy-enable-start.png)
3. <span data-ttu-id="c832e-121">Klikněte na tlačítko **proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c832e-121">Click **Application proxy**.</span></span> <span data-ttu-id="c832e-122">Pokud nemáte předplatné Azure AD Basic nebo Azure AD Premium, uvidíte možnost tooenable zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="c832e-122">If you do not have an Azure AD Basic or Azure AD Premium subscription, you see an option tooenable a trial.</span></span> <span data-ttu-id="c832e-123">Přepnutí **povolení Proxy aplikace?** příliš**povolit** a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c832e-123">Toggle **Enable Application Proxy?** too**Enable** and click **Save**.</span></span>

    ![Povolit Proxy aplikace](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. <span data-ttu-id="c832e-125">toodownload hello konektor, klikněte na tlačítko hello **konektor** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c832e-125">toodownload hello connector, click hello **Connector** button.</span></span>

    ![Stažení konektoru](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. <span data-ttu-id="c832e-127">Na stránce pro stažení hello, přijměte licenční podmínky pro hello a smlouvy o ochraně osobních údajů a klikněte na hello **Stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c832e-127">On hello download page, accept hello license terms and privacy agreement and click hello **Download** button.</span></span>

    ![Potvrďte stahování](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-toodeploy-hello-azure-ad-application-proxy-connector"></a><span data-ttu-id="c832e-129">Úloha 2 – zřídit připojené k doméně Windows servery toodeploy hello Azure AD aplikace konektor Proxy</span><span class="sxs-lookup"><span data-stu-id="c832e-129">Task 2 - Provision domain-joined Windows servers toodeploy hello Azure AD Application Proxy connector</span></span>
<span data-ttu-id="c832e-130">Je nutné připojený k doméně systému Windows Server virtuálních počítačů na které můžete nainstalovat konektor proxy aplikace služby Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="c832e-130">You need domain-joined Windows Server virtual machines on which you can install hello Azure AD Application Proxy connector.</span></span> <span data-ttu-id="c832e-131">V závislosti na aplikace hello publikovanému můžete se rozhodnout tooprovision více serverů, na kterých je nainstalován konektor hello.</span><span class="sxs-lookup"><span data-stu-id="c832e-131">Depending on hello applications being published, you may choose tooprovision multiple servers on which hello connector is installed.</span></span> <span data-ttu-id="c832e-132">Tuto volbu nasazení vám dává větší dostupnosti a pomáhá zpracování větší zátěže pak ověřování.</span><span class="sxs-lookup"><span data-stu-id="c832e-132">This deployment option gives you greater availability and helps handle heavier authentication loads.</span></span>

<span data-ttu-id="c832e-133">Přidělení servery konektoru hello na hello stejné virtuální síti (nebo připojení/peered virtuální sítě), ve kterém jste povolili vaší spravované domény služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="c832e-133">Provision hello connector servers on hello same virtual network (or a connected/peered virtual network), in which you have enabled your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="c832e-134">Podobně, musí servery hello hostování aplikací hello publikujete pomocí Proxy aplikace hello toobe nainstalovaná na hello stejné virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="c832e-134">Similarly, hello servers hosting hello applications you publish via hello Application Proxy need toobe installed on hello same Azure virtual network.</span></span>

<span data-ttu-id="c832e-135">servery konektoru tooprovision, postupujte podle uvedených v článku hello s názvem úlohy hello [připojit k spravované doméně systému Windows virtuálního počítače tooa](active-directory-ds-admin-guide-join-windows-vm.md).</span><span class="sxs-lookup"><span data-stu-id="c832e-135">tooprovision connector servers, follow hello tasks outlined in hello article titled [Join a Windows virtual machine tooa managed domain](active-directory-ds-admin-guide-join-windows-vm.md).</span></span>


## <a name="task-3---install-and-register-hello-azure-ad-application-proxy-connector"></a><span data-ttu-id="c832e-136">Úloha 3 – nainstalujte a zaregistrujte hello konektoru Proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c832e-136">Task 3 - Install and register hello Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="c832e-137">Dříve zřízení virtuálního počítače s Windows serverem a k ní připojili toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="c832e-137">Previously, you provisioned a Windows Server virtual machine and joined it toohello managed domain.</span></span> <span data-ttu-id="c832e-138">V této úloze nainstalujete konektor proxy aplikace služby Azure AD hello na tomto virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="c832e-138">In this task, you will install hello Azure AD Application Proxy connector on this virtual machine.</span></span>

1. <span data-ttu-id="c832e-139">Zkopírujte hello konektor instalace balíčku toohello virtuálních počítačů na který nainstalujete konektor Proxy aplikace Azure AD webových hello.</span><span class="sxs-lookup"><span data-stu-id="c832e-139">Copy hello connector installation package toohello VM on which you install hello Azure AD Web Application Proxy connector.</span></span>

2. <span data-ttu-id="c832e-140">Spustit **AADApplicationProxyConnectorInstaller.exe** hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c832e-140">Run **AADApplicationProxyConnectorInstaller.exe** on hello virtual machine.</span></span> <span data-ttu-id="c832e-141">Přijměte licenční podmínky pro software hello.</span><span class="sxs-lookup"><span data-stu-id="c832e-141">Accept hello software license terms.</span></span>

    ![Přijměte podmínky pro instalaci](./media/app-proxy/app-proxy-install-connector-terms.png)
3. <span data-ttu-id="c832e-143">Během instalace jsou výzvami tooregister hello konektor s hello Proxy aplikací z adresáře služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c832e-143">During installation, you are prompted tooregister hello connector with hello Application Proxy of your Azure AD directory.</span></span>
    * <span data-ttu-id="c832e-144">Zadejte vaše **přihlašovací údaje Azure AD globálního správce**.</span><span class="sxs-lookup"><span data-stu-id="c832e-144">Provide your **Azure AD global administrator credentials**.</span></span> <span data-ttu-id="c832e-145">Klient globálního správce se může lišit od vašich přihlašovacích údajů ke službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c832e-145">Your global administrator tenant may be different from your Microsoft Azure credentials.</span></span>
    * <span data-ttu-id="c832e-146">Hello správce účtu používaného tooregister hello konektor musí patřit toohello stejný adresář, kde jste povolili službu Proxy aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c832e-146">hello administrator account used tooregister hello connector must belong toohello same directory where you enabled hello Application Proxy service.</span></span> <span data-ttu-id="c832e-147">Například pokud hello klienta domény contoso.com, dobrý den, Správce musí být admin@contoso.com nebo jiný platný alias v této doméně.</span><span class="sxs-lookup"><span data-stu-id="c832e-147">For example, if hello tenant domain is contoso.com, hello admin should be admin@contoso.com or any other valid alias on that domain.</span></span>
    * <span data-ttu-id="c832e-148">Pokud je konfigurace rozšířeného zabezpečení aplikace Internet Explorer je vypnuté pro hello server hello konektor instalujete, mohou být blokovány úvodní obrazovka registrace.</span><span class="sxs-lookup"><span data-stu-id="c832e-148">If IE Enhanced Security Configuration is turned on for hello server where you are installing hello connector, hello registration screen might be blocked.</span></span> <span data-ttu-id="c832e-149">tooallow přístup, postupujte podle pokynů hello v hello chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="c832e-149">tooallow access, follow hello instructions in hello error message.</span></span> <span data-ttu-id="c832e-150">Ujistěte se, že je rozšířené zabezpečení aplikace Internet Explorer vypnuto.</span><span class="sxs-lookup"><span data-stu-id="c832e-150">Make sure that Internet Explorer Enhanced Security is off.</span></span>
    * <span data-ttu-id="c832e-151">Pokud registrace konektoru selže, podívejte se do článku [Poradce při potížích s proxy aplikace](../active-directory/active-directory-application-proxy-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="c832e-151">If connector registration does not succeed, see [Troubleshoot Application Proxy](../active-directory/active-directory-application-proxy-troubleshoot.md).</span></span>

    ![Nainstalovaný konektor](./media/app-proxy/app-proxy-connector-installed.png)
4. <span data-ttu-id="c832e-153">konektor hello tooensure funguje správně, spusťte hello Azure AD Application Proxy Connector Poradce při potížích.</span><span class="sxs-lookup"><span data-stu-id="c832e-153">tooensure hello connector works properly, run hello Azure AD Application Proxy Connector Troubleshooter.</span></span> <span data-ttu-id="c832e-154">Měli byste vidět úspěšné sestavy po spuštěné hello Poradce při potížích.</span><span class="sxs-lookup"><span data-stu-id="c832e-154">You should see a successful report after running hello troubleshooter.</span></span>

    ![Poradce při potížích s úspěch](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. <span data-ttu-id="c832e-156">Měli byste vidět hello nově nainstalovaný konektor uvedené na stránce proxy aplikace hello v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c832e-156">You should see hello newly installed connector listed on hello Application proxy page in your Azure AD directory.</span></span>

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> <span data-ttu-id="c832e-157">Můžete se rozhodnout tooinstall konektory na několika serverech tooguarantee vysoká dostupnost pro ověřování aplikacích publikovaných prostřednictvím hello proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c832e-157">You may choose tooinstall connectors on multiple servers tooguarantee high availability for authenticating applications published through hello Azure AD Application Proxy.</span></span> <span data-ttu-id="c832e-158">Proveďte hello stejné kroky uvedené výše tooinstall hello konektoru na jiné servery připojené k tooyour spravované domény.</span><span class="sxs-lookup"><span data-stu-id="c832e-158">Perform hello same steps listed above tooinstall hello connector on other servers joined tooyour managed domain.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="c832e-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c832e-159">Next Steps</span></span>
<span data-ttu-id="c832e-160">Máte nastavit hello proxy aplikace služby Azure AD a integraci s vaší spravované domény služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="c832e-160">You have set up hello Azure AD Application Proxy and integrated it with your Azure AD Domain Services managed domain.</span></span>

* <span data-ttu-id="c832e-161">**Migraci virtuálních počítačů aplikace tooAzure:** můžete navýšení a shift aplikace z místní servery tooAzure virtuální počítače připojené k tooyour spravované domény.</span><span class="sxs-lookup"><span data-stu-id="c832e-161">**Migrate your applications tooAzure virtual machines:** You can lift-and-shift your applications from on-premises servers tooAzure virtual machines joined tooyour managed domain.</span></span> <span data-ttu-id="c832e-162">Díky tomu pomáhá vám zbavit náklady na infrastrukturu hello spuštěných servery místně.</span><span class="sxs-lookup"><span data-stu-id="c832e-162">Doing so helps you get rid of hello infrastructure costs of running servers on-premises.</span></span>

* <span data-ttu-id="c832e-163">**Publikování aplikací pomocí proxy aplikace služby Azure AD:** publikování aplikací, které běží v Azure virtuální počítače pomocí hello proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c832e-163">**Publish applications using Azure AD Application Proxy:** Publish applications running on your Azure virtual machines using hello Azure AD Application Proxy.</span></span> <span data-ttu-id="c832e-164">Další informace najdete v tématu [publikování aplikací pomocí proxy aplikace služby Azure AD](../active-directory/application-proxy-publish-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c832e-164">For more information, see [publish applications using Azure AD Application Proxy](../active-directory/application-proxy-publish-azure-portal.md)</span></span>


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a><span data-ttu-id="c832e-165">Poznámka: nasazení - publikování integrované ověřování systému Windows (integrované ověřování systému Windows) aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c832e-165">Deployment note - Publish IWA (Integrated Windows Authentication) applications using Azure AD Application Proxy</span></span>
<span data-ttu-id="c832e-166">Umožňují aplikacím tooyour přihlašování pomocí integrovaného ověřování systému Windows (IWA) udělení oprávnění konektory Proxy aplikace tooimpersonate uživatelů a odesílat a přijímat tokeny jejich jménem.</span><span class="sxs-lookup"><span data-stu-id="c832e-166">Enable single sign-on tooyour applications using Integrated Windows Authentication (IWA) by granting Application Proxy Connectors permission tooimpersonate users, and send and receive tokens on their behalf.</span></span> <span data-ttu-id="c832e-167">Nakonfigurujte omezené delegování protokolu kerberos (použitím KCD) hello konektor toogrant hello požadované oprávnění tooaccess prostředky v hello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="c832e-167">Configure kerberos constrained delegation (KCD) for hello connector toogrant hello required permissions tooaccess resources on hello managed domain.</span></span> <span data-ttu-id="c832e-168">Pro zvýšení zabezpečení pomocí hello založené na prostředcích použitím KCD mechanismus spravované domény.</span><span class="sxs-lookup"><span data-stu-id="c832e-168">Use hello resource-based KCD mechanism on managed domains for increased security.</span></span>


### <a name="enable-resource-based-kerberos-constrained-delegation-for-hello-azure-ad-application-proxy-connector"></a><span data-ttu-id="c832e-169">Povolit založené na prostředcích omezené delegování protokolu kerberos pro konektor Proxy aplikace hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c832e-169">Enable resource-based kerberos constrained delegation for hello Azure AD Application Proxy connector</span></span>
<span data-ttu-id="c832e-170">konektor Proxy aplikace služby Azure Hello musí být nakonfigurovaný omezeného delegování protokolu kerberos použitím (KCD), takže je možné zosobňovat uživatele na spravované doméně hello.</span><span class="sxs-lookup"><span data-stu-id="c832e-170">hello Azure Application Proxy connector should be configured for kerberos constrained delegation (KCD), so it can impersonate users on hello managed domain.</span></span> <span data-ttu-id="c832e-171">Na spravované doméně služby Azure AD Domain Services nemáte oprávnění správce domény.</span><span class="sxs-lookup"><span data-stu-id="c832e-171">On an Azure AD Domain Services managed domain, you do not have domain administrator privileges.</span></span> <span data-ttu-id="c832e-172">Proto **tradiční použitím KCD úrovni účtu nelze konfigurovat ve spravované doméně**.</span><span class="sxs-lookup"><span data-stu-id="c832e-172">Therefore, **traditional account-level KCD cannot be configured on a managed domain**.</span></span>

<span data-ttu-id="c832e-173">Použít použitím KCD založené na prostředcích, jak je popsáno v tomto [článku](active-directory-ds-enable-kcd.md).</span><span class="sxs-lookup"><span data-stu-id="c832e-173">Use resource-based KCD as described in this [article](active-directory-ds-enable-kcd.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c832e-174">Je třeba toobe členem skupiny "Administrators AAD řadiče domény" hello, tooadminister hello spravované doméně pomocí rutin prostředí AD PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c832e-174">You need toobe a member of hello 'AAD DC Administrators' group, tooadminister hello managed domain using AD PowerShell cmdlets.</span></span>
>
>

<span data-ttu-id="c832e-175">Použijte hello prostředí PowerShell Get-ADComputer rutiny tooretrieve hello nastavení pro hello počítač, na které hello Azure AD Application Proxy connector nainstalován.</span><span class="sxs-lookup"><span data-stu-id="c832e-175">Use hello Get-ADComputer PowerShell cmdlet tooretrieve hello settings for hello computer on which hello Azure AD Application Proxy connector is installed.</span></span>
```
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

<span data-ttu-id="c832e-176">Následně pomocí tooset rutiny Set-ADComputer hello až založené na prostředcích použitím KCD server hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="c832e-176">Thereafter, use hello Set-ADComputer cmdlet tooset up resource-based KCD for hello resource server.</span></span>
```
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

<span data-ttu-id="c832e-177">Pokud jste nasadili více konektorů Proxy aplikace na vaší spravované domény, je třeba tooconfigure založené na prostředcích použitím KCD pro každou konektor instanci.</span><span class="sxs-lookup"><span data-stu-id="c832e-177">If you have deployed multiple Application Proxy connectors on your managed domain, you need tooconfigure resource-based KCD for each such connector instance.</span></span>


## <a name="related-content"></a><span data-ttu-id="c832e-178">Související obsah</span><span class="sxs-lookup"><span data-stu-id="c832e-178">Related Content</span></span>
* [<span data-ttu-id="c832e-179">Azure AD Domain Services – Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="c832e-179">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="c832e-180">Nakonfigurovat omezené delegování protokolu Kerberos na spravované doméně</span><span class="sxs-lookup"><span data-stu-id="c832e-180">Configure Kerberos Constrained Delegation on a managed domain</span></span>](active-directory-ds-enable-kcd.md)
* [<span data-ttu-id="c832e-181">Omezené delegování přehled protokolu Kerberos</span><span class="sxs-lookup"><span data-stu-id="c832e-181">Kerberos Constrained Delegation Overview</span></span>](https://technet.microsoft.com/library/jj553400.aspx)
