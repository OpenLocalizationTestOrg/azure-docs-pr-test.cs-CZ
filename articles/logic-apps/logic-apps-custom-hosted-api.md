---
title: "aaaDeploy, volání a ověřování webových rozhraní API a rozhraní REST API pro Azure Logic Apps | Microsoft Docs"
description: "Nasazení, ověřování a volání webového rozhraní API a rozhraní API REST v pracovních postupech pro integrací systému službou Azure Logic Apps"
keywords: "webové rozhraní API, rozhraní REST API, konektorů, pracovní postupy, integrace v rámci systému, ověření"
services: logic-apps
author: stepsic-microsoft-com
manager: anneta
editor: 
documentationcenter: 
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: ca1e4f28196b21f43b7c9ab94029684121e36f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a><span data-ttu-id="67d96-104">Nasazení, volání a ověření vlastní rozhraní API jako konektory pro logic apps</span><span class="sxs-lookup"><span data-stu-id="67d96-104">Deploy, call, and authenticate custom APIs as connectors for logic apps</span></span>

<span data-ttu-id="67d96-105">Po jste [vytvořte vlastní rozhraní API](./logic-apps-create-api-app.md) , zadejte akce nebo aktivační události toouse v logiku aplikace pracovní postupy, je nutné nasadit vaše rozhraní API, než bude možné volat.</span><span class="sxs-lookup"><span data-stu-id="67d96-105">After you [create custom APIs](./logic-apps-create-api-app.md) that provide actions or triggers toouse in logic apps workflows, you must deploy your APIs before you can call them.</span></span> <span data-ttu-id="67d96-106">A i když můžete volat jakéhokoli rozhraní API z aplikace logiky, pro hello nejlepších, přidejte [Swagger metadata](http://swagger.io/specification/) operace vaše rozhraní API a parametry, který popisuje.</span><span class="sxs-lookup"><span data-stu-id="67d96-106">And although you can call any API from a logic app, for hello best experience, add [Swagger metadata](http://swagger.io/specification/) that describes your API's operations and parameters.</span></span> <span data-ttu-id="67d96-107">Tento soubor Swagger pomáhá rozhraní API fungují lépe a snadněji integrovat s logic apps.</span><span class="sxs-lookup"><span data-stu-id="67d96-107">This Swagger file helps your API work better and integrate more easily with logic apps.</span></span>

<span data-ttu-id="67d96-108">Můžete nasadit vaše rozhraní API jako [webové aplikace](../app-service-web/app-service-web-overview.md), ale zvažte nasazení vašich rozhraní API jako [aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md), který může usnadnit úlohu při sestavení, hostovat a používat rozhraní API v hello cloudu a místně.</span><span class="sxs-lookup"><span data-stu-id="67d96-108">You can deploy your APIs as [web apps](../app-service-web/app-service-web-overview.md), but consider deploying your APIs as [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), which can make your job easier when you build, host, and consume APIs in hello cloud and on premises.</span></span> <span data-ttu-id="67d96-109">Nemáte toochange žádný kód ve vašich rozhraní API – aplikace kód tooan API jen nasadit.</span><span class="sxs-lookup"><span data-stu-id="67d96-109">You don't have toochange any code in your APIs -- just deploy your code tooan API app.</span></span> <span data-ttu-id="67d96-110">Vaše rozhraní API můžete hostovat na [Azure App Service](../app-service/app-service-value-prop-what-is.md), platformy jako služba (PaaS) nabídka, která jedním ze způsobů nejlepší, nejjednodušší a nejvíce škálovatelným hello poskytuje pro hostování rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="67d96-110">You can host your APIs on [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform-as-a-service (PaaS) offering that provides one of hello best, easiest, and most scalable ways for API hosting.</span></span>

<span data-ttu-id="67d96-111">tooauthenticate volání z logiku aplikace tooyour rozhraní API, nastavením Azure Active Directory v hello portálu Azure, nemáte tooupdate vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="67d96-111">tooauthenticate calls from logic apps tooyour APIs, you can set up Azure Active Directory in hello Azure portal so you don't have tooupdate your code.</span></span> <span data-ttu-id="67d96-112">Nebo můžete požadovat a vynutit ověřování prostřednictvím kódu vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="67d96-112">Or, you can require and enforce authentication through your API's code.</span></span>

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a><span data-ttu-id="67d96-113">Nasadit své rozhraní API jako webové aplikace nebo aplikace API</span><span class="sxs-lookup"><span data-stu-id="67d96-113">Deploy your API as a web app or API app</span></span>

<span data-ttu-id="67d96-114">Než bude možné volat vlastního rozhraní API z aplikace logiky, nasaďte své rozhraní API jako webovou aplikaci nebo aplikaci tooAzure rozhraní API služby App Service.</span><span class="sxs-lookup"><span data-stu-id="67d96-114">Before you can call your custom API from a logic app, deploy your API as a web app or API app tooAzure App Service.</span></span> <span data-ttu-id="67d96-115">Navíc toomake váš dokument Swagger přečíst hello návrhář aplikace na základě logiky, nastavte vlastnosti definice hello rozhraní API a zapnout [(CORS) pro sdílení prostředků různého původu](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) pro webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="67d96-115">Also, toomake your Swagger document readable by hello Logic App Designer, set hello API definition properties and turn on [cross-origin resource sharing (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) for your web app or API app.</span></span>

1. <span data-ttu-id="67d96-116">V hello portálu Azure vyberte webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="67d96-116">In hello Azure portal, select your web app or API app.</span></span>

2. <span data-ttu-id="67d96-117">V okně hello, které se otevře v části **rozhraní API**, zvolte **definice rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="67d96-117">In hello blade that opens, under **API**, choose **API definition**.</span></span> <span data-ttu-id="67d96-118">Sada hello **umístění definice rozhraní API** toohello adresa URL pro váš soubor swagger.json.</span><span class="sxs-lookup"><span data-stu-id="67d96-118">Set hello **API definition location** toohello URL for your swagger.json file.</span></span>

   <span data-ttu-id="67d96-119">Adresa URL hello obvykle, se zobrazí v tomto formátu:`https://{name}.azurewebsites.net/swagger/docs/v1)`</span><span class="sxs-lookup"><span data-stu-id="67d96-119">Usually, hello URL appears in this format: `https://{name}.azurewebsites.net/swagger/docs/v1)`</span></span>

   ![Soubor tooSwagger odkazu pro vaše vlastní rozhraní API](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. <span data-ttu-id="67d96-121">V části **rozhraní API**, zvolte **CORS**.</span><span class="sxs-lookup"><span data-stu-id="67d96-121">Under **API**, choose **CORS**.</span></span> <span data-ttu-id="67d96-122">Nastavení zásad hello CORS pro **povolené zdroje** příliš**'*'** (povolit všechny).</span><span class="sxs-lookup"><span data-stu-id="67d96-122">Set hello CORS policy for **Allowed origins** too**'*'** (allow all).</span></span>

   <span data-ttu-id="67d96-123">Toto nastavení umožňuje požadavky z návrháře aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="67d96-123">This setting permits requests from Logic App Designer.</span></span>

   ![Povolení požadavků z vlastního rozhraní API tooyour návrhář aplikace logiky](media/logic-apps-custom-hosted-api/custom-api-cors.png)

<span data-ttu-id="67d96-125">Další informace najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="67d96-125">For more information, see these articles:</span></span>

* [<span data-ttu-id="67d96-126">Přidat metadata Swagger pro rozhraní ASP.NET web API</span><span class="sxs-lookup"><span data-stu-id="67d96-126">Add Swagger metadata for ASP.NET web APIs</span></span>](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [<span data-ttu-id="67d96-127">Nasazení technologie ASP.NET web API tooAzure služby App Service</span><span class="sxs-lookup"><span data-stu-id="67d96-127">Deploy ASP.NET web APIs tooAzure App Service</span></span>](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a><span data-ttu-id="67d96-128">Volání vlastního rozhraní API z logiku aplikace pracovních postupů</span><span class="sxs-lookup"><span data-stu-id="67d96-128">Call your custom API from logic app workflows</span></span>

<span data-ttu-id="67d96-129">Po nastavení vlastnosti definice hello rozhraní API a CORS by měl k dispozici pro tooinclude v pracovním postupu aplikace logiky triggery a akce vlastního rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="67d96-129">After you set up hello API definition properties and CORS, your custom API's triggers and actions should become available for you tooinclude in your logic app workflow.</span></span> 

*  <span data-ttu-id="67d96-130">tooview hello weby, které mají Swagger adresy URL, můžete procházet své předplatné weby v hello návrhář aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="67d96-130">tooview hello websites that have Swagger URLs, you can browse your subscription websites in hello Logic Apps Designer.</span></span>

*  <span data-ttu-id="67d96-131">Dostupné akce tooview a vstupy tak, že odkazuje na dokumentem Swagger pomocí hello [HTTP + Swagger akce](../connectors/connectors-native-http-swagger.md).</span><span class="sxs-lookup"><span data-stu-id="67d96-131">tooview available actions and inputs by pointing at a Swagger document, use hello [HTTP + Swagger action](../connectors/connectors-native-http-swagger.md).</span></span>

*  <span data-ttu-id="67d96-132">toocall jakéhokoli rozhraní API, včetně rozhraní API, která nesmí mít ani vystavit dokumentem Swagger můžete kdykoli vytvořit požadavek s hello [akce HTTP](../connectors/connectors-native-http.md).</span><span class="sxs-lookup"><span data-stu-id="67d96-132">toocall any API, including APIs that don't have or expose a Swagger document, you can always create a request with hello [HTTP action](../connectors/connectors-native-http.md).</span></span>

## <a name="authenticate-calls-tooyour-custom-api"></a><span data-ttu-id="67d96-133">Ověření vlastního rozhraní API tooyour volání</span><span class="sxs-lookup"><span data-stu-id="67d96-133">Authenticate calls tooyour custom API</span></span>

<span data-ttu-id="67d96-134">Můžete zabezpečit volání tooyour vlastního rozhraní API těmito způsoby:</span><span class="sxs-lookup"><span data-stu-id="67d96-134">You can secure calls tooyour custom API in these ways:</span></span>

* <span data-ttu-id="67d96-135">[Žádné změny kódu](#no-code): ochrana rozhraní API s [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) prostřednictvím portálu Azure hello, takže nemáte tooupdate kódu nebo znovu nasadit své rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="67d96-135">[No code changes](#no-code): Protect your API with [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) through hello Azure portal, so you don't have tooupdate your code or redeploy your API.</span></span>

  > [!NOTE]
  > <span data-ttu-id="67d96-136">Ve výchozím nastavení hello ověřování Azure AD, které zapnout v hello portál Azure neposkytuje podrobné autorizace.</span><span class="sxs-lookup"><span data-stu-id="67d96-136">By default, hello Azure AD authentication that you turn on in hello Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="67d96-137">Toto ověřování například zamkne vaše rozhraní API toojust konkrétní klienta, není tooa konkrétního uživatele nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="67d96-137">For example, this authentication locks your API toojust a specific tenant, not tooa specific user or app.</span></span> 

* <span data-ttu-id="67d96-138">[Aktualizujte kód vaše rozhraní API](#update-code): ochrana rozhraní API vynucením [ověřování pomocí certifikátu](#certificate), [základní ověřování](#basic), nebo [ověřování Azure AD](#azure-ad-code) prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="67d96-138">[Update your API's code](#update-code): Protect your API by enforcing [certificate authentication](#certificate), [basic authentication](#basic), or [Azure AD authentication](#azure-ad-code) through code.</span></span>

<a name="no-code"></a>

### <a name="authenticate-calls-tooyour-api-without-changing-code"></a><span data-ttu-id="67d96-139">Ověření volání API tooyour beze změny kódu</span><span class="sxs-lookup"><span data-stu-id="67d96-139">Authenticate calls tooyour API without changing code</span></span>

<span data-ttu-id="67d96-140">Tady je hello obecné kroky pro tuto metodu:</span><span class="sxs-lookup"><span data-stu-id="67d96-140">Here's hello general steps for this method:</span></span>

1. <span data-ttu-id="67d96-141">Vytvořte dvě [identity aplikace Azure Active Directory (Azure AD)](../app-service-api/app-service-api-dotnet-service-principal-auth.md): jeden pro svou aplikaci logiky a jeden pro webové aplikace (nebo aplikace API).</span><span class="sxs-lookup"><span data-stu-id="67d96-141">Create two [Azure Active Directory (Azure AD) application identities](../app-service-api/app-service-api-dotnet-service-principal-auth.md): one for your logic app and one for your web app (or API app).</span></span>

2. <span data-ttu-id="67d96-142">tooyour tooauthenticate volání rozhraní API, pomocí přihlašovacích údajů hello (ID klienta a tajný klíč) hello [instanční objekt](../app-service-api/app-service-api-dotnet-service-principal-auth.md) který je spojen s hello identity aplikací Azure AD pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="67d96-142">tooauthenticate calls tooyour API, use hello credentials (client ID and secret) for hello [service principal](../app-service-api/app-service-api-dotnet-service-principal-auth.md) that's associated with hello Azure AD application identity for your logic app.</span></span>

3. <span data-ttu-id="67d96-143">Zahrňte aplikace hello ID svou definici. aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="67d96-143">Include hello application IDs in your logic app definition.</span></span>

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a><span data-ttu-id="67d96-144">Část 1: Vytvoření identity aplikací Azure AD pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="67d96-144">Part 1: Create an Azure AD application identity for your logic app</span></span>

<span data-ttu-id="67d96-145">Aplikace logiky používá tento tooauthenticate Azure AD application identity služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="67d96-145">Your logic app uses this Azure AD application identity tooauthenticate against Azure AD.</span></span> <span data-ttu-id="67d96-146">Můžete mít pouze tooset si tuto identitu jednou pro váš adresář.</span><span class="sxs-lookup"><span data-stu-id="67d96-146">You only have tooset up this identity one time for your directory.</span></span> <span data-ttu-id="67d96-147">Například můžete toouse hello stejnou identitu pro všechny aplikace logiky, i když můžete vytvořit jedinečné identity pro každou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="67d96-147">For example, you can choose toouse hello same identity for all your logic apps, even though you can create unique identities for each logic app.</span></span> <span data-ttu-id="67d96-148">Můžete nastavit tyto identity v hello portál Azure, [portál Azure classic](#app-identity-logic-classic), nebo použijte [prostředí PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="67d96-148">You can set up these identities in hello Azure portal, [Azure classic portal](#app-identity-logic-classic), or use [PowerShell](#powershell).</span></span>

<span data-ttu-id="67d96-149">**Vytvoření identity aplikace hello aplikace logiky v hello portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="67d96-149">**Create hello application identity for your logic app in hello Azure portal**</span></span>

1. <span data-ttu-id="67d96-150">V hello [portál Azure](https://portal.azure.com "https://portal.azure.com"), zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="67d96-150">In hello [Azure portal](https://portal.azure.com "https://portal.azure.com"), choose **Azure Active Directory**.</span></span> 

2. <span data-ttu-id="67d96-151">Potvrďte, že jste v hello stejného adresáře jako webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="67d96-151">Confirm that you're in hello same directory as your web app or API app.</span></span>

   > [!TIP]
   > <span data-ttu-id="67d96-152">tooswitch adresáře, klikněte na váš profil a vyberte jiný adresář.</span><span class="sxs-lookup"><span data-stu-id="67d96-152">tooswitch directories, click your profile and select another directory.</span></span> <span data-ttu-id="67d96-153">Nebo zvolte **přehled** > **přepínač directory**.</span><span class="sxs-lookup"><span data-stu-id="67d96-153">Or, choose **Overview** > **Switch directory**.</span></span>

3. <span data-ttu-id="67d96-154">V adresáři nabídce hello pod **spravovat**, zvolte **registrace aplikace** > **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="67d96-154">On hello directory menu, under **Manage**, choose **App registrations** > **New application registration**.</span></span>

   > [!TIP]
   > <span data-ttu-id="67d96-155">Ve výchozím nastavení seznam registrace aplikace hello zobrazuje všechny registrace aplikace ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="67d96-155">By default, hello app registrations list shows all app registrations in your directory.</span></span> <span data-ttu-id="67d96-156">Vyberte pouze vaší aplikace registrace, další pole hledání toohello tooview **Moje aplikace**.</span><span class="sxs-lookup"><span data-stu-id="67d96-156">tooview only your app registrations, next toohello search box, select **My apps**.</span></span> 

   ![Vytvořit novou registraci aplikace](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. <span data-ttu-id="67d96-158">Zadejte název vaší identity aplikace, ponechejte **typ aplikace** nastavit příliš**webovou aplikaci nebo rozhraní API**, zadejte jedinečný řetězec formátovaný jako doménu pro **přihlašovací adresa URL**a zvolte  **Vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="67d96-158">Give your application identity a name, leave **Application type** set too**Web app / API**, provide a unique string formatted as a domain for **Sign-on URL**, and choose **Create**.</span></span>

   ![Zadejte název a adresu URL pro přihlašování identita aplikace](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   <span data-ttu-id="67d96-160">Hello identita aplikace, kterou jste vytvořili pro svou aplikaci logiky nyní se zobrazí v seznamu registrace aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="67d96-160">hello application identity that you created for your logic app now appears in hello app registrations list.</span></span>

   ![Identita aplikace pro svou aplikaci logiky](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. <span data-ttu-id="67d96-162">V seznamu aplikací registrace hello vyberte novou identitu aplikace.</span><span class="sxs-lookup"><span data-stu-id="67d96-162">In hello app registrations list, select your new application identity.</span></span> <span data-ttu-id="67d96-163">Zkopírujte a uložte hello **ID aplikace** toouse jako hello "ID klienta" aplikace logiky v části 3.</span><span class="sxs-lookup"><span data-stu-id="67d96-163">Copy and save hello **Application ID** toouse as hello "client ID" for your logic app in Part 3.</span></span>

   ![Zkopírujte a uložte ID aplikace pro aplikaci logiky](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. <span data-ttu-id="67d96-165">Pokud nastavení identity aplikace nejsou zobrazeny, zvolte **nastavení** nebo **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="67d96-165">If your application identity settings aren't visible, choose **Settings** or **All settings**.</span></span>

7. <span data-ttu-id="67d96-166">V části **přístup pomocí rozhraní API**, zvolte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="67d96-166">Under **API Access**, choose **Keys**.</span></span> <span data-ttu-id="67d96-167">V části **popis**, zadejte název pro váš klíč.</span><span class="sxs-lookup"><span data-stu-id="67d96-167">Under **Description**, provide a name for your key.</span></span> <span data-ttu-id="67d96-168">V části **Expires**, vyberte dobu trvání pro váš klíč.</span><span class="sxs-lookup"><span data-stu-id="67d96-168">Under **Expires**, select a duration for your key.</span></span>

   <span data-ttu-id="67d96-169">Hello klíč, který vytváříte funguje jako identita aplikace hello "tajný" nebo heslo pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="67d96-169">hello key that you're creating acts as hello application identity's "secret" or password for your logic app.</span></span>

   ![Vytvořte klíč pro identitu aplikace logiky](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. <span data-ttu-id="67d96-171">Na panelu nástrojů hello, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="67d96-171">On hello toolbar, choose **Save**.</span></span> <span data-ttu-id="67d96-172">V části **hodnotu**, klíč se teď zobrazí.</span><span class="sxs-lookup"><span data-stu-id="67d96-172">Under **Value**, your key now appears.</span></span> 
<span data-ttu-id="67d96-173">**Ujistěte se, že toocopy a uložte klíč** pro pozdější použití protože je skrytá hello klíč když necháte klíče okno hello.</span><span class="sxs-lookup"><span data-stu-id="67d96-173">**Make sure toocopy and save your key** for later use because hello key is hidden when you leave hello key blade.</span></span>

   <span data-ttu-id="67d96-174">Při konfiguraci aplikace logiky v rámci 3, zadejte tento klíč jako hello "tajný" nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="67d96-174">When you configure your logic app in Part 3, you specify this key as hello "secret" or password.</span></span>

   ![Zkopírujte a uložte klíč pro pozdější](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

<span data-ttu-id="67d96-176">**Vytvoření identity aplikace hello aplikace logiky v hello portál Azure classic**</span><span class="sxs-lookup"><span data-stu-id="67d96-176">**Create hello application identity for your logic app in hello Azure classic portal**</span></span>

1. <span data-ttu-id="67d96-177">V hello portál Azure classic, vyberte [ **služby Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="67d96-177">In hello Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2. <span data-ttu-id="67d96-178">Vyberte hello stejný adresář, který používáte pro webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="67d96-178">Select hello same directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="67d96-179">Na hello **aplikace** , zvolte **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="67d96-179">On hello **Applications** tab, choose **Add** at hello bottom of hello page.</span></span>

4. <span data-ttu-id="67d96-180">Zadejte název vaší identity aplikace a zvolte **Další** (šipka vpravo).</span><span class="sxs-lookup"><span data-stu-id="67d96-180">Give your application identity a name, and choose **Next** (right arrow).</span></span>

5. <span data-ttu-id="67d96-181">V části **vlastností aplikace**, poskytovat jedinečný řetězec formátovaný jako doménu pro **přihlašovací adresa URL** a **identifikátor ID URI aplikace**a zvolte **Complete** (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="67d96-181">Under **App properties**, provide a unique string formatted as a domain for **Sign-on URL** and **App ID URI**, and choose **Complete** (checkmark).</span></span>

6. <span data-ttu-id="67d96-182">Na hello **konfigurace** kartě, zkopírujte a uložte hello **ID klienta** pro vaše aplikace toouse logiku v části 3.</span><span class="sxs-lookup"><span data-stu-id="67d96-182">On hello **Configure** tab, copy and save hello **Client ID** for your logic app toouse in Part 3.</span></span>

7. <span data-ttu-id="67d96-183">V části **klíče**, otevřete hello **vyberte dobu trvání** seznamu.</span><span class="sxs-lookup"><span data-stu-id="67d96-183">Under **Keys**, open hello **Select duration** list.</span></span> <span data-ttu-id="67d96-184">Vyberte dobu trvání pro váš klíč.</span><span class="sxs-lookup"><span data-stu-id="67d96-184">Select a duration for your key.</span></span>

   <span data-ttu-id="67d96-185">Hello klíč, který vytváříte funguje jako identita aplikace hello "tajný" nebo heslo pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="67d96-185">hello key that you're creating acts as hello application identity's "secret" or password for your logic app.</span></span>

8. <span data-ttu-id="67d96-186">V dolní části hello hello stránky, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="67d96-186">At hello bottom of hello page, choose **Save**.</span></span> <span data-ttu-id="67d96-187">Toowait může mít několik sekund.</span><span class="sxs-lookup"><span data-stu-id="67d96-187">You might have toowait a few seconds.</span></span>

9. <span data-ttu-id="67d96-188">V části **klíče**, ujistěte se, že toocopy a uložte hello klíč, který se teď zobrazí.</span><span class="sxs-lookup"><span data-stu-id="67d96-188">Under **Keys**, make sure toocopy and save hello key that now appears.</span></span> 

   <span data-ttu-id="67d96-189">Při konfiguraci aplikace logiky v rámci 3, zadejte tento klíč jako hello "tajný" nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="67d96-189">When you configure your logic app in Part 3, you specify this key as hello "secret" or password.</span></span>

<span data-ttu-id="67d96-190">Další informace najdete další informace jak příliš [konfigurace vaší služby App Service aplikace toouse Azure Active Directory přihlášení](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="67d96-190">For more information, learn how too [configure your App Service application toouse Azure Active Directory login](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span></span>

<a name="powershell"></a>

<span data-ttu-id="67d96-191">**Vytvoření identity aplikace hello aplikace logiky v prostředí PowerShell**</span><span class="sxs-lookup"><span data-stu-id="67d96-191">**Create hello application identity for your logic app in PowerShell**</span></span>

<span data-ttu-id="67d96-192">Můžete provést tuto úlohu prostřednictvím Správce Azure Resource Manager pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="67d96-192">You can perform this task through Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="67d96-193">V prostředí PowerShell spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="67d96-193">In PowerShell, run these commands:</span></span>

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. <span data-ttu-id="67d96-194">Ujistěte se, zda text hello toocopy **ID klienta** hello (identifikátor GUID pro vašeho tenanta Azure AD), **ID aplikace**a hello heslo, které jste použili.</span><span class="sxs-lookup"><span data-stu-id="67d96-194">Make sure toocopy hello **Tenant ID** (GUID for your Azure AD tenant), hello **Application ID**, and hello password that you used.</span></span>

<span data-ttu-id="67d96-195">Další informace najdete další informace jak příliš [vytvořit objekt služby s prostředky tooaccess prostředí PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="67d96-195">For more information, learn how too [create a service principal with PowerShell tooaccess resources](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a><span data-ttu-id="67d96-196">Část 2: Vytvoření identity aplikací Azure AD pro vaši webovou aplikaci nebo aplikace API</span><span class="sxs-lookup"><span data-stu-id="67d96-196">Part 2: Create an Azure AD application identity for your web app or API app</span></span>

<span data-ttu-id="67d96-197">Pokud webovou aplikaci nebo aplikaci API je už nasazená, můžete zapnout ověřování a vytvoření identity aplikace hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="67d96-197">If your web app or API app is already deployed, you can turn on authentication and create hello application identity in hello Azure portal.</span></span> <span data-ttu-id="67d96-198">Jinak můžete [zapnout ověřování při nasazení pomocí šablony Azure Resource Manager](#authen-deploy).</span><span class="sxs-lookup"><span data-stu-id="67d96-198">Otherwise, you can [turn on authentication when you deploy with an Azure Resource Manager template](#authen-deploy).</span></span> 

<span data-ttu-id="67d96-199">**Vytvoření identity aplikace hello a zapněte ověřování v hello portál Azure pro nasazené aplikace**</span><span class="sxs-lookup"><span data-stu-id="67d96-199">**Create hello application identity and turn on authentication in hello Azure portal for deployed apps**</span></span>

1. <span data-ttu-id="67d96-200">V hello [portál Azure](https://portal.azure.com "https://portal.azure.com"), najděte a vyberte webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="67d96-200">In hello [Azure portal](https://portal.azure.com "https://portal.azure.com"), find and select your web app or API app.</span></span> 

2. <span data-ttu-id="67d96-201">V části **nastavení**, zvolte **ověřování/autorizace**.</span><span class="sxs-lookup"><span data-stu-id="67d96-201">Under **Settings**, choose **Authentication/Authorization**.</span></span> <span data-ttu-id="67d96-202">V části **ověřování služby aplikace**, zapnout ověřování **na**.</span><span class="sxs-lookup"><span data-stu-id="67d96-202">Under **App Service Authentication**, turn authentication **On**.</span></span> <span data-ttu-id="67d96-203">V části **zprostředkovatele ověřování**, zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="67d96-203">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span>

   ![Zapnout ověřování](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. <span data-ttu-id="67d96-205">Teď vytvořte identity aplikací pro webovou aplikaci nebo aplikaci API, jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="67d96-205">Now create an application identity for your web app or API app as shown here.</span></span> <span data-ttu-id="67d96-206">Na hello **nastavení Azure Active Directory** okně nastavit **režim správy** příliš**Express**.</span><span class="sxs-lookup"><span data-stu-id="67d96-206">On hello **Azure Active Directory Settings** blade, set **Management mode** too**Express**.</span></span> <span data-ttu-id="67d96-207">Zvolte **vytvořit novou aplikaci AD**.</span><span class="sxs-lookup"><span data-stu-id="67d96-207">Choose **Create New AD App**.</span></span> <span data-ttu-id="67d96-208">Zadejte název vaší identity aplikace a zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="67d96-208">Give your application identity a name, and choose **OK**.</span></span> 

   ![Vytvoření identity aplikací pro webovou aplikaci nebo aplikace API](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. <span data-ttu-id="67d96-210">Na hello **ověřování / autorizace okno**, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="67d96-210">On hello **Authentication / Authorization blade**, choose **Save**.</span></span>

<span data-ttu-id="67d96-211">Nyní je nutné vyhledat hello ID klienta a ID klienta pro identitu hello aplikací, který je spojen s webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="67d96-211">Now you must find hello client ID and tenant ID for hello application identity that's associated with your web app or API app.</span></span> <span data-ttu-id="67d96-212">Použijte tyto identifikátory v části 3.</span><span class="sxs-lookup"><span data-stu-id="67d96-212">You use these IDs in Part 3.</span></span> <span data-ttu-id="67d96-213">Proto pokračujte postupem pro hello portál Azure nebo [portál Azure classic](#find-id-classic).</span><span class="sxs-lookup"><span data-stu-id="67d96-213">So continue with these steps for hello Azure portal or [Azure classic portal](#find-id-classic).</span></span>

<span data-ttu-id="67d96-214">**Najít ID klienta aplikace identit a ID klienta pro webovou aplikaci nebo aplikaci API hello portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="67d96-214">**Find application identity's client ID and tenant ID for your web app or API app in hello Azure portal**</span></span>

1. <span data-ttu-id="67d96-215">V části **zprostředkovatele ověřování**, zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="67d96-215">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span> 

   ![Zvolte "Azure Active Directory"](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. <span data-ttu-id="67d96-217">Na hello **nastavení Azure Active Directory** okně nastavit **režim správy** příliš**Upřesnit**.</span><span class="sxs-lookup"><span data-stu-id="67d96-217">On hello **Azure Active Directory Settings** blade, set **Management mode** too**Advanced**.</span></span>

3. <span data-ttu-id="67d96-218">Kopírování hello **ID klienta**a uložte tento identifikátor GUID pro použití v části 3.</span><span class="sxs-lookup"><span data-stu-id="67d96-218">Copy hello **Client ID**, and save that GUID for use in Part 3.</span></span>

   > [!TIP] 
   > <span data-ttu-id="67d96-219">Pokud **ID klienta** a **Url vystavitele** nemáte objeví, zkuste aktualizovat hello portál Azure a zopakujte krok 1.</span><span class="sxs-lookup"><span data-stu-id="67d96-219">If **Client ID** and **Issuer Url** don't appear, try refreshing hello Azure portal, and repeat Step 1.</span></span>

4. <span data-ttu-id="67d96-220">V části **Url vystavitele**, zkopírujte a uložte právě hello identifikátor GUID pro část 3.</span><span class="sxs-lookup"><span data-stu-id="67d96-220">Under **Issuer Url**, copy and save just hello GUID for Part 3.</span></span> <span data-ttu-id="67d96-221">Můžete také použít tento identifikátor GUID ve vaší webové aplikace nebo aplikace API šablonu nasazení, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="67d96-221">You can also use this GUID in your web app or API app's deployment template, if necessary.</span></span>

   <span data-ttu-id="67d96-222">Tento identifikátor GUID je GUID konkrétní klienta ("ID klienta") a by se zobrazit v této adresy URL:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="67d96-222">This GUID is your specific tenant's GUID ("tenant ID") and should appear in this URL: `https://sts.windows.net/{GUID}`</span></span>

5. <span data-ttu-id="67d96-223">Bez uložení změn, zavřete hello **nastavení Azure Active Directory** okno.</span><span class="sxs-lookup"><span data-stu-id="67d96-223">Without saving your changes, close hello **Azure Active Directory Settings** blade.</span></span>

<a name="find-id-classic"></a>

<span data-ttu-id="67d96-224">**Najít ID klienta aplikace identit a ID klienta pro webovou aplikaci nebo aplikaci API hello portál Azure classic**</span><span class="sxs-lookup"><span data-stu-id="67d96-224">**Find application identity's client ID and tenant ID for your web app or API app in hello Azure classic portal**</span></span>

1. <span data-ttu-id="67d96-225">V hello portál Azure classic, vyberte [ **služby Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="67d96-225">In hello Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2.  <span data-ttu-id="67d96-226">Vyberte hello adresář, který používáte pro webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="67d96-226">Select hello directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="67d96-227">V hello **vyhledávání** pole, najděte a vyberte hello identity aplikací pro webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="67d96-227">In hello **Search** box, find and select hello application identity for your web app or API app.</span></span>

4. <span data-ttu-id="67d96-228">Na hello **konfigurace** kartě, kopie hello **ID klienta**a uložte tento identifikátor GUID pro použití v části 3.</span><span class="sxs-lookup"><span data-stu-id="67d96-228">On hello **Configure** tab, copy hello **Client ID**, and save that GUID for use in Part 3.</span></span>

5. <span data-ttu-id="67d96-229">Po získání ID klienta hello, dole hello hello **konfigurace** , zvolte **zobrazit koncové body**.</span><span class="sxs-lookup"><span data-stu-id="67d96-229">After you get hello client ID, at hello bottom of hello **Configure** tab, choose **View endpoints**.</span></span>

6. <span data-ttu-id="67d96-230">Zkopírujte adresu URL hello **dokument federačních metadat**a vyhledejte toothat adresy URL.</span><span class="sxs-lookup"><span data-stu-id="67d96-230">Copy hello URL for **Federation Metadata Document**, and browse toothat URL.</span></span>

7. <span data-ttu-id="67d96-231">V dokumentu metadat hello, které se otevře, vyhledejte kořenový hello **EntityDescriptor ID** element, který má **entityID** atributů v této podobě:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="67d96-231">In hello metadata document that opens, find hello root **EntityDescriptor ID** element, which has an **entityID** attribute in this form: `https://sts.windows.net/{GUID}`</span></span> 

      <span data-ttu-id="67d96-232">Hello identifikátor GUID v tento atribut je GUID konkrétní klienta (ID klienta).</span><span class="sxs-lookup"><span data-stu-id="67d96-232">hello GUID in this attribute is your specific tenant's GUID (tenant ID).</span></span>

8. <span data-ttu-id="67d96-233">ID klienta hello zkopírovat a uložit toto ID pro použití v rámci 3 a toouse ve vaší webové aplikace nebo aplikace API šablonu nasazení, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="67d96-233">Copy hello tenant ID and save that ID for use in Part 3, and also toouse in your web app or API app's deployment template, if necessary.</span></span>

<span data-ttu-id="67d96-234">Další informace naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="67d96-234">For more information, see these topics:</span></span>

* [<span data-ttu-id="67d96-235">Ověřování uživatelů pro aplikace API v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="67d96-235">User authentication for API apps in Azure App Service</span></span>](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [<span data-ttu-id="67d96-236">Ověřování a autorizace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="67d96-236">Authentication and authorization in Azure App Service</span></span>](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

<span data-ttu-id="67d96-237">**Zapnout ověřování při nasazení pomocí šablony Azure Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="67d96-237">**Turn on authentication when you deploy with an Azure Resource Manager template**</span></span>

<span data-ttu-id="67d96-238">Stále je nutné vytvořit identity aplikací Azure AD pro vaši webovou aplikaci nebo aplikaci API, která se liší od identity aplikace hello pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="67d96-238">You must still create an Azure AD application identity for your web app or API app that differs from hello app identity for your logic app.</span></span> <span data-ttu-id="67d96-239">Identita aplikace toocreate hello, hello postupujte podle předchozích kroků v části 2 pro hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="67d96-239">toocreate hello application identity, follow hello previous steps in Part 2 for hello Azure portal.</span></span> <span data-ttu-id="67d96-240">Můžete také postupujte podle kroků hello v část 1, ale ujistěte se, že toouse vaší webové aplikace nebo aplikace API skutečné `https://{URL}` pro **přihlašovací adresa URL** a **identifikátor ID URI aplikace**.</span><span class="sxs-lookup"><span data-stu-id="67d96-240">You can also follow hello steps in Part 1, but make sure toouse your web app or API app's actual `https://{URL}` for **Sign-on URL** and **App ID URI**.</span></span> <span data-ttu-id="67d96-241">Z těchto kroků máte toosave obou client hello ID a ID klienta pro použití v šabloně nasazení vaší aplikace a také pro část 3.</span><span class="sxs-lookup"><span data-stu-id="67d96-241">From these steps, you have toosave both hello client ID and tenant ID for use in your app's deployment template and also for Part 3.</span></span>

> [!NOTE]
> <span data-ttu-id="67d96-242">Když vytvoříte hello Azure AD identity aplikací pro webovou aplikaci nebo aplikaci API, musíte použít hello portál Azure nebo portál Azure classic, nikoli prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="67d96-242">When you create hello Azure AD application identity for your web app or API app, you must use hello Azure portal or Azure classic portal, rather than PowerShell.</span></span> <span data-ttu-id="67d96-243">Hello prostředí PowerShell nemá nastavit hello požadované oprávnění uživatelé toosign do webu.</span><span class="sxs-lookup"><span data-stu-id="67d96-243">hello PowerShell commandlet doesn't set up hello required permissions toosign users into a website.</span></span>

<span data-ttu-id="67d96-244">Po získání ID klienta hello a ID klienta, patří tyto identifikátory jako subresource vaší webové aplikace nebo aplikace API v šabloně nasazení:</span><span class="sxs-lookup"><span data-stu-id="67d96-244">After you get hello client ID and tenant ID, include these IDs as a subresource of your web app or API app in your deployment template:</span></span>

   ```
   "resources": [
       {
           "apiVersion": "2015-08-01",
           "name": "web",
           "type": "config",
           "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
           "properties": {
               "siteAuthEnabled": true,
               "siteAuthSettings": {
                 "clientId": "{client-ID}",
                 "issuer": "https://sts.windows.net/{tenant-ID}/",
               }
           }
       }
   ]
   ```

<span data-ttu-id="67d96-245">tooautomatically nasazení prázdnou webovou aplikaci a aplikace logiky společně s ověřování Azure Active Directory, [zobrazení hello úplnou šablonu zde](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), nebo klikněte na tlačítko **nasazení tooAzure** tady:</span><span class="sxs-lookup"><span data-stu-id="67d96-245">tooautomatically deploy a blank web app and a logic app together with Azure Active Directory authentication, [view hello complete template here](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), or click **Deploy tooAzure** here:</span></span>

<span data-ttu-id="67d96-246">[![Nasazení tooAzure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="67d96-246">[![Deploy tooAzure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span></span>

#### <a name="part-3-populate-hello-authorization-section-in-your-logic-app"></a><span data-ttu-id="67d96-247">Část 3: Naplnění hello části autorizace v aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="67d96-247">Part 3: Populate hello Authorization section in your logic app</span></span>

<span data-ttu-id="67d96-248">již existuje v této části autorizace nastavit Hello předchozí šablona, ale vytváříte-li přímo hello aplikace logiky, musí obsahovat části plnou autorizaci hello.</span><span class="sxs-lookup"><span data-stu-id="67d96-248">hello previous template already has this authorization section set up, but if you are directly authoring hello logic app, you must include hello full authorization section.</span></span>

<span data-ttu-id="67d96-249">Otevřete svou definici. aplikaci logiky v zobrazení kódu, přejděte toohello **HTTP** část akce, najít hello **autorizace** části a zahrnovat tento řádek:</span><span class="sxs-lookup"><span data-stu-id="67d96-249">Open your logic app definition in code view, go toohello **HTTP** action section, find hello **Authorization** section, and include this line:</span></span>

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| <span data-ttu-id="67d96-250">Element</span><span class="sxs-lookup"><span data-stu-id="67d96-250">Element</span></span> | <span data-ttu-id="67d96-251">Popis</span><span class="sxs-lookup"><span data-stu-id="67d96-251">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="67d96-252">Klienta</span><span class="sxs-lookup"><span data-stu-id="67d96-252">tenant</span></span> |<span data-ttu-id="67d96-253">Hello identifikátor GUID pro klienta hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="67d96-253">hello GUID for hello Azure AD tenant</span></span> |
| <span data-ttu-id="67d96-254">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="67d96-254">audience</span></span> |<span data-ttu-id="67d96-255">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="67d96-255">Required.</span></span> <span data-ttu-id="67d96-256">Hello identifikátor GUID pro hello cílový prostředek, který má tooaccess - hello ID klienta z identity hello aplikací pro webovou aplikaci nebo aplikaci API</span><span class="sxs-lookup"><span data-stu-id="67d96-256">hello GUID for hello target resource that you want tooaccess - hello client ID from hello application identity for your web app or API app</span></span> |
| <span data-ttu-id="67d96-257">clientId</span><span class="sxs-lookup"><span data-stu-id="67d96-257">clientId</span></span> |<span data-ttu-id="67d96-258">Hello identifikátor GUID pro klienta hello žádají o přístup - hello ID klienta z identity aplikace hello aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="67d96-258">hello GUID for hello client requesting access - hello client ID from hello application identity for your logic app</span></span> |
| <span data-ttu-id="67d96-259">tajný klíč</span><span class="sxs-lookup"><span data-stu-id="67d96-259">secret</span></span> |<span data-ttu-id="67d96-260">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="67d96-260">Required.</span></span> <span data-ttu-id="67d96-261">Hello klíč nebo heslo z identity aplikace hello pro hello klienta, který požaduje hello přístupový token</span><span class="sxs-lookup"><span data-stu-id="67d96-261">hello key or password from hello application identity for hello client that's requesting hello access token</span></span> |
| <span data-ttu-id="67d96-262">type</span><span class="sxs-lookup"><span data-stu-id="67d96-262">type</span></span> |<span data-ttu-id="67d96-263">typ ověřování Hello.</span><span class="sxs-lookup"><span data-stu-id="67d96-263">hello authentication type.</span></span> <span data-ttu-id="67d96-264">Pro ověřování ActiveDirectoryOAuth hello hodnota je `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="67d96-264">For ActiveDirectoryOAuth authentication, hello value is `ActiveDirectoryOAuth`.</span></span> |

<span data-ttu-id="67d96-265">Například:</span><span class="sxs-lookup"><span data-stu-id="67d96-265">For example:</span></span>

```
{
   ...
   "actions": {
      "some-action": {
         "conditions": [],
         "inputs": {
            "method": "post",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         },
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a><span data-ttu-id="67d96-266">Zabezpečená volání rozhraní API prostřednictvím kódu</span><span class="sxs-lookup"><span data-stu-id="67d96-266">Secure API calls through code</span></span>

<a name="certificate"></a>

#### <a name="certificate-authentication"></a><span data-ttu-id="67d96-267">Ověřování pomocí certifikátu</span><span class="sxs-lookup"><span data-stu-id="67d96-267">Certificate authentication</span></span>

<span data-ttu-id="67d96-268">hello příchozí požadavky toovalidate z logiku aplikace tooyour webové aplikace nebo aplikace API, můžete použít klientské certifikáty.</span><span class="sxs-lookup"><span data-stu-id="67d96-268">toovalidate hello incoming requests from your logic app tooyour web app or API app, you can use client certificates.</span></span> <span data-ttu-id="67d96-269">Další tooset až kódu, [jak vzájemné ověřování TLS tooconfigure](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span><span class="sxs-lookup"><span data-stu-id="67d96-269">tooset up your code, learn [how tooconfigure TLS mutual authentication](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span></span>

<span data-ttu-id="67d96-270">V hello **autorizace** část, zahrnují tento řádek:</span><span class="sxs-lookup"><span data-stu-id="67d96-270">In hello **Authorization** section, include this line:</span></span> 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| <span data-ttu-id="67d96-271">Element</span><span class="sxs-lookup"><span data-stu-id="67d96-271">Element</span></span> | <span data-ttu-id="67d96-272">Popis</span><span class="sxs-lookup"><span data-stu-id="67d96-272">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="67d96-273">type</span><span class="sxs-lookup"><span data-stu-id="67d96-273">type</span></span> |<span data-ttu-id="67d96-274">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="67d96-274">Required.</span></span> <span data-ttu-id="67d96-275">typ ověřování Hello.</span><span class="sxs-lookup"><span data-stu-id="67d96-275">hello authentication type.</span></span> <span data-ttu-id="67d96-276">Pro klientské certifikáty protokolu SSL, musí být hodnota hello `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="67d96-276">For SSL client certificates, hello value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="67d96-277">heslo</span><span class="sxs-lookup"><span data-stu-id="67d96-277">password</span></span> |<span data-ttu-id="67d96-278">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="67d96-278">Required.</span></span> <span data-ttu-id="67d96-279">Hello heslo pro přístup k certifikátu klienta hello (soubor PFX)</span><span class="sxs-lookup"><span data-stu-id="67d96-279">hello password for accessing hello client certificate (PFX file)</span></span> |
| <span data-ttu-id="67d96-280">Soubor PFX</span><span class="sxs-lookup"><span data-stu-id="67d96-280">pfx</span></span> |<span data-ttu-id="67d96-281">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="67d96-281">Required.</span></span> <span data-ttu-id="67d96-282">Kódování Base64 obsah hello klientského certifikátu (soubor PFX)</span><span class="sxs-lookup"><span data-stu-id="67d96-282">Base64-encoded contents of hello client certificate (PFX file)</span></span> |

<a name="basic"></a>

#### <a name="basic-authentication"></a><span data-ttu-id="67d96-283">Základní ověřování</span><span class="sxs-lookup"><span data-stu-id="67d96-283">Basic authentication</span></span>

<span data-ttu-id="67d96-284">toovalidate příchozí požadavky z logiku aplikace tooyour webové aplikace nebo aplikace API, můžete základní ověřování, jako je například uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="67d96-284">toovalidate incoming requests from your logic app tooyour web app or API app, you can use basic authentication, such as a username and password.</span></span> <span data-ttu-id="67d96-285">Základní ověřování je běžný vzor, a můžete toto ověřování v jakékoli toobuild jazyk používaný webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="67d96-285">Basic authentication is a common pattern, and you can use this authentication in any language used toobuild your web app or API app.</span></span>

<span data-ttu-id="67d96-286">V hello **autorizace** část, zahrnují tento řádek:</span><span class="sxs-lookup"><span data-stu-id="67d96-286">In hello **Authorization** section, include this line:</span></span>

<span data-ttu-id="67d96-287">`{"type": "basic", "username": "username", "password": "password"}`.</span><span class="sxs-lookup"><span data-stu-id="67d96-287">`{"type": "basic", "username": "username", "password": "password"}`.</span></span>

| <span data-ttu-id="67d96-288">Element</span><span class="sxs-lookup"><span data-stu-id="67d96-288">Element</span></span> | <span data-ttu-id="67d96-289">Popis</span><span class="sxs-lookup"><span data-stu-id="67d96-289">Description</span></span> |
| --- | --- |
| <span data-ttu-id="67d96-290">type</span><span class="sxs-lookup"><span data-stu-id="67d96-290">type</span></span> |<span data-ttu-id="67d96-291">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="67d96-291">Required.</span></span> <span data-ttu-id="67d96-292">typ ověřování Hello.</span><span class="sxs-lookup"><span data-stu-id="67d96-292">hello authentication type.</span></span> <span data-ttu-id="67d96-293">Pro základní ověřování, musí být hodnota hello `Basic`.</span><span class="sxs-lookup"><span data-stu-id="67d96-293">For basic authentication, hello value must be `Basic`.</span></span> |
| <span data-ttu-id="67d96-294">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="67d96-294">username</span></span> |<span data-ttu-id="67d96-295">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="67d96-295">Required.</span></span> <span data-ttu-id="67d96-296">Hello uživatelské jméno pro ověřování</span><span class="sxs-lookup"><span data-stu-id="67d96-296">hello username for authentication</span></span> |
| <span data-ttu-id="67d96-297">heslo</span><span class="sxs-lookup"><span data-stu-id="67d96-297">password</span></span> |<span data-ttu-id="67d96-298">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="67d96-298">Required.</span></span> <span data-ttu-id="67d96-299">Hello heslo pro ověřování</span><span class="sxs-lookup"><span data-stu-id="67d96-299">hello password for authentication</span></span> |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a><span data-ttu-id="67d96-300">Ověřování Azure Active Directory prostřednictvím kódu</span><span class="sxs-lookup"><span data-stu-id="67d96-300">Azure Active Directory authentication through code</span></span>

<span data-ttu-id="67d96-301">Ve výchozím nastavení hello ověřování Azure AD, které zapnout v hello portál Azure neposkytuje podrobné autorizace.</span><span class="sxs-lookup"><span data-stu-id="67d96-301">By default, hello Azure AD authentication that you turn on in hello Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="67d96-302">Toto ověřování například zamkne vaše rozhraní API toojust konkrétní klienta, není tooa konkrétního uživatele nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="67d96-302">For example, this authentication locks your API toojust a specific tenant, not tooa specific user or app.</span></span> 

<span data-ttu-id="67d96-303">aplikace logiky tooyour toorestrict rozhraní API přístup prostřednictvím kódu, extrahujte hello hlavičky, která má hello JSON web token (JWT).</span><span class="sxs-lookup"><span data-stu-id="67d96-303">toorestrict API access tooyour logic app through code, extract hello header that has hello JSON web token (JWT).</span></span> <span data-ttu-id="67d96-304">Zkontrolujte identitu volajícího hello a odmítnout požadavky, které se neshodují.</span><span class="sxs-lookup"><span data-stu-id="67d96-304">Check hello caller's identity, and reject requests that don't match.</span></span>

<span data-ttu-id="67d96-305">Tooimplement budete pokračovat, toto ověřování zcela v vlastního kódu a není hello použijte portál Azure, zjistěte, jak příliš [ověření pomocí místní služby Active Directory v aplikaci Azure](../app-service-web/web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="67d96-305">Going further, tooimplement this authentication entirely in your own code, and not use hello Azure portal, learn how too [authenticate with on-premises Active Directory in your Azure app](../app-service-web/web-sites-authentication-authorization.md).</span></span>

<span data-ttu-id="67d96-306">toocreate identity aplikací pro svou aplikaci logiky a použít tuto identitu toocall vaše rozhraní API, postupujte podle předchozích kroků hello.</span><span class="sxs-lookup"><span data-stu-id="67d96-306">toocreate an application identity for your logic app and use that identity toocall your API, you must follow hello previous steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="67d96-307">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67d96-307">Next steps</span></span>

* [<span data-ttu-id="67d96-308">Kontrola výkonu aplikace logiky s diagnostické protokoly a výstrahy</span><span class="sxs-lookup"><span data-stu-id="67d96-308">Check logic app performance with diagnostic logs and alerts</span></span>](logic-apps-monitor-your-logic-apps.md)