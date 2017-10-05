---
title: "Nasazení, volání a ověřování webových rozhraní API a rozhraní REST API pro Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 88c62d5ab046d8cf4bce5d23b776e517bb0e1d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a><span data-ttu-id="3928a-104">Nasazení, volání a ověření vlastní rozhraní API jako konektory pro logic apps</span><span class="sxs-lookup"><span data-stu-id="3928a-104">Deploy, call, and authenticate custom APIs as connectors for logic apps</span></span>

<span data-ttu-id="3928a-105">Po jste [vytvořte vlastní rozhraní API](./logic-apps-create-api-app.md) , poskytovat akce nebo aktivační události pro použití v pracovních aplikace logiky, je nutné nasadit vaše rozhraní API, než bude možné volat.</span><span class="sxs-lookup"><span data-stu-id="3928a-105">After you [create custom APIs](./logic-apps-create-api-app.md) that provide actions or triggers to use in logic apps workflows, you must deploy your APIs before you can call them.</span></span> <span data-ttu-id="3928a-106">A i když můžete volat jakéhokoli rozhraní API z aplikace logiky, pro dosažení co nejlepších výsledků, přidejte [Swagger metadata](http://swagger.io/specification/) operace vaše rozhraní API a parametry, který popisuje.</span><span class="sxs-lookup"><span data-stu-id="3928a-106">And although you can call any API from a logic app, for the best experience, add [Swagger metadata](http://swagger.io/specification/) that describes your API's operations and parameters.</span></span> <span data-ttu-id="3928a-107">Tento soubor Swagger pomáhá rozhraní API fungují lépe a snadněji integrovat s logic apps.</span><span class="sxs-lookup"><span data-stu-id="3928a-107">This Swagger file helps your API work better and integrate more easily with logic apps.</span></span>

<span data-ttu-id="3928a-108">Můžete nasadit vaše rozhraní API jako [webové aplikace](../app-service-web/app-service-web-overview.md), ale zvažte nasazení vašich rozhraní API jako [aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md), který může usnadnit úlohu při sestavení, hostovat a používat rozhraní API v cloudu a místně.</span><span class="sxs-lookup"><span data-stu-id="3928a-108">You can deploy your APIs as [web apps](../app-service-web/app-service-web-overview.md), but consider deploying your APIs as [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), which can make your job easier when you build, host, and consume APIs in the cloud and on premises.</span></span> <span data-ttu-id="3928a-109">Není nutné změnit spuštěním kódu vaše rozhraní API – kód do aplikace API jen nasadit.</span><span class="sxs-lookup"><span data-stu-id="3928a-109">You don't have to change any code in your APIs -- just deploy your code to an API app.</span></span> <span data-ttu-id="3928a-110">Vaše rozhraní API můžete hostovat na [Azure App Service](../app-service/app-service-value-prop-what-is.md), platformy jako služba (PaaS) nabídka, která jedním ze způsobů nejlepší, nejjednodušší a nejvíce škálovatelným poskytuje pro hostování rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3928a-110">You can host your APIs on [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform-as-a-service (PaaS) offering that provides one of the best, easiest, and most scalable ways for API hosting.</span></span>

<span data-ttu-id="3928a-111">K ověření volání z aplikace logiky pro vaše rozhraní API, nastavením Azure Active Directory na portálu Azure, nemusíte aktualizace kódu.</span><span class="sxs-lookup"><span data-stu-id="3928a-111">To authenticate calls from logic apps to your APIs, you can set up Azure Active Directory in the Azure portal so you don't have to update your code.</span></span> <span data-ttu-id="3928a-112">Nebo můžete požadovat a vynutit ověřování prostřednictvím kódu vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3928a-112">Or, you can require and enforce authentication through your API's code.</span></span>

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a><span data-ttu-id="3928a-113">Nasadit své rozhraní API jako webové aplikace nebo aplikace API</span><span class="sxs-lookup"><span data-stu-id="3928a-113">Deploy your API as a web app or API app</span></span>

<span data-ttu-id="3928a-114">Než bude možné volat vlastního rozhraní API z aplikace logiky, nasaďte své rozhraní API jako webové aplikace nebo aplikace API Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3928a-114">Before you can call your custom API from a logic app, deploy your API as a web app or API app to Azure App Service.</span></span> <span data-ttu-id="3928a-115">Také, aby váš dokument Swagger přečíst návrháře aplikace logiky, nastavte vlastnosti definice rozhraní API a zapnout [(CORS) pro sdílení prostředků různého původu](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) pro webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="3928a-115">Also, to make your Swagger document readable by the Logic App Designer, set the API definition properties and turn on [cross-origin resource sharing (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) for your web app or API app.</span></span>

1. <span data-ttu-id="3928a-116">Na portálu Azure vyberte webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="3928a-116">In the Azure portal, select your web app or API app.</span></span>

2. <span data-ttu-id="3928a-117">V okně, které se otevře v části **rozhraní API**, zvolte **definice rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="3928a-117">In the blade that opens, under **API**, choose **API definition**.</span></span> <span data-ttu-id="3928a-118">Nastavte **umístění definice rozhraní API** na adresu URL pro váš soubor swagger.json.</span><span class="sxs-lookup"><span data-stu-id="3928a-118">Set the **API definition location** to the URL for your swagger.json file.</span></span>

   <span data-ttu-id="3928a-119">Adresa URL obvykle, zobrazí se v tomto formátu:`https://{name}.azurewebsites.net/swagger/docs/v1)`</span><span class="sxs-lookup"><span data-stu-id="3928a-119">Usually, the URL appears in this format: `https://{name}.azurewebsites.net/swagger/docs/v1)`</span></span>

   ![Odkaz na soubor Swagger pro vaše vlastní rozhraní API](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. <span data-ttu-id="3928a-121">V části **rozhraní API**, zvolte **CORS**.</span><span class="sxs-lookup"><span data-stu-id="3928a-121">Under **API**, choose **CORS**.</span></span> <span data-ttu-id="3928a-122">Nastavení zásad CORS pro **povolené zdroje** k  **'*'** (povolit všechny).</span><span class="sxs-lookup"><span data-stu-id="3928a-122">Set the CORS policy for **Allowed origins** to **'*'** (allow all).</span></span>

   <span data-ttu-id="3928a-123">Toto nastavení umožňuje požadavky z návrháře aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="3928a-123">This setting permits requests from Logic App Designer.</span></span>

   ![Povolení požadavků z návrháře aplikace logiky do vlastního rozhraní API](media/logic-apps-custom-hosted-api/custom-api-cors.png)

<span data-ttu-id="3928a-125">Další informace najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="3928a-125">For more information, see these articles:</span></span>

* [<span data-ttu-id="3928a-126">Přidat metadata Swagger pro rozhraní ASP.NET web API</span><span class="sxs-lookup"><span data-stu-id="3928a-126">Add Swagger metadata for ASP.NET web APIs</span></span>](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [<span data-ttu-id="3928a-127">ASP.NET – webové rozhraní API nasazení do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3928a-127">Deploy ASP.NET web APIs to Azure App Service</span></span>](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a><span data-ttu-id="3928a-128">Volání vlastního rozhraní API z logiku aplikace pracovních postupů</span><span class="sxs-lookup"><span data-stu-id="3928a-128">Call your custom API from logic app workflows</span></span>

<span data-ttu-id="3928a-129">Po nastavení vlastnosti definice rozhraní API a CORS by měl k dispozici pro vás mají být zahrnuty do pracovního postupu aplikace logiky triggery a akce vlastního rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3928a-129">After you set up the API definition properties and CORS, your custom API's triggers and actions should become available for you to include in your logic app workflow.</span></span> 

*  <span data-ttu-id="3928a-130">Chcete-li zobrazit weby, ke kterým mají Swagger adresy URL, můžete procházet své předplatné weby v Návrháři logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="3928a-130">To view the websites that have Swagger URLs, you can browse your subscription websites in the Logic Apps Designer.</span></span>

*  <span data-ttu-id="3928a-131">Chcete-li zobrazit dostupné akce a vstupy tak, že odkazuje na dokumentem Swagger, použijte [HTTP + Swagger akce](../connectors/connectors-native-http-swagger.md).</span><span class="sxs-lookup"><span data-stu-id="3928a-131">To view available actions and inputs by pointing at a Swagger document, use the [HTTP + Swagger action](../connectors/connectors-native-http-swagger.md).</span></span>

*  <span data-ttu-id="3928a-132">K volání jakéhokoli rozhraní API, včetně rozhraní API, která nesmí mít ani vystavit dokumentem Swagger můžete kdykoli vytvořit žádost o se [akce HTTP](../connectors/connectors-native-http.md).</span><span class="sxs-lookup"><span data-stu-id="3928a-132">To call any API, including APIs that don't have or expose a Swagger document, you can always create a request with the [HTTP action](../connectors/connectors-native-http.md).</span></span>

## <a name="authenticate-calls-to-your-custom-api"></a><span data-ttu-id="3928a-133">Ověření volání vlastního rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3928a-133">Authenticate calls to your custom API</span></span>

<span data-ttu-id="3928a-134">Můžete zabezpečit volání vlastního rozhraní API těmito způsoby:</span><span class="sxs-lookup"><span data-stu-id="3928a-134">You can secure calls to your custom API in these ways:</span></span>

* <span data-ttu-id="3928a-135">[Žádné změny kódu](#no-code): ochrana rozhraní API s [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) prostřednictvím portálu Azure, takže nemusíte aktualizace kódu nebo znovu nasadit své rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3928a-135">[No code changes](#no-code): Protect your API with [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) through the Azure portal, so you don't have to update your code or redeploy your API.</span></span>

  > [!NOTE]
  > <span data-ttu-id="3928a-136">Ve výchozím nastavení ověřování Azure AD, které můžete zapnout na portálu Azure neposkytuje podrobné autorizace.</span><span class="sxs-lookup"><span data-stu-id="3928a-136">By default, the Azure AD authentication that you turn on in the Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="3928a-137">Toto ověřování například zamkne rozhraní API k jenom konkrétní klienta, nikoli k konkrétního uživatele nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3928a-137">For example, this authentication locks your API to just a specific tenant, not to a specific user or app.</span></span> 

* <span data-ttu-id="3928a-138">[Aktualizujte kód vaše rozhraní API](#update-code): ochrana rozhraní API vynucením [ověřování pomocí certifikátu](#certificate), [základní ověřování](#basic), nebo [ověřování Azure AD](#azure-ad-code) prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="3928a-138">[Update your API's code](#update-code): Protect your API by enforcing [certificate authentication](#certificate), [basic authentication](#basic), or [Azure AD authentication](#azure-ad-code) through code.</span></span>

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a><span data-ttu-id="3928a-139">Ověření volání rozhraní API beze změny kódu</span><span class="sxs-lookup"><span data-stu-id="3928a-139">Authenticate calls to your API without changing code</span></span>

<span data-ttu-id="3928a-140">Tady je postup je tato metoda:</span><span class="sxs-lookup"><span data-stu-id="3928a-140">Here's the general steps for this method:</span></span>

1. <span data-ttu-id="3928a-141">Vytvořte dvě [identity aplikace Azure Active Directory (Azure AD)](../app-service-api/app-service-api-dotnet-service-principal-auth.md): jeden pro svou aplikaci logiky a jeden pro webové aplikace (nebo aplikace API).</span><span class="sxs-lookup"><span data-stu-id="3928a-141">Create two [Azure Active Directory (Azure AD) application identities](../app-service-api/app-service-api-dotnet-service-principal-auth.md): one for your logic app and one for your web app (or API app).</span></span>

2. <span data-ttu-id="3928a-142">K ověření volání rozhraní API, pomocí přihlašovacích údajů (ID klienta a tajný klíč) [instanční objekt](../app-service-api/app-service-api-dotnet-service-principal-auth.md) který je spojen s Azure AD identity aplikací pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="3928a-142">To authenticate calls to your API, use the credentials (client ID and secret) for the [service principal](../app-service-api/app-service-api-dotnet-service-principal-auth.md) that's associated with the Azure AD application identity for your logic app.</span></span>

3. <span data-ttu-id="3928a-143">ID aplikace zahrňte do vaší definici aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="3928a-143">Include the application IDs in your logic app definition.</span></span>

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a><span data-ttu-id="3928a-144">Část 1: Vytvoření identity aplikací Azure AD pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="3928a-144">Part 1: Create an Azure AD application identity for your logic app</span></span>

<span data-ttu-id="3928a-145">Aplikace logiky používá tuto identitu aplikace služby Azure AD k ověření služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3928a-145">Your logic app uses this Azure AD application identity to authenticate against Azure AD.</span></span> <span data-ttu-id="3928a-146">Stačí nastavit tuto identitu jednou pro váš adresář.</span><span class="sxs-lookup"><span data-stu-id="3928a-146">You only have to set up this identity one time for your directory.</span></span> <span data-ttu-id="3928a-147">Například můžete použít stejnou identitu pro všechny aplikace logiky, i když můžete vytvořit jedinečné identity pro každou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="3928a-147">For example, you can choose to use the same identity for all your logic apps, even though you can create unique identities for each logic app.</span></span> <span data-ttu-id="3928a-148">Můžete nastavit tyto identit na portálu Azure [portál Azure classic](#app-identity-logic-classic), nebo použijte [prostředí PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="3928a-148">You can set up these identities in the Azure portal, [Azure classic portal](#app-identity-logic-classic), or use [PowerShell](#powershell).</span></span>

<span data-ttu-id="3928a-149">**Vytvoření identity aplikací pro svou aplikaci logiky na portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="3928a-149">**Create the application identity for your logic app in the Azure portal**</span></span>

1. <span data-ttu-id="3928a-150">V [portál Azure](https://portal.azure.com "https://portal.azure.com"), zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3928a-150">In the [Azure portal](https://portal.azure.com "https://portal.azure.com"), choose **Azure Active Directory**.</span></span> 

2. <span data-ttu-id="3928a-151">Potvrďte, že jste ve stejném adresáři jako webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="3928a-151">Confirm that you're in the same directory as your web app or API app.</span></span>

   > [!TIP]
   > <span data-ttu-id="3928a-152">Chcete-li přepnout adresáře, klikněte na váš profil a vyberte jiný adresář.</span><span class="sxs-lookup"><span data-stu-id="3928a-152">To switch directories, click your profile and select another directory.</span></span> <span data-ttu-id="3928a-153">Nebo zvolte **přehled** > **přepínač directory**.</span><span class="sxs-lookup"><span data-stu-id="3928a-153">Or, choose **Overview** > **Switch directory**.</span></span>

3. <span data-ttu-id="3928a-154">V adresáři nabídce v části **spravovat**, zvolte **registrace aplikace** > **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3928a-154">On the directory menu, under **Manage**, choose **App registrations** > **New application registration**.</span></span>

   > [!TIP]
   > <span data-ttu-id="3928a-155">Ve výchozím nastavení v seznamu registrace aplikace jsou uvedeny všechny registrace aplikace ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="3928a-155">By default, the app registrations list shows all app registrations in your directory.</span></span> <span data-ttu-id="3928a-156">Chcete-li zobrazit pouze registrace vaší aplikace, u pole hledání, vyberte **Moje aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3928a-156">To view only your app registrations, next to the search box, select **My apps**.</span></span> 

   ![Vytvořit novou registraci aplikace](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. <span data-ttu-id="3928a-158">Zadejte název vaší identity aplikace, ponechejte **typ aplikace** nastavena na **webovou aplikaci nebo rozhraní API**, zadejte jedinečný řetězec formátovaný jako doménu pro **přihlašovací adresa URL**a zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3928a-158">Give your application identity a name, leave **Application type** set to **Web app / API**, provide a unique string formatted as a domain for **Sign-on URL**, and choose **Create**.</span></span>

   ![Zadejte název a adresu URL pro přihlašování identita aplikace](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   <span data-ttu-id="3928a-160">Identita aplikace, kterou jste vytvořili pro svou aplikaci logiky nyní se zobrazí v seznamu aplikací registrace.</span><span class="sxs-lookup"><span data-stu-id="3928a-160">The application identity that you created for your logic app now appears in the app registrations list.</span></span>

   ![Identita aplikace pro svou aplikaci logiky](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. <span data-ttu-id="3928a-162">V seznamu registrace aplikace vyberte novou identitu aplikace.</span><span class="sxs-lookup"><span data-stu-id="3928a-162">In the app registrations list, select your new application identity.</span></span> <span data-ttu-id="3928a-163">Zkopírujte a uložte **ID aplikace** má používat jako "ID klienta" pro svou aplikaci logiky v části 3.</span><span class="sxs-lookup"><span data-stu-id="3928a-163">Copy and save the **Application ID** to use as the "client ID" for your logic app in Part 3.</span></span>

   ![Zkopírujte a uložte ID aplikace pro aplikaci logiky](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. <span data-ttu-id="3928a-165">Pokud nastavení identity aplikace nejsou zobrazeny, zvolte **nastavení** nebo **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="3928a-165">If your application identity settings aren't visible, choose **Settings** or **All settings**.</span></span>

7. <span data-ttu-id="3928a-166">V části **přístup pomocí rozhraní API**, zvolte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="3928a-166">Under **API Access**, choose **Keys**.</span></span> <span data-ttu-id="3928a-167">V části **popis**, zadejte název pro váš klíč.</span><span class="sxs-lookup"><span data-stu-id="3928a-167">Under **Description**, provide a name for your key.</span></span> <span data-ttu-id="3928a-168">V části **Expires**, vyberte dobu trvání pro váš klíč.</span><span class="sxs-lookup"><span data-stu-id="3928a-168">Under **Expires**, select a duration for your key.</span></span>

   <span data-ttu-id="3928a-169">Klíč, který vytváříte funguje jako identita aplikace "tajný" nebo heslo pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="3928a-169">The key that you're creating acts as the application identity's "secret" or password for your logic app.</span></span>

   ![Vytvořte klíč pro identitu aplikace logiky](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. <span data-ttu-id="3928a-171">Na panelu nástrojů vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3928a-171">On the toolbar, choose **Save**.</span></span> <span data-ttu-id="3928a-172">V části **hodnotu**, klíč se teď zobrazí.</span><span class="sxs-lookup"><span data-stu-id="3928a-172">Under **Value**, your key now appears.</span></span> 
<span data-ttu-id="3928a-173">**Nezapomeňte zkopírovat a uložit klíč** pro pozdější použití protože je skrytá klíč když necháte okna klíče.</span><span class="sxs-lookup"><span data-stu-id="3928a-173">**Make sure to copy and save your key** for later use because the key is hidden when you leave the key blade.</span></span>

   <span data-ttu-id="3928a-174">Při konfiguraci aplikace logiky v rámci 3, zadáte tento klíč jako "tajný klíč" nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="3928a-174">When you configure your logic app in Part 3, you specify this key as the "secret" or password.</span></span>

   ![Zkopírujte a uložte klíč pro pozdější](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

<span data-ttu-id="3928a-176">**Vytvoření identity aplikací pro svou aplikaci logiky na portálu Azure classic**</span><span class="sxs-lookup"><span data-stu-id="3928a-176">**Create the application identity for your logic app in the Azure classic portal**</span></span>

1. <span data-ttu-id="3928a-177">Na portálu Azure classic, vyberte [ **služby Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="3928a-177">In the Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2. <span data-ttu-id="3928a-178">Vyberte stejný adresář, který používáte pro webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="3928a-178">Select the same directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="3928a-179">Na **aplikace** , zvolte **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="3928a-179">On the **Applications** tab, choose **Add** at the bottom of the page.</span></span>

4. <span data-ttu-id="3928a-180">Zadejte název vaší identity aplikace a zvolte **Další** (šipka vpravo).</span><span class="sxs-lookup"><span data-stu-id="3928a-180">Give your application identity a name, and choose **Next** (right arrow).</span></span>

5. <span data-ttu-id="3928a-181">V části **vlastností aplikace**, poskytovat jedinečný řetězec formátovaný jako doménu pro **přihlašovací adresa URL** a **identifikátor ID URI aplikace**a zvolte **Complete** (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="3928a-181">Under **App properties**, provide a unique string formatted as a domain for **Sign-on URL** and **App ID URI**, and choose **Complete** (checkmark).</span></span>

6. <span data-ttu-id="3928a-182">Na **konfigurace** kartě, zkopírujte a uložte **ID klienta** pro svou aplikaci logiky pro použití v rámci 3.</span><span class="sxs-lookup"><span data-stu-id="3928a-182">On the **Configure** tab, copy and save the **Client ID** for your logic app to use in Part 3.</span></span>

7. <span data-ttu-id="3928a-183">V části **klíče**, otevřete **vyberte dobu trvání** seznamu.</span><span class="sxs-lookup"><span data-stu-id="3928a-183">Under **Keys**, open the **Select duration** list.</span></span> <span data-ttu-id="3928a-184">Vyberte dobu trvání pro váš klíč.</span><span class="sxs-lookup"><span data-stu-id="3928a-184">Select a duration for your key.</span></span>

   <span data-ttu-id="3928a-185">Klíč, který vytváříte funguje jako identita aplikace "tajný" nebo heslo pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="3928a-185">The key that you're creating acts as the application identity's "secret" or password for your logic app.</span></span>

8. <span data-ttu-id="3928a-186">V dolní části stránky, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3928a-186">At the bottom of the page, choose **Save**.</span></span> <span data-ttu-id="3928a-187">Můžete chtít Počkejte několik sekund.</span><span class="sxs-lookup"><span data-stu-id="3928a-187">You might have to wait a few seconds.</span></span>

9. <span data-ttu-id="3928a-188">V části **klíče**, nezapomeňte zkopírovat a uložit klíč se nyní zobrazí.</span><span class="sxs-lookup"><span data-stu-id="3928a-188">Under **Keys**, make sure to copy and save the key that now appears.</span></span> 

   <span data-ttu-id="3928a-189">Při konfiguraci aplikace logiky v rámci 3, zadáte tento klíč jako "tajný klíč" nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="3928a-189">When you configure your logic app in Part 3, you specify this key as the "secret" or password.</span></span>

<span data-ttu-id="3928a-190">Další informace, zjistěte, jak [konfigurace aplikace služby App Service pomocí přihlášení Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="3928a-190">For more information, learn how to [configure your App Service application to use Azure Active Directory login](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span></span>

<a name="powershell"></a>

<span data-ttu-id="3928a-191">**Vytvoření identity aplikací pro svou aplikaci logiky v prostředí PowerShell**</span><span class="sxs-lookup"><span data-stu-id="3928a-191">**Create the application identity for your logic app in PowerShell**</span></span>

<span data-ttu-id="3928a-192">Můžete provést tuto úlohu prostřednictvím Správce Azure Resource Manager pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3928a-192">You can perform this task through Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="3928a-193">V prostředí PowerShell spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="3928a-193">In PowerShell, run these commands:</span></span>

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. <span data-ttu-id="3928a-194">Nezapomeňte zkopírovat **ID klienta** (identifikátor GUID pro vašeho tenanta Azure AD), **ID aplikace**a heslo, které jste použili.</span><span class="sxs-lookup"><span data-stu-id="3928a-194">Make sure to copy the **Tenant ID** (GUID for your Azure AD tenant), the **Application ID**, and the password that you used.</span></span>

<span data-ttu-id="3928a-195">Další informace, zjistěte, jak [vytvoření objektu služby pomocí prostředí PowerShell pro přístup k prostředkům](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="3928a-195">For more information, learn how to [create a service principal with PowerShell to access resources](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a><span data-ttu-id="3928a-196">Část 2: Vytvoření identity aplikací Azure AD pro vaši webovou aplikaci nebo aplikace API</span><span class="sxs-lookup"><span data-stu-id="3928a-196">Part 2: Create an Azure AD application identity for your web app or API app</span></span>

<span data-ttu-id="3928a-197">Pokud webovou aplikaci nebo aplikaci API je už nasazená, můžete zapnout ověření a vytvoření identity aplikace v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3928a-197">If your web app or API app is already deployed, you can turn on authentication and create the application identity in the Azure portal.</span></span> <span data-ttu-id="3928a-198">Jinak můžete [zapnout ověřování při nasazení pomocí šablony Azure Resource Manager](#authen-deploy).</span><span class="sxs-lookup"><span data-stu-id="3928a-198">Otherwise, you can [turn on authentication when you deploy with an Azure Resource Manager template](#authen-deploy).</span></span> 

<span data-ttu-id="3928a-199">**Vytvoření identity aplikace a zapněte ověřování na portálu Azure pro nasazené aplikace**</span><span class="sxs-lookup"><span data-stu-id="3928a-199">**Create the application identity and turn on authentication in the Azure portal for deployed apps**</span></span>

1. <span data-ttu-id="3928a-200">V [portál Azure](https://portal.azure.com "https://portal.azure.com"), najděte a vyberte webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="3928a-200">In the [Azure portal](https://portal.azure.com "https://portal.azure.com"), find and select your web app or API app.</span></span> 

2. <span data-ttu-id="3928a-201">V části **nastavení**, zvolte **ověřování/autorizace**.</span><span class="sxs-lookup"><span data-stu-id="3928a-201">Under **Settings**, choose **Authentication/Authorization**.</span></span> <span data-ttu-id="3928a-202">V části **ověřování služby aplikace**, zapnout ověřování **na**.</span><span class="sxs-lookup"><span data-stu-id="3928a-202">Under **App Service Authentication**, turn authentication **On**.</span></span> <span data-ttu-id="3928a-203">V části **zprostředkovatele ověřování**, zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3928a-203">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span>

   ![Zapnout ověřování](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. <span data-ttu-id="3928a-205">Teď vytvořte identity aplikací pro webovou aplikaci nebo aplikaci API, jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="3928a-205">Now create an application identity for your web app or API app as shown here.</span></span> <span data-ttu-id="3928a-206">Na **nastavení Azure Active Directory** okně nastavit **režim správy** k **Express**.</span><span class="sxs-lookup"><span data-stu-id="3928a-206">On the **Azure Active Directory Settings** blade, set **Management mode** to **Express**.</span></span> <span data-ttu-id="3928a-207">Zvolte **vytvořit novou aplikaci AD**.</span><span class="sxs-lookup"><span data-stu-id="3928a-207">Choose **Create New AD App**.</span></span> <span data-ttu-id="3928a-208">Zadejte název vaší identity aplikace a zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="3928a-208">Give your application identity a name, and choose **OK**.</span></span> 

   ![Vytvoření identity aplikací pro webovou aplikaci nebo aplikace API](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. <span data-ttu-id="3928a-210">Na **ověřování / autorizace okno**, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3928a-210">On the **Authentication / Authorization blade**, choose **Save**.</span></span>

<span data-ttu-id="3928a-211">Nyní musí najít klienta ID a ID klienta pro identitu aplikací, který je spojen s webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="3928a-211">Now you must find the client ID and tenant ID for the application identity that's associated with your web app or API app.</span></span> <span data-ttu-id="3928a-212">Použijte tyto identifikátory v části 3.</span><span class="sxs-lookup"><span data-stu-id="3928a-212">You use these IDs in Part 3.</span></span> <span data-ttu-id="3928a-213">Proto pokračujte postupem pro portál Azure nebo [portál Azure classic](#find-id-classic).</span><span class="sxs-lookup"><span data-stu-id="3928a-213">So continue with these steps for the Azure portal or [Azure classic portal](#find-id-classic).</span></span>

<span data-ttu-id="3928a-214">**Najít ID klienta aplikace identit a ID klienta pro webovou aplikaci nebo aplikaci API na portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="3928a-214">**Find application identity's client ID and tenant ID for your web app or API app in the Azure portal**</span></span>

1. <span data-ttu-id="3928a-215">V části **zprostředkovatele ověřování**, zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3928a-215">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span> 

   ![Zvolte "Azure Active Directory"](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. <span data-ttu-id="3928a-217">Na **nastavení Azure Active Directory** okně nastavit **režim správy** k **Upřesnit**.</span><span class="sxs-lookup"><span data-stu-id="3928a-217">On the **Azure Active Directory Settings** blade, set **Management mode** to **Advanced**.</span></span>

3. <span data-ttu-id="3928a-218">Kopírování **ID klienta**a uložte tento identifikátor GUID pro použití v části 3.</span><span class="sxs-lookup"><span data-stu-id="3928a-218">Copy the **Client ID**, and save that GUID for use in Part 3.</span></span>

   > [!TIP] 
   > <span data-ttu-id="3928a-219">Pokud **ID klienta** a **Url vystavitele** nemáte objeví, zkuste aktualizovat na portálu Azure a zopakujte krok 1.</span><span class="sxs-lookup"><span data-stu-id="3928a-219">If **Client ID** and **Issuer Url** don't appear, try refreshing the Azure portal, and repeat Step 1.</span></span>

4. <span data-ttu-id="3928a-220">V části **Url vystavitele**, zkopírujte a uložte jenom identifikátor GUID pro část 3.</span><span class="sxs-lookup"><span data-stu-id="3928a-220">Under **Issuer Url**, copy and save just the GUID for Part 3.</span></span> <span data-ttu-id="3928a-221">Můžete také použít tento identifikátor GUID ve vaší webové aplikace nebo aplikace API šablonu nasazení, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="3928a-221">You can also use this GUID in your web app or API app's deployment template, if necessary.</span></span>

   <span data-ttu-id="3928a-222">Tento identifikátor GUID je GUID konkrétní klienta ("ID klienta") a by se zobrazit v této adresy URL:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="3928a-222">This GUID is your specific tenant's GUID ("tenant ID") and should appear in this URL: `https://sts.windows.net/{GUID}`</span></span>

5. <span data-ttu-id="3928a-223">Bez uložení změn, zavřete **nastavení Azure Active Directory** okno.</span><span class="sxs-lookup"><span data-stu-id="3928a-223">Without saving your changes, close the **Azure Active Directory Settings** blade.</span></span>

<a name="find-id-classic"></a>

<span data-ttu-id="3928a-224">**Najít ID klienta aplikace identit a ID klienta pro webovou aplikaci nebo aplikaci API na portálu Azure classic**</span><span class="sxs-lookup"><span data-stu-id="3928a-224">**Find application identity's client ID and tenant ID for your web app or API app in the Azure classic portal**</span></span>

1. <span data-ttu-id="3928a-225">Na portálu Azure classic, vyberte [ **služby Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="3928a-225">In the Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2.  <span data-ttu-id="3928a-226">Vyberte adresář, který používáte pro webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="3928a-226">Select the directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="3928a-227">V **vyhledávání** pole, najděte a vyberte identity aplikací pro webovou aplikaci nebo aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="3928a-227">In the **Search** box, find and select the application identity for your web app or API app.</span></span>

4. <span data-ttu-id="3928a-228">Na **konfigurace** kartě, zkopírujte **ID klienta**a uložte tento identifikátor GUID pro použití v části 3.</span><span class="sxs-lookup"><span data-stu-id="3928a-228">On the **Configure** tab, copy the **Client ID**, and save that GUID for use in Part 3.</span></span>

5. <span data-ttu-id="3928a-229">Po získání ID klienta v dolní části **konfigurace** , zvolte **zobrazit koncové body**.</span><span class="sxs-lookup"><span data-stu-id="3928a-229">After you get the client ID, at the bottom of the **Configure** tab, choose **View endpoints**.</span></span>

6. <span data-ttu-id="3928a-230">Zkopírujte adresu URL pro **dokument federačních metadat**a přejděte na tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3928a-230">Copy the URL for **Federation Metadata Document**, and browse to that URL.</span></span>

7. <span data-ttu-id="3928a-231">V dokumentu metadat, které se otevře, najít kořenu **EntityDescriptor ID** element, který má **entityID** atributů v této podobě:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="3928a-231">In the metadata document that opens, find the root **EntityDescriptor ID** element, which has an **entityID** attribute in this form: `https://sts.windows.net/{GUID}`</span></span> 

      <span data-ttu-id="3928a-232">Identifikátor GUID v tento atribut je GUID konkrétní klienta (ID klienta).</span><span class="sxs-lookup"><span data-stu-id="3928a-232">The GUID in this attribute is your specific tenant's GUID (tenant ID).</span></span>

8. <span data-ttu-id="3928a-233">Zkopírujte ID klienta a uložit toto ID pro použití v části 3 a také použití ve vaší webové aplikace nebo aplikace API šablonu nasazení, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="3928a-233">Copy the tenant ID and save that ID for use in Part 3, and also to use in your web app or API app's deployment template, if necessary.</span></span>

<span data-ttu-id="3928a-234">Další informace naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="3928a-234">For more information, see these topics:</span></span>

* [<span data-ttu-id="3928a-235">Ověřování uživatelů pro aplikace API v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3928a-235">User authentication for API apps in Azure App Service</span></span>](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [<span data-ttu-id="3928a-236">Ověřování a autorizace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3928a-236">Authentication and authorization in Azure App Service</span></span>](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

<span data-ttu-id="3928a-237">**Zapnout ověřování při nasazení pomocí šablony Azure Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="3928a-237">**Turn on authentication when you deploy with an Azure Resource Manager template**</span></span>

<span data-ttu-id="3928a-238">Stále je nutné vytvořit identity aplikací Azure AD pro vaši webovou aplikaci nebo aplikaci API, která se liší od identity aplikace pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="3928a-238">You must still create an Azure AD application identity for your web app or API app that differs from the app identity for your logic app.</span></span> <span data-ttu-id="3928a-239">Pokud chcete vytvořit identita aplikace, postupujte podle předchozí kroky v části 2 pro portál Azure.</span><span class="sxs-lookup"><span data-stu-id="3928a-239">To create the application identity, follow the previous steps in Part 2 for the Azure portal.</span></span> <span data-ttu-id="3928a-240">Můžete také postupujte podle kroků v části 1, ale nezapomeňte použít vaši webovou aplikaci nebo aplikaci API skutečné `https://{URL}` pro **přihlašovací adresa URL** a **identifikátor ID URI aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3928a-240">You can also follow the steps in Part 1, but make sure to use your web app or API app's actual `https://{URL}` for **Sign-on URL** and **App ID URI**.</span></span> <span data-ttu-id="3928a-241">Z těchto kroků budete muset uložit ID klienta a ID klienta pro použití v šabloně nasazení vaší aplikace a také pro část 3.</span><span class="sxs-lookup"><span data-stu-id="3928a-241">From these steps, you have to save both the client ID and tenant ID for use in your app's deployment template and also for Part 3.</span></span>

> [!NOTE]
> <span data-ttu-id="3928a-242">Když vytvoříte identity aplikací Azure AD pro vaši webovou aplikaci nebo aplikaci API, musíte použít na portálu Azure nebo portál Azure classic, nikoli prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3928a-242">When you create the Azure AD application identity for your web app or API app, you must use the Azure portal or Azure classic portal, rather than PowerShell.</span></span> <span data-ttu-id="3928a-243">Prostředí PowerShell nemá nastavit požadovaná oprávnění pro přihlášení uživatelů do webu.</span><span class="sxs-lookup"><span data-stu-id="3928a-243">The PowerShell commandlet doesn't set up the required permissions to sign users into a website.</span></span>

<span data-ttu-id="3928a-244">Po získání klienta ID a ID klienta, patří tyto identifikátory jako subresource vaší webové aplikace nebo aplikace API v šabloně nasazení:</span><span class="sxs-lookup"><span data-stu-id="3928a-244">After you get the client ID and tenant ID, include these IDs as a subresource of your web app or API app in your deployment template:</span></span>

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

<span data-ttu-id="3928a-245">Jak automaticky nasadit prázdnou webovou aplikaci a aplikace logiky společně s ověřování Azure Active Directory, [zobrazit úplnou šablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), nebo klikněte na tlačítko **nasadit do Azure** tady:</span><span class="sxs-lookup"><span data-stu-id="3928a-245">To automatically deploy a blank web app and a logic app together with Azure Active Directory authentication, [view the complete template here](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), or click **Deploy to Azure** here:</span></span>

<span data-ttu-id="3928a-246">[![Nasazení do Azure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="3928a-246">[![Deploy to Azure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span></span>

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a><span data-ttu-id="3928a-247">Část 3: Naplnění části autorizace v aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="3928a-247">Part 3: Populate the Authorization section in your logic app</span></span>

<span data-ttu-id="3928a-248">Předchozí šablona již v této části autorizace nastavit, ale vytváříte-li přímo aplikaci logiky, musí obsahovat části plnou autorizaci.</span><span class="sxs-lookup"><span data-stu-id="3928a-248">The previous template already has this authorization section set up, but if you are directly authoring the logic app, you must include the full authorization section.</span></span>

<span data-ttu-id="3928a-249">Otevřete svou definici. aplikaci logiky v zobrazení kódu, přejděte na **HTTP** části akci najít **autorizace** části a zahrnovat tento řádek:</span><span class="sxs-lookup"><span data-stu-id="3928a-249">Open your logic app definition in code view, go to the **HTTP** action section, find the **Authorization** section, and include this line:</span></span>

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| <span data-ttu-id="3928a-250">Element</span><span class="sxs-lookup"><span data-stu-id="3928a-250">Element</span></span> | <span data-ttu-id="3928a-251">Popis</span><span class="sxs-lookup"><span data-stu-id="3928a-251">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="3928a-252">Klienta</span><span class="sxs-lookup"><span data-stu-id="3928a-252">tenant</span></span> |<span data-ttu-id="3928a-253">Identifikátor GUID pro klienta služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3928a-253">The GUID for the Azure AD tenant</span></span> |
| <span data-ttu-id="3928a-254">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="3928a-254">audience</span></span> |<span data-ttu-id="3928a-255">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="3928a-255">Required.</span></span> <span data-ttu-id="3928a-256">Identifikátor GUID pro cílový prostředek, který chcete získat přístup - ID klienta z identity aplikací pro webovou aplikaci nebo aplikace API</span><span class="sxs-lookup"><span data-stu-id="3928a-256">The GUID for the target resource that you want to access - the client ID from the application identity for your web app or API app</span></span> |
| <span data-ttu-id="3928a-257">clientId</span><span class="sxs-lookup"><span data-stu-id="3928a-257">clientId</span></span> |<span data-ttu-id="3928a-258">Identifikátor GUID pro klienta žádají o přístup - ID klienta z aplikací identity pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="3928a-258">The GUID for the client requesting access - the client ID from the application identity for your logic app</span></span> |
| <span data-ttu-id="3928a-259">tajný klíč</span><span class="sxs-lookup"><span data-stu-id="3928a-259">secret</span></span> |<span data-ttu-id="3928a-260">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="3928a-260">Required.</span></span> <span data-ttu-id="3928a-261">Klíč nebo heslo z identity aplikace pro klienta, který požaduje přístupový token</span><span class="sxs-lookup"><span data-stu-id="3928a-261">The key or password from the application identity for the client that's requesting the access token</span></span> |
| <span data-ttu-id="3928a-262">type</span><span class="sxs-lookup"><span data-stu-id="3928a-262">type</span></span> |<span data-ttu-id="3928a-263">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="3928a-263">The authentication type.</span></span> <span data-ttu-id="3928a-264">Pro ověřování ActiveDirectoryOAuth hodnota je `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="3928a-264">For ActiveDirectoryOAuth authentication, the value is `ActiveDirectoryOAuth`.</span></span> |

<span data-ttu-id="3928a-265">Například:</span><span class="sxs-lookup"><span data-stu-id="3928a-265">For example:</span></span>

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

### <a name="secure-api-calls-through-code"></a><span data-ttu-id="3928a-266">Zabezpečená volání rozhraní API prostřednictvím kódu</span><span class="sxs-lookup"><span data-stu-id="3928a-266">Secure API calls through code</span></span>

<a name="certificate"></a>

#### <a name="certificate-authentication"></a><span data-ttu-id="3928a-267">Ověřování pomocí certifikátu</span><span class="sxs-lookup"><span data-stu-id="3928a-267">Certificate authentication</span></span>

<span data-ttu-id="3928a-268">K ověření příchozích požadavků z aplikace logiky webovou aplikaci nebo aplikaci API, můžete použít klientské certifikáty.</span><span class="sxs-lookup"><span data-stu-id="3928a-268">To validate the incoming requests from your logic app to your web app or API app, you can use client certificates.</span></span> <span data-ttu-id="3928a-269">Chcete-li nastavit kód, zjistěte další [konfiguraci vzájemné ověřování TLS](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span><span class="sxs-lookup"><span data-stu-id="3928a-269">To set up your code, learn [how to configure TLS mutual authentication](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span></span>

<span data-ttu-id="3928a-270">V **autorizace** část, zahrnují tento řádek:</span><span class="sxs-lookup"><span data-stu-id="3928a-270">In the **Authorization** section, include this line:</span></span> 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| <span data-ttu-id="3928a-271">Element</span><span class="sxs-lookup"><span data-stu-id="3928a-271">Element</span></span> | <span data-ttu-id="3928a-272">Popis</span><span class="sxs-lookup"><span data-stu-id="3928a-272">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="3928a-273">type</span><span class="sxs-lookup"><span data-stu-id="3928a-273">type</span></span> |<span data-ttu-id="3928a-274">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="3928a-274">Required.</span></span> <span data-ttu-id="3928a-275">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="3928a-275">The authentication type.</span></span> <span data-ttu-id="3928a-276">Pro klientské certifikáty protokolu SSL, hodnota musí být `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="3928a-276">For SSL client certificates, the value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="3928a-277">heslo</span><span class="sxs-lookup"><span data-stu-id="3928a-277">password</span></span> |<span data-ttu-id="3928a-278">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="3928a-278">Required.</span></span> <span data-ttu-id="3928a-279">Heslo pro přístup k certifikátu klienta (soubor PFX)</span><span class="sxs-lookup"><span data-stu-id="3928a-279">The password for accessing the client certificate (PFX file)</span></span> |
| <span data-ttu-id="3928a-280">Soubor PFX</span><span class="sxs-lookup"><span data-stu-id="3928a-280">pfx</span></span> |<span data-ttu-id="3928a-281">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="3928a-281">Required.</span></span> <span data-ttu-id="3928a-282">Obsah klientského certifikátu (soubor PFX) s kódováním base64</span><span class="sxs-lookup"><span data-stu-id="3928a-282">Base64-encoded contents of the client certificate (PFX file)</span></span> |

<a name="basic"></a>

#### <a name="basic-authentication"></a><span data-ttu-id="3928a-283">Základní ověřování</span><span class="sxs-lookup"><span data-stu-id="3928a-283">Basic authentication</span></span>

<span data-ttu-id="3928a-284">K ověření příchozích požadavků z aplikace logiky webovou aplikaci nebo aplikaci API, můžete základní ověřování, jako je například uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="3928a-284">To validate incoming requests from your logic app to your web app or API app, you can use basic authentication, such as a username and password.</span></span> <span data-ttu-id="3928a-285">Základní ověřování je běžný vzor, a toto ověřování můžete použít v libovolném jazyce sloužící k vytvoření webové aplikace nebo aplikace API.</span><span class="sxs-lookup"><span data-stu-id="3928a-285">Basic authentication is a common pattern, and you can use this authentication in any language used to build your web app or API app.</span></span>

<span data-ttu-id="3928a-286">V **autorizace** část, zahrnují tento řádek:</span><span class="sxs-lookup"><span data-stu-id="3928a-286">In the **Authorization** section, include this line:</span></span>

<span data-ttu-id="3928a-287">`{"type": "basic", "username": "username", "password": "password"}`.</span><span class="sxs-lookup"><span data-stu-id="3928a-287">`{"type": "basic", "username": "username", "password": "password"}`.</span></span>

| <span data-ttu-id="3928a-288">Element</span><span class="sxs-lookup"><span data-stu-id="3928a-288">Element</span></span> | <span data-ttu-id="3928a-289">Popis</span><span class="sxs-lookup"><span data-stu-id="3928a-289">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3928a-290">type</span><span class="sxs-lookup"><span data-stu-id="3928a-290">type</span></span> |<span data-ttu-id="3928a-291">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="3928a-291">Required.</span></span> <span data-ttu-id="3928a-292">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="3928a-292">The authentication type.</span></span> <span data-ttu-id="3928a-293">Pro základní ověřování, musí být hodnota `Basic`.</span><span class="sxs-lookup"><span data-stu-id="3928a-293">For basic authentication, the value must be `Basic`.</span></span> |
| <span data-ttu-id="3928a-294">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3928a-294">username</span></span> |<span data-ttu-id="3928a-295">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="3928a-295">Required.</span></span> <span data-ttu-id="3928a-296">Uživatelské jméno pro ověřování</span><span class="sxs-lookup"><span data-stu-id="3928a-296">The username for authentication</span></span> |
| <span data-ttu-id="3928a-297">heslo</span><span class="sxs-lookup"><span data-stu-id="3928a-297">password</span></span> |<span data-ttu-id="3928a-298">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="3928a-298">Required.</span></span> <span data-ttu-id="3928a-299">Heslo pro ověřování</span><span class="sxs-lookup"><span data-stu-id="3928a-299">The password for authentication</span></span> |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a><span data-ttu-id="3928a-300">Ověřování Azure Active Directory prostřednictvím kódu</span><span class="sxs-lookup"><span data-stu-id="3928a-300">Azure Active Directory authentication through code</span></span>

<span data-ttu-id="3928a-301">Ve výchozím nastavení ověřování Azure AD, které můžete zapnout na portálu Azure neposkytuje podrobné autorizace.</span><span class="sxs-lookup"><span data-stu-id="3928a-301">By default, the Azure AD authentication that you turn on in the Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="3928a-302">Toto ověřování například zamkne rozhraní API k jenom konkrétní klienta, nikoli k konkrétního uživatele nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3928a-302">For example, this authentication locks your API to just a specific tenant, not to a specific user or app.</span></span> 

<span data-ttu-id="3928a-303">Pokud chcete omezit přístup pomocí rozhraní API do aplikace logiky prostřednictvím kódu, extrahujte záhlaví, který má webového tokenu JSON (JWT).</span><span class="sxs-lookup"><span data-stu-id="3928a-303">To restrict API access to your logic app through code, extract the header that has the JSON web token (JWT).</span></span> <span data-ttu-id="3928a-304">Zkontrolujte identitu volajícího a odmítnout požadavky, které se neshodují.</span><span class="sxs-lookup"><span data-stu-id="3928a-304">Check the caller's identity, and reject requests that don't match.</span></span>

<span data-ttu-id="3928a-305">K implementaci tohoto ověřování zcela v kódu a nechcete použít portál Azure, budete pokračovat, zjistěte, jak [ověření pomocí místní služby Active Directory v aplikaci Azure](../app-service-web/web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="3928a-305">Going further, to implement this authentication entirely in your own code, and not use the Azure portal, learn how to [authenticate with on-premises Active Directory in your Azure app](../app-service-web/web-sites-authentication-authorization.md).</span></span>

<span data-ttu-id="3928a-306">Vytvoření identity aplikací pro svou aplikaci logiky a tuto identitu používat pro volání rozhraní API, postupujte podle předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="3928a-306">To create an application identity for your logic app and use that identity to call your API, you must follow the previous steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3928a-307">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3928a-307">Next steps</span></span>

* [<span data-ttu-id="3928a-308">Kontrola výkonu aplikace logiky s diagnostické protokoly a výstrahy</span><span class="sxs-lookup"><span data-stu-id="3928a-308">Check logic app performance with diagnostic logs and alerts</span></span>](logic-apps-monitor-your-logic-apps.md)