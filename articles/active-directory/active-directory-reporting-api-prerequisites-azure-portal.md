---
title: "generování sestav rozhraní API aaaPrerequisites tooaccess hello Azure AD | Microsoft Docs"
description: "Další informace o vytváření sestav API hello požadavky tooaccess hello Azure AD"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ec28a7530f341dda31268a978754b615c727d66f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a><span data-ttu-id="45722-103">Generování sestav rozhraní API požadavky tooaccess hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="45722-103">Prerequisites tooaccess hello Azure AD reporting API</span></span>

<span data-ttu-id="45722-104">Hello [rozhraní API pro generování sestav Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) poskytují programový přístup k datům toohello pomocí sady založené na REST API.</span><span class="sxs-lookup"><span data-stu-id="45722-104">hello [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="45722-105">Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="45722-105">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="45722-106">Hello reporting používá rozhraní API [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize přístup toohello webových rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="45722-106">hello reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize access toohello web APIs.</span></span> 

<span data-ttu-id="45722-107">tooget přístup prostřednictvím rozhraní API hello toohello data pro generování sestav, budete potřebovat toohave hello následující role přiřazené:</span><span class="sxs-lookup"><span data-stu-id="45722-107">tooget access toohello reporting data through hello API, you need toohave one of hello following roles assigned:</span></span>

- <span data-ttu-id="45722-108">Čtečka zabezpečení</span><span class="sxs-lookup"><span data-stu-id="45722-108">Security Reader</span></span>
- <span data-ttu-id="45722-109">Správce zabezpečení</span><span class="sxs-lookup"><span data-stu-id="45722-109">Security Admin</span></span>
- <span data-ttu-id="45722-110">Globální správce.</span><span class="sxs-lookup"><span data-stu-id="45722-110">Global Admin</span></span>


<span data-ttu-id="45722-111">tooprepare váš přístup toohello reporting rozhraní API, musíte:</span><span class="sxs-lookup"><span data-stu-id="45722-111">tooprepare your access toohello reporting API, you must:</span></span>

1. <span data-ttu-id="45722-112">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="45722-112">Register an application</span></span> 
2. <span data-ttu-id="45722-113">Udělení oprávnění</span><span class="sxs-lookup"><span data-stu-id="45722-113">Grant permissions</span></span> 
3. <span data-ttu-id="45722-114">Shromážděte nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="45722-114">Gather configuration settings</span></span> 

<span data-ttu-id="45722-115">Pro dotazy, problémy nebo připomínky, prosím [souboru lístek podpory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span><span class="sxs-lookup"><span data-stu-id="45722-115">For questions, issues or feedback, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span></span>

## <a name="register-an-azure-active-directory-application"></a><span data-ttu-id="45722-116">Zaregistrovat aplikaci Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="45722-116">Register an Azure Active Directory application</span></span>

<span data-ttu-id="45722-117">Je nutné tooregister aplikace i v případě, že se připojujete hello reporting rozhraní API pomocí skriptu.</span><span class="sxs-lookup"><span data-stu-id="45722-117">You need tooregister an app even if you are accessing hello reporting API using a script.</span></span> <span data-ttu-id="45722-118">To vám dává **ID aplikace**, což je vyžadováno pro volání autorizace a umožňuje, aby váš kód tooreceive tokeny.</span><span class="sxs-lookup"><span data-stu-id="45722-118">This gives you an **Application ID**, which is required for an authorization call and it enables your code tooreceive tokens.</span></span>

<span data-ttu-id="45722-119">tooconfigure directory tooaccess hello Azure AD reporting rozhraní API, musíte se přihlásit toohello portálu Azure pomocí účtu správce služby Azure, který je taky členem skupiny hello **globálního správce** role adresáře v klientovi služby Azure AD .</span><span class="sxs-lookup"><span data-stu-id="45722-119">tooconfigure your directory tooaccess hello Azure AD reporting API, you must sign in toohello Azure portal with an Azure administrator account that is also a member of hello **Global Administrator** directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45722-120">Aplikace běžící v části přihlašovací údaje s oprávněním "admin", jako to může být velmi výkonné, takže je potřeba se tookeep hello ID nebo tajný klíč přihlašovací údaje pro aplikaci zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="45722-120">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure tookeep hello application's ID/secret credentials secure.</span></span>
> 


<span data-ttu-id="45722-121">**tooregister aplikaci Azure Active Directory:**</span><span class="sxs-lookup"><span data-stu-id="45722-121">**tooregister an Azure Active Directory application:**</span></span>

1. <span data-ttu-id="45722-122">V hello [portál Azure](https://portal.azure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="45722-122">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="45722-124">Na hello **Azure Active Directory** okně klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="45722-124">On hello **Azure Active Directory** blade, click **App registrations**.</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. <span data-ttu-id="45722-126">Na hello **registrace aplikace** klikněte na okno na hello nástrojů v horní části hello **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="45722-126">On hello **App registrations** blade, in hello toolbar on hello top, click **New application registration**.</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. <span data-ttu-id="45722-128">Na hello **vytvořit** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="45722-128">On hello **Create** blade, perform hello following steps:</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    <span data-ttu-id="45722-130">a.</span><span class="sxs-lookup"><span data-stu-id="45722-130">a.</span></span> <span data-ttu-id="45722-131">V hello **název** textovému poli, typ `Reporting API application`.</span><span class="sxs-lookup"><span data-stu-id="45722-131">In hello **Name** textbox, type `Reporting API application`.</span></span>

    <span data-ttu-id="45722-132">b.</span><span class="sxs-lookup"><span data-stu-id="45722-132">b.</span></span> <span data-ttu-id="45722-133">Jako **typ aplikace**, vyberte **webovou aplikaci nebo API**.</span><span class="sxs-lookup"><span data-stu-id="45722-133">As **Application type**, select **Web app / API**.</span></span>

    <span data-ttu-id="45722-134">c.</span><span class="sxs-lookup"><span data-stu-id="45722-134">c.</span></span> <span data-ttu-id="45722-135">V hello **přihlašovací adresa URL** textovému poli, typ `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="45722-135">In hello **Sign-on URL** textbox, type `https://localhost`.</span></span>

    <span data-ttu-id="45722-136">d.</span><span class="sxs-lookup"><span data-stu-id="45722-136">d.</span></span> <span data-ttu-id="45722-137">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="45722-137">Click **Create**.</span></span> 


## <a name="grant-permissions"></a><span data-ttu-id="45722-138">Udělení oprávnění</span><span class="sxs-lookup"><span data-stu-id="45722-138">Grant permissions</span></span> 

<span data-ttu-id="45722-139">cíl Hello tohoto kroku je toogrant aplikace **čtení dat adresáře** oprávnění toohello **Windows Azure Active Directory** rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="45722-139">hello objective of this step is toogrant your application **Read directory data** permissions toohello **Windows Azure Active Directory** API.</span></span>

![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

<span data-ttu-id="45722-141">**toogrant hello toouse oprávnění vaší aplikace API:**</span><span class="sxs-lookup"><span data-stu-id="45722-141">**toogrant your application permission toouse hello API:**</span></span>

1. <span data-ttu-id="45722-142">Na hello **registrace aplikace** klikněte na okno, v seznamu aplikací hello **aplikace Reporting rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="45722-142">On hello **App registrations** blade, in hello apps list, click **Reporting API application**.</span></span>

2. <span data-ttu-id="45722-143">Na hello **aplikace Reporting rozhraní API** klikněte na okno na hello nástrojů v horní části hello **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="45722-143">On hello **Reporting API application** blade, in hello toolbar on hello top, click **Settings**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. <span data-ttu-id="45722-145">Na hello **nastavení** okně klikněte na tlačítko **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="45722-145">On hello **Settings** blade, click **Required permissions**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. <span data-ttu-id="45722-147">Na hello **požadovaná oprávnění** okno, v hello **rozhraní API** seznamu, klikněte na tlačítko **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="45722-147">On hello **Required permissions** blade, in hello **API** list, click **Windows Azure Active Directory**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. <span data-ttu-id="45722-149">Na hello **povolit přístup** vyberte **čtení dat adresáře**.</span><span class="sxs-lookup"><span data-stu-id="45722-149">On hello **Enable Access** blade, select **Read directory data**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. <span data-ttu-id="45722-151">V panelu nástrojů hello hello nahoře, klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="45722-151">In hello toolbar on hello top, click **Save**.</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a><span data-ttu-id="45722-153">Shromážděte nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="45722-153">Gather configuration settings</span></span> 
<span data-ttu-id="45722-154">V této části se dozvíte, jak tooget hello následujících nastavení z adresáře:</span><span class="sxs-lookup"><span data-stu-id="45722-154">This section shows you how tooget hello following settings from your directory:</span></span>

* <span data-ttu-id="45722-155">Název domény</span><span class="sxs-lookup"><span data-stu-id="45722-155">Domain name</span></span>
* <span data-ttu-id="45722-156">ID klienta</span><span class="sxs-lookup"><span data-stu-id="45722-156">Client ID</span></span>
* <span data-ttu-id="45722-157">Tajný klíč klienta</span><span class="sxs-lookup"><span data-stu-id="45722-157">Client secret</span></span>

<span data-ttu-id="45722-158">Je nutné tyto hodnoty při konfiguraci volání toohello reporting API.</span><span class="sxs-lookup"><span data-stu-id="45722-158">You need these values when configuring calls toohello reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="45722-159">Získat název domény</span><span class="sxs-lookup"><span data-stu-id="45722-159">Get your domain name</span></span>

<span data-ttu-id="45722-160">**tooget název domény:**</span><span class="sxs-lookup"><span data-stu-id="45722-160">**tooget your domain name:**</span></span>

1. <span data-ttu-id="45722-161">V hello [portál Azure](https://portal.azure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="45722-161">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="45722-163">Na hello **Azure Active Directory** okně klikněte na tlačítko **názvy domén**.</span><span class="sxs-lookup"><span data-stu-id="45722-163">On hello **Azure Active Directory** blade, click **Domain names**.</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. <span data-ttu-id="45722-165">Zkopírujte název vaší domény z hello seznam domén.</span><span class="sxs-lookup"><span data-stu-id="45722-165">Copy your domain name from hello list of domains.</span></span>


### <a name="get-your-applications-client-id"></a><span data-ttu-id="45722-166">Získejte ID klienta aplikace</span><span class="sxs-lookup"><span data-stu-id="45722-166">Get your application's client ID</span></span>

<span data-ttu-id="45722-167">**tooget ID klienta aplikace:**</span><span class="sxs-lookup"><span data-stu-id="45722-167">**tooget your application's client ID:**</span></span>

1. <span data-ttu-id="45722-168">V hello [portál Azure](https://portal.azure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="45722-168">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="45722-170">Na hello **registrace aplikace** klikněte na okno, v seznamu aplikací hello **aplikace Reporting rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="45722-170">On hello **App registrations** blade, in hello apps list, click **Reporting API application**.</span></span>

3. <span data-ttu-id="45722-171">Na hello **aplikace Reporting rozhraní API** okno, v hello **ID aplikace**, klikněte na tlačítko **klikněte na tlačítko toocopy**.</span><span class="sxs-lookup"><span data-stu-id="45722-171">On hello **Reporting API application** blade, at hello **Application ID**, click **Click toocopy**.</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a><span data-ttu-id="45722-173">Získat sdílený tajný klíč klienta vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="45722-173">Get your application's client secret</span></span>
<span data-ttu-id="45722-174">tooget vaší aplikace klienta tajný, potřebujete toocreate nový klíč a uložte jeho hodnota při ukládání hello nový klíč, protože není možné tooretrieve tato hodnota už později.</span><span class="sxs-lookup"><span data-stu-id="45722-174">tooget your application's client secret, you need toocreate a new key and save its value upon saving hello new key because it is not possible tooretrieve this value later anymore.</span></span>

<span data-ttu-id="45722-175">**tooget tajný klíč klienta aplikace:**</span><span class="sxs-lookup"><span data-stu-id="45722-175">**tooget your application's client secret:**</span></span>

1. <span data-ttu-id="45722-176">V hello [portál Azure](https://portal.azure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="45722-176">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="45722-178">Na hello **registrace aplikace** klikněte na okno, v seznamu aplikací hello **aplikace Reporting rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="45722-178">On hello **App registrations** blade, in hello apps list, click **Reporting API application**.</span></span>


3. <span data-ttu-id="45722-179">Na hello **aplikace Reporting rozhraní API** klikněte na okno na hello nástrojů v horní části hello **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="45722-179">On hello **Reporting API application** blade, in hello toolbar on hello top, click **Settings**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. <span data-ttu-id="45722-181">Na hello **nastavení** okno, v hello **APIR přístup** klikněte na tlačítko **klíče**.</span><span class="sxs-lookup"><span data-stu-id="45722-181">On hello **Settings** blade, in hello **APIR Access** section, click **Keys**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. <span data-ttu-id="45722-183">Na hello **klíče** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="45722-183">On hello **Keys** blade, perform hello following steps:</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    <span data-ttu-id="45722-185">a.</span><span class="sxs-lookup"><span data-stu-id="45722-185">a.</span></span> <span data-ttu-id="45722-186">V hello **popis** textovému poli, typ `Reporting API`.</span><span class="sxs-lookup"><span data-stu-id="45722-186">In hello **Description** textbox, type `Reporting API`.</span></span>

    <span data-ttu-id="45722-187">b.</span><span class="sxs-lookup"><span data-stu-id="45722-187">b.</span></span> <span data-ttu-id="45722-188">Jako **Expires**, vyberte **ve 2 roky**.</span><span class="sxs-lookup"><span data-stu-id="45722-188">As **Expires**, select **In 2 years**.</span></span>

    <span data-ttu-id="45722-189">c.</span><span class="sxs-lookup"><span data-stu-id="45722-189">c.</span></span> <span data-ttu-id="45722-190">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="45722-190">Click **Save**.</span></span>

    <span data-ttu-id="45722-191">d.</span><span class="sxs-lookup"><span data-stu-id="45722-191">d.</span></span> <span data-ttu-id="45722-192">Zkopírujte hodnotu klíče hello.</span><span class="sxs-lookup"><span data-stu-id="45722-192">Copy hello key value.</span></span>


## <a name="next-steps"></a><span data-ttu-id="45722-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45722-193">Next Steps</span></span>
* <span data-ttu-id="45722-194">Programová způsobem můžete jako tooaccess hello data z hello Azure AD by reporting rozhraní API?</span><span class="sxs-lookup"><span data-stu-id="45722-194">Would you like tooaccess hello data from hello Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="45722-195">Podívejte se na [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="45722-195">Check out [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="45722-196">Pokud chcete toofind Další informace o vytváření sestav Azure Active Directory, přečtěte si téma hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="45722-196">If you would like toofind out more about Azure Active Directory reporting, see hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

