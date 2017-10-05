---
title: "Požadavky pro přístup k Azure AD reporting rozhraní API. | Dokumentace Microsoftu"
description: "Další informace o požadavcích pro přístup k Azure AD reporting rozhraní API"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 6e409fc56b77f37dac7f37382e664c577666ad4d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a><span data-ttu-id="e03e1-104">Požadavky na přístup k Azure AD reporting rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e03e1-104">Prerequisites to access the Azure AD reporting API</span></span>
<span data-ttu-id="e03e1-105">[Rozhraní API pro generování sestav Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) poskytují programový přístup k datům prostřednictvím sady založené na REST API.</span><span class="sxs-lookup"><span data-stu-id="e03e1-105">The [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="e03e1-106">Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="e03e1-106">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="e03e1-107">Generování sestav používá rozhraní API [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) autorizovat přístup k webovému rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e03e1-107">The reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) to authorize access to the web APIs.</span></span> 

<span data-ttu-id="e03e1-108">Chcete-li připravit váš přístup k rozhraní API pro vytváření sestav, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e03e1-108">To prepare your access to the reporting API, you must:</span></span>

1. <span data-ttu-id="e03e1-109">Vytvoření aplikace v klientovi služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e03e1-109">Create an application in your Azure AD tenant</span></span> 
2. <span data-ttu-id="e03e1-110">Udělení příslušných oprávnění aplikací pro přístup k datům Azure AD</span><span class="sxs-lookup"><span data-stu-id="e03e1-110">Grant the application appropriate permissions to access the Azure AD data</span></span>
3. <span data-ttu-id="e03e1-111">Shromážděte nastavení konfigurace z adresáře</span><span class="sxs-lookup"><span data-stu-id="e03e1-111">Gather configuration settings from your directory</span></span>

<span data-ttu-id="e03e1-112">Pro dotazy, problémy nebo připomínky, obraťte se na [AAD Reporting pomoci](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="e03e1-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="create-an-azure-ad-application"></a><span data-ttu-id="e03e1-113">Vytvořit aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="e03e1-113">Create an Azure AD application</span></span>
<span data-ttu-id="e03e1-114">Chcete-li nastavení konfigurace adresáře pro přístup k Azure AD reporting rozhraní API, musíte se přihlásit k portálu Azure classic pomocí účtu správce předplatného Azure, který je taky členem skupiny roli globálního správce adresáře v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e03e1-114">To configure your directory to access the Azure AD reporting API, you must sign in to the Azure classic portal with an Azure subscription administrator account that is also a member of the Global Administrator directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e03e1-115">Aplikace běžící v části přihlašovací údaje s oprávněním "admin", jako to může být velmi výkonné, takže Přesvědčte se, k zabezpečení přihlašovacích údajů ID nebo tajný klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="e03e1-115">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure to keep the application's ID/secret credentials secure.</span></span>
> 
> 

1. <span data-ttu-id="e03e1-116">V [portál Azure classic](https://manage.windowsazure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-116">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="e03e1-118">Z **služby active directory** seznamu, vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="e03e1-118">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="e03e1-119">V nabídce v horní části, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-119">In the menu on the top, click **Applications**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="e03e1-121">Na dolním panelu klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-121">On the bottom bar, click **Add**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/03.png) 
5. <span data-ttu-id="e03e1-123">Na **co chcete udělat?** dialogové okno, klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-123">On the **What do you want to do?** dialog, click **Add an application my organization is developing**.</span></span> 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/04.png) 
6. <span data-ttu-id="e03e1-125">Na **Řekněte nám o své aplikaci** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e03e1-125">On the **Tell us about your application** dialog, perform the following steps:</span></span> 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    <span data-ttu-id="e03e1-127">a.</span><span class="sxs-lookup"><span data-stu-id="e03e1-127">a.</span></span> <span data-ttu-id="e03e1-128">V **název** textovému poli, zadejte název (např: vytváření sestav aplikace rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="e03e1-128">In the **Name** textbox, type a name (e.g.: Reporting API Application).</span></span>
   
    <span data-ttu-id="e03e1-129">b.</span><span class="sxs-lookup"><span data-stu-id="e03e1-129">b.</span></span> <span data-ttu-id="e03e1-130">Vyberte **webovou aplikaci nebo webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-130">Select **Web application and/or web API**.</span></span>
   
    <span data-ttu-id="e03e1-131">c.</span><span class="sxs-lookup"><span data-stu-id="e03e1-131">c.</span></span> <span data-ttu-id="e03e1-132">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-132">Click **Next**.</span></span>
7. <span data-ttu-id="e03e1-133">Na **vlastností aplikace** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e03e1-133">On the **App properties** dialog, perform the following steps:</span></span> 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    <span data-ttu-id="e03e1-135">a.</span><span class="sxs-lookup"><span data-stu-id="e03e1-135">a.</span></span> <span data-ttu-id="e03e1-136">V **přihlašovací adresa URL** textovému poli, typ `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="e03e1-136">In the **Sign-on URL** textbox, type `https://localhost`.</span></span>
   
    <span data-ttu-id="e03e1-137">b.</span><span class="sxs-lookup"><span data-stu-id="e03e1-137">b.</span></span> <span data-ttu-id="e03e1-138">V **identifikátor ID URI aplikace** textovému poli, typ ```https://localhost/ReportingApiApp```.</span><span class="sxs-lookup"><span data-stu-id="e03e1-138">In the **App ID URI** textbox, type ```https://localhost/ReportingApiApp```.</span></span>
   
    <span data-ttu-id="e03e1-139">c.</span><span class="sxs-lookup"><span data-stu-id="e03e1-139">c.</span></span> <span data-ttu-id="e03e1-140">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-140">Click **Complete**.</span></span>

## <a name="grant-your-application-permission-to-use-the-api"></a><span data-ttu-id="e03e1-141">Udělit oprávnění vaše aplikace k používání rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e03e1-141">Grant your application permission to use the API</span></span>
1. <span data-ttu-id="e03e1-142">V [portál Azure classic](https://manage.windowsazure.com/), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-142">In the [Azure classic portal](https://manage.windowsazure.com/), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="e03e1-144">Z **služby active directory** seznamu, vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="e03e1-144">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="e03e1-145">V nabídce v horní části, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-145">In the menu on the top, click **Applications**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png)
4. <span data-ttu-id="e03e1-147">V seznamu aplikací vyberte nově vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="e03e1-147">In the applications list, select your newly created application.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="e03e1-149">V nabídce v horní části, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-149">In the menu on the top, click **Configure**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="e03e1-151">V **oprávnění k ostatním aplikacím** části pro **Azure Active Directory** prostředků, klikněte na tlačítko **oprávnění aplikací** rozevíracího seznamu a potom vyberte **Čtení dat adresáře**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-151">In the **Permissions to other applications** section, for the **Azure Active Directory** resource, click the **Application Permissions** drop-down list, and then select **Read directory data**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/09.png)
7. <span data-ttu-id="e03e1-153">Na dolním panelu klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-153">On the bottom bar, click **Save**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a><span data-ttu-id="e03e1-155">Shromážděte nastavení konfigurace z adresáře</span><span class="sxs-lookup"><span data-stu-id="e03e1-155">Gather configuration settings from your directory</span></span>
<span data-ttu-id="e03e1-156">V této části se dozvíte, jak získat z adresáře následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="e03e1-156">This section shows you how to get the following settings from your directory:</span></span>

* <span data-ttu-id="e03e1-157">Název domény</span><span class="sxs-lookup"><span data-stu-id="e03e1-157">Domain name</span></span>
* <span data-ttu-id="e03e1-158">ID klienta</span><span class="sxs-lookup"><span data-stu-id="e03e1-158">Client ID</span></span>
* <span data-ttu-id="e03e1-159">Tajný klíč klienta</span><span class="sxs-lookup"><span data-stu-id="e03e1-159">Client secret</span></span>

<span data-ttu-id="e03e1-160">Je nutné tyto hodnoty při konfiguraci volání do rozhraní API pro generování sestav.</span><span class="sxs-lookup"><span data-stu-id="e03e1-160">You need these values when configuring calls to the reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="e03e1-161">Získat název domény</span><span class="sxs-lookup"><span data-stu-id="e03e1-161">Get your domain name</span></span>
1. <span data-ttu-id="e03e1-162">V [portál Azure classic](https://manage.windowsazure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-162">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="e03e1-164">Z **služby active directory** seznamu, vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="e03e1-164">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="e03e1-165">V nabídce v horní části, klikněte na tlačítko **domény**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-165">In the menu on the top, click **Domains**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/11.png) 
4. <span data-ttu-id="e03e1-167">V **název domény** sloupce, zkopírujte název vaší domény.</span><span class="sxs-lookup"><span data-stu-id="e03e1-167">In the **Domain Name** column, copy your domain name.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-the-applications-client-id"></a><span data-ttu-id="e03e1-169">Získejte ID klienta aplikace.</span><span class="sxs-lookup"><span data-stu-id="e03e1-169">Get the application's client ID</span></span>
1. <span data-ttu-id="e03e1-170">V [portál Azure classic](https://manage.windowsazure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-170">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="e03e1-172">Z **služby active directory** seznamu, vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="e03e1-172">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="e03e1-173">V nabídce v horní části, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-173">In the menu on the top, click **Applications**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="e03e1-175">V seznamu aplikací vyberte nově vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="e03e1-175">In the applications list, select your newly created application.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="e03e1-177">V nabídce v horní části, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-177">In the menu on the top, click **Configure**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="e03e1-179">Kopie vašeho **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-179">Copy your **Client ID**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-the-applications-client-secret"></a><span data-ttu-id="e03e1-181">Získat sdílený tajný klíč klienta aplikace.</span><span class="sxs-lookup"><span data-stu-id="e03e1-181">Get the application's client secret</span></span>
<span data-ttu-id="e03e1-182">Získat sdílený tajný klíč klienta aplikace, musíte vytvořit nový klíč a uložit jeho hodnota při ukládání nového klíče, protože není možné později už načíst tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e03e1-182">To get your application's client secret, you need to create a new key and save its value upon saving the new key because it is not possible to retrieve this value later anymore.</span></span>

1. <span data-ttu-id="e03e1-183">V [portál Azure classic](https://manage.windowsazure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-183">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="e03e1-185">Z **služby active directory** seznamu, vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="e03e1-185">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="e03e1-186">V nabídce v horní části, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-186">In the menu on the top, click **Applications**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="e03e1-188">V seznamu aplikací vyberte nově vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="e03e1-188">In the applications list, select your newly created application.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="e03e1-190">V nabídce v horní části, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-190">In the menu on the top, click **Configure**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="e03e1-192">V **klíče** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e03e1-192">In the **Keys** section, perform the following steps:</span></span> 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/14.png)
   
    <span data-ttu-id="e03e1-194">a.</span><span class="sxs-lookup"><span data-stu-id="e03e1-194">a.</span></span> <span data-ttu-id="e03e1-195">Ze seznamu dobu trvání vyberte dobu trvání.</span><span class="sxs-lookup"><span data-stu-id="e03e1-195">From the duration list, select a duration</span></span>
   
    <span data-ttu-id="e03e1-196">b.</span><span class="sxs-lookup"><span data-stu-id="e03e1-196">b.</span></span> <span data-ttu-id="e03e1-197">Na dolním panelu klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e03e1-197">On the bottom bar, click **Save**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/10.png)
   
    <span data-ttu-id="e03e1-199">c.</span><span class="sxs-lookup"><span data-stu-id="e03e1-199">c.</span></span> <span data-ttu-id="e03e1-200">Zkopírujte hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="e03e1-200">Copy the key value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e03e1-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e03e1-201">Next Steps</span></span>
* <span data-ttu-id="e03e1-202">Chcete pro přístup k datům z Azure AD reporting rozhraní API programové způsobem?</span><span class="sxs-lookup"><span data-stu-id="e03e1-202">Would you like to access the data from the Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="e03e1-203">Podívejte se na [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="e03e1-203">Check out [Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="e03e1-204">Pokud chcete získat další informace o vytváření sestav Azure Active Directory, přečtěte si téma [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="e03e1-204">If you would like to find out more about Azure Active Directory reporting, see the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

