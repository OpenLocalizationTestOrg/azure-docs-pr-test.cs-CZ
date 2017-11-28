---
title: "Azure Active Directory aaaUse, tooauthenticate řešení služby Azure Batch | Microsoft Docs"
description: "Batch podporuje Azure AD pro ověřování z hello služby Batch."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/20/2017
ms.author: tamram
ms.openlocfilehash: 6c825c30f1c80bb059a797a2e78367e599acd109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a><span data-ttu-id="1b25c-103">Ověření řešení služby Batch se službou Active Directory</span><span class="sxs-lookup"><span data-stu-id="1b25c-103">Authenticate Batch service solutions with Active Directory</span></span>

<span data-ttu-id="1b25c-104">Azure Batch podporuje ověřování s [Azure Active Directory] [ aad_about] (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1b25c-104">Azure Batch supports authentication with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="1b25c-105">Azure AD je víceklientské cloudový adresář společnosti Microsoft a služba identity management.</span><span class="sxs-lookup"><span data-stu-id="1b25c-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="1b25c-106">Azure samotné používá tooauthenticate Azure AD, jeho zákazníků, správci služeb a organizační uživatele.</span><span class="sxs-lookup"><span data-stu-id="1b25c-106">Azure itself uses Azure AD tooauthenticate its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="1b25c-107">Pokud používáte ověřování Azure AD pomocí služby Azure Batch, můžete ověřovat v jednom ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="1b25c-107">When using Azure AD authentication with Azure Batch, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="1b25c-108">Pomocí **integrované ověřování** tooauthenticate uživatele, který komunikuje s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="1b25c-108">By using **integrated authentication** tooauthenticate a user that is interacting with hello application.</span></span> <span data-ttu-id="1b25c-109">Aplikace prostřednictvím integrovaného ověřování přihlašovacích údajů uživatele shromažďuje a používá tooBatch tyto přihlašovací údaje tooauthenticate přístup k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="1b25c-109">An application using integrated authentication gathers a user's credentials and uses those credentials tooauthenticate access tooBatch resources.</span></span>
- <span data-ttu-id="1b25c-110">Pomocí **instanční objekt** tooauthenticate bezobslužné aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b25c-110">By using a **service principal** tooauthenticate an unattended application.</span></span> <span data-ttu-id="1b25c-111">Objekt služby definuje hello zásady a oprávnění pro aplikaci v aplikaci hello toorepresent pořadí, při přístupu k prostředkům v době běhu.</span><span class="sxs-lookup"><span data-stu-id="1b25c-111">A service principal defines hello policy and permissions for an application in order toorepresent hello application when accessing resources at runtime.</span></span>

<span data-ttu-id="1b25c-112">toolearn Další informace o službě Azure AD, najdete v části hello [Azure Active Directory dokumentaci](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="1b25c-112">toolearn more about Azure AD, see hello [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span>

## <a name="authentication-and-pool-allocation-mode"></a><span data-ttu-id="1b25c-113">Režim ověřování a fondu přidělení</span><span class="sxs-lookup"><span data-stu-id="1b25c-113">Authentication and pool allocation mode</span></span>

<span data-ttu-id="1b25c-114">Při vytváření účtu Batch, můžete zadat, kde by měla být pro tento účet vytvořit fondy přidělená.</span><span class="sxs-lookup"><span data-stu-id="1b25c-114">When you create a Batch account, you can specify where pools created for that account should be allocated.</span></span> <span data-ttu-id="1b25c-115">Můžete tooallocate fondy v předplatném služby Batch výchozí hello nebo v předplatném uživatele.</span><span class="sxs-lookup"><span data-stu-id="1b25c-115">You can choose tooallocate pools either in hello default Batch service subscription or in a user subscription.</span></span> <span data-ttu-id="1b25c-116">Svou volbu ovlivňuje, jak ověřovat přístup tooresources v daném účtu.</span><span class="sxs-lookup"><span data-stu-id="1b25c-116">Your choice affects how you authenticate access tooresources in that account.</span></span>

- <span data-ttu-id="1b25c-117">**Předplatné služby batch**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-117">**Batch service subscription**.</span></span> <span data-ttu-id="1b25c-118">Ve výchozím nastavení jsou fondy Batch přidělené v předplatné služby Batch.</span><span class="sxs-lookup"><span data-stu-id="1b25c-118">By default, Batch pools are allocated in a Batch service subscription.</span></span> <span data-ttu-id="1b25c-119">Pokud zvolíte tuto možnost, můžete ověřovat přístup tooresources v daném účtu buď pomocí [sdílený klíč](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) nebo s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1b25c-119">If you choose this option, you can authenticate access tooresources in that account either with [Shared Key](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) or with Azure AD.</span></span>
- <span data-ttu-id="1b25c-120">**Předplatné uživatele.**</span><span class="sxs-lookup"><span data-stu-id="1b25c-120">**User subscription.**</span></span> <span data-ttu-id="1b25c-121">Můžete tooallocate fondy Batch v předplatném uživatele, který určíte.</span><span class="sxs-lookup"><span data-stu-id="1b25c-121">You can choose tooallocate Batch pools in a user subscription that you specify.</span></span> <span data-ttu-id="1b25c-122">Pokud zvolíte tuto možnost, je třeba ověřit pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1b25c-122">If you choose this option, you must authenticate with Azure AD.</span></span>

## <a name="endpoints-for-authentication"></a><span data-ttu-id="1b25c-123">Koncové body pro ověřování</span><span class="sxs-lookup"><span data-stu-id="1b25c-123">Endpoints for authentication</span></span>

<span data-ttu-id="1b25c-124">aplikace Batch tooauthenticate s Azure AD, je nutné tooinclude některé známé koncové body v kódu.</span><span class="sxs-lookup"><span data-stu-id="1b25c-124">tooauthenticate Batch applications with Azure AD, you need tooinclude some well-known endpoints in your code.</span></span>

### <a name="azure-ad-endpoint"></a><span data-ttu-id="1b25c-125">Koncový bod Azure AD</span><span class="sxs-lookup"><span data-stu-id="1b25c-125">Azure AD endpoint</span></span>

<span data-ttu-id="1b25c-126">základní Hello je koncový bod autority Azure AD:</span><span class="sxs-lookup"><span data-stu-id="1b25c-126">hello base Azure AD authority endpoint is:</span></span>

`https://login.microsoftonline.com/`

<span data-ttu-id="1b25c-127">tooauthenticate s Azure AD, použijte tento koncový bod společně s hello ID klienta (ID adresáře).</span><span class="sxs-lookup"><span data-stu-id="1b25c-127">tooauthenticate with Azure AD, you use this endpoint together with hello tenant ID (directory ID).</span></span> <span data-ttu-id="1b25c-128">ID klienta Hello identifikuje toouse klienta hello Azure AD pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="1b25c-128">hello tenant ID identifies hello Azure AD tenant toouse for authentication.</span></span> <span data-ttu-id="1b25c-129">tooretrieve hello ID klienta, postupujte podle kroků hello uvedených v [získání ID tenanta hello vaší služby Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="1b25c-129">tooretrieve hello tenant ID, follow hello steps outlined in [Get hello tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> <span data-ttu-id="1b25c-130">koncový bod konkrétního klienta Hello je potřeba při ověření pomocí objektu služby.</span><span class="sxs-lookup"><span data-stu-id="1b25c-130">hello tenant-specific endpoint is required when you authenticate using a service principal.</span></span> 
> 
> <span data-ttu-id="1b25c-131">koncový bod pro konkrétního klienta k Hello je volitelný při ověřování pomocí integrovaného ověřování, ale doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="1b25c-131">hello tenant-specific endpoint is optional when you authenticate using integrated authentication, but recommended.</span></span> <span data-ttu-id="1b25c-132">Však můžete také použít běžné koncového bodu Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="1b25c-132">However, you can also use hello Azure AD common endpoint.</span></span> <span data-ttu-id="1b25c-133">běžné koncový bod Hello poskytuje obecné pověření shromažďování rozhraní, když není k dispozici konkrétní klienta.</span><span class="sxs-lookup"><span data-stu-id="1b25c-133">hello common endpoint provides a generic credential gathering interface when a specific tenant is not provided.</span></span> <span data-ttu-id="1b25c-134">běžné koncový bod Hello je `https://login.microsoftonline.com/common`.</span><span class="sxs-lookup"><span data-stu-id="1b25c-134">hello common endpoint is `https://login.microsoftonline.com/common`.</span></span>
>
>

<span data-ttu-id="1b25c-135">Další informace o koncové body Azure AD najdete v tématu [scénáře ověřování pro Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="1b25c-135">For more information about Azure AD endpoints, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>

### <a name="batch-resource-endpoint"></a><span data-ttu-id="1b25c-136">Koncový bod prostředků batch</span><span class="sxs-lookup"><span data-stu-id="1b25c-136">Batch resource endpoint</span></span>

<span data-ttu-id="1b25c-137">Použití hello **koncový bod prostředků Azure Batch** tooacquire token pro ověřování požadavků služby Batch toohello:</span><span class="sxs-lookup"><span data-stu-id="1b25c-137">Use hello **Azure Batch resource endpoint** tooacquire a token for authenticating requests toohello Batch service:</span></span>

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a><span data-ttu-id="1b25c-138">Registrace aplikace pomocí klienta</span><span class="sxs-lookup"><span data-stu-id="1b25c-138">Register your application with a tenant</span></span>

<span data-ttu-id="1b25c-139">Hello prvním krokem při použití služby Azure AD tooauthenticate je registrace vaší aplikace v klient služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1b25c-139">hello first step in using Azure AD tooauthenticate is registering your application in an Azure AD tenant.</span></span> <span data-ttu-id="1b25c-140">Registrace aplikace umožňuje toocall hello Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="1b25c-140">Registering your application enables you toocall hello Azure [Active Directory Authentication Library][aad_adal] (ADAL) from your code.</span></span> <span data-ttu-id="1b25c-141">Hello ADAL poskytuje rozhraní API pro ověřování s Azure AD z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b25c-141">hello ADAL provides an API for authenticating with Azure AD from your application.</span></span> <span data-ttu-id="1b25c-142">Registrace aplikace je vyžadováno, zda máte v plánu toouse integrované ověřování nebo hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="1b25c-142">Registering your application is required whether you plan toouse integrated authentication or a service principal.</span></span>

<span data-ttu-id="1b25c-143">Při registraci vaší aplikace, můžete zadat informace o vaší aplikaci tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="1b25c-143">When you register your application, you supply information about your application tooAzure AD.</span></span> <span data-ttu-id="1b25c-144">Potom Azure AD poskytuje ID aplikací použít tooassociate aplikaci s Azure AD za běhu.</span><span class="sxs-lookup"><span data-stu-id="1b25c-144">Azure AD then provides an application ID that you use tooassociate your application with Azure AD at runtime.</span></span> <span data-ttu-id="1b25c-145">toolearn Další informace o ID aplikace hello, najdete v části [aplikace a služby hlavní objekty ve službě Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="1b25c-145">toolearn more about hello application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="1b25c-146">tooregister aplikace Batch, postupujte podle kroků hello v hello [přidání aplikace](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) kapitoly [integrace aplikací s Azure Active Directory][aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="1b25c-146">tooregister your Batch application, follow hello steps in hello [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="1b25c-147">Když si zaregistrujete aplikaci jako nativní aplikaci, můžete zadat libovolný platný identifikátor URI pro hello **identifikátor URI pro přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-147">If you register your application as a Native Application, you can specify any valid URI for hello **Redirect URI**.</span></span> <span data-ttu-id="1b25c-148">To není vyžadováno toobe skutečné koncový bod.</span><span class="sxs-lookup"><span data-stu-id="1b25c-148">It does not need toobe a real endpoint.</span></span>

<span data-ttu-id="1b25c-149">Poté, co jste registrováni vaší aplikace, se zobrazí ID aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="1b25c-149">After you've registered your application, you'll see hello application ID:</span></span>

![Registrace aplikace Batch pomocí Azure AD](./media/batch-aad-auth/app-registration-data-plane.png)

<span data-ttu-id="1b25c-151">Další informace o registraci aplikace s Azure AD najdete v tématu [scénáře ověřování pro Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="1b25c-151">For more information about registering an application with Azure AD, see [Authentication Scenarios for Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span></span>

## <a name="get-hello-tenant-id-for-your-active-directory"></a><span data-ttu-id="1b25c-152">Získání ID tenanta hello vaší služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="1b25c-152">Get hello tenant ID for your Active Directory</span></span>

<span data-ttu-id="1b25c-153">ID klienta Hello identifikuje hello klienta Azure AD, který poskytuje ověřování služby tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b25c-153">hello tenant ID identifies hello Azure AD tenant that provides authentication services tooyour application.</span></span> <span data-ttu-id="1b25c-154">tooget hello ID klienta, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="1b25c-154">tooget hello tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="1b25c-155">V hello portálu Azure vyberte vaší služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1b25c-155">In hello Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="1b25c-156">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-156">Click **Properties**.</span></span>
3. <span data-ttu-id="1b25c-157">Zkopírujte hello GUID hodnota zadaná pro ID hello adresáře.</span><span class="sxs-lookup"><span data-stu-id="1b25c-157">Copy hello GUID value provided for hello directory ID.</span></span> <span data-ttu-id="1b25c-158">Tato hodnota se označuje taky jako ID hello klienta.</span><span class="sxs-lookup"><span data-stu-id="1b25c-158">This value is also called hello tenant ID.</span></span>

![Zkopírujte ID adresáře hello](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a><span data-ttu-id="1b25c-160">Použít integrované ověřování</span><span class="sxs-lookup"><span data-stu-id="1b25c-160">Use integrated authentication</span></span>

<span data-ttu-id="1b25c-161">tooauthenticate s integrované ověřování, musíte toogrant toohello tooconnect oprávnění vaší aplikace API služby Batch.</span><span class="sxs-lookup"><span data-stu-id="1b25c-161">tooauthenticate with integrated authentication, you need toogrant your application permissions tooconnect toohello Batch service API.</span></span> <span data-ttu-id="1b25c-162">Tento krok povoluje aplikace tooauthenticate volání toohello Batch služby rozhraní API pomocí Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1b25c-162">This step enables your application tooauthenticate calls toohello Batch service API with Azure AD.</span></span>

<span data-ttu-id="1b25c-163">Jakmile jste [zaregistrovat aplikaci](#register-your-application-with-an-azure-ad-tenant), postupujte podle těchto kroků v hello Azure portálu toogrant, že služba Batch toohello přístup:</span><span class="sxs-lookup"><span data-stu-id="1b25c-163">Once you've [registered your application](#register-your-application-with-an-azure-ad-tenant), follow these steps in hello Azure portal toogrant it access toohello Batch service:</span></span>

1. <span data-ttu-id="1b25c-164">V levém navigačním podokně hello hello portálu Azure, zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-164">In hello left-hand navigation pane of hello Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="1b25c-165">Hledat hello název vaší aplikace v seznamu hello registrací aplikace:</span><span class="sxs-lookup"><span data-stu-id="1b25c-165">Search for hello name of your application in hello list of app registrations:</span></span>

    ![Vyhledávání pro název aplikace](./media/batch-aad-auth/search-app-registration.png)

3. <span data-ttu-id="1b25c-167">Otevřete hello **nastavení** okno pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1b25c-167">Open hello **Settings** blade for your application.</span></span> <span data-ttu-id="1b25c-168">V hello **přístup pomocí rozhraní API** vyberte **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-168">In hello **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="1b25c-169">V hello **požadovaná oprávnění** okně klikněte na tlačítko hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1b25c-169">In hello **Required permissions** blade, click hello **Add** button.</span></span>
5. <span data-ttu-id="1b25c-170">V kroku 1 vyhledejte hello Batch API.</span><span class="sxs-lookup"><span data-stu-id="1b25c-170">In step 1, search for hello Batch API.</span></span> <span data-ttu-id="1b25c-171">Vyhledávání pro každý z těchto řetězců vyhledejte hello rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="1b25c-171">Search for each of these strings until you find hello API:</span></span>
    1. <span data-ttu-id="1b25c-172">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-172">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="1b25c-173">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-173">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="1b25c-174">Novější tenanti služby Azure AD mohou používat tento název.</span><span class="sxs-lookup"><span data-stu-id="1b25c-174">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="1b25c-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** se hello ID hello Batch API.</span><span class="sxs-lookup"><span data-stu-id="1b25c-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** is hello ID for hello Batch API.</span></span> 
6. <span data-ttu-id="1b25c-176">Po nalezení hello Batch API, vyberte ho a klikněte na tlačítko hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1b25c-176">Once you find hello Batch API, select it and click hello **Select** button.</span></span>
6. <span data-ttu-id="1b25c-177">V kroku 2, vyberte hello zaškrtněte políčko vedle příliš**služby Azure Batch přístup** a klikněte na tlačítko hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1b25c-177">In step 2, select hello check box next too**Access Azure Batch Service** and click hello **Select** button.</span></span>
7. <span data-ttu-id="1b25c-178">Klikněte na tlačítko hello **provádí** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1b25c-178">Click hello **Done** button.</span></span>

<span data-ttu-id="1b25c-179">Hello **požadovaných oprávnění** okno teď zobrazuje, které vaše aplikace Azure AD nemá přístup k tooboth ADAL a hello rozhraní API služby Batch.</span><span class="sxs-lookup"><span data-stu-id="1b25c-179">hello **Required Permissions** blade now shows that your Azure AD application has access tooboth ADAL and hello Batch service API.</span></span> <span data-ttu-id="1b25c-180">Oprávnění tooADAL automaticky při první registraci vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1b25c-180">Permissions are granted tooADAL automatically when you first register your app with Azure AD.</span></span>

![Rozhraní API udělení oprávnění](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a><span data-ttu-id="1b25c-182">Použít hlavní název služby</span><span class="sxs-lookup"><span data-stu-id="1b25c-182">Use a service principal</span></span> 

<span data-ttu-id="1b25c-183">tooauthenticate aplikaci, která běží bezobslužné, použijte objekt služby.</span><span class="sxs-lookup"><span data-stu-id="1b25c-183">tooauthenticate an application that runs unattended, you use a service principal.</span></span> <span data-ttu-id="1b25c-184">Poté, co jste registrováni vaší aplikace, pomocí těchto kroků v hello Azure portálu tooconfigure objekt služby:</span><span class="sxs-lookup"><span data-stu-id="1b25c-184">After you've registered your application, follow these steps in hello Azure portal tooconfigure a service principal:</span></span>

1. <span data-ttu-id="1b25c-185">Požádat o tajný klíč pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1b25c-185">Request a secret key for your application.</span></span>
2. <span data-ttu-id="1b25c-186">Přiřaďte aplikaci tooyour role RBAC.</span><span class="sxs-lookup"><span data-stu-id="1b25c-186">Assign an RBAC role tooyour application.</span></span>

### <a name="request-a-secret-key-for-your-application"></a><span data-ttu-id="1b25c-187">Žádost o tajný klíč pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="1b25c-187">Request a secret key for your application</span></span>

<span data-ttu-id="1b25c-188">Když se aplikace ověřuje pomocí objektu služby, odešle ID aplikace hello a tajný klíč tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="1b25c-188">When your application authenticates with a service principal, it sends both hello application ID and a secret key tooAzure AD.</span></span> <span data-ttu-id="1b25c-189">Budete potřebovat toocreate a zkopírovat tajný klíč toouse hello z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="1b25c-189">You'll need toocreate and copy hello secret key toouse from your code.</span></span>

<span data-ttu-id="1b25c-190">Postupujte podle těchto kroků v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="1b25c-190">Follow these steps in hello Azure portal:</span></span>

1. <span data-ttu-id="1b25c-191">V levém navigačním podokně hello hello portálu Azure, zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-191">In hello left-hand navigation pane of hello Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="1b25c-192">Vyhledat hello název vaší aplikace v seznamu hello registrací aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b25c-192">Search for hello name of your application in hello list of app registrations.</span></span>
3. <span data-ttu-id="1b25c-193">Zobrazení hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="1b25c-193">Display hello **Settings** blade.</span></span> <span data-ttu-id="1b25c-194">V hello **přístup pomocí rozhraní API** vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-194">In hello **API Access** section, select **Keys**.</span></span>
4. <span data-ttu-id="1b25c-195">toocreate klíč, zadejte popis pro klíč hello.</span><span class="sxs-lookup"><span data-stu-id="1b25c-195">toocreate a key, enter a description for hello key.</span></span> <span data-ttu-id="1b25c-196">Pak vyberte dobu trvání pro klíč hello jeden nebo dva roky.</span><span class="sxs-lookup"><span data-stu-id="1b25c-196">Then select a duration for hello key of either one or two years.</span></span> 
5. <span data-ttu-id="1b25c-197">Klikněte na tlačítko hello **Uložit** tlačítko toocreate a zobrazit hello klíč.</span><span class="sxs-lookup"><span data-stu-id="1b25c-197">Click hello **Save** button toocreate and display hello key.</span></span> <span data-ttu-id="1b25c-198">Zkopírujte hello hodnota klíče tooa bezpečné místo, protože nebudou moct tooaccess ho znovu po nechte okno hello.</span><span class="sxs-lookup"><span data-stu-id="1b25c-198">Copy hello key value tooa safe place, as you won't be able tooaccess it again after you leave hello blade.</span></span> 

    ![Vytvoření tajný klíč](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-tooyour-application"></a><span data-ttu-id="1b25c-200">Přiřazení aplikace tooyour role RBAC</span><span class="sxs-lookup"><span data-stu-id="1b25c-200">Assign an RBAC role tooyour application</span></span>

<span data-ttu-id="1b25c-201">tooauthenticate s hlavní název služby, je nutné tooassign aplikace tooyour role RBAC.</span><span class="sxs-lookup"><span data-stu-id="1b25c-201">tooauthenticate with a service principal, you need tooassign an RBAC role tooyour application.</span></span> <span data-ttu-id="1b25c-202">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="1b25c-202">Follow these steps:</span></span>

1. <span data-ttu-id="1b25c-203">V hello portálu Azure přejděte toohello účtu Batch používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b25c-203">In hello Azure portal, navigate toohello Batch account used by your application.</span></span>
2. <span data-ttu-id="1b25c-204">V hello **nastavení** okno pro účet Batch hello, vyberte **řízení přístupu (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-204">In hello **Settings** blade for hello Batch account, select **Access Control (IAM)**.</span></span>
3. <span data-ttu-id="1b25c-205">Klikněte na tlačítko hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1b25c-205">Click hello **Add** button.</span></span> 
4. <span data-ttu-id="1b25c-206">Z hello **Role** rozevíracího seznamu, vyberte buď hello _Přispěvatel_ nebo _čtečky_ role pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1b25c-206">From hello **Role** drop-down, choose either hello _Contributor_ or _Reader_ role for your application.</span></span> <span data-ttu-id="1b25c-207">Další informace o těchto rolí najdete v tématu [Začínáme s řízením přístupu na základě rolí v hello portál Azure](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="1b25c-207">For more information on these roles, see [Get started with Role-Based Access Control in hello Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>  
5. <span data-ttu-id="1b25c-208">V hello **vyberte** zadejte hello název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b25c-208">In hello **Select** field, enter hello name of your application.</span></span> <span data-ttu-id="1b25c-209">Vyberte aplikace hello seznamu a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-209">Select your application from hello list, and click **Save**.</span></span>

<span data-ttu-id="1b25c-210">Aplikace by se měla zobrazit v nastavení řízení přístupu s přiřazenou RBAC roli.</span><span class="sxs-lookup"><span data-stu-id="1b25c-210">Your application should now appear in your access control settings with an RBAC role assigned.</span></span> 

![Přiřazení aplikace tooyour role RBAC](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-hello-tenant-id-for-your-azure-active-directory"></a><span data-ttu-id="1b25c-212">Získání ID tenanta hello vaší služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1b25c-212">Get hello tenant ID for your Azure Active Directory</span></span>

<span data-ttu-id="1b25c-213">ID klienta Hello identifikuje hello klienta Azure AD, který poskytuje ověřování služby tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b25c-213">hello tenant ID identifies hello Azure AD tenant that provides authentication services tooyour application.</span></span> <span data-ttu-id="1b25c-214">tooget hello ID klienta, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="1b25c-214">tooget hello tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="1b25c-215">V hello portálu Azure vyberte vaší služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1b25c-215">In hello Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="1b25c-216">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1b25c-216">Click **Properties**.</span></span>
3. <span data-ttu-id="1b25c-217">Zkopírujte hello GUID hodnota zadaná pro ID hello adresáře.</span><span class="sxs-lookup"><span data-stu-id="1b25c-217">Copy hello GUID value provided for hello directory ID.</span></span> <span data-ttu-id="1b25c-218">Tato hodnota se označuje taky jako ID hello klienta.</span><span class="sxs-lookup"><span data-stu-id="1b25c-218">This value is also called hello tenant ID.</span></span>

![Zkopírujte ID adresáře hello](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a><span data-ttu-id="1b25c-220">Příklady kódu</span><span class="sxs-lookup"><span data-stu-id="1b25c-220">Code examples</span></span>

<span data-ttu-id="1b25c-221">Příklady kódu Hello v této části ukazují, jak tooauthenticate s Azure AD pomocí integrované ověřování a pomocí objektu služby.</span><span class="sxs-lookup"><span data-stu-id="1b25c-221">hello code examples in this section show how tooauthenticate with Azure AD using integrated authentication and with a service principal.</span></span> <span data-ttu-id="1b25c-222">Tyto příklady kódu pomocí rozhraní .NET, ale hello koncepty jsou podobné jako u ostatních jazyků.</span><span class="sxs-lookup"><span data-stu-id="1b25c-222">These code examples use .NET, but hello concepts are similar for other languages.</span></span>

> [!NOTE]
> <span data-ttu-id="1b25c-223">Azure AD ověřovací token platnost vyprší po jedné hodině.</span><span class="sxs-lookup"><span data-stu-id="1b25c-223">An Azure AD authentication token expires after one hour.</span></span> <span data-ttu-id="1b25c-224">Při použití dlohotrvající **BatchClient** objektu, doporučujeme vám, že načtení tokenu z ADAL na každý požadavek tooensure máte vždy platný token.</span><span class="sxs-lookup"><span data-stu-id="1b25c-224">When using a long-lived **BatchClient** object, we recommend that you retrieve a token from ADAL on every request tooensure you always have a valid token.</span></span> 
>
>
> <span data-ttu-id="1b25c-225">tooachieve, v rozhraní .NET, napíše, aby metoda, načítá hello tokenu z Azure AD a předat tuto metodu tooa **BatchTokenCredentials** objektu jako delegáta.</span><span class="sxs-lookup"><span data-stu-id="1b25c-225">tooachieve this in .NET, write a method that retrieves hello token from Azure AD and pass that method tooa **BatchTokenCredentials** object as a delegate.</span></span> <span data-ttu-id="1b25c-226">Hello delegáta metoda je volána v každé žádosti toohello Batch služby tooensure poskytovaná platný token.</span><span class="sxs-lookup"><span data-stu-id="1b25c-226">hello delegate method is called on every request toohello Batch service tooensure that a valid token is provided.</span></span> <span data-ttu-id="1b25c-227">Ve výchozím nastavení ADAL ukládá do mezipaměti tokenů, aby nový token se načítají z Azure AD pouze v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="1b25c-227">By default ADAL caches tokens, so a new token is retrieved from Azure AD only when necessary.</span></span> <span data-ttu-id="1b25c-228">Další informace o tokenů ve službě Azure AD najdete v tématu [scénáře ověřování pro Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="1b25c-228">For more information about tokens in Azure AD, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a><span data-ttu-id="1b25c-229">Příklad kódu: používání služby Azure AD integrované ověřování pomocí rozhraní Batch .NET</span><span class="sxs-lookup"><span data-stu-id="1b25c-229">Code example: Using Azure AD integrated authentication with Batch .NET</span></span>

<span data-ttu-id="1b25c-230">tooauthenticate pomocí integrovaného ověřování z Batch .NET, odkaz hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) balíčku a hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="1b25c-230">tooauthenticate with integrated authentication from Batch .NET, reference hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="1b25c-231">Zahrnout hello následující `using` příkazů v kódu:</span><span class="sxs-lookup"><span data-stu-id="1b25c-231">Include hello following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="1b25c-232">Referenční dokumentace hello koncového bodu Azure AD ve vašem kódu, včetně hello klienta ID.</span><span class="sxs-lookup"><span data-stu-id="1b25c-232">Reference hello Azure AD endpoint in your code, including hello tenant ID.</span></span> <span data-ttu-id="1b25c-233">tooretrieve hello ID klienta, postupujte podle kroků hello uvedených v [získání ID tenanta hello vaší služby Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="1b25c-233">tooretrieve hello tenant ID, follow hello steps outlined in [Get hello tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="1b25c-234">Referenční hello koncový bod prostředků služby Batch:</span><span class="sxs-lookup"><span data-stu-id="1b25c-234">Reference hello Batch service resource endpoint:</span></span>

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="1b25c-235">Referenční vašeho účtu Batch:</span><span class="sxs-lookup"><span data-stu-id="1b25c-235">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="1b25c-236">Zadejte ID aplikace hello (ID klienta) pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1b25c-236">Specify hello application ID (client ID) for your application.</span></span> <span data-ttu-id="1b25c-237">ID aplikace Hello je k dispozici z vaší aplikace registrace v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="1b25c-237">hello application ID is available from your app registration in hello Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="1b25c-238">Taky zkopírujte identifikátor URI, který jste zadali během procesu registrace hello hello přesměrování.</span><span class="sxs-lookup"><span data-stu-id="1b25c-238">Also copy hello redirect URI that you specified during hello registration process.</span></span> <span data-ttu-id="1b25c-239">Hello přesměrování URI zadaný v kódu musí odpovídat hello přesměrování identifikátor URI, který jste zadali při registraci aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="1b25c-239">hello redirect URI specified in your code must match hello redirect URI that you provided when you registered hello application:</span></span>

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

<span data-ttu-id="1b25c-240">Zápisu zpětného volání metoda tooacquire hello ověřovací token ze služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1b25c-240">Write a callback method tooacquire hello authentication token from Azure AD.</span></span> <span data-ttu-id="1b25c-241">Hello **GetAuthenticationTokenAsync** metoda zpětného volání, které jsou tady uvedené volání ADAL tooauthenticate uživatel, který komunikuje s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="1b25c-241">hello **GetAuthenticationTokenAsync** callback method shown here calls ADAL tooauthenticate a user who is interacting with hello application.</span></span> <span data-ttu-id="1b25c-242">Hello **AcquireTokenAsync** metoda zadaný pomocí ADAL hello vyzve uživatele k zadání přihlašovacích údajů a aplikace hello pokračuje jednou hello uživatele poskytuje jim (Pokud již v mezipaměti přihlašovacích údajů):</span><span class="sxs-lookup"><span data-stu-id="1b25c-242">hello **AcquireTokenAsync** method provided by ADAL prompts hello user for their credentials, and hello application proceeds once hello user provides them (unless it has already cached credentials):</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire hello authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

<span data-ttu-id="1b25c-243">Vytvořit **BatchTokenCredentials** objekt, který přebírá hello delegáta jako parametr.</span><span class="sxs-lookup"><span data-stu-id="1b25c-243">Construct a **BatchTokenCredentials** object that takes hello delegate as a parameter.</span></span> <span data-ttu-id="1b25c-244">Použít tyto přihlašovací údaje tooopen **BatchClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="1b25c-244">Use those credentials tooopen a **BatchClient** object.</span></span> <span data-ttu-id="1b25c-245">Můžete ji použít **BatchClient** objekt pro následující operace proti hello služby Batch:</span><span class="sxs-lookup"><span data-stu-id="1b25c-245">You can use that **BatchClient** object for subsequent operations against hello Batch service:</span></span>

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a><span data-ttu-id="1b25c-246">Příklad kódu: objektu služby Azure AD pomocí rozhraní Batch .NET</span><span class="sxs-lookup"><span data-stu-id="1b25c-246">Code example: Using an Azure AD service principal with Batch .NET</span></span>

<span data-ttu-id="1b25c-247">tooauthenticate pomocí objektu služby z Batch .NET, odkaz hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) balíčku a hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="1b25c-247">tooauthenticate with a service principal from Batch .NET, reference hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="1b25c-248">Zahrnout hello následující `using` příkazů v kódu:</span><span class="sxs-lookup"><span data-stu-id="1b25c-248">Include hello following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="1b25c-249">Referenční dokumentace hello koncového bodu Azure AD ve vašem kódu, včetně hello klienta ID.</span><span class="sxs-lookup"><span data-stu-id="1b25c-249">Reference hello Azure AD endpoint in your code, including hello tenant ID.</span></span> <span data-ttu-id="1b25c-250">Pokud používáte hlavní název služby, je nutné zadat koncový bod konkrétního klienta.</span><span class="sxs-lookup"><span data-stu-id="1b25c-250">When using a service principal, you must provide a tenant-specific endpoint.</span></span> <span data-ttu-id="1b25c-251">tooretrieve hello ID klienta, postupujte podle kroků hello uvedených v [získání ID tenanta hello vaší služby Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="1b25c-251">tooretrieve hello tenant ID, follow hello steps outlined in [Get hello tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="1b25c-252">Referenční hello koncový bod prostředků služby Batch:</span><span class="sxs-lookup"><span data-stu-id="1b25c-252">Reference hello Batch service resource endpoint:</span></span>  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="1b25c-253">Referenční vašeho účtu Batch:</span><span class="sxs-lookup"><span data-stu-id="1b25c-253">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="1b25c-254">Zadejte ID aplikace hello (ID klienta) pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1b25c-254">Specify hello application ID (client ID) for your application.</span></span> <span data-ttu-id="1b25c-255">ID aplikace Hello je k dispozici z vaší aplikace registrace v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="1b25c-255">hello application ID is available from your app registration in hello Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="1b25c-256">Zadejte hello tajný klíč, který jste zkopírovali ze hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="1b25c-256">Specify hello secret key that you copied from hello Azure portal:</span></span>

```csharp
private const string ClientKey = "<secret-key>";
```

<span data-ttu-id="1b25c-257">Zápisu zpětného volání metoda tooacquire hello ověřovací token ze služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1b25c-257">Write a callback method tooacquire hello authentication token from Azure AD.</span></span> <span data-ttu-id="1b25c-258">Hello **GetAuthenticationTokenAsync** metoda zpětného volání, které jsou tady uvedené volání ADAL pro bezobslužné ověřování:</span><span class="sxs-lookup"><span data-stu-id="1b25c-258">hello **GetAuthenticationTokenAsync** callback method shown here calls ADAL for unattended authentication:</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

<span data-ttu-id="1b25c-259">Vytvořit **BatchTokenCredentials** objekt, který přebírá hello delegáta jako parametr.</span><span class="sxs-lookup"><span data-stu-id="1b25c-259">Construct a **BatchTokenCredentials** object that takes hello delegate as a parameter.</span></span> <span data-ttu-id="1b25c-260">Použít tyto přihlašovací údaje tooopen **BatchClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="1b25c-260">Use those credentials tooopen a **BatchClient** object.</span></span> <span data-ttu-id="1b25c-261">Můžete jej pak použít **BatchClient** objekt pro následující operace proti hello služby Batch:</span><span class="sxs-lookup"><span data-stu-id="1b25c-261">You can then use that **BatchClient** object for subsequent operations against hello Batch service:</span></span>

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="1b25c-262">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b25c-262">Next steps</span></span>

<span data-ttu-id="1b25c-263">toolearn Další informace o službě Azure AD, najdete v části hello [Azure Active Directory dokumentaci](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="1b25c-263">toolearn more about Azure AD, see hello [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="1b25c-264">Podrobné příklady znázorňující, jak jsou k dispozici v hello toouse ADAL [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=active-directory) knihovny.</span><span class="sxs-lookup"><span data-stu-id="1b25c-264">In-depth examples showing how toouse ADAL are available in hello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="1b25c-265">toolearn více o objektech služby najdete v části [aplikace a služby hlavní objekty ve službě Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="1b25c-265">toolearn more about service principals, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span> <span data-ttu-id="1b25c-266">hello toocreate službu objektu zabezpečení pomocí portálu Azure najdete [pomocí aplikace portálu toocreate Active Directory a objektu služby, které mají přístup k prostředkům](../resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1b25c-266">toocreate a service principal using hello Azure portal, see [Use portal toocreate Active Directory application and service principal that can access resources](../resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="1b25c-267">Hlavní název služby můžete vytvořit také pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1b25c-267">You can also create a service principal with PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="1b25c-268">tooauthenticate Batch správy aplikací pomocí služby Azure AD, najdete v části [řešení pro správu Batch ověření pomocí služby Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="1b25c-268">tooauthenticate Batch Management applications using Azure AD, see [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span> 

[aad_about]: ../active-directory/active-directory-whatis.md "Co je Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Scénáře ověřování pro Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrace aplikací s Azure Active Directory"
[azure_portal]: http://portal.azure.com
