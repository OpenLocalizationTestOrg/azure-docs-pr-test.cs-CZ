---
title: "Ověřování koncového uživatele: Data Lake Store s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak zajistit ověření koncového uživatele s Data Lake Store pomocí Azure Active Directory"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ec586ecd-1b42-459e-b600-fadbb7b80a9b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: c20f5c39b00992d801909c8e5de292f3c2f12673
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a><span data-ttu-id="0d27b-103">Ověření koncových uživatelů s Data Lake Store pomocí Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d27b-103">End-user authentication with Data Lake Store using Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0d27b-104">Ověřování služba-služba</span><span class="sxs-lookup"><span data-stu-id="0d27b-104">Service-to-service authentication</span></span>](data-lake-store-authenticate-using-active-directory.md)
> * [<span data-ttu-id="0d27b-105">Ověřování koncových uživatelů</span><span class="sxs-lookup"><span data-stu-id="0d27b-105">End-user authentication</span></span>](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

<span data-ttu-id="0d27b-106">Azure Data Lake Store využívá Azure Active Directory pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="0d27b-106">Azure Data Lake Store uses Azure Active Directory for authentication.</span></span> <span data-ttu-id="0d27b-107">Před vytváření aplikace, která funguje s Azure Data Lake Store nebo Azure Data Lake Analytics, je třeba nejprve rozhodnout, jak chcete ověřovat vaší aplikace pomocí Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0d27b-107">Before authoring an application that works with Azure Data Lake Store or Azure Data Lake Analytics, you must first decide how you would like to authenticate your application with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="0d27b-108">Jsou dvě hlavní možnosti k dispozici:</span><span class="sxs-lookup"><span data-stu-id="0d27b-108">The two main options available are:</span></span>

* <span data-ttu-id="0d27b-109">Ověření koncových uživatelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="0d27b-109">End-user authentication (this article)</span></span>
* <span data-ttu-id="0d27b-110">Ověřování služba-služba</span><span class="sxs-lookup"><span data-stu-id="0d27b-110">Service-to-service authentication</span></span>

<span data-ttu-id="0d27b-111">Obě tyto možnosti má za následek aplikace se součástí tokenu OAuth 2.0, který získá připojen k každou žádost odeslanou Azure Data Lake Store nebo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0d27b-111">Both these options result in your application being provided with an OAuth 2.0 token, which gets attached to each request made to Azure Data Lake Store or Azure Data Lake Analytics.</span></span>

<span data-ttu-id="0d27b-112">Tento článek rozhovory o vytvoření **nativní aplikaci Azure AD pro ověřování koncového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0d27b-112">This article talks about how create an **Azure AD native application for end-user authentication**.</span></span> <span data-ttu-id="0d27b-113">Pokyny v konfiguraci aplikace Azure AD pro ověřování service-to-service najdete v tématu [Service-to-service ověřování s Data Lake Store pomocí Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="0d27b-113">For instructions on Azure AD application configuration for service-to-service authentication see [Service-to-service authentication with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d27b-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0d27b-114">Prerequisites</span></span>
* <span data-ttu-id="0d27b-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="0d27b-115">An Azure subscription.</span></span> <span data-ttu-id="0d27b-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0d27b-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="0d27b-117">ID vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="0d27b-117">Your subscription ID.</span></span> <span data-ttu-id="0d27b-118">Můžete ji načíst z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0d27b-118">You can retrieve it from the Azure Portal.</span></span> <span data-ttu-id="0d27b-119">Například je k dispozici v okně účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="0d27b-119">For example, it is available from the Data Lake Store account blade.</span></span>
  
    ![Získání ID předplatného](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* <span data-ttu-id="0d27b-121">Název domény vaší služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d27b-121">Your Azure AD domain name.</span></span> <span data-ttu-id="0d27b-122">Můžete ji načíst podržením ukazatele myši v pravém horním rohu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0d27b-122">You can retrieve it by hovering the mouse in the top-right corner of the Azure Portal.</span></span> <span data-ttu-id="0d27b-123">Z na následující snímek obrazovky, je název domény **contoso.onmicrosoft.com**, a identifikátor GUID v závorkách je ID klienta.</span><span class="sxs-lookup"><span data-stu-id="0d27b-123">From the screenshot below, the domain name is **contoso.onmicrosoft.com**, and the GUID within brackets is the tenant ID.</span></span> 
  
    ![Získat doménu AAD](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a><span data-ttu-id="0d27b-125">Ověřování koncových uživatelů</span><span class="sxs-lookup"><span data-stu-id="0d27b-125">End-user authentication</span></span>
<span data-ttu-id="0d27b-126">Toto je doporučený postup, pokud chcete, aby koncovým uživatelem pro přihlášení do aplikace pomocí Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d27b-126">This is the recommended approach if you want an end-user to log in to your application via Azure AD.</span></span> <span data-ttu-id="0d27b-127">Vaše aplikace bude mít přístup k prostředkům Azure se stejnou úrovní přístupu jako koncový uživatel, přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0d27b-127">Your application will be able to access Azure resources with the same level of access as the end-user that logged in.</span></span> <span data-ttu-id="0d27b-128">Mohl koncový uživatel muset zadat přihlašovací údaje pravidelně v pořadí pro vaši aplikaci k získání přístupu.</span><span class="sxs-lookup"><span data-stu-id="0d27b-128">Your end-user will need to provide their credentials periodically in order for your application to maintain access.</span></span>

<span data-ttu-id="0d27b-129">Výsledek toho, že koncový uživatel přihlásit je, že aplikace je daná přístupový token a obnovovací token.</span><span class="sxs-lookup"><span data-stu-id="0d27b-129">The result of having the end-user log in is that your application is given an access token and a refresh token.</span></span> <span data-ttu-id="0d27b-130">Získá přístupový token připojena k každý požadavek na Data Lake Store nebo Data Lake Analytics, a je platná pro jednu hodinu, ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="0d27b-130">The access token gets attached to each request made to Data Lake Store or Data Lake Analytics, and it is valid for one hour by default.</span></span> <span data-ttu-id="0d27b-131">Token obnovení lze použít k získání nového tokenu přístupu, a je platná dobu až dvou týdnů ve výchozím nastavení, pokud se používá pravidelně.</span><span class="sxs-lookup"><span data-stu-id="0d27b-131">The refresh token can be used to obtain a new access token, and it is valid for up to two weeks by default, if used regularly.</span></span> <span data-ttu-id="0d27b-132">Můžete vytvořit dva různé přístupy pro koncového uživatele přihlásit.</span><span class="sxs-lookup"><span data-stu-id="0d27b-132">You can use two different approaches for end-user log in.</span></span>

### <a name="using-the-oauth-20-pop-up"></a><span data-ttu-id="0d27b-133">Pomocí automaticky otevírané okno OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="0d27b-133">Using the OAuth 2.0 pop-up</span></span>
<span data-ttu-id="0d27b-134">Aplikaci můžete spustit OAuth 2.0 autorizace automaticky otevírané okno, ve kterém může uživatel zadat své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0d27b-134">Your application can trigger an OAuth 2.0 authorization pop-up, in which the end-user can enter their credentials.</span></span> <span data-ttu-id="0d27b-135">Toto automaticky otevírané okno taky spolupracuje se službou Azure AD dvoufaktorové ověřování (2FA) proces, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="0d27b-135">This pop-up also works with the Azure AD Two-factor Authentication (2FA) process, if required.</span></span> 

> [!NOTE]
> <span data-ttu-id="0d27b-136">Tato metoda není dosud podporována v Azure AD Authentication Library (ADAL) pro Python nebo Java.</span><span class="sxs-lookup"><span data-stu-id="0d27b-136">This method is not yet supported in the Azure AD Authentication Library (ADAL) for Python or Java.</span></span>
> 
> 

### <a name="directly-passing-in-user-credentials"></a><span data-ttu-id="0d27b-137">Předávání přímo v uživatelských přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="0d27b-137">Directly passing in user credentials</span></span>
<span data-ttu-id="0d27b-138">Aplikace můžete přímo zadat přihlašovací údaje uživatele do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d27b-138">Your application can directly provide user credentials to Azure AD.</span></span> <span data-ttu-id="0d27b-139">Tato metoda funguje jenom s účty organizace ID uživatele; není kompatibilní s osobní / "live ID" uživatelské účty, včetně těch, které končí na @outlook.com nebo @live.com.</span><span class="sxs-lookup"><span data-stu-id="0d27b-139">This method only works with organizational ID user accounts; it is not compatible with personal / “live ID” user accounts, including those ending in @outlook.com or @live.com.</span></span> <span data-ttu-id="0d27b-140">Kromě toho tato metoda není kompatibilní s uživatelskými účty, které vyžadují Azure AD dvoufaktorové ověřování (2FA).</span><span class="sxs-lookup"><span data-stu-id="0d27b-140">Furthermore, this method is not compatible with user accounts that require Azure AD Two-factor Authentication (2FA).</span></span>

### <a name="what-do-i-need-to-use-this-approach"></a><span data-ttu-id="0d27b-141">Co je potřeba tuto metodu použijte?</span><span class="sxs-lookup"><span data-stu-id="0d27b-141">What do I need to use this approach?</span></span>
* <span data-ttu-id="0d27b-142">Název domény Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d27b-142">Azure AD domain name.</span></span> <span data-ttu-id="0d27b-143">To je již uveden v požadovaných součástí tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="0d27b-143">This is already listed in the prerequisite of this article.</span></span>
* <span data-ttu-id="0d27b-144">Azure AD **nativní aplikace**</span><span class="sxs-lookup"><span data-stu-id="0d27b-144">Azure AD **native application**</span></span>
* <span data-ttu-id="0d27b-145">ID aplikací pro nativní aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d27b-145">Application ID for the Azure AD native application</span></span>
* <span data-ttu-id="0d27b-146">Identifikátor URI přesměrování pro nativní aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d27b-146">Redirect URI for the Azure AD native application</span></span>
* <span data-ttu-id="0d27b-147">Nastavte delegovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="0d27b-147">Set delegated permissions</span></span>


## <a name="step-1-create-an-active-directory-native-application"></a><span data-ttu-id="0d27b-148">Krok 1: Vytvoření nativní aplikaci služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d27b-148">Step 1: Create an Active Directory native application</span></span>

<span data-ttu-id="0d27b-149">Vytvořit a nakonfigurovat nativní aplikaci Azure AD pro ověřování koncového uživatele s Azure Data Lake Store pomocí Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0d27b-149">Create and configure an Azure AD native application for end-user authentication with Azure Data Lake Store using Azure Active Directory.</span></span> <span data-ttu-id="0d27b-150">Pokyny najdete v tématu [vytvořit aplikaci Azure AD](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0d27b-150">For instructions, see [Create an Azure AD application](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

<span data-ttu-id="0d27b-151">Při těchto pokynů na výše uvedený odkaz, zkontrolujte, zda jste vybrali **nativní** pro typ aplikace, jak je vidět na tomto snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="0d27b-151">While following the instructions at the above link, make sure you select **Native** for application type, as shown in the screenshot below.</span></span>

<span data-ttu-id="0d27b-152">![Vytvořit webovou aplikaci](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "vytvořit nativní aplikace")</span><span class="sxs-lookup"><span data-stu-id="0d27b-152">![Create web app](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "Create native app")</span></span>

## <a name="step-2-get-application-id-and-redirect-uri"></a><span data-ttu-id="0d27b-153">Krok 2: Získání id aplikace a identifikátor URI pro přesměrování</span><span class="sxs-lookup"><span data-stu-id="0d27b-153">Step 2: Get application id and redirect URI</span></span>

<span data-ttu-id="0d27b-154">V tématu [získání ID aplikace](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) načíst id aplikace nativní aplikace Azure AD (také nazývané ID klienta na portálu Azure classic).</span><span class="sxs-lookup"><span data-stu-id="0d27b-154">See [Get the application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) to retrieve the application id (also called the client ID in the Azure classic portal) of the Azure AD native application.</span></span>

<span data-ttu-id="0d27b-155">Chcete-li získat identifikátor URI přesměrování, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="0d27b-155">To retrieve the redirect URI, follow the steps below.</span></span>

1. <span data-ttu-id="0d27b-156">Z portálu Azure vyberte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a najít a klikněte na nativní aplikaci Azure AD, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0d27b-156">From the Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click the Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="0d27b-157">Z **nastavení** pro aplikaci, klikněte na **identifikátory URI přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="0d27b-157">From the **Settings** blade for the application, click **Redirect URIs**.</span></span>

    ![Identifikátor URI pro přesměrování GET](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. <span data-ttu-id="0d27b-159">Zkopírujte zobrazené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0d27b-159">Copy the value displayed.</span></span>


## <a name="step-3-set-permissions"></a><span data-ttu-id="0d27b-160">Krok 3: Nastavte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="0d27b-160">Step 3: Set permissions</span></span>

1. <span data-ttu-id="0d27b-161">Z portálu Azure vyberte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a najít a klikněte na nativní aplikaci Azure AD, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0d27b-161">From the Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click the Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="0d27b-162">Z **nastavení** pro aplikaci, klikněte na **požadovaná oprávnění**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0d27b-162">From the **Settings** blade for the application, click **Required permissions**, and then click **Add**.</span></span>

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. <span data-ttu-id="0d27b-164">V **přidat přístup pomocí rozhraní API** okně klikněte na tlačítko **vybrat rozhraní API**, klikněte na tlačítko **Azure Data Lake**a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="0d27b-164">In the **Add API Access** blade, click **Select an API**, click **Azure Data Lake**, and then click **Select**.</span></span>

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  <span data-ttu-id="0d27b-166">V **přidat přístup pomocí rozhraní API** okně klikněte na tlačítko **vyberte oprávnění**, zaškrtněte políčko umožnit **úplný přístup do Data Lake Store**a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="0d27b-166">In the **Add API Access** blade, click **Select permissions**, select the check box to give **Full access to Data Lake Store**, and then click **Select**.</span></span>

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    <span data-ttu-id="0d27b-168">Klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="0d27b-168">Click **Done**.</span></span>

5. <span data-ttu-id="0d27b-169">Zopakujte poslední dva kroky k udělení oprávnění pro **Windows Azure Service Management API** také.</span><span class="sxs-lookup"><span data-stu-id="0d27b-169">Repeat the last two steps to grant permissions for **Windows Azure Service Management API** as well.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="0d27b-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d27b-170">Next steps</span></span>
<span data-ttu-id="0d27b-171">V tomto článku jste vytvořili nativní aplikaci Azure AD a shromáždit informace, které budete potřebovat v klientských aplikacích vytvářet pomocí .NET SDK, sady Java SDK, REST API atd. Nyní můžete přejít na následující články, které komunikovat o tom, jak používat webové aplikace Azure AD nejprve ověřit pomocí Data Lake Store a poté provést jiné operace v úložišti.</span><span class="sxs-lookup"><span data-stu-id="0d27b-171">In this article you created an Azure AD native application and gathered the information you need in your client applications that you author using .NET SDK, Java SDK, REST API, etc. You can now proceed to the following articles that talk about how to use the Azure AD web application to first authenticate with Data Lake Store and then perform other operations on the store.</span></span>

* [<span data-ttu-id="0d27b-172">Začínáme s Azure Data Lake Storem pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0d27b-172">Get started with Azure Data Lake Store using .NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
* [<span data-ttu-id="0d27b-173">Začínáme s Azure Data Lake Store pomocí sady Java SDK</span><span class="sxs-lookup"><span data-stu-id="0d27b-173">Get started with Azure Data Lake Store using Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
* [<span data-ttu-id="0d27b-174">Začínáme s Azure Data Lake Store pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="0d27b-174">Get started with Azure Data Lake Store using REST API</span></span>](data-lake-store-get-started-rest-api.md)

