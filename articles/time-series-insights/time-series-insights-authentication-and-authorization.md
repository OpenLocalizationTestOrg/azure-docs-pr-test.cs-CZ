---
title: "aaaConfigure ověřování a autorizace pro vlastní aplikaci, která volá rozhraní API pro Azure časové řady přehledy hello | Microsoft Docs"
description: "Tento kurz popisuje, jak tooconfigure ověřování a autorizace pro vlastní aplikaci, která volá rozhraní API pro Azure časové řady přehledy hello"
keywords: 
services: time-series-insights
documentationcenter: 
author: dmdenmsft
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/24/2017
ms.author: dmden
ms.openlocfilehash: 5043468bfc2af3c0d27e8602508d92ba2848409e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a><span data-ttu-id="9d834-103">Ověřování a autorizace pro rozhraní API pro Azure časové řady přehledy</span><span class="sxs-lookup"><span data-stu-id="9d834-103">Authentication and authorization for Azure Time Series Insights API</span></span>

<span data-ttu-id="9d834-104">Tento článek vysvětluje, jak hello tooconfigure vlastní aplikaci, která volá rozhraní API pro Azure časové řady přehledy.</span><span class="sxs-lookup"><span data-stu-id="9d834-104">This article explains how tooconfigure a custom application that calls hello Azure Time Series Insights API.</span></span>

## <a name="service-principal"></a><span data-ttu-id="9d834-105">Instanční objekt</span><span class="sxs-lookup"><span data-stu-id="9d834-105">Service principal</span></span>

<span data-ttu-id="9d834-106">Tato část vysvětluje, jak tooconfigure tooaccess aplikace hello rozhraní API pro přehledy časové řady jménem aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9d834-106">This section explains how tooconfigure an application tooaccess hello Time Series Insights API on behalf of hello application.</span></span> <span data-ttu-id="9d834-107">aplikace Hello následně zadávat dotazy na data nebo publikování referenční data v prostředí časové řady Statistika hello se přihlašovací údaje aplikací a hello pověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="9d834-107">hello application can then query data or publish reference data in hello Time Series Insights environment with application credentials and not hello user credentials.</span></span>

<span data-ttu-id="9d834-108">Když máte aplikaci, která potřebuje tooaccess časové řady statistiky, musíte nastavit aplikaci Azure Active Directory a přiřadit hello zásady přístupu k datům v prostředí časové řady Statistika hello.</span><span class="sxs-lookup"><span data-stu-id="9d834-108">When you have an application that needs tooaccess Time Series Insights, you must set up an Azure Active Directory application and assign hello data access policies in hello Time Series Insights environment.</span></span> <span data-ttu-id="9d834-109">Tento postup je vhodnější toorunning aplikace hello vlastní oprávnění, protože:</span><span class="sxs-lookup"><span data-stu-id="9d834-109">This approach is preferable toorunning hello app under your own credentials because:</span></span>

* <span data-ttu-id="9d834-110">Můžete přiřadit oprávnění toohello identita aplikace, která se liší od vlastní oprávnění.</span><span class="sxs-lookup"><span data-stu-id="9d834-110">You can assign permissions toohello app identity that are different from your own permissions.</span></span> <span data-ttu-id="9d834-111">Obvykle se tato oprávnění se s omezeným přístupem tooexactly jaké aplikace hello musí toodo.</span><span class="sxs-lookup"><span data-stu-id="9d834-111">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span> <span data-ttu-id="9d834-112">Například můžete povolit, že tooonly aplikace hello číst data v prostředí s konkrétní Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="9d834-112">For example, you can allow hello app tooonly read data in a particular Time Series Insights environment.</span></span>
* <span data-ttu-id="9d834-113">Pokud vaše odpovědnosti změnit nemáte aplikace hello toochange přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9d834-113">You don't have toochange hello app's credentials if your responsibilities change.</span></span>
* <span data-ttu-id="9d834-114">Pokud používáte bezobslužného skriptu, můžete použít certifikát nebo klíče tooautomate ověřování aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d834-114">You can use a certificate or an application key tooautomate authentication when you're running an unattended script.</span></span>

<span data-ttu-id="9d834-115">Tento článek ukazuje, jak hello tooperform ty kroky prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9d834-115">This article shows you how tooperform those steps through hello Azure portal.</span></span> <span data-ttu-id="9d834-116">Zaměřuje se na jednoho klienta aplikace, kde aplikace hello je určený toorun v pouze jedné organizaci.</span><span class="sxs-lookup"><span data-stu-id="9d834-116">It focuses on a single-tenant application where hello application is intended toorun in only one organization.</span></span> <span data-ttu-id="9d834-117">Jednoho klienta aplikace se obvykle používají pro-obchodní aplikace, které běží ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="9d834-117">You typically use single-tenant applications for line-of-business applications that run in your organization.</span></span>

<span data-ttu-id="9d834-118">tok Hello instalační program se skládá z tři hlavní kroky:</span><span class="sxs-lookup"><span data-stu-id="9d834-118">hello setup flow consists of three high-level steps:</span></span>

1. <span data-ttu-id="9d834-119">Vytvoření aplikace v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9d834-119">Create an application in Azure Active Directory.</span></span>
2. <span data-ttu-id="9d834-120">Autorizovat tuto aplikaci tooaccess hello časové řady Statistika prostředí.</span><span class="sxs-lookup"><span data-stu-id="9d834-120">Authorize this application tooaccess hello Time Series Insights environment.</span></span>
3. <span data-ttu-id="9d834-121">Pomocí ID aplikace hello a klíče tooacquire tokenu toohello `"https://api.timeseries.azure.com/"` cílové skupiny nebo prostředku.</span><span class="sxs-lookup"><span data-stu-id="9d834-121">Use hello application ID and key tooacquire a token toohello `"https://api.timeseries.azure.com/"` audience or resource.</span></span> <span data-ttu-id="9d834-122">Hello token pak lze použít toocall hello rozhraní API pro přehledy časové řady.</span><span class="sxs-lookup"><span data-stu-id="9d834-122">hello token can then be used toocall hello Time Series Insights API.</span></span>

<span data-ttu-id="9d834-123">Tady jsou hello podrobné kroky:</span><span class="sxs-lookup"><span data-stu-id="9d834-123">Here are hello detailed steps:</span></span>

1. <span data-ttu-id="9d834-124">V hello portálu Azure, vyberte **Azure Active Directory** > **registrace aplikace** > **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9d834-124">In hello Azure portal, select **Azure Active Directory** > **App registrations** > **New application registration**.</span></span>

   ![Nové registrace aplikace v Azure Active Directory](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. <span data-ttu-id="9d834-126">Poskytují aplikace hello toobe typu názvem, vyberte hello **webovou aplikaci nebo rozhraní API**, vyberte libovolný platný identifikátor URI pro **přihlašovací adresa URL**a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9d834-126">Give hello application a name, select hello type toobe **Web app / API**, select any valid URI for **Sign-on URL**, and click **Create**.</span></span>

   ![Vytvoření aplikace hello v Azure Active Directory](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. <span data-ttu-id="9d834-128">Vyberte nově vytvořené aplikace a zkopírujte jeho aplikace ID tooyour oblíbeném textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="9d834-128">Select your newly created application and copy its application ID tooyour favorite text editor.</span></span>

   ![Zkopírujte ID aplikace hello](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. <span data-ttu-id="9d834-130">Vyberte **klíče**, zadejte název klíče hello, vyberte hello vypršení platnosti a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9d834-130">Select **Keys**, enter hello key name, select hello expiration, and click **Save**.</span></span>

   ![Výběr aplikace klíče](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Zadejte název klíče hello a vypršení platnosti a klikněte na tlačítko Uložit](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. <span data-ttu-id="9d834-133">Zkopírujte hello klíče tooyour oblíbeném textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="9d834-133">Copy hello key tooyour favorite text editor.</span></span>

   ![Zkopírovat klíč aplikace hello](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. <span data-ttu-id="9d834-135">Hello časové řady Statistika prostředí, vyberte **zásady přístupu k datům** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9d834-135">For hello Time Series Insights environment, select **Data Access Policies** and click **Add**.</span></span>

   ![Přidat nové data přístup zásad toohello časové řady Přehled prostředí](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. <span data-ttu-id="9d834-137">V hello **vyberte uživatele** dialogové okno, vložte název aplikace hello (z kroku 2) nebo ID aplikace (z kroku 3).</span><span class="sxs-lookup"><span data-stu-id="9d834-137">In hello **Select User** dialog box, paste hello application name (from step 2) or application ID (from step 3).</span></span>

   ![Vyhledání aplikace v dialogovém okně vyberte uživatele hello](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. <span data-ttu-id="9d834-139">Vyberte hello role (**čtečky** pro dotazování na data, **Přispěvatel** pro dotazování dat a změna referenční data) a klikněte na tlačítko **Ok**.</span><span class="sxs-lookup"><span data-stu-id="9d834-139">Select hello role (**Reader** for querying data, **Contributor** for querying data and changing reference data) and click **Ok**.</span></span>

   ![Vyberte v dialogovém okně vybrat roli hello čtečky nebo přispěvatele](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. <span data-ttu-id="9d834-141">Uložit zásadu hello kliknutím **Ok**.</span><span class="sxs-lookup"><span data-stu-id="9d834-141">Save hello policy by clicking **Ok**.</span></span>

10. <span data-ttu-id="9d834-142">Pomocí ID aplikace hello (z kroku 3) a token hello klíče (z kroku 5) tooacquire aplikace jménem aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9d834-142">Use hello application ID (from step 3) and application key (from step 5) tooacquire hello token on behalf of hello application.</span></span> <span data-ttu-id="9d834-143">Hello token je pak možné předat v hello `Authorization` záhlaví při volání aplikace hello hello rozhraní API pro přehledy časové řady.</span><span class="sxs-lookup"><span data-stu-id="9d834-143">hello token can then be passed in hello `Authorization` header when hello application calls hello Time Series Insights API.</span></span>

    <span data-ttu-id="9d834-144">Pokud používáte C#, můžete použít následující kód tooacquire hello token jménem aplikace hello hello.</span><span class="sxs-lookup"><span data-stu-id="9d834-144">If you're using C#, you can use hello following code tooacquire hello token on behalf of hello application.</span></span> <span data-ttu-id="9d834-145">Kompletní příklad, najdete v části [dotaz na data pomocí jazyka C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="9d834-145">For a complete sample, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set hello resource URI toohello Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of hello application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

## <a name="next-steps"></a><span data-ttu-id="9d834-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d834-146">Next steps</span></span>

<span data-ttu-id="9d834-147">Pomocí ID aplikace hello a klíč ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9d834-147">Use hello application ID and key in your application.</span></span> <span data-ttu-id="9d834-148">Ukázkový kód, který volá hello rozhraní API pro přehledy časové řady, najdete v části [dotaz na data pomocí jazyka C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="9d834-148">For sample code that calls hello Time Series Insights API, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="9d834-149">Viz také</span><span class="sxs-lookup"><span data-stu-id="9d834-149">See also</span></span>

* <span data-ttu-id="9d834-150">[Dotaz na rozhraní API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) pro hello úplné dotazu API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="9d834-150">[Query API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) for hello full Query API reference</span></span>
* [<span data-ttu-id="9d834-151">Vytvoření služby hlavní v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9d834-151">Create a service principal in hello Azure portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
