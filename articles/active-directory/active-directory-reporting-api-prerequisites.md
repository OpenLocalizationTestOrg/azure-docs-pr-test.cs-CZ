---
title: "aaaPrerequisites tooaccess hello Azure AD reporting rozhraní API. | Dokumentace Microsoftu"
description: "Další informace o vytváření sestav API hello požadavky tooaccess hello Azure AD"
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
ms.openlocfilehash: e9d7ceaedb07d18fbd75b70d68b5cfbebc756c36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a><span data-ttu-id="8e76d-104">Generování sestav rozhraní API požadavky tooaccess hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e76d-104">Prerequisites tooaccess hello Azure AD reporting API</span></span>
<span data-ttu-id="8e76d-105">Hello [rozhraní API pro generování sestav Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) poskytují programový přístup k datům toohello pomocí sady založené na REST API.</span><span class="sxs-lookup"><span data-stu-id="8e76d-105">hello [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="8e76d-106">Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="8e76d-106">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="8e76d-107">Hello reporting používá rozhraní API [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize přístup toohello webových rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8e76d-107">hello reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize access toohello web APIs.</span></span> 

<span data-ttu-id="8e76d-108">tooprepare váš přístup toohello reporting rozhraní API, musíte:</span><span class="sxs-lookup"><span data-stu-id="8e76d-108">tooprepare your access toohello reporting API, you must:</span></span>

1. <span data-ttu-id="8e76d-109">Vytvoření aplikace v klientovi služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e76d-109">Create an application in your Azure AD tenant</span></span> 
2. <span data-ttu-id="8e76d-110">Udělení hello aplikace příslušná oprávnění tooaccess hello data služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e76d-110">Grant hello application appropriate permissions tooaccess hello Azure AD data</span></span>
3. <span data-ttu-id="8e76d-111">Shromážděte nastavení konfigurace z adresáře</span><span class="sxs-lookup"><span data-stu-id="8e76d-111">Gather configuration settings from your directory</span></span>

<span data-ttu-id="8e76d-112">Pro dotazy, problémy nebo připomínky, obraťte se na [AAD Reporting pomoci](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="8e76d-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="create-an-azure-ad-application"></a><span data-ttu-id="8e76d-113">Vytvořit aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e76d-113">Create an Azure AD application</span></span>
<span data-ttu-id="8e76d-114">tooconfigure directory tooaccess hello Azure AD reporting rozhraní API, musíte se přihlásit toohello portál Azure classic pomocí účtu správce předplatného Azure, který je také členem hello globální správce adresáře role v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8e76d-114">tooconfigure your directory tooaccess hello Azure AD reporting API, you must sign in toohello Azure classic portal with an Azure subscription administrator account that is also a member of hello Global Administrator directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8e76d-115">Aplikace běžící v části přihlašovací údaje s oprávněním "admin", jako to může být velmi výkonné, takže je potřeba se tookeep hello ID nebo tajný klíč přihlašovací údaje pro aplikaci zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="8e76d-115">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure tookeep hello application's ID/secret credentials secure.</span></span>
> 
> 

1. <span data-ttu-id="8e76d-116">V hello [portál Azure classic](https://manage.windowsazure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-116">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="8e76d-118">Z hello **služby active directory** seznamu, vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="8e76d-118">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="8e76d-119">V nabídce hello hello nahoře, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-119">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="8e76d-121">Na dolním panelu hello, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-121">On hello bottom bar, click **Add**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/03.png) 
5. <span data-ttu-id="8e76d-123">Na hello **co chcete toodo?** dialogové okno, klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-123">On hello **What do you want toodo?** dialog, click **Add an application my organization is developing**.</span></span> 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/04.png) 
6. <span data-ttu-id="8e76d-125">Na hello **Řekněte nám o své aplikaci** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8e76d-125">On hello **Tell us about your application** dialog, perform hello following steps:</span></span> 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    <span data-ttu-id="8e76d-127">a.</span><span class="sxs-lookup"><span data-stu-id="8e76d-127">a.</span></span> <span data-ttu-id="8e76d-128">V hello **název** textovému poli, zadejte název (např: vytváření sestav aplikace rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="8e76d-128">In hello **Name** textbox, type a name (e.g.: Reporting API Application).</span></span>
   
    <span data-ttu-id="8e76d-129">b.</span><span class="sxs-lookup"><span data-stu-id="8e76d-129">b.</span></span> <span data-ttu-id="8e76d-130">Vyberte **webovou aplikaci nebo webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-130">Select **Web application and/or web API**.</span></span>
   
    <span data-ttu-id="8e76d-131">c.</span><span class="sxs-lookup"><span data-stu-id="8e76d-131">c.</span></span> <span data-ttu-id="8e76d-132">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-132">Click **Next**.</span></span>
7. <span data-ttu-id="8e76d-133">Na hello **vlastností aplikace** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8e76d-133">On hello **App properties** dialog, perform hello following steps:</span></span> 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    <span data-ttu-id="8e76d-135">a.</span><span class="sxs-lookup"><span data-stu-id="8e76d-135">a.</span></span> <span data-ttu-id="8e76d-136">V hello **přihlašovací adresa URL** textovému poli, typ `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="8e76d-136">In hello **Sign-on URL** textbox, type `https://localhost`.</span></span>
   
    <span data-ttu-id="8e76d-137">b.</span><span class="sxs-lookup"><span data-stu-id="8e76d-137">b.</span></span> <span data-ttu-id="8e76d-138">V hello **identifikátor ID URI aplikace** textovému poli, typ ```https://localhost/ReportingApiApp```.</span><span class="sxs-lookup"><span data-stu-id="8e76d-138">In hello **App ID URI** textbox, type ```https://localhost/ReportingApiApp```.</span></span>
   
    <span data-ttu-id="8e76d-139">c.</span><span class="sxs-lookup"><span data-stu-id="8e76d-139">c.</span></span> <span data-ttu-id="8e76d-140">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-140">Click **Complete**.</span></span>

## <a name="grant-your-application-permission-toouse-hello-api"></a><span data-ttu-id="8e76d-141">Udělení oprávnění vaší aplikace toouse hello rozhraní API</span><span class="sxs-lookup"><span data-stu-id="8e76d-141">Grant your application permission toouse hello API</span></span>
1. <span data-ttu-id="8e76d-142">V hello [portál Azure classic](https://manage.windowsazure.com/), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-142">In hello [Azure classic portal](https://manage.windowsazure.com/), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="8e76d-144">Z hello **služby active directory** seznamu, vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="8e76d-144">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="8e76d-145">V nabídce hello hello nahoře, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-145">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png)
4. <span data-ttu-id="8e76d-147">V seznamu aplikace hello vyberte nově vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="8e76d-147">In hello applications list, select your newly created application.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="8e76d-149">V nabídce hello hello nahoře, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-149">In hello menu on hello top, click **Configure**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="8e76d-151">V hello **oprávnění tooother aplikace** části pro hello **Azure Active Directory** prostředků, klikněte na tlačítko hello **oprávnění aplikací** rozevíracího seznamu a potom Vyberte **čtení dat adresáře**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-151">In hello **Permissions tooother applications** section, for hello **Azure Active Directory** resource, click hello **Application Permissions** drop-down list, and then select **Read directory data**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/09.png)
7. <span data-ttu-id="8e76d-153">Na dolním panelu hello, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-153">On hello bottom bar, click **Save**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a><span data-ttu-id="8e76d-155">Shromážděte nastavení konfigurace z adresáře</span><span class="sxs-lookup"><span data-stu-id="8e76d-155">Gather configuration settings from your directory</span></span>
<span data-ttu-id="8e76d-156">V této části se dozvíte, jak tooget hello následujících nastavení z adresáře:</span><span class="sxs-lookup"><span data-stu-id="8e76d-156">This section shows you how tooget hello following settings from your directory:</span></span>

* <span data-ttu-id="8e76d-157">Název domény</span><span class="sxs-lookup"><span data-stu-id="8e76d-157">Domain name</span></span>
* <span data-ttu-id="8e76d-158">ID klienta</span><span class="sxs-lookup"><span data-stu-id="8e76d-158">Client ID</span></span>
* <span data-ttu-id="8e76d-159">Tajný klíč klienta</span><span class="sxs-lookup"><span data-stu-id="8e76d-159">Client secret</span></span>

<span data-ttu-id="8e76d-160">Je nutné tyto hodnoty při konfiguraci volání toohello reporting API.</span><span class="sxs-lookup"><span data-stu-id="8e76d-160">You need these values when configuring calls toohello reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="8e76d-161">Získat název domény</span><span class="sxs-lookup"><span data-stu-id="8e76d-161">Get your domain name</span></span>
1. <span data-ttu-id="8e76d-162">V hello [portál Azure classic](https://manage.windowsazure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-162">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="8e76d-164">Z hello **služby active directory** seznamu, vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="8e76d-164">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="8e76d-165">V nabídce hello hello nahoře, klikněte na tlačítko **domény**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-165">In hello menu on hello top, click **Domains**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/11.png) 
4. <span data-ttu-id="8e76d-167">V hello **název domény** sloupce, zkopírujte název vaší domény.</span><span class="sxs-lookup"><span data-stu-id="8e76d-167">In hello **Domain Name** column, copy your domain name.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-hello-applications-client-id"></a><span data-ttu-id="8e76d-169">Získání ID klienta aplikace hello</span><span class="sxs-lookup"><span data-stu-id="8e76d-169">Get hello application's client ID</span></span>
1. <span data-ttu-id="8e76d-170">V hello [portál Azure classic](https://manage.windowsazure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-170">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="8e76d-172">Z hello **služby active directory** seznamu, vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="8e76d-172">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="8e76d-173">V nabídce hello hello nahoře, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-173">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="8e76d-175">V seznamu aplikace hello vyberte nově vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="8e76d-175">In hello applications list, select your newly created application.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="8e76d-177">V nabídce hello hello nahoře, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-177">In hello menu on hello top, click **Configure**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="8e76d-179">Kopie vašeho **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-179">Copy your **Client ID**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-hello-applications-client-secret"></a><span data-ttu-id="8e76d-181">Získat sdílený tajný klíč klienta aplikace hello</span><span class="sxs-lookup"><span data-stu-id="8e76d-181">Get hello application's client secret</span></span>
<span data-ttu-id="8e76d-182">tooget vaší aplikace klienta tajný, potřebujete toocreate nový klíč a uložte jeho hodnota při ukládání hello nový klíč, protože není možné tooretrieve tato hodnota už později.</span><span class="sxs-lookup"><span data-stu-id="8e76d-182">tooget your application's client secret, you need toocreate a new key and save its value upon saving hello new key because it is not possible tooretrieve this value later anymore.</span></span>

1. <span data-ttu-id="8e76d-183">V hello [portál Azure classic](https://manage.windowsazure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-183">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="8e76d-185">Z hello **služby active directory** seznamu, vyberte adresář.</span><span class="sxs-lookup"><span data-stu-id="8e76d-185">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="8e76d-186">V nabídce hello hello nahoře, klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-186">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="8e76d-188">V seznamu aplikace hello vyberte nově vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="8e76d-188">In hello applications list, select your newly created application.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="8e76d-190">V nabídce hello hello nahoře, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-190">In hello menu on hello top, click **Configure**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="8e76d-192">V hello **klíče** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8e76d-192">In hello **Keys** section, perform hello following steps:</span></span> 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/14.png)
   
    <span data-ttu-id="8e76d-194">a.</span><span class="sxs-lookup"><span data-stu-id="8e76d-194">a.</span></span> <span data-ttu-id="8e76d-195">Ze seznamu dobu trvání hello vyberte dobu trvání.</span><span class="sxs-lookup"><span data-stu-id="8e76d-195">From hello duration list, select a duration</span></span>
   
    <span data-ttu-id="8e76d-196">b.</span><span class="sxs-lookup"><span data-stu-id="8e76d-196">b.</span></span> <span data-ttu-id="8e76d-197">Na dolním panelu hello, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8e76d-197">On hello bottom bar, click **Save**.</span></span>
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/10.png)
   
    <span data-ttu-id="8e76d-199">c.</span><span class="sxs-lookup"><span data-stu-id="8e76d-199">c.</span></span> <span data-ttu-id="8e76d-200">Zkopírujte hodnotu klíče hello.</span><span class="sxs-lookup"><span data-stu-id="8e76d-200">Copy hello key value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e76d-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e76d-201">Next Steps</span></span>
* <span data-ttu-id="8e76d-202">Programová způsobem můžete jako tooaccess hello data z hello Azure AD by reporting rozhraní API?</span><span class="sxs-lookup"><span data-stu-id="8e76d-202">Would you like tooaccess hello data from hello Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="8e76d-203">Podívejte se na [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="8e76d-203">Check out [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="8e76d-204">Pokud chcete toofind Další informace o vytváření sestav Azure Active Directory, přečtěte si téma hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="8e76d-204">If you would like toofind out more about Azure Active Directory reporting, see hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

