---
title: "Odsud se tam nelze dostat na webu Azure Portal ze zařízení s Windows | Dokumentace Microsoftu"
description: "Dozvíte se, odkud pochází zpráva, že odsud se tam nelze dostat, a co můžete zkontrolovat, aby se vám toto dialogové okno nezobrazovalo."
services: active-directory
keywords: "přístup podmíněný zařízením, registrace zařízení, povolení registrace zařízení, registrace zařízení a MDM"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 16543c7bb7b6b236dcc24093c9963bc218ca1fa6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a><span data-ttu-id="5a3d6-104">Odsud se tam nelze dostat pro zařízení s Windows</span><span class="sxs-lookup"><span data-stu-id="5a3d6-104">You can't get there from here on a Windows device</span></span>

<span data-ttu-id="5a3d6-105">Při pokusu například o přístup k intranetu Sharepointu Online ve vaší organizaci můžete narazit na stránku, která uvádí, že *odsud se tam nelze dostat*.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-105">During an attempt to, for example, access your organization's SharePoint Online intranet you might run into a page that states that *you can't get there from here*.</span></span> <span data-ttu-id="5a3d6-106">Tato stránka se zobrazuje, protože správce nakonfiguroval zásady podmíněného přístupu, které za určitých podmínek brání v přístupu k prostředkům vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-106">You are seeing this page, because your administrator has configured a conditional access policy that prevents access to your organization's resources under certain conditions.</span></span> <span data-ttu-id="5a3d6-107">Možná bude nutné se při řešení těchto potíží obrátit na helpdesk nebo správce, ale existuje několik věcí, které můžete nejdřív vyzkoušet sami.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-107">While it might be necessary to contact helpdesk or your administrator to get this problem solved, there are a few things you can try out yourself, first.</span></span>

<span data-ttu-id="5a3d6-108">Pokud používáte zařízení s **Windows**, měli byste zkontrolovat toto:</span><span class="sxs-lookup"><span data-stu-id="5a3d6-108">If you are using a **Windows** device, you should check the following:</span></span>

- <span data-ttu-id="5a3d6-109">Používáte podporovaný prohlížeč?</span><span class="sxs-lookup"><span data-stu-id="5a3d6-109">Are you using a supported browser?</span></span>

- <span data-ttu-id="5a3d6-110">Používáte ve vašem zařízení podporovanou verzi systému Windows?</span><span class="sxs-lookup"><span data-stu-id="5a3d6-110">Are you running a supported version of Windows on your device?</span></span>

- <span data-ttu-id="5a3d6-111">Odpovídá vaše zařízení požadavkům?</span><span class="sxs-lookup"><span data-stu-id="5a3d6-111">Is your device compliant?</span></span>






## <a name="supported-browser"></a><span data-ttu-id="5a3d6-112">Podporovaný prohlížeč</span><span class="sxs-lookup"><span data-stu-id="5a3d6-112">Supported browser</span></span>

<span data-ttu-id="5a3d6-113">Pokud váš správce nakonfiguroval zásady podmíněného přístupu, můžete pro přístup k prostředkům vaší organizace používat jenom podporovaný prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-113">If your administrator has configured a conditional access policy, you can only access your organization's resources by using a supported browser.</span></span> <span data-ttu-id="5a3d6-114">V zařízeních se systémem Windows se podporují jenom prohlížeče **Internet Explorer** a **Edge**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-114">On a Windows device, only **Internet Explorer** and **Edge** are supported.</span></span>

<span data-ttu-id="5a3d6-115">To, jestli k prostředku nejde přistupovat kvůli nepodporovanému prohlížeči, snadno zjistíte v sekci podrobností chybové stránky:</span><span class="sxs-lookup"><span data-stu-id="5a3d6-115">You can easily identify whether you can't access a resource due to an unsupported browser by looking at the details section of the error page:</span></span>

<span data-ttu-id="5a3d6-116">![Zpráva „Odsud se tam nelze dostat“ pro nepodporované prohlížeče](./media/active-directory-conditional-access-device-remediation/02.png "Scénář")</span><span class="sxs-lookup"><span data-stu-id="5a3d6-116">!["You can't get there from here" message for unsupported browsers](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")</span></span>

<span data-ttu-id="5a3d6-117">Jedinou možností odstranění problému je použít prohlížeč, který aplikace pro platformu vašeho zařízení podporuje.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-117">The only remediation is to use a browser that the application supports for your device platform.</span></span> <span data-ttu-id="5a3d6-118">Úplný seznam podporovaných prohlížečů najdete v části [Podporované prohlížeče](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).</span><span class="sxs-lookup"><span data-stu-id="5a3d6-118">For a complete list of supported browsers, see [supported browsers](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).</span></span>  


## <a name="supported-versions-of-windows"></a><span data-ttu-id="5a3d6-119">Podporované verze Windows</span><span class="sxs-lookup"><span data-stu-id="5a3d6-119">Supported versions of Windows</span></span>

<span data-ttu-id="5a3d6-120">Pro operační systém Windows v zařízení musí být splněné tyto podmínky:</span><span class="sxs-lookup"><span data-stu-id="5a3d6-120">The following must be true about the Windows operating system on your device:</span></span> 

- <span data-ttu-id="5a3d6-121">Pokud v zařízení používáte desktopový operační systém Windows, musí to být systém Windows 7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-121">If you are running a Windows desktop operating system on your device, it needs to be Windows 7 or later.</span></span>
- <span data-ttu-id="5a3d6-122">Pokud v zařízení používáte serverový operační systém Windows, musí to být systém Windows Server 2008 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-122">If you are running a Windows server operating system on your device, it needs to be Windows Server 2008 R2 or later.</span></span> 


## <a name="compliant-device"></a><span data-ttu-id="5a3d6-123">Odpovídající zařízení</span><span class="sxs-lookup"><span data-stu-id="5a3d6-123">Compliant device</span></span>

<span data-ttu-id="5a3d6-124">Je možné, že správce nakonfiguroval zásady podmíněného přístupu, které umožňují přístup k prostředkům vaší organizace jenom z odpovídajících zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-124">Your administrator might have configured a conditional access policy that allows access to your organization's resources only from compliant devices.</span></span> <span data-ttu-id="5a3d6-125">Zařízení je odpovídající, pokud je připojené k místní službě Active Directory nebo ke službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-125">To be compliant, your device must be either joined to your on-premises Active Directory or joined to your Azure Active Directory.</span></span>

<span data-ttu-id="5a3d6-126">To, jestli k prostředku nejde přistupovat kvůli zařízení, které neodpovídá požadavkům, snadno zjistíte v sekci podrobností chybové stránky:</span><span class="sxs-lookup"><span data-stu-id="5a3d6-126">You can easily identify whether you can't access a resource due to a device that is not compliant by looking at the details section of the error page:</span></span>
 
<span data-ttu-id="5a3d6-127">![Zprávy „Odsud se tam nelze dostat“ pro neregistrovaná zařízení](./media/active-directory-conditional-access-device-remediation/01.png "Scénář")</span><span class="sxs-lookup"><span data-stu-id="5a3d6-127">!["You can't get there from here" messages for unregistered devices](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")</span></span>


### <a name="is-your-device-joined-to-an-on-premises-active-directory"></a><span data-ttu-id="5a3d6-128">Je zařízení připojené k místní službě Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5a3d6-128">Is your device joined to an on-premises Active Directory?</span></span>

<span data-ttu-id="5a3d6-129">**Pokud je zařízení připojené k místní službě Active Directory ve vaší organizaci:**</span><span class="sxs-lookup"><span data-stu-id="5a3d6-129">**If your device is joined to an on-premises Active Directory in your organization:**</span></span>

1. <span data-ttu-id="5a3d6-130">Přihlaste se k Windows pomocí svého pracovního účtu (účtu služby Active Directory).</span><span class="sxs-lookup"><span data-stu-id="5a3d6-130">Make sure that you sign in to Windows by using your work account (your Active Directory account).</span></span>
2. <span data-ttu-id="5a3d6-131">Připojte se k podnikové síti prostřednictvím virtuální privátní sítě (VPN) nebo technologie DirectAccess.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-131">Connect to your corporate network via a virtual private network (VPN) or DirectAccess.</span></span>
3. <span data-ttu-id="5a3d6-132">Jakmile budete připojeni, uzamkněte relaci Windows stisknutím kláves Windows + L.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-132">After you are connected, press the Windows logo key + the L key to lock your Windows session.</span></span>
4. <span data-ttu-id="5a3d6-133">Odemkněte relaci Windows zadáním přihlašovacích údajů ke svému pracovnímu účtu.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-133">Unlock your Windows session by entering your work account credentials.</span></span>
5. <span data-ttu-id="5a3d6-134">Počkejte zhruba minutu a znovu se pokuste o přístup k aplikaci nebo službě.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-134">Wait for a minute, and then try again to access the application or service.</span></span>
6. <span data-ttu-id="5a3d6-135">Pokud se zobrazí stejná stránka, obraťte se na svého správce a sdělte mu podrobnosti, které získáte po kliknutí na odkaz **Podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-135">If you see the same page, click the **More details** link, and then contact your administrator with the details.</span></span>


### <a name="is-your-device-not-joined-to-an-on-premises-active-directory"></a><span data-ttu-id="5a3d6-136">Zařízení není připojené k místní službě Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5a3d6-136">Is your device not joined to an on-premises Active Directory?</span></span>

<span data-ttu-id="5a3d6-137">Pokud zařízení není připojené k místní službě Active Directory a používá systém Windows 10, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="5a3d6-137">If your device is not joined to an on-premises Active Directory and runs Windows 10, you have two options:</span></span>

* <span data-ttu-id="5a3d6-138">Spusťte službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="5a3d6-138">Run Azure AD Join</span></span>
* <span data-ttu-id="5a3d6-139">Přidejte svůj pracovní nebo školní účet do Windows.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-139">Add your work or school account to Windows</span></span>

<span data-ttu-id="5a3d6-140">Informace o rozdílech mezi těmito dvěma možnostmi najdete v článku [Používání zařízení s Windows 10 na pracovišti](active-directory-azureadjoin-windows10-devices.md).</span><span class="sxs-lookup"><span data-stu-id="5a3d6-140">For information about how these options are different, see [Using Windows 10 devices in your workplace](active-directory-azureadjoin-windows10-devices.md).</span></span>  
<span data-ttu-id="5a3d6-141">Pokud zařízení:</span><span class="sxs-lookup"><span data-stu-id="5a3d6-141">If your device:</span></span>

- <span data-ttu-id="5a3d6-142">Patří vaší organizaci, měli byste měli spustit Azure AD Join.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-142">Belongs to your organization, you should run Azure AD Join.</span></span>
- <span data-ttu-id="5a3d6-143">Je osobním zařízením nebo je to Windows Phone, měli byste do Windows přidat svůj pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-143">Is a personal device or a Windows phone, you should add your work or school account to Windows</span></span> 



#### <a name="azure-ad-join-on-windows-10"></a><span data-ttu-id="5a3d6-144">Azure AD Join v systému Windows 10</span><span class="sxs-lookup"><span data-stu-id="5a3d6-144">Azure AD Join on Windows 10</span></span>

<span data-ttu-id="5a3d6-145">Kroky pro připojení zařízení k Azure AD závisejí na verzi Windows 10, kterou na tomto zařízení spouštíte.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-145">The steps to join your device to Azure AD are tied the version of Windows 10 you are running on it.</span></span> <span data-ttu-id="5a3d6-146">Pokud chcete zjistit verzi operačního systému Windows 10, spusťte příkaz **winver**:</span><span class="sxs-lookup"><span data-stu-id="5a3d6-146">To determine the version of your Windows 10 operating system, run the **winver** command:</span></span> 

![Verze systému Windows](./media/active-directory-conditional-access-device-remediation/03.png )


<span data-ttu-id="5a3d6-148">**Windows 10 Anniversary Update (verze 1607):**</span><span class="sxs-lookup"><span data-stu-id="5a3d6-148">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="5a3d6-149">Otevřete aplikaci **Nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-149">Open the **Settings** app.</span></span>
2. <span data-ttu-id="5a3d6-150">Klikněte na **Účty**  >  **Přístup do práce nebo do školy**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-150">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="5a3d6-151">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-151">Click **Connect**.</span></span>
4. <span data-ttu-id="5a3d6-152">Klikněte na **Připojit toto zařízení k Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-152">Click **Join this device to Azure AD**.</span></span>
5. <span data-ttu-id="5a3d6-153">Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a pak postupujte podle zobrazených pokynů.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-153">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
6. <span data-ttu-id="5a3d6-154">Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-154">Sign out, and then sign in with your work account.</span></span>
7. <span data-ttu-id="5a3d6-155">Znovu se pokuste o přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-155">Try again to access the application.</span></span>

<span data-ttu-id="5a3d6-156">**Windows 10 November 2015 Update (verze 1511):**</span><span class="sxs-lookup"><span data-stu-id="5a3d6-156">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="5a3d6-157">Otevřete aplikaci **Nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-157">Open the **Settings** app.</span></span>
2. <span data-ttu-id="5a3d6-158">Klikněte na **Systém**  >  **O systému**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-158">Click **System** > **About**.</span></span>
3. <span data-ttu-id="5a3d6-159">Klikněte na **Připojit se k Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-159">Click **Join Azure AD**.</span></span>
4. <span data-ttu-id="5a3d6-160">Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a pak postupujte podle zobrazených pokynů.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-160">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="5a3d6-161">Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu (účtu Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5a3d6-161">Sign out, and then sign in with your work account (your Azure AD account).</span></span>
6. <span data-ttu-id="5a3d6-162">Znovu se pokuste o přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-162">Try again to access the application.</span></span>


#### <a name="workplace-join-on-windows-81"></a><span data-ttu-id="5a3d6-163">Připojení k pracovišti ve Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="5a3d6-163">Workplace Join on Windows 8.1</span></span>

<span data-ttu-id="5a3d6-164">Pokud zařízení není připojené k doméně a běží na systému Windows 8.1, můžete pomocí následujících kroků provést Workplace Join (připojení k pracovišti) a zaregistrovat se do Microsoft Intune:</span><span class="sxs-lookup"><span data-stu-id="5a3d6-164">If your device is not domain-joined and runs Windows 8.1, to do a Workplace Join and enroll in Microsoft Intune, do the following steps:</span></span>

1. <span data-ttu-id="5a3d6-165">Otevřete **Nastavení počítače**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-165">Open **PC Settings**.</span></span>
2. <span data-ttu-id="5a3d6-166">Klikněte na **Síť**  >  **Pracoviště**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-166">Click **Network** > **Workplace**.</span></span>
3. <span data-ttu-id="5a3d6-167">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-167">Click **Join**.</span></span>
4. <span data-ttu-id="5a3d6-168">Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a pak postupujte podle zobrazených pokynů.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-168">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="5a3d6-169">Klikněte na **Zapnout**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-169">Click **Turn on**.</span></span>
6. <span data-ttu-id="5a3d6-170">Znovu se pokuste o přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-170">Try again to access the application.</span></span>



#### <a name="add-your-work-or-school-account-to-windows"></a><span data-ttu-id="5a3d6-171">Přidejte svůj pracovní nebo školní účet do Windows.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-171">Add your work or school account to Windows</span></span> 


<span data-ttu-id="5a3d6-172">**Windows 10 Anniversary Update (verze 1607):**</span><span class="sxs-lookup"><span data-stu-id="5a3d6-172">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="5a3d6-173">Otevřete aplikaci **Nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-173">Open the **Settings** app.</span></span>
2. <span data-ttu-id="5a3d6-174">Klikněte na **Účty**  >  **Přístup do práce nebo do školy**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-174">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="5a3d6-175">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-175">Click **Connect**.</span></span>
4. <span data-ttu-id="5a3d6-176">Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a pak postupujte podle zobrazených pokynů.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-176">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="5a3d6-177">Znovu se pokuste o přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-177">Try again to access the application.</span></span>


<span data-ttu-id="5a3d6-178">**Windows 10 November 2015 Update (verze 1511):**</span><span class="sxs-lookup"><span data-stu-id="5a3d6-178">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="5a3d6-179">Otevřete aplikaci **Nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-179">Open the **Settings** app.</span></span>
2. <span data-ttu-id="5a3d6-180">Klikněte na **Účty**  >  **Vaše účty**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-180">Click **Accounts** > **Your accounts**.</span></span>
3. <span data-ttu-id="5a3d6-181">Klikněte na **Přidat pracovní nebo školní účet**.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-181">Click **Add work or school account**.</span></span>
4. <span data-ttu-id="5a3d6-182">Ověřte se u své organizace, v případě potřeby proveďte vícefaktorové ověřování a pak postupujte podle zobrazených pokynů.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-182">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="5a3d6-183">Znovu se pokuste o přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5a3d6-183">Try again to access the application.</span></span>





## <a name="next-steps"></a><span data-ttu-id="5a3d6-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a3d6-184">Next steps</span></span>
[<span data-ttu-id="5a3d6-185">Podmíněný přístup ke službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5a3d6-185">Azure Active Directory conditional access</span></span>](active-directory-conditional-access.md)

