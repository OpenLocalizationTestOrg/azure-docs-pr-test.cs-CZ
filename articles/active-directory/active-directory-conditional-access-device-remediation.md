---
title: "aaaYou nelze získat existuje zde na hello portálu Azure ze zařízení se systémem Windows | Microsoft Docs"
description: "Další informace, které nelze get existuje zde pochází z a co můžete zkontrolovat tooavoid běžící do toto dialogové okno."
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
ms.openlocfilehash: eda2aa10fbff5b3e515723219f61c1cb6f634e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a><span data-ttu-id="dcc2e-104">Odsud se tam nelze dostat pro zařízení s Windows</span><span class="sxs-lookup"><span data-stu-id="dcc2e-104">You can't get there from here on a Windows device</span></span>

<span data-ttu-id="dcc2e-105">Při pokusu například o přístup k intranetu Sharepointu Online ve vaší organizaci můžete narazit na stránku, která uvádí, že *odsud se tam nelze dostat*.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-105">During an attempt to, for example, access your organization's SharePoint Online intranet you might run into a page that states that *you can't get there from here*.</span></span> <span data-ttu-id="dcc2e-106">Tato stránka se zobrazuje, protože správce nakonfiguroval zásady podmíněného přístupu, která zabraňuje prostředkům organizace tooyour přístup za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-106">You are seeing this page, because your administrator has configured a conditional access policy that prevents access tooyour organization's resources under certain conditions.</span></span> <span data-ttu-id="dcc2e-107">Může být nutné toocontact helpdesk nebo váš správce tooget tento problém vyřešit, existuje několik věcí, které můžete vyzkoušet sami, nejprve.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-107">While it might be necessary toocontact helpdesk or your administrator tooget this problem solved, there are a few things you can try out yourself, first.</span></span>

<span data-ttu-id="dcc2e-108">Pokud používáte **Windows** zařízení, měli byste zkontrolovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="dcc2e-108">If you are using a **Windows** device, you should check hello following:</span></span>

- <span data-ttu-id="dcc2e-109">Používáte podporovaný prohlížeč?</span><span class="sxs-lookup"><span data-stu-id="dcc2e-109">Are you using a supported browser?</span></span>

- <span data-ttu-id="dcc2e-110">Používáte ve vašem zařízení podporovanou verzi systému Windows?</span><span class="sxs-lookup"><span data-stu-id="dcc2e-110">Are you running a supported version of Windows on your device?</span></span>

- <span data-ttu-id="dcc2e-111">Odpovídá vaše zařízení požadavkům?</span><span class="sxs-lookup"><span data-stu-id="dcc2e-111">Is your device compliant?</span></span>






## <a name="supported-browser"></a><span data-ttu-id="dcc2e-112">Podporovaný prohlížeč</span><span class="sxs-lookup"><span data-stu-id="dcc2e-112">Supported browser</span></span>

<span data-ttu-id="dcc2e-113">Pokud váš správce nakonfiguroval zásady podmíněného přístupu, můžete pro přístup k prostředkům vaší organizace používat jenom podporovaný prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-113">If your administrator has configured a conditional access policy, you can only access your organization's resources by using a supported browser.</span></span> <span data-ttu-id="dcc2e-114">V zařízeních se systémem Windows se podporují jenom prohlížeče **Internet Explorer** a **Edge**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-114">On a Windows device, only **Internet Explorer** and **Edge** are supported.</span></span>

<span data-ttu-id="dcc2e-115">Můžete snadno identifikovat, jestli nelze přistupovat k prostředku z důvodu tooan nepodporovaný prohlížeč tak, že vyhledá v oddílu Podrobnosti hello chybovou stránku hello:</span><span class="sxs-lookup"><span data-stu-id="dcc2e-115">You can easily identify whether you can't access a resource due tooan unsupported browser by looking at hello details section of hello error page:</span></span>

<span data-ttu-id="dcc2e-116">![Zpráva „Odsud se tam nelze dostat“ pro nepodporované prohlížeče](./media/active-directory-conditional-access-device-remediation/02.png "Scénář")</span><span class="sxs-lookup"><span data-stu-id="dcc2e-116">!["You can't get there from here" message for unsupported browsers](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")</span></span>

<span data-ttu-id="dcc2e-117">pouze nápravy Hello je toouse prohlížeč, podporující hello aplikace pro platformu zařízení.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-117">hello only remediation is toouse a browser that hello application supports for your device platform.</span></span> <span data-ttu-id="dcc2e-118">Úplný seznam podporovaných prohlížečů najdete v části [Podporované prohlížeče](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).</span><span class="sxs-lookup"><span data-stu-id="dcc2e-118">For a complete list of supported browsers, see [supported browsers](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).</span></span>  


## <a name="supported-versions-of-windows"></a><span data-ttu-id="dcc2e-119">Podporované verze Windows</span><span class="sxs-lookup"><span data-stu-id="dcc2e-119">Supported versions of Windows</span></span>

<span data-ttu-id="dcc2e-120">musí být true o operační systém Windows hello na vašem zařízení Hello následující:</span><span class="sxs-lookup"><span data-stu-id="dcc2e-120">hello following must be true about hello Windows operating system on your device:</span></span> 

- <span data-ttu-id="dcc2e-121">Pokud používáte operační systém pro stolní Windows na vašem zařízení, musí toobe Windows 7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-121">If you are running a Windows desktop operating system on your device, it needs toobe Windows 7 or later.</span></span>
- <span data-ttu-id="dcc2e-122">Pokud používáte operační systém Windows server na vašem zařízení, je nutné toobe Windows Server 2008 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-122">If you are running a Windows server operating system on your device, it needs toobe Windows Server 2008 R2 or later.</span></span> 


## <a name="compliant-device"></a><span data-ttu-id="dcc2e-123">Odpovídající zařízení</span><span class="sxs-lookup"><span data-stu-id="dcc2e-123">Compliant device</span></span>

<span data-ttu-id="dcc2e-124">Správce může nakonfigurovat zásady podmíněného přístupu, která umožňuje prostředkům organizace tooyour přístup jenom z kompatibilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-124">Your administrator might have configured a conditional access policy that allows access tooyour organization's resources only from compliant devices.</span></span> <span data-ttu-id="dcc2e-125">toobe předpisy, že zařízení musí být buď připojený k tooyour místní služby Active Directory nebo připojený k tooyour Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-125">toobe compliant, your device must be either joined tooyour on-premises Active Directory or joined tooyour Azure Active Directory.</span></span>

<span data-ttu-id="dcc2e-126">Můžete snadno identifikovat, jestli nelze přistupovat k prostředku z důvodu tooa zařízení, který není kompatibilní s prohlížením hello v části Podrobnosti o chybě stránky hello:</span><span class="sxs-lookup"><span data-stu-id="dcc2e-126">You can easily identify whether you can't access a resource due tooa device that is not compliant by looking at hello details section of hello error page:</span></span>
 
<span data-ttu-id="dcc2e-127">![Zprávy „Odsud se tam nelze dostat“ pro neregistrovaná zařízení](./media/active-directory-conditional-access-device-remediation/01.png "Scénář")</span><span class="sxs-lookup"><span data-stu-id="dcc2e-127">!["You can't get there from here" messages for unregistered devices](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")</span></span>


### <a name="is-your-device-joined-tooan-on-premises-active-directory"></a><span data-ttu-id="dcc2e-128">Je vaše zařízení připojených k tooan místní služby Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dcc2e-128">Is your device joined tooan on-premises Active Directory?</span></span>

<span data-ttu-id="dcc2e-129">**Pokud se vaše zařízení připojí tooan místní služby Active Directory ve vaší organizaci:**</span><span class="sxs-lookup"><span data-stu-id="dcc2e-129">**If your device is joined tooan on-premises Active Directory in your organization:**</span></span>

1. <span data-ttu-id="dcc2e-130">Ujistěte se, přihlásit tooWindows pomocí svého pracovního účtu (účtu služby Active Directory).</span><span class="sxs-lookup"><span data-stu-id="dcc2e-130">Make sure that you sign in tooWindows by using your work account (your Active Directory account).</span></span>
2. <span data-ttu-id="dcc2e-131">Připojte tooyour podnikové síti přes virtuální privátní sítě (VPN) nebo technologie DirectAccess.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-131">Connect tooyour corporate network via a virtual private network (VPN) or DirectAccess.</span></span>
3. <span data-ttu-id="dcc2e-132">Až se připojíte, stiskněte klávesu s logem Windows hello + klíče toolock hello L relace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-132">After you are connected, press hello Windows logo key + hello L key toolock your Windows session.</span></span>
4. <span data-ttu-id="dcc2e-133">Odemkněte relaci Windows zadáním přihlašovacích údajů ke svému pracovnímu účtu.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-133">Unlock your Windows session by entering your work account credentials.</span></span>
5. <span data-ttu-id="dcc2e-134">Počkejte několik minut a akci opakujte tooaccess hello aplikaci nebo službě.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-134">Wait for a minute, and then try again tooaccess hello application or service.</span></span>
6. <span data-ttu-id="dcc2e-135">Pokud se zobrazí hello stejné klikněte na tlačítko hello **podrobnosti** propojit a obraťte se na správce s podrobnostmi hello.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-135">If you see hello same page, click hello **More details** link, and then contact your administrator with hello details.</span></span>


### <a name="is-your-device-not-joined-tooan-on-premises-active-directory"></a><span data-ttu-id="dcc2e-136">Je vaše zařízení není připojené k tooan místní služby Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dcc2e-136">Is your device not joined tooan on-premises Active Directory?</span></span>

<span data-ttu-id="dcc2e-137">Pokud zařízení není připojený tooan místní služby Active Directory a systémem Windows 10, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="dcc2e-137">If your device is not joined tooan on-premises Active Directory and runs Windows 10, you have two options:</span></span>

* <span data-ttu-id="dcc2e-138">Spusťte službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="dcc2e-138">Run Azure AD Join</span></span>
* <span data-ttu-id="dcc2e-139">Přidat váš pracovní nebo školní účet tooWindows</span><span class="sxs-lookup"><span data-stu-id="dcc2e-139">Add your work or school account tooWindows</span></span>

<span data-ttu-id="dcc2e-140">Informace o rozdílech mezi těmito dvěma možnostmi najdete v článku [Používání zařízení s Windows 10 na pracovišti](active-directory-azureadjoin-windows10-devices.md).</span><span class="sxs-lookup"><span data-stu-id="dcc2e-140">For information about how these options are different, see [Using Windows 10 devices in your workplace](active-directory-azureadjoin-windows10-devices.md).</span></span>  
<span data-ttu-id="dcc2e-141">Pokud zařízení:</span><span class="sxs-lookup"><span data-stu-id="dcc2e-141">If your device:</span></span>

- <span data-ttu-id="dcc2e-142">Patří tooyour organizace, byste měli spustit připojení ke službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-142">Belongs tooyour organization, you should run Azure AD Join.</span></span>
- <span data-ttu-id="dcc2e-143">Osobní zařízení nebo Windows phone, přidávejte váš pracovní nebo školní účet tooWindows</span><span class="sxs-lookup"><span data-stu-id="dcc2e-143">Is a personal device or a Windows phone, you should add your work or school account tooWindows</span></span> 



#### <a name="azure-ad-join-on-windows-10"></a><span data-ttu-id="dcc2e-144">Azure AD Join v systému Windows 10</span><span class="sxs-lookup"><span data-stu-id="dcc2e-144">Azure AD Join on Windows 10</span></span>

<span data-ttu-id="dcc2e-145">Hello toojoin kroky jsou vaše zařízení tooAzure AD svázané hello verzi Windows 10 běží na něm.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-145">hello steps toojoin your device tooAzure AD are tied hello version of Windows 10 you are running on it.</span></span> <span data-ttu-id="dcc2e-146">toodetermine hello verzi operačního systému Windows 10, spusťte hello **winver** příkaz:</span><span class="sxs-lookup"><span data-stu-id="dcc2e-146">toodetermine hello version of your Windows 10 operating system, run hello **winver** command:</span></span> 

![Verze systému Windows](./media/active-directory-conditional-access-device-remediation/03.png )


<span data-ttu-id="dcc2e-148">**Windows 10 Anniversary Update (verze 1607):**</span><span class="sxs-lookup"><span data-stu-id="dcc2e-148">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="dcc2e-149">Otevřete hello **nastavení** aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-149">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="dcc2e-150">Klikněte na **Účty**  >  **Přístup do práce nebo do školy**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-150">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="dcc2e-151">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-151">Click **Connect**.</span></span>
4. <span data-ttu-id="dcc2e-152">Klikněte na tlačítko **připojit toto zařízení tooAzure AD**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-152">Click **Join this device tooAzure AD**.</span></span>
5. <span data-ttu-id="dcc2e-153">Ověřování organizace tooyour, poskytovat služby Multi-Factor authentication, pokud se zobrazí výzva a potom postupujte podle kroků hello, které jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-153">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
6. <span data-ttu-id="dcc2e-154">Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-154">Sign out, and then sign in with your work account.</span></span>
7. <span data-ttu-id="dcc2e-155">Zkuste to znovu tooaccess hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-155">Try again tooaccess hello application.</span></span>

<span data-ttu-id="dcc2e-156">**Windows 10 November 2015 Update (verze 1511):**</span><span class="sxs-lookup"><span data-stu-id="dcc2e-156">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="dcc2e-157">Otevřete hello **nastavení** aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-157">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="dcc2e-158">Klikněte na **Systém**  >  **O systému**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-158">Click **System** > **About**.</span></span>
3. <span data-ttu-id="dcc2e-159">Klikněte na **Připojit se k Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-159">Click **Join Azure AD**.</span></span>
4. <span data-ttu-id="dcc2e-160">Ověřování organizace tooyour, poskytovat služby Multi-Factor authentication, pokud se zobrazí výzva a potom postupujte podle kroků hello, které jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-160">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="dcc2e-161">Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu (účtu Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dcc2e-161">Sign out, and then sign in with your work account (your Azure AD account).</span></span>
6. <span data-ttu-id="dcc2e-162">Zkuste to znovu tooaccess hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-162">Try again tooaccess hello application.</span></span>


#### <a name="workplace-join-on-windows-81"></a><span data-ttu-id="dcc2e-163">Připojení k pracovišti ve Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="dcc2e-163">Workplace Join on Windows 8.1</span></span>

<span data-ttu-id="dcc2e-164">Pokud zařízení není připojené k doméně a běží Windows 8.1, toodo k síti na pracovišti a zaregistrovat v Microsoft Intune, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dcc2e-164">If your device is not domain-joined and runs Windows 8.1, toodo a Workplace Join and enroll in Microsoft Intune, do hello following steps:</span></span>

1. <span data-ttu-id="dcc2e-165">Otevřete **Nastavení počítače**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-165">Open **PC Settings**.</span></span>
2. <span data-ttu-id="dcc2e-166">Klikněte na **Síť**  >  **Pracoviště**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-166">Click **Network** > **Workplace**.</span></span>
3. <span data-ttu-id="dcc2e-167">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-167">Click **Join**.</span></span>
4. <span data-ttu-id="dcc2e-168">Ověřování organizace tooyour, poskytovat služby Multi-Factor authentication, pokud se zobrazí výzva a potom postupujte podle kroků hello, které jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-168">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="dcc2e-169">Klikněte na **Zapnout**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-169">Click **Turn on**.</span></span>
6. <span data-ttu-id="dcc2e-170">Zkuste to znovu tooaccess hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-170">Try again tooaccess hello application.</span></span>



#### <a name="add-your-work-or-school-account-toowindows"></a><span data-ttu-id="dcc2e-171">Přidat váš pracovní nebo školní účet tooWindows</span><span class="sxs-lookup"><span data-stu-id="dcc2e-171">Add your work or school account tooWindows</span></span> 


<span data-ttu-id="dcc2e-172">**Windows 10 Anniversary Update (verze 1607):**</span><span class="sxs-lookup"><span data-stu-id="dcc2e-172">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="dcc2e-173">Otevřete hello **nastavení** aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-173">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="dcc2e-174">Klikněte na **Účty**  >  **Přístup do práce nebo do školy**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-174">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="dcc2e-175">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-175">Click **Connect**.</span></span>
4. <span data-ttu-id="dcc2e-176">Ověřování organizace tooyour, poskytovat služby Multi-Factor authentication, pokud se zobrazí výzva a potom postupujte podle kroků hello, které jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-176">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="dcc2e-177">Zkuste to znovu tooaccess hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-177">Try again tooaccess hello application.</span></span>


<span data-ttu-id="dcc2e-178">**Windows 10 November 2015 Update (verze 1511):**</span><span class="sxs-lookup"><span data-stu-id="dcc2e-178">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="dcc2e-179">Otevřete hello **nastavení** aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-179">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="dcc2e-180">Klikněte na **Účty**  >  **Vaše účty**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-180">Click **Accounts** > **Your accounts**.</span></span>
3. <span data-ttu-id="dcc2e-181">Klikněte na **Přidat pracovní nebo školní účet**.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-181">Click **Add work or school account**.</span></span>
4. <span data-ttu-id="dcc2e-182">Ověřování organizace tooyour, poskytovat služby Multi-Factor authentication, pokud se zobrazí výzva a potom postupujte podle kroků hello, které jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-182">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="dcc2e-183">Zkuste to znovu tooaccess hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc2e-183">Try again tooaccess hello application.</span></span>





## <a name="next-steps"></a><span data-ttu-id="dcc2e-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dcc2e-184">Next steps</span></span>
[<span data-ttu-id="dcc2e-185">Podmíněný přístup ke službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dcc2e-185">Azure Active Directory conditional access</span></span>](active-directory-conditional-access.md)

