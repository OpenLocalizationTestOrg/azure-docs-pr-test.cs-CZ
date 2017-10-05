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
ms.openlocfilehash: c158c67a82e12501386179e19bc75fd852d7e308
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="eca43-103">Nasazení Azure AD Application Proxy na spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="eca43-103">Deploy Azure AD Application Proxy on an Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="eca43-104">Proxy aplikace služby Azure Active Directory (AD) umožňuje podporují zaměstnanci na vzdálených pracovištích a publikování aplikací místní přístup přes internet.</span><span class="sxs-lookup"><span data-stu-id="eca43-104">Azure Active Directory (AD) Application Proxy helps you support remote workers by publishing on-premises applications to be accessed over the internet.</span></span> <span data-ttu-id="eca43-105">S Azure AD Domain Services můžete nyní navýšení a shift starší aplikace spuštěné místně službám infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="eca43-105">With Azure AD Domain Services, you can now lift-and-shift legacy applications running on-premises to Azure Infrastructure Services.</span></span> <span data-ttu-id="eca43-106">Potom můžete publikovat tyto aplikace pomocí Azure AD Application Proxy poskytnout zabezpečený vzdálený přístup na uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="eca43-106">You can then publish these applications using the Azure AD Application Proxy, to provide secure remote access to users in your organization.</span></span>

<span data-ttu-id="eca43-107">Pokud jste ještě Azure AD Application Proxy, další informace o této funkci se v následujícím článku: [jak poskytnout zabezpečený vzdálený přístup k místním aplikacím](../active-directory/active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="eca43-107">If you're new to the Azure AD Application Proxy, learn more about this feature with the following article: [How to provide secure remote access to on-premises applications](../active-directory/active-directory-application-proxy-get-started.md).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="eca43-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="eca43-108">Before you begin</span></span>
<span data-ttu-id="eca43-109">Chcete-li provést úkoly vypsané v tomto článku, je třeba:</span><span class="sxs-lookup"><span data-stu-id="eca43-109">To perform the tasks listed in this article, you need:</span></span>

1. <span data-ttu-id="eca43-110">Platná **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="eca43-110">A valid **Azure subscription**.</span></span>
2. <span data-ttu-id="eca43-111">**Adresář Azure AD** – buď synchronizovány s místní adresář nebo výhradně cloudový adresář.</span><span class="sxs-lookup"><span data-stu-id="eca43-111">An **Azure AD directory** - either synchronized with an on-premises directory or a cloud-only directory.</span></span>
3. <span data-ttu-id="eca43-112">**Licence Azure AD Basic nebo Premium** je potřeba použít Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="eca43-112">An **Azure AD Basic or Premium license** is required to use the Azure AD Application Proxy.</span></span>
4. <span data-ttu-id="eca43-113">**Azure AD Domain Services** musí být povolen pro adresář Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eca43-113">**Azure AD Domain Services** must be enabled for the Azure AD directory.</span></span> <span data-ttu-id="eca43-114">Pokud jste tak dosud neučinili, postupujte podle všechny úkoly popsané v [příručce Začínáme](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="eca43-114">If you haven't done so, follow all the tasks outlined in the [Getting Started guide](active-directory-ds-getting-started.md).</span></span>

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a><span data-ttu-id="eca43-115">Úloha 1 – povolení služby Azure AD Application Proxy pro váš adresář Azure AD</span><span class="sxs-lookup"><span data-stu-id="eca43-115">Task 1 - Enable Azure AD Application Proxy for your Azure AD directory</span></span>
<span data-ttu-id="eca43-116">Proveďte následující kroky k povolení Azure AD Application Proxy pro váš adresář Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eca43-116">Perform the following steps to enable the Azure AD Application Proxy for your Azure AD directory.</span></span>

1. <span data-ttu-id="eca43-117">Přihlaste se jako správce v [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="eca43-117">Sign in as an administrator in the [Azure portal](http://portal.azure.com).</span></span>

2. <span data-ttu-id="eca43-118">Klikněte na tlačítko **Azure Active Directory** se zprovoznit přehled adresáře.</span><span class="sxs-lookup"><span data-stu-id="eca43-118">Click **Azure Active Directory** to bring up the directory overview.</span></span> <span data-ttu-id="eca43-119">Klikněte na tlačítko **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="eca43-119">Click **Enterprise applications**.</span></span>

    ![Vyberte adresář Azure AD](./media/app-proxy/app-proxy-enable-start.png)
3. <span data-ttu-id="eca43-121">Klikněte na tlačítko **proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="eca43-121">Click **Application proxy**.</span></span> <span data-ttu-id="eca43-122">Pokud nemáte předplatné Azure AD Basic nebo Azure AD Premium, zobrazí se možnost povolit zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="eca43-122">If you do not have an Azure AD Basic or Azure AD Premium subscription, you see an option to enable a trial.</span></span> <span data-ttu-id="eca43-123">Přepnutí **povolení Proxy aplikace?** k **povolit** a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="eca43-123">Toggle **Enable Application Proxy?** to **Enable** and click **Save**.</span></span>

    ![Povolit Proxy aplikace](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. <span data-ttu-id="eca43-125">Chcete-li stáhnout konektor, klikněte na tlačítko **konektor** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eca43-125">To download the connector, click the **Connector** button.</span></span>

    ![Stažení konektoru](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. <span data-ttu-id="eca43-127">Na stránce pro stahování, přijměte licenční podmínky a smlouvy o ochraně osobních údajů a klikněte na **Stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eca43-127">On the download page, accept the license terms and privacy agreement and click the **Download** button.</span></span>

    ![Potvrďte stahování](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-to-deploy-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="eca43-129">Úloha 2 – zřídit připojené k doméně Windows servery nasaďte konektor proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="eca43-129">Task 2 - Provision domain-joined Windows servers to deploy the Azure AD Application Proxy connector</span></span>
<span data-ttu-id="eca43-130">Je nutné připojený k doméně systému Windows Server virtuálních počítačů na které můžete nainstalovat konektor proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eca43-130">You need domain-joined Windows Server virtual machines on which you can install the Azure AD Application Proxy connector.</span></span> <span data-ttu-id="eca43-131">V závislosti na publikování aplikace můžete zřídit několik serverů, na kterých je nainstalován konektor.</span><span class="sxs-lookup"><span data-stu-id="eca43-131">Depending on the applications being published, you may choose to provision multiple servers on which the connector is installed.</span></span> <span data-ttu-id="eca43-132">Tuto volbu nasazení vám dává větší dostupnosti a pomáhá zpracování větší zátěže pak ověřování.</span><span class="sxs-lookup"><span data-stu-id="eca43-132">This deployment option gives you greater availability and helps handle heavier authentication loads.</span></span>

<span data-ttu-id="eca43-133">Zřídit servery konektoru na stejné virtuální síti (nebo připojení/peered virtuální sítě), ve kterých jste povolili vaší spravované domény služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="eca43-133">Provision the connector servers on the same virtual network (or a connected/peered virtual network), in which you have enabled your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="eca43-134">Podobně servery, které hostují aplikace, které publikujete pomocí Proxy aplikace je potřeba nainstalovat na stejnou virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="eca43-134">Similarly, the servers hosting the applications you publish via the Application Proxy need to be installed on the same Azure virtual network.</span></span>

<span data-ttu-id="eca43-135">Chcete-li zřídit na serverech konektoru, postupujte podle úkoly popsané v článku s názvem [připojení virtuálního počítače s Windows k spravované doméně](active-directory-ds-admin-guide-join-windows-vm.md).</span><span class="sxs-lookup"><span data-stu-id="eca43-135">To provision connector servers, follow the tasks outlined in the article titled [Join a Windows virtual machine to a managed domain](active-directory-ds-admin-guide-join-windows-vm.md).</span></span>


## <a name="task-3---install-and-register-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="eca43-136">Úloha 3 – instalace a registrace konektoru Proxy aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="eca43-136">Task 3 - Install and register the Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="eca43-137">Dříve zřízení virtuálního počítače s Windows serverem a připojený k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="eca43-137">Previously, you provisioned a Windows Server virtual machine and joined it to the managed domain.</span></span> <span data-ttu-id="eca43-138">V této úloze nainstalujete konektor proxy aplikace služby Azure AD na tomto virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="eca43-138">In this task, you will install the Azure AD Application Proxy connector on this virtual machine.</span></span>

1. <span data-ttu-id="eca43-139">Zkopírujte instalační balíček konektoru na virtuální počítač, na který nainstalujete konektor Proxy Azure AD webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="eca43-139">Copy the connector installation package to the VM on which you install the Azure AD Web Application Proxy connector.</span></span>

2. <span data-ttu-id="eca43-140">Spustit **AADApplicationProxyConnectorInstaller.exe** na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="eca43-140">Run **AADApplicationProxyConnectorInstaller.exe** on the virtual machine.</span></span> <span data-ttu-id="eca43-141">Přijměte licenční podmínky pro software.</span><span class="sxs-lookup"><span data-stu-id="eca43-141">Accept the software license terms.</span></span>

    ![Přijměte podmínky pro instalaci](./media/app-proxy/app-proxy-install-connector-terms.png)
3. <span data-ttu-id="eca43-143">Během instalace zobrazí se výzva k registraci konektoru k Proxy aplikace z adresáře služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eca43-143">During installation, you are prompted to register the connector with the Application Proxy of your Azure AD directory.</span></span>
    * <span data-ttu-id="eca43-144">Zadejte vaše **přihlašovací údaje Azure AD globálního správce**.</span><span class="sxs-lookup"><span data-stu-id="eca43-144">Provide your **Azure AD global administrator credentials**.</span></span> <span data-ttu-id="eca43-145">Klient globálního správce se může lišit od vašich přihlašovacích údajů ke službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="eca43-145">Your global administrator tenant may be different from your Microsoft Azure credentials.</span></span>
    * <span data-ttu-id="eca43-146">Účet správce použitý k registraci konektoru musí patřit do stejného adresáře, kde jste povolili službu Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="eca43-146">The administrator account used to register the connector must belong to the same directory where you enabled the Application Proxy service.</span></span> <span data-ttu-id="eca43-147">Například, pokud doména klienta je contoso.com, Správce by měl být admin@contoso.com nebo jiný platný alias v této doméně.</span><span class="sxs-lookup"><span data-stu-id="eca43-147">For example, if the tenant domain is contoso.com, the admin should be admin@contoso.com or any other valid alias on that domain.</span></span>
    * <span data-ttu-id="eca43-148">Pokud konfigurace rozšířeného zabezpečení aplikace Internet Explorer je zapnutá pro server který konektor instalujete, mohou být blokovány na obrazovce pro registraci.</span><span class="sxs-lookup"><span data-stu-id="eca43-148">If IE Enhanced Security Configuration is turned on for the server where you are installing the connector, the registration screen might be blocked.</span></span> <span data-ttu-id="eca43-149">Chcete-li povolit přístup, postupujte podle pokynů v chybové zprávě.</span><span class="sxs-lookup"><span data-stu-id="eca43-149">To allow access, follow the instructions in the error message.</span></span> <span data-ttu-id="eca43-150">Ujistěte se, že je rozšířené zabezpečení aplikace Internet Explorer vypnuto.</span><span class="sxs-lookup"><span data-stu-id="eca43-150">Make sure that Internet Explorer Enhanced Security is off.</span></span>
    * <span data-ttu-id="eca43-151">Pokud registrace konektoru selže, podívejte se do článku [Poradce při potížích s proxy aplikace](../active-directory/active-directory-application-proxy-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="eca43-151">If connector registration does not succeed, see [Troubleshoot Application Proxy](../active-directory/active-directory-application-proxy-troubleshoot.md).</span></span>

    ![Nainstalovaný konektor](./media/app-proxy/app-proxy-connector-installed.png)
4. <span data-ttu-id="eca43-153">Aby konektor funguje správně, spusťte Azure AD Application Proxy Connector Poradce při potížích.</span><span class="sxs-lookup"><span data-stu-id="eca43-153">To ensure the connector works properly, run the Azure AD Application Proxy Connector Troubleshooter.</span></span> <span data-ttu-id="eca43-154">Měli byste vidět úspěšné sestavy po spuštění Poradce při potížích.</span><span class="sxs-lookup"><span data-stu-id="eca43-154">You should see a successful report after running the troubleshooter.</span></span>

    ![Poradce při potížích s úspěch](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. <span data-ttu-id="eca43-156">Měli byste vidět nově nainstalovaný konektor uvedené na stránce proxy aplikace v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eca43-156">You should see the newly installed connector listed on the Application proxy page in your Azure AD directory.</span></span>

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> <span data-ttu-id="eca43-157">Můžete se rozhodnout postup instalace konektorů na několika serverech zaručit vysoká dostupnost pro ověřování aplikacích publikovaných prostřednictvím Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="eca43-157">You may choose to install connectors on multiple servers to guarantee high availability for authenticating applications published through the Azure AD Application Proxy.</span></span> <span data-ttu-id="eca43-158">Proveďte stejné kroky uvedené výše, aby konektor nainstalovat na jiné servery připojené k vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="eca43-158">Perform the same steps listed above to install the connector on other servers joined to your managed domain.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="eca43-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eca43-159">Next Steps</span></span>
<span data-ttu-id="eca43-160">Máte nenastavili Proxy aplikace služby Azure AD a integraci s vaší spravované domény služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="eca43-160">You have set up the Azure AD Application Proxy and integrated it with your Azure AD Domain Services managed domain.</span></span>

* <span data-ttu-id="eca43-161">**Migrovat aplikace na virtuálních počítačích Azure:** můžete navýšení a shift aplikace z místních serverů na virtuálních počítačích Azure připojené k vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="eca43-161">**Migrate your applications to Azure virtual machines:** You can lift-and-shift your applications from on-premises servers to Azure virtual machines joined to your managed domain.</span></span> <span data-ttu-id="eca43-162">Díky tomu pomáhá vám zbavit infrastruktury nákladů na běžící servery místně.</span><span class="sxs-lookup"><span data-stu-id="eca43-162">Doing so helps you get rid of the infrastructure costs of running servers on-premises.</span></span>

* <span data-ttu-id="eca43-163">**Publikování aplikací pomocí proxy aplikace služby Azure AD:** publikování aplikací běžících na virtuálních počítačích Azure pomocí Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="eca43-163">**Publish applications using Azure AD Application Proxy:** Publish applications running on your Azure virtual machines using the Azure AD Application Proxy.</span></span> <span data-ttu-id="eca43-164">Další informace najdete v tématu [publikování aplikací pomocí proxy aplikace služby Azure AD](../active-directory/application-proxy-publish-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="eca43-164">For more information, see [publish applications using Azure AD Application Proxy](../active-directory/application-proxy-publish-azure-portal.md)</span></span>


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a><span data-ttu-id="eca43-165">Poznámka: nasazení - publikování integrované ověřování systému Windows (integrované ověřování systému Windows) aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="eca43-165">Deployment note - Publish IWA (Integrated Windows Authentication) applications using Azure AD Application Proxy</span></span>
<span data-ttu-id="eca43-166">Povolte jednotné přihlašování pro vaše aplikace používá integrované ověřování systému Windows (IWA) udělení oprávnění konektory Proxy aplikace zosobňovat uživatele a odesílat a přijímat tokeny jejich jménem.</span><span class="sxs-lookup"><span data-stu-id="eca43-166">Enable single sign-on to your applications using Integrated Windows Authentication (IWA) by granting Application Proxy Connectors permission to impersonate users, and send and receive tokens on their behalf.</span></span> <span data-ttu-id="eca43-167">Nakonfigurujte omezené delegování protokolu kerberos (použitím KCD) pro konektor pro přidělení požadovaných oprávnění pro přístup k prostředkům na spravované domény.</span><span class="sxs-lookup"><span data-stu-id="eca43-167">Configure kerberos constrained delegation (KCD) for the connector to grant the required permissions to access resources on the managed domain.</span></span> <span data-ttu-id="eca43-168">Pro zvýšení zabezpečení pomocí použitím KCD mechanismus založené na prostředcích spravované domény.</span><span class="sxs-lookup"><span data-stu-id="eca43-168">Use the resource-based KCD mechanism on managed domains for increased security.</span></span>


### <a name="enable-resource-based-kerberos-constrained-delegation-for-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="eca43-169">Povolit založené na prostředcích omezené delegování protokolu kerberos pro konektor proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="eca43-169">Enable resource-based kerberos constrained delegation for the Azure AD Application Proxy connector</span></span>
<span data-ttu-id="eca43-170">Konektor Proxy aplikace služby Azure na by měl být nakonfigurovaný omezeného delegování protokolu kerberos použitím (KCD), je možné zosobňovat uživatele spravované domény.</span><span class="sxs-lookup"><span data-stu-id="eca43-170">The Azure Application Proxy connector should be configured for kerberos constrained delegation (KCD), so it can impersonate users on the managed domain.</span></span> <span data-ttu-id="eca43-171">Na spravované doméně služby Azure AD Domain Services nemáte oprávnění správce domény.</span><span class="sxs-lookup"><span data-stu-id="eca43-171">On an Azure AD Domain Services managed domain, you do not have domain administrator privileges.</span></span> <span data-ttu-id="eca43-172">Proto **tradiční použitím KCD úrovni účtu nelze konfigurovat ve spravované doméně**.</span><span class="sxs-lookup"><span data-stu-id="eca43-172">Therefore, **traditional account-level KCD cannot be configured on a managed domain**.</span></span>

<span data-ttu-id="eca43-173">Použít použitím KCD založené na prostředcích, jak je popsáno v tomto [článku](active-directory-ds-enable-kcd.md).</span><span class="sxs-lookup"><span data-stu-id="eca43-173">Use resource-based KCD as described in this [article](active-directory-ds-enable-kcd.md).</span></span>

> [!NOTE]
> <span data-ttu-id="eca43-174">Musíte být členem skupiny 'správci AAD řadič domény, ke správě spravované doméně pomocí rutin prostředí AD PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eca43-174">You need to be a member of the 'AAD DC Administrators' group, to administer the managed domain using AD PowerShell cmdlets.</span></span>
>
>

<span data-ttu-id="eca43-175">Použijte rutinu Get-ADComputer prostředí PowerShell k načtení nastavení pro počítač, na kterém je nainstalovaný konektor proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eca43-175">Use the Get-ADComputer PowerShell cmdlet to retrieve the settings for the computer on which the Azure AD Application Proxy connector is installed.</span></span>
```
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

<span data-ttu-id="eca43-176">Po tomto datu použijte rutinu Set-ADComputer nastavit založené na prostředcích použitím KCD server prostředku.</span><span class="sxs-lookup"><span data-stu-id="eca43-176">Thereafter, use the Set-ADComputer cmdlet to set up resource-based KCD for the resource server.</span></span>
```
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

<span data-ttu-id="eca43-177">Pokud jste nasadili více konektorů Proxy aplikace na vaší spravované domény, musíte nakonfigurovat založené na prostředcích použitím KCD pro každou konektor instanci.</span><span class="sxs-lookup"><span data-stu-id="eca43-177">If you have deployed multiple Application Proxy connectors on your managed domain, you need to configure resource-based KCD for each such connector instance.</span></span>


## <a name="related-content"></a><span data-ttu-id="eca43-178">Související obsah</span><span class="sxs-lookup"><span data-stu-id="eca43-178">Related Content</span></span>
* [<span data-ttu-id="eca43-179">Azure AD Domain Services – Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="eca43-179">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="eca43-180">Nakonfigurovat omezené delegování protokolu Kerberos na spravované doméně</span><span class="sxs-lookup"><span data-stu-id="eca43-180">Configure Kerberos Constrained Delegation on a managed domain</span></span>](active-directory-ds-enable-kcd.md)
* [<span data-ttu-id="eca43-181">Omezené delegování přehled protokolu Kerberos</span><span class="sxs-lookup"><span data-stu-id="eca43-181">Kerberos Constrained Delegation Overview</span></span>](https://technet.microsoft.com/library/jj553400.aspx)
