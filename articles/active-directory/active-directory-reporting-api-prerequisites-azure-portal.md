---
title: "Požadavky na přístup k Azure AD reporting rozhraní API | Microsoft Docs"
description: "Další informace o požadavcích pro přístup k Azure AD reporting rozhraní API"
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
ms.openlocfilehash: 5fafd83c337e3c73260d89cdad7409a01ce5855b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a><span data-ttu-id="cb8a1-103">Požadavky na přístup k Azure AD reporting rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cb8a1-103">Prerequisites to access the Azure AD reporting API</span></span>

<span data-ttu-id="cb8a1-104">[Rozhraní API pro generování sestav Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) poskytují programový přístup k datům prostřednictvím sady založené na REST API.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-104">The [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="cb8a1-105">Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-105">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="cb8a1-106">Generování sestav používá rozhraní API [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) autorizovat přístup k webovému rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-106">The reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) to authorize access to the web APIs.</span></span> 

<span data-ttu-id="cb8a1-107">Chcete-li získat přístup k data pro generování sestav prostřednictvím rozhraní API, musíte mít jeden z následujících role přiřazené:</span><span class="sxs-lookup"><span data-stu-id="cb8a1-107">To get access to the reporting data through the API, you need to have one of the following roles assigned:</span></span>

- <span data-ttu-id="cb8a1-108">Čtečka zabezpečení</span><span class="sxs-lookup"><span data-stu-id="cb8a1-108">Security Reader</span></span>
- <span data-ttu-id="cb8a1-109">Správce zabezpečení</span><span class="sxs-lookup"><span data-stu-id="cb8a1-109">Security Admin</span></span>
- <span data-ttu-id="cb8a1-110">Globální správce.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-110">Global Admin</span></span>


<span data-ttu-id="cb8a1-111">Chcete-li připravit váš přístup k rozhraní API pro vytváření sestav, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="cb8a1-111">To prepare your access to the reporting API, you must:</span></span>

1. <span data-ttu-id="cb8a1-112">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="cb8a1-112">Register an application</span></span> 
2. <span data-ttu-id="cb8a1-113">Udělení oprávnění</span><span class="sxs-lookup"><span data-stu-id="cb8a1-113">Grant permissions</span></span> 
3. <span data-ttu-id="cb8a1-114">Shromážděte nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="cb8a1-114">Gather configuration settings</span></span> 

<span data-ttu-id="cb8a1-115">Pro dotazy, problémy nebo připomínky, prosím [souboru lístek podpory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span><span class="sxs-lookup"><span data-stu-id="cb8a1-115">For questions, issues or feedback, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span></span>

## <a name="register-an-azure-active-directory-application"></a><span data-ttu-id="cb8a1-116">Zaregistrovat aplikaci Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cb8a1-116">Register an Azure Active Directory application</span></span>

<span data-ttu-id="cb8a1-117">Je třeba zaregistrovat aplikaci i v případě, že se připojujete ke generování sestav rozhraní API pomocí skriptu.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-117">You need to register an app even if you are accessing the reporting API using a script.</span></span> <span data-ttu-id="cb8a1-118">To vám dává **ID aplikace**, což je vyžadováno pro volání autorizace a umožňuje kódu přijímat tokeny.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-118">This gives you an **Application ID**, which is required for an authorization call and it enables your code to receive tokens.</span></span>

<span data-ttu-id="cb8a1-119">Ke konfiguraci adresáře pro přístup k Azure AD reporting rozhraní API, musíte se přihlásit k portálu Azure pomocí účtu správce služby Azure, který je taky členem skupiny **globálního správce** role adresáře v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-119">To configure your directory to access the Azure AD reporting API, you must sign in to the Azure portal with an Azure administrator account that is also a member of the **Global Administrator** directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cb8a1-120">Aplikace běžící v části přihlašovací údaje s oprávněním "admin", jako to může být velmi výkonné, takže Přesvědčte se, k zabezpečení přihlašovacích údajů ID nebo tajný klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-120">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure to keep the application's ID/secret credentials secure.</span></span>
> 


<span data-ttu-id="cb8a1-121">**Zaregistrovat aplikaci Azure Active Directory:**</span><span class="sxs-lookup"><span data-stu-id="cb8a1-121">**To register an Azure Active Directory application:**</span></span>

1. <span data-ttu-id="cb8a1-122">V [portál Azure](https://portal.azure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-122">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="cb8a1-124">Na **Azure Active Directory** okně klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-124">On the **Azure Active Directory** blade, click **App registrations**.</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. <span data-ttu-id="cb8a1-126">Na **registrace aplikace** , na panelu nástrojů v horní části klikněte na **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-126">On the **App registrations** blade, in the toolbar on the top, click **New application registration**.</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. <span data-ttu-id="cb8a1-128">Na **vytvořit** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cb8a1-128">On the **Create** blade, perform the following steps:</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    <span data-ttu-id="cb8a1-130">a.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-130">a.</span></span> <span data-ttu-id="cb8a1-131">V **název** textovému poli, typ `Reporting API application`.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-131">In the **Name** textbox, type `Reporting API application`.</span></span>

    <span data-ttu-id="cb8a1-132">b.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-132">b.</span></span> <span data-ttu-id="cb8a1-133">Jako **typ aplikace**, vyberte **webovou aplikaci nebo API**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-133">As **Application type**, select **Web app / API**.</span></span>

    <span data-ttu-id="cb8a1-134">c.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-134">c.</span></span> <span data-ttu-id="cb8a1-135">V **přihlašovací adresa URL** textovému poli, typ `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-135">In the **Sign-on URL** textbox, type `https://localhost`.</span></span>

    <span data-ttu-id="cb8a1-136">d.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-136">d.</span></span> <span data-ttu-id="cb8a1-137">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-137">Click **Create**.</span></span> 


## <a name="grant-permissions"></a><span data-ttu-id="cb8a1-138">Udělení oprávnění</span><span class="sxs-lookup"><span data-stu-id="cb8a1-138">Grant permissions</span></span> 

<span data-ttu-id="cb8a1-139">Cílem tohoto kroku je udělit aplikaci **čtení dat adresáře** oprávnění **Windows Azure Active Directory** rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-139">The objective of this step is to grant your application **Read directory data** permissions to the **Windows Azure Active Directory** API.</span></span>

![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

<span data-ttu-id="cb8a1-141">**Udělení oprávnění vaše aplikace používat rozhraní API:**</span><span class="sxs-lookup"><span data-stu-id="cb8a1-141">**To grant your application permission to use the API:**</span></span>

1. <span data-ttu-id="cb8a1-142">Na **registrace aplikace** okno, v seznamu aplikací klepněte na tlačítko **aplikace Reporting rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-142">On the **App registrations** blade, in the apps list, click **Reporting API application**.</span></span>

2. <span data-ttu-id="cb8a1-143">Na **aplikace Reporting rozhraní API** , na panelu nástrojů v horní části klikněte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-143">On the **Reporting API application** blade, in the toolbar on the top, click **Settings**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. <span data-ttu-id="cb8a1-145">Na **nastavení** okně klikněte na tlačítko **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-145">On the **Settings** blade, click **Required permissions**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. <span data-ttu-id="cb8a1-147">Na **požadovaná oprávnění** okno v **rozhraní API** seznamu, klikněte na tlačítko **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-147">On the **Required permissions** blade, in the **API** list, click **Windows Azure Active Directory**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. <span data-ttu-id="cb8a1-149">Na **povolit přístup** vyberte **čtení dat adresáře**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-149">On the **Enable Access** blade, select **Read directory data**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. <span data-ttu-id="cb8a1-151">Na panelu nástrojů v horní části klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-151">In the toolbar on the top, click **Save**.</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a><span data-ttu-id="cb8a1-153">Shromážděte nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="cb8a1-153">Gather configuration settings</span></span> 
<span data-ttu-id="cb8a1-154">V této části se dozvíte, jak získat z adresáře následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="cb8a1-154">This section shows you how to get the following settings from your directory:</span></span>

* <span data-ttu-id="cb8a1-155">Název domény</span><span class="sxs-lookup"><span data-stu-id="cb8a1-155">Domain name</span></span>
* <span data-ttu-id="cb8a1-156">ID klienta</span><span class="sxs-lookup"><span data-stu-id="cb8a1-156">Client ID</span></span>
* <span data-ttu-id="cb8a1-157">Tajný klíč klienta</span><span class="sxs-lookup"><span data-stu-id="cb8a1-157">Client secret</span></span>

<span data-ttu-id="cb8a1-158">Je nutné tyto hodnoty při konfiguraci volání do rozhraní API pro generování sestav.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-158">You need these values when configuring calls to the reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="cb8a1-159">Získat název domény</span><span class="sxs-lookup"><span data-stu-id="cb8a1-159">Get your domain name</span></span>

<span data-ttu-id="cb8a1-160">**Pokud chcete získat název domény:**</span><span class="sxs-lookup"><span data-stu-id="cb8a1-160">**To get your domain name:**</span></span>

1. <span data-ttu-id="cb8a1-161">V [portál Azure](https://portal.azure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-161">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="cb8a1-163">Na **Azure Active Directory** okno, klikněte na **názvy domén**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-163">On the **Azure Active Directory** blade, click **Domain names**.</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. <span data-ttu-id="cb8a1-165">Zkopírujte názvu domény ze seznamu domén.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-165">Copy your domain name from the list of domains.</span></span>


### <a name="get-your-applications-client-id"></a><span data-ttu-id="cb8a1-166">Získejte ID klienta aplikace</span><span class="sxs-lookup"><span data-stu-id="cb8a1-166">Get your application's client ID</span></span>

<span data-ttu-id="cb8a1-167">**Pokud chcete získat ID klienta aplikace:**</span><span class="sxs-lookup"><span data-stu-id="cb8a1-167">**To get your application's client ID:**</span></span>

1. <span data-ttu-id="cb8a1-168">V [portál Azure](https://portal.azure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-168">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="cb8a1-170">Na **registrace aplikace** okno, v seznamu aplikací klepněte na tlačítko **aplikace Reporting rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-170">On the **App registrations** blade, in the apps list, click **Reporting API application**.</span></span>

3. <span data-ttu-id="cb8a1-171">Na **aplikace Reporting rozhraní API** okně na **ID aplikace**, klikněte na tlačítko **kliknutím zkopírujte**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-171">On the **Reporting API application** blade, at the **Application ID**, click **Click to copy**.</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a><span data-ttu-id="cb8a1-173">Získat sdílený tajný klíč klienta vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="cb8a1-173">Get your application's client secret</span></span>
<span data-ttu-id="cb8a1-174">Získat sdílený tajný klíč klienta aplikace, musíte vytvořit nový klíč a uložit jeho hodnota při ukládání nového klíče, protože není možné později už načíst tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-174">To get your application's client secret, you need to create a new key and save its value upon saving the new key because it is not possible to retrieve this value later anymore.</span></span>

<span data-ttu-id="cb8a1-175">**Pokud chcete získat sdílený tajný klíč klienta aplikace:**</span><span class="sxs-lookup"><span data-stu-id="cb8a1-175">**To get your application's client secret:**</span></span>

1. <span data-ttu-id="cb8a1-176">V [portál Azure](https://portal.azure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-176">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="cb8a1-178">Na **registrace aplikace** okno, v seznamu aplikací klepněte na tlačítko **aplikace Reporting rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-178">On the **App registrations** blade, in the apps list, click **Reporting API application**.</span></span>


3. <span data-ttu-id="cb8a1-179">Na **aplikace Reporting rozhraní API** , na panelu nástrojů v horní části klikněte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-179">On the **Reporting API application** blade, in the toolbar on the top, click **Settings**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. <span data-ttu-id="cb8a1-181">Na **nastavení** okno v **APIR přístup** klikněte na tlačítko **klíče**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-181">On the **Settings** blade, in the **APIR Access** section, click **Keys**.</span></span> 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. <span data-ttu-id="cb8a1-183">Na **klíče** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cb8a1-183">On the **Keys** blade, perform the following steps:</span></span>

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    <span data-ttu-id="cb8a1-185">a.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-185">a.</span></span> <span data-ttu-id="cb8a1-186">V **popis** textovému poli, typ `Reporting API`.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-186">In the **Description** textbox, type `Reporting API`.</span></span>

    <span data-ttu-id="cb8a1-187">b.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-187">b.</span></span> <span data-ttu-id="cb8a1-188">Jako **Expires**, vyberte **ve 2 roky**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-188">As **Expires**, select **In 2 years**.</span></span>

    <span data-ttu-id="cb8a1-189">c.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-189">c.</span></span> <span data-ttu-id="cb8a1-190">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-190">Click **Save**.</span></span>

    <span data-ttu-id="cb8a1-191">d.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-191">d.</span></span> <span data-ttu-id="cb8a1-192">Zkopírujte hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="cb8a1-192">Copy the key value.</span></span>


## <a name="next-steps"></a><span data-ttu-id="cb8a1-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb8a1-193">Next Steps</span></span>
* <span data-ttu-id="cb8a1-194">Chcete pro přístup k datům z Azure AD reporting rozhraní API programové způsobem?</span><span class="sxs-lookup"><span data-stu-id="cb8a1-194">Would you like to access the data from the Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="cb8a1-195">Podívejte se na [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="cb8a1-195">Check out [Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="cb8a1-196">Pokud chcete získat další informace o vytváření sestav Azure Active Directory, přečtěte si téma [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="cb8a1-196">If you would like to find out more about Azure Active Directory reporting, see the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

