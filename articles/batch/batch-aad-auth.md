---
title: "Pomocí Azure Active Directory k ověření řešení služby Azure Batch | Microsoft Docs"
description: "Batch podporuje Azure AD pro ověřování ze služby Batch."
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
ms.openlocfilehash: 9c03bde919c46cd301229255c0b12ee69dda6f78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a><span data-ttu-id="e72f3-103">Ověření řešení služby Batch se službou Active Directory</span><span class="sxs-lookup"><span data-stu-id="e72f3-103">Authenticate Batch service solutions with Active Directory</span></span>

<span data-ttu-id="e72f3-104">Azure Batch podporuje ověřování s [Azure Active Directory] [ aad_about] (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e72f3-104">Azure Batch supports authentication with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="e72f3-105">Azure AD je víceklientské cloudový adresář společnosti Microsoft a služba identity management.</span><span class="sxs-lookup"><span data-stu-id="e72f3-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="e72f3-106">Azure samotné používá Azure AD ověřit jeho zákazníků, správci služeb a organizační uživatele.</span><span class="sxs-lookup"><span data-stu-id="e72f3-106">Azure itself uses Azure AD to authenticate its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="e72f3-107">Pokud používáte ověřování Azure AD pomocí služby Azure Batch, můžete ověřovat v jednom ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="e72f3-107">When using Azure AD authentication with Azure Batch, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="e72f3-108">Pomocí **integrované ověřování** k ověření uživatele, který komunikuje s aplikací.</span><span class="sxs-lookup"><span data-stu-id="e72f3-108">By using **integrated authentication** to authenticate a user that is interacting with the application.</span></span> <span data-ttu-id="e72f3-109">Aplikace prostřednictvím integrovaného ověřování přihlašovacích údajů uživatele shromáždí a použije tyto přihlašovací údaje k ověření přístupu k prostředkům Batch.</span><span class="sxs-lookup"><span data-stu-id="e72f3-109">An application using integrated authentication gathers a user's credentials and uses those credentials to authenticate access to Batch resources.</span></span>
- <span data-ttu-id="e72f3-110">Pomocí **instanční objekt** k ověření bezobslužné aplikace.</span><span class="sxs-lookup"><span data-stu-id="e72f3-110">By using a **service principal** to authenticate an unattended application.</span></span> <span data-ttu-id="e72f3-111">Objekt služby definuje zásady a oprávnění pro aplikaci, aby mohl představovat aplikace při přístupu k prostředkům v době běhu.</span><span class="sxs-lookup"><span data-stu-id="e72f3-111">A service principal defines the policy and permissions for an application in order to represent the application when accessing resources at runtime.</span></span>

<span data-ttu-id="e72f3-112">Další informace o Azure AD, najdete v článku [Azure Active Directory dokumentaci](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="e72f3-112">To learn more about Azure AD, see the [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span>

## <a name="authentication-and-pool-allocation-mode"></a><span data-ttu-id="e72f3-113">Režim ověřování a fondu přidělení</span><span class="sxs-lookup"><span data-stu-id="e72f3-113">Authentication and pool allocation mode</span></span>

<span data-ttu-id="e72f3-114">Při vytváření účtu Batch, můžete zadat, kde by měla být pro tento účet vytvořit fondy přidělená.</span><span class="sxs-lookup"><span data-stu-id="e72f3-114">When you create a Batch account, you can specify where pools created for that account should be allocated.</span></span> <span data-ttu-id="e72f3-115">Můžete přidělit fondy v předplatném služby Batch výchozí nebo v předplatném uživatele.</span><span class="sxs-lookup"><span data-stu-id="e72f3-115">You can choose to allocate pools either in the default Batch service subscription or in a user subscription.</span></span> <span data-ttu-id="e72f3-116">Svou volbu ovlivňuje způsob ověření přístupu k prostředkům v daném účtu.</span><span class="sxs-lookup"><span data-stu-id="e72f3-116">Your choice affects how you authenticate access to resources in that account.</span></span>

- <span data-ttu-id="e72f3-117">**Předplatné služby batch**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-117">**Batch service subscription**.</span></span> <span data-ttu-id="e72f3-118">Ve výchozím nastavení jsou fondy Batch přidělené v předplatné služby Batch.</span><span class="sxs-lookup"><span data-stu-id="e72f3-118">By default, Batch pools are allocated in a Batch service subscription.</span></span> <span data-ttu-id="e72f3-119">Pokud zvolíte tuto možnost, můžete ověřovat přístup k prostředkům v daném účtu buď pomocí [sdílený klíč](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) nebo s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e72f3-119">If you choose this option, you can authenticate access to resources in that account either with [Shared Key](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) or with Azure AD.</span></span>
- <span data-ttu-id="e72f3-120">**Předplatné uživatele.**</span><span class="sxs-lookup"><span data-stu-id="e72f3-120">**User subscription.**</span></span> <span data-ttu-id="e72f3-121">Můžete přidělit fondy Batch v předplatném uživatele, který určíte.</span><span class="sxs-lookup"><span data-stu-id="e72f3-121">You can choose to allocate Batch pools in a user subscription that you specify.</span></span> <span data-ttu-id="e72f3-122">Pokud zvolíte tuto možnost, je třeba ověřit pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e72f3-122">If you choose this option, you must authenticate with Azure AD.</span></span>

## <a name="endpoints-for-authentication"></a><span data-ttu-id="e72f3-123">Koncové body pro ověřování</span><span class="sxs-lookup"><span data-stu-id="e72f3-123">Endpoints for authentication</span></span>

<span data-ttu-id="e72f3-124">K ověření aplikace Batch s Azure AD, budete muset obsahovat některé známé koncové body v kódu.</span><span class="sxs-lookup"><span data-stu-id="e72f3-124">To authenticate Batch applications with Azure AD, you need to include some well-known endpoints in your code.</span></span>

### <a name="azure-ad-endpoint"></a><span data-ttu-id="e72f3-125">Koncový bod Azure AD</span><span class="sxs-lookup"><span data-stu-id="e72f3-125">Azure AD endpoint</span></span>

<span data-ttu-id="e72f3-126">Základní koncový bod autority Azure AD:</span><span class="sxs-lookup"><span data-stu-id="e72f3-126">The base Azure AD authority endpoint is:</span></span>

`https://login.microsoftonline.com/`

<span data-ttu-id="e72f3-127">K ověření s Azure AD, použijte tento koncový bod společně s ID klienta (ID adresáře).</span><span class="sxs-lookup"><span data-stu-id="e72f3-127">To authenticate with Azure AD, you use this endpoint together with the tenant ID (directory ID).</span></span> <span data-ttu-id="e72f3-128">ID klienta identifikuje klienta Azure AD pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="e72f3-128">The tenant ID identifies the Azure AD tenant to use for authentication.</span></span> <span data-ttu-id="e72f3-129">Pokud chcete načíst ID klienta, postupujte podle kroků uvedených v [získání ID tenanta pro Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="e72f3-129">To retrieve the tenant ID, follow the steps outlined in [Get the tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> <span data-ttu-id="e72f3-130">Koncový bod konkrétního klienta je potřeba při ověření pomocí objektu služby.</span><span class="sxs-lookup"><span data-stu-id="e72f3-130">The tenant-specific endpoint is required when you authenticate using a service principal.</span></span> 
> 
> <span data-ttu-id="e72f3-131">Koncový bod konkrétního klienta je volitelný při ověřování pomocí integrovaného ověřování, ale doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="e72f3-131">The tenant-specific endpoint is optional when you authenticate using integrated authentication, but recommended.</span></span> <span data-ttu-id="e72f3-132">Však můžete také použít běžné koncového bodu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e72f3-132">However, you can also use the Azure AD common endpoint.</span></span> <span data-ttu-id="e72f3-133">Běžné koncový bod poskytuje obecné pověření shromažďování rozhraní, když není k dispozici konkrétní klienta.</span><span class="sxs-lookup"><span data-stu-id="e72f3-133">The common endpoint provides a generic credential gathering interface when a specific tenant is not provided.</span></span> <span data-ttu-id="e72f3-134">Běžné koncový bod je `https://login.microsoftonline.com/common`.</span><span class="sxs-lookup"><span data-stu-id="e72f3-134">The common endpoint is `https://login.microsoftonline.com/common`.</span></span>
>
>

<span data-ttu-id="e72f3-135">Další informace o koncové body Azure AD najdete v tématu [scénáře ověřování pro Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="e72f3-135">For more information about Azure AD endpoints, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>

### <a name="batch-resource-endpoint"></a><span data-ttu-id="e72f3-136">Koncový bod prostředků batch</span><span class="sxs-lookup"><span data-stu-id="e72f3-136">Batch resource endpoint</span></span>

<span data-ttu-id="e72f3-137">Použití **koncový bod prostředků Azure Batch** získat token pro ověřování požadavků na službu Batch:</span><span class="sxs-lookup"><span data-stu-id="e72f3-137">Use the **Azure Batch resource endpoint** to acquire a token for authenticating requests to the Batch service:</span></span>

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a><span data-ttu-id="e72f3-138">Registrace aplikace pomocí klienta</span><span class="sxs-lookup"><span data-stu-id="e72f3-138">Register your application with a tenant</span></span>

<span data-ttu-id="e72f3-139">Prvním krokem při používání služby Azure AD k ověření je registrace vaší aplikace v klient služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e72f3-139">The first step in using Azure AD to authenticate is registering your application in an Azure AD tenant.</span></span> <span data-ttu-id="e72f3-140">Registrace aplikace umožňuje volat Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="e72f3-140">Registering your application enables you to call the Azure [Active Directory Authentication Library][aad_adal] (ADAL) from your code.</span></span> <span data-ttu-id="e72f3-141">Knihovnu ADAL poskytuje rozhraní API pro ověřování s Azure AD z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e72f3-141">The ADAL provides an API for authenticating with Azure AD from your application.</span></span> <span data-ttu-id="e72f3-142">Registrace aplikace je vyžadován plánujete použít integrované ověřování nebo hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="e72f3-142">Registering your application is required whether you plan to use integrated authentication or a service principal.</span></span>

<span data-ttu-id="e72f3-143">Při registraci vaší aplikace, můžete zadat informace o vaší aplikaci do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e72f3-143">When you register your application, you supply information about your application to Azure AD.</span></span> <span data-ttu-id="e72f3-144">Potom Azure AD poskytuje ID aplikace, které použijete k aplikaci přidružit Azure AD v době běhu.</span><span class="sxs-lookup"><span data-stu-id="e72f3-144">Azure AD then provides an application ID that you use to associate your application with Azure AD at runtime.</span></span> <span data-ttu-id="e72f3-145">Další informace o ID aplikace, najdete v části [aplikace a služby hlavní objekty ve službě Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="e72f3-145">To learn more about the application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="e72f3-146">Pro registraci aplikace Batch, postupujte podle kroků v [přidání aplikace](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) kapitoly [integrace aplikací s Azure Active Directory][aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="e72f3-146">To register your Batch application, follow the steps in the [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="e72f3-147">Když si zaregistrujete aplikaci jako nativní aplikaci, můžete zadat libovolný platný identifikátor URI pro **identifikátor URI pro přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-147">If you register your application as a Native Application, you can specify any valid URI for the **Redirect URI**.</span></span> <span data-ttu-id="e72f3-148">Nemusí být skutečné koncový bod.</span><span class="sxs-lookup"><span data-stu-id="e72f3-148">It does not need to be a real endpoint.</span></span>

<span data-ttu-id="e72f3-149">Poté, co jste registrováni vaší aplikace, se zobrazí ID aplikace:</span><span class="sxs-lookup"><span data-stu-id="e72f3-149">After you've registered your application, you'll see the application ID:</span></span>

![Registrace aplikace Batch pomocí Azure AD](./media/batch-aad-auth/app-registration-data-plane.png)

<span data-ttu-id="e72f3-151">Další informace o registraci aplikace s Azure AD najdete v tématu [scénáře ověřování pro Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="e72f3-151">For more information about registering an application with Azure AD, see [Authentication Scenarios for Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span></span>

## <a name="get-the-tenant-id-for-your-active-directory"></a><span data-ttu-id="e72f3-152">Získání ID klienta pro vaši službu Active Directory</span><span class="sxs-lookup"><span data-stu-id="e72f3-152">Get the tenant ID for your Active Directory</span></span>

<span data-ttu-id="e72f3-153">ID klienta identifikuje klienta Azure AD, která poskytuje služby ověřování do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e72f3-153">The tenant ID identifies the Azure AD tenant that provides authentication services to your application.</span></span> <span data-ttu-id="e72f3-154">Chcete-li získat ID klienta, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e72f3-154">To get the tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="e72f3-155">Na portálu Azure vyberte vaší služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e72f3-155">In the Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="e72f3-156">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-156">Click **Properties**.</span></span>
3. <span data-ttu-id="e72f3-157">Zkopírujte GUID hodnota zadaná pro ID adresáře.</span><span class="sxs-lookup"><span data-stu-id="e72f3-157">Copy the GUID value provided for the directory ID.</span></span> <span data-ttu-id="e72f3-158">Tato hodnota se označuje taky jako ID klienta.</span><span class="sxs-lookup"><span data-stu-id="e72f3-158">This value is also called the tenant ID.</span></span>

![Zkopírujte ID adresáře sady](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a><span data-ttu-id="e72f3-160">Použít integrované ověřování</span><span class="sxs-lookup"><span data-stu-id="e72f3-160">Use integrated authentication</span></span>

<span data-ttu-id="e72f3-161">K ověřování pro integrované ověřování, budete muset udělit oprávnění vaší aplikace pro připojení k rozhraní API služby Batch.</span><span class="sxs-lookup"><span data-stu-id="e72f3-161">To authenticate with integrated authentication, you need to grant your application permissions to connect to the Batch service API.</span></span> <span data-ttu-id="e72f3-162">Tento krok povoluje aplikace k ověření volání do rozhraní API služby Batch s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e72f3-162">This step enables your application to authenticate calls to the Batch service API with Azure AD.</span></span>

<span data-ttu-id="e72f3-163">Jakmile jste [zaregistrovat aplikaci](#register-your-application-with-an-azure-ad-tenant), postupujte podle těchto kroků na portálu Azure jí udělit přístup do služby Batch:</span><span class="sxs-lookup"><span data-stu-id="e72f3-163">Once you've [registered your application](#register-your-application-with-an-azure-ad-tenant), follow these steps in the Azure portal to grant it access to the Batch service:</span></span>

1. <span data-ttu-id="e72f3-164">V levém navigačním podokně portálu Azure, zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-164">In the left-hand navigation pane of the Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="e72f3-165">Vyhledejte název vaší aplikace v seznamu aplikace registrace:</span><span class="sxs-lookup"><span data-stu-id="e72f3-165">Search for the name of your application in the list of app registrations:</span></span>

    ![Vyhledávání pro název aplikace](./media/batch-aad-auth/search-app-registration.png)

3. <span data-ttu-id="e72f3-167">Otevřete **nastavení** okno pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e72f3-167">Open the **Settings** blade for your application.</span></span> <span data-ttu-id="e72f3-168">V **přístup pomocí rozhraní API** vyberte **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-168">In the **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="e72f3-169">V **požadovaná oprávnění** okně klikněte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e72f3-169">In the **Required permissions** blade, click the **Add** button.</span></span>
5. <span data-ttu-id="e72f3-170">V kroku 1 vyhledejte rozhraní API služby Batch.</span><span class="sxs-lookup"><span data-stu-id="e72f3-170">In step 1, search for the Batch API.</span></span> <span data-ttu-id="e72f3-171">Hledejte každý z těchto řetězců, dokud nenajdete rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="e72f3-171">Search for each of these strings until you find the API:</span></span>
    1. <span data-ttu-id="e72f3-172">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-172">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="e72f3-173">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-173">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="e72f3-174">Novější tenanti služby Azure AD mohou používat tento název.</span><span class="sxs-lookup"><span data-stu-id="e72f3-174">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="e72f3-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** je ID rozhraní API služby Batch.</span><span class="sxs-lookup"><span data-stu-id="e72f3-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** is the ID for the Batch API.</span></span> 
6. <span data-ttu-id="e72f3-176">Jakmile zjistíte, rozhraní API služby Batch, vyberte ho a klikněte na **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e72f3-176">Once you find the Batch API, select it and click the **Select** button.</span></span>
6. <span data-ttu-id="e72f3-177">V kroku 2, zaškrtněte políčko vedle **služby Azure Batch přístup** a klikněte na tlačítko **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e72f3-177">In step 2, select the check box next to **Access Azure Batch Service** and click the **Select** button.</span></span>
7. <span data-ttu-id="e72f3-178">Klikněte **provádí** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e72f3-178">Click the **Done** button.</span></span>

<span data-ttu-id="e72f3-179">**Požadovaných oprávnění** okno teď zobrazuje, vaše aplikace Azure AD má přístup k ADAL a dávky služby rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e72f3-179">The **Required Permissions** blade now shows that your Azure AD application has access to both ADAL and the Batch service API.</span></span> <span data-ttu-id="e72f3-180">Jsou udělena oprávnění pro knihovnu ADAL automaticky při první registraci vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e72f3-180">Permissions are granted to ADAL automatically when you first register your app with Azure AD.</span></span>

![Rozhraní API udělení oprávnění](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a><span data-ttu-id="e72f3-182">Použít hlavní název služby</span><span class="sxs-lookup"><span data-stu-id="e72f3-182">Use a service principal</span></span> 

<span data-ttu-id="e72f3-183">K ověření aplikace, která spouští bezobslužné, použijte objekt služby.</span><span class="sxs-lookup"><span data-stu-id="e72f3-183">To authenticate an application that runs unattended, you use a service principal.</span></span> <span data-ttu-id="e72f3-184">Poté, co jste registrováni vaší aplikace, pomocí těchto kroků na portálu Azure ke konfiguraci hlavní název služby:</span><span class="sxs-lookup"><span data-stu-id="e72f3-184">After you've registered your application, follow these steps in the Azure portal to configure a service principal:</span></span>

1. <span data-ttu-id="e72f3-185">Požádat o tajný klíč pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e72f3-185">Request a secret key for your application.</span></span>
2. <span data-ttu-id="e72f3-186">Přiřazení role RBAC do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e72f3-186">Assign an RBAC role to your application.</span></span>

### <a name="request-a-secret-key-for-your-application"></a><span data-ttu-id="e72f3-187">Žádost o tajný klíč pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="e72f3-187">Request a secret key for your application</span></span>

<span data-ttu-id="e72f3-188">Když se aplikace ověřuje s hlavní službou, odešle do služby Azure AD ID aplikace a tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="e72f3-188">When your application authenticates with a service principal, it sends both the application ID and a secret key to Azure AD.</span></span> <span data-ttu-id="e72f3-189">Budete muset vytvořit a zkopírovat tajný klíč pro použití v kódu.</span><span class="sxs-lookup"><span data-stu-id="e72f3-189">You'll need to create and copy the secret key to use from your code.</span></span>

<span data-ttu-id="e72f3-190">Pomocí těchto kroků na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="e72f3-190">Follow these steps in the Azure portal:</span></span>

1. <span data-ttu-id="e72f3-191">V levém navigačním podokně portálu Azure, zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-191">In the left-hand navigation pane of the Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="e72f3-192">Vyhledejte název vaší aplikace v seznamu aplikace registrací.</span><span class="sxs-lookup"><span data-stu-id="e72f3-192">Search for the name of your application in the list of app registrations.</span></span>
3. <span data-ttu-id="e72f3-193">Zobrazení **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="e72f3-193">Display the **Settings** blade.</span></span> <span data-ttu-id="e72f3-194">V **přístup pomocí rozhraní API** vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-194">In the **API Access** section, select **Keys**.</span></span>
4. <span data-ttu-id="e72f3-195">Vytvoření klíče, zadejte popis pro klíč.</span><span class="sxs-lookup"><span data-stu-id="e72f3-195">To create a key, enter a description for the key.</span></span> <span data-ttu-id="e72f3-196">Pak vyberte dobu trvání pro klíč jeden nebo dva roky.</span><span class="sxs-lookup"><span data-stu-id="e72f3-196">Then select a duration for the key of either one or two years.</span></span> 
5. <span data-ttu-id="e72f3-197">Klikněte **Uložit** tlačítko Vytvořit a zobrazit klíč.</span><span class="sxs-lookup"><span data-stu-id="e72f3-197">Click the **Save** button to create and display the key.</span></span> <span data-ttu-id="e72f3-198">Zkopírujte hodnotu klíče na bezpečné místo, nebudou mít přístup k ho znovu po opuštění okna.</span><span class="sxs-lookup"><span data-stu-id="e72f3-198">Copy the key value to a safe place, as you won't be able to access it again after you leave the blade.</span></span> 

    ![Vytvoření tajný klíč](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-to-your-application"></a><span data-ttu-id="e72f3-200">Přiřazení role RBAC do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="e72f3-200">Assign an RBAC role to your application</span></span>

<span data-ttu-id="e72f3-201">K ověření s hlavní službou, musíte přiřadit role RBAC do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e72f3-201">To authenticate with a service principal, you need to assign an RBAC role to your application.</span></span> <span data-ttu-id="e72f3-202">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="e72f3-202">Follow these steps:</span></span>

1. <span data-ttu-id="e72f3-203">Na portálu Azure přejděte k účtu Batch používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="e72f3-203">In the Azure portal, navigate to the Batch account used by your application.</span></span>
2. <span data-ttu-id="e72f3-204">V **nastavení** okno účtu Batch, vyberte **řízení přístupu (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-204">In the **Settings** blade for the Batch account, select **Access Control (IAM)**.</span></span>
3. <span data-ttu-id="e72f3-205">Klikněte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e72f3-205">Click the **Add** button.</span></span> 
4. <span data-ttu-id="e72f3-206">Z **Role** rozevíracího seznamu, vyberte buď _Přispěvatel_ nebo _čtečky_ role pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e72f3-206">From the **Role** drop-down, choose either the _Contributor_ or _Reader_ role for your application.</span></span> <span data-ttu-id="e72f3-207">Další informace o těchto rolí najdete v tématu [Začínáme s řízením přístupu na základě rolí na portálu Azure](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="e72f3-207">For more information on these roles, see [Get started with Role-Based Access Control in the Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>  
5. <span data-ttu-id="e72f3-208">V **vyberte** pole, zadejte název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e72f3-208">In the **Select** field, enter the name of your application.</span></span> <span data-ttu-id="e72f3-209">Vybrat aplikaci ze seznamu a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-209">Select your application from the list, and click **Save**.</span></span>

<span data-ttu-id="e72f3-210">Aplikace by se měla zobrazit v nastavení řízení přístupu s přiřazenou RBAC roli.</span><span class="sxs-lookup"><span data-stu-id="e72f3-210">Your application should now appear in your access control settings with an RBAC role assigned.</span></span> 

![Přiřazení role RBAC do vaší aplikace](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a><span data-ttu-id="e72f3-212">Získání ID tenanta pro Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e72f3-212">Get the tenant ID for your Azure Active Directory</span></span>

<span data-ttu-id="e72f3-213">ID klienta identifikuje klienta Azure AD, která poskytuje služby ověřování do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e72f3-213">The tenant ID identifies the Azure AD tenant that provides authentication services to your application.</span></span> <span data-ttu-id="e72f3-214">Chcete-li získat ID klienta, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e72f3-214">To get the tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="e72f3-215">Na portálu Azure vyberte vaší služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e72f3-215">In the Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="e72f3-216">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e72f3-216">Click **Properties**.</span></span>
3. <span data-ttu-id="e72f3-217">Zkopírujte GUID hodnota zadaná pro ID adresáře.</span><span class="sxs-lookup"><span data-stu-id="e72f3-217">Copy the GUID value provided for the directory ID.</span></span> <span data-ttu-id="e72f3-218">Tato hodnota se označuje taky jako ID klienta.</span><span class="sxs-lookup"><span data-stu-id="e72f3-218">This value is also called the tenant ID.</span></span>

![Zkopírujte ID adresáře sady](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a><span data-ttu-id="e72f3-220">Příklady kódu</span><span class="sxs-lookup"><span data-stu-id="e72f3-220">Code examples</span></span>

<span data-ttu-id="e72f3-221">Příklady kódu v této části ukazují, jak k ověření s pomocí integrovaného ověřování služby Azure AD a instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="e72f3-221">The code examples in this section show how to authenticate with Azure AD using integrated authentication and with a service principal.</span></span> <span data-ttu-id="e72f3-222">Tyto příklady kódu pomocí rozhraní .NET, ale koncepty jsou podobné jako u ostatních jazyků.</span><span class="sxs-lookup"><span data-stu-id="e72f3-222">These code examples use .NET, but the concepts are similar for other languages.</span></span>

> [!NOTE]
> <span data-ttu-id="e72f3-223">Azure AD ověřovací token platnost vyprší po jedné hodině.</span><span class="sxs-lookup"><span data-stu-id="e72f3-223">An Azure AD authentication token expires after one hour.</span></span> <span data-ttu-id="e72f3-224">Při použití dlohotrvající **BatchClient** objektu, doporučujeme, abyste u každého požadavku vždy máte platný token načtení tokenu z ADAL.</span><span class="sxs-lookup"><span data-stu-id="e72f3-224">When using a long-lived **BatchClient** object, we recommend that you retrieve a token from ADAL on every request to ensure you always have a valid token.</span></span> 
>
>
> <span data-ttu-id="e72f3-225">To můžete udělat v rozhraní .NET, napíše metoda, která načte tokenu z Azure AD a předat tuto metodu **BatchTokenCredentials** objektu jako delegáta.</span><span class="sxs-lookup"><span data-stu-id="e72f3-225">To achieve this in .NET, write a method that retrieves the token from Azure AD and pass that method to a **BatchTokenCredentials** object as a delegate.</span></span> <span data-ttu-id="e72f3-226">U každého požadavku na službu Batch zajistit, že je k dispozici platný token je volání metody delegáta.</span><span class="sxs-lookup"><span data-stu-id="e72f3-226">The delegate method is called on every request to the Batch service to ensure that a valid token is provided.</span></span> <span data-ttu-id="e72f3-227">Ve výchozím nastavení ADAL ukládá do mezipaměti tokenů, aby nový token se načítají z Azure AD pouze v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="e72f3-227">By default ADAL caches tokens, so a new token is retrieved from Azure AD only when necessary.</span></span> <span data-ttu-id="e72f3-228">Další informace o tokenů ve službě Azure AD najdete v tématu [scénáře ověřování pro Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="e72f3-228">For more information about tokens in Azure AD, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a><span data-ttu-id="e72f3-229">Příklad kódu: používání služby Azure AD integrované ověřování pomocí rozhraní Batch .NET</span><span class="sxs-lookup"><span data-stu-id="e72f3-229">Code example: Using Azure AD integrated authentication with Batch .NET</span></span>

<span data-ttu-id="e72f3-230">K ověření pomocí integrovaného ověřování z Batch .NET, odkazovat [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) balíček a [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="e72f3-230">To authenticate with integrated authentication from Batch .NET, reference the [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and the [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="e72f3-231">Patří `using` příkazů v kódu:</span><span class="sxs-lookup"><span data-stu-id="e72f3-231">Include the following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="e72f3-232">Referenční dokumentace Azure AD koncového bodu v kódu, včetně ID klienta.</span><span class="sxs-lookup"><span data-stu-id="e72f3-232">Reference the Azure AD endpoint in your code, including the tenant ID.</span></span> <span data-ttu-id="e72f3-233">Pokud chcete načíst ID klienta, postupujte podle kroků uvedených v [získání ID tenanta pro Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="e72f3-233">To retrieve the tenant ID, follow the steps outlined in [Get the tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="e72f3-234">Reference pro koncový bod prostředků služby Batch.</span><span class="sxs-lookup"><span data-stu-id="e72f3-234">Reference the Batch service resource endpoint:</span></span>

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="e72f3-235">Referenční vašeho účtu Batch:</span><span class="sxs-lookup"><span data-stu-id="e72f3-235">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="e72f3-236">Zadejte ID aplikace (ID klienta) pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e72f3-236">Specify the application ID (client ID) for your application.</span></span> <span data-ttu-id="e72f3-237">ID aplikace je k dispozici z vaší aplikace registrace na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="e72f3-237">The application ID is available from your app registration in the Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="e72f3-238">Taky zkopírujte identifikátor URI, který jste zadali během procesu registrace přesměrování.</span><span class="sxs-lookup"><span data-stu-id="e72f3-238">Also copy the redirect URI that you specified during the registration process.</span></span> <span data-ttu-id="e72f3-239">Přesměrování, které URI zadaný v kódu musí odpovídat přesměrování identifikátor URI, který jste zadali při registraci aplikace:</span><span class="sxs-lookup"><span data-stu-id="e72f3-239">The redirect URI specified in your code must match the redirect URI that you provided when you registered the application:</span></span>

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

<span data-ttu-id="e72f3-240">Zápis metody zpětného volání získat ověřovací token ze služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e72f3-240">Write a callback method to acquire the authentication token from Azure AD.</span></span> <span data-ttu-id="e72f3-241">**GetAuthenticationTokenAsync** metoda zpětného volání, které jsou tady uvedené volání ADAL k ověření uživatele, který komunikuje s aplikací.</span><span class="sxs-lookup"><span data-stu-id="e72f3-241">The **GetAuthenticationTokenAsync** callback method shown here calls ADAL to authenticate a user who is interacting with the application.</span></span> <span data-ttu-id="e72f3-242">**AcquireTokenAsync** metoda poskytované ADAL vyzve uživatele k jejich přihlašovacích údajů a aplikace bude pokračovat po uživatel nezadá (Pokud již v mezipaměti přihlašovacích údajů):</span><span class="sxs-lookup"><span data-stu-id="e72f3-242">The **AcquireTokenAsync** method provided by ADAL prompts the user for their credentials, and the application proceeds once the user provides them (unless it has already cached credentials):</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire the authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

<span data-ttu-id="e72f3-243">Vytvořit **BatchTokenCredentials** objekt, který přebírá delegáta jako parametr.</span><span class="sxs-lookup"><span data-stu-id="e72f3-243">Construct a **BatchTokenCredentials** object that takes the delegate as a parameter.</span></span> <span data-ttu-id="e72f3-244">Použít tyto přihlašovací údaje k otevření **BatchClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="e72f3-244">Use those credentials to open a **BatchClient** object.</span></span> <span data-ttu-id="e72f3-245">Můžete ji použít **BatchClient** objekt pro následující operace proti služba Batch:</span><span class="sxs-lookup"><span data-stu-id="e72f3-245">You can use that **BatchClient** object for subsequent operations against the Batch service:</span></span>

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

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a><span data-ttu-id="e72f3-246">Příklad kódu: objektu služby Azure AD pomocí rozhraní Batch .NET</span><span class="sxs-lookup"><span data-stu-id="e72f3-246">Code example: Using an Azure AD service principal with Batch .NET</span></span>

<span data-ttu-id="e72f3-247">K ověření pomocí objektu služby z Batch .NET, odkazovat [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) balíček a [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="e72f3-247">To authenticate with a service principal from Batch .NET, reference the [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and the [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="e72f3-248">Patří `using` příkazů v kódu:</span><span class="sxs-lookup"><span data-stu-id="e72f3-248">Include the following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="e72f3-249">Referenční dokumentace Azure AD koncového bodu v kódu, včetně ID klienta.</span><span class="sxs-lookup"><span data-stu-id="e72f3-249">Reference the Azure AD endpoint in your code, including the tenant ID.</span></span> <span data-ttu-id="e72f3-250">Pokud používáte hlavní název služby, je nutné zadat koncový bod konkrétního klienta.</span><span class="sxs-lookup"><span data-stu-id="e72f3-250">When using a service principal, you must provide a tenant-specific endpoint.</span></span> <span data-ttu-id="e72f3-251">Pokud chcete načíst ID klienta, postupujte podle kroků uvedených v [získání ID tenanta pro Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="e72f3-251">To retrieve the tenant ID, follow the steps outlined in [Get the tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="e72f3-252">Reference pro koncový bod prostředků služby Batch.</span><span class="sxs-lookup"><span data-stu-id="e72f3-252">Reference the Batch service resource endpoint:</span></span>  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="e72f3-253">Referenční vašeho účtu Batch:</span><span class="sxs-lookup"><span data-stu-id="e72f3-253">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="e72f3-254">Zadejte ID aplikace (ID klienta) pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e72f3-254">Specify the application ID (client ID) for your application.</span></span> <span data-ttu-id="e72f3-255">ID aplikace je k dispozici z vaší aplikace registrace na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="e72f3-255">The application ID is available from your app registration in the Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="e72f3-256">Zadejte tajný klíč, který jste zkopírovali z portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="e72f3-256">Specify the secret key that you copied from the Azure portal:</span></span>

```csharp
private const string ClientKey = "<secret-key>";
```

<span data-ttu-id="e72f3-257">Zápis metody zpětného volání získat ověřovací token ze služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e72f3-257">Write a callback method to acquire the authentication token from Azure AD.</span></span> <span data-ttu-id="e72f3-258">**GetAuthenticationTokenAsync** metoda zpětného volání, které jsou tady uvedené volání ADAL pro bezobslužné ověřování:</span><span class="sxs-lookup"><span data-stu-id="e72f3-258">The **GetAuthenticationTokenAsync** callback method shown here calls ADAL for unattended authentication:</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

<span data-ttu-id="e72f3-259">Vytvořit **BatchTokenCredentials** objekt, který přebírá delegáta jako parametr.</span><span class="sxs-lookup"><span data-stu-id="e72f3-259">Construct a **BatchTokenCredentials** object that takes the delegate as a parameter.</span></span> <span data-ttu-id="e72f3-260">Použít tyto přihlašovací údaje k otevření **BatchClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="e72f3-260">Use those credentials to open a **BatchClient** object.</span></span> <span data-ttu-id="e72f3-261">Můžete jej pak použít **BatchClient** objekt pro následující operace proti služba Batch:</span><span class="sxs-lookup"><span data-stu-id="e72f3-261">You can then use that **BatchClient** object for subsequent operations against the Batch service:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="e72f3-262">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e72f3-262">Next steps</span></span>

<span data-ttu-id="e72f3-263">Další informace o Azure AD, najdete v článku [Azure Active Directory dokumentaci](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="e72f3-263">To learn more about Azure AD, see the [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="e72f3-264">Podrobné příklady znázorňující způsob použití knihovny ADAL jsou k dispozici v [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=active-directory) knihovny.</span><span class="sxs-lookup"><span data-stu-id="e72f3-264">In-depth examples showing how to use ADAL are available in the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="e72f3-265">Další informace o objektech služby najdete v tématu [aplikace a služby hlavní objekty ve službě Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="e72f3-265">To learn more about service principals, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span> <span data-ttu-id="e72f3-266">Pokud chcete vytvořit objekt služby pomocí portálu Azure, najdete v části [použití portálu k vytvoření služby Active Directory objekt zabezpečení aplikací a služeb, který mají přístup k prostředkům](../resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e72f3-266">To create a service principal using the Azure portal, see [Use portal to create Active Directory application and service principal that can access resources](../resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="e72f3-267">Hlavní název služby můžete vytvořit také pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e72f3-267">You can also create a service principal with PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="e72f3-268">K ověření aplikace Batch správy pomocí služby Azure AD, najdete v části [řešení pro správu Batch ověření pomocí služby Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="e72f3-268">To authenticate Batch Management applications using Azure AD, see [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span> 

<span data-ttu-id="e72f3-269">[aad_about]: ../active-directory/active-directory-whatis.md "Co je Azure Active Directory?"</span><span class="sxs-lookup"><span data-stu-id="e72f3-269">[aad_about]: ../active-directory/active-directory-whatis.md "What is Azure Active Directory?"</span></span>
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
<span data-ttu-id="e72f3-270">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Scénáře ověřování pro Azure AD"</span><span class="sxs-lookup"><span data-stu-id="e72f3-270">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentication Scenarios for Azure AD"</span></span>
<span data-ttu-id="e72f3-271">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrace aplikací s Azure Active Directory"</span><span class="sxs-lookup"><span data-stu-id="e72f3-271">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrating Applications with Azure Active Directory"</span></span>
[azure_portal]: http://portal.azure.com
