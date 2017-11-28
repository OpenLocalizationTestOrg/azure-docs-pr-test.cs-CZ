---
title: "Ověřování koncového uživatele: Data Lake Store s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooachieve ověřování koncového uživatele s Data Lake Store pomocí Azure Active Directory"
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
ms.openlocfilehash: fd58f4f2d8fc915b8bc51d9e5b040d2cee34047e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a><span data-ttu-id="56c33-103">Ověření koncových uživatelů s Data Lake Store pomocí Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="56c33-103">End-user authentication with Data Lake Store using Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="56c33-104">Ověřování služba-služba</span><span class="sxs-lookup"><span data-stu-id="56c33-104">Service-to-service authentication</span></span>](data-lake-store-authenticate-using-active-directory.md)
> * [<span data-ttu-id="56c33-105">Ověřování koncových uživatelů</span><span class="sxs-lookup"><span data-stu-id="56c33-105">End-user authentication</span></span>](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

<span data-ttu-id="56c33-106">Azure Data Lake Store využívá Azure Active Directory pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="56c33-106">Azure Data Lake Store uses Azure Active Directory for authentication.</span></span> <span data-ttu-id="56c33-107">Před vytváření aplikace, která funguje s Azure Data Lake Store nebo Azure Data Lake Analytics, musíte nejprve rozhodnout, jak chcete tooauthenticate vaší aplikace pomocí Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="56c33-107">Before authoring an application that works with Azure Data Lake Store or Azure Data Lake Analytics, you must first decide how you would like tooauthenticate your application with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="56c33-108">Hello dvě hlavní možnosti k dispozici jsou:</span><span class="sxs-lookup"><span data-stu-id="56c33-108">hello two main options available are:</span></span>

* <span data-ttu-id="56c33-109">Ověření koncových uživatelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="56c33-109">End-user authentication (this article)</span></span>
* <span data-ttu-id="56c33-110">Ověřování služba-služba</span><span class="sxs-lookup"><span data-stu-id="56c33-110">Service-to-service authentication</span></span>

<span data-ttu-id="56c33-111">Obě tyto možnosti má za následek aplikace se součástí tokenu OAuth 2.0, který získá připojené tooeach požadavek tooAzure Data Lake Store nebo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="56c33-111">Both these options result in your application being provided with an OAuth 2.0 token, which gets attached tooeach request made tooAzure Data Lake Store or Azure Data Lake Analytics.</span></span>

<span data-ttu-id="56c33-112">Tento článek rozhovory o vytvoření **nativní aplikaci Azure AD pro ověřování koncového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="56c33-112">This article talks about how create an **Azure AD native application for end-user authentication**.</span></span> <span data-ttu-id="56c33-113">Pokyny v konfiguraci aplikace Azure AD pro ověřování service-to-service najdete v tématu [Service-to-service ověřování s Data Lake Store pomocí Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="56c33-113">For instructions on Azure AD application configuration for service-to-service authentication see [Service-to-service authentication with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56c33-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="56c33-114">Prerequisites</span></span>
* <span data-ttu-id="56c33-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="56c33-115">An Azure subscription.</span></span> <span data-ttu-id="56c33-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="56c33-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="56c33-117">ID vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="56c33-117">Your subscription ID.</span></span> <span data-ttu-id="56c33-118">Můžete ji načíst z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="56c33-118">You can retrieve it from hello Azure Portal.</span></span> <span data-ttu-id="56c33-119">Například je k dispozici v okně účtu Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="56c33-119">For example, it is available from hello Data Lake Store account blade.</span></span>
  
    ![Získání ID předplatného](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* <span data-ttu-id="56c33-121">Název domény vaší služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56c33-121">Your Azure AD domain name.</span></span> <span data-ttu-id="56c33-122">Můžete jej načíst po výběru ukázáním hello myši v pravém horním rohu hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="56c33-122">You can retrieve it by hovering hello mouse in hello top-right corner of hello Azure Portal.</span></span> <span data-ttu-id="56c33-123">Z hello snímku obrazovky je název domény hello **contoso.onmicrosoft.com**, a hello identifikátor GUID v závorkách je ID hello klienta.</span><span class="sxs-lookup"><span data-stu-id="56c33-123">From hello screenshot below, hello domain name is **contoso.onmicrosoft.com**, and hello GUID within brackets is hello tenant ID.</span></span> 
  
    ![Získat doménu AAD](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a><span data-ttu-id="56c33-125">Ověřování koncových uživatelů</span><span class="sxs-lookup"><span data-stu-id="56c33-125">End-user authentication</span></span>
<span data-ttu-id="56c33-126">Toto je hello doporučenému přístupu, pokud chcete toolog koncového uživatele v aplikaci tooyour přes Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56c33-126">This is hello recommended approach if you want an end-user toolog in tooyour application via Azure AD.</span></span> <span data-ttu-id="56c33-127">Vaše aplikace bude mít tooaccess prostředků Azure s hello stejnou úroveň přístupu jako hello koncového uživatele, který je přihlášen.</span><span class="sxs-lookup"><span data-stu-id="56c33-127">Your application will be able tooaccess Azure resources with hello same level of access as hello end-user that logged in.</span></span> <span data-ttu-id="56c33-128">Váš koncový uživatel potřebovat tooprovide přihlašovacích pravidelně v pořadí pro přístup k vaší aplikaci toomaintain.</span><span class="sxs-lookup"><span data-stu-id="56c33-128">Your end-user will need tooprovide their credentials periodically in order for your application toomaintain access.</span></span>

<span data-ttu-id="56c33-129">Hello výsledek toho, že koncový uživatel hello přihlášení je, že aplikace je daná přístupový token a obnovovací token.</span><span class="sxs-lookup"><span data-stu-id="56c33-129">hello result of having hello end-user log in is that your application is given an access token and a refresh token.</span></span> <span data-ttu-id="56c33-130">Získá přístupový token Hello připojené tooeach požadavek tooData Lake Store nebo Data Lake Analytics a je platná pro jednu hodinu, ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="56c33-130">hello access token gets attached tooeach request made tooData Lake Store or Data Lake Analytics, and it is valid for one hour by default.</span></span> <span data-ttu-id="56c33-131">token obnovení Hello lze použít tooobtain nový přístupový token ale platí pro až tootwo týdny ve výchozím nastavení, pokud se používá pravidelně.</span><span class="sxs-lookup"><span data-stu-id="56c33-131">hello refresh token can be used tooobtain a new access token, and it is valid for up tootwo weeks by default, if used regularly.</span></span> <span data-ttu-id="56c33-132">Můžete vytvořit dva různé přístupy pro koncového uživatele přihlásit.</span><span class="sxs-lookup"><span data-stu-id="56c33-132">You can use two different approaches for end-user log in.</span></span>

### <a name="using-hello-oauth-20-pop-up"></a><span data-ttu-id="56c33-133">Pomocí automaticky otevírané okno hello OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="56c33-133">Using hello OAuth 2.0 pop-up</span></span>
<span data-ttu-id="56c33-134">Aplikaci můžete spustit OAuth 2.0 autorizace automaticky otevírané okno, ve které hello koncového uživatele můžete zadat své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="56c33-134">Your application can trigger an OAuth 2.0 authorization pop-up, in which hello end-user can enter their credentials.</span></span> <span data-ttu-id="56c33-135">Toto automaticky otevírané okno funguje taky s procesem hello Azure AD dvoufaktorové ověřování (2FA), v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="56c33-135">This pop-up also works with hello Azure AD Two-factor Authentication (2FA) process, if required.</span></span> 

> [!NOTE]
> <span data-ttu-id="56c33-136">Tato metoda není dosud podporována v hello Azure AD Authentication Library (ADAL) pro Python nebo Java.</span><span class="sxs-lookup"><span data-stu-id="56c33-136">This method is not yet supported in hello Azure AD Authentication Library (ADAL) for Python or Java.</span></span>
> 
> 

### <a name="directly-passing-in-user-credentials"></a><span data-ttu-id="56c33-137">Předávání přímo v uživatelských přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="56c33-137">Directly passing in user credentials</span></span>
<span data-ttu-id="56c33-138">Aplikace můžete přímo zadat tooAzure přihlašovací údaje uživatele AD.</span><span class="sxs-lookup"><span data-stu-id="56c33-138">Your application can directly provide user credentials tooAzure AD.</span></span> <span data-ttu-id="56c33-139">Tato metoda funguje jenom s účty organizace ID uživatele; není kompatibilní s osobní / "live ID" uživatelské účty, včetně těch, které končí na @outlook.com nebo @live.com. Kromě toho tato metoda není kompatibilní s uživatelskými účty, které vyžadují Azure AD dvoufaktorové ověřování (2FA).</span><span class="sxs-lookup"><span data-stu-id="56c33-139">This method only works with organizational ID user accounts; it is not compatible with personal / “live ID” user accounts, including those ending in @outlook.com or @live.com. Furthermore, this method is not compatible with user accounts that require Azure AD Two-factor Authentication (2FA).</span></span>

### <a name="what-do-i-need-toouse-this-approach"></a><span data-ttu-id="56c33-140">Co dělat potřebuji toouse tento přístup?</span><span class="sxs-lookup"><span data-stu-id="56c33-140">What do I need toouse this approach?</span></span>
* <span data-ttu-id="56c33-141">Název domény Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56c33-141">Azure AD domain name.</span></span> <span data-ttu-id="56c33-142">To je již uveden v požadavku hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="56c33-142">This is already listed in hello prerequisite of this article.</span></span>
* <span data-ttu-id="56c33-143">Azure AD **nativní aplikace**</span><span class="sxs-lookup"><span data-stu-id="56c33-143">Azure AD **native application**</span></span>
* <span data-ttu-id="56c33-144">ID aplikací pro nativní aplikace hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="56c33-144">Application ID for hello Azure AD native application</span></span>
* <span data-ttu-id="56c33-145">Identifikátor URI přesměrování pro hello nativní aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="56c33-145">Redirect URI for hello Azure AD native application</span></span>
* <span data-ttu-id="56c33-146">Nastavte delegovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="56c33-146">Set delegated permissions</span></span>


## <a name="step-1-create-an-active-directory-native-application"></a><span data-ttu-id="56c33-147">Krok 1: Vytvoření nativní aplikaci služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="56c33-147">Step 1: Create an Active Directory native application</span></span>

<span data-ttu-id="56c33-148">Vytvořit a nakonfigurovat nativní aplikaci Azure AD pro ověřování koncového uživatele s Azure Data Lake Store pomocí Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="56c33-148">Create and configure an Azure AD native application for end-user authentication with Azure Data Lake Store using Azure Active Directory.</span></span> <span data-ttu-id="56c33-149">Pokyny najdete v tématu [vytvořit aplikaci Azure AD](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="56c33-149">For instructions, see [Create an Azure AD application](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

<span data-ttu-id="56c33-150">Při podle pokynů hello hello výše odkaz, zkontrolujte, zda jste vybrali **nativní** pro typ aplikace, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="56c33-150">While following hello instructions at hello above link, make sure you select **Native** for application type, as shown in hello screenshot below.</span></span>

<span data-ttu-id="56c33-151">![Vytvořit webovou aplikaci](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "vytvořit nativní aplikace")</span><span class="sxs-lookup"><span data-stu-id="56c33-151">![Create web app](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "Create native app")</span></span>

## <a name="step-2-get-application-id-and-redirect-uri"></a><span data-ttu-id="56c33-152">Krok 2: Získání id aplikace a identifikátor URI pro přesměrování</span><span class="sxs-lookup"><span data-stu-id="56c33-152">Step 2: Get application id and redirect URI</span></span>

<span data-ttu-id="56c33-153">V tématu [získání ID aplikace hello](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) tooretrieve hello id aplikace (také nazývané hello ID klienta v hello portál Azure classic) nativní aplikace hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56c33-153">See [Get hello application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) tooretrieve hello application id (also called hello client ID in hello Azure classic portal) of hello Azure AD native application.</span></span>

<span data-ttu-id="56c33-154">tooretrieve hello identifikátor URI pro přesměrování, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="56c33-154">tooretrieve hello redirect URI, follow hello steps below.</span></span>

1. <span data-ttu-id="56c33-155">Hello portálu Azure, vyberte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a najít a klikněte na tlačítko nativní aplikace hello Azure AD, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="56c33-155">From hello Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click hello Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="56c33-156">Z hello **nastavení** hello aplikaci, klikněte na **identifikátory URI přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="56c33-156">From hello **Settings** blade for hello application, click **Redirect URIs**.</span></span>

    ![Identifikátor URI pro přesměrování GET](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. <span data-ttu-id="56c33-158">Zkopírujte zobrazené hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="56c33-158">Copy hello value displayed.</span></span>


## <a name="step-3-set-permissions"></a><span data-ttu-id="56c33-159">Krok 3: Nastavte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="56c33-159">Step 3: Set permissions</span></span>

1. <span data-ttu-id="56c33-160">Hello portálu Azure, vyberte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a najít a klikněte na tlačítko nativní aplikace hello Azure AD, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="56c33-160">From hello Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click hello Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="56c33-161">Z hello **nastavení** hello aplikaci, klikněte na **požadovaná oprávnění**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="56c33-161">From hello **Settings** blade for hello application, click **Required permissions**, and then click **Add**.</span></span>

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. <span data-ttu-id="56c33-163">V hello **přidat přístup pomocí rozhraní API** okně klikněte na tlačítko **vybrat rozhraní API**, klikněte na tlačítko **Azure Data Lake**a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="56c33-163">In hello **Add API Access** blade, click **Select an API**, click **Azure Data Lake**, and then click **Select**.</span></span>

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  <span data-ttu-id="56c33-165">V hello **přidat přístup pomocí rozhraní API** okně klikněte na tlačítko **vyberte oprávnění**, vyberte hello políčko toogive **plný přístup k úložišti Lake tooData**a potom klikněte na **vyberte** .</span><span class="sxs-lookup"><span data-stu-id="56c33-165">In hello **Add API Access** blade, click **Select permissions**, select hello check box toogive **Full access tooData Lake Store**, and then click **Select**.</span></span>

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    <span data-ttu-id="56c33-167">Klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="56c33-167">Click **Done**.</span></span>

5. <span data-ttu-id="56c33-168">Hello zopakujte poslední dva kroky toogrant oprávnění pro **Windows Azure Service Management API** také.</span><span class="sxs-lookup"><span data-stu-id="56c33-168">Repeat hello last two steps toogrant permissions for **Windows Azure Service Management API** as well.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="56c33-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56c33-169">Next steps</span></span>
<span data-ttu-id="56c33-170">V tomto článku jste vytvořili nativní aplikaci Azure AD a shromáždit hello informace, které potřebujete ve vaší klientské aplikace vytvářet pomocí .NET SDK, sady Java SDK, REST API atd. Nyní můžete přejít toohello následující články, které mluvit o tom, jak ověřit toouse hello Azure AD webové aplikace toofirst s Data Lake Store a poté proveďte jiné operace v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="56c33-170">In this article you created an Azure AD native application and gathered hello information you need in your client applications that you author using .NET SDK, Java SDK, REST API, etc. You can now proceed toohello following articles that talk about how toouse hello Azure AD web application toofirst authenticate with Data Lake Store and then perform other operations on hello store.</span></span>

* [<span data-ttu-id="56c33-171">Začínáme s Azure Data Lake Storem pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="56c33-171">Get started with Azure Data Lake Store using .NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
* [<span data-ttu-id="56c33-172">Začínáme s Azure Data Lake Store pomocí sady Java SDK</span><span class="sxs-lookup"><span data-stu-id="56c33-172">Get started with Azure Data Lake Store using Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
* [<span data-ttu-id="56c33-173">Začínáme s Azure Data Lake Store pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="56c33-173">Get started with Azure Data Lake Store using REST API</span></span>](data-lake-store-get-started-rest-api.md)

